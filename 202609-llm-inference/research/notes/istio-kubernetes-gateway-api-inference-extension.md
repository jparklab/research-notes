---
title: Istio / Kubernetes Gateway API Inference Extension
id: istio-kubernetes-gateway-api-inference-extension
tags:
- llmd-router-nonk8s-backends
- istio
- gie
- inferencepool
- k8s-only
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:44:36.763397Z'
updated: '2026-09-15T04:55:23.731136Z'
source: https://istio.io/latest/docs/tasks/traffic-management/ingress/gateway-api-inference-extension/
source_domain: istio.io
fetched_at: '2026-09-11T19:44:36.736995Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'Istio''s official GIE integration task doc, confirming Istio''s Inference
  Extension support is entirely Kubernetes-native and InferencePool-bound: it requires
  installing Gateway API + Gateway API Inference Extension CRDs, enabling Pilot env
  flags ''SUPPORT_GATEWAY_API_INFERENCE_EXTENSION''/''ENABLE_GATEWAY_API_INFERENCE_EXTENSION'',
  and wiring an InferencePool resource with a ''selector.matchLabels'' targeting Kubernetes
  Pods plus an ''endpointPickerRef'' Service. The InferencePool''s endpoint discovery
  is pod-label-based (RBAC grants the EPP ''get/list/watch'' on Pods and InferencePools)
  and the example EPP config uses queue-scorer/kv-cache-utilization-scorer/prefix-cache-scorer
  plugins. There is no standalone or non-k8s-backend mode documented anywhere in this
  Istio task — confirming Istio is one of the gateways that ''only works with Kubernetes
  InferencePool'' per the research focus. A DestinationRule is required to enable
  TLS between the gateway and the endpoint-picker service.'
utility_score: 10.0
---

