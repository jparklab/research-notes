---
title: 'perf(kvcache): match prefixes over an ordered index walk by nilig · Pull Request
  #2680 · llm-d/llm-d-router · GitHub'
id: perfkvcache-match-prefixes-over-an-ordered-index-walk-by-nilig-pull-request-2680
tags:
- llmd-kv-cache-routing-c665d5
- kv-block-index
- performance
- prefix-matching
- github-pr
created: '2026-09-14T11:56:59.066930Z'
updated: '2026-09-15T04:55:24.089309Z'
source: https://github.com/llm-d/llm-d-router/pull/2680
source_domain: github.com
fetched_at: '2026-09-14T11:56:59.045648Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Merged PR #2680 (llm-d/llm-d-router, merged Sep 9, 2026 by vMaroon) is the
  actual fix for both the InMemoryIndex.Lookup atomicity bug (issue #2698) and a major
  performance rework of prefix-cache matching. It introduces kvblock.KeyWalker, an
  optional Index capability that visits requested keys IN ORDER and lends each key''s
  records (with pod/tier ordinals) under that key''s lock, replacing the old pattern
  where the matcher materialized the full Lookup result (copying every key''s entries
  into a map before scoring). The precise-prefix matcher (Indexer.MatchBlockKeys)
  now walks when the backend supports KeyWalker and falls back to materialized Lookup
  for backends without it. Benchmarks on an AMD EPYC 9655P (16 CPUs, n=6): ScoreTokens
  at 240K tokens/96 pods dropped from 1,185.5ms/12.2GiB (original) to 68.9ms/90.7MiB
  (materialized baseline PR #2669) to 6.11ms/1.6MiB (this PR''s walk) -- roughly 190x
  latency and 7,600x memory reduction end to end; the multi-rank shape (1,034 blocks,
  40 endpoints) went from 551.1ms/4.6GiB to 85.6ms/111MiB to 1.56ms/418KiB. The rewritten
  Lookup also reads entries and their count in a single critical section under PodCache.mu
  via a new PodCache.filteredEntries method (closing #2698 without a separate patch).
  Telemetry semantics changed: score_tokens span attributes blocks_found and block_hit_ratio
  now report the longest CONTIGUOUS prefix one candidate endpoint holds (in block
  positions) instead of the count of distinct requested keys held; lookup_hits_total
  and max_pod_hit_count_total record this once per successful match under EnableMetrics
  rather than per direct Lookup call. Index key recency (LRU) is now refreshed once
  per read instead of once per key. A default of 10 pod entries per key applies when
  inMemoryConfig.podCacheSize is omitted or non-positive.'
---

*Suggested by [[kvblock-inmemoryindexlookup-tests-emptiness-and-reads-entries-without-the-pod-ca]] — PR that fixes the InMemoryIndex.Lookup atomicity bug described in issue 2698*

[Skip to content](https://github.com/llm-d/llm-d-router/pull/2680#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/2680) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/2680) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/2680) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2680).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  336 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Conversation
### 
commented [Sep 3, 2026](https://github.com/llm-d/llm-d-router/pull/2680#issue-5338340144) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2680).
Copy link  Copy Markdown 
Collaborator
**What type of PR is this?** /kind feature
**What this PR does / why we need it** : The matcher materializes `Lookup`: every request copies the entries of every key it reads into a map before scoring, which at fleet scale is tens of megabytes and most of the request time ([#2669](https://github.com/llm-d/llm-d-router/pull/2669) on the multi-rank shape: 84ms and 111 MiB per request on an EPYC node).
`kvblock.KeyWalker` is an optional `Index` capability that visits the requested keys in order and lends each key's records, with pod and tier ordinals, under that key's lock. Its contract is stated on the interface: borrowed-entry lifetime, no re-entry from `visit`, ordinal identity and stability across `Evict` and `Clear`, duplicate and missing positions, no filtering, per-key consistency without a cross-key snapshot, the cancellation polling points, and exactly which keys a walk promotes. `InMemoryIndex` implements it and stores `EntryRef` records, so nothing is hashed per entry; the traced and instrumented decorators expose the capability exactly when the wrapped index has it. `Indexer.MatchBlockKeys` walks when it can and materializes `Lookup` for other backends; both paths feed the same accumulator, and the property test checks both against the replaced algorithms. Request-local state is keyed by ordinal and never sized by it; the index caps the distinct pod identifiers and tiers it interns, and an `Add` past a cap fails before writing anything.
Recency stays exact and gets cheaper: the index owns the request-key LRU lock, reads peek under the shared lock, and every visited key is promoted under one exclusive acquisition when the walk or lookup ends, on every exit path. The contiguous-chain hit metrics are recorded by the matcher under `EnableMetrics` for successful matches rather than direct `Lookup` calls; `score_tokens` `blocks_found` and `block_hit_ratio` report the longest chain one candidate holds, which is as far as a walk reads.
AMD EPYC 9655P node, 16 exclusive CPUs, test binaries built once, six paired rounds in AB/BA order, n=6, per request:  
| Row  | main  | this PR  |  
| --- | --- | --- |  
|  `ScoreTokens`, 240K tokens, 96 pods  | 1,185.5ms / 12.2 GiB  | 68.9ms / 90.7 MiB  | 6.11ms / 1.6 MiB  |  
|  `ScoreTokens`, multi-rank shape (1,034 blocks, 40 endpoints, eight rank entries each)  | 551.1ms / 4.6 GiB  | 85.6ms / 111 MiB  | 1.56ms / 418 KiB  |  
| precise producer `Produce`, serial  | 152.2ms / 163 MiB  | 94.4ms / 143 MiB  | 1.56ms / 430 KiB  |  
| precise producer `Produce`, 16-way  | 37.3ms  | 22.5ms  | 230us  |  
| precise producer `Produce`, eight writers paced at 4,000 adds/s  | not run  | 101.6ms  | 2.02ms  |  
Against the head of the closed [#2402](https://github.com/llm-d/llm-d-router/pull/2402) (`bc40b33b`) in the same paired rounds: `ScoreTokens` at 96 pods -9.8%, multi-rank shape -36.0%; matcher on the multi-rank fixture -58.2% serial and -33.6% 16-way; producer -36.6% serial, -29.4% 16-way, -25.6% with eight paced writers at equal achieved load (3,958 vs 3,952 adds/s). All p=0.002. At an 8,000 adds/s offered rate this PR's writers reach 4% less load than [#2402](https://github.com/llm-d/llm-d-router/pull/2402)'s, because the request-key LRU is one lock where [#2402](https://github.com/llm-d/llm-d-router/pull/2402) shards it; recorded as a follow-up. Protocol and raw files: `test/precise-perf-run1/AUDIT-RESULTS-2026-09-03.md` on the fork.
**Which issue(s) this PR fixes** : Part of [#2670](https://github.com/llm-d/llm-d-router/issues/2670)
**Release note** :

```
The precise prefix matcher walks the in-memory KV-block index in place (`kvblock.KeyWalker`) instead of materializing `Lookup`. The `score_tokens` span attributes `blocks_found` and `block_hit_ratio` report the longest contiguous prefix one candidate endpoint holds, in block positions, instead of the number of distinct requested keys any candidate holds; `lookup_hits_total` and `max_pod_hit_count_total` record the longest contiguous prefix once per successful match under `EnableMetrics`. Direct `Lookup` calls and cancelled matches do not increment these counters. Index reads refresh key recency once per read instead of once per key.

`lookup_latency_seconds` measures the full `Lookup` or `WalkKeys` call, including walk callbacks. The matcher span uses `llm_d.kv_cache.prefix_match.longest_chain` for contiguous prefix length; `llm_d.kv_cache.prefix_match.keys_found` is not emitted.

An omitted or non-positive `inMemoryConfig.podCacheSize` selects 10 pod entries per key, so a configuration containing only `size` supports admissions.

```

Test plan:
  * in-memory clears finish under a cancelled context and preserve other pods
  * recovered walk callback panics leave the key usable for subsequent writes and walks
  * size-only in-memory configuration admits entries and enforces the default per-key capacity
  * walk contract: positions in order, misses, duplicate positions, early stop, cancellation before, during, and on short walks, ordinal stability across `Clear` and `Evict`, decorator truthfulness, concurrent readers and writers under `-race`
  * retention: a key that is only read stays resident; unvisited tails and keys after a cancellation checkpoint are not promoted
  * parity: walked and materialized paths equal the replaced algorithms on 500 random fixtures with random rank groups
  * hit metrics recorded once per match on both feeders and not at all when metrics are off; nil index config
  * cold state: request allocation independent of pod and tier ordinal history
  * `make presubmit`


[github-actions](https://github.com/apps/github-actions) Bot added [ area/epp ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fepp) [ area/scheduling ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fscheduling) [ area/kvcache ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fkvcache) [ size/XXL ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FXXL) Denotes a PR that changes 1000+ lines, ignoring generated files. [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. labels [Sep 3, 2026](https://github.com/llm-d/llm-d-router/pull/2680#event-30498039454)
This was referenced Sep 3, 2026
[ Precise prefix matching: separation of concerns and refactor plan #2670 ](https://github.com/llm-d/llm-d-router/issues/2670)
[ refactor(kvcache): fold prefix matching into one accumulator #2669 ](https://github.com/llm-d/llm-d-router/pull/2669)
Merged 
[github-actions](https://github.com/apps/github-actions) Bot added [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. and removed [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. labels [Sep 3, 2026](https://github.com/llm-d/llm-d-router/pull/2680#event-30510885937)
[nilig](https://github.com/nilig) [force-pushed](https://github.com/llm-d/llm-d-router/compare/c97736d85ea4b73efe107b5549a642e1a363dfd1..e907c8c323e2d7154bbfbc2bf297c738e8b44374) the  perf/prefix-matcher-b branch from [`c97736d`](https://github.com/llm-d/llm-d-router/commit/c97736d85ea4b73efe107b5549a642e1a363dfd1) to [`e907c8c`](https://github.com/llm-d/llm-d-router/commit/e907c8c323e2d7154bbfbc2bf297c738e8b44374) [ Compare ](https://github.com/llm-d/llm-d-router/compare/c97736d85ea4b73efe107b5549a642e1a363dfd1..e907c8c323e2d7154bbfbc2bf297c738e8b44374) [September 4, 2026 17:40](https://github.com/llm-d/llm-d-router/pull/2680#event-30571469857)
[github-actions](https://github.com/apps/github-actions) Bot added [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. and removed [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. labels [Sep 4, 2026](https://github.com/llm-d/llm-d-router/pull/2680#event-30571491912)
[nilig](https://github.com/nilig) mentioned this pull request [ Sep 4, 2026 ](https://github.com/llm-d/llm-d-router/pull/2680#ref-issue-5351242902)
[ kvblock: InMemoryIndex.Lookup tests emptiness and reads entries without the pod-cache lock #2698 ](https://github.com/llm-d/llm-d-router/issues/2698)
### 
commented [Sep 4, 2026](https://github.com/llm-d/llm-d-router/pull/2680#issuecomment-5544362435)
Copy link  Copy Markdown 
Collaborator Author  
|  Worth recording that this also closes [#2698](https://github.com/llm-d/llm-d-router/issues/2698): the rewritten `Lookup` reads entries and their count in one critical section under `PodCache.mu` via `PodCache.filteredEntries`, and stores the result only when the filtered slice is non-empty. The current head tests emptiness with `cache.Len()` and then reads `cache.Keys()` as a separate locked call, so an eviction of the last entry between them can leave an empty entry in the result and skip the prefix-chain cut. No separate fix filed, since it would land in the function this PR rewrites.  |  
| --- |  
### 
commented [Sep 4, 2026](https://github.com/llm-d/llm-d-router/pull/2680#issuecomment-5546236251)
Copy link  Copy Markdown 
Member  
|  pls rebase  |  
| --- |  
[nilig](https://github.com/nilig) added 3 commits [September 5, 2026 06:53](https://github.com/llm-d/llm-d-router/pull/2680#commits-pushed-a5cfc7b)
` perf(kvcache): match prefixes over an ordered index walk[](https://github.com/llm-d/llm-d-router/pull/2680/commits/a5cfc7b1d727e3bc7dc83c767beae9c345185f62 "perf\(kvcache\): match prefixes over an ordered index walk

The matcher materializes Lookup: every request copies the entries of
every key it reads into a map before scoring, which at fleet scale is
tens of megabytes and most of the request time. KeyWalker is an optional
Index capability that visits the requested keys in order and lends each
key's records, with pod and tier ordinals, under that key's lock. The
matcher folds them straight into its accumulator and materializes Lookup
only for backends without the capability. The traced and instrumented
decorators expose the capability exactly when the wrapped index has it,
and pod caches store records with the ordinals the walk hands out, so
nothing is hashed per entry. Request-local state is keyed by ordinal and
never sized by it; the index caps the distinct pod identifiers and tiers
it assigns ordinals to, and an Add past a cap fails before it writes
anything.

Recency stays exact and gets cheaper: the index owns the request-key LRU
lock, reads peek under the shared lock, and every visited key is promoted
under one exclusive acquisition when the walk or lookup ends, on every
exit path, instead of one per key. The contiguous-chain hit metrics are
recorded by the matcher, where the chain is known, under the same
EnableMetrics option that instruments the index, so the decorators keep
storage facts only; the score_tokens block_hit_ratio and blocks_found
report the longest chain one candidate holds, which is as far as a walk
reads.

AMD EPYC 9655P, 16 CPUs, n=6, ScoreTokens per request: 240K tokens at 96
pods 69.2ms and 90.7MiB with the materialized matcher, 6.1ms and 1.6MiB
with the walk; Kermit shape \(1,034 blocks, 40 endpoints, eight rank
entries each\) 86.5ms and 113.8MiB to 1.6ms and 418KiB. At the matcher
boundary on that shape, sixteen concurrent walks take 137us each.

Signed-off-by: nilig <nili.ifergan@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[nilig](https://github.com/nilig) Nili Guy 
SSH Key Fingerprint: 8yE0dIS2bGRG9GqoR4gZS5TDTPog1Gfjb0xD6zqssyA
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
The matcher materializes Lookup: every request copies the entries of
every key it reads into a map before scoring, which at fleet scale is
tens of megabytes and most of the request time. KeyWalker is an optional
Index capability that visits the requested keys in order and lends each
key's records, with pod and tier ordinals, under that key's lock. The
matcher folds them straight into its accumulator and materializes Lookup
only for backends without the capability. The traced and instrumented
decorators expose the capability exactly when the wrapped index has it,
and pod caches store records with the ordinals the walk hands out, so
nothing is hashed per entry. Request-local state is keyed by ordinal and
never sized by it; the index caps the distinct pod identifiers and tiers
it assigns ordinals to, and an Add past a cap fails before it writes
anything.

Recency stays exact and gets cheaper: the index owns the request-key LRU
lock, reads peek under the shared lock, and every visited key is promoted
under one exclusive acquisition when the walk or lookup ends, on every
exit path, instead of one per key. The contiguous-chain hit metrics are
recorded by the matcher, where the chain is known, under the same
EnableMetrics option that instruments the index, so the decorators keep
storage facts only; the score_tokens block_hit_ratio and blocks_found
report the longest chain one candidate holds, which is as far as a walk
reads.

AMD EPYC 9655P, 16 CPUs, n=6, ScoreTokens per request: 240K tokens at 96
pods 69.2ms and 90.7MiB with the materialized matcher, 6.1ms and 1.6MiB
with the walk; Kermit shape (1,034 blocks, 40 endpoints, eight rank
entries each) 86.5ms and 113.8MiB to 1.6ms and 418KiB. At the matcher
boundary on that shape, sixteen concurrent walks take 137us each.

Signed-off-by: nilig <nili.ifergan@gmail.com>
```

` docs(kvblock): state the KeyWalker contract[](https://github.com/llm-d/llm-d-router/pull/2680/commits/11746ca126b7a8aa6b4a3528c65a73bdd5485a57 "docs\(kvblock\): state the KeyWalker contract

Borrowed-entry lifetime, re-entry, ordinal identity and stability,
duplicate and missing positions, filtering, consistency under concurrent
writes, cancellation polling, and what a walk promotes were readable only
from the in-memory implementation. The interface states them, and tests
pin duplicate positions and ordinal stability across Clear and Evict.

Signed-off-by: nilig <nili.ifergan@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[nilig](https://github.com/nilig) Nili Guy 
SSH Key Fingerprint: 8yE0dIS2bGRG9GqoR4gZS5TDTPog1Gfjb0xD6zqssyA
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Borrowed-entry lifetime, re-entry, ordinal identity and stability,
duplicate and missing positions, filtering, consistency under concurrent
writes, cancellation polling, and what a walk promotes were readable only
from the in-memory implementation. The interface states them, and tests
pin duplicate positions and ordinal stability across Clear and Evict.

Signed-off-by: nilig <nili.ifergan@gmail.com>
```

` docs(kvblock): narrow walk consistency guarantees[](https://github.com/llm-d/llm-d-router/pull/2680/commits/55883ac5221d0b5ea0584ce5230a69b63e65fdea "docs\(kvblock\): narrow walk consistency guarantees

Signed-off-by: nilig <nili.ifergan@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[nilig](https://github.com/nilig) Nili Guy 
SSH Key Fingerprint: 8yE0dIS2bGRG9GqoR4gZS5TDTPog1Gfjb0xD6zqssyA
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2680).

```
Signed-off-by: nilig <nili.ifergan@gmail.com>
```

[nilig](https://github.com/nilig) [force-pushed](https://github.com/llm-d/llm-d-router/compare/e907c8c323e2d7154bbfbc2bf297c738e8b44374..55883ac5221d0b5ea0584ce5230a69b63e65fdea) the  perf/prefix-matcher-b branch from [`e907c8c`](https://github.com/llm-d/llm-d-router/commit/e907c8c323e2d7154bbfbc2bf297c738e8b44374) to [`55883ac`](https://github.com/llm-d/llm-d-router/commit/55883ac5221d0b5ea0584ce5230a69b63e65fdea) [ Compare ](https://github.com/llm-d/llm-d-router/compare/e907c8c323e2d7154bbfbc2bf297c738e8b44374..55883ac5221d0b5ea0584ce5230a69b63e65fdea) [September 5, 2026 03:57](https://github.com/llm-d/llm-d-router/pull/2680#event-30594772865)
[nilig](https://github.com/nilig) marked this pull request as ready for review [September 5, 2026 04:00](https://github.com/llm-d/llm-d-router/pull/2680#event-30594847575)
[nilig](https://github.com/nilig) requested review from a team, [hyeongyun0916](https://github.com/hyeongyun0916), [liu-cong](https://github.com/liu-cong), [sagearc](https://github.com/sagearc), [vMaroon](https://github.com/vMaroon) and [yankay](https://github.com/yankay) as [code owners](https://github.com/llm-d/llm-d-router/blob/4f8e571188949b1c666f13c1b32daab5ceba1b03/CODEOWNERS#L61) [September 5, 2026 04:00](https://github.com/llm-d/llm-d-router/pull/2680#event-30594847988)
### 
commented [Sep 5, 2026](https://github.com/llm-d/llm-d-router/pull/2680#issuecomment-5549211883)
Copy link  Copy Markdown 
Collaborator Author  
|  Rebased onto `main` in [`55883ac`](https://github.com/llm-d/llm-d-router/commit/55883ac5221d0b5ea0584ce5230a69b63e65fdea) after [#2669](https://github.com/llm-d/llm-d-router/pull/2669) merged.  |  
| --- |  
[github-actions](https://github.com/apps/github-actions) Bot removed the [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. label [Sep 5, 2026](https://github.com/llm-d/llm-d-router/pull/2680#event-30602498570)
[github-actions](https://github.com/apps/github-actions) Bot added the [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. label [Sep 5, 2026](https://github.com/llm-d/llm-d-router/pull/2680#event-30602499126)
[yankay](https://github.com/yankay) requested a lite review from [Copilot](https://github.com/apps/copilot-pull-request-reviewer) [September 7, 2026 10:27](https://github.com/llm-d/llm-d-router/pull/2680#event-30683954949)
**Copilot** [started reviewing](https://github.com/llm-d/llm-d-router/sessions/381fdc10-b986-4c30-9a0a-96dda9da3868 "View session") on behalf of [yankay](https://github.com/yankay) [September 7, 2026 10:28](https://github.com/llm-d/llm-d-router/pull/2680#event-30683984623) [ View session ](https://github.com/llm-d/llm-d-router/sessions/381fdc10-b986-4c30-9a0a-96dda9da3868)
reviewed  [ Sep 7, 2026 ](https://github.com/llm-d/llm-d-router/pull/2680#pullrequestreview-5130955621)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/2680/files/55883ac5221d0b5ea0584ce5230a69b63e65fdea)
### 
left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
### 🟡 Changes recommended
Metric help text/docs in `pkg/kvcache/metrics/collector.go` still describe the counters as “per lookup” even though this PR records them per prefix match (and counts WalkKeys as a lookup request).
_Once you've addressed the issues Copilot identified, you can request another Copilot review._
Pull request overview
This PR introduces an ordered index-walk capability (`kvblock.KeyWalker`) and updates the precise prefix matcher to consume entries via a lock-borrowed, in-order walk when available, falling back to materialized `Lookup` for non-walker backends. This fits the router’s kvcache layer by reducing per-request allocations and time spent materializing lookup maps while keeping matcher semantics unified via the shared accumulator.
**Changes:**
  * Add `kvblock.KeyWalker` / `EntryRef` and implement ordered walking + batched LRU recency promotion in `InMemoryIndex`, with decorator propagation for the capability.
  * Update `kvcache.Indexer.MatchBlockKeys` to walk when possible and to record contiguous-prefix hit metrics in the matcher (instead of in the index instrumentation).
  * Expand parity/contract tests and add targeted benchmarks for walked vs materialized paths and mixed reader/writer workloads.

File summaries  
| File  | Description  |  
| --- | --- |  
| pkg/kvcache/README.md  | Documents matcher’s walked-vs-materialized behavior and where hit metrics are recorded.  |  
| pkg/kvcache/prefix_match.go  | Switches matcher to walk when possible; refactors feeder API to `acc.key(entries)`; records hit counters here.  |  
| pkg/kvcache/prefix_match_test.go  | Adds dual-path (walked + materialized) assertions and matcher-side hit-metric tests.  |  
| pkg/kvcache/prefix_match_internal_test.go  | Adds cold-state allocation regression tests for ordinal churn.  |  
| pkg/kvcache/prefix_match_bench_test.go  | Adds walked/materialized/parallel/mixed benchmarks for the matcher.  |  
| pkg/kvcache/multi_rank_prefix_bench_test.go  | Ensures production decorator chain preserves `KeyWalker` so benchmarks measure the intended path.  |  
| pkg/kvcache/metrics/collector.go  | Updates comments around contiguous-prefix counters (but help strings still mention “lookup”).  |  
| pkg/kvcache/kvblock/walk.go  | Defines `EntryRef` and `KeyWalker` contract; implements `InMemoryIndex.WalkKeys`.  |  
| pkg/kvcache/kvblock/walk_test.go  | Adds walk contract, retention, decorator-capability, ordinal stability, and concurrency tests.  |  
| pkg/kvcache/kvblock/lru_store.go  | Introduces an RWMutex-guarded LRU to enable batched promotion.  |  
| pkg/kvcache/kvblock/in_memory.go  | Adds ordinal interners and caps; refactors per-key storage; aligns Lookup cancellation/promotion with walk behavior.  |  
| pkg/kvcache/kvblock/traced_index.go  | Preserves `KeyWalker` capability and adds `index_walk` tracing span.  |  
| pkg/kvcache/kvblock/instrumented_index.go  | Preserves `KeyWalker` capability and records read metrics for WalkKeys (hit metrics moved to matcher).  |  
| pkg/kvcache/kvblock/interner_internal_test.go  | Verifies Add fails atomically when interner caps would be exceeded.  |  
| pkg/kvcache/kvblock/in_memory_bench_test.go  | Adds index-level benchmarks for Lookup/Add/WalkKeys and mixed workloads.  |  
| pkg/kvcache/indexer.go  | Wires `keyWalker` detection, hit-metric enablement, and updates `ScoreTokens` telemetry semantics.  |  
| pkg/kvcache/indexer_trace_test.go  | Updates tracing assertions for new `blocks_found` semantics and walked/materialized signaling.  |  
| pkg/kvcache/indexer_test.go  | Adds coverage for nil KVBlockIndexConfig defaulting behavior.  |  
| pkg/kvcache/export_test.go  | Adjusts test constructors to optionally disable hit-metric recording.  |  
Review details
### Suppressed comments (2)
**pkg/kvcache/metrics/collector.go:123**
  * The MaxPodHitCount metric help string still says "per lookup", but this PR changes the semantics to record once per prefix match (MatchBlockKeys). The help string should be updated to match the new behavior.



```
	// MaxPodHitCount counts, per prefix match, the longest contiguous prefix
	// chain any single pod holds counting from the first requested block.
	MaxPodHitCount = newDualCounter("index", "max_pod_hit_count_total",
		"kv_cache_index_max_pod_hit_count_total", "Longest contiguous per-pod prefix chain observed per lookup")
	// LookupHits accumulates the same per-match contiguous chain length as

```

**pkg/kvcache/metrics/collector.go:126**
  * The LookupHits metric help string still says "per lookup", but this PR records the value once per prefix match. Aligning the help string with the new semantics will prevent misinterpretation.



```
	// LookupHits accumulates the same per-match contiguous chain length as
	// MaxPodHitCount.
	LookupHits = newDualCounter("index", "lookup_hits_total",
		"kv_cache_index_lookup_hits_total", "Contiguous prefix blocks matched by the best pod per lookup")

```

  * **Files reviewed:** 21/21 changed files
  * **Comments generated:** 1
  * **Review effort level:** Lite


💡 [Add a `code-review` agent skill](https://github.com/llm-d/llm-d-router/new/main?filename=.github/skills/code-review/SKILL.md) or configure MCP servers for context-aware, tailored reviews. [Learn more in the docs.](https://docs.github.com/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review?tool=webui#mcp-servers-and-agent-skills)
Positive feedbackNegative feedback
Comment thread
Comment on lines to  +120   
|  // LookupRequests counts how many Lookup() calls have been made.  |  
| --- |  
|  LookupRequests = newDualCounter("index", "lookup_requests_total",  |  
|  "kv_cache_index_lookup_requests_total", "Total number of lookup calls")  |  
|  // MaxPodHitCount counts, per lookup, the longest contiguous prefix chain  |  
|  // any single pod holds counting from the first requested block.  |  
|  // MaxPodHitCount counts, per prefix match, the longest contiguous prefix  |  
|  // chain any single pod holds counting from the first requested block.  |  
### Copilot commented on Sep 7, 2026 
CopilotAI
More actions
The LookupRequests comment/help text is now inaccurate: WalkKeys is also counted as a lookup request (instrumentedWalker.WalkKeys increments LookupRequests). Updating the metric description avoids confusion when interpreting dashboards.
This issue also appears in the following locations of the same file:
  * line 119
  * line 123


Positive feedbackNegative feedback
Copilot uses AI. Check for mistakes.
` fix(kvblock): preserve clear semantics and unlock after panic[](https://github.com/llm-d/llm-d-router/pull/2680/commits/9ce2fa7ef60e7848d847df6c97e9b3851950bb03 "fix\(kvblock\): preserve clear semantics and unlock after panic

A recovered walk callback must leave its key usable. In-memory clears
must complete for cancelled callers.

Cover these contracts and keep concurrent test failures bounded.
Document walk telemetry and the default pod-cache capacity.

Signed-off-by: nilig <nili.ifergan@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[nilig](https://github.com/nilig) Nili Guy 
SSH Key Fingerprint: 8yE0dIS2bGRG9GqoR4gZS5TDTPog1Gfjb0xD6zqssyA
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2680).

```
A recovered walk callback must leave its key usable. In-memory clears
must complete for cancelled callers.

Cover these contracts and keep concurrent test failures bounded.
Document walk telemetry and the default pod-cache capacity.

Signed-off-by: nilig <nili.ifergan@gmail.com>
```

[github-actions](https://github.com/apps/github-actions) Bot added [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. and removed [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. labels [Sep 9, 2026](https://github.com/llm-d/llm-d-router/pull/2680#event-30801875456)
[nilig](https://github.com/nilig) mentioned this pull request [ Sep 9, 2026 ](https://github.com/llm-d/llm-d-router/pull/2680#ref-pullrequest-5335544141)
[ feat(kvcache): batch Index.Evict to replace per-key eviction loops #2673 ](https://github.com/llm-d/llm-d-router/pull/2673)
approved these changes  [ Sep 9, 2026 ](https://github.com/llm-d/llm-d-router/pull/2680#pullrequestreview-5152169235)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/2680/files/9ce2fa7ef60e7848d847df6c97e9b3851950bb03)
### 
left a comment 
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Reviewed and tested `9ce2fa7ef60e7848d847df6c97e9b3851950bb03`. The `Clear` cancellation and callback panic-unlock concerns are addressed. No remaining merge-blocking findings from this review.
Local verification passed:

```
go test -race -count=1 ./pkg/kvcache/... ./pkg/kvevents/... \
  ./pkg/epp/framework/plugins/requestcontrol/dataproducer/preciseprefixcache \
  ./pkg/epp/framework/plugins/scheduling/scorer/preciseprefixcache
```

  * Regression coverage verifies that cancelled clears remove the target pod across tiers while preserving other pods, recovered callback panics permit subsequent writes and walks, and size-only configuration enforces the default per-key capacity.
  * Hermetic EPP integration tests passed with `-race`: endpoint-score metadata and full-duplex requests/responses in standard mode and standalone modes with and without CRDs.
  * An additional review-only integration test sent 29 tokenized requests over gRPC to a running EPP and delivered synthetic KV events over real ZeroMQ. Scores and selected endpoints matched expectations after stores, cache-group-specific removals, middle-block eviction, CPU-tier stores/removals, endpoint clears, and rebuilding after clear. Tracing confirmed the ordered-walk path was exercised.


Runtime validation used synthetic backend fixtures and events. It did not include Envoy forwarding to live vLLM/GPU servers, model generation, or events emitted by a live inference engine.
Hide details View details
[vMaroon](https://github.com/vMaroon) merged commit [`3a1c181`](https://github.com/llm-d/llm-d-router/commit/3a1c18122f67e53d4ac989e56bff41e13b2b7ed9) into  llm-d:main [Sep 9, 2026](https://github.com/llm-d/llm-d-router/pull/2680#event-30816504606)
41 checks passed 
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2680).
[llm-d-router-release-notes](https://github.com/apps/llm-d-router-release-notes) Bot mentioned this pull request [ Sep 9, 2026 ](https://github.com/llm-d/llm-d-router/pull/2680#ref-pullrequest-5397462858)
[ docs: add release-note fragment for PR #2680 #2771 ](https://github.com/llm-d/llm-d-router/pull/2771)
Closed 
[yankay](https://github.com/yankay) mentioned this pull request [ Sep 11, 2026 ](https://github.com/llm-d/llm-d-router/pull/2680#ref-pullrequest-5363611890)
[ fix(kvblock): early-stop lookup on missing key or empty filtered pods #2713 ](https://github.com/llm-d/llm-d-router/pull/2713)
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fllm-d%2Fllm-d-router%2Fpull%2F2680)
###  Labels 
[ area/epp ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fepp) [ area/kvcache ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fkvcache) [ area/scheduling ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fscheduling) [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. [ size/XXL ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FXXL) Denotes a PR that changes 1000+ lines, ignoring generated files.
###  3 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
