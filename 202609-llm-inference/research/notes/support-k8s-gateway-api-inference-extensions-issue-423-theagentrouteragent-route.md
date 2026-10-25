---
title: 'Support k8s gateway API inference extensions · Issue #423 · theagentrouter/agent-router'
id: support-k8s-gateway-api-inference-extensions-issue-423-theagentrouteragent-route
tags:
- llmd-router-nonk8s-backends-54b774
- epp
- envoy-ai-gateway
- design-proposal
created: '2026-09-11T19:43:56.569124Z'
updated: '2026-09-15T04:55:23.700570Z'
source: https://github.com/envoyproxy/ai-gateway/issues/423
source_domain: github.com
fetched_at: '2026-09-11T19:43:56.548382Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub issue #423 (Feb 2025-Jul 2025, now theagentrouter/agent-router, formerly
  envoyproxy/ai-gateway) is the original design thread for integrating Gateway API
  Inference Extension into Envoy AI Gateway. Design notes lay out the layered customization
  seam central to this research: HTTPRoute maps to one or more InferencePools; InferencePool
  selects pods via labels+port; the InferenceModel (predecessor to InferenceObjective)
  picks which pool serves a request, normally via the OpenAI-schema ''model'' field.
  Crucially, the spec allows offloading model/endpoint selection to a remote ext_proc
  service so operators can implement proprietary picking logic, and the generic case
  needs two ext_proc callouts: (1) select model/pool from body+cost/priority signals,
  (2) select endpoint within the pool to optimize utilization -- these can collapse
  to one callout if HTTPRoute maps to a single pool or if one callout service handles
  both. Reference implementation cited: kubernetes-sigs/gateway-api-inference-extension
  endpoint-picker-protocol proposal (docs/proposals/004-endpoint-picker-protocol).
  Work culminated in PR #823 ''feat: add support for endpoint picker'' merged Jul
  22, 2025 (v0.3.0), preceded by design-doc PR #771.'
utility_score: 10.0
---

