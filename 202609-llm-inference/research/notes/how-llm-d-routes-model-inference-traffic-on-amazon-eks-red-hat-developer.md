---
title: How llm-d routes model inference traffic on Amazon EKS | Red Hat Developer
id: how-llm-d-routes-model-inference-traffic-on-amazon-eks-red-hat-developer
tags:
- llmd-router-nonk8s-backends-54b774
- epp
- llm-d
- inferencepool
- eks
- kserve
- llmd-kv-cache-routing-c665d5
created: '2026-09-11T19:44:55.045519Z'
updated: '2026-09-15T04:55:23.741933Z'
source: https://developers.redhat.com/articles/2026/08/13/how-llm-d-routes-model-inference-traffic-amazon-eks
source_domain: developers.redhat.com
fetched_at: '2026-09-11T19:44:55.021895Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Red Hat Developer article (Aug 13, 2026) traces a live request through llm-d
  on Amazon EKS via KServe LLMInferenceService, Istio, and Gateway API Inference Extension
  -- the most concrete component-level wiring diagram in this batch. Versions pinned:
  llm-d inference scheduler 0.7.1, Gateway API 1.4.0, Gateway API Inference Extension
  1.3.1, Istio 1.27.8_ossm, KServe (LLMISvc controller) 0.17.0, Red Hat AI Inference
  3.4.0, EKS 1.34.8-eks. Key architecture facts: the KServe-generated scheduler pod
  (''test-kserve-router-scheduler'') runs TWO containers sharing state over ZeroMQ
  -- ''main'' (image odh-llm-d-inference-scheduler-rhel9) implementing the EPP with
  routing plugins queue-scorer, prefix-cache-scorer, max-score-picker, exposing gRPC
  on port 9002; and ''tokenizer'' (image odh-llm-d-kv-cache-rhel9) tracking prefix-cache
  state across vLLM replicas. The EPP acts as an Envoy ext-proc external processor:
  Envoy pauses the request, calls the scheduler over gRPC, and forwards to the returned
  pod. InferencePool.spec.endpointPickerRef points the gateway at the EPP Service
  (failureMode: FailOpen falls back to standard LB if EPP is unreachable; FailClose
  drops requests instead -- explicit availability/correctness tradeoff). InferenceObjective
  (group inference.networking.x-k8s.io, distinct from InferencePool''s inference.networking.k8s.io
  group) sets per-request priority via the client header ''x-gateway-inference-objective'';
  requests without the header default to priority zero. RBAC: the EPP''s Role is scoped
  to get/list/watch only on core pods/endpoints plus inferencepools/inferenceobjectives
  -- no write access. Critically, only the scheduler (EPP) and istiod watch the InferencePool
  CR directly; the Envoy gateway pod itself has NO Kubernetes API access -- istiod
  compiles the InferencePool into gateway config and the gateway only calls the EPP
  over the compiled ext-proc endpoint. The InferencePool auto-creates a headless backing
  Service so the gateway has a stable address even though routing is decided by the
  EPP.'
utility_score: 12.0
---

