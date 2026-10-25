---
title: 'Recover kvBlock.Index from a disruptive event · Issue #372 · llm-d/llm-d-kv-cache'
id: recover-kvblockindex-from-a-disruptive-event-issue-372-llm-dllm-d-kv-cache
tags:
- llmd-kv-cache-routing-c665d5
- kv-block-index
- failure-recovery
- replay-buffer
- github-issue
created: '2026-09-14T11:53:26.457482Z'
updated: '2026-09-15T04:55:24.029923Z'
source: https://github.com/llm-d/llm-d-kv-cache/issues/372
source_domain: github.com
fetched_at: '2026-09-14T11:53:26.435559Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub issue #372 (opened Mar 1, 2026, llm-d/llm-d-kv-cache) documents a
  real bug in kvBlock.Index recovery after a disruptive event (e.g. GAIE/EPP pod restart
  clears the in-memory index while vLLM pods still hold the actual KV blocks). Sequence:
  after restart, a BlockStored event for a child block [B1]B3 arrives whose parent
  B1 is not in the freshly-cleared index, so pkg/kvevents/pool.go:297 fails with ''Failed
  to get request key for parent block'' / ''engine key not found'' and DROPS the block
  -- meaning any descendant blocks (e.g. [B3]B4) are dropped too, since their parent
  chain was never captured. Impact: KV-cache-aware routing silently loses visibility
  into cache-resident blocks after any indexer restart until the affected prefix ages
  out and is regenerated. Root-cause discussion: vLLM''s KV-event replay buffer holds
  the last 10,000 events and could theoretically rebuild the hash chain on subscriber
  reconnect, but (per RishabhSaini, Jun 2026) a bug in vLLM''s replay event protocol
  prevented this, requiring vllm-project/vllm#45177 (include topic frame in replay
  responses) to merge first. As an interim/permanent fallback (PR #663, merged), blocks
  with unrecoverable parents are now indexed using EmptyBlockHash instead of being
  dropped -- the system self-heals as new requests rebuild correct hash chains from
  scratch, and tail-first eviction lets stale malformed entries expire naturally.
  A full fix (PR #691) adds a replay-buffer consumer for KV-event index recovery on
  subscriber connect/reconnect.'
utility_score: 14.0
---

[Skip to content](https://github.com/llm-d/llm-d-kv-cache/issues/372#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/issues/372) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/issues/372) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/issues/372) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-kv-cache/issues/372).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache) You must be signed in to change notification settings
  * [ Fork 156 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache)
  * [ Star  177 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache)


#  Recover kvBlock.Index from a disruptive event
Copy link
Copy link
[Recover kvBlock.Index from a disruptive event](https://github.com/llm-d/llm-d-kv-cache/issues/372#top)#372
Copy link
Labels
## Description
opened [on Mar 1, 2026](https://github.com/llm-d/llm-d-kv-cache/issues/372#issue-4006316344)
Last edited by RishabhSaini
Contributor
Issue body actions
**Problem:**
Note: notation [B1]B2 depicts a BlockStored KVEvent for B2 whose parent is B1
**Sequence:** Event Add B1 → Index Add B1 ✅ Event Add [B1]B2 → Index Add [B1]B2 ✅ kvBlock.Index cleared by restarting GAIE pod. Hence B1, B2 exist on vLLM Pod but GAIE has no memory in its Index Event Add [B1]B3 → Index drops B3 ❌ (parent B1 not found) Event Add [B3]B4 → Index drops B4 ❌ (parent B3 not found since Index never captured the B3 event)
**Impact:** The kv cache index does not capture blocks with parents not found after a disruptive event
**Location:** `pkg/kvevents/pool.go:297` - parent lookup fails with "Failed to get request key for parent block"
Example: The following was replicated by:
  1. Running x requests
  2. Restarting the GAIE pod (to clear out the index)
  3. Running the same x + y requests


First log line is from adding a log statement after 
[llm-d-kv-cache/pkg/kvevents/pool.go](https://github.com/llm-d/llm-d-kv-cache/blob/1bba8914e21617f6a784d23173ce01e04728c5cc/pkg/kvevents/pool.go#L242)
Line 242 in [1bba891](https://github.com/llm-d/llm-d-kv-cache/commit/1bba8914e21617f6a784d23173ce01e04728c5cc)  
|  case BlockStored:   |  
| --- |  
Second log line is from 
[llm-d-kv-cache/pkg/kvevents/pool.go](https://github.com/llm-d/llm-d-kv-cache/blob/1bba8914e21617f6a784d23173ce01e04728c5cc/pkg/kvevents/pool.go#L285)
Line 285 in [1bba891](https://github.com/llm-d/llm-d-kv-cache/commit/1bba8914e21617f6a784d23173ce01e04728c5cc)  
|  debugLogger.Error(err, "Failed to get request key for parent block",   |  
| --- |  

```
{"level":"Level(-4)","ts":"2026-03-01T05:35:30Z","caller":"kvevents/pool.go:244","msg":"[BLOCKS-ADDED] BlockStored event unmarshaled","podIdentifier":"10.0.2.212","modelName":"openai/gpt-oss-120b","blockHashes":[7495486832647906434],"parentBlockHash":859468294727515564,"tokenIds":[6524,13,20064,12901,6687,2049,5498,558,35978,6726,2392,810,16807,13,120347,3611,14360,12166,558,108984,11410,4279,13,13927,2395,4730,11416,11082,4931,13,25926,7203,2867,11540,16419,765,8831,558,13076,10314,860,6047,7680,13,27042,8439,5747,6939,5030,4454,558,25562,13891,2344,56273,1932,4176,316,161100,13,21407,198,26178,38193,976,5181,3422,7890,316,413,261,1701,27853,483,1991,40536,11,889,860,5364,29617,13,3274,382,290,27179,279,1008,461,57889,5744,5296,7167,3343,42703,484,16,220,37516,6120,42566,1078,8663,25,392,16,220,37516,1,889,5477,625,3239,364,976,3809,5003,392,32,12143,44473,1,1118,306,12911,2023,413,392],"blockSize":128,"loraID":null,"medium":"GPU","loraName":null}

{"level":"error","ts":"2026-03-01T05:35:30Z","caller":"kvevents/pool.go:297","msg":"Failed to get request key for parent block","parentEngineKey":"859468294727515564","effectiveModelName":"openai/gpt-oss-120b","error":"engine key not found: 859468294727515564","stacktrace":"github.com/llm-d/llm-d-kv-cache/pkg/kvevents.(*Pool).digestEvents\n\t/go/pkg/mod/github.com/!rishabh!saini/llm-d-kv-cache@v0.0.0-20260301031129-b0ad1558a5d1/pkg/kvevents/pool.go:297\ngithub.com/llm-d/llm-d-kv-cache/pkg/kvevents.(*Pool).processEvent\n\t/go/pkg/mod/github.com/!rishabh!saini/llm-d-kv-cache@v0.0.0-20260301031129-b0ad1558a5d1/pkg/kvevents/pool.go:230\ngithub.com/llm-d/llm-d-kv-cache/pkg/kvevents.(*Pool).worker.func1\n\t/go/pkg/mod/github.com/!rishabh!saini/llm-d-kv-cache@v0.0.0-20260301031129-b0ad1558a5d1/pkg/kvevents/pool.go:190\ngithub.com/llm-d/llm-d-kv-cache/pkg/kvevents.(*Pool).worker\n\t/go/pkg/mod/github.com/!rishabh!saini/llm-d-kv-cache@v0.0.0-20260301031129-b0ad1558a5d1/pkg/kvevents/pool.go:193"}

```

The log shows the BlockStoredEvent happened for `[859468294727515564]7495486832647906434` but it failed to add to kvblock.Index since the parent block 859468294727515564 does not exist in Index
Expected Behaviour: Despite Parent not existing, new blocks referencing the parent blocks should be added to the Index (albeit with empty ParentBlockHash)
## Activity
[RishabhSaini](https://github.com/RishabhSaini)
changed the title ~~[-] BlockStored Events with Evicted Parents are not tracked in kvblock.Index[/-]~~ [+]BlockStored Events with removed Parents are not tracked in kvblock.Index[/+] [on Mar 1, 2026](https://github.com/llm-d/llm-d-kv-cache/issues/372#event-23166200831)
[RishabhSaini](https://github.com/RishabhSaini)
changed the title ~~[-] BlockStored Events with removed Parents are not tracked in kvblock.Index[/-]~~ [+]Recover kvBlock.Index from a disruptive event[/+] [on Mar 1, 2026](https://github.com/llm-d/llm-d-kv-cache/issues/372#event-23166243916)
[guygir](https://github.com/guygir)
added a commit that references this issue [on Apr 20, 2026](https://github.com/llm-d/llm-d-kv-cache/issues/372#event-24675712672)
[fix(scorer): implement PreRequest and PostResponse interface checks ([l…](https://github.com/llm-d/llm-d-kv-cache/issues/372)](https://github.com/guygir/llm-d-kv-cache-manager/commit/aa9eb83823e3e75fa3ef939e72882d79562bdb53)
### ChenChuang commented on May 5, 2026 
More actions
Complete kvindex could be covered from replay buffer provided by sglang/vllm, but this mechanism is not implemented in llm-d.
### RishabhSaini commented on Jun 10, 2026 
ContributorAuthor
More actions
[@ChenChuang](https://github.com/ChenChuang) it replays the last 10_000 events from its buffer. There's a bug in the vLLM replay event protocol (it doesnt match the regular published events). Currently opened a bug and PR for this
### RishabhSaini commented on Jun 12, 2026 
ContributorAuthor
More actions
The proper fix is to consume vLLM's replay buffer on subscriber connect/reconnect to rebuild the full hash chain in the index. This requires [vllm-project/vllm#45177](https://github.com/vllm-project/vllm/pull/45177) (include topic frame in replay responses) to merge first.
Until then, and as a permanent fallback for cases where replay is unavailable (buffer exhausted, vLLM restarted, replay endpoint not configured), [#663](https://github.com/llm-d/llm-d-kv-cache/pull/663) indexes blocks with `EmptyBlockHash` as parent instead of dropping them. The system self-heals as new requests build correct chains from scratch, and with tail-first eviction semantics, stale entries expire naturally.
[RishabhSaini](https://github.com/RishabhSaini)
added 2 commits that reference this issue [on Jun 12, 2026](https://github.com/llm-d/llm-d-kv-cache/issues/372#event-26683015025)
[fix: index blocks with unknown parent using empty parent hash](https://github.com/RishabhSaini/llm-d-kv-cache/commit/8a1ac7c9aa6235164bfd27d97ebc8c618c080a68)
This issue will close once commit 8a1ac7c is merged into the 'main' branch.[8a1ac7c](https://github.com/RishabhSaini/llm-d-kv-cache/commit/8a1ac7c9aa6235164bfd27d97ebc8c618c080a68)
[fix: index blocks with unknown parent using empty parent hash](https://github.com/RishabhSaini/llm-d-kv-cache/commit/fd7d34fd6e064ade23dd1d9bf4e6b85fef1f03fe)
This issue will close once commit fd7d34f is merged into the 'main' branch.[fd7d34f](https://github.com/RishabhSaini/llm-d-kv-cache/commit/fd7d34fd6e064ade23dd1d9bf4e6b85fef1f03fe)
[RishabhSaini](https://github.com/RishabhSaini)
added 8 commits that reference this issue [on Jun 30, 2026](https://github.com/llm-d/llm-d-kv-cache/issues/372#event-27392804172)
[feat: replay buffer consumer for KV event index recovery](https://github.com/RishabhSaini/llm-d-kv-cache/commit/4042f9ea6fe34276718445fe1b195160b3a36922)
This issue will close once commit 4042f9e is merged into the 'main' branch.[4042f9e](https://github.com/RishabhSaini/llm-d-kv-cache/commit/4042f9ea6fe34276718445fe1b195160b3a36922)
[feat: replay buffer consumer for KV event index recovery](https://github.com/RishabhSaini/llm-d-kv-cache/commit/1c694fefb1e0288c41543a74a47ad70fb7ef6245)
This issue will close once commit 1c694fe is merged into the 'main' branch.[1c694fe](https://github.com/RishabhSaini/llm-d-kv-cache/commit/1c694fefb1e0288c41543a74a47ad70fb7ef6245)
[feat: replay buffer consumer for KV event index recovery](https://github.com/RishabhSaini/llm-d-kv-cache/commit/d97c239c75e37769644ce230f9db140c67021aa2)
This issue will close once commit d97c239 is merged into the 'main' branch.[d97c239](https://github.com/RishabhSaini/llm-d-kv-cache/commit/d97c239c75e37769644ce230f9db140c67021aa2)
[feat: replay buffer consumer for KV event index recovery](https://github.com/RishabhSaini/llm-d-kv-cache/commit/529c50055d17993c1ccd8b06231c3eb8ec80ca84)
This issue will close once commit 529c500 is merged into the 'main' branch.[529c500](https://github.com/RishabhSaini/llm-d-kv-cache/commit/529c50055d17993c1ccd8b06231c3eb8ec80ca84)
[feat: replay buffer consumer for KV event index recovery](https://github.com/RishabhSaini/llm-d-kv-cache/commit/233a287c959978d3ea2a6c7a120861d58595ed05)
This issue will close once commit 233a287 is merged into the 'main' branch.[233a287](https://github.com/RishabhSaini/llm-d-kv-cache/commit/233a287c959978d3ea2a6c7a120861d58595ed05)
[feat: replay buffer consumer for KV event index recovery](https://github.com/RishabhSaini/llm-d-kv-cache/commit/fb547e2429b48fd99b3b128b3d7c1240d1bfc882)
This issue will close once commit fb547e2 is merged into the 'main' branch.[fb547e2](https://github.com/RishabhSaini/llm-d-kv-cache/commit/fb547e2429b48fd99b3b128b3d7c1240d1bfc882)
[feat: replay buffer consumer for KV event index recovery](https://github.com/RishabhSaini/llm-d-kv-cache/commit/4f54fb70d02d91ed95bde1ab0c632026e3d73775)
This issue will close once commit 4f54fb7 is merged into the 'main' branch.[4f54fb7](https://github.com/RishabhSaini/llm-d-kv-cache/commit/4f54fb70d02d91ed95bde1ab0c632026e3d73775)
[feat: pass replay endpoint to KV events subscriber](https://github.com/RishabhSaini/llm-d-inference-scheduler/commit/8e5d743b4d75adde0f142933653fcc1742440300)
This issue will close once commit 8e5d743 is merged into the 'main' branch.[8e5d743](https://github.com/RishabhSaini/llm-d-inference-scheduler/commit/8e5d743b4d75adde0f142933653fcc1742440300)
### github-actions commented on Sep 10, 2026 
[on Sep 10, 2026](https://github.com/llm-d/llm-d-kv-cache/issues/372#issuecomment-5627959936) – with [GitHub Actions](https://help.github.com/en/actions)
More actions
This issue is marked as stale after 90d of inactivity. After an additional 30d of inactivity (15d to become rotten, then 15d more), it will be closed. To prevent this issue from being closed, add a comment or remove the `lifecycle/stale` label.
[github-actions](https://github.com/apps/github-actions)
added [on Sep 10, 2026](https://github.com/llm-d/llm-d-kv-cache/issues/372#event-30942124714)
[Sign up for free](https://github.com/signup?return_to=https://github.com/llm-d/llm-d-kv-cache/issues/372)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/llm-d/llm-d-kv-cache/issues/372)
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
  * [ pool: Track blocks without parent lineagellm-d/llm-d-kv-cache ](https://github.com/llm-d/llm-d-kv-cache/pull/347)
  * [ fix: index blocks with unknown parent using empty parent hashllm-d/llm-d-kv-cache ](https://github.com/llm-d/llm-d-kv-cache/pull/663)
  * [ feat: replay buffer consumer for KV event index recoveryllm-d/llm-d-kv-cache ](https://github.com/llm-d/llm-d-kv-cache/pull/691)


### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
