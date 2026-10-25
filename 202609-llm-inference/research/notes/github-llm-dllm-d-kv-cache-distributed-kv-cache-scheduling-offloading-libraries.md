---
title: 'GitHub - llm-d/llm-d-kv-cache: Distributed KV cache scheduling & offloading
  libraries · GitHub'
id: github-llm-dllm-d-kv-cache-distributed-kv-cache-scheduling-offloading-libraries
tags:
- llmd-kv-cache-routing-c665d5
- kv-cache-indexer
- kv-events
- architecture
- deprecated-repo
created: '2026-09-14T11:53:01.917626Z'
updated: '2026-10-10T14:52:17.291758Z'
source: https://github.com/llm-d/llm-d-kv-cache
source_domain: github.com
fetched_at: '2026-09-14T11:53:01.896714Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Repo README for llm-d-kv-cache, the original home of KV-Cache Aware Routing
  libraries. As of PR #1886 (2026), the kvcache, kvblock, and kvevents libraries that
  power KV-cache-aware routing were internalized/moved into llm-d-router; this repo
  is deprecated for scheduling logic and only retains PVC Evictor (disk space manager
  for offloaded KV-cache on PVCs) and llmd-fs-backend (storage backend moving KV-cache
  blocks between GPU and shared storage). llmd-fs-backend v0.23 (llm-d v0.8 / vLLM
  v0.23) was its final release since the FS tier was upstreamed into vLLM''s native
  multi-tier offloading connector (TieringOffloadingSpec). Describes the Read Path
  (scheduler calls kvcache.Indexer.Score(prompt, pods) -> queries kvblock.Index ->
  returns per-pod KV-cache-hit scores) and Write Path (vLLM pods emit KVEvents on
  KV-block create/evict -> kvevents.Pool subscriber updates the kvblock.Index in near-real-time),
  which together let the indexer keep a global, near-real-time view of KV-cache block
  locality (GPU vs CPU tier) across a fleet of vLLM pods for prefix-cache-aware scheduling.
  Latest tagged release v0.9.0 (Jun 14, 2026).'
utility_score: 15.0
---

