---
title: 'Best of Both Worlds: Cloud-Native AI Inference at Scale using KServe and llm-d
  | KServe'
id: best-of-both-worlds-cloud-native-ai-inference-at-scale-using-kserve-and-llm-d-ks
tags:
- llmd-router-nonk8s-backends-54b774
- llmd-router
- llminferenceservice
- llm-d-architecture
created: '2026-09-11T20:49:38.145066Z'
updated: '2026-09-12T03:37:37.699701Z'
source: https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d
source_domain: kserve.github.io
fetched_at: '2026-09-11T20:49:38.122865Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'KServe blog post (post-v0.16, referenced by the Red Hat June 2026 article)
  providing the clearest concise layer-responsibility table for the KServe+llm-d stack:
  KServe = model lifecycle/scaling/governance; LLMInferenceService = generative API
  abstraction; vLLM = efficient execution inside the runtime; llm-d = cross-runtime
  routing and cache awareness; Kubernetes = resource orchestration. States ''llm-d
  provides the cluster-wide optimization'' while KServe/LLMInferenceService supplies
  the OpenAI-compatible API surface (/v1/chat/completions, streaming, multi-turn)
  -- confirming llm-d''s router role is scoped strictly to cross-instance scheduling,
  not API surface or lifecycle, which clarifies exactly which slice a non-k8s-backend
  replacement needs to reproduce. Cites llm-d benchmark numbers (noted as sourced
  from the llm-d project, not independently reproduced by KServe): up to ~57x faster
  P90 TTFT, throughput improving from ~4,400 to ~8,730 tokens/sec (~2x) in a baseline
  test cluster, sustained 4.5k-11k tokens/sec at scale, and ~50% P95/P99 tail latency
  reduction versus round-robin load balancing.'
---

*Suggested by [[intelligent-inference-scheduling-with-llm-d-on-red-hat-ai-red-hat-developer]] — cited for architecture context on how KServe and llm-d work together*

