---
title: datalayer package - sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer
  - Go Packages
id: datalayer-package-sigsk8siogateway-api-inference-extensionpkgeppframeworkinterfa
tags:
- llmd-router-nonk8s
- llmd-router-nonk8s-backends-54b774
- data-layer
- epp-internals
- pods-assumption
created: '2026-09-11T19:48:30.438708Z'
updated: '2026-09-15T04:55:23.808502Z'
source: https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer
source_domain: pkg.go.dev
fetched_at: '2026-09-11T19:48:30.417894Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'godoc for pkg/epp/framework/interface/datalayer (the abstract interface
  package underlying pkg/epp/datalayer''s Runtime; discovered via cross-reference
  from the datalayer package). This is the single most direct answer to whether the
  data layer assumes Kubernetes pods: ''type EndpointMetadata struct { NamespacedName
  types.NamespacedName; PodName string; Address string; MetricsHost string; Labels
  map[string]string }'' with doc comment ''EndpointMetadata represents the relevant
  Kubernetes Pod state of an inference server.'' — the concrete metadata struct is
  Pod-shaped by definition (NamespacedName + PodName + Labels), not a generic backend
  descriptor. The Endpoint interface itself is more abstract (embeds fmt.Stringer,
  EndpointMetaState, EndpointMetricsState — i.e. GetMetadata/UpdateMetadata/GetAttributes/GetMetrics/UpdateMetrics),
  and ModelServer is ''an implementation of the Endpoint interface'' built via NewEndpoint(meta
  *EndpointMetadata, metrics *Metrics). Two DataSource extension points differ sharply
  in k8s-coupling: PollingDataSource (''Poll(ctx, ep Endpoint) (any, error)'') is
  generic — it only needs an Endpoint and returns arbitrary polled data, with no direct
  k8s dependency at the interface level, making it the most portable extension point
  for a non-k8s backend. NotificationSource, by contrast, is explicitly built around
  Kubernetes watches: ''NotificationSource is an event-driven DataSource for a single
  k8s GVK. The framework core owns the k8s notification mechanisms (e.g., watches,
  caches, informers) and calls the source''s Notify on events''; EventType constants
  are literally ''EventAddOrUpdate is fired when a k8s object is created or updated''
  / ''EventDelete is fired when a k8s object is deleted''; NotificationEvent.Object
  is a *unstructured.Unstructured (raw k8s object). EndpointSource/EndpointExtractor
  (added v1.5.0) sit on top of endpoint lifecycle events dispatched by the Runtime,
  decoupled from the raw k8s watch mechanism but still keyed on the Pod-shaped EndpointMetadata.
  Metrics struct (ActiveModels, WaitingModels, MaxActiveModels, RunningRequestsSize,
  WaitingQueueSize, KVCacheUsagePercent, KvCacheMaxTokenCapacity, CacheBlockSize,
  CacheNumBlocks, UpdateTime) is itself backend-agnostic — no k8s or pod fields —
  so a custom PollingDataSource could in principle populate a real Metrics object
  for a non-pod backend, but would still have to synthesize a valid EndpointMetadata
  (with placeholder NamespacedName/PodName) to satisfy the type.'
---

*Suggested by [[datalayer-package-sigsk8siogateway-api-inference-extensionpkgeppdatalayer-go-pac]] — datalayer package's Endpoint/EndpointMetadata types are defined in this framework/interface package, not datalayer itself*

