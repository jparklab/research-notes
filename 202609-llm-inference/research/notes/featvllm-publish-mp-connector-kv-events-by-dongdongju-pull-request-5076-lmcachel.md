---
title: '[feat][vllm] publish MP connector KV events by DongDongJu · Pull Request #5076
  · LMCache/LMCache · GitHub'
id: featvllm-publish-mp-connector-kv-events-by-dongdongju-pull-request-5076-lmcachel
tags:
- llmd-kv-cache-routing-c665d5
- lmcache-mp-mode
- kv-events-gap
created: '2026-09-14T13:04:02.231402Z'
updated: '2026-09-15T04:55:24.143841Z'
source: https://github.com/LMCache/LMCache/pull/5076
source_domain: github.com
fetched_at: '2026-09-14T13:04:02.210310Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'LMCache/LMCache PR #5076 (''[feat][vllm] publish MP connector KV events''
  by DongDongJu, opened Sep 11 2026, refs issue #3661). Confirmed via GitHub API as
  of Sep 14 2026 03:50 UTC (same day as this research run): state=open, merged=false
  -- this is an UNMERGED, in-review fix, not yet shipped. It takes a different approach
  from the rival rebel-jinhwan ZmqKVEventSink fork: rather than a new ZMQ sink with
  LMCache-specific medium strings, it enhances the existing vLLM connector so completed
  MP-server stores get converted to vLLM BlockStored events and drained through the
  connector''s normal take_events() path -- the same mechanism the (deprecated) in-process
  connector already uses -- and ''emits canonical medium="CPU" in both in-process
  and MP store events,'' i.e. it fits directly into llm-d''s/Dynamo''s existing GPU/CPU
  weight bucket rather than requiring new tier configuration. Explicitly STORE-ONLY:
  author states ''It does not publish BlockRemoved or AllBlocksCleared for MP host-cache
  eviction... I will work on this for next step'' -- so even once merged, evicted
  MP-tier blocks would not be retracted from a router''s view, a stale-placement risk.
  Validation cited: 51 unit tests passing (tests/v1/test_vllm_mp_adapter.py); a two-worker
  Kubernetes routing test with ''stock vLLM and Dynamo'' showing 12/12 correct cache-owner
  routing decisions with events enabled versus 4/12 without; 24,000 host-cache-hit
  tokens served; exact ownership for all 3,000 target chunks; zero Dynamo KV-event
  application errors. Validation was Dynamo-specific, not llm-d-specific, though the
  mechanism (standard vLLM BlockStored/medium=CPU over the normal KV-events path)
  is router-agnostic in principle. Discussion thread (Sep 12-14 2026) shows a competing
  proposal (from aeon-x, author of RFC #4352) to publish events directly from the
  MP process using a Dynamo-only KvStateAttachmentOwner interface, which DongDongJu''s
  simpler vLLM-connector approach was judged to supersede; PR #4995 (an earlier overlapping
  attempt by maobaolong) was closed in favor of #5076 on Sep 13 2026.'
---

*Suggested by [[mp-add-zmqkveventsink-re-emit-cache-events-in-vllm-kv-event-format-rebel-jinhwan]] — canonical upstream PR implementing MP-mode connector KV event publishing*

