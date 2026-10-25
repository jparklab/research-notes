---
title: 'GitHub - llm-d/llm-d: Achieve state of the art inference performance with
  modern accelerators on Kubernetes · GitHub'
id: github-llm-dllm-d-achieve-state-of-the-art-inference-performance-with-modern-acc
tags:
- llmd-router-nonk8s-backends-54b774
- llm-d-architecture
- llm-d-releases
- cncf
- component-versioning
created: '2026-09-11T20:45:44.438871Z'
updated: '2026-09-15T04:55:23.815730Z'
source: https://github.com/llm-d/llm-d
source_domain: github.com
fetched_at: '2026-09-11T20:45:44.415694Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'llm-d/llm-d GitHub repo homepage (fetched Sep 11 2026, HEAD at commit 3a82b81,
  1,425 commits, 18 branches, 4.5k stars). Confirms current release cadence and identity:
  latest tagged release v0.9.0 (17 Aug 2026); a same-day commit (#2382, merged Sep
  11 2026) documents an EPP config schema field ''verified against llm-d-router v0.10.0
  (commit 0b43c187)'' -- i.e. the llm-d-router component repo is already at v0.10.0
  even though the umbrella llm-d meta-repo''s latest tagged release is v0.9.0, confirming
  component and meta-repo versioning are decoupled (matches the query''s ''v0.5->v0.10''
  framing, referring to llm-d-router''s own version line, not the meta-repo tag).
  ''About'' tagline: ''Achieve state of the art inference performance with modern
  accelerators on Kubernetes''; GitHub topics include intelligent-routing, distributed-inference,
  kubernetes, model-server. ''Latest News'' timeline confirms dated milestones: 2025-12
  v0.4 (DeepSeek V3.1 40% TTFT reduction on H200, Intel XPU/Google TPU disaggregation,
  prefix-cache-to-CPU well-lit path); 2026-02 v0.5 (reproducible benchmarks, hierarchical
  KV offloading, cache-aware LoRA routing, active-active HA, UCCL transport resilience,
  scale-to-zero autoscaling); 2026-03 llm-d joins CNCF as a Sandbox project, founded
  by Red Hat, Google Cloud, IBM Research, CoreWeave, and NVIDIA, with support from
  AMD, Cisco, Hugging Face, Intel, Lambda, Mistral AI, UC Berkeley, University of
  Chicago; 2026-05 v0.7 (optimized-baseline rename/stabilization, kustomize-first
  guide migration, predicted-latency scheduling GA, ''batch gateway (experimental)''
  -- this is the same release that made standalone mode the deployment default per
  the releases page, though this particular blurb doesn''t mention that UX change
  explicitly). README recommends starting from the ''Optimized Baseline'' well-lit
  path and directs Helm-chart/component-release details to the GitHub Releases page.
  No standalone-EPP or non-Kubernetes/FileDiscovery language appears in this top-level
  README, reinforcing that non-k8s-backend support is documented at the component/release-notes
  level (llm-d-router, GIE) rather than in llm-d''s headline project description.'
utility_score: 12.0
---

