---
title: OpenAI-Compatible Frontend for Triton Inference Server — NVIDIA Triton Inference
  Server
id: openai-compatible-frontend-for-triton-inference-server-nvidia-triton-inference-s
tags:
- internal-llm-inference-stacks
- triton-inference-server
- vllm
- tensorrt-llm
- openai-api-compatibility
- oss-project-docs
created: '2026-10-09T23:02:30.597185Z'
updated: '2026-10-10T14:52:17.169630Z'
source: https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html
source_domain: docs.nvidia.com
fetched_at: '2026-10-09T23:02:30.576839Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'Official NVIDIA documentation (Triton Inference Server v2.73.0) for the
  OpenAI-Compatible Frontend -- the component Netflix (per the Netflix in-house-LLM-serving
  post in this batch) forked via git-subtree and patched to fix a silently-dropped
  response_format parameter. Confirms the frontend supports both vLLM and TensorRT-LLM
  as backends behind a single /v1/chat/completions, /v1/completions, and /v1/embeddings
  OpenAI-compatible surface; vLLM backend additionally supports embeddings via vLLM''s
  embedding-model list. Supports LoRA adapter selection at request time via a --lora-separator
  flag and <model>_<lora> naming convention, for both vLLM and TensorRT-LLM backends.
  Supports OpenAI-style tool calling (tools/tool_choice) via --tool-call-parser {llama3,mistral},
  implemented through vLLM''s guided/structured-decoding and TensorRT-LLM''s guided
  decoding respectively -- directly corroborating the mechanism Netflix''s logits-processor
  patch plugs into. Documents --model-control-mode=explicit for dynamic runtime model
  load/unload without server restart (useful for multi-model GPU sharing), per-endpoint
  access restriction via custom auth headers (--openai-restricted-api), and a configurable
  HTTP request body size limit (default 64 MiB). Does not mention or confirm the specific
  response_format bug Netflix describes -- the docs as fetched describe the intended/current
  behavior, not historical defects. High-authority primary/official documentation
  source (tier: ground_truth).'
---

*Suggested by [[in-house-llm-serving-at-netflix-by-ai-platforms-model-runtime-team-and-by-netfli]] — NVIDIA Triton's OpenAI-compatible frontend, which Netflix forked and patched -- primary docs for the component*

