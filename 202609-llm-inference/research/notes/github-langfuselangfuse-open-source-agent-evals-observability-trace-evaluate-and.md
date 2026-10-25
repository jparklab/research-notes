---
title: 'GitHub - langfuse/langfuse: 🪢 Open source agent evals & observability: Trace,
  evaluate, and improve LLM applications with one open platform. · GitHub'
id: github-langfuselangfuse-open-source-agent-evals-observability-trace-evaluate-and
tags:
- observability
- llm-observability
- self-hosted
created: '2026-10-09T23:14:40.918055Z'
updated: '2026-10-10T14:52:17.237381Z'
source: https://github.com/langfuse/langfuse
source_domain: github.com
fetched_at: '2026-10-09T23:14:40.888490Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Langfuse GitHub README: open-source LLM engineering platform (tracing, evaluation,
  prompt management, datasets/experiments, playground) built on OpenTelemetry-based
  instrumentation and ClickHouse storage; acquired by/merged into ClickHouse as of
  January 2026. License is MIT for the entire repo EXCEPT the ee/ (Enterprise Edition)
  folders, which carry a separate commercial license requiring a LANGFUSE_EE_LICENSE_KEY.
  Per web corroboration (not independently fetched — langfuse.com is bot-walled),
  the ee/ gate covers SCIM provisioning, audit logs, data-retention policies, project-level
  RBAC, protected prompt labels, server-side data masking, UI customization, org-creator/Org
  Management API; notably Enterprise SSO and org-level RBAC are reported free even
  in the OSS self-hosted build, unlike Langfuse Cloud. Fully self-hostable via Docker
  Compose (git clone + docker compose up) — platform that owns its own storage/UI
  (ClickHouse + Postgres + Redis) rather than vendor-neutral OTel-emitting instrumentation.
  Used as a dependency by large OSS projects per its own dependents table.'
---

