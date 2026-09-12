---
title: llm-d/llm-d-router | DeepWiki
id: llm-dllm-d-router-deepwiki
tags:
- llmd-router-nonk8s-backends-54b774
- source-hub
- low-authority
- epp-architecture
- standalone-mode
created: '2026-09-11T20:49:41.679113Z'
updated: '2026-09-12T03:37:37.703178Z'
source: https://deepwiki.com/llm-d/llm-d-router
source_domain: deepwiki.com
fetched_at: '2026-09-11T20:49:41.656840Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'LOW-AUTHORITY AI-generated navigation index (DeepWiki, indexed 27 Jul 2026
  at commit 99298ae8) of the llm-d-router repo -- used here only to locate primary
  source paths, not quoted as authoritative prose. Confirms two deployment modes explicitly
  relevant to the non-k8s question (README.md33-42): (1) Standalone Mode -- a self-managed
  Envoy proxy runs alongside the EPP in the same pod/process, described as "ideal
  for environments without full Gateway API infrastructure"; (2) Gateway Mode -- production-grade,
  EPP is a backend for an InferencePool referenced by an HTTPRoute on a shared Gateway,
  for multi-cluster load balancing. Lists the four architectural pillars with concrete
  code entities: Endpoint Picker/EPP (StreamingServer, Director, InferencePoolReconciler
  in pkg/epp/server, pkg/epp/sched/director.go, pkg/epp/framework/runtime/scheduler.go,
  pkg/epp/datastore/datastore.go), Disaggregation Sidecar/pd-sidecar (ChatCompletionsHandler,
  NIXLConnector, SGLangConnector, cmd/pd-sidecar/main.go), Coordinator (multimodal
  E/P/D orchestration: ReplaceMediaURLsStep, PrefillStep, DecodeStep), and Request
  Management CRDs (InferenceObjective, InferenceModelRewrite). States the EPP-Envoy
  contract is gRPC ext_proc with FULL_DUPLEX_STREAMED as the only supported mode.
  Disaggregation topologies confirmed: P/D, E/PD, and full E/P/D, coordinated via
  the disagg-profile-handler plugin (pkg/epp/framework/plugins/scheduling/profile/disagg/disagg_profile_handler.go)
  and pd-sidecar, using an x-prefiller-host-port header to route the remote-prefill
  leg. Navigation-only value: points to child pages "Kubernetes Controllers and Datastore"
  (2.3), "KV Block Index and Storage Backends" (9.1), and "InferencePool and Standalone
  Helm Charts" (10.1) as the deepest primary-source paths for datastore/discovery
  internals -- confirmed via vault search that the underlying discovery.go interface,
  FileDiscovery plugin, and no-kubernetes-deployment.md guide are already captured
  by other fetchers in this run, so not re-fetched here to avoid duplication.'
utility_score: 9.0
---

