---
title: gateway-api-inference-extension/docs/proposals/003-model-server-protocol/README.md
  at main · kubernetes-sigs/gateway-api-inference-extension · GitHub
id: gateway-api-inference-extensiondocsproposals003-model-server-protocolreadmemd-at
tags:
- llmd-router-nonk8s-backends
- model-server-protocol
- lora-metrics
- prometheus-metrics
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:43:14.036696Z'
updated: '2026-09-12T03:37:37.521657Z'
source: https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/docs/proposals/003-model-server-protocol/README.md
source_domain: github.com
fetched_at: '2026-09-11T19:43:14.017107Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GIE proposal 003, the Model Server Protocol (status: Partially implemented,
  notes it cannot be as strict since the pluggable EPP architecture landed). This
  is the exact contract any non-pod backend must satisfy to work with GIE''s endpoint
  picker. Requirements: (1) model server MUST implement OpenAI''s Completions and
  Chat HTTP APIs; (2) model server MUST expose a Prometheus-style metrics endpoint
  with these metrics (type + semantics fixed, names are flexible) -- TotalQueuedRequests
  (Gauge, vllm:num_requests_waiting / nv_trt_llm_request_metrics{request_type=waiting}
  / trtllm_num_requests_waiting / sglang:num_queue_reqs), TotalRunningRequests (Gauge,
  vllm:num_requests_running / nv_trt_llm_request_metrics{request_type=scheduled} /
  trtllm_num_requests_running / sglang:num_running_reqs), KVCacheUtilization (Gauge
  percentage, vllm:kv_cache_usage_perc / nv_trt_llm_kv_cache_block_metrics{kv_cache_block_type=fraction}
  / trtllm_kv_cache_utilization / sglang:token_usage), and optional BlockSize/NumGPUBlocks
  labeled gauges (used by prefix-cache scorer, derivable from EPP prefix-plugin config
  if absent). (3) For LoRA: model servers MUST support serving an adapter named in
  the request''s model field, and MUST expose a vllm:lora_requests_info-style Gauge
  with labels max_lora, running_lora_adapters, waiting_lora_adapters (comma-separated
  adapter lists) via the same Prometheus endpoint. (4) Prefix cache reuse (SHOULD,
  since EPP v0.4.0) for prefix-aware scheduling. Nothing in the protocol references
  Kubernetes objects directly -- it is purely an HTTP+Prometheus contract, which is
  the load-bearing fact for building a non-pod backend.'
utility_score: 14.0
---

