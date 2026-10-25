---
title: '[RFC]: KV-aware routing for PD disaggregated serving via sourced KV events
  · Issue #9905 · vllm-project/vllm-ascend'
id: rfc-kv-aware-routing-for-pd-disaggregated-serving-via-sourced-kv-events-issue-99
tags:
- llmd-router
- kv-event-sourcing
- non-k8s-backend
- ground-truth-rfc
- llmd-router-nonk8s-backends-54b774
- llmd-kv-cache-routing-c665d5
created: '2026-09-11T19:46:17.548346Z'
updated: '2026-09-15T04:55:23.789993Z'
source: https://github.com/vllm-project/vllm-ascend/issues/9905
source_domain: github.com
fetched_at: '2026-09-11T19:46:17.527654Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Merged-community RFC (GitHub issue #9905, opened June 3 2026, vllm-project/vllm-ascend,
  ground-truth/code tier) that is directly load-bearing for building a non-k8s-pod-backend
  router: it proposes an optional ''KV event collector'' component that runs NEAR
  the router (not inside vLLM, not inside the EPP) and republishes a ''sourced'' KV
  event stream, decoupling KV-cache visibility from the k8s-pod identity model entirely.
  Key primitives reused unchanged from vLLM: KVCacheEvent/BlockStored/BlockRemoved/AllBlocksCleared
  (vllm/distributed/kv_events.py), KVEventBatch (msgspec batch with timestamp+events+DP
  rank), ZmqEventPublisher (PUB socket with optional ROUTER replay), KVEventsConfig.
  Critically, the collector is configured with EXPLICIT upstream sources rather than
  inferring source identity from ZMQ messages (ZMQ PUB/SUB carries no sender endpoint)
  -- UpstreamKVEventSource{instance_id: str, endpoint: str, topic: str} where instance_id
  can be ''a Decode instance ID, PD group ID, or service address,'' NOT necessarily
  a k8s pod. This is the exact abstraction point a non-k8s router should adopt: replace
  ''pod'' with an arbitrary ''source_instance'' string. New wire type SourcedKVCacheEvent{event,
  source_instance} plus SourcedKVEventBatch preserves the existing vLLM event protocol
  (additive, opt-in decode). Router-side state needed: two indexes block_hash->set[source_instance]
  and source_instance->set[block_hash], plus an optional token-prefix trie built from
  BlockStored.token_ids for prefix-aware scoring -- explicitly warns that vLLM block
  hashes include parent hash + process-local seed, so ''an external router cannot
  reliably reproduce hashes,'' meaning any router MUST consume token_ids/block_hashes
  as reported rather than recompute them. Notes the RFC explicitly cross-references
  kvcache-ai/Mooncake#977 (Mooncake-Conductor global scheduler) as a potential integration
  point for unified KV-cache state maintenance.'
utility_score: 9.0
---

