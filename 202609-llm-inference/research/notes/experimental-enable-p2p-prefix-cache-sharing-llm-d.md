---
title: '[Experimental] Enable P2P Prefix Cache Sharing | llm-d'
id: experimental-enable-p2p-prefix-cache-sharing-llm-d
tags:
- llmd-kv-cache-routing-c665d5
- llm-d
- kv-cache
- p2p-sharing
- kv-offloading
- nixl
- derivative-of
created: '2026-09-14T11:56:26.860755Z'
updated: '2026-09-15T04:55:24.075274Z'
source: https://llm-d.ai/docs/dev/well-lit-paths/foundations/enable-p2p-prefix-cache-sharing
source_domain: llm-d.ai
fetched_at: '2026-09-14T11:56:26.839408Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'Experimental (dev-version) llm-d well-lit-path doc on P2P prefix cache sharing,
  which lets a model server pull cached prefix KV blocks CPU-to-CPU over NIXL from
  a peer that already holds them, instead of recomputing, when prefix-aware routing
  can''t co-locate the request with its owner (hot-prefix saturation, working-set
  overflow, session rebalancing). Mechanism: the request''s originating pod is the
  KV cache source (computed the prefix, cached a copy in CPU tier); the router names
  that source on a later request sharing the prefix but scheduled elsewhere; the scheduled
  pod (consumer) pulls the prefix blocks rather than recomputing, with the source''s
  GPU never touched, so a pull costs the source zero prefill capacity. Builds on the
  Tiered Prefix Cache path (vLLM OffloadingConnector with a CPU tier plus P2P secondary
  tier) and the Precise Prefix Cache Routing index; the p2p-source-producer selects
  a source from CPU-tier holders within one index block of the largest cached prefix,
  weighted to avoid overloading one source, and only fires when the source leads the
  scheduled pod by at least minCachedTokenDelta tokens; a failed pull degrades to
  a normal local miss/recompute. Gives concrete benchmark numbers: on measured H200/RDMA
  hardware with openai/gpt-oss-120b, the CPU-to-CPU pull was faster than recompute
  at every measured prefix length from 2,048 to 49,152 tokens, and on a warm 16-pod
  document Q&A benchmark, load-aware placement plus the pull delivered 1.5x better
  p99 TTFT and 35% higher throughput than prefix-affinity routing alone. Under P/D
  disaggregation, the prefill worker is the pull consumer (it computes prompt KV)
  while a decode worker can be the source for retained session history.'
---

*Suggested by [[kv-cache-management-llm-d]] — cited as the guide for the P2P Sharing pillar of KV cache management*

