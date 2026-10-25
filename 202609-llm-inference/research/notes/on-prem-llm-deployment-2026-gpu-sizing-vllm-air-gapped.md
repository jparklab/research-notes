---
title: 'On-Prem LLM Deployment (2026): GPU Sizing, vLLM & Air-Gapped'
id: on-prem-llm-deployment-2026-gpu-sizing-vllm-air-gapped
tags:
- onprem-llm-inference-stacks-859099
- air-gapped
- vendor-marketing
- reliability-low-medium
created: '2026-10-23T17:22:00.067176Z'
updated: '2026-10-23T17:25:06.926208Z'
source: https://iternal.ai/how-to-deploy-llm-on-premise
source_domain: iternal.ai
fetched_at: '2026-10-23T17:22:00.036018Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Iternal.ai vendor guide (AirgapAI seller; reliability low-medium, useful
  for mechanics). Air-gap section: two-phase connected-staging workflow with SHA-256/signature
  verification, NIM download-to-cache / AIR_GAP_NIM_CACHE mounted at /opt/nim/.cache
  without NGC_API_KEY, vLLM snapshot_download + local path + HF_HUB_OFFLINE=1; Table
  of telemetry kill switches (TRANSFORMERS_OFFLINE, VLLM_NO_USAGE_STATS, HF_HUB_DISABLE_TELEMETRY);
  Harbor/oc-mirror mirrors; update cadence asserted monthly (healthcare) to quarterly
  (defense) with no cited source.'
---

