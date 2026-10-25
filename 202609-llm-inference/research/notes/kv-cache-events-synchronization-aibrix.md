---
title: KV Cache Events Synchronization — AIBrix
id: kv-cache-events-synchronization-aibrix
tags:
- kv-cache-routing
- kv-event-sync
- aibrix
- zmq-pubsub
- prefix-cache-routing
- llmd-kv-cache-routing-c665d5
created: '2026-09-14T11:53:59.794788Z'
updated: '2026-10-10T14:52:17.305502Z'
source: https://aibrix.readthedocs.io/latest/features/kv-event-sync.html
source_domain: aibrix.readthedocs.io
fetched_at: '2026-09-14T11:53:59.774762Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'AIBrix (ByteDance''s Kubernetes-native vLLM control plane) documentation
  for KV Cache Events Synchronization, an alternative to llm-d''s KV-event design:
  vLLM instances publish BlockStoredEvent (BlockHashes, TokenIDs, ModelName, LoraID,
  SourcePod, ParentBlockHash) and BlockRemovedEvent messages over a ZMQ pub/sub pattern
  (ports 5557 for events, 5558 for replay) to an in-gateway KV Event Manager, which
  feeds a Sync Prefix Cache Indexer maintaining global prefix cache state for the
  Gateway Router. The feature has a strict hard dependency on a remote tokenizer being
  enabled (AIBRIX_PREFIX_CACHE_USE_REMOTE_TOKENIZER=true) to guarantee identical tokenization
  between gateway and vLLM workers — the system refuses to initialize KV sync without
  it — and requires vLLM >=0.7.0 with KV cache events support plus gateway-plugins
  built with libzmq3-dev (-tags="zmq"). Routing selects pods by prefix-match percentage
  (descending) then running-request count (ascending), picking the first pod satisfying
  running_requests <= mean + STANDARD_DEVIATION_FACTOR * sigma, falling back to the
  globally least-loaded ready pod if no matched pod clears that bar; a separate, centrally-applied
  load-imbalance gate runs ahead of whichever routing strategy is active. Documented
  resource costs: ~1MB/s ZMQ traffic per vLLM pod at high load, ~64 bytes per prefix
  entry in the sync indexer, and <1% CPU overhead per pod.'
utility_score: 12.0
---

