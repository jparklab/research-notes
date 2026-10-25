---
title: 'feat: replay buffer consumer for KV event index recovery by RishabhSaini ·
  Pull Request #691 · llm-d/llm-d-kv-cache · GitHub'
id: feat-replay-buffer-consumer-for-kv-event-index-recovery-by-rishabhsaini-pull-req
tags:
- llmd-kv-cache-routing-c665d5
- kv-block-index
- failure-recovery
- replay-buffer
- github-pr
- closed-not-merged
- derivative-of
created: '2026-09-14T11:58:19.481828Z'
updated: '2026-09-15T04:55:24.106545Z'
source: https://github.com/llm-d/llm-d-kv-cache/pull/691
source_domain: github.com
fetched_at: '2026-09-14T11:58:19.455139Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'PR #691 (llm-d/llm-d-kv-cache, opened Jun 30, 2026) is an earlier attempt
  at the replay-buffer fix for issue #372: the ZMQ subscriber would connect to vLLM''s
  ROUTER socket via a DEALER on connect/reconnect and proactively replay up to 10,000
  buffered event batches to rebuild the KV-block index in seconds, detecting sequence
  gaps (seq > lastSeq+1) to request replay of missed events, with a 30s cooldown after
  failed replay only. Depended on vllm-project/vllm#45177 (merged) for the topic-frame
  fix in vLLM''s replay protocol. States plainly the problem being solved: ''After
  an EPP restart, the KV block index is empty. Without replay, the index only recovers
  as new requests generate new blocks -- cached prefixes from before the restart are
  invisible to routing.'' This PR was CLOSED on Jul 14, 2026 in favor of the equivalent
  work landing directly in llm-d-router (PR #1887), consistent with the kvcache/kvblock/kvevents
  migration out of llm-d-kv-cache.'
---

*Suggested by [[recover-kvblockindex-from-a-disruptive-event-issue-372-llm-dllm-d-kv-cache]] — the replay-buffer-consumer PR that actually landed to fix issue 372, superseding PR 663*

[Skip to content](https://github.com/llm-d/llm-d-kv-cache/pull/691#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/pull/691) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/pull/691) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/pull/691) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-kv-cache/pull/691).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache) You must be signed in to change notification settings
  * [ Fork 156 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache)
  * [ Star  177 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache)


## Conversation
### 
commented [Jun 30, 2026](https://github.com/llm-d/llm-d-kv-cache/pull/691#issue-4778386364) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-kv-cache/pull/691).
Copy link  Copy Markdown 
Contributor
## Summary
  * Proactive replay on subscriber connect rebuilds the index from vLLM's buffered events without waiting for live traffic
  * Sequence gap detection requests replay for missed events on network blips or ZMQ high-water-mark overflow
  * Fallback to EmptyBlockHash when parent block not in index
  * Cooldown on failed replay only — successful replay allows immediate retry on next gap
  * EnsureSubscriber gains a replayEndpoint parameter (breaking change)


## Context
After an EPP restart, the KV block index is empty. Without replay, the index only recovers as new requests generate new blocks — cached prefixes from before the restart are invisible to routing. With replay, the subscriber connects to vLLM's ROUTER socket and replays up to 10,000 buffered event batches to rebuild the index in seconds.
Depends on: [vllm-project/vllm#45177](https://github.com/vllm-project/vllm/pull/45177)
## Test plan
  * Unit tests: proactive replay on connect, gap detection, cooldown only on failure, spurious re-replay prevention, replay disabled
  * E2E: deployed on GKE with Qwen3-32B, verified replay fires on EPP restart and index rebuilds from buffered events


Fixes [#372](https://github.com/llm-d/llm-d-kv-cache/issues/372)
[github-actions](https://github.com/apps/github-actions) Bot added the [ size/XL ](https://github.com/llm-d/llm-d-kv-cache/issues?q=state%3Aopen%20label%3Asize%2FXL) Denotes a PR that changes 500-999 lines, ignoring generated files. label [Jun 30, 2026](https://github.com/llm-d/llm-d-kv-cache/pull/691#event-27387348319)
[github-actions](https://github.com/apps/github-actions) Bot requested review from [hyeongyun0916](https://github.com/hyeongyun0916), [liu-cong](https://github.com/liu-cong), [sagearc](https://github.com/sagearc) and [yankay](https://github.com/yankay) [June 30, 2026 15:58](https://github.com/llm-d/llm-d-kv-cache/pull/691#event-27387355005)
[RishabhSaini](https://github.com/RishabhSaini) [force-pushed](https://github.com/llm-d/llm-d-kv-cache/compare/5a1d8645443a6396ece7c00d46d0893ca544e9e7..4042f9ea6fe34276718445fe1b195160b3a36922) the  fix/372-replay-consumer branch from [`5a1d864`](https://github.com/llm-d/llm-d-kv-cache/commit/5a1d8645443a6396ece7c00d46d0893ca544e9e7) to [`4042f9e`](https://github.com/llm-d/llm-d-kv-cache/commit/4042f9ea6fe34276718445fe1b195160b3a36922) [ Compare ](https://github.com/llm-d/llm-d-kv-cache/compare/5a1d8645443a6396ece7c00d46d0893ca544e9e7..4042f9ea6fe34276718445fe1b195160b3a36922) [June 30, 2026 18:13](https://github.com/llm-d/llm-d-kv-cache/pull/691#event-27392804429)
[RishabhSaini](https://github.com/RishabhSaini) marked this pull request as ready for review [June 30, 2026 18:15](https://github.com/llm-d/llm-d-kv-cache/pull/691#event-27392903622)
[RishabhSaini](https://github.com/RishabhSaini) requested review from [dannyharnik](https://github.com/dannyharnik), [kfirtoledo](https://github.com/kfirtoledo) and [vMaroon](https://github.com/vMaroon) as [code owners](https://github.com/llm-d/llm-d-kv-cache/blob/c4ebd56486eb66b58b9d20915eec90a977f1843a/CODEOWNERS#L5) [June 30, 2026 18:15](https://github.com/llm-d/llm-d-kv-cache/pull/691#event-27392904051)
[RishabhSaini](https://github.com/RishabhSaini) [force-pushed](https://github.com/llm-d/llm-d-kv-cache/compare/233a287c959978d3ea2a6c7a120861d58595ed05..fb547e2429b48fd99b3b128b3d7c1240d1bfc882) the  fix/372-replay-consumer branch 5 times, most recently from [`233a287`](https://github.com/llm-d/llm-d-kv-cache/commit/233a287c959978d3ea2a6c7a120861d58595ed05) to [`fb547e2`](https://github.com/llm-d/llm-d-kv-cache/commit/fb547e2429b48fd99b3b128b3d7c1240d1bfc882) [ Compare ](https://github.com/llm-d/llm-d-kv-cache/compare/233a287c959978d3ea2a6c7a120861d58595ed05..fb547e2429b48fd99b3b128b3d7c1240d1bfc882) [June 30, 2026 18:35](https://github.com/llm-d/llm-d-kv-cache/pull/691#event-27393678333)
` feat: replay buffer consumer for KV event index recovery[](https://github.com/llm-d/llm-d-kv-cache/pull/691/commits/4f54fb70d02d91ed95bde1ab0c632026e3d73775 "feat: replay buffer consumer for KV event index recovery

Add sequence gap detection and replay request support to the ZMQ
subscriber. When a gap is detected \(or the subscriber joins
mid-stream\), it connects to the vLLM ROUTER socket via a DEALER
and replays missed events to rebuild the index.

Also includes the defensive fallback: blocks with unknown parent
hashes are indexed with EmptyBlockHash instead of being dropped,
preventing cascading block loss when the replay buffer is
exhausted.

Key behaviors:
- Gap detection on every received message \(seq > lastSeq + 1\)
- Full replay from seq 0 on first message with seq > 0 \(EPP restart\)
- DEALER socket for multi-message replay \(REQ only receives one\)
- 30s cooldown between replay attempts to avoid blocking on
  unreachable endpoints
- lastSeq only advances forward to prevent spurious re-replays
- ReplaySocketPort disabled by default to avoid DP rank port
  collisions

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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-kv-cache/pull/691).

```
Add sequence gap detection and replay request support to the ZMQ
subscriber. When a gap is detected (or the subscriber joins
mid-stream), it connects to the vLLM ROUTER socket via a DEALER
and replays missed events to rebuild the index.

Also includes the defensive fallback: blocks with unknown parent
hashes are indexed with EmptyBlockHash instead of being dropped,
preventing cascading block loss when the replay buffer is
exhausted.

Key behaviors:
- Gap detection on every received message (seq > lastSeq + 1)
- Full replay from seq 0 on first message with seq > 0 (EPP restart)
- DEALER socket for multi-message replay (REQ only receives one)
- 30s cooldown between replay attempts to avoid blocking on
  unreachable endpoints
- lastSeq only advances forward to prevent spurious re-replays
- ReplaySocketPort disabled by default to avoid DP rank port
  collisions

Fixes llm-d#372[](https://github.com/llm-d/llm-d-kv-cache/issues/372)

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[RishabhSaini](https://github.com/RishabhSaini) [force-pushed](https://github.com/llm-d/llm-d-kv-cache/compare/fb547e2429b48fd99b3b128b3d7c1240d1bfc882..4f54fb70d02d91ed95bde1ab0c632026e3d73775) the  fix/372-replay-consumer branch from [`fb547e2`](https://github.com/llm-d/llm-d-kv-cache/commit/fb547e2429b48fd99b3b128b3d7c1240d1bfc882) to [`4f54fb7`](https://github.com/llm-d/llm-d-kv-cache/commit/4f54fb70d02d91ed95bde1ab0c632026e3d73775) [ Compare ](https://github.com/llm-d/llm-d-kv-cache/compare/fb547e2429b48fd99b3b128b3d7c1240d1bfc882..4f54fb70d02d91ed95bde1ab0c632026e3d73775) [June 30, 2026 18:40](https://github.com/llm-d/llm-d-kv-cache/pull/691#event-27393844050)
[RishabhSaini](https://github.com/RishabhSaini) mentioned this pull request [ Jul 3, 2026 ](https://github.com/llm-d/llm-d-kv-cache/pull/691#ref-pullrequest-4806234113)
[ feat: replay buffer consumer for KV event index recovery llm-d/llm-d-router#1887 ](https://github.com/llm-d/llm-d-router/pull/1887)
Merged 
5 tasks
[RishabhSaini](https://github.com/RishabhSaini) [force-pushed](https://github.com/llm-d/llm-d-kv-cache/compare/3ba57eab79a9f22d542cf121d3fad4a569a55805..e1f9f9ad704dd154ec514e7d664fae32a598ca7c) the  fix/372-replay-consumer branch 4 times, most recently from [`3ba57ea`](https://github.com/llm-d/llm-d-kv-cache/commit/3ba57eab79a9f22d542cf121d3fad4a569a55805) to [`e1f9f9a`](https://github.com/llm-d/llm-d-kv-cache/commit/e1f9f9ad704dd154ec514e7d664fae32a598ca7c) [ Compare ](https://github.com/llm-d/llm-d-kv-cache/compare/3ba57eab79a9f22d542cf121d3fad4a569a55805..e1f9f9ad704dd154ec514e7d664fae32a598ca7c) [July 3, 2026 21:44](https://github.com/llm-d/llm-d-kv-cache/pull/691#event-27544606429)
commented  [ Jul 3, 2026 ](https://github.com/llm-d/llm-d-kv-cache/pull/691#pullrequestreview-4628170731)
[ View reviewed changes ](https://github.com/llm-d/llm-d-kv-cache/pull/691/files)
Comment thread Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-kv-cache/pull/691).
[RishabhSaini](https://github.com/RishabhSaini) [force-pushed](https://github.com/llm-d/llm-d-kv-cache/compare/e7d0d810b9293cd4bcd25dc1e0e8bb4a7c91b85f..50ff2853e9216f0b57aa6e54362b7d4bb2e2671d) the  fix/372-replay-consumer branch 3 times, most recently from [`e7d0d81`](https://github.com/llm-d/llm-d-kv-cache/commit/e7d0d810b9293cd4bcd25dc1e0e8bb4a7c91b85f) to [`50ff285`](https://github.com/llm-d/llm-d-kv-cache/commit/50ff2853e9216f0b57aa6e54362b7d4bb2e2671d) [ Compare ](https://github.com/llm-d/llm-d-kv-cache/compare/e7d0d810b9293cd4bcd25dc1e0e8bb4a7c91b85f..50ff2853e9216f0b57aa6e54362b7d4bb2e2671d) [July 6, 2026 14:27](https://github.com/llm-d/llm-d-kv-cache/pull/691#event-27613626778)
` fix: address replay state machine review feedback[](https://github.com/llm-d/llm-d-kv-cache/pull/691/commits/ad1b02a0040bf510bb81eeaaa691dd41871e8a05 "fix: address replay state machine review feedback

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
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-kv-cache/pull/691).

```
- Remove pool.go EmptyBlockHash fallback (separate PR llm-d#663[](https://github.com/llm-d/llm-d-kv-cache/pull/663))
- Restore context.WithTimeout for DEALER to bound Recv
- requestReplay returns bool; failed replay skips lastSeq advance
- Proactive replay respects cooldown on blackholed endpoints
- Malformed replay frames set lastReplayFailure and return false
- Extract parseEventFrame helper shared by live and replay paths

Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[RishabhSaini](https://github.com/RishabhSaini) [force-pushed](https://github.com/llm-d/llm-d-kv-cache/compare/50ff2853e9216f0b57aa6e54362b7d4bb2e2671d..ad1b02a0040bf510bb81eeaaa691dd41871e8a05) the  fix/372-replay-consumer branch from [`50ff285`](https://github.com/llm-d/llm-d-kv-cache/commit/50ff2853e9216f0b57aa6e54362b7d4bb2e2671d) to [`ad1b02a`](https://github.com/llm-d/llm-d-kv-cache/commit/ad1b02a0040bf510bb81eeaaa691dd41871e8a05) [ Compare ](https://github.com/llm-d/llm-d-kv-cache/compare/50ff2853e9216f0b57aa6e54362b7d4bb2e2671d..ad1b02a0040bf510bb81eeaaa691dd41871e8a05) [July 6, 2026 14:36](https://github.com/llm-d/llm-d-kv-cache/pull/691#event-27614030479)
### 
commented [Jul 14, 2026](https://github.com/llm-d/llm-d-kv-cache/pull/691#issuecomment-4972887616)
Copy link  Copy Markdown 
Contributor Author  
|  in preference of: [llm-d/llm-d-router#1887](https://github.com/llm-d/llm-d-router/pull/1887)  |  
| --- |  
[RishabhSaini](https://github.com/RishabhSaini) closed this [Jul 14, 2026](https://github.com/llm-d/llm-d-kv-cache/pull/691#event-27980066960)
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fllm-d%2Fllm-d-kv-cache%2Fpull%2F691)
###  Labels 
[ size/XL ](https://github.com/llm-d/llm-d-kv-cache/issues?q=state%3Aopen%20label%3Asize%2FXL) Denotes a PR that changes 500-999 lines, ignoring generated files.
###  1 participant 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
