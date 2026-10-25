# How llm-d Manages the KV Cache and Routes Requests Based On It

llm-d does not own the KV cache, the attention keys and values a transformer computes while reading a prompt and keeps to avoid recomputing them. The model server, usually vLLM, stores it in fixed-size blocks named by chained content hashes, reuses matching blocks, and evicts the least recently used ones [[efficient-memory-management-for-large-language]] [[automatic-prefix-caching-vllm]]. Connectors move blocks to CPU memory, storage, peer pods or decode workers [[kv-cache-offloading-llm-d]]. llm-d's router, the Endpoint Picker (EPP), tracks where blocks live and routes each request accordingly [[architecture-llm-d]] [[llm-d-router-llm-d]].

The router tracks blocks either approximately, by remembering what it sent where, or precisely, by consuming `BlockStored`, `BlockRemoved` and `AllBlocksCleared` events that engines publish over ZeroMQ [[prefix-cache-aware-routing-llm-d-2]] [[kv-cache-indexer-llm-d]]. Both now feed the same sticky-until-saturated policy: stay on pods holding at least 80% of the prompt, pick the least loaded, and let go once warm pods fall a calibrated latency budget behind [[sticky-until-saturated-token-aware-routing-in-llm-d-llm-d]]. As of September 14, 2026, every shipped guide uses that policy, but only the precise-prefix-cache-routing guide feeds it from engine events [[optimized-baselinevaluesyaml]] [[precise-prefix-cache-routingvaluesyaml]].

The evidence supports three conclusions. Cache-aware routing clearly beats cache-blind round-robin on shared-prefix traffic. Event-driven indexing is the sounder signal, because only the engine knows what it evicted, yet its measured advantage over approximate routing ranges from +1.3% (calibrated sticky policy) to 57× (older approximate scheduler under heavy eviction), a spread reflecting both eviction pressure and policy era. And routing is only as good as its weakest input — event completeness, calibration, load freshness — each of which fails silently.

## 1. llm-d Architecture and Where the KV Cache Fits

llm-d is a Kubernetes-native project that runs many copies of a model server behind a cache-aware router.

### 1.1 Owner, mover, observer

Think of a library system: each branch (pod) owns its shelves and discards what it likes, vans (connectors) move books around, and the central catalog (router) owns no books, only the branches' reports.

| Layer | Implemented by | Role |
|---|---|---|
| Engine | vLLM or SGLang | Allocates, hashes, reuses, evicts blocks; publishes KV events [[automatic-prefix-caching-vllm]] [[vllmvllmdistributedkv_eventspy-at-main-vllm-projectvllm-github]] |
| Connectors | vLLM `OffloadingConnector`, llm-d filesystem backend, NIXL, LMCache | Move blocks to CPU, storage, peers, decode pods [[tiered-kv-cache-offloading-in-vllm-vllm-blog]] [[disaggregated-serving-llm-d]] |
| Router (proxy + EPP) | llm-d | Tracks block residency; filters, scores, picks pods [[kv-cache-management-llm-d]] |

llm-d's docs say connectors "integrate with llm-d through KV-Events: cache mutation notifications that the KV-Cache Indexer consumes to maintain a global view" [[kv-cache-offloading-llm-d]]. That makes llm-d portable across engines, but blind, without error, to any gap in event coverage.

### 1.2 The router and its plugin graph

The former Inference Scheduler is now the llm-d Router, and the indexer libraries from llm-d-kv-cache-manager (later llm-d-kv-cache) moved into it; its EPP code moved over from the Gateway API Inference Extension, which keeps the `InferencePool` API, a label-selected group of model-server pods [[github-llm-dllm-d-router-llm-d-router-the-intelligent-entry-point-for-inferenc-3]]. An Envoy proxy asks the EPP where each request goes, weighing "KV-cache locality, current load, and priority" [[llm-d-router-llm-d]]. Inside the EPP, data producers compute per-request facts such as prefix matches, filters narrow candidates, weighted scorers rank them, and a picker chooses. The configuration is read only at startup [[configuration-llm-d]] [[request-scheduler-llm-d]].

### 1.3 Two meanings of default

Cache-aware routing is two independent choices usually described as one: the **cache signal** and the **decision policy**. A third, the **tier**, decides where blocks can live. The shipped Helm values for each well-lit path (tested deployment recipe):

| Well-lit path | Cache signal | Policy (calibration) | Tier |
|---|---|---|---|
| optimized-baseline | approximate | `prefix-cache-affinity-filter` → `token-load-scorer` (15,928 tok/s default) [[optimized-baselinevaluesyaml]] | GPU |
| precise-prefix-cache-routing | precise, speculative indexing, replay port 5559 | same (15,926; 64-token blocks; one EPP replica) [[precise-prefix-cache-routingvaluesyaml]] | GPU |
| pd-disaggregation | approximate on prefill; **none** on decode | prefill: affinity filter → `token-load-scorer` (33,821); decode: `active-request-scorer` [[pd-disaggregationvaluesyaml]] | NIXL transfer |
| agentic-serving | approximate, LRU auto-tune off | same (16,444) [[agentic-servingvaluesyaml]] | GPU + CPU |
| p2p-kv-cache-sharing (experimental) | precise + `p2p-source-producer` | affinity or load-aware; `minCachedTokenDelta` 2,048 [[llm-dguidesp2p-kv-cache-sharing-at-main-llm-dllm-d-github]] | CPU + peer pull |

