---
title: llm-d/guides/recipes/router/calibration/configuration-matrix.md at main · llm-d/llm-d
  · GitHub
id: llm-dguidesrecipesroutercalibrationconfiguration-matrixmd-at-main-llm-dllm-d-git
tags:
- llmd-kv-cache-routing-c665d5
- locus-default-scoring-configuration
- peakprefillthroughput
- calibration
- github-primary-source
created: '2026-09-14T12:42:08.086370Z'
updated: '2026-09-15T04:55:24.127840Z'
source: https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/configuration-matrix.md
source_domain: github.com
fetched_at: '2026-09-14T12:42:08.066157Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Primary llm-d GitHub source: the configuration-matrix.md reference table
  of measured peakPrefillThroughput (tok/s) values by (model, accelerator, engine,
  TP, chunk size) for the prefix-cache-affinity-filter''s saturation gate. Confirms
  the plugin''s shipped default, 15928 tok/s, is the measured value for the reference
  path gpu/vllm: Qwen3-32B on NVIDIA H100 80GB vLLM v0.23.0 TP=2, CHUNK_SIZE=8192.
  Other measured rows: gpt-oss-120B/H100 vLLM TP=1 = 39065 (highest, sparse MoE ~5B
  active params in MXFP4); Qwen3-32B/H100 SGLang TP=2 = 30720 (~1.9x vLLM on identical
  hardware); Qwen3-VL-32B-Instruct/H200 vLLM TP=2 (multimodal) = 15751; TPU v6e Qwen3-32B
  TP=8 = 26290; TPU v7x Qwen3-32B TP=8 = 27336; MetaX C500X DeepSeek-R1-Distill-Llama-70B
  TP=8 = 5468; Rebellions NPU gpt-oss-120B TP=1 (CHUNK_SIZE=512, does not borrow a
  proxy) = 12582; CPU (GCP C3 AMX, bf16) Llama-3.2-3B TP=1, CHUNK_SIZE=2048 = 1970.
  AMD vLLM/SGLang and Intel XPU rows are marked with a dagger as unmeasured proxies
  borrowed from the closest measured path (AMD borrows the matching H100 engine value;
  XPU borrows the CPU value), explicitly flagged for re-measurement via calibrate.sh.
  Separately notes agentic-serving guide ships 16444 for Qwen3-Coder-480B-FP8 on TPU
  v7x TP=8. States the value must be re-measured (not transplanted) whenever TP, chunk
  size, quantization, or --max-model-len changes.'
---

*Suggested by [[llm-dguidesrecipesroutercalibration-at-main-llm-dllm-d-github]] — calibration README links this as the per-(model,accelerator) reference table of measured peakPrefillThroughput values*

