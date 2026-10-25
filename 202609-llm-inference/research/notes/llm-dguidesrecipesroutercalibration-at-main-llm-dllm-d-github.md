---
title: llm-d/guides/recipes/router/calibration at main · llm-d/llm-d · GitHub
id: llm-dguidesrecipesroutercalibration-at-main-llm-dllm-d-github
tags:
- llmd-kv-cache-routing-c665d5
- calibration
- peakprefillthroughput
- github-primary-source
- locus-default-scoring-configuration
created: '2026-09-14T11:57:35.066386Z'
updated: '2026-09-15T04:55:24.094722Z'
source: https://github.com/llm-d/llm-d/tree/main/guides/recipes/router/calibration
source_domain: github.com
fetched_at: '2026-09-14T11:57:35.046622Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Primary llm-d GitHub source (guides/recipes/router/calibration README) documenting
  the exact procedure and tooling for calibrating the two hardware-dependent routing
  constants used by llm-d''s router plugins. (1) peakPrefillThroughput, used by the
  prefix-cache-affinity-filter to estimate per-endpoint TTFT from in-flight load:
  measured by calibrate.sh, which runs a Kubernetes Job sending warmup + measurement
  requests of exactly CHUNK_SIZE random token IDs (guaranteed cache misses) and computes
  peakPrefillThroughput = CHUNK_SIZE / median(TTFT) in tokens/sec; CHUNK_SIZE must
  match vLLM''s --max-num-batched-tokens. The plugin default (15928) is calibrated
  for Qwen 32B on H100 80GB (TP=2); the agentic-serving guide ships 16444 for Qwen3-Coder-480B-FP8
  on TPU v7x. A shared configuration-matrix.md tracks reference values by (model,
  accelerator). (2) minCachedTokenDelta, used by the p2p-source-producer to decide
  when a peer''s cached prefix lead is large enough to justify a pull over recompute:
  measured by calibrate-min-cached-token-delta.sh, which seeds a fresh random-token
  prompt on a source pod and times the consumer pod serving it with and without kv_transfer_params.remote_kv_source
  (mesh pre-warmed to exclude one-time session-establishment cost), printing the smallest
  tested length where the pull won. Both calibration values are explicitly stated
  to be model-, hardware- and transport-specific and must be re-measured, not transplanted,
  when any of those change.'
---

*Suggested by [[sticky-until-saturated-token-aware-routing-in-llm-d-llm-d]] — calibration recipe cited as source of shipped peakPrefillThroughput defaults and calibrate.sh methodology*

