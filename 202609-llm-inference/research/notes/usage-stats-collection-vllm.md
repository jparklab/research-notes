---
title: Usage Stats Collection - vLLM
id: usage-stats-collection-vllm
tags:
- onprem-llm-inference-stacks-859099
- air-gapped
- vendor-docs
- reliability-high
created: '2026-10-23T17:23:46.325677Z'
updated: '2026-10-23T17:25:09.122144Z'
source: https://docs.vllm.ai/en/latest/usage/usage_stats.html
source_domain: docs.vllm.ai
fetched_at: '2026-10-23T17:23:46.282508Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'vLLM docs: usage stats collection is on by default; opt-out via VLLM_NO_USAGE_STATS=1,
  DO_NOT_TRACK=1 or config file. Primary source, high reliability.'
---

*Suggested by [[best-air-gapped-llm-stack-apache-weights-on-vllm-beat-nvidias-fee-the-daily-brie]] — cited as primary source*

# Usage Stats Collection[¶](https://docs.vllm.ai/en/latest/usage/usage_stats/#usage-stats-collection "Permanent link")
vLLM collects anonymous usage data by default to help the engineering team better understand which hardware and model configurations are widely used. This data allows them to prioritize their efforts on the most common workloads. The collected data is transparent, does not contain any sensitive information.
A subset of the data, after cleaning and aggregation, will be publicly released for the community's benefit. For example, you can see the 2024 usage report [here](https://2024.vllm.ai).
## What data is collected?[¶](https://docs.vllm.ai/en/latest/usage/usage_stats/#what-data-is-collected "Permanent link")
The list of data collected by the latest version of vLLM can be found here: [ vllm/usage/usage_lib.py](https://github.com/vllm-project/vllm/blob/main/vllm/usage/usage_lib.py)
Here is an example as of v0.4.0:
Output

```
"uuid":"fbe880e9-084d-4cab-a395-8984c50f1109",
"provider":"GCP",
"num_cpu":24,
"cpu_type":"Intel(R) Xeon(R) CPU @ 2.20GHz",
"cpu_family_model_stepping":"6,85,7",
"total_memory":101261135872,
"architecture":"x86_64",
"platform":"Linux-5.10.0-28-cloud-amd64-x86_64-with-glibc2.31",
"gpu_count":2,
"gpu_type":"NVIDIA L4",
"gpu_memory_per_device":23580639232,
"model_architecture":"OPTForCausalLM",
"vllm_version":"0.3.2+cu123",
"context":"LLM_CLASS",
"log_time":1711663373492490000,
"source":"production",
"dtype":"torch.float16",
"tensor_parallel_size":1,
"block_size":16,
"gpu_memory_utilization":0.9,
"quantization":null,
"kv_cache_dtype":"auto",
"enable_lora":false,
"enable_prefix_caching":false,
"enforce_eager":false,
"disable_custom_all_reduce":true

```

You can preview the collected data by running the following command:

```
tail~/.config/vllm/usage_stats.json

```

## Opting out[¶](https://docs.vllm.ai/en/latest/usage/usage_stats/#opting-out "Permanent link")
You can opt out of usage stats collection by setting the `VLLM_NO_USAGE_STATS` or `DO_NOT_TRACK` environment variable, or by creating a `~/.config/vllm/do_not_track` file:

```
# Any of the following methods can disable usage stats collection
exportVLLM_NO_USAGE_STATS=1
exportDO_NOT_TRACK=1
mkdir-p~/.config/vllmtouch~/.config/vllm/do_not_track

```

Back to top 
