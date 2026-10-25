---
title: kvevents package - github.com/llm-d/llm-d-router/pkg/kvevents - Go Packages
id: kvevents-package-githubcomllm-dllm-d-routerpkgkvevents-go-packages
tags:
- llmd-kv-cache-routing-c665d5
- kv-events
- zmq
- llm-d-router
- go-package
created: '2026-09-14T11:53:17.978529Z'
updated: '2026-09-15T04:55:24.022883Z'
source: https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents
source_domain: pkg.go.dev
fetched_at: '2026-09-14T11:53:17.958455Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Go package doc for llm-d-router''s pkg/kvevents (v0.10.0), the write side
  of the KV-cache subsystem that keeps kvblock.Index current. vLLM and SGLang engines
  publish block-stored / block-removed / all-cleared events over ZMQ as they admit/evict
  KV blocks; a SubscriberManager maintains one ZMQ subscription per engine endpoint
  (created/torn down as pods churn via EnsureSubscriber/RemoveSubscriber), and each
  message is decoded by a per-engine EngineAdapter (ParseMessage extracts pod ID,
  model name, and an EventBatch; ShardingKey ensures same-source messages process
  in order). A sharded worker Pool (NewPool) fans decoded events out to workers keyed
  by sharding key -- guaranteeing ordering per source -- and applies Add/Evict calls
  to the kvblock.Index; NewPool also registers kvcache metrics with the controller-runtime
  registry (idempotent via sync.Once). A reference-count dedup filter ensures a block
  removal is forwarded to the index only once no remaining announcement references
  the block, preventing premature eviction when multiple engines/HMA groups share
  a block; suppressed vs forwarded removals are counted in metrics. Event types are
  typed structs: BlockStoredEvent (BlockHashes, Tokens, ParentHash, BlockSize, DeviceTier,
  LoraID/LoraName, GroupIdx, KVCacheSpecKind e.g. full_attention/mla_attention/sliding_window/mamba/chunked_local_attention/sink_full_attention/encoder_only_attention/cross_attention,
  KVCacheSpecSlidingWindowSize), BlockRemovedEvent (BlockHashes, DeviceTier, GroupIdx),
  and AllBlocksClearedEvent (DeviceTier). Config exposes ZMQEndpoint, TopicFilter
  (e.g. ''kv@''), Concurrency, EngineType (''vllm'' or ''sglang'', default vllm),
  and optional Kubernetes-based DiscoverPods/PodDiscoveryConfig (PodLabelSelector,
  PodNamespace, SocketPort default 5557, ReplaySocketPort for replay requests, disabled
  when unset).'
utility_score: 14.0
---

##  README [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#section-readme "Go to Readme")
### KV-Events
The write side of the KV-cache subsystem: it subscribes to the KV-cache events that model-serving engines emit as they admit and evict blocks, and applies them to the [`kvblock.Index`](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/README.md) so the [`kvcache.Indexer`](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/README.md) scores against an up-to-date view of what each pod holds.
#### What It Does
Engines (vLLM, SGLang) publish block-stored / block-removed / all-cleared events over ZMQ. The subsystem subscribes to those streams, decodes each engine's message format via an [engine adapter](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/engineadapter/README.md), and translates them into `Add` / `Evict` calls on the block index -- a `SubscriberManager` owns the per-engine subscriptions and a sharded worker `Pool` does the processing. This keeps cache-locality scoring real-time without polling the engines.
#### How It Works
  * **Subscription.** A ZMQ subscriber receives messages from the engine pods. `SubscriberManager` maintains one subscription per engine endpoint and tears them down as pods come and go.
  * **Decoding.** Each message is parsed by the engine adapter for its topic, producing canonical block-stored / block-removed / all-blocks-cleared events.
  * **Worker pool.** `Pool` fans messages out to workers keyed by a sharding key so events for the same block are processed in order, then applies them to the index.
  * **Reference-count dedup.** A dedup filter reference-counts block hashes so a removal is forwarded to the index only once no remaining announcement references the block, preventing premature eviction of blocks that multiple engines (or HMA groups) still hold. Suppressed and forwarded removals are counted in the [metrics](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/metrics/README.md).