[Skip to content](https://github.com/llm-d/llm-d/tree/main/guides/recipes/router/calibration#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d/tree/main/guides/recipes/router/calibration) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d/tree/main/guides/recipes/router/calibration) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d/tree/main/guides/recipes/router/calibration) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/tree/main/guides/recipes/router/calibration).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d) You must be signed in to change notification settings
  * [ Fork 765 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)
  * [ Star  4.5k ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# calibration
/
Copy path
## Directory actions
## More options
More options
## Directory actions
## More options
More options
## Latest commit
[Hardware support: Rebellions NPU (optimized-baseline) (](https://github.com/llm-d/llm-d/commit/965a5080b354187c9f466fbabe4777954348b3f7)[#2476](https://github.com/llm-d/llm-d/pull/2476)[)](https://github.com/llm-d/llm-d/commit/965a5080b354187c9f466fbabe4777954348b3f7)
Open commit detailssuccess
Sep 12, 2026
[965a508](https://github.com/llm-d/llm-d/commit/965a5080b354187c9f466fbabe4777954348b3f7) · Sep 12, 2026
## History
[History](https://github.com/llm-d/llm-d/commits/main/guides/recipes/router/calibration)
Open commit details
History
/
# calibration
/
Copy path
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| 
### parent directory
 |  
|   |   |  [Harden P2P cache-sharing guide and update GLM results (](https://github.com/llm-d/llm-d/commit/07a0271635459b8dbd68c7bdbbf82090b6437230 "Harden P2P cache-sharing guide and update GLM results \(#2296\)

* Mark P2P cache-sharing guide experimental

Align source-selection and P/D descriptions with shipped behavior and limit performance guidance to published measurements. Link the guide after the tiered prefix cache documentation.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* Replace GLM fork results with C64 study

Use the repeated fixed-window policy comparison and corrected four-arm observation so the guide presents reproducible GLM evidence with exact configurations and data provenance.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* Fix GLM C64 guide lint

Use repository-compliant Markdown table and YAML sequence formatting so the changed benchmark files pass their hooks. Preserve the measured vLLM endpoint semantics and restore the decode-capacity validity gate.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* Name the P2P guide vLLM Service

Identify the prefixed podless Service and its vLLM target port so benchmark-arm adaptation does not point token production at a nonexistent Service or the routing proxy.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* Present GLM C64 artifacts as standalone state

Signed-off-by: nilig <nili.ifergan@gmail.com>

---------

Signed-off-by: nilig <nili.ifergan@gmail.com>
Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>
Co-authored-by: Maroon Ayoub <maroon.ayoub@ibm.com>")[#2296](https://github.com/llm-d/llm-d/pull/2296)[)](https://github.com/llm-d/llm-d/commit/07a0271635459b8dbd68c7bdbbf82090b6437230 "Harden P2P cache-sharing guide and update GLM results \(#2296\)

* Mark P2P cache-sharing guide experimental

Align source-selection and P/D descriptions with shipped behavior and limit performance guidance to published measurements. Link the guide after the tiered prefix cache documentation.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* Replace GLM fork results with C64 study

Use the repeated fixed-window policy comparison and corrected four-arm observation so the guide presents reproducible GLM evidence with exact configurations and data provenance.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* Fix GLM C64 guide lint

Use repository-compliant Markdown table and YAML sequence formatting so the changed benchmark files pass their hooks. Preserve the measured vLLM endpoint semantics and restore the decode-capacity validity gate.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* Name the P2P guide vLLM Service

Identify the prefixed podless Service and its vLLM target port so benchmark-arm adaptation does not point token production at a nonexistent Service or the routing proxy.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* Present GLM C64 artifacts as standalone state

Signed-off-by: nilig <nili.ifergan@gmail.com>

---------

Signed-off-by: nilig <nili.ifergan@gmail.com>
Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>
Co-authored-by: Maroon Ayoub <maroon.ayoub@ibm.com>")  | Aug 14, 2026  |  
| [calibrate-min-cached-token-delta.sh](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/calibrate-min-cached-token-delta.sh "calibrate-min-cached-token-delta.sh")  | [calibrate-min-cached-token-delta.sh](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/calibrate-min-cached-token-delta.sh "calibrate-min-cached-token-delta.sh")  |  [Harden P2P cache-sharing guide and update GLM results (](https://github.com/llm-d/llm-d/commit/07a0271635459b8dbd68c7bdbbf82090b6437230 "Harden P2P cache-sharing guide and update GLM results \(#2296\)

* Mark P2P cache-sharing guide experimental

Align source-selection and P/D descriptions with shipped behavior and limit performance guidance to published measurements. Link the guide after the tiered prefix cache documentation.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* Replace GLM fork results with C64 study

Use the repeated fixed-window policy comparison and corrected four-arm observation so the guide presents reproducible GLM evidence with exact configurations and data provenance.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* Fix GLM C64 guide lint

Use repository-compliant Markdown table and YAML sequence formatting so the changed benchmark files pass their hooks. Preserve the measured vLLM endpoint semantics and restore the decode-capacity validity gate.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* Name the P2P guide vLLM Service

Identify the prefixed podless Service and its vLLM target port so benchmark-arm adaptation does not point token production at a nonexistent Service or the routing proxy.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* Present GLM C64 artifacts as standalone state

Signed-off-by: nilig <nili.ifergan@gmail.com>

---------

Signed-off-by: nilig <nili.ifergan@gmail.com>
Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>
Co-authored-by: Maroon Ayoub <maroon.ayoub@ibm.com>")[#2296](https://github.com/llm-d/llm-d/pull/2296)[)](https://github.com/llm-d/llm-d/commit/07a0271635459b8dbd68c7bdbbf82090b6437230 "Harden P2P cache-sharing guide and update GLM results \(#2296\)

* Mark P2P cache-sharing guide experimental

Align source-selection and P/D descriptions with shipped behavior and limit performance guidance to published measurements. Link the guide after the tiered prefix cache documentation.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* Replace GLM fork results with C64 study

Use the repeated fixed-window policy comparison and corrected four-arm observation so the guide presents reproducible GLM evidence with exact configurations and data provenance.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* Fix GLM C64 guide lint

Use repository-compliant Markdown table and YAML sequence formatting so the changed benchmark files pass their hooks. Preserve the measured vLLM endpoint semantics and restore the decode-capacity validity gate.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* Name the P2P guide vLLM Service

Identify the prefixed podless Service and its vLLM target port so benchmark-arm adaptation does not point token production at a nonexistent Service or the routing proxy.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* Present GLM C64 artifacts as standalone state

Signed-off-by: nilig <nili.ifergan@gmail.com>

---------

Signed-off-by: nilig <nili.ifergan@gmail.com>
Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>
Co-authored-by: Maroon Ayoub <maroon.ayoub@ibm.com>")  | Aug 14, 2026  |  
|   |   | [agentic-serving: token-load router + offload-aware prefix cache + cal…](https://github.com/llm-d/llm-d/commit/c32b4a06e2c9e43cd640daa0290dbefbaa36f4be "agentic-serving: token-load router + offload-aware prefix cache + calibration \(#1817\)

- Router values: switch to token-load-scorer + prefix-cache-affinity, with the
  approx-prefix-cache producer LRU sized for the KV-offload tier
  \(autoTune off, lruCapacityPerServer manual\). maxTTFTPenaltyMs left at its
  18000 default.
- Use the llm-d-router-standalone-dev chart \(v0\); add the GAIE CRD install step.
- Template the benchmark workload on CONCURRENCY_LEVEL/NUM_REQUESTS/SEED so each
  concurrency uses an independent prompt set; bump benchmark image to v0.6.8.1.
- Add calibration/ tooling \(calibrate.sh + tau Job + values template + README\)
  to measure peakPrefillThroughput; note it in the guide.
- Refresh benchmark-results charts \(llm-d-optimized through conc=80\) and summary.

Signed-off-by: Kaushik Mitra <kaushikmitra@google.com>")  | Jun 16, 2026  |  
| [calibration-min-cached-token-delta.yaml](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/calibration-min-cached-token-delta.yaml "calibration-min-cached-token-delta.yaml")  | [calibration-min-cached-token-delta.yaml](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/calibration-min-cached-token-delta.yaml "calibration-min-cached-token-delta.yaml")  |  [Well-lit path and guide: P2P KV cache sharing (](https://github.com/llm-d/llm-d/commit/0db219d1743e5cec69bea7bf2488d1a96ce68336 "Well-lit path and guide: P2P KV cache sharing \(#2067\)")[#2067](https://github.com/llm-d/llm-d/pull/2067)[)](https://github.com/llm-d/llm-d/commit/0db219d1743e5cec69bea7bf2488d1a96ce68336 "Well-lit path and guide: P2P KV cache sharing \(#2067\)")  | Aug 11, 2026  |  
| [calibration-peak-throughput.yaml](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/calibration-peak-throughput.yaml "calibration-peak-throughput.yaml")  | [calibration-peak-throughput.yaml](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/calibration-peak-throughput.yaml "calibration-peak-throughput.yaml")  |   | Aug 14, 2026  |  
|   |   |  [Hardware support: Rebellions NPU (optimized-baseline) (](https://github.com/llm-d/llm-d/commit/965a5080b354187c9f466fbabe4777954348b3f7 "Hardware support: Rebellions NPU \(optimized-baseline\) \(#2476\)

* feat\(recipes\): add the npu-vllm image component

Selects the Rebellions model server image for the overlays that follow, and
carries the USER env patch every other vLLM image component needs since
vLLM v0.20.0 on torch 2.11.

Rebellions publishes the image while the RBLN SDK is pre-GA: rebel-compiler,
lmcache-rbln and nixl-rbln are still internal and every RBLN package in it is
a development build. This moves to a docker/Dockerfile.rbln built by llm-d,
the way Dockerfile.xpu is, once those are published.

Signed-off-by: rebel-minhopark <minho.park@rebellions.ai>

* feat\(optimized-baseline\): add a Rebellions NPU overlay

Serves gpt-oss-120b on Rebellions NPUs, two replicas of one NPU each, on top
of recipes/modelserver/base/single-host.

NPUs are claimed through DRA rather than an extended resource. The operator
advertises a single npu.rebellions.ai DeviceClass for every Rebellions
product, so the claim template narrows to the REBEL family with a CEL
selector on the device's productName attribute. That needs resource.k8s.io/v1,
so Kubernetes 1.34 or later.

The overlay deviates from the base in three places that a first-time reader
would otherwise have to discover the hard way. The compile runs inside the
pod on first start, which is why the startup budget is 7215s and
progressDeadlineSeconds matches it instead of tripping at the 600s default.
Rolling a replica would leave its old and new pod both holding a claim with
no spare accelerator, hence Recreate. And num-gpu-blocks-override carries a
measured count rather than a profiled one, since the KV cache lives in NPU
memory the profiling pass cannot size; that also makes
gpu-memory-utilization redundant, so the overlay does not set it.

router/npu.values.yaml comes with it. The runtime disables prefix caching for
sliding-window models and gpt-oss-120b is one, so the guide's default
scheduling profile would pin requests to endpoints it believes are cache-warm
while the model servers reuse nothing. The NPU profile drops the
prefix-cache-affinity-filter and keeps the token-load-scorer. Both that file
and --no-enable-prefix-caching go away once the runtime supports prefix
caching here.

Signed-off-by: rebel-minhopark <minho.park@rebellions.ai>

* docs: document the Rebellions NPU backend

Adds the accelerator section and support-table row, and the npu/vllm row in
the shared calibration matrix.

vllm-rbln is an out-of-tree vLLM platform plugin on the vllm.platform_plugins
entry point, not a fork, so the accelerator note says so.

The calibration recipe defines peak prefill throughput as CHUNK_SIZE over the
median TTFT with CHUNK_SIZE matching --max-num-batched-tokens, which is 512
here. Five runs of 20 requests against this overlay spread from 12504 to
13101, so the row carries their median, 12884. The overlay does not set the
value on the filter; the guide's "Adapting to other hardware" section directs
readers here instead.

The accelerator note also tells readers to layer router/npu.values.yaml and
to export MODEL, since the overlay pins a model the guide does not default to.

Signed-off-by: rebel-minhopark <minho.park@rebellions.ai>

* fix\(optimized-baseline\): give the NPU pod its share of the host cores

cpu was 10, which is neither the host share nor a measured figure. A node
carrying eight NPUs across 128 cores gives 16 per NPU, and this pod claims
one, so 16 is the share that goes with it. It stays even because a node
running the static CPU manager with SMT rejects odd counts.

Measured on one node, two replicas, against the previous 10: each replica
serves 64/64 requests at 1k in / 1k out and concurrency 8 at 201.8 and 201.7
output tok/s with TPOT p99 4.95 and 4.94 ms, where 10 left one replica at
197.6 tok/s and 5.57 ms. Ready in 458s, 50.7 GiB anonymous, no restarts.

Peak prefill throughput was re-measured at the new share: five runs of 20
requests spread from 12467 to 13467, so the calibration row moves from 12884
to their median, 12582.

Signed-off-by: rebel-minhopark <minho.park@rebellions.ai>

* fix\(optimized-baseline\): drop defaults and name the router values for the runtime

Review feedback from @Gregory-Pereira on #2476.

--served-model-name, --max-model-len and --enable-chunked-prefill all restated
a default. Verified on hardware with the three flags gone: the server resolves
served_model_name=openai/gpt-oss-120b, max_seq_len=131072 and
enable_chunked_prefill=True, and /v1/models returns the same id and length.
gpt-oss-120b carries max_position_embeddings 131072 in its own config.

RBLN_LOCAL_IP and RBLN_ROOT_IP restated a default too. The runtime sets both
to 127.0.0.1 when unset, and at one NPU per replica world_size is 1 so nothing
reads them. Spelling them out invited the reading that they were the serving
host, which they are not. The pd-disaggregation overlay keeps them, where four
devices share a pod and the rendezvous is real.

router/npu.values.yaml becomes npu.rbln.values.yaml. The prefix-caching gap it
works around belongs to the RBLN runtime, not to NPUs in general.

Signed-off-by: rebel-minhopark <minho.park@rebellions.ai>

---------

Signed-off-by: rebel-minhopark <minho.park@rebellions.ai>")[#2476](https://github.com/llm-d/llm-d/pull/2476)[)](https://github.com/llm-d/llm-d/commit/965a5080b354187c9f466fbabe4777954348b3f7 "Hardware support: Rebellions NPU \(optimized-baseline\) \(#2476\)

* feat\(recipes\): add the npu-vllm image component

Selects the Rebellions model server image for the overlays that follow, and
carries the USER env patch every other vLLM image component needs since
vLLM v0.20.0 on torch 2.11.

Rebellions publishes the image while the RBLN SDK is pre-GA: rebel-compiler,
lmcache-rbln and nixl-rbln are still internal and every RBLN package in it is
a development build. This moves to a docker/Dockerfile.rbln built by llm-d,
the way Dockerfile.xpu is, once those are published.

Signed-off-by: rebel-minhopark <minho.park@rebellions.ai>

* feat\(optimized-baseline\): add a Rebellions NPU overlay

Serves gpt-oss-120b on Rebellions NPUs, two replicas of one NPU each, on top
of recipes/modelserver/base/single-host.

NPUs are claimed through DRA rather than an extended resource. The operator
advertises a single npu.rebellions.ai DeviceClass for every Rebellions
product, so the claim template narrows to the REBEL family with a CEL
selector on the device's productName attribute. That needs resource.k8s.io/v1,
so Kubernetes 1.34 or later.

The overlay deviates from the base in three places that a first-time reader
would otherwise have to discover the hard way. The compile runs inside the
pod on first start, which is why the startup budget is 7215s and
progressDeadlineSeconds matches it instead of tripping at the 600s default.
Rolling a replica would leave its old and new pod both holding a claim with
no spare accelerator, hence Recreate. And num-gpu-blocks-override carries a
measured count rather than a profiled one, since the KV cache lives in NPU
memory the profiling pass cannot size; that also makes
gpu-memory-utilization redundant, so the overlay does not set it.

router/npu.values.yaml comes with it. The runtime disables prefix caching for
sliding-window models and gpt-oss-120b is one, so the guide's default
scheduling profile would pin requests to endpoints it believes are cache-warm
while the model servers reuse nothing. The NPU profile drops the
prefix-cache-affinity-filter and keeps the token-load-scorer. Both that file
and --no-enable-prefix-caching go away once the runtime supports prefix
caching here.

Signed-off-by: rebel-minhopark <minho.park@rebellions.ai>

* docs: document the Rebellions NPU backend

Adds the accelerator section and support-table row, and the npu/vllm row in
the shared calibration matrix.

vllm-rbln is an out-of-tree vLLM platform plugin on the vllm.platform_plugins
entry point, not a fork, so the accelerator note says so.

The calibration recipe defines peak prefill throughput as CHUNK_SIZE over the
median TTFT with CHUNK_SIZE matching --max-num-batched-tokens, which is 512
here. Five runs of 20 requests against this overlay spread from 12504 to
13101, so the row carries their median, 12884. The overlay does not set the
value on the filter; the guide's "Adapting to other hardware" section directs
readers here instead.

The accelerator note also tells readers to layer router/npu.values.yaml and
to export MODEL, since the overlay pins a model the guide does not default to.

Signed-off-by: rebel-minhopark <minho.park@rebellions.ai>

* fix\(optimized-baseline\): give the NPU pod its share of the host cores

cpu was 10, which is neither the host share nor a measured figure. A node
carrying eight NPUs across 128 cores gives 16 per NPU, and this pod claims
one, so 16 is the share that goes with it. It stays even because a node
running the static CPU manager with SMT rejects odd counts.

Measured on one node, two replicas, against the previous 10: each replica
serves 64/64 requests at 1k in / 1k out and concurrency 8 at 201.8 and 201.7
output tok/s with TPOT p99 4.95 and 4.94 ms, where 10 left one replica at
197.6 tok/s and 5.57 ms. Ready in 458s, 50.7 GiB anonymous, no restarts.

Peak prefill throughput was re-measured at the new share: five runs of 20
requests spread from 12467 to 13467, so the calibration row moves from 12884
to their median, 12582.

Signed-off-by: rebel-minhopark <minho.park@rebellions.ai>

* fix\(optimized-baseline\): drop defaults and name the router values for the runtime

Review feedback from @Gregory-Pereira on #2476.

--served-model-name, --max-model-len and --enable-chunked-prefill all restated
a default. Verified on hardware with the three flags gone: the server resolves
served_model_name=openai/gpt-oss-120b, max_seq_len=131072 and
enable_chunked_prefill=True, and /v1/models returns the same id and length.
gpt-oss-120b carries max_position_embeddings 131072 in its own config.

RBLN_LOCAL_IP and RBLN_ROOT_IP restated a default too. The runtime sets both
to 127.0.0.1 when unset, and at one NPU per replica world_size is 1 so nothing
reads them. Spelling them out invited the reading that they were the serving
host, which they are not. The pd-disaggregation overlay keeps them, where four
devices share a pod and the rendezvous is real.

router/npu.values.yaml becomes npu.rbln.values.yaml. The prefix-caching gap it
works around belongs to the RBLN runtime, not to NPUs in general.

Signed-off-by: rebel-minhopark <minho.park@rebellions.ai>

---------

Signed-off-by: rebel-minhopark <minho.park@rebellions.ai>")  | Sep 12, 2026  |  
| View all files |  
## [README.md](https://github.com/llm-d/llm-d/tree/main/guides/recipes/router/calibration#readme)
Outline
# Calibrating `peakPrefillThroughput`
Router configs that use the `prefix-cache-affinity-filter` plugin set `peakPrefillThroughput` on it. The filter uses this value to estimate per-endpoint time-to-first-token from in-flight load, which drives prefix-cache-aware routing.
The value is **hardware- and model-specific** — the plugin default (`15928`) is calibrated for Qwen 32B on H100 80 GB (TP=2). If you deploy a different model or accelerator, measure your own with this tool and set it on the `prefix-cache-affinity-filter` plugin in your guide's router values file. (The agentic-serving guide ships `16444`, measured for Qwen3-Coder-480B-FP8 on TPU v7x.)
See the [**configuration matrix**](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/configuration-matrix.md) for the reference values by (model, accelerator) shipped under `guides/`, and which combinations still need a calibration run.
## What it measures
`calibrate.sh` runs a short Kubernetes Job ([`calibration-peak-throughput.yaml`](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/calibration-peak-throughput.yaml)) that sends warmup + measurement requests of exactly `CHUNK_SIZE` random token IDs (so the prefix cache misses every time and we measure true prefill), records TTFT, and computes:

```
peakPrefillThroughput = CHUNK_SIZE / median(TTFT)   # tokens/sec

```

It **only measures and prints** the value — it does not modify any config.
## Prerequisites
  * The stack is deployed and serving (router + model server), reachable from the Job's network.
  * `kubectl` and `envsubst` on your `PATH`.


## Usage

```
GUIDE_NAME=agentic-serving \
NAMESPACE=llm-d-agentic-serving \
MODEL_NAME=Qwen/Qwen3-Coder-480B-A35B-Instruct-FP8 \
CHUNK_SIZE=8192 \
./calibrate.sh
```
  
| env var  | meaning  | default  |  
| --- | --- | --- |  
| `GUIDE_NAME`  | release/guide name (used for the `<name>-epp` service)  | `optimized-baseline`  |  
| `NAMESPACE`  | namespace the stack runs in  | `default`  |  
| `MODEL_NAME`  | model vLLM is serving  | `Qwen/Qwen3-32B`  |  
| `CHUNK_SIZE`  | request size; **must match vLLM`--max-num-batched-tokens`**  | `8192`  |  
| `T_MAX_SECONDS`  | TTFT SLO tolerance (informational `TAU` line only)  |  
| `VLLM_ENDPOINT`  |  `http://host:port`; auto-discovered from the EPP service if unset  | —  |  
|  `NUM_WARMUP` / `NUM_MEASUREMENTS`  | request counts  |  `5` / `20`  |  
## Applying the value
Set the measured number on the `prefix-cache-affinity-filter` plugin in your guide's router values file:

```
- type: prefix-cache-affinity-filter
  parameters:
    peakPrefillThroughput: <measured value>
```

Then re-apply the router release (`helm upgrade ... -f <your-guide>.values.yaml`) and restart the EPP:

```
kubectl rollout restart -n ${NAMESPACE} deployment/${GUIDE_NAME}-epp
```

## Files  
| file  | purpose  |  
| --- | --- |  
| `calibrate.sh`  | orchestration: runs the Job, extracts and prints the value  |  
| `calibration-peak-throughput.yaml`  | the measurement Job + its Python script (ConfigMap)  |  
## Calibrating `minCachedTokenDelta`
Router configs that use the `p2p-source-producer` (the [p2p-kv-cache-sharing guide](https://github.com/llm-d/llm-d/blob/main/guides/p2p-kv-cache-sharing/README.md)) set `minCachedTokenDelta` on it: a pull is requested only when a peer holds at least that many more cached prefix tokens than the scheduled pod. Below the pull-versus-recompute crossover a pull costs more than recomputing, so the right value is the crossover — and the crossover is **model-, hardware- and transport-specific** (measured on gpt-oss-120b/H200: below 2K with `rdma/ib` on the pods; calibrate separately on the TCP fallback).
[`calibrate-min-cached-token-delta.sh`](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/calibrate-min-cached-token-delta.sh) runs a Job ([`calibration-min-cached-token-delta.yaml`](https://github.com/llm-d/llm-d/blob/main/guides/recipes/router/calibration/calibration-min-cached-token-delta.yaml)) that measures it against two live model-server pods: per length and repetition it seeds a fresh random token-ID prompt on the source pod, then times the consumer pod serving that prompt with and without `kv_transfer_params.remote_kv_source` (independent prompts for each leg, because the consumer caches whatever it just served). The mesh is warmed first so the one-time session-establishment cost is excluded. It prints the measured ladder and the recommendation:

```
MIN_CACHED_TOKEN_DELTA=<smallest tested length where the pull won>

```


```
NAMESPACE=llm-d-p2p \
POD_SELECTOR=llm-d.ai/guide=p2p-kv-cache-sharing \
MODEL_NAME=openai/gpt-oss-120b \
./calibrate-min-cached-token-delta.sh
```

Unlike the peak-throughput Job this cannot go through the router — the pull is driven by injecting `kv_transfer_params` directly at two engine endpoints — so the script talks to pod IPs (`ENGINE_PORT`, default `8200`). Prerequisites: the OffloadingConnector P2P tier on the pods, `PYTHONHASHSEED` pinned fleet-wide, and the same transport you will deploy on. Lengths must be multiples of the vLLM block size.
You can’t perform that action at this time. 
