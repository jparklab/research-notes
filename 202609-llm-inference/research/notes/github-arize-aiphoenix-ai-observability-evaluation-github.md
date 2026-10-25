---
title: 'GitHub - Arize-ai/phoenix: AI Observability & Evaluation · GitHub'
id: github-arize-aiphoenix-ai-observability-evaluation-github
tags:
- observability
- llm-observability
- self-hosted
created: '2026-10-09T23:14:59.915944Z'
updated: '2026-10-10T14:52:17.240490Z'
source: https://github.com/Arize-ai/phoenix
source_domain: github.com
fetched_at: '2026-10-09T23:14:59.892000Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Arize Phoenix GitHub README/repo page: open-source AI observability platform
  for tracing (OpenTelemetry-based), LLM-as-judge evaluation, versioned datasets/experiments,
  prompt playground/management, and an MCP server for agent/IDE integration. License
  is Elastic License 2.0 (ELv2) — NOT OSI-approved open source — stated explicitly
  in the repo''s license section alongside a patent notice. ''Phoenix is open-source
  and self-hosted''; the paid/managed counterpart is Arize AX, sharing the same OpenInference/OpenTelemetry
  instrumentation layer so traces are portable between the self-hosted and managed
  tiers. 11.8k GitHub stars, 61 watchers, 1.2k forks, ~10,256 commits, actively maintained
  by Arize with commits as recent as Oct 9 2026. Vendor/language-agnostic instrumentation
  with out-of-the-box integrations for OpenAI Agents SDK, Claude Agent SDK, LangGraph,
  Vercel AI SDK, CrewAI, LlamaIndex, DSPy, Bedrock, LiteLLM — so it occupies the ''owns
  storage+UI platform'' slot (like Langfuse) rather than the vendor-neutral-emitter
  slot, but is ELv2-licensed rather than MIT.'
---

[Skip to content](https://github.com/Arize-ai/phoenix#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/Arize-ai/phoenix) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/Arize-ai/phoenix) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/Arize-ai/phoenix) to refresh your session. Dismiss alert
[**651** Branches](https://github.com/Arize-ai/phoenix/branches)[**916** Tags](https://github.com/Arize-ai/phoenix/tags)
Go to file
Open more actions menu
## Latest commit
and
[refactor(cost-tracking): move model price sync out of .github and dro…](https://github.com/Arize-ai/phoenix/commit/e4713156c6c2eada91d691834954e9d27cae151e)
Open commit detailssuccess
Oct 9, 2026
[e471315](https://github.com/Arize-ai/phoenix/commit/e4713156c6c2eada91d691834954e9d27cae151e) · Oct 9, 2026
## History
[10,256 Commits](https://github.com/Arize-ai/phoenix/commits/main/)
Open commit details
10,256 Commits
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
|   |   |  [docs(skills): weekly audit — 2026-10-07 (](https://github.com/Arize-ai/phoenix/commit/9c8143b862517649050024ff9776db27df4360e4 "docs\(skills\): weekly audit — 2026-10-07 \(#16844\)

Co-authored-by: claude\[bot\] <41898282+claude\[bot\]@users.noreply.github.com>")[#16844](https://github.com/Arize-ai/phoenix/pull/16844)[)](https://github.com/Arize-ai/phoenix/commit/9c8143b862517649050024ff9776db27df4360e4 "docs\(skills\): weekly audit — 2026-10-07 \(#16844\)

Co-authored-by: claude\[bot\] <41898282+claude\[bot\]@users.noreply.github.com>")  | Oct 8, 2026  |  
|   |   | [feat: add Claude Code plugin and marketplace](https://github.com/Arize-ai/phoenix/commit/ff2abd85626fab1d5ce1be58154de23bbfb15592 "feat: add Claude Code plugin and marketplace

Group plugins by agent: plugins/codex/arize-phoenix \(moved\) and
plugins/claude/arize-phoenix \(new\). The root .claude-plugin/marketplace.json
lists the Claude plugin under the same arize-phoenix marketplace name used
for Codex and Cursor.

The Claude plugin registers the remote MCP server over streamable HTTP with
a userConfig "endpoint" option, since plugin MCP configs do not expand
arbitrary environment variables. It ships the three public skills as
symlinks into .agents/skills/, which Claude Code dereferences on install.")  | Sep 10, 2026  |  
|   |   |  [feat(skills): add PHOENIX_ROSTER_INCLUDE to phoenix-github roster (](https://github.com/Arize-ai/phoenix/commit/28282c8d6a858a6d79b1653ed304f12bb83c60e6 "feat\(skills\): add PHOENIX_ROSTER_INCLUDE to phoenix-github roster \(#15289\)

The roster reads live @Arize-ai/oss-eng membership, so collaborators who
carry sprint work without belonging to an org team were invisible to
health.py and standup.py. PHOENIX_ROSTER_INCLUDE adds them by login.

Exclude wins over include, in both the roster and the report label.

Adds Nancy-Chauhan, who has sprint work on board #42 but is not in any
oss-* team.")[#1…](https://github.com/Arize-ai/phoenix/pull/15289)  | Aug 10, 2026  |  
|   |   |  [fix: use Python 3.11 for local environment setup (](https://github.com/Arize-ai/phoenix/commit/fe3a58eff29d41032b3a0e0b8d43283822ce2f45 "fix: use Python 3.11 for local environment setup \(#16555\)")[#16555](https://github.com/Arize-ai/phoenix/pull/16555)[)](https://github.com/Arize-ai/phoenix/commit/fe3a58eff29d41032b3a0e0b8d43283822ce2f45 "fix: use Python 3.11 for local environment setup \(#16555\)")  | Sep 28, 2026  |  
|   |   | [chore: cursor marketplace](https://github.com/Arize-ai/phoenix/commit/b7bbb03594c1948aea0b93c02fc8491081cdbafc "chore: cursor marketplace")  | Sep 9, 2026  |  
|   |   |  [fix: remove dead .cursor/skills/phoenix-skill-development symlink (](https://github.com/Arize-ai/phoenix/commit/1137abd28addda5adf2e3eca1d5dff460b6e37a5 "fix: remove dead .cursor/skills/phoenix-skill-development symlink \(#16327\)")[#1…](https://github.com/Arize-ai/phoenix/pull/16327)  | Sep 21, 2026  |  
|   |   | [refactor(cost-tracking): move model price sync out of .github and dro…](https://github.com/Arize-ai/phoenix/commit/e4713156c6c2eada91d691834954e9d27cae151e "refactor\(cost-tracking\): move model price sync out of .github and drop its tests \(#16908\)

* refactor\(cost-tracking\): move model price sync out of .github so unit tests stop needing it

The Mistral pricing tests loaded .github/.scripts/sync_models.py by path, which
the Unit Tests sparse checkout omits, so CI needed a stopgap checkout entry.

- Move the sync script to scripts/cost_tracking/ with its tests beside it, run
  by `make test-sync-models` and a new Test Model Cost Sync Script CI job.
- Keep the runtime Mistral cost-calculation tests in tests/unit.
- Drop the stopgap .github/.scripts/sync_models.py sparse-checkout entries.
- Add `make check-unit-test-paths` and a CI job that fails when a unit test
  reads a repo path outside the Unit Tests sparse checkout, or when the two
  Unit Tests jobs' checkout lists drift apart.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01QLcWow97hFyWXWxKyuqdEx

* refactor\(cost-tracking\): run sync-script tests in the cost sync workflow, drop path guard

- Run make test-sync-models \(mypy --strict + pytest, lean env\) right before
  the weekday sync instead of in a separate PR CI job
- Remove the static unit-test path guard; the sparse-checkout Unit Tests job
  already fails on a missing path
- Drop test_mistral_cost_calculation.py, which duplicated cases in
  test_cost_details_calculator.py

* refactor\(cost-tracking\): drop sync-script tests, keep a pure move

The Mistral sync tests and the checked-in-manifest schema test duplicate what
the weekday sync and test_model_cost_manifest.py already cover.

---------

Co-authored-by: Claude <noreply@anthropic.com>")  | Oct 9, 2026  |  
|   |   |  [chore(ts): migrate app and js to TypeScript 7 (](https://github.com/Arize-ai/phoenix/commit/bcb9527c85be1735d615805d4971193e2cccee01 "chore\(ts\): migrate app and js to TypeScript 7 \(#14161\)")[#14161](https://github.com/Arize-ai/phoenix/pull/14161)[)](https://github.com/Arize-ai/phoenix/commit/bcb9527c85be1735d615805d4971193e2cccee01 "chore\(ts\): migrate app and js to TypeScript 7 \(#14161\)")  | Jul 10, 2026  |  
|   |   |  [docs: add LinkedIn social links alongside X/Bluesky (](https://github.com/Arize-ai/phoenix/commit/5d14f23598202c062530466a9dbdc9a1a033d6c8 "docs: add LinkedIn social links alongside X/Bluesky \(#12993\)

* docs: add LinkedIn social links alongside X/Bluesky

Adds LinkedIn \(https://www.linkedin.com/showcase/113218220\) wherever X or
Bluesky links appear — README badges and Community sections, package
READMEs \(Python and JS\), Sphinx docs configs, the Mintlify docs.json site
chrome, and the auth layout footer. Also updates the existing LinkedIn
entry in docs.json from the Arize company page to the Phoenix showcase.

* chore: add changeset for LinkedIn README links")[#12993](https://github.com/Arize-ai/phoenix/pull/12993)[)](https://github.com/Arize-ai/phoenix/commit/5d14f23598202c062530466a9dbdc9a1a033d6c8 "docs: add LinkedIn social links alongside X/Bluesky \(#12993\)

* docs: add LinkedIn social links alongside X/Bluesky

Adds LinkedIn \(https://www.linkedin.com/showcase/113218220\) wherever X or
Bluesky links appear — README badges and Community sections, package
READMEs \(Python and JS\), Sphinx docs configs, the Mintlify docs.json site
chrome, and the auth layout footer. Also updates the existing LinkedIn
entry in docs.json from the Arize company page to the Phoenix showcase.

* chore: add changeset for LinkedIn README links")  | May 3, 2026  |  
|   | Oct 9, 2026  |  
|   |   |  [build(evals): load the Harbor plugin from the checkout's client wheel (](https://github.com/Arize-ai/phoenix/commit/248ded98dbfb7478c2a3000c7db7b8f423fbb524 "build\(evals\): load the Harbor plugin from the checkout's client wheel \(#16855\)

make harbor-run installed arize-phoenix-client from PyPI at a pinned release, so
plugin changes merged after that release did not take effect in benchmark runs.
harbor-stage now builds the client wheel from the checkout and harbor-run loads the
plugin from it. HARBOR_CLIENT_VERSION selects a released client instead.")[…](https://github.com/Arize-ai/phoenix/pull/16855)  | Oct 9, 2026  |  
|   |   |  [docs: reorganize Get Started around the agent workflow (](https://github.com/Arize-ai/phoenix/commit/2afdf488e638f282f742c7daf796a09a56662971 "docs: reorganize Get Started around the agent workflow \(#16288\)

* docs: reorganize Get Started around the agent workflow

Restructure the Get Started section to mirror the Arize AX quickstart:
Instrument your agent, Evaluate your agent, Improve your agent, Coding
agent setup, PXI, and Phoenix Demo.

The Python and TypeScript quickstarts previously sat in separate nav
groups, so each language duplicated the whole section tree. Each pair is
now a single page with Python and TypeScript tabs, so the language is a
choice within a section rather than a parallel branch of the nav.

Every page body is carried over verbatim into its language tab. The only
text rewritten is internal links that pointed at the retired
ts-get-started-* paths, which now have redirects to the merged pages.

- Instrument your agent  <- get-started-tracing + ts- counterpart
- Evaluate your agent    <- get-started-evaluations + ts- counterpart
- Improve your agent     <- group over the prompt playground and
                            datasets/experiments guides
- Coding agent setup     <- group over Agent-Assisted Setup and Coding
                            Agents
- MCP group, PXI, and Phoenix Demo are unchanged aside from ordering
- End to End Features Notebook is removed from the nav; the page itself
  is left in place

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: cut duplicated prose from the Get Started guides

The previous commit put each language's full guide in its own tab, which
made every language-agnostic sentence render twice on the same page. On
the prompt playground guide that was 37% of the prose; on datasets and
experiments, 40%.

Where the two tabs told the same story, the narrative is hoisted above
the tabs and the tabs now wrap only what genuinely differs: code, videos,
the companion-code callout, and the two prompt snippets. The prompt
playground and experiments guides had identical step sequences, so their
steps are shared outright. Tracing and evaluations each have an extra
TypeScript step, so their steps stay tabbed and only the intro, prereqs
and closing sections are shared.

No prose was rewritten. Every line either survives verbatim from the
previous commit or is a prefix of one with a redundant trailing clause
removed, verified mechanically. Where the two tabs differed only
trivially, one existing sentence is kept verbatim rather than a new one
written; this also picks up several corrections the TypeScript copy
already had:

- "traces that received an incompleteness evaluation label" ->
  "an incomplete evaluation label", matching the filter beside it
- "Filter your traces by the incorrect evaluation label" -> "incomplete";
  the filter under it has always been evals\['completeness'\] == 'incomplete'
- "Using the explanations apart of the evals" -> "from the evals"
- "New application runs as a results of our task" -> "as a result"
- "ran your first experiment" / "ran an A/B test" -> "run"

Also drops, on the tracing guide, a sentence restating the bullet list
above it and three repeats of the supported-frameworks disclaimer that
the intro already carries.

Verified: all 44 code blocks byte-identical \(multiset\), every step title
and video retained, no duplicate sentence in any rendered page, both link
checkers clean, all four pages render with working tabs.

Words: 10790 -> 8789 \(18.5% shorter\).

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: tighten Instrument and Agent-Assisted Setup, repoint quickstart

Three changes to the Get Started section.

Instrument your agent opened with a paragraph of series framing --
"This guide walks through a complete workflow ... Each guide in this
series introduces one piece of that workflow" -- which is word for word
the Overview page's opening and says nothing about instrumentation. It is
removed here and still lives on the Overview page. The guide now opens on
its own subject: what a trace is.

/docs/phoenix/quickstart redirected to the Overview page, which is not in
the nav. It now lands on Instrument your agent, the actual first guide in
the series. The Overview page is unchanged and still reachable from the
four pages that link to it.

Agent-Assisted Setup is only ~350 words of prose but 89 lines, because
almost two thirds of it is follow-up reference rather than setup. The
three advanced sections -- re-running a single step, non-interactive runs,
and unsupported agents -- move into an AccordionGroup, so the page reads
as an intro, two steps and a verification section. Nothing is deleted:
all five code blocks are byte-identical and the collapsed text is still
in the page. The one prose deletion is "The fastest way to add Phoenix
tracing to your application.", which restated the frontmatter description
rendering directly above it.

Trade-off worth noting in review: collapsed sections leave the "On this
page" table of contents, which now lists only "Confirm traces are
flowing".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: hold every Get Started intro to four sentences or fewer

The intros restated themselves. Evaluate your agent opened with seven
sentences making three points: what an eval is \(said three times\), what
the guide does \(said twice\), and which metrics you can score against.

Deletions only, no rewriting. Every surviving sentence is verbatim.

Evaluate your agent, 7 -> 4. Dropped:
- "Evaluations attach quality signals to runs so that correctness or
  relevance can be reasoned about consistently instead of judged case by
  case." Restates the opening sentence and the traces/evals contrast that
  follows it, and states it as a negative.
- "We'll start with data that already exists in Phoenix, define a simple
  evaluation, and run it so we can see results directly in the UI."
  Restates "In this guide, you'll set up evaluations ... and run them on
  existing trace data".
- "The goal is to move from "I have model outputs" to "I can measure
  quality in a repeatable way."" Restates the opening sentence.

Iterate on Your Prompts, 5 -> 3. Dropped "Up to this point, we've traced
our agent runs and evaluated their outputs. Now we'll focus on prompts by
grouping failures into a dataset and iterating on prompt variants in the
Prompt Playground." -- the sentence above it already says the guide
groups failing runs into a dataset and iterates in the Playground, and
the prereq list directly below states the same continuity as two bullets.

Instrument your agent, TypeScript tab 5 -> 4. Dropped "In this guide,
we'll get tracing set up and walk through how to instrument an
application.", which the sentence after it says with the specifics. The
Python tab already carried this as one sentence.

Counts are per reader, shared intro plus their language tab, excluding
callouts: Instrument 4, Evaluate 4, Iterate 3, Optimize 3,
Agent-Assisted Setup 1.

Code blocks unchanged on all three pages.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say "agent" instead of "app" across Get Started

Refers to the reader's system as an agent throughout, matching the
section titles \("Instrument your agent", "Evaluate your agent"\). 51 lines
across the five guides plus the Overview page. Code blocks and URLs are
untouched; the `js/examples/apps/mastra-quickstart` paths and every
identifier keep their spelling.

The experiments guide's title becomes "Optimize Your Agent with
Experiments", so the nav label changes with it.

Two phrases needed care rather than a plain swap:

- "an agent application" would have become "an agent agent"; it is now
  just "an agent".
- "the agents that make up our application" would have become "the agents
  that make up our agent"; it is now "our agent system", the phrasing the
  experiments guide already uses.

Agent-Assisted Setup keeps "app" for the reader's codebase. That page is
about handing work to a *coding* agent, so "let your coding agent add
tracing to your agent" reads as one thing instrumenting itself. The
collision is worse than the inconsistency.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: merge the prompts and experiments guides into Improve your agent

"Iterate on Your Prompts" and "Optimize Your Agent with Experiments" were
two halves of one workflow split by *how* you make the change rather than
by what you are doing. Both started from eval-labelled failing traces,
both ended in the same Experiments view, and each pointed at the other as
the next step, so a reader could not tell which came first.

They are now one guide, matching the AX quickstart, which has a single
"Improve your agent" page with method tabs rather than a group.

Four steps, with the method choice appearing once instead of at every
stage:

1. Create a dataset from failed traces      \(shared\)
2. Use explanations to identify improvements \(shared; was taught twice\)
3. Make the change and run it   -- tabs: In the Prompt Playground /
   In code, with Python and TypeScript nested inside each
4. Compare versions in the Experiments view  \(shared narrative\)

Nothing is dropped. Verified against both source pages: 13 of 13 code
blocks, 12 of 12 videos, 12 of 12 frame captions, every numbered
instruction, and every H3 section.

Deliberate changes beyond the merge:

- The two cross-links between the guides are gone; they would now point
  at this page. That circularity was the reason to merge.
- "the first from Step 3" is now "the first from your baseline run",
  because the step it referenced no longer has that number.
- "A dataset created from previous runs" leaves the prerequisites, since
  step 1 builds it.
- The two "Congratulations" lines and the two "Learn More" sections are
  one each.
- About eight new connective sentences were written, the only prose on
  this page that is not carried over verbatim. Interleaving two
  workflows needs them.

Seven URLs now redirect here: both retired page paths, both ts- paths,
and the three legacy quickstart paths.

docs/phoenix.mdx had a card for each guide in both its Python and
TypeScript groups; each pair is now one "Improve Your Agent" card.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rewrite Get Started in second person, direct voice

The guides narrated themselves in the first person plural -- "we'll
install", "now that we have trace data, we can", "at this point we've
defined" -- which reads as tutorial filler and contradicts the house rule
that guides use active voice and second person. 155 instances of that
register across the section, now 0:

  royal "our"                   55 -> 0
  royal "we'll" / "we're"       44 -> 0
  "we can"                      26 -> 0
  "At this point,"               7 -> 0
  "Next, we..."                  6 -> 0
  "In this step, we..."          6 -> 0
  "Now that ..., we"             4 -> 0
  "let's"                        5 -> 0
  "More importantly,"            2 -> 0

Instructions are now imperative and the prose states facts about the
system rather than narrating a shared journey. "Next, we'll register a
tracer provider linked to a project in Phoenix. This project is where
your traces will show up" becomes "Register a tracer provider linked to a
project in Phoenix. That project is where your traces show up."

Two errors fixed on the way through:

- "seen how an agent runs shows up from start to finish" -> "how an agent
  run shows up"
- "our finanical writer agent" -> "The financial writer agent"

No instruction was dropped. Code blocks, videos, link destinations, step
titles and H3 sections are identical on all three guides: 17/2/13/9/14 on
tracing, 14/2/9/9/5 on evaluations, 13/12/5/4/9 on Improve your agent.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: sharpen the tracing opener and drop every em dash

The tracing guide opened with a sentence that did not parse: "A trace is
a record of a single run of your agent, broken down into spans that show
what happened at each step \(how agents, tasks, and tools executed\) and
provides the raw data needed for everything that follows." After the
participial phrase, "and provides" cannot reach back to "is a record", so
it reads as attaching to "spans", which would need "provide". It also
said the same thing twice \("what happened at each step" and "how agents,
tasks, and tools executed"\), and closed on "the raw data needed for
everything that follows", which was left over from the series framing
that now lives only on the Overview page.

Replaced with the house formulation from ax/instrument/what-are-traces
and phoenix/tracing/concepts-tracing, which also defines a span instead
of gesturing at it:

  A trace captures one end-to-end run of your agent as a tree of spans.
  Each span is a single step \(an agent, a task, a tool call, or an LLM
  call\) with its input, output, and timing.

The page also picks up a frontmatter description, matching the AX tracing
guide. It renders as the subtitle under the H1, so it carries the framing
while the body carries the mechanic, and the two do not restate each
other.

Em and en dashes are now zero across the section. Two were mine; two were
pre-existing on Agent-Assisted Setup, where a dash joined two independent
clauses that are clearer as separate sentences.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: stop repeating the prerequisites, and drop the long reference from Get Started

The evaluations guide stated its prerequisites twice, about fifteen lines
apart, in different words. At the top: "you'll need to have completed Get
Started with Tracing so you should have: Financial Analysis and Research
Chatbot / Trace Data in Phoenix". Then again inside step 1: "Evaluations
in Phoenix run over existing trace data. If you followed the tracing
guide, you should already have: A project in Phoenix / Traces containing
LLM inputs and outputs". Both tabs carried it, so a reader met the same
list twice per pass.

The step's version was the more precise one, so its detail is folded into
the single list at the top, which now reads "A project in Phoenix holding
traces with LLM inputs and outputs". Step 1 keeps the one fact it added,
that Phoenix evaluations run over existing trace data, and loses the
duplicate list.

Worth noting for future passes: string-similarity scanning did not catch
this, because the two lists share almost no wording. Semantic repetition
needs a read, not a regex.

The Overview page repeated itself inside single paragraphs the same way:
"using the same inputs and evaluation criteria" in two consecutive
sentences, and "spans that show what happened at each step" followed by
"Traces show how agents, tasks, and tools executed during a run". Both
are now stated once.

Coding Agents leaves the Get Started nav. It is 466 lines, 2258 words and
27 code blocks of reference material for Claude Code, Codex, the CLI, MCP
and skills, and its own opening line says "If you haven't instrumented
your app yet, start with Agent-Assisted Setup" -- it is the step after
getting started, not part of it. The page is unchanged and still listed
under Integrations > Developer Tools, and Agent-Assisted Setup already
links to it for ongoing workflows.

That leaves one page in the group, so it collapses to a single nav entry.
Agent-Assisted Setup gains sidebarTitle "Coding agent setup" so the label
still matches the agreed structure, the same way pxi.mdx shows as "PXI
Agent".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix five accuracy defects found by checking Get Started against the source

Verified the guides against the Phoenix source in this repo rather than
against other docs. Five real defects, all inherited from main:

1. `evals\['completeness'\].label == 'incomplete'` is a **span** filter. The
   guide said "Filter your traces by the incomplete evaluation label",
   which points at the traces table, where the expression is rejected:
   src/phoenix/trace/dsl/trace_filter.py refuses it with "`evals\[...\]` is
   not available in the trace filter". docs/.../filter-expressions.mdx
   lists it under span filters, and the UI has separate SpanFiltersContext
   and TraceFiltersContext. Now reads "Filter the spans table".

2. and 3. The add-to-dataset flow is span-based, not trace-based:
   AddSpansToDatasetInput carries `span_ids`
   \(src/phoenix/server/api/mutations/dataset_mutations.py:225\), and the
   dataset guide says "select the span". "Select the traces you want to
   include" and "Add the selected traces" now say spans.

4. `client = Client\(\)` ran with no import on this page. The evaluations
   guide imports it, but this page has to stand alone, so it now carries
   `from phoenix.client import Client`.

5. `import { getDataset } from "@arizeai/phoenix-client/datasets"` was
   never used. The companion example the guide points at,
   js/examples/apps/mastra-quickstart/src/mastra/experiments/experiment.ts,
   imports the identical set without it. Removed.

Everything else checked out against source: register\(project_name,
auto_instrument\), LLM\(provider, model\), ClassificationEvaluator\(name, llm,
prompt_template, choices\) and .bind\(\), Client\(\).datasets.get_dataset\(dataset=\),
Client\(\).experiments.run_experiment\(dataset=, task=, evaluators=\),
suppress_tracing's import path, the phoenix-client and phoenix-evals TS
exports, runExperiment accepting a {datasetName} selector, ports 6006 and
4317, exit code 6 as NOT_VERIFIED, every `px setup` subcommand and all
nine documented flags, and both companion code paths.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: one language choice on Improve your agent, not ten

The page had accumulated 10 separate Python | TypeScript pickers down its
length: one on the companion-code callout, one per video pair, and one
inside each half of the method tabs. Mintlify syncs tabs that share
labels, so picking a language once did switch all of them, but rendering
the control ten times reads as ten decisions.

Language is now a single choice at the top, the same shape the tracing
and evaluations guides use. Method tabs \(In the Prompt Playground /
In code\) stay where they were, inside step 3 and step 4. A reader now
sees one language picker and two method pickers instead of twelve
controls.

This is purely structural: every Python | TypeScript group was resolved
to the chosen language and the whole body wrapped in one pair. No prose
was rewritten. Verified against the previous version: all 13 code blocks,
12 videos, 12 frame captions, every numbered instruction, every heading
and every step title still present.

The cost is 78 lines. Language as the outer axis means the
language-agnostic narrative -- the dataset steps, the explanation step,
the shared prose in steps 3 and 4 -- exists once per branch. That is why
the step count in the file doubles from 4 to 8 while a reader still sees
4. Worth it for one obvious choice at the top; the alternative was
keeping the page shorter and the navigation confusing.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: method first, language inside it, on Improve your agent

Corrects the tab hierarchy to match ax/get-started/get-started-tracing,
where the outer tabs are methods \(By npx evals / By Arize Skills / By
Code\) and the Python | TypeScript picker sits inside By Code. The previous
commit put language on the outside, which was the wrong axis.

A reader now picks the method first \(In the Prompt Playground / In code\),
then the language within it. Two controls, in that order.

Also tightened the prose, since with tabs every shared line exists in more
than one branch and each cut lands more than once. Deleted as restatements
of the line beside them:

- "This dataset now represents a concrete failure case for your agent."
- "Start by grouping together the traces that didn't perform well."
- "Start from a real prompt the agent actually used." -- the next sentence
  already gives "a real starting point".
- "Saving prompt variants makes it easy to track changes..."
- "An experiment reruns the same inputs through different versions..." --
  the page intro already defines an experiment; this was the third time.
- "With two versions in hand, compare them..." -- restated the step title.

Shortened: the two-paths lead-in \(the tab labels say it\), the
create-datasets aside \(said "in code" twice\), and the task and evaluation
definitions. Fixed two royal "we" the voice pass missed: "the dataset we
made earlier" and "we should see more traces".

Line count: 605 -> 581 from the trims, then -> 694 from the inversion.
The growth is real and worth naming. Steps 1 and 2 -- create the dataset,
read the explanations -- are method-agnostic, so method-outer copies them
into both method branches, and language-inner copies them again. Four
copies of about thirty lines. A reader still sees each step once.

The wrinkle that buys: the method choice now sits above two steps that do
not depend on it, so a reader picks Playground or code before creating the
dataset. The alternative keeps steps 1 and 2 shared and moves the method
choice down to step 3, at the cost of a second visible language picker.

Verified against the previous version: every code block, video, caption,
link, heading and step title still present.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: move the companion-code callout out of the no-code path

The Prompt Playground branch offered "a companion notebook with runnable
code examples" in a path where you write no code. That was a side effect
of wrapping the whole body in the method tabs. The callout now appears
only under In code, for both languages.

The Python | TypeScript picker stays inside the Prompt Playground branch,
because that path does depend on which language you followed -- not for
the language itself, but for what you built with it. Six of the videos
record the CrewAI app rather than the Mastra app, and the dataset input
syntax differs with the agent: `{tickers}` and `{focus}` are CrewAI task
variables, `{{input}}` is Mastra. The label is a proxy for "which
quickstart did you follow".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: drop the horizontal rules bracketing the companion-code callouts

Every companion-code callout in Get Started was wrapped in `---` rules,
inherited from main. Each renders as a thematic break with generous
margins, so the callout sat in a band of empty space with a thin line
above and below it. The callout already has its own bordered container,
so the rules added separation that was not needed.

12 rules removed: four on the tracing guide, four on evaluations, four on
Improve your agent. Frontmatter delimiters are untouched, including the
multi-line description block on the tracing guide, whose closing `---`
sits at line 6 and is easy to mistake for one of these.

No prose changed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the playground template syntax, which would not have interpolated

The Python path told readers to set the User prompt to `{tickers}` and
`{focus}`. That is F_STRING syntax, and the Prompt Playground defaults to
Mustache:

  js/app/src/components/templateEditor/constants.ts
    FString:  "F_STRING",  // {variable}
    Mustache: "MUSTACHE",  // {{variable}}

  js/app/src/store/playground/playgroundStore.tsx:273
    templateFormat: TemplateFormats.Mustache,

Playground.tsx reads the format from that store, so with default settings
single braces are not substituted -- the prompt would have carried the
literal text `{tickers}`. The guide never mentioned the format selector,
so there was no way for a reader to know.

Most likely this was copied from the CrewAI task template on the tracing
guide, `Task\(description="Research: {tickers}"\)`, which legitimately uses
single braces. That is CrewAI's own templating, not Phoenix's, and it is
left alone.

Both paths now use Mustache, and the shared lead-in says so, so the two
language tabs differ only in the variable names.

The variable names themselves were already correct and are framework
driven, traceable in the guides' own code: the Python experiment passes
`example.input` straight into `kickoff\(inputs=...\)`, so dataset examples
carry `tickers` and `focus` keys, while the TypeScript task types
`example.input` as a `{role, content}\[\]` message array under a single
`input` key.

Verified rendered: the block shows `Research: {{tickers}}` literally, and
the format note appears in both language tabs.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: expand the Agent-Assisted Setup accordions back into headings

I collapsed three sections into an AccordionGroup when the page read as
too long. That hid 307 of its 542 words, 57% of the page, and dropped the
three section names from the "On this page" table of contents, leaving one
entry. It also made the page look like content had been deleted, which is
what prompted the question.

The reason for collapsing them has gone: the length concern was mostly the
466-line Coding Agents reference, which no longer sits in this nav group.
What remains is ~330 words, the shortest page in Get Started, so hiding
more than half of it costs more than it saves.

Re-run a single step, Run non-interactively \(CI or agents\), and Use an
unsupported agent are `##` headings again, so all four sections appear in
the table of contents and can be linked to directly.

Code blocks and links are identical.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: no language picker in the Prompt Playground path

You write no code in the Prompt Playground, so asking the reader to pick
Python or TypeScript there was misleading. The picker now appears only
under In code, where it selects actual code, which matches
ax/get-started/get-started-tracing.

Two controls on the page, in this order:

  In the Prompt Playground | In code
    In code -> Python | TypeScript

What the picker had been carrying in that path, and where it went:

- Steps 1, 2 and 4 were already identical between the languages.
- The dataset input keys are real, so they are stated inline: which keys
  you have depends on the agent you traced, `{{tickers}}` and `{{focus}}`
  from the Python guide, `{{input}}` from TypeScript. Both forms are now
  written out rather than left to be inferred.
- The example prompt line keeps the Python variant, which is the one that
  matches the finding step 2 states \("the report lacked financial
  ratios"\). The TypeScript variant said something different, about
  per-ticker metrics.
- Five videos drop out: ts-first-dataset, ts-first-prompt,
  ts-playground-experiment, ts-compare-prompts, ts-view-experiment. They
  carried the same captions and demonstrated the same click-path as the
  five that remain, recorded against Mastra instead of CrewAI.

All 13 code blocks and every link destination are unchanged. 668 -> 538
lines.

Braces checked: every `{{...}}` on the page is either inside a fenced
block or an inline code span, so MDX treats them as text. Verified in the
rendered DOM.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: restructure Get Started nav and verify evals against the product

Reshape the Get Started nav to Observe Your Agent / Evaluate Your Agent /
PXI / Arize Phoenix for Agents / Phoenix Demo Account. Per-language pages
become tabs within each section, with language nested inside the code path.

Merge the Improve Your Agent guide into Evaluate Your Agent, which is now
written as offline evals: a dataset feeds a task, an evaluator scores each
output. Phoenix has not released continuous scoring of live production
traffic, so the previous framing was wrong.

Extract Skills from the Coding Agents page into its own page and group the
agent-facing docs \(agent-assisted setup, skills, MCP\) under Arize Phoenix
for Agents. A pointer heading keeps existing #skills anchors resolving.

Correct six claims against Phoenix 20.15.0 running locally:

- `output` is the task's output, not an example payload. `reference` is the
  example's expected output \(experiment_runner.py:831, message_helpers.py:279,
  and the compare view's "reference output" column\).
- Creating a dataset from spans is one action, not two. You select spans and
  click Add to Dataset; creating a new dataset happens inside that popover
  \(SpanSelectionToolbar.tsx:212, DatasetSelectorPopoverContent.tsx:104\).
- The evaluator dialog's submit button is Create, not Save.
- The playground control is "Test over a dataset".
- Experiment runs live on the Experiments tab, compared via the
  "comparisons" selector.
- Playground template format defaults to Mustache, so dataset input keys
  need double braces.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: remove em dashes from the Get Started nav pages

All 98 em dashes on pages now reachable from Get Started, none of them
authored by this branch: pxi.mdx \(31\), coding-agents.mdx \(22\), remote-mcp.mdx
\(14\), mcp.mdx \(4\), docs-mcp.mdx \(3\), phoenix-mcp-server.mdx \(1\), plus 14
bullet and label dashes replaced with colons.

Each was repunctuated in context rather than swapped for a single character:
a colon where a list or restatement follows, a comma for an aside, a period
where an independent clause follows, parentheses for the three parentheticals
that span hard-wrapped lines. No dash inside a fenced code block was touched.

Diff is 89 insertions and 89 deletions, so no prose was added or removed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: point Evaluate Your Agent at the trace-scoring flow

The merged page covers offline evals over experiments. Readers who want to
score traces they have already collected had no path to it from Get Started,
since that content now lives under Tracing and Evaluation. Links to
Evaluating Phoenix Traces and the two eval quickstarts.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: simplify Phoenix get started quickstarts

* docs: keep Agent, not App, in the Get Started flow

Restores "Observe Your Agent" and "Evaluate Your Agent" and aligns the
surrounding copy, which had moved to "app": both page titles and descriptions,
the section headings on the Get Started overview, the landing-page cards, and
two "Observe Your App" cross-links that would have pointed at a page with a
different name. `app.ts` stays, since that is a file the reader creates.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: one Quick Starts card group, without the Improve card

The Improve page merged into Evaluate Your Agent, so its card pointed at the
same page as the Evaluate card. Dropped it and folded its description into
Evaluate's, which had described only the LLM-as-a-judge half.

The Python and TypeScript groups held identical cards, and both target pages
now select language with tabs, so the split no longer carried information.
Collapsed to a single group. The redirect from the retired improve path is
untouched.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make Evaluate Your Agent a single Playground path

Drops the Prompt Playground / Code tab split and keeps the Playground steps,
so the quickstart teaches one path end to end. 401 lines to 146.

Adds a "Prefer to do this in code?" section that sends readers to Run
Experiments for the Python SDK and to the TypeScript experiments reference for
runExperiment, so both languages the removed tab covered still have a home.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say which traces to evaluate and what to measure

The steps described where to click without answering the two questions a
reader arrives with. Five tweaks, 146 lines to 151:

- Dataset step gives a size to aim for and two ways to find failing traces,
  a filter expression or asking PXI to walk them
- Evaluator step names error patterns worth scoring instead of only listing
  the two evaluator types
- Saving a prompt from a trace opens with why that prompt is the one to use
- The baseline is stated as the thing later edits are measured against
- Comparison step closes with how to read the delta, including what to do
  when the score drops

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say offline evaluators in the Evaluate description

Also removes two lines the single-path change left stale: an opening sentence
that restated the description, and a paragraph telling readers to choose
between the Playground and code paths, which are no longer tabs on this page.
The offline framing replaces it, with the caveat that continuous scoring of
live production traffic has not shipped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say the manual path builds everything from the Phoenix UI

Now that this page is Playground-only, the section intro should name the
surface the steps use.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: shorten the PXI line in the dataset step

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: restore the code experiment example as its own page

Making Evaluate Your Agent a single Playground path deleted Nancy's Python
and TypeScript worked examples, and the pages it linked to instead cover the
same APIs without that scenario. Recovered them verbatim from 966510c74 as
Evaluate a Prompt Change in Code, under How-to: Experiments.

All 7 code blocks and both 4-step sequences are byte-identical to the removed
tab; only the Tab wrapper was dropped and the block de-indented one level. The
page opens by tying itself to the quickstart, so the support agent scenario
carries across the UI and code paths again.

"Prefer to do this in code?" now points here first, with Run Experiments as
the wider reference.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the new experiment page headings its siblings have

The page had no H2s, so it contributed nothing to the table of contents while
every other page in How-to: Experiments has several. Adds a heading over the
tabbed walkthrough, notes that the script creates its own dataset so it runs
without traces, and points at Set a Baseline Experiment, which covers the
persistent baseline that this page's in-code comparison does not.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rework Observe and Evaluate, split PXI into Get Started and Learn More

Observe Your Agent: intro rewritten to say what the page does and where the
concepts live, `\(Recommended\)` on the agent-assisted path, and the manual
steps reordered to start Phoenix, get your keys, then set up tracing. The
install step folded into setup, and the three steps that were identical in
both language tabs hoisted above them.

Evaluate Your Agent: no longer depends on having traces. The dataset step
hands you three travel support examples to upload, so the page runs on a
fresh Phoenix. Traces became an optional route rather than the only one.
Switched to an LLM judge, added copyable Playground prompts for the baseline
and the edit, and dropped the save-a-prompt-from-a-trace step.

PXI: 3,315 words split into a 315-word Get Started \(what it is, what it does,
three steps\) and a 1,158-word Learn More grouped as capabilities, surfaces,
configuration, and privacy. Nothing deleted except the architecture diagram
and the flag and slash-command tables, which duplicate the CLI reference.
Accordions flattened so nothing is hidden behind a click.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: consolidate Arize Phoenix for Agents

Snapshot of the agents/MCP consolidation: agent-assisted-setup folded into a
new developer-tools/cli page, and remote-mcp, docs-mcp, and phoenix-mcp-server
collapsed into one integrations/mcp page, with redirects for all four retired
paths. Nav and inbound references updated across the CLI reference, release
notes, self-hosting, and the shared onboarding snippets.

Committed from the docs-get-started-reorg worktree so the two in-flight
branches can be merged.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: point the tracing quickstart at the new CLI page

The agents consolidation folded agent-assisted-setup into
developer-tools/cli. This was the last reference still relying on the
redirect rather than the real page.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: link the uploaded dataset, and cross-link the other Observe methods

The travel-support.csv download pointed at assets/datasets/, but the file was
uploaded to assets/images/arize-docs-images/evaluate/. Repointed to where it
actually lives, so the Evaluate walkthrough runs end to end. Verified the URL
serves 3 rows with the question and expected_answer columns the page maps.

Also adds links out of both quickstarts for the routes they do not teach,
prompted by the Phoenix workflow diagram: reading traces in code, from a
coding agent via the CLI or MCP, by asking PXI, and across Sessions; plus PXI
for curating datasets and running experiments. Links only, no new pages. The
Evaluate step stays absent on purpose, since online evals have not shipped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make both Evaluate paths use one dataset, and close three story gaps

Found by running every path in Get Started end to end as a new user.

The agent-assisted prompt inlined three examples, but only one of them was in
the CSV the manual path downloads. Cutting that file from eight rows to three
had left the prompt behind. The prompt now downloads the same CSV, so there is
one dataset and the two paths cannot drift again. It also restores the page's
own argument, which turns on the visa and refund rows.

Three gaps between the paths:

- The Recommended path ended when px setup finished and never said to open the
  Traces view. Only the manual path told you to look at what you captured.
- "Ask your coding agent" said to start Phoenix without giving the command,
  which only appeared in the other tab.
- Observe handed off promising to "convert selected traces into a dataset",
  which Evaluate no longer does, since it starts from a downloadable dataset.

Verified by execution, not reading: both language paths install and run and
produce correct OpenInference traces \(a support_reply CHAIN wrapping a
ChatCompletion LLM span with messages, model, and token counts\), px setup
connects and writes .env.phoenix, both skills commands install, and the CSV
builds the same 3-example dataset from either path.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: state the Python 3.10 floor and name the Traces tab in Observe

Both quickstart paths were run end to end against Phoenix 20.16.0. On
macOS's default Python 3.9, pip quietly installs arize-phoenix-otel
0.14.0 instead of 0.17.1, because the current packages require 3.10+.
A project also opens on the Spans tab, so "the Traces view" sent
readers looking for a view that isn't the landing page.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>

* docs: make the Evaluate comparison actually move the score

The page claimed the baseline prompt invents visa rules and agrees to refunds
it should decline. Measured against gpt-5-mini it does neither: the baseline
scored 1.0, the edited prompt scored 1.0, and stripping the baseline to
"Answer the customer's question" still scored 1.0. Step 4 promised a rise that
never came, so the lesson did not land.

Fixed by changing what the examples test. They now ask about a fictional
airline's Basic fare policy, with values deliberately unlike industry norms: a
48 hour refund window, a 40 dollar change fee, one included checked bag. The
model cannot guess any of them, so the baseline answers from industry defaults
and gets all three wrong. The edit supplies the policy.

Measured: baseline 0.00, edited 1.00.

This also makes the lesson durable. The baseline fails for lack of information
rather than lack of capability, so a better model will not quietly erase the
gap the way it did with the previous examples.

Also drops the Observe handoff sentence, per review.

The dataset CSV needs re-uploading to the same path; the agent-assisted prompt
reads it by URL and needs no edit.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the PXI paths, found by running them

Tested both PXI pages against a live Phoenix for the first time.

The terminal client command did not work. `npx -y @arizeai/phoenix-cli pxi`
exits with "unknown command 'pxi'", because pxi is a separate bin in the
package rather than a px subcommand, so npx ran the default bin and passed
pxi as an argument. Needs `-p`. Verified the corrected form launches and
reaches the credential check.

PXI defaults to an Anthropic model, so a reader who follows step 1 and sets
only OPENAI_API_KEY opens the assistant and hits "Anthropic credentials are
not configured". The step now says to pick a model matching the provider you
configured. Confirmed by switching to gpt-5.5 and getting a correct answer
about project span counts.

Restores "Where your data goes" under Privacy and tracing. It was dropped with
the architecture section, but it is not architecture: it is the answer to
whether data leaves your instance. No separate Arize service, your provider
called with your key, Arize not in the request path, and remote export gated
three ways. That is the first thing a security review asks about a beta AI
feature.

Also: the provider list mixed three env var names in code with "AWS Bedrock"
in prose, so it now names providers throughout; "Drives the product" lost its
object when I trimmed it; and the roadmap milestone link is out, since a
GitHub milestone goes stale.

Verified live: the consent gate appears as described, trace saving shows
"Disabled by system settings" which matches the documented default, the model
list matches the validated set, and PXI answered using search, get_schema, and
execute, the code-mode surface the page describes.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the CLI page its own job, running Phoenix from the terminal

The page claimed px was "the recommended way for a coding agent to work with
Phoenix", then documented only px setup, which Observe already owns. So it
repeated onboarding and never showed an agent working with Phoenix. Meanwhile
px has 17 top-level commands and the page covered one.

Rewritten around the debugging loop the phoenix-cli skill actually drives:
find what broke, read one trace in full, record the judgement as an
annotation, inspect the datasets and experiments you test against, then work
on the prompt. Plus a table for the remaining commands. Setup is now a single
line pointing at Observe.

Every command was run against a live Phoenix, which caught three errors in my
own first draft:

- `px span list` has no `--filter`. It takes typed flags, so the examples use
  `--status-code ERROR` and `--span-kind LLM`.
- `px experiment list` requires `--dataset`.
- `px trace get` resolves the default project and reports "Trace not found"
  without `--project`.

Verified working: trace list, trace get, trace add-note, span list with both
filters, span annotate \(annotation upserted\), dataset list, dataset get,
experiment list, prompt list, project list, and all 11 commands in the table.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: lead the agents overview with the setup table

Move "Set up your agent" above the three-piece cards so the
per-agent map comes first, and make its lead-in name the three
pieces so it stands on its own.

Narrow the endpoint rule: the plugins and the px CLI take the base
endpoint and append /mcp themselves, while `claude mcp add` and the
per-client MCP steps take the full /mcp URL.

Reword the prompt samples as examples rather than the full set.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the agents overview one job, shorten the MCP page

The overview page was mostly navigation: three cards pointing at CLI,
MCP Servers, and Skills, which already sit next to it in the sidebar,
plus a setup table routing to the same three pages. The sidebar label
"Overview" also repeated the group name, so the page read as a section
divider.

Now it does one thing, connect a coding agent to Phoenix: what this is,
the disambiguation note, the per-agent setup table, the endpoint. The
three cards are gone, the prompt samples are cut from five to two, and
the node_modules section is dropped since the TypeScript SDK overview
already documents it. Relabel the sidebar entry "Set Up Your Agent".

Correct the Claude Code row: the plugin ships three skills, while
px setup skills installs five, so the plugin path needs px setup skills
too. Verified by listing the plugin and running both.

On the MCP page, lead Connect with px setup mcp, merge the Cursor,
VS Code, and OpenCode accordions \(their JSON matches what px writes\),
replace the six Docs MCP accordions with a client table, and merge the
identical npm stdio configs. 633 to 521 lines with no facts dropped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: move the plugins onto the setup page

The setup page was still an index: its table pointed at Install
sections that CLI, MCP Servers, and Skills each already have. What it
lacked was the thing that belongs to no single one of them, the
one-install-per-agent path.

That path was the Plugins section, 137 lines and the largest section on
the MCP page, which is the wrong home for a plugin that registers MCP
and skills together. Move it here, after the endpoint it references,
and leave a pointer on the MCP page.

MCP drops to 388 lines and covers only MCP servers. The setup page now
routes by agent, states the shared endpoint, then gives the full plugin
steps.

Also repoint skills.mdx at the plugins section's new home, and fix its
link to a CLI heading that the CLI rewrite removed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the MCP page opener

It said "two things you might want your assistant to do" and "run
both" above a three-row table. Name the two servers that matter, your
data and the documentation, and say what the third row is.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: cut duplicated prose from the MCP page

The Configuration table repeated two env vars that self-hosting
configuration already documents; point there and keep only
PHOENIX_SKILLS_PATHS, which is unique to this page.

Monty sandbox limits were spelled out twice, in How it works and again
in Security; state them once. Merge the code-mode Warning and Note,
which both said the same thing. Trim the OAuth paragraph. Move the
OAuth2 authorization-server paragraph out of "Mounting your own
skills", where it was misfiled, into Security. Cut the npm sample
questions from seven to three.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: clarify the direction of the agents note

"Operate on Phoenix" is jargon, and "trace your sessions with a coding
agent" reads either way. Say which direction each page covers: this one
connects the agent to Phoenix, the other has Phoenix record the agent.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: correct two plugin commands that fail as written

Both found by running them. claude plugin uninstall acts on user scope
by default, so it stops with a scope error on a plugin installed at
local or project scope; say to pass the scope, and add disable, which
has the same rule. claude plugin details reports a disabled plugin as
not found, so say it has to be enabled, and say what the command lists.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: correct two CLI table rows that overstated what the commands do

Found by running them. px api has one subcommand, graphql, and rejects
mutations, so "authenticated requests to any Phoenix API route" is
wrong. The three annotation commands take only delete, and require
--all or a time window, so "stored annotations" hid that they are
destructive.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make the CLI debugging loop match what px can do

Steps 4 and 5 were titled "Turn failures into a test set" and "Work on
the prompt", but every command under them only reads, and the bodies
said "Inspect", contradicting the titles. px has no way to create a
dataset, add examples, or edit a prompt: for datasets, prompts, and
experiments it offers list, get, and delete only.

Cut the loop to the three steps px actually supports, ending at the
annotation, which with notes is the only thing it writes. Move the
dataset, experiment, and prompt reads into their own section framed as
looking up context, and say plainly that authoring happens in the UI
or the SDK.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rewrite the CLI page as the workflow an agent runs

The page had a "debugging loop" that was not a loop: it ended at an
annotation, and px cannot fix anything. Reorganising it by capability
then made it a feature list. Put it back in order as the pass an agent
actually runs, and let step 4 say plainly that px stops there and the
fix goes in your code, which is what closes the loop.

The examples were the weaker half. They repeated a fake project four
times, used placeholder ids that cannot be run, and carried comments
that restated the command instead of the output. Use real ids, say
which command feeds the next, and split the dataset, experiment, and
prompt reads into their own section, since those are for checking a
change rather than part of the debugging pass.

Add three facts found by running the commands: reads accept --format
json, annotate and add-note want the OpenTelemetry id rather than the
opaque one, and --identifier makes a repeat call overwrite.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: note that px annotation deletes are gated off by default

px span-annotations delete answers "Delete commands are disabled."
before it looks at any flag. Name the variable that opens the gate,
PHOENIX_CLI_DANGEROUSLY_ENABLE_DELETES, so the row does not send a
reader at a command that refuses to run.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say who runs px, and that the docs MCP URL is not a page

The CLI page opened by saying px is how a coding agent works with
Phoenix, then showed a reader commands to type, and titled the section
"How an agent uses it". A reader could not tell whether the commands
were theirs to run. Say it is an ordinary CLI you run yourself, and
that needing only a shell is what makes it easy to hand to an agent.

The Phoenix Docs MCP URL sat in a bare code block, so it read as a link
to open. A browser shows the tool manifest as JSON, which looks like an
error and is not. Say it is an endpoint for a client.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: answer px versus MCP, and add agent instructions to both pages

Neither page said when to reach for which. That choice only existed as
cards on the setup page, which is not where someone deciding ends up.
The CLI page now compares the two on what the client must support, what
setup costs, and how much surface each exposes, and the MCP page points
at it.

Both pages end with a section addressed to an agent reading them,
covering how to discover flags, which output format to parse, the
OpenTelemetry id the annotate commands want, the execute sandbox limits
and import allowlist, and that trace contents are untrusted data.

Every claim in both sections was run first: 116 catalog operations, the
50-call cap, json/re/datetime importable and socket/subprocess/importlib
not, exit status 0 and 1, and the delete gate.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rebuild the CLI page to a fixed outline, and align sibling names

The page now follows one order: what it is and why it matters, CLI or
MCP server, set up, what you can do with it, learn more, agent
instructions. Setup is three steps ending in a check. The examples
keep their real ids and drop the repeated --project in favour of
PHOENIX_PROJECT, set once in step two. The command table is gone; the
two caveats it carried, the delete gate and read-only GraphQL, live in
the agent instructions, and the full list is a link to the reference.

An audit of how each page in the section names its siblings found six
mismatches: the CLI page linked as "px CLI" and as bare "MCP", the
Remote MCP Server as "server", and a PXI section by a title it does
not have. Each link text now matches the page or section it points at.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: name the CLI setup check by what it proves

"Check it" said nothing. The step confirms the CLI reached the
instance, so say that, and say what success looks like.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say what the CLI setup is for before the steps

The section went straight from the heading to step one, so a reader
could not tell what was being set up or where. Name the CLI in the
heading, say to install it where the agent runs, and say what having
finished looks like.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say what px project list returns on a fresh instance, drop "has a terminal"

The setup check said a table of your projects means success, which
leaves a reader with no traces unsure what to expect. Tested on an
empty instance: every Phoenix has a project named default, so the
command always returns at least one row, and a wrong endpoint fails
with "Error fetching projects: fetch failed". Say both.

"When your agent has a terminal" was jargon on the CLI and MCP pages.
Say what it means: the agent can run shell commands, which every
coding agent can.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the CLI-first recommendation, say what the default project is

"Start with the CLI" was a preference with nothing behind it, and it
contradicted the setup page, which sends Claude Code and Codex users
to the plugin first, and the MCP page, which calls itself the primary
choice. The comparison table carries the facts; let the reader choose.
The MCP page mirror is now a plain pointer.

The setup check mentions the default project. Say what it is: created
by the initial migration, the destination for traces sent without a
project name, and empty on a fresh instance. Not a demo.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: make the CLI page work for a user with no traces yet

Run as a brand-new user, in a clean environment against an empty
instance, the previous page failed at step two: PHOENIX_PROJECT=my-app
404s on every read, and every example carried an id from one machine.
Stuff has to work before a project has traces.

Follow the shape of the live CLI section: install, endpoint, verify
with px --help and px project list, which both succeed with zero data,
then the common agent workflows as bullets and one example prompt, so
the reader sees what to ask rather than what the agent types. Say that
PHOENIX_PROJECT comes later, once traces are flowing. Keep the CLI or
MCP table and the agent instructions, and add the missing rule that
read commands need a project.

On the MCP page, restore the per-client tabs for the Docs MCP from the
live site, adding Claude Desktop.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: one story across Arize Phoenix for Agents, working from zero traces

The section was organised by plumbing, CLI, MCP, Skills, and every page
assumed traces already existed. A new user has none. Reframe around
what is true from the start: connect the agent, let it instrument your
app, then debug from what Phoenix records.

Set Up Your Agent owns routing, the endpoint, and the plugins. Its
connection check is a question to the agent, list my Phoenix projects,
which a fresh instance answers with default. Verified through both
pipes on an empty instance: px project list, and MCP search finding
getProjects and execute returning it. A new section points at the
agent-assisted tracing path as the next step.

CLI, MCP Servers, and Skills share one skeleton: what it is, install
ending in a check, what your agent can do with it, learn more,
reference. Deep material on the MCP page, API-key fallback, code mode
internals, server configuration, security, the npm server, sits under
Reference. The MCP Endpoint section folds into one line. The skills
page moves its options table and custom-skills guidance under
Reference and gains an example prompt.

Every shell command on the four pages was run in a clean environment
against an empty Phoenix. Restore the remote-mcp-server anchor, which
eleven pages link to, and repoint two onboarding snippets that still
named a CLI heading removed in an earlier rewrite.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut the instrumentation pointer back to one line

It was a sentence before, and a heading with three sentences after. The
link is the useful part.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: nest the MCP capabilities section under Remote MCP Server

The page covers two servers, so "what your agent can do with it" as a
top-level heading left "it" ambiguous. Make it an H3 under Remote MCP
Server, alongside a new Connect H3 for the setup steps.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut duplicated prose from the MCP page

The intro paragraph restated the table under it. The capabilities
section under Remote MCP Server repeated Reference, How it works,
nearly sentence for sentence; point there instead. Three sentences of
OAuth mechanism sat ahead of the first command; one line after the
accordions covers what a reader needs. The Docs MCP intro restated its
table row, the npm section said maintenance mode twice, and the
mounted-skills paragraph named its variable twice in a row.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: link the CLI page back to Set Up Your Agent

The MCP and Skills pages both point back to the setup page; the CLI
page did not, so a reader who landed there directly had no route to
the per-agent install.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: frame PXI skills as example capabilities

The table led with skill names, which is the mechanism. Lead with what
you can ask PXI to do, give one example ask per row, and keep the skill
concept as a single sentence after the table.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: frame PXI models as bring your own, and trim

Lead with the point: PXI runs on your provider and your key. Name the
providers once, keep the validated-model list and the tool-calling
caveat, drop the rest. Repoint the two links that used the old anchor.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the agent-instructions section from the CLI page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the agent-instructions section from the MCP page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the validated-model list from PXI

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: stop promising a supported-models list PXI no longer has

The list was removed; three references still pointed at it. Retitle the
Get Started link to the section that exists, and say model providers
where the Learn more line and the page description said supported
models.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: remove the npm MCP server from the MCP page

Drop the maintenance-mode section, its row in the intro table, and the
three places that offered it as a fallback: the version-floor line, the
Claude Desktop accordion, and the troubleshooting entry.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: act on the docs review: skills guidance, Docs MCP callout, MCP cut-down

Skills page: name the five skills written for Phoenix users, add the
plugin-versus-skills-add guidance from the review \(plugins track
Phoenix releases; skills add is a one-time copy, rerun it or run npx
skills update\), and take ownership of mounting your own skills from the
MCP page. Document --all honestly: tested in a fresh project it
installed 26 skills to 73 agents, including internal development
skills, so it is the wrong default.

MCP page: Phoenix Docs MCP becomes a short block near the top, as the
review asked, in place of an 87-line section. Fold Antigravity into the
other-clients accordion, trim How it works to the tool table plus two
sentences, and drop the mounting section now that Skills owns it.
361 to 271 lines.

Evaluate Your Agent: move the spans screenshot out of the Note it was
nested in.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: bring llms.txt in line with the Get Started and agents reorg

Add the CLI, Skills, and PXI Learn More pages, which had no entries.
Remove seven Quick Start entries for pages the reorg deleted or merged
into Observe Your Agent and Evaluate Your Agent, and three MCP entries
for pages folded into the single MCP page. Retitle the survivors to
match their pages.

Add the agent-usage guidance for the px CLI and the Remote MCP Server
here, where the review said it belongs, instead of on the human pages.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: give the CLI guide entry in llms.txt a unique title

It collided with the @arizeai/phoenix-cli reference entry.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten the endpoint rule on the setup page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: halve the agent-assisted setup on Observe Your Agent

Start Phoenix once above the tabs instead of in each. One sentence of
outcome per tab. Cut the pasted prompt to the two instructions that
matter, since the tracing skill carries the procedure. Drop the agent
list and the CLI pointer, which the setup page owns.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: add the PXI path to Evaluate Your Agent

A prompt to hand PXI once the dataset exists and the Playground has it
selected. Both preconditions come from running it: PXI cannot fetch a
URL, and its playground and evaluator skills act on the page you are on.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: restore the agent-assisted shape on Observe, trimmed

Back to the two tabs as they were, each self-contained, with the full
prompt. Merge the px setup outcome into one paragraph. Replace the
named --skill list with px setup skills, which installs exactly the
five Phoenix skills for every detected agent: tested via npx in a fresh
project with no CLI installed.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: repoint a PXI conversations link to the Learn More page

The Conversations section moved there when PXI split into Get Started
and Learn More; one link still pointed at the old page.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: Evaluate Your Agent builds a reference-aware evaluator

Built-in LLM templates have no reference field and score the baseline
higher than the fixed prompt, so step 2 now walks through Create new
LLM evaluator with a short rubric and the reference path mapping. PXI
section hands off after the dataset and evaluator exist, since it
cannot fetch the CSV and its own evaluator scores 0 at run time.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten Evaluate Your Agent evaluator step and PXI hand-off

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: PXI runs the full Evaluate flow from one prompt

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten Evaluate Your Agent prose

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut PXI Learn More to the essentials

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: one-line rows in the PXI capabilities table

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: shorten PXI privacy and tracing

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: compact PXI configuration table

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: shorten CLI page intro

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten MCP Servers prose

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop em dashes from Get Started snippets

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: add the follow-up prompt to the coding agent path

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: make px setup skills the primary install on the Skills page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say where the model API key goes before creating an LLM evaluator

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: the key must be on the Server tab for evaluators to use it

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say when to pick the CLI and when the MCP server

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: CLI vs MCP as best-when guidance

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop redundant line from CLI or MCP section

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: plugin rows say what the plugin covers and what is optional

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop Docs MCP aside from Set Up Your Agent

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

*…")[#16288](https://github.com/Arize-ai/phoenix/pull/16288)[)](https://github.com/Arize-ai/phoenix/commit/2afdf488e638f282f742c7daf796a09a56662971 "docs: reorganize Get Started around the agent workflow \(#16288\)

* docs: reorganize Get Started around the agent workflow

Restructure the Get Started section to mirror the Arize AX quickstart:
Instrument your agent, Evaluate your agent, Improve your agent, Coding
agent setup, PXI, and Phoenix Demo.

The Python and TypeScript quickstarts previously sat in separate nav
groups, so each language duplicated the whole section tree. Each pair is
now a single page with Python and TypeScript tabs, so the language is a
choice within a section rather than a parallel branch of the nav.

Every page body is carried over verbatim into its language tab. The only
text rewritten is internal links that pointed at the retired
ts-get-started-* paths, which now have redirects to the merged pages.

- Instrument your agent  <- get-started-tracing + ts- counterpart
- Evaluate your agent    <- get-started-evaluations + ts- counterpart
- Improve your agent     <- group over the prompt playground and
                            datasets/experiments guides
- Coding agent setup     <- group over Agent-Assisted Setup and Coding
                            Agents
- MCP group, PXI, and Phoenix Demo are unchanged aside from ordering
- End to End Features Notebook is removed from the nav; the page itself
  is left in place

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: cut duplicated prose from the Get Started guides

The previous commit put each language's full guide in its own tab, which
made every language-agnostic sentence render twice on the same page. On
the prompt playground guide that was 37% of the prose; on datasets and
experiments, 40%.

Where the two tabs told the same story, the narrative is hoisted above
the tabs and the tabs now wrap only what genuinely differs: code, videos,
the companion-code callout, and the two prompt snippets. The prompt
playground and experiments guides had identical step sequences, so their
steps are shared outright. Tracing and evaluations each have an extra
TypeScript step, so their steps stay tabbed and only the intro, prereqs
and closing sections are shared.

No prose was rewritten. Every line either survives verbatim from the
previous commit or is a prefix of one with a redundant trailing clause
removed, verified mechanically. Where the two tabs differed only
trivially, one existing sentence is kept verbatim rather than a new one
written; this also picks up several corrections the TypeScript copy
already had:

- "traces that received an incompleteness evaluation label" ->
  "an incomplete evaluation label", matching the filter beside it
- "Filter your traces by the incorrect evaluation label" -> "incomplete";
  the filter under it has always been evals\['completeness'\] == 'incomplete'
- "Using the explanations apart of the evals" -> "from the evals"
- "New application runs as a results of our task" -> "as a result"
- "ran your first experiment" / "ran an A/B test" -> "run"

Also drops, on the tracing guide, a sentence restating the bullet list
above it and three repeats of the supported-frameworks disclaimer that
the intro already carries.

Verified: all 44 code blocks byte-identical \(multiset\), every step title
and video retained, no duplicate sentence in any rendered page, both link
checkers clean, all four pages render with working tabs.

Words: 10790 -> 8789 \(18.5% shorter\).

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: tighten Instrument and Agent-Assisted Setup, repoint quickstart

Three changes to the Get Started section.

Instrument your agent opened with a paragraph of series framing --
"This guide walks through a complete workflow ... Each guide in this
series introduces one piece of that workflow" -- which is word for word
the Overview page's opening and says nothing about instrumentation. It is
removed here and still lives on the Overview page. The guide now opens on
its own subject: what a trace is.

/docs/phoenix/quickstart redirected to the Overview page, which is not in
the nav. It now lands on Instrument your agent, the actual first guide in
the series. The Overview page is unchanged and still reachable from the
four pages that link to it.

Agent-Assisted Setup is only ~350 words of prose but 89 lines, because
almost two thirds of it is follow-up reference rather than setup. The
three advanced sections -- re-running a single step, non-interactive runs,
and unsupported agents -- move into an AccordionGroup, so the page reads
as an intro, two steps and a verification section. Nothing is deleted:
all five code blocks are byte-identical and the collapsed text is still
in the page. The one prose deletion is "The fastest way to add Phoenix
tracing to your application.", which restated the frontmatter description
rendering directly above it.

Trade-off worth noting in review: collapsed sections leave the "On this
page" table of contents, which now lists only "Confirm traces are
flowing".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: hold every Get Started intro to four sentences or fewer

The intros restated themselves. Evaluate your agent opened with seven
sentences making three points: what an eval is \(said three times\), what
the guide does \(said twice\), and which metrics you can score against.

Deletions only, no rewriting. Every surviving sentence is verbatim.

Evaluate your agent, 7 -> 4. Dropped:
- "Evaluations attach quality signals to runs so that correctness or
  relevance can be reasoned about consistently instead of judged case by
  case." Restates the opening sentence and the traces/evals contrast that
  follows it, and states it as a negative.
- "We'll start with data that already exists in Phoenix, define a simple
  evaluation, and run it so we can see results directly in the UI."
  Restates "In this guide, you'll set up evaluations ... and run them on
  existing trace data".
- "The goal is to move from "I have model outputs" to "I can measure
  quality in a repeatable way."" Restates the opening sentence.

Iterate on Your Prompts, 5 -> 3. Dropped "Up to this point, we've traced
our agent runs and evaluated their outputs. Now we'll focus on prompts by
grouping failures into a dataset and iterating on prompt variants in the
Prompt Playground." -- the sentence above it already says the guide
groups failing runs into a dataset and iterates in the Playground, and
the prereq list directly below states the same continuity as two bullets.

Instrument your agent, TypeScript tab 5 -> 4. Dropped "In this guide,
we'll get tracing set up and walk through how to instrument an
application.", which the sentence after it says with the specifics. The
Python tab already carried this as one sentence.

Counts are per reader, shared intro plus their language tab, excluding
callouts: Instrument 4, Evaluate 4, Iterate 3, Optimize 3,
Agent-Assisted Setup 1.

Code blocks unchanged on all three pages.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say "agent" instead of "app" across Get Started

Refers to the reader's system as an agent throughout, matching the
section titles \("Instrument your agent", "Evaluate your agent"\). 51 lines
across the five guides plus the Overview page. Code blocks and URLs are
untouched; the `js/examples/apps/mastra-quickstart` paths and every
identifier keep their spelling.

The experiments guide's title becomes "Optimize Your Agent with
Experiments", so the nav label changes with it.

Two phrases needed care rather than a plain swap:

- "an agent application" would have become "an agent agent"; it is now
  just "an agent".
- "the agents that make up our application" would have become "the agents
  that make up our agent"; it is now "our agent system", the phrasing the
  experiments guide already uses.

Agent-Assisted Setup keeps "app" for the reader's codebase. That page is
about handing work to a *coding* agent, so "let your coding agent add
tracing to your agent" reads as one thing instrumenting itself. The
collision is worse than the inconsistency.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: merge the prompts and experiments guides into Improve your agent

"Iterate on Your Prompts" and "Optimize Your Agent with Experiments" were
two halves of one workflow split by *how* you make the change rather than
by what you are doing. Both started from eval-labelled failing traces,
both ended in the same Experiments view, and each pointed at the other as
the next step, so a reader could not tell which came first.

They are now one guide, matching the AX quickstart, which has a single
"Improve your agent" page with method tabs rather than a group.

Four steps, with the method choice appearing once instead of at every
stage:

1. Create a dataset from failed traces      \(shared\)
2. Use explanations to identify improvements \(shared; was taught twice\)
3. Make the change and run it   -- tabs: In the Prompt Playground /
   In code, with Python and TypeScript nested inside each
4. Compare versions in the Experiments view  \(shared narrative\)

Nothing is dropped. Verified against both source pages: 13 of 13 code
blocks, 12 of 12 videos, 12 of 12 frame captions, every numbered
instruction, and every H3 section.

Deliberate changes beyond the merge:

- The two cross-links between the guides are gone; they would now point
  at this page. That circularity was the reason to merge.
- "the first from Step 3" is now "the first from your baseline run",
  because the step it referenced no longer has that number.
- "A dataset created from previous runs" leaves the prerequisites, since
  step 1 builds it.
- The two "Congratulations" lines and the two "Learn More" sections are
  one each.
- About eight new connective sentences were written, the only prose on
  this page that is not carried over verbatim. Interleaving two
  workflows needs them.

Seven URLs now redirect here: both retired page paths, both ts- paths,
and the three legacy quickstart paths.

docs/phoenix.mdx had a card for each guide in both its Python and
TypeScript groups; each pair is now one "Improve Your Agent" card.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rewrite Get Started in second person, direct voice

The guides narrated themselves in the first person plural -- "we'll
install", "now that we have trace data, we can", "at this point we've
defined" -- which reads as tutorial filler and contradicts the house rule
that guides use active voice and second person. 155 instances of that
register across the section, now 0:

  royal "our"                   55 -> 0
  royal "we'll" / "we're"       44 -> 0
  "we can"                      26 -> 0
  "At this point,"               7 -> 0
  "Next, we..."                  6 -> 0
  "In this step, we..."          6 -> 0
  "Now that ..., we"             4 -> 0
  "let's"                        5 -> 0
  "More importantly,"            2 -> 0

Instructions are now imperative and the prose states facts about the
system rather than narrating a shared journey. "Next, we'll register a
tracer provider linked to a project in Phoenix. This project is where
your traces will show up" becomes "Register a tracer provider linked to a
project in Phoenix. That project is where your traces show up."

Two errors fixed on the way through:

- "seen how an agent runs shows up from start to finish" -> "how an agent
  run shows up"
- "our finanical writer agent" -> "The financial writer agent"

No instruction was dropped. Code blocks, videos, link destinations, step
titles and H3 sections are identical on all three guides: 17/2/13/9/14 on
tracing, 14/2/9/9/5 on evaluations, 13/12/5/4/9 on Improve your agent.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: sharpen the tracing opener and drop every em dash

The tracing guide opened with a sentence that did not parse: "A trace is
a record of a single run of your agent, broken down into spans that show
what happened at each step \(how agents, tasks, and tools executed\) and
provides the raw data needed for everything that follows." After the
participial phrase, "and provides" cannot reach back to "is a record", so
it reads as attaching to "spans", which would need "provide". It also
said the same thing twice \("what happened at each step" and "how agents,
tasks, and tools executed"\), and closed on "the raw data needed for
everything that follows", which was left over from the series framing
that now lives only on the Overview page.

Replaced with the house formulation from ax/instrument/what-are-traces
and phoenix/tracing/concepts-tracing, which also defines a span instead
of gesturing at it:

  A trace captures one end-to-end run of your agent as a tree of spans.
  Each span is a single step \(an agent, a task, a tool call, or an LLM
  call\) with its input, output, and timing.

The page also picks up a frontmatter description, matching the AX tracing
guide. It renders as the subtitle under the H1, so it carries the framing
while the body carries the mechanic, and the two do not restate each
other.

Em and en dashes are now zero across the section. Two were mine; two were
pre-existing on Agent-Assisted Setup, where a dash joined two independent
clauses that are clearer as separate sentences.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: stop repeating the prerequisites, and drop the long reference from Get Started

The evaluations guide stated its prerequisites twice, about fifteen lines
apart, in different words. At the top: "you'll need to have completed Get
Started with Tracing so you should have: Financial Analysis and Research
Chatbot / Trace Data in Phoenix". Then again inside step 1: "Evaluations
in Phoenix run over existing trace data. If you followed the tracing
guide, you should already have: A project in Phoenix / Traces containing
LLM inputs and outputs". Both tabs carried it, so a reader met the same
list twice per pass.

The step's version was the more precise one, so its detail is folded into
the single list at the top, which now reads "A project in Phoenix holding
traces with LLM inputs and outputs". Step 1 keeps the one fact it added,
that Phoenix evaluations run over existing trace data, and loses the
duplicate list.

Worth noting for future passes: string-similarity scanning did not catch
this, because the two lists share almost no wording. Semantic repetition
needs a read, not a regex.

The Overview page repeated itself inside single paragraphs the same way:
"using the same inputs and evaluation criteria" in two consecutive
sentences, and "spans that show what happened at each step" followed by
"Traces show how agents, tasks, and tools executed during a run". Both
are now stated once.

Coding Agents leaves the Get Started nav. It is 466 lines, 2258 words and
27 code blocks of reference material for Claude Code, Codex, the CLI, MCP
and skills, and its own opening line says "If you haven't instrumented
your app yet, start with Agent-Assisted Setup" -- it is the step after
getting started, not part of it. The page is unchanged and still listed
under Integrations > Developer Tools, and Agent-Assisted Setup already
links to it for ongoing workflows.

That leaves one page in the group, so it collapses to a single nav entry.
Agent-Assisted Setup gains sidebarTitle "Coding agent setup" so the label
still matches the agreed structure, the same way pxi.mdx shows as "PXI
Agent".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix five accuracy defects found by checking Get Started against the source

Verified the guides against the Phoenix source in this repo rather than
against other docs. Five real defects, all inherited from main:

1. `evals\['completeness'\].label == 'incomplete'` is a **span** filter. The
   guide said "Filter your traces by the incomplete evaluation label",
   which points at the traces table, where the expression is rejected:
   src/phoenix/trace/dsl/trace_filter.py refuses it with "`evals\[...\]` is
   not available in the trace filter". docs/.../filter-expressions.mdx
   lists it under span filters, and the UI has separate SpanFiltersContext
   and TraceFiltersContext. Now reads "Filter the spans table".

2. and 3. The add-to-dataset flow is span-based, not trace-based:
   AddSpansToDatasetInput carries `span_ids`
   \(src/phoenix/server/api/mutations/dataset_mutations.py:225\), and the
   dataset guide says "select the span". "Select the traces you want to
   include" and "Add the selected traces" now say spans.

4. `client = Client\(\)` ran with no import on this page. The evaluations
   guide imports it, but this page has to stand alone, so it now carries
   `from phoenix.client import Client`.

5. `import { getDataset } from "@arizeai/phoenix-client/datasets"` was
   never used. The companion example the guide points at,
   js/examples/apps/mastra-quickstart/src/mastra/experiments/experiment.ts,
   imports the identical set without it. Removed.

Everything else checked out against source: register\(project_name,
auto_instrument\), LLM\(provider, model\), ClassificationEvaluator\(name, llm,
prompt_template, choices\) and .bind\(\), Client\(\).datasets.get_dataset\(dataset=\),
Client\(\).experiments.run_experiment\(dataset=, task=, evaluators=\),
suppress_tracing's import path, the phoenix-client and phoenix-evals TS
exports, runExperiment accepting a {datasetName} selector, ports 6006 and
4317, exit code 6 as NOT_VERIFIED, every `px setup` subcommand and all
nine documented flags, and both companion code paths.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: one language choice on Improve your agent, not ten

The page had accumulated 10 separate Python | TypeScript pickers down its
length: one on the companion-code callout, one per video pair, and one
inside each half of the method tabs. Mintlify syncs tabs that share
labels, so picking a language once did switch all of them, but rendering
the control ten times reads as ten decisions.

Language is now a single choice at the top, the same shape the tracing
and evaluations guides use. Method tabs \(In the Prompt Playground /
In code\) stay where they were, inside step 3 and step 4. A reader now
sees one language picker and two method pickers instead of twelve
controls.

This is purely structural: every Python | TypeScript group was resolved
to the chosen language and the whole body wrapped in one pair. No prose
was rewritten. Verified against the previous version: all 13 code blocks,
12 videos, 12 frame captions, every numbered instruction, every heading
and every step title still present.

The cost is 78 lines. Language as the outer axis means the
language-agnostic narrative -- the dataset steps, the explanation step,
the shared prose in steps 3 and 4 -- exists once per branch. That is why
the step count in the file doubles from 4 to 8 while a reader still sees
4. Worth it for one obvious choice at the top; the alternative was
keeping the page shorter and the navigation confusing.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: method first, language inside it, on Improve your agent

Corrects the tab hierarchy to match ax/get-started/get-started-tracing,
where the outer tabs are methods \(By npx evals / By Arize Skills / By
Code\) and the Python | TypeScript picker sits inside By Code. The previous
commit put language on the outside, which was the wrong axis.

A reader now picks the method first \(In the Prompt Playground / In code\),
then the language within it. Two controls, in that order.

Also tightened the prose, since with tabs every shared line exists in more
than one branch and each cut lands more than once. Deleted as restatements
of the line beside them:

- "This dataset now represents a concrete failure case for your agent."
- "Start by grouping together the traces that didn't perform well."
- "Start from a real prompt the agent actually used." -- the next sentence
  already gives "a real starting point".
- "Saving prompt variants makes it easy to track changes..."
- "An experiment reruns the same inputs through different versions..." --
  the page intro already defines an experiment; this was the third time.
- "With two versions in hand, compare them..." -- restated the step title.

Shortened: the two-paths lead-in \(the tab labels say it\), the
create-datasets aside \(said "in code" twice\), and the task and evaluation
definitions. Fixed two royal "we" the voice pass missed: "the dataset we
made earlier" and "we should see more traces".

Line count: 605 -> 581 from the trims, then -> 694 from the inversion.
The growth is real and worth naming. Steps 1 and 2 -- create the dataset,
read the explanations -- are method-agnostic, so method-outer copies them
into both method branches, and language-inner copies them again. Four
copies of about thirty lines. A reader still sees each step once.

The wrinkle that buys: the method choice now sits above two steps that do
not depend on it, so a reader picks Playground or code before creating the
dataset. The alternative keeps steps 1 and 2 shared and moves the method
choice down to step 3, at the cost of a second visible language picker.

Verified against the previous version: every code block, video, caption,
link, heading and step title still present.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: move the companion-code callout out of the no-code path

The Prompt Playground branch offered "a companion notebook with runnable
code examples" in a path where you write no code. That was a side effect
of wrapping the whole body in the method tabs. The callout now appears
only under In code, for both languages.

The Python | TypeScript picker stays inside the Prompt Playground branch,
because that path does depend on which language you followed -- not for
the language itself, but for what you built with it. Six of the videos
record the CrewAI app rather than the Mastra app, and the dataset input
syntax differs with the agent: `{tickers}` and `{focus}` are CrewAI task
variables, `{{input}}` is Mastra. The label is a proxy for "which
quickstart did you follow".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: drop the horizontal rules bracketing the companion-code callouts

Every companion-code callout in Get Started was wrapped in `---` rules,
inherited from main. Each renders as a thematic break with generous
margins, so the callout sat in a band of empty space with a thin line
above and below it. The callout already has its own bordered container,
so the rules added separation that was not needed.

12 rules removed: four on the tracing guide, four on evaluations, four on
Improve your agent. Frontmatter delimiters are untouched, including the
multi-line description block on the tracing guide, whose closing `---`
sits at line 6 and is easy to mistake for one of these.

No prose changed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the playground template syntax, which would not have interpolated

The Python path told readers to set the User prompt to `{tickers}` and
`{focus}`. That is F_STRING syntax, and the Prompt Playground defaults to
Mustache:

  js/app/src/components/templateEditor/constants.ts
    FString:  "F_STRING",  // {variable}
    Mustache: "MUSTACHE",  // {{variable}}

  js/app/src/store/playground/playgroundStore.tsx:273
    templateFormat: TemplateFormats.Mustache,

Playground.tsx reads the format from that store, so with default settings
single braces are not substituted -- the prompt would have carried the
literal text `{tickers}`. The guide never mentioned the format selector,
so there was no way for a reader to know.

Most likely this was copied from the CrewAI task template on the tracing
guide, `Task\(description="Research: {tickers}"\)`, which legitimately uses
single braces. That is CrewAI's own templating, not Phoenix's, and it is
left alone.

Both paths now use Mustache, and the shared lead-in says so, so the two
language tabs differ only in the variable names.

The variable names themselves were already correct and are framework
driven, traceable in the guides' own code: the Python experiment passes
`example.input` straight into `kickoff\(inputs=...\)`, so dataset examples
carry `tickers` and `focus` keys, while the TypeScript task types
`example.input` as a `{role, content}\[\]` message array under a single
`input` key.

Verified rendered: the block shows `Research: {{tickers}}` literally, and
the format note appears in both language tabs.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: expand the Agent-Assisted Setup accordions back into headings

I collapsed three sections into an AccordionGroup when the page read as
too long. That hid 307 of its 542 words, 57% of the page, and dropped the
three section names from the "On this page" table of contents, leaving one
entry. It also made the page look like content had been deleted, which is
what prompted the question.

The reason for collapsing them has gone: the length concern was mostly the
466-line Coding Agents reference, which no longer sits in this nav group.
What remains is ~330 words, the shortest page in Get Started, so hiding
more than half of it costs more than it saves.

Re-run a single step, Run non-interactively \(CI or agents\), and Use an
unsupported agent are `##` headings again, so all four sections appear in
the table of contents and can be linked to directly.

Code blocks and links are identical.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: no language picker in the Prompt Playground path

You write no code in the Prompt Playground, so asking the reader to pick
Python or TypeScript there was misleading. The picker now appears only
under In code, where it selects actual code, which matches
ax/get-started/get-started-tracing.

Two controls on the page, in this order:

  In the Prompt Playground | In code
    In code -> Python | TypeScript

What the picker had been carrying in that path, and where it went:

- Steps 1, 2 and 4 were already identical between the languages.
- The dataset input keys are real, so they are stated inline: which keys
  you have depends on the agent you traced, `{{tickers}}` and `{{focus}}`
  from the Python guide, `{{input}}` from TypeScript. Both forms are now
  written out rather than left to be inferred.
- The example prompt line keeps the Python variant, which is the one that
  matches the finding step 2 states \("the report lacked financial
  ratios"\). The TypeScript variant said something different, about
  per-ticker metrics.
- Five videos drop out: ts-first-dataset, ts-first-prompt,
  ts-playground-experiment, ts-compare-prompts, ts-view-experiment. They
  carried the same captions and demonstrated the same click-path as the
  five that remain, recorded against Mastra instead of CrewAI.

All 13 code blocks and every link destination are unchanged. 668 -> 538
lines.

Braces checked: every `{{...}}` on the page is either inside a fenced
block or an inline code span, so MDX treats them as text. Verified in the
rendered DOM.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: restructure Get Started nav and verify evals against the product

Reshape the Get Started nav to Observe Your Agent / Evaluate Your Agent /
PXI / Arize Phoenix for Agents / Phoenix Demo Account. Per-language pages
become tabs within each section, with language nested inside the code path.

Merge the Improve Your Agent guide into Evaluate Your Agent, which is now
written as offline evals: a dataset feeds a task, an evaluator scores each
output. Phoenix has not released continuous scoring of live production
traffic, so the previous framing was wrong.

Extract Skills from the Coding Agents page into its own page and group the
agent-facing docs \(agent-assisted setup, skills, MCP\) under Arize Phoenix
for Agents. A pointer heading keeps existing #skills anchors resolving.

Correct six claims against Phoenix 20.15.0 running locally:

- `output` is the task's output, not an example payload. `reference` is the
  example's expected output \(experiment_runner.py:831, message_helpers.py:279,
  and the compare view's "reference output" column\).
- Creating a dataset from spans is one action, not two. You select spans and
  click Add to Dataset; creating a new dataset happens inside that popover
  \(SpanSelectionToolbar.tsx:212, DatasetSelectorPopoverContent.tsx:104\).
- The evaluator dialog's submit button is Create, not Save.
- The playground control is "Test over a dataset".
- Experiment runs live on the Experiments tab, compared via the
  "comparisons" selector.
- Playground template format defaults to Mustache, so dataset input keys
  need double braces.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: remove em dashes from the Get Started nav pages

All 98 em dashes on pages now reachable from Get Started, none of them
authored by this branch: pxi.mdx \(31\), coding-agents.mdx \(22\), remote-mcp.mdx
\(14\), mcp.mdx \(4\), docs-mcp.mdx \(3\), phoenix-mcp-server.mdx \(1\), plus 14
bullet and label dashes replaced with colons.

Each was repunctuated in context rather than swapped for a single character:
a colon where a list or restatement follows, a comma for an aside, a period
where an independent clause follows, parentheses for the three parentheticals
that span hard-wrapped lines. No dash inside a fenced code block was touched.

Diff is 89 insertions and 89 deletions, so no prose was added or removed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: point Evaluate Your Agent at the trace-scoring flow

The merged page covers offline evals over experiments. Readers who want to
score traces they have already collected had no path to it from Get Started,
since that content now lives under Tracing and Evaluation. Links to
Evaluating Phoenix Traces and the two eval quickstarts.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: simplify Phoenix get started quickstarts

* docs: keep Agent, not App, in the Get Started flow

Restores "Observe Your Agent" and "Evaluate Your Agent" and aligns the
surrounding copy, which had moved to "app": both page titles and descriptions,
the section headings on the Get Started overview, the landing-page cards, and
two "Observe Your App" cross-links that would have pointed at a page with a
different name. `app.ts` stays, since that is a file the reader creates.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: one Quick Starts card group, without the Improve card

The Improve page merged into Evaluate Your Agent, so its card pointed at the
same page as the Evaluate card. Dropped it and folded its description into
Evaluate's, which had described only the LLM-as-a-judge half.

The Python and TypeScript groups held identical cards, and both target pages
now select language with tabs, so the split no longer carried information.
Collapsed to a single group. The redirect from the retired improve path is
untouched.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make Evaluate Your Agent a single Playground path

Drops the Prompt Playground / Code tab split and keeps the Playground steps,
so the quickstart teaches one path end to end. 401 lines to 146.

Adds a "Prefer to do this in code?" section that sends readers to Run
Experiments for the Python SDK and to the TypeScript experiments reference for
runExperiment, so both languages the removed tab covered still have a home.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say which traces to evaluate and what to measure

The steps described where to click without answering the two questions a
reader arrives with. Five tweaks, 146 lines to 151:

- Dataset step gives a size to aim for and two ways to find failing traces,
  a filter expression or asking PXI to walk them
- Evaluator step names error patterns worth scoring instead of only listing
  the two evaluator types
- Saving a prompt from a trace opens with why that prompt is the one to use
- The baseline is stated as the thing later edits are measured against
- Comparison step closes with how to read the delta, including what to do
  when the score drops

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say offline evaluators in the Evaluate description

Also removes two lines the single-path change left stale: an opening sentence
that restated the description, and a paragraph telling readers to choose
between the Playground and code paths, which are no longer tabs on this page.
The offline framing replaces it, with the caveat that continuous scoring of
live production traffic has not shipped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say the manual path builds everything from the Phoenix UI

Now that this page is Playground-only, the section intro should name the
surface the steps use.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: shorten the PXI line in the dataset step

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: restore the code experiment example as its own page

Making Evaluate Your Agent a single Playground path deleted Nancy's Python
and TypeScript worked examples, and the pages it linked to instead cover the
same APIs without that scenario. Recovered them verbatim from 966510c74 as
Evaluate a Prompt Change in Code, under How-to: Experiments.

All 7 code blocks and both 4-step sequences are byte-identical to the removed
tab; only the Tab wrapper was dropped and the block de-indented one level. The
page opens by tying itself to the quickstart, so the support agent scenario
carries across the UI and code paths again.

"Prefer to do this in code?" now points here first, with Run Experiments as
the wider reference.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the new experiment page headings its siblings have

The page had no H2s, so it contributed nothing to the table of contents while
every other page in How-to: Experiments has several. Adds a heading over the
tabbed walkthrough, notes that the script creates its own dataset so it runs
without traces, and points at Set a Baseline Experiment, which covers the
persistent baseline that this page's in-code comparison does not.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rework Observe and Evaluate, split PXI into Get Started and Learn More

Observe Your Agent: intro rewritten to say what the page does and where the
concepts live, `\(Recommended\)` on the agent-assisted path, and the manual
steps reordered to start Phoenix, get your keys, then set up tracing. The
install step folded into setup, and the three steps that were identical in
both language tabs hoisted above them.

Evaluate Your Agent: no longer depends on having traces. The dataset step
hands you three travel support examples to upload, so the page runs on a
fresh Phoenix. Traces became an optional route rather than the only one.
Switched to an LLM judge, added copyable Playground prompts for the baseline
and the edit, and dropped the save-a-prompt-from-a-trace step.

PXI: 3,315 words split into a 315-word Get Started \(what it is, what it does,
three steps\) and a 1,158-word Learn More grouped as capabilities, surfaces,
configuration, and privacy. Nothing deleted except the architecture diagram
and the flag and slash-command tables, which duplicate the CLI reference.
Accordions flattened so nothing is hidden behind a click.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: consolidate Arize Phoenix for Agents

Snapshot of the agents/MCP consolidation: agent-assisted-setup folded into a
new developer-tools/cli page, and remote-mcp, docs-mcp, and phoenix-mcp-server
collapsed into one integrations/mcp page, with redirects for all four retired
paths. Nav and inbound references updated across the CLI reference, release
notes, self-hosting, and the shared onboarding snippets.

Committed from the docs-get-started-reorg worktree so the two in-flight
branches can be merged.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: point the tracing quickstart at the new CLI page

The agents consolidation folded agent-assisted-setup into
developer-tools/cli. This was the last reference still relying on the
redirect rather than the real page.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: link the uploaded dataset, and cross-link the other Observe methods

The travel-support.csv download pointed at assets/datasets/, but the file was
uploaded to assets/images/arize-docs-images/evaluate/. Repointed to where it
actually lives, so the Evaluate walkthrough runs end to end. Verified the URL
serves 3 rows with the question and expected_answer columns the page maps.

Also adds links out of both quickstarts for the routes they do not teach,
prompted by the Phoenix workflow diagram: reading traces in code, from a
coding agent via the CLI or MCP, by asking PXI, and across Sessions; plus PXI
for curating datasets and running experiments. Links only, no new pages. The
Evaluate step stays absent on purpose, since online evals have not shipped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make both Evaluate paths use one dataset, and close three story gaps

Found by running every path in Get Started end to end as a new user.

The agent-assisted prompt inlined three examples, but only one of them was in
the CSV the manual path downloads. Cutting that file from eight rows to three
had left the prompt behind. The prompt now downloads the same CSV, so there is
one dataset and the two paths cannot drift again. It also restores the page's
own argument, which turns on the visa and refund rows.

Three gaps between the paths:

- The Recommended path ended when px setup finished and never said to open the
  Traces view. Only the manual path told you to look at what you captured.
- "Ask your coding agent" said to start Phoenix without giving the command,
  which only appeared in the other tab.
- Observe handed off promising to "convert selected traces into a dataset",
  which Evaluate no longer does, since it starts from a downloadable dataset.

Verified by execution, not reading: both language paths install and run and
produce correct OpenInference traces \(a support_reply CHAIN wrapping a
ChatCompletion LLM span with messages, model, and token counts\), px setup
connects and writes .env.phoenix, both skills commands install, and the CSV
builds the same 3-example dataset from either path.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: state the Python 3.10 floor and name the Traces tab in Observe

Both quickstart paths were run end to end against Phoenix 20.16.0. On
macOS's default Python 3.9, pip quietly installs arize-phoenix-otel
0.14.0 instead of 0.17.1, because the current packages require 3.10+.
A project also opens on the Spans tab, so "the Traces view" sent
readers looking for a view that isn't the landing page.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>

* docs: make the Evaluate comparison actually move the score

The page claimed the baseline prompt invents visa rules and agrees to refunds
it should decline. Measured against gpt-5-mini it does neither: the baseline
scored 1.0, the edited prompt scored 1.0, and stripping the baseline to
"Answer the customer's question" still scored 1.0. Step 4 promised a rise that
never came, so the lesson did not land.

Fixed by changing what the examples test. They now ask about a fictional
airline's Basic fare policy, with values deliberately unlike industry norms: a
48 hour refund window, a 40 dollar change fee, one included checked bag. The
model cannot guess any of them, so the baseline answers from industry defaults
and gets all three wrong. The edit supplies the policy.

Measured: baseline 0.00, edited 1.00.

This also makes the lesson durable. The baseline fails for lack of information
rather than lack of capability, so a better model will not quietly erase the
gap the way it did with the previous examples.

Also drops the Observe handoff sentence, per review.

The dataset CSV needs re-uploading to the same path; the agent-assisted prompt
reads it by URL and needs no edit.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the PXI paths, found by running them

Tested both PXI pages against a live Phoenix for the first time.

The terminal client command did not work. `npx -y @arizeai/phoenix-cli pxi`
exits with "unknown command 'pxi'", because pxi is a separate bin in the
package rather than a px subcommand, so npx ran the default bin and passed
pxi as an argument. Needs `-p`. Verified the corrected form launches and
reaches the credential check.

PXI defaults to an Anthropic model, so a reader who follows step 1 and sets
only OPENAI_API_KEY opens the assistant and hits "Anthropic credentials are
not configured". The step now says to pick a model matching the provider you
configured. Confirmed by switching to gpt-5.5 and getting a correct answer
about project span counts.

Restores "Where your data goes" under Privacy and tracing. It was dropped with
the architecture section, but it is not architecture: it is the answer to
whether data leaves your instance. No separate Arize service, your provider
called with your key, Arize not in the request path, and remote export gated
three ways. That is the first thing a security review asks about a beta AI
feature.

Also: the provider list mixed three env var names in code with "AWS Bedrock"
in prose, so it now names providers throughout; "Drives the product" lost its
object when I trimmed it; and the roadmap milestone link is out, since a
GitHub milestone goes stale.

Verified live: the consent gate appears as described, trace saving shows
"Disabled by system settings" which matches the documented default, the model
list matches the validated set, and PXI answered using search, get_schema, and
execute, the code-mode surface the page describes.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the CLI page its own job, running Phoenix from the terminal

The page claimed px was "the recommended way for a coding agent to work with
Phoenix", then documented only px setup, which Observe already owns. So it
repeated onboarding and never showed an agent working with Phoenix. Meanwhile
px has 17 top-level commands and the page covered one.

Rewritten around the debugging loop the phoenix-cli skill actually drives:
find what broke, read one trace in full, record the judgement as an
annotation, inspect the datasets and experiments you test against, then work
on the prompt. Plus a table for the remaining commands. Setup is now a single
line pointing at Observe.

Every command was run against a live Phoenix, which caught three errors in my
own first draft:

- `px span list` has no `--filter`. It takes typed flags, so the examples use
  `--status-code ERROR` and `--span-kind LLM`.
- `px experiment list` requires `--dataset`.
- `px trace get` resolves the default project and reports "Trace not found"
  without `--project`.

Verified working: trace list, trace get, trace add-note, span list with both
filters, span annotate \(annotation upserted\), dataset list, dataset get,
experiment list, prompt list, project list, and all 11 commands in the table.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: lead the agents overview with the setup table

Move "Set up your agent" above the three-piece cards so the
per-agent map comes first, and make its lead-in name the three
pieces so it stands on its own.

Narrow the endpoint rule: the plugins and the px CLI take the base
endpoint and append /mcp themselves, while `claude mcp add` and the
per-client MCP steps take the full /mcp URL.

Reword the prompt samples as examples rather than the full set.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the agents overview one job, shorten the MCP page

The overview page was mostly navigation: three cards pointing at CLI,
MCP Servers, and Skills, which already sit next to it in the sidebar,
plus a setup table routing to the same three pages. The sidebar label
"Overview" also repeated the group name, so the page read as a section
divider.

Now it does one thing, connect a coding agent to Phoenix: what this is,
the disambiguation note, the per-agent setup table, the endpoint. The
three cards are gone, the prompt samples are cut from five to two, and
the node_modules section is dropped since the TypeScript SDK overview
already documents it. Relabel the sidebar entry "Set Up Your Agent".

Correct the Claude Code row: the plugin ships three skills, while
px setup skills installs five, so the plugin path needs px setup skills
too. Verified by listing the plugin and running both.

On the MCP page, lead Connect with px setup mcp, merge the Cursor,
VS Code, and OpenCode accordions \(their JSON matches what px writes\),
replace the six Docs MCP accordions with a client table, and merge the
identical npm stdio configs. 633 to 521 lines with no facts dropped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: move the plugins onto the setup page

The setup page was still an index: its table pointed at Install
sections that CLI, MCP Servers, and Skills each already have. What it
lacked was the thing that belongs to no single one of them, the
one-install-per-agent path.

That path was the Plugins section, 137 lines and the largest section on
the MCP page, which is the wrong home for a plugin that registers MCP
and skills together. Move it here, after the endpoint it references,
and leave a pointer on the MCP page.

MCP drops to 388 lines and covers only MCP servers. The setup page now
routes by agent, states the shared endpoint, then gives the full plugin
steps.

Also repoint skills.mdx at the plugins section's new home, and fix its
link to a CLI heading that the CLI rewrite removed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the MCP page opener

It said "two things you might want your assistant to do" and "run
both" above a three-row table. Name the two servers that matter, your
data and the documentation, and say what the third row is.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: cut duplicated prose from the MCP page

The Configuration table repeated two env vars that self-hosting
configuration already documents; point there and keep only
PHOENIX_SKILLS_PATHS, which is unique to this page.

Monty sandbox limits were spelled out twice, in How it works and again
in Security; state them once. Merge the code-mode Warning and Note,
which both said the same thing. Trim the OAuth paragraph. Move the
OAuth2 authorization-server paragraph out of "Mounting your own
skills", where it was misfiled, into Security. Cut the npm sample
questions from seven to three.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: clarify the direction of the agents note

"Operate on Phoenix" is jargon, and "trace your sessions with a coding
agent" reads either way. Say which direction each page covers: this one
connects the agent to Phoenix, the other has Phoenix record the agent.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: correct two plugin commands that fail as written

Both found by running them. claude plugin uninstall acts on user scope
by default, so it stops with a scope error on a plugin installed at
local or project scope; say to pass the scope, and add disable, which
has the same rule. claude plugin details reports a disabled plugin as
not found, so say it has to be enabled, and say what the command lists.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: correct two CLI table rows that overstated what the commands do

Found by running them. px api has one subcommand, graphql, and rejects
mutations, so "authenticated requests to any Phoenix API route" is
wrong. The three annotation commands take only delete, and require
--all or a time window, so "stored annotations" hid that they are
destructive.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make the CLI debugging loop match what px can do

Steps 4 and 5 were titled "Turn failures into a test set" and "Work on
the prompt", but every command under them only reads, and the bodies
said "Inspect", contradicting the titles. px has no way to create a
dataset, add examples, or edit a prompt: for datasets, prompts, and
experiments it offers list, get, and delete only.

Cut the loop to the three steps px actually supports, ending at the
annotation, which with notes is the only thing it writes. Move the
dataset, experiment, and prompt reads into their own section framed as
looking up context, and say plainly that authoring happens in the UI
or the SDK.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rewrite the CLI page as the workflow an agent runs

The page had a "debugging loop" that was not a loop: it ended at an
annotation, and px cannot fix anything. Reorganising it by capability
then made it a feature list. Put it back in order as the pass an agent
actually runs, and let step 4 say plainly that px stops there and the
fix goes in your code, which is what closes the loop.

The examples were the weaker half. They repeated a fake project four
times, used placeholder ids that cannot be run, and carried comments
that restated the command instead of the output. Use real ids, say
which command feeds the next, and split the dataset, experiment, and
prompt reads into their own section, since those are for checking a
change rather than part of the debugging pass.

Add three facts found by running the commands: reads accept --format
json, annotate and add-note want the OpenTelemetry id rather than the
opaque one, and --identifier makes a repeat call overwrite.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: note that px annotation deletes are gated off by default

px span-annotations delete answers "Delete commands are disabled."
before it looks at any flag. Name the variable that opens the gate,
PHOENIX_CLI_DANGEROUSLY_ENABLE_DELETES, so the row does not send a
reader at a command that refuses to run.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say who runs px, and that the docs MCP URL is not a page

The CLI page opened by saying px is how a coding agent works with
Phoenix, then showed a reader commands to type, and titled the section
"How an agent uses it". A reader could not tell whether the commands
were theirs to run. Say it is an ordinary CLI you run yourself, and
that needing only a shell is what makes it easy to hand to an agent.

The Phoenix Docs MCP URL sat in a bare code block, so it read as a link
to open. A browser shows the tool manifest as JSON, which looks like an
error and is not. Say it is an endpoint for a client.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: answer px versus MCP, and add agent instructions to both pages

Neither page said when to reach for which. That choice only existed as
cards on the setup page, which is not where someone deciding ends up.
The CLI page now compares the two on what the client must support, what
setup costs, and how much surface each exposes, and the MCP page points
at it.

Both pages end with a section addressed to an agent reading them,
covering how to discover flags, which output format to parse, the
OpenTelemetry id the annotate commands want, the execute sandbox limits
and import allowlist, and that trace contents are untrusted data.

Every claim in both sections was run first: 116 catalog operations, the
50-call cap, json/re/datetime importable and socket/subprocess/importlib
not, exit status 0 and 1, and the delete gate.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rebuild the CLI page to a fixed outline, and align sibling names

The page now follows one order: what it is and why it matters, CLI or
MCP server, set up, what you can do with it, learn more, agent
instructions. Setup is three steps ending in a check. The examples
keep their real ids and drop the repeated --project in favour of
PHOENIX_PROJECT, set once in step two. The command table is gone; the
two caveats it carried, the delete gate and read-only GraphQL, live in
the agent instructions, and the full list is a link to the reference.

An audit of how each page in the section names its siblings found six
mismatches: the CLI page linked as "px CLI" and as bare "MCP", the
Remote MCP Server as "server", and a PXI section by a title it does
not have. Each link text now matches the page or section it points at.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: name the CLI setup check by what it proves

"Check it" said nothing. The step confirms the CLI reached the
instance, so say that, and say what success looks like.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say what the CLI setup is for before the steps

The section went straight from the heading to step one, so a reader
could not tell what was being set up or where. Name the CLI in the
heading, say to install it where the agent runs, and say what having
finished looks like.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say what px project list returns on a fresh instance, drop "has a terminal"

The setup check said a table of your projects means success, which
leaves a reader with no traces unsure what to expect. Tested on an
empty instance: every Phoenix has a project named default, so the
command always returns at least one row, and a wrong endpoint fails
with "Error fetching projects: fetch failed". Say both.

"When your agent has a terminal" was jargon on the CLI and MCP pages.
Say what it means: the agent can run shell commands, which every
coding agent can.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the CLI-first recommendation, say what the default project is

"Start with the CLI" was a preference with nothing behind it, and it
contradicted the setup page, which sends Claude Code and Codex users
to the plugin first, and the MCP page, which calls itself the primary
choice. The comparison table carries the facts; let the reader choose.
The MCP page mirror is now a plain pointer.

The setup check mentions the default project. Say what it is: created
by the initial migration, the destination for traces sent without a
project name, and empty on a fresh instance. Not a demo.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: make the CLI page work for a user with no traces yet

Run as a brand-new user, in a clean environment against an empty
instance, the previous page failed at step two: PHOENIX_PROJECT=my-app
404s on every read, and every example carried an id from one machine.
Stuff has to work before a project has traces.

Follow the shape of the live CLI section: install, endpoint, verify
with px --help and px project list, which both succeed with zero data,
then the common agent workflows as bullets and one example prompt, so
the reader sees what to ask rather than what the agent types. Say that
PHOENIX_PROJECT comes later, once traces are flowing. Keep the CLI or
MCP table and the agent instructions, and add the missing rule that
read commands need a project.

On the MCP page, restore the per-client tabs for the Docs MCP from the
live site, adding Claude Desktop.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: one story across Arize Phoenix for Agents, working from zero traces

The section was organised by plumbing, CLI, MCP, Skills, and every page
assumed traces already existed. A new user has none. Reframe around
what is true from the start: connect the agent, let it instrument your
app, then debug from what Phoenix records.

Set Up Your Agent owns routing, the endpoint, and the plugins. Its
connection check is a question to the agent, list my Phoenix projects,
which a fresh instance answers with default. Verified through both
pipes on an empty instance: px project list, and MCP search finding
getProjects and execute returning it. A new section points at the
agent-assisted tracing path as the next step.

CLI, MCP Servers, and Skills share one skeleton: what it is, install
ending in a check, what your agent can do with it, learn more,
reference. Deep material on the MCP page, API-key fallback, code mode
internals, server configuration, security, the npm server, sits under
Reference. The MCP Endpoint section folds into one line. The skills
page moves its options table and custom-skills guidance under
Reference and gains an example prompt.

Every shell command on the four pages was run in a clean environment
against an empty Phoenix. Restore the remote-mcp-server anchor, which
eleven pages link to, and repoint two onboarding snippets that still
named a CLI heading removed in an earlier rewrite.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut the instrumentation pointer back to one line

It was a sentence before, and a heading with three sentences after. The
link is the useful part.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: nest the MCP capabilities section under Remote MCP Server

The page covers two servers, so "what your agent can do with it" as a
top-level heading left "it" ambiguous. Make it an H3 under Remote MCP
Server, alongside a new Connect H3 for the setup steps.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut duplicated prose from the MCP page

The intro paragraph restated the table under it. The capabilities
section under Remote MCP Server repeated Reference, How it works,
nearly sentence for sentence; point there instead. Three sentences of
OAuth mechanism sat ahead of the first command; one line after the
accordions covers what a reader needs. The Docs MCP intro restated its
table row, the npm section said maintenance mode twice, and the
mounted-skills paragraph named its variable twice in a row.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: link the CLI page back to Set Up Your Agent

The MCP and Skills pages both point back to the setup page; the CLI
page did not, so a reader who landed there directly had no route to
the per-agent install.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: frame PXI skills as example capabilities

The table led with skill names, which is the mechanism. Lead with what
you can ask PXI to do, give one example ask per row, and keep the skill
concept as a single sentence after the table.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: frame PXI models as bring your own, and trim

Lead with the point: PXI runs on your provider and your key. Name the
providers once, keep the validated-model list and the tool-calling
caveat, drop the rest. Repoint the two links that used the old anchor.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the agent-instructions section from the CLI page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the agent-instructions section from the MCP page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the validated-model list from PXI

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: stop promising a supported-models list PXI no longer has

The list was removed; three references still pointed at it. Retitle the
Get Started link to the section that exists, and say model providers
where the Learn more line and the page description said supported
models.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: remove the npm MCP server from the MCP page

Drop the maintenance-mode section, its row in the intro table, and the
three places that offered it as a fallback: the version-floor line, the
Claude Desktop accordion, and the troubleshooting entry.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: act on the docs review: skills guidance, Docs MCP callout, MCP cut-down

Skills page: name the five skills written for Phoenix users, add the
plugin-versus-skills-add guidance from the review \(plugins track
Phoenix releases; skills add is a one-time copy, rerun it or run npx
skills update\), and take ownership of mounting your own skills from the
MCP page. Document --all honestly: tested in a fresh project it
installed 26 skills to 73 agents, including internal development
skills, so it is the wrong default.

MCP page: Phoenix Docs MCP becomes a short block near the top, as the
review asked, in place of an 87-line section. Fold Antigravity into the
other-clients accordion, trim How it works to the tool table plus two
sentences, and drop the mounting section now that Skills owns it.
361 to 271 lines.

Evaluate Your Agent: move the spans screenshot out of the Note it was
nested in.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: bring llms.txt in line with the Get Started and agents reorg

Add the CLI, Skills, and PXI Learn More pages, which had no entries.
Remove seven Quick Start entries for pages the reorg deleted or merged
into Observe Your Agent and Evaluate Your Agent, and three MCP entries
for pages folded into the single MCP page. Retitle the survivors to
match their pages.

Add the agent-usage guidance for the px CLI and the Remote MCP Server
here, where the review said it belongs, instead of on the human pages.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: give the CLI guide entry in llms.txt a unique title

It collided with the @arizeai/phoenix-cli reference entry.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten the endpoint rule on the setup page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: halve the agent-assisted setup on Observe Your Agent

Start Phoenix once above the tabs instead of in each. One sentence of
outcome per tab. Cut the pasted prompt to the two instructions that
matter, since the tracing skill carries the procedure. Drop the agent
list and the CLI pointer, which the setup page owns.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: add the PXI path to Evaluate Your Agent

A prompt to hand PXI once the dataset exists and the Playground has it
selected. Both preconditions come from running it: PXI cannot fetch a
URL, and its playground and evaluator skills act on the page you are on.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: restore the agent-assisted shape on Observe, trimmed

Back to the two tabs as they were, each self-contained, with the full
prompt. Merge the px setup outcome into one paragraph. Replace the
named --skill list with px setup skills, which installs exactly the
five Phoenix skills for every detected agent: tested via npx in a fresh
project with no CLI installed.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: repoint a PXI conversations link to the Learn More page

The Conversations section moved there when PXI split into Get Started
and Learn More; one link still pointed at the old page.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: Evaluate Your Agent builds a reference-aware evaluator

Built-in LLM templates have no reference field and score the baseline
higher than the fixed prompt, so step 2 now walks through Create new
LLM evaluator with a short rubric and the reference path mapping. PXI
section hands off after the dataset and evaluator exist, since it
cannot fetch the CSV and its own evaluator scores 0 at run time.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten Evaluate Your Agent evaluator step and PXI hand-off

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: PXI runs the full Evaluate flow from one prompt

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten Evaluate Your Agent prose

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut PXI Learn More to the essentials

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: one-line rows in the PXI capabilities table

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: shorten PXI privacy and tracing

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: compact PXI configuration table

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: shorten CLI page intro

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten MCP Servers prose

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop em dashes from Get Started snippets

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: add the follow-up prompt to the coding agent path

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: make px setup skills the primary install on the Skills page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say where the model API key goes before creating an LLM evaluator

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: the key must be on the Server tab for evaluators to use it

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say when to pick the CLI and when the MCP server

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: CLI vs MCP as best-when guidance

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop redundant line from CLI or MCP section

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: plugin rows say what the plugin covers and what is optional

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop Docs MCP aside from Set Up Your Agent

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

*…")  | Oct 9, 2026  |  
|  [chore: update phoenix version to 20.20.0 in kustomize and helm (](https://github.com/Arize-ai/phoenix/commit/6640ea8493aece628988c9def952a7ba71415319 "chore: update phoenix version to 20.20.0 in kustomize and helm \(#16886\)

Co-authored-by: mikeldking <5640648+mikeldking@users.noreply.github.com>")[#16886](https://github.com/Arize-ai/phoenix/pull/16886)[)](https://github.com/Arize-ai/phoenix/commit/6640ea8493aece628988c9def952a7ba71415319 "chore: update phoenix version to 20.20.0 in kustomize and helm \(#16886\)

Co-authored-by: mikeldking <5640648+mikeldking@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   | [docs: remove editable table system spec](https://github.com/Arize-ai/phoenix/commit/b6f085d97352fde7fee3659ed98ef51322497940 "docs: remove editable table system spec")  | Sep 15, 2026  |  
| [java/examples/google-adk](https://github.com/Arize-ai/phoenix/tree/main/java/examples/google-adk "This path skips through empty directories")  | [java/examples/google-adk](https://github.com/Arize-ai/phoenix/tree/main/java/examples/google-adk "This path skips through empty directories")  | [docs: add Google ADK Java tracing guide](https://github.com/Arize-ai/phoenix/commit/43347e5d541387617ac5d622c6d589d3c729748d "docs: add Google ADK Java tracing guide")  | Sep 15, 2026  |  
|  [chore(js): update versions (](https://github.com/Arize-ai/phoenix/commit/01d879894e6472e4a6be38d657c6d116c68f5aa8 "chore\(js\): update versions \(#16906\)")[#16906](https://github.com/Arize-ai/phoenix/pull/16906)[)](https://github.com/Arize-ai/phoenix/commit/01d879894e6472e4a6be38d657c6d116c68f5aa8 "chore\(js\): update versions \(#16906\)")  | Oct 9, 2026  |  
|   |   |  [chore: update phoenix version to 20.20.0 in kustomize and helm (](https://github.com/Arize-ai/phoenix/commit/6640ea8493aece628988c9def952a7ba71415319 "chore: update phoenix version to 20.20.0 in kustomize and helm \(#16886\)

Co-authored-by: mikeldking <5640648+mikeldking@users.noreply.github.com>")[#16886](https://github.com/Arize-ai/phoenix/pull/16886)[)](https://github.com/Arize-ai/phoenix/commit/6640ea8493aece628988c9def952a7ba71415319 "chore: update phoenix version to 20.20.0 in kustomize and helm \(#16886\)

Co-authored-by: mikeldking <5640648+mikeldking@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   |  [fix: stop dropping evaluator metadata nested under](https://github.com/Arize-ai/phoenix/commit/d927595cb8ee91aacdb7af8fff3fca1d095cf2eb "fix: stop dropping evaluator metadata nested under `result` \(#16902\)

* fix\(client\): send evaluator metadata at top level of evaluation request

* fix: stop dropping evaluator metadata nested under `result`

`POST /v1/experiment_evaluations` reads `metadata` only from the top level
and its `result` model ignores unknown keys, so evaluator metadata nested
under `result` was silently discarded. The JS client \(fixed in #16895\) and
the Python client's `run_experiment`/`evaluate_experiment` both sent it
there.

Why it went unnoticed:
- openapi-fetch infers the `client.POST` body type from the argument, so
  TypeScript skips excess-property checks and stray keys compile.
- Python's `ExperimentEvaluation` subclasses the wire `result` type and adds
  `metadata`, and the body was built from `eval_run.__dict__`.
- The server test posted nested metadata and asserted it was stored as `{}`.

Changes:
- Server: recover `result.metadata` from the raw payload \(top-level keys
  win\), so already-released clients stop losing metadata.
- Python client: build the request body field by field, with metadata at
  the top level.
- JS client: route all three evaluation POSTs through
  `toExperimentEvaluationRequestBody`, which returns the declared request
  type, so a nested `metadata` key is now a compile error.
- Tests: server unit test now asserts metadata round-trips, and a new test
  covers the legacy fallback. Adds an end-to-end integration test \(sync and
  async\) plus client unit tests for both bodies.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01BQxMkhaaPtbrf7CumWNvNm

* ci: include sync_models.py in the unit test sparse checkout

tests/unit/server/cost_tracking/test_sync_models_mistral.py \(#16880\) loads
.github/.scripts/sync_models.py by path, but the Unit Tests jobs check out
only a sparse subset of the repo, so the test errors with FileNotFoundError.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01BQxMkhaaPtbrf7CumWNvNm

---------

Co-authored-by: Suparva <suparva@outlook.in>
Co-authored-by: Claude <noreply@anthropic.com>") [(](https://github.com/Arize-ai/phoenix/commit/d927595cb8ee91aacdb7af8fff3fca1d095cf2eb "fix: stop dropping evaluator metadata nested under `result` \(#16902\)

* fix\(client\): send evaluator metadata at top level of evaluation request

* fix: stop dropping evaluator metadata nested under `result`

`POST /v1/experiment_evaluations` reads `metadata` only from the top level
and its `result` model ignores unknown keys, so evaluator metadata nested
under `result` was silently discarded. The JS client \(fixed in #16895\) and
the Python client's `run_experiment`/`evaluate_experiment` both sent it
there.

Why it went unnoticed:
- openapi-fetch infers the `client.POST` body type from the argument, so
  TypeScript skips excess-property checks and stray keys compile.
- Python's `ExperimentEvaluation` subclasses the wire `result` type and adds
  `metadata`, and the body was built from `eval_run.__dict__`.
- The server test posted nested metadata and asserted it was stored as `{}`.

Changes:
- Server: recover `result.metadata` from the raw payload \(top-level keys
  win\), so already-released clients stop losing metadata.
- Python client: build the request body field by field, with metadata at
  the top level.
- JS client: route all three evaluation POSTs through
  `toExperimentEvaluationRequestBody`, which returns the declared request
  type, so a nested `metadata` key is now a compile error.
- Tests: server unit test now asserts metadata round-trips, and a new test
  covers the legacy fallback. Adds an end-to-end integration test \(sync and
  async\) plus client unit tests for both bodies.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01BQxMkhaaPtbrf7CumWNvNm

* ci: include sync_models.py in the unit test sparse checkout

tests/unit/server/cost_tracking/test_sync_models_mistral.py \(#16880\) loads
.github/.scripts/sync_models.py by path, but the Unit Tests jobs check out
only a sparse subset of the repo, so the test errors with FileNotFoundError.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01BQxMkhaaPtbrf7CumWNvNm

---------

Co-authored-by: Suparva <suparva@outlook.in>
Co-authored-by: Claude <noreply@anthropic.com>")[#16902](https://github.com/Arize-ai/phoenix/pull/16902)[)](https://github.com/Arize-ai/phoenix/commit/d927595cb8ee91aacdb7af8fff3fca1d095cf2eb "fix: stop dropping evaluator metadata nested under `result` \(#16902\)

* fix\(client\): send evaluator metadata at top level of evaluation request

* fix: stop dropping evaluator metadata nested under `result`

`POST /v1/experiment_evaluations` reads `metadata` only from the top level
and its `result` model ignores unknown keys, so evaluator metadata nested
under `result` was silently discarded. The JS client \(fixed in #16895\) and
the Python client's `run_experiment`/`evaluate_experiment` both sent it
there.

Why it went unnoticed:
- openapi-fetch infers the `client.POST` body type from the argument, so
  TypeScript skips excess-property checks and stray keys compile.
- Python's `ExperimentEvaluation` subclasses the wire `result` type and adds
  `metadata`, and the body was built from `eval_run.__dict__`.
- The server test posted nested metadata and asserted it was stored as `{}`.

Changes:
- Server: recover `result.metadata` from the raw payload \(top-level keys
  win\), so already-released clients stop losing metadata.
- Python client: build the request body field by field, with metadata at
  the top level.
- JS client: route all three evaluation POSTs through
  `toExperimentEvaluationRequestBody`, which returns the declared request
  type, so a nested `metadata` key is now a compile error.
- Tests: server unit test now asserts metadata round-trips, and a new test
  covers the legacy fallback. Adds an end-to-end integration test \(sync and
  async\) plus client unit tests for both bodies.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01BQxMkhaaPtbrf7CumWNvNm

* ci: include sync_models.py in the unit test sparse checkout

tests/unit/server/cost_tracking/test_sync_models_mistral.py \(#16880\) loads
.github/.scripts/sync_models.py by path, but the Unit Tests jobs check out
only a sparse subset of the repo, so the test errors with FileNotFoundError.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01BQxMkhaaPtbrf7CumWNvNm

---------

Co-authored-by: Suparva <suparva@outlook.in>
Co-authored-by: Claude <noreply@anthropic.com>")  | Oct 9, 2026  |  
|   |   |  [test(plugin): add phoenix-cli evals for agent troubleshooting (](https://github.com/Arize-ai/phoenix/commit/b8456cc5c25c86a4f4a331cb735df8dc315f6027 "test\(plugin\): add phoenix-cli evals for agent troubleshooting \(#16223\)

* chore\(plugin\): ignore claude plugin eval results

* docs\(plugin\): link plugin eval docs from README

* chore: plugin evals

* test\(plugin\): rewrite phoenix-cli evals around agent troubleshooting

Replace the command-recitation cases with six troubleshooting cases that
mirror how users actually use the skill: pasted `px trace get` output for a
hallucinated SQL parameter, a 429 rate-limit cascade, a system-prompt
escalation rule, and a skill that never loaded; a symptom-only "too many
turns" investigation; and a tool-audit script. Keep the Elixir Phoenix
negative and swap the OTel Collector negative for a LangGraph loop with no
Phoenix mention.

Graders check span-content reasoning and real `px` commands, and catch
invented flags and commands. LLM rubrics are split into one grader per
claim: the runner's judge returns a bare verdict and failed correct answers
on combined rubrics. `tool_used` graders are scoped with `input_match` so
host-level skills leaking into the sandbox don't count.

Baseline \(skill 3.5.0, 3 runs\): mean delta +0.25 on fire cases, 0.00 on
negatives. evals/README.md documents the run command, including the
`--allow-tools` grants for Write and for reading the skill's references.

* style\(plugin\): format python block in eval prompt

* ci\(plugin\): run claude plugin evals on skill and plugin changes

Runs the arize-phoenix plugin eval suite on PRs touching the phoenix-cli
skill or the plugin, gated on the with-plugin score \(--threshold 0.8\) and
on mean uplift \(meanDelta >= 0.10\). Pinned agent and judge models, cost
ceiling of $25, report uploaded as an artifact. Skipped on forks since it
needs ANTHROPIC_API_KEY.

* ci\(plugin\): trigger on all three plugin skills, gate Δ on fire cases at 0.15

The plugin ships phoenix-cli, phoenix-evals and phoenix-tracing, so a change
to any of them should run the suite. The Δ gate now averages only the fire
cases \(negatives are 0 by design\) and requires 0.15 against a baseline of
about 0.25.

* test\(plugin\): stop treating --format json as an invented flag in case 06

--format json is a real option on px trace get and px span list. The
raw preference is already enforced positively by format-raw.md, so
penalizing json on two more graders double-counted one choice.")[#16223](https://github.com/Arize-ai/phoenix/pull/16223)[)](https://github.com/Arize-ai/phoenix/commit/b8456cc5c25c86a4f4a331cb735df8dc315f6027 "test\(plugin\): add phoenix-cli evals for agent troubleshooting \(#16223\)

* chore\(plugin\): ignore claude plugin eval results

* docs\(plugin\): link plugin eval docs from README

* chore: plugin evals

* test\(plugin\): rewrite phoenix-cli evals around agent troubleshooting

Replace the command-recitation cases with six troubleshooting cases that
mirror how users actually use the skill: pasted `px trace get` output for a
hallucinated SQL parameter, a 429 rate-limit cascade, a system-prompt
escalation rule, and a skill that never loaded; a symptom-only "too many
turns" investigation; and a tool-audit script. Keep the Elixir Phoenix
negative and swap the OTel Collector negative for a LangGraph loop with no
Phoenix mention.

Graders check span-content reasoning and real `px` commands, and catch
invented flags and commands. LLM rubrics are split into one grader per
claim: the runner's judge returns a bare verdict and failed correct answers
on combined rubrics. `tool_used` graders are scoped with `input_match` so
host-level skills leaking into the sandbox don't count.

Baseline \(skill 3.5.0, 3 runs\): mean delta +0.25 on fire cases, 0.00 on
negatives. evals/README.md documents the run command, including the
`--allow-tools` grants for Write and for reading the skill's references.

* style\(plugin\): format python block in eval prompt

* ci\(plugin\): run claude plugin evals on skill and plugin changes

Runs the arize-phoenix plugin eval suite on PRs touching the phoenix-cli
skill or the plugin, gated on the with-plugin score \(--threshold 0.8\) and
on mean uplift \(meanDelta >= 0.10\). Pinned agent and judge models, cost
ceiling of $25, report uploaded as an artifact. Skipped on forks since it
needs ANTHROPIC_API_KEY.

* ci\(plugin\): trigger on all three plugin skills, gate Δ on fire cases at 0.15

The plugin ships phoenix-cli, phoenix-evals and phoenix-tracing, so a change
to any of them should run the suite. The Δ gate now averages only the fire
cases \(negatives are 0 by design\) and requires 0.15 against a baseline of
about 0.25.

* test\(plugin\): stop treating --format json as an invented flag in case 06

--format json is a real option on px trace get and px span list. The
raw preference is already enforced positively by format-raw.md, so
penalizing json on two more graders double-counted one choice.")  | Oct 6, 2026  |  
|   |   |  [feat(evals): completeness evaluator (](https://github.com/Arize-ai/phoenix/commit/63805d0f13eb3c2cf5b55378abd542ca505e0d9a "feat\(evals\): completeness evaluator \(#15767\)")[#15767](https://github.com/Arize-ai/phoenix/pull/15767)[)](https://github.com/Arize-ai/phoenix/commit/63805d0f13eb3c2cf5b55378abd542ca505e0d9a "feat\(evals\): completeness evaluator \(#15767\)")  | Sep 9, 2026  |  
|   |   |  [fix: Remove SciPy and scikit-learn from runtime dependencies (](https://github.com/Arize-ai/phoenix/commit/ee5ca59c9baae7deaf4a9e28555302566b8095ea "fix: Remove SciPy and scikit-learn from runtime dependencies \(#16795\)

Compute retrieval NDCG with NumPy while preserving existing behavior.
Keep scikit-learn as a development dependency for reference tests and
remove the unused version lookup.")[#16795](https://github.com/Arize-ai/phoenix/pull/16795)[)](https://github.com/Arize-ai/phoenix/commit/ee5ca59c9baae7deaf4a9e28555302566b8095ea "fix: Remove SciPy and scikit-learn from runtime dependencies \(#16795\)

Compute retrieval NDCG with NumPy while preserving existing behavior.
Keep scikit-learn as a development dependency for reference tests and
remove the unused version lookup.")  | Oct 6, 2026  |  
|   |   |  [feat(tracing): add the DECISION span kind across the platform (](https://github.com/Arize-ai/phoenix/commit/43d1a54406835f452fa44fb3becbb91f778d077a "feat\(tracing\): add the DECISION span kind across the platform \(#16657\)

* feat\(tracing\): add the DECISION span kind across the platform

Mirrors the new OpenInference DECISION span kind \(Python semconv 0.1.40,
TypeScript semconv 2.13.0\) through Phoenix: the internal and GraphQL
SpanKind enums, REST and CLI filter docs, the regenerated GraphQL and
OpenAPI schemas, PXI prompt text, docs, and the tracing/CLI/evals skills.

The UI gets a decision icon \(ring and center dot\) in the orange family
beside LLM and Prompt, plus Storybook entries for the icon, span info,
preview tooltip, and trace tree. Also fixes the tool outline icon, whose
mask used currentColor and so rendered washed out in dark mode.

Co-Authored-By: Claude <noreply@anthropic.com>

* docs\(tracing\): mark the DECISION span kind as experimental

Phoenix recognizes the kind and renders its icon, but decision-model
support is still being built out and no provider is fully supported yet.

Co-Authored-By: Claude <noreply@anthropic.com>

* chore\(stories\): keep the decision span details example empty

Decision spans are experimental with no attribute conventions yet, so the
Span Info fixture carries only the span kind and the skill reference no
longer suggests decision-specific metadata keys.

Co-Authored-By: Claude <noreply@anthropic.com>

* chore: regenerate OpenAPI TypeScript clients for the DECISION span kind

Co-Authored-By: Claude <noreply@anthropic.com>

---------

Co-authored-by: Claude <noreply@anthropic.com>")[#16657](https://github.com/Arize-ai/phoenix/pull/16657)[)](https://github.com/Arize-ai/phoenix/commit/43d1a54406835f452fa44fb3becbb91f778d077a "feat\(tracing\): add the DECISION span kind across the platform \(#16657\)

* feat\(tracing\): add the DECISION span kind across the platform

Mirrors the new OpenInference DECISION span kind \(Python semconv 0.1.40,
TypeScript semconv 2.13.0\) through Phoenix: the internal and GraphQL
SpanKind enums, REST and CLI filter docs, the regenerated GraphQL and
OpenAPI schemas, PXI prompt text, docs, and the tracing/CLI/evals skills.

The UI gets a decision icon \(ring and center dot\) in the orange family
beside LLM and Prompt, plus Storybook entries for the icon, span info,
preview tooltip, and trace tree. Also fixes the tool outline icon, whose
mask used currentColor and so rendered washed out in dark mode.

Co-Authored-By: Claude <noreply@anthropic.com>

* docs\(tracing\): mark the DECISION span kind as experimental

Phoenix recognizes the kind and renders its icon, but decision-model
support is still being built out and no provider is fully supported yet.

Co-Authored-By: Claude <noreply@anthropic.com>

* chore\(stories\): keep the decision span details example empty

Decision spans are experimental with no attribute conventions yet, so the
Span Info fixture carries only the span kind and the skill reference no
longer suggests decision-specific metadata keys.

Co-Authored-By: Claude <noreply@anthropic.com>

* chore: regenerate OpenAPI TypeScript clients for the DECISION span kind

Co-Authored-By: Claude <noreply@anthropic.com>

---------

Co-authored-by: Claude <noreply@anthropic.com>")  | Oct 1, 2026  |  
|   |   | [refactor(cost-tracking): move model price sync out of .github and dro…](https://github.com/Arize-ai/phoenix/commit/e4713156c6c2eada91d691834954e9d27cae151e "refactor\(cost-tracking\): move model price sync out of .github and drop its tests \(#16908\)

* refactor\(cost-tracking\): move model price sync out of .github so unit tests stop needing it

The Mistral pricing tests loaded .github/.scripts/sync_models.py by path, which
the Unit Tests sparse checkout omits, so CI needed a stopgap checkout entry.

- Move the sync script to scripts/cost_tracking/ with its tests beside it, run
  by `make test-sync-models` and a new Test Model Cost Sync Script CI job.
- Keep the runtime Mistral cost-calculation tests in tests/unit.
- Drop the stopgap .github/.scripts/sync_models.py sparse-checkout entries.
- Add `make check-unit-test-paths` and a CI job that fails when a unit test
  reads a repo path outside the Unit Tests sparse checkout, or when the two
  Unit Tests jobs' checkout lists drift apart.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01QLcWow97hFyWXWxKyuqdEx

* refactor\(cost-tracking\): run sync-script tests in the cost sync workflow, drop path guard

- Run make test-sync-models \(mypy --strict + pytest, lean env\) right before
  the weekday sync instead of in a separate PR CI job
- Remove the static unit-test path guard; the sparse-checkout Unit Tests job
  already fails on a missing path
- Drop test_mistral_cost_calculation.py, which duplicated cases in
  test_cost_details_calculator.py

* refactor\(cost-tracking\): drop sync-script tests, keep a pure move

The Mistral sync tests and the checked-in-manifest schema test duplicate what
the weekday sync and test_model_cost_manifest.py already cover.

---------

Co-authored-by: Claude <noreply@anthropic.com>")  | Oct 9, 2026  |  
| [src/phoenix](https://github.com/Arize-ai/phoenix/tree/main/src/phoenix "This path skips through empty directories")  | [src/phoenix](https://github.com/Arize-ai/phoenix/tree/main/src/phoenix "This path skips through empty directories")  |  [fix: stop dropping evaluator metadata nested under](https://github.com/Arize-ai/phoenix/commit/d927595cb8ee91aacdb7af8fff3fca1d095cf2eb "fix: stop dropping evaluator metadata nested under `result` \(#16902\)

* fix\(client\): send evaluator metadata at top level of evaluation request

* fix: stop dropping evaluator metadata nested under `result`

`POST /v1/experiment_evaluations` reads `metadata` only from the top level
and its `result` model ignores unknown keys, so evaluator metadata nested
under `result` was silently discarded. The JS client \(fixed in #16895\) and
the Python client's `run_experiment`/`evaluate_experiment` both sent it
there.

Why it went unnoticed:
- openapi-fetch infers the `client.POST` body type from the argument, so
  TypeScript skips excess-property checks and stray keys compile.
- Python's `ExperimentEvaluation` subclasses the wire `result` type and adds
  `metadata`, and the body was built from `eval_run.__dict__`.
- The server test posted nested metadata and asserted it was stored as `{}`.

Changes:
- Server: recover `result.metadata` from the raw payload \(top-level keys
  win\), so already-released clients stop losing metadata.
- Python client: build the request body field by field, with metadata at
  the top level.
- JS client: route all three evaluation POSTs through
  `toExperimentEvaluationRequestBody`, which returns the declared request
  type, so a nested `metadata` key is now a compile error.
- Tests: server unit test now asserts metadata round-trips, and a new test
  covers the legacy fallback. Adds an end-to-end integration test \(sync and
  async\) plus client unit tests for both bodies.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01BQxMkhaaPtbrf7CumWNvNm

* ci: include sync_models.py in the unit test sparse checkout

tests/unit/server/cost_tracking/test_sync_models_mistral.py \(#16880\) loads
.github/.scripts/sync_models.py by path, but the Unit Tests jobs check out
only a sparse subset of the repo, so the test errors with FileNotFoundError.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01BQxMkhaaPtbrf7CumWNvNm

---------

Co-authored-by: Suparva <suparva@outlook.in>
Co-authored-by: Claude <noreply@anthropic.com>") [(](https://github.com/Arize-ai/phoenix/commit/d927595cb8ee91aacdb7af8fff3fca1d095cf2eb "fix: stop dropping evaluator metadata nested under `result` \(#16902\)

* fix\(client\): send evaluator metadata at top level of evaluation request

* fix: stop dropping evaluator metadata nested under `result`

`POST /v1/experiment_evaluations` reads `metadata` only from the top level
and its `result` model ignores unknown keys, so evaluator metadata nested
under `result` was silently discarded. The JS client \(fixed in #16895\) and
the Python client's `run_experiment`/`evaluate_experiment` both sent it
there.

Why it went unnoticed:
- openapi-fetch infers the `client.POST` body type from the argument, so
  TypeScript skips excess-property checks and stray keys compile.
- Python's `ExperimentEvaluation` subclasses the wire `result` type and adds
  `metadata`, and the body was built from `eval_run.__dict__`.
- The server test posted nested metadata and asserted it was stored as `{}`.

Changes:
- Server: recover `result.metadata` from the raw payload \(top-level keys
  win\), so already-released clients stop losing metadata.
- Python client: build the request body field by field, with metadata at
  the top level.
- JS client: route all three evaluation POSTs through
  `toExperimentEvaluationRequestBody`, which returns the declared request
  type, so a nested `metadata` key is now a compile error.
- Tests: server unit test now asserts metadata round-trips, and a new test
  covers the legacy fallback. Adds an end-to-end integration test \(sync and
  async\) plus client unit tests for both bodies.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01BQxMkhaaPtbrf7CumWNvNm

* ci: include sync_models.py in the unit test sparse checkout

tests/unit/server/cost_tracking/test_sync_models_mistral.py \(#16880\) loads
.github/.scripts/sync_models.py by path, but the Unit Tests jobs check out
only a sparse subset of the repo, so the test errors with FileNotFoundError.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01BQxMkhaaPtbrf7CumWNvNm

---------

Co-authored-by: Suparva <suparva@outlook.in>
Co-authored-by: Claude <noreply@anthropic.com>")[#16902](https://github.com/Arize-ai/phoenix/pull/16902)[)](https://github.com/Arize-ai/phoenix/commit/d927595cb8ee91aacdb7af8fff3fca1d095cf2eb "fix: stop dropping evaluator metadata nested under `result` \(#16902\)

* fix\(client\): send evaluator metadata at top level of evaluation request

* fix: stop dropping evaluator metadata nested under `result`

`POST /v1/experiment_evaluations` reads `metadata` only from the top level
and its `result` model ignores unknown keys, so evaluator metadata nested
under `result` was silently discarded. The JS client \(fixed in #16895\) and
the Python client's `run_experiment`/`evaluate_experiment` both sent it
there.

Why it went unnoticed:
- openapi-fetch infers the `client.POST` body type from the argument, so
  TypeScript skips excess-property checks and stray keys compile.
- Python's `ExperimentEvaluation` subclasses the wire `result` type and adds
  `metadata`, and the body was built from `eval_run.__dict__`.
- The server test posted nested metadata and asserted it was stored as `{}`.

Changes:
- Server: recover `result.metadata` from the raw payload \(top-level keys
  win\), so already-released clients stop losing metadata.
- Python client: build the request body field by field, with metadata at
  the top level.
- JS client: route all three evaluation POSTs through
  `toExperimentEvaluationRequestBody`, which returns the declared request
  type, so a nested `metadata` key is now a compile error.
- Tests: server unit test now asserts metadata round-trips, and a new test
  covers the legacy fallback. Adds an end-to-end integration test \(sync and
  async\) plus client unit tests for both bodies.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01BQxMkhaaPtbrf7CumWNvNm

* ci: include sync_models.py in the unit test sparse checkout

tests/unit/server/cost_tracking/test_sync_models_mistral.py \(#16880\) loads
.github/.scripts/sync_models.py by path, but the Unit Tests jobs check out
only a sparse subset of the repo, so the test errors with FileNotFoundError.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01BQxMkhaaPtbrf7CumWNvNm

---------

Co-authored-by: Suparva <suparva@outlook.in>
Co-authored-by: Claude <noreply@anthropic.com>")  | Oct 9, 2026  |  
| [styles/config/vocabularies/Mintlify](https://github.com/Arize-ai/phoenix/tree/main/styles/config/vocabularies/Mintlify "This path skips through empty directories")  | [styles/config/vocabularies/Mintlify](https://github.com/Arize-ai/phoenix/tree/main/styles/config/vocabularies/Mintlify "This path skips through empty directories")  |  [docs: Adding words to acceptable spelling (](https://github.com/Arize-ai/phoenix/commit/1135a3d380ac929a8db0e93e691c5bb1a1c956c2 "docs: Adding words to acceptable spelling \(#12501\)

* Adding spelling

* Adding styles path")[#12501](https://github.com/Arize-ai/phoenix/pull/12501)[)](https://github.com/Arize-ai/phoenix/commit/1135a3d380ac929a8db0e93e691c5bb1a1c956c2 "docs: Adding words to acceptable spelling \(#12501\)

* Adding spelling

* Adding styles path")  | Apr 6, 2026  |  
|   |   | [refactor(cost-tracking): move model price sync out of .github and dro…](https://github.com/Arize-ai/phoenix/commit/e4713156c6c2eada91d691834954e9d27cae151e "refactor\(cost-tracking\): move model price sync out of .github and drop its tests \(#16908\)

* refactor\(cost-tracking\): move model price sync out of .github so unit tests stop needing it

The Mistral pricing tests loaded .github/.scripts/sync_models.py by path, which
the Unit Tests sparse checkout omits, so CI needed a stopgap checkout entry.

- Move the sync script to scripts/cost_tracking/ with its tests beside it, run
  by `make test-sync-models` and a new Test Model Cost Sync Script CI job.
- Keep the runtime Mistral cost-calculation tests in tests/unit.
- Drop the stopgap .github/.scripts/sync_models.py sparse-checkout entries.
- Add `make check-unit-test-paths` and a CI job that fails when a unit test
  reads a repo path outside the Unit Tests sparse checkout, or when the two
  Unit Tests jobs' checkout lists drift apart.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01QLcWow97hFyWXWxKyuqdEx

* refactor\(cost-tracking\): run sync-script tests in the cost sync workflow, drop path guard

- Run make test-sync-models \(mypy --strict + pytest, lean env\) right before
  the weekday sync instead of in a separate PR CI job
- Remove the static unit-test path guard; the sparse-checkout Unit Tests job
  already fails on a missing path
- Drop test_mistral_cost_calculation.py, which duplicated cases in
  test_cost_details_calculator.py

* refactor\(cost-tracking\): drop sync-script tests, keep a pure move

The Mistral sync tests and the checked-in-manifest schema test duplicate what
the weekday sync and test_model_cost_manifest.py already cover.

---------

Co-authored-by: Claude <noreply@anthropic.com>")  | Oct 9, 2026  |  
|   |   |  [chore(deps): bump strawberry-graphql (](https://github.com/Arize-ai/phoenix/commit/9d9a5a18df05220eddae06917dfe89103dbaff08 "chore\(deps\): bump strawberry-graphql \(#16899\)

Bumps \[strawberry-graphql\]\(https://github.com/sponsors/strawberry-graphql\) from 0.315.7 to 0.327.2.
- \[Commits\]\(https://github.com/sponsors/strawberry-graphql/commits\)

---
updated-dependencies:
- dependency-name: strawberry-graphql
  dependency-version: 0.327.2
  dependency-type: direct:production
...

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>")[#16899](https://github.com/Arize-ai/phoenix/pull/16899)[)](https://github.com/Arize-ai/phoenix/commit/9d9a5a18df05220eddae06917dfe89103dbaff08 "chore\(deps\): bump strawberry-graphql \(#16899\)

Bumps \[strawberry-graphql\]\(https://github.com/sponsors/strawberry-graphql\) from 0.315.7 to 0.327.2.
- \[Commits\]\(https://github.com/sponsors/strawberry-graphql/commits\)

---
updated-dependencies:
- dependency-name: strawberry-graphql
  dependency-version: 0.327.2
  dependency-type: direct:production
...

Signed-off-by: dependabot\[bot\] <support@github.com>
Co-authored-by: dependabot\[bot\] <49699333+dependabot\[bot\]@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   |  [feat: change docker base image to distroless (](https://github.com/Arize-ai/phoenix/commit/89d6fe7bfba0f8cc4feb791f6018cbd59a13a640 "feat: change docker base image to distroless \(#2708\)")[#2708](https://github.com/Arize-ai/phoenix/pull/2708)[)](https://github.com/Arize-ai/phoenix/commit/89d6fe7bfba0f8cc4feb791f6018cbd59a13a640 "feat: change docker base image to distroless \(#2708\)")  | Mar 28, 2024  |  
|   |   |  [chore: remove](https://github.com/Arize-ai/phoenix/commit/43c7f23b060fbd4061a38dc2b7b2fca77500fc70 "chore: remove `SpanEvaluations` from tutorials \(#9247\)

* chore: update cursor rule

* update notebooks

* clen up

* clean up

* more notebooks

* more notebooks

* more notebooks

* clean up

* more notebooks

* clean up

* more notebooks") [from tutorials (](https://github.com/Arize-ai/phoenix/commit/43c7f23b060fbd4061a38dc2b7b2fca77500fc70 "chore: remove `SpanEvaluations` from tutorials \(#9247\)

* chore: update cursor rule

* update notebooks

* clen up

* clean up

* more notebooks

* more notebooks

* more notebooks

* clean up

* more notebooks

* clean up

* more notebooks")[#9247](https://github.com/Arize-ai/phoenix/pull/9247)[)](https://github.com/Arize-ai/phoenix/commit/43c7f23b060fbd4061a38dc2b7b2fca77500fc70 "chore: remove `SpanEvaluations` from tutorials \(#9247\)

* chore: update cursor rule

* update notebooks

* clen up

* clean up

* more notebooks

* more notebooks

* more notebooks

* clean up

* more notebooks

* clean up

* more notebooks")  | Aug 25, 2025  |  
|   |   |  [chore: increase look back period for customer issues needing triage (](https://github.com/Arize-ai/phoenix/commit/f2bd3a7202427ee9dab96712120ba43b2efa329f "chore: increase look back period for customer issues needing triage \(#5295\)")[#…](https://github.com/Arize-ai/phoenix/pull/5295)  | Nov 8, 2024  |  
|   |   | [chore: Ignore formatting changes in git blame](https://github.com/Arize-ai/phoenix/commit/00ed4f2a0422a0096b26a70613ad6327dbbac507 "chore: Ignore formatting changes in git blame")  | Feb 18, 2026  |  
|   |   |  [test(plugin): add phoenix-cli evals for agent troubleshooting (](https://github.com/Arize-ai/phoenix/commit/b8456cc5c25c86a4f4a331cb735df8dc315f6027 "test\(plugin\): add phoenix-cli evals for agent troubleshooting \(#16223\)

* chore\(plugin\): ignore claude plugin eval results

* docs\(plugin\): link plugin eval docs from README

* chore: plugin evals

* test\(plugin\): rewrite phoenix-cli evals around agent troubleshooting

Replace the command-recitation cases with six troubleshooting cases that
mirror how users actually use the skill: pasted `px trace get` output for a
hallucinated SQL parameter, a 429 rate-limit cascade, a system-prompt
escalation rule, and a skill that never loaded; a symptom-only "too many
turns" investigation; and a tool-audit script. Keep the Elixir Phoenix
negative and swap the OTel Collector negative for a LangGraph loop with no
Phoenix mention.

Graders check span-content reasoning and real `px` commands, and catch
invented flags and commands. LLM rubrics are split into one grader per
claim: the runner's judge returns a bare verdict and failed correct answers
on combined rubrics. `tool_used` graders are scoped with `input_match` so
host-level skills leaking into the sandbox don't count.

Baseline \(skill 3.5.0, 3 runs\): mean delta +0.25 on fire cases, 0.00 on
negatives. evals/README.md documents the run command, including the
`--allow-tools` grants for Write and for reading the skill's references.

* style\(plugin\): format python block in eval prompt

* ci\(plugin\): run claude plugin evals on skill and plugin changes

Runs the arize-phoenix plugin eval suite on PRs touching the phoenix-cli
skill or the plugin, gated on the with-plugin score \(--threshold 0.8\) and
on mean uplift \(meanDelta >= 0.10\). Pinned agent and judge models, cost
ceiling of $25, report uploaded as an artifact. Skipped on forks since it
needs ANTHROPIC_API_KEY.

* ci\(plugin\): trigger on all three plugin skills, gate Δ on fire cases at 0.15

The plugin ships phoenix-cli, phoenix-evals and phoenix-tracing, so a change
to any of them should run the suite. The Δ gate now averages only the fire
cases \(negatives are 0 by design\) and requires 0.15 against a baseline of
about 0.25.

* test\(plugin\): stop treating --format json as an invented flag in case 06

--format json is a real option on px trace get and px span list. The
raw preference is already enforced positively by format-raw.md, so
penalizing json on two more graders double-counted one choice.")[#16223](https://github.com/Arize-ai/phoenix/pull/16223)[)](https://github.com/Arize-ai/phoenix/commit/b8456cc5c25c86a4f4a331cb735df8dc315f6027 "test\(plugin\): add phoenix-cli evals for agent troubleshooting \(#16223\)

* chore\(plugin\): ignore claude plugin eval results

* docs\(plugin\): link plugin eval docs from README

* chore: plugin evals

* test\(plugin\): rewrite phoenix-cli evals around agent troubleshooting

Replace the command-recitation cases with six troubleshooting cases that
mirror how users actually use the skill: pasted `px trace get` output for a
hallucinated SQL parameter, a 429 rate-limit cascade, a system-prompt
escalation rule, and a skill that never loaded; a symptom-only "too many
turns" investigation; and a tool-audit script. Keep the Elixir Phoenix
negative and swap the OTel Collector negative for a LangGraph loop with no
Phoenix mention.

Graders check span-content reasoning and real `px` commands, and catch
invented flags and commands. LLM rubrics are split into one grader per
claim: the runner's judge returns a bare verdict and failed correct answers
on combined rubrics. `tool_used` graders are scoped with `input_match` so
host-level skills leaking into the sandbox don't count.

Baseline \(skill 3.5.0, 3 runs\): mean delta +0.25 on fire cases, 0.00 on
negatives. evals/README.md documents the run command, including the
`--allow-tools` grants for Write and for reading the skill's references.

* style\(plugin\): format python block in eval prompt

* ci\(plugin\): run claude plugin evals on skill and plugin changes

Runs the arize-phoenix plugin eval suite on PRs touching the phoenix-cli
skill or the plugin, gated on the with-plugin score \(--threshold 0.8\) and
on mean uplift \(meanDelta >= 0.10\). Pinned agent and judge models, cost
ceiling of $25, report uploaded as an artifact. Skipped on forks since it
needs ANTHROPIC_API_KEY.

* ci\(plugin\): trigger on all three plugin skills, gate Δ on fire cases at 0.15

The plugin ships phoenix-cli, phoenix-evals and phoenix-tracing, so a change
to any of them should run the suite. The Δ gate now averages only the fire
cases \(negatives are 0 by design\) and requires 0.15 against a baseline of
about 0.25.

* test\(plugin\): stop treating --format json as an invented flag in case 06

--format json is a real option on px trace get and px span list. The
raw preference is already enforced positively by format-raw.md, so
penalizing json on two more graders double-counted one choice.")  | Oct 6, 2026  |  
|   |   |  [docs: move README translations to docs/i18n (](https://github.com/Arize-ai/phoenix/commit/3279f387d5605757a18c839cae440c8676f23c24 "docs: move README translations to docs/i18n \(#16869\)

Move the 32 README.<lang>.md translations out of the repository root into
docs/i18n/ and point the language bars at the new paths. Relative links in
the translations are rewritten relative to docs/i18n/.

Update the translation sync workflow and check_readme_translations.py for
the new location; the checker now resolves relative links against each
file's directory before comparing them with README.md. Exclude docs/i18n
from Mintlify since the translations are GitHub-only.


Claude-Session: https://claude.ai/code/session_01M2iUpHu2k54N2uABsHjZPm

Co-authored-by: Claude <noreply@anthropic.com>")[#16869](https://github.com/Arize-ai/phoenix/pull/16869)[)](https://github.com/Arize-ai/phoenix/commit/3279f387d5605757a18c839cae440c8676f23c24 "docs: move README translations to docs/i18n \(#16869\)

Move the 32 README.<lang>.md translations out of the repository root into
docs/i18n/ and point the language bars at the new paths. Relative links in
the translations are rewritten relative to docs/i18n/.

Update the translation sync workflow and check_readme_translations.py for
the new location; the checker now resolves relative links against each
file's directory before comparing them with README.md. Exclude docs/i18n
from Mintlify since the translations are GitHub-only.


Claude-Session: https://claude.ai/code/session_01M2iUpHu2k54N2uABsHjZPm

Co-authored-by: Claude <noreply@anthropic.com>")  | Oct 8, 2026  |  
|   |   | [chore: upgrade Phoenix development to Node 24](https://github.com/Arize-ai/phoenix/commit/9d757402d5ec922854c832712e6325cd00ae8970 "chore: upgrade Phoenix development to Node 24")  | Sep 8, 2026  |  
|   |   |  [chore(js): move the app into the js pnpm workspace (](https://github.com/Arize-ai/phoenix/commit/752e800acb81fcc4b27ea7bd6d4fc01df867f7af "chore\(js\): move the app into the js pnpm workspace \(#15394\)

* chore\(js\): move app/ to js/app/

Pure file move with no content changes, in preparation for merging the
app into the js pnpm workspace \(#15009\). The app's standalone
pnpm-lock.yaml, pnpm-workspace.yaml, .npmrc, and .pnpmfile.cjs are left
behind at app/ and removed in the follow-up workspace-merge commit.

* chore\(js\): merge the app into the js pnpm workspace

Closes #15009

- js/pnpm-workspace.yaml: add app to packages; merge the app's
  packageExtensions \(@browser-ai/core\), allowBuilds, and widen
  minimumReleaseAgeExclude to @arizeai/*; delete the app's standalone
  pnpm-lock.yaml, pnpm-workspace.yaml, .npmrc, and .pnpmfile.cjs and
  regenerate the single js/pnpm-lock.yaml
- js/.pnpmfile.cjs: merge the app's TS6-bridge entries \(storybook docgen,
  react-docgen-typescript\)
- app: switch @arizeai/phoenix-client and @arizeai/phoenix-evals to
  workspace:*; add build:deps \(pnpm --filter 'phoenix-ui^...' run build\);
  fix repo-relative paths \(vite outDir, build:static, generate:openapi,
  oxfmt/oxlint configs, codegen scripts\); drop packageManager \(owned by
  the workspace root\)
- app tsconfig: lib ES2022 -> ES2023; the app uses Array#findLast and
  previously typechecked only because transitive type-fest@0.20.2 pulled
  in lib.esnext, which the fresh workspace resolution no longer does
- js root scripts: exclude phoenix-ui from recursive build/test/typecheck
  \(covered by its own CI\) and give lint explicit paths \(a nested
  typeAware config aborts bare oxlint runs from the workspace root\)
- Makefile: APP_DIR := js/app, single pnpm install plus app dep build,
  filtered build-frontend/build-ts/test-ts/typecheck-ts
- Dockerfile + devops harness: copy js/ and schemas/ and build via
  pnpm --filter 'phoenix-ui...' run build \(topological, deps first\)
- workflows: typescript-CI installs at js/ and builds workspace deps
  before typecheck; typescript-packages-CI filters js/!\(app\)/**;
  playwright/publish/claude-weekly-deps-upgrade updated for the new
  paths; delete package-version-check.yml \(obsolete with one workspace\)
- .gitignore: scope /js/**/docs to typedoc output so it does not swallow
  js/app/src/agent/tools/docs/

* docs: update path references for the app's move to js/app

Skills, cursor rules, DEVELOPMENT/CONTRIBUTING guides, internal docs,
READMEs, and code comments that referenced the old app/ location.
SHA-pinned GitHub permalinks in internal_docs/specs are intentionally
left unchanged since they point at historical trees.

* ci: unify TypeScript CI over the whole js workspace

Now that the app and packages share one pnpm workspace, run one
typescript-CI job instead of separate app and packages jobs:

- js root scripts: build/test/typecheck are unfiltered \(recursive over
  the whole workspace; topological ordering builds the app's workspace
  deps before the app\); lint delegates to the app's own script because
  oxlint only allows options.typeAware in the cwd-root config;
  ci:publish narrows to --filter './packages/*' since a publish run
  only needs the publishable packages built
- typescript-CI.yml: single job covering build \(+codegen-drift check,
  which now also subsumes the app relay drift check since the app build
  runs build:relay\), typecheck, fmt, lint, app OpenAPI codegen,
  generative-UI schema, UI-message fixtures, and tests; delete
  typescript-packages-CI.yml
- Makefile: aggregate build/test/typecheck/format/lint call the unified
  js scripts once; the -frontend targets remain as scoped conveniences
- claude-weekly-deps-upgrade: drop the now-redundant app-* verify steps

The 'TypeScript Packages CI Required' status check should be removed
from branch protection.")[#15394](https://github.com/Arize-ai/phoenix/pull/15394)[)](https://github.com/Arize-ai/phoenix/commit/752e800acb81fcc4b27ea7bd6d4fc01df867f7af "chore\(js\): move the app into the js pnpm workspace \(#15394\)

* chore\(js\): move app/ to js/app/

Pure file move with no content changes, in preparation for merging the
app into the js pnpm workspace \(#15009\). The app's standalone
pnpm-lock.yaml, pnpm-workspace.yaml, .npmrc, and .pnpmfile.cjs are left
behind at app/ and removed in the follow-up workspace-merge commit.

* chore\(js\): merge the app into the js pnpm workspace

Closes #15009

- js/pnpm-workspace.yaml: add app to packages; merge the app's
  packageExtensions \(@browser-ai/core\), allowBuilds, and widen
  minimumReleaseAgeExclude to @arizeai/*; delete the app's standalone
  pnpm-lock.yaml, pnpm-workspace.yaml, .npmrc, and .pnpmfile.cjs and
  regenerate the single js/pnpm-lock.yaml
- js/.pnpmfile.cjs: merge the app's TS6-bridge entries \(storybook docgen,
  react-docgen-typescript\)
- app: switch @arizeai/phoenix-client and @arizeai/phoenix-evals to
  workspace:*; add build:deps \(pnpm --filter 'phoenix-ui^...' run build\);
  fix repo-relative paths \(vite outDir, build:static, generate:openapi,
  oxfmt/oxlint configs, codegen scripts\); drop packageManager \(owned by
  the workspace root\)
- app tsconfig: lib ES2022 -> ES2023; the app uses Array#findLast and
  previously typechecked only because transitive type-fest@0.20.2 pulled
  in lib.esnext, which the fresh workspace resolution no longer does
- js root scripts: exclude phoenix-ui from recursive build/test/typecheck
  \(covered by its own CI\) and give lint explicit paths \(a nested
  typeAware config aborts bare oxlint runs from the workspace root\)
- Makefile: APP_DIR := js/app, single pnpm install plus app dep build,
  filtered build-frontend/build-ts/test-ts/typecheck-ts
- Dockerfile + devops harness: copy js/ and schemas/ and build via
  pnpm --filter 'phoenix-ui...' run build \(topological, deps first\)
- workflows: typescript-CI installs at js/ and builds workspace deps
  before typecheck; typescript-packages-CI filters js/!\(app\)/**;
  playwright/publish/claude-weekly-deps-upgrade updated for the new
  paths; delete package-version-check.yml \(obsolete with one workspace\)
- .gitignore: scope /js/**/docs to typedoc output so it does not swallow
  js/app/src/agent/tools/docs/

* docs: update path references for the app's move to js/app

Skills, cursor rules, DEVELOPMENT/CONTRIBUTING guides, internal docs,
READMEs, and code comments that referenced the old app/ location.
SHA-pinned GitHub permalinks in internal_docs/specs are intentionally
left unchanged since they point at historical trees.

* ci: unify TypeScript CI over the whole js workspace

Now that the app and packages share one pnpm workspace, run one
typescript-CI job instead of separate app and packages jobs:

- js root scripts: build/test/typecheck are unfiltered \(recursive over
  the whole workspace; topological ordering builds the app's workspace
  deps before the app\); lint delegates to the app's own script because
  oxlint only allows options.typeAware in the cwd-root config;
  ci:publish narrows to --filter './packages/*' since a publish run
  only needs the publishable packages built
- typescript-CI.yml: single job covering build \(+codegen-drift check,
  which now also subsumes the app relay drift check since the app build
  runs build:relay\), typecheck, fmt, lint, app OpenAPI codegen,
  generative-UI schema, UI-message fixtures, and tests; delete
  typescript-packages-CI.yml
- Makefile: aggregate build/test/typecheck/format/lint call the unified
  js scripts once; the -frontend targets remain as scoped conveniences
- claude-weekly-deps-upgrade: drop the now-redundant app-* verify steps

The 'TypeScript Packages CI Required' status check should be removed
from branch protection.")  | Aug 14, 2026  |  
|   |   |  [chore(js): enable complexity lint rule (](https://github.com/Arize-ai/phoenix/commit/a9d04b6917743a045ce3d95b6566d4fd3de4c1d7 "chore\(js\): enable complexity lint rule \(#15637\)

* chore\(js\): enable complexity lint rule

* refactor\(js\): address complexity violations

* refactor\(js\): clarify helper call sites

---------

Co-authored-by: Mikyo King <mikeldking@gmail.com>")[#15637](https://github.com/Arize-ai/phoenix/pull/15637)[)](https://github.com/Arize-ai/phoenix/commit/a9d04b6917743a045ce3d95b6566d4fd3de4c1d7 "chore\(js\): enable complexity lint rule \(#15637\)

* chore\(js\): enable complexity lint rule

* refactor\(js\): address complexity violations

* refactor\(js\): clarify helper call sites

---------

Co-authored-by: Mikyo King <mikeldking@gmail.com>")  | Sep 1, 2026  |  
| [.pre-commit-config.yaml](https://github.com/Arize-ai/phoenix/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  | [.pre-commit-config.yaml](https://github.com/Arize-ai/phoenix/blob/main/.pre-commit-config.yaml ".pre-commit-config.yaml")  |  [docs: remove Arize AX from tutorial and get-started flows (](https://github.com/Arize-ai/phoenix/commit/e75fec0399c2e99b67067f6da839c5b3c3a0ad0d "docs: remove Arize AX from tutorial and get-started flows \(#15171\)

The get-started and setup-tracing pages each carried an identical 37-line
preamble between the frontmatter and the first line of content: a visible
Note plus a hidden div of all-caps agent instructions telling readers to
verify which Arize product they wanted before writing any code. That does
not belong in the main tutorial flow, so it is removed from all five pages.

Also:
- drop the AX cookbook link from the prompt-engineering tutorial
- delete tutorials/ai_evals_course and its now-dead notebook-cleaning
  exclusions in .pre-commit-config.yaml and the Makefile
- make the Graphite integration Phoenix-only \(backend option, env config,
  usage example, troubleshooting entry, and resource links\)
- fix stale "Arize-Otel" prose in the Groq tracing tutorial, whose code
  already installed arize-phoenix and used phoenix.otel.register\(\)

arize-otel no longer appears anywhere in the repo. The migration guide,
FAQ comparisons, and feature-boundary callouts are intentionally kept.")[#15171](https://github.com/Arize-ai/phoenix/pull/15171)[)](https://github.com/Arize-ai/phoenix/commit/e75fec0399c2e99b67067f6da839c5b3c3a0ad0d "docs: remove Arize AX from tutorial and get-started flows \(#15171\)

The get-started and setup-tracing pages each carried an identical 37-line
preamble between the frontmatter and the first line of content: a visible
Note plus a hidden div of all-caps agent instructions telling readers to
verify which Arize product they wanted before writing any code. That does
not belong in the main tutorial flow, so it is removed from all five pages.

Also:
- drop the AX cookbook link from the prompt-engineering tutorial
- delete tutorials/ai_evals_course and its now-dead notebook-cleaning
  exclusions in .pre-commit-config.yaml and the Makefile
- make the Graphite integration Phoenix-only \(backend option, env config,
  usage example, troubleshooting entry, and resource links\)
- fix stale "Arize-Otel" prose in the Groq tracing tutorial, whose code
  already installed arize-phoenix and used phoenix.otel.register\(\)

arize-otel no longer appears anywhere in the repo. The migration guide,
FAQ comparisons, and feature-boundary callouts are intentionally kept.")  | Aug 6, 2026  |  
|   |   |  [chore: ignore graphql schema from formatting (](https://github.com/Arize-ai/phoenix/commit/c57b3c404fc4c56bdbe1b9a53179cc4a7fced774 "chore: ignore graphql schema from formatting \(#8166\)")[#8166](https://github.com/Arize-ai/phoenix/pull/8166)[)](https://github.com/Arize-ai/phoenix/commit/c57b3c404fc4c56bdbe1b9a53179cc4a7fced774 "chore: ignore graphql schema from formatting \(#8166\)")  | Jun 19, 2025  |  
|   |   |  [chore: use Python 3.11 as the minimum supported version (](https://github.com/Arize-ai/phoenix/commit/c94318cc820fe1434f45aa655838e1333b548440 "chore: use Python 3.11 as the minimum supported version \(#16447\)")[#16447](https://github.com/Arize-ai/phoenix/pull/16447)[)](https://github.com/Arize-ai/phoenix/commit/c94318cc820fe1434f45aa655838e1333b548440 "chore: use Python 3.11 as the minimum supported version \(#16447\)")  | Sep 24, 2026  |  
|   |   | [fix: Revert "docs: Remove API reference docs and link to subpackage g…](https://github.com/Arize-ai/phoenix/commit/57d9e4590508b78ac3ae95bb1ad60ef2228a62ce "fix: Revert "docs: Remove API reference docs and link to subpackage guides" \(#11511\)

* feat: restore api_reference

* docs: remove inferences_schema.rst reference from api_reference README

Removed inferences_schema.rst from the example directory structure in the api_reference README as part of cleaning up inference-related documentation.

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>

---------

Co-authored-by: Claude Sonnet 4.5 <noreply@anthropic.com>")  | Feb 17, 2026  |  
| [.release-please-manifest.json](https://github.com/Arize-ai/phoenix/blob/main/.release-please-manifest.json ".release-please-manifest.json")  | [.release-please-manifest.json](https://github.com/Arize-ai/phoenix/blob/main/.release-please-manifest.json ".release-please-manifest.json")  |  [chore(main): release arize-phoenix-evals 3.9.2 (](https://github.com/Arize-ai/phoenix/commit/fe2c84b7f0dd49e2fd0f99cda044d9f447af06d0 "chore\(main\): release arize-phoenix-evals 3.9.2 \(#16885\)

* chore\(main\): release arize-phoenix-evals 3.9.2

* chore: sync uv.lock

---------

Co-authored-by: github-actions\[bot\] <github-actions\[bot\]@users.noreply.github.com>")[#16885](https://github.com/Arize-ai/phoenix/pull/16885)[)](https://github.com/Arize-ai/phoenix/commit/fe2c84b7f0dd49e2fd0f99cda044d9f447af06d0 "chore\(main\): release arize-phoenix-evals 3.9.2 \(#16885\)

* chore\(main\): release arize-phoenix-evals 3.9.2

* chore: sync uv.lock

---------

Co-authored-by: github-actions\[bot\] <github-actions\[bot\]@users.noreply.github.com>")  | Oct 9, 2026  |  
|   |   |  [docs: Adding words to acceptable spelling (](https://github.com/Arize-ai/phoenix/commit/1135a3d380ac929a8db0e93e691c5bb1a1c956c2 "docs: Adding words to acceptable spelling \(#12501\)

* Adding spelling

* Adding styles path")[#12501](https://github.com/Arize-ai/phoenix/pull/12501)[)](https://github.com/Arize-ai/phoenix/commit/1135a3d380ac929a8db0e93e691c5bb1a1c956c2 "docs: Adding words to acceptable spelling \(#12501\)

* Adding spelling

* Adding styles path")  | Apr 6, 2026  |  
|   |   |  [docs: add Python pre-release dependency version policy to AGENTS.md (](https://github.com/Arize-ai/phoenix/commit/4bdaf93f8c118ad3e1e10dadfa120f1d3aeb31bd "docs: add Python pre-release dependency version policy to AGENTS.md \(#12659\)

Prevent agents from adding PEP 440 pre-release versions \(alpha, beta, rc, dev\)
to production dependencies in pyproject.toml files, since pip excludes them by
default and they cause resolution failures for users. Dev/test deps are exempt.

Co-authored-by: Claude Opus 4.6 \(1M context\) <noreply@anthropic.com>")[#…](https://github.com/Arize-ai/phoenix/pull/12659)  | Apr 14, 2026  |  
|   |   |  [chore(main): release arize-phoenix 20.20.0 (](https://github.com/Arize-ai/phoenix/commit/4b1fa58bc7d6bd4d43d4034bac9a9a2a1def79a4 "chore\(main\): release arize-phoenix 20.20.0 \(#16696\)")[#16696](https://github.com/Arize-ai/phoenix/pull/16696)[)](https://github.com/Arize-ai/phoenix/commit/4b1fa58bc7d6bd4d43d4034bac9a9a2a1def79a4 "chore\(main\): release arize-phoenix 20.20.0 \(#16696\)")  | Oct 8, 2026  |  
|   |   |   | May 11, 2023  |  
|   |   | [chore: make AGENTS.md the source of truth](https://github.com/Arize-ai/phoenix/commit/d526714324659491bf29d467896a761ba9b01992 "chore: make AGENTS.md the source of truth")  | Jan 28, 2026  |  
|   |   |  [docs: Create CODE_OF_CONDUCT.md (](https://github.com/Arize-ai/phoenix/commit/42536ad99ec5e1067f799b0f67c01b64d84af7bc "docs: Create CODE_OF_CONDUCT.md \(#735\)")[#735](https://github.com/Arize-ai/phoenix/pull/735)[)](https://github.com/Arize-ai/phoenix/commit/42536ad99ec5e1067f799b0f67c01b64d84af7bc "docs: Create CODE_OF_CONDUCT.md \(#735\)")  | May 17, 2023  |  
|   |   |  [chore(js): move the app into the js pnpm workspace (](https://github.com/Arize-ai/phoenix/commit/752e800acb81fcc4b27ea7bd6d4fc01df867f7af "chore\(js\): move the app into the js pnpm workspace \(#15394\)

* chore\(js\): move app/ to js/app/

Pure file move with no content changes, in preparation for merging the
app into the js pnpm workspace \(#15009\). The app's standalone
pnpm-lock.yaml, pnpm-workspace.yaml, .npmrc, and .pnpmfile.cjs are left
behind at app/ and removed in the follow-up workspace-merge commit.

* chore\(js\): merge the app into the js pnpm workspace

Closes #15009

- js/pnpm-workspace.yaml: add app to packages; merge the app's
  packageExtensions \(@browser-ai/core\), allowBuilds, and widen
  minimumReleaseAgeExclude to @arizeai/*; delete the app's standalone
  pnpm-lock.yaml, pnpm-workspace.yaml, .npmrc, and .pnpmfile.cjs and
  regenerate the single js/pnpm-lock.yaml
- js/.pnpmfile.cjs: merge the app's TS6-bridge entries \(storybook docgen,
  react-docgen-typescript\)
- app: switch @arizeai/phoenix-client and @arizeai/phoenix-evals to
  workspace:*; add build:deps \(pnpm --filter 'phoenix-ui^...' run build\);
  fix repo-relative paths \(vite outDir, build:static, generate:openapi,
  oxfmt/oxlint configs, codegen scripts\); drop packageManager \(owned by
  the workspace root\)
- app tsconfig: lib ES2022 -> ES2023; the app uses Array#findLast and
  previously typechecked only because transitive type-fest@0.20.2 pulled
  in lib.esnext, which the fresh workspace resolution no longer does
- js root scripts: exclude phoenix-ui from recursive build/test/typecheck
  \(covered by its own CI\) and give lint explicit paths \(a nested
  typeAware config aborts bare oxlint runs from the workspace root\)
- Makefile: APP_DIR := js/app, single pnpm install plus app dep build,
  filtered build-frontend/build-ts/test-ts/typecheck-ts
- Dockerfile + devops harness: copy js/ and schemas/ and build via
  pnpm --filter 'phoenix-ui...' run build \(topological, deps first\)
- workflows: typescript-CI installs at js/ and builds workspace deps
  before typecheck; typescript-packages-CI filters js/!\(app\)/**;
  playwright/publish/claude-weekly-deps-upgrade updated for the new
  paths; delete package-version-check.yml \(obsolete with one workspace\)
- .gitignore: scope /js/**/docs to typedoc output so it does not swallow
  js/app/src/agent/tools/docs/

* docs: update path references for the app's move to js/app

Skills, cursor rules, DEVELOPMENT/CONTRIBUTING guides, internal docs,
READMEs, and code comments that referenced the old app/ location.
SHA-pinned GitHub permalinks in internal_docs/specs are intentionally
left unchanged since they point at historical trees.

* ci: unify TypeScript CI over the whole js workspace

Now that the app and packages share one pnpm workspace, run one
typescript-CI job instead of separate app and packages jobs:

- js root scripts: build/test/typecheck are unfiltered \(recursive over
  the whole workspace; topological ordering builds the app's workspace
  deps before the app\); lint delegates to the app's own script because
  oxlint only allows options.typeAware in the cwd-root config;
  ci:publish narrows to --filter './packages/*' since a publish run
  only needs the publishable packages built
- typescript-CI.yml: single job covering build \(+codegen-drift check,
  which now also subsumes the app relay drift check since the app build
  runs build:relay\), typecheck, fmt, lint, app OpenAPI codegen,
  generative-UI schema, UI-message fixtures, and tests; delete
  typescript-packages-CI.yml
- Makefile: aggregate build/test/typecheck/format/lint call the unified
  js scripts once; the -frontend targets remain as scoped conveniences
- claude-weekly-deps-upgrade: drop the now-redundant app-* verify steps

The 'TypeScript Packages CI Required' status check should be removed
from branch protection.")[#15394](https://github.com/Arize-ai/phoenix/pull/15394)[)](https://github.com/Arize-ai/phoenix/commit/752e800acb81fcc4b27ea7bd6d4fc01df867f7af "chore\(js\): move the app into the js pnpm workspace \(#15394\)

* chore\(js\): move app/ to js/app/

Pure file move with no content changes, in preparation for merging the
app into the js pnpm workspace \(#15009\). The app's standalone
pnpm-lock.yaml, pnpm-workspace.yaml, .npmrc, and .pnpmfile.cjs are left
behind at app/ and removed in the follow-up workspace-merge commit.

* chore\(js\): merge the app into the js pnpm workspace

Closes #15009

- js/pnpm-workspace.yaml: add app to packages; merge the app's
  packageExtensions \(@browser-ai/core\), allowBuilds, and widen
  minimumReleaseAgeExclude to @arizeai/*; delete the app's standalone
  pnpm-lock.yaml, pnpm-workspace.yaml, .npmrc, and .pnpmfile.cjs and
  regenerate the single js/pnpm-lock.yaml
- js/.pnpmfile.cjs: merge the app's TS6-bridge entries \(storybook docgen,
  react-docgen-typescript\)
- app: switch @arizeai/phoenix-client and @arizeai/phoenix-evals to
  workspace:*; add build:deps \(pnpm --filter 'phoenix-ui^...' run build\);
  fix repo-relative paths \(vite outDir, build:static, generate:openapi,
  oxfmt/oxlint configs, codegen scripts\); drop packageManager \(owned by
  the workspace root\)
- app tsconfig: lib ES2022 -> ES2023; the app uses Array#findLast and
  previously typechecked only because transitive type-fest@0.20.2 pulled
  in lib.esnext, which the fresh workspace resolution no longer does
- js root scripts: exclude phoenix-ui from recursive build/test/typecheck
  \(covered by its own CI\) and give lint explicit paths \(a nested
  typeAware config aborts bare oxlint runs from the workspace root\)
- Makefile: APP_DIR := js/app, single pnpm install plus app dep build,
  filtered build-frontend/build-ts/test-ts/typecheck-ts
- Dockerfile + devops harness: copy js/ and schemas/ and build via
  pnpm --filter 'phoenix-ui...' run build \(topological, deps first\)
- workflows: typescript-CI installs at js/ and builds workspace deps
  before typecheck; typescript-packages-CI filters js/!\(app\)/**;
  playwright/publish/claude-weekly-deps-upgrade updated for the new
  paths; delete package-version-check.yml \(obsolete with one workspace\)
- .gitignore: scope /js/**/docs to typedoc output so it does not swallow
  js/app/src/agent/tools/docs/

* docs: update path references for the app's move to js/app

Skills, cursor rules, DEVELOPMENT/CONTRIBUTING guides, internal docs,
READMEs, and code comments that referenced the old app/ location.
SHA-pinned GitHub permalinks in internal_docs/specs are intentionally
left unchanged since they point at historical trees.

* ci: unify TypeScript CI over the whole js workspace

Now that the app and packages share one pnpm workspace, run one
typescript-CI job instead of separate app and packages jobs:

- js root scripts: build/test/typecheck are unfiltered \(recursive over
  the whole workspace; topological ordering builds the app's workspace
  deps before the app\); lint delegates to the app's own script because
  oxlint only allows options.typeAware in the cwd-root config;
  ci:publish narrows to --filter './packages/*' since a publish run
  only needs the publishable packages built
- typescript-CI.yml: single job covering build \(+codegen-drift check,
  which now also subsumes the app relay drift check since the app build
  runs build:relay\), typecheck, fmt, lint, app OpenAPI codegen,
  generative-UI schema, UI-message fixtures, and tests; delete
  typescript-packages-CI.yml
- Makefile: aggregate build/test/typecheck/format/lint call the unified
  js scripts once; the -frontend targets remain as scoped conveniences
- claude-weekly-deps-upgrade: drop the now-redundant app-* verify steps

The 'TypeScript Packages CI Required' status check should be removed
from branch protection.")  | Aug 14, 2026  |  
|   |   |  [chore(dev): replace mprocs with dekit for the dev stack (](https://github.com/Arize-ai/phoenix/commit/7a1f22e4b99caa3f50c3a9c940393b049366e6ec "chore\(dev\): replace mprocs with dekit for the dev stack \(#16874\)

mprocs was renamed to dekit at 0.10.0 and is no longer published to npm
under its old name. Convert mprocs.yaml to dekit.yaml, run the stack via
dekit up && dekit attach, and require a system dekit install instead of
the npm devDependency.")[#16874](https://github.com/Arize-ai/phoenix/pull/16874)[)](https://github.com/Arize-ai/phoenix/commit/7a1f22e4b99caa3f50c3a9c940393b049366e6ec "chore\(dev\): replace mprocs with dekit for the dev stack \(#16874\)

mprocs was renamed to dekit at 0.10.0 and is no longer published to npm
under its old name. Convert mprocs.yaml to dekit.yaml, run the stack via
dekit up && dekit attach, and require a system dekit install instead of
the npm devDependency.")  | Oct 8, 2026  |  
|   |   | [chore(dev): accept any uv 0.12.x locally and run the pinned uv in iso…](https://github.com/Arize-ai/phoenix/commit/f53b01f09bba4d191d30864d4b3ccf3b5310b187 "chore\(dev\): accept any uv 0.12.x locally and run the pinned uv in isolation \(#16751\)

* chore\(dev\): accept any uv 0.12.x locally and run the pinned uv in isolation

The exact `required-version = "==0.12.19"` made every uv command fail unless
the system uv matched, so developers had to downgrade their global uv.

- `required-version` is now `>=0.12.19,<0.13`.
- The exact pin moves to `.tool-versions`, which setup-uv, the Dependabot
  sync workflow, and mise/asdf read.
- `scripts/uv.sh` runs the pinned uv via `uv tool run` when the system uv
  differs, so lock writes from make, the js/app pnpm scripts and the
  Playwright test server stay byte-identical to CI and Dependabot.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>

* fix\(ci\): keep the uv check in check-tools and read uv from pyproject on non-HEAD checkouts

- check-tools tested `command -v` on the wrapper path, which always succeeds,
  so a missing uv no longer stopped `make setup`. Test the bare `uv` instead.
- python-all-platforms and publish check out other refs \(version-* branches,
  release tags, preview feature branches\) that may predate .tool-versions.
  Read that ref's pyproject.toml with resolution-strategy "lowest", which
  resolves the required-version range to its floor \(the pin\).

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>

* refactor\(ci\): use the required-version lower bound as the only uv pin

setup-uv's resolution-strategy "lowest" resolves `>=0.12.19,<0.13` to
0.12.19, so the floor in pyproject.toml can serve as the exact pin and
.tool-versions is no longer needed. Every setup-uv step reads pyproject.toml
again, which also covers checkouts of refs that predate this change, and
scripts/uv.sh and the Dependabot sync workflow read the same floor.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>

* fix\(dev\): use a POSIX character class when reading the uv lower bound

`\\s` is a GNU grep extension; `\[\[:space:\]\]` is portable. With `set -o pipefail`
a non-matching grep also aborted the script before its own error message,
so the pipeline now tolerates no match and the diagnostic is reachable.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

---------

Co-authored-by: Claude Opus 5.5 <noreply@anthropic.com>")  | Oct 5, 2026  |  
|   |   |  [feat: switch client to be Apache 2.0 (](https://github.com/Arize-ai/phoenix/commit/ee6f6ee15deca45430aff2732fb5ac7253d82522 "feat: switch client to be Apache 2.0 \(#10332\)")[#10332](https://github.com/Arize-ai/phoenix/pull/10332)[)](https://github.com/Arize-ai/phoenix/commit/ee6f6ee15deca45430aff2732fb5ac7253d82522 "feat: switch client to be Apache 2.0 \(#10332\)")  | Nov 19, 2025  |  
|   |   |  [feat: add license and update pyproject.toml (](https://github.com/Arize-ai/phoenix/commit/1ad94f29478c6703082f99ef9c8a9737f78195b1 "feat: add license and update pyproject.toml \(#157\)

* add license and update pyproject.toml

* add missing period in copied license")[#157](https://github.com/Arize-ai/phoenix/pull/157)[)](https://github.com/Arize-ai/phoenix/commit/1ad94f29478c6703082f99ef9c8a9737f78195b1 "feat: add license and update pyproject.toml \(#157\)

* add license and update pyproject.toml

* add missing period in copied license")  | Jan 6, 2023  |  
|   |   |  [chore: complete removal of the legacy phoenix.session.client module (](https://github.com/Arize-ai/phoenix/commit/743d32032f6a34ce51fce44959f88ba1ce197c56 "chore: complete removal of the legacy phoenix.session.client module \(#15709\)

* chore: complete removal of the legacy phoenix.session.client module

* chore: total removal of the legacy client shims and dead code

Addresses review feedback on #15709: drop the removal-shim scaffolding
entirely rather than restructuring it.

- src/phoenix/__init__.py: remove _REMOVED_MODULES / _REMOVED_ATTRIBUTES,
  the sys.meta_path finder+loader, and the PEP 562 __getattr__. Removed
  modules and attributes now fail with the standard ModuleNotFoundError /
  AttributeError; MIGRATION.md remains the migration reference.
- Delete tests/unit/session/test_legacy_client_removal.py \(nothing left to
  cover\).
- Delete src/phoenix/utilities/re.py and its test — parse_env_headers lost
  its last caller with get_env_client_headers; arize-phoenix-client and
  arize-phoenix-otel ship their own copies.
- MIGRATION.md: importing phoenix.session.client now raises
  ModuleNotFoundError, not a guided ImportError.

Co-authored-by: Mikyo King <5640648+mikeldking@users.noreply.github.com>

---------

Co-authored-by: github-actions\[bot\] <41898282+github-actions\[bot\]@users.noreply.github.com>
Co-authored-by: claude\[bot\] <41898282+claude\[bot\]@users.noreply.github.com>
Co-authored-by: Mikyo King <5640648+mikeldking@users.noreply.github.com>")[#…](https://github.com/Arize-ai/phoenix/pull/15709)  | Aug 28, 2026  |  
|   |   | [refactor(cost-tracking): move model price sync out of .github and dro…](https://github.com/Arize-ai/phoenix/commit/e4713156c6c2eada91d691834954e9d27cae151e "refactor\(cost-tracking\): move model price sync out of .github and drop its tests \(#16908\)

* refactor\(cost-tracking\): move model price sync out of .github so unit tests stop needing it

The Mistral pricing tests loaded .github/.scripts/sync_models.py by path, which
the Unit Tests sparse checkout omits, so CI needed a stopgap checkout entry.

- Move the sync script to scripts/cost_tracking/ with its tests beside it, run
  by `make test-sync-models` and a new Test Model Cost Sync Script CI job.
- Keep the runtime Mistral cost-calculation tests in tests/unit.
- Drop the stopgap .github/.scripts/sync_models.py sparse-checkout entries.
- Add `make check-unit-test-paths` and a CI job that fails when a unit test
  reads a repo path outside the Unit Tests sparse checkout, or when the two
  Unit Tests jobs' checkout lists drift apart.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01QLcWow97hFyWXWxKyuqdEx

* refactor\(cost-tracking\): run sync-script tests in the cost sync workflow, drop path guard

- Run make test-sync-models \(mypy --strict + pytest, lean env\) right before
  the weekday sync instead of in a separate PR CI job
- Remove the static unit-test path guard; the sparse-checkout Unit Tests job
  already fails on a missing path
- Drop test_mistral_cost_calculation.py, which duplicated cases in
  test_cost_details_calculator.py

* refactor\(cost-tracking\): drop sync-script tests, keep a pure move

The Mistral sync tests and the checked-in-manifest schema test duplicate what
the weekday sync and test_model_cost_manifest.py already cover.

---------

Co-authored-by: Claude <noreply@anthropic.com>")  | Oct 9, 2026  |  
|   |   |  [docs: remove legacy evaluation cookbooks (](https://github.com/Arize-ai/phoenix/commit/450688042c1df9640279be568bc4888140cab9e0 "docs: remove legacy evaluation cookbooks \(#16651\)

Remove the More Cookbooks index, relevance classification, and Ragas math agent cookbook pages along with the in-repo agent tool classification notebooks. Add redirects for the removed pages.")[#16651](https://github.com/Arize-ai/phoenix/pull/16651)[)](https://github.com/Arize-ai/phoenix/commit/450688042c1df9640279be568bc4888140cab9e0 "docs: remove legacy evaluation cookbooks \(#16651\)

Remove the More Cookbooks index, relevance classification, and Ragas math agent cookbook pages along with the in-repo agent tool classification notebooks. Add redirects for the removed pages.")  | Oct 1, 2026  |  
|   |   |  [docs: reorganize Get Started around the agent workflow (](https://github.com/Arize-ai/phoenix/commit/2afdf488e638f282f742c7daf796a09a56662971 "docs: reorganize Get Started around the agent workflow \(#16288\)

* docs: reorganize Get Started around the agent workflow

Restructure the Get Started section to mirror the Arize AX quickstart:
Instrument your agent, Evaluate your agent, Improve your agent, Coding
agent setup, PXI, and Phoenix Demo.

The Python and TypeScript quickstarts previously sat in separate nav
groups, so each language duplicated the whole section tree. Each pair is
now a single page with Python and TypeScript tabs, so the language is a
choice within a section rather than a parallel branch of the nav.

Every page body is carried over verbatim into its language tab. The only
text rewritten is internal links that pointed at the retired
ts-get-started-* paths, which now have redirects to the merged pages.

- Instrument your agent  <- get-started-tracing + ts- counterpart
- Evaluate your agent    <- get-started-evaluations + ts- counterpart
- Improve your agent     <- group over the prompt playground and
                            datasets/experiments guides
- Coding agent setup     <- group over Agent-Assisted Setup and Coding
                            Agents
- MCP group, PXI, and Phoenix Demo are unchanged aside from ordering
- End to End Features Notebook is removed from the nav; the page itself
  is left in place

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: cut duplicated prose from the Get Started guides

The previous commit put each language's full guide in its own tab, which
made every language-agnostic sentence render twice on the same page. On
the prompt playground guide that was 37% of the prose; on datasets and
experiments, 40%.

Where the two tabs told the same story, the narrative is hoisted above
the tabs and the tabs now wrap only what genuinely differs: code, videos,
the companion-code callout, and the two prompt snippets. The prompt
playground and experiments guides had identical step sequences, so their
steps are shared outright. Tracing and evaluations each have an extra
TypeScript step, so their steps stay tabbed and only the intro, prereqs
and closing sections are shared.

No prose was rewritten. Every line either survives verbatim from the
previous commit or is a prefix of one with a redundant trailing clause
removed, verified mechanically. Where the two tabs differed only
trivially, one existing sentence is kept verbatim rather than a new one
written; this also picks up several corrections the TypeScript copy
already had:

- "traces that received an incompleteness evaluation label" ->
  "an incomplete evaluation label", matching the filter beside it
- "Filter your traces by the incorrect evaluation label" -> "incomplete";
  the filter under it has always been evals\['completeness'\] == 'incomplete'
- "Using the explanations apart of the evals" -> "from the evals"
- "New application runs as a results of our task" -> "as a result"
- "ran your first experiment" / "ran an A/B test" -> "run"

Also drops, on the tracing guide, a sentence restating the bullet list
above it and three repeats of the supported-frameworks disclaimer that
the intro already carries.

Verified: all 44 code blocks byte-identical \(multiset\), every step title
and video retained, no duplicate sentence in any rendered page, both link
checkers clean, all four pages render with working tabs.

Words: 10790 -> 8789 \(18.5% shorter\).

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: tighten Instrument and Agent-Assisted Setup, repoint quickstart

Three changes to the Get Started section.

Instrument your agent opened with a paragraph of series framing --
"This guide walks through a complete workflow ... Each guide in this
series introduces one piece of that workflow" -- which is word for word
the Overview page's opening and says nothing about instrumentation. It is
removed here and still lives on the Overview page. The guide now opens on
its own subject: what a trace is.

/docs/phoenix/quickstart redirected to the Overview page, which is not in
the nav. It now lands on Instrument your agent, the actual first guide in
the series. The Overview page is unchanged and still reachable from the
four pages that link to it.

Agent-Assisted Setup is only ~350 words of prose but 89 lines, because
almost two thirds of it is follow-up reference rather than setup. The
three advanced sections -- re-running a single step, non-interactive runs,
and unsupported agents -- move into an AccordionGroup, so the page reads
as an intro, two steps and a verification section. Nothing is deleted:
all five code blocks are byte-identical and the collapsed text is still
in the page. The one prose deletion is "The fastest way to add Phoenix
tracing to your application.", which restated the frontmatter description
rendering directly above it.

Trade-off worth noting in review: collapsed sections leave the "On this
page" table of contents, which now lists only "Confirm traces are
flowing".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: hold every Get Started intro to four sentences or fewer

The intros restated themselves. Evaluate your agent opened with seven
sentences making three points: what an eval is \(said three times\), what
the guide does \(said twice\), and which metrics you can score against.

Deletions only, no rewriting. Every surviving sentence is verbatim.

Evaluate your agent, 7 -> 4. Dropped:
- "Evaluations attach quality signals to runs so that correctness or
  relevance can be reasoned about consistently instead of judged case by
  case." Restates the opening sentence and the traces/evals contrast that
  follows it, and states it as a negative.
- "We'll start with data that already exists in Phoenix, define a simple
  evaluation, and run it so we can see results directly in the UI."
  Restates "In this guide, you'll set up evaluations ... and run them on
  existing trace data".
- "The goal is to move from "I have model outputs" to "I can measure
  quality in a repeatable way."" Restates the opening sentence.

Iterate on Your Prompts, 5 -> 3. Dropped "Up to this point, we've traced
our agent runs and evaluated their outputs. Now we'll focus on prompts by
grouping failures into a dataset and iterating on prompt variants in the
Prompt Playground." -- the sentence above it already says the guide
groups failing runs into a dataset and iterates in the Playground, and
the prereq list directly below states the same continuity as two bullets.

Instrument your agent, TypeScript tab 5 -> 4. Dropped "In this guide,
we'll get tracing set up and walk through how to instrument an
application.", which the sentence after it says with the specifics. The
Python tab already carried this as one sentence.

Counts are per reader, shared intro plus their language tab, excluding
callouts: Instrument 4, Evaluate 4, Iterate 3, Optimize 3,
Agent-Assisted Setup 1.

Code blocks unchanged on all three pages.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say "agent" instead of "app" across Get Started

Refers to the reader's system as an agent throughout, matching the
section titles \("Instrument your agent", "Evaluate your agent"\). 51 lines
across the five guides plus the Overview page. Code blocks and URLs are
untouched; the `js/examples/apps/mastra-quickstart` paths and every
identifier keep their spelling.

The experiments guide's title becomes "Optimize Your Agent with
Experiments", so the nav label changes with it.

Two phrases needed care rather than a plain swap:

- "an agent application" would have become "an agent agent"; it is now
  just "an agent".
- "the agents that make up our application" would have become "the agents
  that make up our agent"; it is now "our agent system", the phrasing the
  experiments guide already uses.

Agent-Assisted Setup keeps "app" for the reader's codebase. That page is
about handing work to a *coding* agent, so "let your coding agent add
tracing to your agent" reads as one thing instrumenting itself. The
collision is worse than the inconsistency.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: merge the prompts and experiments guides into Improve your agent

"Iterate on Your Prompts" and "Optimize Your Agent with Experiments" were
two halves of one workflow split by *how* you make the change rather than
by what you are doing. Both started from eval-labelled failing traces,
both ended in the same Experiments view, and each pointed at the other as
the next step, so a reader could not tell which came first.

They are now one guide, matching the AX quickstart, which has a single
"Improve your agent" page with method tabs rather than a group.

Four steps, with the method choice appearing once instead of at every
stage:

1. Create a dataset from failed traces      \(shared\)
2. Use explanations to identify improvements \(shared; was taught twice\)
3. Make the change and run it   -- tabs: In the Prompt Playground /
   In code, with Python and TypeScript nested inside each
4. Compare versions in the Experiments view  \(shared narrative\)

Nothing is dropped. Verified against both source pages: 13 of 13 code
blocks, 12 of 12 videos, 12 of 12 frame captions, every numbered
instruction, and every H3 section.

Deliberate changes beyond the merge:

- The two cross-links between the guides are gone; they would now point
  at this page. That circularity was the reason to merge.
- "the first from Step 3" is now "the first from your baseline run",
  because the step it referenced no longer has that number.
- "A dataset created from previous runs" leaves the prerequisites, since
  step 1 builds it.
- The two "Congratulations" lines and the two "Learn More" sections are
  one each.
- About eight new connective sentences were written, the only prose on
  this page that is not carried over verbatim. Interleaving two
  workflows needs them.

Seven URLs now redirect here: both retired page paths, both ts- paths,
and the three legacy quickstart paths.

docs/phoenix.mdx had a card for each guide in both its Python and
TypeScript groups; each pair is now one "Improve Your Agent" card.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rewrite Get Started in second person, direct voice

The guides narrated themselves in the first person plural -- "we'll
install", "now that we have trace data, we can", "at this point we've
defined" -- which reads as tutorial filler and contradicts the house rule
that guides use active voice and second person. 155 instances of that
register across the section, now 0:

  royal "our"                   55 -> 0
  royal "we'll" / "we're"       44 -> 0
  "we can"                      26 -> 0
  "At this point,"               7 -> 0
  "Next, we..."                  6 -> 0
  "In this step, we..."          6 -> 0
  "Now that ..., we"             4 -> 0
  "let's"                        5 -> 0
  "More importantly,"            2 -> 0

Instructions are now imperative and the prose states facts about the
system rather than narrating a shared journey. "Next, we'll register a
tracer provider linked to a project in Phoenix. This project is where
your traces will show up" becomes "Register a tracer provider linked to a
project in Phoenix. That project is where your traces show up."

Two errors fixed on the way through:

- "seen how an agent runs shows up from start to finish" -> "how an agent
  run shows up"
- "our finanical writer agent" -> "The financial writer agent"

No instruction was dropped. Code blocks, videos, link destinations, step
titles and H3 sections are identical on all three guides: 17/2/13/9/14 on
tracing, 14/2/9/9/5 on evaluations, 13/12/5/4/9 on Improve your agent.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: sharpen the tracing opener and drop every em dash

The tracing guide opened with a sentence that did not parse: "A trace is
a record of a single run of your agent, broken down into spans that show
what happened at each step \(how agents, tasks, and tools executed\) and
provides the raw data needed for everything that follows." After the
participial phrase, "and provides" cannot reach back to "is a record", so
it reads as attaching to "spans", which would need "provide". It also
said the same thing twice \("what happened at each step" and "how agents,
tasks, and tools executed"\), and closed on "the raw data needed for
everything that follows", which was left over from the series framing
that now lives only on the Overview page.

Replaced with the house formulation from ax/instrument/what-are-traces
and phoenix/tracing/concepts-tracing, which also defines a span instead
of gesturing at it:

  A trace captures one end-to-end run of your agent as a tree of spans.
  Each span is a single step \(an agent, a task, a tool call, or an LLM
  call\) with its input, output, and timing.

The page also picks up a frontmatter description, matching the AX tracing
guide. It renders as the subtitle under the H1, so it carries the framing
while the body carries the mechanic, and the two do not restate each
other.

Em and en dashes are now zero across the section. Two were mine; two were
pre-existing on Agent-Assisted Setup, where a dash joined two independent
clauses that are clearer as separate sentences.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: stop repeating the prerequisites, and drop the long reference from Get Started

The evaluations guide stated its prerequisites twice, about fifteen lines
apart, in different words. At the top: "you'll need to have completed Get
Started with Tracing so you should have: Financial Analysis and Research
Chatbot / Trace Data in Phoenix". Then again inside step 1: "Evaluations
in Phoenix run over existing trace data. If you followed the tracing
guide, you should already have: A project in Phoenix / Traces containing
LLM inputs and outputs". Both tabs carried it, so a reader met the same
list twice per pass.

The step's version was the more precise one, so its detail is folded into
the single list at the top, which now reads "A project in Phoenix holding
traces with LLM inputs and outputs". Step 1 keeps the one fact it added,
that Phoenix evaluations run over existing trace data, and loses the
duplicate list.

Worth noting for future passes: string-similarity scanning did not catch
this, because the two lists share almost no wording. Semantic repetition
needs a read, not a regex.

The Overview page repeated itself inside single paragraphs the same way:
"using the same inputs and evaluation criteria" in two consecutive
sentences, and "spans that show what happened at each step" followed by
"Traces show how agents, tasks, and tools executed during a run". Both
are now stated once.

Coding Agents leaves the Get Started nav. It is 466 lines, 2258 words and
27 code blocks of reference material for Claude Code, Codex, the CLI, MCP
and skills, and its own opening line says "If you haven't instrumented
your app yet, start with Agent-Assisted Setup" -- it is the step after
getting started, not part of it. The page is unchanged and still listed
under Integrations > Developer Tools, and Agent-Assisted Setup already
links to it for ongoing workflows.

That leaves one page in the group, so it collapses to a single nav entry.
Agent-Assisted Setup gains sidebarTitle "Coding agent setup" so the label
still matches the agreed structure, the same way pxi.mdx shows as "PXI
Agent".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix five accuracy defects found by checking Get Started against the source

Verified the guides against the Phoenix source in this repo rather than
against other docs. Five real defects, all inherited from main:

1. `evals\['completeness'\].label == 'incomplete'` is a **span** filter. The
   guide said "Filter your traces by the incomplete evaluation label",
   which points at the traces table, where the expression is rejected:
   src/phoenix/trace/dsl/trace_filter.py refuses it with "`evals\[...\]` is
   not available in the trace filter". docs/.../filter-expressions.mdx
   lists it under span filters, and the UI has separate SpanFiltersContext
   and TraceFiltersContext. Now reads "Filter the spans table".

2. and 3. The add-to-dataset flow is span-based, not trace-based:
   AddSpansToDatasetInput carries `span_ids`
   \(src/phoenix/server/api/mutations/dataset_mutations.py:225\), and the
   dataset guide says "select the span". "Select the traces you want to
   include" and "Add the selected traces" now say spans.

4. `client = Client\(\)` ran with no import on this page. The evaluations
   guide imports it, but this page has to stand alone, so it now carries
   `from phoenix.client import Client`.

5. `import { getDataset } from "@arizeai/phoenix-client/datasets"` was
   never used. The companion example the guide points at,
   js/examples/apps/mastra-quickstart/src/mastra/experiments/experiment.ts,
   imports the identical set without it. Removed.

Everything else checked out against source: register\(project_name,
auto_instrument\), LLM\(provider, model\), ClassificationEvaluator\(name, llm,
prompt_template, choices\) and .bind\(\), Client\(\).datasets.get_dataset\(dataset=\),
Client\(\).experiments.run_experiment\(dataset=, task=, evaluators=\),
suppress_tracing's import path, the phoenix-client and phoenix-evals TS
exports, runExperiment accepting a {datasetName} selector, ports 6006 and
4317, exit code 6 as NOT_VERIFIED, every `px setup` subcommand and all
nine documented flags, and both companion code paths.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: one language choice on Improve your agent, not ten

The page had accumulated 10 separate Python | TypeScript pickers down its
length: one on the companion-code callout, one per video pair, and one
inside each half of the method tabs. Mintlify syncs tabs that share
labels, so picking a language once did switch all of them, but rendering
the control ten times reads as ten decisions.

Language is now a single choice at the top, the same shape the tracing
and evaluations guides use. Method tabs \(In the Prompt Playground /
In code\) stay where they were, inside step 3 and step 4. A reader now
sees one language picker and two method pickers instead of twelve
controls.

This is purely structural: every Python | TypeScript group was resolved
to the chosen language and the whole body wrapped in one pair. No prose
was rewritten. Verified against the previous version: all 13 code blocks,
12 videos, 12 frame captions, every numbered instruction, every heading
and every step title still present.

The cost is 78 lines. Language as the outer axis means the
language-agnostic narrative -- the dataset steps, the explanation step,
the shared prose in steps 3 and 4 -- exists once per branch. That is why
the step count in the file doubles from 4 to 8 while a reader still sees
4. Worth it for one obvious choice at the top; the alternative was
keeping the page shorter and the navigation confusing.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: method first, language inside it, on Improve your agent

Corrects the tab hierarchy to match ax/get-started/get-started-tracing,
where the outer tabs are methods \(By npx evals / By Arize Skills / By
Code\) and the Python | TypeScript picker sits inside By Code. The previous
commit put language on the outside, which was the wrong axis.

A reader now picks the method first \(In the Prompt Playground / In code\),
then the language within it. Two controls, in that order.

Also tightened the prose, since with tabs every shared line exists in more
than one branch and each cut lands more than once. Deleted as restatements
of the line beside them:

- "This dataset now represents a concrete failure case for your agent."
- "Start by grouping together the traces that didn't perform well."
- "Start from a real prompt the agent actually used." -- the next sentence
  already gives "a real starting point".
- "Saving prompt variants makes it easy to track changes..."
- "An experiment reruns the same inputs through different versions..." --
  the page intro already defines an experiment; this was the third time.
- "With two versions in hand, compare them..." -- restated the step title.

Shortened: the two-paths lead-in \(the tab labels say it\), the
create-datasets aside \(said "in code" twice\), and the task and evaluation
definitions. Fixed two royal "we" the voice pass missed: "the dataset we
made earlier" and "we should see more traces".

Line count: 605 -> 581 from the trims, then -> 694 from the inversion.
The growth is real and worth naming. Steps 1 and 2 -- create the dataset,
read the explanations -- are method-agnostic, so method-outer copies them
into both method branches, and language-inner copies them again. Four
copies of about thirty lines. A reader still sees each step once.

The wrinkle that buys: the method choice now sits above two steps that do
not depend on it, so a reader picks Playground or code before creating the
dataset. The alternative keeps steps 1 and 2 shared and moves the method
choice down to step 3, at the cost of a second visible language picker.

Verified against the previous version: every code block, video, caption,
link, heading and step title still present.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: move the companion-code callout out of the no-code path

The Prompt Playground branch offered "a companion notebook with runnable
code examples" in a path where you write no code. That was a side effect
of wrapping the whole body in the method tabs. The callout now appears
only under In code, for both languages.

The Python | TypeScript picker stays inside the Prompt Playground branch,
because that path does depend on which language you followed -- not for
the language itself, but for what you built with it. Six of the videos
record the CrewAI app rather than the Mastra app, and the dataset input
syntax differs with the agent: `{tickers}` and `{focus}` are CrewAI task
variables, `{{input}}` is Mastra. The label is a proxy for "which
quickstart did you follow".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: drop the horizontal rules bracketing the companion-code callouts

Every companion-code callout in Get Started was wrapped in `---` rules,
inherited from main. Each renders as a thematic break with generous
margins, so the callout sat in a band of empty space with a thin line
above and below it. The callout already has its own bordered container,
so the rules added separation that was not needed.

12 rules removed: four on the tracing guide, four on evaluations, four on
Improve your agent. Frontmatter delimiters are untouched, including the
multi-line description block on the tracing guide, whose closing `---`
sits at line 6 and is easy to mistake for one of these.

No prose changed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the playground template syntax, which would not have interpolated

The Python path told readers to set the User prompt to `{tickers}` and
`{focus}`. That is F_STRING syntax, and the Prompt Playground defaults to
Mustache:

  js/app/src/components/templateEditor/constants.ts
    FString:  "F_STRING",  // {variable}
    Mustache: "MUSTACHE",  // {{variable}}

  js/app/src/store/playground/playgroundStore.tsx:273
    templateFormat: TemplateFormats.Mustache,

Playground.tsx reads the format from that store, so with default settings
single braces are not substituted -- the prompt would have carried the
literal text `{tickers}`. The guide never mentioned the format selector,
so there was no way for a reader to know.

Most likely this was copied from the CrewAI task template on the tracing
guide, `Task\(description="Research: {tickers}"\)`, which legitimately uses
single braces. That is CrewAI's own templating, not Phoenix's, and it is
left alone.

Both paths now use Mustache, and the shared lead-in says so, so the two
language tabs differ only in the variable names.

The variable names themselves were already correct and are framework
driven, traceable in the guides' own code: the Python experiment passes
`example.input` straight into `kickoff\(inputs=...\)`, so dataset examples
carry `tickers` and `focus` keys, while the TypeScript task types
`example.input` as a `{role, content}\[\]` message array under a single
`input` key.

Verified rendered: the block shows `Research: {{tickers}}` literally, and
the format note appears in both language tabs.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: expand the Agent-Assisted Setup accordions back into headings

I collapsed three sections into an AccordionGroup when the page read as
too long. That hid 307 of its 542 words, 57% of the page, and dropped the
three section names from the "On this page" table of contents, leaving one
entry. It also made the page look like content had been deleted, which is
what prompted the question.

The reason for collapsing them has gone: the length concern was mostly the
466-line Coding Agents reference, which no longer sits in this nav group.
What remains is ~330 words, the shortest page in Get Started, so hiding
more than half of it costs more than it saves.

Re-run a single step, Run non-interactively \(CI or agents\), and Use an
unsupported agent are `##` headings again, so all four sections appear in
the table of contents and can be linked to directly.

Code blocks and links are identical.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: no language picker in the Prompt Playground path

You write no code in the Prompt Playground, so asking the reader to pick
Python or TypeScript there was misleading. The picker now appears only
under In code, where it selects actual code, which matches
ax/get-started/get-started-tracing.

Two controls on the page, in this order:

  In the Prompt Playground | In code
    In code -> Python | TypeScript

What the picker had been carrying in that path, and where it went:

- Steps 1, 2 and 4 were already identical between the languages.
- The dataset input keys are real, so they are stated inline: which keys
  you have depends on the agent you traced, `{{tickers}}` and `{{focus}}`
  from the Python guide, `{{input}}` from TypeScript. Both forms are now
  written out rather than left to be inferred.
- The example prompt line keeps the Python variant, which is the one that
  matches the finding step 2 states \("the report lacked financial
  ratios"\). The TypeScript variant said something different, about
  per-ticker metrics.
- Five videos drop out: ts-first-dataset, ts-first-prompt,
  ts-playground-experiment, ts-compare-prompts, ts-view-experiment. They
  carried the same captions and demonstrated the same click-path as the
  five that remain, recorded against Mastra instead of CrewAI.

All 13 code blocks and every link destination are unchanged. 668 -> 538
lines.

Braces checked: every `{{...}}` on the page is either inside a fenced
block or an inline code span, so MDX treats them as text. Verified in the
rendered DOM.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: restructure Get Started nav and verify evals against the product

Reshape the Get Started nav to Observe Your Agent / Evaluate Your Agent /
PXI / Arize Phoenix for Agents / Phoenix Demo Account. Per-language pages
become tabs within each section, with language nested inside the code path.

Merge the Improve Your Agent guide into Evaluate Your Agent, which is now
written as offline evals: a dataset feeds a task, an evaluator scores each
output. Phoenix has not released continuous scoring of live production
traffic, so the previous framing was wrong.

Extract Skills from the Coding Agents page into its own page and group the
agent-facing docs \(agent-assisted setup, skills, MCP\) under Arize Phoenix
for Agents. A pointer heading keeps existing #skills anchors resolving.

Correct six claims against Phoenix 20.15.0 running locally:

- `output` is the task's output, not an example payload. `reference` is the
  example's expected output \(experiment_runner.py:831, message_helpers.py:279,
  and the compare view's "reference output" column\).
- Creating a dataset from spans is one action, not two. You select spans and
  click Add to Dataset; creating a new dataset happens inside that popover
  \(SpanSelectionToolbar.tsx:212, DatasetSelectorPopoverContent.tsx:104\).
- The evaluator dialog's submit button is Create, not Save.
- The playground control is "Test over a dataset".
- Experiment runs live on the Experiments tab, compared via the
  "comparisons" selector.
- Playground template format defaults to Mustache, so dataset input keys
  need double braces.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: remove em dashes from the Get Started nav pages

All 98 em dashes on pages now reachable from Get Started, none of them
authored by this branch: pxi.mdx \(31\), coding-agents.mdx \(22\), remote-mcp.mdx
\(14\), mcp.mdx \(4\), docs-mcp.mdx \(3\), phoenix-mcp-server.mdx \(1\), plus 14
bullet and label dashes replaced with colons.

Each was repunctuated in context rather than swapped for a single character:
a colon where a list or restatement follows, a comma for an aside, a period
where an independent clause follows, parentheses for the three parentheticals
that span hard-wrapped lines. No dash inside a fenced code block was touched.

Diff is 89 insertions and 89 deletions, so no prose was added or removed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: point Evaluate Your Agent at the trace-scoring flow

The merged page covers offline evals over experiments. Readers who want to
score traces they have already collected had no path to it from Get Started,
since that content now lives under Tracing and Evaluation. Links to
Evaluating Phoenix Traces and the two eval quickstarts.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: simplify Phoenix get started quickstarts

* docs: keep Agent, not App, in the Get Started flow

Restores "Observe Your Agent" and "Evaluate Your Agent" and aligns the
surrounding copy, which had moved to "app": both page titles and descriptions,
the section headings on the Get Started overview, the landing-page cards, and
two "Observe Your App" cross-links that would have pointed at a page with a
different name. `app.ts` stays, since that is a file the reader creates.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: one Quick Starts card group, without the Improve card

The Improve page merged into Evaluate Your Agent, so its card pointed at the
same page as the Evaluate card. Dropped it and folded its description into
Evaluate's, which had described only the LLM-as-a-judge half.

The Python and TypeScript groups held identical cards, and both target pages
now select language with tabs, so the split no longer carried information.
Collapsed to a single group. The redirect from the retired improve path is
untouched.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make Evaluate Your Agent a single Playground path

Drops the Prompt Playground / Code tab split and keeps the Playground steps,
so the quickstart teaches one path end to end. 401 lines to 146.

Adds a "Prefer to do this in code?" section that sends readers to Run
Experiments for the Python SDK and to the TypeScript experiments reference for
runExperiment, so both languages the removed tab covered still have a home.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say which traces to evaluate and what to measure

The steps described where to click without answering the two questions a
reader arrives with. Five tweaks, 146 lines to 151:

- Dataset step gives a size to aim for and two ways to find failing traces,
  a filter expression or asking PXI to walk them
- Evaluator step names error patterns worth scoring instead of only listing
  the two evaluator types
- Saving a prompt from a trace opens with why that prompt is the one to use
- The baseline is stated as the thing later edits are measured against
- Comparison step closes with how to read the delta, including what to do
  when the score drops

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say offline evaluators in the Evaluate description

Also removes two lines the single-path change left stale: an opening sentence
that restated the description, and a paragraph telling readers to choose
between the Playground and code paths, which are no longer tabs on this page.
The offline framing replaces it, with the caveat that continuous scoring of
live production traffic has not shipped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say the manual path builds everything from the Phoenix UI

Now that this page is Playground-only, the section intro should name the
surface the steps use.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: shorten the PXI line in the dataset step

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: restore the code experiment example as its own page

Making Evaluate Your Agent a single Playground path deleted Nancy's Python
and TypeScript worked examples, and the pages it linked to instead cover the
same APIs without that scenario. Recovered them verbatim from 966510c74 as
Evaluate a Prompt Change in Code, under How-to: Experiments.

All 7 code blocks and both 4-step sequences are byte-identical to the removed
tab; only the Tab wrapper was dropped and the block de-indented one level. The
page opens by tying itself to the quickstart, so the support agent scenario
carries across the UI and code paths again.

"Prefer to do this in code?" now points here first, with Run Experiments as
the wider reference.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the new experiment page headings its siblings have

The page had no H2s, so it contributed nothing to the table of contents while
every other page in How-to: Experiments has several. Adds a heading over the
tabbed walkthrough, notes that the script creates its own dataset so it runs
without traces, and points at Set a Baseline Experiment, which covers the
persistent baseline that this page's in-code comparison does not.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rework Observe and Evaluate, split PXI into Get Started and Learn More

Observe Your Agent: intro rewritten to say what the page does and where the
concepts live, `\(Recommended\)` on the agent-assisted path, and the manual
steps reordered to start Phoenix, get your keys, then set up tracing. The
install step folded into setup, and the three steps that were identical in
both language tabs hoisted above them.

Evaluate Your Agent: no longer depends on having traces. The dataset step
hands you three travel support examples to upload, so the page runs on a
fresh Phoenix. Traces became an optional route rather than the only one.
Switched to an LLM judge, added copyable Playground prompts for the baseline
and the edit, and dropped the save-a-prompt-from-a-trace step.

PXI: 3,315 words split into a 315-word Get Started \(what it is, what it does,
three steps\) and a 1,158-word Learn More grouped as capabilities, surfaces,
configuration, and privacy. Nothing deleted except the architecture diagram
and the flag and slash-command tables, which duplicate the CLI reference.
Accordions flattened so nothing is hidden behind a click.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: consolidate Arize Phoenix for Agents

Snapshot of the agents/MCP consolidation: agent-assisted-setup folded into a
new developer-tools/cli page, and remote-mcp, docs-mcp, and phoenix-mcp-server
collapsed into one integrations/mcp page, with redirects for all four retired
paths. Nav and inbound references updated across the CLI reference, release
notes, self-hosting, and the shared onboarding snippets.

Committed from the docs-get-started-reorg worktree so the two in-flight
branches can be merged.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: point the tracing quickstart at the new CLI page

The agents consolidation folded agent-assisted-setup into
developer-tools/cli. This was the last reference still relying on the
redirect rather than the real page.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: link the uploaded dataset, and cross-link the other Observe methods

The travel-support.csv download pointed at assets/datasets/, but the file was
uploaded to assets/images/arize-docs-images/evaluate/. Repointed to where it
actually lives, so the Evaluate walkthrough runs end to end. Verified the URL
serves 3 rows with the question and expected_answer columns the page maps.

Also adds links out of both quickstarts for the routes they do not teach,
prompted by the Phoenix workflow diagram: reading traces in code, from a
coding agent via the CLI or MCP, by asking PXI, and across Sessions; plus PXI
for curating datasets and running experiments. Links only, no new pages. The
Evaluate step stays absent on purpose, since online evals have not shipped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make both Evaluate paths use one dataset, and close three story gaps

Found by running every path in Get Started end to end as a new user.

The agent-assisted prompt inlined three examples, but only one of them was in
the CSV the manual path downloads. Cutting that file from eight rows to three
had left the prompt behind. The prompt now downloads the same CSV, so there is
one dataset and the two paths cannot drift again. It also restores the page's
own argument, which turns on the visa and refund rows.

Three gaps between the paths:

- The Recommended path ended when px setup finished and never said to open the
  Traces view. Only the manual path told you to look at what you captured.
- "Ask your coding agent" said to start Phoenix without giving the command,
  which only appeared in the other tab.
- Observe handed off promising to "convert selected traces into a dataset",
  which Evaluate no longer does, since it starts from a downloadable dataset.

Verified by execution, not reading: both language paths install and run and
produce correct OpenInference traces \(a support_reply CHAIN wrapping a
ChatCompletion LLM span with messages, model, and token counts\), px setup
connects and writes .env.phoenix, both skills commands install, and the CSV
builds the same 3-example dataset from either path.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: state the Python 3.10 floor and name the Traces tab in Observe

Both quickstart paths were run end to end against Phoenix 20.16.0. On
macOS's default Python 3.9, pip quietly installs arize-phoenix-otel
0.14.0 instead of 0.17.1, because the current packages require 3.10+.
A project also opens on the Spans tab, so "the Traces view" sent
readers looking for a view that isn't the landing page.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>

* docs: make the Evaluate comparison actually move the score

The page claimed the baseline prompt invents visa rules and agrees to refunds
it should decline. Measured against gpt-5-mini it does neither: the baseline
scored 1.0, the edited prompt scored 1.0, and stripping the baseline to
"Answer the customer's question" still scored 1.0. Step 4 promised a rise that
never came, so the lesson did not land.

Fixed by changing what the examples test. They now ask about a fictional
airline's Basic fare policy, with values deliberately unlike industry norms: a
48 hour refund window, a 40 dollar change fee, one included checked bag. The
model cannot guess any of them, so the baseline answers from industry defaults
and gets all three wrong. The edit supplies the policy.

Measured: baseline 0.00, edited 1.00.

This also makes the lesson durable. The baseline fails for lack of information
rather than lack of capability, so a better model will not quietly erase the
gap the way it did with the previous examples.

Also drops the Observe handoff sentence, per review.

The dataset CSV needs re-uploading to the same path; the agent-assisted prompt
reads it by URL and needs no edit.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the PXI paths, found by running them

Tested both PXI pages against a live Phoenix for the first time.

The terminal client command did not work. `npx -y @arizeai/phoenix-cli pxi`
exits with "unknown command 'pxi'", because pxi is a separate bin in the
package rather than a px subcommand, so npx ran the default bin and passed
pxi as an argument. Needs `-p`. Verified the corrected form launches and
reaches the credential check.

PXI defaults to an Anthropic model, so a reader who follows step 1 and sets
only OPENAI_API_KEY opens the assistant and hits "Anthropic credentials are
not configured". The step now says to pick a model matching the provider you
configured. Confirmed by switching to gpt-5.5 and getting a correct answer
about project span counts.

Restores "Where your data goes" under Privacy and tracing. It was dropped with
the architecture section, but it is not architecture: it is the answer to
whether data leaves your instance. No separate Arize service, your provider
called with your key, Arize not in the request path, and remote export gated
three ways. That is the first thing a security review asks about a beta AI
feature.

Also: the provider list mixed three env var names in code with "AWS Bedrock"
in prose, so it now names providers throughout; "Drives the product" lost its
object when I trimmed it; and the roadmap milestone link is out, since a
GitHub milestone goes stale.

Verified live: the consent gate appears as described, trace saving shows
"Disabled by system settings" which matches the documented default, the model
list matches the validated set, and PXI answered using search, get_schema, and
execute, the code-mode surface the page describes.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the CLI page its own job, running Phoenix from the terminal

The page claimed px was "the recommended way for a coding agent to work with
Phoenix", then documented only px setup, which Observe already owns. So it
repeated onboarding and never showed an agent working with Phoenix. Meanwhile
px has 17 top-level commands and the page covered one.

Rewritten around the debugging loop the phoenix-cli skill actually drives:
find what broke, read one trace in full, record the judgement as an
annotation, inspect the datasets and experiments you test against, then work
on the prompt. Plus a table for the remaining commands. Setup is now a single
line pointing at Observe.

Every command was run against a live Phoenix, which caught three errors in my
own first draft:

- `px span list` has no `--filter`. It takes typed flags, so the examples use
  `--status-code ERROR` and `--span-kind LLM`.
- `px experiment list` requires `--dataset`.
- `px trace get` resolves the default project and reports "Trace not found"
  without `--project`.

Verified working: trace list, trace get, trace add-note, span list with both
filters, span annotate \(annotation upserted\), dataset list, dataset get,
experiment list, prompt list, project list, and all 11 commands in the table.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: lead the agents overview with the setup table

Move "Set up your agent" above the three-piece cards so the
per-agent map comes first, and make its lead-in name the three
pieces so it stands on its own.

Narrow the endpoint rule: the plugins and the px CLI take the base
endpoint and append /mcp themselves, while `claude mcp add` and the
per-client MCP steps take the full /mcp URL.

Reword the prompt samples as examples rather than the full set.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the agents overview one job, shorten the MCP page

The overview page was mostly navigation: three cards pointing at CLI,
MCP Servers, and Skills, which already sit next to it in the sidebar,
plus a setup table routing to the same three pages. The sidebar label
"Overview" also repeated the group name, so the page read as a section
divider.

Now it does one thing, connect a coding agent to Phoenix: what this is,
the disambiguation note, the per-agent setup table, the endpoint. The
three cards are gone, the prompt samples are cut from five to two, and
the node_modules section is dropped since the TypeScript SDK overview
already documents it. Relabel the sidebar entry "Set Up Your Agent".

Correct the Claude Code row: the plugin ships three skills, while
px setup skills installs five, so the plugin path needs px setup skills
too. Verified by listing the plugin and running both.

On the MCP page, lead Connect with px setup mcp, merge the Cursor,
VS Code, and OpenCode accordions \(their JSON matches what px writes\),
replace the six Docs MCP accordions with a client table, and merge the
identical npm stdio configs. 633 to 521 lines with no facts dropped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: move the plugins onto the setup page

The setup page was still an index: its table pointed at Install
sections that CLI, MCP Servers, and Skills each already have. What it
lacked was the thing that belongs to no single one of them, the
one-install-per-agent path.

That path was the Plugins section, 137 lines and the largest section on
the MCP page, which is the wrong home for a plugin that registers MCP
and skills together. Move it here, after the endpoint it references,
and leave a pointer on the MCP page.

MCP drops to 388 lines and covers only MCP servers. The setup page now
routes by agent, states the shared endpoint, then gives the full plugin
steps.

Also repoint skills.mdx at the plugins section's new home, and fix its
link to a CLI heading that the CLI rewrite removed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the MCP page opener

It said "two things you might want your assistant to do" and "run
both" above a three-row table. Name the two servers that matter, your
data and the documentation, and say what the third row is.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: cut duplicated prose from the MCP page

The Configuration table repeated two env vars that self-hosting
configuration already documents; point there and keep only
PHOENIX_SKILLS_PATHS, which is unique to this page.

Monty sandbox limits were spelled out twice, in How it works and again
in Security; state them once. Merge the code-mode Warning and Note,
which both said the same thing. Trim the OAuth paragraph. Move the
OAuth2 authorization-server paragraph out of "Mounting your own
skills", where it was misfiled, into Security. Cut the npm sample
questions from seven to three.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: clarify the direction of the agents note

"Operate on Phoenix" is jargon, and "trace your sessions with a coding
agent" reads either way. Say which direction each page covers: this one
connects the agent to Phoenix, the other has Phoenix record the agent.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: correct two plugin commands that fail as written

Both found by running them. claude plugin uninstall acts on user scope
by default, so it stops with a scope error on a plugin installed at
local or project scope; say to pass the scope, and add disable, which
has the same rule. claude plugin details reports a disabled plugin as
not found, so say it has to be enabled, and say what the command lists.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: correct two CLI table rows that overstated what the commands do

Found by running them. px api has one subcommand, graphql, and rejects
mutations, so "authenticated requests to any Phoenix API route" is
wrong. The three annotation commands take only delete, and require
--all or a time window, so "stored annotations" hid that they are
destructive.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make the CLI debugging loop match what px can do

Steps 4 and 5 were titled "Turn failures into a test set" and "Work on
the prompt", but every command under them only reads, and the bodies
said "Inspect", contradicting the titles. px has no way to create a
dataset, add examples, or edit a prompt: for datasets, prompts, and
experiments it offers list, get, and delete only.

Cut the loop to the three steps px actually supports, ending at the
annotation, which with notes is the only thing it writes. Move the
dataset, experiment, and prompt reads into their own section framed as
looking up context, and say plainly that authoring happens in the UI
or the SDK.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rewrite the CLI page as the workflow an agent runs

The page had a "debugging loop" that was not a loop: it ended at an
annotation, and px cannot fix anything. Reorganising it by capability
then made it a feature list. Put it back in order as the pass an agent
actually runs, and let step 4 say plainly that px stops there and the
fix goes in your code, which is what closes the loop.

The examples were the weaker half. They repeated a fake project four
times, used placeholder ids that cannot be run, and carried comments
that restated the command instead of the output. Use real ids, say
which command feeds the next, and split the dataset, experiment, and
prompt reads into their own section, since those are for checking a
change rather than part of the debugging pass.

Add three facts found by running the commands: reads accept --format
json, annotate and add-note want the OpenTelemetry id rather than the
opaque one, and --identifier makes a repeat call overwrite.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: note that px annotation deletes are gated off by default

px span-annotations delete answers "Delete commands are disabled."
before it looks at any flag. Name the variable that opens the gate,
PHOENIX_CLI_DANGEROUSLY_ENABLE_DELETES, so the row does not send a
reader at a command that refuses to run.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say who runs px, and that the docs MCP URL is not a page

The CLI page opened by saying px is how a coding agent works with
Phoenix, then showed a reader commands to type, and titled the section
"How an agent uses it". A reader could not tell whether the commands
were theirs to run. Say it is an ordinary CLI you run yourself, and
that needing only a shell is what makes it easy to hand to an agent.

The Phoenix Docs MCP URL sat in a bare code block, so it read as a link
to open. A browser shows the tool manifest as JSON, which looks like an
error and is not. Say it is an endpoint for a client.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: answer px versus MCP, and add agent instructions to both pages

Neither page said when to reach for which. That choice only existed as
cards on the setup page, which is not where someone deciding ends up.
The CLI page now compares the two on what the client must support, what
setup costs, and how much surface each exposes, and the MCP page points
at it.

Both pages end with a section addressed to an agent reading them,
covering how to discover flags, which output format to parse, the
OpenTelemetry id the annotate commands want, the execute sandbox limits
and import allowlist, and that trace contents are untrusted data.

Every claim in both sections was run first: 116 catalog operations, the
50-call cap, json/re/datetime importable and socket/subprocess/importlib
not, exit status 0 and 1, and the delete gate.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rebuild the CLI page to a fixed outline, and align sibling names

The page now follows one order: what it is and why it matters, CLI or
MCP server, set up, what you can do with it, learn more, agent
instructions. Setup is three steps ending in a check. The examples
keep their real ids and drop the repeated --project in favour of
PHOENIX_PROJECT, set once in step two. The command table is gone; the
two caveats it carried, the delete gate and read-only GraphQL, live in
the agent instructions, and the full list is a link to the reference.

An audit of how each page in the section names its siblings found six
mismatches: the CLI page linked as "px CLI" and as bare "MCP", the
Remote MCP Server as "server", and a PXI section by a title it does
not have. Each link text now matches the page or section it points at.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: name the CLI setup check by what it proves

"Check it" said nothing. The step confirms the CLI reached the
instance, so say that, and say what success looks like.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say what the CLI setup is for before the steps

The section went straight from the heading to step one, so a reader
could not tell what was being set up or where. Name the CLI in the
heading, say to install it where the agent runs, and say what having
finished looks like.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say what px project list returns on a fresh instance, drop "has a terminal"

The setup check said a table of your projects means success, which
leaves a reader with no traces unsure what to expect. Tested on an
empty instance: every Phoenix has a project named default, so the
command always returns at least one row, and a wrong endpoint fails
with "Error fetching projects: fetch failed". Say both.

"When your agent has a terminal" was jargon on the CLI and MCP pages.
Say what it means: the agent can run shell commands, which every
coding agent can.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the CLI-first recommendation, say what the default project is

"Start with the CLI" was a preference with nothing behind it, and it
contradicted the setup page, which sends Claude Code and Codex users
to the plugin first, and the MCP page, which calls itself the primary
choice. The comparison table carries the facts; let the reader choose.
The MCP page mirror is now a plain pointer.

The setup check mentions the default project. Say what it is: created
by the initial migration, the destination for traces sent without a
project name, and empty on a fresh instance. Not a demo.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: make the CLI page work for a user with no traces yet

Run as a brand-new user, in a clean environment against an empty
instance, the previous page failed at step two: PHOENIX_PROJECT=my-app
404s on every read, and every example carried an id from one machine.
Stuff has to work before a project has traces.

Follow the shape of the live CLI section: install, endpoint, verify
with px --help and px project list, which both succeed with zero data,
then the common agent workflows as bullets and one example prompt, so
the reader sees what to ask rather than what the agent types. Say that
PHOENIX_PROJECT comes later, once traces are flowing. Keep the CLI or
MCP table and the agent instructions, and add the missing rule that
read commands need a project.

On the MCP page, restore the per-client tabs for the Docs MCP from the
live site, adding Claude Desktop.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: one story across Arize Phoenix for Agents, working from zero traces

The section was organised by plumbing, CLI, MCP, Skills, and every page
assumed traces already existed. A new user has none. Reframe around
what is true from the start: connect the agent, let it instrument your
app, then debug from what Phoenix records.

Set Up Your Agent owns routing, the endpoint, and the plugins. Its
connection check is a question to the agent, list my Phoenix projects,
which a fresh instance answers with default. Verified through both
pipes on an empty instance: px project list, and MCP search finding
getProjects and execute returning it. A new section points at the
agent-assisted tracing path as the next step.

CLI, MCP Servers, and Skills share one skeleton: what it is, install
ending in a check, what your agent can do with it, learn more,
reference. Deep material on the MCP page, API-key fallback, code mode
internals, server configuration, security, the npm server, sits under
Reference. The MCP Endpoint section folds into one line. The skills
page moves its options table and custom-skills guidance under
Reference and gains an example prompt.

Every shell command on the four pages was run in a clean environment
against an empty Phoenix. Restore the remote-mcp-server anchor, which
eleven pages link to, and repoint two onboarding snippets that still
named a CLI heading removed in an earlier rewrite.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut the instrumentation pointer back to one line

It was a sentence before, and a heading with three sentences after. The
link is the useful part.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: nest the MCP capabilities section under Remote MCP Server

The page covers two servers, so "what your agent can do with it" as a
top-level heading left "it" ambiguous. Make it an H3 under Remote MCP
Server, alongside a new Connect H3 for the setup steps.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut duplicated prose from the MCP page

The intro paragraph restated the table under it. The capabilities
section under Remote MCP Server repeated Reference, How it works,
nearly sentence for sentence; point there instead. Three sentences of
OAuth mechanism sat ahead of the first command; one line after the
accordions covers what a reader needs. The Docs MCP intro restated its
table row, the npm section said maintenance mode twice, and the
mounted-skills paragraph named its variable twice in a row.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: link the CLI page back to Set Up Your Agent

The MCP and Skills pages both point back to the setup page; the CLI
page did not, so a reader who landed there directly had no route to
the per-agent install.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: frame PXI skills as example capabilities

The table led with skill names, which is the mechanism. Lead with what
you can ask PXI to do, give one example ask per row, and keep the skill
concept as a single sentence after the table.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: frame PXI models as bring your own, and trim

Lead with the point: PXI runs on your provider and your key. Name the
providers once, keep the validated-model list and the tool-calling
caveat, drop the rest. Repoint the two links that used the old anchor.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the agent-instructions section from the CLI page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the agent-instructions section from the MCP page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the validated-model list from PXI

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: stop promising a supported-models list PXI no longer has

The list was removed; three references still pointed at it. Retitle the
Get Started link to the section that exists, and say model providers
where the Learn more line and the page description said supported
models.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: remove the npm MCP server from the MCP page

Drop the maintenance-mode section, its row in the intro table, and the
three places that offered it as a fallback: the version-floor line, the
Claude Desktop accordion, and the troubleshooting entry.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: act on the docs review: skills guidance, Docs MCP callout, MCP cut-down

Skills page: name the five skills written for Phoenix users, add the
plugin-versus-skills-add guidance from the review \(plugins track
Phoenix releases; skills add is a one-time copy, rerun it or run npx
skills update\), and take ownership of mounting your own skills from the
MCP page. Document --all honestly: tested in a fresh project it
installed 26 skills to 73 agents, including internal development
skills, so it is the wrong default.

MCP page: Phoenix Docs MCP becomes a short block near the top, as the
review asked, in place of an 87-line section. Fold Antigravity into the
other-clients accordion, trim How it works to the tool table plus two
sentences, and drop the mounting section now that Skills owns it.
361 to 271 lines.

Evaluate Your Agent: move the spans screenshot out of the Note it was
nested in.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: bring llms.txt in line with the Get Started and agents reorg

Add the CLI, Skills, and PXI Learn More pages, which had no entries.
Remove seven Quick Start entries for pages the reorg deleted or merged
into Observe Your Agent and Evaluate Your Agent, and three MCP entries
for pages folded into the single MCP page. Retitle the survivors to
match their pages.

Add the agent-usage guidance for the px CLI and the Remote MCP Server
here, where the review said it belongs, instead of on the human pages.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: give the CLI guide entry in llms.txt a unique title

It collided with the @arizeai/phoenix-cli reference entry.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten the endpoint rule on the setup page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: halve the agent-assisted setup on Observe Your Agent

Start Phoenix once above the tabs instead of in each. One sentence of
outcome per tab. Cut the pasted prompt to the two instructions that
matter, since the tracing skill carries the procedure. Drop the agent
list and the CLI pointer, which the setup page owns.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: add the PXI path to Evaluate Your Agent

A prompt to hand PXI once the dataset exists and the Playground has it
selected. Both preconditions come from running it: PXI cannot fetch a
URL, and its playground and evaluator skills act on the page you are on.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: restore the agent-assisted shape on Observe, trimmed

Back to the two tabs as they were, each self-contained, with the full
prompt. Merge the px setup outcome into one paragraph. Replace the
named --skill list with px setup skills, which installs exactly the
five Phoenix skills for every detected agent: tested via npx in a fresh
project with no CLI installed.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: repoint a PXI conversations link to the Learn More page

The Conversations section moved there when PXI split into Get Started
and Learn More; one link still pointed at the old page.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: Evaluate Your Agent builds a reference-aware evaluator

Built-in LLM templates have no reference field and score the baseline
higher than the fixed prompt, so step 2 now walks through Create new
LLM evaluator with a short rubric and the reference path mapping. PXI
section hands off after the dataset and evaluator exist, since it
cannot fetch the CSV and its own evaluator scores 0 at run time.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten Evaluate Your Agent evaluator step and PXI hand-off

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: PXI runs the full Evaluate flow from one prompt

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten Evaluate Your Agent prose

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut PXI Learn More to the essentials

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: one-line rows in the PXI capabilities table

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: shorten PXI privacy and tracing

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: compact PXI configuration table

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: shorten CLI page intro

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten MCP Servers prose

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop em dashes from Get Started snippets

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: add the follow-up prompt to the coding agent path

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: make px setup skills the primary install on the Skills page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say where the model API key goes before creating an LLM evaluator

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: the key must be on the Server tab for evaluators to use it

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say when to pick the CLI and when the MCP server

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: CLI vs MCP as best-when guidance

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop redundant line from CLI or MCP section

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: plugin rows say what the plugin covers and what is optional

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop Docs MCP aside from Set Up Your Agent

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

*…")[#16288](https://github.com/Arize-ai/phoenix/pull/16288)[)](https://github.com/Arize-ai/phoenix/commit/2afdf488e638f282f742c7daf796a09a56662971 "docs: reorganize Get Started around the agent workflow \(#16288\)

* docs: reorganize Get Started around the agent workflow

Restructure the Get Started section to mirror the Arize AX quickstart:
Instrument your agent, Evaluate your agent, Improve your agent, Coding
agent setup, PXI, and Phoenix Demo.

The Python and TypeScript quickstarts previously sat in separate nav
groups, so each language duplicated the whole section tree. Each pair is
now a single page with Python and TypeScript tabs, so the language is a
choice within a section rather than a parallel branch of the nav.

Every page body is carried over verbatim into its language tab. The only
text rewritten is internal links that pointed at the retired
ts-get-started-* paths, which now have redirects to the merged pages.

- Instrument your agent  <- get-started-tracing + ts- counterpart
- Evaluate your agent    <- get-started-evaluations + ts- counterpart
- Improve your agent     <- group over the prompt playground and
                            datasets/experiments guides
- Coding agent setup     <- group over Agent-Assisted Setup and Coding
                            Agents
- MCP group, PXI, and Phoenix Demo are unchanged aside from ordering
- End to End Features Notebook is removed from the nav; the page itself
  is left in place

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: cut duplicated prose from the Get Started guides

The previous commit put each language's full guide in its own tab, which
made every language-agnostic sentence render twice on the same page. On
the prompt playground guide that was 37% of the prose; on datasets and
experiments, 40%.

Where the two tabs told the same story, the narrative is hoisted above
the tabs and the tabs now wrap only what genuinely differs: code, videos,
the companion-code callout, and the two prompt snippets. The prompt
playground and experiments guides had identical step sequences, so their
steps are shared outright. Tracing and evaluations each have an extra
TypeScript step, so their steps stay tabbed and only the intro, prereqs
and closing sections are shared.

No prose was rewritten. Every line either survives verbatim from the
previous commit or is a prefix of one with a redundant trailing clause
removed, verified mechanically. Where the two tabs differed only
trivially, one existing sentence is kept verbatim rather than a new one
written; this also picks up several corrections the TypeScript copy
already had:

- "traces that received an incompleteness evaluation label" ->
  "an incomplete evaluation label", matching the filter beside it
- "Filter your traces by the incorrect evaluation label" -> "incomplete";
  the filter under it has always been evals\['completeness'\] == 'incomplete'
- "Using the explanations apart of the evals" -> "from the evals"
- "New application runs as a results of our task" -> "as a result"
- "ran your first experiment" / "ran an A/B test" -> "run"

Also drops, on the tracing guide, a sentence restating the bullet list
above it and three repeats of the supported-frameworks disclaimer that
the intro already carries.

Verified: all 44 code blocks byte-identical \(multiset\), every step title
and video retained, no duplicate sentence in any rendered page, both link
checkers clean, all four pages render with working tabs.

Words: 10790 -> 8789 \(18.5% shorter\).

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: tighten Instrument and Agent-Assisted Setup, repoint quickstart

Three changes to the Get Started section.

Instrument your agent opened with a paragraph of series framing --
"This guide walks through a complete workflow ... Each guide in this
series introduces one piece of that workflow" -- which is word for word
the Overview page's opening and says nothing about instrumentation. It is
removed here and still lives on the Overview page. The guide now opens on
its own subject: what a trace is.

/docs/phoenix/quickstart redirected to the Overview page, which is not in
the nav. It now lands on Instrument your agent, the actual first guide in
the series. The Overview page is unchanged and still reachable from the
four pages that link to it.

Agent-Assisted Setup is only ~350 words of prose but 89 lines, because
almost two thirds of it is follow-up reference rather than setup. The
three advanced sections -- re-running a single step, non-interactive runs,
and unsupported agents -- move into an AccordionGroup, so the page reads
as an intro, two steps and a verification section. Nothing is deleted:
all five code blocks are byte-identical and the collapsed text is still
in the page. The one prose deletion is "The fastest way to add Phoenix
tracing to your application.", which restated the frontmatter description
rendering directly above it.

Trade-off worth noting in review: collapsed sections leave the "On this
page" table of contents, which now lists only "Confirm traces are
flowing".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: hold every Get Started intro to four sentences or fewer

The intros restated themselves. Evaluate your agent opened with seven
sentences making three points: what an eval is \(said three times\), what
the guide does \(said twice\), and which metrics you can score against.

Deletions only, no rewriting. Every surviving sentence is verbatim.

Evaluate your agent, 7 -> 4. Dropped:
- "Evaluations attach quality signals to runs so that correctness or
  relevance can be reasoned about consistently instead of judged case by
  case." Restates the opening sentence and the traces/evals contrast that
  follows it, and states it as a negative.
- "We'll start with data that already exists in Phoenix, define a simple
  evaluation, and run it so we can see results directly in the UI."
  Restates "In this guide, you'll set up evaluations ... and run them on
  existing trace data".
- "The goal is to move from "I have model outputs" to "I can measure
  quality in a repeatable way."" Restates the opening sentence.

Iterate on Your Prompts, 5 -> 3. Dropped "Up to this point, we've traced
our agent runs and evaluated their outputs. Now we'll focus on prompts by
grouping failures into a dataset and iterating on prompt variants in the
Prompt Playground." -- the sentence above it already says the guide
groups failing runs into a dataset and iterates in the Playground, and
the prereq list directly below states the same continuity as two bullets.

Instrument your agent, TypeScript tab 5 -> 4. Dropped "In this guide,
we'll get tracing set up and walk through how to instrument an
application.", which the sentence after it says with the specifics. The
Python tab already carried this as one sentence.

Counts are per reader, shared intro plus their language tab, excluding
callouts: Instrument 4, Evaluate 4, Iterate 3, Optimize 3,
Agent-Assisted Setup 1.

Code blocks unchanged on all three pages.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say "agent" instead of "app" across Get Started

Refers to the reader's system as an agent throughout, matching the
section titles \("Instrument your agent", "Evaluate your agent"\). 51 lines
across the five guides plus the Overview page. Code blocks and URLs are
untouched; the `js/examples/apps/mastra-quickstart` paths and every
identifier keep their spelling.

The experiments guide's title becomes "Optimize Your Agent with
Experiments", so the nav label changes with it.

Two phrases needed care rather than a plain swap:

- "an agent application" would have become "an agent agent"; it is now
  just "an agent".
- "the agents that make up our application" would have become "the agents
  that make up our agent"; it is now "our agent system", the phrasing the
  experiments guide already uses.

Agent-Assisted Setup keeps "app" for the reader's codebase. That page is
about handing work to a *coding* agent, so "let your coding agent add
tracing to your agent" reads as one thing instrumenting itself. The
collision is worse than the inconsistency.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: merge the prompts and experiments guides into Improve your agent

"Iterate on Your Prompts" and "Optimize Your Agent with Experiments" were
two halves of one workflow split by *how* you make the change rather than
by what you are doing. Both started from eval-labelled failing traces,
both ended in the same Experiments view, and each pointed at the other as
the next step, so a reader could not tell which came first.

They are now one guide, matching the AX quickstart, which has a single
"Improve your agent" page with method tabs rather than a group.

Four steps, with the method choice appearing once instead of at every
stage:

1. Create a dataset from failed traces      \(shared\)
2. Use explanations to identify improvements \(shared; was taught twice\)
3. Make the change and run it   -- tabs: In the Prompt Playground /
   In code, with Python and TypeScript nested inside each
4. Compare versions in the Experiments view  \(shared narrative\)

Nothing is dropped. Verified against both source pages: 13 of 13 code
blocks, 12 of 12 videos, 12 of 12 frame captions, every numbered
instruction, and every H3 section.

Deliberate changes beyond the merge:

- The two cross-links between the guides are gone; they would now point
  at this page. That circularity was the reason to merge.
- "the first from Step 3" is now "the first from your baseline run",
  because the step it referenced no longer has that number.
- "A dataset created from previous runs" leaves the prerequisites, since
  step 1 builds it.
- The two "Congratulations" lines and the two "Learn More" sections are
  one each.
- About eight new connective sentences were written, the only prose on
  this page that is not carried over verbatim. Interleaving two
  workflows needs them.

Seven URLs now redirect here: both retired page paths, both ts- paths,
and the three legacy quickstart paths.

docs/phoenix.mdx had a card for each guide in both its Python and
TypeScript groups; each pair is now one "Improve Your Agent" card.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rewrite Get Started in second person, direct voice

The guides narrated themselves in the first person plural -- "we'll
install", "now that we have trace data, we can", "at this point we've
defined" -- which reads as tutorial filler and contradicts the house rule
that guides use active voice and second person. 155 instances of that
register across the section, now 0:

  royal "our"                   55 -> 0
  royal "we'll" / "we're"       44 -> 0
  "we can"                      26 -> 0
  "At this point,"               7 -> 0
  "Next, we..."                  6 -> 0
  "In this step, we..."          6 -> 0
  "Now that ..., we"             4 -> 0
  "let's"                        5 -> 0
  "More importantly,"            2 -> 0

Instructions are now imperative and the prose states facts about the
system rather than narrating a shared journey. "Next, we'll register a
tracer provider linked to a project in Phoenix. This project is where
your traces will show up" becomes "Register a tracer provider linked to a
project in Phoenix. That project is where your traces show up."

Two errors fixed on the way through:

- "seen how an agent runs shows up from start to finish" -> "how an agent
  run shows up"
- "our finanical writer agent" -> "The financial writer agent"

No instruction was dropped. Code blocks, videos, link destinations, step
titles and H3 sections are identical on all three guides: 17/2/13/9/14 on
tracing, 14/2/9/9/5 on evaluations, 13/12/5/4/9 on Improve your agent.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: sharpen the tracing opener and drop every em dash

The tracing guide opened with a sentence that did not parse: "A trace is
a record of a single run of your agent, broken down into spans that show
what happened at each step \(how agents, tasks, and tools executed\) and
provides the raw data needed for everything that follows." After the
participial phrase, "and provides" cannot reach back to "is a record", so
it reads as attaching to "spans", which would need "provide". It also
said the same thing twice \("what happened at each step" and "how agents,
tasks, and tools executed"\), and closed on "the raw data needed for
everything that follows", which was left over from the series framing
that now lives only on the Overview page.

Replaced with the house formulation from ax/instrument/what-are-traces
and phoenix/tracing/concepts-tracing, which also defines a span instead
of gesturing at it:

  A trace captures one end-to-end run of your agent as a tree of spans.
  Each span is a single step \(an agent, a task, a tool call, or an LLM
  call\) with its input, output, and timing.

The page also picks up a frontmatter description, matching the AX tracing
guide. It renders as the subtitle under the H1, so it carries the framing
while the body carries the mechanic, and the two do not restate each
other.

Em and en dashes are now zero across the section. Two were mine; two were
pre-existing on Agent-Assisted Setup, where a dash joined two independent
clauses that are clearer as separate sentences.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: stop repeating the prerequisites, and drop the long reference from Get Started

The evaluations guide stated its prerequisites twice, about fifteen lines
apart, in different words. At the top: "you'll need to have completed Get
Started with Tracing so you should have: Financial Analysis and Research
Chatbot / Trace Data in Phoenix". Then again inside step 1: "Evaluations
in Phoenix run over existing trace data. If you followed the tracing
guide, you should already have: A project in Phoenix / Traces containing
LLM inputs and outputs". Both tabs carried it, so a reader met the same
list twice per pass.

The step's version was the more precise one, so its detail is folded into
the single list at the top, which now reads "A project in Phoenix holding
traces with LLM inputs and outputs". Step 1 keeps the one fact it added,
that Phoenix evaluations run over existing trace data, and loses the
duplicate list.

Worth noting for future passes: string-similarity scanning did not catch
this, because the two lists share almost no wording. Semantic repetition
needs a read, not a regex.

The Overview page repeated itself inside single paragraphs the same way:
"using the same inputs and evaluation criteria" in two consecutive
sentences, and "spans that show what happened at each step" followed by
"Traces show how agents, tasks, and tools executed during a run". Both
are now stated once.

Coding Agents leaves the Get Started nav. It is 466 lines, 2258 words and
27 code blocks of reference material for Claude Code, Codex, the CLI, MCP
and skills, and its own opening line says "If you haven't instrumented
your app yet, start with Agent-Assisted Setup" -- it is the step after
getting started, not part of it. The page is unchanged and still listed
under Integrations > Developer Tools, and Agent-Assisted Setup already
links to it for ongoing workflows.

That leaves one page in the group, so it collapses to a single nav entry.
Agent-Assisted Setup gains sidebarTitle "Coding agent setup" so the label
still matches the agreed structure, the same way pxi.mdx shows as "PXI
Agent".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix five accuracy defects found by checking Get Started against the source

Verified the guides against the Phoenix source in this repo rather than
against other docs. Five real defects, all inherited from main:

1. `evals\['completeness'\].label == 'incomplete'` is a **span** filter. The
   guide said "Filter your traces by the incomplete evaluation label",
   which points at the traces table, where the expression is rejected:
   src/phoenix/trace/dsl/trace_filter.py refuses it with "`evals\[...\]` is
   not available in the trace filter". docs/.../filter-expressions.mdx
   lists it under span filters, and the UI has separate SpanFiltersContext
   and TraceFiltersContext. Now reads "Filter the spans table".

2. and 3. The add-to-dataset flow is span-based, not trace-based:
   AddSpansToDatasetInput carries `span_ids`
   \(src/phoenix/server/api/mutations/dataset_mutations.py:225\), and the
   dataset guide says "select the span". "Select the traces you want to
   include" and "Add the selected traces" now say spans.

4. `client = Client\(\)` ran with no import on this page. The evaluations
   guide imports it, but this page has to stand alone, so it now carries
   `from phoenix.client import Client`.

5. `import { getDataset } from "@arizeai/phoenix-client/datasets"` was
   never used. The companion example the guide points at,
   js/examples/apps/mastra-quickstart/src/mastra/experiments/experiment.ts,
   imports the identical set without it. Removed.

Everything else checked out against source: register\(project_name,
auto_instrument\), LLM\(provider, model\), ClassificationEvaluator\(name, llm,
prompt_template, choices\) and .bind\(\), Client\(\).datasets.get_dataset\(dataset=\),
Client\(\).experiments.run_experiment\(dataset=, task=, evaluators=\),
suppress_tracing's import path, the phoenix-client and phoenix-evals TS
exports, runExperiment accepting a {datasetName} selector, ports 6006 and
4317, exit code 6 as NOT_VERIFIED, every `px setup` subcommand and all
nine documented flags, and both companion code paths.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: one language choice on Improve your agent, not ten

The page had accumulated 10 separate Python | TypeScript pickers down its
length: one on the companion-code callout, one per video pair, and one
inside each half of the method tabs. Mintlify syncs tabs that share
labels, so picking a language once did switch all of them, but rendering
the control ten times reads as ten decisions.

Language is now a single choice at the top, the same shape the tracing
and evaluations guides use. Method tabs \(In the Prompt Playground /
In code\) stay where they were, inside step 3 and step 4. A reader now
sees one language picker and two method pickers instead of twelve
controls.

This is purely structural: every Python | TypeScript group was resolved
to the chosen language and the whole body wrapped in one pair. No prose
was rewritten. Verified against the previous version: all 13 code blocks,
12 videos, 12 frame captions, every numbered instruction, every heading
and every step title still present.

The cost is 78 lines. Language as the outer axis means the
language-agnostic narrative -- the dataset steps, the explanation step,
the shared prose in steps 3 and 4 -- exists once per branch. That is why
the step count in the file doubles from 4 to 8 while a reader still sees
4. Worth it for one obvious choice at the top; the alternative was
keeping the page shorter and the navigation confusing.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: method first, language inside it, on Improve your agent

Corrects the tab hierarchy to match ax/get-started/get-started-tracing,
where the outer tabs are methods \(By npx evals / By Arize Skills / By
Code\) and the Python | TypeScript picker sits inside By Code. The previous
commit put language on the outside, which was the wrong axis.

A reader now picks the method first \(In the Prompt Playground / In code\),
then the language within it. Two controls, in that order.

Also tightened the prose, since with tabs every shared line exists in more
than one branch and each cut lands more than once. Deleted as restatements
of the line beside them:

- "This dataset now represents a concrete failure case for your agent."
- "Start by grouping together the traces that didn't perform well."
- "Start from a real prompt the agent actually used." -- the next sentence
  already gives "a real starting point".
- "Saving prompt variants makes it easy to track changes..."
- "An experiment reruns the same inputs through different versions..." --
  the page intro already defines an experiment; this was the third time.
- "With two versions in hand, compare them..." -- restated the step title.

Shortened: the two-paths lead-in \(the tab labels say it\), the
create-datasets aside \(said "in code" twice\), and the task and evaluation
definitions. Fixed two royal "we" the voice pass missed: "the dataset we
made earlier" and "we should see more traces".

Line count: 605 -> 581 from the trims, then -> 694 from the inversion.
The growth is real and worth naming. Steps 1 and 2 -- create the dataset,
read the explanations -- are method-agnostic, so method-outer copies them
into both method branches, and language-inner copies them again. Four
copies of about thirty lines. A reader still sees each step once.

The wrinkle that buys: the method choice now sits above two steps that do
not depend on it, so a reader picks Playground or code before creating the
dataset. The alternative keeps steps 1 and 2 shared and moves the method
choice down to step 3, at the cost of a second visible language picker.

Verified against the previous version: every code block, video, caption,
link, heading and step title still present.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: move the companion-code callout out of the no-code path

The Prompt Playground branch offered "a companion notebook with runnable
code examples" in a path where you write no code. That was a side effect
of wrapping the whole body in the method tabs. The callout now appears
only under In code, for both languages.

The Python | TypeScript picker stays inside the Prompt Playground branch,
because that path does depend on which language you followed -- not for
the language itself, but for what you built with it. Six of the videos
record the CrewAI app rather than the Mastra app, and the dataset input
syntax differs with the agent: `{tickers}` and `{focus}` are CrewAI task
variables, `{{input}}` is Mastra. The label is a proxy for "which
quickstart did you follow".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: drop the horizontal rules bracketing the companion-code callouts

Every companion-code callout in Get Started was wrapped in `---` rules,
inherited from main. Each renders as a thematic break with generous
margins, so the callout sat in a band of empty space with a thin line
above and below it. The callout already has its own bordered container,
so the rules added separation that was not needed.

12 rules removed: four on the tracing guide, four on evaluations, four on
Improve your agent. Frontmatter delimiters are untouched, including the
multi-line description block on the tracing guide, whose closing `---`
sits at line 6 and is easy to mistake for one of these.

No prose changed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the playground template syntax, which would not have interpolated

The Python path told readers to set the User prompt to `{tickers}` and
`{focus}`. That is F_STRING syntax, and the Prompt Playground defaults to
Mustache:

  js/app/src/components/templateEditor/constants.ts
    FString:  "F_STRING",  // {variable}
    Mustache: "MUSTACHE",  // {{variable}}

  js/app/src/store/playground/playgroundStore.tsx:273
    templateFormat: TemplateFormats.Mustache,

Playground.tsx reads the format from that store, so with default settings
single braces are not substituted -- the prompt would have carried the
literal text `{tickers}`. The guide never mentioned the format selector,
so there was no way for a reader to know.

Most likely this was copied from the CrewAI task template on the tracing
guide, `Task\(description="Research: {tickers}"\)`, which legitimately uses
single braces. That is CrewAI's own templating, not Phoenix's, and it is
left alone.

Both paths now use Mustache, and the shared lead-in says so, so the two
language tabs differ only in the variable names.

The variable names themselves were already correct and are framework
driven, traceable in the guides' own code: the Python experiment passes
`example.input` straight into `kickoff\(inputs=...\)`, so dataset examples
carry `tickers` and `focus` keys, while the TypeScript task types
`example.input` as a `{role, content}\[\]` message array under a single
`input` key.

Verified rendered: the block shows `Research: {{tickers}}` literally, and
the format note appears in both language tabs.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: expand the Agent-Assisted Setup accordions back into headings

I collapsed three sections into an AccordionGroup when the page read as
too long. That hid 307 of its 542 words, 57% of the page, and dropped the
three section names from the "On this page" table of contents, leaving one
entry. It also made the page look like content had been deleted, which is
what prompted the question.

The reason for collapsing them has gone: the length concern was mostly the
466-line Coding Agents reference, which no longer sits in this nav group.
What remains is ~330 words, the shortest page in Get Started, so hiding
more than half of it costs more than it saves.

Re-run a single step, Run non-interactively \(CI or agents\), and Use an
unsupported agent are `##` headings again, so all four sections appear in
the table of contents and can be linked to directly.

Code blocks and links are identical.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: no language picker in the Prompt Playground path

You write no code in the Prompt Playground, so asking the reader to pick
Python or TypeScript there was misleading. The picker now appears only
under In code, where it selects actual code, which matches
ax/get-started/get-started-tracing.

Two controls on the page, in this order:

  In the Prompt Playground | In code
    In code -> Python | TypeScript

What the picker had been carrying in that path, and where it went:

- Steps 1, 2 and 4 were already identical between the languages.
- The dataset input keys are real, so they are stated inline: which keys
  you have depends on the agent you traced, `{{tickers}}` and `{{focus}}`
  from the Python guide, `{{input}}` from TypeScript. Both forms are now
  written out rather than left to be inferred.
- The example prompt line keeps the Python variant, which is the one that
  matches the finding step 2 states \("the report lacked financial
  ratios"\). The TypeScript variant said something different, about
  per-ticker metrics.
- Five videos drop out: ts-first-dataset, ts-first-prompt,
  ts-playground-experiment, ts-compare-prompts, ts-view-experiment. They
  carried the same captions and demonstrated the same click-path as the
  five that remain, recorded against Mastra instead of CrewAI.

All 13 code blocks and every link destination are unchanged. 668 -> 538
lines.

Braces checked: every `{{...}}` on the page is either inside a fenced
block or an inline code span, so MDX treats them as text. Verified in the
rendered DOM.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: restructure Get Started nav and verify evals against the product

Reshape the Get Started nav to Observe Your Agent / Evaluate Your Agent /
PXI / Arize Phoenix for Agents / Phoenix Demo Account. Per-language pages
become tabs within each section, with language nested inside the code path.

Merge the Improve Your Agent guide into Evaluate Your Agent, which is now
written as offline evals: a dataset feeds a task, an evaluator scores each
output. Phoenix has not released continuous scoring of live production
traffic, so the previous framing was wrong.

Extract Skills from the Coding Agents page into its own page and group the
agent-facing docs \(agent-assisted setup, skills, MCP\) under Arize Phoenix
for Agents. A pointer heading keeps existing #skills anchors resolving.

Correct six claims against Phoenix 20.15.0 running locally:

- `output` is the task's output, not an example payload. `reference` is the
  example's expected output \(experiment_runner.py:831, message_helpers.py:279,
  and the compare view's "reference output" column\).
- Creating a dataset from spans is one action, not two. You select spans and
  click Add to Dataset; creating a new dataset happens inside that popover
  \(SpanSelectionToolbar.tsx:212, DatasetSelectorPopoverContent.tsx:104\).
- The evaluator dialog's submit button is Create, not Save.
- The playground control is "Test over a dataset".
- Experiment runs live on the Experiments tab, compared via the
  "comparisons" selector.
- Playground template format defaults to Mustache, so dataset input keys
  need double braces.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: remove em dashes from the Get Started nav pages

All 98 em dashes on pages now reachable from Get Started, none of them
authored by this branch: pxi.mdx \(31\), coding-agents.mdx \(22\), remote-mcp.mdx
\(14\), mcp.mdx \(4\), docs-mcp.mdx \(3\), phoenix-mcp-server.mdx \(1\), plus 14
bullet and label dashes replaced with colons.

Each was repunctuated in context rather than swapped for a single character:
a colon where a list or restatement follows, a comma for an aside, a period
where an independent clause follows, parentheses for the three parentheticals
that span hard-wrapped lines. No dash inside a fenced code block was touched.

Diff is 89 insertions and 89 deletions, so no prose was added or removed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: point Evaluate Your Agent at the trace-scoring flow

The merged page covers offline evals over experiments. Readers who want to
score traces they have already collected had no path to it from Get Started,
since that content now lives under Tracing and Evaluation. Links to
Evaluating Phoenix Traces and the two eval quickstarts.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: simplify Phoenix get started quickstarts

* docs: keep Agent, not App, in the Get Started flow

Restores "Observe Your Agent" and "Evaluate Your Agent" and aligns the
surrounding copy, which had moved to "app": both page titles and descriptions,
the section headings on the Get Started overview, the landing-page cards, and
two "Observe Your App" cross-links that would have pointed at a page with a
different name. `app.ts` stays, since that is a file the reader creates.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: one Quick Starts card group, without the Improve card

The Improve page merged into Evaluate Your Agent, so its card pointed at the
same page as the Evaluate card. Dropped it and folded its description into
Evaluate's, which had described only the LLM-as-a-judge half.

The Python and TypeScript groups held identical cards, and both target pages
now select language with tabs, so the split no longer carried information.
Collapsed to a single group. The redirect from the retired improve path is
untouched.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make Evaluate Your Agent a single Playground path

Drops the Prompt Playground / Code tab split and keeps the Playground steps,
so the quickstart teaches one path end to end. 401 lines to 146.

Adds a "Prefer to do this in code?" section that sends readers to Run
Experiments for the Python SDK and to the TypeScript experiments reference for
runExperiment, so both languages the removed tab covered still have a home.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say which traces to evaluate and what to measure

The steps described where to click without answering the two questions a
reader arrives with. Five tweaks, 146 lines to 151:

- Dataset step gives a size to aim for and two ways to find failing traces,
  a filter expression or asking PXI to walk them
- Evaluator step names error patterns worth scoring instead of only listing
  the two evaluator types
- Saving a prompt from a trace opens with why that prompt is the one to use
- The baseline is stated as the thing later edits are measured against
- Comparison step closes with how to read the delta, including what to do
  when the score drops

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say offline evaluators in the Evaluate description

Also removes two lines the single-path change left stale: an opening sentence
that restated the description, and a paragraph telling readers to choose
between the Playground and code paths, which are no longer tabs on this page.
The offline framing replaces it, with the caveat that continuous scoring of
live production traffic has not shipped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say the manual path builds everything from the Phoenix UI

Now that this page is Playground-only, the section intro should name the
surface the steps use.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: shorten the PXI line in the dataset step

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: restore the code experiment example as its own page

Making Evaluate Your Agent a single Playground path deleted Nancy's Python
and TypeScript worked examples, and the pages it linked to instead cover the
same APIs without that scenario. Recovered them verbatim from 966510c74 as
Evaluate a Prompt Change in Code, under How-to: Experiments.

All 7 code blocks and both 4-step sequences are byte-identical to the removed
tab; only the Tab wrapper was dropped and the block de-indented one level. The
page opens by tying itself to the quickstart, so the support agent scenario
carries across the UI and code paths again.

"Prefer to do this in code?" now points here first, with Run Experiments as
the wider reference.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the new experiment page headings its siblings have

The page had no H2s, so it contributed nothing to the table of contents while
every other page in How-to: Experiments has several. Adds a heading over the
tabbed walkthrough, notes that the script creates its own dataset so it runs
without traces, and points at Set a Baseline Experiment, which covers the
persistent baseline that this page's in-code comparison does not.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rework Observe and Evaluate, split PXI into Get Started and Learn More

Observe Your Agent: intro rewritten to say what the page does and where the
concepts live, `\(Recommended\)` on the agent-assisted path, and the manual
steps reordered to start Phoenix, get your keys, then set up tracing. The
install step folded into setup, and the three steps that were identical in
both language tabs hoisted above them.

Evaluate Your Agent: no longer depends on having traces. The dataset step
hands you three travel support examples to upload, so the page runs on a
fresh Phoenix. Traces became an optional route rather than the only one.
Switched to an LLM judge, added copyable Playground prompts for the baseline
and the edit, and dropped the save-a-prompt-from-a-trace step.

PXI: 3,315 words split into a 315-word Get Started \(what it is, what it does,
three steps\) and a 1,158-word Learn More grouped as capabilities, surfaces,
configuration, and privacy. Nothing deleted except the architecture diagram
and the flag and slash-command tables, which duplicate the CLI reference.
Accordions flattened so nothing is hidden behind a click.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: consolidate Arize Phoenix for Agents

Snapshot of the agents/MCP consolidation: agent-assisted-setup folded into a
new developer-tools/cli page, and remote-mcp, docs-mcp, and phoenix-mcp-server
collapsed into one integrations/mcp page, with redirects for all four retired
paths. Nav and inbound references updated across the CLI reference, release
notes, self-hosting, and the shared onboarding snippets.

Committed from the docs-get-started-reorg worktree so the two in-flight
branches can be merged.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: point the tracing quickstart at the new CLI page

The agents consolidation folded agent-assisted-setup into
developer-tools/cli. This was the last reference still relying on the
redirect rather than the real page.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: link the uploaded dataset, and cross-link the other Observe methods

The travel-support.csv download pointed at assets/datasets/, but the file was
uploaded to assets/images/arize-docs-images/evaluate/. Repointed to where it
actually lives, so the Evaluate walkthrough runs end to end. Verified the URL
serves 3 rows with the question and expected_answer columns the page maps.

Also adds links out of both quickstarts for the routes they do not teach,
prompted by the Phoenix workflow diagram: reading traces in code, from a
coding agent via the CLI or MCP, by asking PXI, and across Sessions; plus PXI
for curating datasets and running experiments. Links only, no new pages. The
Evaluate step stays absent on purpose, since online evals have not shipped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make both Evaluate paths use one dataset, and close three story gaps

Found by running every path in Get Started end to end as a new user.

The agent-assisted prompt inlined three examples, but only one of them was in
the CSV the manual path downloads. Cutting that file from eight rows to three
had left the prompt behind. The prompt now downloads the same CSV, so there is
one dataset and the two paths cannot drift again. It also restores the page's
own argument, which turns on the visa and refund rows.

Three gaps between the paths:

- The Recommended path ended when px setup finished and never said to open the
  Traces view. Only the manual path told you to look at what you captured.
- "Ask your coding agent" said to start Phoenix without giving the command,
  which only appeared in the other tab.
- Observe handed off promising to "convert selected traces into a dataset",
  which Evaluate no longer does, since it starts from a downloadable dataset.

Verified by execution, not reading: both language paths install and run and
produce correct OpenInference traces \(a support_reply CHAIN wrapping a
ChatCompletion LLM span with messages, model, and token counts\), px setup
connects and writes .env.phoenix, both skills commands install, and the CSV
builds the same 3-example dataset from either path.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: state the Python 3.10 floor and name the Traces tab in Observe

Both quickstart paths were run end to end against Phoenix 20.16.0. On
macOS's default Python 3.9, pip quietly installs arize-phoenix-otel
0.14.0 instead of 0.17.1, because the current packages require 3.10+.
A project also opens on the Spans tab, so "the Traces view" sent
readers looking for a view that isn't the landing page.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>

* docs: make the Evaluate comparison actually move the score

The page claimed the baseline prompt invents visa rules and agrees to refunds
it should decline. Measured against gpt-5-mini it does neither: the baseline
scored 1.0, the edited prompt scored 1.0, and stripping the baseline to
"Answer the customer's question" still scored 1.0. Step 4 promised a rise that
never came, so the lesson did not land.

Fixed by changing what the examples test. They now ask about a fictional
airline's Basic fare policy, with values deliberately unlike industry norms: a
48 hour refund window, a 40 dollar change fee, one included checked bag. The
model cannot guess any of them, so the baseline answers from industry defaults
and gets all three wrong. The edit supplies the policy.

Measured: baseline 0.00, edited 1.00.

This also makes the lesson durable. The baseline fails for lack of information
rather than lack of capability, so a better model will not quietly erase the
gap the way it did with the previous examples.

Also drops the Observe handoff sentence, per review.

The dataset CSV needs re-uploading to the same path; the agent-assisted prompt
reads it by URL and needs no edit.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the PXI paths, found by running them

Tested both PXI pages against a live Phoenix for the first time.

The terminal client command did not work. `npx -y @arizeai/phoenix-cli pxi`
exits with "unknown command 'pxi'", because pxi is a separate bin in the
package rather than a px subcommand, so npx ran the default bin and passed
pxi as an argument. Needs `-p`. Verified the corrected form launches and
reaches the credential check.

PXI defaults to an Anthropic model, so a reader who follows step 1 and sets
only OPENAI_API_KEY opens the assistant and hits "Anthropic credentials are
not configured". The step now says to pick a model matching the provider you
configured. Confirmed by switching to gpt-5.5 and getting a correct answer
about project span counts.

Restores "Where your data goes" under Privacy and tracing. It was dropped with
the architecture section, but it is not architecture: it is the answer to
whether data leaves your instance. No separate Arize service, your provider
called with your key, Arize not in the request path, and remote export gated
three ways. That is the first thing a security review asks about a beta AI
feature.

Also: the provider list mixed three env var names in code with "AWS Bedrock"
in prose, so it now names providers throughout; "Drives the product" lost its
object when I trimmed it; and the roadmap milestone link is out, since a
GitHub milestone goes stale.

Verified live: the consent gate appears as described, trace saving shows
"Disabled by system settings" which matches the documented default, the model
list matches the validated set, and PXI answered using search, get_schema, and
execute, the code-mode surface the page describes.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the CLI page its own job, running Phoenix from the terminal

The page claimed px was "the recommended way for a coding agent to work with
Phoenix", then documented only px setup, which Observe already owns. So it
repeated onboarding and never showed an agent working with Phoenix. Meanwhile
px has 17 top-level commands and the page covered one.

Rewritten around the debugging loop the phoenix-cli skill actually drives:
find what broke, read one trace in full, record the judgement as an
annotation, inspect the datasets and experiments you test against, then work
on the prompt. Plus a table for the remaining commands. Setup is now a single
line pointing at Observe.

Every command was run against a live Phoenix, which caught three errors in my
own first draft:

- `px span list` has no `--filter`. It takes typed flags, so the examples use
  `--status-code ERROR` and `--span-kind LLM`.
- `px experiment list` requires `--dataset`.
- `px trace get` resolves the default project and reports "Trace not found"
  without `--project`.

Verified working: trace list, trace get, trace add-note, span list with both
filters, span annotate \(annotation upserted\), dataset list, dataset get,
experiment list, prompt list, project list, and all 11 commands in the table.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: lead the agents overview with the setup table

Move "Set up your agent" above the three-piece cards so the
per-agent map comes first, and make its lead-in name the three
pieces so it stands on its own.

Narrow the endpoint rule: the plugins and the px CLI take the base
endpoint and append /mcp themselves, while `claude mcp add` and the
per-client MCP steps take the full /mcp URL.

Reword the prompt samples as examples rather than the full set.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the agents overview one job, shorten the MCP page

The overview page was mostly navigation: three cards pointing at CLI,
MCP Servers, and Skills, which already sit next to it in the sidebar,
plus a setup table routing to the same three pages. The sidebar label
"Overview" also repeated the group name, so the page read as a section
divider.

Now it does one thing, connect a coding agent to Phoenix: what this is,
the disambiguation note, the per-agent setup table, the endpoint. The
three cards are gone, the prompt samples are cut from five to two, and
the node_modules section is dropped since the TypeScript SDK overview
already documents it. Relabel the sidebar entry "Set Up Your Agent".

Correct the Claude Code row: the plugin ships three skills, while
px setup skills installs five, so the plugin path needs px setup skills
too. Verified by listing the plugin and running both.

On the MCP page, lead Connect with px setup mcp, merge the Cursor,
VS Code, and OpenCode accordions \(their JSON matches what px writes\),
replace the six Docs MCP accordions with a client table, and merge the
identical npm stdio configs. 633 to 521 lines with no facts dropped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: move the plugins onto the setup page

The setup page was still an index: its table pointed at Install
sections that CLI, MCP Servers, and Skills each already have. What it
lacked was the thing that belongs to no single one of them, the
one-install-per-agent path.

That path was the Plugins section, 137 lines and the largest section on
the MCP page, which is the wrong home for a plugin that registers MCP
and skills together. Move it here, after the endpoint it references,
and leave a pointer on the MCP page.

MCP drops to 388 lines and covers only MCP servers. The setup page now
routes by agent, states the shared endpoint, then gives the full plugin
steps.

Also repoint skills.mdx at the plugins section's new home, and fix its
link to a CLI heading that the CLI rewrite removed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the MCP page opener

It said "two things you might want your assistant to do" and "run
both" above a three-row table. Name the two servers that matter, your
data and the documentation, and say what the third row is.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: cut duplicated prose from the MCP page

The Configuration table repeated two env vars that self-hosting
configuration already documents; point there and keep only
PHOENIX_SKILLS_PATHS, which is unique to this page.

Monty sandbox limits were spelled out twice, in How it works and again
in Security; state them once. Merge the code-mode Warning and Note,
which both said the same thing. Trim the OAuth paragraph. Move the
OAuth2 authorization-server paragraph out of "Mounting your own
skills", where it was misfiled, into Security. Cut the npm sample
questions from seven to three.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: clarify the direction of the agents note

"Operate on Phoenix" is jargon, and "trace your sessions with a coding
agent" reads either way. Say which direction each page covers: this one
connects the agent to Phoenix, the other has Phoenix record the agent.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: correct two plugin commands that fail as written

Both found by running them. claude plugin uninstall acts on user scope
by default, so it stops with a scope error on a plugin installed at
local or project scope; say to pass the scope, and add disable, which
has the same rule. claude plugin details reports a disabled plugin as
not found, so say it has to be enabled, and say what the command lists.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: correct two CLI table rows that overstated what the commands do

Found by running them. px api has one subcommand, graphql, and rejects
mutations, so "authenticated requests to any Phoenix API route" is
wrong. The three annotation commands take only delete, and require
--all or a time window, so "stored annotations" hid that they are
destructive.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make the CLI debugging loop match what px can do

Steps 4 and 5 were titled "Turn failures into a test set" and "Work on
the prompt", but every command under them only reads, and the bodies
said "Inspect", contradicting the titles. px has no way to create a
dataset, add examples, or edit a prompt: for datasets, prompts, and
experiments it offers list, get, and delete only.

Cut the loop to the three steps px actually supports, ending at the
annotation, which with notes is the only thing it writes. Move the
dataset, experiment, and prompt reads into their own section framed as
looking up context, and say plainly that authoring happens in the UI
or the SDK.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rewrite the CLI page as the workflow an agent runs

The page had a "debugging loop" that was not a loop: it ended at an
annotation, and px cannot fix anything. Reorganising it by capability
then made it a feature list. Put it back in order as the pass an agent
actually runs, and let step 4 say plainly that px stops there and the
fix goes in your code, which is what closes the loop.

The examples were the weaker half. They repeated a fake project four
times, used placeholder ids that cannot be run, and carried comments
that restated the command instead of the output. Use real ids, say
which command feeds the next, and split the dataset, experiment, and
prompt reads into their own section, since those are for checking a
change rather than part of the debugging pass.

Add three facts found by running the commands: reads accept --format
json, annotate and add-note want the OpenTelemetry id rather than the
opaque one, and --identifier makes a repeat call overwrite.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: note that px annotation deletes are gated off by default

px span-annotations delete answers "Delete commands are disabled."
before it looks at any flag. Name the variable that opens the gate,
PHOENIX_CLI_DANGEROUSLY_ENABLE_DELETES, so the row does not send a
reader at a command that refuses to run.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say who runs px, and that the docs MCP URL is not a page

The CLI page opened by saying px is how a coding agent works with
Phoenix, then showed a reader commands to type, and titled the section
"How an agent uses it". A reader could not tell whether the commands
were theirs to run. Say it is an ordinary CLI you run yourself, and
that needing only a shell is what makes it easy to hand to an agent.

The Phoenix Docs MCP URL sat in a bare code block, so it read as a link
to open. A browser shows the tool manifest as JSON, which looks like an
error and is not. Say it is an endpoint for a client.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: answer px versus MCP, and add agent instructions to both pages

Neither page said when to reach for which. That choice only existed as
cards on the setup page, which is not where someone deciding ends up.
The CLI page now compares the two on what the client must support, what
setup costs, and how much surface each exposes, and the MCP page points
at it.

Both pages end with a section addressed to an agent reading them,
covering how to discover flags, which output format to parse, the
OpenTelemetry id the annotate commands want, the execute sandbox limits
and import allowlist, and that trace contents are untrusted data.

Every claim in both sections was run first: 116 catalog operations, the
50-call cap, json/re/datetime importable and socket/subprocess/importlib
not, exit status 0 and 1, and the delete gate.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rebuild the CLI page to a fixed outline, and align sibling names

The page now follows one order: what it is and why it matters, CLI or
MCP server, set up, what you can do with it, learn more, agent
instructions. Setup is three steps ending in a check. The examples
keep their real ids and drop the repeated --project in favour of
PHOENIX_PROJECT, set once in step two. The command table is gone; the
two caveats it carried, the delete gate and read-only GraphQL, live in
the agent instructions, and the full list is a link to the reference.

An audit of how each page in the section names its siblings found six
mismatches: the CLI page linked as "px CLI" and as bare "MCP", the
Remote MCP Server as "server", and a PXI section by a title it does
not have. Each link text now matches the page or section it points at.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: name the CLI setup check by what it proves

"Check it" said nothing. The step confirms the CLI reached the
instance, so say that, and say what success looks like.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say what the CLI setup is for before the steps

The section went straight from the heading to step one, so a reader
could not tell what was being set up or where. Name the CLI in the
heading, say to install it where the agent runs, and say what having
finished looks like.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say what px project list returns on a fresh instance, drop "has a terminal"

The setup check said a table of your projects means success, which
leaves a reader with no traces unsure what to expect. Tested on an
empty instance: every Phoenix has a project named default, so the
command always returns at least one row, and a wrong endpoint fails
with "Error fetching projects: fetch failed". Say both.

"When your agent has a terminal" was jargon on the CLI and MCP pages.
Say what it means: the agent can run shell commands, which every
coding agent can.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the CLI-first recommendation, say what the default project is

"Start with the CLI" was a preference with nothing behind it, and it
contradicted the setup page, which sends Claude Code and Codex users
to the plugin first, and the MCP page, which calls itself the primary
choice. The comparison table carries the facts; let the reader choose.
The MCP page mirror is now a plain pointer.

The setup check mentions the default project. Say what it is: created
by the initial migration, the destination for traces sent without a
project name, and empty on a fresh instance. Not a demo.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: make the CLI page work for a user with no traces yet

Run as a brand-new user, in a clean environment against an empty
instance, the previous page failed at step two: PHOENIX_PROJECT=my-app
404s on every read, and every example carried an id from one machine.
Stuff has to work before a project has traces.

Follow the shape of the live CLI section: install, endpoint, verify
with px --help and px project list, which both succeed with zero data,
then the common agent workflows as bullets and one example prompt, so
the reader sees what to ask rather than what the agent types. Say that
PHOENIX_PROJECT comes later, once traces are flowing. Keep the CLI or
MCP table and the agent instructions, and add the missing rule that
read commands need a project.

On the MCP page, restore the per-client tabs for the Docs MCP from the
live site, adding Claude Desktop.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: one story across Arize Phoenix for Agents, working from zero traces

The section was organised by plumbing, CLI, MCP, Skills, and every page
assumed traces already existed. A new user has none. Reframe around
what is true from the start: connect the agent, let it instrument your
app, then debug from what Phoenix records.

Set Up Your Agent owns routing, the endpoint, and the plugins. Its
connection check is a question to the agent, list my Phoenix projects,
which a fresh instance answers with default. Verified through both
pipes on an empty instance: px project list, and MCP search finding
getProjects and execute returning it. A new section points at the
agent-assisted tracing path as the next step.

CLI, MCP Servers, and Skills share one skeleton: what it is, install
ending in a check, what your agent can do with it, learn more,
reference. Deep material on the MCP page, API-key fallback, code mode
internals, server configuration, security, the npm server, sits under
Reference. The MCP Endpoint section folds into one line. The skills
page moves its options table and custom-skills guidance under
Reference and gains an example prompt.

Every shell command on the four pages was run in a clean environment
against an empty Phoenix. Restore the remote-mcp-server anchor, which
eleven pages link to, and repoint two onboarding snippets that still
named a CLI heading removed in an earlier rewrite.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut the instrumentation pointer back to one line

It was a sentence before, and a heading with three sentences after. The
link is the useful part.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: nest the MCP capabilities section under Remote MCP Server

The page covers two servers, so "what your agent can do with it" as a
top-level heading left "it" ambiguous. Make it an H3 under Remote MCP
Server, alongside a new Connect H3 for the setup steps.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut duplicated prose from the MCP page

The intro paragraph restated the table under it. The capabilities
section under Remote MCP Server repeated Reference, How it works,
nearly sentence for sentence; point there instead. Three sentences of
OAuth mechanism sat ahead of the first command; one line after the
accordions covers what a reader needs. The Docs MCP intro restated its
table row, the npm section said maintenance mode twice, and the
mounted-skills paragraph named its variable twice in a row.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: link the CLI page back to Set Up Your Agent

The MCP and Skills pages both point back to the setup page; the CLI
page did not, so a reader who landed there directly had no route to
the per-agent install.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: frame PXI skills as example capabilities

The table led with skill names, which is the mechanism. Lead with what
you can ask PXI to do, give one example ask per row, and keep the skill
concept as a single sentence after the table.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: frame PXI models as bring your own, and trim

Lead with the point: PXI runs on your provider and your key. Name the
providers once, keep the validated-model list and the tool-calling
caveat, drop the rest. Repoint the two links that used the old anchor.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the agent-instructions section from the CLI page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the agent-instructions section from the MCP page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the validated-model list from PXI

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: stop promising a supported-models list PXI no longer has

The list was removed; three references still pointed at it. Retitle the
Get Started link to the section that exists, and say model providers
where the Learn more line and the page description said supported
models.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: remove the npm MCP server from the MCP page

Drop the maintenance-mode section, its row in the intro table, and the
three places that offered it as a fallback: the version-floor line, the
Claude Desktop accordion, and the troubleshooting entry.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: act on the docs review: skills guidance, Docs MCP callout, MCP cut-down

Skills page: name the five skills written for Phoenix users, add the
plugin-versus-skills-add guidance from the review \(plugins track
Phoenix releases; skills add is a one-time copy, rerun it or run npx
skills update\), and take ownership of mounting your own skills from the
MCP page. Document --all honestly: tested in a fresh project it
installed 26 skills to 73 agents, including internal development
skills, so it is the wrong default.

MCP page: Phoenix Docs MCP becomes a short block near the top, as the
review asked, in place of an 87-line section. Fold Antigravity into the
other-clients accordion, trim How it works to the tool table plus two
sentences, and drop the mounting section now that Skills owns it.
361 to 271 lines.

Evaluate Your Agent: move the spans screenshot out of the Note it was
nested in.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: bring llms.txt in line with the Get Started and agents reorg

Add the CLI, Skills, and PXI Learn More pages, which had no entries.
Remove seven Quick Start entries for pages the reorg deleted or merged
into Observe Your Agent and Evaluate Your Agent, and three MCP entries
for pages folded into the single MCP page. Retitle the survivors to
match their pages.

Add the agent-usage guidance for the px CLI and the Remote MCP Server
here, where the review said it belongs, instead of on the human pages.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: give the CLI guide entry in llms.txt a unique title

It collided with the @arizeai/phoenix-cli reference entry.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten the endpoint rule on the setup page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: halve the agent-assisted setup on Observe Your Agent

Start Phoenix once above the tabs instead of in each. One sentence of
outcome per tab. Cut the pasted prompt to the two instructions that
matter, since the tracing skill carries the procedure. Drop the agent
list and the CLI pointer, which the setup page owns.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: add the PXI path to Evaluate Your Agent

A prompt to hand PXI once the dataset exists and the Playground has it
selected. Both preconditions come from running it: PXI cannot fetch a
URL, and its playground and evaluator skills act on the page you are on.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: restore the agent-assisted shape on Observe, trimmed

Back to the two tabs as they were, each self-contained, with the full
prompt. Merge the px setup outcome into one paragraph. Replace the
named --skill list with px setup skills, which installs exactly the
five Phoenix skills for every detected agent: tested via npx in a fresh
project with no CLI installed.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: repoint a PXI conversations link to the Learn More page

The Conversations section moved there when PXI split into Get Started
and Learn More; one link still pointed at the old page.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: Evaluate Your Agent builds a reference-aware evaluator

Built-in LLM templates have no reference field and score the baseline
higher than the fixed prompt, so step 2 now walks through Create new
LLM evaluator with a short rubric and the reference path mapping. PXI
section hands off after the dataset and evaluator exist, since it
cannot fetch the CSV and its own evaluator scores 0 at run time.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten Evaluate Your Agent evaluator step and PXI hand-off

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: PXI runs the full Evaluate flow from one prompt

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten Evaluate Your Agent prose

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut PXI Learn More to the essentials

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: one-line rows in the PXI capabilities table

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: shorten PXI privacy and tracing

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: compact PXI configuration table

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: shorten CLI page intro

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten MCP Servers prose

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop em dashes from Get Started snippets

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: add the follow-up prompt to the coding agent path

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: make px setup skills the primary install on the Skills page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say where the model API key goes before creating an LLM evaluator

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: the key must be on the Server tab for evaluators to use it

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say when to pick the CLI and when the MCP server

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: CLI vs MCP as best-when guidance

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop redundant line from CLI or MCP section

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: plugin rows say what the plugin covers and what is optional

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop Docs MCP aside from Set Up Your Agent

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

*…")  | Oct 9, 2026  |  
|   |   |  [fix(server): require auth for custom provider credential test (SSRF) (](https://github.com/Arize-ai/phoenix/commit/8c68957e68222a030587d3b14a8824e13785de22 "fix\(server\): require auth for custom provider credential test \(SSRF\) \(#12870\)

Move `testGenerativeModelCustomProviderCredentials` from `Query` to
`Mutation` and gate it with `IsNotReadOnly`, `IsNotViewer`, and
`IsAdminIfAuthEnabled`. The resolver makes outbound HTTP requests using
a user-supplied `baseUrl` and headers; as a query field it was
unauthenticated and reachable via introspection, making it usable to
SSRF cloud metadata endpoints \(e.g. `metadata.google.internal`\).

Add a regression test that asserts the field is not on `Query`, is on
`Mutation`, and carries the required permission classes. Update the
phoenix-server skill \(`graphql-patterns.md`, `SKILL.md`\) and `REVIEW.md`
with rules so side-effecting GraphQL fields cannot land on `Query` again.")[#…](https://github.com/Arize-ai/phoenix/pull/12870)  | Apr 26, 2026  |  
|   |   |  [docs: Update SECURITY.md (](https://github.com/Arize-ai/phoenix/commit/363e8913a3a0f7dafca5dc6bba6bf0e9776c1158 "docs: Update SECURITY.md \(#2029\)")[#2029](https://github.com/Arize-ai/phoenix/pull/2029)[)](https://github.com/Arize-ai/phoenix/commit/363e8913a3a0f7dafca5dc6bba6bf0e9776c1158 "docs: Update SECURITY.md \(#2029\)")  | Dec 29, 2023  |  
|   |   |  [fix: harden one-click deploy templates (](https://github.com/Arize-ai/phoenix/commit/14662353acc47bf3b8d9a0e13d8d70b0a0c0f0cf "fix: harden one-click deploy templates \(#14323\)

* fix: harden one-click deploy templates

- Azure: random PHOENIX_SECRET via newGuid\(\) \(uniqueString is deterministic
  and recomputable\), require an admin password instead of defaulting to
  'admin', replace SQLite-on-Azure-Files \(unsupported by SQLite WAL over
  SMB\) with an Azure Database for PostgreSQL flexible server, set
  restartPolicy to Always, and surface a plain-HTTP warning in the
  metadata, outputs, and README
- Render: provision 50GB of Postgres storage via diskSizeGB, document the
  rare PHOENIX_SECRET digit-validation failure
- All templates: correct the claim that setting an initial admin password
  skips the forced password change on first login \(it does not\)
- README: note that the Google Cloud button builds from source

* docs: make all deploy buttons the same height

* docs: add one-click deploy buttons to self-hosting docs

* fix: bump Render plan to standard, starter's 512MB OOMs under span queries")[#14323](https://github.com/Arize-ai/phoenix/pull/14323)[)](https://github.com/Arize-ai/phoenix/commit/14662353acc47bf3b8d9a0e13d8d70b0a0c0f0cf "fix: harden one-click deploy templates \(#14323\)

* fix: harden one-click deploy templates

- Azure: random PHOENIX_SECRET via newGuid\(\) \(uniqueString is deterministic
  and recomputable\), require an admin password instead of defaulting to
  'admin', replace SQLite-on-Azure-Files \(unsupported by SQLite WAL over
  SMB\) with an Azure Database for PostgreSQL flexible server, set
  restartPolicy to Always, and surface a plain-HTTP warning in the
  metadata, outputs, and README
- Render: provision 50GB of Postgres storage via diskSizeGB, document the
  rare PHOENIX_SECRET digit-validation failure
- All templates: correct the claim that setting an initial admin password
  skips the forced password change on first login \(it does not\)
- README: note that the Google Cloud button builds from source

* docs: make all deploy buttons the same height

* docs: add one-click deploy buttons to self-hosting docs

* fix: bump Render plan to standard, starter's 512MB OOMs under span queries")  | Jul 13, 2026  |  
|   |   |  [fix: harden one-click deploy templates (](https://github.com/Arize-ai/phoenix/commit/14662353acc47bf3b8d9a0e13d8d70b0a0c0f0cf "fix: harden one-click deploy templates \(#14323\)

* fix: harden one-click deploy templates

- Azure: random PHOENIX_SECRET via newGuid\(\) \(uniqueString is deterministic
  and recomputable\), require an admin password instead of defaulting to
  'admin', replace SQLite-on-Azure-Files \(unsupported by SQLite WAL over
  SMB\) with an Azure Database for PostgreSQL flexible server, set
  restartPolicy to Always, and surface a plain-HTTP warning in the
  metadata, outputs, and README
- Render: provision 50GB of Postgres storage via diskSizeGB, document the
  rare PHOENIX_SECRET digit-validation failure
- All templates: correct the claim that setting an initial admin password
  skips the forced password change on first login \(it does not\)
- README: note that the Google Cloud button builds from source

* docs: make all deploy buttons the same height

* docs: add one-click deploy buttons to self-hosting docs

* fix: bump Render plan to standard, starter's 512MB OOMs under span queries")[#14323](https://github.com/Arize-ai/phoenix/pull/14323)[)](https://github.com/Arize-ai/phoenix/commit/14662353acc47bf3b8d9a0e13d8d70b0a0c0f0cf "fix: harden one-click deploy templates \(#14323\)

* fix: harden one-click deploy templates

- Azure: random PHOENIX_SECRET via newGuid\(\) \(uniqueString is deterministic
  and recomputable\), require an admin password instead of defaulting to
  'admin', replace SQLite-on-Azure-Files \(unsupported by SQLite WAL over
  SMB\) with an Azure Database for PostgreSQL flexible server, set
  restartPolicy to Always, and surface a plain-HTTP warning in the
  metadata, outputs, and README
- Render: provision 50GB of Postgres storage via diskSizeGB, document the
  rare PHOENIX_SECRET digit-validation failure
- All templates: correct the claim that setting an initial admin password
  skips the forced password change on first login \(it does not\)
- README: note that the Google Cloud button builds from source

* docs: make all deploy buttons the same height

* docs: add one-click deploy buttons to self-hosting docs

* fix: bump Render plan to standard, starter's 512MB OOMs under span queries")  | Jul 13, 2026  |  
|   |   | [feat: Remove model inferences, dimensions, embeddings, and pointcloud…](https://github.com/Arize-ai/phoenix/commit/3dfa6ad65b6cbf72114d9f89849d4c01c0e694b8 "feat: Remove model inferences, dimensions, embeddings, and pointcloud UI \(#11589\)")  | Feb 20, 2026  |  
|   |   |  [fix: pin postgres version to 16 in](https://github.com/Arize-ai/phoenix/commit/5b588bf4288043dac03764e46159d090d954ba9f "fix: pin postgres version to 16 in `docker-compose.yml` \(#4855\)") [(](https://github.com/Arize-ai/phoenix/commit/5b588bf4288043dac03764e46159d090d954ba9f "fix: pin postgres version to 16 in `docker-compose.yml` \(#4855\)")[#4855](https://github.com/Arize-ai/phoenix/pull/4855)[)](https://github.com/Arize-ai/phoenix/commit/5b588bf4288043dac03764e46159d090d954ba9f "fix: pin postgres version to 16 in `docker-compose.yml` \(#4855\)")  | Oct 3, 2024  |  
|   |   |  [docs: reorganize Get Started around the agent workflow (](https://github.com/Arize-ai/phoenix/commit/2afdf488e638f282f742c7daf796a09a56662971 "docs: reorganize Get Started around the agent workflow \(#16288\)

* docs: reorganize Get Started around the agent workflow

Restructure the Get Started section to mirror the Arize AX quickstart:
Instrument your agent, Evaluate your agent, Improve your agent, Coding
agent setup, PXI, and Phoenix Demo.

The Python and TypeScript quickstarts previously sat in separate nav
groups, so each language duplicated the whole section tree. Each pair is
now a single page with Python and TypeScript tabs, so the language is a
choice within a section rather than a parallel branch of the nav.

Every page body is carried over verbatim into its language tab. The only
text rewritten is internal links that pointed at the retired
ts-get-started-* paths, which now have redirects to the merged pages.

- Instrument your agent  <- get-started-tracing + ts- counterpart
- Evaluate your agent    <- get-started-evaluations + ts- counterpart
- Improve your agent     <- group over the prompt playground and
                            datasets/experiments guides
- Coding agent setup     <- group over Agent-Assisted Setup and Coding
                            Agents
- MCP group, PXI, and Phoenix Demo are unchanged aside from ordering
- End to End Features Notebook is removed from the nav; the page itself
  is left in place

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: cut duplicated prose from the Get Started guides

The previous commit put each language's full guide in its own tab, which
made every language-agnostic sentence render twice on the same page. On
the prompt playground guide that was 37% of the prose; on datasets and
experiments, 40%.

Where the two tabs told the same story, the narrative is hoisted above
the tabs and the tabs now wrap only what genuinely differs: code, videos,
the companion-code callout, and the two prompt snippets. The prompt
playground and experiments guides had identical step sequences, so their
steps are shared outright. Tracing and evaluations each have an extra
TypeScript step, so their steps stay tabbed and only the intro, prereqs
and closing sections are shared.

No prose was rewritten. Every line either survives verbatim from the
previous commit or is a prefix of one with a redundant trailing clause
removed, verified mechanically. Where the two tabs differed only
trivially, one existing sentence is kept verbatim rather than a new one
written; this also picks up several corrections the TypeScript copy
already had:

- "traces that received an incompleteness evaluation label" ->
  "an incomplete evaluation label", matching the filter beside it
- "Filter your traces by the incorrect evaluation label" -> "incomplete";
  the filter under it has always been evals\['completeness'\] == 'incomplete'
- "Using the explanations apart of the evals" -> "from the evals"
- "New application runs as a results of our task" -> "as a result"
- "ran your first experiment" / "ran an A/B test" -> "run"

Also drops, on the tracing guide, a sentence restating the bullet list
above it and three repeats of the supported-frameworks disclaimer that
the intro already carries.

Verified: all 44 code blocks byte-identical \(multiset\), every step title
and video retained, no duplicate sentence in any rendered page, both link
checkers clean, all four pages render with working tabs.

Words: 10790 -> 8789 \(18.5% shorter\).

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: tighten Instrument and Agent-Assisted Setup, repoint quickstart

Three changes to the Get Started section.

Instrument your agent opened with a paragraph of series framing --
"This guide walks through a complete workflow ... Each guide in this
series introduces one piece of that workflow" -- which is word for word
the Overview page's opening and says nothing about instrumentation. It is
removed here and still lives on the Overview page. The guide now opens on
its own subject: what a trace is.

/docs/phoenix/quickstart redirected to the Overview page, which is not in
the nav. It now lands on Instrument your agent, the actual first guide in
the series. The Overview page is unchanged and still reachable from the
four pages that link to it.

Agent-Assisted Setup is only ~350 words of prose but 89 lines, because
almost two thirds of it is follow-up reference rather than setup. The
three advanced sections -- re-running a single step, non-interactive runs,
and unsupported agents -- move into an AccordionGroup, so the page reads
as an intro, two steps and a verification section. Nothing is deleted:
all five code blocks are byte-identical and the collapsed text is still
in the page. The one prose deletion is "The fastest way to add Phoenix
tracing to your application.", which restated the frontmatter description
rendering directly above it.

Trade-off worth noting in review: collapsed sections leave the "On this
page" table of contents, which now lists only "Confirm traces are
flowing".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: hold every Get Started intro to four sentences or fewer

The intros restated themselves. Evaluate your agent opened with seven
sentences making three points: what an eval is \(said three times\), what
the guide does \(said twice\), and which metrics you can score against.

Deletions only, no rewriting. Every surviving sentence is verbatim.

Evaluate your agent, 7 -> 4. Dropped:
- "Evaluations attach quality signals to runs so that correctness or
  relevance can be reasoned about consistently instead of judged case by
  case." Restates the opening sentence and the traces/evals contrast that
  follows it, and states it as a negative.
- "We'll start with data that already exists in Phoenix, define a simple
  evaluation, and run it so we can see results directly in the UI."
  Restates "In this guide, you'll set up evaluations ... and run them on
  existing trace data".
- "The goal is to move from "I have model outputs" to "I can measure
  quality in a repeatable way."" Restates the opening sentence.

Iterate on Your Prompts, 5 -> 3. Dropped "Up to this point, we've traced
our agent runs and evaluated their outputs. Now we'll focus on prompts by
grouping failures into a dataset and iterating on prompt variants in the
Prompt Playground." -- the sentence above it already says the guide
groups failing runs into a dataset and iterates in the Playground, and
the prereq list directly below states the same continuity as two bullets.

Instrument your agent, TypeScript tab 5 -> 4. Dropped "In this guide,
we'll get tracing set up and walk through how to instrument an
application.", which the sentence after it says with the specifics. The
Python tab already carried this as one sentence.

Counts are per reader, shared intro plus their language tab, excluding
callouts: Instrument 4, Evaluate 4, Iterate 3, Optimize 3,
Agent-Assisted Setup 1.

Code blocks unchanged on all three pages.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say "agent" instead of "app" across Get Started

Refers to the reader's system as an agent throughout, matching the
section titles \("Instrument your agent", "Evaluate your agent"\). 51 lines
across the five guides plus the Overview page. Code blocks and URLs are
untouched; the `js/examples/apps/mastra-quickstart` paths and every
identifier keep their spelling.

The experiments guide's title becomes "Optimize Your Agent with
Experiments", so the nav label changes with it.

Two phrases needed care rather than a plain swap:

- "an agent application" would have become "an agent agent"; it is now
  just "an agent".
- "the agents that make up our application" would have become "the agents
  that make up our agent"; it is now "our agent system", the phrasing the
  experiments guide already uses.

Agent-Assisted Setup keeps "app" for the reader's codebase. That page is
about handing work to a *coding* agent, so "let your coding agent add
tracing to your agent" reads as one thing instrumenting itself. The
collision is worse than the inconsistency.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: merge the prompts and experiments guides into Improve your agent

"Iterate on Your Prompts" and "Optimize Your Agent with Experiments" were
two halves of one workflow split by *how* you make the change rather than
by what you are doing. Both started from eval-labelled failing traces,
both ended in the same Experiments view, and each pointed at the other as
the next step, so a reader could not tell which came first.

They are now one guide, matching the AX quickstart, which has a single
"Improve your agent" page with method tabs rather than a group.

Four steps, with the method choice appearing once instead of at every
stage:

1. Create a dataset from failed traces      \(shared\)
2. Use explanations to identify improvements \(shared; was taught twice\)
3. Make the change and run it   -- tabs: In the Prompt Playground /
   In code, with Python and TypeScript nested inside each
4. Compare versions in the Experiments view  \(shared narrative\)

Nothing is dropped. Verified against both source pages: 13 of 13 code
blocks, 12 of 12 videos, 12 of 12 frame captions, every numbered
instruction, and every H3 section.

Deliberate changes beyond the merge:

- The two cross-links between the guides are gone; they would now point
  at this page. That circularity was the reason to merge.
- "the first from Step 3" is now "the first from your baseline run",
  because the step it referenced no longer has that number.
- "A dataset created from previous runs" leaves the prerequisites, since
  step 1 builds it.
- The two "Congratulations" lines and the two "Learn More" sections are
  one each.
- About eight new connective sentences were written, the only prose on
  this page that is not carried over verbatim. Interleaving two
  workflows needs them.

Seven URLs now redirect here: both retired page paths, both ts- paths,
and the three legacy quickstart paths.

docs/phoenix.mdx had a card for each guide in both its Python and
TypeScript groups; each pair is now one "Improve Your Agent" card.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rewrite Get Started in second person, direct voice

The guides narrated themselves in the first person plural -- "we'll
install", "now that we have trace data, we can", "at this point we've
defined" -- which reads as tutorial filler and contradicts the house rule
that guides use active voice and second person. 155 instances of that
register across the section, now 0:

  royal "our"                   55 -> 0
  royal "we'll" / "we're"       44 -> 0
  "we can"                      26 -> 0
  "At this point,"               7 -> 0
  "Next, we..."                  6 -> 0
  "In this step, we..."          6 -> 0
  "Now that ..., we"             4 -> 0
  "let's"                        5 -> 0
  "More importantly,"            2 -> 0

Instructions are now imperative and the prose states facts about the
system rather than narrating a shared journey. "Next, we'll register a
tracer provider linked to a project in Phoenix. This project is where
your traces will show up" becomes "Register a tracer provider linked to a
project in Phoenix. That project is where your traces show up."

Two errors fixed on the way through:

- "seen how an agent runs shows up from start to finish" -> "how an agent
  run shows up"
- "our finanical writer agent" -> "The financial writer agent"

No instruction was dropped. Code blocks, videos, link destinations, step
titles and H3 sections are identical on all three guides: 17/2/13/9/14 on
tracing, 14/2/9/9/5 on evaluations, 13/12/5/4/9 on Improve your agent.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: sharpen the tracing opener and drop every em dash

The tracing guide opened with a sentence that did not parse: "A trace is
a record of a single run of your agent, broken down into spans that show
what happened at each step \(how agents, tasks, and tools executed\) and
provides the raw data needed for everything that follows." After the
participial phrase, "and provides" cannot reach back to "is a record", so
it reads as attaching to "spans", which would need "provide". It also
said the same thing twice \("what happened at each step" and "how agents,
tasks, and tools executed"\), and closed on "the raw data needed for
everything that follows", which was left over from the series framing
that now lives only on the Overview page.

Replaced with the house formulation from ax/instrument/what-are-traces
and phoenix/tracing/concepts-tracing, which also defines a span instead
of gesturing at it:

  A trace captures one end-to-end run of your agent as a tree of spans.
  Each span is a single step \(an agent, a task, a tool call, or an LLM
  call\) with its input, output, and timing.

The page also picks up a frontmatter description, matching the AX tracing
guide. It renders as the subtitle under the H1, so it carries the framing
while the body carries the mechanic, and the two do not restate each
other.

Em and en dashes are now zero across the section. Two were mine; two were
pre-existing on Agent-Assisted Setup, where a dash joined two independent
clauses that are clearer as separate sentences.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: stop repeating the prerequisites, and drop the long reference from Get Started

The evaluations guide stated its prerequisites twice, about fifteen lines
apart, in different words. At the top: "you'll need to have completed Get
Started with Tracing so you should have: Financial Analysis and Research
Chatbot / Trace Data in Phoenix". Then again inside step 1: "Evaluations
in Phoenix run over existing trace data. If you followed the tracing
guide, you should already have: A project in Phoenix / Traces containing
LLM inputs and outputs". Both tabs carried it, so a reader met the same
list twice per pass.

The step's version was the more precise one, so its detail is folded into
the single list at the top, which now reads "A project in Phoenix holding
traces with LLM inputs and outputs". Step 1 keeps the one fact it added,
that Phoenix evaluations run over existing trace data, and loses the
duplicate list.

Worth noting for future passes: string-similarity scanning did not catch
this, because the two lists share almost no wording. Semantic repetition
needs a read, not a regex.

The Overview page repeated itself inside single paragraphs the same way:
"using the same inputs and evaluation criteria" in two consecutive
sentences, and "spans that show what happened at each step" followed by
"Traces show how agents, tasks, and tools executed during a run". Both
are now stated once.

Coding Agents leaves the Get Started nav. It is 466 lines, 2258 words and
27 code blocks of reference material for Claude Code, Codex, the CLI, MCP
and skills, and its own opening line says "If you haven't instrumented
your app yet, start with Agent-Assisted Setup" -- it is the step after
getting started, not part of it. The page is unchanged and still listed
under Integrations > Developer Tools, and Agent-Assisted Setup already
links to it for ongoing workflows.

That leaves one page in the group, so it collapses to a single nav entry.
Agent-Assisted Setup gains sidebarTitle "Coding agent setup" so the label
still matches the agreed structure, the same way pxi.mdx shows as "PXI
Agent".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix five accuracy defects found by checking Get Started against the source

Verified the guides against the Phoenix source in this repo rather than
against other docs. Five real defects, all inherited from main:

1. `evals\['completeness'\].label == 'incomplete'` is a **span** filter. The
   guide said "Filter your traces by the incomplete evaluation label",
   which points at the traces table, where the expression is rejected:
   src/phoenix/trace/dsl/trace_filter.py refuses it with "`evals\[...\]` is
   not available in the trace filter". docs/.../filter-expressions.mdx
   lists it under span filters, and the UI has separate SpanFiltersContext
   and TraceFiltersContext. Now reads "Filter the spans table".

2. and 3. The add-to-dataset flow is span-based, not trace-based:
   AddSpansToDatasetInput carries `span_ids`
   \(src/phoenix/server/api/mutations/dataset_mutations.py:225\), and the
   dataset guide says "select the span". "Select the traces you want to
   include" and "Add the selected traces" now say spans.

4. `client = Client\(\)` ran with no import on this page. The evaluations
   guide imports it, but this page has to stand alone, so it now carries
   `from phoenix.client import Client`.

5. `import { getDataset } from "@arizeai/phoenix-client/datasets"` was
   never used. The companion example the guide points at,
   js/examples/apps/mastra-quickstart/src/mastra/experiments/experiment.ts,
   imports the identical set without it. Removed.

Everything else checked out against source: register\(project_name,
auto_instrument\), LLM\(provider, model\), ClassificationEvaluator\(name, llm,
prompt_template, choices\) and .bind\(\), Client\(\).datasets.get_dataset\(dataset=\),
Client\(\).experiments.run_experiment\(dataset=, task=, evaluators=\),
suppress_tracing's import path, the phoenix-client and phoenix-evals TS
exports, runExperiment accepting a {datasetName} selector, ports 6006 and
4317, exit code 6 as NOT_VERIFIED, every `px setup` subcommand and all
nine documented flags, and both companion code paths.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: one language choice on Improve your agent, not ten

The page had accumulated 10 separate Python | TypeScript pickers down its
length: one on the companion-code callout, one per video pair, and one
inside each half of the method tabs. Mintlify syncs tabs that share
labels, so picking a language once did switch all of them, but rendering
the control ten times reads as ten decisions.

Language is now a single choice at the top, the same shape the tracing
and evaluations guides use. Method tabs \(In the Prompt Playground /
In code\) stay where they were, inside step 3 and step 4. A reader now
sees one language picker and two method pickers instead of twelve
controls.

This is purely structural: every Python | TypeScript group was resolved
to the chosen language and the whole body wrapped in one pair. No prose
was rewritten. Verified against the previous version: all 13 code blocks,
12 videos, 12 frame captions, every numbered instruction, every heading
and every step title still present.

The cost is 78 lines. Language as the outer axis means the
language-agnostic narrative -- the dataset steps, the explanation step,
the shared prose in steps 3 and 4 -- exists once per branch. That is why
the step count in the file doubles from 4 to 8 while a reader still sees
4. Worth it for one obvious choice at the top; the alternative was
keeping the page shorter and the navigation confusing.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: method first, language inside it, on Improve your agent

Corrects the tab hierarchy to match ax/get-started/get-started-tracing,
where the outer tabs are methods \(By npx evals / By Arize Skills / By
Code\) and the Python | TypeScript picker sits inside By Code. The previous
commit put language on the outside, which was the wrong axis.

A reader now picks the method first \(In the Prompt Playground / In code\),
then the language within it. Two controls, in that order.

Also tightened the prose, since with tabs every shared line exists in more
than one branch and each cut lands more than once. Deleted as restatements
of the line beside them:

- "This dataset now represents a concrete failure case for your agent."
- "Start by grouping together the traces that didn't perform well."
- "Start from a real prompt the agent actually used." -- the next sentence
  already gives "a real starting point".
- "Saving prompt variants makes it easy to track changes..."
- "An experiment reruns the same inputs through different versions..." --
  the page intro already defines an experiment; this was the third time.
- "With two versions in hand, compare them..." -- restated the step title.

Shortened: the two-paths lead-in \(the tab labels say it\), the
create-datasets aside \(said "in code" twice\), and the task and evaluation
definitions. Fixed two royal "we" the voice pass missed: "the dataset we
made earlier" and "we should see more traces".

Line count: 605 -> 581 from the trims, then -> 694 from the inversion.
The growth is real and worth naming. Steps 1 and 2 -- create the dataset,
read the explanations -- are method-agnostic, so method-outer copies them
into both method branches, and language-inner copies them again. Four
copies of about thirty lines. A reader still sees each step once.

The wrinkle that buys: the method choice now sits above two steps that do
not depend on it, so a reader picks Playground or code before creating the
dataset. The alternative keeps steps 1 and 2 shared and moves the method
choice down to step 3, at the cost of a second visible language picker.

Verified against the previous version: every code block, video, caption,
link, heading and step title still present.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: move the companion-code callout out of the no-code path

The Prompt Playground branch offered "a companion notebook with runnable
code examples" in a path where you write no code. That was a side effect
of wrapping the whole body in the method tabs. The callout now appears
only under In code, for both languages.

The Python | TypeScript picker stays inside the Prompt Playground branch,
because that path does depend on which language you followed -- not for
the language itself, but for what you built with it. Six of the videos
record the CrewAI app rather than the Mastra app, and the dataset input
syntax differs with the agent: `{tickers}` and `{focus}` are CrewAI task
variables, `{{input}}` is Mastra. The label is a proxy for "which
quickstart did you follow".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: drop the horizontal rules bracketing the companion-code callouts

Every companion-code callout in Get Started was wrapped in `---` rules,
inherited from main. Each renders as a thematic break with generous
margins, so the callout sat in a band of empty space with a thin line
above and below it. The callout already has its own bordered container,
so the rules added separation that was not needed.

12 rules removed: four on the tracing guide, four on evaluations, four on
Improve your agent. Frontmatter delimiters are untouched, including the
multi-line description block on the tracing guide, whose closing `---`
sits at line 6 and is easy to mistake for one of these.

No prose changed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the playground template syntax, which would not have interpolated

The Python path told readers to set the User prompt to `{tickers}` and
`{focus}`. That is F_STRING syntax, and the Prompt Playground defaults to
Mustache:

  js/app/src/components/templateEditor/constants.ts
    FString:  "F_STRING",  // {variable}
    Mustache: "MUSTACHE",  // {{variable}}

  js/app/src/store/playground/playgroundStore.tsx:273
    templateFormat: TemplateFormats.Mustache,

Playground.tsx reads the format from that store, so with default settings
single braces are not substituted -- the prompt would have carried the
literal text `{tickers}`. The guide never mentioned the format selector,
so there was no way for a reader to know.

Most likely this was copied from the CrewAI task template on the tracing
guide, `Task\(description="Research: {tickers}"\)`, which legitimately uses
single braces. That is CrewAI's own templating, not Phoenix's, and it is
left alone.

Both paths now use Mustache, and the shared lead-in says so, so the two
language tabs differ only in the variable names.

The variable names themselves were already correct and are framework
driven, traceable in the guides' own code: the Python experiment passes
`example.input` straight into `kickoff\(inputs=...\)`, so dataset examples
carry `tickers` and `focus` keys, while the TypeScript task types
`example.input` as a `{role, content}\[\]` message array under a single
`input` key.

Verified rendered: the block shows `Research: {{tickers}}` literally, and
the format note appears in both language tabs.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: expand the Agent-Assisted Setup accordions back into headings

I collapsed three sections into an AccordionGroup when the page read as
too long. That hid 307 of its 542 words, 57% of the page, and dropped the
three section names from the "On this page" table of contents, leaving one
entry. It also made the page look like content had been deleted, which is
what prompted the question.

The reason for collapsing them has gone: the length concern was mostly the
466-line Coding Agents reference, which no longer sits in this nav group.
What remains is ~330 words, the shortest page in Get Started, so hiding
more than half of it costs more than it saves.

Re-run a single step, Run non-interactively \(CI or agents\), and Use an
unsupported agent are `##` headings again, so all four sections appear in
the table of contents and can be linked to directly.

Code blocks and links are identical.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: no language picker in the Prompt Playground path

You write no code in the Prompt Playground, so asking the reader to pick
Python or TypeScript there was misleading. The picker now appears only
under In code, where it selects actual code, which matches
ax/get-started/get-started-tracing.

Two controls on the page, in this order:

  In the Prompt Playground | In code
    In code -> Python | TypeScript

What the picker had been carrying in that path, and where it went:

- Steps 1, 2 and 4 were already identical between the languages.
- The dataset input keys are real, so they are stated inline: which keys
  you have depends on the agent you traced, `{{tickers}}` and `{{focus}}`
  from the Python guide, `{{input}}` from TypeScript. Both forms are now
  written out rather than left to be inferred.
- The example prompt line keeps the Python variant, which is the one that
  matches the finding step 2 states \("the report lacked financial
  ratios"\). The TypeScript variant said something different, about
  per-ticker metrics.
- Five videos drop out: ts-first-dataset, ts-first-prompt,
  ts-playground-experiment, ts-compare-prompts, ts-view-experiment. They
  carried the same captions and demonstrated the same click-path as the
  five that remain, recorded against Mastra instead of CrewAI.

All 13 code blocks and every link destination are unchanged. 668 -> 538
lines.

Braces checked: every `{{...}}` on the page is either inside a fenced
block or an inline code span, so MDX treats them as text. Verified in the
rendered DOM.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: restructure Get Started nav and verify evals against the product

Reshape the Get Started nav to Observe Your Agent / Evaluate Your Agent /
PXI / Arize Phoenix for Agents / Phoenix Demo Account. Per-language pages
become tabs within each section, with language nested inside the code path.

Merge the Improve Your Agent guide into Evaluate Your Agent, which is now
written as offline evals: a dataset feeds a task, an evaluator scores each
output. Phoenix has not released continuous scoring of live production
traffic, so the previous framing was wrong.

Extract Skills from the Coding Agents page into its own page and group the
agent-facing docs \(agent-assisted setup, skills, MCP\) under Arize Phoenix
for Agents. A pointer heading keeps existing #skills anchors resolving.

Correct six claims against Phoenix 20.15.0 running locally:

- `output` is the task's output, not an example payload. `reference` is the
  example's expected output \(experiment_runner.py:831, message_helpers.py:279,
  and the compare view's "reference output" column\).
- Creating a dataset from spans is one action, not two. You select spans and
  click Add to Dataset; creating a new dataset happens inside that popover
  \(SpanSelectionToolbar.tsx:212, DatasetSelectorPopoverContent.tsx:104\).
- The evaluator dialog's submit button is Create, not Save.
- The playground control is "Test over a dataset".
- Experiment runs live on the Experiments tab, compared via the
  "comparisons" selector.
- Playground template format defaults to Mustache, so dataset input keys
  need double braces.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: remove em dashes from the Get Started nav pages

All 98 em dashes on pages now reachable from Get Started, none of them
authored by this branch: pxi.mdx \(31\), coding-agents.mdx \(22\), remote-mcp.mdx
\(14\), mcp.mdx \(4\), docs-mcp.mdx \(3\), phoenix-mcp-server.mdx \(1\), plus 14
bullet and label dashes replaced with colons.

Each was repunctuated in context rather than swapped for a single character:
a colon where a list or restatement follows, a comma for an aside, a period
where an independent clause follows, parentheses for the three parentheticals
that span hard-wrapped lines. No dash inside a fenced code block was touched.

Diff is 89 insertions and 89 deletions, so no prose was added or removed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: point Evaluate Your Agent at the trace-scoring flow

The merged page covers offline evals over experiments. Readers who want to
score traces they have already collected had no path to it from Get Started,
since that content now lives under Tracing and Evaluation. Links to
Evaluating Phoenix Traces and the two eval quickstarts.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: simplify Phoenix get started quickstarts

* docs: keep Agent, not App, in the Get Started flow

Restores "Observe Your Agent" and "Evaluate Your Agent" and aligns the
surrounding copy, which had moved to "app": both page titles and descriptions,
the section headings on the Get Started overview, the landing-page cards, and
two "Observe Your App" cross-links that would have pointed at a page with a
different name. `app.ts` stays, since that is a file the reader creates.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: one Quick Starts card group, without the Improve card

The Improve page merged into Evaluate Your Agent, so its card pointed at the
same page as the Evaluate card. Dropped it and folded its description into
Evaluate's, which had described only the LLM-as-a-judge half.

The Python and TypeScript groups held identical cards, and both target pages
now select language with tabs, so the split no longer carried information.
Collapsed to a single group. The redirect from the retired improve path is
untouched.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make Evaluate Your Agent a single Playground path

Drops the Prompt Playground / Code tab split and keeps the Playground steps,
so the quickstart teaches one path end to end. 401 lines to 146.

Adds a "Prefer to do this in code?" section that sends readers to Run
Experiments for the Python SDK and to the TypeScript experiments reference for
runExperiment, so both languages the removed tab covered still have a home.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say which traces to evaluate and what to measure

The steps described where to click without answering the two questions a
reader arrives with. Five tweaks, 146 lines to 151:

- Dataset step gives a size to aim for and two ways to find failing traces,
  a filter expression or asking PXI to walk them
- Evaluator step names error patterns worth scoring instead of only listing
  the two evaluator types
- Saving a prompt from a trace opens with why that prompt is the one to use
- The baseline is stated as the thing later edits are measured against
- Comparison step closes with how to read the delta, including what to do
  when the score drops

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say offline evaluators in the Evaluate description

Also removes two lines the single-path change left stale: an opening sentence
that restated the description, and a paragraph telling readers to choose
between the Playground and code paths, which are no longer tabs on this page.
The offline framing replaces it, with the caveat that continuous scoring of
live production traffic has not shipped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say the manual path builds everything from the Phoenix UI

Now that this page is Playground-only, the section intro should name the
surface the steps use.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: shorten the PXI line in the dataset step

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: restore the code experiment example as its own page

Making Evaluate Your Agent a single Playground path deleted Nancy's Python
and TypeScript worked examples, and the pages it linked to instead cover the
same APIs without that scenario. Recovered them verbatim from 966510c74 as
Evaluate a Prompt Change in Code, under How-to: Experiments.

All 7 code blocks and both 4-step sequences are byte-identical to the removed
tab; only the Tab wrapper was dropped and the block de-indented one level. The
page opens by tying itself to the quickstart, so the support agent scenario
carries across the UI and code paths again.

"Prefer to do this in code?" now points here first, with Run Experiments as
the wider reference.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the new experiment page headings its siblings have

The page had no H2s, so it contributed nothing to the table of contents while
every other page in How-to: Experiments has several. Adds a heading over the
tabbed walkthrough, notes that the script creates its own dataset so it runs
without traces, and points at Set a Baseline Experiment, which covers the
persistent baseline that this page's in-code comparison does not.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rework Observe and Evaluate, split PXI into Get Started and Learn More

Observe Your Agent: intro rewritten to say what the page does and where the
concepts live, `\(Recommended\)` on the agent-assisted path, and the manual
steps reordered to start Phoenix, get your keys, then set up tracing. The
install step folded into setup, and the three steps that were identical in
both language tabs hoisted above them.

Evaluate Your Agent: no longer depends on having traces. The dataset step
hands you three travel support examples to upload, so the page runs on a
fresh Phoenix. Traces became an optional route rather than the only one.
Switched to an LLM judge, added copyable Playground prompts for the baseline
and the edit, and dropped the save-a-prompt-from-a-trace step.

PXI: 3,315 words split into a 315-word Get Started \(what it is, what it does,
three steps\) and a 1,158-word Learn More grouped as capabilities, surfaces,
configuration, and privacy. Nothing deleted except the architecture diagram
and the flag and slash-command tables, which duplicate the CLI reference.
Accordions flattened so nothing is hidden behind a click.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: consolidate Arize Phoenix for Agents

Snapshot of the agents/MCP consolidation: agent-assisted-setup folded into a
new developer-tools/cli page, and remote-mcp, docs-mcp, and phoenix-mcp-server
collapsed into one integrations/mcp page, with redirects for all four retired
paths. Nav and inbound references updated across the CLI reference, release
notes, self-hosting, and the shared onboarding snippets.

Committed from the docs-get-started-reorg worktree so the two in-flight
branches can be merged.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: point the tracing quickstart at the new CLI page

The agents consolidation folded agent-assisted-setup into
developer-tools/cli. This was the last reference still relying on the
redirect rather than the real page.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: link the uploaded dataset, and cross-link the other Observe methods

The travel-support.csv download pointed at assets/datasets/, but the file was
uploaded to assets/images/arize-docs-images/evaluate/. Repointed to where it
actually lives, so the Evaluate walkthrough runs end to end. Verified the URL
serves 3 rows with the question and expected_answer columns the page maps.

Also adds links out of both quickstarts for the routes they do not teach,
prompted by the Phoenix workflow diagram: reading traces in code, from a
coding agent via the CLI or MCP, by asking PXI, and across Sessions; plus PXI
for curating datasets and running experiments. Links only, no new pages. The
Evaluate step stays absent on purpose, since online evals have not shipped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make both Evaluate paths use one dataset, and close three story gaps

Found by running every path in Get Started end to end as a new user.

The agent-assisted prompt inlined three examples, but only one of them was in
the CSV the manual path downloads. Cutting that file from eight rows to three
had left the prompt behind. The prompt now downloads the same CSV, so there is
one dataset and the two paths cannot drift again. It also restores the page's
own argument, which turns on the visa and refund rows.

Three gaps between the paths:

- The Recommended path ended when px setup finished and never said to open the
  Traces view. Only the manual path told you to look at what you captured.
- "Ask your coding agent" said to start Phoenix without giving the command,
  which only appeared in the other tab.
- Observe handed off promising to "convert selected traces into a dataset",
  which Evaluate no longer does, since it starts from a downloadable dataset.

Verified by execution, not reading: both language paths install and run and
produce correct OpenInference traces \(a support_reply CHAIN wrapping a
ChatCompletion LLM span with messages, model, and token counts\), px setup
connects and writes .env.phoenix, both skills commands install, and the CSV
builds the same 3-example dataset from either path.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: state the Python 3.10 floor and name the Traces tab in Observe

Both quickstart paths were run end to end against Phoenix 20.16.0. On
macOS's default Python 3.9, pip quietly installs arize-phoenix-otel
0.14.0 instead of 0.17.1, because the current packages require 3.10+.
A project also opens on the Spans tab, so "the Traces view" sent
readers looking for a view that isn't the landing page.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>

* docs: make the Evaluate comparison actually move the score

The page claimed the baseline prompt invents visa rules and agrees to refunds
it should decline. Measured against gpt-5-mini it does neither: the baseline
scored 1.0, the edited prompt scored 1.0, and stripping the baseline to
"Answer the customer's question" still scored 1.0. Step 4 promised a rise that
never came, so the lesson did not land.

Fixed by changing what the examples test. They now ask about a fictional
airline's Basic fare policy, with values deliberately unlike industry norms: a
48 hour refund window, a 40 dollar change fee, one included checked bag. The
model cannot guess any of them, so the baseline answers from industry defaults
and gets all three wrong. The edit supplies the policy.

Measured: baseline 0.00, edited 1.00.

This also makes the lesson durable. The baseline fails for lack of information
rather than lack of capability, so a better model will not quietly erase the
gap the way it did with the previous examples.

Also drops the Observe handoff sentence, per review.

The dataset CSV needs re-uploading to the same path; the agent-assisted prompt
reads it by URL and needs no edit.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the PXI paths, found by running them

Tested both PXI pages against a live Phoenix for the first time.

The terminal client command did not work. `npx -y @arizeai/phoenix-cli pxi`
exits with "unknown command 'pxi'", because pxi is a separate bin in the
package rather than a px subcommand, so npx ran the default bin and passed
pxi as an argument. Needs `-p`. Verified the corrected form launches and
reaches the credential check.

PXI defaults to an Anthropic model, so a reader who follows step 1 and sets
only OPENAI_API_KEY opens the assistant and hits "Anthropic credentials are
not configured". The step now says to pick a model matching the provider you
configured. Confirmed by switching to gpt-5.5 and getting a correct answer
about project span counts.

Restores "Where your data goes" under Privacy and tracing. It was dropped with
the architecture section, but it is not architecture: it is the answer to
whether data leaves your instance. No separate Arize service, your provider
called with your key, Arize not in the request path, and remote export gated
three ways. That is the first thing a security review asks about a beta AI
feature.

Also: the provider list mixed three env var names in code with "AWS Bedrock"
in prose, so it now names providers throughout; "Drives the product" lost its
object when I trimmed it; and the roadmap milestone link is out, since a
GitHub milestone goes stale.

Verified live: the consent gate appears as described, trace saving shows
"Disabled by system settings" which matches the documented default, the model
list matches the validated set, and PXI answered using search, get_schema, and
execute, the code-mode surface the page describes.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the CLI page its own job, running Phoenix from the terminal

The page claimed px was "the recommended way for a coding agent to work with
Phoenix", then documented only px setup, which Observe already owns. So it
repeated onboarding and never showed an agent working with Phoenix. Meanwhile
px has 17 top-level commands and the page covered one.

Rewritten around the debugging loop the phoenix-cli skill actually drives:
find what broke, read one trace in full, record the judgement as an
annotation, inspect the datasets and experiments you test against, then work
on the prompt. Plus a table for the remaining commands. Setup is now a single
line pointing at Observe.

Every command was run against a live Phoenix, which caught three errors in my
own first draft:

- `px span list` has no `--filter`. It takes typed flags, so the examples use
  `--status-code ERROR` and `--span-kind LLM`.
- `px experiment list` requires `--dataset`.
- `px trace get` resolves the default project and reports "Trace not found"
  without `--project`.

Verified working: trace list, trace get, trace add-note, span list with both
filters, span annotate \(annotation upserted\), dataset list, dataset get,
experiment list, prompt list, project list, and all 11 commands in the table.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: lead the agents overview with the setup table

Move "Set up your agent" above the three-piece cards so the
per-agent map comes first, and make its lead-in name the three
pieces so it stands on its own.

Narrow the endpoint rule: the plugins and the px CLI take the base
endpoint and append /mcp themselves, while `claude mcp add` and the
per-client MCP steps take the full /mcp URL.

Reword the prompt samples as examples rather than the full set.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the agents overview one job, shorten the MCP page

The overview page was mostly navigation: three cards pointing at CLI,
MCP Servers, and Skills, which already sit next to it in the sidebar,
plus a setup table routing to the same three pages. The sidebar label
"Overview" also repeated the group name, so the page read as a section
divider.

Now it does one thing, connect a coding agent to Phoenix: what this is,
the disambiguation note, the per-agent setup table, the endpoint. The
three cards are gone, the prompt samples are cut from five to two, and
the node_modules section is dropped since the TypeScript SDK overview
already documents it. Relabel the sidebar entry "Set Up Your Agent".

Correct the Claude Code row: the plugin ships three skills, while
px setup skills installs five, so the plugin path needs px setup skills
too. Verified by listing the plugin and running both.

On the MCP page, lead Connect with px setup mcp, merge the Cursor,
VS Code, and OpenCode accordions \(their JSON matches what px writes\),
replace the six Docs MCP accordions with a client table, and merge the
identical npm stdio configs. 633 to 521 lines with no facts dropped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: move the plugins onto the setup page

The setup page was still an index: its table pointed at Install
sections that CLI, MCP Servers, and Skills each already have. What it
lacked was the thing that belongs to no single one of them, the
one-install-per-agent path.

That path was the Plugins section, 137 lines and the largest section on
the MCP page, which is the wrong home for a plugin that registers MCP
and skills together. Move it here, after the endpoint it references,
and leave a pointer on the MCP page.

MCP drops to 388 lines and covers only MCP servers. The setup page now
routes by agent, states the shared endpoint, then gives the full plugin
steps.

Also repoint skills.mdx at the plugins section's new home, and fix its
link to a CLI heading that the CLI rewrite removed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the MCP page opener

It said "two things you might want your assistant to do" and "run
both" above a three-row table. Name the two servers that matter, your
data and the documentation, and say what the third row is.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: cut duplicated prose from the MCP page

The Configuration table repeated two env vars that self-hosting
configuration already documents; point there and keep only
PHOENIX_SKILLS_PATHS, which is unique to this page.

Monty sandbox limits were spelled out twice, in How it works and again
in Security; state them once. Merge the code-mode Warning and Note,
which both said the same thing. Trim the OAuth paragraph. Move the
OAuth2 authorization-server paragraph out of "Mounting your own
skills", where it was misfiled, into Security. Cut the npm sample
questions from seven to three.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: clarify the direction of the agents note

"Operate on Phoenix" is jargon, and "trace your sessions with a coding
agent" reads either way. Say which direction each page covers: this one
connects the agent to Phoenix, the other has Phoenix record the agent.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: correct two plugin commands that fail as written

Both found by running them. claude plugin uninstall acts on user scope
by default, so it stops with a scope error on a plugin installed at
local or project scope; say to pass the scope, and add disable, which
has the same rule. claude plugin details reports a disabled plugin as
not found, so say it has to be enabled, and say what the command lists.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: correct two CLI table rows that overstated what the commands do

Found by running them. px api has one subcommand, graphql, and rejects
mutations, so "authenticated requests to any Phoenix API route" is
wrong. The three annotation commands take only delete, and require
--all or a time window, so "stored annotations" hid that they are
destructive.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make the CLI debugging loop match what px can do

Steps 4 and 5 were titled "Turn failures into a test set" and "Work on
the prompt", but every command under them only reads, and the bodies
said "Inspect", contradicting the titles. px has no way to create a
dataset, add examples, or edit a prompt: for datasets, prompts, and
experiments it offers list, get, and delete only.

Cut the loop to the three steps px actually supports, ending at the
annotation, which with notes is the only thing it writes. Move the
dataset, experiment, and prompt reads into their own section framed as
looking up context, and say plainly that authoring happens in the UI
or the SDK.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rewrite the CLI page as the workflow an agent runs

The page had a "debugging loop" that was not a loop: it ended at an
annotation, and px cannot fix anything. Reorganising it by capability
then made it a feature list. Put it back in order as the pass an agent
actually runs, and let step 4 say plainly that px stops there and the
fix goes in your code, which is what closes the loop.

The examples were the weaker half. They repeated a fake project four
times, used placeholder ids that cannot be run, and carried comments
that restated the command instead of the output. Use real ids, say
which command feeds the next, and split the dataset, experiment, and
prompt reads into their own section, since those are for checking a
change rather than part of the debugging pass.

Add three facts found by running the commands: reads accept --format
json, annotate and add-note want the OpenTelemetry id rather than the
opaque one, and --identifier makes a repeat call overwrite.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: note that px annotation deletes are gated off by default

px span-annotations delete answers "Delete commands are disabled."
before it looks at any flag. Name the variable that opens the gate,
PHOENIX_CLI_DANGEROUSLY_ENABLE_DELETES, so the row does not send a
reader at a command that refuses to run.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say who runs px, and that the docs MCP URL is not a page

The CLI page opened by saying px is how a coding agent works with
Phoenix, then showed a reader commands to type, and titled the section
"How an agent uses it". A reader could not tell whether the commands
were theirs to run. Say it is an ordinary CLI you run yourself, and
that needing only a shell is what makes it easy to hand to an agent.

The Phoenix Docs MCP URL sat in a bare code block, so it read as a link
to open. A browser shows the tool manifest as JSON, which looks like an
error and is not. Say it is an endpoint for a client.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: answer px versus MCP, and add agent instructions to both pages

Neither page said when to reach for which. That choice only existed as
cards on the setup page, which is not where someone deciding ends up.
The CLI page now compares the two on what the client must support, what
setup costs, and how much surface each exposes, and the MCP page points
at it.

Both pages end with a section addressed to an agent reading them,
covering how to discover flags, which output format to parse, the
OpenTelemetry id the annotate commands want, the execute sandbox limits
and import allowlist, and that trace contents are untrusted data.

Every claim in both sections was run first: 116 catalog operations, the
50-call cap, json/re/datetime importable and socket/subprocess/importlib
not, exit status 0 and 1, and the delete gate.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rebuild the CLI page to a fixed outline, and align sibling names

The page now follows one order: what it is and why it matters, CLI or
MCP server, set up, what you can do with it, learn more, agent
instructions. Setup is three steps ending in a check. The examples
keep their real ids and drop the repeated --project in favour of
PHOENIX_PROJECT, set once in step two. The command table is gone; the
two caveats it carried, the delete gate and read-only GraphQL, live in
the agent instructions, and the full list is a link to the reference.

An audit of how each page in the section names its siblings found six
mismatches: the CLI page linked as "px CLI" and as bare "MCP", the
Remote MCP Server as "server", and a PXI section by a title it does
not have. Each link text now matches the page or section it points at.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: name the CLI setup check by what it proves

"Check it" said nothing. The step confirms the CLI reached the
instance, so say that, and say what success looks like.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say what the CLI setup is for before the steps

The section went straight from the heading to step one, so a reader
could not tell what was being set up or where. Name the CLI in the
heading, say to install it where the agent runs, and say what having
finished looks like.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say what px project list returns on a fresh instance, drop "has a terminal"

The setup check said a table of your projects means success, which
leaves a reader with no traces unsure what to expect. Tested on an
empty instance: every Phoenix has a project named default, so the
command always returns at least one row, and a wrong endpoint fails
with "Error fetching projects: fetch failed". Say both.

"When your agent has a terminal" was jargon on the CLI and MCP pages.
Say what it means: the agent can run shell commands, which every
coding agent can.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the CLI-first recommendation, say what the default project is

"Start with the CLI" was a preference with nothing behind it, and it
contradicted the setup page, which sends Claude Code and Codex users
to the plugin first, and the MCP page, which calls itself the primary
choice. The comparison table carries the facts; let the reader choose.
The MCP page mirror is now a plain pointer.

The setup check mentions the default project. Say what it is: created
by the initial migration, the destination for traces sent without a
project name, and empty on a fresh instance. Not a demo.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: make the CLI page work for a user with no traces yet

Run as a brand-new user, in a clean environment against an empty
instance, the previous page failed at step two: PHOENIX_PROJECT=my-app
404s on every read, and every example carried an id from one machine.
Stuff has to work before a project has traces.

Follow the shape of the live CLI section: install, endpoint, verify
with px --help and px project list, which both succeed with zero data,
then the common agent workflows as bullets and one example prompt, so
the reader sees what to ask rather than what the agent types. Say that
PHOENIX_PROJECT comes later, once traces are flowing. Keep the CLI or
MCP table and the agent instructions, and add the missing rule that
read commands need a project.

On the MCP page, restore the per-client tabs for the Docs MCP from the
live site, adding Claude Desktop.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: one story across Arize Phoenix for Agents, working from zero traces

The section was organised by plumbing, CLI, MCP, Skills, and every page
assumed traces already existed. A new user has none. Reframe around
what is true from the start: connect the agent, let it instrument your
app, then debug from what Phoenix records.

Set Up Your Agent owns routing, the endpoint, and the plugins. Its
connection check is a question to the agent, list my Phoenix projects,
which a fresh instance answers with default. Verified through both
pipes on an empty instance: px project list, and MCP search finding
getProjects and execute returning it. A new section points at the
agent-assisted tracing path as the next step.

CLI, MCP Servers, and Skills share one skeleton: what it is, install
ending in a check, what your agent can do with it, learn more,
reference. Deep material on the MCP page, API-key fallback, code mode
internals, server configuration, security, the npm server, sits under
Reference. The MCP Endpoint section folds into one line. The skills
page moves its options table and custom-skills guidance under
Reference and gains an example prompt.

Every shell command on the four pages was run in a clean environment
against an empty Phoenix. Restore the remote-mcp-server anchor, which
eleven pages link to, and repoint two onboarding snippets that still
named a CLI heading removed in an earlier rewrite.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut the instrumentation pointer back to one line

It was a sentence before, and a heading with three sentences after. The
link is the useful part.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: nest the MCP capabilities section under Remote MCP Server

The page covers two servers, so "what your agent can do with it" as a
top-level heading left "it" ambiguous. Make it an H3 under Remote MCP
Server, alongside a new Connect H3 for the setup steps.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut duplicated prose from the MCP page

The intro paragraph restated the table under it. The capabilities
section under Remote MCP Server repeated Reference, How it works,
nearly sentence for sentence; point there instead. Three sentences of
OAuth mechanism sat ahead of the first command; one line after the
accordions covers what a reader needs. The Docs MCP intro restated its
table row, the npm section said maintenance mode twice, and the
mounted-skills paragraph named its variable twice in a row.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: link the CLI page back to Set Up Your Agent

The MCP and Skills pages both point back to the setup page; the CLI
page did not, so a reader who landed there directly had no route to
the per-agent install.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: frame PXI skills as example capabilities

The table led with skill names, which is the mechanism. Lead with what
you can ask PXI to do, give one example ask per row, and keep the skill
concept as a single sentence after the table.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: frame PXI models as bring your own, and trim

Lead with the point: PXI runs on your provider and your key. Name the
providers once, keep the validated-model list and the tool-calling
caveat, drop the rest. Repoint the two links that used the old anchor.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the agent-instructions section from the CLI page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the agent-instructions section from the MCP page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the validated-model list from PXI

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: stop promising a supported-models list PXI no longer has

The list was removed; three references still pointed at it. Retitle the
Get Started link to the section that exists, and say model providers
where the Learn more line and the page description said supported
models.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: remove the npm MCP server from the MCP page

Drop the maintenance-mode section, its row in the intro table, and the
three places that offered it as a fallback: the version-floor line, the
Claude Desktop accordion, and the troubleshooting entry.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: act on the docs review: skills guidance, Docs MCP callout, MCP cut-down

Skills page: name the five skills written for Phoenix users, add the
plugin-versus-skills-add guidance from the review \(plugins track
Phoenix releases; skills add is a one-time copy, rerun it or run npx
skills update\), and take ownership of mounting your own skills from the
MCP page. Document --all honestly: tested in a fresh project it
installed 26 skills to 73 agents, including internal development
skills, so it is the wrong default.

MCP page: Phoenix Docs MCP becomes a short block near the top, as the
review asked, in place of an 87-line section. Fold Antigravity into the
other-clients accordion, trim How it works to the tool table plus two
sentences, and drop the mounting section now that Skills owns it.
361 to 271 lines.

Evaluate Your Agent: move the spans screenshot out of the Note it was
nested in.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: bring llms.txt in line with the Get Started and agents reorg

Add the CLI, Skills, and PXI Learn More pages, which had no entries.
Remove seven Quick Start entries for pages the reorg deleted or merged
into Observe Your Agent and Evaluate Your Agent, and three MCP entries
for pages folded into the single MCP page. Retitle the survivors to
match their pages.

Add the agent-usage guidance for the px CLI and the Remote MCP Server
here, where the review said it belongs, instead of on the human pages.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: give the CLI guide entry in llms.txt a unique title

It collided with the @arizeai/phoenix-cli reference entry.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten the endpoint rule on the setup page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: halve the agent-assisted setup on Observe Your Agent

Start Phoenix once above the tabs instead of in each. One sentence of
outcome per tab. Cut the pasted prompt to the two instructions that
matter, since the tracing skill carries the procedure. Drop the agent
list and the CLI pointer, which the setup page owns.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: add the PXI path to Evaluate Your Agent

A prompt to hand PXI once the dataset exists and the Playground has it
selected. Both preconditions come from running it: PXI cannot fetch a
URL, and its playground and evaluator skills act on the page you are on.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: restore the agent-assisted shape on Observe, trimmed

Back to the two tabs as they were, each self-contained, with the full
prompt. Merge the px setup outcome into one paragraph. Replace the
named --skill list with px setup skills, which installs exactly the
five Phoenix skills for every detected agent: tested via npx in a fresh
project with no CLI installed.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: repoint a PXI conversations link to the Learn More page

The Conversations section moved there when PXI split into Get Started
and Learn More; one link still pointed at the old page.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: Evaluate Your Agent builds a reference-aware evaluator

Built-in LLM templates have no reference field and score the baseline
higher than the fixed prompt, so step 2 now walks through Create new
LLM evaluator with a short rubric and the reference path mapping. PXI
section hands off after the dataset and evaluator exist, since it
cannot fetch the CSV and its own evaluator scores 0 at run time.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten Evaluate Your Agent evaluator step and PXI hand-off

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: PXI runs the full Evaluate flow from one prompt

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten Evaluate Your Agent prose

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut PXI Learn More to the essentials

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: one-line rows in the PXI capabilities table

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: shorten PXI privacy and tracing

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: compact PXI configuration table

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: shorten CLI page intro

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten MCP Servers prose

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop em dashes from Get Started snippets

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: add the follow-up prompt to the coding agent path

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: make px setup skills the primary install on the Skills page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say where the model API key goes before creating an LLM evaluator

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: the key must be on the Server tab for evaluators to use it

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say when to pick the CLI and when the MCP server

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: CLI vs MCP as best-when guidance

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop redundant line from CLI or MCP section

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: plugin rows say what the plugin covers and what is optional

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop Docs MCP aside from Set Up Your Agent

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

*…")[#16288](https://github.com/Arize-ai/phoenix/pull/16288)[)](https://github.com/Arize-ai/phoenix/commit/2afdf488e638f282f742c7daf796a09a56662971 "docs: reorganize Get Started around the agent workflow \(#16288\)

* docs: reorganize Get Started around the agent workflow

Restructure the Get Started section to mirror the Arize AX quickstart:
Instrument your agent, Evaluate your agent, Improve your agent, Coding
agent setup, PXI, and Phoenix Demo.

The Python and TypeScript quickstarts previously sat in separate nav
groups, so each language duplicated the whole section tree. Each pair is
now a single page with Python and TypeScript tabs, so the language is a
choice within a section rather than a parallel branch of the nav.

Every page body is carried over verbatim into its language tab. The only
text rewritten is internal links that pointed at the retired
ts-get-started-* paths, which now have redirects to the merged pages.

- Instrument your agent  <- get-started-tracing + ts- counterpart
- Evaluate your agent    <- get-started-evaluations + ts- counterpart
- Improve your agent     <- group over the prompt playground and
                            datasets/experiments guides
- Coding agent setup     <- group over Agent-Assisted Setup and Coding
                            Agents
- MCP group, PXI, and Phoenix Demo are unchanged aside from ordering
- End to End Features Notebook is removed from the nav; the page itself
  is left in place

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: cut duplicated prose from the Get Started guides

The previous commit put each language's full guide in its own tab, which
made every language-agnostic sentence render twice on the same page. On
the prompt playground guide that was 37% of the prose; on datasets and
experiments, 40%.

Where the two tabs told the same story, the narrative is hoisted above
the tabs and the tabs now wrap only what genuinely differs: code, videos,
the companion-code callout, and the two prompt snippets. The prompt
playground and experiments guides had identical step sequences, so their
steps are shared outright. Tracing and evaluations each have an extra
TypeScript step, so their steps stay tabbed and only the intro, prereqs
and closing sections are shared.

No prose was rewritten. Every line either survives verbatim from the
previous commit or is a prefix of one with a redundant trailing clause
removed, verified mechanically. Where the two tabs differed only
trivially, one existing sentence is kept verbatim rather than a new one
written; this also picks up several corrections the TypeScript copy
already had:

- "traces that received an incompleteness evaluation label" ->
  "an incomplete evaluation label", matching the filter beside it
- "Filter your traces by the incorrect evaluation label" -> "incomplete";
  the filter under it has always been evals\['completeness'\] == 'incomplete'
- "Using the explanations apart of the evals" -> "from the evals"
- "New application runs as a results of our task" -> "as a result"
- "ran your first experiment" / "ran an A/B test" -> "run"

Also drops, on the tracing guide, a sentence restating the bullet list
above it and three repeats of the supported-frameworks disclaimer that
the intro already carries.

Verified: all 44 code blocks byte-identical \(multiset\), every step title
and video retained, no duplicate sentence in any rendered page, both link
checkers clean, all four pages render with working tabs.

Words: 10790 -> 8789 \(18.5% shorter\).

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: tighten Instrument and Agent-Assisted Setup, repoint quickstart

Three changes to the Get Started section.

Instrument your agent opened with a paragraph of series framing --
"This guide walks through a complete workflow ... Each guide in this
series introduces one piece of that workflow" -- which is word for word
the Overview page's opening and says nothing about instrumentation. It is
removed here and still lives on the Overview page. The guide now opens on
its own subject: what a trace is.

/docs/phoenix/quickstart redirected to the Overview page, which is not in
the nav. It now lands on Instrument your agent, the actual first guide in
the series. The Overview page is unchanged and still reachable from the
four pages that link to it.

Agent-Assisted Setup is only ~350 words of prose but 89 lines, because
almost two thirds of it is follow-up reference rather than setup. The
three advanced sections -- re-running a single step, non-interactive runs,
and unsupported agents -- move into an AccordionGroup, so the page reads
as an intro, two steps and a verification section. Nothing is deleted:
all five code blocks are byte-identical and the collapsed text is still
in the page. The one prose deletion is "The fastest way to add Phoenix
tracing to your application.", which restated the frontmatter description
rendering directly above it.

Trade-off worth noting in review: collapsed sections leave the "On this
page" table of contents, which now lists only "Confirm traces are
flowing".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: hold every Get Started intro to four sentences or fewer

The intros restated themselves. Evaluate your agent opened with seven
sentences making three points: what an eval is \(said three times\), what
the guide does \(said twice\), and which metrics you can score against.

Deletions only, no rewriting. Every surviving sentence is verbatim.

Evaluate your agent, 7 -> 4. Dropped:
- "Evaluations attach quality signals to runs so that correctness or
  relevance can be reasoned about consistently instead of judged case by
  case." Restates the opening sentence and the traces/evals contrast that
  follows it, and states it as a negative.
- "We'll start with data that already exists in Phoenix, define a simple
  evaluation, and run it so we can see results directly in the UI."
  Restates "In this guide, you'll set up evaluations ... and run them on
  existing trace data".
- "The goal is to move from "I have model outputs" to "I can measure
  quality in a repeatable way."" Restates the opening sentence.

Iterate on Your Prompts, 5 -> 3. Dropped "Up to this point, we've traced
our agent runs and evaluated their outputs. Now we'll focus on prompts by
grouping failures into a dataset and iterating on prompt variants in the
Prompt Playground." -- the sentence above it already says the guide
groups failing runs into a dataset and iterates in the Playground, and
the prereq list directly below states the same continuity as two bullets.

Instrument your agent, TypeScript tab 5 -> 4. Dropped "In this guide,
we'll get tracing set up and walk through how to instrument an
application.", which the sentence after it says with the specifics. The
Python tab already carried this as one sentence.

Counts are per reader, shared intro plus their language tab, excluding
callouts: Instrument 4, Evaluate 4, Iterate 3, Optimize 3,
Agent-Assisted Setup 1.

Code blocks unchanged on all three pages.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say "agent" instead of "app" across Get Started

Refers to the reader's system as an agent throughout, matching the
section titles \("Instrument your agent", "Evaluate your agent"\). 51 lines
across the five guides plus the Overview page. Code blocks and URLs are
untouched; the `js/examples/apps/mastra-quickstart` paths and every
identifier keep their spelling.

The experiments guide's title becomes "Optimize Your Agent with
Experiments", so the nav label changes with it.

Two phrases needed care rather than a plain swap:

- "an agent application" would have become "an agent agent"; it is now
  just "an agent".
- "the agents that make up our application" would have become "the agents
  that make up our agent"; it is now "our agent system", the phrasing the
  experiments guide already uses.

Agent-Assisted Setup keeps "app" for the reader's codebase. That page is
about handing work to a *coding* agent, so "let your coding agent add
tracing to your agent" reads as one thing instrumenting itself. The
collision is worse than the inconsistency.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: merge the prompts and experiments guides into Improve your agent

"Iterate on Your Prompts" and "Optimize Your Agent with Experiments" were
two halves of one workflow split by *how* you make the change rather than
by what you are doing. Both started from eval-labelled failing traces,
both ended in the same Experiments view, and each pointed at the other as
the next step, so a reader could not tell which came first.

They are now one guide, matching the AX quickstart, which has a single
"Improve your agent" page with method tabs rather than a group.

Four steps, with the method choice appearing once instead of at every
stage:

1. Create a dataset from failed traces      \(shared\)
2. Use explanations to identify improvements \(shared; was taught twice\)
3. Make the change and run it   -- tabs: In the Prompt Playground /
   In code, with Python and TypeScript nested inside each
4. Compare versions in the Experiments view  \(shared narrative\)

Nothing is dropped. Verified against both source pages: 13 of 13 code
blocks, 12 of 12 videos, 12 of 12 frame captions, every numbered
instruction, and every H3 section.

Deliberate changes beyond the merge:

- The two cross-links between the guides are gone; they would now point
  at this page. That circularity was the reason to merge.
- "the first from Step 3" is now "the first from your baseline run",
  because the step it referenced no longer has that number.
- "A dataset created from previous runs" leaves the prerequisites, since
  step 1 builds it.
- The two "Congratulations" lines and the two "Learn More" sections are
  one each.
- About eight new connective sentences were written, the only prose on
  this page that is not carried over verbatim. Interleaving two
  workflows needs them.

Seven URLs now redirect here: both retired page paths, both ts- paths,
and the three legacy quickstart paths.

docs/phoenix.mdx had a card for each guide in both its Python and
TypeScript groups; each pair is now one "Improve Your Agent" card.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rewrite Get Started in second person, direct voice

The guides narrated themselves in the first person plural -- "we'll
install", "now that we have trace data, we can", "at this point we've
defined" -- which reads as tutorial filler and contradicts the house rule
that guides use active voice and second person. 155 instances of that
register across the section, now 0:

  royal "our"                   55 -> 0
  royal "we'll" / "we're"       44 -> 0
  "we can"                      26 -> 0
  "At this point,"               7 -> 0
  "Next, we..."                  6 -> 0
  "In this step, we..."          6 -> 0
  "Now that ..., we"             4 -> 0
  "let's"                        5 -> 0
  "More importantly,"            2 -> 0

Instructions are now imperative and the prose states facts about the
system rather than narrating a shared journey. "Next, we'll register a
tracer provider linked to a project in Phoenix. This project is where
your traces will show up" becomes "Register a tracer provider linked to a
project in Phoenix. That project is where your traces show up."

Two errors fixed on the way through:

- "seen how an agent runs shows up from start to finish" -> "how an agent
  run shows up"
- "our finanical writer agent" -> "The financial writer agent"

No instruction was dropped. Code blocks, videos, link destinations, step
titles and H3 sections are identical on all three guides: 17/2/13/9/14 on
tracing, 14/2/9/9/5 on evaluations, 13/12/5/4/9 on Improve your agent.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: sharpen the tracing opener and drop every em dash

The tracing guide opened with a sentence that did not parse: "A trace is
a record of a single run of your agent, broken down into spans that show
what happened at each step \(how agents, tasks, and tools executed\) and
provides the raw data needed for everything that follows." After the
participial phrase, "and provides" cannot reach back to "is a record", so
it reads as attaching to "spans", which would need "provide". It also
said the same thing twice \("what happened at each step" and "how agents,
tasks, and tools executed"\), and closed on "the raw data needed for
everything that follows", which was left over from the series framing
that now lives only on the Overview page.

Replaced with the house formulation from ax/instrument/what-are-traces
and phoenix/tracing/concepts-tracing, which also defines a span instead
of gesturing at it:

  A trace captures one end-to-end run of your agent as a tree of spans.
  Each span is a single step \(an agent, a task, a tool call, or an LLM
  call\) with its input, output, and timing.

The page also picks up a frontmatter description, matching the AX tracing
guide. It renders as the subtitle under the H1, so it carries the framing
while the body carries the mechanic, and the two do not restate each
other.

Em and en dashes are now zero across the section. Two were mine; two were
pre-existing on Agent-Assisted Setup, where a dash joined two independent
clauses that are clearer as separate sentences.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: stop repeating the prerequisites, and drop the long reference from Get Started

The evaluations guide stated its prerequisites twice, about fifteen lines
apart, in different words. At the top: "you'll need to have completed Get
Started with Tracing so you should have: Financial Analysis and Research
Chatbot / Trace Data in Phoenix". Then again inside step 1: "Evaluations
in Phoenix run over existing trace data. If you followed the tracing
guide, you should already have: A project in Phoenix / Traces containing
LLM inputs and outputs". Both tabs carried it, so a reader met the same
list twice per pass.

The step's version was the more precise one, so its detail is folded into
the single list at the top, which now reads "A project in Phoenix holding
traces with LLM inputs and outputs". Step 1 keeps the one fact it added,
that Phoenix evaluations run over existing trace data, and loses the
duplicate list.

Worth noting for future passes: string-similarity scanning did not catch
this, because the two lists share almost no wording. Semantic repetition
needs a read, not a regex.

The Overview page repeated itself inside single paragraphs the same way:
"using the same inputs and evaluation criteria" in two consecutive
sentences, and "spans that show what happened at each step" followed by
"Traces show how agents, tasks, and tools executed during a run". Both
are now stated once.

Coding Agents leaves the Get Started nav. It is 466 lines, 2258 words and
27 code blocks of reference material for Claude Code, Codex, the CLI, MCP
and skills, and its own opening line says "If you haven't instrumented
your app yet, start with Agent-Assisted Setup" -- it is the step after
getting started, not part of it. The page is unchanged and still listed
under Integrations > Developer Tools, and Agent-Assisted Setup already
links to it for ongoing workflows.

That leaves one page in the group, so it collapses to a single nav entry.
Agent-Assisted Setup gains sidebarTitle "Coding agent setup" so the label
still matches the agreed structure, the same way pxi.mdx shows as "PXI
Agent".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix five accuracy defects found by checking Get Started against the source

Verified the guides against the Phoenix source in this repo rather than
against other docs. Five real defects, all inherited from main:

1. `evals\['completeness'\].label == 'incomplete'` is a **span** filter. The
   guide said "Filter your traces by the incomplete evaluation label",
   which points at the traces table, where the expression is rejected:
   src/phoenix/trace/dsl/trace_filter.py refuses it with "`evals\[...\]` is
   not available in the trace filter". docs/.../filter-expressions.mdx
   lists it under span filters, and the UI has separate SpanFiltersContext
   and TraceFiltersContext. Now reads "Filter the spans table".

2. and 3. The add-to-dataset flow is span-based, not trace-based:
   AddSpansToDatasetInput carries `span_ids`
   \(src/phoenix/server/api/mutations/dataset_mutations.py:225\), and the
   dataset guide says "select the span". "Select the traces you want to
   include" and "Add the selected traces" now say spans.

4. `client = Client\(\)` ran with no import on this page. The evaluations
   guide imports it, but this page has to stand alone, so it now carries
   `from phoenix.client import Client`.

5. `import { getDataset } from "@arizeai/phoenix-client/datasets"` was
   never used. The companion example the guide points at,
   js/examples/apps/mastra-quickstart/src/mastra/experiments/experiment.ts,
   imports the identical set without it. Removed.

Everything else checked out against source: register\(project_name,
auto_instrument\), LLM\(provider, model\), ClassificationEvaluator\(name, llm,
prompt_template, choices\) and .bind\(\), Client\(\).datasets.get_dataset\(dataset=\),
Client\(\).experiments.run_experiment\(dataset=, task=, evaluators=\),
suppress_tracing's import path, the phoenix-client and phoenix-evals TS
exports, runExperiment accepting a {datasetName} selector, ports 6006 and
4317, exit code 6 as NOT_VERIFIED, every `px setup` subcommand and all
nine documented flags, and both companion code paths.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: one language choice on Improve your agent, not ten

The page had accumulated 10 separate Python | TypeScript pickers down its
length: one on the companion-code callout, one per video pair, and one
inside each half of the method tabs. Mintlify syncs tabs that share
labels, so picking a language once did switch all of them, but rendering
the control ten times reads as ten decisions.

Language is now a single choice at the top, the same shape the tracing
and evaluations guides use. Method tabs \(In the Prompt Playground /
In code\) stay where they were, inside step 3 and step 4. A reader now
sees one language picker and two method pickers instead of twelve
controls.

This is purely structural: every Python | TypeScript group was resolved
to the chosen language and the whole body wrapped in one pair. No prose
was rewritten. Verified against the previous version: all 13 code blocks,
12 videos, 12 frame captions, every numbered instruction, every heading
and every step title still present.

The cost is 78 lines. Language as the outer axis means the
language-agnostic narrative -- the dataset steps, the explanation step,
the shared prose in steps 3 and 4 -- exists once per branch. That is why
the step count in the file doubles from 4 to 8 while a reader still sees
4. Worth it for one obvious choice at the top; the alternative was
keeping the page shorter and the navigation confusing.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: method first, language inside it, on Improve your agent

Corrects the tab hierarchy to match ax/get-started/get-started-tracing,
where the outer tabs are methods \(By npx evals / By Arize Skills / By
Code\) and the Python | TypeScript picker sits inside By Code. The previous
commit put language on the outside, which was the wrong axis.

A reader now picks the method first \(In the Prompt Playground / In code\),
then the language within it. Two controls, in that order.

Also tightened the prose, since with tabs every shared line exists in more
than one branch and each cut lands more than once. Deleted as restatements
of the line beside them:

- "This dataset now represents a concrete failure case for your agent."
- "Start by grouping together the traces that didn't perform well."
- "Start from a real prompt the agent actually used." -- the next sentence
  already gives "a real starting point".
- "Saving prompt variants makes it easy to track changes..."
- "An experiment reruns the same inputs through different versions..." --
  the page intro already defines an experiment; this was the third time.
- "With two versions in hand, compare them..." -- restated the step title.

Shortened: the two-paths lead-in \(the tab labels say it\), the
create-datasets aside \(said "in code" twice\), and the task and evaluation
definitions. Fixed two royal "we" the voice pass missed: "the dataset we
made earlier" and "we should see more traces".

Line count: 605 -> 581 from the trims, then -> 694 from the inversion.
The growth is real and worth naming. Steps 1 and 2 -- create the dataset,
read the explanations -- are method-agnostic, so method-outer copies them
into both method branches, and language-inner copies them again. Four
copies of about thirty lines. A reader still sees each step once.

The wrinkle that buys: the method choice now sits above two steps that do
not depend on it, so a reader picks Playground or code before creating the
dataset. The alternative keeps steps 1 and 2 shared and moves the method
choice down to step 3, at the cost of a second visible language picker.

Verified against the previous version: every code block, video, caption,
link, heading and step title still present.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: move the companion-code callout out of the no-code path

The Prompt Playground branch offered "a companion notebook with runnable
code examples" in a path where you write no code. That was a side effect
of wrapping the whole body in the method tabs. The callout now appears
only under In code, for both languages.

The Python | TypeScript picker stays inside the Prompt Playground branch,
because that path does depend on which language you followed -- not for
the language itself, but for what you built with it. Six of the videos
record the CrewAI app rather than the Mastra app, and the dataset input
syntax differs with the agent: `{tickers}` and `{focus}` are CrewAI task
variables, `{{input}}` is Mastra. The label is a proxy for "which
quickstart did you follow".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: drop the horizontal rules bracketing the companion-code callouts

Every companion-code callout in Get Started was wrapped in `---` rules,
inherited from main. Each renders as a thematic break with generous
margins, so the callout sat in a band of empty space with a thin line
above and below it. The callout already has its own bordered container,
so the rules added separation that was not needed.

12 rules removed: four on the tracing guide, four on evaluations, four on
Improve your agent. Frontmatter delimiters are untouched, including the
multi-line description block on the tracing guide, whose closing `---`
sits at line 6 and is easy to mistake for one of these.

No prose changed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the playground template syntax, which would not have interpolated

The Python path told readers to set the User prompt to `{tickers}` and
`{focus}`. That is F_STRING syntax, and the Prompt Playground defaults to
Mustache:

  js/app/src/components/templateEditor/constants.ts
    FString:  "F_STRING",  // {variable}
    Mustache: "MUSTACHE",  // {{variable}}

  js/app/src/store/playground/playgroundStore.tsx:273
    templateFormat: TemplateFormats.Mustache,

Playground.tsx reads the format from that store, so with default settings
single braces are not substituted -- the prompt would have carried the
literal text `{tickers}`. The guide never mentioned the format selector,
so there was no way for a reader to know.

Most likely this was copied from the CrewAI task template on the tracing
guide, `Task\(description="Research: {tickers}"\)`, which legitimately uses
single braces. That is CrewAI's own templating, not Phoenix's, and it is
left alone.

Both paths now use Mustache, and the shared lead-in says so, so the two
language tabs differ only in the variable names.

The variable names themselves were already correct and are framework
driven, traceable in the guides' own code: the Python experiment passes
`example.input` straight into `kickoff\(inputs=...\)`, so dataset examples
carry `tickers` and `focus` keys, while the TypeScript task types
`example.input` as a `{role, content}\[\]` message array under a single
`input` key.

Verified rendered: the block shows `Research: {{tickers}}` literally, and
the format note appears in both language tabs.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: expand the Agent-Assisted Setup accordions back into headings

I collapsed three sections into an AccordionGroup when the page read as
too long. That hid 307 of its 542 words, 57% of the page, and dropped the
three section names from the "On this page" table of contents, leaving one
entry. It also made the page look like content had been deleted, which is
what prompted the question.

The reason for collapsing them has gone: the length concern was mostly the
466-line Coding Agents reference, which no longer sits in this nav group.
What remains is ~330 words, the shortest page in Get Started, so hiding
more than half of it costs more than it saves.

Re-run a single step, Run non-interactively \(CI or agents\), and Use an
unsupported agent are `##` headings again, so all four sections appear in
the table of contents and can be linked to directly.

Code blocks and links are identical.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: no language picker in the Prompt Playground path

You write no code in the Prompt Playground, so asking the reader to pick
Python or TypeScript there was misleading. The picker now appears only
under In code, where it selects actual code, which matches
ax/get-started/get-started-tracing.

Two controls on the page, in this order:

  In the Prompt Playground | In code
    In code -> Python | TypeScript

What the picker had been carrying in that path, and where it went:

- Steps 1, 2 and 4 were already identical between the languages.
- The dataset input keys are real, so they are stated inline: which keys
  you have depends on the agent you traced, `{{tickers}}` and `{{focus}}`
  from the Python guide, `{{input}}` from TypeScript. Both forms are now
  written out rather than left to be inferred.
- The example prompt line keeps the Python variant, which is the one that
  matches the finding step 2 states \("the report lacked financial
  ratios"\). The TypeScript variant said something different, about
  per-ticker metrics.
- Five videos drop out: ts-first-dataset, ts-first-prompt,
  ts-playground-experiment, ts-compare-prompts, ts-view-experiment. They
  carried the same captions and demonstrated the same click-path as the
  five that remain, recorded against Mastra instead of CrewAI.

All 13 code blocks and every link destination are unchanged. 668 -> 538
lines.

Braces checked: every `{{...}}` on the page is either inside a fenced
block or an inline code span, so MDX treats them as text. Verified in the
rendered DOM.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: restructure Get Started nav and verify evals against the product

Reshape the Get Started nav to Observe Your Agent / Evaluate Your Agent /
PXI / Arize Phoenix for Agents / Phoenix Demo Account. Per-language pages
become tabs within each section, with language nested inside the code path.

Merge the Improve Your Agent guide into Evaluate Your Agent, which is now
written as offline evals: a dataset feeds a task, an evaluator scores each
output. Phoenix has not released continuous scoring of live production
traffic, so the previous framing was wrong.

Extract Skills from the Coding Agents page into its own page and group the
agent-facing docs \(agent-assisted setup, skills, MCP\) under Arize Phoenix
for Agents. A pointer heading keeps existing #skills anchors resolving.

Correct six claims against Phoenix 20.15.0 running locally:

- `output` is the task's output, not an example payload. `reference` is the
  example's expected output \(experiment_runner.py:831, message_helpers.py:279,
  and the compare view's "reference output" column\).
- Creating a dataset from spans is one action, not two. You select spans and
  click Add to Dataset; creating a new dataset happens inside that popover
  \(SpanSelectionToolbar.tsx:212, DatasetSelectorPopoverContent.tsx:104\).
- The evaluator dialog's submit button is Create, not Save.
- The playground control is "Test over a dataset".
- Experiment runs live on the Experiments tab, compared via the
  "comparisons" selector.
- Playground template format defaults to Mustache, so dataset input keys
  need double braces.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: remove em dashes from the Get Started nav pages

All 98 em dashes on pages now reachable from Get Started, none of them
authored by this branch: pxi.mdx \(31\), coding-agents.mdx \(22\), remote-mcp.mdx
\(14\), mcp.mdx \(4\), docs-mcp.mdx \(3\), phoenix-mcp-server.mdx \(1\), plus 14
bullet and label dashes replaced with colons.

Each was repunctuated in context rather than swapped for a single character:
a colon where a list or restatement follows, a comma for an aside, a period
where an independent clause follows, parentheses for the three parentheticals
that span hard-wrapped lines. No dash inside a fenced code block was touched.

Diff is 89 insertions and 89 deletions, so no prose was added or removed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: point Evaluate Your Agent at the trace-scoring flow

The merged page covers offline evals over experiments. Readers who want to
score traces they have already collected had no path to it from Get Started,
since that content now lives under Tracing and Evaluation. Links to
Evaluating Phoenix Traces and the two eval quickstarts.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: simplify Phoenix get started quickstarts

* docs: keep Agent, not App, in the Get Started flow

Restores "Observe Your Agent" and "Evaluate Your Agent" and aligns the
surrounding copy, which had moved to "app": both page titles and descriptions,
the section headings on the Get Started overview, the landing-page cards, and
two "Observe Your App" cross-links that would have pointed at a page with a
different name. `app.ts` stays, since that is a file the reader creates.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: one Quick Starts card group, without the Improve card

The Improve page merged into Evaluate Your Agent, so its card pointed at the
same page as the Evaluate card. Dropped it and folded its description into
Evaluate's, which had described only the LLM-as-a-judge half.

The Python and TypeScript groups held identical cards, and both target pages
now select language with tabs, so the split no longer carried information.
Collapsed to a single group. The redirect from the retired improve path is
untouched.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make Evaluate Your Agent a single Playground path

Drops the Prompt Playground / Code tab split and keeps the Playground steps,
so the quickstart teaches one path end to end. 401 lines to 146.

Adds a "Prefer to do this in code?" section that sends readers to Run
Experiments for the Python SDK and to the TypeScript experiments reference for
runExperiment, so both languages the removed tab covered still have a home.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say which traces to evaluate and what to measure

The steps described where to click without answering the two questions a
reader arrives with. Five tweaks, 146 lines to 151:

- Dataset step gives a size to aim for and two ways to find failing traces,
  a filter expression or asking PXI to walk them
- Evaluator step names error patterns worth scoring instead of only listing
  the two evaluator types
- Saving a prompt from a trace opens with why that prompt is the one to use
- The baseline is stated as the thing later edits are measured against
- Comparison step closes with how to read the delta, including what to do
  when the score drops

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say offline evaluators in the Evaluate description

Also removes two lines the single-path change left stale: an opening sentence
that restated the description, and a paragraph telling readers to choose
between the Playground and code paths, which are no longer tabs on this page.
The offline framing replaces it, with the caveat that continuous scoring of
live production traffic has not shipped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say the manual path builds everything from the Phoenix UI

Now that this page is Playground-only, the section intro should name the
surface the steps use.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: shorten the PXI line in the dataset step

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: restore the code experiment example as its own page

Making Evaluate Your Agent a single Playground path deleted Nancy's Python
and TypeScript worked examples, and the pages it linked to instead cover the
same APIs without that scenario. Recovered them verbatim from 966510c74 as
Evaluate a Prompt Change in Code, under How-to: Experiments.

All 7 code blocks and both 4-step sequences are byte-identical to the removed
tab; only the Tab wrapper was dropped and the block de-indented one level. The
page opens by tying itself to the quickstart, so the support agent scenario
carries across the UI and code paths again.

"Prefer to do this in code?" now points here first, with Run Experiments as
the wider reference.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the new experiment page headings its siblings have

The page had no H2s, so it contributed nothing to the table of contents while
every other page in How-to: Experiments has several. Adds a heading over the
tabbed walkthrough, notes that the script creates its own dataset so it runs
without traces, and points at Set a Baseline Experiment, which covers the
persistent baseline that this page's in-code comparison does not.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rework Observe and Evaluate, split PXI into Get Started and Learn More

Observe Your Agent: intro rewritten to say what the page does and where the
concepts live, `\(Recommended\)` on the agent-assisted path, and the manual
steps reordered to start Phoenix, get your keys, then set up tracing. The
install step folded into setup, and the three steps that were identical in
both language tabs hoisted above them.

Evaluate Your Agent: no longer depends on having traces. The dataset step
hands you three travel support examples to upload, so the page runs on a
fresh Phoenix. Traces became an optional route rather than the only one.
Switched to an LLM judge, added copyable Playground prompts for the baseline
and the edit, and dropped the save-a-prompt-from-a-trace step.

PXI: 3,315 words split into a 315-word Get Started \(what it is, what it does,
three steps\) and a 1,158-word Learn More grouped as capabilities, surfaces,
configuration, and privacy. Nothing deleted except the architecture diagram
and the flag and slash-command tables, which duplicate the CLI reference.
Accordions flattened so nothing is hidden behind a click.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: consolidate Arize Phoenix for Agents

Snapshot of the agents/MCP consolidation: agent-assisted-setup folded into a
new developer-tools/cli page, and remote-mcp, docs-mcp, and phoenix-mcp-server
collapsed into one integrations/mcp page, with redirects for all four retired
paths. Nav and inbound references updated across the CLI reference, release
notes, self-hosting, and the shared onboarding snippets.

Committed from the docs-get-started-reorg worktree so the two in-flight
branches can be merged.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: point the tracing quickstart at the new CLI page

The agents consolidation folded agent-assisted-setup into
developer-tools/cli. This was the last reference still relying on the
redirect rather than the real page.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: link the uploaded dataset, and cross-link the other Observe methods

The travel-support.csv download pointed at assets/datasets/, but the file was
uploaded to assets/images/arize-docs-images/evaluate/. Repointed to where it
actually lives, so the Evaluate walkthrough runs end to end. Verified the URL
serves 3 rows with the question and expected_answer columns the page maps.

Also adds links out of both quickstarts for the routes they do not teach,
prompted by the Phoenix workflow diagram: reading traces in code, from a
coding agent via the CLI or MCP, by asking PXI, and across Sessions; plus PXI
for curating datasets and running experiments. Links only, no new pages. The
Evaluate step stays absent on purpose, since online evals have not shipped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make both Evaluate paths use one dataset, and close three story gaps

Found by running every path in Get Started end to end as a new user.

The agent-assisted prompt inlined three examples, but only one of them was in
the CSV the manual path downloads. Cutting that file from eight rows to three
had left the prompt behind. The prompt now downloads the same CSV, so there is
one dataset and the two paths cannot drift again. It also restores the page's
own argument, which turns on the visa and refund rows.

Three gaps between the paths:

- The Recommended path ended when px setup finished and never said to open the
  Traces view. Only the manual path told you to look at what you captured.
- "Ask your coding agent" said to start Phoenix without giving the command,
  which only appeared in the other tab.
- Observe handed off promising to "convert selected traces into a dataset",
  which Evaluate no longer does, since it starts from a downloadable dataset.

Verified by execution, not reading: both language paths install and run and
produce correct OpenInference traces \(a support_reply CHAIN wrapping a
ChatCompletion LLM span with messages, model, and token counts\), px setup
connects and writes .env.phoenix, both skills commands install, and the CSV
builds the same 3-example dataset from either path.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: state the Python 3.10 floor and name the Traces tab in Observe

Both quickstart paths were run end to end against Phoenix 20.16.0. On
macOS's default Python 3.9, pip quietly installs arize-phoenix-otel
0.14.0 instead of 0.17.1, because the current packages require 3.10+.
A project also opens on the Spans tab, so "the Traces view" sent
readers looking for a view that isn't the landing page.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>

* docs: make the Evaluate comparison actually move the score

The page claimed the baseline prompt invents visa rules and agrees to refunds
it should decline. Measured against gpt-5-mini it does neither: the baseline
scored 1.0, the edited prompt scored 1.0, and stripping the baseline to
"Answer the customer's question" still scored 1.0. Step 4 promised a rise that
never came, so the lesson did not land.

Fixed by changing what the examples test. They now ask about a fictional
airline's Basic fare policy, with values deliberately unlike industry norms: a
48 hour refund window, a 40 dollar change fee, one included checked bag. The
model cannot guess any of them, so the baseline answers from industry defaults
and gets all three wrong. The edit supplies the policy.

Measured: baseline 0.00, edited 1.00.

This also makes the lesson durable. The baseline fails for lack of information
rather than lack of capability, so a better model will not quietly erase the
gap the way it did with the previous examples.

Also drops the Observe handoff sentence, per review.

The dataset CSV needs re-uploading to the same path; the agent-assisted prompt
reads it by URL and needs no edit.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the PXI paths, found by running them

Tested both PXI pages against a live Phoenix for the first time.

The terminal client command did not work. `npx -y @arizeai/phoenix-cli pxi`
exits with "unknown command 'pxi'", because pxi is a separate bin in the
package rather than a px subcommand, so npx ran the default bin and passed
pxi as an argument. Needs `-p`. Verified the corrected form launches and
reaches the credential check.

PXI defaults to an Anthropic model, so a reader who follows step 1 and sets
only OPENAI_API_KEY opens the assistant and hits "Anthropic credentials are
not configured". The step now says to pick a model matching the provider you
configured. Confirmed by switching to gpt-5.5 and getting a correct answer
about project span counts.

Restores "Where your data goes" under Privacy and tracing. It was dropped with
the architecture section, but it is not architecture: it is the answer to
whether data leaves your instance. No separate Arize service, your provider
called with your key, Arize not in the request path, and remote export gated
three ways. That is the first thing a security review asks about a beta AI
feature.

Also: the provider list mixed three env var names in code with "AWS Bedrock"
in prose, so it now names providers throughout; "Drives the product" lost its
object when I trimmed it; and the roadmap milestone link is out, since a
GitHub milestone goes stale.

Verified live: the consent gate appears as described, trace saving shows
"Disabled by system settings" which matches the documented default, the model
list matches the validated set, and PXI answered using search, get_schema, and
execute, the code-mode surface the page describes.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the CLI page its own job, running Phoenix from the terminal

The page claimed px was "the recommended way for a coding agent to work with
Phoenix", then documented only px setup, which Observe already owns. So it
repeated onboarding and never showed an agent working with Phoenix. Meanwhile
px has 17 top-level commands and the page covered one.

Rewritten around the debugging loop the phoenix-cli skill actually drives:
find what broke, read one trace in full, record the judgement as an
annotation, inspect the datasets and experiments you test against, then work
on the prompt. Plus a table for the remaining commands. Setup is now a single
line pointing at Observe.

Every command was run against a live Phoenix, which caught three errors in my
own first draft:

- `px span list` has no `--filter`. It takes typed flags, so the examples use
  `--status-code ERROR` and `--span-kind LLM`.
- `px experiment list` requires `--dataset`.
- `px trace get` resolves the default project and reports "Trace not found"
  without `--project`.

Verified working: trace list, trace get, trace add-note, span list with both
filters, span annotate \(annotation upserted\), dataset list, dataset get,
experiment list, prompt list, project list, and all 11 commands in the table.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: lead the agents overview with the setup table

Move "Set up your agent" above the three-piece cards so the
per-agent map comes first, and make its lead-in name the three
pieces so it stands on its own.

Narrow the endpoint rule: the plugins and the px CLI take the base
endpoint and append /mcp themselves, while `claude mcp add` and the
per-client MCP steps take the full /mcp URL.

Reword the prompt samples as examples rather than the full set.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the agents overview one job, shorten the MCP page

The overview page was mostly navigation: three cards pointing at CLI,
MCP Servers, and Skills, which already sit next to it in the sidebar,
plus a setup table routing to the same three pages. The sidebar label
"Overview" also repeated the group name, so the page read as a section
divider.

Now it does one thing, connect a coding agent to Phoenix: what this is,
the disambiguation note, the per-agent setup table, the endpoint. The
three cards are gone, the prompt samples are cut from five to two, and
the node_modules section is dropped since the TypeScript SDK overview
already documents it. Relabel the sidebar entry "Set Up Your Agent".

Correct the Claude Code row: the plugin ships three skills, while
px setup skills installs five, so the plugin path needs px setup skills
too. Verified by listing the plugin and running both.

On the MCP page, lead Connect with px setup mcp, merge the Cursor,
VS Code, and OpenCode accordions \(their JSON matches what px writes\),
replace the six Docs MCP accordions with a client table, and merge the
identical npm stdio configs. 633 to 521 lines with no facts dropped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: move the plugins onto the setup page

The setup page was still an index: its table pointed at Install
sections that CLI, MCP Servers, and Skills each already have. What it
lacked was the thing that belongs to no single one of them, the
one-install-per-agent path.

That path was the Plugins section, 137 lines and the largest section on
the MCP page, which is the wrong home for a plugin that registers MCP
and skills together. Move it here, after the endpoint it references,
and leave a pointer on the MCP page.

MCP drops to 388 lines and covers only MCP servers. The setup page now
routes by agent, states the shared endpoint, then gives the full plugin
steps.

Also repoint skills.mdx at the plugins section's new home, and fix its
link to a CLI heading that the CLI rewrite removed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the MCP page opener

It said "two things you might want your assistant to do" and "run
both" above a three-row table. Name the two servers that matter, your
data and the documentation, and say what the third row is.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: cut duplicated prose from the MCP page

The Configuration table repeated two env vars that self-hosting
configuration already documents; point there and keep only
PHOENIX_SKILLS_PATHS, which is unique to this page.

Monty sandbox limits were spelled out twice, in How it works and again
in Security; state them once. Merge the code-mode Warning and Note,
which both said the same thing. Trim the OAuth paragraph. Move the
OAuth2 authorization-server paragraph out of "Mounting your own
skills", where it was misfiled, into Security. Cut the npm sample
questions from seven to three.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: clarify the direction of the agents note

"Operate on Phoenix" is jargon, and "trace your sessions with a coding
agent" reads either way. Say which direction each page covers: this one
connects the agent to Phoenix, the other has Phoenix record the agent.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: correct two plugin commands that fail as written

Both found by running them. claude plugin uninstall acts on user scope
by default, so it stops with a scope error on a plugin installed at
local or project scope; say to pass the scope, and add disable, which
has the same rule. claude plugin details reports a disabled plugin as
not found, so say it has to be enabled, and say what the command lists.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: correct two CLI table rows that overstated what the commands do

Found by running them. px api has one subcommand, graphql, and rejects
mutations, so "authenticated requests to any Phoenix API route" is
wrong. The three annotation commands take only delete, and require
--all or a time window, so "stored annotations" hid that they are
destructive.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make the CLI debugging loop match what px can do

Steps 4 and 5 were titled "Turn failures into a test set" and "Work on
the prompt", but every command under them only reads, and the bodies
said "Inspect", contradicting the titles. px has no way to create a
dataset, add examples, or edit a prompt: for datasets, prompts, and
experiments it offers list, get, and delete only.

Cut the loop to the three steps px actually supports, ending at the
annotation, which with notes is the only thing it writes. Move the
dataset, experiment, and prompt reads into their own section framed as
looking up context, and say plainly that authoring happens in the UI
or the SDK.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rewrite the CLI page as the workflow an agent runs

The page had a "debugging loop" that was not a loop: it ended at an
annotation, and px cannot fix anything. Reorganising it by capability
then made it a feature list. Put it back in order as the pass an agent
actually runs, and let step 4 say plainly that px stops there and the
fix goes in your code, which is what closes the loop.

The examples were the weaker half. They repeated a fake project four
times, used placeholder ids that cannot be run, and carried comments
that restated the command instead of the output. Use real ids, say
which command feeds the next, and split the dataset, experiment, and
prompt reads into their own section, since those are for checking a
change rather than part of the debugging pass.

Add three facts found by running the commands: reads accept --format
json, annotate and add-note want the OpenTelemetry id rather than the
opaque one, and --identifier makes a repeat call overwrite.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: note that px annotation deletes are gated off by default

px span-annotations delete answers "Delete commands are disabled."
before it looks at any flag. Name the variable that opens the gate,
PHOENIX_CLI_DANGEROUSLY_ENABLE_DELETES, so the row does not send a
reader at a command that refuses to run.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say who runs px, and that the docs MCP URL is not a page

The CLI page opened by saying px is how a coding agent works with
Phoenix, then showed a reader commands to type, and titled the section
"How an agent uses it". A reader could not tell whether the commands
were theirs to run. Say it is an ordinary CLI you run yourself, and
that needing only a shell is what makes it easy to hand to an agent.

The Phoenix Docs MCP URL sat in a bare code block, so it read as a link
to open. A browser shows the tool manifest as JSON, which looks like an
error and is not. Say it is an endpoint for a client.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: answer px versus MCP, and add agent instructions to both pages

Neither page said when to reach for which. That choice only existed as
cards on the setup page, which is not where someone deciding ends up.
The CLI page now compares the two on what the client must support, what
setup costs, and how much surface each exposes, and the MCP page points
at it.

Both pages end with a section addressed to an agent reading them,
covering how to discover flags, which output format to parse, the
OpenTelemetry id the annotate commands want, the execute sandbox limits
and import allowlist, and that trace contents are untrusted data.

Every claim in both sections was run first: 116 catalog operations, the
50-call cap, json/re/datetime importable and socket/subprocess/importlib
not, exit status 0 and 1, and the delete gate.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rebuild the CLI page to a fixed outline, and align sibling names

The page now follows one order: what it is and why it matters, CLI or
MCP server, set up, what you can do with it, learn more, agent
instructions. Setup is three steps ending in a check. The examples
keep their real ids and drop the repeated --project in favour of
PHOENIX_PROJECT, set once in step two. The command table is gone; the
two caveats it carried, the delete gate and read-only GraphQL, live in
the agent instructions, and the full list is a link to the reference.

An audit of how each page in the section names its siblings found six
mismatches: the CLI page linked as "px CLI" and as bare "MCP", the
Remote MCP Server as "server", and a PXI section by a title it does
not have. Each link text now matches the page or section it points at.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: name the CLI setup check by what it proves

"Check it" said nothing. The step confirms the CLI reached the
instance, so say that, and say what success looks like.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say what the CLI setup is for before the steps

The section went straight from the heading to step one, so a reader
could not tell what was being set up or where. Name the CLI in the
heading, say to install it where the agent runs, and say what having
finished looks like.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say what px project list returns on a fresh instance, drop "has a terminal"

The setup check said a table of your projects means success, which
leaves a reader with no traces unsure what to expect. Tested on an
empty instance: every Phoenix has a project named default, so the
command always returns at least one row, and a wrong endpoint fails
with "Error fetching projects: fetch failed". Say both.

"When your agent has a terminal" was jargon on the CLI and MCP pages.
Say what it means: the agent can run shell commands, which every
coding agent can.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the CLI-first recommendation, say what the default project is

"Start with the CLI" was a preference with nothing behind it, and it
contradicted the setup page, which sends Claude Code and Codex users
to the plugin first, and the MCP page, which calls itself the primary
choice. The comparison table carries the facts; let the reader choose.
The MCP page mirror is now a plain pointer.

The setup check mentions the default project. Say what it is: created
by the initial migration, the destination for traces sent without a
project name, and empty on a fresh instance. Not a demo.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: make the CLI page work for a user with no traces yet

Run as a brand-new user, in a clean environment against an empty
instance, the previous page failed at step two: PHOENIX_PROJECT=my-app
404s on every read, and every example carried an id from one machine.
Stuff has to work before a project has traces.

Follow the shape of the live CLI section: install, endpoint, verify
with px --help and px project list, which both succeed with zero data,
then the common agent workflows as bullets and one example prompt, so
the reader sees what to ask rather than what the agent types. Say that
PHOENIX_PROJECT comes later, once traces are flowing. Keep the CLI or
MCP table and the agent instructions, and add the missing rule that
read commands need a project.

On the MCP page, restore the per-client tabs for the Docs MCP from the
live site, adding Claude Desktop.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: one story across Arize Phoenix for Agents, working from zero traces

The section was organised by plumbing, CLI, MCP, Skills, and every page
assumed traces already existed. A new user has none. Reframe around
what is true from the start: connect the agent, let it instrument your
app, then debug from what Phoenix records.

Set Up Your Agent owns routing, the endpoint, and the plugins. Its
connection check is a question to the agent, list my Phoenix projects,
which a fresh instance answers with default. Verified through both
pipes on an empty instance: px project list, and MCP search finding
getProjects and execute returning it. A new section points at the
agent-assisted tracing path as the next step.

CLI, MCP Servers, and Skills share one skeleton: what it is, install
ending in a check, what your agent can do with it, learn more,
reference. Deep material on the MCP page, API-key fallback, code mode
internals, server configuration, security, the npm server, sits under
Reference. The MCP Endpoint section folds into one line. The skills
page moves its options table and custom-skills guidance under
Reference and gains an example prompt.

Every shell command on the four pages was run in a clean environment
against an empty Phoenix. Restore the remote-mcp-server anchor, which
eleven pages link to, and repoint two onboarding snippets that still
named a CLI heading removed in an earlier rewrite.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut the instrumentation pointer back to one line

It was a sentence before, and a heading with three sentences after. The
link is the useful part.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: nest the MCP capabilities section under Remote MCP Server

The page covers two servers, so "what your agent can do with it" as a
top-level heading left "it" ambiguous. Make it an H3 under Remote MCP
Server, alongside a new Connect H3 for the setup steps.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut duplicated prose from the MCP page

The intro paragraph restated the table under it. The capabilities
section under Remote MCP Server repeated Reference, How it works,
nearly sentence for sentence; point there instead. Three sentences of
OAuth mechanism sat ahead of the first command; one line after the
accordions covers what a reader needs. The Docs MCP intro restated its
table row, the npm section said maintenance mode twice, and the
mounted-skills paragraph named its variable twice in a row.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: link the CLI page back to Set Up Your Agent

The MCP and Skills pages both point back to the setup page; the CLI
page did not, so a reader who landed there directly had no route to
the per-agent install.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: frame PXI skills as example capabilities

The table led with skill names, which is the mechanism. Lead with what
you can ask PXI to do, give one example ask per row, and keep the skill
concept as a single sentence after the table.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: frame PXI models as bring your own, and trim

Lead with the point: PXI runs on your provider and your key. Name the
providers once, keep the validated-model list and the tool-calling
caveat, drop the rest. Repoint the two links that used the old anchor.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the agent-instructions section from the CLI page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the agent-instructions section from the MCP page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the validated-model list from PXI

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: stop promising a supported-models list PXI no longer has

The list was removed; three references still pointed at it. Retitle the
Get Started link to the section that exists, and say model providers
where the Learn more line and the page description said supported
models.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: remove the npm MCP server from the MCP page

Drop the maintenance-mode section, its row in the intro table, and the
three places that offered it as a fallback: the version-floor line, the
Claude Desktop accordion, and the troubleshooting entry.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: act on the docs review: skills guidance, Docs MCP callout, MCP cut-down

Skills page: name the five skills written for Phoenix users, add the
plugin-versus-skills-add guidance from the review \(plugins track
Phoenix releases; skills add is a one-time copy, rerun it or run npx
skills update\), and take ownership of mounting your own skills from the
MCP page. Document --all honestly: tested in a fresh project it
installed 26 skills to 73 agents, including internal development
skills, so it is the wrong default.

MCP page: Phoenix Docs MCP becomes a short block near the top, as the
review asked, in place of an 87-line section. Fold Antigravity into the
other-clients accordion, trim How it works to the tool table plus two
sentences, and drop the mounting section now that Skills owns it.
361 to 271 lines.

Evaluate Your Agent: move the spans screenshot out of the Note it was
nested in.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: bring llms.txt in line with the Get Started and agents reorg

Add the CLI, Skills, and PXI Learn More pages, which had no entries.
Remove seven Quick Start entries for pages the reorg deleted or merged
into Observe Your Agent and Evaluate Your Agent, and three MCP entries
for pages folded into the single MCP page. Retitle the survivors to
match their pages.

Add the agent-usage guidance for the px CLI and the Remote MCP Server
here, where the review said it belongs, instead of on the human pages.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: give the CLI guide entry in llms.txt a unique title

It collided with the @arizeai/phoenix-cli reference entry.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten the endpoint rule on the setup page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: halve the agent-assisted setup on Observe Your Agent

Start Phoenix once above the tabs instead of in each. One sentence of
outcome per tab. Cut the pasted prompt to the two instructions that
matter, since the tracing skill carries the procedure. Drop the agent
list and the CLI pointer, which the setup page owns.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: add the PXI path to Evaluate Your Agent

A prompt to hand PXI once the dataset exists and the Playground has it
selected. Both preconditions come from running it: PXI cannot fetch a
URL, and its playground and evaluator skills act on the page you are on.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: restore the agent-assisted shape on Observe, trimmed

Back to the two tabs as they were, each self-contained, with the full
prompt. Merge the px setup outcome into one paragraph. Replace the
named --skill list with px setup skills, which installs exactly the
five Phoenix skills for every detected agent: tested via npx in a fresh
project with no CLI installed.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: repoint a PXI conversations link to the Learn More page

The Conversations section moved there when PXI split into Get Started
and Learn More; one link still pointed at the old page.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: Evaluate Your Agent builds a reference-aware evaluator

Built-in LLM templates have no reference field and score the baseline
higher than the fixed prompt, so step 2 now walks through Create new
LLM evaluator with a short rubric and the reference path mapping. PXI
section hands off after the dataset and evaluator exist, since it
cannot fetch the CSV and its own evaluator scores 0 at run time.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten Evaluate Your Agent evaluator step and PXI hand-off

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: PXI runs the full Evaluate flow from one prompt

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten Evaluate Your Agent prose

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut PXI Learn More to the essentials

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: one-line rows in the PXI capabilities table

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: shorten PXI privacy and tracing

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: compact PXI configuration table

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: shorten CLI page intro

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten MCP Servers prose

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop em dashes from Get Started snippets

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: add the follow-up prompt to the coding agent path

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: make px setup skills the primary install on the Skills page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say where the model API key goes before creating an LLM evaluator

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: the key must be on the Server tab for evaluators to use it

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say when to pick the CLI and when the MCP server

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: CLI vs MCP as best-when guidance

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop redundant line from CLI or MCP section

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: plugin rows say what the plugin covers and what is optional

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop Docs MCP aside from Set Up Your Agent

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

*…")  | Oct 9, 2026  |  
|   |   |   | Jul 2, 2024  |  
|   |   |  [docs: reorganize Get Started around the agent workflow (](https://github.com/Arize-ai/phoenix/commit/2afdf488e638f282f742c7daf796a09a56662971 "docs: reorganize Get Started around the agent workflow \(#16288\)

* docs: reorganize Get Started around the agent workflow

Restructure the Get Started section to mirror the Arize AX quickstart:
Instrument your agent, Evaluate your agent, Improve your agent, Coding
agent setup, PXI, and Phoenix Demo.

The Python and TypeScript quickstarts previously sat in separate nav
groups, so each language duplicated the whole section tree. Each pair is
now a single page with Python and TypeScript tabs, so the language is a
choice within a section rather than a parallel branch of the nav.

Every page body is carried over verbatim into its language tab. The only
text rewritten is internal links that pointed at the retired
ts-get-started-* paths, which now have redirects to the merged pages.

- Instrument your agent  <- get-started-tracing + ts- counterpart
- Evaluate your agent    <- get-started-evaluations + ts- counterpart
- Improve your agent     <- group over the prompt playground and
                            datasets/experiments guides
- Coding agent setup     <- group over Agent-Assisted Setup and Coding
                            Agents
- MCP group, PXI, and Phoenix Demo are unchanged aside from ordering
- End to End Features Notebook is removed from the nav; the page itself
  is left in place

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: cut duplicated prose from the Get Started guides

The previous commit put each language's full guide in its own tab, which
made every language-agnostic sentence render twice on the same page. On
the prompt playground guide that was 37% of the prose; on datasets and
experiments, 40%.

Where the two tabs told the same story, the narrative is hoisted above
the tabs and the tabs now wrap only what genuinely differs: code, videos,
the companion-code callout, and the two prompt snippets. The prompt
playground and experiments guides had identical step sequences, so their
steps are shared outright. Tracing and evaluations each have an extra
TypeScript step, so their steps stay tabbed and only the intro, prereqs
and closing sections are shared.

No prose was rewritten. Every line either survives verbatim from the
previous commit or is a prefix of one with a redundant trailing clause
removed, verified mechanically. Where the two tabs differed only
trivially, one existing sentence is kept verbatim rather than a new one
written; this also picks up several corrections the TypeScript copy
already had:

- "traces that received an incompleteness evaluation label" ->
  "an incomplete evaluation label", matching the filter beside it
- "Filter your traces by the incorrect evaluation label" -> "incomplete";
  the filter under it has always been evals\['completeness'\] == 'incomplete'
- "Using the explanations apart of the evals" -> "from the evals"
- "New application runs as a results of our task" -> "as a result"
- "ran your first experiment" / "ran an A/B test" -> "run"

Also drops, on the tracing guide, a sentence restating the bullet list
above it and three repeats of the supported-frameworks disclaimer that
the intro already carries.

Verified: all 44 code blocks byte-identical \(multiset\), every step title
and video retained, no duplicate sentence in any rendered page, both link
checkers clean, all four pages render with working tabs.

Words: 10790 -> 8789 \(18.5% shorter\).

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: tighten Instrument and Agent-Assisted Setup, repoint quickstart

Three changes to the Get Started section.

Instrument your agent opened with a paragraph of series framing --
"This guide walks through a complete workflow ... Each guide in this
series introduces one piece of that workflow" -- which is word for word
the Overview page's opening and says nothing about instrumentation. It is
removed here and still lives on the Overview page. The guide now opens on
its own subject: what a trace is.

/docs/phoenix/quickstart redirected to the Overview page, which is not in
the nav. It now lands on Instrument your agent, the actual first guide in
the series. The Overview page is unchanged and still reachable from the
four pages that link to it.

Agent-Assisted Setup is only ~350 words of prose but 89 lines, because
almost two thirds of it is follow-up reference rather than setup. The
three advanced sections -- re-running a single step, non-interactive runs,
and unsupported agents -- move into an AccordionGroup, so the page reads
as an intro, two steps and a verification section. Nothing is deleted:
all five code blocks are byte-identical and the collapsed text is still
in the page. The one prose deletion is "The fastest way to add Phoenix
tracing to your application.", which restated the frontmatter description
rendering directly above it.

Trade-off worth noting in review: collapsed sections leave the "On this
page" table of contents, which now lists only "Confirm traces are
flowing".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: hold every Get Started intro to four sentences or fewer

The intros restated themselves. Evaluate your agent opened with seven
sentences making three points: what an eval is \(said three times\), what
the guide does \(said twice\), and which metrics you can score against.

Deletions only, no rewriting. Every surviving sentence is verbatim.

Evaluate your agent, 7 -> 4. Dropped:
- "Evaluations attach quality signals to runs so that correctness or
  relevance can be reasoned about consistently instead of judged case by
  case." Restates the opening sentence and the traces/evals contrast that
  follows it, and states it as a negative.
- "We'll start with data that already exists in Phoenix, define a simple
  evaluation, and run it so we can see results directly in the UI."
  Restates "In this guide, you'll set up evaluations ... and run them on
  existing trace data".
- "The goal is to move from "I have model outputs" to "I can measure
  quality in a repeatable way."" Restates the opening sentence.

Iterate on Your Prompts, 5 -> 3. Dropped "Up to this point, we've traced
our agent runs and evaluated their outputs. Now we'll focus on prompts by
grouping failures into a dataset and iterating on prompt variants in the
Prompt Playground." -- the sentence above it already says the guide
groups failing runs into a dataset and iterates in the Playground, and
the prereq list directly below states the same continuity as two bullets.

Instrument your agent, TypeScript tab 5 -> 4. Dropped "In this guide,
we'll get tracing set up and walk through how to instrument an
application.", which the sentence after it says with the specifics. The
Python tab already carried this as one sentence.

Counts are per reader, shared intro plus their language tab, excluding
callouts: Instrument 4, Evaluate 4, Iterate 3, Optimize 3,
Agent-Assisted Setup 1.

Code blocks unchanged on all three pages.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say "agent" instead of "app" across Get Started

Refers to the reader's system as an agent throughout, matching the
section titles \("Instrument your agent", "Evaluate your agent"\). 51 lines
across the five guides plus the Overview page. Code blocks and URLs are
untouched; the `js/examples/apps/mastra-quickstart` paths and every
identifier keep their spelling.

The experiments guide's title becomes "Optimize Your Agent with
Experiments", so the nav label changes with it.

Two phrases needed care rather than a plain swap:

- "an agent application" would have become "an agent agent"; it is now
  just "an agent".
- "the agents that make up our application" would have become "the agents
  that make up our agent"; it is now "our agent system", the phrasing the
  experiments guide already uses.

Agent-Assisted Setup keeps "app" for the reader's codebase. That page is
about handing work to a *coding* agent, so "let your coding agent add
tracing to your agent" reads as one thing instrumenting itself. The
collision is worse than the inconsistency.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: merge the prompts and experiments guides into Improve your agent

"Iterate on Your Prompts" and "Optimize Your Agent with Experiments" were
two halves of one workflow split by *how* you make the change rather than
by what you are doing. Both started from eval-labelled failing traces,
both ended in the same Experiments view, and each pointed at the other as
the next step, so a reader could not tell which came first.

They are now one guide, matching the AX quickstart, which has a single
"Improve your agent" page with method tabs rather than a group.

Four steps, with the method choice appearing once instead of at every
stage:

1. Create a dataset from failed traces      \(shared\)
2. Use explanations to identify improvements \(shared; was taught twice\)
3. Make the change and run it   -- tabs: In the Prompt Playground /
   In code, with Python and TypeScript nested inside each
4. Compare versions in the Experiments view  \(shared narrative\)

Nothing is dropped. Verified against both source pages: 13 of 13 code
blocks, 12 of 12 videos, 12 of 12 frame captions, every numbered
instruction, and every H3 section.

Deliberate changes beyond the merge:

- The two cross-links between the guides are gone; they would now point
  at this page. That circularity was the reason to merge.
- "the first from Step 3" is now "the first from your baseline run",
  because the step it referenced no longer has that number.
- "A dataset created from previous runs" leaves the prerequisites, since
  step 1 builds it.
- The two "Congratulations" lines and the two "Learn More" sections are
  one each.
- About eight new connective sentences were written, the only prose on
  this page that is not carried over verbatim. Interleaving two
  workflows needs them.

Seven URLs now redirect here: both retired page paths, both ts- paths,
and the three legacy quickstart paths.

docs/phoenix.mdx had a card for each guide in both its Python and
TypeScript groups; each pair is now one "Improve Your Agent" card.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rewrite Get Started in second person, direct voice

The guides narrated themselves in the first person plural -- "we'll
install", "now that we have trace data, we can", "at this point we've
defined" -- which reads as tutorial filler and contradicts the house rule
that guides use active voice and second person. 155 instances of that
register across the section, now 0:

  royal "our"                   55 -> 0
  royal "we'll" / "we're"       44 -> 0
  "we can"                      26 -> 0
  "At this point,"               7 -> 0
  "Next, we..."                  6 -> 0
  "In this step, we..."          6 -> 0
  "Now that ..., we"             4 -> 0
  "let's"                        5 -> 0
  "More importantly,"            2 -> 0

Instructions are now imperative and the prose states facts about the
system rather than narrating a shared journey. "Next, we'll register a
tracer provider linked to a project in Phoenix. This project is where
your traces will show up" becomes "Register a tracer provider linked to a
project in Phoenix. That project is where your traces show up."

Two errors fixed on the way through:

- "seen how an agent runs shows up from start to finish" -> "how an agent
  run shows up"
- "our finanical writer agent" -> "The financial writer agent"

No instruction was dropped. Code blocks, videos, link destinations, step
titles and H3 sections are identical on all three guides: 17/2/13/9/14 on
tracing, 14/2/9/9/5 on evaluations, 13/12/5/4/9 on Improve your agent.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: sharpen the tracing opener and drop every em dash

The tracing guide opened with a sentence that did not parse: "A trace is
a record of a single run of your agent, broken down into spans that show
what happened at each step \(how agents, tasks, and tools executed\) and
provides the raw data needed for everything that follows." After the
participial phrase, "and provides" cannot reach back to "is a record", so
it reads as attaching to "spans", which would need "provide". It also
said the same thing twice \("what happened at each step" and "how agents,
tasks, and tools executed"\), and closed on "the raw data needed for
everything that follows", which was left over from the series framing
that now lives only on the Overview page.

Replaced with the house formulation from ax/instrument/what-are-traces
and phoenix/tracing/concepts-tracing, which also defines a span instead
of gesturing at it:

  A trace captures one end-to-end run of your agent as a tree of spans.
  Each span is a single step \(an agent, a task, a tool call, or an LLM
  call\) with its input, output, and timing.

The page also picks up a frontmatter description, matching the AX tracing
guide. It renders as the subtitle under the H1, so it carries the framing
while the body carries the mechanic, and the two do not restate each
other.

Em and en dashes are now zero across the section. Two were mine; two were
pre-existing on Agent-Assisted Setup, where a dash joined two independent
clauses that are clearer as separate sentences.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: stop repeating the prerequisites, and drop the long reference from Get Started

The evaluations guide stated its prerequisites twice, about fifteen lines
apart, in different words. At the top: "you'll need to have completed Get
Started with Tracing so you should have: Financial Analysis and Research
Chatbot / Trace Data in Phoenix". Then again inside step 1: "Evaluations
in Phoenix run over existing trace data. If you followed the tracing
guide, you should already have: A project in Phoenix / Traces containing
LLM inputs and outputs". Both tabs carried it, so a reader met the same
list twice per pass.

The step's version was the more precise one, so its detail is folded into
the single list at the top, which now reads "A project in Phoenix holding
traces with LLM inputs and outputs". Step 1 keeps the one fact it added,
that Phoenix evaluations run over existing trace data, and loses the
duplicate list.

Worth noting for future passes: string-similarity scanning did not catch
this, because the two lists share almost no wording. Semantic repetition
needs a read, not a regex.

The Overview page repeated itself inside single paragraphs the same way:
"using the same inputs and evaluation criteria" in two consecutive
sentences, and "spans that show what happened at each step" followed by
"Traces show how agents, tasks, and tools executed during a run". Both
are now stated once.

Coding Agents leaves the Get Started nav. It is 466 lines, 2258 words and
27 code blocks of reference material for Claude Code, Codex, the CLI, MCP
and skills, and its own opening line says "If you haven't instrumented
your app yet, start with Agent-Assisted Setup" -- it is the step after
getting started, not part of it. The page is unchanged and still listed
under Integrations > Developer Tools, and Agent-Assisted Setup already
links to it for ongoing workflows.

That leaves one page in the group, so it collapses to a single nav entry.
Agent-Assisted Setup gains sidebarTitle "Coding agent setup" so the label
still matches the agreed structure, the same way pxi.mdx shows as "PXI
Agent".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix five accuracy defects found by checking Get Started against the source

Verified the guides against the Phoenix source in this repo rather than
against other docs. Five real defects, all inherited from main:

1. `evals\['completeness'\].label == 'incomplete'` is a **span** filter. The
   guide said "Filter your traces by the incomplete evaluation label",
   which points at the traces table, where the expression is rejected:
   src/phoenix/trace/dsl/trace_filter.py refuses it with "`evals\[...\]` is
   not available in the trace filter". docs/.../filter-expressions.mdx
   lists it under span filters, and the UI has separate SpanFiltersContext
   and TraceFiltersContext. Now reads "Filter the spans table".

2. and 3. The add-to-dataset flow is span-based, not trace-based:
   AddSpansToDatasetInput carries `span_ids`
   \(src/phoenix/server/api/mutations/dataset_mutations.py:225\), and the
   dataset guide says "select the span". "Select the traces you want to
   include" and "Add the selected traces" now say spans.

4. `client = Client\(\)` ran with no import on this page. The evaluations
   guide imports it, but this page has to stand alone, so it now carries
   `from phoenix.client import Client`.

5. `import { getDataset } from "@arizeai/phoenix-client/datasets"` was
   never used. The companion example the guide points at,
   js/examples/apps/mastra-quickstart/src/mastra/experiments/experiment.ts,
   imports the identical set without it. Removed.

Everything else checked out against source: register\(project_name,
auto_instrument\), LLM\(provider, model\), ClassificationEvaluator\(name, llm,
prompt_template, choices\) and .bind\(\), Client\(\).datasets.get_dataset\(dataset=\),
Client\(\).experiments.run_experiment\(dataset=, task=, evaluators=\),
suppress_tracing's import path, the phoenix-client and phoenix-evals TS
exports, runExperiment accepting a {datasetName} selector, ports 6006 and
4317, exit code 6 as NOT_VERIFIED, every `px setup` subcommand and all
nine documented flags, and both companion code paths.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: one language choice on Improve your agent, not ten

The page had accumulated 10 separate Python | TypeScript pickers down its
length: one on the companion-code callout, one per video pair, and one
inside each half of the method tabs. Mintlify syncs tabs that share
labels, so picking a language once did switch all of them, but rendering
the control ten times reads as ten decisions.

Language is now a single choice at the top, the same shape the tracing
and evaluations guides use. Method tabs \(In the Prompt Playground /
In code\) stay where they were, inside step 3 and step 4. A reader now
sees one language picker and two method pickers instead of twelve
controls.

This is purely structural: every Python | TypeScript group was resolved
to the chosen language and the whole body wrapped in one pair. No prose
was rewritten. Verified against the previous version: all 13 code blocks,
12 videos, 12 frame captions, every numbered instruction, every heading
and every step title still present.

The cost is 78 lines. Language as the outer axis means the
language-agnostic narrative -- the dataset steps, the explanation step,
the shared prose in steps 3 and 4 -- exists once per branch. That is why
the step count in the file doubles from 4 to 8 while a reader still sees
4. Worth it for one obvious choice at the top; the alternative was
keeping the page shorter and the navigation confusing.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: method first, language inside it, on Improve your agent

Corrects the tab hierarchy to match ax/get-started/get-started-tracing,
where the outer tabs are methods \(By npx evals / By Arize Skills / By
Code\) and the Python | TypeScript picker sits inside By Code. The previous
commit put language on the outside, which was the wrong axis.

A reader now picks the method first \(In the Prompt Playground / In code\),
then the language within it. Two controls, in that order.

Also tightened the prose, since with tabs every shared line exists in more
than one branch and each cut lands more than once. Deleted as restatements
of the line beside them:

- "This dataset now represents a concrete failure case for your agent."
- "Start by grouping together the traces that didn't perform well."
- "Start from a real prompt the agent actually used." -- the next sentence
  already gives "a real starting point".
- "Saving prompt variants makes it easy to track changes..."
- "An experiment reruns the same inputs through different versions..." --
  the page intro already defines an experiment; this was the third time.
- "With two versions in hand, compare them..." -- restated the step title.

Shortened: the two-paths lead-in \(the tab labels say it\), the
create-datasets aside \(said "in code" twice\), and the task and evaluation
definitions. Fixed two royal "we" the voice pass missed: "the dataset we
made earlier" and "we should see more traces".

Line count: 605 -> 581 from the trims, then -> 694 from the inversion.
The growth is real and worth naming. Steps 1 and 2 -- create the dataset,
read the explanations -- are method-agnostic, so method-outer copies them
into both method branches, and language-inner copies them again. Four
copies of about thirty lines. A reader still sees each step once.

The wrinkle that buys: the method choice now sits above two steps that do
not depend on it, so a reader picks Playground or code before creating the
dataset. The alternative keeps steps 1 and 2 shared and moves the method
choice down to step 3, at the cost of a second visible language picker.

Verified against the previous version: every code block, video, caption,
link, heading and step title still present.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: move the companion-code callout out of the no-code path

The Prompt Playground branch offered "a companion notebook with runnable
code examples" in a path where you write no code. That was a side effect
of wrapping the whole body in the method tabs. The callout now appears
only under In code, for both languages.

The Python | TypeScript picker stays inside the Prompt Playground branch,
because that path does depend on which language you followed -- not for
the language itself, but for what you built with it. Six of the videos
record the CrewAI app rather than the Mastra app, and the dataset input
syntax differs with the agent: `{tickers}` and `{focus}` are CrewAI task
variables, `{{input}}` is Mastra. The label is a proxy for "which
quickstart did you follow".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: drop the horizontal rules bracketing the companion-code callouts

Every companion-code callout in Get Started was wrapped in `---` rules,
inherited from main. Each renders as a thematic break with generous
margins, so the callout sat in a band of empty space with a thin line
above and below it. The callout already has its own bordered container,
so the rules added separation that was not needed.

12 rules removed: four on the tracing guide, four on evaluations, four on
Improve your agent. Frontmatter delimiters are untouched, including the
multi-line description block on the tracing guide, whose closing `---`
sits at line 6 and is easy to mistake for one of these.

No prose changed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the playground template syntax, which would not have interpolated

The Python path told readers to set the User prompt to `{tickers}` and
`{focus}`. That is F_STRING syntax, and the Prompt Playground defaults to
Mustache:

  js/app/src/components/templateEditor/constants.ts
    FString:  "F_STRING",  // {variable}
    Mustache: "MUSTACHE",  // {{variable}}

  js/app/src/store/playground/playgroundStore.tsx:273
    templateFormat: TemplateFormats.Mustache,

Playground.tsx reads the format from that store, so with default settings
single braces are not substituted -- the prompt would have carried the
literal text `{tickers}`. The guide never mentioned the format selector,
so there was no way for a reader to know.

Most likely this was copied from the CrewAI task template on the tracing
guide, `Task\(description="Research: {tickers}"\)`, which legitimately uses
single braces. That is CrewAI's own templating, not Phoenix's, and it is
left alone.

Both paths now use Mustache, and the shared lead-in says so, so the two
language tabs differ only in the variable names.

The variable names themselves were already correct and are framework
driven, traceable in the guides' own code: the Python experiment passes
`example.input` straight into `kickoff\(inputs=...\)`, so dataset examples
carry `tickers` and `focus` keys, while the TypeScript task types
`example.input` as a `{role, content}\[\]` message array under a single
`input` key.

Verified rendered: the block shows `Research: {{tickers}}` literally, and
the format note appears in both language tabs.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: expand the Agent-Assisted Setup accordions back into headings

I collapsed three sections into an AccordionGroup when the page read as
too long. That hid 307 of its 542 words, 57% of the page, and dropped the
three section names from the "On this page" table of contents, leaving one
entry. It also made the page look like content had been deleted, which is
what prompted the question.

The reason for collapsing them has gone: the length concern was mostly the
466-line Coding Agents reference, which no longer sits in this nav group.
What remains is ~330 words, the shortest page in Get Started, so hiding
more than half of it costs more than it saves.

Re-run a single step, Run non-interactively \(CI or agents\), and Use an
unsupported agent are `##` headings again, so all four sections appear in
the table of contents and can be linked to directly.

Code blocks and links are identical.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: no language picker in the Prompt Playground path

You write no code in the Prompt Playground, so asking the reader to pick
Python or TypeScript there was misleading. The picker now appears only
under In code, where it selects actual code, which matches
ax/get-started/get-started-tracing.

Two controls on the page, in this order:

  In the Prompt Playground | In code
    In code -> Python | TypeScript

What the picker had been carrying in that path, and where it went:

- Steps 1, 2 and 4 were already identical between the languages.
- The dataset input keys are real, so they are stated inline: which keys
  you have depends on the agent you traced, `{{tickers}}` and `{{focus}}`
  from the Python guide, `{{input}}` from TypeScript. Both forms are now
  written out rather than left to be inferred.
- The example prompt line keeps the Python variant, which is the one that
  matches the finding step 2 states \("the report lacked financial
  ratios"\). The TypeScript variant said something different, about
  per-ticker metrics.
- Five videos drop out: ts-first-dataset, ts-first-prompt,
  ts-playground-experiment, ts-compare-prompts, ts-view-experiment. They
  carried the same captions and demonstrated the same click-path as the
  five that remain, recorded against Mastra instead of CrewAI.

All 13 code blocks and every link destination are unchanged. 668 -> 538
lines.

Braces checked: every `{{...}}` on the page is either inside a fenced
block or an inline code span, so MDX treats them as text. Verified in the
rendered DOM.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: restructure Get Started nav and verify evals against the product

Reshape the Get Started nav to Observe Your Agent / Evaluate Your Agent /
PXI / Arize Phoenix for Agents / Phoenix Demo Account. Per-language pages
become tabs within each section, with language nested inside the code path.

Merge the Improve Your Agent guide into Evaluate Your Agent, which is now
written as offline evals: a dataset feeds a task, an evaluator scores each
output. Phoenix has not released continuous scoring of live production
traffic, so the previous framing was wrong.

Extract Skills from the Coding Agents page into its own page and group the
agent-facing docs \(agent-assisted setup, skills, MCP\) under Arize Phoenix
for Agents. A pointer heading keeps existing #skills anchors resolving.

Correct six claims against Phoenix 20.15.0 running locally:

- `output` is the task's output, not an example payload. `reference` is the
  example's expected output \(experiment_runner.py:831, message_helpers.py:279,
  and the compare view's "reference output" column\).
- Creating a dataset from spans is one action, not two. You select spans and
  click Add to Dataset; creating a new dataset happens inside that popover
  \(SpanSelectionToolbar.tsx:212, DatasetSelectorPopoverContent.tsx:104\).
- The evaluator dialog's submit button is Create, not Save.
- The playground control is "Test over a dataset".
- Experiment runs live on the Experiments tab, compared via the
  "comparisons" selector.
- Playground template format defaults to Mustache, so dataset input keys
  need double braces.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: remove em dashes from the Get Started nav pages

All 98 em dashes on pages now reachable from Get Started, none of them
authored by this branch: pxi.mdx \(31\), coding-agents.mdx \(22\), remote-mcp.mdx
\(14\), mcp.mdx \(4\), docs-mcp.mdx \(3\), phoenix-mcp-server.mdx \(1\), plus 14
bullet and label dashes replaced with colons.

Each was repunctuated in context rather than swapped for a single character:
a colon where a list or restatement follows, a comma for an aside, a period
where an independent clause follows, parentheses for the three parentheticals
that span hard-wrapped lines. No dash inside a fenced code block was touched.

Diff is 89 insertions and 89 deletions, so no prose was added or removed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: point Evaluate Your Agent at the trace-scoring flow

The merged page covers offline evals over experiments. Readers who want to
score traces they have already collected had no path to it from Get Started,
since that content now lives under Tracing and Evaluation. Links to
Evaluating Phoenix Traces and the two eval quickstarts.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: simplify Phoenix get started quickstarts

* docs: keep Agent, not App, in the Get Started flow

Restores "Observe Your Agent" and "Evaluate Your Agent" and aligns the
surrounding copy, which had moved to "app": both page titles and descriptions,
the section headings on the Get Started overview, the landing-page cards, and
two "Observe Your App" cross-links that would have pointed at a page with a
different name. `app.ts` stays, since that is a file the reader creates.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: one Quick Starts card group, without the Improve card

The Improve page merged into Evaluate Your Agent, so its card pointed at the
same page as the Evaluate card. Dropped it and folded its description into
Evaluate's, which had described only the LLM-as-a-judge half.

The Python and TypeScript groups held identical cards, and both target pages
now select language with tabs, so the split no longer carried information.
Collapsed to a single group. The redirect from the retired improve path is
untouched.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make Evaluate Your Agent a single Playground path

Drops the Prompt Playground / Code tab split and keeps the Playground steps,
so the quickstart teaches one path end to end. 401 lines to 146.

Adds a "Prefer to do this in code?" section that sends readers to Run
Experiments for the Python SDK and to the TypeScript experiments reference for
runExperiment, so both languages the removed tab covered still have a home.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say which traces to evaluate and what to measure

The steps described where to click without answering the two questions a
reader arrives with. Five tweaks, 146 lines to 151:

- Dataset step gives a size to aim for and two ways to find failing traces,
  a filter expression or asking PXI to walk them
- Evaluator step names error patterns worth scoring instead of only listing
  the two evaluator types
- Saving a prompt from a trace opens with why that prompt is the one to use
- The baseline is stated as the thing later edits are measured against
- Comparison step closes with how to read the delta, including what to do
  when the score drops

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say offline evaluators in the Evaluate description

Also removes two lines the single-path change left stale: an opening sentence
that restated the description, and a paragraph telling readers to choose
between the Playground and code paths, which are no longer tabs on this page.
The offline framing replaces it, with the caveat that continuous scoring of
live production traffic has not shipped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say the manual path builds everything from the Phoenix UI

Now that this page is Playground-only, the section intro should name the
surface the steps use.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: shorten the PXI line in the dataset step

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: restore the code experiment example as its own page

Making Evaluate Your Agent a single Playground path deleted Nancy's Python
and TypeScript worked examples, and the pages it linked to instead cover the
same APIs without that scenario. Recovered them verbatim from 966510c74 as
Evaluate a Prompt Change in Code, under How-to: Experiments.

All 7 code blocks and both 4-step sequences are byte-identical to the removed
tab; only the Tab wrapper was dropped and the block de-indented one level. The
page opens by tying itself to the quickstart, so the support agent scenario
carries across the UI and code paths again.

"Prefer to do this in code?" now points here first, with Run Experiments as
the wider reference.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the new experiment page headings its siblings have

The page had no H2s, so it contributed nothing to the table of contents while
every other page in How-to: Experiments has several. Adds a heading over the
tabbed walkthrough, notes that the script creates its own dataset so it runs
without traces, and points at Set a Baseline Experiment, which covers the
persistent baseline that this page's in-code comparison does not.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rework Observe and Evaluate, split PXI into Get Started and Learn More

Observe Your Agent: intro rewritten to say what the page does and where the
concepts live, `\(Recommended\)` on the agent-assisted path, and the manual
steps reordered to start Phoenix, get your keys, then set up tracing. The
install step folded into setup, and the three steps that were identical in
both language tabs hoisted above them.

Evaluate Your Agent: no longer depends on having traces. The dataset step
hands you three travel support examples to upload, so the page runs on a
fresh Phoenix. Traces became an optional route rather than the only one.
Switched to an LLM judge, added copyable Playground prompts for the baseline
and the edit, and dropped the save-a-prompt-from-a-trace step.

PXI: 3,315 words split into a 315-word Get Started \(what it is, what it does,
three steps\) and a 1,158-word Learn More grouped as capabilities, surfaces,
configuration, and privacy. Nothing deleted except the architecture diagram
and the flag and slash-command tables, which duplicate the CLI reference.
Accordions flattened so nothing is hidden behind a click.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: consolidate Arize Phoenix for Agents

Snapshot of the agents/MCP consolidation: agent-assisted-setup folded into a
new developer-tools/cli page, and remote-mcp, docs-mcp, and phoenix-mcp-server
collapsed into one integrations/mcp page, with redirects for all four retired
paths. Nav and inbound references updated across the CLI reference, release
notes, self-hosting, and the shared onboarding snippets.

Committed from the docs-get-started-reorg worktree so the two in-flight
branches can be merged.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: point the tracing quickstart at the new CLI page

The agents consolidation folded agent-assisted-setup into
developer-tools/cli. This was the last reference still relying on the
redirect rather than the real page.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: link the uploaded dataset, and cross-link the other Observe methods

The travel-support.csv download pointed at assets/datasets/, but the file was
uploaded to assets/images/arize-docs-images/evaluate/. Repointed to where it
actually lives, so the Evaluate walkthrough runs end to end. Verified the URL
serves 3 rows with the question and expected_answer columns the page maps.

Also adds links out of both quickstarts for the routes they do not teach,
prompted by the Phoenix workflow diagram: reading traces in code, from a
coding agent via the CLI or MCP, by asking PXI, and across Sessions; plus PXI
for curating datasets and running experiments. Links only, no new pages. The
Evaluate step stays absent on purpose, since online evals have not shipped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make both Evaluate paths use one dataset, and close three story gaps

Found by running every path in Get Started end to end as a new user.

The agent-assisted prompt inlined three examples, but only one of them was in
the CSV the manual path downloads. Cutting that file from eight rows to three
had left the prompt behind. The prompt now downloads the same CSV, so there is
one dataset and the two paths cannot drift again. It also restores the page's
own argument, which turns on the visa and refund rows.

Three gaps between the paths:

- The Recommended path ended when px setup finished and never said to open the
  Traces view. Only the manual path told you to look at what you captured.
- "Ask your coding agent" said to start Phoenix without giving the command,
  which only appeared in the other tab.
- Observe handed off promising to "convert selected traces into a dataset",
  which Evaluate no longer does, since it starts from a downloadable dataset.

Verified by execution, not reading: both language paths install and run and
produce correct OpenInference traces \(a support_reply CHAIN wrapping a
ChatCompletion LLM span with messages, model, and token counts\), px setup
connects and writes .env.phoenix, both skills commands install, and the CSV
builds the same 3-example dataset from either path.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: state the Python 3.10 floor and name the Traces tab in Observe

Both quickstart paths were run end to end against Phoenix 20.16.0. On
macOS's default Python 3.9, pip quietly installs arize-phoenix-otel
0.14.0 instead of 0.17.1, because the current packages require 3.10+.
A project also opens on the Spans tab, so "the Traces view" sent
readers looking for a view that isn't the landing page.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>

* docs: make the Evaluate comparison actually move the score

The page claimed the baseline prompt invents visa rules and agrees to refunds
it should decline. Measured against gpt-5-mini it does neither: the baseline
scored 1.0, the edited prompt scored 1.0, and stripping the baseline to
"Answer the customer's question" still scored 1.0. Step 4 promised a rise that
never came, so the lesson did not land.

Fixed by changing what the examples test. They now ask about a fictional
airline's Basic fare policy, with values deliberately unlike industry norms: a
48 hour refund window, a 40 dollar change fee, one included checked bag. The
model cannot guess any of them, so the baseline answers from industry defaults
and gets all three wrong. The edit supplies the policy.

Measured: baseline 0.00, edited 1.00.

This also makes the lesson durable. The baseline fails for lack of information
rather than lack of capability, so a better model will not quietly erase the
gap the way it did with the previous examples.

Also drops the Observe handoff sentence, per review.

The dataset CSV needs re-uploading to the same path; the agent-assisted prompt
reads it by URL and needs no edit.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the PXI paths, found by running them

Tested both PXI pages against a live Phoenix for the first time.

The terminal client command did not work. `npx -y @arizeai/phoenix-cli pxi`
exits with "unknown command 'pxi'", because pxi is a separate bin in the
package rather than a px subcommand, so npx ran the default bin and passed
pxi as an argument. Needs `-p`. Verified the corrected form launches and
reaches the credential check.

PXI defaults to an Anthropic model, so a reader who follows step 1 and sets
only OPENAI_API_KEY opens the assistant and hits "Anthropic credentials are
not configured". The step now says to pick a model matching the provider you
configured. Confirmed by switching to gpt-5.5 and getting a correct answer
about project span counts.

Restores "Where your data goes" under Privacy and tracing. It was dropped with
the architecture section, but it is not architecture: it is the answer to
whether data leaves your instance. No separate Arize service, your provider
called with your key, Arize not in the request path, and remote export gated
three ways. That is the first thing a security review asks about a beta AI
feature.

Also: the provider list mixed three env var names in code with "AWS Bedrock"
in prose, so it now names providers throughout; "Drives the product" lost its
object when I trimmed it; and the roadmap milestone link is out, since a
GitHub milestone goes stale.

Verified live: the consent gate appears as described, trace saving shows
"Disabled by system settings" which matches the documented default, the model
list matches the validated set, and PXI answered using search, get_schema, and
execute, the code-mode surface the page describes.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the CLI page its own job, running Phoenix from the terminal

The page claimed px was "the recommended way for a coding agent to work with
Phoenix", then documented only px setup, which Observe already owns. So it
repeated onboarding and never showed an agent working with Phoenix. Meanwhile
px has 17 top-level commands and the page covered one.

Rewritten around the debugging loop the phoenix-cli skill actually drives:
find what broke, read one trace in full, record the judgement as an
annotation, inspect the datasets and experiments you test against, then work
on the prompt. Plus a table for the remaining commands. Setup is now a single
line pointing at Observe.

Every command was run against a live Phoenix, which caught three errors in my
own first draft:

- `px span list` has no `--filter`. It takes typed flags, so the examples use
  `--status-code ERROR` and `--span-kind LLM`.
- `px experiment list` requires `--dataset`.
- `px trace get` resolves the default project and reports "Trace not found"
  without `--project`.

Verified working: trace list, trace get, trace add-note, span list with both
filters, span annotate \(annotation upserted\), dataset list, dataset get,
experiment list, prompt list, project list, and all 11 commands in the table.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: lead the agents overview with the setup table

Move "Set up your agent" above the three-piece cards so the
per-agent map comes first, and make its lead-in name the three
pieces so it stands on its own.

Narrow the endpoint rule: the plugins and the px CLI take the base
endpoint and append /mcp themselves, while `claude mcp add` and the
per-client MCP steps take the full /mcp URL.

Reword the prompt samples as examples rather than the full set.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the agents overview one job, shorten the MCP page

The overview page was mostly navigation: three cards pointing at CLI,
MCP Servers, and Skills, which already sit next to it in the sidebar,
plus a setup table routing to the same three pages. The sidebar label
"Overview" also repeated the group name, so the page read as a section
divider.

Now it does one thing, connect a coding agent to Phoenix: what this is,
the disambiguation note, the per-agent setup table, the endpoint. The
three cards are gone, the prompt samples are cut from five to two, and
the node_modules section is dropped since the TypeScript SDK overview
already documents it. Relabel the sidebar entry "Set Up Your Agent".

Correct the Claude Code row: the plugin ships three skills, while
px setup skills installs five, so the plugin path needs px setup skills
too. Verified by listing the plugin and running both.

On the MCP page, lead Connect with px setup mcp, merge the Cursor,
VS Code, and OpenCode accordions \(their JSON matches what px writes\),
replace the six Docs MCP accordions with a client table, and merge the
identical npm stdio configs. 633 to 521 lines with no facts dropped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: move the plugins onto the setup page

The setup page was still an index: its table pointed at Install
sections that CLI, MCP Servers, and Skills each already have. What it
lacked was the thing that belongs to no single one of them, the
one-install-per-agent path.

That path was the Plugins section, 137 lines and the largest section on
the MCP page, which is the wrong home for a plugin that registers MCP
and skills together. Move it here, after the endpoint it references,
and leave a pointer on the MCP page.

MCP drops to 388 lines and covers only MCP servers. The setup page now
routes by agent, states the shared endpoint, then gives the full plugin
steps.

Also repoint skills.mdx at the plugins section's new home, and fix its
link to a CLI heading that the CLI rewrite removed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the MCP page opener

It said "two things you might want your assistant to do" and "run
both" above a three-row table. Name the two servers that matter, your
data and the documentation, and say what the third row is.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: cut duplicated prose from the MCP page

The Configuration table repeated two env vars that self-hosting
configuration already documents; point there and keep only
PHOENIX_SKILLS_PATHS, which is unique to this page.

Monty sandbox limits were spelled out twice, in How it works and again
in Security; state them once. Merge the code-mode Warning and Note,
which both said the same thing. Trim the OAuth paragraph. Move the
OAuth2 authorization-server paragraph out of "Mounting your own
skills", where it was misfiled, into Security. Cut the npm sample
questions from seven to three.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: clarify the direction of the agents note

"Operate on Phoenix" is jargon, and "trace your sessions with a coding
agent" reads either way. Say which direction each page covers: this one
connects the agent to Phoenix, the other has Phoenix record the agent.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: correct two plugin commands that fail as written

Both found by running them. claude plugin uninstall acts on user scope
by default, so it stops with a scope error on a plugin installed at
local or project scope; say to pass the scope, and add disable, which
has the same rule. claude plugin details reports a disabled plugin as
not found, so say it has to be enabled, and say what the command lists.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: correct two CLI table rows that overstated what the commands do

Found by running them. px api has one subcommand, graphql, and rejects
mutations, so "authenticated requests to any Phoenix API route" is
wrong. The three annotation commands take only delete, and require
--all or a time window, so "stored annotations" hid that they are
destructive.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make the CLI debugging loop match what px can do

Steps 4 and 5 were titled "Turn failures into a test set" and "Work on
the prompt", but every command under them only reads, and the bodies
said "Inspect", contradicting the titles. px has no way to create a
dataset, add examples, or edit a prompt: for datasets, prompts, and
experiments it offers list, get, and delete only.

Cut the loop to the three steps px actually supports, ending at the
annotation, which with notes is the only thing it writes. Move the
dataset, experiment, and prompt reads into their own section framed as
looking up context, and say plainly that authoring happens in the UI
or the SDK.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rewrite the CLI page as the workflow an agent runs

The page had a "debugging loop" that was not a loop: it ended at an
annotation, and px cannot fix anything. Reorganising it by capability
then made it a feature list. Put it back in order as the pass an agent
actually runs, and let step 4 say plainly that px stops there and the
fix goes in your code, which is what closes the loop.

The examples were the weaker half. They repeated a fake project four
times, used placeholder ids that cannot be run, and carried comments
that restated the command instead of the output. Use real ids, say
which command feeds the next, and split the dataset, experiment, and
prompt reads into their own section, since those are for checking a
change rather than part of the debugging pass.

Add three facts found by running the commands: reads accept --format
json, annotate and add-note want the OpenTelemetry id rather than the
opaque one, and --identifier makes a repeat call overwrite.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: note that px annotation deletes are gated off by default

px span-annotations delete answers "Delete commands are disabled."
before it looks at any flag. Name the variable that opens the gate,
PHOENIX_CLI_DANGEROUSLY_ENABLE_DELETES, so the row does not send a
reader at a command that refuses to run.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say who runs px, and that the docs MCP URL is not a page

The CLI page opened by saying px is how a coding agent works with
Phoenix, then showed a reader commands to type, and titled the section
"How an agent uses it". A reader could not tell whether the commands
were theirs to run. Say it is an ordinary CLI you run yourself, and
that needing only a shell is what makes it easy to hand to an agent.

The Phoenix Docs MCP URL sat in a bare code block, so it read as a link
to open. A browser shows the tool manifest as JSON, which looks like an
error and is not. Say it is an endpoint for a client.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: answer px versus MCP, and add agent instructions to both pages

Neither page said when to reach for which. That choice only existed as
cards on the setup page, which is not where someone deciding ends up.
The CLI page now compares the two on what the client must support, what
setup costs, and how much surface each exposes, and the MCP page points
at it.

Both pages end with a section addressed to an agent reading them,
covering how to discover flags, which output format to parse, the
OpenTelemetry id the annotate commands want, the execute sandbox limits
and import allowlist, and that trace contents are untrusted data.

Every claim in both sections was run first: 116 catalog operations, the
50-call cap, json/re/datetime importable and socket/subprocess/importlib
not, exit status 0 and 1, and the delete gate.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rebuild the CLI page to a fixed outline, and align sibling names

The page now follows one order: what it is and why it matters, CLI or
MCP server, set up, what you can do with it, learn more, agent
instructions. Setup is three steps ending in a check. The examples
keep their real ids and drop the repeated --project in favour of
PHOENIX_PROJECT, set once in step two. The command table is gone; the
two caveats it carried, the delete gate and read-only GraphQL, live in
the agent instructions, and the full list is a link to the reference.

An audit of how each page in the section names its siblings found six
mismatches: the CLI page linked as "px CLI" and as bare "MCP", the
Remote MCP Server as "server", and a PXI section by a title it does
not have. Each link text now matches the page or section it points at.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: name the CLI setup check by what it proves

"Check it" said nothing. The step confirms the CLI reached the
instance, so say that, and say what success looks like.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say what the CLI setup is for before the steps

The section went straight from the heading to step one, so a reader
could not tell what was being set up or where. Name the CLI in the
heading, say to install it where the agent runs, and say what having
finished looks like.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say what px project list returns on a fresh instance, drop "has a terminal"

The setup check said a table of your projects means success, which
leaves a reader with no traces unsure what to expect. Tested on an
empty instance: every Phoenix has a project named default, so the
command always returns at least one row, and a wrong endpoint fails
with "Error fetching projects: fetch failed". Say both.

"When your agent has a terminal" was jargon on the CLI and MCP pages.
Say what it means: the agent can run shell commands, which every
coding agent can.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the CLI-first recommendation, say what the default project is

"Start with the CLI" was a preference with nothing behind it, and it
contradicted the setup page, which sends Claude Code and Codex users
to the plugin first, and the MCP page, which calls itself the primary
choice. The comparison table carries the facts; let the reader choose.
The MCP page mirror is now a plain pointer.

The setup check mentions the default project. Say what it is: created
by the initial migration, the destination for traces sent without a
project name, and empty on a fresh instance. Not a demo.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: make the CLI page work for a user with no traces yet

Run as a brand-new user, in a clean environment against an empty
instance, the previous page failed at step two: PHOENIX_PROJECT=my-app
404s on every read, and every example carried an id from one machine.
Stuff has to work before a project has traces.

Follow the shape of the live CLI section: install, endpoint, verify
with px --help and px project list, which both succeed with zero data,
then the common agent workflows as bullets and one example prompt, so
the reader sees what to ask rather than what the agent types. Say that
PHOENIX_PROJECT comes later, once traces are flowing. Keep the CLI or
MCP table and the agent instructions, and add the missing rule that
read commands need a project.

On the MCP page, restore the per-client tabs for the Docs MCP from the
live site, adding Claude Desktop.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: one story across Arize Phoenix for Agents, working from zero traces

The section was organised by plumbing, CLI, MCP, Skills, and every page
assumed traces already existed. A new user has none. Reframe around
what is true from the start: connect the agent, let it instrument your
app, then debug from what Phoenix records.

Set Up Your Agent owns routing, the endpoint, and the plugins. Its
connection check is a question to the agent, list my Phoenix projects,
which a fresh instance answers with default. Verified through both
pipes on an empty instance: px project list, and MCP search finding
getProjects and execute returning it. A new section points at the
agent-assisted tracing path as the next step.

CLI, MCP Servers, and Skills share one skeleton: what it is, install
ending in a check, what your agent can do with it, learn more,
reference. Deep material on the MCP page, API-key fallback, code mode
internals, server configuration, security, the npm server, sits under
Reference. The MCP Endpoint section folds into one line. The skills
page moves its options table and custom-skills guidance under
Reference and gains an example prompt.

Every shell command on the four pages was run in a clean environment
against an empty Phoenix. Restore the remote-mcp-server anchor, which
eleven pages link to, and repoint two onboarding snippets that still
named a CLI heading removed in an earlier rewrite.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut the instrumentation pointer back to one line

It was a sentence before, and a heading with three sentences after. The
link is the useful part.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: nest the MCP capabilities section under Remote MCP Server

The page covers two servers, so "what your agent can do with it" as a
top-level heading left "it" ambiguous. Make it an H3 under Remote MCP
Server, alongside a new Connect H3 for the setup steps.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut duplicated prose from the MCP page

The intro paragraph restated the table under it. The capabilities
section under Remote MCP Server repeated Reference, How it works,
nearly sentence for sentence; point there instead. Three sentences of
OAuth mechanism sat ahead of the first command; one line after the
accordions covers what a reader needs. The Docs MCP intro restated its
table row, the npm section said maintenance mode twice, and the
mounted-skills paragraph named its variable twice in a row.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: link the CLI page back to Set Up Your Agent

The MCP and Skills pages both point back to the setup page; the CLI
page did not, so a reader who landed there directly had no route to
the per-agent install.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: frame PXI skills as example capabilities

The table led with skill names, which is the mechanism. Lead with what
you can ask PXI to do, give one example ask per row, and keep the skill
concept as a single sentence after the table.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: frame PXI models as bring your own, and trim

Lead with the point: PXI runs on your provider and your key. Name the
providers once, keep the validated-model list and the tool-calling
caveat, drop the rest. Repoint the two links that used the old anchor.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the agent-instructions section from the CLI page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the agent-instructions section from the MCP page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the validated-model list from PXI

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: stop promising a supported-models list PXI no longer has

The list was removed; three references still pointed at it. Retitle the
Get Started link to the section that exists, and say model providers
where the Learn more line and the page description said supported
models.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: remove the npm MCP server from the MCP page

Drop the maintenance-mode section, its row in the intro table, and the
three places that offered it as a fallback: the version-floor line, the
Claude Desktop accordion, and the troubleshooting entry.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: act on the docs review: skills guidance, Docs MCP callout, MCP cut-down

Skills page: name the five skills written for Phoenix users, add the
plugin-versus-skills-add guidance from the review \(plugins track
Phoenix releases; skills add is a one-time copy, rerun it or run npx
skills update\), and take ownership of mounting your own skills from the
MCP page. Document --all honestly: tested in a fresh project it
installed 26 skills to 73 agents, including internal development
skills, so it is the wrong default.

MCP page: Phoenix Docs MCP becomes a short block near the top, as the
review asked, in place of an 87-line section. Fold Antigravity into the
other-clients accordion, trim How it works to the tool table plus two
sentences, and drop the mounting section now that Skills owns it.
361 to 271 lines.

Evaluate Your Agent: move the spans screenshot out of the Note it was
nested in.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: bring llms.txt in line with the Get Started and agents reorg

Add the CLI, Skills, and PXI Learn More pages, which had no entries.
Remove seven Quick Start entries for pages the reorg deleted or merged
into Observe Your Agent and Evaluate Your Agent, and three MCP entries
for pages folded into the single MCP page. Retitle the survivors to
match their pages.

Add the agent-usage guidance for the px CLI and the Remote MCP Server
here, where the review said it belongs, instead of on the human pages.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: give the CLI guide entry in llms.txt a unique title

It collided with the @arizeai/phoenix-cli reference entry.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten the endpoint rule on the setup page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: halve the agent-assisted setup on Observe Your Agent

Start Phoenix once above the tabs instead of in each. One sentence of
outcome per tab. Cut the pasted prompt to the two instructions that
matter, since the tracing skill carries the procedure. Drop the agent
list and the CLI pointer, which the setup page owns.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: add the PXI path to Evaluate Your Agent

A prompt to hand PXI once the dataset exists and the Playground has it
selected. Both preconditions come from running it: PXI cannot fetch a
URL, and its playground and evaluator skills act on the page you are on.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: restore the agent-assisted shape on Observe, trimmed

Back to the two tabs as they were, each self-contained, with the full
prompt. Merge the px setup outcome into one paragraph. Replace the
named --skill list with px setup skills, which installs exactly the
five Phoenix skills for every detected agent: tested via npx in a fresh
project with no CLI installed.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: repoint a PXI conversations link to the Learn More page

The Conversations section moved there when PXI split into Get Started
and Learn More; one link still pointed at the old page.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: Evaluate Your Agent builds a reference-aware evaluator

Built-in LLM templates have no reference field and score the baseline
higher than the fixed prompt, so step 2 now walks through Create new
LLM evaluator with a short rubric and the reference path mapping. PXI
section hands off after the dataset and evaluator exist, since it
cannot fetch the CSV and its own evaluator scores 0 at run time.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten Evaluate Your Agent evaluator step and PXI hand-off

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: PXI runs the full Evaluate flow from one prompt

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten Evaluate Your Agent prose

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut PXI Learn More to the essentials

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: one-line rows in the PXI capabilities table

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: shorten PXI privacy and tracing

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: compact PXI configuration table

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: shorten CLI page intro

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten MCP Servers prose

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop em dashes from Get Started snippets

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: add the follow-up prompt to the coding agent path

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: make px setup skills the primary install on the Skills page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say where the model API key goes before creating an LLM evaluator

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: the key must be on the Server tab for evaluators to use it

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say when to pick the CLI and when the MCP server

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: CLI vs MCP as best-when guidance

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop redundant line from CLI or MCP section

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: plugin rows say what the plugin covers and what is optional

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop Docs MCP aside from Set Up Your Agent

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

*…")[#16288](https://github.com/Arize-ai/phoenix/pull/16288)[)](https://github.com/Arize-ai/phoenix/commit/2afdf488e638f282f742c7daf796a09a56662971 "docs: reorganize Get Started around the agent workflow \(#16288\)

* docs: reorganize Get Started around the agent workflow

Restructure the Get Started section to mirror the Arize AX quickstart:
Instrument your agent, Evaluate your agent, Improve your agent, Coding
agent setup, PXI, and Phoenix Demo.

The Python and TypeScript quickstarts previously sat in separate nav
groups, so each language duplicated the whole section tree. Each pair is
now a single page with Python and TypeScript tabs, so the language is a
choice within a section rather than a parallel branch of the nav.

Every page body is carried over verbatim into its language tab. The only
text rewritten is internal links that pointed at the retired
ts-get-started-* paths, which now have redirects to the merged pages.

- Instrument your agent  <- get-started-tracing + ts- counterpart
- Evaluate your agent    <- get-started-evaluations + ts- counterpart
- Improve your agent     <- group over the prompt playground and
                            datasets/experiments guides
- Coding agent setup     <- group over Agent-Assisted Setup and Coding
                            Agents
- MCP group, PXI, and Phoenix Demo are unchanged aside from ordering
- End to End Features Notebook is removed from the nav; the page itself
  is left in place

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: cut duplicated prose from the Get Started guides

The previous commit put each language's full guide in its own tab, which
made every language-agnostic sentence render twice on the same page. On
the prompt playground guide that was 37% of the prose; on datasets and
experiments, 40%.

Where the two tabs told the same story, the narrative is hoisted above
the tabs and the tabs now wrap only what genuinely differs: code, videos,
the companion-code callout, and the two prompt snippets. The prompt
playground and experiments guides had identical step sequences, so their
steps are shared outright. Tracing and evaluations each have an extra
TypeScript step, so their steps stay tabbed and only the intro, prereqs
and closing sections are shared.

No prose was rewritten. Every line either survives verbatim from the
previous commit or is a prefix of one with a redundant trailing clause
removed, verified mechanically. Where the two tabs differed only
trivially, one existing sentence is kept verbatim rather than a new one
written; this also picks up several corrections the TypeScript copy
already had:

- "traces that received an incompleteness evaluation label" ->
  "an incomplete evaluation label", matching the filter beside it
- "Filter your traces by the incorrect evaluation label" -> "incomplete";
  the filter under it has always been evals\['completeness'\] == 'incomplete'
- "Using the explanations apart of the evals" -> "from the evals"
- "New application runs as a results of our task" -> "as a result"
- "ran your first experiment" / "ran an A/B test" -> "run"

Also drops, on the tracing guide, a sentence restating the bullet list
above it and three repeats of the supported-frameworks disclaimer that
the intro already carries.

Verified: all 44 code blocks byte-identical \(multiset\), every step title
and video retained, no duplicate sentence in any rendered page, both link
checkers clean, all four pages render with working tabs.

Words: 10790 -> 8789 \(18.5% shorter\).

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: tighten Instrument and Agent-Assisted Setup, repoint quickstart

Three changes to the Get Started section.

Instrument your agent opened with a paragraph of series framing --
"This guide walks through a complete workflow ... Each guide in this
series introduces one piece of that workflow" -- which is word for word
the Overview page's opening and says nothing about instrumentation. It is
removed here and still lives on the Overview page. The guide now opens on
its own subject: what a trace is.

/docs/phoenix/quickstart redirected to the Overview page, which is not in
the nav. It now lands on Instrument your agent, the actual first guide in
the series. The Overview page is unchanged and still reachable from the
four pages that link to it.

Agent-Assisted Setup is only ~350 words of prose but 89 lines, because
almost two thirds of it is follow-up reference rather than setup. The
three advanced sections -- re-running a single step, non-interactive runs,
and unsupported agents -- move into an AccordionGroup, so the page reads
as an intro, two steps and a verification section. Nothing is deleted:
all five code blocks are byte-identical and the collapsed text is still
in the page. The one prose deletion is "The fastest way to add Phoenix
tracing to your application.", which restated the frontmatter description
rendering directly above it.

Trade-off worth noting in review: collapsed sections leave the "On this
page" table of contents, which now lists only "Confirm traces are
flowing".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: hold every Get Started intro to four sentences or fewer

The intros restated themselves. Evaluate your agent opened with seven
sentences making three points: what an eval is \(said three times\), what
the guide does \(said twice\), and which metrics you can score against.

Deletions only, no rewriting. Every surviving sentence is verbatim.

Evaluate your agent, 7 -> 4. Dropped:
- "Evaluations attach quality signals to runs so that correctness or
  relevance can be reasoned about consistently instead of judged case by
  case." Restates the opening sentence and the traces/evals contrast that
  follows it, and states it as a negative.
- "We'll start with data that already exists in Phoenix, define a simple
  evaluation, and run it so we can see results directly in the UI."
  Restates "In this guide, you'll set up evaluations ... and run them on
  existing trace data".
- "The goal is to move from "I have model outputs" to "I can measure
  quality in a repeatable way."" Restates the opening sentence.

Iterate on Your Prompts, 5 -> 3. Dropped "Up to this point, we've traced
our agent runs and evaluated their outputs. Now we'll focus on prompts by
grouping failures into a dataset and iterating on prompt variants in the
Prompt Playground." -- the sentence above it already says the guide
groups failing runs into a dataset and iterates in the Playground, and
the prereq list directly below states the same continuity as two bullets.

Instrument your agent, TypeScript tab 5 -> 4. Dropped "In this guide,
we'll get tracing set up and walk through how to instrument an
application.", which the sentence after it says with the specifics. The
Python tab already carried this as one sentence.

Counts are per reader, shared intro plus their language tab, excluding
callouts: Instrument 4, Evaluate 4, Iterate 3, Optimize 3,
Agent-Assisted Setup 1.

Code blocks unchanged on all three pages.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say "agent" instead of "app" across Get Started

Refers to the reader's system as an agent throughout, matching the
section titles \("Instrument your agent", "Evaluate your agent"\). 51 lines
across the five guides plus the Overview page. Code blocks and URLs are
untouched; the `js/examples/apps/mastra-quickstart` paths and every
identifier keep their spelling.

The experiments guide's title becomes "Optimize Your Agent with
Experiments", so the nav label changes with it.

Two phrases needed care rather than a plain swap:

- "an agent application" would have become "an agent agent"; it is now
  just "an agent".
- "the agents that make up our application" would have become "the agents
  that make up our agent"; it is now "our agent system", the phrasing the
  experiments guide already uses.

Agent-Assisted Setup keeps "app" for the reader's codebase. That page is
about handing work to a *coding* agent, so "let your coding agent add
tracing to your agent" reads as one thing instrumenting itself. The
collision is worse than the inconsistency.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: merge the prompts and experiments guides into Improve your agent

"Iterate on Your Prompts" and "Optimize Your Agent with Experiments" were
two halves of one workflow split by *how* you make the change rather than
by what you are doing. Both started from eval-labelled failing traces,
both ended in the same Experiments view, and each pointed at the other as
the next step, so a reader could not tell which came first.

They are now one guide, matching the AX quickstart, which has a single
"Improve your agent" page with method tabs rather than a group.

Four steps, with the method choice appearing once instead of at every
stage:

1. Create a dataset from failed traces      \(shared\)
2. Use explanations to identify improvements \(shared; was taught twice\)
3. Make the change and run it   -- tabs: In the Prompt Playground /
   In code, with Python and TypeScript nested inside each
4. Compare versions in the Experiments view  \(shared narrative\)

Nothing is dropped. Verified against both source pages: 13 of 13 code
blocks, 12 of 12 videos, 12 of 12 frame captions, every numbered
instruction, and every H3 section.

Deliberate changes beyond the merge:

- The two cross-links between the guides are gone; they would now point
  at this page. That circularity was the reason to merge.
- "the first from Step 3" is now "the first from your baseline run",
  because the step it referenced no longer has that number.
- "A dataset created from previous runs" leaves the prerequisites, since
  step 1 builds it.
- The two "Congratulations" lines and the two "Learn More" sections are
  one each.
- About eight new connective sentences were written, the only prose on
  this page that is not carried over verbatim. Interleaving two
  workflows needs them.

Seven URLs now redirect here: both retired page paths, both ts- paths,
and the three legacy quickstart paths.

docs/phoenix.mdx had a card for each guide in both its Python and
TypeScript groups; each pair is now one "Improve Your Agent" card.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rewrite Get Started in second person, direct voice

The guides narrated themselves in the first person plural -- "we'll
install", "now that we have trace data, we can", "at this point we've
defined" -- which reads as tutorial filler and contradicts the house rule
that guides use active voice and second person. 155 instances of that
register across the section, now 0:

  royal "our"                   55 -> 0
  royal "we'll" / "we're"       44 -> 0
  "we can"                      26 -> 0
  "At this point,"               7 -> 0
  "Next, we..."                  6 -> 0
  "In this step, we..."          6 -> 0
  "Now that ..., we"             4 -> 0
  "let's"                        5 -> 0
  "More importantly,"            2 -> 0

Instructions are now imperative and the prose states facts about the
system rather than narrating a shared journey. "Next, we'll register a
tracer provider linked to a project in Phoenix. This project is where
your traces will show up" becomes "Register a tracer provider linked to a
project in Phoenix. That project is where your traces show up."

Two errors fixed on the way through:

- "seen how an agent runs shows up from start to finish" -> "how an agent
  run shows up"
- "our finanical writer agent" -> "The financial writer agent"

No instruction was dropped. Code blocks, videos, link destinations, step
titles and H3 sections are identical on all three guides: 17/2/13/9/14 on
tracing, 14/2/9/9/5 on evaluations, 13/12/5/4/9 on Improve your agent.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: sharpen the tracing opener and drop every em dash

The tracing guide opened with a sentence that did not parse: "A trace is
a record of a single run of your agent, broken down into spans that show
what happened at each step \(how agents, tasks, and tools executed\) and
provides the raw data needed for everything that follows." After the
participial phrase, "and provides" cannot reach back to "is a record", so
it reads as attaching to "spans", which would need "provide". It also
said the same thing twice \("what happened at each step" and "how agents,
tasks, and tools executed"\), and closed on "the raw data needed for
everything that follows", which was left over from the series framing
that now lives only on the Overview page.

Replaced with the house formulation from ax/instrument/what-are-traces
and phoenix/tracing/concepts-tracing, which also defines a span instead
of gesturing at it:

  A trace captures one end-to-end run of your agent as a tree of spans.
  Each span is a single step \(an agent, a task, a tool call, or an LLM
  call\) with its input, output, and timing.

The page also picks up a frontmatter description, matching the AX tracing
guide. It renders as the subtitle under the H1, so it carries the framing
while the body carries the mechanic, and the two do not restate each
other.

Em and en dashes are now zero across the section. Two were mine; two were
pre-existing on Agent-Assisted Setup, where a dash joined two independent
clauses that are clearer as separate sentences.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: stop repeating the prerequisites, and drop the long reference from Get Started

The evaluations guide stated its prerequisites twice, about fifteen lines
apart, in different words. At the top: "you'll need to have completed Get
Started with Tracing so you should have: Financial Analysis and Research
Chatbot / Trace Data in Phoenix". Then again inside step 1: "Evaluations
in Phoenix run over existing trace data. If you followed the tracing
guide, you should already have: A project in Phoenix / Traces containing
LLM inputs and outputs". Both tabs carried it, so a reader met the same
list twice per pass.

The step's version was the more precise one, so its detail is folded into
the single list at the top, which now reads "A project in Phoenix holding
traces with LLM inputs and outputs". Step 1 keeps the one fact it added,
that Phoenix evaluations run over existing trace data, and loses the
duplicate list.

Worth noting for future passes: string-similarity scanning did not catch
this, because the two lists share almost no wording. Semantic repetition
needs a read, not a regex.

The Overview page repeated itself inside single paragraphs the same way:
"using the same inputs and evaluation criteria" in two consecutive
sentences, and "spans that show what happened at each step" followed by
"Traces show how agents, tasks, and tools executed during a run". Both
are now stated once.

Coding Agents leaves the Get Started nav. It is 466 lines, 2258 words and
27 code blocks of reference material for Claude Code, Codex, the CLI, MCP
and skills, and its own opening line says "If you haven't instrumented
your app yet, start with Agent-Assisted Setup" -- it is the step after
getting started, not part of it. The page is unchanged and still listed
under Integrations > Developer Tools, and Agent-Assisted Setup already
links to it for ongoing workflows.

That leaves one page in the group, so it collapses to a single nav entry.
Agent-Assisted Setup gains sidebarTitle "Coding agent setup" so the label
still matches the agreed structure, the same way pxi.mdx shows as "PXI
Agent".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix five accuracy defects found by checking Get Started against the source

Verified the guides against the Phoenix source in this repo rather than
against other docs. Five real defects, all inherited from main:

1. `evals\['completeness'\].label == 'incomplete'` is a **span** filter. The
   guide said "Filter your traces by the incomplete evaluation label",
   which points at the traces table, where the expression is rejected:
   src/phoenix/trace/dsl/trace_filter.py refuses it with "`evals\[...\]` is
   not available in the trace filter". docs/.../filter-expressions.mdx
   lists it under span filters, and the UI has separate SpanFiltersContext
   and TraceFiltersContext. Now reads "Filter the spans table".

2. and 3. The add-to-dataset flow is span-based, not trace-based:
   AddSpansToDatasetInput carries `span_ids`
   \(src/phoenix/server/api/mutations/dataset_mutations.py:225\), and the
   dataset guide says "select the span". "Select the traces you want to
   include" and "Add the selected traces" now say spans.

4. `client = Client\(\)` ran with no import on this page. The evaluations
   guide imports it, but this page has to stand alone, so it now carries
   `from phoenix.client import Client`.

5. `import { getDataset } from "@arizeai/phoenix-client/datasets"` was
   never used. The companion example the guide points at,
   js/examples/apps/mastra-quickstart/src/mastra/experiments/experiment.ts,
   imports the identical set without it. Removed.

Everything else checked out against source: register\(project_name,
auto_instrument\), LLM\(provider, model\), ClassificationEvaluator\(name, llm,
prompt_template, choices\) and .bind\(\), Client\(\).datasets.get_dataset\(dataset=\),
Client\(\).experiments.run_experiment\(dataset=, task=, evaluators=\),
suppress_tracing's import path, the phoenix-client and phoenix-evals TS
exports, runExperiment accepting a {datasetName} selector, ports 6006 and
4317, exit code 6 as NOT_VERIFIED, every `px setup` subcommand and all
nine documented flags, and both companion code paths.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: one language choice on Improve your agent, not ten

The page had accumulated 10 separate Python | TypeScript pickers down its
length: one on the companion-code callout, one per video pair, and one
inside each half of the method tabs. Mintlify syncs tabs that share
labels, so picking a language once did switch all of them, but rendering
the control ten times reads as ten decisions.

Language is now a single choice at the top, the same shape the tracing
and evaluations guides use. Method tabs \(In the Prompt Playground /
In code\) stay where they were, inside step 3 and step 4. A reader now
sees one language picker and two method pickers instead of twelve
controls.

This is purely structural: every Python | TypeScript group was resolved
to the chosen language and the whole body wrapped in one pair. No prose
was rewritten. Verified against the previous version: all 13 code blocks,
12 videos, 12 frame captions, every numbered instruction, every heading
and every step title still present.

The cost is 78 lines. Language as the outer axis means the
language-agnostic narrative -- the dataset steps, the explanation step,
the shared prose in steps 3 and 4 -- exists once per branch. That is why
the step count in the file doubles from 4 to 8 while a reader still sees
4. Worth it for one obvious choice at the top; the alternative was
keeping the page shorter and the navigation confusing.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: method first, language inside it, on Improve your agent

Corrects the tab hierarchy to match ax/get-started/get-started-tracing,
where the outer tabs are methods \(By npx evals / By Arize Skills / By
Code\) and the Python | TypeScript picker sits inside By Code. The previous
commit put language on the outside, which was the wrong axis.

A reader now picks the method first \(In the Prompt Playground / In code\),
then the language within it. Two controls, in that order.

Also tightened the prose, since with tabs every shared line exists in more
than one branch and each cut lands more than once. Deleted as restatements
of the line beside them:

- "This dataset now represents a concrete failure case for your agent."
- "Start by grouping together the traces that didn't perform well."
- "Start from a real prompt the agent actually used." -- the next sentence
  already gives "a real starting point".
- "Saving prompt variants makes it easy to track changes..."
- "An experiment reruns the same inputs through different versions..." --
  the page intro already defines an experiment; this was the third time.
- "With two versions in hand, compare them..." -- restated the step title.

Shortened: the two-paths lead-in \(the tab labels say it\), the
create-datasets aside \(said "in code" twice\), and the task and evaluation
definitions. Fixed two royal "we" the voice pass missed: "the dataset we
made earlier" and "we should see more traces".

Line count: 605 -> 581 from the trims, then -> 694 from the inversion.
The growth is real and worth naming. Steps 1 and 2 -- create the dataset,
read the explanations -- are method-agnostic, so method-outer copies them
into both method branches, and language-inner copies them again. Four
copies of about thirty lines. A reader still sees each step once.

The wrinkle that buys: the method choice now sits above two steps that do
not depend on it, so a reader picks Playground or code before creating the
dataset. The alternative keeps steps 1 and 2 shared and moves the method
choice down to step 3, at the cost of a second visible language picker.

Verified against the previous version: every code block, video, caption,
link, heading and step title still present.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: move the companion-code callout out of the no-code path

The Prompt Playground branch offered "a companion notebook with runnable
code examples" in a path where you write no code. That was a side effect
of wrapping the whole body in the method tabs. The callout now appears
only under In code, for both languages.

The Python | TypeScript picker stays inside the Prompt Playground branch,
because that path does depend on which language you followed -- not for
the language itself, but for what you built with it. Six of the videos
record the CrewAI app rather than the Mastra app, and the dataset input
syntax differs with the agent: `{tickers}` and `{focus}` are CrewAI task
variables, `{{input}}` is Mastra. The label is a proxy for "which
quickstart did you follow".

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: drop the horizontal rules bracketing the companion-code callouts

Every companion-code callout in Get Started was wrapped in `---` rules,
inherited from main. Each renders as a thematic break with generous
margins, so the callout sat in a band of empty space with a thin line
above and below it. The callout already has its own bordered container,
so the rules added separation that was not needed.

12 rules removed: four on the tracing guide, four on evaluations, four on
Improve your agent. Frontmatter delimiters are untouched, including the
multi-line description block on the tracing guide, whose closing `---`
sits at line 6 and is easy to mistake for one of these.

No prose changed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the playground template syntax, which would not have interpolated

The Python path told readers to set the User prompt to `{tickers}` and
`{focus}`. That is F_STRING syntax, and the Prompt Playground defaults to
Mustache:

  js/app/src/components/templateEditor/constants.ts
    FString:  "F_STRING",  // {variable}
    Mustache: "MUSTACHE",  // {{variable}}

  js/app/src/store/playground/playgroundStore.tsx:273
    templateFormat: TemplateFormats.Mustache,

Playground.tsx reads the format from that store, so with default settings
single braces are not substituted -- the prompt would have carried the
literal text `{tickers}`. The guide never mentioned the format selector,
so there was no way for a reader to know.

Most likely this was copied from the CrewAI task template on the tracing
guide, `Task\(description="Research: {tickers}"\)`, which legitimately uses
single braces. That is CrewAI's own templating, not Phoenix's, and it is
left alone.

Both paths now use Mustache, and the shared lead-in says so, so the two
language tabs differ only in the variable names.

The variable names themselves were already correct and are framework
driven, traceable in the guides' own code: the Python experiment passes
`example.input` straight into `kickoff\(inputs=...\)`, so dataset examples
carry `tickers` and `focus` keys, while the TypeScript task types
`example.input` as a `{role, content}\[\]` message array under a single
`input` key.

Verified rendered: the block shows `Research: {{tickers}}` literally, and
the format note appears in both language tabs.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: expand the Agent-Assisted Setup accordions back into headings

I collapsed three sections into an AccordionGroup when the page read as
too long. That hid 307 of its 542 words, 57% of the page, and dropped the
three section names from the "On this page" table of contents, leaving one
entry. It also made the page look like content had been deleted, which is
what prompted the question.

The reason for collapsing them has gone: the length concern was mostly the
466-line Coding Agents reference, which no longer sits in this nav group.
What remains is ~330 words, the shortest page in Get Started, so hiding
more than half of it costs more than it saves.

Re-run a single step, Run non-interactively \(CI or agents\), and Use an
unsupported agent are `##` headings again, so all four sections appear in
the table of contents and can be linked to directly.

Code blocks and links are identical.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: no language picker in the Prompt Playground path

You write no code in the Prompt Playground, so asking the reader to pick
Python or TypeScript there was misleading. The picker now appears only
under In code, where it selects actual code, which matches
ax/get-started/get-started-tracing.

Two controls on the page, in this order:

  In the Prompt Playground | In code
    In code -> Python | TypeScript

What the picker had been carrying in that path, and where it went:

- Steps 1, 2 and 4 were already identical between the languages.
- The dataset input keys are real, so they are stated inline: which keys
  you have depends on the agent you traced, `{{tickers}}` and `{{focus}}`
  from the Python guide, `{{input}}` from TypeScript. Both forms are now
  written out rather than left to be inferred.
- The example prompt line keeps the Python variant, which is the one that
  matches the finding step 2 states \("the report lacked financial
  ratios"\). The TypeScript variant said something different, about
  per-ticker metrics.
- Five videos drop out: ts-first-dataset, ts-first-prompt,
  ts-playground-experiment, ts-compare-prompts, ts-view-experiment. They
  carried the same captions and demonstrated the same click-path as the
  five that remain, recorded against Mastra instead of CrewAI.

All 13 code blocks and every link destination are unchanged. 668 -> 538
lines.

Braces checked: every `{{...}}` on the page is either inside a fenced
block or an inline code span, so MDX treats them as text. Verified in the
rendered DOM.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: restructure Get Started nav and verify evals against the product

Reshape the Get Started nav to Observe Your Agent / Evaluate Your Agent /
PXI / Arize Phoenix for Agents / Phoenix Demo Account. Per-language pages
become tabs within each section, with language nested inside the code path.

Merge the Improve Your Agent guide into Evaluate Your Agent, which is now
written as offline evals: a dataset feeds a task, an evaluator scores each
output. Phoenix has not released continuous scoring of live production
traffic, so the previous framing was wrong.

Extract Skills from the Coding Agents page into its own page and group the
agent-facing docs \(agent-assisted setup, skills, MCP\) under Arize Phoenix
for Agents. A pointer heading keeps existing #skills anchors resolving.

Correct six claims against Phoenix 20.15.0 running locally:

- `output` is the task's output, not an example payload. `reference` is the
  example's expected output \(experiment_runner.py:831, message_helpers.py:279,
  and the compare view's "reference output" column\).
- Creating a dataset from spans is one action, not two. You select spans and
  click Add to Dataset; creating a new dataset happens inside that popover
  \(SpanSelectionToolbar.tsx:212, DatasetSelectorPopoverContent.tsx:104\).
- The evaluator dialog's submit button is Create, not Save.
- The playground control is "Test over a dataset".
- Experiment runs live on the Experiments tab, compared via the
  "comparisons" selector.
- Playground template format defaults to Mustache, so dataset input keys
  need double braces.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: remove em dashes from the Get Started nav pages

All 98 em dashes on pages now reachable from Get Started, none of them
authored by this branch: pxi.mdx \(31\), coding-agents.mdx \(22\), remote-mcp.mdx
\(14\), mcp.mdx \(4\), docs-mcp.mdx \(3\), phoenix-mcp-server.mdx \(1\), plus 14
bullet and label dashes replaced with colons.

Each was repunctuated in context rather than swapped for a single character:
a colon where a list or restatement follows, a comma for an aside, a period
where an independent clause follows, parentheses for the three parentheticals
that span hard-wrapped lines. No dash inside a fenced code block was touched.

Diff is 89 insertions and 89 deletions, so no prose was added or removed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: point Evaluate Your Agent at the trace-scoring flow

The merged page covers offline evals over experiments. Readers who want to
score traces they have already collected had no path to it from Get Started,
since that content now lives under Tracing and Evaluation. Links to
Evaluating Phoenix Traces and the two eval quickstarts.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: simplify Phoenix get started quickstarts

* docs: keep Agent, not App, in the Get Started flow

Restores "Observe Your Agent" and "Evaluate Your Agent" and aligns the
surrounding copy, which had moved to "app": both page titles and descriptions,
the section headings on the Get Started overview, the landing-page cards, and
two "Observe Your App" cross-links that would have pointed at a page with a
different name. `app.ts` stays, since that is a file the reader creates.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: one Quick Starts card group, without the Improve card

The Improve page merged into Evaluate Your Agent, so its card pointed at the
same page as the Evaluate card. Dropped it and folded its description into
Evaluate's, which had described only the LLM-as-a-judge half.

The Python and TypeScript groups held identical cards, and both target pages
now select language with tabs, so the split no longer carried information.
Collapsed to a single group. The redirect from the retired improve path is
untouched.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make Evaluate Your Agent a single Playground path

Drops the Prompt Playground / Code tab split and keeps the Playground steps,
so the quickstart teaches one path end to end. 401 lines to 146.

Adds a "Prefer to do this in code?" section that sends readers to Run
Experiments for the Python SDK and to the TypeScript experiments reference for
runExperiment, so both languages the removed tab covered still have a home.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say which traces to evaluate and what to measure

The steps described where to click without answering the two questions a
reader arrives with. Five tweaks, 146 lines to 151:

- Dataset step gives a size to aim for and two ways to find failing traces,
  a filter expression or asking PXI to walk them
- Evaluator step names error patterns worth scoring instead of only listing
  the two evaluator types
- Saving a prompt from a trace opens with why that prompt is the one to use
- The baseline is stated as the thing later edits are measured against
- Comparison step closes with how to read the delta, including what to do
  when the score drops

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say offline evaluators in the Evaluate description

Also removes two lines the single-path change left stale: an opening sentence
that restated the description, and a paragraph telling readers to choose
between the Playground and code paths, which are no longer tabs on this page.
The offline framing replaces it, with the caveat that continuous scoring of
live production traffic has not shipped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say the manual path builds everything from the Phoenix UI

Now that this page is Playground-only, the section intro should name the
surface the steps use.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: shorten the PXI line in the dataset step

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: restore the code experiment example as its own page

Making Evaluate Your Agent a single Playground path deleted Nancy's Python
and TypeScript worked examples, and the pages it linked to instead cover the
same APIs without that scenario. Recovered them verbatim from 966510c74 as
Evaluate a Prompt Change in Code, under How-to: Experiments.

All 7 code blocks and both 4-step sequences are byte-identical to the removed
tab; only the Tab wrapper was dropped and the block de-indented one level. The
page opens by tying itself to the quickstart, so the support agent scenario
carries across the UI and code paths again.

"Prefer to do this in code?" now points here first, with Run Experiments as
the wider reference.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the new experiment page headings its siblings have

The page had no H2s, so it contributed nothing to the table of contents while
every other page in How-to: Experiments has several. Adds a heading over the
tabbed walkthrough, notes that the script creates its own dataset so it runs
without traces, and points at Set a Baseline Experiment, which covers the
persistent baseline that this page's in-code comparison does not.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rework Observe and Evaluate, split PXI into Get Started and Learn More

Observe Your Agent: intro rewritten to say what the page does and where the
concepts live, `\(Recommended\)` on the agent-assisted path, and the manual
steps reordered to start Phoenix, get your keys, then set up tracing. The
install step folded into setup, and the three steps that were identical in
both language tabs hoisted above them.

Evaluate Your Agent: no longer depends on having traces. The dataset step
hands you three travel support examples to upload, so the page runs on a
fresh Phoenix. Traces became an optional route rather than the only one.
Switched to an LLM judge, added copyable Playground prompts for the baseline
and the edit, and dropped the save-a-prompt-from-a-trace step.

PXI: 3,315 words split into a 315-word Get Started \(what it is, what it does,
three steps\) and a 1,158-word Learn More grouped as capabilities, surfaces,
configuration, and privacy. Nothing deleted except the architecture diagram
and the flag and slash-command tables, which duplicate the CLI reference.
Accordions flattened so nothing is hidden behind a click.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: consolidate Arize Phoenix for Agents

Snapshot of the agents/MCP consolidation: agent-assisted-setup folded into a
new developer-tools/cli page, and remote-mcp, docs-mcp, and phoenix-mcp-server
collapsed into one integrations/mcp page, with redirects for all four retired
paths. Nav and inbound references updated across the CLI reference, release
notes, self-hosting, and the shared onboarding snippets.

Committed from the docs-get-started-reorg worktree so the two in-flight
branches can be merged.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: point the tracing quickstart at the new CLI page

The agents consolidation folded agent-assisted-setup into
developer-tools/cli. This was the last reference still relying on the
redirect rather than the real page.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: link the uploaded dataset, and cross-link the other Observe methods

The travel-support.csv download pointed at assets/datasets/, but the file was
uploaded to assets/images/arize-docs-images/evaluate/. Repointed to where it
actually lives, so the Evaluate walkthrough runs end to end. Verified the URL
serves 3 rows with the question and expected_answer columns the page maps.

Also adds links out of both quickstarts for the routes they do not teach,
prompted by the Phoenix workflow diagram: reading traces in code, from a
coding agent via the CLI or MCP, by asking PXI, and across Sessions; plus PXI
for curating datasets and running experiments. Links only, no new pages. The
Evaluate step stays absent on purpose, since online evals have not shipped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make both Evaluate paths use one dataset, and close three story gaps

Found by running every path in Get Started end to end as a new user.

The agent-assisted prompt inlined three examples, but only one of them was in
the CSV the manual path downloads. Cutting that file from eight rows to three
had left the prompt behind. The prompt now downloads the same CSV, so there is
one dataset and the two paths cannot drift again. It also restores the page's
own argument, which turns on the visa and refund rows.

Three gaps between the paths:

- The Recommended path ended when px setup finished and never said to open the
  Traces view. Only the manual path told you to look at what you captured.
- "Ask your coding agent" said to start Phoenix without giving the command,
  which only appeared in the other tab.
- Observe handed off promising to "convert selected traces into a dataset",
  which Evaluate no longer does, since it starts from a downloadable dataset.

Verified by execution, not reading: both language paths install and run and
produce correct OpenInference traces \(a support_reply CHAIN wrapping a
ChatCompletion LLM span with messages, model, and token counts\), px setup
connects and writes .env.phoenix, both skills commands install, and the CSV
builds the same 3-example dataset from either path.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: state the Python 3.10 floor and name the Traces tab in Observe

Both quickstart paths were run end to end against Phoenix 20.16.0. On
macOS's default Python 3.9, pip quietly installs arize-phoenix-otel
0.14.0 instead of 0.17.1, because the current packages require 3.10+.
A project also opens on the Spans tab, so "the Traces view" sent
readers looking for a view that isn't the landing page.

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>

* docs: make the Evaluate comparison actually move the score

The page claimed the baseline prompt invents visa rules and agrees to refunds
it should decline. Measured against gpt-5-mini it does neither: the baseline
scored 1.0, the edited prompt scored 1.0, and stripping the baseline to
"Answer the customer's question" still scored 1.0. Step 4 promised a rise that
never came, so the lesson did not land.

Fixed by changing what the examples test. They now ask about a fictional
airline's Basic fare policy, with values deliberately unlike industry norms: a
48 hour refund window, a 40 dollar change fee, one included checked bag. The
model cannot guess any of them, so the baseline answers from industry defaults
and gets all three wrong. The edit supplies the policy.

Measured: baseline 0.00, edited 1.00.

This also makes the lesson durable. The baseline fails for lack of information
rather than lack of capability, so a better model will not quietly erase the
gap the way it did with the previous examples.

Also drops the Observe handoff sentence, per review.

The dataset CSV needs re-uploading to the same path; the agent-assisted prompt
reads it by URL and needs no edit.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the PXI paths, found by running them

Tested both PXI pages against a live Phoenix for the first time.

The terminal client command did not work. `npx -y @arizeai/phoenix-cli pxi`
exits with "unknown command 'pxi'", because pxi is a separate bin in the
package rather than a px subcommand, so npx ran the default bin and passed
pxi as an argument. Needs `-p`. Verified the corrected form launches and
reaches the credential check.

PXI defaults to an Anthropic model, so a reader who follows step 1 and sets
only OPENAI_API_KEY opens the assistant and hits "Anthropic credentials are
not configured". The step now says to pick a model matching the provider you
configured. Confirmed by switching to gpt-5.5 and getting a correct answer
about project span counts.

Restores "Where your data goes" under Privacy and tracing. It was dropped with
the architecture section, but it is not architecture: it is the answer to
whether data leaves your instance. No separate Arize service, your provider
called with your key, Arize not in the request path, and remote export gated
three ways. That is the first thing a security review asks about a beta AI
feature.

Also: the provider list mixed three env var names in code with "AWS Bedrock"
in prose, so it now names providers throughout; "Drives the product" lost its
object when I trimmed it; and the roadmap milestone link is out, since a
GitHub milestone goes stale.

Verified live: the consent gate appears as described, trace saving shows
"Disabled by system settings" which matches the documented default, the model
list matches the validated set, and PXI answered using search, get_schema, and
execute, the code-mode surface the page describes.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the CLI page its own job, running Phoenix from the terminal

The page claimed px was "the recommended way for a coding agent to work with
Phoenix", then documented only px setup, which Observe already owns. So it
repeated onboarding and never showed an agent working with Phoenix. Meanwhile
px has 17 top-level commands and the page covered one.

Rewritten around the debugging loop the phoenix-cli skill actually drives:
find what broke, read one trace in full, record the judgement as an
annotation, inspect the datasets and experiments you test against, then work
on the prompt. Plus a table for the remaining commands. Setup is now a single
line pointing at Observe.

Every command was run against a live Phoenix, which caught three errors in my
own first draft:

- `px span list` has no `--filter`. It takes typed flags, so the examples use
  `--status-code ERROR` and `--span-kind LLM`.
- `px experiment list` requires `--dataset`.
- `px trace get` resolves the default project and reports "Trace not found"
  without `--project`.

Verified working: trace list, trace get, trace add-note, span list with both
filters, span annotate \(annotation upserted\), dataset list, dataset get,
experiment list, prompt list, project list, and all 11 commands in the table.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: lead the agents overview with the setup table

Move "Set up your agent" above the three-piece cards so the
per-agent map comes first, and make its lead-in name the three
pieces so it stands on its own.

Narrow the endpoint rule: the plugins and the px CLI take the base
endpoint and append /mcp themselves, while `claude mcp add` and the
per-client MCP steps take the full /mcp URL.

Reword the prompt samples as examples rather than the full set.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: give the agents overview one job, shorten the MCP page

The overview page was mostly navigation: three cards pointing at CLI,
MCP Servers, and Skills, which already sit next to it in the sidebar,
plus a setup table routing to the same three pages. The sidebar label
"Overview" also repeated the group name, so the page read as a section
divider.

Now it does one thing, connect a coding agent to Phoenix: what this is,
the disambiguation note, the per-agent setup table, the endpoint. The
three cards are gone, the prompt samples are cut from five to two, and
the node_modules section is dropped since the TypeScript SDK overview
already documents it. Relabel the sidebar entry "Set Up Your Agent".

Correct the Claude Code row: the plugin ships three skills, while
px setup skills installs five, so the plugin path needs px setup skills
too. Verified by listing the plugin and running both.

On the MCP page, lead Connect with px setup mcp, merge the Cursor,
VS Code, and OpenCode accordions \(their JSON matches what px writes\),
replace the six Docs MCP accordions with a client table, and merge the
identical npm stdio configs. 633 to 521 lines with no facts dropped.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: move the plugins onto the setup page

The setup page was still an index: its table pointed at Install
sections that CLI, MCP Servers, and Skills each already have. What it
lacked was the thing that belongs to no single one of them, the
one-install-per-agent path.

That path was the Plugins section, 137 lines and the largest section on
the MCP page, which is the wrong home for a plugin that registers MCP
and skills together. Move it here, after the endpoint it references,
and leave a pointer on the MCP page.

MCP drops to 388 lines and covers only MCP servers. The setup page now
routes by agent, states the shared endpoint, then gives the full plugin
steps.

Also repoint skills.mdx at the plugins section's new home, and fix its
link to a CLI heading that the CLI rewrite removed.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: fix the MCP page opener

It said "two things you might want your assistant to do" and "run
both" above a three-row table. Name the two servers that matter, your
data and the documentation, and say what the third row is.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: cut duplicated prose from the MCP page

The Configuration table repeated two env vars that self-hosting
configuration already documents; point there and keep only
PHOENIX_SKILLS_PATHS, which is unique to this page.

Monty sandbox limits were spelled out twice, in How it works and again
in Security; state them once. Merge the code-mode Warning and Note,
which both said the same thing. Trim the OAuth paragraph. Move the
OAuth2 authorization-server paragraph out of "Mounting your own
skills", where it was misfiled, into Security. Cut the npm sample
questions from seven to three.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: clarify the direction of the agents note

"Operate on Phoenix" is jargon, and "trace your sessions with a coding
agent" reads either way. Say which direction each page covers: this one
connects the agent to Phoenix, the other has Phoenix record the agent.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: correct two plugin commands that fail as written

Both found by running them. claude plugin uninstall acts on user scope
by default, so it stops with a scope error on a plugin installed at
local or project scope; say to pass the scope, and add disable, which
has the same rule. claude plugin details reports a disabled plugin as
not found, so say it has to be enabled, and say what the command lists.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: correct two CLI table rows that overstated what the commands do

Found by running them. px api has one subcommand, graphql, and rejects
mutations, so "authenticated requests to any Phoenix API route" is
wrong. The three annotation commands take only delete, and require
--all or a time window, so "stored annotations" hid that they are
destructive.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: make the CLI debugging loop match what px can do

Steps 4 and 5 were titled "Turn failures into a test set" and "Work on
the prompt", but every command under them only reads, and the bodies
said "Inspect", contradicting the titles. px has no way to create a
dataset, add examples, or edit a prompt: for datasets, prompts, and
experiments it offers list, get, and delete only.

Cut the loop to the three steps px actually supports, ending at the
annotation, which with notes is the only thing it writes. Move the
dataset, experiment, and prompt reads into their own section framed as
looking up context, and say plainly that authoring happens in the UI
or the SDK.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rewrite the CLI page as the workflow an agent runs

The page had a "debugging loop" that was not a loop: it ended at an
annotation, and px cannot fix anything. Reorganising it by capability
then made it a feature list. Put it back in order as the pass an agent
actually runs, and let step 4 say plainly that px stops there and the
fix goes in your code, which is what closes the loop.

The examples were the weaker half. They repeated a fake project four
times, used placeholder ids that cannot be run, and carried comments
that restated the command instead of the output. Use real ids, say
which command feeds the next, and split the dataset, experiment, and
prompt reads into their own section, since those are for checking a
change rather than part of the debugging pass.

Add three facts found by running the commands: reads accept --format
json, annotate and add-note want the OpenTelemetry id rather than the
opaque one, and --identifier makes a repeat call overwrite.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: note that px annotation deletes are gated off by default

px span-annotations delete answers "Delete commands are disabled."
before it looks at any flag. Name the variable that opens the gate,
PHOENIX_CLI_DANGEROUSLY_ENABLE_DELETES, so the row does not send a
reader at a command that refuses to run.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: say who runs px, and that the docs MCP URL is not a page

The CLI page opened by saying px is how a coding agent works with
Phoenix, then showed a reader commands to type, and titled the section
"How an agent uses it". A reader could not tell whether the commands
were theirs to run. Say it is an ordinary CLI you run yourself, and
that needing only a shell is what makes it easy to hand to an agent.

The Phoenix Docs MCP URL sat in a bare code block, so it read as a link
to open. A browser shows the tool manifest as JSON, which looks like an
error and is not. Say it is an endpoint for a client.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: answer px versus MCP, and add agent instructions to both pages

Neither page said when to reach for which. That choice only existed as
cards on the setup page, which is not where someone deciding ends up.
The CLI page now compares the two on what the client must support, what
setup costs, and how much surface each exposes, and the MCP page points
at it.

Both pages end with a section addressed to an agent reading them,
covering how to discover flags, which output format to parse, the
OpenTelemetry id the annotate commands want, the execute sandbox limits
and import allowlist, and that trace contents are untrusted data.

Every claim in both sections was run first: 116 catalog operations, the
50-call cap, json/re/datetime importable and socket/subprocess/importlib
not, exit status 0 and 1, and the delete gate.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>

* docs: rebuild the CLI page to a fixed outline, and align sibling names

The page now follows one order: what it is and why it matters, CLI or
MCP server, set up, what you can do with it, learn more, agent
instructions. Setup is three steps ending in a check. The examples
keep their real ids and drop the repeated --project in favour of
PHOENIX_PROJECT, set once in step two. The command table is gone; the
two caveats it carried, the delete gate and read-only GraphQL, live in
the agent instructions, and the full list is a link to the reference.

An audit of how each page in the section names its siblings found six
mismatches: the CLI page linked as "px CLI" and as bare "MCP", the
Remote MCP Server as "server", and a PXI section by a title it does
not have. Each link text now matches the page or section it points at.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: name the CLI setup check by what it proves

"Check it" said nothing. The step confirms the CLI reached the
instance, so say that, and say what success looks like.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say what the CLI setup is for before the steps

The section went straight from the heading to step one, so a reader
could not tell what was being set up or where. Name the CLI in the
heading, say to install it where the agent runs, and say what having
finished looks like.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say what px project list returns on a fresh instance, drop "has a terminal"

The setup check said a table of your projects means success, which
leaves a reader with no traces unsure what to expect. Tested on an
empty instance: every Phoenix has a project named default, so the
command always returns at least one row, and a wrong endpoint fails
with "Error fetching projects: fetch failed". Say both.

"When your agent has a terminal" was jargon on the CLI and MCP pages.
Say what it means: the agent can run shell commands, which every
coding agent can.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the CLI-first recommendation, say what the default project is

"Start with the CLI" was a preference with nothing behind it, and it
contradicted the setup page, which sends Claude Code and Codex users
to the plugin first, and the MCP page, which calls itself the primary
choice. The comparison table carries the facts; let the reader choose.
The MCP page mirror is now a plain pointer.

The setup check mentions the default project. Say what it is: created
by the initial migration, the destination for traces sent without a
project name, and empty on a fresh instance. Not a demo.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: make the CLI page work for a user with no traces yet

Run as a brand-new user, in a clean environment against an empty
instance, the previous page failed at step two: PHOENIX_PROJECT=my-app
404s on every read, and every example carried an id from one machine.
Stuff has to work before a project has traces.

Follow the shape of the live CLI section: install, endpoint, verify
with px --help and px project list, which both succeed with zero data,
then the common agent workflows as bullets and one example prompt, so
the reader sees what to ask rather than what the agent types. Say that
PHOENIX_PROJECT comes later, once traces are flowing. Keep the CLI or
MCP table and the agent instructions, and add the missing rule that
read commands need a project.

On the MCP page, restore the per-client tabs for the Docs MCP from the
live site, adding Claude Desktop.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: one story across Arize Phoenix for Agents, working from zero traces

The section was organised by plumbing, CLI, MCP, Skills, and every page
assumed traces already existed. A new user has none. Reframe around
what is true from the start: connect the agent, let it instrument your
app, then debug from what Phoenix records.

Set Up Your Agent owns routing, the endpoint, and the plugins. Its
connection check is a question to the agent, list my Phoenix projects,
which a fresh instance answers with default. Verified through both
pipes on an empty instance: px project list, and MCP search finding
getProjects and execute returning it. A new section points at the
agent-assisted tracing path as the next step.

CLI, MCP Servers, and Skills share one skeleton: what it is, install
ending in a check, what your agent can do with it, learn more,
reference. Deep material on the MCP page, API-key fallback, code mode
internals, server configuration, security, the npm server, sits under
Reference. The MCP Endpoint section folds into one line. The skills
page moves its options table and custom-skills guidance under
Reference and gains an example prompt.

Every shell command on the four pages was run in a clean environment
against an empty Phoenix. Restore the remote-mcp-server anchor, which
eleven pages link to, and repoint two onboarding snippets that still
named a CLI heading removed in an earlier rewrite.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut the instrumentation pointer back to one line

It was a sentence before, and a heading with three sentences after. The
link is the useful part.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: nest the MCP capabilities section under Remote MCP Server

The page covers two servers, so "what your agent can do with it" as a
top-level heading left "it" ambiguous. Make it an H3 under Remote MCP
Server, alongside a new Connect H3 for the setup steps.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut duplicated prose from the MCP page

The intro paragraph restated the table under it. The capabilities
section under Remote MCP Server repeated Reference, How it works,
nearly sentence for sentence; point there instead. Three sentences of
OAuth mechanism sat ahead of the first command; one line after the
accordions covers what a reader needs. The Docs MCP intro restated its
table row, the npm section said maintenance mode twice, and the
mounted-skills paragraph named its variable twice in a row.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: link the CLI page back to Set Up Your Agent

The MCP and Skills pages both point back to the setup page; the CLI
page did not, so a reader who landed there directly had no route to
the per-agent install.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: frame PXI skills as example capabilities

The table led with skill names, which is the mechanism. Lead with what
you can ask PXI to do, give one example ask per row, and keep the skill
concept as a single sentence after the table.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: frame PXI models as bring your own, and trim

Lead with the point: PXI runs on your provider and your key. Name the
providers once, keep the validated-model list and the tool-calling
caveat, drop the rest. Repoint the two links that used the old anchor.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the agent-instructions section from the CLI page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the agent-instructions section from the MCP page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop the validated-model list from PXI

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: stop promising a supported-models list PXI no longer has

The list was removed; three references still pointed at it. Retitle the
Get Started link to the section that exists, and say model providers
where the Learn more line and the page description said supported
models.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: remove the npm MCP server from the MCP page

Drop the maintenance-mode section, its row in the intro table, and the
three places that offered it as a fallback: the version-floor line, the
Claude Desktop accordion, and the troubleshooting entry.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: act on the docs review: skills guidance, Docs MCP callout, MCP cut-down

Skills page: name the five skills written for Phoenix users, add the
plugin-versus-skills-add guidance from the review \(plugins track
Phoenix releases; skills add is a one-time copy, rerun it or run npx
skills update\), and take ownership of mounting your own skills from the
MCP page. Document --all honestly: tested in a fresh project it
installed 26 skills to 73 agents, including internal development
skills, so it is the wrong default.

MCP page: Phoenix Docs MCP becomes a short block near the top, as the
review asked, in place of an 87-line section. Fold Antigravity into the
other-clients accordion, trim How it works to the tool table plus two
sentences, and drop the mounting section now that Skills owns it.
361 to 271 lines.

Evaluate Your Agent: move the spans screenshot out of the Note it was
nested in.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: bring llms.txt in line with the Get Started and agents reorg

Add the CLI, Skills, and PXI Learn More pages, which had no entries.
Remove seven Quick Start entries for pages the reorg deleted or merged
into Observe Your Agent and Evaluate Your Agent, and three MCP entries
for pages folded into the single MCP page. Retitle the survivors to
match their pages.

Add the agent-usage guidance for the px CLI and the Remote MCP Server
here, where the review said it belongs, instead of on the human pages.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: give the CLI guide entry in llms.txt a unique title

It collided with the @arizeai/phoenix-cli reference entry.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten the endpoint rule on the setup page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: halve the agent-assisted setup on Observe Your Agent

Start Phoenix once above the tabs instead of in each. One sentence of
outcome per tab. Cut the pasted prompt to the two instructions that
matter, since the tracing skill carries the procedure. Drop the agent
list and the CLI pointer, which the setup page owns.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: add the PXI path to Evaluate Your Agent

A prompt to hand PXI once the dataset exists and the Playground has it
selected. Both preconditions come from running it: PXI cannot fetch a
URL, and its playground and evaluator skills act on the page you are on.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: restore the agent-assisted shape on Observe, trimmed

Back to the two tabs as they were, each self-contained, with the full
prompt. Merge the px setup outcome into one paragraph. Replace the
named --skill list with px setup skills, which installs exactly the
five Phoenix skills for every detected agent: tested via npx in a fresh
project with no CLI installed.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: repoint a PXI conversations link to the Learn More page

The Conversations section moved there when PXI split into Get Started
and Learn More; one link still pointed at the old page.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: Evaluate Your Agent builds a reference-aware evaluator

Built-in LLM templates have no reference field and score the baseline
higher than the fixed prompt, so step 2 now walks through Create new
LLM evaluator with a short rubric and the reference path mapping. PXI
section hands off after the dataset and evaluator exist, since it
cannot fetch the CSV and its own evaluator scores 0 at run time.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten Evaluate Your Agent evaluator step and PXI hand-off

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: PXI runs the full Evaluate flow from one prompt

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten Evaluate Your Agent prose

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: cut PXI Learn More to the essentials

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: one-line rows in the PXI capabilities table

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: shorten PXI privacy and tracing

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: compact PXI configuration table

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: shorten CLI page intro

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: tighten MCP Servers prose

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop em dashes from Get Started snippets

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: add the follow-up prompt to the coding agent path

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: make px setup skills the primary install on the Skills page

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say where the model API key goes before creating an LLM evaluator

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: the key must be on the Server tab for evaluators to use it

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: say when to pick the CLI and when the MCP server

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: CLI vs MCP as best-when guidance

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop redundant line from CLI or MCP section

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: plugin rows say what the plugin covers and what is optional

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs: drop Docs MCP aside from Set Up Your Agent

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

*…")  | Oct 9, 2026  |  
|   |   |  [ci: Unittest speed enhancements (](https://github.com/Arize-ai/phoenix/commit/58c595cff729a239e9df6401db0a035a1757e990 "ci: Unittest speed enhancements \(#11328\)

* ci: sqlite database fixture improvements

* ci: Implement schema injection for faster unit testing

* ci: Skip daemons in unit tests

* ci: Re-enable xdist and directly control daemon cycles

* ci: Trim very slow tests

* ci: Improve postgresql test isolation using template databases

* ci: Fix unit test type checking

* ci: Ensure proper task cleanup

* ci: Revert postgres test changes and clean up debugging artifacts

* ci: Revert unrelated model store test changes

* PR review cleanup

* ci: Isolate integration test for parallel execution

* Revert app changes

* Use -4 instead of -n to force CI core usage")[#11328](https://github.com/Arize-ai/phoenix/pull/11328)[)](https://github.com/Arize-ai/phoenix/commit/58c595cff729a239e9df6401db0a035a1757e990 "ci: Unittest speed enhancements \(#11328\)

* ci: sqlite database fixture improvements

* ci: Implement schema injection for faster unit testing

* ci: Skip daemons in unit tests

* ci: Re-enable xdist and directly control daemon cycles

* ci: Trim very slow tests

* ci: Improve postgresql test isolation using template databases

* ci: Fix unit test type checking

* ci: Ensure proper task cleanup

* ci: Revert postgres test changes and clean up debugging artifacts

* ci: Revert unrelated model store test changes

* PR review cleanup

* ci: Isolate integration test for parallel execution

* Revert app changes

* Use -4 instead of -n to force CI core usage")  | Feb 13, 2026  |  
| [release-please-config.json](https://github.com/Arize-ai/phoenix/blob/main/release-please-config.json "release-please-config.json")  | [release-please-config.json](https://github.com/Arize-ai/phoenix/blob/main/release-please-config.json "release-please-config.json")  |  [chore: release arize-phoenix 20.14.0 (](https://github.com/Arize-ai/phoenix/commit/829f1163fd898d6711fd97d7cc6b5c78fafca87c "chore: release arize-phoenix 20.14.0 \(#16258\)

Release-As: 20.14.0")[#16258](https://github.com/Arize-ai/phoenix/pull/16258)[)](https://github.com/Arize-ai/phoenix/commit/829f1163fd898d6711fd97d7cc6b5c78fafca87c "chore: release arize-phoenix 20.14.0 \(#16258\)

Release-As: 20.14.0")  | Sep 17, 2026  |  
|   |   |  [fix: harden one-click deploy templates (](https://github.com/Arize-ai/phoenix/commit/14662353acc47bf3b8d9a0e13d8d70b0a0c0f0cf "fix: harden one-click deploy templates \(#14323\)

* fix: harden one-click deploy templates

- Azure: random PHOENIX_SECRET via newGuid\(\) \(uniqueString is deterministic
  and recomputable\), require an admin password instead of defaulting to
  'admin', replace SQLite-on-Azure-Files \(unsupported by SQLite WAL over
  SMB\) with an Azure Database for PostgreSQL flexible server, set
  restartPolicy to Always, and surface a plain-HTTP warning in the
  metadata, outputs, and README
- Render: provision 50GB of Postgres storage via diskSizeGB, document the
  rare PHOENIX_SECRET digit-validation failure
- All templates: correct the claim that setting an initial admin password
  skips the forced password change on first login \(it does not\)
- README: note that the Google Cloud button builds from source

* docs: make all deploy buttons the same height

* docs: add one-click deploy buttons to self-hosting docs

* fix: bump Render plan to standard, starter's 512MB OOMs under span queries")[#14323](https://github.com/Arize-ai/phoenix/pull/14323)[)](https://github.com/Arize-ai/phoenix/commit/14662353acc47bf3b8d9a0e13d8d70b0a0c0f0cf "fix: harden one-click deploy templates \(#14323\)

* fix: harden one-click deploy templates

- Azure: random PHOENIX_SECRET via newGuid\(\) \(uniqueString is deterministic
  and recomputable\), require an admin password instead of defaulting to
  'admin', replace SQLite-on-Azure-Files \(unsupported by SQLite WAL over
  SMB\) with an Azure Database for PostgreSQL flexible server, set
  restartPolicy to Always, and surface a plain-HTTP warning in the
  metadata, outputs, and README
- Render: provision 50GB of Postgres storage via diskSizeGB, document the
  rare PHOENIX_SECRET digit-validation failure
- All templates: correct the claim that setting an initial admin password
  skips the forced password change on first login \(it does not\)
- README: note that the Google Cloud button builds from source

* docs: make all deploy buttons the same height

* docs: add one-click deploy buttons to self-hosting docs

* fix: bump Render plan to standard, starter's 512MB OOMs under span queries")  | Jul 13, 2026  |  
|   |   |   | Oct 9, 2026  |  
|   |   |  [chore(skills): add official Harbor skills (](https://github.com/Arize-ai/phoenix/commit/3b3683272645ff5a32488a2d635d9a1b1bb2a794 "chore\(skills\): add official Harbor skills \(#16384\)

* chore\(skills\): add official Harbor skills

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* chore\(skills\): keep only the Harbor task, verifier, and exec skills

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

---------

Co-authored-by: Claude Fable 5.1 <noreply@anthropic.com>")[#16384](https://github.com/Arize-ai/phoenix/pull/16384)[)](https://github.com/Arize-ai/phoenix/commit/3b3683272645ff5a32488a2d635d9a1b1bb2a794 "chore\(skills\): add official Harbor skills \(#16384\)

* chore\(skills\): add official Harbor skills

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* chore\(skills\): keep only the Harbor task, verifier, and exec skills

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

---------

Co-authored-by: Claude Fable 5.1 <noreply@anthropic.com>")  | Sep 22, 2026  |  
|   |   | [refactor(harbor): install the verifier helpers as the harbor_verifier…](https://github.com/Arize-ai/phoenix/commit/6483bf55031e345296460d3c0f8dff6756b48946 "refactor\(harbor\): install the verifier helpers as the harbor_verifiers wheel \(#16699\)

* fix\(harbor\): keep /tests readable by root only

Harbor uploads each step's tests/ into /tests before verification and does not remove
them afterwards, so in a multi-step task the agent for a later step could read the earlier
checks and the task-level helpers. Docker's copy into an existing directory keeps its
mode, and Harbor's between-step reset of /tests does not chmod it, so creating the
directory root-only in the image hides the checks from the agent user.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* refactor\(harbor\): install the verifier helpers as the harbor_verifiers wheel

The shared verifiers were copied into the image at /opt/verifier and reached through
PYTHONPATH in every test.sh and a sys.path edit in every reference solution, and mypy
had to exclude the staged copies. The helpers now form the harbor_verifiers package under
evals/harbor/verifiers, a uv workspace member that staging builds into the task build
context beside the server wheel. Tasks import it like any other installed package.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* ci: install harbor-verifiers in tox and check it out in sparse jobs

The tox unit-test environments install the root project without the dev group, so the
Harbor tests could not import the new package. The filter-DSL and JSON canonicalization
jobs run uv from a sparse checkout, and uv rejects the dev group when the workspace member
it names is absent.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* fix\(harbor\): check for the staged wheels instead of the removed verifier directory

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* refactor\(harbor\): move the LLM judge into the prompt-hill-climb task and return a Score

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* docs\(harbor\): trim the mypy comment and reword the /tests note

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* test\(harbor\): match the judge cassette on parsed JSON bodies

The unit-test environment upgrades the OpenAI client, and its newest release
serializes model before messages. The cassette has no content-type header, so
VCR compared raw bytes and no recorded request matched.

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

* refactor\(harbor\): import harbor_verifiers in the eight new TRAIL tasks

Co-Authored-By: Claude Fable 5.1 <noreply@anthropic.com>

---------

Co-authored-by: Claude Fable 5.1 <noreply@anthropic.com>")  | Oct 6, 2026  |  
|   |   |  [fix(agents): upgrade pydantic-ai for](https://github.com/Arize-ai/phoenix/commit/3cdb004e7b9299c500a0686b85942ef4b590500e "fix\(agents\): upgrade pydantic-ai for CVE-2026-107286 \(#16893\)

Require the first patched pydantic-ai-slim release and refresh the lockfile to the latest stable version allowed by the project resolver policy.

AI-generated-by: OpenAI Codex

Co-authored-by: alex-strattab <alex-strattab@users.noreply.github.com>") [CVE-2026-107286](https://github.com/advisories/GHSA-6fqq-452j-qhrp "CVE-2026-107286") [(](https://github.com/Arize-ai/phoenix/commit/3cdb004e7b9299c500a0686b85942ef4b590500e "fix\(agents\): upgrade pydantic-ai for CVE-2026-107286 \(#16893\)

Require the first patched pydantic-ai-slim release and refresh the lockfile to the latest stable version allowed by the project resolver policy.

AI-generated-by: OpenAI Codex

Co-authored-by: alex-strattab <alex-strattab@users.noreply.github.com>")[#16893](https://github.com/Arize-ai/phoenix/pull/16893)[)](https://github.com/Arize-ai/phoenix/commit/3cdb004e7b9299c500a0686b85942ef4b590500e "fix\(agents\): upgrade pydantic-ai for CVE-2026-107286 \(#16893\)

Require the first patched pydantic-ai-slim release and refresh the lockfile to the latest stable version allowed by the project resolver policy.

AI-generated-by: OpenAI Codex

Co-authored-by: alex-strattab <alex-strattab@users.noreply.github.com>")  | Oct 9, 2026  |  
| View all files |  
## Repository files navigation
**🇺🇸 English** • [🇨🇳 中文](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.zh.md) • [🇹🇼 繁體中文](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.zh-TW.md) • [🇯🇵 日本語](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.ja.md) • [🇵🇹 Português](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.pt-PT.md) • [🇧🇷 Português](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.pt-BR.md) • [🇰🇷 한국어](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.ko.md) • [🇪🇸 Español](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.es.md) • [🇩🇪 Deutsch](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.de.md) • [🇫🇷 Français](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.fr.md) • [🇮🇱 עברית](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.he.md) • [🇸🇦 العربية](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.ar.md) • [🇷🇺 Русский](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.ru.md) • [🇵🇱 Polski](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.pl.md) • [🇨🇿 Čeština](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.cs.md) • [🇳🇱 Nederlands](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.nl.md) • [🇹🇷 Türkçe](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.tr.md) • [🇺🇦 Українська](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.uk.md) • [🇻🇳 Tiếng Việt](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.vi.md) • [🇵🇭 Tagalog](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.tl.md) • [🇮🇩 Indonesia](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.id.md) • [🇹🇭 ไทย](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.th.md) • [🇮🇳 हिन्दी](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.hi.md) • [🇧🇩 বাংলা](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.bn.md) • [🇵🇰 اردو](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.ur.md) • [🇷🇴 Română](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.ro.md) • [🇸🇪 Svenska](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.sv.md) • [🇮🇹 Italiano](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.it.md) • [🇬🇷 Ελληνικά](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.el.md) • [🇭🇺 Magyar](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.hu.md) • [🇫🇮 Suomi](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.fi.md) • [🇩🇰 Dansk](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.da.md) • [🇳🇴 Norsk](https://github.com/Arize-ai/phoenix/blob/main/docs/i18n/README.no.md)
Arize Phoenix is Arize's open-source AI observability platform designed for experimentation, evaluation, and troubleshooting. For managed production workflows, Arize also offers [Arize AX](https://arize.com/products/ax/). Phoenix provides:
  * [**_Tracing_**](https://arize.com/docs/phoenix/tracing/llm-traces) - Trace your LLM application's runtime using OpenTelemetry-based instrumentation.
  * [**_Evaluation_**](https://arize.com/docs/phoenix/evaluation/llm-evals) - Leverage LLMs to benchmark your application's performance using response and retrieval evals.
  * [**_Datasets_**](https://arize.com/docs/phoenix/datasets-and-experiments/overview-datasets) - Create versioned datasets of examples for experimentation, evaluation, and fine-tuning.
  * [**_Experiments_**](https://arize.com/docs/phoenix/datasets-and-experiments/overview-datasets#experiments) - Track and evaluate changes to prompts, LLMs, and retrieval.
  * [**_Playground_**](https://arize.com/docs/phoenix/prompt-engineering/overview-prompts)- Optimize prompts, compare models, adjust parameters, and replay traced LLM calls.
  * [**_Prompt Management_**](https://arize.com/docs/phoenix/prompt-engineering/overview-prompts/prompt-management)- Manage and test prompt changes systematically using version control, tagging, and experimentation.
  * [**_PXI (Phoenix Intelligence)_**](https://arize.com/docs/phoenix/pxi) - An AI engineering agent built into Phoenix for debugging traces, iterating on prompts, and navigating the product.
  * [**_Remote MCP Server_**](https://arize.com/docs/phoenix/integrations/mcp#remote-mcp-server) - Connect Claude Code, Cursor, and other MCP clients directly to your Phoenix instance's `/mcp` endpoint to query traces, datasets, experiments, and more.


Phoenix is vendor and language agnostic with out-of-the-box support for popular frameworks ([OpenAI Agents SDK](https://arize.com/docs/phoenix/tracing/integrations-tracing/openai-agents-sdk), [Claude Agent SDK](https://arize.com/docs/phoenix/integrations/python/claude-agent-sdk), [LangGraph](https://arize.com/docs/phoenix/tracing/integrations-tracing/langchain), [Vercel AI SDK](https://arize.com/docs/phoenix/tracing/integrations-tracing/vercel-ai-sdk), [Mastra](https://arize.com/docs/phoenix/integrations/typescript/mastra), [CrewAI](https://arize.com/docs/phoenix/tracing/integrations-tracing/crewai), [LlamaIndex](https://arize.com/docs/phoenix/tracing/integrations-tracing/llamaindex), [DSPy](https://arize.com/docs/phoenix/tracing/integrations-tracing/dspy)) and LLM providers ([OpenAI](https://arize.com/docs/phoenix/tracing/integrations-tracing/openai), [Anthropic](https://arize.com/docs/phoenix/tracing/integrations-tracing/anthropic), [Google GenAI](https://arize.com/docs/phoenix/tracing/integrations-tracing/google-genai), [Google ADK](https://arize.com/docs/phoenix/integrations/llm-providers/google-gen-ai/google-adk-tracing), [AWS Bedrock](https://arize.com/docs/phoenix/tracing/integrations-tracing/bedrock), [OpenRouter](https://arize.com/docs/phoenix/integrations/python/openrouter), [LiteLLM](https://arize.com/docs/phoenix/tracing/integrations-tracing/litellm), and more). For details on auto-instrumentation, check out the [OpenInference](https://github.com/Arize-ai/openinference) project.
Phoenix runs practically anywhere, including your local machine, a containerized deployment, or in the cloud. See [Environments](https://arize.com/docs/phoenix/environments) for a walkthrough of each option, or jump straight into the [Tracing Quickstart](https://arize.com/docs/phoenix/get-started/get-started-tracing).
Note
Phoenix is open-source and self-hosted. For a managed platform, Arize offers [Arize AX](https://arize.com/products/ax/) — the same OpenTelemetry/OpenInference instrumentation works with both. See the [full comparison](https://arize.com/docs/phoenix/resources/frequently-asked-questions/what-is-the-difference-between-phoenix-and-arize).
## Table of Contents
  * [Trace Your Application](https://github.com/Arize-ai/phoenix#trace-your-application)
  * [Tracing Integrations](https://github.com/Arize-ai/phoenix#tracing-integrations)
  * [For Humans and Coding Agents](https://github.com/Arize-ai/phoenix#for-humans-and-coding-agents)
  * [Security & Privacy](https://github.com/Arize-ai/phoenix#security--privacy)


## Run Locally
Install Phoenix via `pip` or `conda` and have a fully functional Phoenix. For all installation and hosting options, see the [install guide](https://arize.com/docs/phoenix).

```
pip install arize-phoenix
phoenix serve
```

Or run it with no install using [`uvx`](https://docs.astral.sh/uv/):

```
uvx arize-phoenix serve
```

## Trace Your Application
The fastest way to send traces is to let your [coding agent](https://arize.com/docs/phoenix/integrations/developer-tools/coding-agents) (Claude Code, Codex, Cursor, and others) instrument your app. From your project directory, run:

```
npx @arizeai/phoenix-cli setup
# or, with Phoenix installed: px setup
```

Setup detects your framework and LLM provider, installs the right [OpenInference](https://github.com/Arize-ai/openinference) instrumentation, and wires up trace export. Prefer to wire it up in code? See the [tracing documentation](https://arize.com/docs/phoenix/tracing/llm-traces).
## Deploy
Phoenix container images are available via [Docker Hub](https://hub.docker.com/r/arizephoenix/phoenix) and can be deployed using Docker or Kubernetes via the [Helm chart](https://github.com/Arize-ai/phoenix/tree/main/helm).
For Docker Compose, Kubernetes/Helm, and other deployment options, see the [self-hosting documentation](https://arize.com/docs/phoenix/self-hosting).
Note
The Google Cloud button builds Phoenix from source in Cloud Shell rather than deploying the prebuilt Docker Hub image. The Azure template serves plain HTTP (Azure Container Instances does not terminate TLS) — front it with a TLS proxy such as an Application Gateway before production use.
## Packages
The `arize-phoenix` package includes the entire Phoenix platform. However, if you have deployed the Phoenix platform, there are lightweight Python sub-packages and TypeScript packages that can be used in conjunction with the platform.
### Python Subpackages  
| Package  | Version & Docs  | Description  |  
| --- | --- | --- |  
| Provides a lightweight wrapper around OpenTelemetry primitives with Phoenix-aware defaults  |  
| [arize-phoenix-client](https://github.com/Arize-ai/phoenix/tree/main/packages/phoenix-client)  | Lightweight client for interacting with the Phoenix server via its OpenAPI REST interface  |  
| Tooling to evaluate LLM applications including RAG relevance, answer relevance, and more  |  
### TypeScript Subpackages  
| Package  | Version & Docs  | Description  |  
| --- | --- | --- |  
| [@arizeai/phoenix-otel](https://github.com/Arize-ai/phoenix/tree/main/js/packages/phoenix-otel)  | Provides a lightweight wrapper around OpenTelemetry primitives with Phoenix-aware defaults  |  
| [@arizeai/phoenix-client](https://github.com/Arize-ai/phoenix/tree/main/js/packages/phoenix-client)  | Client for the Arize Phoenix API  |  
| [@arizeai/phoenix-evals](https://github.com/Arize-ai/phoenix/tree/main/js/packages/phoenix-evals)  | TypeScript evaluation library for LLM applications (alpha release)  |  
| [@arizeai/phoenix-mcp](https://github.com/Arize-ai/phoenix/tree/main/js/packages/phoenix-mcp)  | Standalone stdio MCP server for older Phoenix versions (maintenance mode — superseded by the [remote MCP server](https://arize.com/docs/phoenix/integrations/mcp#remote-mcp-server) built into Phoenix)  |  
| [@arizeai/phoenix-cli](https://github.com/Arize-ai/phoenix/tree/main/js/packages/phoenix-cli)  | CLI for fetching traces, datasets, and experiments for use with Claude Code, Cursor, and other coding agents  |  
## Tracing Integrations
Phoenix is built on top of OpenTelemetry and is vendor, language, and framework agnostic. For details about tracing integrations and example applications, see the [OpenInference](https://github.com/Arize-ai/openinference) project and the [integrations documentation](https://arize.com/docs/phoenix/integrations).
**Python Integrations**  
| Integration  | Package  | Version  |  
| --- | --- | --- |  
| `openinference-instrumentation-openai`  |  
| `openinference-instrumentation-openai-agents`  |  
| `openinference-instrumentation-llama-index`  |  
| `openinference-instrumentation-dspy`  |  
| `openinference-instrumentation-bedrock`  |  
| `openinference-instrumentation-langchain`  |  
| `openinference-instrumentation-langchain`  |  
| `openinference-instrumentation-mistralai`  |  
| `openinference-instrumentation-cohere`  |  
| `openinference-instrumentation-together`  |  
| `openinference-instrumentation-ollama`  |  
| `openinference-instrumentation-google-genai`  |  
| `openinference-instrumentation-google-adk`  |  
| `openinference-instrumentation-guardrails`  |  
| `openinference-instrumentation-vertexai`  |  
| `openinference-instrumentation-crewai`  |  
| `openinference-instrumentation-haystack`  |  
| `openinference-instrumentation-litellm`  |  
| `openinference-instrumentation-openai`  |  
| `openinference-instrumentation-openai`  |  
| `openinference-instrumentation-groq`  |  
| `openinference-instrumentation-instructor`  |  
| `openinference-instrumentation-anthropic`  |  
| `openinference-instrumentation-smolagents`  |  
| `openinference-instrumentation-agno`  |  
| `openinference-instrumentation-beeai`  |  
| `openinference-instrumentation-strands-agents`  |  
| `openinference-instrumentation-openai-agents`  |  
| `openinference-instrumentation-mcp`  |  
| `openinference-instrumentation-pydantic-ai`  |  
| `openinference-instrumentation-ag2`  |  
| `openinference-instrumentation-autogen-agentchat`  |  
| `openinference-instrumentation-portkey`  |  
| `openinference-instrumentation-agentspec`  |  
| `openinference-instrumentation-claude-agent-sdk`  |  
| `openinference-instrumentation-typesafe`  |  
## Span Processors
Normalize and convert data across other instrumentation libraries by adding span processors that unify data.  
| Package  | Description  | Version  |  
| --- | --- | --- |  
| [`openinference-instrumentation-openlit`](https://pypi.org/project/openinference-instrumentation-openlit/)  | OpenInference Span Processor for OpenLIT traces.  |  
| [`openinference-instrumentation-openllmetry`](https://pypi.org/project/openinference-instrumentation-openllmetry/)  | OpenInference Span Processor for OpenLLMetry (Traceloop) traces.  |  
### JavaScript Integrations  
| Integration  | Package  | Version  |  
| --- | --- | --- |  
| `@arizeai/openinference-instrumentation-openai`  |  
| `@arizeai/openinference-instrumentation-openai-agents`  |  
| `@arizeai/openinference-instrumentation-langchain`  |  
| `@arizeai/openinference-tanstack-ai`  |  
| `@arizeai/openinference-vercel`  |  
| `@arizeai/openinference-instrumentation-beeai`  |  
| `@arizeai/openinference-instrumentation-claude-agent-sdk`  |  
| `@mastra/arize`  |  
| `@arizeai/openinference-instrumentation-mcp`  |  
| `@arizeai/openinference-instrumentation-typesafe`  |  
### Java Integrations  
| Integration  | Package  | Version  |  
| --- | --- | --- |  
| `openinference-instrumentation-langchain4j`  |  
| SpringAI  | `openinference-instrumentation-springAI`  |  
|  [Arconia](https://docs.arconia.io/arconia/latest/observability/semantic-conventions/openinference/) for Spring AI  | `io.arconia:arconia-openinference-semantic-conventions`  |  
| `openinference-instrumentation-adk-java`  |  
### Go Integrations  
| Integration  | Package  | Version  |  
| --- | --- | --- |  
| `github.com/Arize-ai/openinference/go/openinference-instrumentation-openai-go`  |  
| `github.com/Arize-ai/openinference/go/openinference-instrumentation-anthropic-sdk-go`  |  
### Platforms  
| Platform  | Description  | Docs  |  
| --- | --- | --- |  
| AI agent framework with built-in observability  |  
| Open-source LLM app development platform  |  
| AI Gateway built on Envoy Proxy for AI workloads  |  
| Visual framework for building multi-agent and RAG applications  |  
| Proxy server for LLMs  |  
| Visual framework for building LLM applications  |  
| Microsoft's prompt flow orchestration tool  |  
| NVIDIA NeMo Agent Toolkit for enterprise agents  |  
| Multi-agent LLM workflow framework with visual builder  |  
### Vector Databases  
| Integration  | Description  | Docs  |  
| --- | --- | --- |  
| Staged hybrid search tracing with dense, sparse, RRF  |  
## Sandboxes
Run Phoenix [code evaluators](https://arize.com/docs/phoenix/evaluation/server-evals/code-evaluators) in hosted sandbox providers for kernel-level isolation and runtime dependency installation.  
| Integration  | Description  | Docs  |  
| --- | --- | --- |  
| Hosted micro-VM sandboxes for AI-generated code  |  
| Managed development sandboxes with snapshot startup  |  
| Ephemeral compute on Vercel's infrastructure  |  
| Serverless, Python-first container platform  |  
| Cloud sandboxes hosted by Docker  |  
## For Humans and Coding Agents
Phoenix is built to be driven by people and by AI coding agents alike. Three surfaces let agents (Claude Code, Codex, Cursor, and others) work with your traces, datasets, and experiments:
  * — `npx @arizeai/phoenix-cli` fetches traces, datasets, and experiments and instruments your app (`setup`), so an agent can pull context and act on it from the terminal.
  * — [`.agents/skills/`](https://github.com/Arize-ai/phoenix/blob/main/.agents/skills) packages workflows that teach agents how to debug, evaluate, and trace with Phoenix.
  * — connect any MCP client to your instance's `/mcp` endpoint to query Phoenix directly.


See the [coding agents documentation](https://arize.com/docs/phoenix/integrations/developer-tools/coding-agents) for setup and usage.  
| Skill  | Description  |  
| --- | --- |  
| Debug LLM applications using the Phoenix CLI — fetch traces, analyze errors, review experiments, and query the GraphQL API  |  
| Build and run evaluators for AI/LLM applications using Phoenix  |  
| OpenInference semantic conventions and instrumentation for tracing LLM applications  |  
## Security & Privacy
We take data security and privacy very seriously. For more details, see our [Security and Privacy documentation](https://arize.com/docs/phoenix/self-hosting/security/privacy).
### Telemetry
By default, Phoenix collects basic web analytics (e.g., page views, UI interactions) to help us understand how Phoenix is used and improve the product. **None of your trace data, evaluation results, or any sensitive information is ever collected.**
You can opt-out of telemetry by setting the environment variable: `PHOENIX_TELEMETRY_ENABLED=false`
## Community
Join our community to connect with thousands of AI builders.
  * 🌍 Join our [Slack community](https://join.slack.com/t/arize-ai/shared_invite/zt-3r07iavnk-ammtATWSlF0pSrd1DsMW7g).
  * 📚 Read our [documentation](https://arize.com/docs/phoenix).
  * 💡 Ask questions and provide feedback in the _#phoenix-support_ channel.
  * 🌟 Leave a star on our [GitHub](https://github.com/Arize-ai/phoenix).
  * 🐞 Report bugs with [GitHub Issues](https://github.com/Arize-ai/phoenix/issues).
  * 𝕏 Follow us on [𝕏](https://twitter.com/ArizePhoenix).
  * 💼 Follow us on [LinkedIn](https://www.linkedin.com/showcase/113218220).
  * 🗺️ Check out our [roadmap](https://github.com/orgs/Arize-ai/projects/45) to see where we're heading next.
  * 🧑‍🏫 Deep dive into everything [Agents](http://arize.com/ai-agents/) and [LLM Evaluations](https://arize.com/llm-evaluation) on Arize's Learning Hubs.


## Breaking Changes
See the [migration guide](https://github.com/Arize-ai/phoenix/blob/main/MIGRATION.md) for a list of breaking changes.
## Copyright, Patent, and License
Copyright 2025 Arize AI, Inc. All Rights Reserved.
Portions of this code are patent protected by one or more U.S. Patents. See the [IP_NOTICE](https://github.com/Arize-ai/phoenix/blob/main/IP_NOTICE).
This software is licensed under the terms of the Elastic License 2.0 (ELv2). See [LICENSE](https://github.com/Arize-ai/phoenix/blob/main/LICENSE).
## About
AI Observability & Evaluation
### Topics
[agents](https://github.com/topics/agents)[ai-monitoring](https://github.com/topics/ai-monitoring)[ai-observability](https://github.com/topics/ai-observability)[aiengineering](https://github.com/topics/aiengineering)[anthropic](https://github.com/topics/anthropic)[datasets](https://github.com/topics/datasets)[evals](https://github.com/topics/evals)[langchain](https://github.com/topics/langchain)[llamaindex](https://github.com/topics/llamaindex)[llm-eval](https://github.com/topics/llm-eval)[llm-evaluation](https://github.com/topics/llm-evaluation)[llmops](https://github.com/topics/llmops)[llms](https://github.com/topics/llms)[openai](https://github.com/topics/openai)[prompt-engineering](https://github.com/topics/prompt-engineering)[smolagents](https://github.com/topics/smolagents)
### Resources
### Code of conduct
### Contributing
### Security policy
### Stars
**11.8k** stars
### Watchers
**61** watching
### Forks
[**1.2k** forks](https://github.com/Arize-ai/phoenix/forks)
## 
[ @arizeai/phoenix-mcp@4.3.16Latest Oct 9, 2026 ](https://github.com/Arize-ai/phoenix/releases/tag/%40arizeai%2Fphoenix-mcp%404.3.16)
## 
## Languages
  * [Python48.8%](https://github.com/Arize-ai/phoenix/search?l=python)
  * [TypeScript42.8%](https://github.com/Arize-ai/phoenix/search?l=typescript)
  * [Jupyter Notebook7.1%](https://github.com/Arize-ai/phoenix/search?l=jupyter-notebook)
  * [C0.7%](https://github.com/Arize-ai/phoenix/search?l=c)
  * [Shell0.2%](https://github.com/Arize-ai/phoenix/search?l=shell)
  * [MDX0.1%](https://github.com/Arize-ai/phoenix/search?l=mdx)
  * [Other0.3%](https://github.com/Arize-ai/phoenix/search?l=Other)


You can’t perform that action at this time. 