Contents
# Kubernetes Gateway API Inference Extension
5 minute read
This task describes how to configure Istio to use the Kubernetes [Gateway API Inference Extension](https://gateway-api-inference-extension.sigs.k8s.io/)6. The Gateway API Inference Extension aims to improve and standardize routing to self-hosted AI models in Kubernetes. It utilizes CRDs from the [Kubernetes Gateway API](https://istio.io/latest/docs/tasks/traffic-management/ingress/gateway-api-inference-extension/) and leverages Envoy’s [External Processing](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/ext_proc_filter)7 filter to extend any Gateway into an inference gateway.
## API Resources 
The Gateway API Inference Extension introduces two API types in order to assist with the unique challenges of traffic routing for inference workloads:
[`InferencePool`](https://gateway-api-inference-extension.sigs.k8s.io/api-types/inferencepool/)8 represents a collection of backends for an inference workload, and contains a reference to an associated endpoint picker service. The Envoy `ext_proc` filter is used to route incoming requests to the endpoint picker service in order to make an informed routing decision to an optimal backend in the inference pool.
[`InferenceObjective`](https://gateway-api-inference-extension.sigs.k8s.io/api-types/inferenceobjective/)9 allows specifying the serving objectives of the request associated with it.
## Setup 
  1. As the Gateway APIs are a prerequisite for Inference Extension APIs, install both the Gateway API and Gateway API Inference Extension CRDs if they are not present:

```

$ kubectl get crd gateways.gateway.networking.k8s.io  /dev/null || \
  { kubectl kustomize "github.com/kubernetes-sigs/gateway-api/config/crd?ref=v1.6.0" | kubectl apply -f -; }
$ kubectl get crd inference.networking.k8s.io  /dev/null || \
  { kubectl kustomize "github.com/kubernetes-sigs/gateway-api-inference-extension/config/crd?ref=v1.0.1" | kubectl apply -f -; }


```

  2. Install Istio using the `minimal` profile:

```

$ istioctl install --set profile=minimal --set values.pilot.env.SUPPORT_GATEWAY_API_INFERENCE_EXTENSION=true --set values.pilot.env.ENABLE_GATEWAY_API_INFERENCE_EXTENSION=true -y


```



## Configuring an `InferencePool`
For a detailed guide on setting up a local test environment, see the [Gateway API Inference Extension documentation](https://gateway-api-inference-extension.sigs.k8s.io/guides/)10.
In this example, we will deploy a inference model service using a vLLM simulator, and use an `InferencePool` and the endpoint picker in order to route requests to individual backends.
  1. Deploy a basic vLLM simulator to behave as our inference workload, and the essential Gateway API resources:

```

$ kubectl create namespace istio-ingress
$ kubectl apply -f - EOF
apiVersion: v1
kind: Namespace
metadata:
  name: inference-model-server
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: inference-model-server-deployment
  namespace: inference-model-server
  labels:
    app: inference-model-server
spec:
  replicas: 3
  selector:
    matchLabels:
      app: inference-model-server
  template:
    metadata:
      labels:
        app: inference-model-server
    spec:
      containers:
      - name: vllm-sim
        image: ghcr.io/llm-d/llm-d-inference-sim:v0.7.1
        imagePullPolicy: Always
        args:
        - --model
        - meta-llama/Llama-3.1-8B-Instruct
        - --port
        - "8000"
        - --max-loras
        - "2"
        - --lora-modules
        - '{"name": "reviews-1"}'
        ports:
        - containerPort: 8000
          name: http
          protocol: TCP
        env:
        - name: POD_NAME
          valueFrom:
            fieldRef:
              fieldPath: metadata.name
        - name: NAMESPACE
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace
        - name: POD_IP
          valueFrom:
            fieldRef:
              fieldPath: status.podIP
        resources:
          requests:
            cpu: 20m
---
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: gateway
  namespace: istio-ingress
spec:
  gatewayClassName: istio
  listeners:
  - name: http
    port: 80
    protocol: HTTP
    allowedRoutes:
      namespaces:
        from: All
      kinds:
      - group: gateway.networking.k8s.io
        kind: HTTPRoute
---
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: httproute-for-inferencepool
  namespace: inference-model-server
spec:
  parentRefs:
  - group: gateway.networking.k8s.io
    kind: Gateway
    name: gateway
    namespace: istio-ingress
    sectionName: http
  rules:
  - backendRefs:
    - group: inference.networking.k8s.io
      kind: InferencePool
      name: inference-model-server-pool
    matches:
    - path:
        type: PathPrefix
        value: /v1/completions
EOF


```

  2. Deploy the endpoint picker service and create an `InferencePool`:

```

$ kubectl apply -f - EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: inference-endpoint-picker
  namespace: inference-model-server
  labels:
    app: inference-epp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: inference-epp
  template:
    metadata:
      labels:
        app: inference-epp
    spec:
      containers:
      - name: epp
        image: us-central1-docker.pkg.dev/k8s-staging-images/gateway-api-inference-extension/epp:v20251119-2aaf2a6
        imagePullPolicy: Always
        args:
        - --pool-name
        - "inference-model-server-pool"
        - --pool-namespace
        - "inference-model-server"
        - --v
        - "4"
        - --zap-encoder
        - "json"
        - "--config-file"
        - "/config/default-plugins.yaml"
        ports:
        - containerPort: 9002
        - containerPort: 9003
        - name: metrics
          containerPort: 9090
        livenessProbe:
          grpc:
            port: 9003
            service: inference-extension
          initialDelaySeconds: 5
          periodSeconds: 10
        readinessProbe:
          grpc:
            port: 9003
            service: inference-extension
          initialDelaySeconds: 5
          periodSeconds: 10
        volumeMounts:
        - name: plugins-config-volume
          mountPath: "/config"
      volumes:
      - name: plugins-config-volume
        configMap:
          name: plugins-config
---
apiVersion: v1
kind: Service
metadata:
  name: endpoint-picker-svc
  namespace: inference-model-server
spec:
  selector:
    app: inference-epp
  ports:
    - protocol: TCP
      port: 9002
      targetPort: 9002
      appProtocol: http2
  type: ClusterIP
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: plugins-config
  namespace: inference-model-server
data:
  default-plugins.yaml: |
    apiVersion: inference.networking.x-k8s.io/v1alpha1
    kind: EndpointPickerConfig
    plugins:
    - type: queue-scorer
    - type: kv-cache-utilization-scorer
    - type: prefix-cache-scorer
    schedulingProfiles:
    - name: default
      plugins:
      - pluginRef: queue-scorer
        weight: 2
      - pluginRef: kv-cache-utilization-scorer
        weight: 2
      - pluginRef: prefix-cache-scorer
        weight: 3
---
# A DestinationRule is required to enable TLS between the gateway and
# the endpoint picker.
apiVersion: networking.istio.io/v1
kind: DestinationRule
metadata:
  name: endpoint-picker-tls
  namespace: inference-model-server
spec:
  host: endpoint-picker-svc
  trafficPolicy:
      tls:
        mode: SIMPLE
        insecureSkipVerify: true
---
apiVersion: inference.networking.k8s.io/v1
kind: InferencePool
metadata:
  name: inference-model-server-pool
  namespace: inference-model-server
spec:
  selector:
    matchLabels:
      app: inference-model-server
  targetPorts:
    - number: 8000
  endpointPickerRef:
    name: endpoint-picker-svc
    port:
      number: 9002
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: inference-model-reader
  namespace: inference-model-server
rules:
- apiGroups: ["inference.networking.k8s.io"]
  resources: ["inferencepools"]
  verbs: ["get", "list", "watch"]
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: epp-to-inference-model-reader
  namespace: inference-model-server
subjects:
- kind: ServiceAccount
  name: default
  namespace: inference-model-server
roleRef:
  kind: Role
  name: inference-model-reader
  apiGroup: rbac.authorization.k8s.io
EOF


```

  3. Set the Ingress Host environment variable:

```

$ kubectl wait -n istio-ingress --for=condition=programmed gateways.gateway.networking.k8s.io gateway
$ export INGRESS_HOST=$(kubectl get gateways.gateway.networking.k8s.io gateway -n istio-ingress -ojsonpath='{.status.addresses[0].value}')


```

  4. Send an inference request using _curl_ , you should see a successful response from the backend model server:

```

$ curl -s -i "http://$INGRESS_HOST/v1/completions" -d '{"model": "reviews-1", "prompt": "What do reviewers think about The Comedy of Errors?", "max_tokens": 100, "temperature": 0}'



...
HTTP/1.1 200 OK
...
server: istio-envoy
...
{"choices":[{"finish_reason":"stop","index":0,"text":"Testing@, #testing 1$ ,2%,3^, [4"}],"created":1770406965,"id":"cmpl-5e508481-7c11-53e8-9587-972a3704724e","kv_transfer_params":null,"model":"reviews-1","object":"text_completion","usage":{"completion_tokens":16,"prompt_tokens":10,"total_tokens":26}}

```



## Cleanup 
  1. Remove deployments and Gateway API resources:

```

$ kubectl delete deployment inference-model-server-deployment inference-endpoint-picker -n inference-model-server
$ kubectl delete httproute httproute-for-inferencepool -n inference-model-server
$ kubectl delete inferencepool inference-model-server-pool -n inference-model-server
$ kubectl delete gateways.gateway.networking.k8s.io gateway -n istio-ingress
$ kubectl delete ns istio-ingress inference-model-server


```

  2. Uninstall Istio:

```

$ istioctl uninstall -y --purge
$ kubectl delete ns istio-system


```

  3. Remove the Gateway API and Gateway API Inference Extension CRDs if they are no longer needed:

```

$ kubectl kustomize "github.com/kubernetes-sigs/gateway-api/config/crd?ref=v1.6.0" | kubectl delete -f -
$ kubectl kustomize "github.com/kubernetes-sigs/gateway-api-inference-extension/config/crd?ref=v1.0.1" | kubectl delete -f -


```



Was this information useful?Yes No
Do you have any suggestions for improvement? 
Thanks for your feedback!
## Links
  1. https://istio.io/latest/docs/
  2. https://istio.io/latest/docs/tasks/
  3. https://istio.io/latest/docs/tasks/traffic-management/
  4. https://istio.io/latest/docs/tasks/traffic-management/ingress/
  5. https://github.com/istio/istio.io/tree/master/README.md#testing-document-content
  6. https://gateway-api-inference-extension.sigs.k8s.io/
  7. https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/ext_proc_filter
  8. https://gateway-api-inference-extension.sigs.k8s.io/api-types/inferencepool/
  9. https://gateway-api-inference-extension.sigs.k8s.io/api-types/inferenceobjective/
  10. https://gateway-api-inference-extension.sigs.k8s.io/guides/


