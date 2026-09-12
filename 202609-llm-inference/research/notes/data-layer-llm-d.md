---
title: Data Layer | llm-d
id: data-layer-llm-d
tags:
- llmd-router
- llmd-router-nonk8s-backends-54b774
- data-layer
- epp-architecture
- plugin-interfaces
- non-k8s-precedent
created: '2026-09-11T20:50:16.084063Z'
updated: '2026-09-12T03:37:37.709834Z'
source: https://llm-d.ai/docs/dev/architecture/core/router/epp/datalayer
source_domain: llm-d.ai
fetched_at: '2026-09-11T20:50:16.061724Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'llm-d dev docs, canonical Data Layer design page -- the single most direct
  answer to ''where do pod/Kubernetes assumptions live and what are the plugin interfaces.''
  Defines the Data Layer as a Source -> Extract -> Attribute pipeline enriching thread-safe/type-safe
  Endpoint objects. Names THREE extension-point interface pairs: (1) PollingDataSource
  + Extractor -- periodic pull, e.g. metrics-data-source (polls a Prometheus-compatible
  /metrics endpoint of a model server) + core-metrics-extractor (multi-engine-aware,
  maps engine-specific metric names to standard internal keys like KVCacheUsagePercent/WaitingQueueSize);
  a Collector goroutine is started per endpoint by the runtime. (2) NotificationSource
  + NotificationExtractor -- reacts to Kubernetes resource changes; concrete k8s-notification-source
  watches a single configurable Kubernetes GVK (CRD or core API) and is bound to the
  controller-runtime manager, dispatching NotificationEvents on create/update/delete
  (docs note: ''There are currently no upstream extractors that require this source''
  as of this snapshot). (3) EndpointSource + EndpointExtractor -- reacts to EPP-internal
  endpoint lifecycle events (add/update/delete) via endpoint-notification-source,
  independent of Kubernetes, for cache init/cleanup. CRITICAL FINDING for non-k8s
  backends: only the NotificationSource/k8s-notification-source pairing is intrinsically
  Kubernetes-coupled (it binds to controller-runtime and watches a GVK); the PollingDataSource
  and EndpointSource interfaces are backend-agnostic by design -- metrics-data-source
  polls whatever URL/address an Endpoint object carries, and endpoint-notification-source
  only reacts to endpoints already in the EPP''s own datastore regardless of how they
  got there. This means the actual Kubernetes-pod dependency is concentrated in whichever
  DataSource populates the endpoint datastore in the first place (the pod/endpoint
  discovery mechanism itself), not in the metrics-polling or endpoint-lifecycle machinery.'
---

*Suggested by [[llm-d-routerdocsarchitecturemd-at-main-llm-dllm-d-router-github]] — canonical Data Layer design doc referenced from architecture.md*

