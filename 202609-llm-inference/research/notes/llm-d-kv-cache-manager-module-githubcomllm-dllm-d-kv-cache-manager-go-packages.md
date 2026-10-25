---
title: llm-d-kv-cache-manager module - github.com/llm-d/llm-d-kv-cache-manager - Go
  Packages
id: llm-d-kv-cache-manager-module-githubcomllm-dllm-d-kv-cache-manager-go-packages
tags:
- llmd-router-nonk8s-backends
- kv-cache-indexer
- zmq-kv-events
- non-k8s-backend
- llmd-router-nonk8s-backends-54b774
- llmd-kv-cache-routing-c665d5
created: '2026-09-11T19:44:15.880176Z'
updated: '2026-09-15T04:55:23.715187Z'
source: https://pkg.go.dev/github.com/llm-d/llm-d-kv-cache-manager
source_domain: pkg.go.dev
fetched_at: '2026-09-11T19:44:15.860492Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Go Packages page for github.com/llm-d/llm-d-kv-cache-manager (v0.4.0), the
  pluggable KV-Cache Indexer library. Confirms architecture: kvcache.Indexer orchestrates;
  kvblock.Index is the block-hash-to-pod map; kvevents.Pool is the KVEvents ingestion
  component (write path, ingests events emitted by vLLM pods). Read path: scheduler
  calls Indexer.Score(prompt, pods) -> Indexer queries kvblock.Index -> returns per-pod
  cache-hit scores. Write path: vLLM pods emit KVEvents on block create/evict -> Event
  Subscriber (kvevents.Pool) updates kvblock.Index in near-real-time. Directly relevant
  to non-Kubernetes backends: ships a **Valkey Backend** example (''examples/valkey_example/README.md'')
  showing how to configure Valkey (Redis-compatible) as the storage backend for KV-block
  indexing, including RDMA support — i.e., the block index itself can be backed by
  an external key-value store rather than only in-process memory, which is a plausible
  integration point for a router that needs to track non-k8s-pod endpoints. Also ships
  a kv_events/offline example using ''a dummy ZMQ publisher'' (no live vLLM/pod required)
  and a kv_events/online example against a real vLLM Helm deployment — the offline
  example is evidence that the ZMQ event-consumer side can be exercised against any
  process that speaks the same ZMQ pub/sub + msgpack wire format, independent of Kubernetes.
  Points to docs/architecture.md and docs/configuration.md at the v0.4.0 tag for deeper
  detail (not yet fetched).'
utility_score: 13.0
---

