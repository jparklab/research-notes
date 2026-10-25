---
title: gateway-api-inference-extension/site-src/guides/standalone.md at 91ad0da44f64d41642d55e220d239648eef06196
  · kubernetes-sigs/gateway-api-inference-extension · GitHub
id: gateway-api-inference-extensionsite-srcguidesstandalonemd-at-91ad0da44f64d41642d
tags:
- llmd-router-nonk8s
- llmd-router-nonk8s-backends-54b774
- standalone-mode
- epp-flags
created: '2026-09-11T19:48:18.921741Z'
updated: '2026-09-15T04:55:23.806427Z'
source: https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/91ad0da44f64d41642d55e220d239648eef06196/site-src/guides/standalone.md
source_domain: github.com
fetched_at: '2026-09-11T19:48:18.897863Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Last surviving version of site-src/guides/standalone.md (commit 91ad0da4,
  the parent of deletion-commit ebbb148c dated 2026-06-30) -- the file ''Deploy As
  A Standalone Request Scheduler'' guide, fetched as a primary-source substitute since
  the live docs page now returns empty content. States the EPP is ''a smart request
  scheduler for LLM requests'' implementing prefix-cache-aware and load-aware scheduling;
  normally it runs as an ext-proc service fronting a proxy (GKE L7LB, Istio, agentgateway)
  for k8s-cluster model servers, which suits multi-tenant MaaS but is ''in practice
  an operational overhead'' for single-tenant workloads like an offline RL post-training
  job sampler. Standalone mode solves this by deploying a proxy (Envoy or agentgateway)
  as a sidecar to the EPP communicating over localhost, via a Helm chart (oci://us-central1-docker.pkg.dev/k8s-staging-images/gateway-api-inference-extension/charts/standalone).
  Two configuration sub-modes, both still built around k8s: ''With Inference APIs
  Support'' -- EPP configured via the InferencePool CRD, full InferenceObjective/priority
  support; and ''Without Inference APIs Support'' -- ''EPP is configured using command
  line flags. This is the simplest method for standalone jobs which does not require
  installing the inference extension apis... no support for the features expressed
  using the inference APIs.'' Even flag-only mode still targets k8s Pods via the Helm
  value inferenceExtension.endpointsServer.endpointSelector=app=vllm-qwen3-32b (a
  pod label selector) with inferenceExtension.endpointsServer.createInferencePool=false;
  there is no non-k8s endpoint source documented for standalone mode. Explicitly states
  Agentgateway standalone does not support InferencePool. The whole guide, including
  this flag-only path, was deleted from the docs site in the 2026-06-30 cleanup with
  no direct replacement page found.'
utility_score: 16.0
---

*Suggested by [[github-kubernetes-sigsgateway-api-inference-extension-gateway-api-inference-exte]] — last version of standalone.md before it was removed from the docs site in commit ebbb148c*

[Skip to content](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/91ad0da44f64d41642d55e220d239648eef06196/site-src/guides/standalone.md#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/91ad0da44f64d41642d55e220d239648eef06196/site-src/guides/standalone.md) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/91ad0da44f64d41642d55e220d239648eef06196/site-src/guides/standalone.md) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/91ad0da44f64d41642d55e220d239648eef06196/site-src/guides/standalone.md) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/91ad0da44f64d41642d55e220d239648eef06196/site-src/guides/standalone.md).
/ **[gateway-api-inference-extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension) You must be signed in to change notification settings
  * [ Fork 313 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)
  * [ Star  766 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)


## Collapse file tree
## Files
File does not exist on default branch
Search this repository(forward slash)` forward slash/`
/
# standalone.md
Copy path
More file actions
More file actions
## Latest commit
[Support agentgateway standalone sidecar (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/7d8f75061ccb8cfdfdacd872330a9abda78026be)[#2905](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2905)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/7d8f75061ccb8cfdfdacd872330a9abda78026be)
Open commit detailssuccess
May 2, 2026
[7d8f750](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/7d8f75061ccb8cfdfdacd872330a9abda78026be) · May 2, 2026
## History
[History](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commits/91ad0da44f64d41642d55e220d239648eef06196/site-src/guides/standalone.md)
Open commit details
History
185 lines (141 loc) · 8.75 KB
/
# standalone.md
Copy path
## File metadata and controls
  * Preview
  * 

185 lines (141 loc) · 8.75 KB
Copy raw file
Download raw file
Outline
Edit and raw actions
# Deploy As A Standalone Request Scheduler
The endpoint picker (EPP) at its core is a smart request scheduler for LLM requests, it currently implements a number of LLM-specific load balancing optimizations including:
  * Prefix-cache aware scheduling
  * Load-aware scheduling


When using EPP with Gateway API, it works as an ext-proc service for a proxy fronting model servers running in a k8s cluster; examples of such proxies are cloud managed ones like GKE’s L7LB, open source counterparts like Istio and agentgateway. EPP as an ext-proc here offers several key advantages:
  * It utilizes robust, pre-existing L7 proxies, including both managed and open-source options.
  * Seamless integration with the Kubernetes networking ecosystem, the Gateway API, allows for: 
    * Transforming a Kubernetes gateway into an inference scheduler using familiar APIs.
    * Traffic splitting for gradual roll-outs and HTTP rule matching.
    * Access to provider-specific features.


These benefits are critical for online services, including MaaS (Model-as-a-Service), which require support for multi-tenancy, demand high availability, scalability, and streamlined operations.
However, for some batch inference, a tight integration with the Gateway API and requiring an external proxy to be deployed separately is in practice an operational overhead. Consider an offline RL post-training job, where the sampler, the inference service in the job, is a single tenant/workload with a lifecycle tied with the training job; this inference service is specific to the job, it is continuously updated during post-training, and so it is not one that would be serving any other traffic. A simpler deployment mode would reduce the barrier to adopting the EPP for such single-tenant workloads.
A proxy is deployed as a sidecar to the EPP. The proxy and EPP communicate over localhost. The standalone chart currently provides built-in sidecar presets for Envoy and agentgateway. Envoy mirrors the existing ext-proc sidecar flow. Envoy supports two endpoint discovery modes:
  * **With Inference APIs Support** : The EPP is configured using the Inference CRDs, the pool is expressed using an instance of the InferencePool API and the entire suite of inference APIs are supported, including the use of InferenceObjectives for defining priorities.
  * **Without Inference APIs Support** : The EPP is configured using command line flags. This is the simplest method for standalone jobs which doesn't require installing the inference extension apis, which means no support for the features expressed using the inference APIs (such as InferenceObjectives).


Agentgateway standalone does not support `InferencePool`.
## Example
### **Prerequisites**
--8<-- "site-src/_includes/prereqs.md"
### **Steps**
#### Deploy Sample Model Server
--8<-- "site-src/_includes/vllm-gpu.md"

```
```bash
kubectl create secret generic hf-token --from-literal=token=$HF_TOKEN # Your Hugging Face Token with access to the set of Qwen models
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api-inference-extension/raw/main/config/manifests/vllm/gpu-deployment.yaml
```

```

--8<-- "site-src/_includes/model-server-cpu.md"

```
```bash
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api-inference-extension/raw/main/config/manifests/vllm/cpu-deployment.yaml
```

```

--8<-- "site-src/_includes/model-server-sim.md"

```
```bash
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api-inference-extension/raw/main/config/manifests/vllm/sim-deployment.yaml
```

```

#### Deploy Endpoint Picker Extension with a sidecar proxy
Choose one of the following proxy options to deploy an Endpoint Picker Extension with standalone request routing.
=== "Envoy"

```
  Envoy remains the default standalone sidecar and preserves the existing chart behavior.

  **With Inference APIs Support**

  Deploy an InferencePool named `vllm-qwen3-32b` that selects from endpoints with label `app=vllm-qwen3-32b`
  and listening on port `8000`. The Helm install command automatically deploys an InferencePool instance,
  the EPP, and provider-specific resources.

  ```bash
  # Install the Inference Extension CRDs
  kubectl apply -k https://github.com/kubernetes-sigs/gateway-api-inference-extension/config/crd

  export STANDALONE_CHART_VERSION=v0
  export PROVIDER=<YOUR_PROVIDER> # optional, can be gke if you need GKE-specific monitoring resources
  helm install vllm-qwen3-32b-standalone \
  --dependency-update \
  --set inferencePool.modelServers.matchLabels.app=vllm-qwen3-32b \
  --set provider.name=$PROVIDER \
  --version $STANDALONE_CHART_VERSION \
  oci://us-central1-docker.pkg.dev/k8s-staging-images/gateway-api-inference-extension/charts/standalone
  ```

  **Without Inference APIs Support**

  Deploy an Endpoint Picker Extension named `vllm-qwen3-32b` that selects from endpoints with label
  `app=vllm-qwen3-32b` and listening on port `8000`. The Helm install command automatically deploys the EPP
  along with provider-specific resources.

  ```bash
  export STANDALONE_CHART_VERSION=v0
  export PROVIDER=<YOUR_PROVIDER> # optional, can be gke if you need GKE-specific monitoring resources
  helm install vllm-qwen3-32b-standalone \
  --dependency-update \
  --set inferenceExtension.endpointsServer.endpointSelector="app=vllm-qwen3-32b" \
  --set inferenceExtension.endpointsServer.createInferencePool=false \
  --set provider.name=$PROVIDER \
  --version $STANDALONE_CHART_VERSION \
  oci://us-central1-docker.pkg.dev/k8s-staging-images/gateway-api-inference-extension/charts/standalone
  ```

```

=== "Agentgateway"

```
  Agentgateway can also run as the standalone sidecar proxy. Configure the model pods with
  `inferenceExtension.endpointsServer.endpointSelector`, and set
  `inferenceExtension.endpointsServer.createInferencePool=false`.
  `InferencePool` is not supported with agentgateway in standalone mode.

  **Note:** The chart defaults to `cr.agentgateway.dev/agentgateway:latest-dev` on `main` for this preset.
  Release tooling rewrites this to a stable Agentgateway tag when cutting a release.

  Example install:

  ```bash
  export STANDALONE_CHART_VERSION=v0
  export PROVIDER=<YOUR_PROVIDER> # optional, can be gke if you need GKE-specific monitoring resources
  helm install vllm-qwen3-32b-standalone \
  --dependency-update \
  --set inferenceExtension.sidecar.proxyType=agentgateway \
  --set inferenceExtension.endpointsServer.endpointSelector="app=vllm-qwen3-32b" \
  --set inferenceExtension.endpointsServer.createInferencePool=false \
  --set-string inferenceExtension.flags.secure-serving=false \
  --set provider.name=$PROVIDER \
  --version $STANDALONE_CHART_VERSION \
  oci://us-central1-docker.pkg.dev/k8s-staging-images/gateway-api-inference-extension/charts/standalone
  ```

```

#### Try it out
Wait until the EPP deployment is ready.
Once the EPP pod is running, Install the curl pod as follows:

```
cat EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: curl
  labels:
    app: curl
spec:
  containers:
  - name: curl
    image: curlimages/curl:7.83.1
    imagePullPolicy: IfNotPresent
    command:
      - tail
      - -f
      - /dev/null
  restartPolicy: Never

```

Send an inference request via

```
kubectl exec curl -- curl -i http://vllm-qwen3-32b-standalone-epp:8081/v1/completions \
-H 'Content-Type: application/json' \
-d '{"model": "Qwen/Qwen3-32B","prompt": "Write as if you were a critic: San Francisco","max_tokens": 100,"temperature": 0}'
```

#### Cleanup
Run the following commands to remove all resources created by this guide.
The following instructions assume you would like to cleanup ALL resources that were created in this guide. Please be careful not to delete resources you'd like to keep.
  1. Uninstall the EPP, curl pod and model server resources:

```
helm uninstall vllm-qwen3-32b-standalone
kubectl delete -f https://github.com/kubernetes-sigs/gateway-api-inference-extension/raw/main/config/manifests/inferenceobjective.yaml --ignore-not-found
kubectl delete -f https://github.com/kubernetes-sigs/gateway-api-inference-extension/raw/main/config/manifests/vllm/cpu-deployment.yaml --ignore-not-found
kubectl delete -f https://github.com/kubernetes-sigs/gateway-api-inference-extension/raw/main/config/manifests/vllm/gpu-deployment.yaml --ignore-not-found
kubectl delete -f https://github.com/kubernetes-sigs/gateway-api-inference-extension/raw/main/config/manifests/vllm/sim-deployment.yaml --ignore-not-found
kubectl delete -k https://github.com/kubernetes-sigs/gateway-api-inference-extension/config/crd --ignore-not-found
kubectl delete secret hf-token --ignore-not-found
kubectl delete pod curl --ignore-not-found
```



You can’t perform that action at this time. 
