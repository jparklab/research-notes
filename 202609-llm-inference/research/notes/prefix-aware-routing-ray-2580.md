---
title: Prefix-aware routing — Ray 2.58.0
id: prefix-aware-routing-ray-2580
tags:
- llm-d
- kv-cache-routing
- load-balancing
- prefix-caching
- comparison-system
- llmd-kv-cache-routing-c665d5
created: '2026-09-14T11:54:13.717790Z'
updated: '2026-09-15T04:55:24.064525Z'
source: https://docs.ray.io/en/latest/serve/llm/user-guides/prefix-aware-routing.html
source_domain: docs.ray.io
fetched_at: '2026-09-14T11:54:13.699139Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'Ray Serve docs (alpha API, v2.58.0) for PrefixCacheAffinityRouter, a comparison
  implementation of cache-aware routing to llm-d''s KV-cache-aware scheduler. Explicitly
  builds in a load-imbalance escape hatch that llm-d''s design documents reference
  conceptually: the router first checks whether queue-length difference across replicas
  exceeds an imbalanced_threshold (default: infinity, i.e. disabled unless configured);
  only when load is judged balanced does it proceed to prefix-tree-based routing.
  When routing on prefix, match rate >=10% (match_rate_threshold, default 0.1) routes
  to the replica with highest prefix match for cache hits; match rate <10% deliberately
  falls back to the replica with LOWEST prefix cache utilization to spread cold traffic
  and increase utilization; no prefix data falls back to Power-of-Two-Choices. When
  load actually is imbalanced (queue-length gap exceeds threshold), the router abandons
  cache locality entirely and reverts to plain Power-of-Two-Choices, prioritizing
  load balancing over cache hit rate. The prefix tree itself is a distributed, detached
  Ray actor with configurable eviction (default eviction_threshold_chars=400,000,
  eviction_target_chars=360,000 (doc example config differs: 500,000/400,000), eviction_interval_secs=10)
  to bound memory and approximate the underlying vLLM engine''s own eviction policy.
  Requires vLLM''s enable_prefix_caching=True (Automatic Prefix Caching, APC) to have
  any effect.'
utility_score: 12.0
---

