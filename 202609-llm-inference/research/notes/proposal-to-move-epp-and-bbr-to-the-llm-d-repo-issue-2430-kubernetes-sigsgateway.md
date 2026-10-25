---
title: 'Proposal to Move EPP and BBR to the llm-d Repo · Issue #2430 · kubernetes-sigs/gateway-api-inference-extension'
id: proposal-to-move-epp-and-bbr-to-the-llm-d-repo-issue-2430-kubernetes-sigsgateway
tags:
- gateway-routing
- epp
- governance
- llm-d
- audited-absence
created: '2026-10-09T23:02:49.024525Z'
updated: '2026-10-10T14:52:17.175961Z'
source: https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2430
source_domain: github.com
fetched_at: '2026-10-09T23:02:49.000993Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub issue #2430 (kubernetes-sigs/gateway-api-inference-extension, opened
  Feb 26 2026 by Google''s ahg-g, closed completed May 13 2026) -- the single most
  load-bearing primary-source document in this batch for the governance question:
  it is the actual proposal and multi-week contested debate over moving the reference
  Endpoint Picker (EPP) implementation and the Body-Based Router (BBR) OUT of the
  neutral kubernetes-sigs org and INTO the llm-d project''s own repo, with the InferencePool
  API, EPP protocol, and conformance tests staying in k8s/Gateway API.


  Problem statement from the proposal author: two repos (upstream IGW and llm-d-inference-scheduler)
  were each shipping an EPP image, with ''advanced plugins (like P/D and precise kv-cache
  aware scheduling)... mostly driven by the llm-d project,'' creating ''fragmented
  code and duplicate maintenance and documentation'' and confusing both developers
  and end users about ''what to put where.'' The rationale frames IGW as three separable
  layers -- (1) the InferencePool API + endpoint-picker protocol that stays neutral
  and Gateway-API-owned, (2) a conformant gateway ecosystem (named explicitly: GKE
  Gateway, Istio, kGateway, NGINX Gateway Fabric), and (3) the EPP implementation
  itself, proposed to move -- with the explicit analogy ''why should k8s pick a winning
  EPP?'' the same way it doesn''t pick a winning proxy.


  Captures a sharp, named, unresolved governance dispute that is direct evidence of
  what ''neutral OSS infrastructure'' costs an adopter: Microsoft engineer keithmattix
  (Istio maintainer) argues the move destroys GAIE''s core value proposition -- ''What
  LLM-D doesn''t provide that GAIE did is neutral governance. Other inference frameworks
  that want to create an EPP would be faced with starting from scratch or importing
  a competitor''s (i.e. llm-d''s) EPP'' -- and explicitly names NVIDIA Dynamo and
  Ray/KubeRay as non-llm-d inference frameworks with a stake in EPP neutrality (''the
  latter [dynamo] already has an EPP derived from the library that''s present in GAIE'').
  llm-d maintainer kfswain counters that llm-d itself is becoming foundation-governed
  (''soon to be in the same foundation as K8s'') and that a lightweight reference
  EPP remains for conformance testing, so ''anyone can implement their own EPP and
  either use the lightweight version or the llm-d EPP as a reference.'' A third commenter
  (dims) flags mid-thread that NVIDIA Dynamo is independently vendoring its own EPP
  fork with 3 added plugins (github.com/ai-dynamo/dynamo/tree/main/deploy/inference-gateway/epp)
  rather than depending on the shared upstream code -- direct evidence that major
  vendors are already forking/duplicating EPP logic rather than trusting a single
  shared implementation.


  Resolution: the proposal author confirms ''The move happened'' (May 13 2026) and
  closes the issue as completed. A late comment (May 26, after close) flags a concrete
  regression risk the debate anticipated: ''How can we still use envoy gateway? Llm-d
  does not reference it as supports igw, seems pretty regression if that is the case''
  -- i.e., Envoy Gateway support for the EPP protocol may have been weakened or lost
  in the migration, an unresolved/open concern as of the note''s capture date. Also
  surfaces, in passing, that the EPP spec added a conformance-test MUST requirement
  (an Envoy metadata field, x-gateway-endpoint-served or similar) before any open-source
  gateway implementation other than Istio could actually satisfy it -- evidence that
  conformance requirements have at times been driven by one implementation''s needs
  ahead of the broader ecosystem being able to comply.'
