---
title: engineadapter package - github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter
  - Go Packages
id: engineadapter-package-githubcomllm-dllm-d-routerpkgkveventsengineadapter-go-pack
tags:
- llmd-kv-cache-routing-c665d5
- engineadapter
- kv-events
- trtllm-gap
created: '2026-09-14T13:00:41.125436Z'
updated: '2026-09-15T04:55:24.133987Z'
source: https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter
source_domain: pkg.go.dev
fetched_at: '2026-09-14T13:00:41.107329Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Go package doc (pkg.go.dev, resolves to v0.10.0) for llm-d-router''s pkg/kvevents/engineadapter.
  Confirms only two engine adapters exist: VLLMAdapter (NewVLLMAdapter, EngineTypeVLLM=''vllm'')
  and SGLangAdapter (NewSGLangAdapter, EngineTypeSGLang=''sglang''); NewAdapter(engineType)
  supports only ''vllm'' (default) and ''sglang'' -- no TensorRT-LLM/trtllm adapter
  exists in this package as of v0.10.0. Notes vLLM emits events either as positional
  msgpack arrays (msgspec array_like=True) or, since vllm-project/vllm#42892, as field-name
  maps tagged under ''type''; SGLang is documented as using ''the same msgpack wire
  format as vLLM but may omit trailing optional fields.'''
utility_score: 14.0
---

##  README [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#section-readme "Go to Readme")
### Engine Adapters
Decodes the KV-cache event messages that model-serving engines publish into the canonical events the [`kvevents.Pool`](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/README.md) applies to the block index. Each engine has its own topic scheme and wire format; an adapter hides those differences behind one interface.
#### What It Does
Given a raw ZMQ message (topic plus payload), an adapter parses the topic to recover the model and sharding key, decodes the payload, and returns canonical block-stored / block-removed / all-blocks-cleared events. It also exposes the sharding key so the pool can order events for the same block.
#### Adapters  
| Engine  | Constructor  | Notes  |  
| --- | --- | --- |  
| vLLM  | `NewVLLMAdapter`  | msgpack payloads; supports HMA group identity and map-encoded events.  |  
| SGLang  | `NewSGLangAdapter`  | SGLang's topic scheme and payload format.  |  
`NewAdapter` selects the adapter for a given engine type.
#### Related Documentation
  * [KV-Events](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/README.md) -- consumes the decoded events
  * [KV-Block Index](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvcache/kvblock/README.md) -- where the events land


Expand ▾ Collapse ▴
##  Documentation [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#section-documentation "Go to Documentation")
### Overview [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#pkg-overview "Go to Overview")
Package engineadapter provides engine-specific implementations of the kvevents.EngineAdapter interface. Each inference engine (e.g. vLLM, SGLang) has its own adapter that knows how to parse raw transport messages into domain events. 
### Index [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#pkg-index "Go to Index")
  * [func NewAdapter(engineType string) (kvevents.EngineAdapter, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#NewAdapter)
  * [type SGLangAdapter](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#SGLangAdapter)
  *     * [func NewSGLangAdapter() *SGLangAdapter](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#NewSGLangAdapter)
  *     * [func (s *SGLangAdapter) ParseMessage(msg *kvevents.RawMessage) (string, string, kvevents.EventBatch, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#SGLangAdapter.ParseMessage)
    * [func (s *SGLangAdapter) ShardingKey(msg *kvevents.RawMessage) string](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#SGLangAdapter.ShardingKey)
  * [type VLLMAdapter](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#VLLMAdapter)
  *     * [func NewVLLMAdapter() *VLLMAdapter](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#NewVLLMAdapter)
  *     * [func (v *VLLMAdapter) ParseMessage(msg *kvevents.RawMessage) (string, string, kvevents.EventBatch, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#VLLMAdapter.ParseMessage)
    * [func (v *VLLMAdapter) ShardingKey(msg *kvevents.RawMessage) string](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#VLLMAdapter.ShardingKey)


### Constants [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#pkg-constants "Go to Constants")

```
const (
	// EngineTypeVLLM selects the vLLM adapter.
	EngineTypeVLLM = "vllm"
	// EngineTypeSGLang selects the SGLang adapter.
	EngineTypeSGLang = "sglang"
)
```

### Variables [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#pkg-variables "Go to Variables")
This section is empty.
### Functions [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#pkg-functions "Go to Functions")
#### 

```
func NewAdapter(engineType string[](https://pkg.go.dev/builtin#string)) (kvevents[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvevents).EngineAdapter[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvevents#EngineAdapter), error[](https://pkg.go.dev/builtin#error))
```

NewAdapter creates an EngineAdapter for the given engine type. Supported types: "vllm" (default), "sglang". 
### Types [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#pkg-types "Go to Types")
#### 

```
type SGLangAdapter struct {
	// contains filtered or unexported fields
}
```

SGLangAdapter implements the kvevents.EngineAdapter interface for SGLang engines. SGLang uses the same msgpack wire format as vLLM but may omit trailing optional fields. 
#### 

```
func NewSGLangAdapter() *SGLangAdapter[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#SGLangAdapter)
```

NewSGLangAdapter creates a new SGLang adapter. 
####  func (*SGLangAdapter) [ParseMessage](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/engineadapter/sglang_adapter.go#L71) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#SGLangAdapter.ParseMessage "Go to SGLangAdapter.ParseMessage")

```
func (s *SGLangAdapter[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#SGLangAdapter)) ParseMessage(msg *kvevents[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvevents).RawMessage[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvevents#RawMessage)) (string[](https://pkg.go.dev/builtin#string), string[](https://pkg.go.dev/builtin#string), kvevents[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvevents).EventBatch[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvevents#EventBatch), error[](https://pkg.go.dev/builtin#error))
```

ParseMessage parses a raw transport message into domain data. It extracts pod identity and model name from the topic, and decodes the msgpack payload into an EventBatch. 
####  func (*SGLangAdapter) [ShardingKey](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/engineadapter/sglang_adapter.go#L61) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#SGLangAdapter.ShardingKey "Go to SGLangAdapter.ShardingKey")

```
func (s *SGLangAdapter[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#SGLangAdapter)) ShardingKey(msg *kvevents[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvevents).RawMessage[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvevents#RawMessage)) string[](https://pkg.go.dev/builtin#string)
```

ShardingKey extracts the pod-id segment from a SGLang raw message topic. Expected topic format: "kv@<pod-id>@<model-name>" (same as vLLM). 
#### 

```
type VLLMAdapter struct {
	// contains filtered or unexported fields
}
```

VLLMAdapter implements the kvevents.EngineAdapter interface for vLLM engines. It parses raw transport messages (topic + msgpack payload) into domain events. 
vLLM emits events either as positional msgpack arrays with trailing defaults omitted (msgspec array_like=True) or, since vllm-project/vllm#42892, as field-name maps tagged under "type". Both decode into the same positional []any layout, extracted with length guards instead of fixed structs, so the converters stay encoding-agnostic and tolerate appended or omitted fields. 
#### 

```
func NewVLLMAdapter() *VLLMAdapter[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#VLLMAdapter)
```

NewVLLMAdapter creates a new vLLM adapter. 
####  func (*VLLMAdapter) [ParseMessage](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/engineadapter/vllm_adapter.go#L64) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#VLLMAdapter.ParseMessage "Go to VLLMAdapter.ParseMessage")

```
func (v *VLLMAdapter[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#VLLMAdapter)) ParseMessage(msg *kvevents[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvevents).RawMessage[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvevents#RawMessage)) (string[](https://pkg.go.dev/builtin#string), string[](https://pkg.go.dev/builtin#string), kvevents[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvevents).EventBatch[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvevents#EventBatch), error[](https://pkg.go.dev/builtin#error))
```

ParseMessage parses a raw transport message into domain data. It extracts pod identity and model name from the topic, and decodes the msgpack payload into an EventBatch. 
####  func (*VLLMAdapter) [ShardingKey](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/kvevents/engineadapter/vllm_adapter.go#L54) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#VLLMAdapter.ShardingKey "Go to VLLMAdapter.ShardingKey")

```
func (v *VLLMAdapter[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#VLLMAdapter)) ShardingKey(msg *kvevents[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvevents).RawMessage[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvevents#RawMessage)) string[](https://pkg.go.dev/builtin#string)
```

ShardingKey extracts the pod-id segment from a vLLM raw message topic. Expected topic format: "kv@<pod-id>@<model-name>". 
##  Source Files [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/kvevents/engineadapter#section-sourcefiles "Go to Source Files")
[View all Source files](https://github.com/llm-d/llm-d-router/tree/v0.10.0/pkg/kvevents/engineadapter)


Click to show internal directories. 
Click to hide internal directories. 
go.dev uses cookies from Google to deliver and enhance the quality of its services and to analyze traffic. [Learn more.](https://policies.google.com/technologies/cookies)
Okay
