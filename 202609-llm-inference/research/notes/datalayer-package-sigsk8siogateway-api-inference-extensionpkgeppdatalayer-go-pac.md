---
title: datalayer package - sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer
  - Go Packages
id: datalayer-package-sigsk8siogateway-api-inference-extensionpkgeppdatalayer-go-pac
tags:
- llmd-router-nonk8s
- data-layer
- epp-internals
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:43:14.433377Z'
updated: '2026-09-12T03:37:37.523415Z'
source: https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer
source_domain: pkg.go.dev
fetched_at: '2026-09-11T19:43:14.414664Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'godoc for pkg/epp/datalayer (concrete runtime implementation, v1.5.0 as
  latest indexed, features dated back to v1.2.0-v1.5.0). Key finding: the concrete
  EndpointPool type used here is Kubernetes-shaped — ''type EndpointPool struct {
  Selector map[string]string; TargetPorts []int; Namespace string }'' (added v1.2.0)
  — a label-selector-plus-namespace model, and PoolInfo interface literally exposes
  ''PodList(func(fwkdl.Endpoint) bool) []fwkdl.Endpoint'' with the doc comment: ''PoolInfo
  represents the DataStore information needed for endpoints. TODO: Consider if to
  remove/simplify in follow-ups. This is mostly for backward compatibility with backend.metrics''
  expectations and allowing a shared implementation during the transition.'' Runtime.Start(ctx,
  mgr ctrl.Manager) ''wires Kubernetes notifications into the manager'' — i.e. the
  Runtime''s own lifecycle is bound to a controller-runtime Manager. Feature gates:
  ExperimentalDatalayerFeatureGate is now a deprecated no-op (''data layer is now
  enabled by default''); EnableLegacyMetricsFeatureGate falls back to the pre-datalayer
  backend/metrics polling path. Config/DataSourceConfig (v1.3.0) wraps a fwkdl.DataSource
  plugin plus its Extractors, supporting both poll-based and event-driven (notification)
  sources — differentiated at setup, which is the extension point most likely portable
  to non-pod backends since Collector.Start requires only a Ticker, an Endpoint, and
  a slice of fwkdl.PollingDataSource (no direct k8s dependency at that call site).'
utility_score: 14.0
---

