---
title: 'Beyond Gateway: Inference Traffic Control Practice with …'
id: beyond-gateway-inference-traffic-control-practice-with
tags:
- llmd-router-nonk8s-backends
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:45:17.315479Z'
updated: '2026-09-12T03:37:37.597021Z'
source: https://jimmysong.io/blog/gateway-api-inference-extension-inference-traffic-control/
source_domain: jimmysong.io
fetched_at: '2026-09-11T19:45:17.296229Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Secondary (Nov 14, 2025) blog walkthrough of Gateway API Inference Extension,
  useful mainly for its plain-language restatement of the resource model and its own
  caveat that terminology has shifted (InferenceModel+InferencePool -> InferenceObjective+InferencePool
  with optional InferencePoolImport) and the project is ''Alpha'' at v1.1.x. States
  plainly: InferencePool ''is similar to a Service with a selector, responsible for
  selecting a group of model service Pods'' -- i.e. it selects Pods directly by label
  selector, not Services or EndpointSlices, corroborating that a selectorless-Service/manual-EndpointSlice
  trick cannot be swapped in as an InferencePool backend without upstream changes.
  Also describes InferencePoolImport as the mechanism for ''importing InferencePools
  defined in remote clusters into the local cluster'' for multi-cluster/multi-region
  governance. Recommends production users prefer managed implementations (GKE Inference
  Gateway) over the raw OSS extension while it is Alpha. As a secondary/derivative
  source (not primary docs or code), treat its API-shape claims as needing corroboration
  against gateway-api-inference-extension.sigs.k8s.io and the repo itself.'
utility_score: 10.0
---

