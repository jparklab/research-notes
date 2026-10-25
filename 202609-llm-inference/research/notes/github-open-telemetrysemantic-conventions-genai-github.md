---
title: GitHub - open-telemetry/semantic-conventions-genai · GitHub
id: github-open-telemetrysemantic-conventions-genai-github
tags:
- observability
- opentelemetry
- genai-semconv
created: '2026-10-09T23:16:19.529120Z'
updated: '2026-10-10T14:52:17.247011Z'
source: https://github.com/open-telemetry/semantic-conventions-genai
source_domain: github.com
fetched_at: '2026-10-09T23:16:19.506245Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Repo root for open-telemetry/semantic-conventions-genai: as of May 2026
  the GenAI semantic conventions were split out of the core open-telemetry/semantic-conventions
  repo into this dedicated repository (PR #3696), which extends the core OTel semconv
  using the Weaver tool. 421 stars, 8 watchers, 656 commits, actively developed (latest
  commit Oct 9 2026, the same day as this fetch — e.g. making a gen_ai.client.inference.operation.details
  event ''recommended'' instead of ''opt-in''). Covers spans, metrics, and events
  for GenAI clients plus MCP (Model Context Protocol) and provider-specific conventions
  (OpenAI, AWS Bedrock, Azure AI Inference, Anthropic). Human-readable docs live under
  docs/gen-ai/, generated from YAML models under model/; a conformance/reference test
  suite lives under reference/.'
---

[Skip to content](https://github.com/open-telemetry/semantic-conventions-genai#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/open-telemetry/semantic-conventions-genai) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/open-telemetry/semantic-conventions-genai) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/open-telemetry/semantic-conventions-genai) to refresh your session. Dismiss alert
[**11** Branches](https://github.com/open-telemetry/semantic-conventions-genai/branches)[](https://github.com/open-telemetry/semantic-conventions-genai/tags)
Go to file
Open more actions menu
## Latest commit
[Make gen_ai.client.inference.operation.details event recommended inst…](https://github.com/open-telemetry/semantic-conventions-genai/commit/6fd0d763a092db245a54104fc1124031eb3c51d2)
Open commit detailssuccess
Oct 9, 2026
[6fd0d76](https://github.com/open-telemetry/semantic-conventions-genai/commit/6fd0d763a092db245a54104fc1124031eb3c51d2) · Oct 9, 2026
## History
[656 Commits](https://github.com/open-telemetry/semantic-conventions-genai/commits/main/)
Open commit details
656 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |   | Oct 2, 2026  |  
|   |   | [Make gen_ai.client.inference.operation.details event recommended inst…](https://github.com/open-telemetry/semantic-conventions-genai/commit/6fd0d763a092db245a54104fc1124031eb3c51d2 "Make gen_ai.client.inference.operation.details event recommended instead of opt-in \(#579\)

* Make gen_ai.client.inference.operation.details event recommended instead of opt-in

* Rename changelog fragment to 579.enhancement.md

* Remove useless blurb..

* Remove in-development events note blurb

* Revert "Remove in-development events note blurb"

This reverts commit 49247b3ddbacd2ea392f762249b24e913b3dae67.")  | Oct 9, 2026  |  
| [Make gen_ai.client.inference.operation.details event recommended inst…](https://github.com/open-telemetry/semantic-conventions-genai/commit/6fd0d763a092db245a54104fc1124031eb3c51d2 "Make gen_ai.client.inference.operation.details event recommended instead of opt-in \(#579\)

* Make gen_ai.client.inference.operation.details event recommended instead of opt-in

* Rename changelog fragment to 579.enhancement.md

* Remove useless blurb..

* Remove in-development events note blurb

* Revert "Remove in-development events note blurb"

This reverts commit 49247b3ddbacd2ea392f762249b24e913b3dae67.")  | Oct 9, 2026  |  
|   |   | [Make gen_ai.client.inference.operation.details event recommended inst…](https://github.com/open-telemetry/semantic-conventions-genai/commit/6fd0d763a092db245a54104fc1124031eb3c51d2 "Make gen_ai.client.inference.operation.details event recommended instead of opt-in \(#579\)

* Make gen_ai.client.inference.operation.details event recommended instead of opt-in

* Rename changelog fragment to 579.enhancement.md

* Remove useless blurb..

* Remove in-development events note blurb

* Revert "Remove in-development events note blurb"

This reverts commit 49247b3ddbacd2ea392f762249b24e913b3dae67.")  | Oct 9, 2026  |  
| [policies/check/json-schema-annotations](https://github.com/open-telemetry/semantic-conventions-genai/tree/main/policies/check/json-schema-annotations "This path skips through empty directories")  | [policies/check/json-schema-annotations](https://github.com/open-telemetry/semantic-conventions-genai/tree/main/policies/check/json-schema-annotations "This path skips through empty directories")  | [Add json-schema annotation for complex attributes, render boilerplate…](https://github.com/open-telemetry/semantic-conventions-genai/commit/791c341968d4d14cdc4d26f5c59fe690a7fcba5b "Add json-schema annotation for complex attributes, render boilerplate note in templates \(#330\)

* Add json-schema annotation for complex attributes, render boilerplate note in templates

* return deserialization for tool args and result

* make json schemas path configurable in model.py

* update ci")  | Jun 25, 2026  |  
|   |   |   | Oct 7, 2026  |  
| [templates/registry](https://github.com/open-telemetry/semantic-conventions-genai/tree/main/templates/registry "This path skips through empty directories")  | [templates/registry](https://github.com/open-telemetry/semantic-conventions-genai/tree/main/templates/registry "This path skips through empty directories")  | [Use `gen_ai.client.inference.duration` metric instead of `gen_ai.clie…](https://github.com/open-telemetry/semantic-conventions-genai/commit/4f85037ef86e92c510d2ef881a58f1076f6fc0e4 "Use `gen_ai.client.inference.duration` metric instead of `gen_ai.client.operation.duration` and move inference signals to one file \(#521\)

* Define inference conventions and gen_ai.client.inference.duration metric

* Fix inference link, spans matchers, and scenario coverage

* Rename inference.md to client-inference.md

* Keep Inference section heading in gen-ai-spans.md

* Align reference inference duration metrics

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>

* Link token metrics from client-inference doc

* Fix import block formatting in agent-framework scenario

* Add inference span matcher to adk_a2a scenario

* Update adk_a2a data.json findings for inference conventions

* Address review: keep gen_ai.inference.client span type, align refinement IDs, clarify duration on cancellation

* Restore gen_ai.client.inference span type; keep review fixes

* Revert unrelated edits to 469 changelog and agent-framework README

---------

Co-authored-by: Trask Stalnaker <trask.stalnaker@gmail.com>
Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>")  | Oct 6, 2026  |  
|   |   | [Prepare standalone GenAI semantic conventions repo](https://github.com/open-telemetry/semantic-conventions-genai/commit/ebe3d1fb9fdda3398099e11ef91c4c490912f88d "Prepare standalone GenAI semantic conventions repo")  | May 5, 2026  |  
|   |   |   | Jun 19, 2026  |  
|   |   |  [Run the reference scenarios under the conformance runner (](https://github.com/open-telemetry/semantic-conventions-genai/commit/46d43c8949afb53765a202e89f4534eeb75ca3fa "Run the reference scenarios under the conformance runner \(#442\)

* Run the reference scenarios under the conformance runner

Adopt the runner, mock LLM server and GenAI advice policies from
open-telemetry/semantic-conventions-conformance and drop this repo's
copies. The checkout is fetched at the CONFORMANCE_REF pinned in
versions.env; nothing is vendored.

* Resolve the coverage model on demand instead of committing it

* Address review feedback

- Resolve the coverage model on first use rather than at import, so
  importing the package does no work and needs no network.
- Report a failure to bootstrap the runner as a CLI error, not a trace.
- Cover the registry-name to report-key mapping for spans and events.
- Name the full path when the checkout has no coverage model script.

* feedback")[#442](https://github.com/open-telemetry/semantic-conventions-genai/pull/442)[)](https://github.com/open-telemetry/semantic-conventions-genai/commit/46d43c8949afb53765a202e89f4534eeb75ca3fa "Run the reference scenarios under the conformance runner \(#442\)

* Run the reference scenarios under the conformance runner

Adopt the runner, mock LLM server and GenAI advice policies from
open-telemetry/semantic-conventions-conformance and drop this repo's
copies. The checkout is fetched at the CONFORMANCE_REF pinned in
versions.env; nothing is vendored.

* Resolve the coverage model on demand instead of committing it

* Address review feedback

- Resolve the coverage model on first use rather than at import, so
  importing the package does no work and needs no network.
- Report a failure to bootstrap the runner as a CLI error, not a trace.
- Cover the registry-name to report-key mapping for spans and events.
- Name the full path when the checkout has no coverage model script.

* feedback")  | Aug 9, 2026  |  
|   |   |  [Make reference scenario review instructions actionable (](https://github.com/open-telemetry/semantic-conventions-genai/commit/110e8ca819079f8a25f63856053dc21efdf07fba "Make reference scenario review instructions actionable \(#455\)

* Make reference scenario review instructions actionable

* clean up more, drop duplicated file

* Address Copilot review feedback

- Fix link in reference/AGENTS.md to point to .github/skills/reference/SKILL.md
- Expand skill description to include 'evaluating' so reviews trigger it
- Restore public entry point requirement in scenario guidance
- Expand source list to include exceptions and library state

* up

* feedback")[#455](https://github.com/open-telemetry/semantic-conventions-genai/pull/455)[)](https://github.com/open-telemetry/semantic-conventions-genai/commit/110e8ca819079f8a25f63856053dc21efdf07fba "Make reference scenario review instructions actionable \(#455\)

* Make reference scenario review instructions actionable

* clean up more, drop duplicated file

* Address Copilot review feedback

- Fix link in reference/AGENTS.md to point to .github/skills/reference/SKILL.md
- Expand skill description to include 'evaluating' so reviews trigger it
- Restore public entry point requirement in scenario guidance
- Expand source list to include exceptions and library state

* up

* feedback")  | Aug 27, 2026  |  
|   |   |  [Add Towncrier changelog management (](https://github.com/open-telemetry/semantic-conventions-genai/commit/df4a3c10398b14841f6922c712c42dc31618acaf "Add Towncrier changelog management \(#275\)

* Add Towncrier changelog management

* Add changelog fragment review instruction

* Address review comment: document changelog fragment types

* Potential fix for pull request finding

Co-authored-by: Copilot Autofix powered by AI <175728472+Copilot@users.noreply.github.com>

---------

Co-authored-by: Copilot Autofix powered by AI <175728472+Copilot@users.noreply.github.com>")[#275](https://github.com/open-telemetry/semantic-conventions-genai/pull/275)[)](https://github.com/open-telemetry/semantic-conventions-genai/commit/df4a3c10398b14841f6922c712c42dc31618acaf "Add Towncrier changelog management \(#275\)

* Add Towncrier changelog management

* Add changelog fragment review instruction

* Address review comment: document changelog fragment types

* Potential fix for pull request finding

Co-authored-by: Copilot Autofix powered by AI <175728472+Copilot@users.noreply.github.com>

---------

Co-authored-by: Copilot Autofix powered by AI <175728472+Copilot@users.noreply.github.com>")  | Jun 10, 2026  |  
|   |   |   | Jun 26, 2026  |  
|   |   |  [Make reference scenario review instructions actionable (](https://github.com/open-telemetry/semantic-conventions-genai/commit/110e8ca819079f8a25f63856053dc21efdf07fba "Make reference scenario review instructions actionable \(#455\)

* Make reference scenario review instructions actionable

* clean up more, drop duplicated file

* Address Copilot review feedback

- Fix link in reference/AGENTS.md to point to .github/skills/reference/SKILL.md
- Expand skill description to include 'evaluating' so reviews trigger it
- Restore public entry point requirement in scenario guidance
- Expand source list to include exceptions and library state

* up

* feedback")[#455](https://github.com/open-telemetry/semantic-conventions-genai/pull/455)[)](https://github.com/open-telemetry/semantic-conventions-genai/commit/110e8ca819079f8a25f63856053dc21efdf07fba "Make reference scenario review instructions actionable \(#455\)

* Make reference scenario review instructions actionable

* clean up more, drop duplicated file

* Address Copilot review feedback

- Fix link in reference/AGENTS.md to point to .github/skills/reference/SKILL.md
- Expand skill description to include 'evaluating' so reviews trigger it
- Restore public entry point requirement in scenario guidance
- Expand source list to include exceptions and library state

* up

* feedback")  | Aug 27, 2026  |  
|   |   |   | May 1, 2019  |  
|   |   | [Keep upstream doc links pinned to the version in `model/manifest.yaml` (](https://github.com/open-telemetry/semantic-conventions-genai/commit/4a39b6ef1363fab57bc40e18c82abb32cc89ebcd "Keep upstream doc links pinned to the version in `model/manifest.yaml` \(#435\)

* Resolve upstream doc links in model text via upstream_docs_base

Links from model briefs and notes into open-telemetry/semantic-conventions
were hardcoded at a specific ref, so they drifted from the pinned dependency
on every version bump. After #433 bumped the pin to v1.44.0, docs/gen-ai/mcp.md
rendered v1.44.0 links in the generated tables next to v1.43.0 links in the
notes right beside them.

Model YAML is data rather than a template, so it cannot reference template
params directly. Instead, model text writes `{{upstream_docs_base}}` and the
markdown templates substitute the ref the Makefile already derives from the
model/manifest.yaml dependency.

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>
Copilot-Session: 68bee902-c139-4a15-be35-cac99d5a74bb

* Rewrite pinned upstream links from the manifest version

Replaces the `{{upstream_docs_base}}` placeholder approach. Model YAML is
published as-is \(weaver resolves it into the released resolved.yaml\), so a
template placeholder written there leaks to downstream consumers.

Instead, keep real URLs in the YAML and rewrite the version in them from
model/manifest.yaml, matching what open-telemetry/semantic-conventions does
with .github/scripts/update-spec-repo-links.sh.

The new script runs as part of `make generate-all`, and CI's existing
generated-output sync check now covers model/ and AGENTS.md, so a future
dependency bump cannot merge with stale links.

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>
Copilot-Session: 68bee902-c139-4a15-be35-cac99d5a74bb

* Apply suggestion from @trask

* Auto-update generated files on Renovate branches

Renovate only edits the pin it manages, but committed outputs are derived
from those pins, so its pull requests would land red until someone ran
`make generate-all` by hand.

Regenerate on push to `renovate/**` and push the result back as otelbot,
following the two-job split in opentelemetry-java-instrumentation's
auto-license-report workflow: the regeneration job holds no credentials,
and the app private key only appears in the job that applies the patch.

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>
Copilot-Session: 68bee902-c139-4a15-be35-cac99d5a74bb

* Clarify why the auto-update workflow exists

* Address review comment from copilot-pull-request-reviewer: match single-segment Renovate branches

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>

* Address review comment from copilot-pull-request-reviewer: explain missing version argument

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>

---------

Co-authored-by: Copilot App <223556219+Copilot@users.noreply.github.com>
Copilot-Session: 68bee902-c139-4a15-be35-cac99d5a74bb")  | Aug 5, 2026  |  
|   |   |   | Jun 4, 2026  |  
|   |   |  [Add Towncrier changelog management (](https://github.com/open-telemetry/semantic-conventions-genai/commit/df4a3c10398b14841f6922c712c42dc31618acaf "Add Towncrier changelog management \(#275\)

* Add Towncrier changelog management

* Add changelog fragment review instruction

* Address review comment: document changelog fragment types

* Potential fix for pull request finding

Co-authored-by: Copilot Autofix powered by AI <175728472+Copilot@users.noreply.github.com>

---------

Co-authored-by: Copilot Autofix powered by AI <175728472+Copilot@users.noreply.github.com>")[#275](https://github.com/open-telemetry/semantic-conventions-genai/pull/275)[)](https://github.com/open-telemetry/semantic-conventions-genai/commit/df4a3c10398b14841f6922c712c42dc31618acaf "Add Towncrier changelog management \(#275\)

* Add Towncrier changelog management

* Add changelog fragment review instruction

* Address review comment: document changelog fragment types

* Potential fix for pull request finding

Co-authored-by: Copilot Autofix powered by AI <175728472+Copilot@users.noreply.github.com>

---------

Co-authored-by: Copilot Autofix powered by AI <175728472+Copilot@users.noreply.github.com>")  | Jun 10, 2026  |  
|   |   |   | Oct 2, 2026  |  
|   |   |  [Update dependency open-telemetry/weaver to v0.26.0 (](https://github.com/open-telemetry/semantic-conventions-genai/commit/fee465db333bdd6a7d2faa320edab5cf3101a4f4 "Update dependency open-telemetry/weaver to v0.26.0 \(#494\)

* Update dependency open-telemetry/weaver to v0.26.0

* Apply suggestion from @lmolkova

---------

Co-authored-by: renovate\[bot\] <29139614+renovate\[bot\]@users.noreply.github.com>
Co-authored-by: Liudmila Molkova <neskazu@gmail.com>")[#494](https://github.com/open-telemetry/semantic-conventions-genai/pull/494)[)](https://github.com/open-telemetry/semantic-conventions-genai/commit/fee465db333bdd6a7d2faa320edab5cf3101a4f4 "Update dependency open-telemetry/weaver to v0.26.0 \(#494\)

* Update dependency open-telemetry/weaver to v0.26.0

* Apply suggestion from @lmolkova

---------

Co-authored-by: renovate\[bot\] <29139614+renovate\[bot\]@users.noreply.github.com>
Co-authored-by: Liudmila Molkova <neskazu@gmail.com>")  | Sep 3, 2026  |  
| View all files |  
## Repository files navigation
#  OpenTelemetry GenAI Semantic Conventions
Semantic Conventions for Generative AI (GenAI), including spans, metrics, and events for GenAI clients, MCP (Model Context Protocol), and provider-specific conventions (OpenAI, etc.).
This repository extends the [OpenTelemetry Semantic Conventions](https://github.com/open-telemetry/semantic-conventions) with GenAI-specific conventions, using [Weaver](https://github.com/open-telemetry/weaver) to manage dependencies on the core semantic conventions.
## Schema URL
TODO
## Read the docs
The human-readable version of the semantic conventions resides in the [docs](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs) folder. Major parts of these Markdown documents are generated from the YAML definitions located in the [model](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/model) folder.
Reference implementations and their tooling live under [reference](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/reference). For the Python reference compliance matrix and per-signal support reports, see [reference/README.md](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/reference/README.md). For contribution guidance specific to that project, see [reference/CONTRIBUTING.md](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/reference/CONTRIBUTING.md).
## Contributing
See [CONTRIBUTING.md](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/CONTRIBUTING.md).
## About
### Resources
### Code of conduct
### Contributing
### Security policy
### Stars
**421** stars
### Watchers
**8** watching
### Forks
[ forks](https://github.com/open-telemetry/semantic-conventions-genai/forks)
## 
No releases published
## 
## Languages
  * [Python92.3%](https://github.com/open-telemetry/semantic-conventions-genai/search?l=python)
  * [Jinja6.1%](https://github.com/open-telemetry/semantic-conventions-genai/search?l=jinja)
  * [Makefile1.4%](https://github.com/open-telemetry/semantic-conventions-genai/search?l=makefile)
  * [Other0.2%](https://github.com/open-telemetry/semantic-conventions-genai/search?l=Other)


You can’t perform that action at this time. 
