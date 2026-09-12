---
title: Predicted Latency-Based Scheduling | llm-d
id: predicted-latency-based-scheduling-llm-d
tags:
- llmd-router-nonk8s-backends-54b774
- predicted-latency
- epp-plugin-pattern
- latency-predictor
created: '2026-09-11T20:54:59.295980Z'
updated: '2026-09-12T03:37:37.728923Z'
source: https://llm-d.ai/docs/well-lit-paths/foundations/predicted-latency
source_domain: llm-d.ai
fetched_at: '2026-09-11T20:54:59.273300Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'Official llm-d docs (v0.9) overview of predicted-latency-based scheduling:
  an alternative/complement to the optimized-baseline heuristic scorer, using an online-trained
  XGBoost model to predict per-pod TTFT and TPOT and route to the pod predicted fastest,
  optionally enforcing per-request SLOs via x-llm-d-slo-ttft-ms / x-llm-d-slo-tpot-ms
  headers. Explicitly answers the gap question about k8s coupling: "The setup deploys
  an EPP with the predicted latency sidecar containers" -- a Training Server (trains
  XGBoost on observed traffic) and Prediction Servers (serve TPOT/TTFT predictions
  from current server state) -- both colocated with the EPP, not with the model server.
  Request flow: proxy forwards to EPP, EPP queries the prediction server, EPPs latency-scorer
  picks the endpoint, proxy forwards to the vLLM endpoint, and results flow back to
  the training server to update the model online. Explicit limitation: only p90-percentile
  prediction and only streaming-mode training are currently supported; the predictor
  assumes a homogeneous pool (same GPU/model/serving-config) and produces inaccurate
  predictions on a heterogeneous pool. Links to github.com/llm-d/llm-d-latency-predictor
  for the training/prediction server source (Python) and to architecture/advanced/latency-predictor
  for the full plugin pipeline and metric reference.'
utility_score: 12.0
---

*Suggested by [[llm-dllm-d-router-deepwiki]] — gap-fill search for predicted-latency-based scheduling guide*

[Skip to main content](https://llm-d.ai/docs/well-lit-paths/foundations/predicted-latency#__docusaurus_skipToContent_fallback)
🎉 **llm-d 0.9 is here!** Hardened for scale with router HA, enhanced autoscaling, end-to-end observability and expanded hardware support. [**Read the release blog →**](https://llm-d.ai/blog/llm-d-v0.9-hardened-for-scale)
Version: v0.9 (latest)
On this page
llm-d's [optimized baseline guide](https://llm-d.ai/docs/well-lit-paths/foundations/optimized-baseline) leverages load signals and prefix-cache affinity to schedule requests, combining the signals together with heuristics.
This path is for operators who want to adopt predicted latency-based scheduling - which uses an XGBoost model trained online - to make scheduling decisions. This strategy is useful when:
  * Your workload has **high variance in prompt and completion length** , and queue depth alone is a poor proxy for true load.
  * Your clients can express **per-request latency SLOs** (interactive vs. batch) and you want the gateway to enforce them.
  * Static weight tuning between cache affinity and load has become **fragile** as traffic shifts.


note
Predicted latency is not a fit when the pool is **heterogeneous** — mixed GPU types, model variants (e.g. prefill vs decode), or serving configurations in the same pool will produce inaccurate predictions, because the predictor assumes a single pod shape.
## Deploy[​](https://llm-d.ai/docs/well-lit-paths/foundations/predicted-latency#deploy "Direct link to Deploy")
See the [Predicted Latency guide](https://github.com/llm-d/llm-d/tree/main/guides/predicted-latency-routing) for manifests and step-by-step deployment.
## Architecture[​](https://llm-d.ai/docs/well-lit-paths/foundations/predicted-latency#architecture "Direct link to Architecture")
The setup deploys an EPP with the predicted latency sidecar containers:
  * **Training Server** - trains the XGBoost model to predict TPOT and TTFT based on observed traffic
  * **Prediction Servers** - predict TPOT and TTFT of the request based on current server state


During the standard request flow:
  * Request arrives at the proxy, which forwards the request to the EPP
  * EPP queries the prediction server
  * EPP (using `latency-scorer`) selects optimal endpoint based on the prediction
  * Proxy forwards request to the vLLM endpoint
  * vLLM endpoint processes the request, returns response to proxy
  * Proxy sends results to the training server, which uses samples to update the model


## Further Reading[​](https://llm-d.ai/docs/well-lit-paths/foundations/predicted-latency#further-reading "Direct link to Further Reading")
  * [Latency Predictor Architecture](https://llm-d.ai/docs/architecture/advanced/latency-predictor) — plugin pipeline, ML model, scaling characteristics, metric reference.
  * [llm-d/llm-d-latency-predictor](https://github.com/llm-d/llm-d-latency-predictor) — source for the training and prediction server Python code.
  * [Predicted Latency-Based Scheduling for LLMs - Blog](https://llm-d.ai/blog/predicted-latency-based-scheduling-for-llms) — design rationale and benchmark results.




