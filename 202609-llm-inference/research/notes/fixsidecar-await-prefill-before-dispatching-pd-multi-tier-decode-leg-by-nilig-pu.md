---
title: 'fix(sidecar): await prefill before dispatching PD Multi Tier decode leg by
  nilig · Pull Request #2451 · llm-d/llm-d-router · GitHub'
id: fixsidecar-await-prefill-before-dispatching-pd-multi-tier-decode-leg-by-nilig-pu
tags:
- llm-d
- kv-transfer-bug
- pd-disaggregation
- llmd-kv-cache-routing-c665d5
created: '2026-09-14T11:56:44.337942Z'
updated: '2026-09-15T04:55:24.082687Z'
source: https://github.com/llm-d/llm-d-router/pull/2451
source_domain: github.com
fetched_at: '2026-09-14T11:56:44.317286Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Merged llm-d-router PR #2451 (by nilig, Aug 19 2026) fixing issue #2438:
  changes handleP2P (pkg/sidecar/proxy/connector_p2p.go) so the prefill leg runs to
  completion before the decode leg is dispatched, returning the prefill error verbatim
  on failure so decode is never dispatched against KV that was never stored. Matches
  the nixlv2 connector''s existing sequential ordering (concurrent dispatch there
  is opt-in, gated behind MoRIIOParallelDispatch && MoRIIOWriteMode). Confirms the
  same before/after measurements as issue #2438 (supply delay mean 15.830s->0.002s;
  consumer load latency mean 14.387s->0.291s on 8 prefill + 8 decode gpt-oss-120b,
  TP=1, vLLM v0.27.1, 16x H200) and explicitly notes the fix does not eliminate all
  timeouts -- a producer that already holds requested blocks still supplies nothing
  regardless of dispatch ordering, tracked as vllm-project/vllm#52808. Test plan confirms
  decode request count stays zero until prefill releases, then fires exactly once.'
---

*Suggested by [[sidecar-pd-multi-tier-decode-leg-is-dispatched-concurrently-with-prefill-chargin]] — the fix PR for issue 2438*

