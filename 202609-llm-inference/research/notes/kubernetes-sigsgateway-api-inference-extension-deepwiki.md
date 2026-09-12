---
title: kubernetes-sigs/gateway-api-inference-extension | DeepWiki
id: kubernetes-sigsgateway-api-inference-extension-deepwiki
tags:
- llmd-router-nonk8s-backends
- source-hub
- low-authority
- leads-only
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:44:23.637038Z'
updated: '2026-09-12T03:37:37.567886Z'
source: https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension
source_domain: deepwiki.com
fetched_at: '2026-09-11T19:44:23.616797Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'DeepWiki AI-generated wiki (auto-indexed, last indexed 24 Aug 2026 at commit
  84436a) over the gateway-api-inference-extension repo -- LOW AUTHORITY, AI-summarized
  secondary source; useful only as a navigation index into primary repo paths, not
  to be cited for claims without cross-checking the underlying file. Value here is
  purely as a lead-generator: identifies primary repo paths worth fetching directly
  -- site-src/concepts/api-overview.md, site-src/guides/implementers.md (EPP implementer
  guide, referenced for InferencePool field semantics), README.md (project root, references
  Model Server Protocol and a directory/reference called ''llm-d-router'' as an example
  of an ''advanced'' production EPP implementation used in place of the reference
  lwepp), and dedicated subpages on InferencePoolImport/multi-cluster API, LWEPP (Lightweight
  Endpoint Picker) architecture and its ext_proc StreamingServer integration, InferencePool
  rollout strategies, conformance testing, and a page titled ''Latency Prediction
  and SLO-Aware Routing''. Describes the reference EPP implementation (lwepp) internals:
  an ext_proc StreamingServer wrapping a Datastore populated by an InferencePoolReconciler
  and PodReconciler, calling PickEndpoint. Notes ''Over 25 [Gateway API] implementations
  exist'' with several (Envoy Gateway, kgateway, GKE Gateway, Istio) working toward
  inference-extension support.'
utility_score: 9.0
---

