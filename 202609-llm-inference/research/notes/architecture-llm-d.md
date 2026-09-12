---
title: Architecture | llm-d
id: architecture-llm-d
tags:
- llmd-router
- epp-architecture
- inferencepool
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:42:59.201864Z'
updated: '2026-09-12T03:37:37.513920Z'
source: https://llm-d.ai/docs/architecture
source_domain: llm-d.ai
fetched_at: '2026-09-11T19:42:59.182727Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'llm-d top-level architecture overview (v0.9). Defines InferencePool as a
  label-selector grouping of Model Server pods for the same base model, serving as
  the router''s discovery target, with ''Variant'' as a pod-label-based sub-grouping
  (prefill/decode role, cost/perf profile) rather than a dedicated CRD. Lists advanced
  patterns built on the core Router/InferencePool/Model-Server trio: KV-cache management
  (prefix-aware routing, KV-indexer, KV offloading), disaggregated P/D serving, latency-predictor
  sidecar routing, batch inference (Batch Gateway + Async Processor), and autoscaling
  (HPA/KEDA using EPP-exported metrics like queue depth, or the Workload Variant Autoscaler).'
utility_score: 13.0
---

[Skip to main content](https://llm-d.ai/docs/architecture#__docusaurus_skipToContent_fallback)
🎉 **llm-d 0.9 is here!** Hardened for scale with router HA, enhanced autoscaling, end-to-end observability and expanded hardware support. [**Read the release blog →**](https://llm-d.ai/blog/llm-d-v0.9-hardened-for-scale)
Version: v0.9 (latest)
On this page
High-level guide to llm-d architecture. Start here, then dive into specific guides.
## Core Components[​](https://llm-d.ai/docs/architecture#core-components "Direct link to Core Components")
The llm-d architecture is built around three primary concepts: the [Router](https://llm-d.ai/docs/architecture/core/router), the [InferencePool](https://llm-d.ai/docs/architecture/core/inferencepool), and the [Model Server](https://llm-d.ai/docs/architecture/core/model-servers).
  * - The intelligent entry point for inference requests. It provides LLM-aware load balancing, request queuing, and policy enforcement. It is composed of two functional parts:
    * : A high-performance L7 proxy conformant with the [Gateway API Inference Extension (GAIE)](https://gateway-api-inference-extension.sigs.k8s.io/concepts/conformance/) that accepts user requests and consults the EPP via the `ext-proc` protocol to determine the optimal destination.
    * **[Endpoint Picker (EPP)](https://llm-d.ai/docs/architecture/core/router/epp)** : The routing engine that scores and selects model server pods based on real-time metrics, KV-cache affinity, and configured policies.
  * - The API that groups Model Server pods serving the same base model via a label selector. Conceptualized as an "LLM-optimized Service", it serves as the discovery target for the Router. Additionally:
    * **Variant** - A logical sub-grouping of Model Server pods within an InferencePool, expressed through pod labels rather than a dedicated resource. Variants distinguish model servers based on shared characteristics such as serving role (e.g., prefill or decode), cost profile, performance profile (throughput, latency), or other operational attributes.
  * - The inference engine (such as vLLM or SGLang) that executes the model on hardware accelerators (GPUs, TPUs, HPUs).


## Advanced Patterns[​](https://llm-d.ai/docs/architecture#advanced-patterns "Direct link to Advanced Patterns")
llm-d's core design can be extended with optional advanced patterns:
### KV Cache Management[​](https://llm-d.ai/docs/architecture#kv-cache-management "Direct link to KV Cache Management")
llm-d provides a comprehensive ecosystem for managing and reusing the KV cache across the inference pool. This includes:
  * [Prefix-Cache Aware Routing](https://llm-d.ai/docs/architecture/advanced/kv-management/prefix-cache-aware-routing): Heuristic and precise techniques to maximize cache hits.
  * [KV-Cache Indexing](https://llm-d.ai/docs/architecture/advanced/kv-management/kv-indexer): Event-driven tracking of cache state across all model servers.
  * [KV Offloading](https://llm-d.ai/docs/architecture/advanced/kv-management/kv-offloader): Tiered storage hierarchy (CPU, SSD) for extending cache capacity.


See [KV Cache Management](https://llm-d.ai/docs/architecture/advanced/kv-management) for an overview of how these components compose.
### Disaggregated Serving[​](https://llm-d.ai/docs/architecture#disaggregated-serving "Direct link to Disaggregated Serving")
In disaggregated serving, a single inference request is split into multiple phases (e.g., Prefill and Decode) handled by specialized workers. The llm-d Router orchestrates this flow by selecting both a prefill and a decode endpoint and coordinating the KV-cache transfer between them.
See [Disaggregation](https://llm-d.ai/docs/architecture/advanced/disaggregation) for complete details.
### Predicted Latency-Based Routing[​](https://llm-d.ai/docs/architecture#predicted-latency-based-routing "Direct link to Predicted Latency-Based Routing")
The llm-d Router can be extended with "consultant" sidecars that provide advanced signals for routing decisions. The primary implementation is the **Latency Predictor** , which enables routing based on predicted ITL and TTFT.
  * [Latency Predictor](https://llm-d.ai/docs/architecture/advanced/latency-predictor): Trains an XGBoost model online to predict request latency for better endpoint scoring and SLO enforcement.


### Batch Inference[​](https://llm-d.ai/docs/architecture#batch-inference "Direct link to Batch Inference")
Batch and offline inference workloads are handled by two modules that can be deployed independently or together. The Batch Gateway provides an OpenAI-compatible Batch API for job management, while the Async Processor dispatches queued requests with flow-control gating. When composed, the Batch Gateway delegates dispatch to the Async Processor.
See [Batch Inference](https://llm-d.ai/docs/architecture/advanced/batch) for details on the batch inference design.
### Autoscaling[​](https://llm-d.ai/docs/architecture#autoscaling "Direct link to Autoscaling")
llm-d supports proactive, SLO-aware autoscaling through two complementary approaches:
  * **HPA/KEDA** : Standard Kubernetes-native scaling using metrics exported by the EPP (like queue depth).
  * **Workload Variant Autoscaler (WVA)** : Globally optimized scaling that minimizes cost by placing replicas across different variants or across inference pools while meeting latency targets.


See [Autoscaling](https://llm-d.ai/docs/architecture/advanced/autoscaling) for complete details.
  * [Advanced Patterns](https://llm-d.ai/docs/architecture#advanced-patterns)
    * [Disaggregated Serving](https://llm-d.ai/docs/architecture#disaggregated-serving)
    * [Predicted Latency-Based Routing](https://llm-d.ai/docs/architecture#predicted-latency-based-routing)


