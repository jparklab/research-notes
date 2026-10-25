---
title: 'GitHub - vllm-project/aibrix: Cost-efficient and pluggable Infrastructure
  components for GenAI inference · GitHub'
id: github-vllm-projectaibrix-cost-efficient-and-pluggable-infrastructure-components
tags:
- internal-llm-inference-stacks-7f0c90
- aibrix
- governance
- repository-signal
created: '2026-10-09T22:53:33.788810Z'
updated: '2026-10-10T14:52:17.331773Z'
source: https://github.com/vllm-project/aibrix
source_domain: github.com
fetched_at: '2026-10-09T22:53:33.762093Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'AIBrix''s live GitHub repository (vllm-project org, Apache 2.0, 5.1k stars/52
  watchers at fetch time, 1,619 commits, Go 55.9%/Python 28%/Jupyter 8%/TypeScript
  5.8%). Confirms governance: hosted under the vllm-project GitHub org (not its own
  foundation, not CNCF) -- distinct from llm-d''s separate CNCF Sandbox status. Release
  history shows active ByteDance-plus-community maintenance from v0.1.0 (2024-11-13)
  through v0.7.0 (2026-06-16), each with a dedicated blog post. Talks/Presentations
  section documents repeated conference validation beyond ByteDance alone: KubeCon
  North America 2025 keynote (''AIBrix: Kubernetes-native GenAI Inference Infrastructure''),
  KubeCon China 2025 (Jiaxin Shan, Liguang Xie, ByteDance), a co-delivered KubeCon
  EU 2025 keynote WITH Google (''LLM-Aware Load Balancing in Kubernetes: A New Era
  of Efficiency'', Clayton Coleman of Google + Jiaxin Shan of ByteDance), and an ASPLOS''25
  workshop presentation framing AIBrix as ''an Open-Source, Large-Scale LLM Inference
  Infrastructure for System Research.'' White paper published at arXiv 2504.03648.
  Key Features list matches the announcement blogs (LoRA mgmt, LLM gateway/routing,
  autoscaler, unified runtime, distributed inference, distributed KV cache, heterogeneous
  serving, GPU failure detection). Quick-start install is three kubectl manifests
  (dependency, CRDs, core components) against an existing cluster -- confirms AIBrix
  layers onto, rather than replaces, a pre-existing Kubernetes installation. Code
  of conduct, contributing guidelines, and security policy are present, indicating
  open-governance process maturity typical of a CNCF-adjacent but not CNCF-hosted
  project.'
utility_score: 14.0
---

