---
title: 'GitHub - NVIDIA-NeMo/Guardrails: NeMo Guardrails is an open-source toolkit
  for easily adding programmable guardrails to LLM-based conversational systems. ·
  GitHub'
id: github-nvidia-nemoguardrails-nemo-guardrails-is-an-open-source-toolkit-for-easil
tags:
- internal-llm-inference-stacks-7f0c90
- guardrails
- nemo-guardrails
- oss-repo
- oss-governance
created: '2026-10-09T22:56:55.808173Z'
updated: '2026-10-10T14:52:17.125467Z'
source: https://github.com/NVIDIA-NeMo/Guardrails
source_domain: github.com
fetched_at: '2026-10-09T22:56:55.785538Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub repo (NVIDIA-NeMo/Guardrails, v0.24.1, Sep 16 2026 release) for NeMo
  Guardrails, NVIDIA''s open-source programmable-guardrails toolkit for LLM applications
  (pip install nemoguardrails; Python 3.10-3.13; MIT-style OSS). 7.3k stars, 45 watchers,
  450+ network dependents, 3,840 commits, 466 branches, actively committed as of Oct
  9 2026 -- strong signal of real production adoption rather than an abandoned research
  toolkit. Documented and introduced in an arXiv paper (https://arxiv.org/abs/2310.10501)
  with a technical overview and evaluation -- a direct academic primary-source lead.
  Defines five rail types: input rails (reject/alter user input, e.g. PII masking),
  dialog rails (steer canonical-form conversation flow via Colang), retrieval rails
  (reject/alter RAG chunks before they reach the prompt), execution rails (wrap custom
  tool/action calls), and output rails (reject/alter LLM output before it reaches
  the user). Four named use cases: RAG fact-checking/output moderation, domain-specific
  chatbots staying on topic, adding guardrails to any custom LLM endpoint, and an
  optional LangChain integration layer. Usage is a two-step Python API (RailsConfig.from_path
  + LLMRails.generate/generate_async) mirroring the OpenAI Chat Completions request/response
  shape; async-first core. Lists ''LLM Vulnerability Scanning'' docs showing a sample
  jailbreak/prompt-injection protection matrix across configs on an example ''ABC
  Bot''. Shows active third-party rail integrations being merged upstream (e.g. a
  Clavata.ai content-policy integration contributed externally with ANY/ALL label-match
  logic and exponential-backoff rate-limit handling; a recent ''tool_safety_check''
  LLM-judged per-tool rail and per-tool rail scoping for tool calls/results), indicating
  the project is extensible beyond NVIDIA''s own built-in rails via community/vendor
  integrations rather than a closed, complete solution.'
utility_score: 14.0
---

[Skip to content](https://github.com/NVIDIA-NeMo/Guardrails#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/NVIDIA-NeMo/Guardrails) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/NVIDIA-NeMo/Guardrails) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/NVIDIA-NeMo/Guardrails) to refresh your session. Dismiss alert
[**466** Branches](https://github.com/NVIDIA-NeMo/Guardrails/branches)[](https://github.com/NVIDIA-NeMo/Guardrails/tags)
Go to file
Open more actions menu
## Latest commit
[feat(server): integrate OpenAI Chat buffered proxy (](https://github.com/NVIDIA-NeMo/Guardrails/commit/4512d081cb970be8a99ac2ccd1c6a39b1a81d352)[#2414](https://github.com/NVIDIA-NeMo/Guardrails/pull/2414)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/4512d081cb970be8a99ac2ccd1c6a39b1a81d352)
Open commit detailssuccess
Oct 9, 2026
[4512d08](https://github.com/NVIDIA-NeMo/Guardrails/commit/4512d081cb970be8a99ac2ccd1c6a39b1a81d352) · Oct 9, 2026
## History
[3,840 Commits](https://github.com/NVIDIA-NeMo/Guardrails/commits/develop/)
Open commit details
3,840 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| [.agents/skills](https://github.com/NVIDIA-NeMo/Guardrails/tree/develop/.agents/skills "This path skips through empty directories")  | [.agents/skills](https://github.com/NVIDIA-NeMo/Guardrails/tree/develop/.agents/skills "This path skips through empty directories")  |   | Jun 24, 2026  |  
|   |   |   | Jun 24, 2026  |  
|   |   |  [chore: drop Python 3.9 support ahead of October 2025 EOL (](https://github.com/NVIDIA-NeMo/Guardrails/commit/116c051665315a303f250115f288f81dd460cb6c "chore: drop Python 3.9 support ahead of October 2025 EOL \(#1426\)

Python 3.9 reaches end-of-life on October 5, 2025. Remove support
across:
- CI/CD workflows \(GitHub Actions, GitLab CI\)
- Testing configurations \(tox.ini\)
- Development containers
- Documentation and requirements")[#1426](https://github.com/NVIDIA-NeMo/Guardrails/pull/1426)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/116c051665315a303f250115f288f81dd460cb6c "chore: drop Python 3.9 support ahead of October 2025 EOL \(#1426\)

Python 3.9 reaches end-of-life on October 5, 2025. Remove support
across:
- CI/CD workflows \(GitHub Actions, GitLab CI\)
- Testing configurations \(tox.ini\)
- Development containers
- Documentation and requirements")  | Oct 13, 2025  |  
|   |   | [chore(deps): bump the github-actions group across 1 directory with 8 …](https://github.com/NVIDIA-NeMo/Guardrails/commit/ac7298165b1090b457f349581da45755d73eaa64 "chore\(deps\): bump the github-actions group across 1 directory with 8 updates \(#2313\)

Signed-off-by: dependabot\[bot\] <support@github.com>
Signed-off-by: Pouyanpi <13303554+Pouyanpi@users.noreply.github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>
Co-authored-by: Pouyanpi <13303554+Pouyanpi@users.noreply.github.com>")  | Aug 21, 2026  |  
|   |   |  [feat: add clavata community integration (](https://github.com/NVIDIA-NeMo/Guardrails/commit/17f68cc26401551f12134053501477bfe35db88d "feat: add clavata community integration \(#1027\)

* feat\(clavata\): add clavata integration

- Users of the Clavata integration can now specify the exact labels that must match for the input/output to cause the rail to trigger and abort the flow.
- Fixing some aspects of how the configuration is put together
- Policy ID aliases make it easier to specify a policy by name instead of ID.
- The new action `EvaluateUserInputWithClavataPolicy` allows you to evaluate the user input against a Clavata policy part of a flow that a user has written.
- Added the ability for a user to specify ANY/ALL logic for label matches.

Co-authored-by: Brett Levenson <brett@clavata.ai>
Signed-off-by: Ilias Tsangaris <iliastsangaris@gmail.com>

* Add tests

* Updated Clavata integration to support v1 and v2 colang

- v1 colang requires that the policies for input and output rails be specified in the config because parameters cannot be passed to flows
- v2 colang allows parameters in flow definitions, so it should be possible to simply pass the policy and label arguments at the time the flow is defined.

Also improved the Clavata Client to handle rate limit and exponential backoff situations.

* Adding notebook demostrating use of Clavata.ai wtth Guardrails

- Fixed a small bug with UUID format when sent to server

* PR feedback

* Updated PR to address feedback

- Separating examples for 1.0 and 2.x colang
- Consolidating action so there's only 1 "action" function for both 1.0 and 2.x \(with optional parameters-the action will figure out which approach to use based on what is passed to it\).
- Removed the extra action that was an example of returning something other than an boolean.

* Addressed all remaining review comments

- Added test coverage for all pydantic models used by the integration
- Added test coverage for the exp backoff decorator and the calculation of next retry time.
- Updated the example rails configuration for Colang v2.x so it parses correctly on startup with `nemoguardrails chat`
- Changed policy alias topology to use a dict to prevent accidental re-use of the same alias with a different ID. Updated example configs and documentation to show this difference. Notebook updated as well.
- Consolidated code for the Clavata check action: It now uses two helpers to determine the correct policy ID and labels to use. Precedence is given to policy/label that are passed directly to the action, but if values are not passed, the config will then be checked to obtain the correct values. This should make migration from colang 1 to v2.x smoother.

* run pre-commit

* use str in favor of httpurl

* fix issue with dashes getting stripped from policy ids

* python 3.9 compat

* Use union operator rather than pipe for python 3.9

---------

Signed-off-by: Ilias Tsangaris <iliastsangaris@gmail.com>
Co-authored-by: Brett Levenson <brett@clavata.ai>
Co-authored-by: Brett Levenson <bhlevenson@gmail.com>")[#1027](https://github.com/NVIDIA-NeMo/Guardrails/pull/1027)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/17f68cc26401551f12134053501477bfe35db88d "feat: add clavata community integration \(#1027\)

* feat\(clavata\): add clavata integration

- Users of the Clavata integration can now specify the exact labels that must match for the input/output to cause the rail to trigger and abort the flow.
- Fixing some aspects of how the configuration is put together
- Policy ID aliases make it easier to specify a policy by name instead of ID.
- The new action `EvaluateUserInputWithClavataPolicy` allows you to evaluate the user input against a Clavata policy part of a flow that a user has written.
- Added the ability for a user to specify ANY/ALL logic for label matches.

Co-authored-by: Brett Levenson <brett@clavata.ai>
Signed-off-by: Ilias Tsangaris <iliastsangaris@gmail.com>

* Add tests

* Updated Clavata integration to support v1 and v2 colang

- v1 colang requires that the policies for input and output rails be specified in the config because parameters cannot be passed to flows
- v2 colang allows parameters in flow definitions, so it should be possible to simply pass the policy and label arguments at the time the flow is defined.

Also improved the Clavata Client to handle rate limit and exponential backoff situations.

* Adding notebook demostrating use of Clavata.ai wtth Guardrails

- Fixed a small bug with UUID format when sent to server

* PR feedback

* Updated PR to address feedback

- Separating examples for 1.0 and 2.x colang
- Consolidating action so there's only 1 "action" function for both 1.0 and 2.x \(with optional parameters-the action will figure out which approach to use based on what is passed to it\).
- Removed the extra action that was an example of returning something other than an boolean.

* Addressed all remaining review comments

- Added test coverage for all pydantic models used by the integration
- Added test coverage for the exp backoff decorator and the calculation of next retry time.
- Updated the example rails configuration for Colang v2.x so it parses correctly on startup with `nemoguardrails chat`
- Changed policy alias topology to use a dict to prevent accidental re-use of the same alias with a different ID. Updated example configs and documentation to show this difference. Notebook updated as well.
- Consolidated code for the Clavata check action: It now uses two helpers to determine the correct policy ID and labels to use. Precedence is given to policy/label that are passed directly to the action, but if values are not passed, the config will then be checked to obtain the correct values. This should make migration from colang 1 to v2.x smoother.

* run pre-commit

* use str in favor of httpurl

* fix issue with dashes getting stripped from policy ids

* python 3.9 compat

* Use union operator rather than pipe for python 3.9

---------

Signed-off-by: Ilias Tsangaris <iliastsangaris@gmail.com>
Co-authored-by: Brett Levenson <brett@clavata.ai>
Co-authored-by: Brett Levenson <bhlevenson@gmail.com>")  | Apr 24, 2025  |  
|   |   |  [docs(benchmark): drop AIPerf comparison from Locust README (](https://github.com/NVIDIA-NeMo/Guardrails/commit/39b9c5b09e41a4ee71156effdd7ca6f02e98ead4 "docs\(benchmark\): drop AIPerf comparison from Locust README \(#2356\)

Signed-off-by: Yixin Huang <yixinh@nvidia.com>")[#2356](https://github.com/NVIDIA-NeMo/Guardrails/pull/2356)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/39b9c5b09e41a4ee71156effdd7ca6f02e98ead4 "docs\(benchmark\): drop AIPerf comparison from Locust README \(#2356\)

Signed-off-by: Yixin Huang <yixinh@nvidia.com>")  | Sep 2, 2026  |  
|  [feat(server)!: Use structured tool-calling request and response (](https://github.com/NVIDIA-NeMo/Guardrails/commit/9f793de53e432c4c9c765975f5dd54df175fcb6e "feat\(server\)!: Use structured tool-calling request and response \(#2420\)")[#2420](https://github.com/NVIDIA-NeMo/Guardrails/pull/2420)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/9f793de53e432c4c9c765975f5dd54df175fcb6e "feat\(server\)!: Use structured tool-calling request and response \(#2420\)")  | Oct 7, 2026  |  
|   |   |  [feat: add tool_safety_check LLM-judged per-tool rail (](https://github.com/NVIDIA-NeMo/Guardrails/commit/83d03ad529d2be1399f1dede24be59d4381ee42e "feat: add tool_safety_check LLM-judged per-tool rail \(#2386\)")[#2386](https://github.com/NVIDIA-NeMo/Guardrails/pull/2386)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/83d03ad529d2be1399f1dede24be59d4381ee42e "feat: add tool_safety_check LLM-judged per-tool rail \(#2386\)")  | Sep 29, 2026  |  
|  [docs: add v0.24.1 documentation and upgrade Fern (](https://github.com/NVIDIA-NeMo/Guardrails/commit/f4013f6a1e761e4832af8b59ac119e421c768a64 "docs: add v0.24.1 documentation and upgrade Fern \(#2390\)

Signed-off-by: Miyoung Choi <miyoungc@nvidia.com>")[#2390](https://github.com/NVIDIA-NeMo/Guardrails/pull/2390)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/f4013f6a1e761e4832af8b59ac119e421c768a64 "docs: add v0.24.1 documentation and upgrade Fern \(#2390\)

Signed-off-by: Miyoung Choi <miyoungc@nvidia.com>")  | Sep 16, 2026  |  
|   |   |  [feat(server): integrate OpenAI Chat buffered proxy (](https://github.com/NVIDIA-NeMo/Guardrails/commit/4512d081cb970be8a99ac2ccd1c6a39b1a81d352 "feat\(server\): integrate OpenAI Chat buffered proxy \(#2414\)

Signed-off-by: Pouyanpi <13303554+Pouyanpi@users.noreply.github.com>")[#2414](https://github.com/NVIDIA-NeMo/Guardrails/pull/2414)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/4512d081cb970be8a99ac2ccd1c6a39b1a81d352 "feat\(server\): integrate OpenAI Chat buffered proxy \(#2414\)

Signed-off-by: Pouyanpi <13303554+Pouyanpi@users.noreply.github.com>")  | Oct 9, 2026  |  
|   | Jul 2, 2026  |  
|   |   |  [feat: add per-tool rail scoping for tool calls and results (](https://github.com/NVIDIA-NeMo/Guardrails/commit/54692ca7fbb1d97855d8991e97dcaa6fd1a1ae16 "feat: add per-tool rail scoping for tool calls and results \(#2377\)

Signed-off-by: Tanushriya Singh <tanushriyas@nvidia.com>")[#2377](https://github.com/NVIDIA-NeMo/Guardrails/pull/2377)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/54692ca7fbb1d97855d8991e97dcaa6fd1a1ae16 "feat: add per-tool rail scoping for tool calls and results \(#2377\)

Signed-off-by: Tanushriya Singh <tanushriyas@nvidia.com>")  | Sep 17, 2026  |  
|   |   |   | Aug 19, 2026  |  
|   |   |  [feat(server): integrate OpenAI Chat buffered proxy (](https://github.com/NVIDIA-NeMo/Guardrails/commit/4512d081cb970be8a99ac2ccd1c6a39b1a81d352 "feat\(server\): integrate OpenAI Chat buffered proxy \(#2414\)

Signed-off-by: Pouyanpi <13303554+Pouyanpi@users.noreply.github.com>")[#2414](https://github.com/NVIDIA-NeMo/Guardrails/pull/2414)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/4512d081cb970be8a99ac2ccd1c6a39b1a81d352 "feat\(server\): integrate OpenAI Chat buffered proxy \(#2414\)

Signed-off-by: Pouyanpi <13303554+Pouyanpi@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   |  [chore: Update repo to](https://github.com/NVIDIA-NeMo/Guardrails/commit/632678d792e3a7cb98a2b9c648489fe0876885ac "chore: Update repo to https://github.com/NVIDIA-NeMo/Guardrails \(#1594\)

* Rename repo name across the Guardrails codebase

* Caught the last few old-repo references in badges and actions

* Correct capitalization in pyproject.toml homepage and issues references

* Correct local clone directory for the new repo name

* Added missing .git extension to Github repo links") <https://github.com/NVIDIA-NeMo/Guardrails> [(](https://github.com/NVIDIA-NeMo/Guardrails/commit/632678d792e3a7cb98a2b9c648489fe0876885ac "chore: Update repo to https://github.com/NVIDIA-NeMo/Guardrails \(#1594\)

* Rename repo name across the Guardrails codebase

* Caught the last few old-repo references in badges and actions

* Correct capitalization in pyproject.toml homepage and issues references

* Correct local clone directory for the new repo name

* Added missing .git extension to Github repo links")[#1594](https://github.com/NVIDIA-NeMo/Guardrails/pull/1594)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/632678d792e3a7cb98a2b9c648489fe0876885ac "chore: Update repo to https://github.com/NVIDIA-NeMo/Guardrails \(#1594\)

* Rename repo name across the Guardrails codebase

* Caught the last few old-repo references in badges and actions

* Correct capitalization in pyproject.toml homepage and issues references

* Correct local clone directory for the new repo name

* Added missing .git extension to Github repo links")  | Jan 20, 2026  |  
|   |   |  [ci: gate automated reviews on triage labels and auto-label new PRs (](https://github.com/NVIDIA-NeMo/Guardrails/commit/6cb703024e8dddf7242c951b8e885f9e6cda00ba "ci: gate automated reviews on triage labels and auto-label new PRs \(#2021\)")[#…](https://github.com/NVIDIA-NeMo/Guardrails/pull/2021)  | Jun 17, 2026  |  
|   |   |   | Jul 2, 2026  |  
|   |   |   | Aug 19, 2026  |  
|   |   |   | Jul 2, 2026  |  
|   |   |  [chore(deps): bump the pre-commit-hooks group with 2 updates (](https://github.com/NVIDIA-NeMo/Guardrails/commit/2c3553531e36f323b10eb6a99e3e01baa0ffa3fb "chore\(deps\): bump the pre-commit-hooks group with 2 updates \(#2242\)

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>")[#2242](https://github.com/NVIDIA-NeMo/Guardrails/pull/2242)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/2c3553531e36f323b10eb6a99e3e01baa0ffa3fb "chore\(deps\): bump the pre-commit-hooks group with 2 updates \(#2242\)

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>")  | Aug 21, 2026  |  
|   |   | [feat: railsignore added for config loading of LLMRails](https://github.com/NVIDIA-NeMo/Guardrails/commit/e31c391b71aeb5acdac345c97a18fa4e0411bd2b "feat: railsignore added for config loading of LLMRails")  | Oct 17, 2024  |  
|   |   |  [docs: clarify agent guidance in AGENTS.md (](https://github.com/NVIDIA-NeMo/Guardrails/commit/891c13f64e4509294dcaec4c13fe7ca657993099 "docs: clarify agent guidance in AGENTS.md \(#2218\)")[#2218](https://github.com/NVIDIA-NeMo/Guardrails/pull/2218)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/891c13f64e4509294dcaec4c13fe7ca657993099 "docs: clarify agent guidance in AGENTS.md \(#2218\)")  | Jul 30, 2026  |  
|   |   |  [docs: add AI-assisted contribution policy and coding-agent guide (](https://github.com/NVIDIA-NeMo/Guardrails/commit/90ee1ddcb189072f5c9f5f1a4800629ab0c87864 "  docs: add AI-assisted contribution policy and coding-agent guide     \(#2007\)")[#2007](https://github.com/NVIDIA-NeMo/Guardrails/pull/2007)  | Jun 12, 2026  |  
|   |   |  [chore: Update repo to](https://github.com/NVIDIA-NeMo/Guardrails/commit/632678d792e3a7cb98a2b9c648489fe0876885ac "chore: Update repo to https://github.com/NVIDIA-NeMo/Guardrails \(#1594\)

* Rename repo name across the Guardrails codebase

* Caught the last few old-repo references in badges and actions

* Correct capitalization in pyproject.toml homepage and issues references

* Correct local clone directory for the new repo name

* Added missing .git extension to Github repo links") <https://github.com/NVIDIA-NeMo/Guardrails> [(](https://github.com/NVIDIA-NeMo/Guardrails/commit/632678d792e3a7cb98a2b9c648489fe0876885ac "chore: Update repo to https://github.com/NVIDIA-NeMo/Guardrails \(#1594\)

* Rename repo name across the Guardrails codebase

* Caught the last few old-repo references in badges and actions

* Correct capitalization in pyproject.toml homepage and issues references

* Correct local clone directory for the new repo name

* Added missing .git extension to Github repo links")[#1594](https://github.com/NVIDIA-NeMo/Guardrails/pull/1594)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/632678d792e3a7cb98a2b9c648489fe0876885ac "chore: Update repo to https://github.com/NVIDIA-NeMo/Guardrails \(#1594\)

* Rename repo name across the Guardrails codebase

* Caught the last few old-repo references in badges and actions

* Correct capitalization in pyproject.toml homepage and issues references

* Correct local clone directory for the new repo name

* Added missing .git extension to Github repo links")  | Jan 20, 2026  |  
|   |   |  [chore: prepare for release v0.24.1 (](https://github.com/NVIDIA-NeMo/Guardrails/commit/5d81d6700e713018e3b74065f8b1f56d1051f290 "chore: prepare for release v0.24.1 \(#2387\)

Co-authored-by: Pouyanpi <13303554+Pouyanpi@users.noreply.github.com>")[#2387](https://github.com/NVIDIA-NeMo/Guardrails/pull/2387)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/5d81d6700e713018e3b74065f8b1f56d1051f290 "chore: prepare for release v0.24.1 \(#2387\)

Co-authored-by: Pouyanpi <13303554+Pouyanpi@users.noreply.github.com>")  | Sep 16, 2026  |  
|   |   |  [docs: add AI-assisted contribution policy and coding-agent guide (](https://github.com/NVIDIA-NeMo/Guardrails/commit/90ee1ddcb189072f5c9f5f1a4800629ab0c87864 "  docs: add AI-assisted contribution policy and coding-agent guide     \(#2007\)")[#2007](https://github.com/NVIDIA-NeMo/Guardrails/pull/2007)  | Jun 12, 2026  |  
|   |   |   | Aug 19, 2026  |  
|   |   | [feat(service): Add /v1/health and /healthz endpoints for health check…](https://github.com/NVIDIA-NeMo/Guardrails/commit/69d3c2a3017ac4e12b0d824fc9ec47209e7587dc "feat\(service\): Add /v1/health and /healthz endpoints for health checking \(#2169\)")  | Jul 14, 2026  |  
|   |   |   | Apr 25, 2023  |  
|   |   |   | Apr 25, 2023  |  
|   |   |   | Apr 25, 2023  |  
|   |   |   | Aug 19, 2026  |  
|   |   |  [chore: prepare for release v0.24.1 (](https://github.com/NVIDIA-NeMo/Guardrails/commit/5d81d6700e713018e3b74065f8b1f56d1051f290 "chore: prepare for release v0.24.1 \(#2387\)

Co-authored-by: Pouyanpi <13303554+Pouyanpi@users.noreply.github.com>")[#2387](https://github.com/NVIDIA-NeMo/Guardrails/pull/2387)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/5d81d6700e713018e3b74065f8b1f56d1051f290 "chore: prepare for release v0.24.1 \(#2387\)

Co-authored-by: Pouyanpi <13303554+Pouyanpi@users.noreply.github.com>")  | Sep 16, 2026  |  
|   |   |  [docs: add Out of Scope section to SECURITY.md (](https://github.com/NVIDIA-NeMo/Guardrails/commit/7c2f7d5694d1e36880783e012a351e0adfba6c98 "docs: add Out of Scope section to SECURITY.md \(#2192\)

Co-authored-by: Cursor <cursoragent@cursor.com>")[#2192](https://github.com/NVIDIA-NeMo/Guardrails/pull/2192)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/7c2f7d5694d1e36880783e012a351e0adfba6c98 "docs: add Out of Scope section to SECURITY.md \(#2192\)

Co-authored-by: Cursor <cursoragent@cursor.com>")  | Jul 22, 2026  |  
|   |   |  [chore: run pre-commits to update license date for 2026 (](https://github.com/NVIDIA-NeMo/Guardrails/commit/72e2e6eb7e6a3b5f4be31175f199ea1724b85dde "chore: run pre-commits to update license date for 2026 \(#1562\)")[#1562](https://github.com/NVIDIA-NeMo/Guardrails/pull/1562)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/72e2e6eb7e6a3b5f4be31175f199ea1724b85dde "chore: run pre-commits to update license date for 2026 \(#1562\)")  | Jan 5, 2026  |  
|   |   |  [chore: Update repo to](https://github.com/NVIDIA-NeMo/Guardrails/commit/632678d792e3a7cb98a2b9c648489fe0876885ac "chore: Update repo to https://github.com/NVIDIA-NeMo/Guardrails \(#1594\)

* Rename repo name across the Guardrails codebase

* Caught the last few old-repo references in badges and actions

* Correct capitalization in pyproject.toml homepage and issues references

* Correct local clone directory for the new repo name

* Added missing .git extension to Github repo links") <https://github.com/NVIDIA-NeMo/Guardrails> [(](https://github.com/NVIDIA-NeMo/Guardrails/commit/632678d792e3a7cb98a2b9c648489fe0876885ac "chore: Update repo to https://github.com/NVIDIA-NeMo/Guardrails \(#1594\)

* Rename repo name across the Guardrails codebase

* Caught the last few old-repo references in badges and actions

* Correct capitalization in pyproject.toml homepage and issues references

* Correct local clone directory for the new repo name

* Added missing .git extension to Github repo links")[#1594](https://github.com/NVIDIA-NeMo/Guardrails/pull/1594)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/632678d792e3a7cb98a2b9c648489fe0876885ac "chore: Update repo to https://github.com/NVIDIA-NeMo/Guardrails \(#1594\)

* Rename repo name across the Guardrails codebase

* Caught the last few old-repo references in badges and actions

* Correct capitalization in pyproject.toml homepage and issues references

* Correct local clone directory for the new repo name

* Added missing .git extension to Github repo links")  | Jan 20, 2026  |  
|   |   |  [ci: gate automated reviews on triage labels and auto-label new PRs (](https://github.com/NVIDIA-NeMo/Guardrails/commit/6cb703024e8dddf7242c951b8e885f9e6cda00ba "ci: gate automated reviews on triage labels and auto-label new PRs \(#2021\)")[#…](https://github.com/NVIDIA-NeMo/Guardrails/pull/2021)  | Jun 17, 2026  |  
|   |   |   | Jun 9, 2026  |  
|   |   |   | Aug 19, 2026  |  
|   |   |   | Sep 25, 2026  |  
|   |   |  [test(recorded): add replay harness (1/5) (](https://github.com/NVIDIA-NeMo/Guardrails/commit/bed94c27857661f29e7b019978edd0026a2c1f31 "test\(recorded\): add replay harness \(1/5\) \(#1974\)

Introduced recorded testing framework enabling deterministic integration tests using cassette-based API interactions without live network access.")[#1974](https://github.com/NVIDIA-NeMo/Guardrails/pull/1974)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/bed94c27857661f29e7b019978edd0026a2c1f31 "test\(recorded\): add replay harness \(1/5\) \(#1974\)

Introduced recorded testing framework enabling deterministic integration tests using cassette-based API interactions without live network access.")  | Jun 26, 2026  |  
|   |   |  [chore: use ruff linter and formatter (](https://github.com/NVIDIA-NeMo/Guardrails/commit/70ed8b6ec47328b2dbc33350d2cc429b6517c141 "chore: use ruff linter and formatter \(#1351\)

* chore\(pre-commit\): switch to ruff for linting and formatting

Replace isort and black with ruff and ruff-format in pre-commit config.
Update hooks to use ruff for both linting and formatting, specifying
versions v0.9.6 and v0.11.2. Remove isort and black hooks, and keep
other hooks unchanged.

* feat\(imports\): add utilities for optional dependencies

Introduced a new `imports.py` module providing utilities to handle optional
dependencies. This includes functions for importing optional modules with
customizable error handling, checking for their presence, and version
validation. Also added a mapping for commonly used optional dependencies
and a helper to retrieve them with predefined settings.")[#1351](https://github.com/NVIDIA-NeMo/Guardrails/pull/1351)[)](https://github.com/NVIDIA-NeMo/Guardrails/commit/70ed8b6ec47328b2dbc33350d2cc429b6517c141 "chore: use ruff linter and formatter \(#1351\)

* chore\(pre-commit\): switch to ruff for linting and formatting

Replace isort and black with ruff and ruff-format in pre-commit config.
Update hooks to use ruff for both linting and formatting, specifying
versions v0.9.6 and v0.11.2. Remove isort and black hooks, and keep
other hooks unchanged.

* feat\(imports\): add utilities for optional dependencies

Introduced a new `imports.py` module providing utilities to handle optional
dependencies. This includes functions for importing optional modules with
customizable error handling, checking for their presence, and version
validation. Also added a mapping for commonly used optional dependencies
and a helper to retrieve them with predefined settings.")  | Nov 26, 2025  |  
|   |   | [Add script to generate README.md files from notebooks.](https://github.com/NVIDIA-NeMo/Guardrails/commit/1331619fb099907ae4fa938b91b69539dc11ce44 "Add script to generate README.md files from notebooks.")  | Feb 22, 2024  |  
|   |   |   | Sep 25, 2026  |  
| View all files |  
## Repository files navigation
# NVIDIA NeMo Guardrails Library
> **LATEST RELEASE / DEVELOPMENT VERSION** : The [develop](https://github.com/NVIDIA-NeMo/Guardrails/tree/develop) branch tracks the latest top of tree development. The latest released version is [0.24.1](https://github.com/NVIDIA-NeMo/Guardrails/tree/v0.24.1).
✨✨✨
📌 **The official NeMo Guardrails library documentation is available at[docs.nvidia.com/nemo/guardrails](https://docs.nvidia.com/nemo/guardrails).**
✨✨✨
NVIDIA NeMo Guardrails library is an open-source toolkit for easily adding _programmable guardrails_ to LLM-based conversational applications. Guardrails (or "rails" for short) are specific ways of controlling the output of a large language model, such as not talking about politics, responding in a particular way to specific user requests, following a predefined dialog path, using a particular language style, extracting structured data, and more.
[This paper](https://arxiv.org/abs/2310.10501) introduces the NeMo Guardrails library and contains a technical overview of the system and the current evaluation.
## Requirements
Python 3.10, 3.11, 3.12 or 3.13.
## Installation
To install using pip:

```
 pip install nemoguardrails
```

For more detailed instructions, see the [Installation Guide](https://docs.nvidia.com/nemo/guardrails/get-started/installation-guide).
## Overview
The NeMo Guardrails library enables developers building LLM-based applications to add **programmable guardrails** between the application code and the LLM.
Key benefits of adding _programmable guardrails_ include:
  * **Building Trustworthy, Safe, and Secure LLM-based Applications:** you can define rails to guide and safeguard conversations; you can choose to define the behavior of your LLM-based application on specific topics and prevent it from engaging in discussions on unwanted topics.
  * **Connecting models, chains, and other services securely:** you can connect an LLM to other services (a.k.a. tools) seamlessly and securely.
  * **Controllable dialog** : you can steer the LLM to follow pre-defined conversational paths, allowing you to design the interaction following conversation design best practices and enforce standard operating procedures (e.g., authentication, support).


### Protecting against LLM Vulnerabilities
The NeMo Guardrails library provides several mechanisms for protecting an LLM-powered chat application against common LLM vulnerabilities, such as jailbreaks and prompt injections. Below is a sample overview of the protection offered by different guardrails configuration for the example [ABC Bot](https://github.com/NVIDIA-NeMo/Guardrails/blob/develop/examples/bots/abc) included in this repository. For more details, please refer to the [LLM Vulnerability Scanning](https://docs.nvidia.com/nemo/guardrails/evaluation/llm-vulnerability-scanning.html) page.
### Use Cases
You can use programmable guardrails in different types of use cases:
  1. **Question Answering** over a set of documents (a.k.a. Retrieval Augmented Generation): Enforce fact-checking and output moderation.
  2. **Domain-specific Assistants** (a.k.a. chatbots): Ensure the assistant stays on topic and follows the designed conversational flows.
  3. **LLM Endpoints** : Add guardrails to your custom LLM for safer customer interaction.
  4. **LangChain Chains** (optional): If you use LangChain for any use case, you can add a guardrails layer around your chains. To enable this integration, set the `NEMOGUARDRAILS_LLM_FRAMEWORK=langchain` environment variable or call `set_default_framework("langchain")`.


### Usage
To add programmable guardrails to your application you can use the Python API or a guardrails server (see the [Server Guide](https://docs.nvidia.com/nemo/guardrails/get-started/integrate-into-application) for more details). Using the Python API is similar to using the LLM directly. Calling the guardrails layer instead of the LLM requires only minimal changes to the code base, and it involves two simple steps:
  1. Loading a guardrails configuration and creating an `LLMRails` instance.
  2. Making the calls to the LLM using the `generate`/`generate_async` methods.



```
from nemoguardrails import LLMRails, RailsConfig

# Load a guardrails configuration from the specified path.
config = RailsConfig.from_path("PATH/TO/CONFIG")
rails = LLMRails(config)

completion = rails.generate(
    messages=[{"role": "user", "content": "Hello world!"}]
)
```

Sample output:

```
{"role": "assistant", "content": "Hi! How can I help you?"}
```

The input and output format for the `generate` method is similar to the [Chat Completions API](https://platform.openai.com/docs/guides/gpt/chat-completions-api) from OpenAI.
#### Async API
The NeMo Guardrails library is an async-first toolkit as the core mechanics are implemented using the Python async model. The public methods have both a sync and an async version. For example: `LLMRails.generate` and `LLMRails.generate_async`.
### Supported LLMs
You can use NeMo Guardrails with multiple LLMs like OpenAI GPT-3.5, GPT-4, LLaMa-2, Falcon, Vicuna, or Mosaic. For more details, check out the [Supported LLM Models](https://docs.nvidia.com/nemo/guardrails/about-nemo-guardrails-library/supported-llms) section in the Configuration Guide.
### Types of Guardrails
The NeMo Guardrails library supports five main types of guardrails:
  1. **Input rails** : applied to the input from the user; an input rail can reject the input, stopping any additional processing, or alter the input (e.g., to mask potentially sensitive data, to rephrase).
  2. **Dialog rails** : influence how the LLM is prompted; dialog rails operate on canonical form messages for details see [Colang Guide](https://docs.nvidia.com/nemo/guardrails/configure-guardrails/colang)) and determine if an action should be executed, if the LLM should be invoked to generate the next step or a response, if a predefined response should be used instead, etc.
  3. **Retrieval rails** : applied to the retrieved chunks in the case of a RAG (Retrieval Augmented Generation) scenario; a retrieval rail can reject a chunk, preventing it from being used to prompt the LLM, or alter the relevant chunks (e.g., to mask potentially sensitive data).
  4. **Execution rails** : applied to input/output of the custom actions (a.k.a. tools), that need to be called by the LLM.
  5. **Output rails** : applied to the output generated by the LLM; an output rail can reject the output, preventing it from being returned to the user, or alter it (e.g., removing sensitive data).


### Guardrails Configuration
A guardrails configuration defines the **LLM(s)** to be used and **one or more guardrails**. A guardrails configuration can include any number of input/dialog/output/retrieval/execution rails. A configuration without any configured rails will essentially forward the requests to the LLM.
The standard structure for a guardrails configuration folder looks like this:

```
.
├── config
│   ├── actions.py
│   ├── config.py
│   ├── config.yml
│   ├── rails.co
│   ├── ...

```

The `config.yml` contains all the general configuration options, such as LLM models, active rails, and custom configuration data". The `config.py` file contains any custom initialization code and the `actions.py` contains any custom python actions. For a complete overview, see the [Configuration Guide](https://docs.nvidia.com/nemo/guardrails/configure-guardrails/configure-rails).
Below is an example `config.yml`:

```
# config.yml
models:
  - type: main
    engine: openai
    model: gpt-3.5-turbo-instruct

rails:
  # Input rails are invoked when new input from the user is received.
  input:
    flows:
      - check jailbreak
      - mask sensitive data on input

  # Output rails are triggered after a bot message has been generated.
  output:
    flows:
      - self check facts
      - self check hallucination
      - activefence moderation on input

  config:
    # Configure the types of entities that should be masked on user input.
    sensitive_data_detection:
      input:
        entities:
          - PERSON
          - EMAIL_ADDRESS
```

The `.co` files included in a guardrails configuration contain the Colang definitions (see the next section for a quick overview of what Colang is) that define various types of rails. Below is an example `greeting.co` file which defines the dialog rails for greeting the user.

```
define user express greeting
  "Hello!"
  "Good afternoon!"

define flow
  user express greeting
  bot express greeting
  bot offer to help

define bot express greeting
  "Hello there!"

define bot offer to help
  "How can I help you today?"

```

Below is an additional example of Colang definitions for a dialog rail against insults:

```
define user express insult
  "You are stupid"

define flow
  user express insult
  bot express calmly willingness to help

```

### Colang
To configure and implement various types of guardrails, this toolkit introduces **Colang** , a modeling language specifically created for designing flexible, yet controllable, dialogue flows. Colang has a python-like syntax and is designed to be simple and intuitive, especially for developers.

```
Two versions of Colang, 1.0 and 2.0, are supported and Colang 1.0 is the default.

```

For a brief introduction to the Colang 1.0 syntax, see the [Colang 1.0 Language Syntax Guide](https://docs.nvidia.com/nemo/guardrails/configure-guardrails/colang).
To get started with Colang 2.0, see the [Colang 2.0 Documentation](https://docs.nvidia.com/nemo/guardrails/colang-2/overview.html).
### Guardrails Library
NeMo Guardrails comes with a set of [built-in guardrails](https://docs.nvidia.com/nemo/guardrails/user-guides/guardrails-library.html).

```
The built-in guardrails may or may not be suitable for a given production use case. As always, developers should work with their internal application team to ensure guardrails meets requirements for the relevant industry and use case and address unforeseen product misuse.

```

The library includes guardrails for LLM self-checking (input/output moderation, fact-checking, hallucination detection), NVIDIA safety models (content safety, topic safety), jailbreak and injection detection, and integrations with community models and third-party APIs. For the complete list, see the [Guardrails Library documentation](https://docs.nvidia.com/nemo/guardrails/user-guides/guardrails-library.html).
The NeMo Guardrails library also comes with a built-in CLI.

```
$ nemoguardrails --help

Usage: nemoguardrails [OPTIONS] COMMAND [ARGS]...

actions-server    Start a NeMo Guardrails actions server.
chat              Start an interactive chat session.
evaluate          Run an evaluation task.
server            Start a NeMo Guardrails server.
```

### Guardrails Server
You can use the NeMo Guardrails library CLI to start a guardrails server. The server can load one or more configurations from the specified folder and expose and HTTP API for using them.

```
nemoguardrails server [--config PATH/TO/CONFIGS] [--port PORT]

```

For example, to get a chat completion for a `sample` config, you can use the `/v1/chat/completions` endpoint:

```
POST /v1/chat/completions

```


```
{
    "config_id": "sample",
    "messages": [{
      "role":"user",
      "content":"Hello! What can you do for me?"
    }]
}
```

Sample output:

```
{"role": "assistant", "content": "Hi! How can I help you?"}
```

#### Docker
To start a guardrails server, you can also use a Docker container. The NeMo Guardrails library provides a [Dockerfile](https://github.com/NVIDIA-NeMo/Guardrails/blob/develop/Dockerfile) that you can use to build a `nemoguardrails` image. For further information, see the [using Docker](https://docs.nvidia.com/nemo/guardrails/user-guides/advanced/using-docker.html) section.
## Integration with LangChain (Optional)
LangChain integration is opt-in. To enable it, set the `NEMOGUARDRAILS_LLM_FRAMEWORK=langchain` environment variable or call `set_default_framework("langchain")`. Then install the LangChain packages your configuration requires. After you enable the integration, you can wrap a guardrails configuration around a LangChain chain (or any `Runnable`), and you can call a LangChain chain from within a guardrails configuration. For more information, refer to the [LangChain Integration Documentation](https://docs.nvidia.com/nemo/guardrails/user-guides/langchain/langchain-integration.html).
## Evaluation
Evaluating the safety of a LLM-based conversational application is a complex task and still an open research question. To support proper evaluation, the NeMo Guardrails library provides the following:
  1. An [evaluation tool](https://github.com/NVIDIA-NeMo/Guardrails/blob/develop/nemoguardrails/evaluate/README.md), i.e. `nemoguardrails evaluate`, with support for topical rails, fact-checking, moderation (jailbreak and output moderation) and hallucination.
  2. Sample LLM Vulnerability Scanning Reports, e.g, [ABC Bot - LLM Vulnerability Scan Results](https://docs.nvidia.com/nemo/guardrails/evaluation/llm-vulnerability-scanning.html)


## How is this different?
There are many ways guardrails can be added to an LLM-based conversational application. For example: explicit moderation endpoints (e.g., OpenAI, ActiveFence, PolicyAI), critique chains (e.g. constitutional chain), parsing the output (e.g. guardrails.ai), individual guardrails (e.g., LLM-Guard), hallucination detection for RAG applications (e.g., Got It AI, Patronus Lynx).
The NeMo Guardrails library aims to provide a flexible toolkit that can integrate all these complementary approaches into a cohesive LLM guardrails layer. For example, the toolkit provides out-of-the-box integration with ActiveFence, PolicyAI, AlignScore and LangChain chains.
To the best of our knowledge, the NeMo Guardrails library is the only guardrails toolkit that also offers a solution for modeling the dialog between the user and the LLM. This enables on one hand the ability to guide the dialog in a precise way. On the other hand it enables fine-grained control for when certain guardrails should be used, e.g., use fact-checking only for certain types of questions.
## Learn More
  * [Getting Started Guide](https://docs.nvidia.com/nemo/guardrails/getting-started)


## Telemetry and Privacy
The NVIDIA NeMo Guardrails library collects anonymous telemetry to help NVIDIA understand which deployment patterns and safety features are most used. The library emits one usage event when you instantiate `LLMRails`, `IORails`, or `Guardrails`, then emits periodic heartbeats from a single daemon thread per process. This telemetry is separate from per-request [tracing](https://docs.nvidia.com/nemo/guardrails/latest/observability/tracing/index.html). You configure tracing in your guardrails config and send it to your own observability backend. Telemetry is a minimal anonymous ping to NVIDIA.
### Community usage snapshot
Aggregate anonymous usage across exact `0.22.0` and `0.23.0` release builds, May 22–August 18, 2026:
_Last updated on August 18, 2026_
The telemetry includes:
  * Installed library version, Python version, operating system, and platform string
  * Colang configuration language version (1.0 or 2.x)
  * Names of configured LLM engine providers, such as `openai`, `nim`, or `nvidia_ai_endpoints`, never model names or credentials
  * Counts of configured rail flows for input, output, retrieval, tool input, and tool output rails, plus which rail categories are active
  * Names of built-in library features that are active, such as `jailbreak_detection`, `content_safety`, or `topic_safety`
  * Count of user-defined Colang flows (count only, never flow names or contents)
  * Whether tracing, streaming, or a knowledge base is configured
  * How you deployed guardrails (`library`, `api`, or `cli` server)
  * Which runtime rails engine is in use (`LLMRails` or `IORails`)
  * A random per-process UUID for correlating events from the same instance. The library generates it in memory and does not store it for reuse across restarts, but includes it in audit records and transmitted telemetry events


No user content is collected in the event payload. The payload does not include model names, API keys, endpoints, prompts, completions, token counts, per-request metrics, file paths, usernames, or IP addresses. NVIDIA uses the data in aggregate to prioritize engineering work and will share adoption trends with the community.
The library also attempts to write each event payload to a local audit file at `~/.config/nemoguardrails/usage_stats.json`. The audit file stores the event JSONL, not the full NVIDIA telemetry envelope. Audit writes are best effort, and telemetry transmission still proceeds if local audit writing fails.
Set any one of the following options to disable telemetry:

```
export NEMO_GUARDRAILS_NO_USAGE_STATS=1

export DO_NOT_TRACK=1

mkdir -p ~/.config/nemoguardrails  touch ~/.config/nemoguardrails/do_not_track
```

Set the opt-out before the NVIDIA NeMo Guardrails library starts. Changing environment variables or creating `do_not_track` after telemetry has started does not stop an already-running heartbeat thread.
Refer to [docs/telemetry.md](https://docs.nvidia.com/nemo/guardrails/latest/telemetry.html) for the full schema and field-by-field descriptions.
You may opt out of telemetry collection at any time. Opting out applies only to data collection by the NVIDIA NeMo Guardrails library itself.
Third-party endpoints have separate terms and privacy practices. The NVIDIA NeMo Guardrails library can use inference endpoints such as NVIDIA Build (`build.nvidia.com`). If you use NVIDIA Build or another third-party endpoint, that endpoint's terms of service and privacy practices apply independently of the library. Any telemetry opt-out in the NVIDIA NeMo Guardrails library does not extend to the endpoint you choose. NVIDIA Build is intended for evaluation and testing only and must not be used in production environments. Do not submit confidential information or personal data when using NVIDIA Build.
## Inviting the community to contribute
The example rails residing in the repository are excellent starting points. We enthusiastically invite the community to contribute towards making the power of trustworthy, safe, and secure LLMs accessible to everyone. For guidance on setting up a development environment and how to contribute to the NeMo Guardrails library, see the [contributing guidelines](https://github.com/NVIDIA-NeMo/Guardrails/blob/develop/CONTRIBUTING.md).
## License
The NeMo Guardrails library is licensed under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0).
## How to cite
If you use the NeMo Guardrails library, cite the [EMNLP 2023 paper](https://aclanthology.org/2023.emnlp-demo.40) that introduces it.

```
@inproceedings{rebedea-etal-2023-nemo,
    title = "{N}e{M}o Guardrails: A Toolkit for Controllable and Safe {LLM} Applications with Programmable Rails",
    author = "Rebedea, Traian  and
      Dinu, Razvan  and
      Sreedhar, Makesh Narsimhan  and
      Parisien, Christopher  and
      Cohen, Jonathan",
    editor = "Feng, Yansong  and
      Lefever, Els",
    booktitle = "Proceedings of the 2023 Conference on Empirical Methods in Natural Language Processing: System Demonstrations",
    month = dec,
    year = "2023",
    address = "Singapore",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2023.emnlp-demo.40",
    doi = "10.18653/v1/2023.emnlp-demo.40",
    pages = "431--445",
}
```

## About
NeMo Guardrails is an open-source toolkit for easily adding programmable guardrails to LLM-based conversational systems.
[docs.nvidia.com/nemo/guardrails/latest/index.html](https://docs.nvidia.com/nemo/guardrails/latest/index.html)
### Topics
[agents](https://github.com/topics/agents)[generative-ai](https://github.com/topics/generative-ai)[guardrails](https://github.com/topics/guardrails)[llm-safety](https://github.com/topics/llm-safety)[llm-security](https://github.com/topics/llm-security)[llms](https://github.com/topics/llms)[nvidia](https://github.com/topics/nvidia)[python](https://github.com/topics/python)[safety](https://github.com/topics/safety)
### Resources
### Contributing
### Security policy
### Stars
**7.3k** stars
### Watchers
**45** watching
### Forks
[ forks](https://github.com/NVIDIA-NeMo/Guardrails/forks)
## 
[ v0.24.1Latest Sep 16, 2026 ](https://github.com/NVIDIA-NeMo/Guardrails/releases/tag/v0.24.1)
## 
[+ 450](https://github.com/NVIDIA-NeMo/Guardrails/network/dependents)
## 
## Languages
  * [Python99.3%](https://github.com/NVIDIA-NeMo/Guardrails/search?l=python)
  * [JavaScript0.4%](https://github.com/NVIDIA-NeMo/Guardrails/search?l=javascript)
  * [Shell0.1%](https://github.com/NVIDIA-NeMo/Guardrails/search?l=shell)
  * [Makefile0.1%](https://github.com/NVIDIA-NeMo/Guardrails/search?l=makefile)
  * [Dockerfile0.1%](https://github.com/NVIDIA-NeMo/Guardrails/search?l=dockerfile)
  * [YARA0%](https://github.com/NVIDIA-NeMo/Guardrails/search?l=yara)


You can’t perform that action at this time. 
