---
title: Introduction - Kubernetes Gateway API Inference Extension
id: introduction-kubernetes-gateway-api-inference-extension
tags:
- gateway-routing
- epp
- gie-overview
created: '2026-10-09T22:53:28.372309Z'
updated: '2026-10-10T14:52:16.988622Z'
source: https://gateway-api-inference-extension.sigs.k8s.io/
source_domain: gateway-api-inference-extension.sigs.k8s.io
fetched_at: '2026-10-09T22:53:28.346107Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Official GIE project homepage defining the terminology used throughout the
  ecosystem. Defines Inference Gateway (a proxy/LB coupled with an Endpoint Picker
  extension), Inference Router (the decision-making component), ''Metrics and Capabilities''
  (model-server-reported data like Prefix Cache status and LoRA Adapter availability
  used to optimize routing), and Endpoint Picker/EPP (reference implementation of
  an Inference Router, provided here only for conformance testing -- production users
  are told to implement their own EPP or use an existing one like llm-d-router). Explains
  the mechanism is built on Envoy''s External Processing Protocol (ext-proc) layered
  onto any gateway supporting Gateway API + ext-proc; names Envoy Gateway, kgateway,
  and GKE Gateway as gateways this extends into Inference Gateways, and names LiteLLM,
  Gloo AI Gateway, and Apigee as higher-level ''AI Gateways'' this can sit underneath.
  States project is driven by WG-Serving and SIG-Network (official Kubernetes governance
  groups), not a single vendor. Lists 5-step request flow: Gateway selects InferencePool
  via HTTPRoute match -> forwards request info to EPP -> EPP fetches live metrics
  from pool endpoints -> EPP instructs Gateway of chosen endpoint -> Gateway routes.
  Key features named: model-aware routing (including to LoRA fine-tunes), serving
  priority (criticality-based), model rollouts (traffic splitting), extensibility
  for bespoke routing, and customizable inference-optimized load balancing that reduces
  latency and improves GPU utilization versus traditional LB.'
utility_score: 16.0
---