[ Skip to main content ](https://developers.redhat.com/articles/2026/08/13/how-llm-d-routes-model-inference-traffic-amazon-eks#main-content)
All Red Hat 
Back to menu
## Quick links: redhat.com, Customer Portal, Red Hat's developer site, Red Hat's partner site.
  * [ You are here Red Hat Learn about our open source products, services, and company. ](https://www.redhat.com/en)
  * [ You are here Red Hat Customer Portal Get product support and knowledge from the open source experts. ](https://access.redhat.com)
  * [ You are here Red Hat Developer Read developer tutorials and download Red Hat software for cloud application development. ](https://developers.redhat.com)
  * [ You are here Red Hat Partner Connect Get training, subscriptions, certifications, and more for partners to build, sell, and support customer solutions.  ](https://connect.redhat.com)


## Products & tools
  * ### [Red Hat Documentation ](https://docs.redhat.com) Find answers, get guidance, and learn how to use Red Hat products. 
  * ### [Red Hat Ecosystem Catalog ](https://catalog.redhat.com) Find hardware, software, and cloud providers―and download container images―certified to perform with Red Hat technologies. 


## Try, buy, & sell
  * ### [Red Hat Hybrid Cloud Console ](https://www.redhat.com/hybrid-cloud-console) Access technical how-tos, tutorials, and learning paths focused on Red Hat’s hybrid cloud managed services. 
  * ### [Red Hat Store ](https://www.redhat.com/store) Buy select Red Hat products and services online. 


## Events
  * ### [Red Hat Summit ](https://www.redhat.com/summit) Register for and learn about our annual open source IT industry event. 


Ask AI
#  How llm-d routes model inference traffic on Amazon EKS
August 13, 2026 

Related topics:
     [AI inference](https://developers.redhat.com/topics/ai-inference/all)[Kubernetes](https://developers.redhat.com/topics/kubernetes)[Artificial intelligence](https://developers.redhat.com/topics/ai-ml) 

Related products:
     [Red Hat AI](https://developers.redhat.com/taxonomy/term/37288)[Red Hat OpenShift AI](https://developers.redhat.com/products/red-hat-openshift-ai/overview)
Table of contents: 
  * [The llm-d endpoint picker](https://developers.redhat.com/articles/2026/08/13/how-llm-d-routes-model-inference-traffic-amazon-eks#the_llm_d_endpoint_picker)
  * [Routing the request](https://developers.redhat.com/articles/2026/08/13/how-llm-d-routes-model-inference-traffic-amazon-eks#routing_the_request)
  * [Traffic policy (Istio)](https://developers.redhat.com/articles/2026/08/13/how-llm-d-routes-model-inference-traffic-amazon-eks#traffic_policy__istio_)
  * [Identity and security](https://developers.redhat.com/articles/2026/08/13/how-llm-d-routes-model-inference-traffic-amazon-eks#identity_and_security)
  * [How a request flows through the stack](https://developers.redhat.com/articles/2026/08/13/how-llm-d-routes-model-inference-traffic-amazon-eks#how_a_request_flows_through_the_stack)


In [Trace Kubernetes resources for llm-d model serving](https://developers.redhat.com/articles/2026/08/05/trace-kubernetes-resources-llm-d-model-serving), we generated the custom resources needed for llm-d model serving. But what actually happens inside your cluster when a prompt arrives? In this post, we follow a request from the ingress gateway down to individual vLLM pods to see how Kubernetes custom resources make real-time routing decisions.
Prefer a visual overview? Watch the following video to see how KServe and llm-d set up back-end resources, track KV cache locality, and route incoming traffic.
The llm-d endpoint picker
## The llm-d endpoint picker
The scheduler configuration creates an endpoint picker (EPP) pod. The scheduler pod runs the endpoint picker, the process that decides which vLLM pod each request goes to. Its resource name is `test-kserve-router-scheduler`.

```
kubectl get pod -n llm-test -l app.kubernetes.io/component=llminferenceservice-router-scheduler
NAME                           READY   STATUS    test-kserve-router-scheduler   2/2     Running 
```

There are two containers inside the scheduler pod.
Container 1: main (scheduler):
  * Image: `registry.redhat.io/rhoai/odh-llm-d-inference-scheduler-rhel9`
  * Runs the endpoint picker (EPP), which determines which backend pod to use.
  * Exposes a gRPC server on port 9002.
  * Implements routing plug-ins: `queue-scorer`, `prefix-cache-scorer`, and `max-score-picker`.


Container 2: tokenizer (KV cache tracker):
  * Image: `registry.redhat.io/rhoai/odh-llm-d-kv-cache-rhel9`
  * Tracks prefix cache state across vLLM replicas.
  * Helps the scheduler route requests to pods with matching cache for better latency.


As shown in Figure 1, the scheduler pod contains two containers that share state over ZeroMQ.
Figure 1: The scheduler pod's two containers, main (the endpoint picker) and tokenizer (the KV cache tracker), which share cache state over ZeroMQ.
### Envoy ext-proc
The scheduler acts as an Envoy [external processor (ext-proc)](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/ext_proc_filter). The `ext-proc` feature lets Envoy call an external gRPC service and act on the response before continuing to process the request. In practice, when a request arrives at the gateway, Envoy processes it, calls the scheduler over gRPC to ask which backend to use, and forwards the request to the pod the scheduler picks.
The `ext-proc` interaction between Envoy and the scheduler is illustrated in Figure 2.
Figure 2: Envoy pauses the request, asks the scheduler over gRPC which pod to use, then forwards the request to it.
### Inference resources
To make intelligent routing decisions, the scheduler uses resources that answer two questions: which pods it can route to, and how different requests should be prioritized. It reads each answer from its own Kubernetes custom resource:
  * `InferencePool`**:** The set of back-end pods to choose from, grouping the model's vLLM pods into a single target.
  * `InferenceObjective`**(optional):** Defines traffic priorities. This basic deployment does not create an `InferenceObjective`, so every request receives equal treatment. The scheduler has permission to watch for them, and priorities take effect when added at scale.


Figure 3 shows how the EPP scheduler reads both custom resources to determine routing decisions.
Figure 3: The scheduler (EPP) reads the InferencePool for its candidate pods and the InferenceObjective for traffic priorities.
Add an `InferenceObjective` when you need traffic priorities—for example, to keep latency-sensitive requests ahead of batch work under load.
An objective takes effect per request rather than globally. A client selects an objective by setting the `x-gateway-inference-objective` header to the objective's name (for example, `high-priority`). The scheduler then applies that objective's priority when it picks an endpoint, so higher-priority requests are favored when the pool is under load. Requests that omit the header fall back to the default treatment.
To add an `InferenceObjective`, apply the following configuration:

```
apiVersion: inference.networking.x-k8s.io/v1alpha2
kind: InferenceObjective
metadata:
  name: high-priority
  namespace: llm-test
spec:
  priority: 100          # higher value = more critical
  poolRef:
    name: test-inference-pool
    group: inference.networking.k8s.io
```

An `InferenceObjective` defines routing priorities in your cluster, which incoming requests invoke by passing a custom header.

```
x-gateway-inference-objective: high-priority
```

The scheduler reads the header, looks up the matching `InferenceObjective`, and uses its priority to decide which queued request to dispatch first. A request without an objective defaults to priority zero, which is why every request in this deployment receives equal treatment.
### Gateway watch permissions
The scheduler `Role` has watch permissions and reads both resources directly from the Kubernetes API, illustrating how the[ Gateway API Inference Extension](https://gateway-api-inference-extension.sigs.k8s.io/) works with Istio. The gateway pod does not have permission to access these resources. The Istio control plane (`istiod`) reads the `InferencePool` from Kubernetes and compiles it into the gateway configuration. The gateway pod itself never reads from the Kubernetes API. This compiled configuration instructs the gateway to call the scheduler.  
Table: The scheduler watches the InferencePool and InferenceObjective directly, istiod watches only the InferencePool, and the gateway pod has no direct Kubernetes API access.  
| **Component**  | **InferencePool**  | **InferenceObjective**  | **Direct Kubernetes API access**  |  
| --- | --- | --- | --- |  
|  **Scheduler (EPP)** `test-kserve-router-scheduler`  | Watches  | Watches  | Yes (RBAC)  |  
|  **istiod** Istio control plane  | Watches, compiles  | No  | Yes  |  
|  **Gateway pod (Envoy)** `inference-gateway-istio`  | Via `istiod`'s config  | No  | No  |  
With the scheduler and `InferencePool` established, examine their configuration and implementation details.
### Inference pool
The InferencePool is a configuration resource that tells Envoy which endpoint picker to call and contains a selector to identify available backend pods.

```
kubectl get inferencepool test-inference-pool -n llm-test -o yaml
```

```
apiVersion: inference.networking.k8s.io/v1
kind: InferencePool
metadata:
  name: test-inference-pool
  namespace: llm-test
spec:
  endpointPickerRef:
    failureMode: FailOpen
    kind: Service
    name: test-router-epp-service
    port:
      number: 9002
  selector:
    matchLabels:
      app.kubernetes.io/name: test
      kserve.io/component: workload
  targetPorts:
  - number: 8000
```

The relationship between the `InferencePool` and the scheduler is depicted in Figure 4.
Figure 4: The InferencePool's endpointPickerRef field points the gateway at the scheduler, and its selector defines the vLLM pods the scheduler chooses from.
### Failure modes
When you set [failureMode: FailOpen](https://gateway-api-inference-extension.sigs.k8s.io/api-types/inferencepool/), the gateway falls back to standard load balancing if the endpoint picker becomes unreachable. This keeps the system available even if intelligent routing breaks.
The alternative option is `FailClose`, which drops requests if the endpoint picker is unavailable. Choose `FailClose` when a request must never be served without the scheduler's decision, trading availability for that guarantee.
### Service
The `InferencePool` creates a dynamic `Service` resource.

```
kubectl get svc -n llm-test | grep inference-pool
test-inference-pool-ip   ClusterIP   None   8000/TCP
```

The `InferencePool` gets its own backing `Service` so that the gateway has a stable in-cluster address for the pool, even though the routing decision itself is made by the scheduler.
Routing the request
## Routing the request
The `HTTPRoute` connects an incoming request to the correct backend. It matches the request's URL path and forwards it to the `InferencePool`, which points the gateway at the scheduler. This is also where the shared gateway component plugs in.
Figure 5 demonstrates how the `HTTPRoute` matches and rewrites the incoming request URL path.
Figure 5: The HTTPRoute matches the request's URL path, rewrites it, and forwards it to the InferencePool, which points the gateway at the scheduler.
Mapping external URL paths to the `InferencePool`makes the model reachable at a predictable address, such as `/llm-test/test-router/v1/chat/completions`, rather than an internal pod IP.
Remember that we specified `route: {}` in the YAML, and the controller generated the routing rules automatically:

```
kubectl get httproute test-kserve-route -n llm-test -o yaml
```

```
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: test-kserve-route
  namespace: llm-test
spec:
  parentRefs:
  - group: gateway.networking.k8s.io
    kind: Gateway
    name: inference-gateway
    namespace: redhat-ods-applications
  rules:
  - matches:
    - path:
        type: PathPrefix
        value: /llm-test/test-router/v1/chat/completions
    backendRefs:
    - group: inference.networking.k8s.io
      kind: InferencePool
      name: test-inference-pool
      port: 8000
    filters:
    - type: URLRewrite
      urlRewrite:
        path:
          replacePrefixMatch: /v1/chat/completions
```

### What the URL rewriting does
External clients call `http://loadbalancer/llm-test/test-router/v1/chat/completions`.
Then, the `HTTPRoute` strips the namespace and service prefix and forwards the request to Envoy. Envoy calls the EPP configured by the `InferencePool` to select a backend, like `http://pool:8000/v1/chat/completions`.
The `HTTPRoute` attaches to the platform's `inference-gateway` (configured in [part 1](https://developers.redhat.com/articles/2026/06/16/red-hat-ai-inference-amazon-eks-kubernetes-resources)).

```
kubectl get gateway inference-gateway -n redhat-ods-applications -o yaml
```

```
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: inference-gateway
  namespace: redhat-ods-applications
spec:
  gatewayClassName: istio
  listeners:
  - name: http
    port: 80
    protocol: HTTP
    allowedRoutes:
      namespaces:
        from: All
status:
  addresses:
  - type: Hostname
    value: inference-gateway-xyz.elb.us-east-1.amazonaws.com
    listeners:
    - attachedRoutes: 1
      conditions:
      - type: Programmed
        status: "True"
      name: http
```

There are two fields to pay attention to in the status block. First, the`addresses`field displays the external hostname the cloud provider assigned to your gateway. Clients send requests to this external hostname.
Second, the `attachedRoutes: 1` field confirms the `HTTPRoute` you created is properly registered with the gateway.
The complete request path through the infrastructure layers is mapped out in Figure 6.
Figure 6: The full request path from client to AWS load balancer to the Kubernetes Service to the Istio Envoy pod to a vLLM pod, with the scheduler called as a side request.
Let's break down the gateway layers.
  * The gateway resource (`inference-gateway`) contains the configuration and the status of the external address shown above.
  * The Gateway configuration is used by the Istio Envoy pod (via the `inference-gateway-istio` Deployment).
  * The Istio Envoy pod is the proxy that receives incoming traffic and applies the `HTTPRoute` rules.
  * In front of the Istio Envoy pod is a Kubernetes Service of type `LoadBalancer`.
  * The `LoadBalancer` type triggers the cloud's controller to provision the external load balancer (an AWS ELB here).
  * The external load balancer's hostname appears in the gateway status.


The result is one gateway, many `HTTPRoute` resources.
  * Every inference service in the cluster attaches its own `HTTPRoute` to this shared gateway.
  * The Envoy proxy evaluates each of the routes.
  * When the matched route's backend is an `InferencePool`, the Envoy proxy calls the endpoint picker scheduler.
  * The scheduler responds by picking the appropriate endpoint.
  * The Envoy proxy then forwards to the chosen pod.


Figure 7 illustrates the Gateway resource and its underlying infrastructure layers.
Figure 7: The Gateway resource and the layers it sits on, the external load balancer, the LoadBalancer Service, and the Istio Envoy pod that applies the HTTPRoute rules.
### The endpoint picker Service
The `Service` for the endpoint picker scheduler pod provides a stable `ClusterIP` address for the gateway to call. The `EndpointSlice` contains the live list of pod IPs behind the `Service`, allowing traffic sent to the stable address to reach the running scheduler pod.

```
kubectl get svc test-router-epp-service -n llm-test -o wide
NAME               TYPE        CLUSTER-IP       PORT(S)
test-router-epp-service   ClusterIP   172.20.243.216   9002/TCP,9003/TCP,9090/TCP,5557/TCP
```

Ports:
  * **9002** : gRPC endpoint picker scheduler
  * **9003** : Health checks
  * **9090** : Prometheus metrics
  * **5557** : ZeroMQ channel that the scheduler (main) and the KV cache tracker (tokenizer) use to share prefix-cache state


Figure 8 displays how `Service` and `EndpointSlice` resources map stable IP addresses to backend pods.
Figure 8: Each Service has a stable ClusterIP, and its EndpointSlice holds the live list of pod IPs behind it. Traffic policy (Istio)
## Traffic policy (Istio)
A `DestinationRule` is an Istio resource that sets the policy for how traffic reaches a service once the decision of where to send it has already been made. Three rules configure how Istio handles traffic to the scheduler, the workload, and a shadow service:

```
kubectl get destinationrule -n llm-test
NAME
test-kserve-scheduler
test-kserve-shadow-svc
test-kserve-workload-svc
```

Behind the scenes, these `DestinationRule` resources manage connection pooling, circuit breakers, and mutual TLS. Istio keeps your pod-to-pod traffic encrypted and steady under load without requiring manual tuning.
### Note
The `shadow-svc` rule covers shadow traffic. Istio can mirror live requests to another target without affecting the real response, which is useful for A/B testing. The platform sets up the policy; mirroring only happens if you enable it.
Identity and security
## Identity and security
The scheduler runs with a _least-privilege identity_ , meaning it can read the pods and routing resources it needs to make decisions, but cannot create, update, or delete anything. `Role`, `RoleBinding`, `ServiceAccount`, and `Secret` resources coordinate to make sure the platform resources are secure.
The RBAC relationship for the scheduler pod identity is shown in Figure 9.
Figure 9: The RoleBinding grants the Role's read permissions to the ServiceAccount that the scheduler pod runs as.
### Role and RoleBinding
The scheduler needs permission to watch pods, endpoints, and the `InferencePool` and `InferenceObjective` resources. A [Role](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-clusterrole) grants those read permissions, and a [RoleBinding](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#rolebinding-and-clusterrolebinding) attaches the [Role](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-clusterrole) to the scheduler's `ServiceAccount`.
#### Role

```
kubectl get role test-router-epp-role -n llm-test -o yaml
rules:
- apiGroups: [""]
  resources: ["pods", "endpoints"]
  verbs: ["get", "list", "watch"]
- apiGroups: ["inference.networking.k8s.io", "inference.networking.x-k8s.io"]
  resources: ["inferencepools", "inferenceobjectives"]
  verbs: ["get", "list", "watch"]
```

#### RoleBinding

```
kubectl get rolebinding test-router-epp-rb -n llm-test
```

The `RoleBinding` binds the `Role` to the `ServiceAccount`.

```
 apiVersion: rbac.authorization.k8s.io/v1                      
  kind: RoleBinding
  metadata:
    name: test-router-epp-rb
    namespace: llm-test
  roleRef: 
    apiGroup: rbac.authorization.k8s.io                         
    kind: Role
    name: test-router-epp-role
  subjects:
  - kind: ServiceAccount
    name: test-router-epp-sa
    namespace: llm-test
```

### ServiceAccount
The `ServiceAccount` provides the identity for the scheduler pod. It cannot grant permissions by itself; it is simply the subject that the `Role` and `RoleBinding` attach access to.

```
kubectl get sa test-router-epp-sa -n llm-test
```

```
 apiVersion: v1
  kind: ServiceAccount
  metadata:
    name: test-router-epp-sa
    namespace: llm-test
```

### Secret
The `Secret` object holds a self-signed TLS certificate and key that the controller generates and mounts automatically to encrypt communication between platform components.

```
kubectl get secret test-router-kserve-self-signed-certs -n llm-test
```

```
apiVersion: v1
  kind: Secret
  metadata:
    annotations:
      certificates.kserve.io/expiration: "2034-07-04T18:22:06Z"
    name: test-router-kserve-self-signed-certs
    namespace: llm-test 
    ownerReferences:
    - apiVersion: serving.kserve.io/v1alpha2
      kind: LLMInferenceService
      name: test-router 
  type: kubernetes.io/tls
  data:
    ca.crt: <base64-encoded>
    tls.crt: <base64-encoded>
    tls.key: <base64-encoded>
```
How a request flows through the stack
## How a request flows through the stack
Putting it all together, we can see the full request path. As summarized in Figure 10, the request completes an end-to-end traversal of the entire serving stack.
Figure 10: A request flows from the client through the load balancer to the Envoy gateway, which calls the EPP scheduler to select a vLLM pod and then forwards the request to that pod. Testing the stack
## Testing the stack
To test locally, port-forward the gateway.

```
kubectl port-forward -n redhat-ods-applications svc/inference-gateway-istio 8080:80
```

Check that the model is accessible.

```
curl -s http://localhost:8080/llm-test/test-router/v1/models | jq .
{
  "object": "list",
  "data": [{
    "id": "Qwen/Qwen2.5-0.5B-Instruct"
  }]
}
```

Send an inference request.

```
curl -X POST http://localhost:8080/llm-test/test-router/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "Qwen/Qwen2.5-0.5B-Instruct",
    "messages": [{"role": "user", "content": "Say hello"}],
    "max_tokens": 50
  }' | jq .
{
  "id": "chatcmpl-d7a03327-4974-47b8-b195-5c7403d3f59d",
  "object": "chat.completion",
  "model": "Qwen/Qwen2.5-0.5B-Instruct",
  "choices": [{
    "index": 0,
    "message": {
      "role": "assistant",
      "content": "Hello! How can I assist you today?"
    },
    "finish_reason": "stop"
  }],
  "usage": {
    "prompt_tokens": 31,
    "total_tokens": 41,
    "completion_tokens": 10
  }
}
```

Use request ID `d7a03327-4974-47b8-b195-5c7403d3f59d`to check the scheduler logs and verify that it received the request:

```
kubectl logs -n llm-test test-kserve-router-scheduler-1 -c main | grep d7a03327
{
  "level": "debug",
  "caller": "handlers/server.go:214",
  "msg": "EPP received request",
  "x-request-id": "d7a03327-4974-47b8-b195-5c7403d3f59d"
}
{
  "level": "debug",
  "caller": "handlers/server.go:390",
  "msg": "EPP sent response body back to proxy",
  "x-request-id": "d7a03327-4974-47b8-b195-5c7403d3f59d"
}
```

The log output shows that Envoy asked the EPP router for a routing decision, the EPP router responded with the chosen backend, and Envoy forwarded the request. The whole stack is working!
Key takeaways
## Key takeaways
  * **One YAML, many resources:** The controller orchestrates workload, routing, RBAC, and networking from a single declarative specification.
  * **Routing is an additional layer for serving:** vLLM pods handle inference, while the scheduler handles which pod receives which request.
  * **KServe uses intelligent llm-d defaults:** Specifying `route: {}` and `gateway: {}` generates `HTTPRoute` rules, URL rewriting, and gateway attachment automatically.
  * **One gateway, many**`HTTPRoute`**resources:** Every inference service attaches its own `HTTPRoute` to the single shared gateway, giving platform owners precise control over incoming public requests.
  * **The platform, not the model, owns the experience at scale:** Decisions about which pod serves a request, how failures are contained, and how traffic is prioritized all happen at the routing layer.


At production scale, inference becomes a routing, capacity, failure management, and priority enforcement problem. Red Hat AI Inference provides that routing layer as part of the platform with llm-d, handling routing decisions based on capacity, cache state, and traffic priorities.  
| **Component**  | **Version**  |  
| --- | --- |  
| Red Hat AI Inference  | 3.4.0  |  
| vLLM  | 0.18.0+rhaiv.7  |  
| RHAI Operator  | XKS 3.4.0  |  
| Cloud Manager Operator  | 3.4.0 (same image as RHAI operator)  |  
| llm-d inference scheduler  | 0.7.1  |  
| llm-d workload variant autoscaler  | 0.6.0  |  
| Gateway API  | 1.4.0  |  
| Gateway API inference extension  | 1.3.1  |  
| Istio  | 1.27.8_ossm  |  
| Sail operator  | 3.2.3  |  
| cert-manager  | 1.18.4  |  
| LWS (Leader Worker Set)  | 0.7.0  |  
| KServe (LLMISvc controller)  | 0.17.0  |  
| EKS (Kubernetes)  | 1.34.8-eks  |  
| Helm chart  | 0.1.20887+863cde804  |  
Ready to optimize your model-serving stack? Dive into these guides and community repositories to experiment with custom routing policies and KServe configurations:
  * [Combining KServe and llm-d for optimized generative AI inference](https://developers.redhat.com/articles/2026/04/21/kserve-llm-d-optimized-gen-ai-inference) (Red Hat Developer)
  * [LLMInferenceService configuration guide](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-configuration) (KServe docs)
  * [Understanding LLMInferenceService](https://kserve.github.io/website/docs/model-serving/generative-inference/llmisvc/llmisvc-overview) (KServe docs)
  * [llm-d](https://github.com/llm-d/llm-d) (the routing project)


### Platforms
  * [Red Hat Enterprise Linux](https://developers.redhat.com/products/rhel/overview)
  * [Red Hat Ansible Automation Platform](https://developers.redhat.com/products/ansible/overview)
  * [See all products](https://developers.redhat.com/products)


### Build
  * [Developer tools](https://developers.redhat.com/topics/developer-tools)
  * [Interactive tutorials](https://developers.redhat.com/learn#assembly-id-70181)


### Quicklinks
  * [Learning resources](https://developers.redhat.com/learn)


### Communicate
  * [Report a website issue](https://developers.redhat.com/report_a_website_issue)
  * [Site status dashboard](https://status.redhat.com/)
  * [Report a security problem](https://access.redhat.com/security/team/contact/)


### RED HAT DEVELOPER
Build here. Go anywhere.
We serve the builders. The problem solvers who create careers with code.
Join us if you’re a developer, software engineer, web designer, front-end designer, UX designer, computer scientist, architect, tester, product manager, project manager or team lead.
### Red Hat legal and privacy links
  * [Cool Stuff Store](https://coolstuff.redhat.com/)

© 2026 Red Hat
### Red Hat legal and privacy links
  * [All policies and guidelines](https://redhat.com/en/about/all-policies-guidelines)
  * [Digital accessibility](https://redhat.com/en/about/digital-accessibility)
  * Cookie Preferences and Opt-Out Rights


