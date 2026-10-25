---
title: 'GitHub - BerriAI/litellm: The fastest, litest AI Gateway. Rust core with Python
  SDK. Call 100+ LLM APIs in OpenAI (or native) format with cost tracking, guardrails,
  load balancing, and logging [Bedrock, Azure, OpenAI, Anthropic, OpenAI, VertexAI,
  vLLM, Nvidia NIM] · GitHub'
id: github-berriailitellm-the-fastest-litest-ai-gateway-rust-core-with-python-sdk-ca
tags:
- internal-llm-inference-stacks-7f0c90
- litellm
- ai-gateway
- virtual-keys
- self-hosted
created: '2026-10-09T22:53:38.972089Z'
updated: '2026-10-10T14:52:16.994283Z'
source: https://github.com/BerriAI/litellm
source_domain: github.com
fetched_at: '2026-10-09T22:53:38.941853Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'LiteLLM GitHub repo page (50k+ words captured, dominated by repeated commit-changelog
  scroll; the README proper sits ~lines 7885-8437 of the note body). Describes LiteLLM
  as an open-source, self-hosted AI Gateway now tagged ''Rust core with Python SDK''
  (a 2026 architecture shift from pure-Python), claiming 8ms P95 latency at 1k RPS
  per its own /docs/benchmarks page. The OSS/Enterprise split: OSS gateway ships virtual
  keys, multi-tenant cost tracking/spend management per project-user, per-project
  guardrails/logging/caching, admin dashboard, Router with retry/fallback, TPM/RPM
  rate limiting (with a documented P1 bypass bug fixed via token-array counting),
  and Prometheus per-project per-model rate-limit gauges. LiteLLM Enterprise (commercial
  license) adds SSO/SAML, feature prioritization, custom integrations, dedicated support/SLAs
  -- notably NOT core multi-tenancy, which the separate multi_tenant_architecture
  doc confirms lives in OSS via Teams/Virtual Keys (Organizations/org-admin hierarchy
  is the enterprise add-on). Self-hosting is supported via published Terraform modules
  (AWS ECS Fargate+Aurora+ElastiCache+ALB; GCP Cloud Run+Cloud SQL+Memorystore+HTTPS
  LB) and a Helm chart, with cosign-signed Docker images on GHCR, a componentized
  gateway/backend/UI split, and managed Postgres+Redis -- consistent with full self-hosted/air-gapped
  operation, though no page in this batch explicitly uses the word ''air-gapped''.
  Netflix is listed as a named OSS adopter. A May 2026 changelog entry shows LiteLLM
  swapping in the Granian ASGI server for 10-20 RPS throughput improvement under load,
  evidence the stock Python/uvicorn proxy had a known throughput ceiling before the
  Rust-core shift.'
utility_score: 14.0
---

[Skip to content](https://github.com/BerriAI/litellm#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/BerriAI/litellm) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/BerriAI/litellm) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/BerriAI/litellm) to refresh your session. Dismiss alert
[**3386** Branches](https://github.com/BerriAI/litellm/branches)[**1580** Tags](https://github.com/BerriAI/litellm/tags)
Go to file
Open more actions menu
## Latest commit
[devin-ai-integration[bot]](https://github.com/BerriAI/litellm/commits?author=devin-ai-integration%5Bbot%5D)
and
kerry
[refactor(cost-map): remove openrouter rows, part 5 of 5 (](https://github.com/BerriAI/litellm/commit/3ea80696246b79535386ee53c710e103a6b08dc0)[#45669](https://github.com/BerriAI/litellm/pull/45669)[)](https://github.com/BerriAI/litellm/commit/3ea80696246b79535386ee53c710e103a6b08dc0)
Open commit detailspending
Oct 9, 2026
[3ea8069](https://github.com/BerriAI/litellm/commit/3ea80696246b79535386ee53c710e103a6b08dc0) · Oct 9, 2026
## History
[54,074 Commits](https://github.com/BerriAI/litellm/commits/main/)
Open commit details
54,074 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   | [feat(rust): add litellm-db and litellm-db-testing workspace scaffoldi…](https://github.com/BerriAI/litellm/commit/9b08112ed278e9e358d40177034a5ca70febc8e1 "feat\(rust\): add litellm-db and litellm-db-testing workspace scaffolding \(#43504\)

* feat\(rust\): add litellm-db and litellm-db-testing workspace scaffolding

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix\(db-testing\): apply the real Prisma migrations in a test and drop the sort mutation

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

---------

Co-authored-by: Yujong Lee <yujong@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Sep 28, 2026  |  
|   |   |  [ci: run every build_and_test job in every CircleCI pipeline (](https://github.com/BerriAI/litellm/commit/dafa9cbd4c4839deeb230bb89f0432c954e93a36 "ci: run every build_and_test job in every CircleCI pipeline \(#43152\)

* ci: run the main branch CircleCI job set on rc/* branches

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* ci: let litellm_rc_* branches run the main-only CircleCI jobs

The 26 build_and_test jobs filtered to main now share one anchor that also admits branches named litellm_rc_*, so a run-ci pipeline on such a branch at an rc SHA runs the full suite. Every other PR keeps the integration-only subset, and the migration schedule stays on main

* ci: run every build_and_test job in every CircleCI pipeline

Drop the main-only branch filter from the 26 build_and_test jobs, so a run-ci PR pipeline into any base, an rc branch pipeline and main's scheduled pipeline all run the full suite. The migration cron stays on main

---------

Co-authored-by: yuneng <yuneng@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")[#43152](https://github.com/BerriAI/litellm/pull/43152)[)](https://github.com/BerriAI/litellm/commit/dafa9cbd4c4839deeb230bb89f0432c954e93a36 "ci: run every build_and_test job in every CircleCI pipeline \(#43152\)

* ci: run the main branch CircleCI job set on rc/* branches

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* ci: let litellm_rc_* branches run the main-only CircleCI jobs

The 26 build_and_test jobs filtered to main now share one anchor that also admits branches named litellm_rc_*, so a run-ci pipeline on such a branch at an rc SHA runs the full suite. Every other PR keeps the integration-only subset, and the migration schedule stays on main

* ci: run every build_and_test job in every CircleCI pipeline

Drop the main-only branch filter from the 26 build_and_test jobs, so a run-ci PR pipeline into any base, an rc branch pipeline and main's scheduled pipeline all run the full suite. The migration cron stays on main

---------

Co-authored-by: yuneng <yuneng@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   |  [build: migrate packaging, CI, and Docker from Poetry to uv (](https://github.com/BerriAI/litellm/commit/a6c30b30bfd5508614f3024c866bb22f5485060f "build: migrate packaging, CI, and Docker from Poetry to uv \(#25007\)

* build: migrate packaging metadata to uv

* ci: move automation and local tooling to uv

* docker: migrate image builds and runtime setup to uv

* docs: update install and deployment guidance for uv

* chore: align auxiliary scripts and tests with uv

* test: harden test_litellm isolation

* fix: keep release and health check images self-contained

* build: pin uv tooling and health check deps

* test: isolate bedrock image request formatting from suite state

* test: cover sandbox executor requirements flow

* ci: fix circleci no-op command steps

* ci: fix circleci publish workflow parsing

* fix: stabilize remaining uv migration CI checks

* ci: increase matrix test timeout headroom

* fix: restore published docker and license coverage

* fix: restore proxy runtime build parity

* fix: restore proxy extras parity and venv migrations

* ci: persist uv path across circleci steps

* fix: keep psycopg binary in default test env

* docker: preserve prisma cache across stages

* test: run local proxy checks through uv python

* build: restore runtime deps moved into ci

* build: refresh uv lock after upstream merge

* fix: restore module import in test_check_migration after merge

The conflict resolution imported only the function but the test body
references check_migration as a module throughout.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix: revert dependency promotions, remove nodejs-wheel-binaries, fix Docker layer caching

- Move google-generativeai, Pillow, tenacity back to ci group \(they are
  lazily imported and bloat the base SDK install needlessly\)
- Remove nodejs-wheel-binaries from extra_proxy and proxy-dev \(redundant
  in Docker where system Node.js is already installed via apk\)
- Remove all nodejs-wheel node replacement and venv npm patching blocks
  from Dockerfiles since the wheel is no longer installed
- Add --no-default-groups to CodSpeed benchmark workflow so the benchmark
  environment matches the old minimal pip install footprint
- Apply standard uv two-phase Docker pattern: copy metadata first, install
  deps \(cached layer\), then copy source and install project
- Replace CircleCI enterprise no-op with proper uv sync command

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* chore: regenerate uv.lock after removing nodejs-wheel-binaries

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(ci\): use cache/restore instead of cache to prevent cache poisoning

The old workflow used actions/cache/restore \(read-only\). The uv migration
changed it to actions/cache \(read-write\), which zizmor flags as a cache
poisoning risk. Restore the safer read-only variant.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(ci\): disable setup-uv built-in cache to silence cache-poisoning alert

The setup-uv action enables caching by default, which zizmor flags as a
cache poisoning risk. Disable it since we already use a read-only
cache/restore step.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(ci\): disable setup-uv cache in publish workflow

Silences zizmor cache-poisoning alert. Publishing workflow runs
infrequently on protected branches so caching adds no real benefit.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(test\): remove duplicate verbose_logger mock in test_check_migration

The logger was patched twice — first via mocker.patch\(\) then via
mocker.patch.object\(autospec=True\). The second call fails because
autospec cannot inspect an already-mocked attribute. Remove the
redundant first patch.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(ci\): free disk space before Docker build in test-server-root-path

The Dockerfile.non_root build ran out of disk on the CI runner. Remove
Android SDK, .NET, Boost, and GHC toolchains \(~12GB\) to free space.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

---------

Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>")[#25007](https://github.com/BerriAI/litellm/pull/25007)[)](https://github.com/BerriAI/litellm/commit/a6c30b30bfd5508614f3024c866bb22f5485060f "build: migrate packaging, CI, and Docker from Poetry to uv \(#25007\)

* build: migrate packaging metadata to uv

* ci: move automation and local tooling to uv

* docker: migrate image builds and runtime setup to uv

* docs: update install and deployment guidance for uv

* chore: align auxiliary scripts and tests with uv

* test: harden test_litellm isolation

* fix: keep release and health check images self-contained

* build: pin uv tooling and health check deps

* test: isolate bedrock image request formatting from suite state

* test: cover sandbox executor requirements flow

* ci: fix circleci no-op command steps

* ci: fix circleci publish workflow parsing

* fix: stabilize remaining uv migration CI checks

* ci: increase matrix test timeout headroom

* fix: restore published docker and license coverage

* fix: restore proxy runtime build parity

* fix: restore proxy extras parity and venv migrations

* ci: persist uv path across circleci steps

* fix: keep psycopg binary in default test env

* docker: preserve prisma cache across stages

* test: run local proxy checks through uv python

* build: restore runtime deps moved into ci

* build: refresh uv lock after upstream merge

* fix: restore module import in test_check_migration after merge

The conflict resolution imported only the function but the test body
references check_migration as a module throughout.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix: revert dependency promotions, remove nodejs-wheel-binaries, fix Docker layer caching

- Move google-generativeai, Pillow, tenacity back to ci group \(they are
  lazily imported and bloat the base SDK install needlessly\)
- Remove nodejs-wheel-binaries from extra_proxy and proxy-dev \(redundant
  in Docker where system Node.js is already installed via apk\)
- Remove all nodejs-wheel node replacement and venv npm patching blocks
  from Dockerfiles since the wheel is no longer installed
- Add --no-default-groups to CodSpeed benchmark workflow so the benchmark
  environment matches the old minimal pip install footprint
- Apply standard uv two-phase Docker pattern: copy metadata first, install
  deps \(cached layer\), then copy source and install project
- Replace CircleCI enterprise no-op with proper uv sync command

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* chore: regenerate uv.lock after removing nodejs-wheel-binaries

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(ci\): use cache/restore instead of cache to prevent cache poisoning

The old workflow used actions/cache/restore \(read-only\). The uv migration
changed it to actions/cache \(read-write\), which zizmor flags as a cache
poisoning risk. Restore the safer read-only variant.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(ci\): disable setup-uv built-in cache to silence cache-poisoning alert

The setup-uv action enables caching by default, which zizmor flags as a
cache poisoning risk. Disable it since we already use a read-only
cache/restore step.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(ci\): disable setup-uv cache in publish workflow

Silences zizmor cache-poisoning alert. Publishing workflow runs
infrequently on protected branches so caching adds no real benefit.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(test\): remove duplicate verbose_logger mock in test_check_migration

The logger was patched twice — first via mocker.patch\(\) then via
mocker.patch.object\(autospec=True\). The second call fails because
autospec cannot inspect an already-mocked attribute. Remove the
redundant first patch.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(ci\): free disk space before Docker build in test-server-root-path

The Dockerfile.non_root build ran out of disk on the CI runner. Remove
Android SDK, .NET, Boost, and GHC toolchains \(~12GB\) to free space.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

---------

Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>")  | Apr 9, 2026  |  
|   |   | [security(proxy): keep team callback credentials out of the stored req…](https://github.com/BerriAI/litellm/commit/703eb4fa6871feac8fc4de6260e9e0faa21a343e "security\(proxy\): keep team callback credentials out of the stored request body \(#43217\)

* security\(proxy\): keep team callback credentials out of the stored request body

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* ci: allow the security conventional commit type

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

---------

Co-authored-by: oliver <oliver@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Sep 28, 2026  |  
|   |   | [feat(microsoft_365_copilot): add Microsoft 365 Copilot chat provider …](https://github.com/BerriAI/litellm/commit/fbf4af301cfdc6bf0d861eb243682e3e9f1164c4 "feat\(microsoft_365_copilot\): add Microsoft 365 Copilot chat provider with OAuth token exchange \(#45158\)

* feat\(microsoft_365_copilot\): add Microsoft 365 Copilot chat provider with OAuth token exchange

* fix\(ui\): create a credential for credential-only auth types in Add Model

* fix\(microsoft_365_copilot\): accept max_tokens and list the chat model

* fix\(microsoft_365_copilot\): register provider model set

* fix\(ui\): hide other auth types' fields when credential-only types are filtered

* fix\(health\): do not inherit stored auth settings when a connection test brings its own api_key

* docs\(health\): restore connection-test inheritance docstrings

* fix\(lint\): suppress justified provider boundary casts

* fix\(lint\): satisfy M365 type-discipline gate

* fix\(types\): eliminate type-check gate regressions

* style: shorten suppression reasons so ruff format leaves them on one line

* refactor\(types\): drop shared-file type widening unrelated to the Copilot provider

* fix\(health\): pass caller headers as secret_fields to connection-test probes

* fix\(types\): type connection-test probe and token counter locally

* fix\(lint\): drop cast import from connection-test header getter

* chore: remove explanatory comments flagged by review

* fix\(proxy\): gate OAuth client credentials behind proxy admin

* fix\(router\): skip cooldown on caller-scoped OAuth auth failures

* fix\(router\): skip fallback cooldown on caller-scoped OAuth auth failures

* fix\(router\): type caller-scoped cooldown lookups

* fix\(ci\): include Microsoft 365 Copilot tests in unit shard

* fix\(proxy\): keep saved endpoint when test_connection retests a deployment by id

* fix\(m365\): collapse doubled Graph replies

* fix\(m365\): preserve trailing system messages

---------

Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   | [chore(greptile): load nested AGENTS.md files as review context scoped…](https://github.com/BerriAI/litellm/commit/3c56cd31f5e143ce3f048186128586653fb3688b "chore\(greptile\): load nested AGENTS.md files as review context scoped to their directories \(#45636\)

* chore\(greptile\): load nested AGENTS.md files as review context scoped to their directories

* refactor\(greptile\): use a stdlib dataclass so the files.json check runs without uv

* test\(greptile\): write fixture AGENTS.md files through a helper instead of rebinding a loop variable")  | Oct 9, 2026  |  
| [.semgrep/rules](https://github.com/BerriAI/litellm/tree/main/.semgrep/rules "This path skips through empty directories")  | [.semgrep/rules](https://github.com/BerriAI/litellm/tree/main/.semgrep/rules "This path skips through empty directories")  | [security: remove .claude/settings.json and add semgrep rule to preven…](https://github.com/BerriAI/litellm/commit/b77e1ccea64a86068ad91a8b35355d0a4a9f3164 "security: remove .claude/settings.json and add semgrep rule to prevent re-adding")  | Mar 25, 2026  |  
|   |   |  [feat(proxy): track failed requests by HTTP status and caller (](https://github.com/BerriAI/litellm/commit/2df5b0730845d50a51eb4a40f51b1d0bb6c88e61 "feat\(proxy\): track failed requests by HTTP status and caller \(#45244\)

Counts failed gateway requests by HTTP status at the ASGI edge
\(LiteLLM_DailyGatewayFailedRequests\) and rolls failures up per virtual key,
team, user and model group with the status the logging callbacks recorded
\(LiteLLM_DailyRequestErrors, fed from the spend writer, flushed like the
gateway counters, directly or through Redis with a pod lease, and on
shutdown\). GET /gateway/daily/activity gains by_status_code; the new
admin-only GET /gateway/errors/activity returns per-day totals with
per-status counts, failures by status code, and keys, teams, users and
model groups ranked by failures with their top status. Dashboard schema.d.ts
regenerated.

Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")[#45244](https://github.com/BerriAI/litellm/pull/45244)[)](https://github.com/BerriAI/litellm/commit/2df5b0730845d50a51eb4a40f51b1d0bb6c88e61 "feat\(proxy\): track failed requests by HTTP status and caller \(#45244\)

Counts failed gateway requests by HTTP status at the ASGI edge
\(LiteLLM_DailyGatewayFailedRequests\) and rolls failures up per virtual key,
team, user and model group with the status the logging callbacks recorded
\(LiteLLM_DailyRequestErrors, fed from the spend writer, flushed like the
gateway counters, directly or through Redis with a pod lease, and on
shutdown\). GET /gateway/daily/activity gains by_status_code; the new
admin-only GET /gateway/errors/activity returns per-day totals with
per-status counts, failures by status code, and keys, teams, users and
model groups ranked by failures with their top status. Dashboard schema.d.ts
regenerated.

Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   |  [fix(security): remove the publicly known master key from the repo (](https://github.com/BerriAI/litellm/commit/837c6a7481de91f2fcc2dfda929f7f96796e9a0a "fix\(security\): remove the publicly known master key from the repo \(#44718\)

* fix\(security\): hash the publicly known master key

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* docs: replace weak master key examples and regenerate artifacts

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* test: replace weak key fixtures with generated test keys

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* ci: generate master keys for proxy startup

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix: preserve lens dev key entropy

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix: restore proxy key compatibility in scrub examples

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix: scrub merged SSO fixture and refresh dashboard bundle

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix\(ci\): stabilize test keys and metadata collection

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* chore: drop the rebuilt dashboard bundle

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

---------

Co-authored-by: mateo <mateo@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")[#4…](https://github.com/BerriAI/litellm/pull/44718)  | Oct 6, 2026  |  
|   |   | [feat(prometheus): expose per-project per-model rate limit allowed and…](https://github.com/BerriAI/litellm/commit/9f150f74edf2984d24ca0b3bc925d8cf34f84801 "feat\(prometheus\): expose per-project per-model rate limit allowed and used gauges \(#43561\)

Co-authored-by: yassin <yassin@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Oct 8, 2026  |  
|   |   | [fix(ui): explain unbackfilled key lifetime spend and ship a backfill …](https://github.com/BerriAI/litellm/commit/6a7796e6785e21746d81268690026d674693a69a "fix\(ui\): explain unbackfilled key lifetime spend and ship a backfill script \(#42967\)

* fix\(ui\): explain unbackfilled key lifetime spend and ship a backfill script

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix\(db_scripts\): cover linked budgets, deleted keys and double-hashed logs in total_spend backfill

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix\(db_scripts\): count duplicate archived tokens once in total_spend backfill

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix\(db_scripts\): only rebuild resetting archived rows in total_spend backfill

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix\(db_scripts\): split spend log rebuild into opt-in backfill_key_total_spend_from_spend_logs.sql

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* docs\(db_scripts\): scope backfill verify query to non-resetting keys

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix\(db_scripts\): lift every key to at least current spend in backfill_key_total_spend.sql

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* docs\(db_scripts\): align spend log backfill header with lifted floor

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix\(ui\): point resetting keys at the spend log backfill

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix\(ui\): drop script name from lifetime spend tooltip

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

---------

Co-authored-by: ryan <ryan@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Sep 24, 2026  |  
| [deploy/lens](https://github.com/BerriAI/litellm/tree/main/deploy/lens "This path skips through empty directories")  | [deploy/lens](https://github.com/BerriAI/litellm/tree/main/deploy/lens "This path skips through empty directories")  |  [feat(lens): simplify deployment and first trace setup (](https://github.com/BerriAI/litellm/commit/dc2a14a6564c79ef6a5ca337434bda996dd3ecd2 "feat\(lens\): simplify deployment and first trace setup \(#45230\)

* feat\(lens\): simplify deployment and first trace setup

* test\(lens\): keep setup fixtures within lint budgets

* fix\(lens\): preserve setup state and harden bundled storage startup

* fix\(lens\): handle setup recovery and Helm endpoint boundaries")[#45230](https://github.com/BerriAI/litellm/pull/45230)[)](https://github.com/BerriAI/litellm/commit/dc2a14a6564c79ef6a5ca337434bda996dd3ecd2 "feat\(lens\): simplify deployment and first trace setup \(#45230\)

* feat\(lens\): simplify deployment and first trace setup

* test\(lens\): keep setup fixtures within lint budgets

* fix\(lens\): preserve setup state and harden bundled storage startup

* fix\(lens\): handle setup recovery and Helm endpoint boundaries")  | Oct 8, 2026  |  
|   |   |  [feat(lens): isolate ingestion and investigations in a Rust service (](https://github.com/BerriAI/litellm/commit/e9cfba2c1755d7b1ffbdb3b463defbfdf83412e9 "feat\(lens\): isolate ingestion and investigations in a Rust service \(#45148\)

* feat\(lens\): isolate trace storage and investigation in a Rust service

* fix\(lens\): include Rust sources in the image build context

* feat\(lens\): wire service setup, scoped delivery receipts and lease attempts

* fix\(lens\): complete service routing and reject stale investigation results

* fix\(lens\): retry key propagation and validate isolated Compose setup

* fix\(lens\): seed through isolated ingestion and preserve upstream queue fixes

* chore: sync schema.prisma copies from root

* fix\(lens\): bind nullable due timestamps as text for Prisma

* chore\(ui\): remove stale lint suppressions

* fix\(lens\): address CI failures and review findings

* refactor\(lens\): remove retired Python worker and run evaluations in Rust

* fix\(lens\): reuse control connections and satisfy review checks

* test\(lens\): install and upgrade both Helm charts on Kubernetes

* test\(lens\): run connection reuse coverage as an integration test

* fix\(ui\): upgrade Next.js to 16.3.8 security release

* fix\(lens\): fence stale attempts and preserve reviewed evidence

* Revert "fix\(ui\): upgrade Next.js to 16.3.8 security release"

This reverts commit 2f79a51b2564417e4d06dadcafdfe41078db6af1.

* fix\(lens\): stop failed investigations and stream history excerpts

* test\(lens\): cover model tool and result contracts

* test\(lens\): fix retired routes and reuse installation build artifacts

* test\(lens\): use portable grep in Helm installation smoke

* test\(lens\): wait for migrations before forwarding Helm services

* fix\(lens\): keep failed evidence reads retryable

* fix\(lens\): preserve sandbox output during process exit

---------

Co-authored-by: github-actions\[bot\] <41898282+github-actions\[bot\]@users.noreply.github.com>")[#…](https://github.com/BerriAI/litellm/pull/45148)  | Oct 8, 2026  |  
|   |   | [chore: bump litellm-enterprise 0.1.75 -> 0.1.76, litellm-proxy-extras…](https://github.com/BerriAI/litellm/commit/cdba36a1f0c9475ca64bb1e357ebfa152c687be3 "chore: bump litellm-enterprise 0.1.75 -> 0.1.76, litellm-proxy-extras 0.4.107 -> 0.4.108 \(#45534\)

Co-authored-by: yuneng <yuneng@berri.ai>")  | Oct 9, 2026  |  
|   |   |   | Jun 26, 2026  |  
|   |   | [feat(decisions): serve System One format at /v1/systemone and OpenAI …](https://github.com/BerriAI/litellm/commit/8fc31b1e4e340bef600cdbde0fedac1702224d35 "feat\(decisions\): serve System One format at /v1/systemone and OpenAI format at /v1/decisions \(#45184\)

* feat\(decisions\): serve System One format at /v1/systemone and OpenAI format at /v1/decisions

The System One request format moves to /v1/systemone and /systemone. /v1/decisions and
/decisions now accept the OpenAI Decisions API format, translate it into a System One
request, route it through the same pipeline, and translate the answers back.

* fix\(decisions\): import assert_never from typing_extensions for Python 3.10

* fix\(decisions\): cap OpenAI-format questions at the System One limit

/v1/decisions accepted up to 200 questions, but the System One request it translates to takes at most 128, so 129 to 200 questions failed with an internal validation error. Both now share MAX_DECISION_QUESTIONS.

* fix\(decisions\): return 400 for bodies that are not JSON

* test\(decisions\): send System One bodies to /v1/systemone in integration tests

* fix\(decisions\): return guardrail_information on OpenAI-format decisions when requested

* refactor\(decisions\): validate proxy request data before reading guardrail settings")  | Oct 8, 2026  |  
|  [feat(lens): simplify deployment and first trace setup (](https://github.com/BerriAI/litellm/commit/dc2a14a6564c79ef6a5ca337434bda996dd3ecd2 "feat\(lens\): simplify deployment and first trace setup \(#45230\)

* feat\(lens\): simplify deployment and first trace setup

* test\(lens\): keep setup fixtures within lint budgets

* fix\(lens\): preserve setup state and harden bundled storage startup

* fix\(lens\): handle setup recovery and Helm endpoint boundaries")[#45230](https://github.com/BerriAI/litellm/pull/45230)[)](https://github.com/BerriAI/litellm/commit/dc2a14a6564c79ef6a5ca337434bda996dd3ecd2 "feat\(lens\): simplify deployment and first trace setup \(#45230\)

* feat\(lens\): simplify deployment and first trace setup

* test\(lens\): keep setup fixtures within lint budgets

* fix\(lens\): preserve setup state and harden bundled storage startup

* fix\(lens\): handle setup recovery and Helm endpoint boundaries")  | Oct 8, 2026  |  
|   |   | [feat(github_copilot): per-user GitHub OAuth connections for Copilot c…](https://github.com/BerriAI/litellm/commit/d36b66e19ebd2cfc2728ac6e50cacee3e902fbd0 "feat\(github_copilot\): per-user GitHub OAuth connections for Copilot credentials \(#45241\)

* feat\(github_copilot\): per-user GitHub OAuth connections for Copilot credentials

Related to LIT-9306

* fix\(ui\): make per-user GitHub Copilot OAuth credential-only in Add Model

* fix\(proxy\): render LiteLLM_UserProviderCredentials prisma spans

* style\(ui\): prettier-format add_model tests

* fix\(lint\): annotate connection endpoints and narrow per-user credential excepts

* fix\(lint\): satisfy type-discipline gate for per-user copilot code

* fix\(types\): clear basedpyright and strict-lint gate regressions

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix\(types\): keep runtime guards on untyped responses input

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* style: ruff format per-user Copilot files

* fix\(lint\): keep type-discipline suppressions on flagged lines

* fix\(github_copilot\): resolve team and deployment-id per-user routes, share token cache invalidation

* test\(github_copilot\): scope response-cache mutation via monkeypatch, assert route allowlist

* fix\(github_copilot\): fall back to the database on Redis errors and skip worker-local token caching

* fix\(lint\): suppress mutable return annotation after base merge

* style: shorten rebind-ok reason so the dispatch line stays formatted

* fix\(github_copilot\): harden per-user token cache, overrides, fallbacks, headers and device flow

* fix\(lint\): suppress MutableMapping param on session auth pinning

* test\(github_copilot\): narrow pytest.raises to HTTPException for service-key override

* fix\(github_copilot\): fail closed on revoke errors, strip caller auth headers, widen fallback discovery

* fix\(github_copilot\): verify revoke tombstone, bound fallback discovery, cover aliased fallbacks

* fix\(github_copilot\): make fallback discovery a router superset and guard credential override at deployment

* style\(router\): drop explanatory comment on per-user credential guard

* fix\(github_copilot\): gate fallback discovery on per-user credentials and bound aggregate targets

* fix\(github_copilot\): count every fallback mapping toward the discovery limit

* fix\(github_copilot\): suppress banned typing cast imports with per-line reasons

* test\(github_copilot\): cover per-user 401 cooldown skip through the router failure callback

* test\(github_copilot\): cover per-user 401 cooldown skip through router and fallback paths

* fix\(github_copilot\): cover auto-router discovery, per-user fallback cooldown and connect cache race

* fix\(github_copilot\): cover every strategy router kind and verify the connect cache write

* fix merge conflict marker remnant in credential endpoint tests

* fix\(github_copilot\): do not purge user connections on label-only credential updates

* fix\(github_copilot\): read user provider credentials from the writer engine

* test\(github_copilot\): move per-user auth type form test to the integration tier

* test\(github_copilot\): annotate new credential regression test locals as Final

* fix\(github_copilot\): type per-user OAuth JSON and DB boundaries

* fix\(github_copilot\): validate per-user OAuth boundaries and restore truncated comments

* style\(github_copilot\): complete cut-off suppression reasons on new casts

* refactor\(github_copilot\): annotate new boundary-typing locals as Final

---------

Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   |  [fix(rust): update serde_with for serialization advisory (](https://github.com/BerriAI/litellm/commit/3ebc71be2a4691bd5e702a6ab15ef4a40e14f938 "fix\(rust\): update serde_with for serialization advisory \(#45498\)")[#45498](https://github.com/BerriAI/litellm/pull/45498)[)](https://github.com/BerriAI/litellm/commit/3ebc71be2a4691bd5e702a6ab15ef4a40e14f938 "fix\(rust\): update serde_with for serialization advisory \(#45498\)")  | Oct 9, 2026  |  
|   |   |  [refactor(cost-map): remove openrouter rows, part 5 of 5 (](https://github.com/BerriAI/litellm/commit/3ea80696246b79535386ee53c710e103a6b08dc0 "refactor\(cost-map\): remove openrouter rows, part 5 of 5 \(#45669\)

* refactor\(cost-map\): remove openrouter rows, part 1 of 5 \(anthropic to google block\)

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 2 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 3 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 4 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 5 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

---------

Co-authored-by: kerry <kerry@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")[#45669](https://github.com/BerriAI/litellm/pull/45669)[)](https://github.com/BerriAI/litellm/commit/3ea80696246b79535386ee53c710e103a6b08dc0 "refactor\(cost-map\): remove openrouter rows, part 5 of 5 \(#45669\)

* refactor\(cost-map\): remove openrouter rows, part 1 of 5 \(anthropic to google block\)

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 2 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 3 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 4 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 5 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

---------

Co-authored-by: kerry <kerry@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   | [fix(proxy-extras): build the SpendLogs indexes in the migration job i…](https://github.com/BerriAI/litellm/commit/c66c8288c3e6b9111fab633a8d72f5d6a1ddc82f "fix\(proxy-extras\): build the SpendLogs indexes in the migration job instead of in migrations \(#43948\)

The two SpendLogs index migrations shipped in v1.103.0 each break one table shape: the plain CREATE INDEX holds a SHARE lock on a large unpartitioned table and the CONCURRENTLY one fails with 0A000 on a partitioned parent. Both files are now inert and the indexes are built by a table-driven, shape-aware step after migrate deploy: CONCURRENTLY on a plain table, ON ONLY the parent plus per-partition CONCURRENTLY and ATTACH PARTITION on a partitioned one. The migration job builds them synchronously and exits non-zero on failure; a serving proxy that ran migrate deploy itself builds them in the background off the readiness path. A valid index of the same definition under another name is renamed and reused, an invalid one is rebuilt, and extra copies are reported with their DROP INDEX statement instead of being dropped. The migration checker rejects any CREATE INDEX on LiteLLM_SpendLogs or LiteLLM_ErrorLogs in future migrations

Co-authored-by: yassin <yassin@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Oct 1, 2026  |  
|   |   |  [refactor(sdk): separate core AWS and tokenizer dependencies (](https://github.com/BerriAI/litellm/commit/911aff27524282b1ced52a9b50d33a3127e80052 "refactor\(sdk\): separate core AWS and tokenizer dependencies \(#44447\)

* refactor\(sdk\): separate core AWS and tokenizer dependencies

* fix\(sdk\): preserve runtime tokenizer alias compatibility

* test\(sdk\): compare tokenizer installs through existing entry point

* fix\(sdk\): preserve bearer headers and optional dependency interfaces

* fix\(sdk\): retain safe tokenizer fallback diagnostics

* test\(sdk\): inject tokenizer dependency for fallback diagnostics

* fix\(core\): preserve AWS dependency errors with retries

* fix\(core\): centralize optional AWS dependency handling

* refactor\(core\): reuse optional import helper for Invoke streams

* test\(core\): compare complete shared dependency requirements

* fix\(core\): keep tokenizer logging import compatible with main

* fix\(core\): preserve native token decoding and Responses dependency errors

* test\(core\): isolate optional dependency import failures

* fix\(sdk\): retain typed exception message access

* test\(sdk\): scope HTTP verification environment changes

* fix\(sdk\): preserve bearer request typing across Bedrock callers

---------

Co-authored-by: Joshua Valluru <326636767+joshua-berri@users.noreply.github.com>")[#44447](https://github.com/BerriAI/litellm/pull/44447)[)](https://github.com/BerriAI/litellm/commit/911aff27524282b1ced52a9b50d33a3127e80052 "refactor\(sdk\): separate core AWS and tokenizer dependencies \(#44447\)

* refactor\(sdk\): separate core AWS and tokenizer dependencies

* fix\(sdk\): preserve runtime tokenizer alias compatibility

* test\(sdk\): compare tokenizer installs through existing entry point

* fix\(sdk\): preserve bearer headers and optional dependency interfaces

* fix\(sdk\): retain safe tokenizer fallback diagnostics

* test\(sdk\): inject tokenizer dependency for fallback diagnostics

* fix\(core\): preserve AWS dependency errors with retries

* fix\(core\): centralize optional AWS dependency handling

* refactor\(core\): reuse optional import helper for Invoke streams

* test\(core\): compare complete shared dependency requirements

* fix\(core\): keep tokenizer logging import compatible with main

* fix\(core\): preserve native token decoding and Responses dependency errors

* test\(core\): isolate optional dependency import failures

* fix\(sdk\): retain typed exception message access

* test\(sdk\): scope HTTP verification environment changes

* fix\(sdk\): preserve bearer request typing across Bedrock callers

---------

Co-authored-by: Joshua Valluru <326636767+joshua-berri@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   | [ci: render lint, unit and smoke checks as <tier> / <job> with one col…](https://github.com/BerriAI/litellm/commit/4b975e6f490a559ef16b74574c4cd637e20d79cd "ci: render lint, unit and smoke checks as <tier> / <job> with one collector per tier \(#45480\)

* ci: restructure lint, unit and smoke workflows

* ci: preserve source formatting in tier workflows

* ci: retire per-shard coverage flags and tighten the tier workflow guards

* test\(ci\): freeze the workflow startup safety models

* ci: keep the current required check names running until the ruleset moves to the tier collectors

---------

Co-authored-by: yuneng <yuneng@berri.ai>")  | Oct 9, 2026  |  
|   |   | [feat(github_copilot): per-user GitHub OAuth connections for Copilot c…](https://github.com/BerriAI/litellm/commit/d36b66e19ebd2cfc2728ac6e50cacee3e902fbd0 "feat\(github_copilot\): per-user GitHub OAuth connections for Copilot credentials \(#45241\)

* feat\(github_copilot\): per-user GitHub OAuth connections for Copilot credentials

Related to LIT-9306

* fix\(ui\): make per-user GitHub Copilot OAuth credential-only in Add Model

* fix\(proxy\): render LiteLLM_UserProviderCredentials prisma spans

* style\(ui\): prettier-format add_model tests

* fix\(lint\): annotate connection endpoints and narrow per-user credential excepts

* fix\(lint\): satisfy type-discipline gate for per-user copilot code

* fix\(types\): clear basedpyright and strict-lint gate regressions

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix\(types\): keep runtime guards on untyped responses input

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* style: ruff format per-user Copilot files

* fix\(lint\): keep type-discipline suppressions on flagged lines

* fix\(github_copilot\): resolve team and deployment-id per-user routes, share token cache invalidation

* test\(github_copilot\): scope response-cache mutation via monkeypatch, assert route allowlist

* fix\(github_copilot\): fall back to the database on Redis errors and skip worker-local token caching

* fix\(lint\): suppress mutable return annotation after base merge

* style: shorten rebind-ok reason so the dispatch line stays formatted

* fix\(github_copilot\): harden per-user token cache, overrides, fallbacks, headers and device flow

* fix\(lint\): suppress MutableMapping param on session auth pinning

* test\(github_copilot\): narrow pytest.raises to HTTPException for service-key override

* fix\(github_copilot\): fail closed on revoke errors, strip caller auth headers, widen fallback discovery

* fix\(github_copilot\): verify revoke tombstone, bound fallback discovery, cover aliased fallbacks

* fix\(github_copilot\): make fallback discovery a router superset and guard credential override at deployment

* style\(router\): drop explanatory comment on per-user credential guard

* fix\(github_copilot\): gate fallback discovery on per-user credentials and bound aggregate targets

* fix\(github_copilot\): count every fallback mapping toward the discovery limit

* fix\(github_copilot\): suppress banned typing cast imports with per-line reasons

* test\(github_copilot\): cover per-user 401 cooldown skip through the router failure callback

* test\(github_copilot\): cover per-user 401 cooldown skip through router and fallback paths

* fix\(github_copilot\): cover auto-router discovery, per-user fallback cooldown and connect cache race

* fix\(github_copilot\): cover every strategy router kind and verify the connect cache write

* fix merge conflict marker remnant in credential endpoint tests

* fix\(github_copilot\): do not purge user connections on label-only credential updates

* fix\(github_copilot\): read user provider credentials from the writer engine

* test\(github_copilot\): move per-user auth type form test to the integration tier

* test\(github_copilot\): annotate new credential regression test locals as Final

* fix\(github_copilot\): type per-user OAuth JSON and DB boundaries

* fix\(github_copilot\): validate per-user OAuth boundaries and restore truncated comments

* style\(github_copilot\): complete cut-off suppression reasons on new casts

* refactor\(github_copilot\): annotate new boundary-typing locals as Final

---------

Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   |  [refactor(cost-map): remove openrouter rows, part 5 of 5 (](https://github.com/BerriAI/litellm/commit/3ea80696246b79535386ee53c710e103a6b08dc0 "refactor\(cost-map\): remove openrouter rows, part 5 of 5 \(#45669\)

* refactor\(cost-map\): remove openrouter rows, part 1 of 5 \(anthropic to google block\)

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 2 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 3 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 4 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 5 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

---------

Co-authored-by: kerry <kerry@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")[#45669](https://github.com/BerriAI/litellm/pull/45669)[)](https://github.com/BerriAI/litellm/commit/3ea80696246b79535386ee53c710e103a6b08dc0 "refactor\(cost-map\): remove openrouter rows, part 5 of 5 \(#45669\)

* refactor\(cost-map\): remove openrouter rows, part 1 of 5 \(anthropic to google block\)

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 2 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 3 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 4 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 5 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

---------

Co-authored-by: kerry <kerry@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Oct 9, 2026  |  
|  [fix(ui): show the Microsoft 365 Copilot logo instead of the Azure one (](https://github.com/BerriAI/litellm/commit/06006932c7ef76edd2e4cd2150755e85487bcbab "fix\(ui\): show the Microsoft 365 Copilot logo instead of the Azure one \(#45687\)

Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")[…](https://github.com/BerriAI/litellm/pull/45687)  | Oct 9, 2026  |  
|   |   | [fix(deps): bump source-map-js, smol-toml, mako, multidict and werkzeu…](https://github.com/BerriAI/litellm/commit/56a581a536fc8d98b696659a71c07da328aff60e "fix\(deps\): bump source-map-js, smol-toml, mako, multidict and werkzeug for OSV advisories \(#44728\)

* fix\(deps\): bump source-map-js to 1.2.2 for GHSA-68fv-2mgg-jv7q

* fix\(deps\): bump mako, multidict, werkzeug and smol-toml for OSV advisories

---------

Co-authored-by: yuneng <yuneng@berri.ai>")  | Oct 6, 2026  |  
|   |   | [build(docker): build the Admin UI from source in a build-platform-pin…](https://github.com/BerriAI/litellm/commit/52e5b3ae98421763545c4a9430ae67432cdf9518 "build\(docker\): build the Admin UI from source in a build-platform-pinned stage \(#31130\)

The monolith images shipped whatever UI bundle was committed to
litellm/proxy/_experimental/out, so refreshing the UI for a release meant
running build_ui.sh out of band and committing the regenerated bundle. Add a
ui-builder stage to all three monolith Dockerfiles \(root, database, non_root\)
that compiles the Next.js static export from this exact source and replaces the
committed bundle before the final uv sync.

The stage is pinned with FROM --platform=$BUILDPLATFORM so the
architecture-independent static export compiles once on the native builder even
in a multi-arch \(linux/amd64,linux/arm64\) build, rather than once per target
arch under QEMU emulation. The destination is cleared before the COPY because
COPY merges directories and would otherwise leave the committed bundle's
content-hashed chunks behind alongside the fresh ones. build_admin_ui.sh still
runs afterward so the enterprise custom-color override is preserved.

The UI base image is pinned by digest to match LITELLM_BUILD_IMAGE,
LITELLM_RUNTIME_IMAGE and UV_IMAGE, and .dockerignore now excludes the local
.next/out so a developer's build artifacts never enter the context.")  | Jun 26, 2026  |  
|   |   | [docs: stop advertising sk-1234 as the master key in shipped configs a…](https://github.com/BerriAI/litellm/commit/c8e0f2ddb4452f7cf1704a640f903129e16d0a58 "docs: stop advertising sk-1234 as the master key in shipped configs and examples

Shipped proxy configs now read general_settings.master_key from
os.environ/LITELLM_MASTER_KEY, the .env examples ship a blank value with
the openssl generate command above it, and READMEs, the missing env vars
page and Admin UI code snippets show a generate command or the
<your-master-key> placeholder instead of the literal sk-1234

The two CircleCI docker runs that mount proxy_server_config.yaml and
oai_misc_config.yaml now pass LITELLM_MASTER_KEY so their runtime key is
unchanged")  | Sep 19, 2026  |  
|   |   | [chore: ignore the mechanical lint and typing sweeps in git blame](https://github.com/BerriAI/litellm/commit/2c91166d32d5253ae7434c796c2365598c18b8b5 "chore: ignore the mechanical lint and typing sweeps in git blame

Seven wide-reaching but semantically neutral commits landed since the
last entry, together rewriting roughly 162k lines across ~4,700 file
touches. Blame on any line they reflowed points at the sweep instead of
the commit that wrote the logic.

They cover the safe ruff autofix pass, the collections.abc import move,
the f-string !s cleanup, lazy log message construction, the LIT010 and
LIT011 Final and frozen-parameter rollout, ruff coverage for
litellm/types, and the inert type: ignore strip.

Smaller ratchet commits are left out on purpose: each touches a few
hundred lines at most, so listing them would grow the file faster than
it buys back blame accuracy")  | Aug 6, 2026  |  
|   |   |  [feat(ui): generate dashboard API types from the proxy OpenAPI spec (](https://github.com/BerriAI/litellm/commit/e53bd7cbd1de07dde2ed2be07c242d1624269868 "feat\(ui\): generate dashboard API types from the proxy OpenAPI spec \(#29816\)

* feat\(ui\): generate dashboard API types from the proxy OpenAPI spec

Introduces the shared type foundation for the dashboard without touching any
runtime code. The proxy's FastAPI app is the source of truth; app.openapi\(\)
emits the spec and openapi-typescript turns it into src/lib/http/schema.d.ts.

Adds an npm run gen:api script \(a Python spec dump piped into openapi-typescript\)
and a Check UI API Types Sync CI job that regenerates the file from the live
spec and fails if it drifts, so the committed types can never silently fall out
of step with the backend. The generated file is pinned to openapi-typescript
7.13.0 and excluded from prettier, eslint, and knip, and marked linguist-generated
so it collapses in diffs.

No openapi-fetch and no call-site changes yet; this only makes the types exist.

* chore\(ui\): tidy gen-api-types script per review

Write the spec dump inside a with-block and clean up the temp dir in a
finally, so repeated local runs don't leave stray ~MB JSON files behind.")[#…](https://github.com/BerriAI/litellm/pull/29816)  | Jun 6, 2026  |  
|   |   |  [fix(security): remove the publicly known master key from the repo (](https://github.com/BerriAI/litellm/commit/837c6a7481de91f2fcc2dfda929f7f96796e9a0a "fix\(security\): remove the publicly known master key from the repo \(#44718\)

* fix\(security\): hash the publicly known master key

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* docs: replace weak master key examples and regenerate artifacts

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* test: replace weak key fixtures with generated test keys

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* ci: generate master keys for proxy startup

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix: preserve lens dev key entropy

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix: restore proxy key compatibility in scrub examples

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix: scrub merged SSO fixture and refresh dashboard bundle

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix\(ci\): stabilize test keys and metadata collection

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* chore: drop the rebuilt dashboard bundle

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

---------

Co-authored-by: mateo <mateo@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")[#4…](https://github.com/BerriAI/litellm/pull/44718)  | Oct 6, 2026  |  
|   |   |   | Oct 9, 2026  |  
|   |   |  [ci(image-scan): ignore zlib](https://github.com/BerriAI/litellm/commit/064d5d61dad263bad11a77668d1de75176767b69 "ci\(image-scan\): ignore zlib CVE-2026-85091 until Wolfi ships the fix

Wolfi's security database names zlib 1.3.3-r0 as the fix for CVE-2026-85091,
but the newest zlib published to the Wolfi apk repo is 1.3.2-r7. Every
wolfi-base digest, including the current latest, still reports the CVE, so no
base image bump or apk upgrade can clear it and image-scan fails on every PR
touching a Dockerfile or the lockfile, and on the nightly schedule.

Ignore that CVE and its GHSA alias for the zlib apk package only, so a fixable
High in anything else still fails the job.") [CVE-2026-85091](https://github.com/advisories/GHSA-g5fp-32jq-cfw2 "CVE-2026-85091") [until Wolfi ships the fix](https://github.com/BerriAI/litellm/commit/064d5d61dad263bad11a77668d1de75176767b69 "ci\(image-scan\): ignore zlib CVE-2026-85091 until Wolfi ships the fix

Wolfi's security database names zlib 1.3.3-r0 as the fix for CVE-2026-85091,
but the newest zlib published to the Wolfi apk repo is 1.3.2-r7. Every
wolfi-base digest, including the current latest, still reports the CVE, so no
base image bump or apk upgrade can clear it and image-scan fails on every PR
touching a Dockerfile or the lockfile, and on the nightly schedule.

Ignore that CVE and its GHSA alias for the zlib apk package only, so a fixable
High in anything else still fails the job.")  | Sep 16, 2026  |  
|   |   | [[Fix] CI/Tooling: Correct min-release-age value in .npmrc files](https://github.com/BerriAI/litellm/commit/1005fcd592180bd22f44ab21c5f154351d8709ed "\[Fix\] CI/Tooling: Correct min-release-age value in .npmrc files

npm's `min-release-age` config has type `\[null, Number\]`. The value `3d`
parses to NaN, which propagates into `before = new Date\(NaN\)` \(Invalid
Date\). Pacote then calls `.toISOString\(\)` on it and throws
`RangeError: Invalid time value`, breaking every local `npm install`.

Drop the `d` suffix in all six `.npmrc` files. The `<days>` in npm's
type hint is a label, not part of the value.

This is a no-op for CI \(`npm ci` ignores this setting per the comment
in the file\) but unblocks local `npm install`.")  | Apr 30, 2026  |  
|   |   | [ci(lint): gate every rule on its merge-base count and cap Anys at a f…](https://github.com/BerriAI/litellm/commit/d6e453f74b45ab10ee30276091eb40773f3322af "ci\(lint\): gate every rule on its merge-base count and cap Anys at a fixed total \(#40193\)

* ci\(lint\): derive gate ceilings from merge-base counts and drop the budget files

The four lint gates \(ruff strict, type discipline, basedpyright, test quality\)
now fail a branch only when a rule's codebase count grows past its count at the
merge-base with litellm_internal_staging plus a fixed per-rule headroom, which
is zero everywhere except the LIT010/LIT011 and reportAny/reportExplicitAny
seeds. Base counts come from a disk cache, then the CI artifact the renamed
publish-lint-base-counts workflow uploads for every staging push \(all four
checkers, one artifact per checker and sha\), then a scan of the base worktree.

The four *-budget.json files, make lint-budget-update, budget_ratchet_check.py,
and the unratcheted check are gone, so no PR carries a budget edit again.

* test\(lint-gates\): assert checker identities by behavior and type the new gate tests

* test\(lint-gates\): resolve base points over an injected git so unit tests stay in-process

---------

Co-authored-by: mateo-berri <277851410+mateo-berri@users.noreply.github.com>")  | Oct 8, 2026  |  
|   |   |  [refactor(anthropic): rename experimental_pass_through to pass_through (](https://github.com/BerriAI/litellm/commit/69ad0040158a0d0074073b2bdadc2834ded12ee2 "refactor\(anthropic\): rename experimental_pass_through to pass_through \(#43329\)

* refactor\(anthropic\): rename experimental_pass_through to pass_through

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* test\(anthropic\): point compact patch targets at renamed pass_through path

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

---------

Co-authored-by: kerry <kerry@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")[…](https://github.com/BerriAI/litellm/pull/43329)  | Sep 26, 2026  |  
|   |   |  [chore(deps): drop unused pytest-postgresql dev dependency (](https://github.com/BerriAI/litellm/commit/e725832cd8390d659b9c3890b96d0a0ae2ec6c42 "chore\(deps\): drop unused pytest-postgresql dev dependency \(#44056\)

The pytest-postgresql based proxy tests moved to tests/integration on the real
Postgres harness in #43996, so nothing loads the plugin anymore. uv.lock is
edited by hand to drop the package and its now orphaned mirakuru and port-for
deps; uv lock --check passes and a full relock resolves the same package set")[#44056](https://github.com/BerriAI/litellm/pull/44056)[)](https://github.com/BerriAI/litellm/commit/e725832cd8390d659b9c3890b96d0a0ae2ec6c42 "chore\(deps\): drop unused pytest-postgresql dev dependency \(#44056\)

The pytest-postgresql based proxy tests moved to tests/integration on the real
Postgres harness in #43996, so nothing loads the plugin anymore. uv.lock is
edited by hand to drop the package and its now orphaned mirakuru and port-for
deps; uv lock --check passes and a full relock resolves the same package set")  | Oct 1, 2026  |  
|   |   |  [revert(docker): unpin openssl-3.6-dev in the pgbouncer-builder stage (](https://github.com/BerriAI/litellm/commit/5ffad371b021f9b910c2233cf238afe0c4c9d464 "revert\(docker\): unpin openssl-3.6-dev in the pgbouncer-builder stage \(#44997\)

This reverts commit b91b2fc1388606b8cf61df7d34e485fc6db4ce42 \(#44991\).

Co-authored-by: yuneng <yuneng@berri.ai>")[#…](https://github.com/BerriAI/litellm/pull/44997)  | Oct 7, 2026  |  
|   |   | [chore: consolidate CLAUDE.md into AGENTS.md](https://github.com/BerriAI/litellm/commit/bf7d1c07330a7d5676fb4a461f7b7f72bc4098d3 "chore: consolidate CLAUDE.md into AGENTS.md

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Sep 19, 2026  |  
|   |   | [refactor: creating enterprise folder](https://github.com/BerriAI/litellm/commit/a9e79c8d4645f963c642387e2fef9b8c5474765e "refactor: creating enterprise folder")  | Feb 15, 2024  |  
|   |   | [ci: render lint, unit and smoke checks as <tier> / <job> with one col…](https://github.com/BerriAI/litellm/commit/4b975e6f490a559ef16b74574c4cd637e20d79cd "ci: render lint, unit and smoke checks as <tier> / <job> with one collector per tier \(#45480\)

* ci: restructure lint, unit and smoke workflows

* ci: preserve source formatting in tier workflows

* ci: retire per-shard coverage flags and tighten the tier workflow guards

* test\(ci\): freeze the workflow startup safety models

* ci: keep the current required check names running until the ruleset moves to the tier collectors

---------

Co-authored-by: yuneng <yuneng@berri.ai>")  | Oct 9, 2026  |  
|   |   |  [feat(sdk): build core from an independent packaging manifest (](https://github.com/BerriAI/litellm/commit/f48d837cd21208771ee5b15770e4425a32907f1b "feat\(sdk\): build core from an independent packaging manifest \(#44340\)

* feat\(sdk\): build core from an independent packaging manifest

* test\(sdk\): compare rebuilt core payload without generated SBOM identity

* fix\(sdk\): retain native build configuration in the core sdist

* test\(sdk\): collect coverage from the core build entry point

* fix\(ci\): isolate core packaging coverage configuration

* fix\(test\): identify installed core metadata on Python 3.10

* fix\(packaging\): preserve Git ignore rules in core staging

* fix\(packaging\): support source-only core builds

* fix\(packaging\): reject overlapping SDK distributions

* fix\(packaging\): align core requirements with current main

---------

Co-authored-by: Joshua Valluru <326636767+joshua-berri@users.noreply.github.com>")[#44340](https://github.com/BerriAI/litellm/pull/44340)[)](https://github.com/BerriAI/litellm/commit/f48d837cd21208771ee5b15770e4425a32907f1b "feat\(sdk\): build core from an independent packaging manifest \(#44340\)

* feat\(sdk\): build core from an independent packaging manifest

* test\(sdk\): compare rebuilt core payload without generated SBOM identity

* fix\(sdk\): retain native build configuration in the core sdist

* test\(sdk\): collect coverage from the core build entry point

* fix\(ci\): isolate core packaging coverage configuration

* fix\(test\): identify installed core metadata on Python 3.10

* fix\(packaging\): preserve Git ignore rules in core staging

* fix\(packaging\): support source-only core builds

* fix\(packaging\): reject overlapping SDK distributions

* fix\(packaging\): align core requirements with current main

---------

Co-authored-by: Joshua Valluru <326636767+joshua-berri@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   | [ci: render lint, unit and smoke checks as <tier> / <job> with one col…](https://github.com/BerriAI/litellm/commit/4b975e6f490a559ef16b74574c4cd637e20d79cd "ci: render lint, unit and smoke checks as <tier> / <job> with one collector per tier \(#45480\)

* ci: restructure lint, unit and smoke workflows

* ci: preserve source formatting in tier workflows

* ci: retire per-shard coverage flags and tighten the tier workflow guards

* test\(ci\): freeze the workflow startup safety models

* ci: keep the current required check names running until the ruleset moves to the tier collectors

---------

Co-authored-by: yuneng <yuneng@berri.ai>")  | Oct 9, 2026  |  
|   |   | [[Infra] Add release workflow and cosign public key](https://github.com/BerriAI/litellm/commit/0112e53046018d726492c814b3644b7d376029d0 "\[Infra\] Add release workflow and cosign public key

Add create-release.yml workflow triggered via workflow_dispatch to create
GitHub releases with auto-generated notes. Add cosign public key for
container image signature verification.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>")  | Mar 31, 2026  |  
| [docker-compose.hardened.yml](https://github.com/BerriAI/litellm/blob/main/docker-compose.hardened.yml "docker-compose.hardened.yml")  | [docker-compose.hardened.yml](https://github.com/BerriAI/litellm/blob/main/docker-compose.hardened.yml "docker-compose.hardened.yml")  | [build(docker): drop the no-op PROXY_EXTRAS_SOURCE switch from the non…](https://github.com/BerriAI/litellm/commit/3a91029328aeb81dba11236d6427649ce35cbbe5 "build\(docker\): drop the no-op PROXY_EXTRAS_SOURCE switch from the non-root image \(#44097\)

uv sync --frozen ignores --no-sources-package, so the "published" branch
already installed litellm-proxy-extras from the workspace \(the shipped
main-stable non-root image records file:///app/litellm-proxy-extras in its
direct_url.json\). Both branches ran the same install. Keep the single
uv sync that the main and database images use.")  | Oct 2, 2026  |  
| [docker-compose.liteadmin.yml](https://github.com/BerriAI/litellm/blob/main/docker-compose.liteadmin.yml "docker-compose.liteadmin.yml")  | [docker-compose.liteadmin.yml](https://github.com/BerriAI/litellm/blob/main/docker-compose.liteadmin.yml "docker-compose.liteadmin.yml")  |  [feat(enterprise): bundle LiteAdmin Slack with native gateway login (](https://github.com/BerriAI/litellm/commit/6370104c53ef5ae02a5e499993461d2135dbd5ce "feat\(enterprise\): bundle LiteAdmin Slack with native gateway login \(#44444\)

* feat\(enterprise\): bundle LiteAdmin Slack worker with native gateway login

* fix\(enterprise\): preserve gateway prefixes during native Slack linking

* fix\(enterprise\): retain native Slack linking on the admin backend

* fix\(enterprise\): reuse shared native Slack connection services")[#…](https://github.com/BerriAI/litellm/pull/44444)  | Oct 4, 2026  |  
|   |   | [feat: add read-replica routing for Prisma DB via DATABASE_URL_READ_RE…](https://github.com/BerriAI/litellm/commit/b5d3a5fc856ed1cf9b101d37bd0ec6d6d44751b2 "feat: add read-replica routing for Prisma DB via DATABASE_URL_READ_REPLICA \(#27493\)

- Introduce RoutingPrismaWrapper that transparently routes read operations \(find_*, count, group_by, query_raw, query_first\) to a reader endpoint while writes remain on the writer, enabling Aurora-style reader/writer endpoint splits
- Add IAMEndpoint dataclass and parse_iam_endpoint_from_url\(\) to capture static connection fields from a reader URL so only the IAM token needs to rotate, avoiding the need for separate DATABASE_HOST_READ_REPLICA/etc. env vars
- Enhance PrismaWrapper with per-instance knobs \(db_url_env_var, iam_endpoint, recreate_uses_datasource, log_prefix\) so writer and reader wrappers are independent: the reader writes its fresh URL to DATABASE_URL_READ_REPLICA and passes datasource override to Prisma since Prisma only auto-reads DATABASE_URL
- Fix deadlock in PrismaWrapper.__getattr__: when called from inside a running event loop, schedule the token refresh as a background task instead of blocking with run_coroutine_threadsafe + future.result\(\), which would deadlock the loop thread waiting for a coroutine that needs the loop to run
- Fix botocore crash when DATABASE_PORT is unset by defaulting to "5432" in both proxy_cli.py and PrismaWrapper.get_rds_iam_token\(\); passing None caused botocore to embed the literal string "None" in the presigned URL
- Implement graceful reader degradation: reader connect/recreate failures are non-fatal; wrapper sets _reader_unavailable=True and silently routes reads to the writer to keep the proxy serving traffic during transient reader outages
- Add PrismaClient.writer_db property so the reconnect smoke-test always validates the writer engine specifically; query_raw on the routing wrapper would route to the reader and not verify the newly-recreated writer
- Expose DATABASE_URL_READ_REPLICA in Helm chart \(values.yaml + deployment.yaml\) via both plain value and secret key reference, and document the field in docker-compose.yml
- Add 887-line test suite covering routing logic, IAM token refresh paths, reader degradation scenarios, datasource override behavior, and the deadlock regression

Co-authored-by: Yassin Kortam <yassinkortam@g.ucla.edu>")  | May 9, 2026  |  
|   |   | [Add granian as a ASGI compliant web server. Provider better throughpu…](https://github.com/BerriAI/litellm/commit/d04373f4ce80a7b7b3feb71fa64ec6111a678d16 "Add granian as a ASGI compliant web server. Provider better throughput stability, \(#26027\)

* Add granian as a ASGI compliant web server. Provides better stability, 10-20 RPS improvement under standard LT conditions.

TODO: Verify poetry lock details and add locust numbers to PR

* Update granian version in license_cache.json and pyproject.toml to 2.5.7

* Enhance proxy CLI tests by adding SSL initialization checks for Granian server. Remove Python version skip conditions and implement tests to ensure SSL certificate and key are required for server initialization.

* update uv lock to fix granian import error")  | May 22, 2026  |  
|   |   |  [Add ScrapeGraph MCP server configuration (](https://github.com/BerriAI/litellm/commit/c82d74587db29be2b12b560afddfa2958f4d3b9b "Add ScrapeGraph MCP server configuration \(#18923\)")[#18923](https://github.com/BerriAI/litellm/pull/18923)[)](https://github.com/BerriAI/litellm/commit/c82d74587db29be2b12b560afddfa2958f4d3b9b "Add ScrapeGraph MCP server configuration \(#18923\)")  | Jan 11, 2026  |  
| [model_prices_and_context_window.json](https://github.com/BerriAI/litellm/blob/main/model_prices_and_context_window.json "model_prices_and_context_window.json")  | [model_prices_and_context_window.json](https://github.com/BerriAI/litellm/blob/main/model_prices_and_context_window.json "model_prices_and_context_window.json")  |  [refactor(cost-map): remove openrouter rows, part 5 of 5 (](https://github.com/BerriAI/litellm/commit/3ea80696246b79535386ee53c710e103a6b08dc0 "refactor\(cost-map\): remove openrouter rows, part 5 of 5 \(#45669\)

* refactor\(cost-map\): remove openrouter rows, part 1 of 5 \(anthropic to google block\)

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 2 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 3 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 4 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 5 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

---------

Co-authored-by: kerry <kerry@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")[#45669](https://github.com/BerriAI/litellm/pull/45669)[)](https://github.com/BerriAI/litellm/commit/3ea80696246b79535386ee53c710e103a6b08dc0 "refactor\(cost-map\): remove openrouter rows, part 5 of 5 \(#45669\)

* refactor\(cost-map\): remove openrouter rows, part 1 of 5 \(anthropic to google block\)

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 2 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 3 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 4 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 5 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

---------

Co-authored-by: kerry <kerry@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Oct 9, 2026  |  
| [model_prices_and_context_window.schema.json](https://github.com/BerriAI/litellm/blob/main/model_prices_and_context_window.schema.json "model_prices_and_context_window.schema.json")  | [model_prices_and_context_window.schema.json](https://github.com/BerriAI/litellm/blob/main/model_prices_and_context_window.schema.json "model_prices_and_context_window.schema.json")  |  [refactor(cost-map): remove openrouter rows, part 3 of 5 (](https://github.com/BerriAI/litellm/commit/ed22fd929c9f3ebdfff274b95672204c9dc7377a "refactor\(cost-map\): remove openrouter rows, part 3 of 5 \(#45667\)

* refactor\(cost-map\): remove openrouter rows, part 1 of 5 \(anthropic to google block\)

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 2 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 3 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

---------

Co-authored-by: kerry <kerry@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")[#45667](https://github.com/BerriAI/litellm/pull/45667)[)](https://github.com/BerriAI/litellm/commit/ed22fd929c9f3ebdfff274b95672204c9dc7377a "refactor\(cost-map\): remove openrouter rows, part 3 of 5 \(#45667\)

* refactor\(cost-map\): remove openrouter rows, part 1 of 5 \(anthropic to google block\)

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 2 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* refactor\(cost-map\): remove openrouter rows, part 3 of 5

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

---------

Co-authored-by: kerry <kerry@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   |  [build(deps): suppress unfixed braces](https://github.com/BerriAI/litellm/commit/4ece6c9fb8186e8a70df2c3cd5807d246b55a415 "build\(deps\): suppress unfixed braces GHSA-vfj7-8cjw-p6xm to clear osv-scan \(#44347\)

Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>") [GHSA-vfj7-8cjw-p6xm](https://github.com/advisories/GHSA-vfj7-8cjw-p6xm "GHSA-vfj7-8cjw-p6xm") [to clear osv…](https://github.com/BerriAI/litellm/commit/4ece6c9fb8186e8a70df2c3cd5807d246b55a415 "build\(deps\): suppress unfixed braces GHSA-vfj7-8cjw-p6xm to clear osv-scan \(#44347\)

Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Oct 3, 2026  |  
|   |   |  [[Infra] Promote internal staging to main (](https://github.com/BerriAI/litellm/commit/6ff668c7aa01a73738ed39aa64913a089a183565 "\[Infra\] Promote internal staging to main \(#27245\)

* default requested_model to empty string on litellm-side rejects

* Update litellm/router.py

Co-authored-by: greptile-apps\[bot\] <165735046+greptile-apps\[bot\]@users.noreply.github.com>

* fix: scope key access_group_ids override by team's assigned groups

A team member could set any access_group_ids on their key \(e.g. a group
assigned only to a different team\) and override the team's model
restriction. Intersect the key's access_group_ids with team_object.access_group_ids
in _key_access_group_grants_model so foreign groups are dropped before
model expansion. Adds a regression test that asserts expansion is never
called for foreign groups.

* \[Fix\] Proxy: Skip Personal Budget Hook When Reservation Covers Counter

The reservation path \(PR #26845\) atomically pre-fills `spend:user:{user_id}`
and admits at the strict-`<` boundary. The legacy `_PROXY_MaxBudgetLimiter`
pre-call hook re-reads the same counter with `>=`, so a reservation that
fills the counter to exactly `max_budget` \(e.g. a request without a
`max_tokens` cap that falls back to reserving the smallest remaining
headroom\) is rejected by the hook even though the reservation already
admitted it.

Skip the hook when the request's active `budget_reservation` covers
`spend:user:{user_id}`. The reservation is the source of truth for that
counter cross-pod; the legacy `>=` path remains in place for requests
without a reservation \(e.g. paths that bypass the reservation entirely\).

Reproduces as `tests/otel_tests/test_prometheus.py::test_user_budget_metrics`
on a fresh user with `max_budget=10` calling `fake-openai-endpoint` without
`max_tokens`. Adds focused unit coverage in
`tests/test_litellm/proxy/hooks/test_max_budget_limiter.py`.

* harden bedrock file bucket validation

* Fix syntax errors from botched merge in router.py

* Fix Vertex batch output edge cases

* \[Fix\] RBAC: Drop management_routes Write Fallback for Admin Viewer

Greptile P1: the unsafe-method branch of `_check_proxy_admin_viewer_access`
ended with a blanket `if route in management_routes: return`. That set is a
mix of reads \(info/list — handled via the safe-method GET branch above\) and
writes. The fallback let Admin Viewer POST to write endpoints not enumerated
in `_ADMIN_VIEWER_BLOCKED_WRITE_ROUTES`, including:
  - /team/block, /team/unblock, /team/permissions_update
  - /jwt/key/mapping/{new,update,delete}
  - /key/bulk_update
  - /key/{key_id}/reset_spend

Remove the fallback. The two remaining allow sets \(admin_viewer_routes and
global_spend_tracking_routes\) are both read-only, so removal does not affect
the legitimate POST-as-read cases \(e.g. /spend/calculate, which is in
spend_tracking_routes ⊂ admin_viewer_routes\).

Tests:
  - 8 new parametrized cases pinning each previously-leaking management write
    endpoint to 403 on POST for PROXY_ADMIN_VIEW_ONLY.

* fix\(tests\): anchor VCR redis cassette key to repo root

`os.path.relpath` with no `start` arg uses the current working
directory, so running pytest from a subdirectory produced a
different Redis key than running from the repo root. CI-recorded
cassettes and locally-replayed runs would silently miss each
other's cache.

Anchor the path to the repo root \(derived from `__file__`\) so the
key is stable regardless of CWD.

https://claude.ai/code/session_018uCx7pcrkdUJZrCVMaTdPx

* fix: gate key access_group override on group's own assignment

Replaces the previous intersect-with-team.access_group_ids check, which
made the override unreachable in practice \(the team-gate fallback already
covered every case the intersection allowed\). The override now resolves
each of the key's access_group_ids via get_access_object and accepts the
group only if its assigned_team_ids includes the key's team_id, or its
assigned_key_ids includes the key's token. This fulfills the original ask
\(a key can extend a team's allow-list via a group the admin granted to
that team or that specific key\) while still rejecting foreign groups
referenced by team members of other teams.

* \[Fix\] Proxy/Key Management: Honor team_member_permissions /key/list In /key/list Endpoint

When a team grants /key/list via team_member_permissions, non-admin members
should see all keys for that team — same as a team admin. Previously the
classification in list_keys\(\) only checked admin status, so permitted
members fell into the service-account-only path and could not see other
members' personal keys. Routes those members into the full-visibility set.

* Fix access-group bypass via litellm-model fallback path

When _get_all_deployments returns 0 candidates and the litellm-model
fallback branch \(_get_deployment_by_litellm_model\) finds deployments that
the access-group filter then empties, _access_group_filter_emptied_candidates
remained False \(it was captured before that branch ran\). The router would
then proceed to default fallbacks; the fallback model could have no
access_groups and short-circuit the filter, silently serving a caller
blocked by access-group restrictions.

Update the flag inside the litellm-model branch when filtering empties a
non-empty candidate set so the default-fallback guard still triggers.

* fix\(proxy\): redact MCP server URL and headers for non-admin viewers \(VERIA-8\)

Many MCP integrations \(Zapier, etc.\) embed an upstream API key
directly in the server URL, e.g.
``https://actions.zapier.com/mcp/<api-key>/sse``. The list and
single-server endpoints were returning the full URL to any
authenticated user — `_redact_mcp_credentials` only stripped the
explicit ``credentials`` field, and `_sanitize_mcp_server_for_virtual_key`
only ran for restricted virtual keys. Non-admin internal users could
read the dashboard, click the unmask toggle, and exfiltrate the raw
token.

Add `_sanitize_mcp_server_for_non_admin` that runs on top of the
existing credential redaction and clears the credential-bearing
fields:

- ``url`` \(the primary leak vector\)
- ``spec_path`` \(OpenAPI spec URLs that may carry tokens\)
- ``static_headers`` / ``extra_headers`` \(Authorization\)
- ``env`` \(arbitrary secrets\)
- ``authorization_url`` / ``token_url`` / ``registration_url``

Identity fields \(``server_id``, ``alias``, ``mcp_info``, etc.\) are
preserved so the UI can still list servers a non-admin's team has
access to.

Apply the new sanitizer in `fetch_all_mcp_servers` and the per-server
fetch path right after the existing virtual-key branch. Update the
existing `test_list_mcp_servers_non_admin_user_filtered` assertions
that previously checked URL visibility.

Frontend defense-in-depth: hide the URL unmask toggle on
`mcp_server_view.tsx` unless the viewer is a proxy admin.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* Fix runtime policy attachment initialization

Mark runtime-created policies and attachments initialized so global policy attachments created from the policy builder apply immediately without requiring a restart.

Co-authored-by: Cursor <cursoragent@cursor.com>

* test\(router\): cover _try_early_resolve_deployments_for_model_not_in_names

The router_code_coverage CI check requires every function in router.py to
be referenced by at least one test under tests/{local_testing,
router_unit_tests,test_litellm} in a file with "router" in its name.
The recently-extracted helper had no direct test, so the check failed
with "0.45% of functions in router.py are not tested".

Add a focused test that exercises the four return paths: model already
in self.model_names, no fallback applies, pattern-router match, and
default_deployment substitution \(also asserting the stored default
isn't mutated\).

https://claude.ai/code/session_019AVp1XL7RT9RxRe4qRLkay

* Fix policy registry teardown in tests

Reset the policy ID index during policy engine test cleanup so stale policy versions cannot leak between tests.

Co-authored-by: Cursor <cursoragent@cursor.com>

* fix\(batches\): count non-chat tokens, validate batch-file model access \(VERIA-39\) \(#27015\)

* fix\(batches\): count non-chat tokens and validate every model in batch file

Two security control bypasses on POST /v1/batches:

1. `_get_batch_job_input_file_usage` only summed tokens for
   `body.messages` \(chat completions\). Embedding \(`input`\) and text
   completion \(`prompt`\) batches reported zero, letting massive
   non-chat workloads slip past TPM rate limits. Extend the counter
   to handle string and list shapes for both fields.

2. The batch input file was forwarded to the upstream provider
   without inspecting the models named inside the JSONL — only the
   outer `model` query parameter was checked against the caller's
   allowlist. A caller restricted to gpt-3.5 could submit a batch
   targeting gpt-4o and the upstream would execute it under the
   proxy's shared API key.

Add `_get_models_from_batch_input_file_content` \(returns the
distinct `body.model` values\) and call it from
`_enforce_batch_file_model_access` in the pre-call hook, which runs
each model through `can_key_call_model` so the same allowlist
semantics \(wildcards, access groups, all-proxy-models, team aliases\)
the proxy enforces on `/chat/completions` apply here too. Any
unauthorized model raises a 403 before the file is forwarded.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(batches\): count pre-tokenized prompt/input shapes, classify 403 logs

Two follow-ups from the Greptile review on the batch validation PR:

1. P1 TPM bypass via integer token arrays. The OpenAI batch schema
   accepts ``prompt`` and ``input`` as ``list\[int\]`` \(a single
   pre-tokenized prompt\) or ``list\[list\[int\]\]`` \(multiple\) in addition
   to the string and ``list\[str\]`` shapes. Pre-fix only the string
   shapes were counted, so a caller could submit a batch with hundreds
   of millions of pre-tokenized tokens and the rate limiter would
   record zero. Extract the per-field logic into
   ``_count_prompt_or_input_tokens`` and count each int as one token.

2. P2 access-denial logs were indistinguishable from I/O failures.
   ``count_input_file_usage`` caught every exception under a generic
   "Error counting input file usage" message, so an intentional 403
   from ``_enforce_batch_file_model_access`` looked the same in the
   logs as a missing file or a Prisma timeout. Catch ``HTTPException``
   separately and log 403s at WARNING level with a security-relevant
   message before re-raising.

Tests cover the new shapes: single ``list\[int\]``, ``list\[list\[int\]\]``
\(the worst-case bypass vector\), and embeddings ``input`` with
pre-tokenized arrays.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

---------

Co-authored-by: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(proxy\): re-validate user_id after /user/info re-parses query \(#27009\)

* fix\(proxy\): re-validate user_id ownership after /user/info re-parses query

The route-level access check in `RouteChecks.non_proxy_admin_allowed_routes_check`
reads `request.query_params.get\("user_id"\)`, which decodes literal `+` to
spaces. The endpoint then re-parses the raw query string with `urllib.unquote`
in `get_user_id_from_request` to preserve `+` characters \(so plus-addressed
emails work as user_ids\). Those two paths produce different ids: a caller
who registered a user_id containing a literal space could pass the route
check and then read another user's row by sending the encoded `+` form.

Add `_enforce_user_info_access` and call it after `_normalize_user_info_user_id`
returns the final id. Proxy admin / view-only admin still bypass; everyone
else must match the resolved user_id \(or have no user_id, which falls back
to the caller's own id later in the handler\).

Tests cover the admin bypass, owner-match path, and the cross-user lookup
that this change blocks.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(proxy\): apply user_info ownership check to PROXY_ADMIN_VIEW_ONLY

`_enforce_user_info_access` was bypassing both PROXY_ADMIN and
PROXY_ADMIN_VIEW_ONLY, but the upstream route check in
`RouteChecks.non_proxy_admin_allowed_routes_check` only treats
PROXY_ADMIN as a true admin for the `/user/info` route — view-only
admins go through the `user_id == valid_token.user_id` enforcement
along with regular users. Mirroring that asymmetry left the same
encoded-`+` bypass open for view-only admins whose user_id contains a
literal space.

Drop the PROXY_ADMIN_VIEW_ONLY exemption so the post-decode re-check
matches the upstream rule. Update tests: a view-only admin must now
be blocked from cross-user lookups but still allowed to read their
own row.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

---------

Co-authored-by: yuneng-jiang <yuneng@berri.ai>
Co-authored-by: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* feat\(spend-logs\): opt-in suppression of stack traces in spend-tracking error logs

Adds LITELLM_SUPPRESS_SPEND_LOG_TRACEBACKS env var. When set to true and the
proxy log level is INFO or above, spend-tracking error paths emit a single
ERROR line without the full traceback. Stack traces are preserved at DEBUG
and the Sentry / proxy_logging_obj.failure_handler path is unchanged.

The new spend_log_error helper is wired through the spend write hot path:
  - DBSpendUpdateWriter \(update_database, _update_*_db, batch upsert,
    redis-commit fallbacks\)
  - _ProxyDBLogger._PROXY_track_cost_callback
  - get_logging_payload exception path
  - update_spend / update_daily_tag_spend / spend logs queue monitor

Resolves LIT-2704.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(spend-logs\): preserve no-traceback behavior for update_daily_tag_spend

This call site previously logged a single-line error via verbose_proxy_logger.error\(\)
with no traceback. Switching it to spend_log_error\(..., exc=e\) caused a full stack
trace to render by default \(when LITELLM_SUPPRESS_SPEND_LOG_TRACEBACKS is unset\),
which contradicts the PR goal of leaving default behavior unchanged. Revert this
specific site to the original error log call.

* fix\(spend-logs\): preserve no-traceback behavior for update_daily_tag_spend

Bugbot caught a regression: the previous error log here was a single-line
verbose_proxy_logger.error\(...\) with no traceback. spend_log_error attaches
the active exception's traceback by default \(when the suppression env var
is unset\), so swapping it in changed default behavior. Revert this one site
to its original .error\(\) call to keep the PR strictly opt-in.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* feat\(spend-logs\): suppress traceback in SpendLogs error_information row

Extend LITELLM_SUPPRESS_SPEND_LOG_TRACEBACKS to the failure callback so the
per-row Metadata pane in the UI no longer shows the stack trace when the
opt-in env var is set, matching the existing console-side suppression.

https://claude.ai/code/session_014dztoRbRnRvq54HL9EyHx6

* \[Fix\] Proxy: Repair Merge Fallout In Router-Override Fallback Auth

Conflict resolution for #26968 dropped the `Iterator` typing import
\(NameError at module load\), left a dead `fallback_models = cast\(...\)`
block, and the new tests called `_enforce_key_and_fallback_model_access`
without the now-required `request` kwarg.

* isolate dual OTEL handlers

* harden cloud file compatibility path

* harden cloud file compatibility path

* \[Fix\] Proxy/Key Management: Align Key-Org Membership Checks On Generate And Regenerate

Mirrors the membership rule on /key/update so that /key/generate and
/key/{key}/regenerate apply the same `_validate_caller_can_assign_key_org`
gate when the caller specifies an `organization_id`. Proxy admins bypass.
The check no-ops when `organization_id` is not being set.

* thread trusted params through vertex file content

* trust only server legacy file flag

* chore\(proxy\): keep public AI hub unauthenticated

* fix\(proxy\): preserve low-detail readiness status

* \[Test\] Anthropic: Replace Legacy Claude-4-Sonnet Alias With Haiku 4.5

Three live-API tests pinned to claude-4-sonnet-20250514, which is a
non-canonical alias of claude-sonnet-4-20250514. Anthropic's main API
no longer resolves the legacy form under freshly issued keys, so the
tests fail with not_found_error. The token counter test pinned to
claude-sonnet-4-20250514 itself \(deprecation_date 2026-05-14, two weeks
out\) was on borrowed time too.

Bump all four to claude-haiku-4-5-20251001 — capability superset for what
these tests exercise \(streaming, parallel tool calling, extended thinking,
token counting\), no upcoming deprecation, cheaper per-token.

* chore\(proxy\): move URL-valued model/file_id guard from SDK to proxy

The previous per-provider guards in HuggingFace, Oobabooga, and Gemini
files lived in the SDK layer, breaking SDK callers who legitimately pass
URL-valued model identifiers. Move the check to the proxy boundary in
add_litellm_data_to_request so SDK users keep working while proxy users
default-deny URL-valued model and file_id, with admin opt-in via
litellm.provider_url_destination_allowed_hosts.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* \[Chore\] Proxy/UI: Drop stray _experimental/out/chat/index.html

This file is a regenerable UI build artifact that should not be tracked
in source. Removing so the merge into litellm_internal_staging stays clean.

* \[Test\] Anthropic Passthrough: Bump Streaming Cost-Injection Test To Haiku 4.5

test_anthropic_messages_streaming_cost_injection hits the proxy's
/v1/messages route, which routes via the anthropic/* wildcard to
api.anthropic.com. The 404 surfaced in the test was Anthropic's own
not_found_error propagated back through the proxy \(visible from the
x-litellm-model-id hash on the response — the proxy did route\).

Same root cause as the prior commit: the legacy claude-4-sonnet-20250514
alias is no longer recognized by Anthropic's main API under the new key.
Swap to claude-haiku-4-5-20251001 — same routing path, canonical model.

* fix\(proxy\): handle ownership-recording failures after upstream create

If record_container_owner raises after the upstream container is created,
the user previously got a 500 with no usable container — they were billed
for an unreachable resource. Move ownership recording into the create
path's exception handling and split the two failure modes:

- HTTPException from the recorder \(auth conflicts\) propagates verbatim
  so the client sees the real status code, not a generic LLM error.
- Unexpected exceptions are logged and swallowed; the response is
  returned to the caller so they aren't billed for a container they
  can't address. The DB row stays untracked until an operator reconciles.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(guardrails\): close post-call coverage gaps

* fix\(types\): add /team/permissions_bulk_update to management_routes

The blocklist check in _check_proxy_admin_viewer_access only fires for
routes that match LiteLLMRoutes.management_routes — the bulk-update
endpoint was missing from that list, so the test for view-only admins
on /team/permissions_bulk_update fell through to "allow."

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* \[Test\] Anthropic Passthrough: Bump Thinking Tests Off Legacy Sonnet 4 Alias

base_anthropic_messages_test.test_anthropic_messages_with_thinking and
test_anthropic_streaming_with_thinking still pinned to
claude-4-sonnet-20250514 — the same legacy alias Anthropic no longer
recognizes under freshly issued keys. The other four tests in this base
class already use claude-sonnet-4-5-20250929; these two were missed.

Bump to claude-haiku-4-5-20251001 \(supports_reasoning=true, no upcoming
deprecation\). Subclasses including TestAnthropicPassthroughBasic
inherit these methods.

* fix\(guardrails\): cover multi-choice output variants

* fix\(proxy\): preserve public ai hub ui setting

* fix\(scim\): cascade FK cleanup on user delete and surface block status in UI

SCIM DELETE /Users/{id} previously called litellm_usertable.delete without
clearing rows that FK back to the user, so Postgres rejected the delete with
LiteLLM_InvitationLink_user_id_fkey and the SCIM caller saw a 500. Add a
helper to drop invitation_link, organization_membership, and team_membership
rows before the user delete \(mirrors /user/delete in internal_user_endpoints\).

Also add a Status column to the Virtual Keys and Internal Users tables so
admins can see at a glance which keys are blocked and which users SCIM has
deactivated. SCIM-blocked keys carry a tooltip explaining the origin.

Pin the dashboard's Node version to 20 via .nvmrc to match CI.

* chore: update Next.js build artifacts \(2026-05-02 03:21 UTC, node v20.20.2\)

* perf\(proxy\): cache container/skill ownership reads on the hot path

Container ownership and skill rows are looked up on every retrieve /
delete / list / file-content / chat-completion-with-skill call. The new
stores wrapped raw Prisma queries with no cache, putting one DB
round-trip on each request. Add an in-process TTL'd cache mirroring the
_byok_cred_cache pattern in mcp_server/server.py: per-key \(value,
monotonic_timestamp\), 60s TTL, 10000-entry cap with full-clear on
overflow, invalidated by every write. Negative results \(`None`\) are
cached too so untracked-resource checks also skip the DB.

Tests cover: cache-after-first-hit, negative caching, write
invalidation, no-caching-on-DB-error, TTL expiry, capacity eviction.
56 tests pass.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* chore: update Next.js build artifacts \(2026-05-02 03:39 UTC, node v20.20.2\)

* fix: remove traceback key instead of it being ""

* fix: linting error

* fix\(scim\): preserve scim_active on PUT when client omits the field

A SCIM PUT may legally omit `active` \(full-replace with the field
absent\). Pydantic fills the SCIMUser.active default of True, so the PUT
handler was overwriting metadata.scim_active with True even when the
client never sent it — silently reactivating a previously SCIM-blocked
user and unblocking their keys.

Use model_fields_set to detect whether the client actually sent
`active`. If omitted, preserve the prior scim_active value and skip
the cascade to virtual keys.

Also drop comments added in this PR that just narrate what the code
does; keep only the docstrings and the SQL-NULL pitfall note that
explain non-obvious behaviour.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(proxy\): use set lookup for permitted agent filters

* fix\(mcp\): redact command fields for non-admin server views

* fix\(proxy\): forward decoded container ids after ownership checks

* fix\(caching\): handle stale isolated Redis semantic index

* fix\(cloudflare\): support response_text in streaming chunk parser

Newer Cloudflare Workers AI models \(e.g. Nemotron\) emit 'response_text'
instead of 'response' on streamed chunks. The non-streaming path was
already updated to fall back to 'response_text' \(#26385\), but the
streaming chunk parser still only read 'response', which caused
streaming requests against those models to silently produce empty
content.

Mirror the non-streaming fallback in CloudflareChatResponseIterator.chunk_parser
and add a streaming test for the response_text shape.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* Fix code qa

* Address bugbot: drop dead encode/decode helpers; preserve empty custom_id

- Remove unused _encode_gcp_label_value / _decode_gcp_label_value singular
  helpers; only the _chunks variants are actually called.
- Use 'is not None' check for custom_id so empty-string custom_ids are
  still labeled and round-trip through batch outputs.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* Forward Vertex file content logging context

* test vertex file content logging forwarding

Co-authored-by: Sameer Kankute <Sameerlite@users.noreply.github.com>

* Fix Vertex batch output logging mutation

* fix: don't mutate caller's logging_obj in _try_transform_vertex_batch_output_to_openai

The method was overwriting logging_obj.optional_params, logging_obj.model,
and logging_obj.start_time on the caller's Logging instance. When invoked
from llm_http_handler.py's generic framework path, the framework's own
logging_obj \(which already went through pre_call\) had its properties
clobbered, causing model and start_time to reflect the last batch line's
values rather than the original call context.

Fix: create a fresh local Logging instance for the per-line transformation
instead of mutating the incoming logging_obj. The caller's object is now
left entirely untouched regardless of whether a logging_obj was passed in
or not.

Regression tests added to verify model, start_time, and optional_params
are not mutated on the caller's logging_obj.

Co-authored-by: Sameer Kankute <Sameerlite@users.noreply.github.com>

* feat: add opt-out flag for Vertex batch output transformation

Adds litellm.disable_vertex_batch_output_transformation \(default False\).
When True, afile_content returns raw Vertex predictions.jsonl untouched
so users that parse candidates/modelVersion directly are not broken.

* fix\(anthropic,bedrock\): omit thinking/output_config when reasoning_effort="none"

Setting reasoning_effort="none" on Anthropic chat models \(direct, Bedrock
Invoke, Bedrock Converse, Vertex AI Anthropic, Azure AI Anthropic\) crashed
LiteLLM with:

  litellm.APIConnectionError: 'NoneType' object has no attribute 'get'

Both the Anthropic chat transformation and Bedrock Converse called
``AnthropicConfig._map_reasoning_effort`` and assigned the ``None`` it returns
for ``"none"`` directly to ``optional_params\["thinking"\]``. Downstream
``is_thinking_enabled`` then did ``optional_params\["thinking"\].get\("type"\)``
and crashed.

Pop ``thinking`` \(and on Claude 4.6/4.7, ``output_config``\) instead of
assigning ``None``, restoring the documented contract that
``reasoning_effort="none"`` means "do not enable thinking". This also
prevents downstream Anthropic 400s \("thinking: Input should be an object",
"output_config.effort: Input should be ..."\) if the bug were ever masked.

Verified end-to-end against the live Anthropic API and Bedrock Converse
on claude-opus-4-{5,6,7} and claude-sonnet-4-6, plus Bedrock Invoke for
Claude 4.5/4.6. Vertex AI Anthropic and Azure AI Anthropic inherit the
fixed ``map_openai_params`` from ``AnthropicConfig`` and need no further
changes.

* fix\(vertex-ai\): set response=null on batch error entries per OpenAI spec

The Vertex batch output transformer was emitting both a populated 'response' and 'error' for failed batch entries. The OpenAI Batch output spec defines them as mutually exclusive: on error 'response' MUST be null. This broke any consumer using 'result\["response"\] is None' to detect failures.

* test\(vertex-ai\): cover transformation_error path emits response=null

* fix\(security\): sandbox jinja2 in gitlab/arize/bitbucket prompt managers

DotpromptManager was hardened to render through
ImmutableSandboxedEnvironment. The three sibling managers \(gitlab,
arize, bitbucket\) were missed and still instantiate plain
jinja2.Environment\(\), leaving the same attribute-traversal SSTI
primitive open: a template fetched from a GitLab/BitBucket repo or
Arize Phoenix workspace can reach __class__.__init__.__globals__ and
execute arbitrary Python on the proxy host.

Match the dotprompt pattern by switching all three to
ImmutableSandboxedEnvironment. The sandbox blocks the dunder-traversal
chain while leaving normal {{ var }} substitution intact, so the
template surface is unchanged for legitimate use.

Adds tests/test_litellm/integrations/test_prompt_manager_ssti.py
\(18 cases\) verifying each manager's jinja_env is a sandbox, that
classic SSTI payloads raise SecurityError, and that ordinary variable
rendering still works.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* chore\(proxy\): drop client-supplied pricing fields from request bodies

The proxy currently forwards request-body pricing parameters \(the fields
on `CustomPricingLiteLLMParams`, plus `metadata.model_info`\) into the
core call path. Those fields belong to deployment configuration, not to
per-request input — sending them from a client mutates the request's
recorded cost and, via `litellm.completion` → `register_model`, the
process-wide `litellm.model_cost` map for every later caller in the
worker. Strip them at the boundary.

The strip set is built from `CustomPricingLiteLLMParams.model_fields` so
pricing fields added later are covered automatically. Operators who do
want clients to supply per-request pricing can opt back in per key or
team via `metadata.allow_client_pricing_override = true`, mirroring the
existing `allow_client_mock_response` and
`allow_client_message_redaction_opt_out` flags.

Tests cover the strip set's coverage, root and metadata strips, the
opt-in skip on both key and team metadata, and a regression check that
the global `litellm.model_cost` map is unmutated after a stripped
request.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* chore\(proxy\): log stripped pricing fields at debug for operator visibility

Operators upgrading would otherwise see client-supplied pricing overrides
silently stop applying with no diagnostic. Emit a debug-level line listing
the dropped fields and pointing at the opt-in flag when any are stripped;
stay silent on the no-op path so the log isn't filled with noise.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(proxy\): move pricing strip below the litellm_metadata JSON-string parse

The strip ran before the proxy parses ``litellm_metadata`` from a JSON
string into a dict \(a path used by multipart/form-data and ``extra_body``
callers\), so ``isinstance\(metadata, dict\)`` was False and ``model_info``
survived the strip. Move the call to the same post-parse position the
``user_api_key_*`` strip already uses for the same reason. Adds a
regression test exercising the JSON-string ``litellm_metadata`` path.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* test\(responses\): replace legacy claude-4-sonnet alias in multiturn tool-call test

Anthropic's main API no longer resolves the non-canonical 'claude-4-sonnet-20250514'
alias for freshly issued keys, returning 404 not_found_error. PR #27031 already
swept three other live tests pinned to this alias to claude-haiku-4-5-20251001
but missed test_multiturn_tool_calls in the responses API suite, which is now
failing reliably on PR CI runs \(e.g. PR #27074, job 1603363\).

Bump the two model references in test_multiturn_tool_calls to the same
claude-haiku-4-5-20251001 snapshot used by PR #27031 -- it covers everything
this test exercises \(tool calling, multi-turn\) and isn't on a deprecation
schedule.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* chore\(proxy\): close callback-config and observability-credential side channels

Two related gaps in the proxy's request bouncer:

1. ``is_request_body_safe`` \(auth_utils.py\) walked the request-body root
   and the ``litellm_embedding_config`` nested dict, but not ``metadata``
   or ``litellm_metadata``. The same fields it bans at root — Langfuse /
   Langsmith / Arize / PostHog / Braintrust / Phoenix / W&B Weave / GCS /
   Humanloop / Lunary credentials and routing — were silently accepted
   when the caller put them inside metadata, retargeting observability
   callbacks to a caller-controlled host with caller-supplied creds.
   Walk both metadata containers \(and parse the JSON-string form sent via
   multipart / ``extra_body``\) through the same banned-params helper, so
   the existing ``allow_client_side_credentials`` opt-in covers both
   paths consistently.

2. The banned-params list was hand-maintained and lagged the canonical
   ``_supported_callback_params`` allow-list in
   ``initialize_dynamic_callback_params``. Derive the observability bans
   from that allow-list \(minus a small ``_SAFE_CLIENT_CALLBACK_PARAMS``
   set for informational fields like ``langfuse_prompt_version`` and
   ``langsmith_sampling_rate``\) so future integrations are covered
   automatically; ``_EXTRA_BANNED_OBSERVABILITY_PARAMS`` carries the
   handful of fields integrations read but the allow-list hasn't caught
   up to. A guard test fails CI if a new entry is added to
   ``_supported_callback_params`` without an explicit safe-list decision.

Separately in ``litellm_pre_call_utils.py``: add ``callbacks``,
``service_callback``, ``logger_fn``, and ``litellm_disabled_callbacks``
to ``_UNTRUSTED_ROOT_CONTROL_FIELDS``. The first three are appended to
worker-wide ``litellm.{input,success,failure,_async_*,service}_callback``
lists / ``litellm.user_logger_fn`` from inside ``function_setup`` — one
request poisons every subsequent caller in that worker. The last is the
inverse primitive: the legitimate path reads it from key/team metadata,
the request-body version silently disables admin-configured audit /
observability for the call.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(auth\): per-param allow must continue, not return early

A pre-existing logic bug in ``_check_banned_params``: when the
deployment-level ``configurable_clientside_auth_params`` permitted one
banned field, the loop ``return``-ed on the first match instead of
``continue``-ing, so any other banned param later in the same body or
metadata dict was never checked. This PR's metadata walk multiplies the
surface where that bypass matters — a body pairing an allowed
``api_base`` with an observability credential like ``langfuse_host``
would silently pass.

Proxy-wide ``allow_client_side_credentials`` keeps ``return`` \(it's a
global opt-in for every banned param\). The per-param branch becomes
``continue`` so only the one explicitly-permitted field is skipped.

Adds a regression test that exercises the api_base + langfuse_host pair.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(vector_store\): resolve embedding config at request time, never persist creds

The vector store create/update path previously called
``_resolve_embedding_config`` against the admin-configured router/DB
model and persisted the resolved ``litellm_embedding_config`` dict
\(``api_key`` / ``api_base`` / ``api_version``\) into the
``litellm_managedvectorstorestable.litellm_params`` column. Because the
resolver expanded ``os.environ/...`` references via ``get_secret``, the
DB row carried cleartext provider credentials, and the
``/vector_store/{new,info,update,list}`` responses returned them to any
authenticated caller who could supply a known admin model name.

Move the auto-resolve out of ``create_vector_store_in_db`` and out of
the update path. Persist only the user-supplied ``litellm_embedding_model``
reference. Resolve at request-handling time inside
``_update_request_data_with_litellm_managed_vector_store_registry`` so
the resolved config lives in the per-request ``data`` dict and is
garbage-collected after the response. Legacy rows that were created by
an earlier proxy version and already carry a resolved
``litellm_embedding_config`` skip the re-resolution and pass through
unchanged so embedding calls keep working.

The ``new_vector_store`` response now also runs the existing
``_redact_sensitive_litellm_params`` masker \(already used by ``info``,
``update``, and ``list``\), defending against caller-supplied cleartext
on the create path and against legacy rows whose persisted credentials
are still in the database.

Existing tests that asserted the old write-time-resolve behaviour are
updated to assert the new persistence shape \(no embedding config
stored, just the model reference\). Two new tests cover the use-time
path: one asserting fresh resolution happens when a row carries only
the model reference, the other asserting legacy rows with persisted
config skip re-resolution and continue to work.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(vector_store\): tighten registry-mutation comment and dedupe test helpers

* fix\(vector_store\): cache use-time embedding-config resolution

Hold the resolved config in a process-memory TTL cache so the
request-handling path doesn't run litellm_proxymodeltable.find_first
on every vector-store call.

* fix\(anthropic,bedrock,vertex\): forward output_config.effort + 400 on garbage reasoning_effort

Follow-up bugs surfaced by the QA sweep on PR #27039
\(https://github.com/BerriAI/litellm/pull/27039#issuecomment-4363363610\).

1. Stop stripping output_config.effort on Bedrock + Vertex adaptive routes.
   - Vertex AI Claude 4.6/4.7 accepts output_config.effort on rawPredict
     \(verified end-to-end against us-east5 / global\). The strip helper now
     no-ops for effort.
   - Bedrock Converse routes output_config into additionalModelRequestFields
     for anthropic base models so the requested adaptive tier \(low/medium/
     high/xhigh/max\) actually reaches the wire instead of all collapsing to
     identical thinking.
   - Bedrock Invoke chat transformation \(AmazonAnthropicClaudeConfig\) stops
     popping output_config from the post-AnthropicConfig request body.
   - Bedrock Invoke /v1/messages allowlist \(BedrockInvokeAnthropicMessagesRequest\)
     now lists output_config so the runtime allowlist filter forwards it.

2. Validate effort across Bedrock Converse so 'disabled' / 'invalid' / '' /
   unsupported tiers \(xhigh/max on Sonnet 4.6 or budget-mode 4.5 models\)
   surface as a clean 400 BadRequestError instead of 500.

3. ValueError -> BadRequestError throughout \(AnthropicConfig.map_openai_params,
   _apply_output_config, AmazonConverseConfig._handle_reasoning_effort_parameter\).
   Empty-string effort is now rejected \(was silently passing the
   'if effort and ...' short-circuit\).

4. Floor reasoning_effort='minimal' at the Anthropic provider minimum
   \(1024 budget_tokens\) via new ANTHROPIC_MIN_THINKING_BUDGET_TOKENS so it's
   a usable tier on direct Anthropic / Azure AI Anthropic / Vertex AI Anthropic /
   Bedrock Invoke \(all of which 400 below 1024\).

5. model_prices: dedupe duplicate supports_max_reasoning_effort key on
   claude-opus-4-7 / claude-opus-4-7-20260416.

Adds regression tests across all five affected paths; existing tests asserting
the silent-strip behavior were updated to reflect the new pass-through and
clean 400 surfaces.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(constants\): make ANTHROPIC_MIN_THINKING_BUDGET_TOKENS a plain constant

The documentation CI test \(tests/documentation_tests/test_env_keys.py\)
asserts every os.getenv\(\) key in the source has a matching entry in the
litellm-docs config_settings.md table. ANTHROPIC_MIN_THINKING_BUDGET_TOKENS
tracks Anthropic's published wire-protocol minimum \(1024\) — it's not a
user-tunable, so making it env-overridable was wrong anyway. Drop the
os.getenv\(\) wrapper; the value is now a plain literal.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(anthropic,bedrock\): correct effort error message and dedupe effort_map

- Remove 'none' from the Bedrock _validate_anthropic_adaptive_effort error
  message; it was listed as a valid value but rejected by the membership
  check, leaving users in a feedback loop if they tried 'none'.
- Hoist the duplicated reasoning_effort -> output_config.effort mapping
  out of AnthropicConfig.map_openai_params and
  AmazonConverseConfig._handle_reasoning_effort_parameter into a single
  AnthropicConfig.REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT class constant
  so the two routes cannot drift.

* fix\(anthropic\): translate reasoning_effort on /v1/messages route

Closes the remaining QA-sweep gap on PR #27074: Bedrock Invoke
/v1/messages was silently ignoring ``reasoning_effort`` because the
shared param filter only kept native Anthropic keys, so every effort
tier collapsed to the same behavior on the wire \(27/231 cells failing
across opus-4-5 / opus-4-6 / sonnet-4-6\).

Map ``reasoning_effort`` to native Anthropic ``thinking`` /
``output_config.effort`` at the ``AnthropicMessagesConfig`` layer so
all four /v1/messages routes \(direct Anthropic, Azure AI, Vertex AI,
Bedrock Invoke\) inherit the same translation:

- Add ``reasoning_effort`` to ``AnthropicMessagesRequestOptionalParams``
  so the param filter in
  ``AnthropicMessagesRequestUtils.get_requested_anthropic_messages_optional_param``
  no longer drops it before the transformation runs.

- Add ``_translate_reasoning_effort_to_anthropic`` and call it from
  ``transform_anthropic_messages_request``. Mirrors
  ``AnthropicConfig.map_openai_params`` on the chat completion path
  \(re-uses ``_map_reasoning_effort`` and
  ``REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT``\) so the two routes
  cannot drift. Pops ``reasoning_effort`` so it never reaches the wire.

- Caller-supplied native ``thinking`` / ``output_config.effort`` always
  win — same precedence as
  ``_translate_legacy_thinking_for_adaptive_model``.

- Garbage values \(``""``, ``"disabled"``, ``"invalid"``\) raise
  ``AnthropicError\(status_code=400\)`` instead of falling through and
  surfacing as 500s from the provider.

- ``"none"`` clears thinking + output_config so callers can opt out
  per request.

Also restores the non-adaptive-model test coverage on Bedrock Invoke
/v1/messages that the previous commit lost when
``test_bedrock_messages_strips_output_config`` was renamed to the
``forwards`` variant on Opus 4.7.

Adds a new test file
``test_reasoning_effort_translation.py`` covering the translation at
the shared config level \(adaptive + non-adaptive models, none, garbage,
caller precedence\) so all four /v1/messages routes are exercised by a
single suite.

Adds parametrized + behavioral tests on the Bedrock Invoke /v1/messages
suite covering: minimal/low/medium/high/xhigh/max mapping for adaptive
models, thinking-budget mapping for non-adaptive Opus 4.5, ``none``
clears both, garbage raises 400, explicit ``output_config`` wins.

Refs: https://github.com/BerriAI/litellm/pull/27074

* fix\(anthropic,bedrock\): reject unmapped reasoning_effort at mapping site

Both the chat completion path \(AnthropicConfig.map_openai_params\) and the
Bedrock Converse path \(_handle_reasoning_effort_parameter\) used
REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT.get\(value, value\) which falls
back to the raw input on unmapped keys. Combined with _map_reasoning_effort
returning type='adaptive' for any string on Claude 4.6/4.7, garbage values
\(e.g. 'disabled'\) could leak into optional_params\['output_config'\]\['effort'\]
unvalidated if map_openai_params ran without the downstream transform_request
or _validate_anthropic_adaptive_effort check.

Mirror the /v1/messages pattern: use .get\(value\) \(no fallback\) and raise
BadRequestError immediately when the value is unmapped, co-locating
validation with the mapping for defense in depth.

* style: black formatting

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(anthropic\): stop class-attr leak; gate xhigh/max on every route

The reasoning-effort mapping dict was a public class attribute on
AnthropicConfig, so BaseConfig.get_config returned it as a request
parameter and every Anthropic-backed call \(Anthropic / Azure / Vertex /
Bedrock Invoke\) hit a 400 'REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT:
Extra inputs are not permitted' from the provider. Move the mapping
to a module-level constant.

_supports_effort_level only looked the model up under
custom_llm_provider='anthropic', so bedrock-prefixed model ids
\(e.g. bedrock/invoke/us.anthropic.claude-opus-4-7\) returned False
for both 'max' and 'xhigh' even when the underlying model entry has
the flag set. Strip known provider prefixes and retry the lookup
against litellm.model_cost directly so per-model gating works on
every route.

Mirror the per-model xhigh/max gate from
AnthropicConfig._apply_output_config in
AnthropicMessagesConfig._translate_reasoning_effort_to_anthropic so
the /v1/messages route also raises a clean 400 instead of forwarding
the unsupported tier.

* feat\(anthropic,bedrock\): strip output_config under drop_params for non-effort models

When a proxy fronts Claude Code \(which always sends `output_config.effort`\)
at a pre-4.5 Anthropic model — haiku-3, sonnet-3.5, opus-3, sonnet-4 — the
forwarded knob causes a forced 400 the client can't fix. Gating a strip
behind the existing `drop_params` flag lets operators opt into silent
fixup once and stop worrying about per-model param hygiene.

Default \(`drop_params=False`\) still forwards and surfaces the provider's
error, preserving the strict, debuggable contract from #27074.

Per https://platform.claude.com/docs/en/build-with-claude/effort the
supporting set is Opus 4.5+, Sonnet 4.6+, and Mythos Preview; everything
else is dropped \(with a verbose_logger warning so the strip is visible\).
Recognition uses model-name patterns plus a fallback to any
`supports_*_reasoning_effort` flag in the model map for forward
compatibility with new entries.

https://claude.ai/code/session_01WjHq31rvXT6xYNdVmSJvRp

\(cherry picked from commit 1233943e7861ba8a9062f792310ebd401cb03db8\)

* fix\(base_llm\): filter all _-prefixed class attrs from get_config

The drop_params strip work added `AnthropicConfig._EFFORT_SUPPORTING_MODEL_PATTERNS`
as a private class-level lookup tuple. `BaseConfig.get_config\(\)` only
filtered the `__`-prefixed names plus `_abc` / `_is_base_class`, so
`_EFFORT_SUPPORTING_MODEL_PATTERNS` would have leaked into the request
body the same way `REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT` did before
the previous commit.

Generalize the existing `_abc` / `_is_base_class` carve-outs to skip
every `_`-prefixed name. `AmazonConverseConfig.get_config\(\)` overrides
the base method, so apply the same change there.

Also unblocks future internal helpers from accidentally serialising into
the wire body.

* fix\(anthropic\): drive output_config.effort support from model map flags

Replace hardcoded _EFFORT_SUPPORTING_MODEL_PATTERNS with a JSON-backed
check that uses supports_*_reasoning_effort flags from the model map.
Add supports_minimal_reasoning_effort: true to opus-4-5 and mythos-preview
entries \(which previously only carried supports_reasoning\) so the JSON
remains the single source of truth for effort capability.

* fix\(anthropic,bedrock,databricks\): four reasoning_effort follow-ups

- claude-sonnet-4-6 + reasoning_effort=max no longer 400s. Renamed
  _is_opus_4_6_model to _is_claude_4_6_model at three sites and added
  supports_max_reasoning_effort: true to 12 model entries in the JSON
  cost map \(10 sonnet 4.6 ids + OpenRouter opus 4.6/4.7\).
- _map_reasoning_effort now raises BadRequestError\(400\) directly with
  llm_provider, instead of letting Databricks \(and similar callers\)
  surface its raw ValueError as a 500.
- output_config.effort on Opus 4.5 over Bedrock no longer 400s for
  missing effort-2025-11-24 beta. Flipped JSON to "effort-2025-11-24"
  for bedrock + bedrock_converse and added an auto-attach branch in
  _process_tools_and_beta for non-adaptive Anthropic + output_config
  on Converse.
- reasoning_effort=xhigh / =max on legacy budget-mode models
  \(Haiku 4.5, Sonnet 4.5, Opus 4.5\) now map to thinking.budget_tokens
  8192 / 16384 instead of returning 400. Added two constants in
  litellm/constants.py.

Tests updated for all four flips. Validated end-to-end via 306-cell
live proxy matrix \(6 model families x 3 routes x 17 effort cases\),
all pass.

* fix\(databricks\): validate reasoning_effort and set output_config on adaptive Claude

The Databricks path called `AnthropicConfig._map_reasoning_effort` for
Claude models but never validated the effort string nor set
`output_config.effort` for adaptive models \(Claude 4.6/4.7\). Since
`_map_reasoning_effort` returns `type=adaptive` for ANY non-None /
non-"none" string on adaptive models \(including "disabled",
"invalid", ""\), Databricks silently accepted garbage and emitted a
request without an `output_config.effort`, collapsing every adaptive
tier to identical behavior.

Match the Anthropic native, Bedrock Converse, Bedrock Invoke, and
/v1/messages paths: when the resolved `thinking` is non-None on a
4.6/4.7 model, look up the value in
`REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT` and either raise a clean
`BadRequestError` or set `optional_params\["output_config"\]`.

* fix\(azure\): omit model from image generation and image edit deployment requests

Azure OpenAI routes image gen/edit by deployment in the URL; sending the
deployment id in model breaks gpt-image-2 \(invalid_value\). Strip model from
JSON for deployments/.../images/generations and from multipart data for
.../images/edits. Non-deployment URLs \(e.g. Azure AI FLUX\) unchanged.

Fixes #26316.

Co-authored-by: Cursor <cursoragent@cursor.com>

* test\(azure\): exercise image gen JSON filter via HTTP client; dedupe image edit URL

- Image generation tests patch HTTPHandler.post / get_async_httpx_client so
  make_*_azure_httpx_request runs and wire json is asserted on call kwargs.
- Azure image edit: strip model in finalize_image_edit_multipart_data using the
  same URL string the handler passes to POST \(no second get_complete_url in
  transform\). BaseImageEditConfig default finalize is a no-op.

Co-authored-by: Cursor <cursoragent@cursor.com>

* fix\(azure_ai/anthropic\): promote output_config out of extra_body so validation runs

`azure_ai` is registered in `litellm.openai_compatible_providers`, so
`add_provider_specific_params_to_optional_params` \(litellm/utils.py\)
auto-stuffs any non-OpenAI kwarg \(e.g. `output_config={"effort": "..."}`\)
into `optional_params\["extra_body"\]`. `AzureAnthropicConfig.transform_request`
then strips `extra_body` entirely on the way out, silently dropping the
param — and `AnthropicConfig._apply_output_config` never sees it, so
`effort="invalid"` / `effort="xhigh"` on a non-supporting model
quietly reaches the model with default behavior instead of returning a
clean 400 \(as the native `anthropic` provider does\).

Promote the keys back to top-level `optional_params` \(using `setdefault`
so explicit top-level values win\) before delegating to the parent
`AnthropicConfig`. Apply in both `validate_environment` and
`transform_request` so flag detection \(`is_mcp_server_used`, etc.\) and
output-config validation both run.

Surfaced by the QA matrix expansion on PR #27074: 20 cells where Azure
returned 200 while `anthropic` returned 400 — all `output_config` mode
across haiku_4_5, sonnet_4_5, opus_4_5, sonnet_4_6, opus_4_6, opus_4_7
families with `effort` in {invalid, xhigh, max, low, medium, high}.

Tests:
* `test_output_config_promoted_from_extra_body`: valid effort reaches data
* `test_invalid_output_config_effort_raises_via_extra_body`: 400 on bad effort
* `test_unsupported_effort_xhigh_raises_via_extra_body`: 400 on xhigh-on-Sonnet-4.6
* `test_extra_body_promotion_does_not_clobber_top_level`: setdefault semantics

* test\(image_gen\): expect no model in Azure image edit multipart \(#26316\)

Align test_azure_image_edit_litellm_sdk with deployment-scoped Azure edits.

Co-authored-by: Cursor <cursoragent@cursor.com>

* refactor\(anthropic\): extract _validate_effort_for_model to prevent drift

The chat completion path \(`_apply_output_config`\) and the /v1/messages
pass-through \(`AnthropicMessagesConfig._translate_reasoning_effort_to_anthropic`\)
both gate `max` / `xhigh` per model. The two sites had diverged from
near-identical copies into separately maintained blocks, creating a real
drift risk when a new model tier \(e.g. Claude 4.8\) lands -- a contributor
could update one site and miss the other.

Centralise the gating in `AnthropicConfig._validate_effort_for_model`,
which returns an error message string or `None`. Each call site keeps
its own provider-appropriate exception type \(`BadRequestError` for the
chat path, `AnthropicError` for the /v1/messages pass-through\) but the
gating decision now comes from one place. Net -11 LOC.

Adds a parametrised unit test exercising the helper directly across
4.5 / 4.6 / 4.7 model families and `max` / `xhigh` / lower-effort
inputs. Existing tests at both call sites continue to pass unchanged.

Addresses Greptile finding on PR #27074.

* fix\(databricks\): narrow reasoning_effort_value to str for mypy

`non_default_params.get\("reasoning_effort"\)` returns `Any | None`,
but `REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT.get\(\)` expects `str`.
Mypy flagged this on the strict pass. Narrow with `isinstance` before
the lookup; non-strings fall through to the existing `BadRequestError`
below with a clean validation message, so behavior is unchanged.

Fixes a regression introduced by 1a10746e95 in this PR.

* feat\(proxy\): add health_check_reasoning_effort for model health checks

Co-authored-by: Cursor <cursoragent@cursor.com>

* test\(image_gen\): align Azure image gen fixture with body omitting model

Expected JSON matches deployment-scoped Azure POST \(#26316\).

Co-authored-by: Cursor <cursoragent@cursor.com>

* test\(anthropic/chat\): force PR-local model_cost map via autouse fixture

CI runs without LITELLM_LOCAL_MODEL_COST_MAP=True, so litellm.model_cost
is loaded from main-branch JSON \(default model_cost_map_url\) instead of
the PR's checked-out model_prices_and_context_window.json. Tests that
assert per-model flags added in this PR \(supports_max_reasoning_effort,
supports_xhigh_reasoning_effort\) therefore pass locally but fail in CI
with 'AssertionError: assert False is True' on 5 cases:

  - test_anthropic_model_supports_effort_param_recognizes_supporting_models
    \[anthropic.claude-mythos-preview, bedrock/.../mythos-preview,
     claude-opus-4-5-20251101\]
  - test_supports_effort_level_handles_provider_prefixes
    \[bedrock/invoke/us.anthropic.claude-sonnet-4-6-max-True,
     claude-sonnet-4-6-max-True\]

Add an autouse fixture at tests/test_litellm/llms/anthropic/chat/conftest.py
that monkey-patches litellm.model_cost to the PR-local JSON for every test
in this directory. The parent conftest already snapshots+restores
litellm.model_cost per-function, so the mutation is contained.

This is a scoped workaround. The proper fix is to set the env var
globally in the test workflow once the ~10 inline self-set test files
are audited; tracking that as a follow-up issue.

* \[Fix\] Docker: Pin Wolfi And Uv To Multi-Arch Index Digests

The previous pins resolved to single-platform amd64 manifests, so buildx
pulled the same amd64 base for both linux/amd64 and linux/arm64 targets.
The published OCI index then advertised an arm64 entry whose layers are
byte-identical to amd64 -- arm64 users got an amd64 binary.

Switch all three Dockerfiles to the multi-arch image-index digests:
  - cgr.dev/chainguard/wolfi-base   \(index has linux/amd64 + linux/arm64\)
  - ghcr.io/astral-sh/uv:0.11.7     \(index has linux/amd64 + linux/arm64\)

Resolved with `docker buildx imagetools inspect <ref>` -- that returns
the index digest. `docker pull` + `docker inspect` returns the per-host
platform digest, which is what slipped in last time.

* \[Fix\] Docker: Pin Uv To Multi-Arch Index Digest In Remaining Dockerfiles

Apply the same fix to the three Dockerfiles not in the release pipeline
today \(alpine, dev, health_check\) so they stay correct if/when they're
built for arm64 in the future.

Wolfi pins are not present in these files; the python:3.11-alpine and
python:3.13-slim digests they already use are multi-arch indexes that
include arm64/v8, so only the uv pin needed swapping.

* fix\(xai\): fold reasoning_tokens into completion_tokens to satisfy OpenAI invariant

xAI's chat completions API accounts reasoning_tokens separately from
completion_tokens, but rolls them into total_tokens. This breaks the
OpenAI invariant total_tokens == prompt_tokens + completion_tokens
that downstream consumers \(including litellm's own _usage_format_tests
in tests/llm_translation/base_llm_unit_tests.py:58\) rely on.

Live capture \(grok-3-mini-beta, 2026-05-04\):
    prompt=14, completion=10, total=336, reasoning=312
    14 + 10 = 24, NOT 336.

OpenAI's o1/o3 reasoning models include reasoning_tokens in
completion_tokens, leaving the prompt+completion=total invariant
intact. xAI deviates. This patch aligns xAI to OpenAI semantics by
folding reasoning_tokens into completion_tokens after the parent
OpenAI parser runs.

The fold is idempotent and defensive:
- Only fires when total_tokens == prompt_tokens + completion_tokens
  + reasoning_tokens \(the documented xAI shape\). Refuses to fold if
  the gap doesn't match, guarding against silent corruption when xAI
  changes accounting.
- Skips if completion_tokens already covers the gap \(already
  normalised — e.g. cost calc replays a previously-folded Usage\).

xai.cost_calculator.cost_per_token already added reasoning_tokens to
the visible completion count for billing. Post-fold the Usage block
now satisfies that invariant directly, so the cost calc would
double-bill. Updated cost_per_token to detect the OpenAI-normalised
shape \(total == prompt + completion\) and skip the reasoning add-on
in that case, falling through to the legacy raw-shape behaviour for
callers that bypass the transformation \(e.g. proxy log replay\).

Tests:
- Adds TestXAIReasoningTokenFolding covering: gap-explained-fold,
  idempotent-no-double-fold, no-reasoning-skip, gap-mismatch-skip.
- Adds test_already_normalised_usage_does_not_double_count_reasoning
  to lock the cost-calc idempotency.
- Updates 7 pre-existing cost-calc tests whose total_tokens was
  internally inconsistent \(used the OpenAI-normalised total but kept
  reasoning_tokens external\) to use the documented xAI raw shape
  total = prompt + visible completion + reasoning. Pre-existing
  values masked the missing-fold by accident.

Verified end-to-end against the live xAI API:
    LITELLM_LOCAL_MODEL_COST_MAP=False \(CI default\) +
    XAI_API_KEY set +
    pytest tests/llm_translation/test_xai.py::TestXAIChat::test_prompt_caching
        -> PASSED in 18.81s \(was: AssertionError on
        usage.total_tokens == usage.prompt_tokens + usage.completion_tokens\)

20/20 tests in tests/test_litellm/llms/xai/test_xai_cost_calculator.py
and 8/8 in tests/test_litellm/llms/xai/test_xai_chat_transformation.py
pass.

* refactor\(bedrock/converse\): delegate effort gating to AnthropicConfig._validate_effort_for_model

Removes the duplicated max/xhigh gating logic in
_validate_anthropic_adaptive_effort and the now-unused
_supports_effort_level_on_bedrock helper. Per-model gating now flows
through the centralized AnthropicConfig._validate_effort_for_model
\(whose _supports_effort_level already strips Bedrock prefixes\), so the
chat completion, /v1/messages, and Bedrock Converse paths can't drift
when a new gated effort tier is added.

* Implement normalize_nonempty_secret_str function to trim whitespace from secrets and treat empty values as unset. Update proxy_server to use this function for Grafana credentials. Enhance tests to validate the new normalization behavior.

* Fix qdrant semantic cache miss metadata

* chore\(deps\): refresh dependency locks

* chore\(deps\): authorize pytest license

* fix: preserve tokenizer decode round trips

* refactor\(anthropic\): drive adaptive-thinking gate via supports_adaptive_thinking flag

Three of greptile's open comments on #27074 \(P2 converse:512, P1
databricks:361, and the underlying capability-flag policy rule\) flagged
the same pattern: _is_claude_4_6_model\(...\) or _is_claude_4_7_model\(...\)
used inline as a runtime 'is this an adaptive-thinking model?' check.
That requires a code release each time a new adaptive Claude lands.

Consolidate the inline gating to AnthropicModelInfo._is_adaptive_thinking_model,
and switch the helper itself to read a new supports_adaptive_thinking
flag from `model_prices_and_context_window.json` via `_supports_factory`,
falling back to the family pattern only when the model-map entry doesn't
carry the flag \(preserves OpenRouter / Vercel / Bedrock-prefixed variants
that route through the same code path with non-canonical ids\).

Adds `supports_adaptive_thinking: true` to the four 4.6/4.7 anthropic
entries \(opus-4-6 + dated, opus-4-7 + dated, sonnet-4-6\). Bedrock-prefixed
and Vertex-prefixed entries don't need the flag because both fall back
through the family pattern \(the helper short-circuits early on True from
either path\) and the bedrock/vertex Claude IDs all match the existing
opus-4-{6,7} / sonnet-4-{6,7} pattern.

Affected call sites:

- `bedrock/chat/converse_transformation.py:_handle_reasoning_effort_parameter`
- `anthropic/chat/transformation.py:_map_reasoning_effort`
- `anthropic/chat/transformation.py:map_openai_params` \(output_config branch\)
- `databricks/chat/transformation.py:map_openai_params` \(output_config branch\)

The remaining `_is_claude_4_6_model` / `_is_claude_4_7_model` references
in `AnthropicConfig._validate_effort_for_model` and
`AnthropicConfig.get_supported_openai_params` are intentionally retained:
they're per-model gating fallbacks for variants whose model-map entries
don't yet carry the `supports_max_reasoning_effort` /
`supports_reasoning` flag. Those are documented in-place.

Tests: 537 anthropic/bedrock/databricks/vertex/messages tests pass.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* chore\(deps\): address dependency review notes

* test\(model_prices\): add supports_adaptive_thinking to schema

`test_aaamodel_prices_and_context_window_json_is_valid` validates the
model-map JSON against an explicit schema with `additionalProperties`,
so the new `supports_adaptive_thinking` flag added in
98ced0ae43 needs a matching schema entry.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* refactor: remove unnecessary comments from #27074

Strip out the explanatory and historical comments that don't carry
business-logic justification. Comments that simply narrate what code
does — or that explain prior behavior, what was changed, or which PR
introduced a fix — are removed. Docstrings are reduced to a one-line
summary where the long form repeated information already evident from
the code or test data.

No code-behavior changes. All 643 affected unit tests still pass.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* test: keep decode token test local

* chore\(deps\): align dashboard node engine

* feat: selectively apply routing strategy according to model name

* style: make _model_supports_effort_param more concise

* refactor\(anthropic,bedrock\): hoist drop_params output_config warning to module constant

Three call sites \(anthropic chat, bedrock converse, bedrock invoke messages\)
emitted the same '...Effort is only supported on Opus 4.5+, Sonnet 4.6+, and
Mythos Preview' warning verbatim. Extract DROP_UNSUPPORTED_OUTPUT_CONFIG_WARNING
in litellm/llms/anthropic/chat/transformation.py and import it from the bedrock
sites so future copy edits live in one place.

Addresses Michael's review on PR #27074.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* refactor\(anthropic,bedrock,databricks\): factor BadRequestError for unknown reasoning_effort

Three call sites raised the same BadRequestError\("Invalid reasoning_effort:
... Must be one of 'minimal', 'low', ..."\) block when REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT
returned None: anthropic chat map_openai_params, bedrock converse
_handle_reasoning_effort_parameter, and databricks chat reasoning_effort path.

Extract AnthropicConfig._raise_invalid_reasoning_effort\(model, value, llm_provider\)
so future copy edits / valid-set changes happen in one place. Typed as NoReturn
so type-checkers correctly narrow control flow at call sites.

Addresses Michael's review on PR #27074.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* Clean up Redis semantic cache isolation fallback

* fix\(guardrails\): align banned_keywords + azure_content_safety call_type gates with runtime route_type

The hooks gated on ``call_type == "completion"`` but the proxy ingress
passes ``route_type`` straight through as ``call_type`` —
``"acompletion"`` for /v1/chat/completions and ``"aresponses"`` for
/v1/responses. Tests passed because they used the literal sync
``"completion"`` value, masking the gap.

Switch both hooks to ``is_text_content_call_type`` \(matches the
canonical runtime values: completion / acompletion / aresponses\) and
update existing tests to assert against runtime values, plus parametrize
a regression test that pins the gate.

* fix: remove unused import

* Add semantic cache legacy migration flag

* Treat 0 team_member_budget as no cap

* chore\(caching\): annotate qdrant quantization_params dict type

Mypy infers the dict's value type from the first branch
\(Dict\[str, bool\]\) which clashes with the scalar branch's mixed-type
inner dict. Explicit Dict\[str, Any\] annotation lifts the inference.

* chore\(caching\): remove allow_legacy_unscoped_cache_hits opt-in

The flag was an opt-in escape hatch for the cross-tenant leak the rest
of the patch closes — flipping it on \(env var or constructor param\)
re-enables exactly the VERIA-54 primitive on either backend. There is
no operational need that the secure path doesn't already meet:

- Qdrant: legacy points without ``litellm_cache_key`` payload are
  excluded by the must-clause filter and treated as misses; new sets
  populate the cache key, so cold-start lasts only as long as the
  natural cache rebuild.
- Redis: existing unscoped index can't carry the new schema; the init
  path falls back to ``{name}_isolated`` \(and recreates it on stale
  schema\), leaving the legacy index untouched.

Drop the construc…")[#27245](https://github.com/BerriAI/litellm/pull/27245)[)](https://github.com/BerriAI/litellm/commit/6ff668c7aa01a73738ed39aa64913a089a183565 "\[Infra\] Promote internal staging to main \(#27245\)

* default requested_model to empty string on litellm-side rejects

* Update litellm/router.py

Co-authored-by: greptile-apps\[bot\] <165735046+greptile-apps\[bot\]@users.noreply.github.com>

* fix: scope key access_group_ids override by team's assigned groups

A team member could set any access_group_ids on their key \(e.g. a group
assigned only to a different team\) and override the team's model
restriction. Intersect the key's access_group_ids with team_object.access_group_ids
in _key_access_group_grants_model so foreign groups are dropped before
model expansion. Adds a regression test that asserts expansion is never
called for foreign groups.

* \[Fix\] Proxy: Skip Personal Budget Hook When Reservation Covers Counter

The reservation path \(PR #26845\) atomically pre-fills `spend:user:{user_id}`
and admits at the strict-`<` boundary. The legacy `_PROXY_MaxBudgetLimiter`
pre-call hook re-reads the same counter with `>=`, so a reservation that
fills the counter to exactly `max_budget` \(e.g. a request without a
`max_tokens` cap that falls back to reserving the smallest remaining
headroom\) is rejected by the hook even though the reservation already
admitted it.

Skip the hook when the request's active `budget_reservation` covers
`spend:user:{user_id}`. The reservation is the source of truth for that
counter cross-pod; the legacy `>=` path remains in place for requests
without a reservation \(e.g. paths that bypass the reservation entirely\).

Reproduces as `tests/otel_tests/test_prometheus.py::test_user_budget_metrics`
on a fresh user with `max_budget=10` calling `fake-openai-endpoint` without
`max_tokens`. Adds focused unit coverage in
`tests/test_litellm/proxy/hooks/test_max_budget_limiter.py`.

* harden bedrock file bucket validation

* Fix syntax errors from botched merge in router.py

* Fix Vertex batch output edge cases

* \[Fix\] RBAC: Drop management_routes Write Fallback for Admin Viewer

Greptile P1: the unsafe-method branch of `_check_proxy_admin_viewer_access`
ended with a blanket `if route in management_routes: return`. That set is a
mix of reads \(info/list — handled via the safe-method GET branch above\) and
writes. The fallback let Admin Viewer POST to write endpoints not enumerated
in `_ADMIN_VIEWER_BLOCKED_WRITE_ROUTES`, including:
  - /team/block, /team/unblock, /team/permissions_update
  - /jwt/key/mapping/{new,update,delete}
  - /key/bulk_update
  - /key/{key_id}/reset_spend

Remove the fallback. The two remaining allow sets \(admin_viewer_routes and
global_spend_tracking_routes\) are both read-only, so removal does not affect
the legitimate POST-as-read cases \(e.g. /spend/calculate, which is in
spend_tracking_routes ⊂ admin_viewer_routes\).

Tests:
  - 8 new parametrized cases pinning each previously-leaking management write
    endpoint to 403 on POST for PROXY_ADMIN_VIEW_ONLY.

* fix\(tests\): anchor VCR redis cassette key to repo root

`os.path.relpath` with no `start` arg uses the current working
directory, so running pytest from a subdirectory produced a
different Redis key than running from the repo root. CI-recorded
cassettes and locally-replayed runs would silently miss each
other's cache.

Anchor the path to the repo root \(derived from `__file__`\) so the
key is stable regardless of CWD.

https://claude.ai/code/session_018uCx7pcrkdUJZrCVMaTdPx

* fix: gate key access_group override on group's own assignment

Replaces the previous intersect-with-team.access_group_ids check, which
made the override unreachable in practice \(the team-gate fallback already
covered every case the intersection allowed\). The override now resolves
each of the key's access_group_ids via get_access_object and accepts the
group only if its assigned_team_ids includes the key's team_id, or its
assigned_key_ids includes the key's token. This fulfills the original ask
\(a key can extend a team's allow-list via a group the admin granted to
that team or that specific key\) while still rejecting foreign groups
referenced by team members of other teams.

* \[Fix\] Proxy/Key Management: Honor team_member_permissions /key/list In /key/list Endpoint

When a team grants /key/list via team_member_permissions, non-admin members
should see all keys for that team — same as a team admin. Previously the
classification in list_keys\(\) only checked admin status, so permitted
members fell into the service-account-only path and could not see other
members' personal keys. Routes those members into the full-visibility set.

* Fix access-group bypass via litellm-model fallback path

When _get_all_deployments returns 0 candidates and the litellm-model
fallback branch \(_get_deployment_by_litellm_model\) finds deployments that
the access-group filter then empties, _access_group_filter_emptied_candidates
remained False \(it was captured before that branch ran\). The router would
then proceed to default fallbacks; the fallback model could have no
access_groups and short-circuit the filter, silently serving a caller
blocked by access-group restrictions.

Update the flag inside the litellm-model branch when filtering empties a
non-empty candidate set so the default-fallback guard still triggers.

* fix\(proxy\): redact MCP server URL and headers for non-admin viewers \(VERIA-8\)

Many MCP integrations \(Zapier, etc.\) embed an upstream API key
directly in the server URL, e.g.
``https://actions.zapier.com/mcp/<api-key>/sse``. The list and
single-server endpoints were returning the full URL to any
authenticated user — `_redact_mcp_credentials` only stripped the
explicit ``credentials`` field, and `_sanitize_mcp_server_for_virtual_key`
only ran for restricted virtual keys. Non-admin internal users could
read the dashboard, click the unmask toggle, and exfiltrate the raw
token.

Add `_sanitize_mcp_server_for_non_admin` that runs on top of the
existing credential redaction and clears the credential-bearing
fields:

- ``url`` \(the primary leak vector\)
- ``spec_path`` \(OpenAPI spec URLs that may carry tokens\)
- ``static_headers`` / ``extra_headers`` \(Authorization\)
- ``env`` \(arbitrary secrets\)
- ``authorization_url`` / ``token_url`` / ``registration_url``

Identity fields \(``server_id``, ``alias``, ``mcp_info``, etc.\) are
preserved so the UI can still list servers a non-admin's team has
access to.

Apply the new sanitizer in `fetch_all_mcp_servers` and the per-server
fetch path right after the existing virtual-key branch. Update the
existing `test_list_mcp_servers_non_admin_user_filtered` assertions
that previously checked URL visibility.

Frontend defense-in-depth: hide the URL unmask toggle on
`mcp_server_view.tsx` unless the viewer is a proxy admin.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* Fix runtime policy attachment initialization

Mark runtime-created policies and attachments initialized so global policy attachments created from the policy builder apply immediately without requiring a restart.

Co-authored-by: Cursor <cursoragent@cursor.com>

* test\(router\): cover _try_early_resolve_deployments_for_model_not_in_names

The router_code_coverage CI check requires every function in router.py to
be referenced by at least one test under tests/{local_testing,
router_unit_tests,test_litellm} in a file with "router" in its name.
The recently-extracted helper had no direct test, so the check failed
with "0.45% of functions in router.py are not tested".

Add a focused test that exercises the four return paths: model already
in self.model_names, no fallback applies, pattern-router match, and
default_deployment substitution \(also asserting the stored default
isn't mutated\).

https://claude.ai/code/session_019AVp1XL7RT9RxRe4qRLkay

* Fix policy registry teardown in tests

Reset the policy ID index during policy engine test cleanup so stale policy versions cannot leak between tests.

Co-authored-by: Cursor <cursoragent@cursor.com>

* fix\(batches\): count non-chat tokens, validate batch-file model access \(VERIA-39\) \(#27015\)

* fix\(batches\): count non-chat tokens and validate every model in batch file

Two security control bypasses on POST /v1/batches:

1. `_get_batch_job_input_file_usage` only summed tokens for
   `body.messages` \(chat completions\). Embedding \(`input`\) and text
   completion \(`prompt`\) batches reported zero, letting massive
   non-chat workloads slip past TPM rate limits. Extend the counter
   to handle string and list shapes for both fields.

2. The batch input file was forwarded to the upstream provider
   without inspecting the models named inside the JSONL — only the
   outer `model` query parameter was checked against the caller's
   allowlist. A caller restricted to gpt-3.5 could submit a batch
   targeting gpt-4o and the upstream would execute it under the
   proxy's shared API key.

Add `_get_models_from_batch_input_file_content` \(returns the
distinct `body.model` values\) and call it from
`_enforce_batch_file_model_access` in the pre-call hook, which runs
each model through `can_key_call_model` so the same allowlist
semantics \(wildcards, access groups, all-proxy-models, team aliases\)
the proxy enforces on `/chat/completions` apply here too. Any
unauthorized model raises a 403 before the file is forwarded.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(batches\): count pre-tokenized prompt/input shapes, classify 403 logs

Two follow-ups from the Greptile review on the batch validation PR:

1. P1 TPM bypass via integer token arrays. The OpenAI batch schema
   accepts ``prompt`` and ``input`` as ``list\[int\]`` \(a single
   pre-tokenized prompt\) or ``list\[list\[int\]\]`` \(multiple\) in addition
   to the string and ``list\[str\]`` shapes. Pre-fix only the string
   shapes were counted, so a caller could submit a batch with hundreds
   of millions of pre-tokenized tokens and the rate limiter would
   record zero. Extract the per-field logic into
   ``_count_prompt_or_input_tokens`` and count each int as one token.

2. P2 access-denial logs were indistinguishable from I/O failures.
   ``count_input_file_usage`` caught every exception under a generic
   "Error counting input file usage" message, so an intentional 403
   from ``_enforce_batch_file_model_access`` looked the same in the
   logs as a missing file or a Prisma timeout. Catch ``HTTPException``
   separately and log 403s at WARNING level with a security-relevant
   message before re-raising.

Tests cover the new shapes: single ``list\[int\]``, ``list\[list\[int\]\]``
\(the worst-case bypass vector\), and embeddings ``input`` with
pre-tokenized arrays.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

---------

Co-authored-by: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(proxy\): re-validate user_id after /user/info re-parses query \(#27009\)

* fix\(proxy\): re-validate user_id ownership after /user/info re-parses query

The route-level access check in `RouteChecks.non_proxy_admin_allowed_routes_check`
reads `request.query_params.get\("user_id"\)`, which decodes literal `+` to
spaces. The endpoint then re-parses the raw query string with `urllib.unquote`
in `get_user_id_from_request` to preserve `+` characters \(so plus-addressed
emails work as user_ids\). Those two paths produce different ids: a caller
who registered a user_id containing a literal space could pass the route
check and then read another user's row by sending the encoded `+` form.

Add `_enforce_user_info_access` and call it after `_normalize_user_info_user_id`
returns the final id. Proxy admin / view-only admin still bypass; everyone
else must match the resolved user_id \(or have no user_id, which falls back
to the caller's own id later in the handler\).

Tests cover the admin bypass, owner-match path, and the cross-user lookup
that this change blocks.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(proxy\): apply user_info ownership check to PROXY_ADMIN_VIEW_ONLY

`_enforce_user_info_access` was bypassing both PROXY_ADMIN and
PROXY_ADMIN_VIEW_ONLY, but the upstream route check in
`RouteChecks.non_proxy_admin_allowed_routes_check` only treats
PROXY_ADMIN as a true admin for the `/user/info` route — view-only
admins go through the `user_id == valid_token.user_id` enforcement
along with regular users. Mirroring that asymmetry left the same
encoded-`+` bypass open for view-only admins whose user_id contains a
literal space.

Drop the PROXY_ADMIN_VIEW_ONLY exemption so the post-decode re-check
matches the upstream rule. Update tests: a view-only admin must now
be blocked from cross-user lookups but still allowed to read their
own row.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

---------

Co-authored-by: yuneng-jiang <yuneng@berri.ai>
Co-authored-by: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* feat\(spend-logs\): opt-in suppression of stack traces in spend-tracking error logs

Adds LITELLM_SUPPRESS_SPEND_LOG_TRACEBACKS env var. When set to true and the
proxy log level is INFO or above, spend-tracking error paths emit a single
ERROR line without the full traceback. Stack traces are preserved at DEBUG
and the Sentry / proxy_logging_obj.failure_handler path is unchanged.

The new spend_log_error helper is wired through the spend write hot path:
  - DBSpendUpdateWriter \(update_database, _update_*_db, batch upsert,
    redis-commit fallbacks\)
  - _ProxyDBLogger._PROXY_track_cost_callback
  - get_logging_payload exception path
  - update_spend / update_daily_tag_spend / spend logs queue monitor

Resolves LIT-2704.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(spend-logs\): preserve no-traceback behavior for update_daily_tag_spend

This call site previously logged a single-line error via verbose_proxy_logger.error\(\)
with no traceback. Switching it to spend_log_error\(..., exc=e\) caused a full stack
trace to render by default \(when LITELLM_SUPPRESS_SPEND_LOG_TRACEBACKS is unset\),
which contradicts the PR goal of leaving default behavior unchanged. Revert this
specific site to the original error log call.

* fix\(spend-logs\): preserve no-traceback behavior for update_daily_tag_spend

Bugbot caught a regression: the previous error log here was a single-line
verbose_proxy_logger.error\(...\) with no traceback. spend_log_error attaches
the active exception's traceback by default \(when the suppression env var
is unset\), so swapping it in changed default behavior. Revert this one site
to its original .error\(\) call to keep the PR strictly opt-in.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* feat\(spend-logs\): suppress traceback in SpendLogs error_information row

Extend LITELLM_SUPPRESS_SPEND_LOG_TRACEBACKS to the failure callback so the
per-row Metadata pane in the UI no longer shows the stack trace when the
opt-in env var is set, matching the existing console-side suppression.

https://claude.ai/code/session_014dztoRbRnRvq54HL9EyHx6

* \[Fix\] Proxy: Repair Merge Fallout In Router-Override Fallback Auth

Conflict resolution for #26968 dropped the `Iterator` typing import
\(NameError at module load\), left a dead `fallback_models = cast\(...\)`
block, and the new tests called `_enforce_key_and_fallback_model_access`
without the now-required `request` kwarg.

* isolate dual OTEL handlers

* harden cloud file compatibility path

* harden cloud file compatibility path

* \[Fix\] Proxy/Key Management: Align Key-Org Membership Checks On Generate And Regenerate

Mirrors the membership rule on /key/update so that /key/generate and
/key/{key}/regenerate apply the same `_validate_caller_can_assign_key_org`
gate when the caller specifies an `organization_id`. Proxy admins bypass.
The check no-ops when `organization_id` is not being set.

* thread trusted params through vertex file content

* trust only server legacy file flag

* chore\(proxy\): keep public AI hub unauthenticated

* fix\(proxy\): preserve low-detail readiness status

* \[Test\] Anthropic: Replace Legacy Claude-4-Sonnet Alias With Haiku 4.5

Three live-API tests pinned to claude-4-sonnet-20250514, which is a
non-canonical alias of claude-sonnet-4-20250514. Anthropic's main API
no longer resolves the legacy form under freshly issued keys, so the
tests fail with not_found_error. The token counter test pinned to
claude-sonnet-4-20250514 itself \(deprecation_date 2026-05-14, two weeks
out\) was on borrowed time too.

Bump all four to claude-haiku-4-5-20251001 — capability superset for what
these tests exercise \(streaming, parallel tool calling, extended thinking,
token counting\), no upcoming deprecation, cheaper per-token.

* chore\(proxy\): move URL-valued model/file_id guard from SDK to proxy

The previous per-provider guards in HuggingFace, Oobabooga, and Gemini
files lived in the SDK layer, breaking SDK callers who legitimately pass
URL-valued model identifiers. Move the check to the proxy boundary in
add_litellm_data_to_request so SDK users keep working while proxy users
default-deny URL-valued model and file_id, with admin opt-in via
litellm.provider_url_destination_allowed_hosts.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* \[Chore\] Proxy/UI: Drop stray _experimental/out/chat/index.html

This file is a regenerable UI build artifact that should not be tracked
in source. Removing so the merge into litellm_internal_staging stays clean.

* \[Test\] Anthropic Passthrough: Bump Streaming Cost-Injection Test To Haiku 4.5

test_anthropic_messages_streaming_cost_injection hits the proxy's
/v1/messages route, which routes via the anthropic/* wildcard to
api.anthropic.com. The 404 surfaced in the test was Anthropic's own
not_found_error propagated back through the proxy \(visible from the
x-litellm-model-id hash on the response — the proxy did route\).

Same root cause as the prior commit: the legacy claude-4-sonnet-20250514
alias is no longer recognized by Anthropic's main API under the new key.
Swap to claude-haiku-4-5-20251001 — same routing path, canonical model.

* fix\(proxy\): handle ownership-recording failures after upstream create

If record_container_owner raises after the upstream container is created,
the user previously got a 500 with no usable container — they were billed
for an unreachable resource. Move ownership recording into the create
path's exception handling and split the two failure modes:

- HTTPException from the recorder \(auth conflicts\) propagates verbatim
  so the client sees the real status code, not a generic LLM error.
- Unexpected exceptions are logged and swallowed; the response is
  returned to the caller so they aren't billed for a container they
  can't address. The DB row stays untracked until an operator reconciles.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(guardrails\): close post-call coverage gaps

* fix\(types\): add /team/permissions_bulk_update to management_routes

The blocklist check in _check_proxy_admin_viewer_access only fires for
routes that match LiteLLMRoutes.management_routes — the bulk-update
endpoint was missing from that list, so the test for view-only admins
on /team/permissions_bulk_update fell through to "allow."

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* \[Test\] Anthropic Passthrough: Bump Thinking Tests Off Legacy Sonnet 4 Alias

base_anthropic_messages_test.test_anthropic_messages_with_thinking and
test_anthropic_streaming_with_thinking still pinned to
claude-4-sonnet-20250514 — the same legacy alias Anthropic no longer
recognizes under freshly issued keys. The other four tests in this base
class already use claude-sonnet-4-5-20250929; these two were missed.

Bump to claude-haiku-4-5-20251001 \(supports_reasoning=true, no upcoming
deprecation\). Subclasses including TestAnthropicPassthroughBasic
inherit these methods.

* fix\(guardrails\): cover multi-choice output variants

* fix\(proxy\): preserve public ai hub ui setting

* fix\(scim\): cascade FK cleanup on user delete and surface block status in UI

SCIM DELETE /Users/{id} previously called litellm_usertable.delete without
clearing rows that FK back to the user, so Postgres rejected the delete with
LiteLLM_InvitationLink_user_id_fkey and the SCIM caller saw a 500. Add a
helper to drop invitation_link, organization_membership, and team_membership
rows before the user delete \(mirrors /user/delete in internal_user_endpoints\).

Also add a Status column to the Virtual Keys and Internal Users tables so
admins can see at a glance which keys are blocked and which users SCIM has
deactivated. SCIM-blocked keys carry a tooltip explaining the origin.

Pin the dashboard's Node version to 20 via .nvmrc to match CI.

* chore: update Next.js build artifacts \(2026-05-02 03:21 UTC, node v20.20.2\)

* perf\(proxy\): cache container/skill ownership reads on the hot path

Container ownership and skill rows are looked up on every retrieve /
delete / list / file-content / chat-completion-with-skill call. The new
stores wrapped raw Prisma queries with no cache, putting one DB
round-trip on each request. Add an in-process TTL'd cache mirroring the
_byok_cred_cache pattern in mcp_server/server.py: per-key \(value,
monotonic_timestamp\), 60s TTL, 10000-entry cap with full-clear on
overflow, invalidated by every write. Negative results \(`None`\) are
cached too so untracked-resource checks also skip the DB.

Tests cover: cache-after-first-hit, negative caching, write
invalidation, no-caching-on-DB-error, TTL expiry, capacity eviction.
56 tests pass.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* chore: update Next.js build artifacts \(2026-05-02 03:39 UTC, node v20.20.2\)

* fix: remove traceback key instead of it being ""

* fix: linting error

* fix\(scim\): preserve scim_active on PUT when client omits the field

A SCIM PUT may legally omit `active` \(full-replace with the field
absent\). Pydantic fills the SCIMUser.active default of True, so the PUT
handler was overwriting metadata.scim_active with True even when the
client never sent it — silently reactivating a previously SCIM-blocked
user and unblocking their keys.

Use model_fields_set to detect whether the client actually sent
`active`. If omitted, preserve the prior scim_active value and skip
the cascade to virtual keys.

Also drop comments added in this PR that just narrate what the code
does; keep only the docstrings and the SQL-NULL pitfall note that
explain non-obvious behaviour.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(proxy\): use set lookup for permitted agent filters

* fix\(mcp\): redact command fields for non-admin server views

* fix\(proxy\): forward decoded container ids after ownership checks

* fix\(caching\): handle stale isolated Redis semantic index

* fix\(cloudflare\): support response_text in streaming chunk parser

Newer Cloudflare Workers AI models \(e.g. Nemotron\) emit 'response_text'
instead of 'response' on streamed chunks. The non-streaming path was
already updated to fall back to 'response_text' \(#26385\), but the
streaming chunk parser still only read 'response', which caused
streaming requests against those models to silently produce empty
content.

Mirror the non-streaming fallback in CloudflareChatResponseIterator.chunk_parser
and add a streaming test for the response_text shape.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* Fix code qa

* Address bugbot: drop dead encode/decode helpers; preserve empty custom_id

- Remove unused _encode_gcp_label_value / _decode_gcp_label_value singular
  helpers; only the _chunks variants are actually called.
- Use 'is not None' check for custom_id so empty-string custom_ids are
  still labeled and round-trip through batch outputs.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* Forward Vertex file content logging context

* test vertex file content logging forwarding

Co-authored-by: Sameer Kankute <Sameerlite@users.noreply.github.com>

* Fix Vertex batch output logging mutation

* fix: don't mutate caller's logging_obj in _try_transform_vertex_batch_output_to_openai

The method was overwriting logging_obj.optional_params, logging_obj.model,
and logging_obj.start_time on the caller's Logging instance. When invoked
from llm_http_handler.py's generic framework path, the framework's own
logging_obj \(which already went through pre_call\) had its properties
clobbered, causing model and start_time to reflect the last batch line's
values rather than the original call context.

Fix: create a fresh local Logging instance for the per-line transformation
instead of mutating the incoming logging_obj. The caller's object is now
left entirely untouched regardless of whether a logging_obj was passed in
or not.

Regression tests added to verify model, start_time, and optional_params
are not mutated on the caller's logging_obj.

Co-authored-by: Sameer Kankute <Sameerlite@users.noreply.github.com>

* feat: add opt-out flag for Vertex batch output transformation

Adds litellm.disable_vertex_batch_output_transformation \(default False\).
When True, afile_content returns raw Vertex predictions.jsonl untouched
so users that parse candidates/modelVersion directly are not broken.

* fix\(anthropic,bedrock\): omit thinking/output_config when reasoning_effort="none"

Setting reasoning_effort="none" on Anthropic chat models \(direct, Bedrock
Invoke, Bedrock Converse, Vertex AI Anthropic, Azure AI Anthropic\) crashed
LiteLLM with:

  litellm.APIConnectionError: 'NoneType' object has no attribute 'get'

Both the Anthropic chat transformation and Bedrock Converse called
``AnthropicConfig._map_reasoning_effort`` and assigned the ``None`` it returns
for ``"none"`` directly to ``optional_params\["thinking"\]``. Downstream
``is_thinking_enabled`` then did ``optional_params\["thinking"\].get\("type"\)``
and crashed.

Pop ``thinking`` \(and on Claude 4.6/4.7, ``output_config``\) instead of
assigning ``None``, restoring the documented contract that
``reasoning_effort="none"`` means "do not enable thinking". This also
prevents downstream Anthropic 400s \("thinking: Input should be an object",
"output_config.effort: Input should be ..."\) if the bug were ever masked.

Verified end-to-end against the live Anthropic API and Bedrock Converse
on claude-opus-4-{5,6,7} and claude-sonnet-4-6, plus Bedrock Invoke for
Claude 4.5/4.6. Vertex AI Anthropic and Azure AI Anthropic inherit the
fixed ``map_openai_params`` from ``AnthropicConfig`` and need no further
changes.

* fix\(vertex-ai\): set response=null on batch error entries per OpenAI spec

The Vertex batch output transformer was emitting both a populated 'response' and 'error' for failed batch entries. The OpenAI Batch output spec defines them as mutually exclusive: on error 'response' MUST be null. This broke any consumer using 'result\["response"\] is None' to detect failures.

* test\(vertex-ai\): cover transformation_error path emits response=null

* fix\(security\): sandbox jinja2 in gitlab/arize/bitbucket prompt managers

DotpromptManager was hardened to render through
ImmutableSandboxedEnvironment. The three sibling managers \(gitlab,
arize, bitbucket\) were missed and still instantiate plain
jinja2.Environment\(\), leaving the same attribute-traversal SSTI
primitive open: a template fetched from a GitLab/BitBucket repo or
Arize Phoenix workspace can reach __class__.__init__.__globals__ and
execute arbitrary Python on the proxy host.

Match the dotprompt pattern by switching all three to
ImmutableSandboxedEnvironment. The sandbox blocks the dunder-traversal
chain while leaving normal {{ var }} substitution intact, so the
template surface is unchanged for legitimate use.

Adds tests/test_litellm/integrations/test_prompt_manager_ssti.py
\(18 cases\) verifying each manager's jinja_env is a sandbox, that
classic SSTI payloads raise SecurityError, and that ordinary variable
rendering still works.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* chore\(proxy\): drop client-supplied pricing fields from request bodies

The proxy currently forwards request-body pricing parameters \(the fields
on `CustomPricingLiteLLMParams`, plus `metadata.model_info`\) into the
core call path. Those fields belong to deployment configuration, not to
per-request input — sending them from a client mutates the request's
recorded cost and, via `litellm.completion` → `register_model`, the
process-wide `litellm.model_cost` map for every later caller in the
worker. Strip them at the boundary.

The strip set is built from `CustomPricingLiteLLMParams.model_fields` so
pricing fields added later are covered automatically. Operators who do
want clients to supply per-request pricing can opt back in per key or
team via `metadata.allow_client_pricing_override = true`, mirroring the
existing `allow_client_mock_response` and
`allow_client_message_redaction_opt_out` flags.

Tests cover the strip set's coverage, root and metadata strips, the
opt-in skip on both key and team metadata, and a regression check that
the global `litellm.model_cost` map is unmutated after a stripped
request.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* chore\(proxy\): log stripped pricing fields at debug for operator visibility

Operators upgrading would otherwise see client-supplied pricing overrides
silently stop applying with no diagnostic. Emit a debug-level line listing
the dropped fields and pointing at the opt-in flag when any are stripped;
stay silent on the no-op path so the log isn't filled with noise.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(proxy\): move pricing strip below the litellm_metadata JSON-string parse

The strip ran before the proxy parses ``litellm_metadata`` from a JSON
string into a dict \(a path used by multipart/form-data and ``extra_body``
callers\), so ``isinstance\(metadata, dict\)`` was False and ``model_info``
survived the strip. Move the call to the same post-parse position the
``user_api_key_*`` strip already uses for the same reason. Adds a
regression test exercising the JSON-string ``litellm_metadata`` path.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* test\(responses\): replace legacy claude-4-sonnet alias in multiturn tool-call test

Anthropic's main API no longer resolves the non-canonical 'claude-4-sonnet-20250514'
alias for freshly issued keys, returning 404 not_found_error. PR #27031 already
swept three other live tests pinned to this alias to claude-haiku-4-5-20251001
but missed test_multiturn_tool_calls in the responses API suite, which is now
failing reliably on PR CI runs \(e.g. PR #27074, job 1603363\).

Bump the two model references in test_multiturn_tool_calls to the same
claude-haiku-4-5-20251001 snapshot used by PR #27031 -- it covers everything
this test exercises \(tool calling, multi-turn\) and isn't on a deprecation
schedule.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* chore\(proxy\): close callback-config and observability-credential side channels

Two related gaps in the proxy's request bouncer:

1. ``is_request_body_safe`` \(auth_utils.py\) walked the request-body root
   and the ``litellm_embedding_config`` nested dict, but not ``metadata``
   or ``litellm_metadata``. The same fields it bans at root — Langfuse /
   Langsmith / Arize / PostHog / Braintrust / Phoenix / W&B Weave / GCS /
   Humanloop / Lunary credentials and routing — were silently accepted
   when the caller put them inside metadata, retargeting observability
   callbacks to a caller-controlled host with caller-supplied creds.
   Walk both metadata containers \(and parse the JSON-string form sent via
   multipart / ``extra_body``\) through the same banned-params helper, so
   the existing ``allow_client_side_credentials`` opt-in covers both
   paths consistently.

2. The banned-params list was hand-maintained and lagged the canonical
   ``_supported_callback_params`` allow-list in
   ``initialize_dynamic_callback_params``. Derive the observability bans
   from that allow-list \(minus a small ``_SAFE_CLIENT_CALLBACK_PARAMS``
   set for informational fields like ``langfuse_prompt_version`` and
   ``langsmith_sampling_rate``\) so future integrations are covered
   automatically; ``_EXTRA_BANNED_OBSERVABILITY_PARAMS`` carries the
   handful of fields integrations read but the allow-list hasn't caught
   up to. A guard test fails CI if a new entry is added to
   ``_supported_callback_params`` without an explicit safe-list decision.

Separately in ``litellm_pre_call_utils.py``: add ``callbacks``,
``service_callback``, ``logger_fn``, and ``litellm_disabled_callbacks``
to ``_UNTRUSTED_ROOT_CONTROL_FIELDS``. The first three are appended to
worker-wide ``litellm.{input,success,failure,_async_*,service}_callback``
lists / ``litellm.user_logger_fn`` from inside ``function_setup`` — one
request poisons every subsequent caller in that worker. The last is the
inverse primitive: the legitimate path reads it from key/team metadata,
the request-body version silently disables admin-configured audit /
observability for the call.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(auth\): per-param allow must continue, not return early

A pre-existing logic bug in ``_check_banned_params``: when the
deployment-level ``configurable_clientside_auth_params`` permitted one
banned field, the loop ``return``-ed on the first match instead of
``continue``-ing, so any other banned param later in the same body or
metadata dict was never checked. This PR's metadata walk multiplies the
surface where that bypass matters — a body pairing an allowed
``api_base`` with an observability credential like ``langfuse_host``
would silently pass.

Proxy-wide ``allow_client_side_credentials`` keeps ``return`` \(it's a
global opt-in for every banned param\). The per-param branch becomes
``continue`` so only the one explicitly-permitted field is skipped.

Adds a regression test that exercises the api_base + langfuse_host pair.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(vector_store\): resolve embedding config at request time, never persist creds

The vector store create/update path previously called
``_resolve_embedding_config`` against the admin-configured router/DB
model and persisted the resolved ``litellm_embedding_config`` dict
\(``api_key`` / ``api_base`` / ``api_version``\) into the
``litellm_managedvectorstorestable.litellm_params`` column. Because the
resolver expanded ``os.environ/...`` references via ``get_secret``, the
DB row carried cleartext provider credentials, and the
``/vector_store/{new,info,update,list}`` responses returned them to any
authenticated caller who could supply a known admin model name.

Move the auto-resolve out of ``create_vector_store_in_db`` and out of
the update path. Persist only the user-supplied ``litellm_embedding_model``
reference. Resolve at request-handling time inside
``_update_request_data_with_litellm_managed_vector_store_registry`` so
the resolved config lives in the per-request ``data`` dict and is
garbage-collected after the response. Legacy rows that were created by
an earlier proxy version and already carry a resolved
``litellm_embedding_config`` skip the re-resolution and pass through
unchanged so embedding calls keep working.

The ``new_vector_store`` response now also runs the existing
``_redact_sensitive_litellm_params`` masker \(already used by ``info``,
``update``, and ``list``\), defending against caller-supplied cleartext
on the create path and against legacy rows whose persisted credentials
are still in the database.

Existing tests that asserted the old write-time-resolve behaviour are
updated to assert the new persistence shape \(no embedding config
stored, just the model reference\). Two new tests cover the use-time
path: one asserting fresh resolution happens when a row carries only
the model reference, the other asserting legacy rows with persisted
config skip re-resolution and continue to work.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(vector_store\): tighten registry-mutation comment and dedupe test helpers

* fix\(vector_store\): cache use-time embedding-config resolution

Hold the resolved config in a process-memory TTL cache so the
request-handling path doesn't run litellm_proxymodeltable.find_first
on every vector-store call.

* fix\(anthropic,bedrock,vertex\): forward output_config.effort + 400 on garbage reasoning_effort

Follow-up bugs surfaced by the QA sweep on PR #27039
\(https://github.com/BerriAI/litellm/pull/27039#issuecomment-4363363610\).

1. Stop stripping output_config.effort on Bedrock + Vertex adaptive routes.
   - Vertex AI Claude 4.6/4.7 accepts output_config.effort on rawPredict
     \(verified end-to-end against us-east5 / global\). The strip helper now
     no-ops for effort.
   - Bedrock Converse routes output_config into additionalModelRequestFields
     for anthropic base models so the requested adaptive tier \(low/medium/
     high/xhigh/max\) actually reaches the wire instead of all collapsing to
     identical thinking.
   - Bedrock Invoke chat transformation \(AmazonAnthropicClaudeConfig\) stops
     popping output_config from the post-AnthropicConfig request body.
   - Bedrock Invoke /v1/messages allowlist \(BedrockInvokeAnthropicMessagesRequest\)
     now lists output_config so the runtime allowlist filter forwards it.

2. Validate effort across Bedrock Converse so 'disabled' / 'invalid' / '' /
   unsupported tiers \(xhigh/max on Sonnet 4.6 or budget-mode 4.5 models\)
   surface as a clean 400 BadRequestError instead of 500.

3. ValueError -> BadRequestError throughout \(AnthropicConfig.map_openai_params,
   _apply_output_config, AmazonConverseConfig._handle_reasoning_effort_parameter\).
   Empty-string effort is now rejected \(was silently passing the
   'if effort and ...' short-circuit\).

4. Floor reasoning_effort='minimal' at the Anthropic provider minimum
   \(1024 budget_tokens\) via new ANTHROPIC_MIN_THINKING_BUDGET_TOKENS so it's
   a usable tier on direct Anthropic / Azure AI Anthropic / Vertex AI Anthropic /
   Bedrock Invoke \(all of which 400 below 1024\).

5. model_prices: dedupe duplicate supports_max_reasoning_effort key on
   claude-opus-4-7 / claude-opus-4-7-20260416.

Adds regression tests across all five affected paths; existing tests asserting
the silent-strip behavior were updated to reflect the new pass-through and
clean 400 surfaces.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(constants\): make ANTHROPIC_MIN_THINKING_BUDGET_TOKENS a plain constant

The documentation CI test \(tests/documentation_tests/test_env_keys.py\)
asserts every os.getenv\(\) key in the source has a matching entry in the
litellm-docs config_settings.md table. ANTHROPIC_MIN_THINKING_BUDGET_TOKENS
tracks Anthropic's published wire-protocol minimum \(1024\) — it's not a
user-tunable, so making it env-overridable was wrong anyway. Drop the
os.getenv\(\) wrapper; the value is now a plain literal.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(anthropic,bedrock\): correct effort error message and dedupe effort_map

- Remove 'none' from the Bedrock _validate_anthropic_adaptive_effort error
  message; it was listed as a valid value but rejected by the membership
  check, leaving users in a feedback loop if they tried 'none'.
- Hoist the duplicated reasoning_effort -> output_config.effort mapping
  out of AnthropicConfig.map_openai_params and
  AmazonConverseConfig._handle_reasoning_effort_parameter into a single
  AnthropicConfig.REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT class constant
  so the two routes cannot drift.

* fix\(anthropic\): translate reasoning_effort on /v1/messages route

Closes the remaining QA-sweep gap on PR #27074: Bedrock Invoke
/v1/messages was silently ignoring ``reasoning_effort`` because the
shared param filter only kept native Anthropic keys, so every effort
tier collapsed to the same behavior on the wire \(27/231 cells failing
across opus-4-5 / opus-4-6 / sonnet-4-6\).

Map ``reasoning_effort`` to native Anthropic ``thinking`` /
``output_config.effort`` at the ``AnthropicMessagesConfig`` layer so
all four /v1/messages routes \(direct Anthropic, Azure AI, Vertex AI,
Bedrock Invoke\) inherit the same translation:

- Add ``reasoning_effort`` to ``AnthropicMessagesRequestOptionalParams``
  so the param filter in
  ``AnthropicMessagesRequestUtils.get_requested_anthropic_messages_optional_param``
  no longer drops it before the transformation runs.

- Add ``_translate_reasoning_effort_to_anthropic`` and call it from
  ``transform_anthropic_messages_request``. Mirrors
  ``AnthropicConfig.map_openai_params`` on the chat completion path
  \(re-uses ``_map_reasoning_effort`` and
  ``REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT``\) so the two routes
  cannot drift. Pops ``reasoning_effort`` so it never reaches the wire.

- Caller-supplied native ``thinking`` / ``output_config.effort`` always
  win — same precedence as
  ``_translate_legacy_thinking_for_adaptive_model``.

- Garbage values \(``""``, ``"disabled"``, ``"invalid"``\) raise
  ``AnthropicError\(status_code=400\)`` instead of falling through and
  surfacing as 500s from the provider.

- ``"none"`` clears thinking + output_config so callers can opt out
  per request.

Also restores the non-adaptive-model test coverage on Bedrock Invoke
/v1/messages that the previous commit lost when
``test_bedrock_messages_strips_output_config`` was renamed to the
``forwards`` variant on Opus 4.7.

Adds a new test file
``test_reasoning_effort_translation.py`` covering the translation at
the shared config level \(adaptive + non-adaptive models, none, garbage,
caller precedence\) so all four /v1/messages routes are exercised by a
single suite.

Adds parametrized + behavioral tests on the Bedrock Invoke /v1/messages
suite covering: minimal/low/medium/high/xhigh/max mapping for adaptive
models, thinking-budget mapping for non-adaptive Opus 4.5, ``none``
clears both, garbage raises 400, explicit ``output_config`` wins.

Refs: https://github.com/BerriAI/litellm/pull/27074

* fix\(anthropic,bedrock\): reject unmapped reasoning_effort at mapping site

Both the chat completion path \(AnthropicConfig.map_openai_params\) and the
Bedrock Converse path \(_handle_reasoning_effort_parameter\) used
REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT.get\(value, value\) which falls
back to the raw input on unmapped keys. Combined with _map_reasoning_effort
returning type='adaptive' for any string on Claude 4.6/4.7, garbage values
\(e.g. 'disabled'\) could leak into optional_params\['output_config'\]\['effort'\]
unvalidated if map_openai_params ran without the downstream transform_request
or _validate_anthropic_adaptive_effort check.

Mirror the /v1/messages pattern: use .get\(value\) \(no fallback\) and raise
BadRequestError immediately when the value is unmapped, co-locating
validation with the mapping for defense in depth.

* style: black formatting

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(anthropic\): stop class-attr leak; gate xhigh/max on every route

The reasoning-effort mapping dict was a public class attribute on
AnthropicConfig, so BaseConfig.get_config returned it as a request
parameter and every Anthropic-backed call \(Anthropic / Azure / Vertex /
Bedrock Invoke\) hit a 400 'REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT:
Extra inputs are not permitted' from the provider. Move the mapping
to a module-level constant.

_supports_effort_level only looked the model up under
custom_llm_provider='anthropic', so bedrock-prefixed model ids
\(e.g. bedrock/invoke/us.anthropic.claude-opus-4-7\) returned False
for both 'max' and 'xhigh' even when the underlying model entry has
the flag set. Strip known provider prefixes and retry the lookup
against litellm.model_cost directly so per-model gating works on
every route.

Mirror the per-model xhigh/max gate from
AnthropicConfig._apply_output_config in
AnthropicMessagesConfig._translate_reasoning_effort_to_anthropic so
the /v1/messages route also raises a clean 400 instead of forwarding
the unsupported tier.

* feat\(anthropic,bedrock\): strip output_config under drop_params for non-effort models

When a proxy fronts Claude Code \(which always sends `output_config.effort`\)
at a pre-4.5 Anthropic model — haiku-3, sonnet-3.5, opus-3, sonnet-4 — the
forwarded knob causes a forced 400 the client can't fix. Gating a strip
behind the existing `drop_params` flag lets operators opt into silent
fixup once and stop worrying about per-model param hygiene.

Default \(`drop_params=False`\) still forwards and surfaces the provider's
error, preserving the strict, debuggable contract from #27074.

Per https://platform.claude.com/docs/en/build-with-claude/effort the
supporting set is Opus 4.5+, Sonnet 4.6+, and Mythos Preview; everything
else is dropped \(with a verbose_logger warning so the strip is visible\).
Recognition uses model-name patterns plus a fallback to any
`supports_*_reasoning_effort` flag in the model map for forward
compatibility with new entries.

https://claude.ai/code/session_01WjHq31rvXT6xYNdVmSJvRp

\(cherry picked from commit 1233943e7861ba8a9062f792310ebd401cb03db8\)

* fix\(base_llm\): filter all _-prefixed class attrs from get_config

The drop_params strip work added `AnthropicConfig._EFFORT_SUPPORTING_MODEL_PATTERNS`
as a private class-level lookup tuple. `BaseConfig.get_config\(\)` only
filtered the `__`-prefixed names plus `_abc` / `_is_base_class`, so
`_EFFORT_SUPPORTING_MODEL_PATTERNS` would have leaked into the request
body the same way `REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT` did before
the previous commit.

Generalize the existing `_abc` / `_is_base_class` carve-outs to skip
every `_`-prefixed name. `AmazonConverseConfig.get_config\(\)` overrides
the base method, so apply the same change there.

Also unblocks future internal helpers from accidentally serialising into
the wire body.

* fix\(anthropic\): drive output_config.effort support from model map flags

Replace hardcoded _EFFORT_SUPPORTING_MODEL_PATTERNS with a JSON-backed
check that uses supports_*_reasoning_effort flags from the model map.
Add supports_minimal_reasoning_effort: true to opus-4-5 and mythos-preview
entries \(which previously only carried supports_reasoning\) so the JSON
remains the single source of truth for effort capability.

* fix\(anthropic,bedrock,databricks\): four reasoning_effort follow-ups

- claude-sonnet-4-6 + reasoning_effort=max no longer 400s. Renamed
  _is_opus_4_6_model to _is_claude_4_6_model at three sites and added
  supports_max_reasoning_effort: true to 12 model entries in the JSON
  cost map \(10 sonnet 4.6 ids + OpenRouter opus 4.6/4.7\).
- _map_reasoning_effort now raises BadRequestError\(400\) directly with
  llm_provider, instead of letting Databricks \(and similar callers\)
  surface its raw ValueError as a 500.
- output_config.effort on Opus 4.5 over Bedrock no longer 400s for
  missing effort-2025-11-24 beta. Flipped JSON to "effort-2025-11-24"
  for bedrock + bedrock_converse and added an auto-attach branch in
  _process_tools_and_beta for non-adaptive Anthropic + output_config
  on Converse.
- reasoning_effort=xhigh / =max on legacy budget-mode models
  \(Haiku 4.5, Sonnet 4.5, Opus 4.5\) now map to thinking.budget_tokens
  8192 / 16384 instead of returning 400. Added two constants in
  litellm/constants.py.

Tests updated for all four flips. Validated end-to-end via 306-cell
live proxy matrix \(6 model families x 3 routes x 17 effort cases\),
all pass.

* fix\(databricks\): validate reasoning_effort and set output_config on adaptive Claude

The Databricks path called `AnthropicConfig._map_reasoning_effort` for
Claude models but never validated the effort string nor set
`output_config.effort` for adaptive models \(Claude 4.6/4.7\). Since
`_map_reasoning_effort` returns `type=adaptive` for ANY non-None /
non-"none" string on adaptive models \(including "disabled",
"invalid", ""\), Databricks silently accepted garbage and emitted a
request without an `output_config.effort`, collapsing every adaptive
tier to identical behavior.

Match the Anthropic native, Bedrock Converse, Bedrock Invoke, and
/v1/messages paths: when the resolved `thinking` is non-None on a
4.6/4.7 model, look up the value in
`REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT` and either raise a clean
`BadRequestError` or set `optional_params\["output_config"\]`.

* fix\(azure\): omit model from image generation and image edit deployment requests

Azure OpenAI routes image gen/edit by deployment in the URL; sending the
deployment id in model breaks gpt-image-2 \(invalid_value\). Strip model from
JSON for deployments/.../images/generations and from multipart data for
.../images/edits. Non-deployment URLs \(e.g. Azure AI FLUX\) unchanged.

Fixes #26316.

Co-authored-by: Cursor <cursoragent@cursor.com>

* test\(azure\): exercise image gen JSON filter via HTTP client; dedupe image edit URL

- Image generation tests patch HTTPHandler.post / get_async_httpx_client so
  make_*_azure_httpx_request runs and wire json is asserted on call kwargs.
- Azure image edit: strip model in finalize_image_edit_multipart_data using the
  same URL string the handler passes to POST \(no second get_complete_url in
  transform\). BaseImageEditConfig default finalize is a no-op.

Co-authored-by: Cursor <cursoragent@cursor.com>

* fix\(azure_ai/anthropic\): promote output_config out of extra_body so validation runs

`azure_ai` is registered in `litellm.openai_compatible_providers`, so
`add_provider_specific_params_to_optional_params` \(litellm/utils.py\)
auto-stuffs any non-OpenAI kwarg \(e.g. `output_config={"effort": "..."}`\)
into `optional_params\["extra_body"\]`. `AzureAnthropicConfig.transform_request`
then strips `extra_body` entirely on the way out, silently dropping the
param — and `AnthropicConfig._apply_output_config` never sees it, so
`effort="invalid"` / `effort="xhigh"` on a non-supporting model
quietly reaches the model with default behavior instead of returning a
clean 400 \(as the native `anthropic` provider does\).

Promote the keys back to top-level `optional_params` \(using `setdefault`
so explicit top-level values win\) before delegating to the parent
`AnthropicConfig`. Apply in both `validate_environment` and
`transform_request` so flag detection \(`is_mcp_server_used`, etc.\) and
output-config validation both run.

Surfaced by the QA matrix expansion on PR #27074: 20 cells where Azure
returned 200 while `anthropic` returned 400 — all `output_config` mode
across haiku_4_5, sonnet_4_5, opus_4_5, sonnet_4_6, opus_4_6, opus_4_7
families with `effort` in {invalid, xhigh, max, low, medium, high}.

Tests:
* `test_output_config_promoted_from_extra_body`: valid effort reaches data
* `test_invalid_output_config_effort_raises_via_extra_body`: 400 on bad effort
* `test_unsupported_effort_xhigh_raises_via_extra_body`: 400 on xhigh-on-Sonnet-4.6
* `test_extra_body_promotion_does_not_clobber_top_level`: setdefault semantics

* test\(image_gen\): expect no model in Azure image edit multipart \(#26316\)

Align test_azure_image_edit_litellm_sdk with deployment-scoped Azure edits.

Co-authored-by: Cursor <cursoragent@cursor.com>

* refactor\(anthropic\): extract _validate_effort_for_model to prevent drift

The chat completion path \(`_apply_output_config`\) and the /v1/messages
pass-through \(`AnthropicMessagesConfig._translate_reasoning_effort_to_anthropic`\)
both gate `max` / `xhigh` per model. The two sites had diverged from
near-identical copies into separately maintained blocks, creating a real
drift risk when a new model tier \(e.g. Claude 4.8\) lands -- a contributor
could update one site and miss the other.

Centralise the gating in `AnthropicConfig._validate_effort_for_model`,
which returns an error message string or `None`. Each call site keeps
its own provider-appropriate exception type \(`BadRequestError` for the
chat path, `AnthropicError` for the /v1/messages pass-through\) but the
gating decision now comes from one place. Net -11 LOC.

Adds a parametrised unit test exercising the helper directly across
4.5 / 4.6 / 4.7 model families and `max` / `xhigh` / lower-effort
inputs. Existing tests at both call sites continue to pass unchanged.

Addresses Greptile finding on PR #27074.

* fix\(databricks\): narrow reasoning_effort_value to str for mypy

`non_default_params.get\("reasoning_effort"\)` returns `Any | None`,
but `REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT.get\(\)` expects `str`.
Mypy flagged this on the strict pass. Narrow with `isinstance` before
the lookup; non-strings fall through to the existing `BadRequestError`
below with a clean validation message, so behavior is unchanged.

Fixes a regression introduced by 1a10746e95 in this PR.

* feat\(proxy\): add health_check_reasoning_effort for model health checks

Co-authored-by: Cursor <cursoragent@cursor.com>

* test\(image_gen\): align Azure image gen fixture with body omitting model

Expected JSON matches deployment-scoped Azure POST \(#26316\).

Co-authored-by: Cursor <cursoragent@cursor.com>

* test\(anthropic/chat\): force PR-local model_cost map via autouse fixture

CI runs without LITELLM_LOCAL_MODEL_COST_MAP=True, so litellm.model_cost
is loaded from main-branch JSON \(default model_cost_map_url\) instead of
the PR's checked-out model_prices_and_context_window.json. Tests that
assert per-model flags added in this PR \(supports_max_reasoning_effort,
supports_xhigh_reasoning_effort\) therefore pass locally but fail in CI
with 'AssertionError: assert False is True' on 5 cases:

  - test_anthropic_model_supports_effort_param_recognizes_supporting_models
    \[anthropic.claude-mythos-preview, bedrock/.../mythos-preview,
     claude-opus-4-5-20251101\]
  - test_supports_effort_level_handles_provider_prefixes
    \[bedrock/invoke/us.anthropic.claude-sonnet-4-6-max-True,
     claude-sonnet-4-6-max-True\]

Add an autouse fixture at tests/test_litellm/llms/anthropic/chat/conftest.py
that monkey-patches litellm.model_cost to the PR-local JSON for every test
in this directory. The parent conftest already snapshots+restores
litellm.model_cost per-function, so the mutation is contained.

This is a scoped workaround. The proper fix is to set the env var
globally in the test workflow once the ~10 inline self-set test files
are audited; tracking that as a follow-up issue.

* \[Fix\] Docker: Pin Wolfi And Uv To Multi-Arch Index Digests

The previous pins resolved to single-platform amd64 manifests, so buildx
pulled the same amd64 base for both linux/amd64 and linux/arm64 targets.
The published OCI index then advertised an arm64 entry whose layers are
byte-identical to amd64 -- arm64 users got an amd64 binary.

Switch all three Dockerfiles to the multi-arch image-index digests:
  - cgr.dev/chainguard/wolfi-base   \(index has linux/amd64 + linux/arm64\)
  - ghcr.io/astral-sh/uv:0.11.7     \(index has linux/amd64 + linux/arm64\)

Resolved with `docker buildx imagetools inspect <ref>` -- that returns
the index digest. `docker pull` + `docker inspect` returns the per-host
platform digest, which is what slipped in last time.

* \[Fix\] Docker: Pin Uv To Multi-Arch Index Digest In Remaining Dockerfiles

Apply the same fix to the three Dockerfiles not in the release pipeline
today \(alpine, dev, health_check\) so they stay correct if/when they're
built for arm64 in the future.

Wolfi pins are not present in these files; the python:3.11-alpine and
python:3.13-slim digests they already use are multi-arch indexes that
include arm64/v8, so only the uv pin needed swapping.

* fix\(xai\): fold reasoning_tokens into completion_tokens to satisfy OpenAI invariant

xAI's chat completions API accounts reasoning_tokens separately from
completion_tokens, but rolls them into total_tokens. This breaks the
OpenAI invariant total_tokens == prompt_tokens + completion_tokens
that downstream consumers \(including litellm's own _usage_format_tests
in tests/llm_translation/base_llm_unit_tests.py:58\) rely on.

Live capture \(grok-3-mini-beta, 2026-05-04\):
    prompt=14, completion=10, total=336, reasoning=312
    14 + 10 = 24, NOT 336.

OpenAI's o1/o3 reasoning models include reasoning_tokens in
completion_tokens, leaving the prompt+completion=total invariant
intact. xAI deviates. This patch aligns xAI to OpenAI semantics by
folding reasoning_tokens into completion_tokens after the parent
OpenAI parser runs.

The fold is idempotent and defensive:
- Only fires when total_tokens == prompt_tokens + completion_tokens
  + reasoning_tokens \(the documented xAI shape\). Refuses to fold if
  the gap doesn't match, guarding against silent corruption when xAI
  changes accounting.
- Skips if completion_tokens already covers the gap \(already
  normalised — e.g. cost calc replays a previously-folded Usage\).

xai.cost_calculator.cost_per_token already added reasoning_tokens to
the visible completion count for billing. Post-fold the Usage block
now satisfies that invariant directly, so the cost calc would
double-bill. Updated cost_per_token to detect the OpenAI-normalised
shape \(total == prompt + completion\) and skip the reasoning add-on
in that case, falling through to the legacy raw-shape behaviour for
callers that bypass the transformation \(e.g. proxy log replay\).

Tests:
- Adds TestXAIReasoningTokenFolding covering: gap-explained-fold,
  idempotent-no-double-fold, no-reasoning-skip, gap-mismatch-skip.
- Adds test_already_normalised_usage_does_not_double_count_reasoning
  to lock the cost-calc idempotency.
- Updates 7 pre-existing cost-calc tests whose total_tokens was
  internally inconsistent \(used the OpenAI-normalised total but kept
  reasoning_tokens external\) to use the documented xAI raw shape
  total = prompt + visible completion + reasoning. Pre-existing
  values masked the missing-fold by accident.

Verified end-to-end against the live xAI API:
    LITELLM_LOCAL_MODEL_COST_MAP=False \(CI default\) +
    XAI_API_KEY set +
    pytest tests/llm_translation/test_xai.py::TestXAIChat::test_prompt_caching
        -> PASSED in 18.81s \(was: AssertionError on
        usage.total_tokens == usage.prompt_tokens + usage.completion_tokens\)

20/20 tests in tests/test_litellm/llms/xai/test_xai_cost_calculator.py
and 8/8 in tests/test_litellm/llms/xai/test_xai_chat_transformation.py
pass.

* refactor\(bedrock/converse\): delegate effort gating to AnthropicConfig._validate_effort_for_model

Removes the duplicated max/xhigh gating logic in
_validate_anthropic_adaptive_effort and the now-unused
_supports_effort_level_on_bedrock helper. Per-model gating now flows
through the centralized AnthropicConfig._validate_effort_for_model
\(whose _supports_effort_level already strips Bedrock prefixes\), so the
chat completion, /v1/messages, and Bedrock Converse paths can't drift
when a new gated effort tier is added.

* Implement normalize_nonempty_secret_str function to trim whitespace from secrets and treat empty values as unset. Update proxy_server to use this function for Grafana credentials. Enhance tests to validate the new normalization behavior.

* Fix qdrant semantic cache miss metadata

* chore\(deps\): refresh dependency locks

* chore\(deps\): authorize pytest license

* fix: preserve tokenizer decode round trips

* refactor\(anthropic\): drive adaptive-thinking gate via supports_adaptive_thinking flag

Three of greptile's open comments on #27074 \(P2 converse:512, P1
databricks:361, and the underlying capability-flag policy rule\) flagged
the same pattern: _is_claude_4_6_model\(...\) or _is_claude_4_7_model\(...\)
used inline as a runtime 'is this an adaptive-thinking model?' check.
That requires a code release each time a new adaptive Claude lands.

Consolidate the inline gating to AnthropicModelInfo._is_adaptive_thinking_model,
and switch the helper itself to read a new supports_adaptive_thinking
flag from `model_prices_and_context_window.json` via `_supports_factory`,
falling back to the family pattern only when the model-map entry doesn't
carry the flag \(preserves OpenRouter / Vercel / Bedrock-prefixed variants
that route through the same code path with non-canonical ids\).

Adds `supports_adaptive_thinking: true` to the four 4.6/4.7 anthropic
entries \(opus-4-6 + dated, opus-4-7 + dated, sonnet-4-6\). Bedrock-prefixed
and Vertex-prefixed entries don't need the flag because both fall back
through the family pattern \(the helper short-circuits early on True from
either path\) and the bedrock/vertex Claude IDs all match the existing
opus-4-{6,7} / sonnet-4-{6,7} pattern.

Affected call sites:

- `bedrock/chat/converse_transformation.py:_handle_reasoning_effort_parameter`
- `anthropic/chat/transformation.py:_map_reasoning_effort`
- `anthropic/chat/transformation.py:map_openai_params` \(output_config branch\)
- `databricks/chat/transformation.py:map_openai_params` \(output_config branch\)

The remaining `_is_claude_4_6_model` / `_is_claude_4_7_model` references
in `AnthropicConfig._validate_effort_for_model` and
`AnthropicConfig.get_supported_openai_params` are intentionally retained:
they're per-model gating fallbacks for variants whose model-map entries
don't yet carry the `supports_max_reasoning_effort` /
`supports_reasoning` flag. Those are documented in-place.

Tests: 537 anthropic/bedrock/databricks/vertex/messages tests pass.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* chore\(deps\): address dependency review notes

* test\(model_prices\): add supports_adaptive_thinking to schema

`test_aaamodel_prices_and_context_window_json_is_valid` validates the
model-map JSON against an explicit schema with `additionalProperties`,
so the new `supports_adaptive_thinking` flag added in
98ced0ae43 needs a matching schema entry.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* refactor: remove unnecessary comments from #27074

Strip out the explanatory and historical comments that don't carry
business-logic justification. Comments that simply narrate what code
does — or that explain prior behavior, what was changed, or which PR
introduced a fix — are removed. Docstrings are reduced to a one-line
summary where the long form repeated information already evident from
the code or test data.

No code-behavior changes. All 643 affected unit tests still pass.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* test: keep decode token test local

* chore\(deps\): align dashboard node engine

* feat: selectively apply routing strategy according to model name

* style: make _model_supports_effort_param more concise

* refactor\(anthropic,bedrock\): hoist drop_params output_config warning to module constant

Three call sites \(anthropic chat, bedrock converse, bedrock invoke messages\)
emitted the same '...Effort is only supported on Opus 4.5+, Sonnet 4.6+, and
Mythos Preview' warning verbatim. Extract DROP_UNSUPPORTED_OUTPUT_CONFIG_WARNING
in litellm/llms/anthropic/chat/transformation.py and import it from the bedrock
sites so future copy edits live in one place.

Addresses Michael's review on PR #27074.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* refactor\(anthropic,bedrock,databricks\): factor BadRequestError for unknown reasoning_effort

Three call sites raised the same BadRequestError\("Invalid reasoning_effort:
... Must be one of 'minimal', 'low', ..."\) block when REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT
returned None: anthropic chat map_openai_params, bedrock converse
_handle_reasoning_effort_parameter, and databricks chat reasoning_effort path.

Extract AnthropicConfig._raise_invalid_reasoning_effort\(model, value, llm_provider\)
so future copy edits / valid-set changes happen in one place. Typed as NoReturn
so type-checkers correctly narrow control flow at call sites.

Addresses Michael's review on PR #27074.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* Clean up Redis semantic cache isolation fallback

* fix\(guardrails\): align banned_keywords + azure_content_safety call_type gates with runtime route_type

The hooks gated on ``call_type == "completion"`` but the proxy ingress
passes ``route_type`` straight through as ``call_type`` —
``"acompletion"`` for /v1/chat/completions and ``"aresponses"`` for
/v1/responses. Tests passed because they used the literal sync
``"completion"`` value, masking the gap.

Switch both hooks to ``is_text_content_call_type`` \(matches the
canonical runtime values: completion / acompletion / aresponses\) and
update existing tests to assert against runtime values, plus parametrize
a regression test that pins the gate.

* fix: remove unused import

* Add semantic cache legacy migration flag

* Treat 0 team_member_budget as no cap

* chore\(caching\): annotate qdrant quantization_params dict type

Mypy infers the dict's value type from the first branch
\(Dict\[str, bool\]\) which clashes with the scalar branch's mixed-type
inner dict. Explicit Dict\[str, Any\] annotation lifts the inference.

* chore\(caching\): remove allow_legacy_unscoped_cache_hits opt-in

The flag was an opt-in escape hatch for the cross-tenant leak the rest
of the patch closes — flipping it on \(env var or constructor param\)
re-enables exactly the VERIA-54 primitive on either backend. There is
no operational need that the secure path doesn't already meet:

- Qdrant: legacy points without ``litellm_cache_key`` payload are
  excluded by the must-clause filter and treated as misses; new sets
  populate the cache key, so cold-start lasts only as long as the
  natural cache rebuild.
- Redis: existing unscoped index can't carry the new schema; the init
  path falls back to ``{name}_isolated`` \(and recreates it on stale
  schema\), leaving the legacy index untouched.

Drop the construc…")  | May 5, 2026  |  
|   |   |  [[Infra] Promote internal staging to main (](https://github.com/BerriAI/litellm/commit/6ff668c7aa01a73738ed39aa64913a089a183565 "\[Infra\] Promote internal staging to main \(#27245\)

* default requested_model to empty string on litellm-side rejects

* Update litellm/router.py

Co-authored-by: greptile-apps\[bot\] <165735046+greptile-apps\[bot\]@users.noreply.github.com>

* fix: scope key access_group_ids override by team's assigned groups

A team member could set any access_group_ids on their key \(e.g. a group
assigned only to a different team\) and override the team's model
restriction. Intersect the key's access_group_ids with team_object.access_group_ids
in _key_access_group_grants_model so foreign groups are dropped before
model expansion. Adds a regression test that asserts expansion is never
called for foreign groups.

* \[Fix\] Proxy: Skip Personal Budget Hook When Reservation Covers Counter

The reservation path \(PR #26845\) atomically pre-fills `spend:user:{user_id}`
and admits at the strict-`<` boundary. The legacy `_PROXY_MaxBudgetLimiter`
pre-call hook re-reads the same counter with `>=`, so a reservation that
fills the counter to exactly `max_budget` \(e.g. a request without a
`max_tokens` cap that falls back to reserving the smallest remaining
headroom\) is rejected by the hook even though the reservation already
admitted it.

Skip the hook when the request's active `budget_reservation` covers
`spend:user:{user_id}`. The reservation is the source of truth for that
counter cross-pod; the legacy `>=` path remains in place for requests
without a reservation \(e.g. paths that bypass the reservation entirely\).

Reproduces as `tests/otel_tests/test_prometheus.py::test_user_budget_metrics`
on a fresh user with `max_budget=10` calling `fake-openai-endpoint` without
`max_tokens`. Adds focused unit coverage in
`tests/test_litellm/proxy/hooks/test_max_budget_limiter.py`.

* harden bedrock file bucket validation

* Fix syntax errors from botched merge in router.py

* Fix Vertex batch output edge cases

* \[Fix\] RBAC: Drop management_routes Write Fallback for Admin Viewer

Greptile P1: the unsafe-method branch of `_check_proxy_admin_viewer_access`
ended with a blanket `if route in management_routes: return`. That set is a
mix of reads \(info/list — handled via the safe-method GET branch above\) and
writes. The fallback let Admin Viewer POST to write endpoints not enumerated
in `_ADMIN_VIEWER_BLOCKED_WRITE_ROUTES`, including:
  - /team/block, /team/unblock, /team/permissions_update
  - /jwt/key/mapping/{new,update,delete}
  - /key/bulk_update
  - /key/{key_id}/reset_spend

Remove the fallback. The two remaining allow sets \(admin_viewer_routes and
global_spend_tracking_routes\) are both read-only, so removal does not affect
the legitimate POST-as-read cases \(e.g. /spend/calculate, which is in
spend_tracking_routes ⊂ admin_viewer_routes\).

Tests:
  - 8 new parametrized cases pinning each previously-leaking management write
    endpoint to 403 on POST for PROXY_ADMIN_VIEW_ONLY.

* fix\(tests\): anchor VCR redis cassette key to repo root

`os.path.relpath` with no `start` arg uses the current working
directory, so running pytest from a subdirectory produced a
different Redis key than running from the repo root. CI-recorded
cassettes and locally-replayed runs would silently miss each
other's cache.

Anchor the path to the repo root \(derived from `__file__`\) so the
key is stable regardless of CWD.

https://claude.ai/code/session_018uCx7pcrkdUJZrCVMaTdPx

* fix: gate key access_group override on group's own assignment

Replaces the previous intersect-with-team.access_group_ids check, which
made the override unreachable in practice \(the team-gate fallback already
covered every case the intersection allowed\). The override now resolves
each of the key's access_group_ids via get_access_object and accepts the
group only if its assigned_team_ids includes the key's team_id, or its
assigned_key_ids includes the key's token. This fulfills the original ask
\(a key can extend a team's allow-list via a group the admin granted to
that team or that specific key\) while still rejecting foreign groups
referenced by team members of other teams.

* \[Fix\] Proxy/Key Management: Honor team_member_permissions /key/list In /key/list Endpoint

When a team grants /key/list via team_member_permissions, non-admin members
should see all keys for that team — same as a team admin. Previously the
classification in list_keys\(\) only checked admin status, so permitted
members fell into the service-account-only path and could not see other
members' personal keys. Routes those members into the full-visibility set.

* Fix access-group bypass via litellm-model fallback path

When _get_all_deployments returns 0 candidates and the litellm-model
fallback branch \(_get_deployment_by_litellm_model\) finds deployments that
the access-group filter then empties, _access_group_filter_emptied_candidates
remained False \(it was captured before that branch ran\). The router would
then proceed to default fallbacks; the fallback model could have no
access_groups and short-circuit the filter, silently serving a caller
blocked by access-group restrictions.

Update the flag inside the litellm-model branch when filtering empties a
non-empty candidate set so the default-fallback guard still triggers.

* fix\(proxy\): redact MCP server URL and headers for non-admin viewers \(VERIA-8\)

Many MCP integrations \(Zapier, etc.\) embed an upstream API key
directly in the server URL, e.g.
``https://actions.zapier.com/mcp/<api-key>/sse``. The list and
single-server endpoints were returning the full URL to any
authenticated user — `_redact_mcp_credentials` only stripped the
explicit ``credentials`` field, and `_sanitize_mcp_server_for_virtual_key`
only ran for restricted virtual keys. Non-admin internal users could
read the dashboard, click the unmask toggle, and exfiltrate the raw
token.

Add `_sanitize_mcp_server_for_non_admin` that runs on top of the
existing credential redaction and clears the credential-bearing
fields:

- ``url`` \(the primary leak vector\)
- ``spec_path`` \(OpenAPI spec URLs that may carry tokens\)
- ``static_headers`` / ``extra_headers`` \(Authorization\)
- ``env`` \(arbitrary secrets\)
- ``authorization_url`` / ``token_url`` / ``registration_url``

Identity fields \(``server_id``, ``alias``, ``mcp_info``, etc.\) are
preserved so the UI can still list servers a non-admin's team has
access to.

Apply the new sanitizer in `fetch_all_mcp_servers` and the per-server
fetch path right after the existing virtual-key branch. Update the
existing `test_list_mcp_servers_non_admin_user_filtered` assertions
that previously checked URL visibility.

Frontend defense-in-depth: hide the URL unmask toggle on
`mcp_server_view.tsx` unless the viewer is a proxy admin.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* Fix runtime policy attachment initialization

Mark runtime-created policies and attachments initialized so global policy attachments created from the policy builder apply immediately without requiring a restart.

Co-authored-by: Cursor <cursoragent@cursor.com>

* test\(router\): cover _try_early_resolve_deployments_for_model_not_in_names

The router_code_coverage CI check requires every function in router.py to
be referenced by at least one test under tests/{local_testing,
router_unit_tests,test_litellm} in a file with "router" in its name.
The recently-extracted helper had no direct test, so the check failed
with "0.45% of functions in router.py are not tested".

Add a focused test that exercises the four return paths: model already
in self.model_names, no fallback applies, pattern-router match, and
default_deployment substitution \(also asserting the stored default
isn't mutated\).

https://claude.ai/code/session_019AVp1XL7RT9RxRe4qRLkay

* Fix policy registry teardown in tests

Reset the policy ID index during policy engine test cleanup so stale policy versions cannot leak between tests.

Co-authored-by: Cursor <cursoragent@cursor.com>

* fix\(batches\): count non-chat tokens, validate batch-file model access \(VERIA-39\) \(#27015\)

* fix\(batches\): count non-chat tokens and validate every model in batch file

Two security control bypasses on POST /v1/batches:

1. `_get_batch_job_input_file_usage` only summed tokens for
   `body.messages` \(chat completions\). Embedding \(`input`\) and text
   completion \(`prompt`\) batches reported zero, letting massive
   non-chat workloads slip past TPM rate limits. Extend the counter
   to handle string and list shapes for both fields.

2. The batch input file was forwarded to the upstream provider
   without inspecting the models named inside the JSONL — only the
   outer `model` query parameter was checked against the caller's
   allowlist. A caller restricted to gpt-3.5 could submit a batch
   targeting gpt-4o and the upstream would execute it under the
   proxy's shared API key.

Add `_get_models_from_batch_input_file_content` \(returns the
distinct `body.model` values\) and call it from
`_enforce_batch_file_model_access` in the pre-call hook, which runs
each model through `can_key_call_model` so the same allowlist
semantics \(wildcards, access groups, all-proxy-models, team aliases\)
the proxy enforces on `/chat/completions` apply here too. Any
unauthorized model raises a 403 before the file is forwarded.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(batches\): count pre-tokenized prompt/input shapes, classify 403 logs

Two follow-ups from the Greptile review on the batch validation PR:

1. P1 TPM bypass via integer token arrays. The OpenAI batch schema
   accepts ``prompt`` and ``input`` as ``list\[int\]`` \(a single
   pre-tokenized prompt\) or ``list\[list\[int\]\]`` \(multiple\) in addition
   to the string and ``list\[str\]`` shapes. Pre-fix only the string
   shapes were counted, so a caller could submit a batch with hundreds
   of millions of pre-tokenized tokens and the rate limiter would
   record zero. Extract the per-field logic into
   ``_count_prompt_or_input_tokens`` and count each int as one token.

2. P2 access-denial logs were indistinguishable from I/O failures.
   ``count_input_file_usage`` caught every exception under a generic
   "Error counting input file usage" message, so an intentional 403
   from ``_enforce_batch_file_model_access`` looked the same in the
   logs as a missing file or a Prisma timeout. Catch ``HTTPException``
   separately and log 403s at WARNING level with a security-relevant
   message before re-raising.

Tests cover the new shapes: single ``list\[int\]``, ``list\[list\[int\]\]``
\(the worst-case bypass vector\), and embeddings ``input`` with
pre-tokenized arrays.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

---------

Co-authored-by: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(proxy\): re-validate user_id after /user/info re-parses query \(#27009\)

* fix\(proxy\): re-validate user_id ownership after /user/info re-parses query

The route-level access check in `RouteChecks.non_proxy_admin_allowed_routes_check`
reads `request.query_params.get\("user_id"\)`, which decodes literal `+` to
spaces. The endpoint then re-parses the raw query string with `urllib.unquote`
in `get_user_id_from_request` to preserve `+` characters \(so plus-addressed
emails work as user_ids\). Those two paths produce different ids: a caller
who registered a user_id containing a literal space could pass the route
check and then read another user's row by sending the encoded `+` form.

Add `_enforce_user_info_access` and call it after `_normalize_user_info_user_id`
returns the final id. Proxy admin / view-only admin still bypass; everyone
else must match the resolved user_id \(or have no user_id, which falls back
to the caller's own id later in the handler\).

Tests cover the admin bypass, owner-match path, and the cross-user lookup
that this change blocks.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(proxy\): apply user_info ownership check to PROXY_ADMIN_VIEW_ONLY

`_enforce_user_info_access` was bypassing both PROXY_ADMIN and
PROXY_ADMIN_VIEW_ONLY, but the upstream route check in
`RouteChecks.non_proxy_admin_allowed_routes_check` only treats
PROXY_ADMIN as a true admin for the `/user/info` route — view-only
admins go through the `user_id == valid_token.user_id` enforcement
along with regular users. Mirroring that asymmetry left the same
encoded-`+` bypass open for view-only admins whose user_id contains a
literal space.

Drop the PROXY_ADMIN_VIEW_ONLY exemption so the post-decode re-check
matches the upstream rule. Update tests: a view-only admin must now
be blocked from cross-user lookups but still allowed to read their
own row.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

---------

Co-authored-by: yuneng-jiang <yuneng@berri.ai>
Co-authored-by: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* feat\(spend-logs\): opt-in suppression of stack traces in spend-tracking error logs

Adds LITELLM_SUPPRESS_SPEND_LOG_TRACEBACKS env var. When set to true and the
proxy log level is INFO or above, spend-tracking error paths emit a single
ERROR line without the full traceback. Stack traces are preserved at DEBUG
and the Sentry / proxy_logging_obj.failure_handler path is unchanged.

The new spend_log_error helper is wired through the spend write hot path:
  - DBSpendUpdateWriter \(update_database, _update_*_db, batch upsert,
    redis-commit fallbacks\)
  - _ProxyDBLogger._PROXY_track_cost_callback
  - get_logging_payload exception path
  - update_spend / update_daily_tag_spend / spend logs queue monitor

Resolves LIT-2704.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(spend-logs\): preserve no-traceback behavior for update_daily_tag_spend

This call site previously logged a single-line error via verbose_proxy_logger.error\(\)
with no traceback. Switching it to spend_log_error\(..., exc=e\) caused a full stack
trace to render by default \(when LITELLM_SUPPRESS_SPEND_LOG_TRACEBACKS is unset\),
which contradicts the PR goal of leaving default behavior unchanged. Revert this
specific site to the original error log call.

* fix\(spend-logs\): preserve no-traceback behavior for update_daily_tag_spend

Bugbot caught a regression: the previous error log here was a single-line
verbose_proxy_logger.error\(...\) with no traceback. spend_log_error attaches
the active exception's traceback by default \(when the suppression env var
is unset\), so swapping it in changed default behavior. Revert this one site
to its original .error\(\) call to keep the PR strictly opt-in.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* feat\(spend-logs\): suppress traceback in SpendLogs error_information row

Extend LITELLM_SUPPRESS_SPEND_LOG_TRACEBACKS to the failure callback so the
per-row Metadata pane in the UI no longer shows the stack trace when the
opt-in env var is set, matching the existing console-side suppression.

https://claude.ai/code/session_014dztoRbRnRvq54HL9EyHx6

* \[Fix\] Proxy: Repair Merge Fallout In Router-Override Fallback Auth

Conflict resolution for #26968 dropped the `Iterator` typing import
\(NameError at module load\), left a dead `fallback_models = cast\(...\)`
block, and the new tests called `_enforce_key_and_fallback_model_access`
without the now-required `request` kwarg.

* isolate dual OTEL handlers

* harden cloud file compatibility path

* harden cloud file compatibility path

* \[Fix\] Proxy/Key Management: Align Key-Org Membership Checks On Generate And Regenerate

Mirrors the membership rule on /key/update so that /key/generate and
/key/{key}/regenerate apply the same `_validate_caller_can_assign_key_org`
gate when the caller specifies an `organization_id`. Proxy admins bypass.
The check no-ops when `organization_id` is not being set.

* thread trusted params through vertex file content

* trust only server legacy file flag

* chore\(proxy\): keep public AI hub unauthenticated

* fix\(proxy\): preserve low-detail readiness status

* \[Test\] Anthropic: Replace Legacy Claude-4-Sonnet Alias With Haiku 4.5

Three live-API tests pinned to claude-4-sonnet-20250514, which is a
non-canonical alias of claude-sonnet-4-20250514. Anthropic's main API
no longer resolves the legacy form under freshly issued keys, so the
tests fail with not_found_error. The token counter test pinned to
claude-sonnet-4-20250514 itself \(deprecation_date 2026-05-14, two weeks
out\) was on borrowed time too.

Bump all four to claude-haiku-4-5-20251001 — capability superset for what
these tests exercise \(streaming, parallel tool calling, extended thinking,
token counting\), no upcoming deprecation, cheaper per-token.

* chore\(proxy\): move URL-valued model/file_id guard from SDK to proxy

The previous per-provider guards in HuggingFace, Oobabooga, and Gemini
files lived in the SDK layer, breaking SDK callers who legitimately pass
URL-valued model identifiers. Move the check to the proxy boundary in
add_litellm_data_to_request so SDK users keep working while proxy users
default-deny URL-valued model and file_id, with admin opt-in via
litellm.provider_url_destination_allowed_hosts.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* \[Chore\] Proxy/UI: Drop stray _experimental/out/chat/index.html

This file is a regenerable UI build artifact that should not be tracked
in source. Removing so the merge into litellm_internal_staging stays clean.

* \[Test\] Anthropic Passthrough: Bump Streaming Cost-Injection Test To Haiku 4.5

test_anthropic_messages_streaming_cost_injection hits the proxy's
/v1/messages route, which routes via the anthropic/* wildcard to
api.anthropic.com. The 404 surfaced in the test was Anthropic's own
not_found_error propagated back through the proxy \(visible from the
x-litellm-model-id hash on the response — the proxy did route\).

Same root cause as the prior commit: the legacy claude-4-sonnet-20250514
alias is no longer recognized by Anthropic's main API under the new key.
Swap to claude-haiku-4-5-20251001 — same routing path, canonical model.

* fix\(proxy\): handle ownership-recording failures after upstream create

If record_container_owner raises after the upstream container is created,
the user previously got a 500 with no usable container — they were billed
for an unreachable resource. Move ownership recording into the create
path's exception handling and split the two failure modes:

- HTTPException from the recorder \(auth conflicts\) propagates verbatim
  so the client sees the real status code, not a generic LLM error.
- Unexpected exceptions are logged and swallowed; the response is
  returned to the caller so they aren't billed for a container they
  can't address. The DB row stays untracked until an operator reconciles.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(guardrails\): close post-call coverage gaps

* fix\(types\): add /team/permissions_bulk_update to management_routes

The blocklist check in _check_proxy_admin_viewer_access only fires for
routes that match LiteLLMRoutes.management_routes — the bulk-update
endpoint was missing from that list, so the test for view-only admins
on /team/permissions_bulk_update fell through to "allow."

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* \[Test\] Anthropic Passthrough: Bump Thinking Tests Off Legacy Sonnet 4 Alias

base_anthropic_messages_test.test_anthropic_messages_with_thinking and
test_anthropic_streaming_with_thinking still pinned to
claude-4-sonnet-20250514 — the same legacy alias Anthropic no longer
recognizes under freshly issued keys. The other four tests in this base
class already use claude-sonnet-4-5-20250929; these two were missed.

Bump to claude-haiku-4-5-20251001 \(supports_reasoning=true, no upcoming
deprecation\). Subclasses including TestAnthropicPassthroughBasic
inherit these methods.

* fix\(guardrails\): cover multi-choice output variants

* fix\(proxy\): preserve public ai hub ui setting

* fix\(scim\): cascade FK cleanup on user delete and surface block status in UI

SCIM DELETE /Users/{id} previously called litellm_usertable.delete without
clearing rows that FK back to the user, so Postgres rejected the delete with
LiteLLM_InvitationLink_user_id_fkey and the SCIM caller saw a 500. Add a
helper to drop invitation_link, organization_membership, and team_membership
rows before the user delete \(mirrors /user/delete in internal_user_endpoints\).

Also add a Status column to the Virtual Keys and Internal Users tables so
admins can see at a glance which keys are blocked and which users SCIM has
deactivated. SCIM-blocked keys carry a tooltip explaining the origin.

Pin the dashboard's Node version to 20 via .nvmrc to match CI.

* chore: update Next.js build artifacts \(2026-05-02 03:21 UTC, node v20.20.2\)

* perf\(proxy\): cache container/skill ownership reads on the hot path

Container ownership and skill rows are looked up on every retrieve /
delete / list / file-content / chat-completion-with-skill call. The new
stores wrapped raw Prisma queries with no cache, putting one DB
round-trip on each request. Add an in-process TTL'd cache mirroring the
_byok_cred_cache pattern in mcp_server/server.py: per-key \(value,
monotonic_timestamp\), 60s TTL, 10000-entry cap with full-clear on
overflow, invalidated by every write. Negative results \(`None`\) are
cached too so untracked-resource checks also skip the DB.

Tests cover: cache-after-first-hit, negative caching, write
invalidation, no-caching-on-DB-error, TTL expiry, capacity eviction.
56 tests pass.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* chore: update Next.js build artifacts \(2026-05-02 03:39 UTC, node v20.20.2\)

* fix: remove traceback key instead of it being ""

* fix: linting error

* fix\(scim\): preserve scim_active on PUT when client omits the field

A SCIM PUT may legally omit `active` \(full-replace with the field
absent\). Pydantic fills the SCIMUser.active default of True, so the PUT
handler was overwriting metadata.scim_active with True even when the
client never sent it — silently reactivating a previously SCIM-blocked
user and unblocking their keys.

Use model_fields_set to detect whether the client actually sent
`active`. If omitted, preserve the prior scim_active value and skip
the cascade to virtual keys.

Also drop comments added in this PR that just narrate what the code
does; keep only the docstrings and the SQL-NULL pitfall note that
explain non-obvious behaviour.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(proxy\): use set lookup for permitted agent filters

* fix\(mcp\): redact command fields for non-admin server views

* fix\(proxy\): forward decoded container ids after ownership checks

* fix\(caching\): handle stale isolated Redis semantic index

* fix\(cloudflare\): support response_text in streaming chunk parser

Newer Cloudflare Workers AI models \(e.g. Nemotron\) emit 'response_text'
instead of 'response' on streamed chunks. The non-streaming path was
already updated to fall back to 'response_text' \(#26385\), but the
streaming chunk parser still only read 'response', which caused
streaming requests against those models to silently produce empty
content.

Mirror the non-streaming fallback in CloudflareChatResponseIterator.chunk_parser
and add a streaming test for the response_text shape.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* Fix code qa

* Address bugbot: drop dead encode/decode helpers; preserve empty custom_id

- Remove unused _encode_gcp_label_value / _decode_gcp_label_value singular
  helpers; only the _chunks variants are actually called.
- Use 'is not None' check for custom_id so empty-string custom_ids are
  still labeled and round-trip through batch outputs.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* Forward Vertex file content logging context

* test vertex file content logging forwarding

Co-authored-by: Sameer Kankute <Sameerlite@users.noreply.github.com>

* Fix Vertex batch output logging mutation

* fix: don't mutate caller's logging_obj in _try_transform_vertex_batch_output_to_openai

The method was overwriting logging_obj.optional_params, logging_obj.model,
and logging_obj.start_time on the caller's Logging instance. When invoked
from llm_http_handler.py's generic framework path, the framework's own
logging_obj \(which already went through pre_call\) had its properties
clobbered, causing model and start_time to reflect the last batch line's
values rather than the original call context.

Fix: create a fresh local Logging instance for the per-line transformation
instead of mutating the incoming logging_obj. The caller's object is now
left entirely untouched regardless of whether a logging_obj was passed in
or not.

Regression tests added to verify model, start_time, and optional_params
are not mutated on the caller's logging_obj.

Co-authored-by: Sameer Kankute <Sameerlite@users.noreply.github.com>

* feat: add opt-out flag for Vertex batch output transformation

Adds litellm.disable_vertex_batch_output_transformation \(default False\).
When True, afile_content returns raw Vertex predictions.jsonl untouched
so users that parse candidates/modelVersion directly are not broken.

* fix\(anthropic,bedrock\): omit thinking/output_config when reasoning_effort="none"

Setting reasoning_effort="none" on Anthropic chat models \(direct, Bedrock
Invoke, Bedrock Converse, Vertex AI Anthropic, Azure AI Anthropic\) crashed
LiteLLM with:

  litellm.APIConnectionError: 'NoneType' object has no attribute 'get'

Both the Anthropic chat transformation and Bedrock Converse called
``AnthropicConfig._map_reasoning_effort`` and assigned the ``None`` it returns
for ``"none"`` directly to ``optional_params\["thinking"\]``. Downstream
``is_thinking_enabled`` then did ``optional_params\["thinking"\].get\("type"\)``
and crashed.

Pop ``thinking`` \(and on Claude 4.6/4.7, ``output_config``\) instead of
assigning ``None``, restoring the documented contract that
``reasoning_effort="none"`` means "do not enable thinking". This also
prevents downstream Anthropic 400s \("thinking: Input should be an object",
"output_config.effort: Input should be ..."\) if the bug were ever masked.

Verified end-to-end against the live Anthropic API and Bedrock Converse
on claude-opus-4-{5,6,7} and claude-sonnet-4-6, plus Bedrock Invoke for
Claude 4.5/4.6. Vertex AI Anthropic and Azure AI Anthropic inherit the
fixed ``map_openai_params`` from ``AnthropicConfig`` and need no further
changes.

* fix\(vertex-ai\): set response=null on batch error entries per OpenAI spec

The Vertex batch output transformer was emitting both a populated 'response' and 'error' for failed batch entries. The OpenAI Batch output spec defines them as mutually exclusive: on error 'response' MUST be null. This broke any consumer using 'result\["response"\] is None' to detect failures.

* test\(vertex-ai\): cover transformation_error path emits response=null

* fix\(security\): sandbox jinja2 in gitlab/arize/bitbucket prompt managers

DotpromptManager was hardened to render through
ImmutableSandboxedEnvironment. The three sibling managers \(gitlab,
arize, bitbucket\) were missed and still instantiate plain
jinja2.Environment\(\), leaving the same attribute-traversal SSTI
primitive open: a template fetched from a GitLab/BitBucket repo or
Arize Phoenix workspace can reach __class__.__init__.__globals__ and
execute arbitrary Python on the proxy host.

Match the dotprompt pattern by switching all three to
ImmutableSandboxedEnvironment. The sandbox blocks the dunder-traversal
chain while leaving normal {{ var }} substitution intact, so the
template surface is unchanged for legitimate use.

Adds tests/test_litellm/integrations/test_prompt_manager_ssti.py
\(18 cases\) verifying each manager's jinja_env is a sandbox, that
classic SSTI payloads raise SecurityError, and that ordinary variable
rendering still works.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* chore\(proxy\): drop client-supplied pricing fields from request bodies

The proxy currently forwards request-body pricing parameters \(the fields
on `CustomPricingLiteLLMParams`, plus `metadata.model_info`\) into the
core call path. Those fields belong to deployment configuration, not to
per-request input — sending them from a client mutates the request's
recorded cost and, via `litellm.completion` → `register_model`, the
process-wide `litellm.model_cost` map for every later caller in the
worker. Strip them at the boundary.

The strip set is built from `CustomPricingLiteLLMParams.model_fields` so
pricing fields added later are covered automatically. Operators who do
want clients to supply per-request pricing can opt back in per key or
team via `metadata.allow_client_pricing_override = true`, mirroring the
existing `allow_client_mock_response` and
`allow_client_message_redaction_opt_out` flags.

Tests cover the strip set's coverage, root and metadata strips, the
opt-in skip on both key and team metadata, and a regression check that
the global `litellm.model_cost` map is unmutated after a stripped
request.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* chore\(proxy\): log stripped pricing fields at debug for operator visibility

Operators upgrading would otherwise see client-supplied pricing overrides
silently stop applying with no diagnostic. Emit a debug-level line listing
the dropped fields and pointing at the opt-in flag when any are stripped;
stay silent on the no-op path so the log isn't filled with noise.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(proxy\): move pricing strip below the litellm_metadata JSON-string parse

The strip ran before the proxy parses ``litellm_metadata`` from a JSON
string into a dict \(a path used by multipart/form-data and ``extra_body``
callers\), so ``isinstance\(metadata, dict\)`` was False and ``model_info``
survived the strip. Move the call to the same post-parse position the
``user_api_key_*`` strip already uses for the same reason. Adds a
regression test exercising the JSON-string ``litellm_metadata`` path.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* test\(responses\): replace legacy claude-4-sonnet alias in multiturn tool-call test

Anthropic's main API no longer resolves the non-canonical 'claude-4-sonnet-20250514'
alias for freshly issued keys, returning 404 not_found_error. PR #27031 already
swept three other live tests pinned to this alias to claude-haiku-4-5-20251001
but missed test_multiturn_tool_calls in the responses API suite, which is now
failing reliably on PR CI runs \(e.g. PR #27074, job 1603363\).

Bump the two model references in test_multiturn_tool_calls to the same
claude-haiku-4-5-20251001 snapshot used by PR #27031 -- it covers everything
this test exercises \(tool calling, multi-turn\) and isn't on a deprecation
schedule.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* chore\(proxy\): close callback-config and observability-credential side channels

Two related gaps in the proxy's request bouncer:

1. ``is_request_body_safe`` \(auth_utils.py\) walked the request-body root
   and the ``litellm_embedding_config`` nested dict, but not ``metadata``
   or ``litellm_metadata``. The same fields it bans at root — Langfuse /
   Langsmith / Arize / PostHog / Braintrust / Phoenix / W&B Weave / GCS /
   Humanloop / Lunary credentials and routing — were silently accepted
   when the caller put them inside metadata, retargeting observability
   callbacks to a caller-controlled host with caller-supplied creds.
   Walk both metadata containers \(and parse the JSON-string form sent via
   multipart / ``extra_body``\) through the same banned-params helper, so
   the existing ``allow_client_side_credentials`` opt-in covers both
   paths consistently.

2. The banned-params list was hand-maintained and lagged the canonical
   ``_supported_callback_params`` allow-list in
   ``initialize_dynamic_callback_params``. Derive the observability bans
   from that allow-list \(minus a small ``_SAFE_CLIENT_CALLBACK_PARAMS``
   set for informational fields like ``langfuse_prompt_version`` and
   ``langsmith_sampling_rate``\) so future integrations are covered
   automatically; ``_EXTRA_BANNED_OBSERVABILITY_PARAMS`` carries the
   handful of fields integrations read but the allow-list hasn't caught
   up to. A guard test fails CI if a new entry is added to
   ``_supported_callback_params`` without an explicit safe-list decision.

Separately in ``litellm_pre_call_utils.py``: add ``callbacks``,
``service_callback``, ``logger_fn``, and ``litellm_disabled_callbacks``
to ``_UNTRUSTED_ROOT_CONTROL_FIELDS``. The first three are appended to
worker-wide ``litellm.{input,success,failure,_async_*,service}_callback``
lists / ``litellm.user_logger_fn`` from inside ``function_setup`` — one
request poisons every subsequent caller in that worker. The last is the
inverse primitive: the legitimate path reads it from key/team metadata,
the request-body version silently disables admin-configured audit /
observability for the call.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(auth\): per-param allow must continue, not return early

A pre-existing logic bug in ``_check_banned_params``: when the
deployment-level ``configurable_clientside_auth_params`` permitted one
banned field, the loop ``return``-ed on the first match instead of
``continue``-ing, so any other banned param later in the same body or
metadata dict was never checked. This PR's metadata walk multiplies the
surface where that bypass matters — a body pairing an allowed
``api_base`` with an observability credential like ``langfuse_host``
would silently pass.

Proxy-wide ``allow_client_side_credentials`` keeps ``return`` \(it's a
global opt-in for every banned param\). The per-param branch becomes
``continue`` so only the one explicitly-permitted field is skipped.

Adds a regression test that exercises the api_base + langfuse_host pair.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(vector_store\): resolve embedding config at request time, never persist creds

The vector store create/update path previously called
``_resolve_embedding_config`` against the admin-configured router/DB
model and persisted the resolved ``litellm_embedding_config`` dict
\(``api_key`` / ``api_base`` / ``api_version``\) into the
``litellm_managedvectorstorestable.litellm_params`` column. Because the
resolver expanded ``os.environ/...`` references via ``get_secret``, the
DB row carried cleartext provider credentials, and the
``/vector_store/{new,info,update,list}`` responses returned them to any
authenticated caller who could supply a known admin model name.

Move the auto-resolve out of ``create_vector_store_in_db`` and out of
the update path. Persist only the user-supplied ``litellm_embedding_model``
reference. Resolve at request-handling time inside
``_update_request_data_with_litellm_managed_vector_store_registry`` so
the resolved config lives in the per-request ``data`` dict and is
garbage-collected after the response. Legacy rows that were created by
an earlier proxy version and already carry a resolved
``litellm_embedding_config`` skip the re-resolution and pass through
unchanged so embedding calls keep working.

The ``new_vector_store`` response now also runs the existing
``_redact_sensitive_litellm_params`` masker \(already used by ``info``,
``update``, and ``list``\), defending against caller-supplied cleartext
on the create path and against legacy rows whose persisted credentials
are still in the database.

Existing tests that asserted the old write-time-resolve behaviour are
updated to assert the new persistence shape \(no embedding config
stored, just the model reference\). Two new tests cover the use-time
path: one asserting fresh resolution happens when a row carries only
the model reference, the other asserting legacy rows with persisted
config skip re-resolution and continue to work.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(vector_store\): tighten registry-mutation comment and dedupe test helpers

* fix\(vector_store\): cache use-time embedding-config resolution

Hold the resolved config in a process-memory TTL cache so the
request-handling path doesn't run litellm_proxymodeltable.find_first
on every vector-store call.

* fix\(anthropic,bedrock,vertex\): forward output_config.effort + 400 on garbage reasoning_effort

Follow-up bugs surfaced by the QA sweep on PR #27039
\(https://github.com/BerriAI/litellm/pull/27039#issuecomment-4363363610\).

1. Stop stripping output_config.effort on Bedrock + Vertex adaptive routes.
   - Vertex AI Claude 4.6/4.7 accepts output_config.effort on rawPredict
     \(verified end-to-end against us-east5 / global\). The strip helper now
     no-ops for effort.
   - Bedrock Converse routes output_config into additionalModelRequestFields
     for anthropic base models so the requested adaptive tier \(low/medium/
     high/xhigh/max\) actually reaches the wire instead of all collapsing to
     identical thinking.
   - Bedrock Invoke chat transformation \(AmazonAnthropicClaudeConfig\) stops
     popping output_config from the post-AnthropicConfig request body.
   - Bedrock Invoke /v1/messages allowlist \(BedrockInvokeAnthropicMessagesRequest\)
     now lists output_config so the runtime allowlist filter forwards it.

2. Validate effort across Bedrock Converse so 'disabled' / 'invalid' / '' /
   unsupported tiers \(xhigh/max on Sonnet 4.6 or budget-mode 4.5 models\)
   surface as a clean 400 BadRequestError instead of 500.

3. ValueError -> BadRequestError throughout \(AnthropicConfig.map_openai_params,
   _apply_output_config, AmazonConverseConfig._handle_reasoning_effort_parameter\).
   Empty-string effort is now rejected \(was silently passing the
   'if effort and ...' short-circuit\).

4. Floor reasoning_effort='minimal' at the Anthropic provider minimum
   \(1024 budget_tokens\) via new ANTHROPIC_MIN_THINKING_BUDGET_TOKENS so it's
   a usable tier on direct Anthropic / Azure AI Anthropic / Vertex AI Anthropic /
   Bedrock Invoke \(all of which 400 below 1024\).

5. model_prices: dedupe duplicate supports_max_reasoning_effort key on
   claude-opus-4-7 / claude-opus-4-7-20260416.

Adds regression tests across all five affected paths; existing tests asserting
the silent-strip behavior were updated to reflect the new pass-through and
clean 400 surfaces.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(constants\): make ANTHROPIC_MIN_THINKING_BUDGET_TOKENS a plain constant

The documentation CI test \(tests/documentation_tests/test_env_keys.py\)
asserts every os.getenv\(\) key in the source has a matching entry in the
litellm-docs config_settings.md table. ANTHROPIC_MIN_THINKING_BUDGET_TOKENS
tracks Anthropic's published wire-protocol minimum \(1024\) — it's not a
user-tunable, so making it env-overridable was wrong anyway. Drop the
os.getenv\(\) wrapper; the value is now a plain literal.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(anthropic,bedrock\): correct effort error message and dedupe effort_map

- Remove 'none' from the Bedrock _validate_anthropic_adaptive_effort error
  message; it was listed as a valid value but rejected by the membership
  check, leaving users in a feedback loop if they tried 'none'.
- Hoist the duplicated reasoning_effort -> output_config.effort mapping
  out of AnthropicConfig.map_openai_params and
  AmazonConverseConfig._handle_reasoning_effort_parameter into a single
  AnthropicConfig.REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT class constant
  so the two routes cannot drift.

* fix\(anthropic\): translate reasoning_effort on /v1/messages route

Closes the remaining QA-sweep gap on PR #27074: Bedrock Invoke
/v1/messages was silently ignoring ``reasoning_effort`` because the
shared param filter only kept native Anthropic keys, so every effort
tier collapsed to the same behavior on the wire \(27/231 cells failing
across opus-4-5 / opus-4-6 / sonnet-4-6\).

Map ``reasoning_effort`` to native Anthropic ``thinking`` /
``output_config.effort`` at the ``AnthropicMessagesConfig`` layer so
all four /v1/messages routes \(direct Anthropic, Azure AI, Vertex AI,
Bedrock Invoke\) inherit the same translation:

- Add ``reasoning_effort`` to ``AnthropicMessagesRequestOptionalParams``
  so the param filter in
  ``AnthropicMessagesRequestUtils.get_requested_anthropic_messages_optional_param``
  no longer drops it before the transformation runs.

- Add ``_translate_reasoning_effort_to_anthropic`` and call it from
  ``transform_anthropic_messages_request``. Mirrors
  ``AnthropicConfig.map_openai_params`` on the chat completion path
  \(re-uses ``_map_reasoning_effort`` and
  ``REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT``\) so the two routes
  cannot drift. Pops ``reasoning_effort`` so it never reaches the wire.

- Caller-supplied native ``thinking`` / ``output_config.effort`` always
  win — same precedence as
  ``_translate_legacy_thinking_for_adaptive_model``.

- Garbage values \(``""``, ``"disabled"``, ``"invalid"``\) raise
  ``AnthropicError\(status_code=400\)`` instead of falling through and
  surfacing as 500s from the provider.

- ``"none"`` clears thinking + output_config so callers can opt out
  per request.

Also restores the non-adaptive-model test coverage on Bedrock Invoke
/v1/messages that the previous commit lost when
``test_bedrock_messages_strips_output_config`` was renamed to the
``forwards`` variant on Opus 4.7.

Adds a new test file
``test_reasoning_effort_translation.py`` covering the translation at
the shared config level \(adaptive + non-adaptive models, none, garbage,
caller precedence\) so all four /v1/messages routes are exercised by a
single suite.

Adds parametrized + behavioral tests on the Bedrock Invoke /v1/messages
suite covering: minimal/low/medium/high/xhigh/max mapping for adaptive
models, thinking-budget mapping for non-adaptive Opus 4.5, ``none``
clears both, garbage raises 400, explicit ``output_config`` wins.

Refs: https://github.com/BerriAI/litellm/pull/27074

* fix\(anthropic,bedrock\): reject unmapped reasoning_effort at mapping site

Both the chat completion path \(AnthropicConfig.map_openai_params\) and the
Bedrock Converse path \(_handle_reasoning_effort_parameter\) used
REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT.get\(value, value\) which falls
back to the raw input on unmapped keys. Combined with _map_reasoning_effort
returning type='adaptive' for any string on Claude 4.6/4.7, garbage values
\(e.g. 'disabled'\) could leak into optional_params\['output_config'\]\['effort'\]
unvalidated if map_openai_params ran without the downstream transform_request
or _validate_anthropic_adaptive_effort check.

Mirror the /v1/messages pattern: use .get\(value\) \(no fallback\) and raise
BadRequestError immediately when the value is unmapped, co-locating
validation with the mapping for defense in depth.

* style: black formatting

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(anthropic\): stop class-attr leak; gate xhigh/max on every route

The reasoning-effort mapping dict was a public class attribute on
AnthropicConfig, so BaseConfig.get_config returned it as a request
parameter and every Anthropic-backed call \(Anthropic / Azure / Vertex /
Bedrock Invoke\) hit a 400 'REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT:
Extra inputs are not permitted' from the provider. Move the mapping
to a module-level constant.

_supports_effort_level only looked the model up under
custom_llm_provider='anthropic', so bedrock-prefixed model ids
\(e.g. bedrock/invoke/us.anthropic.claude-opus-4-7\) returned False
for both 'max' and 'xhigh' even when the underlying model entry has
the flag set. Strip known provider prefixes and retry the lookup
against litellm.model_cost directly so per-model gating works on
every route.

Mirror the per-model xhigh/max gate from
AnthropicConfig._apply_output_config in
AnthropicMessagesConfig._translate_reasoning_effort_to_anthropic so
the /v1/messages route also raises a clean 400 instead of forwarding
the unsupported tier.

* feat\(anthropic,bedrock\): strip output_config under drop_params for non-effort models

When a proxy fronts Claude Code \(which always sends `output_config.effort`\)
at a pre-4.5 Anthropic model — haiku-3, sonnet-3.5, opus-3, sonnet-4 — the
forwarded knob causes a forced 400 the client can't fix. Gating a strip
behind the existing `drop_params` flag lets operators opt into silent
fixup once and stop worrying about per-model param hygiene.

Default \(`drop_params=False`\) still forwards and surfaces the provider's
error, preserving the strict, debuggable contract from #27074.

Per https://platform.claude.com/docs/en/build-with-claude/effort the
supporting set is Opus 4.5+, Sonnet 4.6+, and Mythos Preview; everything
else is dropped \(with a verbose_logger warning so the strip is visible\).
Recognition uses model-name patterns plus a fallback to any
`supports_*_reasoning_effort` flag in the model map for forward
compatibility with new entries.

https://claude.ai/code/session_01WjHq31rvXT6xYNdVmSJvRp

\(cherry picked from commit 1233943e7861ba8a9062f792310ebd401cb03db8\)

* fix\(base_llm\): filter all _-prefixed class attrs from get_config

The drop_params strip work added `AnthropicConfig._EFFORT_SUPPORTING_MODEL_PATTERNS`
as a private class-level lookup tuple. `BaseConfig.get_config\(\)` only
filtered the `__`-prefixed names plus `_abc` / `_is_base_class`, so
`_EFFORT_SUPPORTING_MODEL_PATTERNS` would have leaked into the request
body the same way `REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT` did before
the previous commit.

Generalize the existing `_abc` / `_is_base_class` carve-outs to skip
every `_`-prefixed name. `AmazonConverseConfig.get_config\(\)` overrides
the base method, so apply the same change there.

Also unblocks future internal helpers from accidentally serialising into
the wire body.

* fix\(anthropic\): drive output_config.effort support from model map flags

Replace hardcoded _EFFORT_SUPPORTING_MODEL_PATTERNS with a JSON-backed
check that uses supports_*_reasoning_effort flags from the model map.
Add supports_minimal_reasoning_effort: true to opus-4-5 and mythos-preview
entries \(which previously only carried supports_reasoning\) so the JSON
remains the single source of truth for effort capability.

* fix\(anthropic,bedrock,databricks\): four reasoning_effort follow-ups

- claude-sonnet-4-6 + reasoning_effort=max no longer 400s. Renamed
  _is_opus_4_6_model to _is_claude_4_6_model at three sites and added
  supports_max_reasoning_effort: true to 12 model entries in the JSON
  cost map \(10 sonnet 4.6 ids + OpenRouter opus 4.6/4.7\).
- _map_reasoning_effort now raises BadRequestError\(400\) directly with
  llm_provider, instead of letting Databricks \(and similar callers\)
  surface its raw ValueError as a 500.
- output_config.effort on Opus 4.5 over Bedrock no longer 400s for
  missing effort-2025-11-24 beta. Flipped JSON to "effort-2025-11-24"
  for bedrock + bedrock_converse and added an auto-attach branch in
  _process_tools_and_beta for non-adaptive Anthropic + output_config
  on Converse.
- reasoning_effort=xhigh / =max on legacy budget-mode models
  \(Haiku 4.5, Sonnet 4.5, Opus 4.5\) now map to thinking.budget_tokens
  8192 / 16384 instead of returning 400. Added two constants in
  litellm/constants.py.

Tests updated for all four flips. Validated end-to-end via 306-cell
live proxy matrix \(6 model families x 3 routes x 17 effort cases\),
all pass.

* fix\(databricks\): validate reasoning_effort and set output_config on adaptive Claude

The Databricks path called `AnthropicConfig._map_reasoning_effort` for
Claude models but never validated the effort string nor set
`output_config.effort` for adaptive models \(Claude 4.6/4.7\). Since
`_map_reasoning_effort` returns `type=adaptive` for ANY non-None /
non-"none" string on adaptive models \(including "disabled",
"invalid", ""\), Databricks silently accepted garbage and emitted a
request without an `output_config.effort`, collapsing every adaptive
tier to identical behavior.

Match the Anthropic native, Bedrock Converse, Bedrock Invoke, and
/v1/messages paths: when the resolved `thinking` is non-None on a
4.6/4.7 model, look up the value in
`REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT` and either raise a clean
`BadRequestError` or set `optional_params\["output_config"\]`.

* fix\(azure\): omit model from image generation and image edit deployment requests

Azure OpenAI routes image gen/edit by deployment in the URL; sending the
deployment id in model breaks gpt-image-2 \(invalid_value\). Strip model from
JSON for deployments/.../images/generations and from multipart data for
.../images/edits. Non-deployment URLs \(e.g. Azure AI FLUX\) unchanged.

Fixes #26316.

Co-authored-by: Cursor <cursoragent@cursor.com>

* test\(azure\): exercise image gen JSON filter via HTTP client; dedupe image edit URL

- Image generation tests patch HTTPHandler.post / get_async_httpx_client so
  make_*_azure_httpx_request runs and wire json is asserted on call kwargs.
- Azure image edit: strip model in finalize_image_edit_multipart_data using the
  same URL string the handler passes to POST \(no second get_complete_url in
  transform\). BaseImageEditConfig default finalize is a no-op.

Co-authored-by: Cursor <cursoragent@cursor.com>

* fix\(azure_ai/anthropic\): promote output_config out of extra_body so validation runs

`azure_ai` is registered in `litellm.openai_compatible_providers`, so
`add_provider_specific_params_to_optional_params` \(litellm/utils.py\)
auto-stuffs any non-OpenAI kwarg \(e.g. `output_config={"effort": "..."}`\)
into `optional_params\["extra_body"\]`. `AzureAnthropicConfig.transform_request`
then strips `extra_body` entirely on the way out, silently dropping the
param — and `AnthropicConfig._apply_output_config` never sees it, so
`effort="invalid"` / `effort="xhigh"` on a non-supporting model
quietly reaches the model with default behavior instead of returning a
clean 400 \(as the native `anthropic` provider does\).

Promote the keys back to top-level `optional_params` \(using `setdefault`
so explicit top-level values win\) before delegating to the parent
`AnthropicConfig`. Apply in both `validate_environment` and
`transform_request` so flag detection \(`is_mcp_server_used`, etc.\) and
output-config validation both run.

Surfaced by the QA matrix expansion on PR #27074: 20 cells where Azure
returned 200 while `anthropic` returned 400 — all `output_config` mode
across haiku_4_5, sonnet_4_5, opus_4_5, sonnet_4_6, opus_4_6, opus_4_7
families with `effort` in {invalid, xhigh, max, low, medium, high}.

Tests:
* `test_output_config_promoted_from_extra_body`: valid effort reaches data
* `test_invalid_output_config_effort_raises_via_extra_body`: 400 on bad effort
* `test_unsupported_effort_xhigh_raises_via_extra_body`: 400 on xhigh-on-Sonnet-4.6
* `test_extra_body_promotion_does_not_clobber_top_level`: setdefault semantics

* test\(image_gen\): expect no model in Azure image edit multipart \(#26316\)

Align test_azure_image_edit_litellm_sdk with deployment-scoped Azure edits.

Co-authored-by: Cursor <cursoragent@cursor.com>

* refactor\(anthropic\): extract _validate_effort_for_model to prevent drift

The chat completion path \(`_apply_output_config`\) and the /v1/messages
pass-through \(`AnthropicMessagesConfig._translate_reasoning_effort_to_anthropic`\)
both gate `max` / `xhigh` per model. The two sites had diverged from
near-identical copies into separately maintained blocks, creating a real
drift risk when a new model tier \(e.g. Claude 4.8\) lands -- a contributor
could update one site and miss the other.

Centralise the gating in `AnthropicConfig._validate_effort_for_model`,
which returns an error message string or `None`. Each call site keeps
its own provider-appropriate exception type \(`BadRequestError` for the
chat path, `AnthropicError` for the /v1/messages pass-through\) but the
gating decision now comes from one place. Net -11 LOC.

Adds a parametrised unit test exercising the helper directly across
4.5 / 4.6 / 4.7 model families and `max` / `xhigh` / lower-effort
inputs. Existing tests at both call sites continue to pass unchanged.

Addresses Greptile finding on PR #27074.

* fix\(databricks\): narrow reasoning_effort_value to str for mypy

`non_default_params.get\("reasoning_effort"\)` returns `Any | None`,
but `REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT.get\(\)` expects `str`.
Mypy flagged this on the strict pass. Narrow with `isinstance` before
the lookup; non-strings fall through to the existing `BadRequestError`
below with a clean validation message, so behavior is unchanged.

Fixes a regression introduced by 1a10746e95 in this PR.

* feat\(proxy\): add health_check_reasoning_effort for model health checks

Co-authored-by: Cursor <cursoragent@cursor.com>

* test\(image_gen\): align Azure image gen fixture with body omitting model

Expected JSON matches deployment-scoped Azure POST \(#26316\).

Co-authored-by: Cursor <cursoragent@cursor.com>

* test\(anthropic/chat\): force PR-local model_cost map via autouse fixture

CI runs without LITELLM_LOCAL_MODEL_COST_MAP=True, so litellm.model_cost
is loaded from main-branch JSON \(default model_cost_map_url\) instead of
the PR's checked-out model_prices_and_context_window.json. Tests that
assert per-model flags added in this PR \(supports_max_reasoning_effort,
supports_xhigh_reasoning_effort\) therefore pass locally but fail in CI
with 'AssertionError: assert False is True' on 5 cases:

  - test_anthropic_model_supports_effort_param_recognizes_supporting_models
    \[anthropic.claude-mythos-preview, bedrock/.../mythos-preview,
     claude-opus-4-5-20251101\]
  - test_supports_effort_level_handles_provider_prefixes
    \[bedrock/invoke/us.anthropic.claude-sonnet-4-6-max-True,
     claude-sonnet-4-6-max-True\]

Add an autouse fixture at tests/test_litellm/llms/anthropic/chat/conftest.py
that monkey-patches litellm.model_cost to the PR-local JSON for every test
in this directory. The parent conftest already snapshots+restores
litellm.model_cost per-function, so the mutation is contained.

This is a scoped workaround. The proper fix is to set the env var
globally in the test workflow once the ~10 inline self-set test files
are audited; tracking that as a follow-up issue.

* \[Fix\] Docker: Pin Wolfi And Uv To Multi-Arch Index Digests

The previous pins resolved to single-platform amd64 manifests, so buildx
pulled the same amd64 base for both linux/amd64 and linux/arm64 targets.
The published OCI index then advertised an arm64 entry whose layers are
byte-identical to amd64 -- arm64 users got an amd64 binary.

Switch all three Dockerfiles to the multi-arch image-index digests:
  - cgr.dev/chainguard/wolfi-base   \(index has linux/amd64 + linux/arm64\)
  - ghcr.io/astral-sh/uv:0.11.7     \(index has linux/amd64 + linux/arm64\)

Resolved with `docker buildx imagetools inspect <ref>` -- that returns
the index digest. `docker pull` + `docker inspect` returns the per-host
platform digest, which is what slipped in last time.

* \[Fix\] Docker: Pin Uv To Multi-Arch Index Digest In Remaining Dockerfiles

Apply the same fix to the three Dockerfiles not in the release pipeline
today \(alpine, dev, health_check\) so they stay correct if/when they're
built for arm64 in the future.

Wolfi pins are not present in these files; the python:3.11-alpine and
python:3.13-slim digests they already use are multi-arch indexes that
include arm64/v8, so only the uv pin needed swapping.

* fix\(xai\): fold reasoning_tokens into completion_tokens to satisfy OpenAI invariant

xAI's chat completions API accounts reasoning_tokens separately from
completion_tokens, but rolls them into total_tokens. This breaks the
OpenAI invariant total_tokens == prompt_tokens + completion_tokens
that downstream consumers \(including litellm's own _usage_format_tests
in tests/llm_translation/base_llm_unit_tests.py:58\) rely on.

Live capture \(grok-3-mini-beta, 2026-05-04\):
    prompt=14, completion=10, total=336, reasoning=312
    14 + 10 = 24, NOT 336.

OpenAI's o1/o3 reasoning models include reasoning_tokens in
completion_tokens, leaving the prompt+completion=total invariant
intact. xAI deviates. This patch aligns xAI to OpenAI semantics by
folding reasoning_tokens into completion_tokens after the parent
OpenAI parser runs.

The fold is idempotent and defensive:
- Only fires when total_tokens == prompt_tokens + completion_tokens
  + reasoning_tokens \(the documented xAI shape\). Refuses to fold if
  the gap doesn't match, guarding against silent corruption when xAI
  changes accounting.
- Skips if completion_tokens already covers the gap \(already
  normalised — e.g. cost calc replays a previously-folded Usage\).

xai.cost_calculator.cost_per_token already added reasoning_tokens to
the visible completion count for billing. Post-fold the Usage block
now satisfies that invariant directly, so the cost calc would
double-bill. Updated cost_per_token to detect the OpenAI-normalised
shape \(total == prompt + completion\) and skip the reasoning add-on
in that case, falling through to the legacy raw-shape behaviour for
callers that bypass the transformation \(e.g. proxy log replay\).

Tests:
- Adds TestXAIReasoningTokenFolding covering: gap-explained-fold,
  idempotent-no-double-fold, no-reasoning-skip, gap-mismatch-skip.
- Adds test_already_normalised_usage_does_not_double_count_reasoning
  to lock the cost-calc idempotency.
- Updates 7 pre-existing cost-calc tests whose total_tokens was
  internally inconsistent \(used the OpenAI-normalised total but kept
  reasoning_tokens external\) to use the documented xAI raw shape
  total = prompt + visible completion + reasoning. Pre-existing
  values masked the missing-fold by accident.

Verified end-to-end against the live xAI API:
    LITELLM_LOCAL_MODEL_COST_MAP=False \(CI default\) +
    XAI_API_KEY set +
    pytest tests/llm_translation/test_xai.py::TestXAIChat::test_prompt_caching
        -> PASSED in 18.81s \(was: AssertionError on
        usage.total_tokens == usage.prompt_tokens + usage.completion_tokens\)

20/20 tests in tests/test_litellm/llms/xai/test_xai_cost_calculator.py
and 8/8 in tests/test_litellm/llms/xai/test_xai_chat_transformation.py
pass.

* refactor\(bedrock/converse\): delegate effort gating to AnthropicConfig._validate_effort_for_model

Removes the duplicated max/xhigh gating logic in
_validate_anthropic_adaptive_effort and the now-unused
_supports_effort_level_on_bedrock helper. Per-model gating now flows
through the centralized AnthropicConfig._validate_effort_for_model
\(whose _supports_effort_level already strips Bedrock prefixes\), so the
chat completion, /v1/messages, and Bedrock Converse paths can't drift
when a new gated effort tier is added.

* Implement normalize_nonempty_secret_str function to trim whitespace from secrets and treat empty values as unset. Update proxy_server to use this function for Grafana credentials. Enhance tests to validate the new normalization behavior.

* Fix qdrant semantic cache miss metadata

* chore\(deps\): refresh dependency locks

* chore\(deps\): authorize pytest license

* fix: preserve tokenizer decode round trips

* refactor\(anthropic\): drive adaptive-thinking gate via supports_adaptive_thinking flag

Three of greptile's open comments on #27074 \(P2 converse:512, P1
databricks:361, and the underlying capability-flag policy rule\) flagged
the same pattern: _is_claude_4_6_model\(...\) or _is_claude_4_7_model\(...\)
used inline as a runtime 'is this an adaptive-thinking model?' check.
That requires a code release each time a new adaptive Claude lands.

Consolidate the inline gating to AnthropicModelInfo._is_adaptive_thinking_model,
and switch the helper itself to read a new supports_adaptive_thinking
flag from `model_prices_and_context_window.json` via `_supports_factory`,
falling back to the family pattern only when the model-map entry doesn't
carry the flag \(preserves OpenRouter / Vercel / Bedrock-prefixed variants
that route through the same code path with non-canonical ids\).

Adds `supports_adaptive_thinking: true` to the four 4.6/4.7 anthropic
entries \(opus-4-6 + dated, opus-4-7 + dated, sonnet-4-6\). Bedrock-prefixed
and Vertex-prefixed entries don't need the flag because both fall back
through the family pattern \(the helper short-circuits early on True from
either path\) and the bedrock/vertex Claude IDs all match the existing
opus-4-{6,7} / sonnet-4-{6,7} pattern.

Affected call sites:

- `bedrock/chat/converse_transformation.py:_handle_reasoning_effort_parameter`
- `anthropic/chat/transformation.py:_map_reasoning_effort`
- `anthropic/chat/transformation.py:map_openai_params` \(output_config branch\)
- `databricks/chat/transformation.py:map_openai_params` \(output_config branch\)

The remaining `_is_claude_4_6_model` / `_is_claude_4_7_model` references
in `AnthropicConfig._validate_effort_for_model` and
`AnthropicConfig.get_supported_openai_params` are intentionally retained:
they're per-model gating fallbacks for variants whose model-map entries
don't yet carry the `supports_max_reasoning_effort` /
`supports_reasoning` flag. Those are documented in-place.

Tests: 537 anthropic/bedrock/databricks/vertex/messages tests pass.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* chore\(deps\): address dependency review notes

* test\(model_prices\): add supports_adaptive_thinking to schema

`test_aaamodel_prices_and_context_window_json_is_valid` validates the
model-map JSON against an explicit schema with `additionalProperties`,
so the new `supports_adaptive_thinking` flag added in
98ced0ae43 needs a matching schema entry.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* refactor: remove unnecessary comments from #27074

Strip out the explanatory and historical comments that don't carry
business-logic justification. Comments that simply narrate what code
does — or that explain prior behavior, what was changed, or which PR
introduced a fix — are removed. Docstrings are reduced to a one-line
summary where the long form repeated information already evident from
the code or test data.

No code-behavior changes. All 643 affected unit tests still pass.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* test: keep decode token test local

* chore\(deps\): align dashboard node engine

* feat: selectively apply routing strategy according to model name

* style: make _model_supports_effort_param more concise

* refactor\(anthropic,bedrock\): hoist drop_params output_config warning to module constant

Three call sites \(anthropic chat, bedrock converse, bedrock invoke messages\)
emitted the same '...Effort is only supported on Opus 4.5+, Sonnet 4.6+, and
Mythos Preview' warning verbatim. Extract DROP_UNSUPPORTED_OUTPUT_CONFIG_WARNING
in litellm/llms/anthropic/chat/transformation.py and import it from the bedrock
sites so future copy edits live in one place.

Addresses Michael's review on PR #27074.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* refactor\(anthropic,bedrock,databricks\): factor BadRequestError for unknown reasoning_effort

Three call sites raised the same BadRequestError\("Invalid reasoning_effort:
... Must be one of 'minimal', 'low', ..."\) block when REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT
returned None: anthropic chat map_openai_params, bedrock converse
_handle_reasoning_effort_parameter, and databricks chat reasoning_effort path.

Extract AnthropicConfig._raise_invalid_reasoning_effort\(model, value, llm_provider\)
so future copy edits / valid-set changes happen in one place. Typed as NoReturn
so type-checkers correctly narrow control flow at call sites.

Addresses Michael's review on PR #27074.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* Clean up Redis semantic cache isolation fallback

* fix\(guardrails\): align banned_keywords + azure_content_safety call_type gates with runtime route_type

The hooks gated on ``call_type == "completion"`` but the proxy ingress
passes ``route_type`` straight through as ``call_type`` —
``"acompletion"`` for /v1/chat/completions and ``"aresponses"`` for
/v1/responses. Tests passed because they used the literal sync
``"completion"`` value, masking the gap.

Switch both hooks to ``is_text_content_call_type`` \(matches the
canonical runtime values: completion / acompletion / aresponses\) and
update existing tests to assert against runtime values, plus parametrize
a regression test that pins the gate.

* fix: remove unused import

* Add semantic cache legacy migration flag

* Treat 0 team_member_budget as no cap

* chore\(caching\): annotate qdrant quantization_params dict type

Mypy infers the dict's value type from the first branch
\(Dict\[str, bool\]\) which clashes with the scalar branch's mixed-type
inner dict. Explicit Dict\[str, Any\] annotation lifts the inference.

* chore\(caching\): remove allow_legacy_unscoped_cache_hits opt-in

The flag was an opt-in escape hatch for the cross-tenant leak the rest
of the patch closes — flipping it on \(env var or constructor param\)
re-enables exactly the VERIA-54 primitive on either backend. There is
no operational need that the secure path doesn't already meet:

- Qdrant: legacy points without ``litellm_cache_key`` payload are
  excluded by the must-clause filter and treated as misses; new sets
  populate the cache key, so cold-start lasts only as long as the
  natural cache rebuild.
- Redis: existing unscoped index can't carry the new schema; the init
  path falls back to ``{name}_isolated`` \(and recreates it on stale
  schema\), leaving the legacy index untouched.

Drop the construc…")[#27245](https://github.com/BerriAI/litellm/pull/27245)[)](https://github.com/BerriAI/litellm/commit/6ff668c7aa01a73738ed39aa64913a089a183565 "\[Infra\] Promote internal staging to main \(#27245\)

* default requested_model to empty string on litellm-side rejects

* Update litellm/router.py

Co-authored-by: greptile-apps\[bot\] <165735046+greptile-apps\[bot\]@users.noreply.github.com>

* fix: scope key access_group_ids override by team's assigned groups

A team member could set any access_group_ids on their key \(e.g. a group
assigned only to a different team\) and override the team's model
restriction. Intersect the key's access_group_ids with team_object.access_group_ids
in _key_access_group_grants_model so foreign groups are dropped before
model expansion. Adds a regression test that asserts expansion is never
called for foreign groups.

* \[Fix\] Proxy: Skip Personal Budget Hook When Reservation Covers Counter

The reservation path \(PR #26845\) atomically pre-fills `spend:user:{user_id}`
and admits at the strict-`<` boundary. The legacy `_PROXY_MaxBudgetLimiter`
pre-call hook re-reads the same counter with `>=`, so a reservation that
fills the counter to exactly `max_budget` \(e.g. a request without a
`max_tokens` cap that falls back to reserving the smallest remaining
headroom\) is rejected by the hook even though the reservation already
admitted it.

Skip the hook when the request's active `budget_reservation` covers
`spend:user:{user_id}`. The reservation is the source of truth for that
counter cross-pod; the legacy `>=` path remains in place for requests
without a reservation \(e.g. paths that bypass the reservation entirely\).

Reproduces as `tests/otel_tests/test_prometheus.py::test_user_budget_metrics`
on a fresh user with `max_budget=10` calling `fake-openai-endpoint` without
`max_tokens`. Adds focused unit coverage in
`tests/test_litellm/proxy/hooks/test_max_budget_limiter.py`.

* harden bedrock file bucket validation

* Fix syntax errors from botched merge in router.py

* Fix Vertex batch output edge cases

* \[Fix\] RBAC: Drop management_routes Write Fallback for Admin Viewer

Greptile P1: the unsafe-method branch of `_check_proxy_admin_viewer_access`
ended with a blanket `if route in management_routes: return`. That set is a
mix of reads \(info/list — handled via the safe-method GET branch above\) and
writes. The fallback let Admin Viewer POST to write endpoints not enumerated
in `_ADMIN_VIEWER_BLOCKED_WRITE_ROUTES`, including:
  - /team/block, /team/unblock, /team/permissions_update
  - /jwt/key/mapping/{new,update,delete}
  - /key/bulk_update
  - /key/{key_id}/reset_spend

Remove the fallback. The two remaining allow sets \(admin_viewer_routes and
global_spend_tracking_routes\) are both read-only, so removal does not affect
the legitimate POST-as-read cases \(e.g. /spend/calculate, which is in
spend_tracking_routes ⊂ admin_viewer_routes\).

Tests:
  - 8 new parametrized cases pinning each previously-leaking management write
    endpoint to 403 on POST for PROXY_ADMIN_VIEW_ONLY.

* fix\(tests\): anchor VCR redis cassette key to repo root

`os.path.relpath` with no `start` arg uses the current working
directory, so running pytest from a subdirectory produced a
different Redis key than running from the repo root. CI-recorded
cassettes and locally-replayed runs would silently miss each
other's cache.

Anchor the path to the repo root \(derived from `__file__`\) so the
key is stable regardless of CWD.

https://claude.ai/code/session_018uCx7pcrkdUJZrCVMaTdPx

* fix: gate key access_group override on group's own assignment

Replaces the previous intersect-with-team.access_group_ids check, which
made the override unreachable in practice \(the team-gate fallback already
covered every case the intersection allowed\). The override now resolves
each of the key's access_group_ids via get_access_object and accepts the
group only if its assigned_team_ids includes the key's team_id, or its
assigned_key_ids includes the key's token. This fulfills the original ask
\(a key can extend a team's allow-list via a group the admin granted to
that team or that specific key\) while still rejecting foreign groups
referenced by team members of other teams.

* \[Fix\] Proxy/Key Management: Honor team_member_permissions /key/list In /key/list Endpoint

When a team grants /key/list via team_member_permissions, non-admin members
should see all keys for that team — same as a team admin. Previously the
classification in list_keys\(\) only checked admin status, so permitted
members fell into the service-account-only path and could not see other
members' personal keys. Routes those members into the full-visibility set.

* Fix access-group bypass via litellm-model fallback path

When _get_all_deployments returns 0 candidates and the litellm-model
fallback branch \(_get_deployment_by_litellm_model\) finds deployments that
the access-group filter then empties, _access_group_filter_emptied_candidates
remained False \(it was captured before that branch ran\). The router would
then proceed to default fallbacks; the fallback model could have no
access_groups and short-circuit the filter, silently serving a caller
blocked by access-group restrictions.

Update the flag inside the litellm-model branch when filtering empties a
non-empty candidate set so the default-fallback guard still triggers.

* fix\(proxy\): redact MCP server URL and headers for non-admin viewers \(VERIA-8\)

Many MCP integrations \(Zapier, etc.\) embed an upstream API key
directly in the server URL, e.g.
``https://actions.zapier.com/mcp/<api-key>/sse``. The list and
single-server endpoints were returning the full URL to any
authenticated user — `_redact_mcp_credentials` only stripped the
explicit ``credentials`` field, and `_sanitize_mcp_server_for_virtual_key`
only ran for restricted virtual keys. Non-admin internal users could
read the dashboard, click the unmask toggle, and exfiltrate the raw
token.

Add `_sanitize_mcp_server_for_non_admin` that runs on top of the
existing credential redaction and clears the credential-bearing
fields:

- ``url`` \(the primary leak vector\)
- ``spec_path`` \(OpenAPI spec URLs that may carry tokens\)
- ``static_headers`` / ``extra_headers`` \(Authorization\)
- ``env`` \(arbitrary secrets\)
- ``authorization_url`` / ``token_url`` / ``registration_url``

Identity fields \(``server_id``, ``alias``, ``mcp_info``, etc.\) are
preserved so the UI can still list servers a non-admin's team has
access to.

Apply the new sanitizer in `fetch_all_mcp_servers` and the per-server
fetch path right after the existing virtual-key branch. Update the
existing `test_list_mcp_servers_non_admin_user_filtered` assertions
that previously checked URL visibility.

Frontend defense-in-depth: hide the URL unmask toggle on
`mcp_server_view.tsx` unless the viewer is a proxy admin.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* Fix runtime policy attachment initialization

Mark runtime-created policies and attachments initialized so global policy attachments created from the policy builder apply immediately without requiring a restart.

Co-authored-by: Cursor <cursoragent@cursor.com>

* test\(router\): cover _try_early_resolve_deployments_for_model_not_in_names

The router_code_coverage CI check requires every function in router.py to
be referenced by at least one test under tests/{local_testing,
router_unit_tests,test_litellm} in a file with "router" in its name.
The recently-extracted helper had no direct test, so the check failed
with "0.45% of functions in router.py are not tested".

Add a focused test that exercises the four return paths: model already
in self.model_names, no fallback applies, pattern-router match, and
default_deployment substitution \(also asserting the stored default
isn't mutated\).

https://claude.ai/code/session_019AVp1XL7RT9RxRe4qRLkay

* Fix policy registry teardown in tests

Reset the policy ID index during policy engine test cleanup so stale policy versions cannot leak between tests.

Co-authored-by: Cursor <cursoragent@cursor.com>

* fix\(batches\): count non-chat tokens, validate batch-file model access \(VERIA-39\) \(#27015\)

* fix\(batches\): count non-chat tokens and validate every model in batch file

Two security control bypasses on POST /v1/batches:

1. `_get_batch_job_input_file_usage` only summed tokens for
   `body.messages` \(chat completions\). Embedding \(`input`\) and text
   completion \(`prompt`\) batches reported zero, letting massive
   non-chat workloads slip past TPM rate limits. Extend the counter
   to handle string and list shapes for both fields.

2. The batch input file was forwarded to the upstream provider
   without inspecting the models named inside the JSONL — only the
   outer `model` query parameter was checked against the caller's
   allowlist. A caller restricted to gpt-3.5 could submit a batch
   targeting gpt-4o and the upstream would execute it under the
   proxy's shared API key.

Add `_get_models_from_batch_input_file_content` \(returns the
distinct `body.model` values\) and call it from
`_enforce_batch_file_model_access` in the pre-call hook, which runs
each model through `can_key_call_model` so the same allowlist
semantics \(wildcards, access groups, all-proxy-models, team aliases\)
the proxy enforces on `/chat/completions` apply here too. Any
unauthorized model raises a 403 before the file is forwarded.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(batches\): count pre-tokenized prompt/input shapes, classify 403 logs

Two follow-ups from the Greptile review on the batch validation PR:

1. P1 TPM bypass via integer token arrays. The OpenAI batch schema
   accepts ``prompt`` and ``input`` as ``list\[int\]`` \(a single
   pre-tokenized prompt\) or ``list\[list\[int\]\]`` \(multiple\) in addition
   to the string and ``list\[str\]`` shapes. Pre-fix only the string
   shapes were counted, so a caller could submit a batch with hundreds
   of millions of pre-tokenized tokens and the rate limiter would
   record zero. Extract the per-field logic into
   ``_count_prompt_or_input_tokens`` and count each int as one token.

2. P2 access-denial logs were indistinguishable from I/O failures.
   ``count_input_file_usage`` caught every exception under a generic
   "Error counting input file usage" message, so an intentional 403
   from ``_enforce_batch_file_model_access`` looked the same in the
   logs as a missing file or a Prisma timeout. Catch ``HTTPException``
   separately and log 403s at WARNING level with a security-relevant
   message before re-raising.

Tests cover the new shapes: single ``list\[int\]``, ``list\[list\[int\]\]``
\(the worst-case bypass vector\), and embeddings ``input`` with
pre-tokenized arrays.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

---------

Co-authored-by: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(proxy\): re-validate user_id after /user/info re-parses query \(#27009\)

* fix\(proxy\): re-validate user_id ownership after /user/info re-parses query

The route-level access check in `RouteChecks.non_proxy_admin_allowed_routes_check`
reads `request.query_params.get\("user_id"\)`, which decodes literal `+` to
spaces. The endpoint then re-parses the raw query string with `urllib.unquote`
in `get_user_id_from_request` to preserve `+` characters \(so plus-addressed
emails work as user_ids\). Those two paths produce different ids: a caller
who registered a user_id containing a literal space could pass the route
check and then read another user's row by sending the encoded `+` form.

Add `_enforce_user_info_access` and call it after `_normalize_user_info_user_id`
returns the final id. Proxy admin / view-only admin still bypass; everyone
else must match the resolved user_id \(or have no user_id, which falls back
to the caller's own id later in the handler\).

Tests cover the admin bypass, owner-match path, and the cross-user lookup
that this change blocks.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(proxy\): apply user_info ownership check to PROXY_ADMIN_VIEW_ONLY

`_enforce_user_info_access` was bypassing both PROXY_ADMIN and
PROXY_ADMIN_VIEW_ONLY, but the upstream route check in
`RouteChecks.non_proxy_admin_allowed_routes_check` only treats
PROXY_ADMIN as a true admin for the `/user/info` route — view-only
admins go through the `user_id == valid_token.user_id` enforcement
along with regular users. Mirroring that asymmetry left the same
encoded-`+` bypass open for view-only admins whose user_id contains a
literal space.

Drop the PROXY_ADMIN_VIEW_ONLY exemption so the post-decode re-check
matches the upstream rule. Update tests: a view-only admin must now
be blocked from cross-user lookups but still allowed to read their
own row.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

---------

Co-authored-by: yuneng-jiang <yuneng@berri.ai>
Co-authored-by: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* feat\(spend-logs\): opt-in suppression of stack traces in spend-tracking error logs

Adds LITELLM_SUPPRESS_SPEND_LOG_TRACEBACKS env var. When set to true and the
proxy log level is INFO or above, spend-tracking error paths emit a single
ERROR line without the full traceback. Stack traces are preserved at DEBUG
and the Sentry / proxy_logging_obj.failure_handler path is unchanged.

The new spend_log_error helper is wired through the spend write hot path:
  - DBSpendUpdateWriter \(update_database, _update_*_db, batch upsert,
    redis-commit fallbacks\)
  - _ProxyDBLogger._PROXY_track_cost_callback
  - get_logging_payload exception path
  - update_spend / update_daily_tag_spend / spend logs queue monitor

Resolves LIT-2704.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(spend-logs\): preserve no-traceback behavior for update_daily_tag_spend

This call site previously logged a single-line error via verbose_proxy_logger.error\(\)
with no traceback. Switching it to spend_log_error\(..., exc=e\) caused a full stack
trace to render by default \(when LITELLM_SUPPRESS_SPEND_LOG_TRACEBACKS is unset\),
which contradicts the PR goal of leaving default behavior unchanged. Revert this
specific site to the original error log call.

* fix\(spend-logs\): preserve no-traceback behavior for update_daily_tag_spend

Bugbot caught a regression: the previous error log here was a single-line
verbose_proxy_logger.error\(...\) with no traceback. spend_log_error attaches
the active exception's traceback by default \(when the suppression env var
is unset\), so swapping it in changed default behavior. Revert this one site
to its original .error\(\) call to keep the PR strictly opt-in.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* feat\(spend-logs\): suppress traceback in SpendLogs error_information row

Extend LITELLM_SUPPRESS_SPEND_LOG_TRACEBACKS to the failure callback so the
per-row Metadata pane in the UI no longer shows the stack trace when the
opt-in env var is set, matching the existing console-side suppression.

https://claude.ai/code/session_014dztoRbRnRvq54HL9EyHx6

* \[Fix\] Proxy: Repair Merge Fallout In Router-Override Fallback Auth

Conflict resolution for #26968 dropped the `Iterator` typing import
\(NameError at module load\), left a dead `fallback_models = cast\(...\)`
block, and the new tests called `_enforce_key_and_fallback_model_access`
without the now-required `request` kwarg.

* isolate dual OTEL handlers

* harden cloud file compatibility path

* harden cloud file compatibility path

* \[Fix\] Proxy/Key Management: Align Key-Org Membership Checks On Generate And Regenerate

Mirrors the membership rule on /key/update so that /key/generate and
/key/{key}/regenerate apply the same `_validate_caller_can_assign_key_org`
gate when the caller specifies an `organization_id`. Proxy admins bypass.
The check no-ops when `organization_id` is not being set.

* thread trusted params through vertex file content

* trust only server legacy file flag

* chore\(proxy\): keep public AI hub unauthenticated

* fix\(proxy\): preserve low-detail readiness status

* \[Test\] Anthropic: Replace Legacy Claude-4-Sonnet Alias With Haiku 4.5

Three live-API tests pinned to claude-4-sonnet-20250514, which is a
non-canonical alias of claude-sonnet-4-20250514. Anthropic's main API
no longer resolves the legacy form under freshly issued keys, so the
tests fail with not_found_error. The token counter test pinned to
claude-sonnet-4-20250514 itself \(deprecation_date 2026-05-14, two weeks
out\) was on borrowed time too.

Bump all four to claude-haiku-4-5-20251001 — capability superset for what
these tests exercise \(streaming, parallel tool calling, extended thinking,
token counting\), no upcoming deprecation, cheaper per-token.

* chore\(proxy\): move URL-valued model/file_id guard from SDK to proxy

The previous per-provider guards in HuggingFace, Oobabooga, and Gemini
files lived in the SDK layer, breaking SDK callers who legitimately pass
URL-valued model identifiers. Move the check to the proxy boundary in
add_litellm_data_to_request so SDK users keep working while proxy users
default-deny URL-valued model and file_id, with admin opt-in via
litellm.provider_url_destination_allowed_hosts.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* \[Chore\] Proxy/UI: Drop stray _experimental/out/chat/index.html

This file is a regenerable UI build artifact that should not be tracked
in source. Removing so the merge into litellm_internal_staging stays clean.

* \[Test\] Anthropic Passthrough: Bump Streaming Cost-Injection Test To Haiku 4.5

test_anthropic_messages_streaming_cost_injection hits the proxy's
/v1/messages route, which routes via the anthropic/* wildcard to
api.anthropic.com. The 404 surfaced in the test was Anthropic's own
not_found_error propagated back through the proxy \(visible from the
x-litellm-model-id hash on the response — the proxy did route\).

Same root cause as the prior commit: the legacy claude-4-sonnet-20250514
alias is no longer recognized by Anthropic's main API under the new key.
Swap to claude-haiku-4-5-20251001 — same routing path, canonical model.

* fix\(proxy\): handle ownership-recording failures after upstream create

If record_container_owner raises after the upstream container is created,
the user previously got a 500 with no usable container — they were billed
for an unreachable resource. Move ownership recording into the create
path's exception handling and split the two failure modes:

- HTTPException from the recorder \(auth conflicts\) propagates verbatim
  so the client sees the real status code, not a generic LLM error.
- Unexpected exceptions are logged and swallowed; the response is
  returned to the caller so they aren't billed for a container they
  can't address. The DB row stays untracked until an operator reconciles.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(guardrails\): close post-call coverage gaps

* fix\(types\): add /team/permissions_bulk_update to management_routes

The blocklist check in _check_proxy_admin_viewer_access only fires for
routes that match LiteLLMRoutes.management_routes — the bulk-update
endpoint was missing from that list, so the test for view-only admins
on /team/permissions_bulk_update fell through to "allow."

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* \[Test\] Anthropic Passthrough: Bump Thinking Tests Off Legacy Sonnet 4 Alias

base_anthropic_messages_test.test_anthropic_messages_with_thinking and
test_anthropic_streaming_with_thinking still pinned to
claude-4-sonnet-20250514 — the same legacy alias Anthropic no longer
recognizes under freshly issued keys. The other four tests in this base
class already use claude-sonnet-4-5-20250929; these two were missed.

Bump to claude-haiku-4-5-20251001 \(supports_reasoning=true, no upcoming
deprecation\). Subclasses including TestAnthropicPassthroughBasic
inherit these methods.

* fix\(guardrails\): cover multi-choice output variants

* fix\(proxy\): preserve public ai hub ui setting

* fix\(scim\): cascade FK cleanup on user delete and surface block status in UI

SCIM DELETE /Users/{id} previously called litellm_usertable.delete without
clearing rows that FK back to the user, so Postgres rejected the delete with
LiteLLM_InvitationLink_user_id_fkey and the SCIM caller saw a 500. Add a
helper to drop invitation_link, organization_membership, and team_membership
rows before the user delete \(mirrors /user/delete in internal_user_endpoints\).

Also add a Status column to the Virtual Keys and Internal Users tables so
admins can see at a glance which keys are blocked and which users SCIM has
deactivated. SCIM-blocked keys carry a tooltip explaining the origin.

Pin the dashboard's Node version to 20 via .nvmrc to match CI.

* chore: update Next.js build artifacts \(2026-05-02 03:21 UTC, node v20.20.2\)

* perf\(proxy\): cache container/skill ownership reads on the hot path

Container ownership and skill rows are looked up on every retrieve /
delete / list / file-content / chat-completion-with-skill call. The new
stores wrapped raw Prisma queries with no cache, putting one DB
round-trip on each request. Add an in-process TTL'd cache mirroring the
_byok_cred_cache pattern in mcp_server/server.py: per-key \(value,
monotonic_timestamp\), 60s TTL, 10000-entry cap with full-clear on
overflow, invalidated by every write. Negative results \(`None`\) are
cached too so untracked-resource checks also skip the DB.

Tests cover: cache-after-first-hit, negative caching, write
invalidation, no-caching-on-DB-error, TTL expiry, capacity eviction.
56 tests pass.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* chore: update Next.js build artifacts \(2026-05-02 03:39 UTC, node v20.20.2\)

* fix: remove traceback key instead of it being ""

* fix: linting error

* fix\(scim\): preserve scim_active on PUT when client omits the field

A SCIM PUT may legally omit `active` \(full-replace with the field
absent\). Pydantic fills the SCIMUser.active default of True, so the PUT
handler was overwriting metadata.scim_active with True even when the
client never sent it — silently reactivating a previously SCIM-blocked
user and unblocking their keys.

Use model_fields_set to detect whether the client actually sent
`active`. If omitted, preserve the prior scim_active value and skip
the cascade to virtual keys.

Also drop comments added in this PR that just narrate what the code
does; keep only the docstrings and the SQL-NULL pitfall note that
explain non-obvious behaviour.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(proxy\): use set lookup for permitted agent filters

* fix\(mcp\): redact command fields for non-admin server views

* fix\(proxy\): forward decoded container ids after ownership checks

* fix\(caching\): handle stale isolated Redis semantic index

* fix\(cloudflare\): support response_text in streaming chunk parser

Newer Cloudflare Workers AI models \(e.g. Nemotron\) emit 'response_text'
instead of 'response' on streamed chunks. The non-streaming path was
already updated to fall back to 'response_text' \(#26385\), but the
streaming chunk parser still only read 'response', which caused
streaming requests against those models to silently produce empty
content.

Mirror the non-streaming fallback in CloudflareChatResponseIterator.chunk_parser
and add a streaming test for the response_text shape.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* Fix code qa

* Address bugbot: drop dead encode/decode helpers; preserve empty custom_id

- Remove unused _encode_gcp_label_value / _decode_gcp_label_value singular
  helpers; only the _chunks variants are actually called.
- Use 'is not None' check for custom_id so empty-string custom_ids are
  still labeled and round-trip through batch outputs.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* Forward Vertex file content logging context

* test vertex file content logging forwarding

Co-authored-by: Sameer Kankute <Sameerlite@users.noreply.github.com>

* Fix Vertex batch output logging mutation

* fix: don't mutate caller's logging_obj in _try_transform_vertex_batch_output_to_openai

The method was overwriting logging_obj.optional_params, logging_obj.model,
and logging_obj.start_time on the caller's Logging instance. When invoked
from llm_http_handler.py's generic framework path, the framework's own
logging_obj \(which already went through pre_call\) had its properties
clobbered, causing model and start_time to reflect the last batch line's
values rather than the original call context.

Fix: create a fresh local Logging instance for the per-line transformation
instead of mutating the incoming logging_obj. The caller's object is now
left entirely untouched regardless of whether a logging_obj was passed in
or not.

Regression tests added to verify model, start_time, and optional_params
are not mutated on the caller's logging_obj.

Co-authored-by: Sameer Kankute <Sameerlite@users.noreply.github.com>

* feat: add opt-out flag for Vertex batch output transformation

Adds litellm.disable_vertex_batch_output_transformation \(default False\).
When True, afile_content returns raw Vertex predictions.jsonl untouched
so users that parse candidates/modelVersion directly are not broken.

* fix\(anthropic,bedrock\): omit thinking/output_config when reasoning_effort="none"

Setting reasoning_effort="none" on Anthropic chat models \(direct, Bedrock
Invoke, Bedrock Converse, Vertex AI Anthropic, Azure AI Anthropic\) crashed
LiteLLM with:

  litellm.APIConnectionError: 'NoneType' object has no attribute 'get'

Both the Anthropic chat transformation and Bedrock Converse called
``AnthropicConfig._map_reasoning_effort`` and assigned the ``None`` it returns
for ``"none"`` directly to ``optional_params\["thinking"\]``. Downstream
``is_thinking_enabled`` then did ``optional_params\["thinking"\].get\("type"\)``
and crashed.

Pop ``thinking`` \(and on Claude 4.6/4.7, ``output_config``\) instead of
assigning ``None``, restoring the documented contract that
``reasoning_effort="none"`` means "do not enable thinking". This also
prevents downstream Anthropic 400s \("thinking: Input should be an object",
"output_config.effort: Input should be ..."\) if the bug were ever masked.

Verified end-to-end against the live Anthropic API and Bedrock Converse
on claude-opus-4-{5,6,7} and claude-sonnet-4-6, plus Bedrock Invoke for
Claude 4.5/4.6. Vertex AI Anthropic and Azure AI Anthropic inherit the
fixed ``map_openai_params`` from ``AnthropicConfig`` and need no further
changes.

* fix\(vertex-ai\): set response=null on batch error entries per OpenAI spec

The Vertex batch output transformer was emitting both a populated 'response' and 'error' for failed batch entries. The OpenAI Batch output spec defines them as mutually exclusive: on error 'response' MUST be null. This broke any consumer using 'result\["response"\] is None' to detect failures.

* test\(vertex-ai\): cover transformation_error path emits response=null

* fix\(security\): sandbox jinja2 in gitlab/arize/bitbucket prompt managers

DotpromptManager was hardened to render through
ImmutableSandboxedEnvironment. The three sibling managers \(gitlab,
arize, bitbucket\) were missed and still instantiate plain
jinja2.Environment\(\), leaving the same attribute-traversal SSTI
primitive open: a template fetched from a GitLab/BitBucket repo or
Arize Phoenix workspace can reach __class__.__init__.__globals__ and
execute arbitrary Python on the proxy host.

Match the dotprompt pattern by switching all three to
ImmutableSandboxedEnvironment. The sandbox blocks the dunder-traversal
chain while leaving normal {{ var }} substitution intact, so the
template surface is unchanged for legitimate use.

Adds tests/test_litellm/integrations/test_prompt_manager_ssti.py
\(18 cases\) verifying each manager's jinja_env is a sandbox, that
classic SSTI payloads raise SecurityError, and that ordinary variable
rendering still works.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* chore\(proxy\): drop client-supplied pricing fields from request bodies

The proxy currently forwards request-body pricing parameters \(the fields
on `CustomPricingLiteLLMParams`, plus `metadata.model_info`\) into the
core call path. Those fields belong to deployment configuration, not to
per-request input — sending them from a client mutates the request's
recorded cost and, via `litellm.completion` → `register_model`, the
process-wide `litellm.model_cost` map for every later caller in the
worker. Strip them at the boundary.

The strip set is built from `CustomPricingLiteLLMParams.model_fields` so
pricing fields added later are covered automatically. Operators who do
want clients to supply per-request pricing can opt back in per key or
team via `metadata.allow_client_pricing_override = true`, mirroring the
existing `allow_client_mock_response` and
`allow_client_message_redaction_opt_out` flags.

Tests cover the strip set's coverage, root and metadata strips, the
opt-in skip on both key and team metadata, and a regression check that
the global `litellm.model_cost` map is unmutated after a stripped
request.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* chore\(proxy\): log stripped pricing fields at debug for operator visibility

Operators upgrading would otherwise see client-supplied pricing overrides
silently stop applying with no diagnostic. Emit a debug-level line listing
the dropped fields and pointing at the opt-in flag when any are stripped;
stay silent on the no-op path so the log isn't filled with noise.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(proxy\): move pricing strip below the litellm_metadata JSON-string parse

The strip ran before the proxy parses ``litellm_metadata`` from a JSON
string into a dict \(a path used by multipart/form-data and ``extra_body``
callers\), so ``isinstance\(metadata, dict\)`` was False and ``model_info``
survived the strip. Move the call to the same post-parse position the
``user_api_key_*`` strip already uses for the same reason. Adds a
regression test exercising the JSON-string ``litellm_metadata`` path.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* test\(responses\): replace legacy claude-4-sonnet alias in multiturn tool-call test

Anthropic's main API no longer resolves the non-canonical 'claude-4-sonnet-20250514'
alias for freshly issued keys, returning 404 not_found_error. PR #27031 already
swept three other live tests pinned to this alias to claude-haiku-4-5-20251001
but missed test_multiturn_tool_calls in the responses API suite, which is now
failing reliably on PR CI runs \(e.g. PR #27074, job 1603363\).

Bump the two model references in test_multiturn_tool_calls to the same
claude-haiku-4-5-20251001 snapshot used by PR #27031 -- it covers everything
this test exercises \(tool calling, multi-turn\) and isn't on a deprecation
schedule.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* chore\(proxy\): close callback-config and observability-credential side channels

Two related gaps in the proxy's request bouncer:

1. ``is_request_body_safe`` \(auth_utils.py\) walked the request-body root
   and the ``litellm_embedding_config`` nested dict, but not ``metadata``
   or ``litellm_metadata``. The same fields it bans at root — Langfuse /
   Langsmith / Arize / PostHog / Braintrust / Phoenix / W&B Weave / GCS /
   Humanloop / Lunary credentials and routing — were silently accepted
   when the caller put them inside metadata, retargeting observability
   callbacks to a caller-controlled host with caller-supplied creds.
   Walk both metadata containers \(and parse the JSON-string form sent via
   multipart / ``extra_body``\) through the same banned-params helper, so
   the existing ``allow_client_side_credentials`` opt-in covers both
   paths consistently.

2. The banned-params list was hand-maintained and lagged the canonical
   ``_supported_callback_params`` allow-list in
   ``initialize_dynamic_callback_params``. Derive the observability bans
   from that allow-list \(minus a small ``_SAFE_CLIENT_CALLBACK_PARAMS``
   set for informational fields like ``langfuse_prompt_version`` and
   ``langsmith_sampling_rate``\) so future integrations are covered
   automatically; ``_EXTRA_BANNED_OBSERVABILITY_PARAMS`` carries the
   handful of fields integrations read but the allow-list hasn't caught
   up to. A guard test fails CI if a new entry is added to
   ``_supported_callback_params`` without an explicit safe-list decision.

Separately in ``litellm_pre_call_utils.py``: add ``callbacks``,
``service_callback``, ``logger_fn``, and ``litellm_disabled_callbacks``
to ``_UNTRUSTED_ROOT_CONTROL_FIELDS``. The first three are appended to
worker-wide ``litellm.{input,success,failure,_async_*,service}_callback``
lists / ``litellm.user_logger_fn`` from inside ``function_setup`` — one
request poisons every subsequent caller in that worker. The last is the
inverse primitive: the legitimate path reads it from key/team metadata,
the request-body version silently disables admin-configured audit /
observability for the call.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(auth\): per-param allow must continue, not return early

A pre-existing logic bug in ``_check_banned_params``: when the
deployment-level ``configurable_clientside_auth_params`` permitted one
banned field, the loop ``return``-ed on the first match instead of
``continue``-ing, so any other banned param later in the same body or
metadata dict was never checked. This PR's metadata walk multiplies the
surface where that bypass matters — a body pairing an allowed
``api_base`` with an observability credential like ``langfuse_host``
would silently pass.

Proxy-wide ``allow_client_side_credentials`` keeps ``return`` \(it's a
global opt-in for every banned param\). The per-param branch becomes
``continue`` so only the one explicitly-permitted field is skipped.

Adds a regression test that exercises the api_base + langfuse_host pair.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(vector_store\): resolve embedding config at request time, never persist creds

The vector store create/update path previously called
``_resolve_embedding_config`` against the admin-configured router/DB
model and persisted the resolved ``litellm_embedding_config`` dict
\(``api_key`` / ``api_base`` / ``api_version``\) into the
``litellm_managedvectorstorestable.litellm_params`` column. Because the
resolver expanded ``os.environ/...`` references via ``get_secret``, the
DB row carried cleartext provider credentials, and the
``/vector_store/{new,info,update,list}`` responses returned them to any
authenticated caller who could supply a known admin model name.

Move the auto-resolve out of ``create_vector_store_in_db`` and out of
the update path. Persist only the user-supplied ``litellm_embedding_model``
reference. Resolve at request-handling time inside
``_update_request_data_with_litellm_managed_vector_store_registry`` so
the resolved config lives in the per-request ``data`` dict and is
garbage-collected after the response. Legacy rows that were created by
an earlier proxy version and already carry a resolved
``litellm_embedding_config`` skip the re-resolution and pass through
unchanged so embedding calls keep working.

The ``new_vector_store`` response now also runs the existing
``_redact_sensitive_litellm_params`` masker \(already used by ``info``,
``update``, and ``list``\), defending against caller-supplied cleartext
on the create path and against legacy rows whose persisted credentials
are still in the database.

Existing tests that asserted the old write-time-resolve behaviour are
updated to assert the new persistence shape \(no embedding config
stored, just the model reference\). Two new tests cover the use-time
path: one asserting fresh resolution happens when a row carries only
the model reference, the other asserting legacy rows with persisted
config skip re-resolution and continue to work.

Co-Authored-By: Claude Opus 4.7 \(1M context\) <noreply@anthropic.com>

* fix\(vector_store\): tighten registry-mutation comment and dedupe test helpers

* fix\(vector_store\): cache use-time embedding-config resolution

Hold the resolved config in a process-memory TTL cache so the
request-handling path doesn't run litellm_proxymodeltable.find_first
on every vector-store call.

* fix\(anthropic,bedrock,vertex\): forward output_config.effort + 400 on garbage reasoning_effort

Follow-up bugs surfaced by the QA sweep on PR #27039
\(https://github.com/BerriAI/litellm/pull/27039#issuecomment-4363363610\).

1. Stop stripping output_config.effort on Bedrock + Vertex adaptive routes.
   - Vertex AI Claude 4.6/4.7 accepts output_config.effort on rawPredict
     \(verified end-to-end against us-east5 / global\). The strip helper now
     no-ops for effort.
   - Bedrock Converse routes output_config into additionalModelRequestFields
     for anthropic base models so the requested adaptive tier \(low/medium/
     high/xhigh/max\) actually reaches the wire instead of all collapsing to
     identical thinking.
   - Bedrock Invoke chat transformation \(AmazonAnthropicClaudeConfig\) stops
     popping output_config from the post-AnthropicConfig request body.
   - Bedrock Invoke /v1/messages allowlist \(BedrockInvokeAnthropicMessagesRequest\)
     now lists output_config so the runtime allowlist filter forwards it.

2. Validate effort across Bedrock Converse so 'disabled' / 'invalid' / '' /
   unsupported tiers \(xhigh/max on Sonnet 4.6 or budget-mode 4.5 models\)
   surface as a clean 400 BadRequestError instead of 500.

3. ValueError -> BadRequestError throughout \(AnthropicConfig.map_openai_params,
   _apply_output_config, AmazonConverseConfig._handle_reasoning_effort_parameter\).
   Empty-string effort is now rejected \(was silently passing the
   'if effort and ...' short-circuit\).

4. Floor reasoning_effort='minimal' at the Anthropic provider minimum
   \(1024 budget_tokens\) via new ANTHROPIC_MIN_THINKING_BUDGET_TOKENS so it's
   a usable tier on direct Anthropic / Azure AI Anthropic / Vertex AI Anthropic /
   Bedrock Invoke \(all of which 400 below 1024\).

5. model_prices: dedupe duplicate supports_max_reasoning_effort key on
   claude-opus-4-7 / claude-opus-4-7-20260416.

Adds regression tests across all five affected paths; existing tests asserting
the silent-strip behavior were updated to reflect the new pass-through and
clean 400 surfaces.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(constants\): make ANTHROPIC_MIN_THINKING_BUDGET_TOKENS a plain constant

The documentation CI test \(tests/documentation_tests/test_env_keys.py\)
asserts every os.getenv\(\) key in the source has a matching entry in the
litellm-docs config_settings.md table. ANTHROPIC_MIN_THINKING_BUDGET_TOKENS
tracks Anthropic's published wire-protocol minimum \(1024\) — it's not a
user-tunable, so making it env-overridable was wrong anyway. Drop the
os.getenv\(\) wrapper; the value is now a plain literal.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(anthropic,bedrock\): correct effort error message and dedupe effort_map

- Remove 'none' from the Bedrock _validate_anthropic_adaptive_effort error
  message; it was listed as a valid value but rejected by the membership
  check, leaving users in a feedback loop if they tried 'none'.
- Hoist the duplicated reasoning_effort -> output_config.effort mapping
  out of AnthropicConfig.map_openai_params and
  AmazonConverseConfig._handle_reasoning_effort_parameter into a single
  AnthropicConfig.REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT class constant
  so the two routes cannot drift.

* fix\(anthropic\): translate reasoning_effort on /v1/messages route

Closes the remaining QA-sweep gap on PR #27074: Bedrock Invoke
/v1/messages was silently ignoring ``reasoning_effort`` because the
shared param filter only kept native Anthropic keys, so every effort
tier collapsed to the same behavior on the wire \(27/231 cells failing
across opus-4-5 / opus-4-6 / sonnet-4-6\).

Map ``reasoning_effort`` to native Anthropic ``thinking`` /
``output_config.effort`` at the ``AnthropicMessagesConfig`` layer so
all four /v1/messages routes \(direct Anthropic, Azure AI, Vertex AI,
Bedrock Invoke\) inherit the same translation:

- Add ``reasoning_effort`` to ``AnthropicMessagesRequestOptionalParams``
  so the param filter in
  ``AnthropicMessagesRequestUtils.get_requested_anthropic_messages_optional_param``
  no longer drops it before the transformation runs.

- Add ``_translate_reasoning_effort_to_anthropic`` and call it from
  ``transform_anthropic_messages_request``. Mirrors
  ``AnthropicConfig.map_openai_params`` on the chat completion path
  \(re-uses ``_map_reasoning_effort`` and
  ``REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT``\) so the two routes
  cannot drift. Pops ``reasoning_effort`` so it never reaches the wire.

- Caller-supplied native ``thinking`` / ``output_config.effort`` always
  win — same precedence as
  ``_translate_legacy_thinking_for_adaptive_model``.

- Garbage values \(``""``, ``"disabled"``, ``"invalid"``\) raise
  ``AnthropicError\(status_code=400\)`` instead of falling through and
  surfacing as 500s from the provider.

- ``"none"`` clears thinking + output_config so callers can opt out
  per request.

Also restores the non-adaptive-model test coverage on Bedrock Invoke
/v1/messages that the previous commit lost when
``test_bedrock_messages_strips_output_config`` was renamed to the
``forwards`` variant on Opus 4.7.

Adds a new test file
``test_reasoning_effort_translation.py`` covering the translation at
the shared config level \(adaptive + non-adaptive models, none, garbage,
caller precedence\) so all four /v1/messages routes are exercised by a
single suite.

Adds parametrized + behavioral tests on the Bedrock Invoke /v1/messages
suite covering: minimal/low/medium/high/xhigh/max mapping for adaptive
models, thinking-budget mapping for non-adaptive Opus 4.5, ``none``
clears both, garbage raises 400, explicit ``output_config`` wins.

Refs: https://github.com/BerriAI/litellm/pull/27074

* fix\(anthropic,bedrock\): reject unmapped reasoning_effort at mapping site

Both the chat completion path \(AnthropicConfig.map_openai_params\) and the
Bedrock Converse path \(_handle_reasoning_effort_parameter\) used
REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT.get\(value, value\) which falls
back to the raw input on unmapped keys. Combined with _map_reasoning_effort
returning type='adaptive' for any string on Claude 4.6/4.7, garbage values
\(e.g. 'disabled'\) could leak into optional_params\['output_config'\]\['effort'\]
unvalidated if map_openai_params ran without the downstream transform_request
or _validate_anthropic_adaptive_effort check.

Mirror the /v1/messages pattern: use .get\(value\) \(no fallback\) and raise
BadRequestError immediately when the value is unmapped, co-locating
validation with the mapping for defense in depth.

* style: black formatting

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* fix\(anthropic\): stop class-attr leak; gate xhigh/max on every route

The reasoning-effort mapping dict was a public class attribute on
AnthropicConfig, so BaseConfig.get_config returned it as a request
parameter and every Anthropic-backed call \(Anthropic / Azure / Vertex /
Bedrock Invoke\) hit a 400 'REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT:
Extra inputs are not permitted' from the provider. Move the mapping
to a module-level constant.

_supports_effort_level only looked the model up under
custom_llm_provider='anthropic', so bedrock-prefixed model ids
\(e.g. bedrock/invoke/us.anthropic.claude-opus-4-7\) returned False
for both 'max' and 'xhigh' even when the underlying model entry has
the flag set. Strip known provider prefixes and retry the lookup
against litellm.model_cost directly so per-model gating works on
every route.

Mirror the per-model xhigh/max gate from
AnthropicConfig._apply_output_config in
AnthropicMessagesConfig._translate_reasoning_effort_to_anthropic so
the /v1/messages route also raises a clean 400 instead of forwarding
the unsupported tier.

* feat\(anthropic,bedrock\): strip output_config under drop_params for non-effort models

When a proxy fronts Claude Code \(which always sends `output_config.effort`\)
at a pre-4.5 Anthropic model — haiku-3, sonnet-3.5, opus-3, sonnet-4 — the
forwarded knob causes a forced 400 the client can't fix. Gating a strip
behind the existing `drop_params` flag lets operators opt into silent
fixup once and stop worrying about per-model param hygiene.

Default \(`drop_params=False`\) still forwards and surfaces the provider's
error, preserving the strict, debuggable contract from #27074.

Per https://platform.claude.com/docs/en/build-with-claude/effort the
supporting set is Opus 4.5+, Sonnet 4.6+, and Mythos Preview; everything
else is dropped \(with a verbose_logger warning so the strip is visible\).
Recognition uses model-name patterns plus a fallback to any
`supports_*_reasoning_effort` flag in the model map for forward
compatibility with new entries.

https://claude.ai/code/session_01WjHq31rvXT6xYNdVmSJvRp

\(cherry picked from commit 1233943e7861ba8a9062f792310ebd401cb03db8\)

* fix\(base_llm\): filter all _-prefixed class attrs from get_config

The drop_params strip work added `AnthropicConfig._EFFORT_SUPPORTING_MODEL_PATTERNS`
as a private class-level lookup tuple. `BaseConfig.get_config\(\)` only
filtered the `__`-prefixed names plus `_abc` / `_is_base_class`, so
`_EFFORT_SUPPORTING_MODEL_PATTERNS` would have leaked into the request
body the same way `REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT` did before
the previous commit.

Generalize the existing `_abc` / `_is_base_class` carve-outs to skip
every `_`-prefixed name. `AmazonConverseConfig.get_config\(\)` overrides
the base method, so apply the same change there.

Also unblocks future internal helpers from accidentally serialising into
the wire body.

* fix\(anthropic\): drive output_config.effort support from model map flags

Replace hardcoded _EFFORT_SUPPORTING_MODEL_PATTERNS with a JSON-backed
check that uses supports_*_reasoning_effort flags from the model map.
Add supports_minimal_reasoning_effort: true to opus-4-5 and mythos-preview
entries \(which previously only carried supports_reasoning\) so the JSON
remains the single source of truth for effort capability.

* fix\(anthropic,bedrock,databricks\): four reasoning_effort follow-ups

- claude-sonnet-4-6 + reasoning_effort=max no longer 400s. Renamed
  _is_opus_4_6_model to _is_claude_4_6_model at three sites and added
  supports_max_reasoning_effort: true to 12 model entries in the JSON
  cost map \(10 sonnet 4.6 ids + OpenRouter opus 4.6/4.7\).
- _map_reasoning_effort now raises BadRequestError\(400\) directly with
  llm_provider, instead of letting Databricks \(and similar callers\)
  surface its raw ValueError as a 500.
- output_config.effort on Opus 4.5 over Bedrock no longer 400s for
  missing effort-2025-11-24 beta. Flipped JSON to "effort-2025-11-24"
  for bedrock + bedrock_converse and added an auto-attach branch in
  _process_tools_and_beta for non-adaptive Anthropic + output_config
  on Converse.
- reasoning_effort=xhigh / =max on legacy budget-mode models
  \(Haiku 4.5, Sonnet 4.5, Opus 4.5\) now map to thinking.budget_tokens
  8192 / 16384 instead of returning 400. Added two constants in
  litellm/constants.py.

Tests updated for all four flips. Validated end-to-end via 306-cell
live proxy matrix \(6 model families x 3 routes x 17 effort cases\),
all pass.

* fix\(databricks\): validate reasoning_effort and set output_config on adaptive Claude

The Databricks path called `AnthropicConfig._map_reasoning_effort` for
Claude models but never validated the effort string nor set
`output_config.effort` for adaptive models \(Claude 4.6/4.7\). Since
`_map_reasoning_effort` returns `type=adaptive` for ANY non-None /
non-"none" string on adaptive models \(including "disabled",
"invalid", ""\), Databricks silently accepted garbage and emitted a
request without an `output_config.effort`, collapsing every adaptive
tier to identical behavior.

Match the Anthropic native, Bedrock Converse, Bedrock Invoke, and
/v1/messages paths: when the resolved `thinking` is non-None on a
4.6/4.7 model, look up the value in
`REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT` and either raise a clean
`BadRequestError` or set `optional_params\["output_config"\]`.

* fix\(azure\): omit model from image generation and image edit deployment requests

Azure OpenAI routes image gen/edit by deployment in the URL; sending the
deployment id in model breaks gpt-image-2 \(invalid_value\). Strip model from
JSON for deployments/.../images/generations and from multipart data for
.../images/edits. Non-deployment URLs \(e.g. Azure AI FLUX\) unchanged.

Fixes #26316.

Co-authored-by: Cursor <cursoragent@cursor.com>

* test\(azure\): exercise image gen JSON filter via HTTP client; dedupe image edit URL

- Image generation tests patch HTTPHandler.post / get_async_httpx_client so
  make_*_azure_httpx_request runs and wire json is asserted on call kwargs.
- Azure image edit: strip model in finalize_image_edit_multipart_data using the
  same URL string the handler passes to POST \(no second get_complete_url in
  transform\). BaseImageEditConfig default finalize is a no-op.

Co-authored-by: Cursor <cursoragent@cursor.com>

* fix\(azure_ai/anthropic\): promote output_config out of extra_body so validation runs

`azure_ai` is registered in `litellm.openai_compatible_providers`, so
`add_provider_specific_params_to_optional_params` \(litellm/utils.py\)
auto-stuffs any non-OpenAI kwarg \(e.g. `output_config={"effort": "..."}`\)
into `optional_params\["extra_body"\]`. `AzureAnthropicConfig.transform_request`
then strips `extra_body` entirely on the way out, silently dropping the
param — and `AnthropicConfig._apply_output_config` never sees it, so
`effort="invalid"` / `effort="xhigh"` on a non-supporting model
quietly reaches the model with default behavior instead of returning a
clean 400 \(as the native `anthropic` provider does\).

Promote the keys back to top-level `optional_params` \(using `setdefault`
so explicit top-level values win\) before delegating to the parent
`AnthropicConfig`. Apply in both `validate_environment` and
`transform_request` so flag detection \(`is_mcp_server_used`, etc.\) and
output-config validation both run.

Surfaced by the QA matrix expansion on PR #27074: 20 cells where Azure
returned 200 while `anthropic` returned 400 — all `output_config` mode
across haiku_4_5, sonnet_4_5, opus_4_5, sonnet_4_6, opus_4_6, opus_4_7
families with `effort` in {invalid, xhigh, max, low, medium, high}.

Tests:
* `test_output_config_promoted_from_extra_body`: valid effort reaches data
* `test_invalid_output_config_effort_raises_via_extra_body`: 400 on bad effort
* `test_unsupported_effort_xhigh_raises_via_extra_body`: 400 on xhigh-on-Sonnet-4.6
* `test_extra_body_promotion_does_not_clobber_top_level`: setdefault semantics

* test\(image_gen\): expect no model in Azure image edit multipart \(#26316\)

Align test_azure_image_edit_litellm_sdk with deployment-scoped Azure edits.

Co-authored-by: Cursor <cursoragent@cursor.com>

* refactor\(anthropic\): extract _validate_effort_for_model to prevent drift

The chat completion path \(`_apply_output_config`\) and the /v1/messages
pass-through \(`AnthropicMessagesConfig._translate_reasoning_effort_to_anthropic`\)
both gate `max` / `xhigh` per model. The two sites had diverged from
near-identical copies into separately maintained blocks, creating a real
drift risk when a new model tier \(e.g. Claude 4.8\) lands -- a contributor
could update one site and miss the other.

Centralise the gating in `AnthropicConfig._validate_effort_for_model`,
which returns an error message string or `None`. Each call site keeps
its own provider-appropriate exception type \(`BadRequestError` for the
chat path, `AnthropicError` for the /v1/messages pass-through\) but the
gating decision now comes from one place. Net -11 LOC.

Adds a parametrised unit test exercising the helper directly across
4.5 / 4.6 / 4.7 model families and `max` / `xhigh` / lower-effort
inputs. Existing tests at both call sites continue to pass unchanged.

Addresses Greptile finding on PR #27074.

* fix\(databricks\): narrow reasoning_effort_value to str for mypy

`non_default_params.get\("reasoning_effort"\)` returns `Any | None`,
but `REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT.get\(\)` expects `str`.
Mypy flagged this on the strict pass. Narrow with `isinstance` before
the lookup; non-strings fall through to the existing `BadRequestError`
below with a clean validation message, so behavior is unchanged.

Fixes a regression introduced by 1a10746e95 in this PR.

* feat\(proxy\): add health_check_reasoning_effort for model health checks

Co-authored-by: Cursor <cursoragent@cursor.com>

* test\(image_gen\): align Azure image gen fixture with body omitting model

Expected JSON matches deployment-scoped Azure POST \(#26316\).

Co-authored-by: Cursor <cursoragent@cursor.com>

* test\(anthropic/chat\): force PR-local model_cost map via autouse fixture

CI runs without LITELLM_LOCAL_MODEL_COST_MAP=True, so litellm.model_cost
is loaded from main-branch JSON \(default model_cost_map_url\) instead of
the PR's checked-out model_prices_and_context_window.json. Tests that
assert per-model flags added in this PR \(supports_max_reasoning_effort,
supports_xhigh_reasoning_effort\) therefore pass locally but fail in CI
with 'AssertionError: assert False is True' on 5 cases:

  - test_anthropic_model_supports_effort_param_recognizes_supporting_models
    \[anthropic.claude-mythos-preview, bedrock/.../mythos-preview,
     claude-opus-4-5-20251101\]
  - test_supports_effort_level_handles_provider_prefixes
    \[bedrock/invoke/us.anthropic.claude-sonnet-4-6-max-True,
     claude-sonnet-4-6-max-True\]

Add an autouse fixture at tests/test_litellm/llms/anthropic/chat/conftest.py
that monkey-patches litellm.model_cost to the PR-local JSON for every test
in this directory. The parent conftest already snapshots+restores
litellm.model_cost per-function, so the mutation is contained.

This is a scoped workaround. The proper fix is to set the env var
globally in the test workflow once the ~10 inline self-set test files
are audited; tracking that as a follow-up issue.

* \[Fix\] Docker: Pin Wolfi And Uv To Multi-Arch Index Digests

The previous pins resolved to single-platform amd64 manifests, so buildx
pulled the same amd64 base for both linux/amd64 and linux/arm64 targets.
The published OCI index then advertised an arm64 entry whose layers are
byte-identical to amd64 -- arm64 users got an amd64 binary.

Switch all three Dockerfiles to the multi-arch image-index digests:
  - cgr.dev/chainguard/wolfi-base   \(index has linux/amd64 + linux/arm64\)
  - ghcr.io/astral-sh/uv:0.11.7     \(index has linux/amd64 + linux/arm64\)

Resolved with `docker buildx imagetools inspect <ref>` -- that returns
the index digest. `docker pull` + `docker inspect` returns the per-host
platform digest, which is what slipped in last time.

* \[Fix\] Docker: Pin Uv To Multi-Arch Index Digest In Remaining Dockerfiles

Apply the same fix to the three Dockerfiles not in the release pipeline
today \(alpine, dev, health_check\) so they stay correct if/when they're
built for arm64 in the future.

Wolfi pins are not present in these files; the python:3.11-alpine and
python:3.13-slim digests they already use are multi-arch indexes that
include arm64/v8, so only the uv pin needed swapping.

* fix\(xai\): fold reasoning_tokens into completion_tokens to satisfy OpenAI invariant

xAI's chat completions API accounts reasoning_tokens separately from
completion_tokens, but rolls them into total_tokens. This breaks the
OpenAI invariant total_tokens == prompt_tokens + completion_tokens
that downstream consumers \(including litellm's own _usage_format_tests
in tests/llm_translation/base_llm_unit_tests.py:58\) rely on.

Live capture \(grok-3-mini-beta, 2026-05-04\):
    prompt=14, completion=10, total=336, reasoning=312
    14 + 10 = 24, NOT 336.

OpenAI's o1/o3 reasoning models include reasoning_tokens in
completion_tokens, leaving the prompt+completion=total invariant
intact. xAI deviates. This patch aligns xAI to OpenAI semantics by
folding reasoning_tokens into completion_tokens after the parent
OpenAI parser runs.

The fold is idempotent and defensive:
- Only fires when total_tokens == prompt_tokens + completion_tokens
  + reasoning_tokens \(the documented xAI shape\). Refuses to fold if
  the gap doesn't match, guarding against silent corruption when xAI
  changes accounting.
- Skips if completion_tokens already covers the gap \(already
  normalised — e.g. cost calc replays a previously-folded Usage\).

xai.cost_calculator.cost_per_token already added reasoning_tokens to
the visible completion count for billing. Post-fold the Usage block
now satisfies that invariant directly, so the cost calc would
double-bill. Updated cost_per_token to detect the OpenAI-normalised
shape \(total == prompt + completion\) and skip the reasoning add-on
in that case, falling through to the legacy raw-shape behaviour for
callers that bypass the transformation \(e.g. proxy log replay\).

Tests:
- Adds TestXAIReasoningTokenFolding covering: gap-explained-fold,
  idempotent-no-double-fold, no-reasoning-skip, gap-mismatch-skip.
- Adds test_already_normalised_usage_does_not_double_count_reasoning
  to lock the cost-calc idempotency.
- Updates 7 pre-existing cost-calc tests whose total_tokens was
  internally inconsistent \(used the OpenAI-normalised total but kept
  reasoning_tokens external\) to use the documented xAI raw shape
  total = prompt + visible completion + reasoning. Pre-existing
  values masked the missing-fold by accident.

Verified end-to-end against the live xAI API:
    LITELLM_LOCAL_MODEL_COST_MAP=False \(CI default\) +
    XAI_API_KEY set +
    pytest tests/llm_translation/test_xai.py::TestXAIChat::test_prompt_caching
        -> PASSED in 18.81s \(was: AssertionError on
        usage.total_tokens == usage.prompt_tokens + usage.completion_tokens\)

20/20 tests in tests/test_litellm/llms/xai/test_xai_cost_calculator.py
and 8/8 in tests/test_litellm/llms/xai/test_xai_chat_transformation.py
pass.

* refactor\(bedrock/converse\): delegate effort gating to AnthropicConfig._validate_effort_for_model

Removes the duplicated max/xhigh gating logic in
_validate_anthropic_adaptive_effort and the now-unused
_supports_effort_level_on_bedrock helper. Per-model gating now flows
through the centralized AnthropicConfig._validate_effort_for_model
\(whose _supports_effort_level already strips Bedrock prefixes\), so the
chat completion, /v1/messages, and Bedrock Converse paths can't drift
when a new gated effort tier is added.

* Implement normalize_nonempty_secret_str function to trim whitespace from secrets and treat empty values as unset. Update proxy_server to use this function for Grafana credentials. Enhance tests to validate the new normalization behavior.

* Fix qdrant semantic cache miss metadata

* chore\(deps\): refresh dependency locks

* chore\(deps\): authorize pytest license

* fix: preserve tokenizer decode round trips

* refactor\(anthropic\): drive adaptive-thinking gate via supports_adaptive_thinking flag

Three of greptile's open comments on #27074 \(P2 converse:512, P1
databricks:361, and the underlying capability-flag policy rule\) flagged
the same pattern: _is_claude_4_6_model\(...\) or _is_claude_4_7_model\(...\)
used inline as a runtime 'is this an adaptive-thinking model?' check.
That requires a code release each time a new adaptive Claude lands.

Consolidate the inline gating to AnthropicModelInfo._is_adaptive_thinking_model,
and switch the helper itself to read a new supports_adaptive_thinking
flag from `model_prices_and_context_window.json` via `_supports_factory`,
falling back to the family pattern only when the model-map entry doesn't
carry the flag \(preserves OpenRouter / Vercel / Bedrock-prefixed variants
that route through the same code path with non-canonical ids\).

Adds `supports_adaptive_thinking: true` to the four 4.6/4.7 anthropic
entries \(opus-4-6 + dated, opus-4-7 + dated, sonnet-4-6\). Bedrock-prefixed
and Vertex-prefixed entries don't need the flag because both fall back
through the family pattern \(the helper short-circuits early on True from
either path\) and the bedrock/vertex Claude IDs all match the existing
opus-4-{6,7} / sonnet-4-{6,7} pattern.

Affected call sites:

- `bedrock/chat/converse_transformation.py:_handle_reasoning_effort_parameter`
- `anthropic/chat/transformation.py:_map_reasoning_effort`
- `anthropic/chat/transformation.py:map_openai_params` \(output_config branch\)
- `databricks/chat/transformation.py:map_openai_params` \(output_config branch\)

The remaining `_is_claude_4_6_model` / `_is_claude_4_7_model` references
in `AnthropicConfig._validate_effort_for_model` and
`AnthropicConfig.get_supported_openai_params` are intentionally retained:
they're per-model gating fallbacks for variants whose model-map entries
don't yet carry the `supports_max_reasoning_effort` /
`supports_reasoning` flag. Those are documented in-place.

Tests: 537 anthropic/bedrock/databricks/vertex/messages tests pass.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* chore\(deps\): address dependency review notes

* test\(model_prices\): add supports_adaptive_thinking to schema

`test_aaamodel_prices_and_context_window_json_is_valid` validates the
model-map JSON against an explicit schema with `additionalProperties`,
so the new `supports_adaptive_thinking` flag added in
98ced0ae43 needs a matching schema entry.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* refactor: remove unnecessary comments from #27074

Strip out the explanatory and historical comments that don't carry
business-logic justification. Comments that simply narrate what code
does — or that explain prior behavior, what was changed, or which PR
introduced a fix — are removed. Docstrings are reduced to a one-line
summary where the long form repeated information already evident from
the code or test data.

No code-behavior changes. All 643 affected unit tests still pass.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* test: keep decode token test local

* chore\(deps\): align dashboard node engine

* feat: selectively apply routing strategy according to model name

* style: make _model_supports_effort_param more concise

* refactor\(anthropic,bedrock\): hoist drop_params output_config warning to module constant

Three call sites \(anthropic chat, bedrock converse, bedrock invoke messages\)
emitted the same '...Effort is only supported on Opus 4.5+, Sonnet 4.6+, and
Mythos Preview' warning verbatim. Extract DROP_UNSUPPORTED_OUTPUT_CONFIG_WARNING
in litellm/llms/anthropic/chat/transformation.py and import it from the bedrock
sites so future copy edits live in one place.

Addresses Michael's review on PR #27074.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* refactor\(anthropic,bedrock,databricks\): factor BadRequestError for unknown reasoning_effort

Three call sites raised the same BadRequestError\("Invalid reasoning_effort:
... Must be one of 'minimal', 'low', ..."\) block when REASONING_EFFORT_TO_OUTPUT_CONFIG_EFFORT
returned None: anthropic chat map_openai_params, bedrock converse
_handle_reasoning_effort_parameter, and databricks chat reasoning_effort path.

Extract AnthropicConfig._raise_invalid_reasoning_effort\(model, value, llm_provider\)
so future copy edits / valid-set changes happen in one place. Typed as NoReturn
so type-checkers correctly narrow control flow at call sites.

Addresses Michael's review on PR #27074.

Co-authored-by: Mateo Wang <mateo-berri@users.noreply.github.com>

* Clean up Redis semantic cache isolation fallback

* fix\(guardrails\): align banned_keywords + azure_content_safety call_type gates with runtime route_type

The hooks gated on ``call_type == "completion"`` but the proxy ingress
passes ``route_type`` straight through as ``call_type`` —
``"acompletion"`` for /v1/chat/completions and ``"aresponses"`` for
/v1/responses. Tests passed because they used the literal sync
``"completion"`` value, masking the gap.

Switch both hooks to ``is_text_content_call_type`` \(matches the
canonical runtime values: completion / acompletion / aresponses\) and
update existing tests to assert against runtime values, plus parametrize
a regression test that pins the gate.

* fix: remove unused import

* Add semantic cache legacy migration flag

* Treat 0 team_member_budget as no cap

* chore\(caching\): annotate qdrant quantization_params dict type

Mypy infers the dict's value type from the first branch
\(Dict\[str, bool\]\) which clashes with the scalar branch's mixed-type
inner dict. Explicit Dict\[str, Any\] annotation lifts the inference.

* chore\(caching\): remove allow_legacy_unscoped_cache_hits opt-in

The flag was an opt-in escape hatch for the cross-tenant leak the rest
of the patch closes — flipping it on \(env var or constructor param\)
re-enables exactly the VERIA-54 primitive on either backend. There is
no operational need that the secure path doesn't already meet:

- Qdrant: legacy points without ``litellm_cache_key`` payload are
  excluded by the must-clause filter and treated as misses; new sets
  populate the cache key, so cold-start lasts only as long as the
  natural cache rebuild.
- Redis: existing unscoped index can't carry the new schema; the init
  path falls back to ``{name}_isolated`` \(and recreates it on stale
  schema\), leaving the legacy index untouched.

Drop the construc…")  | May 5, 2026  |  
|   |   | [fix(packaging): keep wheel paths under Windows MAX_PATH for Store Pyt…](https://github.com/BerriAI/litellm/commit/f39c811d3419607228c907db4bbf1ebcf3628ff2 "fix\(packaging\): keep wheel paths under Windows MAX_PATH for Store Python \(#43903\)

* fix\(packaging\): keep wheel paths under Windows MAX_PATH for Store Python

pip install litellm fails on Microsoft Store Python because its user
site-packages is already 134 chars plus the profile name, and the content
filter guardrail ships YAML five directories deep under
litellm/proxy/guardrails/guardrail_hooks/litellm_content_filter/. The
existing wheel guard assumed a 100-char install prefix, so it never saw it.

Move categories/ and policy_templates/ to
litellm/proxy/guardrails/content_filter_data/ and drop the benchmark
fixtures from the wheel. Old category_file paths keep resolving because
the resolver only keys on the trailing categories/<file> or
policy_templates/<file> suffix.

Derive the guard's worst-case prefix from the Store Python site-packages
path with a 15-char profile name \(149\), fail files at 260 and directories
at 248 \(CreateDirectoryW\), and fix the off-by-one that let a 260-char
path through.

Fixes #43851

* ci: run the Windows wheel install guard on pull requests

The two Windows jobs live in CircleCI, which never runs on pull requests,
so nothing installs the wheel on Windows before merge. Add a GitHub Actions
job on windows-latest that builds the wheel and runs the guard.

Two things make the run deterministic instead of image dependent. The job
turns the LongPathsEnabled registry key off first, because runner images
ship with it on and python.exe is long-path aware, so a 300-char path would
install fine. The guard installs with pip instead of uv, because uv writes
files from Rust, which switches to extended-length paths on its own and can
never hit MAX_PATH.

* fix\(guardrails\): keep the old content filter package dir as a category search root

Deployments that copied their own category YAML into
guardrail_hooks/litellm_content_filter/ before the data move would have had
that file rejected by the new directory jail and missing from by-name loads,
inherit_from lookups, the UI category listing and the category YAML endpoint.
Every lookup now searches the bundled data dir first and the old package dir
second, with the bundled copy winning on a name clash.

* fix\(guardrails\): resolve category files through safe_join

By-name category lookups and the suffix search in the category_file resolver now go through safe_join, so a name or suffix that would escape its data root never reaches the filesystem. The LITELLM_CONTENT_FILTER_ALLOW_EXTERNAL_PATHS opt-out keeps its unjailed search. Clears the two CodeQL path-injection findings on the new lookup code.

* fix\(guardrails\): keep symlinked category files loadable by name

By-name category lookups resolved symlinks through safe_join, so a category file symlinked into the categories folder from elsewhere stopped loading. Those lookups now only reject names that leave the folder lexically and return the link untouched, matching how by-name loads behaved before the data move. The category_file resolver keeps its realpath jail as before.

* fix\(guardrails\): keep the category viewer inside the category folders

GET /guardrails/ui/category_yaml/{name} hands raw file contents to any valid key, and on main it refused a symlink whose target left the categories folder. The previous commit let by-name lookups follow symlinks again, which also let the viewer read whatever a symlink in a legacy categories folder pointed at. The viewer now checks the found file's real path against every categories folder it searches and answers 400 as before, while the guardrail's own by-name loads keep following symlinks

The roots come in through a FastAPI dependency so the check is testable against a temp folder, and the content filter's realpath containment moves to path_utils.is_within so both surfaces share it. The test that patched os.path.commonpath covered a branch that no longer exists and goes with it

* ci: drop the Windows wheel install job from pull requests

The job took about 13 minutes on every PR to guard an edge case. The
guard still runs its path-length check on Linux in base_sdk_install and
on Windows in the CircleCI windows_release_wheel job.")  | Sep 30, 2026  |  
|   |   | [build(docker-compose.yml): add prometheus scraper to docker compose](https://github.com/BerriAI/litellm/commit/d9539e518e2d4d82ea2b6ac737de19147790e5ea "build\(docker-compose.yml\): add prometheus scraper to docker compose

persists prometheus data across restarts")  | Jul 24, 2024  |  
| [provider_endpoints_support.json](https://github.com/BerriAI/litellm/blob/main/provider_endpoints_support.json "provider_endpoints_support.json")  | [provider_endpoints_support.json](https://github.com/BerriAI/litellm/blob/main/provider_endpoints_support.json "provider_endpoints_support.json")  | [feat(microsoft_365_copilot): add Microsoft 365 Copilot chat provider …](https://github.com/BerriAI/litellm/commit/fbf4af301cfdc6bf0d861eb243682e3e9f1164c4 "feat\(microsoft_365_copilot\): add Microsoft 365 Copilot chat provider with OAuth token exchange \(#45158\)

* feat\(microsoft_365_copilot\): add Microsoft 365 Copilot chat provider with OAuth token exchange

* fix\(ui\): create a credential for credential-only auth types in Add Model

* fix\(microsoft_365_copilot\): accept max_tokens and list the chat model

* fix\(microsoft_365_copilot\): register provider model set

* fix\(ui\): hide other auth types' fields when credential-only types are filtered

* fix\(health\): do not inherit stored auth settings when a connection test brings its own api_key

* docs\(health\): restore connection-test inheritance docstrings

* fix\(lint\): suppress justified provider boundary casts

* fix\(lint\): satisfy M365 type-discipline gate

* fix\(types\): eliminate type-check gate regressions

* style: shorten suppression reasons so ruff format leaves them on one line

* refactor\(types\): drop shared-file type widening unrelated to the Copilot provider

* fix\(health\): pass caller headers as secret_fields to connection-test probes

* fix\(types\): type connection-test probe and token counter locally

* fix\(lint\): drop cast import from connection-test header getter

* chore: remove explanatory comments flagged by review

* fix\(proxy\): gate OAuth client credentials behind proxy admin

* fix\(router\): skip cooldown on caller-scoped OAuth auth failures

* fix\(router\): skip fallback cooldown on caller-scoped OAuth auth failures

* fix\(router\): type caller-scoped cooldown lookups

* fix\(ci\): include Microsoft 365 Copilot tests in unit shard

* fix\(proxy\): keep saved endpoint when test_connection retests a deployment by id

* fix\(m365\): collapse doubled Graph replies

* fix\(m365\): preserve trailing system messages

---------

Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Oct 9, 2026  |  
| [proxy_server_config.yaml](https://github.com/BerriAI/litellm/blob/main/proxy_server_config.yaml "proxy_server_config.yaml")  | [proxy_server_config.yaml](https://github.com/BerriAI/litellm/blob/main/proxy_server_config.yaml "proxy_server_config.yaml")  |  [test(ci): refresh qualified retired OpenAI fixtures (](https://github.com/BerriAI/litellm/commit/f8f05767daf5e5d9a1bb4bae2acf0d1806c59e1f "test\(ci\): refresh qualified retired OpenAI fixtures \(#43938\)

* test\(ci\): refresh qualified retired OpenAI fixtures

* test\(ci\): compare fallback input usage instead of provider wording")[#43938](https://github.com/BerriAI/litellm/pull/43938)[)](https://github.com/BerriAI/litellm/commit/f8f05767daf5e5d9a1bb4bae2acf0d1806c59e1f "test\(ci\): refresh qualified retired OpenAI fixtures \(#43938\)

* test\(ci\): refresh qualified retired OpenAI fixtures

* test\(ci\): compare fallback input usage instead of provider wording")  | Sep 30, 2026  |  
|   |   | [chore: bump litellm-enterprise 0.1.75 -> 0.1.76, litellm-proxy-extras…](https://github.com/BerriAI/litellm/commit/cdba36a1f0c9475ca64bb1e357ebfa152c687be3 "chore: bump litellm-enterprise 0.1.75 -> 0.1.76, litellm-proxy-extras 0.4.107 -> 0.4.108 \(#45534\)

Co-authored-by: yuneng <yuneng@berri.ai>")  | Oct 9, 2026  |  
|   |   |  [test(e2e): move the harness self-tests out of tests/e2e (](https://github.com/BerriAI/litellm/commit/048a1500dfb83cdcb0d811be6b00d602fca43759 "test\(e2e\): move the harness self-tests out of tests/e2e \(#45172\)

* test\(e2e\): move the harness self-tests out of tests/e2e

The nightly Buildkite run copies tests/e2e into the runner image and runs
bare pytest, so the 672 tests of the harness itself \(fixture parsing, JUnit
properties, the stack lock, the load aggregators, the Claude Code driver\)
counted as e2e tests on the status page even though none of them reaches a
proxy. They now live in tests/e2e_harness, mirroring the tests/e2e layout,
and run in the GitHub Actions lint job and the CircleCI
provider_replay_harness job instead

* fix\(ci\): point the providers replay controls at tests/e2e_harness

The providers integration job still selected the four replay-control
tests under tests/e2e/test_provider_edge.py, so pytest exited before
they ran. The raw-HTTP check's file walk also drops to one loop per
comprehension

* style\(tests\): mark the raw-HTTP check's bindings Final")[#45172](https://github.com/BerriAI/litellm/pull/45172)[)](https://github.com/BerriAI/litellm/commit/048a1500dfb83cdcb0d811be6b00d602fca43759 "test\(e2e\): move the harness self-tests out of tests/e2e \(#45172\)

* test\(e2e\): move the harness self-tests out of tests/e2e

The nightly Buildkite run copies tests/e2e into the runner image and runs
bare pytest, so the 672 tests of the harness itself \(fixture parsing, JUnit
properties, the stack lock, the load aggregators, the Claude Code driver\)
counted as e2e tests on the status page even though none of them reaches a
proxy. They now live in tests/e2e_harness, mirroring the tests/e2e layout,
and run in the GitHub Actions lint job and the CircleCI
provider_replay_harness job instead

* fix\(ci\): point the providers replay controls at tests/e2e_harness

The providers integration job still selected the four replay-control
tests under tests/e2e/test_provider_edge.py, so pytest exited before
they ran. The raw-HTTP check's file walk also drops to one loop per
comprehension

* style\(tests\): mark the raw-HTTP check's bindings Final")  | Oct 7, 2026  |  
|   |   |  [fix(security): remove the publicly known master key from the repo (](https://github.com/BerriAI/litellm/commit/837c6a7481de91f2fcc2dfda929f7f96796e9a0a "fix\(security\): remove the publicly known master key from the repo \(#44718\)

* fix\(security\): hash the publicly known master key

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* docs: replace weak master key examples and regenerate artifacts

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* test: replace weak key fixtures with generated test keys

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* ci: generate master keys for proxy startup

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix: preserve lens dev key entropy

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix: restore proxy key compatibility in scrub examples

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix: scrub merged SSO fixture and refresh dashboard bundle

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix\(ci\): stabilize test keys and metadata collection

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* chore: drop the rebuilt dashboard bundle

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

---------

Co-authored-by: mateo <mateo@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")[#4…](https://github.com/BerriAI/litellm/pull/44718)  | Oct 6, 2026  |  
|   |   | [feat(proxy)!: refuse to start with an unset, empty, or publicly known…](https://github.com/BerriAI/litellm/commit/fe480533e862bd25569b36d61411a2774fe0fce6 "feat\(proxy\)!: refuse to start with an unset, empty, or publicly known master key

The proxy used to boot with no master key \(every request accepted without
authentication\) and with sk-1234, the key every example used. It now stops at
startup, before it connects to the database, and prints how to fix it: where the
bad key came from, a copy-pastable command that generates a secure key, and,
when the public key is also encrypting a database, a link to the rotation guide

general_settings.dangerously_allow_unsafe_proxy: true or
LITELLM_DANGEROUSLY_ALLOW_UNSAFE_PROXY=true starts the proxy anyway, for local
development. CI and test boots that rely on sk-1234 or on no key set it

BREAKING CHANGE: deployments with no master key, an empty one, or sk-1234 no
longer start until they set a real key or opt in to the override")  | Sep 19, 2026  |  
|   |   |  [feat(router): add router plugin reference catalog (](https://github.com/BerriAI/litellm/commit/f9a217e45b3bf1c7936180db85465ad6fb02a98a "feat\(router\): add router plugin reference catalog \(#33746\)")[#33746](https://github.com/BerriAI/litellm/pull/33746)[)](https://github.com/BerriAI/litellm/commit/f9a217e45b3bf1c7936180db85465ad6fb02a98a "feat\(router\): add router plugin reference catalog \(#33746\)")  | Jul 17, 2026  |  
|   |   | [feat(guardrails): add llm shield pii redaction and rehydration guardr…](https://github.com/BerriAI/litellm/commit/d94670674434cc9f9ac50b974ea2b1e3bde9be32 "feat\(guardrails\): add llm shield pii redaction and rehydration guardrail \(#42645\)

* feat\(guardrails\): add llm shield pii redaction and rehydration guardrail

LLM Shield is a self-hosted PII gateway. This adds it as a guardrail so a
proxy operator can redact personal data out of outbound requests and have
the original values restored in the model's reply.

The substitution is reversible, which is the difference from a masking
guardrail. Outbound text is replaced with placeholders held in a session
vault inside the operator's own LLM Shield deployment, and the reply is
restored before it reaches the caller, so the end user still sees real
values while the provider never received them.

Streaming responses are restored incrementally. LLM Shield holds back only
the trailing characters that could still turn out to be part of a
placeholder, so tokens are forwarded as they arrive rather than the whole
response being collected first. A placeholder split across two chunks is
never emitted in fragments.

The integration talks to LLM Shield over HTTP and adds no dependency.

Notes for reviewers:

- The guardrail sets use_native_lifecycle_hooks, since redaction and
  restoration need the native pre-call, post-call and streaming hooks
  rather than the unified path.
- Per-request state lives on the request dict, never on the guardrail
  instance, because the proxy registers a single instance process-wide.
  The streaming carry-over is a local of the generator for the same reason.
- Every failure blocks the request. A redaction guardrail that fails open
  would send the exact data it exists to protect to the provider.

* feat\(ui\): list llm shield in the guardrail garden

Adds the card, preset and logo so operators can pick LLM Shield from the
guardrails page the same way as the other partner guardrails.

* docs\(guardrails\): add llm shield example config

Shows both modes on one entry. Listing only pre_call redacts the request
and then hands the placeholders back to the end user, so the test asserts
both hooks are enabled.

* feat\(ui\): use the llm shield brand mark for the guardrail logo

* fix\(guardrails\): restore llm shield values in anthropic replies

The /v1/messages reply is a plain dict with a content block list and no
choices, so it fell through the restore path and went back to the caller
still carrying placeholders. The request was redacted correctly, which is
what made this easy to miss.

Found by running all three endpoints against a live provider; the mocked
tests all passed because they only built the OpenAI shape. Adds tests for
the message shape and for leaving non-text blocks alone.

* docs\(guardrails\): correct the llm shield start command

* fix\(guardrails\): redact every request shape and restore every reply shape

Three gaps, all of which let an enabled guardrail hand data to the provider
or hand placeholders to the caller.

Requests only walked `messages`. The Responses API `input` and tool call
`arguments` went out untouched. Measured against a live provider: a request
sent through `/v1/responses` reached the model with the real address in it
while the guardrail reported as enabled. Request traversal now covers chat
content \(string and multimodal\), tool call arguments, and `input` as a bare
string or a list of items.

Fixing that exposed the matching gap on the way back: the Responses API reply
carries `output` items rather than `choices`, so it returned to the caller
still holding placeholders. It now gets its own walk, handling text blocks as
dicts or objects.

The dashboard preset seeded only pre_call, so a guardrail created from the UI
would redact the request and return the placeholders to the user. Presets can
now seed both modes; the form already normalised either shape.

Adds tests for each request shape, for both Responses API reply forms, and
replaces a test that had asserted the `input` bypass as correct behaviour.

* fix\(guardrails\): narrow the stream delta before writing to it

basedpyright could not prove the delta was non-None on the write path, and
reportOptionalMemberAccess has a zero budget. The guard is also clearer than
relying on the text check to imply it.

* fix\(guardrails\): mint the vault id instead of trusting the caller's

The vault id was taken from caller-supplied session metadata, and every
caller shares one LLM Shield key. Someone who knew or guessed another
caller's session id could send a placeholder, have the model echo it back,
and get that caller's plaintext restored into their own reply.

Vault ids are now minted per request behind a per-process prefix, so a
caller cannot name a vault this process uses. Redaction mints, restoration
reads back, and a reply whose id does not match is left holding its
placeholders rather than resolved against some other vault.

Also covers two more request fields that were reaching the provider intact:
the Responses API `instructions`, and the legacy `function_call.arguments`
alongside `tool_calls`.

The collectors move to module level, which drops the traversal back under
the complexity limit and lets the code carry its own explanation instead of
the comments that were restating it.

* fix\(guardrails\): drop Final from a loop-assigned local

basedpyright rejects a Final assigned inside a loop, and
reportGeneralTypeIssues sits one over its budget ceiling.

* fix\(guardrails\): redact completion prompts and responses tool items

Two more provider-bound request shapes were reaching the model intact while
the guardrail reported as enabled.

/v1/completions carries its text in a top-level `prompt`, which the
traversal never looked at. It is handled as a string and as the array form,
where each entry is rewritten in place.

Responses input items hold tool data outside `content`: a function_call item
in `arguments`, a function_call_output item in `output`. Both are now
collected alongside the item's content.

Adds a test per shape.

* fix\(guardrails\): redact the anthropic system prompt and string-array input

Two more provider-bound shapes, found by walking the request types rather
than waiting for them to be reported.

/v1/messages carries its system prompt at the top level, as a string or a
list of text blocks. It is one of the endpoints this guardrail claims to
cover, and a system prompt is a natural place to put a customer's details.

`input` as an array of bare strings, the embeddings and moderations shape,
was skipped because the loop only handled item dicts.

Verified against a live provider: a system prompt holding an address now
reaches the model as a stand-in and is restored in the reply.

* fix\(guardrails\): narrow prompt and input to a list before iterating

Guarding with a conditional iterable left the value un-narrowed, so passing
it on was an argument-type error and the element checks read as unreachable.
An early return narrows it properly and reads better.

* fix\(guardrails\): restore every streaming choice, not just the first

Streaming rehydration read and rewrote choices\[0\] only, so with n>1 every
later choice went back to the caller still holding its placeholders.

Each choice is its own token stream, so the sliding window is now tracked
per choice index rather than once per stream. A single shared window would
have been worse than the bug: it would splice the characters held back for
one choice onto the next one's delta.

The final flush walks every choice the same way, and the two helpers that
only ever looked at choices\[0\] are gone.

Adds a test that both choices come back restored, and one that each choice
gets its own window handed back rather than its neighbour's.

* refactor\(guardrails\): name the guardrail llm_shield_proxy throughout

The integration was called llm_shield in code, llm-shield in the example
config, and LLM Shield in the dashboard, while the product and its PyPI
package are both llm-shield-proxy. An operator who saw the guardrail in
LiteLLM could not tell what to install.

One identifier now: llm_shield_proxy for the enum value, module, directory,
class, config model, logo and environment variables, with LLM Shield Proxy
as the display name. That matches `pip install llm-shield-proxy`.

Renames only; no behaviour change.

* feat\(guardrails\): redact the participant name on a message

`name` on a user or assistant turn identifies a person and was going to the
provider intact. The proxy this integrates with already redacts it, so the
integration was the weaker of the two.

On a tool or function turn the same field carries the function's name, which
has to arrive unchanged or the call stops routing. That case is skipped, and
a test asserts the value is never even sent to the shield.

* fix\(guardrails\): flush every held choice, and cover tool results and suffix

Three review findings.

The trailing flush walked the last chunk's choices, so a choice that finished
earlier and stopped appearing lost whatever text was still held for it and its
answer was truncated. It is now driven by the windows themselves and emits one
chunk per choice, synthesising the choice when the terminal chunk omits it.
That was data loss, not just under-redaction.

An Anthropic tool_result carries its own content, as a string or as further
blocks, and only each part's `text` was being collected. Handled recursively;
image and audio parts still fall through untouched.

The legacy completions `suffix` is forwarded to providers that support it and
was never collected. Note the placement: it has to be gathered before the
string-prompt early return, which is what the new test pins.

* fix\(guardrails\): walk nested tool results iteratively, with a depth bound

CI flagged _collect_content as recursive. It was, and worse, it was unbounded:
a tool_result nests its own content, the nesting is caller controlled, and the
descent had nothing to stop it. That is a JSON bomb, not a style issue.

Now an explicit queue with a depth bound of 8. Real payloads nest one or two
deep. The queue is walked in document order because the shield maps its replies
back by position, so collection order is part of the contract.

* fix\(guardrails\): redact Responses PromptObject variables

A Responses request can send `prompt` as a PromptObject rather than a string.
Its `variables` are substituted into the stored prompt on the provider side, so
they are caller text, and the dict shape was falling through untouched.

`id` and `version` pick which stored prompt to run and are left unchanged.

* test\(guardrails\): assert the depth bound instead of only reaching the end

The depth test asserted nothing, so it passed whether or not the bound held,
and the test-quality gate counted it as a zero-assert test. It now sends a
shallow value alongside a 200-deep chain and asserts the shallow one is
collected while the value past the bound is not.

* fix\(guardrails\): keep system-prompt values out of the restored reply

Redaction put every span of a request into one vault, and the reply was restored
against that same vault. System prompts are written by the application and the
caller never sees them, so a caller who got the model to echo a placeholder back
had its plaintext restored into their own reply -- a way to read a system prompt
they were never shown.

Server-authored spans now go into a vault of their own: system and developer
turns, Anthropic's top-level `system`, and the Responses API `instructions`.
Its id is deliberately never stored, so nothing restores against it. The reply
is restored against the caller's vault alone, and an echoed placeholder from a
system prompt comes back as the placeholder.

Values the caller also wrote themselves are unaffected -- they are in the
caller's vault too, and still restore. The extra round trip happens only when a
request actually carries server-authored text.

* style\(guardrails\): satisfy ruff format and annotate the new tests

`ruff format` wanted the widened `_collect_responses_fields` signature on one
line, and the three tests added with the split-vault fix needed return
annotations to keep ANN201 level with the base.

* fix\(guardrails\): restore tool calls in the LLM Shield guardrail

The request walk redacted a tool call's `arguments` -- plus the legacy `function_call`,
Anthropic `tool_use.input` leaves and the Responses API's `function_call` /
`function_call_output` fields -- while the response walk restored only `message.content`.
A placeholder therefore reached the caller inside a tool call, and nothing raised.

This is the same change as the out-of-tree example adapter this file is copied from, kept
body-identical on purpose: the response side now collects every restorable span in one
positional rehydrate batch, streaming keeps a window per \(choice index, tool-call index\)
and flushes each into the chunk carrying the finish_reason, and `apply_guardrail` restores
`inputs\["tool_calls"\]` on the response side. The declared limit on restoring values inside
a JSON string is documented in the module.

* fix\(guardrails\): import copy, keep the vault id off the provider, drop recursion

Three defects Greptile and veria-ai found on the reopened PR, all real:

- `copy.deepcopy` was called in `apply_guardrail` with no `import copy`, a
  guaranteed NameError on every response carrying tool calls. It landed on
  2026-09-13, ten days after the review that rated this branch safe, and no test
  reached it: every tool-call test covered the request side. Adds the import and
  a regression test on the response side.
- The vault session id was stored in `metadata`, which is forwarded to the
  provider on /v1/responses. A provider holding the placeholders and the session
  id can call the shield's rehydrate endpoint and read back the plaintext this
  guardrail exists to withhold. Moves it to `litellm_metadata`, which is not
  forwarded, and reads it back from there only.
- `_collect_json_leaves` recursed over model-controlled JSON; the repo's
  recursive_detector gate rejects that. Rewritten with an explicit stack, same
  depth bound.

52 tests pass. ruff format, ruff-strict and check_type_discipline all clean, with
LIT counts identical to the merge base.

* fix\(guardrails\): build llm_shield_proxy stream deltas without new mutable literals

The lint job's LIT002 budget gate failed on this PR: the file added 11
mutable-collection constructions and the tree sits at its limit. Build the
index-only tool-call continuation in one helper, keep read-only inputs as
tuples, and annotate the lists the delta and texts fields require.

Adds tests for the two tool-call flush paths the refactor touches, which
had no coverage: held arguments landing in the finish_reason chunk next to
that chunk's own fragment, and the trailing flush of a stream that ends
without a finish_reason.

* fix\(guardrails\): drop Final from loop-body locals in llm_shield_proxy

basedpyright rejects Final on a name assigned inside a loop, and the eleven
such locals put reportGeneralTypeIssues over its budget \(112/101\). The LIT010
Final rule already exempts loop-body assignments, so the annotations go.

* feat\(guardrails\): restore llm_shield_proxy placeholders on native streams

Anthropic /v1/messages and /v1/responses streams have no `choices`, so the
streaming hook passed them through with placeholders still in them. Both
are now restored incrementally, with the same per-stream windows as chat:

- /v1/messages arrives as raw SSE. Frames are cut at event boundaries,
  text_delta and input_json_delta are restored per block index, and held
  text is emitted as one more delta ahead of content_block_stop. Signed
  thinking deltas, frames from other endpoints and non-SSE raw streams
  pass through unchanged.
- /v1/responses events are restored per item and part. Held text goes out
  as a copy of the stream's last delta before its .done event, and the
  events that repeat the reply \(.done, content_part.done, output_item.done,
  response.completed\) are restored in full.

The request side now also redacts Anthropic tool_use inputs and Responses
reasoning summaries, and sends tool and function descriptions \(including
parameter schema descriptions\) and the user / safety_identifier fields to
the non-restorable vault, like system prompts. Tool results stay
restorable: the model reads them to answer, so restoring them returns what
the caller would have seen without the guardrail.

* fix\(guardrails\): redact llm_shield_proxy predicted outputs and output schemas

`prediction.content` is the caller's own draft of the reply, so it is
redacted into the caller vault and restored with the reply. The
descriptions in a structured-output schema \(Chat
response_format.json_schema, Responses text.format\) are application
authored like tool schemas, so they go to the non-restorable vault.

* fix\(guardrails\): fail closed on deep llm_shield_proxy requests, widen coverage

- Request walks no longer skip what lies past their depth bound. Content
  nested past it, and tool inputs or schemas past the new JSON bound, now
  block the request instead of reaching the provider unredacted. The old
  depth test asserted the skip; it now asserts the block.
- Tool and output schemas are walked by their JSON Schema structure, and
  give up `title`, `examples` and `default` as well as `description`.
  `enum` and `const` still go out as sent.
- Responses events are matched by shape: any `*.delta` with a string delta
  is a token stream, and any `*.done` restores every non-identifier text
  field plus the `part` or `item` it repeats. This covers
  reasoning_summary_part.done and MCP arguments, and future families.
  Audio deltas are left alone.
- An SSE stream whose first chunk ends partway through a field name
  \(`b"eve"`\) is no longer taken for a non-SSE stream.

* fix\(guardrails\): scan llm_shield_proxy schemas by default

The schema walk collected an allowlist of keywords, so any keyword it did
not list -- draft-07 `dependencies`, `$comment`, vendor `x-` extensions --
went to the provider in clear. Invert it: every string is collected except
under keywords whose value must go out verbatim \(types, formats, patterns,
references, required lists, enum, const\). Name -> subschema maps still
treat their keys as property names, so a property called `type` is
walked, not skipped.

* fix\(guardrails\): redact llm_shield_proxy schema enum and const values

`enum` and `const` were skipped by the schema walk, so a value holding PII
went to the provider in clear. They now go to the caller's vault rather
than the non-restorable one: the model emits the stand-in in its tool
arguments or structured output, and restoring the reply turns it back into
the value the schema allows, so the call still routes.

* fix\(guardrails\): redact llm_shield_proxy web search user locations

Web search forwards the user's approximate location, and its free-text
`city` and `region` fields can hold an address. Collect them into the
non-restorable vault, from Chat `web_search_options.user_location` and
from the `user_location` of Responses and Anthropic web-search tools.

* fix\(guardrails\): drop unused llm_shield_proxy suppressions

Upstream added LIT013 \(a *-ok marker that suppresses nothing\) and LIT014
\(at most one for and one if per comprehension\). Remove the 34 markers
that no longer suppress anything and flatten the finished streams with
itertools.chain.from_iterable.

* fix\(guardrails\): type the llm_shield_proxy request and reply walks

Narrowing with isinstance\(x, dict\) leaves keys and values unknown, so
every call that passed a narrowed value counted against the
reportUnknownArgumentType budget. Parse into dict\[str, object\] and
list\[object\] once, in _as_object and _as_array, type the carry keys and
accumulators, and bind writers with functools.partial instead of lambdas.
The shield's batch reply is now also checked to hold only strings.

* fix\(guardrails\): keep restored llm_shield_proxy replies out of the cache, widen coverage

Addresses the open veria-ai and Cursor Bugbot findings on #42645.

- Restore a copy of the reply and of each stream chunk, never LiteLLM's own object.
  LiteLLM caches and logs that object, and placeholders are numbered per request, so
  two callers' redacted requests can share a cache key: restoring in place cached one
  caller's plaintext for the next. The deployment hook no longer restores either,
  since LiteLLM caches what it returns; the proxy's post-call hook restores
  model-level guardrails after the cache write.
- Restore /v1/completions replies, streamed and not, which carry `choice.text`.
- Redact Responses replay fields the reply side already restores: tool output sent
  as input_text parts, custom_tool_call `input`, code_interpreter_call `code`.
- Redact typed Responses prompt variables \(`{"type": "input_text", "text": ...}`\).
- Put Responses system and developer input items in the non-restorable vault, like
  their Chat counterparts.
- Expose LLMShieldProxyGuardrailConfigModel through get_config_model, so the
  dashboard can collect the Shield URL and key.

* fix\(guardrails\): redact llm_shield_proxy plain-text document blocks

An Anthropic document block carries text inline, in a text source's `data` or a
content source's `content`, and that text reached the provider unredacted. Collect
both, plus the block's `title` and `context`; base64, URL and file sources pass
untouched.

* fix\(guardrails\): redact llm_shield_proxy extra_body overrides

LiteLLM merges extra_body over the transformed request just before sending, so text
placed there \(input, messages, system, ...\) replaced the redacted field on the wire.
Walk extra_body with the same collectors as the request, keeping the caller /
application split.

* test\(guardrails\): import InMemoryCache directly in the llm_shield_proxy cache test

litellm keeps a deprecated module-level `caching` bool, so `litellm.caching.caching`
resolves to that bool once an earlier test in the same worker has set it, and the test
failed with AttributeError depending on test order.

* fix\(guardrails\): restore llm_shield_proxy replies for model-level use outside the proxy

71e68fd stopped the deployment post-call hook from restoring, so the response cache
never holds restored plaintext. Inside the proxy that is right: the proxy's post-call
hook restores after the cache write. But with model-level `guardrails` on the SDK,
the deployment hooks are the only redact and restore steps, so callers got
placeholders back.

When the deployment pre-call hook is the one that redacts, it now records the
request's vault id and marks the request no-cache / no-store; the deployment
post-call hook restores only when that record matches. The cache key there is built
from the redacted request and a cache hit skips the post-call hook, so a cached reply
could neither be restored nor safely shared. Proxy requests carry no record and keep
restoring in the proxy's post-call hook, after the cache write.

* fix\(guardrails\): don't repeat usage in llm_shield_proxy end-of-stream flush chunks

With n>=2 and stream_options.include_usage, the end-of-stream flush copies the last
chunk the stream carried, which is the one holding usage, so each synthetic flush
chunk repeated it and a consumer summing usage chunks counted the request twice.
The copy now drops `usage`, matching a normal mid-stream chunk. Reported by
@yucheng-berri.

* fix\(guardrails\): keep restored llm_shield_proxy values out of telemetry, refuse SDK streams

- The post-call restore hook no longer goes through log_guardrail_information,
  which recorded its whole return value, the restored reply, as guardrail_response.
  That field is exported to traces even with message logging turned off.
- A model-level stream outside the proxy is refused once redacted. Nothing restores
  an SDK stream, and its cache writer reads the request from before the deployment
  hook, so it also got cached despite the no-store bypass.
- Drop a narrating comment, and keep example_config.yaml to config only; the
  how-to lives in the docs PR.

* refactor\(guardrails\): split llm_shield_proxy into payload, request walk and stream modules

The module had grown past 1,700 lines. Shared payload types and helpers move to
payload.py, the request walk to request_walk.py and the stream restorers to
stream_restorers.py; llm_shield_proxy.py keeps the guardrail class. No behaviour
change.

* style\(guardrails\): drop routine comments from llm_shield_proxy

AGENTS.md keeps source comments to tool directives and genuinely complex logic;
the rationale stays in the docstrings.")  | Oct 5, 2026  |  
|   |   | [test: gate the test tree on fifteen assertion and handler rules it al…](https://github.com/BerriAI/litellm/commit/7e7ac69258b1780d3bd0db9efc3721a4048d32cd "test: gate the test tree on fifteen assertion and handler rules it already satisfies \(#38361\)")  | Aug 26, 2026  |  
|   |   | [ci(lint): gate every rule on its merge-base count and cap Anys at a f…](https://github.com/BerriAI/litellm/commit/d6e453f74b45ab10ee30276091eb40773f3322af "ci\(lint\): gate every rule on its merge-base count and cap Anys at a fixed total \(#40193\)

* ci\(lint\): derive gate ceilings from merge-base counts and drop the budget files

The four lint gates \(ruff strict, type discipline, basedpyright, test quality\)
now fail a branch only when a rule's codebase count grows past its count at the
merge-base with litellm_internal_staging plus a fixed per-rule headroom, which
is zero everywhere except the LIT010/LIT011 and reportAny/reportExplicitAny
seeds. Base counts come from a disk cache, then the CI artifact the renamed
publish-lint-base-counts workflow uploads for every staging push \(all four
checkers, one artifact per checker and sha\), then a scan of the base worktree.

The four *-budget.json files, make lint-budget-update, budget_ratchet_check.py,
and the unratcheted check are gone, so no PR carries a budget edit again.

* test\(lint-gates\): assert checker identities by behavior and type the new gate tests

* test\(lint-gates\): resolve base points over an injected git so unit tests stay in-process

---------

Co-authored-by: mateo-berri <277851410+mateo-berri@users.noreply.github.com>")  | Oct 8, 2026  |  
|   |   | [fix(ci): pin workflow toolchain dependencies](https://github.com/BerriAI/litellm/commit/38150dfc2c50526664125c25c6eabf07d06e07ed "fix\(ci\): pin workflow toolchain dependencies")  | Sep 2, 2026  |  
|   |   | [feat(github_copilot): per-user GitHub OAuth connections for Copilot c…](https://github.com/BerriAI/litellm/commit/d36b66e19ebd2cfc2728ac6e50cacee3e902fbd0 "feat\(github_copilot\): per-user GitHub OAuth connections for Copilot credentials \(#45241\)

* feat\(github_copilot\): per-user GitHub OAuth connections for Copilot credentials

Related to LIT-9306

* fix\(ui\): make per-user GitHub Copilot OAuth credential-only in Add Model

* fix\(proxy\): render LiteLLM_UserProviderCredentials prisma spans

* style\(ui\): prettier-format add_model tests

* fix\(lint\): annotate connection endpoints and narrow per-user credential excepts

* fix\(lint\): satisfy type-discipline gate for per-user copilot code

* fix\(types\): clear basedpyright and strict-lint gate regressions

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* fix\(types\): keep runtime guards on untyped responses input

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* style: ruff format per-user Copilot files

* fix\(lint\): keep type-discipline suppressions on flagged lines

* fix\(github_copilot\): resolve team and deployment-id per-user routes, share token cache invalidation

* test\(github_copilot\): scope response-cache mutation via monkeypatch, assert route allowlist

* fix\(github_copilot\): fall back to the database on Redis errors and skip worker-local token caching

* fix\(lint\): suppress mutable return annotation after base merge

* style: shorten rebind-ok reason so the dispatch line stays formatted

* fix\(github_copilot\): harden per-user token cache, overrides, fallbacks, headers and device flow

* fix\(lint\): suppress MutableMapping param on session auth pinning

* test\(github_copilot\): narrow pytest.raises to HTTPException for service-key override

* fix\(github_copilot\): fail closed on revoke errors, strip caller auth headers, widen fallback discovery

* fix\(github_copilot\): verify revoke tombstone, bound fallback discovery, cover aliased fallbacks

* fix\(github_copilot\): make fallback discovery a router superset and guard credential override at deployment

* style\(router\): drop explanatory comment on per-user credential guard

* fix\(github_copilot\): gate fallback discovery on per-user credentials and bound aggregate targets

* fix\(github_copilot\): count every fallback mapping toward the discovery limit

* fix\(github_copilot\): suppress banned typing cast imports with per-line reasons

* test\(github_copilot\): cover per-user 401 cooldown skip through the router failure callback

* test\(github_copilot\): cover per-user 401 cooldown skip through router and fallback paths

* fix\(github_copilot\): cover auto-router discovery, per-user fallback cooldown and connect cache race

* fix\(github_copilot\): cover every strategy router kind and verify the connect cache write

* fix merge conflict marker remnant in credential endpoint tests

* fix\(github_copilot\): do not purge user connections on label-only credential updates

* fix\(github_copilot\): read user provider credentials from the writer engine

* test\(github_copilot\): move per-user auth type form test to the integration tier

* test\(github_copilot\): annotate new credential regression test locals as Final

* fix\(github_copilot\): type per-user OAuth JSON and DB boundaries

* fix\(github_copilot\): validate per-user OAuth boundaries and restore truncated comments

* style\(github_copilot\): complete cut-off suppression reasons on new casts

* refactor\(github_copilot\): annotate new boundary-typing locals as Final

---------

Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   | [docs(security): point readers to the security announcements mailing l…](https://github.com/BerriAI/litellm/commit/684a1edd44efa3a7c7f0395ccfa1bf9803017ea2 "docs\(security\): point readers to the security announcements mailing list signup \(#43713\)

* docs\(security\): point readers to the security announcements mailing list signup

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* docs\(security\): formalize the security announcements wording

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

* docs\(security\): tighten the best-effort sentence

Co-Authored-By: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>

---------

Co-authored-by: oliver <oliver@berri.ai>
Co-authored-by: Devin AI <158243242+devin-ai-integration\[bot\]@users.noreply.github.com>")  | Sep 29, 2026  |  
|   |   |  [fix(agentcore): simplify agentcore streaming (](https://github.com/BerriAI/litellm/commit/5db0e3289ad0f69e3dd327c0ba4b1b1dc8fa27f8 "fix\(agentcore\): simplify agentcore streaming \(#17141\)

* fix\(agentcore\): simplify agentcore streaming

* fix\(agentcore\): move CustomStreamWrapper import to module level

The deferred imports inside streaming methods caused initialization delays
during health check requests, leading to timeouts in ECS deployments.

- Move CustomStreamWrapper import to module-level \(line 19\)
- Remove deferred imports from get_sync_custom_stream_wrapper \(line 588\)
- Remove deferred import from get_async_custom_stream_wrapper \(line 747\)
- Remove from TYPE_CHECKING block to use actual import

This ensures the import happens at module load time rather than during
first request processing, preventing health check endpoint blocking.

🤖 Generated with \[Claude Code\]\(https://claude.com/claude-code\)

Co-Authored-By: Claude <noreply@anthropic.com>

* test\(agentcore\): ensure sync response

* chore: upgrade boto3 to 1.40.76 in pyproject.toml

* chore: added taplo.toml

* fix\(types\): correct annotation type hint for MyPy compatibility

Update _convert_annotations_to_chat_format return type from
Dict\[str, Any\] to ChatCompletionAnnotation TypedDict to match
the Message class's expected type signature.

Co-Authored-By: Claude <noreply@anthropic.com>

---------

Co-authored-by: Claude <noreply@anthropic.com>
Co-authored-by: Benedikt Óskarsson <bensi94@hotmail.com>")[#17141](https://github.com/BerriAI/litellm/pull/17141)[)](https://github.com/BerriAI/litellm/commit/5db0e3289ad0f69e3dd327c0ba4b1b1dc8fa27f8 "fix\(agentcore\): simplify agentcore streaming \(#17141\)

* fix\(agentcore\): simplify agentcore streaming

* fix\(agentcore\): move CustomStreamWrapper import to module level

The deferred imports inside streaming methods caused initialization delays
during health check requests, leading to timeouts in ECS deployments.

- Move CustomStreamWrapper import to module-level \(line 19\)
- Remove deferred imports from get_sync_custom_stream_wrapper \(line 588\)
- Remove deferred import from get_async_custom_stream_wrapper \(line 747\)
- Remove from TYPE_CHECKING block to use actual import

This ensures the import happens at module load time rather than during
first request processing, preventing health check endpoint blocking.

🤖 Generated with \[Claude Code\]\(https://claude.com/claude-code\)

Co-Authored-By: Claude <noreply@anthropic.com>

* test\(agentcore\): ensure sync response

* chore: upgrade boto3 to 1.40.76 in pyproject.toml

* chore: added taplo.toml

* fix\(types\): correct annotation type hint for MyPy compatibility

Update _convert_annotations_to_chat_format return type from
Dict\[str, Any\] to ChatCompletionAnnotation TypedDict to match
the Message class's expected type signature.

Co-Authored-By: Claude <noreply@anthropic.com>

---------

Co-authored-by: Claude <noreply@anthropic.com>
Co-authored-by: Benedikt Óskarsson <bensi94@hotmail.com>")  | Jan 19, 2026  |  
|   |   | [chore: bump litellm-enterprise 0.1.75 -> 0.1.76, litellm-proxy-extras…](https://github.com/BerriAI/litellm/commit/cdba36a1f0c9475ca64bb1e357ebfa152c687be3 "chore: bump litellm-enterprise 0.1.75 -> 0.1.76, litellm-proxy-extras 0.4.107 -> 0.4.108 \(#45534\)

Co-authored-by: yuneng <yuneng@berri.ai>")  | Oct 9, 2026  |  
| [whitelisted_bedrock_models.txt](https://github.com/BerriAI/litellm/blob/main/whitelisted_bedrock_models.txt "whitelisted_bedrock_models.txt")  | [whitelisted_bedrock_models.txt](https://github.com/BerriAI/litellm/blob/main/whitelisted_bedrock_models.txt "whitelisted_bedrock_models.txt")  | [fix(bedrock): add the bare Pegasus 1.5 row and take GPT-5.x context w…](https://github.com/BerriAI/litellm/commit/fe955f5e428044d30c9a097e25e875345995fe2d "fix\(bedrock\): add the bare Pegasus 1.5 row and take GPT-5.x context windows from the model cards \(#45621\)

* fix\(bedrock\): add the bare Pegasus 1.5 row and take GPT-5.x context windows from the model cards

Price-Sync: litellm-providers

* fix\(bedrock\): whitelist the bare Pegasus 1.5 id for the converse routing check

---------

Co-authored-by: berriai-litellm-provider-info-sync\[bot\] <328147090+berriai-litellm-provider-info-sync\[bot\]@users.noreply.github.com>
Co-authored-by: Kerry Lu <klu@berri.ai>")  | Oct 9, 2026  |  
| View all files |  
## Repository files navigation
#  🚅 LiteLLM 
LiteLLM AI Gateway 
Open Source AI Gateway for 100+ LLMs. Self-hosted. Enterprise-ready. Call any LLM in OpenAI format.
####  [LiteLLM Proxy Server (AI Gateway)](https://docs.litellm.ai/docs/simple_proxy) | [ Hosted Proxy](https://docs.litellm.ai/docs/enterprise#hosted-litellm-proxy) | [Enterprise Tier](https://litellm.ai/enterprise) | [Website](https://www.litellm.ai/ai-gateway)
## What is LiteLLM
LiteLLM is an open source AI Gateway that gives you a single, unified interface to call 100+ LLM providers — OpenAI, Anthropic, Gemini, Bedrock, Azure, and more — using the OpenAI format.
Use it as a **Python SDK** for direct library integration, or deploy the **AI Gateway (Proxy Server)** as a centralized service for your team or organization.
[**Jump to LiteLLM Proxy (LLM Gateway) Docs**](https://docs.litellm.ai/docs/simple_proxy) [**Jump to Supported LLM Providers**](https://docs.litellm.ai/docs/providers)
## Why LiteLLM
Managing LLM calls across providers gets complicated fast — different SDKs, auth patterns, request formats, and error types for every model. LiteLLM removes that friction:
  * **Unified API** — one interface for 100+ LLMs, no provider-specific SDK juggling
  * **Drop-in OpenAI compatibility** — swap providers without rewriting your code
  * **Production-ready gateway** — virtual keys, spend tracking, guardrails, load balancing, and an admin dashboard out of the box
  * **8ms P95 latency** at 1k RPS ([benchmarks](https://docs.litellm.ai/docs/benchmarks))


### OSS Adopters  
| 
## Netflix
 |  
| --- |  
## Features
**LLMs** - Call 100+ LLMs (Python SDK + AI Gateway)
[**All Supported Endpoints**](https://docs.litellm.ai/docs/supported_endpoints) - `/chat/completions`, `/responses`, `/embeddings`, `/images`, `/audio`, `/batches`, `/rerank`, `/a2a`, `/messages` and more.
### Python SDK

```
uv add litellm
```

An independent `litellm-core` distribution provides the Python SDK with the same `import litellm` API and runtime dependencies. It has no optional extras, CLI entry points, or bundled dashboard. Install one SDK distribution per environment because `litellm` and `litellm-core` own overlapping Python files. Use `litellm` for the proxy, CLI, and optional extras
Build and install core from this checkout while its release integration is pending:

```
python scripts/build_core_distribution.py --out-dir dist/core
python -m pip install dist/core/litellm_core-*.whl
```

The builder requires Git, `uv` and the Rust build toolchain. It reads the release version from the root `pyproject.toml`, leaves source files unchanged, and produces a wheel and a self-contained sdist in `dist/core`. Run installation in a fresh environment without `litellm`

```
from litellm import completion
import os

os.environ["OPENAI_API_KEY"] = "your-openai-key"
os.environ["ANTHROPIC_API_KEY"] = "your-anthropic-key"

# OpenAI
response = completion(model="openai/gpt-4o", messages=[{"role": "user", "content": "Hello!"}])

# Anthropic  
response = completion(model="anthropic/claude-sonnet-4-20250514", messages=[{"role": "user", "content": "Hello!"}])
```

### AI Gateway (Proxy Server)
[**Getting Started - E2E Tutorial**](https://docs.litellm.ai/docs/proxy/docker_quick_start) - Setup virtual keys, make your first request

```
uv tool install 'litellm[proxy]'
litellm --model gpt-4o
```


```
import openai

client = openai.OpenAI(api_key="anything", base_url="http://0.0.0.0:4000")
response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Hello!"}]
)
```

[**Docs: LLM Providers**](https://docs.litellm.ai/docs/providers)
**Agents** - Invoke A2A Agents (Python SDK + AI Gateway)
[**Supported Providers**](https://docs.litellm.ai/docs/a2a#add-a2a-agents) - LangGraph, Vertex AI Agent Engine, Azure AI Foundry, Bedrock AgentCore, Pydantic AI
### Python SDK - A2A Protocol

```
from litellm.a2a_protocol import A2AClient
from a2a.types import SendMessageRequest, MessageSendParams
from uuid import uuid4

client = A2AClient(base_url="http://localhost:10001")

request = SendMessageRequest(
    id=str(uuid4()),
    params=MessageSendParams(
        message={
            "role": "user",
            "parts": [{"kind": "text", "text": "Hello!"}],
            "messageId": uuid4().hex,
        }
    )
)
response = await client.send_message(request)
```

### AI Gateway (Proxy Server)
**Step 1.** [Add your Agent to the AI Gateway](https://docs.litellm.ai/docs/a2a#adding-your-agent) — set `protocolVersion` to `1.0` or `0.3` per agent
**Step 2.** Call Agent via A2A SDK (requires `a2a-sdk>=1.1.0`)

```
import httpx
from a2a.client import A2ACardResolver, ClientConfig, ClientFactory
from a2a.types import Message, Part, Role, SendMessageRequest
from a2a.utils.constants import TransportProtocol
from uuid import uuid4

base_url = "http://localhost:4000/a2a/my-agent"  # LiteLLM proxy + agent name
headers = {"Authorization": "Bearer <your-master-key>"}    # LiteLLM master key or a virtual key

async with httpx.AsyncClient(headers=headers, timeout=60.0) as http_client:
    resolver = A2ACardResolver(httpx_client=http_client, base_url=base_url)
    agent_card = await resolver.get_agent_card()
    config = ClientConfig(
        httpx_client=http_client,
        streaming=False,
        supported_protocol_bindings=[TransportProtocol.JSONRPC, TransportProtocol.HTTP_JSON],
    )
    client = ClientFactory(config).create(agent_card)

    request = SendMessageRequest(
        message=Message(
            message_id=uuid4().hex,
            role=Role.ROLE_USER,
            parts=[Part(text="Hello!")],
        )
    )
    async for event in client.send_message(request):
        populated = event.ListFields()
        if populated and populated[0][0].name in ("message", "msg"):
            print("".join(getattr(p, "text", "") or "" for p in populated[0][1].parts))
```

[**Docs: A2A Agent Gateway**](https://docs.litellm.ai/docs/a2a)
**MCP Tools** - Connect MCP servers to any LLM (Python SDK + AI Gateway)
### Python SDK - MCP Bridge

```
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client
from litellm import experimental_mcp_client
import litellm

server_params = StdioServerParameters(command="python", args=["mcp_server.py"])

async with stdio_client(server_params) as (read, write):
    async with ClientSession(read, write) as session:
        await session.initialize()

        # Load MCP tools in OpenAI format
        tools = await experimental_mcp_client.load_mcp_tools(session=session, format="openai")

        # Use with any LiteLLM model
        response = await litellm.acompletion(
            model="gpt-4o",
            messages=[{"role": "user", "content": "What's 3 + 5?"}],
            tools=tools
        )
```

### AI Gateway - MCP Gateway
**Step 1.** [Add your MCP Server to the AI Gateway](https://docs.litellm.ai/docs/mcp#adding-your-mcp)
**Step 2.** Call MCP tools via `/chat/completions`

```
curl -X POST 'http://0.0.0.0:4000/v1/chat/completions' \
  -H 'Authorization: Bearer <your-master-key>' \
  -H 'Content-Type: application/json' \
  -d 
    "model": "gpt-4o",
    "messages": [{"role": "user", "content": "Summarize the latest open PR"}],
    "tools": [{
      "type": "mcp",
      "server_url": "litellm_proxy/mcp/github",
      "server_label": "github_mcp",
      "require_approval": "never"
    }]

```

### Use with Cursor IDE

```
{
  "mcpServers": {
    "LiteLLM": {
      "url": "http://localhost:4000/mcp/",
      "headers": {
        "x-litellm-api-key": "Bearer <your-master-key>"
      }
    }
  }
}
```

For MCP OAuth, an upstream may advertise dynamic client registration but refuse requests with HTTP 401 or 403. If the provider requires a pre-registered OAuth app, configure its `credentials.client_id` and, when required, `credentials.client_secret` on the MCP server. This skips dynamic registration in the gateway sign-in flow. The provider must approve the app for MCP access; reaching its authorization page does not establish that login or tool calls will succeed
[**Docs: MCP Gateway**](https://docs.litellm.ai/docs/mcp)
**Agents** - Run Claude Code, Codex, OpenCode or Deep Agents on any model (Python SDK)
### Python SDK - Agents

```
import litellm
from litellm import Harness, sandbox

result = litellm.agent(
    Harness.CLAUDE_CODE,  # or Harness.CODEX, Harness.OPENCODE, Harness.DEEPAGENTS
    "Find why tests/test_router.py is flaky and fix it.",
    sandbox=sandbox.local("./repo"),
    model="litellm_proxy/claude-sonnet-4-5",  # a model group on your AI Gateway
)

print(result.text, result.cost, [f.path for f in result.files])
```

Set `LITELLM_PROXY_API_BASE` and `LITELLM_PROXY_API_KEY` and every model call the agent makes goes through your AI Gateway, tagged `harness,claude_code`. Drop the `litellm_proxy/` prefix to call a provider directly. Install `starlette uvicorn` plus the agent's CLI (`claude`, `codex` or `opencode`), or `deepagents langchain-litellm` for Deep Agents.
[**Docs: Agent Harnesses**](https://docs.litellm.ai/docs/harness)
### Supported Providers ([Website Supported Models](https://models.litellm.ai/) | [Docs](https://docs.litellm.ai/docs/providers))  
| Provider  | `/chat/completions`  | `/messages`  | `/responses`  | `/embeddings`  | `/image/generations`  | `/audio/transcriptions`  | `/audio/speech`  | `/moderations`  | `/batches`  | `/rerank`  |  
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |  
| [Abliteration (`abliteration`)](https://docs.litellm.ai/docs/providers/abliteration)  | ✅  |  
| [AI/ML API (`aiml`)](https://docs.litellm.ai/docs/providers/aiml)  | ✅  | ✅  | ✅  | ✅  | ✅  |  
| [AI21 (`ai21`)](https://docs.litellm.ai/docs/providers/ai21)  | ✅  | ✅  | ✅  |  
| [AI21 Chat (`ai21_chat`)](https://docs.litellm.ai/docs/providers/ai21)  | ✅  | ✅  | ✅  |  
| ✅  | ✅  | ✅  |  
| ✅  | ✅  | ✅  |  
| [Anthropic (`anthropic`)](https://docs.litellm.ai/docs/providers/anthropic)  | ✅  | ✅  | ✅  | ✅  |  
| [Anthropic Text (`anthropic_text`)](https://docs.litellm.ai/docs/providers/anthropic)  | ✅  | ✅  | ✅  | ✅  |  
| ✅  | ✅  | ✅  |  
| [AssemblyAI (`assemblyai`)](https://docs.litellm.ai/docs/pass_through/assembly_ai)  | ✅  | ✅  | ✅  | ✅  |  
| [Auto Router (`auto_router`)](https://docs.litellm.ai/docs/proxy/auto_routing)  | ✅  | ✅  | ✅  |  
| [AWS - Bedrock (`bedrock`)](https://docs.litellm.ai/docs/providers/bedrock)  | ✅  | ✅  | ✅  | ✅  | ✅  |  
| [AWS - Sagemaker (`sagemaker`)](https://docs.litellm.ai/docs/providers/aws_sagemaker)  | ✅  | ✅  | ✅  | ✅  |  
| [Azure (`azure`)](https://docs.litellm.ai/docs/providers/azure)  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  |  
| [Azure AI (`azure_ai`)](https://docs.litellm.ai/docs/providers/azure_ai)  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  |  
| [Azure Text (`azure_text`)](https://docs.litellm.ai/docs/providers/azure)  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  |  
| [Baseten (`baseten`)](https://docs.litellm.ai/docs/providers/baseten)  | ✅  | ✅  | ✅  |  
| [Bytez (`bytez`)](https://docs.litellm.ai/docs/providers/bytez)  | ✅  | ✅  | ✅  |  
| [Cerebras (`cerebras`)](https://docs.litellm.ai/docs/providers/cerebras)  | ✅  | ✅  | ✅  |  
| [Clarifai (`clarifai`)](https://docs.litellm.ai/docs/providers/clarifai)  | ✅  | ✅  | ✅  |  
| [Cloudflare AI Workers (`cloudflare`)](https://docs.litellm.ai/docs/providers/cloudflare_workers)  | ✅  | ✅  | ✅  |  
| [Codestral (`codestral`)](https://docs.litellm.ai/docs/providers/codestral)  | ✅  | ✅  | ✅  |  
| [Cognition (`cognition`)](https://docs.litellm.ai/docs/providers/cognition)  | ✅  | ✅  | ✅  |  
| [Cohere (`cohere`)](https://docs.litellm.ai/docs/providers/cohere)  | ✅  | ✅  | ✅  | ✅  | ✅  |  
| [Cohere Chat (`cohere_chat`)](https://docs.litellm.ai/docs/providers/cohere)  | ✅  | ✅  | ✅  |  
| [CometAPI (`cometapi`)](https://docs.litellm.ai/docs/providers/cometapi)  | ✅  | ✅  | ✅  | ✅  |  
| [CompactifAI (`compactifai`)](https://docs.litellm.ai/docs/providers/compactifai)  | ✅  | ✅  | ✅  |  
| [Custom (`custom`)](https://docs.litellm.ai/docs/providers/custom_llm_server)  | ✅  | ✅  | ✅  |  
| [Custom OpenAI (`custom_openai`)](https://docs.litellm.ai/docs/providers/openai_compatible)  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  |  
| [Dashscope (`dashscope`)](https://docs.litellm.ai/docs/providers/dashscope)  | ✅  | ✅  | ✅  | ✅  | ✅  |  
| [Databricks (`databricks`)](https://docs.litellm.ai/docs/providers/databricks)  | ✅  | ✅  | ✅  |  
| [DataRobot (`datarobot`)](https://docs.litellm.ai/docs/providers/datarobot)  | ✅  | ✅  | ✅  |  
| [Deepgram (`deepgram`)](https://docs.litellm.ai/docs/providers/deepgram)  | ✅  | ✅  | ✅  | ✅  |  
| [DeepInfra (`deepinfra`)](https://docs.litellm.ai/docs/providers/deepinfra)  | ✅  | ✅  | ✅  |  
| [Deepseek (`deepseek`)](https://docs.litellm.ai/docs/providers/deepseek)  | ✅  | ✅  | ✅  |  
| [Eden AI (`edenai`)](https://docs.litellm.ai/docs/providers/edenai)  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  |  
| [ElevenLabs (`elevenlabs`)](https://docs.litellm.ai/docs/providers/elevenlabs)  | ✅  | ✅  | ✅  | ✅  | ✅  |  
| [Empower (`empower`)](https://docs.litellm.ai/docs/providers/empower)  | ✅  | ✅  | ✅  |  
| [Fal AI (`fal_ai`)](https://docs.litellm.ai/docs/providers/fal_ai)  | ✅  | ✅  | ✅  | ✅  |  
| [Featherless AI (`featherless_ai`)](https://docs.litellm.ai/docs/providers/featherless_ai)  | ✅  | ✅  | ✅  |  
| [Fireworks AI (`fireworks_ai`)](https://docs.litellm.ai/docs/providers/fireworks_ai)  | ✅  | ✅  | ✅  |  
| [FriendliAI (`friendliai`)](https://docs.litellm.ai/docs/providers/friendliai)  | ✅  | ✅  | ✅  |  
| [Galadriel (`galadriel`)](https://docs.litellm.ai/docs/providers/galadriel)  | ✅  | ✅  | ✅  |  
| [GitHub Copilot (`github_copilot`)](https://docs.litellm.ai/docs/providers/github_copilot)  | ✅  | ✅  | ✅  | ✅  |  
| [GitHub Models (`github`)](https://docs.litellm.ai/docs/providers/github)  | ✅  | ✅  | ✅  |  
| ✅  | ✅  | ✅  |  
| [Google - Vertex AI (`vertex_ai`)](https://docs.litellm.ai/docs/providers/vertex)  | ✅  | ✅  | ✅  | ✅  | ✅  |  
| [Google AI Studio - Gemini (`gemini`)](https://docs.litellm.ai/docs/providers/gemini)  | ✅  | ✅  | ✅  |  
| [GradientAI (`gradient_ai`)](https://docs.litellm.ai/docs/providers/gradient_ai)  | ✅  | ✅  | ✅  |  
| [Groq AI (`groq`)](https://docs.litellm.ai/docs/providers/groq)  | ✅  | ✅  | ✅  |  
| [Heroku (`heroku`)](https://docs.litellm.ai/docs/providers/heroku)  | ✅  | ✅  | ✅  |  
| [Hosted VLLM (`hosted_vllm`)](https://docs.litellm.ai/docs/providers/vllm)  | ✅  | ✅  | ✅  |  
| [Huggingface (`huggingface`)](https://docs.litellm.ai/docs/providers/huggingface)  | ✅  | ✅  | ✅  | ✅  | ✅  |  
| [Hyperbolic (`hyperbolic`)](https://docs.litellm.ai/docs/providers/hyperbolic)  | ✅  | ✅  | ✅  |  
| [IBM - Watsonx.ai (`watsonx`)](https://docs.litellm.ai/docs/providers/watsonx)  | ✅  | ✅  | ✅  | ✅  |  
| [Infinity (`infinity`)](https://docs.litellm.ai/docs/providers/infinity)  | ✅  |  
| [Jina AI (`jina_ai`)](https://docs.litellm.ai/docs/providers/jina_ai)  | ✅  |  
| [Lambda AI (`lambda_ai`)](https://docs.litellm.ai/docs/providers/lambda_ai)  | ✅  | ✅  | ✅  |  
| [Lemonade (`lemonade`)](https://docs.litellm.ai/docs/providers/lemonade)  | ✅  | ✅  | ✅  |  
| [LiteLLM Proxy (`litellm_proxy`)](https://docs.litellm.ai/docs/providers/litellm_proxy)  | ✅  | ✅  | ✅  | ✅  | ✅  |  
| [Llamafile (`llamafile`)](https://docs.litellm.ai/docs/providers/llamafile)  | ✅  | ✅  | ✅  |  
| [LM Studio (`lm_studio`)](https://docs.litellm.ai/docs/providers/lm_studio)  | ✅  | ✅  | ✅  |  
| [Maritalk (`maritalk`)](https://docs.litellm.ai/docs/providers/maritalk)  | ✅  | ✅  | ✅  |  
| [Meta - Llama API (`meta_llama`)](https://docs.litellm.ai/docs/providers/meta_llama)  | ✅  | ✅  | ✅  |  
| [Mistral AI API (`mistral`)](https://docs.litellm.ai/docs/providers/mistral)  | ✅  | ✅  | ✅  | ✅  |  
| [ModelScope (`modelscope`)](https://docs.litellm.ai/docs/providers/modelscope)  | ✅  | ✅  | ✅  | ✅  |  
| [Moonshot (`moonshot`)](https://docs.litellm.ai/docs/providers/moonshot)  | ✅  | ✅  | ✅  |  
| [Morph (`morph`)](https://docs.litellm.ai/docs/providers/morph)  | ✅  | ✅  | ✅  |  
| [Nebius AI Studio (`nebius`)](https://docs.litellm.ai/docs/providers/nebius)  | ✅  | ✅  | ✅  | ✅  |  
| [NLP Cloud (`nlp_cloud`)](https://docs.litellm.ai/docs/providers/nlp_cloud)  | ✅  | ✅  | ✅  |  
| [Novita AI (`novita`)](https://novita.ai/models/llm?utm_source=github_litellm&utm_medium=github_readme&utm_campaign=github_link)  | ✅  | ✅  | ✅  |  
| [Nscale (`nscale`)](https://docs.litellm.ai/docs/providers/nscale)  | ✅  | ✅  | ✅  |  
| [Nvidia NIM (`nvidia_nim`)](https://docs.litellm.ai/docs/providers/nvidia_nim)  | ✅  | ✅  | ✅  |  
| [OCI (`oci`)](https://docs.litellm.ai/docs/providers/oci)  | ✅  | ✅  | ✅  |  
| [Ollama (`ollama`)](https://docs.litellm.ai/docs/providers/ollama)  | ✅  | ✅  | ✅  | ✅  |  
| [Ollama Chat (`ollama_chat`)](https://docs.litellm.ai/docs/providers/ollama)  | ✅  | ✅  | ✅  |  
| [Oobabooga (`oobabooga`)](https://docs.litellm.ai/docs/providers/openai_compatible)  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  |  
| [OpenAI (`openai`)](https://docs.litellm.ai/docs/providers/openai)  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  |  
| [OpenAI-like (`openai_like`)](https://docs.litellm.ai/docs/providers/openai_compatible)  | ✅  |  
| [OpenRouter (`openrouter`)](https://docs.litellm.ai/docs/providers/openrouter)  | ✅  | ✅  | ✅  |  
| [OVHCloud AI Endpoints (`ovhcloud`)](https://docs.litellm.ai/docs/providers/ovhcloud)  | ✅  | ✅  | ✅  |  
| [Perplexity AI (`perplexity`)](https://docs.litellm.ai/docs/providers/perplexity)  | ✅  | ✅  | ✅  |  
| [Petals (`petals`)](https://docs.litellm.ai/docs/providers/petals)  | ✅  | ✅  | ✅  |  
| [Pinstripes (`pinstripes`)](https://docs.litellm.ai/docs/providers/pinstripes)  | ✅  | ✅  | ✅  |  
| [Predibase (`predibase`)](https://docs.litellm.ai/docs/providers/predibase)  | ✅  | ✅  | ✅  |  
| [Qianwen AI Platform (`qwen_ai_platform`)](https://docs.litellm.ai/docs/providers/qwencloud)  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  |  
| [QwenCloud (`qwencloud`)](https://docs.litellm.ai/docs/providers/qwencloud)  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  |  
| [Recraft (`recraft`)](https://docs.litellm.ai/docs/providers/recraft)  | ✅  |  
| [Replicate (`replicate`)](https://docs.litellm.ai/docs/providers/replicate)  | ✅  | ✅  | ✅  |  
| [Sagemaker Chat (`sagemaker_chat`)](https://docs.litellm.ai/docs/providers/aws_sagemaker)  | ✅  | ✅  | ✅  |  
| [Sail (`sail`)](https://docs.litellm.ai/docs/providers/sail)  | ✅  | ✅  | ✅  |  
| [Sambanova (`sambanova`)](https://docs.litellm.ai/docs/providers/sambanova)  | ✅  | ✅  | ✅  |  
| [Snowflake (`snowflake`)](https://docs.litellm.ai/docs/providers/snowflake)  | ✅  | ✅  | ✅  |  
| [Strands Decider (`strands_decider`)](https://docs.litellm.ai/docs/providers)  |  
| [Text Completion Codestral (`text-completion-codestral`)](https://docs.litellm.ai/docs/providers/codestral)  | ✅  | ✅  | ✅  |  
| [Text Completion OpenAI (`text-completion-openai`)](https://docs.litellm.ai/docs/providers/text_completion_openai)  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  | ✅  |  
| [Together AI (`together_ai`)](https://docs.litellm.ai/docs/providers/togetherai)  | ✅  | ✅  | ✅  |  
| [Topaz (`topaz`)](https://docs.litellm.ai/docs/providers/topaz)  | ✅  | ✅  | ✅  |  
| [Triton (`triton`)](https://docs.litellm.ai/docs/providers/triton-inference-server)  | ✅  | ✅  | ✅  |  
| [Typesafe Decisions API (`typesafe`)](https://docs.litellm.ai/docs/providers)  |  
| [V0 (`v0`)](https://docs.litellm.ai/docs/providers/v0)  | ✅  | ✅  | ✅  |  
| [Vercel AI Gateway (`vercel_ai_gateway`)](https://docs.litellm.ai/docs/providers/vercel_ai_gateway)  | ✅  | ✅  | ✅  |  
| [VLLM (`vllm`)](https://docs.litellm.ai/docs/providers/vllm)  | ✅  | ✅  | ✅  |  
| [Volcengine (`volcengine`)](https://docs.litellm.ai/docs/providers/volcano)  | ✅  | ✅  | ✅  |  
| [VoyageAI by MongoDB (`voyage`)](https://docs.litellm.ai/docs/providers/voyage)  | ✅  |  
| [WandB Inference (`wandb`)](https://docs.litellm.ai/docs/providers/wandb_inference)  | ✅  | ✅  | ✅  |  
| [Watsonx Text (`watsonx_text`)](https://docs.litellm.ai/docs/providers/watsonx)  | ✅  | ✅  | ✅  |  
| [xAI (`xai`)](https://docs.litellm.ai/docs/providers/xai)  | ✅  | ✅  | ✅  |  
| [Xinference (`xinference`)](https://docs.litellm.ai/docs/providers/xinference)  | ✅  |  
[**Read the Docs**](https://docs.litellm.ai/docs/)
## Get Started
You can use LiteLLM through either the Proxy Server or Python SDK. Both give you a unified interface to access multiple LLMs (100+ LLMs). Choose the option that best fits your needs:  
| **[LiteLLM AI Gateway](https://docs.litellm.ai/docs/simple_proxy)**  | **[LiteLLM Python SDK](https://docs.litellm.ai/docs/)**  |  
| --- | --- |  
| **Use Case**  | Central service (LLM Gateway) to access multiple LLMs  | Use LiteLLM directly in your Python code  |  
| **Who Uses It?**  | Gen AI Enablement / ML Platform Teams  | Developers building LLM projects  |  
| **Key Features**  | Centralized API gateway with authentication and authorization, multi-tenant cost tracking and spend management per project/user, per-project customization (logging, guardrails, caching), virtual keys for secure access control, admin dashboard UI for monitoring and management  | Direct Python library integration in your codebase, Router with retry/fallback logic across multiple deployments (e.g. Azure/OpenAI) - [Router](https://docs.litellm.ai/docs/routing), application-level load balancing and cost tracking, exception handling with OpenAI-compatible errors, observability callbacks (Lunary, MLflow, Langfuse, etc.)  |  
**Stable Release:** Use docker images with the `-stable` tag. These have undergone 12 hour load tests, before being published. [More information about the release cycle here](https://docs.litellm.ai/docs/proxy/release_cycle)
Support for more providers. Missing a provider or LLM Platform, raise a [feature request](https://github.com/BerriAI/litellm/issues/new?assignees=&labels=enhancement&projects=&template=feature_request.yml&title=%5BFeature%5D%3A+).
### Deploy on AWS or GCP with Terraform
Run the LiteLLM proxy as a production-ready componentized stack (gateway, backend, UI on separate services; managed Postgres + Redis + object store) using the published Terraform modules. Both modules are on the [public Terraform Registry](https://registry.terraform.io/namespaces/BerriAI) — no auth needed.
#### AWS — ECS Fargate + Aurora + ElastiCache + ALB
— opens an in-browser shell, already authenticated to your AWS account. Once inside, run:

```
git clone https://github.com/BerriAI/litellm.git
cd litellm/terraform/litellm/aws/examples/default
cp terraform.tfvars.example terraform.tfvars   # edit region/tenant/env
terraform init  terraform apply
```

[Module page →](https://registry.terraform.io/modules/BerriAI/litellm/aws/latest)
Or call the module from your own root config:

```
# main.tf
terraform {
  required_version = ">= 1.6.0"
  required_providers {
    { source = "hashicorp/aws", version = "~> 5.60" }
  }
}

provider "aws" {
  region = "us-west-2"
}

module "litellm" {
  source  = "BerriAI/litellm/aws"
  version = "~> 1.89"

  region = "us-west-2"
  ["us-west-2a", "us-west-2b"]
  tenant = "acme"
  "prod"

  # Production: provide an ACM cert. Without one, set allow_plaintext_alb = true
  # (dev/trial only).
  # acm_certificate_arn = "arn:aws:acm:us-west-2:111122223333:certificate/..."
  allow_plaintext_alb = true
}

output "litellm_url" {
  value = module.litellm.alb_dns_name
}
```


```
terraform init
terraform apply
```

Provider API keys live in AWS Secrets Manager; reference ARNs via `gateway_extra_secrets`. Full input list and architecture diagram on the [registry page](https://registry.terraform.io/modules/BerriAI/litellm/aws/latest?tab=inputs).
#### GCP — Cloud Run + Cloud SQL + Memorystore + HTTPS LB
Real 1-click. Opens Cloud Shell, clones this repo, and walks you through `terraform apply` via a built-in [DeployStack tutorial](https://github.com/BerriAI/litellm/blob/main/terraform/litellm/gcp/examples/default/TUTORIAL.md) — pick the project, the tutorial sets up the Artifact Registry remote repo, writes `terraform.tfvars` from your answers, and runs apply.
[Module page →](https://registry.terraform.io/modules/BerriAI/litellm/google/latest)
To call the module from your own config instead, Cloud Run can't pull from `ghcr.io` directly, so first set up a one-time Artifact Registry remote repo backed by GHCR:

```
gcloud artifacts repositories create litellm \
  --location=us-central1 \
  --repository-format=docker \
  --mode=remote-repository \
  --remote-docker-repo=https://ghcr.io \
  --project=my-gcp-project
```

Then:

```
# main.tf
terraform {
  required_version = ">= 1.6.0"
  required_providers {
    google      = { source = "hashicorp/google",      version = "~> 6.10" }
    google-beta = { source = "hashicorp/google-beta", version = "~> 6.10" }
  }
}

provider "google"      { project = "my-gcp-project"; region = "us-central1" }
provider "google-beta" { project = "my-gcp-project"; region = "us-central1" }

module "litellm" {
  source  = "BerriAI/litellm/google"
  version = "~> 1.89"

  project_id = "my-gcp-project"
  region     = "us-central1"
  tenant     = "acme"
  "prod"

  # Replace my-gcp-project with your GCP project ID (same value as project_id above).
  image_registry = "us-central1-docker.pkg.dev/my-gcp-project/litellm/berriai"

  # Production: provide DNS already pointing at the LB IP for Google-managed certs.
  # Without one, set allow_plaintext_lb = true (dev/trial only).
  # lb_domains         = ["proxy.example.com"]
  allow_plaintext_lb = true
}

output "litellm_url" {
  value = module.litellm.load_balancer_url
}
```


```
terraform init
terraform apply
```

Provider API keys live in Secret Manager; reference resource IDs (e.g. `projects/my-gcp-project/secrets/openai-api-key`) via `gateway_extra_secrets`. Full input list and architecture diagram on the [registry page](https://registry.terraform.io/modules/BerriAI/litellm/google/latest?tab=inputs).
#### Both stacks include
  * The full componentized split (gateway / backend / UI as independent services)
  * Managed Postgres (writer + reader) and Redis
  * Versioned object store for proxy state + file uploads
  * An auto-generated `LITELLM_MASTER_KEY` in your cloud's secret manager
  * A one-off migration job that runs `prisma migrate deploy` before the proxy starts
  * The same `proxy_config` surface as the [Helm chart](https://github.com/BerriAI/litellm/blob/main/helm/litellm) — pass YAML as a typed map


The Terraform modules live at [`terraform/litellm/aws/`](https://github.com/BerriAI/litellm/blob/main/terraform/litellm/aws) and [`terraform/litellm/gcp/`](https://github.com/BerriAI/litellm/blob/main/terraform/litellm/gcp) in this repo; the registry entries are read-only mirrors updated on each release.
### Run in Developer Mode
#### Services
  1. Setup .env file in root
  2. Run dependent services `docker-compose up db prometheus`


#### Backend
  1. Run `make bootstrap`
  2. Start proxy backend: `uv run python litellm/proxy/proxy_cli.py`


#### Frontend
  1. Navigate to `ui/litellm-dashboard` (dependencies were already installed w/ `make bootstrap`)
  2. Start dashboard: `npm run dev`


### Verify Docker Image Signatures
All LiteLLM Docker images published to GHCR are signed with [cosign](https://docs.sigstore.dev/cosign/overview/). Every release is signed with the same key introduced in [commit `0112e53`](https://github.com/BerriAI/litellm/commit/0112e53046018d726492c814b3644b7d376029d0).
**Verify using the pinned commit hash (recommended):**
A commit hash is cryptographically immutable, so this is the strongest way to ensure you are using the original signing key:

```
cosign verify \
  --key https://raw.githubusercontent.com/BerriAI/litellm/0112e53046018d726492c814b3644b7d376029d0/cosign.pub \
  ghcr.io/berriai/litellm:release-tag
```

**Verify using a release tag (convenience):**
Tags are protected in this repository and resolve to the same key. This option is easier to read but relies on tag protection rules:

```
cosign verify \
  --key https://raw.githubusercontent.com/BerriAI/litellm/release-tag/cosign.pub \
  ghcr.io/berriai/litellm:release-tag
```

Replace `<release-tag>` with the version you are deploying (e.g. `v1.83.0-stable`).
# Enterprise
For companies that need better security, user management and professional support
[Get an Enterprise License](https://litellm.ai/enterprise) [Talk to founders](https://enterprise.litellm.ai/demo)
This covers:
  * ✅ **Features under the[LiteLLM Commercial License](https://docs.litellm.ai/docs/proxy/enterprise):**
  * ✅ **Feature Prioritization**
  * ✅ **Custom Integrations**
  * ✅ **Professional Support - Dedicated discord + slack**
  * ✅ **Custom SLAs**
  * ✅ **Secure access with Single Sign-On**


# Contributing
We welcome contributions to LiteLLM! Whether you're fixing bugs, adding features, or improving documentation, we appreciate your help.
## Quick Start for Contributors
This requires uv to be installed.

```
git clone https://github.com/BerriAI/litellm.git
cd litellm
make install-dev    # Install development dependencies
make format         # Format your code
make lint           # Run all linting checks
make test-unit      # Run unit tests
make format-check   # Check formatting only
```

For detailed contributing guidelines, see [CONTRIBUTING.md](https://github.com/BerriAI/litellm/blob/main/CONTRIBUTING.md).
> **📖 Contributing to documentation?** The LiteLLM docs have moved to a separate repository: [BerriAI/litellm-docs](https://github.com/BerriAI/litellm-docs). Please open doc PRs there. Docs are served at [docs.litellm.ai](https://docs.litellm.ai).
## Code Quality / Linting
LiteLLM follows the [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html).
Our automated checks include:
  * **Ruff** for formatting, linting, and code quality
  * **basedpyright** for type checking
  * **Circular import detection**
  * **Import safety checks**


All these checks must pass before your PR can be merged.
# Support / talk with founders
  * [Community Discord 💭](https://discord.gg/wuPM9dRgDw)
  * [Community Slack 💭](https://www.litellm.ai/support)
  * Our emails ✉️ ishaan@berri.ai / krrish@berri.ai


# Contributors
## About
The fastest, litest AI Gateway. Rust core with Python SDK. Call 100+ LLM APIs in OpenAI (or native) format with cost tracking, guardrails, load balancing, and logging [Bedrock, Azure, OpenAI, Anthropic, OpenAI, VertexAI, vLLM, Nvidia NIM]
### Topics
[ai-gateway](https://github.com/topics/ai-gateway)[anthropic](https://github.com/topics/anthropic)[azure-openai](https://github.com/topics/azure-openai)[bedrock](https://github.com/topics/bedrock)[gateway](https://github.com/topics/gateway)[langchain](https://github.com/topics/langchain)[litellm](https://github.com/topics/litellm)[llm](https://github.com/topics/llm)[llm-gateway](https://github.com/topics/llm-gateway)[llmops](https://github.com/topics/llmops)[mcp-gateway](https://github.com/topics/mcp-gateway)[openai](https://github.com/topics/openai)[openai-proxy](https://github.com/topics/openai-proxy)[rust](https://github.com/topics/rust)[rust-ai](https://github.com/topics/rust-ai)[vertex-ai](https://github.com/topics/vertex-ai)
### Resources
### Contributing
### Security policy
### Stars
**60.6k** stars
### Watchers
**236** watching
### Forks
[**12.2k** forks](https://github.com/BerriAI/litellm/forks)
## [Releases](https://github.com/BerriAI/litellm/releases)1,504 (1,504)
[ v1.104.2Latest Oct 8, 2026 ](https://github.com/BerriAI/litellm/releases/tag/v1.104.2)
## Sponsor this project
  * <https://buy.stripe.com/9AQ03Kd3P91o0Q8bIS>


## 
[+ 25,330](https://github.com/BerriAI/litellm/network/dependents)
## [Contributors](https://github.com/BerriAI/litellm/graphs/contributors)1,919 (1,919)
## Languages
  * [Python82%](https://github.com/BerriAI/litellm/search?l=python)
  * [TypeScript12.4%](https://github.com/BerriAI/litellm/search?l=typescript)
  * [Rust3.9%](https://github.com/BerriAI/litellm/search?l=rust)
  * [HTML0.8%](https://github.com/BerriAI/litellm/search?l=html)
  * [Go0.5%](https://github.com/BerriAI/litellm/search?l=go)
  * [HCL0.2%](https://github.com/BerriAI/litellm/search?l=hcl)
  * [Other0.2%](https://github.com/BerriAI/litellm/search?l=Other)


You can’t perform that action at this time. 
