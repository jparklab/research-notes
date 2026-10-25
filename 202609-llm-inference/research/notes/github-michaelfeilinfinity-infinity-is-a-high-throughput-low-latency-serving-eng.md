---
title: 'GitHub - michaelfeil/infinity: Infinity is a high-throughput, low-latency
  serving engine for text-embeddings, reranking models, clip, clap and colpali · GitHub'
id: github-michaelfeilinfinity-infinity-is-a-high-throughput-low-latency-serving-eng
tags:
- internal-llm-inference-stacks-7f0c90
- embeddings
- reranking
- oss-repo
- rag
- model-serving
created: '2026-10-09T22:57:16.168442Z'
updated: '2026-10-10T14:52:17.132644Z'
source: https://github.com/michaelfeil/infinity
source_domain: github.com
fetched_at: '2026-10-09T22:57:16.146765Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub repo for Infinity (michaelfeil/infinity, MIT license), a high-throughput
  low-latency REST API server for serving text-embeddings, reranking models, CLIP,
  CLAP and ColPali -- the embeddings/reranking-layer counterpart to generation-focused
  engines like vLLM/TGI, directly relevant to the claim that internal RAG services
  serve far more embedding/rerank calls than generation calls. 3.0k stars, 22 watchers,
  latest tagged release 0.0.77 (Aug 22, 2025) -- a release gap of roughly 14 months
  as of this Oct 2026 fetch, worth flagging as a possible slowdown signal even though
  a ''2025/07 Blackwell support'' changelog entry suggests some continued activity
  after that tag. Technical design: deploys any embedding/reranking/CLIP/sentence-transformer
  model directly from HuggingFace; built on PyTorch, Optimum (ONNX/TensorRT), and
  CTranslate2 with FlashAttention, supporting NVIDIA CUDA, AMD ROCm, CPU, AWS Inferentia2
  (INF2), and Apple MPS backends; uses dynamic batching and dedicated tokenizer worker
  threads; multi-model orchestration lets one server instance mix-and-match several
  embedding/rerank/CLIP models simultaneously; exposes an OpenAPI surface aligned
  to OpenAI''s embeddings API spec so it is a drop-in replacement at the API-contract
  level; ships specialized Docker images per backend (latest-cpu for Optimum/ONNX,
  latest-rocm, latest-trt-onnx with CUDA/TensorRT execution providers and flash-attn).
  CLI v2 supports launching multiple models at once via repeated --model-id flags
  or semicolon-delimited env vars (INFINITY_MODEL_ID, INFINITY_BATCH_SIZE), and a
  separate infinity_client pip package (since 2024/10) decouples client code from
  the server. A commit-log entry for AWS Neuron (inf2.xlarge) deployment reports ~210
  embeddings/sec throughput and ~25ms latency for bge-small-en-v1.5 as a measured
  benchmark data point.'
utility_score: 14.0
---

