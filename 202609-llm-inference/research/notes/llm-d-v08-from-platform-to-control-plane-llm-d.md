---
title: 'llm-d v0.8: From Platform to Control Plane | llm-d'
id: llm-d-v08-from-platform-to-control-plane-llm-d
tags:
- llmd-router-nonk8s
- file-discovery
- release-notes
- no-kubernetes
- llmd-router-nonk8s-backends-54b774
- llmd-kv-cache-routing-c665d5
created: '2026-09-11T19:42:44.424341Z'
updated: '2026-09-15T04:55:23.658049Z'
source: https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane
source_domain: llm-d.ai
fetched_at: '2026-09-11T19:42:44.402568Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'llm-d.ai v0.8 release blog (''From Platform to Control Plane'', targeted
  ~mid-2026, v0.9 milestone referenced as ''early August''). This is the release that
  FIRST shipped the FileDiscovery plugin under the heading ''Beyond Kubernetes: RL,
  Slurm, and File Discovery'' -- states it ''enables llm-d''s routing layer to discover
  inference endpoints through filesystem-based service discovery rather than the Kubernetes
  API,'' unlocking Slurm clusters and bare-metal research labs, motivated primarily
  by RL rollout workloads that run on Slurm rather than Kubernetes. Also documents
  component renames relevant to any fork/customization plan: llm-d-inference-scheduler
  -> llm-d-router-endpoint-picker, and llm-d-routing-sidecar -> llm-d-router-disagg-sidecar
  (both now explicitly named as components of the llm-d Router, not standalone systems).
  Confirms llm-d dropped custom vLLM image builds in favor of upstream vllm/vllm-openai:v0.23.0
  (control-plane-not-fork positioning) and made SGLang a first-class second backend
  engine alongside vLLM across most well-lit paths (optimized-baseline, precise-prefix-cache-routing,
  tiered-prefix-cache, P/D disaggregation), plus added TensorRT-LLM as a third engine
  via trtllm-serve. Notes GAIE (Gateway API Inference Extension) dependency pinned
  to v1.5.0 at this release, with GAIE Helm charts published from the llm-d-router
  OCI registry. Workload Variant Autoscaler (WVA) CRD migrated to the llm-d.ai API
  group (breaking change).'
utility_score: 16.0
---