[Skip to main content](https://iternal.ai/how-to-deploy-llm-on-premise#main-content)
Research Report — 2026
#  On-Prem LLM Deployment: How to Deploy an LLM On-Premise in 2026
A step-by-step engineering guide to deploying a large language model on hardware you own and control: VRAM and GPU sizing math, model selection, vLLM vs NVIDIA NIM, multi-GPU scaling, quantization, air-gapped setup, CPU inference on Intel Xeon, and total cost of ownership versus the cloud.
On-Prem LLM
VRAM Sizing
vLLM
NVIDIA NIM
Air-Gapped
~14 GBVRAM for a 7B model at FP16
8–18xCheaper per token vs cloud (at scale)
~3.7 mo8x H100 break-even vs Azure on-demand
71%AI infra outside public cloud by 2025
## On-prem LLM, summarized
**An on-prem LLM is an open-weight model served on hardware inside your own network boundary, with no third-party API in the path.** The work is sizing (VRAM for weights plus KV cache), a serving stack (vLLM or NVIDIA NIM behind an OpenAI-compatible endpoint), and operations (quantization, batching, air-gapped install). The six-step path below links each to its section.
  1. [Choose an open-weight model](https://iternal.ai/how-to-deploy-llm-on-premise#step-model) — clear the license first, then pick the size class the workload actually needs.
  2. [Do the VRAM math](https://iternal.ai/how-to-deploy-llm-on-premise#step-vram) — weights, plus KV cache at your context and concurrency, plus a 1.3–2.0x overhead multiplier.
  3. [Select GPUs (or Intel Xeon CPUs)](https://iternal.ai/how-to-deploy-llm-on-premise#step-gpu) — memory bandwidth sets decode speed; capacity sets which model fits.
  4. [Pick a serving stack](https://iternal.ai/how-to-deploy-llm-on-premise#step-serving) — vLLM for flexibility and zero license cost, NVIDIA NIM for a supported container. Both speak the OpenAI API.
  5. [Quantize and budget the KV cache](https://iternal.ai/how-to-deploy-llm-on-premise#step-quantize) — FP8 for throughput, INT4 weight-only for latency-bound serving, prefix caching for long context.
  6. [Scale, secure and operate](https://iternal.ai/how-to-deploy-llm-on-premise#section-ops) — tensor and pipeline parallelism, air-gapped install, queue-depth autoscaling, and a go-live checklist.


For more information visit the [Deploy Local AI](https://iternal.ai/jobs/deploy-local-ai) page.
## How do you deploy an LLM on-premise?
**Deploying an LLM on-premise means running the model weights, serving engine, and API entirely on hardware you own, inside your own network boundary.** Size VRAM first (a 7B model needs one 24 GB GPU; a 70B model needs 2–4x 80 GB GPUs), then serve it with vLLM or NVIDIA NIM. At sustained volume, on-prem inference runs 8–18x cheaper per token than cloud APIs.
##  On-Premise LLM Deployment in 2026: What This Guide Covers
Deploying a large language model on-premise means running the full inference stack — model weights, serving engine, and API — on hardware you own and control, inside your own network boundary, with no dependency on a third-party API. For enterprises in regulated industries, that control is increasingly non-negotiable: by 2025, roughly 71% of AI infrastructure ran outside the public cloud, a shift driven heavily by financial-services data-residency requirements and the arrival of enforceable AI regulation.
The good news for platform teams is that the open-weight model ecosystem has matured to the point where self-hosted models rival frontier hosted APIs on most enterprise tasks, and the serving software — vLLM, NVIDIA NIM, SGLang, TensorRT-LLM — is production-hardened. The hard part is no longer "can we run it" but "how do we size it correctly and operate it reliably." This guide walks the full decision path in order: when on-prem makes sense, how to choose a model, how to compute exact VRAM requirements, how to select GPUs, how to pick and configure a serving stack, how to scale across GPUs and nodes, how to quantize, how to plan capacity from real demand, how to deploy in an air-gapped enclave, what it actually costs versus cloud, and how to run it in production.
A note on numbers: hardware specs and formulas in this guide are stable, but model versions and software defaults drift monthly. Where we name models, we use families and tiers rather than chasing point releases; where benchmarks are version-specific, we say so. For deeper dives, see our companion [Hardware Sizing Guide](https://iternal.ai/hardware-sizing-guide), [LLM Selection Guide](https://iternal.ai/llm-selection-guide), and [Best AI Tools for Air-Gapped Environments](https://iternal.ai/best-ai-air-gapped-environments).
##  LLM On-Premise Requirements: What You Need by Model Size
Running an LLM on premise takes three things: enough GPU (or CPU) memory for the model tier you choose, a production serving engine, and an operations plan. For a **7–8B model** — the sweet spot for chat, summarization, and RAG over internal documents — a single 24 GB GPU (RTX 4090, L4) or a modern Intel Xeon CPU server is enough. A **32–34B model** wants 48–80 GB of VRAM (one A100/H100, or two 24 GB cards with tensor parallelism). A **70B model** needs 2–4x 80 GB GPUs at FP8/BF16, or a single 80 GB card with INT4 quantization for lower-concurrency serving.
On the software side, the standard on-premise LLM stack is Linux with NVIDIA drivers/CUDA, vLLM or NVIDIA NIM for serving, and Prometheus/Grafana for monitoring — all installable inside an air-gapped enclave. Budget for people, not just metal: staffing is often the largest TCO line item. Use our [Hardware Sizing Guide](https://iternal.ai/hardware-sizing-guide) for exact configurations, or skip GPU infrastructure entirely with [AirgapAI](https://iternal.ai/airgapai), which runs local models on a standard business laptop. Before committing capital, weigh the [edge AI economics](https://iternal.ai/edge-ai-vs-cloud-economics) of owned infrastructure against per-token subscription pricing.
##  On-Prem vs Cloud: When to Run LLMs in Your Own Data Center
The decision between on-premise and a hosted API turns on four axes: utilization, volume, data sovereignty, and latency.
On-premise wins when inference demand is **sustained, predictable, and high-volume**. The economics are unforgiving of idle GPUs but generous to busy ones — against hyperscaler on-demand pricing, an owned cluster typically breaks even somewhere above roughly 50–83% sustained GPU utilization, and a fully-utilized owned cluster delivers token costs 8–18x lower than equivalent cloud over a multi-year horizon. It also wins outright when **data sovereignty is a legal mandate** : under GDPR Article 46, EU financial institutions cannot freely route customer data through US-hosted LLM APIs, and the EU AI Act's general-purpose-AI obligations — enforceable since August 2025 — carry fines up to €35 million or 7% of global turnover. For regulated finance, healthcare, government, and defense, the deployment location is decided before any cost spreadsheet is opened.
Cloud and hosted APIs win when demand is **spiky or unpredictable** , when volume is low (small and mid-size workloads below ~10M tokens/month outside small-model cases), when you need frontier closed models, or when you must scale fast without capital expenditure. Token prices on hosted APIs also fell roughly 80% from 2025 to 2026, which structurally erodes the on-prem cost advantage over time and should be modeled, not assumed away.
Rule of Thumb
If you can keep GPUs busy more than ~4–5 hours per day equivalent over a multi-year horizon, or if regulation forces your hand, on-prem is the default; otherwise start in the cloud and revisit at volume. For a full treatment of the crossover math, see [Edge AI vs Cloud Economics](https://iternal.ai/edge-ai-vs-cloud-economics) and the dedicated TCO section later in this guide.
##  Step 1: Choose the Right Open-Weight Model
### License first: what you can legally productize
For enterprise on-prem, the license gates everything — pick the model your legal team can clear before you benchmark quality. Three tiers matter:
Apache 2.0 / MIT
Fully permissive: no monthly-active-user caps, no naming obligations, explicit patent grant (Apache). Covers gpt-oss-120b/20b, all Qwen3 models, Mistral Small 3.x and Mixtral (Apache 2.0), and DeepSeek V3/R1 plus Phi-4 (MIT). MIT is the most permissive — DeepSeek even permits downstream distillation.
Custom community (Llama 4)
The Llama 4 Community License adds a clause requiring a separate Meta license if your products exceeded 700 million monthly active users in the calendar month before the model's release date, plus "Built with Llama" attribution and a "Llama-" model-name prefix. It is not OSI-approved open source.
Use-restricted (Gemma)
Google's Gemma license permits commercial use after accepting its Terms of Use and Prohibited Use Policy, but is not Apache/MIT and carries redistribution restrictions.
### MoE vs dense: the distinction that decides serveability
Mixture-of-Experts (MoE) models — gpt-oss, Qwen 3.5 / 3.6, DeepSeek V3.2 / V4, Kimi K2 Thinking, GLM-5, MiniMax M2.7, Gemma 4 26B-A4B, Llama 4 — activate only a fraction of their parameters per token, which lowers per-token compute and raises throughput. But the critical sizing insight is that **VRAM must hold all total parameters** (every expert is resident in GPU memory), while only the active parameters drive compute. A 671B MoE with 37B active still needs roughly 700 GB at FP8, and the 2026 frontier open-weights (DeepSeek V4, Kimi K2, GLM-5) are 700B–1.6T total — they require multi-GPU and usually multi-node serving (see [Step 6: Scaling](https://iternal.ai/how-to-deploy-llm-on-premise#step-scaling)). Dense models (Qwen3.6-27B, Phi-4, Gemma 3, Mistral Small) are simpler to serve, have more predictable latency, and are easier to fully fit and quantize on a single GPU — which is why they are often the better choice for constrained single-node on-prem. MoE models are also especially CPU-friendly because so few parameters activate per token — see [Option B: Run Inference on Intel Xeon CPUs](https://iternal.ai/how-to-deploy-llm-on-premise#cpu-xeon-airgapai-edge).
#### Table 1 — Open-Weight Model Landscape for Enterprise On-Prem (May 2026)  
| Model  | Total Params  | Active Params  | Arch  | Context  | License  | On-prem note  |  
| --- | --- | --- | --- | --- | --- | --- |  
| Frontier open-weights — multi-GPU / multi-node |  
| **DeepSeek V4-Pro**  | ~1.6T  | ~49B  | MoE  | 1M  | MIT  | Most permissive frontier; needs a multi-node cluster (8+ GPUs at FP8/INT4)  |  
| **Kimi K2 Thinking**  | ~1T  | ~32B  | MoE (reasoning)  | 256K  | Modified MIT  | Top agentic / coding scores (SWE-bench Pro leader); multi-node  |  
| **GLM-5**  | ~744B  | ~40B  | MoE  | 200K  | MIT  | Strong permissive frontier; multi-GPU  |  
| **DeepSeek V3.2**  | 671B  | ~37B  | MoE (MLA, 256+1 shared)  | 128K  | MIT  | Most permissive; distillation allowed; MLA shrinks KV cache ~28x  |  
| **DeepSeek R1**  | 671B  | 37B  | MoE (reasoning)  | 128K  | MIT  | Distilled 1.5–70B variants run on a single GPU  |  
| **MiniMax M2.7**  | ~230B  | ~10B  | MoE  | 200K+  | Modified MIT  | Long-context agentic; open weights  |  
| **Qwen3.5-397B-A17B**  | 397B  | 17B  | MoE (GDN + sparse)  | 262K (→1M)  | Apache 2.0  | Largest open Qwen flagship; fully permissive  |  
| **Llama 4 Maverick**  | 400B  | 17B  | MoE (128 exp), multimodal  | 1M  | Llama 4 Community  | 700M-MAU clause; "Built with Llama"  |  
| Deployable single-node (1–2x 80GB GPUs) |  
| **gpt-oss-120b**  | 116.8B  | 5.1B  | MoE (128 exp/4 active)  | 131K  | Apache 2.0  | Fully permissive; single 80GB GPU via MXFP4  |  
| **Llama 4 Scout**  | 109B  | 17B  | MoE (16 exp), multimodal  | up to 10M  | Llama 4 Community  | Fits 1–2x 80GB; 700M-MAU clause  |  
| **Qwen3-Coder-Next**  | 80B  | 3B  | MoE  | 256K  | Apache 2.0  | Coding / agents; very low active-param footprint  |  
| **Devstral 2 (Mistral)**  | 123B  | 123B  | Dense  | 256K  | Apache 2.0  | Coding-tuned dense; predictable latency  |  
| Small / single-GPU / edge & CPU |  
| **Qwen3.6-27B (dense)**  | 27B  | 27B  | Dense  | 262K  | Apache 2.0  | Single-GPU general / RAG; long context  |  
| **Gemma 3 27B**  | 27B  | 27B  | Dense, multimodal  | 128K  | Gemma (use-restricted)  | Commercial OK after terms; not OSI  |  
| **Mistral Small 3.x 24B**  | 24B  | 24B  | Dense, multimodal  | 128K  | Apache 2.0  | Strong single-GPU mid-size pick  |  
| **gpt-oss-20b**  | 20.9B  | 3.6B  | MoE (32 exp/4 active)  | 131K  | Apache 2.0  | Runs in 16GB; ideal for edge / Xeon CPU (Option B)  |  
| **Phi-4 14B**  | 14B  | 14B  | Dense  | 128K  | MIT  | Strong math; synthetic-data trained  |  
Total/active parameter counts marked with "~" are approximate where providers have not published exact figures for the newest frontier releases — verify specs and license terms against the model card before sizing. Benchmark scores and rankings for these models are tracked live on our [LLM Benchmark Repository](https://iternal.ai/llm-benchmark-repository).
#### Table 1b — Full Variant Lineups: Qwen 3.5, Qwen 3.6 & Gemma 4 (May 2026)
These three families ship a complete size ladder from sub-1B edge models to 397B-parameter MoE flagships, all under permissive licenses — making them the most common starting point for on-prem standardization. The small Gemma 4 and Qwen variants are also the best fit for [Intel Xeon CPU inference (Option B)](https://iternal.ai/how-to-deploy-llm-on-premise#cpu-xeon-airgapai-edge); the Gemma 4 26B-A4B MoE is the exact model benchmarked there.  
| Variant  | Total Params  | Active Params  | Arch / Modality  | Context  | License  | Best-fit deployment  |  
| --- | --- | --- | --- | --- | --- | --- |  
| Qwen 3.6 — open weights (Apr 2026; multimodal, hybrid-thinking) |  
| **Qwen3.6-35B-A3B**  | 35B  | ~3B  | MoE / text+vision+code  | 262K (→1M)  | Apache 2.0  | Flagship open MoE; ~21 GB at Q4, ~120 tok/s on one RTX 4090  |  
| **Qwen3.6-27B**  | 27B  | 27B  | Dense / text+vision+code  | 262K (→1M)  | Apache 2.0  | Flagship-level coding; ~16.8 GB at Q4 on a single consumer GPU  |  
| **Qwen3-Coder-Next**  | 80B  | 3B  | MoE / code+agents  | 256K  | Apache 2.0  | Coding/agent specialist; very low active footprint  |  
| Qwen 3.5 — full family, 0.8B–397B (Feb 2026; multimodal, GDN + MoE, 262K native) |  
| **Qwen3.5-397B-A17B**  | 397B  | 17B  | MoE / multimodal  | 262K (→1M)  | Apache 2.0  | Frontier flagship; multi-node cluster  |  
| **Qwen3.5-122B-A10B**  | 122B  | 10B  | MoE / multimodal  | 262K  | Apache 2.0  | High-end; 2–4x 80 GB GPUs  |  
| **Qwen3.5-35B-A3B**  | 35B  | 3B  | MoE / multimodal  | 262K  | Apache 2.0  | Single-node; throughput-friendly (low active params)  |  
| **Qwen3.5-27B**  | 27B  | 27B  | Dense / multimodal  | 262K  | Apache 2.0  | Single 48–80 GB GPU; predictable latency  |  
| **Qwen3.5-9B**  | 9B  | 9B  | Dense / multimodal  | 262K  | Apache 2.0  | Single 24 GB GPU; punches above its size  |  
| **Qwen3.5-4B**  | 4B  | 4B  | Dense / multimodal  | 262K  | Apache 2.0  | Lightweight agents; edge / Xeon CPU  |  
| **Qwen3.5-2B**  | 2B  | 2B  | Dense / multimodal  | 262K  | Apache 2.0  | Phones, tablets, embedded  |  
| **Qwen3.5-0.8B**  | 0.8B  | 0.8B  | Dense / multimodal  | 262K  | Apache 2.0  | <2 GB VRAM at full precision; micro-edge  |  
| Gemma 4 — four variants (Apr 2026; multimodal text+image, audio on small) |  
| **Gemma 4 31B**  | 30.7B  | 30.7B  | Dense / multimodal  | 256K  | Apache 2.0  | Flagship dense; reportedly rivals far larger models  |  
| **Gemma 4 26B-A4B**  | 26B  | 3.8B  | MoE (8 of 128 exp) / multimodal  | 256K  | Apache 2.0  | MoE; 3.69x faster than 31B dense on Intel Xeon CPU (Option B)  |  
| **Gemma 4 E4B**  | ~4.5B eff.  | ~4.5B eff.  | Dense (edge) / multimodal  | 128K  | Apache 2.0  | Edge-optimized; laptops, workstations, Xeon CPU  |  
| **Gemma 4 E2B**  | ~2.3B eff. (~5.1B w/ PLE)  | ~2.3B eff.  | Dense (edge) / multimodal  | 128K  | Apache 2.0  | Fits ~2 GB at Q4; runs on a Raspberry Pi  |  
Qwen "Plus" / "Max" tiers (e.g. Qwen3.5-Plus, Qwen 3.7 Max) are **hosted, closed-weight** Alibaba Cloud endpoints and are not deployable on-prem — only the numbered open-weight variants above ship downloadable weights. Gemma 4 ships under the permissive **Apache 2.0** license — a notable change from the use-restricted custom Gemma license used through Gemma 3. Gemma 4 "E" sizes (E2B / E4B) use effective-parameter counts (per-layer embeddings / MatFormer), so on-disk size differs from the effective figure.
### Model selection by use case
#### Table 2 — Use-Case Model Selection (on-prem)  
| Use case  | Recommended models  | Why  |  
| --- | --- | --- |  
| **General chat / assistant**  | Qwen3.6-27B, Gemma 4 31B, Mistral Small 3.x 24B, Llama 4 Scout (if MAU < 700M)  | Strong general quality, single-node serveable, permissive (except Llama)  |  
| **RAG / grounded enterprise**  | Qwen3.6-27B, Gemma 4 31B / 26B-A4B, Phi-4 14B, DeepSeek V3.2 (if cluster available)  | Dense, predictable latency, long context, easy to fully fit/quantize  |  
| **Coding**  | Kimi K2 Thinking, Qwen3-Coder-Next, gpt-oss-120b, DeepSeek V3.2, Devstral 2  | Leading SWE-bench Pro / agentic-coding scores, strong tool use  |  
| **Reasoning / agentic**  | DeepSeek V4 / R1, Kimi K2 Thinking, GLM-5, Qwen 3.6 (thinking mode), gpt-oss-120b  | RL-trained chain-of-thought, configurable reasoning effort  |  
| **Edge / CPU-constrained**  | Gemma 4 E2B / E4B, gpt-oss-20b (16GB), Qwen3.5-2B / 4B, Phi-4  | Small footprint, on-device / Intel Xeon CPU inference (Option B)  |  
For RAG specifically, model choice is only half the equation — retrieval quality dominates grounded accuracy. Pair a dense long-context model with a disciplined ingestion pipeline; see [Blockify Data Ingestion](https://iternal.ai/blockify-data-ingestion) for how to structure source data before it reaches the model. For a fuller decision tree across every family, see the [LLM Selection Guide](https://iternal.ai/llm-selection-guide).
##  Step 2: Do the VRAM Math (Weights + KV Cache + Overhead)
GPU memory for inference splits into four buckets: model weights, KV cache, activations, and framework/CUDA overhead. Weights and KV cache dominate. Get this math right and the rest of the deployment falls into place; get it wrong and you will either over-buy hardware or hit out-of-memory failures in production.
### Model weights
The weights formula is exact:
Model WeightsVRAM_weights = num_params × bytes_per_param
#### Table 3 — Bytes per parameter by precision  
| Precision  | Bytes/param  | VRAM per 1B params (weights)  | Notes  |  
| --- | --- | --- | --- |  
| **FP32**  | 4  | ~4 GB  | Full precision; rarely used for inference  |  
| **FP16 / BF16**  | 2  | ~2 GB  | Standard inference precision  |  
| **FP8**  | 1  | ~1 GB  | Native DeepSeek-V3 training/inference precision  |  
| **INT8**  | 1  | ~1 GB  | 8-bit quantization  |  
| **INT4 / 4-bit**  | 0.5  | ~0.5 GB  | Aggressive quantization (GPTQ/AWQ/GGUF Q4)  |  
Canonical Anchor
A 7B model in FP16 needs about 7B × 2 bytes = ~14 GB of VRAM for weights alone.
### KV cache (the long-context tax)
During decoding the model caches the Key and Value tensors of every prior token so it does not recompute attention each step. NVIDIA's formulas are:
KV CacheKV bytes per token = 2 × num_layers × (num_heads × head_dim) × precision_bytes KV bytes total = batch_size × seq_len × 2 × num_layers × hidden_size × precision_bytes
The leading 2 accounts for the separate Key and Value tensors, and hidden_size = num_heads × head_dim. KV cache scales **linearly with both context length and batch size** while weights stay fixed — so at long context or high concurrency the KV cache can rival or exceed weight memory and becomes the binding constraint. Two corrections keep modern models from matching the naive formula's worst case:
  * **GQA (Grouped-Query Attention):** Replace num_heads with the smaller num_kv_heads. Llama 3 70B has 64 query heads but only 8 KV heads — an 8x KV-cache reduction versus full multi-head attention.
  * **MLA (Multi-head Latent Attention), DeepSeek-V3:** Stores a 512-dim latent per token instead of the full KV, roughly 28x smaller, cutting a ~213.5 GB max cache down to ~7.6 GB.


Worked KV Example
NVIDIA, Llama 2 7B, FP16, batch 1, seq 4096, 32 layers, hidden 4096: 1 × 4096 × 2 × 32 × 4096 × 2 bytes ≈ **2 GB**.
### Activations and framework overhead
Add a runtime multiplier on top of weights. A practical rule of thumb: total VRAM ≈ weights × 1.3–1.5 for moderate concurrency and context, rising to × 1.5–2.0 for long context or high concurrency. Modal's compact sizing formula folds this in:
Compact Sizing (Modal)M (GB) = P (billions) × (Q / 8) × 1.2 (Q = bit precision, 1.2 = ~20% overhead) Example: 70B at INT4 = 70 × (4/8) × 1.2 = 42 GB
### Worked per-model VRAM tables
#### Table 4 — Worked VRAM examples (weights + ~15–20% overhead unless noted)  
| Model  | Params (total / active)  | Config (layers / hidden / KV heads)  | FP16 total  | INT8  | INT4 / 4-bit  |  
| --- | --- | --- | --- | --- | --- |  
| **Mistral 7B**  | 7B / 7B  | 32 / 4096 / 8 (GQA)  | ~18 GB  | ~9 GB  | ~5 GB  |  
| **Llama 3.1 8B**  | 8B / 8B  | 32 / 4096 / 8 (GQA)  | ~20 GB  | ~10 GB  | ~6 GB  |  
| **Llama 2 13B**  | 13B / 13B  | 40 / 5120 / 40 (MHA)  | ~26 GB  | ~14 GB  | ~8 GB  |  
| **Llama 3.3 70B**  | 70B / 70B  | 80 / 8192 / 8 (GQA)  | ~168 GB  | ~84 GB  | ~46 GB  |  
| **DeepSeek V3.2 (MoE)**  | 671B / 37B  | 61 / 7168 / MLA (d_c=512)  | ~1,543 GB  | ~671 GB (FP8)  | ~386 GB  |  
MoE Sizing Gotcha
DeepSeek-V3's weights bill the full 671B parameters (all experts resident), but per-token compute bills only the 37B active. This is the single most common point of confusion in MoE sizing. For the full per-tier mapping including which GPU each cell requires, see the GPU section below and the [LLM server configurations by budget](https://iternal.ai/hardware-sizing-guide#section-llm-server).
##  Step 3: Select Your GPUs (H100 / H200 / A100 / L40S / Blackwell / RTX)
### The two axes that decide inference: capacity and bandwidth
Two GPU properties govern LLM serving. **VRAM capacity** gates which model and context length fit at all. **Memory bandwidth** governs decode/token-generation latency, because decode is memory-bandwidth-bound: every new token streams all model weights from HBM once per forward pass. This is why the H200 — which has compute identical to the H100 but 43% more bandwidth (4.8 TB/s vs 3.35 TB/s) — generates tokens roughly 43% faster in the small-batch (memory-bound) regime, despite no compute uplift.
Reading Note
NVIDIA datasheets usually headline the "with sparsity" (2:4) tensor numbers. The dense throughput is half. The table below reports dense / sparse explicitly so you do not double-count.
### Data-center and workstation GPU comparison
#### Table 5 — NVIDIA Data-Center / Pro GPU Specs for LLM Inference (2025–2026)  
| GPU (variant)  | Arch / Tensor Gen  | VRAM  | Mem Bandwidth  | FP8 (dense / sparse) TFLOPS  | FP16/BF16 (dense / sparse)  | FP4 (dense / sparse)  | NVLink/GPU  | TDP  |  
| --- | --- | --- | --- | --- | --- | --- | --- | --- |  
| **A100 SXM (40GB)**  | Ampere / 3rd  | 40GB HBM2e  | 1,555 GB/s  | N/A (no FP8)  | 312 / 624  | N/A  | NVLink3 600 GB/s  | 400W  |  
| **A100 SXM (80GB)**  | Ampere / 3rd  | 80GB HBM2e  | ~2,039 GB/s  | N/A (no FP8)  | 312 / 624  | N/A  | NVLink3 600 GB/s  | 400W  |  
| **H100 SXM5 (80GB)**  | Hopper / 4th  | 80GB HBM3  | 3,350 GB/s  | 1,979 / 3,958  | 989 / 1,979  | N/A  | NVLink4 900 GB/s  | 700W  |  
| **H100 PCIe (80GB)**  | Hopper / 4th  | 80GB HBM2e  | 2,000 GB/s  | ~1,513 / ~3,026  | ~756 / ~1,513  | N/A  | Bridge 600 GB/s  | 350W  |  
| **H200 SXM (141GB)**  | Hopper / 4th  | 141GB HBM3e  | 4,800 GB/s  | 1,979 / 3,958  | 989 / 1,979  | N/A  | NVLink4 900 GB/s  | 700W  |  
| **L4 (24GB)**  | Ada / 4th  | 24GB GDDR6  | ~300 GB/s  | ~242 / ~485  | ~121 / ~242  | N/A  | None (PCIe)  | 72W  |  
| **L40S (48GB)**  | Ada / 4th  | 48GB GDDR6 ECC  | 864 GB/s  | 733 / 1,466  | 366 / 733  | N/A  | None (PCIe)  | 300W  |  
| **RTX 6000 Ada (48GB)**  | Ada / 4th  | 48GB GDDR6 ECC  | 960 GB/s  | ~728 / ~1,457  | ~364 / ~728  | N/A  | None  | 300W  |  
| **B200 SXM (192GB)**  | Blackwell / 5th  | 192GB HBM3e  | 8,000 GB/s  | 4,500 / 9,000  | 2,250 / 4,500  | 9,000 / 18,000  | NVLink5 1,800 GB/s  | 1,000W  |  
| **GB200 (= 2x B200 + Grace)**  | Blackwell / 5th  | 2x192GB HBM3e  | 2x 8,000 GB/s  | 2x 4,500 dense  | 2x 2,250 dense  | 2x 9,000 dense  | NVLink5 1,800 GB/s  | ~2,700W  |  
| **RTX PRO 6000 Blackwell (96GB)**  | Blackwell / 5th  | 96GB GDDR7 ECC  | 1,800 GB/s  | ~2,000 (AI TOPS class)  | —  | ~4,000 AI TOPS  | None  | 600W (WS) / 300W (Server)  |  
Organize procurement by generation, because precision support tracks it: Ampere (A100) tops out at INT8/FP16 with no FP8; Hopper (H100/H200) adds FP8; Blackwell (B200/GB200, RTX PRO 6000) adds native FP4/NVFP4, which roughly doubles throughput and halves memory versus FP8 and is the 2025–2026 cost-per-token frontier. Note that L40S, L4, and the RTX cards have no NVLink — they scale only over PCIe, which makes them better suited to pipeline parallelism than tensor parallelism (see Step 6).
#### Table 6 — Approximate Model-Size Fit by VRAM (weights-only, +20–40% for KV/runtime)  
| Model size  | FP16 weights (~2GB/1B)  | INT4 (~0.5GB/1B)  | Single-GPU fit (FP16)  | Single-GPU fit (INT4)  |  
| --- | --- | --- | --- | --- |  
| ~14 GB  | ~4 GB  | Any 24GB+ (L40S/A100/H100 easily)  | Any 8GB+  |  
| **13B**  | ~26 GB  | ~7 GB  | 48GB+ (L40S / RTX 6000 Ada / A100-80 / H100)  | 24GB (L4 tight)  |  
| **34B**  | ~68 GB  | ~17 GB  | 80GB+ (A100-80 / H100); tight  | 48GB (L40S / RTX 6000 Ada)  |  
| **70B**  | ~140 GB  | ~35–40 GB  | 141GB H200 single GPU; else 2x H100 (TP)  | 48GB tight / 80GB comfortably  |  
| **180B (Falcon-class)**  | ~360 GB  | ~90 GB  | Multi-GPU only  | 96GB RTX PRO 6000 / B200 192GB  |  
| **Trillion-param (MoE)**  | Rack-scale  | Rack-scale  | GB200 NVL72 (72-GPU NVLink domain)  | GB200 NVL72  |  
Production Caveat (70B row)
NVIDIA NIM's supported production minimum for a 70B at BF16 is **4x 80GB GPUs** , not 2 — because 2x 80GB technically holds the ~140 GB of weights but leaves too little room for KV cache at realistic context and concurrency.
### Consumer GPUs (RTX 4090 / 5090): where they fit and where they stop
For 7B–13B single-GPU inference, consumer cards are genuinely competitive and cost a fraction of data-center GPUs — an RTX 4090 can match or beat an A100 on small models. The ceilings are firm: a single 24GB 4090 tops out around 32B at Q4; a 32GB 5090 fits 32B at Q8 and 70B only at aggressive Q2/Q3 with tiny context; comfortable 70B-Q4 needs dual GPUs (48GB combined). Neither consumer card has NVLink, so multi-GPU communication runs over PCIe, achieving roughly 85–90% of NVLink-linked throughput with about a 30% loss versus a monolithic 80GB card. For any model needing 40–80GB+ of VRAM there is no consumer alternative — data-center cards are required.
#### Table 7 — Consumer vs Data-Center Spec Comparison  
| GPU  | VRAM  | Bandwidth  | NVLink  | Native FP4  | TDP  | MSRP  |  
| --- | --- | --- | --- | --- | --- | --- |  
| **RTX 4090**  | 24GB GDDR6X  | ~1.0 TB/s  | No  | No  | 450W  | $1,599  |  
| **RTX 5090**  | 32GB GDDR7  | 1.79 TB/s  | No  | Yes (MXFP4)  | 575W  | $1,999  |  
| **A100**  | 40/80GB HBM2e  | ~2.0 TB/s  | Yes  | No  | 400W  | data-center  |  
| **H100 SXM**  | 80GB HBM3  | ~3.35 TB/s  | Yes (NVLink/NVSwitch)  | No (FP8)  | 700W  | data-center  |  
| **H200**  | 141GB HBM3e  | ~4.8 TB/s  | Yes  | No (FP8)  | 700W  | data-center  |  
##  Option B: Run Inference on Intel Xeon CPUs with AirgapAI Edge (No GPU)
GPUs are the default path, but they are not the only one. **AirgapAI Edge runs LLM inference entirely on Intel Xeon CPUs — no GPU required** — using Intel AMX (Advanced Matrix Extensions) acceleration with the OpenVINO Model Server (and llama.cpp built with AMX kernels). For teams with no GPUs, constrained power and cooling, or an existing Xeon fleet to reuse, this turns on-prem LLM serving into a software problem rather than a hardware procurement project. Learn more on the [AirgapAI](https://iternal.ai/airgapai) product page, and see the crossover math in [Edge AI vs Cloud Economics](https://iternal.ai/edge-ai-vs-cloud-economics).
2026 Internal Benchmark (verified)
A Gemma-class 26B-A4B Mixture-of-Experts model (~4B active of 26B total) at INT8 (Q8_0), on a **single half-socket Intel Xeon 6 (Granite Rapids, 48 physical cores, 768 GiB RAM)** , delivered **~32 tokens/sec single-stream decode** — about 3x reading speed and fully interactive — and scaled near-linearly to **~105 tokens/sec aggregate at 16 concurrent requests (16/16 success, zero failures)**.
### Why MoE makes CPU inference viable
On the same box, the 26B-A4B MoE model ran **3.69x faster than a 31B dense model** (8.77 tok/s at INT4) — because the MoE activates only ~4B parameters per token, dramatically easing the CPU memory-bandwidth bottleneck that throttles dense models. CPU inference is fundamentally memory-bandwidth bound: streaming fewer active weights per token is exactly what a CPU needs to stay interactive.
#### Table B1 — AirgapAI Edge on a Single Half-Socket Intel Xeon 6 (Granite Rapids, 48 cores, 768 GiB)  
| Model / Precision  | Active Params  | Single-stream decode  | Aggregate @ 16 concurrent  | Cost per page (16-way)  | Throughput per box  |  
| --- | --- | --- | --- | --- | --- |  
| **Gemma-class 26B-A4B MoE (INT8 / Q8_0)**  | ~4B of 26B  | ~32 tok/s (~3x reading speed)  | ~105 tok/s (16/16 success)  | ~$0.044 per page  | up to ~4,100 pages/day  |  
| **31B dense model (INT4)**  | 31B (all)  | 8.77 tok/s  | —  | —  | — (MoE 3.69x faster)  |  
Economics, fully on-prem / in-VPC: at 16-way concurrency on a 600-token-in to 2,000-token-out workload, AirgapAI Edge costs roughly **$0.044 per page** , about **$181/day per box** , processing up to **~4,100 pages/day per box** — with no GPU, no data egress, and no per-token API fee.
### Why CPU inference is fast enough in 2026
Intel AMX-INT8
AMX-INT8 kernels deliver roughly 2x the throughput of AMX-BF16 on Granite Rapids, turning the tile-matrix unit into the inference workhorse.
INT8 / INT4 + u8 KV cache
INT8/INT4 weight quantization plus an 8-bit (u8) KV cache shrink the memory footprint and the bandwidth the CPU must stream per token.
Prompt-lookup decoding
A free 1.4–2x speedup on RAG and grounded tasks where output echoes input — no draft model required.
Multi-token prediction
Multi-token-prediction / speculative decoding yields up to 2–3x on MoE models, compounding the AMX and quantization gains.
The Sweet Spot (and the Limits)
CPU inference is **memory-bandwidth bound** : large dense 70B-class models are slow on CPU. The sweet spot is small-to-mid dense models, MoE models, and throughput/batch workloads — extraction, RAG, summarization, classification — where aggregate tok/s and concurrency beat single-stream latency. GPUs still win for very large dense models and high-concurrency, low-latency interactive chat at scale.
### When to choose AirgapAI Edge over GPUs
Choose Xeon CPU
AirgapAI Edge
No or constrained GPUs; edge / branch / forward-deployed / air-gapped sites where GPU power, cooling, and physical security are impractical; an existing Xeon fleet to reuse; cost-sensitive batch pipelines; SCIF / classified enclaves.
Choose GPUs
NVIDIA / Data-Center
Very large dense models (70B+ dense), and high-concurrency, low-latency interactive chat at scale where single-stream latency dominates the user experience.
AirgapAI Edge is **fully offline / air-gapped** — OpenVINO runs from a local model IR with no telemetry — and pairs with [Blockify for on-prem RAG ingestion](https://iternal.ai/blockify-data-ingestion), so the entire retrieval-and-generation pipeline stays inside your boundary on CPU hardware you already own.
##  Step 4: Pick a Serving Stack — vLLM vs NVIDIA NIM
The serving engine is what turns model weights into a production API. The two leading choices for on-prem are vLLM (open-source, maximum flexibility) and NVIDIA NIM (enterprise-packaged, vendor-supported). They share an OpenAI-compatible API surface, so application code rarely changes when you switch.
### vLLM: the open-source default
vLLM is a high-throughput, memory-efficient inference engine originally from UC Berkeley (2023), built around two innovations:
  * **PagedAttention** applies OS-style virtual-memory paging to the KV cache. Each sequence's KV cache is addressed through a logical block table mapping to non-contiguous physical blocks (default block size 16 tokens), eliminating the contiguous-allocation fragmentation that wasted 60–80% of KV memory in naive serving — reducing waste to under 4%.
  * **Continuous (in-flight) batching** schedules at the per-token level: when a request finishes it immediately frees its KV blocks and the next queued request is admitted on the following step, keeping the GPU near 100% utilized. vLLM cites up to ~4x more tokens/sec versus naive Hugging Face generation.


You launch it with `vllm serve <model>`, which listens on 0.0.0.0:8000 and exposes /v1/chat/completions, /v1/completions, /v1/embeddings, /v1/models, plus /health and a Prometheus /metrics endpoint. Connect with the standard OpenAI Python client by setting base_url='http://localhost:8000/v1'; require auth with --api-key or the VLLM_API_KEY environment variable.
Minimal single-GPU vLLM launch`# Minimal single-GPU vLLM launch vllm serve /models/qwen3-32b   --gpu-memory-utilization 0.9   --max-model-len 32768   --api-key "$VLLM_API_KEY"`
#### Table 8 — Most-used vllm serve engine arguments  
| Flag  | Purpose  | Default / typical  |  
| --- | --- | --- |  
| `--tensor-parallel-size`  | Shard model across GPUs in a node  | = GPUs per node  |  
| `--pipeline-parallel-size`  | Split layers across nodes  | = number of nodes  |  
| `--gpu-memory-utilization`  | Fraction of VRAM for weights+activations+KV  | 0.9  |  
| `--max-model-len`  | Max context length  | model default  |  
| `--max-num-batched-tokens`  | Per-step token budget (controls chunked prefill)  | version/model dependent  |  
| `--max-num-seqs`  | Max concurrent sequences  | version dependent  |  
| `--block-size`  | KV cache block size (tokens)  | 16  |  
| `--kv-cache-dtype`  | KV cache precision (e.g. fp8)  | auto  |  
| `--quantization`  | Weight quantization method  | none  |  
|  `--host` / `--port`  | Bind address / port  | 0.0.0.0 / 8000  |  
The current V1 engine (default since vLLM v0.8.0) is a core rewrite delivering up to 1.7x higher throughput than V0, with FlashAttention 3, piecewise CUDA graphs, and near-zero-overhead prefix caching (under 1% throughput drop even at a 0% cache-hit rate, so it is on by default).
### NVIDIA NIM: the enterprise-packaged option
NVIDIA NIM (NVIDIA Inference Microservices) packages a model, an optimized inference engine, and an OpenAI-compatible API server into a single prebuilt Docker container that runs on NVIDIA GPUs anywhere. It auto-selects among TensorRT-LLM, vLLM, and SGLang backends and applies performance-tuned settings; the NIM LLM 2.0 line moved to a "one container, one backend" philosophy built on vLLM for predictable behavior. The default serving port is 8000, with native OpenAI endpoints plus a /metrics observability endpoint.
Prerequisites for the latest NIM LLM (2026): NVIDIA driver 580+ with CUDA 13.0+ (older NIMs accept CUDA 12.1+), Docker ≥ 19.03, and the NVIDIA Container Toolkit; the CUDA Toolkit does not need to be on the host, only the driver. A typical single-node run:
Single-node NIM container`docker run --runtime=nvidia --gpus all --shm-size=16GB   -v ~/.cache/nim:/opt/nim/.cache -u $(id -u)   -p 8000:8000 <nim-llm-container>`
#### Table 9 — NIM offering tiers  
| Tier  | Purpose  | Notable attributes  |  
| --- | --- | --- |  
| **NIM Day 0**  | Rapid access to newly released models  | Earliest availability, less hardening  |  
| **NIM Turbo**  | Validated performance  | Performance-optimized, validated profiles  |  
| **NIM Certified**  | Enterprise production  | CVE patching, OSRB open-source review compliance, AI Enterprise support  |  
#### Table 10 — NIM licensing / access tiers  
| Tier  | Cost  | Limits / terms  |  
| --- | --- | --- |  
| **Developer Program (free)**  | $0  | Up to 2 nodes / 16 GPUs; 1,000 inference credits at signup (up to 5,000 on request); research/dev/test only  |  
| **AI Enterprise 90-day eval**  | $0 for 90 days  | Free evaluation license for production validation  |  
| **AI Enterprise (production)**  | ~$4,500 per GPU/year or ~$1 per GPU/hour (cloud)  | Per-GPU pricing (not per-NIM); same price regardless of GPU size; includes support + Certified NIMs  |  
The AI Enterprise list price (~$4,500/GPU/year) is a starting figure subject to volume and term discounts — confirm with NVIDIA sales.
### vLLM vs NIM vs the rest of the field
The real tradeoff: vLLM gives maximum flexibility, zero license cost, and the fastest access to new open models, at the price of you owning integration, hardening, and support. NIM gives a turnkey container with vendor SLAs, proactive security patching, and validated performance profiles, at the price of NVIDIA AI Enterprise licensing and tighter version coupling. Raw throughput between the top GPU engines is narrow — within roughly 15% — and flips by workload.
#### Table 11 — On-Prem LLM Serving Engine Comparison (2026)  
| Engine  | Core Tech  | OpenAI-Compatible  | Quantization  | Throughput Tier  | Ease of Setup  | Enterprise Support  | Best-Fit Use Case  |  
| --- | --- | --- | --- | --- | --- | --- | --- |  
| **vLLM**  | PagedAttention + continuous batching  | Yes  | GPTQ, AWQ, FP8  | Highest (100+ QPS)  | Moderate  | Community / commercial via vendors  | General-purpose production multi-user GPU serving  |  
| **NVIDIA NIM**  | Prebuilt optimized containers  | Yes  | FP8 + TRT-LLM quant  | High  | Easy (turnkey)  | Yes — NVIDIA AI Enterprise (SLAs, security patches)  | Enterprises needing vendor support, stability, security SLAs  |  
| **TensorRT-LLM**  | Compiled CUDA kernels + KV reuse  | Yes (via Triton/serve)  | FP8, paged+quantized KV  | Highest latency-optimized (NVIDIA-only)  | Hard (long compile)  | Via NVIDIA AI Enterprise  | Latency-sensitive, high-volume, NVIDIA-standardized fleets  |  
| **SGLang**  | RadixAttention (radix-tree KV reuse)  | Yes  | FP8, AWQ  | Very high on shared-context  | Moderate  | Community  | Agents, RAG, structured generation, high prefix reuse  |  
| **Hugging Face TGI v3**  | Chunked prefill + prefix caching  | Yes  | GPTQ, AWQ, EETQ  | High  | Moderate  | Community (upstream in maintenance mode 2026)  | HF-ecosystem teams, long chat histories  |  
| **Ollama**  | Wraps llama.cpp; auto model mgmt  | Yes  | GGUF (Q2–Q8)  | Medium (10–50 QPS)  | Easiest (one command)  | Community  | Local dev, prototyping  |  
| **llama.cpp**  | C/C++ GGUF runtime  | Yes (server mode)  | GGUF (Q2–Q8)  | Low-medium (5–30 QPS)  | Easy (binary + GGUF)  | Community  | CPU-only servers, edge, embedded  |  
#### Table 12 — Single H100 SXM5 80GB Benchmark, Llama-3.3-70B-Instruct FP8 (~512 in / ~256 out)  
| Metric  | Concurrency  | vLLM v0.18.0  | TensorRT-LLM v1.2.0  | SGLang v0.5.9  |  
| --- | --- | --- | --- | --- |  
| **Throughput (output tok/s)**  | 1 req  | 120  | 130  | 125  |  
| **Throughput (output tok/s)**  | 10 req  | 650  | 710  | 680  |  
| **Throughput (output tok/s)**  | 50 req  | 1,850  | 2,100  | 1,920  |  
| **Throughput (output tok/s)**  | 100 req  | 2,400  | 2,780  | 2,460  |  
| **TTFT p50 (ms)**  | 100 req  | 740  | 680  | 710  |  
| **TTFT p95 (ms)**  | 100 req  | 1,450  | 1,280  | 1,380  |  
| **Peak VRAM @100 req (GB)**  | 100 req  | 78  | 79  | 78  |  
| **Cold start**  | first load  | ~62 s  | ~28 min (compile)  | ~58 s  |  
The decisive operational figure is the cold start: TensorRT-LLM's ~28-minute first-time engine compile (subsequent reloads ~90s) makes it painful for rapid model iteration, whereas vLLM and SGLang start in about a minute. A common, sound pattern is to develop and prototype on Ollama or llama.cpp, then serve production on vLLM or NIM. For the broader tool landscape, see [Best Local AI Tools for Enterprise](https://iternal.ai/best-local-ai-tools-enterprise).
### vLLM vs Ollama: Which Serving Stack for On-Prem?
Use vLLM for any on-prem deployment that serves more than one person at a time, and Ollama for a single workstation or a prototype. Ollama wraps llama.cpp and optimizes for one-command setup and GGUF quantization on modest hardware, holding roughly 10–50 QPS. vLLM adds PagedAttention and continuous batching, which keep KV-cache waste under 4% and sustain 100+ QPS on the same GPU — what multi-user chat, RAG, and agent traffic actually need.
#### vLLM vs Ollama at a glance  
| Decision factor  | vLLM  | Ollama  |  
| --- | --- | --- |  
| Best fit  | Multi-user production serving on GPUs you own  | One workstation, prototypes, single-user development  |  
| Concurrency  | 100+ QPS with continuous batching  | ~10–50 QPS  |  
| KV cache  | PagedAttention holds waste under 4%  | llama.cpp allocation; no paged KV cache  |  
| Quantization  | GPTQ, AWQ, FP8  | GGUF (Q2–Q8)  |  
| Multi-GPU  | Tensor and pipeline parallelism  | Layer offload across cards; no tensor parallelism  |  
| Setup effort  | Moderate — engine arguments and capacity planning  | Easiest — one command  |  
Both expose an OpenAI-compatible `/v1` endpoint, so the usual path is to prototype on Ollama and promote the same application code to vLLM or NVIDIA NIM in production — base_url changes, application code does not. If the target is a laptop or a single developer box rather than a server, start with [how to run an LLM locally](https://iternal.ai/how-to-run-llm-locally).
Recommended Reading
### The AI Strategy Blueprint
The executive playbook for aligning AI strategy with infrastructure decisions — covering model selection, deployment architecture, security, and the ROI frameworks behind on-premise and edge AI investments.
5.0 on Amazon
$24.95
[ Get it on Amazon](https://amzn.to/45Q6Xv8)
Infrastructure Chapters
Deployment Playbooks
Security Architecture
ROI Frameworks
##  Step 5: Understand Throughput and Latency (Tokens/sec, TTFT, ITL)
Four metrics define serving performance:
  * **Throughput** — total output tokens/sec across all concurrent requests.
  * **TTFT (Time To First Token)** — latency from request to first token, dominated by prefill of the input prompt.
  * **ITL (Inter-Token Latency)** , a.k.a. TPOT — time between successive output tokens during decode. Per-request decode speed = 1000 / ITL tokens/sec.
  * **Goodput** — throughput that meets your SLOs.


The mechanism that explains everything: **prefill is compute-bound, decode is memory-bandwidth-bound**. Aggregate system throughput and per-request latency move in opposite directions as concurrency rises — continuous batching keeps the GPU busy and lifts total tokens/sec, but each individual request's ITL grows because the GPU time-slices decode across more sequences.
Concrete anchors: a single H100 running Llama 3.1 8B in vLLM peaks around **12,500 tokens/sec aggregate** , with sub-80ms TTFT at low concurrency and ITL of ~11–21ms. For 70B, a single H200 reaches **> 3,800 tok/s/GPU at FP8** (up to 6.7x faster than A100), and 8x H100 in MLPerf delivered 24,525 tok/s total (~3,066 per GPU). The H100-vs-A100 gap widens sharply with concurrency: at 16 concurrent requests the H100 produced the first token roughly 16x faster than the A100.
#### Table 13 — Batch size / concurrency effect on throughput (Llama, H200 FP8, TP=1)  
| Model  | Batch size  | Input/Output tokens  | Throughput (tok/s)  | Takeaway  |  
| --- | --- | --- | --- | --- |  
| **Llama-13B**  | 1024  | 128/128  | 11,819  | Large batch maximizes aggregate throughput  |  
| **Llama-13B**  | 128  | 128/2048  | 4,750  | Long output lowers per-batch tok/s  |  
| **Llama-70B**  | 512  | 128/128  | 3,014  | Peak 70B aggregate at large batch  |  
| **Llama-70B**  | 64  | 2048/128  | 341  | Long input (prefill) crushes throughput  |  
| **Llama-70B**  | 32  | 2048/128  | 303  | Smaller batch + long prompt = lowest tok/s  |  
Long prompts collapse throughput because prefill cost dominates — note how 70B falls from 3,014 tok/s (128 input) to ~303 tok/s (2,048 input).
#### Table 14 — Latency SLA targets (MLPerf Inference v5.1, Llama 3.1 8B scenarios)  
| Scenario  | TTFT limit  | TPOT / ITL limit  | Approx reading speed  |  
| --- | --- | --- | --- |  
| **Server**  | ≤ 2 s  | ≤ 100 ms  | ~480 words/min  |  
| **Interactive**  | ≤ 0.5 s  | ≤ 30 ms  | ~1,600 words/min  |  
| **Practical interactive bar (vLLM/H100, up to 70B)**  | < 200 ms  | < 30 ms (8B ITL ~11–21 ms observed)  | Fluid streaming  |  
Pin Your Engine Version
Always pin the engine version when citing numbers: vLLM v0.6.0 alone delivered 2.7x higher throughput and 5x lower TPOT on Llama 8B versus v0.5.3.
##  Step 6: Scale Across GPUs — Tensor, Pipeline, and Expert Parallelism
When a model exceeds one GPU, you shard it. There are three primary strategies, and matching them to your interconnect is what separates a fast cluster from a slow one.
**Tensor parallelism (TP)** shards each layer's weight matrices across GPUs (the Megatron column-parallel to row-parallel pattern), producing exactly two all-reduce collectives per transformer layer in the forward pass. Llama-3-70B's 80 layers means 160 all-reduce synchronization points per forward pass — so TP is bandwidth-bound and effectively requires NVLink/NVSwitch. On 4x L40 without NVLink, communication can exceed 50% of prefill cost.
**Pipeline parallelism (PP)** splits the model by layers across stages and only passes activations at stage boundaries, so it tolerates slower inter-node links (InfiniBand or even Ethernet) far better than TP. **Expert parallelism (EP)** shards MoE experts across GPUs, using all-to-all dispatch/combine; it pairs with data-parallel attention for large MoE models like DeepSeek-V3/R1.
The vLLM decision rule is clean: TP inside a node, PP across nodes, with tensor_parallel_size = GPUs per node and pipeline_parallel_size = number of nodes. For 2 nodes x 8 GPUs: `--tensor-parallel-size 8 --pipeline-parallel-size 2`. The critical exception: **if GPUs lack NVLink (e.g. L40S) or the GPU count does not evenly divide the model, use pipeline parallelism instead of tensor parallelism**.
#### Table 15 — vLLM Parallelism Strategy Selection  
| Scenario  | Recommended config  | Example flags  |  
| --- | --- | --- |  
| **Model fits on 1 GPU**  | Single GPU, no distribution  | (none)  |  
| **Single node, multiple GPUs, NVLink present**  | Tensor parallel = GPU count  | `--tensor-parallel-size 4`  |  
| **Multi-node, multiple GPUs**  | TP = GPUs per node, PP = number of nodes  | `--tensor-parallel-size 8 --pipeline-parallel-size 2`  |  
| **Single node, no NVLink (e.g. L40S) or uneven split**  | TP=1, PP = GPU count  | `--tensor-parallel-size 1 --pipeline-parallel-size 8`  |  
| **Large MoE (DeepSeek-V3/R1, Mixtral)**  | DP attention + EP experts  | `--tensor-parallel-size 1 --data-parallel-size 8 --enable-expert-parallel`  |  
#### Table 16 — NVLink / NVSwitch bandwidth by GPU generation  
| GPU / Generation  | NVLink gen  | Per-GPU bandwidth (bidirectional)  |  
| --- | --- | --- |  
| **A100 (Ampere)**  | NVLink 3  | 600 GB/s  |  
| **H100 (Hopper)**  | NVLink 4  | 900 GB/s  |  
| **Blackwell (B200/GB200)**  | NVLink 5  | 1,800 GB/s  |  
| **Rubin (announced)**  | NVLink 6  | 3,600 GB/s  |  
Interconnect Notes
NVLink 4 is more than 14x the bandwidth of a PCIe Gen4 x16 bus; an 8-GPU 20GB all-reduce takes ~22ms with NVSwitch versus ~150ms without (~7x). Multi-node TP needs InfiniBand/RoCE with ≥100 Gbps and GPUDirect RDMA; verify it is engaged by checking NCCL logs for `[send] via NET/IB/GDRDMA` (good) versus `[send] via NET/Socket` (slow fallback). Container requirements for TP: run with `--ipc=host --shm-size=16G -v /dev/shm:/dev/shm`; on Kubernetes mount a /dev/shm emptyDir and grant IPC_LOCK — a missing /dev/shm is a common cause of hangs and OOMKilled pods. Single-node multi-GPU uses native multiprocessing; multi-node currently requires Ray.
##  Step 7: Quantize for Memory and Throughput (FP8 / INT8 / INT4 / FP4)
Quantization is the highest-leverage lever for fitting a model on fewer GPUs. The precision ladder runs FP32 to FP16/BF16 to FP8 to INT8 to INT4/FP4, halving memory roughly at each step beyond FP16.
The accuracy results are encouraging. Red Hat/Neural Magic's study spanning over 500,000 evaluations on the Llama-3.1 family found FP8 (W8A8-FP) **effectively lossless across all model scales** , INT8 (W8A8-INT) showing a surprisingly low 1–3% degradation per task, and even INT4 weight-only (W4A16) "more competitive than expected, rivaling 8-bit." The reason FP8 is near-lossless while INT8 needs calibration: FP8's exponential value spacing handles outlier activations gracefully, whereas INT8's uniform spacing needs SmoothQuant-style calibration.
The Deployment Rule
Use **W4A16 (INT4 weight-only)** for latency-bound, low-batch, synchronous serving where weight-loading dominates; use **W8A8 (FP8 preferred)** for high-throughput continuous batching where you are compute-bound. On Intel Xeon, AMX-INT8 is the high-throughput CPU path — see [Option B](https://iternal.ai/how-to-deploy-llm-on-premise#cpu-xeon-airgapai-edge).
#### Table 17 — Quantization format comparison (vs FP16/BF16 baseline)  
| Format  | Bits (W/A)  | Memory vs FP16  | Accuracy vs BF16  | Throughput note  | Best for  |  
| --- | --- | --- | --- | --- | --- |  
| **FP16 / BF16**  | 16 / 16  | 1x (baseline)  | Baseline  | Baseline  | Max accuracy, fine-tune  |  
| **FP8 W8A8 (E4M3)**  | 8 / 8  | ~2x smaller  | Effectively lossless (all scales)  | ~33% faster tok/s on H100  | High-throughput continuous batching  |  
| **INT8 W8A8 (SmoothQuant)**  | 8 / 8  | ~2x smaller  | 1–3% drop per task  | Strong on Ampere/Turing (no FP8 HW)  | High-throughput on pre-Ada GPUs  |  
| **INT4 W4A16 (AWQ)**  | 4 / 16  | ~4x smaller  | Competitive, rivals 8-bit  | Marlin kernel ~741 tok/s (~10.9x vs no-Marlin)  | Latency / low-batch sync serving  |  
| **INT4 W4A16 (GPTQ)**  | 4 / 16  | ~4x smaller  | Slightly below AWQ  | Marlin-accelerated on Ampere+  | Latency / low-batch sync serving  |  
| **GGUF Q4_K_M (llama.cpp)**  | ~4.5 / mixed  | ~4x smaller  | ~6.74 ppl vs 6.56 BF16  | CPU/mixed  | CPU / Apple Silicon / edge  |  
| **bitsandbytes NF4 / INT8**  | 4 or 8 / 16  | ~4x / ~2x  | NF4 ~6.66 ppl  | On-the-fly (no prequant)  | Experimentation, QLoRA  |  
| **NVFP4 (Blackwell)**  | 4 / 4  | ~4x smaller  | Near-FP8 with calibration  | ~2x math throughput vs FP8  | Blackwell high-throughput serving  |  
AWQ (activation-aware) slightly edges GPTQ (Hessian-based) on perplexity, and the Marlin kernel makes both fast on Ampere+. Hardware support is the practical constraint:
#### Table 18 — Engine x GPU-architecture support matrix (2026, version-sensitive)  
| Format  | Ampere SM8.0/8.6  | Ada SM8.9  | Hopper SM9.0  | Blackwell SM100/103/120  | vLLM  | TensorRT-LLM  |  
| --- | --- | --- | --- | --- | --- | --- |  
| **FP8 W8A8**  | No  | Yes  | Yes  | Yes  | Yes (Ada/Hopper+)  | Yes  |  
| **INT8 W8A8**  | Yes  | Yes  | Yes  | No (CC≥10.0 unsupported in vLLM)  | Yes  | Yes (SmoothQuant)  |  
| **INT4 W4A16 AWQ**  | Yes  | Yes  | Yes  | Yes  | Yes (AutoAWQ + Marlin)  | Yes  |  
| **INT4 W4A16 GPTQ**  | Yes  | Yes  | Yes  | Yes  | Yes (GPTQModel + Marlin)  | Yes  |  
| **NVFP4 / MXFP4**  | No  | No  | No  | Yes  | Yes (NVIDIA ModelOpt)  | Yes (Blackwell only)  |  
| **GGUF**  | Yes  | Yes  | Yes  | Yes  | Yes  | No (llama.cpp)  |  
| **FP8 KV cache**  | Yes  | Yes  | Yes  | Yes  | Yes  | Yes  |  
Two Gotchas
FP8 needs Ada/Hopper or newer (not Ampere), and INT8 W8A8 is currently unsupported in vLLM on Blackwell (compute capability ≥ 10.0) — use FP8 there instead.
Free Download
### Get Chapter 1 Free + AI Academy Access
Download the first chapter of The AI Strategy Blueprint and get instant access to our AI Academy — covering infrastructure planning, model selection, and on-premise deployment frameworks.
##  Step 8: Manage the KV Cache and Long Context
The KV cache caches Keys and Values from prior tokens to avoid O(n²) recomputation each decode step, and at long context it is the primary memory bottleneck, frequently exceeding weight memory. Per-token cost = 2 × num_layers × num_kv_heads × head_dim × bytes_per_element, scaled by tokens × batch.
#### Table 19 — KV Cache per-token cost and VRAM (Llama 3.1 70B, single sequence)  
| Precision  | Bytes/element  | Per-token KV cost  | KV cache at 32K ctx  | KV cache at 128K ctx  |  
| --- | --- | --- | --- | --- |  
| **BF16/FP16**  | 2 bytes  | ~0.31 MB (310 KB)  | ~10 GB  | ~42.9 GB  |  
| **FP8 (e4m3/e5m2)**  | 1 byte  | ~0.155 MB  | ~5 GB  | ~21.5 GB  |  
| **NVFP4 / 4-bit (Blackwell)**  | 0.5 bytes  | ~0.078 MB  | ~2.7 GB  | ~10.7 GB  |  
Every additional 1,000 tokens of context adds ~310 MB for a 70B-class model at BF16, and FP8 KV-cache quantization halves the footprint. Two techniques tame this:
  * **GQA/MQA** shrink the cache by the ratio of query heads to KV heads. Llama 3.1 70B's 8 KV heads (versus 64 query heads) give an 8x reduction — which can mean 2 GPUs instead of 4 at 128K context.
  * **Automatic prefix caching** (vLLM --enable-prefix-caching, on by default) hashes complete 16-token KV blocks (SHA-256) and reuses them across requests sharing a prefix — system prompts, tool definitions, few-shot examples — with LRU eviction and a cache_salt for multi-tenant isolation.


#### Table 20 — PagedAttention vs prior serving systems (vLLM paper)  
| Metric  | Prior systems  | vLLM PagedAttention  |  
| --- | --- | --- |  
| **KV-cache memory waste**  | 60%–80% (fragmentation + over-reservation)  | under 4% (last partial block only)  |  
| **Throughput vs HF Transformers**  | 1x  | 14x–24x  |  
| **Throughput vs TGI (1 completion)**  | 1x  | 2.2x–2.5x  |  
Practical Sizing
Budget KV cache as (GPU memory − weights − activations) / per-token cost to derive the maximum total tokens (the sum of all concurrent sequence lengths) the GPU can hold. If kv_cache_usage_perc approaches 100% in production, new requests queue and risk preemption — lower --max-num-seqs or enable --kv-cache-dtype fp8, which roughly doubles effective capacity.
##  Step 9: Tune Batching and Speculative Decoding
**Continuous (in-flight) batching** is the single biggest throughput lever: rather than padding to a fixed batch, the engine evicts finished requests and admits queued ones every step. The vLLM V1 scheduler can mix prefill and decode in the same step, prioritizing decode then filling the remaining token budget with (chunked) prefill.
**Chunked prefill** splits a long prompt's prefill across steps so one long request cannot stall all others — the technique introduced by Sarathi-Serve. The tuning tradeoff: a smaller max_num_batched_tokens (e.g. 2048) gives better ITL because fewer prefill tokens stall decodes; a higher value gives better TTFT and throughput.
**Speculative decoding** drafts k tokens cheaply, then verifies them in one target-model forward pass, accepting the longest valid prefix. vLLM supports n-gram/prompt-lookup, draft-model, EAGLE/EAGLE-3, and Medusa/MTP.
#### Table 21 — Speculative decoding methods in vLLM  
| Method  | Proposer  | Key config  | Notes  |  
| --- | --- | --- | --- |  
| **n-gram / prompt-lookup**  | Match trailing n-gram, propose following k tokens  | method=ngram, num_speculative_tokens, prompt_lookup_max  | Best when output echoes input (RAG, code edit)  |  
| **Draft model**  | Small separate LLM  | model=<draft>, num_speculative_tokens=5  | Needs a quality draft sharing the target vocab  |  
| **EAGLE / EAGLE-3**  | Lightweight MLP replacing target transformer stack  | method=eagle3, draft_tensor_parallel_size=1  | Top performer; draft runs without TP even if target uses TP  |  
| **Medusa / MTP**  | Auxiliary heads predict next k tokens  | draft_tensor_parallel_size=1  | No separate draft model  |  
Load-Dependence Caveat
EAGLE-3 delivers up to 2.5x speedup at low load — on MT-Bench with Llama-3.1-8B, 4.40x at acceptance length 6.13 tokens — but the gain erodes under high concurrency: SGLang measured EAGLE-3 at 1.81x throughput at batch 2 but only 1.38x at batch 64. vLLM's own docs warn that speculative decoding "is not yet optimized and does not usually yield inter-token latency reductions for all prompt datasets" — under high concurrency, rejected draft tokens waste target FLOPs that would otherwise serve other requests. Enable it for low-concurrency, latency-sensitive workloads; benchmark before enabling it under heavy load.
##  Capacity Planning: A Worked Sizing Example, End to End
This is where the math becomes a purchase order. The flow: define demand to compute memory to compute per-token timing to convert to GPU count to apply SLO-driven utilization ceilings and headroom.
Memory formulas (VMware/Lenovo)Weights: M = P × Z × 1.2 (P in billions, Z = bytes/param, 1.2 = ~20% overhead) Llama 3.3 70B FP16 = 70 × 2 × 1.2 = 168 GB KV/token: 2 × precision_bytes × num_layers × num_kv_heads × head_dim Llama-3-8B FP16 = ~128 KB/token ; Llama-3-70B = ~0.000305 GiB/token Max concurrent = max_kv_cache_tokens / max_context_window where max_kv_cache_tokens = (GPU_mem − weights) / kv_per_token
Latency formulasPrefill (compute-bound): weights_per_GPU × 2 FLOP / GPU_TFLOPS Decode (bandwidth-bound): weights_per_GPU × 2 bytes / GPU_bandwidth Example (Llama-3-8B on L40, 181 TFLOPS, 864 GB/s): prefill 0.088 ms/token ; decode 18.5 ms/token 4,000-token prompt + 256-token response = (4000 × 0.088) + (256 × 18.5) ≈ 5.1 seconds
### Worked GPU-count example
Suppose peak demand is **1,000 requests/sec** , average service time **40 ms** , target GPU utilization **70%**. Per-H100 service rate = 0.70 / 0.040 = 17.5 RPS. GPU count = ceil(1000 / 17.5) = **60 H100 instances**. But the SLO sets the utilization ceiling, because P99 TTFT degrades nonlinearly with concurrency:
#### Table 22 — P99 TTFT degradation vs concurrency (70B FP8 on H100 SXM5, 512-token prompts)  
| Concurrent Requests  | P50 TTFT  | P99 TTFT  | P99/P50  |  
| --- | --- | --- | --- |  
| 45ms  | 90ms  | 2.0x  |  
| 52ms  | 160ms  | 3.1x  |  
| 68ms  | 280ms  | 4.1x  |  
| 95ms  | 480ms  | 5.1x  |  
#### Table 23 — SLO target to max GPU utilization ceiling, and resulting fleet (1,000 RPS, 40ms service, H100 spot)  
| TTFT P99 target  | Max GPU utilization  | Instances (ceil)  | Monthly cost (spot)  |  
| --- | --- | --- | --- |  
| **200ms**  | 55%  | 73  | $88,826  |  
| **300ms**  | 63%  | 64  | $77,875  |  
| **400ms**  | 70%  | 60  | $73,008  |  
| **500ms**  | 75%  | 54  | $65,707  |  
The Lesson
A tighter SLO buys you fewer requests per GPU and roughly 15% more cost per tightening step. Scale on the **concurrency trigger** (~24–28 concurrent per H100 for 70B FP8), not on raw utilization — by the time CPU-based autoscaling fires, the queue is already deep. Add peak-to-average headroom on top. For models ≥ 70B, factor in tensor parallelism (TP ≥ 2) and use per-GPU weight count in the latency formulas. To model your own numbers interactively, use the [LLM Pricing Calculator](https://iternal.ai/llm-pricing-calculator); for hardware specifics see the [Hardware Sizing Guide](https://iternal.ai/hardware-sizing-guide).
##  Air-Gapped and Secure Deployment
For classified, defense, and the most sensitive regulated workloads, air-gapping is the deployment model — and it is an **architecture, not a configuration flag**. Every runtime dependency must be pre-staged inside the enclave: a signed model registry, GPU inference workers, a local vector DB with a **local embedding model** , a container-registry mirror, OS/language package mirrors, on-prem observability, and internal PKI. True air-gap means no NAT, no DNS to external hostnames, no public CA chain, and no route by which a packet can leave. The single most common way an "air-gapped" RAG stack secretly breaks the gap is calling a remote embedding API — the embedding model must run inside the enclave alongside the LLM — see [how to choose an embedding model](https://iternal.ai/embedding-models) for the selection criteria that apply once the model has to ship inside the boundary. For a fuller treatment, see [Best AI for Air-Gapped Environments](https://iternal.ai/best-ai-air-gapped-environments).
The workflow is two-phase. On a connected staging host, pre-download models and containers; verify SHA-256/signatures; physically transfer across the gap; then run isolated. For NVIDIA NIM, the connected host sets NGC_API_KEY and LOCAL_NIM_CACHE, runs `download-to-cache -p <profile-hash>`, copies the cache to AIR_GAP_NIM_CACHE, and the disconnected host mounts it at /opt/nim/.cache and runs the container **without** NGC_API_KEY or HF_TOKEN — omitting the keys prevents any model-download, registry, or telemetry call. For open-source vLLM, use huggingface-cli/snapshot_download on the connected host, serve a **local directory path** (not a hub repo ID), and set HF_HUB_OFFLINE=1 so the tokenizer resolves locally.
#### Table 24 — Telemetry / phone-home kill switches by component (air-gap hardening)  
| Component  | Variable / mechanism  | Effect  |  
| --- | --- | --- |  
| **Hugging Face Hub**  | `HF_HUB_OFFLINE=1`  | No HTTP to the Hub; cache-only; skips cached-file version check  |  
| **Transformers**  | `TRANSFORMERS_OFFLINE=1`  | Loads strictly from local cache  |  
| **HF ecosystem**  |  `HF_HUB_DISABLE_TELEMETRY=1` (or `DO_NOT_TRACK=1`)  | Disables usage telemetry across transformers/datasets/diffusers/gradio  |  
| **HF auth**  | `HF_HUB_DISABLE_IMPLICIT_TOKEN=1`  | Stops auto-attaching token to read requests  |  
| **vLLM**  |  `VLLM_NO_USAGE_STATS=1` / `VLLM_DO_NOT_TRACK=1` / `~/.config/vllm/do_not_track`  | Disables default-on anonymous usage stats  |  
| **NVIDIA NIM (air-gap run)**  | Omit `NGC_API_KEY` and `HF_TOKEN`  | Runs from mounted cache with no registry/Hub callouts  |  
Mirror every container image through a frozen local registry (Harbor, or oc-mirror on OpenShift) and version-pin scanned PyPI/npm/apt snapshots. Updates arrive as signed tarballs (manifests + images + Helm charts) physically walked across the gap, integrity- and signature-verified before staging, on a slow cadence — monthly (healthcare) to quarterly (defense). Use the customer's internal PKI with mTLS between gateway and workers; there is no route to a public CA.
#### Table 25 — Compliance frameworks for on-prem / air-gapped LLM  
| Framework  | Key figure / control set  | Air-gap relevance  |  
| --- | --- | --- |  
| **FedRAMP High**  | 421 controls  | Eliminates boundary-defense & external-monitoring control categories (no boundary)  |  
| **DoD Impact Levels**  | IL4 = CUI, IL5 = CUI+mission-critical, IL6 = classified to SECRET  | Air-gap required/expected at IL5–IL6  |  
| **CMMC 2.0 Level 2**  | NIST SP 800-171 (110 controls)  | Eases MP, SC, AC families; avoids 32 CFR Part 170 FedRAMP-Moderate cloud rule on-prem  |  
| **CMMC 2.0 Level 3**  | NIST 800-171 + 800-172 enhanced  | Highest CUI tier; air-gap simplifies enhanced SC/AC  |  
| **HIPAA**  | Not required; BAA + "minimum necessary"  | Air-gap + HITRUST CSF attestation common for PHI  |  
| **SCIF / classified**  | Encrypted drives, cleared installers, cross-domain media updates  | No external connectivity; physical update channel only  |  
Strategic Point
Air-gapping does not merely satisfy controls, it **eliminates entire control categories** — there is no network boundary to defend or continuously monitor. Pair the deployment with a written [AI Governance Framework](https://iternal.ai/ai-governance-framework) so the model-update, access, and audit processes are documented before an assessor asks. AirgapAI Edge (see [Option B](https://iternal.ai/how-to-deploy-llm-on-premise#cpu-xeon-airgapai-edge)) runs fully offline on Intel Xeon CPUs from a local model IR — a natural fit for SCIF and classified enclaves where GPU power and cooling are impractical.
##  Total Cost of Ownership: On-Prem vs Cloud
The GPU sticker is only about 35% of five-year TCO — power, cooling, networking, redundancy, and staff make up the rest.
#### Table 26 — On-Prem GPU Server CAPEX (full system, Lenovo Press 2026, priced Jan 15 2026)  
| Config  | GPU Setup  | GPU Memory  | Price (USD)  |  
| --- | --- | --- | --- |  
| 8x H100  | 80 GB  | $250,141.80  |  
| 8x H200  | 141 GB  | $277,897.75  |  
| 8x B200  | 192 GB  | $338,495.75  |  
| 8x B300  | 288 GB  | $461,567.50  |  
| 4x L40S  | 48 GB  | $52,390.50  |  
An 8x H100 server pulls ~10 kW at full load (~$10,500/yr electricity at $0.12/kWh), with cooling adding ~30%. Staff is typically the single largest line item, exceeding hardware depreciation over three years. To run these categories against a CPU-based build, use the [Xeon server ROI calculator (3-year AI inference TCO)](https://iternal.ai/calculators/xeon-server-roi-calculator):
#### Table 27 — 3-Year TCO of One 8x H100 SXM5 Server (Spheron cost model, 2026)  
| Cost Category  | Annual  | 3-Year Total  |  
| --- | --- | --- |  
| **Hardware depreciation**  | $116,000–150,000  | $350,000–450,000  |  
| **Power (~10 kW @ $0.12/kWh)**  | $10,500–10,700  | $31,500–32,100  |  
| **Cooling (~30% of power)**  | $3,150–3,210  | $9,450–9,630  |  
| **Datacenter / colocation**  | $12,000–24,000  | $36,000–72,000  |  
| **Networking (InfiniBand)**  | ~$10,000  | ~$30,000  |  
| **Storage (NVMe, object)**  | $5,000–8,000  | $15,000–24,000  |  
| **Staff (0.5 FTE engineer)**  | $75,000–100,000  | $225,000–300,000  |  
| **Maintenance / spares**  | $5,000–10,000  | $15,000–30,000  |  
| **TOTAL**  | ~$236,650–315,910  | ~$711,950–947,730  |  
#### Table 28 — Break-Even Time, On-Prem 8x H100 vs Azure (Lenovo 2026)  
| Cloud Pricing Tier  | Rate ($/hr, 8-GPU server)  | On-Prem Break-Even  |  
| --- | --- | --- |  
| **Azure on-demand**  | $98.32  | ~3.7 months  |  
| **Azure 1-year reserved**  | $62.92  | ~6 months  |  
| **Azure 5-year reserved**  | $39.32  | ~10.4 months  |  
#### Table 29 — Per-Token Cost: On-Prem vs Cloud/API (Lenovo 2026)  
| Model / Config  | Throughput  | On-Prem $/1M tokens  | Cloud/API $/1M tokens  | On-Prem advantage  |  
| --- | --- | --- | --- | --- |  
| **Llama-70B, 8x H100**  | 30,576 tok/s  | $0.11  | $0.89 (Azure H100)  | 8x  |  
| **Llama-3.1-405B, 8x B300**  | 1,360 tok/s  | $4.74  | $29.09 (AWS)  | 84% cheaper  |  
| **GPT-5-mini-equivalent open model, 8x H100**  | n/a  | $0.11  | ~$2.00 (GPT-5 mini API)  | ~18x  |  
Two Counterweights
First, independent academic analysis (arXiv 2509.18101) finds break-even is sharply model-size-dependent: small ~30B models pay back in 0.3–3 months, medium ~70B in 2.3–34 months, and large 235B+ models in 4.3–69.3 months. Second, against ultra-cheap specialist clouds (e.g. ~$2.90/hr H100), cloud can beat on-prem even at 100% utilization — and real production teams run only 40–65% utilization, well below the 80–90% optimistic vendor models assume. The break-even that pays back in 3.7 months at 90% utilization may never pay back at 40%. Model your own utilization realistically; see [Edge AI vs Cloud Economics](https://iternal.ai/edge-ai-vs-cloud-economics) for the full crossover analysis and the [LLM Pricing Calculator](https://iternal.ai/llm-pricing-calculator) to plug in your token volume.
##  Production Operations: Observability, Autoscaling, and Go-Live
Four pillars carry an on-prem LLM from "it runs" to "it runs reliably": observability, autoscaling, health/lifecycle, and go-live readiness. Where the deployment serves agents rather than single-turn requests, these serving metrics sit underneath [agent observability and tracing](https://iternal.ai/ai-agent-evaluation#observability), which follows a request across every tool call and step.
Observability
vLLM exposes Prometheus metrics at /metrics. Monitor the golden signals: latency histograms (time_to_first_token, inter_token_latency, e2e_request_latency, request_queue_time), saturation gauges (num_requests_running, num_requests_waiting, kv_cache_usage_perc), and throughput/health counters (generation_tokens, num_preemptions, prefix-cache hit rate). Triage rule: if num_requests_waiting > 0 consistently, requests are queuing and TTFT is rising — add capacity; if num_requests_waiting == 0 but TTFT is still high, the bottleneck is prefill compute, not scaling. Healthy steady state is zero requests waiting with KV cache below 90%.
Autoscaling
Standard Kubernetes HPA on CPU/memory is wrong for GPU inference — the GPU saturates while CPU stays low. Use KEDA scaling on queue depth (num_requests_waiting) per replica via a Prometheus trigger. A reference ScaledObject: threshold ~5 pending, minReplicaCount 1, maxReplicaCount 3, pollingInterval 15s, cooldownPeriod 360s. Model-weight load is the dominant pod-startup cost; a shared weights cache on an NFS-backed PVC cuts startup "from minutes to seconds," making reactive autoscaling feasible.
Health & Lifecycle
vLLM's /health confirms only that the engine process is alive — it does not verify the GPU can run a forward pass. Set Kubernetes readinessProbe (initialDelaySeconds 120) and livenessProbe (initialDelaySeconds 180) with high initial delays because model load takes minutes, and drain active streams gracefully on deploy. Version model weights, tokenizer, prompt templates, and inference config together with commit hashes; ship via stable deployment IDs with shadow traffic and canary rollout that auto-rolls-back on TTFT/TPS regression.
Go-Live
Before launch, run a saturation sweep with GuideLLM or genai-perf across realistic input/output lengths to find the knee and set P95/P99 SLOs from observed data. Token-aware rate limits, client retries with jitter, and idempotency keys round out the production posture. The full pre-launch checklist follows below.
##  Printable On-Prem LLM Requirements Checklist
Model & Licensing
  * Model license cleared by legal (Apache 2.0 / MIT preferred; verify Llama 700M-MAU clause; review Gemma terms)
  * Model selected by use case (chat / RAG / coding / reasoning / edge)
  * MoE vs dense decision recorded (VRAM bills total params, compute bills active)


Sizing
  * Weights VRAM computed (params × bytes/param × 1.2)
  * KV cache budgeted at target context AND concurrency (GQA/MLA-aware)
  * Quantization chosen (W4A16 for latency/low-batch; W8A8/FP8 for throughput)
  * Max concurrent requests per GPU derived from leftover VRAM


Hardware
  * GPU model selected on capacity AND bandwidth (not just VRAM) — or Intel Xeon + AirgapAI Edge for no-GPU CPU inference
  * Precision support verified (FP8 needs Ada/Hopper+; FP4 needs Blackwell; AMX-INT8 on Xeon)
  * NVLink present if using tensor parallelism; else plan pipeline parallelism
  * InfiniBand/RoCE ≥100 Gbps + GPUDirect RDMA for multi-node TP


Serving Stack
  * Engine chosen (vLLM / NIM / SGLang / TensorRT-LLM) with rationale
  * OpenAI-compatible endpoint + API-key auth configured
  * --gpu-memory-utilization, --max-model-len, --max-num-seqs tuned
  * Continuous batching + prefix caching confirmed on; speculative decoding benchmarked under real load
  * --ipc=host --shm-size=16G / /dev/shm + IPC_LOCK set for multi-GPU


Capacity & SLO
  * Demand model built (concurrent users, RPS, in/out tokens)
  * GPU count derived two ways (tokens/sec and queueing)
  * SLO-driven utilization ceiling applied; scale trigger = concurrency, not CPU
  * Peak-to-average headroom added


Air-Gap & Security (if applicable)
  * All dependencies pre-staged inside enclave (incl. local embedding model)
  * Two-phase download/verify/transfer workflow documented; SHA-256 verified
  * Telemetry kill switches set (HF_HUB_OFFLINE, VLLM_NO_USAGE_STATS, NIM keys omitted)
  * Private registry mirror frozen; packages version-pinned and scanned
  * Internal PKI + mTLS; on-prem observability; signed-bundle update cadence defined
  * Compliance mapping documented (FedRAMP / CMMC / HIPAA / IL level)


Production Ops
  * Prometheus /metrics scraped; Grafana dashboards on golden signals
  * Alerts on P95 TTFT regression, queue depth, KV%, preemptions, error rate
  * KEDA autoscaling on queue depth validated under load
  * Liveness + GPU-level readiness probes; graceful drain on deploy
  * Load tested with GuideLLM/genai-perf; P95/P99 SLOs set from data
  * Token-aware rate limits; client retries with jitter; idempotency keys
  * Model artifacts versioned together; canary + auto-rollback; DR runbooks drilled


## Put the Sizing Math to Work
An on-prem deployment is one chapter of a defensible enterprise AI program. Build the strategy behind the infrastructure, then turn this guide into a tailored deployment roadmap.
Build the Strategy Behind the Infrastructure
Get the full playbook in the AI Strategy Blueprint — the executive guide to deploying AI with the right infrastructure, security, and ROI built in from day one. $24.95 on Amazon, rated 5 stars.
[ Get the AI Strategy Blueprint](https://iternal.ai/ai-strategy-blueprint)
Turn This Guide Into Your Roadmap
Use the AI Blueprint Builder to generate a tailored on-premise deployment plan mapped to your models, hardware, concurrency targets, and compliance requirements.
[ Launch the AI Blueprint Builder](https://iternal.ai/ai-blueprint-builder)
##  Frequently Asked Questions
How much VRAM do I need to run a 70B model on-premise?
A 70B model needs ~140 GB at FP16 (70B x 2 bytes), ~70 GB at FP8/INT8, and ~35-46 GB at INT4 -- before KV cache and activations. In practice, FP16 requires 2x H100 80GB (tensor-parallel) or a single H200 141GB, while INT4 fits comfortably on one 80GB GPU. For production at realistic context and concurrency, NVIDIA NIM's supported minimum for a 70B at BF16 is 4x 80GB GPUs, because 2x 80GB leaves too little headroom for the KV cache.
vLLM or NVIDIA NIM -- which should we use?
Use vLLM when you want maximum flexibility, no license cost, and the fastest access to new open models, and you have the platform team to own integration and support. Use NIM when you need a turnkey, vendor-supported container with SLAs, proactive CVE patching, and validated performance -- and you are licensing NVIDIA AI Enterprise (~$4,500/GPU/year). Raw throughput between the top engines is within ~15% and flips by workload, so the decision is about support model and operational fit, not speed.
Can I deploy an LLM fully air-gapped with no internet at all?
Yes. Air-gapping is an architecture: you pre-stage the model, container, embedding model, and all dependencies on a connected host, verify signatures, physically transfer them across the gap, and run isolated with telemetry disabled. For NIM, run the container without NGC_API_KEY/HF_TOKEN; for vLLM, serve a local model path with HF_HUB_OFFLINE=1. The most common mistake is leaving a remote embedding-API call in a RAG pipeline, which silently breaks the air gap.
How do I calculate the KV cache size for long context?
KV cache per token = 2 x num_layers x num_kv_heads x head_dim x bytes_per_element, multiplied by tokens x batch size. For Llama 3.1 70B at BF16 that is ~0.31 MB/token, so 128K context for one stream is ~42.9 GB; FP8 halves it to ~21.5 GB. Because it scales linearly with both context length and concurrency, the KV cache often exceeds weight memory at long context -- budget it explicitly as (GPU memory - weights - activations) / per-token cost.
Which quantization should I use -- FP8, INT8, or INT4?
FP8 is effectively lossless and is the production standard for high-throughput continuous batching on Hopper/Blackwell; INT8 shows only 1-3% degradation and is the right choice on Ampere GPUs that lack FP8; INT4 weight-only (AWQ/GPTQ) is competitive and best for latency-bound, low-batch serving where weight loading dominates. Rule of thumb: W4A16 for latency/cost-efficiency, W8A8 (FP8 preferred) for throughput.
Does on-premise actually cost less than cloud APIs?
For sustained, high-volume, predictable inference, yes -- self-hosting an open model runs roughly 8-18x cheaper per token over a multi-year horizon, and an 8x H100 cluster can break even versus Azure on-demand in about 3.7 months. But the GPU sticker is only ~35% of true TCO (staff is often the largest line item), break-even is sharply model-size-dependent, and at the 40-65% utilization real teams actually achieve, cheap specialist clouds can win even so. Model your real utilization before committing capital.
What GPU bandwidth do I need, and why does it matter more than TFLOPS for inference?
Decode -- the token-by-token generation phase -- is memory-bandwidth-bound, not compute-bound, because every new token streams all model weights from HBM once per forward pass. That is why the H200, with compute identical to the H100 but 43% more bandwidth (4.8 vs 3.35 TB/s), generates tokens ~43% faster at small batch sizes. Prioritize HBM bandwidth and capacity over raw TFLOPS for inference workloads.
When do I need multiple GPUs or multiple nodes, and how do I connect them?
Use a single GPU if the model fits; tensor parallelism (TP = GPU count) within a node when it does not, provided NVLink is present; and TP-per-node plus pipeline parallelism (PP = node count) across nodes. If GPUs lack NVLink (e.g. L40S) or do not evenly divide the model, prefer pipeline parallelism. Multi-node TP needs InfiniBand/RoCE ≥100 Gbps with GPUDirect RDMA -- verify with NCCL logs showing NET/IB/GDRDMA rather than NET/Socket.
How do I autoscale an on-prem LLM service?
Do not use CPU-based Kubernetes HPA -- the GPU saturates while CPU stays idle, so the queue is already deep by the time it triggers. Use KEDA scaling on queue depth (vllm:num_requests_waiting) per replica via Prometheus, with a threshold around 5 pending requests and a cooldown of ~360s. Mitigate cold starts with a shared NFS-backed PVC weights cache, which drops pod startup from minutes to seconds.
Can I self-host an open-source LLM instead of using OpenAI or Anthropic APIs?
Yes. Open-weight models (Llama, Qwen, Mistral, DeepSeek, gpt-oss) now match hosted frontier APIs on most enterprise tasks, and you can self-host them on hardware you own with vLLM or NVIDIA NIM behind an OpenAI-compatible API -- so application code barely changes. The trade-off is operational: you own capacity planning, GPU procurement, patching, and uptime. Self-hosting wins on cost at sustained high volume (roughly 8-18x cheaper per token) and is often mandatory for data-sovereignty; hosted APIs win for spiky, low-volume, or frontier-closed-model workloads.
##  Sources & References
#### Serving Engines (vLLM, NIM, TensorRT-LLM, SGLang)
  * [vLLM V1: A Major Upgrade to vLLM's Core Architecture](https://blog.vllm.ai/2025/01/27/v1-alpha-release.html)
  * [vLLM: OpenAI-Compatible / Online Serving](https://docs.vllm.ai/en/stable/serving/openai_compatible_server/)
  * [vLLM: Easy, Fast, and Cheap LLM Serving with PagedAttention](https://vllm.ai/blog/2023-06-20-vllm)
  * [Overview -- NVIDIA NIM for Large Language Models](https://docs.nvidia.com/nim/large-language-models/latest/about-nim-llm/overview.html)
  * [Pricing -- NVIDIA AI Enterprise Licensing Guide](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/pricing.html)
  * [vLLM vs TensorRT-LLM vs SGLang: H100 Benchmarks (2026)](https://www.spheron.network/blog/vllm-vs-tensorrt-llm-vs-sglang-benchmarks/)


#### Sizing, VRAM & KV Cache
  * [NVIDIA: Mastering LLM Techniques -- Inference Optimization](https://developer.nvidia.com/blog/mastering-llm-techniques-inference-optimization/)
  * [VMware: LLM Inference Sizing and Performance Guidance](https://blogs.vmware.com/cloud-foundation/2024/09/25/llm-inference-sizing-and-performance-guidance/)
  * [Lenovo Press: LLM Sizing Guide (LP2130)](https://lenovopress.lenovo.com/lp2130-lenovo-llm-sizing-guide)
  * [Modal: How much VRAM do I need for LLM inference?](https://modal.com/blog/how-much-vram-need-inference)
  * [Kwon et al.: Efficient Memory Management for LLM Serving with PagedAttention (arXiv 2309.06180)](https://arxiv.org/pdf/2309.06180)
  * [DeepSeek-AI: DeepSeek-V3 Technical Report (arXiv 2412.19437)](https://arxiv.org/pdf/2412.19437)


#### GPUs, Quantization & Parallelism
  * [Spheron: NVIDIA H100 vs H200 -- Specs, FP8 Throughput & Cloud Pricing (2026)](https://www.spheron.network/blog/nvidia-h100-vs-h200/)
  * [Spheron: GPU Requirements Cheat Sheet 2026](https://www.spheron.network/blog/gpu-requirements-cheat-sheet-2026/)
  * [Red Hat / Neural Magic: 'Give Me BF16 or Give Me Death'? (arXiv 2411.02355)](https://arxiv.org/abs/2411.02355)
  * [vLLM: Parallelism and Scaling](https://docs.vllm.ai/en/stable/serving/parallelism_scaling/)
  * [Flash Communication: Reducing Tensor Parallelization Bottleneck (arXiv 2412.04964)](https://arxiv.org/html/2412.04964v1)


#### Air-Gap, TCO & Operations
  * [NVIDIA: Air-Gap Deployment for NIM LLMs](https://docs.nvidia.com/nim/large-language-models/latest/deploy-air-gap.html)
  * [TrueFoundry: Air-Gapped AI -- Deploying Enterprise LLMs in Regulated Industries](https://www.truefoundry.com/blog/air-gapped-ai-deploying-enterprise-llms-in-highly-regulated-industries)
  * [Hugging Face Hub: Environment Variables Reference](https://huggingface.co/docs/huggingface_hub/package_reference/environment_variables)
  * [Spheron: On-Premise vs GPU Cloud -- 2026 Cost and Break-Even Analysis](https://www.spheron.network/blog/llm-inference-on-premise-vs-cloud/)
  * [Lenovo Press: On-Premise vs Cloud Generative AI TCO (2026, LP2368)](https://lenovopress.lenovo.com/lp2368-on-premise-vs-cloud-generative-ai-total-cost-of-ownership-2026-edition)
  * [A Cost-Benefit Analysis of On-Premise LLM Deployment (arXiv 2509.18101)](https://arxiv.org/html/2509.18101v3)
  * [vLLM: Production Metrics](https://docs.vllm.ai/en/stable/usage/metrics/)
  * [vLLM: Autoscaling with KEDA (production-stack docs)](https://docs.vllm.ai/projects/production-stack/en/latest/use_cases/autoscaling-keda.html)


##  Related Resources
For the job-by-job version, see [Running AI on the Infrastructure You Already Own](https://iternal.ai/jobs/deploy-local-ai).
[ Guide Private LLM: The Enterprise Guide The hub above this how-to: what a private LLM is, how it compares to open-source and cloud, and the full private/local/air-gapped family.](https://iternal.ai/private-llm) [ Research On-Premises Hardware Sizing Guide GPU platform specs, per-model VRAM, concurrency sizing, and TCO across H100, H200, and Gaudi 3.](https://iternal.ai/hardware-sizing-guide) [ Research Best AI for Air-Gapped Environments Deploying disconnected, secure AI with telemetry kill switches and signed-bundle update workflows.](https://iternal.ai/best-ai-air-gapped-environments) [ Guide LLM Selection Guide A decision tree across every open-weight family by license, architecture, and use case.](https://iternal.ai/llm-selection-guide) [ Guide On-Premise AI Chat Platform options, hardware requirements, and total cost for chat assistants that run entirely inside your boundary.](https://iternal.ai/on-premise-ai-chat) [ Calculator LLM Pricing Calculator Model your token volume, GPU count, and break-even against cloud API pricing interactively.](https://iternal.ai/llm-pricing-calculator) [ Analysis Edge AI vs Cloud Economics The full crossover math for when owned CPU/GPU infrastructure beats per-token cloud pricing.](https://iternal.ai/edge-ai-vs-cloud-economics) [ Product AirgapAI Run LLM inference entirely on Intel Xeon CPUs -- no GPU, fully offline, in your own boundary.](https://iternal.ai/airgapai)
Before You Go — Free Chapter
## 95% of AI investments fail. Get the chapter that puts you in the 5%.
A free chapter of the AI Strategy Blueprint — the framework Fortune 500 leaders use to take AI from stalled pilot to production.
  * **The Fortune 500 framework** for AI strategy that actually ships
  * **Why most AI pilots never reach production** — and how to avoid it
  * **Instant PDF download** — read it in 15 minutes


No spam. Unsubscribe anytime. Delivered in seconds
[Prefer the whole playbook? Learn more about the book ](https://iternal.ai/ai-strategy-blueprint)
Questions about AirgapAI or Blockify? Ask me.
Iternal AI Assistant Online 
Hi! I'm Iternal's AI assistant. How can I help you today?
What is AirgapAI? What's the ROI? Book a demo
Message limit reached. Resets in --
50 messages remaining today
Email captured! Check your inbox for a demo booking link.
### Continue the conversation
You're getting great insights! Share your email to keep chatting and receive helpful resources.
Maybe later
AI Blueprint Builder
AI Blueprint Builder
#### Build your Enterprise AI Blueprint
Use the AI Blueprint Builder to score and rank your AI use cases across value, feasibility, risk, and readiness — before you commit budget.
3,000,000 Tokens of Custom Prompting
17 Autonomous Agentic Workflows
0-100% Clarity in 60 Minutes
[ Get Access to the Builder ](https://iternal.ai/landing/ai-blueprint-builder?utm_source=site-promo&utm_medium=popover)
Free · Link by email · ~60 min to a full blueprint
Enterprise Decision Framework
