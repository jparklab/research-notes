---
title: 'llm-d: Intelligent Inference at Scale — Beyond What More GPUs Can Deliver
  | by Yakov Beder | Medium'
id: llm-d-intelligent-inference-at-scale-beyond-what-more-gpus-can-deliver-by-yakov
tags:
- llmd-router
- epp-architecture
- kv-cache-scoring
- llmd-router-nonk8s-backends-54b774
- llmd-kv-cache-routing-c665d5
created: '2026-09-11T19:45:03.172708Z'
updated: '2026-09-15T04:55:23.747703Z'
source: https://medium.com/@yakovbeder/llm-d-the-inference-scheduler-that-fixes-what-more-gpus-cant-03644ac55504
source_domain: medium.com
fetched_at: '2026-09-11T19:45:03.150728Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: commentary
content_type: blog
deprecated: false
summary: 'Deep technical walkthrough (dated Apr 9 2026, llm-d v0.6.0) of the llm-d
  Endpoint Picker (EPP): 4-step Discover->Filter->Score->Select cycle, default scoring
  formula final_score = prefix_score*3 + kv_utilization_score*2 + queue_score*2, and
  the full v0.7.1 plugin catalog (handlers: single-profile-handler, disagg-profile-handler,
  disagg-headers-handler; filters: decode-filter/prefill-filter/encode-filter keyed
  on llm-d.ai/role labels, by-label, by-label-selector; scorers: prefix-cache-scorer,
  precise-prefix-cache-scorer, queue-scorer, kv-cache-utilization-scorer, load-aware-scorer,
  active-request-scorer, session-affinity-scorer, no-hit-lru-scorer; deciders: prefix-based-pd-decider;
  pickers: max-score-picker, random-picker). Documents the llm-d-kv-cache Indexer''s
  Read Path (EPP queries KV-Block Index) and Write Path (vLLM pods emit KVEvents via
  ZMQ), and the 3-tier cache hierarchy (GPU HBM hot / CPU DRAM warm / SSD-Lustre-LMCache
  cold). Reports 57x TTFT improvement and 2x throughput on 8 pods/16 H100s vs round-robin,
  and load-aware scoring lifting success rate from ~55% to 100% at high QPS.'
utility_score: 8.0
---

