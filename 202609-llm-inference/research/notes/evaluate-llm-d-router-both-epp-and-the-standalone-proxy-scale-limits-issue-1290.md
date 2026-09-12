---
title: 'Evaluate llm-d Router, both EPP and the standalone proxy, scale limits · Issue
  #1290 · llm-d/llm-d-router'
id: evaluate-llm-d-router-both-epp-and-the-standalone-proxy-scale-limits-issue-1290
tags:
- llmd-router-nonk8s-backends-54b774
- epp-scale-benchmark
- active-active
- audited-absence
created: '2026-09-11T21:45:05.475695Z'
updated: '2026-09-12T03:37:37.789513Z'
source: https://github.com/llm-d/llm-d-router/issues/1290
source_domain: github.com
fetched_at: '2026-09-11T21:45:05.442181Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Scale/benchmark evaluation issue (opened May 2026, closed Jul 21 2026),
  NOT a design doc — contains no statement that the precise prefix-cache index is
  partitioned across replicas and no mention of peer coordination/peer discovery being
  required for Active-Active correctness. It is the target of docs/operations.md''s
  ''For more technical details and context on EPP replica state sync and scaling limitations,
  see Issue #1290'' but the issue itself never uses the phrase ''replica state sync''
  nor discusses index partitioning; it is pure QPS/latency benchmarking. Key benchmark
  data (madhugoutham, EPP v0.8.0, llm-d-inference-sim v0.8.0, real vLLM v0.19.1 Qwen2.5-7B):
  Active-Active CPU-simulator scaling is near-linear to ~1,650 QPS at 4 replicas (1r
  ~478, 2r ~959, 3r ~1,308, 4r ~1,650) but collapses at 2,000 target QPS (only 1,642
  achieved, 4,767 failures, TTFT P50 4,565ms) -- a capacity ceiling, not a correctness
  failure. Critically, the ''Precise prefix cache active-active scaling'' table with
  real GPU vLLM backends shows failures at 500 QPS DECREASING as replicas increase:
  1r=broken/3,148 failures, 2r=57, 3r=31, 4r=18, with TTFT P50 dropping from 165ms(1r)
  to 30ms(4r) -- empirical evidence that precise prefix-cache routing scales well
  and gets BETTER (not worse) with more Active-Active replicas, which supports rather
  than overturns the draft''s claim. Separately documents a ''Leader election trap'':
  the standalone Helm chart auto-injects --ha-enable-leader-election when replicas>1,
  silently forcing active-passive behavior unless explicitly disabled. Session-affinity-filter
  (#815) was tested only with synthetic INDEPENDENT requests (no x-session-token echoing)
  -- the issue explicitly flags that validating session-pinning correctness across
  active-active replicas ''requires trace replay with session header echoing, which
  is a separate evaluation'' that was never done in this issue. No mention anywhere
  of LRU/no-hit-lru scorer state, predicted-latency training state, or saturation-view
  state differing per replica beyond what docs/operations.md itself already states
  (flow-control/saturation state is per-replica, not shared).'
---

[Skip to content](https://github.com/llm-d/llm-d-router/issues/1290#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/1290) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/1290) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/1290) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/issues/1290).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


#  Evaluate llm-d Router, both EPP and the standalone proxy, scale limits
Copy link
Copy link
Closed
Listed in
Closed
[Evaluate llm-d Router, both EPP and the standalone proxy, scale limits](https://github.com/llm-d/llm-d-router/issues/1290#top)#1290
Listed in
Copy link
Assignees
Labels
[triage/acceptedIndicates an issue or PR is ready to be actively worked on.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22triage%2Faccepted%22)Indicates an issue or PR is ready to be actively worked on.
Milestone
## Description
opened [on May 24, 2026](https://github.com/llm-d/llm-d-router/issues/1290#issue-4511198581)
Collaborator
Issue body actions
  * Understanding QPS and concurrency limits for EPP in active-passive mode for: 
    * Various configurations:
    * With prefix cache aware routing vs session affinity
    * With metrics probing vs active requests/tokens tracking
    * Different machine types: common medium and large CPU machine types on cloud
  * Understanding QPS and concurrency limits for EPP in active-active mode for: 
    * Various configurations: with/without precise prefix cache aware routing
    * Replication level, 2 vs 3 vs 4 on medium and large CPU machine types on cloud


## Activity
[ahg-g](https://github.com/ahg-g)
added 
[needs-triageIndicates an issue or PR lacks a triage label and requires one.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a triage label and requires one.
[on May 24, 2026](https://github.com/llm-d/llm-d-router/issues/1290#event-25896909308)
[github-project-automation](https://github.com/apps/github-project-automation)
added this to [on May 24, 2026](https://github.com/llm-d/llm-d-router/issues/1290#event-25896910186)
[ahg-g](https://github.com/ahg-g)
added 
[triage/acceptedIndicates an issue or PR is ready to be actively worked on.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22triage%2Faccepted%22)Indicates an issue or PR is ready to be actively worked on.
and removed 
[needs-triageIndicates an issue or PR lacks a triage label and requires one.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a triage label and requires one.
[on May 24, 2026](https://github.com/llm-d/llm-d-router/issues/1290#event-25896909867)
[mwaykole](https://github.com/mwaykole)
added a commit that references this issue [on May 25, 2026](https://github.com/llm-d/llm-d-router/issues/1290#event-25928173945)
[Promote plugin v2 config to be the default ([llm-d#1290](https://github.com/llm-d/llm-d-router/issues/1290))](https://github.com/mwaykole/llm-d-inference-scheduler/commit/b3a914685b6381de299e3b241c4e6a6d40e511cb)
[liu-cong](https://github.com/liu-cong)
added this to the [v0.9](https://github.com/llm-d/llm-d-router/milestone/7) milestone [on Jun 1, 2026](https://github.com/llm-d/llm-d-router/issues/1290#event-26199257941)
[ahg-g](https://github.com/ahg-g)
assigned [on Jun 1, 2026](https://github.com/llm-d/llm-d-router/issues/1290#event-26200127889)
### madhugoutham commented on Jun 1, 2026 
Last edited by madhugoutham
Member
More actions
[@ahg-g](https://github.com/ahg-g) Planning to pick this up. Session-affinity-filter ([#815](https://github.com/llm-d/llm-d-router/issues/815)) hasn't landed yet so initial results will cover load-aware vs prefix-cache across active-passive/active-active with automated benchmark scripts. Session affinity becomes a config file and re-run once [#815](https://github.com/llm-d/llm-d-router/issues/815) ships. Does this approach work? Also noticed [@jtechapps](https://github.com/jtechapps) was recently assigned, happy to step back if they've started.
### jtechapps commented on Jun 1, 2026 
Contributor
More actions
[@madhugoutham](https://github.com/madhugoutham) I haven't started this yet. I have only looked into prefix token size on EPP performance [#1217](https://github.com/llm-d/llm-d-router/issues/1217)
### madhugoutham commented on Jun 1, 2026 
Member
More actions
[@jtechapps](https://github.com/jtechapps) Thanks for the heads up. I'll take this one.
### madhugoutham commented on Jun 1, 2026 
Member
More actions
/assign
[github-actions](https://github.com/apps/github-actions)
assigned [on Jun 1, 2026](https://github.com/llm-d/llm-d-router/issues/1290#event-26214088691)
### madhugoutham commented on Jun 5, 2026 
Member
More actions
## EPP Scale Evaluation - Results
**Environment:**
  * EPP: `llm-d-inference-scheduler:v0.8.0`, standalone chart v1.5.0
  * Simulators: `llm-d-inference-sim:v0.8.0` (3 replicas, CPU-only)
  * GPU: `vllm-openai:v0.19.1`, Qwen/Qwen2.5-7B-Instruct (3x1 GPU)
  * Load gen: inference-perf v0.4.0 (via llm-d-benchmark image v0.6.0), synthetic (~ 125 input / ~30 output tokens)
  * Cluster: CoreWeave K8s, bare-metal nodes


### Active-Passive: Routing Strategy Comparison  
| Config  | Ceiling QPS  | TTFT P50 @ 200 QPS  | TTFT P99 @ 200 QPS  |  
| --- | --- | --- | --- |  
| Default (load-aware)  | ~478  | 4.2ms  | 12.3ms  |  
| Optimized baseline  | ~485  | 4.2ms  | 15.4ms  |  
| Active-request scorer  | ~461  | 4.3ms  | 15.3ms  |  
Routing strategy does not affect the throughput ceiling. Bottleneck is in EPP core processing (ext_proc gRPC + request parsing), not scorer plugins.
### CPU Sizing (full matrix)  
| CPU  | 1 Replica  | 2 Replicas  | 3 Replicas  | 4 Replicas  |  
| --- | --- | --- | --- | --- |  
| No limit  | ~478  | ~959  | ~1,308  | ~1,650  |  
| 4 CPU (medium)  | ~482  | ~948  | ~1,293  | ~1,653  |  
| 8 CPU (large)  | -  | ~953  | ~1,287  | ~1,647  |  
CPU has no measurable impact at any replica count. Scale replicas, not CPU.
> **Note:** CPU sizing tested with K8s resource requests (4 and 8 CPU) on bare-metal nodes to approximate medium and large cloud VM types.
### Active-Active: Scaling Curve  
| Replicas  | Ceiling QPS  | Scaling Factor  |  
| --- | --- | --- |  
| 1  | ~478  | 1.0x  |  
| 2  | ~959  | 2.0x  |  
| 3  | ~1,308  | 2.7x  |  
| 4  | ~1,650  | 3.5x  |  
Near-linear scaling. Each replica adds ~400-480 QPS.
**_Click_** to view per-stage breakdown (4 replicas)  
| Target QPS  | Achieved QPS  | Failures  | TTFT P50  | TTFT P99  |  
| --- | --- | --- | --- | --- |  
| 200  | 200  | 0  | 3.6ms  | 7.1ms  |  
| 500  | 500  | 0  | 3.7ms  | 9.9ms  |  
| 1,000  | 1,000  | 0  | 5.2ms  | 32.8ms  |  
| 2,000  | 1,642  | 4,767  | 4,565ms  | 11,535ms  |  
| 5,000  | 1,650  | 0*  | 5,111ms  | 13,815ms  |  
*Requests queued, no failures but high latency.
### GPU: Real vLLM (Qwen 7B, 3 pods x 1 GPU)
**Load-aware vs precise prefix cache:**  
| Config  | Ceiling QPS  | TTFT P50 @ 200 QPS  |  
| --- | --- | --- |  
| Load-aware, 1r  | ~436  | 172ms  |  
| Precise prefix cache, 1r  | ~429  | 165ms  |  
**Precise prefix cache active-active scaling:**  
| Replicas  | TTFT P50 @ 200 QPS  | TTFT P50 @ 500 QPS  | Failures @ 500 QPS  |  
| --- | --- | --- | --- |  
| 1  | 165ms  | broken  | 3,148  |  
| 2  | 59ms  | 295ms  | 57  |  
| 3  | 41ms  | 139ms  | 31  |  
| 4  | 30ms  | 96ms  | 18  |  
EPP routing overhead (4ms) is less than 3% of request latency with real models. The ~500 QPS ceiling is the model server limit (3 vLLM pods), not EPP. More EPP replicas reduce TTFT and failures at the same QPS.
> **Note:** These are standalone mode results.
### Findings
  1. **Leader election trap.** The standalone chart auto-enables `--ha-enable-leader-election` when replicas > 1. This makes it active-passive (only leader processes traffic). Deploying 4 replicas without disabling it gives 1-replica throughput. Fix: install with 1 replica then `kubectl scale`, or set `--ha-enable-leader-election=false`.
  2. **Cold-start penalty.** Freshly scaled replicas need 2-3 min warm-up before full throughput.
  3. **Scale replicas, not CPU.** Each replica adds ~400-480 QPS. CPU does not move the needle.


### Not yet tested
  * **Session affinity:** Blocked on [A session-affinity-filter plugin #815](https://github.com/llm-d/llm-d-router/issues/815). Config ready to re-run when it ships.
  * **Cloud VM types:** CPU sizing approximated with K8s resource limits on bare-metal. Results can be validated on actual cloud VMs (GKE/EKS).


[liu-cong](https://github.com/liu-cong)
moved this to In progress in [on Jun 8, 2026](https://github.com/llm-d/llm-d-router/issues/1290)
### madhugoutham commented on Jun 15, 2026 
Member
More actions
## Complete Routing Strategy Comparison (1r and 4r)
Session affinity filter tested with synthetic independent requests (no session token echoing). Full matrix now covers all 4 routing strategies at both 1 and 4 replicas.
**Environment:** EPP dev image (includes session-affinity-filter), CPU simulators (3 replicas), inference-perf synthetic load, CoreWeave bare-metal.  
| Config  | 1r Ceiling  | 4r Ceiling  | 4r Failures  | Scaling  |  
| --- | --- | --- | --- | --- |  
| Default (load-aware)  | ~478  | ~1,650  | 4,767  | 3.5x  |  
| Optimized baseline  | ~485  | ~1,408  | 0  | 2.9x  |  
| Active-request scorer  | ~461  | ~1,432  | 4,375  | 3.1x  |  
| Session affinity (filter)  | ~469  | ~1,414  | 0  | 3.0x  |  
All strategies within 5% at 1 replica. Bottleneck is ext_proc gRPC overhead, not plugin logic. At 4 replicas, default pushes ~15% more throughput but at the cost of routing quality (4,767 failures vs 0 for optimized baseline).
Failures come from the model server rejecting requests when overloaded, not from EPP itself. Zero failures means EPP distributed load well enough that no pod overflowed. Higher throughput does not mean better routing.
**Production guidance:** 1 EPP replica per ~450 QPS expected load. Scale replicas, not CPU. For production workloads that value reliability, use optimized baseline or session affinity (zero failures, add 1-2 extra replicas to compensate for the ~15% lower ceiling). EPP pods are CPU-only so the cost of extra replicas is low compared to GPU model server pods.
**Session affinity note:** Tested with independent synthetic requests. The session-affinity-filter was present in the pipeline but never triggered (no x-session-token header echoing). This confirms session affinity is safe to enable without throughput penalty. Session pinning latency benefit (KV cache reuse on repeat requests) requires trace replay with session header echoing, which is a separate evaluation.
[liu-cong](https://github.com/liu-cong)
modified the milestones: [v0.9](https://github.com/llm-d/llm-d-router/milestone/7), [v0.10](https://github.com/llm-d/llm-d-router/milestone/8) [on Jun 15, 2026](https://github.com/llm-d/llm-d-router/issues/1290#event-26780770200)
[ahg-g](https://github.com/ahg-g)
closed this as [completed](https://github.com/llm-d/llm-d-router/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)[on Jul 21, 2026](https://github.com/llm-d/llm-d-router/issues/1290#event-28280885243)
[Sign up for free](https://github.com/signup?return_to=https://github.com/llm-d/llm-d-router/issues/1290)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/llm-d/llm-d-router/issues/1290)
## Metadata
## Metadata
### Assignees
  * [jtechapps](https://github.com/jtechapps)
  * [madhugoutham](https://github.com/madhugoutham)


### Labels
[triage/acceptedIndicates an issue or PR is ready to be actively worked on.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22triage%2Faccepted%22)Indicates an issue or PR is ready to be actively worked on.
No type
### Projects
  * [llm-d-router Closed](https://github.com/orgs/llm-d/projects/15)
Status
In progress
Show more project fields


Show 1 closed project
### Milestone
  * [ v0.10 llm-d v0.9.0 releaseClosed Aug 27, 2026, 100% complete ](https://github.com/llm-d/llm-d-router/milestone/8)


### Relationships
None yet
### Development
No branches or pull requests
### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