[Skip to main content](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#__docusaurus_skipToContent_fallback)
🎉️ **[KServe has joined CNCF!](https://www.cncf.io/blog/2025/11/11/kserve-becomes-a-cncf-incubating-project/)** 🥳️
Enterprises today seek to integrate generative AI (GenAI) capabilities into their applications. However, scaling large AI models introduces complexity: managing high-volume traffic from large language models (LLMs), optimizing inference performance, maintaining predictable latency, and controlling infrastructure costs.
Platform engineering leaders require more than just model deployment capabilities. They need a robust, Kubernetes-native infrastructure that supports:
  * Efficient GPU utilization
  * Intelligent request routing
  * Distributed inference patterns
  * Cost-aware autoscaling
  * Production-grade governance


This article demonstrates how two open-source solutions, KServe and llm-d, can be combined to address these challenges.
We explore the role of each solution, illustrate their integration architecture, and provide practical guidance for AI platform teams, with deeper focus on KServe's LLMInferenceService, available since KServe v0.16.
## KServe: Simplified Deployment of AI Models on Kubernetes[​](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#kserve-simplified-deployment-of-ai-models-on-kubernetes "Direct link to KServe: Simplified Deployment of AI Models on Kubernetes")
KServe is a Kubernetes-based model serving platform that simplifies deploying and managing ML models, including LLMs, at scale.
For platform engineers, KServe acts as the model serving control plane: the layer responsible for lifecycle, scaling, and operational governance.
### Inference as a Service[​](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#inference-as-a-service "Direct link to Inference as a Service")
InferenceService serves as KServe's core abstraction for model deployment, encapsulating the full serving lifecycle, including:
  * Automatic deployment creation and reconciliation
  * Request-based autoscaling with scale-to-zero and autoscaling based on custom metrics
  * Revision management and canary rollouts
  * Endpoint exposure and traffic routing
  * Runtime abstraction across serving backends for both predictive and generative AI
  * Optional pre-processing/post-processing, inference pipelines, and ensembles


ML engineers provide trained models. Platform engineers retain operational control without writing custom deployment code.
### LLMInferenceService in KServe[​](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#llminferenceservice-in-kserve "Direct link to LLMInferenceService in KServe")
KServe v0.16 introduces stronger generative AI capabilities, including LLMInferenceService, designed specifically for large language model workloads.
Unlike traditional stateless predictors, LLM workloads require:
  * Long-running streaming responses
  * GPU-heavy memory footprints
  * Prefix KV-cache management
  * High-concurrency token streaming
  * OpenAI-compatible APIs


LLMInferenceService shares common foundations with InferenceService but introduces additional capabilities tailored for large language models, including:
### Unlocking Generative AI Serving with LLMInferenceService: From Pod-Level Speed to Cluster-Wide Intelligence[​](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#unlocking-generative-ai-serving-with-llminferenceservice-from-pod-level-speed-to-cluster-wide-intelligence "Direct link to Unlocking Generative AI Serving with LLMInferenceService: From Pod-Level Speed to Cluster-Wide Intelligence")
Imagine you want to bring the power of generative AI directly into your applications, but without rewriting your entire stack. It offers OpenAI-compatible endpoints like `/v1/chat/completions`, complete with streaming token responses and multi-turn support. With prompt templating built in, developers can integrate seamlessly with existing tools—whether it's the OpenAI SDKs, LangChain, LlamaIndex, Llama Stack, RAG frameworks, or even enterprise GenAI gateways.
Under the hood, KServe connects to LLM-optimized runtimes such as vLLM, Hugging Face TGI, or other GPU-native backends. These engines bring advanced capabilities like continuous batching, memory-efficient paged attention, and KV-cache reuse, delivering high throughput per GPU.
Yet, while these runtime-level optimizations make each pod lightning fast, true cluster-wide efficiency needs more. That's exactly the role of llm-d: adding an extra layer of intelligence that orchestrates resources and maximizes performance across the entire deployment.
### Distributed & Multi-Node Model Support[​](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#distributed--multi-node-model-support "Direct link to Distributed & Multi-Node Model Support")
LLMInferenceService supports advanced parallelism strategies implemented by runtimes, including tensor parallelism, pipeline parallelism, and multi-GPU sharding.
This enables hosting 70B+ parameter models, partitioning models across nodes, and serving models larger than single-GPU memory.
KServe orchestrates the deployment topology, while the runtime manages execution parallelism.
### Advanced Autoscaling & Networking (Including Scale-to-Zero)[​](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#advanced-autoscaling--networking-including-scale-to-zero "Direct link to Advanced Autoscaling & Networking \(Including Scale-to-Zero\)")
KServe integrates deeply with Kubernetes to support request- and concurrency-based autoscaling via Knative, GPU-backed scaling, and scale-to-zero for cost control.
It also integrates with the Kubernetes Gateway API for TLS termination, traffic splitting, and advanced routing.
This makes it suitable for development environments, internal copilots, and large-scale production workloads.
### Kubernetes Gateway API Integration[​](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#kubernetes-gateway-api-integration "Direct link to Kubernetes Gateway API Integration")
KServe integrates with Kubernetes Gateway API for:
  * Enterprise-grade routing
  * TLS termination
  * Traffic splitting
  * Multi-model routing


This enables integration with modern Kubernetes networking stacks.
### Where KServe Alone Is Not Enough[​](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#where-kserve-alone-is-not-enough "Direct link to Where KServe Alone Is Not Enough")
Even with LLMInferenceService and optimized runtimes, KServe does not inherently:
  * Route requests based on KV-cache locality across replicas
  * Separate prefill and decode cluster-wide
  * Perform SLA-aware routing decisions
  * Optimize GPU utilization across multiple pods


To address these, we introduce llm-d.
## llm-d: Distributed Intelligence for LLM Inference[​](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#llm-d-distributed-intelligence-for-llm-inference "Direct link to llm-d: Distributed Intelligence for LLM Inference")
llm-d is a Kubernetes-native distributed inference framework designed to enhance performance and efficiency of LLM workloads.
If KServe is the control plane for models, llm-d is the distributed intelligence scheduling layer.
### KV-Cache Aware Scheduling and Disaggregated Inference with llm-d[​](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#kv-cache-aware-scheduling-and-disaggregated-inference-with-llm-d "Direct link to KV-Cache Aware Scheduling and Disaggregated Inference with llm-d")
As LLM deployments mature, scaling is no longer just about adding GPUs. It's about using them intelligently. Modern runtimes such as vLLM introduced prefix (KV) caching to reduce redundant computation, but without smart scheduling, much of that benefit is lost.
This is where llm-d changes the game.
### Disaggregated Inference (Prefill / Decode Separation)[​](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#disaggregated-inference-prefill--decode-separation "Direct link to Disaggregated Inference \(Prefill / Decode Separation\)")
LLM inference consists of two distinct phases: prefill and decode. The prefill phase is compute-heavy, processing the full prompt and building the model's attention context. The decode phase is latency-sensitive, generating tokens step by step where responsiveness directly impacts user experience.
llm-d separates these phases across different GPU groups, assigning compute-optimized resources to prefill and latency-optimized resources to decode. With intelligent scheduling between them, workloads are aligned to the right hardware profile.
This phase-aware architecture increases GPU utilization, reduces tail latency, and lowers cost per token by eliminating resource contention between fundamentally different workloads.
### Intelligent Inference Scheduler[​](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#intelligent-inference-scheduler "Direct link to Intelligent Inference Scheduler")
llm-d's inference scheduler evaluates the following metrics:
  * GPU utilization
  * Queue depth
  * Cache residency
  * SLA constraints
  * Load distribution


It enhances load balancing with an intelligent scheduler to decrease serving latency and increase throughput with prefix-cache aware routing, utilization-based load balancing, fairness and prioritization for multi-tenant serving, and predicted latency balancing.
## KServe LLMInferenceService and llm-d[​](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#kserve-llminferenceservice-and-llm-d "Direct link to KServe LLMInferenceService and llm-d")
### Responsibility Separation[​](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#responsibility-separation "Direct link to Responsibility Separation")
This layered design ensures composability and specialization, providing a complete, production-ready solution for generative AI. KServe acts as the control plane and LLMInferenceService delivers the generative API abstraction, while llm-d provides the cluster-wide optimization.  
| Layer  | Responsibility  |  
| --- | --- |  
| KServe  | Model lifecycle, scaling, governance  |  
| LLMInferenceService  | Generative API abstraction  |  
| vLLM  | Efficient execution inside runtime  |  
| llm-d  | Cross-runtime routing & cache awareness  |  
| Kubernetes  | Resource orchestration  |  
Together, KServe and llm-d enable a production-ready, Kubernetes-native inference platform that balances scalability, performance, and cost efficiency, providing the best of both worlds for cloud-native AI inference at scale.
## Cost Efficiency Comparison: Naive vs Optimized[​](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#cost-efficiency-comparison-naive-vs-optimized "Direct link to Cost Efficiency Comparison: Naive vs Optimized")
Serving LLMs at scale is no longer just a model problem. It is a distributed systems problem where naive load balancing leads to significant inefficiencies and wasted resources — lost cache locality, GPU imbalance, redundant prefill processing, high tail latency, and overprovisioned GPUs.
**Naive Problems:**
  * Cache locality loss
  * GPU imbalance
  * Redundant prefill processing
  * High tail latency
  * Overprovisioned GPUs


### Optimized Architecture with KServe + llm-d[​](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#optimized-architecture-with-kserve--llm-d "Direct link to Optimized Architecture with KServe + llm-d")
The combined KServe and llm-d solution introduces distributed intelligence to solve the problems of naive architectures, delivering superior performance, scalability, and cost control. This optimized architecture is pluggable and extensible to work well with many AI and cloud-native technologies.
**Benefits:**
  * Cache reuse preserved
  * Balanced GPU utilization
  * Reduced recomputation
  * Lower cost per token
  * Controlled autoscaling via LLMInferenceService


## Benchmark Results: Why Cluster-Level Intelligence Matters[​](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#benchmark-results-why-cluster-level-intelligence-matters "Direct link to Benchmark Results: Why Cluster-Level Intelligence Matters")
By integrating llm-d's cache-aware routing, prefill and decode disaggregation, and SLA-based scheduling with KServe's enterprise-grade generative serving and autoscaling, the system achieves cluster-wide GPU optimization.
_Note: The following results are based on benchmarks published by the llm-d project_  
| Optimization Area  | Naive Architecture (Round Robin LB)  | Optimized (KServe + llm-d)  | Source  |  
| --- | --- | --- | --- |  
| Cache Locality  | Requests routed randomly → KV cache frequently missed  | Cache-aware routing preserves prefix locality  |  
| Time to First Token (P90)  | Baseline latency under cache-blind scheduling  | Up to ~57× faster P90 TTFT in benchmark  |  
| Token Throughput  | ~4,400 tokens/sec (baseline test cluster)  | ~8,730 tokens/sec (~2× improvement)  |  
| Throughput at Scale  | Degrades under multi-tenant load  | Sustained 4.5k–11k tokens/sec  |  
| Tail Latency (P95/P99)  | Higher tail latency due to stragglers & imbalance  | ~50% tail latency reduction (reported tests)  |  
| GPU Utilization  | Uneven utilization, idle GPUs possible  | Improved effective utilization via routing intelligence  |  
| Autoscaling Control  | Scale reacts to load only  | Works with KServe autoscaling + routing intelligence  |  
Modern GenAI platforms require cache locality awareness, phase-aware scheduling, distributed intelligence, and composable Kubernetes-native design. This combination ensures a production-ready system that meets the demands of large-scale production workloads.
## Next Steps[​](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#next-steps "Direct link to Next Steps")
Explore detailed project documentation:


Engage with community resources and Slack channels to stay updated and contribute to ongoing developments:


  * [KServe: Simplified Deployment of AI Models on Kubernetes](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#kserve-simplified-deployment-of-ai-models-on-kubernetes)
    * [Inference as a Service](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#inference-as-a-service)
    * [LLMInferenceService in KServe](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#llminferenceservice-in-kserve)
    * [Unlocking Generative AI Serving with LLMInferenceService: From Pod-Level Speed to Cluster-Wide Intelligence](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#unlocking-generative-ai-serving-with-llminferenceservice-from-pod-level-speed-to-cluster-wide-intelligence)
    * [Distributed & Multi-Node Model Support](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#distributed--multi-node-model-support)
    * [Advanced Autoscaling & Networking (Including Scale-to-Zero)](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#advanced-autoscaling--networking-including-scale-to-zero)
    * [Kubernetes Gateway API Integration](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#kubernetes-gateway-api-integration)
    * [Where KServe Alone Is Not Enough](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#where-kserve-alone-is-not-enough)
  * [llm-d: Distributed Intelligence for LLM Inference](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#llm-d-distributed-intelligence-for-llm-inference)
    * [KV-Cache Aware Scheduling and Disaggregated Inference with llm-d](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#kv-cache-aware-scheduling-and-disaggregated-inference-with-llm-d)
    * [Disaggregated Inference (Prefill / Decode Separation)](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#disaggregated-inference-prefill--decode-separation)
    * [Intelligent Inference Scheduler](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#intelligent-inference-scheduler)
  * [KServe LLMInferenceService and llm-d](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#kserve-llminferenceservice-and-llm-d)
    * [Responsibility Separation](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#responsibility-separation)
  * [Cost Efficiency Comparison: Naive vs Optimized](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#cost-efficiency-comparison-naive-vs-optimized)
    * [Optimized Architecture with KServe + llm-d](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#optimized-architecture-with-kserve--llm-d)
  * [Benchmark Results: Why Cluster-Level Intelligence Matters](https://kserve.github.io/website/blog/cloud-native-ai-inference-kserve-llm-d#benchmark-results-why-cluster-level-intelligence-matters)