##  Documentation [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#section-documentation "Go to Documentation")
### Index [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#pkg-index "Go to Index")
  * [type AttributeMap](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#AttributeMap)
  *     * [func NewAttributes() AttributeMap](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#NewAttributes)
  * [type Attributes](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Attributes)
  *     * [func (a *Attributes) Clone() AttributeMap](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Attributes.Clone)
    * [func (a *Attributes) Get(key string) (Cloneable, bool)](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Attributes.Get)
    * [func (a *Attributes) Keys() []string](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Attributes.Keys)
    * [func (a *Attributes) Put(key string, value Cloneable)](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Attributes.Put)
  * [type Cloneable](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Cloneable)
  * [type DataSource](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#DataSource)
  * [type Endpoint](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Endpoint)
  * [type EndpointEvent](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointEvent)
  * [type EndpointExtractor](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointExtractor)
  * [type EndpointMetaState](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetaState)
  * [type EndpointMetadata](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata)
  *     * [func (p *EndpointMetadata) Clone() *EndpointMetadata](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata.Clone)
    * [func (e *EndpointMetadata) GetIPAddress() string](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata.GetIPAddress)
    * [func (e *EndpointMetadata) GetMetricsHost() string](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata.GetMetricsHost)
    * [func (e *EndpointMetadata) GetNamespacedName() types.NamespacedName](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata.GetNamespacedName)
    * [func (e *EndpointMetadata) GetPort() string](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata.GetPort)
    * [func (e *EndpointMetadata) String() string](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata.String)
  * [type EndpointMetricsState](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetricsState)
  * [type EndpointSource](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointSource)
  * [type EventType](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EventType)
  * [type Extractor](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Extractor)
  * [type Metrics](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Metrics)
  *     * [func NewMetrics() *Metrics](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#NewMetrics)
  *     * [func (m *Metrics) Clone() *Metrics](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Metrics.Clone)
    * [func (m *Metrics) String() string](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Metrics.String)
  * [type ModelServer](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer)
  *     * [func NewEndpoint(meta *EndpointMetadata, metrics *Metrics) *ModelServer](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#NewEndpoint)
  *     * [func (srv *ModelServer) Clone() *ModelServer](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer.Clone)
    * [func (srv *ModelServer) GetAttributes() AttributeMap](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer.GetAttributes)
    * [func (srv *ModelServer) GetMetadata() *EndpointMetadata](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer.GetMetadata)
    * [func (srv *ModelServer) GetMetrics() *Metrics](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer.GetMetrics)
    * [func (srv *ModelServer) String() string](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer.String)
    * [func (srv *ModelServer) UpdateMetadata(pod *EndpointMetadata)](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer.UpdateMetadata)
    * [func (srv *ModelServer) UpdateMetrics(metrics *Metrics)](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer.UpdateMetrics)
  * [type NotificationEvent](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#NotificationEvent)
  * [type NotificationExtractor](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#NotificationExtractor)
  * [type NotificationSource](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#NotificationSource)
  * [type PollingDataSource](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#PollingDataSource)
  * [type ValidatingDataSource](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ValidatingDataSource)


### Constants [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#pkg-constants "Go to Constants")
This section is empty.
### Variables [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#pkg-variables "Go to Variables")

```
var (
	ExtractorType             = reflect[](https://pkg.go.dev/reflect).TypeFor[](https://pkg.go.dev/reflect#TypeFor)[Extractor[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Extractor)]()
	NotificationExtractorType = reflect[](https://pkg.go.dev/reflect).TypeFor[](https://pkg.go.dev/reflect#TypeFor)[NotificationExtractor[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#NotificationExtractor)]()
	NotificationEventType     = reflect[](https://pkg.go.dev/reflect).TypeFor[](https://pkg.go.dev/reflect#TypeFor)[NotificationEvent[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#NotificationEvent)]()
	EndpointExtractorType     = reflect[](https://pkg.go.dev/reflect).TypeFor[](https://pkg.go.dev/reflect#TypeFor)[EndpointExtractor[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointExtractor)]()
	EndpointEventReflectType  = reflect[](https://pkg.go.dev/reflect).TypeFor[](https://pkg.go.dev/reflect#TypeFor)[EndpointEvent[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointEvent)]()
)
```

### Functions [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#pkg-functions "Go to Functions")
This section is empty.
### Types [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#pkg-types "Go to Types")
#### 

```
type AttributeMap interface {
	Put(string[](https://pkg.go.dev/builtin#string), Cloneable[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Cloneable))
	Get(string[](https://pkg.go.dev/builtin#string)) (Cloneable[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Cloneable), bool[](https://pkg.go.dev/builtin#bool))
	Keys() []string[](https://pkg.go.dev/builtin#string)
	Clone() AttributeMap[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#AttributeMap)
}
```

AttributeMap is used to store flexible metadata or traits across different aspects of an inference server. Stored values must be Cloneable. 
#### 

```
func NewAttributes() AttributeMap[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#AttributeMap)
```

NewAttributes returns a new instance of Attributes. 
#### 

```
type Attributes struct {
	// contains filtered or unexported fields
}
```

Attributes provides a goroutine-safe implementation of AttributeMap. 
####  func (*Attributes) [Clone](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/attributemap.go#L80) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Attributes.Clone "Go to Attributes.Clone")

```
func (a *Attributes[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Attributes)) Clone() AttributeMap[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#AttributeMap)
```

Clone creates a deep copy of the entire attribute map. 
####  func (*Attributes) [Get](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/attributemap.go#L56) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Attributes.Get "Go to Attributes.Get")

```
func (a *Attributes[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Attributes)) Get(key string[](https://pkg.go.dev/builtin#string)) (Cloneable[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Cloneable), bool[](https://pkg.go.dev/builtin#bool))
```

Get retrieves an attribute by key, returning a cloned copy. 
####  func (*Attributes) [Keys](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/attributemap.go#L68) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Attributes.Keys "Go to Attributes.Keys")

```
func (a *Attributes[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Attributes)) Keys() []string[](https://pkg.go.dev/builtin#string)
```

Keys returns all keys in the attribute map. 
####  func (*Attributes) [Put](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/attributemap.go#L49) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Attributes.Put "Go to Attributes.Put")

```
func (a *Attributes[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Attributes)) Put(key string[](https://pkg.go.dev/builtin#string), value Cloneable[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Cloneable))
```

Put adds or updates an attribute in the map. 
#### 

```
type Cloneable interface {
	Clone() Cloneable[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Cloneable)
}
```

Cloneable types support cloning of the value. 
#### 

```
type DataSource interface {
	plugin[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/plugin).Plugin[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/plugin#Plugin)
	// OutputType returns the type of data this DataSource produces.
	// Used for validating extractor compatibility.
	OutputType() reflect[](https://pkg.go.dev/reflect).Type[](https://pkg.go.dev/reflect#Type)
	// ExtractorType returns the type of Extractor this DataSource expects.
	// For poll-based sources, this is the base Extractor interface.
	// For notification sources, this is the NotificationExtractor interface.
	ExtractorType() reflect[](https://pkg.go.dev/reflect).Type[](https://pkg.go.dev/reflect#Type)
}
```

DataSource provides raw data to registered Extractors. For poll-based sources, use PollingDataSource. For event-driven sources, use NotificationSource. 
#### 

```
type Endpoint interface {
	fmt[](https://pkg.go.dev/fmt).Stringer[](https://pkg.go.dev/fmt#Stringer)
	EndpointMetaState[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetaState)
	EndpointMetricsState[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetricsState)
}
```

Endpoint represents an inference serving endpoint and its related attributes. 
####  added in v1.5.0

```
type EndpointEvent struct {
	Type     EventType[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EventType)
	Endpoint Endpoint[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Endpoint)
}
```

EndpointEvent carries an endpoint lifecycle event. Reuses EventType: EventAddOrUpdate signals an endpoint was added to the datastore; EventDelete signals an endpoint was removed. 
####  added in v1.5.0

```
type EndpointExtractor interface {
	Extractor[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Extractor)
	// ExtractEndpoint processes an endpoint lifecycle event.
	ExtractEndpoint(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), event EndpointEvent[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointEvent)) error[](https://pkg.go.dev/builtin#error)
}
```

EndpointExtractor processes endpoint lifecycle events pushed from an EndpointSource. Called synchronously by the Runtime in event order. 
#### 

```
type EndpointMetaState interface {
	GetMetadata() *EndpointMetadata[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata)
	UpdateMetadata(*EndpointMetadata[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata))
	GetAttributes() AttributeMap[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#AttributeMap)
}
```

EndpointMetaState allows management of the EndpointMetadata related attributes. 
#### 

```
type EndpointMetadata struct {
	NamespacedName types[](https://pkg.go.dev/k8s.io/apimachinery/pkg/types).NamespacedName[](https://pkg.go.dev/k8s.io/apimachinery/pkg/types#NamespacedName)
	PodName        string[](https://pkg.go.dev/builtin#string)
	Address        string[](https://pkg.go.dev/builtin#string)
	MetricsHost    string[](https://pkg.go.dev/builtin#string)
	Labels         map[string[](https://pkg.go.dev/builtin#string)]string[](https://pkg.go.dev/builtin#string)
}
```

EndpointMetadata represents the relevant Kubernetes Pod state of an inference server. 
####  func (*EndpointMetadata) [Clone](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/endpoint_metadata.go#L45) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata.Clone "Go to EndpointMetadata.Clone")

```
func (p *EndpointMetadata[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata)) Clone() *EndpointMetadata[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata)
```

Clone returns a full copy of the object. 
####  func (*EndpointMetadata) [GetIPAddress](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/endpoint_metadata.go#L71) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata.GetIPAddress "Go to EndpointMetadata.GetIPAddress")

```
func (e *EndpointMetadata[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata)) GetIPAddress() string[](https://pkg.go.dev/builtin#string)
```

GetIPAddress returns the Endpoint's IP address. 
####  func (*EndpointMetadata) [GetMetricsHost](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/endpoint_metadata.go#L81) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata.GetMetricsHost "Go to EndpointMetadata.GetMetricsHost")

```
func (e *EndpointMetadata[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata)) GetMetricsHost() string[](https://pkg.go.dev/builtin#string)
```

GetMetricsHost returns the Endpoint's metrics host (ip:port) 
####  func (*EndpointMetadata) [GetNamespacedName](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/endpoint_metadata.go#L66) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata.GetNamespacedName "Go to EndpointMetadata.GetNamespacedName")

```
func (e *EndpointMetadata[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata)) GetNamespacedName() types[](https://pkg.go.dev/k8s.io/apimachinery/pkg/types).NamespacedName[](https://pkg.go.dev/k8s.io/apimachinery/pkg/types#NamespacedName)
```

GetNamespacedName gets the namespace name of the Endpoint. 
####  func (*EndpointMetadata) [GetPort](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/endpoint_metadata.go#L76) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata.GetPort "Go to EndpointMetadata.GetPort")

```
func (e *EndpointMetadata[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata)) GetPort() string[](https://pkg.go.dev/builtin#string)
```

GetPort returns the Endpoint's inference port. 
####  func (*EndpointMetadata) [String](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/endpoint_metadata.go#L37) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata.String "Go to EndpointMetadata.String")

```
func (e *EndpointMetadata[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata)) String() string[](https://pkg.go.dev/builtin#string)
```

String returns a string representation of the endpoint. 
#### 

```
type EndpointMetricsState interface {
	GetMetrics() *Metrics[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Metrics)
	UpdateMetrics(*Metrics[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Metrics))
}
```

EndpointMetricsState allows management of the Metrics related attributes. 
####  added in v1.5.0

```
type EndpointSource interface {
	DataSource[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#DataSource)
	// NotifyEndpoint is called by the Runtime on each endpoint lifecycle event.
	// Returns the event (possibly modified) for the Runtime to dispatch to extractors.
	// Returns nil event to signal Runtime to skip extractor dispatch.
	NotifyEndpoint(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), event EndpointEvent[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointEvent)) (*EndpointEvent[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointEvent), error[](https://pkg.go.dev/builtin#error))
}
```

EndpointSource is an event-driven DataSource driven by endpoint lifecycle changes. The Runtime calls NotifyEndpoint when an endpoint is added to or removed from the datastore, then dispatches the (possibly modified) event to registered EndpointExtractors. Return nil to suppress extractor dispatch. 
#### 

```
type EventType int[](https://pkg.go.dev/builtin#int)
```

EventType identifies the type of mutation that triggered the notification. 

```
const (
	// EventAddOrUpdate is fired when a k8s object is created or updated.
	EventAddOrUpdate EventType[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EventType) = iota[](https://pkg.go.dev/builtin#iota)
	// EventDelete is fired when a k8s object is deleted.
	EventDelete
)
```

#### 

```
type Extractor interface {
	plugin[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/plugin).Plugin[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension@v1.5.0/pkg/epp/framework/interface/plugin#Plugin)
	// ExpectedInputType defines the type expected by the extractor.
	ExpectedInputType() reflect[](https://pkg.go.dev/reflect).Type[](https://pkg.go.dev/reflect#Type)
	// Extract transforms the raw data source output into a concrete structured
	// attribute, stored on the given endpoint.
	Extract(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), data any[](https://pkg.go.dev/builtin#any), ep Endpoint[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Endpoint)) error[](https://pkg.go.dev/builtin#error)
}
```

Extractor transforms raw data into structured attributes. 
#### 

```
type Metrics struct {
	// ActiveModels is a set of models(including LoRA adapters) that are currently cached to GPU.
	ActiveModels  map[string[](https://pkg.go.dev/builtin#string)]int[](https://pkg.go.dev/builtin#int)
	WaitingModels map[string[](https://pkg.go.dev/builtin#string)]int[](https://pkg.go.dev/builtin#int)
	// MaxActiveModels is the maximum number of models that can be loaded to GPU.
	MaxActiveModels         int[](https://pkg.go.dev/builtin#int)
	RunningRequestsSize     int[](https://pkg.go.dev/builtin#int)
	WaitingQueueSize        int[](https://pkg.go.dev/builtin#int)
	KVCacheUsagePercent     float64[](https://pkg.go.dev/builtin#float64)
	KvCacheMaxTokenCapacity int[](https://pkg.go.dev/builtin#int)
	CacheBlockSize          int[](https://pkg.go.dev/builtin#int)
	// Number of GPU blocks in the model server for KV Cache.
	CacheNumBlocks int[](https://pkg.go.dev/builtin#int)

	// UpdateTime records the last time when the metrics were updated.
	UpdateTime time[](https://pkg.go.dev/time).Time[](https://pkg.go.dev/time#Time)
}
```

Metrics holds the latest metrics snapshot scraped from a pod. 
#### 

```
func NewMetrics() *Metrics[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Metrics)
```

NewMetrics initializes a new empty Metrics object. 
####  func (*Metrics) [Clone](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/metrics.go#L62) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Metrics.Clone "Go to Metrics.Clone")

```
func (m *Metrics[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Metrics)) Clone() *Metrics[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Metrics)
```

Clone creates a copy of Metrics and returns its pointer. Clone returns nil if the object being cloned is nil. 
####  func (*Metrics) [String](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/metrics.go#L53) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Metrics.String "Go to Metrics.String")

```
func (m *Metrics[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Metrics)) String() string[](https://pkg.go.dev/builtin#string)
```

String returns a string with all Metric information 
#### 

```
type ModelServer struct {
	// contains filtered or unexported fields
}
```

ModelServer is an implementation of the Endpoint interface. 
#### 

```
func NewEndpoint(meta *EndpointMetadata[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata), metrics *Metrics[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Metrics)) *ModelServer[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer)
```

NewEndpoint returns a new ModelServer with the given EndpointMetadata and Metrics. 
####  func (*ModelServer) [Clone](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/endpoint.go#L93) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer.Clone "Go to ModelServer.Clone")

```
func (srv *ModelServer[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer)) Clone() *ModelServer[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer)
```

####  func (*ModelServer) [GetAttributes](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/endpoint.go#L89) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer.GetAttributes "Go to ModelServer.GetAttributes")

```
func (srv *ModelServer[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer)) GetAttributes() AttributeMap[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#AttributeMap)
```

####  func (*ModelServer) [GetMetadata](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/endpoint.go#L73) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer.GetMetadata "Go to ModelServer.GetMetadata")

```
func (srv *ModelServer[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer)) GetMetadata() *EndpointMetadata[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata)
```

####  func (*ModelServer) [GetMetrics](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/endpoint.go#L81) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer.GetMetrics "Go to ModelServer.GetMetrics")

```
func (srv *ModelServer[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer)) GetMetrics() *Metrics[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Metrics)
```

####  func (*ModelServer) [String](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/endpoint.go#L69) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer.String "Go to ModelServer.String")

```
func (srv *ModelServer[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer)) String() string[](https://pkg.go.dev/builtin#string)
```

String returns a representation of the ModelServer. For brevity, only names of extended attributes are returned and not their values. 
####  func (*ModelServer) [UpdateMetadata](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/endpoint.go#L77) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer.UpdateMetadata "Go to ModelServer.UpdateMetadata")

```
func (srv *ModelServer[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer)) UpdateMetadata(pod *EndpointMetadata[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EndpointMetadata))
```

####  func (*ModelServer) [UpdateMetrics](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/v1.5.0/pkg/epp/framework/interface/datalayer/endpoint.go#L85) [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer.UpdateMetrics "Go to ModelServer.UpdateMetrics")

```
func (srv *ModelServer[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#ModelServer)) UpdateMetrics(metrics *Metrics[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Metrics))
```

#### 

```
type NotificationEvent struct {
	// Type is the mutation type.
	Type EventType[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#EventType)
	// Object is the current state of the object (for add/update) or the
	// last known state (for delete). Note that for delete notifications
	// only the object's name and namespace can be relied on.
	Object *unstructured[](https://pkg.go.dev/k8s.io/apimachinery/pkg/apis/meta/v1/unstructured).Unstructured[](https://pkg.go.dev/k8s.io/apimachinery/pkg/apis/meta/v1/unstructured#Unstructured)
}
```

NotificationEvent carries the event type and the affected object. Object is deep-copied by the framework core before delivery. 
#### 

```
type NotificationExtractor interface {
	Extractor[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Extractor)
	// GVK returns the GroupVersionKind this extractor handles.
	GVK() schema[](https://pkg.go.dev/k8s.io/apimachinery/pkg/runtime/schema).GroupVersionKind[](https://pkg.go.dev/k8s.io/apimachinery/pkg/runtime/schema#GroupVersionKind)
	// ExtractNotification processes a notification event. Called synchronously
	// by the source in event order.
	ExtractNotification(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), event NotificationEvent[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#NotificationEvent)) error[](https://pkg.go.dev/builtin#error)
}
```

NotificationExtractor processes k8s object events pushed from a NotificationSource. 
#### 

```
type NotificationSource interface {
	DataSource[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#DataSource)
	// GVK returns the GroupVersionKind this source watches.
	GVK() schema[](https://pkg.go.dev/k8s.io/apimachinery/pkg/runtime/schema).GroupVersionKind[](https://pkg.go.dev/k8s.io/apimachinery/pkg/runtime/schema#GroupVersionKind)
	// Notify is called by the framework core when a mutation event fires.
	// The event object is already deep-copied.
	// Returns the event (possibly modified) for Runtime to dispatch to extractors.
	// Returns nil event to signal Runtime to skip extractor dispatch.
	// TODO: ahy accept event but return *event?
	Notify(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), event NotificationEvent[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#NotificationEvent)) (*NotificationEvent[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#NotificationEvent), error[](https://pkg.go.dev/builtin#error))
}
```

NotificationSource is an event-driven DataSource for a single k8s GVK. The framework core owns the k8s notification mechanisms (e.g., watches, caches, informers) and calls the source's Notify on events. 
#### 

```
type PollingDataSource interface {
	DataSource[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#DataSource)
	// Poll fetches data for an endpoint and returns it.
	// The Runtime handles calling extractors with the returned data.
	Poll(ctx context[](https://pkg.go.dev/context).Context[](https://pkg.go.dev/context#Context), ep Endpoint[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Endpoint)) (any[](https://pkg.go.dev/builtin#any), error[](https://pkg.go.dev/builtin#error))
}
```

PollingDataSource is a poll-based DataSource that fetches data at regular intervals. 
#### 

```
type ValidatingDataSource interface {
	// ValidateExtractor allows the DataSource to perform additional validation
	// beyond the standard type compatibility checks. Return an error if validation fails.
	ValidateExtractor(extractor Extractor[](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#Extractor)) error[](https://pkg.go.dev/builtin#error)
}
```

ValidatingDataSource is an optional interface that DataSources can implement to perform additional custom validation when adding extractors. 
##  Source Files [¶](https://pkg.go.dev/sigs.k8s.io/gateway-api-inference-extension/pkg/epp/framework/interface/datalayer#section-sourcefiles "Go to Source Files")


Click to show internal directories. 
Click to hide internal directories. 
go.dev uses cookies from Google to deliver and enhance the quality of its services and to analyze traffic. [Learn more.](https://policies.google.com/technologies/cookies)
Okay
