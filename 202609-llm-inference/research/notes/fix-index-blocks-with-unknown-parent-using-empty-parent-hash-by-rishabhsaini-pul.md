---
title: 'fix: index blocks with unknown parent using empty parent hash by RishabhSaini
  · Pull Request #663 · llm-d/llm-d-kv-cache · GitHub'
id: fix-index-blocks-with-unknown-parent-using-empty-parent-hash-by-rishabhsaini-pul
tags:
- llmd-kv-cache-routing-c665d5
- kv-block-index
- failure-recovery
- github-pr
- closed-not-merged
- derivative-of
created: '2026-09-14T11:57:06.344101Z'
updated: '2026-09-15T04:55:24.091114Z'
source: https://github.com/llm-d/llm-d-kv-cache/pull/663
source_domain: github.com
fetched_at: '2026-09-14T11:57:06.318932Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'PR #663 (llm-d/llm-d-kv-cache, opened Jun 12, 2026 by RishabhSaini) implements
  the interim fix described in issue #372: when a BlockStoredEvent references a parent
  block hash not present in the index (e.g. after a GAIE/EPP restart clears in-memory
  state), pool.go''s processEventBatch now falls back to EmptyBlockHash as the parent
  and continues processing the child block, instead of silently dropping it via ''continue''
  -- which previously cascaded to drop all descendant blocks too, since their parents
  never entered the index. Log level for this case changes from Error to Info since
  it is now an expected condition during index recovery, not a failure. A Copilot
  reviewer flagged that the fix treats ALL GetRequestKey errors as ''parent not in
  index'' rather than distinguishing not-found from other failures (e.g. storage/serialization/corruption),
  risking masked real errors. IMPORTANT CORRECTION: despite being fully implemented
  and reviewed, this PR was ultimately CLOSED (not merged) on Jul 3, 2026 -- the author''s
  own follow-up commit message states ''Remove pool.go EmptyBlockHash fallback (separate
  PR #663)'', indicating the approach was superseded by the more complete replay-buffer-consumer
  fix (PR #691) referenced in issue #372''s later comments, which rebuilds the full
  hash chain from vLLM''s KV-event replay buffer on subscriber reconnect rather than
  indexing with a placeholder empty parent hash.'
---

*Suggested by [[recover-kvblockindex-from-a-disruptive-event-issue-372-llm-dllm-d-kv-cache]] — PR that fixes indexing of blocks with unknown parent using EmptyBlockHash, described in issue 372*

[Skip to content](https://github.com/llm-d/llm-d-kv-cache/pull/663#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/pull/663) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/pull/663) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/pull/663) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-kv-cache/pull/663).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache) You must be signed in to change notification settings
  * [ Fork 156 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache)
  * [ Star  177 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache)


## Conversation
### 
commented [Jun 12, 2026](https://github.com/llm-d/llm-d-kv-cache/pull/663#issue-4650514400)
Copy link  Copy Markdown 
Contributor
## Summary
  * When a `BlockStoredEvent` references a parent block not in the index, fall back to `EmptyBlockHash` instead of dropping the block
  * Changes log level from `Error` to `Info` since this is expected during index recovery
  * Adds `TestBlockStored_MissingParentFallback` verifying blocks are indexed despite missing parent


## Context
After an index-clearing event (e.g. GAIE pod restart), new `BlockStoredEvent`s referencing parent blocks that no longer exist in the index were silently dropped via `continue`. This cascaded — child blocks of dropped blocks were also dropped, since their parents never made it into the index.
With this fix, blocks are indexed with `EmptyBlockHash` as the parent. The request keys will differ from the fully-chained keys, but the blocks are discoverable and their engine-key mappings are established.
Fixes [#372](https://github.com/llm-d/llm-d-kv-cache/issues/372)
[RishabhSaini](https://github.com/RishabhSaini) requested review from [dannyharnik](https://github.com/dannyharnik), [kfirtoledo](https://github.com/kfirtoledo), [liu-cong](https://github.com/liu-cong) and [vMaroon](https://github.com/vMaroon) as [code owners](https://github.com/llm-d/llm-d-kv-cache/blob/7be76e119ca50c5700b3bca65a19b6a0218a9b62/CODEOWNERS#L5) [June 12, 2026 15:31](https://github.com/llm-d/llm-d-kv-cache/pull/663#event-26682440353)
[github-actions](https://github.com/apps/github-actions) Bot added the [ size/M ](https://github.com/llm-d/llm-d-kv-cache/issues?q=state%3Aopen%20label%3Asize%2FM) Denotes a PR that changes 30-99 lines, ignoring generated files. label [Jun 12, 2026](https://github.com/llm-d/llm-d-kv-cache/pull/663#event-26682446898)
[github-actions](https://github.com/apps/github-actions) Bot requested review from [hyeongyun0916](https://github.com/hyeongyun0916), [sagearc](https://github.com/sagearc) and [yankay](https://github.com/yankay) [June 12, 2026 15:32](https://github.com/llm-d/llm-d-kv-cache/pull/663#event-26682449621)
[RishabhSaini](https://github.com/RishabhSaini) mentioned this pull request [ Jun 12, 2026 ](https://github.com/llm-d/llm-d-kv-cache/pull/663#ref-issue-4006316344)
[ Recover kvBlock.Index from a disruptive event #372 ](https://github.com/llm-d/llm-d-kv-cache/issues/372)
[RishabhSaini](https://github.com/RishabhSaini) [force-pushed](https://github.com/llm-d/llm-d-kv-cache/compare/c528c4918294e055190a8ebfd4d73bea278adaf2..8a1ac7c9aa6235164bfd27d97ebc8c618c080a68) the  fix/372-missing-parent-fallback branch from [`c528c49`](https://github.com/llm-d/llm-d-kv-cache/commit/c528c4918294e055190a8ebfd4d73bea278adaf2) to [`8a1ac7c`](https://github.com/llm-d/llm-d-kv-cache/commit/8a1ac7c9aa6235164bfd27d97ebc8c618c080a68) [ Compare ](https://github.com/llm-d/llm-d-kv-cache/compare/c528c4918294e055190a8ebfd4d73bea278adaf2..8a1ac7c9aa6235164bfd27d97ebc8c618c080a68) [June 12, 2026 15:46](https://github.com/llm-d/llm-d-kv-cache/pull/663#event-26683015027)
[github-actions](https://github.com/apps/github-actions) Bot added [ size/L ](https://github.com/llm-d/llm-d-kv-cache/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files. and removed [ size/M ](https://github.com/llm-d/llm-d-kv-cache/issues?q=state%3Aopen%20label%3Asize%2FM) Denotes a PR that changes 30-99 lines, ignoring generated files. labels [Jun 12, 2026](https://github.com/llm-d/llm-d-kv-cache/pull/663#event-26683024209)
` fix: index blocks with unknown parent using empty parent hash[](https://github.com/llm-d/llm-d-kv-cache/pull/663/commits/fd7d34fd6e064ade23dd1d9bf4e6b85fef1f03fe "fix: index blocks with unknown parent using empty parent hash

When a BlockStored event references a parent block not in the index,
fall back to EmptyBlockHash instead of dropping the block entirely.
This prevents cascading block loss when the index is rebuilt.

Fixes #372

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[RishabhSaini](https://github.com/RishabhSaini) Rishabh Saini 
SSH Key Fingerprint: Z0YQ3Puf3ZHyNOafj/g27Wr9bNHrnEEy/7QbuTOFGt8
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-kv-cache/pull/663).

```
When a BlockStored event references a parent block not in the index,
fall back to EmptyBlockHash instead of dropping the block entirely.
This prevents cascading block loss when the index is rebuilt.

Fixes llm-d#372[](https://github.com/llm-d/llm-d-kv-cache/issues/372)

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[RishabhSaini](https://github.com/RishabhSaini) [force-pushed](https://github.com/llm-d/llm-d-kv-cache/compare/8a1ac7c9aa6235164bfd27d97ebc8c618c080a68..fd7d34fd6e064ade23dd1d9bf4e6b85fef1f03fe) the  fix/372-missing-parent-fallback branch from [`8a1ac7c`](https://github.com/llm-d/llm-d-kv-cache/commit/8a1ac7c9aa6235164bfd27d97ebc8c618c080a68) to [`fd7d34f`](https://github.com/llm-d/llm-d-kv-cache/commit/fd7d34fd6e064ade23dd1d9bf4e6b85fef1f03fe) [ Compare ](https://github.com/llm-d/llm-d-kv-cache/compare/8a1ac7c9aa6235164bfd27d97ebc8c618c080a68..fd7d34fd6e064ade23dd1d9bf4e6b85fef1f03fe) [June 12, 2026 15:51](https://github.com/llm-d/llm-d-kv-cache/pull/663#event-26683204630)
[github-actions](https://github.com/apps/github-actions) Bot added [ size/M ](https://github.com/llm-d/llm-d-kv-cache/issues?q=state%3Aopen%20label%3Asize%2FM) Denotes a PR that changes 30-99 lines, ignoring generated files. and removed [ size/L ](https://github.com/llm-d/llm-d-kv-cache/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files. labels [Jun 12, 2026](https://github.com/llm-d/llm-d-kv-cache/pull/663#event-26683211292)
[yankay](https://github.com/yankay) requested a review from [Copilot](https://github.com/apps/copilot-pull-request-reviewer) [June 16, 2026 13:22](https://github.com/llm-d/llm-d-kv-cache/pull/663#event-26810280456)
reviewed  [ Jun 16, 2026 ](https://github.com/llm-d/llm-d-kv-cache/pull/663#pullrequestreview-4506779755)
[ View reviewed changes ](https://github.com/llm-d/llm-d-kv-cache/pull/663/files/fd7d34fd6e064ade23dd1d9bf4e6b85fef1f03fe)
### 
left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
## Pull request overview
Note
Copilot was unable to run its full agentic suite in this review.
This PR changes `BlockStoredEvent` handling so that when a parent block isn’t present in the index, the child block is still processed by falling back to an “empty parent hash”, and adds tests covering cascading chains and mixed batches with missing parents.
**Changes:**
  * Update `processEventBatch` to continue processing when a parent lookup fails, using an empty parent instead of skipping the event.
  * Add tests to ensure missing-parent scenarios no longer drop blocks and don’t block other events in the same batch.


### Reviewed changes
Copilot reviewed 2 out of 2 changed files in this pull request and generated 3 comments.  
| File  | Description  |  
| --- | --- |  
| pkg/kvevents/pool.go  | Changes missing-parent handling to fall back to empty parent and continue processing.  |  
| pkg/kvevents/pool_test.go  | Adds regression tests for missing-parent cascading chain and mixed batch processing.  |  
💡 [Add Copilot custom instructions](https://github.com/llm-d/llm-d-kv-cache/new/main?filename=.github/instructions/*.instructions.md) for smarter, more guided reviews. [Learn how to get started](https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot).
Positive feedbackNegative feedback
Comment thread
Comment on lines to   
|  parentEngineKey := kvblock.BlockHash(ev.ParentHash)  |  
| --- |  
|  key, err := p.index.GetRequestKey(ctx, parentEngineKey)  |  
|  if err != nil {  |  
|  debugLogger.Error(err, "Failed to get request key for parent block",  |  
|  debugLogger.Info("Parent block not in index, using empty parent hash",  |  
|  "parentEngineKey", parentEngineKey, "effectiveModelName", effectiveModelName)  |  
|  continue  |  
|  } else {  |  
|  parentRequestKey = key  |  
### Copilot commented on Jun 16, 2026 
CopilotAI
More actions
This treats all `GetRequestKey` errors as 'parent not in index' and silently falls back. If `err` can represent non-NotFound failures (e.g., storage/serialization/corruption), this will mask real issues and may produce incorrect chains. Handle NotFound explicitly (e.g., `errors.Is(err, <not-found>)`) and log/return (or keep previous behavior) for unexpected errors. Also consider explicitly setting `parentRequestKey` to `kvblock.EmptyBlockHash` in the error branch to guarantee the intended fallback rather than relying on prior initialization.
Positive feedbackNegative feedback
Copilot uses AI. Check for mistakes.
Comment thread
Comment on lines  +839  to  +842   
|  // TestBlockStored_MissingParentCascadingChain verifies that when block B's  |  
| --- |  
|  // parent A is not in the index, B is indexed with EmptyBlockHash as parent,  |  
|  // and a subsequent block C referencing B chains correctly without being dropped.  |  
|  func TestBlockStored_MissingParentCascadingChain(t *testing.T) {  |  
### Copilot commented on Jun 16, 2026 
CopilotAI
  * Medium


More actions
The test comment claims B is indexed with `EmptyBlockHash` as parent and that C chains correctly to B, but the assertions only verify that B and C engine keys are resolvable and that C’s request keys are non-empty/different from B’s. This doesn’t actually prove (1) B used `EmptyBlockHash` as parent, or (2) C used B as its parent rather than also falling back. Strengthen the test to assert the parent relationship via whatever persisted metadata is available (e.g., stored block record, token DB entry, or an index API that exposes parent), or adjust the test name/comment to match what’s actually asserted.
Positive feedbackNegative feedback
Copilot uses AI. Check for mistakes.
Comment thread
Comment on lines  +862  to  +863   
|  lastEngineKeyB := kvblock.BlockHash(engineKeysB[len(engineKeysB)-1])  |  
| --- |  
|  parentKeyForC, err := idx.GetRequestKey(ctx, lastEngineKeyB)  |  
### Copilot commented on Jun 16, 2026 
CopilotAI
More actions
`parentKeyForC` is the request key for block B (looked up by B’s engine key), not the 'parent key for C' yet. Renaming to something like `blockBRequestKey` (or similar) would make the later assertions easier to interpret and reduce confusion about what the value represents.
Positive feedbackNegative feedback
Copilot uses AI. Check for mistakes.
[RishabhSaini](https://github.com/RishabhSaini) closed this [Jul 3, 2026](https://github.com/llm-d/llm-d-kv-cache/pull/663#event-27540495495)
[RishabhSaini](https://github.com/RishabhSaini) added a commit to RishabhSaini/llm-d-kv-cache that referenced this pull request [ Jul 3, 2026 ](https://github.com/llm-d/llm-d-kv-cache/pull/663#ref-commit-81dbdbc)
` fix: address replay state machine review feedback[](https://github.com/RishabhSaini/llm-d-kv-cache/commit/81dbdbcb6961800e17ac6dda82f9662602724f11 "fix: address replay state machine review feedback

- Remove pool.go EmptyBlockHash fallback \(separate PR #663\)
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Detect publisher seq reset \(seq==0 after lastSeq>100\)
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[RishabhSaini](https://github.com/RishabhSaini) Rishabh Saini 
SSH Key Fingerprint: Z0YQ3Puf3ZHyNOafj/g27Wr9bNHrnEEy/7QbuTOFGt8
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
- Remove pool.go EmptyBlockHash fallback (separate PR llm-d#663[](https://github.com/llm-d/llm-d-kv-cache/pull/663))
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Detect publisher seq reset (seq==0 after lastSeq>100)
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[RishabhSaini](https://github.com/RishabhSaini) added a commit to RishabhSaini/llm-d-kv-cache that referenced this pull request [ Jul 3, 2026 ](https://github.com/llm-d/llm-d-kv-cache/pull/663#ref-commit-a87e680)
` fix: address replay state machine review feedback[](https://github.com/RishabhSaini/llm-d-kv-cache/commit/a87e6802fa946d662b704d8b7eca3cbfaff5ac6d "fix: address replay state machine review feedback

- Remove pool.go EmptyBlockHash fallback \(separate PR #663\)
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Detect publisher seq reset \(seq==0 after lastSeq>100\)
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[RishabhSaini](https://github.com/RishabhSaini) Rishabh Saini 
SSH Key Fingerprint: Z0YQ3Puf3ZHyNOafj/g27Wr9bNHrnEEy/7QbuTOFGt8
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
- Remove pool.go EmptyBlockHash fallback (separate PR llm-d#663[](https://github.com/llm-d/llm-d-kv-cache/pull/663))
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Detect publisher seq reset (seq==0 after lastSeq>100)
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[RishabhSaini](https://github.com/RishabhSaini) added a commit to RishabhSaini/llm-d-kv-cache that referenced this pull request [ Jul 3, 2026 ](https://github.com/llm-d/llm-d-kv-cache/pull/663#ref-commit-29d48dc)
` fix: address replay state machine review feedback[](https://github.com/RishabhSaini/llm-d-kv-cache/commit/29d48dcc34e93182b3826bbc3026735f0f3cc305 "fix: address replay state machine review feedback

- Remove pool.go EmptyBlockHash fallback \(separate PR #663\)
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[RishabhSaini](https://github.com/RishabhSaini) Rishabh Saini 
SSH Key Fingerprint: Z0YQ3Puf3ZHyNOafj/g27Wr9bNHrnEEy/7QbuTOFGt8
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
- Remove pool.go EmptyBlockHash fallback (separate PR llm-d#663[](https://github.com/llm-d/llm-d-kv-cache/pull/663))
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[RishabhSaini](https://github.com/RishabhSaini) added a commit to RishabhSaini/llm-d-kv-cache that referenced this pull request [ Jul 3, 2026 ](https://github.com/llm-d/llm-d-kv-cache/pull/663#ref-commit-3ba57ea)
` fix: address replay state machine review feedback[](https://github.com/RishabhSaini/llm-d-kv-cache/commit/3ba57eab79a9f22d542cf121d3fad4a569a55805 "fix: address replay state machine review feedback

- Remove pool.go EmptyBlockHash fallback \(separate PR #663\)
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[RishabhSaini](https://github.com/RishabhSaini) Rishabh Saini 
SSH Key Fingerprint: Z0YQ3Puf3ZHyNOafj/g27Wr9bNHrnEEy/7QbuTOFGt8
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
- Remove pool.go EmptyBlockHash fallback (separate PR llm-d#663[](https://github.com/llm-d/llm-d-kv-cache/pull/663))
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[RishabhSaini](https://github.com/RishabhSaini) added a commit to RishabhSaini/llm-d-kv-cache that referenced this pull request [ Jul 3, 2026 ](https://github.com/llm-d/llm-d-kv-cache/pull/663#ref-commit-e1f9f9a)
` fix: address replay state machine review feedback[](https://github.com/RishabhSaini/llm-d-kv-cache/commit/e1f9f9ad704dd154ec514e7d664fae32a598ca7c "fix: address replay state machine review feedback

- Remove pool.go EmptyBlockHash fallback \(separate PR #663\)
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[RishabhSaini](https://github.com/RishabhSaini) Rishabh Saini 
SSH Key Fingerprint: Z0YQ3Puf3ZHyNOafj/g27Wr9bNHrnEEy/7QbuTOFGt8
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
- Remove pool.go EmptyBlockHash fallback (separate PR llm-d#663[](https://github.com/llm-d/llm-d-kv-cache/pull/663))
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[RishabhSaini](https://github.com/RishabhSaini) added a commit to RishabhSaini/llm-d-kv-cache that referenced this pull request [ Jul 6, 2026 ](https://github.com/llm-d/llm-d-kv-cache/pull/663#ref-commit-6a041f1)
` fix: address replay state machine review feedback[](https://github.com/RishabhSaini/llm-d-kv-cache/commit/6a041f1bcd76f633d06440438d3606aa755cba28 "fix: address replay state machine review feedback

- Remove pool.go EmptyBlockHash fallback \(separate PR #663\)
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[RishabhSaini](https://github.com/RishabhSaini) Rishabh Saini 
SSH Key Fingerprint: Z0YQ3Puf3ZHyNOafj/g27Wr9bNHrnEEy/7QbuTOFGt8
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
- Remove pool.go EmptyBlockHash fallback (separate PR llm-d#663[](https://github.com/llm-d/llm-d-kv-cache/pull/663))
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[RishabhSaini](https://github.com/RishabhSaini) added a commit to RishabhSaini/llm-d-kv-cache that referenced this pull request [ Jul 6, 2026 ](https://github.com/llm-d/llm-d-kv-cache/pull/663#ref-commit-e7d0d81)
` fix: address replay state machine review feedback[](https://github.com/RishabhSaini/llm-d-kv-cache/commit/e7d0d810b9293cd4bcd25dc1e0e8bb4a7c91b85f "fix: address replay state machine review feedback

- Remove pool.go EmptyBlockHash fallback \(separate PR #663\)
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[RishabhSaini](https://github.com/RishabhSaini) Rishabh Saini 
SSH Key Fingerprint: Z0YQ3Puf3ZHyNOafj/g27Wr9bNHrnEEy/7QbuTOFGt8
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
- Remove pool.go EmptyBlockHash fallback (separate PR llm-d#663[](https://github.com/llm-d/llm-d-kv-cache/pull/663))
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[RishabhSaini](https://github.com/RishabhSaini) added a commit to RishabhSaini/llm-d-kv-cache that referenced this pull request [ Jul 6, 2026 ](https://github.com/llm-d/llm-d-kv-cache/pull/663#ref-commit-50ff285)
` fix: address replay state machine review feedback[](https://github.com/RishabhSaini/llm-d-kv-cache/commit/50ff2853e9216f0b57aa6e54362b7d4bb2e2671d "fix: address replay state machine review feedback

- Remove pool.go EmptyBlockHash fallback \(separate PR #663\)
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[RishabhSaini](https://github.com/RishabhSaini) Rishabh Saini 
SSH Key Fingerprint: Z0YQ3Puf3ZHyNOafj/g27Wr9bNHrnEEy/7QbuTOFGt8
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
- Remove pool.go EmptyBlockHash fallback (separate PR llm-d#663[](https://github.com/llm-d/llm-d-kv-cache/pull/663))
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[RishabhSaini](https://github.com/RishabhSaini) added a commit to RishabhSaini/llm-d-kv-cache that referenced this pull request [ Jul 6, 2026 ](https://github.com/llm-d/llm-d-kv-cache/pull/663#ref-commit-ad1b02a)
` fix: address replay state machine review feedback[](https://github.com/RishabhSaini/llm-d-kv-cache/commit/ad1b02a0040bf510bb81eeaaa691dd41871e8a05 "fix: address replay state machine review feedback

- Remove pool.go EmptyBlockHash fallback \(separate PR #663\)
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[RishabhSaini](https://github.com/RishabhSaini) Rishabh Saini 
SSH Key Fingerprint: Z0YQ3Puf3ZHyNOafj/g27Wr9bNHrnEEy/7QbuTOFGt8
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
- Remove pool.go EmptyBlockHash fallback (separate PR llm-d#663[](https://github.com/llm-d/llm-d-kv-cache/pull/663))
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fllm-d%2Fllm-d-kv-cache%2Fpull%2F663)
###  Labels 
[ size/M ](https://github.com/llm-d/llm-d-kv-cache/issues?q=state%3Aopen%20label%3Asize%2FM) Denotes a PR that changes 30-99 lines, ignoring generated files.
###  2 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
