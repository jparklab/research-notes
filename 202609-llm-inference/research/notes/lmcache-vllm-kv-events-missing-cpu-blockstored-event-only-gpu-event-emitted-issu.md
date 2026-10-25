---
title: 'LMCache + vLLM KV Events: Missing CPU BlockStored Event (Only GPU event emitted)
  · Issue #3532 · LMCache/LMCache'
id: lmcache-vllm-kv-events-missing-cpu-blockstored-event-only-gpu-event-emitted-issu
tags:
- llmd-kv-cache-routing-c665d5
- locus-offload-connector-routing-visibility
- llm-d
- kv-cache
- zmq-kv-events
created: '2026-09-14T12:44:02.839128Z'
updated: '2026-09-15T04:55:24.130428Z'
source: https://github.com/LMCache/LMCache/issues/3532
source_domain: github.com
fetched_at: '2026-09-14T12:44:02.814646Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'LMCache/LMCache GitHub issue #3532 (filed Jun 2026): LMCache''s LMCacheConnectorV1Dynamic
  internally generates a CPU-medium BlockStored event but it never reaches the ZMQ
  subscriber -- only the GPU-tier event is published. Reporter cross-checked against
  vLLM''s own OffloadingConnector (no LMCache), which correctly publishes medium=''CPU''.
  Auto-closed ''not planned'' Sep 4 2026; fix appears to be arriving via LMCache''s
  newer MP-mode ZmqKVEventSink rather than a patch to the (deprecated) connector the
  bug was filed against.'
---

