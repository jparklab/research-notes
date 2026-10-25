---
title: Production Metrics - vLLM
id: production-metrics-vllm
tags:
- internal-llm-inference-stacks-7f0c90
- locus-observability-layer-oss-maturity
created: '2026-10-09T23:59:59.408890Z'
updated: '2026-10-10T14:52:17.258148Z'
source: https://docs.vllm.ai/en/latest/usage/metrics.html
source_domain: docs.vllm.ai
fetched_at: '2026-10-09T23:59:59.369205Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: vLLM exposes a number of metrics that can be used to monitor the health of
  the system. These metrics are exposed via ...
---

# Production Metrics[¶](https://docs.vllm.ai/en/latest/usage/metrics/#production-metrics "Permanent link")
vLLM exposes a number of metrics that can be used to monitor the health of the system. These metrics are exposed via the `/metrics` endpoint on the vLLM OpenAI compatible API server.
You can start the server using Python, or using [Docker](https://docs.vllm.ai/en/latest/deployment/docker/):

```
vllmserveunsloth/Llama-3.2-1B-Instruct

```

Then query the endpoint to get the latest metrics from the server:
Output

```
$ curlhttp://0.0.0.0:8000/metrics
# HELPvllm:iteration_tokens_totalHistogramofnumberoftokensperengine_step.
# TYPEvllm:iteration_tokens_totalhistogram
vllm:iteration_tokens_total_sum{model_name="unsloth/Llama-3.2-1B-Instruct"} 0.0
vllm:iteration_tokens_total_bucket{le="1.0",model_name="unsloth/Llama-3.2-1B-Instruct"} 3.0
vllm:iteration_tokens_total_bucket{le="8.0",model_name="unsloth/Llama-3.2-1B-Instruct"} 3.0
vllm:iteration_tokens_total_bucket{le="16.0",model_name="unsloth/Llama-3.2-1B-Instruct"} 3.0
vllm:iteration_tokens_total_bucket{le="32.0",model_name="unsloth/Llama-3.2-1B-Instruct"} 3.0
vllm:iteration_tokens_total_bucket{le="64.0",model_name="unsloth/Llama-3.2-1B-Instruct"} 3.0
vllm:iteration_tokens_total_bucket{le="128.0",model_name="unsloth/Llama-3.2-1B-Instruct"} 3.0
vllm:iteration_tokens_total_bucket{le="256.0",model_name="unsloth/Llama-3.2-1B-Instruct"} 3.0
vllm:iteration_tokens_total_bucket{le="512.0",model_name="unsloth/Llama-3.2-1B-Instruct"} 3.0

```

The following metrics are exposed:
## General Metrics[¶](https://docs.vllm.ai/en/latest/usage/metrics/#general-metrics "Permanent link")  
| Metric Name  | Type  | Description  |  
| --- | --- | --- |  
| `vllm:corrupted_requests`  | Counter  | Corrupted requests, in terms of total number of requests with NaNs in logits.  |  
| `vllm:external_prefix_cache_hits`  | Counter  | External prefix cache hits from KV connector cross-instance cache sharing, in terms of number of cached tokens.  |  
| `vllm:external_prefix_cache_queries`  | Counter  | External prefix cache queries from KV connector cross-instance cache sharing, in terms of number of queried tokens.  |  
| `vllm:generation_tokens`  | Counter  | Number of generation tokens processed.  |  
| `vllm:mm_cache_hits`  | Counter  | Multi-modal cache hits, in terms of number of cached items.  |  
| `vllm:mm_cache_queries`  | Counter  | Multi-modal cache queries, in terms of number of queried items.  |  
| `vllm:num_preemptions`  | Counter  | Cumulative number of preemption from the engine.  |  
| `vllm:prefix_cache_hits`  | Counter  | Prefix cache hits, in terms of number of cached tokens.  |  
| `vllm:prefix_cache_queries`  | Counter  | Prefix cache queries, in terms of number of queried tokens.  |  
| `vllm:prompt_tokens`  | Counter  | Number of prefill tokens processed.  |  
| `vllm:prompt_tokens_by_source`  | Counter  | Number of prompt tokens by source.  |  
| `vllm:prompt_tokens_cached`  | Counter  | Number of cached prompt tokens (local + external).  |  
| `vllm:prompt_tokens_cached_by_source`  | Counter  | Prefix-cache hit tokens by the cache tier that supplied them: device (local HBM), host (offloaded to DRAM), disk, p2p (transferred from another vLLM instance) or external_unspecified. Counted at admission; sums to vllm:prefix_cache_hits + vllm:external_prefix_cache_hits.  |  
| `vllm:request_success`  | Counter  | Count of successfully processed requests.  |  
| `vllm:engine_sleep_state`  | Gauge  | Engine sleep state; awake = 0 means engine is sleeping; awake = 1 means engine is awake; weights_offloaded = 1 means sleep level 1; discard_all = 1 means sleep level 2.  |  
| `vllm:kv_cache_usage_perc`  | Gauge  | KV-cache usage. 1 means 100 percent usage.  |  
| `vllm:lora_requests_info`  | Gauge  | Running stats on lora requests.  |  
| `vllm:num_requests_kv_fetch_by_stage`  | Gauge  | Number of waiting requests by async KV load stage. Stage labels: 'waiting_to_start' = needs an async KV load that has not started; 'in_progress' = load started and not yet reported finished; 'completed_waiting' = load finished, request not running yet.  |  
| `vllm:num_requests_running`  | Gauge  | Number of requests in model execution batches.  |  
| `vllm:num_requests_waiting`  | Gauge  | Number of requests waiting to be processed.  |  
| `vllm:num_requests_waiting_by_reason`  | Gauge  | Number of waiting requests by reason. Reason labels: 'capacity' = waiting for scheduling capacity; 'deferred' = deferred by transient constraints (LoRA budget, KV transfer, blocked status). Sum of all reasons equals vllm:num_requests_waiting.  |  
| `vllm:e2e_request_latency_seconds`  | Histogram  | Histogram of e2e request latency in seconds.  |  
| `vllm:inter_token_latency_seconds`  | Histogram  | Histogram of inter-token latency in seconds.  |  
| `vllm:iteration_tokens_total`  | Histogram  | Histogram of number of tokens per engine_step.  |  
| `vllm:kv_block_idle_before_evict_seconds`  | Histogram  | Histogram of idle time before KV cache block eviction. Sampled metrics (controlled by --kv-cache-metrics-sample).  |  
| `vllm:kv_block_lifetime_seconds`  | Histogram  | Histogram of KV cache block lifetime from allocation to eviction. Sampled metrics (controlled by --kv-cache-metrics-sample).  |  
| `vllm:kv_block_reuse_gap_seconds`  | Histogram  | Histogram of time gaps between consecutive KV cache block accesses. Only the most recent accesses are recorded (ring buffer). Sampled metrics (controlled by --kv-cache-metrics-sample).  |  
| `vllm:request_decode_time_seconds`  | Histogram  | Histogram of time spent in DECODE phase for request.  |  
| `vllm:request_generation_tokens`  | Histogram  | Number of generation tokens processed.  |  
| `vllm:request_inference_time_seconds`  | Histogram  | Histogram of time spent in RUNNING phase for request.  |  
| `vllm:request_max_num_generation_tokens`  | Histogram  | Histogram of maximum number of requested generation tokens.  |  
| `vllm:request_num_preemptions`  | Histogram  | Histogram of the number of times a request was preempted.  |  
| `vllm:request_params_max_tokens`  | Histogram  | Histogram of the max_tokens request parameter.  |  
| `vllm:request_params_n`  | Histogram  | Histogram of the n request parameter.  |  
| `vllm:request_prefill_kv_computed_tokens`  | Histogram  | Histogram of new KV tokens computed during prefill (excluding cached tokens).  |  
| `vllm:request_prefill_time_seconds`  | Histogram  | Histogram of time spent in PREFILL phase for request.  |  
| `vllm:request_prompt_tokens`  | Histogram  | Number of prefill tokens processed.  |  
| `vllm:request_queue_time_seconds`  | Histogram  | Histogram of time spent in WAITING phase for request.  |  
| `vllm:request_time_per_output_token_seconds`  | Histogram  | Histogram of time_per_output_token_seconds per request.  |  
| `vllm:time_to_first_token_seconds`  | Histogram  | Histogram of time to first token in seconds.  |  
## Speculative Decoding Metrics[¶](https://docs.vllm.ai/en/latest/usage/metrics/#speculative-decoding-metrics "Permanent link")  
| Metric Name  | Type  | Description  |  
| --- | --- | --- |  
| `vllm:spec_decode_num_accepted_tokens_per_pos`  | Counter  | Accepted tokens per draft position.  |  
## NIXL KV Connector Metrics[¶](https://docs.vllm.ai/en/latest/usage/metrics/#nixl-kv-connector-metrics "Permanent link")  
| Metric Name  | Type  | Description  |  
| --- | --- | --- |  
| `vllm:nixl_num_failed_notifications`  | Counter  | Number of failed NIXL KV Cache notifications. Retained for compatibility; these failures are also included in vllm:nixl_num_failed_transfers.  |  
| `vllm:nixl_num_failed_transfers`  | Counter  | Number of failed NIXL KV Cache transfers, including handshake and notification failures. NOTE: KV expiry is tracked separately in vllm:nixl_num_kv_expired_reqs.  |  
| `vllm:nixl_num_kv_expired_reqs`  | Counter  | Number of requests that had their KV expire. NOTE: This metric is tracked on the P instance.  |  
| `vllm:nixl_num_notifications_after_expiry`  | Counter  | Number of completion notifications for requests that were no longer tracked, usually because their KV lease expired. The KV blocks may have been reused before the transfer finished. Counted per notification (one per remote rank), not per request.  |  
| `vllm:nixl_bytes_transferred`  | Histogram  | Histogram of bytes transferred per NIXL KV Cache transfers.  |  
| `vllm:nixl_num_descriptors`  | Histogram  | Histogram of number of descriptors per NIXL KV Cache transfers.  |  
| `vllm:nixl_post_time_seconds`  | Histogram  | Histogram of transfer post time for NIXL KV Cache transfers.  |  
| `vllm:nixl_xfer_time_seconds`  | Histogram  | Histogram of transfer duration for NIXL KV Cache transfers.  |  
## HiSparse KV Connector Metrics[¶](https://docs.vllm.ai/en/latest/usage/metrics/#hisparse-kv-connector-metrics "Permanent link")
These metrics are exposed when the [`HiSparseConnector`](https://docs.vllm.ai/en/latest/api/vllm/distributed/kv_transfer/kv_connector/v1/hisparse/connector/#vllm.distributed.kv_transfer.kv_connector.v1.hisparse.connector.HiSparseConnector "            HiSparseConnector") KV connector is configured. The host-pool gauges are reported by the scheduler once per engine step, for the host pool shared by all tensor-parallel ranks; evictable cached blocks count as free. With `--kv-cache-metrics`, the `vllm:hisparse_host_block_*_seconds` histograms sample host-pool block residency, like the device `vllm:kv_block_*_seconds` histograms, which cover only the GPU pool.  
| Metric Name  | Type  | Description  |  
| --- | --- | --- |  
| `vllm:hisparse_cache_hits`  | Counter  | Number of HiSparse device hot-buffer hits.  |  
| `vllm:hisparse_cache_misses`  | Counter  | Number of HiSparse device hot-buffer misses.  |  
| `vllm:hisparse_host_to_device_bytes`  | Counter  | Bytes transferred from host KV storage to HiSparse hot buffers.  |  
| `vllm:hisparse_host_cache_usage_perc`  | Gauge  | HiSparse host KV-cache usage. 1 means 100 percent usage. Evictable cached blocks count as free.  |  
| `vllm:hisparse_pending_page_transfers`  | Gauge  | HiSparse page transfers (write-backs and restores) not yet completed.  |  
| `vllm:hisparse_host_block_idle_before_evict_seconds`  | Histogram  | Histogram of HiSparse host KV block idle time before eviction. Sampled metrics (controlled by --kv-cache-metrics-sample).  |  
| `vllm:hisparse_host_block_lifetime_seconds`  | Histogram  | Histogram of HiSparse host KV block lifetime from allocation to eviction. Sampled metrics (controlled by --kv-cache-metrics-sample).  |  
| `vllm:hisparse_host_block_reuse_gap_seconds`  | Histogram  | Histogram of time gaps between consecutive HiSparse host KV block accesses. Sampled metrics (controlled by --kv-cache-metrics-sample).  |  
## Simple CPU Offload Connector Metrics[¶](https://docs.vllm.ai/en/latest/usage/metrics/#simple-cpu-offload-connector-metrics "Permanent link")
These metrics are exposed when the [`SimpleCPUOffloadConnector`](https://docs.vllm.ai/en/latest/api/vllm/distributed/kv_transfer/kv_connector/v1/simple_cpu_offload_connector/#vllm.distributed.kv_transfer.kv_connector.v1.simple_cpu_offload_connector.SimpleCPUOffloadConnector "            SimpleCPUOffloadConnector") KV connector is configured (e.g. `--kv-transfer-config='{"kv_connector": "SimpleCPUOffloadConnector", "kv_role": "kv_both", "kv_connector_extra_config": {"kv_offload_backend": "disk", "disk_path": "/mnt/nvme/kv"}}'`). They are updated once per engine step.
Caveats to keep in mind when interpreting them:
  * A "completed" store means the write syscalls returned; it is **not** fsync-durable, and the disk backend's file is process-lifetime scratch, unlinked at startup and shutdown.
  * `save_outcomes_total` classifies eager-mode boundary hand-off stores; lazy-mode stores are not classified.
  * `used_blocks` counts blocks pinned by in-flight transfers or cache hits; warm cached blocks that are evictable are not counted. Use the `capacity_blocks` label of `simple_kv_offload_info` as the denominator.
  * Counters and gauges are quantized to engine steps. They are reported by the scheduler process and reflect engine-wide logical block counts, not values pooled from individual tensor-parallel workers.

  
| Metric Name  | Type  | Description  |  
| --- | --- | --- |  
| `vllm:simple_kv_offload_load_blocks_total`  | Counter  | KV blocks restored to GPU from the offload pool, counted when a load finishes. This is prefill work avoided through offload cache hits.  |  
| `vllm:simple_kv_offload_save_outcomes_total`  | Counter  | Store admission decisions in SimpleCPUOffloadConnector, by outcome. Outcomes classify eager-mode boundary hand-off stores; lazy-mode stores are not classified.  |  
| `vllm:simple_kv_offload_info`  | Gauge  | SimpleCPUOffloadConnector deployment facts. Value is always 1. backend is cpu or disk; page_cache and lazy_offload are true/false; capacity_blocks is the offload pool size.  |  
| `vllm:simple_kv_offload_pending_store_blocks`  | Gauge  | Offload blocks in pending stores: queued for dispatch, issued to workers, or awaiting release after a cache reset. A persistently growing value indicates a stuck transfer.  |  
| `vllm:simple_kv_offload_used_blocks`  | Gauge  | Offload-pool blocks currently pinned by in-flight transfers or cache hits (capacity minus free). Evictable cached blocks are not counted; capacity_blocks is on vllm:simple_kv_offload_info.  |  
## Model Flops Utilization (MFU) Performance Metrics[¶](https://docs.vllm.ai/en/latest/usage/metrics/#model-flops-utilization-mfu-performance-metrics "Permanent link")
These metrics are available via `--enable-mfu-metrics`:  
| Metric Name  | Type  | Description  |  
| --- | --- | --- |  
| `vllm:estimated_flops_per_gpu_total`  | Counter  | Estimated number of floating point operations per GPU (for Model Flops Utilization calculations).  |  
| `vllm:estimated_read_bytes_per_gpu_total`  | Counter  | Estimated number of bytes read from memory per GPU (for Model Flops Utilization calculations).  |  
| `vllm:estimated_write_bytes_per_gpu_total`  | Counter  | Estimated number of bytes written to memory per GPU (for Model Flops Utilization calculations).  |  
## Custom Histogram Buckets[¶](https://docs.vllm.ai/en/latest/usage/metrics/#custom-histogram-buckets "Permanent link")
The core engine histograms ship with default bucket boundaries tuned for typical serving workloads. The `--custom-histogram-buckets` option replaces the boundaries of one or more _bucket families_ — exactly the histograms listed in the table below — with your own list; histograms owned by other subsystems (for example, the NIXL connector metrics) are not affected. Use it, for example, to track sub-300ms latency SLOs with the request-phase histograms, whose smallest default boundary is 0.3s:

```
vllmserveQwen/Qwen3-0.6B\
--custom-histogram-buckets'{"request_latency": [0.01, 0.05, 0.1, 0.25, 0.5, 1.0, 5.0, 30.0]}'

```

Each family key overrides a group of related histograms:  
| Family key  | Histograms  |  
| --- | --- |  
| `request_latency`  |  `vllm:e2e_request_latency_seconds`, `vllm:request_queue_time_seconds`, `vllm:request_inference_time_seconds`, `vllm:request_prefill_time_seconds`, `vllm:request_decode_time_seconds`  |  
| `time_to_first_token`  | `vllm:time_to_first_token_seconds`  |  
| `inter_token_latency`  |  `vllm:inter_token_latency_seconds`, `vllm:request_time_per_output_token_seconds`  |  
| `iteration_tokens`  | `vllm:iteration_tokens_total`  |  
| `request_params_n`  | `vllm:request_params_n`  |  
| `request_num_preemptions`  | `vllm:request_num_preemptions`  |  
| `request_tokens`  |  `vllm:request_prompt_tokens`, `vllm:request_generation_tokens`, `vllm:request_max_num_generation_tokens`, `vllm:request_params_max_tokens`, `vllm:request_prefill_kv_computed_tokens`  |  
| `kv_cache_residency`  |  `vllm:kv_block_lifetime_seconds`, `vllm:kv_block_idle_before_evict_seconds`, `vllm:kv_block_reuse_gap_seconds`  |  
Bucket values must be positive, finite, and strictly increasing; unknown family keys are rejected at startup. Families you do not list keep their default boundaries. The `request_tokens` defaults normally scale with `--max-model-len`; an override replaces that computed list. The `kv_cache_residency` family only takes effect when `--kv-cache-metrics` is enabled.
Bucket cardinality
Every bucket boundary creates one extra time series per metric and per label combination (model and engine index, multiplied under data-parallel deployments). Long bucket lists inflate Prometheus storage, scrape sizes, and query costs. Keep custom lists short, and only override the families you actively monitor.
## Deprecation Policy[¶](https://docs.vllm.ai/en/latest/usage/metrics/#deprecation-policy "Permanent link")
Note: when metrics are deprecated in version `X.Y`, they are hidden in version `X.Y+1` but can be re-enabled using the `--show-hidden-metrics-for-version=X.Y` escape hatch, and are then removed in version `X.Y+2`.
Back to top 
