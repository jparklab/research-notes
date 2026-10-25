---
title: 'Bring back Kubernetes Service support in AIServiceBackend''s ref · Issue #902
  · theagentrouter/agent-router'
id: bring-back-kubernetes-service-support-in-aiservicebackends-ref-issue-902-theagen
tags:
- llmd-router-nonk8s-backends-54b774
- envoy-ai-gateway
- non-k8s-backend
- envoy-gateway-eds
created: '2026-09-11T20:47:45.712999Z'
updated: '2026-09-15T04:55:23.822326Z'
source: https://github.com/envoyproxy/ai-gateway/issues/902
source_domain: github.com
fetched_at: '2026-09-11T20:47:45.690521Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub issue #902 (theagentrouter/agent-router, formerly envoyproxy/ai-gateway,
  opened Jul 17 2025, still open/past-due as of Jul 2026) reveals that plain Kubernetes
  Service support in AIServiceBackend''s BackendRef was not merely unimplemented but
  ''temporarily disabled'' -- it existed and was pulled. Maintainer mathetake''s investigation
  (Jul 24 2025) found the blocker is structural, not superficial: Envoy Gateway propagates
  Service backends via EDS (Endpoint Discovery Service) separately from CDS, so the
  ai-gateway''s own extproc server (internal/extproc/server.go) never receives the
  individual endpoint list for a Service-typed backend -- only the aggregate BackendRef
  identity via ''xds.upstream_host_locality_metadata'' (added in envoyproxy/gateway#5875
  and envoyproxy/envoy#39604), which carries name/namespace/kind/port but not the
  per-rule backend-reference index needed to support per-backendRef priority/modelNameOverride.
  Fix was deferred first to v0.4.0 then v0.5.0 then v0.6.0 (past due by Apr 25, 2026),
  pending EG v1.5 shipping endpoint-list support directly to the extension server
  so ai-gateway can build its own metadata instead of relying on EG''s locality metadata.
  Interim mitigation landed as PR #1328 ''enforce the limitation on k8s service at
  CRD'' (Oct 2025) -- i.e. the CRD now explicitly rejects/validates against plain
  Service refs rather than silently mis-routing. This is direct evidence that even
  the REFERENCE two-tier gateway implementation cannot cleanly front a generic (non-InferencePool,
  non-Envoy-Gateway-Backend) backend today; the constraint originates in Envoy Gateway''s
  control-plane data model, not in ai-gateway''s own code, which matters for anyone
  building a custom router against a non-k8s backend outside this stack entirely.'
---

*Suggested by [[api-reference-agent-router]] — cited in AIServiceBackend.BackendRef field doc as the tracking issue for adding k8s Service support (non-Envoy-Gateway-Backend backends)*

[Skip to content](https://github.com/theagentrouter/agent-router/issues/902#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/theagentrouter/agent-router/issues/902) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/theagentrouter/agent-router/issues/902) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/theagentrouter/agent-router/issues/902) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/theagentrouter/agent-router/issues/902).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Ftheagentrouter%2Fagent-router) You must be signed in to change notification settings
  * [ Fork 372 ](https://github.com/login?return_to=%2Ftheagentrouter%2Fagent-router)
  * [ Star  2k ](https://github.com/login?return_to=%2Ftheagentrouter%2Fagent-router)


#  Bring back Kubernetes Service support in AIServiceBackend's ref
Copy link
Copy link
[Bring back Kubernetes Service support in AIServiceBackend's ref](https://github.com/theagentrouter/agent-router/issues/902#top)#902
Task
Copy link
Labels
[area/routingRouting, fallback, resilience, InferencePool integration](https://github.com/theagentrouter/agent-router/issues?q=state%3Aopen%20label%3A%22area%2Frouting%22)Routing, fallback, resilience, InferencePool integration[enhancementNew feature or request](https://github.com/theagentrouter/agent-router/issues?q=state%3Aopen%20label%3A%22enhancement%22)New feature or request
Milestone
## Description
opened [on Jul 17, 2025](https://github.com/theagentrouter/agent-router/issues/902#issue-3240883458)
Contributor
Issue body actions
Kubernetes Service was temporarily disabled as a BackendRef in AIserviceBackend. We can bring it back by support of the EG native endpoint metadata ([envoyproxy/gateway#5875](https://github.com/envoyproxy/gateway/pull/5875)) via " xds.upstream_host_locality_metadata" [envoyproxy/envoy#39604](https://github.com/envoyproxy/envoy/pull/39604) in setBackend function: <https://github.com/envoyproxy/ai-gateway/blob/18379e87ed56e3fd9ad9124df61154a20e46d7dc/internal/extproc/server.go#L281>
## Activity
[mathetake](https://github.com/mathetake)
added 
[enhancementNew feature or request](https://github.com/theagentrouter/agent-router/issues?q=state%3Aopen%20label%3A%22enhancement%22)New feature or request
[on Jul 17, 2025](https://github.com/theagentrouter/agent-router/issues/902#event-18687792989)
[mathetake](https://github.com/mathetake)
added this to the [v0.3.0](https://github.com/theagentrouter/agent-router/milestone/3) milestone [on Jul 17, 2025](https://github.com/theagentrouter/agent-router/issues/902#event-18687805458)
### mathetake commented on Jul 24, 2025 
Last edited by mathetake
ContributorAuthor
More actions
After spending a few hours, this seems like much more nuanced than I thought. Currently, upstream_host_locality_metadata only carries the BackendRef's name (+namespace, kind, port) [envoyproxy/gateway#5875](https://github.com/envoyproxy/gateway/pull/5875) but that doesn't include the exact index of the backend reference in a specific rule. That makes sense because EG doesn't have any configuration knob on the backend reference vs us having priorities and modelNameOverride. Even if that index info is available in the metadata, we still cannot support the per-backendRef level priority as the endpoints are not passed to extension server (note that with Service, EG distributes endpoints via EDS, not together with CDS.).
So, i think I would defer this to v0.4.0 with the endpoints support in the extension server after EG v1.5. With that extension server support, we won't need to rely on upstream_host_locality_metadata but just we can construct our own metadata just like the current implementation details for EG's Backend support.
cc [@arkodg](https://github.com/arkodg)
[mathetake](https://github.com/mathetake)
modified the milestones: [v0.3.0](https://github.com/theagentrouter/agent-router/milestone/3), [v0.4.0](https://github.com/theagentrouter/agent-router/milestone/4) [on Jul 24, 2025](https://github.com/theagentrouter/agent-router/issues/902#event-18801958739)
[mathetake](https://github.com/mathetake)
modified the milestones: [v0.4.0](https://github.com/theagentrouter/agent-router/milestone/4), [v0.5.0](https://github.com/theagentrouter/agent-router/milestone/5) [on Oct 9, 2025](https://github.com/theagentrouter/agent-router/issues/902#event-20188104105)
[mathetake](https://github.com/mathetake)
added a commit that references this issue [on Oct 10, 2025](https://github.com/theagentrouter/agent-router/issues/902#event-20214046647)
[api: enforce the limitation on k8s service at CRD ([#1328](https://github.com/theagentrouter/agent-router/pull/1328))](https://github.com/theagentrouter/agent-router/commit/fe040f46a7067f23fe28e5c1d053e6539be0be0a)
[nutanix-Hrushikesh](https://github.com/nutanix-Hrushikesh)
added 3 commits that reference this issue [on Oct 16, 2025](https://github.com/theagentrouter/agent-router/issues/902#event-20318402097)
[api: enforce the limitation on k8s service at CRD ([theagentrouter#1328](https://github.com/theagentrouter/agent-router/pull/1328))](https://github.com/nutanix-Hrushikesh/ai-gateway/commit/5700a5ee0323963da74e5574b25ffc79dc32eea8)
[api: enforce the limitation on k8s service at CRD ([theagentrouter#1328](https://github.com/theagentrouter/agent-router/pull/1328))](https://github.com/nutanix-Hrushikesh/ai-gateway/commit/37736adc38c25733ab0bd7152ecac7d9ef1ad7a9)
[api: enforce the limitation on k8s service at CRD ([theagentrouter#1328](https://github.com/theagentrouter/agent-router/pull/1328))](https://github.com/nutanix-Hrushikesh/ai-gateway/commit/7b55cbc421f8042c4cacf9cf1a545f37674708f2)
[mathetake](https://github.com/mathetake)
modified the milestones: [v0.5.0](https://github.com/theagentrouter/agent-router/milestone/5), [v0.6.0](https://github.com/theagentrouter/agent-router/milestone/6) [on Dec 2, 2025](https://github.com/theagentrouter/agent-router/issues/902#event-21314618116)
[missBerg](https://github.com/missBerg)
added a commit that references this issue [on Dec 19, 2025](https://github.com/theagentrouter/agent-router/issues/902#event-21682212462)
[api: enforce the limitation on k8s service at CRD ([theagentrouter#1328](https://github.com/theagentrouter/agent-router/pull/1328))](https://github.com/missBerg/agent-router/commit/4fc38aa2a3219d4a4f9379d68125dac8dbd9c043)
[nicolerenee](https://github.com/nicolerenee)
added a commit that references this issue [on Jul 9, 2026](https://github.com/theagentrouter/agent-router/issues/902#event-27748427802)
[fix(inference): headless qwen Service for per-request LB across pods ([#…](https://github.com/nicolerenee/infra/pull/2092)](https://github.com/nicolerenee/infra/commit/8a28fbad4b0df45e58d00ecdb5318da6b3c174fa)
[missBerg](https://github.com/missBerg)
added 
[area/routingRouting, fallback, resilience, InferencePool integration](https://github.com/theagentrouter/agent-router/issues?q=state%3Aopen%20label%3A%22area%2Frouting%22)Routing, fallback, resilience, InferencePool integration
[on Jul 13, 2026](https://github.com/theagentrouter/agent-router/issues/902#event-27925298751)
[Sign up for free](https://github.com/signup?return_to=https://github.com/theagentrouter/agent-router/issues/902)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/theagentrouter/agent-router/issues/902)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
[area/routingRouting, fallback, resilience, InferencePool integration](https://github.com/theagentrouter/agent-router/issues?q=state%3Aopen%20label%3A%22area%2Frouting%22)Routing, fallback, resilience, InferencePool integration[enhancementNew feature or request](https://github.com/theagentrouter/agent-router/issues?q=state%3Aopen%20label%3A%22enhancement%22)New feature or request
### Projects
No projects
### Milestone
  * [ v0.6.0Past due by Apr 25, 2026 ](https://github.com/theagentrouter/agent-router/milestone/6)


### Relationships
None yet
### Development
No branches or pull requests
### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
