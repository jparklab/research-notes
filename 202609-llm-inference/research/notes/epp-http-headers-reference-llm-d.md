---
title: EPP HTTP Headers Reference | llm-d
id: epp-http-headers-reference-llm-d
tags:
- llmd-router-nonk8s-backends-54b774
- locus-lost-k8s-policy-inputs
- epp-headers
- inference-objective
- flow-control
created: '2026-09-11T21:23:26.454652Z'
updated: '2026-09-12T03:37:37.759434Z'
source: https://llm-d.ai/docs/api-reference/epp-http-headers
source_domain: llm-d.ai
fetched_at: '2026-09-11T21:23:26.433192Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: llm-d Router v0.9 canonical EPP HTTP header reference. Priority/objective
  routing uses x-llm-d-inference-objective (names an InferenceObjective resource in
  the same namespace as the InferencePool, looked up by EPP for priority/performance
  goals); fairness/tenant grouping uses x-llm-d-inference-fairness-id (defaults to
  'default-flow' if omitted); model-name routing/override uses x-llm-d-model-name-rewrite
  (an explicit per-request alternative to the server-side rule-based InferenceModelRewrite
  API). SLO headers x-llm-d-slo-ttft-ms and x-llm-d-slo-tpot-ms drive admission control.
  Deprecated aliases (x-gateway-inference-objective, x-gateway-inference-fairness-id,
  x-gateway-model-name-rewrite, x-slo-ttft-ms, x-slo-tpot-ms) are still accepted for
  reads in v0.9.0+ with canonical value winning on conflict; no removal date announced.
  Explicitly states GAIE Endpoint Picker Protocol headers like x-gateway-destination-endpoint*
  are unchanged/untouched by this rename and belong to a separate spec. Implementation
  notes state header lookups are case-insensitive and 'these values are typically
  provided as standard HTTP headers in the incoming request' -- i.e., the doc frames
  header origin generically as request headers, not as something exclusively derived
  from a Kubernetes CRD reconciler, though the InferenceObjective/InferenceModelRewrite
  names they carry still resolve against namespaced K8s custom resources.
---

