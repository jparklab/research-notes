---
title: llm-d-router/docs at main · llm-d/llm-d-router · GitHub
id: llm-d-routerdocs-at-main-llm-dllm-d-router-github
tags:
- llmd-router-nonk8s
- docs
- source-hub
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:42:59.381442Z'
updated: '2026-09-15T04:55:23.666470Z'
source: https://github.com/llm-d/llm-d-router/tree/main/docs
source_domain: github.com
fetched_at: '2026-09-11T19:42:59.362342Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub file-tree view of llm-d/llm-d-router/docs (fetched Sep 11, 2026).
  The crawler only captured a partial file listing -- three top-level docs surfaced
  with working links: coordinator_architecture.md, coordinator_async_broker.md, and
  plugin-metric-protocol.md (GitHub''s JS file-tree component did not fully render
  for the crawler, so this is NOT a complete inventory of docs/; other docs referenced
  elsewhere, e.g. architecture.md, disaggregation.md, operations.md, exist per the
  main README but were not confirmed present at this exact path by this fetch). Commit
  history embedded in the page corroborates two other facts already captured from
  the repo README note: (1) a merged PR ''feat: implement EPP plugin stability lifecycle
  and --allow-experimental-plugins flag (#1912) (#2073)'', merged Aug 3 2026, is the
  actual commit introducing the Alpha/Beta/Stable plugin lifecycle described in the
  v0.9 blog; (2) the conditional-decode 412 gate and prefix-based-pd-decider plugin
  documentation live in docs/disaggregation.md per PR commit messages, confirming
  that file''s existence in the docs tree even though the crawl did not list it directly.
  Low standalone value; superseded by direct fetches of specific doc files in Phase
  2.'
utility_score: 14.0
---

[Skip to content](https://github.com/llm-d/llm-d-router/tree/main/docs#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/tree/main/docs) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/tree/main/docs) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/tree/main/docs) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/tree/main/docs).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# docs
/
Copy path
## Directory actions
## More options
More options
## Directory actions
## More options
More options
## Latest commit
[feat(metrics): add ATOM as a built-in engine (](https://github.com/llm-d/llm-d-router/commit/d242a2ec2e70ca4582da2d554f09344e87dd2f8d)[#2476](https://github.com/llm-d/llm-d-router/pull/2476)[)](https://github.com/llm-d/llm-d-router/commit/d242a2ec2e70ca4582da2d554f09344e87dd2f8d)
Open commit detailssuccess
Sep 11, 2026
[d242a2e](https://github.com/llm-d/llm-d-router/commit/d242a2ec2e70ca4582da2d554f09344e87dd2f8d) · Sep 11, 2026
## History
[History](https://github.com/llm-d/llm-d-router/commits/main/docs)
Open commit details
History
/
# docs
/
Copy path
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| 
### parent directory
 |  
|   |   | [Updated the main readme file to align with the epp code migration and…](https://github.com/llm-d/llm-d-router/commit/17cfd250c44ee61e8a6e1f0cf6e1b5b61870cdca "Updated the main readme file to align with the epp code migration and the terminology change to llm-d Router \(#978\)

Signed-off-by: ahg-g <ahg@google.com>")  | May 5, 2026  |  
|   |   |  [feat(epp): add --feature-gates flag (](https://github.com/llm-d/llm-d-router/commit/1711860cdcc02fb41a4cd2c9ac4641a8087f6715 "feat\(epp\): add --feature-gates flag \(#2388\)

Enabling an EPP feature gate previously required owning the entire
EndpointPickerConfig document, which the Helm chart splices into the
ConfigMap as a single string. Helm replaces strings rather than merging
them, so an overlay could not add only a gate and had to restate the
whole plugin list, causing guides to drift from the optimized baseline.

Thread flag-supplied gates through LoadRawConfig, appending them to
rawConfig.FeatureGates after the config file is read. Appending to the
struct rather than the returned map matters: InstantiateAndConfigure and
validateConfig each re-derive gates from rawConfig.FeatureGates.

Refs llm-d/llm-d-router#2358

Signed-off-by: abhishek karki <abhishekkarki202@gmail.com>")[#2388](https://github.com/llm-d/llm-d-router/pull/2388)[)](https://github.com/llm-d/llm-d-router/commit/1711860cdcc02fb41a4cd2c9ac4641a8087f6715 "feat\(epp\): add --feature-gates flag \(#2388\)

Enabling an EPP feature gate previously required owning the entire
EndpointPickerConfig document, which the Helm chart splices into the
ConfigMap as a single string. Helm replaces strings rather than merging
them, so an overlay could not add only a gate and had to restate the
whole plugin list, causing guides to drift from the optimized baseline.

Thread flag-supplied gates through LoadRawConfig, appending them to
rawConfig.FeatureGates after the config file is read. Appending to the
struct rather than the returned map matters: InstantiateAndConfigure and
validateConfig each re-derive gates from rawConfig.FeatureGates.

Refs llm-d/llm-d-router#2358

Signed-off-by: abhishek karki <abhishekkarki202@gmail.com>")  | Aug 25, 2026  |  
|   |   |   | Aug 19, 2026  |  
|   |   |  [feat(epp): move conditional-decode gate into prefix-based-pd-decider (](https://github.com/llm-d/llm-d-router/commit/63cb9d330a2c5937f80f8f51d119ec89afe85b9c "feat\(epp\): move conditional-decode gate into prefix-based-pd-decider \(#2335\)

* feat\(epp\): move conditional-decode 412 gate into prefix-based-pd-decider

The RFC 7240 Prefer: if-available cache-adequacy gate is expressed via the
existing prefix-based-pd-decider plugin instead of a hard-coded director
check. The gate becomes opt-in \(deployments without the plugin forward
speculative decode requests unconditionally\) and its threshold is expressed
in non-cached tokens, using the plugin's existing nonCachedTokens knob and
the unweighted cached-block count so RAM-cached prefixes are counted at
their full length.

The director's runner already aggregates PreRequest errors and preserves a
typed errcommon.Error{Code: PreconditionFailed} when it is the sole
failure, so the 412 response the coordinator restart step \(pkg/coordinator/
steps/decode_proxy\) sees is byte-for-byte identical to what the director
used to emit.

Closes #1686

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* feat\(epp\): honor promptTokens in conditional-decode 412 gate

Align the PreRequest gate with disaggregate\(\): a prompt shorter than
promptTokens skips remote prefill in both paths, so the operator's short-
prompt shortcut is not contradicted by a 412 that forces the coordinator
to run remote prefill anyway.

Collapse the 412-gate write-up into a single canonical explanation in
docs/disaggregation.md; communication.md and coordinator_architecture.md
become one-line pointers.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* refactor\(epp\): share decision logic between disaggregate and PreRequest

Extract needsRemotePrefill, the private helper that answers "does this
request's non-cached suffix meet NonCachedTokens?", and rewrite both
disaggregate and PreRequest as thin wrappers over it. The two callers
keep their distinct policies for read-failure — disaggregate logs and
falls back to no-disagg \(fail-soft, internal routing\); PreRequest maps
it to a 412 \(fail-closed, speculative-decode gate\).

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs: describe nonCachedTokens as a unified gate

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs: state conclusion in short-input log messages

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* refactor\(epp\): memoize needsRemotePrefill on the request

disaggregate\(\) and PreRequest\(\) ran the same computation twice per
conditional-decode request against the same decode endpoint. Cache the
outcome on the request via the plugin-local attribute store so PreRequest
reuses the decision disaggregate computed during scheduling.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs: split prefix-based-pd-decider into decider and gate roles

Introduce the plugin's two roles in the section intro and add a
gate-only EndpointPickerConfig example so the standalone usage is
discoverable next to the 412 gate description.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* fix\(epp\): fail conditional-decode gate closed on missing tokenization

A nil TokenizedPrompt on the request read as zero input tokens, which
tripped the short-input shortcut and silently forwarded the request.
Director swallows DataProducer errors, so a tokenizer-backend hiccup
disabled the whole gate in production. Treat missing tokenization as an
error in getUserInputLenInTokens: disaggregate still soft-fails to
decode-only via its existing Error-log branch, and PreRequest returns
412 via its existing typed-error branch.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* test\(epp\): build fresh requests per logical scheduling call

The prefix-based-pd-decider memoizes its decision on the request's
attribute store, so tests that reused one *InferenceRequest across
multiple table cases or sequence steps saw the first call's memo on
later calls. Rebuild the request per subtest and per series step to
match production, where each HTTP request is its own *InferenceRequest.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* fix\(epp\): soft-fail PreRequest to match disaggregate on gate errors

disaggregate and PreRequest share needsRemotePrefill but reacted to its
errors differently: disaggregate soft-failed \(decode-only\), PreRequest
returned 412 \(retry with prefill\). PreRequest now log-and-forwards on
gate-evaluation errors so the same input produces the same outcome on
both paths. Both callers log at Info; 412 stays for the legitimate gate
firing and the defensive nil-endpoint check.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* refactor\(epp\): simplify needsRemotePrefill nil-request handling

Return \(false, error\) early when request is nil so the cache read and
write no longer need their own guards. ReadRequestAttribute and
PutAttribute dereference the request internally, so a single up-front
check covers both call sites.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* feat\(epp\): declare prefix-based-pd-decider data dependencies

Standalone-mode deployments \(plugin declared for its PreRequest gate,
not just as a disagg-profile-handler decider\) previously produced a
silent per-request forward when required producers were absent. Declare
Consumes\(\) on the plugin so the framework's dependency graph resolves
PrefixCacheMatchInfo and TokenizedPrompt at init — either auto-creating
the default producers or surfacing ErrNoDefaultProducer at startup.
Runtime soft-fail behavior in disaggregate/PreRequest stays for the
now-rare transient cases. Docs' standalone-mode example updated to
include token-producer and to note the required-dependency validation.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* feat\(epp\): default-deny conditional-decode when no gate plugin claims it

Prefer: if-available requests without any gate plugin declared used to
silently forward. PrefixBasedPDDecider.PreRequest now marks the request
with routing.ConditionalDecodeHandledAttributeKey as soon as it sees the
header, and director.prepareRequest rejects any conditional-decode
request whose attribute stayed unset with 412. Missing gate plugins now
surface as the coordinator's cache-miss fallback rather than a silent
forward. Docs and new plugin/director tests cover the marked, unmarked,
and non-conditional shapes.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs\(epp\): fix stale "fails closed" comment on getUserInputLenInTokens

The comment described the original fail-closed fix, but a later change
aligned PreRequest with disaggregate on soft-fail. On a tokenizer
failure the gate now log-and-forwards, so the error return exists to
drive an Info-level "prompt not tokenized" diagnostic in the soft-fail
branch, not to fail the request closed.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* refactor\(epp\): log gate soft-fail via logger.Error to match repo convention

The soft-fail branches in disaggregate and PreRequest logged via
Info\(msg, "error", err.Error\(\)\). Repo convention \(57 sites\) and the
CLAUDE.md logging rules reserve logger.Error\(err, msg\) for recoverable
errors that carry an underlying err value — this is exactly that case.
Behavior is unchanged: disaggregate still returns false and PreRequest
still returns nil on error.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* fix\(epp\): move conditional-decode handled key into interface package

Rebase resolution had placed ConditionalDecodeHandledAttributeKey in
pkg/common/routing, which forced that package to import
pkg/epp/framework/interface/plugin. Sidecar images build with a subset
of the module that excludes framework/interface/plugin, so the sidecar
Docker build broke.

Move the DataKey to pkg/epp/framework/interface/requestcontrol — the
contract layer that already houses the PreRequest interface and other
extension points. Director and plugins already import it as fwkrc, so
neither side takes on a new dependency; the sidecar's build is
untouched. Also picks up the two gofmt fixups CI flagged.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs\(communication\): note director default-deny for conditional-decode 412

The Stage 3 conditional-decode paragraph described the plugin as the
only source of the 412 response. The sibling coordinator_architecture.md
was updated when the default-deny landed but this file was missed. Align
the wording so operators looking at either doc see the plugin-configured
gate first and the director-side default-deny as the fallback.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs\(disagg\): frame prefix-based-pd-decider roles by deployment topology

The "two roles" paragraph presented the decider and gate roles as
independent options. In practice each role maps to a specific topology:
sidecar-based deployments use disagg-profile-handler \(decider role, no
Prefer: if-available emitted so the gate is dormant\), and coordinator-
based deployments use the default profile handler with the plugin at
the top level \(gate role only\). Also warn that declaring two named
instances with different parameters is not supported because the
per-request memoization is keyed by plugin type.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs\(disagg\): drop internal "PreRequest phase" wording from fallback note

The surrounding sentence already tells the operator the director is the
one rejecting; the plugin-lifecycle stage is a director-internal detail
that doesn't earn its keep in operator-facing docs.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs\(disagg\): simplify standalone-mode intro paragraph

The lead-in above the standalone-mode YAML re-explained the roles that
the topology section just above already covers and buried an operator-
useful note about dependency validation behind two sentences of
mechanism detail. Replace it with a one-line intro and pull the
dependency-validation note out as its own sentence after the YAML.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs\(communication\): tighten conditional-decode 412 note

Replace the three-sentence explanation with the shorter one-sentence
form suggested during review. Same semantic content — plugin-configured
gate, default-deny fallback for deployments without a claiming gate —
without the extra mechanism/motivation prose.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* ci: retrigger e2e-router \(flaky network fetch\)

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

---------

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>")[#…](https://github.com/llm-d/llm-d-router/pull/2335)  | Aug 24, 2026  |  
| [coordinator_architecture.md](https://github.com/llm-d/llm-d-router/blob/main/docs/coordinator_architecture.md "coordinator_architecture.md")  | [coordinator_architecture.md](https://github.com/llm-d/llm-d-router/blob/main/docs/coordinator_architecture.md "coordinator_architecture.md")  | [feat(coordinator): async-broker step serving OpenAI compatible enqueu…](https://github.com/llm-d/llm-d-router/commit/2b105ffecb08a3afad801c6b4b26f47bbb5b1257 "feat\(coordinator\): async-broker step serving OpenAI compatible enqueue, wait, and passthrough modes \(#2325\)

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
| [coordinator_async_broker.md](https://github.com/llm-d/llm-d-router/blob/main/docs/coordinator_async_broker.md "coordinator_async_broker.md")  | [coordinator_async_broker.md](https://github.com/llm-d/llm-d-router/blob/main/docs/coordinator_async_broker.md "coordinator_async_broker.md")  | [feat(coordinator): async-broker step serving OpenAI compatible enqueu…](https://github.com/llm-d/llm-d-router/commit/2b105ffecb08a3afad801c6b4b26f47bbb5b1257 "feat\(coordinator\): async-broker step serving OpenAI compatible enqueue, wait, and passthrough modes \(#2325\)

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
|   |   | [feat: implement EPP plugin stability lifecycle and --allow-experiment…](https://github.com/llm-d/llm-d-router/commit/8791c008bb7ae6fc7bdee0bba69e7de96eb4c13c "feat: implement EPP plugin stability lifecycle and --allow-experimental-plugins flag \(#1912\) \(#2073\)

* feat: implement EPP plugin stability lifecycle and experimentalPlugins feature gate \(#1912\)

Signed-off-by: Cong Liu <conliu@google.com>

* mark session affinity plugins alpha - they have not been used and are under major refactoring
Signed-off-by: Cong Liu <conliu@google.com>

* fix: restore sessionaffinity filter and scorer to StabilityBeta
Signed-off-by: Cong Liu <conliu@google.com>

* fix: set sessionaffinity filter and scorer to StabilityAlpha and enable experimentalPlugins in test
Signed-off-by: Cong Liu <conliu@google.com>

* Merge origin/main into plugin-stability-lifecycle
Signed-off-by: Cong Liu <conliu@google.com>

* test: enable experimentalPlugins featureGate for test configs using Alpha plugins
Signed-off-by: Cong Liu <conliu@google.com>

* feat: replace experimentalPlugins feature gate with --allow-experimental-plugins CLI flag
Signed-off-by: Cong Liu <conliu@google.com>

* feat: enforce experimental plugin check on auto-created system defaults and data producers
Signed-off-by: Cong Liu <conliu@google.com>

* test: remove experimentalPlugins feature gate reference from coordinator configs_test.go
Signed-off-by: Cong Liu <conliu@google.com>

* refactor: validate plugin stability once after all plugin initialization
Signed-off-by: Cong Liu <conliu@google.com>

* fix: remove obsolete activerequest filter package from branch \(replaced by utilization filter in upstream #2218\)
Signed-off-by: Cong Liu <conliu@google.com>

* refactor\(runner\): move plugin stability validation out of configuration parsing to run after all phases complete
Signed-off-by: Cong Liu <conliu@google.com>

* refactor\(runner\): remove unused opts parameter from parseConfigurationPhaseTwo
Signed-off-by: Cong Liu <conliu@google.com>

* docs & test: address PR #2073 review feedback
Signed-off-by: Cong Liu <conliu@google.com>

---------

Signed-off-by: Cong Liu <conliu@google.com>")  | Aug 3, 2026  |  
|   |   |  [feat(sidecar): add TLS security profile flags (](https://github.com/llm-d/llm-d-router/commit/def1fd246e578aded64467256b33902f8b529da0 "feat\(sidecar\): add TLS security profile flags \(#2676\)

* feat\(sidecar\): add TLS security profile flags

Allow operators to configure the minimum TLS version and cipher suites used by the pd-sidecar secure proxy while preserving its existing defaults.

Signed-off-by: Andres Llausas <allausas@redhat.com>

* docs\(sidecar\): document TLS profile flags

Signed-off-by: Andres Llausas <allausas@redhat.com>

---------

Signed-off-by: Andres Llausas <allausas@redhat.com>")[#2676](https://github.com/llm-d/llm-d-router/pull/2676)[)](https://github.com/llm-d/llm-d-router/commit/def1fd246e578aded64467256b33902f8b529da0 "feat\(sidecar\): add TLS security profile flags \(#2676\)

* feat\(sidecar\): add TLS security profile flags

Allow operators to configure the minimum TLS version and cipher suites used by the pd-sidecar secure proxy while preserving its existing defaults.

Signed-off-by: Andres Llausas <allausas@redhat.com>

* docs\(sidecar\): document TLS profile flags

Signed-off-by: Andres Llausas <allausas@redhat.com>

---------

Signed-off-by: Andres Llausas <allausas@redhat.com>")  | Sep 10, 2026  |  
|   |   | [docs: document that endpoint discovery honours custom pod readinessGa…](https://github.com/llm-d/llm-d-router/commit/aab0c190ce9e18ee6a5587589be1b881bc7da87c "docs: document that endpoint discovery honours custom pod readinessGates \(#2604\)

* docs: document that endpoint discovery honours pod readiness gates

The EPP admits a pod into the routing pool only when the pod's aggregate Ready
condition is True. The kubelet computes Ready as ContainersReady AND every
condition named in spec.readinessGates, so custom readiness gates already gate
routing fail-closed, with no EPP configuration required. That was not written
down anywhere, so it was not discoverable.

Document the contract in docs/discovery.md, including the point that a gated pod
still shows 1/1 in kubectl get pods because that column counts ready containers,
not the Ready condition.

Add IsPodReady cases for a satisfied and an unsatisfied readiness gate so the
behaviour is pinned. The unsatisfied case fails if IsPodReady is ever switched
from PodReady to ContainersReady.

Signed-off-by: Almog Tavor <almogtavor@gmail.com>

* docs: trim explanatory comments in readiness-gate docs and test

Drop the inline kubelet explanation from the test case and condense the
closing paragraph in docs/discovery.md, keeping the 1/1 vs Ready-condition
note that is the non-obvious part.

Signed-off-by: Almog Tavor <almogtavor@gmail.com>

---------

Signed-off-by: Almog Tavor <almogtavor@gmail.com>")  | Sep 2, 2026  |  
|   |   |  [docs: add coordinator metrics specification (](https://github.com/llm-d/llm-d-router/commit/d8d0987ff5ee87610531154ff61c1db34afc46df "docs: add coordinator metrics specification \(#2277\)

* Add docs/metrics.coord.md.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Remove phase references.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Review version.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Additional changes.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Fix wording.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Simplify

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Simplify metrics.coord.md explanations.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review commenst.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* More changes after comments.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Coord metrics documentation updates \(#74\)

* docs: update coordinator metrics spec based on comments in PR #2277

Bring docs/metrics.coord.md into line with the coordinator metrics
package landing in #2435 \(based on comments in PR #2277\).
Drops response_size_bytes, adds request_input_tokens to the Request family, renames
disagg_decision_total -> execution_path_total \(label decision_type ->
path\), extends conditional_decode_probes_total.result with the error
value the code now emits, and rewrites the error-classes note now that
decode and conditional-decode capture upstream status through the
reverse proxy and return UpstreamStreamedError on 4xx/5xx or transport
failures.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs: address remaining review comments on coordinator metrics

Add bucket ranges next to each histogram row. Note that the step
label uses the step's registered name. Update the Cardinality
section to cover every metric with a model_name label and point at
the shared helper used by both the coordinator and the EPP.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs: keep coordinator metrics doc standalone until #2435 lands

Drop the relative link to pkg/common/observability/metrics/cardinality.go
in the Cardinality section, since that file only exists on the
coord-metrics-impl branch \(#2435\). Keep the path in prose so the doc
still points to the shared bounded-label helper without producing a
broken link on branches that do not carry the implementation.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

---------

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>
Signed-off-by: Revital Sur <eres@il.ibm.com>

* Restore relative links in the Cardinality section

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Correct metrics doc and comments against code

Signed-off-by: Revital Sur <eres@il.ibm.com>

---------

Signed-off-by: Revital Sur <eres@il.ibm.com>
Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>
Co-authored-by: dmitripikus <46105577+dmitripikus@users.noreply.github.com>")[#2277](https://github.com/llm-d/llm-d-router/pull/2277)[)](https://github.com/llm-d/llm-d-router/commit/d8d0987ff5ee87610531154ff61c1db34afc46df "docs: add coordinator metrics specification \(#2277\)

* Add docs/metrics.coord.md.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Remove phase references.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Review version.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Additional changes.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Fix wording.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Simplify

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Simplify metrics.coord.md explanations.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review commenst.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Address review comments.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* More changes after comments.

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Coord metrics documentation updates \(#74\)

* docs: update coordinator metrics spec based on comments in PR #2277

Bring docs/metrics.coord.md into line with the coordinator metrics
package landing in #2435 \(based on comments in PR #2277\).
Drops response_size_bytes, adds request_input_tokens to the Request family, renames
disagg_decision_total -> execution_path_total \(label decision_type ->
path\), extends conditional_decode_probes_total.result with the error
value the code now emits, and rewrites the error-classes note now that
decode and conditional-decode capture upstream status through the
reverse proxy and return UpstreamStreamedError on 4xx/5xx or transport
failures.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs: address remaining review comments on coordinator metrics

Add bucket ranges next to each histogram row. Note that the step
label uses the step's registered name. Update the Cardinality
section to cover every metric with a model_name label and point at
the shared helper used by both the coordinator and the EPP.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs: keep coordinator metrics doc standalone until #2435 lands

Drop the relative link to pkg/common/observability/metrics/cardinality.go
in the Cardinality section, since that file only exists on the
coord-metrics-impl branch \(#2435\). Keep the path in prose so the doc
still points to the shared bounded-label helper without producing a
broken link on branches that do not carry the implementation.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

---------

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>
Signed-off-by: Revital Sur <eres@il.ibm.com>

* Restore relative links in the Cardinality section

Signed-off-by: Revital Sur <eres@il.ibm.com>

* Correct metrics doc and comments against code

Signed-off-by: Revital Sur <eres@il.ibm.com>

---------

Signed-off-by: Revital Sur <eres@il.ibm.com>
Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>
Co-authored-by: dmitripikus <46105577+dmitripikus@users.noreply.github.com>")  | Sep 8, 2026  |  
|   |   |  [metrics: remove core legacy metric families (](https://github.com/llm-d/llm-d-router/commit/f9fa604cb62c414dbbe2eb7c69a2995736ef3bed "metrics: remove core legacy metric families \(#2675\)

* cleanup\(metrics\): remove core legacy metric families

Signed-off-by: Jared Tan <jian.tan@daocloud.io>

* cleanup\(metrics\): remove empty collector declaration

Signed-off-by: Jared Tan <jian.tan@daocloud.io>

* test\(metrics\): use canonical average metric names

Signed-off-by: Jared Tan <jian.tan@daocloud.io>

* test\(metrics\): fix canonical metric assertions

Use the Prometheus test helper for canonical llm-d router metric names because the Kubernetes wrapper applies a naming lint that rejects the `epp` abbreviation. Also expect the request metric’s default fairness ID.

Signed-off-by: Jared Tan <jian.tan@daocloud.io>

---------

Signed-off-by: Jared Tan <jian.tan@daocloud.io>")[#2675](https://github.com/llm-d/llm-d-router/pull/2675)[)](https://github.com/llm-d/llm-d-router/commit/f9fa604cb62c414dbbe2eb7c69a2995736ef3bed "metrics: remove core legacy metric families \(#2675\)

* cleanup\(metrics\): remove core legacy metric families

Signed-off-by: Jared Tan <jian.tan@daocloud.io>

* cleanup\(metrics\): remove empty collector declaration

Signed-off-by: Jared Tan <jian.tan@daocloud.io>

* test\(metrics\): use canonical average metric names

Signed-off-by: Jared Tan <jian.tan@daocloud.io>

* test\(metrics\): fix canonical metric assertions

Use the Prometheus test helper for canonical llm-d router metric names because the Kubernetes wrapper applies a naming lint that rejects the `epp` abbreviation. Also expect the request metric’s default fairness ID.

Signed-off-by: Jared Tan <jian.tan@daocloud.io>

---------

Signed-off-by: Jared Tan <jian.tan@daocloud.io>")  | Sep 9, 2026  |  
|   |   |  [docs: describe standalone service proxy mode (](https://github.com/llm-d/llm-d-router/commit/01b983ad732abb052d2ae97755fafa72ac4d9de7 "docs: describe standalone service proxy mode \(#2748\)

Signed-off-by: Sean Westfall <sean.westfall@gmail.com>")[#2748](https://github.com/llm-d/llm-d-router/pull/2748)[)](https://github.com/llm-d/llm-d-router/commit/01b983ad732abb052d2ae97755fafa72ac4d9de7 "docs: describe standalone service proxy mode \(#2748\)

Signed-off-by: Sean Westfall <sean.westfall@gmail.com>")  | Sep 9, 2026  |  
|   |   | [config: consolidate discovery plugins under a single discovery sectio…](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19 "config: consolidate discovery plugins under a single discovery section \(follow up #2341\) \(#2434\)

* config: consolidate discovery plugins under a single discovery section

Group endpoint discovery and peer discovery under dataLayer.discovery
with endpoints and peers sub-fields instead of two separate top-level
fields \(dataLayer.discovery and dataLayer.peerDiscovery\).

A migration shim in the config loader preserves backward compatibility:
- dataLayer.discovery.pluginRef migrates to discovery.endpoints.pluginRef
- dataLayer.peerDiscovery migrates to discovery.peers

Follows the existing deprecation pattern used for parser and
saturationDetector fields.

Signed-off-by: Stefano Pascolutti <spascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>

* config: remove pre-release peerDiscovery deprecation shim

The dataLayer.peerDiscovery field was introduced in #2341 but never
shipped in a release, so no users depend on it. Remove the field, its
migration shim, and its test instead of carrying unnecessary deprecation
machinery. The endpoint discovery pluginRef migration \(which IS released\)
is preserved.

Signed-off-by: Stefano Pascolutti <stefano.pascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>

* address review feedback on discovery config migration

Add "If both are set, the new field is used." to the discovery
pluginRef deprecation message, matching the saturationDetector and
parser migration patterns. Extract a local variable in runner.go
to reduce repetition of the DataLayer field path.

Signed-off-by: Stefano Pascolutti <spascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>

---------

Signed-off-by: Stefano Pascolutti <spascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>
Signed-off-by: Stefano Pascolutti <stefano.pascolutti@gmail.com>")  | Aug 25, 2026  |  
| [plugin-metric-protocol.md](https://github.com/llm-d/llm-d-router/blob/main/docs/plugin-metric-protocol.md "plugin-metric-protocol.md")  | [plugin-metric-protocol.md](https://github.com/llm-d/llm-d-router/blob/main/docs/plugin-metric-protocol.md "plugin-metric-protocol.md")  |  [feat(metrics): add ATOM as a built-in engine (](https://github.com/llm-d/llm-d-router/commit/d242a2ec2e70ca4582da2d554f09344e87dd2f8d "feat\(metrics\): add ATOM as a built-in engine \(#2476\)

ATOM is an LLM inference engine for AMD GPUs built on AITER kernels. It
exposes the load signals this extractor needs -- queue depth, running
count, KV-cache utilization, and cache geometry -- under an `atom:` prefix,
following the same convention as `sglang:` and `trtllm_`.

Registering it here means a deployment only has to label its pods with
`llm-d.ai/engine-type: atom`, rather than carrying an engineConfigs block
that restates metric names the engine already fixes.

Verified against a live ATOM deployment on 8xMI355X: two GLM-5.2-MXFP4 TP4
instances behind a stock endpoint-picker image, scored by queue-scorer,
kv-cache-utilization-scorer, prefix-cache-scorer and no-hit-lru-scorer.
A full 1319-sample GSM8K run split 651/668 across the two engines with no
scheduling errors. That run predates the spec strings below: ATOM ships
these counters as `atom:requests_waiting`, `atom:requests_running` and
`atom:kv_cache_usage_ratio`, so the entry names those. Only the four
string literals differ from what the run exercised; the extraction path
is unchanged.

Signed-off-by: Jasen2201 <yajizhan@amd.com>")[#2476](https://github.com/llm-d/llm-d-router/pull/2476)[)](https://github.com/llm-d/llm-d-router/commit/d242a2ec2e70ca4582da2d554f09344e87dd2f8d "feat\(metrics\): add ATOM as a built-in engine \(#2476\)

ATOM is an LLM inference engine for AMD GPUs built on AITER kernels. It
exposes the load signals this extractor needs -- queue depth, running
count, KV-cache utilization, and cache geometry -- under an `atom:` prefix,
following the same convention as `sglang:` and `trtllm_`.

Registering it here means a deployment only has to label its pods with
`llm-d.ai/engine-type: atom`, rather than carrying an engineConfigs block
that restates metric names the engine already fixes.

Verified against a live ATOM deployment on 8xMI355X: two GLM-5.2-MXFP4 TP4
instances behind a stock endpoint-picker image, scored by queue-scorer,
kv-cache-utilization-scorer, prefix-cache-scorer and no-hit-lru-scorer.
A full 1319-sample GSM8K run split 651/668 across the two engines with no
scheduling errors. That run predates the spec strings below: ATOM ships
these counters as `atom:requests_waiting`, `atom:requests_running` and
`atom:kv_cache_usage_ratio`, so the entry names those. Only the four
string literals differ from what the run exercised; the extraction path
is unchanged.

Signed-off-by: Jasen2201 <yajizhan@amd.com>")  | Sep 11, 2026  |  
|   |   |   | Jun 10, 2026  |  
|   |   | [feat: implement EPP plugin stability lifecycle and --allow-experiment…](https://github.com/llm-d/llm-d-router/commit/8791c008bb7ae6fc7bdee0bba69e7de96eb4c13c "feat: implement EPP plugin stability lifecycle and --allow-experimental-plugins flag \(#1912\) \(#2073\)

* feat: implement EPP plugin stability lifecycle and experimentalPlugins feature gate \(#1912\)

Signed-off-by: Cong Liu <conliu@google.com>

* mark session affinity plugins alpha - they have not been used and are under major refactoring
Signed-off-by: Cong Liu <conliu@google.com>

* fix: restore sessionaffinity filter and scorer to StabilityBeta
Signed-off-by: Cong Liu <conliu@google.com>

* fix: set sessionaffinity filter and scorer to StabilityAlpha and enable experimentalPlugins in test
Signed-off-by: Cong Liu <conliu@google.com>

* Merge origin/main into plugin-stability-lifecycle
Signed-off-by: Cong Liu <conliu@google.com>

* test: enable experimentalPlugins featureGate for test configs using Alpha plugins
Signed-off-by: Cong Liu <conliu@google.com>

* feat: replace experimentalPlugins feature gate with --allow-experimental-plugins CLI flag
Signed-off-by: Cong Liu <conliu@google.com>

* feat: enforce experimental plugin check on auto-created system defaults and data producers
Signed-off-by: Cong Liu <conliu@google.com>

* test: remove experimentalPlugins feature gate reference from coordinator configs_test.go
Signed-off-by: Cong Liu <conliu@google.com>

* refactor: validate plugin stability once after all plugin initialization
Signed-off-by: Cong Liu <conliu@google.com>

* fix: remove obsolete activerequest filter package from branch \(replaced by utilization filter in upstream #2218\)
Signed-off-by: Cong Liu <conliu@google.com>

* refactor\(runner\): move plugin stability validation out of configuration parsing to run after all phases complete
Signed-off-by: Cong Liu <conliu@google.com>

* refactor\(runner\): remove unused opts parameter from parseConfigurationPhaseTwo
Signed-off-by: Cong Liu <conliu@google.com>

* docs & test: address PR #2073 review feedback
Signed-off-by: Cong Liu <conliu@google.com>

---------

Signed-off-by: Cong Liu <conliu@google.com>")  | Aug 3, 2026  |  
| View all files |  
You can’t perform that action at this time. 
