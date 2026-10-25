---
title: 'GitHub - huggingface/text-generation-inference: Large Language Model Text
  Generation Inference · GitHub'
id: github-huggingfacetext-generation-inference-large-language-model-text-generation
tags:
- internal-llm-inference-stacks-7f0c90
- tgi-maintenance-mode
- tgi-archived
- ground-truth
- staleness-check
created: '2026-10-09T23:02:40.377433Z'
updated: '2026-10-10T14:52:17.173454Z'
source: https://github.com/huggingface/text-generation-inference
source_domain: github.com
fetched_at: '2026-10-09T23:02:40.354641Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Primary source: Hugging Face''s own text-generation-inference GitHub repository.
  This is the ground-truth confirmation and UPDATE of the maintenance-mode fact cited
  by secondary sources in this batch -- the repo has moved beyond maintenance mode
  to full archival: a GitHub banner states ''This repository was archived by the owner
  on Mar 21, 2026. It is now read-only,'' with the final commit (#3352, an AWS deployment/benchmarking
  docs PR) also dated Mar 21, 2026. The repo''s own README caution banner (last live
  state before archival) reads verbatim: ''text-generation-inference is now in maintenance
  mode. Going forward, we will accept pull requests for minor bug fixes, documentation
  improvements and lightweight maintenance tasks. TGI has initiated the movement for
  optimized inference engines to rely on a transformers model architectures. This
  approach is now adopted by downstream inference engines, which we contribute to
  and recommend using going forward: vllm, SGLang, as well as local engines with inter-compatibility
  such as llama.cpp or MLX.'' This is Hugging Face''s own first-party successor recommendation,
  naming vLLM and SGLang as the production successors and llama.cpp/MLX as the local-engine
  successors -- not a third party''s interpretation. TIMELINE: maintenance-mode caution
  text had been live for some months (matches the December 11, 2025 date reported
  by secondary PremAI sources in this batch) before the repository itself was fully
  archived/frozen on March 21, 2026. This is a stronger, more final staleness signal
  than ''maintenance mode'' alone: the repo now accepts zero further commits of any
  kind.'
---

*Suggested by [[llm-inference-servers-compared-vllm-vs-tgi-vs-sglang-vs-triton-2026]] — primary TGI repo to confirm maintenance-mode status directly from Hugging Face*

