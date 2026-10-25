---
title: 'GitHub - kvcache-ai/Mooncake: Mooncake is the serving platform for Kimi, a
  leading LLM service provided by Moonshot AI. · GitHub'
id: github-kvcache-aimooncake-mooncake-is-the-serving-platform-for-kimi-a-leading-ll
tags:
- llmd-kv-cache-routing-c665d5
- mooncake
- kv-cache
- kv-offloading
- rdma
created: '2026-09-14T11:56:36.675747Z'
updated: '2026-09-15T04:55:24.078976Z'
source: https://github.com/kvcache-ai/Mooncake
source_domain: github.com
fetched_at: '2026-09-14T11:56:36.652038Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Mooncake GitHub repository page (6.6k stars, 1.2k forks, v0.3.13.post1 latest
  release, ~79% C++/13.5% Python/2.2% CUDA) — the upstream project behind llm-d''s
  MooncakeStoreConnector. README describes Mooncake as the serving platform for Kimi
  (Moonshot AI''s LLM service) and details Mooncake Store: a high-performance distributed
  key-value cache storage engine built on the Mooncake Transfer Engine that stores
  and manages reusable KV caches and model weights across inference clusters. Key
  architectural claims: large-object striping, parallel I/O, and end-to-end zero-copy
  transfer fully utilize aggregated bandwidth across multiple NICs; a multi-level
  cache hierarchy spans DRAM and SSD/NVMe for larger capacity; storage is elastic
  and disaggregated from inference engines, so storage nodes can be added/removed
  independently of engine restarts, upgrades, or scheduling; and per-object policies
  (replica counts, preferred segments, soft pin, hard pin) give applications programmatic
  control over placement and eviction. States broad ecosystem adoption: integrations
  with SGLang''s Hierarchical KV Caching (HiCache), vLLM''s disaggregated prefill
  serving, and LMCache, plus adoption by TorchSpec and TransferQueue. Also documents
  newer components: Mooncake EP (DeepEP-style fault-tolerant expert-parallel dispatch/combine
  with active_ranks awareness for MoE inference) and Mooncake PG (a PyTorch distributed
  process-group backend with failure detection and elastic rank recovery). Cites the
  project''s FAST''25 paper (Qin et al. 2024, arXiv:2407.00079) describing the KVCache-centric
  disaggregated architecture. Note: much of the raw fetched page is GitHub UI/commit-log
  noise (file listing, individual PR descriptions); the substantive README content
  begins roughly two-thirds through the body.'
---

*Suggested by [[kv-cache-offloading-llm-d]] — cited as the upstream Mooncake project that MooncakeStoreConnector integrates*