##  Documentation [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#section-documentation "Go to Documentation")
### Index [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#pkg-index "Go to Index")
  * [func BindNotificationSource(src fwkdl.NotificationSource, extractors []fwkdl.NotificationExtractor, ...) error](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#BindNotificationSource)
  * [func ValidateAndOrderDataDependencies(plugins []plugin.Plugin) ([]string, error)](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#ValidateAndOrderDataDependencies)
  * [type Collector](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Collector)
  *     * [func NewCollector() *Collector](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#NewCollector)
  *     * [func (c *Collector) Start(ctx context.Context, ticker Ticker, ep fwkdl.Endpoint, ...) error](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Collector.Start)
    * [func (c *Collector) Stop() error](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Collector.Stop)
  * [type DataSourceConfig](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#DataSourceConfig)
  * [type EndpointFactory](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#EndpointFactory)
  *     * [func NewTestRuntime(t *testing.T, refreshInterval time.Duration) EndpointFactory](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#NewTestRuntime)
    * [func NewTestRuntimeWithConfig(t *testing.T, refreshInterval time.Duration, cfg *Config) EndpointFactory](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#NewTestRuntimeWithConfig)
  * [type EndpointPool](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#EndpointPool)
  *     * [func NewEndpointPool(namespace string, name string) *EndpointPool](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#NewEndpointPool)
  * [type PoolInfo](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#PoolInfo)
  * [type Runtime](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Runtime)
  *     * [func NewRuntime(pollingInterval time.Duration) *Runtime](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#NewRuntime)
  *     * [func (r *Runtime) Configure(cfg *Config, enableNewMetrics bool, disallowedExtractorType string, ...) error](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Runtime.Configure)
    * [func (r *Runtime) NewEndpoint(ctx context.Context, endpointMetadata *fwkdl.EndpointMetadata, _ PoolInfo) fwkdl.Endpoint](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Runtime.NewEndpoint)
    * [func (r *Runtime) ReleaseEndpoint(ep fwkdl.Endpoint)](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Runtime.ReleaseEndpoint)
    * [func (r *Runtime) Start(ctx context.Context, mgr ctrl.Manager) error](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Runtime.Start)
    * [func (r *Runtime) Stop() error](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Runtime.Stop)
  *     * [func NewTimeTicker(d time.Duration) Ticker](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#NewTimeTicker)
  * [type TimeTicker](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#TimeTicker)
  *     * [func (t *TimeTicker) Channel() <-chan time.Time](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#TimeTicker.Channel)


### Constants [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#pkg-constants "Go to Constants")

```
const (
	FlowControlLayer    = 0
	RequestControlLayer = 1
	SchedulingLayer     = 2
	DefaultLayer        = -1 // For plugins that don't fit into a known layer
)
```

Define constants for layer execution order. Lower value means earlier execution. 

```
const (
	// ExperimentalDatalayerFeatureGate is deprecated. The data layer is now enabled by default.
	// This gate is a no-op and will be removed in a future version.
	ExperimentalDatalayerFeatureGate = "dataLayer"
	// EnableLegacyMetricsFeatureGate falls back to the legacy backend/metrics polling path.
	// This gate is temporary and will be removed when the legacy path is deleted.
	// To disable metrics collection without falling back to legacy, provide an empty data section in the config.
	EnableLegacyMetricsFeatureGate = "enableLegacyMetrics"
)
```

### Variables [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#pkg-variables "Go to Variables")
This section is empty.
### Functions [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#pkg-functions "Go to Functions")
####  added in v1.4.0

```
func BindNotificationSource(src fwkdl[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer).NotificationSource[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer#NotificationSource), extractors []fwkdl[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer).NotificationExtractor[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer#NotificationExtractor), mgr ctrl[](https://pkg.go.dev/sigs.k8s.io/controller-runtime).Manager[](https://pkg.go.dev/sigs.k8s.io/controller-runtime#Manager)) error[](https://pkg.go.dev/builtin#error)
```

BindNotificationSource registers a watcher/reconciler for the source's GVK. The framework core owns the cache and reconciliation; the source only receives deep-copied events via Notify. 
####  func [ValidateAndOrderDataDependencies](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/datalayer/data_graph.go#L33) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#ValidateAndOrderDataDependencies "Go to ValidateAndOrderDataDependencies") added in v1.4.0

```
func ValidateAndOrderDataDependencies(plugins []plugin[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/plugin).Plugin[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/plugin#Plugin)) ([]string[](https://pkg.go.dev/builtin#string), error[](https://pkg.go.dev/builtin#error))
```

ValidateAndOrderDataDependencies validates that the data dependencies among the given plugins are acyclic and returns a topologically sorted order of plugin names based on their data dependencies. Further, it validates that the plugins are ordered in a way that respects the layer execution order. 
### Types [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#pkg-types "Go to Types")
#### 

```
type Collector struct {
	// contains filtered or unexported fields
}
```

Collector runs the data collection for a single endpoint. 
#### 

```
func NewCollector() *Collector[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Collector)
```

NewCollector returns a new collector. 
####  func (*Collector) [Start](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/datalayer/collector.go#L99) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Collector.Start "Go to Collector.Start")

```
func (c *Collector[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Collector)) Start(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), ticker Ticker[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Ticker), ep fwkdl[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer).Endpoint[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer#Endpoint), pollers []fwkdl[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer).PollingDataSource[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer#PollingDataSource), extractors map[string[](https://pkg.go.dev/builtin#string)][]fwkdl[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer).Extractor[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer#Extractor)) error[](https://pkg.go.dev/builtin#error)
```

Start initiates data source collection for the endpoint. All sources must implement PollingDataSource. Validation is performed by the caller. 
####  func (*Collector) [Stop](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/datalayer/collector.go#L187) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Collector.Stop "Go to Collector.Stop")

```
func (c *Collector[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Collector)) Stop() error[](https://pkg.go.dev/builtin#error)
```

Stop terminates the collector and waits for the collection goroutine to exit. 
####  added in v1.3.0

```
type Config struct {
	Sources []DataSourceConfig[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#DataSourceConfig) // the data sources configured in the data layer
}
```

Config defines the configuration of EPP data layer, as the set of DataSources and Extractors defined on them. Both poll-based and event-driven (notification) sources are stored in Sources. Differentiation by type of source is handled during the set-up phase. 
####  added in v1.3.0

```
type DataSourceConfig struct {
	Plugin     fwkdl[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer).DataSource[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer#DataSource)  // the data source plugin instance
	Extractors []fwkdl[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer).Extractor[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer#Extractor) // extractors defined for the data source
}
```

DataSourceConfig defines the configuration of a specific DataSource 
#### 

```
type EndpointFactory interface {
	NewEndpoint(parent context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), inEnpointMetadata *fwkdl[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer).EndpointMetadata[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer#EndpointMetadata), poolinfo PoolInfo[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#PoolInfo)) fwkdl[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer).Endpoint[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer#Endpoint)
	ReleaseEndpoint(ep fwkdl[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer).Endpoint[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer#Endpoint))
}
```

EndpointFactory defines an interface for managing Endpoint lifecycle. Specifically, providing methods to allocate and retire endpoints. This can potentially be used for pooled memory or other management chores in the implementation. 
####  added in v1.5.0

```
func NewTestRuntime(t *testing[](https://pkg.go.dev/testing).T[](https://pkg.go.dev/testing#T), refreshInterval time[](https://pkg.go.dev/time).Duration[](https://pkg.go.dev/time#Duration)) EndpointFactory[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#EndpointFactory)
```

NewTestRuntime creates a Runtime configured with a mocks.MetricsDataSource for testing. It configures the runtime but does not start it. Returns an EndpointFactory for use in tests. 
####  added in v1.5.0

```
func NewTestRuntimeWithConfig(t *testing[](https://pkg.go.dev/testing).T[](https://pkg.go.dev/testing#T), refreshInterval time[](https://pkg.go.dev/time).Duration[](https://pkg.go.dev/time#Duration), cfg *Config[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Config)) EndpointFactory[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#EndpointFactory)
```

NewTestRuntimeWithConfig creates a Runtime with the given config for testing. It configures the runtime but does not start it. Returns an EndpointFactory for use in tests. 
####  added in v1.2.0

```
type EndpointPool struct {
	Selector    map[string[](https://pkg.go.dev/builtin#string)]string[](https://pkg.go.dev/builtin#string)
	TargetPorts []int[](https://pkg.go.dev/builtin#int)
	Namespace   string[](https://pkg.go.dev/builtin#string)
}
```

####  added in v1.2.0

```
func NewEndpointPool(namespace string[](https://pkg.go.dev/builtin#string), name string[](https://pkg.go.dev/builtin#string)) *EndpointPool[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#EndpointPool)
```

NewEndpointPool creates and returns a new empty instance of EndpointPool. 
#### 

```
type PoolInfo interface {
	PoolGet() (*EndpointPool[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#EndpointPool), error[](https://pkg.go.dev/builtin#error))
	PodList(func(fwkdl[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer).Endpoint[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer#Endpoint)) bool[](https://pkg.go.dev/builtin#bool)) []fwkdl[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer).Endpoint[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer#Endpoint)
}
```

PoolInfo represents the DataStore information needed for endpoints. TODO: Consider if to remove/simplify in follow-ups. This is mostly for backward compatibility with backend.metrics' expectations and allowing a shared implementation during the transition. 
  * Endpoint metric scraping uses PoolGet to access the pool's Port and Name.
  * Global metrics logging uses PoolGet solely for error return and PodList to enumerate all endpoints for metrics summarization.


####  added in v1.5.0

```
type Runtime struct {
	// contains filtered or unexported fields
}
```

Runtime manages data sources, extractors, their mapping, and endpoint lifecycle. 
####  added in v1.5.0

```
func NewRuntime(pollingInterval time[](https://pkg.go.dev/time).Duration[](https://pkg.go.dev/time#Duration)) *Runtime[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Runtime)
```

NewRuntime creates a new Runtime with the given polling interval. If duration is <= 0, uses the defaultRefreshInterval. 
####  func (*Runtime) [Configure](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/datalayer/runtime.go#L66) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Runtime.Configure "Go to Runtime.Configure") added in v1.5.0

```
func (r *Runtime[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Runtime)) Configure(cfg *Config[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Config), enableNewMetrics bool[](https://pkg.go.dev/builtin#bool), disallowedExtractorType string[](https://pkg.go.dev/builtin#string), logger logr[](https://pkg.go.dev/github.com/go-logr/logr).Logger[](https://pkg.go.dev/github.com/go-logr/logr#Logger)) error[](https://pkg.go.dev/builtin#error)
```

Configure is called to transform the configuration information into the Runtime's internal fields. 
####  func (*Runtime) [NewEndpoint](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/datalayer/runtime.go#L165) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Runtime.NewEndpoint "Go to Runtime.NewEndpoint") added in v1.5.0

```
func (r *Runtime[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Runtime)) NewEndpoint(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), endpointMetadata *fwkdl[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer).EndpointMetadata[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer#EndpointMetadata), _ PoolInfo[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#PoolInfo)) fwkdl[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer).Endpoint[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer#Endpoint)
```

NewEndpoint sets up data polling on the provided endpoint. 
####  func (*Runtime) [ReleaseEndpoint](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/datalayer/runtime.go#L214) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Runtime.ReleaseEndpoint "Go to Runtime.ReleaseEndpoint") added in v1.5.0

```
func (r *Runtime[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Runtime)) ReleaseEndpoint(ep fwkdl[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer).Endpoint[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/datalayer#Endpoint))
```

ReleaseEndpoint terminates polling for data on the given endpoint. 
####  func (*Runtime) [Start](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/datalayer/runtime.go#L127) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Runtime.Start "Go to Runtime.Start") added in v1.5.0

```
func (r *Runtime[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Runtime)) Start(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), mgr ctrl[](https://pkg.go.dev/sigs.k8s.io/controller-runtime).Manager[](https://pkg.go.dev/sigs.k8s.io/controller-runtime#Manager)) error[](https://pkg.go.dev/builtin#error)
```

Start is called to enable the Runtime to start processing data collection. It wires Kubernetes notifications into the manager. 
####  func (*Runtime) [Stop](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/datalayer/runtime.go#L154) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Runtime.Stop "Go to Runtime.Stop") added in v1.5.0

```
func (r *Runtime[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Runtime)) Stop() error[](https://pkg.go.dev/builtin#error)
```

Stop is called to terminate the Runtime's data collection. It terminates all go routines used for polling data sources. 
#### 

```
type Ticker interface {
	Channel() <-chan time[](https://pkg.go.dev/time).Time[](https://pkg.go.dev/time#Time)
	Stop()
}
```

Ticker implements a time source for periodic invocation. The Ticker is passed in as parameter a Collector to allow control over time progress in tests, ensuring tests are deterministic and fast. 
#### 

```
func NewTimeTicker(d time[](https://pkg.go.dev/time).Duration[](https://pkg.go.dev/time#Duration)) Ticker[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#Ticker)
```

NewTimeTicker returns a new time.Ticker with the configured duration. 
#### 

```
type TimeTicker struct {
}
```

TimeTicker implements a Ticker based on time.Ticker. 
####  func (*TimeTicker) [Channel](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/datalayer/collector.go#L64) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#TimeTicker.Channel "Go to TimeTicker.Channel")

```
func (t *TimeTicker[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#TimeTicker)) Channel() <-chan time[](https://pkg.go.dev/time).Time[](https://pkg.go.dev/time#Time)
```

Channel exposes the ticker's channel. 
##  Source Files [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#section-sourcefiles "Go to Source Files")


##  Directories [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/datalayer#section-directories "Go to Directories")
Show internal  Collapse all  
| Path  | Synopsis  |  
| --- | --- |  
|   |  
|   |  
Click to show internal directories. 
Click to hide internal directories. 
go.dev uses cookies from Google to deliver and enhance the quality of its services and to analyze traffic. [Learn more.](https://policies.google.com/technologies/cookies)
Okay
