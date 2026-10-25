---
title: Request routing — Ray 2.58.0
id: request-routing-ray-2580
tags:
- llm-d
- kv-cache-routing
- load-balancing
- comparison-system
- llmd-kv-cache-routing-c665d5
created: '2026-09-14T11:59:17.631697Z'
updated: '2026-09-15T04:55:24.117167Z'
source: https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html
source_domain: docs.ray.io
fetched_at: '2026-09-14T11:59:17.610900Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'Ray Serve LLM architecture doc confirming the default request router (used
  when prefix-aware routing is not configured) is plain Power-of-Two-Choices -- randomly
  sample two replicas, route to the one with fewer ongoing requests -- with PrefixCacheAffinityRouter
  as an opt-in alternative that explicitly falls back to Power-of-Two-Choices whenever
  replica load is imbalanced (queue-length difference exceeds threshold). This is
  comparison context confirming that even Ray''s cache-aware router treats load-balance-not-cache-locality
  as the default/fallback behavior, i.e. prefix affinity is deliberately subordinate
  to load balancing rather than the reverse. Also documents two architectural patterns
  for building custom LLM-aware routers: a centralized singleton metric-store actor
  (consistent cluster-wide state, but a coordination bottleneck) versus decentralized
  per-router state.'
---

*Suggested by [[prefix-aware-routing-ray-2580]] — linked architecture doc explaining Ray's request routing policies referenced in the prefix-aware-routing user guide*