[Skip to content](https://github.com/huggingface/text-generation-inference#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/huggingface/text-generation-inference) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/huggingface/text-generation-inference) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/huggingface/text-generation-inference) to refresh your session. Dismiss alert
This repository was archived by the owner on Mar 21, 2026. It is now read-only. 
[**241** Branches](https://github.com/huggingface/text-generation-inference/branches)[](https://github.com/huggingface/text-generation-inference/tags)
Go to file
Open more actions menu
## Latest commit
3 people
[docs: add AWS (EC2/SageMaker) deployment + benchmarking guide (](https://github.com/huggingface/text-generation-inference/commit/b4adbf2f6e2e721280bd0ea5f91d70f7d033f5ed)[#3352](https://github.com/huggingface/text-generation-inference/pull/3352)[)](https://github.com/huggingface/text-generation-inference/commit/b4adbf2f6e2e721280bd0ea5f91d70f7d033f5ed)
Open commit detailsfailure
Mar 21, 2026
[b4adbf2](https://github.com/huggingface/text-generation-inference/commit/b4adbf2f6e2e721280bd0ea5f91d70f7d033f5ed) · Mar 21, 2026
## History
[1,450 Commits](https://github.com/huggingface/text-generation-inference/commits/main/)
Open commit details
1,450 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   | [Update CodeQL workflow for security analysis](https://github.com/huggingface/text-generation-inference/commit/db931fcf42da018a3657580c64bf2720f167f9ab "Update CodeQL workflow for security analysis")  | Jan 8, 2026  |  
|   |   |   | Dec 10, 2024  |  
|   |   |  [feat: support max_image_fetch_size to limit (](https://github.com/huggingface/text-generation-inference/commit/24ee40d143d8d046039f12f76940a85886cbe152 "feat: support max_image_fetch_size to limit \(#3339\)

* feat: support max_image_fetch_size to limit

* fix: update model path for test

* fix: adjust model repo id for test again

* fix: apply clippy lints

* fix: clippy fix

* fix: avoid torch build isolation in docker

* fix: bump repo id in flash llama tests

* fix: temporarily avoid problematic repos in tests")[#3339](https://github.com/huggingface/text-generation-inference/pull/3339)[)](https://github.com/huggingface/text-generation-inference/commit/24ee40d143d8d046039f12f76940a85886cbe152 "feat: support max_image_fetch_size to limit \(#3339\)

* feat: support max_image_fetch_size to limit

* fix: update model path for test

* fix: adjust model repo id for test again

* fix: apply clippy lints

* fix: clippy fix

* fix: avoid torch build isolation in docker

* fix: bump repo id in flash llama tests

* fix: temporarily avoid problematic repos in tests")  | Nov 18, 2025  |  
|   |   |   | Sep 16, 2025  |  
| [clients/python](https://github.com/huggingface/text-generation-inference/tree/main/clients/python "This path skips through empty directories")  | [clients/python](https://github.com/huggingface/text-generation-inference/tree/main/clients/python "This path skips through empty directories")  | [Upgrading the python client deps (still deprecated, but used for](https://github.com/huggingface/text-generation-inference/commit/fe56f760df3230dd9a24c4ed25741bb576904621 "Upgrading the python client deps \(still deprecated, but used for
integration-tests\)")  | Apr 14, 2025  |  
|  [docs: add AWS (EC2/SageMaker) deployment + benchmarking guide (](https://github.com/huggingface/text-generation-inference/commit/b4adbf2f6e2e721280bd0ea5f91d70f7d033f5ed "docs: add AWS \(EC2/SageMaker\) deployment + benchmarking guide \(#3352\)

* docs: add AWS \(EC2/SageMaker\) deployment + benchmarking guide

- Add an AWS deployment tutorial for EC2 + SageMaker
- Fix SageMaker example indentation and link to the new guide
- Add the new guide to the docs toctree

* docs: address AWS deployment review feedback

* Apply suggestion from code review

---------

Co-authored-by: Fahad Alghanim <fkalghan@email.sc.edu>
Co-authored-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")[#3352](https://github.com/huggingface/text-generation-inference/pull/3352)[)](https://github.com/huggingface/text-generation-inference/commit/b4adbf2f6e2e721280bd0ea5f91d70f7d033f5ed "docs: add AWS \(EC2/SageMaker\) deployment + benchmarking guide \(#3352\)

* docs: add AWS \(EC2/SageMaker\) deployment + benchmarking guide

- Add an AWS deployment tutorial for EC2 + SageMaker
- Fix SageMaker example indentation and link to the new guide
- Add the new guide to the docs toctree

* docs: address AWS deployment review feedback

* Apply suggestion from code review

---------

Co-authored-by: Fahad Alghanim <fkalghan@email.sc.edu>
Co-authored-by: Alvaro Bartolome <36760800+alvarobartt@users.noreply.github.com>")  | Mar 21, 2026  |  
|   |   |  [feat: support max_image_fetch_size to limit (](https://github.com/huggingface/text-generation-inference/commit/24ee40d143d8d046039f12f76940a85886cbe152 "feat: support max_image_fetch_size to limit \(#3339\)

* feat: support max_image_fetch_size to limit

* fix: update model path for test

* fix: adjust model repo id for test again

* fix: apply clippy lints

* fix: clippy fix

* fix: avoid torch build isolation in docker

* fix: bump repo id in flash llama tests

* fix: temporarily avoid problematic repos in tests")[#3339](https://github.com/huggingface/text-generation-inference/pull/3339)[)](https://github.com/huggingface/text-generation-inference/commit/24ee40d143d8d046039f12f76940a85886cbe152 "feat: support max_image_fetch_size to limit \(#3339\)

* feat: support max_image_fetch_size to limit

* fix: update model path for test

* fix: adjust model repo id for test again

* fix: apply clippy lints

* fix: clippy fix

* fix: avoid torch build isolation in docker

* fix: bump repo id in flash llama tests

* fix: temporarily avoid problematic repos in tests")  | Nov 18, 2025  |  
|   |   | [ fix(num_devices): fix num_shard/num device auto compute when NVIDIA_…](https://github.com/huggingface/text-generation-inference/commit/dfb3fbe78e0d1d69251bd33065591c61bafdfaa8 " fix\(num_devices\): fix num_shard/num device auto compute when NVIDIA_VISIBLE_DEVICES == "all" or "void" \(#3346\)

* fix\(num_devices\): fix num_shard/num devices auto compute when NVIDIA_VISIBLE_DEVICES == "all"
the computed num_shards was always 1 in this case, no matter what

* fix\(num_devices\): make TGI shard auto compute compliant with nvidia-container-toolkit in cdi mode")  | Dec 18, 2025  |  
|   |   |   | Dec 6, 2024  |  
| [Revert "feat: bump flake including transformers and huggingface_hub v…](https://github.com/huggingface/text-generation-inference/commit/5e747f4e306589254a5287f4b536b2efe5f54cbf "Revert "feat: bump flake including transformers and huggingface_hub versions" \(#3330\)

Revert "feat: bump flake including transformers and huggingface_hub versions …"

This reverts commit 356de85c290667a9ba193f0b25ab4b260d29855f.")  | Sep 16, 2025  |  
|   |   |  [Choosing input/total tokens automatically based on available VRAM? (](https://github.com/huggingface/text-generation-inference/commit/0c9b6cdd768558652afdf5e5053aeb49bf4bc21f "Choosing input/total tokens automatically based on available VRAM? \(#2673\)

* Choosing input/total tokens automatically based on available VRAM?

* Update doc.

* Remove generated files.

* Trying to fix non chunking targets.

* Attempt #2

* fix.

* QuantLinear is rocm compatible.

* Much simpler logic after the overhead.

* Updating logic + non flash.

* Revert doc text.

* Simple updates.

* Fix integration mt0 \(transformers update\).")[#…](https://github.com/huggingface/text-generation-inference/pull/2673)  | Oct 28, 2024  |  
|   |   |  [feat: support max_image_fetch_size to limit (](https://github.com/huggingface/text-generation-inference/commit/24ee40d143d8d046039f12f76940a85886cbe152 "feat: support max_image_fetch_size to limit \(#3339\)

* feat: support max_image_fetch_size to limit

* fix: update model path for test

* fix: adjust model repo id for test again

* fix: apply clippy lints

* fix: clippy fix

* fix: avoid torch build isolation in docker

* fix: bump repo id in flash llama tests

* fix: temporarily avoid problematic repos in tests")[#3339](https://github.com/huggingface/text-generation-inference/pull/3339)[)](https://github.com/huggingface/text-generation-inference/commit/24ee40d143d8d046039f12f76940a85886cbe152 "feat: support max_image_fetch_size to limit \(#3339\)

* feat: support max_image_fetch_size to limit

* fix: update model path for test

* fix: adjust model repo id for test again

* fix: apply clippy lints

* fix: clippy fix

* fix: avoid torch build isolation in docker

* fix: bump repo id in flash llama tests

* fix: temporarily avoid problematic repos in tests")  | Nov 18, 2025  |  
|   |   |  [feat: support max_image_fetch_size to limit (](https://github.com/huggingface/text-generation-inference/commit/24ee40d143d8d046039f12f76940a85886cbe152 "feat: support max_image_fetch_size to limit \(#3339\)

* feat: support max_image_fetch_size to limit

* fix: update model path for test

* fix: adjust model repo id for test again

* fix: apply clippy lints

* fix: clippy fix

* fix: avoid torch build isolation in docker

* fix: bump repo id in flash llama tests

* fix: temporarily avoid problematic repos in tests")[#3339](https://github.com/huggingface/text-generation-inference/pull/3339)[)](https://github.com/huggingface/text-generation-inference/commit/24ee40d143d8d046039f12f76940a85886cbe152 "feat: support max_image_fetch_size to limit \(#3339\)

* feat: support max_image_fetch_size to limit

* fix: update model path for test

* fix: adjust model repo id for test again

* fix: apply clippy lints

* fix: clippy fix

* fix: avoid torch build isolation in docker

* fix: bump repo id in flash llama tests

* fix: temporarily avoid problematic repos in tests")  | Nov 18, 2025  |  
|   |   |  [nix: experimental support for building a Docker container (](https://github.com/huggingface/text-generation-inference/commit/584b4d7a68a00991a61134b602364f6db33587a0 " nix: experimental support for building a Docker container \(#2470\)

* nix: experimental support for building a Docker image

Run using something like:

```
docker run \\
  --device nvidia.com/gpu=all \\
  -it --rm -p 8080:80 \\
  -v $PWD/data:/data \\
  -v $PWD/tmp:/tmp \\
  tgi-docker:latest \\
  --model-id <model_id>
```

* Example of building the Docker image using Nix inside Docker

* Stream to make the builder image smaller

This avoids storing a Docker image tarball in the image. Instead,
stream the layers while doing `docker run`.

* Don't spam journalctl on Linux

* Other dockerfile.

---------

Co-authored-by: Nicolas Patry <patry.nicolas@protonmail.com>")[#2470](https://github.com/huggingface/text-generation-inference/pull/2470)[)](https://github.com/huggingface/text-generation-inference/commit/584b4d7a68a00991a61134b602364f6db33587a0 " nix: experimental support for building a Docker container \(#2470\)

* nix: experimental support for building a Docker image

Run using something like:

```
docker run \\
  --device nvidia.com/gpu=all \\
  -it --rm -p 8080:80 \\
  -v $PWD/data:/data \\
  -v $PWD/tmp:/tmp \\
  tgi-docker:latest \\
  --model-id <model_id>
```

* Example of building the Docker image using Nix inside Docker

* Stream to make the builder image smaller

This avoids storing a Docker image tarball in the image. Instead,
stream the layers while doing `docker run`.

* Don't spam journalctl on Linux

* Other dockerfile.

---------

Co-authored-by: Nicolas Patry <patry.nicolas@protonmail.com>")  | Oct 1, 2024  |  
|   |   |  [Release of Gaudi Backend for TGI (](https://github.com/huggingface/text-generation-inference/commit/27ed8486768fc0ebd72a36a3a47183980e209135 "Release of Gaudi Backend for TGI \(#3091\)

* feat\(gaudi\): release ready \(docs, docker image and vlm ready\)

* fix\(gaudi\): add default argument for the dockerfile

* fix\(gaudi\): remove use of latest for gaudi docker image + redid gaudi benchmarking section to include best practices")[#3091](https://github.com/huggingface/text-generation-inference/pull/3091)[)](https://github.com/huggingface/text-generation-inference/commit/27ed8486768fc0ebd72a36a3a47183980e209135 "Release of Gaudi Backend for TGI \(#3091\)

* feat\(gaudi\): release ready \(docs, docker image and vlm ready\)

* fix\(gaudi\): add default argument for the dockerfile

* fix\(gaudi\): remove use of latest for gaudi docker image + redid gaudi benchmarking section to include best practices")  | Mar 13, 2025  |  
|   |   |  [Move JSON grammar -> regex grammar conversion to the router (](https://github.com/huggingface/text-generation-inference/commit/289aa4855449fe9162c8da7e13aaaaef81755a3b "Move JSON grammar -> regex grammar conversion to the router \(#2772\)

* Move JSON grammar -> regex grammar conversion to the router

This change moves the JSON grammar -> regex grammar conversion to the
router by adding a dependency on the `outlines-core` Rust crate. In
contrast to the Python implementation, the conversions are not LRU-cached
since they seem to be fast enough:

simple schema           time:   \[5.8293 µs 5.8307 µs 5.8320 µs\]
                        change: \[-13.166% -12.884% -12.641%\] \(p = 0.00 < 0.05\)
                        Performance has improved.

complex schema          time:   \[14.875 µs 14.881 µs 14.887 µs\]
                        change: \[-2.1637% -1.9914% -1.7852%\] \(p = 0.00 < 0.05\)
                        Performance has improved.

Using the schemas from:
https://github.com/dottxt-ai/outlines-core/blob/main/benchmarks/bench_json_schema.py")[#2772](https://github.com/huggingface/text-generation-inference/pull/2772)[)](https://github.com/huggingface/text-generation-inference/commit/289aa4855449fe9162c8da7e13aaaaef81755a3b "Move JSON grammar -> regex grammar conversion to the router \(#2772\)

* Move JSON grammar -> regex grammar conversion to the router

This change moves the JSON grammar -> regex grammar conversion to the
router by adding a dependency on the `outlines-core` Rust crate. In
contrast to the Python implementation, the conversions are not LRU-cached
since they seem to be fast enough:

simple schema           time:   \[5.8293 µs 5.8307 µs 5.8320 µs\]
                        change: \[-13.166% -12.884% -12.641%\] \(p = 0.00 < 0.05\)
                        Performance has improved.

complex schema          time:   \[14.875 µs 14.881 µs 14.887 µs\]
                        change: \[-2.1637% -1.9914% -1.7852%\] \(p = 0.00 < 0.05\)
                        Performance has improved.

Using the schemas from:
https://github.com/dottxt-ai/outlines-core/blob/main/benchmarks/bench_json_schema.py")  | Nov 25, 2024  |  
| [.redocly.lint-ignore.yaml](https://github.com/huggingface/text-generation-inference/blob/main/.redocly.lint-ignore.yaml ".redocly.lint-ignore.yaml")  | [.redocly.lint-ignore.yaml](https://github.com/huggingface/text-generation-inference/blob/main/.redocly.lint-ignore.yaml ".redocly.lint-ignore.yaml")  |   | Sep 19, 2024  |  
|   |   |  [Set maximum grpc message receive size to 2GiB (](https://github.com/huggingface/text-generation-inference/commit/c8c7ccd31e1e760d216c9d2f2b17b0d984ed033b "Set maximum grpc message receive size to 2GiB \(#2075\)

* Set maximum grpc message receive size to 2GiB

The previous default was 4MiB, which doesn't really work well for
multi-modal models.

* Update to Rust 1.79.0

* Fixup formatting to make PR pass")[#2075](https://github.com/huggingface/text-generation-inference/pull/2075)[)](https://github.com/huggingface/text-generation-inference/commit/c8c7ccd31e1e760d216c9d2f2b17b0d984ed033b "Set maximum grpc message receive size to 2GiB \(#2075\)

* Set maximum grpc message receive size to 2GiB

The previous default was 4MiB, which doesn't really work well for
multi-modal models.

* Update to Rust 1.79.0

* Fixup formatting to make PR pass")  | Jun 17, 2024  |  
|   |   |  [Set maximum grpc message receive size to 2GiB (](https://github.com/huggingface/text-generation-inference/commit/c8c7ccd31e1e760d216c9d2f2b17b0d984ed033b "Set maximum grpc message receive size to 2GiB \(#2075\)

* Set maximum grpc message receive size to 2GiB

The previous default was 4MiB, which doesn't really work well for
multi-modal models.

* Update to Rust 1.79.0

* Fixup formatting to make PR pass")[#2075](https://github.com/huggingface/text-generation-inference/pull/2075)[)](https://github.com/huggingface/text-generation-inference/commit/c8c7ccd31e1e760d216c9d2f2b17b0d984ed033b "Set maximum grpc message receive size to 2GiB \(#2075\)

* Set maximum grpc message receive size to 2GiB

The previous default was 4MiB, which doesn't really work well for
multi-modal models.

* Update to Rust 1.79.0

* Fixup formatting to make PR pass")  | Jun 17, 2024  |  
|   |   |   | Sep 16, 2025  |  
|   |   |   | Sep 16, 2025  |  
|   |   |  [Switch to punica-sgmv kernel from the Hub (](https://github.com/huggingface/text-generation-inference/commit/e32528792cc7ccfdc5dd4b10fecedeb907422261 "Switch to punica-sgmv kernel from the Hub \(#3236\)

* Switch to punica-sgmv kernel from the Hub

This also switches \(temporarily\) to the tgi-nix/kernel-builder merge
branch, bumping up to CUDA 12.8 \(same as non-Nix Torch\).

* nix: client depends on aiohttp

This probably worked before the nixpkgs bump because a dependency
propagated aiohttp.")[#3236](https://github.com/huggingface/text-generation-inference/pull/3236)[)](https://github.com/huggingface/text-generation-inference/commit/e32528792cc7ccfdc5dd4b10fecedeb907422261 "Switch to punica-sgmv kernel from the Hub \(#3236\)

* Switch to punica-sgmv kernel from the Hub

This also switches \(temporarily\) to the tgi-nix/kernel-builder merge
branch, bumping up to CUDA 12.8 \(same as non-Nix Torch\).

* nix: client depends on aiohttp

This probably worked before the nixpkgs bump because a dependency
propagated aiohttp.")  | May 21, 2025  |  
|   |   |   | Aug 26, 2025  |  
|   |   |   | May 22, 2025  |  
|   |   |   | Apr 7, 2025  |  
|   |   |  [[gaudi] Deepseek v2 mla and add ep to unquantized moe (](https://github.com/huggingface/text-generation-inference/commit/ebb26f0ccd8eb8a9d2defa7ba8acbd6c1d3401ae "\[gaudi\] Deepseek v2 mla and add ep to unquantized moe \(#3287\)

Signed-off-by: Wang, Yi A <yi.a.wang@intel.com>")[#3287](https://github.com/huggingface/text-generation-inference/pull/3287)[)](https://github.com/huggingface/text-generation-inference/commit/ebb26f0ccd8eb8a9d2defa7ba8acbd6c1d3401ae "\[gaudi\] Deepseek v2 mla and add ep to unquantized moe \(#3287\)

Signed-off-by: Wang, Yi A <yi.a.wang@intel.com>")  | Jul 7, 2025  |  
|   |   |   | Aug 18, 2025  |  
|   |   |   | Mar 24, 2025  |  
|   |   |  [misc(gha): expose action cache url and runtime as secrets (](https://github.com/huggingface/text-generation-inference/commit/85790a19a7fa9917733a9a4536e50057f8d513d8 "misc\(gha\): expose action cache url and runtime as secrets \(#2964\)

* misc\(gha\): expose action cache url and runtime as secrets

* \(CI\): Move S3 Auth to OIDC

* Fix Typo

* change bucket name

* fix aws auth creds

* misc\(gha\): fix invalid syntax for secrets

* WIP: Add AWS session token

* Increase session time

* Remove actions_cache_url mount from Dockerfile

Removed an unused mount for actions_cache_url in the Dockerfile.

* WIP

---------

Co-authored-by: Guillaume LEGENDRE <glegendre01@gmail.com>")[#2964](https://github.com/huggingface/text-generation-inference/pull/2964)[)](https://github.com/huggingface/text-generation-inference/commit/85790a19a7fa9917733a9a4536e50057f8d513d8 "misc\(gha\): expose action cache url and runtime as secrets \(#2964\)

* misc\(gha\): expose action cache url and runtime as secrets

* \(CI\): Move S3 Auth to OIDC

* Fix Typo

* change bucket name

* fix aws auth creds

* misc\(gha\): fix invalid syntax for secrets

* WIP: Add AWS session token

* Increase session time

* Remove actions_cache_url mount from Dockerfile

Removed an unused mount for actions_cache_url in the Dockerfile.

* WIP

---------

Co-authored-by: Guillaume LEGENDRE <glegendre01@gmail.com>")  | Nov 17, 2025  |  
|   |   |  [Revert license to Apache 2.0 (](https://github.com/huggingface/text-generation-inference/commit/ff42d33e9944832a19171967d2edd6c292bdb2d6 "Revert license to Apache 2.0 \(#1714\)

Reverts huggingface/text-generation-inference#725

---------

Co-authored-by: Julien Chaumond <julien@huggingface.co>")[#1714](https://github.com/huggingface/text-generation-inference/pull/1714)[)](https://github.com/huggingface/text-generation-inference/commit/ff42d33e9944832a19171967d2edd6c292bdb2d6 "Revert license to Apache 2.0 \(#1714\)

Reverts huggingface/text-generation-inference#725

---------

Co-authored-by: Julien Chaumond <julien@huggingface.co>")  | Apr 8, 2024  |  
|   |   |  [Release of Gaudi Backend for TGI (](https://github.com/huggingface/text-generation-inference/commit/27ed8486768fc0ebd72a36a3a47183980e209135 "Release of Gaudi Backend for TGI \(#3091\)

* feat\(gaudi\): release ready \(docs, docker image and vlm ready\)

* fix\(gaudi\): add default argument for the dockerfile

* fix\(gaudi\): remove use of latest for gaudi docker image + redid gaudi benchmarking section to include best practices")[#3091](https://github.com/huggingface/text-generation-inference/pull/3091)[)](https://github.com/huggingface/text-generation-inference/commit/27ed8486768fc0ebd72a36a3a47183980e209135 "Release of Gaudi Backend for TGI \(#3091\)

* feat\(gaudi\): release ready \(docs, docker image and vlm ready\)

* fix\(gaudi\): add default argument for the dockerfile

* fix\(gaudi\): remove use of latest for gaudi docker image + redid gaudi benchmarking section to include best practices")  | Mar 13, 2025  |  
|   |   |   | Dec 11, 2025  |  
|   |   |  [Move JSON grammar -> regex grammar conversion to the router (](https://github.com/huggingface/text-generation-inference/commit/289aa4855449fe9162c8da7e13aaaaef81755a3b "Move JSON grammar -> regex grammar conversion to the router \(#2772\)

* Move JSON grammar -> regex grammar conversion to the router

This change moves the JSON grammar -> regex grammar conversion to the
router by adding a dependency on the `outlines-core` Rust crate. In
contrast to the Python implementation, the conversions are not LRU-cached
since they seem to be fast enough:

simple schema           time:   \[5.8293 µs 5.8307 µs 5.8320 µs\]
                        change: \[-13.166% -12.884% -12.641%\] \(p = 0.00 < 0.05\)
                        Performance has improved.

complex schema          time:   \[14.875 µs 14.881 µs 14.887 µs\]
                        change: \[-2.1637% -1.9914% -1.7852%\] \(p = 0.00 < 0.05\)
                        Performance has improved.

Using the schemas from:
https://github.com/dottxt-ai/outlines-core/blob/main/benchmarks/bench_json_schema.py")[#2772](https://github.com/huggingface/text-generation-inference/pull/2772)[)](https://github.com/huggingface/text-generation-inference/commit/289aa4855449fe9162c8da7e13aaaaef81755a3b "Move JSON grammar -> regex grammar conversion to the router \(#2772\)

* Move JSON grammar -> regex grammar conversion to the router

This change moves the JSON grammar -> regex grammar conversion to the
router by adding a dependency on the `outlines-core` Rust crate. In
contrast to the Python implementation, the conversions are not LRU-cached
since they seem to be fast enough:

simple schema           time:   \[5.8293 µs 5.8307 µs 5.8320 µs\]
                        change: \[-13.166% -12.884% -12.641%\] \(p = 0.00 < 0.05\)
                        Performance has improved.

complex schema          time:   \[14.875 µs 14.881 µs 14.887 µs\]
                        change: \[-2.1637% -1.9914% -1.7852%\] \(p = 0.00 < 0.05\)
                        Performance has improved.

Using the schemas from:
https://github.com/dottxt-ai/outlines-core/blob/main/benchmarks/bench_json_schema.py")  | Nov 25, 2024  |  
|   |   | [Revert "feat: bump flake including transformers and huggingface_hub v…](https://github.com/huggingface/text-generation-inference/commit/5e747f4e306589254a5287f4b536b2efe5f54cbf "Revert "feat: bump flake including transformers and huggingface_hub versions" \(#3330\)

Revert "feat: bump flake including transformers and huggingface_hub versions …"

This reverts commit 356de85c290667a9ba193f0b25ab4b260d29855f.")  | Sep 16, 2025  |  
|   |   | [Revert "feat: bump flake including transformers and huggingface_hub v…](https://github.com/huggingface/text-generation-inference/commit/5e747f4e306589254a5287f4b536b2efe5f54cbf "Revert "feat: bump flake including transformers and huggingface_hub versions" \(#3330\)

Revert "feat: bump flake including transformers and huggingface_hub versions …"

This reverts commit 356de85c290667a9ba193f0b25ab4b260d29855f.")  | Sep 16, 2025  |  
|   |   |   | Mar 24, 2025  |  
|   |   |  [feat(sagemaker): add trust remote code to entrypoint (](https://github.com/huggingface/text-generation-inference/commit/62fc40103079bc27e97194ef69e9e34a180b0a85 "feat\(sagemaker\): add trust remote code to entrypoint \(#394\)")[#394](https://github.com/huggingface/text-generation-inference/pull/394)[)](https://github.com/huggingface/text-generation-inference/commit/62fc40103079bc27e97194ef69e9e34a180b0a85 "feat\(sagemaker\): add trust remote code to entrypoint \(#394\)")  | Jun 2, 2023  |  
|   |   |   | Mar 18, 2025  |  
|   |   |   | Mar 4, 2025  |  
| View all files |  
## Repository files navigation
Caution
text-generation-inference is now in maintenance mode. Going forward, we will accept pull requests for minor bug fixes, documentation improvements and lightweight maintenance tasks.
TGI has initiated the movement for optimized inference engines to rely on a `transformers` model architectures. This approach is now adopted by downstream inference engines, which we contribute to and recommend using going forward: [vllm](https://github.com/vllm-project/vllm), [SGLang](https://github.com/sgl-project/sglang), as well as local engines with inter-compatibility such as llama.cpp or MLX.
# Text Generation Inference
A Rust, Python and gRPC server for text generation inference. Used in production at [Hugging Face](https://huggingface.co) to power Hugging Chat, the Inference API and Inference Endpoints.
## Table of contents
  * [Get Started](https://github.com/huggingface/text-generation-inference#get-started)
    * [API documentation](https://github.com/huggingface/text-generation-inference#api-documentation)
    * [Using a private or gated model](https://github.com/huggingface/text-generation-inference#using-a-private-or-gated-model)
    * [A note on Shared Memory (shm)](https://github.com/huggingface/text-generation-inference#a-note-on-shared-memory-shm)
    * [Distributed Tracing](https://github.com/huggingface/text-generation-inference#distributed-tracing)
    * [Local install](https://github.com/huggingface/text-generation-inference#local-install)
    * [Local install (Nix)](https://github.com/huggingface/text-generation-inference#local-install-nix)
  * [Optimized architectures](https://github.com/huggingface/text-generation-inference#optimized-architectures)
  * [Run locally](https://github.com/huggingface/text-generation-inference#run-locally)


Text Generation Inference (TGI) is a toolkit for deploying and serving Large Language Models (LLMs). TGI enables high-performance text generation for the most popular open-source LLMs, including Llama, Falcon, StarCoder, BLOOM, GPT-NeoX, and [more](https://huggingface.co/docs/text-generation-inference/supported_models). TGI implements many features, such as:
  * Simple launcher to serve most popular LLMs
  * Production ready (distributed tracing with Open Telemetry, Prometheus metrics)
  * Tensor Parallelism for faster inference on multiple GPUs
  * Token streaming using Server-Sent Events (SSE)
  * Continuous batching of incoming requests for increased total throughput
  * [Messages API](https://huggingface.co/docs/text-generation-inference/en/messages_api) compatible with Open AI Chat Completion API
  * Optimized transformers code for inference using [Flash Attention](https://github.com/HazyResearch/flash-attention) and [Paged Attention](https://github.com/vllm-project/vllm) on the most popular architectures
  * Quantization with : 
  * [Safetensors](https://github.com/huggingface/safetensors) weight loading
  * Watermarking with [A Watermark for Large Language Models](https://arxiv.org/abs/2301.10226)
  * Logits warper (temperature scaling, top-p, top-k, repetition penalty, more details see [transformers.LogitsProcessor](https://huggingface.co/docs/transformers/internal/generation_utils#transformers.LogitsProcessor))
  * Stop sequences
  * Log probabilities
  * [Speculation](https://huggingface.co/docs/text-generation-inference/conceptual/speculation) ~2x latency
  * [Guidance/JSON](https://huggingface.co/docs/text-generation-inference/conceptual/guidance). Specify output format to speed up inference and make sure the output is valid according to some specs..
  * Custom Prompt Generation: Easily generate text by providing custom prompts to guide the model's output
  * Fine-tuning Support: Utilize fine-tuned models for specific tasks to achieve higher accuracy and performance


### Hardware support
  * [AMD](https://github.com/huggingface/text-generation-inference/pkgs/container/text-generation-inference) (-rocm)


## Get Started
### Docker
For a detailed starting guide, please see the [Quick Tour](https://huggingface.co/docs/text-generation-inference/quicktour). The easiest way of getting started is using the official Docker container:

```
model=HuggingFaceH4/zephyr-7b-beta
# share a volume with the Docker container to avoid downloading weights every run
volume=$PWD/data

docker run --gpus all --shm-size 1g -p 8080:80 -v $volume:/data \
    ghcr.io/huggingface/text-generation-inference:3.3.5 --model-id $model
```

And then you can make requests like

```
curl 127.0.0.1:8080/generate_stream \
    -X POST \
    -d '{"inputs":"What is Deep Learning?","parameters":{"max_new_tokens":20}}' \
    -H 'Content-Type: application/json'
```

You can also use [TGI's Messages API](https://huggingface.co/docs/text-generation-inference/en/messages_api) to obtain Open AI Chat Completion API compatible responses.

```
curl localhost:8080/v1/chat/completions \
    -X POST \
    -d 
  "model": "tgi",
  "messages": [

      "role": "system",
      "content": "You are a helpful assistant."
    },

      "role": "user",
      "content": "What is deep learning?"

  ],
  "stream": true,
  "max_tokens": 20
 \
    -H 'Content-Type: application/json'
```

**Note:** To use NVIDIA GPUs, you need to install the [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html). We also recommend using NVIDIA drivers with CUDA version 12.2 or higher. For running the Docker container on a machine with no GPUs or CUDA support, it is enough to remove the `--gpus all` flag and add `--disable-custom-kernels`, please note CPU is not the intended platform for this project, so performance might be subpar.
**Note:** TGI supports AMD Instinct MI210 and MI250 GPUs. Details can be found in the [Supported Hardware documentation](https://huggingface.co/docs/text-generation-inference/installation_amd#using-tgi-with-amd-gpus). To use AMD GPUs, please use `docker run --device /dev/kfd --device /dev/dri --shm-size 1g -p 8080:80 -v $volume:/data ghcr.io/huggingface/text-generation-inference:3.3.5-rocm --model-id $model` instead of the command above.
To see all options to serve your models (in the [code](https://github.com/huggingface/text-generation-inference/blob/main/launcher/src/main.rs) or in the cli):

```
text-generation-launcher --help

```

### API documentation
You can consult the OpenAPI documentation of the `text-generation-inference` REST API using the `/docs` route. The Swagger UI is also available at: <https://huggingface.github.io/text-generation-inference>.
### Using a private or gated model
You have the option to utilize the `HF_TOKEN` environment variable for configuring the token employed by `text-generation-inference`. This allows you to gain access to protected resources.
For example, if you want to serve the gated Llama V2 model variants:
  1. Go to <https://huggingface.co/settings/tokens>
  2. Copy your CLI READ token
  3. Export `HF_TOKEN=<your CLI READ token>`


or with Docker:

```
model=meta-llama/Meta-Llama-3.1-8B-Instruct
volume=$PWD/data # share a volume with the Docker container to avoid downloading weights every run
token=your cli READ token

docker run --gpus all --shm-size 1g -e HF_TOKEN=$token -p 8080:80 -v $volume:/data \
    ghcr.io/huggingface/text-generation-inference:3.3.5 --model-id $model
```

### A note on Shared Memory (shm)
[`NCCL`](https://docs.nvidia.com/deeplearning/nccl/user-guide/docs/index.html) is a communication framework used by `PyTorch` to do distributed training/inference. `text-generation-inference` makes use of `NCCL` to enable Tensor Parallelism to dramatically speed up inference for large language models.
In order to share data between the different devices of a `NCCL` group, `NCCL` might fall back to using the host memory if peer-to-peer using NVLink or PCI is not possible.
To allow the container to use 1G of Shared Memory and support SHM sharing, we add `--shm-size 1g` on the above command.
If you are running `text-generation-inference` inside `Kubernetes`. You can also add Shared Memory to the container by creating a volume with:

```
- name: shm
  emptyDir:
   medium: Memory
   sizeLimit: 1Gi
```

and mounting it to `/dev/shm`.
Finally, you can also disable SHM sharing by using the `NCCL_SHM_DISABLE=1` environment variable. However, note that this will impact performance.
### Distributed Tracing
`text-generation-inference` is instrumented with distributed tracing using OpenTelemetry. You can use this feature by setting the address to an OTLP collector with the `--otlp-endpoint` argument. The default service name can be overridden with the `--otlp-service-name` argument
### Architecture
Detailed blogpost by Adyen on TGI inner workings: [LLM inference at scale with TGI (Martin Iglesias Goyanes - Adyen, 2024)](https://www.adyen.com/knowledge-hub/llm-inference-at-scale-with-tgi)
### Local install
You can also opt to install `text-generation-inference` locally.
First clone the repository and change directory into it:

```
git clone https://github.com/huggingface/text-generation-inference
cd text-generation-inference
```

Then [install Rust](https://rustup.rs/) and create a Python virtual environment with at least Python 3.9, e.g. using `conda` or `python venv`:

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

#using conda
conda create -n text-generation-inference python=3.11
conda activate text-generation-inference

#using python venv
python3 -m venv .venv
source .venv/bin/activate
```

You may also need to install Protoc.
On Linux:

```
PROTOC_ZIP=protoc-21.12-linux-x86_64.zip
curl -OL https://github.com/protocolbuffers/protobuf/releases/download/v21.12/$PROTOC_ZIP
sudo unzip -o $PROTOC_ZIP -d /usr/local bin/protoc
sudo unzip -o $PROTOC_ZIP -d /usr/local 'include/*'
rm -f $PROTOC_ZIP
```

On MacOS, using Homebrew:

```
brew install protobuf
```

Then run:

```
BUILD_EXTENSIONS=True make install # Install repository and HF/transformer fork with CUDA kernels
text-generation-launcher --model-id mistralai/Mistral-7B-Instruct-v0.2
```

**Note:** on some machines, you may also need the OpenSSL libraries and gcc. On Linux machines, run:

```
sudo apt-get install libssl-dev gcc -y
```

### Local install (Nix)
Another option is to install `text-generation-inference` locally using [Nix](https://nixos.org). Currently, we only support Nix on x86_64 Linux with CUDA GPUs. When using Nix, all dependencies can be pulled from a binary cache, removing the need to build them locally.
First follow the instructions to [install Cachix and enable the Hugging Face cache](https://app.cachix.org/cache/huggingface). Setting up the cache is important, otherwise Nix will build many of the dependencies locally, which can take hours.
After that you can run TGI with `nix run`:

```
cd text-generation-inference
nix run --extra-experimental-features nix-command --extra-experimental-features flakes . -- --model-id meta-llama/Llama-3.1-8B-Instruct
```

**Note:** when you are using Nix on a non-NixOS system, you have to [make some symlinks](https://danieldk.eu/Nix-CUDA-on-non-NixOS-systems#make-runopengl-driverlib-and-symlink-the-driver-library) to make the CUDA driver libraries visible to Nix packages.
For TGI development, you can use the `impure` dev shell:

```
nix develop .#impure

# Only needed the first time the devshell is started or after updating the protobuf.
(
cd server
mkdir text_generation_server/pb || true
python -m grpc_tools.protoc -I../proto/v3 --python_out=text_generation_server/pb \
       --grpc_python_out=text_generation_server/pb --mypy_out=text_generation_server/pb ../proto/v3/generate.proto
find text_generation_server/pb/ -type f -name "*.py" -print0 -exec sed -i -e 's/^\(import.*pb2\)/from . \1/g' {} \;
touch text_generation_server/pb/__init__.py
)
```

All development dependencies (cargo, Python, Torch), etc. are available in this dev shell.
## Optimized architectures
TGI works out of the box to serve optimized models for all modern models. They can be found in [this list](https://huggingface.co/docs/text-generation-inference/supported_models).
Other architectures are supported on a best-effort basis using:
`AutoModelForCausalLM.from_pretrained(<model>, device_map="auto")`
or
`AutoModelForSeq2SeqLM.from_pretrained(<model>, device_map="auto")`
## Run locally

```
text-generation-launcher --model-id mistralai/Mistral-7B-Instruct-v0.2
```

### Quantization
You can also run pre-quantized weights (AWQ, GPTQ, Marlin) or on-the-fly quantize weights with bitsandbytes, EETQ, fp8, to reduce the VRAM requirement:

```
text-generation-launcher --model-id mistralai/Mistral-7B-Instruct-v0.2 --quantize
```

4bit quantization is available using the [NF4 and FP4 data types from bitsandbytes](https://arxiv.org/pdf/2305.14314.pdf). It can be enabled by providing `--quantize bitsandbytes-nf4` or `--quantize bitsandbytes-fp4` as a command line argument to `text-generation-launcher`.
Read more about quantization in the [Quantization documentation](https://huggingface.co/docs/text-generation-inference/en/conceptual/quantization).
## Develop

```
make server-dev
make router-dev
```

## Testing

```
# python
make python-server-tests
make python-client-tests
# or both server and client tests
make python-tests
# rust cargo tests
make rust-tests
# integration tests
make integration-tests
```

## About
Large Language Model Text Generation Inference
[hf.co/docs/text-generation-inference](http://hf.co/docs/text-generation-inference)
### Topics
[bloom](https://github.com/topics/bloom)[deep-learning](https://github.com/topics/deep-learning)[falcon](https://github.com/topics/falcon)[gpt](https://github.com/topics/gpt)[inference](https://github.com/topics/inference)[nlp](https://github.com/topics/nlp)[pytorch](https://github.com/topics/pytorch)[starcoder](https://github.com/topics/starcoder)[transformer](https://github.com/topics/transformer)
### Resources
### Code of conduct
### Contributing
### Stars
**10.9k** stars
### Watchers
**103** watching
### Forks
[**1.3k** forks](https://github.com/huggingface/text-generation-inference/forks)
## 
[ v3.3.7Latest Dec 19, 2025 ](https://github.com/huggingface/text-generation-inference/releases/tag/v3.3.7)
## 
## Languages
  * [Python78.6%](https://github.com/huggingface/text-generation-inference/search?l=python)
  * [Rust16.3%](https://github.com/huggingface/text-generation-inference/search?l=rust)
  * [Cuda2.9%](https://github.com/huggingface/text-generation-inference/search?l=cuda)
  * [C++0.9%](https://github.com/huggingface/text-generation-inference/search?l=c%2B%2B)
  * [Nix0.3%](https://github.com/huggingface/text-generation-inference/search?l=nix)
  * [Shell0.3%](https://github.com/huggingface/text-generation-inference/search?l=shell)
  * [Other0.7%](https://github.com/huggingface/text-generation-inference/search?l=Other)


You can’t perform that action at this time. 
