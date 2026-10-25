---
title: 'GitHub - llm-d/llm-d-router: llm-d Router: The intelligent entry point for
  inference requests · GitHub'
id: github-llm-dllm-d-router-llm-d-router-the-intelligent-entry-point-for-inferenc-3
tags:
- llmd-kv-cache-routing-c665d5
- llm-d
- epp
- routing
- kubernetes
created: '2026-09-14T11:56:43.317234Z'
updated: '2026-09-15T04:55:24.080962Z'
source: https://github.com/llm-d/llm-d-inference-scheduler
source_domain: github.com
fetched_at: '2026-09-14T11:56:43.297266Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'llm-d-router GitHub repository README (v0.10.0 latest, 336 stars, ~97% Go)
  — source repo for the plugins cited across llm-d''s KV-cache routing docs (precise-prefix-cache-scorer,
  tokenizer/token-producer, approx-prefix-cache-producer). Notes a terminology change:
  ''Inference Scheduler'' was renamed to ''llm-d Router,'' and core Endpoint Picker
  (EPP) code plus the InferenceObjective and InferenceModelRewrite APIs were merged
  into this repo from the Gateway API Inference Extension (GIE) project — GIE now
  hosts only the InferencePool API (an extension of the Kubernetes Gateway API) and
  defines the Endpoint Picker Protocol. Describes the llm-d Router as the intelligent
  entry point for inference traffic delivering LLM load- and prefix-cache-aware routing,
  request prioritization, and flow control; it integrates with proxies (e.g. Envoy)
  via the ext-proc protocol and supports two deployment modes: Standalone Mode (self-managed
  Envoy, either sidecar-in-EPP-pod or separately scalable Service topology) and Gateway
  Mode (production-recommended, EPP as backend for an InferencePool referenced by
  an HTTPRoute on a shared Gateway, enabling multi-cluster load balancing). Core components:
  the EPP itself (evaluates requests against InferencePool state considering KV-cache
  locality, load, and priority), Request Management APIs (InferenceObjective for scheduling
  goals/priority, InferenceModelRewrite for traffic-shaping model-name rewrites),
  and a Disaggregation Sidecar deployed alongside model servers (typically on the
  decode worker) that orchestrates P/D and E/P/D multi-stage inference lifecycles
  by coordinating KV-cache and embedding transfers between encode/prefill/decode workers.
  Points to further docs: architecture.md (routing logic and filter/scorer plugins)
  and operations.md (EPP container sizing for heavy/long-context workloads). Note:
  the raw fetched page is dominated by GitHub commit-log noise (individual PR titles/descriptions);
  the README/project-description content is concentrated in the latter portion of
  the body.'
---

*Suggested by [[kv-cache-indexer-llm-d]] — cited as source for the precise-prefix-cache-scorer and tokenizer plugins*

