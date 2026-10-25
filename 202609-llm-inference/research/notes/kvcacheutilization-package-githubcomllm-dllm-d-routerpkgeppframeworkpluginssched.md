---
title: kvcacheutilization package - github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization
  - Go Packages
id: kvcacheutilization-package-githubcomllm-dllm-d-routerpkgeppframeworkpluginssched
tags:
- llm-d
- kv-cache-routing
- epp-scheduler
- kv-cache-scorer
- llmd-kv-cache-routing-c665d5
created: '2026-09-14T11:53:13.440638Z'
updated: '2026-09-15T04:55:24.019027Z'
source: https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization
source_domain: pkg.go.dev
fetched_at: '2026-09-14T11:53:13.418289Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'GoDoc reference (llm-d-router v0.10.0) for the kv-cache-utilization-scorer
  plugin. Exact formula: score(endpoint) = 1 - kvCacheUsagePercent, read from endpoint
  metrics via metrics.KVCacheUsagePercentKey, so lower KV-cache usage yields a higher
  score (Distribution category -- spreads traffic away from high-KV-pressure endpoints).
  No runtime parameters; wired into a scheduling profile with a weight. Also documents
  multicluster-kv-cache-utilization-scorer, a cluster-scoped variant (added v0.10.0)
  that scores on a pool-level llm-d.ai/multicluster-kv-cache-utilization attribute
  rather than a per-pod scrape -- requires configuring a multicluster-metrics-data-source
  (scrapes the peer cluster) and multicluster-metrics-extractor in dataLayer, or the
  scorer silently returns no score for that endpoint.'
utility_score: 13.0
---

##  README [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#section-readme "Go to Readme")
### KV Cache Utilization Scorer Plugin
**Type:** `kv-cache-utilization-scorer`
This plugin scores candidate endpoints using each endpoint's current KV-cache utilization.
#### What it does
For each candidate endpoint, the plugin computes:

```
  {score(endpoint)} = 1 - {kvCacheUsagePercent}

```

Where `kvCacheUsagePercent` is read from endpoint metrics.
This means:
  * lower KV-cache usage -> higher score
  * higher KV-cache usage -> lower score


#### Scheduling intent
The scorer returns category `Distribution`, so it helps spread traffic away from endpoints with high KV-cache pressure.
#### Inputs consumed
The plugin consumes:
  * `metrics.KVCacheUsagePercentKey` (`float64`)


#### Configuration
This scorer currently has no runtime parameters.
**Configuration Example:**

```
plugins:
  - type: kv-cache-utilization-scorer
    name: kv-cache-util
schedulingProfiles:
  - name: default
    plugins:
      - pluginRef: kv-cache-util
        weight: 1

```

