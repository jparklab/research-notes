---
title: Gateway API Inference Extension (GAIE) | NVIDIA Dynamo Documentation
id: gateway-api-inference-extension-gaie-nvidia-dynamo-documentation
tags:
- gateway-routing
- epp
- nvidia-dynamo
- kv-cache-routing
created: '2026-10-09T23:00:58.103909Z'
updated: '2026-10-10T14:52:17.159976Z'
source: https://docs.nvidia.com/dynamo/v1.3.0/kubernetes-deployment/request-routing/gateway-api-inference-extension/overview
source_domain: docs.nvidia.com
fetched_at: '2026-10-09T23:00:58.079304Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'NVIDIA''s own operator documentation (Dynamo platform, docs version v1.3.0)
  for integrating Dynamo''s model-serving framework with Gateway API Inference Extension
  (GAIE) -- a first-person vendor account of exactly how a third-party inference engine
  plugs into the GIE abstraction, confirming the EPP extension point is a real, actively-maintained
  integration surface rather than a one-off. Describes two routing topologies: Dynamo-native
  Frontend routing (Dynamo''s own integrated router selects workers directly) versus
  ''Gateway API routing with GAIE'' (a Kubernetes Gateway receives traffic, GAIE calls
  the Dynamo Endpoint Picker Plugin/EPP for endpoint selection, and the selected worker''s
  Frontend sidecar forwards in ''direct'' routing mode -- i.e., the worker trusts
  the EPP''s choice rather than re-routing itself). States explicitly: ''Dynamo is
  independent of the Gateway implementation'' and verifies two specific Gateway API
  implementations for this integration: agentgateway (Rust-based AI gateway, GatewayClass
  agentgateway, enabled via inferenceExtension.enabled=true) and Istio (uses Envoy
  in its data plane, enabled via ENABLE_GATEWAY_API_INFERENCE_EXTENSION=true, requires
  an EPP DestinationRule when mesh TLS policy applies). Names the exact component
  ownership split: user/platform team creates Gateway and HTTPRoute; Dynamo operator
  creates the EPP Service, worker pods, Frontend sidecars, and the generated InferencePool
  that binds the route to the EPP. Documents two concrete routing modes the EPP can
  run in: ''KV cache aware routing'' (default, uses worker-published KV cache events
  delivered through Dynamo''s own NATS-Core-based event plane for cache-locality-informed
  endpoint selection) versus ''Approximate routing'' (fallback using only endpoint
  availability plus local request-lifecycle bookkeeping when precise KV events are
  unavailable). Gives a pinned compatibility table for a specific release line: Gateway
  API CRDs v1.5.1, GAIE CRDs v1.2.1, agentgateway v1.0.0, Istio 1.29.2, Dynamo images
  1.2.1 -- concrete versioned evidence of the stack maturity as of this doc''s publication.
  Lists exact scorer/picker plugin config knobs (schedulingProfiles[].plugins[].weight,
  label-filter for worker topology labels, DYN_MODEL_NAME, DYN_KV_CACHE_BLOCK_SIZE)
  showing GAIE''s plugin architecture supports per-deployment custom scoring strategies
  beyond the reference implementation.'
---

*Suggested by [[envoy-ai-gateway-hits-10-what-an-llm-aware-kubernetes-gateway-actually-load-bala]] — primary vendor doc for NVIDIA Dynamo's own EPP implementation, named in the bex.co deep dive*

