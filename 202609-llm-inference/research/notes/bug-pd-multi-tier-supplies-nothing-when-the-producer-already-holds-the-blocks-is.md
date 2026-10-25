---
title: '[Bug]: PD Multi Tier supplies nothing when the producer already holds the
  blocks · Issue #52808 · vllm-project/vllm'
id: bug-pd-multi-tier-supplies-nothing-when-the-producer-already-holds-the-blocks-is
tags:
- llm-d
- kv-transfer-bug
- vllm
- llmd-kv-cache-routing-c665d5
created: '2026-09-14T11:56:34.637949Z'
updated: '2026-09-15T04:55:24.077118Z'
source: https://github.com/vllm-project/vllm/issues/52808
source_domain: github.com
fetched_at: '2026-09-14T11:56:34.617427Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Closed upstream vLLM GitHub issue #52808 (opened Aug 18 2026 by nilig, same
  reporter as llm-d-router#2438) diagnosing a root-cause defect in vLLMs PD Multi
  Tier KV-offload path (remote_decoder/remote_prefiller, secondary tier type=p2p,
  OffloadingConnector with TieringOffloadingSpec). When a producer already holds the
  requested prefix blocks (i.e. the prefix cache worked), it creates no store job,
  so the consumers fetch demand parks in the demanded map and is never matched --
  the consumer waits out the hard-coded _LOAD_TIMEOUT_S=30.0s and recomputes locally.
  Effect is inverted from intent: the better the producers prefix cache, the more
  often PD Multi Tier fails to deliver. Root cause: P2PTierManager.on_new_request
  defaults offloading_context to BLOCK_LEVEL policy, which skips prefix-hit chunks
  from being offloaded (next_stored_chunk_idx stays at num_chunks), and grep confirms
  REQUEST_LEVEL is never declared by any tier (p2p/fs/obj/example) in v0.27.1 or main,
  so the cascade to request-level tiers never runs. Reproduced with isolated HTTP
  calls: warm producer -> consumer took 32.9s vs 0.16s cold; identical in a real P/D
  pair (30.2s/0 bytes warm vs 0.086s/256MiB cold). At scale (8 producers + 8 consumers,
  10,800 requests, shared-prefix workload): fetch-demand match rate fell from 93.8%
  (cold) to 51.5% (warming) to 36.7% (warm) as producer cache warmed up. 6,083 load
  timeouts logged against 10,800 requests; PD Multi Tier ended up SLOWER than plain
  NIXL P/D on identical hardware (1571s vs 1032s wall time to completion; mean TPOT
  92ms vs 10.7ms). Maintainer liranschour confirmed the fix direction: the p2p tier
  should declare REQUEST_LEVEL (bug, not by design).'
---

*Suggested by [[sidecar-pd-multi-tier-decode-leg-is-dispatched-concurrently-with-prefill-chargin]] — referenced as related unresolved upstream vLLM defect causing KV-transfer timeouts*