[Skip to content](https://github.com/LMCache/LMCache/issues/3532#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/LMCache/LMCache/issues/3532) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/LMCache/LMCache/issues/3532) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/LMCache/LMCache/issues/3532) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/LMCache/LMCache/issues/3532).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2FLMCache%2FLMCache) You must be signed in to change notification settings
  * [ Fork 1.9k ](https://github.com/login?return_to=%2FLMCache%2FLMCache)
  * [ Star  11.8k ](https://github.com/login?return_to=%2FLMCache%2FLMCache)


#  LMCache + vLLM KV Events: Missing CPU BlockStored Event (Only GPU event emitted)
Copy link
Copy link
Closed as not planned
Closed as not planned
[LMCache + vLLM KV Events: Missing CPU BlockStored Event (Only GPU event emitted)](https://github.com/LMCache/LMCache/issues/3532#top)#3532
Copy link
Labels
## Description
opened [on Jun 4, 2026](https://github.com/LMCache/LMCache/issues/3532#issue-4590065842)
Last edited by tonibofarull
Issue body actions
### Summary
When running vLLM with LMCache enabled (with `local_cpu: true`), I observe that KV cache events are only emitted for GPU VRAM. I expected separate BlockStored events for both GPU VRAM and CPU RAM, but only the GPU-side event appears in the kv-events topic.
### Environment
  * lmcache v0.4.4
  * vllm v0.19.1: custom build from <https://github.com/llm-d/llm-d/blob/v0.7.0/docker/common-versions#L5-L6>


### Configuration
vLLM args,

```
--kv-events-config={"publisher": "zmq", "enable_kv_cache_events": true, "topic":"kv-events"}
--kv-transfer-config={"kv_connector": "LMCacheConnectorV1Dynamic", "kv_role":"kv_both", "kv_connector_module_path": "lmcache.integration.vllm.lmcache_connector_v1"}
--block-size=128
--max-model-len=4096
--gpu-memory-utilization=0.3
--default-chat-template-kwargs={"enable_thinking": false}

```

LMCache,

```
chunk_size: 256
local_cpu: true
max_local_cpu_size: 4.0
enable_kv_events: true
pre_caching_hash_algorithm: sha256_cbor_64bit

```

### Details
From vLLM and LMCache, both in DEBUG level,

```
(APIServer pid=58) INFO:     Started server process [58]
(APIServer pid=58) INFO:     Waiting for application startup.
(APIServer pid=58) INFO:     Application startup complete.
(APIServer pid=58) DEBUG 06-04 15:12:10 [v1/metrics/loggers.py:259] Engine 000: Avg prompt throughput: 0.0 tokens/s, Avg generation throughput: 0.0 tokens/s, Running: 0 reqs, Waiting: 0 reqs, GPU KV cache usage: 0.0%, Prefix cache hit rate: 0.0%
(APIServer pid=58) WARNING 06-04 15:12:14 [tracing/utils.py:72] Received a request with trace context but tracing is disabled
(APIServer pid=58) DEBUG 06-04 15:12:14 [v1/sample/logits_processor/__init__.py:65] No logitsprocs plugins installed (group vllm.logits_processors).
(EngineCore pid=101) DEBUG 06-04 15:12:14 [v1/engine/core.py:1170] EngineCore loop active.
(EngineCore pid=101) [2026-06-04 15:12:14,333] LMCache DEBUG: Looking up cache for the first time for request chatcmpl-ec50eb40-4a0e-4611-982d-d2c2c9ef0376-9c5e420a! (vllm_v1_adapter.py:1263:lmcache.integration.vllm.vllm_v1_adapter)
(EngineCore pid=101) [2026-06-04 15:12:14,335] LMCache INFO: Reqid: chatcmpl-ec50eb40-4a0e-4611-982d-d2c2c9ef0376-9c5e420a, Total tokens 349, Inference Engine computed tokens: 0, LMCache hit tokens: 0, need to load: 0 (vllm_v1_adapter.py:1327:lmcache.integration.vllm.vllm_v1_adapter)
(EngineCore pid=101) DEBUG 06-04 15:12:14 [v1/worker/gpu_model_runner.py:3888] Running batch with cudagraph_mode: PIECEWISE, batch_descriptor: BatchDescriptor(num_tokens=352, num_reqs=None, uniform=False, has_lora=False, num_active_loras=0), should_ubatch: False, num_tokens_across_dp: None
(EngineCore pid=101) DEBUG 06-04 15:12:14 [v1/worker/gpu_model_runner.py:3909] ubatch_slices: None, ubatch_slices_padded: None
(EngineCore pid=101) [2026-06-04 15:12:14,663] LMCache DEBUG: Storing KV cache for 256 out of 256 tokens (skip_leading_tokens=0) for request chatcmpl-ec50eb40-4a0e-4611-982d-d2c2c9ef0376-9c5e420a (vllm_v1_adapter.py:1152:lmcache.integration.vllm.vllm_v1_adapter)
(EngineCore pid=101) [2026-06-04 15:12:14,663] LMCache DEBUG: Allocating memory in local cpu backend with busy loop: False (local_cpu_backend.py:515:lmcache.v1.storage_backend.local_cpu_backend)
(EngineCore pid=101) [2026-06-04 15:12:14,664] LMCache DEBUG: Added kv cache event 'CacheStoreEvent(block_hashes=[b'\xb9\x95\xb9\x06\xcciVqP\x007\xbc\xf3\xad\xd2(k\x98#\xcb.\xc4\x95gCX\xf8\xc5\xae@|\xa8'], parent_block_hash=None, token_ids=[151644, 8948, 198, 391, 5060, 16, 15, 25, 1446, 525, 264, 10950, 17847, 13, 4615, 2618, 374, 311, 4226, 4755, 9355, 323, 3529, 285, 974, 13, 1446, 2677, 3410, 13382, 1995, 323, 38296, 697, 32711, 13, 3197, 4588, 911, 3840, 11, 8038, 11, 37596, 11, 476, 894, 1008, 8544, 11, 498, 2968, 42666, 323, 4583, 11253, 429, 1492, 279, 1196, 3535, 279, 3832, 17247, 13, 1446, 2581, 1281, 2513, 705, 323, 2677, 24645, 26826, 979, 432, 6724, 13, 23240, 387, 3093, 323, 8720, 448, 279, 1196, 11, 323, 1281, 2704, 697, 40841, 525, 4135, 311, 1795, 13, 1446, 525, 458, 6203, 304, 1657, 5043, 2670, 21321, 11, 29803, 11, 33358, 11, 6366, 8038, 11, 17206, 11, 19128, 11, 27889, 11, 323, 279, 18560, 13, 1446, 1896, 21770, 304, 7086, 1632, 12, 51143, 14507, 429, 525, 2176, 38219, 323, 22570, 13, 1446, 10515, 697, 10535, 1707, 311, 7781, 279, 3880, 315, 1817, 3842, 1196, 11, 3425, 807, 525, 264, 48948, 476, 458, 6203, 304, 279, 8544, 518, 1424, 38437, 525, 264, 10950, 17847, 13, 4615, 2618, 374, 311, 4226, 4755, 9355, 323, 3529, 285, 974, 13, 1446, 2677, 3410, 13382, 1995, 323, 38296, 697, 32711, 13, 3197, 4588, 911, 3840, 11, 8038, 11, 37596, 11, 476, 894, 1008, 8544, 11, 498, 2968, 42666, 323, 4583, 11253, 429, 1492, 279, 1196, 3535, 279, 3832, 17247, 13, 1446, 2581, 1281, 2513, 705, 323, 2677, 24645, 26826, 979, 432, 6724, 13, 23240, 387, 3093, 323, 8720, 448, 279, 1196, 11, 323, 1281, 2704, 697, 40841, 525, 4135, 311, 1795, 13], block_size=256, lora_id=None, medium='cpu', lora_name=None)' to kv cache events queue (cache_engine.py:519:lmcache.v1.cache_engine)
(EngineCore pid=101) [2026-06-04 15:12:14,664] LMCache INFO: list_depth: 1, tensor_dim: 5 (utils.py:361:lmcache.v1.gpu_connector.utils)
(EngineCore pid=101) [2026-06-04 15:12:14,664] LMCache INFO: GPU KV Cache Dimensions: [28][2, 844, 128, 8, 128] (utils.py:374:lmcache.v1.gpu_connector.utils)
(EngineCore pid=101) [2026-06-04 15:12:14,665] LMCache INFO: vLLM KV cache layout: NHD (utils.py:388:lmcache.v1.gpu_connector.utils)
(EngineCore pid=101) [2026-06-04 15:12:14,665] LMCache INFO: GPU KV Format: NL x [2, NB, BS, NH, HS] (utils.py:315:lmcache.v1.gpu_connector.utils)
(EngineCore pid=101) [2026-06-04 15:12:14,665] LMCache INFO: Currently used by:
(EngineCore pid=101)   - vLLM non-MLA flash attention (utils.py:316:lmcache.v1.gpu_connector.utils)
(EngineCore pid=101) [2026-06-04 15:12:14,668] LMCache INFO: [req_id=chatcmpl-ec50eb40-4a0e-4611-982d-d2c2c9ef0376-9c5e420a] Stored 256 out of total 256 tokens. size: 0.0273 GB, cost 5.1004 ms, throughput: 5.3611 GB/s; offload_time: 4.8540 ms, put_time: 0.1609 ms (cache_engine.py:552:lmcache.v1.cache_engine)
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3888] Running batch with cudagraph_mode: FULL, batch_descriptor: BatchDescriptor(num_tokens=1, num_reqs=1, uniform=True, has_lora=False, num_active_loras=0), should_ubatch: False, num_tokens_across_dp: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3909] ubatch_slices: None, ubatch_slices_padded: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3888] Running batch with cudagraph_mode: FULL, batch_descriptor: BatchDescriptor(num_tokens=1, num_reqs=1, uniform=True, has_lora=False, num_active_loras=0), should_ubatch: False, num_tokens_across_dp: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3909] ubatch_slices: None, ubatch_slices_padded: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3888] Running batch with cudagraph_mode: FULL, batch_descriptor: BatchDescriptor(num_tokens=1, num_reqs=1, uniform=True, has_lora=False, num_active_loras=0), should_ubatch: False, num_tokens_across_dp: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3909] ubatch_slices: None, ubatch_slices_padded: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3888] Running batch with cudagraph_mode: FULL, batch_descriptor: BatchDescriptor(num_tokens=1, num_reqs=1, uniform=True, has_lora=False, num_active_loras=0), should_ubatch: False, num_tokens_across_dp: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3909] ubatch_slices: None, ubatch_slices_padded: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3888] Running batch with cudagraph_mode: FULL, batch_descriptor: BatchDescriptor(num_tokens=1, num_reqs=1, uniform=True, has_lora=False, num_active_loras=0), should_ubatch: False, num_tokens_across_dp: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3909] ubatch_slices: None, ubatch_slices_padded: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3888] Running batch with cudagraph_mode: FULL, batch_descriptor: BatchDescriptor(num_tokens=1, num_reqs=1, uniform=True, has_lora=False, num_active_loras=0), should_ubatch: False, num_tokens_across_dp: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3909] ubatch_slices: None, ubatch_slices_padded: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3888] Running batch with cudagraph_mode: FULL, batch_descriptor: BatchDescriptor(num_tokens=1, num_reqs=1, uniform=True, has_lora=False, num_active_loras=0), should_ubatch: False, num_tokens_across_dp: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3909] ubatch_slices: None, ubatch_slices_padded: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3888] Running batch with cudagraph_mode: FULL, batch_descriptor: BatchDescriptor(num_tokens=1, num_reqs=1, uniform=True, has_lora=False, num_active_loras=0), should_ubatch: False, num_tokens_across_dp: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3909] ubatch_slices: None, ubatch_slices_padded: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3888] Running batch with cudagraph_mode: FULL, batch_descriptor: BatchDescriptor(num_tokens=1, num_reqs=1, uniform=True, has_lora=False, num_active_loras=0), should_ubatch: False, num_tokens_across_dp: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3909] ubatch_slices: None, ubatch_slices_padded: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3888] Running batch with cudagraph_mode: FULL, batch_descriptor: BatchDescriptor(num_tokens=1, num_reqs=1, uniform=True, has_lora=False, num_active_loras=0), should_ubatch: False, num_tokens_across_dp: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3909] ubatch_slices: None, ubatch_slices_padded: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3888] Running batch with cudagraph_mode: FULL, batch_descriptor: BatchDescriptor(num_tokens=1, num_reqs=1, uniform=True, has_lora=False, num_active_loras=0), should_ubatch: False, num_tokens_across_dp: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3909] ubatch_slices: None, ubatch_slices_padded: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3888] Running batch with cudagraph_mode: FULL, batch_descriptor: BatchDescriptor(num_tokens=1, num_reqs=1, uniform=True, has_lora=False, num_active_loras=0), should_ubatch: False, num_tokens_across_dp: None
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/worker/gpu_model_runner.py:3909] ubatch_slices: None, ubatch_slices_padded: None
(EngineCore pid=101) [2026-06-04 15:12:15,174] LMCache DEBUG: In connector.start_load_kv, but the attn_metadata is None (vllm_v1_adapter.py:770:lmcache.integration.vllm.vllm_v1_adapter)
(EngineCore pid=101) DEBUG 06-04 15:12:15 [v1/engine/core.py:1158] EngineCore waiting for work.
(APIServer pid=58) DEBUG 06-04 15:12:15 [v1/engine/async_llm.py:875] Called check_health.
(APIServer pid=58) INFO 06-04 15:12:20 [v1/metrics/loggers.py:259] Engine 000: Avg prompt throughput: 34.9 tokens/s, Avg generation throughput: 1.2 tokens/s, Running: 0 reqs, Waiting: 0 reqs, GPU KV cache usage: 0.0%, Prefix cache hit rate: 0.0%, External prefix cache hit rate: 0.0%
(EngineCore pid=101) [2026-06-04 15:12:22,662] LMCache DEBUG: PinMonitor check: pinned_objects=0, timeout_objects=0, force_unpin_success=0 (pin_monitor.py:148:lmcache.v1.pin_monitor)

```

Using <https://github.com/vllm-project/vllm/blob/v0.22.0/examples/features/kv_events/kv_events_subscriber.py>,

```
Listening for KV cache events on topic: kv-events
Received event batch at 1780585934.7584138:
  - BlockStored(block_hashes=[11872861634006681712, 13920826077325614439], parent_block_hash=None, token_ids=[151644, 8948, 198, 391, 5060, 16, 15, 25, 1446, 525, 264, 10950, 17847, 13, 4615, 2618, 374, 311, 4226, 4755, 9355, 323, 3529, 285, 974, 13, 1446, 2677, 3410, 13382, 1995, 323, 38296, 697, 32711, 13, 3197, 4588, 911, 3840, 11, 8038, 11, 37596, 11, 476, 894, 1008, 8544, 11, 498, 2968, 42666, 323, 4583, 11253, 429, 1492, 279, 1196, 3535, 279, 3832, 17247, 13, 1446, 2581, 1281, 2513, 705, 323, 2677, 24645, 26826, 979, 432, 6724, 13, 23240, 387, 3093, 323, 8720, 448, 279, 1196, 11, 323, 1281, 2704, 697, 40841, 525, 4135, 311, 1795, 13, 1446, 525, 458, 6203, 304, 1657, 5043, 2670, 21321, 11, 29803, 11, 33358, 11, 6366, 8038, 11, 17206, 11, 19128, 11, 27889, 11, 323, 279, 18560, 13, 1446, 1896, 21770, 304, 7086, 1632, 12, 51143, 14507, 429, 525, 2176, 38219, 323, 22570, 13, 1446, 10515, 697, 10535, 1707, 311, 7781, 279, 3880, 315, 1817, 3842, 1196, 11, 3425, 807, 525, 264, 48948, 476, 458, 6203, 304, 279, 8544, 518, 1424, 38437, 525, 264, 10950, 17847, 13, 4615, 2618, 374, 311, 4226, 4755, 9355, 323, 3529, 285, 974, 13, 1446, 2677, 3410, 13382, 1995, 323, 38296, 697, 32711, 13, 3197, 4588, 911, 3840, 11, 8038, 11, 37596, 11, 476, 894, 1008, 8544, 11, 498, 2968, 42666, 323, 4583, 11253, 429, 1492, 279, 1196, 3535, 279, 3832, 17247, 13, 1446, 2581, 1281, 2513, 705, 323, 2677, 24645, 26826, 979, 432, 6724, 13, 23240, 387, 3093, 323, 8720, 448, 279, 1196, 11, 323, 1281, 2704, 697, 40841, 525, 4135, 311, 1795, 13], block_size=128, lora_id=None, medium='GPU', lora_name=None, extra_keys=[None, None], group_idx=None)

```

### vLLM default CPU offloader
To discard an incorrect configuration, I enabled the default CPU offloader (`OffloadingConnector`) as described in the [vLLM Blog](https://vllm.ai/blog/2026-01-08-kv-offloading-connector),

```
--kv-transfer-config={"kv_connector": "OffloadingConnector", "kv_role":"kv_both", "kv_connector_extra_config": {"block_size": 128, "cpu_bytes_to_use": 10737418240}}

```

In this case, the events of CPU appear,

```
Listening for KV cache events on topic: kv-events
[b'kv-events', b'\x00\x00\x00\x00\x00\x00\x00\x00', b'\x93\xcbA\xda\x88\x9dt\x9bP\xba\x91\x99\xabBlockStored\x92\xcf\xf1\x9bId\xba\xf9\x9a\xb5\xcfV\x03\xf9t!\x99\r!\xc0\xdc\x01\x00\xce\x00\x02P\\\xcd"\xf4\xcc\xc6\x10\x0f\x0f\x19\xcd\x05\xa6\xcd\x02\r\xcd\x01\x08\xcd*\xc6\xcdE\xb7\r\xcd\x12\x07\xcd\n:\xcd\x01v\xcd\x017\xcd\x10\x82\xcd\x12\x93\xcd$\x8b\xcd\x01C\xcd\r\xc9\xcd\x01\x1d\xcd\x03\xce\r\xcd\x05\xa6\xcd\nu\xcd\rR\xcd4F\xcd\x07\xcb\xcd\x01C\xcd\x95\x98\xcd\x02\xb9\xcd\x7f\xc7\r\xcd\x0c}\xcd\x11\xec\xcd\x03\x8f\xcd\x0f\x00\x0b\xcd\x1ff\x0b\xcd\x92\xdc\x0b\xcd\x01\xdc\xcd\x03~\xcd\x03\xf0\xcd!`\x0b\xcd\x01\xf2\xcd\x0b\x98\xcd\xa6\xaa\xcd\x01C\xcd\x11\xe7\xcd+\xf5\xcd\x01\xad\xcd\x05\xd4\xcd\x01\x17\xcd\x04\xac\xcd\r\xcf\xcd\x01\x17\xcd\x0e\xf8\xcdC_\r\xcd\x05\xa6\xcd\n\x15\xcd\x05\x01\xcd\t\xd1\xcd\x02\xc1\xcd\x01C\xcd\nu\xcd`E\xcdh\xca\xcd\x03\xd3\xcd\x01\xb0\xcd\x1aD\r\xcdZ\xc8\xcd\x01\x83\xcd\x0c\x15\xcd\x01C\xcd"\x10\xcd\x01\xc0\xcd\x01\x17\xcd\x04\xac\x0b\xcd\x01C\xcd\x05\x01\xcd\n\x90\xcd\x02\xb9\xcd\x9f\x89\xcd\x02\r\xcd\x10\'\xcd\x017\xcd\x07\x03\r\xcd\x05\xa6\xcd\x02\r\xcd\x01\xca\xcd\x18;\xcd\x010\xcd\x06y\xcd\x13\xb3\xcd\nn\xcdSI\x0b\xcdtk\x0b\xcd\x82N\x0b\xcd\x18\xde\xcd\x1ff\x0b\xcdC6\x0b\xcdJ\xb8\x0b\xcdl\xf1\x0b\xcd\x01C\xcd\x01\x17\xcdH\x80\r\xcd\x05\xa6\xcd\x07h\xcdU\n\xcd\x010\xcd\x1b\xae\xcd\x06`\x0c\xcd\xc7\xc7\xcd8\xab\xcd\x01\xad\xcd\x02\r\xcd\x08\x80\xcd\x95K\xcd\x01C\xcdX*\r\xcd\x05\xa6\xcd)\x13\xcd\x02\xb9\xcd)\'\xcd\x06\xab\xcd\x017\xcd\x1ee\xcd\x01\x17\xcd\x0f(\xcd\x01;\xcd\x07\x19\xcd\x0f\x02\xcd\x04\xac\x0b\xcd\ra\xcd\x03\'\xcd\x02\r\xcd\x01\x08\xcd\xbf4\xcd\x01\xdc\xcd\x01\xca\xcd\x18;\xcd\x010\xcd\x01\x17\xcd!`\xcd\x02\x06\xcd\x05\x90\xcd\x96%\xcd\x02\r\xcd\x01\x08\xcd*\xc6\xcdE\xb7\r\xcd\x12\x07\xcd\n:\xcd\x01v\xcd\x017\xcd\x10\x82\xcd\x12\x93\xcd$\x8b\xcd\x01C\xcd\r\xc9\xcd\x01\x1d\xcd\x03\xce\r\xcd\x05\xa6\xcd\nu\xcd\rR\xcd4F\xcd\x07\xcb\xcd\x01C\xcd\x95\x98\xcd\x02\xb9\xcd\x7f\xc7\r\xcd\x0c}\xcd\x11\xec\xcd\x03\x8f\xcd\x0f\x00\x0b\xcd\x1ff\x0b\xcd\x92\xdc\x0b\xcd\x01\xdc\xcd\x03~\xcd\x03\xf0\xcd!`\x0b\xcd\x01\xf2\xcd\x0b\x98\xcd\xa6\xaa\xcd\x01C\xcd\x11\xe7\xcd+\xf5\xcd\x01\xad\xcd\x05\xd4\xcd\x01\x17\xcd\x04\xac\xcd\r\xcf\xcd\x01\x17\xcd\x0e\xf8\xcdC_\r\xcd\x05\xa6\xcd\n\x15\xcd\x05\x01\xcd\t\xd1\xcd\x02\xc1\xcd\x01C\xcd\nu\xcd`E\xcdh\xca\xcd\x03\xd3\xcd\x01\xb0\xcd\x1aD\r\xcdZ\xc8\xcd\x01\x83\xcd\x0c\x15\xcd\x01C\xcd"\x10\xcd\x01\xc0\xcd\x01\x17\xcd\x04\xac\x0b\xcd\x01C\xcd\x05\x01\xcd\n\x90\xcd\x02\xb9\xcd\x9f\x89\xcd\x02\r\xcd\x10\'\xcd\x017\xcd\x07\x03\r\xcd\x05\xa6\xcc\x80\xc0\xa3GPU\xc0\x92\xc0\xc0\x00']
[b'kv-events', b'\x00\x00\x00\x00\x00\x00\x00\x01', b"\x93\xcbA\xda\x88\x9dt\xb8\xael\x91\x99\xabBlockStored\x92\xc4 \xc74\xb1\xc1KE\xcb\x8b=\xa2)RJ\xfc2G&\xad\xed(/^\xb8\xe8\xf1\x9bId\xba\xf9\x9a\xb5\xc4 \xfcM%\xc5\xbcgju\x13y\t\xbb'\x8a\x13\x9dO\xce\xb5tm\x8fW\xbeV\x03\xf9t!\x99\r!\xc0\x90\xcc\x80\xc0\xa3CPU\xc0\xc0\x00"]

```

Using,

```
import zmq

def main():
    context = zmq.Context()

    sub = context.socket(zmq.SUB)
    sub.connect("tcp://localhost:5557")
    topic = "kv-events"
    sub.setsockopt_string(zmq.SUBSCRIBE, topic)

    print("Listening for KV cache events on topic:", topic)

    while True:
        if sub.poll(50):
            print(sub.recv_multipart())

if __name__ == "__main__":
    main()
```

**Expected Outcome / Goal**
I would expect 2 `BlockStored`. One for GPU VRAM and one for CPU RAM. This behavior should be consistent regardless of whether I use `OffloadingConnector` or `LMCacheConnectorV1Dynamic`.
**Actual Outcome (if applicable)**
When using `LMCacheConnectorV1Dynamic`, only GPU VRAM block is shown. Even though in the LMCache we see the `LMCache DEBUG: Added kv cache event` line.
## Activity
### tonibofarull commented on Jun 4, 2026 
Author
More actions
Hi [@hickeyma](https://github.com/hickeyma),
I came across your RFC in [#1846](https://github.com/LMCache/LMCache/issues/1846) and was wondering if you could help us 🙇‍♂️
Is this a known issue, or could it be a misconfiguration on our side with vLLM / LMCache?
Any guidance would be greatly appreciated. Thank you!
### github-actions commented on Aug 4, 2026 
[on Aug 4, 2026](https://github.com/LMCache/LMCache/issues/3532#issuecomment-5186833301) – with [GitHub Actions](https://help.github.com/en/actions)
Contributor
More actions
This issue has been automatically marked as stale because it has not had activity within 60 days. It will be automatically closed if no further activity occurs within 30 days.
[github-actions](https://github.com/apps/github-actions)
added [on Aug 4, 2026](https://github.com/LMCache/LMCache/issues/3532#event-28984736818)
[rebel-jinhwan](https://github.com/rebel-jinhwan)
added a commit that references this issue [on Aug 27, 2026](https://github.com/LMCache/LMCache/issues/3532#event-30104210737)
[[MP] Add ZmqKVEventSink: re-emit cache events in vLLM KV-event format…](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472)
### github-actions commented on Sep 4, 2026 
[on Sep 4, 2026](https://github.com/LMCache/LMCache/issues/3532#issuecomment-5548497219) – with [GitHub Actions](https://help.github.com/en/actions)
Contributor
More actions
This issue has been automatically closed due to inactivity. Please feel free to reopen if you feel it is still relevant!
[github-actions](https://github.com/apps/github-actions)
closed this as [not planned](https://github.com/LMCache/LMCache/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Anot-planned)[on Sep 4, 2026](https://github.com/LMCache/LMCache/issues/3532#event-30591589255)
[Sign up for free](https://github.com/signup?return_to=https://github.com/LMCache/LMCache/issues/3532)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/LMCache/LMCache/issues/3532)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
No type
### Projects
No projects
### Milestone
No milestone
### Relationships
None yet
### Development
No branches or pull requests
### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
