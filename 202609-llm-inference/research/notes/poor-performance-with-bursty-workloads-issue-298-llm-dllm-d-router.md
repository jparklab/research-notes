---
title: 'Poor performance with bursty workloads · Issue #298 · llm-d/llm-d-router'
id: poor-performance-with-bursty-workloads-issue-298-llm-dllm-d-router
tags:
- llmd-kv-cache-routing-c665d5
- hotspot-mitigation
- llm-d
- load-balancing
- independent-adversarial-evidence
- maintainer-issue
created: '2026-09-14T13:03:58.656293Z'
updated: '2026-09-15T04:55:24.141831Z'
source: https://github.com/llm-d/llm-d-router/issues/298
source_domain: github.com
fetched_at: '2026-09-14T13:03:58.628541Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub issue #298 on llm-d/llm-d-router (opened Aug 18 2025 by Red Hat contributor
  dagrayvid, closed-not-planned Jan 2026, 17 comments): a maintainer''s own performance
  experiment found llm-d''s inference-scheduler underperforming plain Knative request-count-based
  load balancing. Setup: 8 replicas of Llama-3.3-70B-fp8 on 2xH100/replica (TP=2),
  prefix-caching/prefix-aware routing explicitly DISABLED (queue-scorer + kv-cache-scorer
  only, weight 1.0 each), guidellm workload at 1000 ISL/1000 OSL, concurrency=800
  -- a deliberately bursty, fixed-sequence-length, concurrency-based traffic pattern.
  Result: llm-d gave slightly lower throughput and ''significantly worse TTFT'' than
  Knative, with vllm:num_requests_waiting showing heavy queuing concentrated on a
  subset of replicas -- because metric refresh lag (50ms interval) meant a burst of
  near-simultaneous requests all landed on the same pod (0 KV utilization/queue depth
  at decision time) before the score updated. Root cause per maintainer liu-cong:
  ''metrics update delay for very bursty traffic ... this is a corner case ... only
  happens when you send a very bursty traffic with shared prefixes, which in practice
  is likely rare.'' Quantified fix comparison (KServe baseline vs. llm-d max-score-picker
  vs. llm-d weighted-random-picker): TTFT mean 1413ms (KServe) vs. 2083.4ms (llm-d
  top-score-picker, 47% WORSE than plain LB) vs. 367.2ms (weighted-random fix); TTFT
  P99 6941ms vs. 7749ms (worse) vs. 2061ms; output tok/s 17509 vs. 16861 (slightly
  worse) vs. 17601. This is a first-party, maintainer-documented instance of llm-d''s
  load-aware routing (with prefix-affinity off) actually losing to plain load balancing
  on a bursty, low-prefix-diversity workload. Follow-up experiments show the fix (weighted
  random sampling) trades off against multi-turn chat performance where prefix affinity
  matters: ''weighted-random-sampling significantly hurts performance compared to
  max-score for this use-case'' (output tok/s drops from 12608 to 8292 at matched
  scorer weights) unless the prefix-scorer weight is substantially increased to compensate.'
---

*Suggested by [[issues-llm-dllm-d-router-github-4]] — surfaced by GIE gap-fill issue search for hotspot/imbalance/skew*