##  README [¶](https://pkg.go.dev/github.com/llm-d/llm-d-kv-cache-manager#section-readme "Go to Readme")
### KV-Cache Manager
##### Introduction
Efficiently caching Key & Value (KV) tensors is crucial for optimizing LLM inference. Reusing the KV-Cache, rather than recomputing it, significantly improves both Time To First Token (TTFT) and overall throughput, while also maximizing system resource-utilization. As a distributed LLM inference platform, `llm-d` provides a comprehensive suite of KV-Cache management capabilities to achieve these goals.
This repository contains the `llm-d-kv-cache-manager`, a pluggable service designed to enable **KV-Cache Aware Routing** and lay the foundation for advanced, cross-node cache coordination in vLLM-based serving platforms.
##### Project Northstar
See the [Project Northstar](https://docs.google.com/document/d/1EM1QtDUaw7pVRkbHQFTSCQhmWqAcRPJugJgqPbvzGTA/edit?tab=t.ikcvw3heciha) document for a detailed overview of the project's goals and vision.
#### KV-Cache Indexer Overview
The major component of this project is the **KV-Cache Indexer** is a high-performance library that keeps a global, near-real-time view of KV-Cache block locality across a fleet of vLLM pods.
It is powered by `KVEvents` streamed from vLLM, which provide structured metadata as KV-blocks are created or evicted from a vLLM instance's KV-cache. This allows the indexer to track which blocks reside on which nodes and on which tier (e.g., GPU or CPU). This metadata is the foundation for intelligent routing, enabling schedulers to make optimal, KV-cache-aware placement decisions.
The diagram below shows the primary data flows: the **Read Path** (scoring) and the **Write Path** (event ingestion).

```
graph TD
    subgraph "Inference Scheduler"
        A[Scheduler]

        subgraph "KV-Cache Manager"
            B[`kvcache.Indexer`]
            C[`kvblock.Index`]
            D[`kvevents.Pool`]
        end
    end

    subgraph "vLLM Fleet"
        E[vLLM Pod 1]
        F[vLLM Pod 2]
        G[...]
    end

    A--"1: Score(prompt, pods)"-->B
    B--"2: Query Index"-->C
    B--"3: Return Scores"-->A
    
    E--"A: Emit KVEvents"-->D
    F--"A: Emit KVEvents"-->D
    D--"B: Update Index"-->C

```

**Read Path:**
  * 1: **Scoring Request** : A scheduler asks the **KVCache Indexer** to score a set of pods for a given prompt
  * 2: **Index Query** : The indexer calculates the necessary KV-block keys from the prompt and queries the **KV-Block Index** to see which pods have those blocks
  * 3: **Return Scores** : The indexer returns a map of pods and their corresponding KV-cache-hit scores to the scheduler


**Write Path:**
  * A: **Event Ingestion** : As vLLM pods create or evict KV-blocks, they emit `KVEvents` containing metadata about these changes
  * B: **Index Update** : The **Event Subscriber** consumes these events and updates the **KV-Block Index** in near-real-time


> For a more detailed breakdown, please see the high-level [Architecture](https://github.com/llm-d/llm-d-kv-cache-manager/blob/v0.4.0/docs/architecture.md) and the [Configuration](https://github.com/llm-d/llm-d-kv-cache-manager/blob/v0.4.0/docs/configuration.md) docs.
##### Examples
  * [**KVCache Indexer**](https://github.com/llm-d/llm-d-kv-cache-manager/blob/v0.4.0/examples/kv_cache_index/README.md): A reference implementation showing how to run and use the `kvcache.Indexer` module
  * [**KVCache Aware Scorer**](https://github.com/llm-d/llm-d-kv-cache-manager/blob/v0.4.0/examples/kv_cache_aware_scorer/README.md): A reference implementation of how to integrate the `kvcache.Indexer` into a scheduler like the `llm-d-inference-scheduler`
  * [**KV-Events**](https://github.com/llm-d/llm-d-kv-cache-manager/blob/v0.4.0/examples/kv_events/README.md): Demonstrates how the KV-Cache Manager handles KV-Events through both an offline example with a dummy ZMQ publisher and an online example using a vLLM Helm chart.
  * [**Valkey Backend**](https://github.com/llm-d/llm-d-kv-cache-manager/blob/v0.4.0/examples/valkey_example/README.md): Shows how to configure and use Valkey as the backend for KV-block indexing, including RDMA support for high-performance scenarios.


Expand ▾ Collapse ▴
##  Directories [¶](https://pkg.go.dev/github.com/llm-d/llm-d-kv-cache-manager#section-directories "Go to Directories")
Show internal  Expand all  
| Path  | Synopsis  |  
| --- | --- |  
|  examples  |  
|   |  
|  [kv_cache_index](https://pkg.go.dev/github.com/llm-d/llm-d-kv-cache-manager@v0.4.0/examples/kv_cache_index) command  |  
|  [kv_cache_index_service/client](https://pkg.go.dev/github.com/llm-d/llm-d-kv-cache-manager@v0.4.0/examples/kv_cache_index_service/client) command  |  
|  [kv_cache_index_service/server](https://pkg.go.dev/github.com/llm-d/llm-d-kv-cache-manager@v0.4.0/examples/kv_cache_index_service/server) command  |  
|  [kv_events/offline](https://pkg.go.dev/github.com/llm-d/llm-d-kv-cache-manager@v0.4.0/examples/kv_events/offline) command  |  
|  [kv_events/online](https://pkg.go.dev/github.com/llm-d/llm-d-kv-cache-manager@v0.4.0/examples/kv_events/online) command  |  
|  [valkey_example](https://pkg.go.dev/github.com/llm-d/llm-d-kv-cache-manager@v0.4.0/examples/valkey_example) command  |  
|   |  
|   |  
|  Package kvevents contains the implementation of the KV-events processing system.  | Package kvevents contains the implementation of the KV-events processing system.  |  
|   |  
|  [preprocessing/chat_completions](https://pkg.go.dev/github.com/llm-d/llm-d-kv-cache-manager@v0.4.0/pkg/preprocessing/chat_completions)  |  
|   |  
|   |  
|   |  
|   |  
Click to show internal directories. 
Click to hide internal directories. 
go.dev uses cookies from Google to deliver and enhance the quality of its services and to analyze traffic. [Learn more.](https://policies.google.com/technologies/cookies)
Okay
