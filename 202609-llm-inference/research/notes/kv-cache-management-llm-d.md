---
title: KV Cache Management | llm-d
id: kv-cache-management-llm-d
tags:
- llmd-kv-cache-routing-c665d5
- llm-d
- kv-cache
- prefix-caching
- architecture
created: '2026-09-14T11:52:41.875628Z'
updated: '2026-09-15T04:55:23.999382Z'
source: https://llm-d.ai/docs/dev/architecture/advanced/kv-management
source_domain: llm-d.ai
fetched_at: '2026-09-14T11:52:41.853954Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'llm-d dev-version overview page defining KV cache management as three composable
  architectural pillars: (1) Prefix-Cache Aware Routing — the EPP-managed intelligence
  layer that routes requests to replicas already holding the relevant KV cache, offered
  in Approximate (heuristic) and Precise (event-driven) variants; (2) KV-Cache Indexing
  — the observability layer consuming high-frequency KVEvents from engines like vLLM
  to track block movement/eviction across offloaded tiers; (3) KV Offloading — the
  capacity layer that spills cache to CPU memory or SSD beyond GPU HBM limits. Also
  introduces a fourth layer, P2P Sharing, which composes the other three into a fleet-wide
  cache: the index identifies which peer holds a prefix, the router stamps the request,
  and the model server pulls blocks directly from the peer''s CPU offload tier over
  NIXL without touching the source pod''s GPU; a promised-but-undelivered block degrades
  to normal recompute rather than failing the request.'
utility_score: 16.0
---

[Skip to main content](https://llm-d.ai/docs/dev/architecture/advanced/kv-management#__docusaurus_skipToContent_fallback)
🎉 **llm-d 0.9 is here!** Hardened for scale with router HA, enhanced autoscaling, end-to-end observability and expanded hardware support. [**Read the release blog →**](https://llm-d.ai/blog/llm-d-v0.9-hardened-for-scale)
This is unreleased documentation for llm-d **dev** version.
For up-to-date documentation, see the (v0.9 (latest)).
Version: dev
On this page
Key-Value (KV) cache management is the foundation of high-performance LLM serving in llm-d. By efficiently tracking, preserving, and reusing the KV cache—the intermediate state generated during LLM inference—llm-d significantly reduces latency and increases the overall throughput of the inference pool.
The KV cache management ecosystem in llm-d consists of three core architectural pillars. By composing these layers, llm-d allows an inference pool to scale its "effective cache capacity" far beyond physical HBM limits, sustaining high hit rates even under heavy, diverse workloads. The three pillars are:
## Prefix-Cache Aware Routing[​](https://llm-d.ai/docs/dev/architecture/advanced/kv-management#prefix-cache-aware-routing "Direct link to Prefix-Cache Aware Routing")
The "intelligence" layer managed by the **llm-d Router** (via its **EPP** component) that uses the index to determine the optimal model server Pod for each incoming request. It aims to maximize "cache hits" by routing requests to replicas that already contain the relevant KV cache for the request's prompt prefix.
See **[Prefix-Cache Aware Routing](https://llm-d.ai/docs/dev/architecture/advanced/kv-management/prefix-cache-aware-routing)** for a deep dive into the two different routing implementations that the llm-d Router offers: the Approximate (heuristic-based) and the Precise (event-driven) implementations.
## KV-Cache Indexing[​](https://llm-d.ai/docs/dev/architecture/advanced/kv-management#kv-cache-indexing "Direct link to KV-Cache Indexing")
The "observability" layer that continuously monitors the state of the pool, knowing exactly what is cached and where (including offloaded tiers and across all active model servers). It consumes high-frequency events from engines like vLLM to track the movement and eviction of individual token blocks.
See for a deep dive into how the indexer processes `KVEvents` and provides the source-of-truth for precise routing decisions.
## KV Offloading[​](https://llm-d.ai/docs/dev/architecture/advanced/kv-management#kv-offloading "Direct link to KV Offloading")
The "capacity" layer that extends the cache beyond the limited high-bandwidth memory (HBM) of accelerators (GPUs/TPUs). It enables model servers to "spill" or offload cache entries to CPU memory or local SSDs, effectively creating a tiered storage hierarchy for the KV cache.
for a deep dive into the design of the multi-tier storage API and its integration with the inference engine.
## P2P Sharing[​](https://llm-d.ai/docs/dev/architecture/advanced/kv-management#p2p-sharing "Direct link to P2P Sharing")
The "sharing" layer that composes the three pillars into a fleet-wide cache: the index knows which peer holds a request's prefix, the router stamps the request with that source, and the model server pulls the blocks directly from the peer's CPU offload tier over NIXL instead of recomputing them. The source pod's GPU is never touched, so serving a pull costs it no prefill capacity, and an ordinary lookup miss or partial match degrades to a normal recompute rather than failing the request (a promised-but-undelivered block is a documented limitation - see the guide).
**See[Enable P2P Prefix Cache Sharing](https://llm-d.ai/docs/dev/well-lit-paths/foundations/enable-p2p-prefix-cache-sharing)** for the mechanism and the when-to-use rule, and the **[P2P KV Cache Sharing guide](https://github.com/llm-d/llm-d/tree/main/guides/p2p-kv-cache-sharing)** for deployment and benchmarks.
  * [Prefix-Cache Aware Routing](https://llm-d.ai/docs/dev/architecture/advanced/kv-management#prefix-cache-aware-routing)


