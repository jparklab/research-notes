---
title: P/D Disaggregation | llm-d
id: pd-disaggregation-llm-d-2
tags:
- llm-d
- pd-disaggregation
- nixl
- llmd-kv-cache-routing-c665d5
created: '2026-09-14T11:52:48.851715Z'
updated: '2026-09-14T11:58:51.352789Z'
source: https://llm-d.ai/docs/dev/well-lit-paths/foundations/pd-disaggregation
source_domain: llm-d.ai
fetched_at: '2026-09-14T11:52:48.831927Z'
fetch_provider: crawl4ai
status: deprecated
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'llm-d dev-branch overview of prefill/decode (P/D) disaggregation. Prefill
  is compute-bound (GPU flops); decode is memory-bandwidth-bound (KV-cache read from
  HBM). Recommended for long-context (10:1 ISL:OSL) medium/large-model workloads.
  Describes a reference topology: 4 prefill replicas (TP=1) and 1 decode replica (TP=5)
  in one InferencePool, labeled llm-d.ai/role=prefill|decode. Request flow: proxy
  -> EPP (schedules P/D using role labels) -> sidecar on the decode pod -> sidecar
  forwards to prefill instance -> prefill returns KV-block metadata -> decode instance
  pulls KV blocks over RDMA (IB/RoCE/EFA) via NIXL -> decode proceeds. Notes NIXL
  supports TCP but high-bandwidth RDMA networking is ''highly recommended'' for production.
  Observability guidance: because P/D runs two independently scaled pools joined by
  a NIXL transfer, operators should watch pool balance and transfer health rather
  than one aggregate latency metric.'
utility_score: 15.0
---

[Skip to main content](https://llm-d.ai/docs/dev/well-lit-paths/foundations/pd-disaggregation#__docusaurus_skipToContent_fallback)
🎉 **llm-d 0.9 is here!** Hardened for scale with router HA, enhanced autoscaling, end-to-end observability and expanded hardware support. [**Read the release blog →**](https://llm-d.ai/blog/llm-d-v0.9-hardened-for-scale)
This is unreleased documentation for llm-d **dev** version.
For up-to-date documentation, see the (v0.9 (latest)).
Version: dev
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
## Deploy[​](https://llm-d.ai/docs/dev/well-lit-paths/foundations/pd-disaggregation#deploy "Direct link to Deploy")
See the [P/D Disaggregation guide](https://github.com/llm-d/llm-d/tree/main/guides/pd-disaggregation) for manifests and step-by-step deployment.
## Architecture[​](https://llm-d.ai/docs/dev/well-lit-paths/foundations/pd-disaggregation#architecture "Direct link to Architecture")
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


## Observability[​](https://llm-d.ai/docs/dev/well-lit-paths/foundations/pd-disaggregation#observability "Direct link to Observability")
P/D disaggregation runs prefill and decode as independently scaled pools joined by a NIXL KV transfer, so the signals that matter are pool **balance** and transfer health rather than a single aggregate latency. The [P/D guide's Observability & Troubleshooting section](https://github.com/llm-d/llm-d/tree/main/guides/pd-disaggregation#4-observability--troubleshooting) covers the key metrics for this path and the common failure modes, backed by the shared [PromQL](https://llm-d.ai/docs/dev/operations/observability/promql#prefilldecode-disaggregation) and [metric](https://llm-d.ai/docs/dev/operations/observability/metrics) references.
## Further Reading[​](https://llm-d.ai/docs/dev/well-lit-paths/foundations/pd-disaggregation#further-reading "Direct link to Further Reading")
See [PD Architecture](https://llm-d.ai/docs/dev/architecture/advanced/disaggregation) for more details.


