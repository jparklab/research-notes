---
title: 'sidecar: PD Multi Tier decode leg is dispatched concurrently with prefill,
  charging producer latency to the connector''s load deadline · Issue #2438 · llm-d/llm-d-router'
id: sidecar-pd-multi-tier-decode-leg-is-dispatched-concurrently-with-prefill-chargin
tags:
- llm-d
- kv-cache-routing
- pd-disaggregation
- kv-transfer-bug
- llmd-kv-cache-routing-c665d5
created: '2026-09-14T11:53:21.127508Z'
updated: '2026-09-15T04:55:24.028277Z'
source: https://github.com/llm-d/llm-d-router/issues/2438
source_domain: github.com
fetched_at: '2026-09-14T11:53:21.107181Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Closed llm-d-router GitHub issue #2438 (opened Aug 18 2026, closed Sep 5
  2026 via PR #2451) documenting a KV-transfer race in the PD Multi Tier sidecar path
  (P2P/offloading connector, distinct from the default sequential nixlv2 path). handleP2P
  dispatched prefill and decode legs concurrently, so the decoder''s FetchMsg could
  reach the prefiller before it had stored KV for that kv_request_id; the decoder
  then burned vLLM''s hard-coded 30s _LOAD_TIMEOUT_S waiting, and under kv_load_failure_policy=recompute
  silently fell back to recomputing the prompt locally -- defeating the point of disaggregation.
  Measured on 8 prefill + 8 decode gpt-oss-120b (TP=1, vLLM v0.27.1, 16x H200): fetch_hit_immediate=0
  in every sample; concurrent-dispatch supply delay mean 15.83s (max 29.92s, effectively
  clipped by the 30s deadline) versus sequential-dispatch mean 0.002s (max 0.234s);
  consumer load latency mean fell from 14.387s to 0.291s after the fix. 6,083 load
  timeouts logged against 10,800 requests before the fix. Fix: sequence the legs (await
  prefill completion, then dispatch decode, propagating prefill errors verbatim) to
  match the NIXL connector''s ordering. Notes a related unresolved upstream vLLM defect
  (vllm-project/vllm#52808) where a producer already holding requested blocks supplies
  nothing, causing timeouts independent of dispatch ordering.'
utility_score: 12.0
---

[Skip to content](https://github.com/llm-d/llm-d-router/issues/2438#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/2438) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/2438) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/2438) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/issues/2438).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  336 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


#  sidecar: PD Multi Tier decode leg is dispatched concurrently with prefill, charging producer latency to the connector's load deadline
Copy link
Copy link
Closed
Closed
[sidecar: PD Multi Tier decode leg is dispatched concurrently with prefill, charging producer latency to the connector's load deadline](https://github.com/llm-d/llm-d-router/issues/2438#top)#2438
Copy link
## Description
opened [on Aug 18, 2026](https://github.com/llm-d/llm-d-router/issues/2438#issue-5185192314)
Last edited by nilig
Collaborator
Issue body actions
### What happened
`handleP2P` dispatches the prefill and decode legs **concurrently**. For the PD Multi Tier path this means the decoder's `FetchMsg` reaches the prefiller before the prefiller has stored any KV for that `kv_request_id`. The prefiller parks the demand awaiting supply, and the decoder spends its fixed `_LOAD_TIMEOUT_S` (30 s, hard-coded in vLLM's `kv_offload/tiering/p2p/session/client.py`) waiting for KV that has not been produced yet. On expiry it aborts the load and, under `kv_load_failure_policy: recompute`, recomputes the prompt locally — the work disaggregation exists to avoid.
The current comment on `handleP2P` states the ordering is safe because the connector parks blocks stored before the fetch binds the session. Parking is indeed safe for correctness, but it silently charges the producer's queue and compute time against the consumer's load deadline.
### Why this differs from the NIXL path
The default `nixlv2` path is sequential — it forwards to the prefiller and waits for it to return before dispatching decode. Concurrent dispatch there is an opt-in gated on `MoRIIOParallelDispatch && MoRIIOWriteMode`, and even then it only defers _committing_ decode's response. The PD Multi Tier path is the outlier.
### Evidence
Measured on 8 prefill + 8 decode (gpt-oss-120b, TP=1, vLLM v0.27.1, 16x H200), instrumenting the vLLM Multi Tier session:
  * `fetch_hit_immediate = 0` in every sample — the decoder's fetch never found blocks already staged.
  * Supply delay (fetch parked -> matched by a store): mean **15.83 s** , max **29.92 s** , i.e. truncated by the 30 s deadline rather than bounded by transfer cost. The transfers themselves run at ~45 GiB/s and account for ~14 s of work across a 26-minute run.
  * 6,083 load timeouts logged against 10,800 requests.


With the legs sequenced (prefill awaited, then decode dispatched):  
| concurrent  | sequential  |  
| --- | --- |  
| supply delay mean  | 15.830 s  | **0.002 s**  |  
| supply delay max  | 29.920 s  | **0.234 s**  |  
| consumer load latency mean  | 14.387 s  | **0.291 s**  |  
### Proposed fix
Run the prefill leg to completion, then dispatch decode, returning the prefill error verbatim if it fails (decode is then never dispatched, so it cannot fetch KV that was never stored). This matches the NIXL connector's ordering.
A change is prepared on `fix/p2p-sequential-dispatch` (`pkg/sidecar/proxy/connector_p2p.go`, +57/-54); `make test-unit-sidecar` passes. I will open the PR against this issue.
### Note
This does not fix everything. There is a separate upstream vLLM defect where a producer that already holds the requested blocks supplies nothing at all, so the consumer still times out regardless of ordering — filed as [vllm-project/vllm#52808](https://github.com/vllm-project/vllm/issues/52808). The sequencing change above is independently correct and removes the ordering-induced share of the timeouts.
## Activity
[nilig](https://github.com/nilig)
changed the title ~~[-] sidecar: P2P decode leg is dispatched concurrently with prefill, charging producer latency to the connector's load deadline[/-]~~ [+]sidecar: PD Multi Tier decode leg is dispatched concurrently with prefill, charging producer latency to the connector's load deadline[/+] [on Aug 18, 2026](https://github.com/llm-d/llm-d-router/issues/2438#event-29639912591)
[vMaroon](https://github.com/vMaroon)
closed this as [completed](https://github.com/llm-d/llm-d-router/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)in [#2451](https://github.com/llm-d/llm-d-router/pull/2451)[on Sep 5, 2026](https://github.com/llm-d/llm-d-router/issues/2438#event-30607461334)
[Sign up for free](https://github.com/signup?return_to=https://github.com/llm-d/llm-d-router/issues/2438)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/llm-d/llm-d-router/issues/2438)
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
  * [ fix(sidecar): await prefill before dispatching PD Multi Tier decode legllm-d/llm-d-router ](https://github.com/llm-d/llm-d-router/pull/2451)


### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
