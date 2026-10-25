---
title: Text Embeddings Inference · Hugging Face
id: text-embeddings-inference-hugging-face
tags:
- llm-inference
- embeddings
- reranker
- open-source
- docs
created: '2026-10-09T23:15:15.096878Z'
updated: '2026-10-10T14:52:17.345109Z'
source: https://huggingface.co/docs/text-embeddings-inference/index
source_domain: huggingface.co
fetched_at: '2026-10-09T23:15:15.074376Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'HF''s official TEI docs-index page: restates the same feature set as the
  GitHub README (no model-graph compile step, small Docker images/fast boot, token-based
  dynamic batching, Flash Attention/Candle/cuBLASLt kernels, Safetensors loading,
  OTel tracing + Prometheus metrics) at thinner detail; no licensing statement or
  named adopter on this page.'
---

text-embeddings-inference documentation
Text Embeddings Inference
# text-embeddings-inference
🏡 View all docs AWS Trainium & Inferentia Accelerate Argilla AutoTrain Bitsandbytes CLI Chat UI Dataset viewer Datasets Deploying on AWS Diffusers Distilabel Evaluate Google Cloud Google TPUs Gradio Hub Hub Python Library Huggingface.js Inference Endpoints (dedicated) Inference Providers Kernels LeRobot Leaderboards Lighteval Microsoft Azure OpenEnv Optimum PEFT Reachy Mini Safetensors Sentence Transformers TRL Tasks Text Embeddings Inference Text Generation Inference Tokenizers Trackio Transformers Transformers.js Xet smolagents timm
Search documentation
main
Join the Hugging Face community
and get access to the augmented documentation experience
Collaborate on models, datasets and Spaces
Faster examples with accelerated inference
Switch between documentation themes
[Sign Up](https://huggingface.co/join)
to get started
Copy page
#  Text Embeddings Inference
Text Embeddings Inference (TEI) is a comprehensive toolkit designed for efficient deployment and serving of open source text embeddings models. It enables high-performance extraction for the most popular models, including FlagEmbedding, Ember, GTE, and E5.
TEI offers multiple features tailored to optimize the deployment process and enhance overall performance.
**Key Features:**
  * **Streamlined Deployment:** TEI eliminates the need for a model graph compilation step for an easier deployment process.
  * **Efficient Resource Utilization:** Benefit from small Docker images and rapid boot times, allowing for true serverless capabilities.
  * **Dynamic Batching:** TEI incorporates token-based dynamic batching thus optimizing resource utilization during inference.
  * **Optimized Inference:** TEI leverages [Flash Attention](https://github.com/HazyResearch/flash-attention), [Candle](https://github.com/huggingface/candle), and [cuBLASLt](https://docs.nvidia.com/cuda/cublas/#using-the-cublaslt-api) by using optimized transformers code for inference.
  * **Safetensors weight loading:** TEI loads [Safetensors](https://github.com/huggingface/safetensors) weights for faster boot times.
  * **Production-Ready:** TEI supports distributed tracing through Open Telemetry and exports Prometheus metrics.


**Benchmarks**
Benchmark for [BAAI/bge-base-en-v1.5](https://hf.co/BAAI/bge-large-en-v1.5) on an NVIDIA A10 with a sequence length of 512 tokens:
**Getting Started:**
To start using TEI, check the [Quick Tour](https://huggingface.co/docs/text-embeddings-inference/quick_tour) guide.
[ Update on GitHub](https://github.com/huggingface/text-embeddings-inference/blob/main/docs/source/en/index.md)
[Quick Tour→](https://huggingface.co/docs/text-embeddings-inference/quick_tour)
