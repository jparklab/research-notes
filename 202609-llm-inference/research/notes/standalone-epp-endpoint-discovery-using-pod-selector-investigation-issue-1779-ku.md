---
title: 'Standalone EPP - Endpoint Discovery Using Pod Selector Investigation · Issue
  #1779 · kubernetes-sigs/gateway-api-inference-extension'
id: standalone-epp-endpoint-discovery-using-pod-selector-investigation-issue-1779-ku
tags:
- llmd-router-nonk8s-backends-54b774
- standalone-epp
- pod-selector
- gie-design
- rl-batch-use-case
created: '2026-09-11T21:06:28.860930Z'
updated: '2026-09-15T04:55:23.915065Z'
source: https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1779
source_domain: github.com
fetched_at: '2026-09-11T21:06:28.840604Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GIE issue #1779 (capri-xiyue, opened Oct 28 2025) tracks investigation of
  ''Endpoint Discovery with Pod Selector'' for standalone EPP, referencing the same
  ''EPP As a Standalone Request Scheduler'' design doc cited in issue #1838. Motivation,
  stated verbatim: ''The endpoint picker (EPP)... is currently deployed as an ext-proc
  service tightly coupled to the Kubernetes Gateway API, a setup that adds unnecessary
  operational overhead for non-serving workloads like Reinforcement Learning (RL)
  post-training. The proof of concept for a "Standalone EPP" aims to solve this by
  creating a decoupled deployment mode... packaging the EPP and an Envoy proxy as
  a single sidecar-based unit, bypassing the Gateway API and instead using a simple
  command-line pod selector to discover and manage model server endpoints, thereby
  lowering the barrier for adopting the EPP''s advanced scheduling logic for critical
  RL and batch inference jobs.'' A sharp maintainer pushback from nirrozenbaum clarifies
  an important scope distinction that matters for router customization: ''this issue
  is not about bypassing Gateway API, it''s about bypassing InferencePool CRD which
  is one of GIE main building blocks... specifying pod labels selector as cmd-line
  is against k8s principles and is not matching how GIE works today where the selection
  label can [be] changed dynamically in runtime. How do you handle conflicts if one
  specifies the cmd-line arg and uses the crd? ... why do we need this? standalone
  epp without ext proc can work with InferencePool CRD.'' Author capri-xiyue''s response:
  the EPP would validate that only one discovery source (cmd-line pod-selector or
  InferencePool CRD) is active at a time, avoiding conflicts. This exchange is direct
  evidence of an unresolved tension in the community between (a) fully declarative,
  CRD-driven discovery (InferencePool, dynamically reconciled) and (b) simpler, static/CLI-configured
  discovery (pod-selector or file-based) aimed at lowering the barrier for RL/batch
  users -- the same tension documented independently in the FileDiscovery-plugin work
  in llm-d-router.'
---

*Suggested by [[standalone-epp-issue-1838-kubernetes-sigsgateway-api-inference-extension]] — podSelector discovery mode proposal referenced as linked to standalone EPP design*

