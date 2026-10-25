---
title: P/D Disaggregation | llm-d
id: pd-disaggregation-llm-d
tags:
- llm-d
- pd-disaggregation
- well-lit-path
- epp
- nixl
- llmd-router-nonk8s-backends-54b774
- llmd-kv-cache-routing-c665d5
created: '2026-09-11T19:45:25.830310Z'
updated: '2026-09-15T04:55:23.760430Z'
source: https://llm-d.ai/docs/well-lit-paths/foundations/pd-disaggregation
source_domain: llm-d.ai
fetched_at: '2026-09-11T19:45:25.810384Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'llm-d v0.9 ''well-lit path'' quickstart for P/D disaggregation (short, links
  to the fuller architecture doc). Gives a concrete reference topology: 2 vLLM Deployments
  in the same InferencePool -- prefill Deployment = 4 replicas of TP=1 vLLM labeled
  llm-d.ai/role=prefill, decode Deployment = 1 replica of TP=5 vLLM labeled llm-d.ai/role=decode,
  with routing proxy sidecars on all decode pods. Standard request flow: proxy forwards
  to EPP, EPP schedules using the role labels to pick decode+prefill variants, request
  routed to the sidecar which forwards to the prefill instance, prefill returns KV-block
  retrieval metadata, decode instance pulls KVs over RDMA (IB, RoCE, or EFA) via NIXL,
  then decode runs. States NIXL supports TCP transfer but high-bandwidth networking
  (IB/RoCE/EFA) is ''highly recommended'' for production. Notes observability guidance
  treats P/D as independently scaled pools joined by NIXL KV transfer, so pool balance
  and transfer health (not aggregate latency) are the key signals -- points to a PromQL
  reference and the llm-d/llm-d guides/pd-disaggregation repo folder for manifests,
  troubleshooting and metrics.'
utility_score: 13.0
---

[Skip to main content](https://llm-d.ai/docs/well-lit-paths/foundations/pd-disaggregation#__docusaurus_skipToContent_fallback)
🎉 **llm-d 0.9 is here!** Hardened for scale with router HA, enhanced autoscaling, end-to-end observability and expanded hardware support. [**Read the release blog →**](https://llm-d.ai/blog/llm-d-v0.9-hardened-for-scale)
Version: v0.9 (latest)
On this page
LLM inference has two computationally distinct phases:
  * **Prefill** processes the entire input prompt in a single forward pass - it is compute-bound, bottlenecked by the GPU flops available.
  * **Decode** generates output tokens one at a time from the KV-cache - it is memory-bandwidth-bound, bottlenecked by how fast data moves from HBM to on-chip memory.


For long context workloads (10:1 ISL:OSL) and medium-to-large models, separating prefill and decode into separate instances enables:
  * Improved throughput via specialization or prefill and decode
  * Improved quality of service, as long context prefills will not block decode work


llm-d's EPP natively supports the concept of disaggregation, enabling composition with other scorers (e.g. prefix-aware routing).
info
NIXL supports TCP transfer, but high-bandwidth networking (IB, RoCE, EFA) is **highly recommended** for production usage.
## Deploy[​](https://llm-d.ai/docs/well-lit-paths/foundations/pd-disaggregation#deploy "Direct link to Deploy")
See the [P/D Disaggregation guide](https://github.com/llm-d/llm-d/tree/main/guides/pd-disaggregation) for manifests and step-by-step deployment.
## Architecture[​](https://llm-d.ai/docs/well-lit-paths/foundations/pd-disaggregation#architecture "Direct link to Architecture")
The setup creates 2 `Deployments` of vLLM (all are part of the same `InferencePool`):
  * The **prefill** `Deployment` is 4 replicas of TP=1 vLLM - labeled with `llm-d.ai/role=prefill`.
  * The **decode** `Deployment` is 1 replica of TP=5 vLLM - labeled with `llm-d.ai/role=decode`. All these pods have a routing proxy sidecar.


During the standard request flow:
  * Request arrives at the proxy, which forwards the request to the EPP
  * EPP schedules the request with P/D disaggregation, using the labels to detect the decode and prefill variants
  * Request is routed to the sidecar, which forwards the request to the prefill instance
  * Prefill instance processes the prompt, returning metadata about how to retrieve the KV blocks
  * Decode instance pulls the KVs over RDMA (IB, RoCE, EFA) with NIXL
  * Decode instances processes the decodes


## Observability[​](https://llm-d.ai/docs/well-lit-paths/foundations/pd-disaggregation#observability "Direct link to Observability")
P/D disaggregation runs prefill and decode as independently scaled pools joined by a NIXL KV transfer, so the signals that matter are pool **balance** and transfer health rather than a single aggregate latency. The [P/D guide's Observability & Troubleshooting section](https://github.com/llm-d/llm-d/tree/main/guides/pd-disaggregation#4-observability--troubleshooting) covers the key metrics for this path and the common failure modes, backed by the shared [PromQL](https://llm-d.ai/docs/operations/observability/promql#prefilldecode-disaggregation) and [metric](https://llm-d.ai/docs/operations/observability/metrics) references.
## Further Reading[​](https://llm-d.ai/docs/well-lit-paths/foundations/pd-disaggregation#further-reading "Direct link to Further Reading")
See [PD Architecture](https://llm-d.ai/docs/architecture/advanced/disaggregation) for more details.


