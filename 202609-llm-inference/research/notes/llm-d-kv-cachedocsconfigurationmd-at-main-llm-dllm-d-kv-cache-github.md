---
title: llm-d-kv-cache/docs/configuration.md at main · llm-d/llm-d-kv-cache · GitHub
id: llm-d-kv-cachedocsconfigurationmd-at-main-llm-dllm-d-kv-cache-github
tags:
- llmd-router-nonk8s-backends-54b774
- kv-cache-indexer
- zmq-kv-events
- pod-discovery
- non-k8s-backend
created: '2026-09-11T20:49:57.104893Z'
updated: '2026-09-11T20:50:35.696937Z'
source: https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/configuration.md
source_domain: github.com
fetched_at: '2026-09-11T20:49:57.081020Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GROUND-TRUTH configuration reference (github.com/llm-d/llm-d-kv-cache, docs/configuration.md,
  commit a746476, May 7 2026) -- gives the exact JSON config schema that operationalizes
  the architecture doc''s two event-delivery modes, and is the most directly actionable
  source found for the router-customization question.


  KV-EVENT POOL CONFIG explicitly supports two modes: "1. Static Endpoint Mode: Connects
  to a single ZMQ endpoint" and "2. Auto-Discovery Mode (default): Automatically discovers
  and subscribes to per-pod ZMQ endpoints." Static Endpoint Mode config: {"zmqEndpoint":
  "tcp://indexer:5557", "topicFilter": ..., "concurrency": 8, "engineType": "vllm",
  "discoverPods": false} -- and critically: "The zmqEndpoint field specifies the local
  ZMQ socket address to bind to." This means in Static mode the INDEXER (not the pods)
  binds a socket, and backend pods/processes connect outward to it as ZMQ PUB clients
  -- confirming this static mode requires ZERO Kubernetes API dependency and is usable
  today with any backend (VM, bare-metal, non-k8s container) that can run a ZMQ PUB
  client publishing to a "kv@..."-prefixed topic (topicFilter default "kv@") matching
  the wire format vLLM/SGLang use.


  Auto-Discovery Mode (the k8s-coupled path, default discoverPods: true) is driven
  by a PodDiscoveryConfig: {"podLabelSelector": "llm-d.ai/inference-serving=true",
  "podNamespace": "", "socketPort": 5557} implemented as "the Kubernetes pod reconciler
  for automatic per-pod ZMQ subscriber management. The reconciler watches Kubernetes
  pods and dynamically creates/removes ZMQ subscribers based on pod lifecycle." Pod
  eligibility requires ALL FOUR conditions: label-selector match, pod.Status.Phase
  == Running, pod.Status.PodIP != "", and PodReady == ConditionTrue -- when any goes
  false the subscriber is automatically torn down. Requires explicit RBAC (Role/ClusterRole
  with get/list/watch on the "pods" resource) depending on whether podNamespace is
  scoped or empty (cluster-wide).


  engineType field ("vllm" or "sglang", default "vllm") selects the EngineAdapter,
  confirming engine-adapter selection (not endpoint discovery) is the extension point
  for supporting additional inference engines'' wire formats.


  Also documents: InMemoryIndexConfig (size default 100,000,000 keys, podCacheSize
  per-key pod-entry cap), CostAwareMemoryIndexConfig (byte-budgeted, e.g. "2GiB"),
  and RedisIndexConfig (shared by both Redis and Valkey backends: address, backendType
  "redis"|"valkey", enableRDMA) -- "only the first available one will be used" among
  configured backends. TokenProcessorConfig''s hashSeed is explicitly independent
  of vLLM''s PYTHONHASHSEED ("no alignment with vLLM''s PYTHONHASHSEED is required")
  because engine keys and request keys are decoupled per the dual-key design. tokenizersPoolConfig
  confirmed deprecated -- leaving it null makes the indexer skip in-process tokenization
  pool creation entirely, requiring callers to supply token IDs directly via ScoreTokens.


  STRATEGIC IMPLICATION for the router-customization question: the KV-cache-indexer
  layer already ships a Kubernetes-independent code path (Static Endpoint Mode) that
  requires no changes to support non-Kubernetes backends for KV-event ingestion --
  the customization burden for a non-k8s-aware llm-d router falls almost entirely
  on (a) how the EPP/router''s own endpoint-pool membership and health state (analogous
  to the InferencePool/EndpointSlice watch) is populated for non-k8s backends, and
  (b) ensuring that whatever identity string a non-k8s backend uses for its own endpoint
  bookkeeping matches the IP:port the backend publishes in its ZMQ topic -- not on
  modifying the KV-cache indexer itself.'
