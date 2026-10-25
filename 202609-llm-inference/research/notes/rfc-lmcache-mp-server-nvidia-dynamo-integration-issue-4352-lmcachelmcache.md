---
title: '[RFC] LMCache MP server NVIDIA Dynamo integration · Issue #4352 · LMCache/LMCache'
id: rfc-lmcache-mp-server-nvidia-dynamo-integration-issue-4352-lmcachelmcache
tags:
- llmd-kv-cache-routing-c665d5
- lmcache-mp-mode
- kv-events-gap
- dynamo-specific
created: '2026-09-14T13:04:54.217871Z'
updated: '2026-09-15T04:55:24.148119Z'
source: https://github.com/LMCache/LMCache/issues/4352
source_domain: github.com
fetched_at: '2026-09-14T13:04:54.197207Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'LMCache/LMCache issue #4352, ''[RFC] LMCache MP server NVIDIA Dynamo integration''
  (opened Jul 30 2026 by aeon-x). Explicitly and exclusively scoped to NVIDIA Dynamo
  -- llm-d is never mentioned in the RFC body, despite being cited elsewhere (in the
  rebel-jinhwan ZmqKVEventSink fork commit) as ''the RFC'' behind llm-d EPP support.
  Motivation: because the LMCache MP server is a standalone process (not an inference
  worker), naively sending its KV events to Dynamo would index them under the MP server''s
  own identity, not crediting any actual vLLM worker in Dynamo''s routing. Two proposals:
  (1) LMCache MP server fan-out-registers its ZMQ PUB endpoint under each connected
  engine''s (worker_id, dp_rank) so all connected workers get credited when it emits
  events -- blocked because Dynamo''s indexer registry currently rejects a second
  registration for an existing worker_id/dp_rank; (2) register LMCache as a Dynamo
  ''shared cache'' queried via a worker-agnostic pull path at route time (LMCache
  coordinator''s /directory/lookup_tokens API, returning per-chunk placements with
  instance_id/tier/backend/size_bytes), folded into scoring as a shared_cache_multiplier
  with soft-fail to indexer-only scoring -- modeled on how Dynamo''s SGLang-HiCache-over-Mooncake
  shared-cache path already works. Proposal 2 was endorsed by LMCache maintainer DongDongJu
  (Jul 31 2026) as the preferred direction. Confirms the SGLang tier-transition event
  vocabulary Dynamo expects: store(GPU), store(CPU_PINNED) after GPU-to-host DMA,
  remove(GPU) on GPU eviction, remove(CPU_PINNED) on host eviction, store(GPU) on
  load-back promotion, store(CPU_PINNED) on external-to-host prefetch.'
---

*Suggested by [[mp-add-zmqkveventsink-re-emit-cache-events-in-vllm-kv-event-format-rebel-jinhwan]] — RFC explicitly naming llm-d's EPP as a consumer of MP-mode engine-format KV events*