[Skip to main content](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#main-content)
Back to top
[ NVIDIA Triton Inference Server ](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/contents.html)
2.73.0 (current release)
[2.73.0 (current release)](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/index.htmlclient_guide/openai_readme.html)[2.72.0](https://docs.nvidia.com/deeplearning/triton-inference-server/archives/triton-inference-server-2720/user-guide/docsclient_guide/openai_readme.html)[2.71.0](https://docs.nvidia.com/deeplearning/triton-inference-server/archives/triton-inference-server-2710/user-guide/docsclient_guide/openai_readme.html)[2.70.0](https://docs.nvidia.com/deeplearning/triton-inference-server/archives/triton-inference-server-2700/user-guide/docsclient_guide/openai_readme.html)[2.69.0](https://docs.nvidia.com/deeplearning/triton-inference-server/archives/triton-inference-server-2690/user-guide/docsclient_guide/openai_readme.html)[2.68.0](https://docs.nvidia.com/deeplearning/triton-inference-server/archives/triton-inference-server-2680/user-guide/docsclient_guide/openai_readme.html)[2.67.0](https://docs.nvidia.com/deeplearning/triton-inference-server/archives/triton-inference-server-2670/user-guide/docsclient_guide/openai_readme.html)[2.66.0](https://docs.nvidia.com/deeplearning/triton-inference-server/archives/triton-inference-server-2660/user-guide/docsclient_guide/openai_readme.html)[2.65.0](https://docs.nvidia.com/deeplearning/triton-inference-server/archives/triton-inference-server-2650/user-guide/docsclient_guide/openai_readme.html)[2.64.0](https://docs.nvidia.com/deeplearning/triton-inference-server/archives/triton-inference-server-2640/user-guide/docsclient_guide/openai_readme.html)[2.63.0](https://docs.nvidia.com/deeplearning/triton-inference-server/archives/triton-inference-server-2630/user-guide/docsclient_guide/openai_readme.html)[2.62.0](https://docs.nvidia.com/deeplearning/triton-inference-server/archives/triton-inference-server-2620/user-guide/docsclient_guide/openai_readme.html)[older releases](https://docs.nvidia.com/deeplearning/triton-inference-server/archives/client_guide/openai_readme.html)
  * [ GitHub](https://github.com/triton-inference-server/server)


# OpenAI-Compatible Frontend for Triton Inference Server[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#openai-compatible-frontend-for-triton-inference-server "Link to this heading")
## Pre-requisites[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#pre-requisites "Link to this heading")
  1. Docker + NVIDIA Container Runtime
  2. A correctly configured `HF_TOKEN` for access to HuggingFace models.
     * The current examples and testing primarily use the [`meta-llama/Meta-Llama-3.1-8B-Instruct`](https://huggingface.co/meta-llama/Meta-Llama-3.1-8B-Instruct) model, but you can manually bring your own models and adjust accordingly.


## VLLM[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#vllm "Link to this heading")
  1. Launch the container and install dependencies:


  * Mounts the `~/.cache/huggingface` for re-use of downloaded models across runs, containers, etc.
  * Sets the [`HF_TOKEN`](https://huggingface.co/docs/huggingface_hub/en/package_reference/environment_variables#hftoken) environment variable to access gated models, make sure this is set in your local environment if needed.



```
dockerrun-it--net=host--gpusall--rm\
-v${HOME}/.cache/huggingface:/root/.cache/huggingface\
-eHF_TOKEN\
nvcr.io/nvidia/tritonserver:26.09-vllm-python-py3

```
Copy to clipboard
  1. Launch the OpenAI-compatible Triton Inference Server:



```
cd/opt/tritonserver/python/openai

# NOTE: Adjust the --tokenizer based on the model being used
python3openai_frontend/main.py--model-repositorytests/vllm_models--tokenizermeta-llama/Meta-Llama-3.1-8B-Instruct

```
Copy to clipboard
Example output

```
...
+-----------------------+---------+--------+
| Model                 | Version | Status |
+-----------------------+---------+--------+
| llama-3.1-8b-instruct | 1       | READY  | <- Correct Model Loaded in Triton
+-----------------------+---------+--------+
...
Found model: name='llama-3.1-8b-instruct', backend='vllm'
[WARNING] Adding CORS for the following origins: ['http://localhost']
INFO:     Started server process [126]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:9000 (Press CTRL+C to quit) <- OpenAI Frontend Started Successfully

```
Copy to clipboard
  1. Send a `/v1/chat/completions` request:


  * Note the use of is optional, but provides a nicely formatted output for JSON responses.



```
MODEL="llama-3.1-8b-instruct"
curl-shttp://localhost:9000/v1/chat/completions-H'Content-Type: application/json'-d'{
  "model": "'${MODEL}'",
  "messages": [{"role": "user", "content": "Say this is a test!"}]
}'|jq

```
Copy to clipboard
Example output

```
{
"id":"cmpl-0242093d-51ae-11f0-b339-e7480668bfbe",
"choices":[
{
"finish_reason":"stop",
"index":0,
"message":
{
"content":"This is only a test.",
"tool_calls":null,
"role":"assistant",
"function_call":null
},
"logprobs":null
}
],
"created":1750846825,
"model":"llama-3.1-8b-instruct",
"system_fingerprint":null,
"object":"chat.completion",
"usage":{
"completion_tokens":7,
"prompt_tokens":42,
"total_tokens":49
}
}

```
Copy to clipboard
  1. Send a `/v1/completions` request:


  * Note the use of is optional, but provides a nicely formatted output for JSON responses.



```
MODEL="llama-3.1-8b-instruct"
curl-shttp://localhost:9000/v1/completions-H'Content-Type: application/json'-d'{
  "model": "'${MODEL}'",
  "prompt": "Machine learning is"
}'|jq

```
Copy to clipboard
Example output

```
{
"id":"cmpl-58fba3a0-51ae-11f0-859d-e7480668bfbe",
"choices":[
{
"finish_reason":"stop",
"index":0,
"logprobs":null,
"text":" an amazing field that can truly understand the hidden patterns that exist in the data,"
}
],
"created":1750846970,
"model":"llama-3.1-8b-instruct",
"system_fingerprint":null,
"object":"text_completion",
"usage":{
"completion_tokens":16,
"prompt_tokens":4,
"total_tokens":20
}
}

```
Copy to clipboard
  1. Benchmark with `genai-perf`:


  * To install genai-perf in this container, see the instructions [here](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/perf_analyzer/genai-perf/README.html#install-genai-perf-ubuntu-24-04-python-3-10)
  * Or try using genai-perf from the [SDK container](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tritonserver)



```
MODEL="llama-3.1-8b-instruct"
TOKENIZER="meta-llama/Meta-Llama-3.1-8B-Instruct"
genai-perfprofile\
--model${MODEL}\
--tokenizer${TOKENIZER}\
--service-kindopenai\
--endpoint-typechat\
--urllocalhost:9000\
--streaming

```
Copy to clipboard
Example output

```
2024-10-14 22:43 [INFO] genai_perf.parser:82 - Profiling these models: llama-3.1-8b-instruct
2024-10-14 22:43 [INFO] genai_perf.wrapper:163 - Running Perf Analyzer : 'perf_analyzer -m llama-3.1-8b-instruct --async --input-data artifacts/llama-3.1-8b-instruct-openai-chat-concurrency1/inputs.json -i http --concurrency-range 1 --endpoint v1/chat/completions --service-kind openai -u localhost:9000 --measurement-interval 10000 --stability-percentage 999 --profile-export-file artifacts/llama-3.1-8b-instruct-openai-chat-concurrency1/profile_export.json'
                              NVIDIA GenAI-Perf | LLM Metrics
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━┳━━━━━━━━┳━━━━━━━━┳━━━━━━━━┳━━━━━━━━┳━━━━━━━━┓
┃                         Statistic ┃    avg ┃    min ┃    max ┃    p99 ┃    p90 ┃    p75 ┃
┡━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━╇━━━━━━━━╇━━━━━━━━╇━━━━━━━━╇━━━━━━━━╇━━━━━━━━┩
│          Time to first token (ms) │  71.66 │  64.32 │  86.52 │  76.13 │  74.92 │  73.26 │
│          Inter token latency (ms) │  18.47 │  18.25 │  18.72 │  18.67 │  18.61 │  18.53 │
│              Request latency (ms) │ 348.00 │ 274.60 │ 362.27 │ 355.41 │ 352.29 │ 350.66 │
│            Output sequence length │  15.96 │  12.00 │  16.00 │  16.00 │  16.00 │  16.00 │
│             Input sequence length │ 549.66 │ 548.00 │ 551.00 │ 550.00 │ 550.00 │ 550.00 │
│ Output token throughput (per sec) │  45.84 │    N/A │    N/A │    N/A │    N/A │    N/A │
│      Request throughput (per sec) │   2.87 │    N/A │    N/A │    N/A │    N/A │    N/A │
└───────────────────────────────────┴────────┴────────┴────────┴────────┴────────┴────────┘
2024-10-14 22:44 [INFO] genai_perf.export_data.json_exporter:62 - Generating artifacts/llama-3.1-8b-instruct-openai-chat-concurrency1/profile_export_genai_perf.json
2024-10-14 22:44 [INFO] genai_perf.export_data.csv_exporter:71 - Generating artifacts/llama-3.1-8b-instruct-openai-chat-concurrency1/profile_export_genai_perf.csv

```
Copy to clipboard
  1. Use the OpenAI python client directly:



```
fromopenaiimport OpenAI

client = OpenAI(
    base_url="http://localhost:9000/v1",
    api_key="EMPTY",
)

model = "llama-3.1-8b-instruct"
completion = client.chat.completions.create(
    model=model,
    messages=[
        {
            "role": "system",
            "content": "You are a helpful assistant.",
        },
        {"role": "user", "content": "What are LLMs?"},
    ],
    max_completion_tokens=256,
)

print(completion.choices[0].message.content)

```
Copy to clipboard
  1. Run tests (NOTE: The server should not be running, the tests will handle starting/stopping the server as necessary):



```
cd/opt/tritonserver/python/openai/
pipinstall-rrequirements-test.txt

pytest-vtests/

```
Copy to clipboard
### LoRA Adapters[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#lora-adapters "Link to this heading")
If the command line argument `--lora-separator=<separator_string>` is provided when starting the OpenAI Frontend, a LoRA adaptor listed in `multi_lora.json` may be selected by appending the LoRA name to the model name, separated by the LoRA separator, on the inference request in `<model_name><separator_string><lora_name>` format.
For example

```
# start server with model named gemma-2b
python3openai_frontend/main.py--lora-separator=_lora_...

# inference without LoRA
curl-shttp://localhost:9000/v1/completions-H'Content-Type: application/json'-d'{
  "model": "gemma-2b",
  "temperature": 0,
  "prompt": "When was the wheel invented?"
}'
{
...
"choices":[{..."text":"\n\nThe wheel was invented by the Sumerians in Mesopotamia around 350"}],
...
}

# inference with LoRA named doll
curl-shttp://localhost:9000/v1/completions-H'Content-Type: application/json'-d'{
  "model": "gemma-2b_lora_doll",
  "temperature": 0,
  "prompt": "When was the wheel invented?"
}'
{
...
"choices":[{..."text":"\n\nThe wheel was invented in Mesopotamia around 3500 BC.\n\n"}],
...
}

# inference with LoRA named sheep
curl-shttp://localhost:9000/v1/completions-H'Content-Type: application/json'-d'{
  "model": "gemma-2b_lora_sheep",
  "temperature": 0,
  "prompt": "When was the wheel invented?"
}'
{
...
"choices":[{..."text":"\n\nThe wheel was invented around 3000 BC in Mesopotamia.\n\n"}],
...
}

```
Copy to clipboard
When listing or retrieving model(s), the model id will include the LoRA name in the same `<model_name><separator_string><lora_name>` format for each LoRA adapter listed on the `multi_lora.json`. Note: The LoRA name inclusion is limited to locally stored models, inference requests are not limited though.
#### vLLM[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#id1 "Link to this heading")
See the [vLLM documentation](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/vllm_backend/docs/llama_multi_lora_tutorial.html) on how to serve a vLLM model with LoRA adapters.
#### TensorRT-LLM[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#tensorrt-llm "Link to this heading")
Similarly, see [TensorRT-LLM document](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/tensorrtllm_backend/docs/lora.html) on how to prepare LoRA-enabled TensorRT-LLM engines and generate LoRA tensors. The path of LoRA adapter in `multi_lora.json` is the directory of `model.lora_config.npy` and `model.lora_weights.npy` tensors.
For example
model repository

```
inflight_batcher_llm
├── postprocessing
|   ├── 1
|   |   └── model.py
|   └── config.pbtxt
├── preprocessing
|   ├── 1
|   |   └── model.py
|   └── config.pbtxt
├── tensorrt_llm
|   ├── 1
|   |   └── model.py
|   └── config.pbtxt
└── tensorrt_llm_bls
    ├── 1
    |   ├── Japanese-Alpaca-LoRA-7b-v0-weights
    |   |   ├── model.lora_config.npy
    |   |   └── model.lora_weights.npy
    |   ├── luotuo-lora-7b-0.1-weights
    |   |   ├── model.lora_config.npy
    |   |   └── model.lora_weights.npy
    |   ├── model.py
    |   └── multi_lora.json
    └── config.pbtxt

```
Copy to clipboard
multi_lora.json

```
{
  "doll": "inflight_batcher_llm/tensorrt_llm_bls/1/luotuo-lora-7b-0.1-weights",
  "sheep": "inflight_batcher_llm/tensorrt_llm_bls/1/Japanese-Alpaca-LoRA-7b-v0-weights"
}

```
Copy to clipboard
### Embedding Models[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#embedding-models "Link to this heading")
Currently, OpenAI-Compatible Frontend supports loading embedding models and embeddings endpoints via vLLM backend. Check [vLLM supported models](https://docs.vllm.ai/en/latest/models/supported_models.html#embedding) for all supported embedding models from vLLM.
  1. Launch the container and install dependencies:


  * Mounts the `~/.cache/huggingface` for re-use of downloaded models across runs, containers, etc.
  * Sets the [`HF_TOKEN`](https://huggingface.co/docs/huggingface_hub/en/package_reference/environment_variables#hftoken) environment variable to access gated models, make sure this is set in your local environment if needed.



```
dockerrun-it--net=host--gpusall--rm\
-v${HOME}/.cache/huggingface:/root/.cache/huggingface\
-eHF_TOKEN\
nvcr.io/nvidia/tritonserver:26.09-vllm-python-py3

```
Copy to clipboard
  1. Launch the OpenAI-compatible Triton Inference Server:



```
cd/opt/tritonserver/python/openai

# NOTE: Embeddings endpoint does not require "--tokenizer"
python3openai_frontend/main.py--model-repositorytests/vllm_embedding_models

```
Copy to clipboard
Example output

```
...
+------------------+---------+--------+
| Model            | Version | Status |
+------------------+---------+--------+
| all-MiniLM-L6-v2 | 1       | READY  | <- Correct Model Loaded in Triton
+------------------+---------+--------+
...
Found model: name='all-MiniLM-L6-v2', backend='vllm'
[WARNING] Adding CORS for the following origins: ['http://localhost']
INFO:     Started server process [133]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:9000 (Press CTRL+C to quit) <- OpenAI Frontend Started Successfully

```
Copy to clipboard
  1. Send a `/v1/embeddings` request:


  * Note the use of is optional, but provides a nicely formatted output for JSON responses.



```
MODEL="all-MiniLM-L6-v2"
curl-shttp://localhost:9000/v1/embeddings\
-H'Content-Type: application/json'\
-d'{
    "model": "'${MODEL}'",
    "input": "The food was delicious and the waiter...",
    "dimensions": 10,
    "encoding_format": "float"
  }'|jq

```
Copy to clipboard
Example output

```
{
"object":"list",
"data":[
{
"object":"embedding",
"embedding":[
-0.1914404183626175,
0.4000193178653717,
0.058502197265625,
0.18909454345703125,
-0.4690297544002533,
0.004936536308377981,
0.45893096923828125,
-0.31141534447669983,
0.18299102783203125,
-0.4907582700252533
],
"index":0
}
],
"model":"all-MiniLM-L6-v2",
"usage":{
"prompt_tokens":12,
"total_tokens":12
}
}

```
Copy to clipboard
## TensorRT-LLM[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#id2 "Link to this heading")
  1. Prepare your model repository for a TensorRT-LLM model, build the engine, etc. You can try any of the following options:


  *   * [TRT-LLM Backend Quickstart](https://github.com/triton-inference-server/tensorrtllm_backend?tab=readme-ov-file#quick-start)


  1. Launch the container:


  * Mounts the `~/.cache/huggingface` for re-use of downloaded models across runs, containers, etc.
  * Sets the [`HF_TOKEN`](https://huggingface.co/docs/huggingface_hub/en/package_reference/environment_variables#hftoken) environment variable to access gated models, make sure this is set in your local environment if needed.



```
dockerrun-it--net=host--gpusall--rm\
-v${HOME}/.cache/huggingface:/root/.cache/huggingface\
-eHF_TOKEN\
-eTRTLLM_ORCHESTRATOR=1\
nvcr.io/nvidia/tritonserver:26.09-trtllm-python-py3

```
Copy to clipboard
  1. Install dependencies inside the container:



```
# Install python bindings for tritonserver and tritonfrontend
pipinstall/opt/tritonserver/python/triton*.whl

# Install application requirements
gitclonehttps://github.com/triton-inference-server/server.git
cdserver/python/openai/
pipinstall-rrequirements.txt

```
Copy to clipboard
  1. Launch the OpenAI server:



```
# NOTE: Adjust the --tokenizer based on the model being used
python3openai_frontend/main.py--model-repositorypath/to/models--tokenizermeta-llama/Meta-Llama-3.1-8B-Instruct

```
Copy to clipboard
  1. Send a `/v1/chat/completions` request:


  * Note the use of is optional, but provides a nicely formatted output for JSON responses.



```
# MODEL should be the client-facing model name in your model repository for a pipeline like TRT-LLM.
# For example, this could also be "ensemble", or something like "gpt2" if generated from Triton CLI
MODEL="tensorrt_llm_bls"
curl-shttp://localhost:9000/v1/chat/completions-H'Content-Type: application/json'-d'{
  "model": "'${MODEL}'",
  "messages": [{"role": "user", "content": "Say this is a test!"}]
}'|jq

```
Copy to clipboard
Example output

```
{
"id":"cmpl-5ad4f860-bf13-11f0-b137-b75b7f0a8586",
"choices":[
{
"finish_reason":"stop",
"index":0,
"message":{
"content":"It looks like you're ready to see if I'm functioning properly. What would",
"tool_calls":null,
"role":"assistant",
"function_call":null
},
"logprobs":null
}
],
"created":1762875029,
"model":"tensorrt_llm_bls",
"system_fingerprint":null,
"object":"chat.completion",
"usage":{
"prompt_tokens":42,
"total_tokens":58,
"completion_tokens":16
}
}

```
Copy to clipboard
The other examples should be the same as vLLM, except that you should set `MODEL="tensorrt_llm_bls"` or `MODEL="ensemble"`, everywhere applicable as seen in the example request above.
## KServe Frontends[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#kserve-frontends "Link to this heading")
To support serving requests through both the OpenAI-Compatible and KServe Predict v2 frontends to the same running Triton Inference Server, the `tritonfrontend` python bindings are included for optional use in this application as well.
You can opt-in to including these additional frontends, assuming `tritonfrontend` is installed, with `--enable-kserve-frontends` like below:

```
python3 openai_frontend/main.py \
  --model-repository tests/vllm_models \
  --tokenizer meta-llama/Meta-Llama-3.1-8B-Instruct \
  --enable-kserve-frontends

```
Copy to clipboard
See `python3 openai_frontend/main.py --help` for more information on the available arguments and default values.
For more information on the `tritonfrontend` python bindings, see the docs [here](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/customization_guide/tritonfrontend.html).
## Model Management[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#model-management "Link to this heading")
The OpenAI-compatible frontend supports explicit model control, allowing you to dynamically load and unload models at runtime without restarting the server. This is particularly useful when hosting multiple large models on a shared GPU cluster and needing to swap models on demand.
### Model Control Mode[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#model-control-mode "Link to this heading")
Use `--model-control-mode` to specify how models are managed at startup and runtime. The default is `none`.  
| Mode  | Behavior  |  
| --- | --- |  
| `none` (default)  | All models in the repository are loaded at startup. Load/unload APIs are not available.  |  
| `explicit`  | No models are loaded at startup unless specified with `--load-model`. Load and unload are controlled via the management API.  |  
> [!NOTE] This matches the native `tritonserver --model-control-mode` behavior. See [Triton Model Management](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/user_guide/model_management.html) for more details.
### Load Models at Startup (Explicit Mode)[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#load-models-at-startup-explicit-mode "Link to this heading")
When using `--model-control-mode=explicit`, use `--load-model` to specify which models should be loaded at startup. It may be specified multiple times to load multiple models.
Example

```
# Start in explicit mode with no models loaded
python3openai_frontend/main.py\
--model-repository/path/to/models\
--tokenizermeta-llama/Meta-Llama-3.1-8B-Instruct\
--model-control-modeexplicit

# Start in explicit mode and load a specific model at startup
python3openai_frontend/main.py\
--model-repository/path/to/models\
--tokenizermeta-llama/Meta-Llama-3.1-8B-Instruct\
--model-control-modeexplicit\
--load-modelllama-3.1-8b-instruct

# Load multiple models at startup
python3openai_frontend/main.py\
--model-repository/path/to/models\
--tokenizermeta-llama/Meta-Llama-3.1-8B-Instruct\
--model-control-modeexplicit\
--load-modelmodel-a\
--load-modelmodel-b

# Load ALL models in the repository at startup
python3openai_frontend/main.py\
--model-repository/path/to/models\
--tokenizermeta-llama/Meta-Llama-3.1-8B-Instruct\
--model-control-modeexplicit\
--load-model'*'

```
Copy to clipboard
> [!IMPORTANT]
>   * `--load-model` requires `--model-control-mode=explicit`.
>   * `--load-model=*` can not be used together with loading specific models.
> 

### Dynamic Load / Unload API[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#dynamic-load-unload-api "Link to this heading")
Once the server is running in `explicit` mode, models can be loaded and unloaded at runtime via the following endpoints:  
| Method  | Endpoint  | Description  |  
| --- | --- | --- |  
| `/v1/models/{model_name}/load`  | Load a model. Blocks until model is fully loaded and ready.  |  
| `/v1/models/{model_name}/unload`  | Unload a model. Blocks until fully unloaded. In-flight requests complete before removal.  |  
Both endpoints return an error if `--model-control-mode` is not `explicit`.
#### Load a model[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#load-a-model "Link to this heading")

```
MODEL="llama-3.1-8b-instruct"
curl-s-XPOSThttp://localhost:9000/v1/models/${MODEL}/load|jq

```
Copy to clipboard
Example output

```
{
"id":"llama-3.1-8b-instruct",
"object":"model",
"created":1750000000,
"owned_by":"Triton Inference Server"
}

```
Copy to clipboard
#### Unload a model[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#unload-a-model "Link to this heading")

```
MODEL="llama-3.1-8b-instruct"
curl-s-XPOSThttp://localhost:9000/v1/models/${MODEL}/unload|jq

```
Copy to clipboard
Example output

```
{
"status":"success",
"model":"llama-3.1-8b-instruct"
}

```
Copy to clipboard
## Model Parallelism Support[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#model-parallelism-support "Link to this heading")
  * [x] vLLM ([EngineArgs](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/vllm_backend/README.html#using-the-vllm-backend))
    * ex: Configure `tensor_parallel_size: 2` in the [model.json](https://github.com/triton-inference-server/vllm_backend/blob/main/samples/model_repository/vllm_model/1/model.json)
  * [x] TensorRT-LLM ([Orchestrator Mode](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/tensorrtllm_backend/README.html#orchestrator-mode))
    * Set the following environment variable: `export TRTLLM_ORCHESTRATOR=1`
  * [ ] TensorRT-LLM ([Leader Mode](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/tensorrtllm_backend/README.html#leader-mode))
    * Not currently supported


## Tool Calling[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#tool-calling "Link to this heading")
The OpenAI frontend supports `tools` and `tool_choice` in the `v1/chat/completions` API. Please refer to the OpenAI API reference for more details about these parameters: [tools](https://platform.openai.com/docs/api-reference/chat/create#chat-create-tools), [tool_choice](https://platform.openai.com/docs/api-reference/chat/create#chat-create-tool_choice)
To enable the tool-calling feature, add the `--tool-call-parser {parser_name}` flag when starting the server. The two available parsers are `llama3` and `mistral`. The `llama3` parser supports tool-calling features for LLaMA 3.1, 3.2, and 3.3 models, while the `mistral` parser supports tool-calling features for the Mistral Instruct model.
Example for launching the OpenAI frontend with a tool call parser:

```
python3 openai_frontend/main.py \
  --model-repository tests/vllm_models \
  --tokenizer meta-llama/Meta-Llama-3.1-8B-Instruct \
  --tool-call-parser llama3

```
Copy to clipboard
During streaming, the tool-call parser re-parses the entire accumulated output with each new chunk. This means that very large tool-call arguments can significantly increase per-request CPU and memory usage. The `--max-tool-call-parse-bytes` flag sets a limit on the maximum number of bytes the streaming tool parser will process per request. If a streamed response exceeds this limit, the stream is truncated with finish_reason=”length” and backend inference is cancelled. The default limit is 128 KiB (131,072 bytes). Increase this value if you expect larger tool-call payloads. Example usage:

```
python3 openai_frontend/main.py \
  --model-repository tests/vllm_models \
  --tokenizer meta-llama/Meta-Llama-3.1-8B-Instruct \
  --tool-call-parser llama3 \
  --max-tool-call-parse-bytes 131072

```
Copy to clipboard
Example for making a tool calling request:

```
importjson
fromopenaiimport OpenAI


defget_current_weather(city: str, state: str, unit: "str"):
    return (
        "The weather in Dallas, Texas is 85 degrees fahrenheit. It is "
        "partly cloudly, with highs in the 90's."
    )

available_tools = {"get_current_weather": get_current_weather}

openai_api_key = "EMPTY"
openai_api_base = "http://localhost:9000/v1"

client = OpenAI(
    api_key=openai_api_key,
    base_url=openai_api_base,
)

model = "llama-3.1-8b-instruct" # change this to the model in the repository

tools = [
    {
        "type": "function",
        "function": {
            "name": "get_current_weather",
            "description": "Get the current weather in a given location",
            "parameters": {
                "type": "object",
                "properties": {
                    "city": {
                        "type": "string",
                        "description": "The city to find the weather for, e.g. 'San Francisco'",
                    },
                    "state": {
                        "type": "string",
                        "description": "the two-letter abbreviation for the state that the city is"
                        " in, e.g. 'CA' which would mean 'California'",
                    },
                    "unit": {
                        "type": "string",
                        "description": "The unit to fetch the temperature in",
                        "enum": ["celsius", "fahrenheit"],
                    },
                },
                "required": ["city", "state", "unit"],
            },
        },
    }
]

messages = [
    {
        "role": "system",
        "content": "You're a helpful assistant! Answer the users question best you can.",
    },
    {"role": "user", "content": "What is the weather in Dallas, Texas in Fahrenheit?"},
]

tool_calls = client.chat.completions.create(
    messages=messages, model=model, tools=tools, max_completion_tokens=128
)
function_name = tool_calls.choices[0].message.tool_calls[0].function.name
function_arguments = tool_calls.choices[0].message.tool_calls[0].function.arguments

print(f"function name: " f"{function_name}")
print(f"function arguments: {function_arguments}")
print(f"tool calling result: {available_tools[function_name](https://github.com/triton-inference-server/server/blob/main/docs/client_guide/**json.loads(function_arguments))}")

```
Copy to clipboard
Example output:

```
function name: get_current_weather
function arguments: {"city": "Dallas", "state": "TX", "unit": "fahrenheit"}
tool calling result: The weather in Dallas, Texas is 85 degrees fahrenheit. It is partly cloudly, with highs in the 90's.

```
Copy to clipboard
### Named Tool Calling[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#named-tool-calling "Link to this heading")
The OpenAI frontend supports named function calling, utilizing structured outputs in the vLLM backend and guided decoding in TensorRT-LLM backend. Users can specify one of the tools in `tool_choice` to force the model to select a specific tool for function calling.
> [!NOTE] For instructions on enabling guided decoding in the TensorRT-LLM backend, please refer to [this guide](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/tensorrtllm_backend/docs/guided_decoding.html)
Example for making a named tool calling request:

```
importjson
fromopenaiimport OpenAI


defget_current_weather(city: str, state: str, unit: "str"):
    return (
        "The weather in Dallas, Texas is 85 degrees fahrenheit. It is "
        "partly cloudly, with highs in the 90's."
    )

defget_n_day_weather_forecast(city: str, state: str, unit: str, num_days: int):
    return (
        f"The weather in Dallas, Texas is 85 degrees fahrenheit in next {num_days} days."
    )

available_tools = {"get_current_weather": get_current_weather,
                  "get_n_day_weather_forecast": get_n_day_weather_forecast}

openai_api_key = "EMPTY"
openai_api_base = "http://localhost:9000/v1"
client = OpenAI(
    api_key=openai_api_key,
    base_url=openai_api_base,
)
model = "llama-3.1-8b-instruct" # change this to the model in the repository
tools = [
    {
        "type": "function",
        "function": {
            "name": "get_current_weather",
            "description": "Get the current weather in a given location",
            "parameters": {
                "type": "object",
                "properties": {
                    "city": {
                        "type": "string",
                        "description": "The city to find the weather for, e.g. 'San Francisco'",
                    },
                    "state": {
                        "type": "string",
                        "description": "the two-letter abbreviation for the state that the city is"
                        " in, e.g. 'CA' which would mean 'California'",
                    },
                    "unit": {
                        "type": "string",
                        "description": "The unit to fetch the temperature in",
                        "enum": ["celsius", "fahrenheit"],
                    },
                },
                "required": ["city", "state", "unit"],
            },
        },
    },
    {
        "type": "function",
        "function": {
            "name": "get_n_day_weather_forecast",
            "description": "Get an N-day weather forecast",
            "parameters": {
                "type": "object",
                "properties": {
                    "city": {
                        "type": "string",
                        "description": "The city to find the weather for, "
                        "e.g. 'San Francisco'",
                    },
                    "state": {
                        "type": "string",
                        "description": "must the two-letter abbreviation for the state "
                        "that the city is in, e.g. 'CA' which would "
                        "mean 'California'",
                    },
                    "unit": {
                        "type": "string",
                        "description": "The unit to fetch the temperature in",
                        "enum": ["celsius", "fahrenheit"],
                    },
                    "num_days": {
                        "type": "integer",
                        "description": "The number of days to forecast",
                    },
                },
                "required": ["city", "state", "unit", "num_days"],
            },
        },
     }
]

tool_choice = {"function": {"name": "get_n_day_weather_forecast"}, "type": "function"}

messages = [
    {
        "role": "system",
        "content": "You're a helpful assistant! Answer the users question best you can.",
    },
    {"role": "user", "content": "What is the weather in Dallas, Texas in Fahrenheit?"},
]

tool_calls = client.chat.completions.create(
    messages=messages, model=model, tools=tools, tool_choice=tool_choice, max_completion_tokens=128
)
function_name = tool_calls.choices[0].message.tool_calls[0].function.name
function_arguments = tool_calls.choices[0].message.tool_calls[0].function.arguments

print(f"function name: {function_name}")
print(f"function arguments: {function_arguments}")
print(f"tool calling result: {available_tools[function_name](https://github.com/triton-inference-server/server/blob/main/docs/client_guide/**json.loads(function_arguments))}")

```
Copy to clipboard
Example output:

```
function name: get_n_day_weather_forecast
function arguments: {"city": "Dallas", "state": "TX", "unit": "fahrenheit", num_days: 1}
tool calling result: The weather in Dallas, Texas is 85 degrees fahrenheit in next 1 days.

```
Copy to clipboard
## Limit Endpoint Access[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#limit-endpoint-access "Link to this heading")
The OpenAI-compatible server supports restricting access to specific API endpoints through authentication headers. This feature allows you to protect sensitive endpoints while keeping others publicly accessible.
### Configuration[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#configuration "Link to this heading")
Use the `--openai-restricted-api` command-line argument to configure endpoint restrictions:

```
--openai-restricted-api API_1,API_2,... restricted-key restricted-value

```
Copy to clipboard
  * : A comma-separated list of APIs to be included in this group. Note that currently a given API is not allowed to be included in multiple groups. The following protocols / APIs are recognized:
    * **inference** : Chat completions and text completions endpoints
      * `POST /v1/chat/completions`
      * `POST /v1/completions`
    * **embedding** : Embedding endpoint
      * `POST /v1/embeddings`
    * **model-repository** : Model listing, information, and dynamic load/unload endpoints
      * `GET /v1/models`
      * `GET /v1/models/{model_name}`
      * `POST /v1/models/{model_name}/load`
      * `POST /v1/models/{model_name}/unload`
    * **metrics** : Server metrics endpoint
      * `GET /metrics`
    * **health** : Health check endpoint
      * `GET /health/ready`
  * **`restricted-key`**: The HTTP request header to be checked when a request is received.
  * **`restricted-value`**: The header value required to access the specified protocols.


### Examples[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#examples "Link to this heading")
#### Restrict Inference API Endpoints Only[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#restrict-inference-api-endpoints-only "Link to this heading")

```
--openai-restricted-api"inference api-key my-secret-key"

```
Copy to clipboard
Clients must include the header:

```
curl-H"api-key: my-secret-key"\
-XPOSThttp://localhost:9000/v1/chat/completions\
-d'{"model": "my-model", "messages": [{"role": "user", "content": "Hello"}]}'

```
Copy to clipboard
#### Restrict Multiple API Endpoints[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#restrict-multiple-api-endpoints "Link to this heading")

```
# Different authentication for different APIs
--openai-restricted-api"inference user-key user-secret"\
--openai-restricted-api"model-repository admin-key admin-secret"

# Multiple APIs in single argument with shared authentication
--openai-restricted-api"inference,model-repository shared-key shared-secret"

```
Copy to clipboard
## HTTP Request Body Size Limit[#](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_guide/openai_readme.html#http-request-body-size-limit "Link to this heading")
The frontend enforces a maximum request body size prior to JSON parsing. Requests that exceed this limit are rejected with an error response.
Use `--http-max-input-size` to configure the limit (default: `67108864` bytes / 64 MiB):

```
python3openai_frontend/main.py\
--model-repository/path/to/models\
--tokenizermeta-llama/Meta-Llama-3.1-8B-Instruct\
--http-max-input-size67108864

```
Copy to clipboard
The limit applies to all endpoints. Example error response:

```
{
"error":{
"message":"Request content size exceeds the maximum allowed input size of 67108864 bytes. Use --http-max-input-size to increase the limit.",
"type":"invalid_request_error",
"code":"content_too_large"
}
}

```
Copy to clipboard
On this page 
