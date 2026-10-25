---
title: llm-d-router module - github.com/llm-d/llm-d-router - Go Packages
id: llm-d-router-module-githubcomllm-dllm-d-router-go-packages
tags:
- llmd-router-nonk8s
- llmd-router-nonk8s-backends-54b774
- go-packages
- endpoint-discovery
- plugin-catalog
created: '2026-09-11T19:43:04.552145Z'
updated: '2026-09-15T04:55:23.668170Z'
source: https://pkg.go.dev/github.com/llm-d/llm-d-router
source_domain: pkg.go.dev
fetched_at: '2026-09-11T19:43:04.533553Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'pkg.go.dev auto-generated package index for github.com/llm-d/llm-d-router@v0.10.0
  (dated as of fetch, module version pinned in URLs to v0.10.0, matching the Aug 17
  2026 release). Provides the authoritative, dated list of exported Go packages, confirming
  exact plugin taxonomy as of this release: (1) pkg/epp/framework/interface/datalayer
  -- the interface package (contains EndpointDiscovery/DiscoveryNotifier per the no-Kubernetes
  blog); (2) pkg/epp/framework/plugins/datalayer/discovery/file, documented inline
  as ''a file-based EndpointDiscovery implementation that reads a YAML (or JSON) file
  listing inference endpoints'' -- this is the ONLY discovery-backend implementation
  package publicly exported at v0.10.0 (no discovery/kube or discovery/consul package
  is listed; a ''Click to show internal directories'' toggle suggests the Kubernetes-watch
  based discovery, if refactored at all, is either internal/unexported or still embedded
  directly in the EPP''s controller-runtime manager rather than a separate plugin
  package -- corroborating the blog''s statement that Kube discovery has not yet moved
  behind the interface). Also lists pkg/epp/framework/plugins/scheduling/filter/bylabel
  (filter plugins including the P/D role-based filter), pkg/epp/framework/plugins/scheduling/filter/endpointattribute
  (''a generic filter plugin that filters candidate endpoints by a single configured
  numeric endpoint attribute'' -- directly reusable for custom backend-supplied metrics),
  pkg/epp/framework/plugins/datalayer/extractor/dcgm and source/dcgm (GPU metrics
  extraction), pkg/epp/framework/plugins/datalayer/source/http (a generic HTTP data
  source), and full scorer/filter/picker plugin catalog (kvcacheutilization, loadaware,
  prefix, queuedepth, runningrequests, sessionaffinity, topologyaffinity, latency,
  mmcacheaffinity, headerlabelaffinity, nohitlru, preciseprefixcache). Confirms core
  client-go generated types are scoped to apix/v1alpha2 (InferenceObjective/InferenceModelRewrite),
  consistent with the README''s statement that InferencePool itself lives in the separate
  GIE repo.'
utility_score: 13.0
---