[Skip to main content](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#main-content)
Back to top
Search


  * System Settings
  * Light


# KV Cache Events Synchronization
##  Contents 
# KV Cache Events Synchronization[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#kv-cache-events-synchronization "Link to this heading")
## Overview[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#overview "Link to this heading")
KV Cache Event Synchronization is a feature that enables multiple vLLM instances to share key-value cache states through ZMQ-based event publishing. This improves prefix cache hit rates and reduces redundant computation by allowing the AIBrix gateway to make intelligent routing decisions based on real-time cache state.
## Architecture[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#architecture "Link to this heading")
The KV event synchronization system consists of:
  1. **vLLM Instances** : Publish KV cache events via ZMQ pub/sub pattern
  2. **AIBrix Cache** : Manages subscriptions and processes events
  3. **Sync Prefix Cache Indexer** : Maintains global prefix cache state
  4. **Gateway Router** : Uses cache state for intelligent routing decisions


### Event Flow[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#event-flow "Link to this heading")

```
vLLM Pod 1 ─────┐
                 ├─── ZMQ Events ───► KV Event Manager ───► Sync Indexer ───► Gateway Router
vLLM Pod N ─────┘                         (in Cache)

```
Copy to clipboard
The system uses a two-stage initialization:
  1. **Cache Initialization** : Using `InitWithOptions` pattern with `EnableKVSync=true`
  2. **KV Event Manager** : Automatically created when conditions are met


## Requirements[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#requirements "Link to this heading")
  * vLLM version 0.7.0 or later with KV cache events support
  * AIBrix gateway-plugins built with ZMQ support (`-tags="zmq"`)
  * ZMQ library (libzmq3-dev) installed on gateway nodes
  * Remote tokenizer enabled (strict prerequisite)
  * Redis client configured (for production deployments)


Important
KV event sync has a strict dependency on remote tokenizer to ensure consistent tokenization between gateway and vLLM instances. The system will not initialize if remote tokenizer is disabled.
## Configuration[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#configuration "Link to this heading")
### Environment Variables[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#environment-variables "Link to this heading")  
| Variable  | Default  | Description  |  
| --- | --- | --- |  
| `AIBRIX_PREFIX_CACHE_KV_EVENT_SYNC_ENABLED`  | `false`  | Enable KV event synchronization  |  
| `AIBRIX_PREFIX_CACHE_USE_REMOTE_TOKENIZER`  | `false`  | Must be `true` for KV sync  |  
| `AIBRIX_PREFIX_CACHE_REMOTE_TOKENIZER_ENDPOINT`  | vLLM service endpoint  |  
### Pod Labels[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#pod-labels "Link to this heading")  
| Label  | Value  | Description  |  
| --- | --- | --- |  
| `model.aibrix.ai/kv-events-enabled`  | Enable KV events for this pod  |  
| `model.aibrix.ai/lora-id`  | string  | LoRA adapter ID (optional)  |  
### vLLM Configuration[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#vllm-configuration "Link to this heading")
Add these arguments to your vLLM container:

```
args:
---enable-kv-cache-events
---kv-events-publisher=zmq
---kv-events-endpoint=tcp://*:5557
---kv-events-replay-endpoint=tcp://*:5558
---kv-events-buffer-steps=10000

```
Copy to clipboard
Add corresponding ports:

```
ports:
-name:kv-events
containerPort:5557
protocol:TCP
-name:kv-replay
containerPort:5558
protocol:TCP

```
Copy to clipboard
## Deployment[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#deployment "Link to this heading")
### Quick Start[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#quick-start "Link to this heading")
  1. **Enable Remote Tokenizer** (mandatory prerequisite):

```
kubectl set env deployment/aibrix-gateway-plugins -n aibrix-system \
  AIBRIX_PREFIX_CACHE_USE_REMOTE_TOKENIZER=true \
  AIBRIX_PREFIX_CACHE_REMOTE_TOKENIZER_ENDPOINT=http://vllm-service:8000

```
Copy to clipboard
  2. **Enable KV Event Sync** :

```
kubectl set env deployment/aibrix-gateway-plugins -n aibrix-system \
  AIBRIX_PREFIX_CACHE_KV_EVENT_SYNC_ENABLED=true

```
Copy to clipboard
  3. **Deploy vLLM with KV Events** :

```
apiVersion:apps/v1
kind:Deployment
metadata:
name:vllm-model
spec:
template:
metadata:
labels:
model.aibrix.ai/name:"llama-7b"
model.aibrix.ai/kv-events-enabled:"true"
spec:
containers:
-name:vllm
args:
---enable-kv-cache-events
---kv-events-publisher=zmq
---kv-events-endpoint=tcp://*:5557
---kv-events-replay-endpoint=tcp://*:5558

```
Copy to clipboard


### Build Considerations[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#build-considerations "Link to this heading")
AIBrix uses conditional compilation to manage ZMQ dependencies:
**Components requiring ZMQ support:**
  * `gateway-plugins`: Main component for KV event sync
  * `kvcache-watcher`: Optional component for cache monitoring


**Build commands:**

```
# Build with ZMQ support
gobuild-tags="zmq"./cmd/plugins/main.go

# Docker build with ZMQ
makedocker-build-gateway-plugins# Automatically includes ZMQ

```
Copy to clipboard
**Components that do NOT require ZMQ:**
  * `controller-manager`: Uses default build
  * `metadata-service`: Uses default build
  * `runtime`: Python component, no ZMQ needed


## Routing Behavior[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#routing-behavior "Link to this heading")
The KV sync router applies the same prefix-match-with-stddev-filter selection logic as the standard prefix-cache router. It does not apply a load-imbalance gate itself — that concern is handled centrally by the gateway, which applies the load-balance router’s (`algorithms/load_balance.go`) `ApplyLoadImbalanceGate` once ahead of whichever strategy actually routes the request, KV sync included.
**Prefix match with stddev filter**
Pods are matched against the sync indexer and sorted by prefix-match percentage (descending), then by running requests (ascending). The first pod satisfying:

```
running_requests ≤ mean + STANDARD_DEVIATION_FACTOR × σ

```
Copy to clipboard
is selected. If no matched pod meets the threshold, the router falls back to the globally least-loaded ready pod.
Relevant environment variables:  
| Variable  | Default  | Description  |  
| --- | --- | --- |  
| `AIBRIX_PREFIX_CACHE_STANDARD_DEVIATION_FACTOR`  | Controls how aggressively overloaded cache-holding pods are skipped during selection.  |  
## Event Types[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#event-types "Link to this heading")
### BlockStoredEvent[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#blockstoredevent "Link to this heading")
Published when new KV cache blocks are stored:

```
typeBlockStoredEventstruct{
BlockHashes[]int64// Hash values of stored blocks
TokenIDs[][]byte// Token IDs for each block (each token is a big-endian uint32)
ModelNamestring// Model identifier
LoraIDint64// LoRA adapter ID (-1 if none)
SourcePodstring// Source pod name
ParentBlockHash*int64// Hash value of the parent block or nil
}

```
Copy to clipboard
### BlockRemovedEvent[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#blockremovedevent "Link to this heading")
Published when blocks are removed from cache:

```
typeBlockRemovedEventstruct{
BlockHashes[]int64// Hash values of removed blocks
ModelNamestring// Model identifier
LoraIDint64// LoRA adapter ID
SourcePodstring// Source pod name
}

```
Copy to clipboard
## Troubleshooting[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#troubleshooting "Link to this heading")
### Initialization Failures[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#initialization-failures "Link to this heading")
  1. **Check initialization logs** :

```
kubectl logs deployment/aibrix-gateway-plugins -n aibrix-system | grep -E "KV event|initialize cache"

```
Copy to clipboard
  2. **Verify remote tokenizer** :

```
# Must see both enabled
kubectl get deployment/aibrix-gateway-plugins -n aibrix-system -o yaml | grep -A2 "REMOTE_TOKENIZER\|KV_EVENT_SYNC"

```
Copy to clipboard


### Events Not Publishing[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#events-not-publishing "Link to this heading")
  1. **Check vLLM logs** :

```
kubectl logs deployment/vllm-model | grep "KV cache events"

```
Copy to clipboard
  2. **Verify ZMQ connectivity** :

```
kubectl exec -it gateway-pod -n aibrix-system -- nc -zv vllm-pod-ip 5557

```
Copy to clipboard
  3. **Check ZMQ build support** :

```
kubectl exec gateway-pod -n aibrix-system -- ldd /app/gateway-plugin | grep zmq

```
Copy to clipboard


### Connection Issues[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#connection-issues "Link to this heading")
  1. **Verify pod labels** :

```
kubectl get pods -l model.aibrix.ai/kv-events-enabled=true

```
Copy to clipboard
  2. **Check network policies** :
     * Ensure ports 5557-5558 are accessible
     * No blocking NetworkPolicies
  3. **Validate tokenizer** :

```
kubectl exec gateway-pod -- curl http://tokenizer:8080/health

```
Copy to clipboard


### Performance Tuning[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#performance-tuning "Link to this heading")
  * **High Memory Usage** : Reduce buffer steps in vLLM
  * **Event Processing Lag** : Adjust batch size and polling timeout
  * **Network Overhead** : ~1MB/s per pod at high load


## Migration from Existing Deployments[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#migration-from-existing-deployments "Link to this heading")
### Enable on Existing vLLM[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#enable-on-existing-vllm "Link to this heading")
  1. Add labels:

```
kubectl label deployment vllm-model model.aibrix.ai/kv-events-enabled=true

```
Copy to clipboard
  2. Update deployment with KV event args (see Configuration section)
  3. Restart pods:

```
kubectl rollout restart deployment vllm-model

```
Copy to clipboard


### Rollback[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#rollback "Link to this heading")
To disable KV event sync:

```
# Disable in gateway
kubectl set env deployment/aibrix-gateway-plugins -n aibrix-system \
  AIBRIX_PREFIX_CACHE_KV_EVENT_SYNC_ENABLED=false

# Remove from vLLM deployments
kubectl label deployment vllm-model model.aibrix.ai/kv-events-enabled-

```
Copy to clipboard
## Best Practices[#](https://aibrix.readthedocs.io/latest/features/kv-event-sync.html#best-practices "Link to this heading")
  1. **Deployment Order** :
     * Enable remote tokenizer first and verify it’s working
     * Deploy vLLM with KV events configuration
     * Enable KV sync in gateway last
  2. **Monitoring** :
     * Enable prefix cache metrics for visibility
     * Monitor ZMQ connection status in logs
     * Track prefix cache hit rates in Grafana
  3. **Resource Planning** :
     * ZMQ traffic: ~1MB/s per vLLM pod at high load
     * Memory: Sync indexer uses ~64 bytes per prefix entry
     * CPU: Minimal overhead (<1% per pod)
  4. **Production Considerations** :
     * Use dedicated network for ZMQ traffic if possible
     * Configure appropriate timeouts based on network latency
     * Plan for graceful degradation if KV sync fails


Contents 
[AppSignal feeds Claude or Cursor the error, trace, deploy and code.**No more vibe-debugging.**](https://server.ethicalads.io/proxy/click/10878/01a09fc4-5110-78f0-b223-4633dec3ca14/)
Close Ad