[Skip to content](https://github.com/vllm-project/vllm/issues/52808#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/52808) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/52808) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/vllm-project/vllm/issues/52808) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/vllm-project/vllm/issues/52808).
/ Public
  * Sponsor
#  Sponsor vllm-project/vllm 
##### GitHub Sponsors
[Learn more about Sponsors](https://github.com/sponsors)
[ vllm-project ](https://github.com/vllm-project)
[ vllm-project ](https://github.com/vllm-project)
[ Sponsor  ](https://github.com/sponsors/vllm-project)
##### External links
[opencollective.com/**vllm**](https://opencollective.com/vllm)
[Learn more about funding links in repositories](https://docs.github.com/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/displaying-a-sponsor-button-in-your-repository). 
[Report abuse](https://github.com/contact/report-abuse?report=vllm-project%2Fvllm+%28Repository+Funding+Links%29)
  * [ Notifications ](https://github.com/login?return_to=%2Fvllm-project%2Fvllm) You must be signed in to change notification settings
  * [ Fork 22.2k ](https://github.com/login?return_to=%2Fvllm-project%2Fvllm)
  * [ Star  91.7k ](https://github.com/login?return_to=%2Fvllm-project%2Fvllm)


#  [Bug]: PD Multi Tier supplies nothing when the producer already holds the blocks #52808
Copy link
Copy link
Closed
Closed
[[Bug]: PD Multi Tier supplies nothing when the producer already holds the blocks](https://github.com/vllm-project/vllm/issues/52808#top)#52808
Copy link
## Description
opened [on Aug 18, 2026](https://github.com/vllm-project/vllm/issues/52808#issue-5184938345)
Last edited by nilig
Issue body actions
### Summary
In the PD Multi Tier flow (`remote_decoder` / `remote_prefiller`, secondary tier of `type: p2p`), a producer that **already holds the requested blocks** supplies nothing to the consumer. The consumer's fetch demand parks unsatisfied, waits out `_LOAD_TIMEOUT_S = 30.0`, aborts, and recomputes the prompt locally.
The effect is inverted from the intent: the better the producer's prefix cache works, the more often the Multi Tier path fails to deliver.
### Environment
  * vLLM `v0.27.1`, stock `docker.io/vllm/vllm-openai:v0.27.1`
  * `openai/gpt-oss-120b`, TP=1, `--block-size 128`, `--no-disable-hybrid-kv-cache-manager`, `PYTHONHASHSEED=0` on all instances
  * 2x H200, RDMA between pods
  * Connector on both instances:



```
--kv-transfer-config '{"kv_connector":"OffloadingConnector","kv_role":"kv_both",
  "kv_load_failure_policy":"recompute",
  "kv_connector_extra_config":{"spec_name":"TieringOffloadingSpec",
    "cpu_bytes_to_use":107374182400,"block_size":128,"eviction_policy":"lru",
    "offload_prompt_only":false,
    "secondary_tiers":[{"type":"p2p","host":"$(POD_IP)","port":7777}]}}'

```

Two files (`tiering/p2p/session/server.py`, `.../client.py`) carried added `logger.info` counters only, no behavioural change; the timings and byte counts below come from vLLM's own metrics and logs.
### Reproducer
Two instances, A (producer) and B (consumer). No P/D topology, router or proxy required — three HTTP calls.
  1. **Producer leg** on A: `POST /v1/completions`, `max_tokens=1`, `kv_transfer_params={"remote_decoder":{"kv_request_id":ID}}`
  2. **Consumer leg** on B: `POST /v1/completions`, `kv_transfer_params={"remote_prefiller":{"kv_request_id":ID,"remote_host":A,"remote_port":7777}}`


Run that pair twice with a **unique prompt each time** (so the consumer is always cold). Before the second pair, send the prompt to A once on its own with no `kv_transfer_params`, so A already holds the blocks.
Prompt ~8K tokens. Watch `vllm:kv_offload_load_bytes_total` on B.
### Observed  
| case  | producer leg  | consumer leg  | consumer bytes loaded  |  
| --- | --- | --- | --- |  
| A cold for the prefix  | 0.20 s  | 0.16 s  | +268,959,744 (256 MiB)  |  
| A pre-warmed  | 0.03 s  | **32.9 s**  |  
Identical with a real P/D pair (prefill producer, decode consumer): 0.086 s / +256 MiB cold vs 30.2 s / 0 bytes warm. Reproduces every time.
Consumer log for the warm case:

```
P2PSession <A>:7777: <id> round=0 timed out, sending abort
P2PSession <A>:7777: load request <id> (job_id=N) timed out; load job completed
with failure. If this recurs, ensure PYTHONHASHSEED is set to the same value on
all nodes.

```

`PYTHONHASHSEED=0` was set and verified identical on every instance, so that hint is misleading for this failure mode.
### Analysis
Supply for a PD round can only come from that request's store jobs (`session/server.py` `_OutboundRequestState.add_stored_blocks`). A warm producer creates no store job, so demand parked in `demanded` (`key -> remote_block_idx: blocks peer wants, awaiting supply`) is never matched.
Two gates prevent the blocks from being offered:
  1. `distributed/kv_transfer/kv_connector/v1/offloading/scheduler.py` (~L941):



```
# Skip prefix-hit chunks for block-level policy; for
# request-level, next_stored_chunk_idx stays at 0 so all
# chunks (including hits) are offloaded.
if req_status.offloading_context.policy == OffloadPolicy.BLOCK_LEVEL:
    group_state.next_stored_chunk_idx = num_chunks
```

`P2PTierManager.on_new_request` returns a bare `RequestOffloadingContext()`, which defaults to `BLOCK_LEVEL`. `grep -c REQUEST_LEVEL` is **0** for every tier (`p2p`, `fs`, `obj`, `example`) at `v0.27.1` **and on`main`** , so `request_level_tiers` is always empty and `TieringOffloadingManager._cascade_existing_blocks_to_request_level_tiers` never runs.
  1. Forcing `REQUEST_LEVEL` for producer-side requests clears gate 1 — verified, the request then reaches the store path with `next_stored_chunk_idx=0` and 56 storable chunks — but the keys are then dropped by `if block_id == 0: continue`, since a prefix-hit request has no GPU block ids recorded for those blocks (measured: 55 keys dropped). `new_offload_keys` empties, `prepare_store` is skipped, and with it the cascade.


### Why symmetric (non-PD) Multi Tier is unaffected
`session/server.py`:
> `# Supply came from inbound lookup pins (symmetric): no late submit_store can arrive, so unmatched fetch demand fails fast. PD rounds park demand for stores instead.`
`session/client.py`:
> `PD-only loads leave probes/unsent empty ... PD clients never probe and stay on round 0.`
The symmetric path supplies from tier lookups and pins, which need neither a store job nor GPU block ids, so a warm producer is its best case. PD has no equivalent route.
### Impact at scale
Shared-prefix workload, 8 producers + 8 consumers, 10,800 requests. Supply match rate tracks producer cache warmth:  
| cold  | warming  | warm  |  
| --- | --- | --- |  
| fetch demand matched  | 93.8%  | 51.5%  | **36.7%**  |  
6,083 load timeouts were logged against 10,800 requests. End to end this made PD Multi Tier slower than plain NIXL P/D on the same hardware (1571 s vs 1032 s to completion; mean TPOT 92 ms vs 10.7 ms).
### Question
Should the PD Multi Tier path gain a lookup/pin supply route like the symmetric one, or should the p2p tier declare `REQUEST_LEVEL` (and the store path then be able to offload prefix-hit blocks that have no GPU block id)? The second looks like it fights both the `BLOCK_LEVEL` optimisation and the missing block-id constraint, so the first seems closer to the existing design — but that is your call.
cc [@liranschour](https://github.com/liranschour)
👍React with 👍1Reacted by Almog Tavor
## Activity
[nilig](https://github.com/nilig)
changed the title ~~[-][Bug]: P2P secondary tier supplies nothing in the PD flow when the producer already holds the blocks[/-]~~ [+][Bug]: PD Multi Tier supplies nothing when the producer already holds the blocks[/+] [on Aug 18, 2026](https://github.com/vllm-project/vllm/issues/52808#event-29639922762)
### liranschour commented on Aug 19, 2026 
Contributor
More actions
[@nilig](https://github.com/nilig) the p2p tier should declare REQUEST_LEVEL. Somehow the current code declare p2p as BLOCK_LEVEL. Will fix this bug.
[almogtavor](https://github.com/almogtavor)
added a commit that references this issue [on Aug 22, 2026](https://github.com/vllm-project/vllm/issues/52808#event-29845371128)
[[Bugfix][KV Offload] Guard P2P supply against consumer demand](https://github.com/almogtavor/vllm/commit/8da15e841c9e05584e92f906b07408a409cad388)
[tlrmchlsmth](https://github.com/tlrmchlsmth)
closed this as [completed](https://github.com/vllm-project/vllm/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)in [#52912](https://github.com/vllm-project/vllm/pull/52912)[on Aug 31, 2026](https://github.com/vllm-project/vllm/issues/52808#event-30271362547)
[Sign up for free](https://github.com/signup?return_to=https://github.com/vllm-project/vllm/issues/52808)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/vllm-project/vllm/issues/52808)
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
  * [[Bugfix][KVOffload] P2P tier declares REQUEST_LEVEL on the producer legvllm-project/vllm](https://github.com/vllm-project/vllm/pull/52912)[Releasev0.29.0Latest](https://github.com/vllm-project/vllm/releases/tag/v0.29.0)


### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
