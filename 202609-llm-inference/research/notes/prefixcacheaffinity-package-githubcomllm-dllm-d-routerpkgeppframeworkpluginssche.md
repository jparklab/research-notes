---
title: prefixcacheaffinity package - github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity
  - Go Packages
id: prefixcacheaffinity-package-githubcomllm-dllm-d-routerpkgeppframeworkpluginssche
tags:
- llmd-kv-cache-routing-c665d5
- locus-affinity-helps-or-hurts
- prefix-cache-affinity-filter
- scheduler-defaults
created: '2026-09-14T12:41:17.652064Z'
updated: '2026-09-15T04:55:24.123605Z'
source: https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity
source_domain: pkg.go.dev
fetched_at: '2026-09-14T12:41:17.633586Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Godoc for llm-d-router''s prefix-cache-affinity-filter plugin (package prefixcacheaffinity,
  github.com/llm-d/llm-d-router pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity,
  v0.10.0). Documents the exported DefaultConfig struct verbatim: AffinityThreshold=0.80,
  ExplorationProbability=0, MaxTTFTPenaltyMs=18000, TTFTSource=prefillThroughput,
  PeakPrefillThroughput=15928 tokens/sec (calibrated for Qwen 32B on 2x H100 80GB
  TP=2 with vLLM 0.19, measured as prefill throughput of one unloaded max_num_batched_tokens=8192
  chunk). Filter narrows candidates to endpoints with prefix cache score >= affinityThreshold
  (''sticky''); if best sticky endpoint''s estimated TTFT (inFlightTokens/peakPrefillThroughput*1000)
  exceeds best non-sticky by more than maxTTFTPenaltyMs, stickiness breaks and all
  endpoints are reopened. Config example shows the plugin wired into a schedulingProfile
  named ''default''.'
---

##  README [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#section-readme "Go to Readme")
### Prefix Cache Affinity Filter (`prefix-cache-affinity-filter`)
**Type:** `prefix-cache-affinity-filter`
#### When to use this filter
Enable this filter when your workload has repeated or similar prompts across requests (e.g., shared system prompts, multi-turn conversations, or RAG pipelines with overlapping context). In these scenarios, vLLM's automatic prefix caching keeps KV cache blocks from previous requests in GPU memory. Without this filter, the default load-balancing strategy may route a request to any endpoint, forcing a full prefill even when another endpoint already has most of the prompt cached. This filter steers requests toward endpoints that hold a cache hit, converting expensive prefill into a near-free cache lookup and significantly reducing time-to-first-token (TTFT).
If your workload consists of unique, non-overlapping prompts, this filter has no effect because no endpoint will accumulate cache hits, and the filter falls through to keeping all candidates (no-op).
#### Difference from `prefix-cache-scorer`
The `prefix-cache-scorer` plugin scores endpoints by prefix cache hit ratio. It works with any picker, but the choice of picker creates a trade-off:
  * **With`max-picker`** (the default): the scorer consistently picks the single highest-scoring endpoint, which maximizes cache hits but causes **hot-spotting** — many concurrent requests with similar prompts all land on the same endpoint, overloading it and degrading TTFT.
  * **With`weighted-random-picker`** : requests spread across endpoints proportional to their cache scores. This avoids hot-spotting but dilutes cache affinity — requests are frequently sent to endpoints with low or zero cache hits, losing the prefill savings that prefix caching provides.


This filter resolves the trade-off by operating as a **pre-filter** rather than a scorer. It narrows the candidate set to only the sticky endpoints (those above `affinityThreshold`), then passes them to downstream plugins. When paired with `weighted-random-picker`, requests are spread across the sticky set — maintaining cache affinity while distributing load. The TTFT load gate (`maxTTFTPenaltyMs`) adds automatic back-off: if sticky endpoints become overloaded and their TTFT exceeds non-sticky endpoints by more than the configured penalty, the filter breaks stickiness and opens up all endpoints, preventing the hot-spotting problem. The exploration mechanism (`explorationProbability`) seeds cache state on other endpoints over time, preventing permanent stickiness to a fixed subset.
#### Overview
Probabilistic filter that narrows candidates to "sticky" endpoints. An endpoint is sticky when it has a high prefix cache score for the current request, meaning the request's prompt (or most of it) is already cached on that endpoint from a previous request with the same or similar prompt. Routing to a sticky endpoint avoids redundant prefill computation, reducing TTFT.
Can be instantiated multiple times with different thresholds (e.g., 0.99 for global gate, 0.80 for within-tier gate).
#### Behavior
  * Keep only endpoints with prefix cache score >= `affinityThreshold`
  * If no endpoints pass, all are kept (no-op)
  * With probability `explorationProbability` (default 0, disabled), skip the gate entirely for exploration
  * TTFT load gate: if best sticky endpoint's TTFT exceeds best non-sticky by more than `maxTTFTPenaltyMs`, break stickiness and keep all endpoints (0 = always stick). The per-endpoint TTFT is estimated from in-flight tokens as `inFlightTokens / peakPrefillThroughput * 1000` (ms) when `ttftSource` is `prefillThroughput` (default), or comes from the latency predictor when `ttftSource` is `latencyPredictor`
  * If no endpoints have the TTFT source attribute (`LatencyPredictionInfo` or `InFlightLoad`), the TTFT load gate is skipped. If no endpoints have `PrefixCacheMatchInfo`, all prefix scores default to 0 and no endpoints pass the affinity threshold, so all are kept (no-op)


