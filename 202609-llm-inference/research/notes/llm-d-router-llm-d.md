---
title: llm-d Router | llm-d
id: llm-d-router-llm-d
tags:
- llmd-router
- epp-architecture
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:42:43.925491Z'
updated: '2026-09-12T03:37:37.505778Z'
source: https://llm-d.ai/docs/dev/architecture/core/router
source_domain: llm-d.ai
fetched_at: '2026-09-11T19:42:43.906447Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'llm-d docs page defining the llm-d Router as Proxy + Endpoint Picker (EPP).
  Proxy is any conformant L7 proxy (Istio, agentgateway, Envoy AI Gateway, cloud ALBs)
  handling data-plane concerns; EPP holds the routing intelligence. Clarifies terminology:
  ''llm-d Router'' = Proxy+EPP; ''Inference Gateway'' = llm-d Router in Gateway Mode;
  ''Request Scheduler'' = the EPP sub-component doing filter/score. Confirms the EPP
  evaluates requests against InferencePool state (KV-cache locality, load, priority)
  and returns a model-server pod address to the Proxy.'
utility_score: 15.0
---

[Skip to main content](https://llm-d.ai/docs/dev/architecture/core/router#__docusaurus_skipToContent_fallback)
🎉 **llm-d 0.9 is here!** Hardened for scale with router HA, enhanced autoscaling, end-to-end observability and expanded hardware support. [**Read the release blog →**](https://llm-d.ai/blog/llm-d-v0.9-hardened-for-scale)
This is unreleased documentation for llm-d **dev** version.
For up-to-date documentation, see the (v0.9 (latest)).
Version: dev
On this page
The **llm-d Router** is the intelligent entry point for inference requests in the llm-d stack. It provides sophisticated, LLM-aware load balancing, request queuing, and policy enforcement without reimplementing a full-featured network proxy.
## Composition[​](https://llm-d.ai/docs/dev/architecture/core/router#composition "Direct link to Composition")
The llm-d Router is composed of two primary functional parts:
### Proxy[​](https://llm-d.ai/docs/dev/architecture/core/router#proxy "Direct link to Proxy")
Any conformant industry-grade L7 proxy. The proxy handles the data plane, including connection management, TLS termination, and request forwarding. Supported implementations range from self-managed application load balancers (e.g., Istio, agentgateway, Envoy AI Gateway) to compliant cloud-managed services, such as Google Cloud's Application Load Balancer.
See the [**Proxy deep dive**](https://llm-d.ai/docs/dev/architecture/core/router/proxy) to learn about deployment modes (Standalone vs. Gateway Mode), request flow, and Gateway API integration.
### llm-d Endpoint Picker (EPP)[​](https://llm-d.ai/docs/dev/architecture/core/router#llm-d-endpoint-picker-epp "Direct link to llm-d Endpoint Picker \(EPP\)")
A specialized service that the proxy consults for every request. The [EPP](https://llm-d.ai/docs/dev/architecture/core/router/epp) contains the routing "intelligence," using real-time signals from model servers to make optimal placement decisions.
See the [**llm-d EPP deep dive**](https://llm-d.ai/docs/dev/architecture/core/router/epp) to learn about the the routing engine's architecture, plugin pipeline (Filters, Scorers, Pickers), and flow control mechanisms.
## How it Works[​](https://llm-d.ai/docs/dev/architecture/core/router#how-it-works "Direct link to How it Works")
When an inference request arrives at the Proxy, the Proxy "parks" the request and initiates a callback to the EPP via the `ext-proc` (External Processing) protocol.
The EPP evaluates the request against the current state of the [InferencePool](https://llm-d.ai/docs/dev/architecture/core/inferencepool)—considering factors like KV-cache locality, current load, and priority—and returns the address of the optimal model server pod back to the Proxy. The Proxy then forwards the original request to that specific destination.
This decoupled architecture allows llm-d to leverage the performance and reliability of production-grade proxies while providing a highly extensible framework for LLM-specific routing logic.
## Terminology[​](https://llm-d.ai/docs/dev/architecture/core/router#terminology "Direct link to Terminology")
To ensure clarity across the project, we use the following standard terminology:
  * **llm-d Router** : The complete intelligent entry point, comprising both the **Proxy** and the **Endpoint Picker (EPP)**. This term replaces "Router" in all contexts.
  * **llm-d Endpoint Picker (EPP)** : The specific component that implements the routing intelligence and scoring logic. Use this term when referring to capabilities or configurations specific to the EPP itself, rather than the request routing system as a whole.
  * **Inference Gateway** : A synonym for the **llm-d Router** when operating in **Gateway Mode**.
  * **Request Scheduler** : A sub-component within the EPP that acts as the core decision-making engine for intelligent request routing by filtering and scoring candidate model servers (aka endpoints).


  * [Composition](https://llm-d.ai/docs/dev/architecture/core/router#composition)
    * [llm-d Endpoint Picker (EPP)](https://llm-d.ai/docs/dev/architecture/core/router#llm-d-endpoint-picker-epp)


