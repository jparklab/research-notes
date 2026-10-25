---
title: 'GitHub - neuralmagic/fs-offload-experiments: Benchmark sweep experiments for
  FS Offloading · GitHub'
id: github-neuralmagicfs-offload-experiments-benchmark-sweep-experiments-for-fs-offl
tags:
- kv-cache
- benchmark
- benchmark-methodology
- reproducibility
- llmd-kv-cache-routing-c665d5
created: '2026-09-14T11:59:21.046449Z'
updated: '2026-09-15T04:55:24.118660Z'
source: https://github.com/neuralmagic/fs-offload-experiments
source_domain: github.com
fetched_at: '2026-09-14T11:59:21.027040Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub repo (neuralmagic/fs-offload-experiments) providing the exact benchmark
  harness and configuration behind the throughput numbers in vLLM''s Sept 2026 tiered
  KV cache offloading blog post. Uses ''nyann-bench'' with a conversation-pool workload:
  32 conversations x 8 turns, turn-1 prompt 12,288 tokens, each subsequent turn appends
  4,096 new prompt tokens plus prior output, concurrency fixed at 16 in-flight requests,
  and each sweep point runs a size-adjusted number of whole conversations (not wall-clock-bounded)
  so faster configs are not advantaged by simply completing more requests. Tests four
  vLLM configurations with progressively larger effective cache capacity: (1) GPU
  only, no prefix caching; (2) GPU + Prefix Cache; (3) GPU + CPU KV offloading; (4)
  GPU + CPU + NVMe filesystem offloading -- confirming the blog''s 3-tier (HBM/CPU/storage)
  throughput-scaling narrative is a controlled ablation, not just marketing framing.
  Sweep parameters for the Qwen model: Qwen/Qwen3.6-35B-A3B, CPU_BYTES ~147GB (~2x
  GPU KV size), NVMe PVC 8Ti, conversation pool sizes swept across 16/32/48/64/96/128/192/256/384/448/512,
  best concurrency baseline of 64 estimated from GPU-only/no-prefix-caching runs --
  directly matching the ''~64 conversations'' and ''64-128 conversations'' inflection
  points reported in the blog. Uses vllm/vllm-openai:v0.26.0. Flags a known nyann-bench
  main-branch bug where conversation_pool mode can stall and fail to sustain requested
  concurrency (tracked in neuralmagic/nyann-bench#77); the pinned benchmark image
  used a hot-fix branch that is now lost, so results are not trivially reproducible
  from a fresh nyann-bench main checkout.'
---

*Suggested by [[tiered-kv-cache-offloading-in-vllm-vllm-blog]] — benchmark reproduction repo for the storage-tier KV offloading throughput results*

[Skip to content](https://github.com/neuralmagic/fs-offload-experiments#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/neuralmagic/fs-offload-experiments) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/neuralmagic/fs-offload-experiments) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/neuralmagic/fs-offload-experiments) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/neuralmagic/fs-offload-experiments).
/ **[fs-offload-experiments](https://github.com/neuralmagic/fs-offload-experiments) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Fneuralmagic%2Ffs-offload-experiments) You must be signed in to change notification settings
  * [ Fork 0 ](https://github.com/login?return_to=%2Fneuralmagic%2Ffs-offload-experiments)
  * [ Star  0 ](https://github.com/login?return_to=%2Fneuralmagic%2Ffs-offload-experiments)


[**1** Branch](https://github.com/neuralmagic/fs-offload-experiments/branches)[](https://github.com/neuralmagic/fs-offload-experiments/tags)
Go to file
Open more actions menu
## Latest commit
Open commit details
Sep 3, 2026
[a3feee2](https://github.com/neuralmagic/fs-offload-experiments/commit/a3feee2825c460374e02fb3ea196f67dae1420a0) · Sep 3, 2026
## History
[6 Commits](https://github.com/neuralmagic/fs-offload-experiments/commits/main/)
Open commit details
6 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |   | Sep 3, 2026  |  
|   |   | Sep 3, 2026  |  
|   |   | Sep 3, 2026  |  
| Sep 3, 2026  |  
|   |   | Sep 3, 2026  |  
|   |   | Sep 3, 2026  |  
|   |   |   | Sep 3, 2026  |  
|   |   |   | Sep 3, 2026  |  
| View all files |  
## Repository files navigation
# FS Offload Experiments
📊 **[View latest results report](https://github.com/neuralmagic/fs-offload-experiments/blob/main/report.html)**
Demonstrate that progressively larger KV cache tiers improve vLLM inference throughput as the working-set size grows. Each caching tier should accommodate a larger working set, avoiding recompute and increasing output token throughput.
## Benchmark Setup
We use **nyann-bench** with a conversation-pool workload. The scenario config looks like:

```
scenario(
  stages=[
    stage("240m", mode="conversation_pool", concurrency=16,
          conversation_pool_size=32, rampup="30s", max_requests=5120)
  ],
  workload=workload("faker", isl=12288, subsequent_isl=4096, osl=1, turns=8)
)

```

### Workload Behaviour
  * **32 conversations** , each with **8 turns**.
  * **Turn 1** prompt: `12,288` tokens.
  * **Turn N** prompt: previous turn's prompt + `1` (prior OSL) + `4,096` (subsequent ISL).
  * Concurrency is respected (16 in-flight requests at a time).
  * Turn N is only submitted once turn N−1 of **all** conversations is complete.
  * When a conversation finishes all turns, a new conversation replaces it.
  * The stage ends after **5,120 total requests** (rather than a fixed wall-clock time).


### Example Request Timeline  
| Time  | Action  |  
| --- | --- |  
| T=0  | Submit turn 0 of conversations 1–16  |  
| T=1  | Submit turn 0 of conversations 17–32  |  
| T=2  | Submit turn 1 of conversations 1–16  |  
| T=3  | Submit turn 1 of conversations 17–32  |  
This keeps the working-set size stable and lets us tune it by adjusting `conversation_pool_size`.
### Why `max_requests` varies by pool size
Each run is bounded by `max_requests` rather than wall-clock time. A fixed time limit causes two problems: faster configs (e.g. `4-gpu-cpu-nvme`) push through more requests than slower ones at the same pool size — skewing the comparison — and the cutoff can land mid-conversation, truncating exactly the cache-heavy later turns the benchmark exists to measure.
Instead, `max_requests` is sized so every pool size runs only **whole conversations** :

```
conv_reqs              = pool_size × turns
num_full_conversations = floor(POOL_MAX_REQUESTS / conv_reqs)
max_requests           = num_full_conversations × conv_reqs

```

`SWEEP_MAX_REQUESTS_DURATION` (240m) remains as a generous backstop in case of stalls.
## vLLM Configurations Under Test  
| #  | Config  | Description  |  
| --- | --- | --- |  
| 1  | **GPU**  | No prefix caching  |  
| 2  | **GPU + Prefix Cache**  | GPU-side prefix caching enabled  |  
| 3  | **GPU + CPU**  | GPU prefix cache + CPU KV offloading  |  
| 4  | **GPU + CPU + NVMe**  | GPU prefix cache + CPU offloading + NVMe FS offloading  |  
Each successive config provides more total KV cache capacity. We expect **total token throughput to increase** with each tier as a larger fraction of the working set is served from cache.
## How to Run
### 1. Configure
All variables are set at the top of the `Justfile` and can be overridden via environment variables. Update these for your cluster before running:  
| Variable  | Default  | Description  |  
| --- | --- | --- |  
| `NAMESPACE`  | `machine-learning`  | Kubernetes namespace to deploy into  |  
| `GPU_PRODUCT`  | `NVIDIA-H100-80GB-HBM3`  | Node label used to schedule GPU pods  |  
| `HF_SECRET`  | `<username>-token`  | Kubernetes secret holding the HuggingFace token  |  
| `MODEL_CACHE_PVC`  | `runtime-model-cache`  | PVC name for the shared read-only model cache  |  
| `MODEL_CACHE_DIR`  | `/mnt/model-cache`  | Mount path for the model cache PVC  |  
| `NVME_STORAGE_CLASS`  | `lvms-h100-tier1-storage`  | StorageClass for NVMe-backed PVCs (config 4)  |  
### 2. Run

```
# Deploy all 4 configs, run the sweep, tear down — all in one shot
just run-all qwen35b

# Copy raw results from the cluster PVC to local results/
just fetch-results qwen35b

# Run nyann-bench analyze on each raw/ dir to produce benchmark.json
just analyze-results qwen35b

# Generate a comparison HTML report across all configs
just compare-sweep qwen35b
```

`run-all` runs all 4 configs in **parallel** by default (deploy → warmup → sweep → teardown). Pass `serial` as a second argument to run them one after the other:

```
just run-all qwen35b serial
```

`compare-sweep` outputs a self-contained HTML file in `results/qwen35b/` with throughput curves for each config overlaid.
## Versions & Hardware
These are the default values in the `Justfile` and can be overridden via environment variables. The experiment should work with any compatible vLLM/nyann-bench version subject to the caveat below.  
| Component  | Value  |  
| --- | --- |  
| vLLM image  | `vllm/vllm-openai:v0.26.0`  |  
| nyann-bench image  |  `quay.io/rh-ee-vsundarr/nyann-bench:ad0f53` ⚠️ see note below  |  
| GPU  | `NVIDIA H100 80GB HBM3`  |  
> **nyann-bench caveat:** `main` has a bug where `conversation_pool` mode can stall and fail to maintain the requested concurrency (tracked in [neuralmagic/nyann-bench#77](https://github.com/neuralmagic/nyann-bench/pull/77)). The image used here (`ad0f53`) was built from a hot-fix branch (now lost) that resolved the issue. Do not swap in a current `main` build without verifying concurrency is sustained throughout the run.
## Model & Sweep Parameters (Qwen)  
| Parameter  | Value  |  
| --- | --- |  
| `MODEL`  | `Qwen/Qwen3.6-35B-A3B`  |  
| `TP_SIZE`  |  
| `CPU_BYTES`  |  `147,400,000,000` (~147 GB, ≈2× GPU KV size)  |  
| `NVME_PVC_SIZE`  | `8Ti`  |  
| `SWEEP_POOL_SIZES`  | `16 32 48 64 96 128 192 256 384 448 512`  |  
| `BEST_CONCURRENCY`  |  `64` (estimated with GPU-only, no prefix caching)  |  
## About
Benchmark sweep experiments for FS Offloading 
### Resources
### Stars
**0** stars
### Watchers
**0** watching
### Forks
[ forks](https://github.com/neuralmagic/fs-offload-experiments/forks)
## 
No releases published
## 
  * [**varun-sundar-rabindranath** Varun Sundar Rabindranath](https://github.com/varun-sundar-rabindranath)


## Languages
  * [HTML72.2%](https://github.com/neuralmagic/fs-offload-experiments/search?l=html)
  * [Python17.2%](https://github.com/neuralmagic/fs-offload-experiments/search?l=python)
  * [Just10.6%](https://github.com/neuralmagic/fs-offload-experiments/search?l=just)


You can’t perform that action at this time. 
