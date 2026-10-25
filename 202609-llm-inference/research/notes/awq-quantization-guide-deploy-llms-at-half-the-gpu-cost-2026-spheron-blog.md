---
title: 'AWQ Quantization Guide: Deploy LLMs at Half the GPU Cost (2026) | Spheron
  Blog'
id: awq-quantization-guide-deploy-llms-at-half-the-gpu-cost-2026-spheron-blog
tags:
- internal-llm-inference-stacks-7f0c90
- awq
- quantization
- llm-compressor
- vendor-marketing
created: '2026-10-09T22:56:11.265297Z'
updated: '2026-10-10T14:52:17.105169Z'
source: https://www.spheron.network/blog/awq-quantization-guide-llm-deployment/
source_domain: www.spheron.network
fetched_at: '2026-10-09T22:56:11.244710Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Vendor engineering blog from Spheron (a GPU-rental/cloud provider), dated
  content current to April 2026. Despite the hardware-rental framing, it contains
  a specific, checkable claim about the quantization TOOLCHAIN that is directly on-topic:
  AutoAWQ (casper-hansen/AutoAWQ), the original reference implementation for running
  AWQ quantization, "was archived in May 2025 and is no longer maintained," and the
  post names vllm-project/llm-compressor as "the recommended replacement" for ongoing
  support -- i.e., the vLLM project''s own compressor has effectively become the maintained
  successor toolchain for producing AWQ (and other) quantized checkpoints, not just
  a runtime consumer of them. The post frames AWQ as "the default INT4 format for
  production inference" as of 2026, with major model families shipping pre-quantized
  AWQ checkpoints on Hugging Face and vLLM, SGLang, and TensorRT-LLM all shipping
  optimized AWQ kernels (vLLM: `--quantization awq` flag auto-loads AWQ weights; SGLang
  auto-detects AWQ from the HF config; TensorRT-LLM supports AWQ via NVIDIA''s `modelopt`
  toolkit with a more involved build pipeline but reportedly higher peak throughput).


  Software-side comparative claims (self-reported/aggregated, not independently verified
  by this source): AWQ INT4 gives "1-3% quality degradation on most tasks" vs BF16,
  slightly better than GPTQ INT4''s "2-5% degradation" at the same bit-width, attributed
  to AWQ''s activation-aware calibration vs. GPTQ''s layer-wise Hessian-based reconstruction;
  a worked accuracy table cites Llama 3.3 70B MMLU dropping ~85% to ~83% (-2%) and
  HumanEval ~72% to ~70% (-3%) under AWQ INT4, and Mistral 7B WikiText-2 perplexity
  rising from ~5.25 to ~5.40 (+2.9%) -- explicitly flagged by the source itself as
  "community-sourced estimates," not an official/MLPerf benchmark. The piece explicitly
  recommends validating AWQ quality on reasoning/math-heavy tasks before production
  use, since those degrade more (3-5%) than instruction-following or summarization
  (1-3%). It also distinguishes AWQ from bitsandbytes NF4 (used for QLoRA fine-tuning,
  not calibration-based, not optimized for inference quality) and from vendor-trained
  QAT checkpoints (e.g., a cited Gemma 4 QAT release), which it says outperform post-hoc
  AWQ at the same bit-width because "error compensation is trained into the weights
  rather than patched on top." Flags common operational pitfalls: forgetting the `--quantization
  awq` flag silently falls back to BF16; mismatched `q_group_size` (standard is 128)
  produces garbage output; domain-mismatched calibration data (128-512 samples) degrades
  quality on the target distribution; AWQ''s throughput benefit shrinks or reverses
  on compute-bound (large-batch, small-model) workloads due to dequantization overhead.'
utility_score: 12.0
---

If you're paying for a full-precision 70B model on an H100 when an A100 running AWQ INT4 will do, you're overpaying by roughly 2x. AWQ (Activation-Aware Weight Quantization) cuts model VRAM requirements by about 50% with 1-3% quality degradation on most tasks. That's often the difference between needing two GPUs and needing one. For a full breakdown of how VRAM requirements scale with model size and precision, see [GPU memory requirements for LLMs](https://www.spheron.network/blog/gpu-memory-requirements-llm/).
As of 2026, AWQ is the default INT4 format for production inference. Major model families ship pre-quantized AWQ checkpoints on Hugging Face. vLLM, SGLang, and TensorRT-LLM all include optimized AWQ kernels. If you're serving on H100, A100, L40S, or RTX 4090 hardware and haven't evaluated AWQ, this guide walks through the full process: how it works, how to quantize a 70B model, expected memory and throughput gains, and production deployment on cloud GPUs. For per-hour rates on AWQ-friendly hardware, browse the [GPU rental catalog](https://www.spheron.network/gpu-rental/).
## Why AWQ Became the Default INT4 Method
INT4 quantization has existed for years. The early methods (naive INT4, round-to-nearest) worked on small models but produced significant quality loss on 7B+ parameter models. GPTQ (2022) improved this by using layer-wise second-order weight optimization, which produced better INT4 quality than naive quantization. But it still left headroom on the table.
AWQ (2023, from MIT Han Lab) solved the remaining quality problem with a different insight: not all weights are equally important. A small fraction of weights, maybe 1%, have outsized influence on model outputs. If you can identify and protect those weights during quantization, the rest can be aggressively compressed with minimal quality impact.
The result: AWQ INT4 models consistently outperform GPTQ at the same bit-width on quality benchmarks. By 2024, major model releases started shipping AWQ checkpoints alongside their standard weights. By 2026, it's the expected format.
The practical outcome for you: AWQ weights are already available on Hugging Face for most major models. You may never need to run quantization yourself. But understanding the process matters when pre-quantized weights don't exist for your model, or when you need to optimize for a specific domain. For framework selection context, see [Ollama vs vLLM: Which Should You Use?](https://www.spheron.network/blog/ollama-vs-vllm/).
## How AWQ Works: Activation-Aware Weight Quantization
The core insight behind AWQ: activation magnitudes tell you which weights matter. During a forward pass, certain weight channels consistently produce high-magnitude activations. These are the salient weights. Perturbing them introduces large errors into downstream computations. The non-salient weights have small activation response; you can quantize them aggressively without much impact.
The quantization process has two steps:
**Step 1: Calibration pass.** Run a small dataset (128-512 samples) through the model and record activation magnitudes for each weight channel. This identifies which channels are salient.
**Step 2: Per-channel scaling before quantization.** Scale the salient weight channels up (by a learned factor) and scale the corresponding activations down by the same factor. The net effect on model outputs is neutral, but now when you quantize to INT4, the salient weights get more precision (because they're spread over a wider range), while non-salient weights are compressed normally.
This is different from GPTQ, which uses a layer-wise second-order (Hessian) approximation to minimize quantization error across all weights. GPTQ is more computationally expensive and doesn't specifically target the most important weights. AWQ's calibration-based approach is both faster and produces better quality.
It's also different from bitsandbytes NF4 (used in QLoRA fine-tuning), which applies a different 4-bit representation without a calibration step. NF4 is good for fine-tuning but AWQ is better for inference quality.
Here's how the formats compare on memory and quality:  
| Format  | Bits  | Bytes/param  | 70B model size  | Quality vs BF16  |  
| --- | --- | --- | --- | --- |  
| BF16  | 16  | 2 bytes  | ~140 GB  | Reference  |  
| FP8  | 8  | 1 byte  | ~70 GB  | Negligible difference  |  
| GPTQ INT4  | 4  | 0.5 bytes  | ~35 GB  | 2-5% degradation  |  
| AWQ INT4  | 4  | 0.5 bytes  | ~35 GB  | 1-3% degradation  |  
| FP4 (NVFP4)  | 4  | 0.5 bytes  | ~35 GB  | Blackwell only; see [FP4 guide](https://www.spheron.network/blog/fp4-quantization-blackwell-gpu-cost/)  |  
Both GPTQ and AWQ get you to ~35 GB for a 70B model. The difference is ~1-2% better benchmark scores with AWQ, and better throughput because AWQ's weight layout is more amenable to fast INT4 matrix multiplication kernels in vLLM and SGLang. For the measured tokens/sec, perplexity retention, and cost-per-million-token numbers across GPTQ, AWQ, and GGUF on the same GPU, see the [GPTQ vs AWQ vs GGUF benchmark comparison](https://www.spheron.network/blog/gptq-vs-awq-vs-gguf-4-bit-quantization-comparison-2026/). For hardware-native 8-bit inference without per-layer calibration, see the [FP8 quantization guide](https://www.spheron.network/blog/fp8-quantization-inference-performance-hardware-explained/).
For models where the original vendor has released official QAT checkpoints, such as Gemma 4, QAT at 4-bit offers better quality than AWQ at the same bit-width because error compensation is trained into the weights rather than patched on top. See the [Gemma 4 QAT deployment guide](https://www.spheron.network/blog/deploy-gemma-4-qat-gpu-cloud/) for a direct comparison of QAT versus AWQ quality at w4a16.
## AWQ vs GPTQ vs GGUF vs FP4: When to Use Each  
| Method  | Format  | GPU support  | Framework  | Best for  |  
| --- | --- | --- | --- | --- |  
| AWQ  | INT4  | All CUDA GPUs  | vLLM, SGLang, TRT-LLM  | Production GPU server inference  |  
| GPTQ  | INT4  | All CUDA GPUs  | vLLM, AutoGPTQ  | When AWQ weights unavailable  |  
| GGUF  | Variable  | CPU + NVIDIA + Apple  | llama.cpp, Ollama  | Local/edge/CPU inference  |  
| FP8  | 8-bit float  | H100, H200, Blackwell  | vLLM, SGLang  | Server inference, near-lossless  |  
| FP4  | 4-bit float  | Blackwell only  | TRT-LLM, vLLM  | Maximum throughput on B200/B300  |  
| MXFP4 / NVFP4  | 4-bit float  | Blackwell, AMD MI355  | TRT-LLM, vLLM  | Hardware-native FP4 on Blackwell; higher throughput than AWQ on B200  |  
The choice is mostly determined by where you're running:
  * **Running on H100, A100, L40S, or RTX 4090** : AWQ is your INT4 option. GPTQ works too but AWQ is better when available. If you need to go beyond quantization, [SparseGPT and Wanda pruning](https://www.spheron.network/blog/llm-pruning-sparsegpt-wanda-gpu-cloud/) can remove 50% of weights before you quantize, cutting VRAM further without additional precision loss.
  * **Running on consumer hardware or CPU** : GGUF via llama.cpp handles mixed CPU/GPU inference and is the right format for Ollama.
  * **Running on Blackwell (B200, RTX 5090)** : FP8 first (near-lossless, good framework support). FP4 if you need maximum throughput and can validate quality. See the [FP4 quantization guide](https://www.spheron.network/blog/fp4-quantization-blackwell-gpu-cost/) for Blackwell-specific setup. For the MXFP4 microscaling workflow specifically (MR-GPTQ calibration, TensorRT Model Optimizer), see the [MXFP4 quantization guide](https://www.spheron.network/blog/mxfp4-microscaling-quantization-gpu-cloud/).
  * **Framework comparison** : For a throughput breakdown across vLLM, TensorRT-LLM, and SGLang, see [inference framework benchmarks](https://www.spheron.network/blog/vllm-vs-tensorrt-llm-vs-sglang-benchmarks/).


For workflows that need FP8 or FP4 alongside INT4, [NVIDIA TensorRT Model Optimizer (ModelOpt)](https://www.spheron.network/blog/tensorrt-model-optimizer-modelopt-quantization-guide/) provides a unified API across all precision formats.
## Step-by-Step: Quantizing a 70B Model with AWQ
Most of the time you won't need to run quantization yourself. TheBloke and the official model authors publish AWQ checkpoints for nearly every major model on Hugging Face. Search for `-AWQ` suffix variants. If you find a pre-quantized checkpoint, skip to the deployment section.
If you need to quantize your own model, here's the full process using AutoAWQ.
**Hardware requirements:**
  * 7B-13B models: RTX 4090 (24 GB VRAM) is sufficient
  * 70B models: A100 80G or H100 required (model needs to fit in VRAM during quantization)
  * Estimated time: 30-90 minutes for a 70B model on a single A100 80G


**Installation:**
bash

```
pip install autoawq autoawq-kernels
```

> **Note:** AutoAWQ (casper-hansen/AutoAWQ) was archived in May 2025 and is no longer maintained. For ongoing support, see [vllm-project/llm-compressor](https://github.com/vllm-project/llm-compressor) as the recommended replacement. The code below still works with the last released version.
**Quantization script** (tested with AutoAWQ 0.2.x; check the [AutoAWQ GitHub](https://github.com/casper-hansen/AutoAWQ) for the last released API):
python

```
from awq import AutoAWQForCausalLM
from transformers import AutoTokenizer

