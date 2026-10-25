---
title: 'GitHub - huggingface/text-embeddings-inference: A blazing fast inference solution
  for text embeddings models · GitHub'
id: github-huggingfacetext-embeddings-inference-a-blazing-fast-inference-solution-fo
tags:
- llm-inference
- embeddings
- reranker
- open-source
- apache-2.0
- build-vs-buy
created: '2026-10-09T23:15:08.411756Z'
updated: '2026-10-10T14:52:17.343976Z'
source: https://github.com/huggingface/text-embeddings-inference
source_domain: github.com
fetched_at: '2026-10-09T23:15:08.389481Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Official TEI repo (HF, Rust 86.9%, v1.9.4 as of Sep 15 2026, 5.1k stars):
  licensed Apache 2.0 since commit #231 (Apr 8 2024) — a non-standard, more restrictive
  license preceded this. Serves both text-embedding models (Nomic/BERT/XLM-RoBERTa/JinaBERT/Mistral/Alibaba-GTE/Qwen2/Qwen3/MPNet/ModernBERT/Gemma3)
  and sequence-classification/re-ranker cross-encoder models (e.g. BAAI/bge-reranker-large/base,
  Alibaba-NLP GTE rerankers) via a dedicated /rerank endpoint, added in TEI v0.4.0.
  Performance design: no model-graph compilation step, token-based dynamic batching,
  Flash Attention + Candle + cuBLASLt optimized kernels, Safetensors/ONNX weight loading,
  gRPC alternative to HTTP, OpenTelemetry tracing + Prometheus metrics. It is embeddings/reranking-only
  — not a text-generation engine, and not a vLLM alternative. No named production
  adopter is stated in the repo itself.'
---

