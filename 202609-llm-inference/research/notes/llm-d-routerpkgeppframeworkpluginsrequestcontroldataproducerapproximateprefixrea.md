---
title: llm-d-router/pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/README.md
  at main · llm-d/llm-d-router · GitHub
id: llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducerapproximateprefixrea
tags:
- llmd-kv-cache-routing-c665d5
- locus-cache-salt-lora-multitenant-routing
- approximate-prefix-routing
created: '2026-09-14T12:42:09.471537Z'
updated: '2026-09-15T04:55:24.129259Z'
source: https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/README.md
source_domain: github.com
fetched_at: '2026-09-14T12:42:09.451878Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'llm-d-router approx-prefix-cache-producer README: hashes token IDs from
  request.Body.TokenizedPrompt into fixed-size blocks and keeps a per-endpoint LRU
  of recently routed prefixes; auto-creates a tokenizer-free estimate token-producer
  when none is configured; autoTune infers blockSizeTokens and lruCapacityPerServer
  (default 31250) from metrics; character-based blockSize is deprecated; blind to
  engine eviction, cache_salt and LoRA identity.'
---

*Suggested by [[llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducer-at-main-llm-dllm-d]]*

[Skip to content](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/README.md#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/README.md) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/README.md) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/README.md) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/README.md).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 367 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  336 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# README.md
Copy path
More file actions
More file actions
## Latest commit
[epp: add multicluster scheduling plugin variants (](https://github.com/llm-d/llm-d-router/commit/38e3412cdcb55ed78560af1ed921bb97f51f3f21)[#2232](https://github.com/llm-d/llm-d-router/pull/2232)[)](https://github.com/llm-d/llm-d-router/commit/38e3412cdcb55ed78560af1ed921bb97f51f3f21)
Open commit detailssuccess
Aug 4, 2026
[38e3412](https://github.com/llm-d/llm-d-router/commit/38e3412cdcb55ed78560af1ed921bb97f51f3f21) · Aug 4, 2026
## History
[History](https://github.com/llm-d/llm-d-router/commits/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/README.md)
Open commit details
History
62 lines (48 loc) · 3.99 KB
Code owner: @liu-cong, @vMaroon, and @llm-d/router-maintainers
/
# README.md
Copy path
## File metadata and controls
  * Preview
  * 

62 lines (48 loc) · 3.99 KB
Code owner: @liu-cong, @vMaroon, and @llm-d/router-maintainers
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Outline
Edit and raw actions
# Approximate Prefix Cache Producer Plugin
**Type:** `approx-prefix-cache-producer`
Prepares per-endpoint prefix cache match data consumed by the `prefix-cache-affinity-filter` and `prefix-cache-scorer`. Runs in the request handling's `DataProducer` phase before scheduling.
For each request, the plugin consumes `request.Body.TokenizedPrompt` (token IDs), hashes the token IDs into fixed-size blocks, and looks up which endpoints have recently served requests with a matching prefix. It writes a `PrefixCacheMatchInfo` attribute onto each candidate endpoint, then records the selected endpoint(s) in the index after scheduling completes (via `PreRequest`).
`TokenizedPrompt` is produced by a `token-producer`. When none is configured, the framework auto-creates one with the tokenizer-free `estimate` backend, so prefix caching works without extra setup; configure a `token-producer` explicitly to select the vLLM `/render` backend.
**Parameters:**
  * `autoTune` (bool, optional, default: `true`): Infer `blockSizeTokens` and `lruCapacityPerServer` from endpoint metrics when available.
  * `blockSizeTokens` (int, optional, default: `16`): Prefix block size in tokens. Used when `autoTune` is false or endpoint metrics are unavailable; ignored when auto-tuned from metrics. Values below the minimum of `64` are clamped up at request time (see #1158), so the `16` default is effectively `64` absent a larger metric/configured value.
  * `maxPrefixBlocksToMatch` (int, optional, default: `2048`): Maximum number of prefix blocks hashed and matched per request. Not auto-tuned. Applies only when `maxPrefixTokensToMatch` is `0`.
  * `maxPrefixTokensToMatch` (int, optional, default: `131072`): Cap expressed in tokens instead of blocks (`maxBlocks = maxPrefixTokensToMatch / blockSizeTokens`). Not auto-tuned. Takes precedence over `maxPrefixBlocksToMatch` when set (> 0); set to `0` to fall back to the block-based cap. The `131072` default (128K, the context window of large production models such as gpt-oss 120b) is a reasonable upper bound that covers the long-prompt use cases seen in production.


Setting both caps to `0` matches the whole prompt. Prompt length is bounded by the model server's context window, so this caps matching at one context window without needing to name a token count. It raises EPP CPU on long prompts (see [operations](https://github.com/llm-d/llm-d-router/blob/main/docs/operations.md)); to turn prefix matching off, remove this producer from the configuration rather than zeroing the caps.
  * `lruCapacityPerServer` (int, optional, default: `31250`): Per-pod LRU index capacity. Used when `autoTune` is false or endpoint metrics are unavailable; ignored when auto-tuned from metrics.
  * `blockSize` (int, optional): Deprecated — character-based block size. Use `blockSizeTokens` instead.


**Configuration Examples:**
Standard single instance:

```
plugins:
  - type: approx-prefix-cache-producer
    parameters:
      autoTune: true
      lruCapacityPerServer: 1000
```

Configuring multiple named instances (e.g., for tiered caching with different parameters):

```
plugins:
  - name: gpuPrefixProducer
    type: approx-prefix-cache-producer
    parameters:
      blockSizeTokens: 16
  - name: cpuPrefixProducer
    type: approx-prefix-cache-producer
    parameters:
      blockSizeTokens: 64
  - name: gpuPrefixScorer
    type: prefix-cache-scorer
    parameters:
      prefixMatchInfoProducerName: gpuPrefixProducer
  - name: cpuPrefixScorer
    type: prefix-cache-scorer
    parameters:
      prefixMatchInfoProducerName: cpuPrefixProducer
```

## Related Documentation
  * [Prefix Cache Affinity Filter](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity/README.md)


## Multi-cluster support
`multicluster-approx-prefix-cache-producer` is the cluster-scoped variant. Its indexer keys on endpoint identity, so over cluster-endpoints it tracks (cluster, block) and routes a repeated prefix back to its cluster. It hashes 64-token blocks, so a shared prefix must be at least about 64 tokens to match.
You can’t perform that action at this time. 
