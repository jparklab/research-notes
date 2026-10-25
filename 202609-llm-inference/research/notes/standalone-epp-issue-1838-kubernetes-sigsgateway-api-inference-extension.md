---
title: 'Standalone EPP · Issue #1838 · kubernetes-sigs/gateway-api-inference-extension'
id: standalone-epp-issue-1838-kubernetes-sigsgateway-api-inference-extension
tags:
- llmd-router-nonk8s-backends-54b774
- standalone-epp
- gie-design
- deployment-modes
- service-discovery
created: '2026-09-11T20:49:09.383040Z'
updated: '2026-09-15T04:55:23.846934Z'
source: https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1838
source_domain: github.com
fetched_at: '2026-09-11T20:49:09.359751Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GIE issue #1838 ''Standalone EPP'' (opened by ahg-g, a GIE maintainer, Nov
  7 2025; closed completed Mar 4 2026) is the design-rationale thread for why standalone
  (non-InferencePool, non-Gateway-API-provisioned) EPP deployment exists. ahg-g frames
  it as a maturation ladder: ''The idea is to offer deployment modes that allow the
  least amount of friction for users as they mature in the journey from simple deployments
  to full fledged ones that integrate with Gateway API. The end goal is to land users
  on k8s and the gateway api, but many users are not there yet.'' He enumerates four
  deployment modes: (1) orchestrator-agnostic static endpoint config with ZERO k8s
  dependency (no reconcilers started at all); (2.1) k8s-native, no Gateway API/CRD
  dependency, endpoints found via pod-selector+port flags; (2.2) k8s-native, extension
  CRDs used for richer EPP features but proxy statically configured (typically sidecar);
  (2.3) full Gateway-API-provisioned mode. Corresponding to this he identifies exactly
  three discovery mechanisms EPP should support: 1) InferencePool (reconciler-driven),
  2) podSelector, 3) static set of endpoints -- each as its own pluggable ''discovery
  plugin.'' Contributor nirrozenbaum initially argued modes (1) and (2.1) were unnecessary
  and ''awkward to develop... under kubernetes-sigs and say it''s going to be with
  zero dependency on k8s,'' but ahg-g countered with concrete demand: ''We hear this
  from batch inference and RL customers. In fact, Mistral asked for those two deployment
  modes independently.'' ahg-g explicitly rejects requiring any control-plane dependency
  for the static/orchestrator-agnostic mode -- ''I don''t think we need to offer anything
  beyond a static set of endpoints because otherwise we need to build or create a
  dependency on a control plane, which goes against the main motivation behind this
  mode'' -- and cites sgl-project/sglang''s sgl-router prefill/decode disaggregation
  static-endpoint config as the model to follow. Contributor elevran proposed generalizing
  service discovery further to include DNS resolvers and etcd/Consul/cloud-service
  backends, listed as options for mode (1). This thread is the direct motivating-use-case
  evidence for why standalone EPP (and thus non-pod backend support) exists: friction
  reduction for non-k8s-native batch-inference and RL workloads, not a rejection of
  k8s but a staged on-ramp.'
utility_score: 15.0
---

[Skip to content](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1838#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1838) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1838) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1838) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1838).
/ **[gateway-api-inference-extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension) You must be signed in to change notification settings
  * [ Fork 313 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)
  * [ Star  766 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)


#  Standalone EPP
Copy link
Copy link
Closed
Closed
Copy link
Labels
[triage/acceptedIndicates an issue or PR is ready to be actively worked on.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22triage%2Faccepted%22)Indicates an issue or PR is ready to be actively worked on.
## Description
opened [on Nov 7, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1838#issue-3602383483)
Last edited by ahg-g
Contributor
Issue body actions
The idea is to offer deployment modes that allow the least amount of friction for users as they mature in the journey from simple deployments to full fledged ones that integrate with Gateway API.
The end goal is to land users on k8s and the gateway api, but many users are not there yet and find it operationally unjustified to start with the full fledged solution. A strategy where we allow users to start simple with a clear maturation path towards landing them on k8s + gateway api is a win for all.
I see the following useful deployment modes:
  1. Orchestrator agnostic deployment mode: static configuration of endpoints with zero dependency on k8s as a control plane, which means the reconcilers are not started as well.
  2. k8s-native deployment modes 2.1) No dependency on Gateway API or extension CRDs, endpoints are discovered using a pod selector and port flags 2.2) No dependency Gateway APIs, extension CRDs are used to enable a richer feature set for EPP, but the proxy is statically configured (typically as a sidecar) 2.3) Full fledged deployment mode where the proxy is provisioned via the Gateway API.