[Skip to content](https://github.com/michaelfeil/infinity#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/michaelfeil/infinity) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/michaelfeil/infinity) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/michaelfeil/infinity) to refresh your session. Dismiss alert
[**24** Branches](https://github.com/michaelfeil/infinity/branches)[](https://github.com/michaelfeil/infinity/tags)
Go to file
Open more actions menu
## Latest commit
and
Ubuntu
[Fix Neuron engine for SDK 2.28+ and add Trainium (trn2) support (](https://github.com/michaelfeil/infinity/commit/1eb4396b9c7711111224b31c09eda71fbce93075)[#659](https://github.com/michaelfeil/infinity/pull/659)[)](https://github.com/michaelfeil/infinity/commit/1eb4396b9c7711111224b31c09eda71fbce93075)
Open commit detailsfailure
Mar 24, 2026
[1eb4396](https://github.com/michaelfeil/infinity/commit/1eb4396b9c7711111224b31c09eda71fbce93075) · Mar 24, 2026
## History
[989 Commits](https://github.com/michaelfeil/infinity/commits/main/)
Open commit details
989 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |   | Aug 26, 2025  |  
| [Prevent special tokens from being added into yes no tokens, improve c…](https://github.com/michaelfeil/infinity/commit/fcb951cb679d0de185bf254600d9b7d2cd3a263a "Prevent special tokens from being added into yes no tokens, improve code readability with minor yes no ordering bug \(#645\)")  | Oct 7, 2025  |  
|   |   |  [Fix Neuron engine for SDK 2.28+ and add Trainium (trn2) support (](https://github.com/michaelfeil/infinity/commit/1eb4396b9c7711111224b31c09eda71fbce93075 "Fix Neuron engine for SDK 2.28+ and add Trainium \(trn2\) support \(#659\)

* Fix Neuron engine compatibility with optimum >= 2.0 and Neuron SDK 2.28

- Handle removal of optimum.bettertransformer in optimum >= 2.0 by wrapping
  the import in try/except \(acceleration.py\)
- Include token_type_ids in Neuron tokenizer output to match compiled model
  expectations \(neuron.py\)
- Update Dockerfile.neuron to remove pinned SDK versions
- Rewrite README.md with tested instructions for AWS DLAMI setup

Tested on inf2.xlarge with Neuron SDK 2.28, optimum-neuron 0.4.3 and 0.4.5.
Performance: ~210 embeddings/sec throughput, ~25ms latency \(bge-small-en-v1.5\).

* Add CHECK_OPTIMUM guard and cross-instance benchmarks

- Fix NameError when BetterTransformerManager is not available in
  check_if_bettertransformer_possible\(\) \(affects both torch and neuron
  engines on optimum >= 2.0\)
- Update README with benchmark results across g5, inf2, trn2 instances

* Use data parallelism instead of tensor parallelism for Neuron

Compile models for a single NeuronCore \(num_cores=1\) instead of
sharding across all cores.  This gives 5% better single-core
performance and enables linear scaling via multiple processes.

With 2 processes on inf2.xlarge \(2 cores\):
- Before \(tensor parallel\): 206 emb/s
- After \(data parallel\):    425 emb/s  \(+106%\)

Update README with multi-process deployment instructions and
revised benchmark results.

* Add trn2.3xlarge benchmarks and data parallelism examples to Neuron docs

- Add trn2.3xlarge \(4 NeuronCores\) benchmark results: 19ms latency, 753 emb/s
- Add trn2 data parallelism example \(4 processes on 4 cores\)
- Restructure throughput table to show scaling across instance types
- Note: trn2 has ~30% lower per-core latency than inf2

* Address code review feedback: fix BetterTransformer guard, add NEURON_NUM_CORES env var

- acceleration.py: Use globals\(\) check instead of CHECK_OPTIMUM.is_available
  to correctly detect when bettertransformer import failed \(mark_dirty does
  not invalidate the cached is_available property\)
- neuron.py: Add NEURON_NUM_CORES env var \(default 1\) so large models can
  opt in to tensor parallelism without source changes
- neuron.py: Remove dead get_nc_count\(\) function and unused imports
- Dockerfile: Merge RUN layers, add --no-cache-dir, add tested version comment
- README: Clean up redundant text in throughput table

---------

Co-authored-by: Ubuntu <ubuntu@ip-172-31-42-254.sa-east-1.compute.internal>")[#659](https://github.com/michaelfeil/infinity/pull/659)[)](https://github.com/michaelfeil/infinity/commit/1eb4396b9c7711111224b31c09eda71fbce93075 "Fix Neuron engine for SDK 2.28+ and add Trainium \(trn2\) support \(#659\)

* Fix Neuron engine compatibility with optimum >= 2.0 and Neuron SDK 2.28

- Handle removal of optimum.bettertransformer in optimum >= 2.0 by wrapping
  the import in try/except \(acceleration.py\)
- Include token_type_ids in Neuron tokenizer output to match compiled model
  expectations \(neuron.py\)
- Update Dockerfile.neuron to remove pinned SDK versions
- Rewrite README.md with tested instructions for AWS DLAMI setup

Tested on inf2.xlarge with Neuron SDK 2.28, optimum-neuron 0.4.3 and 0.4.5.
Performance: ~210 embeddings/sec throughput, ~25ms latency \(bge-small-en-v1.5\).

* Add CHECK_OPTIMUM guard and cross-instance benchmarks

- Fix NameError when BetterTransformerManager is not available in
  check_if_bettertransformer_possible\(\) \(affects both torch and neuron
  engines on optimum >= 2.0\)
- Update README with benchmark results across g5, inf2, trn2 instances

* Use data parallelism instead of tensor parallelism for Neuron

Compile models for a single NeuronCore \(num_cores=1\) instead of
sharding across all cores.  This gives 5% better single-core
performance and enables linear scaling via multiple processes.

With 2 processes on inf2.xlarge \(2 cores\):
- Before \(tensor parallel\): 206 emb/s
- After \(data parallel\):    425 emb/s  \(+106%\)

Update README with multi-process deployment instructions and
revised benchmark results.

* Add trn2.3xlarge benchmarks and data parallelism examples to Neuron docs

- Add trn2.3xlarge \(4 NeuronCores\) benchmark results: 19ms latency, 753 emb/s
- Add trn2 data parallelism example \(4 processes on 4 cores\)
- Restructure throughput table to show scaling across instance types
- Note: trn2 has ~30% lower per-core latency than inf2

* Address code review feedback: fix BetterTransformer guard, add NEURON_NUM_CORES env var

- acceleration.py: Use globals\(\) check instead of CHECK_OPTIMUM.is_available
  to correctly detect when bettertransformer import failed \(mark_dirty does
  not invalidate the cached is_available property\)
- neuron.py: Add NEURON_NUM_CORES env var \(default 1\) so large models can
  opt in to tensor parallelism without source changes
- neuron.py: Remove dead get_nc_count\(\) function and unused imports
- Dockerfile: Merge RUN layers, add --no-cache-dir, add tested version comment
- README: Clean up redundant text in throughput table

---------

Co-authored-by: Ubuntu <ubuntu@ip-172-31-42-254.sa-east-1.compute.internal>")  | Mar 24, 2026  |  
|  [Fix Neuron engine for SDK 2.28+ and add Trainium (trn2) support (](https://github.com/michaelfeil/infinity/commit/1eb4396b9c7711111224b31c09eda71fbce93075 "Fix Neuron engine for SDK 2.28+ and add Trainium \(trn2\) support \(#659\)

* Fix Neuron engine compatibility with optimum >= 2.0 and Neuron SDK 2.28

- Handle removal of optimum.bettertransformer in optimum >= 2.0 by wrapping
  the import in try/except \(acceleration.py\)
- Include token_type_ids in Neuron tokenizer output to match compiled model
  expectations \(neuron.py\)
- Update Dockerfile.neuron to remove pinned SDK versions
- Rewrite README.md with tested instructions for AWS DLAMI setup

Tested on inf2.xlarge with Neuron SDK 2.28, optimum-neuron 0.4.3 and 0.4.5.
Performance: ~210 embeddings/sec throughput, ~25ms latency \(bge-small-en-v1.5\).

* Add CHECK_OPTIMUM guard and cross-instance benchmarks

- Fix NameError when BetterTransformerManager is not available in
  check_if_bettertransformer_possible\(\) \(affects both torch and neuron
  engines on optimum >= 2.0\)
- Update README with benchmark results across g5, inf2, trn2 instances

* Use data parallelism instead of tensor parallelism for Neuron

Compile models for a single NeuronCore \(num_cores=1\) instead of
sharding across all cores.  This gives 5% better single-core
performance and enables linear scaling via multiple processes.

With 2 processes on inf2.xlarge \(2 cores\):
- Before \(tensor parallel\): 206 emb/s
- After \(data parallel\):    425 emb/s  \(+106%\)

Update README with multi-process deployment instructions and
revised benchmark results.

* Add trn2.3xlarge benchmarks and data parallelism examples to Neuron docs

- Add trn2.3xlarge \(4 NeuronCores\) benchmark results: 19ms latency, 753 emb/s
- Add trn2 data parallelism example \(4 processes on 4 cores\)
- Restructure throughput table to show scaling across instance types
- Note: trn2 has ~30% lower per-core latency than inf2

* Address code review feedback: fix BetterTransformer guard, add NEURON_NUM_CORES env var

- acceleration.py: Use globals\(\) check instead of CHECK_OPTIMUM.is_available
  to correctly detect when bettertransformer import failed \(mark_dirty does
  not invalidate the cached is_available property\)
- neuron.py: Add NEURON_NUM_CORES env var \(default 1\) so large models can
  opt in to tensor parallelism without source changes
- neuron.py: Remove dead get_nc_count\(\) function and unused imports
- Dockerfile: Merge RUN layers, add --no-cache-dir, add tested version comment
- README: Clean up redundant text in throughput table

---------

Co-authored-by: Ubuntu <ubuntu@ip-172-31-42-254.sa-east-1.compute.internal>")[#659](https://github.com/michaelfeil/infinity/pull/659)[)](https://github.com/michaelfeil/infinity/commit/1eb4396b9c7711111224b31c09eda71fbce93075 "Fix Neuron engine for SDK 2.28+ and add Trainium \(trn2\) support \(#659\)

* Fix Neuron engine compatibility with optimum >= 2.0 and Neuron SDK 2.28

- Handle removal of optimum.bettertransformer in optimum >= 2.0 by wrapping
  the import in try/except \(acceleration.py\)
- Include token_type_ids in Neuron tokenizer output to match compiled model
  expectations \(neuron.py\)
- Update Dockerfile.neuron to remove pinned SDK versions
- Rewrite README.md with tested instructions for AWS DLAMI setup

Tested on inf2.xlarge with Neuron SDK 2.28, optimum-neuron 0.4.3 and 0.4.5.
Performance: ~210 embeddings/sec throughput, ~25ms latency \(bge-small-en-v1.5\).

* Add CHECK_OPTIMUM guard and cross-instance benchmarks

- Fix NameError when BetterTransformerManager is not available in
  check_if_bettertransformer_possible\(\) \(affects both torch and neuron
  engines on optimum >= 2.0\)
- Update README with benchmark results across g5, inf2, trn2 instances

* Use data parallelism instead of tensor parallelism for Neuron

Compile models for a single NeuronCore \(num_cores=1\) instead of
sharding across all cores.  This gives 5% better single-core
performance and enables linear scaling via multiple processes.

With 2 processes on inf2.xlarge \(2 cores\):
- Before \(tensor parallel\): 206 emb/s
- After \(data parallel\):    425 emb/s  \(+106%\)

Update README with multi-process deployment instructions and
revised benchmark results.

* Add trn2.3xlarge benchmarks and data parallelism examples to Neuron docs

- Add trn2.3xlarge \(4 NeuronCores\) benchmark results: 19ms latency, 753 emb/s
- Add trn2 data parallelism example \(4 processes on 4 cores\)
- Restructure throughput table to show scaling across instance types
- Note: trn2 has ~30% lower per-core latency than inf2

* Address code review feedback: fix BetterTransformer guard, add NEURON_NUM_CORES env var

- acceleration.py: Use globals\(\) check instead of CHECK_OPTIMUM.is_available
  to correctly detect when bettertransformer import failed \(mark_dirty does
  not invalidate the cached is_available property\)
- neuron.py: Add NEURON_NUM_CORES env var \(default 1\) so large models can
  opt in to tensor parallelism without source changes
- neuron.py: Remove dead get_nc_count\(\) function and unused imports
- Dockerfile: Merge RUN layers, add --no-cache-dir, add tested version comment
- README: Clean up redundant text in throughput table

---------

Co-authored-by: Ubuntu <ubuntu@ip-172-31-42-254.sa-east-1.compute.internal>")  | Mar 24, 2026  |  
|   |   |   | Jan 15, 2024  |  
|   |   |   | Aug 15, 2024  |  
|   |   |   | Jan 31, 2025  |  
|   |   |   | Jun 1, 2024  |  
|   |   |   | Oct 11, 2023  |  
|   |   |  [New torch version (2.8) New flash attnention, remove hf_transfer (](https://github.com/michaelfeil/infinity/commit/18bfb975801de324e367b76a94f61c229cb3caea "New torch version \(2.8\) New flash attnention, remove hf_transfer \(#636\)

* hf-xet

* update torch deps

* update docker file

* update docker templates

* hf token no permissions

* remove pg bar")[#636](https://github.com/michaelfeil/infinity/pull/636)[)](https://github.com/michaelfeil/infinity/commit/18bfb975801de324e367b76a94f61c229cb3caea "New torch version \(2.8\) New flash attnention, remove hf_transfer \(#636\)

* hf-xet

* update torch deps

* update docker file

* update docker templates

* hf token no permissions

* remove pg bar")  | Aug 22, 2025  |  
| View all files |  
## Repository files navigation
# Infinity ♾️
Infinity is a high-throughput, low-latency REST API for serving text-embeddings, reranking models, clip, clap and colpali. Infinity is developed under [MIT License](https://github.com/michaelfeil/infinity/blob/main/LICENSE).
## Why Infinity
  * **Deploy any model from HuggingFace** : deploy any embedding, reranking, clip and sentence-transformer model from [HuggingFace](https://huggingface.co/models?other=text-embeddings-inference&sort=trending)
  * **Fast inference backends** : The inference server is built on top of [PyTorch](https://github.com/pytorch/pytorch), [optimum (ONNX/TensorRT)](https://huggingface.co/docs/optimum/index) and [CTranslate2](https://github.com/OpenNMT/CTranslate2), using FlashAttention to get the most out of your **NVIDIA CUDA** , **AMD ROCM** , **CPU** , **AWS INF2** or **APPLE MPS** accelerator. Infinity uses dynamic batching and tokenization dedicated in worker threads.
  * **Multi-modal and multi-model** : Mix-and-match multiple models. Infinity orchestrates them.
  * **Tested implementation** : Unit and end-to-end tested. Embeddings via infinity are correctly embedded. Lets API users create embeddings till infinity and beyond.
  * **Easy to use** : Built on [FastAPI](https://fastapi.tiangolo.com/). Infinity CLI v2 allows launching of all arguments via Environment variable or argument. OpenAPI aligned to [OpenAI's API specs](https://platform.openai.com/docs/guides/embeddings/what-are-embeddings). View the docs at [https://michaelfeil.github.io/infinity](https://michaelfeil.github.io/infinity/) on how to get started.


### Latest News 🔥
  * [2025/07] Blackwell support
  * [2024/11] AMD, CPU, ONNX docker images
  * [2024/10] `pip install infinity_client`
  * [2024/07] Inference deployment example via [Modal](https://github.com/michaelfeil/infinity/blob/main/infra/modal/README.md) and a [free GPU deployment](https://infinity.modal.michaelfeil.eu/)
  * [2024/06] Support for multi-modal: clip, text-classification & launch all arguments from env variables
  * [2024/05] launch multiple models using the `v2` cli, including `--api-key`
  * [2024/03] infinity supports experimental int8 (cpu/cuda) and fp8 (H100/MI300) support
  * [2024/03] Docs are online: <https://michaelfeil.github.io/infinity/latest/>
  * [2024/02] Community meetup at the [Run:AI Infra Club](https://discord.gg/7D4fbEgWjv)
  * [2024/01] TensorRT / ONNX inference
  * [2023/10] Initial release


## Getting started
### Launch the cli via pip install

```
pip install infinity-emb[all]
```

After your pip install, with your venv active, you can run the CLI directly.

```
infinity_emb v2 --model-id BAAI/bge-small-en-v1.5
```

Check the `v2 --help` command to get a description for all parameters.

```
infinity_emb v2 --help
```

### Launch the CLI using a pre-built docker container (recommended)
Instead of installing the CLI via pip, you may also use docker to run `michaelf34/infinity`. Make sure you mount your accelerator ( i.e. install `nvidia-docker` and activate with `--gpus all`).

```
port=7997
model1=michaelfeil/bge-small-en-v1.5
model2=mixedbread-ai/mxbai-rerank-xsmall-v1
volume=$PWD/data

docker run -it --gpus all \
 -v $volume:/app/.cache \
 -p $port:$port \
 michaelf34/infinity:latest \
 v2 \
 --model-id $model1 \
 --model-id $model2 \
 --port $port
```

The cache path inside the docker container is set by the environment variable `HF_HOME`.
#### Specialized docker images
Docker container for CPU Use the `latest-cpu` image or `x.x.x-cpu` for slimer image. Run like any other cpu-only docker image. Optimum/Onnx is often the prefered engine. 

```
docker run -it \
-v $volume:/app/.cache \
-p $port:$port \
michaelf34/infinity:latest-cpu \
v2 \
--engine optimum \
--model-id $model1 \
--model-id $model2 \
--port $port

```

Docker Container for ROCm (MI200 Series and MI300 Series) Use the `latest-rocm` image or `x.x.x-rocm` for rocm compatible inference. **This image is currently not build via CI/CD (to large), consider pinning to exact version.** Make sure you have ROCm is correctly installed and ready to use with Docker. 
Visit [Docs](https://michaelfeil.github.io/infinity) for more info.
Docker Container for Onnx-GPU, Cuda Extensions, TensorRT Use the `latest-trt-onnx` image or `x.x.x-trt-onnx` for nvidia compatible inference. **This image is currently not build via CI/CD (to large), consider pinning to exact version.** 
This image has support for:
  * ONNX-Cuda "CudaExecutionProvider"
  * ONNX-TensorRT "TensorRTExecutionProvider" (may not always work due to version mismatch with ORT)
  * CudaExtensions and packages, e.g. Tri-Dao's `pip install flash-attn` package when using Pytorch.
  * nvcc compiler support



```
docker run -it \
-v $volume:/app/.cache \
-p $port:$port \
michaelf34/infinity:latest-trt-onnx \
v2 \
--engine optimum \
--device cuda \
--model-id $model1 \
--port $port

```

#### Using local models with Docker container
In order to deploy a local model with a docker container, you need to mount the model inside the container and specify the path in the container to the launch command.
Example:

```
git lfs install 
cd /tmp
mkdir models  cd models  git clone https://huggingface.co/BAAI/bge-small-en-v1.5
docker run -it   -v /tmp/models:/models  -p 8081:8081  michaelf34/infinity:latest v2  --model-id "/models/bge-small-en-v1.5" --port 8081
```

#### Advanced CLI usage
Launching multiple models at once
Since `infinity_emb>=0.0.34`, you can use cli `v2` method to launch multiple models at the same time. Checkout `infinity_emb v2 --help` for all args and validation.
Multiple Model CLI Playbook:
  *     1. cli options can be repeated e.g. `v2 --model-id model/id1 --model-id model/id2 --batch-size 8 --batch-size 4`. This will create two models `model/id1` and `model/id2`
  *     1. or adapt the defaults by setting ENV Variables separated by `;`: `INFINITY_MODEL_ID="model/id1;model/id2;" && INFINITY_BATCH_SIZE="8;4;"`
  *     1. single items are broadcasted to `--model-id` length, `v2 --model-id model/id1 --model-id/id2 --batch-size 8` making both models have batch-size 8.
  *     1. Everything is broadcasted to the number of `--model-id` + API requests are routed to the `--served-model-name/--model-id`

Using environment variables instead of the cli All CLI arguments are also launchable via environment variables. 
Environment variables start with `INFINITY_{UPPER_CASE_SNAKE_CASE}` and often match the `--{lower-case-kebab-case}` cli arguments.
The following two are equivalent:
  * CLI `infinity_emb v2 --model-id BAAI/bge-base-en-v1.5`
  * ENV-CLI: `export INFINITY_MODEL_ID="BAAI/bge-base-en-v1.5" && infinity_emb v2`


Multiple arguments can be used via `;` syntax: `INFINITY_MODEL_ID="model/id1;model/id2;"`
API Key Supply an `--api-key secret123` via CLI or ENV INFINITY_API_KEY="secret123".  Chosing the fastest engine
With the command `--engine torch` the model must be compatible with <https://github.com/UKPLab/sentence-transformers/> and AutoModel
With the command `--engine optimum`, there must be an onnx file. Models from <https://huggingface.co/Xenova> are recommended.
With the command `--engine ctranslate2` - only `BERT` models are supported.
Telemetry opt-out
See which telemetry is collected: <https://michaelfeil.eu/infinity/main/telemetry/>

```
# Disable
export INFINITY_ANONYMOUS_USAGE_STATS="0"

```

### Supported Tasks and Models by Infinity
Infinity aims to be the inference server supporting most functionality for embeddings, reranking and related RAG tasks. The following Infinity tests 15+ architectures and all of the below cases in the Github CI. Click on the sections below to find tasks and **validated example models**.
Text Embeddings
Text embeddings measure the relatedness of text strings. Embeddings are used for search, clustering, recommendations. Think about a private deployed version of openai's text embeddings. <https://platform.openai.com/docs/guides/embeddings>
Tested embedding models:
  * [mixedbread-ai/mxbai-embed-large-v1](https://huggingface.co/mixedbread-ai/mxbai-embed-large-v1)
  * [WhereIsAI/UAE-Large-V1](https://huggingface.co/WhereIsAI/UAE-Large-V1)
  * [BAAI/bge-base-en-v1.5](https://huggingface.co/BAAI/bge-base-en-v1.5)
  * [Alibaba-NLP/gte-large-en-v1.5](https://huggingface.co/Alibaba-NLP/gte-large-en-v1.5)
  * [jinaai/jina-embeddings-v2-base-code](https://huggingface.co/jinaai/jina-embeddings-v2-base-code)
  * [sentence-transformers/all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2)
  * [intfloat/multilingual-e5-large-instruct](https://huggingface.co/intfloat/multilingual-e5-large-instruct)
  * [intfloat/multilingual-e5-small](https://huggingface.co/intfloat/multilingual-e5-small)
  * [jinaai/jina-embeddings-v3](https://github.com/michaelfeil/infinity/blob/main/nomic-ai/nomic-embed-text-v1.5)
  * [BAAI/bge-m3, no sparse](https://huggingface.co/BAAI/bge-m3)
  * decoder-based models. Keep in mind that they are ~20-100x larger (&slower) than bert-small models: 
    * [Alibaba-NLP/gte-Qwen2-1.5B-instruct](https://huggingface.co/Alibaba-NLP/gte-Qwen2-1.5B-instruct/discussions/20)
    * [Salesforce/SFR-Embedding-2_R](https://huggingface.co/Salesforce/SFR-Embedding-2_R/discussions/6)
    * [Alibaba-NLP/gte-Qwen2-7B-instruct](https://huggingface.co/Alibaba-NLP/gte-Qwen2-7B-instruct/discussions/39)


Other models:
  * Most embedding model are likely supported: [https://huggingface.co/models?pipeline_tag=feature-extraction&other=text-embeddings-inference&sort=trending](https://huggingface.co/models?pipeline_tag=feature-extraction&other=text-embeddings-inference&sort=trending)
  * Check MTEB leaderboard for models <https://huggingface.co/spaces/mteb/leaderboard>.

Reranking Given a query and a list of documents, Reranking indexes the documents from most to least semantically relevant to the query. Think like a locally deployed version of <https://docs.cohere.com/reference/rerank>
Tested reranking models:
  * [mixedbread-ai/mxbai-rerank-xsmall-v1](https://huggingface.co/mixedbread-ai/mxbai-rerank-xsmall-v1)
  * [Alibaba-NLP/gte-multilingual-reranker-base](https://huggingface.co/Alibaba-NLP/gte-multilingual-reranker-base)
  * [BAAI/bge-reranker-base](https://huggingface.co/BAAI/bge-reranker-base)
  * [BAAI/bge-reranker-large](https://huggingface.co/BAAI/bge-reranker-large)
  * [BAAI/bge-reranker-v2-m3](https://huggingface.co/BAAI/bge-reranker-v2-m3)
  * [jinaai/jina-reranker-v1-turbo-en](https://huggingface.co/jinaai/jina-reranker-v1-turbo-en)


Other reranking models:
  * Reranking Models supported by infinity are bert-style classification Models with one category.
  * Most reranking model are likely supported: [https://huggingface.co/models?pipeline_tag=text-classification&other=text-embeddings-inference&sort=trending](https://huggingface.co/models?pipeline_tag=text-classification&other=text-embeddings-inference&sort=trending)
  * [https://huggingface.co/models?pipeline_tag=text-classification&sort=trending&search=rerank](https://huggingface.co/models?pipeline_tag=text-classification&sort=trending&search=rerank)

Multi-modal and cross-modal - image and audio embeddings Specialized embedding models that allow for image<->text or image<->audio search. Typically, these models allow for text<->text, text<->other and other<->other search, with accuracy tradeoffs when going cross-modal. 
Image<->text models can be used for e.g. photo-gallery search, where users can type in keywords to find photos, or use a photo to find related images. Audio<->text models are less popular, and can be e.g. used to find music songs based on a text description or related music songs.
Tested image<->text models:
  * [wkcn/TinyCLIP-ViT-8M-16-Text-3M-YFCC15M](https://huggingface.co/wkcn/TinyCLIP-ViT-8M-16-Text-3M-YFCC15M)
  * [jinaai/jina-clip-v1](https://huggingface.co/jinaai/jina-clip-v1)
  * [google/siglip-so400m-patch14-384](https://huggingface.co/google/siglip-so400m-patch14-384)
  * Models of type: ClipModel / SiglipModel in `config.json`


Tested audio<->text models:
  * limited number open source organizations training these models
  *     * Note: The sampling rate of the audio data needs to match the model *


Not supported:
  * Plain vision models e.g. nomic-ai/nomic-embed-vision-v1.5

ColBert-style late-interaction Embeddings ColBert Embeddings don't perform any special Pooling methods, but return the raw **token embeddings**. The **token embeddings** are then to be scored with the MaxSim Metric in a VectorDB (Qdrant / Vespa) 
For usage via the RestAPI, late-interaction embeddings may best be transported via `base64` encoding. Example notebook: <https://colab.research.google.com/drive/14FqLc0N_z92_VgL_zygWV5pJZkaskyk7?usp=sharing>
Tested colbert models:
  * [colbert-ir/colbertv2.0](https://huggingface.co/colbert-ir/colbertv2.0)
  * [jinaai/jina-colbert-v2](https://huggingface.co/jinaai/jina-colbert-v2)
  * [mixedbread-ai/mxbai-colbert-large-v1](https://huggingface.co/mixedbread-ai/mxbai-colbert-large-v1)
  * [answerai-colbert-small-v1 - click link for instructions](https://huggingface.co/answerdotai/answerai-colbert-small-v1/discussions/14)

ColPali-style late-interaction Image<->Text Embeddings Similar usage to ColBert, but scanning over an image<->text instead of only text. 
For usage via the RestAPI, late-interaction embeddings may best be transported via `base64` encoding. Example notebook: <https://colab.research.google.com/drive/14FqLc0N_z92_VgL_zygWV5pJZkaskyk7?usp=sharing>
Tested ColPali/ColQwen models:
  * [vidore/colpali-v1.2-merged](https://huggingface.co/michaelfeil/colpali-v1.2-merged)
  * [michaelfeil/colqwen2-v0.1](https://huggingface.co/michaelfeil/colqwen2-v0.1)
  * No lora adapters supported, only "merged" models.

Text classification A bert-style multi-label text classification. Classifies it into distinct categories. 
Tested models:
  * [ProsusAI/finbert](https://huggingface.co/ProsusAI/finbert), financial news classification
  * [SamLowe/roberta-base-go_emotions](https://huggingface.co/SamLowe/roberta-base-go_emotions), text to emotion categories.
  * bert-style text-classifcation models with more than >1 label in `config.json`


### Infinity usage via the Python API
Instead of the cli & RestAPI use infinity's interface via the Python API. This gives you most flexibility. The Python API builds on `asyncio` with its `await/async` features, to allow concurrent processing of requests. Arguments of the CLI are also available via Python.
#### Embeddings

```
import asyncio
from infinity_emb import AsyncEngineArray, EngineArgs, AsyncEmbeddingEngine

sentences = ["Embed this is sentence via Infinity.", "Paris is in France."]
array = AsyncEngineArray.from_args([
  EngineArgs(model_name_or_path = "BAAI/bge-small-en-v1.5", engine="torch", embedding_dtype="float32", dtype="auto")
])

async def embed_text(engine: AsyncEmbeddingEngine): 
    async with engine: 
        embeddings, usage = await engine.embed(sentences=sentences)
    # or handle the async start / stop yourself.
    await engine.astart()
    embeddings, usage = await engine.embed(sentences=sentences)
    await engine.astop()
asyncio.run(embed_text(array[0]))
```

#### Reranking
Reranking gives you a score for similarity between a query and multiple documents. Use it in conjunction with a VectorDB+Embeddings, or as standalone for small amount of documents. Please select a model from huggingface that is a AutoModelForSequenceClassification compatible model with one class classification.

```
import asyncio
from infinity_emb import AsyncEngineArray, EngineArgs, AsyncEmbeddingEngine
query = "What is the python package infinity_emb?"
docs = ["This is a document not related to the python package infinity_emb, hence...", 
    "Paris is in France!",
    "infinity_emb is a package for sentence embeddings and rerankings using transformer models in Python!"]
array = AsyncEmbeddingEngine.from_args(
  [EngineArgs(model_name_or_path = "mixedbread-ai/mxbai-rerank-xsmall-v1", engine="torch")]
)

async def rerank(engine: AsyncEmbeddingEngine): 
    async with engine:
        ranking, usage = await engine.rerank(query=query, docs=docs)
        print(list(zip(ranking, docs)))
    # or handle the async start / stop yourself.
    await engine.astart()
    ranking, usage = await engine.rerank(query=query, docs=docs)
    await engine.astop()

asyncio.run(rerank(array[0]))
```

When using the CLI, use this command to launch rerankers:

```
infinity_emb v2 --model-id mixedbread-ai/mxbai-rerank-xsmall-v1
```

#### Image-Embeddings: CLIP models
CLIP models are able to encode images and text at the same time.

```
import asyncio
from infinity_emb import AsyncEngineArray, EngineArgs, AsyncEmbeddingEngine

sentences = ["This is awesome.", "I am bored."]
images = ["http://images.cocodataset.org/val2017/000000039769.jpg"]
engine_args = EngineArgs(
    model_name_or_path = "wkcn/TinyCLIP-ViT-8M-16-Text-3M-YFCC15M", 
    engine="torch"
)
array = AsyncEngineArray.from_args([engine_args])

async def embed(engine: AsyncEmbeddingEngine): 
    await engine.astart()
    embeddings, usage = await engine.embed(sentences=sentences)
    embeddings_image, _ = await engine.image_embed(images=images)
    await engine.astop()

asyncio.run(embed(array["wkcn/TinyCLIP-ViT-8M-16-Text-3M-YFCC15M"]))
```

#### Audio-Embeddings: CLAP models
CLAP models are able to encode audio and text at the same time.

```
import asyncio
from infinity_emb import AsyncEngineArray, EngineArgs, AsyncEmbeddingEngine
import requests
import soundfile as sf
import io

sentences = ["This is awesome.", "I am bored."]

url = "https://bigsoundbank.com/UPLOAD/wav/2380.wav"
raw_bytes = requests.get(url, stream=True).content

audios = [raw_bytes]
engine_args = EngineArgs(
    model_name_or_path = "laion/clap-htsat-unfused",
    dtype="float32", 
    engine="torch"

)
array = AsyncEngineArray.from_args([engine_args])

async def embed(engine: AsyncEmbeddingEngine): 
    await engine.astart()
    embeddings, usage = await engine.embed(sentences=sentences)
    embedding_audios = await engine.audio_embed(audios=audios)
    await engine.astop()

asyncio.run(embed(array["laion/clap-htsat-unfused"]))
```

#### Text Classification
Use text classification with Infinity's `classify` feature, which allows for sentiment analysis, emotion detection, and more classification tasks.

```
import asyncio
from infinity_emb import AsyncEngineArray, EngineArgs, AsyncEmbeddingEngine

sentences = ["This is awesome.", "I am bored."]
engine_args = EngineArgs(
    model_name_or_path = "SamLowe/roberta-base-go_emotions", 
    engine="torch", model_warmup=True)
array = AsyncEngineArray.from_args([engine_args])

async def classifier(engine: AsyncEmbeddingEngine): 
    async with engine:
        predictions, usage = await engine.classify(sentences=sentences)
    # or handle the async start / stop yourself.
    await engine.astart()
    predictions, usage = await engine.classify(sentences=sentences)
    await engine.astop()
asyncio.run(classifier(array["SamLowe/roberta-base-go_emotions"]))
```

### Infinity usage via the Python Client
Infinity has a generated client code for RestAPI client side usage.
If you want to call a remote infinity instance via RestAPI, install the following package locally:

```
pip install infinity_client
```

For more information, check out the Client Readme <https://github.com/michaelfeil/infinity/tree/main/libs/client_infinity/infinity_client>
## Integrations:
  * [Serverless deployments at Runpod](https://github.com/runpod-workers/worker-infinity-embedding)
  * [Truefoundry Cognita](https://github.com/truefoundry/cognita)
  * [Langchain example](https://github.com/langchain-ai/langchain)
  * [imitater - A unified language model server built upon vllm and infinity.](https://github.com/the-seeds/imitater)
  * [Dwarves Foundation: Deployment examples using Modal.com](https://github.com/dwarvesf/llm-hosting)
  * [infiniflow/Ragflow](https://github.com/infiniflow/ragflow)
  * [gpt_server - gpt_server is an open-source framework designed for production-level deployment of LLMs (Large Language Models) or Embeddings.](https://github.com/shell-nlp/gpt_server)
  * [KubeAI: Kubernetes AI Operator for inferencing](https://github.com/substratusai/kubeai)
  * [Batched, modification of the Batching algoritm in Infinity](https://github.com/mixedbread-ai/batched)


## Documentation
View the docs at [https:///michaelfeil.github.io/infinity](https://michaelfeil.github.io/infinity) on how to get started. After startup, the Swagger Ui will be available under `{url}:{port}/docs`, in this case `http://localhost:7997/docs`. You can also find a interactive preview here: <https://infinity.modal.michaelfeil.eu/docs> (and <https://michaelfeil-infinity.hf.space/docs>)
## Contribute and Develop
Install via Poetry 1.8.1, Python3.11 on Ubuntu 22.04

```
cd libs/infinity_emb
poetry install --extras all --with lint,test
```

To pass the CI:

```
cd libs/infinity_emb
make precommit
```

All contributions must be made in a way to be compatible with the MIT License of this repo.
### Citation

```
@software{feil_2023_11630143,
  author       = {Feil, Michael},
  title        = {Infinity - To Embeddings and Beyond},
  month        = oct,
  year         = 2023,
  publisher    = {Zenodo},
  doi          = {10.5281/zenodo.11630143},
  url          = {https://doi.org/10.5281/zenodo.11630143}
}

```

### 💚 Current contributors 
## About
Infinity is a high-throughput, low-latency serving engine for text-embeddings, reranking models, clip, clap and colpali
[michaelfeil.github.io/infinity/](https://michaelfeil.github.io/infinity/)
### Topics
[bert-embeddings](https://github.com/topics/bert-embeddings)[llm](https://github.com/topics/llm)[text-embeddings](https://github.com/topics/text-embeddings)
### Resources
Cite this repository
### Stars
**3.0k** stars
### Watchers
**22** watching
### Forks
[ forks](https://github.com/michaelfeil/infinity/forks)
## 
[ 0.0.77Latest Aug 22, 2025 ](https://github.com/michaelfeil/infinity/releases/tag/0.0.77)
## 
## Languages
  * [Python96.5%](https://github.com/michaelfeil/infinity/search?l=python)
  * [Makefile1.4%](https://github.com/michaelfeil/infinity/search?l=makefile)
  * [Jinja1.1%](https://github.com/michaelfeil/infinity/search?l=jinja)
  * [Shell1%](https://github.com/michaelfeil/infinity/search?l=shell)


You can’t perform that action at this time. 
