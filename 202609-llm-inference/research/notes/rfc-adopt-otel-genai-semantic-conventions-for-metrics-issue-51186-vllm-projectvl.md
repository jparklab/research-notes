---
title: '[RFC]: Adopt otel genai semantic conventions for metrics · Issue #51186 ·
  vllm-project/vllm'
id: rfc-adopt-otel-genai-semantic-conventions-for-metrics-issue-51186-vllm-projectvl
tags:
- internal-llm-inference-stacks-7f0c90
- locus-oss-boundary-sharpening
created: '2026-10-10T00:06:40.216536Z'
updated: '2026-10-10T14:52:17.351158Z'
source: https://github.com/vllm-project/vllm/issues/51186
source_domain: github.com
fetched_at: '2026-10-10T00:06:40.192494Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'vLLM RFC issue #51186 (''Adopt otel genai semantic conventions for metrics''),
  opened 2026-08-05. Proposes adding gen_ai.server.* histogram metrics (request.duration,
  time_per_output_token, time_to_first_token) matching OTel GenAI semantic conventions,
  requiring new attributes (gen_ai.operation.name, gen_ai.provider.name, error.type).
  Contributor Shaurya856 proposed a dual-emit migration (keep legacy vllm:* names,
  add gen_ai.* alongside). Maintainer markmc objected on 2026-09-14 on cardinality
  grounds: an operation label with only 3 possible values on 3 metrics still ''lays
  the groundwork for further expansion with little scrutiny,'' questioned what user
  need justifies a required attribute, and redirected interest toward generic OTLP
  export (issue #30252) instead of conforming Prometheus metric names. Confirmed via
  GitHub API: issue remains OPEN as of 2026-10-10 (state=open, updated_at=2026-09-14T11:45:11Z,
  4 comments, no activity since markmc''s objection). The implementing PR (#53858,
  dual-emit) was closed 2026-09-14 without merging, explicitly citing markmc''s objection
  and lack of maintainer appetite. The alternative PR it deferred to (#52249, operation-label-only)
  remains open and unmerged as of 2026-10-10, blocked on an unresolved rebase, with
  no further maintainer engagement. STATUS: CONFIRMS the gap description -- no reversal
  found; the objection stands and has, if anything, hardened (one implementation path
  explicitly abandoned).'
---

[Skip to content](https://github.com/vllm-project/vllm/issues/51186#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/51186) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/51186) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/51186) to refresh your session. Dismiss alert
#  [RFC]: Adopt otel genai semantic conventions for metrics #51186
Copy link
Copy link
[[RFC]: Adopt otel genai semantic conventions for metrics](https://github.com/vllm-project/vllm/issues/51186#top)#51186
Copy link
Labels
## Description
opened [on Aug 5, 2026](https://github.com/vllm-project/vllm/issues/51186#issue-5073879848)
Issue body actions
### Motivation.
Reduce vendor lock-in and enable dashboard portability by implementing standard metric and attribute names.
### Proposed Change.
Add the three model server histogram metrics currently in the [otel genai semantic conventions](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-metrics.md#generative-ai-model-server-metrics):
  * `gen_ai.server.request.duration` (similar to `vllm:e2e_request_latency_seconds`)
  * `gen_ai.server.time_per_output_token` (similar to `vllm:request_decode_time_seconds`)
  * `gen_ai.server.time_to_first_token` (similar to `vllm:time_to_first_token_seconds`)


vLLM will need to gather/propagate a few more required attributes, namely:
  * `gen_ai.operation.name` (derive from endpoint `/chat/completions` vs. `/completions`?)
  * `gen_ai.provider.name` ("vllm"?)
  * `error.type` (propagate to histograms)


A few other attributes are recommended and should be easy to implement:
  * `gen_ai.response.model`
  * `server.address`
  * `server.port`


### Feedback Period.
_No response_
### CC List.
_No response_
### Any Other Things.
Because the conventions are not yet considered stable, we should track changes in the conventions and look to prioritize any changes to required attributes.
### Before submitting a new issue...
  * Make sure you already searched for relevant issues, and asked the chatbot living at the bottom right corner of the [documentation page](https://docs.vllm.ai/en/latest/), which can answer lots of frequently asked questions.
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


## Activity
  1. [kahowell](https://github.com/kahowell)
added [on Aug 5, 2026](https://github.com/vllm-project/vllm/issues/51186#event-29026202507)
  2. ### Shaurya856 commented on Aug 6, 2026 
More actions
Happy to pick this up. Notes + a proposal:
**Scope** : The three latencies (e2e, ttft, time_per_output_token) are already computed from `req_state.stats` timestamps in `output_processor.py::do_tracing()`, and `PrometheusStatLogger` (`v1/metrics/loggers.py`) already has equivalent histograms under vLLM's own names (`vllm:e2e_request_latency_seconds`, etc). So this is mainly adding new `Histogram` objects under the `gen_ai.*` names using the existing pattern, not new computation.
**Migration** : To avoid breaking existing dashboards on `vllm:*` names, I'd propose dual-emitting both name sets initially, with `vllm:*` marked legacy later once `gen_ai.*` stabilizes. Similar to how OTel handles semconv transitions via `OTEL_SEMCONV_STABILITY_OPT_IN`. Wanted to confirm this matches intent before scoping a PR.
**Also noticed** : `tracing/utils.py`'s `SpanAttributes` already has `gen_ai.*` constants for spans (e.g. `GEN_AI_LATENCY_E2E = "gen_ai.latency.e2e"`), but these predate the real OTel GenAI semconv and don't match it (spec says `gen_ai.server.request.duration`). Should this stay scoped to Prometheus only, or would it make sense to align both in one pass?
For `gen_ai.operation.name` / `gen_ai.provider.name` / `error.type`, planning to derive `operation.name` from the request endpoint (chat vs completions). Let me know if there's a preferred place to hook that in.
  3. ### cemigo114 commented on Aug 7, 2026 
Contributor
More actions
[@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) ^
  4. ### Shaurya856 commented on Aug 12, 2026 
More actions
Following up in case this got buried. Happy to start on the dual-emit approach (proposal above) if there's no objection. Would rather get a quick nod on the migration/naming questions before opening a PR, but can also just go ahead with dual-emit as the safest default if that's easier than a full design discussion.
  5. ### markmc commented on Sep 14, 2026 
Member
More actions
See my comments on [#53858](https://github.com/vllm-project/vllm/pull/53858) and [#52249](https://github.com/vllm-project/vllm/pull/52249) which both propose adding an `operation` label
The OpenTelemetry Collector (and its Prometheus receiver) is commonly deployed to scrape our Prometheus-style /metrics and push metrics out via OTLP
The OpenTelemetry GenAI Semantic conventions for metrics are (a relatively new?) effort to standardize the metrics that get pushed out for OTLP. For vLLM, those would currently just be the three model server metrics (request duration, TTFT, TPOT)
In order for the collector/receiver to be able to translate those 3 vLLM metrics into ones that conform to the specification, we would need an "operation" label (chat vs text_completion vs embeddings) so that gen_ai.operation.name can be generated
One of the key challenges with metrics is to keep control of "cardinality explosion" - and so requests for new labels need particular scrutiny. Particularly labels that arguably apply to a large subset of our metrics. Yes, what you propose is a label with 3 possible values on only 3 metrics, but my concern is that lays the groundwork for further expansion with little scrutiny - more possible values, applied to many more metrics.
Given the cost of this label, what user need do we think gen_ai.operation.name actually solves for? I struggle to imagine it being sufficiently critical to justify it being a required attribute
Separately, I think OTLP export support ([#30252](https://github.com/vllm-project/vllm/issues/30252)) is much more interesting. If we implement this, perhaps we can have it also emit the metrics defined by the conventions (with the operation attribute, purely for conformance)
Finally - and I haven't checked this - we are committed to backwards compatibility so a metric like vllm:time_to_first_token_seconds would need to continue exposing the aggregate view when it is read without the label. AFAIR, this is not automatic, so we would need to add a by_operation variant of the metric


[Sign up for free](https://github.com/signup?return_to=https://github.com/vllm-project/vllm/issues/51186)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/vllm-project/vllm/issues/51186)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
No type
### Projects
No projects
### Milestone
No milestone
### Relationships
None yet
### Development
No branches or pull requests
### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