##  README [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router#section-readme "Go to Readme")
### llm-d Router
> [!IMPORTANT] **Terminology Change** : The _Inference Scheduler_ has been renamed to **llm-d Router** ; see [Terminology](https://github.com/llm-d/llm-d-router/blob/v0.10.0/README.md).
> [!IMPORTANT] **API & Code Consolidation**: Core Endpoint Picker (EPP) code and the `InferenceObjective` and `InferenceModelRewrite` APIs have been merged into this repository from [Gateway API Inference Extension (GIE)](https://github.com/kubernetes-sigs/gateway-api-inference-extension). The GIE repository now exclusively hosts the `InferencePool` API—an extension of the [Kubernetes Gateway API](https://gateway-api.sigs.k8s.io/)—and defines the Endpoint Picker Protocol.
The **llm-d Router** is the intelligent entry point for inference traffic, delivering LLM load and prefix-cache aware routing, request prioritization, and advanced flow control across diverse request formats to fulfill complex serving objectives. It supports a flexible deployment model: it can run in **Standalone Mode** (where a self-managed Envoy proxy runs alongside the EPP in the same pod) or integrate with L7 load balancers—including self-managed instances (e.g., Istio, AgentGateway) and cloud-managed services (e.g., Google Cloud's Application Load Balancer)—via the Kubernetes Gateway API.
The router achieves its intelligence through an **Endpoint Picker (EPP)** that integrates with production-grade proxies (such as [Envoy](https://github.com/envoyproxy/envoy)) via the [ext-proc](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/ext_proc_filter) protocol, injecting real-time signals into the data plane to optimize request placement.
#### Core Components and APIs
This repository hosts the following core components:
  * **Endpoint Picker (EPP)** : The intelligent routing engine that serves as the "brain" of the router. It evaluates incoming requests against the current state of the [InferencePool](https://github.com/kubernetes-sigs/gateway-api-inference-extension), considering factors like KV-cache locality, current load, and priority to make optimal placement decisions. It integrates with L7 proxies via the `ext-proc` protocol.
  * **Request Management APIs** : These resources directly influence the EPP's request handling behavior: 
    * **InferenceObjective** : Configures the EPP's scheduling goals for specific requests, including priority levels and performance targets.
    * **InferenceModelRewrite** : Directs the EPP to perform model name rewriting, enabling flexible traffic management for A/B testing and canary rollouts.
  * **Disaggregation Sidecar** : A coordination component deployed alongside model servers (typically as a sidecar to the decode worker). It orchestrates complex multi-stage inference lifecycles, such as **P/D (Prefill/Decode)** and **E/P/D (Encode/Prefill/Decode)** , by communicating with specialized encode and prefill workers to manage KV-cache and embedding transfers. For more details, see the [Disaggregation Documentation](https://github.com/llm-d/llm-d-router/blob/v0.10.0/docs/disaggregation.md).


#### Modes of Operation
The llm-d Router supports two primary deployment modes as specified in the [Kubernetes Gateway API Inference Extensions](https://github.com/kubernetes-sigs/gateway-api-inference-extension):
##### 1. Standalone Mode
A lightweight deployment where a self-managed Envoy proxy runs alongside the EPP in the same pod. This mode is ideal for clusters without Gateway API infrastructure or for basic testing and local evaluations.
##### 2. Gateway Mode (Inference Gateway)
The recommended mode for production environments, leveraging the official [Gateway API](https://github.com/kubernetes-sigs/gateway-api). In this mode, the EPP acts as a backend for an `InferencePool`, which is referenced by an `HTTPRoute` on a shared `Gateway`. This enables advanced traffic management, multi-cluster load balancing, and shared infrastructure for both inference and traditional workloads.
For more details on the router architecture, routing logic, and different plugins (filters and scorers), see the [Architecture Documentation](https://github.com/llm-d/llm-d-router/blob/v0.10.0/docs/architecture.md). For resource provisioning and container sizing recommendations under heavy or long-context workloads, see the [EPP Container Sizing Guide](https://github.com/llm-d/llm-d-router/blob/v0.10.0/docs/operations.md).
> [!NOTE] The project provides tools for automatic Envoy installation. However, if you install or configure it yourself, please note that the only supported [request_body_mode and response_body_mode](https://www.envoyproxy.io/docs/envoy/latest/api-v3/service/ext_proc/v3/external_processor.proto) is `FULL_DUPLEX_STREAMED`
#### Terminology
To ensure clarity across the project, we use the following standard terminology:
  * **llm-d Router** : The complete intelligent entry point, comprising both the **Proxy** (e.g., Envoy) and the **Endpoint Picker (EPP)**. This term replaces "Inference Scheduler" in all contexts.
  * **llm-d Endpoint Picker (EPP)** : The specific component that implements the routing intelligence and scoring logic. Use this term when referring to capabilities or configurations specific to the EPP itself, rather than the request routing system as a whole.
  * **Inference Gateway** : A synonym for the **llm-d Router** when operating in **Gateway Mode**.
  * **Request Scheduler** : A sub-component within the EPP responsible for the queuing and dispatching of requests.


#### Contributing
Start with the [llm-d organization contributing guide](https://github.com/llm-d/llm-d/blob/main/CONTRIBUTING.md) for project-wide guidelines, code of conduct, and community resources.
Our community meeting is bi-weekly at Wednesday 10AM PDT ([Google Meet](https://meet.google.com/ozx-goao-cxh), [Meeting Notes](https://docs.google.com/document/d/1Pf3x7ZM8nNpU56nt6CzePAOmFZ24NXDeXyaYb565Wq4)).
We currently utilize the [#sig-router](https://llm-d.slack.com/?redir=%2Fmessages%2Fsig-router) channel in llm-d Slack workspace for communications.
For large changes please [create an issue](https://github.com/llm-d/llm-d-router/issues/new) first describing the change so the maintainers can do an assessment, and work on the details with you. See [DEVELOPMENT.md](https://github.com/llm-d/llm-d-router/blob/v0.10.0/DEVELOPMENT.md) for details on how to work with the codebase.
Contributions are welcome!
#### License
Expand ▾ Collapse ▴
##  Directories [¶](https://pkg.go.dev/github.com/llm-d/llm-d-router#section-directories "Go to Directories")
Show internal  Expand all  
| Path  | Synopsis  |  
| --- | --- |  
|  Package v1alpha1 contains API Schema definitions for the llm-d.ai API group.  | Package v1alpha1 contains API Schema definitions for the llm-d.ai API group.  |  
|  Package v1alpha2 contains API Schema definitions for the llm-d.ai API group.  | Package v1alpha2 contains API Schema definitions for the llm-d.ai API group.  |  
|  client-go  |  
|   |  
|  [applyconfiguration/apix/v1alpha2](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/client-go/applyconfiguration/apix/v1alpha2)  |  
|  [applyconfiguration/internal](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/client-go/applyconfiguration/internal)  |  
|   |  
|  This package has the automatically generated fake clientset.  | This package has the automatically generated fake clientset.  |  
|  This package contains the scheme of the automatically generated clientset.  | This package contains the scheme of the automatically generated clientset.  |  
|  [clientset/versioned/typed/apix/v1alpha2](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/client-go/clientset/versioned/typed/apix/v1alpha2) This package has the automatically generated typed clients.  | This package has the automatically generated typed clients.  |  
|  [clientset/versioned/typed/apix/v1alpha2/fake](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/client-go/clientset/versioned/typed/apix/v1alpha2/fake) Package fake has the automatically generated clients.  | Package fake has the automatically generated clients.  |  
|   |  
|  [informers/externalversions/apix](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/client-go/informers/externalversions/apix)  |  
|  [informers/externalversions/apix/v1alpha2](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/client-go/informers/externalversions/apix/v1alpha2)  |  
|  [informers/externalversions/internalinterfaces](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/client-go/informers/externalversions/internalinterfaces)  |  
|   |  
|  [coordinator](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/cmd/coordinator) command  |  
|  Package main contains the "Endpoint Picker (EPP)" program for scheduling inference requests.  | Package main contains the "Endpoint Picker (EPP)" program for scheduling inference requests.  |  
|   |  
|  [pd-sidecar](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/cmd/pd-sidecar) command  |  
|  internal  |  
|   |  
|  Package common defines structs for referring to fully qualified k8s resources.  | Package common defines structs for referring to fully qualified k8s resources.  |  
|   |  
|   |  
|   |  
|  [common/observability/logging](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/common/observability/logging)  |  
|  [common/observability/metrics](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/common/observability/metrics)  |  
|  [common/observability/profiling](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/common/observability/profiling)  |  
|  [common/observability/tracing](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/common/observability/tracing)  |  
|   |  
|  Package routing contains routing constants and utilities shared between the EPP/Inference-Scheduler and the Routing Sidecar.  | Package routing contains routing constants and utilities shared between the EPP/Inference-Scheduler and the Routing Sidecar.  |  
|  [coordinator/common/httplog](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/coordinator/common/httplog) Package httplog provides helpers for logging HTTP headers safely and consistently across services.  | Package httplog provides helpers for logging HTTP headers safely and consistently across services.  |  
|   |  
|  Package ec contains EC (encoder cache) transfer connector implementations selected at config time.  | Package ec contains EC (encoder cache) transfer connector implementations selected at config time.  |  
|  Package kv contains KV transfer connector implementations selected at config time.  | Package kv contains KV transfer connector implementations selected at config time.  |  
|   |  
|   |  
|  [coordinator/pipeline/builder](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/coordinator/pipeline/builder) Package builder assembles a coordinator pipeline from configuration.  | Package builder assembles a coordinator pipeline from configuration.  |  
|   |  
|   |  
|   |  
|   |  
|   |  
|   |  
|   |  
|   |  
|   |  
|  Package flowcontrol wires the flow control system: the FlowController data plane and the FlowRegistry control plane.  | Package flowcontrol wires the flow control system: the FlowController data plane and the FlowRegistry control plane.  |  
|  Package benchmark load-tests the Flow Control layer.  | Package benchmark load-tests the Flow Control layer.  |  
|  Package contracts defines the boundaries and service interfaces for the Flow Control system.  | Package contracts defines the boundaries and service interfaces for the Flow Control system.  |  
|  [epp/flowcontrol/contracts/mocks](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/flowcontrol/contracts/mocks) Package mocks provides mocks for the interfaces defined in the `contracts` package.  | Package mocks provides mocks for the interfaces defined in the `contracts` package.  |  
|  [epp/flowcontrol/controller](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/flowcontrol/controller) Package controller contains the implementation of the FlowController engine.  | Package controller contains the implementation of the FlowController engine.  |  
|  [epp/flowcontrol/controller/internal](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/flowcontrol/controller/internal) Package internal provides the core worker implementation for the controller.FlowController.  | Package internal provides the core worker implementation for the controller.FlowController.  |  
|   |  
|  Package queue provides the priority-ordered SafeQueue used by flow control.  | Package queue provides the priority-ordered SafeQueue used by flow control.  |  
|  Package registry provides the concrete implementation of the `contracts.FlowRegistry` interface.  | Package registry provides the concrete implementation of the `contracts.FlowRegistry` interface.  |  
|  Package types defines the fundamental data structures, interfaces, and errors that form the vocabulary of the Flow Control system.  | Package types defines the fundamental data structures, interfaces, and errors that form the vocabulary of the Flow Control system.  |  
|  [epp/framework/common/request](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/common/request)  |  
|  [epp/framework/interface/datalayer](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/datalayer)  |  
|  [epp/framework/interface/flowcontrol](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/flowcontrol) Package flowcontrol defines the core plugin interfaces for extending the Flow Control layer.  | Package flowcontrol defines the core plugin interfaces for extending the Flow Control layer.  |  
|  [epp/framework/interface/flowcontrol/mocks](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/flowcontrol/mocks)  |  
|  [epp/framework/interface/plugin](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/plugin)  |  
|  [epp/framework/interface/requestcontrol](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/requestcontrol)  |  
|  [epp/framework/interface/requesthandling](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/requesthandling)  |  
|  [epp/framework/interface/scheduling](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/interface/scheduling)  |  
|  [epp/framework/observability/multimodal](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/observability/multimodal) Package multimodal provides span-attribute helpers derived from a request's multimodal features.  | Package multimodal provides span-attribute helpers derived from a request's multimodal features.  |  
|  [epp/framework/plugins/datalayer/attribute/concurrency](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/attribute/concurrency)  |  
|  [epp/framework/plugins/datalayer/attribute/gpu](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/attribute/gpu)  |  
|  [epp/framework/plugins/datalayer/attribute/latency](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/attribute/latency)  |  
|  [epp/framework/plugins/datalayer/attribute/metrics](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/attribute/metrics)  |  
|  [epp/framework/plugins/datalayer/attribute/models](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/attribute/models)  |  
|  [epp/framework/plugins/datalayer/attribute/multimodal](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/attribute/multimodal)  |  
|  [epp/framework/plugins/datalayer/attribute/prefix](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/attribute/prefix)  |  
|  [epp/framework/plugins/datalayer/attribute/session](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/attribute/session) Package session declares the SessionID attribute that carries per-request session identity for affinity scoring and filtering.  | Package session declares the SessionID attribute that carries per-request session identity for affinity scoring and filtering.  |  
|  [epp/framework/plugins/datalayer/attribute/topology](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/attribute/topology) Package topology declares the Topology attribute that carries endpoint locality information for topology-aware routing.  | Package topology declares the Topology attribute that carries endpoint locality information for topology-aware routing.  |  
|  [epp/framework/plugins/datalayer/cross_plugin](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/cross_plugin)  |  
|  [epp/framework/plugins/datalayer/discovery/file](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/discovery/file) Package file provides a file-based EndpointDiscovery implementation that reads a YAML (or JSON) file listing inference endpoints.  | Package file provides a file-based EndpointDiscovery implementation that reads a YAML (or JSON) file listing inference endpoints.  |  
|  [epp/framework/plugins/datalayer/extractor/dcgm](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/extractor/dcgm)  |  
|  [epp/framework/plugins/datalayer/extractor/metrics](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/extractor/metrics)  |  
|  [epp/framework/plugins/datalayer/extractor/mocks](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/extractor/mocks)  |  
|  [epp/framework/plugins/datalayer/extractor/models](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/extractor/models)  |  
|  [epp/framework/plugins/datalayer/extractor/topology](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/extractor/topology) Package topology provides a datalayer plugin that stamps each endpoint with a Topology attribute containing locality fields (hostname, zone, region).  | Package topology provides a datalayer plugin that stamps each endpoint with a Topology attribute containing locality fields (hostname, zone, region).  |  
|  [epp/framework/plugins/datalayer/source/dcgm](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/source/dcgm)  |  
|  [epp/framework/plugins/datalayer/source/http](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/source/http)  |  
|  [epp/framework/plugins/datalayer/source/http/httptest](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/source/http/httptest) Package httptest provides test helpers for HTTPDataSource parser implementations.  | Package httptest provides test helpers for HTTPDataSource parser implementations.  |  
|  [epp/framework/plugins/datalayer/source/metrics](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/source/metrics)  |  
|  [epp/framework/plugins/datalayer/source/mocks](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/source/mocks)  |  
|  [epp/framework/plugins/datalayer/source/models](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/source/models)  |  
|  [epp/framework/plugins/datalayer/source/notifications](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/datalayer/source/notifications)  |  
|  [epp/framework/plugins/flowcontrol/eviction/filtering](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/flowcontrol/eviction/filtering) Package filtering provides built-in EvictionFilterPolicy implementations.  | Package filtering provides built-in EvictionFilterPolicy implementations.  |  
|  [epp/framework/plugins/flowcontrol/eviction/ordering](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/flowcontrol/eviction/ordering)  |  
|  [epp/framework/plugins/flowcontrol/fairness](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/flowcontrol/fairness) Package fairness provides the standard implementations of the FairnessPolicy interface.  | Package fairness provides the standard implementations of the FairnessPolicy interface.  |  
|  [epp/framework/plugins/flowcontrol/fairness/globalstrict](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/flowcontrol/fairness/globalstrict) Package globalstrict implements a greedy fairness policy that enforces a strict global ordering across all flows within a priority band, ignoring flow boundaries.  | Package globalstrict implements a greedy fairness policy that enforces a strict global ordering across all flows within a priority band, ignoring flow boundaries.  |  
|  [epp/framework/plugins/flowcontrol/fairness/program-aware](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/flowcontrol/fairness/program-aware) Package programaware implements a flow-control fairness policy that schedules per-program queues using a swappable scoring strategy.  | Package programaware implements a flow-control fairness policy that schedules per-program queues using a swappable scoring strategy.  |  
|  [epp/framework/plugins/flowcontrol/fairness/roundrobin](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/flowcontrol/fairness/roundrobin) Package roundrobin implements a fairness policy that selects queues from a priority band using a round-robin strategy, cycling through active flows one by one to guarantee that no single flow can starve others.  | Package roundrobin implements a fairness policy that selects queues from a priority band using a round-robin strategy, cycling through active flows one by one to guarantee that no single flow can starve others.  |  
|  [epp/framework/plugins/flowcontrol/ordering](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/flowcontrol/ordering) Package ordering provides the standard implementations of the OrderingPolicy interface.  | Package ordering provides the standard implementations of the OrderingPolicy interface.  |  
|  [epp/framework/plugins/flowcontrol/ordering/edf](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/flowcontrol/ordering/edf) Package edf implements an ordering policy that selects requests based on their absolute deadlines (Earliest Deadline First).  | Package edf implements an ordering policy that selects requests based on their absolute deadlines (Earliest Deadline First).  |  
|  [epp/framework/plugins/flowcontrol/ordering/fcfs](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/flowcontrol/ordering/fcfs) Package fcfs implements an ordering policy that selects requests based on their arrival order at the Flow Control layer (First-Come, First-Served).  | Package fcfs implements an ordering policy that selects requests based on their arrival order at the Flow Control layer (First-Come, First-Served).  |  
|  [epp/framework/plugins/flowcontrol/ordering/slodeadline](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/flowcontrol/ordering/slodeadline) Package slodeadline implements an ordering policy that selects requests based on an SLO-based deadline derived from request headers.  | Package slodeadline implements an ordering policy that selects requests based on an SLO-based deadline derived from request headers.  |  
|  [epp/framework/plugins/flowcontrol/saturationdetector/concurrency](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/flowcontrol/saturationdetector/concurrency)  |  
|  [epp/framework/plugins/flowcontrol/saturationdetector/utilization](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/flowcontrol/saturationdetector/utilization)  |  
|  [epp/framework/plugins/flowcontrol/usagelimits](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/flowcontrol/usagelimits)  |  
|  [epp/framework/plugins/flowcontrol/usagelimits/priorityholdback](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/flowcontrol/usagelimits/priorityholdback) Package priorityholdback implements a UsageLimitPolicy that computes differentiated admission ceilings per priority level.  | Package priorityholdback implements a UsageLimitPolicy that computes differentiated admission ceilings per priority level.  |  
|  [epp/framework/plugins/flowcontrol/usagelimits/softreflectiveceiling](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/flowcontrol/usagelimits/softreflectiveceiling) Package softreflectiveceiling implements a UsageLimitPolicy that gates lower-priority bands proportionally as saturation rises.  | Package softreflectiveceiling implements a UsageLimitPolicy that gates lower-priority bands proportionally as saturation rises.  |  
|  [epp/framework/plugins/requestcontrol](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol) Package requestcontrol holds definitions shared by all requestcontrol framework plugins.  | Package requestcontrol holds definitions shared by all requestcontrol framework plugins.  |  
|  [epp/framework/plugins/requestcontrol/admitter/latencyslo](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/admitter/latencyslo)  |  
|  [epp/framework/plugins/requestcontrol/admitter/probabilisticadmitter](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/admitter/probabilisticadmitter) Package probabilisticadmitter implements a binary-tier probabilistic admission control plugin.  | Package probabilisticadmitter implements a binary-tier probabilistic admission control plugin.  |  
|  [epp/framework/plugins/requestcontrol/dataproducer/approximateprefix](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix)  |  
|  [epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/constants](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/constants)  |  
|  [epp/framework/plugins/requestcontrol/dataproducer/burstprefix](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/dataproducer/burstprefix) Package burstprefix provides a request-level data producer that co-locates bursts of prompt-sharing requests.  | Package burstprefix provides a request-level data producer that co-locates bursts of prompt-sharing requests.  |  
|  [epp/framework/plugins/requestcontrol/dataproducer/inflightload](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/dataproducer/inflightload)  |  
|  [epp/framework/plugins/requestcontrol/dataproducer/inflightload/constants](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/dataproducer/inflightload/constants)  |  
|  [epp/framework/plugins/requestcontrol/dataproducer/multimodal](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/dataproducer/multimodal) Package multimodal provides a data producer for multimodal encoder-cache affinity.  | Package multimodal provides a data producer for multimodal encoder-cache affinity.  |  
|  [epp/framework/plugins/requestcontrol/dataproducer/p2psource](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/dataproducer/p2psource) Package p2psource emits the KV cache source header: a candidate pod within one block of the most cached prefix KV blocks for the request, for the routing sidecar to pull from over the P2P connector instead of recomputing them.  | Package p2psource emits the KV cache source header: a candidate pod within one block of the most cached prefix KV blocks for the request, for the routing sidecar to pull from over the P2P connector instead of recomputing them.  |  
|  [epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache) Package preciseprefixcache hashes TokenizedPrompt into KV-block keys, looks them up in the index, and writes per-endpoint PrefixCacheMatchInfo.  | Package preciseprefixcache hashes TokenizedPrompt into KV-block keys, looks them up in the index, and writes per-endpoint PrefixCacheMatchInfo.  |  
|  [epp/framework/plugins/requestcontrol/dataproducer/predictedlatency](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency) Package requestcontrol contains helpers to decouple latency-predictor logic.  | Package requestcontrol contains helpers to decouple latency-predictor logic.  |  
|  [epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/constants](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/constants)  |  
|  [epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient)  |  
|  [epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient/tests](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/latencypredictorclient/tests) command  |  
|  [epp/framework/plugins/requestcontrol/dataproducer/prefixhash](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/dataproducer/prefixhash) Package prefixhash computes stable prefix block hashes for a tokenized prompt.  | Package prefixhash computes stable prefix block hashes for a tokenized prompt.  |  
|  [epp/framework/plugins/requestcontrol/dataproducer/sessionid](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/dataproducer/sessionid) Package sessionid provides a DataProducer that extracts a session identifier from a configured request header or named cookie and publishes it as a SessionID attribute on the InferenceRequest attribute store, so that affinity-aware scorers and filters can read it without knowing how the session was carried on the wire.  | Package sessionid provides a DataProducer that extracts a session identifier from a configured request header or named cookie and publishes it as a SessionID attribute on the InferenceRequest attribute store, so that affinity-aware scorers and filters can read it without knowing how the session was carried on the wire.  |  
|  [epp/framework/plugins/requestcontrol/dataproducer/sessionid/constants](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/dataproducer/sessionid/constants)  |  
|  [epp/framework/plugins/requestcontrol/dataproducer/tokenizer](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/dataproducer/tokenizer) Package tokenizer provides a DataProducer plugin that tokenizes the request prompt and publishes the result on InferenceRequestBody.TokenizedPrompt for downstream consumers (scorers, filters, other data producers).  | Package tokenizer provides a DataProducer plugin that tokenizes the request prompt and publishes the result on InferenceRequestBody.TokenizedPrompt for downstream consumers (scorers, filters, other data producers).  |  
|  [epp/framework/plugins/requestcontrol/requestattributereporter](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/requestattributereporter)  |  
|  [epp/framework/plugins/requestcontrol/requestheader/agentidentity](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/requestheader/agentidentity) Package agentidentity provides a RequestHeaderProcessor plugin that resolves agent identity from provider-specific headers and stores it as a request attribute for use by other subsystems (flow control, scheduling, KV cache, etc.).  | Package agentidentity provides a RequestHeaderProcessor plugin that resolves agent identity from provider-specific headers and stores it as a request attribute for use by other subsystems (flow control, scheduling, KV cache, etc.).  |  
|  [epp/framework/plugins/requestcontrol/screener/disaggregatedsetrollout](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/screener/disaggregatedsetrollout) Package disaggregatedsetrollout provides a request-control Screener for safe rollouts across roles of a disaggregated inference deployment.  | Package disaggregatedsetrollout provides a request-control Screener for safe rollouts across roles of a disaggregated inference deployment.  |  
|  [epp/framework/plugins/requestcontrol/test](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/test)  |  
|  [epp/framework/plugins/requestcontrol/test/responsereceived](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requestcontrol/test/responsereceived)  |  
|  [epp/framework/plugins/requesthandling/parsers](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requesthandling/parsers)  |  
|  [epp/framework/plugins/requesthandling/parsers/anthropic](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requesthandling/parsers/anthropic)  |  
|  [epp/framework/plugins/requesthandling/parsers/openai](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requesthandling/parsers/openai)  |  
|  [epp/framework/plugins/requesthandling/parsers/passthrough](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requesthandling/parsers/passthrough)  |  
|  [epp/framework/plugins/requesthandling/parsers/util](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requesthandling/parsers/util)  |  
|  [epp/framework/plugins/requesthandling/parsers/vertexai](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requesthandling/parsers/vertexai)  |  
|  [epp/framework/plugins/requesthandling/parsers/vllmgrpc](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requesthandling/parsers/vllmgrpc)  |  
|  [epp/framework/plugins/requesthandling/parsers/vllmgrpc/api/gen](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requesthandling/parsers/vllmgrpc/api/gen)  |  
|  [epp/framework/plugins/requesthandling/parsers/vllmhttp](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/requesthandling/parsers/vllmhttp) Package vllmhttp provides a request parser for vLLM HTTP endpoints that are not part of the OpenAI-compatible API surface — specifically /inference/v1/generate.  | Package vllmhttp provides a request parser for vLLM HTTP endpoints that are not part of the OpenAI-compatible API surface — specifically /inference/v1/generate.  |  
|  [epp/framework/plugins/scheduling](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling) Package scheduling holds definitions shared by all scheduling framework plugins.  | Package scheduling holds definitions shared by all scheduling framework plugins.  |  
|  [epp/framework/plugins/scheduling/filter/bylabel](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/filter/bylabel) Package bylabel provides filter plugins for the epp.  | Package bylabel provides filter plugins for the epp.  |  
|  [epp/framework/plugins/scheduling/filter/endpointattribute](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/filter/endpointattribute) Package endpointattribute provides a generic filter plugin that filters candidate endpoints by a single configured numeric endpoint attribute.  | Package endpointattribute provides a generic filter plugin that filters candidate endpoints by a single configured numeric endpoint attribute.  |  
|  [epp/framework/plugins/scheduling/filter/prefixcacheaffinity](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/filter/prefixcacheaffinity) Package prefixcacheaffinity provides a probabilistic filter that narrows candidates to "sticky" endpoints (those with high prefix cache scores).  | Package prefixcacheaffinity provides a probabilistic filter that narrows candidates to "sticky" endpoints (those with high prefix cache scores).  |  
|  [epp/framework/plugins/scheduling/filter/sessionaffinity](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/filter/sessionaffinity) Package sessionaffinity provides a session affinity filter plugin for the epp.  | Package sessionaffinity provides a session affinity filter plugin for the epp.  |  
|  [epp/framework/plugins/scheduling/filter/sloheadroomtier](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/filter/sloheadroomtier) Package headroomtier provides a probabilistic filter that selects endpoints based on predicted latency headroom (SLO - predicted).  | Package headroomtier provides a probabilistic filter that selects endpoints based on predicted latency headroom (SLO - predicted).  |  
|  [epp/framework/plugins/scheduling/filter/topologyaffinity](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/filter/topologyaffinity) Package topologyaffinity provides a filter that keeps candidate endpoints whose topology is co-located with the peer endpoint at a configured minimum affinity level.  | Package topologyaffinity provides a filter that keeps candidate endpoints whose topology is co-located with the peer endpoint at a configured minimum affinity level.  |  
|  [epp/framework/plugins/scheduling/filter/utilization](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/filter/utilization) Package utilization provides a filter that drops endpoints whose utilization metric exceeds a configured maximum.  | Package utilization provides a filter that drops endpoints whose utilization metric exceeds a configured maximum.  |  
|  [epp/framework/plugins/scheduling/picker](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/picker) Package picker provides the standard implementations of the Picker interface for the scheduling framework.  | Package picker provides the standard implementations of the Picker interface for the scheduling framework.  |  
|  [epp/framework/plugins/scheduling/picker/maxscore](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/picker/maxscore) Package maxscore implements a scheduling picker that selects the endpoint(s) with the highest score calculated during the scoring phase.  | Package maxscore implements a scheduling picker that selects the endpoint(s) with the highest score calculated during the scoring phase.  |  
|  [epp/framework/plugins/scheduling/picker/random](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/picker/random) Package random implements a scheduling picker that selects endpoints uniformly at random, ignoring any scores calculated by scorer plugins.  | Package random implements a scheduling picker that selects endpoints uniformly at random, ignoring any scores calculated by scorer plugins.  |  
|  [epp/framework/plugins/scheduling/picker/weightedrandom](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/picker/weightedrandom) Package weightedrandom implements a scheduling picker that selects endpoints randomly, where the probability of an endpoint being selected is proportional to its score.  | Package weightedrandom implements a scheduling picker that selects endpoints randomly, where the probability of an endpoint being selected is proportional to its score.  |  
|  [epp/framework/plugins/scheduling/profilehandler/dataparallel](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/profilehandler/dataparallel) Package dataparallel provides a data-parallel profile handler plugin for the epp.  | Package dataparallel provides a data-parallel profile handler plugin for the epp.  |  
|  [epp/framework/plugins/scheduling/profilehandler/disagg](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/profilehandler/disagg) Package disagg provides profile handler plugins for the epp.  | Package disagg provides profile handler plugins for the epp.  |  
|  [epp/framework/plugins/scheduling/profilehandler/headerprofile](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/profilehandler/headerprofile)  |  
|  [epp/framework/plugins/scheduling/profilehandler/single](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/profilehandler/single)  |  
|  [epp/framework/plugins/scheduling/scorer/activerequest](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/activerequest) Package activerequest provides an active request scorer plugin for the epp.  | Package activerequest provides an active request scorer plugin for the epp.  |  
|  [epp/framework/plugins/scheduling/scorer/contextlengthaware](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/contextlengthaware) Package contextlengthaware provides plugins that implement multiple extension points (e.g., both Filter and Scorer interfaces).  | Package contextlengthaware provides plugins that implement multiple extension points (e.g., both Filter and Scorer interfaces).  |  
|  [epp/framework/plugins/scheduling/scorer/endpointattribute](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/endpointattribute)  |  
|  [epp/framework/plugins/scheduling/scorer/headerlabelaffinity](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/headerlabelaffinity) Package headerlabelaffinity provides a scorer that prefers endpoints whose configured label matches a request header.  | Package headerlabelaffinity provides a scorer that prefers endpoints whose configured label matches a request header.  |  
|  [epp/framework/plugins/scheduling/scorer/kvcacheutilization](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/kvcacheutilization)  |  
|  [epp/framework/plugins/scheduling/scorer/latency](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/latency) Package latency provides a scorer that scores endpoints based on predicted latency headroom.  | Package latency provides a scorer that scores endpoints based on predicted latency headroom.  |  
|  [epp/framework/plugins/scheduling/scorer/loadaware](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/loadaware) Package loadaware provides a load-aware scorer plugin for the epp.  | Package loadaware provides a load-aware scorer plugin for the epp.  |  
|  [epp/framework/plugins/scheduling/scorer/loraaffinity](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/loraaffinity)  |  
|  [epp/framework/plugins/scheduling/scorer/mmcacheaffinity](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/mmcacheaffinity) Package mmcacheaffinity scores endpoints from multimodal encoder-cache match info produced by the request-control multimodal data producer.  | Package mmcacheaffinity scores endpoints from multimodal encoder-cache match info produced by the request-control multimodal data producer.  |  
|  [epp/framework/plugins/scheduling/scorer/nohitlru](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/nohitlru) Package nohitlru provides a no-hit LRU scorer plugin for the epp.  | Package nohitlru provides a no-hit LRU scorer plugin for the epp.  |  
|  [epp/framework/plugins/scheduling/scorer/preciseprefixcache](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/preciseprefixcache) Package preciseprefixcache provides a precise prefix cache scorer plugin for the epp.  | Package preciseprefixcache provides a precise prefix cache scorer plugin for the epp.  |  
|  [epp/framework/plugins/scheduling/scorer/prefix](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/prefix)  |  
|  [epp/framework/plugins/scheduling/scorer/queuedepth](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/queuedepth)  |  
|  [epp/framework/plugins/scheduling/scorer/runningrequests](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/runningrequests)  |  
|  [epp/framework/plugins/scheduling/scorer/sessionaffinity](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/sessionaffinity) Package sessionaffinity provides a session affinity scorer plugin for the epp.  | Package sessionaffinity provides a session affinity scorer plugin for the epp.  |  
|  [epp/framework/plugins/scheduling/scorer/tokenload](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/tokenload)  |  
|  [epp/framework/plugins/scheduling/scorer/topologyaffinity](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/scorer/topologyaffinity) Package topologyaffinity provides a scorer that grades candidate endpoints by topology proximity to the peer endpoint.  | Package topologyaffinity provides a scorer that grades candidate endpoints by topology proximity to the peer endpoint.  |  
|  [epp/framework/plugins/scheduling/test](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/test)  |  
|  [epp/framework/plugins/scheduling/test/filter](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/test/filter)  |  
|  [epp/framework/plugins/scheduling/util/sessionaffinity](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/util/sessionaffinity) Package sessionaffinity provides the session header helpers shared by the session-affinity scorer and filter plugins.  | Package sessionaffinity provides the session header helpers shared by the session-affinity scorer and filter plugins.  |  
|  [epp/framework/plugins/scheduling/util/topology](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/epp/framework/plugins/scheduling/util/topology) Package topology provides comparison of endpoint Topology attributes, shared by the topology-affinity-filter and topology-affinity-scorer plugins.  | Package topology provides comparison of endpoint Topology attributes, shared by the topology-affinity-filter and topology-affinity-scorer plugins.  |  
|   |  
|   |  
|   |  
|   |  
|  Package requestcontrol defines the Director component responsible for orchestrating request processing after initial parsing.  | Package requestcontrol defines the Director component responsible for orchestrating request processing after initial parsing.  |  
|  Package scheduling implements request scheduling algorithms.  | Package scheduling implements request scheduling algorithms.  |  
|   |  
|   |  
|   |  
|   |  
|   |  
|   |  
|   |  
|   |  
|   |  
|   |  
|   |  
|   |  
|  [kvcache/tokenization/types](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/pkg/kvcache/tokenization/types)  |  
|  Package kvevents contains the implementation of the KV-events processing system.  | Package kvevents contains the implementation of the KV-events processing system.  |  
|  Package engineadapter provides engine-specific implementations of the kvevents.EngineAdapter interface.  | Package engineadapter provides engine-specific implementations of the kvevents.EngineAdapter interface.  |  
|   |  
|  Package proxy contains the disaggregated prefilling routing proxy implementation  | Package proxy contains the disaggregated prefilling routing proxy implementation  |  
|  Package version contains build version information  | Package version contains build version information  |  
|  [coordinator/e2e/internal/e2eutil](https://pkg.go.dev/github.com/llm-d/llm-d-router@v0.10.0/test/coordinator/e2e/internal/e2eutil) Package e2eutil holds helpers shared across coordinator e2e test suites.  | Package e2eutil holds helpers shared across coordinator e2e test suites.  |  
|  Package framework is the root of the shared test framework tree.  | Package framework is the root of the shared test framework tree.  |  
|  Package net provides network helpers for tests.  | Package net provides network helpers for tests.  |  
|  Package integration provides shared utilities, request builders, and assertions for the hermetic integration test suites of the Gateway API Inference Extension.  | Package integration provides shared utilities, request builders, and assertions for the hermetic integration test suites of the Gateway API Inference Extension.  |  
|   |  
|  Package mock offers a mock CompletionHandler for tests  | Package mock offers a mock CompletionHandler for tests  |  
|  Package utils provides various utilities  | Package utils provides various utilities  |  
|  Package utils contains utilities for testing  | Package utils contains utilities for testing  |  
|   |  
Click to show internal directories. 
Click to hide internal directories. 
go.dev uses cookies from Google to deliver and enhance the quality of its services and to analyze traffic. [Learn more.](https://policies.google.com/technologies/cookies)
Okay