#### Config  
| Parameter  | Type  | Required  | Default  | Description  |  
| --- | --- | --- | --- | --- |  
| `affinityThreshold`  | `float64`  | No  | `0.80`  | Prefix cache score threshold for stickiness  |  
| `explorationProbability`  | `float64`  | No  | Probability of skipping the gate  |  
| `maxTTFTPenaltyMs`  | `float64`  | No  | `18000`  | Max TTFT penalty (ms) before breaking stickiness. 0 = always stick  |  
| `ttftSource`  | `string`  | No  | `prefillThroughput`  | TTFT source for the load gate: `prefillThroughput` or `latencyPredictor`  |  
| `peakPrefillThroughput`  | `float64`  | No  | `15928`  | Peak prefill throughput (tokens/sec), used to estimate TTFT when `ttftSource` is `prefillThroughput`  |  
The `peakPrefillThroughput` default of `15928` tokens/sec is calibrated for Qwen 32B on 2x H100 80GB (TP=2) with vLLM 0.19, measured as the prefill throughput of a single unloaded chunk of `max_num_batched_tokens` (8192) tokens. It is hardware-, model-, and serving-stack-specific; retune it for a different deployment, or set `ttftSource` to `latencyPredictor` to source TTFT from the latency predictor instead.
#### Dependencies
  * Reads `PrefixCacheMatchInfo` from endpoint attributes (from `prefix-cache-scorer`)
  * Reads `InFlightLoad` for the TTFT load gate when `ttftSource` is `prefillThroughput` (from `in-flight-load-producer`)
  * Reads `LatencyPredictionInfo` for the TTFT load gate when `ttftSource` is `latencyPredictor` (from `predicted-latency-producer`)


**Configuration Example:**

```
plugins:
  - type: prefix-cache-affinity-filter
    name: prefix-affinity
    parameters:
      affinityThreshold: 0.80
      explorationProbability: 0.01
      maxTTFTPenaltyMs: 5000
      ttftSource: prefillThroughput
schedulingProfiles:
  - name: default
    plugins:
      - pluginRef: prefix-affinity

```

