---
title: kvblock package - github.com/llm-d/llm-d-router/pkg/kvcache/kvblock - Go Packages
id: kvblock-package-githubcomllm-dllm-d-routerpkgkvcachekvblock-go-packages
tags:
- llmd-kv-cache-routing-c665d5
- locus-approximate-vs-precise-index
- kv-cache-indexer
- block-key-hashing
created: '2026-09-14T12:41:45.020593Z'
updated: '2026-09-14T12:42:55.317542Z'
source: https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock
source_domain: pkg.go.dev
fetched_at: '2026-09-14T12:41:45.001121Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'GoDoc reference for github.com/llm-d/llm-d-router/pkg/kvcache/kvblock (v0.10.0)
  -- the KV-Block Index package implementing block-key hashing and the dual-key store.
  Key types: BlockHash is a uint64 (fits an FNV-64a output); TokenProcessor (backed
  by ChunkedTokenDatabase, created via NewChunkedTokenDatabase) ''chunks tokens into
  fixed-size blocks and hashes each block, chaining the previous block''s hash so
  a key encodes its full prefix'' -- default BlockSizeTokens is 16 (0 resolves to
  default). ComputeBlockExtraFeatures ''matches vLLM''s _gen_mm_extra_hash_keys()
  algorithm'' and ''matches vLLM v0.18.0+ which appends mm_feature.identifier per
  overlapping item'' (MMHash mirrors vLLM''s per-block extra_keys format); ParseRawExtraKeys
  also accepts the legacy [string,int] tuple format for pre-0.18 vLLM.\n\nCRITICAL
  HASHSEED DETAIL (tension with the llm-d-kv-cache architecture.md doc): TokenProcessorConfig.HashSeed''s
  godoc comment reads verbatim: ''HashSeed is used to prefix initial hash chunks,
  similarly to vLLM''s NONE_HASH. This should be aligned with vLLM''s PYTHONHASHSEED
  environment variable. The system''s deployer is responsible for aligning the vLLM
  deployments with the same seed value.'' This directly contradicts (or at least is
  in tension with) the architecture.md doc''s claim that the indexer''s request-key
  space ''does not require alignment with any engine-side hash seed'' -- the godoc
  explicitly places the alignment burden on the deployer. Neither source states the
  underlying hash algorithm name (SHA-256/xxhash/FNV) inside this package doc itself
  (that detail -- FNV-64a over CBOR -- comes from architecture.md, not from this godoc
  page).\n\nDual-key index API: Index interface exposes Add(engineKeys, requestKeys,
  entries), Lookup(requestKeys, podIdentifierSet), Evict(key, keyType, entries) where
  keyType is EngineKey or RequestKey, and GetRequestKey(engineKey) -> requestKey.
  Three Index implementations ship: InMemoryIndex, CostAwareMemoryIndex, and RedisIndex
  (Redis backend pages the keyspace with SCAN, stores JSON-encoded PodEntry per hash
  field, skips ''engine:'' prefixed keys). KeyType.RequestKey variant is ''used for
  speculative entries that were added without engineKey mapping.'' No mention of SGLang
  or TensorRT-LLM anywhere in this package''s godoc.'
---

*Suggested by [[prefix-cache-aware-routing-llm-d-2]] — block key hashing implementation referenced in routing docs*

##  README [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#section-readme "Go to Readme")
### KV-Block Index
The block-level store that maps KV-block keys to the pods holding them, plus the token-to-block-key processing that feeds it. This is the shared state the [`kvcache.Indexer`](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/README.md) reads and the [`kvevents`](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/README.md) subscriber writes.
#### What It Does
  * **Token processing.** `TokenProcessor` (backed by `ChunkedTokenDatabase`) turns a token sequence into KV-block keys: it chunks tokens into fixed-size blocks and hashes each block, chaining the previous block's hash so a key encodes its full prefix. `ComputeBlockExtraFeatures` folds multimodal metadata into per-block `BlockExtraFeatures` that taint the hash.
  * **Block index.** `Index` maps each block key to the set of pods (`PodEntry`, carrying a pod identifier and device tier) that currently hold it. Producers `Add` and `Evict` entries; the indexer's `Lookup` reads them.


#### Backends
`NewIndex` selects a backend from `IndexConfig` (first configured wins):  
| Backend  | Constructor  | Use  |  
| --- | --- | --- |  
| In-memory  | `NewInMemoryIndex`  | Single-replica, LRU-bounded local index.  |  
| Cost-aware memory  | `NewCostAwareMemoryIndex`  | Local index with cost-aware eviction.  |  
| Redis  | `NewRedisIndex`  | Shared index across EPP replicas (Redis-protocol server).  |  
Two decorators wrap any backend: `NewTracedIndex` adds OpenTelemetry spans (no-op when tracing is off) and `NewInstrumentedIndex` records the [metrics](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/metrics/README.md).
#### Grouped (HMA) Entries
`GroupCatalog` tracks group identity for heterogeneous-memory-aware (HMA) models, where a block may be announced by a group of engines; eviction is reference-counted at the group level so a block is removed only once no group still references it.
#### Key Types  
| Symbol  | Role  |  
| --- | --- |  
| `Index`  | Block-key -> pods mapping (`Add` / `Lookup` / `Evict`).  |  
| `TokenProcessor`  | Tokens -> block keys; exposes `BlockSize`.  |  
| `BlockHash`  | A single block key.  |  
| `PodEntry`  | A pod holding a block, with its device tier.  |  
|  `BlockExtraFeatures` / `ComputeBlockExtraFeatures`  | Per-block multimodal metadata folded into the hash.  |  
| `PlaceholderRange`  | Placeholder-token range for a multimodal item.  |  
#### Related Documentation
  * [KV-Cache Indexer](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/README.md) -- reads this index to score pods
  * [KV-Events](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/README.md) -- writes to this index from engine events


