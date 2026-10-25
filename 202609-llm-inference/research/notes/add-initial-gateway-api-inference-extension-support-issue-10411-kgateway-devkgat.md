---
title: 'Add Initial Gateway API Inference Extension Support · Issue #10411 · kgateway-dev/kgateway'
id: add-initial-gateway-api-inference-extension-support-issue-10411-kgateway-devkgat
tags:
- llmd-router-nonk8s-backends
- kgateway
- gie
- inferencepool
- k8s-only
- envoy
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:44:57.248441Z'
updated: '2026-09-15T04:55:23.743910Z'
source: https://github.com/kgateway-dev/kgateway/issues/10411
source_domain: github.com
fetched_at: '2026-09-11T19:44:57.223110Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Closed kgateway (formerly Gloo Edge/k8sgateway) meta-issue #10411 (Dec 2024
  - Sep 2025) scoping initial GIE support, confirming kgateway''s implementation is
  fully Kubernetes-CRD-driven: it adds controllers to reconcile InferencePool CRs,
  an ext-proc server deployment managed by kgateway''s ''deployer'' package, and translator/proxy_syncer
  logic that turns GIE CRs into Envoy proxy config — auto-enabled only when the InferencePool
  CRDs exist in-cluster. No standalone or non-k8s mode is scoped anywhere in this
  issue. Two details are directly relevant to the header-override/ORIGINAL_DST routing
  question: (1) a struck-through line notes an abandoned plan to ''Implement Override
  Host LB policy'' referencing Envoy PR #38757 and GIE PR #761, explicitly ''no longer
  being pursued since the Envoy-based data plane is being deprecated in v2.1'' — i.e.,
  kgateway considered and dropped an Envoy load-balancer-level override-host mechanism
  for honoring EPP destinations; (2) a tracked follow-up item for ''failureMode''
  support, confirming fail-open/fail-closed configurability was treated as a distinct,
  later-scoped feature rather than baseline behavior. kgateway is moving off its Envoy-based
  data plane in v2.1, which undercuts any recommendation to reuse kgateway''s Envoy
  config approach going forward.'
utility_score: 9.0
---