[Skip to main content](https://llm-d.ai/docs/dev/architecture/core/router/epp/datalayer#__docusaurus_skipToContent_fallback)
🎉 **llm-d 0.9 is here!** Hardened for scale with router HA, enhanced autoscaling, end-to-end observability and expanded hardware support. [**Read the release blog →**](https://llm-d.ai/blog/llm-d-v0.9-hardened-for-scale)
This is unreleased documentation for llm-d **dev** version.
For up-to-date documentation, see the (v0.9 (latest)).
Version: dev
On this page
The Data Layer is a pluggable and extensible component within the EPP responsible for gathering, processing, and storing real-time state for model server endpoints. It provides the foundational data—such as queue depths, KV cache utilization, and LoRA adapter states—that [request scheduling](https://llm-d.ai/docs/dev/architecture/core/router/epp/scheduling) and [flow control](https://llm-d.ai/docs/dev/architecture/core/router/epp/flow-control) plugins use to make informed decisions.
## Architecture Overview[​](https://llm-d.ai/docs/dev/architecture/core/router/epp/datalayer#architecture-overview "Direct link to Architecture Overview")
The Data Layer operates as a reactive and polling-based system that enriches `Endpoint` objects with metadata and dynamic attributes. It follows a **Source - > Extract -> Attribute** lifecycle.

```
%% flowchart-v2 diagram
Datalayer Runtime
Update
Extractors
Metrics Extractor
GVK Extractor
Lifecycle Extractor
Data Sources
Polling Source
K8s Notification Source
Endpoint Event Source
Collector
Source-to-Extractor Mapping
Endpoint Attributes
```

### Core Concepts[​](https://llm-d.ai/docs/dev/architecture/core/router/epp/datalayer#core-concepts "Direct link to Core Concepts")
  * **Endpoint Attributes** : A thread-safe, type-safe map stored on each `Endpoint`. Attributes are the primary way information is shared between the Data Layer and other EPP components.
  * **Data Source** : A plugin that retrieves raw data from external systems (e.g., Prometheus metrics, Kubernetes API, or local events).
  * **Extractor** : A plugin that transforms raw data from a source into structured attributes and stores them on an endpoint.
  * **Runtime** : The logic that manages the lifecycle of sources and extractors, orchestrates polling intervals, and handles Kubernetes event bindings.


## Extension Points[​](https://llm-d.ai/docs/dev/architecture/core/router/epp/datalayer#extension-points "Direct link to Extension Points")
The Data Layer is built around three primary source types, each paired with a corresponding extractor interface:
### 1. Polling Data Sources[​](https://llm-d.ai/docs/dev/architecture/core/router/epp/datalayer#1-polling-data-sources "Direct link to 1. Polling Data Sources")
Used for periodic retrieval of data from model servers, primarily metrics.
  * **Source Interface** : `PollingDataSource` (e.g., `metrics-data-source`)
  * **Extractor Interface** : `Extractor` (e.g., `core-metrics-extractor`)
  * **Workflow** : The runtime starts a `Collector` for every new endpoint. The collector polls the source at a configured interval and passes the result to associated extractors. Metrics pulling is the primary motivation for this type of data source.


### 2. Notification Sources[​](https://llm-d.ai/docs/dev/architecture/core/router/epp/datalayer#2-notification-sources "Direct link to 2. Notification Sources")
Used for reacting to changes in Kubernetes resources.
  * **Source Interface** : `NotificationSource` (e.g., `k8s-notification-source`)
  * **Extractor Interface** : `NotificationExtractor`
  * **Workflow** : The runtime binds these sources to the Kubernetes controller-runtime manager. When a resource (e.g., a Pod or Service) changes, the source dispatches an event to its extractors.


### 3. Endpoint Event Sources[​](https://llm-d.ai/docs/dev/architecture/core/router/epp/datalayer#3-endpoint-event-sources "Direct link to 3. Endpoint Event Sources")
Used for reacting to endpoint lifecycle events (Add, Update, Delete) within the EPP itself.
  * **Source Interface** : `EndpointSource` (e.g., `endpoint-notification-source`)
  * **Extractor Interface** : `EndpointExtractor`
  * **Workflow** : When the EPP's endpoint pool changes, these sources notify extractors to perform setup or cleanup (e.g., initializing caches or purging stale metrics).


## Concrete Plugins[​](https://llm-d.ai/docs/dev/architecture/core/router/epp/datalayer#concrete-plugins "Direct link to Concrete Plugins")
### Sources[​](https://llm-d.ai/docs/dev/architecture/core/router/epp/datalayer#sources "Direct link to Sources")
  * - polls a Prometheus-compatible metrics endpoint of a model server and parses the response into a structured format for extraction.
  * — can be configured to watch a single Kubernetes GVK (either a CRD or a k8s core API) and dispatches `NotificationEvent`s to registered `NotificationExtractor`s when objects are created, updated, or deleted. Multiple extractors can be notified of GVK changes by a single source. There are currently no upstream extractors that require this source.
  * **[`endpoint-notification-source`](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/datalayer/source/notifications)**— delivers endpoint (aka model server engine) lifecycle events (add, update, delete) to registered`EndpointExtractor` s whenever an endpoint is added to or removed from the datastore.


### Extractors[​](https://llm-d.ai/docs/dev/architecture/core/router/epp/datalayer#extractors "Direct link to Extractors")
  * - responsible for extracting a set of model server metrics sourced from `metrics-data-source` and storing them as endpoint attributes (such as `KVCacheUsagePercent` and `WaitingQueueSize`), which scorers like `kv-cache-utilization-scorer` consume. It supports multiple inference engines and can be configured to map engine-specific metric names to a standard set of internal keys.


  * [Architecture Overview](https://llm-d.ai/docs/dev/architecture/core/router/epp/datalayer#architecture-overview)
  * [Extension Points](https://llm-d.ai/docs/dev/architecture/core/router/epp/datalayer#extension-points)
    * [1. Polling Data Sources](https://llm-d.ai/docs/dev/architecture/core/router/epp/datalayer#1-polling-data-sources)
    * [2. Notification Sources](https://llm-d.ai/docs/dev/architecture/core/router/epp/datalayer#2-notification-sources)
    * [3. Endpoint Event Sources](https://llm-d.ai/docs/dev/architecture/core/router/epp/datalayer#3-endpoint-event-sources)
  * [Concrete Plugins](https://llm-d.ai/docs/dev/architecture/core/router/epp/datalayer#concrete-plugins)


