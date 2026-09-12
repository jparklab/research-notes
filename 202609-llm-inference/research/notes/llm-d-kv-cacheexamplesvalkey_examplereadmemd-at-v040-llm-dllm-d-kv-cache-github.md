---
title: llm-d-kv-cache/examples/valkey_example/README.md at v0.4.0 · llm-d/llm-d-kv-cache
  · GitHub
id: llm-d-kv-cacheexamplesvalkey_examplereadmemd-at-v040-llm-dllm-d-kv-cache-github
tags:
- llmd-router-nonk8s-backends-54b774
- kv-cache-indexer
- non-k8s-backend
created: '2026-09-11T20:47:30.618045Z'
updated: '2026-09-12T03:37:37.662957Z'
source: https://github.com/llm-d/llm-d-kv-cache-manager/blob/v0.4.0/examples/valkey_example/README.md
source_domain: github.com
fetched_at: '2026-09-11T20:47:30.596045Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub example README (llm-d/llm-d-kv-cache repo, examples/valkey_example,
  PR #139 "feat: Add Valkey and RDMA support for KV-cache indexing", merged Oct 19
  2025; URL requested was the old llm-d-kv-cache-manager v0.4.0 path, which GitHub
  redirected to the renamed llm-d-kv-cache repo -- confirms llm-d-kv-cache-manager
  was renamed/merged into llm-d-kv-cache). Demonstrates configuring the KV-Cache Indexer''s
  block-index backend to use Valkey (a BSD-licensed, Redis-wire-compatible fork) instead
  of the default in-memory LRU, via a JSON kvBlockIndexConfig.valkeyConfig block with
  address/backendType/enableRDMA fields (parallel redisConfig also shown, confirming
  Redis is directly supported too). Config is driven by a VALKEY_ADDR environment
  variable (default valkey://127.0.0.1:6379) and VALKEY_ENABLE_RDMA flag; the README
  states Valkey "data persists across restarts (unlike in-memory backends)" and is
  "suitable for distributed deployments with multiple indexer replicas." Confirms
  the indexer API treats pod identity as an opaque string, unrelated to Kubernetes:
  the example log output shows scoring calls keyed on an arbitrary podID string ("Cache
  score prompt=... score=1.0 podID=\"demo-pod-1\""), which is directly relevant to
  the non-Kubernetes-backend question -- it demonstrates the indexer''s core scoring
  API does not require the podID to correspond to a real Kubernetes pod object at
  all, only that the same identity string is used consistently between the ZMQ-topic-derived
  pod entry and whatever the caller (a custom router/EPP) uses to identify its routable
  backends.'
---

*Suggested by [[llm-d-kv-cache-manager-module-githubcomllm-dllm-d-kv-cache-manager-go-packages]] — Valkey external backend example, directly relevant to decoupling KV index storage from k8s*

[Skip to content](https://github.com/llm-d/llm-d-kv-cache/blob/v0.4.0/examples/valkey_example/README.md#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/blob/v0.4.0/examples/valkey_example/README.md) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/blob/v0.4.0/examples/valkey_example/README.md) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/blob/v0.4.0/examples/valkey_example/README.md) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-kv-cache/blob/v0.4.0/examples/valkey_example/README.md).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache) You must be signed in to change notification settings
  * [ Fork 157 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache)
  * [ Star  177 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache)


## Collapse file tree
## Files
View file on default branch
Search this repository(forward slash)` forward slash/`
/
# README.md
Copy path
More file actions
More file actions
## Latest commit
[feat: Add Valkey and RDMA support for KV-cache indexing (](https://github.com/llm-d/llm-d-kv-cache/commit/cf33a2dda741e2ea620627badfb35412d329df95)[#139](https://github.com/llm-d/llm-d-kv-cache/pull/139)[)](https://github.com/llm-d/llm-d-kv-cache/commit/cf33a2dda741e2ea620627badfb35412d329df95)
Open commit details
Oct 19, 2025
[cf33a2d](https://github.com/llm-d/llm-d-kv-cache/commit/cf33a2dda741e2ea620627badfb35412d329df95) · Oct 19, 2025
## History
[History](https://github.com/llm-d/llm-d-kv-cache/commits/v0.4.0/examples/valkey_example/README.md)
Open commit details
History
138 lines (103 loc) · 4.35 KB
Code owner: @vMaroon, @dannyharnik, @kfirtoledo, and @liu-cong
/
# README.md
Copy path
## File metadata and controls
  * Preview
  * 

138 lines (103 loc) · 4.35 KB
Code owner: @vMaroon, @dannyharnik, @kfirtoledo, and @liu-cong
Copy raw file
Download raw file
Outline
Edit and raw actions
# Valkey Example for KV-Cache Manager
This example demonstrates how to use Valkey as the backend for the KV-Cache Manager's KV-block indexing system.
## What is Valkey?
Valkey is a community-forked version of Redis that remains under the original BSD license. It's fully API-compatible with Redis and offers additional features like RDMA support for improved latency in high-performance scenarios.
## Benefits of Using Valkey
  * **Open Source** : Remains under the BSD license
  * **Redis Compatibility** : Drop-in replacement for Redis
  * **RDMA Support** : Lower latency networking for high-performance workloads
  * **Community Backed** : Supported by major cloud vendors and Linux Foundation
  * **Performance** : Optimizations for modern hardware


## Prerequisites
  1. **Valkey Server** : Install and run a Valkey server

```
# Using Docker
docker run -d -p 6379:6379 valkey/valkey:latest

# Or install from source/package manager
```

  2. **Go Environment** : Go 1.24.1 or later
  3. **Optional** : Hugging Face token for tokenizer access

```
export HF_TOKEN="your-huggingface-token"
```



## Running the Example
### Basic Usage

```
# Run with default Valkey configuration
go run main.go

# Run with custom Valkey address
VALKEY_ADDR="valkey://your-valkey-server:6379" go run main.go
```

### With RDMA Support
If your Valkey server supports RDMA:

```
VALKEY_ADDR="valkey://rdma-valkey-server:6379" \
VALKEY_ENABLE_RDMA="true" \
go run main.go
```

### Environment Variables
  * `VALKEY_ADDR`: Valkey server address (default: `valkey://127.0.0.1:6379`)
  * `VALKEY_ENABLE_RDMA`: Enable RDMA transport (default: `false`)
  * `HF_TOKEN`: Hugging Face token for tokenizer access (optional)


## What the Example Does
  1. **Configuration** : Sets up a KV-Cache Manager with Valkey backend
  2. **Cache Operations** : Demonstrates adding prompts to the cache
  3. **Cache Hits** : Shows how repeated prompts result in cache hits
  4. **Multi-Pod Lookup** : Demonstrates cache sharing across multiple pods
  5. **Metrics** : Enables metrics collection for monitoring cache performance


## Expected Output

```
I0104 10:30:00.123456       1 main.go:45] Initializing KV-Cache Manager with Valkey backend valkeyAddr="valkey://127.0.0.1:6379" rdmaEnabled=false
I0104 10:30:00.234567       1 main.go:109] Processing prompt iteration=1 prompt="Hello, how are you today?"
I0104 10:30:00.345678       1 main.go:122] Cache score prompt="Hello, how are you today?" score=1.0 podID="demo-pod-1"
I0104 10:30:00.456789       1 main.go:109] Processing prompt iteration=3 prompt="Hello, how are you today?"
I0104 10:30:00.567890       1 main.go:122] Cache score prompt="Hello, how are you today?" score=1.0 podID="demo-pod-1"
...
I0104 10:30:02.123456       1 main.go:65] Valkey example completed successfully

```

## Comparison with Redis
The Valkey backend is API-compatible with Redis, so you can easily switch between them:
### Redis Configuration

```
{
  "kvBlockIndexConfig": {
    "redisConfig": {
      "address": "redis://127.0.0.1:6379"
    }
  }
}
```

### Valkey Configuration

```
{
  "kvBlockIndexConfig": {
    "valkeyConfig": {
      "address": "valkey://127.0.0.1:6379",
      "backendType": "valkey",
      "enableRDMA": false
    }
  }
}
```

## Performance Considerations
  * **RDMA** : Enable RDMA for ultra-low latency if your infrastructure supports it
  * **Connection Pooling** : The underlying Redis client handles connection pooling
  * **Persistence** : Valkey data persists across restarts (unlike in-memory backends)
  * **Scalability** : Suitable for distributed deployments with multiple indexer replicas


## Troubleshooting
### Connection Issues
  * Ensure Valkey server is running and accessible
  * Check network connectivity and firewall rules
  * Verify the address format (supports `valkey://`, `redis://`, or plain addresses)


### RDMA Issues
  * Confirm Valkey server is compiled with RDMA support
  * Verify RDMA hardware and drivers are properly configured
  * Check that both client and server are on RDMA-enabled networks


### Performance Issues
  * Monitor cache hit rates using the built-in metrics
  * Adjust block size in TokenProcessorConfig for your use case
  * Consider using multiple Valkey instances for horizontal scaling


## See Also
  * [Valkey Configuration Guide](https://github.com/llm-d/llm-d-kv-cache/blob/v0.4.0/examples/valkey_configuration.md)
  * [KV-Cache Manager Architecture](https://github.com/llm-d/llm-d-kv-cache/blob/v0.4.0/docs/architecture.md)
  * [Configuration Reference](https://github.com/llm-d/llm-d-kv-cache/blob/v0.4.0/docs/configuration.md)


You can’t perform that action at this time. 