#### Key Types  
| Symbol  | Role  |  
| --- | --- |  
| `Pool`  | Entry point; constructed with `NewPool(config)`. Subscribes, decodes, and applies events.  |  
| `SubscriberManager`  | Manages per-endpoint ZMQ subscriptions across pod churn.  |  
|  `Config` / `DefaultConfig`  | Worker count, subscription, and dedup configuration.  |  
#### Related Documentation
  * [Engine Adapters](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/engineadapter/README.md) -- per-engine event decoding
  * [KV-Block Index](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/README.md) -- the index these events update
  * [KV-Cache Indexer](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/README.md) -- reads the resulting index state


Expand ▾ Collapse ▴
##  Documentation [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#section-documentation "Go to Documentation")
### Overview [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#pkg-overview "Go to Overview")
Package kvevents contains the implementation of the KV-events processing system. It provides a pool for handling events coming from a distributed KV-cache pool, allowing for cache-tracking and real-time updates to the KV-cache index. The package is designed to work with the kvcache.Indexer to maintain an up-to-date state of the KV-cache and to facilitate the scoring of pods based on the KV-cache index state. 
### Index [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#pkg-index "Go to Index")
  * [type AllBlocksClearedEvent](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#AllBlocksClearedEvent)
  *     * [func (e *AllBlocksClearedEvent) Type() EventType](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#AllBlocksClearedEvent.Type)
  * [type BlockRemovedEvent](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#BlockRemovedEvent)
  *     * [func (e *BlockRemovedEvent) Type() EventType](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#BlockRemovedEvent.Type)
  * [type BlockStoredEvent](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#BlockStoredEvent)
  *     * [func (e *BlockStoredEvent) Type() EventType](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#BlockStoredEvent.Type)
  *     * [func DefaultConfig() *Config](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#DefaultConfig)
  * [type EngineAdapter](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#EngineAdapter)
  * [type EventBatch](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#EventBatch)
  * [type EventType](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#EventType)
  * [type GenericEvent](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#GenericEvent)
  * [type KVCacheSpecKind](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#KVCacheSpecKind)
  * [type PodDiscoveryConfig](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#PodDiscoveryConfig)
  *     * [func DefaultPodReconcilerConfig() *PodDiscoveryConfig](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#DefaultPodReconcilerConfig)
  *     * [func (c *PodDiscoveryConfig) EffectiveReplayPort() int](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#PodDiscoveryConfig.EffectiveReplayPort)
  *     * [func NewPool(cfg *Config, index kvblock.Index, tokenProcessor kvblock.TokenProcessor, ...) *Pool](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#NewPool)
  *     * [func (p *Pool) AddTask(task *RawMessage)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#Pool.AddTask)
    * [func (p *Pool) GroupCatalog() *kvblock.GroupCatalog](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#Pool.GroupCatalog)
    * [func (p *Pool) Shutdown(ctx context.Context)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#Pool.Shutdown)
    * [func (p *Pool) Start(ctx context.Context)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#Pool.Start)
  * [type RawMessage](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#RawMessage)
  * [type SubscriberManager](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#SubscriberManager)
  *     * [func NewSubscriberManager(pool *Pool) *SubscriberManager](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#NewSubscriberManager)
  *     * [func (sm *SubscriberManager) EnsureSubscriber(ctx context.Context, ...) error](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#SubscriberManager.EnsureSubscriber)
    * [func (sm *SubscriberManager) GetActiveSubscribers() ([]string, []string)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#SubscriberManager.GetActiveSubscribers)
    * [func (sm *SubscriberManager) RemoveSubscriber(ctx context.Context, podIdentifier string)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#SubscriberManager.RemoveSubscriber)
    * [func (sm *SubscriberManager) Shutdown(ctx context.Context)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#SubscriberManager.Shutdown)


### Constants [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#pkg-constants "Go to Constants")
This section is empty.
### Variables [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#pkg-variables "Go to Variables")
This section is empty.
### Functions [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#pkg-functions "Go to Functions")
This section is empty.
### Types [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#pkg-types "Go to Types")
#### 

```
type AllBlocksClearedEvent struct {
	DeviceTier string[](https://pkg.go.dev/builtin#string)
}
```

AllBlocksClearedEvent represents all blocks being cleared from a pod's cache. 
####  func (*AllBlocksClearedEvent) [Type](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/events.go#L128) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#AllBlocksClearedEvent.Type "Go to AllBlocksClearedEvent.Type")

```
func (e *AllBlocksClearedEvent[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#AllBlocksClearedEvent)) Type() EventType[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#EventType)
```

Type returns the event type. 
#### 

```
type BlockRemovedEvent struct {
	BlockHashes []uint64[](https://pkg.go.dev/builtin#uint64)
	DeviceTier  string[](https://pkg.go.dev/builtin#string)
	// GroupIdx identifies the vLLM KV cache group that removed this block.
	GroupIdx *int[](https://pkg.go.dev/builtin#int)
}
```

BlockRemovedEvent represents blocks being evicted from the cache. 
####  func (*BlockRemovedEvent) [Type](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/events.go#L118) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#BlockRemovedEvent.Type "Go to BlockRemovedEvent.Type")

```
func (e *BlockRemovedEvent[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#BlockRemovedEvent)) Type() EventType[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#EventType)
```

Type returns the event type. 
#### 

```
type BlockStoredEvent struct {
	BlockHashes []uint64[](https://pkg.go.dev/builtin#uint64)
	Tokens      []uint32[](https://pkg.go.dev/builtin#uint32)
	ParentHash  uint64[](https://pkg.go.dev/builtin#uint64)
	BlockSize   int[](https://pkg.go.dev/builtin#int)
	DeviceTier  string[](https://pkg.go.dev/builtin#string)
	LoraID      *int[](https://pkg.go.dev/builtin#int)
	LoraName    *string[](https://pkg.go.dev/builtin#string)
	ExtraKeys   [][]any[](https://pkg.go.dev/builtin#any)
	// GroupIdx identifies the vLLM KV cache group that emitted this event.
	GroupIdx *int[](https://pkg.go.dev/builtin#int)
	// KVCacheSpecKind carries vLLM's semantic cache type for the group.
	KVCacheSpecKind KVCacheSpecKind[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#KVCacheSpecKind)
	// KVCacheSpecSlidingWindowSize carries the SWA window size when applicable.
	KVCacheSpecSlidingWindowSize *int[](https://pkg.go.dev/builtin#int)
}
```

BlockStoredEvent represents blocks being added to the cache. 
####  func (*BlockStoredEvent) [Type](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/events.go#L105) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#BlockStoredEvent.Type "Go to BlockStoredEvent.Type")

```
func (e *BlockStoredEvent[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#BlockStoredEvent)) Type() EventType[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#EventType)
```

Type returns the event type. 
#### 

```
type Config struct {
	// ZMQEndpoint is the ZMQ address to connect to (e.g., "tcp://indexer:5557").
	ZMQEndpoint string[](https://pkg.go.dev/builtin#string) `json:"zmqEndpoint,omitempty"`
	// TopicFilter is the ZMQ subscription filter (e.g., "kv@").
	TopicFilter string[](https://pkg.go.dev/builtin#string) `json:"topicFilter"`
	// Concurrency is the number of parallel workers to run.
	Concurrency int[](https://pkg.go.dev/builtin#int) `json:"concurrency"`
	// EngineType selects the inference engine adapter ("vllm" or "sglang").
	// Default: "vllm".
	EngineType string[](https://pkg.go.dev/builtin#string) `json:"engineType,omitempty"`
	// DiscoverPods enables the Kubernetes pod reconciler for automatic
	// per-pod subscriber management. When enabled, the reconciler watches
	// Kubernetes pods and creates/removes ZMQ subscribers dynamically.
	DiscoverPods bool[](https://pkg.go.dev/builtin#bool) `json:"discoverPods"`
	// PodDiscoveryConfig holds the configuration for pod discovery.
	// Only used when DiscoverPods is true.
	PodDiscoveryConfig *PodDiscoveryConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#PodDiscoveryConfig) `json:"podDiscoveryConfig,omitempty"`
}
```

Config holds the configuration for the event processing pool. 
#### 

```
func DefaultConfig() *Config[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#Config)
```

DefaultConfig returns a default configuration for the event processing pool. 
#### 

```
type EngineAdapter interface {
	// ParseMessage parses a raw transport message into domain data.
	// It extracts pod identity and model name from the topic,
	// and decodes the payload into an EventBatch.
	ParseMessage(msg *RawMessage[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#RawMessage)) (podID, modelName string[](https://pkg.go.dev/builtin#string), batch EventBatch[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#EventBatch), err error[](https://pkg.go.dev/builtin#error))

	// ShardingKey extracts the key used to shard messages across worker queues.
	// Messages with the same sharding key are guaranteed to be processed in order.
	ShardingKey(msg *RawMessage[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#RawMessage)) string[](https://pkg.go.dev/builtin#string)
}
```

EngineAdapter defines the interface for engine-specific message parsers. Each inference engine has its own adapter implementation that handles parsing raw transport messages into domain events. 
#### 

```
type EventBatch struct {
	Timestamp float64[](https://pkg.go.dev/builtin#float64)
	Events    []GenericEvent[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#GenericEvent)
}
```

EventBatch represents a batch of generic events from an inference engine. 
#### 

```
type EventType string[](https://pkg.go.dev/builtin#string)
```

EventType represents the type of KV-cache event. 

```
const (
	// EventTypeBlockStored indicates blocks were added to cache.
	EventTypeBlockStored EventType[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#EventType) = "BlockStored"
	// EventTypeBlockRemoved indicates blocks were evicted from cache.
	EventTypeBlockRemoved EventType[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#EventType) = "BlockRemoved"
	// EventTypeAllBlocksCleared indicates entire cache was cleared.
	EventTypeAllBlocksCleared EventType[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#EventType) = "AllBlocksCleared"
)
```

#### 

```
type GenericEvent interface {
	// Type returns the event type.
	Type() EventType[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#EventType)
}
```

GenericEvent represents a KV-cache event containing already-parsed data. 
#### 

```
type KVCacheSpecKind string[](https://pkg.go.dev/builtin#string)
```

KVCacheSpecKind identifies vLLM KV cache group semantics. 

```
const (
	KVCacheSpecKindFullAttention    KVCacheSpecKind[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#KVCacheSpecKind) = "full_attention"
	KVCacheSpecKindMlaAttention     KVCacheSpecKind[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#KVCacheSpecKind) = "mla_attention"
	KVCacheSpecKindSlidingWindow    KVCacheSpecKind[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#KVCacheSpecKind) = "sliding_window"
	KVCacheSpecKindSlidingWindowMla KVCacheSpecKind[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#KVCacheSpecKind) = "sliding_window_mla"
	KVCacheSpecKindMamba            KVCacheSpecKind[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#KVCacheSpecKind) = "mamba"
	KVCacheSpecKindChunkedLocal     KVCacheSpecKind[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#KVCacheSpecKind) = "chunked_local_attention"
	KVCacheSpecKindSinkFull         KVCacheSpecKind[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#KVCacheSpecKind) = "sink_full_attention"
	KVCacheSpecKindEncoder          KVCacheSpecKind[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#KVCacheSpecKind) = "encoder_only_attention"
	KVCacheSpecKindCross            KVCacheSpecKind[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#KVCacheSpecKind) = "cross_attention"
	KVCacheSpecKindUnknown          KVCacheSpecKind[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#KVCacheSpecKind) = "unknown"
)
```

#### 

```
type PodDiscoveryConfig struct {
	// PodLabelSelector is a label selector string for filtering which pods to watch.
	// Example: "app=vllm" or "app=vllm,tier=gpu"
	PodLabelSelector string[](https://pkg.go.dev/builtin#string) `json:"podLabelSelector"`
	// PodNamespace limits the reconciler to watch pods in a specific namespace.
	// If empty, watches all namespaces (requires appropriate RBAC).
	PodNamespace string[](https://pkg.go.dev/builtin#string) `json:"podNamespace,omitempty"`
	// SocketPort is the port number where vLLM pods expose their ZMQ socket.
	// The reconciler will connect to tcp://<PodIP>:<SocketPort>
	// Default: 5557
	SocketPort int[](https://pkg.go.dev/builtin#int) `json:"socketPort"`
	// ReplaySocketPort is the port where vLLM pods expose their ZMQ ROUTER
	// socket for replay requests. Disabled when not set (0 or negative).
	ReplaySocketPort int[](https://pkg.go.dev/builtin#int) `json:"replaySocketPort,omitempty"`
}
```

PodDiscoveryConfig holds configuration for the Kubernetes pod reconciler. 
#### 

```
func DefaultPodReconcilerConfig() *PodDiscoveryConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#PodDiscoveryConfig)
```

DefaultPodReconcilerConfig returns a default configuration for the pod reconciler. 
####  func (*PodDiscoveryConfig) [EffectiveReplayPort](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/pool.go#L123) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#PodDiscoveryConfig.EffectiveReplayPort "Go to PodDiscoveryConfig.EffectiveReplayPort")

```
func (c *PodDiscoveryConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#PodDiscoveryConfig)) EffectiveReplayPort() int[](https://pkg.go.dev/builtin#int)
```

EffectiveReplayPort returns the replay socket port. Returns -1 (disabled) when not explicitly configured. 
#### 

```
type Pool struct {
	// contains filtered or unexported fields
}
```

Pool is a sharded worker pool that processes events from ZMQ subscribers. It ensures that events for the same PodIdentifier are processed in order. Pool keeps transient event-stream state while durable key mappings are delegated to the Index. 
#### 

```
func NewPool(cfg *Config[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#Config), index kvblock[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock).Index[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock#Index), tokenProcessor kvblock[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock).TokenProcessor[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock#TokenProcessor),
	adapter EngineAdapter[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#EngineAdapter),
) *Pool[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#Pool)
```

NewPool creates a Pool with a sharded worker setup. Subscribers are managed by SubscriberManager which is controlled by the pod reconciler. 
Side effect: it registers the kvcache metrics with the controller-runtime registry so that the kvevents metrics are scraped wherever a pool runs. Registration is idempotent (guarded by a sync.Once). 
####  func (*Pool) [AddTask](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/pool.go#L252) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#Pool.AddTask "Go to Pool.AddTask")

```
func (p *Pool[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#Pool)) AddTask(task *RawMessage[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#RawMessage))
```

AddTask is called by the subscriber to add a message to the processing queue. It hashes the sharding key to select a queue, ensuring messages for the same source endpoint always go to the same worker (ordered queue). 
####  func (*Pool) [GroupCatalog](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/pool.go#L211) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#Pool.GroupCatalog "Go to Pool.GroupCatalog")

```
func (p *Pool[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#Pool)) GroupCatalog() *kvblock[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock).GroupCatalog[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock#GroupCatalog)
```

GroupCatalog returns the KV cache group metadata learned from events. 
####  func (*Pool) [Shutdown](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/pool.go#L231) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#Pool.Shutdown "Go to Pool.Shutdown")

```
func (p *Pool[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#Pool)) Shutdown(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context))
```

Shutdown gracefully stops the pool and its global subscriber if present. 
####  func (*Pool) [Start](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/pool.go#L217) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#Pool.Start "Go to Pool.Start")

```
func (p *Pool[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#Pool)) Start(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context))
```

Start begins the worker pool. It is non-blocking. 
#### 

```
type RawMessage struct {
	// Topic is the original transport topic string.
	Topic string[](https://pkg.go.dev/builtin#string)
	// Sequence is the message sequence number from the transport.
	Sequence uint64[](https://pkg.go.dev/builtin#uint64)
	// Payload is the raw encoded event batch bytes, not yet decoded.
	Payload []byte[](https://pkg.go.dev/builtin#byte)
	// SourceEndpoint is the serving endpoint associated with the subscriber.
	SourceEndpoint string[](https://pkg.go.dev/builtin#string)
	// contains filtered or unexported fields
}
```

RawMessage holds the raw transport-level data from a received pub/sub message. It contains no domain-specific fields — parsing is deferred to the EngineAdapter. 
#### 

```
type SubscriberManager struct {
	// contains filtered or unexported fields
}
```

SubscriberManager manages multiple ZMQ subscribers, one per LLM engine. 
#### 

```
func NewSubscriberManager(pool *Pool[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#Pool)) *SubscriberManager[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#SubscriberManager)
```

NewSubscriberManager creates a new subscriber manager. 
####  func (*SubscriberManager) [EnsureSubscriber](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/subscriber_manager.go#L58) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#SubscriberManager.EnsureSubscriber "Go to SubscriberManager.EnsureSubscriber")

```
func (sm *SubscriberManager[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#SubscriberManager)) EnsureSubscriber(
	ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context),
	podIdentifier, sourceEndpoint, endpoint, replayEndpoint, topicFilter string[](https://pkg.go.dev/builtin#string),
	remoteSocket bool[](https://pkg.go.dev/builtin#bool),
) error[](https://pkg.go.dev/builtin#error)
```

EnsureSubscriber ensures a subscriber exists for the given pod. If the subscriber already exists with the same transport, source, and replay endpoints, it's a no-op. If an endpoint changed, the old subscriber is removed and a new one is created. 
####  func (*SubscriberManager) [GetActiveSubscribers](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/subscriber_manager.go#L171) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#SubscriberManager.GetActiveSubscribers "Go to SubscriberManager.GetActiveSubscribers")

```
func (sm *SubscriberManager[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#SubscriberManager)) GetActiveSubscribers() ([]string[](https://pkg.go.dev/builtin#string), []string[](https://pkg.go.dev/builtin#string))
```

GetActiveSubscribers returns the list of active pod identifiers and their endpoints. 
####  func (*SubscriberManager) [RemoveSubscriber](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/subscriber_manager.go#L120) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#SubscriberManager.RemoveSubscriber "Go to SubscriberManager.RemoveSubscriber")

```
func (sm *SubscriberManager[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#SubscriberManager)) RemoveSubscriber(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), podIdentifier string[](https://pkg.go.dev/builtin#string))
```

RemoveSubscriber removes a subscriber for the given pod identifier. 
####  func (*SubscriberManager) [Shutdown](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/subscriber_manager.go#L150) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#SubscriberManager.Shutdown "Go to SubscriberManager.Shutdown")

```
func (sm *SubscriberManager[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#SubscriberManager)) Shutdown(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context))
```

Shutdown shuts down all subscribers. 
##  Source Files [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#section-sourcefiles "Go to Source Files")
[View all Source files](https://github.com/llm-d/llm-d-router/tree/v0.10.0/pkg/kvevents)


##  Directories [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents#section-directories "Go to Directories")
Show internal  Collapse all  
| Path  | Synopsis  |  
| --- | --- |  
|  Package engineadapter provides engine-specific implementations of the kvevents.EngineAdapter interface.  | Package engineadapter provides engine-specific implementations of the kvevents.EngineAdapter interface.  |  
Click to show internal directories. 
Click to hide internal directories. 
go.dev uses cookies from Google to deliver and enhance the quality of its services and to analyze traffic. [Learn more.](https://policies.google.com/technologies/cookies)
Okay