[Skip to content](https://github.com/vllm-project/vllm-ascend/issues/9905#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/vllm-project/vllm-ascend/issues/9905) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/vllm-project/vllm-ascend/issues/9905) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/vllm-project/vllm-ascend/issues/9905) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/vllm-project/vllm-ascend/issues/9905).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fvllm-project%2Fvllm-ascend) You must be signed in to change notification settings
  * [ Fork 2.2k ](https://github.com/login?return_to=%2Fvllm-project%2Fvllm-ascend)
  * [ Star  2.8k ](https://github.com/login?return_to=%2Fvllm-project%2Fvllm-ascend)


#  [RFC]: KV-aware routing for PD disaggregated serving via sourced KV events
Copy link
Copy link
[[RFC]: KV-aware routing for PD disaggregated serving via sourced KV events](https://github.com/vllm-project/vllm-ascend/issues/9905#top)#9905
Copy link
Labels
[Request For Comments](https://github.com/vllm-project/vllm-ascend/issues?q=state%3Aopen%20label%3A%22RFC%22)Request For Comments
## Description
opened [on Jun 3, 2026](https://github.com/vllm-project/vllm-ascend/issues/9905#issue-4575991618)
Last edited by lio1226
Contributor
Issue body actions
### Motivation.
In prefill-decode (PD) disaggregated serving, requests usually pass through an external gateway or router before they reach a specific Decode instance or PD group. vLLM can already publish KV cache events for a single engine, which lets an external component observe prefix-cache state and route similar prompts to an instance with cached blocks.
In PD deployments, this signal is often hidden behind a router boundary. The gateway may know the router endpoint, but not the per-instance KV cache distribution underneath it. As a result, requests may be sent to instances that do not hold the relevant prefix blocks, causing repeated prefill work or unnecessary cross-instance KV transfer. Even when a KV connector can transfer blocks across machines, using a local cached prefix is still preferable when a good routing choice is available.
The goal of this RFC is to expose a per-instance KV cache view from the router side, so the gateway or router can choose the Decode instance or PD group with the highest prefix-cache hit rate.
### Proposed Change.
Add an optional KV event collector that runs near the router. The collector subscribes to the existing vLLM KV event streams from the instances managed by that router, annotates each event with a stable source identifier, batches the events, and republishes a sourced event stream to the gateway or router.
The upstream vLLM instances continue to use the existing configuration:

```
vllm serve MODEL \
  --kv-events-config 
    "enable_kv_cache_events": true,
    "publisher": "zmq",
    "endpoint": "tcp://*:5557",
    "topic": ""

```

### Existing vLLM primitives
This design reuses the current KV event infrastructure:  
| Component  | Location  | Role  |  
| --- | --- | --- |  
| `KVCacheEvent`  | `vllm/distributed/kv_events.py`  | Base type for `BlockStored`, `BlockRemoved`, and `AllBlocksCleared`  |  
| `KVEventBatch`  | `vllm/distributed/kv_events.py`  | msgspec batch containing timestamp, events, and optional DP rank  |  
| `ZmqEventPublisher`  | `vllm/distributed/kv_events.py`  | PUB socket with optional ROUTER replay  |  
| `KVEventsConfig`  | `vllm/config/kv_events.py`  | CLI/API configuration for event publishing  |  
The collector should not require changes to the inference hot path. It consumes events already produced by `BlockPool` and published by the scheduler.
### Source attribution
The collector should not infer the source instance from a received ZMQ message. ZMQ PUB/SUB messages do not include the sender endpoint. Instead, the collector should be configured with explicit upstream sources and use one SUB socket per source:

```
@dataclass
class UpstreamKVEventSource:
    instance_id: str
    endpoint: str
    topic: str = ""
```

`instance_id` should be a stable routing target, such as a Decode instance ID, PD group ID, or service address. An IP address can be used as a default in simple deployments, but it should not be required.
### Sourced event envelope
The collector should publish a new envelope type instead of mutating the existing `BlockStored`, `BlockRemoved`, and `AllBlocksCleared` event structs. This keeps the existing event protocol intact and makes the new stream explicit to consumers.

```
class SourcedKVCacheEvent(
    msgspec.Struct,
    array_like=True,
    omit_defaults=True,
    gc=False,
):
    event: BlockStored | BlockRemoved | AllBlocksCleared
    source_instance: str


class SourcedKVEventBatch(EventBatch):
    events: list[SourcedKVCacheEvent]
```

The collector can still reuse `ZmqEventPublisher`, because the publisher accepts `EventBatch` payloads and serializes them with msgspec.
### Batching and ordering
The collector batches events using a small time window, for example 100-200 ms. Within a single source, the collector must preserve event order. Across sources, global ordering is not required.
The collector should not deduplicate events across instances. A deduplicated union stream can answer whether a block exists anywhere, but it loses the information needed for routing: which instance owns the block. The downstream router needs the full `block_hash -> set[source_instance]` view.
`AllBlocksCleared` may be published immediately or included in the current batch, as long as ordering is preserved for the source that emitted it.
### Router-side state
The consumer maintains at least two indexes:

```
block_hash -> set[source_instance]
source_instance -> set[block_hash]

```

The second index is needed to handle `AllBlocksCleared` and instance removal efficiently.
For prefix-aware routing by incoming tokens, the consumer can also build a token prefix trie from `BlockStored.token_ids`:

```
token prefix block -> block_hash -> set[source_instance]

```

When a request arrives, the router tokenizes the prompt, walks the trie by full-size prefix blocks, scores instances by the number of matched cached blocks, and picks the highest-scoring instance. If no match is found, the router falls back to the existing policy, such as round-robin or least-connections.
The router should consume the `token_ids` and `block_hashes` reported by `BlockStored` rather than recomputing block hashes. vLLM block hashes include parent hash and extra keys, and the initial hash seed may be process-local, so an external router cannot reliably reproduce hashes in all deployments.
### Configuration sketch

```
@dataclass
class KVAwareRouterCollectorConfig:
    upstream_sources: list[UpstreamKVEventSource]

    window_ms: int = 200

    publish_endpoint: str = "tcp://*:5560"
    publish_topic: str = "kv-events-sourced"
    replay_endpoint: str | None = None
    buffer_steps: int = 10_000
    hwm: int = 100_000
    max_queue_size: int = 100_000
```

Example collector invocation:

```
python examples/features/kv_events/kv_events_router_collector.py \
  --upstream inst-a=tcp://inst-a:5557 \
  --upstream inst-b=tcp://inst-b:5557 \
  --publish-endpoint tcp://router:5560 \
  --publish-topic kv-events-sourced \
  --window-ms 200
```

## Compatibility and Impact
This proposal is intended to be additive:
  * Existing `KVEventBatch` publishers and subscribers continue to work.
  * Existing vLLM serving processes only need to enable `--kv-events-config`.
  * The sourced event stream uses a new msgspec type, so consumers opt in by decoding `SourcedKVEventBatch`.
  * The collector can initially live under `examples/features/kv_events/` or a router integration package before being promoted to a stable component.


The main operational cost is an additional event consumer and publisher near the router. The event rate is bounded by KV block store/remove activity and can be controlled with the existing ZMQ HWM, queue size, and batching window.
## Alternatives Considered
### Let the router subscribe to every vLLM instance directly
This avoids an extra process, but it mixes ZMQ subscription, replay handling, source attribution, and routing policy in the router itself. A collector keeps event normalization separate from routing decisions and can be tested or scaled independently.
### Add `source_instance` to every vLLM KV event
This gives the cleanest source attribution, but it changes the core event schema and requires every vLLM process to know the externally meaningful routing identifier. A collector-side envelope avoids changing the existing event protocol.
### Publish a deduplicated union of KV events
A union stream is useful for answering whether a block exists anywhere, but it does not tell the router where to send the request. KV-aware routing requires per-instance ownership, so cross-instance deduplication is not appropriate for the primary stream.
### Reuse `KVEventAggregator`
The current `KVEventAggregator` counts equivalent events across workers and can return common or all events. That is useful for worker-level aggregation, but it does not preserve the source ownership needed for routing. This RFC proposes a source-preserving collector instead.
## Open Questions
  * Mooncake is currently discussing and implementing Mooncake-Conductor as a global scheduler for KV-cache-centric disaggregated serving ([[RFC]: Mooncake-Conductor: Design and Implementation of a Global Scheduler Module for KV-Cache-Centric Disaggregated Architecture kvcache-ai/Mooncake#977](https://github.com/kvcache-ai/Mooncake/issues/977)). Can this RFC rely on or integrate with that component for unified KV cache state maintenance, and focus the follow-up work on the vLLM event/adaptor side?


### Feedback Period.
Suggested feedback period: at least one week after opening the RFC issue.
### CC List.
Router, disaggregated serving, and KV connector maintainers.
### Any Other Things.
_No response_
## Activity
[lio1226](https://github.com/lio1226)
added 
[Request For Comments](https://github.com/vllm-project/vllm-ascend/issues?q=state%3Aopen%20label%3A%22RFC%22)Request For Comments
[on Jun 2, 2026](https://github.com/vllm-project/vllm-ascend/issues/9905#event-26263371080)
[Sign up for free](https://github.com/signup?return_to=https://github.com/vllm-project/vllm-ascend/issues/9905)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/vllm-project/vllm-ascend/issues/9905)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
[Request For Comments](https://github.com/vllm-project/vllm-ascend/issues?q=state%3Aopen%20label%3A%22RFC%22)Request For Comments
No type
### Projects
No projects
### Milestone
No milestone
### Relationships
None yet
### Development
No branches or pull requests
### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
