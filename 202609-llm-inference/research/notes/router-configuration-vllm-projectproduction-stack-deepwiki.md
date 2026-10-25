---
title: Router Configuration | vllm-project/production-stack | DeepWiki
id: router-configuration-vllm-projectproduction-stack-deepwiki
tags:
- llmd-router-nonk8s
- service-discovery
- production-stack
- llmd-router-nonk8s-backends-54b774
- llmd-kv-cache-routing-c665d5
created: '2026-09-11T19:45:23.999445Z'
updated: '2026-09-15T04:55:23.758625Z'
source: https://deepwiki.com/vllm-project/production-stack/4.3-router-configuration
source_domain: deepwiki.com
fetched_at: '2026-09-11T19:45:23.977557Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'DeepWiki AI-generated wiki (low-authority secondary summary, indexed 12
  Mar 2026 at commit 99ab33ab) of vllm-project/production-stack''s router configuration,
  sourced from actual repo files (helm/values.yaml, src/vllm_router/service_discovery.py,
  routing_logic.py, parser.py) with line-numbered citations back to GitHub — usable
  as a pointer to primary code, not as authoritative prose itself. Documents the router''s
  ServiceDiscovery abstraction with exactly two implementations: K8sServiceDiscovery
  (watches pods via the K8s API, selectable as ''pod-ip'' direct-to-pod or ''service-name''
  via K8s Service, label selector auto-derived from servingEngineSpec.labels) and
  StaticServiceDiscovery, explicitly labeled ''for non-Kubernetes deployments or testing''
  — configured via routerSpec.serviceDiscovery: static plus comma-separated staticBackends/staticModels/staticAliases
  URL lists, matched positionally. Routing logic is a separate pluggable axis from
  service discovery (roundrobin, session with consistent-hash ring + QPS fallback,
  kvaware requiring an LMCache controller port, prefixaware with tokenization-based
  prefix matching, disaggregated_prefill / disaggregated_prefill_orchestrated requiring
  prefill/decode model-label args) — meaning any routing policy can in principle be
  paired with static (non-K8s) discovery except where the policy''s docs imply a specific
  engine-labeling scheme. Router exposes /health for liveness/readiness/startup probes
  and is architected as a FastAPI app whose initialize_all() wires ServiceDiscovery
  + RoutingLogic + EngineStatsScraper + RequestStatsMonitor independently — i.e.,
  the discovery backend and the routing algorithm are decoupled components in this
  codebase''s own architecture diagram.'
utility_score: 9.0
---