[Skip to content](https://github.com/langfuse/langfuse#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/langfuse/langfuse) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/langfuse/langfuse) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/langfuse/langfuse) to refresh your session. Dismiss alert
[**946** Branches](https://github.com/langfuse/langfuse/branches)[**721** Tags](https://github.com/langfuse/langfuse/tags)
Go to file
Open more actions menu
## Latest commit
and
[fix(web): Beta switch sits left of the view toggle (](https://github.com/langfuse/langfuse/commit/eecc7d023b9aa351bb96b3ddd22cf6761a842dd9)[#18559](https://github.com/langfuse/langfuse/pull/18559)[)](https://github.com/langfuse/langfuse/commit/eecc7d023b9aa351bb96b3ddd22cf6761a842dd9)
Open commit detailssuccess
Oct 9, 2026
[eecc7d0](https://github.com/langfuse/langfuse/commit/eecc7d023b9aa351bb96b3ddd22cf6761a842dd9) · Oct 9, 2026
## History
[9,996 Commits](https://github.com/langfuse/langfuse/commits/main/)
Open commit details
9,996 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |  [feat(tables): let rows use a custom height (](https://github.com/langfuse/langfuse/commit/a1ade6eb7286620d9691f5e034cf3c5d665bb159 "feat\(tables\): let rows use a custom height \(#18445\)

* feat\(experiments\): let comparison rows use a custom height

Long outputs stay clipped at the Large preset. Dragging a row edge sets one
pixel height for every run column, shown as Custom beside the presets, and
stores it in the same per-table row-height setting.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(experiments\): load the full output when comparison rows are tall

A custom row still stopped at the short event preview, so making it taller
left empty space under a clipped output. Medium, Large, and Custom heights
now read the full event text, capped at the expanded character limit.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(experiments\): announce and focus the row-height handle

Arrow keys preview the shared height and save it once when the key is
released, so holding an arrow does not record a resize on every repeat.
The handle stays visible while focused, and a preset reports its measured
height.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* feat\(tables\): drag a custom row height wherever the switch is shown

Hide Custom until a height has been dragged, and drop the bar that
highlighted a row edge while dragging. Keyboard focus stays a ring.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(tables\): restore compact text when a custom row shrinks

A dragged height was always treated as expanded, so shrinking it back
toward Small left input and output on the JSON preview. Compact text
now follows the same Medium line as the presets, in both directions.
The row-edge target stays invisible; the resize cursor and a keyboard
focus ring are the only affordances.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(tables\): center the row-resize hit area on the border

The grab strip sat entirely above the row border. It is now 6px,
split 3px above that line and 3px into the next row, and the last
row keeps a 6px strip centered on its own bottom edge.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(tables\): switch row rendering from the height, not the preset

Input and output stayed on the preset's text-or-JSON choice until a drag
was released. Presets and a drag now share one height, and the cell
switches at Medium while the pointer is still moving.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(tables\): always follow row height in IO and tag columns

The shared column factories no longer take an opt-in. Inside a data
table the resolved row height selects one line versus the JSON preview,
and tag wrapping. singleLine and shouldWrap remain the fallback when
that context is absent. The input and output color stays on the table
cell.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(tables\): follow row height by default, keep comparison display

IO and tag columns follow the table row height unless a caller opts out.
Experiment comparison keeps its Display text/JSON setting. Outside a data
table, the existing singleLine and shouldWrap values still decide.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* test\(tables\): pin row-height follow default and opt-out

Inside a data table, input cells follow the compact row unless they opt
out. Outside one, singleLine stays in charge.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(tables\): let a row drag shrink back to Small

The drag floor was a fixed 48px. On the default table that is taller than
Small, and on comparison tables it is shorter, so the handle could not
return a row to the Small preset. The floor is now that table's Small
height, and a drag that lands on a preset selects it.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(tables\): keep the row-height floor private

The default Small height is only the clamp inside this module. Exporting it made knip fail and invited a second, table-agnostic minimum.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(tables\): resize one row during a drag, then keep it in view

A live preview on every row moved the dragged row under the pointer.
Only that row changes height and text or JSON mode until release. The
shared height is committed on pointer-up, and the scroller is adjusted
so the row's top edge stays put, including when the maximum scroll shrinks.
Experiment tables can still drag from Medium down to their own Small.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(tables\): resize the row above the border from either half of the strip

The lower 3px of the row-height hit area lives in the next row. A drag
started there now previews, anchors, and commits the row above the line.
The last row's strip still resizes the last row.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

---------

Co-authored-by: Cursor Agent <cursoragent@cursor.com>")[#18445](https://github.com/langfuse/langfuse/pull/18445)[)](https://github.com/langfuse/langfuse/commit/a1ade6eb7286620d9691f5e034cf3c5d665bb159 "feat\(tables\): let rows use a custom height \(#18445\)

* feat\(experiments\): let comparison rows use a custom height

Long outputs stay clipped at the Large preset. Dragging a row edge sets one
pixel height for every run column, shown as Custom beside the presets, and
stores it in the same per-table row-height setting.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(experiments\): load the full output when comparison rows are tall

A custom row still stopped at the short event preview, so making it taller
left empty space under a clipped output. Medium, Large, and Custom heights
now read the full event text, capped at the expanded character limit.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(experiments\): announce and focus the row-height handle

Arrow keys preview the shared height and save it once when the key is
released, so holding an arrow does not record a resize on every repeat.
The handle stays visible while focused, and a preset reports its measured
height.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* feat\(tables\): drag a custom row height wherever the switch is shown

Hide Custom until a height has been dragged, and drop the bar that
highlighted a row edge while dragging. Keyboard focus stays a ring.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(tables\): restore compact text when a custom row shrinks

A dragged height was always treated as expanded, so shrinking it back
toward Small left input and output on the JSON preview. Compact text
now follows the same Medium line as the presets, in both directions.
The row-edge target stays invisible; the resize cursor and a keyboard
focus ring are the only affordances.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(tables\): center the row-resize hit area on the border

The grab strip sat entirely above the row border. It is now 6px,
split 3px above that line and 3px into the next row, and the last
row keeps a 6px strip centered on its own bottom edge.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(tables\): switch row rendering from the height, not the preset

Input and output stayed on the preset's text-or-JSON choice until a drag
was released. Presets and a drag now share one height, and the cell
switches at Medium while the pointer is still moving.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(tables\): always follow row height in IO and tag columns

The shared column factories no longer take an opt-in. Inside a data
table the resolved row height selects one line versus the JSON preview,
and tag wrapping. singleLine and shouldWrap remain the fallback when
that context is absent. The input and output color stays on the table
cell.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(tables\): follow row height by default, keep comparison display

IO and tag columns follow the table row height unless a caller opts out.
Experiment comparison keeps its Display text/JSON setting. Outside a data
table, the existing singleLine and shouldWrap values still decide.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* test\(tables\): pin row-height follow default and opt-out

Inside a data table, input cells follow the compact row unless they opt
out. Outside one, singleLine stays in charge.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(tables\): let a row drag shrink back to Small

The drag floor was a fixed 48px. On the default table that is taller than
Small, and on comparison tables it is shorter, so the handle could not
return a row to the Small preset. The floor is now that table's Small
height, and a drag that lands on a preset selects it.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(tables\): keep the row-height floor private

The default Small height is only the clamp inside this module. Exporting it made knip fail and invited a second, table-agnostic minimum.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(tables\): resize one row during a drag, then keep it in view

A live preview on every row moved the dragged row under the pointer.
Only that row changes height and text or JSON mode until release. The
shared height is committed on pointer-up, and the scroller is adjusted
so the row's top edge stays put, including when the maximum scroll shrinks.
Experiment tables can still drag from Medium down to their own Small.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* fix\(tables\): resize the row above the border from either half of the strip

The lower 3px of the row-height hit area lives in the next row. A drag
started there now previews, anchors, and commits the row above the line.
The last row's strip still resizes the last row.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

---------

Co-authored-by: Cursor Agent <cursoragent@cursor.com>")  | Oct 9, 2026  |  
|   |   |  [chore(dx): enable pnpm dedupe and task-aware turbo watch (](https://github.com/langfuse/langfuse/commit/f75c661dbe8c6b85523c81486b39e8403ac2c141 "chore\(dx\): enable pnpm dedupe and task-aware turbo watch \(#17852\)

* chore\(tooling\): enable pnpm, Turbo and Next.js dev features

* update

* bump

* dedupe

* simp

* exclude OK liceneses")[#17852](https://github.com/langfuse/langfuse/pull/17852)[)](https://github.com/langfuse/langfuse/commit/f75c661dbe8c6b85523c81486b39e8403ac2c141 "chore\(dx\): enable pnpm dedupe and task-aware turbo watch \(#17852\)

* chore\(tooling\): enable pnpm, Turbo and Next.js dev features

* update

* bump

* dedupe

* simp

* exclude OK liceneses")  | Oct 2, 2026  |  
|   |   |  [chore(dx): enable pnpm dedupe and task-aware turbo watch (](https://github.com/langfuse/langfuse/commit/f75c661dbe8c6b85523c81486b39e8403ac2c141 "chore\(dx\): enable pnpm dedupe and task-aware turbo watch \(#17852\)

* chore\(tooling\): enable pnpm, Turbo and Next.js dev features

* update

* bump

* dedupe

* simp

* exclude OK liceneses")[#17852](https://github.com/langfuse/langfuse/pull/17852)[)](https://github.com/langfuse/langfuse/commit/f75c661dbe8c6b85523c81486b39e8403ac2c141 "chore\(dx\): enable pnpm dedupe and task-aware turbo watch \(#17852\)

* chore\(tooling\): enable pnpm, Turbo and Next.js dev features

* update

* bump

* dedupe

* simp

* exclude OK liceneses")  | Oct 2, 2026  |  
|   |   | [fix(ci): let the model price audit list the rows its validator expects (](https://github.com/langfuse/langfuse/commit/a36654dcff05c72c0e73b2b14e6d0f8f3de794b0 "fix\(ci\): let the model price audit list the rows its validator expects \(#18514\)

The audit folded dated snapshots such as o1-preview-2024-09-12 into their
alias's row, so the validator found no row for the removed snapshot. The
prompt now asks for one row per removed ID, and the audit can run a
read-only helper that prints every model ID the validator will expect.

Co-authored-by: Cursor Agent <cursoragent@cursor.com>")  | Oct 9, 2026  |  
|   |   |  [chore: Remove linting from pre-commit hook (](https://github.com/langfuse/langfuse/commit/b2dac595108342193aef56734c2c47f6352848c2 "chore: Remove linting from pre-commit hook \(#17703\)")[#17703](https://github.com/langfuse/langfuse/pull/17703)[)](https://github.com/langfuse/langfuse/commit/b2dac595108342193aef56734c2c47f6352848c2 "chore: Remove linting from pre-commit hook \(#17703\)")  | Sep 21, 2026  |  
|   |   |  [chore(dx): enable pnpm dedupe and task-aware turbo watch (](https://github.com/langfuse/langfuse/commit/f75c661dbe8c6b85523c81486b39e8403ac2c141 "chore\(dx\): enable pnpm dedupe and task-aware turbo watch \(#17852\)

* chore\(tooling\): enable pnpm, Turbo and Next.js dev features

* update

* bump

* dedupe

* simp

* exclude OK liceneses")[#17852](https://github.com/langfuse/langfuse/pull/17852)[)](https://github.com/langfuse/langfuse/commit/f75c661dbe8c6b85523c81486b39e8403ac2c141 "chore\(dx\): enable pnpm dedupe and task-aware turbo watch \(#17852\)

* chore\(tooling\): enable pnpm, Turbo and Next.js dev features

* update

* bump

* dedupe

* simp

* exclude OK liceneses")  | Oct 2, 2026  |  
|   |   | [Revert "feat(docker): build FIPS images on Iron Bank Alpine instead o…](https://github.com/langfuse/langfuse/commit/f964009483c719176b29c057474d2da586e9345c "Revert "feat\(docker\): build FIPS images on Iron Bank Alpine instead of UBI 9" \(#18449\)

Revert "feat\(docker\): build FIPS images on Iron Bank Alpine instead of UBI 9 …"

This reverts commit 3d680c019ddaec28770db53082b58c1ec3dbeff5.")  | Oct 8, 2026  |  
|  [chore(agents): use AGENTS.md directly for folder instructions (](https://github.com/langfuse/langfuse/commit/1357013dee16afcd6ba3c93181d6678e9f404278 "chore\(agents\): use AGENTS.md directly for folder instructions \(#17719\)

Remove compatibility aliases and their generator, retain shared configuration and skills, and upgrade pinned agent runtimes to support AGENTS.md.

Co-authored-by: Claude Opus 4.8 \(1M context\) <noreply@anthropic.com>")[#17719](https://github.com/langfuse/langfuse/pull/17719)[)](https://github.com/langfuse/langfuse/commit/1357013dee16afcd6ba3c93181d6678e9f404278 "chore\(agents\): use AGENTS.md directly for folder instructions \(#17719\)

Remove compatibility aliases and their generator, retain shared configuration and skills, and upgrade pinned agent runtimes to support AGENTS.md.

Co-authored-by: Claude Opus 4.8 \(1M context\) <noreply@anthropic.com>")  | Sep 21, 2026  |  
|  [fix(api): match batch score results before the accepted response (](https://github.com/langfuse/langfuse/commit/1744e301ff3706c055439e7b77e17cfd5cb71e23 "fix\(api\): match batch score results before the accepted response \(#18518\)

* fix\(api\): match batch score results before the accepted response

CreateScoresResponse is an undiscriminated union. Generated Python clients default the literal message field of CreateScoreBatchResponse, so with older pydantic a 207 body parsed as the 202 shape and dropped the accepted/rejected counts and errors. List CreateScoreBatchResults first and the 202 shape last. The wire format is unchanged.

* fix\(api\): apply the batch score union order to the client API spec

---------

Co-authored-by: Cursor Agent <cursoragent@cursor.com>")[#18518](https://github.com/langfuse/langfuse/pull/18518)  | Oct 9, 2026  |  
|   |   |  [fix(web): prevent media tags from appearing squished (](https://github.com/langfuse/langfuse/commit/95816be3ae8134fcd7b7c3842a15c3190d5417b2 "fix\(web\): prevent media tags from appearing squished \(#18524\)

* fix\(web\): prevent media tags from appearing squished

Co-authored-by: Tobias Wochinger <wochinge@users.noreply.github.com>

* fix\(web\): scope media tag height to formatted views

Co-authored-by: Tobias Wochinger <wochinge@users.noreply.github.com>

* test\(web\): reproduce narrow formatted media layout

* fix\(web\): keep formatted JSON within narrow containers

* fix\(web\): expand unresolved file previews in session messages

* revert\(web\): limit media tag fix to tag rendering

* fix\(web\): use readable media tags by default

* fix\(web\): size media tags to their content

* fix\(web\): render session S3 files as media

* fix\(web\): contain unresolved session files

* fix\(web\): expand S3 media in session files

* test\(web\): cover expanded external media

* test\(web\): keep session media stories visual

* refactor\(web\): address external media review feedback

* fix\(web\): avoid resolving disabled external media

* fix\(web\): satisfy media rendering lint rules

* test\(web\): cover expired external media refresh

* fix\(web\): refresh expired external media

* test\(web\): cover external media size and playback refresh

* fix\(web\): harden external media previews

* test\(web\): keep media checks focused on existing suites

* Revert "fix\(web\): harden external media previews"

This reverts commit bd89857eb7d60cf59f20d2896a304113fca516ca.

* fix\(web\): limit automatic external media previews

* fix\(web\): remove duplicate storage test mock

* fix\(web\): keep media size lookup best effort

---------

Co-authored-by: Cursor Agent <cursoragent@cursor.com>
Co-authored-by: Tobias Wochinger <wochinge@users.noreply.github.com>")[#18524](https://github.com/langfuse/langfuse/pull/18524)[)](https://github.com/langfuse/langfuse/commit/95816be3ae8134fcd7b7c3842a15c3190d5417b2 "fix\(web\): prevent media tags from appearing squished \(#18524\)

* fix\(web\): prevent media tags from appearing squished

Co-authored-by: Tobias Wochinger <wochinge@users.noreply.github.com>

* fix\(web\): scope media tag height to formatted views

Co-authored-by: Tobias Wochinger <wochinge@users.noreply.github.com>

* test\(web\): reproduce narrow formatted media layout

* fix\(web\): keep formatted JSON within narrow containers

* fix\(web\): expand unresolved file previews in session messages

* revert\(web\): limit media tag fix to tag rendering

* fix\(web\): use readable media tags by default

* fix\(web\): size media tags to their content

* fix\(web\): render session S3 files as media

* fix\(web\): contain unresolved session files

* fix\(web\): expand S3 media in session files

* test\(web\): cover expanded external media

* test\(web\): keep session media stories visual

* refactor\(web\): address external media review feedback

* fix\(web\): avoid resolving disabled external media

* fix\(web\): satisfy media rendering lint rules

* test\(web\): cover expired external media refresh

* fix\(web\): refresh expired external media

* test\(web\): cover external media size and playback refresh

* fix\(web\): harden external media previews

* test\(web\): keep media checks focused on existing suites

* Revert "fix\(web\): harden external media previews"

This reverts commit bd89857eb7d60cf59f20d2896a304113fca516ca.

* fix\(web\): limit automatic external media previews

* fix\(web\): remove duplicate storage test mock

* fix\(web\): keep media size lookup best effort

---------

Co-authored-by: Cursor Agent <cursoragent@cursor.com>
Co-authored-by: Tobias Wochinger <wochinge@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   |  [fix(web): guard radix select typeahead focus on unmounted items (](https://github.com/langfuse/langfuse/commit/7e9d6c9700409af5e2af2921b66643f0fe2a48b1 "fix\(web\): guard radix select typeahead focus on unmounted items \(#17373\)

Select typeahead scheduled focus on a collection item that could unmount
before the timeout ran, throwing an unhandled TypeError. Match the
upstream 2.3.7 optional chain without bumping the package.

Co-authored-by: Cursor Agent <cursoragent@cursor.com>")[#17373](https://github.com/langfuse/langfuse/pull/17373)[)](https://github.com/langfuse/langfuse/commit/7e9d6c9700409af5e2af2921b66643f0fe2a48b1 "fix\(web\): guard radix select typeahead focus on unmounted items \(#17373\)

Select typeahead scheduled focus on a collection item that could unmount
before the timeout ran, throwing an unhandled TypeError. Match the
upstream 2.3.7 optional chain without bumping the package.

Co-authored-by: Cursor Agent <cursoragent@cursor.com>")  | Sep 11, 2026  |  
|   |   | [fix(ci): let the model price audit list the rows its validator expects (](https://github.com/langfuse/langfuse/commit/a36654dcff05c72c0e73b2b14e6d0f8f3de794b0 "fix\(ci\): let the model price audit list the rows its validator expects \(#18514\)

The audit folded dated snapshots such as o1-preview-2024-09-12 into their
alias's row, so the validator found no row for the removed snapshot. The
prompt now asks for one row per removed ID, and the audit can run a
read-only helper that prints every model ID the validator will expect.

Co-authored-by: Cursor Agent <cursoragent@cursor.com>")  | Oct 9, 2026  |  
|   |   |  [feat(topics): classify traces into buckets (](https://github.com/langfuse/langfuse/commit/3fed0dadc5d4d17f465fbd01b18f41f3cf232aff "feat\(topics\): classify traces into buckets \(#17664\)

* feat\(topics\): classify traces into buckets

* add feature flag etc

* move from python to rust

* enable topics for demo@

* remove token budgetting

* fix ui: tabbing

* fix ui: improv

* no random file storage as queue

* shorter transcripts

* datadog tracing

* Allow custom min sample number.

* better progress tracking

* database separation

* perf faster clustering

* embedding queue

* clean UX

* cleanup

* fix

* fix

* cleanup

* no unbound

* cleanup

* simplify queueing, deletion

* no s3

* sessionid, traceid instead of unitid

* simp

* simp

* allow list project ids

* simplify CH schema

* rename schema, simplify

* simplify

* ci

* simplify, fix ci

* consolidate tests

* fix ci worker test, gate env flag

* add db table init script

* fix tests

* add flag parser

* fix tests

* switch to bedrock based cohere embed

* simplify

* use design system compos

* use assembleTranscript

* fix

* remove dev tables

* fix

* switch to bedrock served models

* fix

* fix reasoning

* run CH tables in ci

* fix float precision

* sec

* manual review 1

* slimmer setup

* remove IDs

* datadog monitors

* simplify

* fix

* feat\(worker\): summarize trace batches into topics

Run facet summarization from the assembled transcript on the trace-batch queue, using instance Bedrock credentials, and enqueue older matching traces through the same worker.

Co-authored-by: Cursor <cursoragent@cursor.com>

* feat\(worker\): admit every trace of Topics projects to trace batching

Projects in LANGFUSE_TOPICS_ENABLED_PROJECT_IDS bypass the trace-batch sampling
rate at ingestion, so each of their traces reaches the trace-batch queue and is
summarized. With LANGFUSE_TRACE_BATCH_SAMPLING_RATE=0 the automated flow runs
for those projects only.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* perf\(worker\): skip trace-batch tracking at sampling rate 0

At LANGFUSE_TRACE_BATCH_SAMPLING_RATE=0 only Topics-enabled projects are
admitted; for all other projects ingestion now returns before building
per-trace state, recording metrics or calling Redis.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* feat\(worker\): default trace-batch sampling to off

LANGFUSE_TRACE_BATCH_SAMPLING_RATE now defaults to 0: with trace-batch
ingestion enabled and no rate set, only Topics-enabled projects are admitted
and all other projects are skipped. Set a rate to sample other projects.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* fix\(worker\): pin trace-batch idle time in tests and drop unused exports

The trace-batch idle default is 2 minutes on DEV, which the scheduling test
ran under in CI; pin the 10-minute value it asserts. Make two helpers that are
only used in their own module private so knip passes.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* feat\(worker\): keep trace batches running past failed Topics traces

A trace whose Topics summary fails no longer fails the batch job. Every trace
is processed, outcomes are counted once per job, and failed traces are
re-enqueued as a new trace-batch job with a retry counter and doubling delay,
up to LANGFUSE_TOPICS_TRACE_MAX_RETRIES. Errors without a provider reason,
such as the input limit, are not retried. Only a failed batch read still fails
the job.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* fix CI

* fix\(worker\): count failed Topics traces without re-enqueueing trace batches

A failed Topics summary re-enqueued its trace as a new trace-batch job,
which read the shared batch from ClickHouse again. During a provider
outage every affected trace repeated that read. Failed traces are now
only counted in langfuse.topics.trace_outcomes by reason.

Co-authored-by: Cursor <cursoragent@cursor.com>

* fix topics positioning

* fix data model after valeriy

* switch to eval tables

* ch migrations are real

* simplify

* feat\(topics\): configure summary and embedding models

* fix\(topics\): gate every model pipeline stage

* fix\(topics\): use geo Bedrock profile IDs

* fix\(topics\): use regional embedding profiles

* docs\(topics\): label PoC env settings as internal

* fix\(topics\): satisfy render lint rules

* test\(topics\): expect env-configured embedding model

* fix\(native\): link N-API symbols in Rust tests

* test\(topics\): update model config expectations

* environtment no default

* slimmer tests

* CH settings on table

* use lightweight deletes

* rename journals to execution store

* empty default env i guess

* move env var

* fix\(clickhouse\): rebuild local databases during reset

* clean

* fix useeffect

* cleanup

* fix

* remove split view

---------

Co-authored-by: Lysander Kiesel <44638111+LysanderKie@users.noreply.github.com>
Co-authored-by: Cursor <cursoragent@cursor.com>
Co-authored-by: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>")[#17664](https://github.com/langfuse/langfuse/pull/17664)[)](https://github.com/langfuse/langfuse/commit/3fed0dadc5d4d17f465fbd01b18f41f3cf232aff "feat\(topics\): classify traces into buckets \(#17664\)

* feat\(topics\): classify traces into buckets

* add feature flag etc

* move from python to rust

* enable topics for demo@

* remove token budgetting

* fix ui: tabbing

* fix ui: improv

* no random file storage as queue

* shorter transcripts

* datadog tracing

* Allow custom min sample number.

* better progress tracking

* database separation

* perf faster clustering

* embedding queue

* clean UX

* cleanup

* fix

* fix

* cleanup

* no unbound

* cleanup

* simplify queueing, deletion

* no s3

* sessionid, traceid instead of unitid

* simp

* simp

* allow list project ids

* simplify CH schema

* rename schema, simplify

* simplify

* ci

* simplify, fix ci

* consolidate tests

* fix ci worker test, gate env flag

* add db table init script

* fix tests

* add flag parser

* fix tests

* switch to bedrock based cohere embed

* simplify

* use design system compos

* use assembleTranscript

* fix

* remove dev tables

* fix

* switch to bedrock served models

* fix

* fix reasoning

* run CH tables in ci

* fix float precision

* sec

* manual review 1

* slimmer setup

* remove IDs

* datadog monitors

* simplify

* fix

* feat\(worker\): summarize trace batches into topics

Run facet summarization from the assembled transcript on the trace-batch queue, using instance Bedrock credentials, and enqueue older matching traces through the same worker.

Co-authored-by: Cursor <cursoragent@cursor.com>

* feat\(worker\): admit every trace of Topics projects to trace batching

Projects in LANGFUSE_TOPICS_ENABLED_PROJECT_IDS bypass the trace-batch sampling
rate at ingestion, so each of their traces reaches the trace-batch queue and is
summarized. With LANGFUSE_TRACE_BATCH_SAMPLING_RATE=0 the automated flow runs
for those projects only.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* perf\(worker\): skip trace-batch tracking at sampling rate 0

At LANGFUSE_TRACE_BATCH_SAMPLING_RATE=0 only Topics-enabled projects are
admitted; for all other projects ingestion now returns before building
per-trace state, recording metrics or calling Redis.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* feat\(worker\): default trace-batch sampling to off

LANGFUSE_TRACE_BATCH_SAMPLING_RATE now defaults to 0: with trace-batch
ingestion enabled and no rate set, only Topics-enabled projects are admitted
and all other projects are skipped. Set a rate to sample other projects.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* fix\(worker\): pin trace-batch idle time in tests and drop unused exports

The trace-batch idle default is 2 minutes on DEV, which the scheduling test
ran under in CI; pin the 10-minute value it asserts. Make two helpers that are
only used in their own module private so knip passes.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* feat\(worker\): keep trace batches running past failed Topics traces

A trace whose Topics summary fails no longer fails the batch job. Every trace
is processed, outcomes are counted once per job, and failed traces are
re-enqueued as a new trace-batch job with a retry counter and doubling delay,
up to LANGFUSE_TOPICS_TRACE_MAX_RETRIES. Errors without a provider reason,
such as the input limit, are not retried. Only a failed batch read still fails
the job.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* fix CI

* fix\(worker\): count failed Topics traces without re-enqueueing trace batches

A failed Topics summary re-enqueued its trace as a new trace-batch job,
which read the shared batch from ClickHouse again. During a provider
outage every affected trace repeated that read. Failed traces are now
only counted in langfuse.topics.trace_outcomes by reason.

Co-authored-by: Cursor <cursoragent@cursor.com>

* fix topics positioning

* fix data model after valeriy

* switch to eval tables

* ch migrations are real

* simplify

* feat\(topics\): configure summary and embedding models

* fix\(topics\): gate every model pipeline stage

* fix\(topics\): use geo Bedrock profile IDs

* fix\(topics\): use regional embedding profiles

* docs\(topics\): label PoC env settings as internal

* fix\(topics\): satisfy render lint rules

* test\(topics\): expect env-configured embedding model

* fix\(native\): link N-API symbols in Rust tests

* test\(topics\): update model config expectations

* environtment no default

* slimmer tests

* CH settings on table

* use lightweight deletes

* rename journals to execution store

* empty default env i guess

* move env var

* fix\(clickhouse\): rebuild local databases during reset

* clean

* fix useeffect

* cleanup

* fix

* remove split view

---------

Co-authored-by: Lysander Kiesel <44638111+LysanderKie@users.noreply.github.com>
Co-authored-by: Cursor <cursoragent@cursor.com>
Co-authored-by: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>")  | Oct 6, 2026  |  
|  [fix(web): Beta switch sits left of the view toggle (](https://github.com/langfuse/langfuse/commit/eecc7d023b9aa351bb96b3ddd22cf6761a842dd9 "fix\(web\): Beta switch sits left of the view toggle \(#18559\)

fix\(web\): Beta switch sits left of the view toggle so the toggle stays right-aligned

Co-authored-by: Claude Opus 5.5 <noreply@anthropic.com>")[#18559](https://github.com/langfuse/langfuse/pull/18559)[)](https://github.com/langfuse/langfuse/commit/eecc7d023b9aa351bb96b3ddd22cf6761a842dd9 "fix\(web\): Beta switch sits left of the view toggle \(#18559\)

fix\(web\): Beta switch sits left of the view toggle so the toggle stays right-aligned

Co-authored-by: Claude Opus 5.5 <noreply@anthropic.com>")  | Oct 9, 2026  |  
|   |   |  [feat(evals): graduate decision model evaluators (](https://github.com/langfuse/langfuse/commit/615a0f2256e9b58756b4ab6ddb5ec5ebb0340d35 "feat\(evals\): graduate decision model evaluators \(#18541\)

Co-authored-by: Cursor Agent <cursoragent@cursor.com>
Co-authored-by: Tobias Wochinger <wochinge@users.noreply.github.com>")[#18541](https://github.com/langfuse/langfuse/pull/18541)[)](https://github.com/langfuse/langfuse/commit/615a0f2256e9b58756b4ab6ddb5ec5ebb0340d35 "feat\(evals\): graduate decision model evaluators \(#18541\)

Co-authored-by: Cursor Agent <cursoragent@cursor.com>
Co-authored-by: Tobias Wochinger <wochinge@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   |  [feat(query-ast): clean Kysely base for the ClickHouse query AST (](https://github.com/langfuse/langfuse/commit/8521ece6c8d5bd828e9492af903d61db4bf13878 "feat\(query-ast\): clean Kysely base for the ClickHouse query AST \(#16687\)

* chore\(deps\): add kysely to shared

Compile-only ClickHouse dialect for the query-builder spike. Pin 0.28.17 so shared can keep emitting CJS; 0.29 is ESM-only.

* feat\(query-ast\): compile ClickHouse queries through Kysely

Replace the environments repository string templates with traced Kysely
nodes. Add a compile-only ClickHouse dialect, real ARRAY JOIN and LIMIT BY
nodes via plugin plus transformer \(no fork\), mandatory ExecutionContext
tenancy injection, and a frozen catalog that checks compile\(AST\) matches
reference SQL after clickhouse format.

* feat\(query-ast\): add typed selection, metadata indexOf, and virtual views

Move Kysely-specific code under query-ast/kysely/. Catch sum\(\) over String
at a runtime validation pass \(DateTime vs Int is a TS error\). Lower
metadata\[key\] to ArrayIndexNode plus indexOf with a bound key. Expose a
named catalog query as a WITH view so the outer query only sees declared
columns, with tenancy still injected on the inner table.

* fix\(query-ast\): identity-stamp tenancy and reject raw relation SQL

Close the Kysely compile-path escape hatches without a fork: stamp
injected trees by identity instead of a copyable node property, and
reject raw fragments that embed SELECT/FROM/JOIN in any position.
Prove JOIN project_id injection with table-qualified SQL and
clickhouse-local EXPLAIN.

* fix\(query-ast\): make tenancy injection prove the scoped project

The tenancy pass treated any `project_id = …` predicate as already
scoping a relation without checking the bound value or the qualified
table. A query carrying `project_id = <otherProject>` was accepted with
no corrective predicate, and one unqualified `project_id` was treated as
covering every tenanted table in a JOIN — leaking sibling relations.

`predicateCovers` now requires the equality's value to equal the
ExecutionContext project, and an unqualified column only counts when a
single tenanted relation is in scope; otherwise a table-qualified
reference is required. Drops the no-op branch in `assertRelationAllowed`.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(query-ast\): derive schema facts from one table registry

Ports the semantic-model idea from the hand-rolled spike arm: a single
`defineTable` declaration per relation is now the source of truth for the
Kysely row types \(`ClickHouseDatabase`\), the runtime column-type map the
aggregate type-check pass consults \(`COLUMN_DATA_TYPES`\), and the set the
tenancy pass scopes \(`TENANTED_TABLES`\) — instead of three hand-maintained
tables that drift.

Drops the unreachable `events` / `events_full` entries from the tenanted
set: they are not in the row type and raw sources are rejected, so no
query could ever reference them. Also documents the compiler's
Kysely-private `visitNode` coupling as a known fragility.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* fix\(query-ast\): re-export formatSql after rebase onto main

The rebase onto main silently dropped the `export` keyword restored by
the Kysely arm, leaving catalog.test.ts importing an undefined
`formatSql`. Restores the export; catalog parity is green again.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): trim provenance from schema registry comment

Drop change/attempt-scoped wording that goes stale once merged; keep the
why \(single source of truth\) and the forward note on physical metadata.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* fix\(query-ast\): satisfy knip and codespell on the Kysely base

- Drop `export` from six symbols used only within their own module
  \(inferClickHouseType, ArrayJoin/LimitBy/VirtualView plugins, stampTenancy,
  isTenancyStamped\); the public wrappers remain exported.
- Anchor the compile-time type-assertion file from schema.test.ts so knip
  keeps it in the build graph without a suppression config.
- Allow `froms` \(Kysely's SelectQueryNode field name\) in codespell.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): drop library-evaluation provenance from code comments

Remove "record for the evaluation / no fork" framing from the plugin,
view, catalog, and type-assertion doc comments; keep the mechanism and
tenancy/gotcha notes that describe the code itself.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(query-ast\): colocate the environments query with its repository

Move the tracing/scores query builders and their compile step out of the
generic query-ast library and into repositories/environments.ts, next to
getEnvironmentsForProject, as file-local functions. Keeps the pure
input->{sql,params} seam \(still unit-testable, golden test unchanged\) while
removing the cross-file jump and the split write-mode branching. The
query-ast/kysely library keeps only generic internals \(nodes, compiler,
plugins, dialect, tenancy, catalog\); per-call-site queries live with their
call site now that the Kysely builder is expected to spread into repositories.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(query-ast\): rely on injected tenancy instead of manual project_id

Drop the hand-written `project_id = <id>` predicate from the environments
call site and every catalog entry, and lean on the mandatory tenancy pass to
inject it from the ExecutionContext. The injector now prepends the scope as
the leading WHERE predicate so the bound value keeps a stable first parameter
position; JOIN-ON scoping stays appended after the join conditions.

Also inline the environments query construction directly into
getEnvironmentsForProject \(no single-caller compile/build wrappers\).

Golden \(4 variants\) and catalog reference-SQL parity are unchanged, proving
the injected SQL is byte-identical to the previous hand-written form.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* test\(query-ast\): prove ARRAY JOIN / LIMIT BY compose when nested

The plugin-based clauses were only proven at the top level. Add a regression
suite asserting they survive embedding as a CTE body, a subquery, and a
virtual-view body \(ARRAY JOIN\) and a CTE body \(LIMIT BY\). Asserts on raw
compiler output so it runs in CI without a `clickhouse` binary.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(query-ast\): curry arrayJoin/limitBy as $call helpers with typed rows

Replace the withArrayJoin\(qb, …\) / withLimitBy\(qb, …\) wrappers with curried
helpers applied via Kysely's public `$call`, so ClickHouse-only clauses chain
inline and compose in any nested position \(CTE, subquery, view\) without a
Kysely fork or prototype patch.

Type inference:
- arrayJoin takes an { alias: arrayExpression } record and widens the builder's
  output row type with each alias, so downstream consumers \(e.g. an outer query
  over a CTE body\) can reference the produced columns and alias typos are
  compile errors. Element value types stay opaque \(Kysely's Expression hides its
  type arg\), so aliases surface as unknown.
- limitBy preserves the row type.
- mapKeys/mapValues now return typed array Expressions so they compose in typed
  expression positions.

Compile-time assertions for the widening live in types.assert.ts. Golden and
catalog reference-SQL parity are unchanged \(byte-identical output\).

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): add kysely/ authoring guide for the unusual patterns

Document the folder's non-obvious conventions for future editors: compile-only
dialect \(never executes\), injected tenancy \(never write project_id\), the
$call-curried-helper pattern for ARRAY JOIN / LIMIT BY and why it beats a
fluent method or a fork, the row-type widening and its unknown-element caveat,
the sql.ref / eb.fn escape hatches and their cost, the single Kysely-internals
coupling and version pin, and how to verify changes. Cross-links the parent
README for node mechanics and the tenancy choke point rather than duplicating.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(query-ast\): make ExecutionContext mandatory at compile time

The compile choke point declared `ctx?: ExecutionContext` and relied
solely on a runtime check, which mis-signalled the contract to callers
and contradicted the function's own "ExecutionContext is mandatory"
doc. `projectId` is already a non-nullable `string`, so dropping the
`?` lets the type system reject a missing context up front. The runtime
check stays as defense-in-depth for the one case the type cannot
express \(an empty `projectId`\) and for `any`-boundary callers.

Prove both contracts in the test: `@ts-expect-error` asserts the
compile-time rejection, a separate case asserts the empty-projectId
runtime throw.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* fix\(query-ast\): scope aliased relation when its alias collides with another table name

The tenancy coverage check accepted a qualified `project_id` predicate as
covering a relation when the qualifier matched *either* the relation's
physical table name or its alias, while the injection side only ever
qualifies by the alias when one is present. When one tenanted relation is
aliased to another tenanted table's physical name \(e.g. `scores AS traces`
joined to `traces AS t`\), a `traces.project_id = {p}` predicate — which
scopes the alias only — was counted as covering the physically-`traces`
relation too, so that relation received no project constraint and could
contribute cross-project rows to the join.

Mirror `projectIdPredicate`: a qualified predicate covers a relation only
when the qualifier matches its alias \(if aliased\) or its table name \(if
not\). Add a regression test that fails against the previous logic.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): clarify compiler comments and strengthen ARRAY JOIN CTE test

Address review feedback on the Kysely ClickHouse compiler:
- compile.ts: rewrite the choke-point JSDoc to state the now-mandatory
  `ctx` contract \(compile-time type error when omitted; runtime guard is
  defense-in-depth for empty projectId / any-boundary callers\).
- compiler.ts: add a worked example for parameter interning; explain why
  ARRAY JOIN / LIMIT BY ride as real operation nodes; justify the private
  visitNode/nodeStack patch and record why it cannot fail silently on a
  Kysely bump \(exact version pin + SQL-asserting compile tests\); document
  the unquoted-identifier overrides; reword the obscure "golden scores
  query" reference; note what visitSelectQuery changes vs the parent; and
  explain the bind-type inference/width trade-off.
- composition.test.ts: assert the ARRAY JOIN stays inside the CTE body and
  does not leak into the outer query, instead of only asserting presence.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): document extensions module and its AST-shape tests

Address review feedback that extensions.ts and its tests are hard to
follow from excerpts:
- extensions.ts: add a file header explaining the module supplies
  ClickHouse constructs Kysely lacks, in three forms \(node-attaching
  plugins, type-extending $call helpers, and parameter-bound expression
  helpers\).
- extensions.test.ts: explain the two-layer test strategy — the first
  metadata test asserts the lowered AST shape \(real ArrayIndexNode +
  indexOf FunctionNode, never a RawNode\) while the siblings assert the
  emitted SQL — and why the first selection's inner node is an AliasNode.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): explain why columnRef reimplements dotted-reference parsing

Kysely parses "table.column" only at the expression-builder layer and does
not export its string-reference parser, so these node-level plugins/helpers
split the dotted name into ReferenceNode/ColumnNode/TableNode themselves.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(query-ast\): build metadata refs directly instead of via a string

metadataValue held the table alias and column names separately, then
concatenated them into "alias.column" only for columnRef to split them
back apart. Add qualifiedColumn\(table, column\) to build the ReferenceNode
straight from the parts and use it in metadataValue; columnRef now
delegates to it and remains for genuinely-string inputs \(limitBy columns\).

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): explain the ElementOf type utility

Document that ElementOf extracts T from an Expression<T\[\]> and why arrayJoin
needs it — ARRAY JOIN yields one row per element, so each produced alias
carries the array's element type in the builder output row.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): tighten compiler comments per review

- interning example moved to its own paragraph in the class JSDoc
- drop the reader-rationale prose on ARRAY JOIN / LIMIT BY; just state they
  ride as real operation nodes
- shorten the visitNode-patch block and put an explicit anti-pattern note at
  the first private-member cast rather than only in the paragraph above

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): split READMEs into using \(parent\) vs developing \(kysely\)

Consolidate per review: the parent README is now a usage guide \(compile
entrypoint, the no-project_id contract, $call clauses\) and points to the
kysely README for internals. The kysely README absorbs the extension-record
table and the full tenancy-injection mechanics. Trim the compiler's
visitNode-patch comment to point at the README's upgrade-hazard section.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): default tenant to true, clarify events_full, add clause recipes

- schema.ts: make defineTable's tenant flag optional, defaulting to true —
  every modeled relation is project-gated and the default is fail-closed.
- schema.test.ts: clarify events_full is as project-gated as events_core; it is
  absent only because the builder targets events_core, not because it is exempt.
- README: add findable ARRAY JOIN / LIMIT BY / metadata-select recipes.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(query-ast\): apply low-risk tenancy/compiler simplifications

Remove the redundant raw-relation branch in describeRelation \(the
fallthrough already returns the identical value\), drop the throwaway
node copy and index-map in injectSelect, inline the single-use
isTenancyStamped, and collapse the compiler's triple `this as unknown`
casts into one named alias.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(query-ast\): address review comments and fix knip

Fix knip CI failure by dropping the unused `export` on the file-local
CatalogTier and ArrayJoinItemNode types. Add an anti-pattern comment on
the ArrayIndexNode cast in the transformer, rename condition7TypeAssertions
to schemaTypeAssertions with a self-explanatory doc comment, and trim the
tenancy comment in environments.ts to behavior-only prose.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* test\(query-ast\): run catalog parity in the SQL-equivalence CI step

Rename catalog.test.ts to catalog.golden.test.ts so CI's non-blocking
SQL-equivalence step \(which installs the pinned clickhouse binary and
selects tests by the `.golden.test` name\) actually runs the
compile\(AST\) ≡ referenceSQL parity checks. As catalog.test.ts it ran in
neither job: the main run skips it \(no clickhouse binary yet\) and the
golden step's name filter did not match it. Update the kysely README to
match and drop the now-false "does not yet run in CI" note.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

---------

Co-authored-by: Cursor Agent <cursoragent@cursor.com>
Co-authored-by: Claude Opus 4.8 <noreply@anthropic.com>")[#16687](https://github.com/langfuse/langfuse/pull/16687)[)](https://github.com/langfuse/langfuse/commit/8521ece6c8d5bd828e9492af903d61db4bf13878 "feat\(query-ast\): clean Kysely base for the ClickHouse query AST \(#16687\)

* chore\(deps\): add kysely to shared

Compile-only ClickHouse dialect for the query-builder spike. Pin 0.28.17 so shared can keep emitting CJS; 0.29 is ESM-only.

* feat\(query-ast\): compile ClickHouse queries through Kysely

Replace the environments repository string templates with traced Kysely
nodes. Add a compile-only ClickHouse dialect, real ARRAY JOIN and LIMIT BY
nodes via plugin plus transformer \(no fork\), mandatory ExecutionContext
tenancy injection, and a frozen catalog that checks compile\(AST\) matches
reference SQL after clickhouse format.

* feat\(query-ast\): add typed selection, metadata indexOf, and virtual views

Move Kysely-specific code under query-ast/kysely/. Catch sum\(\) over String
at a runtime validation pass \(DateTime vs Int is a TS error\). Lower
metadata\[key\] to ArrayIndexNode plus indexOf with a bound key. Expose a
named catalog query as a WITH view so the outer query only sees declared
columns, with tenancy still injected on the inner table.

* fix\(query-ast\): identity-stamp tenancy and reject raw relation SQL

Close the Kysely compile-path escape hatches without a fork: stamp
injected trees by identity instead of a copyable node property, and
reject raw fragments that embed SELECT/FROM/JOIN in any position.
Prove JOIN project_id injection with table-qualified SQL and
clickhouse-local EXPLAIN.

* fix\(query-ast\): make tenancy injection prove the scoped project

The tenancy pass treated any `project_id = …` predicate as already
scoping a relation without checking the bound value or the qualified
table. A query carrying `project_id = <otherProject>` was accepted with
no corrective predicate, and one unqualified `project_id` was treated as
covering every tenanted table in a JOIN — leaking sibling relations.

`predicateCovers` now requires the equality's value to equal the
ExecutionContext project, and an unqualified column only counts when a
single tenanted relation is in scope; otherwise a table-qualified
reference is required. Drops the no-op branch in `assertRelationAllowed`.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(query-ast\): derive schema facts from one table registry

Ports the semantic-model idea from the hand-rolled spike arm: a single
`defineTable` declaration per relation is now the source of truth for the
Kysely row types \(`ClickHouseDatabase`\), the runtime column-type map the
aggregate type-check pass consults \(`COLUMN_DATA_TYPES`\), and the set the
tenancy pass scopes \(`TENANTED_TABLES`\) — instead of three hand-maintained
tables that drift.

Drops the unreachable `events` / `events_full` entries from the tenanted
set: they are not in the row type and raw sources are rejected, so no
query could ever reference them. Also documents the compiler's
Kysely-private `visitNode` coupling as a known fragility.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* fix\(query-ast\): re-export formatSql after rebase onto main

The rebase onto main silently dropped the `export` keyword restored by
the Kysely arm, leaving catalog.test.ts importing an undefined
`formatSql`. Restores the export; catalog parity is green again.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): trim provenance from schema registry comment

Drop change/attempt-scoped wording that goes stale once merged; keep the
why \(single source of truth\) and the forward note on physical metadata.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* fix\(query-ast\): satisfy knip and codespell on the Kysely base

- Drop `export` from six symbols used only within their own module
  \(inferClickHouseType, ArrayJoin/LimitBy/VirtualView plugins, stampTenancy,
  isTenancyStamped\); the public wrappers remain exported.
- Anchor the compile-time type-assertion file from schema.test.ts so knip
  keeps it in the build graph without a suppression config.
- Allow `froms` \(Kysely's SelectQueryNode field name\) in codespell.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): drop library-evaluation provenance from code comments

Remove "record for the evaluation / no fork" framing from the plugin,
view, catalog, and type-assertion doc comments; keep the mechanism and
tenancy/gotcha notes that describe the code itself.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(query-ast\): colocate the environments query with its repository

Move the tracing/scores query builders and their compile step out of the
generic query-ast library and into repositories/environments.ts, next to
getEnvironmentsForProject, as file-local functions. Keeps the pure
input->{sql,params} seam \(still unit-testable, golden test unchanged\) while
removing the cross-file jump and the split write-mode branching. The
query-ast/kysely library keeps only generic internals \(nodes, compiler,
plugins, dialect, tenancy, catalog\); per-call-site queries live with their
call site now that the Kysely builder is expected to spread into repositories.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(query-ast\): rely on injected tenancy instead of manual project_id

Drop the hand-written `project_id = <id>` predicate from the environments
call site and every catalog entry, and lean on the mandatory tenancy pass to
inject it from the ExecutionContext. The injector now prepends the scope as
the leading WHERE predicate so the bound value keeps a stable first parameter
position; JOIN-ON scoping stays appended after the join conditions.

Also inline the environments query construction directly into
getEnvironmentsForProject \(no single-caller compile/build wrappers\).

Golden \(4 variants\) and catalog reference-SQL parity are unchanged, proving
the injected SQL is byte-identical to the previous hand-written form.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* test\(query-ast\): prove ARRAY JOIN / LIMIT BY compose when nested

The plugin-based clauses were only proven at the top level. Add a regression
suite asserting they survive embedding as a CTE body, a subquery, and a
virtual-view body \(ARRAY JOIN\) and a CTE body \(LIMIT BY\). Asserts on raw
compiler output so it runs in CI without a `clickhouse` binary.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(query-ast\): curry arrayJoin/limitBy as $call helpers with typed rows

Replace the withArrayJoin\(qb, …\) / withLimitBy\(qb, …\) wrappers with curried
helpers applied via Kysely's public `$call`, so ClickHouse-only clauses chain
inline and compose in any nested position \(CTE, subquery, view\) without a
Kysely fork or prototype patch.

Type inference:
- arrayJoin takes an { alias: arrayExpression } record and widens the builder's
  output row type with each alias, so downstream consumers \(e.g. an outer query
  over a CTE body\) can reference the produced columns and alias typos are
  compile errors. Element value types stay opaque \(Kysely's Expression hides its
  type arg\), so aliases surface as unknown.
- limitBy preserves the row type.
- mapKeys/mapValues now return typed array Expressions so they compose in typed
  expression positions.

Compile-time assertions for the widening live in types.assert.ts. Golden and
catalog reference-SQL parity are unchanged \(byte-identical output\).

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): add kysely/ authoring guide for the unusual patterns

Document the folder's non-obvious conventions for future editors: compile-only
dialect \(never executes\), injected tenancy \(never write project_id\), the
$call-curried-helper pattern for ARRAY JOIN / LIMIT BY and why it beats a
fluent method or a fork, the row-type widening and its unknown-element caveat,
the sql.ref / eb.fn escape hatches and their cost, the single Kysely-internals
coupling and version pin, and how to verify changes. Cross-links the parent
README for node mechanics and the tenancy choke point rather than duplicating.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(query-ast\): make ExecutionContext mandatory at compile time

The compile choke point declared `ctx?: ExecutionContext` and relied
solely on a runtime check, which mis-signalled the contract to callers
and contradicted the function's own "ExecutionContext is mandatory"
doc. `projectId` is already a non-nullable `string`, so dropping the
`?` lets the type system reject a missing context up front. The runtime
check stays as defense-in-depth for the one case the type cannot
express \(an empty `projectId`\) and for `any`-boundary callers.

Prove both contracts in the test: `@ts-expect-error` asserts the
compile-time rejection, a separate case asserts the empty-projectId
runtime throw.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* fix\(query-ast\): scope aliased relation when its alias collides with another table name

The tenancy coverage check accepted a qualified `project_id` predicate as
covering a relation when the qualifier matched *either* the relation's
physical table name or its alias, while the injection side only ever
qualifies by the alias when one is present. When one tenanted relation is
aliased to another tenanted table's physical name \(e.g. `scores AS traces`
joined to `traces AS t`\), a `traces.project_id = {p}` predicate — which
scopes the alias only — was counted as covering the physically-`traces`
relation too, so that relation received no project constraint and could
contribute cross-project rows to the join.

Mirror `projectIdPredicate`: a qualified predicate covers a relation only
when the qualifier matches its alias \(if aliased\) or its table name \(if
not\). Add a regression test that fails against the previous logic.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): clarify compiler comments and strengthen ARRAY JOIN CTE test

Address review feedback on the Kysely ClickHouse compiler:
- compile.ts: rewrite the choke-point JSDoc to state the now-mandatory
  `ctx` contract \(compile-time type error when omitted; runtime guard is
  defense-in-depth for empty projectId / any-boundary callers\).
- compiler.ts: add a worked example for parameter interning; explain why
  ARRAY JOIN / LIMIT BY ride as real operation nodes; justify the private
  visitNode/nodeStack patch and record why it cannot fail silently on a
  Kysely bump \(exact version pin + SQL-asserting compile tests\); document
  the unquoted-identifier overrides; reword the obscure "golden scores
  query" reference; note what visitSelectQuery changes vs the parent; and
  explain the bind-type inference/width trade-off.
- composition.test.ts: assert the ARRAY JOIN stays inside the CTE body and
  does not leak into the outer query, instead of only asserting presence.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): document extensions module and its AST-shape tests

Address review feedback that extensions.ts and its tests are hard to
follow from excerpts:
- extensions.ts: add a file header explaining the module supplies
  ClickHouse constructs Kysely lacks, in three forms \(node-attaching
  plugins, type-extending $call helpers, and parameter-bound expression
  helpers\).
- extensions.test.ts: explain the two-layer test strategy — the first
  metadata test asserts the lowered AST shape \(real ArrayIndexNode +
  indexOf FunctionNode, never a RawNode\) while the siblings assert the
  emitted SQL — and why the first selection's inner node is an AliasNode.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): explain why columnRef reimplements dotted-reference parsing

Kysely parses "table.column" only at the expression-builder layer and does
not export its string-reference parser, so these node-level plugins/helpers
split the dotted name into ReferenceNode/ColumnNode/TableNode themselves.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(query-ast\): build metadata refs directly instead of via a string

metadataValue held the table alias and column names separately, then
concatenated them into "alias.column" only for columnRef to split them
back apart. Add qualifiedColumn\(table, column\) to build the ReferenceNode
straight from the parts and use it in metadataValue; columnRef now
delegates to it and remains for genuinely-string inputs \(limitBy columns\).

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): explain the ElementOf type utility

Document that ElementOf extracts T from an Expression<T\[\]> and why arrayJoin
needs it — ARRAY JOIN yields one row per element, so each produced alias
carries the array's element type in the builder output row.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): tighten compiler comments per review

- interning example moved to its own paragraph in the class JSDoc
- drop the reader-rationale prose on ARRAY JOIN / LIMIT BY; just state they
  ride as real operation nodes
- shorten the visitNode-patch block and put an explicit anti-pattern note at
  the first private-member cast rather than only in the paragraph above

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): split READMEs into using \(parent\) vs developing \(kysely\)

Consolidate per review: the parent README is now a usage guide \(compile
entrypoint, the no-project_id contract, $call clauses\) and points to the
kysely README for internals. The kysely README absorbs the extension-record
table and the full tenancy-injection mechanics. Trim the compiler's
visitNode-patch comment to point at the README's upgrade-hazard section.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* docs\(query-ast\): default tenant to true, clarify events_full, add clause recipes

- schema.ts: make defineTable's tenant flag optional, defaulting to true —
  every modeled relation is project-gated and the default is fail-closed.
- schema.test.ts: clarify events_full is as project-gated as events_core; it is
  absent only because the builder targets events_core, not because it is exempt.
- README: add findable ARRAY JOIN / LIMIT BY / metadata-select recipes.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(query-ast\): apply low-risk tenancy/compiler simplifications

Remove the redundant raw-relation branch in describeRelation \(the
fallthrough already returns the identical value\), drop the throwaway
node copy and index-map in injectSelect, inline the single-use
isTenancyStamped, and collapse the compiler's triple `this as unknown`
casts into one named alias.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(query-ast\): address review comments and fix knip

Fix knip CI failure by dropping the unused `export` on the file-local
CatalogTier and ArrayJoinItemNode types. Add an anti-pattern comment on
the ArrayIndexNode cast in the transformer, rename condition7TypeAssertions
to schemaTypeAssertions with a self-explanatory doc comment, and trim the
tenancy comment in environments.ts to behavior-only prose.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* test\(query-ast\): run catalog parity in the SQL-equivalence CI step

Rename catalog.test.ts to catalog.golden.test.ts so CI's non-blocking
SQL-equivalence step \(which installs the pinned clickhouse binary and
selects tests by the `.golden.test` name\) actually runs the
compile\(AST\) ≡ referenceSQL parity checks. As catalog.test.ts it ran in
neither job: the main run skips it \(no clickhouse binary yet\) and the
golden step's name filter did not match it. Update the kysely README to
match and drop the now-false "does not yet run in CI" note.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

---------

Co-authored-by: Cursor Agent <cursoragent@cursor.com>
Co-authored-by: Claude Opus 4.8 <noreply@anthropic.com>")  | Aug 28, 2026  |  
|   |   |  [feat(worker): add Rust native addon scaffold (@langfuse/native) (](https://github.com/langfuse/langfuse/commit/1a2d1f21157ed7dfde63a4b332c5728d924ed557 "feat\(worker\): add Rust native addon scaffold \(@langfuse/native\) \(#17062\)

* feat\(worker\): add Rust native addon scaffold \(@langfuse/native\)

Add packages/native, a napi-rs crate the worker loads in-process, with a
hello-world entry point that reports through the addon's own telemetry:
DogStatsD metrics to the Datadog Agent dd-trace already uses, and logs to
stdout in the worker logger's shape. The worker calls it at startup and on
every health probe, so a broken addon stops the worker or fails the probe.

Build with cargo when installed; otherwise cross-compile for the host
platform \(macOS included\) inside a pinned builder container. The worker
image compiles the crate with Alpine's Rust. CI runners get the pinned
toolchain from rustup auto-install; the addon's rustfmt and clippy run as a
dedicated step in the lint job because cargo rejects the ESLint flag the
turbo lint invocation forwards.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs\(native\): fix spelling flagged by codespell

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs\(native\): state that the docker build strategy covers macOS and Linux only

Windows hosts without cargo were sent down the docker path and failed on
target selection. Name the limitation where it fails and in the docs, and
point at rustup or WSL2.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* fix\(native\): log hello calls at debug level

The worker calls hello\(\) on every health probe, so an info-level line per
probe would add permanent log volume on every replica. The counter remains
the heartbeat; the log line stays below the default level.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* chore\(native\): drop the Docker build strategy

Rust via rustup is now a repository prerequisite \(the AI gateway starts
with `pnpm dev`\), so the cross-compile fallback for developers without a
toolchain no longer earns its complexity. The package builds with
`napi build` directly again. Each Rust crate keeps its own
rust-toolchain.toml.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* chore\(native\): pin Rust 1.98.0 like ai-gateway and build the worker image with it

The two crates keep separate toolchain files but start on the same version.
The worker image now copies that exact toolchain from the official
rust:<version>-alpine image instead of using Alpine's packaged Rust, so
production, CI, and laptops compile the addon with one compiler. rustc's
musl targets expect a linker name Alpine does not ship, so cargo is pointed
at cc.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

---------

Co-authored-by: Claude Fable 5.1 <noreply@anthropic.com>")[#17062](https://github.com/langfuse/langfuse/pull/17062)[)](https://github.com/langfuse/langfuse/commit/1a2d1f21157ed7dfde63a4b332c5728d924ed557 "feat\(worker\): add Rust native addon scaffold \(@langfuse/native\) \(#17062\)

* feat\(worker\): add Rust native addon scaffold \(@langfuse/native\)

Add packages/native, a napi-rs crate the worker loads in-process, with a
hello-world entry point that reports through the addon's own telemetry:
DogStatsD metrics to the Datadog Agent dd-trace already uses, and logs to
stdout in the worker logger's shape. The worker calls it at startup and on
every health probe, so a broken addon stops the worker or fails the probe.

Build with cargo when installed; otherwise cross-compile for the host
platform \(macOS included\) inside a pinned builder container. The worker
image compiles the crate with Alpine's Rust. CI runners get the pinned
toolchain from rustup auto-install; the addon's rustfmt and clippy run as a
dedicated step in the lint job because cargo rejects the ESLint flag the
turbo lint invocation forwards.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs\(native\): fix spelling flagged by codespell

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs\(native\): state that the docker build strategy covers macOS and Linux only

Windows hosts without cargo were sent down the docker path and failed on
target selection. Name the limitation where it fails and in the docs, and
point at rustup or WSL2.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* fix\(native\): log hello calls at debug level

The worker calls hello\(\) on every health probe, so an info-level line per
probe would add permanent log volume on every replica. The counter remains
the heartbeat; the log line stays below the default level.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* chore\(native\): drop the Docker build strategy

Rust via rustup is now a repository prerequisite \(the AI gateway starts
with `pnpm dev`\), so the cross-compile fallback for developers without a
toolchain no longer earns its complexity. The package builds with
`napi build` directly again. Each Rust crate keeps its own
rust-toolchain.toml.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* chore\(native\): pin Rust 1.98.0 like ai-gateway and build the worker image with it

The two crates keep separate toolchain files but start on the same version.
The worker image now copies that exact toolchain from the official
rust:<version>-alpine image instead of using Alpine's packaged Rust, so
production, CI, and laptops compile the addon with one compiler. rustc's
musl targets expect a linker name Alpine does not ship, so cargo is pointed
at cc.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

---------

Co-authored-by: Claude Fable 5.1 <noreply@anthropic.com>")  | Sep 15, 2026  |  
|   |   | [feat(v4): promote events tables to CH migration and flip v4 env defau…](https://github.com/langfuse/langfuse/commit/eaa34c3e7fd8e75952ed72dc059982b0397fe69c "feat\(v4\): promote events tables to CH migration and flip v4 env defaults \(#14812\)

* feat\(v4\): promote events tables to CH migration and flip v4 env defaults

Prepare the Langfuse v4 preview line.

- Add migration 0036_add_v4_events_tables \(clustered + unclustered, up + down\)
  promoting observations_batch_staging, events_full, events_core and
  events_core_mv out of clickhouse/scripts/dev-tables.sh, now that v4 raises the
  minimum ClickHouse version to 25.12 \(block_number columns >= 24.5, text
  indexes >= 25.x\). Folds in the ingestion_api_key / ingestion_sdk_name /
  ingestion_sdk_version columns \(previously added via dev-tables.sh ALTERs,
  #14593\). Uses CREATE ... IF NOT EXISTS so preview self-hosters that applied
  the manual schema upgrade cleanly.
- Flip the v4 env defaults to the target state in worker, shared and web:
  LANGFUSE_MIGRATION_V4_WRITE_MODE=events_only,
  LANGFUSE_MIGRATION_V4_NATIVE_OTEL_BEHAVIOUR=direct,
  LANGFUSE_MIGRATION_V4_ALLOW_PREVIEW_OPT_IN=true,
  LANGFUSE_BACKGROUND_MIGRATION_V4_ENABLE_HISTORIC_BACKFILL=true. These couple
  via validateV4Flags; the DROP_PID_TID cleanup stays opt-in.
- Pin .env.dev*.example \(all four deploy modes\) to the dual-write dev/CI config
  with the historic backfill held off, so the flipped defaults only affect bare
  net-new deployments and do not destabilise the deploy-mode CI matrix.
- Trim dev-tables.sh to Cloud-only diagnostics \(ingestion_size_stats + MVs,
  analytics_events_core, ngram indexes\) and local sample-data seeding.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(v4\): split events-table migration into one DDL per migration

Break the monolithic 0036_add_v4_events_tables into per-statement migrations so
each DDL change is tracked individually, mirroring how the self-hosted events
rollout doc records the initial schema plus each subsequent change:

- 0036_create_observations_batch_staging
- 0037_create_events_full
- 0038_create_events_core
- 0039_create_events_core_mv
- 0040_add_events_ingestion_attribution_columns

The ingestion_api_key / ingestion_sdk_name / ingestion_sdk_version columns
\(#14593\) are no longer folded into the initial CREATEs; they live in their own
0040 migration \(ADD COLUMN on the three tables + events_core_mv MODIFY QUERY\),
matching the doc's "added later" entry. Each migration has matching up/down in
both clustered \(ON CLUSTER default + Replicated*, alter_sync=2 on column ops\)
and unclustered variants; down migrations revert the MV projection before
dropping the columns it references.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* chore: comments and cleanups

* feat\(v4\): add events metadata ngram index as its own migration \(0041\)

The idx_fts_metadata_values_ngram index \(ngrambf_v1 over arrayStringConcat
\(metadata_values\)\) accelerates the `contains` / `starts with` / `ends with`
metadata filter operators on the events tables. It was added by #14133, which
also wired the shared clickhouse filter builder to emit an ngram-targeted
prefilter — like\(arrayStringConcat\(metadata_values\), '%value%'\) — for those
operators. That prefilter is not deployment-gated, so self-hosted v4 emits it
regardless; without the index the prefilter is pure per-row overhead. So the
index must ship with the events tables, not stay dev/Cloud-only.

Promote it out of clickhouse/scripts/dev-tables.sh into its own migration
0041_add_events_metadata_ngram_index \(clustered: ON CLUSTER default +
alter_sync=2; down drops the index\), consistent with how 0040 handles the
ingestion attribution columns.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* chore\(v4\): bump all dev/CI ClickHouse to 25.12, drop 24.3 testing

v4 makes ClickHouse 25.12 the minimum \(the events tables need the >=25.x text
indexes / enable_full_text_index used by migrations 0037/0041\). The -azure and
-redis-cluster deploy-mode configs were still on 24.3 to test old-version
behaviour, but those CI legs run `ch:up` unconditionally and would fail on the
events-table migrations. Bump both to 25.12 to match docker-compose.dev.yml and
update its now-stale rationale comment. No more 24.3 testing.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* fix\(ci\): pin v4 dual-write config in all dev env examples

The deploy-mode CI matrix copies .env.dev*.example verbatim; without explicit
pins the flipped v4 defaults \(events_only\) apply and legacy-table test reads
come up empty. Restores the pin in .env.dev.example \(dropped in 8a342ef96\) and
adds the missing blocks to the redis-cluster and oci examples.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>

* fix\(worker\): keep event-propagation health gate active in events_only mode

isEventPropagationEnabled only fired for WRITE_MODE=dual while the worker
registers the propagation queue via v4WritesToEventsTable \(dual OR
events_only\). With the flipped events_only default the
?failIfEventPropagationStuck liveness probe silently reported disabled and
could never trigger a restart. Mirror the app.ts registration guard.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>

* chore\(clickhouse\): warn about destructive rollback in v4 events down migrations

The up migrations use IF NOT EXISTS, so on preview deployments the tables may
predate the migration \(created via dev-tables.sh\) and hold production-grade
data the rollback would drop.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>

* chore\(clickhouse\): renumber v4 events migrations to 0038-0043 after main's 0037

main landed 0037_add_created_at_indexes \(#15368\) while the branch was in
review, colliding with 0037_create_observations_batch_staging —
golang-migrate errors on duplicate versions. Pure renames, no SQL changes.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>

---------

Co-authored-by: Claude Opus 4.8 <noreply@anthropic.com>")  | Jul 23, 2026  |  
|   |   | [feat(v4): promote events tables to CH migration and flip v4 env defau…](https://github.com/langfuse/langfuse/commit/eaa34c3e7fd8e75952ed72dc059982b0397fe69c "feat\(v4\): promote events tables to CH migration and flip v4 env defaults \(#14812\)

* feat\(v4\): promote events tables to CH migration and flip v4 env defaults

Prepare the Langfuse v4 preview line.

- Add migration 0036_add_v4_events_tables \(clustered + unclustered, up + down\)
  promoting observations_batch_staging, events_full, events_core and
  events_core_mv out of clickhouse/scripts/dev-tables.sh, now that v4 raises the
  minimum ClickHouse version to 25.12 \(block_number columns >= 24.5, text
  indexes >= 25.x\). Folds in the ingestion_api_key / ingestion_sdk_name /
  ingestion_sdk_version columns \(previously added via dev-tables.sh ALTERs,
  #14593\). Uses CREATE ... IF NOT EXISTS so preview self-hosters that applied
  the manual schema upgrade cleanly.
- Flip the v4 env defaults to the target state in worker, shared and web:
  LANGFUSE_MIGRATION_V4_WRITE_MODE=events_only,
  LANGFUSE_MIGRATION_V4_NATIVE_OTEL_BEHAVIOUR=direct,
  LANGFUSE_MIGRATION_V4_ALLOW_PREVIEW_OPT_IN=true,
  LANGFUSE_BACKGROUND_MIGRATION_V4_ENABLE_HISTORIC_BACKFILL=true. These couple
  via validateV4Flags; the DROP_PID_TID cleanup stays opt-in.
- Pin .env.dev*.example \(all four deploy modes\) to the dual-write dev/CI config
  with the historic backfill held off, so the flipped defaults only affect bare
  net-new deployments and do not destabilise the deploy-mode CI matrix.
- Trim dev-tables.sh to Cloud-only diagnostics \(ingestion_size_stats + MVs,
  analytics_events_core, ngram indexes\) and local sample-data seeding.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(v4\): split events-table migration into one DDL per migration

Break the monolithic 0036_add_v4_events_tables into per-statement migrations so
each DDL change is tracked individually, mirroring how the self-hosted events
rollout doc records the initial schema plus each subsequent change:

- 0036_create_observations_batch_staging
- 0037_create_events_full
- 0038_create_events_core
- 0039_create_events_core_mv
- 0040_add_events_ingestion_attribution_columns

The ingestion_api_key / ingestion_sdk_name / ingestion_sdk_version columns
\(#14593\) are no longer folded into the initial CREATEs; they live in their own
0040 migration \(ADD COLUMN on the three tables + events_core_mv MODIFY QUERY\),
matching the doc's "added later" entry. Each migration has matching up/down in
both clustered \(ON CLUSTER default + Replicated*, alter_sync=2 on column ops\)
and unclustered variants; down migrations revert the MV projection before
dropping the columns it references.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* chore: comments and cleanups

* feat\(v4\): add events metadata ngram index as its own migration \(0041\)

The idx_fts_metadata_values_ngram index \(ngrambf_v1 over arrayStringConcat
\(metadata_values\)\) accelerates the `contains` / `starts with` / `ends with`
metadata filter operators on the events tables. It was added by #14133, which
also wired the shared clickhouse filter builder to emit an ngram-targeted
prefilter — like\(arrayStringConcat\(metadata_values\), '%value%'\) — for those
operators. That prefilter is not deployment-gated, so self-hosted v4 emits it
regardless; without the index the prefilter is pure per-row overhead. So the
index must ship with the events tables, not stay dev/Cloud-only.

Promote it out of clickhouse/scripts/dev-tables.sh into its own migration
0041_add_events_metadata_ngram_index \(clustered: ON CLUSTER default +
alter_sync=2; down drops the index\), consistent with how 0040 handles the
ingestion attribution columns.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* chore\(v4\): bump all dev/CI ClickHouse to 25.12, drop 24.3 testing

v4 makes ClickHouse 25.12 the minimum \(the events tables need the >=25.x text
indexes / enable_full_text_index used by migrations 0037/0041\). The -azure and
-redis-cluster deploy-mode configs were still on 24.3 to test old-version
behaviour, but those CI legs run `ch:up` unconditionally and would fail on the
events-table migrations. Bump both to 25.12 to match docker-compose.dev.yml and
update its now-stale rationale comment. No more 24.3 testing.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* fix\(ci\): pin v4 dual-write config in all dev env examples

The deploy-mode CI matrix copies .env.dev*.example verbatim; without explicit
pins the flipped v4 defaults \(events_only\) apply and legacy-table test reads
come up empty. Restores the pin in .env.dev.example \(dropped in 8a342ef96\) and
adds the missing blocks to the redis-cluster and oci examples.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>

* fix\(worker\): keep event-propagation health gate active in events_only mode

isEventPropagationEnabled only fired for WRITE_MODE=dual while the worker
registers the propagation queue via v4WritesToEventsTable \(dual OR
events_only\). With the flipped events_only default the
?failIfEventPropagationStuck liveness probe silently reported disabled and
could never trigger a restart. Mirror the app.ts registration guard.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>

* chore\(clickhouse\): warn about destructive rollback in v4 events down migrations

The up migrations use IF NOT EXISTS, so on preview deployments the tables may
predate the migration \(created via dev-tables.sh\) and hold production-grade
data the rollback would drop.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>

* chore\(clickhouse\): renumber v4 events migrations to 0038-0043 after main's 0037

main landed 0037_add_created_at_indexes \(#15368\) while the branch was in
review, colliding with 0037_create_observations_batch_staging —
golang-migrate errors on duplicate versions. Pure renames, no SQL changes.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>

---------

Co-authored-by: Claude Opus 4.8 <noreply@anthropic.com>")  | Jul 23, 2026  |  
| [.env.dev-redis-cluster.example](https://github.com/langfuse/langfuse/blob/main/.env.dev-redis-cluster.example ".env.dev-redis-cluster.example")  | [.env.dev-redis-cluster.example](https://github.com/langfuse/langfuse/blob/main/.env.dev-redis-cluster.example ".env.dev-redis-cluster.example")  | [feat(v4): promote events tables to CH migration and flip v4 env defau…](https://github.com/langfuse/langfuse/commit/eaa34c3e7fd8e75952ed72dc059982b0397fe69c "feat\(v4\): promote events tables to CH migration and flip v4 env defaults \(#14812\)

* feat\(v4\): promote events tables to CH migration and flip v4 env defaults

Prepare the Langfuse v4 preview line.

- Add migration 0036_add_v4_events_tables \(clustered + unclustered, up + down\)
  promoting observations_batch_staging, events_full, events_core and
  events_core_mv out of clickhouse/scripts/dev-tables.sh, now that v4 raises the
  minimum ClickHouse version to 25.12 \(block_number columns >= 24.5, text
  indexes >= 25.x\). Folds in the ingestion_api_key / ingestion_sdk_name /
  ingestion_sdk_version columns \(previously added via dev-tables.sh ALTERs,
  #14593\). Uses CREATE ... IF NOT EXISTS so preview self-hosters that applied
  the manual schema upgrade cleanly.
- Flip the v4 env defaults to the target state in worker, shared and web:
  LANGFUSE_MIGRATION_V4_WRITE_MODE=events_only,
  LANGFUSE_MIGRATION_V4_NATIVE_OTEL_BEHAVIOUR=direct,
  LANGFUSE_MIGRATION_V4_ALLOW_PREVIEW_OPT_IN=true,
  LANGFUSE_BACKGROUND_MIGRATION_V4_ENABLE_HISTORIC_BACKFILL=true. These couple
  via validateV4Flags; the DROP_PID_TID cleanup stays opt-in.
- Pin .env.dev*.example \(all four deploy modes\) to the dual-write dev/CI config
  with the historic backfill held off, so the flipped defaults only affect bare
  net-new deployments and do not destabilise the deploy-mode CI matrix.
- Trim dev-tables.sh to Cloud-only diagnostics \(ingestion_size_stats + MVs,
  analytics_events_core, ngram indexes\) and local sample-data seeding.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(v4\): split events-table migration into one DDL per migration

Break the monolithic 0036_add_v4_events_tables into per-statement migrations so
each DDL change is tracked individually, mirroring how the self-hosted events
rollout doc records the initial schema plus each subsequent change:

- 0036_create_observations_batch_staging
- 0037_create_events_full
- 0038_create_events_core
- 0039_create_events_core_mv
- 0040_add_events_ingestion_attribution_columns

The ingestion_api_key / ingestion_sdk_name / ingestion_sdk_version columns
\(#14593\) are no longer folded into the initial CREATEs; they live in their own
0040 migration \(ADD COLUMN on the three tables + events_core_mv MODIFY QUERY\),
matching the doc's "added later" entry. Each migration has matching up/down in
both clustered \(ON CLUSTER default + Replicated*, alter_sync=2 on column ops\)
and unclustered variants; down migrations revert the MV projection before
dropping the columns it references.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* chore: comments and cleanups

* feat\(v4\): add events metadata ngram index as its own migration \(0041\)

The idx_fts_metadata_values_ngram index \(ngrambf_v1 over arrayStringConcat
\(metadata_values\)\) accelerates the `contains` / `starts with` / `ends with`
metadata filter operators on the events tables. It was added by #14133, which
also wired the shared clickhouse filter builder to emit an ngram-targeted
prefilter — like\(arrayStringConcat\(metadata_values\), '%value%'\) — for those
operators. That prefilter is not deployment-gated, so self-hosted v4 emits it
regardless; without the index the prefilter is pure per-row overhead. So the
index must ship with the events tables, not stay dev/Cloud-only.

Promote it out of clickhouse/scripts/dev-tables.sh into its own migration
0041_add_events_metadata_ngram_index \(clustered: ON CLUSTER default +
alter_sync=2; down drops the index\), consistent with how 0040 handles the
ingestion attribution columns.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* chore\(v4\): bump all dev/CI ClickHouse to 25.12, drop 24.3 testing

v4 makes ClickHouse 25.12 the minimum \(the events tables need the >=25.x text
indexes / enable_full_text_index used by migrations 0037/0041\). The -azure and
-redis-cluster deploy-mode configs were still on 24.3 to test old-version
behaviour, but those CI legs run `ch:up` unconditionally and would fail on the
events-table migrations. Bump both to 25.12 to match docker-compose.dev.yml and
update its now-stale rationale comment. No more 24.3 testing.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* fix\(ci\): pin v4 dual-write config in all dev env examples

The deploy-mode CI matrix copies .env.dev*.example verbatim; without explicit
pins the flipped v4 defaults \(events_only\) apply and legacy-table test reads
come up empty. Restores the pin in .env.dev.example \(dropped in 8a342ef96\) and
adds the missing blocks to the redis-cluster and oci examples.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>

* fix\(worker\): keep event-propagation health gate active in events_only mode

isEventPropagationEnabled only fired for WRITE_MODE=dual while the worker
registers the propagation queue via v4WritesToEventsTable \(dual OR
events_only\). With the flipped events_only default the
?failIfEventPropagationStuck liveness probe silently reported disabled and
could never trigger a restart. Mirror the app.ts registration guard.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>

* chore\(clickhouse\): warn about destructive rollback in v4 events down migrations

The up migrations use IF NOT EXISTS, so on preview deployments the tables may
predate the migration \(created via dev-tables.sh\) and hold production-grade
data the rollback would drop.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>

* chore\(clickhouse\): renumber v4 events migrations to 0038-0043 after main's 0037

main landed 0037_add_created_at_indexes \(#15368\) while the branch was in
review, colliding with 0037_create_observations_batch_staging —
golang-migrate errors on duplicate versions. Pure renames, no SQL changes.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>

---------

Co-authored-by: Claude Opus 4.8 <noreply@anthropic.com>")  | Jul 23, 2026  |  
|   |   | [chore(clickhouse): revert add CLICKHOUSE_EXTRA_SETTINGS passthrough f…](https://github.com/langfuse/langfuse/commit/4b4ac12a90bf64ef4d25af3e68a92ed7d3484f8e "chore\(clickhouse\): revert add CLICKHOUSE_EXTRA_SETTINGS passthrough for query settings \(#18521\)

Revert "chore\(clickhouse\): add CLICKHOUSE_EXTRA_SETTINGS passthrough for quer…"

This reverts commit 7643ceac199d65601dd53e9b83ffac8931d7ac09.")  | Oct 9, 2026  |  
|   |   |  [feat(api-keys): independently gate project and organization presets (](https://github.com/langfuse/langfuse/commit/787f6890b23fa298cfc4836bc5b320506af0803c "feat\(api-keys\): independently gate project and organization presets \(#18419\)

* feat\(api-keys\): separate role rollout from authorization enforcement

* feat\(api-keys\): split project and organization preset rollout flags")[#…](https://github.com/langfuse/langfuse/pull/18419)  | Oct 7, 2026  |  
|   |   |  [chore(dx): unit tests run against their own db (](https://github.com/langfuse/langfuse/commit/9b8095f8f522420a8b377964c7467151900f3656 "chore\(dx\): unit tests run against their own db \(#7671\)

* chore\(dx\): unit tests run against their own db

* fix clickhouse

* clean up a bit

* close

* update only use different postgres

* update with command

* remove CH")[#7671](https://github.com/langfuse/langfuse/pull/7671)[)](https://github.com/langfuse/langfuse/commit/9b8095f8f522420a8b377964c7467151900f3656 "chore\(dx\): unit tests run against their own db \(#7671\)

* chore\(dx\): unit tests run against their own db

* fix clickhouse

* clean up a bit

* close

* update only use different postgres

* update with command

* remove CH")  | Jul 4, 2025  |  
|   |   |  [feat(worker): add Rust native addon scaffold (@langfuse/native) (](https://github.com/langfuse/langfuse/commit/1a2d1f21157ed7dfde63a4b332c5728d924ed557 "feat\(worker\): add Rust native addon scaffold \(@langfuse/native\) \(#17062\)

* feat\(worker\): add Rust native addon scaffold \(@langfuse/native\)

Add packages/native, a napi-rs crate the worker loads in-process, with a
hello-world entry point that reports through the addon's own telemetry:
DogStatsD metrics to the Datadog Agent dd-trace already uses, and logs to
stdout in the worker logger's shape. The worker calls it at startup and on
every health probe, so a broken addon stops the worker or fails the probe.

Build with cargo when installed; otherwise cross-compile for the host
platform \(macOS included\) inside a pinned builder container. The worker
image compiles the crate with Alpine's Rust. CI runners get the pinned
toolchain from rustup auto-install; the addon's rustfmt and clippy run as a
dedicated step in the lint job because cargo rejects the ESLint flag the
turbo lint invocation forwards.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs\(native\): fix spelling flagged by codespell

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs\(native\): state that the docker build strategy covers macOS and Linux only

Windows hosts without cargo were sent down the docker path and failed on
target selection. Name the limitation where it fails and in the docs, and
point at rustup or WSL2.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* fix\(native\): log hello calls at debug level

The worker calls hello\(\) on every health probe, so an info-level line per
probe would add permanent log volume on every replica. The counter remains
the heartbeat; the log line stays below the default level.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* chore\(native\): drop the Docker build strategy

Rust via rustup is now a repository prerequisite \(the AI gateway starts
with `pnpm dev`\), so the cross-compile fallback for developers without a
toolchain no longer earns its complexity. The package builds with
`napi build` directly again. Each Rust crate keeps its own
rust-toolchain.toml.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* chore\(native\): pin Rust 1.98.0 like ai-gateway and build the worker image with it

The two crates keep separate toolchain files but start on the same version.
The worker image now copies that exact toolchain from the official
rust:<version>-alpine image instead of using Alpine's packaged Rust, so
production, CI, and laptops compile the addon with one compiler. rustc's
musl targets expect a linker name Alpine does not ship, so cargo is pointed
at cc.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

---------

Co-authored-by: Claude Fable 5.1 <noreply@anthropic.com>")[#17062](https://github.com/langfuse/langfuse/pull/17062)[)](https://github.com/langfuse/langfuse/commit/1a2d1f21157ed7dfde63a4b332c5728d924ed557 "feat\(worker\): add Rust native addon scaffold \(@langfuse/native\) \(#17062\)

* feat\(worker\): add Rust native addon scaffold \(@langfuse/native\)

Add packages/native, a napi-rs crate the worker loads in-process, with a
hello-world entry point that reports through the addon's own telemetry:
DogStatsD metrics to the Datadog Agent dd-trace already uses, and logs to
stdout in the worker logger's shape. The worker calls it at startup and on
every health probe, so a broken addon stops the worker or fails the probe.

Build with cargo when installed; otherwise cross-compile for the host
platform \(macOS included\) inside a pinned builder container. The worker
image compiles the crate with Alpine's Rust. CI runners get the pinned
toolchain from rustup auto-install; the addon's rustfmt and clippy run as a
dedicated step in the lint job because cargo rejects the ESLint flag the
turbo lint invocation forwards.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs\(native\): fix spelling flagged by codespell

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs\(native\): state that the docker build strategy covers macOS and Linux only

Windows hosts without cargo were sent down the docker path and failed on
target selection. Name the limitation where it fails and in the docs, and
point at rustup or WSL2.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* fix\(native\): log hello calls at debug level

The worker calls hello\(\) on every health probe, so an info-level line per
probe would add permanent log volume on every replica. The counter remains
the heartbeat; the log line stays below the default level.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* chore\(native\): drop the Docker build strategy

Rust via rustup is now a repository prerequisite \(the AI gateway starts
with `pnpm dev`\), so the cross-compile fallback for developers without a
toolchain no longer earns its complexity. The package builds with
`napi build` directly again. Each Rust crate keeps its own
rust-toolchain.toml.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* chore\(native\): pin Rust 1.98.0 like ai-gateway and build the worker image with it

The two crates keep separate toolchain files but start on the same version.
The worker image now copies that exact toolchain from the official
rust:<version>-alpine image instead of using Alpine's packaged Rust, so
production, CI, and laptops compile the addon with one compiler. rustc's
musl targets expect a linker name Alpine does not ship, so cargo is pointed
at cc.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

---------

Co-authored-by: Claude Fable 5.1 <noreply@anthropic.com>")  | Sep 15, 2026  |  
|   |   |  [docs(agents): propose Linear ticket updates before writing (](https://github.com/langfuse/langfuse/commit/a85d3ecd84142409cd54902a7006947557dfe9ee "docs\(agents\): propose Linear ticket updates before writing \(#17176\)

* docs\(agents\): propose Linear ticket updates before writing

Comments and description edits now need an explicit yes; planning
subtickets under an existing ticket stay free. Align dependent skills
and AGENTS.md with the tightened write policy.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* docs\(agents\): ask to preserve session results on tickets

Make the end-of-session Linear preserve ask explicit, align skills that
still restated the old ungated comment/description rule, and note where
org vs personal agent preferences live.

* docs\(agents\): drop onboarding, stack, and planning skills

Remove the heavy onboarding, PR-stack, and Linear-planning skills. Keep
identity and stacking as short notes in AGENTS.md and nearby guidance, and
delete whoami.sh.

* docs\(agents\): never print Linear API secrets when proving access

Restore an explicit ban on echoing token values in the Linear connection
guidance, next to the existing do-not-commit header note.

* test\(agents\): drop whoami coverage after onboarding skill removal

The identity suite no longer calls the deleted onboarding whoami script.

* chore\(agents\): drop the identity concept

Remove me.md recovery, the configure-langfuse-identity scripts, and
guidance that treated personal identity as an agent concern. Scope stays
contributor vs maintainer; Linear work uses the authenticated viewer or an
explicit ask.

---------

Co-authored-by: Cursor Agent <cursoragent@cursor.com>")[#17176](https://github.com/langfuse/langfuse/pull/17176)[)](https://github.com/langfuse/langfuse/commit/a85d3ecd84142409cd54902a7006947557dfe9ee "docs\(agents\): propose Linear ticket updates before writing \(#17176\)

* docs\(agents\): propose Linear ticket updates before writing

Comments and description edits now need an explicit yes; planning
subtickets under an existing ticket stay free. Align dependent skills
and AGENTS.md with the tightened write policy.

Co-authored-by: Nikita Kabardin <nikita@kabardin.com>

* docs\(agents\): ask to preserve session results on tickets

Make the end-of-session Linear preserve ask explicit, align skills that
still restated the old ungated comment/description rule, and note where
org vs personal agent preferences live.

* docs\(agents\): drop onboarding, stack, and planning skills

Remove the heavy onboarding, PR-stack, and Linear-planning skills. Keep
identity and stacking as short notes in AGENTS.md and nearby guidance, and
delete whoami.sh.

* docs\(agents\): never print Linear API secrets when proving access

Restore an explicit ban on echoing token values in the Linear connection
guidance, next to the existing do-not-commit header note.

* test\(agents\): drop whoami coverage after onboarding skill removal

The identity suite no longer calls the deleted onboarding whoami script.

* chore\(agents\): drop the identity concept

Remove me.md recovery, the configure-langfuse-identity scripts, and
guidance that treated personal identity as an agent concern. Scope stays
contributor vs maintainer; Linear work uses the authenticated viewer or an
explicit ask.

---------

Co-authored-by: Cursor Agent <cursoragent@cursor.com>")  | Sep 11, 2026  |  
|   |   |  [chore(node): bump to 24.20 (](https://github.com/langfuse/langfuse/commit/e1c57f5c2c5d7c85e2c2f2cc5dad3d781a0569da "chore\(node\): bump to 24.20 \(#16907\)

* chore\(node\): bump to 24.20

* more simp

* more simp")[#16907](https://github.com/langfuse/langfuse/pull/16907)[)](https://github.com/langfuse/langfuse/commit/e1c57f5c2c5d7c85e2c2f2cc5dad3d781a0569da "chore\(node\): bump to 24.20 \(#16907\)

* chore\(node\): bump to 24.20

* more simp

* more simp")  | Sep 2, 2026  |  
|   |   |  [feat(worker): add Rust native addon scaffold (@langfuse/native) (](https://github.com/langfuse/langfuse/commit/1a2d1f21157ed7dfde63a4b332c5728d924ed557 "feat\(worker\): add Rust native addon scaffold \(@langfuse/native\) \(#17062\)

* feat\(worker\): add Rust native addon scaffold \(@langfuse/native\)

Add packages/native, a napi-rs crate the worker loads in-process, with a
hello-world entry point that reports through the addon's own telemetry:
DogStatsD metrics to the Datadog Agent dd-trace already uses, and logs to
stdout in the worker logger's shape. The worker calls it at startup and on
every health probe, so a broken addon stops the worker or fails the probe.

Build with cargo when installed; otherwise cross-compile for the host
platform \(macOS included\) inside a pinned builder container. The worker
image compiles the crate with Alpine's Rust. CI runners get the pinned
toolchain from rustup auto-install; the addon's rustfmt and clippy run as a
dedicated step in the lint job because cargo rejects the ESLint flag the
turbo lint invocation forwards.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs\(native\): fix spelling flagged by codespell

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs\(native\): state that the docker build strategy covers macOS and Linux only

Windows hosts without cargo were sent down the docker path and failed on
target selection. Name the limitation where it fails and in the docs, and
point at rustup or WSL2.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* fix\(native\): log hello calls at debug level

The worker calls hello\(\) on every health probe, so an info-level line per
probe would add permanent log volume on every replica. The counter remains
the heartbeat; the log line stays below the default level.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* chore\(native\): drop the Docker build strategy

Rust via rustup is now a repository prerequisite \(the AI gateway starts
with `pnpm dev`\), so the cross-compile fallback for developers without a
toolchain no longer earns its complexity. The package builds with
`napi build` directly again. Each Rust crate keeps its own
rust-toolchain.toml.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* chore\(native\): pin Rust 1.98.0 like ai-gateway and build the worker image with it

The two crates keep separate toolchain files but start on the same version.
The worker image now copies that exact toolchain from the official
rust:<version>-alpine image instead of using Alpine's packaged Rust, so
production, CI, and laptops compile the addon with one compiler. rustc's
musl targets expect a linker name Alpine does not ship, so cargo is pointed
at cc.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

---------

Co-authored-by: Claude Fable 5.1 <noreply@anthropic.com>")[#17062](https://github.com/langfuse/langfuse/pull/17062)[)](https://github.com/langfuse/langfuse/commit/1a2d1f21157ed7dfde63a4b332c5728d924ed557 "feat\(worker\): add Rust native addon scaffold \(@langfuse/native\) \(#17062\)

* feat\(worker\): add Rust native addon scaffold \(@langfuse/native\)

Add packages/native, a napi-rs crate the worker loads in-process, with a
hello-world entry point that reports through the addon's own telemetry:
DogStatsD metrics to the Datadog Agent dd-trace already uses, and logs to
stdout in the worker logger's shape. The worker calls it at startup and on
every health probe, so a broken addon stops the worker or fails the probe.

Build with cargo when installed; otherwise cross-compile for the host
platform \(macOS included\) inside a pinned builder container. The worker
image compiles the crate with Alpine's Rust. CI runners get the pinned
toolchain from rustup auto-install; the addon's rustfmt and clippy run as a
dedicated step in the lint job because cargo rejects the ESLint flag the
turbo lint invocation forwards.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs\(native\): fix spelling flagged by codespell

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs\(native\): state that the docker build strategy covers macOS and Linux only

Windows hosts without cargo were sent down the docker path and failed on
target selection. Name the limitation where it fails and in the docs, and
point at rustup or WSL2.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* fix\(native\): log hello calls at debug level

The worker calls hello\(\) on every health probe, so an info-level line per
probe would add permanent log volume on every replica. The counter remains
the heartbeat; the log line stays below the default level.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* chore\(native\): drop the Docker build strategy

Rust via rustup is now a repository prerequisite \(the AI gateway starts
with `pnpm dev`\), so the cross-compile fallback for developers without a
toolchain no longer earns its complexity. The package builds with
`napi build` directly again. Each Rust crate keeps its own
rust-toolchain.toml.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* chore\(native\): pin Rust 1.98.0 like ai-gateway and build the worker image with it

The two crates keep separate toolchain files but start on the same version.
The worker image now copies that exact toolchain from the official
rust:<version>-alpine image instead of using Alpine's packaged Rust, so
production, CI, and laptops compile the addon with one compiler. rustc's
musl targets expect a linker name Alpine does not ship, so cargo is pointed
at cc.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

---------

Co-authored-by: Claude Fable 5.1 <noreply@anthropic.com>")  | Sep 15, 2026  |  
|   |   |  [chore(ci): trace gh action deps upgrade wf to langfuse (](https://github.com/langfuse/langfuse/commit/a1778e1200b3bafac5c22e130194f475c13c89a1 "chore\(ci\): trace gh action deps upgrade wf to langfuse \(#17028\)

* chore\(ci\): trace gh action deps upgrade wf to langfuse

* fix

* fix")[#17028](https://github.com/langfuse/langfuse/pull/17028)[)](https://github.com/langfuse/langfuse/commit/a1778e1200b3bafac5c22e130194f475c13c89a1 "chore\(ci\): trace gh action deps upgrade wf to langfuse \(#17028\)

* chore\(ci\): trace gh action deps upgrade wf to langfuse

* fix

* fix")  | Sep 3, 2026  |  
|   |   |  [chore(agent-setup): centralize shared config and skills under .agents (](https://github.com/langfuse/langfuse/commit/1dc9ce65cfb97fe2e348e3cca463f842e7e2dcc3 "chore\(agent-setup\): centralize shared config and skills under .agents \(#12795\)")[…](https://github.com/langfuse/langfuse/pull/12795)  | Mar 26, 2026  |  
|   |   |  [fix(web): green and blue badge text meets contrast in light mode (](https://github.com/langfuse/langfuse/commit/f445bcef34f9d0269237f23ff27bc946e888e389 "fix\(web\): green and blue badge text meets contrast in light mode \(#18482\)

* fix\(web\): green and blue badge text meets contrast in light mode

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>

* fix\(web\): darker light-mode green and blue tokens for text contrast

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>

---------

Co-authored-by: Claude Opus 5.5 <noreply@anthropic.com>")[#18482](https://github.com/langfuse/langfuse/pull/18482)  | Oct 9, 2026  |  
|   |   |   | Aug 18, 2026  |  
|   |   |  [docs: update telemetry docs in localized READMEs (](https://github.com/langfuse/langfuse/commit/bb7fe64b4e31804aeea5ccba8209f72657567e27 "docs: update telemetry docs in localized READMEs \(#12820\)

Update localized telemetry docs for OSS wording")[#12820](https://github.com/langfuse/langfuse/pull/12820)[)](https://github.com/langfuse/langfuse/commit/bb7fe64b4e31804aeea5ccba8209f72657567e27 "docs: update telemetry docs in localized READMEs \(#12820\)

Update localized telemetry docs for OSS wording")  | Mar 26, 2026  |  
|   |   |  [docs: update telemetry docs in localized READMEs (](https://github.com/langfuse/langfuse/commit/bb7fe64b4e31804aeea5ccba8209f72657567e27 "docs: update telemetry docs in localized READMEs \(#12820\)

Update localized telemetry docs for OSS wording")[#12820](https://github.com/langfuse/langfuse/pull/12820)[)](https://github.com/langfuse/langfuse/commit/bb7fe64b4e31804aeea5ccba8209f72657567e27 "docs: update telemetry docs in localized READMEs \(#12820\)

Update localized telemetry docs for OSS wording")  | Mar 26, 2026  |  
|   |   |  [docs: update telemetry docs in localized READMEs (](https://github.com/langfuse/langfuse/commit/bb7fe64b4e31804aeea5ccba8209f72657567e27 "docs: update telemetry docs in localized READMEs \(#12820\)

Update localized telemetry docs for OSS wording")[#12820](https://github.com/langfuse/langfuse/pull/12820)[)](https://github.com/langfuse/langfuse/commit/bb7fe64b4e31804aeea5ccba8209f72657567e27 "docs: update telemetry docs in localized READMEs \(#12820\)

Update localized telemetry docs for OSS wording")  | Mar 26, 2026  |  
|   |   |  [docs(self-hosting): document Docker log retention (](https://github.com/langfuse/langfuse/commit/3a3f190ddff9dd7b899f0f49baae1ef2a9c983f8 "docs\(self-hosting\): document Docker log retention \(#16363\)

* fix\(docker\): bound logs for compose services

* fix\(docker\): make bounded Compose logging opt-in

* docs\(self-hosting\): document Docker log retention

---------

Co-authored-by: Steffen Reuther <steffen@langfuse.com>")[#16363](https://github.com/langfuse/langfuse/pull/16363)[)](https://github.com/langfuse/langfuse/commit/3a3f190ddff9dd7b899f0f49baae1ef2a9c983f8 "docs\(self-hosting\): document Docker log retention \(#16363\)

* fix\(docker\): bound logs for compose services

* fix\(docker\): make bounded Compose logging opt-in

* docs\(self-hosting\): document Docker log retention

---------

Co-authored-by: Steffen Reuther <steffen@langfuse.com>")  | Sep 24, 2026  |  
|   |   |  [docs: Differentiate useEffect guidance in REVIEW.md (](https://github.com/langfuse/langfuse/commit/59076914c669ff99d1e1491540f833e9acadc80e "docs: Differentiate useEffect guidance in REVIEW.md \(#18176\)")[#18176](https://github.com/langfuse/langfuse/pull/18176)[)](https://github.com/langfuse/langfuse/commit/59076914c669ff99d1e1491540f833e9acadc80e "docs: Differentiate useEffect guidance in REVIEW.md \(#18176\)")  | Oct 7, 2026  |  
|   |   | [docs: update SECURITY.md](https://github.com/langfuse/langfuse/commit/45e31c0b3f1b21c0dfd5e32fc148f173b6206280 "docs: update SECURITY.md")  | Dec 12, 2023  |  
| [docker-compose.build.yml](https://github.com/langfuse/langfuse/blob/main/docker-compose.build.yml "docker-compose.build.yml")  | [docker-compose.build.yml](https://github.com/langfuse/langfuse/blob/main/docker-compose.build.yml "docker-compose.build.yml")  |  [feat(topics): classify traces into buckets (](https://github.com/langfuse/langfuse/commit/3fed0dadc5d4d17f465fbd01b18f41f3cf232aff "feat\(topics\): classify traces into buckets \(#17664\)

* feat\(topics\): classify traces into buckets

* add feature flag etc

* move from python to rust

* enable topics for demo@

* remove token budgetting

* fix ui: tabbing

* fix ui: improv

* no random file storage as queue

* shorter transcripts

* datadog tracing

* Allow custom min sample number.

* better progress tracking

* database separation

* perf faster clustering

* embedding queue

* clean UX

* cleanup

* fix

* fix

* cleanup

* no unbound

* cleanup

* simplify queueing, deletion

* no s3

* sessionid, traceid instead of unitid

* simp

* simp

* allow list project ids

* simplify CH schema

* rename schema, simplify

* simplify

* ci

* simplify, fix ci

* consolidate tests

* fix ci worker test, gate env flag

* add db table init script

* fix tests

* add flag parser

* fix tests

* switch to bedrock based cohere embed

* simplify

* use design system compos

* use assembleTranscript

* fix

* remove dev tables

* fix

* switch to bedrock served models

* fix

* fix reasoning

* run CH tables in ci

* fix float precision

* sec

* manual review 1

* slimmer setup

* remove IDs

* datadog monitors

* simplify

* fix

* feat\(worker\): summarize trace batches into topics

Run facet summarization from the assembled transcript on the trace-batch queue, using instance Bedrock credentials, and enqueue older matching traces through the same worker.

Co-authored-by: Cursor <cursoragent@cursor.com>

* feat\(worker\): admit every trace of Topics projects to trace batching

Projects in LANGFUSE_TOPICS_ENABLED_PROJECT_IDS bypass the trace-batch sampling
rate at ingestion, so each of their traces reaches the trace-batch queue and is
summarized. With LANGFUSE_TRACE_BATCH_SAMPLING_RATE=0 the automated flow runs
for those projects only.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* perf\(worker\): skip trace-batch tracking at sampling rate 0

At LANGFUSE_TRACE_BATCH_SAMPLING_RATE=0 only Topics-enabled projects are
admitted; for all other projects ingestion now returns before building
per-trace state, recording metrics or calling Redis.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* feat\(worker\): default trace-batch sampling to off

LANGFUSE_TRACE_BATCH_SAMPLING_RATE now defaults to 0: with trace-batch
ingestion enabled and no rate set, only Topics-enabled projects are admitted
and all other projects are skipped. Set a rate to sample other projects.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* fix\(worker\): pin trace-batch idle time in tests and drop unused exports

The trace-batch idle default is 2 minutes on DEV, which the scheduling test
ran under in CI; pin the 10-minute value it asserts. Make two helpers that are
only used in their own module private so knip passes.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* feat\(worker\): keep trace batches running past failed Topics traces

A trace whose Topics summary fails no longer fails the batch job. Every trace
is processed, outcomes are counted once per job, and failed traces are
re-enqueued as a new trace-batch job with a retry counter and doubling delay,
up to LANGFUSE_TOPICS_TRACE_MAX_RETRIES. Errors without a provider reason,
such as the input limit, are not retried. Only a failed batch read still fails
the job.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* fix CI

* fix\(worker\): count failed Topics traces without re-enqueueing trace batches

A failed Topics summary re-enqueued its trace as a new trace-batch job,
which read the shared batch from ClickHouse again. During a provider
outage every affected trace repeated that read. Failed traces are now
only counted in langfuse.topics.trace_outcomes by reason.

Co-authored-by: Cursor <cursoragent@cursor.com>

* fix topics positioning

* fix data model after valeriy

* switch to eval tables

* ch migrations are real

* simplify

* feat\(topics\): configure summary and embedding models

* fix\(topics\): gate every model pipeline stage

* fix\(topics\): use geo Bedrock profile IDs

* fix\(topics\): use regional embedding profiles

* docs\(topics\): label PoC env settings as internal

* fix\(topics\): satisfy render lint rules

* test\(topics\): expect env-configured embedding model

* fix\(native\): link N-API symbols in Rust tests

* test\(topics\): update model config expectations

* environtment no default

* slimmer tests

* CH settings on table

* use lightweight deletes

* rename journals to execution store

* empty default env i guess

* move env var

* fix\(clickhouse\): rebuild local databases during reset

* clean

* fix useeffect

* cleanup

* fix

* remove split view

---------

Co-authored-by: Lysander Kiesel <44638111+LysanderKie@users.noreply.github.com>
Co-authored-by: Cursor <cursoragent@cursor.com>
Co-authored-by: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>")[#17664](https://github.com/langfuse/langfuse/pull/17664)[)](https://github.com/langfuse/langfuse/commit/3fed0dadc5d4d17f465fbd01b18f41f3cf232aff "feat\(topics\): classify traces into buckets \(#17664\)

* feat\(topics\): classify traces into buckets

* add feature flag etc

* move from python to rust

* enable topics for demo@

* remove token budgetting

* fix ui: tabbing

* fix ui: improv

* no random file storage as queue

* shorter transcripts

* datadog tracing

* Allow custom min sample number.

* better progress tracking

* database separation

* perf faster clustering

* embedding queue

* clean UX

* cleanup

* fix

* fix

* cleanup

* no unbound

* cleanup

* simplify queueing, deletion

* no s3

* sessionid, traceid instead of unitid

* simp

* simp

* allow list project ids

* simplify CH schema

* rename schema, simplify

* simplify

* ci

* simplify, fix ci

* consolidate tests

* fix ci worker test, gate env flag

* add db table init script

* fix tests

* add flag parser

* fix tests

* switch to bedrock based cohere embed

* simplify

* use design system compos

* use assembleTranscript

* fix

* remove dev tables

* fix

* switch to bedrock served models

* fix

* fix reasoning

* run CH tables in ci

* fix float precision

* sec

* manual review 1

* slimmer setup

* remove IDs

* datadog monitors

* simplify

* fix

* feat\(worker\): summarize trace batches into topics

Run facet summarization from the assembled transcript on the trace-batch queue, using instance Bedrock credentials, and enqueue older matching traces through the same worker.

Co-authored-by: Cursor <cursoragent@cursor.com>

* feat\(worker\): admit every trace of Topics projects to trace batching

Projects in LANGFUSE_TOPICS_ENABLED_PROJECT_IDS bypass the trace-batch sampling
rate at ingestion, so each of their traces reaches the trace-batch queue and is
summarized. With LANGFUSE_TRACE_BATCH_SAMPLING_RATE=0 the automated flow runs
for those projects only.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* perf\(worker\): skip trace-batch tracking at sampling rate 0

At LANGFUSE_TRACE_BATCH_SAMPLING_RATE=0 only Topics-enabled projects are
admitted; for all other projects ingestion now returns before building
per-trace state, recording metrics or calling Redis.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* feat\(worker\): default trace-batch sampling to off

LANGFUSE_TRACE_BATCH_SAMPLING_RATE now defaults to 0: with trace-batch
ingestion enabled and no rate set, only Topics-enabled projects are admitted
and all other projects are skipped. Set a rate to sample other projects.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* fix\(worker\): pin trace-batch idle time in tests and drop unused exports

The trace-batch idle default is 2 minutes on DEV, which the scheduling test
ran under in CI; pin the 10-minute value it asserts. Make two helpers that are
only used in their own module private so knip passes.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* feat\(worker\): keep trace batches running past failed Topics traces

A trace whose Topics summary fails no longer fails the batch job. Every trace
is processed, outcomes are counted once per job, and failed traces are
re-enqueued as a new trace-batch job with a retry counter and doubling delay,
up to LANGFUSE_TOPICS_TRACE_MAX_RETRIES. Errors without a provider reason,
such as the input limit, are not retried. Only a failed batch read still fails
the job.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* fix CI

* fix\(worker\): count failed Topics traces without re-enqueueing trace batches

A failed Topics summary re-enqueued its trace as a new trace-batch job,
which read the shared batch from ClickHouse again. During a provider
outage every affected trace repeated that read. Failed traces are now
only counted in langfuse.topics.trace_outcomes by reason.

Co-authored-by: Cursor <cursoragent@cursor.com>

* fix topics positioning

* fix data model after valeriy

* switch to eval tables

* ch migrations are real

* simplify

* feat\(topics\): configure summary and embedding models

* fix\(topics\): gate every model pipeline stage

* fix\(topics\): use geo Bedrock profile IDs

* fix\(topics\): use regional embedding profiles

* docs\(topics\): label PoC env settings as internal

* fix\(topics\): satisfy render lint rules

* test\(topics\): expect env-configured embedding model

* fix\(native\): link N-API symbols in Rust tests

* test\(topics\): update model config expectations

* environtment no default

* slimmer tests

* CH settings on table

* use lightweight deletes

* rename journals to execution store

* empty default env i guess

* move env var

* fix\(clickhouse\): rebuild local databases during reset

* clean

* fix useeffect

* cleanup

* fix

* remove split view

---------

Co-authored-by: Lysander Kiesel <44638111+LysanderKie@users.noreply.github.com>
Co-authored-by: Cursor <cursoragent@cursor.com>
Co-authored-by: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>")  | Oct 6, 2026  |  
| [docker-compose.dev-azure.yml](https://github.com/langfuse/langfuse/blob/main/docker-compose.dev-azure.yml "docker-compose.dev-azure.yml")  | [docker-compose.dev-azure.yml](https://github.com/langfuse/langfuse/blob/main/docker-compose.dev-azure.yml "docker-compose.dev-azure.yml")  | [feat(v4): promote events tables to CH migration and flip v4 env defau…](https://github.com/langfuse/langfuse/commit/eaa34c3e7fd8e75952ed72dc059982b0397fe69c "feat\(v4\): promote events tables to CH migration and flip v4 env defaults \(#14812\)

* feat\(v4\): promote events tables to CH migration and flip v4 env defaults

Prepare the Langfuse v4 preview line.

- Add migration 0036_add_v4_events_tables \(clustered + unclustered, up + down\)
  promoting observations_batch_staging, events_full, events_core and
  events_core_mv out of clickhouse/scripts/dev-tables.sh, now that v4 raises the
  minimum ClickHouse version to 25.12 \(block_number columns >= 24.5, text
  indexes >= 25.x\). Folds in the ingestion_api_key / ingestion_sdk_name /
  ingestion_sdk_version columns \(previously added via dev-tables.sh ALTERs,
  #14593\). Uses CREATE ... IF NOT EXISTS so preview self-hosters that applied
  the manual schema upgrade cleanly.
- Flip the v4 env defaults to the target state in worker, shared and web:
  LANGFUSE_MIGRATION_V4_WRITE_MODE=events_only,
  LANGFUSE_MIGRATION_V4_NATIVE_OTEL_BEHAVIOUR=direct,
  LANGFUSE_MIGRATION_V4_ALLOW_PREVIEW_OPT_IN=true,
  LANGFUSE_BACKGROUND_MIGRATION_V4_ENABLE_HISTORIC_BACKFILL=true. These couple
  via validateV4Flags; the DROP_PID_TID cleanup stays opt-in.
- Pin .env.dev*.example \(all four deploy modes\) to the dual-write dev/CI config
  with the historic backfill held off, so the flipped defaults only affect bare
  net-new deployments and do not destabilise the deploy-mode CI matrix.
- Trim dev-tables.sh to Cloud-only diagnostics \(ingestion_size_stats + MVs,
  analytics_events_core, ngram indexes\) and local sample-data seeding.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* refactor\(v4\): split events-table migration into one DDL per migration

Break the monolithic 0036_add_v4_events_tables into per-statement migrations so
each DDL change is tracked individually, mirroring how the self-hosted events
rollout doc records the initial schema plus each subsequent change:

- 0036_create_observations_batch_staging
- 0037_create_events_full
- 0038_create_events_core
- 0039_create_events_core_mv
- 0040_add_events_ingestion_attribution_columns

The ingestion_api_key / ingestion_sdk_name / ingestion_sdk_version columns
\(#14593\) are no longer folded into the initial CREATEs; they live in their own
0040 migration \(ADD COLUMN on the three tables + events_core_mv MODIFY QUERY\),
matching the doc's "added later" entry. Each migration has matching up/down in
both clustered \(ON CLUSTER default + Replicated*, alter_sync=2 on column ops\)
and unclustered variants; down migrations revert the MV projection before
dropping the columns it references.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* chore: comments and cleanups

* feat\(v4\): add events metadata ngram index as its own migration \(0041\)

The idx_fts_metadata_values_ngram index \(ngrambf_v1 over arrayStringConcat
\(metadata_values\)\) accelerates the `contains` / `starts with` / `ends with`
metadata filter operators on the events tables. It was added by #14133, which
also wired the shared clickhouse filter builder to emit an ngram-targeted
prefilter — like\(arrayStringConcat\(metadata_values\), '%value%'\) — for those
operators. That prefilter is not deployment-gated, so self-hosted v4 emits it
regardless; without the index the prefilter is pure per-row overhead. So the
index must ship with the events tables, not stay dev/Cloud-only.

Promote it out of clickhouse/scripts/dev-tables.sh into its own migration
0041_add_events_metadata_ngram_index \(clustered: ON CLUSTER default +
alter_sync=2; down drops the index\), consistent with how 0040 handles the
ingestion attribution columns.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* chore\(v4\): bump all dev/CI ClickHouse to 25.12, drop 24.3 testing

v4 makes ClickHouse 25.12 the minimum \(the events tables need the >=25.x text
indexes / enable_full_text_index used by migrations 0037/0041\). The -azure and
-redis-cluster deploy-mode configs were still on 24.3 to test old-version
behaviour, but those CI legs run `ch:up` unconditionally and would fail on the
events-table migrations. Bump both to 25.12 to match docker-compose.dev.yml and
update its now-stale rationale comment. No more 24.3 testing.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>

* fix\(ci\): pin v4 dual-write config in all dev env examples

The deploy-mode CI matrix copies .env.dev*.example verbatim; without explicit
pins the flipped v4 defaults \(events_only\) apply and legacy-table test reads
come up empty. Restores the pin in .env.dev.example \(dropped in 8a342ef96\) and
adds the missing blocks to the redis-cluster and oci examples.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>

* fix\(worker\): keep event-propagation health gate active in events_only mode

isEventPropagationEnabled only fired for WRITE_MODE=dual while the worker
registers the propagation queue via v4WritesToEventsTable \(dual OR
events_only\). With the flipped events_only default the
?failIfEventPropagationStuck liveness probe silently reported disabled and
could never trigger a restart. Mirror the app.ts registration guard.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>

* chore\(clickhouse\): warn about destructive rollback in v4 events down migrations

The up migrations use IF NOT EXISTS, so on preview deployments the tables may
predate the migration \(created via dev-tables.sh\) and hold production-grade
data the rollback would drop.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>

* chore\(clickhouse\): renumber v4 events migrations to 0038-0043 after main's 0037

main landed 0037_add_created_at_indexes \(#15368\) while the branch was in
review, colliding with 0037_create_observations_batch_staging —
golang-migrate errors on duplicate versions. Pure renames, no SQL changes.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>

---------

Co-authored-by: Claude Opus 4.8 <noreply@anthropic.com>")  | Jul 23, 2026  |  
| [docker-compose.dev-redis-cluster.yml](https://github.com/langfuse/langfuse/blob/main/docker-compose.dev-redis-cluster.yml "docker-compose.dev-redis-cluster.yml")  | [docker-compose.dev-redis-cluster.yml](https://github.com/langfuse/langfuse/blob/main/docker-compose.dev-redis-cluster.yml "docker-compose.dev-redis-cluster.yml")  |  [chore: bump clickhouse test version to 26.9 (](https://github.com/langfuse/langfuse/commit/e842be0294b82bd4963d7f8a8e73ecb41078eb33 "chore: bump clickhouse test version to 26.9 \(#17769\)")[#17769](https://github.com/langfuse/langfuse/pull/17769)[)](https://github.com/langfuse/langfuse/commit/e842be0294b82bd4963d7f8a8e73ecb41078eb33 "chore: bump clickhouse test version to 26.9 \(#17769\)")  | Sep 22, 2026  |  
|   |   |  [chore(ci): pin plain CI ClickHouse leg to 26.4 (](https://github.com/langfuse/langfuse/commit/48d4671c547cb5ac060663914a04f76324ecd770 "chore\(ci\): pin plain CI ClickHouse leg to 26.4 \(#17726\)

The plain deploy-mode config exists to run the version Langfuse Cloud runs,
and its comment says so, but it pinned 26.2 while Cloud runs 26.4 in every
region. CONTRIBUTING.md already documents 26.4 for this leg.

26.2 also sits inside the disable-lazy-materialization-for-patch-parts
compatibility band, so the leg ran a workaround Cloud does not have.
ClickHouse #102904 is fixed upstream below the first shipped 26.4 release,
and the azure leg at 25.12 still exercises the band's active side.

Co-authored-by: Claude Opus 5 \(1M context\) <noreply@anthropic.com>")[#17726](https://github.com/langfuse/langfuse/pull/17726)[)](https://github.com/langfuse/langfuse/commit/48d4671c547cb5ac060663914a04f76324ecd770 "chore\(ci\): pin plain CI ClickHouse leg to 26.4 \(#17726\)

The plain deploy-mode config exists to run the version Langfuse Cloud runs,
and its comment says so, but it pinned 26.2 while Cloud runs 26.4 in every
region. CONTRIBUTING.md already documents 26.4 for this leg.

26.2 also sits inside the disable-lazy-materialization-for-patch-parts
compatibility band, so the leg ran a workaround Cloud does not have.
ClickHouse #102904 is fixed upstream below the first shipped 26.4 release,
and the azure leg at 25.12 still exercises the band's active side.

Co-authored-by: Claude Opus 5 \(1M context\) <noreply@anthropic.com>")  | Sep 21, 2026  |  
|   |   |  [feat(topics): classify traces into buckets (](https://github.com/langfuse/langfuse/commit/3fed0dadc5d4d17f465fbd01b18f41f3cf232aff "feat\(topics\): classify traces into buckets \(#17664\)

* feat\(topics\): classify traces into buckets

* add feature flag etc

* move from python to rust

* enable topics for demo@

* remove token budgetting

* fix ui: tabbing

* fix ui: improv

* no random file storage as queue

* shorter transcripts

* datadog tracing

* Allow custom min sample number.

* better progress tracking

* database separation

* perf faster clustering

* embedding queue

* clean UX

* cleanup

* fix

* fix

* cleanup

* no unbound

* cleanup

* simplify queueing, deletion

* no s3

* sessionid, traceid instead of unitid

* simp

* simp

* allow list project ids

* simplify CH schema

* rename schema, simplify

* simplify

* ci

* simplify, fix ci

* consolidate tests

* fix ci worker test, gate env flag

* add db table init script

* fix tests

* add flag parser

* fix tests

* switch to bedrock based cohere embed

* simplify

* use design system compos

* use assembleTranscript

* fix

* remove dev tables

* fix

* switch to bedrock served models

* fix

* fix reasoning

* run CH tables in ci

* fix float precision

* sec

* manual review 1

* slimmer setup

* remove IDs

* datadog monitors

* simplify

* fix

* feat\(worker\): summarize trace batches into topics

Run facet summarization from the assembled transcript on the trace-batch queue, using instance Bedrock credentials, and enqueue older matching traces through the same worker.

Co-authored-by: Cursor <cursoragent@cursor.com>

* feat\(worker\): admit every trace of Topics projects to trace batching

Projects in LANGFUSE_TOPICS_ENABLED_PROJECT_IDS bypass the trace-batch sampling
rate at ingestion, so each of their traces reaches the trace-batch queue and is
summarized. With LANGFUSE_TRACE_BATCH_SAMPLING_RATE=0 the automated flow runs
for those projects only.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* perf\(worker\): skip trace-batch tracking at sampling rate 0

At LANGFUSE_TRACE_BATCH_SAMPLING_RATE=0 only Topics-enabled projects are
admitted; for all other projects ingestion now returns before building
per-trace state, recording metrics or calling Redis.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* feat\(worker\): default trace-batch sampling to off

LANGFUSE_TRACE_BATCH_SAMPLING_RATE now defaults to 0: with trace-batch
ingestion enabled and no rate set, only Topics-enabled projects are admitted
and all other projects are skipped. Set a rate to sample other projects.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* fix\(worker\): pin trace-batch idle time in tests and drop unused exports

The trace-batch idle default is 2 minutes on DEV, which the scheduling test
ran under in CI; pin the 10-minute value it asserts. Make two helpers that are
only used in their own module private so knip passes.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* feat\(worker\): keep trace batches running past failed Topics traces

A trace whose Topics summary fails no longer fails the batch job. Every trace
is processed, outcomes are counted once per job, and failed traces are
re-enqueued as a new trace-batch job with a retry counter and doubling delay,
up to LANGFUSE_TOPICS_TRACE_MAX_RETRIES. Errors without a provider reason,
such as the input limit, are not retried. Only a failed batch read still fails
the job.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* fix CI

* fix\(worker\): count failed Topics traces without re-enqueueing trace batches

A failed Topics summary re-enqueued its trace as a new trace-batch job,
which read the shared batch from ClickHouse again. During a provider
outage every affected trace repeated that read. Failed traces are now
only counted in langfuse.topics.trace_outcomes by reason.

Co-authored-by: Cursor <cursoragent@cursor.com>

* fix topics positioning

* fix data model after valeriy

* switch to eval tables

* ch migrations are real

* simplify

* feat\(topics\): configure summary and embedding models

* fix\(topics\): gate every model pipeline stage

* fix\(topics\): use geo Bedrock profile IDs

* fix\(topics\): use regional embedding profiles

* docs\(topics\): label PoC env settings as internal

* fix\(topics\): satisfy render lint rules

* test\(topics\): expect env-configured embedding model

* fix\(native\): link N-API symbols in Rust tests

* test\(topics\): update model config expectations

* environtment no default

* slimmer tests

* CH settings on table

* use lightweight deletes

* rename journals to execution store

* empty default env i guess

* move env var

* fix\(clickhouse\): rebuild local databases during reset

* clean

* fix useeffect

* cleanup

* fix

* remove split view

---------

Co-authored-by: Lysander Kiesel <44638111+LysanderKie@users.noreply.github.com>
Co-authored-by: Cursor <cursoragent@cursor.com>
Co-authored-by: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>")[#17664](https://github.com/langfuse/langfuse/pull/17664)[)](https://github.com/langfuse/langfuse/commit/3fed0dadc5d4d17f465fbd01b18f41f3cf232aff "feat\(topics\): classify traces into buckets \(#17664\)

* feat\(topics\): classify traces into buckets

* add feature flag etc

* move from python to rust

* enable topics for demo@

* remove token budgetting

* fix ui: tabbing

* fix ui: improv

* no random file storage as queue

* shorter transcripts

* datadog tracing

* Allow custom min sample number.

* better progress tracking

* database separation

* perf faster clustering

* embedding queue

* clean UX

* cleanup

* fix

* fix

* cleanup

* no unbound

* cleanup

* simplify queueing, deletion

* no s3

* sessionid, traceid instead of unitid

* simp

* simp

* allow list project ids

* simplify CH schema

* rename schema, simplify

* simplify

* ci

* simplify, fix ci

* consolidate tests

* fix ci worker test, gate env flag

* add db table init script

* fix tests

* add flag parser

* fix tests

* switch to bedrock based cohere embed

* simplify

* use design system compos

* use assembleTranscript

* fix

* remove dev tables

* fix

* switch to bedrock served models

* fix

* fix reasoning

* run CH tables in ci

* fix float precision

* sec

* manual review 1

* slimmer setup

* remove IDs

* datadog monitors

* simplify

* fix

* feat\(worker\): summarize trace batches into topics

Run facet summarization from the assembled transcript on the trace-batch queue, using instance Bedrock credentials, and enqueue older matching traces through the same worker.

Co-authored-by: Cursor <cursoragent@cursor.com>

* feat\(worker\): admit every trace of Topics projects to trace batching

Projects in LANGFUSE_TOPICS_ENABLED_PROJECT_IDS bypass the trace-batch sampling
rate at ingestion, so each of their traces reaches the trace-batch queue and is
summarized. With LANGFUSE_TRACE_BATCH_SAMPLING_RATE=0 the automated flow runs
for those projects only.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* perf\(worker\): skip trace-batch tracking at sampling rate 0

At LANGFUSE_TRACE_BATCH_SAMPLING_RATE=0 only Topics-enabled projects are
admitted; for all other projects ingestion now returns before building
per-trace state, recording metrics or calling Redis.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* feat\(worker\): default trace-batch sampling to off

LANGFUSE_TRACE_BATCH_SAMPLING_RATE now defaults to 0: with trace-batch
ingestion enabled and no rate set, only Topics-enabled projects are admitted
and all other projects are skipped. Set a rate to sample other projects.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* fix\(worker\): pin trace-batch idle time in tests and drop unused exports

The trace-batch idle default is 2 minutes on DEV, which the scheduling test
ran under in CI; pin the 10-minute value it asserts. Make two helpers that are
only used in their own module private so knip passes.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* feat\(worker\): keep trace batches running past failed Topics traces

A trace whose Topics summary fails no longer fails the batch job. Every trace
is processed, outcomes are counted once per job, and failed traces are
re-enqueued as a new trace-batch job with a retry counter and doubling delay,
up to LANGFUSE_TOPICS_TRACE_MAX_RETRIES. Errors without a provider reason,
such as the input limit, are not retried. Only a failed batch read still fails
the job.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* fix CI

* fix\(worker\): count failed Topics traces without re-enqueueing trace batches

A failed Topics summary re-enqueued its trace as a new trace-batch job,
which read the shared batch from ClickHouse again. During a provider
outage every affected trace repeated that read. Failed traces are now
only counted in langfuse.topics.trace_outcomes by reason.

Co-authored-by: Cursor <cursoragent@cursor.com>

* fix topics positioning

* fix data model after valeriy

* switch to eval tables

* ch migrations are real

* simplify

* feat\(topics\): configure summary and embedding models

* fix\(topics\): gate every model pipeline stage

* fix\(topics\): use geo Bedrock profile IDs

* fix\(topics\): use regional embedding profiles

* docs\(topics\): label PoC env settings as internal

* fix\(topics\): satisfy render lint rules

* test\(topics\): expect env-configured embedding model

* fix\(native\): link N-API symbols in Rust tests

* test\(topics\): update model config expectations

* environtment no default

* slimmer tests

* CH settings on table

* use lightweight deletes

* rename journals to execution store

* empty default env i guess

* move env var

* fix\(clickhouse\): rebuild local databases during reset

* clean

* fix useeffect

* cleanup

* fix

* remove split view

---------

Co-authored-by: Lysander Kiesel <44638111+LysanderKie@users.noreply.github.com>
Co-authored-by: Cursor <cursoragent@cursor.com>
Co-authored-by: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>")  | Oct 6, 2026  |  
|   |   |  [fix(web): stop serving web workers from URLs that 404 after a deploy (](https://github.com/langfuse/langfuse/commit/242583d50f521efb3daa8ad6952c0a8d5ea1af28 "fix\(web\): stop serving web workers from URLs that 404 after a deploy \(#18315\)

* fix\(web\): stop serving web workers from URLs that 404 after a deploy

Both web workers were started from Turbopack chunk URLs under
/_next/static/chunks. Those are deliberately pinned to the app origin, which
only ever serves the current build, so every tab that outlives a deploy 404s on
its worker: the JSON parse worker then leaves the Raw tab on "Parsing in
background..." for as long as the tab is open, and the graph layout worker
silently drops to the much smaller main-thread layout budget.

- Bundle the parse worker into a string module at build time and start it from
  a blob, so it has no URL at all. Importing deepParseJsonIterative from its own
  module instead of the @langfuse/shared barrel takes that bundle from
  1,025,505 B to 3,669 B, which is also why Turbopack split the worker into a
  bootstrap plus importScripts chunks in the first place. The generated module
  is committed and a test fails if it no longer matches a fresh build.
- Serve elkjs's own prebuilt, standalone worker from /api/workers/elk-worker,
  a path that keeps resolving across deploys, the way /api/docs serves Scalar.
- Give every parse request a deadline and settle it on the main thread whenever
  the worker stops answering, reported or not. The two hooks each carried a copy
  of the worker plumbing, which is how the same missing recovery came to exist
  twice; they now share one client.
- Cancel the worker error event when reporting it, so the browser does not
  report the same failure a second time on the owning document, where it is
  captured again as unhandled.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* fix\(web\): start a parse request's deadline when the worker takes it

The client posted every request to the worker as it arrived. The worker is
single-threaded, so that only moved the queue inside it, where the deadline
cannot see it: a request waiting behind a slow parse burned its 30s without
the worker ever having looked at it, then terminated a worker that was
answering normally and dropped the whole backlog onto the main thread. Two of
those and background parsing was off for the rest of the tab.

Hold the queue on this side and keep one request in flight, so the deadline
measures what it is meant to measure. A miss now settles only the request the
worker actually held on the main thread; whatever was merely queued has not
been tried yet and goes to a fresh worker instead of blocking rendering.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

---------

Co-authored-by: Claude Opus 5 <noreply@anthropic.com>")[#…](https://github.com/langfuse/langfuse/pull/18315)  | Oct 6, 2026  |  
|   |   |   | Oct 9, 2026  |  
|   |   |  [chore(deps): bump @modelcontextprotocol/sdk to 1.31.0 (](https://github.com/langfuse/langfuse/commit/223978955ebfe6cbf6e8572524f64c2565d43039 "chore\(deps\): bump @modelcontextprotocol/sdk to 1.31.0 \(#18560\)")[#18560](https://github.com/langfuse/langfuse/pull/18560)[)](https://github.com/langfuse/langfuse/commit/223978955ebfe6cbf6e8572524f64c2565d43039 "chore\(deps\): bump @modelcontextprotocol/sdk to 1.31.0 \(#18560\)")  | Oct 9, 2026  |  
|   |   |  [chore(deps): bump handlebars to 4.7.10 (](https://github.com/langfuse/langfuse/commit/89c8c740551c7db3a7b139878223ba798f6d414b "chore\(deps\): bump handlebars to 4.7.10 \(#18512\)

* chore\(deps\): bump handlebars to 4.7.10

* ci: trigger checks

---------

Co-authored-by: github-actions\[bot\] <41898282+github-actions\[bot\]@users.noreply.github.com>
Co-authored-by: langfuse-bot <accounts+githubbot@langfuse.com>")[#18512](https://github.com/langfuse/langfuse/pull/18512)[)](https://github.com/langfuse/langfuse/commit/89c8c740551c7db3a7b139878223ba798f6d414b "chore\(deps\): bump handlebars to 4.7.10 \(#18512\)

* chore\(deps\): bump handlebars to 4.7.10

* ci: trigger checks

---------

Co-authored-by: github-actions\[bot\] <41898282+github-actions\[bot\]@users.noreply.github.com>
Co-authored-by: langfuse-bot <accounts+githubbot@langfuse.com>")  | Oct 9, 2026  |  
|   |   |   | Jul 7, 2025  |  
|   |   |  [feat(agents): add grill-me skill (](https://github.com/langfuse/langfuse/commit/9717a63321b567ce41b581b8a5d2079d3259f767 "feat\(agents\): add grill-me skill \(#16614\)

feat\(agents\): added a version of the grill-me skill \(shout out @mattpocock\)")[#16614](https://github.com/langfuse/langfuse/pull/16614)[)](https://github.com/langfuse/langfuse/commit/9717a63321b567ce41b581b8a5d2079d3259f767 "feat\(agents\): add grill-me skill \(#16614\)

feat\(agents\): added a version of the grill-me skill \(shout out @mattpocock\)")  | Aug 26, 2026  |  
|   |   |  [feat(topics): classify traces into buckets (](https://github.com/langfuse/langfuse/commit/3fed0dadc5d4d17f465fbd01b18f41f3cf232aff "feat\(topics\): classify traces into buckets \(#17664\)

* feat\(topics\): classify traces into buckets

* add feature flag etc

* move from python to rust

* enable topics for demo@

* remove token budgetting

* fix ui: tabbing

* fix ui: improv

* no random file storage as queue

* shorter transcripts

* datadog tracing

* Allow custom min sample number.

* better progress tracking

* database separation

* perf faster clustering

* embedding queue

* clean UX

* cleanup

* fix

* fix

* cleanup

* no unbound

* cleanup

* simplify queueing, deletion

* no s3

* sessionid, traceid instead of unitid

* simp

* simp

* allow list project ids

* simplify CH schema

* rename schema, simplify

* simplify

* ci

* simplify, fix ci

* consolidate tests

* fix ci worker test, gate env flag

* add db table init script

* fix tests

* add flag parser

* fix tests

* switch to bedrock based cohere embed

* simplify

* use design system compos

* use assembleTranscript

* fix

* remove dev tables

* fix

* switch to bedrock served models

* fix

* fix reasoning

* run CH tables in ci

* fix float precision

* sec

* manual review 1

* slimmer setup

* remove IDs

* datadog monitors

* simplify

* fix

* feat\(worker\): summarize trace batches into topics

Run facet summarization from the assembled transcript on the trace-batch queue, using instance Bedrock credentials, and enqueue older matching traces through the same worker.

Co-authored-by: Cursor <cursoragent@cursor.com>

* feat\(worker\): admit every trace of Topics projects to trace batching

Projects in LANGFUSE_TOPICS_ENABLED_PROJECT_IDS bypass the trace-batch sampling
rate at ingestion, so each of their traces reaches the trace-batch queue and is
summarized. With LANGFUSE_TRACE_BATCH_SAMPLING_RATE=0 the automated flow runs
for those projects only.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* perf\(worker\): skip trace-batch tracking at sampling rate 0

At LANGFUSE_TRACE_BATCH_SAMPLING_RATE=0 only Topics-enabled projects are
admitted; for all other projects ingestion now returns before building
per-trace state, recording metrics or calling Redis.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* feat\(worker\): default trace-batch sampling to off

LANGFUSE_TRACE_BATCH_SAMPLING_RATE now defaults to 0: with trace-batch
ingestion enabled and no rate set, only Topics-enabled projects are admitted
and all other projects are skipped. Set a rate to sample other projects.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* fix\(worker\): pin trace-batch idle time in tests and drop unused exports

The trace-batch idle default is 2 minutes on DEV, which the scheduling test
ran under in CI; pin the 10-minute value it asserts. Make two helpers that are
only used in their own module private so knip passes.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* feat\(worker\): keep trace batches running past failed Topics traces

A trace whose Topics summary fails no longer fails the batch job. Every trace
is processed, outcomes are counted once per job, and failed traces are
re-enqueued as a new trace-batch job with a retry counter and doubling delay,
up to LANGFUSE_TOPICS_TRACE_MAX_RETRIES. Errors without a provider reason,
such as the input limit, are not retried. Only a failed batch read still fails
the job.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* fix CI

* fix\(worker\): count failed Topics traces without re-enqueueing trace batches

A failed Topics summary re-enqueued its trace as a new trace-batch job,
which read the shared batch from ClickHouse again. During a provider
outage every affected trace repeated that read. Failed traces are now
only counted in langfuse.topics.trace_outcomes by reason.

Co-authored-by: Cursor <cursoragent@cursor.com>

* fix topics positioning

* fix data model after valeriy

* switch to eval tables

* ch migrations are real

* simplify

* feat\(topics\): configure summary and embedding models

* fix\(topics\): gate every model pipeline stage

* fix\(topics\): use geo Bedrock profile IDs

* fix\(topics\): use regional embedding profiles

* docs\(topics\): label PoC env settings as internal

* fix\(topics\): satisfy render lint rules

* test\(topics\): expect env-configured embedding model

* fix\(native\): link N-API symbols in Rust tests

* test\(topics\): update model config expectations

* environtment no default

* slimmer tests

* CH settings on table

* use lightweight deletes

* rename journals to execution store

* empty default env i guess

* move env var

* fix\(clickhouse\): rebuild local databases during reset

* clean

* fix useeffect

* cleanup

* fix

* remove split view

---------

Co-authored-by: Lysander Kiesel <44638111+LysanderKie@users.noreply.github.com>
Co-authored-by: Cursor <cursoragent@cursor.com>
Co-authored-by: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>")[#17664](https://github.com/langfuse/langfuse/pull/17664)[)](https://github.com/langfuse/langfuse/commit/3fed0dadc5d4d17f465fbd01b18f41f3cf232aff "feat\(topics\): classify traces into buckets \(#17664\)

* feat\(topics\): classify traces into buckets

* add feature flag etc

* move from python to rust

* enable topics for demo@

* remove token budgetting

* fix ui: tabbing

* fix ui: improv

* no random file storage as queue

* shorter transcripts

* datadog tracing

* Allow custom min sample number.

* better progress tracking

* database separation

* perf faster clustering

* embedding queue

* clean UX

* cleanup

* fix

* fix

* cleanup

* no unbound

* cleanup

* simplify queueing, deletion

* no s3

* sessionid, traceid instead of unitid

* simp

* simp

* allow list project ids

* simplify CH schema

* rename schema, simplify

* simplify

* ci

* simplify, fix ci

* consolidate tests

* fix ci worker test, gate env flag

* add db table init script

* fix tests

* add flag parser

* fix tests

* switch to bedrock based cohere embed

* simplify

* use design system compos

* use assembleTranscript

* fix

* remove dev tables

* fix

* switch to bedrock served models

* fix

* fix reasoning

* run CH tables in ci

* fix float precision

* sec

* manual review 1

* slimmer setup

* remove IDs

* datadog monitors

* simplify

* fix

* feat\(worker\): summarize trace batches into topics

Run facet summarization from the assembled transcript on the trace-batch queue, using instance Bedrock credentials, and enqueue older matching traces through the same worker.

Co-authored-by: Cursor <cursoragent@cursor.com>

* feat\(worker\): admit every trace of Topics projects to trace batching

Projects in LANGFUSE_TOPICS_ENABLED_PROJECT_IDS bypass the trace-batch sampling
rate at ingestion, so each of their traces reaches the trace-batch queue and is
summarized. With LANGFUSE_TRACE_BATCH_SAMPLING_RATE=0 the automated flow runs
for those projects only.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* perf\(worker\): skip trace-batch tracking at sampling rate 0

At LANGFUSE_TRACE_BATCH_SAMPLING_RATE=0 only Topics-enabled projects are
admitted; for all other projects ingestion now returns before building
per-trace state, recording metrics or calling Redis.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* feat\(worker\): default trace-batch sampling to off

LANGFUSE_TRACE_BATCH_SAMPLING_RATE now defaults to 0: with trace-batch
ingestion enabled and no rate set, only Topics-enabled projects are admitted
and all other projects are skipped. Set a rate to sample other projects.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* fix\(worker\): pin trace-batch idle time in tests and drop unused exports

The trace-batch idle default is 2 minutes on DEV, which the scheduling test
ran under in CI; pin the 10-minute value it asserts. Make two helpers that are
only used in their own module private so knip passes.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* feat\(worker\): keep trace batches running past failed Topics traces

A trace whose Topics summary fails no longer fails the batch job. Every trace
is processed, outcomes are counted once per job, and failed traces are
re-enqueued as a new trace-batch job with a retry counter and doubling delay,
up to LANGFUSE_TOPICS_TRACE_MAX_RETRIES. Errors without a provider reason,
such as the input limit, are not retried. Only a failed batch read still fails
the job.

Co-Authored-By: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>

* fix CI

* fix\(worker\): count failed Topics traces without re-enqueueing trace batches

A failed Topics summary re-enqueued its trace as a new trace-batch job,
which read the shared batch from ClickHouse again. During a provider
outage every affected trace repeated that read. Failed traces are now
only counted in langfuse.topics.trace_outcomes by reason.

Co-authored-by: Cursor <cursoragent@cursor.com>

* fix topics positioning

* fix data model after valeriy

* switch to eval tables

* ch migrations are real

* simplify

* feat\(topics\): configure summary and embedding models

* fix\(topics\): gate every model pipeline stage

* fix\(topics\): use geo Bedrock profile IDs

* fix\(topics\): use regional embedding profiles

* docs\(topics\): label PoC env settings as internal

* fix\(topics\): satisfy render lint rules

* test\(topics\): expect env-configured embedding model

* fix\(native\): link N-API symbols in Rust tests

* test\(topics\): update model config expectations

* environtment no default

* slimmer tests

* CH settings on table

* use lightweight deletes

* rename journals to execution store

* empty default env i guess

* move env var

* fix\(clickhouse\): rebuild local databases during reset

* clean

* fix useeffect

* cleanup

* fix

* remove split view

---------

Co-authored-by: Lysander Kiesel <44638111+LysanderKie@users.noreply.github.com>
Co-authored-by: Cursor <cursoragent@cursor.com>
Co-authored-by: Claude Opus 5.5 \(1M context\) <noreply@anthropic.com>")  | Oct 6, 2026  |  
|   |   |  [refactor(web): migrate test framework from Jest to Vitest (](https://github.com/langfuse/langfuse/commit/585b50ae767083d194c188680818861bb2ce50ba "refactor\(web\): migrate test framework from Jest to Vitest \(#13191\)

* refactor\(web\): migrate test framework from Jest to Vitest

Replace Jest with Vitest in the web package for faster test execution
and native ESM/TypeScript support without requiring next/jest SWC transforms.

- Replace jest/jest-environment-jsdom/@types/jest with vitest/@vitejs/plugin-react/vite-tsconfig-paths
- Add vitest.config.mts with 3 projects \(client/server/e2e-server\) matching the original Jest config
- Convert all jest.* API calls to vi.* equivalents across ~28 test files
- Convert jest.requireActual\(\) to async vi.importActual\(\) with async factory functions
- Remove @jest-environment docblock pragmas \(environment set in vitest config\)
- Add resolveWorkspaceDeps vite plugin for pnpm strict mode compatibility
- Set testTimeout to 30s to match previous Jest/next-jest default

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(web\): fix type error in jumptoplayground test mock

Cast vi.importActual\("zod"\) to any to satisfy both the TypeScript
compiler and the consistent-type-imports lint rule.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix: remove jest from shared tsconfig types

The nextjs.json shared tsconfig included "jest" in the types array,
which required @types/jest to be installed. Since we migrated to vitest,
vitest globals are provided via web/vitest-env.d.ts instead.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix: update CI pipeline to use vitest instead of jest

Replace jest CLI calls with vitest equivalents in the GitHub Actions
pipeline. Also regenerate pnpm-lock.yaml to remove stale jest entries.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(web\): include client project in test:watch

The old Jest test:watch ran all projects. The Vitest migration narrowed
it to only --project server, silently excluding *.clienttest.{ts,tsx}
files from watch mode.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* refactor\(web\): simplify vitest shared aliases and add web to root workspace

- Auto-generate @langfuse/shared resolve aliases from its package.json
  exports instead of hardcoding each subpath
- Add web to root vitest.workspace.ts alongside worker

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(web\): address PR review feedback

- Update AGENTS.md quick commands to use vitest positional patterns
  instead of Jest --testPathPatterns flag
- Move admin-access-webhook.servertest.ts from src/__tests__/async/ to
  src/__tests__/server/async/ so it matches the server project include
  pattern \(was silently orphaned under both Jest and Vitest\)

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(web\): fix admin-access-webhook test for 5-minute dedupe window

The dedupe window was increased from 60s to 5 minutes in bbd209194 but
the test was never updated because it was orphaned \(not picked up by any
test project\). Now that it runs, fix the test to advance the clock past
the actual 5-minute window.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* refactor\(web\): simplify vitest config using deps.inline

Replace the custom resolve aliases, resolveWorkspaceDeps plugin, and
esbuild.tsconfigRaw workaround with a single deps.inline config — the
same approach used by the worker package. This tells Vitest to process
@langfuse/* packages through its transform pipeline using normal Node
resolution \(following pnpm symlinks\) instead of Vite's strict exports
resolver.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* chore\(web\): remove unnecessary vitest-env.d.ts

Test files are excluded from the build tsconfig, and vitest injects
global types at runtime when globals: true is set. The declaration
file is not needed.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(web\): add vitest/globals to tsconfig types and update docs

- Add "vitest/globals" to web/tsconfig.json types so Next.js build
  can resolve describe/it/expect in test files \(fixes CI build error\)
- Move @testing-library/jest-dom/vitest to client project setupFiles
  instead of per-file imports
- Update CONTRIBUTING.md, AGENTS.md, and skill reference docs to
  replace Jest references with Vitest

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(web\): add @testing-library/jest-dom to tsconfig types

Without this, IDE shows type errors on jest-dom matchers like
toBeInTheDocument\(\) in client test files. The setupFiles config
registers the matchers at runtime but doesn't provide the types.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix: correct file location paths in testing-guide.md

Update three embedded file location annotations from async/ to server/
to match the actual directory structure.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(web\): enable parallel server tests, clean up CONTRIBUTING.md diff, revert turborepo skill

- Remove fileParallelism: false from server/e2e-server projects to
  enable parallel test execution \(faster CI\)
- Rewrite CONTRIBUTING.md changes preserving original CRLF line endings
  to minimize diff noise
- Revert turborepo dependencies.md change \(generic skill, not repo-specific\)

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(web\): restore sequential server tests, clean up docs

- Add maxWorkers: 1 to server/e2e-server projects to match Jest's
  --runInBand \(shared DB requires sequential execution\)
- Clean CONTRIBUTING.md diff \(preserve CRLF line endings\)
- Revert turborepo skill change \(generic skill, not repo-specific\)

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix: align CONTRIBUTING.md test command description with example

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(web\): convert remaining jest.* calls in controller.clienttest.ts

New test code merged from main still used jest.mock/jest.fn/jest.mocked
instead of vi.* equivalents.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

---------

Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>
Co-authored-by: Nimar <l.nimar.b@gmail.com>")[#13191](https://github.com/langfuse/langfuse/pull/13191)[)](https://github.com/langfuse/langfuse/commit/585b50ae767083d194c188680818861bb2ce50ba "refactor\(web\): migrate test framework from Jest to Vitest \(#13191\)

* refactor\(web\): migrate test framework from Jest to Vitest

Replace Jest with Vitest in the web package for faster test execution
and native ESM/TypeScript support without requiring next/jest SWC transforms.

- Replace jest/jest-environment-jsdom/@types/jest with vitest/@vitejs/plugin-react/vite-tsconfig-paths
- Add vitest.config.mts with 3 projects \(client/server/e2e-server\) matching the original Jest config
- Convert all jest.* API calls to vi.* equivalents across ~28 test files
- Convert jest.requireActual\(\) to async vi.importActual\(\) with async factory functions
- Remove @jest-environment docblock pragmas \(environment set in vitest config\)
- Add resolveWorkspaceDeps vite plugin for pnpm strict mode compatibility
- Set testTimeout to 30s to match previous Jest/next-jest default

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(web\): fix type error in jumptoplayground test mock

Cast vi.importActual\("zod"\) to any to satisfy both the TypeScript
compiler and the consistent-type-imports lint rule.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix: remove jest from shared tsconfig types

The nextjs.json shared tsconfig included "jest" in the types array,
which required @types/jest to be installed. Since we migrated to vitest,
vitest globals are provided via web/vitest-env.d.ts instead.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix: update CI pipeline to use vitest instead of jest

Replace jest CLI calls with vitest equivalents in the GitHub Actions
pipeline. Also regenerate pnpm-lock.yaml to remove stale jest entries.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(web\): include client project in test:watch

The old Jest test:watch ran all projects. The Vitest migration narrowed
it to only --project server, silently excluding *.clienttest.{ts,tsx}
files from watch mode.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* refactor\(web\): simplify vitest shared aliases and add web to root workspace

- Auto-generate @langfuse/shared resolve aliases from its package.json
  exports instead of hardcoding each subpath
- Add web to root vitest.workspace.ts alongside worker

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(web\): address PR review feedback

- Update AGENTS.md quick commands to use vitest positional patterns
  instead of Jest --testPathPatterns flag
- Move admin-access-webhook.servertest.ts from src/__tests__/async/ to
  src/__tests__/server/async/ so it matches the server project include
  pattern \(was silently orphaned under both Jest and Vitest\)

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(web\): fix admin-access-webhook test for 5-minute dedupe window

The dedupe window was increased from 60s to 5 minutes in bbd209194 but
the test was never updated because it was orphaned \(not picked up by any
test project\). Now that it runs, fix the test to advance the clock past
the actual 5-minute window.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* refactor\(web\): simplify vitest config using deps.inline

Replace the custom resolve aliases, resolveWorkspaceDeps plugin, and
esbuild.tsconfigRaw workaround with a single deps.inline config — the
same approach used by the worker package. This tells Vitest to process
@langfuse/* packages through its transform pipeline using normal Node
resolution \(following pnpm symlinks\) instead of Vite's strict exports
resolver.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* chore\(web\): remove unnecessary vitest-env.d.ts

Test files are excluded from the build tsconfig, and vitest injects
global types at runtime when globals: true is set. The declaration
file is not needed.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(web\): add vitest/globals to tsconfig types and update docs

- Add "vitest/globals" to web/tsconfig.json types so Next.js build
  can resolve describe/it/expect in test files \(fixes CI build error\)
- Move @testing-library/jest-dom/vitest to client project setupFiles
  instead of per-file imports
- Update CONTRIBUTING.md, AGENTS.md, and skill reference docs to
  replace Jest references with Vitest

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(web\): add @testing-library/jest-dom to tsconfig types

Without this, IDE shows type errors on jest-dom matchers like
toBeInTheDocument\(\) in client test files. The setupFiles config
registers the matchers at runtime but doesn't provide the types.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix: correct file location paths in testing-guide.md

Update three embedded file location annotations from async/ to server/
to match the actual directory structure.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(web\): enable parallel server tests, clean up CONTRIBUTING.md diff, revert turborepo skill

- Remove fileParallelism: false from server/e2e-server projects to
  enable parallel test execution \(faster CI\)
- Rewrite CONTRIBUTING.md changes preserving original CRLF line endings
  to minimize diff noise
- Revert turborepo dependencies.md change \(generic skill, not repo-specific\)

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(web\): restore sequential server tests, clean up docs

- Add maxWorkers: 1 to server/e2e-server projects to match Jest's
  --runInBand \(shared DB requires sequential execution\)
- Clean CONTRIBUTING.md diff \(preserve CRLF line endings\)
- Revert turborepo skill change \(generic skill, not repo-specific\)

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix: align CONTRIBUTING.md test command description with example

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

* fix\(web\): convert remaining jest.* calls in controller.clienttest.ts

New test code merged from main still used jest.mock/jest.fn/jest.mocked
instead of vi.* equivalents.

Co-Authored-By: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>

---------

Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>
Co-authored-by: Nimar <l.nimar.b@gmail.com>")  | Apr 22, 2026  |  
| View all files |  
## Repository files navigation
###  [ **Langfuse Cloud** ](https://cloud.langfuse.com) · [ **Self Host** ](https://langfuse.com/docs/deployment/self-host) · [ **Demo** ](https://langfuse.com/demo)
[**Docs**](https://langfuse.com/docs) · [**Report Bug**](https://langfuse.com/issues) · [**Feature Request**](https://langfuse.com/ideas) · [**Changelog**](https://langfuse.com/changelog) · [**Roadmap**](https://langfuse.com/roadmap) · 
Langfuse is an **open source LLM engineering** platform. It helps teams collaboratively **develop, monitor, evaluate,** and **debug** AI applications. Langfuse can be **self-hosted in minutes** and is **battle-tested**. Proudly made with [ClickHouse open source database](https://github.com/ClickHouse/ClickHouse).
> ### 🧑‍💻 We're hiring
> Langfuse is growing fast (we doubled the team in the last 6 months) - since January 2026 we're part of ClickHouse, we're hiring engineering hybrid across the EU. We hire engineers who love open source and great developer experiences. 
## ✨ Core Features
  * [LLM Application Observability](https://langfuse.com/docs/tracing): Instrument your app and start ingesting traces to Langfuse, thereby tracking LLM calls and other relevant logic in your app such as retrieval, embedding, or agent actions. Inspect and debug complex logs and user sessions. Try the interactive [demo](https://langfuse.com/docs/demo) to see this in action.
  * [Prompt Management](https://langfuse.com/docs/prompt-management/get-started) helps you centrally manage, version control, and collaboratively iterate on your prompts. Thanks to strong caching on server and client side, you can iterate on prompts without adding latency to your application.
  * [Evaluations](https://langfuse.com/docs/evaluation/overview) are key to the LLM application development workflow, and Langfuse adapts to your needs. It supports LLM-as-a-judge, Code evaluators, user feedback collection, manual labeling, and custom evaluation pipelines via APIs/SDKs.
  * [Datasets](https://langfuse.com/docs/evaluation/dataset-runs/datasets) enable test sets and benchmarks for evaluating your LLM application. They support continuous improvement, pre-deployment testing, structured experiments, flexible evaluation, and seamless integration with frameworks like LangChain and LlamaIndex.
  * [LLM Playground](https://langfuse.com/docs/playground) is a tool for testing and iterating on your prompts and model configurations, shortening the feedback loop and accelerating development. When you see a bad result in tracing, you can directly jump to the playground to iterate on it.
  * [Comprehensive API](https://langfuse.com/docs/api): Langfuse is frequently used to power bespoke LLMOps workflows while using the building blocks provided by Langfuse via the API. OpenAPI spec, Postman collection, and typed SDKs for Python, JS/TS are available.


## 📦 Deploy Langfuse
### Langfuse Cloud
Managed deployment by the Langfuse team, generous free-tier, no credit card required.
### Self-Host Langfuse
Run Langfuse on your own infrastructure:
  * [Local (docker compose)](https://langfuse.com/self-hosting/local): Run Langfuse on your own machine in 5 minutes using Docker Compose.

```
# Get a copy of the latest Langfuse repository
git clone --depth=1 https://github.com/langfuse/langfuse.git
cd langfuse

# Run the langfuse docker compose
docker compose up
```

  * [VM](https://langfuse.com/self-hosting/docker-compose): Run Langfuse on a single Virtual Machine using Docker Compose.
  * [Kubernetes (Helm)](https://langfuse.com/self-hosting/kubernetes-helm): Run Langfuse on a Kubernetes cluster using Helm. This is the preferred production deployment.
  * Terraform Templates: [AWS](https://langfuse.com/self-hosting/aws), [Azure](https://langfuse.com/self-hosting/azure), [GCP](https://langfuse.com/self-hosting/gcp)


See [self-hosting documentation](https://langfuse.com/self-hosting) to learn more about architecture and configuration options.
#### Docker log rotation
The default `docker-compose.yml` inherits the [Docker daemon's logging configuration](https://docs.docker.com/engine/logging/configure/). Docker's default `json-file` driver does not rotate logs unless configured, which can exhaust disk space. Set its [`max-size` and `max-file` options](https://docs.docker.com/engine/logging/drivers/json-file/), or keep your chosen logging backend and configure retention there. Do not rotate or truncate Docker-managed JSON log files with external tools.
After changing daemon logging defaults, restart Docker and recreate existing containers to apply them; restarting containers alone is insufficient. Plan for interruption and preserve data volumes. These settings cover container stdout/stderr, not database data volumes or ClickHouse's internal log files.
Tip
**Self-hosting Langfuse?** Subscribe to the [self-hosting update list](https://langfuse.com/self-hosting#subscribe) to get an email about important features and new releases for open source Langfuse — self-hosting updates only, no marketing.
## 🔌 Integrations
### Main Integrations:  
| Integration  | Supports  | Description  |  
| --- | --- | --- |  
| Python, JS/TS  | Manual instrumentation using the SDKs for full flexibility.  |  
| Python, JS/TS  | Automated instrumentation using drop-in replacement of OpenAI SDK.  |  
| Python, JS/TS  | Automated instrumentation by passing callback handler to Langchain application.  |  
| Python  | Automated instrumentation via LlamaIndex callback system.  |  
| Python  | Automated instrumentation via Haystack content tracing system.  |  
| Python, JS/TS (proxy only)  | Use any LLM as a drop in replacement for GPT. Use Azure, OpenAI, Cohere, Anthropic, Ollama, VLLM, Sagemaker, HuggingFace, Replicate (100+ LLMs).  |  
| JS/TS  | TypeScript toolkit designed to help developers build AI-powered applications with React, Next.js, Vue, Svelte, Node.js.  |  
| JS/TS  | Open source framework for building AI agents and multi-agent systems.  |  
| Directly call the public API. OpenAPI spec available.  |  
### Packages integrated with Langfuse:  
| Name  | Type  | Description  |  
| --- | --- | --- |  
| Library  | Library to get structured LLM outputs (JSON, Pydantic)  |  
| Library  | Framework that systematically optimizes language model prompts and weights  |  
| Library  | Python toolkit for building LLM applications.  |  
| Model (local)  | Easily run open source LLMs on your own machine.  |  
| Model  | Run foundation and fine-tuned models on AWS.  |  
| Agent Framework  | Open source LLM platform for building distributed agents.  |  
| Chat/Agent UI  | JS/TS no-code builder for customized LLM flows.  |  
| Chat/Agent UI  | Python-based UI for LangChain, designed with react-flow to provide an effortless way to experiment and prototype flows.  |  
| Chat/Agent UI  | Open source LLM app development platform with no-code builder.  |  
| Chat/Agent UI  | Self-hosted LLM Chat web ui supporting various LLM runners including self-hosted and local models.  |  
| Tool  | Open source LLM testing platform.  |  
| Chat/Agent UI  | Open source chatbot platform.  |  
| Platform  | Open source voice AI platform.  |  
| Agents  | Open source LLM platform for building distributed agents.  |  
| Chat/Agent UI  | Open source Python library to build web interfaces like Chat UI.  |  
| Agents  | Open source LLM platform for building distributed agents.  |  
| Agents  | Open source AI agents framework.  |  
| Agents  | Multi agent framework for agent collaboration and tool use.  |  
## 🚀 Quickstart
Instrument your app and start ingesting traces to Langfuse, thereby tracking LLM calls and other relevant logic in your app such as retrieval, embedding, or agent actions. Inspect and debug complex logs and user sessions.
### 1️⃣ Create new project
  1. [Create Langfuse account](https://cloud.langfuse.com/auth/sign-up) or [self-host](https://langfuse.com/self-hosting)
  2. Create a new project
  3. Create new API credentials in the project settings


### 2️⃣ Log your first LLM call
The [`@observe()` decorator](https://langfuse.com/docs/sdk/python/decorators) makes it easy to trace any Python LLM application. In this quickstart we also use the Langfuse [OpenAI integration](https://langfuse.com/integrations/model-providers/openai-py) to automatically capture all model parameters.
Tip
Not using OpenAI? Visit [our documentation](https://langfuse.com/docs/get-started#log-your-first-llm-call-to-langfuse) to learn how to log other models and frameworks.

```
pip install langfuse openai
```


```
LANGFUSE_SECRET_KEY="sk-lf-..."
LANGFUSE_PUBLIC_KEY="pk-lf-..."
LANGFUSE_BASE_URL="https://cloud.langfuse.com" # 🇪🇺 EU region
# LANGFUSE_BASE_URL="https://us.cloud.langfuse.com" # 🇺🇸 US region
```


```
from langfuse import observe
from langfuse.openai import openai # OpenAI integration

@observe()
def story():
    return openai.chat.completions.create(
        model="gpt-4o",
        messages=[{"role": "user", "content": "What is Langfuse?"}],
    ).choices[0].message.content

@observe()
def main():
    return story()

main()
```

### 3️⃣ See traces in Langfuse
See your language model calls and other application logic in Langfuse.
_[Public example trace in Langfuse](https://cloud.langfuse.com/project/cloramnkj0002jz088vzn1ja4/traces/db22d0a442216b485abd83cc9df6d9ee)_
## 💭 Support
Finding an answer to your question:
  * Our [documentation](https://langfuse.com/docs) is the best place to start looking for answers. It is comprehensive, and we invest significant time into maintaining it. You can also suggest edits to the docs via GitHub.
  * [Langfuse FAQs](https://langfuse.com/faq) where the most common questions are answered.
  * Use "[Ask AI](https://langfuse.com/docs/ask-ai)" to get instant answers to your questions.


Support Channels:
  * **Ask any question in our[public Q&A](https://github.com/orgs/langfuse/discussions/categories/support) on GitHub Discussions.** Please include as much detail as possible (e.g. code snippets, screenshots, background information) to help us understand your question.
  * [Request a feature](https://github.com/orgs/langfuse/discussions/categories/ideas) on GitHub Discussions.
  * [Report a Bug](https://github.com/langfuse/langfuse/issues) on GitHub Issues.
  * For time-sensitive queries, ping us via the in-app chat widget.


## 🤝 Contributing
Your contributions are welcome!
  * Vote on [Ideas](https://github.com/orgs/langfuse/discussions/categories/ideas) in GitHub Discussions.
  * Raise and comment on [Issues](https://github.com/langfuse/langfuse/issues).
  * Open a PR - see [CONTRIBUTING.md](https://github.com/langfuse/langfuse/blob/main/CONTRIBUTING.md) for details on how to setup a development environment.


## 🥇 License
This repository is MIT licensed, except for the `ee` folders. See [LICENSE](https://github.com/langfuse/langfuse/blob/main/LICENSE) and [docs](https://langfuse.com/docs/open-source) for more details.
## Dependencies
We deploy this code base in Docker containers based on the Linux Alpine Image ([source](https://github.com/nodejs/docker-node)). You may find the Dockerfiles in [web/Dockerfile](https://github.com/langfuse/langfuse/blob/main/web/Dockerfile) and [worker/Dockerfile](https://github.com/langfuse/langfuse/blob/main/worker/Dockerfile).
## ⭐️ Star History
## ❤️ Open Source Projects Using Langfuse
Top open-source Python projects that use Langfuse, ranked by stars ([Source](https://github.com/langfuse/langfuse-docs/blob/main/components-mdx/dependents)):  
| Repository  | Stars  |  
| --- | --- |  
| 116251  |  
| 109642  |  
| 70877  |  
| 65454  |  
| 64118  |  
| 56713  |  
| 44203  |  
| 43547  |  
| 38415  |  
|  [microsoft](https://github.com/microsoft) / [ai-agents-for-beginners](https://github.com/microsoft/ai-agents-for-beginners)  | 38012  |  
|  [chatchat-space](https://github.com/chatchat-space) / [Langchain-Chatchat](https://github.com/chatchat-space/Langchain-Chatchat)  | 36071  |  
| 35669  |  
| 33142  |  
| 28726  |  
| 22447  |  
| 21748  |  
| 17976  |  
| 17057  |  
| 16484  |  
| 16054  |  
| 11868  |  
| 8350  |  
| 8313  |  
| 7093  |  
| 7011  |  
| 6785  |  
| 6231  |  
| 6019  |  
| 4873  |  
| 4654  |  
| 4165  |  
|  [JoshuaC215](https://github.com/JoshuaC215) / [agent-service-toolkit](https://github.com/JoshuaC215/agent-service-toolkit)  | 3557  |  
| 3517  |  
| 3210  |  
| 3010  |  
| 2651  |  
| 2570  |  
| 2055  |  
| 1734  |  
|  [TheSpaghettiDetective](https://github.com/TheSpaghettiDetective) / [obico-server](https://github.com/TheSpaghettiDetective/obico-server)  | 1687  |  
| 1387  |  
|  [TIGER-AI-Lab](https://github.com/TIGER-AI-Lab) / [TheoremExplainAgent](https://github.com/TIGER-AI-Lab/TheoremExplainAgent)  | 1385  |  
| 1223  |  
|  [wassim249](https://github.com/wassim249) / [fastapi-langgraph-agent-production-ready-template](https://github.com/wassim249/fastapi-langgraph-agent-production-ready-template)  | 1200  |  
| 1098  |  
| 1010  |  
| 993  |  
| 939  |  
| 927  |  
| 857  |  
| 845  |  
| 829  |  
| 781  |  
| 719  |  
| 688  |  
| 643  |  
| 587  |  
| 487  |  
|  [NicholasGoh](https://github.com/NicholasGoh) / [fastapi-mcp-langgraph-template](https://github.com/NicholasGoh/fastapi-mcp-langgraph-template)  | 481  |  
| 472  |  
| 470  |  
|  [phospho-app](https://github.com/phospho-app) / [text-analytics-legacy](https://github.com/phospho-app/text-analytics-legacy)  | 439  |  
| 403  |  
| 397  |  
| 385  |  
| 380  |  
| 373  |  
|  [ElectricCodeGuy](https://github.com/ElectricCodeGuy) / [SupabaseAuthWithSSR](https://github.com/ElectricCodeGuy/SupabaseAuthWithSSR)  | 370  |  
| 323  |  
|  [aws-samples](https://github.com/aws-samples) / [aws-ai-ml-workshop-kr](https://github.com/aws-samples/aws-ai-ml-workshop-kr)  | 295  |  
| 285  |  
| 276  |  
| 275  |  
| 269  |  
| 241  |  
| 238  |  
|  [bklieger-groq](https://github.com/bklieger-groq) / [mathtutor-on-groq](https://github.com/bklieger-groq/mathtutor-on-groq)  | 233  |  
| 224  |  
| 202  |  
|  [zhutoutoutousan](https://github.com/zhutoutoutousan) / [worldquant-miner](https://github.com/zhutoutoutousan/worldquant-miner)  | 202  |  
| 186  |  
| 181  |  
| 181  |  
|  [aws-samples](https://github.com/aws-samples) / [sample-serverless-mcp-servers](https://github.com/aws-samples/sample-serverless-mcp-servers)  | 175  |  
| 171  |  
| 164  |  
| 163  |  
|  [deepset-ai](https://github.com/deepset-ai) / [haystack-core-integrations](https://github.com/deepset-ai/haystack-core-integrations)  | 163  |  
| 152  |  
| 150  |  
|  [chatchat-space](https://github.com/chatchat-space) / [LangGraph-Chatchat](https://github.com/chatchat-space/LangGraph-Chatchat)  | 144  |  
| 139  |  
| 135  |  
| 132  |  
| 127  |  
| 121  |  
| 119  |  
## 🔒 Security & Privacy
We take data security and privacy seriously. Please refer to our [Security and Privacy](https://langfuse.com/security) page for more information.
### Telemetry
By default, Langfuse automatically reports basic usage statistics of self-hosted instances to a centralized server (PostHog).
This helps us to:
  1. Understand how Langfuse is used and improve the most relevant features.
  2. Track overall usage for internal and external (e.g. fundraising) reporting.


The telemetry does not include raw traces, prompts, observations, scores, or dataset contents. We document the exact fields that are collected, where they are sent, and the implementation reference in our [telemetry docs](https://langfuse.com/self-hosting/security/telemetry).
For Langfuse OSS, you can opt out by setting `TELEMETRY_ENABLED=false`.
## About
🪢 Open source agent evals & observability: Trace, evaluate, and improve LLM applications with one open platform.
### Topics
[analytics](https://github.com/topics/analytics)[autogen](https://github.com/topics/autogen)[evaluation](https://github.com/topics/evaluation)[langchain](https://github.com/topics/langchain)[large-language-models](https://github.com/topics/large-language-models)[llama-index](https://github.com/topics/llama-index)[llm](https://github.com/topics/llm)[llm-evaluation](https://github.com/topics/llm-evaluation)[llm-observability](https://github.com/topics/llm-observability)[llmops](https://github.com/topics/llmops)[monitoring](https://github.com/topics/monitoring)[observability](https://github.com/topics/observability)[open-source](https://github.com/topics/open-source)[openai](https://github.com/topics/openai)[playground](https://github.com/topics/playground)[prompt-engineering](https://github.com/topics/prompt-engineering)[prompt-management](https://github.com/topics/prompt-management)[self-hosted](https://github.com/topics/self-hosted)[ycombinator](https://github.com/topics/ycombinator)
### Resources
### Contributing
### Security policy
### Stars
**35.6k** stars
### Watchers
**114** watching
### Forks
[**4.0k** forks](https://github.com/langfuse/langfuse/forks)
## 
[ v4.56.0Latest Oct 9, 2026 ](https://github.com/langfuse/langfuse/releases/tag/v4.56.0)
## 
## Languages
  * [TypeScript96.8%](https://github.com/langfuse/langfuse/search?l=typescript)
  * [Rust1.6%](https://github.com/langfuse/langfuse/search?l=rust)
  * [JavaScript0.9%](https://github.com/langfuse/langfuse/search?l=javascript)
  * [Shell0.3%](https://github.com/langfuse/langfuse/search?l=shell)
  * [CSS0.1%](https://github.com/langfuse/langfuse/search?l=css)
  * [PLpgSQL0.1%](https://github.com/langfuse/langfuse/search?l=plpgsql)
  * [Other0.2%](https://github.com/langfuse/langfuse/search?l=Other)


You can’t perform that action at this time. 
