---
title: KV-Aware Routing on Kubernetes | NVIDIA Dynamo Documentation
id: kv-aware-routing-on-kubernetes-nvidia-dynamo-documentation
tags:
- llmd-router-nonk8s-backends-54b774
- nvidia-dynamo
- epp-protocol
- standalone-epp
- kv-cache-routing
created: '2026-09-11T20:50:19.178303Z'
updated: '2026-09-12T03:37:37.711580Z'
source: https://docs.nvidia.com/dynamo/user-guides/kv-cache-aware-routing
source_domain: docs.nvidia.com
fetched_at: '2026-09-11T20:50:19.157299Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'NVIDIA Dynamo docs page (redirect landed on ''KV-Aware Routing on Kubernetes'',
  current as of fetch) lays out three deployment topologies for KV-cache-aware routing,
  directly relevant to the GIE/EPP + non-pod-backend question. (1) ''Dynamo Frontend
  routing'': the Dynamo Frontend itself tokenizes, scores workers, and forwards --
  fewest components, no Gateway API needed, router flags live on the Frontend (--router-mode
  kv). (2) ''GAIE with Dynamo'': a k8s Gateway calls a Dynamo Endpoint Picker Plugin
  (EPP) implementing the GAIE endpoint-selection protocol before forwarding to a worker
  pod; worker Frontend sidecars then run --router-mode direct since EPP already decided.
  (3) ''GAIE with vanilla vLLM'': explicitly described as adding ''Dynamo''s advanced
  KV-aware routing without migrating the fleet to Dynamo workers'' via ''standalone
  Dynamo EPP support'' with ''EPP with direct vLLM worker discovery and vLLM token
  rendering'' -- i.e., this is NVIDIA''s own standalone-EPP pattern for routing to
  a backend fleet (upstream vLLM pods) that is NOT running the Dynamo-specific worker
  stack, evidence that EPP-based routing to heterogeneous/non-native backends is an
  established, productized pattern beyond llm-d/GIE''s own standalone-EPP work. Explicit
  anti-pattern warning: ''Do not configure the Frontend to select a worker after the
  EPP has already selected one'' -- i.e. worker-selection authority must be singular
  per request path, a design constraint relevant to any router customization that
  layers additional logic on top of EPP''s picks.'
utility_score: 12.0
---

For AI agents: a documentation index is available at the root level at /llms.txt. Append /llms.txt to any URL for a page-level index, or .md for the markdown version of any page.
Dynamo supports two KV-aware request-routing topologies on Kubernetes. They use the same routing concepts but place worker selection in different components. Choose based on how your platform accepts and governs traffic, not because one topology uses a different KV-routing algorithm.
## Recommendation
Use **Dynamo Frontend routing** unless your platform already needs Kubernetes Gateway API. It has fewer components, requires no Gateway API installation, and works with a standard `DynamoGraphDeployment`.
Use **Gateway API routing** when the Gateway must own the external request path—for example, when a platform team standardizes ingress, authentication, rate limiting, traffic policy, or gateway-level telemetry through Gateway API.  
| Choose  | When it fits  | Additional infrastructure  | Where routing is configured  |  
| --- | --- | --- | --- |  
| Using the Dynamo Frontend  | Clients can call one Dynamo Frontend Service directly and you want the simplest deployment.  | None beyond the normal Dynamo platform.  | Frontend with `--router-mode kv`.  |  
| Using GAIE with Dynamo  | Traffic must enter through a Kubernetes Gateway or share platform-managed gateway policy.  | Gateway API, GAIE, a compatible Gateway implementation, and a Dynamo EPP.  | EPP; worker Frontend sidecars use `--router-mode direct`.  |  
| Using GAIE with vanilla vLLM  | You already run upstream vLLM pods and want to add Dynamo’s advanced KV-aware routing without migrating the fleet to Dynamo workers.  | Gateway API, GAIE, a compatible Gateway implementation, and standalone Dynamo EPP support.  | EPP with direct vLLM worker discovery and vLLM token rendering.  |  
The Frontend and GAIE topologies are alternatives for a request path. Do not configure the Frontend to select a worker after the EPP has already selected one.
## Dynamo Frontend Routing
The Dynamo Frontend receives the client request, tokenizes it, scores eligible workers, and forwards the request to the selected worker. Router flags and environment variables belong on the Frontend component.
Choose this topology when:
  * Each deployment can expose its own Frontend Service.
  * You do not need Gateway API policy in the request path.
  * You want the fewest routing components to install and operate.
  * Dynamo should own both HTTP request handling and worker selection.


This topology works out of the box with the normal Dynamo platform; no Gateway API components are required. Use [the Dynamo Frontend](https://docs.nvidia.com/dynamo/dev/kubernetes/kv-aware-routing/using-the-dynamo-frontend) to enable KV-aware worker selection in a DGD.
## GAIE Routing
Gateway API routing separates traffic entry from worker selection. A Kubernetes `Gateway` receives the request and asks the Dynamo Endpoint Picker Plugin (EPP) to select an endpoint before forwarding the request to a worker pod.
Choose this topology when:
  * Your organization already uses Gateway API as the standard ingress layer.
  * Authentication, authorization, rate limiting, traffic policy, or telemetry belongs at the Gateway.
  * Multiple model routes should share a platform-managed address and listener.
  * A platform team manages the Gateway while an application team manages the DGD and `HTTPRoute`.


Gateway API routing requires a separate installation. See [Install Gateway API Inference Extension](https://docs.nvidia.com/dynamo/dev/kubernetes/installation/gateway-api-inference-extension), then [GAIE with Dynamo](https://docs.nvidia.com/dynamo/dev/kubernetes/kv-aware-routing/using-gaie-with-dynamo) or [GAIE with vanilla vLLM](https://docs.nvidia.com/dynamo/dev/kubernetes/kv-aware-routing/vanilla-vllm-onramp).
## What Is the Dynamo EPP?
The Dynamo **Endpoint Picker Plugin (EPP)** is a service that implements the GAIE endpoint-selection protocol. The Gateway calls it before forwarding an inference request. The EPP tokenizes the request, reads Dynamo worker and KV cache state, scores eligible endpoints, and returns the selected worker to the Gateway.
The EPP is not a public inference endpoint and does not run the model. The Dynamo operator deploys it when a DGD contains a component with `type: epp`. In this topology, worker Frontend sidecars run with `--router-mode direct` because the EPP has already made the routing decision.
For the operator-managed setup and request flow, see [Using GAIE with Dynamo](https://docs.nvidia.com/dynamo/dev/kubernetes/kv-aware-routing/using-gaie-with-dynamo).
## Shared Routing Behavior
Both topologies can use worker-published KV cache events to account for prefix overlap and active load. Both can also operate with predicted state when precise KV events are unavailable. The selection algorithm and tuning concepts are shared; only the component hosting them changes.
For the cost model, see [Routing Concepts](https://docs.nvidia.com/dynamo/dev/knowledge-base/modular-components/router/routing-concepts). For shared tuning guidance, see [Configuration and Tuning](https://docs.nvidia.com/dynamo/dev/knowledge-base/modular-components/router/configuration-and-tuning).
Ask AI
Assistant
Responses are generated using AI and may contain mistakes.
Hi, I'm an AI assistant with access to documentation and other content.
Tip: You can toggle this pane with
+