[Skip to main content](https://llm-d.ai/docs/dev/well-lit-paths/foundations/enable-p2p-prefix-cache-sharing#__docusaurus_skipToContent_fallback)
🎉 **llm-d 0.9 is here!** Hardened for scale with router HA, enhanced autoscaling, end-to-end observability and expanded hardware support. [**Read the release blog →**](https://llm-d.ai/blog/llm-d-v0.9-hardened-for-scale)
This is unreleased documentation for llm-d **dev** version.
For up-to-date documentation, see the **[latest version](https://llm-d.ai/docs)** (v0.9 (latest)).
Version: dev
On this page
Prefix caches are per-pod, but their content is often fleet-wide: shared system prompts, common documents, session histories. Prefix-aware routing sends each request to the pod that caches its prefix, but routing cannot always follow the cache: a hot prefix's owner saturates, a working set outgrows any single pod, a session is rebalanced. Those requests recompute KV tensors that already exist on a peer.
P2P prefix cache sharing closes that gap: a model server pulls cached prefix KV blocks from a peer's CPU offload tier instead of recomputing them. The transfer is CPU-to-CPU over NIXL. The source pod's GPU is never touched, so serving a pull costs the source no prefill capacity.
The pull fires when a request shares a prefix with an earlier one but is scheduled to a different pod. Two requests share a prefix whenever they begin with the same tokens: the next turn of a conversation, another question against the same document, another session on a shared system prompt. The first request's pod is the **KV cache source** : it computed the prefix and holds a copy in its CPU tier. When the router schedules a prefix-sharing request to a different pod, it names the source on the request, and the scheduled pod - the **consumer** - pulls the prefix instead of recomputing it:

```
%% sequence diagram
consumer pod
(serves request 2, prefix missing)
KV cache source pod
(serves request 1, caches the prefix)
llm-d router
computes the prefix KV, caches it,
offloads a copy to its CPU tier
request 2 arrives sharing request 1's prefix,
but placement picks a different pod
recomputes the full shared prefix
computes only the remainder
(request 2's unshared tokens)
alt
[without P2P prefix cache sharing]
[with P2P prefix cache sharing]
request 1
request 2 + header naming the source pod
request the prefix blocks
prefix KV blocks, CPU tier to CPU tier over NIXL
```

info
P2P prefix cache sharing builds on the [Tiered Prefix Cache](https://llm-d.ai/docs/dev/well-lit-paths/foundations/tiered-prefix-cache) path: peers serve pulls from their CPU offload tier. The shipped configuration uses the same `--block-size`, `PYTHONHASHSEED`, and tensor parallel layout on every peer. Other layouts need a compatible peer session fingerprint; the guide documents the tensor-parallel and model-runner constraints. Size the CPU tier to retain useful blocks; making it larger than the per-pod GPU KV cache is the guide's recommended starting point, not a correctness requirement. The [guide's Best Practices](https://github.com/llm-d/llm-d/tree/main/guides/p2p-kv-cache-sharing#best-practices) covers each requirement, its sizing rule, and its failure mode.
## When It Pays[​](https://llm-d.ai/docs/dev/well-lit-paths/foundations/enable-p2p-prefix-cache-sharing#when-it-pays "Direct link to When It Pays")
Recompute cost grows with prefix length. On the measured H200/RDMA setup, the CPU-to-CPU pull also grows with prefix length, but much more slowly. For `openai/gpt-oss-120b`, the pull was faster at every measured point from 2,048 to 49,152 tokens. The crossover is model-, hardware-, and transport-specific. The router requests a pull only when the selected source holds at least `minCachedTokenDelta` more prefix tokens than the scheduled pod, so calibrate that value on the target deployment.
Whether the pull helps also depends on the placement policy in front of it:
  * **Ownership is stable and uncontended** : prefix-aware routing avoids the transfer. A local hit is cheaper than a pull, so the pull stays quiet.
  * **A hot prefix saturates its owner, or the working set outgrows the caches** : load-aware placement plus the pull serves the same content from the whole fleet. On the warm 16-pod document Q&A benchmark this delivered 1.5x better p99 TTFT and 35% higher throughput than prefix-affinity routing.
  * **GPU KV capacity itself is the bottleneck** : cache co-location can use capacity more efficiently. Concurrent same-prefix requests on one pod share one copy of the blocks; spreading pays a per-pod copy whether the prefix is pulled or recomputed.


The guide ships prefix affinity plus the pull as the general-purpose default. Reach for load-aware placement plus the pull when many concurrent sessions contend on their owner pods. Both regimes are measured in the [benchmark report](https://github.com/llm-d/llm-d/tree/main/guides/p2p-kv-cache-sharing/benchmark-results/gpt-oss-120b-h200.md).
## Deploy[​](https://llm-d.ai/docs/dev/well-lit-paths/foundations/enable-p2p-prefix-cache-sharing#deploy "Direct link to Deploy")
See the [P2P KV Cache Sharing guide](https://github.com/llm-d/llm-d/tree/main/guides/p2p-kv-cache-sharing) for manifests, verification gates, and step-by-step deployment.
## Architecture[​](https://llm-d.ai/docs/dev/well-lit-paths/foundations/enable-p2p-prefix-cache-sharing#architecture "Direct link to Architecture")
  1. **Model server pods publish KV-cache events** and run vLLM's `OffloadingConnector` with a CPU tier plus a P2P secondary tier: every pod both offloads computed KV to CPU and serves it to peers.
  2. **The router builds the precise prefix index** from the KV events (the [Precise Prefix Cache Routing](https://llm-d.ai/docs/dev/well-lit-paths/foundations/precise-prefix-cache-routing) mechanism), so it knows which pods hold which prefix blocks.
  3. **The`p2p-source-producer` selects a source** from the CPU-tier holders within one index block of the largest cached prefix, weighted to avoid concentrating pulls on a queued source. After scheduling, it sets the KV cache source header only when that source leads the computing pod by at least `minCachedTokenDelta` tokens.
  4. **The routing sidecar injects`kv_transfer_params.remote_kv_source`** from the header, and the engine pulls the prefix blocks from the peer's CPU tier over NIXL. Hits load as normal cache hits; a failed lookup is reported as a miss and the scheduled pod computes the missing prefix locally.


Under P/D disaggregation, the prefill worker is the pull consumer because it computes the prompt KV. A decode worker may be the source for generated session history retained in its CPU tier. After prefill completes, the normal NIXL P/D path transfers the request's KV to the selected decoder.
## Further Reading[​](https://llm-d.ai/docs/dev/well-lit-paths/foundations/enable-p2p-prefix-cache-sharing#further-reading "Direct link to Further Reading")
  * [P2P KV Cache Sharing guide](https://github.com/llm-d/llm-d/tree/main/guides/p2p-kv-cache-sharing) - manifests, verification gates, benchmarking.
  * [Benchmark report: gpt-oss-120b on H200](https://github.com/llm-d/llm-d/tree/main/guides/p2p-kv-cache-sharing/benchmark-results/gpt-oss-120b-h200.md) - crossover, shared-prefix pools, document Q&A.
  * [Benchmark report: GLM-5.2 on H200](https://github.com/llm-d/llm-d/tree/main/guides/p2p-kv-cache-sharing/benchmark-results/glm-5.2-h200.md) - C64 policy comparison, four-arm observation, and pull mechanism evidence.
  * [Tiered Prefix Cache](https://llm-d.ai/docs/dev/well-lit-paths/foundations/tiered-prefix-cache) - the offload tiers P2P serves from.
  * [Precise Prefix Cache Routing](https://llm-d.ai/docs/dev/well-lit-paths/foundations/precise-prefix-cache-routing) - the index that selects the pull source.




