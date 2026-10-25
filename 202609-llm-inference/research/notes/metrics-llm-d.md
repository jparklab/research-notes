---
title: Metrics | llm-d
id: metrics-llm-d
tags:
- llmd-router-nonk8s-backends-54b774
- metrics
- observability
- epp-architecture
- prometheus-security
- security
created: '2026-09-11T20:53:24.729820Z'
updated: '2026-09-15T04:55:23.885023Z'
source: https://llm-d.ai/docs/operations/observability/metrics
source_domain: llm-d.ai
fetched_at: '2026-09-11T20:53:24.709755Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'Official llm-d docs (v0.9, current) walkthrough of the metrics pipeline:
  model-server metrics (vLLM: num_requests_running, num_requests_waiting, kv_cache_usage_perc,
  time_to_first_token_seconds, inter_token_latency_seconds, prefix_cache_hits_total/queries_total,
  prompt/generation_tokens_total; SGLang: analogous sglang_* names) are scraped via
  Prometheus PodMonitors created by a Kustomize monitoring component (Kubernetes Prometheus-Operator
  CRD, decode-podmonitor / prefill-podmonitor). EPP (router) metrics are exposed via
  a ServiceMonitor (epp-servicemonitor) and enumerated in full: llm_d_epp_request_total/error_total/duration_seconds/size_bytes/input_tokens/output_tokens/cached_tokens/running/ntpot_seconds/ttft_seconds/streaming_tpot_seconds/streaming_itl_seconds,
  llm_d_epp_ready_endpoints, llm_d_epp_scheduler_attempts_total/e2e_duration_seconds,
  llm_d_epp_plugin_duration_seconds, llm_d_epp_disagg_decision_total, plus flow-control-specific
  gauges/histograms (queue_size, queue_bytes, request_queue_duration_seconds, dispatch_cycle_duration_seconds,
  request_enqueue_duration_seconds, pool_saturation, stale_endpoints, requests_total-by-outcome).
  Security-relevant for non-k8s backends: the whole scrape path is built on Kubernetes-native
  PodMonitor/ServiceMonitor CRDs, so a non-k8s backend loses this auto-discovery/scrape-config
  mechanism entirely and must supply its own static Prometheus scrape_configs; troubleshooting
  guidance shows the model-server /metrics endpoint is verified with a plain HTTP
  curl against port 8000 by default, and TLS between Prometheus and its targets is
  an explicit opt-in the operator must enable (an --enable-tls flag on the bundled
  install script) rather than a default -- i.e., EPP-to-model-server metrics scraping
  is unencrypted unless the operator deliberately adds TLS.'
utility_score: 13.0
---