Expand ▾ Collapse ▴
##  Documentation [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#section-documentation "Go to Documentation")
### Index [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#pkg-index "Go to Index")
  * [type BlockExtraFeatures](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockExtraFeatures)
  *     * [func ComputeBlockExtraFeatures(mmHashes map[string][]string, mmPlaceholders map[string][]PlaceholderRange, ...) []*BlockExtraFeatures](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#ComputeBlockExtraFeatures)
    * [func ParseRawExtraKeys(raw [][]any) ([]*BlockExtraFeatures, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#ParseRawExtraKeys)
  * [type BlockHash](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash)
  *     * [func (c BlockHash) String() string](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash.String)
  * [type CostAwareMemoryIndex](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex)
  *     * [func NewCostAwareMemoryIndex(cfg *CostAwareMemoryIndexConfig) (*CostAwareMemoryIndex, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#NewCostAwareMemoryIndex)
  *     * [func (m *CostAwareMemoryIndex) Add(ctx context.Context, engineKeys, requestKeys []BlockHash, entries []PodEntry) error](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex.Add)
    * [func (m *CostAwareMemoryIndex) Clear(ctx context.Context, podIdentifier string) error](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex.Clear)
    * [func (m *CostAwareMemoryIndex) Evict(ctx context.Context, key BlockHash, keyType KeyType, entries []PodEntry) error](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex.Evict)
    * [func (m *CostAwareMemoryIndex) GetRequestKey(ctx context.Context, engineKey BlockHash) (BlockHash, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex.GetRequestKey)
    * [func (m *CostAwareMemoryIndex) Lookup(ctx context.Context, requestKeys []BlockHash, ...) (map[BlockHash][]PodEntry, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex.Lookup)
    * [func (m *CostAwareMemoryIndex) MaxCost() int64](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex.MaxCost)
  * [type CostAwareMemoryIndexConfig](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndexConfig)
  *     * [func DefaultCostAwareMemoryIndexConfig() *CostAwareMemoryIndexConfig](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#DefaultCostAwareMemoryIndexConfig)
  * [type CostPodCache](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostPodCache)
  *     * [func (c *CostPodCache) Add(entry PodEntry)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostPodCache.Add)
    * [func (c *CostPodCache) CalculateByteSize(keyStr string) int64](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostPodCache.CalculateByteSize)
    * [func (c *CostPodCache) Delete(entry PodEntry)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostPodCache.Delete)
    * [func (c *CostPodCache) Len() int](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostPodCache.Len)
  * [type GroupCatalog](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#GroupCatalog)
  *     * [func NewGroupCatalog() *GroupCatalog](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#NewGroupCatalog)
  *     * [func (c *GroupCatalog) Get(podID string, g GroupID) (GroupMetadata, bool)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#GroupCatalog.Get)
    * [func (c *GroupCatalog) Learn(podID string, g GroupID, meta GroupMetadata)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#GroupCatalog.Learn)
  * [type GroupID](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#GroupID)
  * [type GroupMetadata](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#GroupMetadata)
  * [type InMemoryIndex](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndex)
  *     * [func NewInMemoryIndex(cfg *InMemoryIndexConfig) (*InMemoryIndex, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#NewInMemoryIndex)
  *     * [func (m *InMemoryIndex) Add(ctx context.Context, engineKeys, requestKeys []BlockHash, entries []PodEntry) error](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndex.Add)
    * [func (m *InMemoryIndex) Clear(ctx context.Context, podIdentifier string) error](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndex.Clear)
    * [func (m *InMemoryIndex) Evict(ctx context.Context, key BlockHash, keyType KeyType, entries []PodEntry) error](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndex.Evict)
    * [func (m *InMemoryIndex) GetRequestKey(ctx context.Context, engineKey BlockHash) (BlockHash, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndex.GetRequestKey)
    * [func (m *InMemoryIndex) Lookup(ctx context.Context, requestKeys []BlockHash, ...) (map[BlockHash][]PodEntry, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndex.Lookup)
  * [type InMemoryIndexConfig](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndexConfig)
  *     * [func DefaultInMemoryIndexConfig() *InMemoryIndexConfig](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#DefaultInMemoryIndexConfig)
  *     * [func NewIndex(ctx context.Context, cfg *IndexConfig) (Index, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#NewIndex)
    * [func NewInstrumentedIndex(next Index) Index](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#NewInstrumentedIndex)
    * [func NewRedisIndex(config *RedisIndexConfig) (Index, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#NewRedisIndex)
    * [func NewTracedIndex(next Index) Index](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#NewTracedIndex)
  * [type IndexConfig](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#IndexConfig)
  *     * [func DefaultIndexConfig() *IndexConfig](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#DefaultIndexConfig)
  * [type KeyType](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#KeyType)
  * [type PlaceholderRange](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PlaceholderRange)
  * [type PodCache](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodCache)
  * [type PodEntry](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodEntry)
  *     * [func (e *PodEntry) String() string](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodEntry.String)
  * [type RedisIndex](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndex)
  *     * [func (r *RedisIndex) Add(ctx context.Context, engineKeys, requestKeys []BlockHash, entries []PodEntry) error](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndex.Add)
    * [func (r *RedisIndex) Clear(ctx context.Context, podIdentifier string) error](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndex.Clear)
    * [func (r *RedisIndex) Evict(ctx context.Context, key BlockHash, keyType KeyType, entries []PodEntry) error](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndex.Evict)
    * [func (r *RedisIndex) GetRequestKey(ctx context.Context, engineKey BlockHash) (BlockHash, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndex.GetRequestKey)
    * [func (r *RedisIndex) Lookup(ctx context.Context, requestKeys []BlockHash, ...) (map[BlockHash][]PodEntry, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndex.Lookup)
  * [type RedisIndexConfig](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndexConfig)
  *     * [func DefaultRedisIndexConfig() *RedisIndexConfig](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#DefaultRedisIndexConfig)
  * [type TokenProcessor](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#TokenProcessor)
  *     * [func NewChunkedTokenDatabase(config *TokenProcessorConfig) (TokenProcessor, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#NewChunkedTokenDatabase)
  * [type TokenProcessorConfig](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#TokenProcessorConfig)
  *     * [func DefaultTokenProcessorConfig() *TokenProcessorConfig](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#DefaultTokenProcessorConfig)


### Constants [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#pkg-constants "Go to Constants")
This section is empty.
### Variables [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#pkg-variables "Go to Variables")
This section is empty.
### Functions [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#pkg-functions "Go to Functions")
This section is empty.
### Types [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#pkg-types "Go to Types")
#### 

```
type BlockExtraFeatures struct {
	MMHashes []MMHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#MMHash)
}
```

BlockExtraFeatures holds per-block extra data that taints the block hash. A nil *BlockExtraFeatures means pure text (no taint). 
#### 

```
func ComputeBlockExtraFeatures(
	mmHashes map[string[](https://pkg.go.dev/builtin#string)][]string[](https://pkg.go.dev/builtin#string),
	mmPlaceholders map[string[](https://pkg.go.dev/builtin#string)][]PlaceholderRange[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PlaceholderRange),
	blockSize, numTokens int[](https://pkg.go.dev/builtin#int),
) []*BlockExtraFeatures[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockExtraFeatures)
```

ComputeBlockExtraFeatures converts tokenizer-provided multimodal metadata into per-block extra features, matching vLLM's _gen_mm_extra_hash_keys() algorithm. 
For each block, it finds overlapping placeholder ranges and emits the identifier (hash) of each overlapping multimodal item. This matches vLLM v0.18.0+ which appends mm_feature.identifier per overlapping item. 
#### 

```
func ParseRawExtraKeys(raw [][]any[](https://pkg.go.dev/builtin#any)) ([]*BlockExtraFeatures[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockExtraFeatures), error[](https://pkg.go.dev/builtin#error))
```

ParseRawExtraKeys converts the raw [][]any from BlockStoredEvent.ExtraKeys into typed []*BlockExtraFeatures. Each inner []any element is either: 
  * a bare string identifier (vLLM v0.18.0+: mm_feature.identifier), or
  * a 2-element [string, int] tuple (legacy format, offset is ignored).


nil inner slices produce nil entries. Returns nil if raw is nil. 
#### 

```
type BlockHash uint64[](https://pkg.go.dev/builtin#uint64)
```

BlockHash struct represents a unique identifier for a KV-cache block. 

```
const EmptyBlockHash BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash) = 0
```

EmptyBlockHash represents an invalid or uninitialized block hash. This serves as the "error value". 
####  func (BlockHash) [String](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/index.go#L169) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash.String "Go to BlockHash.String")

```
func (c BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash)) String() string[](https://pkg.go.dev/builtin#string)
```

String returns a string representation of the Key. 
#### 

```
type CostAwareMemoryIndex struct {
	// contains filtered or unexported fields
}
```

CostAwareMemoryIndex implements the Index interface using Ristretto cache for cost-aware memory management. The two caches below are kept in sync: 
  * data: requestKey -> pod cache (cost-bound by Ristretto MaxCost)
  * requestKeys: engineKey -> requestKey (LRU to cap mapping size)


Add always writes both maps; Evict removes pods and, when empty, removes both the requestKey entry and its engineKey mapping to avoid dangling keys. 
#### 

```
func NewCostAwareMemoryIndex(cfg *CostAwareMemoryIndexConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndexConfig)) (*CostAwareMemoryIndex[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex), error[](https://pkg.go.dev/builtin#error))
```

NewCostAwareMemoryIndex creates a new CostAwareMemoryIndex instance. 
####  func (*CostAwareMemoryIndex) [Add](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/cost_aware_memory.go#L231) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex.Add "Go to CostAwareMemoryIndex.Add")

```
func (m *CostAwareMemoryIndex[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex)) Add(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), engineKeys, requestKeys []BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash), entries []PodEntry[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodEntry)) error[](https://pkg.go.dev/builtin#error)
```

Add adds a set of keys and their associated pod entries to the index backend. If engineKeys is nil, only requestKey -> PodEntry mappings are created (no engineKey -> requestKey mapping). This is used for speculative entries where engine keys are not yet known. When engineKeys is non-nil, the mapping type is inferred from the ratio of array lengths. 
####  func (*CostAwareMemoryIndex) [Clear](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/cost_aware_memory.go#L416) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex.Clear "Go to CostAwareMemoryIndex.Clear")

```
func (m *CostAwareMemoryIndex[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex)) Clear(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), podIdentifier string[](https://pkg.go.dev/builtin#string)) error[](https://pkg.go.dev/builtin#error)
```

Clear removes every entry for the pod from the index, across all device tiers. It is O(N) over the index but runs off the Lookup/Add hot path at a coarse cadence. The scan is chunked: it snapshots the request keys, then processes them in fixed-size chunks, each taking mu only briefly, so a Clear never blocks Lookup (which takes mu.RLock) for the whole pass. The trade is atomicity — a concurrent Lookup may see the pod cleared from some keys but not yet from others; that is acceptable since the pod's cache is cold post-reset and a stale hit only costs a cache miss. 
The engineKey->requestKey mapping (requestKeys) is intentionally left untouched: it is LRU-bounded, self-heals when the pod re-Adds the same prefixes, and any stale mapping resolves to an emptied request key that correctly breaks the prefix chain in Lookup. Reverse-pruning it would need an O(M) scan for no correctness gain. 
####  func (*CostAwareMemoryIndex) [Evict](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/cost_aware_memory.go#L333) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex.Evict "Go to CostAwareMemoryIndex.Evict")

```
func (m *CostAwareMemoryIndex[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex)) Evict(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), key BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash), keyType KeyType[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#KeyType), entries []PodEntry[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodEntry)) error[](https://pkg.go.dev/builtin#error)
```

Evict removes a key and its associated pod entries from the index backend. keyType indicates whether the key is an EngineKey (requires engine→request lookup) or a RequestKey (used directly for speculative entries without engineKey mapping). 
####  func (*CostAwareMemoryIndex) [GetRequestKey](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/cost_aware_memory.go#L475) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex.GetRequestKey "Go to CostAwareMemoryIndex.GetRequestKey")

```
func (m *CostAwareMemoryIndex[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex)) GetRequestKey(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), engineKey BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash)) (BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash), error[](https://pkg.go.dev/builtin#error))
```

GetRequestKey returns the last request key (highest index in the chain) associated with the given engineKey. Returns an error if the engineKey is not mapped (e.g., evicted earlier). 
####  func (*CostAwareMemoryIndex) [Lookup](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/cost_aware_memory.go#L275) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex.Lookup "Go to CostAwareMemoryIndex.Lookup")

```
func (m *CostAwareMemoryIndex[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex)) Lookup(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), requestKeys []BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash),
	podIdentifierSet sets[](https://pkg.go.dev/k8s.io/apimachinery/pkg/util/sets).Set[](https://pkg.go.dev/k8s.io/apimachinery/pkg/util/sets#Set)[string[](https://pkg.go.dev/builtin#string)],
) (map[BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash)][]PodEntry[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodEntry), error[](https://pkg.go.dev/builtin#error))
```

####  func (*CostAwareMemoryIndex) [MaxCost](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/cost_aware_memory.go#L155) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex.MaxCost "Go to CostAwareMemoryIndex.MaxCost")

```
func (m *CostAwareMemoryIndex[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndex)) MaxCost() int64[](https://pkg.go.dev/builtin#int64)
```

#### 

```
type CostAwareMemoryIndexConfig struct {
	// Size is the maximum memory size that can be used by the index.
	// Supports human-readable formats like "2GiB", "500MiB", "1GB", etc.
	Size string[](https://pkg.go.dev/builtin#string) `json:"size,omitempty"`
}
```

CostAwareMemoryIndexConfig holds the configuration for the CostAwareMemoryIndex. 
####  func [DefaultCostAwareMemoryIndexConfig](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/cost_aware_memory.go#L47) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#DefaultCostAwareMemoryIndexConfig "Go to DefaultCostAwareMemoryIndexConfig")

```
func DefaultCostAwareMemoryIndexConfig() *CostAwareMemoryIndexConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndexConfig)
```

#### 

```
type CostPodCache struct {
	// contains filtered or unexported fields
}
```

CostPodCache wraps a sync.Map of PodEntry and provides cost calculation for memory usage estimation. 
####  func (*CostPodCache) [Add](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/cost_aware_memory.go#L171) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostPodCache.Add "Go to CostPodCache.Add")

```
func (c *CostPodCache[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostPodCache)) Add(entry PodEntry[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodEntry))
```

Add adds a PodEntry to the cache. 
####  func (*CostPodCache) [CalculateByteSize](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/cost_aware_memory.go#L191) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostPodCache.CalculateByteSize "Go to CostPodCache.CalculateByteSize")

```
func (c *CostPodCache[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostPodCache)) CalculateByteSize(keyStr string[](https://pkg.go.dev/builtin#string)) int64[](https://pkg.go.dev/builtin#int64)
```

CalculateByteSize estimates memory usage for ristretto cost calculation. This is an approximation used for cache eviction decisions. 
####  func (*CostPodCache) [Delete](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/cost_aware_memory.go#L178) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostPodCache.Delete "Go to CostPodCache.Delete")

```
func (c *CostPodCache[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostPodCache)) Delete(entry PodEntry[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodEntry))
```

Delete removes a PodEntry from the cache. 
####  func (*CostPodCache) [Len](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/cost_aware_memory.go#L185) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostPodCache.Len "Go to CostPodCache.Len")

```
func (c *CostPodCache[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostPodCache)) Len() int[](https://pkg.go.dev/builtin#int)
```

Len returns the number of entries in the cache. 
#### 

```
type GroupCatalog struct {
	// contains filtered or unexported fields
}
```

GroupCatalog is a thread-safe catalog of per-pod KV cache group metadata. 
#### 

```
func NewGroupCatalog() *GroupCatalog[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#GroupCatalog)
```

NewGroupCatalog creates a new, empty GroupCatalog. 
####  func (*GroupCatalog) [Get](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/hma.go#L56) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#GroupCatalog.Get "Go to GroupCatalog.Get")

```
func (c *GroupCatalog[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#GroupCatalog)) Get(podID string[](https://pkg.go.dev/builtin#string), g GroupID[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#GroupID)) (GroupMetadata[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#GroupMetadata), bool[](https://pkg.go.dev/builtin#bool))
```

Get returns the metadata for a pod group. 
####  func (*GroupCatalog) [Learn](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/hma.go#L45) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#GroupCatalog.Learn "Go to GroupCatalog.Learn")

```
func (c *GroupCatalog[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#GroupCatalog)) Learn(podID string[](https://pkg.go.dev/builtin#string), g GroupID[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#GroupID), meta GroupMetadata[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#GroupMetadata))
```

Learn records group metadata for a pod. 
#### 

```
type GroupID int[](https://pkg.go.dev/builtin#int)
```

GroupID identifies a vLLM KV cache group. 
#### 

```
type GroupMetadata struct {
	BlockSize         int[](https://pkg.go.dev/builtin#int)
	SlidingWindowSize *int[](https://pkg.go.dev/builtin#int)
}
```

GroupMetadata holds per-group KV cache spec info learned from BlockStored events. 
#### 

```
type InMemoryIndex struct {
	// contains filtered or unexported fields
}
```

InMemoryIndex is an in-memory implementation of the Index interface. 
#### 

```
func NewInMemoryIndex(cfg *InMemoryIndexConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndexConfig)) (*InMemoryIndex[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndex), error[](https://pkg.go.dev/builtin#error))
```

NewInMemoryIndex creates a new InMemoryIndex instance. 
####  func (*InMemoryIndex) [Add](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/in_memory.go#L157) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndex.Add "Go to InMemoryIndex.Add")

```
func (m *InMemoryIndex[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndex)) Add(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), engineKeys, requestKeys []BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash), entries []PodEntry[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodEntry)) error[](https://pkg.go.dev/builtin#error)
```

Add adds a set of engineKeys/requestKeys and their associated pod entries to the index backend. If engineKeys is nil, only requestKey -> PodEntry mappings are created (no engineKey -> requestKey mapping). This is used for speculative entries where engine keys are not yet known. When engineKeys is non-nil, the mapping type is inferred from the ratio of array lengths. 
####  func (*InMemoryIndex) [Clear](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/in_memory.go#L318) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndex.Clear "Go to InMemoryIndex.Clear")

```
func (m *InMemoryIndex[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndex)) Clear(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), podIdentifier string[](https://pkg.go.dev/builtin#string)) error[](https://pkg.go.dev/builtin#error)
```

Clear removes every entry for the pod from the index, across all device tiers. O(N) over the index, but Clear is rare and off the Lookup/Add hot path. Reuses evictPodsFromRequestKey for race-safe removal, and holds no global lock — only each PodCache's mu, briefly — so it does not stall Lookup. 
The engineKey->requestKey mapping (engineToRequestKeys) is intentionally left untouched: it is LRU-bounded, self-heals when the pod re-Adds the same prefixes, and any stale mapping resolves to an emptied request key that correctly breaks the prefix chain in Lookup. 
####  func (*InMemoryIndex) [Evict](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/in_memory.go#L231) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndex.Evict "Go to InMemoryIndex.Evict")

```
func (m *InMemoryIndex[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndex)) Evict(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), key BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash), keyType KeyType[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#KeyType), entries []PodEntry[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodEntry)) error[](https://pkg.go.dev/builtin#error)
```

Evict removes a key and its associated pod entries from the index backend. keyType indicates whether the key is an EngineKey (requires engine→request lookup) or a RequestKey (used directly for speculative entries without engineKey mapping). 
####  func (*InMemoryIndex) [GetRequestKey](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/in_memory.go#L349) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndex.GetRequestKey "Go to InMemoryIndex.GetRequestKey")

```
func (m *InMemoryIndex[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndex)) GetRequestKey(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), engineKey BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash)) (BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash), error[](https://pkg.go.dev/builtin#error))
```

GetRequestKey returns the last request key (highest index in the chain) associated with the given engineKey. This is what Pool uses for parent hash resolution. Returns an error if the engineKey mapping is missing (e.g., already evicted). 
####  func (*InMemoryIndex) [Lookup](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/in_memory.go#L110) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndex.Lookup "Go to InMemoryIndex.Lookup")

```
func (m *InMemoryIndex[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndex)) Lookup(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), requestKeys []BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash),
	podIdentifierSet sets[](https://pkg.go.dev/k8s.io/apimachinery/pkg/util/sets).Set[](https://pkg.go.dev/k8s.io/apimachinery/pkg/util/sets#Set)[string[](https://pkg.go.dev/builtin#string)],
) (map[BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash)][]PodEntry[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodEntry), error[](https://pkg.go.dev/builtin#error))
```

Lookup receives a list of requestKeys and a set of pod identifiers, and retrieves the filtered pods associated with those keys. The filtering is done based on the pod identifiers provided. If the podIdentifierSet is empty, all pods are returned. 
It returns: 1. A map where the keys are those in (1) and the values are pod-identifiers. 2. An error if any occurred during the operation. 
#### 

```
type InMemoryIndexConfig struct {
	// Size is the maximum number of keys that can be stored in the index.
	Size int[](https://pkg.go.dev/builtin#int) `json:"size"`
	// PodCacheSize is the maximum number of pod entries per key.
	PodCacheSize int[](https://pkg.go.dev/builtin#int) `json:"podCacheSize"`
}
```

InMemoryIndexConfig holds the configuration for the InMemoryIndex. 
#### 

```
func DefaultInMemoryIndexConfig() *InMemoryIndexConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndexConfig)
```

DefaultInMemoryIndexConfig returns a default configuration for the InMemoryIndex. 
#### 

```
type Index interface {
	// Lookup receives a list of keys and a set of pod identifiers,
	// and retrieves the filtered pods associated with those keys.
	// The filtering is done based on the pod identifiers provided.
	// If the podIdentifierSet is empty, all pods are returned.
	//
	// It returns:
	// 1. A map where the keys are those in requestKeys and the values are pod-identifiers.
	// 2. An error if any occurred during the operation.
	Lookup(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), requestKeys []BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash), podIdentifierSet sets[](https://pkg.go.dev/k8s.io/apimachinery/pkg/util/sets).Set[](https://pkg.go.dev/k8s.io/apimachinery/pkg/util/sets#Set)[string[](https://pkg.go.dev/builtin#string)]) (map[BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash)][]PodEntry[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodEntry), error[](https://pkg.go.dev/builtin#error))
	// Add stores requestKey -> pod entries and (optionally) engineKey -> requestKey
	// mappings. If engineKeys is nil, only requestKey -> pod mappings are created
	// (used for speculative entries where engine keys are not yet known).
	//
	// When engineKeys is non-nil, the backend infers the mapping from the ratio
	// of len(engineKeys) to len(requestKeys). Both lengths derive from the same
	// token count divided by their respective block sizes, so they always divide
	// evenly. Examples with 256 tokens:
	//
	//   1:1   (engine=64, canonical=64)  -> 4 eng, 4 req  -> E0->R0, E1->R1, ...
	//   many:1 (engine=16, canonical=64) -> 16 eng, 4 req -> E0..E3->R0, E4..E7->R1, ...
	//   1:many (engine=128, canonical=64) -> 2 eng, 4 req -> E0->[R0,R1], E1->[R2,R3]
	Add(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), engineKeys, requestKeys []BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash), entries []PodEntry[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodEntry)) error[](https://pkg.go.dev/builtin#error)
	// Evict removes a key and its associated pod entries from the index backend.
	// keyType indicates whether the key is an EngineKey (requires engine→request lookup)
	// or a RequestKey (used directly).
	Evict(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), key BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash), keyType KeyType[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#KeyType), entries []PodEntry[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodEntry)) error[](https://pkg.go.dev/builtin#error)
	// GetRequestKey returns the requestKey associated with the given engineKey.
	GetRequestKey(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), engineKey BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash)) (BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash), error[](https://pkg.go.dev/builtin#error))
	// Clear removes all index entries for the given pod, across every device tier.
	// It backs the AllBlocksCleared KV-event (a vLLM prefix-cache reset, e.g. after
	// an RLHF weight update), which is pod-wide — vLLM emits it with no tier. Clear is
	// O(N) over the index but runs off the Lookup/Add hot path, at a coarse cadence
	// (typically once per weight sync).
	Clear(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), podIdentifier string[](https://pkg.go.dev/builtin#string)) error[](https://pkg.go.dev/builtin#error)
}
```

Index defines the interface for a backend that manages KV-block indexing. 
An index backend is a data store that will aggregate possibly the entire global KV cache block index, and will be used to retrieve pod-localities for a given set of consecutive keys that constitute a prefix-cache hit. The hit may not necessarily be on all keys, but of the longest prefix match. 
The index backend allows efficient tracking of which vLLM engines hold which KV-blocks, on what device tier, and when they were last updated. 
Index operations are thread-safe and can be performed concurrently. 
#### 

```
func NewIndex(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), cfg *IndexConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#IndexConfig)) (Index[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#Index), error[](https://pkg.go.dev/builtin#error))
```

NewIndex creates a new Index instance. 
#### 

```
func NewInstrumentedIndex(next Index[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#Index)) Index[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#Index)
```

NewInstrumentedIndex wraps an Index and emits metrics for Add, Evict, and Lookup. 
#### 

```
func NewRedisIndex(config *RedisIndexConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndexConfig)) (Index[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#Index), error[](https://pkg.go.dev/builtin#error))
```

NewRedisIndex creates a new RedisIndex instance. 
#### 

```
func NewTracedIndex(next Index[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#Index)) Index[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#Index)
```

NewTracedIndex wraps an Index and emits OpenTelemetry traces for index operations. This encapsulates all tracing logic for the kvblock.Index interface. 
#### 

```
type IndexConfig struct {
	// InMemoryConfig holds the configuration for the in-memory index.
	InMemoryConfig *InMemoryIndexConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#InMemoryIndexConfig) `json:"inMemoryConfig"`
	// RedisConfig holds the configuration for the Redis index.
	RedisConfig *RedisIndexConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndexConfig) `json:"redisConfig"`
	// CostAwareMemoryConfig holds the configuration for the cost-aware memory index.
	CostAwareMemoryConfig *CostAwareMemoryIndexConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#CostAwareMemoryIndexConfig) `json:"costAwareMemoryConfig"`

	// EnableMetrics toggles whether admissions/evictions/hits/misses are
	// recorded.
	EnableMetrics bool[](https://pkg.go.dev/builtin#bool) `json:"enableMetrics"`
	// MetricsLoggingInterval defines the interval at which metrics are logged.
	// If zero, metrics logging is disabled.
	// Requires `EnableMetrics` to be true.
	MetricsLoggingInterval time[](https://pkg.go.dev/time).Duration[](https://pkg.go.dev/time#Duration) `json:"metricsLoggingInterval"`
}
```

IndexConfig holds the configuration for the KV-block index. It may configure several backends such as listed within the struct. If multiple backends are configured, only the first one will be used. 
#### 

```
func DefaultIndexConfig() *IndexConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#IndexConfig)
```

DefaultIndexConfig returns a default configuration for the KV-block index. 
#### 

```
type KeyType int[](https://pkg.go.dev/builtin#int)
```

KeyType indicates whether a key passed to Evict is an engine key or a request key. 

```
const (
	// EngineKey means the key is an engine-assigned key that must be resolved
	// to a request key via the engineToRequestKeys mapping.
	EngineKey KeyType[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#KeyType) = iota[](https://pkg.go.dev/builtin#iota)
	// RequestKey means the key is a request key and can be used directly.
	// This is used for speculative entries that were added without engineKey mapping.
	RequestKey
)
```

#### 

```
type MMHash struct {
}
```

MMHash represents a single multimodal content hash entry. This matches vLLM's per-block extra_keys format where each entry is the mm_feature.identifier string for an overlapping multimodal item. 
#### 

```
type PlaceholderRange struct {
	Offset int[](https://pkg.go.dev/builtin#int) // absolute start token index
	Length int[](https://pkg.go.dev/builtin#int) // number of placeholder tokens
}
```

PlaceholderRange describes a contiguous range of placeholder tokens for one multimodal item within the full token sequence. 
#### 

```
type PodCache struct {
	// contains filtered or unexported fields
}
```

PodCache represents a cache for pod entries. 
#### 

```
type PodEntry struct {
	// PodIdentifier is the unique identifier for the pod.
	PodIdentifier string[](https://pkg.go.dev/builtin#string)
	// DeviceTier is the tier of the device where the KV-block is stored.
	DeviceTier string[](https://pkg.go.dev/builtin#string)
	// Speculative indicates the entry was added predictively before a KV event confirmed it.
	Speculative bool[](https://pkg.go.dev/builtin#bool)
	// HasGroup indicates GroupIdx identifies a vLLM KV cache group.
	HasGroup bool[](https://pkg.go.dev/builtin#bool)
	// GroupIdx identifies the vLLM KV cache group for HMA events.
	GroupIdx GroupID[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#GroupID)
}
```

PodEntry struct represents a pod entry in the KV-block index. 
####  func (*PodEntry) [String](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/index.go#L188) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodEntry.String "Go to PodEntry.String")

```
func (e *PodEntry[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodEntry)) String() string[](https://pkg.go.dev/builtin#string)
```

String returns a string representation of the PodEntry. 
#### 

```
type RedisIndex struct {
	RedisClient *redis[](https://pkg.go.dev/github.com/redis/go-redis/v9).Client[](https://pkg.go.dev/github.com/redis/go-redis/v9#Client)
}
```

RedisIndex implements the Index interface using Redis as the backend for KV block indexing. 
####  func (*RedisIndex) [Add](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/redis.go#L178) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndex.Add "Go to RedisIndex.Add")

```
func (r *RedisIndex[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndex)) Add(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), engineKeys, requestKeys []BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash), entries []PodEntry[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodEntry)) error[](https://pkg.go.dev/builtin#error)
```

Add adds a set of keys and their associated pod entries to the index backend. If engineKeys is nil, only requestKey -> PodEntry mappings are created (no engineKey -> requestKey mapping). This is used for speculative entries where engine keys are not yet known. When engineKeys is non-nil, the mapping type is inferred from the ratio of array lengths. 
####  func (*RedisIndex) [Clear](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/redis.go#L351) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndex.Clear "Go to RedisIndex.Clear")

```
func (r *RedisIndex[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndex)) Clear(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), podIdentifier string[](https://pkg.go.dev/builtin#string)) error[](https://pkg.go.dev/builtin#error)
```

Clear removes every hash field for the pod across all request-key hashes and device tiers. Each field is a JSON-encoded PodEntry, so matching decodes the field and compares PodIdentifier — catching every tier, group, and speculative variant. It pages the keyspace with SCAN (skipping engine: keys), HDELs the pod's fields, and prunes now-empty hashes. O(keyspace), but Clear is rare and off the Lookup/Add hot path. Because it deletes from the shared store, it is correct for multi-replica deployments with no cross-process coordination. 
####  func (*RedisIndex) [Evict](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/redis.go#L221) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndex.Evict "Go to RedisIndex.Evict")

```
func (r *RedisIndex[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndex)) Evict(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), key BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash), keyType KeyType[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#KeyType), entries []PodEntry[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodEntry)) error[](https://pkg.go.dev/builtin#error)
```

Evict removes a key and its associated pod entries from the index backend. keyType indicates whether the key is an EngineKey (requires engine→request lookup) or a RequestKey (used directly for speculative entries without engineKey mapping). 
####  func (*RedisIndex) [GetRequestKey](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/redis.go#L319) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndex.GetRequestKey "Go to RedisIndex.GetRequestKey")

```
func (r *RedisIndex[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndex)) GetRequestKey(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), engineKey BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash)) (BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash), error[](https://pkg.go.dev/builtin#error))
```

GetRequestKey returns the last request key (highest score) associated with the given engineKey. 
####  func (*RedisIndex) [Lookup](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/redis.go#L112) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndex.Lookup "Go to RedisIndex.Lookup")

```
func (r *RedisIndex[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndex)) Lookup(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), requestKeys []BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash),
	podIdentifierSet sets[](https://pkg.go.dev/k8s.io/apimachinery/pkg/util/sets).Set[](https://pkg.go.dev/k8s.io/apimachinery/pkg/util/sets#Set)[string[](https://pkg.go.dev/builtin#string)],
) (map[BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash)][]PodEntry[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#PodEntry), error[](https://pkg.go.dev/builtin#error))
```

Lookup receives a list of keys and a set of pod identifiers, and retrieves the filtered pods associated with those keys. The filtering is done based on the pod identifiers provided. If the podIdentifierSet is empty, all pods are returned. 
It returns: 1. A map where the keys are those in (1) and the values are pod-identifiers. 2. An error if any occurred during the operation. 
#### 

```
type RedisIndexConfig struct {
	Address string[](https://pkg.go.dev/builtin#string) `json:"address,omitempty"` // Redis server address
}
```

RedisIndexConfig holds the configuration for the RedisIndex. 
#### 

```
func DefaultRedisIndexConfig() *RedisIndexConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#RedisIndexConfig)
```

#### 

```
type TokenProcessor interface {
	// TokensToKVBlockKeys converts tokens into kv_block.Keys.
	// It accepts an optional parentKey to continue a hash chain.
	// extraFeatures provides per-block multimodal data that taints the hash;
	// nil means text-only (no taint). When non-nil, its length must match the
	// number of token chunks.
	// It returns a slice of generated Keys.
	TokensToKVBlockKeys(
		parentKey BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash), tokens []uint32[](https://pkg.go.dev/builtin#uint32), modelName string[](https://pkg.go.dev/builtin#string),
		extraFeatures []*BlockExtraFeatures[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockExtraFeatures),
	) ([]BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#BlockHash), error[](https://pkg.go.dev/builtin#error))

	// BlockSize returns the number of tokens per block used by this processor.
	BlockSize() int[](https://pkg.go.dev/builtin#int)
}
```

TokenProcessor defines the interface for converting tokens to KVBlockKeys. 
#### 

```
func NewChunkedTokenDatabase(config *TokenProcessorConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#TokenProcessorConfig)) (TokenProcessor[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#TokenProcessor), error[](https://pkg.go.dev/builtin#error))
```

NewChunkedTokenDatabase creates a new instance with the given config and metadata. 
#### 

```
type TokenProcessorConfig struct {
	// BlockSize is deprecated. Use BlockSizeTokens instead.
	//
	// Deprecated: Use BlockSizeTokens instead.
	BlockSize int[](https://pkg.go.dev/builtin#int) `json:"blockSize,omitempty"`
	// BlockSizeTokens is the number of tokens per block.
	// A value of zero is treated as "not set" and resolved to the default (16) by NewChunkedTokenDatabase.
	BlockSizeTokens int[](https://pkg.go.dev/builtin#int) `json:"blockSizeTokens"`
	// HashSeed is used to prefix initial hash chunks, similarly to vLLM's NONE_HASH.
	// This should be aligned with vLLM's `PYTHONHASHSEED` environment variable.
	// The system's deployer is responsible for aligning the vLLM deployments
	// with the same seed value.
	HashSeed string[](https://pkg.go.dev/builtin#string) `json:"hashSeed"`
	// contains filtered or unexported fields
}
```

TokenProcessorConfig holds the configuration for the token processor. 
#### 

```
func DefaultTokenProcessorConfig() *TokenProcessorConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#TokenProcessorConfig)
```

DefaultTokenProcessorConfig returns the default configuration for the token processor. 
##  Source Files [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache/kvblock#section-sourcefiles "Go to Source Files")
[View all Source files](https://github.com/llm-d/llm-d-router/tree/v0.10.0/pkg/kvcache/kvblock)


Click to show internal directories. 
Click to hide internal directories. 
go.dev uses cookies from Google to deliver and enhance the quality of its services and to analyze traffic. [Learn more.](https://policies.google.com/technologies/cookies)
Okay
