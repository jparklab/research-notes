---
title: 'feat: replay buffer consumer for KV event index recovery by RishabhSaini ·
  Pull Request #1887 · llm-d/llm-d-router · GitHub'
id: feat-replay-buffer-consumer-for-kv-event-index-recovery-by-rishabhsaini-pull-r-2
tags:
- llmd-kv-cache-routing-c665d5
- kv-block-index
- failure-recovery
- replay-buffer
- zmq
- github-pr
- llm-d-router
created: '2026-09-14T11:58:35.199174Z'
updated: '2026-09-15T04:55:24.110171Z'
source: https://github.com/llm-d/llm-d-router/pull/1887
source_domain: github.com
fetched_at: '2026-09-14T11:58:35.177312Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'PR #1887 (llm-d/llm-d-router, merged Aug 6, 2026 by vMaroon) is the actual,
  currently-merged fix that gives llm-d''s KV-cache index disaster recovery via vLLM''s
  KV-event replay buffer, closing out issue #372''s root cause. The ZMQ subscriber
  (pkg/kvevents/zmq_subscriber.go) connects to vLLM''s ROUTER socket via a DEALER
  and: (1) proactively replays on connect so the index rebuilds immediately without
  waiting for live traffic; (2) detects sequence gaps (seq > lastSeq+1) and requests
  replay starting at lastSeq+1; (3) tracks DUAL watermarks -- lastSeq (what''s been
  indexed) vs lastLiveSeq (what''s arrived on the wire) -- specifically so replay
  overlap cannot inflate the kvevents dedup reference counts described in the kvevents
  package docs; (4) applies a strict event gate so that after replay, live events
  with seq<=lastSeq are dropped before reaching AddTask; (5) detects sequence RESET
  (seq < lastLiveSeq, e.g. vLLM pod restarted but kept the same IP so EnsureSubscriber
  no-ops) and clears the index and dedup state via the pool queue; (6) backs off 30s
  on failed replay only, with success clearing failure state immediately; (7) uses
  DEALER instead of REQ because REQ can only receive one reply per multi-message replay.
  EnsureSubscriber''s signature grew sourceEndpoint and replayEndpoint parameters
  (a breaking change) computed from PodDiscoveryConfig.ReplaySocketPort. Pre-merge
  review (Jul 30, 2026) caught real correctness bugs in earlier revisions -- gap replay
  double-storing an already-buffered event because vLLM''s _service_replay resends
  anything with seq>=start_seq, corrupting the dedup reference count and leaking a
  block that should have been evicted; and cooldown freezing lastSeq while AddTask
  kept running, so the eventual retry re-sent (and re-double-stored) the ENTIRE frozen
  window, not just the missed event. Depends on vllm-project/vllm#45177 (merged) for
  the topic-frame fix in vLLM''s own replay protocol. This PR supersedes both llm-d-kv-cache
  PR #663 (EmptyBlockHash-only fallback) and PR #691 (the same replay-buffer feature
  attempted in the pre-migration repo).'
---

*Suggested by [[feat-replay-buffer-consumer-for-kv-event-index-recovery-by-rishabhsaini-pull-req]] — the merged PR in llm-d-router that actually landed the replay-buffer-consumer fix for issue 372, superseding PR 691*

