---
title: Announcing KServe v0.17 - Production-Ready LLM Serving with LLMInferenceService
  | KServe
id: announcing-kserve-v017-production-ready-llm-serving-with-llminferenceservice-kse
tags:
- llmd-router
- llminferenceservice
- epp-architecture
- kserve
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:45:46.014597Z'
updated: '2026-09-12T03:37:37.613433Z'
source: https://kserve.github.io/website/blog/kserve-0.17-release
source_domain: kserve.github.io
fetched_at: '2026-09-11T19:45:45.993145Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'KServe v0.17 release notes (published March 13, 2026; KServe joined CNCF)
  announcing LLMInferenceService reaching production readiness, ''built on the llm-d
  framework.'' This is the single strongest documented case of a downstream product
  reusing llm-d''s EPP as-is rather than wrapping it: the LLMInferenceService controller
  auto-creates a full stack including InferencePool, InferenceModel, and ''EPP (Endpoint
  Picker Pod)'' -- integrating Gateway Inference Extension (GIE) v1.3.0 and citing
  the llm-d-inference-scheduler repo directly for its plugin architecture. Gives the
  exact EndpointPickerConfig CRD shape used to configure the EPP plugin pipeline (plugins:
  single-profile-handler, prefix-cache-scorer, load-aware-scorer with a ''threshold''
  parameter; schedulingProfiles with weighted pluginRefs, e.g. prefix-cache-scorer
  weight 2.0, load-aware-scorer weight 1.0, max-score-picker) and shows it can be
  supplied inline in the LLMInferenceService spec or referenced from a ConfigMap for
  platform-wide standardization. Documents that KV-cache tracking works via ZMQ events
  (BlockStored, BlockRemoved) building an index mapping {ModelName, BlockHash} ->
  {PodID, DeviceTier}. Also covers disaggregated P/D via NixlConnector/RDMA-RoCE,
  TP/DP/EP parallelism via LeaderWorkerSet, Envoy AI Gateway token-based rate limiting,
  the Workload Variant Autoscaler (WVA, from github.com/llm-d/llm-d-workload-variant-autoscaler)
  with cost-aware variantCost scaling, EPP horizontal scaling/HA support, and the
  new modular Helm chart split (kserve core / llmisvc controller / localmodel) across
  10 independent charts as of this release.'
utility_score: 10.0
---