[Skip to content](https://github.com/llm-d/llm-d-router/pull/2451#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/2451) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/2451) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/2451) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2451).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  336 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Conversation
### 
commented [Aug 19, 2026](https://github.com/llm-d/llm-d-router/pull/2451#issue-5191256545)
Copy link  Copy Markdown 
Collaborator
**What type of PR is this?**
/kind bug
**What this PR does / why we need it** :
The PD Multi Tier decode leg pulls KV from the prefiller's secondary tier. `handleP2P` dispatched both legs concurrently, so the decoder's fetch reached the prefiller before any blocks were stored for that `kv_request_id`. The prefiller parks that demand awaiting supply, and the decoder spends its fixed load deadline (`_LOAD_TIMEOUT_S`, 30s in vLLM) waiting for KV that has not been produced yet. On expiry it aborts the load and, under `kv_load_failure_policy: recompute`, recomputes the prompt locally — the work disaggregation exists to avoid.
This runs the prefill leg to completion first and returns the prefill error verbatim if it fails, so decode is never dispatched against KV that was never stored. It matches the `nixlv2` path, which already forwards to the prefiller and waits for it to return; concurrent dispatch there is opt-in behind `MoRIIOParallelDispatch && MoRIIOWriteMode`.
Measured on 8 prefill + 8 decode (gpt-oss-120b, TP=1, vLLM v0.27.1, 16x H200), instrumenting the vLLM Multi Tier session:  
| concurrent  | sequential  |  
| --- | --- |  
| supply delay mean  | 15.830 s  | 0.002 s  |  
| supply delay max  | 29.920 s  | 0.234 s  |  
| consumer load latency mean  | 14.387 s  | 0.291 s  |  
This does not remove every timeout: a producer that already holds the requested blocks supplies nothing regardless of ordering, tracked upstream as [vllm-project/vllm#52808](https://github.com/vllm-project/vllm/issues/52808).
**Which issue(s) this PR fixes** :
Fixes [#2438](https://github.com/llm-d/llm-d-router/issues/2438)
**Test plan** :
  * Decode is not dispatched until the prefill leg returns (prefill held open; decode request count stays zero, then fires exactly once on release).
  * A failing prefill is returned to the caller verbatim and decode is never dispatched.
  * Existing PD Multi Tier `kv_transfer_params` coverage still passes.


Both new specs were confirmed to fail against the previous concurrent implementation and pass with this change.
**Release note** _(write`NONE` if no user-facing change)_:

```
Fixed the PD Multi Tier decode leg being dispatched before the prefiller had stored its KV, which charged prefill latency against the KV load deadline and caused requests to time out and recompute under load.

```

` fix(sidecar): await prefill before dispatching PD Multi Tier decode leg[](https://github.com/llm-d/llm-d-router/pull/2451/commits/82f601b639bf9a37ce749d1ebdcc971832400cb4 "fix\(sidecar\): await prefill before dispatching PD Multi Tier decode leg

The PD Multi Tier decode leg pulls KV from the prefiller's secondary tier.
Dispatching it concurrently with prefill means its fetch reaches the
prefiller before any blocks are stored, so the demand parks unsatisfied and
consumes the connector's fixed load deadline waiting for KV that has not been
produced. On expiry the decoder aborts the load and recomputes the prompt
locally, which is the work disaggregation exists to avoid.

Run the prefill leg to completion first, as the NIXL connector already does.

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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2451).

```
The PD Multi Tier decode leg pulls KV from the prefiller's secondary tier.
Dispatching it concurrently with prefill means its fetch reaches the
prefiller before any blocks are stored, so the demand parks unsatisfied and
consumes the connector's fixed load deadline waiting for KV that has not been
produced. On expiry the decoder aborts the load and recomputes the prompt
locally, which is the work disaggregation exists to avoid.

Run the prefill leg to completion first, as the NIXL connector already does.

Signed-off-by: nilig <nili.ifergan@gmail.com>
```

[nilig](https://github.com/nilig) requested review from a team, [roytman](https://github.com/roytman) and [shmuelk](https://github.com/shmuelk) as [code owners](https://github.com/llm-d/llm-d-router/blob/ef296028a43f7a00f67fc11d2e90cb45d8fd1dd2/CODEOWNERS#L30) [August 19, 2026 09:45](https://github.com/llm-d/llm-d-router/pull/2451#event-29673675380)
[nilig](https://github.com/nilig) requested review from [elevran](https://github.com/elevran) and [hexfusion](https://github.com/hexfusion) [August 19, 2026 09:45](https://github.com/llm-d/llm-d-router/pull/2451#event-29673675960)
[github-actions](https://github.com/apps/github-actions) Bot added [ size/L ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files. [ kind/bug ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Fbug) Categorizes issue or PR as related to a bug. labels [Aug 19, 2026](https://github.com/llm-d/llm-d-router/pull/2451#event-29673699212)
[nilig](https://github.com/nilig) requested a review from [vMaroon](https://github.com/vMaroon) [August 19, 2026 10:56](https://github.com/llm-d/llm-d-router/pull/2451#event-29677004682)
` test(sidecar): use method value in Eventually to satisfy gocritic[](https://github.com/llm-d/llm-d-router/pull/2451/commits/a2b45c1236d83aedfe22cfb53bfb7a360eccf896 "test\(sidecar\): use method value in Eventually to satisfy gocritic

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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2451).

```
Signed-off-by: nilig <nili.ifergan@gmail.com>
```

[nilig](https://github.com/nilig) added a commit to nilig/llm-d-router that referenced this pull request [ Aug 20, 2026 ](https://github.com/llm-d/llm-d-router/pull/2451#ref-commit-d4aa234)
` test(p2p-findings): PD Multi Tier Workload 3 nightly matched set[](https://github.com/nilig/llm-d-router/commit/d4aa23438363b61c7f4c51684ba21c1ef2feb428 "test\(p2p-findings\): PD Multi Tier Workload 3 nightly matched set

Three-arm rerun on vLLM nightly-5a4c8d99 with the sequential-dispatch
sidecar \(router #2451\) and vllm#52912 mounted in the PD Multi Tier arm.
PD Multi Tier goes from collapse on v0.27.1 \(5.9 req/s, TTFT P90 30-102s\)
to parity with local CPU offload \(10.9 req/s, TTFT P90 0.78s, zero
cannot-store-chunks\). Needs vllm#51840 + vllm#52912 + router #2451.

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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2451).

```
Three-arm rerun on vLLM nightly-5a4c8d99 with the sequential-dispatch
sidecar (router llm-d#2451[](https://github.com/llm-d/llm-d-router/pull/2451)) and vllm#52912 mounted in the PD Multi Tier arm.
PD Multi Tier goes from collapse on v0.27.1 (5.9 req/s, TTFT P90 30-102s)
to parity with local CPU offload (10.9 req/s, TTFT P90 0.78s, zero
cannot-store-chunks). Needs vllm#51840 + vllm#52912 + router llm-d#2451[](https://github.com/llm-d/llm-d-router/pull/2451).

Signed-off-by: nilig <nili.ifergan@gmail.com>
```

reviewed  [ Aug 20, 2026 ](https://github.com/llm-d/llm-d-router/pull/2451#pullrequestreview-4979975504)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/2451/files/a2b45c1236d83aedfe22cfb53bfb7a360eccf896)
Comment thread
### 
[Aug 20, 2026](https://github.com/llm-d/llm-d-router/pull/2451#discussion_r3819356796)
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Q. This doc comment got dropped in this PR. p2pPullAvailable itself isn't otherwise touched by this fix, so this looks like accidental collateral damage rather than an intended edit or a reduction of the comments. Mind restoring it?
### 
Copy link  Copy Markdown 
Collaborator Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Good catch, that was accidental - the comment and its preceding blank line got swallowed when the concurrent helper was replaced. Restored verbatim in `e756fc43`.
I also re-checked the rest of the diff for the same problem; everything else removed is the old concurrent path (its doc comment described concurrent dispatch, so it was replaced rather than dropped) and the now-unused `context` import.
` docs(sidecar): restore p2pPullAvailable doc comment[](https://github.com/llm-d/llm-d-router/pull/2451/commits/e756fc43bafaf1b6fc1f478e2435a312f0a1ea64 "docs\(sidecar\): restore p2pPullAvailable doc comment

The comment was dropped while replacing the concurrent-dispatch helper;
p2pPullAvailable itself is unchanged by this PR.

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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2451).

```
The comment was dropped while replacing the concurrent-dispatch helper;
p2pPullAvailable itself is unchanged by this PR.

Signed-off-by: nilig <nili.ifergan@gmail.com>
```

[github-actions](https://github.com/apps/github-actions) Bot added the [ area/sidecar ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fsidecar) label [Aug 20, 2026](https://github.com/llm-d/llm-d-router/pull/2451#event-29729130553)
previously approved these changes  [ Aug 20, 2026 ](https://github.com/llm-d/llm-d-router/pull/2451#pullrequestreview-4980464763)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/2451/files/e756fc43bafaf1b6fc1f478e2435a312f0a1ea64)
[nilig](https://github.com/nilig) mentioned this pull request [ Aug 20, 2026 ](https://github.com/llm-d/llm-d-router/pull/2451#ref-issue-5205336131)
[ [Bug][KVOffload] PD Multi Tier discards parked KV after a fixed 60s, so a queued consumer recomputes vllm-project/vllm#53128 ](https://github.com/vllm-project/vllm/issues/53128)
[vMaroon](https://github.com/vMaroon) mentioned this pull request [ Aug 22, 2026 ](https://github.com/llm-d/llm-d-router/pull/2451#ref-issue-5221182634)
[ [1.0] Precise prefix-cache routing tracker #2513 ](https://github.com/llm-d/llm-d-router/issues/2513)
59 tasks
` Merge main and resolve P2P sequential dispatch conflict[](https://github.com/llm-d/llm-d-router/pull/2451/commits/488082308725b6e4e990aa57291d3671affea33e "Merge main and resolve P2P sequential dispatch conflict

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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2451).

```
Signed-off-by: nilig <nili.ifergan@gmail.com>
```

[nilig](https://github.com/nilig) dismissed [roytman](https://github.com/roytman)’s [stale review](https://github.com/llm-d/llm-d-router/pull/2451#pullrequestreview-4980464763) via [September 5, 2026 09:59](https://github.com/llm-d/llm-d-router/pull/2451#event-30603152312)
approved these changes  [ Sep 5, 2026 ](https://github.com/llm-d/llm-d-router/pull/2451#pullrequestreview-5120827548)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/2451/files/488082308725b6e4e990aa57291d3671affea33e)
Hide details View details
[vMaroon](https://github.com/vMaroon) merged commit [`291d508`](https://github.com/llm-d/llm-d-router/commit/291d5082494c5f141a4cdb3a74a756345b9dc329) into  llm-d:main [Sep 5, 2026](https://github.com/llm-d/llm-d-router/pull/2451#event-30607460854)
38 checks passed 
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2451).
[llm-d-router-release-notes](https://github.com/apps/llm-d-router-release-notes) Bot mentioned this pull request [ Sep 5, 2026 ](https://github.com/llm-d/llm-d-router/pull/2451#ref-pullrequest-5358137136)
[ docs: add release-note fragment for PR #2451 #2711 ](https://github.com/llm-d/llm-d-router/pull/2711)
Merged 
[elevran](https://github.com/elevran) pushed a commit that referenced this pull request [ Sep 6, 2026 ](https://github.com/llm-d/llm-d-router/pull/2451#ref-commit-e66795f)
` docs: add release-note fragment for PR[](https://github.com/llm-d/llm-d-router/commit/e66795f6905d4d43d08d78c0b231db1ece29fbbf "docs: add release-note fragment for PR #2451 \(#2711\)

Signed-off-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>
Co-authored-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>") #2451[](https://github.com/llm-d/llm-d-router/pull/2451) ([](https://github.com/llm-d/llm-d-router/commit/e66795f6905d4d43d08d78c0b231db1ece29fbbf "docs: add release-note fragment for PR #2451 \(#2711\)

Signed-off-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>
Co-authored-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>")#2711[](https://github.com/llm-d/llm-d-router/pull/2711))[](https://github.com/llm-d/llm-d-router/commit/e66795f6905d4d43d08d78c0b231db1ece29fbbf "docs: add release-note fragment for PR #2451 \(#2711\)

Signed-off-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>
Co-authored-by: llm-d-router-release-notes\[bot\] <287676111+llm-d-router-release-notes\[bot\]@users.noreply.github.com>") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2451).

```
Signed-off-by: llm-d-router-release-notes[bot] <287676111+llm-d-router-release-notes[bot]@users.noreply.github.com>
Co-authored-by: llm-d-router-release-notes[bot] <287676111+llm-d-router-release-notes[bot]@users.noreply.github.com>
```

[elevran](https://github.com/elevran) mentioned this pull request [ Sep 8, 2026 ](https://github.com/llm-d/llm-d-router/pull/2451#ref-pullrequest-5265894680)
[ refactor(sidecar): extract shared concurrent prefill/decode dispatch helper #2589 ](https://github.com/llm-d/llm-d-router/pull/2589)
Merged 
2 tasks
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fllm-d%2Fllm-d-router%2Fpull%2F2451)
###  Labels 
[ area/sidecar ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fsidecar) [ kind/bug ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Fbug) Categorizes issue or PR as related to a bug. [ size/L ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files.
###  3 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