[Skip to main content](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html#main-content)
Back to top
  * System Settings
  * Light


latest
[latest](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html)[master](https://docs.ray.io/en/master/serve/llm/architecture/routing-policies.html)[releases-2.58.0](https://docs.ray.io/en/releases-2.58.0/serve/llm/architecture/routing-policies.html)[releases-2.57.0](https://docs.ray.io/en/releases-2.57.0/serve/llm/architecture/routing-policies.html)[releases-2.56.1](https://docs.ray.io/en/releases-2.56.1/serve/llm/architecture/routing-policies.html)[releases-2.56.0](https://docs.ray.io/en/releases-2.56.0/serve/llm/architecture/routing-policies.html)[releases-2.55.1](https://docs.ray.io/en/releases-2.55.1/serve/llm/architecture/routing-policies.html)[releases-2.55.0](https://docs.ray.io/en/releases-2.55.0/serve/llm/architecture/routing-policies.html)[releases-2.54.1](https://docs.ray.io/en/releases-2.54.1/serve/llm/architecture/routing-policies.html)[releases-2.54.0](https://docs.ray.io/en/releases-2.54.0/serve/llm/architecture/routing-policies.html)[releases-2.53.0](https://docs.ray.io/en/releases-2.53.0/serve/llm/architecture/routing-policies.html)[releases-2.52.1](https://docs.ray.io/en/releases-2.52.1/serve/llm/architecture/routing-policies.html)[releases-2.52.0](https://docs.ray.io/en/releases-2.52.0/serve/llm/architecture/routing-policies.html)[releases-2.51.2](https://docs.ray.io/en/releases-2.51.2/serve/llm/architecture/routing-policies.html)[releases-2.51.1](https://docs.ray.io/en/releases-2.51.1/serve/llm/architecture/routing-policies.html)[releases-2.51.0](https://docs.ray.io/en/releases-2.51.0/serve/llm/architecture/routing-policies.html)[releases-2.50.1](https://docs.ray.io/en/releases-2.50.1/serve/llm/architecture/routing-policies.html)[releases-2.50.0](https://docs.ray.io/en/releases-2.50.0/serve/llm/architecture/routing-policies.html)[releases-2.49.2](https://docs.ray.io/en/releases-2.49.2/serve/llm/architecture/routing-policies.html)[releases-2.49.1](https://docs.ray.io/en/releases-2.49.1/serve/llm/architecture/routing-policies.html)[releases-2.49.0](https://docs.ray.io/en/releases-2.49.0/serve/llm/architecture/routing-policies.html)[releases-2.48.0](https://docs.ray.io/en/releases-2.48.0/serve/llm/architecture/routing-policies.html)[releases-2.47.1](https://docs.ray.io/en/releases-2.47.1/serve/llm/architecture/routing-policies.html)[releases-2.47.0](https://docs.ray.io/en/releases-2.47.0/serve/llm/architecture/routing-policies.html)[releases-2.46.0](https://docs.ray.io/en/releases-2.46.0/serve/llm/architecture/routing-policies.html)[releases-2.45.0](https://docs.ray.io/en/releases-2.45.0/serve/llm/architecture/routing-policies.html)[releases-2.44.1](https://docs.ray.io/en/releases-2.44.1/serve/llm/architecture/routing-policies.html)[releases-2.44.0](https://docs.ray.io/en/releases-2.44.0/serve/llm/architecture/routing-policies.html)[releases-2.43.0](https://docs.ray.io/en/releases-2.43.0/serve/llm/architecture/routing-policies.html)[releases-2.42.1](https://docs.ray.io/en/releases-2.42.1/serve/llm/architecture/routing-policies.html)[releases-2.42.0](https://docs.ray.io/en/releases-2.42.0/serve/llm/architecture/routing-policies.html)[releases-2.41.0](https://docs.ray.io/en/releases-2.41.0/serve/llm/architecture/routing-policies.html)[releases-2.40.0](https://docs.ray.io/en/releases-2.40.0/serve/llm/architecture/routing-policies.html)[releases-2.39.0](https://docs.ray.io/en/releases-2.39.0/serve/llm/architecture/routing-policies.html)[releases-2.38.0](https://docs.ray.io/en/releases-2.38.0/serve/llm/architecture/routing-policies.html)[releases-2.37.0](https://docs.ray.io/en/releases-2.37.0/serve/llm/architecture/routing-policies.html)[releases-2.36.1](https://docs.ray.io/en/releases-2.36.1/serve/llm/architecture/routing-policies.html)[releases-2.36.0](https://docs.ray.io/en/releases-2.36.0/serve/llm/architecture/routing-policies.html)[releases-2.35.0](https://docs.ray.io/en/releases-2.35.0/serve/llm/architecture/routing-policies.html)[releases-2.34.0](https://docs.ray.io/en/releases-2.34.0/serve/llm/architecture/routing-policies.html)[releases-2.33.0](https://docs.ray.io/en/releases-2.33.0/serve/llm/architecture/routing-policies.html)[releases-2.32.0](https://docs.ray.io/en/releases-2.32.0/serve/llm/architecture/routing-policies.html)[releases-2.31.0](https://docs.ray.io/en/releases-2.31.0/serve/llm/architecture/routing-policies.html)[releases-2.30.0](https://docs.ray.io/en/releases-2.30.0/serve/llm/architecture/routing-policies.html)[releases-2.24.0](https://docs.ray.io/en/releases-2.24.0/serve/llm/architecture/routing-policies.html)[releases-2.23.0](https://docs.ray.io/en/releases-2.23.0/serve/llm/architecture/routing-policies.html)[releases-2.22.0](https://docs.ray.io/en/releases-2.22.0/serve/llm/architecture/routing-policies.html)[releases-2.21.0](https://docs.ray.io/en/releases-2.21.0/serve/llm/architecture/routing-policies.html)[releases-2.20.0](https://docs.ray.io/en/releases-2.20.0/serve/llm/architecture/routing-policies.html)[releases-2.12.0](https://docs.ray.io/en/releases-2.12.0/serve/llm/architecture/routing-policies.html)[releases-2.11.0](https://docs.ray.io/en/releases-2.11.0/serve/llm/architecture/routing-policies.html)[releases-2.10.0](https://docs.ray.io/en/releases-2.10.0/serve/llm/architecture/routing-policies.html)[releases-2.9.3](https://docs.ray.io/en/releases-2.9.3/serve/llm/architecture/routing-policies.html)[releases-2.9.2](https://docs.ray.io/en/releases-2.9.2/serve/llm/architecture/routing-policies.html)[releases-2.9.1](https://docs.ray.io/en/releases-2.9.1/serve/llm/architecture/routing-policies.html)[releases-2.9.0](https://docs.ray.io/en/releases-2.9.0/serve/llm/architecture/routing-policies.html)[releases-2.8.1](https://docs.ray.io/en/releases-2.8.1/serve/llm/architecture/routing-policies.html)[releases-2.8.0](https://docs.ray.io/en/releases-2.8.0/serve/llm/architecture/routing-policies.html)[releases-2.7.1](https://docs.ray.io/en/releases-2.7.1/serve/llm/architecture/routing-policies.html)[releases-2.7.0](https://docs.ray.io/en/releases-2.7.0/serve/llm/architecture/routing-policies.html)[releases-2.6.3](https://docs.ray.io/en/releases-2.6.3/serve/llm/architecture/routing-policies.html)[releases-2.6.2](https://docs.ray.io/en/releases-2.6.2/serve/llm/architecture/routing-policies.html)[releases-2.6.1](https://docs.ray.io/en/releases-2.6.1/serve/llm/architecture/routing-policies.html)[releases-2.6.0](https://docs.ray.io/en/releases-2.6.0/serve/llm/architecture/routing-policies.html)[releases-2.5.1](https://docs.ray.io/en/releases-2.5.1/serve/llm/architecture/routing-policies.html)[releases-2.5.0](https://docs.ray.io/en/releases-2.5.0/serve/llm/architecture/routing-policies.html)[releases-2.4.0](https://docs.ray.io/en/releases-2.4.0/serve/llm/architecture/routing-policies.html)[releases-2.3.1](https://docs.ray.io/en/releases-2.3.1/serve/llm/architecture/routing-policies.html)[releases-2.3.0](https://docs.ray.io/en/releases-2.3.0/serve/llm/architecture/routing-policies.html)[releases-2.2.0](https://docs.ray.io/en/releases-2.2.0/serve/llm/architecture/routing-policies.html)[releases-2.1.0](https://docs.ray.io/en/releases-2.1.0/serve/llm/architecture/routing-policies.html)[releases-2.0.1](https://docs.ray.io/en/releases-2.0.1/serve/llm/architecture/routing-policies.html)[releases-2.0.0](https://docs.ray.io/en/releases-2.0.0/serve/llm/architecture/routing-policies.html)[releases-1.13.1](https://docs.ray.io/en/releases-1.13.1/serve/llm/architecture/routing-policies.html)[releases-1.13.0](https://docs.ray.io/en/releases-1.13.0/serve/llm/architecture/routing-policies.html)[releases-1.12.1](https://docs.ray.io/en/releases-1.12.1/serve/llm/architecture/routing-policies.html)[releases-1.12.0](https://docs.ray.io/en/releases-1.12.0/serve/llm/architecture/routing-policies.html)[releases-1.11.1](https://docs.ray.io/en/releases-1.11.1/serve/llm/architecture/routing-policies.html)[releases-1.11.0](https://docs.ray.io/en/releases-1.11.0/serve/llm/architecture/routing-policies.html)
[ Try Managed Ray ](https://console.anyscale.com/register/ha?render_flow=ray&utm_source=ray_docs&utm_medium=docs&utm_campaign=navbar)
# Request routing[#](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html#request-routing "Link to this heading")
Ray Serve LLM provides customizable request routing to optimize request distribution across replicas for different workload patterns. Request routing operates at the **replica selection level** , distinct from ingress-level model routing.
## Routing versus ingress[#](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html#routing-versus-ingress "Link to this heading")
You need to distinguish between two levels of routing:
**Ingress routing** (model-level):
  * Maps `model_id` to deployment
  * Example: `OpenAiIngress` gets `/v1/chat/completions` with `model="gptoss"` and maps it to the `gptoss` deployment.


**Request routing** (replica-level):
  * Chooses which replica to send the request to
  * Example: The `gptoss` deployment handle inside the `OpenAiIngress` replica decides which replica of the deployment (1, 2, or 3) to send the request to.


This document focuses on **request routing** (replica selection).

```
HTTP Request → Ingress (model routing) → Request Router (replica selection) → Server Replica

```
Copy to clipboard
Note
Direct streaming uses these same routing policies and the same `request_router_config`, so nothing in this document changes. The only difference is where replica selection runs: at the HTTP ingress instead of inside `OpenAiIngress`. See the [direct streaming guide](https://docs.ray.io/en/latest/serve/llm/user-guides/direct-streaming.html#direct-streaming-guide) for details and caveats.
## Request routing architecture[#](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html#request-routing-architecture "Link to this heading")
Ray Serve LLM request routing operates at the deployment handle level:

```
┌──────────────┐
│   Ingress    │
│  (Replica 1) │
└──────┬───────┘
       │
       │ handle.remote(request)
       ↓
┌──────────────────┐
│ Deployment Handle│
│   + Router       │  ← Request routing happens here
└──────┬───────────┘
       │
       │ Chooses replica based on policy
       ↓
   ┌───┴────┬────────┬────────┐
   │        │        │        │
┌──▼──┐  ┌──▼──┐  ┌──▼──┐  ┌──▼──┐
│ LLM │  │ LLM │  │ LLM │  │ LLM │
│  1  │  │  2  │  │  3  │  │  4  │
└─────┘  └─────┘  └─────┘  └─────┘

```
Copy to clipboard
## Available routing policies[#](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html#available-routing-policies "Link to this heading")
Ray Serve LLM provides multiple request routing policies to optimize for different workload patterns:
### Default routing: Power of Two Choices[#](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html#default-routing-power-of-two-choices "Link to this heading")
The default router uses the Power of Two Choices algorithm to:
  1. Randomly sample two replicas.
  2. Route to the replica with fewer ongoing requests.


This provides good load balancing with minimal coordination overhead.
### Prefix-aware routing[#](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html#prefix-aware-routing "Link to this heading")
The `PrefixCacheAffinityRouter` optimizes for workloads with shared prefixes by routing requests with similar prefixes to the same replicas. This improves KV cache hit rates in vLLM’s Automatic Prefix Caching (APC).
The routing strategy:
  1. **Check load balance** : If replicas are balanced (queue difference < threshold), use prefix matching.
  2. **High match rate (≥10%)** : Route to replicas with highest prefix match.
  3. **Low match rate ( <10%)**: Route to replicas with lowest cache utilization.
  4. **Fallback** : Use Power of Two Choices when load is imbalanced.


For more details, see [Prefix-aware routing](https://docs.ray.io/en/latest/serve/llm/user-guides/prefix-aware-routing.html#prefix-aware-routing-guide).
## Design patterns for custom routing policies[#](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html#design-patterns-for-custom-routing-policies "Link to this heading")
Customizing request routers is a feature in Ray Serve’s native APIs that you can define per deployment. For each deployment, you can customize the routing logic that executes every time you call `.remote()` on the deployment handle from a caller. Because deployment handles are globally available objects across the cluster, you can call them from any actor or task in the Ray cluster. For more details on this API, see [Use Custom Algorithm for Request Routing](https://docs.ray.io/en/latest/serve/advanced-guides/custom-request-router.html#custom-request-router-guide).
As a result, the same routing logic runs even when you have multiple handles. The default request router in Ray Serve is Power of Two Choices, which balances load equalization and prioritizes locality routing. However, you can customize this to use LLM-specific metrics.
Ray Serve LLM includes prefix-aware routing in the framework. There are two common architectural patterns for customizing request routers. There are clear trade-offs between them, so choose the suitable one and balance simplicity with performance:
### Pattern 1: Centralized singleton metric store[#](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html#pattern-1-centralized-singleton-metric-store "Link to this heading")
In this approach, you keep a centralized metric store (for example, a singleton actor) for tracking routing-related information. The request router logic physically runs on the process that owns the deployment handle, so there can be many such processes. Each one can query the singleton actor, creating a multi-tenant actor that provides a consistent view of the cluster state to the request routers.
The single actor can provide atomic thread-safe operations such as `get()` for querying the global state and `set()` for updating the global state, which the router can use during `choose_replicas()` and `on_request_routed()`.

```
┌─────────┐     ┌─────────┐     ┌─────────┐
│ Ingress │────►│ Metric  │◄────│ Ingress │
│    1    │     │  Store  │     │    2    │
└────┬────┘     └─────────┘     └────┬────┘
     │                               │
     └────────────────┬──────────────┘
                      │
           ┌──────────┴──────────┐
           │                     │
      ┌────▼────┐           ┌────▼────┐
      │  LLM    │           │  LLM    │
      │ Server  │           │ Server  │
      └─────────┘           └─────────┘

```
Copy to clipboard
Centralized metric store pattern for custom routing[#](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html#centralized-metric-store-pattern "Link to this image")
**Pros:**
  * Simple implementation - no need to modify deployment logic for recording replica statistics.
  * Request metrics are immediately available.
  * Strong consistency guarantees.


**Cons:**
  * A single actor can become a bottleneck in high-throughput applications where TTFT is impacted by the RPC call (~1000s of requests/s).
  * Requires an additional network hop for every routing decision.


### Pattern 2: Metrics broadcasted from Serve controller[#](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html#pattern-2-metrics-broadcasted-from-serve-controller "Link to this heading")
In this approach, the Serve controller polls each replica for local statistics and then broadcasts them to all request routers on their deployment handles. The request router can then use this globally broadcasted information to pick the right replica. After a request reaches the replica, the replica updates its local statistics so it can send them back to the Serve controller when the controller polls it next time.

```
          ┌──────────────┐
          │    Serve     │
          │  Controller  │
          └──────┬───────┘
                 │ (broadcast)
       ┌─────────┴─────────┐
       │                   │
  ┌────▼────┐         ┌────▼────┐
  │ Ingress │         │ Ingress │
  │  +Cache │         │  +Cache │
  └────┬────┘         └────┬────┘
       │                   │
       └────────┬──────────┘
                │
         ┌──────┴──────┐
         │             │
    ┌────▼────┐   ┌────▼────┐
    │  LLM    │   │  LLM    │
    │ Server  │   │ Server  │
    └─────────┘   └─────────┘

```
Copy to clipboard
Broadcast metrics pattern for custom routing[#](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html#broadcast-metrics-pattern "Link to this image")
**Pros:**
  * Scalable to higher throughput.
  * No additional RPC overhead per routing decision.
  * Distributed routing decision making.


**Cons:**
  * Time lag between the request router’s view of statistics and the ground truth state of the replicas.
  * Eventual consistency - routers may base decisions on slightly stale data.
  * More complex implementation requiring coordination with the Serve controller.
  * **Use Pattern 1 (Centralized store)** when you need strong consistency, have moderate throughput requirements, or want simpler implementation.
  * **Use Pattern 2 (Broadcast metrics)** when you need very high throughput, can tolerate eventual consistency, or want to minimize per-request overhead.


## Custom routing policies[#](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html#custom-routing-policies "Link to this heading")
You can implement custom routing policies by extending Ray Serve’s [`RequestRouter`](https://docs.ray.io/en/latest/serve/api/doc/ray.serve.request_router.RequestRouter.html) base class. For detailed examples and step-by-step guides on implementing custom routers, see [Use Custom Algorithm for Request Routing](https://docs.ray.io/en/latest/serve/advanced-guides/custom-request-router.html#custom-request-router-guide).
Key methods to implement:
  * [`choose_replicas()`](https://docs.ray.io/en/latest/serve/api/doc/ray.serve.request_router.RequestRouter.choose_replicas.html): Select which replicas should handle a request.
  * [`on_request_routed()`](https://docs.ray.io/en/latest/serve/api/doc/ray.serve.request_router.RequestRouter.on_request_routed.html): Update the router state after a request is routed.
  * [`on_replica_actor_died()`](https://docs.ray.io/en/latest/serve/api/doc/ray.serve.request_router.RequestRouter.on_replica_actor_died.html): Clean up the state when a replica dies.


### Utility mixins[#](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html#utility-mixins "Link to this heading")
Ray Serve provides mixin classes that add common functionality to routers. See the [Use Custom Algorithm for Request Routing](https://docs.ray.io/en/latest/serve/advanced-guides/custom-request-router.html#custom-request-router-guide) for examples:
  * [`LocalityMixin`](https://docs.ray.io/en/latest/serve/api/doc/ray.serve.request_router.LocalityMixin.html): Prefers replicas on the same node to reduce network latency.
  * [`MultiplexMixin`](https://docs.ray.io/en/latest/serve/api/doc/ray.serve.request_router.MultiplexMixin.html): Tracks which models are loaded on each replica for LoRA deployments.
  * [`FIFOMixin`](https://docs.ray.io/en/latest/serve/api/doc/ray.serve.request_router.FIFOMixin.html): Ensures FIFO ordering of requests.


### Router lifecycle[#](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html#router-lifecycle "Link to this heading")
The typical lifecycle of request routers includes the following stages:
  1. **Initialization** : Router created with list of replicas.
  2. **Request routing** : `choose_replicas()` called for each request.
  3. **Callback** : `on_request_routed()` called after successful routing.
  4. **Replica failure** : `on_replica_actor_died()` called when replica dies.
  5. **Cleanup** : Router cleaned up when deployment is deleted.


#### Async operations[#](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html#async-operations "Link to this heading")
Routers should use async operations for best performance. The following example demonstrates the recommended pattern:

```
# Recommended pattern: Async operation
async def choose_replicas(self, ...):
    state = await self.state_actor.get.remote()
    return self._select(state)

# Not recommended pattern: Blocking operation
async def choose_replicas(self, ...):
    state = ray.get(self.state_actor.get.remote())  # Blocks!
    return self._select(state)

```
Copy to clipboard
#### State management[#](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html#state-management "Link to this heading")
For routers with state, use appropriate synchronization. The following example shows the recommended pattern:

```
class StatefulRouter(RequestRouter):
    def __init__(self):
        self.lock = asyncio.Lock()  # For async code
        self.state = {}
    
    async def choose_replicas(self, ...):
        async with self.lock:  # Protect shared state
            # Update state
            self.state[...] = ...
            return [...]

```
Copy to clipboard
## See also[#](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html#see-also "Link to this heading")
  * [Prefix-aware routing](https://docs.ray.io/en/latest/serve/llm/user-guides/prefix-aware-routing.html#prefix-aware-routing-guide) - user guide for deploying prefix-aware routing
  * [Use Custom Algorithm for Request Routing](https://docs.ray.io/en/latest/serve/advanced-guides/custom-request-router.html#custom-request-router-guide) - Ray Serve guide for implementing custom routers
  * [`RequestRouter` API Reference](https://docs.ray.io/en/latest/serve/api/doc/ray.serve.request_router.RequestRouter.html) - complete API documentation


On this page 
[ Edit on GitHub ](https://github.com/ray-project/ray/edit/master/doc/source/serve/llm/architecture/routing-policies.md)
