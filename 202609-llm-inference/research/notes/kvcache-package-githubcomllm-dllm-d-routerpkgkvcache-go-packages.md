---
title: kvcache package - github.com/llm-d/llm-d-router/pkg/kvcache - Go Packages
id: kvcache-package-githubcomllm-dllm-d-routerpkgkvcache-go-packages
tags:
- llmd-kv-cache-routing-c665d5
- kv-cache-indexer
- longest-prefix-scoring
- llm-d-router
- go-package
created: '2026-09-14T11:53:12.442739Z'
updated: '2026-09-15T04:55:24.017544Z'
source: https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache
source_domain: pkg.go.dev
fetched_at: '2026-09-14T11:53:12.423349Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Go package doc for llm-d-router''s pkg/kvcache (v0.10.0), the read side
  of the KV-cache subsystem. The Indexer (constructed via NewKVCacheIndexer) scores
  model-serving pods by KV-cache locality: ComputeBlockKeysFromTokens runs the injected
  kvblock.TokenProcessor to chunk pre-tokenized input into fixed-size blocks and hashes
  each block while chaining in the previous block''s hash so a key encodes its whole
  prefix (extraFeatures taints the hash with per-block multimodal metadata). ScoreTokens
  then queries the kvblock.Index for which pods hold each block key (optionally restricted
  to a caller-supplied pod set) and reduces the lookup to per-pod scores via a pluggable
  KVBlockScorer. The default LongestPrefixScorer credits each pod for its longest
  run of consecutive block hits starting from block 0, weighted per device tier via
  KVCacheBackendConfig (Name/Weight, e.g. gpu/cpu/disk media). Tokenization is explicitly
  external -- TokenizersPoolConfig/internal tokenization is marked Deprecated in favor
  of calling ScoreTokens with tokens already computed. Index and scorer are wrapped
  with OpenTelemetry tracing (no-op when unconfigured). This package, along with kvblock
  and kvevents, was internalized from llm-d-kv-cache into llm-d-router per PR #1886.'
utility_score: 14.0
---