---

*Suggested by [[llm-d-kv-cachedocsarchitecturemd-at-main-llm-dllm-d-kv-cache-github]] — linked See Also configuration reference: all JSON-serializable config fields, likely exposes the Centralized-vs-Pod-discovery mode switch*

[Skip to content](https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/configuration.md#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/configuration.md) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/configuration.md) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/configuration.md) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/configuration.md).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache) You must be signed in to change notification settings
  * [ Fork 157 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache)
  * [ Star  177 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# configuration.md
Copy path
More file actions
More file actions
## Latest commit
[Replace BlockSize with BlockSizeTokens (](https://github.com/llm-d/llm-d-kv-cache/commit/a746476b8266099aea0e77fc5e8edfbc106111d1)[#564](https://github.com/llm-d/llm-d-kv-cache/pull/564)[)](https://github.com/llm-d/llm-d-kv-cache/commit/a746476b8266099aea0e77fc5e8edfbc106111d1)
Open commit details
May 7, 2026
[a746476](https://github.com/llm-d/llm-d-kv-cache/commit/a746476b8266099aea0e77fc5e8edfbc106111d1) · May 7, 2026
## History
[History](https://github.com/llm-d/llm-d-kv-cache/commits/main/docs/configuration.md)
Open commit details
History
478 lines (375 loc) · 17.9 KB
Code owner: @vMaroon, @dannyharnik, @kfirtoledo, and @liu-cong
/
# configuration.md
Copy path
## File metadata and controls
  * Preview
  * 

478 lines (375 loc) · 17.9 KB
Code owner: @vMaroon, @dannyharnik, @kfirtoledo, and @liu-cong
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Outline
Edit and raw actions
# Configuration
This document describes all configuration options available in the llm-d KV Cache libraries. All configurations are JSON-serializable.
## Main Configuration
This package consists of two components:
  1. **KV Cache Indexer** : Manages the KV cache index, allowing efficient retrieval of cached blocks.
  2. **KV Event Processing** : Handles events from vLLM to update the cache index.


See the [Architecture Overview](https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/architecture.md) for a high-level view of how these components work and interact.
The two components are configured separately, but share both the index backend for storing KV block localities and the token processor for converting tokens into blocks. The token processor is configured via the `tokenProcessorConfig` field in the main configuration. The index backend is configured via the `kvBlockIndexConfig` field in the KV Cache Indexer configuration.
### Main Configuration Structure
The main configuration structure for the llm-d KV Cache system.

```
{
  "indexerConfig": { ... },
  "kvEventsConfig": { ... },
  "tokenProcessorConfig": { ... }
}
```
  
| Field  | Type  | Description  | Default  |  
| --- | --- | --- | --- |  
| `indexerConfig`  | Configuration for the KV Cache Indexer module  | See defaults  |  
| `kvEventsConfig`  | Configuration for the KV Event Processing pool  | See defaults  |  
| `tokenProcessorConfig`  | [TokenProcessorConfig](https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/configuration.md#token-processor-configuration-tokenprocessorconfig)  | Configuration for token processing  | See defaults  |  
## KV-Cache Indexer Configuration
### Indexer Configuration (`Config`)
The indexer configuration structure for the KV Cache Indexer module.

```
{
  "kvBlockIndexConfig": { ... },
  "tokenizersPoolConfig": { ... },
  "kvCacheBackendConfigs": { ... }
}
```
  
| Field  | Type  | Description  | Default  |  
| --- | --- | --- | --- |  
| `kvBlockIndexConfig`  | Configuration for KV block indexing  | See defaults  |  
| `tokenizersPoolConfig`  |  **Deprecated.** Configuration for the in-process tokenization pool. Leave unset (`null`) and tokenize externally; the indexer accepts token IDs via `ScoreTokens`.  | `null`  |  
| `kvCacheBackendConfigs`  | [KVCacheBackendConfig](https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/configuration.md#kv-cache-backend-configuration-kvcachebackendconfig)  | Configuration for KV Cache Device Backends  | See defaults  |  
## Complete Example Configuration
Here's a complete configuration example with all options:

```
{
  "kvBlockIndexConfig": {
    "inMemoryConfig": {
      "size": 100000000,
      "podCacheSize": 10
    },
    "enableMetrics": true,
    "metricsLoggingInterval": "1m0s"
  },
  "tokenizersPoolConfig": {
    "modelName": "namespace/model-name",
    "workersCount": 8,
    "hf": {
      "huggingFaceToken": "your_hf_token_here",
      "tokenizersCacheDir": "/tmp/tokenizers"
    },
    "local": {
      "autoDiscoveryDir": "/mnt/models",
      "autoDiscoveryTokenizerFileName": "tokenizer.json"
    }
  },
  "kvCacheBackendConfigs": [
    {
      "name": ,
      "weight": 1.0
    },
    {
      "name": ,
      "weight": 0.8
    }
  ]
}
```

## KV-Block Index Configuration
### Index Configuration (`IndexConfig`)
Configures the KV-block index backend. Multiple backends can be configured, but only the first available one will be used.

```
{
  "inMemoryConfig": { ... },
  "costAwareMemoryConfig": { ... },
  "redisConfig": { ... },
  "enableMetrics": false
}
```
  
| Field  | Type  | Description  | Default  |  
| --- | --- | --- | --- |  
| `inMemoryConfig`  | [InMemoryIndexConfig](https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/configuration.md#in-memory-index-configuration)  | In-memory index configuration  | See defaults  |  
| `costAwareMemoryConfig`  | [CostAwareMemoryIndexConfig](https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/configuration.md#cost-aware-memory-index-configuration)  | Cost-aware memory index configuration  | `null`  |  
| `redisConfig`  | [RedisIndexConfig](https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/configuration.md#redis-index-configuration)  | Redis index configuration  | `null`  |  
| `enableMetrics`  | `boolean`  | Enable admissions/evictions/hits/misses recording  | `false`  |  
| `metricsLoggingInterval`  |  `string` (duration)  | Interval at which metrics are logged (e.g., `"1m0s"`). If zero or omitted, metrics logging is disabled. Requires `enableMetrics` to be `true`.  | `"0s"`  |  
### In-Memory Index Configuration (`InMemoryIndexConfig`)
Configures the in-memory KV block index implementation.

```
{
  "size": 100000000,
  "podCacheSize": 10
}
```
  
| Field  | Type  | Description  | Default  |  
| --- | --- | --- | --- |  
| `size`  | `integer`  | Maximum number of keys that can be stored  | `100000000`  |  
| `podCacheSize`  | `integer`  | Maximum number of pod entries per key  |  
### Cost-Aware Memory Index Configuration (`CostAwareMemoryIndexConfig`)
Configures the cost-aware memory-based KV block index implementation using Ristretto cache.

```
{
  "size": "2GiB"
}
```
  
| Field  | Type  | Description  | Default  |  
| --- | --- | --- | --- |  
| `size`  | `string`  | Maximum memory size for the cache. Supports human-readable formats like "2GiB", "500MiB", "1GB", etc.  | `"2GiB"`  |  
### Redis Index Configuration (`RedisIndexConfig`)
Configures the Redis-backed KV block index implementation.

```
{
  "address": "redis://127.0.0.1:6379"
}
```
  
| Field  | Type  | Description  | Default  |  
| --- | --- | --- | --- |  
| `address`  | `string`  | Redis server address (can include auth: `redis://user:pass@host:port/db`)  | `"redis://127.0.0.1:6379"`  |  
| `backendType`  | `string`  | Backend type: "redis" or "valkey" (optional, mainly for documentation)  | `"redis"`  |  
| `enableRDMA`  | `boolean`  | Enable RDMA transport for Valkey (experimental, requires Valkey with RDMA support)  | `false`  |  
### Valkey Index Configuration (`RedisIndexConfig`)
Configures the Valkey-backed KV block index implementation. Valkey is a Redis-compatible, open-source alternative that supports RDMA for improved latency.

```
{
  "address": "valkey://127.0.0.1:6379",
  "backendType": "valkey", 
  "enableRDMA": false
}
```
  
| Field  | Type  | Description  | Default  |  
| --- | --- | --- | --- |  
| `address`  | `string`  | Valkey server address. Supports `valkey://`, `valkeys://` (SSL), `redis://`, or plain address  | `"valkey://127.0.0.1:6379"`  |  
| `backendType`  | `string`  | Should be "valkey" for Valkey instances  | `"valkey"`  |  
| `enableRDMA`  | `boolean`  | Enable RDMA transport (requires Valkey server with RDMA support)  | `false`  |  
**Note** : Both Redis and Valkey configurations use the same `RedisIndexConfig` structure since Valkey is API-compatible with Redis.
## Tokenization Configuration
> **Deprecated.** The in-process tokenization pool described below is retained only for backwards-compatibility with the prompt-string indexer APIs (`GetPodScores`, `ComputeBlockKeys`). New integrations should tokenize externally — in the host process or a sidecar — and feed token IDs into `Indexer.ScoreTokens` (or `Indexer.ComputeBlockKeysFromTokens`) directly. When `tokenizersPoolConfig` is left unset, the indexer skips pool creation and the prompt-string entry points return an error.
### Tokenization Pool Configuration (`Config`)
Configures the tokenization worker pool and cache utilization strategy.

```
{
  "modelName": "namespace/model-name",
  "workersCount": 5,
  "hf": {
    "enabled": true,
    "huggingFaceToken": ,
    "tokenizersCacheDir": 
  },
  "local": {
    "autoDiscoveryDir": "/mnt/models",
    "autoDiscoveryTokenizerFileName": "tokenizer.json",
    "modelTokenizerMap": {
      "my-model": "/path/to/custom-model/tokenizer.json"
    }
  }
}
```
  
| Field  | Type  | Description  | Default  |  
| --- | --- | --- | --- |  
| `modelName`  | `string`  | Base model name for the tokenizer.  |  
| `workersCount`  | `integer`  | Number of tokenization worker goroutines  |  
| `HFTokenizerConfig`  | HuggingFace tokenizer config  |  
| `local`  | `LocalTokenizerConfig`  | Local tokenizer config  |  
### Local Tokenizer Configuration (`LocalTokenizerConfig`)
Configures loading tokenizers from local files. Useful for air-gapped environments or when models are pre-loaded.

```
{
  "autoDiscoveryDir": "/mnt/models",
  "autoDiscoveryTokenizerFileName": "tokenizer.json",
  "modelTokenizerMap": {
    "my-model": "/path/to/custom-model/tokenizer.json"
  }
}
```
  
| Field  | Type  | Description  | Default  |  
| --- | --- | --- | --- |  
| `autoDiscoveryDir`  | `string`  | Directory to recursively scan for tokenizer files. Can be set via `LOCAL_TOKENIZER_DIR` environment variable.  | `"/mnt/models"`  |  
| `autoDiscoveryTokenizerFileName`  | `string`  | Filename to search for during auto-discovery. Can be set via `LOCAL_TOKENIZER_FILENAME` environment variable.  | `"tokenizer.json"`  |  
| `modelTokenizerMap`  | `map[string]string`  | Manual mapping from model name to tokenizer file path. Overrides auto-discovered model mappings.  |  
**Auto-Discovery Behavior:**
When `autoDiscoveryDir` is set, the system recursively scans the directory for files matching `autoDiscoveryTokenizerFileName`. It supports two directory structure patterns:
  1. **HuggingFace Cache Structure** (automatically detected):

```
~/.cache/huggingface/hub/
  models--Qwen--Qwen3-0.6B/snapshots/{hash}/tokenizer.json
    → Model name: "Qwen/Qwen3-0.6B"
  models--meta-llama--Llama-2-7b-chat-hf/snapshots/{hash}/tokenizer.json
    → Model name: "meta-llama/Llama-2-7b-chat-hf"

```

  2. **Custom Directory Structure** (arbitrary nesting):

```
/mnt/models/
  llama-7b/tokenizer.json
    → Model name: "llama-7b"
  Qwen/Qwen3/tokenizer.json
    → Model name: "Qwen/Qwen3"
  org/team/model/tokenizer.json
    → Model name: "org/team/model"

```



**Environment Variables:**
  * `LOCAL_TOKENIZER_DIR`: Overrides the default auto-discovery directory
  * `LOCAL_TOKENIZER_FILENAME`: Overrides the default tokenizer filename


### HuggingFace Tokenizer Configuration (`HFTokenizerConfig`)
Configures the HuggingFace tokenizer backend for downloading tokenizers from HuggingFace Hub.

```
{
  "enabled": true,
  "huggingFaceToken": ,
  "tokenizersCacheDir": "./bin"
}
```
  
| Field  | Type  | Description  | Default  |  
| --- | --- | --- | --- |  
| `enabled`  | `boolean`  | Enable HuggingFace tokenizer backend  | `true`  |  
| `huggingFaceToken`  | `string`  | HuggingFace API token for accessing private models  |  
| `tokenizersCacheDir`  | `string`  | Local directory for caching downloaded tokenizers  | `"./bin"`  |  
**Note** : The system uses a composite tokenizer by default that tries local tokenizers first, then falls back to HuggingFace tokenizers if enabled and the model is not found locally.
## KV Cache Backend Tiers
### KV Cache Backend Configuration (`KVCacheBackendConfig`)
Configures the available device backends which store the KV Cache blocks. This will be used in scoring.

```
{
  [
    {
      "name": ,
      "weight": 1.0,
    },
    {
      "name": ,
      "weight": 0.8,
    }
  ]
}
```

## KV-Event Processing Configuration
### KV-Event Pool Configuration (`Config`)
Configures the ZMQ event processing pool for handling KV cache events. The pool supports two modes:
  1. **Static Endpoint Mode** : Connects to a single ZMQ endpoint
  2. **Auto-Discovery Mode** (default): Automatically discovers and subscribes to per-pod ZMQ endpoints



```
{
  "topicFilter": ,
  "concurrency": 16,
  "discoverPods": true
}
```
  
| Field  | Type  | Description  | Default  |  
| --- | --- | --- | --- |  
| `zmqEndpoint`  | `string`  | ZMQ address to connect to  |  
| `topicFilter`  | `string`  | ZMQ subscription filter  | `"kv@"`  |  
| `concurrency`  | `integer`  | Number of parallel workers  |  
| `engineType`  | `string`  | Inference engine adapter type (`"vllm"` or `"sglang"`)  | `"vllm"`  |  
| `discoverPods`  | `boolean`  | Enable Kubernetes pod reconciler for automatic per-pod subscriber management  | `true`  |  
| `podDiscoveryConfig`  | [PodDiscoveryConfig](https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/configuration.md#pod-discovery-configuration-podDiscoveryConfig)  | Configuration for pod reconciler (only used when `discoverPods` is true)  | `null`  |  
#### Static Endpoint Mode Example
For connecting to a single ZMQ endpoint:

```
{
  "zmqEndpoint": "tcp://indexer:5557",
  "topicFilter": ,
  "concurrency": 8,
  "engineType": "vllm",
  "discoverPods": false
}
```

The `zmqEndpoint` field specifies the **local** ZMQ socket address to **bind** to.
#### Auto-Discovery Mode Example
For automatic Kubernetes pod discovery:

```
{
  "topicFilter": ,
  "concurrency": 8,
  "discoverPods": true,
  "podDiscoveryConfig": {
    "podLabelSelector": "llm-d.ai/inference-serving=true",
    "podNamespace": "inference",
    "socketPort": 5557,
  }
}
```

### Pod Discovery Configuration (`PodDiscoveryConfig`)
Configures the Kubernetes pod reconciler for automatic per-pod ZMQ subscriber management. The reconciler watches Kubernetes pods and dynamically creates/removes ZMQ subscribers based on pod lifecycle.

```
{
  "podLabelSelector": "llm-d.ai/inference-serving=true",
  "podNamespace": ,
  "socketPort": 5556,
}
```
  
| Field  | Type  | Description  | Default  |  
| --- | --- | --- | --- |  
| `podLabelSelector`  | `string`  | Label selector for filtering which pods to watch. Examples: `"app=vllm"`, `"app=vllm,tier=gpu"`  | `"llm-d.ai/inference-serving=true"`  |  
| `podNamespace`  | `string`  | Namespace to watch pods in. If empty, watches all namespaces (requires cluster-wide RBAC)  |  `""` (all namespaces)  |  
| `socketPort`  | `integer`  | Port number where vLLM pods expose their ZMQ socket  | `5557`  |  
#### Pod Requirements
For the reconciler to create a subscriber for a pod, the pod must meet these conditions:
  1. **Match label selector** : Pod labels must match the configured `podLabelSelector`
  2. **Running state** : `pod.Status.Phase == Running`
  3. **Has IP address** : `pod.Status.PodIP != ""`
  4. **Ready condition** : Pod has condition `PodReady == ConditionTrue`


When any of these conditions becomes false, the subscriber is automatically removed.
#### RBAC Requirements
When using the pod reconciler, ensure the service account has appropriate RBAC permissions:
**Namespace-scoped** (when `podNamespace` is set):

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: kv-cache-manager
  namespace: inference
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]
```

**Cluster-wide** (when `podNamespace` is empty):

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: kv-cache-manager
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]
```

## Event Processing Configuration Example
For the ZMQ event processing pool:

```
{
  "zmqEndpoint": "tcp://indexer:5557",
  "topicFilter": ,
  "concurrency": 8
}
```

## Token Processing Configuration
### Token Processor Configuration (`TokenProcessorConfig`)
Configures how tokens are converted to KV-block keys.

```
{
  "blockSizeTokens": 16,
  "hashSeed": 
}
```
  
| Field  | Type  | Description  | Default  |  
| --- | --- | --- | --- |  
| `blockSizeTokens`  | `integer`  | Number of tokens per block  |  
| `blockSize`  | `integer`  |  **Deprecated:** Use `blockSizeTokens` instead. Still accepted for backward compatibility.  | default of `blockSizeTokens`  |  
| `hashSeed`  | `string`  | Seed for the indexer's own request-key hash chain. Independent of any engine-side seed.  |  
## Notes
  1. **Hash Seed** : The `hashSeed` in `TokenProcessorConfig` seeds the indexer's internal request-key hashing. Engine keys and request keys are decoupled — no alignment with vLLM's `PYTHONHASHSEED` is required. See [architecture.md](https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/architecture.md#the-dual-key-design) for how the two key spaces are bridged.
  2. **Memory Considerations** :
     * The `size` parameter in `InMemoryIndexConfig` directly affects memory usage. Each key-value pair consumes memory proportional to the number of associated pods.
     * The `size` parameter in `CostAwareMemoryIndexConfig` controls the maximum memory footprint and supports human-readable formats (e.g., "2GiB", "500MiB", "1GB").
  3. **Performance Tuning** :
     * Increase `workersCount` in tokenization config for higher tokenization throughput
     * Adjust `concurrency` in event processing for better event handling performance
     * Tune cache sizes based on available memory and expected workload
  4. **Cache Directories** : If used, ensure the `tokenizersCacheDir` has sufficient disk space and appropriate permissions for the application to read/write tokenizer files.
  5. **Redis Configuration** : When using Redis backend, ensure Redis server is accessible and has sufficient memory. The `address` field supports full Redis URLs including authentication: `redis://user:pass@host:port/db`.


You can’t perform that action at this time. 
