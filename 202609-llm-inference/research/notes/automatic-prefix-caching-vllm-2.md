---
title: Automatic Prefix Caching - vLLM
id: automatic-prefix-caching-vllm-2
tags:
- llmd-kv-cache-routing-c665d5
- vllm
- prefix-caching
- kv-cache
- derivative-of
created: '2026-09-14T11:56:05.372487Z'
updated: '2026-09-15T04:55:24.070213Z'
source: https://docs.vllm.ai/en/latest/features/automatic_prefix_caching/
source_domain: docs.vllm.ai
fetched_at: '2026-09-14T11:56:05.328028Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'vLLM''s official Automatic Prefix Caching (APC) feature docs, the foundational
  mechanism llm-d''s prefix-cache-aware routing depends on: APC caches KV cache of
  existing queries so new queries sharing the same prefix can skip recomputation,
  enabled via enable_prefix_caching=True. Documents a hybrid Mamba model extension:
  under --mamba-cache-mode align, Mamba state is stored only on the Mamba block grid
  so prefix-cache hits can resume only at block boundaries; --enable-mamba-fine-grained-prefix-cache
  adds a checkpoint at the shared-prefix junction so requests whose shared prefix
  ends mid-block can still reuse it, requiring --prefix-match-unit smaller than the
  Mamba block size plus EAGLE/MTP speculative decoding on the Mamba group. Cites two
  canonical use cases (long document query, multi-round conversation) and states APC''s
  key limitation: it only reduces prefill time, not decode time, so it brings no benefit
  when answers are long or new queries don''t share a prefix with cached ones.'
---

*Suggested by [[prefix-cache-aware-routing-llm-d-2]] — cited as the underlying vLLM mechanism prefix-cache-aware routing relies on*

# Automatic Prefix Caching[¶](https://docs.vllm.ai/en/latest/features/automatic_prefix_caching/#automatic-prefix-caching "Permanent link")
## Introduction[¶](https://docs.vllm.ai/en/latest/features/automatic_prefix_caching/#introduction "Permanent link")
Automatic Prefix Caching (APC in short) caches the KV cache of existing queries, so that a new query can directly reuse the KV cache if it shares the same prefix with one of the existing queries, allowing the new query to skip the computation of the shared part.
Note
Technical details on how vLLM implements APC can be found [here](https://docs.vllm.ai/en/latest/design/prefix_caching/).
## Enabling APC in vLLM[¶](https://docs.vllm.ai/en/latest/features/automatic_prefix_caching/#enabling-apc-in-vllm "Permanent link")
Set `enable_prefix_caching=True` in vLLM engine to enable APC. Here is an example:
[ examples/features/automatic_prefix_caching/automatic_prefix_caching_offline.py](https://github.com/vllm-project/vllm/blob/main/examples/features/automatic_prefix_caching/automatic_prefix_caching_offline.py)
## Hybrid Mamba models[¶](https://docs.vllm.ai/en/latest/features/automatic_prefix_caching/#hybrid-mamba-models "Permanent link")
Under `--mamba-cache-mode align`, Mamba state is stored only on the Mamba block grid, so a prefix-cache hit can resume only at a block boundary. `--enable-mamba-fine-grained-prefix-cache` also stores a checkpoint at the shared-prefix junction, the point where an earlier request with the same prefix stopped. Requests whose shared prefix ends inside a block can then reuse it.
This helps when many requests share a long system prompt and then diverge. It is off by default, and takes effect only when all of the following hold:
  * `--mamba-cache-mode align`
  * EAGLE/MTP speculative decoding on the Mamba group
  * `--prefix-match-unit` smaller than the Mamba block size
  * the model does not use multi-module MTP



```
vllmserve<hybrid-model>\
--mamba-cache-modealign\
--prefix-match-unit64\
--enable-mamba-fine-grained-prefix-cache

```

`--prefix-match-unit` is required. It sets the granularity at which prefix-cache keys are computed. When unset it defaults to the greatest common divisor of the prefix-cacheable KV cache group block sizes. Under `align` that is the block size itself, so no sub-block boundary exists and the flag has no effect.
Choose a value that divides the block size of every prefix-cacheable KV cache group, and that is a multiple of the per-state compression ratio for models that use one, such as sparse MLA. vLLM validates both at startup and names the offending sizes in the error. Read the served block size from the startup log. 64 is a reasonable starting point.
## Example workloads[¶](https://docs.vllm.ai/en/latest/features/automatic_prefix_caching/#example-workloads "Permanent link")
We describe two example workloads, where APC can provide huge performance benefit:
  * Long document query, where the user repeatedly queries the same long document (e.g. software manual or annual report) with different queries. In this case, instead of processing the long document again and again, APC allows vLLM to process this long document _only once_ , and all future requests can avoid recomputing this long document by reusing its KV cache. This allows vLLM to serve future requests with much higher throughput and much lower latency.
  * Multi-round conversation, where the user may chat with the application multiple times in the same chatting session. In this case, instead of processing the whole chatting history again and again, APC allows vLLM to reuse the processing results of the chat history across all future rounds of conversation, allowing vLLM to serve future requests with much higher throughput and much lower latency.


## Limits[¶](https://docs.vllm.ai/en/latest/features/automatic_prefix_caching/#limits "Permanent link")
APC in general does not reduce the performance of vLLM. With that being said, APC only reduces the time of processing the queries (the prefilling phase) and does not reduce the time of generating new tokens (the decoding phase). So APC does not bring performance gain when vLLM spends most of the time generating answers to the queries (e.g. when the length of the answer is long), or new queries do not share the same prefix with any of existing queries (so that the computation cannot be reused).
Back to top 
