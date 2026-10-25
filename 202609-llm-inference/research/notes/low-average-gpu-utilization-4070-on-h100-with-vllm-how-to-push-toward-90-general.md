---
title: Low Average GPU Utilization (40–70%) on H100 with vLLM — How to Push Toward
  90%+? - General - vLLM Forums
id: low-average-gpu-utilization-4070-on-h100-with-vllm-how-to-push-toward-90-general
tags:
- onprem-llm-inference-stacks-859099
- utilization
created: '2026-10-23T18:15:24.129395Z'
updated: '2026-10-23T18:17:21.606179Z'
source: https://discuss.vllm.ai/t/low-average-gpu-utilization-40-70-on-h100-with-vllm-how-to-push-toward-90/2293
source_domain: discuss.vllm.ai
fetched_at: '2026-10-23T18:15:24.108662Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Forum anecdote: 40-70% avg GPU util, H100, vLLM, 0.5B model batch data-processing
  job; low relevance/authority.'
---

[ Skip to last reply ](https://discuss.vllm.ai/t/low-average-gpu-utilization-40-70-on-h100-with-vllm-how-to-push-toward-90/2293/2) [ Skip to top ](https://discuss.vllm.ai/t/low-average-gpu-utilization-40-70-on-h100-with-vllm-how-to-push-toward-90/2293/1)
[ Skip to main content ](https://discuss.vllm.ai/t/low-average-gpu-utilization-40-70-on-h100-with-vllm-how-to-push-toward-90/2293#main-container)
#  [ Low Average GPU Utilization (40–70%) on H100 with vLLM — How to Push Toward 90%+? ](https://discuss.vllm.ai/t/low-average-gpu-utilization-40-70-on-h100-with-vllm-how-to-push-toward-90/2293)
You have selected **0** posts.
[ select all ](https://discuss.vllm.ai/t/low-average-gpu-utilization-40-70-on-h100-with-vllm-how-to-push-toward-90/2293)
[ cancel selecting ](https://discuss.vllm.ai/t/low-average-gpu-utilization-40-70-on-h100-with-vllm-how-to-push-toward-90/2293)
[ Jan 31  ](https://discuss.vllm.ai/t/low-average-gpu-utilization-40-70-on-h100-with-vllm-how-to-push-toward-90/2293/1 "Jump to the first post")
1 / 2 
Jan 31 
[ Jan 31 ](https://discuss.vllm.ai/t/low-average-gpu-utilization-40-70-on-h100-with-vllm-how-to-push-toward-90/2293/2)
##  post by Noman654 on Jan 31 
[ Noman654 ](https://discuss.vllm.ai/u/noman654)
[ Jan 31 ](https://discuss.vllm.ai/t/low-average-gpu-utilization-40-70-on-h100-with-vllm-how-to-push-toward-90/2293 "Post date")
Hi everyone,
I’m running vLLM for large-scale inference on H100 GPUs, and I’m seeing lower-than-expected average GPU utilization.
vllm setup command `docker run -d \   --name vllm-dp8 \   --gpus all \   -p 8000:8000 \   --ipc=host \   -v /projects/data/downloads/nauman/lang_filter/nemov2:/workspace \   vllm/vllm-openai:latest \   --model EssentialAI/eai-distill-0.5b \   --dtype float16 \   --data-parallel-size 8 \   --gpu-memory-utilization 0.95 \   --max-num-seqs 4096 \   --max-num-batched-tokens 131072 \   --enable-chunked-prefill \   --enable-prefix-caching \   --disable-log-requests \   --disable-log-stats`
###  Setup
  * GPU: NVIDIA H100
  * Framework: vLLM (latest)
  * Serving via: OpenAI-compatible API
  * GPU Memory Utilization: ~90%
  * GPU Compute Utilization:
    * Peaks: ~70–90%
    * Average: ~40–70%


Repository (client + workload generator): <https://github.com/Noman654/Essential_ai_quality_classifier.git>
###  Goal
I’m trying to achieve **sustained ~90%+ GPU utilization** for inference-heavy workloads.
###  Current Behavior
  * Memory is mostly full, so KV cache is not the limiting factor.
  * Utilization fluctuates heavily.
  * GPU often waits between batches.
  * Increasing traffic only improves utilization slightly.


###  What I’ve Tried
  * Increasing max_num_seqs
  * Increasing max_num_batched_tokens
  * Adjusting concurrency on client side
  * Running multiple clients


Still, average utilization stays below ~70%.
687 views  4 links 
##  post by RunLLM on Jan 31 
Reply
###  Related topics   
Topic list, column headers with buttons are sortable.  
|  Topic   |  Replies   |  Views   |  Activity   |  
| --- | --- | --- | --- |  
|  [When running the benchmark with vllm 0.10.1 V1, the GPU usage was very high](https://discuss.vllm.ai/t/when-running-the-benchmark-with-vllm-0-10-1-v1-the-gpu-usage-was-very-high/1861)  |  
|  [Why vllm cannot fully use GPU in batch processing](https://discuss.vllm.ai/t/why-vllm-cannot-fully-use-gpu-in-batch-processing/305)  |  1.3k  |  
|  [VLLM 0.10.1 benchmark do not free memory](https://discuss.vllm.ai/t/vllm-0-10-1-benchmark-do-not-free-memory/1867)  |  
|  [vLLM Hihg CPU usage when doin nothing](https://discuss.vllm.ai/t/vllm-hihg-cpu-usage-when-doin-nothing/1168)  |  1.2k  |  
|  [Benchmarking vllm performance - Having trouble sending requests to vllm servers](https://discuss.vllm.ai/t/benchmarking-vllm-performance-having-trouble-sending-requests-to-vllm-servers/1429)  |  
[ Powered by Discourse ](https://discourse.org/powered-by)