[Skip to content](https://github.com/llm-d/llm-d-kv-cache#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-kv-cache).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache) You must be signed in to change notification settings
  * [ Fork 156 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache)
  * [ Star  177 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache)


[**21** Branches](https://github.com/llm-d/llm-d-kv-cache/branches)[](https://github.com/llm-d/llm-d-kv-cache/tags)
Go to file
Open more actions menu
## Latest commit
[chore(pvc-evictor): standardize dev environment via pyproject.toml (](https://github.com/llm-d/llm-d-kv-cache/commit/8cf43067afb7fc9fefafc1b64de063c769f2c90f)[#698](https://github.com/llm-d/llm-d-kv-cache/pull/698)
Open commit detailssuccess
Jul 23, 2026
[8cf4306](https://github.com/llm-d/llm-d-kv-cache/commit/8cf43067afb7fc9fefafc1b64de063c769f2c90f) · Jul 23, 2026
## History
[393 Commits](https://github.com/llm-d/llm-d-kv-cache/commits/main/)
Open commit details
393 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |  [chore(pvc-evictor): standardize dev environment via pyproject.toml (](https://github.com/llm-d/llm-d-kv-cache/commit/8cf43067afb7fc9fefafc1b64de063c769f2c90f "chore\(pvc-evictor\): standardize dev environment via pyproject.toml \(#698\)

* chore\(pvc-evictor\): standardize dev environment via pyproject.toml

Adds pyproject.toml with project metadata, a python>=3.12 pin matching
CI and the runtime image, and a dev extra replacing requirements-dev.txt.
The Makefile test target now runs inside a project venv \(same pattern as
the root Makefile venvs\) and CI installs the project editable with its
dev extra.

The evictor code is stdlib-only, so dependencies is deliberately empty;
the msgpack/pyzmq install in the Dockerfile is not imported anywhere and
is flagged in the PR rather than carried into the metadata.

Signed-off-by: Greg Neighbors <26003+gkneighb@users.noreply.github.com>

* pvc-evictor: correct dep comment and drop stale README install step

Address review on #698: the msgpack/pyzmq install in the Dockerfile is
not dead weight -- it serves the optional STORAGE_EVENTS_ENDPOINT
publishing path \(llmd_fs_backend.event_publisher, copied into the image\).
dependencies stays empty since the package imports none directly, but the
comment now says why. README no longer references the removed
requirements-dev.txt \(make test installs the dev extra\).

Signed-off-by: Greg Neighbors <26003+gkneighb@users.noreply.github.com>

---------

Signed-off-by: Greg Neighbors <26003+gkneighb@users.noreply.github.com>
Co-authored-by: Greg Neighbors <26003+gkneighb@users.noreply.github.com>")[#698](https://github.com/llm-d/llm-d-kv-cache/pull/698)  | Jul 23, 2026  |  
|  [fix(tokenization): preserve assistant tool calls in UDS rendering (](https://github.com/llm-d/llm-d-kv-cache/commit/5f55c2f9e082edc9547f656e28f2c15b33d52c30 "fix\(tokenization\): preserve assistant tool calls in UDS rendering \(#577\)

* fix\(tokenization\): preserve assistant tool calls in UDS rendering

Keep assistant tool calls on the Go-to-Python UDS rendering path so chat templates can render tool-call turns correctly while keeping regression coverage focused on that boundary.

Signed-off-by: Kay Yan <kay.yan@daocloud.io>

* fix\(tokenization\): address tool calls review feedback

Signed-off-by: Kay Yan <kay.yan@daocloud.io>

---------

Signed-off-by: Kay Yan <kay.yan@daocloud.io>")[#577](https://github.com/llm-d/llm-d-kv-cache/pull/577)[)](https://github.com/llm-d/llm-d-kv-cache/commit/5f55c2f9e082edc9547f656e28f2c15b33d52c30 "fix\(tokenization\): preserve assistant tool calls in UDS rendering \(#577\)

* fix\(tokenization\): preserve assistant tool calls in UDS rendering

Keep assistant tool calls on the Go-to-Python UDS rendering path so chat templates can render tool-call turns correctly while keeping regression coverage focused on that boundary.

Signed-off-by: Kay Yan <kay.yan@daocloud.io>

* fix\(tokenization\): address tool calls review feedback

Signed-off-by: Kay Yan <kay.yan@daocloud.io>

---------

Signed-off-by: Kay Yan <kay.yan@daocloud.io>")  | May 16, 2026  |  
|   |   |  [Replace BlockSize with BlockSizeTokens (](https://github.com/llm-d/llm-d-kv-cache/commit/a746476b8266099aea0e77fc5e8edfbc106111d1 "Replace BlockSize with BlockSizeTokens \(#564\)

* Replace BlockSize with BlockSizeTokens

Signed-off-by: roytman <roytman@il.ibm.com>

* fix lint and e2e test errors

Signed-off-by: roytman <roytman@il.ibm.com>

* fix comments

Signed-off-by: roytman <roytman@il.ibm.com>

---------

Signed-off-by: roytman <roytman@il.ibm.com>")[#564](https://github.com/llm-d/llm-d-kv-cache/pull/564)[)](https://github.com/llm-d/llm-d-kv-cache/commit/a746476b8266099aea0e77fc5e8edfbc106111d1 "Replace BlockSize with BlockSizeTokens \(#564\)

* Replace BlockSize with BlockSizeTokens

Signed-off-by: roytman <roytman@il.ibm.com>

* fix lint and e2e test errors

Signed-off-by: roytman <roytman@il.ibm.com>

* fix comments

Signed-off-by: roytman <roytman@il.ibm.com>

---------

Signed-off-by: roytman <roytman@il.ibm.com>")  | May 7, 2026  |  
|   |   | [switching dev to ghcr.io](https://github.com/llm-d/llm-d-kv-cache/commit/6ee69a65b1a218c4c9b57dc24ee592759481f7ec "switching dev to ghcr.io")  | May 15, 2025  |  
|  [Fix CPU offloading for precise-prefix-cache-aware (](https://github.com/llm-d/llm-d-kv-cache/commit/1d8e62e77fc557138794284e16546186a14f2dfb "Fix CPU offloading for precise-prefix-cache-aware \(#561\)

* fix issues in cpu offloading

* fix for a Redis-specific edge case

* addressed racing issues and made DeviceTier consistent

* update document on CPU Offloading for precise-prefix-cache-aware

* update document on CPU Offloading

* fix lint issue

* add more details on doc

* add note on event ordering guarantee

* add note for storage to CPU path")[#561](https://github.com/llm-d/llm-d-kv-cache/pull/561)[)](https://github.com/llm-d/llm-d-kv-cache/commit/1d8e62e77fc557138794284e16546186a14f2dfb "Fix CPU offloading for precise-prefix-cache-aware \(#561\)

* fix issues in cpu offloading

* fix for a Redis-specific edge case

* addressed racing issues and made DeviceTier consistent

* update document on CPU Offloading for precise-prefix-cache-aware

* update document on CPU Offloading

* fix lint issue

* add more details on doc

* add note on event ordering guarantee

* add note for storage to CPU path")  | May 10, 2026  |  
|   |   |  [fix(tokenization): preserve assistant tool calls in UDS rendering (](https://github.com/llm-d/llm-d-kv-cache/commit/5f55c2f9e082edc9547f656e28f2c15b33d52c30 "fix\(tokenization\): preserve assistant tool calls in UDS rendering \(#577\)

* fix\(tokenization\): preserve assistant tool calls in UDS rendering

Keep assistant tool calls on the Go-to-Python UDS rendering path so chat templates can render tool-call turns correctly while keeping regression coverage focused on that boundary.

Signed-off-by: Kay Yan <kay.yan@daocloud.io>

* fix\(tokenization\): address tool calls review feedback

Signed-off-by: Kay Yan <kay.yan@daocloud.io>

---------

Signed-off-by: Kay Yan <kay.yan@daocloud.io>")[#577](https://github.com/llm-d/llm-d-kv-cache/pull/577)[)](https://github.com/llm-d/llm-d-kv-cache/commit/5f55c2f9e082edc9547f656e28f2c15b33d52c30 "fix\(tokenization\): preserve assistant tool calls in UDS rendering \(#577\)

* fix\(tokenization\): preserve assistant tool calls in UDS rendering

Keep assistant tool calls on the Go-to-Python UDS rendering path so chat templates can render tool-call turns correctly while keeping regression coverage focused on that boundary.

Signed-off-by: Kay Yan <kay.yan@daocloud.io>

* fix\(tokenization\): address tool calls review feedback

Signed-off-by: Kay Yan <kay.yan@daocloud.io>

---------

Signed-off-by: Kay Yan <kay.yan@daocloud.io>")  | May 16, 2026  |  
|  [docs: update examples to use UDS tokenizer Docker image (](https://github.com/llm-d/llm-d-kv-cache/commit/29ac9f5282ff686f2749f13e9235fa5b9cfe6004 "docs: update examples to use UDS tokenizer Docker image \(#471\)

* update examples

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* cache image

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* run examples

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* remove build-embedded CI job from examples workflow

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* fix examples path exclusion in golangci-lint config

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* lint

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* address review: fix lint, container cleanup, exit codes, and port config

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* build fix

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* decoder only model name for examples

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* Examples docs update

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* compute blocks fix

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* simplify verify examples and rm -f container

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* makefile dep

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* rm flag

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* update examples helper with extra keys

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* reduce git diff

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* Revert "reduce git diff"

This reverts commit f2eb22f6369143cfa6afd3a916041b5f34834576.

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* Fix TCP detection + export ApplyTokenizerEndpoint in helper

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* Split run-example Makefile target + fix verify-examples.sh

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* fix trap

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* reuse run-example-only in run-example

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

---------

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>")[#471](https://github.com/llm-d/llm-d-kv-cache/pull/471)[)](https://github.com/llm-d/llm-d-kv-cache/commit/29ac9f5282ff686f2749f13e9235fa5b9cfe6004 "docs: update examples to use UDS tokenizer Docker image \(#471\)

* update examples

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* cache image

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* run examples

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* remove build-embedded CI job from examples workflow

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* fix examples path exclusion in golangci-lint config

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* lint

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* address review: fix lint, container cleanup, exit codes, and port config

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* build fix

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* decoder only model name for examples

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* Examples docs update

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* compute blocks fix

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* simplify verify examples and rm -f container

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* makefile dep

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* rm flag

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* update examples helper with extra keys

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* reduce git diff

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* Revert "reduce git diff"

This reverts commit f2eb22f6369143cfa6afd3a916041b5f34834576.

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* Fix TCP detection + export ApplyTokenizerEndpoint in helper

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* Split run-example Makefile target + fix verify-examples.sh

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* fix trap

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* reuse run-example-only in run-example

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

---------

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>")  | Mar 31, 2026  |  
|   |   | [[build]: Add pre-commit target](https://github.com/llm-d/llm-d-kv-cache/commit/ccf360c1be3d48f387b07d988aabb50ef9e1d703 "\[build\]: Add pre-commit target

Signed-off-by: Kfir Toledo <kfir.toledo@ibm.com>")  | May 19, 2025  |  
|   |   |  [chore(pvc-evictor): standardize dev environment via pyproject.toml (](https://github.com/llm-d/llm-d-kv-cache/commit/8cf43067afb7fc9fefafc1b64de063c769f2c90f "chore\(pvc-evictor\): standardize dev environment via pyproject.toml \(#698\)

* chore\(pvc-evictor\): standardize dev environment via pyproject.toml

Adds pyproject.toml with project metadata, a python>=3.12 pin matching
CI and the runtime image, and a dev extra replacing requirements-dev.txt.
The Makefile test target now runs inside a project venv \(same pattern as
the root Makefile venvs\) and CI installs the project editable with its
dev extra.

The evictor code is stdlib-only, so dependencies is deliberately empty;
the msgpack/pyzmq install in the Dockerfile is not imported anywhere and
is flagged in the PR rather than carried into the metadata.

Signed-off-by: Greg Neighbors <26003+gkneighb@users.noreply.github.com>

* pvc-evictor: correct dep comment and drop stale README install step

Address review on #698: the msgpack/pyzmq install in the Dockerfile is
not dead weight -- it serves the optional STORAGE_EVENTS_ENDPOINT
publishing path \(llmd_fs_backend.event_publisher, copied into the image\).
dependencies stays empty since the package imports none directly, but the
comment now says why. README no longer references the removed
requirements-dev.txt \(make test installs the dev extra\).

Signed-off-by: Greg Neighbors <26003+gkneighb@users.noreply.github.com>

---------

Signed-off-by: Greg Neighbors <26003+gkneighb@users.noreply.github.com>
Co-authored-by: Greg Neighbors <26003+gkneighb@users.noreply.github.com>")[#698](https://github.com/llm-d/llm-d-kv-cache/pull/698)  | Jul 23, 2026  |  
|  [fix(kvcache): unify engine-to-request mapping and fix Redis scoring (](https://github.com/llm-d/llm-d-kv-cache/commit/c4ebd56486eb66b58b9d20915eec90a977f1843a "fix\(kvcache\): unify engine-to-request mapping and fix Redis scoring \(#670\)

* fix\(kvcache\): unify engine-to-request mapping and fix Redis scoring

Extract duplicated proportional distribution logic into a shared engineToRequestMapping helper in index.go to reduce code duplication and improve maintainability. Update CostAwareMemoryIndex, InMemoryIndex, and RedisIndex to use the centralized function. Additionally, correct the Redis ZAdd score calculation to use the relative index within each mapping slice instead of the global loop counter, ensuring accurate positional scoring for request keys.

Signed-off-by: Alex <alex.tech.lab@outlook.com>

* fix\(kvcache\): guard engineToRequestMapping against empty slices and add tests

Add defensive check for empty engineKeys/requestKeys to prevent index
out of range panic. Add comprehensive unit tests covering 1:1, many:1,
1:many, empty slices, and score ordering scenarios.

Signed-off-by: Alex <alex.tech.lab@outlook.com>

* fix\(kvcache\): preallocate map capacity and document empty-input behavior

Address Copilot review feedback on engineToRequestMapping:
- Preallocate map with len\(engineKeys\) to avoid unnecessary rehashing
- Add explicit doc comment stating empty-input returns empty map

Signed-off-by: Alex <alex.tech.lab@outlook.com>

---------

Signed-off-by: Alex <alex.tech.lab@outlook.com>")[#…](https://github.com/llm-d/llm-d-kv-cache/pull/670)  | Jun 29, 2026  |  
| [services/uds_tokenizer](https://github.com/llm-d/llm-d-kv-cache/tree/main/services/uds_tokenizer "This path skips through empty directories")  | [services/uds_tokenizer](https://github.com/llm-d/llm-d-kv-cache/tree/main/services/uds_tokenizer "This path skips through empty directories")  |  [fix(tokenization): preserve assistant tool calls in UDS rendering (](https://github.com/llm-d/llm-d-kv-cache/commit/5f55c2f9e082edc9547f656e28f2c15b33d52c30 "fix\(tokenization\): preserve assistant tool calls in UDS rendering \(#577\)

* fix\(tokenization\): preserve assistant tool calls in UDS rendering

Keep assistant tool calls on the Go-to-Python UDS rendering path so chat templates can render tool-call turns correctly while keeping regression coverage focused on that boundary.

Signed-off-by: Kay Yan <kay.yan@daocloud.io>

* fix\(tokenization\): address tool calls review feedback

Signed-off-by: Kay Yan <kay.yan@daocloud.io>

---------

Signed-off-by: Kay Yan <kay.yan@daocloud.io>")[#577](https://github.com/llm-d/llm-d-kv-cache/pull/577)[)](https://github.com/llm-d/llm-d-kv-cache/commit/5f55c2f9e082edc9547f656e28f2c15b33d52c30 "fix\(tokenization\): preserve assistant tool calls in UDS rendering \(#577\)

* fix\(tokenization\): preserve assistant tool calls in UDS rendering

Keep assistant tool calls on the Go-to-Python UDS rendering path so chat templates can render tool-call turns correctly while keeping regression coverage focused on that boundary.

Signed-off-by: Kay Yan <kay.yan@daocloud.io>

* fix\(tokenization\): address tool calls review feedback

Signed-off-by: Kay Yan <kay.yan@daocloud.io>

---------

Signed-off-by: Kay Yan <kay.yan@daocloud.io>")  | May 16, 2026  |  
|   |   |  [feat(evictor): implement background empty directory cleanup process (](https://github.com/llm-d/llm-d-kv-cache/commit/756627813c4f58515a7d918b6b3fe594ab24eb6b "feat\(evictor\): implement background empty directory cleanup process \(#619\)

* feat\(evictor\): background empty directory cleanup

Empty cache directories accumulate as files are evicted. This adds a
background folder-cleaner process \(P\(N+3\), gated by ENABLE_DIR_CLEANUP\)
that removes them.

How it works:
- The crawler detects empty rank/{hhh}/{hh} directories during its sweep
  and the deleter offers each freshly-emptied parent directory after a
  batch delete. Both feed a shared folder_queue.
- The folder cleaner pulls paths off the queue and removes them with
  os.rmdir, which is inherently safe: it is a no-op if a file has landed
  in the directory in the meantime.

Safety:
- queue_folder skips directories modified within DIR_CLEANUP_TTL_SECONDS
  \(default 120s\) so we don't race a writer that just created a bucket and
  is about to populate it. This is defense-in-depth on top of rmdir's
  empty-only semantics.

Config / Helm:
- New ENABLE_DIR_CLEANUP \(default true\) and DIR_CLEANUP_TTL_SECONDS
  \(default 120\) env vars, wired through config.py, the Helm values and
  Deployment template, and documented in CONFIGURATION.md.

Reporting:
- The folder cleaner reports folders_purged via a new folder_cleaner_stats
  channel surfaced in the aggregated log. The crawler's per-sweep counter
  is named empty_folders_queued to reflect that it counts directories
  handed to the cleaner, not directories it deleted itself. The deleter's
  progress/done result-queue protocol is left unchanged.

Signed-off-by: Miro <mironikolov@google.com>

* Fix pvc_evictor unit tests due to delete_file_batch signature update

Signed-off-by: Miro <mironikolov@google.com>

* test: merge empty directory cleanup unit tests from PR #625

Signed-off-by: Miro <mironikolov@google.com>

* fix\(test\): use moby container package instead of docker docker package to fix testcontainers-go build error

Signed-off-by: Miro <mironikolov@google.com>

* fix\(ci\): update golangci-lint configuration format

Signed-off-by: Miro <mironikolov@google.com>

* fix\(kvblock\): resolve ZRevRange deprecation and lll warnings

Signed-off-by: Miro <mironikolov@google.com>

* fix\(test\): resolve lll and unused gocritic lint warnings in uds_e2e_suite_test.go

Signed-off-by: Miro <mironikolov@google.com>

* Fix the linting errors

Signed-off-by: Miro <mironikolov@google.com>

---------

Signed-off-by: Miro <mironikolov@google.com>")[#…](https://github.com/llm-d/llm-d-kv-cache/pull/619)  | Jun 8, 2026  |  
|   |   | [feat(telemetry): align kv-cache tracing with the standard llm-d obser…](https://github.com/llm-d/llm-d-kv-cache/commit/f453b6a49a31fe75ae14afa2dc9a0b86325d4b1b "feat\(telemetry\): align kv-cache tracing with the standard llm-d observability pattern \(#653\)

Signed-off-by: Guangya Liu <gyliu513@gmail.com>")  | Jun 11, 2026  |  
|   |   |   | Feb 22, 2026  |  
|   |   |  [[build]: Add pre-commit config (](https://github.com/llm-d/llm-d-kv-cache/commit/523ad37f030dd32bbab19fe4fc710ce33dc135aa "\[build\]: Add pre-commit config \(#239\)

Signed-off-by: Kfir Toledo <kfir.toledo@ibm.com>")[#239](https://github.com/llm-d/llm-d-kv-cache/pull/239)[)](https://github.com/llm-d/llm-d-kv-cache/commit/523ad37f030dd32bbab19fe4fc710ce33dc135aa "\[build\]: Add pre-commit config \(#239\)

Signed-off-by: Kfir Toledo <kfir.toledo@ibm.com>")  | Jan 1, 2026  |  
|   |   |  [Refactor: Replace transformers with vLLM (](https://github.com/llm-d/llm-d-kv-cache/commit/a8ca9ba665df0e14897e544adda699e8fdba4366 "Refactor: Replace transformers with vLLM \(#234\)

* apply chat template

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* lint

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* apply copilot review

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* add example_usage

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* Update pkg/preprocessing/chat_completions/requirements.txt

Co-authored-by: Edoardo Vacchi <evacchi@users.noreply.github.com>
Signed-off-by: Hyunkyun Moon <mhg5303@gmail.com>

* request with key

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* edit data

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* resolve CStrings memory leaks

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* apply review

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* edit

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* edit

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* add path

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

---------

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>
Signed-off-by: Hyunkyun Moon <mhg5303@gmail.com>
Co-authored-by: Edoardo Vacchi <evacchi@users.noreply.github.com>")[#234](https://github.com/llm-d/llm-d-kv-cache/pull/234)[)](https://github.com/llm-d/llm-d-kv-cache/commit/a8ca9ba665df0e14897e544adda699e8fdba4366 "Refactor: Replace transformers with vLLM \(#234\)

* apply chat template

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* lint

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* apply copilot review

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* add example_usage

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* Update pkg/preprocessing/chat_completions/requirements.txt

Co-authored-by: Edoardo Vacchi <evacchi@users.noreply.github.com>
Signed-off-by: Hyunkyun Moon <mhg5303@gmail.com>

* request with key

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* edit data

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* resolve CStrings memory leaks

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* apply review

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* edit

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* edit

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

* add path

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>

---------

Signed-off-by: HyunKyun Moon <mhg5303@gmail.com>
Signed-off-by: Hyunkyun Moon <mhg5303@gmail.com>
Co-authored-by: Edoardo Vacchi <evacchi@users.noreply.github.com>")  | Jan 12, 2026  |  
|   |   |  [Ignore large reformatting PRs in git blame (](https://github.com/llm-d/llm-d-kv-cache/commit/fe9af521205d1bbd8a093927042c3205160fb8c6 "Ignore large reformatting PRs in git blame \(#364\)

Signed-off-by: Kay Yan <kay.yan@daocloud.io>")[#364](https://github.com/llm-d/llm-d-kv-cache/pull/364)[)](https://github.com/llm-d/llm-d-kv-cache/commit/fe9af521205d1bbd8a093927042c3205160fb8c6 "Ignore large reformatting PRs in git blame \(#364\)

Signed-off-by: Kay Yan <kay.yan@daocloud.io>")  | Feb 27, 2026  |  
|   |   |  [🌱 Remove legacy typo and link checker workflows (](https://github.com/llm-d/llm-d-kv-cache/commit/6e667a7a6ba111d14fe4927e6a22bdf995033761 "🌱 Remove legacy typo and link checker workflows \(#324\)

* 🌱 Remove legacy caller workflow

Replaced by gh-aw agentic workflow in llm-d-infra.

Signed-off-by: Andy Anderson <andy@clubanderson.com>
Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>

* 🌱 Remove legacy caller workflow

Replaced by gh-aw agentic workflow in llm-d-infra.

Signed-off-by: Andy Anderson <andy@clubanderson.com>
Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>

* ✨ Add GitHub Agentic Workflows for typo, link, and upstream checks

Replace legacy CI callers with AI-powered gh-aw workflows:
- typo-checker.md: domain-aware typo checking on PR changed files
- link-checker.md: smart link checking with transient failure handling
- upstream-monitor.md: daily scan for upstream breaking changes
- copilot-setup-steps.yml: installs gh-aw CLI extension
- .gitattributes: marks lock files as generated
- docs/upstream-versions.md: template for tracking dependency pins

Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>
Signed-off-by: Andrew Anderson <andy@clubanderson.com>

* 🐛 Recompile gh-aw lock files with v0.45.0

- Upgrade from gh-aw v0.33.12 to v0.45.0
- Lock files 8x smaller \(50KB vs 400KB\)
- Fix trailing whitespace that broke pre-commit hooks
- Update network config to use ecosystem names

Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>
Signed-off-by: Andrew Anderson <andy@clubanderson.com>

* 🐛 Fix CI: trailing EOF, upstream-versions link, licenserc

- Fix lock files trailing blank line \(end-of-file-fixer\)
- Fix relative link in docs/upstream-versions.md
- Add .gitattributes to licenserc excludes where needed

Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>
Signed-off-by: Andrew Anderson <andy@clubanderson.com>

---------

Signed-off-by: Andy Anderson <andy@clubanderson.com>
Signed-off-by: Andrew Anderson <andy@clubanderson.com>
Co-authored-by: Claude Opus 4.5 <noreply@anthropic.com>")[#324](https://github.com/llm-d/llm-d-kv-cache/pull/324)[)](https://github.com/llm-d/llm-d-kv-cache/commit/6e667a7a6ba111d14fe4927e6a22bdf995033761 "🌱 Remove legacy typo and link checker workflows \(#324\)

* 🌱 Remove legacy caller workflow

Replaced by gh-aw agentic workflow in llm-d-infra.

Signed-off-by: Andy Anderson <andy@clubanderson.com>
Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>

* 🌱 Remove legacy caller workflow

Replaced by gh-aw agentic workflow in llm-d-infra.

Signed-off-by: Andy Anderson <andy@clubanderson.com>
Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>

* ✨ Add GitHub Agentic Workflows for typo, link, and upstream checks

Replace legacy CI callers with AI-powered gh-aw workflows:
- typo-checker.md: domain-aware typo checking on PR changed files
- link-checker.md: smart link checking with transient failure handling
- upstream-monitor.md: daily scan for upstream breaking changes
- copilot-setup-steps.yml: installs gh-aw CLI extension
- .gitattributes: marks lock files as generated
- docs/upstream-versions.md: template for tracking dependency pins

Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>
Signed-off-by: Andrew Anderson <andy@clubanderson.com>

* 🐛 Recompile gh-aw lock files with v0.45.0

- Upgrade from gh-aw v0.33.12 to v0.45.0
- Lock files 8x smaller \(50KB vs 400KB\)
- Fix trailing whitespace that broke pre-commit hooks
- Update network config to use ecosystem names

Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>
Signed-off-by: Andrew Anderson <andy@clubanderson.com>

* 🐛 Fix CI: trailing EOF, upstream-versions link, licenserc

- Fix lock files trailing blank line \(end-of-file-fixer\)
- Fix relative link in docs/upstream-versions.md
- Add .gitattributes to licenserc excludes where needed

Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>
Signed-off-by: Andrew Anderson <andy@clubanderson.com>

---------

Signed-off-by: Andy Anderson <andy@clubanderson.com>
Signed-off-by: Andrew Anderson <andy@clubanderson.com>
Co-authored-by: Claude Opus 4.5 <noreply@anthropic.com>")  | Feb 16, 2026  |  
|   |   |  [fix(pvc_evictor): configure activator log level via config_dict (](https://github.com/llm-d/llm-d-kv-cache/commit/500b6069cedb9f0d863ba6c8744bc15e7ca814c2 "fix\(pvc_evictor\): configure activator log level via config_dict \(#649\)

* fix\(pvc_evictor\): configure activator log level via config_dict

Pass config_dict to the activator process to dynamically set the logging level and log file path. This replaces the hardcoded INFO level and environment variable fallback, enabling consistent and configurable logging across processes. Updated evictor.py to forward the config during initialization and respawn, and added the local virtual environment to .gitignore.

Signed-off-by: Alex <alex.tech.lab@outlook.com>

* fix\(pvc_evictor\): use defensive .get\(\) for log_level across all processes

Replace direct dict access config_dict\["log_level"\] with
config_dict.get\("log_level", "INFO"\) in activator, crawler, deleter,
and folder_cleaner for consistent defensive coding.

Signed-off-by: Alex <alex.tech.lab@outlook.com>

---------

Signed-off-by: Alex <alex.tech.lab@outlook.com>")[#649](https://github.com/llm-d/llm-d-kv-cache/pull/649)[)](https://github.com/llm-d/llm-d-kv-cache/commit/500b6069cedb9f0d863ba6c8744bc15e7ca814c2 "fix\(pvc_evictor\): configure activator log level via config_dict \(#649\)

* fix\(pvc_evictor\): configure activator log level via config_dict

Pass config_dict to the activator process to dynamically set the logging level and log file path. This replaces the hardcoded INFO level and environment variable fallback, enabling consistent and configurable logging across processes. Updated evictor.py to forward the config during initialization and respawn, and added the local virtual environment to .gitignore.

Signed-off-by: Alex <alex.tech.lab@outlook.com>

* fix\(pvc_evictor\): use defensive .get\(\) for log_level across all processes

Replace direct dict access config_dict\["log_level"\] with
config_dict.get\("log_level", "INFO"\) in activator, crawler, deleter,
and folder_cleaner for consistent defensive coding.

Signed-off-by: Alex <alex.tech.lab@outlook.com>

---------

Signed-off-by: Alex <alex.tech.lab@outlook.com>")  | Jun 10, 2026  |  
|   |   |  [[1/3] deprecating internal tokenization on the lookup path (](https://github.com/llm-d/llm-d-kv-cache/commit/90088ae08194e96e943e5a84b649c47ee6ed8d76 "\[1/3\] deprecating internal tokenization on the lookup path \(#543\)

* chore: deprecate internal tokenization \(phase 1 of #517\)

Mark the prompt-string indexer APIs \(GetPodScores, ComputeBlockKeys,
SetTokenizer\) and the internal tokenization.Pool as Deprecated. Make
TokenizersPoolConfig optional in the indexer: when nil, no pool is
created and the deprecated entry points return an explicit error
pointing callers at ScoreTokens.

Add Indexer.ComputeBlockKeysFromTokens as the tokens-in counterpart
of ComputeBlockKeys. Drop the auto-populated TokenizersPoolConfig
from NewDefaultConfig so new integrations opt out by default.

Examples and e2e suites that still demonstrate the prompt-string
flow opt in explicitly via a new helper.ConfigureInternalTokenizer.
Architecture and configuration docs carry deprecation callouts that
redirect to the tokens-in path.

* address PR #543 review feedback

- Export ErrInternalTokenizationDisabled so callers can detect the
  missing-pool case via errors.Is.
- docs/architecture.md: drop the obsolete CachedLocalTokenizer /
  CachedHFTokenizer / CompositeTokenizer subsection \(those backends
  were removed in #473\) and distinguish EPP-side tokenization from
  the indexer-side UDS sidecar.
- docs/architecture.md: AllBlocksCleared description and sequence
  diagram now reflect the current implementation, which logs the
  event but does not yet clear per-pod entries.
- examples/kv_cache_aware_scorer \(build-excluded\): drop the dead
  HFTokenizerConfig setup that referenced types removed in #473.")[#543](https://github.com/llm-d/llm-d-kv-cache/pull/543)[)](https://github.com/llm-d/llm-d-kv-cache/commit/90088ae08194e96e943e5a84b649c47ee6ed8d76 "\[1/3\] deprecating internal tokenization on the lookup path \(#543\)

* chore: deprecate internal tokenization \(phase 1 of #517\)

Mark the prompt-string indexer APIs \(GetPodScores, ComputeBlockKeys,
SetTokenizer\) and the internal tokenization.Pool as Deprecated. Make
TokenizersPoolConfig optional in the indexer: when nil, no pool is
created and the deprecated entry points return an explicit error
pointing callers at ScoreTokens.

Add Indexer.ComputeBlockKeysFromTokens as the tokens-in counterpart
of ComputeBlockKeys. Drop the auto-populated TokenizersPoolConfig
from NewDefaultConfig so new integrations opt out by default.

Examples and e2e suites that still demonstrate the prompt-string
flow opt in explicitly via a new helper.ConfigureInternalTokenizer.
Architecture and configuration docs carry deprecation callouts that
redirect to the tokens-in path.

* address PR #543 review feedback

- Export ErrInternalTokenizationDisabled so callers can detect the
  missing-pool case via errors.Is.
- docs/architecture.md: drop the obsolete CachedLocalTokenizer /
  CachedHFTokenizer / CompositeTokenizer subsection \(those backends
  were removed in #473\) and distinguish EPP-side tokenization from
  the indexer-side UDS sidecar.
- docs/architecture.md: AllBlocksCleared description and sequence
  diagram now reflect the current implementation, which logs the
  event but does not yet clear per-pod entries.
- examples/kv_cache_aware_scorer \(build-excluded\): drop the dead
  HFTokenizerConfig setup that referenced types removed in #473.")  | Apr 27, 2026  |  
|   |   |  [Ignore large reformatting PRs in git blame (](https://github.com/llm-d/llm-d-kv-cache/commit/fe9af521205d1bbd8a093927042c3205160fb8c6 "Ignore large reformatting PRs in git blame \(#364\)

Signed-off-by: Kay Yan <kay.yan@daocloud.io>")[#364](https://github.com/llm-d/llm-d-kv-cache/pull/364)[)](https://github.com/llm-d/llm-d-kv-cache/commit/fe9af521205d1bbd8a093927042c3205160fb8c6 "Ignore large reformatting PRs in git blame \(#364\)

Signed-off-by: Kay Yan <kay.yan@daocloud.io>")  | Feb 27, 2026  |  
| [.pre-commit-config.yaml](https://github.com/llm-d/llm-d-kv-cache/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  | [.pre-commit-config.yaml](https://github.com/llm-d/llm-d-kv-cache/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  |  [feat(tokenization): replace RenderChat with RenderChatCompletion RPC (](https://github.com/llm-d/llm-d-kv-cache/commit/c66fd5c4a4237c8979e06a4e87ae052570b84eeb "feat\(tokenization\): replace RenderChat with RenderChatCompletion RPC \(#432\)

* render

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* render chat

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* update test assertions

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* update e2e tests

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* update python tests to use protos

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* python grpc

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* update protos

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* update python grpc renderer service with native protos instead of request json

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* parse tools

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* revert naming

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* naming fix

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* naming fix

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* nolint grpc files

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* lint

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* ruff ignore grpc files

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* ruff ignore grpc files in ci

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* align protos

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* fix getter name

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* fix tests

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* one prompt per completion

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* update tests

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* add_generation_prompt default true

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* simplify tools to json

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* unused import

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* empty commit

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* empty commit

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* doc fix

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* cr rename test

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* fix default omitted values

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

---------

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>")[#…](https://github.com/llm-d/llm-d-kv-cache/pull/432)  | Mar 28, 2026  |  
|   |   |   | Sep 11, 2025  |  
|   |   |   | Feb 25, 2026  |  
|   |   |   | May 20, 2025  |  
|   |   |   | May 21, 2025  |  
|   |   |   | Apr 18, 2026  |  
|   |   |   | Jul 31, 2025  |  
|   |   | [feat(telemetry): align kv-cache tracing with the standard llm-d obser…](https://github.com/llm-d/llm-d-kv-cache/commit/f453b6a49a31fe75ae14afa2dc9a0b86325d4b1b "feat\(telemetry\): align kv-cache tracing with the standard llm-d observability pattern \(#653\)

Signed-off-by: Guangya Liu <gyliu513@gmail.com>")  | Jun 11, 2026  |  
|   |   |   | Feb 25, 2026  |  
|   |   |  [docs: point the KV-Cache Indexer to llm-d-router (](https://github.com/llm-d/llm-d-kv-cache/commit/dade211e012fcde5114b3e8f19c2074b6c3ca583 "docs: point the KV-Cache Indexer to llm-d-router \(#700\)

* docs: point the KV-Cache Indexer to llm-d-router

The kvcache, kvblock, and kvevents libraries were internalized into
llm-d-router \(llm-d/llm-d-router#1886\). Add a migration notice at the top
of the README directing new development and PRs to the router.

Signed-off-by: Maroon Ayoub <mayoub@redhat.com>

* Apply suggestions from code review

Co-authored-by: Copilot Autofix powered by AI <175728472+Copilot@users.noreply.github.com>
Signed-off-by: Maroon Ayoub <Maroonay@gmail.com>

---------

Signed-off-by: Maroon Ayoub <mayoub@redhat.com>
Signed-off-by: Maroon Ayoub <Maroonay@gmail.com>
Co-authored-by: Copilot Autofix powered by AI <175728472+Copilot@users.noreply.github.com>")[#700](https://github.com/llm-d/llm-d-kv-cache/pull/700)[)](https://github.com/llm-d/llm-d-kv-cache/commit/dade211e012fcde5114b3e8f19c2074b6c3ca583 "docs: point the KV-Cache Indexer to llm-d-router \(#700\)

* docs: point the KV-Cache Indexer to llm-d-router

The kvcache, kvblock, and kvevents libraries were internalized into
llm-d-router \(llm-d/llm-d-router#1886\). Add a migration notice at the top
of the README directing new development and PRs to the router.

Signed-off-by: Maroon Ayoub <mayoub@redhat.com>

* Apply suggestions from code review

Co-authored-by: Copilot Autofix powered by AI <175728472+Copilot@users.noreply.github.com>
Signed-off-by: Maroon Ayoub <Maroonay@gmail.com>

---------

Signed-off-by: Maroon Ayoub <mayoub@redhat.com>
Signed-off-by: Maroon Ayoub <Maroonay@gmail.com>
Co-authored-by: Copilot Autofix powered by AI <175728472+Copilot@users.noreply.github.com>")  | Jul 14, 2026  |  
|   |   |   | Feb 25, 2026  |  
|   |   | [feat(telemetry): align kv-cache tracing with the standard llm-d obser…](https://github.com/llm-d/llm-d-kv-cache/commit/f453b6a49a31fe75ae14afa2dc9a0b86325d4b1b "feat\(telemetry\): align kv-cache tracing with the standard llm-d observability pattern \(#653\)

Signed-off-by: Guangya Liu <gyliu513@gmail.com>")  | Jun 11, 2026  |  
|   |   | [feat(telemetry): align kv-cache tracing with the standard llm-d obser…](https://github.com/llm-d/llm-d-kv-cache/commit/f453b6a49a31fe75ae14afa2dc9a0b86325d4b1b "feat\(telemetry\): align kv-cache tracing with the standard llm-d observability pattern \(#653\)

Signed-off-by: Guangya Liu <gyliu513@gmail.com>")  | Jun 11, 2026  |  
|   |   |  [update: add make target to python lint + fix lint (](https://github.com/llm-d/llm-d-kv-cache/commit/cc3f9fa53b0860f5bb842031ba1d85ff3c2a7577 "update: add make target to python lint + fix lint \(#536\)

* update: add lint for python + fix lint

- run "make lint" now does both golang and python

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: add back python 3.12 in makefile for unit test UDS

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: install ruff than only check it

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")[#536](https://github.com/llm-d/llm-d-kv-cache/pull/536)[)](https://github.com/llm-d/llm-d-kv-cache/commit/cc3f9fa53b0860f5bb842031ba1d85ff3c2a7577 "update: add make target to python lint + fix lint \(#536\)

* update: add lint for python + fix lint

- run "make lint" now does both golang and python

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: add back python 3.12 in makefile for unit test UDS

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: install ruff than only check it

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")  | May 10, 2026  |  
| View all files |  
## Repository files navigation
# KV-Cache
Important
The **KV-Cache Indexer** — the `kvcache`, `kvblock`, and `kvevents` libraries that power KV-cache-aware routing — has moved to [llm-d-router](https://github.com/llm-d/llm-d-router) ([llm-d/llm-d-router#1886](https://github.com/llm-d/llm-d-router/pull/1886)). New development happens there. A few imports of deprecated modules remain in this repository; once removed, no scheduling-related logic will live here. Open PRs against these libraries should be replicated against llm-d-router.
### Introduction
Efficiently caching Key & Value (KV) tensors is crucial for optimizing LLM inference. Reusing the KV-Cache, rather than recomputing it, significantly improves both Time To First Token (TTFT) and overall throughput, while also maximizing system resource-utilization. As a distributed LLM inference platform, `llm-d` provides a comprehensive suite of KV-Cache management capabilities to achieve these goals.
This repository contains the `llm-d-kv-cache`, a pluggable service designed to enable **KV-Cache Aware Routing** and lay the foundation for advanced, cross-node cache coordination in vLLM-based serving platforms.
### Project Northstar
See the [Project Northstar](https://docs.google.com/document/d/1EM1QtDUaw7pVRkbHQFTSCQhmWqAcRPJugJgqPbvzGTA/edit?tab=t.ikcvw3heciha) document for a detailed overview of the project's goals and vision.
## KV-Cache Indexer Overview
The major component of this project is the **KV-Cache Indexer** is a high-performance library that keeps a global, near-real-time view of KV-Cache block locality across a fleet of vLLM pods.
It is powered by `KVEvents` streamed from vLLM, which provide structured metadata as KV-blocks are created or evicted from a vLLM instance's KV-cache. This allows the indexer to track which blocks reside on which nodes and on which tier (e.g., GPU or CPU). This metadata is the foundation for intelligent routing, enabling schedulers to make optimal, KV-cache-aware placement decisions.
The diagram below shows the primary data flows: the **Read Path** (scoring) and the **Write Path** (event ingestion).
Loading

```
graph TD
    subgraph "Inference Scheduler"
        A[Scheduler]

        subgraph "KV-Cache"
            B[`kvcache.Indexer`]
            C[`kvblock.Index`]
            D[`kvevents.Pool`]
        end
    end

    subgraph "vLLM Fleet"
        E[vLLM Pod 1]
        F[vLLM Pod 2]
        G[...]
    end

    A--"1: Score(prompt, pods)"-->B
    B--"2: Query Index"-->C
    B--"3: Return Scores"-->A
    
    E--"A: Emit KVEvents"-->D
    F--"A: Emit KVEvents"-->D
    D--"B: Update Index"-->C

```

**Read Path:**
  * 1: **Scoring Request** : A scheduler asks the **KVCache Indexer** to score a set of pods for a given prompt
  * 2: **Index Query** : The indexer calculates the necessary KV-block keys from the prompt and queries the **KV-Block Index** to see which pods have those blocks
  * 3: **Return Scores** : The indexer returns a map of pods and their corresponding KV-cache-hit scores to the scheduler


**Write Path:**
  * A: **Event Ingestion** : As vLLM pods create or evict KV-blocks, they emit `KVEvents` containing metadata about these changes
  * B: **Index Update** : The **Event Subscriber** consumes these events and updates the **KV-Block Index** in near-real-time


> For a more detailed breakdown, please see the high-level [Architecture](https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/architecture.md) and the [Configuration](https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/configuration.md) docs.
### Examples
  * [**KVCache Indexer**](https://github.com/llm-d/llm-d-kv-cache/blob/main/examples/kv_cache_index/README.md): A reference implementation showing how to run and use the `kvcache.Indexer` module
  * [**KVCache Aware Scorer**](https://github.com/llm-d/llm-d-kv-cache/blob/main/examples/kv_cache_aware_scorer/README.md): A reference implementation of how to integrate the `kvcache.Indexer` into a scheduler like the `llm-d-router`
  * [**KV-Events**](https://github.com/llm-d/llm-d-kv-cache/blob/main/examples/kv_events/README.md): Demonstrates how the KV-Cache libraries handles KV-Events through both an offline example with a dummy ZMQ publisher and an online example using a vLLM Helm chart.


## Connectors & Utilities
  * [**PVC Evictor**](https://github.com/llm-d/llm-d-kv-cache/blob/main/kv_connectors/pvc_evictor/README.md): Kubernetes-side utility that automatically manages disk space for the offloading connector's KV-cache storage on PVCs.
  * [**llmd-fs-backend**](https://github.com/llm-d/llm-d-kv-cache/blob/main/kv_connectors/llmd_fs_backend/README.md): Storage backend for vLLM's `OffloadingConnector` — moves KV-cache blocks between GPU and shared storage (local disk, shared filesystem, or object store). Pre-built wheels are published via the project's pip index; see the connector README for install instructions and configuration.
> [!IMPORTANT] **Now upstreamed into vLLM.** `llmd-fs-connector==0.23` (llm-d v0.8 / vLLM v0.23) is the **final release** — llmd-fs-backend is now the FS tier of vLLM's multi-tier offloading connector (`TieringOffloadingSpec`). All new features and support continue there; see the [vLLM KV offloading guide](https://docs.vllm.ai/en/latest/features/kv_offloading_usage/).


## About
Distributed KV cache scheduling & offloading libraries
### Topics
### Resources
### Code of conduct
### Contributing
### Security policy
### Stars
**177** stars
### Watchers
**28** watching
### Forks
[ forks](https://github.com/llm-d/llm-d-kv-cache/forks)
## 
[ v0.9.0Latest Jun 14, 2026 ](https://github.com/llm-d/llm-d-kv-cache/releases/tag/v0.9.0)
## 
## Languages
  * [Go48.2%](https://github.com/llm-d/llm-d-kv-cache/search?l=go)
  * [Python36.9%](https://github.com/llm-d/llm-d-kv-cache/search?l=python)
  * [C++9.2%](https://github.com/llm-d/llm-d-kv-cache/search?l=c%2B%2B)
  * [Makefile2.4%](https://github.com/llm-d/llm-d-kv-cache/search?l=makefile)
  * [Cuda1.5%](https://github.com/llm-d/llm-d-kv-cache/search?l=cuda)
  * [Shell0.7%](https://github.com/llm-d/llm-d-kv-cache/search?l=shell)
  * [Other1.1%](https://github.com/llm-d/llm-d-kv-cache/search?l=Other)


You can’t perform that action at this time. 
