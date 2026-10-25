---
title: 'GitHub - ai-dynamo/nixl: NVIDIA Inference Xfer Library (NIXL) · GitHub'
id: github-ai-dynamonixl-nvidia-inference-xfer-library-nixl-github
tags:
- llmd-kv-cache-routing-c665d5
- nixl
- kv-cache-transfer
- primary-source
created: '2026-09-14T11:59:05.707042Z'
updated: '2026-09-15T04:55:24.112093Z'
source: https://github.com/ai-dynamo/nixl
source_domain: github.com
fetched_at: '2026-09-14T11:59:05.685766Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub repo page for ai-dynamo/nixl (NVIDIA Inference Xfer Library), the
  point-to-point data-transfer library llm-d uses for prefill/decode KV cache transfer
  (e.g. on AWS via EFA/libfabric, per the AWS llm-d blog post). Core README description:
  NIXL "is targeted for accelerating point to point communications in AI inference
  frameworks such as NVIDIA Dynamo, while providing an abstraction over various types
  of memory (e.g., CPU and GPU) and storage (e.g., file, block and object store) through
  a modular plug-in architecture." 1.3k GitHub stars, 441 forks, actively developed
  (1,219+ commits, latest commit Sep 13 2026). Supports a plugin/backend architecture
  (UCX as primary transport, GDS for storage, POSIX, libfabric for EFA, AMD/HIP-ROCm
  support added for MI300X GPUs alongside NVIDIA/CUDA, optional ETCD for distributed
  metadata coordination). Installable via PyPI (pip install nixl, bundles both CUDA
  12 and CUDA 13 backends selected automatically at runtime) or built from source
  with Meson/Ninja (C++20, GCC>=11/Clang>=14, tested against UCX v1.23.x). Documentation
  links to docs/nixl.md (core concepts/architecture), docs/BackendGuide.md (plugin
  development), docs/telemetry.md, and benchmark/nixlbench + benchmark/kvbench for
  performance testing. NOTE: fetched page is dominated by GitHub commit-history/file-listing
  chrome (crawl4ai extraction artifact); the substantive README content is a short
  overview plus build instructions rather than a deep architecture document -- for
  architecture depth, docs/nixl.md and docs/BackendGuide.md (linked but not yet fetched)
  would be the next primary sources to chase.'
---

*Suggested by [[introducing-disaggregated-inference-on-aws-powered-by-llm-d-artificial-intellige]] — NIXL is the point-to-point KV cache transfer library llm-d uses for prefill/decode disaggregation on AWS*

