---
title: Understanding LLMInferenceService | KServe
id: understanding-llminferenceservice-kserve
tags:
- internal-llm-inference-stacks-7f0c90
- kserve
- llm-d
- cncf
- layer-boundaries
created: '2026-10-09T22:59:41.048554Z'
updated: '2026-10-10T14:52:17.338582Z'
source: https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview
source_domain: kserve.github.io
fetched_at: '2026-10-09T22:59:41.027117Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'KServe''s own documentation (v0.20 docs) for LLMInferenceService, the CRD
  KServe added specifically for generative AI serving. States directly: ''Built on
  the foundation of llm-d -- a production-ready framework for scalable LLM serving
  -- LLMInferenceService delivers enterprise-grade capabilities for deploying and
  managing Large Language Model inference workloads on Kubernetes.'' Documents KServe''s
  deliberate dual-track CRD split: the legacy InferenceService CRD remains for ''Predictive
  AI'' (scikit-learn, TensorFlow, PyTorch); LLMInferenceService is ''purpose-built
  for Generative AI workloads'' and is the only path to prefill-decode disaggregation,
  multi-node orchestration, and intelligent (KV-cache-aware) scheduling -- InferenceService
  ''can still serve LLMs, but only for basic single-node deployments.'' This is a
  direct, named instance of a broad existing platform absorbing a narrower orchestration-layer
  project''s capabilities via CRD rather than building them independently, exactly
  matching llm-d''s own stated non-goal of not duplicating KServe''s platform. Confirms
  KServe''s own governance milestone: a banner states ''KServe has joined CNCF!''
  (as an Incubating project, Nov 2025) -- places KServe and llm-d in the same CNCF
  governance structure but at different maturity levels (KServe Incubating vs llm-d
  Sandbox per other sources). Lists concrete production-oriented features: RBAC/authentication,
  model storage integration (HuggingFace/S3/PVC), KV cache transfer via RDMA, tensor/data/expert
  parallelism (the latter explicitly for MoE models like Mixtral and DeepSeek-R1),
  and a worked YAML example deploying Llama-3.1-8B across 3 replica pods with a managed
  gateway/route/scheduler.'
---

*Suggested by [[llm-d-kubernetes-native-distributed-inference-at-scale-llm-d]] — llm-d founding proposal cites this CRD as exposing llm-d optimizations into KServe*

