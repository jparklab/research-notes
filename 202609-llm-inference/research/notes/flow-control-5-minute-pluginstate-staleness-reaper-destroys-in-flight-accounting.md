---
title: 'Flow Control: 5-minute PluginState staleness reaper destroys in-flight accounting
  for live requests, collapsing the saturation gate under sustained load · Issue #2295
  · llm-d/llm-d-router'
id: flow-control-5-minute-pluginstate-staleness-reaper-destroys-in-flight-accounting
tags:
- llmd-kv-cache-routing-c665d5
- hotspot-mitigation
- llm-d
- flow-control
- independent-adversarial-evidence
- maintainer-issue
created: '2026-09-14T13:04:05.201186Z'
updated: '2026-09-15T04:55:24.146075Z'
source: https://github.com/llm-d/llm-d-router/issues/2295
source_domain: github.com
fetched_at: '2026-09-14T13:04:05.180442Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub issue #2295 on llm-d/llm-d-router (opened Aug 6 2026, contributor-authored
  incident report, closed as bug fix): a detailed, instrumented reproduction of catastrophic
  prefix-cache-affinity-driven hotspotting under sustained load in llm-d''s Flow Control
  system. Setup: 8-replica vLLM pool (Qwen3-Coder-480B-A35B-Instruct-FP8, TP=4 on
  GB200), 1 EPP replica, concurrency-detector saturation gate (maxConcurrency=12/endpoint,
  96 aggregate), agentic multi-turn traces up to 262k-token contexts, session arrival
  1qps->2qps burst->1qps->0.1qps->1qps over ~2.5 hours. Root cause: a hardcoded 5-minute
  PluginState ''staleness reaper'' evicts in-flight-accounting entries for requests
  still queued past 5 minutes, decrementing counters while the request still occupies
  the server; the saturation gate then sees phantom free capacity and dispatches more
  work, deepening queues further -- a self-accelerating loop that does not recover
  when load drops. Documented numbers: engine occupancy (running+waiting across 8
  pods) rose from a healthy steady-state of 23-32 to ~1000 within 55 minutes of a
  10-minute 2x burst and ~1700 by run end; EPP''s own in-flight count read only 40-60%
  of reality, decaying ~90/minute with no completions; 24% of 18,699 requests (4,485)
  ended with TTFT above 300s; a 15-minute near-idle period (0.1 qps) failed to drain
  the backlog (occupancy only fell from ~1000 to ~700). Critically, the excess load
  was NOT spread evenly: prefix-cache-affinity concentrated it onto specific pods
  because every session turn''s full conversation prefix pulls it back to the pod
  holding those KV blocks -- per-pod waiting depth data shows one pod''s waiting queue
  growing from 2 to 64 to 159 to 201 while others emptied to near-zero, because the
  affinity scorer''s load signal was itself corrupted by the same reaping bug (a reaped
  hot pod ''looks lightly loaded,'' so both the affinity trade-off and the load scorer
  keep selecting it). The author states this is a real mechanism, not a synthetic
  corner case: ''in 51 minutes of healthy-plus-burst operation, 0 of 8545 requests
  waited [5 minutes]... The 5-minute waits were then manufactured by the gate''s own
  burst-overshoot behavior.'' This is the single most detailed, numerically load-bearing
  evidence in the corpus of llm-d''s cache-affinity routing catastrophically concentrating
  load onto specific replicas under sustained/bursty conditions, though the trigger
  is a specific flow-control accounting bug (fixed in this issue) rather than a fundamental
  property of affinity routing itself.'
---

*Suggested by [[issues-llm-dllm-d-router-github-4]] — surfaced by GIE gap-fill issue search for hotspot/imbalance/skew*

