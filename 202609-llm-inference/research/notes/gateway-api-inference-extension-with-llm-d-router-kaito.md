---
title: Gateway API Inference Extension with llm-d Router | KAITO
id: gateway-api-inference-extension-with-llm-d-router-kaito
tags:
- llmd-router
- llmd-router-nonk8s-backends-54b774
- epp-architecture
- kaito
created: '2026-09-11T19:46:23.287481Z'
updated: '2026-09-12T03:37:37.637523Z'
source: https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/
source_domain: kaito-project.github.io
fetched_at: '2026-09-11T19:46:23.268291Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'KAITO official docs (v0.12.x, ground-truth/docs tier) describing how KAITO
  integrates Gateway API Inference Extension (GWIE) with ''the llm-d Router EPP.''
  States plainly: ''the EPP is provided by the llm-d Router project, which builds
  on the GWIE EPP with advanced scheduling plugins including KV cache-aware routing,
  prefill/decode (P/D) disaggregation, and pluggable filters/scorers'' -- i.e. KAITO
  does not reimplement scheduling logic, it deploys the upstream llm-d-router-gateway
  artifact unmodified. Gives exact reuse mechanics: the KAITO InferenceSet controller
  uses Flux to deploy the llm-d-router-gateway Helm chart from ''oci://ghcr.io/llm-d/charts/llm-d-router-gateway''
  pinned to version v0.9.0, and the EPP container image is pinned to ''mcr.microsoft.com/oss/v2/llm-d/llm-d-router-endpoint-picker:v0.9.0''
  to match. This chart packages InferencePool + EPP together. KAITO explicitly configures
  the EPP to listen on plaintext gRPC (router.epp.flags.secure-serving=false) requiring
  an Istio DestinationRule with tls.mode: DISABLE for the Gateway->EPP ext_proc connection.
  Documents the full quickstart wiring: Istio Gateway with ENABLE_GATEWAY_API_INFERENCE_EXTENSION=true,
  InferenceSet CR triggering OCIRepository+HelmRelease creation, HTTPRoute targeting
  the InferencePool via .spec.endpointPickerRef, and optional Body-Based Routing (BBR,
  from kubernetes-sigs/gateway-api-inference-extension) that extracts model names
  from OpenAI-style request bodies into an X-Gateway-Model-Name header for multi-model
  routing without client changes. This is one of the cleanest documented examples
  of ''wrap, don''t fork'' reuse: KAITO''s only original code is the InferenceSet
  controller and Flux wiring; the router/EPP itself is consumed as a versioned, pinned
  upstream artifact.'
utility_score: 9.0
---