[Skip to content](https://github.com/ai-dynamo/nixl#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/ai-dynamo/nixl) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/ai-dynamo/nixl) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/ai-dynamo/nixl) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/ai-dynamo/nixl).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fai-dynamo%2Fnixl) You must be signed in to change notification settings
  * [ Fork 441 ](https://github.com/login?return_to=%2Fai-dynamo%2Fnixl)
  * [ Star  1.3k ](https://github.com/login?return_to=%2Fai-dynamo%2Fnixl)


[**209** Branches](https://github.com/ai-dynamo/nixl/branches)[](https://github.com/ai-dynamo/nixl/tags)
Go to file
Open more actions menu
## Latest commit
and
[libfabric: request FI_RMA_EVENT on MR for cxi (](https://github.com/ai-dynamo/nixl/commit/e77af99f4521d25a04e3c348eb6ed29adb34ef46)[#2158](https://github.com/ai-dynamo/nixl/pull/2158)[)](https://github.com/ai-dynamo/nixl/commit/e77af99f4521d25a04e3c348eb6ed29adb34ef46)
Open commit detailssuccess
Sep 13, 2026
[e77af99](https://github.com/ai-dynamo/nixl/commit/e77af99f4521d25a04e3c348eb6ed29adb34ef46) · Sep 13, 2026
## History
[1,219 Commits](https://github.com/ai-dynamo/nixl/commits/main/)
Open commit details
1,219 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| [.agents/skills/nixl-python-api](https://github.com/ai-dynamo/nixl/tree/main/.agents/skills/nixl-python-api "This path skips through empty directories")  | [.agents/skills/nixl-python-api](https://github.com/ai-dynamo/nixl/tree/main/.agents/skills/nixl-python-api "This path skips through empty directories")  |  [Add NIXL Python API skill (](https://github.com/ai-dynamo/nixl/commit/1558e5c98c8e7b2520ef1ca7d635e669806092ad "Add NIXL Python API skill \(#1680\)")[#1680](https://github.com/ai-dynamo/nixl/pull/1680)[)](https://github.com/ai-dynamo/nixl/commit/1558e5c98c8e7b2520ef1ca7d635e669806092ad "Add NIXL Python API skill \(#1680\)")  | May 26, 2026  |  
|  [build: bump CUDA and CI base images, stop restating them across CI (](https://github.com/ai-dynamo/nixl/commit/149fbd65a21d293e421f4c33679d17761f10edfc "build: bump CUDA and CI base images, stop restating them across CI \(#2205\)

## What?
**Base image bumps**
- `contrib/Dockerfile`, `benchmark/nixlbench/contrib/Dockerfile`:
`nvcr.io/nvidia/cuda-dl-base` tag `25.10-cuda13.0-devel-ubuntu24.04` →
`26.08-cuda13.4-devel-ubuntu24.04`.
- `.ci/dockerfiles/Dockerfile.base`, `Dockerfile.gpu-test`:
`nvcr.io/nvidia/pytorch` `26.06-py3` → `26.08-py3`.

**Stop restating the pinned image everywhere**
- `benchmark/nixlbench/contrib/build.sh` kept its own copy of the
cuda-dl-base tag as a default — dropped; `BASE_IMAGE`/`BASE_IMAGE_TAG`
now default to empty and each is forwarded only when non-empty.
- `build-container-pr-matrix.yaml` pinned a DLFW PyTorch base for both
targets — dropped, so the PR gate builds on the base we ship.
- Twelve `runs_on_dockers` entries across seven matrix files passed
`--build-arg BASE_IMAGE=<value>` where the value was character-identical
to the target Dockerfile's `ARG` default — dropped.
- The wheel matrix restated five `Dockerfile.manylinux` pins
\(`BASE_IMAGE`, `BASE_TAG`, `CUDA_VERSION`, `MANYLINUX_IMAGE`,
`MANYLINUX_IMAGE_TAG`\) at two call sites — dropped.
- `VLLM_BASE_UPSTREAM`/`SGLANG_BASE_UPSTREAM` overrode the sanity-image
Dockerfile pins — dropped \(see below\).

**Drop versions from CI image names** \(review feedback\), to
`<pipeline-name>-base`:

| before | after |
|---|---|
| `nixl-base-pytorch26.06-cuda13.3-ubuntu24.04` | `nixl-ci-non-gpu-base`
|
| `nixl-base-25.06-cuda12.9-ubuntu24.04` |
`nixl-ci-non-gpu-base-cuda12.9` |
| `nixl-base-13.0.1-ubuntu22.04` | `nixl-ci-non-gpu-base-ubuntu22` |
| `nixl-ci-gpu-base-pytorch26.06-cuda13.3-ubuntu24.04` |
`nixl-ci-gpu-base` |
| `nixl-ci-dl-gpu-base-pytorch26.06-cuda13.3-ubuntu24.04` |
`nixl-ci-dl-gpu-base` |
| `nixl-ci-dl-gpu-ep-base-pytorch26.06-cuda13.3-ubuntu24.04` |
`nixl-ci-dl-gpu-ep-base` |
| `nixl-base-pytorch26.06-cuda13.3-ubuntu24.04-{tsan,asan}` |
`nixl-ci-test-sanitizers-base-{tsan,asan}` |

## Why?
The pinned image was restated in the build scripts and across the CI
matrices, so a bump had to be repeated in several places with nothing to
catch the copies that were missed. Names that advertise their contents
are the same problem in another form: every base bump silently
invalidated eight of them.

That is not hypothetical. Two concrete failures were already in the
tree:

1. **The PR container gate tested a different build path than we ship.**
`contrib/Dockerfile` skips the torch install when the base already
provides torch >= 2.7, and skips the DOCA SDK install when the base
already ships it. On the DLFW PyTorch base the gate was pinned to, both
were skipped — so a break in either step could reach `main` unseen.
2. **A merged upgrade never took effect.** `148eeeba` \("Upgrade vLLM and
SGLang versions used in CI", #2073\) bumped the `ARG BASE_IMAGE` defaults
in `Dockerfile.{vllm,sglang}-base` to vLLM `v0.27.1` / SGLang `v0.5.17`.
But `build-wheel-matrix.yaml` overrode both, and `--build-arg` wins over
`ARG` — so since 2026-08-11 the sanity gate has still been building on
vLLM `v0.23.0` / SGLang `v0.5.13.post1`. Removing the override lets that
upgrade land.

## How?
`build.sh` already gated its `--build-arg` forwarding behind an override
flag, so emptying its defaults is behaviour-preserving for callers that
override. Each of `BASE_IMAGE`/`BASE_IMAGE_TAG` is forwarded
independently only when non-empty, so overriding one still takes the
other from the Dockerfile rather than passing an empty string and
building an invalid `FROM` reference.

For the wheel matrix the `env:` entries had to go along with the flags:
`build-container.sh` reads `BASE_IMAGE` and `CUDA_VERSION` from the
environment \(`BASE_IMAGE=${BASE_IMAGE:-}`\), so leaving them set would
have kept overriding the Dockerfile silently.

Overrides that select a genuinely different image are kept:
`build-matrix.yaml`'s CUDA 12.9 and ubuntu22 variants, and the entries
pointing at an image built earlier in the same job.

**What the pytorch 26.08 bump actually changes**, read from the image
configs: CUDA `13.3.0` → `13.4.1`, torch `2.13.0a0` → `2.14.0a0`, DOCA
`3.4.0` → `3.5.0`. Both DOCA versions are >= 3.4, so `build.sh` keeps
taking the "base already ships DOCA" branch. Torch is still present in
the base, so the system-torch check passes and the PyTorch wheel index
is never consulted — which matters, since that index is derived from
`nvcc` and PyTorch publishes no `cu134` \(verified: `cu130` and `cu132`
exist, `cu131`/`cu133`/`cu134` 404\).

The UCX bundled in the base image \(`1.21.0`\) is unchanged by this bump,
and is not what the test suites exercise: `.gitlab/build.sh` compiles
UCX at `v1.23.x` regardless. For `non-gpu` and `sanitizers` that happens
in the base image itself; for `gpu`, `dl-gpu` and `dl-gpu-ep` the base
build passes `PRE_INSTALLED_UCX_ENV=true` to defer it, and UCX is then
compiled in the `Dockerfile.gpu-test` layer on top \(`ARG
UCX_VERSION=v1.23.x`, overridable via the `UCX_VER` job parameter for
nightly master runs\).

**Scope of behaviour change:** the base bumps and the vLLM/SGLang pins.
Every override removal is a no-op — each removed value equalled the
default it was overriding. The image renames change registry paths, so
the first run rebuilds under the new names.

Note `nixl-ci-non-gpu-base` is now a prefix of its two variant names.
`build-matrix.yaml` selects no containers by name, so this is inert
today, but a `containerSelector` added there later would need anchoring
at both ends.

The new cuda-dl-base tag was checked to exist and to publish both
`amd64` and `arm64` manifests.

Note: the standalone nightly `nixl-ci-build-container` job still carries
its own `BASE_IMAGE_TAG` job-parameter default
\(`25.10-cuda13.0-devel-ubuntu24.04`\) and is intentionally left alone
here; it is tracked separately.

---------

Signed-off-by: NirWolfer <nwolfer@nvidia.com>
Co-authored-by: ovidiusm <ovidium@nvidia.com>")[#…](https://github.com/ai-dynamo/nixl/pull/2205)  | Sep 10, 2026  |  
|   |   |  [ci: exclude organization members from external label (](https://github.com/ai-dynamo/nixl/commit/8585eee0b2a37be52bb5c727f0d21e714518b689 "ci: exclude organization members from external label \(#2201\)

Skip the external-contributor workflow for ai-dynamo organization
members and document the updated behavior.

<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->
## Summary by CodeRabbit

* **Bug Fixes**
* External contributor reminders now run for pull requests authored by
users who are not repository members.
* Eligibility no longer depends on whether a pull request comes from
another repository.

* **Documentation**
* Clarified that the External Contributor workflow adds the
`external-contribution` label and applies to fork pull requests authored
by users outside the organization.
<!-- end of auto-generated comment: release notes by coderabbit.ai -->

---------

Signed-off-by: Anant Sharma <anants@nvidia.com>
Co-authored-by: Mikhail Brinskiy <brminich@users.noreply.github.com>")[#2201](https://github.com/ai-dynamo/nixl/pull/2201)[)](https://github.com/ai-dynamo/nixl/commit/8585eee0b2a37be52bb5c727f0d21e714518b689 "ci: exclude organization members from external label \(#2201\)

Skip the external-contributor workflow for ai-dynamo organization
members and document the updated behavior.

<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->
## Summary by CodeRabbit

* **Bug Fixes**
* External contributor reminders now run for pull requests authored by
users who are not repository members.
* Eligibility no longer depends on whether a pull request comes from
another repository.

* **Documentation**
* Clarified that the External Contributor workflow adds the
`external-contribution` label and applies to fork pull requests authored
by users outside the organization.
<!-- end of auto-generated comment: release notes by coderabbit.ai -->

---------

Signed-off-by: Anant Sharma <anants@nvidia.com>
Co-authored-by: Mikhail Brinskiy <brminich@users.noreply.github.com>")  | Sep 8, 2026  |  
|   |   |  [fix(posix): recover AIO after I/O failures (](https://github.com/ai-dynamo/nixl/commit/adf179a54a0951193bff0ab90201dfc605ed5d5c "fix\(posix\): recover AIO after I/O failures \(#2085\)

Fix AIO handling of I/O errors. Right now we fail fast with leaving stale IOs, this PR fixes that by correctly cancelling and delaying completion until all IOs have completed \(succeeded or cancelled\). Follows closely the uring implementation.

Fixes #1696 

Signed-off-by: Lukas Humbel <lhumbel@nvidia.com>
Co-authored-by: Vishwanath Venkatesan <vvenkates27@users.noreply.github.com>")[#2085](https://github.com/ai-dynamo/nixl/pull/2085)[)](https://github.com/ai-dynamo/nixl/commit/adf179a54a0951193bff0ab90201dfc605ed5d5c "fix\(posix\): recover AIO after I/O failures \(#2085\)

Fix AIO handling of I/O errors. Right now we fail fast with leaving stale IOs, this PR fixes that by correctly cancelling and delaying completion until all IOs have completed \(succeeded or cancelled\). Follows closely the uring implementation.

Fixes #1696 

Signed-off-by: Lukas Humbel <lhumbel@nvidia.com>
Co-authored-by: Vishwanath Venkatesan <vvenkates27@users.noreply.github.com>")  | Sep 2, 2026  |  
|   |   |  [build: bump CUDA and CI base images, stop restating them across CI (](https://github.com/ai-dynamo/nixl/commit/149fbd65a21d293e421f4c33679d17761f10edfc "build: bump CUDA and CI base images, stop restating them across CI \(#2205\)

## What?
**Base image bumps**
- `contrib/Dockerfile`, `benchmark/nixlbench/contrib/Dockerfile`:
`nvcr.io/nvidia/cuda-dl-base` tag `25.10-cuda13.0-devel-ubuntu24.04` →
`26.08-cuda13.4-devel-ubuntu24.04`.
- `.ci/dockerfiles/Dockerfile.base`, `Dockerfile.gpu-test`:
`nvcr.io/nvidia/pytorch` `26.06-py3` → `26.08-py3`.

**Stop restating the pinned image everywhere**
- `benchmark/nixlbench/contrib/build.sh` kept its own copy of the
cuda-dl-base tag as a default — dropped; `BASE_IMAGE`/`BASE_IMAGE_TAG`
now default to empty and each is forwarded only when non-empty.
- `build-container-pr-matrix.yaml` pinned a DLFW PyTorch base for both
targets — dropped, so the PR gate builds on the base we ship.
- Twelve `runs_on_dockers` entries across seven matrix files passed
`--build-arg BASE_IMAGE=<value>` where the value was character-identical
to the target Dockerfile's `ARG` default — dropped.
- The wheel matrix restated five `Dockerfile.manylinux` pins
\(`BASE_IMAGE`, `BASE_TAG`, `CUDA_VERSION`, `MANYLINUX_IMAGE`,
`MANYLINUX_IMAGE_TAG`\) at two call sites — dropped.
- `VLLM_BASE_UPSTREAM`/`SGLANG_BASE_UPSTREAM` overrode the sanity-image
Dockerfile pins — dropped \(see below\).

**Drop versions from CI image names** \(review feedback\), to
`<pipeline-name>-base`:

| before | after |
|---|---|
| `nixl-base-pytorch26.06-cuda13.3-ubuntu24.04` | `nixl-ci-non-gpu-base`
|
| `nixl-base-25.06-cuda12.9-ubuntu24.04` |
`nixl-ci-non-gpu-base-cuda12.9` |
| `nixl-base-13.0.1-ubuntu22.04` | `nixl-ci-non-gpu-base-ubuntu22` |
| `nixl-ci-gpu-base-pytorch26.06-cuda13.3-ubuntu24.04` |
`nixl-ci-gpu-base` |
| `nixl-ci-dl-gpu-base-pytorch26.06-cuda13.3-ubuntu24.04` |
`nixl-ci-dl-gpu-base` |
| `nixl-ci-dl-gpu-ep-base-pytorch26.06-cuda13.3-ubuntu24.04` |
`nixl-ci-dl-gpu-ep-base` |
| `nixl-base-pytorch26.06-cuda13.3-ubuntu24.04-{tsan,asan}` |
`nixl-ci-test-sanitizers-base-{tsan,asan}` |

## Why?
The pinned image was restated in the build scripts and across the CI
matrices, so a bump had to be repeated in several places with nothing to
catch the copies that were missed. Names that advertise their contents
are the same problem in another form: every base bump silently
invalidated eight of them.

That is not hypothetical. Two concrete failures were already in the
tree:

1. **The PR container gate tested a different build path than we ship.**
`contrib/Dockerfile` skips the torch install when the base already
provides torch >= 2.7, and skips the DOCA SDK install when the base
already ships it. On the DLFW PyTorch base the gate was pinned to, both
were skipped — so a break in either step could reach `main` unseen.
2. **A merged upgrade never took effect.** `148eeeba` \("Upgrade vLLM and
SGLang versions used in CI", #2073\) bumped the `ARG BASE_IMAGE` defaults
in `Dockerfile.{vllm,sglang}-base` to vLLM `v0.27.1` / SGLang `v0.5.17`.
But `build-wheel-matrix.yaml` overrode both, and `--build-arg` wins over
`ARG` — so since 2026-08-11 the sanity gate has still been building on
vLLM `v0.23.0` / SGLang `v0.5.13.post1`. Removing the override lets that
upgrade land.

## How?
`build.sh` already gated its `--build-arg` forwarding behind an override
flag, so emptying its defaults is behaviour-preserving for callers that
override. Each of `BASE_IMAGE`/`BASE_IMAGE_TAG` is forwarded
independently only when non-empty, so overriding one still takes the
other from the Dockerfile rather than passing an empty string and
building an invalid `FROM` reference.

For the wheel matrix the `env:` entries had to go along with the flags:
`build-container.sh` reads `BASE_IMAGE` and `CUDA_VERSION` from the
environment \(`BASE_IMAGE=${BASE_IMAGE:-}`\), so leaving them set would
have kept overriding the Dockerfile silently.

Overrides that select a genuinely different image are kept:
`build-matrix.yaml`'s CUDA 12.9 and ubuntu22 variants, and the entries
pointing at an image built earlier in the same job.

**What the pytorch 26.08 bump actually changes**, read from the image
configs: CUDA `13.3.0` → `13.4.1`, torch `2.13.0a0` → `2.14.0a0`, DOCA
`3.4.0` → `3.5.0`. Both DOCA versions are >= 3.4, so `build.sh` keeps
taking the "base already ships DOCA" branch. Torch is still present in
the base, so the system-torch check passes and the PyTorch wheel index
is never consulted — which matters, since that index is derived from
`nvcc` and PyTorch publishes no `cu134` \(verified: `cu130` and `cu132`
exist, `cu131`/`cu133`/`cu134` 404\).

The UCX bundled in the base image \(`1.21.0`\) is unchanged by this bump,
and is not what the test suites exercise: `.gitlab/build.sh` compiles
UCX at `v1.23.x` regardless. For `non-gpu` and `sanitizers` that happens
in the base image itself; for `gpu`, `dl-gpu` and `dl-gpu-ep` the base
build passes `PRE_INSTALLED_UCX_ENV=true` to defer it, and UCX is then
compiled in the `Dockerfile.gpu-test` layer on top \(`ARG
UCX_VERSION=v1.23.x`, overridable via the `UCX_VER` job parameter for
nightly master runs\).

**Scope of behaviour change:** the base bumps and the vLLM/SGLang pins.
Every override removal is a no-op — each removed value equalled the
default it was overriding. The image renames change registry paths, so
the first run rebuilds under the new names.

Note `nixl-ci-non-gpu-base` is now a prefix of its two variant names.
`build-matrix.yaml` selects no containers by name, so this is inert
today, but a `containerSelector` added there later would need anchoring
at both ends.

The new cuda-dl-base tag was checked to exist and to publish both
`amd64` and `arm64` manifests.

Note: the standalone nightly `nixl-ci-build-container` job still carries
its own `BASE_IMAGE_TAG` job-parameter default
\(`25.10-cuda13.0-devel-ubuntu24.04`\) and is intentionally left alone
here; it is tracked separately.

---------

Signed-off-by: NirWolfer <nwolfer@nvidia.com>
Co-authored-by: ovidiusm <ovidium@nvidia.com>")[#…](https://github.com/ai-dynamo/nixl/pull/2205)  | Sep 10, 2026  |  
|   |   |  [build: bump CUDA and CI base images, stop restating them across CI (](https://github.com/ai-dynamo/nixl/commit/149fbd65a21d293e421f4c33679d17761f10edfc "build: bump CUDA and CI base images, stop restating them across CI \(#2205\)

## What?
**Base image bumps**
- `contrib/Dockerfile`, `benchmark/nixlbench/contrib/Dockerfile`:
`nvcr.io/nvidia/cuda-dl-base` tag `25.10-cuda13.0-devel-ubuntu24.04` →
`26.08-cuda13.4-devel-ubuntu24.04`.
- `.ci/dockerfiles/Dockerfile.base`, `Dockerfile.gpu-test`:
`nvcr.io/nvidia/pytorch` `26.06-py3` → `26.08-py3`.

**Stop restating the pinned image everywhere**
- `benchmark/nixlbench/contrib/build.sh` kept its own copy of the
cuda-dl-base tag as a default — dropped; `BASE_IMAGE`/`BASE_IMAGE_TAG`
now default to empty and each is forwarded only when non-empty.
- `build-container-pr-matrix.yaml` pinned a DLFW PyTorch base for both
targets — dropped, so the PR gate builds on the base we ship.
- Twelve `runs_on_dockers` entries across seven matrix files passed
`--build-arg BASE_IMAGE=<value>` where the value was character-identical
to the target Dockerfile's `ARG` default — dropped.
- The wheel matrix restated five `Dockerfile.manylinux` pins
\(`BASE_IMAGE`, `BASE_TAG`, `CUDA_VERSION`, `MANYLINUX_IMAGE`,
`MANYLINUX_IMAGE_TAG`\) at two call sites — dropped.
- `VLLM_BASE_UPSTREAM`/`SGLANG_BASE_UPSTREAM` overrode the sanity-image
Dockerfile pins — dropped \(see below\).

**Drop versions from CI image names** \(review feedback\), to
`<pipeline-name>-base`:

| before | after |
|---|---|
| `nixl-base-pytorch26.06-cuda13.3-ubuntu24.04` | `nixl-ci-non-gpu-base`
|
| `nixl-base-25.06-cuda12.9-ubuntu24.04` |
`nixl-ci-non-gpu-base-cuda12.9` |
| `nixl-base-13.0.1-ubuntu22.04` | `nixl-ci-non-gpu-base-ubuntu22` |
| `nixl-ci-gpu-base-pytorch26.06-cuda13.3-ubuntu24.04` |
`nixl-ci-gpu-base` |
| `nixl-ci-dl-gpu-base-pytorch26.06-cuda13.3-ubuntu24.04` |
`nixl-ci-dl-gpu-base` |
| `nixl-ci-dl-gpu-ep-base-pytorch26.06-cuda13.3-ubuntu24.04` |
`nixl-ci-dl-gpu-ep-base` |
| `nixl-base-pytorch26.06-cuda13.3-ubuntu24.04-{tsan,asan}` |
`nixl-ci-test-sanitizers-base-{tsan,asan}` |

## Why?
The pinned image was restated in the build scripts and across the CI
matrices, so a bump had to be repeated in several places with nothing to
catch the copies that were missed. Names that advertise their contents
are the same problem in another form: every base bump silently
invalidated eight of them.

That is not hypothetical. Two concrete failures were already in the
tree:

1. **The PR container gate tested a different build path than we ship.**
`contrib/Dockerfile` skips the torch install when the base already
provides torch >= 2.7, and skips the DOCA SDK install when the base
already ships it. On the DLFW PyTorch base the gate was pinned to, both
were skipped — so a break in either step could reach `main` unseen.
2. **A merged upgrade never took effect.** `148eeeba` \("Upgrade vLLM and
SGLang versions used in CI", #2073\) bumped the `ARG BASE_IMAGE` defaults
in `Dockerfile.{vllm,sglang}-base` to vLLM `v0.27.1` / SGLang `v0.5.17`.
But `build-wheel-matrix.yaml` overrode both, and `--build-arg` wins over
`ARG` — so since 2026-08-11 the sanity gate has still been building on
vLLM `v0.23.0` / SGLang `v0.5.13.post1`. Removing the override lets that
upgrade land.

## How?
`build.sh` already gated its `--build-arg` forwarding behind an override
flag, so emptying its defaults is behaviour-preserving for callers that
override. Each of `BASE_IMAGE`/`BASE_IMAGE_TAG` is forwarded
independently only when non-empty, so overriding one still takes the
other from the Dockerfile rather than passing an empty string and
building an invalid `FROM` reference.

For the wheel matrix the `env:` entries had to go along with the flags:
`build-container.sh` reads `BASE_IMAGE` and `CUDA_VERSION` from the
environment \(`BASE_IMAGE=${BASE_IMAGE:-}`\), so leaving them set would
have kept overriding the Dockerfile silently.

Overrides that select a genuinely different image are kept:
`build-matrix.yaml`'s CUDA 12.9 and ubuntu22 variants, and the entries
pointing at an image built earlier in the same job.

**What the pytorch 26.08 bump actually changes**, read from the image
configs: CUDA `13.3.0` → `13.4.1`, torch `2.13.0a0` → `2.14.0a0`, DOCA
`3.4.0` → `3.5.0`. Both DOCA versions are >= 3.4, so `build.sh` keeps
taking the "base already ships DOCA" branch. Torch is still present in
the base, so the system-torch check passes and the PyTorch wheel index
is never consulted — which matters, since that index is derived from
`nvcc` and PyTorch publishes no `cu134` \(verified: `cu130` and `cu132`
exist, `cu131`/`cu133`/`cu134` 404\).

The UCX bundled in the base image \(`1.21.0`\) is unchanged by this bump,
and is not what the test suites exercise: `.gitlab/build.sh` compiles
UCX at `v1.23.x` regardless. For `non-gpu` and `sanitizers` that happens
in the base image itself; for `gpu`, `dl-gpu` and `dl-gpu-ep` the base
build passes `PRE_INSTALLED_UCX_ENV=true` to defer it, and UCX is then
compiled in the `Dockerfile.gpu-test` layer on top \(`ARG
UCX_VERSION=v1.23.x`, overridable via the `UCX_VER` job parameter for
nightly master runs\).

**Scope of behaviour change:** the base bumps and the vLLM/SGLang pins.
Every override removal is a no-op — each removed value equalled the
default it was overriding. The image renames change registry paths, so
the first run rebuilds under the new names.

Note `nixl-ci-non-gpu-base` is now a prefix of its two variant names.
`build-matrix.yaml` selects no containers by name, so this is inert
today, but a `containerSelector` added there later would need anchoring
at both ends.

The new cuda-dl-base tag was checked to exist and to publish both
`amd64` and `arm64` manifests.

Note: the standalone nightly `nixl-ci-build-container` job still carries
its own `BASE_IMAGE_TAG` job-parameter default
\(`25.10-cuda13.0-devel-ubuntu24.04`\) and is intentionally left alone
here; it is tracked separately.

---------

Signed-off-by: NirWolfer <nwolfer@nvidia.com>
Co-authored-by: ovidiusm <ovidium@nvidia.com>")[#…](https://github.com/ai-dynamo/nixl/pull/2205)  | Sep 10, 2026  |  
|  [DOCS: Document UCX backend initialization options (](https://github.com/ai-dynamo/nixl/commit/b8bbfdb62a22dc4d1aab61d2b8db8ead17730fb7 "DOCS: Document UCX backend initialization options \(#2050\)

## What?

Documents the UCX plugin's initialization options and the generic
mechanism for
overriding backend init parameters.

- `docs/BackendGuide.md`: new `#### UCX backend initialization options`
under
"Create transfer backends", where parameter passing is already
described. Adds
the option table for `ucx_error_handling_mode` \(default `peer`,
alternative
`none`\), then explains the lane-selection consequence and the tradeoff
of
  each value.
- `docs/python_api.md`: new `## Backend initialization parameters`
section with
the `get_plugin_params` -> override -> `create_backend` snippet,
cross-linked
  to the backend guide.

Documentation only. No functional change.

## Why?

`ucx_error_handling_mode` is exposed through
`getPluginParams`/`get_plugin_params`
but was undocumented, and its effect is easy to misread as "error
reporting only".
It also affects UCP transport lane selection: NIXL sets
`ep_params.err_mode` from
this option, and UCP only selects lanes whose transport advertises peer
failure
support. A transport that does not advertise it is excluded from `peer`
endpoints
even when it is available and faster.

ROCm IPC is the concrete case. `rocm_ipc` in UCX 1.22 reports
`error handling: none`, so it is not selected for NIXL's default `peer`
endpoints.
This is a contributing factor to the transport-selection behavior
reported in
#2039, where large ROCm VRAM RMA falls back to `software emulation |
tcp/bond0`
instead of `rocm_ipc`.

The docs deliberately do not present `none` as the fix. It restores lane
eligibility but disables the peer failure detection NIXL relies on to
move an
endpoint to its failed state and report `NIXL_ERR_REMOTE_DISCONNECT`, so
it
trades remote failure visibility for transport availability. The
guidance is to
fix lane eligibility in the transport where remote failure reporting is
needed.

Related: #2039


<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->
## Summary by CodeRabbit

* **Documentation**
* Added guidance for configuring UCX backend initialization parameters
through C++ and Python APIs.
* Documented how to retrieve defaults, override settings, and create the
backend.
* Explained `ucx_error_handling_mode` options, including their effects
on transport selection and remote failure reporting.
* Added references to supported UCX configuration options and clarified
behavior for peer and disabled error-handling modes.
* Clarified how error-handling settings influence available transport
lanes and failure notifications.
<!-- end of auto-generated comment: release notes by coderabbit.ai -->

Signed-off-by: Yaser Afshar <yaser.afshar@intel.com>")[#2050](https://github.com/ai-dynamo/nixl/pull/2050)[)](https://github.com/ai-dynamo/nixl/commit/b8bbfdb62a22dc4d1aab61d2b8db8ead17730fb7 "DOCS: Document UCX backend initialization options \(#2050\)

## What?

Documents the UCX plugin's initialization options and the generic
mechanism for
overriding backend init parameters.

- `docs/BackendGuide.md`: new `#### UCX backend initialization options`
under
"Create transfer backends", where parameter passing is already
described. Adds
the option table for `ucx_error_handling_mode` \(default `peer`,
alternative
`none`\), then explains the lane-selection consequence and the tradeoff
of
  each value.
- `docs/python_api.md`: new `## Backend initialization parameters`
section with
the `get_plugin_params` -> override -> `create_backend` snippet,
cross-linked
  to the backend guide.

Documentation only. No functional change.

## Why?

`ucx_error_handling_mode` is exposed through
`getPluginParams`/`get_plugin_params`
but was undocumented, and its effect is easy to misread as "error
reporting only".
It also affects UCP transport lane selection: NIXL sets
`ep_params.err_mode` from
this option, and UCP only selects lanes whose transport advertises peer
failure
support. A transport that does not advertise it is excluded from `peer`
endpoints
even when it is available and faster.

ROCm IPC is the concrete case. `rocm_ipc` in UCX 1.22 reports
`error handling: none`, so it is not selected for NIXL's default `peer`
endpoints.
This is a contributing factor to the transport-selection behavior
reported in
#2039, where large ROCm VRAM RMA falls back to `software emulation |
tcp/bond0`
instead of `rocm_ipc`.

The docs deliberately do not present `none` as the fix. It restores lane
eligibility but disables the peer failure detection NIXL relies on to
move an
endpoint to its failed state and report `NIXL_ERR_REMOTE_DISCONNECT`, so
it
trades remote failure visibility for transport availability. The
guidance is to
fix lane eligibility in the transport where remote failure reporting is
needed.

Related: #2039


<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->
## Summary by CodeRabbit

* **Documentation**
* Added guidance for configuring UCX backend initialization parameters
through C++ and Python APIs.
* Documented how to retrieve defaults, override settings, and create the
backend.
* Explained `ucx_error_handling_mode` options, including their effects
on transport selection and remote failure reporting.
* Added references to supported UCX configuration options and clarified
behavior for peer and disabled error-handling modes.
* Clarified how error-handling settings influence available transport
lanes and failure notifications.
<!-- end of auto-generated comment: release notes by coderabbit.ai -->

Signed-off-by: Yaser Afshar <yaser.afshar@intel.com>")  | Aug 13, 2026  |  
|   |   |  [device api: Layer UCX device API from GPU API (](https://github.com/ai-dynamo/nixl/commit/6c352ce2e6396d5f7179e96f5f3bd4eac88b30c4 "device api: Layer UCX device API from GPU API \(#2147\)

## What?
Introduce a backend-neutral GPU Device API and migrate existing
consumers from the UCX-specific header to the new generic interface.

## Why?
Decouple the public GPU API from UCX, enabling support for additional
backends and execution modes \(such as CPU Proxy\) and cleaner behavior
when UCX GPU support is unavailable.

## How?
Split common types, the public API, and the UCX implementation into
separate layers, using build-time configuration to select the backend.
Remove the legacy UCX header and update build, install, test, benchmark,
and example references.

<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->
## Summary by CodeRabbit

- **New Features**
- Added a unified GPU device API for transfer-status checks, memory
transfers, atomic additions, and remote pointer access.
- Added support for GPU execution levels, deferred operations,
transfer-status tracking, and memory-view elements.
- Added UCX-backed implementations with graceful fallback when UCX GPU
support is unavailable.

- **Build Improvements**
- Added configuration and installation support for GPU device API
headers.
- Updated GPU device API integration across benchmarks, examples, and
tests for more consistent builds.
<!-- end of auto-generated comment: release notes by coderabbit.ai -->

---------

Signed-off-by: Tomer Davidor <tdavidor@nvidia.com>
Co-authored-by: hbadihi <hbadihi@nvidia.com>")[#2147](https://github.com/ai-dynamo/nixl/pull/2147)[)](https://github.com/ai-dynamo/nixl/commit/6c352ce2e6396d5f7179e96f5f3bd4eac88b30c4 "device api: Layer UCX device API from GPU API \(#2147\)

## What?
Introduce a backend-neutral GPU Device API and migrate existing
consumers from the UCX-specific header to the new generic interface.

## Why?
Decouple the public GPU API from UCX, enabling support for additional
backends and execution modes \(such as CPU Proxy\) and cleaner behavior
when UCX GPU support is unavailable.

## How?
Split common types, the public API, and the UCX implementation into
separate layers, using build-time configuration to select the backend.
Remove the legacy UCX header and update build, install, test, benchmark,
and example references.

<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->
## Summary by CodeRabbit

- **New Features**
- Added a unified GPU device API for transfer-status checks, memory
transfers, atomic additions, and remote pointer access.
- Added support for GPU execution levels, deferred operations,
transfer-status tracking, and memory-view elements.
- Added UCX-backed implementations with graceful fallback when UCX GPU
support is unavailable.

- **Build Improvements**
- Added configuration and installation support for GPU device API
headers.
- Updated GPU device API integration across benchmarks, examples, and
tests for more consistent builds.
<!-- end of auto-generated comment: release notes by coderabbit.ai -->

---------

Signed-off-by: Tomer Davidor <tdavidor@nvidia.com>
Co-authored-by: hbadihi <hbadihi@nvidia.com>")  | Sep 7, 2026  |  
|  [docs: add static Fern documentation pages (](https://github.com/ai-dynamo/nixl/commit/f71a468f6790c1c519db37f7508a5dca2dc98976 "docs: add static Fern documentation pages \(#2099\)

Add the static fern documentation pages. This is part of series of fern NIXL documentation and the first one that adds some content. 

The content added:
* Added an interactive NIXL software stack diagram with responsive layouts, dark mode, and linked hover highlighting.
* Added Getting Started guides for installation, architecture, and quick-start workflows in Python, C++, and Rust.
* Added backend guides for networking, storage, GPU, file-transfer, and object-storage options, including INFINIA.
* Added build, benchmarking, telemetry, metadata exchange, environment-variable, troubleshooting, and contribution documentation.

Signed-off-by: Lukas Humbel <lhumbel@nvidia.com>")[#2099](https://github.com/ai-dynamo/nixl/pull/2099)[)](https://github.com/ai-dynamo/nixl/commit/f71a468f6790c1c519db37f7508a5dca2dc98976 "docs: add static Fern documentation pages \(#2099\)

Add the static fern documentation pages. This is part of series of fern NIXL documentation and the first one that adds some content. 

The content added:
* Added an interactive NIXL software stack diagram with responsive layouts, dark mode, and linked hover highlighting.
* Added Getting Started guides for installation, architecture, and quick-start workflows in Python, C++, and Rust.
* Added backend guides for networking, storage, GPU, file-transfer, and object-storage options, including INFINIA.
* Added build, benchmarking, telemetry, metadata exchange, environment-variable, troubleshooting, and contribution documentation.

Signed-off-by: Lukas Humbel <lhumbel@nvidia.com>")  | Aug 26, 2026  |  
|   |   |   | Jul 22, 2026  |  
|  [libfabric: request FI_RMA_EVENT on MR for cxi (](https://github.com/ai-dynamo/nixl/commit/e77af99f4521d25a04e3c348eb6ed29adb34ef46 "libfabric: request FI_RMA_EVENT on MR for cxi \(#2158\)

registerMemory\(\) registered the destination MR with only FI_REMOTE_WRITE
| FI_REMOTE_READ for all non-tcp/sockets providers.

CXI only generates a target-side remote-write completion event when the
destination MR key has events enabled \(FI_RMA_EVENT\). NIXL's libfabric
backend protocol depends on that target-side completion \(in combination
with the SEND/RECV notification\) to know a transfer has completed, so
CXI destination MRs must be registered with FI_RMA_EVENT.

Fix by adding a cxi-specific branch in registerMemory\(\) that includes
FI_RMA_EVENT in provider_access_flags.

<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->
## Summary by CodeRabbit

* **Bug Fixes**
  * Improved memory registration for CXI-based networking.
* Preserved remote read and write access while enabling required remote
memory events.
* Other supported networking providers retain their existing memory
access behavior.
<!-- end of auto-generated comment: release notes by coderabbit.ai -->

Signed-off-by: Ryan Hankins <ryan.hankins@hpe.com>
Co-authored-by: Adit Ranadive <aranadive@nvidia.com>")[#2158](https://github.com/ai-dynamo/nixl/pull/2158)[)](https://github.com/ai-dynamo/nixl/commit/e77af99f4521d25a04e3c348eb6ed29adb34ef46 "libfabric: request FI_RMA_EVENT on MR for cxi \(#2158\)

registerMemory\(\) registered the destination MR with only FI_REMOTE_WRITE
| FI_REMOTE_READ for all non-tcp/sockets providers.

CXI only generates a target-side remote-write completion event when the
destination MR key has events enabled \(FI_RMA_EVENT\). NIXL's libfabric
backend protocol depends on that target-side completion \(in combination
with the SEND/RECV notification\) to know a transfer has completed, so
CXI destination MRs must be registered with FI_RMA_EVENT.

Fix by adding a cxi-specific branch in registerMemory\(\) that includes
FI_RMA_EVENT in provider_access_flags.

<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->
## Summary by CodeRabbit

* **Bug Fixes**
  * Improved memory registration for CXI-based networking.
* Preserved remote read and write access while enabling required remote
memory events.
* Other supported networking providers retain their existing memory
access behavior.
<!-- end of auto-generated comment: release notes by coderabbit.ai -->

Signed-off-by: Ryan Hankins <ryan.hankins@hpe.com>
Co-authored-by: Adit Ranadive <aranadive@nvidia.com>")  | Sep 13, 2026  |  
|   |   |  [Use tagged Taskflow git wrap for NIXL builds (](https://github.com/ai-dynamo/nixl/commit/76275cfca1a0645b9acfb2788880258c6eb06411 "Use tagged Taskflow git wrap for NIXL builds \(#2121\)

## What

Switch the Taskflow fallback dependency in `subprojects/taskflow.wrap`
from a
  hash-pinned GitHub source archive to a tag-pinned Meson `\[wrap-git\]`
  configuration.

The dependency now uses:

 ```ini
  \[wrap-git\]
  url = https://github.com/taskflow/taskflow.git
  revision = v3.10.0
  depth = 1
  patch_directory = taskflow
```
  ## Why

  The previous configuration downloaded GitHub's automatically generated
  Taskflow v3.10.0 archive and verified it with a SHA256 checksum.

  The archive produced different checksums at different times, causing Meson
  configuration to fail with an Incorrect hash for source error. Updating the
  checksum would only address the currently served archive and could break again
  if the generated archive changes.

  Using the Taskflow v3.10.0 release tag through Meson's \[wrap-git\] support
  avoids relying on the byte-level stability of the generated tarball while
  keeping the dependency pinned to the same Taskflow release.

  The shallow clone keeps the download limited to the selected release tag.

  ## Validation

  - Confirmed that the upstream Taskflow repository provides the v3.10.0 tag.
  - Confirmed that the configuration is compatible with the project's required
    Meson version.

Co-authored-by: Adit Ranadive <aranadive@nvidia.com>")[#2121](https://github.com/ai-dynamo/nixl/pull/2121)[)](https://github.com/ai-dynamo/nixl/commit/76275cfca1a0645b9acfb2788880258c6eb06411 "Use tagged Taskflow git wrap for NIXL builds \(#2121\)

## What

Switch the Taskflow fallback dependency in `subprojects/taskflow.wrap`
from a
  hash-pinned GitHub source archive to a tag-pinned Meson `\[wrap-git\]`
  configuration.

The dependency now uses:

 ```ini
  \[wrap-git\]
  url = https://github.com/taskflow/taskflow.git
  revision = v3.10.0
  depth = 1
  patch_directory = taskflow
```
  ## Why

  The previous configuration downloaded GitHub's automatically generated
  Taskflow v3.10.0 archive and verified it with a SHA256 checksum.

  The archive produced different checksums at different times, causing Meson
  configuration to fail with an Incorrect hash for source error. Updating the
  checksum would only address the currently served archive and could break again
  if the generated archive changes.

  Using the Taskflow v3.10.0 release tag through Meson's \[wrap-git\] support
  avoids relying on the byte-level stability of the generated tarball while
  keeping the dependency pinned to the same Taskflow release.

  The shallow clone keeps the download limited to the selected release tag.

  ## Validation

  - Confirmed that the upstream Taskflow repository provides the v3.10.0 tag.
  - Confirmed that the configuration is compatible with the project's required
    Meson version.

Co-authored-by: Adit Ranadive <aranadive@nvidia.com>")  | Aug 27, 2026  |  
|  [utils: add shared S3-over-RDMA transport and protocol helpers (](https://github.com/ai-dynamo/nixl/commit/992b0a8c7f91f5ed043c22e30e6ca1bfb36c06e6 "utils: add shared S3-over-RDMA transport and protocol helpers \(#1840\)

## What

Adds the shared GPU-direct S3-over-RDMA infrastructure as reusable
utilities under `src/utils/object/`, so object engines compose it rather
than each embedding its own copy. This keeps the transport/protocol
layer in one place and easy to maintain as more engines adopt it.
Additive only — no existing code paths change.

## Contents

- **`rdma_protocol.h`** — dependency-free wire helpers \(token
formatting, reply parsing, constants\), unit-tested.
- **`rdma.{h,cpp}`** — `SharedCuObjClient` \(process-wide cuObject RDMA
plumbing\) and `S3RdmaControlPlane` \(SigV4 `UNSIGNED-PAYLOAD` control
plane carrying `x-amz-rdma-token`, reading `x-amz-rdma-reply` /
`x-amz-rdma-bytes-transferred`\) plus retry wrappers. Guarded by
`HAVE_CUOBJ_CLIENT`; compiles to nothing without cuObjClient.
- Built into the `obj_utils` library; `RdmaProtocol` unit test under
`test/gtest/unit/obj`.

The header names and token layout follow NVIDIA's published \[cuObject
docs\]\(https://docs.nvidia.com/gpudirect-storage/cuobject/index.html\) and
the protocol proposed to AWS \(\[aws-c-s3
`RDMA_PROTOCOL_SPEC.md`\]\(https://github.com/KiranModukuri/aws-c-s3/blob/nvidia_rdma/RDMA_PROTOCOL_SPEC.md\)\).

## Notes

- `formatRdmaToken` uses `PRIx64`; `rdmaGet` rejects `size==0`;
credentials are resolved once at construction \(documented limitation:
rotating IAM session tokens need a fresh backend\).
- `SharedCuObjClient` has no S3 dependency, so it can move under
`src/utils/cuobj/` if preferred — happy to do that.

## Test

Builds into `obj_utils`; `RdmaProtocol.*` unit tests pass \(7/7\).

## Types of changes
- \[x\] New feature \(non-breaking — additive utilities\)

<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->
## Summary by CodeRabbit

* **New Features**
* Added accelerated S3 transfers over RDMA for supported hardware and
configurations.
* Added RDMA support for uploads, downloads, ranged reads, checksums,
and multipart operations.
* Added reliable retry handling and clear fallback behavior when RDMA is
unavailable.
* Added strict validation for RDMA transfer responses and protocol
tokens.

* **Bug Fixes**
* Improved S3 configuration handling by treating backend parameters as
read-only.
* Aligned build and linkage requirements for RDMA-enabled S3
functionality.

* **Tests**
* Added comprehensive coverage for RDMA token formatting, response
parsing, and protocol constants.
<!-- end of auto-generated comment: release notes by coderabbit.ai -->

---------

Signed-off-by: Harshavardhana <harsha@minio.io>
Co-authored-by: Adit Ranadive <aranadive@nvidia.com>
Co-authored-by: Vishwanath Venkatesan <vvenkates27@users.noreply.github.com>")[#1840](https://github.com/ai-dynamo/nixl/pull/1840)[)](https://github.com/ai-dynamo/nixl/commit/992b0a8c7f91f5ed043c22e30e6ca1bfb36c06e6 "utils: add shared S3-over-RDMA transport and protocol helpers \(#1840\)

## What

Adds the shared GPU-direct S3-over-RDMA infrastructure as reusable
utilities under `src/utils/object/`, so object engines compose it rather
than each embedding its own copy. This keeps the transport/protocol
layer in one place and easy to maintain as more engines adopt it.
Additive only — no existing code paths change.

## Contents

- **`rdma_protocol.h`** — dependency-free wire helpers \(token
formatting, reply parsing, constants\), unit-tested.
- **`rdma.{h,cpp}`** — `SharedCuObjClient` \(process-wide cuObject RDMA
plumbing\) and `S3RdmaControlPlane` \(SigV4 `UNSIGNED-PAYLOAD` control
plane carrying `x-amz-rdma-token`, reading `x-amz-rdma-reply` /
`x-amz-rdma-bytes-transferred`\) plus retry wrappers. Guarded by
`HAVE_CUOBJ_CLIENT`; compiles to nothing without cuObjClient.
- Built into the `obj_utils` library; `RdmaProtocol` unit test under
`test/gtest/unit/obj`.

The header names and token layout follow NVIDIA's published \[cuObject
docs\]\(https://docs.nvidia.com/gpudirect-storage/cuobject/index.html\) and
the protocol proposed to AWS \(\[aws-c-s3
`RDMA_PROTOCOL_SPEC.md`\]\(https://github.com/KiranModukuri/aws-c-s3/blob/nvidia_rdma/RDMA_PROTOCOL_SPEC.md\)\).

## Notes

- `formatRdmaToken` uses `PRIx64`; `rdmaGet` rejects `size==0`;
credentials are resolved once at construction \(documented limitation:
rotating IAM session tokens need a fresh backend\).
- `SharedCuObjClient` has no S3 dependency, so it can move under
`src/utils/cuobj/` if preferred — happy to do that.

## Test

Builds into `obj_utils`; `RdmaProtocol.*` unit tests pass \(7/7\).

## Types of changes
- \[x\] New feature \(non-breaking — additive utilities\)

<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->
## Summary by CodeRabbit

* **New Features**
* Added accelerated S3 transfers over RDMA for supported hardware and
configurations.
* Added RDMA support for uploads, downloads, ranged reads, checksums,
and multipart operations.
* Added reliable retry handling and clear fallback behavior when RDMA is
unavailable.
* Added strict validation for RDMA transfer responses and protocol
tokens.

* **Bug Fixes**
* Improved S3 configuration handling by treating backend parameters as
read-only.
* Aligned build and linkage requirements for RDMA-enabled S3
functionality.

* **Tests**
* Added comprehensive coverage for RDMA token formatting, response
parsing, and protocol constants.
<!-- end of auto-generated comment: release notes by coderabbit.ai -->

---------

Signed-off-by: Harshavardhana <harsha@minio.io>
Co-authored-by: Adit Ranadive <aranadive@nvidia.com>
Co-authored-by: Vishwanath Venkatesan <vvenkates27@users.noreply.github.com>")  | Sep 11, 2026  |  
|   |   |  [Enforce braces on control statements with](https://github.com/ai-dynamo/nixl/commit/f34ce83d60e1bed0ce247c0df7b485a5bb89fcf9 "Enforce braces on control statements with `clang-format` \(#1599\)

---------

Signed-off-by: Guy Ealey Morag <gealeymorag@nvidia.com>") [(](https://github.com/ai-dynamo/nixl/commit/f34ce83d60e1bed0ce247c0df7b485a5bb89fcf9 "Enforce braces on control statements with `clang-format` \(#1599\)

---------

Signed-off-by: Guy Ealey Morag <gealeymorag@nvidia.com>")[#1599](https://github.com/ai-dynamo/nixl/pull/1599)[)](https://github.com/ai-dynamo/nixl/commit/f34ce83d60e1bed0ce247c0df7b485a5bb89fcf9 "Enforce braces on control statements with `clang-format` \(#1599\)

---------

Signed-off-by: Guy Ealey Morag <gealeymorag@nvidia.com>")  | May 5, 2026  |  
|   |   |  [clang-tidy: Add config to enforce naming style (](https://github.com/ai-dynamo/nixl/commit/0f56527178f2d97fe695f17387383828cbe7679c "clang-tidy: Add config to enforce naming style \(#575\)

* clang-tidy: Add config to enforce naming style

Signed-off-by: Vlad Buslov <vladbu@nvidia.com>

* Update .clang-tidy

Co-authored-by: Raul Akhmetshin <74596089+rakhmets@users.noreply.github.com>
Signed-off-by: Vlad Buslov <vlad@buslov.dev>

* clang-tidy: Update according to latest code style changes

Signed-off-by: Vlad Buslov <vladbu@nvidia.com>

---------

Signed-off-by: Vlad Buslov <vladbu@nvidia.com>
Signed-off-by: Vlad Buslov <vlad@buslov.dev>
Co-authored-by: Raul Akhmetshin <74596089+rakhmets@users.noreply.github.com>")[#575](https://github.com/ai-dynamo/nixl/pull/575)[)](https://github.com/ai-dynamo/nixl/commit/0f56527178f2d97fe695f17387383828cbe7679c "clang-tidy: Add config to enforce naming style \(#575\)

* clang-tidy: Add config to enforce naming style

Signed-off-by: Vlad Buslov <vladbu@nvidia.com>

* Update .clang-tidy

Co-authored-by: Raul Akhmetshin <74596089+rakhmets@users.noreply.github.com>
Signed-off-by: Vlad Buslov <vlad@buslov.dev>

* clang-tidy: Update according to latest code style changes

Signed-off-by: Vlad Buslov <vladbu@nvidia.com>

---------

Signed-off-by: Vlad Buslov <vladbu@nvidia.com>
Signed-off-by: Vlad Buslov <vlad@buslov.dev>
Co-authored-by: Raul Akhmetshin <74596089+rakhmets@users.noreply.github.com>")  | Jul 17, 2025  |  
|   |   |  [CODERABBIT: Updated instructions to C++20. (](https://github.com/ai-dynamo/nixl/commit/6e35da51b94e01657d9c5da882a4e4c979cefc76 "CODERABBIT: Updated instructions to C++20. \(#2091\)

## What?
Updated coderabbit instructions for C++ source files to C++20 instead of
C++17.

## Why?
https://github.com/ai-dynamo/nixl/pull/1571

Signed-off-by: Raul Akhmetshin <rakhmetshin@nvidia.com>")[#2091](https://github.com/ai-dynamo/nixl/pull/2091)[)](https://github.com/ai-dynamo/nixl/commit/6e35da51b94e01657d9c5da882a4e4c979cefc76 "CODERABBIT: Updated instructions to C++20. \(#2091\)

## What?
Updated coderabbit instructions for C++ source files to C++20 instead of
C++17.

## Why?
https://github.com/ai-dynamo/nixl/pull/1571

Signed-off-by: Raul Akhmetshin <rakhmetshin@nvidia.com>")  | Aug 17, 2026  |  
|   |   | [feat: Add AMD accelerator detection and tracking in libfabric topology (](https://github.com/ai-dynamo/nixl/commit/f7cd6470885565259d0ad2aa3183d15d3852fe1e "feat: Add AMD accelerator detection and tracking in libfabric topology \(#1750\)

## What?
1. AMD GPU vendor ID detection \(0x1002\)
1. MI300X device ID range support \(0x74a0-0x74af\)
1. PCI class fallback detection for unknown AMD GPU models
1. `num_amd_accel` counter variable
1. `isAmdAccel\(\)` helper function
1. `getNumAmdAccel\(\)` public accessor
1. HMEM interface mapping returns `FI_HMEM_ROCR` for AMD GPUs
1. Topology-aware grouping includes AMD accelerators
1. Logging updated to report AMD GPUs separately

## Why?
To enable AMD GPU usage with libfabric plugin

## How?
Added hipification of libfabric plugins and tests


<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->
## Summary by CodeRabbit

* **New Features**
* AMD \(HIP/ROCm\) GPU support added for accelerated communications
alongside NVIDIA.

* **Improvements**
* Runtime, memory registration, and context handling extended to treat
AMD devices alongside CUDA where applicable.
* Topology now reports NVIDIA/AMD/Neuron counts separately and includes
AMD in NIC/EFA grouping.
* Example Python script now detects missing optional bindings, logs a
warning, and exits cleanly.

* **Tests**
  * Unit tests updated to exercise AMD/HIP GPU paths.

* **Chores**
  * Spell-check config updated to ignore AMD-related term.
<!-- end of auto-generated comment: release notes by coderabbit.ai -->

---------

Signed-off-by: Matvei Pashkovskii <matvei.pashkovskii@amd.com>
Signed-off-by: Matvei Pashkovskii <mpashkov@amd.com>
Co-authored-by: Adit Ranadive <aranadive@nvidia.com>
Co-authored-by: Mikhail Brinskiy <brminich@users.noreply.github.com>")  | Sep 5, 2026  |  
|   |   |  [Build wheels with the UCX spcx external plugin bundled (](https://github.com/ai-dynamo/nixl/commit/b212c124b0a0b15b5db6a41d536d1731b251db2f "Build wheels with the UCX spcx external plugin bundled \(#1918\)

## What

Adds opt-in support for building the internal `ucx-spcx-plugin` \(UCX
external plugin, `libuct_ib_mlx5_ext.so`\) as part of the NIXL wheel
image build and bundling it into the `nixl-cuXX` wheel.

## How

- **`contrib/Dockerfile.manylinux`**: a dedicated RUN right after the
UCX build \(same pattern as the UCX build itself\) clones the plugin from
internal GitLab at `UCX_SPCX_PLUGIN_REF` \(branch, tag, or full sha;
default `main`\), builds it against the just-built UCX \(`--with-ucx=/usr
--with-ucx-src=/usr/local/src/ucx`, shared-only like UCX\), and
make-installs it directly into `/usr/lib64/ucx` via `--libdir`. Gated by
`BUILD_UCX_SPCX_PLUGIN` \(default `false`\).
- **Wheel bundling — no changes**: `build-wheel.sh` and
`wheel_add_ucx_plugins.py` are untouched; the plugin sits in the UCX
plugins dir and rides the existing bundling/RPATH-patching like any
other UCX module.
- **`contrib/build-container.sh`**: new `--build-ucx-spcx-plugin` /
`--ucx-spcx-plugin-ref` options; the GitLab token is taken from
`NIXL_GITLAB_TOKEN` and handed to the docker build as a file-based
`--secret` \(always supplied, empty when the feature is off, so the
unconditional secret mount works identically on docker and podman\).
- **DPA/FlexIO readiness**: the image pre-installs the FlexIO SDK
\(`libflexio`\) and `dpacc` from the already-configured doca-host repo;
when the plugin's upcoming DPA transport lands and links `libflexio`,
the Dockerfile bundles it next to the plugin automatically so the wheel
ships it — no further changes needed here.

## Token hygiene

The token never appears in image layers, `docker history`, argv, or git
error output: it travels env → 0600 temp file → build secret → env
inside the plugin RUN only, and the clone uses a git credential helper
reading it from the environment \(credential-free URL,
`GIT_TERMINAL_PROMPT=0`\).

## Behavior when disabled

Flag off ⇒ the wheel build is byte-identical to today; the plugin RUN is
a no-op and the wheel RUN is unchanged from main.

## Testing

- Feature leg built end-to-end locally \(x86_64, CUDA 13, Python 3.12\):
plugin cloned/built/installed by the inline RUN, wheel contains
`nixl_cu13.libs/ucx/libuct_ib_mlx5_ext.so.0.0.0` \(shared-only, no static
archive\) alongside `libplugin_UCX.so`; clean-venv `pip install`
verified; `docker history` clean of the token.
- Regression: with the flag off the wheel RUN is byte-identical to main
\(verified by diff\); default builds don't reach any new code path.
- Build fixes discovered during E2E are baked in: UCX installs to `/usr`
in this image \(its configure default\), `ACLOCAL_PATH` so the plugin's
autogen finds `pkg.m4` with the image's source-built autotools, and
explicit `-L/usr/lib64/ucx -L/usr/lib64` link dirs \(the plugin's
configure assumes a `lib/` layout\).

## Why?
\[HPCINFRA-4539\]\(https://jirasw.nvidia.com/browse/HPCINFRA-4539\)


<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->
## Summary by CodeRabbit

* **New Features**
* Added an opt-in build flow to include the UCX SPX plugin in manylinux
container builds.
* Introduced build options/flags to enable the plugin and select the
plugin revision.
* Updated build help/output to show plugin status and required
prerequisites when enabled.
* **Chores**
* Prevented the temporary plugin source directory from being committed.
* Ensured the downloaded plugin sources are cleaned up after the build.
<!-- end of auto-generated comment: release notes by coderabbit.ai -->

Signed-off-by: NirWolfer <nwolfer@nvidia.com>")[#1918](https://github.com/ai-dynamo/nixl/pull/1918)[)](https://github.com/ai-dynamo/nixl/commit/b212c124b0a0b15b5db6a41d536d1731b251db2f "Build wheels with the UCX spcx external plugin bundled \(#1918\)

## What

Adds opt-in support for building the internal `ucx-spcx-plugin` \(UCX
external plugin, `libuct_ib_mlx5_ext.so`\) as part of the NIXL wheel
image build and bundling it into the `nixl-cuXX` wheel.

## How

- **`contrib/Dockerfile.manylinux`**: a dedicated RUN right after the
UCX build \(same pattern as the UCX build itself\) clones the plugin from
internal GitLab at `UCX_SPCX_PLUGIN_REF` \(branch, tag, or full sha;
default `main`\), builds it against the just-built UCX \(`--with-ucx=/usr
--with-ucx-src=/usr/local/src/ucx`, shared-only like UCX\), and
make-installs it directly into `/usr/lib64/ucx` via `--libdir`. Gated by
`BUILD_UCX_SPCX_PLUGIN` \(default `false`\).
- **Wheel bundling — no changes**: `build-wheel.sh` and
`wheel_add_ucx_plugins.py` are untouched; the plugin sits in the UCX
plugins dir and rides the existing bundling/RPATH-patching like any
other UCX module.
- **`contrib/build-container.sh`**: new `--build-ucx-spcx-plugin` /
`--ucx-spcx-plugin-ref` options; the GitLab token is taken from
`NIXL_GITLAB_TOKEN` and handed to the docker build as a file-based
`--secret` \(always supplied, empty when the feature is off, so the
unconditional secret mount works identically on docker and podman\).
- **DPA/FlexIO readiness**: the image pre-installs the FlexIO SDK
\(`libflexio`\) and `dpacc` from the already-configured doca-host repo;
when the plugin's upcoming DPA transport lands and links `libflexio`,
the Dockerfile bundles it next to the plugin automatically so the wheel
ships it — no further changes needed here.

## Token hygiene

The token never appears in image layers, `docker history`, argv, or git
error output: it travels env → 0600 temp file → build secret → env
inside the plugin RUN only, and the clone uses a git credential helper
reading it from the environment \(credential-free URL,
`GIT_TERMINAL_PROMPT=0`\).

## Behavior when disabled

Flag off ⇒ the wheel build is byte-identical to today; the plugin RUN is
a no-op and the wheel RUN is unchanged from main.

## Testing

- Feature leg built end-to-end locally \(x86_64, CUDA 13, Python 3.12\):
plugin cloned/built/installed by the inline RUN, wheel contains
`nixl_cu13.libs/ucx/libuct_ib_mlx5_ext.so.0.0.0` \(shared-only, no static
archive\) alongside `libplugin_UCX.so`; clean-venv `pip install`
verified; `docker history` clean of the token.
- Regression: with the flag off the wheel RUN is byte-identical to main
\(verified by diff\); default builds don't reach any new code path.
- Build fixes discovered during E2E are baked in: UCX installs to `/usr`
in this image \(its configure default\), `ACLOCAL_PATH` so the plugin's
autogen finds `pkg.m4` with the image's source-built autotools, and
explicit `-L/usr/lib64/ucx -L/usr/lib64` link dirs \(the plugin's
configure assumes a `lib/` layout\).

## Why?
\[HPCINFRA-4539\]\(https://jirasw.nvidia.com/browse/HPCINFRA-4539\)


<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->
## Summary by CodeRabbit

* **New Features**
* Added an opt-in build flow to include the UCX SPX plugin in manylinux
container builds.
* Introduced build options/flags to enable the plugin and select the
plugin revision.
* Updated build help/output to show plugin status and required
prerequisites when enabled.
* **Chores**
* Prevented the temporary plugin source directory from being committed.
* Ensured the downloaded plugin sources are cleaned up after the build.
<!-- end of auto-generated comment: release notes by coderabbit.ai -->

Signed-off-by: NirWolfer <nwolfer@nvidia.com>")  | Jul 19, 2026  |  
| [.pre-commit-config.yaml](https://github.com/ai-dynamo/nixl/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  | [.pre-commit-config.yaml](https://github.com/ai-dynamo/nixl/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  |  [Python: Export missing symbols in the meta package (](https://github.com/ai-dynamo/nixl/commit/4f9cd700be96a1779a7d4c2367bdea142033c32d "Python: Export missing symbols in the meta package \(#1604\)

Export missing symbols in the meta package")[#1604](https://github.com/ai-dynamo/nixl/pull/1604)[)](https://github.com/ai-dynamo/nixl/commit/4f9cd700be96a1779a7d4c2367bdea142033c32d "Python: Export missing symbols in the meta package \(#1604\)

Export missing symbols in the meta package")  | Apr 30, 2026  |  
|   |   |  [build: bump UCX version to v1.23.x (](https://github.com/ai-dynamo/nixl/commit/226e76d74d7dc2dc0501c452d4bfe2285b42f2c4 "build: bump UCX version to v1.23.x \(#2163\)

## What?
Bump the default/pinned UCX version from v1.22.x to v1.23.x across CI
Dockerfiles, Jenkins configs, GitLab build scripts, contrib Docker/build
files, and docs.

<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->

## Summary by CodeRabbit

- **Chores**
- Updated the default UCX version to 1.23.x across standard, GPU, ROCm,
container, wheel, and CI builds.
- Updated source-build instructions and attribution details to reference
UCX 1.23.x.
- Existing environment overrides and nightly UCX master coverage remain
unchanged.

<!-- end of auto-generated comment: release notes by coderabbit.ai -->

Signed-off-by: Mikhail Brinskii <mikhailb@nvidia.com>")[#2163](https://github.com/ai-dynamo/nixl/pull/2163)[)](https://github.com/ai-dynamo/nixl/commit/226e76d74d7dc2dc0501c452d4bfe2285b42f2c4 "build: bump UCX version to v1.23.x \(#2163\)

## What?
Bump the default/pinned UCX version from v1.22.x to v1.23.x across CI
Dockerfiles, Jenkins configs, GitLab build scripts, contrib Docker/build
files, and docs.

<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->

## Summary by CodeRabbit

- **Chores**
- Updated the default UCX version to 1.23.x across standard, GPU, ROCm,
container, wheel, and CI builds.
- Updated source-build instructions and attribution details to reference
UCX 1.23.x.
- Existing environment overrides and nightly UCX master coverage remain
unchanged.

<!-- end of auto-generated comment: release notes by coderabbit.ai -->

Signed-off-by: Mikhail Brinskii <mikhailb@nvidia.com>")  | Aug 27, 2026  |  
|   |   |  [chore: update NIXL attributions to 1.3.1 (](https://github.com/ai-dynamo/nixl/commit/b20f4d0adb95f53c4a1272915ef8d86cec304584 "chore: update NIXL attributions to 1.3.1 \(#1901\)

Bump the nixl and nixl-cu13 self-version headers in
ATTRIBUTIONS-Python.md from 1.3.0 to 1.3.1 to match the released package
version \(pyproject is already at 1.3.1\). No third-party dependency
attributions changed for the 1.3.1 patch release.

## What?
_Describe what this PR is doing._

## Why?
_Justification for the PR. If there is an existing issue/bug, please
reference it. For
bug fixes, the 'Why?' and 'What?' can be merged into a single item._

## How?
_It is optional, but for complex PRs, please provide information about
the design,
architecture, approach, etc._


<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->

## Summary by CodeRabbit

* **Documentation**
* Updated license attribution entries for two packages to reflect the
latest versions.
* Kept the same Apache 2.0 license details while refreshing the recorded
package versions.

<!-- end of auto-generated comment: release notes by coderabbit.ai -->

Co-authored-by: Claude Opus 4.8 \(1M context\) <noreply@anthropic.com>")[#1901](https://github.com/ai-dynamo/nixl/pull/1901)[)](https://github.com/ai-dynamo/nixl/commit/b20f4d0adb95f53c4a1272915ef8d86cec304584 "chore: update NIXL attributions to 1.3.1 \(#1901\)

Bump the nixl and nixl-cu13 self-version headers in
ATTRIBUTIONS-Python.md from 1.3.0 to 1.3.1 to match the released package
version \(pyproject is already at 1.3.1\). No third-party dependency
attributions changed for the 1.3.1 patch release.

## What?
_Describe what this PR is doing._

## Why?
_Justification for the PR. If there is an existing issue/bug, please
reference it. For
bug fixes, the 'Why?' and 'What?' can be merged into a single item._

## How?
_It is optional, but for complex PRs, please provide information about
the design,
architecture, approach, etc._


<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->

## Summary by CodeRabbit

* **Documentation**
* Updated license attribution entries for two packages to reflect the
latest versions.
* Kept the same Apache 2.0 license details while refreshing the recorded
package versions.

<!-- end of auto-generated comment: release notes by coderabbit.ai -->

Co-authored-by: Claude Opus 4.8 \(1M context\) <noreply@anthropic.com>")  | Jul 8, 2026  |  
|   |   |  [chore: update attributions for the latest release (](https://github.com/ai-dynamo/nixl/commit/0bae9b542dba9ffefffb64477c48d56e59f8ff12 "chore: update attributions for the latest release \(#1480\)

* chore: update attributions for the latest release

* fix missing license

* update attr

* fix dup

* more changes

* add missing license

* add new pckage")[#1480](https://github.com/ai-dynamo/nixl/pull/1480)[)](https://github.com/ai-dynamo/nixl/commit/0bae9b542dba9ffefffb64477c48d56e59f8ff12 "chore: update attributions for the latest release \(#1480\)

* chore: update attributions for the latest release

* fix missing license

* update attr

* fix dup

* more changes

* add missing license

* add new pckage")  | Apr 9, 2026  |  
|   |   |  [libfabric: update code owner list (](https://github.com/ai-dynamo/nixl/commit/e1186f2e4281ce74528931f35fc88ce6a9fc2589 "libfabric: update code owner list \(#2008\)

## What?
update code owner list

<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->

## Summary by CodeRabbit

* **Chores**
* Updated code ownership for Libfabric-related source, utility, and test
areas.

<!-- end of auto-generated comment: release notes by coderabbit.ai -->

Co-authored-by: Adit Ranadive <aranadive@nvidia.com>
Co-authored-by: ovidiusm <ovidium@nvidia.com>")[#2008](https://github.com/ai-dynamo/nixl/pull/2008)[)](https://github.com/ai-dynamo/nixl/commit/e1186f2e4281ce74528931f35fc88ce6a9fc2589 "libfabric: update code owner list \(#2008\)

## What?
update code owner list

<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->

## Summary by CodeRabbit

* **Chores**
* Updated code ownership for Libfabric-related source, utility, and test
areas.

<!-- end of auto-generated comment: release notes by coderabbit.ai -->

Co-authored-by: Adit Ranadive <aranadive@nvidia.com>
Co-authored-by: ovidiusm <ovidium@nvidia.com>")  | Aug 6, 2026  |  
|   |   |  [feat: Add code of conduct for project (](https://github.com/ai-dynamo/nixl/commit/a58f9f3622e2cce6768279533b23f1f99a28f86c "feat: Add code of conduct for project \(#102\)

* chore: add code of conduct to repo

* fix: add additional files to codeowners")[#102](https://github.com/ai-dynamo/nixl/pull/102)[)](https://github.com/ai-dynamo/nixl/commit/a58f9f3622e2cce6768279533b23f1f99a28f86c "feat: Add code of conduct for project \(#102\)

* chore: add code of conduct to repo

* fix: add additional files to codeowners")  | Apr 1, 2025  |  
|   |   |  [Docs/python: Use docstring standard (](https://github.com/ai-dynamo/nixl/commit/3e26bfd4de360f7ae766f09525d29df78ba1cd13 "Docs/python: Use docstring standard \(#2058\)

## What?
Change the doxygen-like docstrings into standard python docstrings.

## Why?

The Python API currently places documentation strings before classes and
functions and uses tags such as `@brief`, `@args`, `@param`, and
`@return`. In that position they are ordinary, discarded string
expressions rather than Python docstrings, so Python documentation tools
do not pick them up; a Python docstring must be the first statement
inside the class or function it documents.

This PR standardizes the Python API on PEP 257 docstrings with
Google-style structured sections, following the format now documented in
`CONTRIBUTING.md`. First commits updates `CONTRIBUTING.md`, the second
commit moves the docstrings into that format. No text changes are done
in this PR.

I have already experimented with automatic documentation generation for
Fern \(docs.nvidia.com\) and this format is supported by a python docs
generator \(handsdown, see comment
https://github.com/ai-dynamo/nixl/pull/1946#issuecomment-5216012849 \). I
couldn't find any that would process the current format.

Note, due to the dynamic imports in `nixl.py` the docstrings still do
not show up in VSCode \(and i presume other IDEs\) autocomplete, but if
the dynamic import is replaced with a static one, they do show up in the
autocomplete. Nevertheless, for interactive documentation query using
the `.__doc__` property, these strings now show up:

## Verification

Before:

```bash
$ cd /workspace/nixl-upstream
$ LD_LIBRARY_PATH="$PWD/.venv/lib" .venv/bin/python3 -c 'import nixl; print\(nixl.nixl_agent_config.__doc__\)'
None
```

After:

```bash
$ cd /workspace/nixl-pydoc-std
$ LD_LIBRARY_PATH="$PWD/.venv/lib" .venv/bin/python3 -c 'import nixl; print\(nixl.nixl_agent_config.__doc__\)'
Configuration class for NIXL agent.

    Args:
        enable_prog_thread: Whether to enable the progress thread, if available.
        enable_listen_thread: Whether to enable the listener thread for metadata communication.
        listen_port: Specify the port for the listener thread to listen on.
        capture_telemetry: Whether to enable telemetry capture.
        num_threads: Specify number of threads for the supported multi-threaded backends.
        backends: List of backend names for agent to initialize.
            Default is UCX, other backends can be added to the list, or after
            agent creation, can be initialized with create_backend.
        sync_mode: Thread synchronization mode to use for the agent.
            If None, sync_mode is set based on the enable_listen flag.
```

<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->

## Summary by CodeRabbit

* **Documentation**
* Added Python docstring standards covering PEP 257, Google-style
formatting, placement, summaries, and examples.
* Improved API documentation across Python interfaces with consistent,
clearer docstrings.
  * No runtime behavior or public API signatures were changed.

<!-- end of auto-generated comment: release notes by coderabbit.ai -->

---------

Signed-off-by: Lukas Humbel <lhumbel@nvidia.com>")[#2058](https://github.com/ai-dynamo/nixl/pull/2058)[)](https://github.com/ai-dynamo/nixl/commit/3e26bfd4de360f7ae766f09525d29df78ba1cd13 "Docs/python: Use docstring standard \(#2058\)

## What?
Change the doxygen-like docstrings into standard python docstrings.

## Why?

The Python API currently places documentation strings before classes and
functions and uses tags such as `@brief`, `@args`, `@param`, and
`@return`. In that position they are ordinary, discarded string
expressions rather than Python docstrings, so Python documentation tools
do not pick them up; a Python docstring must be the first statement
inside the class or function it documents.

This PR standardizes the Python API on PEP 257 docstrings with
Google-style structured sections, following the format now documented in
`CONTRIBUTING.md`. First commits updates `CONTRIBUTING.md`, the second
commit moves the docstrings into that format. No text changes are done
in this PR.

I have already experimented with automatic documentation generation for
Fern \(docs.nvidia.com\) and this format is supported by a python docs
generator \(handsdown, see comment
https://github.com/ai-dynamo/nixl/pull/1946#issuecomment-5216012849 \). I
couldn't find any that would process the current format.

Note, due to the dynamic imports in `nixl.py` the docstrings still do
not show up in VSCode \(and i presume other IDEs\) autocomplete, but if
the dynamic import is replaced with a static one, they do show up in the
autocomplete. Nevertheless, for interactive documentation query using
the `.__doc__` property, these strings now show up:

## Verification

Before:

```bash
$ cd /workspace/nixl-upstream
$ LD_LIBRARY_PATH="$PWD/.venv/lib" .venv/bin/python3 -c 'import nixl; print\(nixl.nixl_agent_config.__doc__\)'
None
```

After:

```bash
$ cd /workspace/nixl-pydoc-std
$ LD_LIBRARY_PATH="$PWD/.venv/lib" .venv/bin/python3 -c 'import nixl; print\(nixl.nixl_agent_config.__doc__\)'
Configuration class for NIXL agent.

    Args:
        enable_prog_thread: Whether to enable the progress thread, if available.
        enable_listen_thread: Whether to enable the listener thread for metadata communication.
        listen_port: Specify the port for the listener thread to listen on.
        capture_telemetry: Whether to enable telemetry capture.
        num_threads: Specify number of threads for the supported multi-threaded backends.
        backends: List of backend names for agent to initialize.
            Default is UCX, other backends can be added to the list, or after
            agent creation, can be initialized with create_backend.
        sync_mode: Thread synchronization mode to use for the agent.
            If None, sync_mode is set based on the enable_listen flag.
```

<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->

## Summary by CodeRabbit

* **Documentation**
* Added Python docstring standards covering PEP 257, Google-style
formatting, placement, summaries, and examples.
* Improved API documentation across Python interfaces with consistent,
clearer docstrings.
  * No runtime behavior or public API signatures were changed.

<!-- end of auto-generated comment: release notes by coderabbit.ai -->

---------

Signed-off-by: Lukas Humbel <lhumbel@nvidia.com>")  | Aug 11, 2026  |  
|   |   |   | Sep 4, 2026  |  
|   |   |   | Sep 4, 2026  |  
|   |   |  [CI: Add Doxygen documentation build support (](https://github.com/ai-dynamo/nixl/commit/ebac6a26f377f9d483bdb53a7196147a5026b8f6 "CI: Add Doxygen documentation build support \(#142\)

- Add build_docs option to meson_options.txt
- Add Doxygen documentation build configuration in meson.build
- Update README.md with build options and documentation instructions
- Add documentation build location information

Signed-off-by: Daniel Pressler <danielpr@nvidia.com>")[#142](https://github.com/ai-dynamo/nixl/pull/142)[)](https://github.com/ai-dynamo/nixl/commit/ebac6a26f377f9d483bdb53a7196147a5026b8f6 "CI: Add Doxygen documentation build support \(#142\)

- Add build_docs option to meson_options.txt
- Add Doxygen documentation build configuration in meson.build
- Update README.md with build options and documentation instructions
- Add documentation build location information

Signed-off-by: Daniel Pressler <danielpr@nvidia.com>")  | Apr 14, 2025  |  
|   |   |  [EXAMPLES: Introduce NIXL-EP example (](https://github.com/ai-dynamo/nixl/commit/8879d6519c5d0e6380ddba391f963da9413fc69c "EXAMPLES: Introduce NIXL-EP example \(#1043\)

Add an example implementation of expert-parallel
dispatch and combine operations using the NIXL
device API.

Signed-off-by: Itay Alroy <ialroy@nvidia.com>")[#1043](https://github.com/ai-dynamo/nixl/pull/1043)[)](https://github.com/ai-dynamo/nixl/commit/8879d6519c5d0e6380ddba391f963da9413fc69c "EXAMPLES: Introduce NIXL-EP example \(#1043\)

Add an example implementation of expert-parallel
dispatch and combine operations using the NIXL
device API.

Signed-off-by: Itay Alroy <ialroy@nvidia.com>")  | Dec 8, 2025  |  
|   |   |  [build: bump UCX version to v1.23.x (](https://github.com/ai-dynamo/nixl/commit/226e76d74d7dc2dc0501c452d4bfe2285b42f2c4 "build: bump UCX version to v1.23.x \(#2163\)

## What?
Bump the default/pinned UCX version from v1.22.x to v1.23.x across CI
Dockerfiles, Jenkins configs, GitLab build scripts, contrib Docker/build
files, and docs.

<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->

## Summary by CodeRabbit

- **Chores**
- Updated the default UCX version to 1.23.x across standard, GPU, ROCm,
container, wheel, and CI builds.
- Updated source-build instructions and attribution details to reference
UCX 1.23.x.
- Existing environment overrides and nightly UCX master coverage remain
unchanged.

<!-- end of auto-generated comment: release notes by coderabbit.ai -->

Signed-off-by: Mikhail Brinskii <mikhailb@nvidia.com>")[#2163](https://github.com/ai-dynamo/nixl/pull/2163)[)](https://github.com/ai-dynamo/nixl/commit/226e76d74d7dc2dc0501c452d4bfe2285b42f2c4 "build: bump UCX version to v1.23.x \(#2163\)

## What?
Bump the default/pinned UCX version from v1.22.x to v1.23.x across CI
Dockerfiles, Jenkins configs, GitLab build scripts, contrib Docker/build
files, and docs.

<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->

## Summary by CodeRabbit

- **Chores**
- Updated the default UCX version to 1.23.x across standard, GPU, ROCm,
container, wheel, and CI builds.
- Updated source-build instructions and attribution details to reference
UCX 1.23.x.
- Existing environment overrides and nightly UCX master coverage remain
unchanged.

<!-- end of auto-generated comment: release notes by coderabbit.ai -->

Signed-off-by: Mikhail Brinskii <mikhailb@nvidia.com>")  | Aug 27, 2026  |  
|   |   |   | Mar 13, 2025  |  
|   |   | [build: enable cuObject S3 acceleration in the NIXL and nixlbench cont…](https://github.com/ai-dynamo/nixl/commit/e6fb7714a9f3ae452683b3f8f7b63e7d23dc82c5 "build: enable cuObject S3 acceleration in the NIXL and nixlbench containers \(#2228\)

## What?
- meson.build: discover the cuobjclient pkg-config module from
pkg-config --list-all \(newest first\) instead of a hardcoded list that
stopped at cuobjclient-13.3. Warn when none is found.
- Both contrib/Dockerfiles: install the libcuobjclient development files
from the inference-devel image of the same release, then assert with
readelf that libplugin_OBJ.so links libcuobjclient.

No change to how the containers are invoked; no new build flags.

## Why?
The NGC cuda-dl-base devel images ship the libcuobjclient runtime but
not libcuobjclient-dev. NIXL's cuObject probe is required: false, so
both containers built an OBJ plugin with no RDMA data path and nothing
said so — every plugin loaded and the containers were otherwise fully
functional, so it went unnoticed until a downstream accelerated-OBJ test
failed its preflight check.

Separately, meson never probed cuobjclient-13.4, so CUDA 13.4 was
undetected even where packaging is correct.

The wheels were unaffected: they build from cuda:13.2.1-devel-ubi8,
which packages cuObject completely, and Dockerfile.manylinux already has
this readelf assertion. This brings the containers in line.

## How?
Both Dockerfile steps are guarded, so the change is inert unless it is
needed. Dev files are installed only when the base has no cuobjclient.h
of its own, does have a libcuobjclient.so.1.* runtime, and that
runtime's SONAME matches the staged files — otherwise it prints a note
and continues. The assertion is gated on the same runtime check, which
also covers CUDA 12 without a separate flag.

Keep CUOBJ_DEV_IMAGE pinned to the same release as BASE_IMAGE_TAG;
readelf catches a missing link, not a subtly wrong pin.

Verified on stock NGC images:

``` 
  ┌──────────────────────┬───────────────────────────────────────────────────────────────────┐
  │         Base         │                              Result                               │
  ├──────────────────────┼───────────────────────────────────────────────────────────────────┤
  │ 26.08-cuda13.4-devel │ installs; both containers link libcuobjclient.so.1, NIXL EP built │
  ├──────────────────────┼───────────────────────────────────────────────────────────────────┤
  │ 26.07-cuda13.3-devel │ no cuObject, skips, builds unchanged                              │
  ├──────────────────────┼───────────────────────────────────────────────────────────────────┤
  │ cuda:13.3.1-devel    │ already has 13.3 dev files, left untouched                        │ 
  └──────────────────────┴───────────────────────────────────────────────────────────────────┘
```

### Release builds

The current default `BASE_IMAGE_TAG`
\(`25.10-cuda13.0-devel-ubuntu24.04`\) ships no cuObject at all, so on the
default this change is inert — it prints `cuObject: no runtime in base,
skipping` and builds exactly as before.

Until the default moves to a 13.4 image, **release container builds
should pin the base explicitly**:

```
--base-image-tag 26.08-cuda13.4-devel-ubuntu24.04
```

With that pin the `readelf` assertion makes acceleration enforced rather
than assumed: if the base ever stops providing cuObject, the release
build fails instead of quietly shipping an unaccelerated OBJ plugin.
Both containers are verified on that image \(NIXL EP built, 11 plugins,
`ldd -r` clean\). Wheels are unaffected — they build from
`cuda:13.2.1-devel-ubi8`.

Note: #2205 flips the default `BASE_IMAGE_TAG` to `26.08-cuda13.4-devel`
and touches the same files. Once it lands the pin becomes redundant,
though it stays valid either way; whichever of the two merges second
needs a trivial rebase.

<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->

## Summary by CodeRabbit

- **New Features**
- Added support for automatically detecting compatible CUDA cuObject
development files during container builds.
- Added compatibility handling for multiple CUDA cuObject runtime
versions.

- **Bug Fixes**
- Container builds now verify that required cuObject plugin libraries
are present and correctly linked when CUDA support is available.
- Improved cuObject dependency discovery across supported CUDA versions.

- **Documentation**
- Configuration warnings now identify missing cuObject dependencies and
the modules that were searched.

<!-- end of auto-generated comment: release notes by coderabbit.ai -->

---------

Signed-off-by: Adit Ranadive <aranadive@nvidia.com>")  | Sep 10, 2026  |  
|   |   | [feat: Add AMD accelerator detection and tracking in libfabric topology (](https://github.com/ai-dynamo/nixl/commit/f7cd6470885565259d0ad2aa3183d15d3852fe1e "feat: Add AMD accelerator detection and tracking in libfabric topology \(#1750\)

## What?
1. AMD GPU vendor ID detection \(0x1002\)
1. MI300X device ID range support \(0x74a0-0x74af\)
1. PCI class fallback detection for unknown AMD GPU models
1. `num_amd_accel` counter variable
1. `isAmdAccel\(\)` helper function
1. `getNumAmdAccel\(\)` public accessor
1. HMEM interface mapping returns `FI_HMEM_ROCR` for AMD GPUs
1. Topology-aware grouping includes AMD accelerators
1. Logging updated to report AMD GPUs separately

## Why?
To enable AMD GPU usage with libfabric plugin

## How?
Added hipification of libfabric plugins and tests


<!-- This is an auto-generated comment: release notes by coderabbit.ai
-->
## Summary by CodeRabbit

* **New Features**
* AMD \(HIP/ROCm\) GPU support added for accelerated communications
alongside NVIDIA.

* **Improvements**
* Runtime, memory registration, and context handling extended to treat
AMD devices alongside CUDA where applicable.
* Topology now reports NVIDIA/AMD/Neuron counts separately and includes
AMD in NIC/EFA grouping.
* Example Python script now detects missing optional bindings, logs a
warning, and exits cleanly.

* **Tests**
  * Unit tests updated to exercise AMD/HIP GPU paths.

* **Chores**
  * Spell-check config updated to ignore AMD-related term.
<!-- end of auto-generated comment: release notes by coderabbit.ai -->

---------

Signed-off-by: Matvei Pashkovskii <matvei.pashkovskii@amd.com>
Signed-off-by: Matvei Pashkovskii <mpashkov@amd.com>
Co-authored-by: Adit Ranadive <aranadive@nvidia.com>
Co-authored-by: Mikhail Brinskiy <brminich@users.noreply.github.com>")  | Sep 5, 2026  |  
|   |   | [SRC/BINDINGS/RUST: build and install rust bindings with meson, add ru…](https://github.com/ai-dynamo/nixl/commit/d2d41a77efee01b9d725225e7e1b4c28931719fb "SRC/BINDINGS/RUST: build and install rust bindings with meson, add rust bindings tests to CI \(#290\)

* BINDINGS/RUST: added Cargo.toml in project root, rust bindings are being compiled automatically

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: rust bindings are built and installed with meson / ninja

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* MESON_OPTIONS.TXT: added rust_bindings meson option, updated readme file

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: removed a redundant space

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* Cargo.toml: added missing copyrights

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: include -> includes, added Cargo.lock, fixed aio detection issue

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: removed a blank line

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/PLUGINS/POSIX: added back compiler.find_library in addition to the dependency call

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB/TEST_RUST.SH: added rust bindings tests to CI

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: revert lib path to x86_64-linux-gnu for CI

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: build.rs will search in both ubuntu and redhat lib paths

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB/BUILD.SH: enable_rust_bindings=true on build

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: removed unnecessary inner Cargo.lock

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: added a flag to cargo build

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: lib64 path is based on OS dist

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: added /usr/include, for some reason CI cant find stdbool.h

* SRC/BINDINGS/RUST: added use os_info;

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: checked linux type using os_info

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB: cargo version should be 1.86

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB/BUILD.SH: fixed rust installation on CI

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB/BUILD.SH added -y to rust installation

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: changed to CPP imports

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: tried to fix CI again

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB: fixed rust CI scripts

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: still fixing CI

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB: fixed rust testing in CI

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB: fixed PKG_CONFIG_PATH

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB: make sure cargo in PATH

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB: \[DEBUGGING\] added prints for cargo

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB: added ucx pkg-config path to to PKG_CONFIG_PATH

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: used pkg-config in build.rs

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB: set NIXL_PREFIX to

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* BINDINGS/PYTHON: changed installation path

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB: fixing python CI - WIP

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/PYTHON: create a wheel file for python bindings

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/PYTHON: hopefull fixed python CI

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB: uncomment -x -e

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB: fixed cpu-specific paths

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB: commented out some env exports

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB: removed unnecessary modifications in test_python.sh

* .GITLAB: set gds_path by CPU arch

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB: trying to disable gds

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB: commented out -e -x

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: adding back some missing APIs and tests \(WIP\)

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* BINDINGS/RUST: added back send_local_md

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST/TESTS: adding back missing helper functions

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: fixed import issues

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: added back estimate_xfer_cost

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB: reverted commenting set -e / set -x

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST/SRC: added back nixl_cap_estimate_xfer_cost bindings to lib.rs

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: req.inner.as_ptr\(\) -> req.handle\(\)

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUST: removed commented out lines

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* BINDINGS/RUST: replaced error prints to error! instead of trace,, removed code duplication in build.rs

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .GITLAB: minor fixes

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .CI/JENKINS/LIB: added rust tests to blossom CI

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* .CI/JENKINS/LIB: removed a space

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* SRC/BINDINGS/RUS: resolved CR comments, changed enable_rust_bindings to just 'rust' nixl -> NIXL, etc

Signed-off-by: Roie Danino <rdanino@nvidia.com>

* CARGO.TOML: fixed keywords list to fit into crates.io limitations

Signed-off-by: Roie Danino <rdanino@nvidia.com>

---------

Signed-off-by: Roie Danino <rdanino@nvidia.com>
Co-authored-by: Yossi Itigin <yosefe@nvidia.com>")  | Aug 11, 2025  |  
|   |   |   | Sep 4, 2026  |  
|   |   |   | May 22, 2025  |  
| View all files |  
## Repository files navigation
# NVIDIA Inference Xfer Library (NIXL)
NVIDIA Inference Xfer Library (NIXL) is targeted for accelerating point to point communications in AI inference frameworks such as NVIDIA Dynamo, while providing an abstraction over various types of memory (e.g., CPU and GPU) and storage (e.g., file, block and object store) through a modular plug-in architecture.
## Documentation and Resources
  * [NIXL overview](https://github.com/ai-dynamo/nixl/blob/main/docs/nixl.md) - Core concepts/architecture overview (`docs/nixl.md`)
  * [Python API](https://github.com/ai-dynamo/nixl/blob/main/docs/python_api.md) - Python API usage and examples (`docs/python_api.md`)
  * [Backend guide](https://github.com/ai-dynamo/nixl/blob/main/docs/BackendGuide.md) - Backend/plugin development guide (`docs/BackendGuide.md`)
  * [Telemetry](https://github.com/ai-dynamo/nixl/blob/main/docs/telemetry.md) - Observability and telemetry details (`docs/telemetry.md`)
  * [Doxygen guide](https://github.com/ai-dynamo/nixl/blob/main/docs/doxygen/nixl_doxygen.md) - API/class diagrams overview (`docs/doxygen/nixl_doxygen.md`)
  * [Doxygen images](https://github.com/ai-dynamo/nixl/tree/main/docs/doxygen) - Diagram assets (`docs/doxygen/`)
  * [NIXLBench docs](https://github.com/ai-dynamo/nixl/blob/main/benchmark/nixlbench/README.md) - Benchmark usage guide (`benchmark/nixlbench/README.md`)
  * [KVBench docs](https://github.com/ai-dynamo/nixl/tree/main/benchmark/kvbench/docs) - KVBench workflows and tutorials (`benchmark/kvbench/docs/`)


## Supported Platforms
NIXL is supported on a Linux environment only. It is tested on Ubuntu (22.04/24.04) and Fedora. macOS and Windows are not currently supported; use a Linux host or container/VM.
## Pre-build Distributions
### PyPI Wheel
The nixl python API and libraries, including UCX, are available directly through PyPI. For example, if you have a GPU running on a Linux host, container, or VM, you can do the following install:
Install with:

```
pip install nixl
```

This installs both CUDA 12 and CUDA 13 backends. At runtime, the correct backend is selected automatically based on the CUDA version reported by PyTorch.
## Prerequisites for source build (Linux)
NIXL requires a C++20 compatible compiler (GCC >= 11 or Clang >= 14).
### Ubuntu:
`$ sudo apt install build-essential cmake pkg-config`
### Fedora:
`$ sudo dnf install gcc-c++ cmake pkg-config`
### Python
`$ pip3 install meson ninja pybind11 tomlkit`
NIXL was tested with UCX version 1.23.x.
[GDRCopy](https://github.com/NVIDIA/gdrcopy) is available on Github and is necessary for maximum performance, but UCX and NIXL will work without it.

```
$ git clone https://github.com/openucx/ucx.git
$ cd ucx
$ git checkout v1.23.x
$ ./autogen.sh
$ ./contrib/configure-release-mt       \
    --enable-shared                    \
    --disable-static                   \
    --disable-doxygen-doc              \
    --enable-optimizations             \
    --without-avx                      \
    --enable-cma                       \
    --enable-devel-headers             \
    --with-cuda=<cuda install>         \
    --with-verbs                       \
    --with-dm                          \
    --with-gdrcopy=<gdrcopy install>
$ make -j
$ make -j install-strip
$ ldconfig

```

### ETCD (Optional)
NIXL can use ETCD for metadata distribution and coordination between nodes in distributed environments. To use ETCD with NIXL:
#### ETCD Server and Client

```
$ sudo apt install etcd etcd-server etcd-client

# Or use Docker
$ docker run -d -p 2379:2379 quay.io/coreos/etcd:v3.5.1

```

#### ETCD CPP API
Installed from <https://github.com/etcd-cpp-apiv3/etcd-cpp-apiv3>

```
$ sudo apt install libgrpc-dev libgrpc++-dev libprotobuf-dev protobuf-compiler-grpc
$ sudo apt install libcpprest-dev
$ git clone https://github.com/etcd-cpp-apiv3/etcd-cpp-apiv3.git
$ cd etcd-cpp-apiv3
$ mkdir build && cd build
$ cmake ..
$ make -j$(nproc) && make install

```

### Additional plugins
Some plugins may have additional build requirements, see them here:


## Getting started
### Build & install

```
$ meson setup <name_of_build_dir>
$ cd <name_of_build_dir>
$ ninja
$ ninja install

```

### Build Options
#### Release build (default)

```
$ meson setup name_of_build_dir
```

#### Debug build

```
$ meson setup name_of_build_dir --buildtype=debug
```

#### NIXL-specific build options

```
# Example with custom options
$ meson setup name_of_build_dir \
    -Dbuild_docs=true \           # Build Doxygen documentation
    -Ducx_path=/path/to/ucx \     # Custom UCX installation path
    -Dinstall_headers=true \      # Install development headers
    -Ddisable_gds_backend=false   # Enable GDS backend
```

Common build options:
  * `build_docs`: Build Doxygen documentation (default: false)
  * `ucx_path`: Path to UCX installation (default: system path)
  * `install_headers`: Install development headers (default: true)
  * `disable_gds_backend`: Disable GDS backend (default: false)
  * `cudapath_inc`, `cudapath_lib`: Custom CUDA paths
  * `static_plugins`: Comma-separated list of plugins to build statically
  * `enable_plugins`: Comma-separated list of plugins to build (e.g. `-Denable_plugins=UCX,POSIX`). Cannot be used with `disable_plugins`.
  * `disable_plugins`: Comma-separated list of plugins to exclude (e.g. `-Ddisable_plugins=GDS`). Cannot be used with `enable_plugins`.
  * `wheel_variant`: Override the Python wheel variant suffix (e.g. `-Dwheel_variant=rocm` yields `nixl_rocm`). Empty (default) = autodetect from the CUDA major version.


#### Building for AMD ROCm
NIXL itself builds vendor-neutrally; CPU-side hardware detection (`hwInfo::numAmdGpus`) discovers AMD GPUs via PCI vendor `0x1002` whether or not a ROCm toolchain is present. GPU-side ROCm/HIP build support is available for nixlbench and UCX plugin unit tests. When packaging a ROCm wheel, pass `-Dwheel_variant=rocm` so the wheel is named `nixl_rocm`.
**Building with ROCm support:**

```
# For UCX unit tests with ROCm
$ meson setup build -Drocm_path=/opt/rocm

# Or specify a custom ROCm path
$ meson setup build -Drocm_path=/custom/path/to/rocm
```

**Plugins on ROCm hosts (CUDA toolchain absent):**
  * `UCX` — primary transport for AMD GPU memory (requires UCX built with `--with-rocm`).
  * `POSIX`, `OBJ`, `AZURE_BLOB`, `HF3FS`, `MOONCAKE`, `GUSLI`, `UCCL` — vendor-neutral; build unchanged.
  * `GDS` / `GDS_MT`, `GPUNETIO`, `LIBFABRIC` (with `-DHAVE_CUDA`) — skip automatically because their CUDA / cuFile / DOCA dependencies are not found.


**Known gaps (will be addressed in follow-up PRs):**
  * `LIBFABRIC` plugin disabled on ROCm pending header refactor.
  * No NVSHMEM-equivalent backend yet (rocSHMEM analog is a candidate for a future plugin).


#### Environment Variables
There are a few environment variables that can be set to configure the build:
  * `NIXL_NO_STUBS_FALLBACK`: If not set or 0, build NIXL stub library if the library build fails


### Building Documentation
If you have Doxygen installed, you can build the documentation:

```
# Configure with documentation enabled
$ meson setup name_of_build_dir -Dbuild_docs=true
$ cd name_of_build_dir
$ ninja

# Documentation will be generated in <name_of_build_dir>/html
# After installation (ninja install), documentation will be available in <prefix>/share/doc/nixl/
```

### Python Interface
NIXL provides Python bindings through pybind11. For detailed Python API documentation, see [docs/python_api.md](https://github.com/ai-dynamo/nixl/blob/main/docs/python_api.md).
The preferred way to install the Python bindings is through pip from PyPI:

```
pip install nixl
```

This installs both CUDA 12 and CUDA 13 backends. At runtime, the correct backend is selected automatically based on the CUDA version reported by PyTorch.
#### Installation from source
Prerequisites:
  * `uv`: <https://docs.astral.sh/uv/getting-started/installation/>
  * `tomlkit`: <https://pypi.org/project/tomlkit/>
  * `PyTorch`: <https://pytorch.org/get-started/locally/>


`uv` is always required _even if_ you have another kind of Python virtual environment manager or if you are using a system-wide Python installation without using a virtual environment.
Example with `uv` Python virtual environment:

```
curl -LsSf https://astral.sh/uv/install.sh | sh
export PATH="$HOME/.local/bin:${PATH}"

uv venv .venv --python 3.12
source .venv/bin/activate
uv pip install tomlkit

```

Example with python-virtualenv:

```
curl -LsSf https://astral.sh/uv/install.sh | sh
export PATH="$HOME/.local/bin:${PATH}"

python3 -m venv .venv
source .venv/bin/activate
pip install tomlkit

```

Example with system-wide Python installation without using a virtual environment:

```
curl -LsSf https://astral.sh/uv/install.sh | sh
export PATH="$HOME/.local/bin:${PATH}"

pip install tomlkit

```

Then install PyTorch following the instructions on the PyTorch website: <https://pytorch.org/get-started/locally/>
After installing the prerequisites, you can build and install the NIXL binaries and the Python bindings from source. You have to:
  1. Build NIXL binaries and install them
  2. Build and install the CUDA platform-specific package (`nixl-cu12` or `nixl-cu13`)
  3. Build and install the `nixl` meta-package


**For CUDA 12:**

```
pip install .
meson setup build
ninja -C build install
pip install build/src/bindings/python/nixl-meta/nixl-*-py3-none-any.whl

```

**For CUDA 13:**

```
pip install .
./contrib/tomlutil.py --wheel-name nixl-cu13 pyproject.toml
meson setup build
ninja -C build install
pip install build/src/bindings/python/nixl-meta/nixl-*-py3-none-any.whl

```

To check if the installation is successful, you can run the following command:

```
python3 -c "import nixl; agent = nixl.nixl_agent('agent1')"

```

which should print:

```
2026-01-08 13:36:27 NIXL INFO    _api.py:363 Backend UCX was instantiated
2026-01-08 13:36:27 NIXL INFO    _api.py:253 Initialized NIXL agent: agent1

```

You can also run a complete Python example to test the installation:

```
python3 examples/python/expanded_two_peers.py --mode=target --use_cuda=true --ip=127.0.0.1 --port=4242 &
sleep 5
python3 examples/python/expanded_two_peers.py --mode=initiator --use_cuda=true --ip=127.0.0.1 --port=4242

```

For more Python examples, see [examples/python/](https://github.com/ai-dynamo/nixl/blob/main/examples/python).
### Rust Bindings
#### Build
  * Use `-Drust=true` meson option to build rust bindings.
  * Use `--buildtype=debug` for a debug build (default is release).
  * Or build manually: 

```
$ cargo build --release
```



#### Install
The bindings will be installed under `nixl-sys` in the configured installation prefix. Can be done using ninja, from project build directory:

```
$ ninja install
```


```
# Rust bindings tests
$ cargo test

```

Use in your project by adding to `Cargo.toml`:

```
[dependencies]
nixl-sys = { path = "path/to/nixl/bindings/rust" }
```

### Other build options
See [contrib/README.md](https://github.com/ai-dynamo/nixl/blob/main/contrib/README.md) for more build options.
### Building Docker container
To build the docker container, first clone the current repository. Also make sure you are able to pull docker images to your machine before attempting to build the container.
Run the following from the root folder of the cloned NIXL repository:

```
$ ./contrib/build-container.sh

```

By default, the container is built with Ubuntu 24.04. To build a container for Ubuntu 22.04 use the --os option as follows:

```
$ ./contrib/build-container.sh --os ubuntu22

```

To see all the options supported by the container use:

```
$ ./contrib/build-container.sh -h

```

The container has the NIXL python bindings preinstalled (built from source against the container's own PyTorch). For a redistributable python wheel, use the wheel build script below or install the published `nixl` package.
### Building the python wheel
The contrib folder also includes a script to build the python wheel with the UCX dependencies. Note, that UCX and other NIXL dependencies are required to be installed.

```
$ ./contrib/build-wheel.sh

```

## Running with ETCD
NIXL can use ETCD for metadata exchange between distributed nodes. This is especially useful in containerized or cloud-native environments.
### Environment Setup
To use ETCD with NIXL, set the following environment variables:

```
# Set ETCD endpoints (required) - replace localhost with the hostname of the etcd server
export NIXL_ETCD_ENDPOINTS="http://localhost:2379"

# Set ETCD namespace (optional, defaults to /nixl/agents)
export NIXL_ETCD_NAMESPACE="/nixl/agents"
```

### Running the ETCD Example
NIXL includes an example demonstrating metadata exchange and data transfer using ETCD:

```
# Start an ETCD server if not already running
# For example:
# docker run -d -p 2379:2379 quay.io/coreos/etcd:v3.5.1

# Set the ETCD env variables as above

# Run the example. The two agents in the example will exchange metadata through ETCD
# and perform data transfers
./nixl_build_path/examples/nixl_etcd_example
```

### nixlbench Benchmark
For more comprehensive testing, the nixlbench benchmarking tool supports ETCD for worker coordination:

```
# Build nixlbench (see benchmark/nixlbench/README.md for details)
cd benchmark/nixlbench
meson setup build  cd build  ninja

# Run benchmark with ETCD
./nixlbench --etcd-endpoints http://localhost:2379 --backend UCX --initiator_seg_type VRAM
```

## Code Examples
  * [C++ examples](https://github.com/ai-dynamo/nixl/tree/main/examples/cpp)
  * [Python examples](https://github.com/ai-dynamo/nixl/tree/main/examples/python)


## Contributing
For contribution guidelines, see [CONTRIBUTING.md](https://github.com/ai-dynamo/nixl/blob/main/CONTRIBUTING.md) (`CONTRIBUTING.md`).
## Third-Party Components
This project will download and install additional third-party open source software projects. Review the license terms of these open source projects before use.
NIXL Python wheels bundle NVIDIA modules (`libuct_ib_mlx5_ext.so`, `libuct_ib_mlx5_gda.so`, `libuct_ib_mlx5_gdp.so`) licensed under the [NVIDIA Proprietary License](https://github.com/ai-dynamo/nixl/blob/main/licenses/NVIDIA-proprietary-LICENSE.txt) (`LicenseRef-NvidiaProprietary`).
## About
NVIDIA Inference Xfer Library (NIXL)
### Resources
### Code of conduct
### Contributing
### Security policy
### Stars
**1.3k** stars
### Watchers
**27** watching
### Forks
[ forks](https://github.com/ai-dynamo/nixl/forks)
## 
[ v1.4.1Latest Sep 1, 2026 ](https://github.com/ai-dynamo/nixl/releases/tag/v1.4.1)
## 
## Languages
  * [C++73.3%](https://github.com/ai-dynamo/nixl/search?l=c%2B%2B)
  * [Python8.9%](https://github.com/ai-dynamo/nixl/search?l=python)
  * [Rust5.2%](https://github.com/ai-dynamo/nixl/search?l=rust)
  * [Meson4.5%](https://github.com/ai-dynamo/nixl/search?l=meson)
  * [Shell3.9%](https://github.com/ai-dynamo/nixl/search?l=shell)
  * [Cuda2.1%](https://github.com/ai-dynamo/nixl/search?l=cuda)
  * [Other2.1%](https://github.com/ai-dynamo/nixl/search?l=Other)


You can’t perform that action at this time. 