[Skip to content](https://github.com/vllm-project/aibrix#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/vllm-project/aibrix) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/vllm-project/aibrix) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/vllm-project/aibrix) to refresh your session. Dismiss alert
[**13** Branches](https://github.com/vllm-project/aibrix/branches)[](https://github.com/vllm-project/aibrix/tags)
Go to file
Open more actions menu
## Latest commit
Open commit detailsfailure
Oct 9, 2026
[433a8e1](https://github.com/vllm-project/aibrix/commit/433a8e1b68527c9ee90b0ca562f9ddb16e1a6a03) · Oct 9, 2026
## History
[1,619 Commits](https://github.com/vllm-project/aibrix/commits/main/)
Open commit details
1,619 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |  [fix: separate GPU optimizer deps to reduce runtime image size (](https://github.com/vllm-project/aibrix/commit/048a70f99d427b489d1971813ca101f2cfddf5dc "fix: separate GPU optimizer deps to reduce runtime image size \(#2863\) \(#2872\)

* fix: separate GPU optimizer deps to reduce runtime image size \(#2863\)

The aibrix/runtime and aibrix/metadata-service nightly images were both
~970 MB because all GPU Optimizer scientific-computing packages \(numpy,
pandas, matplotlib, PuLP, Dash, incdbscan — ~584 MiB\) were declared as
main Poetry dependencies, even though they are only needed by
aibrix.gpu_optimizer \(load monitor, visualizer, LP solver\).

The Dockerfile comment stated these packages were excluded via
'--without profiling', but they were never in the profiling group, so
that flag had no effect on them.

Additionally, both images were built from the same Dockerfile with no
--target specified, so they ended up identical in content but pushed as
separate tags with different layer digests \(no layer sharing\).

Changes:
- python/aibrix/pyproject.toml: move numpy, pandas, PuLP, incdbscan,
  dash, matplotlib out of main deps into a new
  \[tool.poetry.group.gpu_optimizer.dependencies\] group. Fix stale
  comments.
- build/container/Dockerfile.python: add two named build targets:
    'runtime'          -- slim image, no GPU optimizer deps \(~584 MiB saved\)
    'metadata-service' -- extends shared base, installs gpu_optimizer
                          group so GPU Optimizer keeps running there
- .github/workflows/docker-push-images.yml,
  .github/workflows/release-build.yaml,
  .github/workflows/installation-smoke-tests.yml,
  .github/workflows/installation-tests.yml:
  add target: runtime / metadata-service to matrix entries and pass
  target to docker/build-push-action.
* fix: address reviewer comments for dependency split and docker builds
* Use gpu-optimizer extra for metadata-service image

Signed-off-by: Paramveersingh-S <paramveersingh-s@users.noreply.github.com>
Signed-off-by: Jiaxin Shan <seedjeffwan@gmail.com>
Co-authored-by: Paramveersingh-S <paramveersingh-s@users.noreply.github.com>
Co-authored-by: Jiaxin Shan <seedjeffwan@gmail.com>")[#2863](https://github.com/vllm-project/aibrix/issues/2863)[) (](https://github.com/vllm-project/aibrix/commit/048a70f99d427b489d1971813ca101f2cfddf5dc "fix: separate GPU optimizer deps to reduce runtime image size \(#2863\) \(#2872\)

* fix: separate GPU optimizer deps to reduce runtime image size \(#2863\)

The aibrix/runtime and aibrix/metadata-service nightly images were both
~970 MB because all GPU Optimizer scientific-computing packages \(numpy,
pandas, matplotlib, PuLP, Dash, incdbscan — ~584 MiB\) were declared as
main Poetry dependencies, even though they are only needed by
aibrix.gpu_optimizer \(load monitor, visualizer, LP solver\).

The Dockerfile comment stated these packages were excluded via
'--without profiling', but they were never in the profiling group, so
that flag had no effect on them.

Additionally, both images were built from the same Dockerfile with no
--target specified, so they ended up identical in content but pushed as
separate tags with different layer digests \(no layer sharing\).

Changes:
- python/aibrix/pyproject.toml: move numpy, pandas, PuLP, incdbscan,
  dash, matplotlib out of main deps into a new
  \[tool.poetry.group.gpu_optimizer.dependencies\] group. Fix stale
  comments.
- build/container/Dockerfile.python: add two named build targets:
    'runtime'          -- slim image, no GPU optimizer deps \(~584 MiB saved\)
    'metadata-service' -- extends shared base, installs gpu_optimizer
                          group so GPU Optimizer keeps running there
- .github/workflows/docker-push-images.yml,
  .github/workflows/release-build.yaml,
  .github/workflows/installation-smoke-tests.yml,
  .github/workflows/installation-tests.yml:
  add target: runtime / metadata-service to matrix entries and pass
  target to docker/build-push-action.
* fix: address reviewer comments for dependency split and docker builds
* Use gpu-optimizer extra for metadata-service image

Signed-off-by: Paramveersingh-S <paramveersingh-s@users.noreply.github.com>
Signed-off-by: Jiaxin Shan <seedjeffwan@gmail.com>
Co-authored-by: Paramveersingh-S <paramveersingh-s@users.noreply.github.com>
Co-authored-by: Jiaxin Shan <seedjeffwan@gmail.com>")[…](https://github.com/vllm-project/aibrix/pull/2872)  | Oct 8, 2026  |  
|  [[API] Rename the ModelClaim keys and cap claim names at 63 characters (](https://github.com/vllm-project/aibrix/commit/fadc14f560308208b77628181e26bc2af8fbcd8b "\[API\] Rename the ModelClaim keys and cap claim names at 63 characters \(#2946\)

* Move the ModelClaim keys under claim.model.aibrix.ai
- pool.aibrix.ai/enabled -> claim.model.aibrix.ai/enabled
- pool.aibrix.ai/name -> claim.model.aibrix.ai/pool
- pool.aibrix.ai/policy -> claim.model.aibrix.ai/pool-policy
- modelclaim.aibrix.ai/<claim> -> route.claim.model.aibrix.ai/<claim>

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>")[…](https://github.com/vllm-project/aibrix/pull/2946)  | Oct 8, 2026  |  
|  [[UI] Batch: expose adaptive AIMD ramp-up controls (](https://github.com/vllm-project/aibrix/commit/2be431de9196e4aa4d7f54689bd32a39c9ee6c11 "\[UI\] Batch: expose adaptive AIMD ramp-up controls \(#2602\)

* feat\(console\): expose adaptive AIMD settings
* fix\(console\): disable inactive adaptive settings

Signed-off-by: Jiaxin Shan <seedjeffwan@gmail.com>")[#2602](https://github.com/vllm-project/aibrix/pull/2602)[)](https://github.com/vllm-project/aibrix/commit/2be431de9196e4aa4d7f54689bd32a39c9ee6c11 "\[UI\] Batch: expose adaptive AIMD ramp-up controls \(#2602\)

* feat\(console\): expose adaptive AIMD settings
* fix\(console\): disable inactive adaptive settings

Signed-off-by: Jiaxin Shan <seedjeffwan@gmail.com>")  | Oct 3, 2026  |  
|   |   |  [docs: fix broken relative links in docs (](https://github.com/vllm-project/aibrix/commit/32987960ecd000e84d8c815470500e03281f9135 "docs: fix broken relative links in docs \(#2687\)

Fix broken relative links in docs")[#2687](https://github.com/vllm-project/aibrix/pull/2687)[)](https://github.com/vllm-project/aibrix/commit/32987960ecd000e84d8c815470500e03281f9135 "docs: fix broken relative links in docs \(#2687\)

Fix broken relative links in docs")  | Oct 6, 2026  |  
|   |   |  [Feat/brixbench dynamo v1.3.1 v1.4.0 (](https://github.com/vllm-project/aibrix/commit/56d016ae194924f630d037037d1c9c62bfec3129 "Feat/brixbench dynamo v1.3.1 v1.4.0 \(#2644\)

* feat\(brixbench\): add Dynamo v1.3.1 routing scenario

Add a version-specific 4p4d multinode routing fixture for Dynamo 1.3.1
without changing the existing 1.2.1 scenario. The engine YAML keeps
image NIXL/CUDA paths, disables worker health checks, and leaves
PodMonitor ownership to Brixbench.

Signed-off-by: jinhyu.kim <jinhyu.kim@bytedance.com>

* fix\(brixbench\): support Dynamo v1.3.1 CRD upgrades

Keep namespace-restricted installs for Dynamo 1.2.1. Skip that Helm
set for 1.3.1 and later so CRD upgrades can run cluster-wide.

Signed-off-by: jinhyu.kim <jinhyu.kim@bytedance.com>

* fix\(brixbench\): skip Dynamo release LFS downloads

Checkout Dynamo release tags with GIT_LFS_SKIP_SMUDGE=1 so Git LFS
test-media blobs are not downloaded.

Signed-off-by: jinhyu.kim <jinhyu.kim@bytedance.com>

* fix\(brixbench\): report Dynamo v1.3 engine version

Map Dynamo 1.3.x aggregate CSV engine version to bundled vLLM 0.23.0
and keep Dynamo 1.2.1 on 0.21.0.

Signed-off-by: jinhyu.kim <jinhyu.kim@bytedance.com>

* feat\(brixbench\): add Dynamo v1.4.0 routing scenario

Add a version-gated 4p4d multinode fixture from the official v1.4.0
release \(vLLM 0.26.0, CUDA 13, NIXL 1.3.2\) without changing 1.2.1 or 1.3.1.

Signed-off-by: jinhyu.kim <jinhyu.kim@bytedance.com>

* fix\(brixbench\): report Dynamo v1.4 engine version

Map Dynamo 1.4.x to bundled vLLM 0.26.0 from the official v1.4.0
release notes. Keep 1.3.x at 0.23.0 and 1.2.1 at 0.21.0.

Signed-off-by: jinhyu.kim <jinhyu.kim@bytedance.com>

* fix\(brixbench\): drop Dynamo v1.4.0 frontend --router-reset-states

The v1.4.0 frontend rejects that flag as an unknown argument. Leave it on the v1.2.1 and v1.3.1 fixtures.

Signed-off-by: jinhyu.kim <jinhyu.kim@bytedance.com>

---------

Signed-off-by: jinhyu.kim <jinhyu.kim@bytedance.com>
Co-authored-by: jinhyu.kim <jinhyu.kim@bytedance.com>
Co-authored-by: Varun Gupta <varungup90@gmail.com>")[#2644](https://github.com/vllm-project/aibrix/pull/2644)[)](https://github.com/vllm-project/aibrix/commit/56d016ae194924f630d037037d1c9c62bfec3129 "Feat/brixbench dynamo v1.3.1 v1.4.0 \(#2644\)

* feat\(brixbench\): add Dynamo v1.3.1 routing scenario

Add a version-specific 4p4d multinode routing fixture for Dynamo 1.3.1
without changing the existing 1.2.1 scenario. The engine YAML keeps
image NIXL/CUDA paths, disables worker health checks, and leaves
PodMonitor ownership to Brixbench.

Signed-off-by: jinhyu.kim <jinhyu.kim@bytedance.com>

* fix\(brixbench\): support Dynamo v1.3.1 CRD upgrades

Keep namespace-restricted installs for Dynamo 1.2.1. Skip that Helm
set for 1.3.1 and later so CRD upgrades can run cluster-wide.

Signed-off-by: jinhyu.kim <jinhyu.kim@bytedance.com>

* fix\(brixbench\): skip Dynamo release LFS downloads

Checkout Dynamo release tags with GIT_LFS_SKIP_SMUDGE=1 so Git LFS
test-media blobs are not downloaded.

Signed-off-by: jinhyu.kim <jinhyu.kim@bytedance.com>

* fix\(brixbench\): report Dynamo v1.3 engine version

Map Dynamo 1.3.x aggregate CSV engine version to bundled vLLM 0.23.0
and keep Dynamo 1.2.1 on 0.21.0.

Signed-off-by: jinhyu.kim <jinhyu.kim@bytedance.com>

* feat\(brixbench\): add Dynamo v1.4.0 routing scenario

Add a version-gated 4p4d multinode fixture from the official v1.4.0
release \(vLLM 0.26.0, CUDA 13, NIXL 1.3.2\) without changing 1.2.1 or 1.3.1.

Signed-off-by: jinhyu.kim <jinhyu.kim@bytedance.com>

* fix\(brixbench\): report Dynamo v1.4 engine version

Map Dynamo 1.4.x to bundled vLLM 0.26.0 from the official v1.4.0
release notes. Keep 1.3.x at 0.23.0 and 1.2.1 at 0.21.0.

Signed-off-by: jinhyu.kim <jinhyu.kim@bytedance.com>

* fix\(brixbench\): drop Dynamo v1.4.0 frontend --router-reset-states

The v1.4.0 frontend rejects that flag as an unknown argument. Leave it on the v1.2.1 and v1.3.1 fixtures.

Signed-off-by: jinhyu.kim <jinhyu.kim@bytedance.com>

---------

Signed-off-by: jinhyu.kim <jinhyu.kim@bytedance.com>
Co-authored-by: jinhyu.kim <jinhyu.kim@bytedance.com>
Co-authored-by: Varun Gupta <varungup90@gmail.com>")  | Aug 31, 2026  |  
| [build/container](https://github.com/vllm-project/aibrix/tree/main/build/container "This path skips through empty directories")  | [build/container](https://github.com/vllm-project/aibrix/tree/main/build/container "This path skips through empty directories")  |   | Oct 9, 2026  |  
|  [[Feat] Verify gateway model-list discovery on demand (](https://github.com/vllm-project/aibrix/commit/8e5775cbc1522c01f5bca4c7297aeb12e0ef415f "\[Feat\] Verify gateway model-list discovery on demand \(#2915\)

* \[Feat\] Verify gateway model-list discovery on demand

Add an opt-in maximum age for Kubernetes model-list verification. Compare a complete Pod and enabled-adapter snapshot with events applied to the cache, return 503 when discovery cannot be trusted, and serve that response during initial sync. Keep the default listing path unchanged.

Signed-off-by: Andrea Abellonio <andrea.abellonio@servescale.ai>

* \[Fix\] Reject invalid model discovery events before cache handling

Validate required discovery objects before invoking cache handlers. A typed nil object now invalidates the model-list proof instead of causing a panic or leaving a stale proof healthy. Add regression coverage for Pods and ModelAdapters.

Signed-off-by: Andrea Abellonio <andrea.abellonio@servescale.ai>

---------

Signed-off-by: Andrea Abellonio <andrea.abellonio@servescale.ai>")[#2915](https://github.com/vllm-project/aibrix/pull/2915)[)](https://github.com/vllm-project/aibrix/commit/8e5775cbc1522c01f5bca4c7297aeb12e0ef415f "\[Feat\] Verify gateway model-list discovery on demand \(#2915\)

* \[Feat\] Verify gateway model-list discovery on demand

Add an opt-in maximum age for Kubernetes model-list verification. Compare a complete Pod and enabled-adapter snapshot with events applied to the cache, return 503 when discovery cannot be trusted, and serve that response during initial sync. Keep the default listing path unchanged.

Signed-off-by: Andrea Abellonio <andrea.abellonio@servescale.ai>

* \[Fix\] Reject invalid model discovery events before cache handling

Validate required discovery objects before invoking cache handlers. A typed nil object now invalidates the model-list proof instead of causing a panic or leaving a stale proof healthy. Add regression coverage for Pods and ModelAdapters.

Signed-off-by: Andrea Abellonio <andrea.abellonio@servescale.ai>

---------

Signed-off-by: Andrea Abellonio <andrea.abellonio@servescale.ai>")  | Oct 6, 2026  |  
|   |   |   | Oct 9, 2026  |  
|   |   |  [docs: fix broken relative links in docs (](https://github.com/vllm-project/aibrix/commit/32987960ecd000e84d8c815470500e03281f9135 "docs: fix broken relative links in docs \(#2687\)

Fix broken relative links in docs")[#2687](https://github.com/vllm-project/aibrix/pull/2687)[)](https://github.com/vllm-project/aibrix/commit/32987960ecd000e84d8c815470500e03281f9135 "docs: fix broken relative links in docs \(#2687\)

Fix broken relative links in docs")  | Oct 6, 2026  |  
|   |   |  [[Bug] Keep a claim on a warm pod that leaves the pool (](https://github.com/vllm-project/aibrix/commit/bea7d00f059b99757b2f336e88129351cf17cba1 "\[Bug\] Keep a claim on a warm pod that leaves the pool \(#2955\)

* Keep a claim on a warm pod that leaves the pool

When a warm pod stopped being a candidate while it still ran, for
example after its enabled label was removed, the controller dropped the
claim's instance and placed the claim on another pod. The old engine
kept running with its route, so the gateway kept sending it requests,
and nothing tracked it: deleting the claim did not stop it.

A candidate is where a new instance may go. An instance whose pod still
runs now stays, and its engine keeps serving the claim there. Only an
instance whose pod is gone, does not run, or is being deleted is
dropped, and a pod that still exists loses the claim's route and wake
request. A pod that cannot be read keeps its instance until a later
pass can tell.

The guide says that a claim stays on a pod that loses either label.

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>

* Read the Pod's routes in the runbook when it has no annotations

The deletion check lists the route annotations left on the warm Pod.
Once the claims are gone, a Pod with no other annotations has no
metadata.annotations, and jq fails on the keys of null. Read an empty
object instead, so the check prints an empty list.

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>

* Pass the events of a pod that keeps a claim after leaving its pool

A pod that has left its pool can still run a claim's instance, but the
controller ignored its events. Its wake requests, and the pod stopping
or being deleted, reached the claim only on its next periodic pass, up
to 10 seconds later. The pod event filter now also passes a pod that
carries a claim's route, and an update in which either version of the
pod passes.

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>

---------

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>")[#2955](https://github.com/vllm-project/aibrix/pull/2955)[)](https://github.com/vllm-project/aibrix/commit/bea7d00f059b99757b2f336e88129351cf17cba1 "\[Bug\] Keep a claim on a warm pod that leaves the pool \(#2955\)

* Keep a claim on a warm pod that leaves the pool

When a warm pod stopped being a candidate while it still ran, for
example after its enabled label was removed, the controller dropped the
claim's instance and placed the claim on another pod. The old engine
kept running with its route, so the gateway kept sending it requests,
and nothing tracked it: deleting the claim did not stop it.

A candidate is where a new instance may go. An instance whose pod still
runs now stays, and its engine keeps serving the claim there. Only an
instance whose pod is gone, does not run, or is being deleted is
dropped, and a pod that still exists loses the claim's route and wake
request. A pod that cannot be read keeps its instance until a later
pass can tell.

The guide says that a claim stays on a pod that loses either label.

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>

* Read the Pod's routes in the runbook when it has no annotations

The deletion check lists the route annotations left on the warm Pod.
Once the claims are gone, a Pod with no other annotations has no
metadata.annotations, and jq fails on the keys of null. Read an empty
object instead, so the check prints an empty list.

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>

* Pass the events of a pod that keeps a claim after leaving its pool

A pod that has left its pool can still run a claim's instance, but the
controller ignored its events. Its wake requests, and the pod stopping
or being deleted, reached the claim only on its next periodic pass, up
to 10 seconds later. The pod event filter now also passes a pod that
carries a claim's route, and an update in which either version of the
pod passes.

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>

---------

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>")  | Oct 9, 2026  |  
| [dist/chart](https://github.com/vllm-project/aibrix/tree/main/dist/chart "This path skips through empty directories")  | [dist/chart](https://github.com/vllm-project/aibrix/tree/main/dist/chart "This path skips through empty directories")  |   | Oct 9, 2026  |  
|   | Oct 9, 2026  |  
|  [[API] Add ModelWarmup image preloading (](https://github.com/vllm-project/aibrix/commit/26fd44c908b0de2bce4acf6b1984c34bbdd1ff9c "\[API\] Add ModelWarmup image preloading \(#2769\)

* \[API\] Add ModelWarmup image preloading

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[API\] Add ModelWarmup Helm CRD

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[Misc\] Normalize ModelWarmup license headers

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[API\] Fix ModelWarmup CI and retry semantics

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[Bug\] Make RoleSet finalization idempotent

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[Bug\] Refresh objects before removing finalizers

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[CI\] Use tainted Ready nodes in ModelWarmup E2E

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[API\] Address ModelWarmup review feedback

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[Bug\] Preserve failed ModelWarmup diagnostics

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[Docs\] Clarify ModelWarmup prerequisites

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[CI\] Fix ModelWarmup installation E2E

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[CI\] Capture ModelWarmup controller diagnostics

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[CI\] Grant ModelWarmup namespace cache permissions

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* fix: harden ModelWarmup lifecycle and validation

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

---------

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>")[#2769](https://github.com/vllm-project/aibrix/pull/2769)[)](https://github.com/vllm-project/aibrix/commit/26fd44c908b0de2bce4acf6b1984c34bbdd1ff9c "\[API\] Add ModelWarmup image preloading \(#2769\)

* \[API\] Add ModelWarmup image preloading

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[API\] Add ModelWarmup Helm CRD

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[Misc\] Normalize ModelWarmup license headers

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[API\] Fix ModelWarmup CI and retry semantics

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[Bug\] Make RoleSet finalization idempotent

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[Bug\] Refresh objects before removing finalizers

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[CI\] Use tainted Ready nodes in ModelWarmup E2E

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[API\] Address ModelWarmup review feedback

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[Bug\] Preserve failed ModelWarmup diagnostics

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[Docs\] Clarify ModelWarmup prerequisites

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[CI\] Fix ModelWarmup installation E2E

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[CI\] Capture ModelWarmup controller diagnostics

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* \[CI\] Grant ModelWarmup namespace cache permissions

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

* fix: harden ModelWarmup lifecycle and validation

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>

---------

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>")  | Oct 2, 2026  |  
|   |   |  [[API] Rename the ModelClaim keys and cap claim names at 63 characters (](https://github.com/vllm-project/aibrix/commit/fadc14f560308208b77628181e26bc2af8fbcd8b "\[API\] Rename the ModelClaim keys and cap claim names at 63 characters \(#2946\)

* Move the ModelClaim keys under claim.model.aibrix.ai
- pool.aibrix.ai/enabled -> claim.model.aibrix.ai/enabled
- pool.aibrix.ai/name -> claim.model.aibrix.ai/pool
- pool.aibrix.ai/policy -> claim.model.aibrix.ai/pool-policy
- modelclaim.aibrix.ai/<claim> -> route.claim.model.aibrix.ai/<claim>

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>")[…](https://github.com/vllm-project/aibrix/pull/2946)  | Oct 8, 2026  |  
|  [[Bug] Keep a claim on a warm pod that leaves the pool (](https://github.com/vllm-project/aibrix/commit/bea7d00f059b99757b2f336e88129351cf17cba1 "\[Bug\] Keep a claim on a warm pod that leaves the pool \(#2955\)

* Keep a claim on a warm pod that leaves the pool

When a warm pod stopped being a candidate while it still ran, for
example after its enabled label was removed, the controller dropped the
claim's instance and placed the claim on another pod. The old engine
kept running with its route, so the gateway kept sending it requests,
and nothing tracked it: deleting the claim did not stop it.

A candidate is where a new instance may go. An instance whose pod still
runs now stays, and its engine keeps serving the claim there. Only an
instance whose pod is gone, does not run, or is being deleted is
dropped, and a pod that still exists loses the claim's route and wake
request. A pod that cannot be read keeps its instance until a later
pass can tell.

The guide says that a claim stays on a pod that loses either label.

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>

* Read the Pod's routes in the runbook when it has no annotations

The deletion check lists the route annotations left on the warm Pod.
Once the claims are gone, a Pod with no other annotations has no
metadata.annotations, and jq fails on the keys of null. Read an empty
object instead, so the check prints an empty list.

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>

* Pass the events of a pod that keeps a claim after leaving its pool

A pod that has left its pool can still run a claim's instance, but the
controller ignored its events. Its wake requests, and the pod stopping
or being deleted, reached the claim only on its next periodic pass, up
to 10 seconds later. The pod event filter now also passes a pod that
carries a claim's route, and an update in which either version of the
pod passes.

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>

---------

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>")[#2955](https://github.com/vllm-project/aibrix/pull/2955)[)](https://github.com/vllm-project/aibrix/commit/bea7d00f059b99757b2f336e88129351cf17cba1 "\[Bug\] Keep a claim on a warm pod that leaves the pool \(#2955\)

* Keep a claim on a warm pod that leaves the pool

When a warm pod stopped being a candidate while it still ran, for
example after its enabled label was removed, the controller dropped the
claim's instance and placed the claim on another pod. The old engine
kept running with its route, so the gateway kept sending it requests,
and nothing tracked it: deleting the claim did not stop it.

A candidate is where a new instance may go. An instance whose pod still
runs now stays, and its engine keeps serving the claim there. Only an
instance whose pod is gone, does not run, or is being deleted is
dropped, and a pod that still exists loses the claim's route and wake
request. A pod that cannot be read keeps its instance until a later
pass can tell.

The guide says that a claim stays on a pod that loses either label.

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>

* Read the Pod's routes in the runbook when it has no annotations

The deletion check lists the route annotations left on the warm Pod.
Once the claims are gone, a Pod with no other annotations has no
metadata.annotations, and jq fails on the keys of null. Read an empty
object instead, so the check prints an empty list.

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>

* Pass the events of a pod that keeps a claim after leaving its pool

A pod that has left its pool can still run a claim's instance, but the
controller ignored its events. Its wake requests, and the pod stopping
or being deleted, reached the claim only on its next periodic pass, up
to 10 seconds later. The pod event filter now also passes a pod that
carries a claim's route, and an update in which either version of the
pod passes.

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>

---------

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>")  | Oct 9, 2026  |  
|   |   | [[Bug] Fix flaky test_job_restore_after_mds_crash_during_in_progress (…](https://github.com/vllm-project/aibrix/commit/6a72bb12465c8cb41b63fbe72af1079b983854ae "\[Bug\] Fix flaky test_job_restore_after_mds_crash_during_in_progress \(crash re-arms on restart\) \(#2959\)

fix\(batch\): sync restarted driver with consumed crash before recovery

wait_for_status reaches "in_progress" at validation time, before the
injected crash \(crash_after_n_requests=1\) fires during dispatch. The
restarted app was then built immediately, so its JobDriverErrorInjector
read a pre-crash snapshot with last_crashed_at unset, re-armed the
injection, and crashed a second time. The job was left stuck
in_progress until the poll timeout, failing the test.

Add wait_for_crash_consumed which reads the shared metastore until
status.last_crashed_at is persisted, and call it before building the
restarted app so recovery mirrors a crash-then-restart sequence. This
fixes test_job_restore_after_mds_crash_during_in_progress flakiness.

Signed-off-by: Saylee <saylee21@users.noreply.github.com>
Co-authored-by: Saylee <saylee21@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   |   | Oct 9, 2026  |  
|   |   |  [Link to system-installed zmq without building (](https://github.com/vllm-project/aibrix/commit/68bc872dcb201d7ad795bba931d7c6ea6ba232e7 "Link to system-installed zmq without building \(#1372\)

Signed-off-by: Qizhong Mao <qizhong.mao@bytedance.com>")[#1372](https://github.com/vllm-project/aibrix/pull/1372)[)](https://github.com/vllm-project/aibrix/commit/68bc872dcb201d7ad795bba931d7c6ea6ba232e7 "Link to system-installed zmq without building \(#1372\)

Signed-off-by: Qizhong Mao <qizhong.mao@bytedance.com>")  | Oct 17, 2025  |  
|  [[Bug] Keep a claim on a warm pod that leaves the pool (](https://github.com/vllm-project/aibrix/commit/bea7d00f059b99757b2f336e88129351cf17cba1 "\[Bug\] Keep a claim on a warm pod that leaves the pool \(#2955\)

* Keep a claim on a warm pod that leaves the pool

When a warm pod stopped being a candidate while it still ran, for
example after its enabled label was removed, the controller dropped the
claim's instance and placed the claim on another pod. The old engine
kept running with its route, so the gateway kept sending it requests,
and nothing tracked it: deleting the claim did not stop it.

A candidate is where a new instance may go. An instance whose pod still
runs now stays, and its engine keeps serving the claim there. Only an
instance whose pod is gone, does not run, or is being deleted is
dropped, and a pod that still exists loses the claim's route and wake
request. A pod that cannot be read keeps its instance until a later
pass can tell.

The guide says that a claim stays on a pod that loses either label.

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>

* Read the Pod's routes in the runbook when it has no annotations

The deletion check lists the route annotations left on the warm Pod.
Once the claims are gone, a Pod with no other annotations has no
metadata.annotations, and jq fails on the keys of null. Read an empty
object instead, so the check prints an empty list.

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>

* Pass the events of a pod that keeps a claim after leaving its pool

A pod that has left its pool can still run a claim's instance, but the
controller ignored its events. Its wake requests, and the pod stopping
or being deleted, reached the claim only on its next periodic pass, up
to 10 seconds later. The pod event filter now also passes a pod that
carries a claim's route, and an update in which either version of the
pod passes.

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>

---------

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>")[#2955](https://github.com/vllm-project/aibrix/pull/2955)[)](https://github.com/vllm-project/aibrix/commit/bea7d00f059b99757b2f336e88129351cf17cba1 "\[Bug\] Keep a claim on a warm pod that leaves the pool \(#2955\)

* Keep a claim on a warm pod that leaves the pool

When a warm pod stopped being a candidate while it still ran, for
example after its enabled label was removed, the controller dropped the
claim's instance and placed the claim on another pod. The old engine
kept running with its route, so the gateway kept sending it requests,
and nothing tracked it: deleting the claim did not stop it.

A candidate is where a new instance may go. An instance whose pod still
runs now stays, and its engine keeps serving the claim there. Only an
instance whose pod is gone, does not run, or is being deleted is
dropped, and a pod that still exists loses the claim's route and wake
request. A pod that cannot be read keeps its instance until a later
pass can tell.

The guide says that a claim stays on a pod that loses either label.

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>

* Read the Pod's routes in the runbook when it has no annotations

The deletion check lists the route annotations left on the warm Pod.
Once the claims are gone, a Pod with no other annotations has no
metadata.annotations, and jq fails on the keys of null. Read an empty
object instead, so the check prints an empty list.

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>

* Pass the events of a pod that keeps a claim after leaving its pool

A pod that has left its pool can still run a claim's instance, but the
controller ignored its events. Its wake requests, and the pod stopping
or being deleted, reached the claim only on its next periodic pass, up
to 10 seconds later. The pod event filter now also passes a pod that
carries a claim's route, and an update in which either version of the
pod passes.

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>

---------

Signed-off-by: Zeyu Zhang <zhang@zeyu.tw>")  | Oct 9, 2026  |  
|   |   |  [[Docs] Add Simplified Chinese catalogs and a navbar language switcher (](https://github.com/vllm-project/aibrix/commit/3a379cb9c72bfd0477a59fed1e75508e0a2bdc38 "\[Docs\] Add Simplified Chinese catalogs and a navbar language switcher \(#2700\)

* \[Docs\] Add Simplified Chinese catalogs and a navbar language switcher

Signed-off-by: zhoujinyu <2319109590@qq.com>

* \[Docs\] Harden zh-CN language switcher before publish

Address review feedback on PR #2700: fix file:// redirects, prefer
RTD/local build path segments when rewriting URLs, preserve non-en/zh
Sphinx languages, and gate the Chinese navbar option on Read the Docs
until AIBRIX_DOCS_SHOW_ZH is enabled after the translation project is
linked.

Signed-off-by: Cursor Agent <cursoragent@cursor.com>

Co-authored-by: Varun Gupta <varungup90@gmail.com>

---------

Signed-off-by: zhoujinyu <2319109590@qq.com>
Co-authored-by: Cursor Agent <cursoragent@cursor.com>
Co-authored-by: Varun Gupta <varungup90@gmail.com>
Co-authored-by: CYJiang <86391540+googs1025@users.noreply.github.com>")[…](https://github.com/vllm-project/aibrix/pull/2700)  | Oct 8, 2026  |  
|   |   |  [[CI] Upgrade golangci-lint to v2.13.1 (](https://github.com/vllm-project/aibrix/commit/c05557c2e7b60cff0761b5017d1690ae1fa7e7a1 "\[CI\] Upgrade golangci-lint to v2.13.1 \(#2630\)")[#2630](https://github.com/vllm-project/aibrix/pull/2630)[)](https://github.com/vllm-project/aibrix/commit/c05557c2e7b60cff0761b5017d1690ae1fa7e7a1 "\[CI\] Upgrade golangci-lint to v2.13.1 \(#2630\)")  | Sep 4, 2026  |  
|   |   |  [[Docs] Publish English and Chinese docs from one RTD project (](https://github.com/vllm-project/aibrix/commit/4a8be316264e5f9b1c6c96b29298595be5589201 "\[Docs\] Publish English and Chinese docs from one RTD project \(#2950\)

* docs: publish Chinese pages from one RTD project
* docs: require synchronized Chinese translations
* docs: translate newly added documentation pages
* docs: use RTD virtualenv for Sphinx builds

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>")[#2950](https://github.com/vllm-project/aibrix/pull/2950)[)](https://github.com/vllm-project/aibrix/commit/4a8be316264e5f9b1c6c96b29298595be5589201 "\[Docs\] Publish English and Chinese docs from one RTD project \(#2950\)

* docs: publish Chinese pages from one RTD project
* docs: require synchronized Chinese translations
* docs: translate newly added documentation pages
* docs: use RTD virtualenv for Sphinx builds

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   |  [[Docs] Publish English and Chinese docs from one RTD project (](https://github.com/vllm-project/aibrix/commit/4a8be316264e5f9b1c6c96b29298595be5589201 "\[Docs\] Publish English and Chinese docs from one RTD project \(#2950\)

* docs: publish Chinese pages from one RTD project
* docs: require synchronized Chinese translations
* docs: translate newly added documentation pages
* docs: use RTD virtualenv for Sphinx builds

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>")[#2950](https://github.com/vllm-project/aibrix/pull/2950)[)](https://github.com/vllm-project/aibrix/commit/4a8be316264e5f9b1c6c96b29298595be5589201 "\[Docs\] Publish English and Chinese docs from one RTD project \(#2950\)

* docs: publish Chinese pages from one RTD project
* docs: require synchronized Chinese translations
* docs: translate newly added documentation pages
* docs: use RTD virtualenv for Sphinx builds

Signed-off-by: CYJiang <86391540+googs1025@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   |  [Add common project documents and skeleton folders (](https://github.com/vllm-project/aibrix/commit/737677fa8a9e3ace6f490bcb8ee160dfefcac74d "Add common project documents and skeleton folders \(#4\)

* Update README.md

* Add common project documents

* Add project skeleton folder")[#4](https://github.com/vllm-project/aibrix/pull/4)[)](https://github.com/vllm-project/aibrix/commit/737677fa8a9e3ace6f490bcb8ee160dfefcac74d "Add common project documents and skeleton folders \(#4\)

* Update README.md

* Add common project documents

* Add project skeleton folder")  | Jul 2, 2024  |  
|   |   |  [docs: fill TODO placeholders in CONTRIBUTING.md with community links (](https://github.com/vllm-project/aibrix/commit/a40be9ba404730e4be2d2a0820942bca79275dc6 "docs: fill TODO placeholders in CONTRIBUTING.md with community links \(#2489\)

Replace TODO placeholders with actual GitHub and Slack URLs:
- Community events: link to Slack and Discussions
- Communication channels: Slack #aibrix, GitHub Discussions/Issues
- FAQ: point to Discussions and Slack
- Contact: provide concrete GitHub and Slack links

Removed WeChat/Email TODOs as these channels aren't established.

Signed-off-by: arijitroy003 <arijitroy003@gmail.com>")[#…](https://github.com/vllm-project/aibrix/pull/2489)  | Jul 25, 2026  |  
|   |   |   | Jun 10, 2024  |  
|   |   |  [fix: separate GPU optimizer deps to reduce runtime image size (](https://github.com/vllm-project/aibrix/commit/048a70f99d427b489d1971813ca101f2cfddf5dc "fix: separate GPU optimizer deps to reduce runtime image size \(#2863\) \(#2872\)

* fix: separate GPU optimizer deps to reduce runtime image size \(#2863\)

The aibrix/runtime and aibrix/metadata-service nightly images were both
~970 MB because all GPU Optimizer scientific-computing packages \(numpy,
pandas, matplotlib, PuLP, Dash, incdbscan — ~584 MiB\) were declared as
main Poetry dependencies, even though they are only needed by
aibrix.gpu_optimizer \(load monitor, visualizer, LP solver\).

The Dockerfile comment stated these packages were excluded via
'--without profiling', but they were never in the profiling group, so
that flag had no effect on them.

Additionally, both images were built from the same Dockerfile with no
--target specified, so they ended up identical in content but pushed as
separate tags with different layer digests \(no layer sharing\).

Changes:
- python/aibrix/pyproject.toml: move numpy, pandas, PuLP, incdbscan,
  dash, matplotlib out of main deps into a new
  \[tool.poetry.group.gpu_optimizer.dependencies\] group. Fix stale
  comments.
- build/container/Dockerfile.python: add two named build targets:
    'runtime'          -- slim image, no GPU optimizer deps \(~584 MiB saved\)
    'metadata-service' -- extends shared base, installs gpu_optimizer
                          group so GPU Optimizer keeps running there
- .github/workflows/docker-push-images.yml,
  .github/workflows/release-build.yaml,
  .github/workflows/installation-smoke-tests.yml,
  .github/workflows/installation-tests.yml:
  add target: runtime / metadata-service to matrix entries and pass
  target to docker/build-push-action.
* fix: address reviewer comments for dependency split and docker builds
* Use gpu-optimizer extra for metadata-service image

Signed-off-by: Paramveersingh-S <paramveersingh-s@users.noreply.github.com>
Signed-off-by: Jiaxin Shan <seedjeffwan@gmail.com>
Co-authored-by: Paramveersingh-S <paramveersingh-s@users.noreply.github.com>
Co-authored-by: Jiaxin Shan <seedjeffwan@gmail.com>")[#2863](https://github.com/vllm-project/aibrix/issues/2863)[) (](https://github.com/vllm-project/aibrix/commit/048a70f99d427b489d1971813ca101f2cfddf5dc "fix: separate GPU optimizer deps to reduce runtime image size \(#2863\) \(#2872\)

* fix: separate GPU optimizer deps to reduce runtime image size \(#2863\)

The aibrix/runtime and aibrix/metadata-service nightly images were both
~970 MB because all GPU Optimizer scientific-computing packages \(numpy,
pandas, matplotlib, PuLP, Dash, incdbscan — ~584 MiB\) were declared as
main Poetry dependencies, even though they are only needed by
aibrix.gpu_optimizer \(load monitor, visualizer, LP solver\).

The Dockerfile comment stated these packages were excluded via
'--without profiling', but they were never in the profiling group, so
that flag had no effect on them.

Additionally, both images were built from the same Dockerfile with no
--target specified, so they ended up identical in content but pushed as
separate tags with different layer digests \(no layer sharing\).

Changes:
- python/aibrix/pyproject.toml: move numpy, pandas, PuLP, incdbscan,
  dash, matplotlib out of main deps into a new
  \[tool.poetry.group.gpu_optimizer.dependencies\] group. Fix stale
  comments.
- build/container/Dockerfile.python: add two named build targets:
    'runtime'          -- slim image, no GPU optimizer deps \(~584 MiB saved\)
    'metadata-service' -- extends shared base, installs gpu_optimizer
                          group so GPU Optimizer keeps running there
- .github/workflows/docker-push-images.yml,
  .github/workflows/release-build.yaml,
  .github/workflows/installation-smoke-tests.yml,
  .github/workflows/installation-tests.yml:
  add target: runtime / metadata-service to matrix entries and pass
  target to docker/build-push-action.
* fix: address reviewer comments for dependency split and docker builds
* Use gpu-optimizer extra for metadata-service image

Signed-off-by: Paramveersingh-S <paramveersingh-s@users.noreply.github.com>
Signed-off-by: Jiaxin Shan <seedjeffwan@gmail.com>
Co-authored-by: Paramveersingh-S <paramveersingh-s@users.noreply.github.com>
Co-authored-by: Jiaxin Shan <seedjeffwan@gmail.com>")[…](https://github.com/vllm-project/aibrix/pull/2872)  | Oct 8, 2026  |  
|   |   |  [[Feat]: add podautoscaler webhook (](https://github.com/vllm-project/aibrix/commit/dfb5b35c97c236d2ee9322df08d6d747f6aff3ad "\[Feat\]: add podautoscaler webhook \(#1683\)

add podautoscaler webhook

Signed-off-by: googs1025 <googs1025@gmail.com>")[#1683](https://github.com/vllm-project/aibrix/pull/1683)[)](https://github.com/vllm-project/aibrix/commit/dfb5b35c97c236d2ee9322df08d6d747f6aff3ad "\[Feat\]: add podautoscaler webhook \(#1683\)

add podautoscaler webhook

Signed-off-by: googs1025 <googs1025@gmail.com>")  | Oct 23, 2025  |  
|   |   |   | Oct 9, 2026  |  
|   |   |  [Create SECURITY.md to enable security policy (](https://github.com/vllm-project/aibrix/commit/4d8cf44ce59924ce46517bcfdc0ca443495a1701 "Create SECURITY.md to enable security policy \(#756\)

Create SECURITY.md

Signed-off-by: Liguang Xie <liguang.vt@gmail.com>")[#756](https://github.com/vllm-project/aibrix/pull/756)[)](https://github.com/vllm-project/aibrix/commit/4d8cf44ce59924ce46517bcfdc0ca443495a1701 "Create SECURITY.md to enable security policy \(#756\)

Create SECURITY.md

Signed-off-by: Liguang Xie <liguang.vt@gmail.com>")  | Feb 27, 2025  |  
|   |   |  [fix(telemetry): propagate PD routing traces (](https://github.com/vllm-project/aibrix/commit/213b64711d94fc2fffe064cd21e02080c69c726f "fix\(telemetry\): propagate PD routing traces \(#2697\)")[#2697](https://github.com/vllm-project/aibrix/pull/2697)[)](https://github.com/vllm-project/aibrix/commit/213b64711d94fc2fffe064cd21e02080c69c726f "fix\(telemetry\): propagate PD routing traces \(#2697\)")  | Sep 10, 2026  |  
|   |   |  [fix(telemetry): propagate PD routing traces (](https://github.com/vllm-project/aibrix/commit/213b64711d94fc2fffe064cd21e02080c69c726f "fix\(telemetry\): propagate PD routing traces \(#2697\)")[#2697](https://github.com/vllm-project/aibrix/pull/2697)[)](https://github.com/vllm-project/aibrix/commit/213b64711d94fc2fffe064cd21e02080c69c726f "fix\(telemetry\): propagate PD routing traces \(#2697\)")  | Sep 10, 2026  |  
| View all files |  
## Repository files navigation
# AIBrix
Welcome to AIBrix, an open-source initiative designed to provide essential building blocks to construct scalable GenAI inference infrastructure. AIBrix delivers a cloud-native solution optimized for deploying, managing, and scaling large language model (LLM) inference, tailored specifically to enterprise needs.
| [**Documentation**](https://aibrix.readthedocs.io/latest/) | [**Blog**](https://aibrix.github.io/) | [**White Paper**](https://arxiv.org/abs/2504.03648) | [**Twitter/X**](https://x.com/vllm_project) | [**Developer Slack**](https://vllm-dev.slack.com/archives/C08EQ883CSV) | 
## Latest News
### Releases
  * **[2026-06-16]** AIBrix v0.7.0 is released. Check out the [release notes](https://github.com/vllm-project/aibrix/releases/tag/v0.7.0) and [Blog Post](https://aibrix.github.io/posts/2026-06-16-v0.7.0-release/) for more details.
  * **[2026-03-05]** AIBrix v0.6.0 is released. Check out the [release notes](https://github.com/vllm-project/aibrix/releases/tag/v0.6.0) and [Blog Post](https://aibrix.github.io/posts/2026-03-03-v0.6.0-release/) for more details.
  * **[2025-11-10]** AIBrix v0.5.0 is released. Check out the [release notes](https://github.com/vllm-project/aibrix/releases/tag/v0.5.0) and [Blog Post](https://aibrix.github.io/posts/2025-11-10-v0.5.0-release/) for more details.
  * **[2025-08-05]** AIBrix v0.4.0 is released. Check out the [release notes](https://github.com/vllm-project/aibrix/releases/tag/v0.4.0) and [Blog Post](https://aibrix.github.io/posts/2025-08-04-v0.4.0-release/) for more details.
  * **[2025-05-21]** AIBrix v0.3.0 is released. Check out the [release notes](https://github.com/vllm-project/aibrix/releases/tag/v0.3.0) and [Blog Post](https://aibrix.github.io/posts/2025-05-21-v0.3.0-release/) for more details.
  * **[2025-03-09]** AIBrix v0.2.1 is released. DeepSeek-R1 full weights deployment is supported and gateway stability has been improved! Check [Blog Post](https://aibrix.github.io/posts/2025-03-10-deepseek-r1/) for more details.
  * **[2025-02-19]** AIBrix v0.2.0 is released. Check out the [release notes](https://github.com/vllm-project/aibrix/releases/tag/v0.2.0) and [Blog Post](https://aibrix.github.io/posts/2025-02-05-v0.2.0-release/) for more details.
  * **[2024-11-13]** AIBrix v0.1.0 is released. Check out the [release notes](https://github.com/vllm-project/aibrix/releases/tag/v0.1.0) and [Blog Post](https://aibrix.github.io/posts/2024-11-12-v0.1.0-release/) for more details.


### Talks and Presentations
  * **[2025-11-12]** AIBrix team co-delivered a keynote at KubeCon North America 2025 [AIBrix: Kubernetes-native GenAI Inference Infrastructure](https://www.youtube.com/watch?v=7KHenRXNGAw&t=875s), providing AIBrix overview.
  * **[2025-06-10]** AIBrix team delivered a talk at KubeCon China 2025 titled [AIBrix: Cost-Effective and Scalable Kubernetes Control Plane for vLLM](https://kccncchn2025.sched.com/event/1x5im/introducing-aibrix-cost-effective-and-scalable-kubernetes-control-plane-for-vllm-jiaxin-shan-liguang-xie-bytedance), discussing how the framework optimizes vLLM deployment via Kubernetes for cost efficiency and scalability.
  * **[2025-04-04]** AIBrix team co-delivered a keynote at KubeCon EU 2025 with Google on [LLM-Aware Load Balancing in Kubernetes: A New Era of Efficiency](https://kccnceu2025.sched.com/event/1txC7/keynote-llm-aware-load-balancing-in-kubernetes-a-new-era-of-efficiency-clayton-coleman-distinguished-engineer-google-jiaxin-shan-software-engineer-bytedance), focusing on LLM specific routing solutions.
  * **[2025-03-30]** AIBrix was featured at the [ASPLOS'25](http://asplos-conference.org/asplos2025/) workshop with the presentation [AIBrix: An Open-Source, Large-Scale LLM Inference Infrastructure for System Research](https://docs.google.com/presentation/d/1YDVsPFTIgGXnROGaJ1VKuDDAB4T5fzpE/edit), showcasing its architecture for efficient LLM inference in system research scenarios.


## Key Features
The initial release includes the following key features:
  * **High-Density LoRA Management** : Streamlined support for lightweight, low-rank adaptations of models.
  * **LLM Gateway and Routing** : Efficiently manage and direct traffic across multiple models and replicas.
  * **LLM App-Tailored Autoscaler** : Dynamically scale inference resources based on real-time demand.
  * **Unified AI Runtime** : A versatile sidecar enabling metric standardization, model downloading, and management.
  * **Distributed Inference** : Scalable architecture to handle large workloads across multiple nodes.
  * **Distributed KV Cache** : Enables high-capacity, cross-engine KV reuse.
  * **Cost-efficient Heterogeneous Serving** : Enables mixed GPU inference to reduce costs with SLO guarantees.
  * **GPU Hardware Failure Detection** : Proactive detection of GPU hardware issues.


## Architecture
## Quick Start
To get started with AIBrix, clone this repository and follow the setup instructions in the documentation. Our comprehensive guide will help you configure and deploy your first LLM infrastructure seamlessly.

```
# Local Testing
git clone https://github.com/vllm-project/aibrix.git
cd aibrix

# Install nightly aibrix dependencies
kubectl apply -k config/dependency --server-side

# Install nightly AIBrix CRDs (separate from the operator so uninstalls don't wipe user CRs)
kubectl apply -k config/crd --server-side

# Install nightly aibrix components
kubectl apply -k config/default
```

Install stable distribution

```
# Install component dependencies
kubectl apply -f "https://github.com/vllm-project/aibrix/releases/download/v0.8.0-rc.1/aibrix-dependency-v0.8.0-rc.1.yaml" --server-side

# Install AIBrix CRDs (separate from the operator so uninstalls don't wipe user CRs)
kubectl apply -f "https://github.com/vllm-project/aibrix/releases/download/v0.8.0-rc.1/aibrix-core-crds-v0.8.0-rc.1.yaml" --server-side

# Install aibrix components
kubectl apply -f "https://github.com/vllm-project/aibrix/releases/download/v0.8.0-rc.1/aibrix-core-v0.8.0-rc.1.yaml"
```

## Documentation
For detailed documentation on installation, configuration, and usage, please visit our [documentation page](https://aibrix.readthedocs.io/latest/).
## Contributing
We welcome contributions from the community! Check out our [contributing guidelines](https://github.com/vllm-project/aibrix/blob/main/CONTRIBUTING.md) to see how you can make a difference.
Slack Channel: [#aibrix](https://vllm-dev.slack.com/archives/C08EQ883CSV)
## License
AIBrix is licensed under the [Apache 2.0 License](https://github.com/vllm-project/aibrix/blob/main/LICENSE).
## Support
If you have any questions or encounter any issues, please submit an issue on our [GitHub issues page](https://github.com/vllm-project/aibrix/issues).
Thank you for choosing AIBrix for your GenAI infrastructure needs!
## About
Cost-efficient and pluggable Infrastructure components for GenAI inference
### Resources
### Code of conduct
### Contributing
### Security policy
### Stars
**5.1k** stars
### Watchers
**52** watching
### Forks
[ forks](https://github.com/vllm-project/aibrix/forks)
## 
[ v0.7.0Latest Jun 18, 2026 ](https://github.com/vllm-project/aibrix/releases/tag/v0.7.0)
## 
## Languages
  * [Go55.9%](https://github.com/vllm-project/aibrix/search?l=go)
  * [Python28%](https://github.com/vllm-project/aibrix/search?l=python)
  * [Jupyter Notebook8%](https://github.com/vllm-project/aibrix/search?l=jupyter-notebook)
  * [TypeScript5.8%](https://github.com/vllm-project/aibrix/search?l=typescript)
  * [Cuda0.5%](https://github.com/vllm-project/aibrix/search?l=cuda)
  * [Shell0.5%](https://github.com/vllm-project/aibrix/search?l=shell)
  * [Other1.3%](https://github.com/vllm-project/aibrix/search?l=Other)


You can’t perform that action at this time. 
