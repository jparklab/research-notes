---
title: Gateway API Inference Extension | NGINX Documentation
id: gateway-api-inference-extension-nginx-documentation
tags:
- llmd-router-nonk8s-backends
- nginx
- gie
- inferencepool
- k8s-only
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:44:51.599824Z'
updated: '2026-09-12T03:37:37.585499Z'
source: https://docs.nginx.com/nginx-gateway-fabric/how-to/gateway-api-inference-extension/
source_domain: docs.nginx.com
fetched_at: '2026-09-11T19:44:51.576331Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'NGINX Gateway Fabric (NGF) how-to for GIE integration, confirming NGF''s
  Inference Extension support is also Kubernetes/InferencePool-bound: enabled via
  a Helm value (''nginxGateway.gwAPIInferenceExtension.enable=true'') or a container
  flag plus RBAC granting access to the ''inferencepools'' CRD, and the InferencePool
  is installed as a Helm chart (''oci://registry.k8s.io/gateway-api-inference-extension/charts/inferencepool'')
  that auto-deploys the Endpoint Picker Extension and selects pods via ''inferencePool.modelServers.matchLabels''.
  No standalone/non-k8s backend mode is documented; endpoint selection is pod-label-based
  only. Explicitly warns that NGF disables TLS certificate verification by default
  when talking to the third-party Endpoint Picker Extension and disclaims responsibility
  for risks from that EPP application — a relevant security caveat given the broader
  concern about trusting EPP-returned destinations. Links to llm-d''s llm-d-router
  repo for Endpoint Picker documentation and llm-d-inference-sim for the reference
  vLLM simulator used in the walkthrough.'
utility_score: 9.0
---

> NGINX Gateway Fabric 2.7.0 is now available.
> Follow [these instructions](https://docs.nginx.com/nginx-gateway-fabric/install/upgrade-version/#upgrade-from-v1x-to-v2x) to upgrade from 1.x to 2.0.
> For 1.x, checkout [an older version](https://docs.nginx.com/nginx-gateway-fabric/install/upgrade-version/#access-nginx-gateway-fabric-1x-documentation) of documentation.
# Gateway API Inference Extension
Learn how to use NGINX Gateway Fabric with the Gateway API Inference Extension to optimize traffic routing to self-hosting Generative AI Models on Kubernetes.
## 
The [Gateway API Inference Extension](https://gateway-api-inference-extension.sigs.k8s.io/) is an official Kubernetes project that aims to provide optimized load-balancing for self-hosted Generative AI Models on Kubernetes. The project’s goal is to improve and standardize routing to inference workloads across the ecosystem.
Coupled with the provided Endpoint Picker Service, NGINX Gateway Fabric becomes an [Inference Gateway](https://gateway-api-inference-extension.sigs.k8s.io/#concepts-and-definitions), with additional AI specific traffic management features such as model-aware routing, serving priority for models, model rollouts, and more.
## 
Install the Gateway API Inference Extension CRDs:
Copy

```
kubectl kustomize "https://github.com/nginx/nginx-gateway-fabric/config/crd/inference-extension/?ref=v2.7.0" | kubectl apply -f -
```

To enable the Gateway API Inference Extension, [install](https://docs.nginx.com/nginx-gateway-fabric/install/) NGINX Gateway Fabric with these modifications:
  * Using Helm: set the `nginxGateway.gwAPIInferenceExtension.enable=true` Helm value.
  * Using Kubernetes manifests: set the `--gateway-api-inference-extension` flag in the nginx-gateway container argument, update the ClusterRole RBAC to add the `inferencepools`:


yaml
Copy

```
- apiGroups:- inference.networking.k8s.ioresources:- inferencepools- apiGroups:- inference.networking.k8s.ioresources:- inferencepools/status- update
```

See this [example manifest](https://raw.githubusercontent.com/nginx/nginx-gateway-fabric/main/deploy/inference/deploy.yaml) for clarification.
## 
[ Deploy a sample model server ](https://docs.nginx.com/nginx-gateway-fabric/how-to/gateway-api-inference-extension/#deploy-a-sample-model-server "Deploy a sample model server")
The [vLLM simulator](https://github.com/llm-d/llm-d-inference-sim/tree/main) model server does not use GPUs and is ideal for test/development environments. To deploy the vLLM simulator, run the following command:
Copy

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/gateway-api-inference-extension/refs/tags/v1.5.0/config/manifests/vllm/sim-deployment.yaml
```

## 
[ Deploy the InferencePool and Endpoint Picker Extension ](https://docs.nginx.com/nginx-gateway-fabric/how-to/gateway-api-inference-extension/#deploy-the-inferencepool-and-endpoint-picker-extension "Deploy the InferencePool and Endpoint Picker Extension")
The InferencePool is a Gateway API Inference Extension resource that represents a set of Inference-focused Pods. With InferencePool, you can configure a routing extension as well as inference-specific routing optimizations. For more information on this resource, refer to the Gateway API Inference Extension [InferencePool documentation](https://gateway-api-inference-extension.sigs.k8s.io/api-types/inferencepool/).
Install an InferencePool named `vllm-qwen3-32b` that selects from endpoints with label `app: vllm-qwen3-32b` and listening on port 8000. The Helm install command automatically installs the Endpoint Picker Extension and InferencePool.
NGINX will query the Endpoint Picker Extension to determine the appropriate pod endpoint to route traffic to. These pods are selected from a pool of ready pods designated by the assigned InferencePool’s Selector field. For more information on the [Endpoint Picker](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/README.md).
> Warning 
> The Endpoint Picker Extension is a third-party application written and provided by the Gateway API Inference Extension project. Communication between NGINX and the Endpoint Picker uses TLS with certificate verification disabled by default. NGINX Gateway Fabric is not responsible for any threats or risks associated with using this third-party Endpoint Picker Extension application.
shell
Copy

```
export IGW_CHART_VERSION=v1.5.0
helm install vllm-qwen3-32b \
--dependency-update \
--set inferencePool.modelServers.matchLabels.app=vllm-qwen3-32b \
--version $IGW_CHART_VERSION \
--set inferenceExtension.resources.requests.memory=4Gi \
oci://registry.k8s.io/gateway-api-inference-extension/charts/inferencepool
```

> For test environments, lower the CPU and memory requests and limits to reduce resource use:
> shell
> Copy
> 
```
export IGW_CHART_VERSION=v1.5.0
helm install vllm-qwen3-32b \
--dependency-update \
--set inferencePool.modelServers.matchLabels.app=vllm-qwen3-32b \
--version $IGW_CHART_VERSION \
--set inferenceExtension.resources.requests.cpu=100m \
--set inferenceExtension.resources.requests.memory=512Mi \
--set inferenceExtension.resources.limits.memory=2Gi \
oci://registry.k8s.io/gateway-api-inference-extension/charts/inferencepool
```

Confirm that the Endpoint Picker was deployed and is running:
Copy

```
kubectl describe deployment vllm-qwen3-32b-epp
```

## 
[ Deploy an Inference Gateway ](https://docs.nginx.com/nginx-gateway-fabric/how-to/gateway-api-inference-extension/#deploy-an-inference-gateway "Deploy an Inference Gateway")
yaml
Copy

```
kubectl apply -f - <<EOFapiVersion:gateway.networking.k8s.io/v1kind:Gatewaymetadata:name:inference-gatewaygatewayClassName:nginxlisteners:- name:httpport:80protocol:HTTP
```

Confirm that the Gateway was assigned an IP address and reports a `Programmed=True` status:
Copy

```
kubectl describe gateways.gateway.networking.k8s.io inference-gateway
```

text
Copy

```
Status:
  Addresses:
    Type:   IPAddress
    Value:  10.96.36.219
  Conditions:
    Last Transition Time:  2026-01-09T05:40:37Z
    Message:               The Gateway is accepted
    Observed Generation:   1
    Reason:                Accepted
    Status:                True
    Type:                  Accepted
    Last Transition Time:  2026-01-09T05:40:37Z
    Message:               The Gateway is programmed
    Observed Generation:   1
    Reason:                Programmed
    Status:                True
    Type:                  Programmed
```

Save the public IP address and port(s) of the Gateway into shell variables:
text
Copy

```
GW_IP=XXX.YYY.ZZZ.III
GW_PORT=<port number>
```

## 
yaml
Copy

```
kubectl apply -f - <<EOFapiVersion:gateway.networking.k8s.io/v1kind:HTTPRoutemetadata:name:llm-routeparentRefs:- group:gateway.networking.k8s.iokind:Gatewayname:inference-gateway- backendRefs:- group:inference.networking.k8s.iokind:InferencePoolname:vllm-qwen3-32bmatches:type:PathPrefixvalue:/
```

Confirm that the HTTPRoute status conditions include `Accepted=True` and `ResolvedRefs=True`:
Copy

```
kubectl describe httproute llm-route
```

## 
Send traffic to the Gateway:
shell
Copy

```
curl -i $GW_IP:$GW_PORT/v1/completions -H 'Content-Type: application/json' -d '{
"model": "food-review-1",
"prompt": "Write as if you were a critic: San Francisco",
"max_tokens": 100,
"temperature": 0

```

## 
Uninstall the InferencePool and model server resources:
shell
Copy

```
helm uninstall vllm-qwen3-32b
kubectl delete -f https://raw.githubusercontent.com/kubernetes-sigs/gateway-api-inference-extension/refs/tags/v1.5.0/config/manifests/vllm/sim-deployment.yaml
```

Uninstall the Gateway API Inference Extension CRDs:
Copy

```
kubectl delete -k https://github.com/kubernetes-sigs/gateway-api-inference-extension/config/crd --ignore-not-found
```

Uninstall Inference Gateway and HTTPRoute:
shell
Copy

```
kubectl delete gateway inference-gateway
kubectl delete httproute llm-route
```

Uninstall NGINX Gateway Fabric:
Copy

```
helm uninstall ngf -n nginx-gateway
```

If needed, replace ngf with your chosen release name.
Remove namespace and NGINX Gateway Fabric CRDs:
shell
Copy

```
kubectl delete ns nginx-gateway
kubectl delete -f https://raw.githubusercontent.com/nginx/nginx-gateway-fabric/v2.7.0/deploy/crds.yaml
```

Remove the Gateway API CRDs:
> Warning 
> This step will remove all corresponding custom resources in your entire cluster, across every namespace.
> Ensure you don’t have any custom resources you need to keep, and confirm that there are no other Gateway API implementations active in your cluster.
To uninstall the Gateway API resources, run the following:
Copy

```
kubectl kustomize "https://github.com/nginx/nginx-gateway-fabric/config/crd/gateway-api/standard?ref=v2.7.0" | kubectl delete -f -
```

Example output
text
Copy

```
customresourcedefinition.apiextensions.k8s.io "backendtlspolicies.gateway.networking.k8s.io" deleted
customresourcedefinition.apiextensions.k8s.io "gatewayclasses.gateway.networking.k8s.io" deleted
customresourcedefinition.apiextensions.k8s.io "gateways.gateway.networking.k8s.io" deleted
customresourcedefinition.apiextensions.k8s.io "grpcroutes.gateway.networking.k8s.io" deleted
customresourcedefinition.apiextensions.k8s.io "httproutes.gateway.networking.k8s.io" deleted
customresourcedefinition.apiextensions.k8s.io "referencegrants.gateway.networking.k8s.io" deleted
```

If you installed the Gateway APIs from the experimental channel, run the following instead:
Copy

```
kubectl kustomize "https://github.com/nginx/nginx-gateway-fabric/config/crd/gateway-api/experimental?ref=v2.7.0" | kubectl delete -f -
```

## 
  * [Gateway API Inference Extension Introduction](https://gateway-api-inference-extension.sigs.k8s.io/): for introductory details to the project.
  * [Gateway API Inference Extension API Overview](https://gateway-api-inference-extension.sigs.k8s.io/concepts/api-overview/): for an API overview.
  * [Gateway API Inference Extension User Guides](https://gateway-api-inference-extension.sigs.k8s.io/guides/implementers/): for additional use cases and guides.
  * [llm-d](https://github.com/llm-d/llm-d): for information on the llm-d project.


