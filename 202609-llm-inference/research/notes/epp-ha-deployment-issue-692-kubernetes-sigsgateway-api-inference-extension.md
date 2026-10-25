---
title: 'EPP HA deployment · Issue #692 · kubernetes-sigs/gateway-api-inference-extension'
id: epp-ha-deployment-issue-692-kubernetes-sigsgateway-api-inference-extension
tags:
- llmd-kv-cache-routing-c665d5
- epp-ha
- maintainer-issue
created: '2026-09-14T13:03:08.495047Z'
updated: '2026-10-10T14:52:17.317770Z'
source: https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/692
source_domain: github.com
fetched_at: '2026-09-14T13:03:08.474643Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GIE GitHub issue #692 (Apr-Aug 2025, closed completed): tracks adding HA
  support for the Endpoint Picker (EPP), which runs as a single replica by default.
  Maintainers explicitly flag that ''with multiple active EPP replicas, we expect
  decreased performance on stateful operations such as queuing and prefix cache aware
  routing'' because EPP state (in-flight counts, prefix-cache maps) is in-memory and
  per-replica; sharding InferencePools/InferenceModels across active-active EPPs is
  flagged as imperfect since multiple LoRA adapters can co-reside on the same pod.
  Resolved short-term via active-passive leader election (#1337); active-active sharding
  deferred to #1406. Maintainer-authored, not an independent benchmark, but a first-party
  admission that EPP horizontal scaling degrades the accuracy of the load/cache-affinity
  signals the router depends on.'
---

[Skip to content](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/692#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/692) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/692) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/692) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/692).
/ **[gateway-api-inference-extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension) You must be signed in to change notification settings
  * [ Fork 313 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)
  * [ Star  767 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)


#  EPP HA deployment
Copy link
Copy link
Closed
Listed in
Closed
Task
Listed in
Copy link
Assignees
Labels
[triage/acceptedIndicates an issue or PR is ready to be actively worked on.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22triage%2Faccepted%22)Indicates an issue or PR is ready to be actively worked on.
Milestone
## Description
opened [on Apr 14, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/692#issue-2994424057)
Last edited by danehans
Contributor
Issue body actions
**What would you like to be added** :
Our current user guide sets 1 replica for EPP: 
[gateway-api-inference-extension/config/manifests/inferencepool-resources.yaml](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/92431f582ca4f8c6d75781e303acd8c84492dbea/config/manifests/inferencepool-resources.yaml#L39)
Line 39 in [92431f5](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/92431f582ca4f8c6d75781e303acd8c84492dbea)  
|  replicas: 1  |  
| --- |  
We should provide the option to run EPP in HA mode. I suggest the following priorities:
  * Short-term: Active-passive mode. This is a good starting point, as in some benchmarks we saw EPP can handle >500 QPS, and 10s (potentially 100s) of model servers, without a clear bottleneck. So a single leader EPP is sufficient for many use cases. Currently we run EPP without leader election [code](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/92431f582ca4f8c6d75781e303acd8c84492dbea/pkg/epp/server/runserver.go#L124), we will need to modify EPP to support leader election, and run some test to verify the downtime if the leader dies.
Mid-Long term: Support active-active EPPs, with options to shard the InferenceModels and InferencePool. With multiple active EPP replicas, we expect decreased performance on stateful operations such as queuing and prefix cache aware routing. This will require some experiments to quantify the impact.
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


If there are too many inference models, or if if the pool becomes very large, we may need to further shard them.
**Why is this needed** :
👍React with 👍1Reacted by Shane Utt
## Activity
### nirrozenbaum commented on Apr 15, 2025 
Last edited by nirrozenbaum
Contributor
More actions
for the mid-long term, please keep in mind that multiple models are running on a single pod, and there might be a setup where sharding is not possible. for example - pod1 runs adapter1,2,3 pod2 runs adapter2,3,4 pod3 runs adapter 3.4,1 pod4 runs adapter 4,1,2
in this setup if epp is scraping from one pod it should scrape from all of them to get the full state. If sharding was possible that is the ideal solution but unfortunately I think this is not going to work.
I also thought about this topic and came into conclusion that we need a high speed distributed cache, something like redis. see this issue - [#671](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/671)
[kfswain](https://github.com/kfswain)
added theissue type [on Apr 21, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/692#event-17344194340)
### kfswain commented on Apr 21, 2025 
Contributor
More actions
> for the mid-long term, please keep in mind that multiple models are running on a single pod, and there might be a setup where sharding is not possible. for example - pod1 runs adapter1,2,3 pod2 runs adapter2,3,4 pod3 runs adapter 3.4,1 pod4 runs adapter 4,1,2
> in this setup if epp is scraping from one pod it should scrape from all of them to get the full state. If sharding was possible that is the ideal solution but unfortunately I think this is not going to work.
> I also thought about this topic and came into conclusion that we need a high speed distributed cache, something like redis. see this issue - [#671](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/671)
I think we would actually need to benchmark/load test this to be certain. Adding redis adds latency to a decision we are already trying to make essentially real time. Redis, while fast, would still be slower than in mem.
Agreed that sharding is imperfect, but I think we actually would need to test this before we can be sure.
### liu-cong commented on Apr 24, 2025 
ContributorAuthor
More actions
> for the mid-long term, please keep in mind that multiple models are running on a single pod, and there might be a setup where sharding is not possible.
At that point we may need a control plane component to place/shard those adapters to pods, and align that sharding decision with EPPs. We should gather some real use case before going too far there though.
We haven't benchmarked but I think EPPs having the global view of all pods will likely be enough for a long time.
[kfswain](https://github.com/kfswain)
added 
[triage/acceptedIndicates an issue or PR is ready to be actively worked on.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22triage%2Faccepted%22)Indicates an issue or PR is ready to be actively worked on.
[on Apr 24, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/692#event-17396786012)
### rlakhtakia commented on Apr 28, 2025 
Contributor
More actions
/assign
[k8s-ci-robot](https://github.com/k8s-ci-robot)
assigned [on Apr 28, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/692#event-17439478500)
### danehans commented on May 8, 2025 
Contributor
More actions
[@rlakhtakia](https://github.com/rlakhtakia) thanks for taking on this issue. When you have a moment, do you mind providing an update on your progress?
[danehans](https://github.com/danehans)
added this to the [v0.5](https://github.com/kubernetes-sigs/gateway-api-inference-extension/milestone/8) milestone [on Jun 12, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/692#event-18120167308)
[nirrozenbaum](https://github.com/nirrozenbaum)
modified the milestones: [v0.5](https://github.com/kubernetes-sigs/gateway-api-inference-extension/milestone/8), [v0.6](https://github.com/kubernetes-sigs/gateway-api-inference-extension/milestone/9) [on Jul 14, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/692#event-18623910183)
[nirrozenbaum](https://github.com/nirrozenbaum)
modified the milestones: [v1.0-candidates (needs triage)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/milestone/9), [v1.0 (InferencePool GA)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/milestone/6) [on Aug 3, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/692#event-18955185400)
### danehans commented on Aug 15, 2025 
Contributor
More actions
[@rlakhtakia](https://github.com/rlakhtakia) our [next release](https://github.com/kubernetes-sigs/gateway-api-inference-extension/milestones) is scheduled for 8/20. Can you provide an update on the status of this issue?
cc: [@kfswain](https://github.com/kfswain) [@ahg-g](https://github.com/ahg-g) [@robscott](https://github.com/robscott)
### kfswain commented on Aug 15, 2025 
Last edited by kfswain
Contributor
More actions
I can update this with artifacts when I'm at my desk (eta prob tomorrow).
But we have benchmarks showing the degraded performance of the passive case from Radhika, emphasizing why we need active-passive.
And we have a simple HA setup using leader election to manipulate ready status. That's been submitted in [#1337](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1337). I'll have a proposal for a more robust and long term HA solution shortly, it should be simple to do, but it will be envoy-based.
### danehans commented on Aug 15, 2025 
Contributor
More actions
> And we have a simple HA setup using leader election to manipulate ready status. I believe that's been submitted
I see [#1337](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1337) recently landed to resolve the first check box in this issue. IMO, we should spin off the 2nd checkbox into a separate issue if we don't expect active-active to land soon.
👍React with 👍1Reacted by Nir Rozenbaum
### kfswain commented on Aug 15, 2025 
Contributor
More actions
That Sgtm, we can discuss if/when in that issue
### danehans commented on Aug 19, 2025 
Contributor
More actions
Closing since active-passive HA was added and [#1406](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1406) was created to track active-active HA.
[danehans](https://github.com/danehans)
closed this as [completed](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)[on Aug 19, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/692#event-19227480562)
[Sign up for free](https://github.com/signup?return_to=https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/692)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/692)
## Metadata
## Metadata
### Assignees
  * [rlakhtakia](https://github.com/rlakhtakia)


### Labels
[triage/acceptedIndicates an issue or PR is ready to be actively worked on.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22triage%2Faccepted%22)Indicates an issue or PR is ready to be actively worked on.
### Projects
No projects
### Milestone
  * [ v1.0 (InferencePool GA)Closed Sep 15, 2025, 100% complete ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/milestone/6)


### Relationships
None yet
### Development
No branches or pull requests
### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