[Skip to content](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/configuration-matrix.md#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/configuration-matrix.md) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/configuration-matrix.md) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/configuration-matrix.md) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/configuration-matrix.md).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d) You must be signed in to change notification settings
  * [ Fork 765 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)
  * [ Star  4.5k ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# configuration-matrix.md
Copy path
More file actions
More file actions
## Latest commit
[Hardware support: Rebellions NPU (optimized-baseline) (](https://github.com/llm-d/llm-d/commit/965a5080b354187c9f466fbabe4777954348b3f7)[#2476](https://github.com/llm-d/llm-d/pull/2476)[)](https://github.com/llm-d/llm-d/commit/965a5080b354187c9f466fbabe4777954348b3f7)
Open commit detailssuccess
Sep 12, 2026
[965a508](https://github.com/llm-d/llm-d/commit/965a5080b354187c9f466fbabe4777954348b3f7) · Sep 12, 2026
## History
[History](https://github.com/llm-d/llm-d/commits/main/guides/recipes/router/calibration/configuration-matrix.md)
Open commit details
History
86 lines (75 loc) · 5.81 KB
/
# configuration-matrix.md
Copy path
## File metadata and controls
  * Preview
  * 

86 lines (75 loc) · 5.81 KB
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Outline
Edit and raw actions
# Configuration matrix — `peakPrefillThroughput` by (model, accelerator)
The `prefix-cache-affinity-filter` plugin needs exactly one hardware/model-specific number: **`peakPrefillThroughput`**(tokens/sec). It converts a pod's in-flight token load into a predicted time-to-first-token, which is what gates sticky prefix-affinity routing. See[the calibration README](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/README.md) for how it is measured (`peakPrefillThroughput = CHUNK_SIZE / median(TTFT)`).
The value depends on **model × accelerator × tensor-parallel size × chunk size** (`--max-num-batched-tokens`) — not on the model or the accelerator alone. This page is the reference set for the combinations shipped under `guides/`: **bold** cells are measured; **‡** cells are proxies borrowed from the closest measured path until that hardware can be calibrated (see [Filling a cell](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/configuration-matrix.md#filling-a-cell)).
## Matrix
There is **one row per model-server overlay** shipped by the [optimized-baseline](https://github.com/llm-d/llm-d/blob/main/guides/optimized-baseline) `modelserver/` paths, listing the model that overlay actually serves and its tensor-parallel size. vLLM **and** SGLang are separate rows because the serving engine changes prefill throughput.  
| Overlay path  | Accelerator · engine  | Version  | TP  | Model (as shipped)  |  `peakPrefillThroughput` (tok/s)  |  
| --- | --- | --- | --- | --- | --- |  
| `gpu/vllm`  | NVIDIA H100 80 GB · vLLM  | v0.23.0  | 2  | Qwen3-32B  | **15928**  |  
|  `gpu/vllm` (multimodal-serving)  | NVIDIA H200 141 GB · vLLM  | v0.23.0  | 2  | Qwen3-VL-32B-Instruct  | **15751**  |  
| `gpu/vllm/gpt-oss`  | NVIDIA H100 80 GB · vLLM  | v0.22.0  | 1  | gpt-oss-120B  | **39065**  |  
| `gpu/sglang`  | NVIDIA H100 80 GB · SGLang  | v0.5.13.post1  | 2  | Qwen3-32B  | **30720**  |  
| `amd/vllm`  | AMD GPU · vLLM  | rocm v0.7.0  | 2  | Qwen3-32B  | 15928 ‡  |  
| `amd/sglang`  | AMD GPU · SGLang  | v0.5.13.post1 (rocm)  | 2  | Qwen3-32B  | 30720 ‡  |  
| `metax/vllm`  | MetaX C500X 64 GB · vLLM-MetaX  | 0.24.0 (MACA 3.8.2.1)  | 8  | DeepSeek-R1-Distill-Llama-70B  | **5468**  |  
| `tpu/v6/vllm`  | Google TPU v6e · vLLM  | tpu v0.22.0  | 8  | Qwen3-32B  | **26290**  |  
| `tpu/v7/vllm`  | Google TPU v7x · vLLM  | tpu v0.22.0  | 8  | Qwen3-32B  | **27336**  |  
| `npu/vllm`  | Rebellions NPU · vLLM  | vllm-rbln 0.11.3a7  | 1  | gpt-oss-120B  | **12582**  |  
| `xpu/vllm`  | Intel XPU · vLLM  | xpu v0.7.0  | 1  | Qwen3-0.6B  | 1970 ‡  |  
| `cpu/vllm`  | CPU · vLLM (AMX)  | cpu v0.6.0  | 1  | Llama-3.2-3B-Instruct  | **1970**  |  
  * **15928** — the plugin default; measured for the reference path (`gpu/vllm`, Qwen3-32B on H100 80 GB, TP=2).
  * **30720** — measured for `gpu/sglang` (Qwen3-32B, same H100 80 GB / TP=2): SGLang reaches ~1.9× the vLLM prefill throughput on identical hardware, which is exactly why the serving engine is its own row.
  * **39065** — measured for `gpu/vllm/gpt-oss` (gpt-oss-120B, H100 TP=1): the highest in the table despite the largest model, because gpt-oss is a sparse MoE (~5B active params) in MXFP4, so a prefill step touches few weights.
  * **1970** — measured for `cpu/vllm` (Llama-3.2-3B) on **GCP C3 (Intel Sapphire Rapids, AMX)** , bf16. The `llm-d-cpu` image runs the model in bf16, which **requires AMX or AVX512-BF16**. Calibrated at `CHUNK_SIZE=2048` (the CPU vLLM chunked-prefill default), not 8192.
  * **26290 / 27336** — measured for `tpu/v6/vllm` (TPU v6e, 2x4 = **8 chips**) and `tpu/v7/vllm` (TPU v7x, 2x2x1 = **4 chips**), both Qwen3-32B at TP=8.
  * **5773 / 5468** — measured on MetaX C500X for `metax/vllm` (Qwen3-14B, TP=1, `CHUNK_SIZE=8192`) and `metax/vllm` (DeepSeek-R1-Distill-Llama-70B, TP=8, `CHUNK_SIZE=4095`). The single-GPU value is the higher of two valid runs (5361 and 5773 tokens/sec). The eight-GPU chunk size stays below that profile's `--max-model-len=4096` so the calibration prompt plus one output token fits.
  * The other GPU/TPU paths run at the vLLM default `--max-num-batched-tokens=8192`, so calibrate those with `CHUNK_SIZE=8192`. **Re-measure** if you change TP, chunk size, quantization, or `--max-model-len` — those move the number more than the model identity does.
  * **`npu/vllm`**— gpt-oss-120B on one Rebellions NPU at dp1 and`--max-num-seqs=1` , `--max-num-batched-tokens=512`, so calibrate this path with `CHUNK_SIZE=512`. This path does not borrow a proxy: the closest measured row (`gpu/vllm/gpt-oss`, 39065) is an H100 MXFP4 number and applying it here would open the saturation gate when it should stay shut.
  * **‡ proxy, not measured** — these paths are not yet calibrated, so they borrow the closest measured value as a starting point: the **AMD** paths use the same-engine H100 values (`amd/vllm` ← `gpu/vllm` 15928, `amd/sglang` ← `gpu/sglang` 30720), and **XPU** uses the `cpu/vllm` value (1970). Replace each with a real `calibrate.sh` run on that hardware when available.


**Related (other guides):** the [agentic-serving](https://github.com/llm-d/llm-d/blob/main/guides/agentic-serving) guide ships `peakPrefillThroughput=16444` for Qwen3-Coder-480B-FP8 on TPU v7x (TP=8) — same accelerator family, different model, so it is not an optimized-baseline path but is a useful second data point.
## Filling a cell
Deploy that (model, accelerator) per its guide, then run the calibration Job against the live stack:

```
GUIDE_NAME=optimized-baseline \
NAMESPACE=your-namespace \
MODEL_NAME=huggingface/model-id \
CHUNK_SIZE=the overlay's --max-num-batched-tokens> \
guides/recipes/router/calibration/calibrate.sh
```

Copy the printed `peakPrefillThroughput` into both (a) the cell above and (b) the `prefix-cache-affinity-filter` parameters in your guide's router values file, then `helm upgrade` and restart the EPP (see the calibration README).
You can’t perform that action at this time. 
