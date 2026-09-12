---
title: InferencePool - Kubernetes Gateway API Inference Extension
id: inferencepool-kubernetes-gateway-api-inference-extension
tags:
- llmd-router-nonk8s-backends
- inferencepool-spec
- epp
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:43:04.886640Z'
updated: '2026-09-12T03:37:37.518556Z'
source: https://gateway-api-inference-extension.sigs.k8s.io/api-types/inferencepool/
source_domain: gateway-api-inference-extension.sigs.k8s.io
fetched_at: '2026-09-11T19:43:04.867927Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'GIE docs page for InferencePool v1 (GA since v1.0.0). InferencePoolSpec
  has 3 major parts: selector (label selector matching Pod labels, e.g. matchLabels
  app: vllm-qwen3-32b — pods only, selection is by Kubernetes label match, no other
  backend kind is mentioned), targetPorts (port numbers on selected Pods the gateway
  routes to, e.g. number: 8000), and endpointPickerRef (references the EPP Service;
  became OPTIONAL as of v1.5.0 — before that it was required — to allow InferencePool
  without a user-managed EPP deployment). Example config shows failureMode: FailOpen
  on endpointPickerRef: if EPP fails/is unresponsive, request is forwarded to a gateway-chosen
  endpoint instead. States any Pod joining a pool must implement the Model Server
  Protocol so the EPP can route intelligently. Frames InferencePool as pool-level
  replacement for a Kubernetes Service, explicitly Pod-centric (''defines a group
  of Pods'').'
utility_score: 13.0
---

# Inference Pool[¶](https://gateway-api-inference-extension.sigs.k8s.io/api-types/inferencepool/#inference-pool "Permanent link")
GA since v1.0.0
The `InferencePool` resource has been graduated to v1 and is considered stable.
## Background[¶](https://gateway-api-inference-extension.sigs.k8s.io/api-types/inferencepool/#background "Permanent link")
The **InferencePool** API defines a group of Pods (containers) dedicated to serving AI models. Pods within an InferencePool share the same compute configuration, accelerator type, base language model, and model server. This abstraction simplifies the management of AI model serving resources, providing a centralized point of administrative configuration for Platform Admins.
An InferencePool is typically bundled with an Endpoint Picker extension. This extension is responsible for tracking key metrics on each model server (i.e. the KV-cache utilization, queue length of pending requests, active LoRA adapters, etc.) and routing incoming inference requests to the optimal model server replica based on these metrics. An EPP can only be associated with a single InferencePool, though an HTTPRoute may reference multiple InferencePools as backendRefs.
Additionally, any Pod that seeks to join an InferencePool would need to support the [model server protocol](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/003-model-server-protocol), defined by this project, to ensure the Endpoint Picker has adequate information to intelligently route requests.
Until release `v1.5.0`, the InferencePool field `endpointPickerRef` was required. Currently, it is optional, to allow usages of InferencePool without user-managed Endpoint Picker deployments. Consult the documentation of your Inference Gateway implementation to check if it supports omitting `endpointPickerRef`.
## How to Configure an InferencePool[¶](https://gateway-api-inference-extension.sigs.k8s.io/api-types/inferencepool/#how-to-configure-an-inferencepool "Permanent link")
The full spec of the InferencePool is defined [here](https://gateway-api-inference-extension.sigs.k8s.io/reference/spec/#inferencepool).
In summary, the InferencePoolSpec consists of 3 major parts:
  * The `selector` field specifies which Pods belong to this pool. The labels in this selector must exactly match the labels applied to your model server Pods. 
  * The `targetPorts` field defines the port numbers that the Inference Gateway should route to on model server Pods that belong to this pool. 
  * The `endpointPickerRef` field references the Endpoint Picker (EPP) service that monitors key metrics from model servers within the InferencePool and provides intelligent routing decisions.


### Example Configuration[¶](https://gateway-api-inference-extension.sigs.k8s.io/api-types/inferencepool/#example-configuration "Permanent link")
Here is an example InferencePool configuration:

```
apiVersion: inference.networking.k8s.io/v1
kind: InferencePool
metadata:
  name: vllm-qwen3-32b
spec:
  selector:
    matchLabels:
      app: vllm-qwen3-32b
  targetPorts:
    - number: 8000
  endpointPickerRef:
    name: vllm-qwen3-32b-epp
    port:
      number: 9002
    failureMode: FailOpen

```

In this example: 
  * An InferencePool named `vllm-qwen3-32b` is created in the `default` namespace.
  * It will select Pods that have the label `app: vllm-qwen3-32b`.
  * Traffic routed to this InferencePool will call out to the EPP service `vllm-qwen3-32b-epp` on port `9002` for making routing decisions. With `failureMode: FailOpen`, if EPP fails to pick an endpoint or is not responsive, the request is forwarded to an endpoint chosen by the gateway.
  * Traffic routed to this InferencePool will be forwarded to the port `8000` on the selected Pods.


## Overlap with Service[¶](https://gateway-api-inference-extension.sigs.k8s.io/api-types/inferencepool/#overlap-with-service "Permanent link")
**InferencePool** has some small overlap with **Service** , displayed here:
The InferencePool is not intended to be a mask of the Service object. It provides a specialized abstraction tailored for managing and routing traffic to groups of LLM model servers, allowing Platform Admins to focus on pool-level management rather than low-level networking details.
Back to top 