[Skip to main content](https://llm-d.ai/docs/api-reference/epp-http-headers#__docusaurus_skipToContent_fallback)
🎉 **llm-d 0.9 is here!** Hardened for scale with router HA, enhanced autoscaling, end-to-end observability and expanded hardware support. [**Read the release blog →**](https://llm-d.ai/blog/llm-d-v0.9-hardened-for-scale)
Version: v0.9 (latest)
On this page
This document describes the HTTP headers that the [Endpoint Picker (EPP)](https://llm-d.ai/docs/architecture/core/router/epp) inspects to manage and control inference requests, specifically for flow control, performance management, and request classification.
## Request Classification and Flow Control[​](https://llm-d.ai/docs/api-reference/epp-http-headers#request-classification-and-flow-control "Direct link to Request Classification and Flow Control")
These headers allow the EPP to identify the request's goals, group them for fair resource allocation, and handle model-specific targeting.  
| Header  | Description  |  
| --- | --- |  
| `x-llm-d-inference-objective`  | Specifies the name of the `InferenceObjective` resource associated with the request. The EPP uses this to look up the corresponding objective resource in the **same namespace as the InferencePool** to apply the defined priority and performance goals.  |  
| `x-llm-d-inference-fairness-id`  | Provides a unique identifier for grouping requests for fairness-based flow control. Requests with the same ID share capacity according to the fairness policy. If omitted, the EPP defaults to `default-flow`.  |  
| `x-llm-d-model-name-rewrite`  | Specifies the target model name to be used for the request. This is an alternative approach to model name rewriting; while the `InferenceModelRewrite` API provides rule-based rewriting on the server side, this header allows for an explicit, per-request override. When present, the EPP uses this value to override the model name in the request body and for recording model-specific metrics.  |  
## Service Level Objectives (SLOs)[​](https://llm-d.ai/docs/api-reference/epp-http-headers#service-level-objectives-slos "Direct link to Service Level Objectives \(SLOs\)")
These headers are used by admission control and load balancing plugins to make decisions based on latency targets.  
| Header  | Description  |  
| --- | --- |  
| `x-llm-d-slo-ttft-ms`  | Specifies the target Time To First Token (TTFT) in milliseconds. Used by plugins to determine if a request can be admitted while meeting the latency goal.  |  
| `x-llm-d-slo-tpot-ms`  | Specifies the target Time Per Output Token (TPOT) in milliseconds. Used for admission control based on predicted or observed token generation latency.  |  
## Deprecated Aliases[​](https://llm-d.ai/docs/api-reference/epp-http-headers#deprecated-aliases "Direct link to Deprecated Aliases")
llm-d Router v0.9.0 and later accept the previous EPP-managed header names as deprecated read aliases. New integrations and examples should use the canonical `x-llm-d-*` names. Earlier releases expect the previous names, so mixed-version fleets should coordinate the header migration during rolling upgrades. See the [llm-d Router releases](https://github.com/llm-d/llm-d-router/releases) page for available versions.
If a request sends both a canonical header and its deprecated alias, the canonical `x-llm-d-*` value wins deterministically. No alias removal release or date has been announced; treat aliases as temporary compatibility support and prefer the canonical names for all new clients.  
| Deprecated alias  | Canonical header  |  
| --- | --- |  
| `x-gateway-inference-fairness-id`  | `x-llm-d-inference-fairness-id`  |  
| `x-gateway-inference-objective`  | `x-llm-d-inference-objective`  |  
| `x-gateway-model-name-rewrite`  | `x-llm-d-model-name-rewrite`  |  
| `x-slo-ttft-ms`  | `x-llm-d-slo-ttft-ms`  |  
| `x-slo-tpot-ms`  | `x-llm-d-slo-tpot-ms`  |  
This rename applies only to llm-d/EPP-managed user and control headers. [Gateway API Inference Extension (GAIE) Endpoint Picker Protocol](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/004-endpoint-picker-protocol) headers, such as `x-gateway-destination-endpoint*`, are unchanged.
## Response Headers[​](https://llm-d.ai/docs/api-reference/epp-http-headers#response-headers "Direct link to Response Headers")
These headers are set by the EPP on responses sent back to the client.  
| Header  | Description  |  
| --- | --- |  
| `x-llm-d-request-dropped-reason`  | Indicates why a request was dropped by flow control. Only present on `429` responses generated by the EPP (not forwarded from a model server). The value uses a two-prefix scheme: `rejected-*` means the request never reached an inference server, `evicted-*` means it was dispatched and then killed. See the table below for possible values.  |  
### Dropped Reason Values[​](https://llm-d.ai/docs/api-reference/epp-http-headers#dropped-reason-values "Direct link to Dropped Reason Values")  
| Value  | Meaning  |  
| --- | --- |  
| `rejected-saturated`  | System at capacity, request rejected before queueing.  |  
| `rejected-ttl-expired`  | Request entered the queue but its TTL expired before dispatch.  |  
| `rejected-context-cancelled`  | Client disconnected while the request was queued.  |  
| `evicted`  | Generic post-dispatch eviction.  |  
| `evicted-queue-pressure`  | Evicted after dispatch due to backpressure from new arrivals.  |  
| `evicted-priority`  | Evicted after dispatch, preempted by a higher-priority request.  |  
The two-prefix convention helps consumers decide retry strategy: `rejected-*` means no inference work was done (cheap to retry), `evicted-*` means GPU cycles were consumed (factor into backoff).
## Implementation Notes[​](https://llm-d.ai/docs/api-reference/epp-http-headers#implementation-notes "Direct link to Implementation Notes")
  * **Case Sensitivity:** All header lookups are case-insensitive.
  * **Source:** These values are typically provided as standard HTTP headers in the incoming request.


  * [Request Classification and Flow Control](https://llm-d.ai/docs/api-reference/epp-http-headers#request-classification-and-flow-control)
  * [Service Level Objectives (SLOs)](https://llm-d.ai/docs/api-reference/epp-http-headers#service-level-objectives-slos)
  * [Response Headers](https://llm-d.ai/docs/api-reference/epp-http-headers#response-headers)
    * [Dropped Reason Values](https://llm-d.ai/docs/api-reference/epp-http-headers#dropped-reason-values)
  * [Implementation Notes](https://llm-d.ai/docs/api-reference/epp-http-headers#implementation-notes)