[Sitemap](https://medium.com/sitemap/sitemap.xml)
[Open in app](https://play.google.com/store/apps/details?id=com.medium.reader&referrer=utm_source%3DmobileNavBar&source=---top_nav_layout_nav-------------------------------------------)
Sign up
Get app
Sign up
## Yakov Beder
Senior Cloud Consultant, Red Hat
Follow writer
Press enter or click to view image in full size
# llm-d: Intelligent Inference at Scale — Beyond What More GPUs Can Deliver
Follow
12 min read Apr 9, 2026
Share
If you’re running vLLM at scale, you’ve probably hit this wall: you add more GPUs, throughput goes up, but your P95 latency stays stubbornly high. Your slowest 5% of users still wait 10x longer than your median. More hardware doesn’t fix it.
The problem isn’t capacity. It’s routing.
**llm-d** is a Kubernetes-native inference scheduling framework that sits above vLLM and makes one deceptively simple change: instead of routing requests randomly, it routes them to the pod that _already has the right data in its KV cache_. The result is dramatic — 57x faster time-to-first-token, 2x throughput, and near-elimination of tail latency on identical hardware.
This article is a deep dive into how llm-d works, what its v0.6.0 release brings, and why cache-aware routing is the single most impactful optimization you can make for production LLM inference.
## The Problem: Three Bottlenecks That More GPUs Won’t Fix
When you deploy multiple vLLM replicas behind a standard load balancer, three things conspire to create unpredictable, high tail latency:
**1. Prefill queuing.** Large prompts block smaller requests from starting. A user with a 50-token prompt who submits just after a 10,000-token request can experience 3x higher TTFT simply due to head-of-line blocking.
**2. KV cache misses.** With round-robin routing, requests sharing the same system prompt land on different vLLM instances. Each one recomputes the same 6,000-token prefix from scratch — wasting the entire GPU prefill cycle even though the data was already computed elsewhere.
**3. Unpredictable queue depth.** Random routing ignores how busy each instance actually is. Your P50 might be 200ms while P95 reaches 2,000ms — a 10x gap caused purely by routing blindness.
Adding a 5th, 10th, or 50th GPU replica doesn’t touch any of these. Capacity goes up, but your unluckiest users still get cache misses, still hit overloaded pods, and still wait.
Three bottlenecks that more GPUs won’t fix — prefill queuing, KV cache misses, and queue blindness all feed into high P95/P99 tail latency.
Press enter or click to view image in full size
_Round-robin routing causes three failure modes that compound into high tail latency — regardless of GPU count._
## What is llm-d?
llm-d is a collaborative open-source project by IBM, Google, Red Hat, Alibaba Cloud, DaoCloud, and the broader AI infrastructure community. It provides four “well-lit paths” — documented, tested, and benchmarked solutions for production LLM serving:
**Path 1: Intelligent inference scheduling** — prefix cache-aware routing across vLLM replicas. Validated at 57x faster TTFT and 2x throughput vs round-robin on 8 pods / 16 H100 GPUs.
**Path 2: Prefill/decode disaggregation** — separate pods for the compute-bound prefill phase and the memory-bound decode phase. Up to 50,000 output tokens/s on a 16x16 B200 topology with order-of-magnitude TTFT reduction.
**Path 3: Wide expert parallelism** — distributing large MoE models like DeepSeek-R1 across multiple nodes using expert parallelism + data parallelism. 2,200 tok/s per H200 GPU, ~3,100 tok/s per B200 GPU.
**Path 4: Tiered prefix cache** — extending the KV cache hierarchy beyond GPU vRAM to CPU memory, local SSD, and shared filesystems like Lustre.
These paths are composable. You can run intelligent scheduling + P/D disaggregation + tiered prefix cache in a single deployment. Each ships as a Helmfile guide with reference configuration and validated benchmarks.
For more context, Red Hat provides an excellent overview of the project: [What is llm-d?](https://www.redhat.com/en/topics/ai/what-is-llm-d)
Press enter or click to view image in full size
## The EPP: The Brain of the Routing Layer
At the heart of llm-d is the **Endpoint Picker (EPP)** — a sidecar component that intercepts every inference request via Envoy’s external processing (ext-proc) callback and makes an intelligent pod selection decision before the request is forwarded.
The EPP runs a 4-step cycle for every request:
**Step 1 — Discover.** Enumerate all pods in the InferencePool. Collect queue depth, loaded models, KV cache contents, and real-time metrics via Prometheus and KV-Events.
**Step 2 — Filter.** Exclude pods that are overloaded, have insufficient memory, or cannot serve the requested model variant. This step uses filters like `decode-filter`, `prefill-filter`, `by-label`, and `by-label-selector`.
**Step 3 — Score.** Run pluggable scorers in parallel: session affinity score + prefix cache hit score + load score. The final score is a weighted combination.
**Step 4 — Select.** `max-score-picker` selects the highest-scoring pod. Built-in tie-breaking and fallback logic ensures a pod is always selected, even under degraded conditions.
Press enter or click to view image in full size
## How the Scoring Pipeline Works
The default well-lit path uses four plugins in sequence:
`single-profile-handler` selects the scheduling profile. It doesn't produce a score — it's a handler that routes the request into the correct scorer pipeline.
`prefix-cache-scorer` **(weight 3)** queries the KV-cache index to find what percentage of the incoming prompt's prefix is already cached on each pod. A pod with 90% of the prompt cached scores 0.9 × 3 = 2.7. This is the dominant signal by design — eliminating redundant prefill is the single highest-value optimization for tail latency.
`kv-cache-utilization-scorer` **(weight 2)** reads each pod's GPU vRAM KV cache utilization. A pod near full capacity scores lower, preventing the router from selecting a pod that would need to evict existing blocks to fit the new request — which would negate any cache hit.
`queue-scorer` **(weight 2)** counts pending requests queued on each pod. Without this, cache affinity alone would route all traffic to one warm pod until its queue overflows. This is the load-balancing safety valve.
The formula per pod:

```
final_score = (prefix_score × 3) + (kv_utilization_score × 2) + (queue_score × 2)
```

A pod with a perfect cache hit but a full queue can still lose to a pod with a partial cache hit and an empty queue — which is exactly the right behavior.
The weights are tunable in `EndpointPickerConfig`. The 3:2:2 ratio leans hard on cache hits, which is optimal when shared prefixes are large and frequent. Equal weights (1:1:1) work well when prefix-sharing is low.
EPP scoring pipeline — 4-step cycle (Discover → Filter → Score → Select) and weighted scoring formula per pod.
Press enter or click to view image in full size
_The EPP scores every candidate pod using a weighted combination of prefix cache affinity, KV utilization, and queue depth._
## The 7 Scheduling Techniques
llm-d’s capabilities extend well beyond the default plugin pipeline. Here are all seven scheduling techniques available in the stack:
Press enter or click to view image in full size
_Handler → Filter → Score → Decide → Pick — the full plugin lifecycle for every request._
### 1. Session-Aware Routing
Pins requests from the same conversation to the same vLLM pod. Every new turn is a cache hit — no re-prefill of previous turns. The limitation: it misses cross-user cache reuse. 150 enterprise users sharing the same system prompt means 150 separate sessions, each re-prefilling identical content.
### 2. Prefix-Cache-Aware Scheduling
Scores pods by how much of the incoming prompt prefix they already have cached — _even across different users_. Two flavors: approximate (predicts from traffic patterns) and precise (reads actual KV block state directly from vLLM via KV-Events). The precise variant delivers up to 57x faster TTFT.
### 3. KV-Cache-Aware Routing
The infrastructure layer that makes prefix-aware and session-aware routing possible. The EPP maintains a global near-real-time index of KV cache block locality across all vLLM pods: what prefixes are cached where, how warm each block is, and how full each pod’s cache pool is. Without this, the gateway is cache-blind.
### 4. Load-Aware Scoring
Prevents cache affinity from creating hotspots. Without load-awareness, prefix-only routing succeeded on only ~55% of requests at high QPS. With the combined scorer: 100% success rate and 5x lower inter-token latency at the same load.
### 5. Prefill/Decode Disaggregation
Separates the compute-bound prefill phase from the memory-bound decode phase onto dedicated pods. KV cache is transferred between them via NIXL over InfiniBand/RoCE RDMA (default port 5600 since vLLM v0.15.1). Decode pods use higher tensor parallelism for larger memory per GPU; prefill pods use more replicas with lower TP for throughput. Now also supported on AMD ROCm, Intel XPU and Intel Gaudi/HPU accelerators.
### 6. Tiered KV Prefix Caching
Extends the KV cache beyond GPU vRAM using vLLM’s KVConnector abstraction. Hot blocks stay in GPU HBM, warm blocks offload to CPU DRAM, and cold blocks go to local SSD or shared filesystems via the llm-d FS backend (Lustre, LMCache, Mooncake). This dramatically improves cache hit rates for workloads where shared context exceeds a single GPU’s vRAM.
### 7. Workload Variant Autoscaling
The Workload Variant Autoscaler (WVA) extends Kubernetes HPA with inference-specific signals: queue depth, cache utilization, and latency SLOs. It can independently scale prefill and decode pools, and supports scale-to-zero for idle models — critical for multi-model SaaS deployments.
## The Complete Plugin Reference (v0.7.1)
The llm-d-inference-scheduler v0.7.1 includes a rich set of pluggable components organized by category:
Press enter or click to view image in full size
**Handlers** route requests into the correct scheduling profile:
  * `single-profile-handler` — for non-disaggregated serving
  * `disagg-profile-handler` — selects profiles for P/D or E/P/D disaggregation
  * `disagg-headers-handler` — sets HTTP headers for disaggregated routing


**Filters** narrow the candidate pod set:
  * `decode-filter`, `prefill-filter`, `encode-filter` — filter by `llm-d.ai/role` labels for disaggregated topologies
  * `by-label` — filter by any specific label with allowed values
  * `by-label-selector` — standard Kubernetes matchLabels with AND logic


**Scorers** assign weighted scores to candidate pods:
  * `prefix-cache-scorer` — estimation-based, approximate locality from routing history
  * `precise-prefix-cache-scorer` — real-time KV cache state tracking via llm-d-kv-cache with KV-Events
  * `queue-scorer` — pending request count per pod
  * `kv-cache-utilization-scorer` — GPU vRAM KV cache utilization percentage
  * `kv-cache-scorer` — KV cache metrics, used in benchmarking configs
  * `load-aware-scorer` — concurrent request load with overload threshold
  * `active-request-scorer` — active in-flight requests with per-request TTL tracking
  * `session-affinity-scorer` — higher scores for pods that previously served the same session
  * `no-hit-lru-scorer` — distributes cache growth evenly for cold requests via LRU ordering


**Deciders** control disaggregation behavior:
  * `prefix-based-pd-decider` — triggers P/D disaggregation based on uncached token count


**Pickers** make the final pod selection:
  * `max-score-picker` — selects the pod with the highest aggregate weighted score
  * `random-picker` — random selection, used as a baseline in benchmarks


## KV Cache Management: The Indexer Architecture
The `llm-d-kv-cache` library (v0.7.1) is the foundation that makes cache-aware routing possible. At its core is the **KV-Cache Indexer** — a high-performance library that keeps a global, near-real-time view of KV-Cache block locality across the entire fleet of vLLM pods.
## Get Yakov Beder’s stories in your inbox
Join Medium for free to get updates from this writer.
Subscribe
Subscribe
Remember me for faster sign in
It works through two paths:
**The Read Path (Scoring):** When the EPP needs to score pods for a new request, it asks the KV-Cache Indexer. The indexer calculates the necessary KV-block keys from the prompt, queries the KV-Block Index to see which pods have those blocks, and returns a score map.
**The Write Path (Event Ingestion):** As vLLM pods create or evict KV-blocks, they emit `KVEvents` via ZMQ. The Event Subscriber consumes these events and updates the KV-Block Index in near-real-time.
The tiered cache hierarchy has three levels:
  * **Tier 0 (Hot)** — GPU HBM (vRAM). Nanosecond access. Active inference context and frequently reused prefixes.
  * **Tier 1 (Warm)** — CPU DRAM. Microsecond access. Recently evicted blocks, overflow from GPU vRAM.
  * **Tier 2 (Cold)** — Local SSD, Lustre, or LMCache. Millisecond access. Long-tail prefixes and shared system prompts across sessions. The llm-d FS backend enables any POSIX-compatible shared filesystem.


Blocks are promoted and demoted between tiers automatically based on access patterns. The `precise-prefix-cache-scorer` is aware of which tier each block resides on, enabling cache-tier-aware routing decisions.
KV cache tiered architecture — GPU HBM (hot) → CPU DRAM (warm) → SSD/Lustre (cold)
Press enter or click to view image in full size
_Blocks are automatically promoted and demoted between tiers based on access patterns. The precise-prefix-cache-scorer is tier-aware._
## Wide Expert Parallelism: Scaling MoE Models Across Nodes
For very large Mixture-of-Experts models like DeepSeek-R1, llm-d provides a Wide Expert Parallelism (EP/DP) well-lit path that distributes experts across GPUs using the DeepEP backend with All-to-All RDMA communication, combined with Data Parallelism for throughput.
The architecture uses **LeaderWorkerSets** — a Kubernetes CRD that manages multi-host inference groups where a leader pod coordinates workers. A typical deployment uses DP=16 prefill workers and DP=16 decode workers, each independently scalable.
Validated configurations in v0.6.0:
  * **DeepSeek-R1–0528** on 32x H200 GPUs with InfiniBand → 2,200 tok/s per GPU
  * **DeepSeek-R1–0528** on 32x H200 GPUs (GKE) with RoCE → comparable performance
  * **DeepSeek-R1–0528** on 32x B200 GPUs (GKE) with RoCE → ~3,100 tok/s per GPU


One critical networking requirement: the DeepEP backend requires full-mesh All-to-All RDMA connectivity. Every NIC on a host must be able to communicate with every NIC on all other hosts. Rail-only connectivity will fail.
Wide expert parallelism — EPP routing to DP=16 prefill and decode workers with NIXL over RDMA, validated at 2,200–3,100 tok/s per GPU
Press enter or click to view image in full size
_DeepSeek-R1 deployed with DP=16 prefill + DP=16 decode. Full-mesh RDMA connectivity is required._
## The Numbers: Naive Scaling vs. llm-d
Here’s what changes when you replace round-robin with cache-aware routing on the same hardware:
**Cache utilization** goes from 20–30% hit rate (isolated caches per pod) to 60–80% hit rate (intelligent routing across the cluster).
**P95/P99 TTFT** drops by 40–60% — cache hits eliminate redundant prefill computation entirely.
**Multi-turn conversations** go from recomputing context on every turn (cache misses) to reusing cached context (cache hits).
**Single-turn shared prefixes** go from every request recomputing the shared prefix to only computing the unique suffix.
**Customer experience** goes from “capacity increases but P95 stays high for unlucky users” to “dramatically better tail-latency experience for all users.”
The key insight bears repeating: **adding more GPUs with naive scaling increases capacity but does NOT reduce tail latency**. Cache misses still occur, causing high latency for unlucky users. llm-d’s cache-aware routing directly attacks the tail latency problem by eliminating redundant computation through intelligent cache reuse.
Naive vLLM scaling vs. llm-d — side-by-side comparison showing 57x TTFT improvement, 2x throughput, and 80% cache hit rate
Press enter or click to view image in full size
_Same hardware, dramatically different results. Cache-aware routing is the single highest-impact change._
## What’s New in v0.6.0
llm-d v0.6.0, released April 3, 2026, brings several important updates:
  * **Inference scheduler v0.7.1** with expanded plugin ecosystem including `no-hit-lru-scorer`, `active-request-scorer`, and speculative indexing
  * **New accelerator support** : AMD ROCm, Intel Gaudi/HPU, CPU on AMX — all with container images
  * **AWS EFA** support re-enabled
  * **kgateway deprecated** in favor of agentgateway for new deployments
  * **Gateway API Inference Extension v1.4.0**
  * **vLLM v0.15.1** with default NIXL port changed to 5600
  * **WVA graduated** from experimental to core component
  * **Tiered prefix cache guide simplified** and unified


The project continues to move fast, with the v0.5 blog post outlining the roadmap toward predictive latency-aware routing, proactive state management APIs, pluggable autoscaling optimizers, and end-to-end distributed tracing.
## Final Thoughts
llm-d addresses a fundamental blind spot in how most teams scale LLM inference today: they add hardware without fixing the routing that causes tail latency in the first place. The shift from cache-blind load balancing to cache-aware scheduling is not incremental — it’s a structural change that unlocks an order-of-magnitude improvement on the same hardware.
What makes the project compelling isn’t just the performance numbers. It’s the architecture: a pluggable, composable system where you can start with simple prefix-aware routing and progressively layer on P/D disaggregation, tiered KV caching, expert parallelism, and SLO-aware autoscaling as your workload demands it. Each well-lit path is independently validated and ships with reproducible benchmarks.
If you’re operating vLLM at any meaningful scale, cache-aware routing should be the first optimization you evaluate — before adding hardware, before tuning model parameters, before exploring quantization. The returns are immediate and the deployment is non-invasive: llm-d sits above your existing model server, not inside it.
## **Resources**
  * **Official documentation and guides** : [llm-d.ai](https://llm-d.ai)
  * **GitHub repository** : [github.com/llm-d/llm-d](https://github.com/llm-d/llm-d)
  * **Red Hat overview of llm-d** : [What is llm-d?](https://www.redhat.com/en/topics/ai/what-is-llm-d)
  * **Deploying models with distributed inference on OpenShift AI** : [Red Hat OpenShift AI — Deploying Models](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/3.4/html/deploying_models/deploying_models#deploying-models-using-distributed-inference_rhoai-user)
  * **Intelligent Inference Scheduling guide** : [Getting started](https://llm-d.ai/docs/guide/Installation/inference-scheduling)


Follow
## [Written by Yakov Beder](https://medium.com/@yakovbeder?source=post_page---post_author_info--03644ac55504-----------------------------------------)
Senior Cloud Consultant, Red Hat
Follow