For AI agents: a documentation index is available at the root level at /llms.txt. Append /llms.txt to any URL for a page-level index, or .md for the markdown version of any page.
This page is for version v1.3.0. For other versions, use one of these documentation indexes: - Latest (v1.5.1) (default): https://docs.nvidia.com/dynamo/latest/llms.txt - dev: https://docs.nvidia.com/dynamo/dev/llms.txt - v1.5.1: https://docs.nvidia.com/dynamo/v1.5.1/llms.txt - v1.5.0: https://docs.nvidia.com/dynamo/v1.5.0/llms.txt - v1.4.2: https://docs.nvidia.com/dynamo/v1.4.2/llms.txt - v1.4.1: https://docs.nvidia.com/dynamo/v1.4.1/llms.txt - v1.4.0: https://docs.nvidia.com/dynamo/v1.4.0/llms.txt - v1.3.0: https://docs.nvidia.com/dynamo/v1.3.0/llms.txt - v1.2.1: https://docs.nvidia.com/dynamo/v1.2.1/llms.txt - v1.2.0: https://docs.nvidia.com/dynamo/v1.2.0/llms.txt - v1.1.1: https://docs.nvidia.com/dynamo/v1.1.1/llms.txt - v1.1.0: https://docs.nvidia.com/dynamo/v1.1.0/llms.txt - v1.0.2: https://docs.nvidia.com/dynamo/v1.0.2/llms.txt - v1.0.1: https://docs.nvidia.com/dynamo/v1.0.1/llms.txt - v1.0.0: https://docs.nvidia.com/dynamo/v1.0.0/llms.txt - v0.9.1: https://docs.nvidia.com/dynamo/v-0-9-1/llms.txt - v0.9.0: https://docs.nvidia.com/dynamo/v-0-9-0/llms.txt - v0.8.1: https://docs.nvidia.com/dynamo/v-0-8-1/llms.txt - v0.8.0: https://docs.nvidia.com/dynamo/v-0-8-0/llms.txt - v0.7.1: https://docs.nvidia.com/dynamo/v-0-7-1/llms.txt - v0.7.0: https://docs.nvidia.com/dynamo/v-0-7-0/llms.txt
Dynamo supports two request routing topologies on Kubernetes:
  * **Dynamo-native Frontend routing.** The Dynamo Frontend receives HTTP requests and the integrated Dynamo Router selects workers.
  * **Gateway API routing with GAIE.** A Kubernetes `Gateway` receives HTTP requests, the [Gateway API Inference Extension (GAIE)](https://github.com/kubernetes-sigs/gateway-api-inference-extension) calls the Dynamo Endpoint Picker Plugin (EPP) for endpoint selection, and the selected worker’s Frontend sidecar forwards the request in direct mode.


This guide covers the Gateway API path for `DynamoGraphDeployment` resources managed by the Dynamo operator. Use it when your Kubernetes platform wants Gateway API to own traffic entry, policy, and observability while Dynamo owns the serving graph, discovery, event plane, and routing logic inside the EPP.
## Components
The operator-managed GAIE path combines user-created Gateway API objects with resources created from the `DynamoGraphDeployment`.  
| Component  | Role  | Created by  |  
| --- | --- | --- |  
| `Gateway`  | Receives external HTTP traffic for the namespace.  | User or platform team  |  
| `HTTPRoute`  | Attaches model traffic to the `Gateway` and points at the `InferencePool`.  | User  |  
| `DynamoGraphDeployment`  | Describes the serving graph, EPP component, workers, and Frontend sidecars.  | User  |  
| Dynamo operator  | Reconciles the DGD into Kubernetes resources.  | Dynamo platform  |  
| `InferencePool`  | Connects GAIE endpoint selection to the Dynamo EPP service.  | Dynamo operator  |  
| Dynamo EPP  | Scores endpoints and returns the selected worker to the gateway.  | Dynamo operator  |  
| Frontend sidecar  | Receives the already-selected request and forwards in direct mode.  | Dynamo operator  |  
| Worker  | Runs the model backend.  | Dynamo operator  |  
## Request Flow

```
%% flowchart-v2 diagram
Client/v1/chat/completions
Gateway APIGateway
HTTPRoute
InferencePool
Dynamo EPPEndpointPicker
Frontend sidecar -- router-mode direct
Dynamo worker
Dynamo runtimeNATS Core event plane
```

Gateway API owns the external request path. Dynamo still owns the serving graph: the operator creates the EPP Service, worker pods, Frontend sidecars, and `InferencePool` that binds the route to the EPP. The EPP receives Dynamo routing state from the runtime event plane and returns the selected worker ID to the gateway. The gateway forwards the request to the selected worker’s Frontend sidecar, which runs in direct routing mode.
In this operator-managed path, the EPP consumes live routing state through the Dynamo event plane using NATS Core. Direct vLLM ZMQ KV-event subscriptions are used by other integration shapes, but not by this quickstart path.
## Shared Prerequisites
  * A Kubernetes cluster with GPU nodes. For the baseline Gateway API environment, start with the upstream [Gateway API getting started guide](https://gateway-api.sigs.k8s.io/guides/getting-started/introduction/) and the upstream [GAIE introduction](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/site-src/index.md).
  * `kubectl`, [Helm](https://helm.sh/docs/intro/install/), and [jq](https://jqlang.org/download/) configured for the cluster.
  * Gateway API and GAIE CRDs installed. The quickstart installs them explicitly from pinned upstream release manifests.
  * A Gateway API implementation that supports GAIE `InferencePool` resources and `endpointPickerRef` calls.
  * Dynamo platform installed with the operator. See the [Kubernetes Quickstart](https://docs.nvidia.com/dynamo/v1.3.0/kubernetes-deployment/start-here/kubernetes-quickstart) and [Installation Guide](https://docs.nvidia.com/dynamo/v1.3.0/kubernetes-deployment/start-here/installation-guide).
  * Model credentials and storage needed by the selected model. Hugging Face token secrets are a Dynamo model-serving prerequisite, not a GAIE-specific resource; see the [Hugging Face token secret](https://docs.nvidia.com/dynamo/v1.3.0/kubernetes-deployment/start-here/kubernetes-quickstart#huggingface-token-secret) setup.


## Gateway Implementation
GAIE requires a Gateway API implementation that can call an Endpoint Picker Plugin before forwarding the request to a backend. Dynamo is independent of the Gateway implementation: pick the gateway that matches your platform, then point its `HTTPRoute` and generated `InferencePool` at the Dynamo EPP.
The quickstart shows two verified paths: agentgateway and Istio. Other Gateway API implementations might work when they support the same GAIE `InferencePool` and `endpointPickerRef` EPP path; check the upstream [GAIE gateway implementation list](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/site-src/implementations/gateways.md) and your controller’s documentation before choosing another implementation.
Istio uses Envoy in its data plane. agentgateway is a Rust-based AI gateway. The requirement for this guide is not Envoy specifically; it is support for Gateway API plus the GAIE EndpointPicker flow.
###### agentgateway
###### Istio
Use agentgateway for a small Gateway API footprint or when the cluster does not already standardize on a service mesh. Install the agentgateway chart with `inferenceExtension.enabled=true`; the GatewayClass is `agentgateway`.
The quickstart walks through the two verified implementation paths shown in this table:  
| agentgateway  | Istio  |  
| --- | --- |  
| Good fit  | New clusters or clusters without a mesh standard  | Clusters that already standardize on Istio  |  
| Install footprint  | agentgateway CRDs and controller in `agentgateway-system`  | Istio control plane in `istio-system` or your chosen namespace  |  
| GatewayClass  | `agentgateway`  | `istio`  |  
| GAIE support  | Enable `inferenceExtension.enabled=true` on the chart  | Install Istio with `ENABLE_GATEWAY_API_INFERENCE_EXTENSION=true`  |  
| Mesh interaction  | Use `AgentgatewayParameters` to keep `agentgateway-proxy` out of sidecar injection  | Configure EPP TLS with a `DestinationRule` when mesh policy applies  |  
## Gateway API Concepts

```
%% flowchart-v2 diagram
Gateway namespace
Gatewaylistener and address
Model namespace
HTTPRoute
InferencePool
Dynamo EPP Service
```

`HTTPRoute.spec.parentRefs` attaches a route to a `Gateway`. If the `HTTPRoute` and `Gateway` live in different namespaces, set `parentRefs[].namespace` to the Gateway namespace. `rules[].backendRefs` points at the `InferencePool`; the pool points at the EPP service through `endpointPickerRef`.
For the upstream API model, see the [Gateway API HTTP routing guide](https://gateway-api.sigs.k8s.io/guides/user-guides/http-routing/) and the [cross-namespace routing guide](https://gateway-api.sigs.k8s.io/guides/user-guides/multiple-ns/).
## Configure DynamoGraphDeployments for GAIE
In GAIE mode, the EPP chooses workers. The worker Frontend sidecar must run in direct routing mode so it honors the EPP selection instead of choosing a worker again.

```



frontendSidecar: sidecar-frontend


podTemplate:




    containers:


 name: sidecar-frontend





 dynamo.frontend


 --router-mode


 direct



```

The EPP component is part of the `DynamoGraphDeployment`. The operator creates the EPP Service and the matching `InferencePool`, so users apply the DGD and the route instead of hand-crafting the pool.
### EPP Component Configuration
Start from the recipe EPP component and update the `DynamoGraphDeployment` for your cluster. Change deployment-level settings such as replicas and resources to fit gateway traffic volume. Change routing plugin settings only when you want different endpoint-selection behavior, then validate the result with production-like traffic.  
| Setting  | When to change it  | Rule  |  
| --- | --- | --- |  
|  `replicas` and `podTemplate.spec.containers[].resources`  | Scale or reserve capacity for EPP pods.  | Keep EPP capacity aligned with gateway request volume.  |  
| `DYN_MODEL_NAME`  | Change the served model.  | Match the worker model name.  |  
| `DYN_KV_CACHE_BLOCK_SIZE`  | Change the backend KV block size.  | Match the backend `--block-size` value.  |  
|  `label-filter` parameters  | Change worker topology labels or component names.  | Keep filter labels and values aligned with worker pod labels.  |  
| `schedulingProfiles[].plugins[].weight`  | Adjust how much each scorer influences endpoint selection.  | Tune scorer weights deliberately; keep required filters and the picker in the profile.  |  
| scorer and picker plugins  | Change the routing strategy.  | Treat this as advanced EPP tuning and validate with traffic.  |  
For upstream Endpoint Picker Plugin (EPP) semantics, see the GAIE [Implementer’s Guide](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/site-src/guides/implementers.md) and the [Endpoint Picker Protocol specification](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/004-endpoint-picker-protocol). For label-based endpoint selection, see the upstream [InferencePool configuration guide](https://gateway-api-inference-extension.sigs.k8s.io/api-types/inferencepool/#how-to-configure-an-inferencepool). The `label-filter` plugin shown here is Dynamo-specific; the component role label comes from the Dynamo [ComponentType](https://docs.nvidia.com/dynamo/v1.3.0/additional-resources/api-reference-k-8-s#componenttype) field.
The operator reconciles the EPP `Deployment`, EPP `Service`, and generated `InferencePool` from the DGD. Tune the DGD first; patch generated resources only for short-lived debugging.

```



- name: Epp


  type: epp


  replicas: 1


  eppConfig:


    config:


      plugins:


 type: disagg-profile-handler


 name: decode-filter


          type: label-filter


          parameters:


            label: nvidia.com/dynamo-component-type


            validValues: [decode]


            allowsNoLabel: true # Aggregated recipes can route unlabeled decode pods.


 name: dyn-decode


          type: dyn-decode-scorer


 name: picker


          type: max-score-picker


      schedulingProfiles:


 name: decode


          plugins:


 pluginRef: decode-filter


              weight: 1 # Keep topology filters aligned with the worker labels.


 pluginRef: dyn-decode


              weight: 1 # Tune scorer weights to change endpoint scoring.


 pluginRef: picker


              weight: 1



```

The `DYN_*` environment values are runtime contracts between the EPP router logic and the workers. Update them when the worker backend changes; do not use them to tune scoring.

```



- name: Epp


  type: epp


  podTemplate:




      containers:


 name: main



 name: DYN_MODEL_NAME


              value: Qwen/Qwen3-0.6B # Match the worker model name.


 name: DYN_KV_CACHE_BLOCK_SIZE


              value: "16" # Match the worker backend's --block-size.



```

See the complete EPP examples in the source tree: `recipes/qwen3-0.6b/vllm/agg/gaie/deploy.yaml` for the Qwen 0.6B aggregated recipe manifest and `examples/backends/vllm/deploy/gaie/disagg.yaml` for the Qwen 0.6B disaggregated example manifest.
## Routing Behavior
GAIE does not require one scoring strategy. Choose the routing behavior based on the routing state available to the EPP.  
| Mode  | What the EPP uses  | When to use it  |  
| --- | --- | --- |  
| KV cache aware routing  | Worker-published KV cache events delivered through the Dynamo event plane.  | Default path when workers publish KV events and you want cache locality to influence endpoint selection.  |  
| Approximate routing  | Endpoint availability plus local bookkeeping from tokenized requests and request lifecycle.  | Fallback path when precise worker-published KV events are unavailable, disabled, or not yet supported by the chosen backend or deployment shape.  |  
With operator-managed GAIE, NATS Core backs live routing-state delivery. After a restart, the EPP rebuilds prefix state from worker-local indexers and then consumes subsequent updates through the Dynamo event plane.
## Compatibility and Defaults
The quickstart pins the Gateway API layer so manual setup is repeatable. Keep the Dynamo platform, EPP, and runtime images on the same Dynamo release line.  
| Component  | Default shown here  | Notes  |  
| --- | --- | --- |  
| Gateway API CRDs  | `v1.5.1`  | Installed from the upstream Gateway API release.  |  
| GAIE CRDs  | `v1.2.1`  | Installed from the upstream Gateway API Inference Extension release.  |  
| agentgateway  | `v1.0.0`  | Installed with `inferenceExtension.enabled=true`.  |  
| Istio  | `1.29.2`  | Install with `ENABLE_GATEWAY_API_INFERENCE_EXTENSION=true`.  |  
| Dynamo images  | `1.2.1`  | Use one Dynamo release line for the platform chart, EPP image, and runtime images.  |  
## Troubleshooting Signals  
| Symptom  | Likely cause  | Check  |  
| --- | --- | --- |  
|  `HTTPRoute` is not accepted  |  `parentRefs` points at the wrong Gateway name or namespace.  |  `kubectl describe httproute -n <model-namespace>` and compare `spec.parentRefs` with the Gateway.  |  
| Requests reach a model but EPP logs stay quiet  | The route bypasses the `InferencePool`, or the pool points at the wrong EPP service.  | Verify `rules.backendRefs` points at the `InferencePool` and `endpointPickerRef` points at the Dynamo EPP service.  |  
| EPP does not receive routing state  | Dynamo event-plane components are not ready, or image tags do not match.  | Check Dynamo platform pods, DGD status, EPP logs, and image tags against the compatibility table.  |  
| Istio path cannot call the EPP  | Istio was installed without GAIE enabled, or mesh TLS policy blocks the EPP call.  | Confirm `ENABLE_GATEWAY_API_INFERENCE_EXTENSION=true` and configure the EPP `DestinationRule`.  |  
## Next Step
Run the [GAIE Quickstart](https://docs.nvidia.com/dynamo/v1.3.0/kubernetes-deployment/request-routing/gateway-api-inference-extension/quickstart) to deploy a `DynamoGraphDeployment`, expose it through Gateway API, and verify an end-to-end request through the Dynamo EPP.
Use [GAIE Reference](https://docs.nvidia.com/dynamo/v1.3.0/kubernetes-deployment/request-routing/gateway-api-inference-extension/reference) for resource contracts, routing knobs, and service mesh settings.
Ask AI
Assistant
Responses are generated using AI and may contain mistakes.
Hi, I'm an AI assistant with access to documentation and other content.
Tip: You can toggle this pane with
+
NVIDIA uses cookies to improve your experience on our web site. We and our third-party partners also use cookies and other tools to collect and record information you provide as well as information about your interactions with our websites for performance improvement, analytics, and to assist in marketing efforts. By clicking "Accept All", you consent to our use of cookies and other tools as described in our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/). You can manage your cookie settings by clicking on "Manage Settings." By continuing to use this site or by clicking one of the buttons below, you agree to our [Terms of Service](https://www.nvidia.com/en-us/about-nvidia/terms-of-service/) (which contains important waivers). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information on our privacy practices.
We have detected the Global Privacy Control (GPC) signal and recorded your rejection of all optional cookies on this site for this browser. You can manage your cookie settings by clicking on "Manage Settings". Please see our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) for more information. To opt out of non-cookie personal information "sales" / "sharing" for targeted advertising purposes, please visit the [NVIDIA Preference Center](https://www.nvidia.com/en-us/about-nvidia/privacy-center/). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information on our privacy practices.
We have detected the Global Privacy Control Signal (GPC) and have opted you out of all optional cookies on this browser. You can manage your cookie settings by clicking on "Manage Settings". Please see our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) for more information. We have also opted you out of "sharing"/"sales" of personal information outside of cookies. You can manage these settings in the NVIDIA [NVIDIA Preference Center](https://www.nvidia.com/en-us/privacy-center/). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information.
We have detected the Global Privacy Control Signal (GPC) and have opted you out of all optional cookies on this browser. You can manage your cookie settings by clicking on "Manage Settings". Please see our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) for more information. We have also opted you out of "sharing"/"sales" of personal information outside of cookies which overrides at least one of your previous settings. You can manage them in the [NVIDIA Preference Center](https://www.nvidia.com/en-us/privacy-center/). Please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information.
Manage Settings
Reject Optional Accept All
Cookie Settings
We and our third-party partners (including social media, advertising, and analytics partners) use cookies and other tracking technologies to collect, store, monitor, and process certain information about you when you visit our website. The information collected might relate to you, your preferences, or your device. We use that information to make the site work, analyze performance and traffic on our website, provide a more personalized web experience, and assist in our marketing efforts.Under certain privacy laws, you have the right to direct us not to "sell" or "share" your personal information for targeted advertising. To opt-out of the "sale" and "sharing" of personal information through cookies, you must opt-out of optional cookies using the toggles below. To opt out of the "sale" and "sharing" of data collected by other means (e.g., online forms) you must also update your data sharing preferences through the [NVIDIA Preference Center](https://www.nvidia.com/en-us/about-nvidia/privacy-center/).Click on the different category headings below to find out more and change the settings according to your preference. You cannot opt out of Required Cookies as they are deployed to ensure the proper functioning of our website (such as prompting the cookie banner and remembering your settings, etc.). By clicking "Save and Accept" or "Decline All" at the bottom, you consent to the use of cookies and other tools as described in our [Cookie Policy](https://www.nvidia.com/en-us/about-nvidia/cookie-policy/) in accordance with your settings and accept our [Terms of Service](https://www.nvidia.com/en-us/about-nvidia/terms-of-service/) (which contains important waivers). For more information about our privacy practices, please see our [Privacy Policy](https://www.nvidia.com/en-us/about-nvidia/privacy-policy/).
Required Cookies
Always Active
These cookies enable core functionality such as security, network management, and accessibility. These cookies are required for the site to function and cannot be turned off.
Cookies Details
Performance Cookies
Performance Cookies
These cookies are used to provide quantitative measures of our website visitors, such as the number of times you visit, time on page, your mouse movements, scrolling, clicks and keystroke activity on the websites; other browsing, search, or product research behavior; and what brought you to our site. These cookies may store a unique ID so that our system will remember you when you return. Information collected with these cookies is used to measure and find ways to improve website performance.
Cookies Details
Personalization Cookies
Personalization Cookies
These cookies collect data about how you have interacted with our website to help us improve your web experience, such as which pages you have visited. These cookies may store a unique ID so that our system will remember you when you return. They may be set by us or by third party providers whose services we have added to our pages. These cookies enable us to provide enhanced website functionality and personalization as well as make the marketing messages we send to you more relevant to your interests. If you do not allow these cookies, then some or all of these services may not function properly.
Cookies Details
Advertising Cookies
Advertising Cookies
These cookies record your visit to our websites, the pages you have visited and the links you have followed to influence the advertisements that you see on other websites. These cookies and the information they collect may be managed by other companies, including our advertising partners, and may be used to build a profile of your interests and show you relevant advertising on other sites. We and our advertising partners will use this information to make our websites and the advertising displayed on it, more relevant to your interests.
Cookies Details
Back Button
Cookie List
Search Icon
Filter Icon
Clear
  * checkbox label label


Apply Cancel
Consent Leg.Interest
checkbox label label
checkbox label label
checkbox label label
Decline All Save and Accept