---

*Suggested by [[docs-add-design-proposal-for-migrating-epp-to-llm-d-inference-scheduler-by-andyz]] — the upstream GIE-to-llm-d EPP consolidation plan referenced by the KAITO migration PR*

[Skip to content](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2430#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2430) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2430) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2430) to refresh your session. Dismiss alert
#  Proposal to Move EPP and BBR to the llm-d Repo
Copy link
Copy link
Closed
Closed
[Proposal to Move EPP and BBR to the llm-d Repo](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2430#top)#2430
Copy link
## Description
opened [on Feb 26, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2430#issue-3997290878)
Last edited by ahg-g
Contributor
Issue body actions
### Problem
We currently have two repositories each producing an EPP image, the upstream [inference gateway](https://github.com/kubernetes-sigs/gateway-api-inference-extension) and the [llm-d-scheduler](https://github.com/llm-d/llm-d-inference-scheduler/tree/main). The llm-d inference scheduler extends the IGW with a set of plugins (basically using it as a library).
Advanced plugins (like P/D and precise kv-cache aware scheduling) as well as recent scheduling framework enhancements have mostly been driven by the llm-d project, with most new plugins being hosted in the llm-d repo. This created a suboptimal situation, fragmented code and duplicate maintenance and documentation.
This not only created confusion among developers on what to put where, but also confusion among end users as well.
### Proposal
Move the Endpoint Picker code from its [current repo under kubernetes org](https://github.com/kubernetes-sigs/gateway-api-inference-extension) to the[ llm-d scheduler repo](https://github.com/llm-d/llm-d-inference-scheduler). The InferencePool API and the EPP protocol definition along its conformance tests will stay in k8s, and ideally move eventually to the Gateway api repository.
The BBR would also move, but into a separate repository.
The two APIs, InferenceObjective and InferenceModelRewrite APIs would also move with EPP since they don’t spec any behavior related to the Gateway itself, rather to the EPP only.
### Rationale
The Inference Gateway (IGW) project is split into three distinct functional parts:
  * An API ([InferencePool](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/002-api-proposal)) and protocol ([endpoint picker protocol](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/004-endpoint-picker-protocol)) that extends the Kubernetes Gateway API. These spec how a Gateway should configure an external callout service—the endpoint picker—for inference-optimized request scheduling. Crucially, the API and protocols remain agnostic of the selection logic, leaving the specific endpoint selection algorithm to the picker implementation.
  * A conformant Gateway ecosystem that supports the InferencePool API and Endpoint Picker protocol. As of this proposal, this includes GKE Gateway, Istio, kGateway, and NGINX Gateway Fabric.
  * An implementation of the endpoint picker service, known as [epp](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/pkg/epp).


Beyond standard protocol implementation (primarily ExtProc gRPC handling), the core value of the epp lies in its inference-specific optimizations. These include routing requests based on LoRA adapter affinity, lowest KV-cache utilization, and predicted latency to pick the most efficient endpoint for efficient serving.
Advancing these optimizations requires deep AI/ML domain expertise and tight integration with model server implementations. Consequently, the ideal community to drive the endpoint picker forward is one that actively involves model server developers (such as the vLLM and sglang communities) and model developers, ensuring the scheduler evolves alongside the underlying serving technology.
As such, llm-d (under CNCF sandbox project application) offers this community and the ideal home for epp to continue to evolve and adapt to new innovations in GenAI serving.
Discussed on sig-net meeting on March 12th 2026: <https://docs.google.com/document/d/1bEA0F8WddLMnQp7zUfr1rZOwRqX6g2y4kcylEcGATHo/edit?tab=t.0>
Detailed migration docs:
  1. Overall: <https://docs.google.com/document/d/17ZVd4poQfLHSBmKSgLrhGGEBhbwQzH4DJes-D4MLYd4/edit?tab=t.0>
  2. Specific to EPP: <https://docs.google.com/document/d/1PMJfVNPZEDDSVU33qgH95g6v09ZNBIINbU8AZVilKpY/edit?tab=t.0#heading=h.sjv62khj5lf9>


👍React with 👍10Reacted by Rahul Gurnani, Anish Asthana, Pierangelo Di Pilato, Kay Yan, Maroon Ayoub, Kellen Swain, Nili Guy, Rishabh Saini, Ryan R. Rosario and Dingkang Li
## Activity
  1. [ahg-g](https://github.com/ahg-g)
added 
[needs-triageIndicates an issue or PR lacks a `triage/foo` label and requires one.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a `triage/foo` label and requires one.
[on Feb 26, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2430#event-23114301806)
  2. [ahg-g](https://github.com/ahg-g)
pinned this issue [on Feb 26, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2430#event-23114308200)
  3. [ahg-g](https://github.com/ahg-g)
removed 
[needs-triageIndicates an issue or PR lacks a `triage/foo` label and requires one.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a `triage/foo` label and requires one.
[on Mar 9, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2430#event-23404816281)
  4. ### kfswain commented on Mar 9, 2026 
Contributor
More actions
/lgtm
  5. ### danehans commented on Mar 9, 2026 
Contributor
More actions
/lgtm
  6. ### nirrozenbaum commented on Mar 9, 2026 
Last edited by nirrozenbaum
Contributor
More actions
while the proposal is overall ok, it’s worth mentioning the small details that should be addressed, for example: contribution history should be preserved, old images should be preserved for those using IGW today, what about open issues/PRs, is it possible to somehow move them?, what’s the timeline for the move? maintainersip in the proposed repos, and more open points..
probably all of us thought at least on some of those, it’s worth having a checklist so we can follow.
we need to sort out all these issues before making any changes.
👍React with 👍2Reacted by Mike Morris and yannisbcu
  7. [ahg-g](https://github.com/ahg-g)
changed the title ~~[-] Proposal to Move EPP to the llm-d Repo[/-]~~ [+]Proposal to Move EPP and BBR to the llm-d Repo[/+] [on Mar 12, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2430#event-23510582617)
  8. ### keithmattix commented on Mar 16, 2026 
Contributor
More actions
My biggest concern with this move is to ensure BBR and EPP are still accessible to users in some form. I haven't seen a clear vision for what happens to that functionality for non llm-d users
  9. ### kfswain commented on Mar 16, 2026 
Contributor
More actions
One of the primary llm-d paradigms is composability. The EPP & BBR will continue to function as they have, and it honestly shouldn't be any different than it is today.
  10. ### keithmattix commented on Mar 16, 2026 
Contributor
More actions
But what LLM-D doesn't provide that GAIE did is neutral governance. Other inference frameworks that want to create an EPP would be faced with starting from scratch or importing a competitor's (i.e llm-d's) EPP
  11. ### kfswain commented on Mar 17, 2026 
Contributor
More actions
Sorry, I'm not following, where is the competitor point from? llm-d is in an Apache product and soon to be in the same foundation as K8s. We would love contributions there, just as we have here.
The move just centralizes the efforts, and gets us down to a single released artifact, while remaining close to the inference ecosystem. None of those should really cause any delta on competition. If that is the perception, I would love to understand the why. I hope we can make llm-d as rich an open ecosystem as k8s is, or any other open project.
  12. ### keithmattix commented on Mar 17, 2026 
Last edited by keithmattix
Contributor
More actions
The genesis of the EPP was that it should be a standardized interface for gateways to be able to route efficiently to model servers based on metrics from vLLM, sglang, etc. This would imply that there should be many EPPs in the future, and the current EPP is written in a modular format to support that. LLM-D is far from the only inference framework that runs on k8s: rayserve/kuberay and nvidia dynamo immediately come to mind, and the latter already has an EPP derived from the library that's present in GAIE. Why would/should dynamo, ray, or any other inference framework have to import from llm-d when the promise of GAIE (as I understood it) was that the specification and modules within it would remain neutral from any particular implementation?
👍React with 👍1Reacted by Mike Morris
  13. ### kfswain commented on Mar 17, 2026 
Contributor
More actions
I think I understand your argument, and it's a fair point; IGW/EPP should be neutral, composable, and usable by all. I deeply resonate with that.
But I want to call out some implicits that I think are here: What makes K8s more neutral than llm-d in this regard? We have seen very healthy contribution in llm-d from many different orgs. Llm-d seems to have the right combination of folks to help foster a collaborative environment that can experiment quickly with SotA serving methods.
I can't think of anything that makes llm-d less neutral than K8s, we support multiple accelerator types, cloud providers, and provide implementations that are entirely composable. The same principles that makes K8s great, makes llm-d great. There is just a tighter focus on inference/AI, and its less established so there is more agility.
  14. ### keithmattix commented on Mar 17, 2026 
Contributor
More actions
What makes K8s/GAIE valuable as a standards body is that it doesn't implicitly crown a winner in the space. All implementations that implement it are equal; the standard evolves in a way to benefit all implementations and having multiple conformant implementations lends credibility to the neutrality of the spec. LLM-D is its own implementation and thus has implicit bias that could cause the spec to prioritize its needs over others. Again, llm-d is not the only framework in the space; in fact dynamo and ray serve both predate the llm-d announcement (based on my quick Googling). I strongly believe it benefits the community of AI practitioners on Kubernetes to have a neutral location to advance the general capabilities of the ecosystem. We've seen how well this model works with Gateway API, and after reading this issue and other various threads, I'm still unclear on the rationale for abandoning a model that clearly works for the community.
👍React with 👍1Reacted by Mike Morris
  15. ### ahg-g commented on Mar 17, 2026 
Last edited by ahg-g
ContributorAuthor
More actions
> What makes K8s/GAIE valuable as a standards body is that it doesn't implicitly crown a winner in the space.
This is actually an argument for moving EPP out. The existing EPP is one implementation of the APIs that this project standardized, the InferencePool API is the standard and the one with well defined conformance tests. Similar to the argument "why should k8s pick a winning proxy", why should k8s pick a winning EPP?
The EPP implementation is NOT the standard; but note that as part of the proposed move, a lightweight EPP will be kept with the InferencePool API to use for conformance tests. Anyone can implement their own EPP and either use the lightweight version or the llm-d EPP as a reference. They also have the option if they wish to extend the llm-d EPP.
> We've seen how well this model works with Gateway API, and after reading this issue and other various threads, I'm still unclear on the rationale for abandoning a model that clearly works for the community.
This move is actually aligned with Gateway API. Gateway API doesn't offer any implementations, it only defines the APIs and conformance tests. As mentioned above, EPP is analogous to proxy.
  16. ###  13 remaining items
Load more
  17. ### ahg-g commented on Mar 27, 2026 
Last edited by ahg-g
ContributorAuthor
More actions
> [@ahg-g](https://github.com/ahg-g) I'm aligned with the EPP move as long as there's a clear plan for how the important conformance work (for gateway implementations) as well as the EPP protocol evolution will continue 🙂
A lightweight epp will be available to run conformance tests.
> This is exactly what my concern is. "BBR is growing..." - why? Because llm-d needs it to grow? That sort of tension is why we need an implementation-neutral body to help decide what should be codified in the spec and what should be left to implementations. And my understanding is that GAIE is supposed to be that group.
BBR is growing has nothing to do with llm-d; it is growing because the field is evolving and new requirements are coming in. If you are interested in having the BBR in its current state to continue to be in k8s, feel free to propose that as an independent project.
> I don't see how this can be true when, as I mentioned earlier, the x-gateway-endoint-served (or some variation of that) Envoy metadata field was added to the EPP spec because the implementation needed it. And then that metadata field was added to conformance before it was even possible to implement in OSS Envoy! And none of those implementations are conformant with v1.4 with that requirement (except for Istio which I think just got there last week after [@ericdbishop](https://github.com/ericdbishop) enhanced OSS Envoy). I linked a thread where NGINX couldn't implement it either. So when this issue was initially opened, there were ~0 conformant OSS implementations.
This is not related to the epp implementation, it is actually related to the fact that the endpoint picking protocol specced that the proxy may fallback to a different endpoint from the one the picker actually picked. It is reasonable to suggest closing the loop and inform the picker with what was actually picked for a whole host of reasons.
  18. ### keithmattix commented on Mar 27, 2026 
Contributor
More actions
> A lightweight epp will be available to run conformance tests.
So you're saying GAIE is committing to evolving the existing suite of conformance tests in a vendor-neutral way that does not prioritize or elevate llm-d above other implementations? And that the EPP protocol will similarly continue as a standard outside of llm-d (driven by GAIE)?
> BBR is growing has nothing to do with llm-d; it is growing because the field is evolving and new requirements are coming in. If you are interested in having the BBR in its current state to continue to be in k8s, feel free to propose that as an independent project.
[@ahg-g](https://github.com/ahg-g) again this is exactly my concern. The fact that the field is evolving and new requirements are coming in has NOTHING to do with where that work happens. We're dealing with "new requirements" and an "evolving field" within the AI Gateway Working Group and we're able to do that without taking code out of a Kubernetes standard specification and giving it to one implementation exclusively. Gateway API deals with new requirements all of the time, and we don't need to give code or APIs to a specific implementation. Kube-agentic-networking is newer than GAIE, owns code, and it still creating resources, prototypes, etc. that are not being donated to a specific implementation. CAPI, the CSI spec, persistent volumes, DRA - there are a plethora of examples within Kubernetes of how changing requirements are addressed in a vendor neutral way, so I'm unfortunately compelled to just reject that argument outright. GAIE is supposed to be the independent project you suggest I create, and what I'm hearing from you is that the people behind this subproject are now uninterested in developing a vendor-neutral specification and would instead prefer to innovate in their implementation of choice.
> This is not related to the epp implementation, it is actually related to the fact that the endpoint picking protocol specced that the proxy may fallback to a different endpoint from the one the picker actually picked. It is reasonable to suggest closing the loop and inform the picker with what was actually picked for a whole host of reasons.
But the specification doesn't "suggest" it; it requires it as a MUST. And this still doesn't address what process issue led to a conformance requirement that wasn't implementable in OSS. It doesn't inspire confidence in the future of conformance.
👍React with 👍1Reacted by Mike Morris
  19. ### kfswain commented on Mar 30, 2026 
Last edited by kfswain
Contributor
More actions
[@keithmattix](https://github.com/keithmattix)
Given that the comments on this issue has been the extent of your contributions to this project. Your clear passion is noted, and we would love for you to get involved further.
Are you offering to take the burden of all releases such that they would continue to coordinate with all projects that depend on this one seamlessly?
It's non-trivial amount of effort and your clear passion would be great to channel to more than just comments.
Otherwise, politicizing an effort that's intended to streamline process seems undue and not in the spirit of open source
  20. ### keithmattix commented on Mar 30, 2026 
Last edited by keithmattix
Contributor
More actions
> Are you offering to take the burden of all releases such that they would continue to coordinate with all projects that depend on this one seamlessly?
If it would help the GAIE community achieve its goals around conformance and standardization, I'm happy to get more involved either directly or through folks on my team like [@ericdbishop](https://github.com/ericdbishop) and [@jaellio](https://github.com/jaellio). I don't want to be misunderstood: I want GAIE to succeed. Though I'm not as active as I originally set out to be, I've been around since the early days ([#958](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/958) [#974](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/974) [#978](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/978)) and try to keep tabs on things while involved in my other projects. My apologies if I've missed calls for other implementations or consumers of GAIE to assist in conformance or standardization efforts. I have no problem with streamlining process; however, as I understand it, it's unprecedented for code within a standard k8s subproject to change ownership to live within just one implementation of that standard.
  21. ### howardjohn commented on Apr 2, 2026 
Last edited by howardjohn
Contributor
More actions
> Given that the comments on this issue has been the extent of your contributions to this project. Your clear passion is noted, and we would love for you to get involved further. Otherwise, politicizing an effort that's intended to streamline process seems undue and not in the spirit of open source
If the project is meant to be a vendor-neutral API supporting a variety of implementations, it doesn't seem great to dismiss someone's -- who led the implementation of one of the most popular implementations of the API -- opinions based on their "lack of contributions".
Implementors (and users) are the ones whose voices should matter the most...
I don't really have a strong stake in the argument either way but I found your comments, quite frankly, shocking and disrespectful.
❤️React with ❤️2Reacted by Mike Morris and Ed Sealing
  22. ### kfswain commented on Apr 7, 2026 
Contributor
More actions
Hey [@howardjohn](https://github.com/howardjohn)
Thanks for the feedback. If there was any disrespect conveyed, that was not intended.
There also wasn't intended to be any dismissal.
The conversation has been going back and forth for a few weeks (linking original comment so this isnt hearsay): [#2430 (comment)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2430#issuecomment-4070954669)
At a certain point we have to agree to disagree and if there are genuinely strong enough feelings, folks should always feel empowered to contribute.
Not all decisions will be loved by all. My engagement was meant as a means to try find solutions that can work for as many as possible, but it seems its become counter productive. So again, no ill-intent meant, I think there is just a fundamental difference in perspectives is all.
  23. ### keithmattix commented on Apr 7, 2026 
Contributor
More actions
[@kfswain](https://github.com/kfswain) +1 on agreeing to disagree. I've raised my concerns and that's all I can do. I'm encouraged by the direction of [#2790](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2790) and will contribute to other similar initiatives as I am able.
  24. ### ahg-g commented on Apr 11, 2026 
ContributorAuthor
More actions
Thanks all for the discussion. The detailed execution plan is documented at <https://docs.google.com/document/d/17ZVd4poQfLHSBmKSgLrhGGEBhbwQzH4DJes-D4MLYd4/edit?tab=t.0>.
  25. [andyzhangx](https://github.com/andyzhangx)
added 2 commits that reference this issue [on Apr 20, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2430#event-24659267533)
[Migrate default EPP from GWIE to llm-d inference scheduler](https://github.com/andyzhangx/kaito/commit/6e4a3cd605106133c7e2d9b38c8483fd8916ed8c)
[Migrate default EPP from GWIE to llm-d inference scheduler](https://github.com/andyzhangx/kaito/commit/934839d0546250add73cf16b16d55ab7a3153eda)
  26. ### dims commented on Apr 24, 2026 
Member
More actions
Did we reach out to folks who are using it currently? Dynamo is vendoring in the code:
<https://github.com/ai-dynamo/dynamo/tree/main/deploy/inference-gateway/epp>
(and adding 3 plugins)
👍React with 👍1Reacted by Mike Morris
  27. ### ahg-g commented on Apr 24, 2026 
ContributorAuthor
More actions
> Did we reach out to folks who are using it currently? Dynamo is vendoring in the code:
> <https://github.com/ai-dynamo/dynamo/tree/main/deploy/inference-gateway/epp>
> (and adding 3 plugins)
Yes, we did. Please reach out to [@atchernych](https://github.com/atchernych); also please check out [#2842](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2842)
  28. ### ahg-g commented on May 13, 2026 
ContributorAuthor
More actions
The move happened. I will open follow up items to archive the code that moved.
  29. [ahg-g](https://github.com/ahg-g)
closed this as [completed](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)[on May 13, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2430#event-25495938383)
  30. ### mtparet commented on May 26, 2026 
More actions
How can we still use envoy gateway ? Llm-d does not reference it as supportes igw, seems pretty regression if that is the case.


[Sign up for free](https://github.com/signup?return_to=https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2430)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2430)
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