[Skip to content](https://github.com/theagentrouter/agent-router/issues/423#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/theagentrouter/agent-router/issues/423) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/theagentrouter/agent-router/issues/423) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/theagentrouter/agent-router/issues/423) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/theagentrouter/agent-router/issues/423).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Ftheagentrouter%2Fagent-router) You must be signed in to change notification settings
  * [ Fork 372 ](https://github.com/login?return_to=%2Ftheagentrouter%2Fagent-router)
  * [ Star  2k ](https://github.com/login?return_to=%2Ftheagentrouter%2Fagent-router)


#  Support k8s gateway API inference extensions
Copy link
Copy link
Closed
2 / 22 of 2 issues completed
Closed
[Support k8s gateway API inference extensions](https://github.com/theagentrouter/agent-router/issues/423#top)#423
Feature
2 / 22 of 2 issues completed
Copy link
Assignees
Labels
[area/apiControl plane API (CRDs)](https://github.com/theagentrouter/agent-router/issues?q=state%3Aopen%20label%3A%22area%2Fapi%22)Control plane API (CRDs)[enhancementNew feature or request](https://github.com/theagentrouter/agent-router/issues?q=state%3Aopen%20label%3A%22enhancement%22)New feature or request
Milestone
## Description
opened [on Feb 25, 2025](https://github.com/theagentrouter/agent-router/issues/423#issue-2879392158)
Last edited by yanavlasov
Contributor
Issue body actions
Support [k8s gateway API inference extensions](https://gateway-api-inference-extension.sigs.k8s.io/concepts/api-overview/#api-overview)
  * Translate k8s gateway API inference extensions CRDs into Envoy configuration
Add e2e tests
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


Design notes:
  * Inference extension maps HTTPRoute to multiple [InferencePools](https://gateway-api-inference-extension.sigs.k8s.io/reference/spec/#inferencepool). Inference pool uses labels to select pods for the pool and the port number to use. There is a [note](https://gateway-api-inference-extension.sigs.k8s.io/reference/spec/#inferencepoolspec) that says a service selector can be used, but not sure how it might work (a well known label?).
  * The [InferenceModel](https://gateway-api-inference-extension.sigs.k8s.io/reference/spec/#inferencemodel) determines which pool is going to be used for serving a request. presently it does by using the `model` value form the request, assuming requests use OpenAI schema.
  * The inference extension spec allows offloading of model and endpoint selection to a remote service that implements ext_proc protocol. This is so operators can implement implement their proprietary business logic for selecting both models and then individual endpoints for serving the request.
  * In the most generic case request processing requires two ext_proc callouts. 
    1. first callout to select the model and as such inference pool for this model based on contents of the body and other requirements (i.e. cost, priority, etc).
    2. second callout to select endpoint from the inference pool - this is to optimize resource utilization for serving requests. Note that in the generic case the endpoint picking extension can be specific to the pool (hence requirement for a dedicated calout).
  * The two steps above can be done by a single callout, if HTTPRoute is mapped to only one pool, external endpoint selection is not needed, or the callout service can select endpoints for any inference pool.
  * Model/pool external picker uses header (TBD: get puclic specification for the header name) to select a route/cluster corresponding to the inference pool.
  * External endpoint picker uses either metadata or well define header to specify primary endpoint. See [proposal](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/docs/proposals/004-endpoint-picker-protocol/README.md) Note that the fallback endpoints (for retries) will be added to that proposal shortly.


Reference implementation for external endpoint picker: <https://github.com/kubernetes-sigs/gateway-api-inference-extension/>
Implementation details and questions:
  1. To support [proposed way](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/main/docs/proposals/004-endpoint-picker-protocol/README.md) of specifying endpoints the cluster either needs to use subsets (a subset for each endpoint) or use a specific LB policy. 
     * [@yanavlasov](https://github.com/yanavlasov) is to open source LB policy to Envoy that will support endpoint picking based on the propsed endpoint picker protocol.
     * It is unclear of Envoy Gateway supports generating endpoint assignments with subsets for each endpoint.
  2. Can Envoy Gateway generate endpoint assignments for the cluster based on Pod labels or does it need a Service ref?


Possible iteration steps:
  1. Consume inference extension CRDs and generate the same configuration as today. This will give us external model/pool selection, but still Envoy internal endpoint selection. 
     * Need to resolve the issue of InferencePool using labels to specify pods.
  2. Open source LB policy to support external endpoint selection in Envoy (parallel to step 1).
  3. Add config (TBD) to use remote endpoint picker - this will use two callouts per request.


👍React with 👍4Reacted by Shane Utt, botengyao, Vladyslav Hordiienko and Xunzhuo
🎉React with 🎉4Reacted by Erica Hughberg, Apoorva Kulkarni, Shane Utt and Dmitry Mishin
🚀React with 🚀1Reacted by Erica Hughberg
## Sub-issues
###  Collapse Sub-issuesSub-issues
2 of 22 of 2 Issues completed
Sub-issue options
2 sub-issues loaded
  * [Support HTTPRoute -> InferencePool Directly #648](https://github.com/theagentrouter/agent-router/issues/648)
Manage this item ` command⌘ shift⇧ uU`
  * [Support DynamicLoadBalancing beyond AIE(API inference extension) #604](https://github.com/theagentrouter/agent-router/issues/604)
Manage this item ` command⌘ shift⇧ uU`


To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 
## Activity
[yanavlasov](https://github.com/yanavlasov)
added 
[enhancementNew feature or request](https://github.com/theagentrouter/agent-router/issues?q=state%3Aopen%20label%3A%22enhancement%22)New feature or request
[on Feb 25, 2025](https://github.com/theagentrouter/agent-router/issues/423#event-16467139678)
[yanavlasov](https://github.com/yanavlasov)
self-assigned this
[on Feb 25, 2025](https://github.com/theagentrouter/agent-router/issues/423#event-16467141237)
[missBerg](https://github.com/missBerg)
added this to the [v0.2.0](https://github.com/theagentrouter/agent-router/milestone/2) milestone [on Feb 25, 2025](https://github.com/theagentrouter/agent-router/issues/423#event-16470212686)
[missBerg](https://github.com/missBerg)
added 
[area/apiControl plane API (CRDs)](https://github.com/theagentrouter/agent-router/issues?q=state%3Aopen%20label%3A%22area%2Fapi%22)Control plane API (CRDs)
[on Feb 25, 2025](https://github.com/theagentrouter/agent-router/issues/423#event-16470215962)
### daixiang0 commented on Feb 26, 2025 
Contributor
More actions
[@yanavlasov](https://github.com/yanavlasov) @envoyproxy/ai-gateway-maintainers I see EG also want to implement it refer to <https://gateway-api-inference-extension.sigs.k8s.io/implementations/#envoy-gateway>, so will we have two implement for this?
### arkodg commented on Feb 26, 2025 
More actions
[@daixiang0](https://github.com/daixiang0) the preference is to implement this in Envoy AI Gateway first, get user feedback, iterate on the API until it moves to standard and then also directly support the implementation in Envoy Gateway
### kfswain commented on Mar 7, 2025 
More actions
We've worked with Envoy GW quite a bit, it's been great. Happy to continue work with y'all to get these projects workin together! Is it easiest if I just pop in on the community meeting on Th?
### arkodg commented on Mar 7, 2025 
More actions
thanks for picking this one up [@kfswain](https://github.com/kfswain) ! you're familiar with the translation so this should be _easy_ for you 😄 the community meeting is a great place to bring your questions
hey [@mathetake](https://github.com/mathetake) can you point Kellen to the reconciliation and translation areas
### mathetake commented on Mar 7, 2025 
Contributor
More actions
[@yanavlasov](https://github.com/yanavlasov) said he will start working from next week, so could you [@kfswain](https://github.com/kfswain) coordinate with Yan (i assume you both are working at Google)
👍React with 👍1Reacted by Kellen Swain
### kfswain commented on Mar 7, 2025 
More actions
SGTM!
👍React with 👍1Reacted by Takeshi Yoneda
### mathetake commented on Mar 11, 2025 
Contributor
More actions
Hi [@yanavlasov](https://github.com/yanavlasov) [@kfswain](https://github.com/kfswain) Luckily I have cycles to work on this from tomorrow, so I will do the initial work soon and i might ask for some help from you guys whenever I need. SG?
### kfswain commented on Mar 12, 2025 
More actions
That sounds great. Happy to help
### mathetake commented on Mar 12, 2025 
Contributor
More actions
now i get the big picture on how to implement in here... i have one question [@kfswain](https://github.com/kfswain) may i ask where's the conformance test suite? i couldn't find one in the repo
### yuzisun commented on Mar 12, 2025 
Contributor
More actions
Let's have a design doc first, this certainly changes the scope of the envoy ai gateway project.
### mathetake commented on Mar 12, 2025 
Last edited by mathetake
Contributor
More actions
[@yuzisun](https://github.com/yuzisun) certainly, i will write up by noon tomorrow
🚀React with 🚀2Reacted by MaYuanyuan and Kellen Swain
###  27 remaining items
Load more
### kerthcet commented on Jun 17, 2025 
Last edited by kerthcet
Contributor
More actions
Agree to reuse the capacity of envoy gateway, I can help with the normal setup of endpoint picker (rather than inferencePool) if any work left since it's exactly what we expect.
[mathetake](https://github.com/mathetake)
assigned and unassigned [on Jun 19, 2025](https://github.com/theagentrouter/agent-router/issues/423#event-18238479403)
[mathetake](https://github.com/mathetake)
added a commit that references this issue [on Jul 3, 2025](https://github.com/theagentrouter/agent-router/issues/423#event-18463325074)
[docs: add epp integration proposal ([#771](https://github.com/theagentrouter/agent-router/pull/771))](https://github.com/theagentrouter/agent-router/commit/c3fda88d50e0913d2e4e61bcae27b8685d9f0383)
[mathetake](https://github.com/mathetake)
added a sub-issue [on Jul 7, 2025](https://github.com/theagentrouter/agent-router/issues/423#event-18509947101)
  * [Support DynamicLoadBalancing beyond AIE(API inference extension) #604](https://github.com/theagentrouter/agent-router/issues/604)


[alexagriffith](https://github.com/alexagriffith)
added 5 commits that reference this issue [on Jul 9, 2025](https://github.com/theagentrouter/agent-router/issues/423#event-18553021135)
[docs: add epp integration proposal ([theagentrouter#771](https://github.com/theagentrouter/agent-router/pull/771))](https://github.com/sukumargaonkar/ai-gateway/commit/1b908b094cae9effe9bf580ae8480bb6e4765018)
[adding gcp anthropic translation files](https://github.com/sukumargaonkar/ai-gateway/commit/9364184084baff218180236f25de6a23c69393ca)
[adding gcp anthropic translation files](https://github.com/sukumargaonkar/ai-gateway/commit/f6dfe320d16561286469e3aec3646732faae1ed4)
[docs: add epp integration proposal ([theagentrouter#771](https://github.com/theagentrouter/agent-router/pull/771))](https://github.com/sukumargaonkar/ai-gateway/commit/fa728168e4696370d493c49ecc8e278298de9817)
[missBerg](https://github.com/missBerg)
added a commit that references this issue [on Jul 14, 2025](https://github.com/theagentrouter/agent-router/issues/423#event-18627073754)
[site: organize blog section and add ref arch blog post ([#890](https://github.com/theagentrouter/agent-router/pull/890))](https://github.com/theagentrouter/agent-router/commit/70861b4fdbab12630a17daa2220c85890b445e1c)
[Xunzhuo](https://github.com/Xunzhuo)
closed this as [completed](https://github.com/theagentrouter/agent-router/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)in [#823](https://github.com/theagentrouter/agent-router/pull/823)[on Jul 22, 2025](https://github.com/theagentrouter/agent-router/issues/423#event-18750296695)
[Xunzhuo](https://github.com/Xunzhuo)
added a commit that references this issue [on Jul 22, 2025](https://github.com/theagentrouter/agent-router/issues/423#event-18750296737)
[feat: add support for endpoint picker ([#823](https://github.com/theagentrouter/agent-router/pull/823))](https://github.com/theagentrouter/agent-router/commit/1f6961daf53308c8d772f655b2fff2fe405fa6bd)
[mathetake](https://github.com/mathetake)
added a commit that references this issue [on Jul 23, 2025](https://github.com/theagentrouter/agent-router/issues/423#event-18785386885)
[site: update refarch blog ([#926](https://github.com/theagentrouter/agent-router/pull/926))](https://github.com/theagentrouter/agent-router/commit/a9d8bf74ee4b15123217e087485e81b54c4aab4a)
[Sign up for free](https://github.com/signup?return_to=https://github.com/theagentrouter/agent-router/issues/423)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/theagentrouter/agent-router/issues/423)
## Metadata
## Metadata
### Assignees
  * [Xunzhuo](https://github.com/Xunzhuo)


### Labels
[area/apiControl plane API (CRDs)](https://github.com/theagentrouter/agent-router/issues?q=state%3Aopen%20label%3A%22area%2Fapi%22)Control plane API (CRDs)[enhancementNew feature or request](https://github.com/theagentrouter/agent-router/issues?q=state%3Aopen%20label%3A%22enhancement%22)New feature or request
### Projects
No projects
### Milestone
  * [ v0.3.0No due date ](https://github.com/theagentrouter/agent-router/milestone/3)


### Relationships
None yet
### Development
  * [ feat: redo Inference Extension supporttheagentrouter/agent-router ](https://github.com/theagentrouter/agent-router/pull/614)
  * [feat: add support for endpoint pickertheagentrouter/agent-router](https://github.com/theagentrouter/agent-router/pull/823)[Releasev0.3.0](https://github.com/theagentrouter/agent-router/releases/tag/v0.3.0)


### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
