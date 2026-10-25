---
title: Precise Prefix Cache Routing | llm-d
id: precise-prefix-cache-routing-llm-d
tags:
- llmd-kv-cache-routing-c665d5
- llm-d
- kv-cache
- prefix-caching
- kv-events
- routing
created: '2026-09-14T11:53:07.713480Z'
updated: '2026-09-15T04:55:24.014619Z'
source: https://llm-d.ai/docs/well-lit-paths/foundations/precise-prefix-cache-routing
source_domain: llm-d.ai
fetched_at: '2026-09-14T11:53:07.694503Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'v0.9 (latest, well-lit-paths/foundations) doc describing precise prefix-cache
  routing as a deployable path built on model-server-emitted KV-events (vLLM, SGLang,
  and NVIDIA TensorRT-LLM all publish cache-change events) that llm-d subscribes to,
  building a near-real-time view of resident blocks across the fleet and scoring requests
  against it; the prefix-affinity score combines with standard load-aware scorers
  as in the Optimized Baseline path. Frames KV-events as the emerging ecosystem-standard
  substrate for exposing cache state, extensible to tier-aware tracking (GPU HBM/CPU
  DRAM/NVMe/shared storage), prompt-cache placement policies, dynamic KV-offloading,
  prefetching, agentic-workload retention/eviction, and hybrid-attention models. Architecture:
  model servers produce KV-events on every cache change; the llm-d Router''s indexer
  maintains a block-key-to-pods mapping and its scorer derives block keys deterministically,
  returning the longest consecutive cached prefix per candidate pod weighted by tier.
  Events flow over ZMQ via pod discovery (each model-server pod binds its own socket;
  every Router replica subscribes to every pod independently), so all Router replicas
  converge to the same index for active-active HA with no extra coordination; the
  reference guide deploys two Router replicas behind one Service by default, scalable
  down to one for small fleets.'
utility_score: 15.0
---

[Skip to main content](https://llm-d.ai/docs/well-lit-paths/foundations/precise-prefix-cache-routing#__docusaurus_skipToContent_fallback)
🎉 **llm-d 0.9 is here!** Hardened for scale with router HA, enhanced autoscaling, end-to-end observability and expanded hardware support. [**Read the release blog →**](https://llm-d.ai/blog/llm-d-v0.9-hardened-for-scale)
Version: v0.9 (latest)
On this page
The model server is the most accurate source of truth for what's cached on its own GPUs and memory tiers. vLLM, SGLang and NVIDIA TensorRT-LLM publish every cache change as an event; llm-d subscribes to that stream, builds a near-real-time view of resident blocks across the fleet, and scores requests against it. The prefix-affinity score is combined with the standard load-aware scorers, similarly to the [Optimized Baseline](https://llm-d.ai/docs/well-lit-paths/foundations/optimized-baseline) path.
KV-events have become the ecosystem-standard substrate for exposing accurate cache state — where reusable inference state lives and how it changes over time. As KV-cache orchestration grows more sophisticated and agentic workloads stretch prefixes longer, cache state becomes something the control plane needs to observe and act on. The same view scales naturally to:
  * tier-aware cache tracking across GPU HBM, CPU DRAM, local NVMe, and shared storage;
  * policies that account for explicit prompt-cache placement and dynamic KV-offloading;
  * cache movement and prefetching workflows for fleet-wide KV reuse;
  * advanced KV retention and eviction policies for agentic patterns;
  * hybrid-attention models where layer groups (full, sliding-window, linear) evict independently.


## Deploy[​](https://llm-d.ai/docs/well-lit-paths/foundations/precise-prefix-cache-routing#deploy "Direct link to Deploy")
See the [precise prefix cache routing guide](https://github.com/llm-d/llm-d/tree/main/guides/precise-prefix-cache-routing) for manifests and step-by-step deployment.
## Architecture[​](https://llm-d.ai/docs/well-lit-paths/foundations/precise-prefix-cache-routing#architecture "Direct link to Architecture")
The split is straightforward: **model servers** produce KV-events on every cache change; the **llm-d Router** consumes them to score pods for better routing decisions. The two sides are decoupled — model server and llm-d Router replicas scale independently.
Inside the llm-d Router:
  * An **indexer** consumes the event stream and maintains a `block key → pods` mapping for every block resident across the fleet.
  * A **scorer** derives block keys deterministically from the input and queries the index. It returns the longest consecutive prefix each candidate pod has cached, weighted by tier.


Events flow from model-server pods to the llm-d Router over ZMQ via **pod discovery** : each model-server pod binds its own ZMQ socket and every llm-d Router replica subscribes to every pod independently. All Router replicas converge to the same index, enabling active-active HA out of the box. The reference guide ships with two Router replicas behind one Service by default, scalable down to one for small fleets.
## Further Reading[​](https://llm-d.ai/docs/well-lit-paths/foundations/precise-prefix-cache-routing#further-reading "Direct link to Further Reading")
See [KV-Cache Indexer](https://llm-d.ai/docs/architecture/advanced/kv-management/kv-indexer) for the full architecture


