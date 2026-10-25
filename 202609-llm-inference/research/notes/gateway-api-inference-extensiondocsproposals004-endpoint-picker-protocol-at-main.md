---
title: gateway-api-inference-extension/docs/proposals/004-endpoint-picker-protocol
  at main · kubernetes-sigs/gateway-api-inference-extension · GitHub
id: gateway-api-inference-extensiondocsproposals004-endpoint-picker-protocol-at-main
tags:
- llmd-router-nonk8s
- epp-protocol
- ext-proc
- llmd-router-nonk8s-backends-54b774
- llmd-kv-cache-routing-c665d5
created: '2026-09-11T19:42:56.819426Z'
updated: '2026-09-15T04:55:23.663459Z'
source: https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/004-endpoint-picker-protocol
source_domain: github.com
fetched_at: '2026-09-11T19:42:56.799313Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GIE proposal 004, the Endpoint Picker Protocol spec (status: Implemented,
  versioned to v1.0.0 dated 2025-07-29). Defines the wire contract between a data-plane
  proxy (e.g. Envoy) and the EPP over Envoy ext_proc, which the EPP MUST implement
  including streaming mode. Endpoint identity on the wire is purely ip:port strings,
  not k8s object references: the data plane can restrict the EPP to a subset via filterMetadata
  key envoy.lb.subset_hint / x-gateway-destination-endpoint-subset (list of ip:port);
  if unset the EPP selects from the InferencePool selector''s endpoint set. The EPP
  returns its pick via the x-gateway-destination-endpoint HTTP header AND the envoy.lb
  dynamic_metadata namespace (both required to match; mismatch is undefined behavior),
  as one or more comma-separated ip:port endpoints — first valid one used sequentially
  for retries. Constraints: 503 Service Unavailable if the EPP fails to set a destination
  when no endpoints are ready, 429 Too Many Requests to shed load. A 2026-07-23 amendment
  (PR #3003, ''clarify EPP endpoint backend scope'') scoped the ordered endpoint list
  to the specific InferencePool backend that produced it: on same-backend retry the
  data plane continues down that backend''s list; on route-level retry/failover to
  a different InferencePool backend, the data plane MUST invoke that backend''s EPP
  and MUST NOT reuse the previous backend''s endpoints, and x-gateway-destination-endpoint-served
  (health/serving confirmation) must be reported to the EPP tied to whichever backend
  produced the terminal response. Also specifies gRPC Health Checking Protocol services
  (liveness, readiness, ext_proc service check) the EPP must expose.'
utility_score: 16.0
---

[Skip to content](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/004-endpoint-picker-protocol#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/004-endpoint-picker-protocol) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/004-endpoint-picker-protocol) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/004-endpoint-picker-protocol) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/004-endpoint-picker-protocol).
/ **[gateway-api-inference-extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension) You must be signed in to change notification settings
  * [ Fork 313 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)
  * [ Star  766 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# 004-endpoint-picker-protocol
/
Copy path
## Directory actions
## More options
More options
## Directory actions
## More options
More options
## Latest commit
[docs: clarify EPP endpoint backend scope (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/f540ee91d86f0112df98da8f65d7e689149cee2c)[#3003](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/3003)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/f540ee91d86f0112df98da8f65d7e689149cee2c)
Open commit detailssuccess
Jul 23, 2026
[f540ee9](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/f540ee91d86f0112df98da8f65d7e689149cee2c) · Jul 23, 2026
## History
[History](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commits/main/docs/proposals/004-endpoint-picker-protocol)
Open commit details
History
/
# 004-endpoint-picker-protocol
/
Copy path
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| 
### parent directory
 |  
|   |   |  [docs: clarify EPP endpoint backend scope (](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/f540ee91d86f0112df98da8f65d7e689149cee2c "docs: clarify EPP endpoint backend scope \(#3003\)

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>")[#3003](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/3003)[)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/f540ee91d86f0112df98da8f65d7e689149cee2c "docs: clarify EPP endpoint backend scope \(#3003\)

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>")  | Jul 23, 2026  |  
| View all files |  
## [README.md](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/004-endpoint-picker-protocol#readme)
Outline
# Endpoint Picker Protocol
## Proposal Status
_**Implemented**_
# Proposal
The Endpoint Picker, or EPP, is a core component of the inference extension. Ultimately it's responsible for picking an endpoint from the `InferencePool`. A reference implementation can be found [here](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/pkg/epp).
This doc defines the protocol between the EPP and the proxy (e.g, Envoy).
The EPP MUST implement the Envoy [external processing service](https://www.envoyproxy.io/docs/envoy/latest/api-v3/extensions/filters/http/ext_proc/v3/ext_proc.proto) protocol.
The EPP MUST support streaming mode for inference requests and responses. Streaming mode enables full-duplex communication between the Gateway (Envoy), EPP, and model servers, allowing real-time token-by-token response delivery for AI inference workloads.
## Version History  
| Version  | Date  | Changes  |  
| --- | --- | --- |  
| v1.0.0  | 2025-07-29  | Added status metadata field for picked endpoints  |  
| v0.4.0  | 2025-06-03  | Added support for multiple fallback endpoints  |  
| v0.3.0  | 2025-03-14  | Added subsetting and fallback support  |  
| v0.2.0  | 2025-02-22  | Initial protocol definition  |  
## Endpoint Subset
[REQUEST: Data Plane -> EPP]
For each HTTP request, the data plane CAN communicate the subset of endpoints the EPP MUST pick from by setting an unstructured entry in the [filter metadata](https://github.com/envoyproxy/go-control-plane/blob/63a55395d7a39a8d43dcc7acc3d05e4cae7eb7a2/envoy/config/core/v3/base.pb.go#L819) field of the ext-proc request. The metadata entry for the subset list MUST be wrapped with an outer key (which represents the metadata namespace) with a default of `envoy.lb.subset_hint`.

```
filterMetadata: {
  "envoy.lb.subset_hint" {
     "x-gateway-destination-endpoint-subset": [ip:port, ip:port, ...]
  }
}
```

If the key `x-gateway-destination-endpoint-subset` is set, the EPP MUST only select endpoints from the specified list. If none of the endpoints in the list is eligible or the list is empty, then the EPP MUST return a [ImmediateResponse](https://github.com/envoyproxy/envoy/blob/f2023ef77bdb4abaf9feef963c9a0c291f55568f/api/envoy/service/ext_proc/v3/external_processor.proto#L195) with 503 (Service Unavailable) HTTP status code. If the EPP does not select from the list, then this leads to unpredictable behavior.
If the key `x-gateway-destination-endpoint-subset` is not set, then the EPP MUST select from the set defined by the `InferencePool` selector.
## Destination Endpoint
[REQUEST: EPP -> Data Plane]
For each HTTP request, the EPP MUST communicate to the data plane one or more selected model server endpoints via:
  1. Setting the `x-gateway-destination-endpoint` HTTP header to one or more selected endpoints.
  2. Set an unstructured entry in the [dynamic_metadata](https://github.com/envoyproxy/go-control-plane/blob/c19bf63a811c90bf9e02f8e0dc1dcef94931ebb4/envoy/service/ext_proc/v3/external_processor.pb.go#L320) field of the ext-proc response. The metadata entry for the picked endpoints MUST be wrapped with an outer key (which represents the metadata namespace) with a default of `envoy.lb`.


The primary endpoint MUST be set using the key `x-gateway-destination-endpoint` as follows:

```
dynamicMetadata: {
  "envoy.lb": {
    "x-gateway-destination-endpoint": ip:port
  }
}
```

or:

```
dynamicMetadata: {
  "envoy.lb": {
    "x-gateway-destination-endpoint": ip:port,ip:port,...
  }
}
```

The value of the header or metadata entry MUST contain at least one endpoint in `<ip:port>` format or multiple endpoints in `<ip:port>,<ip:port>,...` format. Multiple endpoints are separated by commas. The first valid endpoint in the list will be used. If retry is configured, the proxy will go sequentially down the list until one valid endpoint is found.
The ordered endpoint list is scoped to the selected `InferencePool` backend for which the EPP was invoked. If a retry remains on the same backend, the data plane MUST continue through that backend's endpoint list in order. If route-level retry or failover selects a different `InferencePool` backend, the data plane MUST invoke the EPP associated with the newly selected backend and MUST NOT reuse endpoints returned for the previous backend. This protocol does not otherwise define whether route-level retries retain or reselect a backend.
Constraints:
  * If the EPP did not communicate the server endpoint via these two methods, it MUST return an error as follows: 
    * [ImmediateResponse](https://github.com/envoyproxy/envoy/blob/f2023ef77bdb4abaf9feef963c9a0c291f55568f/api/envoy/service/ext_proc/v3/external_processor.proto#L195) with 503 (Serivce Unavailable) HTTP status code if there are no ready endpoints.
    * [ImmediateResponse](https://github.com/envoyproxy/envoy/blob/f2023ef77bdb4abaf9feef963c9a0c291f55568f/api/envoy/service/ext_proc/v3/external_processor.proto#L195) with 429 (Too Many Requests) HTTP status code if the request should be dropped (e.g., a Sheddable request, and the servers under heavy load).
  * The EPP MUST not set two different values in the header and the inner response metadata value.
  * Setting different value leads to unpredictable behavior because proxies aren't guaranteed to support both paths, and so this protocol does not define what takes precedence.


## Destination Endpoint Served
[RESPONSE: Data Plane -> EPP]
For each HTTP response, the data plane MUST communicate to the EPP the endpoint that served the request as follows:
In the ext_proc [ProcessingRequest.metadata_context](https://github.com/envoyproxy/envoy/blob/v1.35.0/api/envoy/service/ext_proc/v3/external_processor.proto#L130) field, a new metadata field "x-gateway-destination-endpoint-served" is added as an unstructured entry with an outer key (which represents the metadata namespace) with a default value of `envoy.lb`:

```
filterMetadata: {
  "envoy.lb": {
    "x-gateway-destination-endpoint-served": ip:port
}
```

This metadata is required because the EPP provides a list of endpoints to the data plane (see [Destination Endpoint](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/004-endpoint-picker-protocol#destination-endpoint)), and the data plane, according to retry configuration, will attempt each endpoint in order until the request is successful or no more endpoints are available.
If route-level retry or failover selects a different `InferencePool` backend, `x-gateway-destination-endpoint-served` MUST be reported to the EPP associated with the backend that produced the terminal response.
## Health Checking
The EPP MUST expose health check endpoints following the [gRPC Health Checking Protocol](https://github.com/grpc/grpc/blob/master/doc/health-checking.md).
### Health Check Services
The EPP exposes the following health check services:
**Liveness Check** (`liveness`): Determines if the EPP process is alive and responsive. Returns `SERVING` if the EPP process can respond to gRPC requests. This check does not depend on datastore sync status or leader election state.
**Readiness Check** (`readiness`): Determines if the EPP is ready to accept and process inference requests. Returns `SERVING` if the EPP datastore has synced and the EPP is the elected leader (in multi-replica deployments). Returns `NOT_SERVING` if the datastore has not synced or the EPP is a follower.
**External Processor Service Check** (`envoy.service.ext_proc.v3.ExternalProcessor`): Verifies the main ext_proc service is healthy. Returns `SERVING` if the EPP is ready to process ext_proc requests (same criteria as readiness check).
### Health Check Protocol
The EPP implements the standard gRPC Health Checking Protocol:

```
rpc Check(HealthCheckRequest) returns (HealthCheckResponse);

message HealthCheckRequest {
  string service = 1;
}

message HealthCheckResponse {
  enum ServingStatus {
    UNKNOWN = 0;
    SERVING = 1;
    NOT_SERVING = 2;
    SERVICE_UNKNOWN = 3;
  }
  ServingStatus status = 1;
}
```

### Why envoy.lb namespace as a default?
The `envoy.lb` namespace is a predefined namespace. One common way to use the selected endpoint returned from the server, is [envoy subsets](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/load_balancing/subsets) where host metadata for subset load balancing must be placed under `envoy.lb`. Note that this is not related to the subsetting feature discussed above, this is an enovy implementation detail.
You can’t perform that action at this time. 