[Skip to main content](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#__docusaurus_skipToContent_fallback)
⭐️ If you like KAITO, please give it a star on [GitHub](https://github.com/kaito-project/kaito)!
Version: v0.12.x
On this page
KAITO integrates with [Gateway API Inference Extension](https://gateway-api-inference-extension.sigs.k8s.io/) (GWIE) and the [llm-d Router](https://github.com/llm-d/llm-d-router) EPP to provide model-aware routing and optimal endpoint selection for inference. This page covers what it is, prerequisites, how to enable it in KAITO, how it's wired, and a quickstart.
## What is it[​](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#what-is-it "Direct link to What is it")
Gateway API Inference Extension extends [Gateway API](https://gateway-api.sigs.k8s.io/) with inference-focused backends and behaviors. It adds:
  * [InferencePool](https://gateway-api-inference-extension.sigs.k8s.io/api-types/inferencepool/) CRD to represent model-serving backends
  * A reference Endpoint Picker Plugin (EPP) that uses inference server metrics and policies to pick the best backend. In KAITO, the EPP is provided by the [llm-d Router](https://github.com/llm-d/llm-d-router) project, which builds on the GWIE EPP with advanced scheduling plugins including KV cache-aware routing, prefill/decode (P/D) disaggregation, and pluggable filters/scorers.
  * Optional [Body-Based Routing](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/pkg/bbr) (BBR) that extracts model names from OpenAI-style requests and injects a header for routing purposes


KAITO uses GWIE to route requests for models to the right Workspace pods, improving latency and GPU utilization.
## Prerequisites[​](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#prerequisites "Direct link to Prerequisites")
Before enabling this feature in KAITO, ensure the following are installed in your cluster:
  * A Gateway API implementation that supports Envoy ext_proc and the Inference Extension pattern. See available Gateway implementations: <https://gateway-api-inference-extension.sigs.k8s.io/implementations/gateways/>


## Enable this feature[​](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#enable-this-feature "Direct link to Enable this feature")
This feature is supported from KAITO v0.8.0. Starting from **KAITO v0.11.0** , the InferenceSet feature has been promoted to **beta** and the `enableInferenceSetController` feature gate is enabled by default — you only need to enable the `gatewayAPIInferenceExtension` feature gate.
### KAITO v0.11.0+[​](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#kaito-v0110 "Direct link to KAITO v0.11.0+")

```


exportCLUSTER_NAME=kaito





helm repo add kaito https://kaito-project.github.io/kaito/charts/kaito




helm repo update




helm upgrade --install kaito-workspace kaito/workspace \




--namespace kaito-workspace \




  --create-namespace \




--setclusterName="$CLUSTER_NAME"\




--setfeatureGates.gatewayAPIInferenceExtension=true \




--wait\




  --take-ownership


```

### KAITO v0.8.0 – v0.10.x[​](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#kaito-v080--v010x "Direct link to KAITO v0.8.0 – v0.10.x")
For older versions, you need to explicitly enable both feature gates:

```


exportCLUSTER_NAME=kaito





helm repo add kaito https://kaito-project.github.io/kaito/charts/kaito




helm repo update




helm upgrade --install kaito-workspace kaito/workspace \




--namespace kaito-workspace \




  --create-namespace \




--setclusterName="$CLUSTER_NAME"\




--setfeatureGates.gatewayAPIInferenceExtension=true \




--setfeatureGates.enableInferenceSetController=true \




--wait\




  --take-ownership


```

## How KAITO wires it[​](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#how-kaito-wires-it "Direct link to How KAITO wires it")
When the feature gate is enabled, [Flux](https://fluxcd.io/) will be installed in the same namespace as the InferenceSet controller as a Helm dependency. It is used to deploy and manage the [llm-d-router-gateway](https://github.com/llm-d/llm-d-router) Helm chart (which packages the InferencePool + EPP together) for each InferenceSet.
When you create an InferenceSet, the KAITO InferenceSet controller will:
  1. Create or update two Flux resources in the InferenceSet namespace: 
     * [OCIRepository](https://fluxcd.io/flux/components/source/ocirepositories/): points to the llm-d-router-gateway Helm chart 
       * URL: `oci://ghcr.io/llm-d/charts/llm-d-router-gateway`
       * Version: `v0.9.0` (pinned by KAITO; may be updated or made configurable in future releases)
     * [HelmRelease](https://fluxcd.io/flux/components/helm/helmreleases/): references the OCIRepository and applies values to deploy the InferencePool and EPP. The EPP image is pinned to [`mcr.microsoft.com/oss/v2/llm-d/llm-d-router-endpoint-picker:v0.9.0`](https://github.com/llm-d/llm-d-router) to match the chart version.
  2. Wait for Flux resources to become Ready


You can inspect these resources with kubectl in the InferenceSet namespace. Updates to the InferenceSet will reconcile these resources.
## Quickstart[​](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#quickstart "Direct link to Quickstart")
In this quickstart example, we will use Istio as the Gateway API provider to handle traffic management and routing, and deploy KAITO InferenceSet to serve inference models. The following steps demonstrate how to set up an end-to-end inference gateway that routes requests to model-serving backends managed by KAITO.
### 1. Install Istio and Deploy Gateway[​](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#1-install-istio-and-deploy-gateway "Direct link to 1. Install Istio and Deploy Gateway")
First, install Istio base and control plane components, setting flags that enable Gateway API Inference Extension support in the data plane and pilot:

```


helm repo add istio https://istio-release.storage.googleapis.com/charts




helm repo update




helm upgrade -i istio-base istio/base \




--version1.28.3 \




--namespace istio-system \




  --create-namespace




helm upgrade -i istiod istio/istiod \




--version1.28.3 \




--namespace istio-system \




--setpilot.env.ENABLE_GATEWAY_API_INFERENCE_EXTENSION="true"\




--wait


```

Then, deploy Gateway API CRDs and create the Gateway resource that will handle incoming requests and integrate with GWIE per the example configuration:

```


kubectl apply -k"github.com/kubernetes-sigs/gateway-api/config/crd?ref=v1.4.1"




kubectl apply -f https://raw.githubusercontent.com/kaito-project/kaito/refs/heads/main/examples/gateway-api-inference-extension/gateway.yaml


```

### 2. Deploy InferenceSet[​](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#2-deploy-inferenceset "Direct link to 2. Deploy InferenceSet")
Create a sample KAITO InferenceSet (using a vLLM preset) that will host the model server behind the inference gateway:

```


kubectl apply -f https://raw.githubusercontent.com/kaito-project/kaito/refs/heads/main/examples/inference/kaito_inferenceset_phi_4_mini.yaml


```

Once the InferenceSet is created, verify that Flux's OCIRepository and HelmRelease resources are ready in the InferenceSet namespace:

```


kubectl get ocirepository,helmrelease





NAME                                                              URL                                                            READY   STATUS                                                                                                        AGE




ocirepository.source.toolkit.fluxcd.io/phi-4-mini-inferencepool   oci://ghcr.io/llm-d/charts/llm-d-router-gateway                True    stored artifact for digest 'v0.9.0@sha256:...'





helmrelease.helm.toolkit.fluxcd.io/phi-4-mini-inferencepool   32s   True    Helm install succeeded for release default/phi-4-mini-inferencepool.v1 with chart llm-d-router-gateway@0.9.0


```

Verify that the InferencePool resource is created:

```


kubectl get inferencepool





NAME                       AGE




phi-4-mini-inferencepool   69s


```

Verify that the Endpoint Picker (EPP) Pod is running in the InferenceSet namespace:

```


kubectl get pod -l app.kubernetes.io/name=phi-4-mini-inferencepool-epp





NAME                                           READY   STATUS    RESTARTS   AGE




phi-4-mini-inferencepool-epp-b74f8994b-s9kkt   1/1     Running   0          87s


```

Confirm the EPP is using the llm-d Router EPP image:

```


kubectl get pod -l app.kubernetes.io/name=phi-4-mini-inferencepool-epp -ojsonpath='{.items[0].spec.containers[0].image}'




# Expected: mcr.microsoft.com/oss/v2/llm-d/llm-d-router-endpoint-picker:v0.9.0


```

### 3. Deploy DestinationRule and HTTPRoute[​](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#3-deploy-destinationrule-and-httproute "Direct link to 3. Deploy DestinationRule and HTTPRoute")
Apply an Istio DestinationRule for the EPP service. In KAITO, the EPP is explicitly configured to listen on **plaintext** gRPC (`router.epp.flags.secure-serving=false`), so the DestinationRule sets `tls.mode: DISABLE` for the Istio Gateway → EPP ext_proc connection:

```


kubectl apply -f https://raw.githubusercontent.com/kaito-project/kaito/refs/heads/main/examples/gateway-api-inference-extension/destinationrule-phi-4-mini-instruct.yaml


```

Create the HTTPRoute that targets the InferenceSet's InferencePool (via `.spec.endpointPickerRef`) and defines the routing matchers used by the Gateway:

```


kubectl apply -f https://raw.githubusercontent.com/kaito-project/kaito/refs/heads/main/examples/gateway-api-inference-extension/httproute.yaml


```

### 4. Test Inference[​](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#4-test-inference "Direct link to 4. Test Inference")
Verify that the HTTPRoute is properly configured and accepted by the Gateway:

```


kubectl describe httproute llm-route






Status:




  Parents:




    Conditions:




      Last Transition Time:  2025-12-02T08:59:58Z




      Message:               Route was valid




      Observed Generation:   2




      Reason:                Accepted




      Status:                True




      Type:                  Accepted




      Last Transition Time:  2025-12-03T13:35:59Z




      Message:               All references resolved




      Observed Generation:   2




      Reason:                ResolvedRefs




      Status:                True




      Type:                  ResolvedRefs




    Controller Name:         istio.io/gateway-controller




    Parent Ref:




      Group:  gateway.networking.k8s.io




      Kind:   Gateway




      Name:   inference-gateway



```

Verify that the InferencePool is properly configured and ready to accept traffic by checking its status conditions:

```


kubectl describe inferencepool phi-4-mini-inferencepool






    Conditions:




      Last Transition Time:  2025-12-03T13:35:59Z




      Message:               Referenced by an HTTPRoute accepted by the parentRef Gateway




      Observed Generation:   1




      Reason:                Accepted




      Status:                True




      Type:                  Accepted




      Last Transition Time:  2025-12-03T13:35:59Z




      Message:               Referenced ExtensionRef resolved successfully




      Observed Generation:   1




      Reason:                ResolvedRefs




      Status:                True




      Type:                  ResolvedRefs



```

Get the ClusterIP of the Istio Gateway service to enable internal cluster routing:

```


kubectl get service





NAME                      TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)




inference-gateway-istio   ClusterIP      10.0.249.124                   15021:31583/TCP,80:30314/TCP   13m


```

Export the ClusterIP for easy access and test the inference endpoint using a temporary curl pod:

```


exportCLUSTERIP=$(kubectl get svc inference-gateway-istio -ojsonpath='{.spec.clusterIP}')




kubectl run -it--rm--restart=Never curl--image=curlimages/curl -- curl-s  http://$CLUSTERIP/v1/models | jq






"data":[





"created":1764772194,




"id":"phi-4-mini-instruct",




"max_model_len":131072,




"object":"model",




"owned_by":"vllm",




"parent": null,




"permission":[





"allow_create_engine": false,




"allow_fine_tuning": false,




"allow_logprobs": true,




"allow_sampling": true,




"allow_search_indices": false,




"allow_view": true,




"created":1764772194,




"group": null,




"id":"modelperm-c535582bbc454bbd93cc3cf370318635",




"is_blocking": false,




"object":"model_permission",




"organization":"*"






"root":"/workspace/vllm/weights"






"object":"list"



```

Send a chat completion request to test the inference endpoint:

```


kubectl run -it--rm--restart=Never curl--image=curlimages/curl -- curl-X POST http://$CLUSTERIP/v1/chat/completions \




-H"Content-Type: application/json"\





    "model": "phi-4-mini-instruct",




    "messages": [{"role": "user", "content": "What is kubernetes?"}],




    "max_tokens": 50




  }'| jq






"choices":[





"finish_reason":"length",




"index":0,




"logprobs": null,




"message":{




"annotations": null,




"audio": null,




"content":"Kubernetes, often abbreviated as K8s, is an open-source platform designed to automate the deployment, scaling, and operations of containerized applications. Developed by Google and donated to the Cloud Native Computing Foundation (CNCF), Kubernetes provides an orches",




"function_call": null,




"reasoning_content": null,




"refusal": null,




"role":"assistant",




"tool_calls":[]





"stop_reason": null






"created":1764815768,




"id":"chatcmpl-d503646c-154d-4413-ba1c-cccba73effa7",




"kv_transfer_params": null,




"model":"phi-4-mini-instruct",




"object":"chat.completion",




"prompt_logprobs": null,




"service_tier": null,




"system_fingerprint": null,




"usage":{




"completion_tokens":50,




"prompt_tokens":17,




"prompt_tokens_details": null,




"total_tokens":67




```

### 4. [Optional] Deploy BBR[​](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#4-optional-deploy-bbr "Direct link to 4. \[Optional\] Deploy BBR")
Deploy a second KAITO InferenceSet and DestinationRule with a different model to demonstrate multi-model routing. This step uses [`mistral-7b-instruct`](https://github.com/kaito-project/kaito/blob/main/examples/inference/kaito_inferenceset_mistral_7b-instruct.yaml) as an example:

```


kubectl apply -f https://raw.githubusercontent.com/kaito-project/kaito/refs/heads/main/examples/inference/kaito_inferenceset_mistral_7b-instruct.yaml




kubectl apply -f https://raw.githubusercontent.com/kaito-project/kaito/refs/heads/main/examples/gateway-api-inference-extension/destinationrule-mistral-7b-instruct.yaml


```

Install the Body-Based Routing (BBR) Helm chart. BBR automatically extracts model names from OpenAI-style API requests and injects an `X-Gateway-Model-Name` header to the inference request, enabling model routing without modifying client code:

```


helm upgrade --install body-based-router oci://registry.k8s.io/gateway-api-inference-extension/charts/body-based-routing \




--version v1.3.0 \




--setprovider.name=istio \




--wait


```

Update the HTTPRoute to use header-based matching so requests are routed by the model name found in the request body:

```


kubectl apply -f https://raw.githubusercontent.com/kaito-project/kaito/refs/heads/main/examples/gateway-api-inference-extension/httproute-bbr.yaml


```

Verify routing with the original model name; the gateway should route to the corresponding Workspace via the InferencePool:

```


kubectl run -it--rm--restart=Never curl--image=curlimages/curl -- curl-X POST http://$CLUSTERIP/v1/chat/completions \




-H"Content-Type: application/json"\





    "model": "phi-4-mini-instruct",




    "messages": [{"role": "user", "content": "What is kubernetes?"}],




    "max_tokens": 50




  }'| jq






"choices":[





"finish_reason":"length",




"index":0,




"logprobs": null,




"message":{




"annotations": null,




"audio": null,




"content":"Kubernetes, often abbreviated as K8s, is an open-source container orchestration platform designed to automate the deployment, scaling, and management of containerized applications. It was originally developed by Google and is now maintained by the Cloud Native Computing Foundation (",




"function_call": null,




"reasoning_content": null,




"refusal": null,




"role":"assistant",




"tool_calls":[]





"stop_reason": null






"created":1764818961,




"id":"chatcmpl-e344b96b-94d2-4e6b-b922-05b312dd02bd",




"kv_transfer_params": null,




"model":"phi-4-mini-instruct",




"object":"chat.completion",




"prompt_logprobs": null,




"service_tier": null,




"system_fingerprint": null,




"usage":{




"completion_tokens":50,




"prompt_tokens":17,




"prompt_tokens_details": null,




"total_tokens":67




```

Now, send the same request but change the model name to `mistral-7b-instruct` to verify BBR-driven model-aware routing across multiple InferenceSets:

```


kubectl run -it--rm--restart=Never curl--image=curlimages/curl -- curl-X POST http://$CLUSTERIP/v1/chat/completions \




-H"Content-Type: application/json"\





    "model": "mistral-7b-instruct",




    "messages": [{"role": "user", "content": "What is kubernetes?"}],




    "max_tokens": 50




  }'| jq






"choices":[





"finish_reason":"length",




"index":0,




"logprobs": null,




"message":{




"annotations": null,




"audio": null,




"content":" Kubernetes (also known as K8s) is an open-source platform designed to automate deployment, scaling, and management of containerized applications. It groups containers that make up an application into logical units for easy management, and helps to ensure",




"function_call": null,




"reasoning_content": null,




"refusal": null,




"role":"assistant",




"tool_calls":[]





"stop_reason": null






"created":1756237560,




"id":"chatcmpl-b563a6a5-8009-43e9-aedc-4f4238d8c6b8",




"kv_transfer_params": null,




"model":"mistral-7b-instruct",




"object":"chat.completion",




"prompt_logprobs": null,




"service_tier": null,




"system_fingerprint": null,




"usage":{




"completion_tokens":50,




"prompt_tokens":8,




"prompt_tokens_details": null,




"total_tokens":58




```

  * [Enable this feature](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#enable-this-feature)
    * [KAITO v0.8.0 – v0.10.x](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#kaito-v080--v010x)
  * [Quickstart](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#quickstart)
    * [1. Install Istio and Deploy Gateway](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#1-install-istio-and-deploy-gateway)
    * [2. Deploy InferenceSet](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#2-deploy-inferenceset)
    * [3. Deploy DestinationRule and HTTPRoute](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#3-deploy-destinationrule-and-httproute)
    * [4. [Optional] Deploy BBR](https://kaito-project.github.io/kaito/docs/gateway-api-inference-extension/#4-optional-deploy-bbr)


