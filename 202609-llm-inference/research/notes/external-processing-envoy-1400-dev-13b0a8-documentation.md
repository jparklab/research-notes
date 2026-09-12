---
title: External Processing — envoy 1.40.0-dev-13b0a8 documentation
id: external-processing-envoy-1400-dev-13b0a8-documentation
tags:
- llmd-router-nonk8s-backends
- envoy
- ext-proc
- fail-open
- gie
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:44:24.258667Z'
updated: '2026-09-12T03:37:37.569429Z'
source: https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/ext_proc_filter
source_domain: www.envoyproxy.io
fetched_at: '2026-09-11T19:44:24.237713Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'Envoy reference docs for the ext_proc HTTP filter, the wire protocol GIE''s
  EPP uses to intercept requests: a bidirectional gRPC stream where the external processor
  (EPP) receives ProcessingRequest and replies ProcessingResponse, able to mutate
  headers/body/trailers or return an immediate response. Documents session-affinity
  configuration (RING_HASH/MAGLEV hashing on a copied header or cookie so repeated
  calls land on the same EPP instance) and the filter''s exposed statistics, notably
  ''failure_mode_allowed'' (count of times an ext_proc error was ignored due to configuration)
  and ''message_timeouts'' — these are the Envoy-level counters that make ext_proc''s
  fail-open/fail-closed behavior observable, directly relevant to whether an EPP outage
  causes the gateway to pass traffic through unchecked (fail-open) or reject it (fail-closed).
  Access-log filter-state fields (request_header_call_status, latencies) let operators
  trace whether/why a given request''s endpoint-picker call failed.'
utility_score: 13.0
---