[Skip to content](https://github.com/llm-d/llm-d-router/pull/1887#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/1887) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/1887) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/1887) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1887).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  336 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Conversation
### 
commented [Jul 3, 2026](https://github.com/llm-d/llm-d-router/pull/1887#issue-4806234113) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1887).
Copy link  Copy Markdown 
Contributor
## Summary
Add replay buffer consumer to rebuild the KV block index after EPP restart or network blips. The subscriber connects to vLLM's ROUTER socket via a DEALER and replays missed events.
  * **Proactive replay on connect** — rebuilds index immediately, no wait for live traffic
  * **Gap detection** — `seq > lastSeq + 1` triggers replay from `lastSeq + 1`
  * **Dual watermarks** — `lastSeq` (index) vs `lastLiveSeq` (wire) so replay overlap cannot inflate dedup reference counts
  * **Strict event gate** — after replay, live events with `seq <= lastSeq` are dropped before reaching AddTask
  * **Sequence reset detection** — `seq < lastLiveSeq` clears index and dedup via pool queue (ordered with pending events)
  * **Cooldown on failure only** — successful replay clears failure state; failed replay backs off 30s
  * **DEALER over REQ** — REQ can only receive one reply per request


`EnsureSubscriber` gains `sourceEndpoint` and `replayEndpoint` parameters. Replay endpoint computed from `PodDiscoveryConfig.ReplaySocketPort` in the extractor.
Depends on: [vllm-project/vllm#45177](https://github.com/vllm-project/vllm/pull/45177) (merged)
## Test plan
  * Proactive replay rebuilds serving endpoint identity on connect
  * Gap replay does not duplicate the triggering event
  * Events dropped during replay cooldown
  * Sequence reset clears and rebuilds pod index
  * Replayed live events do not trigger another replay



```
Add replay buffer consumer for KV event index recovery after EPP
restart or network blips.

```

Relates to [#1737](https://github.com/llm-d/llm-d-router/issues/1737)
[RishabhSaini](https://github.com/RishabhSaini) requested review from a team, [bongwoobak](https://github.com/bongwoobak) and [vMaroon](https://github.com/vMaroon) as [code owners](https://github.com/llm-d/llm-d-router/blob/25355293e1f56456a943983b3c3a90250215a4c7/CODEOWNERS#L14) [July 3, 2026 20:50](https://github.com/llm-d/llm-d-router/pull/1887#event-27543513046)
[RishabhSaini](https://github.com/RishabhSaini) requested a review from [ahg-g](https://github.com/ahg-g) [July 3, 2026 20:50](https://github.com/llm-d/llm-d-router/pull/1887#event-27543513295)
[github-actions](https://github.com/apps/github-actions) Bot added the [ size/S ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FS) Denotes a PR that changes 10-29 lines, ignoring generated files. label [Jul 3, 2026](https://github.com/llm-d/llm-d-router/pull/1887#event-27543515767)
[RishabhSaini](https://github.com/RishabhSaini) marked this pull request as draft [July 3, 2026 20:54](https://github.com/llm-d/llm-d-router/pull/1887#event-27543603714)
[RishabhSaini](https://github.com/RishabhSaini) marked this pull request as ready for review [July 3, 2026 21:12](https://github.com/llm-d/llm-d-router/pull/1887#event-27544010955)
[RishabhSaini](https://github.com/RishabhSaini) [force-pushed](https://github.com/llm-d/llm-d-router/compare/42750faa06652b069f66f744ec0b39f216d57c58..8e5d743b4d75adde0f142933653fcc1742440300) the  fix/1737-replay-consumer branch from [`42750fa`](https://github.com/llm-d/llm-d-router/commit/42750faa06652b069f66f744ec0b39f216d57c58) to [`8e5d743`](https://github.com/llm-d/llm-d-router/commit/8e5d743b4d75adde0f142933653fcc1742440300) [ Compare ](https://github.com/llm-d/llm-d-router/compare/42750faa06652b069f66f744ec0b39f216d57c58..8e5d743b4d75adde0f142933653fcc1742440300) [July 6, 2026 14:39](https://github.com/llm-d/llm-d-router/pull/1887#event-27614215305)
[RishabhSaini](https://github.com/RishabhSaini) [force-pushed](https://github.com/llm-d/llm-d-router/compare/8e5d743b4d75adde0f142933653fcc1742440300..bb68b5c80f61c5501ad4a2dd2752a9d1b2afef2b) the  fix/1737-replay-consumer branch from [`8e5d743`](https://github.com/llm-d/llm-d-router/commit/8e5d743b4d75adde0f142933653fcc1742440300) to [`bb68b5c`](https://github.com/llm-d/llm-d-router/commit/bb68b5c80f61c5501ad4a2dd2752a9d1b2afef2b) [ Compare ](https://github.com/llm-d/llm-d-router/compare/8e5d743b4d75adde0f142933653fcc1742440300..bb68b5c80f61c5501ad4a2dd2752a9d1b2afef2b) [July 14, 2026 18:43](https://github.com/llm-d/llm-d-router/pull/1887#event-27979336483)
[github-actions](https://github.com/apps/github-actions) Bot added [ size/XL ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FXL) Denotes a PR that changes 500-999 lines, ignoring generated files. and removed [ size/S ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FS) Denotes a PR that changes 10-29 lines, ignoring generated files. labels [Jul 14, 2026](https://github.com/llm-d/llm-d-router/pull/1887#event-27979341863)
[RishabhSaini](https://github.com/RishabhSaini) changed the title ~~feat: pass replay endpoint to KV events subscriber~~ feat: replay buffer consumer for KV event index recovery [Jul 14, 2026](https://github.com/llm-d/llm-d-router/pull/1887#event-27979361717)
[RishabhSaini](https://github.com/RishabhSaini) mentioned this pull request [ Jul 14, 2026 ](https://github.com/llm-d/llm-d-router/pull/1887#ref-pullrequest-4778386364)
[ feat: replay buffer consumer for KV event index recovery llm-d/llm-d-kv-cache#691 ](https://github.com/llm-d/llm-d-kv-cache/pull/691)
Closed 
2 tasks
reviewed  [ Jul 30, 2026 ](https://github.com/llm-d/llm-d-router/pull/1887#pullrequestreview-4821026820)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1887/files)
Comment thread [pkg/kvevents/zmq_subscriber.go](https://github.com/llm-d/llm-d-router/pull/1887/files#diff-8e38a3f3a4c819f7972e0545af12eeb8e4894a48258878662708f9e2897aeed9) Outdated   
|  logger.Info("Detected gap in event sequence, requesting replay",  |  
| --- |  
|  "lastSeq", z.lastSeq, "currentSeq", seq, "missed", missed,  |  
|  "endpoint", z.endpoint)  |  
|  if !z.requestReplay(ctx, z.lastSeq+1) {  |  
### 
[Jul 30, 2026](https://github.com/llm-d/llm-d-router/pull/1887#discussion_r3684470509)
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Gap replay asks for `lastSeq+1`, and the triggering event is already buffered on the vLLM side (`_service_replay` sends everything with `seq >= start_seq`), so it comes back and AddTask below stores it a second time. `trackStore` counts both while the engine only ever emits one `BlockRemoved`, so `filterRemove` suppresses the eviction and the block stays in the index. `TestPool_DuplicateStoreSurvivesFirstRemove` is the same mechanism.
Could you skip already-covered seqs before AddTask? Ties into the cooldown and restart cases below.
reviewed  [ Jul 30, 2026 ](https://github.com/llm-d/llm-d-router/pull/1887#pullrequestreview-4821373338)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1887/files)
Comment thread [pkg/kvevents/zmq_subscriber.go](https://github.com/llm-d/llm-d-router/pull/1887/files#diff-8e38a3f3a4c819f7972e0545af12eeb8e4894a48258878662708f9e2897aeed9) Outdated   
|  // Freeze lastSeq during cooldown so the gap is preserved for  |  
| --- |  
|  // retry — blocks past a gap are dropped (broken parent chain)  |  
|  // and can only be recovered via replay.  |  
|  if z.replayEndpoint == "" || z.canAttemptReplay() {  |  
### 
[Jul 30, 2026](https://github.com/llm-d/llm-d-router/pull/1887#discussion_r3684739676)
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Cooldown freezes `lastSeq` but the AddTask above still runs, so events land in the index without the watermark moving. When cooldown lifts, replay starts from the frozen `lastSeq+1` and re-sends that whole window, so the duplicate-store problem above hits every block in it instead of just one. The description justifies the freeze by post-gap blocks being dropped on a broken parent chain, but nothing here drops them, they all go through AddTask.
A seq guard alone will not cover this since `lastSeq` is frozen at that point. How about splitting it into a replay start point and a processed watermark?
reviewed  [ Jul 30, 2026 ](https://github.com/llm-d/llm-d-router/pull/1887#pullrequestreview-4821395135)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1887/files)
Comment thread [pkg/kvevents/zmq_subscriber.go](https://github.com/llm-d/llm-d-router/pull/1887/files#diff-8e38a3f3a4c819f7972e0545af12eeb8e4894a48258878662708f9e2897aeed9) Outdated   
|  // retry — blocks past a gap are dropped (broken parent chain)  |  
| --- |  
|  // and can only be recovered via replay.  |  
|  if z.replayEndpoint == "" || z.canAttemptReplay() {  |  
|  if !z.hasLastSeq || seq > z.lastSeq {  |  
### 
[Jul 30, 2026](https://github.com/llm-d/llm-d-router/pull/1887#discussion_r3684757376)
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
A container restart keeps the pod IP, so `EnsureSubscriber` sees the same endpoint, no-ops, and this subscriber survives with its `lastSeq` while vLLM's seq restarts at 0. After that `seq > lastSeq+1` never fires and this line never advances `lastSeq` either, so gap detection stays dead for the rest of the subscriber's life. It also makes the seq guard from the other two comments risky, every event after a restart sits below the watermark and would get skipped.
`seq < lastSeq` needs to reset the state and pull a full replay, otherwise that guard cannot go in safely.
` feat: replay buffer consumer for KV event index recovery[](https://github.com/llm-d/llm-d-router/pull/1887/commits/41c51ff34eeb8b1e6d20013cd86adf7f28f8c134 "feat: replay buffer consumer for KV event index recovery

Add sequence gap detection and replay request support to the ZMQ
subscriber. When a gap is detected or the subscriber connects for
the first time, it connects to the vLLM ROUTER socket via a DEALER
and replays missed events to rebuild the index.

Key behaviors:
- Proactive replay on connect rebuilds index without waiting for
  live traffic
- Gap detection on every received message \(seq > lastSeq + 1\)
- DEALER socket for multi-message replay
- Dual watermarks: lastSeq \(index\) vs lastLiveSeq \(wire\) so replay
  overlap cannot inflate dedup reference counts
- Strict event gate after replay drops duplicates before AddTask
- Sequence reset detection clears index and dedup via pool queue
- Cooldown on failed replay only — success clears failure state
- Shared parseEventFrame helper for live and replay paths
- Replay endpoint wired via ensureSubscriber in extractor

Co-authored-by: RishabhSaini <rishabhsaini01@gmail.com>
Co-authored-by: Maroon Ayoub <mayoub@redhat.com>
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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1887).

```
Add sequence gap detection and replay request support to the ZMQ
subscriber. When a gap is detected or the subscriber connects for
the first time, it connects to the vLLM ROUTER socket via a DEALER
and replays missed events to rebuild the index.

Key behaviors:
- Proactive replay on connect rebuilds index without waiting for
  live traffic
- Gap detection on every received message (seq > lastSeq + 1)
- DEALER socket for multi-message replay
- Dual watermarks: lastSeq (index) vs lastLiveSeq (wire) so replay
  overlap cannot inflate dedup reference counts
- Strict event gate after replay drops duplicates before AddTask
- Sequence reset detection clears index and dedup via pool queue
- Cooldown on failed replay only — success clears failure state
- Shared parseEventFrame helper for live and replay paths
- Replay endpoint wired via ensureSubscriber in extractor

Co-authored-by: RishabhSaini <rishabhsaini01@gmail.com>
Co-authored-by: Maroon Ayoub <mayoub@redhat.com>
Signed-off-by: RishabhSaini <rishabhsaini01@gmail.com>
```

[RishabhSaini](https://github.com/RishabhSaini) [force-pushed](https://github.com/llm-d/llm-d-router/compare/bb68b5c80f61c5501ad4a2dd2752a9d1b2afef2b..41c51ff34eeb8b1e6d20013cd86adf7f28f8c134) the  fix/1737-replay-consumer branch from [`bb68b5c`](https://github.com/llm-d/llm-d-router/commit/bb68b5c80f61c5501ad4a2dd2752a9d1b2afef2b) to [`41c51ff`](https://github.com/llm-d/llm-d-router/commit/41c51ff34eeb8b1e6d20013cd86adf7f28f8c134) [ Compare ](https://github.com/llm-d/llm-d-router/compare/bb68b5c80f61c5501ad4a2dd2752a9d1b2afef2b..41c51ff34eeb8b1e6d20013cd86adf7f28f8c134) [August 5, 2026 14:10](https://github.com/llm-d/llm-d-router/pull/1887#event-29013800390)
[RishabhSaini](https://github.com/RishabhSaini) requested review from [hyeongyun0916](https://github.com/hyeongyun0916), [liu-cong](https://github.com/liu-cong), [sagearc](https://github.com/sagearc) and [yankay](https://github.com/yankay) as [code owners](https://github.com/llm-d/llm-d-router/blob/65e9ef59f2ac93a556d774c060ae62a9f30d66d1/CODEOWNERS#L49) [August 5, 2026 14:10](https://github.com/llm-d/llm-d-router/pull/1887#event-29013800522)
### 
commented [Aug 6, 2026](https://github.com/llm-d/llm-d-router/pull/1887#issuecomment-5208129847)
Copy link  Copy Markdown 
Collaborator  
|  /assign [@vMaroon](https://github.com/vMaroon)  |  
| --- |  
approved these changes  [ Aug 6, 2026 ](https://github.com/llm-d/llm-d-router/pull/1887#pullrequestreview-4877328981)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1887/files/41c51ff34eeb8b1e6d20013cd86adf7f28f8c134)
### 
left a comment 
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Verified and tested, iterated async for speed.
Hide details View details
[vMaroon](https://github.com/vMaroon) merged commit [`91efee3`](https://github.com/llm-d/llm-d-router/commit/91efee370088fa6423e9f6feb9c4146031172e22) into  llm-d:main [Aug 6, 2026](https://github.com/llm-d/llm-d-router/pull/1887#event-29082465080)
39 checks passed 
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1887).
[RishabhSaini](https://github.com/RishabhSaini) mentioned this pull request [ Aug 6, 2026 ](https://github.com/llm-d/llm-d-router/pull/1887#ref-pullrequest-5084150307)
[ feat(precise): enable KV event replay buffer for index recovery llm-d/llm-d#2196 ](https://github.com/llm-d/llm-d/pull/2196)
Merged 
3 tasks
[bongwoobak](https://github.com/bongwoobak) added a commit to moreh-dev/llm-d-inference-scheduler that referenced this pull request [ Aug 10, 2026 ](https://github.com/llm-d/llm-d-router/pull/1887#ref-commit-37430a8)
` Merge branch 'main' into feature/kv-events-health-monitor[](https://github.com/moreh-dev/llm-d-inference-scheduler/commit/37430a845a18c3f6db982786c60a526287ab0adf "Merge branch 'main' into feature/kv-events-health-monitor

Resolves the PreRequest conflict from #2215, which gave the hook an error
return. Routing is still recorded before the speculative-indexing early
exit, since an endpoint that is routed to but never confirmed is exactly
the broken-pipeline case the monitor exists to separate from idle.

Also picks up the endpoint-identity rename \(meta.NamespacedName -> meta.ID,
#2147\) and the wider EnsureSubscriber signature \(#1887, #2233\) in the
extractor, and routes the new sourceEndpoint through endpointIdentifier so
subscriber attribution, index entries, lookup filters and health state keep
sharing one construction.") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[bongwoobak](https://github.com/bongwoobak) Bongwoo Bak 
GPG key ID: C9E6818A4CC24BAB
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1887).

```
Resolves the PreRequest conflict from llm-d#2215[](https://github.com/llm-d/llm-d-router/pull/2215), which gave the hook an error
return. Routing is still recorded before the speculative-indexing early
exit, since an endpoint that is routed to but never confirmed is exactly
the broken-pipeline case the monitor exists to separate from idle.

Also picks up the endpoint-identity rename (meta.NamespacedName -> meta.ID,
llm-d#2147[](https://github.com/llm-d/llm-d-router/pull/2147)) and the wider EnsureSubscriber signature (llm-d#1887[](https://github.com/llm-d/llm-d-router/pull/1887), llm-d#2233[](https://github.com/llm-d/llm-d-router/pull/2233)) in the
extractor, and routes the new sourceEndpoint through endpointIdentifier so
subscriber attribution, index entries, lookup filters and health state keep
sharing one construction.
```

[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fllm-d%2Fllm-d-router%2Fpull%2F1887)
###  Labels 
[ size/XL ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FXL) Denotes a PR that changes 500-999 lines, ignoring generated files.
###  4 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