[Skip to main content ](https://jimmysong.io/blog/gateway-api-inference-extension-inference-traffic-control/#main-content)[Skip to navigation ](https://jimmysong.io/blog/gateway-api-inference-extension-inference-traffic-control/#navigation)[Skip to social share ](https://jimmysong.io/blog/gateway-api-inference-extension-inference-traffic-control/#social-share)[Skip to footer](https://jimmysong.io/blog/gateway-api-inference-extension-inference-traffic-control/#site-footer)
# Beyond Gateway: Inference Traffic Control Practice with Gateway API Inference Extension
Exploring how Gateway API Inference Extension brings model-aware inference traffic control through InferencePool, InferenceObjective, and metrics-driven routing.
Nov 14, 2025 • • 8 Minute • 1756 words • Updated This Year
Table of content
> AI inference
> #### Inference
> The process of using a trained machine learning model to make predictions.
> traffic governance is undergoing a paradigm shift. The Gateway API Inference Extension makes “model awareness” the new 主线 of traffic control.
## Brief Review of Gateway API’s Current State
Kubernetes Gateway API
#### Gateway API
Next-gen Kubernetes gateway spec for L4/L7 ingress control.
has entered the stable v1 series and continued iterating after the 1.0 GA release, enhancing advanced traffic governance capabilities such as WebSocket, timeout and retry, Service Mesh integration, GRPCRoute, request mirroring, CORS, Retry Budget, and more. Major cloud providers and gateway implementations (such as Alibaba Cloud ACK, GKE Gateway, Envoy Gateway, NGINX Gateway Fabric) have all adopted Gateway API as the new generation north-south traffic model.
Building on this foundation, the community has proposed an extension specification specifically for AI inference traffic - the **Gateway API Inference Extension**. This specification is not about “reinventing an API” but rather supplementing the Gateway API core model with **model-aware load balancing
#### Load Balancing
The process of distributing a set of tasks over a set of resources (computing units), with the aim
and traffic control capabilities** for Large Language Model (LLM
#### LLM
Large Language Model.
) and other inference scenarios.
> Early documentation often mentioned the InferenceModel + InferencePool CRD
> #### CRD
> Custom Resource Definition, a mechanism to extend the Kubernetes API.
> combination; the latest specification has evolved to InferenceObjective + InferencePool (with optional InferencePoolImport). This article consistently uses the latest terminology.
## Typical Challenges in GenAI Inference Traffic
Traditional gateway, Ingress
#### Ingress
A Kubernetes API object that manages external access, providing HTTP and HTTPS routing rules.
, and Service Mesh load balancing models are essentially “request-agnostic + endpoint-agnostic”: they distribute traffic across a group of static backends through algorithms like round-robin, least requests, and hashing.
In GPU-powered Large Language Model (LLM) inference scenarios, this model exposes obvious problems:
  * **Invisible GPU
#### GPU
A parallel computing workhorse often used for deep learning training.
utilization and queuing**
A single LLM instance simultaneously maintains KV Cache
#### KV Cache
A data structure used to store and retrieve key-value pairs.
, LoRA Adapter, and Token queues. Resource consumption varies significantly across the same batch of requests. Load balancing based solely on QPS or connection count can easily lead to extreme situations where “idle GPUs have no work while busy GPUs crash.”
  * **Lack of semantic binding between models and requests**
From the business perspective, one typically only sees a POST /v1/chat/completions endpoint, making it difficult to express intentions like “high-priority model / test version / canary weight
#### Weight
Trainable parameters that determine model predictions.
” at the routing layer.
  * **Difficult unified management of multiple models, versions, and LoRAs**
Each model service implements its own routing and A/B testing solutions, making it difficult for the platform to achieve unified governance and observability at the control plane level.


The goal of Gateway API Inference Extension is to introduce AI-specific semantics and metrics
#### Metrics
Numerical measurable data points used for monitoring and alerting.
into load balancing and traffic control decisions while maintaining the existing Gateway model.
## Core Concepts and Resource Model of Gateway API Inference Extension
This section introduces the overall architecture and key resource objects of the Inference Extension.
### Overall Architecture
The Inference Extension uses Envoy External Processing (ext-proc) mechanism to upgrade Gateway API + ext-proc capable gateways (such as Envoy Gateway, kgateway, GKE
#### GKE
Google Kubernetes Engine, Google Cloud's managed Kubernetes service with native TPU scheduling support.
Gateway) into **Inference Gateways**. Requests still follow the standard Gateway + HTTPRoute path, but before being forwarded to the backend, they pass through an “Endpoint Picker” component that selects the most suitable backend instance based on real-time metrics exposed by the model server.
The flowchart below shows the overall architecture:
Figure 1: Inference Extension Architecture Flow
### InferencePool: Platform-Side “Model Service Pool”
InferencePool is the core resource introduced by the Inference Extension, used to describe a group of inference Pods and routing plugin configurations. It is similar to a Service with a selector, responsible for selecting a group of model service Pods and specifying exposed ports, while also allowing attachment of Endpoint Picker plugins (such as Prefix-Cache-Aware, LoRA-aware, etc.).
In the Gateway API model, InferencePool is treated as a type of “Backend” that can be referenced by HTTPRoute.backendRefs.
The code block below shows a simplified example of InferencePool:

```
apiVersion:inference.networking.x-k8s.io/v1kind:InferencePoolmetadata:name:vllm-llama3-chat-pooltargetPortNumber:8000selector:app:vllm-llama3-chatextensionRef:name:prefix-cache-aware-endpoint-picker
```

The meaning of the above configuration is as follows:
  * Select all Pods with app=vllm-llama3-chat, port 8000.
  * Use the plugin named prefix-cache-aware-endpoint-picker to make routing decisions based on metrics such as KV Cache hit rate, queue length, and GPU utilization.


### InferenceObjective: Business-Side “Request Objective”
InferenceObjective is used to express the goal and priority of a single request, decoupled from the model service pool. One request corresponds to one InferenceObjective, and the same InferencePool can serve multiple different InferenceObjectives.
Typical fields include:
  * Business criticality (Critical / High / BestEffort)
  * Required model family / version preference
  * Acceptable latency / cost upper limits, etc.


The Endpoint Picker can combine InferenceObjective with backend metrics to make decisions: prioritize Critical requests when resources are constrained, and shed BestEffort requests when necessary.
### InferencePoolImport: Cross-Cluster / Gateway Reuse
InferencePoolImport supports importing InferencePools defined in remote clusters into the local cluster, facilitating consistent governance of multi-cluster
#### Multi-Cluster
A deployment architecture that involves multiple clusters.
, multi-region inference services.
### Maturity and Implementation Ecosystem of Inference Extension
The current project version is v1.1.x, overall in the **Alpha** stage. Official recommendation is not to use it directly in production yet; it’s more suitable for platform teams to experiment with the technology stack.
Multiple implementations and integrations already exist:
  * Inference Gateway implementations for Envoy Gateway / kgateway
  * GKE Inference Gateway: Enhanced capabilities based on GKE Gateway, including KV Cache-aware routing, LoRA
#### LoRA
Low-Rank Adaptation, a parameter-efficient fine-tuning technique for large language models.
reuse, priority scheduling, etc.
  * NGINX Gateway Fabric, cloud provider ACK, and others are also following up on related extensions


Therefore, when designing practical solutions, it should be treated as a “forward-looking solution / future main path.” For production environments, prioritize managed implementations (such as GKE Inference Gateway) or commercial products from gateway vendors.
## Practice: Using Inference Extension for Inference Traffic Control
The following example uses a self-hosted LLM cluster on Kubernetes to demonstrate how to serve external traffic through an OpenAI-compatible interface and achieve:
  * Scheduling by request priority (real-time conversation vs. batch processing)
  * Multi-version model canary and rollback
  * Optimized routing using GPU metrics and KV Cache hit rates
  * Unified platform-side observability and rate limiting
#### Rate Limiting
A strategy for limiting network traffic.
entry point


### Deploying Gateway API and Inference Extension
The deployment process is as follows:
  1. Install a Gateway API-compatible gateway implementation (such as Envoy Gateway, kgateway, GKE Gateway) in the cluster.
  2. Install the Gateway API Inference Extension CRD and control plane components.
  3. Enable the metrics endpoints and plugin protocols required by Inference Extension on the model server side (such as vLLM, Triton, TGI).


### Defining InferencePool: Abstracting LLM Pods as an “Inference Pool”
The code block below shows a typical InferencePool configuration:

```
apiVersion:inference.networking.x-k8s.io/v1kind:InferencePoolmetadata:name:chat-llama3-pooltargetPortNumber:8000selector:app:chat-llama3extensionRef:name:prefix-cache-aware# Optional: Plugin configuration ConfigMap / CR
```

Key points:
  * selector selects LLM Pods, targetPortNumber specifies the inference service port.
  * extensionRef binds the Endpoint Picker plugin, implementing KV Cache prefix-aware routing, selecting replicas with lighter load based on metrics like queue_length/gpu_utilization, and triggering load shedding when necessary.


### Defining InferenceObjective: Connecting “Business Intent” to Routing Decisions
The code block below shows a sample InferenceObjective configuration (fields can be adjusted according to the actual version and implementation):

```
apiVersion:inference.networking.x-k8s.io/v1kind:InferenceObjectivemetadata:name:chat-criticalcriticality:Critical        # Real-time conversationpreferredModel:llama3-70bfallbackModel:llama3-8bapiVersion:inference.networking.x-k8s.io/v1kind:InferenceObjectivemetadata:name:chat-batchcriticality:BestEffort      # Batch analysis, log summarizationpreferredModel:llama3-8b
```

The Endpoint Picker can combine InferenceObjective with InferencePool metrics to make the following decisions:
  * When GPU is constrained and queues are too long, prioritize chat-critical requests and discard chat-batch if necessary.
  * For Critical requests, prioritize the large model; if the target pool is unavailable, fall back to the small model pool.


### Importing Business Traffic to InferencePool via HTTPRoute
The code block below shows the HTTPRoute configuration on the Gateway API side:

```
apiVersion:gateway.networking.k8s.io/v1kind:HTTPRoutemetadata:name:llm-routeparentRefs:- name:public-gateway- matches:type:PathPrefixvalue:/v1/chat/completionsbackendRefs:- group:inference.networking.x-k8s.iokind:InferencePoolname:chat-llama3-pool
```

Some implementations (such as GKE Inference Gateway) can route based on the model field in the request body, mapping OpenAI-style model names to different InferencePool / InferenceObjective combinations.
### Fine-Grained Inference Traffic Control Using Metrics
Inference Extension provides platform teams with a unified metrics system, including kv_cache_hits, gpu_utilization, request_queue_length, per-request inference duration, token
#### Token
The basic unit of text processed by large language models, which can be words, subwords, or
count, and more.
Based on these metrics, multi-level traffic control strategies can be built:
  * **Priority + Capacity**
Set priority and capacity upper limits for different InferenceObjectives, automatically guaranteeing critical business when resources are constrained.
  * **Rate Limiting by Cost / Token**
Aggregate token / latency metrics exposed by Inference Extension to Prometheus
#### Prometheus
An open-source monitoring and alerting system that uses a pull model to collect time-series data.
, then add cost-based rate limiting logic at the gateway / API Gateway level (such as total tokens per minute per user / application). The specification itself doesn’t mandate “Token-level rate limiting,” but provides observability and hooks for easy policy extension.
  * **Prefix Cache Aware Routing**
For requests with shared context
#### Context
The information that surrounds a piece of text and helps to determine its meaning.
(such as RAG, template generation), enable the Prefix Cache Aware plugin to route requests with the same prefix to the same replica, maximizing KV Cache hit rates and significantly reducing TTFT.
  * **Auto-Scaling Integration**
Use metrics output by Inference Extension as HPA
#### HPA
Automatically scales pod replicas based on metrics.
input to achieve “model-aware” auto-scaling, rather than relying solely on CPU / memory.


## Relationship and Trade-offs with Traditional Gateway / Service Mesh
  * **Control Plane** : Continue using Gateway API as the unified north-south / east-west traffic modeling specification. Service Mesh
#### Service Mesh
A dedicated infrastructure layer for handling service-to-service communication.
can still perform fine-grained circuit breaking, retry, mTLS, etc. within the cluster.
  * **Data Plane** : Inference Extension pushes “model-aware routing” down to the ext-proc path implemented by the Gateway, avoiding redundant business-side wheel reinvention.
  * **Adoption Strategy** : In the current Alpha stage, prioritize productionized implementations (such as GKE Inference Gateway, commercial gateway vendor Inference Gateways), start with “bypass pilots” outside the critical path, and gradually migrate existing AI gateway routing rules to the Inference Extension model.


## Summary
Combining official documentation and community implementations, we can see:
  * Gateway API has become the Kubernetes standard north-south traffic model and continues to enhance in the 1.x series.
  * Gateway API Inference Extension introduces GPU metrics, KV Cache, LoRA, and other inference semantics into load balancing decisions through InferencePool, InferenceObjective, and Endpoint Picker.
  * The project is still in Alpha stage, but has achieved experimental or productionized adoption in implementations such as GKE, kgateway, and NGINX Gateway Fabric. It is one of the important future directions for inference traffic control.


Previous descriptions of Inference Extension as “built-in Token rate limiting CRD, AIInferencePolicy, and other objects” are no longer accurate and should all be replaced with the design based on InferencePool / InferenceObjective + metrics-driven approach.
## References
  * [Gateway API v1.0: GA Release - kubernetes.io](https://kubernetes.io/blog/2023/10/31/gateway-api-ga/)
  * [Gateway API v1.2: WebSockets, Timeouts, Retries, and More - kubernetes.io](https://kubernetes.io/blog/2024/11/21/gateway-api-v1-2/)
  * [Kubernetes Gateway API Inference Extension – Overview - gateway-api-inference-extension.sigs.k8s.io](https://gateway-api-inference-extension.sigs.k8s.io/)
  * [API Overview – InferencePool / InferenceObjective / InferencePoolImport - gateway-api-inference-extension.sigs.k8s.io](https://gateway-api-inference-extension.sigs.k8s.io/concepts/api-overview/)
  * [Metrics and Observability - gateway-api-inference-extension.sigs.k8s.io](https://gateway-api-inference-extension.sigs.k8s.io/guides/metrics-and-observability/)
  * [Deep Dive into the Gateway API Inference Extension – cncf.io](https://www.cncf.io/blog/2025/04/21/deep-dive-into-the-gateway-api-inference-extension/)
  * [Introducing Gateway API Inference Extension – kubernetes.io](https://kubernetes.io/blog/2025/xx/xx/gateway-api-inference-extension/)
  * [GKE Inference Gateway - cloud.google.com](https://cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway)
  * [NGINX Gateway Fabric – Kubernetes Gateway API and AI Inference - docs.nginx.com](https://docs.nginx.com/nginx-gateway-fabric/)
  * [Alibaba Cloud ACK – Gateway API Components and Versions - alibabacloud.com](https://www.alibabacloud.com/help/en/ack/product-overview/gateway-api)


### Jimmy Song
Focusing on research and open source practices in AI-Native Infrastructure and cloud native application architecture.
Updated on Nov 14, 2025 [Gateway API](https://jimmysong.io/tags/gateway-api) [Inference Extension](https://jimmysong.io/tags/inference-extension) [LLM](https://jimmysong.io/tags/llm) [Inference Routing](https://jimmysong.io/tags/inference-routing) [Kubernetes Gateway](https://jimmysong.io/tags/kubernetes-gateway)
×
#### Share via WeChat
Scan QR code with WeChat to share
Post Navigation
[ **Previous Post** Kimi K2 Thinking: The True Awakening of China's Thinking Model](https://jimmysong.io/blog/kimi-k2-thinking-cn-awakening/)
[**Next Post** TRAE SOLO vs VS Code: AI Engineering Entity Comparison](https://jimmysong.io/blog/trae-vs-vscode-insiders-agent-hq-and-ai-engineering-entity/)
Related Articles
### [Lessons from Ingress NGINX Retirement 2025-11-13  Cloud Native  4min ](https://jimmysong.io/blog/ingress-nginx-retirement-insights/)### [Step-by-Step Guide: Moving from ingress-nginx to Envoy Gateway 2025-05-18  Cloud Native  3min ](https://jimmysong.io/blog/migrating-from-ingress-nginx-to-envoy-gateway/)### [Leveraging EnvoyPatchPolicy to Extend the Capabilities of Envoy Gateway 2025-04-14  Cloud Native  3min ](https://jimmysong.io/blog/leveraging-envoypatchpolicy-to-extend-the-capabilities-of-envoy-gateway/)
Comments
Loading comments... Leave a comment
