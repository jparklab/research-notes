---
title: llm-d-router/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/README.md
  at main · llm-d/llm-d-router · GitHub
id: llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducerpreciseprefixcachere
tags:
- llmd-router-nonk8s-backends-54b774
- locus-kv-event-path-off-k8s
- precise-prefix-cache-producer
- endpoint-extractor
- go-source
- llmd-kv-cache-routing-c665d5
created: '2026-09-11T21:23:21.951727Z'
updated: '2026-09-15T04:55:23.919948Z'
source: https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/README.md
source_domain: github.com
fetched_at: '2026-09-11T21:23:21.930175Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'README.md for llm-d-router''s precise-prefix-cache-producer plugin (type:
  precise-prefix-cache-producer). States its per-request pipeline: consume TokenizedPrompt
  from token-producer, hash tokens into KV-block keys, look up kvblock.Index, write
  PrefixCacheMatchInfo per endpoint, and explicitly labels lifecycle hooks as ''(PreRequest)
  Speculative-index the selected endpoint(s) with TTL eviction'' and ''(EndpointExtractor)
  Per-pod ZMQ subscriber lifecycle on add/delete'' — confirming the plugin itself
  implements the router''s EndpointExtractor interface for per-pod subscriber management,
  separate from the request-scoped Produce path. Documents kvEventsConfig (engineType
  vllm/sglang, tracing flag) and the cross-engine requirement that block keys are
  recomputed EPP-side from TokenizedPrompt (token_ids + extra_keys for multimodal/cache_salt),
  with SGLang KV-events currently omitting extra_keys so salted SGLang requests are
  precise-cache misses.'
---

*Suggested by [[llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducer-at-main-llm-dllm-d]] — linked from dataproducer directory README as Precise Prefix Cache Producer doc*

[Skip to content](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/README.md#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/README.md) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/README.md) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/README.md) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/README.md).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# README.md
Copy path
More file actions
More file actions
## Latest commit
[docs: use blockSizeTokens in tokenProcessorConfig samples (](https://github.com/llm-d/llm-d-router/commit/bffcfb783baacbc827b138e7b976c6b40e66b2af)[#2705](https://github.com/llm-d/llm-d-router/pull/2705)[)](https://github.com/llm-d/llm-d-router/commit/bffcfb783baacbc827b138e7b976c6b40e66b2af)
Open commit detailsfailure
Sep 5, 2026
[bffcfb7](https://github.com/llm-d/llm-d-router/commit/bffcfb783baacbc827b138e7b976c6b40e66b2af) · Sep 5, 2026
## History
[History](https://github.com/llm-d/llm-d-router/commits/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/README.md)
Open commit details
History
72 lines (56 loc) · 3.44 KB
Code owner: @bongwoobak, @vMaroon, and @llm-d/router-maintainers
/
# README.md
Copy path
## File metadata and controls
  * Preview
  * 

72 lines (56 loc) · 3.44 KB
Code owner: @bongwoobak, @vMaroon, and @llm-d/router-maintainers
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Outline
Edit and raw actions
# Precise Prefix Cache Producer
**Type:** `precise-prefix-cache-producer`
DataProducer that owns the precise KV-block index and publishes per-endpoint `PrefixCacheMatchInfo`. Pairs with the generic [`prefix-cache-scorer`](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/scheduling/scorer/prefix); the scorer must reference this producer by name:

```
- type: prefix-cache-scorer
  parameters:
    prefixMatchInfoProducerName: precise-prefix-cache-producer
```

Without the `prefixMatchInfoProducerName` field, the scorer falls back to the auto-spawned approx producer.
Pipeline per request:
  * Consume `TokenizedPrompt` from `token-producer`.
  * Hash tokens → KV-block keys → `kvblock.Index.Lookup`.
  * Write `PrefixCacheMatchInfo(matchBlocks, totalBlocks, blockSizeTokens)` per endpoint, including the unweighted cached-block count and its per-device-tier breakdown.
  * (`PreRequest`) Speculative-index the selected endpoint(s) with TTL eviction.
  * (`EndpointExtractor`) Per-pod ZMQ subscriber lifecycle on add/delete.


Requires `TokenizedPrompt` on the request — set by a `token-producer` upstream. No-op otherwise.
## Parameters  
| Parameter  | Type  | Default  | Description  |  
| --- | --- | --- | --- |  
| `tokenProcessorConfig`  | object  | `kvblock.DefaultTokenProcessorConfig()`  | KV-block hashing for the EPP-recomputed keys (block size, hash seed).  |  
| `indexerConfig`  | object  | `kvcache.NewDefaultConfig()`  |  `kvcache.Indexer` config.  |  
| `kvEventsConfig`  | object  | `kvevents.DefaultConfig()`  | KV-events pool config.  |  
| `speculativeIndexing`  | bool  | `false`  | Seed predicted entries on routing decisions.  |  
| `speculativeTTL`  | duration  | TTL for speculative entries.  |  
Set `kvEventsConfig.engineType` to `sglang` for SGLang KV-events. It defaults to `vllm` when omitted.
Set `kvEventsConfig.tracing` to `true` to emit OpenTelemetry spans for the KV-event pipeline (`events_receive`, `events_process`, `events_decode`). It defaults to `false`: KV events arrive at many times the inference request rate, so with a shared head sampler always-on event spans crowd request traces out of the exported volume. The EPP `--tracing` flag gates tracing as a whole, so this field has no effect while that is off.
See [llm-d-kv-cache/docs/configuration.md](https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/configuration.md) for nested parameter details.
## Engine compatibility
Block keys are recomputed by the EPP from `TokenizedPrompt` (tokens, model, multimodal features, cache salt) on both the lookup path and the KV-event ingestion path, using this plugin's `tokenProcessorConfig`. The engine's own block hashes serve only as opaque keys for the engine-to-request mapping, so `blockSizeTokens`/`hashSeed` need not match the engine.
The cross-engine requirement is that the engine emits, in its KV-events, the hash-affecting inputs the EPP hashes: `token_ids`, and `extra_keys` carrying multimodal identifiers and `cache_salt`. An input the engine omits from `extra_keys` is absent on the event side, so requests carrying it do not correlate.  
| Engine  |  `extra_keys` in KV-events  | `cache_salt`  |  
| --- | --- | --- |  
| vLLM  | emitted  | in block-0 `extra_keys`; salted prefixes isolated and precise-routed  |  
| SGLang  | not emitted  | baked into engine block hashes but not surfaced; salted requests are precise-cache misses until SGLang emits `extra_keys`  |  
Salt isolation is enforced by the engine regardless; the above affects only routing accuracy for salted requests.
You can’t perform that action at this time. 
