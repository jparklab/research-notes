---
title: 'Support KV cache events in MP mode (for Dynamo KV-aware routing) · Issue #3661
  · LMCache/LMCache'
id: support-kv-cache-events-in-mp-mode-for-dynamo-kv-aware-routing-issue-3661-lmcach
tags:
- llmd-kv-cache-routing-c665d5
- lmcache-mp-mode
- kv-events-gap
created: '2026-09-14T13:01:17.074459Z'
updated: '2026-09-15T04:55:24.135363Z'
source: https://github.com/LMCache/LMCache/issues/3661
source_domain: github.com
fetched_at: '2026-09-14T13:01:17.053466Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'LMCache/LMCache issue #3661 (opened Jun 12 2026, still open/stale as of
  Aug 26 2026): as of this issue, LMCache MP mode (standalone server process, not
  co-located with the inference engine) does NOT emit any vLLM KV events -- vLLM''s
  connector take_events() returns empty because store events happen inside the LMCache
  server, not the engine, so ''Dynamo''s router is completely blind to the cache held
  by the LMCache server.'' Contributor Alorun''s Jun 24 design doc proposed bypassing
  vLLM''s event path entirely: the MP server would emit vLLM-wire-format BlockStored/BlockRemoved
  events over its own ZMQ socket to a Dynamo-side relay KvEventPublisher (needs a
  Dynamo Endpoint identity, which the standalone LMCache server lacks natively). By
  Jun 24 2026 the author reported local end-to-end validation (real MP server emits
  correctly-shaped events, a standalone Dynamo KV indexer ingests them) but ''full
  multi-GPU router validation still to come'' and no PR had landed; the issue went
  stale Aug 25 2026 with an unanswered Aug 26 ping. This design is Dynamo-specific
  (uses Dynamo''s KvEventPublisher/Endpoint concept), not llm-d-specific.'
---

*Suggested by [[lmcache-vllm-kv-events-missing-cpu-blockstored-event-only-gpu-event-emitted-issu]] — MP-mode event-support tracking issue, referenced as follow-on to the CPU-tier drop bug*

