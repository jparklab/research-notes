---
title: llm-d/llm-d-inference-scheduler | DeepWiki
id: llm-dllm-d-inference-scheduler-deepwiki
tags:
- llmd-router
- epp-architecture
- plugin-registration
- ai-generated-secondary
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:46:22.651883Z'
updated: '2026-09-12T03:37:37.635645Z'
source: https://deepwiki.com/llm-d/llm-d-inference-scheduler
source_domain: deepwiki.com
fetched_at: '2026-09-11T19:46:22.631822Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'DeepWiki AI-generated wiki over llm-d/llm-d-inference-scheduler (last indexed
  27 Apr 2026, commit 9fb04f4b -- predates the rename to llm-d-router and predates
  the ATOM/structured-plugin-config commits seen in the GitHub repo note). Confirms
  plugin registration is compile-time Go, not purely declarative: the diagram shows
  ''plugins.RegisterAllPlugins'' populating a ''Plugin Registry [Go]'' that the EndpointPickerConfig
  YAML then Instantiates-by-type-string, implementing plugins.Scorer / plugins.Filter
  Go interfaces. Names ''Scrapers'' as a plugin type (''Collect pod metadata and metrics
  for scorers'') alongside Filters and Scorers -- terminology GIE''s own docs render
  as DataSource/Extractor. Cites a custom-filter doc showing a ''ByLabels'' filter
  selecting pods based on Kubernetes labels as the canonical extension-point example
  (docs/create_new_filter.md), and states lifecycle hooks span Pre-call, Scoring,
  Post-choice, and After-response phases. System diagram places Redis as external
  KV-cache storage and a ZMQ event stream for precise prefix-cache tracking, both
  consumed by the EPP''s State Layer alongside Pod/Kubernetes API data -- i.e. non-Kubernetes
  external services are already part of the Data Layer''s normal inputs. Because this
  content is AI-generated and 4+ months stale relative to the live repo, treat structural
  claims as directionally correct but verify exact identifiers against current source
  (note flags this snapshot corresponds to a different repo name/URL than the current
  llm-d-router).'
utility_score: 9.0
---