The default policy is sticky everywhere; the default signal is approximate everywhere except the precise guide.

### 1.4 Glossary

- **Prefill / decode**: reading the prompt in one compute-heavy pass, versus generating tokens one at a time.
- **Block**: a fixed number of tokens' KV cache (e.g., 16 or 64), hashed and evicted as a unit.
- **APC**: vLLM's automatic prefix caching, reusing blocks when a prompt starts with the same tokens.
- **TTFT**: time to first token.
- **KV event**: an engine message that a block was stored, removed, or all blocks cleared.
- **Affinity**: fraction of a prompt already cached on a pod.
- **τ (tau)**: in-flight token backlog at which stickiness is released.
- **NIXL**: point-to-point library moving KV blocks between pods over RDMA-class networks.
- **RDMA**: network transfer directly between machines' memory, bypassing the CPU.
- **LRU**: least-recently-used; evicts whatever was touched longest ago.
- **LoRA**: small fine-tuned adapters sharing one base model.
- **ZeroMQ (ZMQ)**: messaging library engines use to publish KV events.
- **TP**: tensor parallelism, one model split across several GPUs.
- **p50/p90/p99, QPS**: latency percentiles; queries per second.
- **Helm**: Kubernetes package manager whose values files configure each guide.

## 2. KV Cache Management Inside the Model Server

A transformer needs a stored key and value for every token it has read, at every layer, and recomputing them for long prompts is the expensive part of serving. Engines keep them in GPU memory and reuse them when a later prompt starts the same way. The router can only exploit reuse the engine actually performs.

### 2.1 Paged blocks named by their history

PagedAttention stores the cache in fixed-size, non-contiguous blocks, like operating-system pages, with "near-zero waste in KV cache memory" [[efficient-memory-management-for-large-language]]. APC names each full block `hash(parent_hash, block_tokens, extra_hashes)`, where extras cover LoRA IDs, multimodal input hashes and cache salts that isolate caches in multi-tenant environments [[automatic-prefix-caching-vllm]]. Because each hash folds in its parent, a block's name encodes its whole prefix, and a `cache_salt` in the first block keeps differently salted tenants apart.

A worked example: with 4-token blocks, a request whose first 10 tokens match an earlier one reuses 2 blocks (8 tokens); the half-matching third block is recomputed [[automatic-prefix-caching-vllm]]. At scale, resending a ~10,000-token prompt to Qwen3-32B cut TTFT from 4.3 s to 0.6 s [[kv-cache-wins-you-can-see-from-prefix-caching-in-vllm-to-distributed-scheduling]]. The default `sha256` hash "may not be reproducible across different Python or vLLM versions" [[automatic-prefix-caching-vllm]], one reason llm-d never reproduces engine hashes.

### 2.2 Eviction is private, so the cluster problem is placement

Freed blocks enter a free queue in reverse order, so a request's tail blocks, least likely to be shared, are evicted first [[automatic-prefix-caching-vllm]]. That decision is local; as NVIDIA's Dynamo team puts it, "eviction policies (LRU sweeps, memory pressure, preemption) are opaque — there's no way to infer cache state from request-response cycles alone" [[flash-indexer-a-story-of-inter-galactic-kv-routing-nvidia-dynamo-documentation]]. Dynamo, like llm-d, still offers an event-free approximate mode (`--no-router-kv-events`) as a fallback [[router-design-nvidia-dynamo-documentation]]. Each replica's cache is an island: on 8×A10G with 150 repeated prefixes, round-robin reached only ~11% hit rate [[how-llm-d-prefix-cache-routing-made-qwen-7b-on-eks-23x-faster-dev-community]]. The converse holds too: where a runtime disables prefix caching, llm-d's NPU overlay drops the affinity filter, which "would pin requests to endpoints it believes are cache-warm while the model servers reuse nothing" [[llm-dguidesrecipesroutercalibration-at-main-llm-dllm-d-github]].

## 3. Cluster-Wide KV Cache State: Events and the KV-Cache Indexer

For every request the router needs one answer: how many leading blocks of this prompt does each pod hold? It can infer the answer from its own routing history or be told by the engines.

### 3.1 Two ways to know

| | Approximate | Precise |
|---|---|---|
| Tokens | IDs from a token producer; tokenizer-free estimate by default (character-based `blockSize` deprecated) | exact IDs from vLLM's render endpoint |
| State | LRU of prefixes "recently sent to which Pods" | engine KV events |
| Known failure | "can diverge from actual model server state (e.g., if a Pod evicts a prefix due to memory pressure)" | needs render endpoint, ZMQ, engine event support |

Sources: [[prefix-cache-aware-routing-llm-d-2]] [[request-scheduler-llm-d]] [[llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducerapproximateprefixrea]].