[Skip to main content](https://kserve.github.io/website/blog/kserve-0.17-release#__docusaurus_skipToContent_fallback)
🎉️ **[KServe has joined CNCF!](https://www.cncf.io/blog/2025/11/11/kserve-becomes-a-cncf-incubating-project/)** 🥳️
_Published on March 13, 2026_
We are excited to announce the release of **KServe v0.17** , a landmark release that brings **LLMInferenceService** to production readiness with a GenAI-first architecture built on the [llm-d](https://github.com/llm-d/llm-d) framework. This release introduces KV-cache aware intelligent routing, disaggregated prefill-decode, distributed inference with tensor/data/expert parallelism, Envoy AI Gateway integration with token-based rate limiting, and a completely restructured modular Helm chart architecture.
## 🤖 LLMInferenceService: GenAI-First Architecture[​](https://kserve.github.io/website/blog/kserve-0.17-release#-llminferenceservice-genai-first-architecture "Direct link to 🤖 LLMInferenceService: GenAI-First Architecture")
KServe v0.17 elevates **LLMInferenceService** from an experimental feature to a production-ready CRD purpose-built for generative AI workloads. Built on the [llm-d](https://github.com/llm-d/llm-d) framework, LLMInferenceService provides a GenAI-first architecture that goes beyond traditional InferenceService to address the unique challenges of serving large language models at scale.
Unlike InferenceService which is designed for predictive AI workloads, LLMInferenceService natively supports:
  * **Distributed inference** across multiple nodes and GPUs
  * **KV-cache aware scheduling** for intelligent request routing
  * **Disaggregated prefill-decode** for optimal resource utilization
  * **Gateway Inference Extension (GIE)** integration for advanced traffic management
  * **Token-based rate limiting** via Envoy AI Gateway

  
| Feature  | InferenceService  | LLMInferenceService  |  
| --- | --- | --- |  
| **Primary Use Case**  | Predictive AI  | Generative AI  |  
| **Routing**  | Standard Gateway  | KV-cache aware with EPP  |  
| **Parallelism**  | Worker Spec  | TP, DP, EP native support  |  
| **Prefill-Decode**  | N/A  | Disaggregated separation  |  
| **Scaling**  | HPA/KPA  | WVA + KEDA  |  

```
apiVersion: serving.kserve.io/v1alpha2kind: LLMInferenceServicemetadata:name: llama3-servinguri: hf://meta-llama/Llama-3.1-8B-Instructname: meta-llama--Llama-3.1-8B-Instructreplicas:3template:containers:-name: vllmresources:limits:nvidia.com/gpu:"1"router:gateway:managed:{}httpRoute:{}scheduler:
```

This creates a full serving stack including the Deployment, Service, Gateway, HTTPRoute, **InferencePool** , **InferenceModel** , and **EPP (Endpoint Picker Pod)** — all managed by the LLMInferenceService controller.
## 🚀 Key LLMInferenceService Features in v0.17[​](https://kserve.github.io/website/blog/kserve-0.17-release#-key-llminferenceservice-features-in-v017 "Direct link to 🚀 Key LLMInferenceService Features in v0.17")
### 🧠 KV-Cache Aware Scheduling with Gateway Inference Extension[​](https://kserve.github.io/website/blog/kserve-0.17-release#-kv-cache-aware-scheduling-with-gateway-inference-extension "Direct link to 🧠 KV-Cache Aware Scheduling with Gateway Inference Extension")
LLMInferenceService integrates with [**Gateway Inference Extension (GIE) v1.3.0**](https://gateway-api-inference-extension.sigs.k8s.io/), a Kubernetes SIG project that extends the Gateway API with AI-specific routing capabilities. At the heart of this integration is the **Endpoint Picker Pod (EPP)** from the [llm-d inference scheduler](https://github.com/llm-d/llm-d-inference-scheduler), an intelligent scheduler that routes requests based on real-time KV-cache state rather than simple round-robin or random load balancing.
Traditional load balancing treats all LLM inference requests equally, but in practice, requests with similar prompts benefit enormously from being routed to the same pod — because that pod already has the relevant KV cache blocks loaded. The EPP solves this by tracking real-time KV cache states across all vLLM instances via ZMQ events (`BlockStored`, `BlockRemoved`) and building an index mapping `{ModelName, BlockHash}` → `{PodID, DeviceTier}`.
The scheduling behavior is configured through `EndpointPickerConfig`, which defines a plugin pipeline with weighted scorers:

```
apiVersion: inference.networking.x-k8s.io/v1alpha1kind: EndpointPickerConfigplugins:-type: single-profile-handler-type: prefix-cache-scorer-type: load-aware-scorerparameters:threshold:100-type: max-score-pickerschedulingProfiles:-name: defaultplugins:-pluginRef: prefix-cache-scorerweight:2.0-pluginRef: load-aware-scorerweight:1.0-pluginRef: max-score-picker
```

The pipeline uses three types of plugins (see [llm-d scheduler architecture](https://github.com/llm-d/llm-d-inference-scheduler/blob/main/docs/architecture.md) for details):
  * **prefix-cache-scorer** (weight: 2.0): Tracks the actual KV cache contents across all vLLM instances and scores pods based on how many cached prefix blocks match the incoming request's prompt. This reduces Time To First Token (TTFT) by avoiding redundant prefill computation for repeated or similar prompts — particularly beneficial for multi-turn conversations and RAG workloads.
  * **load-aware-scorer** (weight: 1.0): Scores candidate pods based on their current queue depth. Pods with empty queues score 0.5, while pods with growing queues score progressively lower toward 0. The `threshold` parameter controls the sensitivity — when queue depth exceeds the threshold, the pod scores near zero.
  * **max-score-picker** : After all scorers run, selects the pod with the highest weighted aggregate score.


The `EndpointPickerConfig` can be provided inline in the LLMInferenceService spec or referenced from a ConfigMap, giving platform teams the flexibility to standardize scheduling behavior across deployments:

```
apiVersion: serving.kserve.io/v1alpha2kind: LLMInferenceServicemetadata:name: llama3-with-scheduleruri: hf://meta-llama/Llama-3.1-8B-Instructname: meta-llama--Llama-3.1-8B-Instructreplicas:4template:containers:-name: vllmresources:limits:nvidia.com/gpu:"1"router:gateway:managed:{}httpRoute:{}scheduler:config:name: custom-endpoint-picker-configkey: endpoint-picker-config.yaml
```

The GIE CRDs (**InferencePool** and **InferenceModel**) are now bundled as part of the KServe installation, simplifying setup.
### 🔀 Disaggregated Prefill-Decode[​](https://kserve.github.io/website/blog/kserve-0.17-release#-disaggregated-prefill-decode "Direct link to 🔀 Disaggregated Prefill-Decode")
LLMInferenceService natively supports **disaggregated prefill-decode** , which separates the compute-intensive prefill phase from the memory-intensive decode phase into independent workloads. This allows each phase to be scaled and optimized independently.

```
apiVersion: serving.kserve.io/v1alpha2kind: LLMInferenceServicemetadata:name: llama3-prefill-decodeuri: hf://meta-llama/Llama-3.1-8B-Instructname: meta-llama--Llama-3.1-8B-Instructreplicas:2template:containers:-name: vllmresources:limits:nvidia.com/gpu:"1"prefill:replicas:2template:containers:-name: vllmresources:limits:nvidia.com/gpu:"1"router:gateway:managed:{}httpRoute:{}scheduler:
```

KV cache data is transferred between prefill and decode pods using **NixlConnector** with RDMA-based RoCE for high-throughput, low-latency block transfers.
### 📐 Distributed Inference: Tensor, Data, and Expert Parallelism[​](https://kserve.github.io/website/blog/kserve-0.17-release#-distributed-inference-tensor-data-and-expert-parallelism "Direct link to 📐 Distributed Inference: Tensor, Data, and Expert Parallelism")
LLMInferenceService introduces a comprehensive **parallelism specification** for distributed inference across multiple nodes and GPUs using [LeaderWorkerSet](https://github.com/kubernetes-sigs/lws):
  * **Tensor Parallelism (TP)** : Splits model layers across GPUs within a node
  * **Data Parallelism (DP)** : Runs multiple model replicas for higher throughput
  * **Data-Local Parallelism** : Controls GPUs per node for optimal NUMA affinity
  * **Expert Parallelism (EP)** : Distributes Mixture-of-Experts (MoE) model experts across GPUs



```
apiVersion: serving.kserve.io/v1alpha2kind: LLMInferenceServicemetadata:name: llama3-multi-nodeuri: hf://meta-llama/Llama-3.1-70B-Instructname: meta-llama--Llama-3.1-70B-Instructreplicas:8parallelism:tensor:4dataLocal:4template:containers:-name: vllmresources:limits:nvidia.com/gpu:"4"worker:containers:-name: vllmresources:limits:nvidia.com/gpu:"4"router:gateway:managed:{}httpRoute:{}scheduler:
```

### 🌐 Envoy AI Gateway Integration with Token-Based Rate Limiting[​](https://kserve.github.io/website/blog/kserve-0.17-release#-envoy-ai-gateway-integration-with-token-based-rate-limiting "Direct link to 🌐 Envoy AI Gateway Integration with Token-Based Rate Limiting")
LLMInferenceService integrates with [**Envoy AI Gateway**](https://aigateway.envoyproxy.io) for AI-native traffic management. This enables **token-based rate limiting** — a capability critical for LLM serving where request cost varies dramatically based on input and output token counts rather than simple request counts.

```
apiVersion: aigateway.envoyproxy.io/v1alpha1kind: AIGatewayRoutemetadata:name: llm-routetargetRefs:-group: gateway.networking.k8s.iokind: HTTPRoutename: llama3-servingllmRequestCosts:-metadataKey: llm_input_tokentype: InputToken-metadataKey: llm_output_tokentype: OutputToken-metadataKey: llm_total_tokentype: TotalTokenapiVersion: gateway.envoyproxy.io/v1alpha1kind: BackendTrafficPolicymetadata:name: llm-rate-limittargetRefs:-group: aigateway.envoyproxy.iokind: AIGatewayRoutename: llm-routerateLimit:type: Globalglobal:-clientSelectors:-headers:-name: x-user-idtype: Distinctrequests:1000unit: Hourrequest:from: Numbernumber:0response:from: Metadatakey: llm_total_token
```

### ⚡ Autoscaling API with WVA Support[​](https://kserve.github.io/website/blog/kserve-0.17-release#-autoscaling-api-with-wva-support "Direct link to ⚡ Autoscaling API with WVA Support")
A new **autoscaling API** has been added to LLMInferenceService with support for the [**Workload Variant Autoscaler (WVA)**](https://github.com/llm-d/llm-d-workload-variant-autoscaler), a Kubernetes-based global autoscaler designed specifically for LLM inference workloads. Traditional CPU/memory-based autoscaling is inadequate for LLMs because inference cost is driven by token throughput, KV cache utilization, and queue depth rather than CPU or memory usage.
WVA continuously monitors inference server metrics via Prometheus — specifically **KV cache utilization** and **queue depth** — to determine when servers are approaching saturation. It then computes a `wva_desired_replicas` metric and emits it to Prometheus, where an actuator backend (**HPA** or **KEDA**) reads it to drive the actual scaling:
  * **WVA + KEDA** : Queries Prometheus directly for the `wva_desired_replicas` metric. Does not require Prometheus Adapter. Supports idle scale-to-zero via `idleReplicaCount`.
  * **WVA + HPA** : Reads the `wva_desired_replicas` metric via Kubernetes Metrics API. Requires Prometheus Adapter. Supports standard HPA scaling behaviors.


A key concept in WVA is the **variant** — a specific deployment configuration (hardware, runtime, parallelism strategy) for serving a model. The same base model might be served by multiple variants: for example, Llama-3 on A100 GPUs with TP=4 is one variant, while Llama-3 on H100 GPUs with TP=2 is another. The `variantCost` field specifies the relative cost per replica for each variant, enabling WVA to make **cost-aware scaling decisions** across variants — scaling up the cheaper variant first when demand increases, and scaling down the most expensive variant first when demand decreases.

```
apiVersion: serving.kserve.io/v1alpha2kind: LLMInferenceServicemetadata:name: llama3-wva-autoscalinguri: hf://meta-llama/Llama-3.1-8B-Instructname: meta-llama--Llama-3.1-8B-Instructscaling:minReplicas:1maxReplicas:10variantCost:"15.0"pollingInterval:30cooldownPeriod:300initialCooldownPeriod:120idleReplicaCount:0fallback:failureThreshold:3replicas:2template:containers:-name: vllmresources:limits:nvidia.com/gpu:"1"router:gateway:managed:{}httpRoute:{}scheduler:
```

In the example above, `variantCost: "15.0"` indicates the relative cost of running each replica of this variant. If another variant of the same model has `variantCost: "5.0"`, WVA would prefer to add capacity on that cheaper variant before scaling up this one. The default value is `"10.0"` if not specified. When using the KEDA backend, the `fallback` field ensures the deployment maintains a minimum replica count (here, 2 replicas) even if the metrics pipeline fails — a critical safety net for production LLM deployments.
### 🔧 Scheduler High Availability[​](https://kserve.github.io/website/blog/kserve-0.17-release#-scheduler-high-availability "Direct link to 🔧 Scheduler High Availability")
The LLMInferenceService scheduler (EPP) now supports **scaling and high availability** , allowing multiple EPP replicas for production deployments that require fault tolerance and higher routing throughput.
### 🛡️ CRD Webhook Validation[​](https://kserve.github.io/website/blog/kserve-0.17-release#%EF%B8%8F-crd-webhook-validation "Direct link to 🛡️ CRD Webhook Validation")
LLMInferenceService now includes **CRD webhook validation** with comprehensive E2E tests, providing early feedback on invalid configurations before they reach the controller. This catches errors in parallelism settings, workload specifications, and router configurations at admission time.
### 📋 Configuration Composition with LLMInferenceServiceConfig[​](https://kserve.github.io/website/blog/kserve-0.17-release#-configuration-composition-with-llminferenceserviceconfig "Direct link to 📋 Configuration Composition with LLMInferenceServiceConfig")
LLMInferenceService supports a **configuration composition model** through LLMInferenceServiceConfig, enabling reusable templates that can be shared across multiple LLMInferenceService resources. The merge order follows:
  1. **Well-Known Configs** → 2. **Explicit BaseRefs** → 3. **LLMInferenceService Spec**


This allows platform teams to define standardized vLLM worker templates, router/scheduler configurations, and resource defaults while giving application teams the ability to override specific settings.
### 📦 Additional LLMInferenceService Improvements[​](https://kserve.github.io/website/blog/kserve-0.17-release#-additional-llminferenceservice-improvements "Direct link to 📦 Additional LLMInferenceService Improvements")
  * **Label and annotation propagation** to downstream workload resources ([#5009](https://github.com/kserve/kserve/pull/5009))
  * **Prometheus annotation propagation** to workloads for metrics collection ([#5086](https://github.com/kserve/kserve/pull/5086))
  * **Certificate management** with DNS/IP SAN and automatic renewal for self-signed certs ([#5099](https://github.com/kserve/kserve/pull/5099))
  * **Improved CA bundle management** for secure communication ([#4803](https://github.com/kserve/kserve/pull/4803))
  * **Optional storageInitializer** — skip model download when using pre-loaded models ([#4970](https://github.com/kserve/kserve/pull/4970))
  * **InferencePool auto-migration** for seamless upgrades ([#5007](https://github.com/kserve/kserve/pull/5007))
  * **Route-only completions through InferencePool** for chat/completion endpoints ([#5087](https://github.com/kserve/kserve/pull/5087))
  * **Startup probes for vLLM containers** for more reliable health monitoring ([#5063](https://github.com/kserve/kserve/pull/5063))
  * **vLLM arguments migrated to command field** for cleaner configuration ([#5049](https://github.com/kserve/kserve/pull/5049))
  * **Versioned well-known config resolution** for stable config management ([#5096](https://github.com/kserve/kserve/pull/5096))
  * **Scheduler config via ConfigMap or inline** for flexible configuration ([#4856](https://github.com/kserve/kserve/pull/4856))
  * **Pod init container failure monitoring** for better observability ([#5034](https://github.com/kserve/kserve/pull/5034))
  * **Preserve externally managed replicas** during reconciliation ([#4996](https://github.com/kserve/kserve/pull/4996))
  * **Allow stopping LLMInferenceService** gracefully ([#4839](https://github.com/kserve/kserve/pull/4839))
  * **Enhanced Gateway API URL discovery** with listener hostname fallback ([#5104](https://github.com/kserve/kserve/pull/5104), [#5079](https://github.com/kserve/kserve/pull/5079))


## 🏗️ Modular Component Architecture[​](https://kserve.github.io/website/blog/kserve-0.17-release#%EF%B8%8F-modular-component-architecture "Direct link to 🏗️ Modular Component Architecture")
KServe v0.17 introduces a fundamental architectural shift toward **modular, component-based deployment**. KServe now consists of three independent components:
  * **kserve** (core): Manages InferenceService, ServingRuntime, ClusterServingRuntime, InferenceGraph, and TrainedModel CRDs.
  * **llmisvc** : The LLMInferenceService controller for generative AI workloads, managing LLMInferenceService and LLMInferenceServiceConfig CRDs.
  * **localmodel** (optional): The LocalModel controller for efficient model caching with LocalModelCache, LocalModelNode, and LocalModelNodeGroup CRDs.

  
| Combination  | Use Case  | Components  |  
| --- | --- | --- |  
| **KServe Only**  | Predictive AI  | kserve  |  
| **KServe + LLMIsvc**  | Predictive AI + Generative AI  | kserve + llmisvc  |  
| **Full Stack**  | Predictive AI + Generative AI + Model Caching  | kserve + llmisvc + localmodel  |  
### Helm Chart Restructuring[​](https://kserve.github.io/website/blog/kserve-0.17-release#helm-chart-restructuring "Direct link to Helm Chart Restructuring")
To support the new component architecture, the **Helm charts have been completely restructured** from a single chart into **10 independent Helm charts** :
**CRD Charts** (6 charts with full and minimal variants):
  * `kserve-crd` / `kserve-crd-minimal`
  * `kserve-llmisvc-crd` / `kserve-llmisvc-crd-minimal`
  * `kserve-localmodel-crd` / `kserve-localmodel-crd-minimal`


**Resource Charts** (4 charts):
  * `kserve-resources` (renamed from `kserve`)
  * `kserve-llmisvc-resources` (new)
  * `kserve-localmodel-resources` (new)
  * `kserve-runtime-configs` (new — manages ClusterServingRuntimes and LLMIsvcConfigs)


warning
This is a **breaking change**. Users upgrading from v0.16 **cannot** use a simple `helm upgrade` command. Please follow the detailed [upgrade guide](https://kserve.github.io/website/docs/install/upgrade-guide) for step-by-step migration instructions. We strongly recommend testing the upgrade in a non-production environment first.
For fresh installations, the new Kustomize component-based architecture also provides composable deployment options via standalone overlays, addon overlays, and all-in-one overlays. See the [installation concepts](https://kserve.github.io/website/docs/install/overview) for details.
## 🔧 InferenceService and Platform Improvements[​](https://kserve.github.io/website/blog/kserve-0.17-release#-inferenceservice-and-platform-improvements "Direct link to 🔧 InferenceService and Platform Improvements")
### Storage Performance[​](https://kserve.github.io/website/blog/kserve-0.17-release#storage-performance "Direct link to Storage Performance")
  * **Parallelized blob downloads** from Azure and S3 for faster model loading ([#4709](https://github.com/kserve/kserve/pull/4709), [#4714](https://github.com/kserve/kserve/pull/4714))
  * **Faster parallel S3 downloads** with configurable file selection ([#5102](https://github.com/kserve/kserve/pull/5102), [#5119](https://github.com/kserve/kserve/pull/5119))
  * **Git repository support** for downloading models directly from Git repos via HTTPS ([#4966](https://github.com/kserve/kserve/pull/4966))


### New Serving Runtimes[​](https://kserve.github.io/website/blog/kserve-0.17-release#new-serving-runtimes "Direct link to New Serving Runtimes")
  * **OpenVINO Model Server** — Intel's optimized inference runtime for high-performance serving on Intel hardware ([#4592](https://github.com/kserve/kserve/pull/4592))
  * **PredictiveServer** runtime with full build/publish infrastructure and E2E testing ([#4954](https://github.com/kserve/kserve/pull/4954))


### Gateway & Routing[​](https://kserve.github.io/website/blog/kserve-0.17-release#gateway--routing "Direct link to Gateway & Routing")
  * **Gateway API upgraded to v1.4.0** ([#5038](https://github.com/kserve/kserve/pull/5038))
  * **PathTemplate configuration** for flexible inference service routing ([#4817](https://github.com/kserve/kserve/pull/4817))


### vLLM Backend[​](https://kserve.github.io/website/blog/kserve-0.17-release#vllm-backend "Direct link to vLLM Backend")
  * **Upgraded to vLLM v0.15.1** with performance improvements ([#5098](https://github.com/kserve/kserve/pull/5098))
  * **Removed Python 3.9 support** ([#4851](https://github.com/kserve/kserve/pull/4851))


### Additional Enhancements[​](https://kserve.github.io/website/blog/kserve-0.17-release#additional-enhancements "Direct link to Additional Enhancements")
  * **CSV and Parquet marshallers** for expanded data format support ([#5115](https://github.com/kserve/kserve/pull/5115))
  * **Event loop configuration** with new `--event_loop` flag supporting `auto`, `asyncio`, and `uvloop` ([#4971](https://github.com/kserve/kserve/pull/4971))
  * **Annotation-based runtime defaults** for MLServer ([#5064](https://github.com/kserve/kserve/pull/5064))
  * **`INFERENCE_SERVICE_NAME`environment variable** exposed to serving containers ([#5013](https://github.com/kserve/kserve/pull/5013))
  * **Failure condition surfacing** in InferenceService status ([#5114](https://github.com/kserve/kserve/pull/5114))
  * **Inference log batching** with external marshalling support ([#5061](https://github.com/kserve/kserve/pull/5061))


### Infrastructure Updates[​](https://kserve.github.io/website/blog/kserve-0.17-release#infrastructure-updates "Direct link to Infrastructure Updates")
  * Kubernetes packages bumped to **v0.34.0**
  * Knative Serving updated to **v1.21.1**
  * Go updated to **1.25**
  * Kubebuilder updated to **1.9.0**
  * KEDA bumped from 2.16.1 to **2.17.3**
  * MinIO replaced with **SeaweedFS** for testing infrastructure


## 🔒 Security Fixes[​](https://kserve.github.io/website/blog/kserve-0.17-release#-security-fixes "Direct link to 🔒 Security Fixes")
Multiple security vulnerabilities have been addressed:
  * CVE-2025-62727 (Starlette)
  * CVE-2025-22872, CVE-2025-47914, CVE-2025-58181
  * CVE-2024-43598 (LightGBM updated to 4.6.0)
  * CVE-2025-43859 (h11 HTTP parsing)
  * CVE-2025-66418 (decompression chain)
  * CVE-2025-68156 (expr-lang/expr)
  * CVE-2026-26007 (cryptography subgroup attack)
  * CVE-2026-24486 (python-multipart arbitrary file write)
  * Path traversal vulnerabilities in https.go and tar extraction


## 🔍 Release Notes[​](https://kserve.github.io/website/blog/kserve-0.17-release#-release-notes "Direct link to 🔍 Release Notes")
For the complete list of all 167 merged pull requests, bug fixes, and known issues, visit the GitHub release pages:


## 🙏 Acknowledgments[​](https://kserve.github.io/website/blog/kserve-0.17-release#-acknowledgments "Direct link to 🙏 Acknowledgments")
We extend our gratitude to all **38+ contributors** who made this release possible, including **21 first-time contributors**. Your efforts continue to drive the advancement of KServe as a leading platform for serving machine learning models.
  * **Core Contributors** : The KServe maintainers and regular contributors
  * **Community** : Everyone who reported issues, provided feedback, and tested features
  * **New Contributors** : Welcome to all first-time contributors who helped shape this release


## 🤝 Join the Community[​](https://kserve.github.io/website/blog/kserve-0.17-release#-join-the-community "Direct link to 🤝 Join the Community")
We invite you to explore the new features in KServe v0.17 and contribute to the ongoing development of the project:
  * Visit our [Website](https://kserve.github.io/website/) or [GitHub](https://github.com/kserve)
  * Join the Slack ([#kserve](https://github.com/kserve/community?tab=readme-ov-file#questions-and-issues))
  * Attend our community meeting by subscribing to the [KServe calendar](https://zoom-lfx.platform.linuxfoundation.org/meetings/kserve?view=month).
  * View our [community github repository](https://github.com/kserve/community) to learn how to make contributions. We are excited to work with you to make KServe better and promote its adoption!


**Happy serving!**
_The KServe team is committed to making machine learning model serving simple, scalable, and standardized. Thank you for being part of our community!_
  * [🤖 LLMInferenceService: GenAI-First Architecture](https://kserve.github.io/website/blog/kserve-0.17-release#-llminferenceservice-genai-first-architecture)
  * [🚀 Key LLMInferenceService Features in v0.17](https://kserve.github.io/website/blog/kserve-0.17-release#-key-llminferenceservice-features-in-v017)
    * [🧠 KV-Cache Aware Scheduling with Gateway Inference Extension](https://kserve.github.io/website/blog/kserve-0.17-release#-kv-cache-aware-scheduling-with-gateway-inference-extension)
    * [🔀 Disaggregated Prefill-Decode](https://kserve.github.io/website/blog/kserve-0.17-release#-disaggregated-prefill-decode)
    * [📐 Distributed Inference: Tensor, Data, and Expert Parallelism](https://kserve.github.io/website/blog/kserve-0.17-release#-distributed-inference-tensor-data-and-expert-parallelism)
    * [🌐 Envoy AI Gateway Integration with Token-Based Rate Limiting](https://kserve.github.io/website/blog/kserve-0.17-release#-envoy-ai-gateway-integration-with-token-based-rate-limiting)
    * [⚡ Autoscaling API with WVA Support](https://kserve.github.io/website/blog/kserve-0.17-release#-autoscaling-api-with-wva-support)
    * [🔧 Scheduler High Availability](https://kserve.github.io/website/blog/kserve-0.17-release#-scheduler-high-availability)
    * [🛡️ CRD Webhook Validation](https://kserve.github.io/website/blog/kserve-0.17-release#%EF%B8%8F-crd-webhook-validation)
    * [📋 Configuration Composition with LLMInferenceServiceConfig](https://kserve.github.io/website/blog/kserve-0.17-release#-configuration-composition-with-llminferenceserviceconfig)
    * [📦 Additional LLMInferenceService Improvements](https://kserve.github.io/website/blog/kserve-0.17-release#-additional-llminferenceservice-improvements)
  * [🏗️ Modular Component Architecture](https://kserve.github.io/website/blog/kserve-0.17-release#%EF%B8%8F-modular-component-architecture)
    * [Helm Chart Restructuring](https://kserve.github.io/website/blog/kserve-0.17-release#helm-chart-restructuring)
  * [🔧 InferenceService and Platform Improvements](https://kserve.github.io/website/blog/kserve-0.17-release#-inferenceservice-and-platform-improvements)
    * [New Serving Runtimes](https://kserve.github.io/website/blog/kserve-0.17-release#new-serving-runtimes)
    * [Additional Enhancements](https://kserve.github.io/website/blog/kserve-0.17-release#additional-enhancements)
    * [Infrastructure Updates](https://kserve.github.io/website/blog/kserve-0.17-release#infrastructure-updates)
  * [🤝 Join the Community](https://kserve.github.io/website/blog/kserve-0.17-release#-join-the-community)