[Skip to content](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/docs/proposals/003-model-server-protocol/README.md#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/docs/proposals/003-model-server-protocol/README.md) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/docs/proposals/003-model-server-protocol/README.md) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/docs/proposals/003-model-server-protocol/README.md) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/docs/proposals/003-model-server-protocol/README.md).
/ **[gateway-api-inference-extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension) You must be signed in to change notification settings
  * [ Fork 313 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)
  * [ Star  766 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# README.md
Copy path
More file actions
More file actions
## Latest commit
[Add prefix cache metric label overrides and gauge-style extraction fo…](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/25557e290ad41bec74d2313182df35d40b2c0cda)
Open commit detailssuccess
Apr 7, 2026
[25557e2](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/25557e290ad41bec74d2313182df35d40b2c0cda) · Apr 7, 2026
## History
[History](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commits/main/docs/proposals/003-model-server-protocol/README.md)
Open commit details
History
64 lines (46 loc) · 4.82 KB
/
# README.md
Copy path
## File metadata and controls
  * Preview
  * 

64 lines (46 loc) · 4.82 KB
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Outline
Edit and raw actions
# Model Server Protocol
This is the protocol between the EPP and the model servers.
## Proposal status
_**Partially implemented**_
Notes
  * With the creation of the [pluggable architecture](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/0683-epp-architecture-proposal) this protocol can, by definition, not be as strict


### Inference API Protocol
The model server MUST implement OpenAI’s [Completions](https://platform.openai.com/docs/api-reference/completions) and [Chat](https://platform.openai.com/docs/api-reference/chat) APIs.
### Metrics Reporting
The inference extension scrapes metrics from the model servers to make optimal request scheduling decisions. The model servers MUST provide the following metrics via a Prometheus endpoint. The exact metric names don't necessarily need to be the same as the recommended names here, however the metric types and semantics MUST follow this doc.
Note the requirements here are aligned with the [model server metrics standardization](https://docs.google.com/document/d/1SpSp1E6moa4HSrJnS4x3NpLuj88sMXr2tbofKlzTZpk) effort.  
| Metric  | Type  | Description  | vLLM metric  | Triton TensorRT-LLM  | trtllm-serve  | SGLang  |  
| --- | --- | --- | --- | --- | --- | --- |  
| TotalQueuedRequests  | Gauge  | The current total number of requests in the queue.  | `vllm:num_requests_waiting`  | `nv_trt_llm_request_metrics{request_type=waiting}`  | `trtllm_num_requests_waiting`  | `sglang:num_queue_reqs`  |  
| TotalRunningRequests  | Gauge  | The current total number of requests actively being served on the model server.  | `vllm:num_requests_running`  | `nv_trt_llm_request_metrics{request_type=scheduled}`  | `trtllm_num_requests_running`  | `sglang:num_running_reqs`  |  
| KVCacheUtilization  | Gauge  | The current KV cache utilization in percentage.  | `vllm:kv_cache_usage_perc`  | `nv_trt_llm_kv_cache_block_metrics{kv_cache_block_type=fraction}`  | `trtllm_kv_cache_utilization`  | `sglang:token_usage`  |  
| [Optional] BlockSize  | Labeled/Gauge  | The block size in tokens to allocate memory, used by the prefix cache scorer. If this metric is not available, the BlockSize will be derived from the [prefix plugin config](https://gateway-api-inference-extension.sigs.k8s.io/guides/epp-configuration/prefix-aware/#customize-the-prefix-cache-plugin).  | name: `vllm:cache_config_info`, label name: `block_size`  | `nv_trt_llm_kv_cache_block_metrics{kv_cache_block_type=tokens_per}`  | `trtllm_kv_cache_tokens_per_block`  | name: `sglang:cache_config_info`, label name: `page_size`  |  
| [Optional] NumGPUBlocks  | Labeled/Gauge  | The total number of blocks in the HBM KV cache, used by the prefix cache scorer. If this metric is not available, the NumGPUBlocks will be derived from the [prefix plugin config](https://gateway-api-inference-extension.sigs.k8s.io/guides/epp-configuration/prefix-aware/#customize-the-prefix-cache-plugin).  | name: `vllm:cache_config_info`, label name: `num_gpu_blocks`  | `nv_trt_llm_kv_cache_block_metrics{kv_cache_block_type=max}`  | `trtllm_kv_cache_max_blocks`  | name: `sglang:cache_config_info`, label name: `num_pages`  |  
### LoRA Adapter Serving
Model servers that support dynamic LoRA serving can benefit from the LoRA affinity algorithm. Note the current algorithm in the reference EPP is highly biased towards vLLM's current dynamic LoRA implementation.
The model servers MUST support serving a LoRA adapter specified in the `model` argument of the request, provided the requested adapter is valid.
The model server MUST expose the following LoRA adapter metrics via the same Prometheus endpoint:
  * Metric name implemented in vLLM: `vllm:lora_requests_info`
  * Metric type: Gauge
  * Metric value: The last updated timestamp (so the EPP can find the latest).
  * Metric labels: 
    * `max_lora`: The maximum number of adapters that can be loaded to GPU memory to serve a batch. Requests will be queued if the model server has reached MaxActiveAdapter and cannot load the requested adapter. Example: `"max_lora": "8"`.
    * `running_lora_adapters`: A comma separated list of adapters that are currently loaded in GPU memory and ready to serve requests. Example: `"running_lora_adapters": "adapter1, adapter2"`
    * `waiting_lora_adapters`: A comma separated list of adapters that are waiting to be served. Example: `"waiting_lora_adapters": "adapter1, adapter2"`


### Prefix Cache Reuse
Starting from [v0.4.0](https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v0.4.0), the EPP supports [prefix cache optimized request scheduling](https://gateway-api-inference-extension.sigs.k8s.io/guides/epp-configuration/prefix-aware/). To benefit from the optimal prefix aware request scheduling, model servers SHOULD support prefix cache reuse, such as the [vllm automatic prefix caching](https://docs.vllm.ai/en/latest/features/automatic_prefix_caching.html) feature.
You can’t perform that action at this time. 