[Skip to main content](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#__docusaurus_skipToContent_fallback)
🎉️ **[KServe has joined CNCF!](https://www.cncf.io/blog/2025/11/11/kserve-becomes-a-cncf-incubating-project/)** 🥳️
Version: 0.20
On this page
## What is LLMInferenceService?[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#what-is-llminferenceservice "Direct link to What is LLMInferenceService?")
**LLMInferenceService** is a Kubernetes Custom Resource Definition (CRD) introduced in KServe as part of its strategic shift towards **GenAI-first architecture**. Built on the foundation of [llm-d](https://github.com/llm-d)—a production-ready framework for scalable LLM serving—LLMInferenceService delivers enterprise-grade capabilities for deploying and managing Large Language Model inference workloads on Kubernetes.
The llm-d project provides a proven architecture for high-performance LLM serving, combining vLLM's inference engine with Kubernetes orchestration and intelligent routing capabilities. Features like KV-cache aware scheduling, disaggregated prefill-decode serving, and distributed inference enable both optimal performance and cost efficiency. By integrating llm-d's architecture through a native Kubernetes CRD, KServe makes these advanced patterns accessible and easy to deploy, allowing users to achieve faster time-to-value while maintaining production-grade reliability.
### Why a Separate CRD?[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#why-a-separate-crd "Direct link to Why a Separate CRD?")
While KServe has traditionally used the `InferenceService` CRD for serving machine learning models (and it can still serve LLMs), KServe now adopts a dual-track strategy:
  * **`InferenceService`**: Optimized for**Predictive AI** workloads (traditional ML models like scikit-learn, TensorFlow, PyTorch)
  * **`LLMInferenceService`**: Purpose-built for**Generative AI** workloads (Large Language Models)


This separation allows KServe to provide specialized features for LLM serving—such as distributed inference, prefill-decode separation, advanced routing, and multi-node orchestration—without adding complexity to the traditional InferenceService API.
## Evolution: Dual-Track Strategy[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#evolution-dual-track-strategy "Direct link to Evolution: Dual-Track Strategy")
**Strategic Separation:**
  * **InferenceService** : Remains the standard for **Predictive AI** (classification, regression, recommendations)
  * **LLMInferenceService** : Dedicated to **Generative AI** with specialized optimizations
  * **Can you use InferenceService for LLMs?** Yes, but only for basic single-node deployments. Advanced features like prefill-decode separation, multi-node orchestration, and intelligent scheduling are not available


## Key Features[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#key-features "Direct link to Key Features")
### 🎯 Composable Configuration[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#-composable-configuration "Direct link to 🎯 Composable Configuration")
Mix and match **LLMInferenceServiceConfig** resources for flexible deployment patterns:
  * Model configurations
  * Workload templates
  * Router settings
  * Scheduler policies


**Learn more** : [Config Composition Deep Dive](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-config-composition)
### 🚀 Multiple Deployment Patterns[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#-multiple-deployment-patterns "Direct link to 🚀 Multiple Deployment Patterns")
  * **Single-Node** : Simple deployments for small models (less than 7B parameters)
  * **Multi-Node** : Distributed inference with LeaderWorkerSet for medium-large models
  * **Prefill-Decode** : Disaggregated serving for cost optimization
  * **DP+EP** : Data and Expert Parallelism for MoE models (Mixtral, DeepSeek-R1)


### 🌐 Advanced Routing[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#-advanced-routing "Direct link to 🌐 Advanced Routing")
  * **Gateway API** : Standard Kubernetes ingress
  * **Intelligent Scheduling** : KV cache-aware, load-balanced routing
  * **Prefill-Decode Separation** : Automatic routing to optimal pools


### ⚡ Distributed Inference[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#-distributed-inference "Direct link to ⚡ Distributed Inference")
  * **Tensor Parallelism (TP)** : Split model layers across GPUs
  * **Data Parallelism (DP)** : Replicate models for throughput
  * **Expert Parallelism (EP)** : Distribute MoE experts across nodes


### 🔧 Production-Ready[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#-production-ready "Direct link to 🔧 Production-Ready")
  * RBAC and authentication
  * Model storage integration (HuggingFace, S3, PVC)
  * KV cache transfer via RDMA
  * Monitoring and metrics


## When to Use LLMInferenceService[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#when-to-use-llminferenceservice "Direct link to When to Use LLMInferenceService")  
| Scenario  | Use LLMInferenceService?  | Why?  |  
| --- | --- | --- |  
| **Serving LLMs (7B-405B params)**  | ✅ Yes  | Optimized for LLM workloads  |  
| **Multi-GPU inference**  | ✅ Yes  | Built-in parallelism support  |  
| **High throughput requirements**  | ✅ Yes  | Prefill-decode separation, intelligent routing  |  
| **Traditional ML models**  | ❌ No  | Use `InferenceService` instead  |  
| **Small models (less than 70B params)**  | 🟡 Optional  | Either works, but `LLMInferenceService` offers more features  |  
## High Level Architecture Overview[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#high-level-architecture-overview "Direct link to High Level Architecture Overview")
## Core Components at a Glance[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#core-components-at-a-glance "Direct link to Core Components at a Glance")
### 1. Model Specification (`spec.model`)[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#1-model-specification-specmodel "Direct link to 1-model-specification-specmodel")
Defines the LLM model source, name, and characteristics:
  * Model URI (HuggingFace, S3, PVC)
  * Model name for API requests
  * Scheduling criticality
  * **LoRA adapters** (optional) - Attach task-specific Low-Rank Adaptation modules for efficient multi-tenant serving


**Learn more** : [Configuration Guide](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-configuration#model-specification) | [LoRA Adapters](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/lora-adapters)
### 2. Workload Specification[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#2-workload-specification "Direct link to 2. Workload Specification")
Defines compute resources and deployment patterns:
  * **`spec.template`**: Single-node or decode workload
  * **`spec.worker`**: Multi-node workers (triggers LeaderWorkerSet)
  * **`spec.prefill`**: Prefill-only workload (disaggregated)


**Learn more** : [Configuration Guide](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-configuration#workload-specification)
### 3. Router Specification (`spec.router`)[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#3-router-specification-specrouter "Direct link to 3-router-specification-specrouter")
Defines traffic routing and load balancing:
  * **Gateway** : Entry point for external traffic
  * **HTTPRoute** : Path-based routing rules
  * **Scheduler** : Intelligent endpoint selection (EPP)


**Learn more** : [Architecture Guide](https://kserve.github.io/website/docs/concepts/architecture/control-plane-llmisvc#gateway-architecture)
### 4. Parallelism Specification (`spec.parallelism`)[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#4-parallelism-specification-specparallelism "Direct link to 4-parallelism-specification-specparallelism")
Defines distributed inference strategies:
  * Tensor Parallelism (TP)
  * Data Parallelism (DP)
  * Expert Parallelism (EP)


**Learn more** : [Configuration Guide](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-configuration#parallelism-specification)
## Quick Example[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#quick-example "Direct link to Quick Example")
Here's a minimal LLMInferenceService for serving Llama-3.1-8B:

```
apiVersion: serving.kserve.io/v1alpha1kind: LLMInferenceServicemetadata:name: llama-3-8bnamespace: defaulturi: hf://meta-llama/Llama-3.1-8B-Instructname: meta-llama/Llama-3.1-8B-Instructreplicas:3template:containers:-name: mainimage: vllm/vllm-openai:latestresources:limits:nvidia.com/gpu:"1"memory: 32Girouter:gateway:{}# Managed gatewayroute:{}# Managed HTTPRoutescheduler:{}# Managed scheduler
```

**What this creates** :
  * 3 replica pods with 1 GPU each
  * Gateway API ingress
  * Intelligent scheduler for load balancing
  * Storage initializer for model download


## Documentation Map[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#documentation-map "Direct link to Documentation Map")
This overview provides a high-level introduction to LLMInferenceService. For detailed information, explore the following guides:
### 📚 Core Concepts[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#-core-concepts "Direct link to 📚 Core Concepts")
  * : Detailed spec reference and configuration patterns
  * : System architecture and component interactions
  * : Required infrastructure components


### 🔧 Advanced Topics[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#-advanced-topics "Direct link to 🔧 Advanced Topics")
  * : Inference-aware autoscaling using the Workload Variant Autoscaler
  * **Scheduler Configuration** : Prefix cache routing, load-aware scheduling
  * **Multi-Node Deployment** : LeaderWorkerSet, RDMA networking
  * **Security** : Authentication, RBAC, network policies


## Summary[​](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#summary "Direct link to Summary")
**LLMInferenceService** provides a comprehensive, Kubernetes-native approach to LLM serving with:
  * ✅ **[Composable Configuration](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-config-composition)** : Mix and match configs for flexible deployment
  * ✅ **Multiple Workload Patterns** : Single-node, multi-node, prefill-decode separation
  * ✅ **Advanced Routing** : Gateway API + intelligent scheduler
  * ✅ **Distributed Inference** : Tensor, data, and expert parallelism
  * ✅ **Production-Ready** : Monitoring, RBAC, storage integration, KV cache transfer


This architecture enables organizations to deploy and scale LLM inference workloads efficiently on Kubernetes, with the flexibility to optimize for different model sizes, hardware configurations, and performance requirements.
  * [What is LLMInferenceService?](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#what-is-llminferenceservice)
  * [Evolution: Dual-Track Strategy](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#evolution-dual-track-strategy)
  * [Key Features](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#key-features)
    * [🎯 Composable Configuration](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#-composable-configuration)
    * [🚀 Multiple Deployment Patterns](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#-multiple-deployment-patterns)
    * [⚡ Distributed Inference](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#-distributed-inference)
  * [When to Use LLMInferenceService](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#when-to-use-llminferenceservice)
  * [High Level Architecture Overview](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#high-level-architecture-overview)
  * [Core Components at a Glance](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#core-components-at-a-glance)
    * [1. Model Specification (`spec.model`)](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#1-model-specification-specmodel)
    * [2. Workload Specification](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#2-workload-specification)
    * [3. Router Specification (`spec.router`)](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#3-router-specification-specrouter)
    * [4. Parallelism Specification (`spec.parallelism`)](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#4-parallelism-specification-specparallelism)
  * [Documentation Map](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview#documentation-map)


