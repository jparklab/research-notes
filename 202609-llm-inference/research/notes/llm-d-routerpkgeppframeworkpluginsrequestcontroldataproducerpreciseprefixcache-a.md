---
title: llm-d-router/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache
  at main · llm-d/llm-d-router · GitHub
id: llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducerpreciseprefixcache-a
tags:
- llmd-router-nonk8s-backends-54b774
- locus-kv-event-path-off-k8s
- precise-prefix-cache-producer
- go-source
created: '2026-09-11T21:23:28.776566Z'
updated: '2026-09-12T03:37:37.760749Z'
source: https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache
source_domain: github.com
fetched_at: '2026-09-11T21:23:28.755140Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: GitHub directory listing for preciseprefixcache/ subpackage; mostly renders
  commit-message history (JS tree view didn't render filenames in scraped HTML) rather
  than a usable file list. Establishes the package exists at pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache
  and surfaces related commit context (e.g. a refactor folding longest-prefix matching
  into a single prefixAccumulator used by both LongestPrefixScorer and the precise-prefix
  producer's per-tier block counts). Low standalone value; superseded by directly
  fetching producer.go and extractor.go from this same directory.
---

*Suggested by [[llm-d-routerpkgeppframeworkpluginsrequestcontroldataproducer-at-main-llm-dllm-d]] — subdirectory for precise-prefix-cache-producer*

[Skip to content](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# preciseprefixcache
/
Copy path
## Directory actions
## More options
More options
## Directory actions
## More options
More options
## Latest commit
[telemetry: migrate remaining llm_d and gen_ai attributes to semconv (](https://github.com/llm-d/llm-d-router/commit/db208e3d5cb5975c88a784dfd56b1977829d828a)[#…](https://github.com/llm-d/llm-d-router/pull/2516)
Open commit detailsfailure
Sep 11, 2026
[db208e3](https://github.com/llm-d/llm-d-router/commit/db208e3d5cb5975c88a784dfd56b1977829d828a) · Sep 11, 2026
## History
[History](https://github.com/llm-d/llm-d-router/commits/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache)
Open commit details
History
/
# preciseprefixcache
/
Copy path
## Folders and files  
| Name  | Name  | Last commit message  | Last commit date  |  
| --- | --- | --- | --- |  
| 
### parent directory
 |  
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
|   |   |  [refactor(kvcache): fold prefix matching into one accumulator (](https://github.com/llm-d/llm-d-router/commit/74eaecb547c88f1296ca45ddc7b364de561a71c1 "refactor\(kvcache\): fold prefix matching into one accumulator \(#2669\)

* refactor\(kvcache\): fold prefix matching into one accumulator

Longest-prefix matching is spread over implementations that each read
the same rules: LongestPrefixScorer over a materialized Lookup for the
weighted score, and the precise-prefix producer's matchedBlockCount and
matchedBlockCountByTier for the block and per-tier counts it publishes,
re-walking the lookup result once per endpoint. No single place defines
the contract, so nothing can test it in isolation, and the per-endpoint
re-walk grows with fleet size.

Indexer.MatchBlockKeys is the one entry point. A pooled prefixAccumulator
implements the rules once \(candidates are the pods holding the first
key, a chain ends at the pod's first missing key, duplicate entries at a
key take the highest weight, every tier tracks its own chain\) and folds
each key's entries in one call, skipping repeated rank entries of one
endpoint. ScoreTokens and LongestPrefixScorer.Score are projections of
it; the producer aggregates per prompt and publishes all or nothing.
KVBlockScorer and NewTracedScorer stay as deprecated compatibility
surfaces; ScoreTokens no longer scores through them. A property test
checks the accumulator against the three replaced algorithms on random
fixtures.

Index reads are unchanged: the matcher materializes Lookup. The
score_tokens span keeps blocks_found and block_hit_ratio as the count of
requested keys held by any candidate; the matcher's own span reports the
longest chain. M4 Max, n=6, ScoreTokens on the Kermit shape \(1,034
blocks held by 40 endpoints with eight rank entries each\): 294.5ms and
4.6GiB per request before, 67.7ms and 116.5MiB after.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* fix\(kvcache\): count a device tier named speculative in the speculative chain

The per-tier counters the matcher replaced keyed tiers by name, so an
entry whose engine-reported device tier is literally "speculative" and a
speculative entry counted in one chain. The accumulator keyed the two by
ordinal and reported them under the same name, so a chain that continued
from one kind of entry to the other broke instead. Both now fold into the
speculative chain; weights still follow the reported tier.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* fix\(kvcache\): check cancellation when the materialized match completes

The materialized matcher polls cancellation every 256 key positions, so
a request cancelled after its last checkpoint returned a match. A final
check at completion means a cancelled request never reports one.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* fix\(kvcache\): count only keys a pod holds in block-hit telemetry

Index.Lookup is documented to return a map keyed by the requested keys,
so a backend may report a miss as an entry with no pods and
len\(keyToPods\) would publish it as a hit. ScoreTokens guarded on
len\(pods\) > 0 before the matcher took over the count, and #2670 commits
that the score_tokens attributes keep their meaning, so restore it.

InMemoryIndex reaches that shape under a race: Lookup reads cache.Len\(\)
and cache.Keys\(\) as separate locked calls, so evicting the last entry
between them leaves the unfiltered branch assigning an empty slice. A
stub index carries the test to make the case deterministic.

The benchmark fixture is named for what it exercises, endpoints
reporting many rank entries per block, rather than for the cluster it
was first measured on.

Signed-off-by: nilig <nili.ifergan@gmail.com>

---------

Signed-off-by: nilig <nili.ifergan@gmail.com>")[#2669](https://github.com/llm-d/llm-d-router/pull/2669)[)](https://github.com/llm-d/llm-d-router/commit/74eaecb547c88f1296ca45ddc7b364de561a71c1 "refactor\(kvcache\): fold prefix matching into one accumulator \(#2669\)

* refactor\(kvcache\): fold prefix matching into one accumulator

Longest-prefix matching is spread over implementations that each read
the same rules: LongestPrefixScorer over a materialized Lookup for the
weighted score, and the precise-prefix producer's matchedBlockCount and
matchedBlockCountByTier for the block and per-tier counts it publishes,
re-walking the lookup result once per endpoint. No single place defines
the contract, so nothing can test it in isolation, and the per-endpoint
re-walk grows with fleet size.

Indexer.MatchBlockKeys is the one entry point. A pooled prefixAccumulator
implements the rules once \(candidates are the pods holding the first
key, a chain ends at the pod's first missing key, duplicate entries at a
key take the highest weight, every tier tracks its own chain\) and folds
each key's entries in one call, skipping repeated rank entries of one
endpoint. ScoreTokens and LongestPrefixScorer.Score are projections of
it; the producer aggregates per prompt and publishes all or nothing.
KVBlockScorer and NewTracedScorer stay as deprecated compatibility
surfaces; ScoreTokens no longer scores through them. A property test
checks the accumulator against the three replaced algorithms on random
fixtures.

Index reads are unchanged: the matcher materializes Lookup. The
score_tokens span keeps blocks_found and block_hit_ratio as the count of
requested keys held by any candidate; the matcher's own span reports the
longest chain. M4 Max, n=6, ScoreTokens on the Kermit shape \(1,034
blocks held by 40 endpoints with eight rank entries each\): 294.5ms and
4.6GiB per request before, 67.7ms and 116.5MiB after.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* fix\(kvcache\): count a device tier named speculative in the speculative chain

The per-tier counters the matcher replaced keyed tiers by name, so an
entry whose engine-reported device tier is literally "speculative" and a
speculative entry counted in one chain. The accumulator keyed the two by
ordinal and reported them under the same name, so a chain that continued
from one kind of entry to the other broke instead. Both now fold into the
speculative chain; weights still follow the reported tier.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* fix\(kvcache\): check cancellation when the materialized match completes

The materialized matcher polls cancellation every 256 key positions, so
a request cancelled after its last checkpoint returned a match. A final
check at completion means a cancelled request never reports one.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* fix\(kvcache\): count only keys a pod holds in block-hit telemetry

Index.Lookup is documented to return a map keyed by the requested keys,
so a backend may report a miss as an entry with no pods and
len\(keyToPods\) would publish it as a hit. ScoreTokens guarded on
len\(pods\) > 0 before the matcher took over the count, and #2670 commits
that the score_tokens attributes keep their meaning, so restore it.

InMemoryIndex reaches that shape under a race: Lookup reads cache.Len\(\)
and cache.Keys\(\) as separate locked calls, so evicting the last entry
between them leaves the unfiltered branch assigning an empty slice. A
stub index carries the test to make the case deterministic.

The benchmark fixture is named for what it exercises, endpoints
reporting many rank entries per block, rather than for the cluster it
was first measured on.

Signed-off-by: nilig <nili.ifergan@gmail.com>

---------

Signed-off-by: nilig <nili.ifergan@gmail.com>")  | Sep 4, 2026  |  
|   |   | [feat(observability): add mm.modality, mm.hash_count, mm.hit to EPP sc…](https://github.com/llm-d/llm-d-router/commit/5ce6ab56bdac111889ee603a1dd07b89e4516710 "feat\(observability\): add mm.modality, mm.hash_count, mm.hit to EPP scheduler spans \(#1546\)

* feat\(observability\): add mm.modality, mm.hash_count, mm.hit to EPP scheduler spans

Multimodal routing decisions are now distinguishable from text-only ones
across the precise-prefix-cache scorer wrapper and the disagg/pd profile
handler spans. mm.hit reflects per-feature-type block-match attribution
tracked through PrefixCacheMatchInfo.MM\(\); the attribute is omitted when
no endpoint had MM tracked. mm.modality and mm.hash_count derive from the
request's MultiModalFeatures.

Fixes #1542

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

* fix\(observability\): gate WithMM on MM presence

Text-only requests no longer attach MMMatchInfo, so MM\(\) stays nil and the
scheduler span omits mm.hit instead of reporting a false miss. Clarify the
WithMM contract and assert text-only leaves MM untracked.

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>

---------

Signed-off-by: Sam Batschelet <sbatsche@redhat.com>")  | Jul 28, 2026  |  
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
|   |   |  [fix: IPv6 address in remaining endpoints (](https://github.com/llm-d/llm-d-router/commit/31bc02c2f1dc8c118ea89f535a928672e81d0613 "fix: IPv6 address in remaining endpoints \(#2658\)

- use net.JoinHostPort for protocol endpoint to extractor and mooncake
  bootstrap address, nixlv2 remote engine id.
- keep internal cache identifiers unchanged to preserve kv compatibility during router upgrade

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")[#2658](https://github.com/llm-d/llm-d-router/pull/2658)[)](https://github.com/llm-d/llm-d-router/commit/31bc02c2f1dc8c118ea89f535a928672e81d0613 "fix: IPv6 address in remaining endpoints \(#2658\)

- use net.JoinHostPort for protocol endpoint to extractor and mooncake
  bootstrap address, nixlv2 remote engine id.
- keep internal cache identifiers unchanged to preserve kv compatibility during router upgrade

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")  | Sep 2, 2026  |  
|   |   |  [fix: IPv6 address in remaining endpoints (](https://github.com/llm-d/llm-d-router/commit/31bc02c2f1dc8c118ea89f535a928672e81d0613 "fix: IPv6 address in remaining endpoints \(#2658\)

- use net.JoinHostPort for protocol endpoint to extractor and mooncake
  bootstrap address, nixlv2 remote engine id.
- keep internal cache identifiers unchanged to preserve kv compatibility during router upgrade

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")[#2658](https://github.com/llm-d/llm-d-router/pull/2658)[)](https://github.com/llm-d/llm-d-router/commit/31bc02c2f1dc8c118ea89f535a928672e81d0613 "fix: IPv6 address in remaining endpoints \(#2658\)

- use net.JoinHostPort for protocol endpoint to extractor and mooncake
  bootstrap address, nixlv2 remote engine id.
- keep internal cache identifiers unchanged to preserve kv compatibility during router upgrade

Signed-off-by: Wen Zhou <wenzhou@redhat.com>")  | Sep 2, 2026  |  
|   |   |  [feat(epp): add error return to PreRequest plugin interface (](https://github.com/llm-d/llm-d-router/commit/3f272cc47228bbdcfea6e751004180548c341aa5 "feat\(epp\): add error return to PreRequest plugin interface \(#2215\)

* feat\(epp\): add error return to PreRequest plugin interface

Groundwork for issue #1686 \(conditional-decode routing\). PreRequest
plugins need a way to signal failure to the director. The runner now
invokes every registered plugin regardless of individual failures and
returns their aggregated errors via errors.Join, so a caller sees all
failures at once rather than only the first.

Every existing implementation returns nil today; behavior is unchanged.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* fix\(epp\): preserve typed PreRequest plugin error codes downstream

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs\(epp\): document PreRequest side-effect invariant

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* test\(epp\): cover zero-plugin and all-succeed cases for runPreRequestPlugins

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs\(epp\): note errcommon.Error classification on PreRequest interface

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* fix\(epp\): collapse multi-plugin PreRequest failures to Internal

runPreRequestPlugins joins per-plugin errors, so a caller that unwraps
any typed errcommon.Error via errors.As would silently drop siblings when
several plugins fail with different codes. Restrict the unwrap to the
single-failure case; multiple failures now return Internal with the
joined text so every plugin's message reaches the client.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* test\(epp\): assert nil error from PreRequest calls

Replace silent `_ = ...PreRequest\(...\)` discards with `require.NoError`
so a non-nil return actually fails the test. The one call inside a
goroutine uses an atomic counter checked after wg.Wait\(\), matching the
file's convention of not calling require/assert from non-test goroutines.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

---------

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>")[#2215](https://github.com/llm-d/llm-d-router/pull/2215)[)](https://github.com/llm-d/llm-d-router/commit/3f272cc47228bbdcfea6e751004180548c341aa5 "feat\(epp\): add error return to PreRequest plugin interface \(#2215\)

* feat\(epp\): add error return to PreRequest plugin interface

Groundwork for issue #1686 \(conditional-decode routing\). PreRequest
plugins need a way to signal failure to the director. The runner now
invokes every registered plugin regardless of individual failures and
returns their aggregated errors via errors.Join, so a caller sees all
failures at once rather than only the first.

Every existing implementation returns nil today; behavior is unchanged.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* fix\(epp\): preserve typed PreRequest plugin error codes downstream

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs\(epp\): document PreRequest side-effect invariant

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* test\(epp\): cover zero-plugin and all-succeed cases for runPreRequestPlugins

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs\(epp\): note errcommon.Error classification on PreRequest interface

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* fix\(epp\): collapse multi-plugin PreRequest failures to Internal

runPreRequestPlugins joins per-plugin errors, so a caller that unwraps
any typed errcommon.Error via errors.As would silently drop siblings when
several plugins fail with different codes. Restrict the unwrap to the
single-failure case; multiple failures now return Internal with the
joined text so every plugin's message reaches the client.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* test\(epp\): assert nil error from PreRequest calls

Replace silent `_ = ...PreRequest\(...\)` discards with `require.NoError`
so a non-nil return actually fails the test. The one call inside a
goroutine uses an atomic counter checked after wg.Wait\(\), matching the
file's convention of not calling require/assert from non-test goroutines.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

---------

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>")  | Aug 8, 2026  |  
|   |   |  [feat(epp): add error return to PreRequest plugin interface (](https://github.com/llm-d/llm-d-router/commit/3f272cc47228bbdcfea6e751004180548c341aa5 "feat\(epp\): add error return to PreRequest plugin interface \(#2215\)

* feat\(epp\): add error return to PreRequest plugin interface

Groundwork for issue #1686 \(conditional-decode routing\). PreRequest
plugins need a way to signal failure to the director. The runner now
invokes every registered plugin regardless of individual failures and
returns their aggregated errors via errors.Join, so a caller sees all
failures at once rather than only the first.

Every existing implementation returns nil today; behavior is unchanged.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* fix\(epp\): preserve typed PreRequest plugin error codes downstream

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs\(epp\): document PreRequest side-effect invariant

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* test\(epp\): cover zero-plugin and all-succeed cases for runPreRequestPlugins

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs\(epp\): note errcommon.Error classification on PreRequest interface

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* fix\(epp\): collapse multi-plugin PreRequest failures to Internal

runPreRequestPlugins joins per-plugin errors, so a caller that unwraps
any typed errcommon.Error via errors.As would silently drop siblings when
several plugins fail with different codes. Restrict the unwrap to the
single-failure case; multiple failures now return Internal with the
joined text so every plugin's message reaches the client.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* test\(epp\): assert nil error from PreRequest calls

Replace silent `_ = ...PreRequest\(...\)` discards with `require.NoError`
so a non-nil return actually fails the test. The one call inside a
goroutine uses an atomic counter checked after wg.Wait\(\), matching the
file's convention of not calling require/assert from non-test goroutines.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

---------

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>")[#2215](https://github.com/llm-d/llm-d-router/pull/2215)[)](https://github.com/llm-d/llm-d-router/commit/3f272cc47228bbdcfea6e751004180548c341aa5 "feat\(epp\): add error return to PreRequest plugin interface \(#2215\)

* feat\(epp\): add error return to PreRequest plugin interface

Groundwork for issue #1686 \(conditional-decode routing\). PreRequest
plugins need a way to signal failure to the director. The runner now
invokes every registered plugin regardless of individual failures and
returns their aggregated errors via errors.Join, so a caller sees all
failures at once rather than only the first.

Every existing implementation returns nil today; behavior is unchanged.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* fix\(epp\): preserve typed PreRequest plugin error codes downstream

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs\(epp\): document PreRequest side-effect invariant

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* test\(epp\): cover zero-plugin and all-succeed cases for runPreRequestPlugins

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* docs\(epp\): note errcommon.Error classification on PreRequest interface

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* fix\(epp\): collapse multi-plugin PreRequest failures to Internal

runPreRequestPlugins joins per-plugin errors, so a caller that unwraps
any typed errcommon.Error via errors.As would silently drop siblings when
several plugins fail with different codes. Restrict the unwrap to the
single-failure case; multiple failures now return Internal with the
joined text so every plugin's message reaches the client.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

* test\(epp\): assert nil error from PreRequest calls

Replace silent `_ = ...PreRequest\(...\)` discards with `require.NoError`
so a non-nil return actually fails the test. The one call inside a
goroutine uses an atomic counter checked after wg.Wait\(\), matching the
file's convention of not calling require/assert from non-test goroutines.

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>

---------

Signed-off-by: Dmitri Pikus <DPIKUS@il.ibm.com>")  | Aug 8, 2026  |  
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
|   |   |  [Remove deprecated UDS/legacy tokenizer code and drop llm-d-kv-cache (](https://github.com/llm-d/llm-d-router/commit/4f8e571188949b1c666f13c1b32daab5ceba1b03 "Remove deprecated UDS/legacy tokenizer code and drop llm-d-kv-cache \(#2512\)

* Remove deprecated UDS/legacy tokenizer code and drop llm-d-kv-cache

The UDS tokenizer backend and the in-process tokenization pool were both
deprecated in favour of the vLLM HTTP /render backend, which has been the
default since 0.9.0. They were the only remaining consumers of the
llm-d-kv-cache module, so removing them drops the dependency.

Plugin parameters are decoded with DisallowUnknownFields, so a config that
still sets udsTokenizerConfig or indexerConfig.tokenizersPoolConfig is now
rejected at startup rather than ignored.

Closes: #2361
Signed-off-by: satyamg1620 <Satyam.Gupta.3@ibm.com>

* Remove stale UDS references and test the scorer's pool-config rejection

The vLLM render request builder and the tokenization types README describe
behaviour in terms of a UDS tokenizer the tree no longer has.

Strict decoding rejects indexerConfig.tokenizersPoolConfig on the
precise-prefix-cache scorer as well as the producer; only the producer had
a test.

Signed-off-by: satyamg1620 <Satyam.Gupta.3@ibm.com>

* Test that token-producer rejects udsTokenizerConfig

The release note promises a startup failure naming the removed parameter,
but only the tokenizersPoolConfig half of that promise was covered.

Signed-off-by: satyamg1620 <Satyam.Gupta.3@ibm.com>

---------

Signed-off-by: satyamg1620 <Satyam.Gupta.3@ibm.com>")[#…](https://github.com/llm-d/llm-d-router/pull/2512)  | Sep 4, 2026  |  
|   |   |  [refactor(kvcache): fold prefix matching into one accumulator (](https://github.com/llm-d/llm-d-router/commit/74eaecb547c88f1296ca45ddc7b364de561a71c1 "refactor\(kvcache\): fold prefix matching into one accumulator \(#2669\)

* refactor\(kvcache\): fold prefix matching into one accumulator

Longest-prefix matching is spread over implementations that each read
the same rules: LongestPrefixScorer over a materialized Lookup for the
weighted score, and the precise-prefix producer's matchedBlockCount and
matchedBlockCountByTier for the block and per-tier counts it publishes,
re-walking the lookup result once per endpoint. No single place defines
the contract, so nothing can test it in isolation, and the per-endpoint
re-walk grows with fleet size.

Indexer.MatchBlockKeys is the one entry point. A pooled prefixAccumulator
implements the rules once \(candidates are the pods holding the first
key, a chain ends at the pod's first missing key, duplicate entries at a
key take the highest weight, every tier tracks its own chain\) and folds
each key's entries in one call, skipping repeated rank entries of one
endpoint. ScoreTokens and LongestPrefixScorer.Score are projections of
it; the producer aggregates per prompt and publishes all or nothing.
KVBlockScorer and NewTracedScorer stay as deprecated compatibility
surfaces; ScoreTokens no longer scores through them. A property test
checks the accumulator against the three replaced algorithms on random
fixtures.

Index reads are unchanged: the matcher materializes Lookup. The
score_tokens span keeps blocks_found and block_hit_ratio as the count of
requested keys held by any candidate; the matcher's own span reports the
longest chain. M4 Max, n=6, ScoreTokens on the Kermit shape \(1,034
blocks held by 40 endpoints with eight rank entries each\): 294.5ms and
4.6GiB per request before, 67.7ms and 116.5MiB after.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* fix\(kvcache\): count a device tier named speculative in the speculative chain

The per-tier counters the matcher replaced keyed tiers by name, so an
entry whose engine-reported device tier is literally "speculative" and a
speculative entry counted in one chain. The accumulator keyed the two by
ordinal and reported them under the same name, so a chain that continued
from one kind of entry to the other broke instead. Both now fold into the
speculative chain; weights still follow the reported tier.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* fix\(kvcache\): check cancellation when the materialized match completes

The materialized matcher polls cancellation every 256 key positions, so
a request cancelled after its last checkpoint returned a match. A final
check at completion means a cancelled request never reports one.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* fix\(kvcache\): count only keys a pod holds in block-hit telemetry

Index.Lookup is documented to return a map keyed by the requested keys,
so a backend may report a miss as an entry with no pods and
len\(keyToPods\) would publish it as a hit. ScoreTokens guarded on
len\(pods\) > 0 before the matcher took over the count, and #2670 commits
that the score_tokens attributes keep their meaning, so restore it.

InMemoryIndex reaches that shape under a race: Lookup reads cache.Len\(\)
and cache.Keys\(\) as separate locked calls, so evicting the last entry
between them leaves the unfiltered branch assigning an empty slice. A
stub index carries the test to make the case deterministic.

The benchmark fixture is named for what it exercises, endpoints
reporting many rank entries per block, rather than for the cluster it
was first measured on.

Signed-off-by: nilig <nili.ifergan@gmail.com>

---------

Signed-off-by: nilig <nili.ifergan@gmail.com>")[#2669](https://github.com/llm-d/llm-d-router/pull/2669)[)](https://github.com/llm-d/llm-d-router/commit/74eaecb547c88f1296ca45ddc7b364de561a71c1 "refactor\(kvcache\): fold prefix matching into one accumulator \(#2669\)

* refactor\(kvcache\): fold prefix matching into one accumulator

Longest-prefix matching is spread over implementations that each read
the same rules: LongestPrefixScorer over a materialized Lookup for the
weighted score, and the precise-prefix producer's matchedBlockCount and
matchedBlockCountByTier for the block and per-tier counts it publishes,
re-walking the lookup result once per endpoint. No single place defines
the contract, so nothing can test it in isolation, and the per-endpoint
re-walk grows with fleet size.

Indexer.MatchBlockKeys is the one entry point. A pooled prefixAccumulator
implements the rules once \(candidates are the pods holding the first
key, a chain ends at the pod's first missing key, duplicate entries at a
key take the highest weight, every tier tracks its own chain\) and folds
each key's entries in one call, skipping repeated rank entries of one
endpoint. ScoreTokens and LongestPrefixScorer.Score are projections of
it; the producer aggregates per prompt and publishes all or nothing.
KVBlockScorer and NewTracedScorer stay as deprecated compatibility
surfaces; ScoreTokens no longer scores through them. A property test
checks the accumulator against the three replaced algorithms on random
fixtures.

Index reads are unchanged: the matcher materializes Lookup. The
score_tokens span keeps blocks_found and block_hit_ratio as the count of
requested keys held by any candidate; the matcher's own span reports the
longest chain. M4 Max, n=6, ScoreTokens on the Kermit shape \(1,034
blocks held by 40 endpoints with eight rank entries each\): 294.5ms and
4.6GiB per request before, 67.7ms and 116.5MiB after.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* fix\(kvcache\): count a device tier named speculative in the speculative chain

The per-tier counters the matcher replaced keyed tiers by name, so an
entry whose engine-reported device tier is literally "speculative" and a
speculative entry counted in one chain. The accumulator keyed the two by
ordinal and reported them under the same name, so a chain that continued
from one kind of entry to the other broke instead. Both now fold into the
speculative chain; weights still follow the reported tier.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* fix\(kvcache\): check cancellation when the materialized match completes

The materialized matcher polls cancellation every 256 key positions, so
a request cancelled after its last checkpoint returned a match. A final
check at completion means a cancelled request never reports one.

Signed-off-by: nilig <nili.ifergan@gmail.com>

* fix\(kvcache\): count only keys a pod holds in block-hit telemetry

Index.Lookup is documented to return a map keyed by the requested keys,
so a backend may report a miss as an entry with no pods and
len\(keyToPods\) would publish it as a hit. ScoreTokens guarded on
len\(pods\) > 0 before the matcher took over the count, and #2670 commits
that the score_tokens attributes keep their meaning, so restore it.

InMemoryIndex reaches that shape under a race: Lookup reads cache.Len\(\)
and cache.Keys\(\) as separate locked calls, so evicting the last entry
between them leaves the unfiltered branch assigning an empty slice. A
stub index carries the test to make the case deterministic.

The benchmark fixture is named for what it exercises, endpoints
reporting many rank entries per block, rather than for the cluster it
was first measured on.

Signed-off-by: nilig <nili.ifergan@gmail.com>

---------

Signed-off-by: nilig <nili.ifergan@gmail.com>")  | Sep 4, 2026  |  
| View all files |  
## [README.md](https://github.com/llm-d/llm-d-router/tree/main/pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache#readme)
Outline
# Precise Prefix Cache Producer
**Type:** `precise-prefix-cache-producer`
DataProducer that owns the precise KV-block index and publishes per-endpoint `PrefixCacheMatchInfo`. Pairs with the generic [`prefix-cache-scorer`](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/scheduling/scorer/prefix); the scorer must reference this producer by name:

```
- type: prefix-cache-scorer
  parameters:
    prefixMatchInfoProducerName: precise-prefix-cache-producer
```

Without the `prefixMatchInfoProducerName` field, the scorer falls back to the auto-spawned approx producer.
Pipeline per request:
  * Consume `TokenizedPrompt` from `token-producer`.
  * Hash tokens → KV-block keys → `kvblock.Index.Lookup`.
  * Write `PrefixCacheMatchInfo(matchBlocks, totalBlocks, blockSizeTokens)` per endpoint, including the unweighted cached-block count and its per-device-tier breakdown.
  * (`PreRequest`) Speculative-index the selected endpoint(s) with TTL eviction.
  * (`EndpointExtractor`) Per-pod ZMQ subscriber lifecycle on add/delete.


Requires `TokenizedPrompt` on the request — set by a `token-producer` upstream. No-op otherwise.
## Parameters  
| Parameter  | Type  | Default  | Description  |  
| --- | --- | --- | --- |  
| `tokenProcessorConfig`  | object  | `kvblock.DefaultTokenProcessorConfig()`  | KV-block hashing for the EPP-recomputed keys (block size, hash seed).  |  
| `indexerConfig`  | object  | `kvcache.NewDefaultConfig()`  |  `kvcache.Indexer` config.  |  
| `kvEventsConfig`  | object  | `kvevents.DefaultConfig()`  | KV-events pool config.  |  
| `speculativeIndexing`  | bool  | `false`  | Seed predicted entries on routing decisions.  |  
| `speculativeTTL`  | duration  | TTL for speculative entries.  |  
Set `kvEventsConfig.engineType` to `sglang` for SGLang KV-events. It defaults to `vllm` when omitted.
Set `kvEventsConfig.tracing` to `true` to emit OpenTelemetry spans for the KV-event pipeline (`events_receive`, `events_process`, `events_decode`). It defaults to `false`: KV events arrive at many times the inference request rate, so with a shared head sampler always-on event spans crowd request traces out of the exported volume. The EPP `--tracing` flag gates tracing as a whole, so this field has no effect while that is off.
See [llm-d-kv-cache/docs/configuration.md](https://github.com/llm-d/llm-d-kv-cache/blob/main/docs/configuration.md) for nested parameter details.
## Engine compatibility
Block keys are recomputed by the EPP from `TokenizedPrompt` (tokens, model, multimodal features, cache salt) on both the lookup path and the KV-event ingestion path, using this plugin's `tokenProcessorConfig`. The engine's own block hashes serve only as opaque keys for the engine-to-request mapping, so `blockSizeTokens`/`hashSeed` need not match the engine.
The cross-engine requirement is that the engine emits, in its KV-events, the hash-affecting inputs the EPP hashes: `token_ids`, and `extra_keys` carrying multimodal identifiers and `cache_salt`. An input the engine omits from `extra_keys` is absent on the event side, so requests carrying it do not correlate.  
| Engine  |  `extra_keys` in KV-events  | `cache_salt`  |  
| --- | --- | --- |  
| vLLM  | emitted  | in block-0 `extra_keys`; salted prefixes isolated and precise-routed  |  
| SGLang  | not emitted  | baked into engine block hashes but not surfaced; salted requests are precise-cache misses until SGLang emits `extra_keys`  |  
Salt isolation is enforced by the engine regardless; the above affects only routing accuracy for salted requests.
You can’t perform that action at this time. 