Index your code with Devin
[DeepWiki](https://deepwiki.com/)
[DeepWiki](https://deepwiki.com/)
[kubernetes-sigs/gateway-api-inference-extension ](https://github.com/kubernetes-sigs/gateway-api-inference-extension "Open repository")
Index your code with
Devin
Edit WikiShare
Last indexed: 24 August 2026 ([84436a](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commits/84436a96))
  * [Gateway API Inference Extension Overview](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension/1-gateway-api-inference-extension-overview)
  * [InferencePoolImport and Multi-Cluster API](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension/3.6-inferencepoolimport-and-multi-cluster-api)
  * [LWEPP (Lightweight Endpoint Picker)](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension/4-lwepp-\(lightweight-endpoint-picker\))
  * [LWEPP Architecture and Lifecycle](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension/4.1-lwepp-architecture-and-lifecycle)
  * [StreamingServer and ext_proc Integration](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension/4.3-streamingserver-and-ext_proc-integration)
  * [Conformance Reports and Implementations](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension/5.3-conformance-reports-and-implementations)
  * [High Availability and Leader Election](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension/6.5-high-availability-and-leader-election)
  * [InferencePool Rollout Strategies](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension/6.6-inferencepool-rollout-strategies)
  * [Conformance Testing for Developers](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension/7.3-conformance-testing-for-developers)
  * [API Development and Conventions](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension/7.5-api-development-and-conventions)
  * [Latency Prediction and SLO-Aware Routing](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension/8.2-latency-prediction-and-slo-aware-routing)


# Gateway API Inference Extension Overview
Relevant source files
  * [docs/inference-gateway-architecture.svg](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/docs/inference-gateway-architecture.svg)
  * [site-src/concepts/api-overview.md](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/concepts/api-overview.md?plain=1)
  * [site-src/concepts/roles-and-personas.md](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/concepts/roles-and-personas.md?plain=1)


## What is the Gateway API Inference Extension?
The Gateway API Inference Extension is an official Kubernetes project that optimizes self-hosting Generative Models on Kubernetes. By leveraging Envoy's [External Processing Protocol](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/ext_proc_filter), it extends any gateway supporting both ext-proc and [Gateway API](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/Gateway%20API) into an **Inference Gateway**.
**Project Repository** : [httpsNaN-NaN](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/https#LNaN-LNaN)
## Purpose and Scope
The project aims to improve and standardize routing to inference workloads across the ecosystem. It provides optimized load-balancing for self-hosted Generative AI Models, specifically focusing on Large Language Models (LLMs).
**Key Capabilities** :
  * **Model-aware routing** : Route to models based on model names (e.g., OpenAI-compatible chat completion endpoints) rather than just URL paths [site-src/index.md41](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/index.md?plain=1#L41-L41)
  * **Serving priority** : Specify priorities for different tasks, such as prioritizing latency-sensitive chat tasks over summarization [site-src/index.md43](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/index.md?plain=1#L43-L43)
  * **Model rollouts** : Incrementally roll out new model versions or LoRA adapters via traffic splitting [site-src/index.md45](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/index.md?plain=1#L45-L45)
  * **Customizable Load Balancing** : Utilize a reference implementation for model endpoint picking that leverages real-time metrics (KV cache, queue depth) from model servers [site-src/index.md49](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/index.md?plain=1#L49-L49)


For detailed architectural information, see [System Architecture](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension/1.1-system-architecture). For details on personas, see [Personas and Use Cases](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension/1.2-personas-and-use-cases).
## System Overview
The Gateway API Inference Extension integrates into the standard Kubernetes Gateway API resource model as a new type of backend.  
| Layer  | Components  | Responsibility  |  
| --- | --- | --- |  
| **Control Plane**  |  `InferencePool`, `InferencePoolImport` CRDs  | Define pools of model server pods and multi-cluster references [site-src/concepts/api-overview.md22-28](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/concepts/api-overview.md?plain=1#L22-L28)  |  
| **Data Plane Integration**  | Gateway implementation with `ext_proc` filter  | Intercept requests and delegate selection to an Endpoint Picker [site-src/index.md33](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/index.md?plain=1#L33-L33)  |  
| **Scheduling & Selection**  | Endpoint Picker (EPP)  | Implements the Inference Router logic to select the optimal pod [site-src/index.md20-22](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/index.md?plain=1#L20-L22)  |  
### End-to-End Architecture

```



Workload Space
Inference Extension Space
Data Plane (Envoy/GKE/Istio)
Gateway API Layer
Client Space
Request (model: 'llama3')
references
backendRef
External Processing
EPP Protocol
scrapes metrics
picks
forwards to
Client Application
Gateway Resource
HTTPRoute Resource
Gateway Proxy
ext_proc Filter
Endpoint Picker (lwepp)
InferencePool CRD
Model Server Pods(vLLM / SGLang / Triton)




```

**Diagram: Request flow through the Inference Gateway**
Sources: [site-src/index.md78-100](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/index.md?plain=1#L78-L100) [site-src/guides/implementers.md8-10](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/guides/implementers.md?plain=1#L8-L10)
## Core Resource: InferencePool
The `InferencePool` represents a set of inference-focused Pods and an extension (EPP) used to route to them. It is used as a `backendRef` in an `HTTPRoute`.

```

```


apiVersion: inference.networking.k8s.io/v1




kind: InferencePool




metadata:




  name: vllm-llama3




spec:




  selector:




    matchLabels:




      app: vllm-llama3




  targetPorts:




number: 8000




  endpointPickerRef:




    name: llama-epp




    port:




      number: 9002


```

```

**Key Fields:**
  * `spec.selector`: Selects the model server pods [site-src/guides/implementers.md50-52](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/guides/implementers.md?plain=1#L50-L52)
  * `spec.endpointPickerRef`: References the EPP service responsible for routing decisions [site-src/guides/implementers.md55-58](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/guides/implementers.md?plain=1#L55-L58)


Sources: [site-src/guides/implementers.md43-59](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/guides/implementers.md?plain=1#L43-L59) [site-src/concepts/api-overview.md22-25](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/concepts/api-overview.md?plain=1#L22-L25)
## Endpoint Picker (EPP)
The **Endpoint Picker (EPP)** is a data-plane component that communicates via the Envoy external processing protocol. It acts as the **Inference Router** , intercepting requests to route them to the optimal model server replica based on `Metrics and Capabilities` (e.g., KV cache status, LoRA availability) [README.md34-44](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/README.md?plain=1#L34-L44)
### Lightweight EPP (lwepp)
This repository provides a **Lightweight Endpoint Picker (lwepp)**. It serves as a reference implementation for conformance testing purposes [README.md44-45](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/README.md?plain=1#L44-L45) For production, users often use more advanced implementations like the [llm-d-router](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/llm-d-router) [site-src/index.md73-76](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/index.md?plain=1#L73-L76)

```



Controllers
lwepp Process
calls PickEndpoint
updates
watches
runner.ExtProcServerRunner
handlers.StreamingServer
datastore.Datastore
controller.InferencePoolReconciler
controller.PodReconciler
Pod Endpoints




```

**Diagram: Internal components of the lwepp reference implementation**
Sources: [site-src/guides/implementers.md94-104](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/guides/implementers.md?plain=1#L94-L104) [README.md40-45](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/README.md?plain=1#L40-L45)
## Conformance and Ecosystem
A key goal of this project is to provide a set of patterns and specifications to enable a compatible ecosystem.
  * **Gateway API Implementations** : Over 25 implementations exist. Projects like Envoy Gateway, kgateway, GKE Gateway, and Istio are working to support this extension [site-src/index.md64-70](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/index.md?plain=1#L64-L70)
  * **Model Server Protocol** : Standardizes how model servers (like vLLM) emit metrics (KV cache, queue depth) so any EPP can make informed decisions [README.md37-39](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/README.md?plain=1#L37-L39)
  * **Conformance Suite** : This repository remains the primary location for maintaining conformance tests to ensure different Gateway and EPP implementations adhere to the expected behavior [README.md23](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/README.md?plain=1#L23-L23)


Sources: [site-src/index.md59-70](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/site-src/index.md?plain=1#L59-L70) [README.md23-24](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/84436a96/README.md?plain=1#L23-L24)
## Child Pages
For deeper technical details, please refer to the following sub-pages:
  * : Detailed look at the `ext_proc` integration, the `lwepp` internal logic, and the `InferencePool` status lifecycle.
  * : Exploration of the `Inference Platform Admin` vs `Inference Workload Owner` and specific scenarios like prefix cache optimization.


Dismiss
Refresh this wiki
Enter email to refresh
### On this page
  * [Gateway API Inference Extension Overview](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension#gateway-api-inference-extension-overview)
  * [What is the Gateway API Inference Extension?](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension#what-is-the-gateway-api-inference-extension)
  * [End-to-End Architecture](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension#end-to-end-architecture)
  * [Core Resource: InferencePool](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension#core-resource-inferencepool)
  * [Lightweight EPP (lwepp)](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension#lightweight-epp-lwepp)
  * [Conformance and Ecosystem](https://deepwiki.com/kubernetes-sigs/gateway-api-inference-extension#conformance-and-ecosystem)