[Skip to content](https://github.com/llm-d/llm-d-router/issues/298#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/298) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/298) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/298) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/issues/298).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 367 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  336 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


#  Poor performance with bursty workloads
Copy link
Copy link
Closed as not planned
Closed as not planned
[Poor performance with bursty workloads](https://github.com/llm-d/llm-d-router/issues/298#top)#298
Copy link
Labels
[kind/bugCategorizes issue or PR as related to a bug.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22kind%2Fbug%22)Categorizes issue or PR as related to a bug.[lifecycle/rotten](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22lifecycle%2Frotten%22)[needs-triageIndicates an issue or PR lacks a triage label and requires one.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a triage label and requires one.
## Description
opened [on Aug 18, 2025](https://github.com/llm-d/llm-d-router/issues/298#issue-3331766982)
Last edited by dagrayvid
Contributor
Issue body actions
**What happened** : While doing performance experiments comparing Knative requests-based load balancing with llm-d load-aware routing, I have found some cases where llm-d inference scheduler underperforms requests-based load balancing.
For example, with 8 replicas of Llama-3.3-70b-fp8 on 2xH100 per replica (TP=2), with prefix caching and prefix-aware routing disabled (queue-scorer and kv-cache-scorer only, each with weight 1.0), if we run [guidellm](https://github.com/vllm-project/guidellm) with 1000 ISL, 1000 OSL, in concurrency mode with concurrency=800, we get slightly lower throughput with llm-d:
and significantly worse TTFT 
vllm:num_requests_waiting metrics show quite a bit of queuing at some replicas throughout the test 
I believe this is because this workload is quite bursty, being concurrency based with fixed sequence lengths per request. Requests come in waves and when many requests arrive simultaneously they get routed to the same Pod.
Please note that this is a corner-case, picked because it is the best case scenario for knative. In cases where the sequence lengths are heterogeneous or prefix caching comes into play, llm-d significantly outperforms knative.
**What you expected to happen** : Ideally we would like to see on-par or better performance from llm-d-inference-scheduler intelligent routing.
**Anything else we need to know?** : Related issue: [#228](https://github.com/llm-d/llm-d-router/issues/228), but an alternative solution may be to introduce a random filter, so that we can introduce some randomness into the scheduler, e.g. pick 2 random endpoints and route to the better of two ([power of two choices](https://medium.com/the-intuition-project/load-balancing-the-intuition-behind-the-power-of-two-random-choices-6de2e139ac2f)).
**Environment** : llm-d 0.2.0 installed with llm-d-infra quickstart/examples, including [inference-scheduling](https://github.com/llm-d-incubation/llm-d-infra/tree/main/quickstart/examples/inference-scheduling) and [precise-prefix-cache-aware](https://github.com/llm-d-incubation/llm-d-infra/tree/main/quickstart/examples/precise-prefix-cache-aware).
I will continue experimentation to try to better understand the root-cause of the issue and test some potential improvements, but wanted to open this issue to get the discussion started!
## Activity
[dagrayvid](https://github.com/dagrayvid)
added 
[needs-triageIndicates an issue or PR lacks a triage label and requires one.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a triage label and requires one.
[kind/bugCategorizes issue or PR as related to a bug.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22kind%2Fbug%22)Categorizes issue or PR as related to a bug.
[on Aug 18, 2025](https://github.com/llm-d/llm-d-router/issues/298#event-19209956779)
### nirrozenbaum commented on Aug 18, 2025 
Contributor
More actions
I don’t think a random scorer is the solution. I think we need to replace the max score picker with probability based picker, where the probability of each pod to get selected is is based on its score. This is called [Weighted Random Sampling](https://link.springer.com/rwe/10.1007/978-0-387-30162-4_478).
👍React with 👍3Reacted by David Whyte-Gray, MARATRIX and Kay Yan
### Jooho commented on Aug 19, 2025 
Contributor
More actions
I created an issue for a new picker `Weighted Random Sampling` and sent a PR about it. Please review [the PR](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1412) [@nirrozenbaum](https://github.com/nirrozenbaum) [@dagrayvid](https://github.com/dagrayvid)
### liu-cong commented on Aug 20, 2025 
Member
More actions
The root cause of this is likely due to the metrics update delay for very bursty traffic. Imagine a fresh deployment, where initially all pods have 0 k-v cache utilization. When we send a burst of traffic with shared prefixes, all of them are routed to the same pod due to the prefix scorer, until 50ms (the metric refresh interval) later, this pod gets lower score due to high queue and k-v cache. As mentioned in the bug, this is a "corner case" - it only happens when you send a very bursty traffic with shared prefixes, which in practice is likely rare.
While the weighted random sampling can help mitigate this, I am also concerned that it may dilute the prefix affinity, so some careful benchmarking is needed.
I think a short term mitigation is to shorten the metric refresh interval. A better long term solution is to also consider inflight requests (a new scorer perhaps). [@vMaroon](https://github.com/vMaroon) mentioned an `active request` scorer being developed.
### Jooho commented on Aug 20, 2025 
Contributor
More actions
I agree with you but I feel GPU utilization is just as important as cache hit efficiency. Weighted random sampling can efficiently utilize GPUs while still maintaining a high probability of selecting the pod with the highest score. It doesn’t fully solve the root cause (metrics delay), but may serve as a reasonable short-term approach.
### nirrozenbaum commented on Aug 20, 2025 
Contributor
More actions
I think inflight or active request scorer is eventually another scorer. it may help scoring the pods but scoring is based on weights of all the scorers.
I think having a random weighted picker is orthogonal and generally should give better results than the max score picker, as selecting always by max score is not always ideal.
think for example about scorer that scores 10 pods: 8 pods get score 0, 1 gets score 0.5 and the last one gets score 0.499. does it mean we should always select the one with 0.5?
### liu-cong commented on Aug 20, 2025 
Member
More actions
> I agree with you but I feel GPU utilization is just as important as cache hit efficiency.
Just wanna clarify, as mentioned above, I think this hot spot issue only happens in an extreme case where traffic is **bursty, and with shared prefixes**. In most real world use cases, I don't believe this is the case. There will be many different prefixes which will be distributed on different servers, and overall achieving almost equal GPU utilization. If you see otherwise, please share, I'd like to learn more.
### liu-cong commented on Aug 20, 2025 
Member
More actions
> think for example about scorer that scores 10 pods: 8 pods get score 0, 1 gets score 0.5 and the last one gets score 0.499. does it mean we should always select the one with 0.5?
Agree some randomness helps if the scores are very close (e.g, practically there is probably no difference between a 0.5 vs 0.499 score). I tried something like bucketizing the scores and treat the same bucket as the same in the past. However, if we pick a pod with 0.1 due to the randomness over a 0.5, this may dilute the cache hit affinity, causing a cache to cost twice storage as high (e.g, distribute a prefix to 2 pods where 1 pod can hold it). I will be very cautious about the latter (pick a much lower score due to randomness).
### nirrozenbaum commented on Aug 20, 2025 
Contributor
More actions
we could potentially filter out some of the candidate pods and leave only the ones that are either max score or "close" to max score (e.g., not less than 30% of it). so if our max score pod has score of 0.5, this rule will filter out all the pods with score lower than 0.35 and than we do random weight picking from the ones that are relatively reasonable to pick.
### nirrozenbaum commented on Aug 20, 2025 
Contributor
More actions
that was just an example with some magic number of 30%, we could have it as configuration param
### Jooho commented on Aug 21, 2025 
Contributor
More actions
> Agree some randomness helps if the scores are very close (e.g, practically there is probably no difference between a 0.5 vs 0.499 score). I tried something like bucketizing the scores and treat the same bucket as the same in the past. However, if we pick a pod with 0.1 due to the randomness over a 0.5, this may dilute the cache hit affinity, causing a cache to cost twice storage as high (e.g, distribute a prefix to 2 pods where 1 pod can hold it). I will be very cautious about the latter (pick a much lower score due to randomness).
This can be achieved using a Top-N approach, which ensures that pods with low scores are not selected. I updated PR to use TopN.
### vMaroon commented on Aug 21, 2025 
Last edited by vMaroon
Member
More actions
I don't think randomness is the answer - TopN (with a closeness threshold perhaps) is sensible as an additional layer, but I believe the real value will come from smart tuning of the two scorer families we have. I'll quote a section from a [doc](https://docs.google.com/document/d/1PW8ZpMPEVGIRy7P76vyc7MqaXB6VIHlqATS_tfmQPg4/edit?tab=t.k8aq6qim71oj) [@liu-cong](https://github.com/liu-cong) and I worked on a while ago:
> EPP scorer plugins generally fall into two categories:
>   * **Context-aware:** these include session aware, prefix-cache estimation based, and KV-cache aware. These scorers utilize different levels of knowledge to estimate the localities of KV-caches on the vLLM pods, with estimations becoming more precise as the levels get more granular: 
>     * For example, the first scorer utilizes a session-id header that translates to a growing chat, while the third is informed by KV-cache events coming from vLLM pods directly.
>   * **Load-aware:** these focus on metrics such as queue lengths, active request counts, or KV-cache memory utilization. Their goal is to evenly distribute inference load to prevent hotspots and maximize throughput.
> 

> These two categories often pull in opposite directions: context-aware scorers bias toward sticky routing (to improve KV-cache hit rates), while load-aware scorers favor spreading requests across the cluster to reduce queuing delays. Finding the right balance between them is key to minimizing end-to-end latency and maximizing system efficiency - but doing so with static weights may be challenging and suboptimal.
> To address this, the EPP should support **dynamic weight tuning** for scorer plugins. For instance, when cluster-level load imbalance is detected, the system can temporarily increase the weight of load-aware scorers to steer traffic away from overloaded vLLM nodes. Once balance is restored, weights can gradually shift back to favor context-aware routing. This adaptive weighting approach enables the scheduler to respond intelligently to changing cluster conditions.
I think it could be time to get started with this.
### nirrozenbaum commented on Aug 22, 2025 
Contributor
More actions
[@vMaroon](https://github.com/vMaroon) I disagree with your analysis. while dynamic adaptation of scorers weights could be helpful it’s not meant to solve bursty workloads. you can’t expect dynamic scoring weights to get updated while handling a burst. it could be helpful in other scenarios though. I think that is out of scope of the problem that was reported in this issue.
### dagrayvid commented on Aug 28, 2025 
Last edited by dagrayvid
ContributorAuthor
More actions
I repeated the bursty, no-shared prefix workload using [@Jooho](https://github.com/Jooho) 's [WIP WeightedRandomPicker](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1412). The results look really good!

```
                | KServe| top-score-picker      | weighted-random-picker| % change top-score to weighted random
output tok/s    | 17509 | 16861                 | 17601                 | 4.38
TTFT mean       | 1413  | 2083.4                | 367.2                 | -82.3
TTFT median     | 1077  | 1555.2                | 245.9                 | -84.1
TTFT P99        | 6941  | 7749                  | 2061                  | -73.4

```

I plan to run more workloads to see how this picker impacts performance in other use-cases like
  * non bursty workloads
  * multi-turn chat where prefix aware routing is critical


🎉React with 🎉2Reacted by Jooho Lee and Nir Rozenbaum
### nirrozenbaum commented on Aug 28, 2025 
Contributor
More actions
[@dagrayvid](https://github.com/dagrayvid) running these tests is great. I'm hoping to complete soon the review on [@Jooho](https://github.com/Jooho)'s PR that implements a weighted random picker. In theory I think it should solve the original issue that was reported. would you have the capacity to run these tests again after that PR gets merged?
👍React with 👍1Reacted by David Whyte-Gray
### dagrayvid commented on Sep 4, 2025 
Last edited by dagrayvid
ContributorAuthor
More actions
Following up here, I ran 2 more non-bursty workloads with the weighted-random-picker to see how they perform:
  1. Non-bursty workload with no shared prefix (0% prefix cache hit rate). Constant RPS (RPS=17), heterogeneous sequence lengths (mean = 1000, std. dev = 300 for ISL and OSL).



```
             | max-score-picker | weighted-random-picker
--------------------------------------------------------
output tok/s | 15606.55         | 15629.49
TTFT mean    | 298.95           | 164.73
TTFT median  | 273.08           | 156.65
TTFT p95     | 584.97           | 245.28

```

In this case, the weighted-random-picker also outperforms the max-score-picker in terms of TTFT.
  1. Multi-turn chat. Each "conversation" consists of prompts of~256 tokens (std. dev 64), outputs of ~128 tokens (std. dev 64), and 5 turns. The prior messages in the conversation are included in the subsequent requests as in a chat scenario. In this case prefix caching is critical for performance.


I ran multiple tests for each picker with different weights assigned to the prefix-cache-scorer (based off of precise-prefix-cache example in llm-d-infra) -- for example `max-score-w2` used `weight: 2` for the prefix-cache-scorer and `random-sampling-w16` used `weight: 16`. In each case the kv-cache-scorer (utilization) and queue-scorer weights were 1.
Multi-turn chat:

```
              | max-score-w0 | max-score-w2 | max-score-w8 | random-sampling-w2 | random-sampling-w16 | random-sampling-w100
---------------------------------------------------------------------------------------------------------------------------
output tok/s | 6502.86      | 12608.80     | 12602.30     | 8292.74            | 10283.25            | 12360.99
TTFT mean    | 4410.86      | 286.76       | 242.97       | 401.44             | 298.86              | 252.14
TTFT median  | 3278.92      | 185.41       | 177.46       | 341.54             | 246.06              | 183.24
TTFT p95     | 11884.55     | 779.29       | 602.38       | 895.84             | 662.43              | 618.30

```

weighted-random-sampling significantly hurts performance compared to max-score for this use-case, but increasing the weight of the prefix scorer can make up most of the difference.
[dagrayvid](https://github.com/dagrayvid)
closed this as [completed](https://github.com/llm-d/llm-d-router/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)[on Sep 4, 2025](https://github.com/llm-d/llm-d-router/issues/298#event-19515281327)
[nirrozenbaum](https://github.com/nirrozenbaum)
reopened this [on Sep 4, 2025](https://github.com/llm-d/llm-d-router/issues/298#event-19521071095)
### liu-cong commented on Sep 10, 2025 
Member
More actions
> weighted-random-sampling significantly hurts performance compared to max-score for this use-case, but increasing the weight of the prefix scorer can make up most of the difference.
This is what I expect. To make the "randomness" not fight against the scorers, I think the randomness should only happen between candidates with close scores, e.g, introduce random picking between pods with score 2.5 and 2.4, but not between pods with score 2.5 and 1.
[elevran](https://github.com/elevran)
added this to [on Oct 16, 2025](https://github.com/llm-d/llm-d-router/issues/298#event-20318777289)
[elevran](https://github.com/elevran)
moved this to Backlog in [on Oct 19, 2025](https://github.com/llm-d/llm-d-router/issues/298)
### github-actions commented on Dec 9, 2025 
[on Dec 9, 2025](https://github.com/llm-d/llm-d-router/issues/298#issuecomment-3634988248) – with [GitHub Actions](https://help.github.com/en/actions)
Contributor
More actions
This issue is marked as stale after 90d of inactivity. After an additional 30d of inactivity (15d to become rotten, then 15d more), it will be closed. To prevent this issue from being closed, add a comment or remove the `lifecycle/stale` label.
[github-actions](https://github.com/apps/github-actions)
added [on Dec 9, 2025](https://github.com/llm-d/llm-d-router/issues/298#event-21467488562)
[github-actions](https://github.com/apps/github-actions)
added and removed [on Dec 24, 2025](https://github.com/llm-d/llm-d-router/issues/298#event-21747011889)
[github-actions](https://github.com/apps/github-actions)
closed this as [not planned](https://github.com/llm-d/llm-d-router/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Anot-planned)[on Jan 9, 2026](https://github.com/llm-d/llm-d-router/issues/298#event-21959017767)
[github-project-automation](https://github.com/apps/github-project-automation)
moved this from Backlog to Done in [on Jan 9, 2026](https://github.com/llm-d/llm-d-router/issues/298)
[zdtsw](https://github.com/zdtsw)
added a commit that references this issue [on Mar 13, 2026](https://github.com/llm-d/llm-d-router/issues/298#event-23551731419)
[Merge pull request [opendatahub-io#298](https://github.com/opendatahub-io/llm-d-router/pull/298) from zdtsw-forking/sync-odh-to-…](https://github.com/zdtsw-forking/llm-d-router/commit/66b0ff848b9eb1a043604efd066ffd490a8aa0fa)
[Sign up for free](https://github.com/signup?return_to=https://github.com/llm-d/llm-d-router/issues/298)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/llm-d/llm-d-router/issues/298)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
[kind/bugCategorizes issue or PR as related to a bug.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22kind%2Fbug%22)Categorizes issue or PR as related to a bug.[lifecycle/rotten](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22lifecycle%2Frotten%22)[needs-triageIndicates an issue or PR lacks a triage label and requires one.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a triage label and requires one.
No type
### Projects
  * [llm-d-router Closed](https://github.com/orgs/llm-d/projects/15)
StatusShow more project fields


Show 1 closed project
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
