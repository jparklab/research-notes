---
title: KV Cache Offloading — production-stack
id: kv-cache-offloading-production-stack
tags:
- internal-llm-inference-stacks-7f0c90
- lmcache
- kv-cache
- vllm-production-stack
created: '2026-10-09T22:55:38.619980Z'
updated: '2026-10-10T14:52:17.086040Z'
source: https://docs.vllm.ai/projects/production-stack/en/vllm-stack-0.1.2/tutorials/kv_cache.html
source_domain: docs.vllm.ai
fetched_at: '2026-10-09T22:55:38.597964Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'vLLM Production Stack official tutorial (v0.1.2) demonstrating KV cache
  offloading via LMCache in a Kubernetes/Helm deployment. Confirms vLLM Production
  Stack uses LMCache (not a built-in mechanism) for moving KV caches from GPU memory
  to CPU/disk to increase cache-hit opportunities. Config example disables vLLM''s
  native chunked-prefill and prefix-caching (enableChunkedPrefill: false, enablePrefixCaching:
  false) and instead sets lmcacheConfig.enabled: true with cpuOffloadingBufferSize:
  20 (GB), using the lmcache/vllm-openai container image. Validation is via log lines
  from lmcache_connector.py and LMCacheEngine confirming LMCache (not vLLM) is handling
  the store/retrieve path.'
utility_score: 12.0
---

[Skip to main content](https://docs.vllm.ai/projects/production-stack/en/vllm-stack-0.1.2/tutorials/kv_cache.html#main-content)
Back to top
Search
  * [ Repository ](https://github.com/vllm-project/production-stack)
  * [ Suggest edit ](https://github.com/vllm-project/production-stack/edit/main/docs/source/tutorials/kv_cache.rst)




Light Dark System Settings
# KV Cache Offloading
##  Contents 
# KV Cache Offloading 
## Introduction 
This tutorial demonstrates how to enable KV cache offloading using LMCache in a vLLM deployment. KV cache offloading moves large KV caches from GPU memory to CPU or disk, enabling more potential KV cache hits. vLLM Production Stack uses LMCache for KV cache offloading.
## Prerequisites 
  * Installation of minimal example shown in [Minimal Example](https://docs.vllm.ai/projects/production-stack/en/vllm-stack-0.1.2/getting_started/examples.html#examples)
  * Kubernetes environment with GPU support


## Steps to follow 
### 1. Configuring KV Cache Offloading 
Locate the file `tutorials/assets/values-05-cpu-offloading.yaml` with the following content:

```
servingEngineSpec:
modelSpec:
-name:"mistral"
repository:"lmcache/vllm-openai"
tag:"latest"
modelURL:"mistralai/Mistral-7B-Instruct-v0.2"
replicaCount:1
requestCPU:10
requestMemory:"40Gi"
requestGPU:1
pvcStorage:"50Gi"
vllmConfig:
enableChunkedPrefill:false
enablePrefixCaching:false
maxModelLen:16384

lmcacheConfig:
enabled:true
cpuOffloadingBufferSize:"20"

hf_token:<YOUR HF TOKEN>

```
Copy to clipboard
Note
Note: Replace <YOUR HF TOKEN> with your actual Hugging Face token.
Note
The `lmcacheConfig` field enables LMCache and sets the CPU offloading buffer size to `20GB`. You can adjust this value based on your workload.
### Step 2: Deploy the Stack 
Deploy the Helm chart using the predefined configuration file:

```
helminstallvllmvllm/vllm-stack-ftutorials/assets/values-05-cpu-offloading.yaml

```
Copy to clipboard
### Step 3: Validate Installation 
#### 3.1 Check the pod logs to verify LMCache is active: 

```
sudokubectlgetpods

```
Copy to clipboard
Identify the pod name for the vLLM deployment (e.g., `vllm-mistral-deployment-vllm-xxxx-xxxx`). Then run:

```
sudokubectllogs-f<pod-name>

```
Copy to clipboard
Look for the following log message to confirm LMCache is active:

```
INFO 01-21 20:16:58 lmcache_connector.py:41] Initializing LMCacheConfig under kv_transfer_config kv_connector='LMCacheConnector' kv_buffer_device='cuda' kv_buffer_size=1000000000.0 kv_role='kv_both' kv_rank=None kv_parallel_size=1 kv_ip='127.0.0.1' kv_port=14579
INFO LMCache: Creating LMCacheEngine instance vllm-instance [2025-01-21 20:16:58,732] -- /usr/local/lib/python3.12/dist-packages/lmcache/experimental/cache_engine.py:237

```
Copy to clipboard
#### 2. Forward the router service port to access the stack locally: 

```
sudokubectlport-forwardsvc/vllm-router-service30080:80

```
Copy to clipboard
#### 3. Send a request to the stack and observe the logs: 

```
curl-XPOSThttp://localhost:30080/v1/completions\
-H"Content-Type: application/json"\
-d'{
        "model": "mistralai/Mistral-7B-Instruct-v0.2",
        "prompt": "Explain the significance of KV cache in language models.",
        "max_tokens": 10
    }'

```
Copy to clipboard
Expected output:
The response from the stack should contain the completion result, and the logs should show LMCache activity, for example:

```
DEBUG LMCache: Store skips 0 tokens and then stores 13 tokens [2025-01-21 20:23:45,113] -- /usr/local/lib/python3.12/dist-packages/lmcache/integration/vllm/vllm_adapter.py:490

```
Copy to clipboard
Contents 
