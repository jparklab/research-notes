---
title: About GKE Inference Gateway powered by llm-d  |  GKE networking  |  Google
  Cloud Documentation
id: about-gke-inference-gateway-powered-by-llm-d-gke-networking-google-cloud-documen
tags:
- llmd-router-nonk8s-backends
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:44:30.149746Z'
updated: '2026-09-15T04:55:23.726321Z'
source: https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway
source_domain: docs.cloud.google.com
fetched_at: '2026-09-11T19:44:30.122896Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'Google''s ground-truth doc on GKE Inference Gateway ''powered by llm-d''
  confirms the llm-d Router architecture: an L7 Proxy (Envoy/GKE Gateway) that calls
  a separate llm-d Endpoint Picker (EPP) service via the Envoy ext-proc protocol per
  request, then forwards directly to the chosen Pod -- Gateway (data plane) and EPP
  (routing intelligence) are decoupled processes. InferencePool is explicitly defined
  as ''a group of Pods that share the same compute configuration''; it supports up
  to 8 targetPorts, and each port in each zone generates a dedicated Network Endpoint
  Group (NEG), capped at 50 NEGs per GCP Backend Service -- e.g. an 8-port pool across
  3 zones (24 NEGs) can only be aggregated from 2 clusters before hitting the limit,
  a concrete multi-cluster scaling ceiling. Also documents EPP scoring signals (prefix-cache
  match, KV-cache/queue load, LoRA-adapter affinity), the numeric Priority/Criticality
  field (values <0 dropped first, returning 429), and that streaming responses are
  never retried on error.'
utility_score: 10.0
---

