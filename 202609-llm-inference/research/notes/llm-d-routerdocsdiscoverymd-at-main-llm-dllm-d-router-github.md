---
title: llm-d-router/docs/discovery.md at main · llm-d/llm-d-router · GitHub
id: llm-d-routerdocsdiscoverymd-at-main-llm-dllm-d-router-github
tags:
- llmd-router-nonk8s
- llmd-router-nonk8s-backends-54b774
- endpoint-discovery
- file-discovery
- custom-plugin
- go-source
created: '2026-09-11T20:55:01.580995Z'
updated: '2026-09-15T04:55:23.892205Z'
source: https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md
source_domain: github.com
fetched_at: '2026-09-11T20:55:01.558956Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'PRIMARY SOURCE, HIGHEST VALUE FOR THE RESEARCH QUERY. Canonical docs/discovery.md
  on llm-d/llm-d-router main branch (fetched Sep 11 2026, unpinned commit; referenced
  as new documentation delivered by PR #1007). This is the definitive, most detailed
  spec of the discovery subsystem and RESOLVES a config-path discrepancy seen across
  other sources: the authoritative field path is dataLayer.discovery.endpoints.pluginRef
  (confirmed by two full worked YAML examples), NOT the blog''s abbreviated dataLayer.discovery.pluginRef
  nor the discovery.go source comment''s EndpointPickerConfig.discovery.pluginRef
  -- both of those are imprecise shorthand for the same real, more deeply nested path.
  Includes a COMPLETE WORKING CODE SKELETON for writing a from-scratch custom discovery
  plugin (section Writing a custom discovery plugin): implement fwkdl.EndpointDiscovery,
  define a PluginType constant, a Factory(name string, params *json.Decoder, _ fwkplugin.Handle)
  (fwkplugin.Plugin, error) function, a TypedName() accessor, and a Start(ctx, notifier)
  method that upserts known endpoints then blocks on <-ctx.Done(); register the factory
  via fwkplugin.Register(myplugin.PluginType, myplugin.Factory) before calling Run(),
  then reference the plugin type string in the EndpointPickerConfig plugins list and
  point dataLayer.discovery.endpoints.pluginRef at it. Documents the exact EndpointMetadata
  struct fields discovery plugins populate: types.NamespacedName (ID), Name, Address,
  Port, MetricsHost (defaults to address:port if empty -- i.e. a discovery plugin
  CAN report a separate metrics endpoint per backend at the datastore level, even
  though the basic file-discovery YAML schema does not expose a metricsHost key directly),
  and Labels (map[string]string). Gives the full ASCII architecture diagram showing
  EndpointDiscovery plugin -> Upsert/Delete -> Datastore -> Scheduler -> x-gateway-destination-endpoint
  header -> Envoy ORIGINAL_DST cluster. Documents pod-readiness semantics specific
  to Kubernetes discovery (aggregate Ready condition, honors custom readinessGates,
  distinct from container-ready count shown in kubectl) -- notably a K8s-only mechanism
  a custom backend must replicate itself (e.g. only Upsert healthy endpoints) since
  file/custom discovery has no equivalent gate. Lists the SAME three Kubernetes-only
  inactive features as the file-discovery plugin README (InferenceModelRewrite, InferenceObjective,
  k8s-notification-source) but with more precise scope statements for each, and NOTABLY
  DOES NOT mention FlowControl as affected -- a discrepancy versus llm-d/llm-d''s
  docs/infrastructure/no-kubernetes-deployment.md, which named ''InferenceObjective-driven
  FlowControl'' as out of scope; this doc frames the same fact as InferenceObjective-driven
  per-request SLO objectives specifically, without describing FlowControl itself as
  disabled. Documents --pool-namespace resolution order (flag > NAMESPACE env var
  via K8s Downward API > default fallback) with an explicit warning that the fallback
  produces a Kubernetes-flavored label outside Kubernetes unless set explicitly. New
  validation detail not in the plugin README: duplicate endpoint names within the
  same namespace resolve with last-Upsert-wins, since the file is processed top-to-bottom.
  Confirms file-discovery''s own backward-compatibility contract: when dataLayer.discovery.endpoints
  is absent, the EPP falls back to Kubernetes-based discovery -- no breaking change
  for existing deployments. Includes a complete, runnable end-to-end example: endpoints.yaml,
  EPP config.yaml (file-discovery + kv-cache-utilization-scorer + random-picker +
  single-profile-handler), the epp binary invocation, and a full Envoy YAML with the
  ext_proc filter (FULL_DUPLEX_STREAMED on all four body/trailer modes) and an ORIGINAL_DST
  cluster keyed on the x-gateway-destination-endpoint header.'
utility_score: 17.0
---

*Suggested by [[discovery-plugins-k8s-and-file-based-endpoint-discovery-by-ezrasilvera-pull-requ]] — canonical doc for all three built-in discovery plugins, referenced in PR #1007 description*

[Skip to content](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# discovery.md
Copy path
More file actions
More file actions
## Latest commit
[docs: document that endpoint discovery honours custom pod readinessGa…](https://github.com/llm-d/llm-d-router/commit/aab0c190ce9e18ee6a5587589be1b881bc7da87c)
Open commit detailsfailure
Sep 2, 2026
[aab0c19](https://github.com/llm-d/llm-d-router/commit/aab0c190ce9e18ee6a5587589be1b881bc7da87c) · Sep 2, 2026
## History
[History](https://github.com/llm-d/llm-d-router/commits/main/docs/discovery.md)
Open commit details
History
634 lines (506 loc) · 19.3 KB
Code owner: @llm-d/router-maintainers
/
# discovery.md
Copy path
## File metadata and controls
  * Preview
  * 

634 lines (506 loc) · 19.3 KB
Code owner: @llm-d/router-maintainers
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Outline
Edit and raw actions
# Endpoint Discovery
## Table of Contents
  * [Pod readiness in Kubernetes discovery](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#pod-readiness-in-kubernetes-discovery)
  * [The EndpointDiscovery Interface](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#the-endpointdiscovery-interface)
    * [EndpointDiscovery](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#endpointdiscovery)
    * [DiscoveryNotifier](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#discoverynotifier)
    * [Ordering contract](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#ordering-contract)
    * [Wiring to the datastore](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#wiring-to-the-datastore)
  * [Selecting a discovery plugin in the EPP config](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#selecting-a-discovery-plugin-in-the-epp-config)
  * [File Discovery plugin](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#file-discovery-plugin)
    * [Endpoints file format](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#endpoints-file-format)
    * [Validation rules](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#validation-rules)
  * [Running EPP with file discovery (no Kubernetes)](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#running-epp-with-file-discovery-no-kubernetes)
    * [What you need](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#what-you-need)
    * [1. Endpoints file](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#1-endpoints-file)
    * [2. EPP config](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#2-epp-config)
    * [3. Start the EPP](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#3-start-the-epp)
    * [4. Envoy config](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#4-envoy-config)
    * [5. Start Envoy](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#5-start-envoy)
  * [Writing a custom discovery plugin](https://github.com/llm-d/llm-d-router/blob/main/docs/discovery.md#writing-a-custom-discovery-plugin)


## Overview
By default the EPP discovers inference endpoints by watching a Kubernetes `InferencePool` object and the pods it selects. The **discovery plugin interface** replaces that mechanism with a pluggable alternative so the EPP can run in environments where Kubernetes is not available -- for example bare metal inference clusters, Slurm jobs, or local development.
When a discovery plugin is configured:
  * The EPP **does not** call `ctrl.GetConfig()` or start a controller manager.
  * The EPP **does not** require an `InferencePool` CRD or any K8s RBAC.
  * The discovery plugin is solely responsible for populating the endpoint datastore via `DiscoveryNotifier`.
  * All scheduling, request control, and metrics collection behaviour is unchanged.


## Pod readiness in Kubernetes discovery
In the default Kubernetes mode the EPP admits a pod into the routing pool only when the pod's aggregate **`Ready`**condition is`True` (see `pkg/epp/util/pod/pod.go`). A pod whose `Ready` condition is `False` or `Unknown`, or that has a deletion timestamp, is removed from the datastore and receives no traffic.
Reading the aggregate `Ready` condition means custom [pod readiness gates](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#pod-readiness-gate) are honoured without any additional EPP configuration. The kubelet computes `Ready` as `ContainersReady` **AND** every condition listed in `spec.readinessGates`, so an unsatisfied gate holds the pod out of the routing pool even when its container probes already pass:

```
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: vllm          # selected by the InferencePool
spec:
  readinessGates:
    - conditionType: "custom.orchestrator.io/serving"
```

Until an external controller patches `custom.orchestrator.io/serving` to `True` on the pod's status, the pod reports:

```
Ready=False
ContainersReady=True

```

and the EPP does not route to it. Once the condition flips to `True`, the kubelet sets `Ready=True` and the pod joins the pool on the next reconcile.
Note that a gated pod still shows `1/1` in `kubectl get pods`: that column counts ready _containers_ , while the EPP reads the `Ready` condition.
## Architecture

```
  Client request
       |
       v
  +--------+          +------------------------------+
  |  Envoy |--ext_proc-->        EPP (port 9002)      |
  +--------+  gRPC    |                              |
       |               |  Scheduler  |  Datastore    |
       |               |             |   endpoints   |
       |               |          ^  |               |
       |               +----------|--+               |
       |                          |
       |               +----------+----------+
       |               |   EndpointDiscovery  |
       |               |   plugin             |
       |               |                      |
       |               |  (file-discovery)    |
       |               |  reads endpoints.yaml|
       |               |  calls Upsert/Delete |
       |               +---------------------+
       |
       | x-gateway-destination-endpoint header set by EPP
       v
  selected inference endpoint (e.g. 10.0.0.1:8000)

```

The EPP sets the `x-gateway-destination-endpoint` header on the response to `ext_proc`. Envoy's `ORIGINAL_DST` cluster reads that header and forwards the request to the chosen endpoint.
## The EndpointDiscovery Interface
### EndpointDiscovery

```
// pkg/epp/framework/interface/datalayer/discovery.go

type EndpointDiscovery interface {
    plugin.Plugin   // provides TypedName() TypedName

    // Start begins discovery and blocks until ctx is cancelled or a fatal
    // error occurs. It must be called in a dedicated goroutine.
    //
    // Implementations SHOULD enumerate all currently known endpoints via
    // notifier.Upsert before entering the watch loop to avoid serving an
    // empty datastore at startup.
    Start(ctx context.Context, notifier DiscoveryNotifier) error
}
```

### DiscoveryNotifier

```
type DiscoveryNotifier interface {
    // Upsert adds or updates an endpoint in the datastore.
    Upsert(endpoint *EndpointMetadata)
    // Delete removes an endpoint by its ID.
    Delete(id types.NamespacedName)
}
```

`EndpointMetadata` fields relevant to discovery:  
| Field  | Type  | Description  |  
| --- | --- | --- |  
| `types.NamespacedName`  | Unique identity of the endpoint. Each discovery source must set it uniquely across the endpoints it reports.  |  
| `Name`  | `string`  | Name of the workload behind the endpoint.  |  
| `Address`  | `string`  | IP address of the inference server.  |  
| `Port`  | `string`  | Port as a string (e.g. `"8000"`).  |  
| `MetricsHost`  | `string`  |  `host:port` for metrics scraping. Defaults to `address:port` if empty.  |  
| `Labels`  | `map[string]string`  | Arbitrary labels available to scheduler plugins.  |  
### Ordering contract
`DiscoveryNotifier` is **not goroutine-safe**. All `Upsert` and `Delete` calls must be made sequentially from a single goroutine and in causal order. An `Upsert` followed by a `Delete` for the same endpoint must arrive in that order or the endpoint will be incorrectly left in the datastore.
### Wiring to the datastore
The runner creates a `DiscoveryNotifier` backed by the datastore and passes it to `EndpointDiscovery.Start`:

```
disc.Start(ctx, fwkdl.NewDiscoveryNotifier(ds))
```

`NewDiscoveryNotifier` accepts any value that implements `DiscoveryBackendStore` (i.e. has `BackendUpsert` and `BackendDelete`), which the production `Datastore` satisfies.
## Selecting a discovery plugin in the EPP config
Add a `discovery` section inside `dataLayer` in the `EndpointPickerConfig`. The `pluginRef` field names a plugin instance defined in the top-level `plugins` list.

```
plugins:
  - name: my-endpoints          # the name referenced by pluginRef below
    type: file-discovery        # plugin type registered in the runner
    parameters:
      path: /etc/epp/endpoints.yaml
      watchFile: true

# ... other plugins (scorers, filters, etc.) ...

dataLayer:
  discovery:
    endpoints:
      pluginRef: my-endpoints   # must match a name in plugins above
```

When `dataLayer.discovery.endpoints` is present the EPP skips the Kubernetes setup entirely. When it is absent the EPP uses the default Kubernetes-based discovery (backwards compatible).
## File Discovery plugin
**Plugin type:** `file-discovery`
Reads a static YAML or JSON file that lists inference endpoints, calls `DiscoveryNotifier.Upsert` for each entry, and optionally watches the file for changes using `fsnotify`.
### Parameters  
| Parameter  | Type  | Required  | Default  | Description  |  
| --- | --- | --- | --- | --- |  
| `path`  | string  | yes  | --  | Absolute or relative path to the endpoints file.  |  
| `watchFile`  | bool  | no  | `false`  | Re-read the file and reconcile the datastore whenever the file is written or replaced.  |  
### Endpoints file format
The file may be YAML or JSON. YAML is recommended for readability.

```
endpoints:
  - name: <string>              # required -- unique within the file
    namespace: <string>         # optional -- defaults to "default"
    address: <IPv4>             # required -- must be a valid IPv4 address
    port: <string>              # required -- integer 1-65535 as a string
    labels:                     # optional -- arbitrary key/value labels
      <key>: <value>
```

Example with two vLLM instances:

```
endpoints:
  - name: vllm-0
    address: "10.0.0.1"
    port: "8000"
    labels:
      model: llama-3-8b

  - name: vllm-1
    address: "10.0.0.2"
    port: "8000"
    labels:
      model: llama-3-8b
```

Example with multi-rank (tensor-parallel) setup where each rank is a separate endpoint:

```
endpoints:
  - name: tp-rank-0
    namespace: inference
    address: "192.168.1.10"
    port: "8000"
    labels:
      rank: 
      job: tp-job-42

  - name: tp-rank-1
    namespace: inference
    address: "192.168.1.11"
    port: "8000"
    labels:
      rank: 
      job: tp-job-42
```

**Constraints:**
  * `address` must be a literal IPv4 address (IPv6 is not supported).
  * `port` must be a decimal integer in the range 1-65535.
  * The file must not exceed 1 MiB.
  * Duplicate names within the same namespace result in the last `Upsert` winning (the file is processed top-to-bottom).


### Live reload
When `watchFile: true` the plugin watches the file via `fsnotify`. On a `Write` or `Create` event it re-reads the file, upserts all current endpoints, and deletes any endpoint that was present in the previous load but is absent from the new file. Reload errors are logged but do not terminate the plugin -- the datastore retains its previous state.
Atomic file replacement (write to a temp file then rename) triggers a `Create` event and is handled correctly.
### Validation rules
The plugin fails at startup (before `Start` returns an error) if:
  * `path` parameter is missing or empty.
  * The file cannot be opened.
  * The file exceeds 1 MiB.
  * The YAML/JSON cannot be parsed.
  * Any endpoint has an invalid `address` or `port`.


### Kubernetes-only features that do not run in file discovery mode
File discovery starts the EPP without a controller manager, so any feature that depends on a Kubernetes API server is inactive:
  * **`InferenceModelRewrite`**(model name rewriting for traffic split or canary). The reconciler is not started;`ds.ModelRewriteGet` returns nil for every request. There is no file-mode equivalent today.
  * **`InferenceObjective`**(per-request SLO objectives). The reconciler is not started; objective lookups return nil. Plugins that depend on per-request SLO data will see no objectives configured.
  * **`k8s-notification-source`data layer plugin** (and any other notification source that needs the controller manager). The plugin loads from config but is never bound to a manager, so it produces no events. Remove it from `dataLayer.sources` when running in file discovery mode.


If you copy a Kubernetes EPP config into a file-discovery deployment, expect behavior tied to these features to differ. The runner emits a one-time log at startup naming these inactive features.
### Pool namespace
The pool namespace ends up in metrics labels, log fields, and the pool's `{namespace}/{name}` identity. The runner resolves it in this order:
  1. `--pool-namespace` flag, if set.
  2. `NAMESPACE` environment variable, if set (Kubernetes injects this via the Downward API).
  3. The literal string `default`, as a last-resort fallback.


In Kubernetes the second source is always populated, so the fallback is effectively unreachable. On bare metal, in Slurm, in Ray, or in any container without the Downward API, neither source is set and the pool is labeled `default` -- a Kubernetes-flavored string that is meaningless outside K8s. Pass `--pool-namespace` explicitly so labels reflect your environment:

```
epp \
  --pool-name slurm-job-42 \
  --pool-namespace slurm \
  --config-file /etc/epp/config.yaml

```

The runner emits a startup warning when neither `--pool-namespace` nor `NAMESPACE` is set, naming the value it fell back to.
## Running EPP with file discovery (no Kubernetes)
This example runs the EPP alongside Envoy on a single machine with two vLLM processes. No Kubernetes, no `InferencePool` CRD.

```
localhost:8080  <-- Envoy (user-facing)
localhost:9002  <-- EPP gRPC (ext_proc)
localhost:9003  <-- EPP health (gRPC)
localhost:9090  <-- EPP Prometheus metrics

10.0.0.1:8000   <-- vLLM instance 0
10.0.0.2:8000   <-- vLLM instance 1

```

### What you need
  * `epp` binary built from this repo (`go build -o epp ./cmd/epp`).
  * Envoy v1.31+ binary or Docker image.
  * vLLM (or a compatible inference server) running on the target addresses.


### 1. Endpoints file
Save as `/etc/epp/endpoints.yaml`:

```
endpoints:
  - name: vllm-0
    address: "10.0.0.1"
    port: "8000"

  - name: vllm-1
    address: "10.0.0.2"
    port: "8000"
```

### 2. EPP config
Save as `/etc/epp/config.yaml`. This example uses the default KV-cache utilization scorer and random picker -- adapt scheduling plugins as needed.

```
plugins:
  - name: file-disc
    type: file-discovery
    parameters:
      path: /etc/epp/endpoints.yaml
      watchFile: true

  - name: kv-cache-scorer
    type: kv-cache-utilization-scorer

  - name: random-picker
    type: random-picker

  - name: single-profile
    type: single-profile-handler
    parameters:
      scorer: kv-cache-scorer
      picker: random-picker

schedulingProfiles:
  - name: default
    plugins:
      - pluginRef: single-profile

dataLayer:
  discovery:
    endpoints:
      pluginRef: file-disc
```

### 3. Start the EPP

```
epp \
  --pool-name epp \
  --config-file /etc/epp/config.yaml \
  --grpc-port 9002 \
  --grpc-health-port 9003 \
  --metrics-port 9090
```

`--pool-name` is optional in file discovery mode and defaults to `epp` if unset. It is used as the internal pool identifier in the scheduler and metrics. The value is arbitrary -- it does not need to match any Kubernetes object.
The EPP logs `EPP starting (file discovery mode)` on startup and emits a log line for each endpoint loaded from the file.
### 4. Envoy config
Save as `/etc/envoy/envoy.yaml`. The key pieces are:
  * A listener on `0.0.0.0:8080` that applies the `ext_proc` filter.
  * The `ext_proc` filter pointing at the EPP on `localhost:9002`.
  * An `ORIGINAL_DST` cluster that reads the `x-gateway-destination-endpoint` header written by the EPP and forwards to the chosen endpoint.



```
admin:
  address:
    socket_address:
      address: 127.0.0.1
      port_value: 19000

static_resources:
  listeners:
    - name: inference
      address:
        socket_address:
          address: 0.0.0.0
          port_value: 8080
      filter_chains:
        - filters:
            - name: envoy.filters.network.http_connection_manager
              typed_config:
                "@type": type.googleapis.com/envoy.extensions.filters.network.http_connection_manager.v3.HttpConnectionManager
                stat_prefix: inference
                route_config:
                  name: inference
                  virtual_hosts:
                    - name: inference
                      domains: ["*"]
                      routes:
                        - match:
                            prefix: 
                          route:
                            cluster: original_destination_cluster
                            timeout: 600s
                            idle_timeout: 600s
                http_filters:
                  - name: envoy.filters.http.ext_proc
                    typed_config:
                      "@type": type.googleapis.com/envoy.extensions.filters.http.ext_proc.v3.ExternalProcessor
                      grpc_service:
                        envoy_grpc:
                          cluster_name: epp
                          authority: localhost:9002
                        timeout: 10s
                      processing_mode:
                        request_header_mode: SEND
                        response_header_mode: SEND
                        request_body_mode: FULL_DUPLEX_STREAMED
                        response_body_mode: FULL_DUPLEX_STREAMED
                        request_trailer_mode: SEND
                        response_trailer_mode: SEND
                      message_timeout: 600s
                  - name: envoy.filters.http.router
                    typed_config:
                      "@type": type.googleapis.com/envoy.extensions.filters.http.router.v3.Router

  clusters:
    - name: epp
      type: STATIC
      connect_timeout: 86400s
      lb_policy: LEAST_REQUEST
      typed_extension_protocol_options:
        envoy.extensions.upstreams.http.v3.HttpProtocolOptions:
          "@type": type.googleapis.com/envoy.extensions.upstreams.http.v3.HttpProtocolOptions
          explicit_http_config:
            http2_protocol_options: {}
      load_assignment:
        cluster_name: epp
        endpoints:
          - lb_endpoints:
              - endpoint:
                  address:
                    socket_address:
                      address: 127.0.0.1
                      port_value: 9002

    - name: original_destination_cluster
      type: ORIGINAL_DST
      connect_timeout: 600s
      lb_policy: CLUSTER_PROVIDED
      circuit_breakers:
        thresholds:
          - max_connections: 10000
            max_pending_requests: 10000
            max_requests: 10000
      original_dst_lb_config:
        use_http_header: true
        http_header_name: x-gateway-destination-endpoint
```

### 5. Start Envoy

```
envoy -c /etc/envoy/envoy.yaml
```

Requests to `http://localhost:8080/v1/completions` are now routed through the EPP to one of the two vLLM instances.
## Writing a custom discovery plugin
Implement `fwkdl.EndpointDiscovery` and register the factory with the runner's plugin registry.

```
package myplugin

import (
    "context"
    "encoding/json"
    "fmt"

    "k8s.io/apimachinery/pkg/types"

    fwkdl    "github.com/llm-d/llm-d-router/pkg/epp/framework/interface/datalayer"
    fwkplugin "github.com/llm-d/llm-d-router/pkg/epp/framework/interface/plugin"
)

const PluginType = "my-discovery"

type MyDiscovery struct {
    typedName fwkplugin.TypedName
    // ... plugin state ...
}

var _ fwkdl.EndpointDiscovery = (*MyDiscovery)(nil)

func Factory(name string, params *json.Decoder, _ fwkplugin.Handle) (fwkplugin.Plugin, error) {
    if name == "" {
        name = PluginType
    }
    return MyDiscovery{
        typedName: fwkplugin.TypedName{Type: PluginType, Name: name},
    }, nil
}

func (d *MyDiscovery) TypedName() fwkplugin.TypedName { return d.typedName }

func (d *MyDiscovery) Start(ctx context.Context, notifier fwkdl.DiscoveryNotifier) error {
    // 1. Enumerate existing endpoints.
    notifier.Upsert(fwkdl.EndpointMetadata{
        ID:      types.NamespacedName{Name: "ep0", Namespace: "default"},
        Address: "10.0.0.1",
        Port:    "8000",
        MetricsHost:    "10.0.0.1:8000",
    })

    // 2. Watch for changes and call Upsert/Delete as endpoints come and go.
    //    All calls must be from this goroutine (DiscoveryNotifier is not goroutine-safe).
    <-ctx.Done()
    return nil
}
```

Register the factory in the runner before calling `Run`:

```
fwkplugin.Register(myplugin.PluginType, myplugin.Factory)
```

Then reference it in the EPP config:

```
plugins:
  - name: my-disc
    type: my-discovery
    parameters: {}

dataLayer:
  discovery:
    endpoints:
      pluginRef: my-disc
```

You can’t perform that action at this time. 