docs: Latest (development) 
Older releases: type a version. `Shift`+`V` opens this menu.
[Skip to main content](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/ext_proc_filter#envoy-doc-main)
`esc`
`↑↓`navigate `↵`open
  * [Configuration reference](https://www.envoyproxy.io/docs/envoy/latest/configuration/configuration)
  * External Processing


# External Processing 
  * This filter should be configured with the type URL `type.googleapis.com/envoy.extensions.filters.http.ext_proc.v3.ExternalProcessor`.
  * [v3 API reference](https://www.envoyproxy.io/docs/envoy/latest/api-v3/extensions/filters/http/ext_proc/v3/ext_proc.proto#envoy-v3-api-msg-extensions-filters-http-ext-proc-v3-externalprocessor)


The external processing filter connects an external service, called an “external processor,” to the filter chain. The processing service itself implements a gRPC interface that allows it to respond to events in the lifecycle of an HTTP request / response by examining and modifying the headers, body, and trailers of each message, or by returning a brand-new response.
The protocol itself is based on a bidirectional gRPC stream. Envoy will send the external processor [ProcessingRequest](https://www.envoyproxy.io/docs/envoy/latest/api-v3/service/ext_proc/v3/external_processor.proto#envoy-v3-api-msg-service-ext-proc-v3-processingrequest) messages, and the processor must reply with [ProcessingResponse](https://www.envoyproxy.io/docs/envoy/latest/api-v3/service/ext_proc/v3/external_processor.proto#envoy-v3-api-msg-service-ext-proc-v3-processingresponse) messages.
Configuration options are provided to control which events are sent to the processor. This way, the processor may receive headers, body, and trailers for both request and response in any combination. The processor may also change this configuration on a message-by-message basis. This allows for the construction of sophisticated processors that decide how to respond to each message individually to eliminate unnecessary stream requests from the proxy.
The updated list of supported features can be found on the [reference page](https://www.envoyproxy.io/docs/envoy/latest/api-v3/extensions/filters/http/ext_proc/v3/ext_proc.proto#envoy-v3-api-msg-extensions-filters-http-ext-proc-v3-externalprocessor).
## Session affinity 
When the filter uses the Envoy gRPC client, requests to the external processor can use consistent hashing to keep sessions on the same processor endpoint. Configure the gRPC [initial metadata](https://www.envoyproxy.io/docs/envoy/latest/api-v3/config/core/v3/grpc_service.proto#envoy-v3-api-field-config-core-v3-grpcservice-initial-metadata) to copy an affinity key from the downstream request, and configure the external processor cluster with a matching [cluster-level hash policy](https://www.envoyproxy.io/docs/envoy/latest/api-v3/extensions/upstreams/http/v3/http_protocol_options.proto#envoy-v3-api-field-extensions-upstreams-http-v3-httpprotocoloptions-hash-policy).
For example, the filter can copy a session header into the gRPC request:
ext-proc-session-affinity-header.yamlyaml

```
-name:envoy.filters.http.ext_proc
typed_config:
"@type":type.googleapis.com/envoy.extensions.filters.http.ext_proc.v3.ExternalProcessor
grpc_service:
envoy_grpc:
cluster_name:ext_proc_cluster
initial_metadata:
-key:x-session-id
value:"%REQ(x-session-id)%"

```
Copy to clipboard
The external processor cluster can then hash the copied header:
yaml

```
-name:ext_proc_cluster
lb_policy:RING_HASH
typed_extension_protocol_options:
envoy.extensions.upstreams.http.v3.HttpProtocolOptions:
"@type":type.googleapis.com/envoy.extensions.upstreams.http.v3.HttpProtocolOptions
explicit_http_config:
http2_protocol_options:{}
hash_policy:
-header:
header_name:x-session-id
load_assignment:
cluster_name:ext_proc_cluster
endpoints:
-lb_endpoints:
-endpoint:
address:
socket_address:
address:127.0.0.1
port_value:50051
-endpoint:
address:
socket_address:
address:127.0.0.1
port_value:50052

```
Copy to clipboard
Passive cookie affinity can be configured in the same way by copying the downstream `cookie` header into the gRPC initial metadata and selecting the cookie by name in the cluster hash policy:
ext-proc-session-affinity-cookie.yamlyaml

```
-name:envoy.filters.http.ext_proc
typed_config:
"@type":type.googleapis.com/envoy.extensions.filters.http.ext_proc.v3.ExternalProcessor
grpc_service:
envoy_grpc:
cluster_name:ext_proc_cluster
initial_metadata:
-key:cookie
value:"%REQ(cookie)%"

```
Copy to clipboard
yaml

```
-name:ext_proc_cluster
lb_policy:RING_HASH
typed_extension_protocol_options:
envoy.extensions.upstreams.http.v3.HttpProtocolOptions:
"@type":type.googleapis.com/envoy.extensions.upstreams.http.v3.HttpProtocolOptions
explicit_http_config:
http2_protocol_options:{}
hash_policy:
-cookie:
name:session_id
load_assignment:
cluster_name:ext_proc_cluster
endpoints:
-lb_endpoints:
-endpoint:
address:
socket_address:
address:127.0.0.1
port_value:50051
-endpoint:
address:
socket_address:
address:127.0.0.1
port_value:50052

```
Copy to clipboard
The cluster must use a hash-based load balancer such as `RING_HASH` or `MAGLEV`. Cookie affinity must be passive: generated cookies cannot be propagated from the external processor side stream to the original downstream client. This configuration does not apply to the native Google gRPC client. Cluster-level hash policies take precedence over request-level policies.
## Statistics 
This filter outputs statistics in the `http.<stat_prefix>.ext_proc.` namespace. The [stat prefix](https://www.envoyproxy.io/docs/envoy/latest/api-v3/extensions/filters/network/http_connection_manager/v3/http_connection_manager.proto#envoy-v3-api-field-extensions-filters-network-http-connection-manager-v3-httpconnectionmanager-stat-prefix) comes from the owning HTTP connection manager.
The following statistics are supported:  
| Name  | Type  | Description  |  
| --- | --- | --- |  
| streams_started  | Counter  | The number of gRPC streams that have been started to send to the external processing service  |  
| stream_msgs_sent  | Counter  | The number of messages sent on those streams  |  
| stream_msgs_received  | Counter  | The number of messages received on those streams  |  
| spurious_msgs_received  | Counter  | The number of unexpected messages received that violated the protocol  |  
| streams_closed  | Counter  | The number of streams successfully closed on either end  |  
| streams_failed  | Counter  | The number of times a stream produced a gRPC error  |  
| failure_mode_allowed  | Counter  | The number of times an error was ignored due to configuration  |  
| message_timeouts  | Counter  | The number of times a message failed to receive a response within the configured timeout  |  
| rejected_header_mutations  | Counter  | The number of rejected header mutations  |  
| override_message_timeout_received  | Counter  | The number of override_message_timeout messages received  |  
| override_message_timeout_ignored  | Counter  | The number of override_message_timeout messages ignored  |  
| clear_route_cache_ignored  | Counter  | The number of clear cache request that were ignored  |  
| clear_route_cache_disabled  | Counter  | The number of clear cache requests that were rejected from being disabled  |  
| clear_route_cache_upstream_ignored  | Counter  | The number of clear cache request that were ignored if the filter is in upstream  |  
## Access Log Fields 
The external processing filter exposes processing statistics and metadata for use in access logs through the filter state object named `envoy.filters.http.ext_proc`. This information includes gRPC call latencies, status codes, and byte counts that can be used for monitoring and debugging external processor performance.
The filter state supports three serialization modes:
  * **PLAIN** : Comma-separated `key:value` pairs in abbreviated format.
  * **TYPED** : JSON object with descriptive field names.
  * **FIELD** : Individual field access by name.


Available field names:  
| Field Name  | Type  | Description  |  
| --- | --- | --- |  
| request_header_latency_us  | Integer  | Latency in microseconds for request header processing  |  
| request_header_call_status  | Integer  | gRPC status code for request header call  |  
| request_body_call_count  | Integer  | Number of request body chunks processed  |  
| request_body_total_latency_us  | Integer  | Total latency for all request body calls in microseconds  |  
| request_body_max_latency_us  | Integer  | Maximum latency among request body calls in microseconds  |  
| request_body_last_call_status  | Integer  | gRPC status of the last request body call  |  
| request_trailer_latency_us  | Integer  | Latency for request trailer processing in microseconds  |  
| request_trailer_call_status  | Integer  | gRPC status code for request trailer call  |  
| response_header_latency_us  | Integer  | Latency for response header processing in microseconds  |  
| response_header_call_status  | Integer  | gRPC status code for response header call  |  
| response_body_call_count  | Integer  | Number of response body chunks processed  |  
| response_body_total_latency_us  | Integer  | Total latency for all response body calls in microseconds  |  
| response_body_max_latency_us  | Integer  | Maximum latency among response body calls in microseconds  |  
| response_body_last_call_status  | Integer  | gRPC status of the last response body call  |  
| response_trailer_latency_us  | Integer  | Latency for response trailer processing in microseconds  |  
| response_trailer_call_status  | Integer  | gRPC status code for response trailer call  |  
| bytes_sent  | Integer  | Total bytes sent to external processor (Envoy gRPC only)  |  
| bytes_received  | Integer  | Total bytes received from external processor (Envoy gRPC only)  |  
| immediate_responses_sent  | Integer  | Total number of immediate responses sent  |  
| server_half_closed  | Integer  | Number of streams closed by the server  |  
Example usage in access log configuration:
yaml

```
access_log:
-name:envoy.access_loggers.stdout
typed_config:
"@type":type.googleapis.com/envoy.extensions.access_loggers.stream.v3.StdoutAccessLog
log_format:
json_format:
# Individual field access
ext_proc_header_latency:"%FILTER_STATE(envoy.filters.http.ext_proc:FIELD:request_header_latency_us)%"
ext_proc_body_calls:"%FILTER_STATE(envoy.filters.http.ext_proc:FIELD:request_body_call_count)%"
# Full structured data
ext_proc_all_stats:"%FILTER_STATE(envoy.filters.http.ext_proc:TYPED)%"
# Compact format
ext_proc_summary:"%FILTER_STATE(envoy.filters.http.ext_proc:PLAIN)%"

```
Copy to clipboard
Note
The `bytes_sent` and `bytes_received` fields are only populated when using Envoy gRPC client type. For Google gRPC client type, these fields will be 0.
Note
gRPC status codes follow the standard [gRPC status codes](https://grpc.github.io/grpc/core/md_doc_statuscodes.html): 0 = OK, 1 = CANCELLED, 2 = UNKNOWN, 3 = INVALID_ARGUMENT, 4 = DEADLINE_EXCEEDED, etc.