Expand ▾ Collapse ▴
##  Documentation [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#section-documentation "Go to Documentation")
### Overview [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#pkg-overview "Go to Overview")
Package prefixcacheaffinity provides a probabilistic filter that narrows candidates to "sticky" endpoints (those with high prefix cache scores). Can be instantiated multiple times with different thresholds (e.g., 0.99 for global gate, 0.80 for within-tier gate). 
### Index [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#pkg-index "Go to Index")
  * [func Factory(name string, rawParameters *json.Decoder, _ fwkplugin.Handle) (fwkplugin.Plugin, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#Factory)
  *     * [func (p *Plugin) Consumes() fwkplugin.DataDependencies](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#Plugin.Consumes)
    * [func (p *Plugin) Filter(ctx context.Context, _ *fwksched.InferenceRequest, ...) []fwksched.Endpoint](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#Plugin.Filter)
    * [func (p *Plugin) TypedName() fwkplugin.TypedName](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#Plugin.TypedName)
  * [type TTFTSource](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#TTFTSource)


### Constants [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#pkg-constants "Go to Constants")

```
const (
	PluginType = "prefix-cache-affinity-filter"
)
```

### Variables [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#pkg-variables "Go to Variables")

```
var DefaultConfig = Config[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#Config){
	AffinityThreshold:      0.80,
	ExplorationProbability: 0,
	MaxTTFTPenaltyMs:       18000,
	TTFTSource[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#TTFTSource):             TTFTSourcePrefillThroughput[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#TTFTSourcePrefillThroughput),

	PeakPrefillThroughput: 15928,
}
```

### Functions [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#pkg-functions "Go to Functions")
#### 

```
func Factory(name string[](https://pkg.go.dev/builtin#string), rawParameters *json[](https://pkg.go.dev/encoding/json).Decoder[](https://pkg.go.dev/encoding/json#Decoder), _ fwkplugin[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin).Handle[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin#Handle)) (fwkplugin[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin).Plugin[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin#Plugin), error[](https://pkg.go.dev/builtin#error))
```

### Types [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#pkg-types "Go to Types")
#### 

```
type Config struct {
	// AffinityThreshold is the prefix cache score threshold. Endpoints with
	// score >= this value are considered "sticky" (prompt is cached). Default: 0.80.
	AffinityThreshold float64[](https://pkg.go.dev/builtin#float64) `json:"affinityThreshold,omitempty"`

	// ExplorationProbability is the probability of skipping the gate entirely,
	// keeping all endpoints for exploration. Range: [0, 1]. Default: 0.
	ExplorationProbability float64[](https://pkg.go.dev/builtin#float64) `json:"explorationProbability,omitempty"`

	// MaxTTFTPenaltyMs is the max TTFT penalty (ms) before breaking stickiness.
	// If the best sticky endpoint's TTFT exceeds the best non-sticky endpoint's
	// TTFT by more than this value, all endpoints are kept. Set to 0 to always
	// stick. Default: 18000.
	MaxTTFTPenaltyMs float64[](https://pkg.go.dev/builtin#float64) `json:"maxTTFTPenaltyMs,omitempty"`

	// TTFTSource selects where the load gate reads per-endpoint TTFT from.
	// TTFTSourcePrefillThroughput (default) estimates it from in-flight tokens and
	// PeakPrefillThroughput; TTFTSourceLatencyPredictor reads predicted TTFT from
	// the latency predictor.
	TTFTSource TTFTSource[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#TTFTSource) `json:"ttftSource,omitempty"`

	// PeakPrefillThroughput is the peak prefill throughput in tokens/sec, used to
	// estimate TTFT from in-flight tokens when TTFTSource is prefillThroughput:
	//   TTFT_ms = inFlightTokens / PeakPrefillThroughput * 1000
	// (tokens / (tokens/sec) * 1000 = ms). Default: 15928.
	PeakPrefillThroughput float64[](https://pkg.go.dev/builtin#float64) `json:"peakPrefillThroughput,omitempty"`

	PrefixMatchInfoProducerName       string[](https://pkg.go.dev/builtin#string) `json:"prefixMatchInfoProducerName,omitempty"`
	LatencyPredictionInfoProducerName string[](https://pkg.go.dev/builtin#string) `json:"latencyPredictionInfoProducerName,omitempty"`
	InFlightLoadProducerName          string[](https://pkg.go.dev/builtin#string) `json:"inFlightLoadProducerName,omitempty"`
}
```

#### 

```
type Plugin struct {
	// contains filtered or unexported fields
}
```

####  func (*Plugin) [Consumes](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity/plugin.go#L213) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#Plugin.Consumes "Go to Plugin.Consumes")

```
func (p *Plugin[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#Plugin)) Consumes() fwkplugin[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin).DataDependencies[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin#DataDependencies)
```

####  func (*Plugin) [Filter](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity/plugin.go#L165) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#Plugin.Filter "Go to Plugin.Filter")

```
func (p *Plugin[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#Plugin)) Filter(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), _ *fwksched[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling).InferenceRequest[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling#InferenceRequest), endpoints []fwksched[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling).Endpoint[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling#Endpoint)) []fwksched[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling).Endpoint[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling#Endpoint)
```

####  func (*Plugin) [TypedName](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity/plugin.go#L161) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#Plugin.TypedName "Go to Plugin.TypedName")

```
func (p *Plugin[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#Plugin)) TypedName() fwkplugin[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin).TypedName[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin#TypedName)
```

#### 

```
type TTFTSource string[](https://pkg.go.dev/builtin#string)
```

TTFTSource selects the per-endpoint TTFT signal used by the load gate. The choice also determines which producer attribute the filter consumes. 

```
const (
	// TTFTSourceLatencyPredictor reads predicted TTFT from LatencyPredictionInfo
	// (produced by the predicted-latency-producer).
	TTFTSourceLatencyPredictor TTFTSource[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#TTFTSource) = "latencyPredictor"
	// TTFTSourcePrefillThroughput estimates TTFT from in-flight tokens and
	// PeakPrefillThroughput, reading InFlightLoad (produced by the
	// in-flight-load-producer).
	TTFTSourcePrefillThroughput TTFTSource[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#TTFTSource) = "prefillThroughput"
)
```

##  Source Files [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity#section-sourcefiles "Go to Source Files")


Click to show internal directories. 
Click to hide internal directories. 
go.dev uses cookies from Google to deliver and enhance the quality of its services and to analyze traffic. [Learn more.](https://policies.google.com/technologies/cookies)
Okay