model_path = "meta-llama/Llama-3.3-70B-Instruct"
quant_path = "Llama-3.3-70B-Instruct-AWQ"

quant_config = {
    "zero_point": True,
    "q_group_size": 128,
    "w_bit": 4,
    "version": "GEMM"
}

model = AutoAWQForCausalLM.from_pretrained(model_path, low_cpu_mem_usage=True)
tokenizer = AutoTokenizer.from_pretrained(model_path)

model.quantize(tokenizer, quant_config=quant_config)

model.save_quantized(quant_path)
tokenizer.save_pretrained(quant_path)
```

Llama 3.x models do not require custom tokenizer code, so `trust_remote_code=True` is omitted here. Only pass `trust_remote_code=True` when a model's Hugging Face repository explicitly requires it (stated in the model card). The flag allows the model repository to execute arbitrary Python code on your machine during loading, which carries real risk when used with third-party or unvetted model repos.
The `q_group_size=128` is the standard setting and what most published AWQ weights use. Mismatching this value when loading a pre-quantized checkpoint will produce garbage output, so always check the model card for the config used during quantization.
After quantization completes, you'll have a directory with AWQ weight files that can be loaded directly by vLLM, SGLang, or TensorRT-LLM.
## GPU Memory Savings: Before and After AWQ
These figures include KV cache overhead at moderate batch sizes (batch 8-16). Actual VRAM usage varies with sequence length and concurrency.  
| Model  | BF16 VRAM  | AWQ INT4 VRAM  | Savings  | Fits single GPU  |  
| --- | --- | --- | --- | --- |  
| 7B  | ~14 GB  | ~4.5 GB  | 68%  | RTX 4090 (24 GB)  |  
| 13B  | ~26 GB  | ~8.5 GB  | 67%  | RTX 4090 (24 GB)  |  
| 14B (e.g. Phi-5)  | ~28 GB  | ~9 GB  | 68%  | L40S (48 GB) or RTX 4090 with AWQ  |  
| 34B  | ~68 GB  | ~21 GB  | 69%  | L40S (48 GB)  |  
| 70B  | ~140 GB  | ~40 GB  | 71%  | A100 80G (80 GB)  |  
| 405B  | ~810 GB  | ~243 GB  | 70%  | 4x A100 80G  |  
The 14B row shows why AWQ is particularly useful for models like Phi-5: the base FP16 model at ~28GB exceeds a single RTX 4090 (24GB), but AWQ drops it to ~9GB, making RTX 4090 a viable serving platform. For a complete Phi-5 deployment guide including AWQ setup and vLLM configuration, see the [Phi-5 GPU cloud deployment guide](https://www.spheron.network/blog/deploy-phi-5-gpu-cloud/).
The 70B row is where AWQ has the most practical impact: you go from needing two A100s (or one H100, tightly packed) at BF16, to fitting comfortably on a single A100 80G with room for KV cache. For full VRAM budget methodology including KV cache sizing at different sequence lengths, see the [GPU memory requirements for LLMs guide](https://www.spheron.network/blog/gpu-memory-requirements-llm/).
## Inference Speed: Throughput and Latency with AWQ
AWQ improves throughput by reducing memory bandwidth pressure. When inference is memory-bandwidth-bound (small batch sizes, large models), moving from BF16 to INT4 means the GPU can load model weights 4x faster from HBM. This translates directly to more tokens per second.
The gains are largest when you're memory-bound. If you're already compute-saturated (high batch sizes, small models), the throughput improvement is smaller.  
| GPU  | Model  | Precision  | Tokens/sec  | Relative  |  
| --- | --- | --- | --- | --- |  
| A100 80G  | Llama 3.1 70B  | BF16 (2x A100)  | ~1,200  | 1x  |  
| A100 80G  | Llama 3.1 70B  | AWQ INT4  | ~1,800  | ~1.5x  |  
| RTX 4090  | Llama 3.1 8B  | BF16  | ~1,500  | 1x  |  
| RTX 4090  | Llama 3.1 8B  | AWQ INT4  | ~2,400  | ~1.6x  |  
> **Estimated values:** Throughput figures for A100 80G and RTX 4090 are community-sourced estimates based on vLLM benchmark reports and Hugging Face model card benchmarks. No official MLPerf single-GPU AWQ result exists for these configurations as of April 2026. Actual throughput depends on batch size, sequence length, vLLM version, and driver configuration. Run your own benchmarks before production capacity planning.
The throughput gain from AWQ is on top of the GPU consolidation benefit. Going from 2x A100 BF16 to 1x A100 AWQ reduces your hourly cost by 50% while also improving tokens/sec. That's the compound effect that makes AWQ valuable.
## Quality Retention: Perplexity and Benchmark Accuracy
AWQ's calibration step preserves more quality than GPTQ at the same bit-width. The gaps are small but measurable on standard benchmarks:  
| Model  | Benchmark  | BF16  | AWQ INT4  | Delta  |  
| --- | --- | --- | --- | --- |  
| Llama 3.3 70B  | MMLU  | ~85%  | ~83%  | -2%  |  
| Llama 3.3 70B  | HumanEval  | ~72%  | ~70%  | -3%  |  
| Mistral 7B  | Perplexity (WikiText-2)  | ~5.25  | ~5.40  | +2.9%  |  
A few caveats on reading these numbers: perplexity is a proxy metric. It correlates with quality but doesn't capture task-specific behavior well. For production decisions, always benchmark on your actual use case. Instruction following, summarization, and RAG tasks are much less sensitive to AWQ quality loss than multi-step reasoning or math. For reasoning model-specific quantization tradeoffs, see the [reasoning model inference cost guide](https://www.spheron.network/blog/reasoning-model-inference-cost-gpu-optimization/).
The practical rule: if your task involves complex reasoning chains, validate AWQ quality explicitly. For everything else, AWQ is likely fine.
## Deploying AWQ Models with vLLM on GPU Cloud
vLLM handles AWQ natively. The `--quantization awq` flag tells vLLM to load AWQ weights and use the optimized INT4 kernel path. No other changes needed.
**Using a pre-quantized Hugging Face model:**
bash

```
docker run --gpus all \
  -v ~/.cache/huggingface:/root/.cache/huggingface \
  -p 8000:8000 \
  vllm/vllm-openai:latest \
  --model TheBloke/Llama-3.3-70B-Instruct-AWQ \
  --quantization awq \
  --dtype auto \
  --gpu-memory-utilization 0.92 \
  --max-model-len 8192