[Skip to content](https://github.com/llm-d/llm-d#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d) You must be signed in to change notification settings
  * [ Fork 761 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)
  * [ Star  4.5k ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)


[**18** Branches](https://github.com/llm-d/llm-d/branches)[](https://github.com/llm-d/llm-d/tags)
Go to file
Open more actions menu
## Latest commit
and
[docs(api): document requestHandler.parsers as a list (](https://github.com/llm-d/llm-d/commit/3a82b81853c328e849829376777c2d663e813ced)[#2382](https://github.com/llm-d/llm-d/pull/2382)[)](https://github.com/llm-d/llm-d/commit/3a82b81853c328e849829376777c2d663e813ced)
Open commit detailssuccess
Sep 11, 2026
[3a82b81](https://github.com/llm-d/llm-d/commit/3a82b81853c328e849829376777c2d663e813ced) · Sep 11, 2026
## History
[1,425 Commits](https://github.com/llm-d/llm-d/commits/main/)
Open commit details
1,425 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   | [Temporarily disabled scheduled execution of all OpenShit (IBM) workfl…](https://github.com/llm-d/llm-d/commit/1f031c2aca169e0887475d861009a5c180e031ad "Temporarily disabled scheduled execution of all OpenShit \(IBM\) workflows \(#2477\)

Signed-off-by: maugustosilva <maugusto.silva@gmail.com>")  | Sep 11, 2026  |  
|   |   |  [feat(snapshot): add GKE fast pod snapshot provider and vLLM wrapper (](https://github.com/llm-d/llm-d/commit/0c771253874ab8099638c261570dabc9eb0dc7ab "feat\(snapshot\): add GKE fast pod snapshot provider and vLLM wrapper \(#2105\)

* feat\(snapshot\): add GKE fast pod snapshot provider and vLLM wrapper

Add container snapshot provider support and vLLM lifespan wrapper for GKE Fast Pod Snapshotting:
- GKESnapshotProvider for gVisor procfs checkpointing and selective model weight cache purging.
- Modular vLLM lifespan context manager wrapper for VRAM sleep/wake lifecycle.
- Comprehensive unit test suite.

Signed-off-by: Ryan Rosario <6713180+RyanRosario@users.noreply.github.com>

* Switch from using StreamHandler to vllm.logger.init_logger

Signed-off-by: Ryan Rosario <6713180+RyanRosario@users.noreply.github.com>

* Strip down README file to remove user guide and configuration sections

Signed-off-by: Ryan Rosario <6713180+RyanRosario@users.noreply.github.com>

* Changing logger to be part of vllm hierarchy

Signed-off-by: Ryan Rosario <6713180+RyanRosario@users.noreply.github.com>

* log warning when api_server cannot be hooked in launcher

Signed-off-by: Ryan Rosario <6713180+RyanRosario@users.noreply.github.com>

---------

Signed-off-by: Ryan Rosario <6713180+RyanRosario@users.noreply.github.com>
Co-authored-by: Ryan Rosario <6713180+RyanRosario@users.noreply.github.com>")[#…](https://github.com/llm-d/llm-d/pull/2105)  | Sep 9, 2026  |  
|  [docs(api): document requestHandler.parsers as a list (](https://github.com/llm-d/llm-d/commit/3a82b81853c328e849829376777c2d663e813ced "docs\(api\): document requestHandler.parsers as a list \(#2382\)

RequestHandlerConfig in the EPP schema has a single field, Parsers
\(json tag "parsers"\), a list of ParserConfig. The API reference
documented a singular parser object, contradicting the gRPC guide.

Update the RequestHandlerConfig table and the configuration guide to
the list form, and note the deprecated top-level parser field with its
precedence rule.

Verified against llm-d-router v0.10.0 \(commit 0b43c187\)
apix/config/v1alpha1/endpointpickerconfig_types.go lines 320-325
\(RequestHandlerConfig.Parsers\) and 72-78 \(deprecated top-level Parser\).

Fixes #2355

Signed-off-by: Rishabh Sinha <rsinha17@terpmail.umd.edu>
Co-authored-by: Claude Fable 5 <noreply@anthropic.com>")[#2382](https://github.com/llm-d/llm-d/pull/2382)[)](https://github.com/llm-d/llm-d/commit/3a82b81853c328e849829376777c2d663e813ced "docs\(api\): document requestHandler.parsers as a list \(#2382\)

RequestHandlerConfig in the EPP schema has a single field, Parsers
\(json tag "parsers"\), a list of ParserConfig. The API reference
documented a singular parser object, contradicting the gRPC guide.

Update the RequestHandlerConfig table and the configuration guide to
the list form, and note the deprecated top-level parser field with its
precedence rule.

Verified against llm-d-router v0.10.0 \(commit 0b43c187\)
apix/config/v1alpha1/endpointpickerconfig_types.go lines 320-325
\(RequestHandlerConfig.Parsers\) and 72-78 \(deprecated top-level Parser\).

Fixes #2355

Signed-off-by: Rishabh Sinha <rsinha17@terpmail.umd.edu>
Co-authored-by: Claude Fable 5 <noreply@anthropic.com>")  | Sep 11, 2026  |  
|   |   |  [feat(guides): add diffusion-serving text-to-speech guide (](https://github.com/llm-d/llm-d/commit/bcf51c07d14421b4dc7f36ca32c48f5149f72999 "feat\(guides\): add diffusion-serving text-to-speech guide \(#2425\)

Signed-off-by: Bob Tian <bobzetian@google.com>")[#2425](https://github.com/llm-d/llm-d/pull/2425)[)](https://github.com/llm-d/llm-d/commit/bcf51c07d14421b4dc7f36ca32c48f5149f72999 "feat\(guides\): add diffusion-serving text-to-speech guide \(#2425\)

Signed-off-by: Bob Tian <bobzetian@google.com>")  | Sep 11, 2026  |  
|   |   |  [fix: preserve healthcheck failure accounting (](https://github.com/llm-d/llm-d/commit/e09a0816f4d5099d8524e273819de53f62473f0b "fix: preserve healthcheck failure accounting \(#2401\)

Signed-off-by: git-jxj <xinjun.jiang@daocloud.io>")[#2401](https://github.com/llm-d/llm-d/pull/2401)[)](https://github.com/llm-d/llm-d/commit/e09a0816f4d5099d8524e273819de53f62473f0b "fix: preserve healthcheck failure accounting \(#2401\)

Signed-off-by: git-jxj <xinjun.jiang@daocloud.io>")  | Sep 8, 2026  |  
|   |   |   | Apr 29, 2025  |  
|   |   |  [updating images preparing for v0.9.0 release (](https://github.com/llm-d/llm-d/commit/f8f03956eba3e170bf8218ced717da9ad785fdf7 "updating images preparing for v0.9.0 release \(#2206\)

* updating images preparing for v0.9.0 release

changes:
- remove gb200 images
- remove offloading connector as its landed in vllm
- remove lmcache and inifistore build \(exists in vLLM kvconnectors requirements / image\)
- bump images to match vllm v0.26.0
- drop old nvshmem patches that are no longer applicable \(kept the one for 3.4.5 as we sometimes build against that target\)
- dont build flashinfer from source

* updating helper files \(makefile, localbuilder\)

* rocm setuptools fix + cmake retry for network flake

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")[#2206](https://github.com/llm-d/llm-d/pull/2206)[)](https://github.com/llm-d/llm-d/commit/f8f03956eba3e170bf8218ced717da9ad785fdf7 "updating images preparing for v0.9.0 release \(#2206\)

* updating images preparing for v0.9.0 release

changes:
- remove gb200 images
- remove offloading connector as its landed in vllm
- remove lmcache and inifistore build \(exists in vLLM kvconnectors requirements / image\)
- bump images to match vllm v0.26.0
- drop old nvshmem patches that are no longer applicable \(kept the one for 3.4.5 as we sometimes build against that target\)
- dont build flashinfer from source

* updating helper files \(makefile, localbuilder\)

* rocm setuptools fix + cmake retry for network flake

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")  | Aug 13, 2026  |  
|   |   |   | Aug 14, 2026  |  
|   |   |  [Expand and update the release matrix (](https://github.com/llm-d/llm-d/commit/026f8cfcc9dfed34718347a87f7a76762860babf "Expand and update the release matrix \(#2471\)

Signed-off-by: Diego-Castan <diego.castan@ibm.com>")[#2471](https://github.com/llm-d/llm-d/pull/2471)[)](https://github.com/llm-d/llm-d/commit/026f8cfcc9dfed34718347a87f7a76762860babf "Expand and update the release matrix \(#2471\)

Signed-off-by: Diego-Castan <diego.castan@ibm.com>")  | Sep 10, 2026  |  
|   |   |  [docs: fix ModelExpress base image component path (](https://github.com/llm-d/llm-d/commit/035671b4b92e5f36f6884593b90325a171168cb9 "docs: fix ModelExpress base image component path \(#2451\)

Signed-off-by: git-jxj <65210887+git-jxj@users.noreply.github.com>
Signed-off-by: git-jxj <xinjun.jiang@daocloud.io>")[#2451](https://github.com/llm-d/llm-d/pull/2451)[)](https://github.com/llm-d/llm-d/commit/035671b4b92e5f36f6884593b90325a171168cb9 "docs: fix ModelExpress base image component path \(#2451\)

Signed-off-by: git-jxj <65210887+git-jxj@users.noreply.github.com>
Signed-off-by: git-jxj <xinjun.jiang@daocloud.io>")  | Sep 11, 2026  |  
|   |   |  [Arm cuda support (from clean branch) (](https://github.com/llm-d/llm-d/commit/ca2d7124a2e4f6082328424be4b27de757100832 "Arm cuda support \(from clean branch\) \(#382\)

* restore

Signed-off-by: Will Eaton <weaton@redhat.com>

* remove again

Signed-off-by: Will Eaton <weaton@redhat.com>

* support workflow dispatch like in PR #362

Signed-off-by: Will Eaton <weaton@redhat.com>

* Add the workflow_dispatch for xpu ci

Signed-off-by: yuanwu <yuan.wu@intel.com>

* more revert fixes

Signed-off-by: Will Eaton <weaton@redhat.com>

* remove

Signed-off-by: Will Eaton <weaton@redhat.com>

* more revert

Signed-off-by: Will Eaton <weaton@redhat.com>

* moar

Signed-off-by: Will Eaton <weaton@redhat.com>

* fixup ld path

Signed-off-by: Will Eaton <weaton@redhat.com>

* multi-stage ubuntu versioning

Signed-off-by: Will Eaton <weaton@redhat.com>

* reviewer comments round 1

Signed-off-by: Will Eaton <weaton@redhat.com>

* short SHA instead of truncate

Signed-off-by: Will Eaton <weaton@redhat.com>

* skip pre-commit on WD

Signed-off-by: Will Eaton <weaton@redhat.com>

* use uv for python

Signed-off-by: Will Eaton <weaton@redhat.com>

* fixup install dir

Signed-off-by: Will Eaton <weaton@redhat.com>

* fixup

Signed-off-by: Will Eaton <weaton@redhat.com>

* fixup homedir

Signed-off-by: Will Eaton <weaton@redhat.com>

* fix precompiled wheel by stripping cu129; install newer cmake

Signed-off-by: Will Eaton <weaton@redhat.com>

* fix artifact path

Signed-off-by: Will Eaton <weaton@redhat.com>

* temporarily workaround the pip findlinks issue with the bad vllm index

Signed-off-by: Will Eaton <weaton@redhat.com>

* fix bash/sh issue

Signed-off-by: Will Eaton <weaton@redhat.com>

* whitespace

Signed-off-by: Will Eaton <weaton@redhat.com>

* private arm runner

Signed-off-by: Will Eaton <weaton@redhat.com>

* default to 7 char short sha

Signed-off-by: Will Eaton <weaton@redhat.com>

* fixup caching

Signed-off-by: Will Eaton <weaton@redhat.com>

* refactor to common and accelerator specific pacakge manifests

Signed-off-by: Will Eaton <weaton@redhat.com>

* add more runtime packages

Signed-off-by: Will Eaton <weaton@redhat.com>

* dev tag cache; remove free space action

Signed-off-by: Will Eaton <weaton@redhat.com>

* try symlinks for python-devel headers

Signed-off-by: Will Eaton <weaton@redhat.com>

* push to image suffix correctly

Signed-off-by: Will Eaton <weaton@redhat.com>

* fix manifest path as well

Signed-off-by: Will Eaton <weaton@redhat.com>

* using coreweave overlay

Signed-off-by: greg pereira <grpereir@redhat.com>

* testing and fixes

Signed-off-by: greg pereira <grpereir@redhat.com>

* ubuntu python packages

Signed-off-by: greg pereira <grpereir@redhat.com>

* remove python packages from builder stage, add devel headers to final stage

Signed-off-by: greg pereira <grpereir@redhat.com>

* updating commit sha

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: Will Eaton <weaton@redhat.com>
Signed-off-by: yuanwu <yuan.wu@intel.com>
Signed-off-by: greg pereira <grpereir@redhat.com>
Co-authored-by: yuanwu <yuan.wu@intel.com>
Co-authored-by: greg pereira <grpereir@redhat.com>")[#382](https://github.com/llm-d/llm-d/pull/382)[)](https://github.com/llm-d/llm-d/commit/ca2d7124a2e4f6082328424be4b27de757100832 "Arm cuda support \(from clean branch\) \(#382\)

* restore

Signed-off-by: Will Eaton <weaton@redhat.com>

* remove again

Signed-off-by: Will Eaton <weaton@redhat.com>

* support workflow dispatch like in PR #362

Signed-off-by: Will Eaton <weaton@redhat.com>

* Add the workflow_dispatch for xpu ci

Signed-off-by: yuanwu <yuan.wu@intel.com>

* more revert fixes

Signed-off-by: Will Eaton <weaton@redhat.com>

* remove

Signed-off-by: Will Eaton <weaton@redhat.com>

* more revert

Signed-off-by: Will Eaton <weaton@redhat.com>

* moar

Signed-off-by: Will Eaton <weaton@redhat.com>

* fixup ld path

Signed-off-by: Will Eaton <weaton@redhat.com>

* multi-stage ubuntu versioning

Signed-off-by: Will Eaton <weaton@redhat.com>

* reviewer comments round 1

Signed-off-by: Will Eaton <weaton@redhat.com>

* short SHA instead of truncate

Signed-off-by: Will Eaton <weaton@redhat.com>

* skip pre-commit on WD

Signed-off-by: Will Eaton <weaton@redhat.com>

* use uv for python

Signed-off-by: Will Eaton <weaton@redhat.com>

* fixup install dir

Signed-off-by: Will Eaton <weaton@redhat.com>

* fixup

Signed-off-by: Will Eaton <weaton@redhat.com>

* fixup homedir

Signed-off-by: Will Eaton <weaton@redhat.com>

* fix precompiled wheel by stripping cu129; install newer cmake

Signed-off-by: Will Eaton <weaton@redhat.com>

* fix artifact path

Signed-off-by: Will Eaton <weaton@redhat.com>

* temporarily workaround the pip findlinks issue with the bad vllm index

Signed-off-by: Will Eaton <weaton@redhat.com>

* fix bash/sh issue

Signed-off-by: Will Eaton <weaton@redhat.com>

* whitespace

Signed-off-by: Will Eaton <weaton@redhat.com>

* private arm runner

Signed-off-by: Will Eaton <weaton@redhat.com>

* default to 7 char short sha

Signed-off-by: Will Eaton <weaton@redhat.com>

* fixup caching

Signed-off-by: Will Eaton <weaton@redhat.com>

* refactor to common and accelerator specific pacakge manifests

Signed-off-by: Will Eaton <weaton@redhat.com>

* add more runtime packages

Signed-off-by: Will Eaton <weaton@redhat.com>

* dev tag cache; remove free space action

Signed-off-by: Will Eaton <weaton@redhat.com>

* try symlinks for python-devel headers

Signed-off-by: Will Eaton <weaton@redhat.com>

* push to image suffix correctly

Signed-off-by: Will Eaton <weaton@redhat.com>

* fix manifest path as well

Signed-off-by: Will Eaton <weaton@redhat.com>

* using coreweave overlay

Signed-off-by: greg pereira <grpereir@redhat.com>

* testing and fixes

Signed-off-by: greg pereira <grpereir@redhat.com>

* ubuntu python packages

Signed-off-by: greg pereira <grpereir@redhat.com>

* remove python packages from builder stage, add devel headers to final stage

Signed-off-by: greg pereira <grpereir@redhat.com>

* updating commit sha

Signed-off-by: greg pereira <grpereir@redhat.com>

---------

Signed-off-by: Will Eaton <weaton@redhat.com>
Signed-off-by: yuanwu <yuan.wu@intel.com>
Signed-off-by: greg pereira <grpereir@redhat.com>
Co-authored-by: yuanwu <yuan.wu@intel.com>
Co-authored-by: greg pereira <grpereir@redhat.com>")  | Oct 27, 2025  |  
|   |   |  [Add .editorconfig for consistent formatting (](https://github.com/llm-d/llm-d/commit/f4b8e346981c93c40f16bac2f1f0e785fae053b2 "Add .editorconfig for consistent formatting \(#2103\)

Signed-off-by: arijitroy003 <arijitroy003@gmail.com>")[#2103](https://github.com/llm-d/llm-d/pull/2103)[)](https://github.com/llm-d/llm-d/commit/f4b8e346981c93c40f16bac2f1f0e785fae053b2 "Add .editorconfig for consistent formatting \(#2103\)

Signed-off-by: arijitroy003 <arijitroy003@gmail.com>")  | Jul 28, 2026  |  
|   |   |  [action lint plus fix lint errors (](https://github.com/llm-d/llm-d/commit/8c6278438afd8d54b1a89afc8c38e0cff5408792 "action lint plus fix lint errors \(#1584\)

Signed-off-by: Diego-Castan <diego.castan@ibm.com>
Signed-off-by: Diego Castan <diego.castan@ibm.com>")[#1584](https://github.com/llm-d/llm-d/pull/1584)[)](https://github.com/llm-d/llm-d/commit/8c6278438afd8d54b1a89afc8c38e0cff5408792 "action lint plus fix lint errors \(#1584\)

Signed-off-by: Diego-Castan <diego.castan@ibm.com>
Signed-off-by: Diego Castan <diego.castan@ibm.com>")  | May 26, 2026  |  
|   |   |  [Image component resturcture and v0.26.0 bump (](https://github.com/llm-d/llm-d/commit/ada02c1bdd2aa8a8cf12d2b6a05e671314c983b5 "Image component resturcture and v0.26.0 bump \(#2260\)

* base image component restructure + bumps

* composable image component restructure + image bumps

* reset optimized baseline to use none gateway option

* guide fixes + update kustomize dry-run

* update kustomize dry run summary

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")[#2260](https://github.com/llm-d/llm-d/pull/2260)[)](https://github.com/llm-d/llm-d/commit/ada02c1bdd2aa8a8cf12d2b6a05e671314c983b5 "Image component resturcture and v0.26.0 bump \(#2260\)

* base image component restructure + bumps

* composable image component restructure + image bumps

* reset optimized baseline to use none gateway option

* guide fixes + update kustomize dry-run

* update kustomize dry run summary

---------

Signed-off-by: greg pereira <grpereir@redhat.com>")  | Aug 13, 2026  |  
|   |   |  [refactor dockerfiles to a set bash scripts (](https://github.com/llm-d/llm-d/commit/41eaf261d6091067f0db11dd2e1c53c57df43330 "refactor dockerfiles to a set bash scripts \(#324\)

* initial refactor to bash scripts

Signed-off-by: Will Eaton <weaton@redhat.com>

* runtime swap

Signed-off-by: Will Eaton <weaton@redhat.com>

* fix scripts

Signed-off-by: Will Eaton <weaton@redhat.com>

* concurrency

Signed-off-by: Will Eaton <weaton@redhat.com>

* fixup

Signed-off-by: Will Eaton <weaton@redhat.com>

* switch to prek

Signed-off-by: Will Eaton <weaton@redhat.com>

* rebase fixups

Signed-off-by: Will Eaton <weaton@redhat.com>

* single uv pip install command

Signed-off-by: Will Eaton <weaton@redhat.com>

* not all files

Signed-off-by: Will Eaton <weaton@redhat.com>

* reviewer feedback

Signed-off-by: Will Eaton <weaton@redhat.com>

* add env var declaration for scripts and lint

* remove dev branch

Signed-off-by: Will Eaton <weaton@redhat.com>

* unravel

Signed-off-by: Will Eaton <weaton@redhat.com>

* Update docker/scripts/cuda/builder/build-gdrcopy.sh

Co-authored-by: Greg Pereira <grpereir@redhat.com>
Signed-off-by: Will Eaton <wseaton@users.noreply.github.com>

* debug print installed packages

Signed-off-by: Will Eaton <weaton@redhat.com>

* start building aarch64

Signed-off-by: Will Eaton <weaton@redhat.com>

* fix nvidia repo

Signed-off-by: Will Eaton <weaton@redhat.com>

* platform fixes; pipefail considered harmful

Signed-off-by: Will Eaton <weaton@redhat.com>

* skip EP libraries for now

Signed-off-by: Will Eaton <weaton@redhat.com>

* split and combine

Signed-off-by: Will Eaton <weaton@redhat.com>

* split

Signed-off-by: Will Eaton <weaton@redhat.com>

* fix push step

Signed-off-by: Will Eaton <weaton@redhat.com>

* sccache arm fixes

Signed-off-by: Will Eaton <weaton@redhat.com>

* make warnings, not errors for env var lints

Signed-off-by: Will Eaton <weaton@redhat.com>

* various comments

Signed-off-by: Will Eaton <weaton@redhat.com>

* support workflow dispatch like in PR #362

Signed-off-by: Will Eaton <weaton@redhat.com>

* delete file

Signed-off-by: Will Eaton <weaton@redhat.com>

* remove arm from matrix for now

Signed-off-by: Will Eaton <weaton@redhat.com>

---------

Signed-off-by: Will Eaton <weaton@redhat.com>
Signed-off-by: Will Eaton <wseaton@users.noreply.github.com>
Co-authored-by: Greg Pereira <grpereir@redhat.com>")[#324](https://github.com/llm-d/llm-d/pull/324)[)](https://github.com/llm-d/llm-d/commit/41eaf261d6091067f0db11dd2e1c53c57df43330 "refactor dockerfiles to a set bash scripts \(#324\)

* initial refactor to bash scripts

Signed-off-by: Will Eaton <weaton@redhat.com>

* runtime swap

Signed-off-by: Will Eaton <weaton@redhat.com>

* fix scripts

Signed-off-by: Will Eaton <weaton@redhat.com>

* concurrency

Signed-off-by: Will Eaton <weaton@redhat.com>

* fixup

Signed-off-by: Will Eaton <weaton@redhat.com>

* switch to prek

Signed-off-by: Will Eaton <weaton@redhat.com>

* rebase fixups

Signed-off-by: Will Eaton <weaton@redhat.com>

* single uv pip install command

Signed-off-by: Will Eaton <weaton@redhat.com>

* not all files

Signed-off-by: Will Eaton <weaton@redhat.com>

* reviewer feedback

Signed-off-by: Will Eaton <weaton@redhat.com>

* add env var declaration for scripts and lint

* remove dev branch

Signed-off-by: Will Eaton <weaton@redhat.com>

* unravel

Signed-off-by: Will Eaton <weaton@redhat.com>

* Update docker/scripts/cuda/builder/build-gdrcopy.sh

Co-authored-by: Greg Pereira <grpereir@redhat.com>
Signed-off-by: Will Eaton <wseaton@users.noreply.github.com>

* debug print installed packages

Signed-off-by: Will Eaton <weaton@redhat.com>

* start building aarch64

Signed-off-by: Will Eaton <weaton@redhat.com>

* fix nvidia repo

Signed-off-by: Will Eaton <weaton@redhat.com>

* platform fixes; pipefail considered harmful

Signed-off-by: Will Eaton <weaton@redhat.com>

* skip EP libraries for now

Signed-off-by: Will Eaton <weaton@redhat.com>

* split and combine

Signed-off-by: Will Eaton <weaton@redhat.com>

* split

Signed-off-by: Will Eaton <weaton@redhat.com>

* fix push step

Signed-off-by: Will Eaton <weaton@redhat.com>

* sccache arm fixes

Signed-off-by: Will Eaton <weaton@redhat.com>

* make warnings, not errors for env var lints

Signed-off-by: Will Eaton <weaton@redhat.com>

* various comments

Signed-off-by: Will Eaton <weaton@redhat.com>

* support workflow dispatch like in PR #362

Signed-off-by: Will Eaton <weaton@redhat.com>

* delete file

Signed-off-by: Will Eaton <weaton@redhat.com>

* remove arm from matrix for now

Signed-off-by: Will Eaton <weaton@redhat.com>

---------

Signed-off-by: Will Eaton <weaton@redhat.com>
Signed-off-by: Will Eaton <wseaton@users.noreply.github.com>
Co-authored-by: Greg Pereira <grpereir@redhat.com>")  | Oct 21, 2025  |  
|   |   |  [fix[lint]: various kinds of lint error+warning (](https://github.com/llm-d/llm-d/commit/37771f348d3888a3a093741699a74e09e50b955a "fix\[lint\]: various kinds of lint error+warning \(#645\)

* fix: all yaml lint error

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: dockerfile lint error

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: lint whitespace,end-of-file etc

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: markdown lint error and disable check on MD033

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update:

- only whitelist certain keywords than skip MD033 entirely
- fix long length and missing newline in markdown

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: fix rebase and more markdown fix

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")[#645](https://github.com/llm-d/llm-d/pull/645)[)](https://github.com/llm-d/llm-d/commit/37771f348d3888a3a093741699a74e09e50b955a "fix\[lint\]: various kinds of lint error+warning \(#645\)

* fix: all yaml lint error

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: dockerfile lint error

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: lint whitespace,end-of-file etc

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: markdown lint error and disable check on MD033

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update:

- only whitelist certain keywords than skip MD033 entirely
- fix long length and missing newline in markdown

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: fix rebase and more markdown fix

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")  | Jan 29, 2026  |  
| [.pre-commit-config.yaml](https://github.com/llm-d/llm-d/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  | [.pre-commit-config.yaml](https://github.com/llm-d/llm-d/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  |  [action lint plus fix lint errors (](https://github.com/llm-d/llm-d/commit/8c6278438afd8d54b1a89afc8c38e0cff5408792 "action lint plus fix lint errors \(#1584\)

Signed-off-by: Diego-Castan <diego.castan@ibm.com>
Signed-off-by: Diego Castan <diego.castan@ibm.com>")[#1584](https://github.com/llm-d/llm-d/pull/1584)[)](https://github.com/llm-d/llm-d/commit/8c6278438afd8d54b1a89afc8c38e0cff5408792 "action lint plus fix lint errors \(#1584\)

Signed-off-by: Diego-Castan <diego.castan@ibm.com>
Signed-off-by: Diego Castan <diego.castan@ibm.com>")  | May 26, 2026  |  
|   |   |   | Dec 3, 2025  |  
|   |   |   | Jul 10, 2026  |  
|   |   |  [chore: pre-commint/lint and github workflow for build (](https://github.com/llm-d/llm-d/commit/3c0650b10ee4e13f29716714868710d0bf058aeb "chore: pre-commint/lint and github workflow for build \(#568\)

* chore: pre-commint/lint and github workflow for build

- Update pre-commit tool version and docs
- fix lint found by yaml,markdown,hadolint
- threat model link is broken, it has been removed recently
- use lychee.toml for github and markdownlint-cli for local, add config
  files
- remove .markdown-lint-check and its config file
- set all github script to use pipefail
- remove strategy which is not used for xpu and cpu
- add more path filters to trigger build
- use github action build-push-action than make target for xpu and cpu
  build
- create new stage 'set-tag' to capture tags on docker image
- remove sccache.config.toml, which is not in use

- fix for the markdown link
  creds to @petecheslock https://github.com/llm-d/llm-d/commit/f0c603b72312dfd6a1d90d34a194da9606963971

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* lint: fix some markdown by pre-commit

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* chore: update link checker configuration and documentation

- Added exclusions for Google Docs fragments and GitHub blob line anchors in lychee.toml to improve link checking reliability.
- Updated CI workflow to include fragment checking for markdown links.
- Revised README documentation for accelerator vendors and gateway options to fix broken anchors in other docs

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>

* fix: update link in monitoring README for consistency

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>

* fix: update links in README for accuracy and consistency

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>

* update: code review

- fix typo which should onlly use @ for SHA! and : for tag/branch

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>
Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>
Co-authored-by: Pete Cheslock <pete.cheslock@redhat.com>")[#568](https://github.com/llm-d/llm-d/pull/568)[)](https://github.com/llm-d/llm-d/commit/3c0650b10ee4e13f29716714868710d0bf058aeb "chore: pre-commint/lint and github workflow for build \(#568\)

* chore: pre-commint/lint and github workflow for build

- Update pre-commit tool version and docs
- fix lint found by yaml,markdown,hadolint
- threat model link is broken, it has been removed recently
- use lychee.toml for github and markdownlint-cli for local, add config
  files
- remove .markdown-lint-check and its config file
- set all github script to use pipefail
- remove strategy which is not used for xpu and cpu
- add more path filters to trigger build
- use github action build-push-action than make target for xpu and cpu
  build
- create new stage 'set-tag' to capture tags on docker image
- remove sccache.config.toml, which is not in use

- fix for the markdown link
  creds to @petecheslock https://github.com/llm-d/llm-d/commit/f0c603b72312dfd6a1d90d34a194da9606963971

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* lint: fix some markdown by pre-commit

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* chore: update link checker configuration and documentation

- Added exclusions for Google Docs fragments and GitHub blob line anchors in lychee.toml to improve link checking reliability.
- Updated CI workflow to include fragment checking for markdown links.
- Revised README documentation for accelerator vendors and gateway options to fix broken anchors in other docs

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>

* fix: update link in monitoring README for consistency

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>

* fix: update links in README for accuracy and consistency

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>

* update: code review

- fix typo which should onlly use @ for SHA! and : for tag/branch

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>
Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>
Co-authored-by: Pete Cheslock <pete.cheslock@redhat.com>")  | Jan 15, 2026  |  
|   |   | [docs(contributing): Add CNCF license policy requirement for new depen…](https://github.com/llm-d/llm-d/commit/51ae977ca3332d4869ba52bbd1ee246b51e21739 "docs\(contributing\): Add CNCF license policy requirement for new dependencies \(#2275\)

* docs\(contributing\): Add CNCF license policy requirement for new dependencies

Explicitly state that all new external dependencies must comply with the CNCF Allowed License Policy. This clarifies expectations for contributors.

* Potential fix for pull request finding

Co-authored-by: Copilot Autofix powered by AI <175728472+Copilot@users.noreply.github.com>
Signed-off-by: David G. Simmons <davidgs@users.noreply.github.com>

---------

Signed-off-by: David G. Simmons <santafen@mac.com>
Signed-off-by: David G. Simmons <davidgs@users.noreply.github.com>
Co-authored-by: Copilot Autofix powered by AI <175728472+Copilot@users.noreply.github.com>")  | Aug 13, 2026  |  
|  [fix[lint]: various kinds of lint error+warning (](https://github.com/llm-d/llm-d/commit/37771f348d3888a3a093741699a74e09e50b955a "fix\[lint\]: various kinds of lint error+warning \(#645\)

* fix: all yaml lint error

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: dockerfile lint error

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: lint whitespace,end-of-file etc

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: markdown lint error and disable check on MD033

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update:

- only whitelist certain keywords than skip MD033 entirely
- fix long length and missing newline in markdown

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: fix rebase and more markdown fix

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")[#645](https://github.com/llm-d/llm-d/pull/645)[)](https://github.com/llm-d/llm-d/commit/37771f348d3888a3a093741699a74e09e50b955a "fix\[lint\]: various kinds of lint error+warning \(#645\)

* fix: all yaml lint error

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: dockerfile lint error

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: lint whitespace,end-of-file etc

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: markdown lint error and disable check on MD033

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update:

- only whitelist certain keywords than skip MD033 entirely
- fix long length and missing newline in markdown

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: fix rebase and more markdown fix

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")  | Jan 29, 2026  |  
|   |   |   | Apr 29, 2025  |  
|   |   |  [Add Community Managers section to MAINTAINERS.md (](https://github.com/llm-d/llm-d/commit/2c5340f5c9ba5082fba0e331bfee32ae2e1526aa "Add Community Managers section to MAINTAINERS.md \(#1736\)

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>")[#1736](https://github.com/llm-d/llm-d/pull/1736)[)](https://github.com/llm-d/llm-d/commit/2c5340f5c9ba5082fba0e331bfee32ae2e1526aa "Add Community Managers section to MAINTAINERS.md \(#1736\)

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>")  | Jun 11, 2026  |  
|   |   |  [ci: verify rendered guide READMEs (](https://github.com/llm-d/llm-d/commit/f5ddf59769000a34380801d2229fef4d6866c0bb "ci: verify rendered guide READMEs \(#2384\)

Signed-off-by: Kay Yan <kay.yan@daocloud.io>")[#2384](https://github.com/llm-d/llm-d/pull/2384)[)](https://github.com/llm-d/llm-d/commit/f5ddf59769000a34380801d2229fef4d6866c0bb "ci: verify rendered guide READMEs \(#2384\)

Signed-off-by: Kay Yan <kay.yan@daocloud.io>")  | Sep 1, 2026  |  
|   |   |  [chore: pre-commint/lint and github workflow for build (](https://github.com/llm-d/llm-d/commit/3c0650b10ee4e13f29716714868710d0bf058aeb "chore: pre-commint/lint and github workflow for build \(#568\)

* chore: pre-commint/lint and github workflow for build

- Update pre-commit tool version and docs
- fix lint found by yaml,markdown,hadolint
- threat model link is broken, it has been removed recently
- use lychee.toml for github and markdownlint-cli for local, add config
  files
- remove .markdown-lint-check and its config file
- set all github script to use pipefail
- remove strategy which is not used for xpu and cpu
- add more path filters to trigger build
- use github action build-push-action than make target for xpu and cpu
  build
- create new stage 'set-tag' to capture tags on docker image
- remove sccache.config.toml, which is not in use

- fix for the markdown link
  creds to @petecheslock https://github.com/llm-d/llm-d/commit/f0c603b72312dfd6a1d90d34a194da9606963971

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* lint: fix some markdown by pre-commit

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* chore: update link checker configuration and documentation

- Added exclusions for Google Docs fragments and GitHub blob line anchors in lychee.toml to improve link checking reliability.
- Updated CI workflow to include fragment checking for markdown links.
- Revised README documentation for accelerator vendors and gateway options to fix broken anchors in other docs

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>

* fix: update link in monitoring README for consistency

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>

* fix: update links in README for accuracy and consistency

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>

* update: code review

- fix typo which should onlly use @ for SHA! and : for tag/branch

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>
Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>
Co-authored-by: Pete Cheslock <pete.cheslock@redhat.com>")[#568](https://github.com/llm-d/llm-d/pull/568)[)](https://github.com/llm-d/llm-d/commit/3c0650b10ee4e13f29716714868710d0bf058aeb "chore: pre-commint/lint and github workflow for build \(#568\)

* chore: pre-commint/lint and github workflow for build

- Update pre-commit tool version and docs
- fix lint found by yaml,markdown,hadolint
- threat model link is broken, it has been removed recently
- use lychee.toml for github and markdownlint-cli for local, add config
  files
- remove .markdown-lint-check and its config file
- set all github script to use pipefail
- remove strategy which is not used for xpu and cpu
- add more path filters to trigger build
- use github action build-push-action than make target for xpu and cpu
  build
- create new stage 'set-tag' to capture tags on docker image
- remove sccache.config.toml, which is not in use

- fix for the markdown link
  creds to @petecheslock https://github.com/llm-d/llm-d/commit/f0c603b72312dfd6a1d90d34a194da9606963971

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* lint: fix some markdown by pre-commit

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* chore: update link checker configuration and documentation

- Added exclusions for Google Docs fragments and GitHub blob line anchors in lychee.toml to improve link checking reliability.
- Updated CI workflow to include fragment checking for markdown links.
- Revised README documentation for accelerator vendors and gateway options to fix broken anchors in other docs

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>

* fix: update link in monitoring README for consistency

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>

* fix: update links in README for accuracy and consistency

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>

* update: code review

- fix typo which should onlly use @ for SHA! and : for tag/branch

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>
Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>
Co-authored-by: Pete Cheslock <pete.cheslock@redhat.com>")  | Jan 15, 2026  |  
|   |   |  [Add davidgs to the OWNERS file (](https://github.com/llm-d/llm-d/commit/7391ed1499069b78e49ab1378cd1007a3c9b50e3 "Add davidgs to the OWNERS file \(#2042\)

Since the docs and the tooling to build the docs lives here, it might be helpful if I can review/approve PRs that relate to those aspects.

Signed-off-by: David G. Simmons <davidgs@users.noreply.github.com>")[#2042](https://github.com/llm-d/llm-d/pull/2042)[)](https://github.com/llm-d/llm-d/commit/7391ed1499069b78e49ab1378cd1007a3c9b50e3 "Add davidgs to the OWNERS file \(#2042\)

Since the docs and the tooling to build the docs lives here, it might be helpful if I can review/approve PRs that relate to those aspects.

Signed-off-by: David G. Simmons <davidgs@users.noreply.github.com>")  | Aug 12, 2026  |  
|   |   |   | Apr 30, 2026  |  
|   |   |  [fix[lint]: various kinds of lint error+warning (](https://github.com/llm-d/llm-d/commit/37771f348d3888a3a093741699a74e09e50b955a "fix\[lint\]: various kinds of lint error+warning \(#645\)

* fix: all yaml lint error

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: dockerfile lint error

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: lint whitespace,end-of-file etc

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: markdown lint error and disable check on MD033

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update:

- only whitelist certain keywords than skip MD033 entirely
- fix long length and missing newline in markdown

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: fix rebase and more markdown fix

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")[#645](https://github.com/llm-d/llm-d/pull/645)[)](https://github.com/llm-d/llm-d/commit/37771f348d3888a3a093741699a74e09e50b955a "fix\[lint\]: various kinds of lint error+warning \(#645\)

* fix: all yaml lint error

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: dockerfile lint error

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: lint whitespace,end-of-file etc

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: markdown lint error and disable check on MD033

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update:

- only whitelist certain keywords than skip MD033 entirely
- fix long length and missing newline in markdown

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: fix rebase and more markdown fix

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")  | Jan 29, 2026  |  
|   |   |  [docs(README): fix stale version badge and theme count mismatch (](https://github.com/llm-d/llm-d/commit/e2b3068eecff3f47fe9731beaaeffb8c90fe0c74 "docs\(README\): fix stale version badge and theme count mismatch \(#2367\)

Signed-off-by: Lily Sam <lilyoforiwaas@gmail.com>")[#2367](https://github.com/llm-d/llm-d/pull/2367)[)](https://github.com/llm-d/llm-d/commit/e2b3068eecff3f47fe9731beaaeffb8c90fe0c74 "docs\(README\): fix stale version badge and theme count mismatch \(#2367\)

Signed-off-by: Lily Sam <lilyoforiwaas@gmail.com>")  | Sep 11, 2026  |  
|   |   |  [Mark myself as inactive project maintainer while on leave (](https://github.com/llm-d/llm-d/commit/0495325175052fba8e41b9147ef65abe5237d4f6 "Mark myself as inactive project maintainer while on leave \(#985\)

Abdullah will be my replacement for the duration.

Signed-off-by: Clayton Coleman <smarterclayton@gmail.com>")[#985](https://github.com/llm-d/llm-d/pull/985)[)](https://github.com/llm-d/llm-d/commit/0495325175052fba8e41b9147ef65abe5237d4f6 "Mark myself as inactive project maintainer while on leave \(#985\)

Abdullah will be my replacement for the duration.

Signed-off-by: Clayton Coleman <smarterclayton@gmail.com>")  | Mar 19, 2026  |  
|   |   |  [fix[lint]: various kinds of lint error+warning (](https://github.com/llm-d/llm-d/commit/37771f348d3888a3a093741699a74e09e50b955a "fix\[lint\]: various kinds of lint error+warning \(#645\)

* fix: all yaml lint error

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: dockerfile lint error

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: lint whitespace,end-of-file etc

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: markdown lint error and disable check on MD033

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update:

- only whitelist certain keywords than skip MD033 entirely
- fix long length and missing newline in markdown

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: fix rebase and more markdown fix

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")[#645](https://github.com/llm-d/llm-d/pull/645)[)](https://github.com/llm-d/llm-d/commit/37771f348d3888a3a093741699a74e09e50b955a "fix\[lint\]: various kinds of lint error+warning \(#645\)

* fix: all yaml lint error

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: dockerfile lint error

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: lint whitespace,end-of-file etc

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: markdown lint error and disable check on MD033

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update:

- only whitelist certain keywords than skip MD033 entirely
- fix long length and missing newline in markdown

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: fix rebase and more markdown fix

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")  | Jan 29, 2026  |  
|   |   |  [Mark myself as inactive project maintainer while on leave (](https://github.com/llm-d/llm-d/commit/0495325175052fba8e41b9147ef65abe5237d4f6 "Mark myself as inactive project maintainer while on leave \(#985\)

Abdullah will be my replacement for the duration.

Signed-off-by: Clayton Coleman <smarterclayton@gmail.com>")[#985](https://github.com/llm-d/llm-d/pull/985)[)](https://github.com/llm-d/llm-d/commit/0495325175052fba8e41b9147ef65abe5237d4f6 "Mark myself as inactive project maintainer while on leave \(#985\)

Abdullah will be my replacement for the duration.

Signed-off-by: Clayton Coleman <smarterclayton@gmail.com>")  | Mar 19, 2026  |  
|   |   |   | Aug 14, 2026  |  
|   |   |  [chore: pre-commint/lint and github workflow for build (](https://github.com/llm-d/llm-d/commit/3c0650b10ee4e13f29716714868710d0bf058aeb "chore: pre-commint/lint and github workflow for build \(#568\)

* chore: pre-commint/lint and github workflow for build

- Update pre-commit tool version and docs
- fix lint found by yaml,markdown,hadolint
- threat model link is broken, it has been removed recently
- use lychee.toml for github and markdownlint-cli for local, add config
  files
- remove .markdown-lint-check and its config file
- set all github script to use pipefail
- remove strategy which is not used for xpu and cpu
- add more path filters to trigger build
- use github action build-push-action than make target for xpu and cpu
  build
- create new stage 'set-tag' to capture tags on docker image
- remove sccache.config.toml, which is not in use

- fix for the markdown link
  creds to @petecheslock https://github.com/llm-d/llm-d/commit/f0c603b72312dfd6a1d90d34a194da9606963971

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* lint: fix some markdown by pre-commit

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* chore: update link checker configuration and documentation

- Added exclusions for Google Docs fragments and GitHub blob line anchors in lychee.toml to improve link checking reliability.
- Updated CI workflow to include fragment checking for markdown links.
- Revised README documentation for accelerator vendors and gateway options to fix broken anchors in other docs

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>

* fix: update link in monitoring README for consistency

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>

* fix: update links in README for accuracy and consistency

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>

* update: code review

- fix typo which should onlly use @ for SHA! and : for tag/branch

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>
Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>
Co-authored-by: Pete Cheslock <pete.cheslock@redhat.com>")[#568](https://github.com/llm-d/llm-d/pull/568)[)](https://github.com/llm-d/llm-d/commit/3c0650b10ee4e13f29716714868710d0bf058aeb "chore: pre-commint/lint and github workflow for build \(#568\)

* chore: pre-commint/lint and github workflow for build

- Update pre-commit tool version and docs
- fix lint found by yaml,markdown,hadolint
- threat model link is broken, it has been removed recently
- use lychee.toml for github and markdownlint-cli for local, add config
  files
- remove .markdown-lint-check and its config file
- set all github script to use pipefail
- remove strategy which is not used for xpu and cpu
- add more path filters to trigger build
- use github action build-push-action than make target for xpu and cpu
  build
- create new stage 'set-tag' to capture tags on docker image
- remove sccache.config.toml, which is not in use

- fix for the markdown link
  creds to @petecheslock https://github.com/llm-d/llm-d/commit/f0c603b72312dfd6a1d90d34a194da9606963971

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* lint: fix some markdown by pre-commit

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* chore: update link checker configuration and documentation

- Added exclusions for Google Docs fragments and GitHub blob line anchors in lychee.toml to improve link checking reliability.
- Updated CI workflow to include fragment checking for markdown links.
- Revised README documentation for accelerator vendors and gateway options to fix broken anchors in other docs

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>

* fix: update link in monitoring README for consistency

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>

* fix: update links in README for accuracy and consistency

Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>

* update: code review

- fix typo which should onlly use @ for SHA! and : for tag/branch

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>
Signed-off-by: Pete Cheslock <pete.cheslock@redhat.com>
Co-authored-by: Pete Cheslock <pete.cheslock@redhat.com>")  | Jan 15, 2026  |  
|   |   |  [fix: allowlist retuned in typos config (](https://github.com/llm-d/llm-d/commit/3abf22f00b9a875e5e16fc1e4eb94ebc42fc0ab1 "fix: allowlist retuned in typos config \(#2303\) \(#2304\)

Signed-off-by: MeGaurav4 <gaurav3.141592@gmail.com>")[#2303](https://github.com/llm-d/llm-d/issues/2303)[) (](https://github.com/llm-d/llm-d/commit/3abf22f00b9a875e5e16fc1e4eb94ebc42fc0ab1 "fix: allowlist retuned in typos config \(#2303\) \(#2304\)

Signed-off-by: MeGaurav4 <gaurav3.141592@gmail.com>")[#2304](https://github.com/llm-d/llm-d/pull/2304)[)](https://github.com/llm-d/llm-d/commit/3abf22f00b9a875e5e16fc1e4eb94ebc42fc0ab1 "fix: allowlist retuned in typos config \(#2303\) \(#2304\)

Signed-off-by: MeGaurav4 <gaurav3.141592@gmail.com>")  | Aug 31, 2026  |  
| View all files |  
## Repository files navigation
##  Achieve SOTA Inference Performance On Any Accelerator 
llm-d is a high-performance distributed inference serving stack optimized for production deployments on Kubernetes. We help you achieve the fastest "time to state-of-the-art (SOTA) performance" for key OSS large language models across most hardware accelerators and infrastructure providers with well-tested guides and real-world benchmarks.
llm-d is a [Cloud Native Computing Foundation (CNCF)](https://www.cncf.io/) sandbox project, founded by Red Hat, Google Cloud, IBM Research, CoreWeave, and NVIDIA.
## What does llm-d offer to production inference?
Model servers like [vLLM](https://docs.vllm.ai) and [SGLang](https://github.com/sgl-project/sglang) handle efficiently running large language models on accelerators. llm-d provides state-of-the-art orchestration and optimizations above model servers to serve high-scale real-world traffic efficiently and reliably. Our offerings are organized into the following themes:
  * **[Intelligent Routing:](https://llm-d.ai/docs/well-lit-paths/foundations/optimized-baseline)** Maximize performance with prefix-cache and load-aware balancing, including experimental predicted latency-based scheduling to decrease latency and increase throughput.
  * **[Advanced KV-Cache Management:](https://llm-d.ai/docs/well-lit-paths/foundations/tiered-prefix-cache)** Increase the effective "working set size" for multi-turn requests with tiered offloading to CPU or disk and precise global indexing of the KV cache state.
  * **[Serving Large Models:](https://llm-d.ai/docs/well-lit-paths/foundations/wide-expert-parallelism)** Optimize massive models (e.g., DeepSeek-R1, GPT-OSS) using prefill/decode disaggregation and wide expert-parallelism over fast accelerator interconnects.
  * **[Operational Excellence:](https://llm-d.ai/docs/operations)** Ensure production stability with intelligent flow control for multi-tenant serving and proactive, SLO-aware autoscaling based on real-time inference signals.
  * Efficiently manage large-scale offline inference with OpenAI-compatible Batch APIs and asynchronous processing to maximize hardware utilization.


For a complete list of tested recipes and architectural patterns, see our [well-lit path guides](https://llm-d.ai/docs/well-lit-paths). These guides provide benchmarked recipes and Helm charts to start serving quickly with best practices common to production deployments. Our intent is to eliminate the heavy lifting common in tuning and deploying generative AI inference on modern accelerators.
## Performance Highlights
Validated performance gains from production deployments and partner benchmarks:
  * **3x higher output throughput** and **2x faster TTFT** with prefix-cache-aware routing vs round-robin — Llama 3.1 70B on 4× AMD MI300X, Tesla / Red Hat ([blog](https://llm-d.ai/blog/production-grade-llm-inference-at-scale-kserve-llm-d-vllm))
  * **40% reduction in TTFT and ITL** with predicted-latency scheduling vs heuristics on NVIDIA GPUs, Google ([blog](https://llm-d.ai/blog/predicted-latency-based-scheduling-for-llms))
  * **Up to 70% higher tokens/sec** with prefill/decode disaggregation vs standard vLLM — GPT-OSS on NVIDIA B200 (p6-b200), AWS ([blog](https://aws.amazon.com/blogs/machine-learning/introducing-disaggregated-inference-on-aws-powered-by-llm-d/))
  * **10–30% throughput improvement** with disaggregated serving on identical infrastructure — GPT-OSS-120B and Llama 3.3 70B on AMD MI300X, Oracle ([blog](https://blogs.oracle.com/ai-and-datascience/llm-inference-at-scale-with-llm-d-on-oci))
  * **50k tokens/sec** cluster throughput with Wide Expert-Parallelism — 16×16 NVIDIA B200, ~3.1k tok/s per GPU ([blog](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale))
  * **13.9x throughput improvement** with hierarchical KV offloading at 250 concurrent users vs GPU-only — 4× NVIDIA H100 ([blog](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale))


Explore detailed, reproducible benchmarks on [Prism](https://prism.llm-d.ai).
## Get Started Now
Ready to achieve SOTA performance? Follow our [Quickstart Guide](https://llm-d.ai/docs/getting-started/quickstart) to deploy your first optimized inference service on Kubernetes. You'll learn how to set up the llm-d stack, configure the intelligent router, and validate performance with production-ready benchmarks.
Tip
Most users begin with our [Optimized Baseline](https://llm-d.ai/docs/well-lit-paths/foundations/optimized-baseline), which provides a high-performance foundation for a wide range of LLM serving use cases.
## Latest News 🔥
  * [2026-05] The v0.7 release introduces an optimized baseline renamed and stabilized, kustomize-first migrated guides, expanded nightly CI (OpenShift, GKE, CoreWeave), predicted-latency scheduling GA, batch gateway (experimental), and revamped project-wide documentation.
  * [2026-03] llm-d [joins the CNCF as a Sandbox project](https://www.cncf.io/blog/2026/03/24/welcome-llm-d-to-the-cncf-evolving-kubernetes-into-sota-ai-infrastructure/)! Founded by Red Hat, Google Cloud, IBM Research, CoreWeave, and NVIDIA, with support from AMD, Cisco, Hugging Face, Intel, Lambda, Mistral AI, UC Berkeley, and University of Chicago. We're excited to collaborate openly on building flexible, future-proof AI infrastructure.
  * [2026-02] The [v0.5](https://llm-d.ai/blog/llm-d-v0.5-sustaining-performance-at-scale) introduces reproducible benchmark workflows, hierarchical KV offloading, cache-aware LoRA routing, active-active HA, UCCL-based transport resilience, and scale-to-zero autoscaling; validated ~3.1k tok/s per B200 decode GPU (wide-EP) and up to 50k output tok/s on a 16×16 B200 prefill/decode topology with order-of-magnitude TTFT reduction vs round-robin baseline.
  * [2025-12] The [v0.4](https://llm-d.ai/blog/llm-d-v0.4-achieve-sota-inference-across-accelerators) release demonstrates 40% reduction in per output token latency for DeepSeek V3.1 on H200 GPUs, Intel XPU and Google TPU disaggregation support for lower time to first token, a new well-lit path for prefix cache offload to vLLM-native CPU memory tiering, and a preview of the workload variant autoscaler improving model-as-a-service efficiency.


## 🧱 Architecture
llm-d accelerates distributed inference by integrating industry-standard open technologies like vLLM and Kubernetes. For more details, see our full [Architecture Documentation](https://llm-d.ai/docs/architecture).
## 📦 Releases
Our [guides](https://github.com/llm-d/llm-d/blob/main/guides/README.md) are living docs and kept current. For details about the Helm charts and component releases, visit our [GitHub Releases page](https://github.com/llm-d/llm-d/releases) to review release notes.
See the [accelerator docs](https://github.com/llm-d/llm-d/blob/main/docs/getting-started/accelerators.md) for points of contact and more details about the accelerators, networks, and configurations tested.
## Contribute
We adhere to the [CNCF Code of Conduct](https://github.com/cncf/foundation/blob/main/code-of-conduct.md).
  * See [our project overview](https://github.com/llm-d/llm-d/blob/main/PROJECT.md) for more details on our development process and governance.
  * Review [our contributing guidelines](https://github.com/llm-d/llm-d/blob/main/CONTRIBUTING.md) for detailed information on how to contribute to the project.
  * Join one of our [Special Interest Groups (SIGs)](https://github.com/llm-d/llm-d/blob/main/SIGS.md) to contribute to specific areas of the project and collaborate with domain experts.
  * We use Slack to discuss development across organizations. Please join: [Slack](https://llm-d.ai/slack)
  * We host a bi-weekly standup for contributors every other Wednesday at 12:30 PM ET, as well as meetings for various SIGs. You can find them in the [shared llm-d calendar](https://red.ht/llm-d-public-calendar)
  * We use Google Groups to share architecture diagrams and other content. Please join: [Google Group](https://groups.google.com/g/llm-d-contributors)


## License
This project is licensed under Apache License 2.0. See the [LICENSE file](https://github.com/llm-d/llm-d/blob/main/LICENSE) for details.
## About
Achieve state of the art inference performance with modern accelerators on Kubernetes
### Topics
[ai](https://github.com/topics/ai)[cncf](https://github.com/topics/cncf)[distributed-inference](https://github.com/topics/distributed-inference)[gpu](https://github.com/topics/gpu)[inference](https://github.com/topics/inference)[intelligent-routing](https://github.com/topics/intelligent-routing)[kubernetes](https://github.com/topics/kubernetes)[llm](https://github.com/topics/llm)[model-server](https://github.com/topics/model-server)
### Resources
### Code of conduct
### Contributing
### Security policy
### Stars
**4.5k** stars
### Watchers
**64** watching
### Forks
[ forks](https://github.com/llm-d/llm-d/forks)
## 
[ v0.9.0 ReleaseLatest Aug 17, 2026 ](https://github.com/llm-d/llm-d/releases/tag/v0.9.0)
## 
## Languages
  * [Shell51.5%](https://github.com/llm-d/llm-d/search?l=shell)
  * [Python40.7%](https://github.com/llm-d/llm-d/search?l=python)
  * [Makefile3.7%](https://github.com/llm-d/llm-d/search?l=makefile)
  * [HTML3.5%](https://github.com/llm-d/llm-d/search?l=html)
  * [Dockerfile0.6%](https://github.com/llm-d/llm-d/search?l=dockerfile)


You can’t perform that action at this time. 
