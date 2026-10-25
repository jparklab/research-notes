---
title: 'GitHub - agents4science/course-infra: Course infrastructure for AI Agents
  for Science: gateway, budgets, traces, sandbox (CMSC 35370 Winter 2027) · GitHub'
id: github-agents4sciencecourse-infra-course-infrastructure-for-ai-agents-for-scienc
tags:
- onprem-llm-inference-stacks-859099
- gateway
created: '2026-10-23T17:22:28.605937Z'
updated: '2026-10-23T17:23:50.573827Z'
source: https://github.com/agents4science/course-infra
source_domain: github.com
fetched_at: '2026-10-23T17:22:28.578391Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'University of Chicago CMSC 35370 course-infra repo (Oct 2026, Ian Foster):
  LiteLLM proxy gateway with per-student virtual keys and budgets, routing to commercial
  lane and an ''Argonne lane'' (ALCF inference endpoints, OpenAI-compatible; Sophia
  vLLM endpoint), Phoenix/OTel traces, Docker sandbox; production on course AWS VM.
  Downstream-consumer example: LiteLLM in front of HPC-hosted inference, Globus token
  refreshed by cron every 4h.'
---

[Skip to content](https://github.com/agents4science/course-infra#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/agents4science/course-infra) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/agents4science/course-infra) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/agents4science/course-infra) to refresh your session. Dismiss alert
[**1** Branch](https://github.com/agents4science/course-infra/branches)[](https://github.com/agents4science/course-infra/tags)
Go to file
Open more actions menu
## Latest commit
[Export pipeline skeleton (I7): consent-filtered, pseudonymized JSONL …](https://github.com/agents4science/course-infra/commit/eb4e922edfcfa59d2f92761b231bb18ada5c7b5b)
Open commit detailssuccess
Oct 13, 2026
[eb4e922](https://github.com/agents4science/course-infra/commit/eb4e922edfcfa59d2f92761b231bb18ada5c7b5b) · Oct 13, 2026
## History
[20 Commits](https://github.com/agents4science/course-infra/commits/master/)
Open commit details
20 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| [.github/workflows](https://github.com/agents4science/course-infra/tree/master/.github/workflows "This path skips through empty directories")  | [.github/workflows](https://github.com/agents4science/course-infra/tree/master/.github/workflows "This path skips through empty directories")  | [CI: syntax, YAML, and compose-config checks](https://github.com/agents4science/course-infra/commit/ca713170a44c966d4c1ee20ae4f2abfb166e4f03 "CI: syntax, YAML, and compose-config checks")  | Oct 12, 2026  |  
|   |   | [Bake-off harness v0 (I6): task sets, sandboxed scoring, cost/time lea…](https://github.com/agents4science/course-infra/commit/ce2d61dfd6f475a877ce134840cd6b65c6bfd1bc "Bake-off harness v0 \(I6\): task sets, sandboxed scoring, cost/time leaderboard")  | Oct 13, 2026  |  
|   |   | [Export pipeline skeleton (I7): consent-filtered, pseudonymized JSONL …](https://github.com/agents4science/course-infra/commit/eb4e922edfcfa59d2f92761b231bb18ada5c7b5b "Export pipeline skeleton \(I7\): consent-filtered, pseudonymized JSONL corpus with stage-1 redaction")  | Oct 13, 2026  |  
|   |   | [I4 live: OTel->Phoenix trace capture verified (content, usage, per-st…](https://github.com/agents4science/course-infra/commit/f92f6aa34ed251d4f73fd15023fe9f3d7a50a6d8 "I4 live: OTel->Phoenix trace capture verified \(content, usage, per-student identity\); ADR-3 decided")  | Oct 12, 2026  |  
|   |   | [Bake-off harness v0 (I6): task sets, sandboxed scoring, cost/time lea…](https://github.com/agents4science/course-infra/commit/ce2d61dfd6f475a877ce134840cd6b65c6bfd1bc "Bake-off harness v0 \(I6\): task sets, sandboxed scoring, cost/time leaderboard")  | Oct 13, 2026  |  
|   |   | [Budget governor (I2): tiered spend report, fleet budget panel, studen…](https://github.com/agents4science/course-infra/commit/3ef37a7785cbd1e9a5700e101bd7c13075d717fe "Budget governor \(I2\): tiered spend report, fleet budget panel, student self-service usage page")  | Oct 13, 2026  |  
|   |   | [Budget governor (I2): tiered spend report, fleet budget panel, studen…](https://github.com/agents4science/course-infra/commit/3ef37a7785cbd1e9a5700e101bd7c13075d717fe "Budget governor \(I2\): tiered spend report, fleet budget panel, student self-service usage page")  | Oct 13, 2026  |  
|   |   | [Sprint 0: gateway skeleton (LiteLLM), ADRs, provisioning script — smo…](https://github.com/agents4science/course-infra/commit/4ca325cdef6ef224cbd00d68856aacc61705db04 "Sprint 0: gateway skeleton \(LiteLLM\), ADRs, provisioning script — smoke-tested end to end")  | Oct 11, 2026  |  
|   |   | [Export pipeline skeleton (I7): consent-filtered, pseudonymized JSONL …](https://github.com/agents4science/course-infra/commit/eb4e922edfcfa59d2f92761b231bb18ada5c7b5b "Export pipeline skeleton \(I7\): consent-filtered, pseudonymized JSONL corpus with stage-1 redaction")  | Oct 13, 2026  |  
|   |   | [Sandbox runner (I3): course image, resource-limited execution, intern…](https://github.com/agents4science/course-infra/commit/b47cff1198612029302396114ba76de95bdf7a9d "Sandbox runner \(I3\): course image, resource-limited execution, internal-only network")  | Oct 13, 2026  |  
|   |   | [I4 live: OTel->Phoenix trace capture verified (content, usage, per-st…](https://github.com/agents4science/course-infra/commit/f92f6aa34ed251d4f73fd15023fe9f3d7a50a6d8 "I4 live: OTel->Phoenix trace capture verified \(content, usage, per-student identity\); ADR-3 decided")  | Oct 12, 2026  |  
|   |   | [Sandbox runner (I3): course image, resource-limited execution, intern…](https://github.com/agents4science/course-infra/commit/b47cff1198612029302396114ba76de95bdf7a9d "Sandbox runner \(I3\): course image, resource-limited execution, internal-only network")  | Oct 13, 2026  |  
| View all files |  
## Repository files navigation
# course-infra — Instruments for AI Agents for Science (Winter 2027)
Infrastructure for [CMSC 35370, Winter 2027](https://agents4science.github.io/2027/): the course gateway, trace capture, budgets, sandbox, and supporting tools. Design and requirements: [instrument development plan](https://agents4science.github.io/2027/instruments.html).
## Architecture

```
students ──(per-student virtual keys)──> GATEWAY (LiteLLM proxy)
                                           │  budgets, spend caps, routing
                                           ├──> commercial lane (OpenAI, Anthropic, ...)
                                           ├──> Argonne lane (ALCF inference, OpenAI-compatible)
                                           └──> trace capture ──> trace store + viewer
instructor/TAs ──> oversight dashboard (usage, budgets, sessions)
bake-offs/projects ──> sandbox runners (containers) ──> gateway

```
  
| Component  | Instrument  | Approach  |  
| --- | --- | --- |  
| Gateway  | I1  |  **LiteLLM Proxy** (decided; see DECISIONS.md): virtual keys, per-key budgets, routing, callbacks  |  
| Budget governor  | I2  | LiteLLM budget primitives + policy layer (adapts [Bursar](https://agents4science.github.io/Bursar/) concepts)  |  
| Sandbox  | I3  | Docker-based execution, resource-limited, default-deny network; MCP-mediated tools  |  
| Trace store/viewer  | I4  | OTel callback → Phoenix (decided; full content + per-student identity verified); Postgres spend logs; JSONL export for I7  |  
| Oversight dashboard  | I5  | v0 = LiteLLM admin UI + trace viewer; custom fleet view later  |  
| Bake-off harness  | I6  | Existing open eval tooling + task specs (sprint 3)  |  
| Consent/redaction  | I7  | Post-quarter export pipeline (IRB-gated; session-relative timestamps)  |  
## Security requirements (IRB commitments — not optional)
TLS on all connections; encryption at rest; role-based admin access with University 2FA; students can access only their own sessions; access logging; no unauthenticated endpoints. See the instrument plan and IRB protocol.
## Quickstart (development)

```
cp .env.example .env            # fill in provider keys + master key
docker compose up -d            # gateway + postgres
python scripts/provision_students.py roster.csv   # one virtual key per student
```

Students then point any OpenAI-compatible client at `https://<gateway-host>/v1` with their personal key.
## Enabling the Argonne lane (ALCF Inference Endpoints)
Agents can route to open models served on ALCF hardware by requesting Argonne-lane model names (e.g., `argonne-coder`) through the same gateway and the same student key — students never handle Globus credentials. Setup (operator, once):
  1. Prerequisite: an ALCF account with an allocation covering the inference service (for the class: the project-level service credential — see the instrument plan's accounts section).
  2. One-time interactive login: `uvx --from alcf-ai alcf-ai auth login`
  3. Set `ALCF_BASE_URL` in `.env` (e.g., the Sophia vLLM endpoint: `https://inference-api.alcf.anl.gov/resource_server/sophia/vllm/v1`).
  4. Run the lane manager once to create the lane, then cron it (Globus access tokens expire after hours): `0 */4 * * * LITELLM_MASTER_KEY=... scripts/refresh_alcf_token.py`. The lane is DB-managed (`STORE_MODEL_IN_DB=True` in compose) because config-file models cannot be token-rotated via the admin API; the script deletes stale deployments and recreates them with a fresh token, with no gateway restart.


A `401` from the Argonne lane means a stale token — run the refresher. Served model list changes; check the [ALCF inference docs](https://docs.alcf.anl.gov/services/inference-endpoints/) and add entries to `gateway/config.yaml` accordingly. Lane roles (red-team week, bulk judging, open-vs-frontier comparisons) are defined in the [instrument plan](https://agents4science.github.io/2027/instruments.html).
## Repository layout
  * `gateway/config.yaml` — LiteLLM configuration: model lanes, budgets, logging
  * `docker-compose.yml` — gateway + Postgres + Phoenix trace viewer (dev); production deploys to the course AWS VM
  * `scripts/provision_students.py` — create per-student keys with quarter budgets from a roster
  * `DECISIONS.md` — architecture decision records (sprint 0)


## Status
Sprint 0 (October 2026). Target: gateway MVP by mid-November; freeze January 2, 2027. Timeline in the [instrument plan](https://agents4science.github.io/2027/instruments.html).
## About
Course infrastructure for AI Agents for Science: gateway, budgets, traces, sandbox (CMSC 35370 Winter 2027)
### Resources
### Stars
**0** stars
### Watchers
**0** watching
### Forks
[ forks](https://github.com/agents4science/course-infra/forks)
## 
No releases published
## 
  * [**ianfoster** Ian Foster](https://github.com/ianfoster)


## Languages
  * [Python99.2%](https://github.com/agents4science/course-infra/search?l=python)
  * [Dockerfile0.8%](https://github.com/agents4science/course-infra/search?l=dockerfile)


You can’t perform that action at this time. 