[Skip to main content](https://docs.ray.io/en/latest/serve/llm/user-guides/prefix-aware-routing.html#main-content)
Back to top
  * System Settings
  * Light


latest
[latest](https://docs.ray.io/en/latest/serve/llm/user-guides/prefix-aware-routing.html)[master](https://docs.ray.io/en/master/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.58.0](https://docs.ray.io/en/releases-2.58.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.57.0](https://docs.ray.io/en/releases-2.57.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.56.1](https://docs.ray.io/en/releases-2.56.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.56.0](https://docs.ray.io/en/releases-2.56.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.55.1](https://docs.ray.io/en/releases-2.55.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.55.0](https://docs.ray.io/en/releases-2.55.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.54.1](https://docs.ray.io/en/releases-2.54.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.54.0](https://docs.ray.io/en/releases-2.54.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.53.0](https://docs.ray.io/en/releases-2.53.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.52.1](https://docs.ray.io/en/releases-2.52.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.52.0](https://docs.ray.io/en/releases-2.52.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.51.2](https://docs.ray.io/en/releases-2.51.2/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.51.1](https://docs.ray.io/en/releases-2.51.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.51.0](https://docs.ray.io/en/releases-2.51.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.50.1](https://docs.ray.io/en/releases-2.50.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.50.0](https://docs.ray.io/en/releases-2.50.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.49.2](https://docs.ray.io/en/releases-2.49.2/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.49.1](https://docs.ray.io/en/releases-2.49.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.49.0](https://docs.ray.io/en/releases-2.49.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.48.0](https://docs.ray.io/en/releases-2.48.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.47.1](https://docs.ray.io/en/releases-2.47.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.47.0](https://docs.ray.io/en/releases-2.47.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.46.0](https://docs.ray.io/en/releases-2.46.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.45.0](https://docs.ray.io/en/releases-2.45.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.44.1](https://docs.ray.io/en/releases-2.44.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.44.0](https://docs.ray.io/en/releases-2.44.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.43.0](https://docs.ray.io/en/releases-2.43.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.42.1](https://docs.ray.io/en/releases-2.42.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.42.0](https://docs.ray.io/en/releases-2.42.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.41.0](https://docs.ray.io/en/releases-2.41.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.40.0](https://docs.ray.io/en/releases-2.40.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.39.0](https://docs.ray.io/en/releases-2.39.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.38.0](https://docs.ray.io/en/releases-2.38.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.37.0](https://docs.ray.io/en/releases-2.37.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.36.1](https://docs.ray.io/en/releases-2.36.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.36.0](https://docs.ray.io/en/releases-2.36.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.35.0](https://docs.ray.io/en/releases-2.35.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.34.0](https://docs.ray.io/en/releases-2.34.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.33.0](https://docs.ray.io/en/releases-2.33.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.32.0](https://docs.ray.io/en/releases-2.32.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.31.0](https://docs.ray.io/en/releases-2.31.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.30.0](https://docs.ray.io/en/releases-2.30.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.24.0](https://docs.ray.io/en/releases-2.24.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.23.0](https://docs.ray.io/en/releases-2.23.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.22.0](https://docs.ray.io/en/releases-2.22.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.21.0](https://docs.ray.io/en/releases-2.21.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.20.0](https://docs.ray.io/en/releases-2.20.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.12.0](https://docs.ray.io/en/releases-2.12.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.11.0](https://docs.ray.io/en/releases-2.11.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.10.0](https://docs.ray.io/en/releases-2.10.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.9.3](https://docs.ray.io/en/releases-2.9.3/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.9.2](https://docs.ray.io/en/releases-2.9.2/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.9.1](https://docs.ray.io/en/releases-2.9.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.9.0](https://docs.ray.io/en/releases-2.9.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.8.1](https://docs.ray.io/en/releases-2.8.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.8.0](https://docs.ray.io/en/releases-2.8.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.7.1](https://docs.ray.io/en/releases-2.7.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.7.0](https://docs.ray.io/en/releases-2.7.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.6.3](https://docs.ray.io/en/releases-2.6.3/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.6.2](https://docs.ray.io/en/releases-2.6.2/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.6.1](https://docs.ray.io/en/releases-2.6.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.6.0](https://docs.ray.io/en/releases-2.6.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.5.1](https://docs.ray.io/en/releases-2.5.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.5.0](https://docs.ray.io/en/releases-2.5.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.4.0](https://docs.ray.io/en/releases-2.4.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.3.1](https://docs.ray.io/en/releases-2.3.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.3.0](https://docs.ray.io/en/releases-2.3.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.2.0](https://docs.ray.io/en/releases-2.2.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.1.0](https://docs.ray.io/en/releases-2.1.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.0.1](https://docs.ray.io/en/releases-2.0.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-2.0.0](https://docs.ray.io/en/releases-2.0.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-1.13.1](https://docs.ray.io/en/releases-1.13.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-1.13.0](https://docs.ray.io/en/releases-1.13.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-1.12.1](https://docs.ray.io/en/releases-1.12.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-1.12.0](https://docs.ray.io/en/releases-1.12.0/serve/llm/user-guides/prefix-aware-routing.html)[releases-1.11.1](https://docs.ray.io/en/releases-1.11.1/serve/llm/user-guides/prefix-aware-routing.html)[releases-1.11.0](https://docs.ray.io/en/releases-1.11.0/serve/llm/user-guides/prefix-aware-routing.html)
[ Try Managed Ray ](https://console.anyscale.com/register/ha?render_flow=ray&utm_source=ray_docs&utm_medium=docs&utm_campaign=navbar)
# Prefix-aware routing[#](https://docs.ray.io/en/latest/serve/llm/user-guides/prefix-aware-routing.html#prefix-aware-routing "Link to this heading")
Optimize LLM inference with cache locality using prefix-aware request routing.
Warning
This API is in alpha and may change before becoming stable.
LLM inference can benefit significantly from cache locality optimization. When one replica processes multiple prompts that share a prefix, the engine can reuse previously computed KV-cache entries, reducing computation overhead and improving response times. This technique is known as [Automatic Prefix Caching (APC)](https://docs.vllm.ai/en/stable/features/automatic_prefix_caching.html) in vLLM.
The `PrefixCacheAffinityRouter` routes requests with similar prefixes to the same replicas, maximizing KV cache hit rates.
## When to use prefix-aware routing[#](https://docs.ray.io/en/latest/serve/llm/user-guides/prefix-aware-routing.html#when-to-use-prefix-aware-routing "Link to this heading")
Use prefix-aware routing when:
  * Your workload has many requests with shared prefixes (for example, same system prompts or few-shot examples)
  * You’re using vLLM with Automatic Prefix Caching enabled
  * Cache hit rate is more important than perfect load balance in balanced scenarios


## How it works[#](https://docs.ray.io/en/latest/serve/llm/user-guides/prefix-aware-routing.html#how-it-works "Link to this heading")
The `PrefixCacheAffinityRouter` implements a multi-tier routing strategy that balances cache locality with load distribution:
### 1. Load balance check[#](https://docs.ray.io/en/latest/serve/llm/user-guides/prefix-aware-routing.html#load-balance-check "Link to this heading")
First, it evaluates whether the current load is balanced across replicas by comparing queue lengths. If the difference between the highest and lowest queue lengths is below the `imbalanced_threshold`, it proceeds with prefix cache-aware routing.
### 2. Prefix matching strategy[#](https://docs.ray.io/en/latest/serve/llm/user-guides/prefix-aware-routing.html#prefix-matching-strategy "Link to this heading")
When load is balanced, the router uses a prefix tree to find replicas that have previously processed similar input text:
  * **High match rate (≥10%)** : Routes to replicas with the highest prefix match rate for better cache hit rates
  * **Low match rate ( <10%)**: Falls back to replicas with the lowest prefix cache utilization to increase utilization
  * **No prefix data** : Uses the default Power of Two Choices selection


### 3. Imbalanced load fallback[#](https://docs.ray.io/en/latest/serve/llm/user-guides/prefix-aware-routing.html#imbalanced-load-fallback "Link to this heading")
When load is imbalanced (queue length difference exceeds threshold), the router prioritizes load balancing over cache locality and falls back to the standard Power of Two Choices algorithm.
### Prefix tree management[#](https://docs.ray.io/en/latest/serve/llm/user-guides/prefix-aware-routing.html#prefix-tree-management "Link to this heading")
The router maintains a distributed prefix tree actor that:
  * Tracks input text prefixes processed by each replica
  * Supports automatic eviction of old entries to manage memory usage
  * Persists across router instances using Ray’s detached actor pattern


## Deploy with prefix-aware routing[#](https://docs.ray.io/en/latest/serve/llm/user-guides/prefix-aware-routing.html#deploy-with-prefix-aware-routing "Link to this heading")
The following example shows how to deploy an LLM with prefix-aware routing:

```
from ray import serve
from ray.serve.llm import LLMConfig, build_openai_app
from ray.serve.llm.request_router import PrefixCacheAffinityRouter

llm_config = LLMConfig(
    model_loading_config={
        "model_id": "qwen-0.5b",
        "model_source": "Qwen/Qwen2.5-0.5B-Instruct",
    },
    deployment_config={
        "autoscaling_config": {
            "min_replicas": 4,
            "max_replicas": 4,
        },
        "request_router_config": {
            "request_router_class": PrefixCacheAffinityRouter,
            "request_router_kwargs": {
                "imbalanced_threshold": 5,  # More aggressive load balancing
                "match_rate_threshold": 0.15,  # Require 15% match rate
                "do_eviction": True,  # Enable memory management
                "eviction_threshold_chars": 500_000,
                "eviction_target_chars": 400_000,
                "eviction_interval_secs": 30,
            },
        },
    },
)

app = build_openai_app({"llm_configs": [llm_config]})
serve.run(app, blocking=True)

```
Copy to clipboard
## Configuration parameters[#](https://docs.ray.io/en/latest/serve/llm/user-guides/prefix-aware-routing.html#configuration-parameters "Link to this heading")
The `PrefixCacheAffinityRouter` provides several configuration parameters to tune its behavior:
### Core routing parameters[#](https://docs.ray.io/en/latest/serve/llm/user-guides/prefix-aware-routing.html#core-routing-parameters "Link to this heading")
  * **`imbalanced_threshold`**(default: infinity): Queue length difference threshold for considering load balanced. Lower values prioritize load balancing over cache locality.
  * **`match_rate_threshold`**(default: 0.1): Minimum prefix match rate (0.0-1.0) required to use prefix cache-aware routing. Higher values require stronger prefix matches before routing for cache locality.


### Memory management parameters[#](https://docs.ray.io/en/latest/serve/llm/user-guides/prefix-aware-routing.html#memory-management-parameters "Link to this heading")
  * **`do_eviction`**(default: False): Enable automatic eviction of old prefix tree entries to approximate the LLM engine’s eviction policy.
  * **`eviction_threshold_chars`**(default: 400,000): Maximum number of characters in the prefix tree before the LLM engine triggers an eviction.
  * **`eviction_target_chars`**(default: 360,000): Target number of characters to reduce the prefix tree to during eviction.
  * **`eviction_interval_secs`**(default: 10): Interval in seconds between eviction checks when eviction is enabled.


## Best practices[#](https://docs.ray.io/en/latest/serve/llm/user-guides/prefix-aware-routing.html#best-practices "Link to this heading")
  * **Enable vLLM APC** : Make sure to set `enable_prefix_caching=True` in your `engine_kwargs` for the router to have any effect
  * **Tune thresholds** : Adjust `imbalanced_threshold` and `match_rate_threshold` based on your workload characteristics
  * **Monitor cache hit rates** : Track vLLM’s cache hit metrics to verify the router is improving performance
  * **Start conservative** : Begin with default settings and tune incrementally based on observed behavior


## See also[#](https://docs.ray.io/en/latest/serve/llm/user-guides/prefix-aware-routing.html#see-also "Link to this heading")
  * [Architecture: Request routing](https://docs.ray.io/en/latest/serve/llm/architecture/routing-policies.html)
  * [vLLM Automatic Prefix Caching](https://docs.vllm.ai/en/stable/features/automatic_prefix_caching.html)


On this page 
[ Edit on GitHub ](https://github.com/ray-project/ray/edit/master/doc/source/serve/llm/user-guides/prefix-aware-routing.md)