The approximate producer is **a model of the engine**: it hashes token IDs into chained blocks (64-token minimum), checks per-pod LRUs (31,250 entries by default, auto-tuned from endpoint metrics) for pods that recently served a matching prefix, and credits the pod it picks [[llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducerapproximateprefixrea]]. The precise producer is **a subscription**: it renders the chat template and tokenizes once per request, so its block keys cover the same tokens the engine cached [[kv-cache-indexer-llm-d]]. The approximate LRU also has a sizing trap: auto-tuning counts only GPU blocks, so the agentic guide disables it once CPU offload enlarges the cache [[agentic-servingvaluesyaml]].

### 3.2 From events to an index

vLLM's `BlockStored` carries block and parent hashes, token IDs, `medium` (GPU, CPU, STORAGE) and `extra_keys` (multimodal identifiers, LoRA name and cache salt, exposed so external KV cache consumers can reconstruct block hashes), plus `locality` and `ownership` since September 4, 2026. The publisher numbers messages and keeps a 10,000-batch replay buffer [[vllmvllmdistributedkv_eventspy-at-main-vllm-projectvllm-github]]. SGLang and TensorRT-LLM publish the same event types [[sglangpythonsglangsrtmem_cacheeventspy-at-main-sgl-projectsglang-github]] [[precise-prefix-cache-routing-llm-d]]. llm-d-router v0.10.0's indexer then:

1. **Subscribes** to every pod via pod discovery; each EPP replica converges on the same index [[precise-prefix-cache-routing-llm-d]].
2. **Shards** by pod ID, which "guarantees events from the same pod are processed in order" [[llm-d-kv-cachedocsarchitecturemd-at-main-llm-dllm-d-kv-cache-github]].
3. **Decodes** with `VLLMAdapter` or `SGLangAdapter`, the only two [[engineadapter-package-githubcomllm-dllm-d-routerpkgkveventsengineadapter-go-pack]].
4. **Bridges keys**: it computes its own request key from each event's tokens, a chained FNV-64a hash over parent key, token chunk and extras seeded by `hashSeed` plus model name [[kvblock-package-githubcomllm-dllm-d-routerpkgkvcachekvblock-go-packages]], and stores request key → {pod, tier} plus engine key → request key, so token-less `BlockRemoved` and CPU-offload events resolve through the mapping [[llm-d-kv-cachedocsarchitecturemd-at-main-llm-dllm-d-kv-cache-github]].
5. **Deduplicates**, forwarding a removal "only once no remaining announcement references the block" [[kvevents-package-githubcomllm-dllm-d-routerpkgkvevents-go-packages]].

The key bridge keeps the design engine-agnostic: vLLM, SGLang and TensorRT-LLM hash blocks differently, and the router must match their block size and tokens, not their hash. Storing hashes rather than tensors is cheap: about 339 KB of metadata for a 365 GB cache pool [[kv-cache-wins-you-can-see-from-prefix-caching-in-vllm-to-distributed-scheduling]]. The default store is an in-memory two-level LRU (block hash → pods; 100M keys × 10 pods), so the index can itself forget blocks. A byte-budgeted Ristretto cache suits variable-size multimodal or LoRA entries, and Redis/Valkey shares state at the cost of a network hop per lookup [[kv-cache-indexer-llm-d]].

### 3.3 The read path

Each pod scores its "longest run of consecutive block hits starting from block 0, weighted per device tier" [[kvcache-package-githubcomllm-dllm-d-routerpkgkvcache-go-packages]]. Attention is causal, so a pod holding blocks 0, 1 and 3 scores 2. Tier weights default to GPU 1.0 and CPU 0.8, normalized to [0, 1]; speculative entries (2 s TTL) cover the gap between routing and the engine's confirmation [[kv-cache-indexer-llm-d]].

### 3.4 What must match

| Requirement | If wrong |
|---|---|
| Router block size = engine `--block-size` | zero matches [[llm-dguidesprecise-prefix-cache-routingreadmemd-at-main-llm-dllm-d-github]] |
| Tokenizer model = served model | mismatch is rejected rather than silently producing token IDs from the wrong tokenizer [[precise-prefix-cache-routingvaluesyaml]] |
| Event port = router's endpoint port | "a mismatched port leaves the index empty" [[llm-dguidesp2p-kv-cache-sharing-at-main-llm-dllm-d-github]] |
| Render Service sized to load | "prefix scoring silently disabled" [[llm-dguidesp2p-kv-cache-sharing-at-main-llm-dllm-d-github]] |

The index is HA-safe, since each replica subscribes and converges independently, and v0.9 added leader-based router HA [[llm-d-v09-hardened-for-scale-llm-d]]. Yet current precise values pin one replica: `token-load-scorer` counts in-flight tokens per EPP process, so two replicas would each see half the load and misfire the gate [[precise-prefix-cache-routing-llm-d]] [[llm-dguidesprecise-prefix-cache-routingreadmemd-at-main-llm-dllm-d-github]].

### 3.5 Where the precise signal is exact, and where it goes stale

The docs call the precise view "100% accurate" [[prefix-cache-aware-routing-llm-d-2]]. The code supports a narrower claim.