#### Multi-cluster support
`multicluster-kv-cache-utilization-scorer` is the cluster-scoped variant. The stock per-pod KV field is empty on a cluster-endpoint, so it scores on the `llm-d.ai/multicluster-kv-cache-utilization` attribute. The `multicluster-metrics-data-source` scrapes the peer cluster and the `multicluster-metrics-extractor` writes that attribute from the pool's aggregate KV metric. Configure both in `dataLayer`, otherwise this scorer sees no pool metric and returns no score. See the [wiring example](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/epp/framework/plugins/README.md).
Expand ▾ Collapse ▴
##  Documentation [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#section-documentation "Go to Documentation")
### Index [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#pkg-index "Go to Index")
  * [func KvCacheUtilizationScorerFactory(name string, _ *json.Decoder, _ fwkplugin.Handle) (fwkplugin.Plugin, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KvCacheUtilizationScorerFactory)
  * [func MultiClusterScorerFactory(name string, params *json.Decoder, handle fwkplugin.Handle) (fwkplugin.Plugin, error)](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#MultiClusterScorerFactory)
  * [type KVCacheUtilizationScorer](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer)
  *     * [func NewKVCacheUtilizationScorer() *KVCacheUtilizationScorer](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#NewKVCacheUtilizationScorer)
  *     * [func (s *KVCacheUtilizationScorer) Category() fwksched.ScorerCategory](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer.Category)
    * [func (s *KVCacheUtilizationScorer) Consumes() map[string]any](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer.Consumes)
    * [func (s *KVCacheUtilizationScorer) Score(_ context.Context, _ *fwksched.InferenceRequest, endpoints []fwksched.Endpoint) map[fwksched.Endpoint]float64](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer.Score)
    * [func (s *KVCacheUtilizationScorer) TypedName() fwkplugin.TypedName](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer.TypedName)
    * [func (s *KVCacheUtilizationScorer) WithName(name string) *KVCacheUtilizationScorer](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer.WithName)
  * [type MultiClusterScorer](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#MultiClusterScorer)
  *     * [func (s *MultiClusterScorer) Consumes() fwkplugin.DataDependencies](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#MultiClusterScorer.Consumes)
    * [func (s *MultiClusterScorer) Score(_ context.Context, _ *fwksched.InferenceRequest, endpoints []fwksched.Endpoint) map[fwksched.Endpoint]float64](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#MultiClusterScorer.Score)
    * [func (s *MultiClusterScorer) TypedName() fwkplugin.TypedName](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#MultiClusterScorer.TypedName)


### Constants [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#pkg-constants "Go to Constants")

```
const (
	KvCacheUtilizationScorerType = "kv-cache-utilization-scorer"
)
```


```
const MultiClusterScorerType = "multicluster-kv-cache-utilization-scorer"
```

MultiClusterScorerType is the cross-cluster kv-cache-utilization scorer. 
### Variables [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#pkg-variables "Go to Variables")
This section is empty.
### Functions [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#pkg-functions "Go to Functions")
####  func [KvCacheUtilizationScorerFactory](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization/kvcache_utilization.go#L36) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KvCacheUtilizationScorerFactory "Go to KvCacheUtilizationScorerFactory")

```
func KvCacheUtilizationScorerFactory(name string[](https://pkg.go.dev/builtin#string), _ *json[](https://pkg.go.dev/encoding/json).Decoder[](https://pkg.go.dev/encoding/json#Decoder), _ fwkplugin[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin).Handle[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin#Handle)) (fwkplugin[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin).Plugin[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin#Plugin), error[](https://pkg.go.dev/builtin#error))
```

KvCacheUtilizationScorerFactory defines the factory function for KVCacheUtilizationScorer. 
####  added in v0.10.0

```
func MultiClusterScorerFactory(name string[](https://pkg.go.dev/builtin#string), params *json[](https://pkg.go.dev/encoding/json).Decoder[](https://pkg.go.dev/encoding/json#Decoder), handle fwkplugin[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin).Handle[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin#Handle)) (fwkplugin[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin).Plugin[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin#Plugin), error[](https://pkg.go.dev/builtin#error))
```

MultiClusterScorerFactory builds the cross-cluster kv-cache-utilization scorer. 
### Types [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#pkg-types "Go to Types")
#### 

```
type KVCacheUtilizationScorer struct {
	// contains filtered or unexported fields
}
```

KVCacheUtilizationScorer scores list of candidate endpoints based on KV cache utilization. 
#### 

```
func NewKVCacheUtilizationScorer() *KVCacheUtilizationScorer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer)
```

NewKVCacheUtilizationScorer initializes a new KVCacheUtilizationScorer and returns its pointer. 
####  func (*KVCacheUtilizationScorer) [Category](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization/kvcache_utilization.go#L58) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer.Category "Go to KVCacheUtilizationScorer.Category")

```
func (s *KVCacheUtilizationScorer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer)) Category() fwksched[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling).ScorerCategory[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling#ScorerCategory)
```

Category returns the preference the scorer applies when scoring candidate endpoints. 
####  func (*KVCacheUtilizationScorer) [Consumes](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization/kvcache_utilization.go#L63) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer.Consumes "Go to KVCacheUtilizationScorer.Consumes")

```
func (s *KVCacheUtilizationScorer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer)) Consumes() map[string[](https://pkg.go.dev/builtin#string)]any[](https://pkg.go.dev/builtin#any)
```

Consumes returns the list of data that is consumed by the plugin. 
####  func (*KVCacheUtilizationScorer) [Score](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization/kvcache_utilization.go#L76) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer.Score "Go to KVCacheUtilizationScorer.Score")

```
func (s *KVCacheUtilizationScorer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer)) Score(_ context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), _ *fwksched[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling).InferenceRequest[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling#InferenceRequest), endpoints []fwksched[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling).Endpoint[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling#Endpoint)) map[fwksched[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling).Endpoint[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling#Endpoint)]float64[](https://pkg.go.dev/builtin#float64)
```

Score returns the scoring result for the given list of endpoints based on context. 
####  func (*KVCacheUtilizationScorer) [TypedName](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization/kvcache_utilization.go#L53) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer.TypedName "Go to KVCacheUtilizationScorer.TypedName")

```
func (s *KVCacheUtilizationScorer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer)) TypedName() fwkplugin[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin).TypedName[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin#TypedName)
```

TypedName returns the type and name tuple of this plugin instance. 
####  func (*KVCacheUtilizationScorer) [WithName](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization/kvcache_utilization.go#L70) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer.WithName "Go to KVCacheUtilizationScorer.WithName")

```
func (s *KVCacheUtilizationScorer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer)) WithName(name string[](https://pkg.go.dev/builtin#string)) *KVCacheUtilizationScorer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer)
```

WithName sets the name of the scorer. 
####  added in v0.10.0

```
type MultiClusterScorer struct {
	*KVCacheUtilizationScorer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#KVCacheUtilizationScorer)
}
```

MultiClusterScorer scores cluster endpoints from a pool-level KV-cache utilization summary, read by name, instead of a single pod's scrape. 
####  func (*MultiClusterScorer) [Consumes](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization/multicluster.go#L57) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#MultiClusterScorer.Consumes "Go to MultiClusterScorer.Consumes") added in v0.10.0

```
func (s *MultiClusterScorer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#MultiClusterScorer)) Consumes() fwkplugin[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin).DataDependencies[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin#DataDependencies)
```

Consumes marks the pool KV-cache utilization attribute Required, so a config missing the multicluster metrics extractor fails at load rather than silently no-scoring. 
####  func (*MultiClusterScorer) [Score](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization/multicluster.go#L67) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#MultiClusterScorer.Score "Go to MultiClusterScorer.Score") added in v0.10.0

```
func (s *MultiClusterScorer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#MultiClusterScorer)) Score(_ context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), _ *fwksched[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling).InferenceRequest[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling#InferenceRequest), endpoints []fwksched[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling).Endpoint[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling#Endpoint)) map[fwksched[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling).Endpoint[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling#Endpoint)]float64[](https://pkg.go.dev/builtin#float64)
```

Score scores each cluster endpoint as 1 - its pool KV-cache utilization. Endpoints without the attribute are left unscored. 
####  func (*MultiClusterScorer) [TypedName](https://github.com/llm-d/llm-d-router/blob/v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization/multicluster.go#L49) [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#MultiClusterScorer.TypedName "Go to MultiClusterScorer.TypedName") added in v0.10.0

```
func (s *MultiClusterScorer[](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#MultiClusterScorer)) TypedName() fwkplugin[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin).TypedName[](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin#TypedName)
```

TypedName reports the multi-cluster type with this instance's name. 
##  Source Files [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization#section-sourcefiles "Go to Source Files")


Click to show internal directories. 
Click to hide internal directories. 
go.dev uses cookies from Google to deliver and enhance the quality of its services and to analyze traffic. [Learn more.](https://policies.google.com/technologies/cookies)
Okay
