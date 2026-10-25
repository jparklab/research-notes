---
title: 'GitHub - argonne-lcf/FIRST: Inference Gateway for Batch and Interactive LLM
  requests · GitHub'
id: github-argonne-lcffirst-inference-gateway-for-batch-and-interactive-llm-requests
tags:
- onprem-llm-inference-stacks-859099
- hpc-inference
- national-lab
created: '2026-10-23T17:23:01.725288Z'
updated: '2026-10-23T17:23:51.304955Z'
source: https://github.com/auroraGPT-ANL/inference-gateway
source_domain: github.com
fetched_at: '2026-10-23T17:23:01.700849Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: argonne-lcf/FIRST GitHub repo (open-source gateway for ALCF Inference Service),
  847 commits as of Oct 23 2026; commits show deployed catalog on Minerva/Tara (muse-spark-bf16,
  inkling-bf16, nemotron-3-ultra, gpt-oss-20b, beam-1.0-bf16), gpt-oss-120b retained
  on Sophia, health monitor, TPM rate limiting, qstat/PBS parsing, 'V1 pilot adapter'.
---

*Suggested by [[first-federated-inference-resource-scheduling-toolkit-for]] — cited as primary source*

[Skip to content](https://github.com/argonne-lcf/FIRST#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/argonne-lcf/FIRST) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/argonne-lcf/FIRST) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/argonne-lcf/FIRST) to refresh your session. Dismiss alert
[**30** Branches](https://github.com/argonne-lcf/FIRST/branches)[](https://github.com/argonne-lcf/FIRST/tags)
Go to file
Open more actions menu
## Latest commit
success
Oct 23, 2026
[09ef226](https://github.com/argonne-lcf/FIRST/commit/09ef226f281806f3606ac7ddde95ac0da3bbf7d8) · Oct 23, 2026
## History
[847 Commits](https://github.com/argonne-lcf/FIRST/commits/main/)
Open commit details
847 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| [.github/workflows](https://github.com/argonne-lcf/FIRST/tree/main/.github/workflows "This path skips through empty directories")  | [.github/workflows](https://github.com/argonne-lcf/FIRST/tree/main/.github/workflows "This path skips through empty directories")  |  [ci: allow builds with non-](https://github.com/argonne-lcf/FIRST/commit/e8e6e13a014b8a9303bf10c4430696b8897b264f "ci: allow builds with non-`alcf_ai/*` tags \(#291\)") [tags (](https://github.com/argonne-lcf/FIRST/commit/e8e6e13a014b8a9303bf10c4430696b8897b264f "ci: allow builds with non-`alcf_ai/*` tags \(#291\)")[#291](https://github.com/argonne-lcf/FIRST/pull/291)[)](https://github.com/argonne-lcf/FIRST/commit/e8e6e13a014b8a9303bf10c4430696b8897b264f "ci: allow builds with non-`alcf_ai/*` tags \(#291\)")  | Aug 19, 2026  |  
|   |   |  [alcf_ai/agents: default to muse-spark-bf16 (](https://github.com/argonne-lcf/FIRST/commit/6e203826dd1d3819a037a902f9848c8057b4bf35 "alcf_ai/agents: default to muse-spark-bf16 \(#384\)

* alcf_ai/agents: default --default-model to muse-spark-bf16

Align the CLI default with the inference-endpoints docs, which now
suggest muse-spark-bf16 as the default agent model. inkling-bf16 stays
whitelisted and remains selectable with --default-model.

* alcf_ai/agents: sync the allowlist with the deployed catalog

Adds beam-1.0-bf16, openai/gpt-oss-20b, nemotron-35-super-vl,
inkling-bf16, and nemotron-3-ultra from Minerva and Tara, and drops
gpt-oss-120b from Minerva to match the docs retiring it there ahead
of a replacement \(argonne-lcf/user-guides#1387\); it stays on Sophia.")[#384](https://github.com/argonne-lcf/FIRST/pull/384)[)](https://github.com/argonne-lcf/FIRST/commit/6e203826dd1d3819a037a902f9848c8057b4bf35 "alcf_ai/agents: default to muse-spark-bf16 \(#384\)

* alcf_ai/agents: default --default-model to muse-spark-bf16

Align the CLI default with the inference-endpoints docs, which now
suggest muse-spark-bf16 as the default agent model. inkling-bf16 stays
whitelisted and remains selectable with --default-model.

* alcf_ai/agents: sync the allowlist with the deployed catalog

Adds beam-1.0-bf16, openai/gpt-oss-20b, nemotron-35-super-vl,
inkling-bf16, and nemotron-3-ultra from Minerva and Tara, and drops
gpt-oss-120b from Minerva to match the docs retiring it there ahead
of a replacement \(argonne-lcf/user-guides#1387\); it stays on Sophia.")  | Oct 22, 2026  |  
|   |   |  [LCF-16 Backup transfer endpoint (](https://github.com/argonne-lcf/FIRST/commit/afd5ada5892c79444893003a4590e573001df0cb "LCF-16 Backup transfer endpoint \(#257\)

* adding instructions on how to setup transfer endpoints

* Added refinement on previous attempt

* added more clarification, streamlined approach

---------

Co-authored-by: BenoitCote <benoitcote@anlvpn208.mcs.anl.gov>
Co-authored-by: BenoitCote <benoitcote@BenoitCotes-MacBook-Pro.local>")[#257](https://github.com/argonne-lcf/FIRST/pull/257)[)](https://github.com/argonne-lcf/FIRST/commit/afd5ada5892c79444893003a4590e573001df0cb "LCF-16 Backup transfer endpoint \(#257\)

* adding instructions on how to setup transfer endpoints

* Added refinement on previous attempt

* added more clarification, streamlined approach

---------

Co-authored-by: BenoitCote <benoitcote@anlvpn208.mcs.anl.gov>
Co-authored-by: BenoitCote <benoitcote@BenoitCotes-MacBook-Pro.local>")  | Jun 12, 2026  |  
|   |   |   | Jun 9, 2026  |  
|   |   |  [compute_functions: fix qstat parsing edge case (](https://github.com/argonne-lcf/FIRST/commit/aba2e05689259ba5e32d8d3e022c6cc6d67b3cb6 "compute_functions: fix qstat parsing edge case \(#289\)

`qstat` outputs with leading data before a job ID will cause malformed
chunks to populate `jobs_raw` and raise exceptions whilst attempting to
parse the response.")[#289](https://github.com/argonne-lcf/FIRST/pull/289)[)](https://github.com/argonne-lcf/FIRST/commit/aba2e05689259ba5e32d8d3e022c6cc6d67b3cb6 "compute_functions: fix qstat parsing edge case \(#289\)

`qstat` outputs with leading data before a job ID will cause malformed
chunks to populate `jobs_raw` and raise exceptions whilst attempting to
parse the response.")  | Aug 19, 2026  |  
|   |   | [health_monitor: report Minerva and Metis models missing from the stat…](https://github.com/argonne-lcf/FIRST/commit/265d1e7e22cbce9097148f453028915d697d7170 "health_monitor: report Minerva and Metis models missing from the status endpoint \(#373\)

* health_monitor: report Minerva models missing from the status endpoint

* health_monitor: report Metis models missing from the status endpoint

* health_monitor: share the Minerva and Metis checks")  | Oct 14, 2026  |  
|   |   |   | May 12, 2026  |  
|   |   |  [fix: support long-prefill requests through V1 pilot adapter (](https://github.com/argonne-lcf/FIRST/commit/cf37443e96cf12cfdd5ac4d7ab2ba0acd3e3f88d "fix: support long-prefill requests through V1 pilot adapter \(#383\)")[#383](https://github.com/argonne-lcf/FIRST/pull/383)[)](https://github.com/argonne-lcf/FIRST/commit/cf37443e96cf12cfdd5ac4d7ab2ba0acd3e3f88d "fix: support long-prefill requests through V1 pilot adapter \(#383\)")  | Oct 21, 2026  |  
| [feat(minerva): expose muse-spark-bf16 with reasoning effort for openc…](https://github.com/argonne-lcf/FIRST/commit/c76ddfebb6f319daf2e3d4568c7ccf3e1732fc7c "feat\(minerva\): expose muse-spark-bf16 with reasoning effort for opencode \(#378\)

- Add muse-spark-bf16 to minerva ALLOWLIST so alcf-ai configure opencode
  includes it without --include-experimental
- Fix edit_opencode to set variants/options when only effort_levels is
  present \(fallback to medium/first\), enabling Ctrl+T effort display
- Document required gateway fixture capabilities.reasoning with
  effort_levels/default_effort and MODEL_DETAILS_KEYS allowlist
- Update API reference example to show effort_levels for reasoning models

Co-authored-by: opencode <opencode@alcf.anl.gov>")  | Oct 16, 2026  |  
| [examples/load-testing](https://github.com/argonne-lcf/FIRST/tree/main/examples/load-testing "This path skips through empty directories")  | [examples/load-testing](https://github.com/argonne-lcf/FIRST/tree/main/examples/load-testing "This path skips through empty directories")  |  [LCF-70 and LCF-71: Refactor access logging and error handling (](https://github.com/argonne-lcf/FIRST/commit/4d016f53df4d36a7ea936f93419fa0d40c140589 "LCF-70 and LCF-71: Refactor access logging and error handling \(#234\)

* Error handling and TPM rate limiting

* WIP

* WIP

* Mypy passing

* WIP

* WIP

* Add structlogging to mirror database logs

* Enable type checking in CI

* WIP

* Update test cases

* Fix CI

* Split out views and update logging formatters

* Clean up

* Improve auth check

* Fix shadowing schema issue

* logging fix

* Fix serialization of related attributes in structlog

* Improve resilience of get_jobs endpiont config parsing

* Improve logging configuration

* Bug fixes

* Bug fixes

* Improve globus-compute deserialization behavior

* Bug fixes

* Bugfix

* Update log config

* typo

* Structured stdout logging to avoid multiprocess file rotation issues and facilitate k8s migration

* Update logging

* Cleanup

* Update gunicorn root logger config

* Update

* updates

* Update health check

* Coerce UUID to str for BatchLogSummary

* Update batch list

* Fix batchlog update

* Update TPM accounting for streaming

* Update TPM accounting for streaming

* Improve TPM logging

* Fix cooldown state management

* Fix cooldown state management with logs

* Optimizations

* Update systemd gateway_async.service

* Remove DB logging except when using Batch system

* added status route to be able to report up/down/degraded for the IRI Facility Status API

* Remove dependence on DB logs

* incorporated async in authorized_clusters line

* WIP

* Update tests

* Update tests, fix name mangling, and fix user logging record collision

* Update default metrics recording

* Add BatchLog DB migration

* Cleanup names

---------

Co-authored-by: BenoitCote <benoitcote@anlvpn077.mcs.anl.gov>")[#234](https://github.com/argonne-lcf/FIRST/pull/234)[)](https://github.com/argonne-lcf/FIRST/commit/4d016f53df4d36a7ea936f93419fa0d40c140589 "LCF-70 and LCF-71: Refactor access logging and error handling \(#234\)

* Error handling and TPM rate limiting

* WIP

* WIP

* Mypy passing

* WIP

* WIP

* Add structlogging to mirror database logs

* Enable type checking in CI

* WIP

* Update test cases

* Fix CI

* Split out views and update logging formatters

* Clean up

* Improve auth check

* Fix shadowing schema issue

* logging fix

* Fix serialization of related attributes in structlog

* Improve resilience of get_jobs endpiont config parsing

* Improve logging configuration

* Bug fixes

* Bug fixes

* Improve globus-compute deserialization behavior

* Bug fixes

* Bugfix

* Update log config

* typo

* Structured stdout logging to avoid multiprocess file rotation issues and facilitate k8s migration

* Update logging

* Cleanup

* Update gunicorn root logger config

* Update

* updates

* Update health check

* Coerce UUID to str for BatchLogSummary

* Update batch list

* Fix batchlog update

* Update TPM accounting for streaming

* Update TPM accounting for streaming

* Improve TPM logging

* Fix cooldown state management

* Fix cooldown state management with logs

* Optimizations

* Update systemd gateway_async.service

* Remove DB logging except when using Batch system

* added status route to be able to report up/down/degraded for the IRI Facility Status API

* Remove dependence on DB logs

* incorporated async in authorized_clusters line

* WIP

* Update tests

* Update tests, fix name mangling, and fix user logging record collision

* Update default metrics recording

* Add BatchLog DB migration

* Cleanup names

---------

Co-authored-by: BenoitCote <benoitcote@anlvpn077.mcs.anl.gov>")  | May 11, 2026  |  
|   |   |  [LCF-70 and LCF-71: Refactor access logging and error handling (](https://github.com/argonne-lcf/FIRST/commit/4d016f53df4d36a7ea936f93419fa0d40c140589 "LCF-70 and LCF-71: Refactor access logging and error handling \(#234\)

* Error handling and TPM rate limiting

* WIP

* WIP

* Mypy passing

* WIP

* WIP

* Add structlogging to mirror database logs

* Enable type checking in CI

* WIP

* Update test cases

* Fix CI

* Split out views and update logging formatters

* Clean up

* Improve auth check

* Fix shadowing schema issue

* logging fix

* Fix serialization of related attributes in structlog

* Improve resilience of get_jobs endpiont config parsing

* Improve logging configuration

* Bug fixes

* Bug fixes

* Improve globus-compute deserialization behavior

* Bug fixes

* Bugfix

* Update log config

* typo

* Structured stdout logging to avoid multiprocess file rotation issues and facilitate k8s migration

* Update logging

* Cleanup

* Update gunicorn root logger config

* Update

* updates

* Update health check

* Coerce UUID to str for BatchLogSummary

* Update batch list

* Fix batchlog update

* Update TPM accounting for streaming

* Update TPM accounting for streaming

* Improve TPM logging

* Fix cooldown state management

* Fix cooldown state management with logs

* Optimizations

* Update systemd gateway_async.service

* Remove DB logging except when using Batch system

* added status route to be able to report up/down/degraded for the IRI Facility Status API

* Remove dependence on DB logs

* incorporated async in authorized_clusters line

* WIP

* Update tests

* Update tests, fix name mangling, and fix user logging record collision

* Update default metrics recording

* Add BatchLog DB migration

* Cleanup names

---------

Co-authored-by: BenoitCote <benoitcote@anlvpn077.mcs.anl.gov>")[#234](https://github.com/argonne-lcf/FIRST/pull/234)[)](https://github.com/argonne-lcf/FIRST/commit/4d016f53df4d36a7ea936f93419fa0d40c140589 "LCF-70 and LCF-71: Refactor access logging and error handling \(#234\)

* Error handling and TPM rate limiting

* WIP

* WIP

* Mypy passing

* WIP

* WIP

* Add structlogging to mirror database logs

* Enable type checking in CI

* WIP

* Update test cases

* Fix CI

* Split out views and update logging formatters

* Clean up

* Improve auth check

* Fix shadowing schema issue

* logging fix

* Fix serialization of related attributes in structlog

* Improve resilience of get_jobs endpiont config parsing

* Improve logging configuration

* Bug fixes

* Bug fixes

* Improve globus-compute deserialization behavior

* Bug fixes

* Bugfix

* Update log config

* typo

* Structured stdout logging to avoid multiprocess file rotation issues and facilitate k8s migration

* Update logging

* Cleanup

* Update gunicorn root logger config

* Update

* updates

* Update health check

* Coerce UUID to str for BatchLogSummary

* Update batch list

* Fix batchlog update

* Update TPM accounting for streaming

* Update TPM accounting for streaming

* Improve TPM logging

* Fix cooldown state management

* Fix cooldown state management with logs

* Optimizations

* Update systemd gateway_async.service

* Remove DB logging except when using Batch system

* added status route to be able to report up/down/degraded for the IRI Facility Status API

* Remove dependence on DB logs

* incorporated async in authorized_clusters line

* WIP

* Update tests

* Update tests, fix name mangling, and fix user logging record collision

* Update default metrics recording

* Add BatchLog DB migration

* Cleanup names

---------

Co-authored-by: BenoitCote <benoitcote@anlvpn077.mcs.anl.gov>")  | May 11, 2026  |  
|   |   |   | Sep 2, 2026  |  
|   |   |  [fix: support long-prefill requests through V1 pilot adapter (](https://github.com/argonne-lcf/FIRST/commit/cf37443e96cf12cfdd5ac4d7ab2ba0acd3e3f88d "fix: support long-prefill requests through V1 pilot adapter \(#383\)")[#383](https://github.com/argonne-lcf/FIRST/pull/383)[)](https://github.com/argonne-lcf/FIRST/commit/cf37443e96cf12cfdd5ac4d7ab2ba0acd3e3f88d "fix: support long-prefill requests through V1 pilot adapter \(#383\)")  | Oct 21, 2026  |  
|   |   |   | Nov 14, 2025  |  
|   |   |   | Jun 19, 2026  |  
| [.pre-commit-config.yaml](https://github.com/argonne-lcf/FIRST/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  | [.pre-commit-config.yaml](https://github.com/argonne-lcf/FIRST/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  |  [LCF-63: Automate Format and Lint Checks (](https://github.com/argonne-lcf/FIRST/commit/998b3841ad6670b9adb8a3208eae6e85fce075b2 "LCF-63: Automate Format and Lint Checks \(#230\)

* Add lint/format steps to CI and Makefile. Remove transitive deps and pins from pyproject.toml

* Formatting ONLY

* Ruff lint fixes

* Add pre-commit hooks")[#230](https://github.com/argonne-lcf/FIRST/pull/230)[)](https://github.com/argonne-lcf/FIRST/commit/998b3841ad6670b9adb8a3208eae6e85fce075b2 "LCF-63: Automate Format and Lint Checks \(#230\)

* Add lint/format steps to CI and Makefile. Remove transitive deps and pins from pyproject.toml

* Formatting ONLY

* Ruff lint fixes

* Add pre-commit hooks")  | Apr 21, 2026  |  
|   |   |   | Sep 2, 2026  |  
|   |   |   | Apr 26, 2024  |  
|   |   |  [LCF-70 and LCF-71: Refactor access logging and error handling (](https://github.com/argonne-lcf/FIRST/commit/4d016f53df4d36a7ea936f93419fa0d40c140589 "LCF-70 and LCF-71: Refactor access logging and error handling \(#234\)

* Error handling and TPM rate limiting

* WIP

* WIP

* Mypy passing

* WIP

* WIP

* Add structlogging to mirror database logs

* Enable type checking in CI

* WIP

* Update test cases

* Fix CI

* Split out views and update logging formatters

* Clean up

* Improve auth check

* Fix shadowing schema issue

* logging fix

* Fix serialization of related attributes in structlog

* Improve resilience of get_jobs endpiont config parsing

* Improve logging configuration

* Bug fixes

* Bug fixes

* Improve globus-compute deserialization behavior

* Bug fixes

* Bugfix

* Update log config

* typo

* Structured stdout logging to avoid multiprocess file rotation issues and facilitate k8s migration

* Update logging

* Cleanup

* Update gunicorn root logger config

* Update

* updates

* Update health check

* Coerce UUID to str for BatchLogSummary

* Update batch list

* Fix batchlog update

* Update TPM accounting for streaming

* Update TPM accounting for streaming

* Improve TPM logging

* Fix cooldown state management

* Fix cooldown state management with logs

* Optimizations

* Update systemd gateway_async.service

* Remove DB logging except when using Batch system

* added status route to be able to report up/down/degraded for the IRI Facility Status API

* Remove dependence on DB logs

* incorporated async in authorized_clusters line

* WIP

* Update tests

* Update tests, fix name mangling, and fix user logging record collision

* Update default metrics recording

* Add BatchLog DB migration

* Cleanup names

---------

Co-authored-by: BenoitCote <benoitcote@anlvpn077.mcs.anl.gov>")[#234](https://github.com/argonne-lcf/FIRST/pull/234)[)](https://github.com/argonne-lcf/FIRST/commit/4d016f53df4d36a7ea936f93419fa0d40c140589 "LCF-70 and LCF-71: Refactor access logging and error handling \(#234\)

* Error handling and TPM rate limiting

* WIP

* WIP

* Mypy passing

* WIP

* WIP

* Add structlogging to mirror database logs

* Enable type checking in CI

* WIP

* Update test cases

* Fix CI

* Split out views and update logging formatters

* Clean up

* Improve auth check

* Fix shadowing schema issue

* logging fix

* Fix serialization of related attributes in structlog

* Improve resilience of get_jobs endpiont config parsing

* Improve logging configuration

* Bug fixes

* Bug fixes

* Improve globus-compute deserialization behavior

* Bug fixes

* Bugfix

* Update log config

* typo

* Structured stdout logging to avoid multiprocess file rotation issues and facilitate k8s migration

* Update logging

* Cleanup

* Update gunicorn root logger config

* Update

* updates

* Update health check

* Coerce UUID to str for BatchLogSummary

* Update batch list

* Fix batchlog update

* Update TPM accounting for streaming

* Update TPM accounting for streaming

* Improve TPM logging

* Fix cooldown state management

* Fix cooldown state management with logs

* Optimizations

* Update systemd gateway_async.service

* Remove DB logging except when using Batch system

* added status route to be able to report up/down/degraded for the IRI Facility Status API

* Remove dependence on DB logs

* incorporated async in authorized_clusters line

* WIP

* Update tests

* Update tests, fix name mangling, and fix user logging record collision

* Update default metrics recording

* Add BatchLog DB migration

* Cleanup names

---------

Co-authored-by: BenoitCote <benoitcote@anlvpn077.mcs.anl.gov>")  | May 11, 2026  |  
|   |   |  [docs: update repo url for hrefs (](https://github.com/argonne-lcf/FIRST/commit/e615b4fdc5e0f7aa1f3a8ddd6f01b4886c558744 "docs: update repo url for hrefs \(#253\)")[#253](https://github.com/argonne-lcf/FIRST/pull/253)[)](https://github.com/argonne-lcf/FIRST/commit/e615b4fdc5e0f7aa1f3a8ddd6f01b4886c558744 "docs: update repo url for hrefs \(#253\)")  | Jun 9, 2026  |  
|   |   |  [Document and test public model capability metadata (](https://github.com/argonne-lcf/FIRST/commit/329c0cdc4d58a361ab9b2a76de65e8f523400eea "Document and test public model capability metadata \(#306\)")[#306](https://github.com/argonne-lcf/FIRST/pull/306)[)](https://github.com/argonne-lcf/FIRST/commit/329c0cdc4d58a361ab9b2a76de65e8f523400eea "Document and test public model capability metadata \(#306\)")  | Aug 21, 2026  |  
|   |   | [updated VM title in the architecture image](https://github.com/argonne-lcf/FIRST/commit/57b6430739147ddb3f5d5f24e0e09462b28c3613 "updated VM title in the architecture image")  | Oct 8, 2025  |  
| [inference_gateway_architecture_focused.png](https://github.com/argonne-lcf/FIRST/blob/main/inference_gateway_architecture_focused.png "inference_gateway_architecture_focused.png")  | [inference_gateway_architecture_focused.png](https://github.com/argonne-lcf/FIRST/blob/main/inference_gateway_architecture_focused.png "inference_gateway_architecture_focused.png")  |   | Apr 25, 2025  |  
|   |   |  [chore: switch to](https://github.com/argonne-lcf/FIRST/commit/ded5afe6ddddb59354eedc93bc0b2d8969d6e21a "chore: switch to `ruff` and format project \(#206\)

* pyproject: use `ruff` with options

Replaces `black`, `isort`, and the other formatters to centralize around
`ruff`.

* chore: apply formatting with `ruff`

* docs/contributing: add note about formatting with `ruff`") [and format project (](https://github.com/argonne-lcf/FIRST/commit/ded5afe6ddddb59354eedc93bc0b2d8969d6e21a "chore: switch to `ruff` and format project \(#206\)

* pyproject: use `ruff` with options

Replaces `black`, `isort`, and the other formatters to centralize around
`ruff`.

* chore: apply formatting with `ruff`

* docs/contributing: add note about formatting with `ruff`")[#206](https://github.com/argonne-lcf/FIRST/pull/206)[)](https://github.com/argonne-lcf/FIRST/commit/ded5afe6ddddb59354eedc93bc0b2d8969d6e21a "chore: switch to `ruff` and format project \(#206\)

* pyproject: use `ruff` with options

Replaces `black`, `isort`, and the other formatters to centralize around
`ruff`.

* chore: apply formatting with `ruff`

* docs/contributing: add note about formatting with `ruff`")  | Feb 3, 2026  |  
|   |   |  [Add Minerva session affinity and cache isolation (](https://github.com/argonne-lcf/FIRST/commit/f21b9780be39e56324131ddf029007d58174dcbe "Add Minerva session affinity and cache isolation \(#277\)

* Add Minerva session affinity and cache isolation

* Use shallow copies for direct API requests

* Restrict forwarded direct API headers")[#277](https://github.com/argonne-lcf/FIRST/pull/277)[)](https://github.com/argonne-lcf/FIRST/commit/f21b9780be39e56324131ddf029007d58174dcbe "Add Minerva session affinity and cache isolation \(#277\)

* Add Minerva session affinity and cache isolation

* Use shallow copies for direct API requests

* Restrict forwarded direct API headers")  | Aug 5, 2026  |  
|   |   |   | Aug 31, 2026  |  
|   |   |   | Jun 12, 2024  |  
|   |   |   | Oct 23, 2026  |  
| View all files |  
## Repository files navigation
# FIRST
FIRST (Federated Inference Resource Scheduling Toolkit) is a system that enables AI Model inference as a service, allowing secure, remote execution of AI models through an [OpenAI](https://platform.openai.com/docs/overview)-compatible API. FIRST's Inference Gateway is a RESTful API that validates and authorizes inference requests to scientific computing clusters using [Globus Auth](https://www.globus.org/globus-auth-service) and [Globus Compute](https://www.globus.org/compute).
## System Architecture
The Inference Gateway consists of several components:
  * **API Gateway** : [Django](https://www.djangoproject.com/)-based [REST](https://www.django-rest-framework.org/)/[Ninja](https://django-ninja.dev/) API that handles authorization and request routing.
  * **Globus Auth** : Authentication and authorization service.
  * **Globus Compute Endpoints** : Remote execution framework on HPC clusters (or local machines).
  * **Inference Server Backend** : (e.g., [vLLM](https://docs.vllm.ai/en/latest/)) High-performance inference service for LLMs running alongside the Globus Compute Endpoint.


## Documentation
📚 **Complete documentation is available at:<https://argonne-lcf.github.io/FIRST/>**
### Quick Links
  * **[Administrator Guide](https://argonne-lcf.github.io/FIRST/admin-guide/)** - Setup and deployment instructions
    * [Inference Backend Setup](https://argonne-lcf.github.io/FIRST/admin-guide/inference-setup/)
  * - How to use the inference API
  * **[Example Deployment](https://github.com/argonne-lcf/inference-endpoints)** - ALCF production deployment


## Citation
If you use ALCF Inference Endpoints or the Federated Inference Resource Scheduling Toolkit (FIRST) in your research or workflows, please cite our paper:

```
@inproceedings{10.1145/3731599.3767346,
  author = {Tanikanti, Aditya and C\^{o}t\'{e}, Benoit and Guo, Yanfei and Chen, Le and Saint, Nickolaus and Chard, Ryan and Raffenetti, Ken and Thakur, Rajeev and Uram, Thomas and Foster, Ian and Papka, Michael E. and Vishwanath, Venkatram},
  title = {FIRST: Federated Inference Resource Scheduling Toolkit for Scientific AI Model Access},
  year = {2025},
  isbn = {9798400718717},
  publisher = {Association for Computing Machinery},
  address = {New York, NY, USA},
  url = {https://doi.org/10.1145/3731599.3767346},
  doi = {10.1145/3731599.3767346},
  abstract = {We present the Federated Inference Resource Scheduling Toolkit (FIRST), a framework enabling Inference-as-a-Service across distributed High-Performance Computing (HPC) clusters. FIRST provides cloud-like access to diverse AI models, like Large Language Models (LLMs), on existing HPC infrastructure. Leveraging Globus Auth and Globus Compute, the system allows researchers to run parallel inference workloads via an OpenAI-compliant API on private, secure environments. This cluster-agnostic API allows requests to be distributed across federated clusters, targeting numerous hosted models. FIRST supports multiple inference backends (e.g., vLLM), auto-scales resources, maintains "hot" nodes for low-latency execution, and offers both high-throughput batch and interactive modes. The framework addresses the growing demand for private, secure, and scalable AI inference in scientific workflows, allowing researchers to generate billions of tokens daily on-premises without relying on commercial cloud infrastructure.},
  booktitle = {Proceedings of the SC '25 Workshops of the International Conference for High Performance Computing, Networking, Storage and Analysis},
  pages = {52–60},
  numpages = ,
  keywords = {Inference as a Service, High Performance Computing, Job Schedulers, Large Language Models, Globus, Scientific Computing},
  series = {SC Workshops '25}
}
```

## Acknowledgements
This work was supported by the U.S. Department of Energy, Office of Science, Office of Advanced Scientific Computing Research, under Contract No. DE-AC02-06CH11357. This research used resources of the Argonne Leadership Computing Facility, which is a DOE Office of Science User Facility.
## License
This project is licensed under the Apache License 2.0 - see the [LICENSE](https://github.com/argonne-lcf/FIRST/blob/main/LICENSE) file for details.
## About
Inference Gateway for Batch and Interactive LLM requests
[argonne-lcf.github.io/FIRST/](https://argonne-lcf.github.io/FIRST/)
### Resources
### Contributing
### Stars
**7** stars
### Watchers
**2** watching
### Forks
[ forks](https://github.com/argonne-lcf/FIRST/forks)
## 
[ Inference Gateway for FIRST v0.1.0 - Initial Public ReleaseLatest Apr 28, 2025 ](https://github.com/argonne-lcf/FIRST/releases/tag/v0.1.0)
## 
## Languages
  * [Python87.3%](https://github.com/argonne-lcf/FIRST/search?l=python)
  * [HTML6.8%](https://github.com/argonne-lcf/FIRST/search?l=html)
  * [Shell5.6%](https://github.com/argonne-lcf/FIRST/search?l=shell)
  * [Other0.3%](https://github.com/argonne-lcf/FIRST/search?l=Other)


You can’t perform that action at this time. 