##  README [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#section-readme "Go to Readme")
### KV-Cache Indexer
Scores model-serving pods by KV-cache locality: given a request's tokens, it determines which pods already hold the corresponding KV blocks and ranks them by longest shared prefix, so the scheduler can route to a pod that maximizes cache reuse.
#### What It Does
The `Indexer` is the read side of the KV-cache subsystem. It turns a tokenized prompt into KV-block keys, looks those keys up in the block index (kept current by the [`kvevents`](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/README.md) subscriber), and produces a per-pod score. The precise-prefix-cache scheduling scorer consumes these scores.
Tokenization happens externally: callers pass tokens in via `ScoreTokens`. The indexer owns block-key computation, index lookup, and scoring.
#### How It Works
  * **Block-key computation.** `ComputeBlockKeysFromTokens` runs the injected [`kvblock.TokenProcessor`](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/README.md) to chunk tokens into fixed-size blocks and hash each block (chaining the previous block's hash so a key encodes its whole prefix). `extraFeatures` taints the hash with per-block multimodal metadata when present.
  * **Lookup.** `ScoreTokens` queries the [`kvblock.Index`](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/README.md) for the pods that hold each block key, optionally restricted to a caller- supplied pod set.
  * **Scoring.** A `KVBlockScorer` reduces the lookup result to per-pod scores. The default `LongestPrefixScorer` credits each pod for its longest run of consecutive block hits starting from block 0, weighted per device tier (`BackendConfigs`), so a pod that holds a longer contiguous prefix ranks higher.
  * **Tracing.** The index and scorer are wrapped with OpenTelemetry instrumentation that is a no-op when tracing is not configured.


#### Key Types  
| Symbol  | Role  |  
| --- | --- |  
| `Indexer`  | Entry point; constructed with `NewKVCacheIndexer(ctx, config, tokenProcessor)`.  |  
| `ScoreTokens`  | Tokens-in scoring: tokens -> block keys -> lookup -> per-pod scores.  |  
| `ComputeBlockKeysFromTokens`  | Tokens -> block keys, without scoring.  |  
| `KVBlockIndex`  | Accessor for the underlying `kvblock.Index`.  |  
|  `KVBlockScorer` / `LongestPrefixScorer`  | Scoring strategy over block-hit results.  |  
| `Config`  | Wires the block-index backend, scorer, and per-tier backend weights.  |  
#### Related Documentation
  * [KV-Block Index](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/README.md) -- block index backends and token processing
  * [KV-Events](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/README.md) -- keeps the index current from engine events
  * [Metrics](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/metrics/README.md) -- index and event metrics


Expand ▾ Collapse ▴
##  Documentation [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#section-documentation "Go to Documentation")
### Index [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#pkg-index "Go to Index")
  *     * [func NewDefaultConfig() (*Config, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#NewDefaultConfig)
  * [type Indexer](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#Indexer)
  *     * [func NewKVCacheIndexer(ctx context.Context, config *Config, tokenProcessor kvblock.TokenProcessor) (*Indexer, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#NewKVCacheIndexer)
  *     * [func (k *Indexer) ComputeBlockKeysFromTokens(ctx context.Context, tokens []uint32, modelName string, ...) ([]kvblock.BlockHash, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#Indexer.ComputeBlockKeysFromTokens)
    * [func (k *Indexer) KVBlockIndex() kvblock.Index](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#Indexer.KVBlockIndex)
    * [func (k *Indexer) Run(ctx context.Context)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#Indexer.Run)
    * [func (k *Indexer) ScoreTokens(ctx context.Context, tokens []uint32, modelName string, ...) (map[string]float64, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#Indexer.ScoreTokens)
  * [type KVBlockScorer](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#KVBlockScorer)
  *     * [func NewKVBlockScorer(config *KVBlockScorerConfig) (KVBlockScorer, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#NewKVBlockScorer)
    * [func NewTracedScorer(next KVBlockScorer) KVBlockScorer](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#NewTracedScorer)
  * [type KVBlockScorerConfig](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#KVBlockScorerConfig)
  *     * [func DefaultKVBlockScorerConfig() *KVBlockScorerConfig](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#DefaultKVBlockScorerConfig)
  * [type KVCacheBackendConfig](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#KVCacheBackendConfig)
  *     * [func DefaultKVCacheBackendConfig() []*KVCacheBackendConfig](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#DefaultKVCacheBackendConfig)
  * [type KVScoringStrategy](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#KVScoringStrategy)
  * [type LongestPrefixScorer](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#LongestPrefixScorer)
  *     * [func (s *LongestPrefixScorer) Score(_ context.Context, keys []kvblock.BlockHash, ...) (map[string]float64, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#LongestPrefixScorer.Score)
    * [func (s *LongestPrefixScorer) Strategy() KVScoringStrategy](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#LongestPrefixScorer.Strategy)


### Constants [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#pkg-constants "Go to Constants")
This section is empty.
### Variables [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#pkg-variables "Go to Variables")
This section is empty.
### Functions [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#pkg-functions "Go to Functions")
This section is empty.
### Types [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#pkg-types "Go to Types")
#### 

```
type Config struct {
	KVBlockIndexConfig  *kvblock[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock).IndexConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock#IndexConfig)    `json:"kvBlockIndexConfig"`
	KVBlockScorerConfig *KVBlockScorerConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#KVBlockScorerConfig)    // not exported
	BackendConfigs      []*KVCacheBackendConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#KVCacheBackendConfig) `json:"kvCacheBackendConfigs"`

	// TokenizersPoolConfig configured the deprecated in-process tokenization
	// pool. The pool itself lives in llm-d-kv-cache; the indexer no longer
	// consumes it, but the field is retained so existing configurations keep
	// parsing. The precise-prefix-cache legacy producer reads it.
	//
	// Deprecated: tokenize externally and call Indexer.ScoreTokens.
	TokenizersPoolConfig *kvctok[](https://pkg.go.dev/github.com/llm-d/llm-d-kv-cache/pkg/tokenization).Config[](https://pkg.go.dev/github.com/llm-d/llm-d-kv-cache/pkg/tokenization#Config) `json:"tokenizersPoolConfig,omitempty"`
}
```

Config holds the configuration for the Indexer module. The configuration cover the different components found in the Indexer module. 
#### 

```
func NewDefaultConfig() (*Config[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#Config), error[](https://pkg.go.dev/builtin#error))
```

NewDefaultConfig returns a default configuration for the Indexer module. 
#### 

```
type Indexer struct {
	// contains filtered or unexported fields
}
```

Indexer is a concrete implementation of the KVCacheIndex interface. 
#### 

```
func NewKVCacheIndexer(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), config *Config[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#Config), tokenProcessor kvblock[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock).TokenProcessor[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock#TokenProcessor)) (*Indexer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#Indexer), error[](https://pkg.go.dev/builtin#error))
```

NewKVCacheIndexer creates a KVCacheIndex given a Config. Callers tokenize externally and use the tokens-in API (Indexer.ScoreTokens). 
####  func (*Indexer) [ComputeBlockKeysFromTokens](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/indexer.go#L123) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#Indexer.ComputeBlockKeysFromTokens "Go to Indexer.ComputeBlockKeysFromTokens")

```
func (k *Indexer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#Indexer)) ComputeBlockKeysFromTokens(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), tokens []uint32[](https://pkg.go.dev/builtin#uint32), modelName string[](https://pkg.go.dev/builtin#string),
	extraFeatures []*kvblock[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock).BlockExtraFeatures[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock#BlockExtraFeatures),
) ([]kvblock[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock).BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock#BlockHash), error[](https://pkg.go.dev/builtin#error))
```

ComputeBlockKeysFromTokens computes the KV-block keys for a pre-tokenized prompt. Callers tokenize and truncate externally. extraFeatures provides per-block multimodal data that taints the hash; nil means text-only. 
####  func (*Indexer) [KVBlockIndex](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/indexer.go#L116) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#Indexer.KVBlockIndex "Go to Indexer.KVBlockIndex")

```
func (k *Indexer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#Indexer)) KVBlockIndex() kvblock[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock).Index[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock#Index)
```

KVBlockIndex returns the kvblock.Index used by the Indexer. 
####  func (*Indexer) [Run](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/indexer.go#L111) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#Indexer.Run "Go to Indexer.Run")

```
func (k *Indexer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#Indexer)) Run(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context))
```

Run starts the indexer. Blocks until ctx is cancelled. 
####  func (*Indexer) [ScoreTokens](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/indexer.go#L149) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#Indexer.ScoreTokens "Go to Indexer.ScoreTokens")

```
func (k *Indexer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#Indexer)) ScoreTokens(
	ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context),
	tokens []uint32[](https://pkg.go.dev/builtin#uint32),
	modelName string[](https://pkg.go.dev/builtin#string),
	podIdentifiers []string[](https://pkg.go.dev/builtin#string),
	extraFeatures []*kvblock[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock).BlockExtraFeatures[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock#BlockExtraFeatures),
) (map[string[](https://pkg.go.dev/builtin#string)]float64[](https://pkg.go.dev/builtin#float64), error[](https://pkg.go.dev/builtin#error))
```

ScoreTokens computes pod scores for the given tokens and model. It converts tokens into KV block keys, looks up which pods hold matching blocks in the index, and scores each pod based on cache hits. 
extraFeatures provides per-block multimodal data that taints the hash; nil means text-only. podIdentifiers limits scoring to the given pod addresses. If empty, all pods are considered. 
#### 

```
type KVBlockScorer interface {
	// Strategy returns the scoring strategy type.
	Strategy() KVScoringStrategy[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#KVScoringStrategy)
	// Score scores the blocks based on the scoring strategy.
	// It returns a map of pod names to their scores.
	Score(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), keys []kvblock[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock).BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock#BlockHash),
		keyToPods map[kvblock[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock).BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock#BlockHash)][]kvblock[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock).PodEntry[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock#PodEntry)) (map[string[](https://pkg.go.dev/builtin#string)]float64[](https://pkg.go.dev/builtin#float64), error[](https://pkg.go.dev/builtin#error))
}
```

KVBlockScorer defines the interface for implementing a KV block scoring strategy. 
#### 

```
func NewKVBlockScorer(config *KVBlockScorerConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#KVBlockScorerConfig)) (KVBlockScorer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#KVBlockScorer), error[](https://pkg.go.dev/builtin#error))
```

NewKVBlockScorer creates a new KVBlockScorer based on the provided strategy. 
#### 

```
func NewTracedScorer(next KVBlockScorer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#KVBlockScorer)) KVBlockScorer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#KVBlockScorer)
```

NewTracedScorer wraps a KVBlockScorer and emits OpenTelemetry traces for Score operations. This encapsulates all tracing logic for the KVBlockScorer interface. 
#### 

```
type KVBlockScorerConfig struct {
	ScoringStrategy KVScoringStrategy[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#KVScoringStrategy)
	BackendConfigs  []*KVCacheBackendConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#KVCacheBackendConfig) `json:"backendConfigs"`
}
```

KVBlockScorerConfig holds the configuration for the KVBlockScorer. 
#### 

```
func DefaultKVBlockScorerConfig() *KVBlockScorerConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#KVBlockScorerConfig)
```

DefaultKVBlockScorerConfig returns the default configuration for the KVBlockScorer. 
#### 

```
type KVCacheBackendConfig struct {
	// Name is the identifier for this medium (e.g., "gpu", "cpu", "disk")
	Name string[](https://pkg.go.dev/builtin#string) `json:"name"`
	// Weight is the scoring weight for blocks stored on this medium
	Weight float64[](https://pkg.go.dev/builtin#float64) `json:"weight"`
}
```

####  func [DefaultKVCacheBackendConfig](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/backend.go#L26) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#DefaultKVCacheBackendConfig "Go to DefaultKVCacheBackendConfig")

```
func DefaultKVCacheBackendConfig() []*KVCacheBackendConfig[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#KVCacheBackendConfig)
```

#### 

```
type KVScoringStrategy string[](https://pkg.go.dev/builtin#string)
```

KVScoringStrategy defines the strategy used to score pods for KV cache block reuse. 

```
const (
	// LongestPrefixMatch Score by longest consecutive match from start.
	LongestPrefixMatch KVScoringStrategy[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#KVScoringStrategy) = "LongestPrefix"
)
```

#### 

```
type LongestPrefixScorer struct {
	// mediumWeights maps medium/device tier names to their scoring weights
	MediumWeights map[string[](https://pkg.go.dev/builtin#string)]float64[](https://pkg.go.dev/builtin#float64)
}
```

LongestPrefixScorer scores based on longest consecutive block matches count starting from block 0. 
####  func (*LongestPrefixScorer) [Score](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock_scorer.go#L106) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#LongestPrefixScorer.Score "Go to LongestPrefixScorer.Score")

```
func (s *LongestPrefixScorer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#LongestPrefixScorer)) Score(
	_ context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context),
	keys []kvblock[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock).BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock#BlockHash),
	keyToPods map[kvblock[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock).BlockHash[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock#BlockHash)][]kvblock[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock).PodEntry[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/kvblock#PodEntry),
) (map[string[](https://pkg.go.dev/builtin#string)]float64[](https://pkg.go.dev/builtin#float64), error[](https://pkg.go.dev/builtin#error))
```

Score implements the longest prefix scoring logic with weighted sum based on BackendConfig. 
####  func (*LongestPrefixScorer) [Strategy](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock_scorer.go#L85) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#LongestPrefixScorer.Strategy "Go to LongestPrefixScorer.Strategy")

```
func (s *LongestPrefixScorer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#LongestPrefixScorer)) Strategy() KVScoringStrategy[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#KVScoringStrategy)
```

Strategy returns the strategy type: LongestPrefixMatch. 
##  Source Files [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#section-sourcefiles "Go to Source Files")
[View all Source files](https://github.com/llm-d/llm-d-router/tree/v0.10.0/pkg/kvcache)


##  Directories [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvcache#section-directories "Go to Directories")
Show internal  Expand all  
| Path  | Synopsis  |  
| --- | --- |  
|   |  
|   |  
|   |  
|   |  
Click to show internal directories. 
Click to hide internal directories. 
go.dev uses cookies from Google to deliver and enhance the quality of its services and to analyze traffic. [Learn more.](https://policies.google.com/technologies/cookies)
Okay