Index your code with Devin
[DeepWiki](https://deepwiki.com/)
[DeepWiki](https://deepwiki.com/)
[vllm-project/production-stack ](https://github.com/vllm-project/production-stack "Open repository")
Index your code with
Devin
Edit WikiShare
Last indexed: 12 March 2026 ([99ab33](https://github.com/vllm-project/production-stack/commits/99ab33ab))
  * [VLLMRouter and CacheServer CRDs](https://deepwiki.com/vllm-project/production-stack/10.3-vllmrouter-and-cacheserver-crds)


# Router Configuration
Relevant source files
  * [helm/templates/deployment-router.yaml](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml)
  * [helm/templates/deployment-vllm-multi.yaml](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-vllm-multi.yaml)
  * [src/vllm_router/dynamic_config.py](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/dynamic_config.py)
  * [src/vllm_router/parsers/parser.py](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/parsers/parser.py)
  * [src/vllm_router/routers/routing_logic.py](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/routers/routing_logic.py)
  * [src/vllm_router/service_discovery.py](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/service_discovery.py)
  * [src/vllm_router/services/request_service/request.py](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/services/request_service/request.py)


This page documents how to configure the vLLM router component through Helm chart values. The router acts as an intelligent traffic director that discovers serving engine instances and routes client requests using pluggable strategies.
**Scope** : This page covers Helm-based router configuration in `routerSpec`. For detailed information about router internals, see [vLLM Router](https://deepwiki.com/vllm-project/production-stack/5-vllm-router). For serving engine configuration, see [Serving Engine Configuration](https://deepwiki.com/vllm-project/production-stack/4.2-serving-engine-configuration). For complete values reference, see [Values Reference](https://deepwiki.com/vllm-project/production-stack/4.1-values-reference).
## Router Configuration Architecture
The following diagram shows how `routerSpec` values flow through Helm templates to generate Kubernetes resources and initialize the router application:
**Diagram: Router Configuration Flow**

```



processed by
generates
generates (if enabled)
creates
runs
initializes
values.yamlrouterSpec section
deployment-router.yamlHelm template
Deploymentrouter-deployment
Servicerouter-service
Ingress(optional)
HTTPRoute(optional)
Router Pod
app.pyinitialize_all()
ServiceDiscovery(K8s or Static)
RoutingLogic(roundrobin/session/kvaware/etc)
EngineStatsScraper
RequestStatsMonitor




```

**Sources** : [helm/values.yaml367-607](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L367-L607) [helm/templates/deployment-router.yaml1-200](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L1-L200) [src/vllm_router/app.py142-323](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/app.py#L142-L323)
## Basic Router Settings
### Image Configuration
Configure the router container image and pull policy:

```

```


routerSpec:




  repository: "lmcache/lmstack-router"




  tag: "latest"




  imagePullPolicy: "Always"




  imagePullSecrets:




name: my-registry-secret


```

```
  
| Field  | Type  | Default  | Description  |  
| --- | --- | --- | --- |  
| `routerSpec.repository`  | string  | `"lmcache/lmstack-router"`  | Docker image repository  |  
| `routerSpec.tag`  | string  | `"latest"`  | Image tag  |  
| `routerSpec.imagePullPolicy`  | string  | `"Always"`  | Image pull policy  |  
| `routerSpec.imagePullSecrets`  | list  | Secrets for private registries  |  
**Sources** : [helm/values.yaml370-385](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L370-L385) [helm/templates/deployment-router.yaml47-58](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L47-L58)
### Replica Configuration
Control the number of router instances:

```

```


routerSpec:




  replicaCount: 1




  # Optional: Horizontal Pod Autoscaling



  autoscaling:




    enabled: false




    minReplicas: 1




    maxReplicas: 3




    targetCPUUtilizationPercentage: 80


```

```

When `autoscaling.enabled` is `true`, the deployment uses `autoscaling.minReplicas` as the initial replica count.
**Sources** : [helm/values.yaml387-395](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L387-L395) [helm/templates/deployment-router.yaml11-15](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L11-L15)
### Resource Allocation
Define CPU and memory requirements:

```

```


routerSpec:




  resources:




    requests:




      cpu: 400m




      memory: 1000Mi




    limits:




      memory: 1000Mi


```

```

The router typically requires modest resources compared to GPU-bound serving engines.
**Sources** : [helm/values.yaml473-479](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L473-L479) [helm/templates/deployment-router.yaml157-173](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L157-L173)
### Security Context
Configure pod and container security settings:

```

```


routerSpec:



  # Pod-level security context



  securityContext:




    runAsUser: 1000




    runAsGroup: 1000




    fsGroup: 1000




  # Container-level security context



  containerSecurityContext:




    runAsNonRoot: true




    allowPrivilegeEscalation: false




    capabilities:




      drop:




ALL




    readOnlyRootFilesystem: true


```

```

**Sources** : [helm/values.yaml375-379](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L375-L379) [helm/templates/deployment-router.yaml51-62](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L51-L62)
## Service Discovery Configuration
**Diagram: Service Discovery Configuration Mapping**

```



ServiceDiscovery Implementation
Router Arguments
values.yaml Configuration
k8s
static
serviceDiscovery:k8s or static
k8s mode:- k8sServiceDiscoveryType- k8sNamespace- k8sLabelSelector
static mode:- staticBackends- staticModels- staticAliases
--service-discovery
--k8s-namespace--k8s-service-discovery-type--k8s-label-selector
--static-backends--static-models--static-aliases
K8sServiceDiscoveryWatches pods via K8s API
StaticServiceDiscoveryUses fixed URLs




```

**Sources** : [helm/values.yaml428-434](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L428-L434) [helm/templates/deployment-router.yaml105-122](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L105-L122) [src/vllm_router/parsers/parser.py133-208](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/parsers/parser.py#L133-L208)
### Kubernetes Service Discovery
When using Kubernetes service discovery, the router automatically discovers serving engine pods:

```

```


routerSpec:




  serviceDiscovery: "k8s"




  k8sServiceDiscoveryType: "pod-ip"  # or "service-name"



  # k8sNamespace is automatically set to .Release.Namespace


  # k8sLabelSelector is automatically set from servingEngineSpec.labels

```

```

The template automatically constructs the label selector from `servingEngineSpec.labels`:

```
environment=test,release=test

```

**Service Discovery Types** :
  * `pod-ip`: Router connects directly to pod IPs (default)
  * `service-name`: Router connects through Kubernetes services


**Sources** : [helm/values.yaml428-431](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L428-L431) [helm/templates/deployment-router.yaml107-116](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L107-L116) [src/vllm_router/service_discovery.py427-728](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/service_discovery.py#L427-L728)
### Static Service Discovery
For non-Kubernetes deployments or testing, configure static backends:

```

```


routerSpec:




  serviceDiscovery: "static"




  staticBackends: "http://model1:8000,http://model2:8000"




  staticModels: "llama-3.1-8b,opt-125m"




  staticAliases: "gpt-4:llama-3.1-8b"


```

```

Each comma-separated value corresponds positionally (first backend serves first model).
**Sources** : [helm/values.yaml432-434](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L432-L434) [helm/templates/deployment-router.yaml117-122](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L117-L122) [src/vllm_router/service_discovery.py206-354](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/service_discovery.py#L206-L354)
## Routing Logic Configuration
**Diagram: Routing Logic Options**

```



requires
routingLogic
roundrobinRoundRobinRouter
sessionSessionRouterRequires: sessionKey
kvawareKvawareRouterRequires: lmcacheControllerPort
prefixawarePrefixAwareRouter
disaggregated_prefillDisaggregatedPrefillRouter
disaggregated_prefill_orchestratedDisaggregatedPrefillOrchestratedRouter
sessionKey header/field
LMCache controller
prefillModelLabelsdecodeModelLabels




```

**Sources** : [helm/values.yaml436-440](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L436-L440) [helm/templates/deployment-router.yaml123-128](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L123-L128) [src/vllm_router/routers/routing_logic.py52-59](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/routers/routing_logic.py#L52-L59)
### Round Robin Routing
Distributes requests evenly across all available backends:

```

```


routerSpec:




  routingLogic: "roundrobin"


```

```

Implemented by `RoundRobinRouter` which maintains a counter and cycles through sorted endpoints.
**Sources** : [helm/values.yaml437](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L437-L437) [src/vllm_router/routers/routing_logic.py140-181](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/routers/routing_logic.py#L140-L181)
### Session-Based Routing
Routes requests from the same session to the same backend for cache locality:

```

```


routerSpec:




  routingLogic: "session"




  sessionKey: "x-user-id"  # Header or JSON field containing session ID


```

```

The router uses a consistent hash ring to map session IDs to backends. Sessions without IDs are routed based on QPS.
**Sources** : [helm/values.yaml437-440](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L437-L440) [helm/templates/deployment-router.yaml125-128](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L125-L128) [src/vllm_router/routers/routing_logic.py183-235](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/routers/routing_logic.py#L183-L235)
### KV-Aware Routing
Routes requests to the backend with the longest prefix match in LMCache:

```

```


routerSpec:




  routingLogic: "kvaware"




  sessionKey: "x-session-id"




  lmcacheControllerPort: 9000


```

```

Requires LMCache controller integration and serving engines configured with `lmcacheConfig.enableController: true`.
**Sources** : [helm/values.yaml437-440](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L437-L440) [helm/templates/deployment-router.yaml144-147](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L144-L147) [src/vllm_router/routers/routing_logic.py237-401](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/routers/routing_logic.py#L237-L401)
### Prefix-Aware Routing
Routes based on prompt prefix similarity using tokenization:

```

```


routerSpec:




  routingLogic: "prefixaware"




  sessionKey: "x-session-id"


```

```

Falls back to session-based routing when no prefix match is found.
**Sources** : [helm/values.yaml437](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L437-L437) [src/vllm_router/routers/routing_logic.py403-526](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/routers/routing_logic.py#L403-L526)
### Disaggregated Prefill Routing
Separates prefill and decode workloads across different GPU instances:

```

```


routerSpec:




  routingLogic: "disaggregated_prefill"  # or "disaggregated_prefill_orchestrated"




  extraArgs:




"--prefill-model-labels"




"prefill"




"--decode-model-labels"




"decode"


```

```

Requires serving engines labeled with prefill/decode roles. For details, see [Disaggregated Inference](https://deepwiki.com/vllm-project/production-stack/8.2-disaggregated-inference).
**Sources** : [helm/values.yaml437](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L437-L437) [src/vllm_router/routers/routing_logic.py528-709](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/routers/routing_logic.py#L528-L709) [src/vllm_router/parsers/parser.py396-408](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/parsers/parser.py#L396-L408)
## Service Exposure
### Service Type Configuration
Configure how the router service is exposed:

```

```


routerSpec:




  serviceType: ClusterIP  # or NodePort, LoadBalancer




  servicePort: 80




  containerPort: 8000




  # For NodePort, optionally fix the port



  nodePort: 30080




  # Optional service annotations



  serviceAnnotations:




    service.beta.kubernetes.io/aws-load-balancer-type: "nlb"


```

```

**Sources** : [helm/values.yaml417-426](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L417-L426) [helm/templates/service-router.yaml](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/service-router.yaml)
### Ingress Configuration
Expose the router through a Kubernetes Ingress:

```

```


routerSpec:




  ingress:




    enabled: true




    className: "nginx"




    annotations:




      cert-manager.io/cluster-issuer: "letsencrypt-prod"




    hosts:




host: vllm-router.example.com




        paths:




path: /




            pathType: Prefix




    tls:




secretName: vllm-router-tls




        hosts:




vllm-router.example.com


```

```

**Sources** : [helm/values.yaml489-514](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L489-L514)
### Gateway API HTTPRoute
For advanced routing with Gateway API:

```

```


routerSpec:




  route:




    main:




      enabled: true




      apiVersion: gateway.networking.k8s.io/v1




      kind: HTTPRoute




      hostnames:




vllm-router.example.com




      parentRefs:




name: acme-gateway




      matches:




path:




            type: PathPrefix




            value: /



      # Optional: redirect HTTP to HTTPS



      httpsRedirect: false


```

```

**Sources** : [helm/values.yaml516-553](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L516-L553)
## Monitoring Configuration
### Engine Statistics Scraping
Configure periodic collection of serving engine metrics:

```

```


routerSpec:




  engineScrapeInterval: 15  # seconds


```

```

The router scrapes metrics like `num_requests_running`, `num_requests_waiting`, and `gpu_cache_usage_perc` from each backend's `/metrics` endpoint.
Implemented by `EngineStatsScraper` in [src/vllm_router/stats/engine_stats.py](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/stats/engine_stats.py)
**Sources** : [helm/values.yaml450-451](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L450-L451) [helm/templates/deployment-router.yaml133-136](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L133-L136) [src/vllm_router/stats/engine_stats.py](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/stats/engine_stats.py)
### Request Statistics Monitoring
Track request-level performance metrics:

```

```


routerSpec:




  requestStatsWindow: 60  # seconds


```

```

Maintains a sliding window of request statistics including QPS, latency, and TTFT per backend.
Implemented by `RequestStatsMonitor` in [src/vllm_router/stats/request_stats.py](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/stats/request_stats.py)
**Sources** : [helm/values.yaml453-454](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L453-L454) [helm/templates/deployment-router.yaml137-140](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L137-L140) [src/vllm_router/stats/request_stats.py](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/stats/request_stats.py)
### OpenTelemetry Tracing
Enable distributed tracing with OpenTelemetry:

```

```


routerSpec:




  otel:




    endpoint: "otel-collector:4317"




    serviceName: "vllm-router"




    secure: false  # Set true for TLS


```

```

The router creates spans for each request, including routing decisions and backend communication.
**Sources** : [helm/values.yaml456-464](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L456-L464) [helm/templates/deployment-router.yaml148-156](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L148-L156) [src/vllm_router/experimental/otel.py](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/experimental/otel.py)
## Security Configuration
### vLLM API Key
Secure communication between router and serving engines:

```

```

# Option 1: Use the same API key configured in servingEngineSpec



servingEngineSpec:




  vllmApiKey: "vllm_xxxxxxxxxxxxx"




# Option 2: Reference an existing secret (router-only deployment)



routerSpec:




  vllmApiKey:




    secretName: "my-vllm-secret"




    secretKey: "api-key"


```

```

When `servingEngineSpec.vllmApiKey` is set, the router automatically inherits it. For router-only deployments (no serving engines), use `routerSpec.vllmApiKey`.
The API key is injected as the `VLLM_API_KEY` environment variable and sent in the `Authorization` header.
**Sources** : [helm/values.yaml469-471](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L469-L471) [helm/templates/deployment-router.yaml74-99](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L74-L99) [helm/values.schema.json19-40](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.schema.json#L19-L40)
## Advanced Configuration
### Root Path
Host the router under a subpath:

```

```


routerSpec:




  rootPath: "/vllm"


```

```

This sets the FastAPI `root_path` parameter, making the router accessible at `http://host/vllm/v1/chat/completions` instead of `http://host/v1/chat/completions`.
**Sources** : [helm/values.yaml442-443](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L442-L443) [helm/templates/deployment-router.yaml129-132](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L129-L132) [src/vllm_router/parsers/parser.py127-131](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/parsers/parser.py#L127-L131)
### Extra Arguments
Pass additional command-line arguments to the router:

```

```


routerSpec:




  extraArgs:




"--log-level"




"debug"




"--feature-gates"




"SemanticCache=true"




"--semantic-cache-model"




"sentence-transformers/all-MiniLM-L6-v2"


```

```

All arguments from [src/vllm_router/parsers/parser.py118-442](https://github.com/vllm-project/production-stack/blob/99ab33ab/src/vllm_router/parsers/parser.py#L118-L442) are supported.
**Sources** : [helm/values.yaml445-446](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L445-L446) [helm/templates/deployment-router.yaml141-143](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L141-L143)
### Extra Service Ports
Expose additional ports from the router:

```

```


routerSpec:




  extraPorts:




name: metrics




      port: 9090




      targetPort: 9090




      protocol: TCP


```

```

**Sources** : [helm/values.yaml447-448](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L447-L448) [helm/templates/_helpers.tpl76-81](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/_helpers.tpl#L76-L81)
### Node Affinity
Control router pod placement:

```

```


routerSpec:



  # Option 1: Full affinity specification



  affinity:




    nodeAffinity:




      requiredDuringSchedulingIgnoredDuringExecution:




        nodeSelectorTerms:




matchExpressions:




key: node-role.kubernetes.io/control-plane




                operator: DoesNotExist




  # Option 2: Simplified node selector terms (ignored if affinity is set)



  nodeSelectorTerms:




matchExpressions:




key: kubernetes.io/hostname




          operator: In




          values:




node-1




node-2


```

```

**Sources** : [helm/values.yaml555-566](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L555-L566) [helm/templates/deployment-router.yaml35-46](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L35-L46)
## Health Probes
The router exposes a `/health` endpoint for Kubernetes health checks:

```

```


routerSpec:




  livenessProbe:




    initialDelaySeconds: 30




    periodSeconds: 5




    failureThreshold: 3




    httpGet:




      path: /health





  startupProbe:




    initialDelaySeconds: 5




    periodSeconds: 5




    failureThreshold: 3




    httpGet:




      path: /health





  readinessProbe:




    initialDelaySeconds: 30




    periodSeconds: 5




    failureThreshold: 3




    httpGet:




      path: /health


```

```

The port is automatically set to `routerSpec.containerPort`.
**Sources** : [helm/values.yaml571-593](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L571-L593) [helm/templates/deployment-router.yaml179-199](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/templates/deployment-router.yaml#L179-L199)
## Configuration Examples
### Minimal Router Configuration

```

```


routerSpec:




  enableRouter: true




  routingLogic: "roundrobin"




  serviceDiscovery: "k8s"


```

```

When deploying with serving engines, this is sufficient. The router automatically discovers engines using label selectors.
### Session-Based Routing with Static Backends

```

```


routerSpec:




  routingLogic: "session"




  sessionKey: "x-user-id"




  serviceDiscovery: "static"




  staticBackends: "http://model1:8000,http://model2:8000"




  staticModels: "llama-3.1-8b,llama-3.1-8b"


```

```

### Production Router with Monitoring

```

```


routerSpec:




  replicaCount: 3




  routingLogic: "kvaware"




  sessionKey: "x-session-id"




  lmcacheControllerPort: 9000





  resources:




    requests:




      cpu: 1000m




      memory: 2Gi




    limits:




      memory: 2Gi





  engineScrapeInterval: 10




  requestStatsWindow: 60





  otel:




    endpoint: "otel-collector:4317"




    serviceName: "vllm-router-prod"




    secure: true





  ingress:




    enabled: true




    className: "nginx"




    hosts:




host: vllm.example.com




        paths:




path: /




            pathType: Prefix




    tls:




secretName: vllm-tls




        hosts:




vllm.example.com


```

```

**Sources** : [helm/values.yaml367-607](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/values.yaml#L367-L607) [helm/README.md29-283](https://github.com/vllm-project/production-stack/blob/99ab33ab/helm/README.md?plain=1#L29-L283)
## Related Pages
  * [Values Reference](https://deepwiki.com/vllm-project/production-stack/4.1-values-reference) - Complete Helm values specification
  * [Serving Engine Configuration](https://deepwiki.com/vllm-project/production-stack/4.2-serving-engine-configuration) - Configure vLLM serving engines
  * [vLLM Router](https://deepwiki.com/vllm-project/production-stack/5-vllm-router) - Router architecture deep dive
  * [Service Discovery](https://deepwiki.com/vllm-project/production-stack/5.2-service-discovery) - Service discovery implementations
  * [Routing Strategies](https://deepwiki.com/vllm-project/production-stack/5.3-routing-strategies) - Detailed routing algorithm descriptions
  * [Dynamic Configuration](https://deepwiki.com/vllm-project/production-stack/5.5-dynamic-configuration) - Runtime configuration updates


Dismiss
Refresh this wiki
Enter email to refresh
### On this page
  * [Router Configuration Architecture](https://deepwiki.com/vllm-project/production-stack/4.3-router-configuration#router-configuration-architecture)
  * [Service Discovery Configuration](https://deepwiki.com/vllm-project/production-stack/4.3-router-configuration#service-discovery-configuration)
  * [Kubernetes Service Discovery](https://deepwiki.com/vllm-project/production-stack/4.3-router-configuration#kubernetes-service-discovery)
  * [Static Service Discovery](https://deepwiki.com/vllm-project/production-stack/4.3-router-configuration#static-service-discovery)
  * [Routing Logic Configuration](https://deepwiki.com/vllm-project/production-stack/4.3-router-configuration#routing-logic-configuration)
  * [Disaggregated Prefill Routing](https://deepwiki.com/vllm-project/production-stack/4.3-router-configuration#disaggregated-prefill-routing)
  * [Service Type Configuration](https://deepwiki.com/vllm-project/production-stack/4.3-router-configuration#service-type-configuration)
  * [Monitoring Configuration](https://deepwiki.com/vllm-project/production-stack/4.3-router-configuration#monitoring-configuration)
  * [Engine Statistics Scraping](https://deepwiki.com/vllm-project/production-stack/4.3-router-configuration#engine-statistics-scraping)
  * [Request Statistics Monitoring](https://deepwiki.com/vllm-project/production-stack/4.3-router-configuration#request-statistics-monitoring)
  * [Minimal Router Configuration](https://deepwiki.com/vllm-project/production-stack/4.3-router-configuration#minimal-router-configuration)
  * [Session-Based Routing with Static Backends](https://deepwiki.com/vllm-project/production-stack/4.3-router-configuration#session-based-routing-with-static-backends)
  * [Production Router with Monitoring](https://deepwiki.com/vllm-project/production-stack/4.3-router-configuration#production-router-with-monitoring)