[ Skip to main content ](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#main-content)
[ Console ](https://console.cloud.google.com/)
  * [Español – América Latina](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway?hl=es-419)


  * [ Google Kubernetes Engine (GKE) ](https://docs.cloud.google.com/kubernetes-engine/docs)


  * On this page
  * [Overview](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#overview)
    * [Understand key concepts](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#understand-key-concepts)
  * [llm-d Router](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#llm-d-router)
  * [How GKE Inference Gateway works](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#how-it-works)
    * [How the request flow works](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#request-flow)
    * [How traffic distribution works](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#traffic-distribution)
  * [Explore application examples](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#application-examples)
    * [Example 1: Serve multiple generative AI models on a GKE cluster](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#example-1)
    * [Example 2: Serve LoRA adapters on a shared accelerator](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#example-2)


  * [ Google Kubernetes Engine (GKE) ](https://docs.cloud.google.com/kubernetes-engine/docs)


Was this helpful?
Send feedback 
#  About GKE Inference Gateway powered by llm-d Stay organized with collections  Save and categorize content based on your preferences.
  * On this page
  * [Overview](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#overview)
    * [Understand key concepts](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#understand-key-concepts)
  * [llm-d Router](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#llm-d-router)
  * [How GKE Inference Gateway works](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#how-it-works)
    * [How the request flow works](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#request-flow)
    * [How traffic distribution works](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#traffic-distribution)
  * [Explore application examples](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#application-examples)
    * [Example 1: Serve multiple generative AI models on a GKE cluster](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#example-1)
    * [Example 2: Serve LoRA adapters on a shared accelerator](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#example-2)


This page explains the key concepts and features of Google Kubernetes Engine (GKE) Inference Gateway, an extension to the GKE Gateway for optimized serving of generative AI applications.
This page assumes that you know about the following:
  * [AI/ML orchestration on GKE](https://docs.cloud.google.com/kubernetes-engine/docs/integrations/ai-infra)
  * [Generative AI terminology](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/glossary-genai)
  * [GKE networking concepts](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/network-overview), including [Services](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/service), and the [GKE Gateway API](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/gateway-api)
  * [Load balancing in Google Cloud](https://docs.cloud.google.com/load-balancing/docs/load-balancing-overview), especially how load balancers interact with GKE


This page is intended for the following personas:
  * Machine learning (ML) engineers, Platform admins and operators, and Data and AI specialists who are interested in using Kubernetes container orchestration capabilities for serving AI/ML workloads.
  * Cloud architects and Networking specialists who interact with Kubernetes networking.


## Overview
GKE Inference Gateway is an extension to the [GKE Gateway](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/gateway-api#gateway) that provides optimized routing and load balancing for serving generative Artificial Intelligence (AI) workloads. It simplifies the deployment, management, and observability of AI inference workloads. GKE Inference Gateway is available at no additional cost. You pay only for the underlying Google Cloud resources used.
To choose the optimal load balancing strategy for your AI/ML workloads, see [Choose a load balancing strategy for AI inference on GKE](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/machine-learning/choose-lb-strategy).
### Features and benefits
GKE Inference Gateway provides the following key capabilities to efficiently serve generative AI models for generative AI applications on GKE:
  * **Supported metrics** : 
    * **`KV cache hits`**: the number of successful lookups in the key-value (KV) cache.
    * **GPU or TPU utilization** : the percentage of time that the GPU or TPU is actively processing.
    * **Request queue length** : the number of requests waiting to be processed.
  * **Optimized load balancing for inference** : distributes requests to optimize AI model serving performance. It uses metrics from model servers, such as `KV cache hits` and the `queue length of pending requests` to consume accelerators (such as GPUs and TPUs) more efficiently for generative AI workloads. This enables **Prefix-Cache Aware Routing** , a key feature that sends requests with shared context, identified by analyzing the request body, to the same model replica by maximizing cache hits. This approach dramatically reduces redundant computations and improves Time-to-First-Token, making it highly effective for conversational AI, Retrieval-Augmented Generation (RAG), and other template-based generative AI workloads.
  * **Dynamic LoRA fine-tuned model serving** : supports serving dynamic LoRA fine-tuned models on a common accelerator. This reduces the number of GPUs and TPUs required to serve models by multiplexing multiple LoRA fine-tuned models on a common base model and accelerator.
  * **Optimized autoscaling for inference** : the GKE Horizontal Pod Autoscaler (HPA) uses model server metrics to [autoscale](https://docs.cloud.google.com/kubernetes-engine/docs/how-to/customize-gke-inference-gateway-configurations#autoscaling), which helps ensure efficient compute resource use and optimized inference performance.
  * **Model-aware routing** : routes inference requests based on the model names defined in the [`OpenAI API` specifications](https://platform.openai.com/docs/api-reference/chat/create) within your GKE cluster. You can define Gateway routing policies, such as traffic splitting and request mirroring, to manage different model versions and simplify model rollouts. For example, you can route requests for a specific model name to different InferencePool objects, each serving a different version of the model. For more information on how to configure this, see [Configure Body-Based Routing](https://docs.cloud.google.com/kubernetes-engine/docs/how-to/configure-body-based-routing).
  * **Integrated AI safety and content filtering** : GKE Inference Gateway integrates with Google Cloud [Model Armor](https://docs.cloud.google.com/security-command-center/docs/model-armor-overview) to apply AI safety checks and content filtering to prompts and responses at the gateway. You can also use [NVIDIA NeMo Guardrails](https://docs.nvidia.com/nemo/microservices/latest/set-up/deploy-as-microservices/guardrails/gcp-installation.html). Model Armor provides logs of requests, responses, and processing for retrospective analysis and optimization. GKE Inference Gateway's open interfaces allow third-party providers and developers to integrate custom services into the inference request process.
  * **Model-specific serving`Priority`** : lets you specify the serving `Priority` of AI models. Prioritize latency-sensitive requests over latency-tolerant batch inference jobs. For example, you can prioritize requests from latency-sensitive applications and drop less time-sensitive tasks when resources are constrained.
  * **Predicted latency-based routing** : routes inference requests using an XGBoost model trained continuously on live traffic, optimizing for per-request Time-to-First-Token (TTFT) and Time-per-Output-Token (TPOT) objectives. More accurate than static heuristics under high-variance workloads. See Use predicted latency-based routing with GKE Inference Gateway.
  * **Inference observability** : provides observability metrics for inference requests, such as request rate, latency, errors, and saturation. Monitor the performance and behavior of your inference services through Cloud Monitoring and Cloud Logging, leveraging specialized prebuilt dashboards for detailed insights. For more information, see [View GKE Inference Gateway dashboard](https://docs.cloud.google.com/kubernetes-engine/docs/how-to/customize-gke-inference-gateway-configurations#inference-gateway-dashboard).
  * **CORS configuration** : supports Cross-Origin Resource Sharing (CORS) on `HTTPRoute` resources. CORS helps web applications that run on different domains securely access your inference endpoints. CORS is supported for single-cluster Gateways.
  * **Advanced API Management with Apigee** : integrates with Apigee to enhance your inference gateway with features such as API security, rate limiting, and quotas. For detailed instructions, see [Configure Apigee for authentication and API management](https://docs.cloud.google.com/kubernetes-engine/docs/how-to/customize-gke-inference-gateway-configurations#config-auth-api-management).
  * **Extensibility** : built on an extensible, open-source Kubernetes Gateway API Inference Extension that supports a user-managed llm-d Endpoint Picker (EPP) algorithm, powered by the llm-d. The **llm-d Endpoint Picker (EPP)** algorithm, powered by llm-d, provides the core routing intelligence for this extension.
  * **Multiport support** : supports model servers that expose multiple ports, which is essential for advanced serving scenarios such as Data Parallel attention.
  * **Network Endpoint Group (NEG) limits** : has a limit of 50 NEGs per Google Cloud Backend Service. When using a multi-port InferencePool, each port in each zone creates a dedicated NEG. For example, an InferencePool with eight ports in a typical regional cluster (three zones) generates 24 NEGs. Therefore, a multi-cluster Gateway can only aggregate such an InferencePool from a maximum of two clusters (two clusters × 24 NEGs = 48 NEGs) before reaching the 50 NEG limit.


### Understand key concepts
GKE Inference Gateway enhances the existing [GKE Gateway](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/gateway-api#gateway) that uses [`GatewayClass`](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/gateway-api#gatewayclass) objects. GKE Inference Gateway introduces the following new Gateway API Custom Resource Definitions (CRDs), aligned with the [OSS Kubernetes Gateway API extension for Inference](https://gateway-api-inference-extension.sigs.k8s.io/concepts/api-overview/):
  * **InferencePool object** : represents a group of Pods (containers) that share the same compute configuration, accelerator type, base language model, and model server. This logically groups and manages your AI model serving resources. A single InferencePool object can span multiple Pods across different GKE nodes and provides scalability and high availability. You can specify up to eight `targetPorts` in an InferencePool resource to support model servers that require multiple ports.
  * **InferenceObjective object** : specifies the serving model's name from the InferencePool according to the `OpenAI API` specification. The InferenceObjective object also specifies the model's serving properties, such as the AI model's `Priority`. GKE Inference Gateway gives preference to workloads with a higher priority value. This lets you multiplex latency-critical and latency-tolerant AI workloads on a GKE cluster. You can also configure the InferenceObjective object to serve LoRA fine-tuned models.


The following diagram illustrates GKE Inference Gateway and its integration with AI safety, observability, and model serving within a GKE cluster.
**Figure:** GKE Inference Gateway resource model
The following diagram illustrates the resource model that focuses on two new [inference-focused personas](https://gateway-api-inference-extension.sigs.k8s.io/concepts/roles-and-personas) and the resources they manage.
**Figure:** GKE Inference Gateway resource model with inference-focused personas
## llm-d Router
The `llm-d` Router is the intelligent request routing component that Inference Gateway uses to make per request endpoint decisions. It is composed of two sub-components:  
| Sub-component  | Description  |  
| --- | --- |  
| **`L7 Proxy`**  | Any conformant industry-grade L7 proxy (typically Envoy) that handles the data plane: connection management, TLS termination, and request forwarding. In GKE Inference Gateway (Gateway Mode), the Proxy is the GKE Gateway.  |  
| **`llm-d Endpoint Picker (EPP)`**  | A specialized service that the Proxy consults for every request using the `ext-proc` protocol. The EPP contains the routing intelligence. It uses real-time signals from model servers (KV cache utilization, queue length, prefix cache state, and LoRA adapter affinity) to select the optimal model server Pod for each request.  |  
### Gateway Mode
GKE Inference Gateway is the `llm-d Router` operating in Gateway Mode. In Gateway Mode, the Proxy is a formal Kubernetes Gateway provisioned and managed separately from the EPP service. The Gateway calls the EPP over `ext-proc` to make routing decisions, then forwards the request directly to the selected model server Pod.
This separation of the Gateway (data plane) from the EPP (routing intelligence) enables:
  * **Shared infrastructure** : a single GKE Gateway serves multiple InferencePools alongside standard Kubernetes Services.
  * **Advanced traffic management** : `HTTPRoute` policies support weighted splitting, gradual rollouts, and request mirroring.
  * **Independent scaling** : the EPP service scales independently of the Gateway.
  * **Cloud-native integration** : works with GKE's managed Gateway controller, Cloud Load Balancing, and existing observability tooling.


## How GKE Inference Gateway works
GKE Inference Gateway uses Gateway API extensions and model-specific routing logic to handle client requests to an AI model. The following steps describe the request flow.
### How the request flow works
GKE Inference Gateway routes client requests from the initial request to a model instance. This section describes how GKE Inference Gateway handles requests. This request flow is common for all clients.
  1. The client sends a request, formatted as described in the [OpenAI API](https://platform.openai.com/docs/api-reference/chat) specification, to the model running in GKE.
  2. GKE Inference Gateway processes the request using the following inference extensions:
    1. **Body-based routing extension** : extracts the model identifier from the client request body and sends it to GKE Inference Gateway. GKE Inference Gateway then uses this identifier to route the request based on rules defined in the Gateway API `HTTPRoute` object. Request body routing is similar to routing based on the URL path. The difference is that request body routing uses data from the request body.
    2. **Security extension** : uses Model Armor, [NVIDIA NeMo Guardrails](https://docs.nvidia.com/nemo/microservices/latest/set-up/deploy-as-microservices/guardrails/gcp-installation.html), or supported third-party solutions to enforce model-specific security policies, which include content filtering, threat detection, sanitization, and logging. The security extension applies these policies to both request and response processing paths.
    3. **llm-d Endpoint Picker (EPP)** : monitors key metrics from model servers within the InferencePool and routes the request to the optimal model replica. For more information, see [llm-d Router](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway#llm-d-router).
  3. GKE Inference Gateway routes the request to the model replica returned by the endpoint picker extension.


The following diagram illustrates the request flow from a client to a model instance through GKE Inference Gateway.
**Figure:** GKE Inference Gateway request flow
### How traffic distribution works
GKE Inference Gateway dynamically distributes inference requests to model servers within the InferencePool object. This helps optimize resource utilization and maintains performance under varying load conditions. GKE Inference Gateway uses the following two mechanisms to manage traffic distribution:
  * **Endpoint picking** : dynamically selects the most suitable model server to handle an inference request. It monitors server load and availability, then makes optimal routing decisions by calculating a `score` for each server combining a number of optimization heuristics:
    * **Prefix cache aware routing** : GKE Inference Gateway tracks available prefix cache indexes on each model server, and gives a higher score to a server with a longer prefix cache match.
    * **Load aware routing** : GKE Inference Gateway monitors server load (KV cache utilization and pending queue depth), and gives a higher score to a server with lower load.
    * **LoRA aware routing** : when dynamic LoRA serving is enabled, GKE Inference Gateway monitors active LoRA adapters per server, and gives a higher score to a server with the requested LoRA adapter active, or additional room to dynamically load the requested LoRA adapter. A server with the highest total score of all the preceding is picked.
  * **Queueing and shedding** : manages request flow and prevents traffic overload. GKE Inference Gateway stores incoming requests in a queue and prioritizes requests based on defined priority.


GKE Inference Gateway uses a numerical `Priority` system, also known as `Criticality`, to manage request flow and prevent overload. This `Priority` is an optional integer field defined by the user for each InferenceObjective. A higher value signifies a more important request. When the system is under pressure, requests with a `Priority` less than `0` are considered lower priority and are dropped first, returning a `429` error to protect more critical workloads. By default, the `Priority` is `0`. Requests are only dropped due to priority if their `Priority` is explicitly set to a value less than `0`. This system lets you prioritize latency-sensitive online inference traffic over less time-sensitive batch jobs.
GKE Inference Gateway supports streaming inference for applications like chatbots and live translation, which require continuous or near-real-time updates. Streaming inference delivers responses in incremental chunks or segments, instead of a single, complete output. If an error occurs during a streaming response, the stream terminates, and the client receives an error message. GKE Inference Gateway does not retry streaming responses.
## Explore application examples
This section provides examples of using GKE Inference Gateway to address various generative AI application scenarios.
### Example 1: Serve multiple generative AI models on a GKE cluster
A company wants to deploy multiple large language models (LLMs) to serve different workloads. For example, they might want to deploy a `Gemma3` model for a chatbot interface and a `DeepSeek` model for a recommendation application. The company needs to ensure optimal serving performance for these LLMs.
Using GKE Inference Gateway, you can deploy these LLMs on your GKE cluster with your chosen accelerator configuration in an InferencePool. You can then route requests based on the model name (such as `chatbot` and `recommender`) and the `Priority` property.
The following diagram illustrates how GKE Inference Gateway routes requests to different models based on the model name and `Priority`.
**Figure:** Serving multiple generative AI models on a GKE cluster using GKE Inference Gateway
This diagram illustrates how a request to a GenAI service on `example.com/completions` is handled by GKE Inference Gateway. The request first reaches a `Gateway` in the `Infra` namespace. This `Gateway` forwards the request to an `HTTPRoute` in the `GenAI Inference` namespace, which is configured to handle requests for chatbot and sentiment models. For the chatbot model, the `HTTPRoute` splits traffic: 90% is directed to an InferencePool running the current model version (selected by `{pool: gemma}`), and 10% goes to a pool with a newer version (`{pool: gemma-new}`), typically for canary testing. Both pools are linked to an InferenceObjective that assigns a `Priority` of 10 to requests for the chatbot model, ensuring these requests are treated as high priority.
### Example 2: Serve LoRA adapters on a shared accelerator
A company wants to serve LLMs for document analysis and focuses on audiences in multiple languages, such as English and Spanish. They have fine-tuned models for each language, but need to efficiently use their GPU and TPU capacity. You can use GKE Inference Gateway to deploy dynamic LoRA fine-tuned adapters for each language (for example, `english-bot` and `spanish-bot`) on a common base model (for example, `llm-base`) and accelerator. This lets you reduce the number of required accelerators by densely packing multiple models on a common accelerator.
The following diagram illustrates how GKE Inference Gateway serves multiple LoRA adapters on a shared accelerator.
**Figure:** Serving LoRA adapters on a shared accelerator
## What's next
  * [Deploy GKE Inference Gateway](https://docs.cloud.google.com/kubernetes-engine/docs/how-to/deploy-gke-inference-gateway)
  * [Customize GKE Inference Gateway configuration](https://docs.cloud.google.com/kubernetes-engine/docs/how-to/customize-gke-inference-gateway-configurations)
  * [Serve an LLM with GKE Inference Gateway](https://docs.cloud.google.com/kubernetes-engine/docs/tutorials/serve-with-gke-inference-gateway)
  * [Use predicted latency-based routing with GKE Inference Gateway](https://docs.cloud.google.com/kubernetes-engine/docs/how-to/use-predicted-latency-based-routing)(Preview)


Was this helpful?
Send feedback 
Except as otherwise noted, the content of this page is licensed under the [Creative Commons Attribution 4.0 License](https://creativecommons.org/licenses/by/4.0/), and code samples are licensed under the [Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0). For details, see the [Google Developers Site Policies](https://developers.google.com/site-policies). Java is a registered trademark of Oracle and/or its affiliates.
Last updated 2026-09-09 UTC.