[Skip to content](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1779#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1779) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1779) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1779) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1779).
/ **[gateway-api-inference-extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension) You must be signed in to change notification settings
  * [ Fork 313 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)
  * [ Star  766 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)


#  Standalone EPP - Endpoint Discovery Using Pod Selector Investigation
Copy link
Copy link
Closed
Closed
[Standalone EPP - Endpoint Discovery Using Pod Selector Investigation](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1779#top)#1779
Copy link
Assignees
Labels
[kind/featureCategorizes issue or PR as related to a new feature.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22kind%2Ffeature%22)Categorizes issue or PR as related to a new feature.[triage/acceptedIndicates an issue or PR is ready to be actively worked on.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22triage%2Faccepted%22)Indicates an issue or PR is ready to be actively worked on.
## Description
opened [on Oct 28, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1779#issue-3563470205)
Contributor
Issue body actions
**What would you like to be added** : [EPP As a Standalone Request Scheduler](https://docs.google.com/document/d/1CXjqmxC3te5u3jOgGH3aaVJQDJFqQ3ZKPJ8Mm6JeY4Y/edit?usp=sharing) mentioned using `Endpoint Discovery with Pod Selector` . This issue is to track the investigation of endpoint discovery with pod selector.
**Why is this needed** : The endpoint picker (EPP), an intelligent scheduler for LLM inference requests, is currently deployed as an ext-proc service tightly coupled to the Kubernetes Gateway API, a setup that adds unnecessary operational overhead for non-serving workloads like Reinforcement Learning (RL) post-training. The proof of concept for a "Standalone EPP" aims to solve this by creating a decoupled deployment mode. This new approach will be tested by packaging the EPP and an Envoy proxy as a single sidecar-based unit, bypassing the Gateway API and instead using a simple command-line pod selector to discover and manage model server endpoints, thereby lowering the barrier for adopting the EPP's advanced scheduling logic for critical RL and batch inference jobs.
## Activity
[capri-xiyue](https://github.com/capri-xiyue)
added 
[needs-triageIndicates an issue or PR lacks a `triage/foo` label and requires one.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a `triage/foo` label and requires one.
[on Oct 28, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1779#event-20565113646)
### capri-xiyue commented on Oct 29, 2025 
ContributorAuthor
More actions
/assign [@capri-xiyue](https://github.com/capri-xiyue)
[k8s-ci-robot](https://github.com/k8s-ci-robot)
assigned [on Oct 29, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1779#event-20586209168)
[ahg-g](https://github.com/ahg-g)
added 
[kind/featureCategorizes issue or PR as related to a new feature.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22kind%2Ffeature%22)Categorizes issue or PR as related to a new feature.
[triage/acceptedIndicates an issue or PR is ready to be actively worked on.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22triage%2Faccepted%22)Indicates an issue or PR is ready to be actively worked on.
and removed 
[needs-triageIndicates an issue or PR lacks a `triage/foo` label and requires one.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a `triage/foo` label and requires one.
[on Oct 29, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1779#event-20587926091)
### nirrozenbaum commented on Nov 7, 2025 
Last edited by nirrozenbaum
Contributor
More actions
[@ahg-g](https://github.com/ahg-g) I’m not sure I understand the motivation behind this issue and would be happy to understand. this issue is not about bypassing Gateway API, it’s about bypassing InferencePool CRD which is one of GIE main building blocks.
specifying pod labels selector as cmd-line is against k8s principles and is not matching how GIE works today where the selection label can changed dynamically in runtime. how do you handle conflicts if one species the cmd-line arg and uses the crd? (not even mentioning the multi port that was ignored).
why do we need this? standalone epp without ext proc can work with InferencePool CRD
### capri-xiyue commented on Nov 7, 2025 
Last edited by capri-xiyue
ContributorAuthor
More actions
Re - "For conflicts if one species the cmd-line arg and uses the crd" The EPP will have validation that either use inferencepool or use pod label selector as cmd-line. Users can't specify both. When users specifies the pod label selector via cmd-line agrs and create a inferencepool, as such inferencepool is not specified in EPP, I think users won't hit any issues. Just an unused inferencepool resources lives in the cluster?
### ahg-g commented on Nov 7, 2025 
Contributor
More actions
[@nirrozenbaum](https://github.com/nirrozenbaum) I tried to capture the feature request in [#1838](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1838); do you mind moving the discussion there since the answer to your question requires alignment on a higher level proposed strategy.
### capri-xiyue commented on Nov 20, 2025 
Last edited by capri-xiyue
ContributorAuthor
More actions
The POC of the feature has been merged in [#1833](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1833) (I've manually tested it works e2e). To avoid regression failure, I will add UT and e2e tests later. But before the UT and e2e tests in place, the feature may encounter regression failure.
[capri-xiyue](https://github.com/capri-xiyue)
closed this as [completed](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)[on Feb 3, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1779#event-22511211544)
[Sign up for free](https://github.com/signup?return_to=https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1779)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1779)
## Metadata
## Metadata
### Assignees
  * [capri-xiyue](https://github.com/capri-xiyue)


### Labels
[kind/featureCategorizes issue or PR as related to a new feature.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22kind%2Ffeature%22)Categorizes issue or PR as related to a new feature.[triage/acceptedIndicates an issue or PR is ready to be actively worked on.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22triage%2Faccepted%22)Indicates an issue or PR is ready to be actively worked on.
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