[Skip to main content](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#__docusaurus_skipToContent_fallback)
🎉 **llm-d 0.9 is here!** Hardened for scale with router HA, enhanced autoscaling, end-to-end observability and expanded hardware support. [**Read the release blog →**](https://llm-d.ai/blog/llm-d-v0.9-hardened-for-scale)
If v0.7 was about making features deployable — standalone mode, Kustomize, documentation from scratch — then v0.8 is the payoff. Flow Control, Batch Gateway, and multi-modal serving graduate from experimental to production. The routing layer reaches beyond Kubernetes into RL training loops. And the image pipeline simplifies from a growing matrix of custom builds to upstream vLLM images with llm-d as the control plane on top. Three axes define this release: graduating capabilities that were introduced in v0.7 to production readiness, extending llm-d into non-Kubernetes environments for reinforcement learning and Slurm-based research, and aligning the project's identity around what it actually is — an inference control plane, not a fork of any engine underneath it.
The scope of what llm-d orchestrates has broadened considerably. A single deployment can now serve interactive chat, batch processing, multi-modal requests, and agentic multi-step workflows — with routing intelligence that understands the differences between them. SGLang joins vLLM as a first-class inference engine across most well-lit paths, and the first [agentic serving guide](https://github.com/llm-d/llm-d/tree/release-0.8/guides/agentic-serving) lands as a new workload-level well-lit path. Forty-eight new contributors joined the project since v0.6, more than doubling v0.7's twenty-three, reflecting broadening adoption across the industry.
## Delivering on Promises: Feature Graduation[​](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#delivering-on-promises-feature-graduation "Direct link to Delivering on Promises: Feature Graduation")
**Flow Control** moves from experimental to production. The centralized request queuing and admission control system introduced in v0.7 now ships with production guides for multi-tenant deployments. Flow Control classifies incoming requests by a FlowKey combining Fairness ID and Priority, maintains separate in-memory queues per flow, and dispatches based on priority bands, tenant fairness cycling, and request ordering. The result is no-regret scheduling: under saturation, the router holds requests rather than committing them to already-overloaded backend queues. The defaults still mimic first-come-first-served behavior, so existing deployments see no change until operators opt into multi-tenant policy.
**Batch Gateway** graduates to production with enterprise storage backend hardening. The OpenAI-compatible `/v1/batches` and `/v1/files` API handles up to 50,000 requests per batch job, with pluggable storage across PostgreSQL or Redis/Valkey for metadata, Redis/Valkey for queue and event streams, and S3 or filesystem for input/output files. Batch traffic coexists with interactive serving on shared infrastructure through integration with Flow Control backpressure, so operators no longer need to maintain separate clusters for offline workloads.
**Multi-modal serving** — image and text models like LLaVA and Qwen-VL — gains production guides with end-to-end testing, including disaggregated prefill/decode paths for multi-modal inputs. The inference simulator now supports multi-modal payloads, making it possible to validate routing behavior without provisioning GPU hardware.
## Upstream, Not Fork: Aligning with vLLM[​](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#upstream-not-fork-aligning-with-vllm "Direct link to Upstream, Not Fork: Aligning with vLLM")
One of the most consequential changes in v0.8 is also one of the simplest to describe: llm-d no longer builds its own vLLM images for most platforms.
The default GPU image is now `vllm/vllm-openai:v0.23.0` — the upstream release, unmodified. AMD ROCm moves to `vllm/vllm-openai-rocm`. Intel XPU moves to the prebuilt upstream vLLM XPU image. The custom `llm-d-cuda-gb200`, `llm-d-xpu`, and `llm-d-rocm` images are removed. The `llm-d-cuda` image remains available for advanced builds that require custom wheels (DeepGEMM, DeepEP), but it is no longer the default path.
This is not just a packaging simplification — it is a statement about what llm-d is. The project is a control plane that wraps inference engines, not a distribution that replaces them. When vLLM ships a release, llm-d users get it directly. When operators need to pin a specific vLLM commit for a hotfix, they build that image themselves and point llm-d at it. The control plane — the router, the endpoint picker, the KV cache management layer, the autoscaler — is where llm-d's value lives.
The component renames in this release reflect the same thinking. `llm-d-inference-scheduler` becomes `llm-d-router-endpoint-picker`. `llm-d-routing-sidecar` becomes `llm-d-router-disagg-sidecar`. The names now describe function rather than aspiration: these are components of the llm-d Router, not a standalone scheduling system.
HPU (Intel Gaudi) support is removed in this release. The upstream vLLM project does not currently maintain Gaudi images, and maintaining a custom build path for a single accelerator ran counter to the upstream-first direction. If upstream vLLM adds Gaudi support, llm-d will pick it up automatically.
## Beyond Kubernetes: RL, Slurm, and File Discovery[​](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#beyond-kubernetes-rl-slurm-and-file-discovery "Direct link to Beyond Kubernetes: RL, Slurm, and File Discovery")
Every previous llm-d release assumed Kubernetes. v0.8 breaks that assumption.
The **FileDiscovery plugin** enables llm-d's routing layer to discover inference endpoints through filesystem-based service discovery rather than the Kubernetes API. Operators write endpoint addresses to a file; the router watches it. This unlocks llm-d on Slurm clusters, bare-metal research labs, and any environment where Kubernetes is not present or not wanted.
This matters most for **reinforcement learning workflows**. RL training loops require high-throughput inference for rollout generation — the model being trained serves thousands of inference requests per training step. These workloads typically run on Slurm, not Kubernetes, and they need the inference scheduler to be embedded in the training loop rather than running as a separate service.
This is where the "From Platform to Control Plane" thesis lands concretely. llm-d's routing intelligence — prefix cache locality, saturation awareness, flow control — is useful anywhere inference happens, not just inside a Kubernetes cluster.
## Intelligent Routing for the Agentic Era[​](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#intelligent-routing-for-the-agentic-era "Direct link to Intelligent Routing for the Agentic Era")
Workloads are getting more complex. A single user session might involve a chain of inference calls — retrieval, reasoning, tool use, synthesis — each with different latency requirements, context dependencies, and failure modes. v0.8 adds routing intelligence designed for this reality.
**Agentic workload routing** enables the router to understand multi-step inference patterns. Combined with **Responses API support** , llm-d can now serve OpenAI-compatible agentic endpoints where the model orchestrates tool calls and multi-turn reasoning. The router tracks session affinity and prefix cache state across the steps of an agentic workflow, keeping related requests on the same backend where prior KV cache is resident.
**DP-Aware scheduling** graduates from its initial experimental form with **WideEP** support for DeepSeek-class MoE models. In expert-parallel deployments, different experts run on different GPU groups; the router needs to understand this topology to place requests on the correct data-parallel replica. The [WideEP guide](https://github.com/llm-d/llm-d/tree/release-0.8/guides/wide-ep-lws) has been migrated to the main example path with nightly CI coverage on GKE and CKS.
**Predicted latency scheduling** receives a complete rewrite with a new production-ready guide. The latency predictor provides model-specific latency estimates that the endpoint picker uses for routing decisions, moving beyond queue-depth heuristics to predictions grounded in actual model serving characteristics. Nightly CI now validates that the predictor is serving real predictions rather than falling back to heuristics — asserting that the optimization path operators configured is actually running.
## Engine-Agnostic: SGLang as a First-Class Backend[​](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#engine-agnostic-sglang-as-a-first-class-backend "Direct link to Engine-Agnostic: SGLang as a First-Class Backend")
v0.8 makes llm-d engine-agnostic. SGLang joins vLLM as a supported inference backend across the [optimized baseline](https://github.com/llm-d/llm-d/tree/release-0.8/guides/optimized-baseline), [precise prefix cache routing](https://github.com/llm-d/llm-d/tree/release-0.8/guides/precise-prefix-cache-routing), [tiered prefix cache](https://github.com/llm-d/llm-d/tree/release-0.8/guides/tiered-prefix-cache), and [P/D disaggregation](https://github.com/llm-d/llm-d/tree/release-0.8/guides/pd-disaggregation) well-lit paths. The only guide not yet covered is wide expert parallelism.
The prefix cache scorer, previously vLLM-only, now works with SGLang backends with optimal configuration and benchmark numbers. This makes llm-d's most impactful routing optimization — prefix-cache-aware request placement — available regardless of which engine runs underneath. Combined with the upstream vLLM image alignment described above, the message is clear: llm-d is a control plane for inference engines, not a wrapper around any single one.
## Multi-Tier KV Cache and Hybrid Model Support[​](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#multi-tier-kv-cache-and-hybrid-model-support "Direct link to Multi-Tier KV Cache and Hybrid Model Support")
KV cache management continues to be one of llm-d's deepest areas of investment. v0.8 extends the multi-tier architecture introduced in previous releases with two significant additions.
**HMA-aware KV offloading** adds support for hybrid model architectures that mix attention mechanisms — full attention, sliding-window, Mamba/SSM, and linear attention — in the same model. These architectures, exemplified by Jamba and emerging hybrid designs, allocate KV cache differently across layer types. The KV cache management layer now understands the Hybrid Memory Allocator (HMA) in vLLM, enabling offloading from GPU HBM to CPU DRAM to persistent storage while respecting the heterogeneous memory layout. For a deep dive into HMA-aware offloading, see the companion [blog post on serving hybrid models at scale](https://llm-d.ai/blog/serving-hybrid-models-at-scale-in-llm-d).
The **Mooncake connector** adds Alibaba's KV cache transfer engine as a NIXL backend for both P/D disaggregation transfers and [tiered offloading](https://github.com/llm-d/llm-d/tree/release-0.8/guides/tiered-prefix-cache). Combined with the existing UCX and UCCL backends, this gives operators three transfer engines to choose from based on their network fabric and deployment constraints. For comprehensive benchmarks comparing these backends across RoCE, InfiniBand, and TCP, see the [networking for distributed inference](https://llm-d.ai/blog/networking-for-distributed-inference-llm-d) blog post.
Storage events enable reactive tier management — the KV cache layer can respond to storage-level signals (capacity pressure, eviction notifications) rather than relying solely on proactive policies. This is a foundation for more sophisticated tiered caching strategies in future releases.
## New Well-Lit Paths and Ecosystem Reach[​](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#new-well-lit-paths-and-ecosystem-reach "Direct link to New Well-Lit Paths and Ecosystem Reach")
Each release expands the set of tested, documented deployment patterns — what we call well-lit paths. v0.8 adds several that reflect the project's broadening ecosystem.
**TensorRT-LLM** receives a [full recipe and optimized baseline guide](https://github.com/llm-d/llm-d/tree/release-0.8/guides/optimized-baseline/modelserver/gpu/trtllm). Alongside SGLang, this extends llm-d's engine coverage to three inference backends. The recipe uses `trtllm-serve` with llm-d's router handling prefix-cache-aware scheduling on top.
**DeepSeek-V4 on GB200 NVL72** gets a [dedicated deployment guide](https://github.com/llm-d/llm-d/tree/release-0.8/guides/wide-ep-lws/modelserver/gpu/vllm-deepseek-v4), running a frontier MoE model on NVIDIA's latest hardware with WideEP expert parallelism. The guide includes API server scaling (bumped from 1 to 4 replicas) and NVSHMEM configuration for the GB200's NVLink domain.
**Envoy AI Gateway** installation instructions [land as a new guide](https://github.com/llm-d/llm-d/tree/release-0.8/guides/recipes/gateway/envoy-ai-gateway), offering an alternative gateway layer alongside Istio and kgateway/agentgateway. This reflects the project's philosophy of providing pluggable infrastructure components rather than mandating a specific stack.
The **Workload Variant Autoscaler (WVA)** CRD migrates to the `llm-d.ai` API group, with improved observability and a replicas rebalancing guide. Infrastructure dependencies update to Istio 1.29.4, kgateway/agentgateway v2.3.3, and Gateway API Inference Extension v1.5.0, with the GAIE Helm charts now published from the llm-d-router OCI registry.
P/D disaggregation guides expand with [GKE RDMA/DRA/DRANET configurations](https://github.com/llm-d/llm-d/tree/release-0.8/guides/pd-disaggregation) for high-performance KV cache transfer, and preflight networking checks that validate RDMA bindings and NIC configuration before vLLM starts serving traffic.
## What This Means for You[​](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#what-this-means-for-you "Direct link to What This Means for You")
**Platform teams** running llm-d in production can now graduate Flow Control and Batch Gateway from experimental flags to production configuration. The shift to upstream vLLM images simplifies the image pipeline — no more tracking llm-d-specific builds for each accelerator platform. The WVA CRD migration to `llm-d.ai` is a breaking change that requires updating existing autoscaling configurations, but it aligns the resource model with the project's long-term API surface.
**ML researchers** working on reinforcement learning can now use llm-d's routing intelligence without Kubernetes, bringing prefix-cache-aware scheduling into Slurm-based training loops, where it directly reduces the wall-clock time of RL rollout generation. If your RL pipeline spends time on inference and you want cache-aware routing without re-architecting onto Kubernetes, this is built for you.
**Agentic application architects** building multi-step inference workflows get native routing support for session affinity, Responses API compatibility, and prefix cache awareness across the steps of an agent chain. Combined with SGLang support in the prefix cache scorer, the agentic routing path is no longer tied to a single inference engine.
## What Is Next?[​](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#what-is-next "Direct link to What Is Next?")
The [v0.9 milestone](https://github.com/llm-d/llm-d/issues/1945) targets early August and continues the trajectory this release established.
**Agentic inference patterns** are a primary focus. v0.8 introduced agentic routing and Responses API support; v0.9 aims to make the agentic story end-to-end — from request classification through multi-step orchestration to session-level observability. The goal is that operators deploying agentic workloads on llm-d get routing intelligence that understands the structure of agent chains, not just individual requests.
**RL workflow maturity** The initial integration demonstrated feasibility; v0.9 targets usable end-to-end RL workflows with deeper scheduler integration, more sophisticated cache management for rollout patterns, and broader framework support.
**Scaling and hardening** turns attention to the control plane itself. As llm-d deployments grow — more pods, more models, higher QPS — the router and endpoint picker need to scale horizontally without becoming bottlenecks. v0.9 addresses this directly with EPP horizontal scaling and smarter autoscaling policies.
**Continued accelerator and runtime expansion** includes broader SGLang CI coverage, AMD ecosystem growth, and ongoing upstream alignment. The direction is clear: llm-d supports every runtime and accelerator that upstream vLLM and SGLang support, with the control plane adapting to each platform's characteristics rather than requiring platform-specific builds.
## Community and Contribution[​](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#community-and-contribution "Direct link to Community and Contribution")
Forty-eight new contributors joined the project since v0.6, more than doubling the contributor base added in v0.7. The project's velocity continues to accelerate, with the v0.8 cycle delivering feature graduation, ecosystem expansion, and a clearer architectural identity simultaneously.
To get started with llm-d v0.8:
  * **Documentation** : [llm-d.ai](https://llm-d.ai)
  * **Guides** : [Well-lit paths](https://github.com/llm-d/llm-d/tree/main/docs/well-lit-paths) for optimized baseline, prefix-cache routing, P/D disaggregation, batch gateway, flow control, and more
  * **Release notes** : [v0.8.0 on GitHub](https://github.com/llm-d/llm-d/releases/tag/v0.8.0)
  * **Slack community** : Join the conversation at [llm-d.ai/slack](https://llm-d.ai/slack)


Follow [@llm_d](https://twitter.com/llm_d) on Twitter/X, [llm-d on Bluesky](https://bsky.app/profile/llm-d.bsky.social), or [llm-d on LinkedIn](https://www.linkedin.com/company/llm-d/) for updates. Check [llm-d.ai/community/events](https://llm-d.ai/community/events) for upcoming community events and [llm-d on YouTube](https://www.youtube.com/@llm-d) for talks and demos. Come build with us.
  * [Delivering on Promises: Feature Graduation](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#delivering-on-promises-feature-graduation)
  * [Upstream, Not Fork: Aligning with vLLM](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#upstream-not-fork-aligning-with-vllm)
  * [Beyond Kubernetes: RL, Slurm, and File Discovery](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#beyond-kubernetes-rl-slurm-and-file-discovery)
  * [Intelligent Routing for the Agentic Era](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#intelligent-routing-for-the-agentic-era)
  * [Engine-Agnostic: SGLang as a First-Class Backend](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#engine-agnostic-sglang-as-a-first-class-backend)
  * [Multi-Tier KV Cache and Hybrid Model Support](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#multi-tier-kv-cache-and-hybrid-model-support)
  * [New Well-Lit Paths and Ecosystem Reach](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#new-well-lit-paths-and-ecosystem-reach)
  * [What This Means for You](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#what-this-means-for-you)
  * [Community and Contribution](https://llm-d.ai/blog/llm-d-v0.8-from-platform-to-control-plane#community-and-contribution)