[Skip to content](https://github.com/llm-d/llm-d-router#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  336 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


[**11** Branches](https://github.com/llm-d/llm-d-router/branches)[](https://github.com/llm-d/llm-d-router/tags)
Go to file
Open more actions menu
## Latest commit
[refactor: unify API type handling across sidecar and coordinator (](https://github.com/llm-d/llm-d-router/commit/d5ff9100312b4a39d7ba050638e149d37ff46b4a)[#2743](https://github.com/llm-d/llm-d-router/pull/2743)[)](https://github.com/llm-d/llm-d-router/commit/d5ff9100312b4a39d7ba050638e149d37ff46b4a)
Open commit detailspending
Sep 14, 2026
[d5ff910](https://github.com/llm-d/llm-d-router/commit/d5ff9100312b4a39d7ba050638e149d37ff46b4a) · Sep 14, 2026
## History
[2,963 Commits](https://github.com/llm-d/llm-d-router/commits/main/)
Open commit details
2,963 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |  [feat(docs): agentic context engineering (](https://github.com/llm-d/llm-d-router/commit/5fc77aa7d2e243970247fad1dd3c7c9172f17da6 "feat\(docs\): agentic context engineering \(#1295\)

* docs: add AGENTS.md and symlinks for common agent harnesses

Single source of truth at AGENTS.md covering code style, git workflow,
and agent operating rules. Symlinks point legacy tool-specific paths
\(.cursorrules, .cursor/rules/main.mdc, .windsurfrules, GEMINI.md,
.github/copilot-instructions.md\) at the same file so one source covers
agents that don't yet read AGENTS.md natively.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* docs: forbid AI co-author trailers; track CLAUDE.md symlink

DCO sign-off is the only required trailer. Drop CLAUDE.md from
.gitignore so the symlink to AGENTS.md tracks alongside the rest.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* docs: drop legacy agent context symlinks

Current major agents \(Codex, Cursor, Copilot, Windsurf, Gemini CLI,
Amp, Devin\) read AGENTS.md natively. Only Claude Code still requires
CLAUDE.md, kept as a symlink. The rest are removed.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* docs: restore Copilot and Gemini CLI hooks for AGENTS.md

GitHub Copilot supports AGENTS.md only partially; full-featured
repository instructions still flow through .github/copilot-instructions.md.
Gemini CLI reads GEMINI.md by default and requires .gemini/settings.json
to point at AGENTS.md.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* docs: ground AGENTS.md opening in community context

Name the project as multi-vendor open source under llm-d and surface
the shared-review-bandwidth principle that the rules below build on.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* .gitignore: ignore .agents/ local agent workspace

Carve out a default-ignored directory for SDD-style local agent state
\(plans, research notes, memory files\) so contributors don't have to
extend .gitignore per checkout.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* AGENTS.md: add verify-against-code and minimal-change principles

Two cross-cutting working principles: verify behavior against the code
rather than from filenames or familiarity, and prefer the smallest
correct change inside a tracked issue.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* AGENTS.md: name minimalism, split Pull requests into bullets

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* rollback .gitignore

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* add agentic section to gitignore

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* AGENTS.md: add intent-first and checkable-success principles

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* AGENTS.md: add exception, re-plan, mention, and self-check rules

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

---------

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>")[#1295](https://github.com/llm-d/llm-d-router/pull/1295)[)](https://github.com/llm-d/llm-d-router/commit/5fc77aa7d2e243970247fad1dd3c7c9172f17da6 "feat\(docs\): agentic context engineering \(#1295\)

* docs: add AGENTS.md and symlinks for common agent harnesses

Single source of truth at AGENTS.md covering code style, git workflow,
and agent operating rules. Symlinks point legacy tool-specific paths
\(.cursorrules, .cursor/rules/main.mdc, .windsurfrules, GEMINI.md,
.github/copilot-instructions.md\) at the same file so one source covers
agents that don't yet read AGENTS.md natively.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* docs: forbid AI co-author trailers; track CLAUDE.md symlink

DCO sign-off is the only required trailer. Drop CLAUDE.md from
.gitignore so the symlink to AGENTS.md tracks alongside the rest.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* docs: drop legacy agent context symlinks

Current major agents \(Codex, Cursor, Copilot, Windsurf, Gemini CLI,
Amp, Devin\) read AGENTS.md natively. Only Claude Code still requires
CLAUDE.md, kept as a symlink. The rest are removed.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* docs: restore Copilot and Gemini CLI hooks for AGENTS.md

GitHub Copilot supports AGENTS.md only partially; full-featured
repository instructions still flow through .github/copilot-instructions.md.
Gemini CLI reads GEMINI.md by default and requires .gemini/settings.json
to point at AGENTS.md.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* docs: ground AGENTS.md opening in community context

Name the project as multi-vendor open source under llm-d and surface
the shared-review-bandwidth principle that the rules below build on.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* .gitignore: ignore .agents/ local agent workspace

Carve out a default-ignored directory for SDD-style local agent state
\(plans, research notes, memory files\) so contributors don't have to
extend .gitignore per checkout.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* AGENTS.md: add verify-against-code and minimal-change principles

Two cross-cutting working principles: verify behavior against the code
rather than from filenames or familiarity, and prefer the smallest
correct change inside a tracked issue.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* AGENTS.md: name minimalism, split Pull requests into bullets

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* rollback .gitignore

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* add agentic section to gitignore

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* AGENTS.md: add intent-first and checkable-success principles

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* AGENTS.md: add exception, re-plan, mention, and self-check rules

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

---------

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>")  | May 25, 2026  |  
|   |   |  [ci: assemble release notes once per tag instead of per PR merge (](https://github.com/llm-d/llm-d-router/commit/21977e0f57b6dc6ae2c7fe2dd58efd10bc080ad8 "ci: assemble release notes once per tag instead of per PR merge \(#2778\)

Every merged PR with a release note spawned a second bot PR whose only
content was one staged fragment file, doubling review volume for no
reason. Replace the two-workflow fragment pipeline with a required PR
check that validates the release-note block and writes nothing, plus a
tag-time workflow that recovers the PR list from the tag-to-tag commit
range and reads notes live from PR bodies via the API.

Fixes #2746

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")[#2778](https://github.com/llm-d/llm-d-router/pull/2778)[)](https://github.com/llm-d/llm-d-router/commit/21977e0f57b6dc6ae2c7fe2dd58efd10bc080ad8 "ci: assemble release notes once per tag instead of per PR merge \(#2778\)

Every merged PR with a release note spawned a second bot PR whose only
content was one staged fragment file, doubling review volume for no
reason. Replace the two-workflow fragment pipeline with a required PR
check that validates the release-note block and writes nothing, plus a
tag-time workflow that recovers the PR list from the tag-to-tag commit
range and reads notes live from PR bodies via the API.

Fixes #2746

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")  | Sep 11, 2026  |  
|  [feat(flowcontrol): support scoped request queue TTLs (](https://github.com/llm-d/llm-d-router/commit/065811dbde88db66a9093a5300bab2e22360b6a1 "feat\(flowcontrol\): support scoped request queue TTLs \(#2649\)

* feat\(flowcontrol\): support scoped request queue TTLs

Allow operators to select queue-wait bounds per priority band and callers to shorten that bound through the inference TTL header.

Signed-off-by: Nikhil Thomas <nikhil.thomas@mistral.ai>

* fix\(flowcontrol\): expire requests buffered before enqueue

Account for enqueue-channel residence in the active queue-wait budget so expired requests cannot dispatch.

Signed-off-by: Nikhil Thomas <nikhil.thomas@mistral.ai>

* fix\(flowcontrol\) address comments to make locking private to registry

Addresses a concern that accessing a lock for another entity would be error prone.

Signed-off-by: Nikhil Thomas <nikhil.thomas@mistral.ai>

* fix\(flowcontrol\): address scoped TTL review feedback

Signed-off-by: Nikhil Thomas <nikhil.thomas@mistral.ai>

---------

Signed-off-by: Nikhil Thomas <nikhil.thomas@mistral.ai>")[#2649](https://github.com/llm-d/llm-d-router/pull/2649)[)](https://github.com/llm-d/llm-d-router/commit/065811dbde88db66a9093a5300bab2e22360b6a1 "feat\(flowcontrol\): support scoped request queue TTLs \(#2649\)

* feat\(flowcontrol\): support scoped request queue TTLs

Allow operators to select queue-wait bounds per priority band and callers to shorten that bound through the inference TTL header.

Signed-off-by: Nikhil Thomas <nikhil.thomas@mistral.ai>

* fix\(flowcontrol\): expire requests buffered before enqueue

Account for enqueue-channel residence in the active queue-wait budget so expired requests cannot dispatch.

Signed-off-by: Nikhil Thomas <nikhil.thomas@mistral.ai>

* fix\(flowcontrol\) address comments to make locking private to registry

Addresses a concern that accessing a lock for another entity would be error prone.

Signed-off-by: Nikhil Thomas <nikhil.thomas@mistral.ai>

* fix\(flowcontrol\): address scoped TTL review feedback

Signed-off-by: Nikhil Thomas <nikhil.thomas@mistral.ai>

---------

Signed-off-by: Nikhil Thomas <nikhil.thomas@mistral.ai>")  | Sep 9, 2026  |  
|   |   |  [feat(apix): migrate v1alpha2 objective/rewrite defaults to llm-d.ai (](https://github.com/llm-d/llm-d-router/commit/0fb6a7c51e777f7a3c142e99fe823ae7b7151439 "feat\(apix\): migrate v1alpha2 objective/rewrite defaults to llm-d.ai \(#1169\)

* feat\(apix\): migrate v1alpha2 objective/rewrite defaults to llm-d.ai

Signed-off-by: zhouyou9505 <zhouyou9505@gmail.com>

* test: add legacy v1alpha2 group coverage for hermetic objective

Signed-off-by: zhouyou9505 <zhouyou9505@gmail.com>

* chore: gofmt proxy constants

Signed-off-by: zhouyou9505 <zhouyou9505@gmail.com>

* chore: restore legacy pool group comment

Signed-off-by: zhouyou9505 <zhouyou9505@gmail.com>

* test: use local hermetic CRDs and fixtures

Signed-off-by: zhouyou9505 <zhouyou9505@gmail.com>

* fix: default pool ref to inference pool group

Signed-off-by: zhouyou9505 <zhouyou9505@gmail.com>

* Fix e2e llm-d.ai CRD installation

Signed-off-by: zhouyou9505 <zhouyou9505@gmail.com>

* fix: install inferencepool crd in hermetic tests

Signed-off-by: zhouyou9505 <zhouyou9505@gmail.com>

---------

Signed-off-by: zhouyou9505 <zhouyou9505@gmail.com>
Signed-off-by: zhouyou <zhouyou9505@gmail.com>")[#…](https://github.com/llm-d/llm-d-router/pull/1169)  | May 21, 2026  |  
| [refactor(epp): add NewExtProcServerRunner and route every caller thro…](https://github.com/llm-d/llm-d-router/commit/c5175276b1610905620395484627661b46294ca8 "refactor\(epp\): add NewExtProcServerRunner and route every caller through it \(#1370\) \(#2706\)

* test\(epp\): assert the ext_proc runner constructor wires every field

Pins ExtProcServerRunner's construction so a field added to the struct
cannot reach one server while silently staying zero in another. Fails
today with "undefined: server.NewExtProcServerRunner"; the constructor
lands in the next commit.

Refs #1370

Signed-off-by: ChethanUK <chethanuk@outlook.com>

* refactor\(epp\): add NewExtProcServerRunner and route every caller through it

ExtProcServerRunner was built by hand at three sites -- runner.setup,
runWithFileDiscovery some 660 lines away, and NewDefaultExtProcServerRunner.
A composite literal zero-fills what it omits, so a new Options field compiles
while only the site its author edited honours it; the ext_proc servers then
disagree about their own configuration with nothing to flag it.

NewExtProcServerRunner is now the single Options -> runner mapping. All three
sites call it; GrpcListener and EvictChannelLookup stay caller-assigned since
only some sites have them. NewDefaultExtProcServerRunner keeps its behaviour
exactly: every field the factory adds beyond the nine it used to set is zero
under NewOptions\(\).

Refs #1370

Signed-off-by: ChethanUK <chethanuk@outlook.com>

* test\(epp\): pin the same-typed runner fields the zero sweep cannot tell apart

The reflect sweep in TestNewExtProcServerRunnerPopulatesEveryField only proves
each field is non-zero, so two fields of the same type wired to each other's
source still pass. Pin the two durations, add identity assertions for the
remaining dependency parameters, and cover the five booleans with a table test
that sets one option at a time -- distinct values cannot separate bools.

Swapping SecureServing with HealthChecking, or the two metrics durations, in
NewExtProcServerRunner now fails the suite; before this it did not.

Refs #1370

Signed-off-by: ChethanUK <chethanuk@outlook.com>

---------

Signed-off-by: ChethanUK <chethanuk@outlook.com>")  | Sep 13, 2026  |  
|   |   |  [feat(sidecar+coordinator): add support for HTTPS /metrics (](https://github.com/llm-d/llm-d-router/commit/d01182a20c0e247483bbadee87fe4834f5dd567f "feat\(sidecar+coordinator\):  add support for HTTPS  /metrics \(#2752\)

* feat\(sidecar\):  add support for HTTPS for sidecar /metrics

- sidecar metrics currently does not provide HTTPS but only HTTP
- make it close to how its data-plane which support HTTPS, by adding a
  new flag --metrics-cert-path
- only need point to the path where certs are, if not set the flag, use
  HTTP, if set the flag but cannot find either certs file, log an error
  and disable /metrics but sidecar data-plane keep working

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* feat\(coordinator\): support HTTPS metrics

- Allow the coordinator metrics listener to use tls.crt and tls.key

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* chore: rename the flag to make it align with "router"s

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: code review

- reuse helper CreateSelfSignedTLSCertificate\(\) instead of rewrite logic
- update logic on sidecar: if tls enabled but cert file\(S\) invalid, stop sidecar
  including data-plane not just metrics
- add new test and update docs

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")[#2752](https://github.com/llm-d/llm-d-router/pull/2752)[)](https://github.com/llm-d/llm-d-router/commit/d01182a20c0e247483bbadee87fe4834f5dd567f "feat\(sidecar+coordinator\):  add support for HTTPS  /metrics \(#2752\)

* feat\(sidecar\):  add support for HTTPS for sidecar /metrics

- sidecar metrics currently does not provide HTTPS but only HTTP
- make it close to how its data-plane which support HTTPS, by adding a
  new flag --metrics-cert-path
- only need point to the path where certs are, if not set the flag, use
  HTTP, if set the flag but cannot find either certs file, log an error
  and disable /metrics but sidecar data-plane keep working

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* feat\(coordinator\): support HTTPS metrics

- Allow the coordinator metrics listener to use tls.crt and tls.key

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* chore: rename the flag to make it align with "router"s

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: code review

- reuse helper CreateSelfSignedTLSCertificate\(\) instead of rewrite logic
- update logic on sidecar: if tls enabled but cert file\(S\) invalid, stop sidecar
  including data-plane not just metrics
- add new test and update docs

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")  | Sep 13, 2026  |  
|   |   |  [fix: point sim-epp token-producer at the render Service (](https://github.com/llm-d/llm-d-router/commit/7de00e5452818546815417aee2d6c68d2c2ff323 "fix: point sim-epp token-producer at the render Service \(#2426\)

The sim-epp-kvcache EPP config set `token-producer.url` to
http://localhost:8000, but the kind dev env runs the vLLM render server
as a separate Service \(vllm-render:8082\), not a sidecar. The EPP pod has
no listener on localhost:8000, so every request failed tokenization
before the scorer ran and cache-aware routing never engaged.

Use `VLLM_RENDER_URL` \(already exported, defaults to
http://vllm-render:8082\) and add it to `kind-dev-env.sh`'s envsubst var
list so it is substituted into the config.

Signed-off-by: tonibofarull <tonibofarull@gmail.com>")[#2426](https://github.com/llm-d/llm-d-router/pull/2426)[)](https://github.com/llm-d/llm-d-router/commit/7de00e5452818546815417aee2d6c68d2c2ff323 "fix: point sim-epp token-producer at the render Service \(#2426\)

The sim-epp-kvcache EPP config set `token-producer.url` to
http://localhost:8000, but the kind dev env runs the vLLM render server
as a separate Service \(vllm-render:8082\), not a sidecar. The EPP pod has
no listener on localhost:8000, so every request failed tokenization
before the scorer ran and cache-aware routing never engaged.

Use `VLLM_RENDER_URL` \(already exported, defaults to
http://vllm-render:8082\) and add it to `kind-dev-env.sh`'s envsubst var
list so it is substituted into the config.

Signed-off-by: tonibofarull <tonibofarull@gmail.com>")  | Sep 7, 2026  |  
|  [metrics: remove predicted latency legacy metrics (](https://github.com/llm-d/llm-d-router/commit/a8bd70fa2b33dae169fca02d3808f293a2b0e63e "metrics: remove predicted latency legacy metrics \(#2821\)

Signed-off-by: JaredTan95 <jian.tan@daocloud.io>")[#2821](https://github.com/llm-d/llm-d-router/pull/2821)[)](https://github.com/llm-d/llm-d-router/commit/a8bd70fa2b33dae169fca02d3808f293a2b0e63e "metrics: remove predicted latency legacy metrics \(#2821\)

Signed-off-by: JaredTan95 <jian.tan@daocloud.io>")  | Sep 13, 2026  |  
|  [feat(helm): support structured plugin config (](https://github.com/llm-d/llm-d-router/commit/6493998a3735ccf1b07450fe6308c9ec5dd929e1 "feat\(helm\): support structured plugin config \(#2620\)

Signed-off-by: hugosmoreira <hugo.n4@gmail.com>")[#2620](https://github.com/llm-d/llm-d-router/pull/2620)[)](https://github.com/llm-d/llm-d-router/commit/6493998a3735ccf1b07450fe6308c9ec5dd929e1 "feat\(helm\): support structured plugin config \(#2620\)

Signed-off-by: hugosmoreira <hugo.n4@gmail.com>")  | Sep 11, 2026  |  
|   |   | [first commit of pipeline stuff](https://github.com/llm-d/llm-d-router/commit/a5f22ae40edf521a2bbbf74699d9272df71795c7 "first commit of pipeline stuff")  | May 5, 2025  |  
|   |   |  [test: harden port-bind race in EPP integration harness (](https://github.com/llm-d/llm-d-router/commit/fc1842c399d52c2b660ea3dec33e64bbd704beee "test: harden port-bind race in EPP integration harness \(#2081\)

* test: harden port-bind race in EPP integration harness

GetFreePort returns a vacated port that the manager binds a moment later,
so the OS can hand the port to another process in that window and the bind
fails intermittently with 'address already in use' \(issue #1066\).

Wrap the harness bring-up in a bounded retry that re-rolls the free ports
and restarts the manager only on an EADDRINUSE bind failure, resetting
metrics between attempts and failing loud once the budget is exhausted so
a genuine bind bug still surfaces. GetFreePort's signature is unchanged.

Signed-off-by: ChethanUK <chethanuk@outlook.com>

* test: fix fatcontext lint in port-bind retry harness

The per-attempt cancellable context reassigns the outer mgrCtx inside the
retry closure, which fatcontext flags as a nested context. It is intentional
\(each attempt derives a fresh context from the stable parent and cancels the
prior one\), so suppress it with a justified nolint. Verified with golangci-lint
on ./test/integration/... = 0 issues.

Signed-off-by: ChethanUK <chethanuk@outlook.com>

* test: treat clean manager exit after dial as not-ready

WaitExtProcReady's pre-dial select already reports an error when the
manager exits with a nil error, but the post-dial select fell through to
return ready on the same condition. A nil there means the manager returned
\(context cancelled\) and is shutting down, not that the ext-proc server is
ready, so mirror the pre-dial block and fail loud instead of handing back a
dying server.

Signed-off-by: ChethanUK <chethanuk@outlook.com>

* test: stop binding a chosen port for EPP harness metrics and health

The hermetic harness picked three random ports with GetFreePort, which binds,
closes, and returns the number, leaving a window for another process to take
the port before the server binds it. That is the "address already in use"
flake in issue #1066. Only the ext_proc port is ever used by a test.

Let the kernel assign the metrics and health ports instead. A bind on port 0
cannot collide, so two thirds of the race surface goes away without any
production change. The metrics server itself stays up, because the pprof and
plugin-state debug handlers register through it.

This also removes a self-collision: GetFreePort was called three times back to
back, each closing its listener before the next, so two calls could return the
same port. One call now remains.

Refs: #1066
Signed-off-by: ChethanUK <chethanuk@outlook.com>

* test: serve ext_proc on a pre-bound listener in the EPP harness

GetFreePort selects a port by binding, closing, and returning the number; the
server binds it again a moment later. Between the close and the bind the OS can
hand that port to another process, which is the "address already in use" flake
in issue #1066.

Hand the ext_proc server a listener that is already bound instead. Holding the
listener open means the port is never released, so there is no window to lose.
Verified: with a listener held, draining the entire ephemeral port range never
reassigns that port, and a plain bind of it fails with EADDRINUSE.

ExtProcServerRunner gains an optional GrpcListener that takes precedence over
GrpcPort, following manager.Server's Listener field, which documents the same
pattern for the same reason. It defaults to nil, so production keeps binding
from the CLI-supplied port and options.go and the flags are untouched. The
health runnable also stays on the port path, which keeps the net.Listen branch
under test.

Refs: #1066
Signed-off-by: ChethanUK <chethanuk@outlook.com>

* test: drop the port-bind retry and gate readiness on gRPC state

A held listener cannot lose a port race, so RetryOnAddrInUse and isAddrInUse
have nothing left to retry. Removing them returns the harness to straight-line
startup: no per-attempt manager rebuild and no matching on the text of the
aggregated manager error.

The dial-based readiness check has to go with them, for a subtler reason. The
listener is bound before the server starts, so the kernel completes the
handshake out of the listen backlog and a dial succeeds while grpc.Serve has
not run yet. Waiting for the client connection to reach connectivity.Ready
requires the HTTP/2 handshake, which only completes once the server is
serving, and it still selects on the manager's exit so a start failure fails
fast instead of hanging.

Both claims are gated by tests rather than asserted:
TestReserveListenerDefendsPort holds a reservation, draws 2000 ephemeral
ports and shows none of them is the reserved one, and shows an explicit bind
of it gets EADDRINUSE; TestPreBoundListenerQueuesDials dials before Serve and
shows the request is still served.

Refs: #1066
Signed-off-by: ChethanUK <chethanuk@outlook.com>

---------

Signed-off-by: ChethanUK <chethanuk@outlook.com>")[#2081](https://github.com/llm-d/llm-d-router/pull/2081)[)](https://github.com/llm-d/llm-d-router/commit/fc1842c399d52c2b660ea3dec33e64bbd704beee "test: harden port-bind race in EPP integration harness \(#2081\)

* test: harden port-bind race in EPP integration harness

GetFreePort returns a vacated port that the manager binds a moment later,
so the OS can hand the port to another process in that window and the bind
fails intermittently with 'address already in use' \(issue #1066\).

Wrap the harness bring-up in a bounded retry that re-rolls the free ports
and restarts the manager only on an EADDRINUSE bind failure, resetting
metrics between attempts and failing loud once the budget is exhausted so
a genuine bind bug still surfaces. GetFreePort's signature is unchanged.

Signed-off-by: ChethanUK <chethanuk@outlook.com>

* test: fix fatcontext lint in port-bind retry harness

The per-attempt cancellable context reassigns the outer mgrCtx inside the
retry closure, which fatcontext flags as a nested context. It is intentional
\(each attempt derives a fresh context from the stable parent and cancels the
prior one\), so suppress it with a justified nolint. Verified with golangci-lint
on ./test/integration/... = 0 issues.

Signed-off-by: ChethanUK <chethanuk@outlook.com>

* test: treat clean manager exit after dial as not-ready

WaitExtProcReady's pre-dial select already reports an error when the
manager exits with a nil error, but the post-dial select fell through to
return ready on the same condition. A nil there means the manager returned
\(context cancelled\) and is shutting down, not that the ext-proc server is
ready, so mirror the pre-dial block and fail loud instead of handing back a
dying server.

Signed-off-by: ChethanUK <chethanuk@outlook.com>

* test: stop binding a chosen port for EPP harness metrics and health

The hermetic harness picked three random ports with GetFreePort, which binds,
closes, and returns the number, leaving a window for another process to take
the port before the server binds it. That is the "address already in use"
flake in issue #1066. Only the ext_proc port is ever used by a test.

Let the kernel assign the metrics and health ports instead. A bind on port 0
cannot collide, so two thirds of the race surface goes away without any
production change. The metrics server itself stays up, because the pprof and
plugin-state debug handlers register through it.

This also removes a self-collision: GetFreePort was called three times back to
back, each closing its listener before the next, so two calls could return the
same port. One call now remains.

Refs: #1066
Signed-off-by: ChethanUK <chethanuk@outlook.com>

* test: serve ext_proc on a pre-bound listener in the EPP harness

GetFreePort selects a port by binding, closing, and returning the number; the
server binds it again a moment later. Between the close and the bind the OS can
hand that port to another process, which is the "address already in use" flake
in issue #1066.

Hand the ext_proc server a listener that is already bound instead. Holding the
listener open means the port is never released, so there is no window to lose.
Verified: with a listener held, draining the entire ephemeral port range never
reassigns that port, and a plain bind of it fails with EADDRINUSE.

ExtProcServerRunner gains an optional GrpcListener that takes precedence over
GrpcPort, following manager.Server's Listener field, which documents the same
pattern for the same reason. It defaults to nil, so production keeps binding
from the CLI-supplied port and options.go and the flags are untouched. The
health runnable also stays on the port path, which keeps the net.Listen branch
under test.

Refs: #1066
Signed-off-by: ChethanUK <chethanuk@outlook.com>

* test: drop the port-bind retry and gate readiness on gRPC state

A held listener cannot lose a port race, so RetryOnAddrInUse and isAddrInUse
have nothing left to retry. Removing them returns the harness to straight-line
startup: no per-attempt manager rebuild and no matching on the text of the
aggregated manager error.

The dial-based readiness check has to go with them, for a subtler reason. The
listener is bound before the server starts, so the kernel completes the
handshake out of the listen backlog and a dial succeeds while grpc.Serve has
not run yet. Waiting for the client connection to reach connectivity.Ready
requires the HTTP/2 handshake, which only completes once the server is
serving, and it still selects on the manager's exit so a start failure fails
fast instead of hanging.

Both claims are gated by tests rather than asserted:
TestReserveListenerDefendsPort holds a reservation, draws 2000 ephemeral
ports and shows none of them is the reserved one, and shows an explicit bind
of it gets EADDRINUSE; TestPreBoundListenerQueuesDials dials before Serve and
shows the request is still served.

Refs: #1066
Signed-off-by: ChethanUK <chethanuk@outlook.com>

---------

Signed-off-by: ChethanUK <chethanuk@outlook.com>")  | Jul 22, 2026  |  
|  [refactor: unify API type handling across sidecar and coordinator (](https://github.com/llm-d/llm-d-router/commit/d5ff9100312b4a39d7ba050638e149d37ff46b4a "refactor: unify API type handling across sidecar and coordinator \(#2743\)

* refactor: unify API type handling across sidecar and coordinator

Signed-off-by: Revital Sur <eres@il.ibm.com>

* refactor: address simplify review findings

Let CapSingleToken own sampling_params in the generate prefill body, derive
the sidecar route table from DetectAPIType, inline the single-caller
capMaxTokensField, and merge the duplicated APIType test tables.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* fix\(request\): cap max_output_tokens on Responses prefill legs

CapSingleToken named each API's cap fields in an if-chain while
APIType.TokenLimitFields already listed them as data, and the two
disagreed: a /v1/responses leg was capped on max_tokens, which that API
ignores, and left max_output_tokens as the client sent it, so the leg ran
a full generation. Drive the caps from TokenLimitFields.

min_tokens is a floor rather than a cap and is still stripped for every
API, so chat completions, completions and generate are unchanged.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* refactor\(sidecar\): cap the NIXLv2 prefill leg on a copy

NIXLv2 capped the client's own body for the prefill leg and restored every
field afterwards for the decode leg, the only connector doing so. Copy the
body one level and cap the copy, as the shared-storage, mooncake and p2p
connectors already do, so the shared CapSingleToken helper covers all five.

The concurrent WRITE-mode path gains the apiType it previously ignored,
which had it capping chat-completions fields on generate and Responses
bodies. min_tokens is now stripped from the prefill leg rather than set to
1, matching every other connector.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* refactor\(sidecar\): copy request bodies with maps.Clone

Five sites hand-rolled the same one-level map copy with make plus a range
loop. Use maps.Clone, which the same package already uses elsewhere.

buildEncoderRequest documented its copy as deep when it is one level, which
matters because nested values stay shared with the client's body.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* fix\(sidecar\): refuse request bodies that are not JSON objects

A JSON "null" body unmarshals without error into a nil map. Connectors
clone the parsed body and write the transfer params into the clone, and a
write to a nil map panics. Reject a non-object body in the parser, where
every connector sees it, and route the remaining connectors through
readJSONBody so the check cannot be skipped.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* test\(request\): cover a malformed sampling_params on the generate cap

The sidecar caps a leg straight off the parsed client body, with no
equivalent of the coordinator's validateSamplingParams ahead of it, so a
non-object or null sampling_params reaches TokenLimitMap. Both land on
the synthesize branch and the field is replaced; pin that.

State the one-level-copy guarantee in the CapSingleToken doc comment,
where callers that clone before capping read it, instead of inside the
function body.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Add tests and simplify.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Default unrecognized API types to chat completions

The sidecar treated both the zero value and an unrecognized APIType as
chat completions, while the shared type fell back to generate. Use the
sidecar's default everywhere. LookupAPIType reports whether a path is
known, so the coordinator render and format steps still skip or
collapse to generate for paths the router does not register.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Clean up API type comments and tighten min_tokens prefill test

Flatten resolveFormat and state the chat completions fallback once in
apitype.go. Document why resolveFormat falls back to generate. Fix a
dupword lint error in render_test.go.

CapSingleToken always strips min_tokens, so assert that the prefill leg
has no min_tokens instead of guarding a check that can never run.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Remove LookupAPIType and use DetectAPIType in coordinator steps

The coordinator router sends only the chat completions, completions,
and generate routes to the steps, so the known flag guarded a path that
production never takes. An unrecognized path now resolves as chat
completions in the steps too. TestPrefillStep_GatewayError relied on
the empty path falling back to generate, so it sets the generate path.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* State token-limit invariants once and reference them elsewhere

The sampling_params sharing rule and the min_tokens stripping rule were
each explained in several helper and test comments. Keep the reasoning
in tokenLimitMap and CapSingleToken, and make the other comments name
what they test and point to those docs.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments on API types and path constants

Give /v1/messages its own APIType so its prefill leg caps only
max_tokens, the one output cap the Anthropic Messages API defines.

Use the path constants in pkg/common/request directly in the sidecar
and the coordinator instead of keeping aliases, and inline
tokenLimitMap into CapSingleToken, its only caller.

Make the render step skip explicit with a default case, classify the
encode generate skip with DetectAPIType so a prefixed path matches,
log the reason readJSONBody refuses a body at DEBUG, and document why
the SGLang connector ignores the API type.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments on the EC encoder request

Document why buildEncoderRequest caps the encoder request as chat
completions: it carries the item in messages and is always sent to
/v1/chat/completions, whatever API the client used.

Keep the buildEncoderRequest tests next to the code they cover in
connector_ec_common_test.go, and name request fields there with the
reqcommon.Field constants.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Use "request" instead of "leg" in comments and test names

Address review comments: the prefill, decode, and encoder requests are
described as requests, not legs, in the code this change adds.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments.

Signed-off-by: Revital Sur <eres@il.ibm.com>

---------

Signed-off-by: Revital Sur <eres@il.ibm.com>")[#2743](https://github.com/llm-d/llm-d-router/pull/2743)[)](https://github.com/llm-d/llm-d-router/commit/d5ff9100312b4a39d7ba050638e149d37ff46b4a "refactor: unify API type handling across sidecar and coordinator \(#2743\)

* refactor: unify API type handling across sidecar and coordinator

Signed-off-by: Revital Sur <eres@il.ibm.com>

* refactor: address simplify review findings

Let CapSingleToken own sampling_params in the generate prefill body, derive
the sidecar route table from DetectAPIType, inline the single-caller
capMaxTokensField, and merge the duplicated APIType test tables.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* fix\(request\): cap max_output_tokens on Responses prefill legs

CapSingleToken named each API's cap fields in an if-chain while
APIType.TokenLimitFields already listed them as data, and the two
disagreed: a /v1/responses leg was capped on max_tokens, which that API
ignores, and left max_output_tokens as the client sent it, so the leg ran
a full generation. Drive the caps from TokenLimitFields.

min_tokens is a floor rather than a cap and is still stripped for every
API, so chat completions, completions and generate are unchanged.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* refactor\(sidecar\): cap the NIXLv2 prefill leg on a copy

NIXLv2 capped the client's own body for the prefill leg and restored every
field afterwards for the decode leg, the only connector doing so. Copy the
body one level and cap the copy, as the shared-storage, mooncake and p2p
connectors already do, so the shared CapSingleToken helper covers all five.

The concurrent WRITE-mode path gains the apiType it previously ignored,
which had it capping chat-completions fields on generate and Responses
bodies. min_tokens is now stripped from the prefill leg rather than set to
1, matching every other connector.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* refactor\(sidecar\): copy request bodies with maps.Clone

Five sites hand-rolled the same one-level map copy with make plus a range
loop. Use maps.Clone, which the same package already uses elsewhere.

buildEncoderRequest documented its copy as deep when it is one level, which
matters because nested values stay shared with the client's body.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* fix\(sidecar\): refuse request bodies that are not JSON objects

A JSON "null" body unmarshals without error into a nil map. Connectors
clone the parsed body and write the transfer params into the clone, and a
write to a nil map panics. Reject a non-object body in the parser, where
every connector sees it, and route the remaining connectors through
readJSONBody so the check cannot be skipped.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* test\(request\): cover a malformed sampling_params on the generate cap

The sidecar caps a leg straight off the parsed client body, with no
equivalent of the coordinator's validateSamplingParams ahead of it, so a
non-object or null sampling_params reaches TokenLimitMap. Both land on
the synthesize branch and the field is replaced; pin that.

State the one-level-copy guarantee in the CapSingleToken doc comment,
where callers that clone before capping read it, instead of inside the
function body.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Add tests and simplify.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Default unrecognized API types to chat completions

The sidecar treated both the zero value and an unrecognized APIType as
chat completions, while the shared type fell back to generate. Use the
sidecar's default everywhere. LookupAPIType reports whether a path is
known, so the coordinator render and format steps still skip or
collapse to generate for paths the router does not register.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Clean up API type comments and tighten min_tokens prefill test

Flatten resolveFormat and state the chat completions fallback once in
apitype.go. Document why resolveFormat falls back to generate. Fix a
dupword lint error in render_test.go.

CapSingleToken always strips min_tokens, so assert that the prefill leg
has no min_tokens instead of guarding a check that can never run.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Remove LookupAPIType and use DetectAPIType in coordinator steps

The coordinator router sends only the chat completions, completions,
and generate routes to the steps, so the known flag guarded a path that
production never takes. An unrecognized path now resolves as chat
completions in the steps too. TestPrefillStep_GatewayError relied on
the empty path falling back to generate, so it sets the generate path.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* State token-limit invariants once and reference them elsewhere

The sampling_params sharing rule and the min_tokens stripping rule were
each explained in several helper and test comments. Keep the reasoning
in tokenLimitMap and CapSingleToken, and make the other comments name
what they test and point to those docs.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments on API types and path constants

Give /v1/messages its own APIType so its prefill leg caps only
max_tokens, the one output cap the Anthropic Messages API defines.

Use the path constants in pkg/common/request directly in the sidecar
and the coordinator instead of keeping aliases, and inline
tokenLimitMap into CapSingleToken, its only caller.

Make the render step skip explicit with a default case, classify the
encode generate skip with DetectAPIType so a prefixed path matches,
log the reason readJSONBody refuses a body at DEBUG, and document why
the SGLang connector ignores the API type.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments on the EC encoder request

Document why buildEncoderRequest caps the encoder request as chat
completions: it carries the item in messages and is always sent to
/v1/chat/completions, whatever API the client used.

Keep the buildEncoderRequest tests next to the code they cover in
connector_ec_common_test.go, and name request fields there with the
reqcommon.Field constants.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Use "request" instead of "leg" in comments and test names

Address review comments: the prefill, decode, and encoder requests are
described as requests, not legs, in the code this change adds.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments.

Signed-off-by: Revital Sur <eres@il.ibm.com>

---------

Signed-off-by: Revital Sur <eres@il.ibm.com>")  | Sep 14, 2026  |  
| [release-notes.d/unreleased](https://github.com/llm-d/llm-d-router/tree/main/release-notes.d/unreleased "This path skips through empty directories")  | [release-notes.d/unreleased](https://github.com/llm-d/llm-d-router/tree/main/release-notes.d/unreleased "This path skips through empty directories")  |  [docs: add release-note fragment for PR](https://github.com/llm-d/llm-d-router/commit/e268ef9e390fbb1341b9ff0a609981bbc27f681b "docs: add release-note fragment for PR #2763 \(#2814\)

Signed-off-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>
Co-authored-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>") [#2763](https://github.com/llm-d/llm-d-router/pull/2763) [(](https://github.com/llm-d/llm-d-router/commit/e268ef9e390fbb1341b9ff0a609981bbc27f681b "docs: add release-note fragment for PR #2763 \(#2814\)

Signed-off-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>
Co-authored-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>")[#2814](https://github.com/llm-d/llm-d-router/pull/2814)[)](https://github.com/llm-d/llm-d-router/commit/e268ef9e390fbb1341b9ff0a609981bbc27f681b "docs: add release-note fragment for PR #2763 \(#2814\)

Signed-off-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>
Co-authored-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>")  | Sep 13, 2026  |  
|   |   |  [fix: point sim-epp token-producer at the render Service (](https://github.com/llm-d/llm-d-router/commit/7de00e5452818546815417aee2d6c68d2c2ff323 "fix: point sim-epp token-producer at the render Service \(#2426\)

The sim-epp-kvcache EPP config set `token-producer.url` to
http://localhost:8000, but the kind dev env runs the vLLM render server
as a separate Service \(vllm-render:8082\), not a sidecar. The EPP pod has
no listener on localhost:8000, so every request failed tokenization
before the scorer ran and cache-aware routing never engaged.

Use `VLLM_RENDER_URL` \(already exported, defaults to
http://vllm-render:8082\) and add it to `kind-dev-env.sh`'s envsubst var
list so it is substituted into the config.

Signed-off-by: tonibofarull <tonibofarull@gmail.com>")[#2426](https://github.com/llm-d/llm-d-router/pull/2426)[)](https://github.com/llm-d/llm-d-router/commit/7de00e5452818546815417aee2d6c68d2c2ff323 "fix: point sim-epp token-producer at the render Service \(#2426\)

The sim-epp-kvcache EPP config set `token-producer.url` to
http://localhost:8000, but the kind dev env runs the vLLM render server
as a separate Service \(vllm-render:8082\), not a sidecar. The EPP pod has
no listener on localhost:8000, so every request failed tokenization
before the scorer ran and cache-aware routing never engaged.

Use `VLLM_RENDER_URL` \(already exported, defaults to
http://vllm-render:8082\) and add it to `kind-dev-env.sh`'s envsubst var
list so it is substituted into the config.

Signed-off-by: tonibofarull <tonibofarull@gmail.com>")  | Sep 7, 2026  |  
|  [refactor: unify API type handling across sidecar and coordinator (](https://github.com/llm-d/llm-d-router/commit/d5ff9100312b4a39d7ba050638e149d37ff46b4a "refactor: unify API type handling across sidecar and coordinator \(#2743\)

* refactor: unify API type handling across sidecar and coordinator

Signed-off-by: Revital Sur <eres@il.ibm.com>

* refactor: address simplify review findings

Let CapSingleToken own sampling_params in the generate prefill body, derive
the sidecar route table from DetectAPIType, inline the single-caller
capMaxTokensField, and merge the duplicated APIType test tables.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* fix\(request\): cap max_output_tokens on Responses prefill legs

CapSingleToken named each API's cap fields in an if-chain while
APIType.TokenLimitFields already listed them as data, and the two
disagreed: a /v1/responses leg was capped on max_tokens, which that API
ignores, and left max_output_tokens as the client sent it, so the leg ran
a full generation. Drive the caps from TokenLimitFields.

min_tokens is a floor rather than a cap and is still stripped for every
API, so chat completions, completions and generate are unchanged.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* refactor\(sidecar\): cap the NIXLv2 prefill leg on a copy

NIXLv2 capped the client's own body for the prefill leg and restored every
field afterwards for the decode leg, the only connector doing so. Copy the
body one level and cap the copy, as the shared-storage, mooncake and p2p
connectors already do, so the shared CapSingleToken helper covers all five.

The concurrent WRITE-mode path gains the apiType it previously ignored,
which had it capping chat-completions fields on generate and Responses
bodies. min_tokens is now stripped from the prefill leg rather than set to
1, matching every other connector.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* refactor\(sidecar\): copy request bodies with maps.Clone

Five sites hand-rolled the same one-level map copy with make plus a range
loop. Use maps.Clone, which the same package already uses elsewhere.

buildEncoderRequest documented its copy as deep when it is one level, which
matters because nested values stay shared with the client's body.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* fix\(sidecar\): refuse request bodies that are not JSON objects

A JSON "null" body unmarshals without error into a nil map. Connectors
clone the parsed body and write the transfer params into the clone, and a
write to a nil map panics. Reject a non-object body in the parser, where
every connector sees it, and route the remaining connectors through
readJSONBody so the check cannot be skipped.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* test\(request\): cover a malformed sampling_params on the generate cap

The sidecar caps a leg straight off the parsed client body, with no
equivalent of the coordinator's validateSamplingParams ahead of it, so a
non-object or null sampling_params reaches TokenLimitMap. Both land on
the synthesize branch and the field is replaced; pin that.

State the one-level-copy guarantee in the CapSingleToken doc comment,
where callers that clone before capping read it, instead of inside the
function body.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Add tests and simplify.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Default unrecognized API types to chat completions

The sidecar treated both the zero value and an unrecognized APIType as
chat completions, while the shared type fell back to generate. Use the
sidecar's default everywhere. LookupAPIType reports whether a path is
known, so the coordinator render and format steps still skip or
collapse to generate for paths the router does not register.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Clean up API type comments and tighten min_tokens prefill test

Flatten resolveFormat and state the chat completions fallback once in
apitype.go. Document why resolveFormat falls back to generate. Fix a
dupword lint error in render_test.go.

CapSingleToken always strips min_tokens, so assert that the prefill leg
has no min_tokens instead of guarding a check that can never run.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Remove LookupAPIType and use DetectAPIType in coordinator steps

The coordinator router sends only the chat completions, completions,
and generate routes to the steps, so the known flag guarded a path that
production never takes. An unrecognized path now resolves as chat
completions in the steps too. TestPrefillStep_GatewayError relied on
the empty path falling back to generate, so it sets the generate path.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* State token-limit invariants once and reference them elsewhere

The sampling_params sharing rule and the min_tokens stripping rule were
each explained in several helper and test comments. Keep the reasoning
in tokenLimitMap and CapSingleToken, and make the other comments name
what they test and point to those docs.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments on API types and path constants

Give /v1/messages its own APIType so its prefill leg caps only
max_tokens, the one output cap the Anthropic Messages API defines.

Use the path constants in pkg/common/request directly in the sidecar
and the coordinator instead of keeping aliases, and inline
tokenLimitMap into CapSingleToken, its only caller.

Make the render step skip explicit with a default case, classify the
encode generate skip with DetectAPIType so a prefixed path matches,
log the reason readJSONBody refuses a body at DEBUG, and document why
the SGLang connector ignores the API type.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments on the EC encoder request

Document why buildEncoderRequest caps the encoder request as chat
completions: it carries the item in messages and is always sent to
/v1/chat/completions, whatever API the client used.

Keep the buildEncoderRequest tests next to the code they cover in
connector_ec_common_test.go, and name request fields there with the
reqcommon.Field constants.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Use "request" instead of "leg" in comments and test names

Address review comments: the prefill, decode, and encoder requests are
described as requests, not legs, in the code this change adds.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments.

Signed-off-by: Revital Sur <eres@il.ibm.com>

---------

Signed-off-by: Revital Sur <eres@il.ibm.com>")[#2743](https://github.com/llm-d/llm-d-router/pull/2743)[)](https://github.com/llm-d/llm-d-router/commit/d5ff9100312b4a39d7ba050638e149d37ff46b4a "refactor: unify API type handling across sidecar and coordinator \(#2743\)

* refactor: unify API type handling across sidecar and coordinator

Signed-off-by: Revital Sur <eres@il.ibm.com>

* refactor: address simplify review findings

Let CapSingleToken own sampling_params in the generate prefill body, derive
the sidecar route table from DetectAPIType, inline the single-caller
capMaxTokensField, and merge the duplicated APIType test tables.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* fix\(request\): cap max_output_tokens on Responses prefill legs

CapSingleToken named each API's cap fields in an if-chain while
APIType.TokenLimitFields already listed them as data, and the two
disagreed: a /v1/responses leg was capped on max_tokens, which that API
ignores, and left max_output_tokens as the client sent it, so the leg ran
a full generation. Drive the caps from TokenLimitFields.

min_tokens is a floor rather than a cap and is still stripped for every
API, so chat completions, completions and generate are unchanged.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* refactor\(sidecar\): cap the NIXLv2 prefill leg on a copy

NIXLv2 capped the client's own body for the prefill leg and restored every
field afterwards for the decode leg, the only connector doing so. Copy the
body one level and cap the copy, as the shared-storage, mooncake and p2p
connectors already do, so the shared CapSingleToken helper covers all five.

The concurrent WRITE-mode path gains the apiType it previously ignored,
which had it capping chat-completions fields on generate and Responses
bodies. min_tokens is now stripped from the prefill leg rather than set to
1, matching every other connector.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* refactor\(sidecar\): copy request bodies with maps.Clone

Five sites hand-rolled the same one-level map copy with make plus a range
loop. Use maps.Clone, which the same package already uses elsewhere.

buildEncoderRequest documented its copy as deep when it is one level, which
matters because nested values stay shared with the client's body.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* fix\(sidecar\): refuse request bodies that are not JSON objects

A JSON "null" body unmarshals without error into a nil map. Connectors
clone the parsed body and write the transfer params into the clone, and a
write to a nil map panics. Reject a non-object body in the parser, where
every connector sees it, and route the remaining connectors through
readJSONBody so the check cannot be skipped.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* test\(request\): cover a malformed sampling_params on the generate cap

The sidecar caps a leg straight off the parsed client body, with no
equivalent of the coordinator's validateSamplingParams ahead of it, so a
non-object or null sampling_params reaches TokenLimitMap. Both land on
the synthesize branch and the field is replaced; pin that.

State the one-level-copy guarantee in the CapSingleToken doc comment,
where callers that clone before capping read it, instead of inside the
function body.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Add tests and simplify.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Default unrecognized API types to chat completions

The sidecar treated both the zero value and an unrecognized APIType as
chat completions, while the shared type fell back to generate. Use the
sidecar's default everywhere. LookupAPIType reports whether a path is
known, so the coordinator render and format steps still skip or
collapse to generate for paths the router does not register.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Clean up API type comments and tighten min_tokens prefill test

Flatten resolveFormat and state the chat completions fallback once in
apitype.go. Document why resolveFormat falls back to generate. Fix a
dupword lint error in render_test.go.

CapSingleToken always strips min_tokens, so assert that the prefill leg
has no min_tokens instead of guarding a check that can never run.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Remove LookupAPIType and use DetectAPIType in coordinator steps

The coordinator router sends only the chat completions, completions,
and generate routes to the steps, so the known flag guarded a path that
production never takes. An unrecognized path now resolves as chat
completions in the steps too. TestPrefillStep_GatewayError relied on
the empty path falling back to generate, so it sets the generate path.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* State token-limit invariants once and reference them elsewhere

The sampling_params sharing rule and the min_tokens stripping rule were
each explained in several helper and test comments. Keep the reasoning
in tokenLimitMap and CapSingleToken, and make the other comments name
what they test and point to those docs.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments on API types and path constants

Give /v1/messages its own APIType so its prefill leg caps only
max_tokens, the one output cap the Anthropic Messages API defines.

Use the path constants in pkg/common/request directly in the sidecar
and the coordinator instead of keeping aliases, and inline
tokenLimitMap into CapSingleToken, its only caller.

Make the render step skip explicit with a default case, classify the
encode generate skip with DetectAPIType so a prefixed path matches,
log the reason readJSONBody refuses a body at DEBUG, and document why
the SGLang connector ignores the API type.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments on the EC encoder request

Document why buildEncoderRequest caps the encoder request as chat
completions: it carries the item in messages and is always sent to
/v1/chat/completions, whatever API the client used.

Keep the buildEncoderRequest tests next to the code they cover in
connector_ec_common_test.go, and name request fields there with the
reqcommon.Field constants.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Use "request" instead of "leg" in comments and test names

Address review comments: the prefill, decode, and encoder requests are
described as requests, not legs, in the code this change adds.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments.

Signed-off-by: Revital Sur <eres@il.ibm.com>

---------

Signed-off-by: Revital Sur <eres@il.ibm.com>")  | Sep 14, 2026  |  
|   |   |  [fix: wrong bundle version in artifacts CRDs (](https://github.com/llm-d/llm-d-router/commit/e2e725317883c2e0f25c4747aeeaedd45e2c6ade "fix: wrong bundle version in artifacts CRDs \(#1742\)

- 0.9.0 has main-dev set in annoataion llm-d.ai/bundle-version

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")[#1742](https://github.com/llm-d/llm-d-router/pull/1742)[)](https://github.com/llm-d/llm-d-router/commit/e2e725317883c2e0f25c4747aeeaedd45e2c6ade "fix: wrong bundle version in artifacts CRDs \(#1742\)

- 0.9.0 has main-dev set in annoataion llm-d.ai/bundle-version

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")  | Jun 27, 2026  |  
|   |   |  [Switch to pre-built vLLM wheels for CPU builds (](https://github.com/llm-d/llm-d-router/commit/3476cafa7136c13c528bb111e1b218d556b3558c "Switch to pre-built vLLM wheels for CPU builds \(#582\)

* try use official vllm wheels in dockerfile.epp

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* wip

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* use wheels in makefile

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* wip

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* write permissions to setup.sh

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* update kv cache manager commit

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* try instal py deps wo sudo

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* CR changes

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

---------

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>")[#582](https://github.com/llm-d/llm-d-router/pull/582)[)](https://github.com/llm-d/llm-d-router/commit/3476cafa7136c13c528bb111e1b218d556b3558c "Switch to pre-built vLLM wheels for CPU builds \(#582\)

* try use official vllm wheels in dockerfile.epp

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* wip

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* use wheels in makefile

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* wip

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* write permissions to setup.sh

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* update kv cache manager commit

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* try instal py deps wo sudo

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

* CR changes

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>

---------

Signed-off-by: Sage Ahrac <sagiahrak@gmail.com>")  | Jan 22, 2026  |  
|   |   |  [[CI][Security]: Run gosec via golangci-lint in the lint job (](https://github.com/llm-d/llm-d-router/commit/ecad7dfa963b706d6074e522c84e2c9ae026ac5f "\[CI\]\[Security\]: Run gosec via golangci-lint in the lint job \(#2203\)

* ci: Add security workflow with Gosec, Gitleaks and CodeQL scanning

Nothing in CI currently scans our the source. govulncheck covers known
CVEs in dependencies and Trivy covers image layers but both look at
code we pull in rather than code we write.

Gosec finds insecure Go patterns, gitleaks finds secrets committed to
history and CodeQL finds dataflow bugs that pattern matching misses.
All three publish SARIF so findings land in the Security tab.

Gosec and gitleaks are advisory for now so the baseline can be triaged
before anything blocks a merge. CodeQL runs weekly rather than per PR
because it is slow.

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

* ci: Pin codeql action to SHA and keep scan SARIF as artifact

Fork PRs run with a read only token, so the SARIF upload step is
skipped and the reports were being thrown away. gosec writes only to
the report file and logs nothing useful which left those runs with no
way to see findings at all. Keep both reports as artifacts instead.

The codeql action refs already had the resolved SHA sitting in a
comment but still used a mutable tag. Pin to the SHA so a retargeted
tag can't change what runs.

Copilot review comments:

- https://github.com/llm-d/llm-d-router/pull/2203#pullrequestreview-4787563816

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

* Make checks consistent in security workflow

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

* Run gosec through golangci-lint instead of a standalone job

The securego/gosec container ships GOTOOLCHAIN=local with a Go older than
go.mod requires, so package loading failed and every scan reported nothing.

Running it through golangci-lint in the builder container gets the right
toolchain. It also picks up the 6 //nolint:gosec directives already in the
tree which standalone gosec ignores and it can gate fork PRs since
failing a lint step needs no write token.

Advisory for now. SECURITY_LINT_EXIT_CODE defaults to 0 so the 50 finding
baseline can be triaged before this blocks merges.

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

* Remove the security workflow

Following discussions in https://github.com/llm-d/llm-d-router/pull/2203#issuecomment-5116486236
it is decided to srip the security workflow as goign to use
native CodeQL and Secret scanning.

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

---------

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>")[#2203](https://github.com/llm-d/llm-d-router/pull/2203)[)](https://github.com/llm-d/llm-d-router/commit/ecad7dfa963b706d6074e522c84e2c9ae026ac5f "\[CI\]\[Security\]: Run gosec via golangci-lint in the lint job \(#2203\)

* ci: Add security workflow with Gosec, Gitleaks and CodeQL scanning

Nothing in CI currently scans our the source. govulncheck covers known
CVEs in dependencies and Trivy covers image layers but both look at
code we pull in rather than code we write.

Gosec finds insecure Go patterns, gitleaks finds secrets committed to
history and CodeQL finds dataflow bugs that pattern matching misses.
All three publish SARIF so findings land in the Security tab.

Gosec and gitleaks are advisory for now so the baseline can be triaged
before anything blocks a merge. CodeQL runs weekly rather than per PR
because it is slow.

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

* ci: Pin codeql action to SHA and keep scan SARIF as artifact

Fork PRs run with a read only token, so the SARIF upload step is
skipped and the reports were being thrown away. gosec writes only to
the report file and logs nothing useful which left those runs with no
way to see findings at all. Keep both reports as artifacts instead.

The codeql action refs already had the resolved SHA sitting in a
comment but still used a mutable tag. Pin to the SHA so a retargeted
tag can't change what runs.

Copilot review comments:

- https://github.com/llm-d/llm-d-router/pull/2203#pullrequestreview-4787563816

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

* Make checks consistent in security workflow

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

* Run gosec through golangci-lint instead of a standalone job

The securego/gosec container ships GOTOOLCHAIN=local with a Go older than
go.mod requires, so package loading failed and every scan reported nothing.

Running it through golangci-lint in the builder container gets the right
toolchain. It also picks up the 6 //nolint:gosec directives already in the
tree which standalone gosec ignores and it can gate fork PRs since
failing a lint step needs no write token.

Advisory for now. SECURITY_LINT_EXIT_CODE defaults to 0 so the 50 finding
baseline can be triaged before this blocks merges.

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

* Remove the security workflow

Following discussions in https://github.com/llm-d/llm-d-router/pull/2203#issuecomment-5116486236
it is decided to srip the security workflow as goign to use
native CodeQL and Secret scanning.

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

---------

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>")  | Aug 3, 2026  |  
|   |   |  [[CI][Security]: Run gosec via golangci-lint in the lint job (](https://github.com/llm-d/llm-d-router/commit/ecad7dfa963b706d6074e522c84e2c9ae026ac5f "\[CI\]\[Security\]: Run gosec via golangci-lint in the lint job \(#2203\)

* ci: Add security workflow with Gosec, Gitleaks and CodeQL scanning

Nothing in CI currently scans our the source. govulncheck covers known
CVEs in dependencies and Trivy covers image layers but both look at
code we pull in rather than code we write.

Gosec finds insecure Go patterns, gitleaks finds secrets committed to
history and CodeQL finds dataflow bugs that pattern matching misses.
All three publish SARIF so findings land in the Security tab.

Gosec and gitleaks are advisory for now so the baseline can be triaged
before anything blocks a merge. CodeQL runs weekly rather than per PR
because it is slow.

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

* ci: Pin codeql action to SHA and keep scan SARIF as artifact

Fork PRs run with a read only token, so the SARIF upload step is
skipped and the reports were being thrown away. gosec writes only to
the report file and logs nothing useful which left those runs with no
way to see findings at all. Keep both reports as artifacts instead.

The codeql action refs already had the resolved SHA sitting in a
comment but still used a mutable tag. Pin to the SHA so a retargeted
tag can't change what runs.

Copilot review comments:

- https://github.com/llm-d/llm-d-router/pull/2203#pullrequestreview-4787563816

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

* Make checks consistent in security workflow

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

* Run gosec through golangci-lint instead of a standalone job

The securego/gosec container ships GOTOOLCHAIN=local with a Go older than
go.mod requires, so package loading failed and every scan reported nothing.

Running it through golangci-lint in the builder container gets the right
toolchain. It also picks up the 6 //nolint:gosec directives already in the
tree which standalone gosec ignores and it can gate fork PRs since
failing a lint step needs no write token.

Advisory for now. SECURITY_LINT_EXIT_CODE defaults to 0 so the 50 finding
baseline can be triaged before this blocks merges.

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

* Remove the security workflow

Following discussions in https://github.com/llm-d/llm-d-router/pull/2203#issuecomment-5116486236
it is decided to srip the security workflow as goign to use
native CodeQL and Secret scanning.

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

---------

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>")[#2203](https://github.com/llm-d/llm-d-router/pull/2203)[)](https://github.com/llm-d/llm-d-router/commit/ecad7dfa963b706d6074e522c84e2c9ae026ac5f "\[CI\]\[Security\]: Run gosec via golangci-lint in the lint job \(#2203\)

* ci: Add security workflow with Gosec, Gitleaks and CodeQL scanning

Nothing in CI currently scans our the source. govulncheck covers known
CVEs in dependencies and Trivy covers image layers but both look at
code we pull in rather than code we write.

Gosec finds insecure Go patterns, gitleaks finds secrets committed to
history and CodeQL finds dataflow bugs that pattern matching misses.
All three publish SARIF so findings land in the Security tab.

Gosec and gitleaks are advisory for now so the baseline can be triaged
before anything blocks a merge. CodeQL runs weekly rather than per PR
because it is slow.

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

* ci: Pin codeql action to SHA and keep scan SARIF as artifact

Fork PRs run with a read only token, so the SARIF upload step is
skipped and the reports were being thrown away. gosec writes only to
the report file and logs nothing useful which left those runs with no
way to see findings at all. Keep both reports as artifacts instead.

The codeql action refs already had the resolved SHA sitting in a
comment but still used a mutable tag. Pin to the SHA so a retargeted
tag can't change what runs.

Copilot review comments:

- https://github.com/llm-d/llm-d-router/pull/2203#pullrequestreview-4787563816

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

* Make checks consistent in security workflow

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

* Run gosec through golangci-lint instead of a standalone job

The securego/gosec container ships GOTOOLCHAIN=local with a Go older than
go.mod requires, so package loading failed and every scan reported nothing.

Running it through golangci-lint in the builder container gets the right
toolchain. It also picks up the 6 //nolint:gosec directives already in the
tree which standalone gosec ignores and it can gate fork PRs since
failing a lint step needs no write token.

Advisory for now. SECURITY_LINT_EXIT_CODE defaults to 0 so the 50 finding
baseline can be triaged before this blocks merges.

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

* Remove the security workflow

Following discussions in https://github.com/llm-d/llm-d-router/pull/2203#issuecomment-5116486236
it is decided to srip the security workflow as goign to use
native CodeQL and Secret scanning.

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>

---------

Signed-off-by: Martin Hickey <martin.hickey@ie.ibm.com>")  | Aug 3, 2026  |  
|   |   |  [chore(lint): enable redundant-import-alias, drop self-aliased imports (](https://github.com/llm-d/llm-d-router/commit/149fc81c61d5baadc1f77d02eb956c178d4fb234 "chore\(lint\): enable redundant-import-alias, drop self-aliased imports \(#2760\)

Several recent PRs introduced import aliases identical to the
package's own name, adding no disambiguation. Enable revive's
redundant-import-alias to catch this going forward, and remove the
20 existing instances that goimports doesn't fight.

Version-suffixed packages \(v1, v1alpha1, ...\) keep their alias:
goimports always re-adds it since its assumed name skips the version
segment, so those are excluded to avoid a permanent make format/make
lint disagreement.

Signed-off-by: roytman <roytman@il.ibm.com>")[…](https://github.com/llm-d/llm-d-router/pull/2760)  | Sep 9, 2026  |  
|   |   |  [fix(markdown): correct broken links flagged by lychee (](https://github.com/llm-d/llm-d-router/commit/d7ef54f17b8805f285625c49ccd68c04327c3770 "fix\(markdown\): correct broken links flagged by lychee \(#2776\)

The lychee link checker was effectively dormant since it was added
in PR #130: the workflow's 'if:' guard referenced a non-existent
job name \(fixed in #1914\) and the '**/*.md' glob was unquoted so the
shell only expanded top-level files \(fixed in #2754\). Once recursive
checking actually started, three long-broken links surfaced.

- gpu/README.md: drop one '../' in the dcgm-extractor link so it
  resolves into the datalayer/ tree where the file lives.
- README.md: use repo-relative 'LICENSE' instead of root-relative
  '/LICENSE' so lychee can resolve it without --root-dir.
- .lychee.toml: consolidate exclude_path entries here \(release-notes.d
  was previously in the workflow arg\) and skip the symlinked
  .github/copilot-instructions.md, which is a duplicate of AGENTS.md
  that GitHub dereferences for display but lychee does not.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")[#2776](https://github.com/llm-d/llm-d-router/pull/2776)[)](https://github.com/llm-d/llm-d-router/commit/d7ef54f17b8805f285625c49ccd68c04327c3770 "fix\(markdown\): correct broken links flagged by lychee \(#2776\)

The lychee link checker was effectively dormant since it was added
in PR #130: the workflow's 'if:' guard referenced a non-existent
job name \(fixed in #1914\) and the '**/*.md' glob was unquoted so the
shell only expanded top-level files \(fixed in #2754\). Once recursive
checking actually started, three long-broken links surfaced.

- gpu/README.md: drop one '../' in the dcgm-extractor link so it
  resolves into the datalayer/ tree where the file lives.
- README.md: use repo-relative 'LICENSE' instead of root-relative
  '/LICENSE' so lychee can resolve it without --root-dir.
- .lychee.toml: consolidate exclude_path entries here \(release-notes.d
  was previously in the workflow arg\) and skip the symlinked
  .github/copilot-instructions.md, which is a duplicate of AGENTS.md
  that GitHub dereferences for display but lychee does not.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")  | Sep 9, 2026  |  
|   |   |  [ci: assemble release notes once per tag instead of per PR merge (](https://github.com/llm-d/llm-d-router/commit/21977e0f57b6dc6ae2c7fe2dd58efd10bc080ad8 "ci: assemble release notes once per tag instead of per PR merge \(#2778\)

Every merged PR with a release note spawned a second bot PR whose only
content was one staged fragment file, doubling review volume for no
reason. Replace the two-workflow fragment pipeline with a required PR
check that validates the release-note block and writes nothing, plus a
tag-time workflow that recovers the PR list from the tag-to-tag commit
range and reads notes live from PR bodies via the API.

Fixes #2746

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")[#2778](https://github.com/llm-d/llm-d-router/pull/2778)[)](https://github.com/llm-d/llm-d-router/commit/21977e0f57b6dc6ae2c7fe2dd58efd10bc080ad8 "ci: assemble release notes once per tag instead of per PR merge \(#2778\)

Every merged PR with a release note spawned a second bot PR whose only
content was one staged fragment file, doubling review volume for no
reason. Replace the two-workflow fragment pipeline with a required PR
check that validates the release-note block and writes nothing, plus a
tag-time workflow that recovers the PR list from the tag-to-tag commit
range and reads notes live from PR bodies via the API.

Fixes #2746

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")  | Sep 11, 2026  |  
|   |   |  [ci: assemble release notes once per tag instead of per PR merge (](https://github.com/llm-d/llm-d-router/commit/21977e0f57b6dc6ae2c7fe2dd58efd10bc080ad8 "ci: assemble release notes once per tag instead of per PR merge \(#2778\)

Every merged PR with a release note spawned a second bot PR whose only
content was one staged fragment file, doubling review volume for no
reason. Replace the two-workflow fragment pipeline with a required PR
check that validates the release-note block and writes nothing, plus a
tag-time workflow that recovers the PR list from the tag-to-tag commit
range and reads notes live from PR bodies via the API.

Fixes #2746

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")[#2778](https://github.com/llm-d/llm-d-router/pull/2778)[)](https://github.com/llm-d/llm-d-router/commit/21977e0f57b6dc6ae2c7fe2dd58efd10bc080ad8 "ci: assemble release notes once per tag instead of per PR merge \(#2778\)

Every merged PR with a release note spawned a second bot PR whose only
content was one staged fragment file, doubling review volume for no
reason. Replace the two-workflow fragment pipeline with a required PR
check that validates the release-note block and writes nothing, plus a
tag-time workflow that recovers the PR list from the tag-to-tag commit
range and reads notes live from PR bodies via the API.

Fixes #2746

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")  | Sep 11, 2026  |  
|   |   |  [feat(docs): agentic context engineering (](https://github.com/llm-d/llm-d-router/commit/5fc77aa7d2e243970247fad1dd3c7c9172f17da6 "feat\(docs\): agentic context engineering \(#1295\)

* docs: add AGENTS.md and symlinks for common agent harnesses

Single source of truth at AGENTS.md covering code style, git workflow,
and agent operating rules. Symlinks point legacy tool-specific paths
\(.cursorrules, .cursor/rules/main.mdc, .windsurfrules, GEMINI.md,
.github/copilot-instructions.md\) at the same file so one source covers
agents that don't yet read AGENTS.md natively.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* docs: forbid AI co-author trailers; track CLAUDE.md symlink

DCO sign-off is the only required trailer. Drop CLAUDE.md from
.gitignore so the symlink to AGENTS.md tracks alongside the rest.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* docs: drop legacy agent context symlinks

Current major agents \(Codex, Cursor, Copilot, Windsurf, Gemini CLI,
Amp, Devin\) read AGENTS.md natively. Only Claude Code still requires
CLAUDE.md, kept as a symlink. The rest are removed.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* docs: restore Copilot and Gemini CLI hooks for AGENTS.md

GitHub Copilot supports AGENTS.md only partially; full-featured
repository instructions still flow through .github/copilot-instructions.md.
Gemini CLI reads GEMINI.md by default and requires .gemini/settings.json
to point at AGENTS.md.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* docs: ground AGENTS.md opening in community context

Name the project as multi-vendor open source under llm-d and surface
the shared-review-bandwidth principle that the rules below build on.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* .gitignore: ignore .agents/ local agent workspace

Carve out a default-ignored directory for SDD-style local agent state
\(plans, research notes, memory files\) so contributors don't have to
extend .gitignore per checkout.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* AGENTS.md: add verify-against-code and minimal-change principles

Two cross-cutting working principles: verify behavior against the code
rather than from filenames or familiarity, and prefer the smallest
correct change inside a tracked issue.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* AGENTS.md: name minimalism, split Pull requests into bullets

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* rollback .gitignore

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* add agentic section to gitignore

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* AGENTS.md: add intent-first and checkable-success principles

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* AGENTS.md: add exception, re-plan, mention, and self-check rules

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

---------

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>")[#1295](https://github.com/llm-d/llm-d-router/pull/1295)[)](https://github.com/llm-d/llm-d-router/commit/5fc77aa7d2e243970247fad1dd3c7c9172f17da6 "feat\(docs\): agentic context engineering \(#1295\)

* docs: add AGENTS.md and symlinks for common agent harnesses

Single source of truth at AGENTS.md covering code style, git workflow,
and agent operating rules. Symlinks point legacy tool-specific paths
\(.cursorrules, .cursor/rules/main.mdc, .windsurfrules, GEMINI.md,
.github/copilot-instructions.md\) at the same file so one source covers
agents that don't yet read AGENTS.md natively.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* docs: forbid AI co-author trailers; track CLAUDE.md symlink

DCO sign-off is the only required trailer. Drop CLAUDE.md from
.gitignore so the symlink to AGENTS.md tracks alongside the rest.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* docs: drop legacy agent context symlinks

Current major agents \(Codex, Cursor, Copilot, Windsurf, Gemini CLI,
Amp, Devin\) read AGENTS.md natively. Only Claude Code still requires
CLAUDE.md, kept as a symlink. The rest are removed.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* docs: restore Copilot and Gemini CLI hooks for AGENTS.md

GitHub Copilot supports AGENTS.md only partially; full-featured
repository instructions still flow through .github/copilot-instructions.md.
Gemini CLI reads GEMINI.md by default and requires .gemini/settings.json
to point at AGENTS.md.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* docs: ground AGENTS.md opening in community context

Name the project as multi-vendor open source under llm-d and surface
the shared-review-bandwidth principle that the rules below build on.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* .gitignore: ignore .agents/ local agent workspace

Carve out a default-ignored directory for SDD-style local agent state
\(plans, research notes, memory files\) so contributors don't have to
extend .gitignore per checkout.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* AGENTS.md: add verify-against-code and minimal-change principles

Two cross-cutting working principles: verify behavior against the code
rather than from filenames or familiarity, and prefer the smallest
correct change inside a tracked issue.

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* AGENTS.md: name minimalism, split Pull requests into bullets

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* rollback .gitignore

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* add agentic section to gitignore

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* AGENTS.md: add intent-first and checkable-success principles

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

* AGENTS.md: add exception, re-plan, mention, and self-check rules

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>

---------

Signed-off-by: Maroon Ayoub <maroon.ayoub@ibm.com>")  | May 25, 2026  |  
|   |   |  [Add Nili as P2P routing code owner (](https://github.com/llm-d/llm-d-router/commit/31217b912e2df0956afcf3e4275af9160ececed4 "Add Nili as P2P routing code owner \(#2614\)

Signed-off-by: Maroon Ayoub <mayoub@redhat.com>")[#2614](https://github.com/llm-d/llm-d-router/pull/2614)[)](https://github.com/llm-d/llm-d-router/commit/31217b912e2df0956afcf3e4275af9160ececed4 "Add Nili as P2P routing code owner \(#2614\)

Signed-off-by: Maroon Ayoub <mayoub@redhat.com>")  | Aug 31, 2026  |  
|   |   |  [docs: add a repository contributing guide covering issue claiming (](https://github.com/llm-d/llm-d-router/commit/d81a9b6fd0ab872900965a1ef39a1bb1710b8846 "docs: add a repository contributing guide covering issue claiming \(#2666\)

* docs: add a repository contributing guide covering issue claiming

llm-d-router is the only llm-d component repository without a CONTRIBUTING.md,
and since Prow was removed in #2114 there is no self-assignment command here.
GitHub's API only permits assigning users with triage access or prior
involvement, so a non-member has no documented way to claim an issue.

Add a short guide that defers to the organization contributing guide for
project-wide process and documents only what differs in this repository: that
commenting is what marks an issue as taken, and that sibling repositories still
running Prow behave differently. Link it from the README contributing section.

Fixes #2212

Signed-off-by: Mario Mesas <mmesasro@gmail.com>

* docs: apply review feedback on the contributing guide

Simplify the pointer to the organization guide and move the Prow note into a
callout, per review.

Signed-off-by: Mario Mesas <mmesasro@gmail.com>

---------

Signed-off-by: Mario Mesas <mmesasro@gmail.com>")[#2666](https://github.com/llm-d/llm-d-router/pull/2666)  | Sep 4, 2026  |  
|   |   |  [ci: assemble release notes once per tag instead of per PR merge (](https://github.com/llm-d/llm-d-router/commit/21977e0f57b6dc6ae2c7fe2dd58efd10bc080ad8 "ci: assemble release notes once per tag instead of per PR merge \(#2778\)

Every merged PR with a release note spawned a second bot PR whose only
content was one staged fragment file, doubling review volume for no
reason. Replace the two-workflow fragment pipeline with a required PR
check that validates the release-note block and writes nothing, plus a
tag-time workflow that recovers the PR list from the tag-to-tag commit
range and reads notes live from PR bodies via the API.

Fixes #2746

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")[#2778](https://github.com/llm-d/llm-d-router/pull/2778)[)](https://github.com/llm-d/llm-d-router/commit/21977e0f57b6dc6ae2c7fe2dd58efd10bc080ad8 "ci: assemble release notes once per tag instead of per PR merge \(#2778\)

Every merged PR with a release note spawned a second bot PR whose only
content was one staged fragment file, doubling review volume for no
reason. Replace the two-workflow fragment pipeline with a required PR
check that validates the release-note block and writes nothing, plus a
tag-time workflow that recovers the PR list from the tag-to-tag commit
range and reads notes live from PR bodies via the API.

Fixes #2746

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")  | Sep 11, 2026  |  
|   |   |  [deps(docker): bump golang from 1.26.7 to 1.26.8 (](https://github.com/llm-d/llm-d-router/commit/238d7a14ede399ed58b6e246c040a3c740627447 "deps\(docker\): bump golang from 1.26.7 to 1.26.8 \(#2734\)

Bumps golang from 1.26.7 to 1.26.8.

---
updated-dependencies:
- dependency-name: golang
  dependency-version: 1.26.8
  dependency-type: direct:production
  update-type: version-update:semver-patch
...

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>")[#2734](https://github.com/llm-d/llm-d-router/pull/2734)[)](https://github.com/llm-d/llm-d-router/commit/238d7a14ede399ed58b6e246c040a3c740627447 "deps\(docker\): bump golang from 1.26.7 to 1.26.8 \(#2734\)

Bumps golang from 1.26.7 to 1.26.8.

---
updated-dependencies:
- dependency-name: golang
  dependency-version: 1.26.8
  dependency-type: direct:production
  update-type: version-update:semver-patch
...

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>")  | Sep 8, 2026  |  
|   |   |  [deps(docker): bump golang from 1.26.7 to 1.26.8 (](https://github.com/llm-d/llm-d-router/commit/238d7a14ede399ed58b6e246c040a3c740627447 "deps\(docker\): bump golang from 1.26.7 to 1.26.8 \(#2734\)

Bumps golang from 1.26.7 to 1.26.8.

---
updated-dependencies:
- dependency-name: golang
  dependency-version: 1.26.8
  dependency-type: direct:production
  update-type: version-update:semver-patch
...

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>")[#2734](https://github.com/llm-d/llm-d-router/pull/2734)[)](https://github.com/llm-d/llm-d-router/commit/238d7a14ede399ed58b6e246c040a3c740627447 "deps\(docker\): bump golang from 1.26.7 to 1.26.8 \(#2734\)

Bumps golang from 1.26.7 to 1.26.8.

---
updated-dependencies:
- dependency-name: golang
  dependency-version: 1.26.8
  dependency-type: direct:production
  update-type: version-update:semver-patch
...

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>")  | Sep 8, 2026  |  
|   |   |  [deps(docker): bump golang from 1.26.7 to 1.26.8 (](https://github.com/llm-d/llm-d-router/commit/238d7a14ede399ed58b6e246c040a3c740627447 "deps\(docker\): bump golang from 1.26.7 to 1.26.8 \(#2734\)

Bumps golang from 1.26.7 to 1.26.8.

---
updated-dependencies:
- dependency-name: golang
  dependency-version: 1.26.8
  dependency-type: direct:production
  update-type: version-update:semver-patch
...

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>")[#2734](https://github.com/llm-d/llm-d-router/pull/2734)[)](https://github.com/llm-d/llm-d-router/commit/238d7a14ede399ed58b6e246c040a3c740627447 "deps\(docker\): bump golang from 1.26.7 to 1.26.8 \(#2734\)

Bumps golang from 1.26.7 to 1.26.8.

---
updated-dependencies:
- dependency-name: golang
  dependency-version: 1.26.8
  dependency-type: direct:production
  update-type: version-update:semver-patch
...

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>")  | Sep 8, 2026  |  
|   |   |  [deps(docker): bump golang from 1.26.7 to 1.26.8 (](https://github.com/llm-d/llm-d-router/commit/238d7a14ede399ed58b6e246c040a3c740627447 "deps\(docker\): bump golang from 1.26.7 to 1.26.8 \(#2734\)

Bumps golang from 1.26.7 to 1.26.8.

---
updated-dependencies:
- dependency-name: golang
  dependency-version: 1.26.8
  dependency-type: direct:production
  update-type: version-update:semver-patch
...

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>")[#2734](https://github.com/llm-d/llm-d-router/pull/2734)[)](https://github.com/llm-d/llm-d-router/commit/238d7a14ede399ed58b6e246c040a3c740627447 "deps\(docker\): bump golang from 1.26.7 to 1.26.8 \(#2734\)

Bumps golang from 1.26.7 to 1.26.8.

---
updated-dependencies:
- dependency-name: golang
  dependency-version: 1.26.8
  dependency-type: direct:production
  update-type: version-update:semver-patch
...

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>")  | Sep 8, 2026  |  
|   |   |  [docs: move Shmuel Kallner to emeritus, drop from CODEOWNERS (](https://github.com/llm-d/llm-d-router/commit/95f099c83e523df6c1f051ea6b9b94c096d10fea "docs: move Shmuel Kallner to emeritus, drop from CODEOWNERS \(#2449\)

Shmuel Kallner has retired from active maintenance. List him under
Emeritus in LEADS.md and remove him as a named code owner for
flowcontrol, envoy, config, sidecar, and coordinator paths; those
paths retain their other named owners or fall back to
@llm-d/router-maintainers.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")[#2449](https://github.com/llm-d/llm-d-router/pull/2449)[)](https://github.com/llm-d/llm-d-router/commit/95f099c83e523df6c1f051ea6b9b94c096d10fea "docs: move Shmuel Kallner to emeritus, drop from CODEOWNERS \(#2449\)

Shmuel Kallner has retired from active maintenance. List him under
Emeritus in LEADS.md and remove him as a named code owner for
flowcontrol, envoy, config, sidecar, and coordinator paths; those
paths retain their other named owners or fall back to
@llm-d/router-maintainers.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")  | Aug 19, 2026  |  
|   |   |   | May 5, 2025  |  
|   |   | [Raise e2e EXISTS_TIMEOUT to 90s in CI to avoid namespace teardown fla…](https://github.com/llm-d/llm-d-router/commit/debfe671674513b51ad670990f529efd962024d5 "Raise e2e EXISTS_TIMEOUT to 90s in CI to avoid namespace teardown flakes \(#2582\)

* Raise e2e EXISTS_TIMEOUT to 90s in CI to avoid namespace teardown flakes

Signed-off-by: bob tian <zetxqx@gmail.com>

* retrigger CI

Signed-off-by: bob tian <zetxqx@gmail.com>

---------

Signed-off-by: bob tian <zetxqx@gmail.com>")  | Aug 27, 2026  |  
|   |   |  [update: change after rename git repo to "llm-d-router" (](https://github.com/llm-d/llm-d-router/commit/ef239d7aab9c997ac0e570e108b1e6c22b51b17d "update: change after rename git repo to "llm-d-router" \(#1105\)

* update: change after rename git repo to "llm-d-router"

- update PROJECT_NAME to reflect new value
- introduce a new env variable in Makefile for epp image name
- remove unused env in envsubst
- update migration script to point to new repo name
- update docs

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: code review

- we do need the epp image for openshift deployment

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")[#1105](https://github.com/llm-d/llm-d-router/pull/1105)[)](https://github.com/llm-d/llm-d-router/commit/ef239d7aab9c997ac0e570e108b1e6c22b51b17d "update: change after rename git repo to "llm-d-router" \(#1105\)

* update: change after rename git repo to "llm-d-router"

- update PROJECT_NAME to reflect new value
- introduce a new env variable in Makefile for epp image name
- remove unused env in envsubst
- update migration script to point to new repo name
- update docs

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: code review

- we do need the epp image for openshift deployment

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")  | May 14, 2026  |  
|   |   | [test(coordinator): rewire e2e onto shared inference-gateway and add 3…](https://github.com/llm-d/llm-d-router/commit/6c172ba0573b4784afe2f9aff32ddb74b0b8a3a0 "test\(coordinator\): rewire e2e onto shared inference-gateway and add 3-EPP topology \(#2253\)

* test\(coordinator\): rewire e2e onto shared inference-gateway component

Signed-off-by: Revital Sur <eres@il.ibm.com>

* test\(coordinator\): add 3-EPP topology option to e2e suite

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Extract common resources.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Simplify

Signed-off-by: Revital Sur <eres@il.ibm.com>

* test\(coordinator\): substitute VLLM_RENDER_URL for e2e workers

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Minor change.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Fix test.

Signed-off-by: Revital Sur <eres@il.ibm.com>

---------

Signed-off-by: Revital Sur <eres@il.ibm.com>")  | Aug 3, 2026  |  
|   |   |  [update: pull GIE CRD from upstream instead of locally (](https://github.com/llm-d/llm-d-router/commit/edd31ede61156b94991858a4bcc39b660788cb36 "update: pull GIE CRD from upstream instead of locally \(#1043\)

* update: use local config/crd for CRD than pull from remote GIE repo

- make generate fetch CRD to local config/crd with version set in go.mod
- point reference to local config/crd
- remove deploy/components/crds-gie to keep only one place if need bump
  version later

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: missing kustomization.yaml

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: code review

- uncheckout two upstream CRD
- add new make target to get version set in go.mod and  update
  deploy and hack

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: still need apply upstream GIE CRDs

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: comment typo and rename stale gieCrdsKustomize variable

- Fix 'config/crd/base' -> 'config/crd/bases' typo in hack script comments
- Rename gieCrdsKustomize to crdKustomizePath in e2e test since it now
  covers both upstream GIE and local llm-d.ai CRDs

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>
Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: update test to local all from local crd folder

- since we do not check in 2 CRD from upstream GIE into local

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: code review

- use GIE release artifacts to get CRD
- update make target for version replacement
- update e2e test accordingly as we wont get 2 CRD from upstream any
  longer.

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix\(test\): update rbac.yaml after CRDs only comes from llm-d.ai not upstream GIE

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>
Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>")[#1043](https://github.com/llm-d/llm-d-router/pull/1043)[)](https://github.com/llm-d/llm-d-router/commit/edd31ede61156b94991858a4bcc39b660788cb36 "update: pull GIE CRD from upstream instead of locally \(#1043\)

* update: use local config/crd for CRD than pull from remote GIE repo

- make generate fetch CRD to local config/crd with version set in go.mod
- point reference to local config/crd
- remove deploy/components/crds-gie to keep only one place if need bump
  version later

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: missing kustomization.yaml

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: code review

- uncheckout two upstream CRD
- add new make target to get version set in go.mod and  update
  deploy and hack

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: still need apply upstream GIE CRDs

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: comment typo and rename stale gieCrdsKustomize variable

- Fix 'config/crd/base' -> 'config/crd/bases' typo in hack script comments
- Rename gieCrdsKustomize to crdKustomizePath in e2e test since it now
  covers both upstream GIE and local llm-d.ai CRDs

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>
Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix: update test to local all from local crd folder

- since we do not check in 2 CRD from upstream GIE into local

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* update: code review

- use GIE release artifacts to get CRD
- update make target for version replacement
- update e2e test accordingly as we wont get 2 CRD from upstream any
  longer.

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

* fix\(test\): update rbac.yaml after CRDs only comes from llm-d.ai not upstream GIE

Signed-off-by: Wen Zhou <wenzhou@redhat.com>

---------

Signed-off-by: Wen Zhou <wenzhou@redhat.com>
Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>")  | May 25, 2026  |  
|   |   |  [fix(kind): forward Prometheus host port (](https://github.com/llm-d/llm-d-router/commit/9c4f31820bd89d38d64002b3f858da9dbd30f218 "fix\(kind\): forward Prometheus host port \(#2654\)

* fix\(kind\): forward Prometheus host port

Signed-off-by: Alex <jihui.huang@daocloud.io>

* fix\(kind\): define Prometheus host port default

Signed-off-by: Alex <jihui.huang@daocloud.io>

---------

Signed-off-by: Alex <jihui.huang@daocloud.io>")[#2654](https://github.com/llm-d/llm-d-router/pull/2654)[)](https://github.com/llm-d/llm-d-router/commit/9c4f31820bd89d38d64002b3f858da9dbd30f218 "fix\(kind\): forward Prometheus host port \(#2654\)

* fix\(kind\): forward Prometheus host port

Signed-off-by: Alex <jihui.huang@daocloud.io>

* fix\(kind\): define Prometheus host port default

Signed-off-by: Alex <jihui.huang@daocloud.io>

---------

Signed-off-by: Alex <jihui.huang@daocloud.io>")  | Sep 4, 2026  |  
|   |   |  [Add check-all-tools target to verify environment setup (](https://github.com/llm-d/llm-d-router/commit/a21c39db6b0c5d03bb62c036edb64c87db77ffbd "Add check-all-tools target to verify environment setup \(#1469\)

Signed-off-by: Omer Aplatony <omerap12@gmail.com>")[#1469](https://github.com/llm-d/llm-d-router/pull/1469)[)](https://github.com/llm-d/llm-d-router/commit/a21c39db6b0c5d03bb62c036edb64c87db77ffbd "Add check-all-tools target to verify environment setup \(#1469\)

Signed-off-by: Omer Aplatony <omerap12@gmail.com>")  | Jun 4, 2026  |  
|   |   | [test(coordinator): rewire e2e onto shared inference-gateway and add 3…](https://github.com/llm-d/llm-d-router/commit/6c172ba0573b4784afe2f9aff32ddb74b0b8a3a0 "test\(coordinator\): rewire e2e onto shared inference-gateway and add 3-EPP topology \(#2253\)

* test\(coordinator\): rewire e2e onto shared inference-gateway component

Signed-off-by: Revital Sur <eres@il.ibm.com>

* test\(coordinator\): add 3-EPP topology option to e2e suite

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Extract common resources.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Simplify

Signed-off-by: Revital Sur <eres@il.ibm.com>

* test\(coordinator\): substitute VLLM_RENDER_URL for e2e workers

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Minor change.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Fix test.

Signed-off-by: Revital Sur <eres@il.ibm.com>

---------

Signed-off-by: Revital Sur <eres@il.ibm.com>")  | Aug 3, 2026  |  
|   |   |  [fix(docs): repair broken links surfaced by quoted lychee glob (](https://github.com/llm-d/llm-d-router/commit/92a5e828a31097bb4e3fa29a0986180baa82a68a "fix\(docs\): repair broken links surfaced by quoted lychee glob \(#2764\)

Signed-off-by: Iceber Gu <caiwei95@hotmail.com>")[#2764](https://github.com/llm-d/llm-d-router/pull/2764)[)](https://github.com/llm-d/llm-d-router/commit/92a5e828a31097bb4e3fa29a0986180baa82a68a "fix\(docs\): repair broken links surfaced by quoted lychee glob \(#2764\)

Signed-off-by: Iceber Gu <caiwei95@hotmail.com>")  | Sep 9, 2026  |  
|   |   |  [docs: assemble release notes for v0.10.0 (](https://github.com/llm-d/llm-d-router/commit/af44b38662ea9c16189c2b08fe5faea44029892a "docs: assemble release notes for v0.10.0 \(#2424\)

Signed-off-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>
Co-authored-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>")[#2424](https://github.com/llm-d/llm-d-router/pull/2424)[)](https://github.com/llm-d/llm-d-router/commit/af44b38662ea9c16189c2b08fe5faea44029892a "docs: assemble release notes for v0.10.0 \(#2424\)

Signed-off-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>
Co-authored-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>")  | Aug 19, 2026  |  
|   |   |  [docs: add SECURITY.md with vulnerability reporting policy (](https://github.com/llm-d/llm-d-router/commit/730f880412196a79a13c750c1ac435659143c5a9 "docs: add SECURITY.md with vulnerability reporting policy \(#2189\)

Signed-off-by: arijitroy003 <arijitroy003@gmail.com>")[#2189](https://github.com/llm-d/llm-d-router/pull/2189)[)](https://github.com/llm-d/llm-d-router/commit/730f880412196a79a13c750c1ac435659143c5a9 "docs: add SECURITY.md with vulnerability reporting policy \(#2189\)

Signed-off-by: arijitroy003 <arijitroy003@gmail.com>")  | Jul 28, 2026  |  
|   |   | [feat(coordinator): async-broker step serving OpenAI compatible enqueu…](https://github.com/llm-d/llm-d-router/commit/2b105ffecb08a3afad801c6b4b26f47bbb5b1257 "feat\(coordinator\): async-broker step serving OpenAI compatible enqueue, wait, and passthrough modes \(#2325\)

* feat\(coordinator\): let pipeline steps register auxiliary HTTP routes

Steps that implement RegisterRoutes\(chi.Router\) get it called once at
server construction, after the built-in inference routes. This gives a
step with its own HTTP surface \(result retrieval, model listing\) a home
on the coordinator listener without the server learning step specifics.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* feat\(coordinator\): add async-broker step bridging llm-d-async

Optional first pipeline step giving standard OpenAI clients access to
request level queueing through the gateway. Clients opt in per request
with a mode header: passthrough classifies against tenant quota and
stamps objective and fairness headers, enqueue writes to the broker
queue and answers 202 with the request id, wait holds the connection
until the result lands. The step also serves GET and DELETE
/v1/requests/{id} and GET /v1/models, with a reference doc and release
note.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* feat\(coordinator\): configurable deadlines, wait caps, and post-fetch grace

Per mode timeout bounds resolve field by field with 60s wait and 1h
enqueue defaults, max_seconds clamps client requested deadlines, and
holds that end at the deadline answer 504 DEADLINE_EXCEEDED.
wait_cap_seconds ends a hold early with the 202 response, leaving the
request fetchable. fetch_grace_seconds sets the mailbox TTL applied
after a delivered fetch, and zero deletes the result on delivery. The
doc's timeouts table is rewritten around clock spans.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* feat\(coordinator\): tenant scope AP-side keys and default the quota attribute

The envelope id is now tenant:id, so the in-flight marker and
cancellation key the AP derives from it are tenant scoped like the
mailbox, and a fetch with the wrong tenant cannot learn that an id
exists. The client-visible id is unchanged. The quota attribute
defaults to userid to match the AP's redis-quota gate.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* test\(coordinator\): stop echoing the URL param in the aux route stub

gosec flags writing a request-derived value straight to the response as
G705 XSS. The stub only needs to prove the path param reached the
handler, so capture it on the struct and assert there instead.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* docs\(coordinator\): state the async broker trust model and routing preconditions

The tenant header is trusted as asserted, so drop the doc wording that
implied tenant scoping alone prevents cross tenant probing. A deployment
note now states that the request id is the only secret protecting a
stored result and that clients wanting an unguessable handle should use
the minted UUID. A second note covers gateway routing: stock llm-d only
forwards the inference paths to the coordinator, so the fetch and models
routes need adding to the HTTPRoute. Also corrects the RouteRegistrar
comment, chi silently keeps the last handler for a duplicate pattern
rather than panicking.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* fix\(coordinator\): harden async broker error paths and validation

Review follow-ups from #2325:

- Guard the stored result status code range before WriteHeader, a
  corrupted mailbox value now answers 502 instead of panicking into the
  recovery middleware
- Re-read the mailbox once when the active marker is missing, closing
  the window where a result landing between the two reads produced a
  spurious 410 for a completed request
- Stop writing raw Redis errors into client responses on fetch, delete,
  and enqueue failures, log them server side instead
- Log quota release failures \(the counter stays elevated until the
  window TTL\) and surface result lookup errors during held waits
- Use logger.Error for recoverable Redis failures across the step
- Apply the enqueue path's tenant and id validation to the fetch and
  delete routes
- Reject negative wait_cap_seconds, timeout bounds, quota window, and
  quota limits instead of silently reinterpreting them
- Reject pipeline configs that place async-broker anywhere but first
- Narrow the retry comment in the wait disconnect path, SubmitRequest
  clears markers but does not deduplicate a still queued original

Adds tests for cross tenant DELETE, the malformed status guard, read
path validation, quota fail open on Redis error, the negative config
values, and builder ordering.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* feat\(coordinator\): cap wait deadlines at 1h and cancel on DELETE

Wait mode holds a live connection per request, so an unclamped client
requested deadline could pin one for days. timeouts.wait.max_seconds
now defaults to 3600, raised to default_seconds when that is configured
higher. Enqueue stays unclamped.

DELETE /v1/requests/{id} now cancels a still queued request before
reclaiming the mailbox, so the AP drops it pre-dispatch instead of
running it and recreating the key the delete just removed. The cancel
is a no-op for ids with no active marker.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* docs\(coordinator\): state the wait deadline cap once

The clamp default belongs in the timeouts and TTLs table, not repeated
in the config param table.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* fix\(coordinator\): support concurrent waits on one request id

Two wait holds sharing a tenant and id \(legal via retry by id\) used to
overwrite each other's wake channel, and the first to finish
unsubscribed the notification channel the survivor still needed,
silently degrading it to the backup poll. The waiter now keeps a wake
list per channel, subscribes on the first waiter, and unsubscribes on
the last, with the removal and unsubscribe under one lock so a
concurrent registration re-subscribes in order.

Covers the notify wake-up path with tests: the waiter multiplexing and
refcounted unsubscribe directly, and a full wait hold in notify mode
completing well under the backup poll interval. miniredis cannot emit
keyspace notifications, so the tests publish the notification an LPUSH
would fire.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* docs\(coordinator\): state each async broker fact once

Review follow-up on duplication: the no-header no-op, the timeout
defaults, and the fetch grace default were each restated across the
doc, the config example, struct comments, and method comments. Each now
has one home. Behavior facts live in the doc prose and the Execute
comment, defaults live in the doc's param table and the config struct
field comments the doc points at, and method comments describe only
what their method adds.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* fix\(coordinator\): report mailbox presence with a bool

The nilnil linter rejects readMailbox returning a nil result with a nil
error for an empty mailbox, so it now returns an explicit found flag.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* docs\(coordinator\): state the id reuse contract for retries

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* docs\(coordinator\): broaden the id note to the retry contract

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* feat\(coordinator\): reattach retries to the live request instead of duplicating

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* docs\(coordinator\): note the 499 answer for requests cancelled while queued

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* refactor\(async-broker\): reference shared header constants

Use the epp metadata constants for the SLO, objective, and fairness
header defaults instead of redefining the strings, pull the default
mode header into a constant used by the tests, and reword the
replace-media-urls comment in the example config so it no longer
claims to be the first step.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* refactor\(async-broker\): move the step into a dedicated package

The step spans five source files, so it moves from the flat steps
package into pkg/coordinator/steps/asyncbroker. The type, constructor,
and step-name constant become asyncbroker.Step, asyncbroker.New, and
asyncbroker.StepName, and the builder's first-position check imports
the new package. No behavior change.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* refactor\(async-broker\): drop the models list endpoint

The gateway already forwards GET /v1/models to the model servers, so
the step's listener serves only the result lifecycle routes.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* docs\(async-broker\): state Redis topology, memory, and ACL requirements

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* docs: move the release note to the PR description

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

---------

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>")  | Sep 10, 2026  |  
|   |   | [feat(coordinator): async-broker step serving OpenAI compatible enqueu…](https://github.com/llm-d/llm-d-router/commit/2b105ffecb08a3afad801c6b4b26f47bbb5b1257 "feat\(coordinator\): async-broker step serving OpenAI compatible enqueue, wait, and passthrough modes \(#2325\)

* feat\(coordinator\): let pipeline steps register auxiliary HTTP routes

Steps that implement RegisterRoutes\(chi.Router\) get it called once at
server construction, after the built-in inference routes. This gives a
step with its own HTTP surface \(result retrieval, model listing\) a home
on the coordinator listener without the server learning step specifics.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* feat\(coordinator\): add async-broker step bridging llm-d-async

Optional first pipeline step giving standard OpenAI clients access to
request level queueing through the gateway. Clients opt in per request
with a mode header: passthrough classifies against tenant quota and
stamps objective and fairness headers, enqueue writes to the broker
queue and answers 202 with the request id, wait holds the connection
until the result lands. The step also serves GET and DELETE
/v1/requests/{id} and GET /v1/models, with a reference doc and release
note.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* feat\(coordinator\): configurable deadlines, wait caps, and post-fetch grace

Per mode timeout bounds resolve field by field with 60s wait and 1h
enqueue defaults, max_seconds clamps client requested deadlines, and
holds that end at the deadline answer 504 DEADLINE_EXCEEDED.
wait_cap_seconds ends a hold early with the 202 response, leaving the
request fetchable. fetch_grace_seconds sets the mailbox TTL applied
after a delivered fetch, and zero deletes the result on delivery. The
doc's timeouts table is rewritten around clock spans.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* feat\(coordinator\): tenant scope AP-side keys and default the quota attribute

The envelope id is now tenant:id, so the in-flight marker and
cancellation key the AP derives from it are tenant scoped like the
mailbox, and a fetch with the wrong tenant cannot learn that an id
exists. The client-visible id is unchanged. The quota attribute
defaults to userid to match the AP's redis-quota gate.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* test\(coordinator\): stop echoing the URL param in the aux route stub

gosec flags writing a request-derived value straight to the response as
G705 XSS. The stub only needs to prove the path param reached the
handler, so capture it on the struct and assert there instead.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* docs\(coordinator\): state the async broker trust model and routing preconditions

The tenant header is trusted as asserted, so drop the doc wording that
implied tenant scoping alone prevents cross tenant probing. A deployment
note now states that the request id is the only secret protecting a
stored result and that clients wanting an unguessable handle should use
the minted UUID. A second note covers gateway routing: stock llm-d only
forwards the inference paths to the coordinator, so the fetch and models
routes need adding to the HTTPRoute. Also corrects the RouteRegistrar
comment, chi silently keeps the last handler for a duplicate pattern
rather than panicking.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* fix\(coordinator\): harden async broker error paths and validation

Review follow-ups from #2325:

- Guard the stored result status code range before WriteHeader, a
  corrupted mailbox value now answers 502 instead of panicking into the
  recovery middleware
- Re-read the mailbox once when the active marker is missing, closing
  the window where a result landing between the two reads produced a
  spurious 410 for a completed request
- Stop writing raw Redis errors into client responses on fetch, delete,
  and enqueue failures, log them server side instead
- Log quota release failures \(the counter stays elevated until the
  window TTL\) and surface result lookup errors during held waits
- Use logger.Error for recoverable Redis failures across the step
- Apply the enqueue path's tenant and id validation to the fetch and
  delete routes
- Reject negative wait_cap_seconds, timeout bounds, quota window, and
  quota limits instead of silently reinterpreting them
- Reject pipeline configs that place async-broker anywhere but first
- Narrow the retry comment in the wait disconnect path, SubmitRequest
  clears markers but does not deduplicate a still queued original

Adds tests for cross tenant DELETE, the malformed status guard, read
path validation, quota fail open on Redis error, the negative config
values, and builder ordering.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* feat\(coordinator\): cap wait deadlines at 1h and cancel on DELETE

Wait mode holds a live connection per request, so an unclamped client
requested deadline could pin one for days. timeouts.wait.max_seconds
now defaults to 3600, raised to default_seconds when that is configured
higher. Enqueue stays unclamped.

DELETE /v1/requests/{id} now cancels a still queued request before
reclaiming the mailbox, so the AP drops it pre-dispatch instead of
running it and recreating the key the delete just removed. The cancel
is a no-op for ids with no active marker.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* docs\(coordinator\): state the wait deadline cap once

The clamp default belongs in the timeouts and TTLs table, not repeated
in the config param table.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* fix\(coordinator\): support concurrent waits on one request id

Two wait holds sharing a tenant and id \(legal via retry by id\) used to
overwrite each other's wake channel, and the first to finish
unsubscribed the notification channel the survivor still needed,
silently degrading it to the backup poll. The waiter now keeps a wake
list per channel, subscribes on the first waiter, and unsubscribes on
the last, with the removal and unsubscribe under one lock so a
concurrent registration re-subscribes in order.

Covers the notify wake-up path with tests: the waiter multiplexing and
refcounted unsubscribe directly, and a full wait hold in notify mode
completing well under the backup poll interval. miniredis cannot emit
keyspace notifications, so the tests publish the notification an LPUSH
would fire.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* docs\(coordinator\): state each async broker fact once

Review follow-up on duplication: the no-header no-op, the timeout
defaults, and the fetch grace default were each restated across the
doc, the config example, struct comments, and method comments. Each now
has one home. Behavior facts live in the doc prose and the Execute
comment, defaults live in the doc's param table and the config struct
field comments the doc points at, and method comments describe only
what their method adds.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* fix\(coordinator\): report mailbox presence with a bool

The nilnil linter rejects readMailbox returning a nil result with a nil
error for an empty mailbox, so it now returns an explicit found flag.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* docs\(coordinator\): state the id reuse contract for retries

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* docs\(coordinator\): broaden the id note to the retry contract

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* feat\(coordinator\): reattach retries to the live request instead of duplicating

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* docs\(coordinator\): note the 499 answer for requests cancelled while queued

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* refactor\(async-broker\): reference shared header constants

Use the epp metadata constants for the SLO, objective, and fairness
header defaults instead of redefining the strings, pull the default
mode header into a constant used by the tests, and reword the
replace-media-urls comment in the example config so it no longer
claims to be the first step.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* refactor\(async-broker\): move the step into a dedicated package

The step spans five source files, so it moves from the flat steps
package into pkg/coordinator/steps/asyncbroker. The type, constructor,
and step-name constant become asyncbroker.Step, asyncbroker.New, and
asyncbroker.StepName, and the builder's first-position check imports
the new package. No behavior change.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* refactor\(async-broker\): drop the models list endpoint

The gateway already forwards GET /v1/models to the model servers, so
the step's listener serves only the result lifecycle routes.

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* docs\(async-broker\): state Redis topology, memory, and ACL requirements

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

* docs: move the release note to the PR description

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>

---------

Signed-off-by: Benjamin Braun <benjaminbraun@google.com>")  | Sep 10, 2026  |  
| View all files |  
## Repository files navigation
# llm-d Router
Important
**Terminology Change** : The _Inference Scheduler_ has been renamed to **llm-d Router** ; see [Terminology](https://github.com/llm-d/llm-d-router/blob/main/README.md#terminology).
Important
**API & Code Consolidation**: Core Endpoint Picker (EPP) code and the `InferenceObjective` and `InferenceModelRewrite` APIs have been merged into this repository from [Gateway API Inference Extension (GIE)](https://github.com/kubernetes-sigs/gateway-api-inference-extension). The GIE repository now exclusively hosts the `InferencePool` API—an extension of the [Kubernetes Gateway API](https://gateway-api.sigs.k8s.io/)—and defines the Endpoint Picker Protocol.
The **llm-d Router** is the intelligent entry point for inference traffic, delivering LLM load and prefix-cache aware routing, request prioritization, and advanced flow control across diverse request formats to fulfill complex serving objectives. It supports a flexible deployment model: it can run in **Standalone Mode** , where a self-managed Envoy proxy runs either alongside the EPP or as a separate service, or integrate with L7 load balancers—including self-managed instances (e.g., Istio, AgentGateway) and cloud-managed services (e.g., Google Cloud's Application Load Balancer)—via the Kubernetes Gateway API.
The router achieves its intelligence through an **Endpoint Picker (EPP)** that integrates with production-grade proxies (such as [Envoy](https://github.com/envoyproxy/envoy)) via the [ext-proc](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/ext_proc_filter) protocol, injecting real-time signals into the data plane to optimize request placement.
## Core Components and APIs
This repository hosts the following core components:
  * **Endpoint Picker (EPP)** : The intelligent routing engine that serves as the "brain" of the router. It evaluates incoming requests against the current state of the [InferencePool](https://github.com/kubernetes-sigs/gateway-api-inference-extension), considering factors like KV-cache locality, current load, and priority to make optimal placement decisions. It integrates with L7 proxies via the `ext-proc` protocol.
  * **Request Management APIs** : These resources directly influence the EPP's request handling behavior: 
    * **InferenceObjective** : Configures the EPP's scheduling goals for specific requests, including priority levels and performance targets.
    * **InferenceModelRewrite** : Directs the EPP to perform model name rewriting, enabling flexible traffic management for A/B testing and canary rollouts.
  * **Disaggregation Sidecar** : A coordination component deployed alongside model servers (typically as a sidecar to the decode worker). It orchestrates complex multi-stage inference lifecycles, such as **P/D (Prefill/Decode)** and **E/P/D (Encode/Prefill/Decode)** , by communicating with specialized encode and prefill workers to manage KV-cache and embedding transfers. For more details, see the [Disaggregation Documentation](https://github.com/llm-d/llm-d-router/blob/main/docs/disaggregation.md).


## Modes of Operation
The llm-d Router supports two primary deployment modes as specified in the [Kubernetes Gateway API Inference Extensions](https://github.com/kubernetes-sigs/gateway-api-inference-extension):
### 1. Standalone Mode
A deployment with a self-managed Envoy proxy that does not require Gateway API infrastructure. The standalone Helm chart supports two Envoy proxy topologies:
  * **Sidecar mode** (default): The proxy runs in the EPP pod. This topology is suited to basic testing and local evaluations.
  * **Service mode** : The proxy runs as a separate, horizontally scalable Deployment and Service and reaches EPP through the EPP Service. Set `router.proxy.mode=service` to scale the proxy independently from EPP.


See the [Helm chart documentation](https://github.com/llm-d/llm-d-router/blob/main/config/charts/README.md) for configuration examples.
### 2. Gateway Mode (Inference Gateway)
The recommended mode for production environments, leveraging the official [Gateway API](https://github.com/kubernetes-sigs/gateway-api). In this mode, the EPP acts as a backend for an `InferencePool`, which is referenced by an `HTTPRoute` on a shared `Gateway`. This enables advanced traffic management, multi-cluster load balancing, and shared infrastructure for both inference and traditional workloads.
For more details on the router architecture, routing logic, and different plugins (filters and scorers), see the [Architecture Documentation](https://github.com/llm-d/llm-d-router/blob/main/docs/architecture.md). For resource provisioning and container sizing recommendations under heavy or long-context workloads, see the [EPP Container Sizing Guide](https://github.com/llm-d/llm-d-router/blob/main/docs/operations.md).
Note
The project provides tools for automatic Envoy installation. However, if you install or configure it yourself, please note that the only supported [request_body_mode and response_body_mode](https://www.envoyproxy.io/docs/envoy/latest/api-v3/service/ext_proc/v3/external_processor.proto) is `FULL_DUPLEX_STREAMED`
## Terminology
To ensure clarity across the project, we use the following standard terminology:
  * **llm-d Router** : The complete intelligent entry point, comprising both the **Proxy** (e.g., Envoy) and the **Endpoint Picker (EPP)**. This term replaces "Inference Scheduler" in all contexts.
  * **llm-d Endpoint Picker (EPP)** : The specific component that implements the routing intelligence and scoring logic. Use this term when referring to capabilities or configurations specific to the EPP itself, rather than the request routing system as a whole.
  * **Inference Gateway** : A synonym for the **llm-d Router** when operating in **Gateway Mode**.
  * **Request Scheduler** : A sub-component within the EPP responsible for the queuing and dispatching of requests.


## Contributing
Start with the [llm-d organization contributing guide](https://github.com/llm-d/llm-d/blob/main/CONTRIBUTING.md) for project-wide guidelines, code of conduct, and community resources, then see [CONTRIBUTING.md](https://github.com/llm-d/llm-d-router/blob/main/CONTRIBUTING.md) for what is specific to this repository, including how to claim an issue.
Our community meeting is bi-weekly at Wednesday 10AM PDT ([Google Meet](https://meet.google.com/ozx-goao-cxh), [Meeting Notes](https://docs.google.com/document/d/1Pf3x7ZM8nNpU56nt6CzePAOmFZ24NXDeXyaYb565Wq4)).
We currently utilize the [#sig-router](https://llm-d.slack.com/?redir=%2Fmessages%2Fsig-router) channel in llm-d Slack workspace for communications.
For large changes please [create an issue](https://github.com/llm-d/llm-d-router/issues/new) first describing the change so the maintainers can do an assessment, and work on the details with you. See [DEVELOPMENT.md](https://github.com/llm-d/llm-d-router/blob/main/DEVELOPMENT.md) for details on how to work with the codebase.
Contributions are welcome!
## License
## About
llm-d Router: The intelligent entry point for inference requests
[llm-d.ai/docs/architecture/core/router](https://llm-d.ai/docs/architecture/core/router)
### Topics
[ai](https://github.com/topics/ai)[gateway-api](https://github.com/topics/gateway-api)[inference](https://github.com/topics/inference)[kubernetes](https://github.com/topics/kubernetes)[networking](https://github.com/topics/networking)
### Resources
### Code of conduct
### Contributing
### Security policy
### Stars
**336** stars
### Watchers
**32** watching
### Forks
[ forks](https://github.com/llm-d/llm-d-router/forks)
## 
[ v0.10.0Latest Aug 17, 2026 ](https://github.com/llm-d/llm-d-router/releases/tag/v0.10.0)
## 
## Languages
  * [Go96.8%](https://github.com/llm-d/llm-d-router/search?l=go)
  * [Shell1.3%](https://github.com/llm-d/llm-d-router/search?l=shell)
  * [Python0.8%](https://github.com/llm-d/llm-d-router/search?l=python)
  * [Makefile0.6%](https://github.com/llm-d/llm-d-router/search?l=makefile)
  * [Go Template0.4%](https://github.com/llm-d/llm-d-router/search?l=go-template)
  * [Ruby0.1%](https://github.com/llm-d/llm-d-router/search?l=ruby)


You can’t perform that action at this time. 
