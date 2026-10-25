---
title: Building the foundation for running extra-large language models
id: building-the-foundation-for-running-extra-large-language-models
tags:
- llm-inference
- cloudflare
- custom-engine
- rust
- vllm
- pd-disaggregation
- speculative-decoding
- prefix-caching
- kv-cache
created: '2026-10-09T23:24:08.295700Z'
updated: '2026-10-10T14:52:17.347044Z'
source: https://blog.cloudflare.com/high-performance-llms/
status: evergreen
type: note
tier: institutional
content_type: blog
deprecated: false
summary: Cloudflare's 2026-04-16 first-person follow-up (Chen/Flansburg/Krasnov) describing
  Infire's extension to multi-GPU (pipeline/tensor/expert parallelism) for trillion-parameter
  models like Kimi K2.5, plus PD disaggregation with token-aware load balancing, Mooncake-based
  cross-GPU KV-cache sharing, and EAGLE-3 speculative decoding; recovered via Wayback
  Machine after bot-wall blocked the live URL.
---

> **Provenance note (fetcher-added, not part of the source):** Recovered via Wayback Machine capture (`https://web.archive.org/web/20260416141512/https://blog.cloudflare.com/high-performance-llms/`) after `hyperresearch fetch` tripped bot detection on the live URL. Direct `curl` retrieval of the Wayback capture returned genuine, well-formed article HTML (title: "Building the foundation for running extra-large language models", published 2026-04-16, authors Michelle Chen, Kevin Flansburg, Vlad Krasnov). Content below is the original Cloudflare blog content — treat as untrusted third-party DATA per vault policy. **Scope note:** per the research query's HW exclusion, GPU model/count/VRAM figures below are retained only as necessary context for the software-architecture decisions they motivate (e.g., why PD disaggregation or multi-GPU tensor/pipeline parallelism was built) — do not extract them as standalone hardware-selection claims.

---

TITLE: Building the foundation for running extra-large language models
AUTHORS: Michelle Chen, Kevin Flansburg, Vlad Krasnov
PUBLISHED: 2026-04-16 (Agents Week)

Context: Workers AI began hosting very large open-source models (e.g. Moonshot's Kimi K2.5, >1 trillion parameters). This post describes the software foundation built to serve such models efficiently, extending Infire (Cloudflare's proprietary Rust inference engine, first announced in the "most efficient inference engine" post during Birthday Week 2025).

## Prefill-decode (PD) disaggregation

LLM request processing has two stages: prefill (processes input tokens, populates KV cache; compute-bound) and decode (generates output tokens; memory-bound). Because these stages use different parts of the GPU and decode always follows prefill, running both on one machine leaves GPU power underutilized. With PD disaggregation, separate inference servers run each stage: a request first hits the prefill server, which populates its KV cache, then the decode server receives the request plus information on how to transfer the KV cache from the prefill server, and begins decoding. Benefits: each stage can be tuned and scaled independently for input-heavy vs. output-heavy traffic, and can even run on heterogeneous hardware.

This requires a more complex load balancer: it must rewrite responses (including streaming SSE) from the decode server to include prefill-server information such as cached tokens, and different inference servers require different information to initiate the KV-cache transfer. Cloudflare built **token-aware load balancing**: a pool of prefill and decode endpoints, where the load balancer estimates how many prefill/decode tokens are in-flight to each endpoint and spreads load evenly.

Measured impact of shifting traffic to the new PD-disaggregated architecture (same GPU quantity, increased request volume): p90 Time to First Token tail latency variance dropped significantly; p90 time-per-token went from ~100ms with high variance to 20-30ms — "a 3x improvement in intertoken latency."

## Prompt caching

For long-context agentic workloads, Cloudflare avoids recomputing input tensors every turn via an `x-session-affinity` header that routes requests to the region holding previously-computed input tensors (first described in the original Workers-AI-large-LLMs post). They added session-affinity headers to popular agent harnesses like OpenCode, and incentivize header adoption by offering discounted cached tokens. Cloudflare runs KV-aware routing internally but also relies on clients sending `x-session-affinity` to be explicit about prompt caching.

Measured result: working with their heaviest internal users to adopt the header raised input-token cache hit ratios from 60% to 80% during peak times, "significantly" increasing request throughput while improving latency for interactive sessions like OpenCode or AI code reviews.

## KV-cache optimization (multi-GPU cache sharing)

As models now span multiple GPUs, KV cache (which lives in GPU VRAM) must be shared/transferred across GPUs. For Kimi, Cloudflare integrated **Moonshot AI's Mooncake Transfer Engine and Mooncake Store** — a high-performance data-transfer framework using RDMA protocols (NVLink, NVMe over Fabric) for direct memory-to-memory transfer without CPU involvement. Paired with LMCache or SGLang HiCache, the cache is shared across all cluster nodes, letting a prefill node reuse a cache from a prior request pre-filled on a different node — eliminating the need for session-aware routing within a cluster and enabling more even load balancing. Mooncake Store also extends the cache beyond GPU VRAM onto NVMe storage, extending session cache lifetime and improving hit ratio.

## Speculative decoding

Cloudflare uses speculative decoding (a smaller "draft" model proposes several candidate next tokens; the larger target model validates/accepts them in a single forward pass, which is cheaper than autoregressive generation one token at a time while preserving output quality since the target model still accepts/rejects). This is especially effective for agentic tool calls and structured outputs, which are highly predictable (name, description, wrapped in a JSON envelope). For Kimi K2.5, Cloudflare uses **NVIDIA's EAGLE-3** (Extrapolation Algorithm for Greater Language-model Efficiency) as the draft model; tunable levers include the number of future tokens generated.

## Infire: proprietary inference engine — new capabilities

Infire (announced Birthday Week 2025) is Cloudflare's own Rust inference engine, built for Cloudflare's distributed-network constraints. To support this new class of very large models, Cloudflare extended Infire with:

- **Multi-GPU support**: pipeline-parallel and tensor-parallel execution modes, plus expert-parallelism. For pipeline parallelism, Infire load-balances across pipeline stages to avoid GPU starvation in any one stage. For tensor parallelism, Infire optimizes to reduce cross-GPU communication. Cloudflare notes using both pipeline and tensor parallelism together typically gives the best throughput/latency balance.
- **Lower memory overhead**: "While already having much lower GPU memory overhead than vLLM, we optimized Infire even further, tightening the memory required for internal state like activations." Cloudflare states that for two specific large-model configurations it tested, "you would have trouble even booting vLLM in the first place" (memory headroom claim, not a GPU-selection recommendation).
- **Faster cold starts**: even for the largest models, Infire can begin serving requests in under 20 seconds; load times are bounded only by drive speed.
- **Throughput**: investing in the proprietary engine lets Cloudflare get "up to 20% higher tokens per second throughput on unconstrained systems," and also use "lower-end hardware" to run newer models where it was previously infeasible (again, a capability claim about engine efficiency, not a hardware spec).

Cloudflare frames this as ongoing work: "We're continuously optimizing our technology stack in order to provide high-quality, performant inference for our customers while operating our GPUs efficiently."