| Condition | Status (September 14, 2026) |
|---|---|
| Router restart empties the index; orphaned child blocks were dropped [[recover-kvblockindex-from-a-disruptive-event-issue-372-llm-dllm-d-kv-cache]] | **Fixed**: PR #1887 (August 6) replays on connect and on sequence gaps [[feat-replay-buffer-consumer-for-kv-event-index-recovery-by-rishabhsaini-pull-r-2]], after vLLM PR #45177 (July 7) [[fix-include-topic-frame-in-kv-events-replay-response-by-rishabhsaini-pull-reques]] |
| Ungraceful pod loss removes the subscriber but never purges index entries [[pod_reconcilergo]] | **Open**: #544's `Index.Clear()` unimplemented, marked stale September 4 [[dynamic-speculative-ttl-for-unified-prefix-cache-scoring-issue-544-llm-dllm-d-kv]] |
| SGLang adapter parses LoRA and `ExtraKeys` but not `BlockStoredWithMetadata`, which carries `cache_salt` [[sglang_adaptergo]] | **Open**: salted SGLang requests get no precise affinity |
| v0.10.0 (August 17, latest) flattens tiers and ignores `locality`/`ownership` [[kvevents-package-githubcomllm-dllm-d-routerpkgkvevents-go-packages]] | **Open** |
| Non-atomic lookup could count empty entries as hits [[kvblock-inmemoryindexlookup-tests-emptiness-and-reads-entries-without-the-pod-ca]] | **Fixed** September 9 (PR #2680); telemetry only |

Replay needed vLLM #45177 to attach the pod-identifying topic frame to replayed messages. #1887 tracks separate indexed and arrived sequence watermarks so replayed copies do not inflate dedup counts, and treats a same-address sequence reset as a restart; a pod that vanishes or returns at a new IP gives no such signal [[feat-replay-buffer-consumer-for-kv-event-index-recovery-by-rishabhsaini-pull-r-2]].

For vLLM, the precise producer reproduces salt-, LoRA- and multimodal-aware keys [[llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducerpreciseprefixcachere]] [[kv-cache-indexer-llm-d]]. The precise signal is therefore exact for a healthy vLLM pod's GPU block lifecycle, and across router restarts since August 2026 — a large advantage over a model that never hears of evictions, but not fleet-wide truth.

## 4. Tiered KV Cache Offloading and Disaggregated KV Transfer

GPU memory fills fast: Llama-3.1-70B needs about 305 GB of KV cache per million tokens [[native-kv-cache-offloading-to-any-filesystem-with-llm-d-llm-d]]. llm-d enlarges each pod's island three ways: offloading spills blocks to CPU or storage, peer-to-peer (P2P) pulls fetch blocks another pod holds, and prefill/decode (P/D) disaggregation ships a cache computed on one pod to another.

### 4.1 CPU and storage tiers

vLLM's `OffloadingConnector` supports a CPU tier alone or with filesystem, object or P2P tiers, and "Only the CPU primary tier has direct GPU access" [[kv-offloading-usage-guide-vllm]]. llm-d's filesystem backend became that connector's filesystem tier in llm-d v0.8 / vLLM v0.23 [[github-llm-dllm-d-kv-cache-distributed-kv-cache-scheduling-offloading-libraries]]. It "does not handle cleanup or eviction", which falls to a separate PVC Evictor [[kv-cache-offloading-llm-d]] [[github-llm-dllm-d-kv-cache-distributed-kv-cache-scheduling-offloading-libraries]]. Loading from storage beat recomputation by up to 16.8× on long Llama-3.1-70B prompts [[native-kv-cache-offloading-to-any-filesystem-with-llm-d-llm-d]].

Offload pays once the working set outgrows GPU memory: −25.6% mean TTFT with 100 GB CPU offload in v0.4 [[llm-d-04-achieve-sota-performance-across-accelerators-llm-d]], and 13.9× throughput at 250 users where GPU-only serving collapsed in v0.5 [[llm-d-05-sustaining-performance-at-scale-llm-d]]. Without pressure it only adds work; one 64-GPU GLM-5.2 run wrote ~5 TB with zero restores [[serving-glm-52-for-agentic-workloads-on-llm-d-llm-d]].

Visibility varies. Native CPU offload events carry a CPU tier, which the scorer weights 0.8 [[llm-d-kv-cachedocsarchitecturemd-at-main-llm-dllm-d-kv-cache-github]]; filesystem and object tiers emit only opt-in, hash-only events [[kv-offloading-usage-guide-vllm]]. A vLLM contributor states the governing rule: "whoever owns eviction should also provide invalidation. Otherwise, routers may score or route based on stale placement state" [[rfc-multi-tier-kv-offloading-via-the-vllm-offloading-connector-issue-38260-vllm]].

Out-of-tree connectors (LMCache, Mooncake, NVIDIA KVBM) evict outside vLLM, so their visibility depends entirely on publishing events [[kv-cache-offloading-llm-d]]. Mooncake has a publisher, enabled in release wheels in September 2026 [[cibuild-enable-kv-events-in-release-wheels-by-ishandhanani-pull-request-4039-kvc]]. LMCache falls short: its in-process connector drops CPU events [[lmcache-vllm-kv-events-missing-cpu-blockstored-event-only-gpu-event-emitted-issu]], its multi-process mode emits none in any release [[support-kv-cache-events-in-mp-mode-for-dynamo-kv-aware-routing-issue-3661-lmcach]], and the fix (PR #5076, opened September 11, 2026) is unmerged, store-only and validated against Dynamo, not llm-d [[featvllm-publish-mp-connector-kv-events-by-dongdongju-pull-request-5076-lmcachel]].

### 4.2 Peer-to-peer pulls

The `p2p-source-producer` picks a CPU-tier holder "within one index block of the largest cached prefix", favoring shorter queues, and requests a pull only if it leads the chosen pod by `minCachedTokenDelta` tokens [[llm-dguidesp2p-kv-cache-sharing-at-main-llm-dllm-d-github]]. That delta is a measured crossover: on gpt-oss-120b over H200/RDMA pulling won at every length (−88% near 48K tokens). A 753B GLM rig broke even only near 8.7K, because "Break-even depends on the ratio of prefill cost to KV-transfer cost, not model size alone" [[pull-dont-recompute-peer-to-peer-kv-cache-sharing-in-llm-d-llm-d]]. Peers must share block size and hash seed, or P2P silently yields "zero matching transfers" [[pull-dont-recompute-peer-to-peer-kv-cache-sharing-in-llm-d-llm-d]]. Benefit depends on policy: with affinity routing the gain "is within run-to-run spread"; with load-aware placement the pull adds +143% sustained rate on a uniform pool [[llm-dguidesp2p-kv-cache-sharing-at-main-llm-dllm-d-github]].

### 4.3 Prefill/decode disaggregation

In P/D mode the EPP picks a decode pod and names a prefill pod in a header [[request-scheduler-llm-d]]. A decode-side sidecar runs prefill with `max_tokens=1`, and the decoder pulls the resulting blocks over NIXL [[disaggregated-serving-llm-d]]. Only prefill uses a cache signal, calibrated through the full P/D path [[pd-disaggregationvaluesyaml]]. Disaggregation pays most for prefill-heavy traffic and large models, where separating the phases improves TTFT and throughput together [[arxiv250605508v1-csdc-5-jun-2025]] [[introducing-disaggregated-inference-on-aws-powered-by-llm-d-artificial-intellige]]. Rules are strict: decode tensor parallelism must be at least prefill's, the EPP assumes all P and D instances within an InferencePool are compatible, `kv_load_failure_policy=fail` should stay because `recompute` hides failures as latency, and a stale NIXL agent after a prefill restart can break decode for up to an hour, with fixes not yet in a published image [[llm-ddocsoperationsdisaggregationvllmmd-at-main-llm-dllm-d-github]]. Two multi-tier bugs were fixed: concurrent leg dispatch that made decoders wait a mean 15.83 s (PR #2451, September 5, 2026) [[sidecar-pd-multi-tier-decode-leg-is-dispatched-concurrently-with-prefill-chargin]], and warm producers supplying nothing (vLLM v0.29.0) [[bug-pd-multi-tier-supplies-nothing-when-the-producer-already-holds-the-blocks-is]].

Working P/D composes with P2P: letting prefill pull decode-generated history cut median TTFT 6.3× (6.83 s to 1.09 s) on a 2-prefill, 4-decode Qwen3-30B-A3B rig on H200 [[pull-dont-recompute-peer-to-peer-kv-cache-sharing-in-llm-d-llm-d]]. Broken P/D is silent: on one GPU without RDMA, NIXL never initialized and p50 latency rose 6.5×, while the EPP still reported an 81.3% hit rate [[pd-disaggregation-on-a-single-gpu-what-the-architecture-actually-requires-karthi]].

## 5. Cache-Aware Routing in the Inference Scheduler

Always choosing the pod with the longest cached prefix piles popular prefixes onto a few pods; ignoring the cache wastes saved prefill. llm-d has shipped three balances: a weighted blend, a filter with a calibrated release valve, and a learned latency predictor.

### 5.1 The pipeline and the legacy blend

A profile runs filters, then weighted scorers (each 0–1, weighted and summed), then a picker; if no producer is named, the prefix scorer falls back to the auto-instantiated `approx-prefix-cache-producer` [[request-scheduler-llm-d]], so a mis-referenced precise producer silently becomes approximate. Before v0.9, routing summed prefix-cache (weight 3), queue (2), KV utilization (2) and `no-hit-lru-scorer` (2) [[sticky-until-saturated-token-aware-routing-in-llm-d-llm-d]]. Utilization is `1 − kvCacheUsagePercent` [[kvcacheutilization-package-githubcomllm-dllm-d-routerpkgeppframeworkpluginssched]], and the LRU scorer (v0.4) spread cache-miss requests, cutting P99 TTFT from 18.3 s to 8 s [[llm-d-04-achieve-sota-performance-across-accelerators-llm-d]]. Schema docs still call this blend "recommended for a typical production setup" [[configuration-llm-d]]. Its queue and KV-utilization inputs are Prometheus gauges each engine must expose and the EPP polls [[gateway-api-inference-extensiondocsproposals003-model-server-protocolreadmemd-at]]; `token-load-scorer`, below, instead counts in-flight tokens inside the EPP.

### 5.2 Sticky until saturated

v0.9 made this "the default strategy across all guides" [[llm-d-v09-hardened-for-scale-llm-d]], replacing a blend that was hard to tune and prone to "the hot spotting most routers suffer from over-indexing on KV-cache affinity" [[sticky-until-saturated-token-aware-routing-in-llm-d-llm-d]].

- **Filter.** Pods with affinity ≥ 0.80 are candidates; "If no endpoint meets the threshold, all endpoints pass through."
- **Load scorer.** `token-load-scorer` ranks by `current_uncached_in_flight(endpoint) + prompt_tokens × (1 − hit_rate(endpoint))`: queued uncached work plus what this request adds.
- **Release valve.** Estimated TTFT is in-flight tokens ÷ `peakPrefillThroughput`, and stickiness breaks when the best warm endpoint's estimated TTFT exceeds the best non-warm endpoint's by more than `maxTTFTPenaltyMs`. [[sticky-until-saturated-token-aware-routing-in-llm-d-llm-d]]

Defaults are 0.80, 18,000 ms and 15,928 tok/s [[prefixcacheaffinity-package-githubcomllm-dllm-d-routerpkgeppframeworkpluginssche]], equivalent to a token gap τ = R_peak × T_max ≈ 15,928 × 18 ≈ 286,700 tokens, about 35 full 8,192-token chunks. A warm pod wins until its backlog would cost ~18 s more than a cold pod's [[sticky-until-saturated-token-aware-routing-in-llm-d-llm-d]].

The default was measured on Qwen3-32B/H100/TP=2, and measured values span ~20×, from 1,970 tok/s on CPU to 39,065 for gpt-oss-120B on H100 [[llm-dguidesrecipesroutercalibrationconfiguration-matrixmd-at-main-llm-dllm-d-git]]. `calibrate.sh` measures it with always-missing random-token prompts [[llm-dguidesrecipesroutercalibration-at-main-llm-dllm-d-github]]. Transplanted constants misfire: gpt-oss-120B's valve lands ~1.9× too low, multimodal serving needed a 0.6 threshold and 36 s budget, and on one stress workload small τ changes flipped the cluster between stable prefix ownership and eviction churn — "There's no smooth in-between" [[sticky-until-saturated-token-aware-routing-in-llm-d-llm-d]].

A filter beats a scorer because a prefix scorer with `max-score-picker` "causes hot-spotting" while `weighted-random-picker` "dilutes cache affinity" [[prefixcacheaffinity-package-githubcomllm-dllm-d-routerpkgeppframeworkpluginssche]]. The same godoc notes that if no endpoint carries `PrefixCacheMatchInfo`, every prefix score defaults to 0 and all endpoints are kept, so a missing producer silently means plain load balancing.

Nor is the policy strictly better. On b2b-saas, built so cacheable content (~900k tokens) exceeds per-pod KV capacity, both policies held a stable prefix partition at ~90–95% hit rate. Above 50 QPS the new policy fell to 70–73% because the valve fired and moved traffic off saturated warm pods, while the valveless blend held 96–97% and the lowest TTFT-p90. The authors concede "a carefully tuned weight set will outperform a general configuration on the specific workload it was tuned for"; the new policy's case is one calibration measurement instead of four tuned weights [[sticky-until-saturated-token-aware-routing-in-llm-d-llm-d]].

### 5.3 Worked example through the precise guide

A hypothetical request carries a 6,000-token shared system prompt plus a 400-token question.

1. **Look up.** Exact tokens give 100 blocks of 64. Pods A and B hold the first 93 on GPU (affinity ≈ 0.93); C holds none; D holds 41 on CPU, worth ≈ 0.33 (0.8 CPU weight) [[kv-cache-indexer-llm-d]].
2. **Filter.** Only A and B pass.
3. **Score.** A has 250,000 uncached tokens in flight, B 40,000; each adds ≈ 448, so B wins.
4. **Gate.** B's estimated TTFT ≈ 40,448 / 15,926 ≈ 2.5 s; cold C ≈ 1.0 s. The 1.5 s gap is under 18 s, so B keeps the request. Had A and B each carried ~320,000 tokens, the ~19 s gap would readmit C and D, and C would win [[sticky-until-saturated-token-aware-routing-in-llm-d-llm-d]].
5. **Execute and confirm.** B gets 2 s speculative entries [[kv-cache-indexer-llm-d]]; vLLM on B reuses 93 blocks, prefills 448 tokens, and publishes `BlockStored` for new blocks [[llm-d-kv-cachedocsarchitecturemd-at-main-llm-dllm-d-kv-cache-github]].
6. **Evict.** Later `BlockRemoved` deletes B's evicted tail entries; if offloaded first, a CPU event keeps B listed at 0.8.

The approximate producer cannot do step 6; its LRU keeps crediting B until its own capacity pushes the entry out [[prefix-cache-aware-routing-llm-d-2]].

### 5.4 Predicted-latency routing (opt-in)

This policy replaces the hand-set valve with an online XGBoost model predicting each pod's TTFT from queue depth, KV utilization, prefix match and in-flight tokens, keeping the 0.80 gate, adding 1% exploration, and breaking affinity when predicted TTFT is 5,000 ms worse [[predicted-latency-based-scheduling-for-llms-llm-d]]. On a production-derived Qwen3-480B workload it cut p50 TTFT 70% (0.97 s vs 3.38 s) against a tuned 3/2/2 blend, but was only comparable in an eviction-driven scenario [[predicted-latency-based-scheduling-for-llms-llm-d]]. Against sticky-until-saturated (single runs, older filter parameters), it stayed within ~10 s of its TTFT on code generation, matched the best decode-bound configuration at saturation, and could not reach b2b-saas's operating range. The authors still call it "the safer default" for mixed workloads whose bottleneck shifts [[sticky-until-saturated-token-aware-routing-in-llm-d-llm-d]].

## 6. Trade-offs, Measured Impact, and Limitations

A routing benchmark means little without its baseline: beating round-robin on repeated-prefix traffic says nothing about how cache-aware designs compare.

### 6.1 What the numbers compare

| Result | Setup | Baseline |
|---|---|---|
| Run 840.2 → 358.7 s; hit ~11% → ~93% | 8×A10G, Qwen2.5-7B, 150 prefixes, concurrency 512 [[how-llm-d-prefix-cache-routing-made-qwen-7b-on-eks-23x-faster-dev-community]] | round-robin |
| P95 TTFT 727 → 237 ms | 4×L4, multi-turn chat [[accelerate-multi-turn-llm-workloads-on-openshift-ai-with-llm-d-intelligent-routi]] | round-robin |
| 3× output tok/s, 2× lower TTFT | Tesla production, Llama 3.1 70B, 4 AMD MI300X [[production-grade-llm-inference-at-scale-with-kserve-llm-d-and-vllm-llm-d]] | prefix routing off |
| 2–3× throughput, prefill-bound | Qwen3-32B/H100, single runs [[sticky-until-saturated-token-aware-routing-in-llm-d-llm-d]] | round-robin |
| TTFT p90 0.542 vs 31.083 s (57×) | 16×H100, prefix demand ≈ 73% of cluster KV, then-default load-aware scheduler [[kv-cache-wins-you-can-see-from-prefix-caching-in-vllm-to-distributed-scheduling]] | **approximate** llm-d |
| Precise ≈+1.3% successful throughput (median TTFT +7.7%); P2P adds 4.6% on approximate, 9.7% on precise; +11.1% combined | GLM-5.2-FP8, 32×H200, 300 s window [[pull-dont-recompute-peer-to-peer-kv-cache-sharing-in-llm-d-llm-d]] | calibrated approximate |

The big multipliers are against cache-blind routing, often saturated. The 57× is precise versus approximate, not versus round-robin as some third-party posts claim [[kv-cache-routing-why-standard-load-balancers-break-prefix-caching-and-how-to-fix]]. The 57× beat the then-default approximate scheduler on b2b-saas, where a record of what the router sent can point at pods that evicted the prefix (vLLM wait queue 8.1 versus 0.1) [[kv-cache-wins-you-can-see-from-prefix-caching-in-vllm-to-distributed-scheduling]]. But that workload is bimodal, settling into a stable partition or eviction churn on small tuning differences, so the 57× may also reflect which state that configuration reached [[sticky-until-saturated-token-aware-routing-in-llm-d-llm-d]].

In the calibrated GLM run, precise indexing mainly helped by roughly doubling P2P's lift, plausibly because exact residency improves source selection. The current policy has been run on b2b-saas and held its partition (§5.2); no benchmark yet compares precise with approximate signals under it at heavy eviction.

### 6.2 When affinity hurts

The case against affinity comes from skewed workloads. CacheRoute's ablation shows affinity alone raising load imbalance to 3.46× with no capacity gain, while affinity plus balanced placement cut it to 1.24× and a 70B conversational workload gained 2.3× over the best baseline. Yet where "affinity moves KV hit only from 1.1% to 11.8%," capacity "falls to 0.50–0.67× Flat-LB" [[cacheroute-planned-prefix-affinity-routingfor-large-scale-llm-serving]]. DualMap finds skew overloading "nodes responsible for hot prefixes" [[published-as-a-conference-paper-at-iclr-2026]]. llm-d's own document-Q&A test hit 25.2 s p99 TTFT because "document owners become queueing hotspots" [[pull-dont-recompute-peer-to-peer-kv-cache-sharing-in-llm-d-llm-d]].

The case for llm-d is structural but untested against CacheRoute. Its filter plus load scorer resembles affinity-plus-placement, and it admits per request rather than per key. CacheRoute pins a key by aggregate rate, whereas llm-d checks each request's own match, so a 250-token shared prompt before a 1,000-token question (0.25 affinity) passes every pod to the load scorer [[sticky-until-saturated-token-aware-routing-in-llm-d-llm-d]]. In high-reuse skew the gate holds and only the valve protects the tail, and the document-Q&A p99 shows it can fire late. Load-aware placement plus P2P, still experimental, cut that p99 to 16.6 s with 35% more throughput [[pull-dont-recompute-peer-to-peer-kv-cache-sharing-in-llm-d-llm-d]].

Two first-party incidents expose the load signal as the weak point, one per signal source:

- **#298 (August 2025, llm-d 0.2.0, affinity disabled, polled metrics):** bursts piled onto one pod "until 50ms (the metric refresh interval) later", giving 2,083 ms mean TTFT versus Knative's 1,413 ms; the weighted-random fix cut multi-turn throughput from 12,608 to 8,292 tok/s until reweighted [[poor-performance-with-bursty-workloads-issue-298-llm-dllm-d-router]].
- **#2295 (August 2026, precise producer plus affinity, in-router counts):** a 5-minute reaper erased in-flight accounting, and "a reaped hot pod looks lightly loaded, so both the affinity trade-off and the load scorer keep selecting it"; one queue reached 201 while six pods emptied, and 24% of 18,699 requests exceeded 300 s TTFT. PR #2313 fixed it in v0.10.0 [[flow-control-5-minute-pluginstate-staleness-reaper-destroys-in-flight-accounting]].

Neither is steady-state behavior, but a sticky policy concentrates whatever error its inputs carry, plausibly including overstated reuse from salt-blind matches or dead pods.

### 6.3 The multi-tenant side channel

Cache hits are faster, so timing can leak other tenants' prompts: KVGov measured a cold/cached TTFT ratio of 0.22 on vLLM 0.26.0 and cites attacks reaching up to 100% success against unprotected vLLM and SGLang deployments [[governing-the-kv-cache]]. vLLM's merged `cache_salt` isolates tenants but erases cross-group reuse [[rfc-cache-salting-for-secure-and-flexible-prefix-caching-in-vllm-issue-16016-vll]], while PrefixWall's selective isolation keeps up to 70% more reuse [[prefixwall-mitigating-prefix-caching-side-channels-in-shared-llm-systems]]. In llm-d the engine enforces isolation; the router only decides whether salted traffic gets affinity, which the precise producer handles for vLLM but not SGLang [[sglang_adaptergo]].

### 6.4 Verifying each layer, and what remains open

| Layer | Working if | Broken if |
|---|---|---|
| Engine cache | vLLM "Prefix cache hit rate" climbs [[how-llm-d-prefix-cache-routing-made-qwen-7b-on-eks-23x-faster-dev-community]] | prefix caching disabled |
| Precise signal | non-zero prefix scores for every pod | `bestCachedTokens:0`; TTFT pinned at the 5 s render timeout [[llm-dguidesp2p-kv-cache-sharing-at-main-llm-dllm-d-github]] |
| CPU/FS tier | `vllm:kv_offload_*` metrics show blocks loaded back [[kv-cache-offloading-llm-d]] | writes, zero restores |
| P/D | `llm_d_epp_pd_decision_total` ratio holds [[llm-dguidespd-disaggregationreadmemd-at-main-llm-dllm-d-github]] | empty NIXL panels despite high hit rates |

Open as of September 14, 2026:

- Most guides use the eviction- and salt-blind approximate signal [[llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducerapproximateprefixrea]].
- Dead pods' entries are never purged, whereas Dynamo "removes all its blocks from the global radix tree" [[router-design-nvidia-dynamo-documentation]].
- Tier metadata and SGLang salts are ignored and no TensorRT-LLM adapter exists.
- LMCache multi-process offload is invisible.
- In-flight load is counted per EPP process, so the precise guide runs one router replica.
- No independent adversarial benchmark of llm-d exists.

## 7. Synthesis: How the Pieces Fit Together

llm-d divides labor by knowledge. The engine alone sees allocation and eviction and names blocks by content [[automatic-prefix-caching-vllm]]; connectors alone move blocks; the router alone sees every request and the fleet's load [[llm-d-router-llm-d]]. One narrow contract, the KV event, joins them. It buys portability, since engines plug in without sharing hash functions [[llm-d-kv-cachedocsarchitecturemd-at-main-llm-dllm-d-kv-cache-github]]; correctness for GPU-tier evictions on healthy pods (§3.5), since those reach the index; and composability, since one index drives affinity, P2P source selection and P/D placement.

The same design explains the weaknesses. An observer fails silently: a dropped event, mismatched block size, missing producer, lagging load counter or dead transfer path each leave routing cache-blind but confident. The inputs also interact: a precise signal is wasted under a miscalibrated τ, a perfect τ still sticks to phantom matches if the signal misses a salt, and an offload tier adds capacity the router may never see.

What the evidence supports: calibrated approximate routing suits single-tenant text fleets with light eviction, while the precise signal earns its machinery when prefix demand far exceeds per-pod capacity, when vLLM tenants use salts or LoRA, or when P2P is in play. The observer design's fragility argues for completing the event contract rather than returning to guessing, because the approximate producer permanently shares the eviction, salt and pod-loss blind spots, even though it avoids the render, tokenizer and event-delivery failures precise mode adds.

The trend favors this architecture: vLLM added `locality` and `ownership` to its events [[vllmvllmdistributedkv_eventspy-at-main-vllm-projectvllm-github]], TensorRT-LLM merged a ZeroMQ event publisher on September 10, 2026 [[trtllm-15331feat-improve-kv-event-publish-performance-via-streamingevent-by-tanm]], and the v0.10 roadmap targets "KV-cache observability and intelligence" [[llm-d-v09-hardened-for-scale-llm-d]]. Issue #544's proposal would weight entries by provenance — engine-confirmed 1.0, speculative 0.95, approximate 0.7 — making freshness explicit [[dynamic-speculative-ttl-for-unified-prefix-cache-scoring-issue-544-llm-dllm-d-kv]]. Of the open gaps, a heavy-eviction comparison of precise and approximate signals under the sticky policy would matter most. Until then, llm-d routes on its best estimate of where the cache lives, which is most reliable on a healthy cluster.