Index your code with Devin
[DeepWiki](https://deepwiki.com/)
[DeepWiki](https://deepwiki.com/)
[llm-d/llm-d-inference-scheduler ](https://github.com/llm-d/llm-d-inference-scheduler "Open repository")
Index your code with
Devin
Edit WikiShare
Last indexed: 27 April 2026 ([9fb04f](https://github.com/llm-d/llm-d-inference-scheduler/commits/9fb04f4b))
  * [System Components and Data Flow](https://deepwiki.com/llm-d/llm-d-inference-scheduler/2.1-system-components-and-data-flow)
  * [Endpoint Picker (EPP) Component](https://deepwiki.com/llm-d/llm-d-inference-scheduler/3-endpoint-picker-\(epp\)-component)
  * [Configuration and EndpointPickerConfig](https://deepwiki.com/llm-d/llm-d-inference-scheduler/3.2-configuration-and-endpointpickerconfig)
  * [Plugin Architecture and Lifecycle](https://deepwiki.com/llm-d/llm-d-inference-scheduler/4.1-plugin-architecture-and-lifecycle)
  * [Data Producer Plugins and SLO-Aware Scheduling](https://deepwiki.com/llm-d/llm-d-inference-scheduler/4.8-data-producer-plugins-and-slo-aware-scheduling)
  * [Disaggregated Inference (P/D, E/PD, E/P/D)](https://deepwiki.com/llm-d/llm-d-inference-scheduler/5.1-disaggregated-inference-\(pd-epd-epd\))
  * [Context-Length-Aware Scheduling](https://deepwiki.com/llm-d/llm-d-inference-scheduler/5.6-context-length-aware-scheduling)
  * [Sidecar Architecture and Build](https://deepwiki.com/llm-d/llm-d-inference-scheduler/6.1-sidecar-architecture-and-build)
  * [Encode/Prefill/Decode (EPD) Connectors](https://deepwiki.com/llm-d/llm-d-inference-scheduler/6.3-encodeprefilldecode-\(epd\)-connectors)
  * [Kubernetes Development Environment](https://deepwiki.com/llm-d/llm-d-inference-scheduler/7.3-kubernetes-development-environment)
  * [Istio and KGateway Integration](https://deepwiki.com/llm-d/llm-d-inference-scheduler/11.2-istio-and-kgateway-integration)
  * [InferencePool and Backend Configuration](https://deepwiki.com/llm-d/llm-d-inference-scheduler/11.3-inferencepool-and-backend-configuration)
  * [Flow Control Policies and Queues](https://deepwiki.com/llm-d/llm-d-inference-scheduler/12.2-flow-control-policies-and-queues)


# Overview
Relevant source files
  * [.github/workflows/check-typos.yaml](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/.github/workflows/check-typos.yaml)
  * [.github/workflows/ci-release.yaml](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/.github/workflows/ci-release.yaml)
  * [.github/workflows/md-link-check.yml](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/.github/workflows/md-link-check.yml)
  * [pkg/epp/framework/plugins/flowcontrol/fairness/globalstrict/README.md](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/pkg/epp/framework/plugins/flowcontrol/fairness/globalstrict/README.md?plain=1)
  * [pkg/epp/framework/plugins/flowcontrol/fairness/roundrobin/README.md](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/pkg/epp/framework/plugins/flowcontrol/fairness/roundrobin/README.md?plain=1)
  * [pkg/epp/framework/plugins/flowcontrol/ordering/README.md](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/pkg/epp/framework/plugins/flowcontrol/ordering/README.md?plain=1)
  * [pkg/epp/framework/plugins/flowcontrol/ordering/edf/README.md](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/pkg/epp/framework/plugins/flowcontrol/ordering/edf/README.md?plain=1)
  * [pkg/epp/framework/plugins/flowcontrol/ordering/fcfs/README.md](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/pkg/epp/framework/plugins/flowcontrol/ordering/fcfs/README.md?plain=1)
  * [pkg/epp/framework/plugins/flowcontrol/ordering/slodeadline/README.md](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/pkg/epp/framework/plugins/flowcontrol/ordering/slodeadline/README.md?plain=1)


## Purpose and Scope
The **llm-d-inference-scheduler** provides an intelligent request routing system for large language model (LLM) inference serving. The scheduler acts as an **Endpoint Picker (EPP)** that makes cache-aware, load-balanced routing decisions to optimize request distribution across model-serving pods in a Kubernetes cluster. It is designed to handle complex LLM serving patterns, including disaggregated prefill/decode/encode architectures and multi-modal workloads.
The project implements the extension protocol enabling a proxy or gateway to request endpoint hints from an extension, interacting with model servers via the model server protocol [pkg/epp/README.md1-5](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/pkg/epp/README.md?plain=1#L1-L5)
This document introduces the overall system architecture, key components, and capabilities. For detailed information about specific subsystems:
  * Core architecture and system data flow: see [Core Architecture](https://deepwiki.com/llm-d/llm-d-inference-scheduler/2-core-architecture)
  * EPP application structure and configuration: see [Endpoint Picker (EPP) Component](https://deepwiki.com/llm-d/llm-d-inference-scheduler/3-endpoint-picker-\(epp\)-component)
  * Plugin types and lifecycle hooks: see [Plugin System](https://deepwiki.com/llm-d/llm-d-inference-scheduler/4-plugin-system)
  * Disaggregated inference strategies (P/D, E/P/D): see [Scheduling Strategies](https://deepwiki.com/llm-d/llm-d-inference-scheduler/5-scheduling-strategies)
  * Development and deployment procedures: see [Development Environment](https://deepwiki.com/llm-d/llm-d-inference-scheduler/7-development-environment) and [Building and Deployment](https://deepwiki.com/llm-d/llm-d-inference-scheduler/8-building-and-deployment)


**Sources:** [README.md6-18](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/README.md?plain=1#L6-L18) [docs/architecture.md5-19](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L5-L19) [pkg/epp/README.md1-5](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/pkg/epp/README.md?plain=1#L1-L5)
## Relationship to Gateway API Inference Extension
The llm-d-inference-scheduler extends the [Gateway API Inference Extension (GIE)](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/Gateway%20API%20Inference%20Extension%20\(GIE\)) project [README.md19-25](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/README.md?plain=1#L19-L25) GIE provides the foundational API resources and scheduling machinery, while llm-d adds specialized features for advanced LLM serving scenarios.
**Dependency Structure:**
  * **GIE provides:** `InferencePool`, `EndpointPickerConfig` CRDs, and the base scheduling framework interfaces [pkg/epp/README.md7-18](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/pkg/epp/README.md?plain=1#L7-L18)
  * **llm-d adds:** Specialized plugins for disaggregated prefill/decode/encode, precise KV-cache tracking via ZMQ, and load-aware scoring.


The division of responsibilities follows this principle: general-purpose inference scheduling capabilities are contributed to GIE, while llm-d-specific features (particularly those requiring deep integration with the broader llm-d inference framework or experimental disaggregation modes) remain in this repository [README.md27-38](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/README.md?plain=1#L27-L38)
**Sources:** [README.md19-38](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/README.md?plain=1#L19-L38) [docs/architecture.md38-50](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L38-L50) [pkg/epp/README.md7-22](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/pkg/epp/README.md?plain=1#L7-L22)
## System Architecture
The llm-d-inference-scheduler operates as a control plane component for a Kubernetes Gateway, intercepting requests to make intelligent routing decisions via the Envoy `ext_proc` protocol [README.md40-43](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/README.md?plain=1#L40-L43)
**System Entity Map** The following diagram bridges high-level system concepts to specific code entities and protocols used in the implementation.

```



State Layer
Inference Layer [vLLM]
Scheduling Layer [EPP]
Gateway Layer [Envoy]
Client Layer
HTTP Request
gRPC Call [Port 9002]
Consults
Selects
Routes to
Remote Prefill
Publishes Events
Subscribed by
Writes KV
Reads KV
External Client
ext_proc Filter
Main [cmd/epp/main.go]
Runner [runner.NewRunner]
Scheduling Framework
Plugin Registry [plugins.RegisterAllPlugins]
Prefill Worker
Decode Worker
Sidecar [llm-d-routing-sidecar]
ZMQ Event Stream
Redis [KV-Cache Storage]




```

**Request Flow:**
  1. **Intercept:** Envoy proxy receives a request and forwards it to the EPP via gRPC `ext_proc` [README.md40-43](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/README.md?plain=1#L40-L43)
  2. **Schedule:** The `Runner` executes the scheduling pipeline, using registered plugins to filter and score endpoints [cmd/epp/main.go59-70](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/cmd/epp/main.go#L59-L70)
  3. **Route:** EPP returns a routing decision to Envoy, potentially including headers for disaggregated execution [pkg/epp/README.md11-18](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/pkg/epp/README.md?plain=1#L11-L18)
  4. **Execute:** The backend worker (often vLLM with a sidecar) processes the request, coordinating with other roles as needed.


**Sources:** [docs/architecture.md34-51](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L34-L51) [cmd/epp/main.go38-71](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/cmd/epp/main.go#L38-L71) [pkg/epp/README.md1-22](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/pkg/epp/README.md?plain=1#L1-L22)
## Core Components
The system consists of these primary components:  
| Component  | Type  | Purpose  | Code Location  |  
| --- | --- | --- | --- |  
| **Endpoint Picker (EPP)**  | Go Application  | Main scheduler making decisions via `ext_proc`  |  
| **Plugin System**  | Framework  | Extensible logic for Filtering, Scoring, and Picking  | [docs/architecture.md54-68](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L54-L68)  |  
| **Routing Sidecar**  | Go Application  | Coordinates remote prefill/encode on worker pods  | [.github/workflows/ci-release.yaml24](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/.github/workflows/ci-release.yaml#L24-L24)  |  
| **Filters**  | Plugin Type  | Exclude pods based on static or dynamic criteria  | [docs/architecture.md60](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L60-L60)  |  
| **Scorers**  | Plugin Type  | Assign scores to candidate pods for ranking  | [docs/architecture.md61](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L61-L61)  |  
| **Scrapers**  | Plugin Type  | Collect pod metadata and metrics for scorers  | [docs/architecture.md62](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L62-L62)  |  
**Sources:** [cmd/epp/main.go23-25](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/cmd/epp/main.go#L23-L25) [docs/architecture.md54-68](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L54-L68) [.github/workflows/ci-release.yaml24](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/.github/workflows/ci-release.yaml#L24-L24)
## Key Capabilities
### Disaggregated Inference (P/D and E/P/D)
The scheduler supports routing a single inference request across specialized workers (Prefill, Decode, and experimental Encode) [docs/architecture.md16-17](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L16-L17) This allows for independent scaling of compute-intensive prefill stages and memory-bandwidth-bound decode stages.
**Disaggregation Logic:**
  1. **Decision:** The system evaluates if a request should be disaggregated based on configured thresholds.
  2. **Multi-Profile Execution:** The EPP executes multiple scheduling profiles (e.g., a "prefill" profile and a "decode" profile) [docs/architecture.md149-153](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L149-L153)
  3. **Coordination:** The EPP injects routing metadata into request headers so backend sidecars can coordinate KV-cache transfers between workers.


**Sources:** [docs/architecture.md16-17](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L16-L17) [docs/architecture.md149-166](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L149-L166)
### Cache-Aware Scheduling
The system optimizes for KV-cache reuse to minimize Time To First Token (TTFT). It can track cache state across endpoints to route requests where prompt prefixes are already resident [docs/architecture.md14-15](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L14-L15)
**Mechanism:**
  * **Filtering:** Pods are narrowed down based on model compatibility and resource usage [docs/architecture.md81-84](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L81-L84)
  * **Scoring:** Filtered pods are scored using a weighted set of scorers, such as the `precise-prefix-cache-scorer` [docs/architecture.md85-89](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L85-L89)
  * **Selection:** The highest-scored pod is selected, with random tie-breaking [docs/architecture.md90-94](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L90-L94)


**Sources:** [docs/architecture.md14-15](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L14-L15) [docs/architecture.md79-94](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L79-L94) [docs/architecture.md172-182](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L172-L182)
### Load-Aware and Pluggable Scheduling
The scheduler is highly extensible, allowing operators to compose complex routing logic via `EndpointPickerConfig` [docs/architecture.md105-114](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L105-L114)
**Code-to-Config Association** This diagram shows how the YAML configuration maps to the plugin architecture.

```



Plugin Registry [Go]
EndpointPickerConfig [YAML]
Instantiates
Orchestrates
type: precise-prefix-cache-scorer
type: decode-filter
type: single-profile-handler
plugins.RegisterAllPlugins
plugins.Scorer Interface
plugins.Filter Interface




```

**Key Features:**
  * **Lifecycle Hooks:** Plugins can hook into `Pre-call`, `Scoring`, `Post-choice`, and `After-response` phases [docs/architecture.md96-103](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L96-L103)
  * **Custom Filters:** Developers can implement custom logic, such as the `ByLabels` filter which selects pods based on Kubernetes labels [docs/create_new_filter.md8-11](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/create_new_filter.md?plain=1#L8-L11)
  * **Traffic Splitting:** Supports controlled rollouts by implementing traffic splitting between adapters within the same `InferencePool` [pkg/epp/README.md17-18](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/pkg/epp/README.md?plain=1#L17-L18)


**Sources:** [docs/architecture.md105-166](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/architecture.md?plain=1#L105-L166) [docs/create_new_filter.md24-35](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/docs/create_new_filter.md?plain=1#L24-L35) [pkg/epp/README.md11-22](https://github.com/llm-d/llm-d-inference-scheduler/blob/9fb04f4b/pkg/epp/README.md?plain=1#L11-L22)
Dismiss
Refresh this wiki
Enter email to refresh
### On this page
  * [Relationship to Gateway API Inference Extension](https://deepwiki.com/llm-d/llm-d-inference-scheduler#relationship-to-gateway-api-inference-extension)
  * [Disaggregated Inference (P/D and E/P/D)](https://deepwiki.com/llm-d/llm-d-inference-scheduler#disaggregated-inference-pd-and-epd)
  * [Load-Aware and Pluggable Scheduling](https://deepwiki.com/llm-d/llm-d-inference-scheduler#load-aware-and-pluggable-scheduling)