```

**Verify the endpoint is working:**
bash

```
curl http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "TheBloke/Llama-3.3-70B-Instruct-AWQ",
    "messages": [{"role": "user", "content": "Hello"}],
    "max_tokens": 50
  }'
```

Check the model card for your chosen checkpoint before using it in production. TheBloke (AWQ) and bartowski (GGUF) are community-maintained namespaces on Hugging Face; specific model slugs may change. If the model name has changed, search `{model-name}-AWQ` on Hugging Face for the current checkpoint.
For the full production setup including multi-GPU deployment, FP8, load balancing, and monitoring, see the [vLLM production deployment guide](https://www.spheron.network/blog/vllm-production-deployment-2026/).
## Deploying AWQ with SGLang and TensorRT-LLM
**SGLang** supports AWQ directly:
bash

```
python -m sglang.launch_server \
  --model-path TheBloke/Llama-3.3-70B-Instruct-AWQ \
  --port 30000
```

> **Note:** SGLang auto-detects AWQ quantization from the model's Hugging Face config, so `--quantization awq` is not needed for pre-quantized checkpoints. The flag is optional.
**TensorRT-LLM** supports AWQ via the `modelopt` toolkit. The build steps are more complex and version-dependent, so reference the [NVIDIA TensorRT-LLM AWQ documentation](https://nvidia.github.io/TensorRT-LLM/) directly rather than following a specific command sequence here. TRT-LLM gives higher peak throughput than vLLM or SGLang for production deployment on NVIDIA hardware, at the cost of a more involved build pipeline.
LMDeploy's TurboMind engine has native AWQ kernel paths that avoid Python dispatch overhead, making it another strong option for AWQ INT4 deployment. See the [LMDeploy TurboMind deployment guide](https://www.spheron.network/blog/deploy-lmdeploy-gpu-cloud-turbomind-inference/).
For the full SGLang deployment walkthrough, see the [SGLang production deployment guide](https://www.spheron.network/blog/sglang-production-deployment-guide/). For throughput and latency comparisons across all three frameworks, see the [vLLM vs TensorRT-LLM vs SGLang benchmarks](https://www.spheron.network/blog/vllm-vs-tensorrt-llm-vs-sglang-benchmarks/).
## Cost Analysis: AWQ vs Full Precision on Spheron
The cost formula:

```
Cost per 1M tokens = ($/hr) ÷ (tokens/sec × 3,600) × 1,000,000
```

Using Spheron pricing recorded on 07 Apr 2026 and estimated throughput figures:  
| Config  | GPU  | $/hr  | Tokens/sec  | Cost/1M tokens  |  
| --- | --- | --- | --- | --- |  
| 70B BF16, 2x GPU  | 2x A100 80G SXM4  | $3.28  | ~1,200  | ~$0.757  |  
| 70B AWQ INT4, 1x GPU  | 1x A100 80G SXM4  | $1.64  | ~1,800  | ~$0.253  |  
| 70B BF16  | H100 SXM5  | $2.98  | ~1,600  | ~$0.517  |  
| 70B AWQ INT4  | H100 SXM5  | $2.98  | ~2,400  | ~$0.345  |  
| 7B AWQ INT4  | A100 80G PCIe  | $1.04  | ~3,800  | ~$0.076  |  
The 70B AWQ on a single A100 column is the clearest case: $0.253/M tokens vs $0.757/M for 2x A100 BF16, a 3x cost reduction. The throughput improvement compounds the GPU count reduction. For broader GPU cost strategies beyond quantization, see the [GPU cost optimization playbook](https://www.spheron.network/blog/gpu-cost-optimization-playbook/).
> Pricing fluctuates based on GPU availability. The prices above are based on **07 Apr 2026** and may have changed. Check [current GPU pricing →](https://www.spheron.network/pricing/) for live rates.
Rent the GPU you need: [A100 GPUs →](https://www.spheron.network/gpu-rental/a100/) | [L40S →](https://www.spheron.network/gpu-rental/l40s/) | [RTX 4090 →](https://www.spheron.network/gpu-rental/rtx-4090/)
## Choosing the Right GPU for Quantized Inference  
| Use case  | Recommended GPU  | Reason  |  
| --- | --- | --- |  
| 70B model production serving  | A100 80G  | Fits full quantized 70B in single GPU; high memory bandwidth; cost-effective  |  
| 34B model serving  | L40S 48G  | 48 GB fits quantized 34B; good token throughput; lower cost than A100  |  
| 7B-13B models, high throughput  | RTX 4090  | 24 GB fits quantized 13B; highest tokens/$ for small models  |  
| 7B fine-tune then serve  | RTX 4090  |  [QLoRA fine-tuning](https://www.spheron.network/blog/how-to-fine-tune-llm-2026/) + AWQ inference on same GPU; lowest total cost  |  
| 405B model serving  | 4x A100 80G  | Tensor parallelism across 4 GPUs; AWQ keeps per-GPU load manageable  |  
For the broader GPU selection guide covering FP8, FP4, and full-precision workloads, see [Best GPU for AI Inference 2026](https://www.spheron.network/blog/best-gpu-for-ai-inference-2026/). For a quick-reference cheat sheet, see the [GPU requirements cheat sheet](https://www.spheron.network/blog/gpu-requirements-cheat-sheet-2026/).
## Common Pitfalls and How to Avoid Them
  * **Using dynamic quantization instead of AWQ.** Loading a model and running round-to-nearest INT4 without calibration produces worse quality than AWQ, with no memory benefit over loading a pre-quantized checkpoint. Use AutoAWQ or get pre-calibrated weights from Hugging Face.


  * **Forgetting`--quantization awq` in vLLM.** If you omit this flag, vLLM loads the model as BF16. Your memory savings disappear and throughput drops back to baseline. The flag is required every time you launch the server.


  * **Skipping KV cache in your VRAM budget.** AWQ dramatically reduces weight memory, but the KV cache is separate and scales with batch size and sequence length. At production batch sizes, KV cache can add 10-30 GB on top of model weights. See the [KV cache optimization guide](https://www.spheron.network/blog/kv-cache-optimization-guide/) for sizing formulas and FP8 KV cache compression options.


  * **Using AWQ for reasoning tasks without quality validation.** Chain-of-thought reasoning and multi-step math are more sensitive to INT4 quantization noise than instruction following or summarization. If your use case is reasoning-heavy, benchmark your specific task before deploying AWQ.


  * **Mismatching`q_group_size` settings.** The standard is 128. Pre-quantized weights from Hugging Face are almost always quantized with `q_group_size=128`. If you quantize yourself with a different value and then load with the wrong setting, output is garbage. Always match the quantization config to what the weights expect.


  * **Using a general-purpose calibration dataset for a specialized domain.** AWQ calibrates salient weights using activation patterns from the calibration data. If you're deploying for code generation, calibrate with code samples. If you're deploying for medical text, use medical text. Domain mismatch in calibration data leads to worse quality on the target distribution.


  * **Over-estimating throughput gains on compute-bound workloads.** AWQ's throughput benefits are largest when inference is memory-bandwidth-bound: large models at small batch sizes. If you're running 7B models at batch 256, you're likely compute-bound and the throughput improvement from AWQ will be smaller than the tables above suggest. In some compute-bound cases, dequantization overhead can even make INT4 slower than BF16 outright; see [why INT4 sometimes runs slower than FP16 inference](https://www.spheron.network/blog/myth-quantization-doesn-t-always-make-inference-faster/) for the benchmarks showing where that crossover happens.


> AWQ INT4 quantization is the most practical way to fit larger models on mid-tier GPUs without a meaningful quality tradeoff. A100, L40S, and RTX 4090 instances are available on Spheron. Run the cost math against your current setup.
> [Rent A100 GPUs →](https://www.spheron.network/gpu-rental/a100/) | [L40S GPU pricing →](https://www.spheron.network/gpu-rental/l40s/) | [View all GPU pricing →](https://www.spheron.network/pricing/)
STEPS / 04
## Quick Setup Guide
  1. 01
### Install the AutoAWQ library
Install AutoAWQ and its dependencies: pip install autoawq autoawq-kernels. For GPU acceleration during quantization, a CUDA-capable GPU with at least 24 GB VRAM is required. The library supports CUDA 11.8 and 12.x.
  2. 02
### Prepare your calibration dataset
AWQ requires a calibration dataset of 128-512 representative text samples to identify salient weights. Use a sample from your target domain - code, conversation, technical text - for best quality. The AutoAWQ library includes a default dataset (Pile) if no custom calibration data is provided.
  3. 03
### Run AWQ quantization
Load the model and run quantization using AutoAWQ's quantize() method with w_bit=4, q_group_size=128, and zero_point=True. For a 70B model, quantization takes 30-90 minutes on a single A100 80G. Save the output to a local or Hugging Face repository.
  4. 04
### Deploy the AWQ model with vLLM
Run the quantized model with vLLM by adding --quantization awq to your vllm serve command. vLLM automatically loads AWQ weights and applies optimized INT4 kernels. No additional configuration required beyond the quantization flag.


FAQ / 05
## Frequently Asked Questions
### 01What is AWQ quantization and how does it work?
AWQ (Activation-Aware Weight Quantization) is an INT4 post-training quantization method that identifies and protects the ~1% of model weights that most affect output quality - the salient weights - by analyzing activation magnitudes during calibration. The remaining weights are quantized to 4-bit integers. This produces INT4 models with significantly better quality than naive INT4 quantization, at roughly 50% the GPU memory of a BF16 baseline.
### 02How much GPU memory does AWQ save compared to FP16?
AWQ reduces model weight storage from 2 bytes per parameter (BF16) to approximately 0.5 bytes per parameter (INT4), a 4x reduction in weight memory. A 70B parameter model that requires ~140 GB in BF16 fits in roughly 35-40 GB in AWQ INT4 - within range of a single A100 80G or two RTX 4090s. KV cache and activation memory are separate; the total VRAM reduction in practice is typically 45-55% versus BF16 inference.
### 03Is AWQ better than GPTQ for production LLM inference?
AWQ generally outperforms GPTQ on both quality metrics (lower perplexity, higher task accuracy) and inference throughput, because AWQ's calibration is activation-aware while GPTQ uses a layer-wise second-order approximation. In practice, AWQ models score 1-3% higher on MMLU and HumanEval benchmarks vs GPTQ at the same INT4 bit-width. For vLLM and SGLang deployments, AWQ is the recommended INT4 format; both frameworks include optimized AWQ kernels.
### 04Which GPUs work best for AWQ quantized LLM inference?
AWQ's INT4 weight compression is most valuable on GPUs with limited VRAM. The A100 80G is the best balance of VRAM (80 GB, fitting a quantized 70B without tensor parallelism), memory bandwidth, and cost for production INT4 inference. The L40S (48 GB) fits quantized 34B models comfortably and costs less per hour. The RTX 4090 (24 GB) handles 7B-13B quantized models well and offers the lowest cost per token for small-to-medium models. All three run AWQ with full vLLM support.
### 05Does AWQ hurt output quality versus FP16?
For most production tasks - instruction following, summarization, code generation, conversational AI - AWQ INT4 models show 1-3% degradation on standard benchmarks versus FP16 or BF16 baselines. Complex multi-step reasoning tasks show slightly more degradation (3-5%). Pre-calibrated AWQ weights from Hugging Face (TheBloke and official model authors) are well-optimized; always prefer these over running your own quantization without a calibration dataset.
## Related Reading
[More on Inference Optimization →](https://www.spheron.network/blog/topics/inference-optimization/)
[ Engineering What Is Inference Engineering? The 2026 GPU Cloud Guide Apr 7, 2026 ](https://www.spheron.network/blog/inference-engineering-guide-2026/)[ Engineering Why Your LLM Inference Is Slow (And How to Fix It) Mar 29, 2026 ](https://www.spheron.network/blog/llm-inference-slow/)[ Engineering KV Cache Quantization vLLM Setup: KIVI's INT2 Method (2026) Sep 5, 2026 ](https://www.spheron.network/blog/kv-cache-quantization-with-kivi-int2-setup-guide-2026/)[ Tutorial Multi-Token Prediction on GPU Cloud: Deploy MTP LLMs for 2-3x Faster Inference (2026) Jun 5, 2026 ](https://www.spheron.network/blog/multi-token-prediction-mtp-gpu-cloud-deployment-guide/)[ Tutorial Eagle-3 Speculative Decoding on GPU Cloud: 3-4x Faster LLM Inference (2026) May 30, 2026 ](https://www.spheron.network/blog/eagle-3-speculative-decoding-gpu-cloud/)[ Engineering From Prototype to Production: A Complete LLM Deployment Guide Apr 15, 2026 ](https://www.spheron.network/blog/llm-deployment-guide/)
Try It Yourself
### Try It on Real GPUs
The GPUs behind these guides are the ones you can rent here: H100s, H200s, B200s, and more, billed per minute after a 20-minute minimum runtime, with no contracts. Pick one and you are live in under two minutes.
Rent a GPU
Deploy Time
< 2 min
Uptime SLA
99.9%
GPU Models
10+
Starting At
$0.78/hr
We use cookies to measure traffic and improve Spheron. Accept all, reject non-essential, or choose what's on. See our [Privacy Policy](https://www.spheron.network/privacy/).
ManageRejectAccept