[Skip to content](https://github.com/kvcache-ai/Mooncake#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kvcache-ai/Mooncake) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kvcache-ai/Mooncake) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kvcache-ai/Mooncake) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkvcache-ai%2FMooncake) You must be signed in to change notification settings
  * [ Fork 1.2k ](https://github.com/login?return_to=%2Fkvcache-ai%2FMooncake)
  * [ Star  6.6k ](https://github.com/login?return_to=%2Fkvcache-ai%2FMooncake)


[**67** Branches](https://github.com/kvcache-ai/Mooncake/branches)[](https://github.com/kvcache-ai/Mooncake/tags)
Go to file
Open more actions menu
## Latest commit
and
TTThanos
[[Bugfix][TENT] Forward classic RDMA device filters (](https://github.com/kvcache-ai/Mooncake/commit/6cc1a100404ea40a13940b49616a9127c5662b1d)[#4028](https://github.com/kvcache-ai/Mooncake/pull/4028)[)](https://github.com/kvcache-ai/Mooncake/commit/6cc1a100404ea40a13940b49616a9127c5662b1d)
Open commit detailsfailure
Sep 14, 2026
[6cc1a10](https://github.com/kvcache-ai/Mooncake/commit/6cc1a100404ea40a13940b49616a9127c5662b1d) · Sep 14, 2026
## History
[2,158 Commits](https://github.com/kvcache-ai/Mooncake/commits/main/)
Open commit details
2,158 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |  [docs: publish built-in skills and add plugin marketplace (](https://github.com/kvcache-ai/Mooncake/commit/9741cd75754d563ac5fe991b27f2b62ce129fde0 "docs: publish built-in skills and add plugin marketplace \(#2497\)

Add a Skills section to the documentation site and a Claude Code plugin
marketplace so the built-in skills are discoverable and installable
without cloning the full repository.

- Register mooncake-api as a slash command \(add missing SKILL.md frontmatter\)
- Add per-skill plugin.json manifests
- Add .claude-plugin/marketplace.json using git-subdir sources so installs
  fetch only the relevant skill subdirectory
- Add docs/source/skills/ pages \(overview + per-skill\) and link from index")[#2497](https://github.com/kvcache-ai/Mooncake/pull/2497)[)](https://github.com/kvcache-ai/Mooncake/commit/9741cd75754d563ac5fe991b27f2b62ce129fde0 "docs: publish built-in skills and add plugin marketplace \(#2497\)

Add a Skills section to the documentation site and a Claude Code plugin
marketplace so the built-in skills are discoverable and installable
without cloning the full repository.

- Register mooncake-api as a slash command \(add missing SKILL.md frontmatter\)
- Add per-skill plugin.json manifests
- Add .claude-plugin/marketplace.json using git-subdir sources so installs
  fetch only the relevant skill subdirectory
- Add docs/source/skills/ pages \(overview + per-skill\) and link from index")  | Jun 17, 2026  |  
| [.claude/skills](https://github.com/kvcache-ai/Mooncake/tree/main/.claude/skills "This path skips through empty directories")  | [.claude/skills](https://github.com/kvcache-ai/Mooncake/tree/main/.claude/skills "This path skips through empty directories")  |  [[Store] Add Optional Object Checksum Diagnostics (](https://github.com/kvcache-ai/Mooncake/commit/62ca2c23ab3f8cc0e70742a89d505ffa3f65067f "\[Store\] Add Optional Object Checksum Diagnostics \(#3003\)")[#3003](https://github.com/kvcache-ai/Mooncake/pull/3003)[)](https://github.com/kvcache-ai/Mooncake/commit/62ca2c23ab3f8cc0e70742a89d505ffa3f65067f "\[Store\] Add Optional Object Checksum Diagnostics \(#3003\)")  | Jul 31, 2026  |  
|   |   |   | Aug 24, 2026  |  
|   |   |  [[CI/Build] Enable KV events in wheels with ZeroMQ dependencies (](https://github.com/kvcache-ai/Mooncake/commit/5f2a5c6d8b0e6100a09bbd84b02dacc5e5d7c08f "\[CI/Build\] Enable KV events in wheels with ZeroMQ dependencies \(#4060\)



---------

Signed-off-by: Ishan Dhanani <ishandhanani@gmail.com>
Co-authored-by: Ishan Dhanani <ishandhanani@gmail.com>")[#4060](https://github.com/kvcache-ai/Mooncake/pull/4060)[)](https://github.com/kvcache-ai/Mooncake/commit/5f2a5c6d8b0e6100a09bbd84b02dacc5e5d7c08f "\[CI/Build\] Enable KV events in wheels with ZeroMQ dependencies \(#4060\)



---------

Signed-off-by: Ishan Dhanani <ishandhanani@gmail.com>
Co-authored-by: Ishan Dhanani <ishandhanani@gmail.com>")  | Sep 13, 2026  |  
|   |   |  [[Doc] clarify FAST25 trace release (](https://github.com/kvcache-ai/Mooncake/commit/1d0e4c7587b57b78a1997be370b349b79828c5dd "\[Doc\] clarify FAST25 trace release \(#2727\)")[#2727](https://github.com/kvcache-ai/Mooncake/pull/2727)[)](https://github.com/kvcache-ai/Mooncake/commit/1d0e4c7587b57b78a1997be370b349b79828c5dd "\[Doc\] clarify FAST25 trace release \(#2727\)")  | Jul 3, 2026  |  
|   |   |  [[Feature] Add per-file storage mode to storage_benchmark_v1 (](https://github.com/kvcache-ai/Mooncake/commit/bce9a7c3fc95579c7a93ba788c61f97320e76735 "\[Feature\] Add per-file storage mode to storage_benchmark_v1 \(#3454\)")[#3454](https://github.com/kvcache-ai/Mooncake/pull/3454)[)](https://github.com/kvcache-ai/Mooncake/commit/bce9a7c3fc95579c7a93ba788c61f97320e76735 "\[Feature\] Add per-file storage mode to storage_benchmark_v1 \(#3454\)")  | Sep 7, 2026  |  
|   |   |  [[CI/Build] Make CMake target dependencies explicit (](https://github.com/kvcache-ai/Mooncake/commit/dc9d25f37ce3f561129bc820d4330f7007288bcf "\[CI/Build\] Make CMake target dependencies explicit \(#3276\)")[#3276](https://github.com/kvcache-ai/Mooncake/pull/3276)[)](https://github.com/kvcache-ai/Mooncake/commit/dc9d25f37ce3f561129bc820d4330f7007288bcf "\[CI/Build\] Make CMake target dependencies explicit \(#3276\)")  | Aug 20, 2026  |  
|   |   |  [[TENT] Add Intel XPU platform to the Transfer Engine (](https://github.com/kvcache-ai/Mooncake/commit/fa601df23d53dd8edf2826dee7a71c24e7118e11 "\[TENT\] Add Intel XPU platform to the Transfer Engine \(#4030\)

Add an XpuPlatform to the TENT runtime so Mooncake can stage transfers
through Intel XPU \(GPU\) device memory. XpuPlatform extends CpuPlatform:
host DRAM allocation, NUMA probing, and host<->host copies are inherited
unchanged, and only the XPU-device-aware operations are overridden.

Implementation is a native, direct-link SYCL build: the platform sources
include <sycl/sycl.hpp> and link libsycl directly via a private
XpuSyclBackend \(device enumeration with GPU-preferred/any-device
fallback, sycl::malloc_device USM allocation, an interior-pointer
registry so chunked staging addresses classify correctly, and
VRAM<->host memcpy\). Because SYCL requires the Intel DPC++ compiler,
USE_XPU=ON now requires USE_TENT=ON and fails fast unless the build is
configured with icpx \(CXX=icpx\); -fsycl is applied to platform_xpu and
propagated to everything that links it.

- runtime: MTYPE_XPU memory type + USE_XPU loader branch
- platform: XpuPlatform \(probe/allocate/free/copy/getMemoryType/
  getLocation\) backed by XpuSyclBackend
- build: USE_XPU option/gate in common.cmake; platform_xpu with -fsycl
- test: tent_xpu_platform_test drives the real backend \(alloc -> H2D ->
  D2H -> byte-equality -> free, host/interior pointer classification\),
  skipping when no SYCL device is present; runs against the OpenCL CPU
  device on GPU-less hosts
- docker/xpu.Dockerfile: build image based on intel/pytorch:xpu with the
  oneAPI DPC++ compiler added, so it both builds and runs the platform

Test on Intel B60 platform.")[#4030](https://github.com/kvcache-ai/Mooncake/pull/4030)[)](https://github.com/kvcache-ai/Mooncake/commit/fa601df23d53dd8edf2826dee7a71c24e7118e11 "\[TENT\] Add Intel XPU platform to the Transfer Engine \(#4030\)

Add an XpuPlatform to the TENT runtime so Mooncake can stage transfers
through Intel XPU \(GPU\) device memory. XpuPlatform extends CpuPlatform:
host DRAM allocation, NUMA probing, and host<->host copies are inherited
unchanged, and only the XPU-device-aware operations are overridden.

Implementation is a native, direct-link SYCL build: the platform sources
include <sycl/sycl.hpp> and link libsycl directly via a private
XpuSyclBackend \(device enumeration with GPU-preferred/any-device
fallback, sycl::malloc_device USM allocation, an interior-pointer
registry so chunked staging addresses classify correctly, and
VRAM<->host memcpy\). Because SYCL requires the Intel DPC++ compiler,
USE_XPU=ON now requires USE_TENT=ON and fails fast unless the build is
configured with icpx \(CXX=icpx\); -fsycl is applied to platform_xpu and
propagated to everything that links it.

- runtime: MTYPE_XPU memory type + USE_XPU loader branch
- platform: XpuPlatform \(probe/allocate/free/copy/getMemoryType/
  getLocation\) backed by XpuSyclBackend
- build: USE_XPU option/gate in common.cmake; platform_xpu with -fsycl
- test: tent_xpu_platform_test drives the real backend \(alloc -> H2D ->
  D2H -> byte-equality -> free, host/interior pointer classification\),
  skipping when no SYCL device is present; runs against the OpenCL CPU
  device on GPU-less hosts
- docker/xpu.Dockerfile: build image based on intel/pytorch:xpu with the
  oneAPI DPC++ compiler added, so it both builds and runs the platform

Test on Intel B60 platform.")  | Sep 14, 2026  |  
| [[Store] Add a per-tenant eviction watermark so quota pressure evicts …](https://github.com/kvcache-ai/Mooncake/commit/662cd44df877c003f6574a2543e266db0ed966a2 "\[Store\] Add a per-tenant eviction watermark so quota pressure evicts instead of rejecting \(#3805\)")  | Sep 14, 2026  |  
|   |   |   | Aug 24, 2026  |  
|   |   |  [[Doc] docs: add Biren logo to supported hardware (](https://github.com/kvcache-ai/Mooncake/commit/275cf358aa80a8570cab9bc88315add9b238dcc9 "\[Doc\] docs: add Biren logo to supported hardware \(#3298\)")[#3298](https://github.com/kvcache-ai/Mooncake/pull/3298)[)](https://github.com/kvcache-ai/Mooncake/commit/275cf358aa80a8570cab9bc88315add9b238dcc9 "\[Doc\] docs: add Biren logo to supported hardware \(#3298\)")  | Aug 6, 2026  |  
|   |   | [feat: expand Grafana dashboard with comprehensive Master metrics pane…](https://github.com/kvcache-ai/Mooncake/commit/3541bf506c68a4d239a0cbe690d6f317a71c0d0e "feat: expand Grafana dashboard with comprehensive Master metrics panels \(#2944\)

* feat: expand Grafana dashboard with comprehensive Master metrics panels

* fix: address review feedback on Grafana dashboard metrics

1. Serialize missing metrics in MasterMetricManager::serialize_metrics\(\):
   - NoF capacity gauges \(nof_allocated_size_, nof_total_capacity_,
     nof_allocated_size_per_segment_, nof_total_capacity_per_segment_\)
   - NoF segment mount/unmount/remount counters and their failures
   - Tiered eviction counters \(mem_eviction_*, nof_eviction_*\)
   - Batch operation items/partial_successes/failed_items counters

2. Fix division-by-constant in percentage panels \(id 704, 705\):
   Replace '+ 1' with clamp_min\(denominator, 1e-9\) to avoid ratio
   distortion at low traffic.

3. Fix histogram bucket rate misuse as latency \(id 803, 903\):
   Replace rate\(..._bucket\[5m\]\) with
   histogram_quantile\(0.95, sum by \(le\) \(rate\(..._bucket\[5m\]\)\)\)
   to show true latency percentiles.

* fix: apply histogram_quantile to value size panel \(id 1201\) for consistency

Panel 1201 had the same histogram bucket misuse issue as panels 803/903.
Replace bare 'master_value_size_bytes_bucket' with
histogram_quantile\(0.95, sum by \(le\) \(rate\(..._bucket\[5m\]\)\)\).
Update unit from 'short' to 'bytes' and title to reflect p95 value size.

* fix: resolve grid layout overlap and refresh interval

1. Grid layout overlap \(row 200 vs panel 108\):
   Row 200 was at y=9, overlapping with panel 108 \(y=9, h=8, ends at y=17\).
   Moved row 200 to y=17 and shifted all subsequent panels by +8.

2. Refresh interval too low:
   Changed dashboard refresh from 500ms to 5s, as 500ms is below Grafana's
   default min_refresh_interval \(5s\) and gets clamped with a warning.")  | Jul 21, 2026  |  
|   |   | [[Store] Drain in-flight RPCs and keep pools alive across client teard…](https://github.com/kvcache-ai/Mooncake/commit/fecdda7300d1f46beb2880ba14c1e1052580cce8 "\[Store\] Drain in-flight RPCs and keep pools alive across client teardown \(#3943\)

Fixes #3909: tearing down a read-side client after consecutive RPC timeouts segfaults in asio's epoll_reactor. Two lifetime violations, both verified by reading current main against ylt's documented contract \(only send_request is thread-safe; close/destruction must not race in-flight RPC\):

In-flight requests. MasterClient/DummyClient/ClientRequester destructors just dropped the client pool while co_await pool->send_request\(...\) coroutines could still be suspended. A shared RpcDrainGuard \(mooncake-common\) stops admitting new calls once draining and makes the destructor wait for in-flight ones, bounded at 30s with a loud error rather than silently proceeding.

Background reconnect coroutines. ylt's client_pool reconnect loop \(client_pool.hpp\) holds references into pool storage and resumes across sleeps, so no amount of request draining makes pool destruction safe mid-retry. RpcClientPool now hands out pools from a process-wide registry keyed by address instead of freeing them on teardown or address flap. Pools are one-per-distinct-address per process and deliberately process-lifetime.

Signed-off-by: Yufeng He <40085740+he-yufeng@users.noreply.github.com>")  | Sep 14, 2026  |  
|   |   |  [[Conductor] conductor: tiered prefix indexer (](https://github.com/kvcache-ai/Mooncake/commit/7dee8db1a68c5656ea699acf71617783a13633c7 "\[Conductor\] conductor: tiered prefix indexer \(#4041\)

* \[Conductor\] conductor: tiered prefix indexer

Third slice of the Mooncake Conductor upstreaming series, on top of the
prefix index types and hash strategy. Adds PrefixCacheTable, the
in-memory index that answers cache-hit queries across GPU, CPU and disk
tiers

Co-authored-by: Misak2333 <167268798+Misak2333@users.noreply.github.com>
Co-authored-by: Chase-R <lolopop.rong@gmail.com>")[#4041](https://github.com/kvcache-ai/Mooncake/pull/4041)[)](https://github.com/kvcache-ai/Mooncake/commit/7dee8db1a68c5656ea699acf71617783a13633c7 "\[Conductor\] conductor: tiered prefix indexer \(#4041\)

* \[Conductor\] conductor: tiered prefix indexer

Third slice of the Mooncake Conductor upstreaming series, on top of the
prefix index types and hash strategy. Adds PrefixCacheTable, the
in-memory index that answers cache-hit queries across GPU, CPU and disk
tiers

Co-authored-by: Misak2333 <167268798+Misak2333@users.noreply.github.com>
Co-authored-by: Chase-R <lolopop.rong@gmail.com>")  | Sep 14, 2026  |  
|   |   |  [[EP] Support NCCL ElasticBuffer recovery after rank replacement (](https://github.com/kvcache-ai/Mooncake/commit/a88bd082bc5469393347c1bcd85237c778502a5d "\[EP\] Support NCCL ElasticBuffer recovery after rank replacement \(#3963\)

* feat\(ep\): rebuild NCCL state after rank recovery

* fix\(ep\): allow reserved PG capacity during NCCL recovery

Validate the buffer's logical ranks independently of reserved PG mask capacity. Exchange expected world sizes before membership collectives so actual resizing is rejected consistently, and add unit and GPU recovery regressions.

* fix\(ep\): address NCCL recovery review feedback

Recognize the mooncake-cpu backend and use CPU metadata tensors for NCCL bootstrap. Call the always-registered NCCL capability binding directly. Move recovery integration tests into the EP suite while reusing the PG worker harness, add CPU-PG recovery coverage, and document the test command.")[#3963](https://github.com/kvcache-ai/Mooncake/pull/3963)[)](https://github.com/kvcache-ai/Mooncake/commit/a88bd082bc5469393347c1bcd85237c778502a5d "\[EP\] Support NCCL ElasticBuffer recovery after rank replacement \(#3963\)

* feat\(ep\): rebuild NCCL state after rank recovery

* fix\(ep\): allow reserved PG capacity during NCCL recovery

Validate the buffer's logical ranks independently of reserved PG mask capacity. Exchange expected world sizes before membership collectives so actual resizing is rejected consistently, and add unit and GPU recovery regressions.

* fix\(ep\): address NCCL recovery review feedback

Recognize the mooncake-cpu backend and use CPU metadata tensors for NCCL bootstrap. Call the always-registered NCCL capability binding directly. Move recovery integration tests into the EP suite while reusing the PG worker harness, add CPU-PG recovery coverage, and document the test command.")  | Sep 10, 2026  |  
|   |   |  [[Bugfix][TENT] Forward classic RDMA device filters (](https://github.com/kvcache-ai/Mooncake/commit/6cc1a100404ea40a13940b49616a9127c5662b1d "\[Bugfix\]\[TENT\] Forward classic RDMA device filters \(#4028\)

Preserve the device whitelist passed through the classic TransferEngine API
when MC_USE_TENT is enabled, and inject it into TENT's
`topology/rdma_whitelist` before topology discovery.

Keep explicit caller filters ahead of MC_TENT_CONF and MC_TE_FILTERS defaults,
retain filters across move operations and the pre-init setter, and leave empty
filters to the existing environment configuration.

Add compatibility, configuration-precedence, move-semantics, and real multi-HCA
regression coverage.

Co-authored-by: TTThanos <yaozhong.lyz@alibaba-inc.com>")[#4028](https://github.com/kvcache-ai/Mooncake/pull/4028)[)](https://github.com/kvcache-ai/Mooncake/commit/6cc1a100404ea40a13940b49616a9127c5662b1d "\[Bugfix\]\[TENT\] Forward classic RDMA device filters \(#4028\)

Preserve the device whitelist passed through the classic TransferEngine API
when MC_USE_TENT is enabled, and inject it into TENT's
`topology/rdma_whitelist` before topology discovery.

Keep explicit caller filters ahead of MC_TENT_CONF and MC_TE_FILTERS defaults,
retain filters across move operations and the pre-init setter, and leave empty
filters to the existing environment configuration.

Add compatibility, configuration-precedence, move-semantics, and real multi-HCA
regression coverage.

Co-authored-by: TTThanos <yaozhong.lyz@alibaba-inc.com>")  | Sep 14, 2026  |  
|   |   |  [[P2P-Store] Back off transfer status polling after a spin budget (](https://github.com/kvcache-ai/Mooncake/commit/4c72064c2975aa4e2838f7c26801bab30052f4d9 "\[P2P-Store\] Back off transfer status polling after a spin budget \(#3851\)

Signed-off-by: chlins <chlins.zhang@gmail.com>")[#3851](https://github.com/kvcache-ai/Mooncake/pull/3851)[)](https://github.com/kvcache-ai/Mooncake/commit/4c72064c2975aa4e2838f7c26801bab30052f4d9 "\[P2P-Store\] Back off transfer status polling after a spin budget \(#3851\)

Signed-off-by: chlins <chlins.zhang@gmail.com>")  | Sep 11, 2026  |  
|   |   |  [[PG] Fix CUDA enqueue-stream serialization (](https://github.com/kvcache-ai/Mooncake/commit/69d41d5e70c928a4e94a036716f489f2e868003b "\[PG\] Fix CUDA enqueue-stream serialization \(#3798\)")[#3798](https://github.com/kvcache-ai/Mooncake/pull/3798)[)](https://github.com/kvcache-ai/Mooncake/commit/69d41d5e70c928a4e94a036716f489f2e868003b "\[PG\] Fix CUDA enqueue-stream serialization \(#3798\)")  | Sep 3, 2026  |  
|   |   |  [[Reshard] Add KV cache reshard planning (](https://github.com/kvcache-ai/Mooncake/commit/6acb781ed17175ce707b3b7ab2290a831dab3488 "\[Reshard\] Add KV cache reshard planning \(#3564\)

* feat: Add kv reshard

* fix\(reshard\): repair KV merge integration

* fix

* Phase1

* put lowering to mooncake

* move lowering

* feat\(reshard\): add runtime-to-runtime KV cache transfer execution

- Harden snapshot, placement, and logical plan validation
- Add operation-scoped resolved bindings and bounded transfer lowering
- Add TE batch execution and writer/target completion contracts
- Preserve existing SGLang PD planning interfaces
- Add contract and native CPU/GPU TCP/RDMA transfer tests

* fix\(reshard\): tighten KV cache snapshot and memory validation

- Reject mismatched binding snapshots while preserving legacy PD calls
- Validate derived transfer byte sizes against uint64 bounds
- Scope address checks by runtime instance, worker, and device
- Reject conflicting backing allocations while allowing shared storage
- Consolidate boundary tests and remove redundant assertions

Signed-off-by: huanglong <huanglong@linux.alibaba.com>

* fix\(reshard\): quarantine KV executor on registration failures

Signed-off-by: huanglong <huanglong@linux.alibaba.com>

---------

Signed-off-by: huanglong <huanglong@linux.alibaba.com>")[#3564](https://github.com/kvcache-ai/Mooncake/pull/3564)[)](https://github.com/kvcache-ai/Mooncake/commit/6acb781ed17175ce707b3b7ab2290a831dab3488 "\[Reshard\] Add KV cache reshard planning \(#3564\)

* feat: Add kv reshard

* fix\(reshard\): repair KV merge integration

* fix

* Phase1

* put lowering to mooncake

* move lowering

* feat\(reshard\): add runtime-to-runtime KV cache transfer execution

- Harden snapshot, placement, and logical plan validation
- Add operation-scoped resolved bindings and bounded transfer lowering
- Add TE batch execution and writer/target completion contracts
- Preserve existing SGLang PD planning interfaces
- Add contract and native CPU/GPU TCP/RDMA transfer tests

* fix\(reshard\): tighten KV cache snapshot and memory validation

- Reject mismatched binding snapshots while preserving legacy PD calls
- Validate derived transfer byte sizes against uint64 bounds
- Scope address checks by runtime instance, worker, and device
- Reject conflicting backing allocations while allowing shared storage
- Consolidate boundary tests and remove redundant assertions

Signed-off-by: huanglong <huanglong@linux.alibaba.com>

* fix\(reshard\): quarantine KV executor on registration failures

Signed-off-by: huanglong <huanglong@linux.alibaba.com>

---------

Signed-off-by: huanglong <huanglong@linux.alibaba.com>")  | Sep 10, 2026  |  
| [mooncake-rl/examples](https://github.com/kvcache-ai/Mooncake/tree/main/mooncake-rl/examples "This path skips through empty directories")  | [mooncake-rl/examples](https://github.com/kvcache-ai/Mooncake/tree/main/mooncake-rl/examples "This path skips through empty directories")  |  [[RL] Add dummy example of RL training on mooncake store (](https://github.com/kvcache-ai/Mooncake/commit/e711b6fcc36270beb5193f24d2de2a68d2bbcd79 "\[RL\] Add dummy example of RL training on mooncake store \(#810\)

* feat: init rl training using store for rollout

* chore: update structure

* fix: object store setup and training")[#810](https://github.com/kvcache-ai/Mooncake/pull/810)[)](https://github.com/kvcache-ai/Mooncake/commit/e711b6fcc36270beb5193f24d2de2a68d2bbcd79 "\[RL\] Add dummy example of RL training on mooncake store \(#810\)

* feat: init rl training using store for rollout

* chore: update structure

* fix: object store setup and training")  | Sep 8, 2025  |  
|   |   | [[Store] Drain in-flight RPCs and keep pools alive across client teard…](https://github.com/kvcache-ai/Mooncake/commit/fecdda7300d1f46beb2880ba14c1e1052580cce8 "\[Store\] Drain in-flight RPCs and keep pools alive across client teardown \(#3943\)

Fixes #3909: tearing down a read-side client after consecutive RPC timeouts segfaults in asio's epoll_reactor. Two lifetime violations, both verified by reading current main against ylt's documented contract \(only send_request is thread-safe; close/destruction must not race in-flight RPC\):

In-flight requests. MasterClient/DummyClient/ClientRequester destructors just dropped the client pool while co_await pool->send_request\(...\) coroutines could still be suspended. A shared RpcDrainGuard \(mooncake-common\) stops admitting new calls once draining and makes the destructor wait for in-flight ones, bounded at 30s with a loud error rather than silently proceeding.

Background reconnect coroutines. ylt's client_pool reconnect loop \(client_pool.hpp\) holds references into pool storage and resumes across sleeps, so no amount of request draining makes pool destruction safe mid-retry. RpcClientPool now hands out pools from a process-wide registry keyed by address instead of freeing them on teardown or address flap. Pools are one-per-distinct-address per process and deliberately process-lifetime.

Signed-off-by: Yufeng He <40085740+he-yufeng@users.noreply.github.com>")  | Sep 14, 2026  |  
| [mooncake-transfer-engine](https://github.com/kvcache-ai/Mooncake/tree/main/mooncake-transfer-engine "mooncake-transfer-engine")  | [mooncake-transfer-engine](https://github.com/kvcache-ai/Mooncake/tree/main/mooncake-transfer-engine "mooncake-transfer-engine")  |  [[Bugfix][TENT] Forward classic RDMA device filters (](https://github.com/kvcache-ai/Mooncake/commit/6cc1a100404ea40a13940b49616a9127c5662b1d "\[Bugfix\]\[TENT\] Forward classic RDMA device filters \(#4028\)

Preserve the device whitelist passed through the classic TransferEngine API
when MC_USE_TENT is enabled, and inject it into TENT's
`topology/rdma_whitelist` before topology discovery.

Keep explicit caller filters ahead of MC_TENT_CONF and MC_TE_FILTERS defaults,
retain filters across move operations and the pre-init setter, and leave empty
filters to the existing environment configuration.

Add compatibility, configuration-precedence, move-semantics, and real multi-HCA
regression coverage.

Co-authored-by: TTThanos <yaozhong.lyz@alibaba-inc.com>")[#4028](https://github.com/kvcache-ai/Mooncake/pull/4028)[)](https://github.com/kvcache-ai/Mooncake/commit/6cc1a100404ea40a13940b49616a9127c5662b1d "\[Bugfix\]\[TENT\] Forward classic RDMA device filters \(#4028\)

Preserve the device whitelist passed through the classic TransferEngine API
when MC_USE_TENT is enabled, and inject it into TENT's
`topology/rdma_whitelist` before topology discovery.

Keep explicit caller filters ahead of MC_TENT_CONF and MC_TE_FILTERS defaults,
retain filters across move operations and the pre-init setter, and leave empty
filters to the existing environment configuration.

Add compatibility, configuration-precedence, move-semantics, and real multi-HCA
regression coverage.

Co-authored-by: TTThanos <yaozhong.lyz@alibaba-inc.com>")  | Sep 14, 2026  |  
|   |   |  [[CI/Build] Retry transient PUT failures in SSD eviction test (](https://github.com/kvcache-ai/Mooncake/commit/202ad9c89e1814f0273c605cc12b3ccb1c8117c6 "\[CI/Build\] Retry transient PUT failures in SSD eviction test \(#3875\)")[#3875](https://github.com/kvcache-ai/Mooncake/pull/3875)[)](https://github.com/kvcache-ai/Mooncake/commit/202ad9c89e1814f0273c605cc12b3ccb1c8117c6 "\[CI/Build\] Retry transient PUT failures in SSD eviction test \(#3875\)")  | Sep 13, 2026  |  
|   |   |  [[EP] Support NCCL ElasticBuffer recovery after rank replacement (](https://github.com/kvcache-ai/Mooncake/commit/a88bd082bc5469393347c1bcd85237c778502a5d "\[EP\] Support NCCL ElasticBuffer recovery after rank replacement \(#3963\)

* feat\(ep\): rebuild NCCL state after rank recovery

* fix\(ep\): allow reserved PG capacity during NCCL recovery

Validate the buffer's logical ranks independently of reserved PG mask capacity. Exchange expected world sizes before membership collectives so actual resizing is rejected consistently, and add unit and GPU recovery regressions.

* fix\(ep\): address NCCL recovery review feedback

Recognize the mooncake-cpu backend and use CPU metadata tensors for NCCL bootstrap. Call the always-registered NCCL capability binding directly. Move recovery integration tests into the EP suite while reusing the PG worker harness, add CPU-PG recovery coverage, and document the test command.")[#3963](https://github.com/kvcache-ai/Mooncake/pull/3963)[)](https://github.com/kvcache-ai/Mooncake/commit/a88bd082bc5469393347c1bcd85237c778502a5d "\[EP\] Support NCCL ElasticBuffer recovery after rank replacement \(#3963\)

* feat\(ep\): rebuild NCCL state after rank recovery

* fix\(ep\): allow reserved PG capacity during NCCL recovery

Validate the buffer's logical ranks independently of reserved PG mask capacity. Exchange expected world sizes before membership collectives so actual resizing is rejected consistently, and add unit and GPU recovery regressions.

* fix\(ep\): address NCCL recovery review feedback

Recognize the mooncake-cpu backend and use CPU metadata tensors for NCCL bootstrap. Call the always-registered NCCL capability binding directly. Move recovery integration tests into the EP suite while reusing the PG worker harness, add CPU-PG recovery coverage, and document the test command.")  | Sep 10, 2026  |  
|   |   | [[Store] Support DeepSeek-V4.1-Flash Engram with local and Store-backe…](https://github.com/kvcache-ai/Mooncake/commit/68484eba55cbac6012d032c2107efd2ef28223f8 "\[Store\] Support DeepSeek-V4.1-Flash Engram with local and Store-backed byte tables \(#4065\)

* \[Store\] Support multi-layer byte Engram tables with caller-owned output

Replace the float32-specific Engram layout with a required positive row_bytes field and retain fixed per-layer, per-head keys. One EngramStore manages a map of layer layouts over a shared Store client; reads, writes and removal select a layer explicitly.

Expose lookup_into as the only read interface, writing directly into caller-owned, pre-registered uint8 output without allocating or registering output internally. Pass replication configuration through populate, validate row sizes and buffer layouts, and update integration tests, the benchmark, and API documentation.

Validation: Store binding build, 17 integration tests, small benchmark smoke, scoped pre-commit and documentation HTML build passed.

---------

Signed-off-by: Teng Ma <stmatengss@gmail.com>")  | Sep 13, 2026  |  
|   |   |  [code format & enable code format checking in ci (](https://github.com/kvcache-ai/Mooncake/commit/4e03dbe0139a78a7c90fdcc1cb85d2b82f70b91d "code format & enable code format checking in ci \(#677\)

Signed-off-by: doujiang24 <doujiang24@gmail.com>")[#677](https://github.com/kvcache-ai/Mooncake/pull/677)[)](https://github.com/kvcache-ai/Mooncake/commit/4e03dbe0139a78a7c90fdcc1cb85d2b82f70b91d "code format & enable code format checking in ci \(#677\)

Signed-off-by: doujiang24 <doujiang24@gmail.com>")  | Aug 2, 2025  |  
|   |   |  [[Build] update dockerfile and install mooncake from scratch (](https://github.com/kvcache-ai/Mooncake/commit/1abb593ad89c6edef5a51a10879f985897279a83 "\[Build\] update dockerfile and install mooncake from scratch \(#1214\)

* \[Misc\] add new dockerfile

* fix ci

* Add musa.Dockerfile

Signed-off-by: Xiaodong Ye <xiaodong.ye@mthreads.com>

* Update docker/mooncake.Dockerfile

* Apply suggestions from code review

* Fix Dockerfile issues: add submodule init, remove duplicate pip install, add newline

- Add missing git submodule update command in mooncake.Dockerfile
- Remove duplicate pip install command in mooncake.Dockerfile \(was installing twice\)
- Add missing newline at end of musa.Dockerfile

* Simplify Dockerfiles by removing unimportant steps

Removed unnecessary steps to streamline the build process:
- Remove separate pip upgrade \(dependencies.sh handles this\)
- Remove git submodule init \(not needed for Docker builds from source\)
- Remove bash -x verbose flag \(reduces noise\)
- Remove LD_LIBRARY_PATH setting \(cmake install handles this\)
- Remove venv creation in runtime \(install directly to system Python\)
- Remove WORKDIR /workspace in runtime \(not needed\)
- Remove unused packages: python3-distutils, python3-venv, python3-setuptools, curl, netcat
- Remove redundant pip upgrade in musa runtime

This reduces image size and build time while maintaining functionality.

* reduce size

---------

Signed-off-by: Xiaodong Ye <xiaodong.ye@mthreads.com>
Co-authored-by: Xiaodong Ye <xiaodong.ye@mthreads.com>")[#1214](https://github.com/kvcache-ai/Mooncake/pull/1214)[)](https://github.com/kvcache-ai/Mooncake/commit/1abb593ad89c6edef5a51a10879f985897279a83 "\[Build\] update dockerfile and install mooncake from scratch \(#1214\)

* \[Misc\] add new dockerfile

* fix ci

* Add musa.Dockerfile

Signed-off-by: Xiaodong Ye <xiaodong.ye@mthreads.com>

* Update docker/mooncake.Dockerfile

* Apply suggestions from code review

* Fix Dockerfile issues: add submodule init, remove duplicate pip install, add newline

- Add missing git submodule update command in mooncake.Dockerfile
- Remove duplicate pip install command in mooncake.Dockerfile \(was installing twice\)
- Add missing newline at end of musa.Dockerfile

* Simplify Dockerfiles by removing unimportant steps

Removed unnecessary steps to streamline the build process:
- Remove separate pip upgrade \(dependencies.sh handles this\)
- Remove git submodule init \(not needed for Docker builds from source\)
- Remove bash -x verbose flag \(reduces noise\)
- Remove LD_LIBRARY_PATH setting \(cmake install handles this\)
- Remove venv creation in runtime \(install directly to system Python\)
- Remove WORKDIR /workspace in runtime \(not needed\)
- Remove unused packages: python3-distutils, python3-venv, python3-setuptools, curl, netcat
- Remove redundant pip upgrade in musa runtime

This reduces image size and build time while maintaining functionality.

* reduce size

---------

Signed-off-by: Xiaodong Ye <xiaodong.ye@mthreads.com>
Co-authored-by: Xiaodong Ye <xiaodong.ye@mthreads.com>")  | Feb 25, 2026  |  
|   |   |   | Jul 4, 2026  |  
|   |   |   | Aug 24, 2026  |  
| [.pre-commit-config.yaml](https://github.com/kvcache-ai/Mooncake/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  | [.pre-commit-config.yaml](https://github.com/kvcache-ai/Mooncake/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  |  [[Reshard] Add resource and model-weight manifest contracts (](https://github.com/kvcache-ai/Mooncake/commit/0af9ed7e0b9f482624a1dcc856701c7d2e1b8cd2 "\[Reshard\] Add resource and model-weight manifest contracts \(#3187\)

* feat\(reshard\): define resource and weight manifest contracts

Signed-off-by: Vincent Gao <vincentbo@linux.alibaba.com>

* test\(reshard\): cover manifest contracts and runtime binding

Signed-off-by: Vincent Gao <vincentbo@linux.alibaba.com>

* build\(reshard\): stage contracts in the Mooncake wheel

Signed-off-by: Vincent Gao <vincentbo@linux.alibaba.com>

* docs\(reshard\): document split placement and binding contracts

Signed-off-by: Vincent Gao <vincentbo@linux.alibaba.com>

* chore\(reshard\): register module ownership and labels

Signed-off-by: Vincent Gao <vincentbo@linux.alibaba.com>

* Apply suggestion from @stmatengss

---------

Signed-off-by: Vincent Gao <vincentbo@linux.alibaba.com>
Co-authored-by: Teng Ma <stmatengss@gmail.com>")[#3187](https://github.com/kvcache-ai/Mooncake/pull/3187)[)](https://github.com/kvcache-ai/Mooncake/commit/0af9ed7e0b9f482624a1dcc856701c7d2e1b8cd2 "\[Reshard\] Add resource and model-weight manifest contracts \(#3187\)

* feat\(reshard\): define resource and weight manifest contracts

Signed-off-by: Vincent Gao <vincentbo@linux.alibaba.com>

* test\(reshard\): cover manifest contracts and runtime binding

Signed-off-by: Vincent Gao <vincentbo@linux.alibaba.com>

* build\(reshard\): stage contracts in the Mooncake wheel

Signed-off-by: Vincent Gao <vincentbo@linux.alibaba.com>

* docs\(reshard\): document split placement and binding contracts

Signed-off-by: Vincent Gao <vincentbo@linux.alibaba.com>

* chore\(reshard\): register module ownership and labels

Signed-off-by: Vincent Gao <vincentbo@linux.alibaba.com>

* Apply suggestion from @stmatengss

---------

Signed-off-by: Vincent Gao <vincentbo@linux.alibaba.com>
Co-authored-by: Teng Ma <stmatengss@gmail.com>")  | Aug 13, 2026  |  
|   |   |  [[Conductor] conductor: prefix index types and hash strategy (](https://github.com/kvcache-ai/Mooncake/commit/1b78033dea7a2861dffa7a9674e707d0463b967d "\[Conductor\] conductor: prefix index types and hash strategy \(#3879\)

Co-authored-by: Misak2333 <167268798+Misak2333@users.noreply.github.com>")[#3879](https://github.com/kvcache-ai/Mooncake/pull/3879)[)](https://github.com/kvcache-ai/Mooncake/commit/1b78033dea7a2861dffa7a9674e707d0463b967d "\[Conductor\] conductor: prefix index types and hash strategy \(#3879\)

Co-authored-by: Misak2333 <167268798+Misak2333@users.noreply.github.com>")  | Sep 11, 2026  |  
|   |   |  [[Doc] Align pre-commit guidance to PR-changed files (](https://github.com/kvcache-ai/Mooncake/commit/1b6b711850c6501156b56504be2445b1c6cafc0e "\[Doc\] Align pre-commit guidance to PR-changed files \(#3277\)

* \[Doc\] Align pre-commit guidance to PR-changed files

Recommend running pre-commit on staged or PR-scoped files instead of
--all-files, so routine PRs do not pick up unrelated historical rewrites.

Signed-off-by: staryxchen <staryxchen@tencent.com>
Co-authored-by: Cursor <cursoragent@cursor.com>

* \[Doc\] Fix PR-scoped pre-commit base to origin/main

Use `origin/main...HEAD` instead of local `main...HEAD` so the
diff base always reflects the upstream tip; prepend `git fetch origin main`
to keep the remote ref fresh before resolving the file list.

Local `main` lags behind when working long on a feature branch, which
would include upstream files unrelated to the PR.

Addresses Aionw's review comment on PR#3277.

Signed-off-by: staryxchen <staryxchen@tencent.com>

* \[Doc\] Fix PR-scoped pre-commit base in CONTRIBUTING.md

Sync the PR-scoped pre-commit command in CONTRIBUTING.md with the
preceding fix in .pre-commit-config.yaml: use `origin/main...HEAD`
instead of local `main...HEAD` and prepend `git fetch origin main`
so the remote ref stays fresh before resolving the file list.

Part of PR#3277 review feedback from Aionw.

Signed-off-by: staryxchen <staryxchen@tencent.com>

---------

Signed-off-by: staryxchen <staryxchen@tencent.com>
Co-authored-by: Cursor <cursoragent@cursor.com>")[#3277](https://github.com/kvcache-ai/Mooncake/pull/3277)[)](https://github.com/kvcache-ai/Mooncake/commit/1b6b711850c6501156b56504be2445b1c6cafc0e "\[Doc\] Align pre-commit guidance to PR-changed files \(#3277\)

* \[Doc\] Align pre-commit guidance to PR-changed files

Recommend running pre-commit on staged or PR-scoped files instead of
--all-files, so routine PRs do not pick up unrelated historical rewrites.

Signed-off-by: staryxchen <staryxchen@tencent.com>
Co-authored-by: Cursor <cursoragent@cursor.com>

* \[Doc\] Fix PR-scoped pre-commit base to origin/main

Use `origin/main...HEAD` instead of local `main...HEAD` so the
diff base always reflects the upstream tip; prepend `git fetch origin main`
to keep the remote ref fresh before resolving the file list.

Local `main` lags behind when working long on a feature branch, which
would include upstream files unrelated to the PR.

Addresses Aionw's review comment on PR#3277.

Signed-off-by: staryxchen <staryxchen@tencent.com>

* \[Doc\] Fix PR-scoped pre-commit base in CONTRIBUTING.md

Sync the PR-scoped pre-commit command in CONTRIBUTING.md with the
preceding fix in .pre-commit-config.yaml: use `origin/main...HEAD`
instead of local `main...HEAD` and prepend `git fetch origin main`
so the remote ref stays fresh before resolving the file list.

Part of PR#3277 review feedback from Aionw.

Signed-off-by: staryxchen <staryxchen@tencent.com>

---------

Signed-off-by: staryxchen <staryxchen@tencent.com>
Co-authored-by: Cursor <cursoragent@cursor.com>")  | Aug 11, 2026  |  
|   |   |   | Jul 4, 2026  |  
|   |   |  [[Build] conductor: build skeleton and common layer (](https://github.com/kvcache-ai/Mooncake/commit/5c68052c6327f804eec506185b62398229ac3afb "\[Build\] conductor: build skeleton and common layer \(#3825\)

* \[Build\] conductor: build skeleton and common layer

First slice of the Mooncake Conductor upstreaming series. Adds the
component skeleton so the later slices are pure additions:

- mooncake-conductor/CMakeLists.txt building the conductor_cpp_core
  static library, with a standalone-build guard so the component also
  configures on its own \(outside the top-level GLOBAL_CONFIG build\).
- common layer: ServiceConfig / HashProfileConfig data model
  \(common/types.h\) and the environment-variable and log-level helpers
  \(common/utils.{h,cpp}\).
- tests/CMakeLists.txt with the conductor_test gtest target, covering
  the env helpers and the JSON uint64 round-trip contract that the
  service HTTP responses rely on.
- WITH_CONDUCTOR option \(default OFF\) wired into the top-level build,
  enabled in the CI unit-test configure so conductor_test is picked up
  by the existing unified CTest run, and documented in build.md.

No conductor binary is produced yet; src/main.cpp arrives with the KV
event manager slice.

Co-Authored-By: Misak2333 <167268798+Misak2333@users.noreply.github.com>

* \[Build\] conductor: use the top-level googletest targets when present

* \[Build\] conductor: nest common namespace under mooncake

Address review feedback on #3825: use mooncake::conductor::common instead
of a top-level conductor namespace, matching the convention used across the
tree \(mooncake::ha, mooncake::tent::ub, mooncake::offset_allocator\).

Co-Authored-By: Claude Opus 5 \(1M context\) <noreply@anthropic.com>

* \[Build\] conductor: reuse mooncake-common parsing helpers

---------

Co-authored-by: rongchenghao <r00875518@china.huawei.com>
Co-authored-by: Misak2333 <167268798+Misak2333@users.noreply.github.com>
Co-authored-by: Claude Opus 5 \(1M context\) <noreply@anthropic.com>")[#3825](https://github.com/kvcache-ai/Mooncake/pull/3825)[)](https://github.com/kvcache-ai/Mooncake/commit/5c68052c6327f804eec506185b62398229ac3afb "\[Build\] conductor: build skeleton and common layer \(#3825\)

* \[Build\] conductor: build skeleton and common layer

First slice of the Mooncake Conductor upstreaming series. Adds the
component skeleton so the later slices are pure additions:

- mooncake-conductor/CMakeLists.txt building the conductor_cpp_core
  static library, with a standalone-build guard so the component also
  configures on its own \(outside the top-level GLOBAL_CONFIG build\).
- common layer: ServiceConfig / HashProfileConfig data model
  \(common/types.h\) and the environment-variable and log-level helpers
  \(common/utils.{h,cpp}\).
- tests/CMakeLists.txt with the conductor_test gtest target, covering
  the env helpers and the JSON uint64 round-trip contract that the
  service HTTP responses rely on.
- WITH_CONDUCTOR option \(default OFF\) wired into the top-level build,
  enabled in the CI unit-test configure so conductor_test is picked up
  by the existing unified CTest run, and documented in build.md.

No conductor binary is produced yet; src/main.cpp arrives with the KV
event manager slice.

Co-Authored-By: Misak2333 <167268798+Misak2333@users.noreply.github.com>

* \[Build\] conductor: use the top-level googletest targets when present

* \[Build\] conductor: nest common namespace under mooncake

Address review feedback on #3825: use mooncake::conductor::common instead
of a top-level conductor namespace, matching the convention used across the
tree \(mooncake::ha, mooncake::tent::ub, mooncake::offset_allocator\).

Co-Authored-By: Claude Opus 5 \(1M context\) <noreply@anthropic.com>

* \[Build\] conductor: reuse mooncake-common parsing helpers

---------

Co-authored-by: rongchenghao <r00875518@china.huawei.com>
Co-authored-by: Misak2333 <167268798+Misak2333@users.noreply.github.com>
Co-authored-by: Claude Opus 5 \(1M context\) <noreply@anthropic.com>")  | Sep 3, 2026  |  
|   |   |  [[Chore] Add Contributor Covenant Code of Conduct (](https://github.com/kvcache-ai/Mooncake/commit/ad2097bf0d0307f22517a6f0f193eb5c4b0a318d "\[Chore\] Add Contributor Covenant Code of Conduct \(#1056\)

This document outlines the Contributor Covenant Code of Conduct, including our pledge, standards of behavior, enforcement responsibilities, and consequences for violations.")[#1056](https://github.com/kvcache-ai/Mooncake/pull/1056)[)](https://github.com/kvcache-ai/Mooncake/commit/ad2097bf0d0307f22517a6f0f193eb5c4b0a318d "\[Chore\] Add Contributor Covenant Code of Conduct \(#1056\)

This document outlines the Contributor Covenant Code of Conduct, including our pledge, standards of behavior, enforcement responsibilities, and consequences for violations.")  | Nov 17, 2025  |  
|   |   |  [[Conductor] Add PR classification and automatic labeling (](https://github.com/kvcache-ai/Mooncake/commit/f40474e839d6ba9bb8aa81c63c888a238b5bc09d "\[Conductor\] Add PR classification and automatic labeling \(#4010\)")[#4010](https://github.com/kvcache-ai/Mooncake/pull/4010)[)](https://github.com/kvcache-ai/Mooncake/commit/f40474e839d6ba9bb8aa81c63c888a238b5bc09d "\[Conductor\] Add PR classification and automatic labeling \(#4010\)")  | Sep 11, 2026  |  
|   |   | [Squashed commits related to transfer engine](https://github.com/kvcache-ai/Mooncake/commit/348afd75319025441b8e2024a98f56f153245a4f "Squashed commits related to transfer engine")  | Nov 27, 2024  |  
|   |   |  [[Doc] Add AWS and MetaX maintainer logos (](https://github.com/kvcache-ai/Mooncake/commit/4d9a36d806b532093f752249c2316dcf222e4152 "\[Doc\] Add AWS and MetaX maintainer logos \(#3430\)

Co-authored-by: copilot-swe-agent\[bot\] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: stmatengss <11641725+stmatengss@users.noreply.github.com>")[#3430](https://github.com/kvcache-ai/Mooncake/pull/3430)[)](https://github.com/kvcache-ai/Mooncake/commit/4d9a36d806b532093f752249c2316dcf222e4152 "\[Doc\] Add AWS and MetaX maintainer logos \(#3430\)

Co-authored-by: copilot-swe-agent\[bot\] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: stmatengss <11641725+stmatengss@users.noreply.github.com>")  | Aug 14, 2026  |  
|   |   |  [docs: update README AgentX news (](https://github.com/kvcache-ai/Mooncake/commit/0a09c221eddaba0cef40156606d3ab60dc119c9a "docs: update README AgentX news \(#4011\)

Co-authored-by: copilot-swe-agent\[bot\] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: stmatengss <11641725+stmatengss@users.noreply.github.com>")[#4011](https://github.com/kvcache-ai/Mooncake/pull/4011)[)](https://github.com/kvcache-ai/Mooncake/commit/0a09c221eddaba0cef40156606d3ab60dc119c9a "docs: update README AgentX news \(#4011\)

Co-authored-by: copilot-swe-agent\[bot\] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: stmatengss <11641725+stmatengss@users.noreply.github.com>")  | Sep 11, 2026  |  
|   |   |  [[CI/Build] Enable KV events in wheels with ZeroMQ dependencies (](https://github.com/kvcache-ai/Mooncake/commit/5f2a5c6d8b0e6100a09bbd84b02dacc5e5d7c08f "\[CI/Build\] Enable KV events in wheels with ZeroMQ dependencies \(#4060\)



---------

Signed-off-by: Ishan Dhanani <ishandhanani@gmail.com>
Co-authored-by: Ishan Dhanani <ishandhanani@gmail.com>")[#4060](https://github.com/kvcache-ai/Mooncake/pull/4060)[)](https://github.com/kvcache-ai/Mooncake/commit/5f2a5c6d8b0e6100a09bbd84b02dacc5e5d7c08f "\[CI/Build\] Enable KV events in wheels with ZeroMQ dependencies \(#4060\)



---------

Signed-off-by: Ishan Dhanani <ishandhanani@gmail.com>
Co-authored-by: Ishan Dhanani <ishandhanani@gmail.com>")  | Sep 13, 2026  |  
|   |   |  [[Build] Establish scikit-build-core Python foundation (](https://github.com/kvcache-ai/Mooncake/commit/23b3d7d94cbfcf3cf83df05e0cca510a14115ab1 "\[Build\] Establish scikit-build-core Python foundation \(#3577\)")[#3577](https://github.com/kvcache-ai/Mooncake/pull/3577)[)](https://github.com/kvcache-ai/Mooncake/commit/23b3d7d94cbfcf3cf83df05e0cca510a14115ab1 "\[Build\] Establish scikit-build-core Python foundation \(#3577\)")  | Aug 21, 2026  |  
|   |   |  [[Reshard] Add N-D logical weight transfer planner (](https://github.com/kvcache-ai/Mooncake/commit/777cc7782417b6e554cf7c2d53210d0d8f89f5cc "\[Reshard\] Add N-D logical weight transfer planner \(#3441\)")[#3441](https://github.com/kvcache-ai/Mooncake/pull/3441)[)](https://github.com/kvcache-ai/Mooncake/commit/777cc7782417b6e554cf7c2d53210d0d8f89f5cc "\[Reshard\] Add N-D logical weight transfer planner \(#3441\)")  | Aug 21, 2026  |  
| View all files |  
## Repository files navigation
##  A KVCache-centric Disaggregated Architecture for LLM Serving 
[**Paper**](https://www.usenix.org/system/files/fast25-qin.pdf) | [**Slides**](https://www.usenix.org/system/files/fast25_slides-qin.pdf) | [**Traces**](https://github.com/kvcache-ai/Mooncake/blob/main/FAST25-release/traces) | [**Documentation**](https://kvcache-ai.github.io/Mooncake/) | [**Blog**](https://kvcache.ai/) | [**Slack**](https://join.slack.com/t/mooncake-project/shared_invite/zt-3qx4x35ea-zSSTqTHItHJs9SCoXLOSPA)
Mooncake is the serving platform for [ Kimi](https://kimi.ai/), a leading LLM service provided by [ Moonshot AI](https://www.moonshot.cn/). Under real workloads, Mooncake’s innovative architecture enables Kimi to handle 75% more requests while adhering to SLOs.
## 🔄 Updates
  * **Aug 24, 2026** : [AgentX / InferenceX v3](https://newsletter.semianalysis.com/p/agentx-inferencexv3-does-cuda-moat) highlights Mooncake's contributions to agentic inference workloads, including ROCm wheels, CI, and packaging improvements. [Mooncake optimization details](https://inferencex.semianalysis.com/agentx/optimizations/mooncake).
  * **Aug 20, 2026** : Mooncake is integrated into [Miles](https://github.com/radixark/miles) as a rollout data-transfer backend for the fragmented, heterogeneous data moving between rollout and training in disaggregated RL. Blogs: [KVCache.AI](https://kvcache.ai/blog/mooncake-rl-rollout-data-transfer/), [lmsys](https://www.lmsys.org/blog/2026-08-20-miles-mooncake-rollout-data-transfer).
  * **Aug 17, 2026** : Mooncake is integrated into [Speculators](https://github.com/vllm-project/speculators) as a distributed backend for multi-node online training, efficiently moves hidden-state between vLLM inference workers and Speculators trainers through RDMA, eliminating the need for massive hidden-state storage in offline training. [Benchmark on GB300 NVL72](https://x.com/mgoin_/status/2072785822231728363).
  * **May 7, 2026** : 🚀 [vLLM officially features Mooncake Store](https://vllm.ai/blog/mooncake-store) — a deep dive into how Mooncake's distributed KVCache engine supercharges vLLM inference with high-throughput, memory-efficient, cross-instance KV cache sharing!
  * **Apr 29, 2026** : SGLang introduces [RDMA-based P2P weight transfer for large-scale distributed RL](https://lmsys.org/blog/2026-04-29-p2p-update/) using Mooncake TransferEngine, achieving 7x faster weight updates for the 1T-parameter Kimi-K2 model (53s → 7.2s) with zero-copy RDMA transfer across thousands of GPUs.
  * **Mar 19, 2026** : [TorchSpec: Speculative Decoding Training at Scale](https://pytorch.org/blog/torchspec-speculative-decoding-training-at-scale) is [open sourced](https://github.com/torchspec-project/TorchSpec), using Mooncake to decouple inference and training via efficient hidden states management.

More
  * **Mar 5, 2026** : [LightX2V](https://github.com/ModelTC/LightX2V/pull/893) now supports disaggregated deployment based on Mooncake, enabling encoder/transformer service decoupling with Mooncake Transfer Engine for high-performance cross-device and cross-machine data transfer. Details in [blog](https://light-ai.top/LightX2V-BLOG/posts/Disaggregation/).
  * **Feb 25, 2026** : [SGLang](https://github.com/sgl-project/sglang) merged [Encoder Global Cache Manager](https://github.com/sgl-project/sglang/pull/16137), introducing a Mooncake-powered global multimodal embedding cache that enables cross-instance sharing of ViT embeddings to avoid redundant GPU computation.
  * **Feb 24, 2026** : [vLLM-Omni](https://docs.vllm.ai/projects/vllm-omni/en/latest/design/feature/disaggregated_inference/) introduces disaggregated inference connectors with support for both `MooncakeStoreConnector` and `MooncakeTransferEngineConnector` for multi-node omni-modality pipelines.
  * **Feb 12, 2026** : [Mooncake Joins PyTorch Ecosystem](https://pytorch.org/blog/mooncake-joins-pytorch-ecosystem/) We are thrilled to announce that Mooncake has officially joined the PyTorch Ecosystem!
  * **Jan 28, 2026** : [FlexKV](https://github.com/taco-project/FlexKV), a distributed KV store and cache system from Tencent and NVIDIA in collaboration with the community, now supports [distributed KVCache reuse](https://github.com/taco-project/FlexKV/blob/main/docs/dist_reuse/README_en.md) with the Mooncake Transfer Engine.
  * **Dec 27, 2025** : Collaboration with [ROLL](https://github.com/alibaba/ROLL)! Check out the paper [here](https://arxiv.org/abs/2512.22560).
  * **Dec 23, 2025** : SGLang introduces [Encode-Prefill-Decode (EPD) Disaggregation](https://lmsys.org/blog/2026-01-12-epd/) with Mooncake as a transfer backend. This integration allows decoupling compute-intensive multimodal encoders (e.g., Vision Transformers) from language model nodes, utilizing Mooncake's RDMA engine for zero-copy transfer of large multimodal embeddings.
  * **Dec 19, 2025** : Mooncake Transfer Engine has been [integrated into TensorRT LLM](https://github.com/NVIDIA/TensorRT-LLM/tree/main/cpp/tensorrt_llm/executor/cache_transmission/mooncake_utils) for KVCache transfer in PD-disaggregated inference.
  * **Dec 19, 2025** : Mooncake Transfer Engine has been directly integrated into vLLM v1 as a [KV Connector](https://docs.vllm.ai/en/latest/features/mooncake_connector_usage/) in PD-disaggregated setups.
  * **Nov 07, 2025** : [RBG + SGLang HiCache + Mooncake](https://github.com/sgl-project/rbg/blob/main/keps/74-mooncake-integration/README.md), a role-based out-of-the-box solution for cloud native deployment, which is elastic, scalable, and high-performance.
  * **Sept 18, 2025** : Mooncake Store empowers vLLM Ascend by serving as [the distributed KV cache pool backend](https://docs.vllm.ai/projects/ascend/zh-cn/main/user_guide/feature_guide/kv_pool.html).
  * **Sept 10, 2025** : SGLang officially supports Mooncake Store as a [hierarchical KV caching storage backend](https://lmsys.org/blog/2025-09-10-sglang-hicache/). The integration extends RadixAttention with multi-tier KV cache storage across device, host, and remote storage layers.
  * **Sept 10, 2025** : The official & high-performance version of Mooncake P2P Store is open-sourced as [checkpoint-engine](https://github.com/MoonshotAI/checkpoint-engine/). It has been successfully applied in K1.5 and K2 production training, updating Kimi-K2 model (1T parameters) across thousands of GPUs in ~20s.
  * **Aug 23, 2025** : [xLLM](https://github.com/jd-opensource/xllm) high-performance inference engine builds hybrid KV cache management based on Mooncake, supporting global KV cache management with intelligent offloading and prefetching.
  * **Aug 18, 2025** : vLLM-Ascend [integrates Mooncake Transfer Engine](https://docs.vllm.ai/projects/ascend/en/latest/developer_guide/feature_guide/disaggregated_prefill.html) for KV cache register and disaggregate prefill, enabling efficient distributed inference on Ascend NPUs.
  * **Jul 20, 2025** : Mooncake powers [the deployment of Kimi K2](https://lmsys.org/blog/2025-07-20-k2-large-scale-ep/) on 128 H200 GPUs with PD disaggregation and large-scale expert parallelism, achieving 224k tokens/sec prefill throughput and 288k tokens/sec decode throughput.
  * **Jun 20, 2025** : Mooncake becomes a PD disaggregation [backend](https://kvcache-ai.github.io/Mooncake/deployment/integrations/lmdeploy.html) for LMDeploy.
  * **May 9, 2025** : NIXL officially supports Mooncake Transfer Engine as [a backend plugin](https://github.com/ai-dynamo/nixl/blob/main/src/plugins/mooncake/README.md).
  * **May 8, 2025** : [Mooncake x LMCache](https://kvcache-ai.github.io/Mooncake/deployment/integrations/lmcache/index.html) unite to pioneer KVCache-centric LLM serving system.
  * **May 5, 2025** : Supported by Mooncake Team, SGLang release [guidance](https://lmsys.org/blog/2025-05-05-large-scale-ep/) to deploy DeepSeek with PD Disaggregation on 96 H100 GPUs.
  * **Apr 22, 2025** : LMCache officially supports Mooncake Store as a [remote connector](https://blog.lmcache.ai/2025-04-22-tencent/).
  * **Apr 10, 2025** : SGLang officially supports Mooncake Transfer Engine for disaggregated prefilling and KV cache transfer.
  * **Mar 7, 2025** : We open-sourced the Mooncake Store, a distributed KVCache based on Transfer Engine. vLLM's xPyD disaggregated prefilling & decoding based on Mooncake Store will be released soon.
  * **Feb 25, 2025** : Mooncake receives the **Best Paper Award** at **FAST 2025**!
  * **Feb 21, 2025** : The updated [traces](https://github.com/kvcache-ai/Mooncake/blob/main/FAST25-release/traces) used in our FAST'25 paper have been released.
  * **Dec 16, 2024** : vLLM officially supports Mooncake Transfer Engine for disaggregated prefilling and KV cache transfer.
  * **Nov 28, 2024** : We open-sourced the Transfer Engine, the central component of Mooncake. We also provide two demonstrations of Transfer Engine: a P2P Store and vLLM integration.
  * **July 9, 2024** : We open-sourced the trace as a [JSONL file](https://github.com/kvcache-ai/Mooncake/blob/main/FAST25-release/arxiv-trace/mooncake_trace.jsonl).
  * **June 27, 2024** : We present a series of Chinese blogs with more discussions on [zhihu 1](https://zhuanlan.zhihu.com/p/705754254), [2](https://zhuanlan.zhihu.com/p/705910725), [3](https://zhuanlan.zhihu.com/p/706204757), [4](https://zhuanlan.zhihu.com/p/707997501), [5](https://zhuanlan.zhihu.com/p/9461861451), [6](https://zhuanlan.zhihu.com/p/1939988652114580803), [7](https://zhuanlan.zhihu.com/p/1959366095443064318).
  * **June 26, 2024** : Initial technical report release.


## 🎉 Overview
Mooncake is an infrastructure project for large-scale LLM inference and training. It features a KV cache-centric disaggregated architecture that separates prefill and decode clusters, while leveraging otherwise underutilized CPU, DRAM, and SSD resources in GPU clusters to build a disaggregated KV cache pool.
Mooncake includes a high-performance Transfer Engine for low-latency data movement across heterogeneous networks and accelerators; Mooncake Store for distributed KV cache and model-weight management; and Mooncake EP & PG for elastic MoE serving. Deeply integrated with ecosystems such as SGLang and vLLM, Mooncake helps LLM systems improve cache reuse, reduce serving latency, and scale efficiently across multi-node clusters.
## 🔥 Show Cases
### Transfer Engine (TE)
The core of Mooncake is the Transfer Engine (TE), a high-performance data transfer framework. TE offers a unified interface for batched data movement across diverse storage, network, and accelerator environments. By supporting multiple transport protocols, topology-aware routing, multi-NIC bandwidth aggregation, and automatic failover, TE delivers low-latency, scalable, and robust data transmission for distributed AI workloads. See the [Transfer Engine guide](https://kvcache-ai.github.io/Mooncake/design/transfer-engine/index.html) for details.
Highlights
  * **Efficient use of multiple RDMA NIC devices.** Transfer Engine supports the use of multiple RDMA NIC devices to achieve the _aggregation of transfer bandwidth_.
  * **Topology-aware path selection.** Transfer Engine can _select optimal devices_ based on the location (NUMA affinity, etc.) of both source and destination.
  * **Robust against temporary network errors.** Once transmission fails, Transfer Engine will try to use alternative paths for data delivery automatically.
  * **Superior performance at scale.** With 40 GB of data (equivalent to the size of the KVCache generated by 128k tokens in the LLaMA3-70B model), Mooncake Transfer Engine delivers up to **87 GB/s** and **190 GB/s** of bandwidth in 4×200 Gbps and 8×400 Gbps RoCE networks respectively, which are about **2.4x and 4.6x faster** than the TCP protocol.


  * **Broad support for heterogeneous transports and accelerators.** Transfer Engine provides unified data transfer across diverse protocols, including TCP, RDMA, AWS EFA, NVMe-oF, NVLink, HIP, Barex, CXL, and Ascend-family transports. When built with the corresponding runtime, Transfer Engine can detect accelerator memory and select suitable transport paths for efficient data movement across CUDA, MUSA, HIP, MACA, Cambricon MLU, and Ascend-enabled environments. For a complete list of supported protocols and configuration guide, see the [Supported Protocols Documentation](https://kvcache-ai.github.io/Mooncake/getting_started/supported-protocols.html).
  * **Widely adopted across the LLM ecosystem.** TE is used in production inference stacks such as [SGLang](https://github.com/sgl-project/sglang), [vLLM](https://github.com/vllm-project/vllm), [TensorRT-LLM](https://github.com/NVIDIA/TensorRT-LLM), [vLLM-Ascend](https://github.com/vllm-project/vllm-ascend), [checkpoint-engine](https://github.com/MoonshotAI/checkpoint-engine), and [NIXL](https://github.com/ai-dynamo/nixl), among others, to efficiently transfer KV cache, embeddings, model weights, and other data.


### Mooncake Store
Mooncake Store is a high-performance distributed key-value cache storage engine designed for LLM inference. Built on the Transfer Engine, it stores and manages reusable KV caches and model weights across inference clusters, with support for efficient object storage, replication, eviction, and high-bandwidth data transfer. See the [Mooncake Store guide](https://kvcache-ai.github.io/Mooncake/design/mooncake-store.html) for details.
Highlights
  * **High bandwidth utilization.** Mooncake Store supports large-object striping, parallel I/O, and end-to-end zero-copy data transfer, fully utilizing aggregated bandwidth across multiple NICs.
  * **Multi-tier cache hierarchy**. Mooncake Store supports a multi-level cache design across DRAM and SSD/NVMe, enabling larger cache capacity.
  * **Elastic and disaggregated storage.** Mooncake Store decouples KVCache storage from inference engines, allowing storage nodes to be dynamically added or removed while keeping cached data independent from engine restarts, upgrades, and scheduling decisions.
  * **Programmatic object management.** Mooncake Store allows applications to control object placement and lifecycle through per-object policies, including replica counts, preferred segments, soft pin, and hard pin. These controls help inference systems protect important KV caches and model weights while guiding replication, placement, and eviction behavior.
  * **Broad ecosystem adoption.** Mooncake Store is used across the LLM systems ecosystem as a high-performance distributed storage backend for KV caches, hidden states, and model weights. It supports integrations with [SGLang's Hierarchical KV Caching](https://lmsys.org/blog/2025-09-10-sglang-hicache/), [vLLM's prefill serving](https://docs.vllm.ai/en/latest/features/disagg_prefill.html), and [LMCache](https://kvcache-ai.github.io/Mooncake/deployment/integrations/lmcache/index.html), and has been adopted by systems such as [TorchSpec](https://pytorch.org/blog/torchspec-speculative-decoding-training-at-scale/) and [TransferQueue](https://github.com/Ascend/TransferQueue) to decouple inference, training, and reinforcement-learning workloads through efficient state management and asynchronous data movement.


### Mooncake EP and Process Group (PG)
Mooncake EP and Mooncake PG extend Mooncake from high-performance data movement to fault-tolerant distributed execution for large-scale MoE inference. Mooncake EP adapts DeepEP-style expert-parallel dispatch and combine operations with rank activeness awareness, while Mooncake PG provides a PyTorch distributed process-group backend with collective communication primitives that can detect failed ranks, report failures to upper layers, and recover ranks without restarting the entire inference service. See the [Mooncake EP & Backend guide](https://kvcache-ai.github.io/Mooncake/api-reference/python/ep-backend.html) for details.
Highlights
  * **Fault-tolerant expert parallelism.** Mooncake EP adds `active_ranks` awareness to expert-parallel dispatch and combine APIs, allowing MoE inference systems to route around failed ranks and continue serving with healthy experts.
  * **DeepEP-compatible programming model.** Mooncake EP keeps the API largely consistent with DeepEP's low-latency mode, making it easier for inference engines to adopt fault-tolerant expert parallelism without rewriting their MoE communication stack.
  * **PyTorch ProcessGroup integration.** Mooncake PG can be registered as a `torch.distributed` backend, enabling standard collective APIs such as `all_gather` while using Mooncake's communication and failure-reporting mechanisms underneath.
  * **Elastic rank recovery.** Mooncake PG exposes recovery-oriented primitives such as peer-state polling and rank recovery, allowing replacement processes to rejoin existing process groups and helping inference services recover from partial failures.
  * **SGLang integration for production MoE serving.** Mooncake's collective backend and expert-parallel kernels are integrated into SGLang to support fault-tolerant expert-parallel inference for large MoE models, including Elastic Expert Parallel serving scenarios.


### Tensor-Centric Ecosystem
Mooncake establishes a full-stack, Tensor-oriented AI infrastructure where Tensors serve as the fundamental data carrier. The ecosystem spans from the Transfer Engine, which accelerates Tensor data movement across heterogeneous storage (DRAM/VRAM/NVMe), to Mooncake Store for distributed management of Tensor objects (e.g., KVCache and model weight), up to the Mooncake Backend enabling Tensor-based elastic distributed computing. This architecture is designed to maximize Tensor processing efficiency for large-scale model inference and training.
### SGLang Integration ([Guide](https://kvcache-ai.github.io/Mooncake/deployment/integrations/sglang/index.html))
Mooncake is deeply integrated into [SGLang](https://github.com/sgl-project/sglang/) as a high-performance communication and storage backend. These integrations enable efficient KV cache transfer in PD-disaggregated serving, scalable multi-level KV caching through HiCache, fault-tolerant expert-parallel inference, high-performance multimodal pipeline data movement, and fast RDMA-based weight synchronization for large-scale RL training. Together, Mooncake and SGLang provide a production-oriented foundation for building elastic, high-throughput, and resource-efficient LLM and multimodal serving systems.
Details
  * **PD Disaggregated Serving:** SGLang officially supports Mooncake Transfer Engine as a backend for disaggregated serving and KV cache transfer, enabling prefill and decode workers to exchange KV cache data efficiently across devices and machines.
  * **Hierarchical KV Caching** : Mooncake Store serves as an external storage backend in SGLang's HiCache system, extending RadixAttention with multi-level KV cache storage across device, host, and remote storage layers.
  * **Elastic Expert Parallel** : Mooncake's collective communication backend and expert parallel kernels are integrated into SGLang to enable fault-tolerant expert parallel inference ([Elastic EP](https://www.lmsys.org/blog/2026-03-25-eep-partial-failure-tolerance/)).
  * **Cloud-Native SGLang HiCache Deployment with RBG** : The [RBG](https://github.com/sgl-project/rbg) + SGLang HiCache + Mooncake integration provides a role-based, out-of-the-box cloud-native deployment solution that is elastic, scalable, and optimized for high-performance inference workloads.
  * **Encode-Prefill-Decode Disaggregation for Multimodal Serving** : SGLang introduces Encode-Prefill-Decode disaggregation with Mooncake as a transfer backend. This enables compute-intensive multimodal encoders, such as Vision Transformers, to be decoupled from language model workers while transferring large embeddings efficiently through Mooncake’s RDMA-based engine.
  * **SGLang-Omni Multi-Stage Pipeline Data Transfer** : [SGLang-Omni](https://github.com/sgl-project/sglang-omni) integrates Mooncake as a relay backend for efficient cross-stage tensor and blob transfer in multimodal serving pipelines. This enables high-performance data movement between heterogeneous components such as thinker, talker, codec, and vocoder stages.
  * **RDMA-Based P2P Weight Transfer for Distributed RL** : SGLang adopts Mooncake TransferEngine for RDMA-based peer-to-peer weight transfer in large-scale distributed reinforcement learning. This enables zero-copy weight updates across thousands of GPUs and significantly accelerates synchronization for trillion-parameter models.


### vLLM Integration ([Guide](https://kvcache-ai.github.io/Mooncake/deployment/integrations/vllm/index.html))
Mooncake integrates with [vLLM](https://github.com/vllm-project/vllm) to accelerate large language model serving through high-performance KV cache transfer and distributed KV cache storage. The integration supports both disaggregated prefill-decode serving and cross-instance KV cache sharing, helping vLLM deployments reduce TTFT, improve cache reuse, and scale more efficiently across multi-node inference clusters.
Details
  * **Disaggregated prefill-decode serving** : Mooncake enables vLLM to split prefill and decode workloads across different nodes. Through MooncakeConnector, vLLM transfers KV cache blocks from prefill workers to decode workers using Mooncake’s high-performance transfer engine, allowing prefill and decode resources to scale independently while keeping cross-node KV transfer overhead low.
  * **Distributed KV cache pooling and sharing** : [Mooncake Store extends vLLM](https://vllm.ai/blog/2026-05-06-mooncake-store) from isolated per-instance KV caches to a shared, cluster-level KV cache pool. Through MooncakeStoreConnector, multiple vLLM instances can store, retrieve, and reuse KV cache blocks based on hash-based prefix caching, reducing redundant prefill computation and improving cache efficiency for workloads with repeated prefixes, especially agentic and multi-turn serving scenarios.
  * **vLLM-Omni stage communication** : Mooncake also integrates with [vLLM-Omni](https://github.com/vllm-project/vllm-omni) through `MooncakeTransferEngineConnector` and `MooncakeStoreConnector`, enabling efficient cross-node data exchange between vLLM-Omni stages.


## 🖥️ Supported Hardware
Mooncake supports hardware backends across accelerator vendors, cloud fabrics, and standard datacenter interconnects, as listed below. See the [supported protocols](https://kvcache-ai.github.io/Mooncake/getting_started/supported-protocols.html) and [Transfer Engine design docs](https://kvcache-ai.github.io/Mooncake/design/transfer-engine/index.html) for details.
## 🚀 Getting Started
Install Mooncake using `pip`. The `mooncake-transfer-engine` package includes Mooncake Transfer Engine, Mooncake Store, Mooncake EP and PG:
  * CUDA < 13.0



```
pip install mooncake-transfer-engine
```

  * CUDA >= 13.0



```
pip install mooncake-transfer-engine-cuda13
```

In addition to CUDA, Mooncake also supports other accelerator backends, along with flexible installation and deployment options. See the guides below for details:


### Skills for AI Assistants
Mooncake ships a set of **built-in skills** under [`.claude/skills`](https://github.com/kvcache-ai/Mooncake/blob/main/.claude/skills) — reusable, task-focused playbooks that an AI coding assistant (such as Claude Code) invokes automatically when your request matches, or that you can run as a slash command.
Details  
| Skill  | Description  |  
| --- | --- |  
| `/mooncake-troubleshoot`  | Diagnose Mooncake deployment and runtime issues (services, RDMA, env vars, logs).  |  
| `/mooncake-ci-local`  | Run pre-PR local validation via `scripts/run_ci_test.sh`.  |  
| `/mooncake-api`  | Work with the Mooncake Store, Transfer Engine, and EP/Backend Python APIs.  |  
Install them without cloning the repository via the [Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces):

```
/plugin marketplace add kvcache-ai/Mooncake --sparse .claude-plugin
/plugin install mooncake-troubleshoot@mooncake
/plugin install mooncake-ci-local@mooncake
/plugin install mooncake-api@mooncake

```

The `--sparse .claude-plugin` flag fetches only the marketplace catalog, and each plugin is published as a `git-subdir` source, so installing one fetches only that single skill directory — never the whole repo. If you are already working inside a Mooncake checkout, the skills under `.claude/skills/` load automatically with no setup.
## 📦 Open Source Traces and Tools 
We open-source anonymized request traces containing request arrival times, input and output token counts, and remapped block hashes. These traces are designed to support reproducible simulation and evaluation of caching behavior while preserving user privacy. The released traces and related details are available in [FAST25-release](https://github.com/kvcache-ai/Mooncake/blob/main/FAST25-release).
Together with the released traces, we also provide two KV cache analysis tools: a [KV Cache Size Calculator](https://kvcache.ai/tools/kv-cache-size-calculator/) for calculating cache capacity across popular LLM model families, and a [KV Cache Hit Rate Simulator](https://kvcache.ai/tools/kv-cache-hit-rate-simulator/) for analyzing KV cache hit rates and planning cache capacity under different workloads and models. These tools help users better understand KV cache storage costs and caching effectiveness when analyzing or reproducing serving workloads. The tools are open-sourced [here](https://github.com/kvcache-ai/kvcache-blog).
## 📑 Citation
Please kindly cite our papers if you find the papers or the traces are useful: 

```
@inproceedings{qin2025mooncake,
  author    = {Ruoyu Qin and Zheming Li and Weiran He and Jialei Cui and Feng Ren and Mingxing Zhang and Yongwei Wu and Weimin Zheng and Xinran Xu},
  title     = {Mooncake: Trading More Storage for Less Computation {\textemdash} A {KVCache-centric} Architecture for Serving {LLM} Chatbot},
  booktitle = {23rd USENIX Conference on File and Storage Technologies (FAST 25)},
  year      = {2025},
  isbn      = {978-1-939133-45-8},
  address   = {Santa Clara, CA},
  pages     = {155--170},
  url       = {https://www.usenix.org/conference/fast25/presentation/qin},
  publisher = {USENIX Association},
  month     = ,
}
```

More

```
@misc{ren2026tentdeclarativeslicespraying,
  title     = {TENT: A Declarative Slice Spraying Engine for Performant and Resilient Data Movement in Disaggregated LLM Serving},
  author    = {Feng Ren and Ruoyu Qin and Teng Ma and Shangming Cai and Zheng Liu and Chao Lei and Dejiang Zhu and Ke Yang and Zheming Li and Jialei Cui and Weixiao Huang and Yikai Zhao and Yineng Zhang and Hao Wu and Xiang Gao and Yuhao Fu and Jinlei Jiang and Yongwei Wu and Mingxing Zhang},
  year      = {2026},
  eprint    = {2604.00368},
  archivePrefix = {arXiv},
  primaryClass  = {cs.DC},
  url       = {https://arxiv.org/abs/2604.00368},
}

@article{sun2026survivingpartialrankfailures,
  title     = {Surviving Partial Rank Failures in Wide Expert-Parallel MoE Inference},
  author    = {Xun Sun and Shaoyuan Chen and Pingchuan Ma and Yue Chen and Ziwei Yuan and Zhanhao Cao and Han Han and Shangming Cai and Teng Ma and Xuchun Shang and Xinpeng Zhao and Ke Yang and Junlin Wei and Lianzhi Lin and Yuji Liu and Feng Ren and Haoran Hu and Cheng Wan and Yingdi Shan and Yongwei Wu and Mingxing Zhang},
  year      = {2026},
  url       = {https://arxiv.org/abs/2605.10670},
}

@article{qin2025mooncake_tos,
  author    = {Qin Ruoyu and Li Zheming and He Weiran and Cui Jialei and Tang Heyi and Ren Feng and Ma Teng and Cai Shangming and Zhang Yineng and Zhang Mingxing and Wu Yongwei and Zheng Weimin and Xu Xinran},
  title     = {Mooncake: A KVCache-centric Disaggregated Architecture for LLM Serving},
  year      = {2025},
  publisher = {Association for Computing Machinery},
  address   = {New York, NY, USA},
  issn      = {1553-3077},
  url       = {https://doi.org/10.1145/3773772},
  doi       = {10.1145/3773772},
  journal   = {ACM Trans. Storage},
  month     = ,
  keywords  = {Machine learning system, LLM serving, KVCache},
}

@article{qin2024mooncake_arxiv,
  title  = {Mooncake: A KVCache-centric Disaggregated Architecture for LLM Serving},
  author = {Ruoyu Qin and Zheming Li and Weiran He and Mingxing Zhang and Yongwei Wu and Weimin Zheng and Xinran Xu},
  year   = {2024},
  url    = {https://arxiv.org/abs/2407.00079},
}
```

## About
Mooncake is the serving platform for Kimi, a leading LLM service provided by Moonshot AI.
[kvcache-ai.github.io/Mooncake/](https://kvcache-ai.github.io/Mooncake/)
### Topics
[disaggregation](https://github.com/topics/disaggregation)[inference](https://github.com/topics/inference)[kvcache](https://github.com/topics/kvcache)[llm](https://github.com/topics/llm)[rdma](https://github.com/topics/rdma)[reinforcement-learning](https://github.com/topics/reinforcement-learning)[sglang](https://github.com/topics/sglang)[tokenspeed](https://github.com/topics/tokenspeed)[trt-llm](https://github.com/topics/trt-llm)[vllm](https://github.com/topics/vllm)
### Resources
### Code of conduct
### Contributing
### Stars
**6.6k** stars
### Watchers
**48** watching
### Forks
[**1.2k** forks](https://github.com/kvcache-ai/Mooncake/forks)
## 
[ v0.3.13.post1Latest Aug 31, 2026 ](https://github.com/kvcache-ai/Mooncake/releases/tag/v0.3.13.post1)
## 
## Languages
  * [C++79.1%](https://github.com/kvcache-ai/Mooncake/search?l=c%2B%2B)
  * [Python13.5%](https://github.com/kvcache-ai/Mooncake/search?l=python)
  * [Cuda2.2%](https://github.com/kvcache-ai/Mooncake/search?l=cuda)
  * [Shell1.6%](https://github.com/kvcache-ai/Mooncake/search?l=shell)
  * [CMake1.3%](https://github.com/kvcache-ai/Mooncake/search?l=cmake)
  * [C0.8%](https://github.com/kvcache-ai/Mooncake/search?l=c)
  * [Other1.5%](https://github.com/kvcache-ai/Mooncake/search?l=Other)


You can’t perform that action at this time. 