[Skip to main content](https://llm-d.ai/docs/operations/observability/metrics#__docusaurus_skipToContent_fallback)
🎉 **llm-d 0.9 is here!** Hardened for scale with router HA, enhanced autoscaling, end-to-end observability and expanded hardware support. [**Read the release blog →**](https://llm-d.ai/blog/llm-d-v0.9-hardened-for-scale)
Version: v0.9 (latest)
On this page
This page covers how to enable and interpret metrics from an llm-d deployment. For Prometheus and Grafana installation, see [Observability Setup](https://llm-d.ai/docs/operations/observability/setup) first.
note
Commands in this page use `${NAMESPACE}` for the namespace where your llm-d workload runs. Set it before following along:

```


exportNAMESPACE=your-llm-d-namespace


```

## Prerequisites[​](https://llm-d.ai/docs/operations/observability/metrics#prerequisites "Direct link to Prerequisites")
  * A running llm-d deployment with an InferencePool and model servers — see the [quickstart](https://llm-d.ai/docs/getting-started/quickstart) if needed
  * Prometheus and Grafana installed — see [Observability Setup](https://llm-d.ai/docs/operations/observability/setup)


## Step 1: Enable Model Server Metrics[​](https://llm-d.ai/docs/operations/observability/metrics#step-1-enable-model-server-metrics "Direct link to Step 1: Enable Model Server Metrics")
Model server metrics are enabled by default. Configuration varies by deployment method.
### Kustomize Deployments[​](https://llm-d.ai/docs/operations/observability/metrics#kustomize-deployments "Direct link to Kustomize Deployments")
If you deployed your model server using `kustomize build`, add the monitoring component to your `kustomization.yaml`:

```


components:




- ../../../recipes/modelserver/components/monitoring       # decode PodMonitor




# - ../../../recipes/modelserver/components/monitoring-pd  # add for prefill/decode disaggregation


```

The monitoring component creates PodMonitors that scrape model server metrics. See [`guides/recipes/modelserver/components/monitoring/`](https://github.com/llm-d/llm-d/tree/main/guides/recipes/modelserver/components/monitoring/) for details.
### Verify PodMonitors[​](https://llm-d.ai/docs/operations/observability/metrics#verify-podmonitors "Direct link to Verify PodMonitors")
Verify the PodMonitors exist:

```


kubectl get podmonitors -n${NAMESPACE}


```

Expected output:

```


NAME                    AGE




decode-podmonitor       5m




prefill-podmonitor      5m


```

### Key vLLM Metrics[​](https://llm-d.ai/docs/operations/observability/metrics#key-vllm-metrics "Direct link to Key vLLM Metrics")  
| Metric  | What it measures  | Why it matters  |  
| --- | --- | --- |  
| `vllm:num_requests_running`  | Active requests being processed  | High values indicate GPU saturation; new requests will queue. Watch for sustained spikes  |  
| `vllm:num_requests_waiting`  | Requests queued, waiting to be processed  | Non-zero means pods are saturated. Primary signal for autoscaling decisions  |  
| `vllm:kv_cache_usage_perc`  | KV cache utilization (0.0 to 1.0)  | Above 0.9 means GPU memory is nearly full and requests may get preempted or rejected  |  
|  `vllm:time_to_first_token_seconds` (histogram)  | Time from request arrival to first generated token (TTFT)  | Directly impacts user experience. Use `histogram_quantile()` to query percentiles  |  
|  `vllm:inter_token_latency_seconds` (histogram)  | Time between consecutive generated tokens (ITL)  | Affects streaming response speed. High ITL causes choppy output. Use `histogram_quantile()` to query percentiles  |  
| `vllm:prefix_cache_hits_total`  | Number of prefix cache hits  | Compare with `prefix_cache_queries_total` to get hit rate. Low hit rate suggests the EPP is not routing effectively  |  
| `vllm:prefix_cache_queries_total`  | Total prefix cache lookups  | Divide `prefix_cache_hits_total` by this to get hit rate. A dropping ratio indicates routing or prompt pattern changes  |  
| `vllm:prompt_tokens_total`  | Total input tokens processed  | Use `rate()` to get tokens/sec per pod. Compare across pods to spot uneven load distribution  |  
| `vllm:generation_tokens_total`  | Total output tokens generated  | Use `rate()` alongside prompt tokens to get total throughput. A drop signals degraded model performance  |  
### Key SGLang Metrics[​](https://llm-d.ai/docs/operations/observability/metrics#key-sglang-metrics "Direct link to Key SGLang Metrics")  
| Metric  | What it measures  | Why it matters  |  
| --- | --- | --- |  
| `sglang_num_running_reqs`  | Active requests being processed  | High values indicate GPU saturation; new requests will queue  |  
| `sglang_num_queue_reqs`  | Requests queued, waiting to be processed  | Non-zero means pods are saturated. Primary signal for autoscaling decisions  |  
| `sglang_token_usage`  | KV cache token utilization (0.0 to 1.0)  | Above 0.9 means GPU memory is nearly full  |  
| `sglang_cache_hit_rate`  | Prefix cache hit rate (0.0 to 1.0)  | High hit rate indicates efficient KV cache reuse  |  
|  `sglang_time_to_first_token_seconds` (histogram)  | Time from request arrival to first generated token (TTFT)  | Directly impacts user experience. Use `histogram_quantile()` to query percentiles  |  
|  `sglang_inter_token_latency_seconds` (histogram)  | Time between consecutive generated tokens (ITL)  | Affects streaming response speed. Use `histogram_quantile()` to query percentiles  |  
| `sglang_prompt_tokens_total`  | Total input tokens processed  | Use `rate()` to get tokens/sec per pod  |  
| `sglang_generation_tokens_total`  | Total output tokens generated  | Use `rate()` alongside prompt tokens to get total throughput  |  
## Step 3: Enable EPP Metrics[​](https://llm-d.ai/docs/operations/observability/metrics#step-3-enable-epp-metrics "Direct link to Step 3: Enable EPP Metrics")
EPP (Endpoint Picker) metrics are enabled by default. To verify or enable manually, see the [Monitoring & Tracing Configuration](https://github.com/llm-d/llm-d-router/tree/main/config/charts#4-monitoring--tracing-configuration) section in the llm-d-router Helm chart docs.
Verify the ServiceMonitor exists:

```


kubectl get servicemonitors -n${NAMESPACE}


```

Expected output:

```


NAME                    AGE




epp-servicemonitor      5m


```

### Key llm-d Router EPP Metrics[​](https://llm-d.ai/docs/operations/observability/metrics#key-llm-d-router-epp-metrics "Direct link to Key llm-d Router EPP Metrics")  
| Metric  | What it measures  | Why it matters  |  
| --- | --- | --- |  
| `llm_d_epp_request_total`  | Total request count per flow ID and priority  | Baseline for calculating error rate and throughput per model  |  
| `llm_d_epp_request_error_total`  | Error count per flow ID and priority  | Rising errors signal backend failures. Alert when error rate exceeds 5%  |  
| `llm_d_epp_request_duration_seconds`  | Response latency distribution per flow ID and priority  | The SLO metric. Tracks full round-trip time from request to response  |  
| `llm_d_epp_request_size_bytes`  | Incoming request size distribution in bytes per flow ID and priority  | Helps identify payload size anomalies and exceptionally large incoming prompts  |  
| `llm_d_epp_response_size_bytes`  | Outgoing response size distribution in bytes per flow ID and priority  | Tracks outgoing bandwidth usage and response payload size distribution  |  
| `llm_d_epp_request_input_tokens`  | Input token count distribution per flow ID and priority  | Helps identify expensive requests. Long prompts cost more compute  |  
| `llm_d_epp_request_output_tokens`  | Output token count distribution per flow ID and priority  | Combined with duration, gives normalized cost and generation volume per token  |  
| `llm_d_epp_request_cached_tokens`  | Cached prompt token distribution per flow ID and priority  | Measures prefix cache utilization reported by model servers  |  
| `llm_d_epp_request_running`  | Active request count per flow ID and priority  | Shows real-time load concurrency across models  |  
| `llm_d_epp_request_ntpot_seconds`  | Normalized time per output token (NTPOT) distribution per flow ID and priority  | Key efficiency metric (lower is better). Compare across pods to find stragglers  |  
| `llm_d_epp_request_ttft_seconds`  | Time to first token (TTFT) distribution per flow ID and priority  | Directly measures user-perceived responsiveness and time to initial output byte  |  
| `llm_d_epp_request_streaming_tpot_seconds`  | Time per output token (TPOT) distribution per flow ID and priority; applicable to streaming requests  | Tracks ongoing generation speed excluding initial prompt prefill latency  |  
| `llm_d_epp_request_streaming_itl_seconds`  | Inter-token latency (ITL) distribution per flow ID and priority; applicable to streaming requests  | Measures pacing between consecutive response body chunks; spikes indicate choppy output  |  
| `llm_d_epp_ready_endpoints`  | Number of ready endpoints in the pool  | If this drops below expected count, pods are crashing or not scheduling  |  
| `llm_d_epp_scheduler_attempts_total`  | Scheduling attempt counts and outcomes  | Track failed scheduling attempts. High failure rate indicates filter/scorer misconfiguration  |  
| `llm_d_epp_scheduler_e2e_duration_seconds`  | End-to-end scheduling latency distribution  | Tracks latency spent within the EPP scheduling cycle  |  
| `llm_d_epp_plugin_duration_seconds`  | Per-plugin processing latency distribution  | Identifies slow or bottlenecked scheduling plugins  |  
| `llm_d_epp_disagg_decision_total`  | Routing decisions across disaggregation stages  | Tracks breakdown of decode-only, prefill-decode, and encode decisions  |  
### Flow Control Metrics[​](https://llm-d.ai/docs/operations/observability/metrics#flow-control-metrics "Direct link to Flow Control Metrics")
When flow control is enabled, these additional metrics are exposed:  
| Metric  | What it measures  | Why it matters  |  
| --- | --- | --- |  
| `llm_d_epp_flow_control_queue_size`  | Queued request count per flow ID and priority  | Growing queue means the pool cannot keep up. Consider scaling or adjusting priority bands  |  
| `llm_d_epp_flow_control_queue_bytes`  | Queued payload size in bytes per flow ID and priority  | Large queued payloads can exhaust EPP memory. Monitor alongside `maxBytes` config  |  
| `llm_d_epp_flow_control_request_queue_duration_seconds`  | Queuing duration distribution per flow ID and priority  | Directly impacts user-perceived latency. High values mean flow control is holding requests too long  |  
| `llm_d_epp_flow_control_dispatch_cycle_duration_seconds`  | Internal dispatch cycle duration distribution  | Tracks execution speed of the flow control scheduler loop  |  
| `llm_d_epp_flow_control_request_enqueue_duration_seconds`  | Request enqueue duration distribution per flow ID and priority  | Measures admission overhead entering the flow control queue  |  
| `llm_d_epp_flow_control_pool_saturation`  | Pool saturation level (0.0 to 1.0+)  | Above 1.0 means demand exceeds capacity and flow control is actively throttling. Scale up or shed load  |  
| `llm_d_epp_flow_control_stale_endpoints`  | Number of endpoints with missing or stale telemetry  | Non-zero value indicates metrics collection issues rather than genuine overload  |  
| `llm_d_epp_flow_control_requests_total`  | Total requests processed by flow control by outcome  | Direct signal for rejection (`outcome="RejectedCapacity"`) and eviction rates  |  
## Step 4: View Dashboards[​](https://llm-d.ai/docs/operations/observability/metrics#step-4-view-dashboards "Direct link to Step 4: View Dashboards")
llm-d provides pre-built Grafana dashboards for common monitoring scenarios.
### Access Grafana[​](https://llm-d.ai/docs/operations/observability/metrics#access-grafana "Direct link to Access Grafana")
note
The commands below use namespace and service names from the bundled install script. If you use an existing Prometheus or Grafana instance, adjust the namespace and service names accordingly.

```


kubectl port-forward -n llm-d-monitoring svc/llmd-grafana 3000:80




# Open http://localhost:3000




# Default login: admin / admin


```

### Import Dashboards[​](https://llm-d.ai/docs/operations/observability/metrics#import-dashboards "Direct link to Import Dashboards")
Load all llm-d dashboards into Grafana:

```


./guides/recipes/observability/load-llm-d-dashboards.sh


```

Verify dashboards were imported:

```


kubectl get configmaps -n llm-d-monitoring -lgrafana_dashboard=1


```

Expected output:

```


NAME                                              DATA   AGE




llm-d-vllm-overview                               1      30s




llm-d-sglang-overview                             1      30s




llm-d-failure-saturation-dashboard                1      30s




llm-d-diagnostic-drilldown-dashboard              1      30s




llm-d-performance-kv-cache                        1      30s




llm-d-pd-coordinator-metrics                      1      30s


```

Or import individual dashboard JSON files manually from `guides/recipes/observability/grafana/dashboards/`:  
| Dashboard  | What it shows  |  
| --- | --- |  
| `llm-d-vllm-overview.json`  | General vLLM metrics overview  |  
| `llm-d-sglang-overview.json`  | General SGLang metrics overview  |  
| `llm-d-failure-saturation-dashboard.json`  | Failure and saturation indicators  |  
| `llm-d-diagnostic-drilldown-dashboard.json`  | Detailed diagnostic metrics for troubleshooting  |  
| `llm-d-performance-kv-cache.json`  | Performance metrics including KV cache utilization  |  
| `llm-d-pd-coordinator-metrics.json`  | Prefill/decode disaggregation metrics  |  
## Step 5: Query Metrics[​](https://llm-d.ai/docs/operations/observability/metrics#step-5-query-metrics "Direct link to Step 5: Query Metrics")
Access the Prometheus UI:

```


kubectl port-forward -n llm-d-monitoring svc/llmd-kube-prometheus-stack-prometheus 9090:9090




# Open http://localhost:9090 (or https://localhost:9090 if TLS is enabled)


```

## Cleanup[​](https://llm-d.ai/docs/operations/observability/metrics#cleanup "Direct link to Cleanup")

```


./guides/recipes/observability/install-prometheus-grafana.sh -u-n llm-d-monitoring


```

## Troubleshooting[​](https://llm-d.ai/docs/operations/observability/metrics#troubleshooting "Direct link to Troubleshooting")
### Autoscaler reports "http: server gave HTTP response to HTTPS client"[​](https://llm-d.ai/docs/operations/observability/metrics#autoscaler-reports-http-server-gave-http-response-to-https-client "Direct link to Autoscaler reports "http: server gave HTTP response to HTTPS client"")
The autoscaler is configured for HTTPS but Prometheus is serving HTTP. Enable TLS:

```


./guides/recipes/observability/install-prometheus-grafana.sh -u




./guides/recipes/observability/install-prometheus-grafana.sh --enable-tls


```

### Metrics not appearing in Prometheus[​](https://llm-d.ai/docs/operations/observability/metrics#metrics-not-appearing-in-prometheus "Direct link to Metrics not appearing in Prometheus")
  1. Check that PodMonitors and ServiceMonitors exist:

```


kubectl get podmonitors,servicemonitors -n${NAMESPACE}


```

  2. Verify Prometheus is scraping the targets. Open `http://localhost:9090/targets` (after port-forwarding) and check that vLLM and EPP targets show `UP`
  3. Confirm pods expose metrics:

```


VLLM_POD=$(kubectl get pods -n ${NAMESPACE}-lapp=my-model -ojsonpath='{.items[0].metadata.name}')




kubectl port-forward -n${NAMESPACE}${VLLM_POD}8000:8000




curl http://localhost:8000/metrics |head-20


```



### Grafana dashboards show "No data"[​](https://llm-d.ai/docs/operations/observability/metrics#grafana-dashboards-show-no-data "Direct link to Grafana dashboards show "No data"")
  1. Verify the Grafana datasource points to the correct Prometheus URL
  2. Check that metrics are flowing in Prometheus first (use the Prometheus UI)
  3. If using TLS, ensure the Grafana datasource is configured for HTTPS with the correct CA certificate


  * [Step 1: Enable Model Server Metrics](https://llm-d.ai/docs/operations/observability/metrics#step-1-enable-model-server-metrics)
    * [Kustomize Deployments](https://llm-d.ai/docs/operations/observability/metrics#kustomize-deployments)
  * [Step 3: Enable EPP Metrics](https://llm-d.ai/docs/operations/observability/metrics#step-3-enable-epp-metrics)
    * [Key llm-d Router EPP Metrics](https://llm-d.ai/docs/operations/observability/metrics#key-llm-d-router-epp-metrics)
    * [Flow Control Metrics](https://llm-d.ai/docs/operations/observability/metrics#flow-control-metrics)
  * [Step 4: View Dashboards](https://llm-d.ai/docs/operations/observability/metrics#step-4-view-dashboards)
  * [Step 5: Query Metrics](https://llm-d.ai/docs/operations/observability/metrics#step-5-query-metrics)
  * [Troubleshooting](https://llm-d.ai/docs/operations/observability/metrics#troubleshooting)
    * [Autoscaler reports "http: server gave HTTP response to HTTPS client"](https://llm-d.ai/docs/operations/observability/metrics#autoscaler-reports-http-server-gave-http-response-to-https-client)
    * [Metrics not appearing in Prometheus](https://llm-d.ai/docs/operations/observability/metrics#metrics-not-appearing-in-prometheus)
    * [Grafana dashboards show "No data"](https://llm-d.ai/docs/operations/observability/metrics#grafana-dashboards-show-no-data)