[Skip to content](https://github.com/llm-d/llm-d-router/issues/2295#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/2295) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/2295) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/2295) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/issues/2295).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 367 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  336 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


#  Flow Control: 5-minute PluginState staleness reaper destroys in-flight accounting for live requests, collapsing the saturation gate under sustained load
Copy link
Copy link
Closed
Closed
[Flow Control: 5-minute PluginState staleness reaper destroys in-flight accounting for live requests, collapsing the saturation gate under sustained load](https://github.com/llm-d/llm-d-router/issues/2295#top)#2295
Copy link
Labels
[kind/bugCategorizes issue or PR as related to a bug.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22kind%2Fbug%22)Categorizes issue or PR as related to a bug.[needs-triageIndicates an issue or PR lacks a triage label and requires one.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a triage label and requires one.
## Description
opened [on Aug 6, 2026](https://github.com/llm-d/llm-d-router/issues/2295#issue-5076873115)
Contributor
Issue body actions
## Summary
When Flow Control is enabled with the `concurrency-detector` saturation gate, the in-flight accounting that drives the dispatch gate is corrupted for any request that produces no response chunk for 5 minutes: the `PluginState` staleness reaper (hardcoded `stalenessThreshold = 5min`) evicts the request's `inflight-load-producer` entry and decrements the in-flight counters while the request still occupies the model server. The gate then sees phantom capacity and dispatches more work, which deepens engine queues, which pushes more requests past the 5-minute mark. The loop is self-accelerating and does not recover when load drops.
We measured this end to end on an 8-replica vLLM pool with an aggregate dispatch limit of 96 requests (`maxConcurrency: 12` x 8 endpoints). A single 10-minute 2x traffic burst was enough to trigger the loop; engine occupancy (running + waiting) went from a healthy steady state of ~28 to ~1000 within 55 minutes of the burst and ~1700 by the end of the run, while the EPP's own in-flight count read 40-60% of reality and decayed ~90/minute without completions. 24% of all requests ended with TTFT above 300 s; a 15-minute near-idle period (0.1 qps) failed to drain the backlog.
A predictable objection is that requests waiting 5 minutes without a token are unrealistic outside synthetic tests. Our data supports the premise and refutes the conclusion: in 51 minutes of healthy-plus-burst operation, 0 of 8545 requests waited that long. The 5-minute waits were then manufactured by the gate's own burst-overshoot behavior within 15 minutes, at nominal load, while the accounting was still accurate (verified by live comparison). The chronological reconstruction below shows each step with data; the code walkthrough that follows maps each step to the responsible code path.
## Environment
  * llm-d-router: `main` @ `cc715b1b8ef62028950ddae38dfe75b292f2d58a` (image built 2026-08-03)
  * Model pool: Qwen3-Coder-480B-A35B-Instruct-FP8, vLLM, TP=4 on GB200, 8 replicas, 1 EPP replica
  * Workload: agentic multi-turn traces (streaming chat completions, contexts up to 262k tokens; each completed turn immediately issues the session's next turn, so turn throughput is ~3/s at 1 qps session arrival), client per-request timeout 1200 s
  * Session arrival schedule (t = seconds since benchmark start): 1.0 qps for 0-2700, 2.0 qps for 2700-3300, 1.0 qps for 3300-6000, 0.1 qps for 6000-6900, 1.0 qps for 6900-9600
  * EPP config (relevant part):



```
featureGates:
- flowControl
plugins:
- type: concurrency-detector
  parameters:
    maxConcurrency: 12   # per endpoint; 8 endpoints -> 96 aggregate
    concurrencyMode: requests
    headroom: 0.0
# plus inflight-load-producer, token-load-scorer,
# prefix-cache-affinity-filter, precise-prefix-cache-producer, token-producer
flowControl:
  maxRequests: 
  maxBytes: 
  defaultRequestTTL: "1200s"
  saturationDetector:
    pluginRef: concurrency-detector
```

Data sources: per-request client records (18699 requests) and per-pod vLLM metrics scraped every ~5 s for the whole run, plus two live EPP metric snapshots taken during the run.
## Measured timeline: how a healthy system reached collapse
Per-phase request statistics (client-side):  
| Phase  | n  | e2e p50/p95/p99 (s)  | TTFT p50/p95/p99 (s)  | requests with TTFT > 300 s  |  
| --- | --- | --- | --- | --- |  
| 1 qps steady (0-2700)  | 6080  | 5.8 / 46 / 93  | 0.8 / 3.0 / 8.8  |  
| 2 qps burst (2700-3300)  | 2465  | 17 / 120 / 243  | 1.3 / 58 / 71  |  
| 1 qps (3300-6000)  | 4995  | 266 / 1090 / 1194  | 204 / 1022 / 1185  | 1678  |  
| 0.1 qps (6000-6900)  | 986  | 338 / 860 / 1095  | 284 / 736 / 930  | 417  |  
| 1 qps (6900-9600)  | 4173  | 435 / 1065 / 1167  | 377 / 996 / 1112  | 2390  |  
Pool occupancy (sum of vLLM running + waiting across the 8 replicas, from scraped engine metrics):  
| t (s)  | 0-2700  | 3120  | 3300  | 3960  | 4560  | 4920  | 6000  | 6900  | 9600  |  
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |  
| running + waiting  | 23-32  | 103  | 128  | 310  | 406  | 370  | ~1000  | ~700  | ~1700  |  
### Stage 0 (t < 2700): healthy steady state
45 minutes at 1 qps: 23-32 requests in-system, TTFT p99 of 8.8 s, zero requests anywhere near a 5-minute wait. This is the regime a skeptic has in mind when arguing 5-minute waits cannot happen, and the data agrees: they do not happen here.
### Stage 1 (2700-3300): burst overshoots the gate; still zero 5-minute waits
The gate should hold aggregate in-flight at 96. Occupancy crosses 103 by t=3120 and reaches 128 at burst end. The gate is open-loop: its counter increments in `PreRequest`, which fires only after the dispatched request finishes the scheduling pipeline (including a remote `/tokenize` call for prompts up to 262k tokens, hundreds of ms to seconds each), while dispatch decisions are made in microsecond cycles against the not-yet-incremented count. Engine waiting depth at burst end is only ~10 per the pool; all 2465 burst-phase requests still complete with TTFT far below 300 s.
### Stage 2 (3300-4200): the ratchet - occupancy triples at nominal load, with accounting still accurate
This is the step that manufactures the 5-minute condition. After the burst ended and arrivals returned to 1 qps (a rate this pool had sustained for 45 minutes), occupancy climbed 128 -> 310 in 11 minutes. No reaping is involved in this stage, and the accounting is demonstrably accurate when quiescent: a live snapshot at t~3890 read EPP counted in-flight 148 vs 140 actually in the engines (the difference is responses in transit). Still zero requests over 300 s.
**How more than 96 requests get admitted although the gate hard-stops at 96.** The gate bounds the _counted_ in-flight at dispatch time, but the true in-flight is:

```
true_inflight = counted + in_pipeline

```

where `in_pipeline` is every request already dispatched whose `PreRequest` increment has not landed yet. The increment fires only after the dispatched request finishes the scheduling pipeline - including `token-producer`'s remote `/tokenize` HTTP call against the serving pods for prompts up to 262k tokens - a latency L of hundreds of milliseconds to seconds per request, and longer when the pods are loaded. The dispatch loop meanwhile re-reads the counter every ~14 us cycle. The gate cannot see `in_pipeline`, and it has no mechanism to recall work it already released.
The ratchet cycle, per completion wave:
  1. Turn completions arrive in bursts of 100-200/minute (turns batched together finish together); each decrements the counter immediately at `EndOfStream`. Counted falls from ~148 through 96 and below.
  2. The instant counted dips under 96, the gate opens and drains the EPP queue (~211 deep at this point) at microsecond cadence. Every released request is invisible for the next L seconds, so the gate keeps reading a low count and keeps dispatching. The queue does not run dry: each completed turn immediately mints the session's next turn into it.
  3. Dispatch stops only when the late increments from the earliest releases push counted back over 96. By then the gate has released roughly (completions in the wave) + (dispatch rate x L) requests - the first part legitimate replacement, the second part pure overshoot, bounded only by queue depth.
  4. The in-pipeline increments then drain into the counter; counted settles at 96 + overshoot (which is why the t~3890 snapshot could read an _accurate_ 148 with the gate closed). The overshoot is now real running/waiting work in the engines and stays until completed. The next wave repeats from this higher floor.


The invariant actually enforced is therefore not "in-flight <= 96" but "no new dispatch while _counted_ >= 96", which permits growth of one dispatch-burst per completion wave: 128 -> 160 -> 310 -> 406. The cleanest specimen is t=3900 -> 3960: ~150 completions in that minute, occupancy 160 -> 310 - about 300 dispatches went out in wave-open windows, ~150 replacing completions plus ~150 net overshoot, all before a single accounting entry was reaped.
### Stage 2b (3300-4560): prefix-cache affinity concentrates the backlog onto one pod
The Stage 2 excess is not spread evenly. Per-pod waiting depth (running+waiting per pod, from engine metrics):  
| t (s)  | per-pod occupancy (run+wait)  | max single-pod waiting  |  
| --- | --- | --- |  
| 3360  | 16+2, 19+5, 21+3, 13+3, 10+4, 12+4, 23+4, 14+2  | 5  |  
| 3960  | 16+11, **19+64** , 13+16, 22+13, 13+41, 18+6, 13+18, 14+13  | 64  |  
| 4560  | 14+20, **16+159** , 20+10, 12+12, 17+13, 15+39, 20+12, 16+13  | 159  |  
| 4920  | 3+0, **16+124** , 3+0, **16+201** , 1+0, 3+0, 2+0, 1+0  | 201  |  
How the hotspot forms: every turn of a session carries the full conversation as its prompt prefix, and the pod that served the session's earlier turns holds those KV blocks. The prefix-cache affinity scheduling stage therefore steers each session's next turn back to its home pod - reusing tens of thousands of cached prefix tokens normally beats a cold prefill elsewhere, and under healthy load this is the correct and profitable decision. During a backlog it self-reinforces: the sessions homed on the backlogged pod keep appending their next turns to its queue (session turn arrival is closed-loop, but new sessions keep arriving at 1 qps and being distributed), and the affinity decision weighs prefill savings against the target pod's load using the same `InFlightLoad` data that (a) lags reality during Stage 2 drain bursts and (b) is outright falsified once Stage 3 reaping begins - a reaped hot pod looks lightly loaded, so both the affinity trade-off and the load scorer keep selecting it. By t=4920 two pods hold 124 and 201 waiting while six pods are nearly empty.
The hotspot matters for the timeline because per-request wait is queue-position x per-slot service time on that pod, not the pool average: a request at position ~60 on a pod draining ~10-15 turns/minute waits ~5 minutes. Pool-level occupancy of ~300 understates what requests on the hot pod experience.
### Stage 3 (t = 4207): first reaps - the loop closes
The first requests to exceed 300 s without a first token were admitted at t=3907-3915 (by a Stage 2 drain burst, into the hot pod's queue) and crossed the 5-minute mark at t=4207-4215: 54 of them inside a single one-minute window, matching the janitor's 1/minute sweep cadence. From that point the reaper deletes their accounting entries, the counters drop without completions, and the gate refills the "freed" capacity. In terms of the Stage 2 ratchet, the reaper changes the trigger of the counter dips: gate-opening windows no longer require genuine completions - janitor sweeps now delete 50-100 entries per minute for requests still stuck in the engines, so the gate keeps opening (with the same lag overshoot on every drain) even as real throughput collapses. The crossing rate and occupancy growth then track each other:  
| window (s)  | new TTFT>300s crossings  | pool occupancy  |  
| --- | --- | --- |  
| 4200-4500  | 65  | ~170-280  |  
| 4500-4800  | 110  | ~180-400  |  
| 4800-5100  | 309  | 155 -> 537  |  
| 5400-6000  | 673  | 540 -> ~1000  |  
Simultaneously, running collapses from ~130 to 21-44 while waiting explodes (KV pressure; cumulative preemptions climb from 5 to 16): each reaped slot is refilled with a new dispatch into pods that can no longer make progress. Turn throughput falls from ~200/minute to ~20/minute.
A second live snapshot late in the run (t~8440) quantifies the corruption directly: EPP counted 588 in-flight (saturation 6.125 x 96) versus 1029 actually in the engines, with the counted value decaying ~90/minute with no matching completions - the janitor sweep visible in real time. Cumulative EPP counters at that moment: 13643 enqueued, 13484 dispatched, 159 queued, zero rejections - nothing bypassed the flow-control queue; the gate itself kept reopening.
### Stage 4: absorbing state
Fifteen minutes at 0.1 qps (t=6000-6900, near-zero offered load) drained occupancy only from ~1000 to ~700, because reaped slots keep being refilled from the still-deep EPP queue and surviving sessions keep issuing turns. Resuming 1 qps drove occupancy to ~1700 by run end. Final tally: 4485 of 18699 requests (24%) exceeded 300 s TTFT, 1924 errors/cancellations, final-phase e2e p50 of 435 s against the healthy-phase 5.8 s.
## Mechanism (code walkthrough)
All references at commit `cc715b1b`.
  1. `concurrency-detector` does not count requests itself. It consumes `InFlightLoad` produced by `inflight-load-producer` (`pkg/epp/framework/plugins/flowcontrol/saturationdetector/concurrency/detector.go`, `Consumes()`; pool capacity is `maxConcurrency x numEndpoints`, `detector.go:146`).
  2. `inflight-load-producer` increments per-endpoint atomic counters in its `PreRequest` hook (`pkg/epp/framework/plugins/requestcontrol/dataproducer/inflightload/producer.go:376`) and stores a per-request entry in `PluginState`. The decrement fires when the entry is released: normally at `EndOfStream` via `PluginState.Delete` (`producer.go:446-507`), or via `OnEvicted` if the janitor reaps it (comment at `producer.go:150-152`: "whether triggered by explicit release at end-of-stream or by the janitor's TTL reaper"). The `PreRequest`-after-scheduling placement is what drives Stages 1-2: dispatch decisions read a count that trails the dispatches themselves by the scheduling-pipeline latency.
  3. Entry liveness between increment and release is maintained exclusively by `PluginState.Touch(requestID)`, called once per response body chunk (`producer.go`, the `EndOfStream == false` branch). A request that has not produced its first token has never been touched.
  4. `PluginState` hardcodes the liveness bound (`pkg/epp/framework/interface/plugin/plugin_state.go:31-37`):



```
stalenessThreshold = time.Minute * 5
cleanupInterval    = time.Minute
```

  1. A request sitting in the model server's waiting queue (or mid-prefill, or a non-streaming request mid-generation) produces zero chunks. Five minutes after `PreRequest`, the janitor evicts its entry; `OnEvicted` decrements the counters; the EPP permanently forgets the request while the engine still holds it. This is Stage 3.
  2. The dispatch loop re-reads saturation every cycle and hard-stops at `saturation >= 1.0` (`pkg/epp/flowcontrol/controller/internal/processor.go:378-397`). The logic is correct; its input is what fails. With reaped counters reading low, the gate reopens and drains the EPP queue into the already-backlogged engines, closing the loop.
  3. The same `InFlightLoad` data feeds `token-load-scorer` and the prefix-cache affinity cost model, so routing under corruption prefers exactly the most-backlogged endpoints (Stage 2b's second amplifier; observed end state in a follow-up run: 430 waiting on one pod, 0 on others).


## Why the "requests never wait 5 minutes in production" objection does not hold
  * It assumes the 5-minute waits are an input. They are an output: the measured run had zero such waits through 51 minutes of healthy and burst operation, and 54 of them fifteen minutes after a routine 10-minute 2x burst ended - produced by the gate's own overshoot ratchet (Stage 2) plus ordinary prefix-affinity queueing (Stage 2b), both at nominal load with accounting verified accurate. Any deployment that ever transiently exceeds drain capacity is one burst away from the same sequence.
  * Non-streaming requests never emit intermediate chunks, so `Touch` never fires for them: any non-streaming request whose total latency exceeds 5 minutes (6k-18k output tokens at 20-60 tok/s - routine for reasoning and code-generation workloads) is reaped mid-flight on a perfectly healthy system. Streaming requests are likewise exposed by any single inter-chunk gap over 5 minutes.
  * The failure mode inverts incident handling: an engine that stalls for more than 5 minutes has all its requests reaped, so the EPP concludes it is empty and (via the shared `InFlightLoad` data) steers new traffic toward it. Failing open onto a degraded endpoint is the opposite of the posture the project's own `utilization-detector` documentation argues for on missing data.


## Suggested fixes (any one of the first three prevents the collapse)
  1. Do not reap accounting entries of requests whose ext_proc stream is still alive. Tie entry lifetime to request-context cancellation / stream termination instead of wall-clock chunk gaps. The reaper then only covers genuinely leaked entries (its apparent purpose).
  2. Make `stalenessThreshold` configurable per consumer, and document that it must exceed the maximum expected time-in-system (TTFT tail plus full generation time), not typical time.
  3. Periodically reconcile the in-flight counters against scraped engine telemetry (`num_requests_running + num_requests_waiting` are already collected by the data layer); log and correct divergence. In our run the scraped metrics tracked reality correctly the entire time - the accurate signal existed in the EPP while the gate consumed the corrupted one.
  4. Independently: dispatch-time reservation for the counter increment (increment optimistically at dispatch, reconcile after scheduling). This closes the Stage 1-2 overshoot ratchet, which is what builds the backlog that feeds the reaper; it is worth fixing even with 1-3 in place, since it tripled occupancy at nominal load before any reaping occurred.


## Reproduction
Reproduced as described above on an 8x vLLM pool; the shape generalizes to any pool where per-turn service time is seconds-to-minutes and requests share cacheable prefixes:
  1. Enable `flowControl` with `concurrency-detector` (`concurrencyMode: requests`, `maxConcurrency = target / numEndpoints`, `headroom: 0`).
  2. Run a steady multi-turn streaming workload comfortably within capacity, then apply a 2x rate step for ~10 minutes, then return to the steady rate.
  3. Watch `flow_control_pool_saturation x capacity` versus `sum(vllm:num_requests_running + vllm:num_requests_waiting)`. Occupancy ratchets above the gate during and after the step (Stage 2); ~5 minutes after per-pod waiting queues form, the counted value starts decaying in 1/minute steps without completions (Stage 3) and occupancy grows without bound while the dispatched counter keeps rising.


Unit-level demonstration without a cluster: create an `inflight-load-producer` entry via `PreRequest`, send no response chunks for >5 minutes, observe `OnEvicted` decrement while the (simulated) request is still active, then observe the saturation gate admit new work.
## Activity
[yangligt2](https://github.com/yangligt2)
added 
[needs-triageIndicates an issue or PR lacks a triage label and requires one.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a triage label and requires one.
[kind/bugCategorizes issue or PR as related to a bug.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22kind%2Fbug%22)Categorizes issue or PR as related to a bug.
[on Aug 5, 2026](https://github.com/llm-d/llm-d-router/issues/2295#event-29043389982)
### LukeAVanDrie commented on Aug 5, 2026 
Contributor
More actions
/assign
[LukeAVanDrie](https://github.com/LukeAVanDrie)
added a commit that references this issue [on Aug 7, 2026](https://github.com/llm-d/llm-d-router/issues/2295#event-29134266673)
[Skip janitor reaping for requests bound to a live context](https://github.com/LukeAVanDrie/llm-d-router/commit/1c6db330f75c36abdfbaccad893bf262afdb4e2b)
[ahg-g](https://github.com/ahg-g)
added a commit that references this issue [on Aug 7, 2026](https://github.com/llm-d/llm-d-router/issues/2295#event-29134718549)
[Skip janitor reaping for requests bound to a live context ([#2313](https://github.com/llm-d/llm-d-router/pull/2313))](https://github.com/llm-d/llm-d-router/commit/443fa9e74444b9192fd289b48f671126f8b586aa)
[ahg-g](https://github.com/ahg-g)
closed this as [completed](https://github.com/llm-d/llm-d-router/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)in [#2313](https://github.com/llm-d/llm-d-router/pull/2313)[on Aug 7, 2026](https://github.com/llm-d/llm-d-router/issues/2295#event-29134718617)
[Sign up for free](https://github.com/signup?return_to=https://github.com/llm-d/llm-d-router/issues/2295)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/llm-d/llm-d-router/issues/2295)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
[kind/bugCategorizes issue or PR as related to a bug.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22kind%2Fbug%22)Categorizes issue or PR as related to a bug.[needs-triageIndicates an issue or PR lacks a triage label and requires one.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a triage label and requires one.
No type
### Projects
No projects
### Milestone
No milestone
### Relationships
None yet
### Development
  * [Skip janitor reaping for requests bound to a live contextllm-d/llm-d-router](https://github.com/llm-d/llm-d-router/pull/2313)[Releasev0.10.0Latest](https://github.com/llm-d/llm-d-router/releases/tag/v0.10.0)


### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
