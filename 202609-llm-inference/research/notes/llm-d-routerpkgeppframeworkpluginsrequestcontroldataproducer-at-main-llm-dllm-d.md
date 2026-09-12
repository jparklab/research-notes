---
title: llm-d-router/pkg/epp/framework/plugins/requestcontrol/dataproducer at main
  · llm-d/llm-d-router · GitHub
id: llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducer-at-main-llm-dllm-d
tags:
- llmd-router-nonk8s-backends-54b774
- locus-kv-event-path-off-k8s
- precise-prefix-cache-producer
- go-source
created: '2026-09-11T21:22:58.683444Z'
updated: '2026-09-12T03:37:37.754988Z'
source: https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/requestcontrol/dataproducer
source_domain: github.com
fetched_at: '2026-09-11T21:22:58.663173Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: GitHub directory listing + README for llm-d-router's pkg/epp/framework/plugins/requestcontrol/dataproducer
  package. Lists all DataProducer plugins run in the PrepareRequestData phase before
  scorers/filters, each implementing requestcontrol.DataProducer. Table of producers
  includes precise-prefix-cache-producer (produces PrefixCacheMatchInfo by 'Maintains
  a precise KV-block index by subscribing to vLLM KV-events; requires token-producer
  upstream') alongside approx-prefix-cache-producer, burst-prefix-cache-producer,
  inflight-load-producer, predicted-latency-producer, session-id-producer, mm-embeddings-cache-producer,
  and p2p-source-producer. Confirms plugin ordering is resolved via a DAG over each
  plugin's Produces/Consumes declarations.
---

[Skip to content](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/requestcontrol/dataproducer#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/requestcontrol/dataproducer) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/requestcontrol/dataproducer) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/requestcontrol/dataproducer) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/requestcontrol/dataproducer).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# dataproducer
/
Copy path
## Directory actions
## More options
More options
## Directory actions
## More options
More options
## Latest commit
[feature(epp): authenticate token-producer warmup probe via VLLM_API_K…](https://github.com/llm-d/llm-d-router/commit/805c2ed0303f3bcb120e45041e8ec283710b2e7e)
Open commit detailsfailure
Sep 11, 2026
[805c2ed](https://github.com/llm-d/llm-d-router/commit/805c2ed0303f3bcb120e45041e8ec283710b2e7e) · Sep 11, 2026
## History
[History](https://github.com/llm-d/llm-d-router/commits/main/pkg/epp/framework/plugins/requestcontrol/dataproducer)
Open commit details
History
/
# dataproducer
/
Copy path
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| 
### parent directory
 |  
|   |   |  [approximateprefix: change block hash insertion order to tail-first (](https://github.com/llm-d/llm-d-router/commit/033986e9d1f4b6fe5c6ad80bd2a6329a9dc7636f "approximateprefix: change block hash insertion order to tail-first \(#2661\)

Signed-off-by: Iceber Gu <caiwei95@hotmail.com>")[#…](https://github.com/llm-d/llm-d-router/pull/2661)  | Sep 4, 2026  |  
|   |   |  [fix: Make TokenizedPrompt.MultiModalFeatures per-prompt (](https://github.com/llm-d/llm-d-router/commit/229b8ea5f56a99c732b02506cbd733cbc3d13cea "fix: Make TokenizedPrompt.MultiModalFeatures per-prompt \(#1633\)

* fix: Make MultiModalFeatures per-prompt in TokenizedPrompt

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Bundle per-prompt tokens and MM features into PromptTokens struct

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename missing instances

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename missing instances

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: update stale TokenizedPrompt reference missed during rebase

The auto-merge left one test using the old TokenizedPrompt type and
PerPromptTokens field in producer_test.go. Updated to use
TokenizedRequest with Prompts/PromptTokens.

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename stale entries

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename stale entries

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename stale entries

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

---------

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>")[#1633](https://github.com/llm-d/llm-d-router/pull/1633)[)](https://github.com/llm-d/llm-d-router/commit/229b8ea5f56a99c732b02506cbd733cbc3d13cea "fix: Make TokenizedPrompt.MultiModalFeatures per-prompt \(#1633\)

* fix: Make MultiModalFeatures per-prompt in TokenizedPrompt

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Bundle per-prompt tokens and MM features into PromptTokens struct

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename missing instances

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename missing instances

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: update stale TokenizedPrompt reference missed during rebase

The auto-merge left one test using the old TokenizedPrompt type and
PerPromptTokens field in producer_test.go. Updated to use
TokenizedRequest with Prompts/PromptTokens.

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename stale entries

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename stale entries

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename stale entries

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

---------

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>")  | Aug 20, 2026  |  
|   |   |  [feat(datalayer): typed attribute slots and value-safety registry (](https://github.com/llm-d/llm-d-router/commit/6592a6fa3dff1f09b54dc85743b9dd036653829e "feat\(datalayer\): typed attribute slots and value-safety registry \(#2352\)

* feat\(datalayer\): typed attribute slots and value-safety registry

Producers writing to their declared key cannot put a value of the wrong
type: Slot\[T\] pins the value type at compile time and validates at
write. A central attribute registry built from ProducerPlugin.Produces\(\)
validates at startup that every consumer's Consumes\(\) declaration
references a key the framework knows and names the same value type the
producer declared. This is the value-safety complement to PR 2190's
key-safety check.

Drops the reflect.ValueOf\(v\).Kind\(\) == reflect.String workaround in
session affinity now that the consumer can read via a typed accessor.

Position: complement to PR 2190 \(key safety -> value safety\).

Tracking issue: #2338

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* fix\(datalayer\): don't fail startup on an unproduced Optional data key

ValidateConsumer treated Optional consumer dependencies the same as
Required ones, erroring when no producer existed for the key. This
contradicted CreateMissingDataProducers, which already tolerates a
missing Optional producer and only warns. Any config relying on that
tolerance \(e.g. inflight-load-producer's optional prefix-cache-match
dependency with no prefix-cache producer configured\) failed to start.

Skip the existence check for Optional keys with no producer; keep the
type check for Optional keys that are produced.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* Fix EndpointAttributeFilter.Consumes\(\) key mismatch and session_id polymorphic read

Consumes\(\) wrapped an already-qualified attribute string in a fresh
DataKey with an empty producer name, so the declared key never matched
the DCGM extractor's registered key. Split on the first "/" to recover
the producer name.

attributeString was tightened to a strict typed assertion, silently
rejecting the agent-identity plugin's plain-string attribute. Revert
to the reflect-based "any string-kind value" read, which is required
because the consumer reads values from unrelated, uncoordinated
producers whose value type it cannot pin at compile time.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

---------

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")[#2352](https://github.com/llm-d/llm-d-router/pull/2352)[)](https://github.com/llm-d/llm-d-router/commit/6592a6fa3dff1f09b54dc85743b9dd036653829e "feat\(datalayer\): typed attribute slots and value-safety registry \(#2352\)

* feat\(datalayer\): typed attribute slots and value-safety registry

Producers writing to their declared key cannot put a value of the wrong
type: Slot\[T\] pins the value type at compile time and validates at
write. A central attribute registry built from ProducerPlugin.Produces\(\)
validates at startup that every consumer's Consumes\(\) declaration
references a key the framework knows and names the same value type the
producer declared. This is the value-safety complement to PR 2190's
key-safety check.

Drops the reflect.ValueOf\(v\).Kind\(\) == reflect.String workaround in
session affinity now that the consumer can read via a typed accessor.

Position: complement to PR 2190 \(key safety -> value safety\).

Tracking issue: #2338

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* fix\(datalayer\): don't fail startup on an unproduced Optional data key

ValidateConsumer treated Optional consumer dependencies the same as
Required ones, erroring when no producer existed for the key. This
contradicted CreateMissingDataProducers, which already tolerates a
missing Optional producer and only warns. Any config relying on that
tolerance \(e.g. inflight-load-producer's optional prefix-cache-match
dependency with no prefix-cache producer configured\) failed to start.

Skip the existence check for Optional keys with no producer; keep the
type check for Optional keys that are produced.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* Fix EndpointAttributeFilter.Consumes\(\) key mismatch and session_id polymorphic read

Consumes\(\) wrapped an already-qualified attribute string in a fresh
DataKey with an empty producer name, so the declared key never matched
the DCGM extractor's registered key. Split on the first "/" to recover
the producer name.

attributeString was tightened to a strict typed assertion, silently
rejecting the agent-identity plugin's plain-string attribute. Revert
to the reflect-based "any string-kind value" read, which is required
because the consumer reads values from unrelated, uncoordinated
producers whose value type it cannot pin at compile time.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

---------

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")  | Sep 11, 2026  |  
|   |   |  [feat(epp): add latency-observation scorer and producer (](https://github.com/llm-d/llm-d-router/commit/7244e8bd64970eb91f2e620b9b7fec9e4b6abb99 "feat\(epp\): add latency-observation scorer and producer \(#2371\)

* Addin ttft-aware-scorer.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Update comments.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Move unneeded files.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Add draft issue.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Add Readmes.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix review comments.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Add notes.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Update tests.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Use polling dispatcher and rename.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Remove issue file.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix comments.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix test.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Remove conf file.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix lint.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Declare producer keys.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Add nil checks.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix review comments.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix comments and add missing tests.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Change plugin name.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Align docs with AGENTS.md style.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

---------

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>
Signed-off-by: Mohammad-nassar10 <79787844+Mohammad-nassar10@users.noreply.github.com>")[#2371](https://github.com/llm-d/llm-d-router/pull/2371)[)](https://github.com/llm-d/llm-d-router/commit/7244e8bd64970eb91f2e620b9b7fec9e4b6abb99 "feat\(epp\): add latency-observation scorer and producer \(#2371\)

* Addin ttft-aware-scorer.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Update comments.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Move unneeded files.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Add draft issue.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Add Readmes.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix review comments.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Add notes.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Update tests.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Use polling dispatcher and rename.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Remove issue file.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix comments.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix test.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Remove conf file.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix lint.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Declare producer keys.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Add nil checks.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix review comments.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix comments and add missing tests.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Change plugin name.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Align docs with AGENTS.md style.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

---------

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>
Signed-off-by: Mohammad-nassar10 <79787844+Mohammad-nassar10@users.noreply.github.com>")  | Sep 8, 2026  |  
|   |   | [feat(epp): add atomic cross-replica coordination API [Safe DisaggSet …](https://github.com/llm-d/llm-d-router/commit/ab72b2298acff69dce770a265c5394c5de560e53 "feat\(epp\): add atomic cross-replica coordination API \[Safe DisaggSet 1/4\] \(#2570\)

* feat\(epp\): add atomic cross-replica coordination API

Signed-off-by: Mathis Felardos <mathis@mistral.ai>

* docs\(epp\): clarify cross-replica plugin roles

Signed-off-by: Mathis Felardos <mathis@mistral.ai>

* refactor\(epp\): remove generic local syncer

Signed-off-by: Mathis Felardos <mathis@mistral.ai>

* refactor\(epp\): expose cross-replica syncer through handle

Let plugins discover the configured syncer through their framework handle instead of adding a consumer interface and scanning every plugin after construction.

Signed-off-by: Mathis Felardos <mathis@mistral.ai>

---------

Signed-off-by: Mathis Felardos <mathis@mistral.ai>")  | Sep 1, 2026  |  
|   |   |  [docs: use blockSizeTokens in tokenProcessorConfig samples (](https://github.com/llm-d/llm-d-router/commit/bffcfb783baacbc827b138e7b976c6b40e66b2af "docs: use blockSizeTokens in tokenProcessorConfig samples \(#2705\)

The sample EPP configurations and plugin READMEs set the deprecated
blockSize key inside tokenProcessorConfig. TokenProcessorConfig resolves
blockSizeTokens first and promotes blockSize only as a fallback, so the
samples teach a key that is on its way out.

Rename the key in the four deploy/config samples and the three READMEs
that carry it, keeping every value. Nothing in Go reads deploy/config, so
these files have no compile-time or test-time guard against drift.

The deprecation shims in kvblock.TokenProcessorConfig and the
approximate-prefix producer stay, along with the approximate-prefix
README that documents the deprecated field.

Fixes #641
Fixes #747

Signed-off-by: ChethanUK <chethanuk@outlook.com>")[#2705](https://github.com/llm-d/llm-d-router/pull/2705)[)](https://github.com/llm-d/llm-d-router/commit/bffcfb783baacbc827b138e7b976c6b40e66b2af "docs: use blockSizeTokens in tokenProcessorConfig samples \(#2705\)

The sample EPP configurations and plugin READMEs set the deprecated
blockSize key inside tokenProcessorConfig. TokenProcessorConfig resolves
blockSizeTokens first and promotes blockSize only as a fallback, so the
samples teach a key that is on its way out.

Rename the key in the four deploy/config samples and the three READMEs
that carry it, keeping every value. Nothing in Go reads deploy/config, so
these files have no compile-time or test-time guard against drift.

The deprecation shims in kvblock.TokenProcessorConfig and the
approximate-prefix producer stay, along with the approximate-prefix
README that documents the deprecated field.

Fixes #641
Fixes #747

Signed-off-by: ChethanUK <chethanuk@outlook.com>")  | Sep 5, 2026  |  
|   |   |  [telemetry: migrate remaining llm_d and gen_ai attributes to semconv (](https://github.com/llm-d/llm-d-router/commit/db208e3d5cb5975c88a784dfd56b1977829d828a "telemetry: migrate remaining llm_d and gen_ai attributes to semconv \(#2516\)

Complete migration of all remaining llm_d.* and gen_ai.* OpenTelemetry
attributes across llm-d-router to centralized semantic conventions in
pkg/common/observability/semconv:
- Expand semconv/llm_d.go with typed keys and constructors for all EPP
  scheduling/filtering/scoring, token producer, KV cache indexing/lookup,
  KV events pub/sub, and sidecar proxy attributes.
- Migrate EPP scheduling, profile handler, data producers, and tests to
  semconv keys and constructors.
- Migrate kvcache indexer, prefix match, traced scorer, and kvblock to
  semconv.
- Migrate kvevents pool and ZeroMQ subscriber to semconv.
- Migrate sidecar proxy connectors \(nixlv2, p2p, decode, pd_common,
  chat completions\) to semconv.
- Add unit tests for all new attribute constructors in semconv/llm_d_test.go.

Signed-off-by: Yuan Gao <plateau451@gmail.com>")[#…](https://github.com/llm-d/llm-d-router/pull/2516)  | Sep 11, 2026  |  
|   |   |  [fix: Make TokenizedPrompt.MultiModalFeatures per-prompt (](https://github.com/llm-d/llm-d-router/commit/229b8ea5f56a99c732b02506cbd733cbc3d13cea "fix: Make TokenizedPrompt.MultiModalFeatures per-prompt \(#1633\)

* fix: Make MultiModalFeatures per-prompt in TokenizedPrompt

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Bundle per-prompt tokens and MM features into PromptTokens struct

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename missing instances

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename missing instances

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: update stale TokenizedPrompt reference missed during rebase

The auto-merge left one test using the old TokenizedPrompt type and
PerPromptTokens field in producer_test.go. Updated to use
TokenizedRequest with Prompts/PromptTokens.

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename stale entries

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename stale entries

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename stale entries

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

---------

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>")[#1633](https://github.com/llm-d/llm-d-router/pull/1633)[)](https://github.com/llm-d/llm-d-router/commit/229b8ea5f56a99c732b02506cbd733cbc3d13cea "fix: Make TokenizedPrompt.MultiModalFeatures per-prompt \(#1633\)

* fix: Make MultiModalFeatures per-prompt in TokenizedPrompt

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Bundle per-prompt tokens and MM features into PromptTokens struct

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename missing instances

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename missing instances

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: update stale TokenizedPrompt reference missed during rebase

The auto-merge left one test using the old TokenizedPrompt type and
PerPromptTokens field in producer_test.go. Updated to use
TokenizedRequest with Prompts/PromptTokens.

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename stale entries

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename stale entries

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename stale entries

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

---------

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>")  | Aug 20, 2026  |  
|   |   |  [fix: Make TokenizedPrompt.MultiModalFeatures per-prompt (](https://github.com/llm-d/llm-d-router/commit/229b8ea5f56a99c732b02506cbd733cbc3d13cea "fix: Make TokenizedPrompt.MultiModalFeatures per-prompt \(#1633\)

* fix: Make MultiModalFeatures per-prompt in TokenizedPrompt

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Bundle per-prompt tokens and MM features into PromptTokens struct

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename missing instances

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename missing instances

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: update stale TokenizedPrompt reference missed during rebase

The auto-merge left one test using the old TokenizedPrompt type and
PerPromptTokens field in producer_test.go. Updated to use
TokenizedRequest with Prompts/PromptTokens.

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename stale entries

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename stale entries

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename stale entries

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

---------

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>")[#1633](https://github.com/llm-d/llm-d-router/pull/1633)[)](https://github.com/llm-d/llm-d-router/commit/229b8ea5f56a99c732b02506cbd733cbc3d13cea "fix: Make TokenizedPrompt.MultiModalFeatures per-prompt \(#1633\)

* fix: Make MultiModalFeatures per-prompt in TokenizedPrompt

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Bundle per-prompt tokens and MM features into PromptTokens struct

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename missing instances

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename missing instances

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: update stale TokenizedPrompt reference missed during rebase

The auto-merge left one test using the old TokenizedPrompt type and
PerPromptTokens field in producer_test.go. Updated to use
TokenizedRequest with Prompts/PromptTokens.

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename stale entries

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename stale entries

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

* fix: Rename stale entries

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>

---------

Signed-off-by: Alberto Perdomo <aperdomo@redhat.com>")  | Aug 20, 2026  |  
|   |   |  [refactor(datalayer): key AttributeMap by DataKey instead of string (](https://github.com/llm-d/llm-d-router/commit/800ec0e453d8b962ca0af6f03d3bfbe8e685350c "refactor\(datalayer\): key AttributeMap by DataKey instead of string \(#2190\)

* refactor\(datalayer\): key AttributeMap by DataKey instead of string

Signed-off-by: satyamg1620 <Satyam.Gupta.3@ibm.com>

* enforce\(requestcontrol\): scope DataProducer Put/Get to declared keys

Signed-off-by: satyamg1620 <Satyam.Gupta.3@ibm.com>

* enforce\(framework\): confine plugin attribute access to declared keys

Signed-off-by: satyamg1620 <Satyam.Gupta.3@ibm.com>

* refactor\(framework\): confine plugin attribute access to declared keys

Signed-off-by: satyamg1620 <Satyam.Gupta.3@ibm.com>

* refactor\(datalayer\): surface plugin data-scope violations

Move runtime confinement alongside the static dependency validation it
mirrors, so both halves of the Produces\(\)/Consumes\(\) contract live in one
package.

A rejected access surfaced only as a log line, and a rejected read logged
one per endpoint per request. Count every rejection under
llm_d_epp_plugin_data_scope_violations_total and log the first offence of
each kind per invocation, so a misdeclared plugin stays visible in
production without depending on log verbosity.

Signed-off-by: satyamg1620 <Satyam.Gupta.3@ibm.com>

* fix\(endpointattribute\): reject the combined Attribute/Producer spelling

The attribute name and its producer are separate parameters, but they were
once a single "Attribute/Producer" string. A key built from that spelling
matches nothing: the read resolves as absent, so the filter keeps every
endpoint and the scorer returns zero, with no error anywhere.

Reject it at construction and name the split in the message. A producer
pointer tells an omitted producer from one set to the empty string, so an
attribute whose own name contains a slash stays reachable.

Signed-off-by: satyamg1620 <Satyam.Gupta.3@ibm.com>

---------

Signed-off-by: satyamg1620 <Satyam.Gupta.3@ibm.com>")[#…](https://github.com/llm-d/llm-d-router/pull/2190)  | Aug 12, 2026  |  
|   |   | [feature(epp): authenticate token-producer warmup probe via VLLM_API_K…](https://github.com/llm-d/llm-d-router/commit/805c2ed0303f3bcb120e45041e8ec283710b2e7e "feature\(epp\): authenticate token-producer warmup probe via VLLM_API_KEY \(#2763\)

Signed-off-by: Iceber Gu <caiwei95@hotmail.com>")  | Sep 11, 2026  |  
|   |   |  [feat(epp): add latency-observation scorer and producer (](https://github.com/llm-d/llm-d-router/commit/7244e8bd64970eb91f2e620b9b7fec9e4b6abb99 "feat\(epp\): add latency-observation scorer and producer \(#2371\)

* Addin ttft-aware-scorer.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Update comments.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Move unneeded files.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Add draft issue.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Add Readmes.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix review comments.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Add notes.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Update tests.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Use polling dispatcher and rename.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Remove issue file.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix comments.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix test.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Remove conf file.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix lint.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Declare producer keys.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Add nil checks.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix review comments.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix comments and add missing tests.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Change plugin name.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Align docs with AGENTS.md style.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

---------

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>
Signed-off-by: Mohammad-nassar10 <79787844+Mohammad-nassar10@users.noreply.github.com>")[#2371](https://github.com/llm-d/llm-d-router/pull/2371)[)](https://github.com/llm-d/llm-d-router/commit/7244e8bd64970eb91f2e620b9b7fec9e4b6abb99 "feat\(epp\): add latency-observation scorer and producer \(#2371\)

* Addin ttft-aware-scorer.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Update comments.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Move unneeded files.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Add draft issue.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Add Readmes.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix review comments.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Add notes.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Update tests.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Use polling dispatcher and rename.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Remove issue file.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix comments.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix test.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Remove conf file.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix lint.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Declare producer keys.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Add nil checks.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix review comments.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Fix comments and add missing tests.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Change plugin name.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

* Align docs with AGENTS.md style.

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>

---------

Signed-off-by: Mohammad <mohammad.nassar@ibm.com>
Signed-off-by: Mohammad-nassar10 <79787844+Mohammad-nassar10@users.noreply.github.com>")  | Sep 8, 2026  |  
|   |   |  [docs: add README for requestcontrol/dataproducer directory (](https://github.com/llm-d/llm-d-router/commit/89dbdf9b00c94de97c8196618b796c9b6b18f684 "docs: add README for requestcontrol/dataproducer directory \(#1455\)

* docs: add README for requestcontrol/dataproducer directory

Summarises the seven data producer plugins, their produced attributes,
lifecycle hooks, and inter-plugin dependency ordering.

Signed-off-by: Rahul Gurnani <rahulgurnani@google.com>

* Add dataproducer diagram

Signed-off-by: Rahul Gurnani <rahulgurnani@google.com>

---------

Signed-off-by: Rahul Gurnani <rahulgurnani@google.com>")[#1455](https://github.com/llm-d/llm-d-router/pull/1455)[)](https://github.com/llm-d/llm-d-router/commit/89dbdf9b00c94de97c8196618b796c9b6b18f684 "docs: add README for requestcontrol/dataproducer directory \(#1455\)

* docs: add README for requestcontrol/dataproducer directory

Summarises the seven data producer plugins, their produced attributes,
lifecycle hooks, and inter-plugin dependency ordering.

Signed-off-by: Rahul Gurnani <rahulgurnani@google.com>

* Add dataproducer diagram

Signed-off-by: Rahul Gurnani <rahulgurnani@google.com>

---------

Signed-off-by: Rahul Gurnani <rahulgurnani@google.com>")  | Jun 11, 2026  |  
| View all files |  
## [README.md](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/requestcontrol/dataproducer#readme)
Outline
# Request-Control Data Producer Plugins
Data producer plugins run in the `PrepareRequestData` phase of the request-control pipeline, before filters and scorers execute. Each plugin implements `requestcontrol.DataProducer` and writes typed attributes onto endpoints or the `InferenceRequest` itself. Downstream plugins (scorers, filters, admission controllers) read these attributes without re-doing the computation.
The following diagram shows how producers are connected to the request control and scheduling plugins: 
Producers may also implement additional lifecycle hooks:
  * `PreRequest` — called after a routing decision is made; used to persist bookkeeping state (e.g., update a cache index, increment an in-flight counter).
  * `ResponseHeader` / `ResponseBody` — called as response data arrives; used to collect training data or release in-flight counters.


## Available Producers  
| Plugin type  | Package  | Produces  | Summary  |  
| --- | --- | --- | --- |  
| `token-producer`  | `TokenizedPrompt`  | Tokenizes the request prompt via vLLM `/render`; required by precise-prefix-cache-producer and context-length-aware scorers.  |  
| `approx-prefix-cache-producer`  | `PrefixCacheMatchInfo`  | Hashes the prompt into blocks and matches against a per-pod LRU index for approximate prefix-cache affinity.  |  
| `precise-prefix-cache-producer`  | `PrefixCacheMatchInfo`  | Maintains a precise KV-block index by subscribing to vLLM KV-events; requires `token-producer` upstream.  |  
| `burst-prefix-cache-producer`  | `PrefixCacheMatchInfo`  | Batches requests within a time window and co-locates prompt-sharing samples (e.g. RL rollout groups) onto shared replicas; requires `token-producer` upstream.  |  
| `inflight-load-producer`  | `InFlightLoad`  | Tracks real-time in-flight request and token counts per endpoint across the full request lifecycle.  |  
| `predicted-latency-producer`  | `LatencyPredictionInfo`  | Trains XGBoost models via a sidecar and generates per-endpoint TTFT/TPOT predictions.  |  
| `latency-observer-producer-hub`  | `TTFTPercentiles`  | Measures each request's actual time-to-first-token and publishes per-endpoint percentile anchors for `latency-observation-scorer-hub`. Needs nothing from the endpoint but its response.  |  
| `session-id-producer`  | `SessionID`  | Extracts a session identifier from a request header or cookie and publishes it for affinity-aware plugins.  |  
| `mm-embeddings-cache-producer`  | `EncoderCacheMatchInfo`  | Tracks which pods recently processed each multimodal input hash and scores encoder-cache affinity.  |  
| `p2p-source-producer`  | request attribute only  | Sets the `x-kv-cache-source-host-port` header to the candidate holding the most cached prefix tokens when it out-caches the pod computing the prefix, for P2P KV pulls.  |  
## Plugin ordering and dependencies
The framework resolves a DAG from each plugin's `Produces` and `Consumes` declarations and runs producers in dependency order. Explicit dependencies to be aware of:
  * `precise-prefix-cache-producer` **requires** `token-producer` upstream (it consumes `TokenizedPrompt`).
  * `burst-prefix-cache-producer` **requires** `token-producer` upstream (it consumes `TokenizedPrompt`).
  * `mm-embeddings-cache-producer` **optionally** consumes `TokenizedPrompt`; configure `token-producer` first when multimodal features need tokenizer-derived hashes.
  * `inflight-load-producer` **optionally** consumes `PrefixCacheMatchInfo` from an approx or precise prefix producer; prefix-discounting is applied automatically when the attribute is present.
  * `p2p-source-producer` **requires** `PrefixCacheMatchInfo` from a prefix producer; set `prefixMatchInfoProducerName` to select a non-default producer instance. Omitting it binds the default key, which auto-wires the approximate producer (no error) — set it explicitly for precise-only deployments. Set `prefillProfileName` to match a renamed `disagg-profile-handler` prefill profile.
  * `predicted-latency-producer` **optionally** consumes `PrefixCacheMatchInfo`; set `prefixMatchInfoProducerName` in its config to the name of the prefix producer instance.
  * `latency-observer-producer-hub` **requires** `InFlightLoad`, so `inflight-load-producer` is ordered ahead of it and auto-created when absent. It must itself be listed under `dataLayer.sources`; auto-creation from `latency-observation-scorer-hub`'s required data key only wires the attribute, not the periodic tick that publishes it. See the [producer README](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/latencyobserver/README.md#configuration).


## Related documentation
  * [Approximate Prefix Cache Producer](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/approximateprefix/README.md)
  * [Precise Prefix Cache Producer](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache/README.md)
  * [Burst Prefix Cache Producer](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/burstprefix/README.md)
  * [In-Flight Load Producer](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/inflightload/README.md)
  * [Predicted Latency Producer](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/predictedlatency/README.md)
  * [Latency Observer Producer](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/latencyobserver/README.md)
  * [Multimodal Embeddings Cache Producer](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/multimodal/README.md)


You can’t perform that action at this time. 