Index your code with Devin
[DeepWiki](https://deepwiki.com/)
[DeepWiki](https://deepwiki.com/)
[llm-d/llm-d-router ](https://github.com/llm-d/llm-d-router "Open repository")
Index your code with
Devin
Edit WikiShare
Last indexed: 27 July 2026 ([99298a](https://github.com/llm-d/llm-d-router/commits/99298ae8))
  * [Architecture and Core Concepts](https://deepwiki.com/llm-d/llm-d-router/1.1-architecture-and-core-concepts)
  * [Getting Started and Development Environment](https://deepwiki.com/llm-d/llm-d-router/1.2-getting-started-and-development-environment)
  * [Request Handling Pipeline (StreamingServer and Director)](https://deepwiki.com/llm-d/llm-d-router/2.2-request-handling-pipeline-\(streamingserver-and-director\))
  * [Kubernetes Controllers and Datastore](https://deepwiki.com/llm-d/llm-d-router/2.3-kubernetes-controllers-and-datastore)
  * [Configuration Loading and Plugin Registry](https://deepwiki.com/llm-d/llm-d-router/3.4-configuration-loading-and-plugin-registry)
  * [Datalayer Runtime and Metrics Extraction](https://deepwiki.com/llm-d/llm-d-router/4.2-datalayer-runtime-and-metrics-extraction)
  * [Flow Controller and Shard Processor](https://deepwiki.com/llm-d/llm-d-router/5.1-flow-controller-and-shard-processor)
  * [Fairness, Ordering, and Eviction Plugins](https://deepwiki.com/llm-d/llm-d-router/5.2-fairness-ordering-and-eviction-plugins)
  * [Disaggregation Sidecar (pd-sidecar)](https://deepwiki.com/llm-d/llm-d-router/6-disaggregation-sidecar-\(pd-sidecar\))
  * [Proxy and Connector Architecture](https://deepwiki.com/llm-d/llm-d-router/6.1-proxy-and-connector-architecture)
  * [Coordinator Pipeline and Steps](https://deepwiki.com/llm-d/llm-d-router/7.1-coordinator-pipeline-and-steps)
  * [Coordinator Server and Connectors](https://deepwiki.com/llm-d/llm-d-router/7.2-coordinator-server-and-connectors)
  * [Request Attribute Reporting and Predicted Latency](https://deepwiki.com/llm-d/llm-d-router/8.2-request-attribute-reporting-and-predicted-latency)
  * [KV Block Index and Storage Backends](https://deepwiki.com/llm-d/llm-d-router/9.1-kv-block-index-and-storage-backends)
  * [InferencePool and Standalone Helm Charts](https://deepwiki.com/llm-d/llm-d-router/10.1-inferencepool-and-standalone-helm-charts)
  * [Kustomize Components and Environment Overlays](https://deepwiki.com/llm-d/llm-d-router/10.2-kustomize-components-and-environment-overlays)
  * [Custom Resource Definitions (CRDs)](https://deepwiki.com/llm-d/llm-d-router/11.1-custom-resource-definitions-\(crds\))
  * [EPP End-to-End and Integration Tests](https://deepwiki.com/llm-d/llm-d-router/12.1-epp-end-to-end-and-integration-tests)
  * [Sidecar Tests and Test Utilities](https://deepwiki.com/llm-d/llm-d-router/12.2-sidecar-tests-and-test-utilities)
  * [Coordinator Tests and Performance Testing](https://deepwiki.com/llm-d/llm-d-router/12.3-coordinator-tests-and-performance-testing)
  * [CI/CD and Repository Governance](https://deepwiki.com/llm-d/llm-d-router/13-cicd-and-repository-governance)
  * [Container Build and Release Pipeline](https://deepwiki.com/llm-d/llm-d-router/13.1-container-build-and-release-pipeline)
  * [PR Governance and Code Quality](https://deepwiki.com/llm-d/llm-d-router/13.2-pr-governance-and-code-quality)


# Overview
Relevant source files
  * [.github/workflows/ci-pr-checks.yaml](https://github.com/llm-d/llm-d-router/blob/99298ae8/.github/workflows/ci-pr-checks.yaml)
  * [config/manifests/vllm/sim-deployment.yaml](https://github.com/llm-d/llm-d-router/blob/99298ae8/config/manifests/vllm/sim-deployment.yaml)
  * [config/manifests/vllm/sim-grpc-deployment.yaml](https://github.com/llm-d/llm-d-router/blob/99298ae8/config/manifests/vllm/sim-grpc-deployment.yaml)
  * [deploy/config/dp-epp-config.yaml](https://github.com/llm-d/llm-d-router/blob/99298ae8/deploy/config/dp-epp-config.yaml)
  * [deploy/config/epp-precise-prefix-cache-config.yaml](https://github.com/llm-d/llm-d-router/blob/99298ae8/deploy/config/epp-precise-prefix-cache-config.yaml)
  * [deploy/config/sim-epp-kvcache-config.yaml](https://github.com/llm-d/llm-d-router/blob/99298ae8/deploy/config/sim-epp-kvcache-config.yaml)
  * [deploy/config/sim-epp-no-hit-lru.yaml](https://github.com/llm-d/llm-d-router/blob/99298ae8/deploy/config/sim-epp-no-hit-lru.yaml)
  * [deploy/config/sim-epp-tokenizer-vllm-http-config.yaml](https://github.com/llm-d/llm-d-router/blob/99298ae8/deploy/config/sim-epp-tokenizer-vllm-http-config.yaml)
  * [deploy/environments/dev/README.md](https://github.com/llm-d/llm-d-router/blob/99298ae8/deploy/environments/dev/README.md?plain=1)
  * [deploy/environments/dev/kubernetes-kgateway/patch-deployments.yaml](https://github.com/llm-d/llm-d-router/blob/99298ae8/deploy/environments/dev/kubernetes-kgateway/patch-deployments.yaml)
  * [pkg/epp/framework/plugins/requesthandling/parsers/passthrough/README.md](https://github.com/llm-d/llm-d-router/blob/99298ae8/pkg/epp/framework/plugins/requesthandling/parsers/passthrough/README.md?plain=1)
  * [pkg/epp/framework/plugins/scheduling/scorer/nohitlru/README.md](https://github.com/llm-d/llm-d-router/blob/99298ae8/pkg/epp/framework/plugins/scheduling/scorer/nohitlru/README.md?plain=1)
  * [test/testdata/sim-deployment.yaml](https://github.com/llm-d/llm-d-router/blob/99298ae8/test/testdata/sim-deployment.yaml)


The **llm-d-router** is a high-performance, intelligent entry point for Large Language Model (LLM) inference traffic. It provides load-aware and prefix-cache-aware routing, request prioritization, and advanced flow control to satisfy complex serving objectives [README.md14-17](https://github.com/llm-d/llm-d-router/blob/99298ae8/README.md?plain=1#L14-L17)
By integrating with the Kubernetes-native **Gateway API Inference Extension (GIE)** , the router enables scalable request scheduling across heterogeneous model-serving pods [docs/architecture.md23-26](https://github.com/llm-d/llm-d-router/blob/99298ae8/docs/architecture.md?plain=1#L23-L26) It serves as the "brain" of the inference data plane, injecting real-time signals into L7 proxies like Envoy [README.md17-18](https://github.com/llm-d/llm-d-router/blob/99298ae8/README.md?plain=1#L17-L18)
## Core Components
The system is composed of four primary architectural pillars:  
| Component  | Description  | Key Code Entities  |  
| --- | --- | --- |  
| **Endpoint Picker (EPP)**  | The intelligent routing engine. A gRPC server implementing the `ext_proc` protocol to make placement decisions based on real-time cluster state [README.md26-27](https://github.com/llm-d/llm-d-router/blob/99298ae8/README.md?plain=1#L26-L27)  |  `StreamingServer`, `Director`, `InferencePoolReconciler`  |  
| **Disaggregation Sidecar**  | A coordination proxy (`pd-sidecar`) deployed alongside model servers to orchestrate multi-stage execution like Prefill/Decode (P/D) [README.md31](https://github.com/llm-d/llm-d-router/blob/99298ae8/README.md?plain=1#L31-L31)  |  `ChatCompletionsHandler`, `NIXLConnector`, `SGLangConnector`  |  
| **Coordinator**  | An orchestration layer for multimodal LLM inference, managing complex pipelines like Encode/Prefill/Decode [go.mod1-51](https://github.com/llm-d/llm-d-router/blob/99298ae8/go.mod#L1-L51)  |  `ReplaceMediaURLsStep`, `PrefillStep`, `DecodeStep`  |  
| **Request Management APIs**  | Kubernetes Custom Resources (CRDs) that define scheduling goals, priority levels, and model rewriting rules [README.md28-30](https://github.com/llm-d/llm-d-router/blob/99298ae8/README.md?plain=1#L28-L30)  |  `InferenceObjective`, `InferenceModelRewrite`  |  
**Sources:** [README.md23-31](https://github.com/llm-d/llm-d-router/blob/99298ae8/README.md?plain=1#L23-L31) [docs/architecture.md23-35](https://github.com/llm-d/llm-d-router/blob/99298ae8/docs/architecture.md?plain=1#L23-L35) [go.mod1-63](https://github.com/llm-d/llm-d-router/blob/99298ae8/go.mod#L1-L63)
## System Architecture
The router operates as an **Inference Gateway** , sitting between the client and the model-serving backend. It leverages [Envoy](https://www.envoyproxy.io/) as the data plane, which communicates with the EPP via the `ext_proc` (External Processing) protocol [README.md15-17](https://github.com/llm-d/llm-d-router/blob/99298ae8/README.md?plain=1#L15-L17) The only supported communication mode for the `ext_proc` filter is `FULL_DUPLEX_STREAMED` [README.md47-50](https://github.com/llm-d/llm-d-router/blob/99298ae8/README.md?plain=1#L47-L50)
### Request Lifecycle and Code Association
The following diagram illustrates how a request flows through the system, mapping natural language concepts to specific code entities.

```



Disaggregation_(Sidecar)
Model_Mesh_(Pods)
Control_Plane_(EPP_Process)
Data_Plane_(Envoy_Proxy)
gRPC:_ExternalProcessor
Scrapes_Metrics
Proxies_Traffic
Internal_Coordination
Client_Request
Envoy_ext_proc_Filter
StreamingServer_[pkg/epp/server/server.go]
Director_[pkg/epp/sched/director.go]
Scheduler_[pkg/epp/framework/runtime/scheduler.go]
Datastore_[pkg/epp/datastore/datastore.go]
vLLM_/_SGLang_Pods
pd-sidecar_[cmd/pd-sidecar/main.go]




```

**Sources:** [README.md19-21](https://github.com/llm-d/llm-d-router/blob/99298ae8/README.md?plain=1#L19-L21) [docs/architecture.md59-74](https://github.com/llm-d/llm-d-router/blob/99298ae8/docs/architecture.md?plain=1#L59-L74) [cmd/epp/main.go39-50](https://github.com/llm-d/llm-d-router/blob/99298ae8/cmd/epp/main.go#L39-L50) [README.md47-50](https://github.com/llm-d/llm-d-router/blob/99298ae8/README.md?plain=1#L47-L50) [DEVELOPMENT.md53-54](https://github.com/llm-d/llm-d-router/blob/99298ae8/DEVELOPMENT.md?plain=1#L53-L54)
## Deployment Modes
The router supports two primary modes of operation [README.md33-35](https://github.com/llm-d/llm-d-router/blob/99298ae8/README.md?plain=1#L33-L35):
  1. **Standalone Mode** : A lightweight deployment where a self-managed Envoy proxy runs alongside the EPP in the same pod. This is ideal for environments without full Gateway API infrastructure [README.md37-38](https://github.com/llm-d/llm-d-router/blob/99298ae8/README.md?plain=1#L37-L38)
  2. **Gateway Mode** : The production-grade mode where the EPP acts as a backend for an `InferencePool` referenced by an `HTTPRoute` on a shared `Gateway`. This enables multi-cluster load balancing and integration with cloud-managed L7 balancers [README.md40-42](https://github.com/llm-d/llm-d-router/blob/99298ae8/README.md?plain=1#L40-L42)


**Sources:** [README.md33-42](https://github.com/llm-d/llm-d-router/blob/99298ae8/README.md?plain=1#L33-L42) [DEVELOPMENT.md15-20](https://github.com/llm-d/llm-d-router/blob/99298ae8/DEVELOPMENT.md?plain=1#L15-L20)
## Disaggregation Topologies
The router can split a single inference request across multiple specialized workers (e.g., dedicated GPUs for Prefill and different ones for Decode) [docs/architecture.md32-33](https://github.com/llm-d/llm-d-router/blob/99298ae8/docs/architecture.md?plain=1#L32-L33) This is configured via flags like `DISAGG_E` and `DISAGG_P` in development environments [scripts/kind-dev-env.sh81-91](https://github.com/llm-d/llm-d-router/blob/99298ae8/scripts/kind-dev-env.sh#L81-L91)
  * **P/D (Prefill/Decode)** : Separates prompt processing from token generation [README.md31](https://github.com/llm-d/llm-d-router/blob/99298ae8/README.md?plain=1#L31-L31)
  * **E/PD (Encode/Prefill-Decode)** : Offloads multimodal encoding (images/video) while keeping prefill/decode unified [DEVELOPMENT.md19](https://github.com/llm-d/llm-d-router/blob/99298ae8/DEVELOPMENT.md?plain=1#L19-L19)
  * **E/P/D (Encode/Prefill/Decode)** : Full disaggregation across three specialized stages [DEVELOPMENT.md20](https://github.com/llm-d/llm-d-router/blob/99298ae8/DEVELOPMENT.md?plain=1#L20-L20)


### Disaggregation Routing Flow
The `disagg-profile-handler` plugin and the `pd-sidecar` coordinate these stages [docs/architecture.md129-131](https://github.com/llm-d/llm-d-router/blob/99298ae8/docs/architecture.md?plain=1#L129-L131)

```



"Decode_Worker_(vLLM)"
"Prefill_Worker_(vLLM)"
"Decode_Sidecar_(pd-sidecar)_[cmd/pd-sidecar/main.go]"
"EPP_(disagg-profile-handler)_[pkg/epp/framework/plugins/scheduling/profile/disagg/disagg_profile_handler.go]"
"Request_+_x-prefiller-host-port"
"Remote_Prefill_(prompt)"
"KV-cache_Metadata"
"Decode_Request_+_Block_IDs"
"Read_KV-cache_(P2P/Shared)"
"Generated_Tokens"
"Final_Response"




```

**Sources:** [docs/architecture.md129-141](https://github.com/llm-d/llm-d-router/blob/99298ae8/docs/architecture.md?plain=1#L129-L141) [README.md31](https://github.com/llm-d/llm-d-router/blob/99298ae8/README.md?plain=1#L31-L31) [scripts/kind-dev-env.sh143-157](https://github.com/llm-d/llm-d-router/blob/99298ae8/scripts/kind-dev-env.sh#L143-L157)
## Navigation
For deep technical dives into specific subsystems, refer to the following child pages:
  * **[Architecture and Core Concepts](https://deepwiki.com/llm-d/llm-d-router/1.1-architecture-and-core-concepts)** : Detailed request lifecycle, `InferencePool` mechanics, and the `ext_proc` integration.
  * **[Getting Started and Development Environment](https://deepwiki.com/llm-d/llm-d-router/1.2-getting-started-and-development-environment)** : Local setup using Kind, build instructions via `Makefile`, and running the test suite [Makefile1-15](https://github.com/llm-d/llm-d-router/blob/99298ae8/Makefile#L1-L15) [DEVELOPMENT.md74-80](https://github.com/llm-d/llm-d-router/blob/99298ae8/DEVELOPMENT.md?plain=1#L74-L80)


**Sources:** [README.md1-97](https://github.com/llm-d/llm-d-router/blob/99298ae8/README.md?plain=1#L1-L97) [docs/architecture.md1-180](https://github.com/llm-d/llm-d-router/blob/99298ae8/docs/architecture.md?plain=1#L1-L180) [go.mod1-63](https://github.com/llm-d/llm-d-router/blob/99298ae8/go.mod#L1-L63) [DEVELOPMENT.md1-48](https://github.com/llm-d/llm-d-router/blob/99298ae8/DEVELOPMENT.md?plain=1#L1-L48) [scripts/kind-dev-env.sh1-170](https://github.com/llm-d/llm-d-router/blob/99298ae8/scripts/kind-dev-env.sh#L1-L170)
Dismiss
Refresh this wiki
Enter email to refresh
### On this page
  * [Request Lifecycle and Code Association](https://deepwiki.com/llm-d/llm-d-router#request-lifecycle-and-code-association)
  * [Disaggregation Topologies](https://deepwiki.com/llm-d/llm-d-router#disaggregation-topologies)
  * [Disaggregation Routing Flow](https://deepwiki.com/llm-d/llm-d-router#disaggregation-routing-flow)