# Introduction[¶](https://gateway-api-inference-extension.sigs.k8s.io/#introduction "Permanent link")
Gateway API Inference Extension is an official Kubernetes project that optimizes self-hosting Generative Models on Kubernetes.
The overall resource model focuses on the Inference Platform Owner [persona](https://gateway-api-inference-extension.sigs.k8s.io/concepts/roles-and-personas) and corresponding resources:
## Concepts and Definitions[¶](https://gateway-api-inference-extension.sigs.k8s.io/#concepts-and-definitions "Permanent link")
The following specific terms to this project:
  * **Inference Gateway** : A proxy/load-balancer that has been coupled with the EndPointer Picker extension. It provides optimized routing and load balancing for serving Kubernetes self-hosted generative Artificial Intelligence (AI) workloads. It simplifies the deployment, management, and observability of AI inference workloads.
  * **Inference Router** : An extendable component that makes decisions about which endpoint is optimal (best cost / best performance) for an inference request based on `Metrics and Capabilities` from [Model Serving](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/003-model-server-protocol/README.md).
  * **Metrics and Capabilities** : Data provided by model serving platforms about performance, availability and capabilities to optimize routing. Includes things like [Prefix Cache](https://docs.vllm.ai/en/stable/design/v1/prefix_caching.html) status or [LoRA Adapters](https://docs.vllm.ai/en/stable/features/lora.html) availability.
  * **Endpoint Picker(EPP)** : An implementation of an `Inference Router`. In this repo, we provide a lightweight EPP for conformance testing purposes.


## Key Features[¶](https://gateway-api-inference-extension.sigs.k8s.io/#key-features "Permanent link")
Gateway API Inference Extension optimizes self-hosting Generative AI Models on Kubernetes. It provides optimized load-balancing for self-hosted Generative AI Models on Kubernetes. The project’s goal is to improve and standardize routing to inference workloads across the ecosystem.
This is achieved by leveraging Envoy's [External Processing Protocol](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/ext_proc_filter) to extend any gateway that supports both ext-proc and [Gateway API](https://github.com/kubernetes-sigs/gateway-api) into an [inference gateway](https://gateway-api-inference-extension.sigs.k8s.io/#concepts-and-definitions). This extension extends popular gateways like Envoy Gateway, kgateway, and GKE Gateway - to become [Inference Gateway](https://gateway-api-inference-extension.sigs.k8s.io/#concepts-and-definitions) - supporting inference platform teams self-hosting Generative Models (with a current focus on large language models) on Kubernetes. This integration makes it easy to expose and control access to your local [OpenAI-compatible chat completion endpoints](https://platform.openai.com/docs/api-reference/chat) to other workloads on or off cluster, or to integrate your self-hosted models alongside model-as-a-service providers in a higher level **AI Gateways** like [LiteLLM](https://www.litellm.ai/), [Gloo AI Gateway](https://www.solo.io/products/gloo-ai-gateway), or [Apigee](https://cloud.google.com/apigee).
  * **Model-aware routing** : Instead of simply routing based on the path of the request, an **[inference gateway]** allows you to route to models based on the model names. This is enabled by support for GenAI Inference API specifications (such as OpenAI API) in the gateway implementations such as in Envoy Proxy. This model-aware routing also extends to Low-Rank Adaptation (LoRA) fine-tuned models.
  * **Serving priority** : an **[inference gateway]** allows you to specify the serving priority of your models. For example, you can specify that your models for online inference of chat tasks (which is more latency sensitive) have a higher priority than a model for latency tolerant tasks such as a summarization. 
  * **Model rollouts** : an **[inference gateway]** allows you to incrementally roll out new model versions by traffic splitting definitions based on the model names. 
  * **Extensibility for Inference Services** : an **[inference gateway]** defines extensibility pattern for additional Inference services to create bespoke routing capabilities should out of the box solutions not fit your needs.
  * **Customizable Load Balancing for Inference** : an **[inference gateway]** defines a pattern for customizable load balancing and request routing that is optimized for Inference. An **[inference gateway]** provides a reference implementation of model endpoint picking leveraging metrics emitted from the model servers. This endpoint picking mechanism can be used in lieu of traditional load balancing mechanisms. Model Server-aware load balancing ("smart" load balancing as its sometimes referred to in this repo) has been proven to reduce the serving latency and improve utilization of accelerators in your clusters.


By achieving these, the project aims to reduce latency and improve accelerator (GPU) utilization for AI workloads.
## API Resources[¶](https://gateway-api-inference-extension.sigs.k8s.io/#api-resources "Permanent link")
Head to our [API overview](https://gateway-api-inference-extension.sigs.k8s.io/concepts/api-overview/#api-overview) to start exploring our APIs!
## Composable Layers[¶](https://gateway-api-inference-extension.sigs.k8s.io/#composable-layers "Permanent link")
This project aims to define specifications to enable a compatible ecosystem for extending the Gateway API with custom endpoint selection algorithms. This project defines a set of patterns across three distinct layers of components that are relevant to this project:
### Gateway API Implementations[¶](https://gateway-api-inference-extension.sigs.k8s.io/#gateway-api-implementations "Permanent link")
Gateway API has [more than 25 implementations](https://gateway-api.sigs.k8s.io/implementations/). As this pattern stabilizes, we expect a wide set of these implementations to support this project to become an **[inference gateway]**
### Endpoint Picker[¶](https://gateway-api-inference-extension.sigs.k8s.io/#endpoint-picker "Permanent link")
As part of this project, we provide a lightweight Endpoint Picker (EPP) reference implementation designed for conformance testing. For production deployments, users and implementers should implement their own EPP or use an existing one (such as [llm-d-router](https://github.com/llm-d/llm-d-router)).
## Request Flow[¶](https://gateway-api-inference-extension.sigs.k8s.io/#request-flow "Permanent link")
To illustrate how this all comes together, it may be helpful to walk through a sample request.
  1. The first step involves the Gateway selecting the correct InferencePool (set of endpoints running a model server framework) or Service to route to. This logic is based on the existing Gateway and HTTPRoute APIs, and will be familiar to any Gateway API users or implementers.
  2. If the request should be routed to an InferencePool, the Gateway will forward the request information to the endpoint selection extension for that pool.
  3. The Endpoint Picker (EPP) will fetch metrics from whichever portion of the InferencePool endpoints can best achieve the configured objectives. Note that this kind of metrics probing may happen asynchronously, depending on the EPP.
  4. The EPP will instruct the Gateway which endpoint the request should be routed to.
  5. The Gateway will route the request to the desired endpoint.


## Who is working on Gateway API Inference Extension?[¶](https://gateway-api-inference-extension.sigs.k8s.io/#who-is-working-on-gateway-api-inference-extension "Permanent link")
This project is being driven by [WG-Serving](https://github.com/kubernetes/community/tree/master/wg-serving) [SIG-Network](https://github.com/kubernetes/community/tree/master/sig-network) to improve and standardize routing to inference workloads in Kubernetes. Check out the [implementations reference](https://gateway-api-inference-extension.sigs.k8s.io/implementations/gateways/) to see the latest projects & products that support this project. If you are interested in contributing to or building an implementation using Gateway API then don’t hesitate to [get involved!](https://gateway-api-inference-extension.sigs.k8s.io/contributing)
Back to top 