[Skip to content](https://github.com/LMCache/LMCache/pull/5076#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/LMCache/LMCache/pull/5076) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/LMCache/LMCache/pull/5076) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/LMCache/LMCache/pull/5076) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/LMCache/LMCache/pull/5076).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2FLMCache%2FLMCache) You must be signed in to change notification settings
  * [ Fork 1.9k ](https://github.com/login?return_to=%2FLMCache%2FLMCache)
  * [ Star  11.8k ](https://github.com/login?return_to=%2FLMCache%2FLMCache)


## Conversation
### 
commented [Sep 11, 2026](https://github.com/LMCache/LMCache/pull/5076#issue-5426380853)
Copy link  Copy Markdown 
Collaborator
**What this PR does / why we need it** :
Refs [#3661](https://github.com/LMCache/LMCache/issues/3661).
LMCache MP stores and retrieves KV correctly, but its vLLM connector does not currently publish completed MP stores through vLLM's KV-event path. KV-aware routers therefore cannot see which worker owns an LMCache host-cache prefix.
This change:
  * records MP store events only after the store future succeeds;
  * discards events for failed or degraded-mode stores;
  * computes event hashes with the MP server's configured hash algorithm;
  * converts completed stores to vLLM `BlockStored` events;
  * emits canonical `medium="CPU"` in both in-process and MP store events;
  * keeps cache residency independent from the input token tensor's device;
  * aggregates multi-worker events before `take_events()` drains them;
  * accepts both integer and byte-backed cache hashes.


[@maobaolong](https://github.com/maobaolong) : either [#4995](https://github.com/LMCache/LMCache/pull/4995) or this one fine. I made based on [#4995](https://github.com/LMCache/LMCache/pull/4995)
**Special notes for your reviewers** :
This is a store-only integration. It does not publish `BlockRemoved` or `AllBlocksCleared` for MP host-cache eviction. Until those lifecycle events exist, use this only where host-cache entries are not evicted during the routing window. I will work on this for next step.
Runtime requirements:
  * Enable vLLM KV events.
  * Match LMCache chunk size to the vLLM/Dynamo routing block size.
  * Set `lmcache.mp.hash_algorithm` to the MP server's hash algorithm.
  * Use the same `PYTHONHASHSEED` for the coordinator, MP servers, and workers.


Validation:
  * `tests/v1/test_vllm_mp_adapter.py`: 51 passed.
  * Ruff check and format check passed.
  * Two-worker Kubernetes routing with stock vLLM and Dynamo: 12/12 correct owners versus 4/12 without events.
  * Both MP caches served 24,000 host-cache hit tokens.
  * Coordinator ownership was exact for all 3,000 target chunks.
  * Dynamo reported zero KV-event application errors.


**If applicable** :
  * this PR contains user facing changes - docs added
  * this PR contains unit tests


` feat(vllm): publish MP connector KV events[](https://github.com/LMCache/LMCache/pull/5076/commits/2ffb636cca949e00cfa58d3afeeb793ed52d59b8 "feat\(vllm\): publish MP connector KV events

Signed-off-by: baoloongmao <307499405@qq.com>
Signed-off-by: Dongjoo Seo <commisori28@gmail.com>") `
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/LMCache/LMCache/pull/5076).

```
Signed-off-by: baoloongmao <307499405@qq.com>
Signed-off-by: Dongjoo Seo <commisori28@gmail.com>
```

[DongDongJu](https://github.com/DongDongJu) requested review from [ApostaC](https://github.com/ApostaC), [YaoJiayi](https://github.com/YaoJiayi), [deng451e](https://github.com/deng451e), [hickeyma](https://github.com/hickeyma), [hlin99](https://github.com/hlin99), [maobaolong](https://github.com/maobaolong), [sammshen](https://github.com/sammshen) and [zhengfeihe](https://github.com/zhengfeihe) as [code owners](https://github.com/LMCache/LMCache/blob/fcb67c0ab1db2a4bad78e085b3a2df33da003b7c/.github/CODEOWNERS#L4) [September 11, 2026 15:59](https://github.com/LMCache/LMCache/pull/5076#event-30983641240)
### 
commented [Sep 11, 2026](https://github.com/LMCache/LMCache/pull/5076#issuecomment-5637153211)
Copy link  Copy Markdown 
Collaborator Author  
|  [@maobaolong](https://github.com/maobaolong), [@ApostaC](https://github.com/ApostaC), [@sammshen](https://github.com/sammshen) PTAL  |  
| --- |  
### 
commented [Sep 12, 2026](https://github.com/LMCache/LMCache/pull/5076#issuecomment-5644517546) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/LMCache/LMCache/pull/5076).
Copy link  Copy Markdown 
Contributor  
|  Hi [@DongDongJu](https://github.com/DongDongJu), i am working on kv events integration with dynamo as well. My proposal was sending kv events directly in the LMCache MP process using the new dynamo.KvStateAttachmentOwner interface. This approach is much more complicated as it requires LMCache MP process to maintain the dynamo worker identity for each vLLM connected to LMCache MP process. And also requires extra conversion between chunk level kv stores to block level kv events. I think your solution of sending kv events directly in vLLM is much simpler and straight forward. However, I wonder how do you plan to support
  * kv events for eviction, since eviction only happens inside LMCache MP process
  * kv events for L2 storage and eviction

 |  
| --- |  
### 
commented [Sep 13, 2026](https://github.com/LMCache/LMCache/pull/5076#issuecomment-5652625284)
Copy link  Copy Markdown 
Collaborator  
|  [@DongDongJu](https://github.com/DongDongJu) Thanks for take this work, as you are working on this feature, i will close my PR [#4995](https://github.com/LMCache/LMCache/pull/4995) .  |  
| --- |  
### 
commented [Sep 14, 2026](https://github.com/LMCache/LMCache/pull/5076#issuecomment-5658719291)
Copy link  Copy Markdown 
Collaborator Author  
| 
> Hi [@DongDongJu](https://github.com/DongDongJu), i am working on kv events integration with dynamo as well. My proposal was sending kv events directly in the LMCache MP process using the new dynamo.KvStateAttachmentOwner interface. This approach is much more complicated as it requires LMCache MP process to maintain the dynamo worker identity for each vLLM connected to LMCache MP process. And also requires extra conversion between chunk level kv stores to block level kv events. I think your solution of sending kv events directly in vLLM is much simpler and straight forward. However, I wonder how do you plan to support
>   * kv events for eviction, since eviction only happens inside LMCache MP process
>   * kv events for L2 storage and eviction
> 

Hello [@aeon-x](https://github.com/aeon-x), Good point. I suggest keeping this PR store-only and adding a small MP-to-connector event channel as a follow-up. The MP process can reuse the existing L1_KEYS_EVICTED, L2_KEYS_STORED, and L2_KEYS_DELETED events, while the connector polls and translates them into vLLM BlockRemoved/BlockStored events. This avoids coupling the MP server directly to Dynamo. WDYT?  |  
| --- |  
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2FLMCache%2FLMCache%2Fpull%2F5076)
###  Labels 
None yet 
###  3 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