[Skip to content](https://github.com/kgateway-dev/kgateway/issues/10411#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kgateway-dev/kgateway/issues/10411) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kgateway-dev/kgateway/issues/10411) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kgateway-dev/kgateway/issues/10411) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kgateway-dev/kgateway/issues/10411).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkgateway-dev%2Fkgateway) You must be signed in to change notification settings
  * [ Fork 807 ](https://github.com/login?return_to=%2Fkgateway-dev%2Fkgateway)
  * [ Star  5.7k ](https://github.com/login?return_to=%2Fkgateway-dev%2Fkgateway)


#  Add Initial Gateway API Inference Extension Support #10411
Copy link
Copy link
Closed
Closed
[Add Initial Gateway API Inference Extension Support](https://github.com/kgateway-dev/kgateway/issues/10411#top)#10411
Feature
Copy link
Assignees
Labels
[Area: InferenceActivities related to Gateway API Inference Extension support.](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Area%3A%20Inference%22)Activities related to Gateway API Inference Extension support.[Area: K8S Gateway APIIssues related to the Kubernetes Gateway API](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Area%3A%20K8S%20Gateway%20API%22)Issues related to the Kubernetes Gateway API[PrioritizedIndicating issue prioritized to be worked on in RFE stream](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Prioritized%22)Indicating issue prioritized to be worked on in RFE stream[Priority: HighRequired in next 3 months to make progress, bugs that affect multiple users, or very bad UX](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Priority%3A%20High%22)Required in next 3 months to make progress, bugs that affect multiple users, or very bad UX[Size: XL>2 weeks](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Size%3A%20XL%22)>2 weeks[Type: EnhancementNew feature or request](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Type%3A%20Enhancement%22)New feature or request[scope/kg-2.0](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22scope%2Fkg-2.0%22)
Milestone
## Description
opened [on Dec 19, 2024](https://github.com/kgateway-dev/kgateway/issues/10411#issue-2751302693)
Last edited by danehans
Contributor
Issue body actions
### Gloo Edge Product
Open Source
### Gloo Edge Version
main
### Is your feature request related to a problem? Please describe.
[Gateway API Inference Extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main) (formerly llm-instance-gateway) is a project that originated from [wg-serving](https://github.com/kubernetes/community/tree/master/wg-serving) and is sponsored by [SIG Network](https://github.com/kubernetes/community/blob/master/sig-network/README.md#gateway-api-inference-extension). The project provides APIs, a load balancing algorithm, [ext-proc](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/ext_proc_filter) code, and controllers to support advanced routing of LLM traffic.
### Describe the solution you'd like
Add the following support:
  * Create an enhancement proposal that provides the API design and implementation details: [Adds EP-10411: Gateway API Inference Extension Support #10420](https://github.com/kgateway-dev/kgateway/pull/10420)
~~Update the GatewayClassParameters API to surface user-facing configuration for supported inference extensions:[Adds InferenceExtension to GatewayParameters #10601](https://github.com/kgateway-dev/kgateway/pull/10601)~~. ATM not needed since the feature is auto-enabled if the inference extension CRDs are present in the cluster.
~~Update the[configuration](https://github.com/k8sgateway/k8sgateway/blob/main/install/helm/gloo/generate/values.go) API to enable/disable the gateway-api-inference-extension feature~~. ATM not needed since the feature is auto-enabled if the inference extension CRDs are present in the cluster.
~~Update Helm charts to install k8sgateway with the gateway-api-inference-extension feature based on the provided configuration~~. ATM not needed since the feature is auto-enabled if the inference extension CRDs are present in the cluster.
Add gateway-api-inference-extension as a supported [extension](https://github.com/k8sgateway/k8sgateway/tree/main/projects/gateway2/extensions2).
Add controllers that reconcile gateway-api-inference-extension custom resources, e.g. InferencePool. The controller should be optional, i.e. only run if the configuration option is enabled and the gateway-api-inference-extension CRDs exist.
Update RBAC rules to allow gateway-api-inference-extension controllers to get, list, watch, etc. gateway-api-inference-extension custom resources.
Update the [deployer](https://github.com/k8sgateway/k8sgateway/tree/main/projects/gateway2/deployer) pkg to manage the required gateway-api-inference-extension resources, e.g. Deployment, to run the [ext-proc](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/ext_proc_filter) server.
Add InferencePool as a supported HTTPRoute backend reference.
Update the translator pkg to translate HTTPRoutes referencing an InferencePool resource.
Update the [proxy_syncer](https://github.com/k8sgateway/k8sgateway/tree/main/projects/gateway2/proxy_syncer) pkg to translate gateway-api-inference-extension CRs into Gloo Proxies and sync the proxy client with the newly translated proxies.
Update the [reporter](https://github.com/k8sgateway/k8sgateway/tree/main/projects/gateway2/reports) pkg to support reporting gateway-api-inference-extension CRD status.
Add initial e2e tests for this feature.
Update CI to run e2e tests.
Add initial user docs: [InferenceExtension in kgateway kgateway.dev#70](https://github.com/kgateway-dev/kgateway.dev/issues/70). Owner: [@artberger](https://github.com/artberger).
Add `failureMode` support as a follow-up to [Add Initial Gateway API Inference Extension Support #10411](https://github.com/kgateway-dev/kgateway/issues/10411).
Update the deployer to support an HTTPRoute switching between Service and InferencePool backendRefs ([xref](https://github.com/kgateway-dev/kgateway/pull/10684#discussion_r1992121372)).
Improve EPP RBAC based on [EPP: Use Dedicated Service Account kubernetes-sigs/gateway-api-inference-extension#224](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/224). Either change ClusterRole and ClusterRoleBinding turn into Role and RoleBinding or the first EPP creates a common CR/CRB if it does not exist, additional InferencePools add their ServiceAccount to the `subjects` in the common ClusterRoleBinding and remove their entry upon InferencePool deletion. This additional complexity may not be worth the benefit of having a common CR/CRB for all EPPs.
Track [Extension Auto-Provisioning kubernetes-sigs/gateway-api-inference-extension#507](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/507) for the status of auto-provisioning InferencePool infra and adjust the deployer accordingly.
For multiple backends on one route, investigate using `RouteAction_WeightedClusters` which may use the current `ExtProcPerRoute` approach or put the ExtProc as an upstream Cluster filter. [(xref)](https://github.com/kgateway-dev/kgateway/pull/10684#discussion_r1992124021).
Investigate using the standard EDS cluster created for the ext-proc service instead of creating a separate one [(xref)](https://github.com/kgateway-dev/kgateway/pull/10684#discussion_r1992132327). Consider changing the model from endpoint picker per upstream, to per GW.
~~Investigate whether or not to remove finalizer from inferencepool controller.~~ Being tracked by [Deployer: Evaluate Inference Extension Support #10878](https://github.com/kgateway-dev/kgateway/issues/10878).
The `usedPools` field of `endpointPickerPass` should be `map[string]map[types.NamespacedName]*ir.InferencePool` to support per-filter-chain ([xref](https://github.com/kgateway-dev/kgateway/pull/10684#discussion_r1994185281)).
~~Implement Override Host LB policy ([Envoy PR](https://github.com/envoyproxy/envoy/pull/38757) and [Infer Ext PR](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761))~~. No longer being pursued since the Envoy-based data plane is being deprecated in v2.1.
~~Run[benchmarks](https://gateway-api-inference-extension.sigs.k8s.io/performance/benchmark/) and publish results~~. Tracked by [Inference: Publish Benchmark Tests #12289](https://github.com/kgateway-dev/kgateway/issues/12289)
To pick up a draggable item, press the space bar. While dragging, use the arrow keys to move the item. Press space again to drop the item in its new position, or press escape to cancel. 


### Describe alternatives you've considered
Do not support the [Gateway API Inference Extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main) project.
### Additional Context
_No response_
👍React with 👍1Reacted by Rob Scott
🚀React with 🚀1Reacted by Zhengke Zhou
## Activity
[danehans](https://github.com/danehans)
added 
[Type: EnhancementNew feature or request](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Type%3A%20Enhancement%22)New feature or request
[on Dec 19, 2024](https://github.com/kgateway-dev/kgateway/issues/10411#event-15724657583)
[danehans](https://github.com/danehans)
self-assigned this
[on Dec 19, 2024](https://github.com/kgateway-dev/kgateway/issues/10411#event-15724657991)
[danehans](https://github.com/danehans)
added 
[Area: K8S Gateway APIIssues related to the Kubernetes Gateway API](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Area%3A%20K8S%20Gateway%20API%22)Issues related to the Kubernetes Gateway API
[on Dec 19, 2024](https://github.com/kgateway-dev/kgateway/issues/10411#event-15724659791)
[tiberiuac](https://github.com/tiberiuac)
added a commit that references this issue [on Dec 30, 2024](https://github.com/kgateway-dev/kgateway/issues/10411#event-15785101664)
[bugfix: propagate eds changes when annotations change. ([kgateway-dev#…](https://github.com/kgateway-dev/kgateway/issues/10411)](https://github.com/openet/gloo/commit/da0f35ac2125cdbdc51a774319e3ed33a24b9233)
[danehans](https://github.com/danehans)
added 
[Size: XL>2 weeks](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Size%3A%20XL%22)>2 weeks
[PrioritizedIndicating issue prioritized to be worked on in RFE stream](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Prioritized%22)Indicating issue prioritized to be worked on in RFE stream
[Priority: HighRequired in next 3 months to make progress, bugs that affect multiple users, or very bad UX](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Priority%3A%20High%22)Required in next 3 months to make progress, bugs that affect multiple users, or very bad UX
[on Jan 2, 2025](https://github.com/kgateway-dev/kgateway/issues/10411#event-15802639569)
### danehans commented on Jan 8, 2025 
ContributorAuthor
More actions
cc [@yuval-k](https://github.com/yuval-k) [@EItanya](https://github.com/EItanya)
### yuval-k commented on Jan 13, 2025 
Contributor
More actions
[@danehans](https://github.com/danehans) high level looks good; but the above task list is somewhat out of date. As the k8s gw internal architecture has changed to allow opaque extensions without the need to modify the translator.
👍React with 👍1Reacted by Daneyon Hansen
### danehans commented on Jan 24, 2025 
ContributorAuthor
More actions
xref <https://github.com/solo-io/gloo-gateway/issues/1084> as a use case for this issue, specifically supporting Endpoint Picker, the reference inference extension implementation.
### danehans commented on Feb 12, 2025 
ContributorAuthor
More actions
xref [#10614](https://github.com/kgateway-dev/kgateway/issues/10614) required for setting custom proxy connections settings- timeouts, circuit breakers, etc.
[lgadban](https://github.com/lgadban)
added this to [on Feb 24, 2025](https://github.com/kgateway-dev/kgateway/issues/10411#event-16433833399)
[github-project-automation](https://github.com/apps/github-project-automation)
moved this to Backlog in [on Feb 24, 2025](https://github.com/kgateway-dev/kgateway/issues/10411)
[lgadban](https://github.com/lgadban)
added this to the [kgateway 2.0.0 release](https://github.com/kgateway-dev/kgateway/milestone/57) milestone [on Feb 24, 2025](https://github.com/kgateway-dev/kgateway/issues/10411#event-16433834197)
[lgadban](https://github.com/lgadban)
removed this from [on Feb 24, 2025](https://github.com/kgateway-dev/kgateway/issues/10411)
[lgadban](https://github.com/lgadban)
removed this from the [kgateway 2.0.0 release](https://github.com/kgateway-dev/kgateway/milestone/57) milestone [on Apr 1, 2025](https://github.com/kgateway-dev/kgateway/issues/10411#event-17085848587)
[danehans](https://github.com/danehans)
added this to the [v2.1.0](https://github.com/kgateway-dev/kgateway/milestone/58) milestone [on Apr 15, 2025](https://github.com/kgateway-dev/kgateway/issues/10411#event-17275372524)
[danehans](https://github.com/danehans)
added the
issue type [on Apr 15, 2025](https://github.com/kgateway-dev/kgateway/issues/10411#event-17275374275)
[DuncanDoyle](https://github.com/DuncanDoyle)
added and removed [on May 9, 2025](https://github.com/kgateway-dev/kgateway/issues/10411#event-17591567594)
[danehans](https://github.com/danehans)
added 
[Area: InferenceActivities related to Gateway API Inference Extension support.](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Area%3A%20Inference%22)Activities related to Gateway API Inference Extension support.
[on Jul 18, 2025](https://github.com/kgateway-dev/kgateway/issues/10411#event-18704426200)
### danehans commented on Sep 16, 2025 
ContributorAuthor
More actions
Closing since all the initial features are either complete or have separate tracking issues.
[danehans](https://github.com/danehans)
closed this as [completed](https://github.com/kgateway-dev/kgateway/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)[on Sep 16, 2025](https://github.com/kgateway-dev/kgateway/issues/10411#event-19731366280)
[Sign up for free](https://github.com/signup?return_to=https://github.com/kgateway-dev/kgateway/issues/10411)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/kgateway-dev/kgateway/issues/10411)
## Metadata
## Metadata
### Assignees
  * [danehans](https://github.com/danehans)


### Labels
[Area: InferenceActivities related to Gateway API Inference Extension support.](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Area%3A%20Inference%22)Activities related to Gateway API Inference Extension support.[Area: K8S Gateway APIIssues related to the Kubernetes Gateway API](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Area%3A%20K8S%20Gateway%20API%22)Issues related to the Kubernetes Gateway API[PrioritizedIndicating issue prioritized to be worked on in RFE stream](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Prioritized%22)Indicating issue prioritized to be worked on in RFE stream[Priority: HighRequired in next 3 months to make progress, bugs that affect multiple users, or very bad UX](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Priority%3A%20High%22)Required in next 3 months to make progress, bugs that affect multiple users, or very bad UX[Size: XL>2 weeks](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Size%3A%20XL%22)>2 weeks[Type: EnhancementNew feature or request](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Type%3A%20Enhancement%22)New feature or request[scope/kg-2.0](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22scope%2Fkg-2.0%22)
### Projects
No projects
### Milestone
  * [ v2.1.0Closed Oct 27, 2025, 100% complete ](https://github.com/kgateway-dev/kgateway/milestone/58)


### Relationships
None yet
### Development
No branches or pull requests
### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
