---
title: llm-d
id: llm-d
tags:
- internal-llm-inference-stacks
- llm-d
- vllm
- cncf
- prefill-decode-disaggregation
- oss-project-page
created: '2026-10-09T23:00:36.407372Z'
updated: '2026-10-10T14:52:17.155186Z'
source: https://llm-d.ai/
source_domain: llm-d.ai
fetched_at: '2026-10-09T23:00:36.386642Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Official landing page for llm-d (llm-d.ai), an open-source, Kubernetes-native
  stack for distributed LLM inference, CNCF Sandbox project (accepted Mar 12 2026,
  donated by IBM Research, Red Hat, and Google Cloud at KubeCon Europe 2026). Runs
  vLLM and SGLang across a cluster, converting single-node engines into production-grade
  distributed serving; built on top of vLLM and the Kubernetes Gateway API Inference
  Extension. Current version at fetch time: llm-d 0.10 (''operational hardening and
  production readiness: safer rollouts, signed release images, vLLM 0.30, and a published
  test matrix for every well-lit path''). Ships named ''Well-Lit Paths'' (tested deployment
  recipes): optimized baseline, route-requests-by-predicted-latency, precise prefix-cache-aware
  routing, tiered prefix cache, prefill/decode disaggregation, wide expert parallelism
  for MoE models, flow control and fairness, and inference-pool autoscaling. Claims
  on the landing page (unverified, vendor self-reported, no methodology given): 3x
  higher output throughput, 2x faster TTFT, up to 70% higher tokens/sec, 30% throughput
  improvement, and a 40% reduction in TTFT and ITL on Google Vertex. Backed by NVIDIA,
  CoreWeave, AMD, Cisco, Hugging Face, Intel, Lambda, and Mistral AI. This is the
  OSS project Hetzner names as adopted for inference routing improvements and that
  Intility names as a larger-scale alternative to its EndPointPicker-based routing.'
---

*Suggested by [[what-we-learned-from-our-inference-experiment]] — llm-d is the OSS routing project adopted by Hetzner and referenced by Intility*

[Skip to main content](https://llm-d.ai/#__docusaurus_skipToContent_fallback)
🎉 **llm-d 0.10 is here!** Operational hardening and production readiness: safer rollouts, signed release images, vLLM 0.30, and a published test matrix for every well-lit path. [**Read the docs →**](https://llm-d.ai/docs)
Start building with a well-lit path.
llm-d provides Well-Lit Paths—tested deployment recipes for common production patterns. They’re adaptable starting points across models, hardware, and workloads.
[ Deploy an optimized baseline Get started ](https://llm-d.ai/docs/well-lit-paths/foundations/optimized-baseline)[ Route Requests by Predicted Latency Get started ](https://llm-d.ai/docs/well-lit-paths/foundations/predicted-latency)[ Enable Precise Prefix-Cache Aware Routing Get started ](https://llm-d.ai/docs/well-lit-paths/foundations/precise-prefix-cache-routing)[ Configure a Tiered Prefix Cache Get started ](https://llm-d.ai/docs/well-lit-paths/foundations/tiered-prefix-cache)
[ Deploy Prefill/Decode Disaggregation Get started ](https://llm-d.ai/docs/well-lit-paths/foundations/pd-disaggregation)[ Scale MoE Models with Wide Expert Parallelism Get started ](https://llm-d.ai/docs/well-lit-paths/foundations/wide-expert-parallelism)[ Apply Flow Control and Fairness Get started ](https://llm-d.ai/docs/well-lit-paths/foundations/flow-control)[ Autoscale your Inference Pool Get started ](https://llm-d.ai/docs/well-lit-paths/foundations/workload-autoscaling)
The fastest path to state-of-the-artLLM inference on any accelerator.
llm-d is an open-source, Kubernetes-native stack for distributed LLM inference. It runs vLLM, SGLang, and more across your cluster, turning single-node engines into production-grade serving on the hardware you already have.
3x higher output throughput
2x faster TTFT
Up to 70% higher tokens/sec
30% throughput improvement
40% reduction in TTFT and ITL in Google Vertex
Portability across heterogeneous hardware
llm-d runs production LLM inference across GPUs, TPUs, XPUs, CPUs, and emerging NPUs with consistent performance patterns, so teams can deploy once and scale anywhere with predictable cost and behavior.
Google TPU
Intel XPU
Release updates
v0.8.1
Join the community
From first-time users the teams debugging complex deployments, the community is open to everyone.
[Contribute](https://llm-d.ai/community/contribute)
Get started