[Skip to content](https://github.com/huggingface/text-embeddings-inference#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/huggingface/text-embeddings-inference) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/huggingface/text-embeddings-inference) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/huggingface/text-embeddings-inference) to refresh your session. Dismiss alert
[**27** Branches](https://github.com/huggingface/text-embeddings-inference/branches)[](https://github.com/huggingface/text-embeddings-inference/tags)
Go to file
Open more actions menu
## Latest commit
and
Open commit detailssuccess
Oct 6, 2026
[5696458](https://github.com/huggingface/text-embeddings-inference/commit/56964585069318d01ddeeec9454a952a66f1d3a2) · Oct 6, 2026
## History
[369 Commits](https://github.com/huggingface/text-embeddings-inference/commits/main/)
Open commit details
369 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |  [fix: add cors_allow_origin to cli (](https://github.com/huggingface/text-embeddings-inference/commit/00a17ea6617099e681bee41af7ebcaf5dd5b803e "fix: add cors_allow_origin to cli \(#162\)")[#162](https://github.com/huggingface/text-embeddings-inference/pull/162)[)](https://github.com/huggingface/text-embeddings-inference/commit/00a17ea6617099e681bee41af7ebcaf5dd5b803e "fix: add cors_allow_origin to cli \(#162\)")  | Feb 22, 2024  |  
|   |   |  [Bump doc-builder workflow pin (](https://github.com/huggingface/text-embeddings-inference/commit/56964585069318d01ddeeec9454a952a66f1d3a2 "Bump doc-builder workflow pin \(#944\)

Co-authored-by: Claude Opus 4.8 <noreply@anthropic.com>")[#944](https://github.com/huggingface/text-embeddings-inference/pull/944)[)](https://github.com/huggingface/text-embeddings-inference/commit/56964585069318d01ddeeec9454a952a66f1d3a2 "Bump doc-builder workflow pin \(#944\)

Co-authored-by: Claude Opus 4.8 <noreply@anthropic.com>")  | Oct 6, 2026  |  
|   |   |   | Oct 13, 2023  |  
|   |   |  [feat: ROCm flash-attn varlen, triton layer norm, and AMD Dockerfile (](https://github.com/huggingface/text-embeddings-inference/commit/d246fbf17cc7e3c87450afcb5c168461ca8f5cfe "feat: ROCm flash-attn varlen, triton layer norm, and AMD Dockerfile \(#860\)

Signed-off-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>
Co-authored-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")[#…](https://github.com/huggingface/text-embeddings-inference/pull/860)  | Sep 15, 2026  |  
|  [Fix to acquire the permit in a blocking way (](https://github.com/huggingface/text-embeddings-inference/commit/0ec2ba5073de67c016cf77b8942ddb15ed3b0397 "Fix to acquire the permit in a blocking way \(#726\)

Co-authored-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")[#726](https://github.com/huggingface/text-embeddings-inference/pull/726)[)](https://github.com/huggingface/text-embeddings-inference/commit/0ec2ba5073de67c016cf77b8942ddb15ed3b0397 "Fix to acquire the permit in a blocking way \(#726\)

Co-authored-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")  | Jan 2, 2026  |  
|  [feat: ROCm flash-attn varlen, triton layer norm, and AMD Dockerfile (](https://github.com/huggingface/text-embeddings-inference/commit/d246fbf17cc7e3c87450afcb5c168461ca8f5cfe "feat: ROCm flash-attn varlen, triton layer norm, and AMD Dockerfile \(#860\)

Signed-off-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>
Co-authored-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")[#…](https://github.com/huggingface/text-embeddings-inference/pull/860)  | Sep 15, 2026  |  
|   |   |  [Add integration tests for Gaudi (](https://github.com/huggingface/text-embeddings-inference/commit/cad4b556c633919b3f50cec7b5b4337cf8f15f0c "Add integration tests for Gaudi \(#598\)")[#598](https://github.com/huggingface/text-embeddings-inference/pull/598)[)](https://github.com/huggingface/text-embeddings-inference/commit/cad4b556c633919b3f50cec7b5b4337cf8f15f0c "Add integration tests for Gaudi \(#598\)")  | Jun 24, 2025  |  
|   |   |  [fix(ort): fix mean pooling (](https://github.com/huggingface/text-embeddings-inference/commit/4cc38bda85b2ecdbf34082796ca9738466483b85 "fix\(ort\): fix mean pooling \(#332\)")[#332](https://github.com/huggingface/text-embeddings-inference/pull/332)[)](https://github.com/huggingface/text-embeddings-inference/commit/4cc38bda85b2ecdbf34082796ca9738466483b85 "fix\(ort\): fix mean pooling \(#332\)")  | Jul 8, 2024  |  
|   |   | [Align the `normalize` param between the gRPC and HTTP /embed interfac…](https://github.com/huggingface/text-embeddings-inference/commit/1bb59202500e5f69dd8be63dd1604f7625124fbe "Align the `normalize` param between the gRPC and HTTP /embed interfaces \(#810\)

Co-authored-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")  | Feb 9, 2026  |  
|   |   |  [Fix single predict request metrics (](https://github.com/huggingface/text-embeddings-inference/commit/f4e75bfbdff97357c8b7b8bb74584787bdff5e6a "Fix single predict request metrics \(#917\)

Co-authored-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")[#917](https://github.com/huggingface/text-embeddings-inference/pull/917)[)](https://github.com/huggingface/text-embeddings-inference/commit/f4e75bfbdff97357c8b7b8bb74584787bdff5e6a "Fix single predict request metrics \(#917\)

Co-authored-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")  | Sep 7, 2026  |  
|   |   |   | Oct 13, 2023  |  
|   |   |   | Oct 13, 2023  |  
|   |   |  [Add integration tests for Gaudi (](https://github.com/huggingface/text-embeddings-inference/commit/cad4b556c633919b3f50cec7b5b4337cf8f15f0c "Add integration tests for Gaudi \(#598\)")[#598](https://github.com/huggingface/text-embeddings-inference/pull/598)[)](https://github.com/huggingface/text-embeddings-inference/commit/cad4b556c633919b3f50cec7b5b4337cf8f15f0c "Add integration tests for Gaudi \(#598\)")  | Jun 24, 2025  |  
|   |   |  [Removing candle-extensions to live on crates.io (](https://github.com/huggingface/text-embeddings-inference/commit/c3e91f33092fe2c47ded5f449231f059e9912a8f "Removing candle-extensions to live on crates.io \(#583\)")[#583](https://github.com/huggingface/text-embeddings-inference/pull/583)[)](https://github.com/huggingface/text-embeddings-inference/commit/c3e91f33092fe2c47ded5f449231f059e9912a8f "Removing candle-extensions to live on crates.io \(#583\)")  | Apr 15, 2025  |  
|   |   |  [fix: add cors_allow_origin to cli (](https://github.com/huggingface/text-embeddings-inference/commit/00a17ea6617099e681bee41af7ebcaf5dd5b803e "fix: add cors_allow_origin to cli \(#162\)")[#162](https://github.com/huggingface/text-embeddings-inference/pull/162)[)](https://github.com/huggingface/text-embeddings-inference/commit/00a17ea6617099e681bee41af7ebcaf5dd5b803e "fix: add cors_allow_origin to cli \(#162\)")  | Feb 22, 2024  |  
|   |   |  [Update GitHub templates & fix mentions to Text Embeddings Inference (](https://github.com/huggingface/text-embeddings-inference/commit/ea5fe86d3fb0d8356e48e54ebbdb80e030af3fd1 "Update GitHub templates & fix mentions to Text Embeddings Inference \(#697\)")[#…](https://github.com/huggingface/text-embeddings-inference/pull/697)  | Aug 11, 2025  |  
|   |   |  [Update GitHub templates & fix mentions to Text Embeddings Inference (](https://github.com/huggingface/text-embeddings-inference/commit/ea5fe86d3fb0d8356e48e54ebbdb80e030af3fd1 "Update GitHub templates & fix mentions to Text Embeddings Inference \(#697\)")[#…](https://github.com/huggingface/text-embeddings-inference/pull/697)  | Aug 11, 2025  |  
|   |   |  [feat: ROCm flash-attn varlen, triton layer norm, and AMD Dockerfile (](https://github.com/huggingface/text-embeddings-inference/commit/d246fbf17cc7e3c87450afcb5c168461ca8f5cfe "feat: ROCm flash-attn varlen, triton layer norm, and AMD Dockerfile \(#860\)

Signed-off-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>
Co-authored-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")[#…](https://github.com/huggingface/text-embeddings-inference/pull/860)  | Sep 15, 2026  |  
|   |   |  [feat: ROCm flash-attn varlen, triton layer norm, and AMD Dockerfile (](https://github.com/huggingface/text-embeddings-inference/commit/d246fbf17cc7e3c87450afcb5c168461ca8f5cfe "feat: ROCm flash-attn varlen, triton layer norm, and AMD Dockerfile \(#860\)

Signed-off-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>
Co-authored-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")[#…](https://github.com/huggingface/text-embeddings-inference/pull/860)  | Sep 15, 2026  |  
|   |   | [Bump Rust 1.92, CUDA 12.6, Ubuntu 24.04 and add `Dockerfile-cuda-blac…](https://github.com/huggingface/text-embeddings-inference/commit/b7e3401880ec0a3a6adfa674b5f3a5395355ac56 "Bump Rust 1.92, CUDA 12.6, Ubuntu 24.04 and add `Dockerfile-cuda-blackwell-all` \(#823\)")  | Feb 12, 2026  |  
|   |   | [Add Dockerfile for ARM64 architecture support and update README instr…](https://github.com/huggingface/text-embeddings-inference/commit/a2c07dded544925aa3130bc4e2d4b867d6d2761e "Add Dockerfile for ARM64 architecture support and update README instructions \(#827\)")  | Mar 30, 2026  |  
|   |   |  [feat: multi-arch CUDA Dockerfile and sm_121 (DGX Spark GB10) (](https://github.com/huggingface/text-embeddings-inference/commit/2e690c25efab29342a797909bd8d1c7222795760 "feat: multi-arch CUDA Dockerfile and sm_121 \(DGX Spark GB10\) \(#840\)

Co-authored-by: github-actions\[bot\] <41898282+github-actions\[bot\]@users.noreply.github.com>
Co-authored-by: z4y4ts <z4y4ts@users.noreply.github.com>")[#840](https://github.com/huggingface/text-embeddings-inference/pull/840)[)](https://github.com/huggingface/text-embeddings-inference/commit/2e690c25efab29342a797909bd8d1c7222795760 "feat: multi-arch CUDA Dockerfile and sm_121 \(DGX Spark GB10\) \(#840\)

Co-authored-by: github-actions\[bot\] <41898282+github-actions\[bot\]@users.noreply.github.com>
Co-authored-by: z4y4ts <z4y4ts@users.noreply.github.com>")  | Mar 31, 2026  |  
|   |   | [Use `rust-toolchain.toml` before `rustup` on `Dockerfile-{cuda,cuda-a…](https://github.com/huggingface/text-embeddings-inference/commit/8f756c4e71b6608c8b1b17ce66c24d1b1c3d6e94 "Use `rust-toolchain.toml` before `rustup` on `Dockerfile-{cuda,cuda-all}` \(#842\)")  | Mar 6, 2026  |  
|   |   | [Bump Rust 1.92, CUDA 12.6, Ubuntu 24.04 and add `Dockerfile-cuda-blac…](https://github.com/huggingface/text-embeddings-inference/commit/b7e3401880ec0a3a6adfa674b5f3a5395355ac56 "Bump Rust 1.92, CUDA 12.6, Ubuntu 24.04 and add `Dockerfile-cuda-blackwell-all` \(#823\)")  | Feb 12, 2026  |  
|   |   |  [feat: ROCm flash-attn varlen, triton layer norm, and AMD Dockerfile (](https://github.com/huggingface/text-embeddings-inference/commit/d246fbf17cc7e3c87450afcb5c168461ca8f5cfe "feat: ROCm flash-attn varlen, triton layer norm, and AMD Dockerfile \(#860\)

Signed-off-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>
Co-authored-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")[#…](https://github.com/huggingface/text-embeddings-inference/pull/860)  | Sep 15, 2026  |  
|   |   |  [Change license to Apache 2.0 (](https://github.com/huggingface/text-embeddings-inference/commit/3c385a4fdced6c526a3ef3ec340e343a2fa40196 "Change license to Apache 2.0 \(#231\)

Co-authored-by: Julien Chaumond <julien@huggingface.co>")[#231](https://github.com/huggingface/text-embeddings-inference/pull/231)[)](https://github.com/huggingface/text-embeddings-inference/commit/3c385a4fdced6c526a3ef3ec340e343a2fa40196 "Change license to Apache 2.0 \(#231\)

Co-authored-by: Julien Chaumond <julien@huggingface.co>")  | Apr 8, 2024  |  
|   |   |  [fix(candle): fix FlashJinaCodeModel (](https://github.com/huggingface/text-embeddings-inference/commit/46de6a49802edd87811163486233fdc6502ccdb1 "fix\(candle\): fix FlashJinaCodeModel \(#302\)")[#302](https://github.com/huggingface/text-embeddings-inference/pull/302)[)](https://github.com/huggingface/text-embeddings-inference/commit/46de6a49802edd87811163486233fdc6502ccdb1 "fix\(candle\): fix FlashJinaCodeModel \(#302\)")  | Jun 21, 2024  |  
|   |   |  [feat: ROCm flash-attn varlen, triton layer norm, and AMD Dockerfile (](https://github.com/huggingface/text-embeddings-inference/commit/d246fbf17cc7e3c87450afcb5c168461ca8f5cfe "feat: ROCm flash-attn varlen, triton layer norm, and AMD Dockerfile \(#860\)

Signed-off-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>
Co-authored-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")[#…](https://github.com/huggingface/text-embeddings-inference/pull/860)  | Sep 15, 2026  |  
|   |   |  [Align](https://github.com/huggingface/text-embeddings-inference/commit/4150561d42c495fe95f2aebb57fbb602c13ff4e6 "Align `cuda-{all-entrypoint,entrypoint}.sh` \(#893\)

Signed-off-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>") `cuda-{all-entrypoint,entrypoint}.sh[](https://github.com/huggingface/text-embeddings-inference/commit/4150561d42c495fe95f2aebb57fbb602c13ff4e6 "Align `cuda-{all-entrypoint,entrypoint}.sh` \(#893\)

Signed-off-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")` [(](https://github.com/huggingface/text-embeddings-inference/commit/4150561d42c495fe95f2aebb57fbb602c13ff4e6 "Align `cuda-{all-entrypoint,entrypoint}.sh` \(#893\)

Signed-off-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")[#893](https://github.com/huggingface/text-embeddings-inference/pull/893)[)](https://github.com/huggingface/text-embeddings-inference/commit/4150561d42c495fe95f2aebb57fbb602c13ff4e6 "Align `cuda-{all-entrypoint,entrypoint}.sh` \(#893\)

Signed-off-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")  | Jul 8, 2026  |  
|   |   |  [Align](https://github.com/huggingface/text-embeddings-inference/commit/4150561d42c495fe95f2aebb57fbb602c13ff4e6 "Align `cuda-{all-entrypoint,entrypoint}.sh` \(#893\)

Signed-off-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>") `cuda-{all-entrypoint,entrypoint}.sh[](https://github.com/huggingface/text-embeddings-inference/commit/4150561d42c495fe95f2aebb57fbb602c13ff4e6 "Align `cuda-{all-entrypoint,entrypoint}.sh` \(#893\)

Signed-off-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")` [(](https://github.com/huggingface/text-embeddings-inference/commit/4150561d42c495fe95f2aebb57fbb602c13ff4e6 "Align `cuda-{all-entrypoint,entrypoint}.sh` \(#893\)

Signed-off-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")[#893](https://github.com/huggingface/text-embeddings-inference/pull/893)[)](https://github.com/huggingface/text-embeddings-inference/commit/4150561d42c495fe95f2aebb57fbb602c13ff4e6 "Align `cuda-{all-entrypoint,entrypoint}.sh` \(#893\)

Signed-off-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")  | Jul 8, 2026  |  
|   |   |   | Jun 25, 2025  |  
|   |   |   | Jun 25, 2025  |  
|   |   |  [Update](https://github.com/huggingface/text-embeddings-inference/commit/ff2f0cccc76db649741381b2973afa5bb7d695ef "Update `rustc` version to 1.92.0 \(#826\)") [version to 1.92.0 (](https://github.com/huggingface/text-embeddings-inference/commit/ff2f0cccc76db649741381b2973afa5bb7d695ef "Update `rustc` version to 1.92.0 \(#826\)")[#826](https://github.com/huggingface/text-embeddings-inference/pull/826)[)](https://github.com/huggingface/text-embeddings-inference/commit/ff2f0cccc76db649741381b2973afa5bb7d695ef "Update `rustc` version to 1.92.0 \(#826\)")  | Feb 12, 2026  |  
| View all files |  
## Repository files navigation
# Text Embeddings Inference
A blazing fast inference solution for text embeddings models.
Benchmark for [BAAI/bge-base-en-v1.5](https://huggingface.co/BAAI/bge-base-en-v1.5) on an NVIDIA A10 with a sequence length of 512 tokens:
## Table of contents
  * [Get Started](https://github.com/huggingface/text-embeddings-inference#get-started)
    * [Supported Models](https://github.com/huggingface/text-embeddings-inference#supported-models)
    * [Docker Images](https://github.com/huggingface/text-embeddings-inference#docker-images)
    * [API Documentation](https://github.com/huggingface/text-embeddings-inference#api-documentation)
    * [Using a private or gated model](https://github.com/huggingface/text-embeddings-inference#using-a-private-or-gated-model)
    * [Air gapped deployment](https://github.com/huggingface/text-embeddings-inference#air-gapped-deployment)
    * [Using Re-rankers models](https://github.com/huggingface/text-embeddings-inference#using-re-rankers-models)
    * [Using Sequence Classification models](https://github.com/huggingface/text-embeddings-inference#using-sequence-classification-models)
    * [Using SPLADE pooling](https://github.com/huggingface/text-embeddings-inference#using-splade-pooling)
    * [Distributed Tracing](https://github.com/huggingface/text-embeddings-inference#distributed-tracing)
  * [Local Install](https://github.com/huggingface/text-embeddings-inference#local-install)
    * [Apple Silicon (Homebrew)](https://github.com/huggingface/text-embeddings-inference#apple-silicon-homebrew)
  * [Docker Build](https://github.com/huggingface/text-embeddings-inference#docker-build)
    * [ARM64 / aarch64](https://github.com/huggingface/text-embeddings-inference#arm64--aarch64)
  * [AMD Instinct GPUs (ROCm)](https://github.com/huggingface/text-embeddings-inference#amd-instinct-gpus-rocm)


Text Embeddings Inference (TEI) is a toolkit for deploying and serving open source text embeddings and sequence classification models. TEI enables high-performance extraction for the most popular models, including FlagEmbedding, Ember, GTE and E5. TEI implements many features such as:
  * No model graph compilation step
  * Metal support for local execution on Macs
  * Small docker images and fast boot times. Get ready for true serverless!
  * Token based dynamic batching
  * Optimized transformers code for inference using [Flash Attention](https://github.com/HazyResearch/flash-attention), [Candle](https://github.com/huggingface/candle) and [cuBLASLt](https://docs.nvidia.com/cuda/cublas/#using-the-cublaslt-api)
  * [Safetensors](https://github.com/huggingface/safetensors) weight loading
  * [ONNX](https://github.com/onnx/onnx) weight loading
  * Production ready (distributed tracing with Open Telemetry, Prometheus metrics)


## Get Started
### Supported Models
#### Text Embeddings
Text Embeddings Inference currently supports Nomic, BERT, CamemBERT, XLM-RoBERTa models with absolute positions, JinaBERT model with Alibi positions and Mistral, Alibaba GTE, Qwen2 models with Rope positions, MPNet, ModernBERT, Qwen3, and Gemma3.
Below are some examples of the currently supported models:  
| MTEB Rank  | Model Size  | Model Type  | Model ID  |  
| --- | --- | --- | --- |  
| 2  | 7.57B (Very Expensive)  | Qwen3  | [Qwen/Qwen3-Embedding-8B](https://hf.co/Qwen/Qwen3-Embedding-8B)  |  
| 3  | 4.02B (Very Expensive)  | Qwen3  | [Qwen/Qwen3-Embedding-4B](https://hf.co/Qwen/Qwen3-Embedding-4B)  |  
| 4  | 509M  | Qwen3  | [Qwen/Qwen3-Embedding-0.6B](https://hf.co/Qwen/Qwen3-Embedding-0.6B)  |  
| 6  | 7.61B (Very Expensive)  | Qwen2  | [Alibaba-NLP/gte-Qwen2-7B-instruct](https://hf.co/Alibaba-NLP/gte-Qwen2-7B-instruct)  |  
| 7  | 560M  | XLM-RoBERTa  | [intfloat/multilingual-e5-large-instruct](https://hf.co/intfloat/multilingual-e5-large-instruct)  |  
| 8  | 308M  | Gemma3  |  [google/embeddinggemma-300m](https://hf.co/google/embeddinggemma-300m) (gated)  |  
| 15  | 1.78B (Expensive)  | Qwen2  | [Alibaba-NLP/gte-Qwen2-1.5B-instruct](https://hf.co/Alibaba-NLP/gte-Qwen2-1.5B-instruct)  |  
| 18  | 7.11B (Very Expensive)  | Mistral  | [Salesforce/SFR-Embedding-2_R](https://hf.co/Salesforce/SFR-Embedding-2_R)  |  
| 35  | 568M  | XLM-RoBERTa  | [Snowflake/snowflake-arctic-embed-l-v2.0](https://hf.co/Snowflake/snowflake-arctic-embed-l-v2.0)  |  
| 41  | 305M  | Alibaba GTE  | [Snowflake/snowflake-arctic-embed-m-v2.0](https://hf.co/Snowflake/snowflake-arctic-embed-m-v2.0)  |  
| 52  | 335M  | BERT  | [WhereIsAI/UAE-Large-V1](https://hf.co/WhereIsAI/UAE-Large-V1)  |  
| 58  | 137M  | NomicBERT  | [nomic-ai/nomic-embed-text-v1](https://hf.co/nomic-ai/nomic-embed-text-v1)  |  
| 79  | 137M  | NomicBERT  | [nomic-ai/nomic-embed-text-v1.5](https://hf.co/nomic-ai/nomic-embed-text-v1.5)  |  
| 103  | 109M  | MPNet  | [sentence-transformers/all-mpnet-base-v2](https://hf.co/sentence-transformers/all-mpnet-base-v2)  |  
| N/A  | 475M-A305M  | NomicBERT  | [nomic-ai/nomic-embed-text-v2-moe](https://hf.co/nomic-ai/nomic-embed-text-v2-moe)  |  
| N/A  | 434M  | Alibaba GTE  | [Alibaba-NLP/gte-large-en-v1.5](https://hf.co/Alibaba-NLP/gte-large-en-v1.5)  |  
| N/A  | 396M  | ModernBERT  | [answerdotai/ModernBERT-large](https://hf.co/answerdotai/ModernBERT-large)  |  
| N/A  | 340M  | Qwen3  | [voyageai/voyage-4-nano](https://hf.co/voyageai/voyage-4-nano)  |  
| N/A  | 137M  | JinaBERT  | [jinaai/jina-embeddings-v2-base-en](https://hf.co/jinaai/jina-embeddings-v2-base-en)  |  
| N/A  | 137M  | JinaBERT  | [jinaai/jina-embeddings-v2-base-code](https://hf.co/jinaai/jina-embeddings-v2-base-code)  |  
To explore the list of best performing text embeddings models, visit the [Massive Text Embedding Benchmark (MTEB) Leaderboard](https://huggingface.co/spaces/mteb/leaderboard).
#### Sequence Classification and Re-Ranking
Text Embeddings Inference currently supports CamemBERT, and XLM-RoBERTa Sequence Classification models with absolute positions.
Below are some examples of the currently supported models:  
| Task  | Model Type  | Model ID  |  
| --- | --- | --- |  
| Re-Ranking  | XLM-RoBERTa  | [BAAI/bge-reranker-large](https://huggingface.co/BAAI/bge-reranker-large)  |  
| Re-Ranking  | XLM-RoBERTa  | [BAAI/bge-reranker-base](https://huggingface.co/BAAI/bge-reranker-base)  |  
| Re-Ranking  | GTE  | [Alibaba-NLP/gte-multilingual-reranker-base](https://huggingface.co/Alibaba-NLP/gte-multilingual-reranker-base)  |  
| Re-Ranking  | ModernBert  | [Alibaba-NLP/gte-reranker-modernbert-base](https://huggingface.co/Alibaba-NLP/gte-reranker-modernbert-base)  |  
| Sentiment Analysis  | RoBERTa  | [SamLowe/roberta-base-go_emotions](https://huggingface.co/SamLowe/roberta-base-go_emotions)  |  
### Docker

```
model=Qwen/Qwen3-Embedding-0.6B
volume=$PWD/data # share a volume with the Docker container to avoid downloading weights every run

docker run --gpus all -p 8080:80 -v $volume:/data --pull always ghcr.io/huggingface/text-embeddings-inference:cuda-1.9 --model-id $model
```

And then you can make requests like

```
curl 127.0.0.1:8080/embed \
    -X POST \
    -d '{"inputs":"What is Deep Learning?"}' \
    -H 'Content-Type: application/json'
```

**Note:** To use GPUs, you need to install the [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html). NVIDIA drivers on your machine need to be compatible with CUDA version 12.2 or higher.
To see all options to serve your models:

```
$ text-embeddings-router --help
Text Embedding Webserver

Usage: text-embeddings-router [OPTIONS] --model-id <MODEL_ID>

Options:
      --model-id <MODEL_ID>
          The Hugging Face model ID, can be any model listed on <https://huggingface.co/models> with the `text-embeddings-inference` tag (meaning it's compatible with Text Embeddings Inference).

          Alternatively, the specified ID can also be a path to a local directory containing the necessary model files saved by the `save_pretrained(...)` methods of either Transformers or Sentence Transformers.

          [env: MODEL_ID=]

      --revision <REVISION>
          The actual revision of the model if you're referring to a model on the hub. You can use a specific commit id or a branch like `refs/pr/2`

          [env: REVISION=]

      --tokenization-workers <TOKENIZATION_WORKERS>
          Optionally control the number of tokenizer workers used for payload tokenization, validation and truncation. Default to the number of CPU cores on the machine

          [env: TOKENIZATION_WORKERS=]

      --dtype <DTYPE>
          The dtype to be forced upon the model

          [env: DTYPE=]
          [possible values: float16, float32]

      --served-model-name <SERVED_MODEL_NAME>
          The name of the model that is being served. If not specified, defaults to `--model-id`. It is only used for the OpenAI-compatible endpoints via HTTP

          [env: SERVED_MODEL_NAME=]

      --pooling <POOLING>
          Optionally control the pooling method for embedding models.

          If `pooling` is not set, the pooling configuration will be parsed from the model `1_Pooling/config.json` configuration.

          If `pooling` is set, it will override the model pooling configuration

          [env: POOLING=]

          Possible values:
          - cls:        Select the CLS token as embedding
          - mean:       Apply Mean pooling to the model embeddings
          - splade:     Apply SPLADE (Sparse Lexical and Expansion) to the model embeddings. This option is only available if the loaded model is a `ForMaskedLM` Transformer model
          - last-token: Select the last token as embedding

      --max-concurrent-requests <MAX_CONCURRENT_REQUESTS>
          The maximum amount of concurrent requests for this particular deployment. Having a low limit will refuse clients requests instead of having them wait for too long and is usually good to handle backpressure correctly

          [env: MAX_CONCURRENT_REQUESTS=]
          [default: 512]

      --max-batch-tokens <MAX_BATCH_TOKENS>
          **IMPORTANT** This is one critical control to allow maximum usage of the available hardware.

          This represents the total amount of potential tokens within a batch.

          For `max_batch_tokens=1000`, you could fit `10` queries of `total_tokens=100` or a single query of `1000` tokens.

          Overall this number should be the largest possible until the model is compute bound. Since the actual memory overhead depends on the model implementation, text-embeddings-inference cannot infer this number automatically.

          [env: MAX_BATCH_TOKENS=]
          [default: 16384]

      --max-batch-requests <MAX_BATCH_REQUESTS>
          Optionally control the maximum number of individual requests in a batch

          [env: MAX_BATCH_REQUESTS=]

      --max-client-batch-size <MAX_CLIENT_BATCH_SIZE>
          Control the maximum number of inputs that a client can send in a single request

          [env: MAX_CLIENT_BATCH_SIZE=]
          [default: 32]

      --auto-truncate
          Control automatic truncation of inputs that exceed the model's maximum supported size. Defaults to `true` (truncation enabled). Set to `false` to disable truncation; when disabled and the model's maximum input length exceeds `--max-batch-tokens`, the server will refuse to start with an error instead of silently truncating sequences.

          Unused for gRPC servers

          [env: AUTO_TRUNCATE=]

      --default-prompt-name <DEFAULT_PROMPT_NAME>
          The name of the prompt that should be used by default for encoding. If not set, no prompt will be applied.

          Must be a key in the `sentence-transformers` configuration `prompts` dictionary.

          For example if ``default_prompt_name`` is "query" and the ``prompts`` is {"query": "query: ", ...}, then the sentence "What is the capital of France?" will be encoded as "query: What is the capital of France?" because the prompt text will be prepended before any text to encode.

          The argument '--default-prompt-name <DEFAULT_PROMPT_NAME>' cannot be used with '--default-prompt <DEFAULT_PROMPT>`

          [env: DEFAULT_PROMPT_NAME=]

      --default-prompt <DEFAULT_PROMPT>
          The prompt that should be used by default for encoding. If not set, no prompt will be applied.

          For example if ``default_prompt`` is "query: " then the sentence "What is the capital of France?" will be encoded as "query: What is the capital of France?" because the prompt text will be prepended before any text to encode.

          The argument '--default-prompt <DEFAULT_PROMPT>' cannot be used with '--default-prompt-name <DEFAULT_PROMPT_NAME>`

          [env: DEFAULT_PROMPT=]

      --dense-path <DENSE_PATH>
          Optionally, define the path to the Dense module required for some embedding models.

          Some embedding models require an extra `Dense` module which contains a single Linear layer and an activation function. By default, those `Dense` modules are stored under the `2_Dense` directory, but there might be cases where different `Dense` modules are provided, to convert the pooled embeddings into different dimensions, available as `2_Dense_<dims>` e.g. https://huggingface.co/NovaSearch/stella_en_400M_v5.

          Note that this argument is optional, only required to be set if there is no `modules.json` file or when you want to override a single Dense module path, only when running with the `candle` backend.

          [env: DENSE_PATH=]

      --hf-token <HF_TOKEN>
          Your Hugging Face Hub token. If neither `--hf-token` nor `HF_TOKEN` are set, the token will be read from the `$HF_HOME/token` path, if it exists. This ensures access to private or gated models, and allows for a more permissive rate limiting

          [env: HF_TOKEN=]

      --hostname <HOSTNAME>
          The IP address to listen on

          [env: HOSTNAME=]
          [default: 0.0.0.0]

      -p, --port <PORT>
          The port to listen on

          [env: PORT=]
          [default: 3000]

      --uds-path <UDS_PATH>
          The name of the unix socket some text-embeddings-inference backends will use as they communicate internally with gRPC

          [env: UDS_PATH=]
          [default: /tmp/text-embeddings-inference-server]

      --huggingface-hub-cache <HUGGINGFACE_HUB_CACHE>
          The location of the huggingface hub cache. Used to override the location if you want to provide a mounted disk for instance

          [env: HUGGINGFACE_HUB_CACHE=]

      --payload-limit <PAYLOAD_LIMIT>
          Payload size limit in bytes

          Default is 2MB

          [env: PAYLOAD_LIMIT=]
          [default: 2000000]

      --api-key <API_KEY>
          Set an api key for request authorization.

          By default the server responds to every request. With an api key set, the requests must have the Authorization header set with the api key as Bearer token.

          [env: API_KEY=]

      --json-output
          Outputs the logs in JSON format (useful for telemetry)

          [env: JSON_OUTPUT=]

      --disable-spans
          Whether or not to include the log trace through spans

          [env: DISABLE_SPANS=]

      --otlp-endpoint <OTLP_ENDPOINT>
          The grpc endpoint for opentelemetry. Telemetry is sent to this endpoint as OTLP over gRPC. e.g. `http://localhost:4317`

          [env: OTLP_ENDPOINT=]

      --otlp-service-name <OTLP_SERVICE_NAME>
          The service name for opentelemetry. e.g. `text-embeddings-inference.server`

          [env: OTLP_SERVICE_NAME=]
          [default: text-embeddings-inference.server]

      --prometheus-port <PROMETHEUS_PORT>
          The Prometheus port to listen on

          [env: PROMETHEUS_PORT=]
          [default: 9000]

      --cors-allow-origin <CORS_ALLOW_ORIGIN>
          Unused for gRPC servers

          [env: CORS_ALLOW_ORIGIN=]

  -h, --help
          Print help (see a summary with '-h')

  -V, --version
          Print version
```

### Docker Images
Text Embeddings Inference ships with multiple Docker images that you can use to target a specific backend:  
| Architecture  | Platform  | Image  |  
| --- | --- | --- |  
| CPU  | x86_64  | ghcr.io/huggingface/text-embeddings-inference:cpu-1.9  |  
| CPU  | aarch64  | ghcr.io/huggingface/text-embeddings-inference:cpu-arm64-1.9  |  
| Volta  | x86_64  | NOT SUPPORTED  |  
| Turing (T4, RTX 2000 series, ...)  | x86_64  | ghcr.io/huggingface/text-embeddings-inference:turing-1.9 (experimental)  |  
| Ampere 8.0 (A100, A30)  | x86_64  | ghcr.io/huggingface/text-embeddings-inference:1.9  |  
| Ampere 8.6 (A10, A40, ...)  | x86_64  | ghcr.io/huggingface/text-embeddings-inference:86-1.9  |  
| Ada Lovelace (RTX 4000 series, ...)  | x86_64  | ghcr.io/huggingface/text-embeddings-inference:89-1.9  |  
| Hopper (H100)  | x86_64  | ghcr.io/huggingface/text-embeddings-inference:hopper-1.9  |  
| Blackwell 10.0 (B200, GB200, ...)  | x86_64  | ghcr.io/huggingface/text-embeddings-inference:100-1.9 (experimental)  |  
| Blackwell 12.0 (GeForce RTX 50X0, ...)  | x86_64  | ghcr.io/huggingface/text-embeddings-inference:120-1.9 (experimental)  |  
| Blackwell 12.1 (DGX Spark GB10, ...)  | multi  | ghcr.io/huggingface/text-embeddings-inference:121-1.9 (experimental)  |  
**Warning** : Flash Attention is turned off by default for the Turing image as it suffers from precision issues. You can turn Flash Attention v1 ON by using the `USE_FLASH_ATTENTION=True` environment variable.
### API documentation
You can consult the OpenAPI documentation of the `text-embeddings-inference` REST API using the `/docs` route. The Swagger UI is also available at: <https://huggingface.github.io/text-embeddings-inference>.
### Using a private or gated model
You have the option to utilize the `HF_TOKEN` environment variable for configuring the token employed by `text-embeddings-inference`. This allows you to gain access to protected resources.
For example:
  1. Go to <https://huggingface.co/settings/tokens>
  2. Copy your CLI READ token
  3. Export `HF_TOKEN=<your CLI READ token>`


or with Docker:

```
model=your private model
volume=$PWD/data # share a volume with the Docker container to avoid downloading weights every run
token=your CLI READ token

docker run --gpus all -e HF_TOKEN=$token -p 8080:80 -v $volume:/data --pull always ghcr.io/huggingface/text-embeddings-inference:cuda-1.9 --model-id $model
```

### Air gapped deployment
To deploy Text Embeddings Inference in an air-gapped environment, first download the weights and then mount them inside the container using a volume.
For example:

```
# (Optional) create a `models` directory
mkdir models
cd models

# Make sure you have git-lfs installed (https://git-lfs.com)
git lfs install
git clone https://huggingface.co/Qwen/Qwen3-Embedding-0.6B

# Set the models directory as the volume path
volume=$PWD

# Mount the models directory inside the container with a volume and set the model ID
docker run --gpus all -p 8080:80 -v $volume:/data --pull always ghcr.io/huggingface/text-embeddings-inference:cuda-1.9 --model-id /data/Qwen3-Embedding-0.6B
```

### Using Re-rankers models
`text-embeddings-inference` v0.4.0 added support for CamemBERT, RoBERTa, XLM-RoBERTa, and GTE Sequence Classification models. Re-rankers models are Sequence Classification cross-encoders models with a single class that scores the similarity between a query and a text.
See [this blogpost](https://blog.llamaindex.ai/boosting-rag-picking-the-best-embedding-reranker-models-42d079022e83) by the LlamaIndex team to understand how you can use re-rankers models in your RAG pipeline to improve downstream performance.

```
model=BAAI/bge-reranker-large
volume=$PWD/data # share a volume with the Docker container to avoid downloading weights every run

docker run --gpus all -p 8080:80 -v $volume:/data --pull always ghcr.io/huggingface/text-embeddings-inference:cuda-1.9 --model-id $model
```

And then you can rank the similarity between a query and a list of texts with:

```
curl 127.0.0.1:8080/rerank \
    -X POST \
    -d '{"query": "What is Deep Learning?", "texts": ["Deep Learning is not...", "Deep learning is..."]}' \
    -H 'Content-Type: application/json'
```

### Using Sequence Classification models
You can also use classic Sequence Classification models like `SamLowe/roberta-base-go_emotions`:

```
model=SamLowe/roberta-base-go_emotions
volume=$PWD/data # share a volume with the Docker container to avoid downloading weights every run

docker run --gpus all -p 8080:80 -v $volume:/data --pull always ghcr.io/huggingface/text-embeddings-inference:cuda-1.9 --model-id $model
```

Once you have deployed the model you can use the `predict` endpoint to get the emotions most associated with an input:

```
curl 127.0.0.1:8080/predict \
    -X POST \
    -d '{"inputs":"I like you."}' \
    -H 'Content-Type: application/json'
```

### Using SPLADE pooling
You can choose to activate SPLADE pooling for Bert and Distilbert MaskedLM architectures:

```
model=naver/efficient-splade-VI-BT-large-query
volume=$PWD/data # share a volume with the Docker container to avoid downloading weights every run

docker run --gpus all -p 8080:80 -v $volume:/data --pull always ghcr.io/huggingface/text-embeddings-inference:cuda-1.9 --model-id $model --pooling splade
```

Once you have deployed the model you can use the `/embed_sparse` endpoint to get the sparse embedding:

```
curl 127.0.0.1:8080/embed_sparse \
    -X POST \
    -d '{"inputs":"I like you."}' \
    -H 'Content-Type: application/json'
```

### Distributed Tracing
`text-embeddings-inference` is instrumented with distributed tracing using OpenTelemetry. You can use this feature by setting the address to an OTLP collector with the `--otlp-endpoint` argument.
`text-embeddings-inference` offers a gRPC API as an alternative to the default HTTP API for high performance deployments. The API protobuf definition can be found [here](https://github.com/huggingface/text-embeddings-inference/blob/main/proto/tei.proto).
You can use the gRPC API by adding the `-grpc` tag to any TEI Docker image. For example:

```
model=Qwen/Qwen3-Embedding-0.6B
volume=$PWD/data # share a volume with the Docker container to avoid downloading weights every run

docker run --gpus all -p 8080:80 -v $volume:/data --pull always ghcr.io/huggingface/text-embeddings-inference:cuda-1.9-grpc --model-id $model
```


```
grpcurl -d '{"inputs": "What is Deep Learning"}' -plaintext 0.0.0.0:8080 tei.v1.Embed/Embed
```

## Local install
### Apple Silicon (Homebrew)
On Apple Silicon (M1/M2/M3/M4), you can install a prebuilt binary via Homebrew:

```
brew install text-embeddings-inference
```

Then launch Text Embeddings Inference with Metal acceleration:

```
model=Qwen/Qwen3-Embedding-0.6B

text-embeddings-router --model-id $model --port 8080
```

You can also opt to install `text-embeddings-inference` locally.
First [install Rust](https://rustup.rs/):

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Then run:

```
# On x86 with ONNX backend (recommended)
cargo install --path router -F ort
# On x86 with Intel backend
cargo install --path router -F mkl
# On M1 or M2
cargo install --path router -F metal
```

You can now launch Text Embeddings Inference on CPU with:

```
model=Qwen/Qwen3-Embedding-0.6B

text-embeddings-router --model-id $model --port 8080
```

**Note:** on some machines, you may also need the OpenSSL libraries and gcc. On Linux machines, run:

```
sudo apt-get install libssl-dev gcc -y
```

GPUs with CUDA compute capabilities < 7.5 are not supported (V100, Titan V, GTX 1000 series, ...).
Make sure you have CUDA and the NVIDIA drivers installed. NVIDIA drivers on your device need to be compatible with CUDA version 12.2 or higher. You also need to add the NVIDIA binaries to your path:

```
export PATH=$PATH:/usr/local/cuda/bin
```

Then run the following (might take a while as it needs to compile the CUDA kernels):

```
# On Turing GPUs (T4, RTX 2000 series ... )
cargo install --path router -F candle-cuda-turing

# On Ampere, Ada Lovelace, Hopper and Blackwell
cargo install --path router -F candle-cuda
```

You can now launch Text Embeddings Inference on GPU as follows:

```
model=Qwen/Qwen3-Embedding-0.6B

text-embeddings-router --model-id $model --port 8080
```

## Docker
You can build the CPU container with Docker as:

```
docker build -f Dockerfile .
```

To build the CUDA containers, you need to know the compute cap of the GPU you will be using at runtime, to build the image accordingly:

```
# Get submodule dependencies
git submodule update --init

# Example for Turing (T4, RTX 2000 series, ...)
runtime_compute_cap=75

# Example for Ampere (A100, ...)
runtime_compute_cap=80

# Example for Ampere (A10, ...)
runtime_compute_cap=86

# Example for Ada Lovelace (RTX 4000 series, ...)
runtime_compute_cap=89

# Example for Hopper (H100, ...)
runtime_compute_cap=90

# Example for Blackwell (B200, GB200, ...)
runtime_compute_cap=100

# Example for Blackwell (GeForce RTX 50X0, RTX PRO 6000, ...)
runtime_compute_cap=120

# Example for Blackwell GB10 (DGX Spark)
runtime_compute_cap=121

docker build . -f Dockerfile-cuda --build-arg CUDA_COMPUTE_CAP=$runtime_compute_cap
```

### ARM64 / aarch64
#### CPU-only (Apple Silicon, Ampere, Graviton)
For ARM64 hosts without NVIDIA GPUs, use the CPU Dockerfile. Inference runs on CPU cores only (no Metal/MPS support via Docker).

```
docker build . -f Dockerfile-arm64 --platform=linux/arm64
```

#### CUDA on ARM64 (DGX Spark, Jetson)
For ARM64 hosts with NVIDIA GPUs, build `Dockerfile-cuda` with the appropriate compute capability and `--platform linux/arm64`:

```
# DGX Spark (GB10, sm_121)
docker build . -f Dockerfile-cuda \
  --build-arg CUDA_COMPUTE_CAP=121 \
  --platform linux/arm64

# Future ARM64 + Blackwell devices (sm_120)
docker build . -f Dockerfile-cuda \
  --build-arg CUDA_COMPUTE_CAP=120 \
  --platform linux/arm64
```

## AMD Instinct GPUs (ROCm)
TEI supports AMD Instinct GPUs (MI200, MI300 series) via ROCm.

```
model=BAAI/bge-base-en-v1.5
volume=$PWD/data

docker run \
  --device /dev/kfd --device /dev/dri \
  --group-add video \
  --ipc=host \
  -p 8080:80 \
  -v $volume:/data \
  --pull always \
  ghcr.io/huggingface/text-embeddings-inference:rocm-latest \
  --model-id $model --dtype bfloat16
```

For full setup instructions, see the **[AMD Instinct GPU guide](https://huggingface.co/docs/text-embeddings-inference/amd_gpu)**.
## Examples
  * [Set up an Inference Endpoint with TEI](https://huggingface.co/learn/cookbook/automatic_embedding_tei_inference_endpoints)
  * [RAG containers with TEI](https://github.com/plaggy/rag-containers)


## About
A blazing fast inference solution for text embeddings models
[huggingface.co/docs/text-embeddings-inference/quick_tour](https://huggingface.co/docs/text-embeddings-inference/quick_tour)
### Topics
### Resources
### Code of conduct
### Contributing
### Stars
**5.1k** stars
### Watchers
**44** watching
### Forks
[ forks](https://github.com/huggingface/text-embeddings-inference/forks)
## 
[ v1.9.4Latest Sep 15, 2026 ](https://github.com/huggingface/text-embeddings-inference/releases/tag/v1.9.4)
## 
## Languages
  * [Rust86.9%](https://github.com/huggingface/text-embeddings-inference/search?l=rust)
  * [Python11.1%](https://github.com/huggingface/text-embeddings-inference/search?l=python)
  * [JavaScript1.1%](https://github.com/huggingface/text-embeddings-inference/search?l=javascript)
  * [Dockerfile0.4%](https://github.com/huggingface/text-embeddings-inference/search?l=dockerfile)
  * [Shell0.3%](https://github.com/huggingface/text-embeddings-inference/search?l=shell)
  * [Makefile0.1%](https://github.com/huggingface/text-embeddings-inference/search?l=makefile)
  * [Nix0.1%](https://github.com/huggingface/text-embeddings-inference/search?l=nix)


You can’t perform that action at this time. 