To enable those deployment modes, the EPP needs to offer flags to be tuned to enable the above modes of operation.
I tried to capture all the above in the [Standalone EPP](https://docs.google.com/document/d/1CXjqmxC3te5u3jOgGH3aaVJQDJFqQ3ZKPJ8Mm6JeY4Y/edit?tab=t.0) proposal which I presented in one of the community meetings as well, and I am happy to convert that to proposal [docs/proposals](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals) if we believe this is necessary.
## Activity
[ahg-g](https://github.com/ahg-g)
added 
[needs-triageIndicates an issue or PR lacks a `triage/foo` label and requires one.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a `triage/foo` label and requires one.
[on Nov 7, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1838#event-20800737001)
### nirrozenbaum commented on Nov 9, 2025 
Last edited by nirrozenbaum
Contributor
More actions
continuing the discussion started on [#1779](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1779) - I haven't seen any requirement to deployment modes (1) or (2.1) and I also don't think that is a correct path to pursue. On the other hand, I think having optionally (2.2) and (2.3) might be very beneficial and could fit in some use cases.
regarding (1): that's a bit awkward to develop a project under kubernetes-sigs and say it's going to be with zero dependency on k8s as control plane. I don't think this audience (people who want zero dependency on k8s) are going to use this project.
regarding (2.1): if we already use a k8s-native deployment mode, there's no reason not to use the reconcilers and the extension CRDs. users users of EPP don't need to understand the reconcilers, whoever runs things in k8s should have the basic understanding of specifying pod selector and ports in CRD.
### elevran commented on Nov 11, 2025 
Contributor
More actions
[@ahg-g](https://github.com/ahg-g) the idea make sense. Some clarifications and a proposal.
>   1. Orchestrator agnostic deployment mode: static configuration of endpoints with zero dependency on k8s as a control plane, which means the reconcilers are not started as well.
> 

This mode should work in the absence of k8s altogether? If so, defining a `Selector` for `Pod` discovery in [#1779](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1779) seems misaligned and is likely addressing modes 2.x below? Asking since there's linkage between [#1833](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1833) and this issue.
>   1. k8s-native deployment modes 2.1) No dependency on Gateway API or extension CRDs, endpoints are discovered using a pod selector and port flags 2.2) No dependency Gateway APIs, extension CRDs are used to enable a richer feature set for EPP, but the proxy is statically configured (typically as a sidecar) 2.3) Full fledged deployment mode where the proxy is provisioned via the Gateway API.
> 

I see the value in this progression once someone settles on k8s, with incremental adoption of IGW features. Not entirely clear what IGW features mode 2.2 uses (e.g., seems InferencePool is in scope already?). 2.1 make sense but perhaps we can reposition it in the context of more generalized service discovery?
[#1833](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1833) can perhaps be considered it in the context of Service discovery:
  * For mode (1) one might use a fix list of IP's and ports, or an external DNS resolver, or etcd/Consul/cloud service.
  * For 2.1 one could use a Service object, and EndpointSlice, a DNS resover as in 1, or a Selector as suggested.


Each mechanism would be it's own discovery plugin and would make EPP adaptable to more use cases. Ultimately, these service discovery mechanisms could amount to making data store calls to add Pod endpoints or InferencePool. They do not necessarily have to come from watchers/reconcilers.
### ahg-g commented on Nov 12, 2025 
ContributorAuthor
More actions
> I haven't seen any requirement to deployment modes (1) or (2.1)
We hear this from batch inference and RL customers. In fact, Mistral asked for those two deployment modes independently without knowing that we are already pursuing them.
> that's a bit awkward to develop a project under kubernetes-sigs and say it's going to be with zero dependency on k8s as control plane. I don't think this audience (people who want zero dependency on k8s) are going to use this project.
It is not awkward, there are a few examples like that that where mentioned by the community during KubeCon. But the main point that we need to consider is thinking strategically on how we expand our adoption. A lot of customers are not running natively on k8s for various good reasons, to ensure that those eventually land on k8s, we need to offer them a path; meeting customers where they are and offering a clear maturation path into k8s is the strategy I am proposing here, and not be dogmatic about k8s and say to users "meet us where we are, we will not come to you".
> if we already use a k8s-native deployment mode, there's no reason not to use the reconcilers and the extension CRDs. users users of EPP don't need to understand the reconcilers, whoever runs things in k8s should have the basic understanding of specifying pod selector and ports in CRD.
Every unnecessary added step for adoption is a friction point that hinders adoption. Why do a batch inference job need to care about InferencePool if it does not require an api gateway provisioned LB or any of the features that might be enabled by it in the future?
### nirrozenbaum commented on Nov 12, 2025 
Contributor
More actions
to be honest it sounds from the description like we should break GIE to smaller building blocks and allow composition of those building blocks in more than one way.
if we go that path, I think it might makes sense.
e.g., add a discoery layer that “discovers” the model server ip:port endpoints. and then we could use a fixed set or reconciler with InferencePool. as long as we keep the absraction we should be fine.
so instead of proposing several deployment modes, IMO a cleaner approach would be to allow custom logic, could be kinda similar to scheduler pluggability
### ahg-g commented on Nov 12, 2025 
ContributorAuthor
More actions
> This mode should work in the absence of k8s altogether? If so, defining a Selector for Pod discovery in [#1779](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1779) seems misaligned and is likely addressing modes 2.x below? Asking since there's linkage between [#1833](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1833) and this issue.
The podSelector is for mode 2.1; once the podSelector based approach is available, we can easily follow up with another PR to mode 1;
> Not entirely clear what IGW features mode 2.2 uses (e.g., seems InferencePool is in scope already?).
InfObjective/InfModelRewrite, any possible configurations that we may add to infPool in the future that are consumed by EPP.
> 2.1 make sense but perhaps we can reposition it in the context of more generalized service discovery?
I will share another doc on that possibility in the context of Ray + an inference scheduling library deployed within Ray; but for EPP as a container, I don't think we need to offer anything beyond a static set of endpoints because otherwise we need to build or create a dependency on a control plane, which goes against the main motivation behind this mode. See sglang-router as an example of how static endpoints are set (<https://github.com/sgl-project/sglang/tree/main/sgl-router#prefilldecode-disaggregation-pd>)
> [#1833](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1833) can perhaps be considered it in the context of Service discovery:
Yes, exactly; there are three discovery modes I had in mind: 1) InfPool 2) podSelector 3) static set
👍React with 👍1Reacted by Etai Lev Ran
### ahg-g commented on Nov 12, 2025 
ContributorAuthor
More actions
> to be honest it sounds from the description like we should break GIE to smaller building blocks and allow composition of those building blocks in more than one way.
> if we go that path, I think it might makes sense.
> e.g., add a discoery layer that “discovers” the model server ip:port endpoints. and then we could use a fixed set or reconciler with InferencePool. as long as we keep the absraction we should be fine.
> so instead of proposing several deployment modes, IMO a cleaner approach would be to allow custom logic, could be kinda similar to scheduler pluggability
We need to be first anchored on the concrete use cases or CUJs, the deployment modes is that and is the language we would use to communicate to users what we are offering; pluggable or fixed set of options is an implementation detail.
### nirrozenbaum commented on Nov 12, 2025 
Contributor
More actions
> We need to be first anchored on the concrete use cases or CUJs, the deployment modes is that and is the language we would use to communicate to users what we are offering; pluggable or fixed set of options is an implementation detail.
fair point.
[ahg-g](https://github.com/ahg-g)
added 
[triage/acceptedIndicates an issue or PR is ready to be actively worked on.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22triage%2Faccepted%22)Indicates an issue or PR is ready to be actively worked on.
and removed 
[needs-triageIndicates an issue or PR lacks a `triage/foo` label and requires one.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a `triage/foo` label and requires one.
[on Nov 13, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1838#event-20921736557)
[ahg-g](https://github.com/ahg-g)
closed this as [completed](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)[on Mar 4, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1838#event-23274503974)
[Sign up for free](https://github.com/signup?return_to=https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1838)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1838)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
[triage/acceptedIndicates an issue or PR is ready to be actively worked on.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22triage%2Faccepted%22)Indicates an issue or PR is ready to be actively worked on.
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