[Skip to content](https://github.com/LMCache/LMCache/issues/3661#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/LMCache/LMCache/issues/3661) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/LMCache/LMCache/issues/3661) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/LMCache/LMCache/issues/3661) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/LMCache/LMCache/issues/3661).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2FLMCache%2FLMCache) You must be signed in to change notification settings
  * [ Fork 1.9k ](https://github.com/login?return_to=%2FLMCache%2FLMCache)
  * [ Star  11.8k ](https://github.com/login?return_to=%2FLMCache%2FLMCache)


#  Support KV cache events in MP mode (for Dynamo KV-aware routing)
Copy link
Copy link
[Support KV cache events in MP mode (for Dynamo KV-aware routing)](https://github.com/LMCache/LMCache/issues/3661#top)#3661
Task
Copy link
Labels
[help wantedExtra attention is needed](https://github.com/LMCache/LMCache/issues?q=state%3Aopen%20label%3A%22help%20wanted%22)Extra attention is needed
## Description
opened [on Jun 12, 2026](https://github.com/LMCache/LMCache/issues/3661#issue-4652015264)
Last edited by glbyktjys
Collaborator
Issue body actions
### Background
In non-MP mode, LMCache emits a [CacheStoreEvent](https://github.com/LMCache/LMCache/blob/dev/lmcache/v1/cache_engine.py#L507) on each chunk store. vLLM pulls these event through the connector's [take_events() ](https://github.com/vllm-project/vllm/blob/main/vllm/distributed/kv_transfer/kv_connector/v1/base.py)and publishes them as [BlockStored events](https://github.com/vllm-project/vllm/blob/main/vllm/distributed/kv_events.py) (--kv-events-config) for consumers like Dynamo's KV-aware router.
See docs/source/production/kv_cache_events.rst.
### Problem
In MP mode, the cache engine runs in the LMCache server process rather than alongside the inference engine. As a result, cache store events are not propagated to the upstream engine.
### Goal
In MP mode, chunks stored by the LMCache server produce KV cache events published directly from the LMCache server to Dynamo's KV-aware router through Dynamo's KvEventPublisher, carrying the same information as non-MP BlockStored events. This bypasses vLLM's event path entirely.
### Task (not limited to the following)
  1. Write a design doc for the event path. Good reference: <https://docs.nvidia.com/dynamo/v1.1.1/integrations/kv-events-for-custom-engines>
  2. Event creation in the MP server when a chunk is stored.
  3. Integrate Dynamo's KvEventPublisher with the MP server.


## Activity
[glbyktjys](https://github.com/glbyktjys)
added 
[help wantedExtra attention is needed](https://github.com/LMCache/LMCache/issues?q=state%3Aopen%20label%3A%22help%20wanted%22)Extra attention is needed
[on Jun 12, 2026](https://github.com/LMCache/LMCache/issues/3661#event-26691126882)
[glbyktjys](https://github.com/glbyktjys)
added theissue type [on Jun 12, 2026](https://github.com/LMCache/LMCache/issues/3661#event-26691127003)
### Alorun commented on Jun 13, 2026 
Contributor
More actions
Hi! I'm very interested in this and would love to take it on.
I went through the relevant parts of all three projects (LMCache, vLLM, and Dynamo), plus the Dynamo "KV Events for Custom Engines" doc, to scope it out. Two findings:
**1. Hashing** — Dynamo expects cumulative _sequence_ `block_hashes` chained by `parent_hash`. LMCache's chunk hash is already prefix-chained and can emit signed-i64 (e.g. `sha256_cbor_64bit`), so we can supply those directly instead of building a new hash (`lmcache/v1/multiprocess/token_hasher.py`).
Dynamo additionally recomputes a per-block `tokens_hash` from the event's `token_ids` for tree lookup, so the emitted `token_ids` must be correct (`lib/kv-router/src/zmq_wire/convert.rs`).
**2. Block size** — `kv_block_size` must match the engine block size and only full blocks are accepted (`lib/kv-router/src/zmq_wire/convert.rs`), so the LMCache chunk (default 256) needs re-partitioning into engine-size blocks before publishing.
Could this be assigned to me? I'll put up a design doc as soon as possible.
### Sahib12spoke commented on Jun 13, 2026 
More actions
is there any scope [@glbyktjys](https://github.com/glbyktjys) i contribute along with [@Alorun](https://github.com/Alorun) ? If [@Alorun](https://github.com/Alorun) is fine
### sammshen commented on Jun 13, 2026 
Contributor
More actions
Hey [@Alorun](https://github.com/Alorun) [@Sahib12spoke](https://github.com/Sahib12spoke) if one of you could split the task into steps or you could guys collaborate on design docs, that is one way to collab on the scope
### glbyktjys commented on Jun 14, 2026 
CollaboratorAuthor
More actions
Thank you [@sammshen](https://github.com/sammshen) for helping provide guidance here. Yes, this would be a great opportunity to collaborate on the design doc first before moving into implementation。 We will also continue proposing new issues for the community, and we really appreciate your interest in contributing and moving LMCache forward!
### Alorun commented on Jun 14, 2026 
Contributor
More actions
# Design Doc: KV Cache Event Publishing in MP Mode (for Dynamo KV-aware Routing)
## 1. Overview
In MP (multiprocess) mode, have the LMCache server publish chunk **store / evict** events to Dynamo's KV-aware router, so the router can see what's in LMCache's shared cache and do cache-aware routing. This document covers the event-path design, the key technical decisions already settled, and the main design choices.
## 2. Background & Problem
**Non-MP mode (current, working):** LMCache emits a `CacheStoreEvent` on each chunk store; vLLM pulls these via its LMCache connector's `take_events()`, translates them into `BlockStored`, and publishes them through the `--kv-events-config` ZMQ publisher for consumers like Dynamo's router.
> Note: the caller of `get_kv_events()` on this path lives in the **vLLM repo's** LMCache connector (cross-repo), so looking only inside the LMCache repo makes it appear to have "no caller / a dead path."
**MP mode (problem):** The cache engine runs in a standalone LMCache server process, no longer co-located with the inference engine. Chunks are stored by the LMCache server, vLLM's `take_events()` returns empty, so store events never reach the upstream engine — **Dynamo's router is completely blind to the cache held by the LMCache server.**
**Goal:** In MP mode, chunks stored by the LMCache server directly produce KV cache events that reach Dynamo's router, carrying information equivalent to non-MP `BlockStored`, bypassing vLLM's event path.
## 3. Goals & Non-Goals
**Goals**
  * The MP server produces events on chunk store / evict and delivers them to Dynamo's router, with information aligned to non-MP `BlockStored`.


**Non-Goals (v1)**
  * Only local L1 store / evict; remote / L2 storage tiers are out of scope.
  * Worker attribution is v1 only (see D2).


## 4. Event Flow
  * **Old (non-MP):** LMCache (in-process) → vLLM `take_events()` → vLLM ZMQ publisher → Dynamo router
  * **New (MP):** LMCache MP server → (self-built events, vLLM format) → ZMQ socket → (Dynamo-side relay forwards) → event plane → router


## 5. Key Technical Design (largely settled)
  * **Hashing:** Dynamo's `block_hashes` must be **cumulative sequence hashes** , chained via `parent_hash`. LMCache's chunk hash is already prefix-chained and can emit signed-i64 values (`sha256_cbor_64bit`), so **we reuse it directly — no need to reimplement a hash function.** Dynamo separately recomputes a `tokens_hash` from the event's `token_ids` for its index lookup, so the emitted `token_ids` must be correct.
  * **Block size:** `kv_block_size` must equal the engine block size; Dynamo only accepts full blocks (on the first block whose token count != `kv_block_size` it breaks — that block and everything after it are dropped). The LMCache chunk (default 256) must be **re-partitioned** into engine-size blocks, emitting only full blocks; constraint `chunk_size % kv_block_size == 0`, trailing partial block dropped.
  * **parent chain:** within a chunk `parent = previous block`; a chunk's first block links to the previous chunk's last block; the sequence start is `None`.
  * **Generation point:** for store, hook the existing `L1_WRITE_FINISHED` (enrich its metadata with `token_ids` when enabled — it currently carries only the `ObjectKey`) or hook the store handler directly; for eviction, hook `L1_KEYS_EVICTED` (carries `chunk_hash`).
  * **Publishing:** ZMQ relay (see §6 · D1).


## 6. Key Decisions
### D1 · Publishing mechanism: ZMQ relay (adopted)
**Background:** Constructing Dynamo's `KvEventPublisher` requires a Dynamo `Endpoint` (a logical identity: namespace / component / endpoint), which only members of the Dynamo system have. The LMCache MP server is a standalone process outside that system; it has no such Endpoint and no ready way to generate one.
**Approach:** The LMCache MP server simply emits events in the **vLLM-compatible wire format** to a ZMQ socket; a relay-mode `KvEventPublisher` running **on the Dynamo side** (which is itself a Dynamo member and has an Endpoint) subscribes to that socket and forwards the events into Dynamo's event plane → router.
### D2 · Worker attribution under shared cache
`KvEventPublisher` / the router organize events by `(worker_id, dp_rank)`, but in MP mode the L1 cache is shared across all local pods with no single owner, and the MP server does not know the Dynamo identity of the workers connected to it (only a TP/PP slice rank and the local PID). v1 attributes each event to the worker that triggered the store (the information is correct and it is simple to implement); a better attribution scheme is left for later discussion.
### D3 · Reproducing block_hash at eviction
The cumulative `block_hash` is computed from `token_ids` at store time; but the eviction event provides only `chunk_hash`, with no `token_ids`, so it cannot be recomputed. **Approach:** at store time, maintain a small `chunk_hash → [block_hashes]` table; at eviction, look up by `chunk_hash` to get the `block_hashes` to remove and pass them to `publish_removed`.
## 7. Configuration / CLI
  * `--enable-dynamo-kv-events` (off by default; when off, no effect on existing behavior)
  * `--kv-block-size` (must equal the engine block size)
  * ZMQ bind address (`host:port`, for the Dynamo-side relay to connect to)


## 8. Testing
  * **Unit:** partitioning (256 → engine-size blocks, dropping the tail); cumulative `block_hash` + `parent_hash` chain (deterministic, correctly chained across chunks); `block_hash` reproduced consistently at eviction.
  * **Integration:** subscribe to the LMCache ZMQ socket, store a chunk → assert the emitted `BlockStored` fields are correct; evict → assert `BlockRemoved`. Dynamo's exported `compute_block_hash_for_seq` can be used to verify.
  * Follow CONTRIBUTING: test the public interface, include tests, pass `pre-commit`.


### glbyktjys commented on Jun 15, 2026 
Last edited by glbyktjys
CollaboratorAuthor
More actions
Hi [@Alorun](https://github.com/Alorun), thanks for sharing the design! [@sammshen](https://github.com/sammshen) could you please take a look at the proposed direction, particularly Section 6 D2, when you have a chance? Should Dynamo communicate with the MP coordinator directly, or would there be a better way to implement this?
Also, [@Alorun](https://github.com/Alorun), please feel free to join our Slack channel for easier discussion and coordination if you think there is a good opportunity to collaborate with [@Sahib12spoke](https://github.com/Sahib12spoke): <https://join.slack.com/t/lmcacheworkspace/shared_invite/zt-3zxjao8h0-lRfBfnLqbALOtLsWn2ITxA>
### Sahib12spoke commented on Jun 15, 2026 
More actions
Hi [@glbyktjys](https://github.com/glbyktjys) absolutely would be eager to love to contribute If [@Alorun](https://github.com/Alorun) you are fine I will ping you from my account [@sahibpreetsingh12](https://github.com/sahibpreetsingh12)
### sahibpreetsingh12 commented on Jun 15, 2026 
Contributor
More actions
Hi [@Alorun](https://github.com/Alorun) I read about it First of all great design doc and really solid scoping doc, the ZMQ-relay choice (D1) and the eviction lookup table (D3) both read well. I don't want to duplicate your design work, so I'll offer the part I can genuinely add: I have an RTX 5090 box and I'm happy to own the end-to-end validation.
Concretely, once your implementation takes shape I'd stand up vLLM + LMCache in MP mode + a Dynamo router on real hardware, subscribe to the event socket, and verify he emitted BlockStored/BlockRemoved fields against Dynamo's compute_block_hash_for_seq — including the 256→engine-block re-partitioning and the eviction path.
To be Honest: I'm newer to the ZMQ/relay side, so that design stays yours — I'd plug in on the integration-test half. And since it's a single GPU, I'd demo cache-aware routing with two small DP workers rather than a full cluster, which should still exercise the event correctness that matters most here.
Open to a collaborator on the testing/integration side?
### Alorun commented on Jun 16, 2026 
Contributor
More actions
Thanks [@sahibpreetsingh12](https://github.com/sahibpreetsingh12) — great split, and real-hardware end-to-end validation (the 5090) is genuinely valuable since I don't have a comparable setup. Happy to have you own the testing/integration half; ping me anytime and I'll join the Slack to coordinate (thanks [@glbyktjys](https://github.com/glbyktjys)).
### sahibpreetsingh12 commented on Jun 16, 2026 
Contributor
More actions
Hi [@Alorun](https://github.com/Alorun) First of all thanks for sharing would try and do my best and lets get it sorted :) I am on slack with name Sahib
### glbyktjys commented on Jun 23, 2026 
CollaboratorAuthor
More actions
Hi [@Alorun](https://github.com/Alorun), just checking in on this issue. No rush just let us know if you’re still working on it or if there’s anything we can help with. Thanks!
### Alorun commented on Jun 24, 2026 
Contributor
More actions
> Hi [@Alorun](https://github.com/Alorun), just checking in on this issue. No rush just let us know if you’re still working on it or if there’s anything we can help with. Thanks!
Hi [@glbyktjys](https://github.com/glbyktjys), thanks for checking in — still actively working on it. The implementation is largely done (config, core hashing/block-building, eviction-replay table, ZMQ emitter, and the store/evict wiring), with unit and component tests passing. I've also validated end-to-end locally: a real MP server emits correctly-shaped vLLM BlockStored/BlockRemoved events, and a real standalone Dynamo KV indexer ingests them (radix tree + overlap scores confirmed). Full multi-GPU router validation is still to come. Before opening the PR I want to do a careful self-review to trim the diff, since it's grown fairly large. Will follow up here when it's up — happy to take any early guidance in the meantime.
👍React with 👍2Reacted by Jiayue Chen and Yihua Cheng
### glbyktjys commented on Jun 25, 2026 
Last edited by glbyktjys
CollaboratorAuthor
More actions
Hi [@Alorun](https://github.com/Alorun) this sounds good. I think this may need more involvement from the Dynamo side as well. Would you be able to join our Slack channel so we can discuss this more easily? We would also be happy to set up a meeting, since this is a great effort from you and we want to make sure it is on the right track.
You can join slack channel here~ <https://join.slack.com/t/lmcacheworkspace/shared_invite/zt-3zxjao8h0-lRfBfnLqbALOtLsWn2ITxA>
### glbyktjys commented on Jun 25, 2026 
CollaboratorAuthor
More actions
One thing we may need to double-check is the current assumption around chunk size. It looks like the implementation may require LMCache to use the same chunk size as vLLM blocks, which could be a bit limiting.
Another point is Dynamo expect events at the vLLM instance level, while LMCache currently emits events at the node level. It would be great if we could discuss this more on Slack!
### Alorun commented on Jun 26, 2026 
Contributor
More actions
Thanks — really appreciate that. I've joined the Slack channel, and I'd be glad to meet as well if that's easier. Agreed that some questions will need input from the Dynamo side too, so working through it together sounds right. I have also considered the two points you mentioned. Chunk size: it doesn't actually require LMCache's chunk size to match the vLLM block size —the implementation re-slices each chunk into kv_block_size (vLLM-block-sized) blocks at publish time, since Dynamo expects events at vLLM-block granularity. The only constraint is that chunk_size be a multiple of kv_block_size (true for common configs like 256 / 16). Node vs instance level: agreed this is the main open question. The MP cache is shared across local workers, so it doesn't map to a single vLLM instance the way Dynamo expects. I've kept the worker attribution configurable (single-DP for now) since the right approach isn't settled — happy to dig into the options on Slack.
👍React with 👍1Reacted by Jiayue Chen
### sahibpreetsingh12 commented on Jun 26, 2026 
Contributor
More actions
Hi [@Alorun](https://github.com/Alorun) If you are open I can work on checking once things are done. If you are still open I am on slack Sahib
> Thanks [@sahibpreetsingh12](https://github.com/sahibpreetsingh12) — great split, and real-hardware end-to-end validation (the 5090) is genuinely valuable since I don't have a comparable setup. Happy to have you own the testing/integration half; ping me anytime and I'll join the Slack to coordinate (thanks [@glbyktjys](https://github.com/glbyktjys)).
👍React with 👍1Reacted by Jiayue Chen
### Alorun commented on Jun 26, 2026 
Contributor
More actions
> Hi [@Alorun](https://github.com/Alorun) If you are open I can work on checking once things are done. If you are still open I am on slack Sahib
>> Thanks [@sahibpreetsingh12](https://github.com/sahibpreetsingh12) — great split, and real-hardware end-to-end validation (the 5090) is genuinely valuable since I don't have a comparable setup. Happy to have you own the testing/integration half; ping me anytime and I'll join the Slack to coordinate (thanks [@glbyktjys](https://github.com/glbyktjys)).
Thanks [@sahibpreetsingh12](https://github.com/sahibpreetsingh12), appreciate the offer! I've got a GPU available for testing, so I can handle that part myself. There're still some open design questions to sort out first, but I'll get the PR up once that's settled. See you on Slack
👍React with 👍1Reacted by Jiayue Chen
❤️React with ❤️1Reacted by sahibpreet singh
### sahibpreetsingh12 commented on Jun 26, 2026 
Contributor
More actions
[@Alorun](https://github.com/Alorun) sure got it will wait once you raise a PR so i can learn about all this :)
### github-actions commented on Aug 25, 2026 
[on Aug 25, 2026](https://github.com/LMCache/LMCache/issues/3661#issuecomment-5419587232) – with [GitHub Actions](https://help.github.com/en/actions)
Contributor
More actions
This issue has been automatically marked as stale because it has not had activity within 60 days. It will be automatically closed if no further activity occurs within 30 days.
[github-actions](https://github.com/apps/github-actions)
added [on Aug 25, 2026](https://github.com/LMCache/LMCache/issues/3661#event-30016832191)
### bluayer commented on Aug 26, 2026 
Contributor
More actions
Hi team, just checking if there are any updates on this. Thanks!
[github-actions](https://github.com/apps/github-actions)
removed [on Aug 26, 2026](https://github.com/LMCache/LMCache/issues/3661#event-30080357383)
[Sign up for free](https://github.com/signup?return_to=https://github.com/LMCache/LMCache/issues/3661)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/LMCache/LMCache/issues/3661)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
[help wantedExtra attention is needed](https://github.com/LMCache/LMCache/issues?q=state%3Aopen%20label%3A%22help%20wanted%22)Extra attention is needed
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