[Skip to content](https://github.com/LMCache/LMCache/issues/4352#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/LMCache/LMCache/issues/4352) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/LMCache/LMCache/issues/4352) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/LMCache/LMCache/issues/4352) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/LMCache/LMCache/issues/4352).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2FLMCache%2FLMCache) You must be signed in to change notification settings
  * [ Fork 1.9k ](https://github.com/login?return_to=%2FLMCache%2FLMCache)
  * [ Star  11.8k ](https://github.com/login?return_to=%2FLMCache%2FLMCache)


#  [RFC] LMCache MP server NVIDIA Dynamo integration
Copy link
Copy link
[[RFC] LMCache MP server NVIDIA Dynamo integration](https://github.com/LMCache/LMCache/issues/4352#top)#4352
Copy link
## Description
opened [on Jul 30, 2026](https://github.com/LMCache/LMCache/issues/4352#issue-5024253617)
Last edited by aeon-x
Contributor
Issue body actions
## Motivation
The LMCache MP server is a standalone process, not an inference worker. Simply sending KV events from LMCache MP Server to NVIDIA Dynamo therefore does not affect Dynamo's KV routing decisions, as the events would be indexed under the MP server's own identity.
This RFC proposes solutions to attribute LMCache's cache contents so they actually count in Dynamo's routing.
## Background: how Dynamo credits cached blocks
**KV events** Inference engines (workers) send `BlockStored` / `BlockRemoved` events carrying a `medium` field on every tier transition
Sglang Example:  
| Transition  | Event emitted  |  
| --- | --- |  
| Fresh prefill writes blocks to GPU  | `store(GPU)`  |  
| GPU → Host copy (after async DMA completes)  | `store(CPU_PINNED)`  |  
| GPU evicted, block still resident on Host  | `remove(GPU)`  |  
| Host evicted (block gone from all worker tiers)  | `remove(CPU_PINNED)`  |  
| Host → GPU promotion (`load_back`)  | `store(GPU)`  |  
| External → Host prefetch (L2 materialization)  | `store(CPU_PINNED)`  |  
Events will be owned by LMCache MP server: store(CPU_PINNED), remove(CPU_PINNED), store(External), remove(External)
**KV events are attributed per stream registration, not per message.** The KV-event payload carries no worker id (only `data_parallel_rank` at the batch level). Identity is bound at registration: the indexer's `POST /register` maps one ZMQ endpoint to one `(worker_id/instance_id, dp_rank)`, and every event on that subscription is indexed under that id.
## Proposal 1 — LMCache push KV events under its N engines' worker IDs
  1. Each engine passes its (worker_id / instance_id, dp_rank) to LMCache when it connects.
  2. **Fan-out register LMCache ZMQ PUB endpoint under N engines** : LMCache MP server registers with the KV indexer N times — once per connected engine — and reconciles register/unregister as engines come and go.
  3. All workers registered under LMCache MP server will be **credited** when LMCache MP server sends KV events.


**Blocker** : The indexer registry currently rejects a second registration for an existing (worker_id / instance_id, dp_rank), so an engine and LMCache cannot both register under the same worker ID.
## Proposal 2 — Register LMCache as a shared cache, Pull token placement
Alongside the kv events sent by workers, dynamo also supports a **worker-agnostic pull path** : at route time it queries a shared cache backend (SGLang HiCache over Mooncake) and credits the hits to all workers.
### LMCache Coordinator Token Placement Look Up API
  1. Inference engines advertise the node's MP server's instance_id in their runtime config. node A: worker A1, A2 → lmcache-instance-1 node B: worker B1, B2 → lmcache-instance-2
  2. The router can query the existing LMCache coordinator `/directory/lookup_tokens` API to check the kvcache placement by token_ids



```
# Sample Request
curl -X POST http://coordinator:9300/directory/lookup_tokens \
  -H 'Content-Type: application/json' \
  -d '{
    "model_name": "meta-llama/Llama-3.1-8B-Instruct",
    "world_size": 1,
    "token_ids": [1000, 1001, 1002, "... 512 tokens total ..."],
  }'

# Sample Response
{
  "chunks": 2,
  "results": [
    {
      "placements": [
        {
          "instance_id": "lmcache-instance-1",
          "incarnation": 3,
          "tier": "l1",
          "backend": "dram",
          "size_bytes": 4194304
        },
        {
          "instance_id": "lmcache-instance-2 ",   # ignore 
          "incarnation": 3,
          "tier": "l2",
          "backend": "mooncake",                                        
          "size_bytes": 4194304
        }
      ]
    }

```

  1. Folding the result into scoring as shared_cache_multiplier * hits_beyond(device_blocks), then credit the workers. Failures soft-fail to indexer-only scoring, exactly as HiCache does today.


  * L1 (CPU dram) placement will credit all the workers connected to this lmcache instance (in the same node).
  * L2 (External storage) placement will credit all the workers in the cluster.


👍React with 👍5Reacted by Yihua Cheng, Zhengfei He, Dongjoo Seo, Ziqi Fan and Changhyeon Nam
## Activity
[aeon-x](https://github.com/aeon-x)
changed the title ~~[-] RFC: LMCache MP server Dynamo integration[/-]~~ [+][RFC] LMCache MP server Dynamo integration[/+] [on Jul 30, 2026](https://github.com/LMCache/LMCache/issues/4352#event-28746700333)
[aeon-x](https://github.com/aeon-x)
changed the title ~~[-][RFC] LMCache MP server Dynamo integration[/-]~~ [+][RFC] LMCache MP server NVIDIA Dynamo integration[/+] [on Jul 30, 2026](https://github.com/LMCache/LMCache/issues/4352#event-28746719572)
### DongDongJu commented on Jul 31, 2026 
Collaborator
More actions
[@aeon-x](https://github.com/aeon-x) Thanks for great suggestion. with current all other movement in LMCache, Proposal 2 is make sense to me. What is the SharedKVCache backend? its like virtual memory concept?
### aeon-x commented on Aug 3, 2026 
ContributorAuthor
More actions
> [@aeon-x](https://github.com/aeon-x) Thanks for great suggestion. with current all other movement in LMCache, Proposal 2 is make sense to me. What is the SharedKVCache backend? its like virtual memory concept?
Hey [@DongDongJu](https://github.com/DongDongJu) , thanks for the review. Updated this RFC, SharedKVCache backend is a component in dynamo framework that supports pulling to check if kvcache exists, then credit the workers accordingly in kvcache aware routing.
[rebel-jinhwan](https://github.com/rebel-jinhwan)
added a commit that references this issue [on Aug 27, 2026](https://github.com/LMCache/LMCache/issues/4352#event-30104210525)
[[MP] Add ZmqKVEventSink: re-emit cache events in vLLM KV-event format…](https://github.com/rebel-jinhwan/LMCache/commit/4f5040fccdbbaf995717c4b4606922ec084c1472)
[Sign up for free](https://github.com/signup?return_to=https://github.com/LMCache/LMCache/issues/4352)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/LMCache/LMCache/issues/4352)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
No labels
No labels
No type
### Projects
No projects
### Milestone
No milestone
### Relationships
None yet
### Development
No branches or pull requests
### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
