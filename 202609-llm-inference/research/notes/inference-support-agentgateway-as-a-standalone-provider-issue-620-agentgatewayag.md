---
title: 'Inference: Support agentgateway as a Standalone Provider · Issue #620 · agentgateway/agentgateway'
id: inference-support-agentgateway-as-a-standalone-provider-issue-620-agentgatewayag
tags:
- llmd-router-nonk8s-backends
- agentgateway
- standalone-mode
- epp
- slurm
- inferencepool
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:44:01.035032Z'
updated: '2026-09-12T03:37:37.553854Z'
source: https://github.com/agentgateway/agentgateway/issues/620
source_domain: github.com
fetched_at: '2026-09-11T19:44:01.007966Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Closed agentgateway design/implementation issue (#620, Nov 2025 - Apr 2026)
  tracing the full origin of standalone (non-Gateway-API) EPP support, which is the
  load-bearing design history for routing to non-k8s backends. Key facts: (1) the
  GIE community pursued a ''standalone'' mode dropping the Gateway API dependency
  specifically to support non-k8s orchestrators like Slurm and RL/batch-inference
  workloads (design doc linked); (2) danehans''s Mar 15 2026 implementation plan states
  the intended behavior as ''default target = stable model Service DNS/port, EPP override
  = chosen pod IP:port,'' giving compatibility with pod-selector discovery while preserving
  a future FailOpen fallback; (3) it documents the exact request flow: client -> agtw
  sidecar -> agtw calls EPP over ext-proc -> EPP returns ''x-gateway-destination-endpoint=<ip:port>''
  -> agtw forwards directly to that endpoint; (4) an example standalone config shows
  ''policies.inferenceRouting.endpointPicker.host'' plus a ''failureMode: failOpen''
  field that maintainer howardjohn immediately questioned (''if the EPP is bundled,
  should we expose failureMode at all? Seems you would not want failOpen'') and danehans
  agreed to remove pending user feedback; (5) InferencePool is confirmed NOT required
  in standalone mode — endpoint discovery instead uses a label/pod selector, explicitly
  to ''support non-k8s environments'' later; (6) closed via merged PRs #1429 (''Add
  standalone inference routing local config'') and #1658 (''Trust standalone EPP-selected
  destinations''), landing in agentgateway v1.2.0-alpha.1; (7) notes llm-d has begun
  documenting standalone mode as a supported gateway deployment option, and GIE has
  since published an upstream standalone guide at gateway-api-inference-extension.sigs.k8s.io/guides/standalone/.
  Maintainer howardjohn''s initial skepticism (''made up use case'') was overturned
  once ahg-g explained the batch-inference/Slurm rationale and confirmed the ext-proc/EPP
  wire protocol does not change between modes.'
utility_score: 12.0
---

[Skip to content](https://github.com/agentgateway/agentgateway/issues/620#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/agentgateway/agentgateway/issues/620) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/agentgateway/agentgateway/issues/620) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/agentgateway/agentgateway/issues/620) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/agentgateway/agentgateway/issues/620).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fagentgateway%2Fagentgateway) You must be signed in to change notification settings
  * [ Fork 829 ](https://github.com/login?return_to=%2Fagentgateway%2Fagentgateway)
  * [ Star  4.8k ](https://github.com/login?return_to=%2Fagentgateway%2Fagentgateway)


#  Inference: Support agentgateway as a Standalone Provider
Copy link
Copy link
Closed
Closed
[Inference: Support agentgateway as a Standalone Provider](https://github.com/agentgateway/agentgateway/issues/620#top)#620
Copy link
Labels
[inference-extensionGateway API Inference Extension issue](https://github.com/agentgateway/agentgateway/issues?q=state%3Aopen%20label%3A%22inference-extension%22)Gateway API Inference Extension issue
Milestone
## Description
opened [on Nov 7, 2025](https://github.com/agentgateway/agentgateway/issues/620#issue-3601821303)
Collaborator
Issue body actions
Currently, Inference Gateway EPP/implementation depends on Gateway API for configuration. The Inference Gateway community is pursuing a new "standalone" mode that drops the Gateway API dependency to reduce barriers for adopting EPP for reinforcement learning workloads, and is compatible with other cluster orchestrators like Slurm. See [this design](https://docs.google.com/document/d/1CXjqmxC3te5u3jOgGH3aaVJQDJFqQ3ZKPJ8Mm6JeY4Y/edit?usp=sharing) doc for additional details.
As a first step:
  1. Allow agentgateway to run as a sidecar to the EPP.
  2. Have agentgateway statically configured, e.g. ConfigMap.
  3. Include a Service to front agentgateway


xref: [kubernetes-sigs/gateway-api-inference-extension#1821](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821)
cc: [@howardjohn](https://github.com/howardjohn)
## Activity
### howardjohn commented on Nov 7, 2025 
Collaborator
More actions
I find it incredibly hard to believe that someone is willing to deploy an EPP to Kubernetes, deploy models to Kubernetes, but somehow a gateway being a standalone pod instead of a sidecar container is a step too far in complexity...
### howardjohn commented on Nov 7, 2025 
Collaborator
More actions
like it's probably not a lot of work... but why?
### danehans commented on Nov 7, 2025 
Last edited by danehans
CollaboratorAuthor
More actions
When you have a moment, take a look at the design proposal. Option 1 (sidecar) is moving forward. Option 2 is to implement a native proxy that links with the EPP binary.
  * Users don't need to know anything about InferencePool, HTTPRoute, etc.
  * Provides the foundation for running EPP outside of k8s, e.g. [Slurm](https://slurm.schedmd.com/overview.html).
  * This approach is focused on training-related use cases instead of inference.
  * This could be a good opportunity to position agtw for add'l AI use cases, e.g. training, non-k8s.


### howardjohn commented on Nov 7, 2025 
Collaborator
More actions
> Provides the foundation for running EPP outside of k8s, e.g. [Slurm](https://slurm.schedmd.com/overview.html).
The idea of deploying EPP outside of k8s is fine. Deploying it _in k8s_ but without gateway seems like a made up use case... Also, from the proposal, it seems InferencePool is still needed?
### howardjohn commented on Nov 8, 2025 
Collaborator
More actions
I think the gaps on AGW are probably supporting arbitrary passthrough (today, it must match a known endpoint (btw, this breaks failOpen mode to do passthrough)), and configuring it with local config. Both are low effort, though also low value IMO
### danehans commented on Nov 12, 2025 
CollaboratorAuthor
More actions
> Deploying it in k8s but without gateway seems like a made up use case
[Batch inference](https://docs.ray.io/en/latest/ray-overview/examples/entity-recognition-with-llms/README.html#online-serving) is a common AI/ML use case that standalone EPP is trying to solve.
### howardjohn commented on Nov 12, 2025 
[on Nov 12, 2025](https://github.com/agentgateway/agentgateway/issues/620#issuecomment-3523104320) via email
Collaborator
More actions
I get that but why would they run on Kubernetes without wanting to use Kubernetes APIs. Two things make sense: - All Kubernetes - No Kubernetes A hybrid of those seems very very odd
On Wed, Nov 12, 2025, 12:29 PM Daneyon Hansen ***@***.***> wrote: *danehans* left a comment ([agentgateway/agentgateway#620](https://github.com/agentgateway/agentgateway/issues/620)) <[#620 (comment)](https://github.com/agentgateway/agentgateway/issues/620#issuecomment-3523077416)> Deploying it in k8s but without gateway seems like a made up use case Batch inference <<https://docs.ray.io/en/latest/ray-overview/examples/entity-recognition-with-llms/README.html#online-serving>> is a common AI/ML use case that standalone EPP is trying to solve. — Reply to this email directly, view it on GitHub <[#620 (comment)](https://github.com/agentgateway/agentgateway/issues/620#issuecomment-3523077416)>, or unsubscribe <<https://github.com/notifications/unsubscribe-auth/AAEYGXN2I6S5LHR5C3VBZQT34NU6DAVCNFSM6AAAAACLPJWBU2VHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTKMRTGA3TONBRGY>> . You are receiving this because you were mentioned.Message ID: ***@***.***>
### danehans commented on Nov 12, 2025 
CollaboratorAuthor
More actions
> Also, from the proposal, it seems InferencePool is still needed?
From the [endpoint discovery](https://docs.google.com/document/d/1CXjqmxC3te5u3jOgGH3aaVJQDJFqQ3ZKPJ8Mm6JeY4Y/edit?tab=t.0) section, InferencePool will not be required and instead use a label selector for endpoint discovery. This will help with the batch inference use case and the other approaches in this section, e.g. endpoints registering with EPP, will be introduced later to support non-k8s environments.
### danehans commented on Nov 12, 2025 
CollaboratorAuthor
More actions
We discussed [#620 (comment)](https://github.com/agentgateway/agentgateway/issues/620#issuecomment-3523104320) in more detail during a break out session today at KubeCon. [@ahg-g](https://github.com/ahg-g) (design doc author) chime-in..
### ahg-g commented on Nov 13, 2025 
More actions
It is not a made up case, this is for batch inference and we have customers explicitly asking for that; Using Gateway API to provision a proxy adds friction in the cases where the proxy is literally part of the lifecycle of a batch job. More on this at [kubernetes-sigs/gateway-api-inference-extension#1838](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1838)
### howardjohn commented on Nov 14, 2025 
Collaborator
More actions
If the user is already deploying on kubernetes, already deploying a helm chart, etc -- isn't it more of a packaging problem than anything? We could just bundle a Gateway in the chart? I get there is a bit more complexity there, but it seems like the additional complexity (which can be hidden from the user), relative to the simplicity to not make every implementation implement something new may be worthwhile.
### ahg-g commented on Nov 14, 2025 
More actions
It is not just creating the resources, but also ensuring that they have a gateway controller up and running.
> relative to the simplicity to not make every implementation implement something new may be worthwhile.
There is no requirement for every implementation to support this mode of operation. The protocol is not changing (the ext-proc and EPP protocol on how the endpoint is communicated back to the proxy).
### howardjohn commented on Nov 14, 2025 
[on Nov 14, 2025](https://github.com/agentgateway/agentgateway/issues/620#issuecomment-3533952921) via email
Collaborator
More actions
alright I'm convinced actually .thanks for the context
On Fri, Nov 14, 2025, 9:56 AM Abdullah Gharaibeh ***@***.***> wrote: *ahg-g* left a comment ([agentgateway/agentgateway#620](https://github.com/agentgateway/agentgateway/issues/620)) <[#620 (comment)](https://github.com/agentgateway/agentgateway/issues/620#issuecomment-3533916868)> It is not just creating the resources, but also ensuring that they have a gateway controller up and running. relative to the simplicity to not make every implementation implement something new may be worthwhile. There is no requirement for every implementation to support this mode of operation. The protocol is not changing (the ext-proc and EPP protocol on how the endpoint is communicated back to the proxy). — Reply to this email directly, view it on GitHub <[#620 (comment)](https://github.com/agentgateway/agentgateway/issues/620#issuecomment-3533916868)>, or unsubscribe <<https://github.com/notifications/unsubscribe-auth/AAEYGXNEC4A43UKRQ7CYQY334YJWTAVCNFSM6AAAAACLPJWBU2VHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTKMZTHEYTMOBWHA>> . You are receiving this because you were mentioned.Message ID: ***@***.***>
[npolshakova](https://github.com/npolshakova)
added 
[inference-extensionGateway API Inference Extension issue](https://github.com/agentgateway/agentgateway/issues?q=state%3Aopen%20label%3A%22inference-extension%22)Gateway API Inference Extension issue
[on Mar 6, 2026](https://github.com/agentgateway/agentgateway/issues/620#event-23337911237)
### danehans commented on Mar 15, 2026 
Last edited by danehans
CollaboratorAuthor
More actions
## Implementation Plan
Implement agentgateway as the sidecar proxy for standalone EPP.
  1. Use a pod-selector for endpoint discovery instead of GW API + InferencePool. See [this design](https://docs.google.com/document/d/1CXjqmxC3te5u3jOgGH3aaVJQDJFqQ3ZKPJ8Mm6JeY4Y/edit?usp=sharing) doc for additional details.
  2. Add a dedicated local inference routing backend policy at the full backend-policy layer. Agentgateway already supports a [standalone mode](https://agentgateway.dev/docs/standalone/latest/) based on a local config and the runtime already understands EPP endpoint overrides. Update the local config to support expressing this override policy.
  3. Make local backends honor EPP-selected destinations directly. Today the override path is tied to service-endpoint selection. For the standalone mode, the right behavior is: default target = stable model Service DNS/port, EPP override = chosen pod IP:port. This gives us compatibility with upstream pod-selector discovery and preserves FailOpen fallback (future based on user feedback) when the EPP is unavailable.
  4. Target the GIE upstream [standalone chart](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/config/charts/standalone) as the primary packaging for EPP+AGTW standalone mode. For initial support, add an agentgateway values overlay/example against that chart. In the future, upstream either: 
     * An agentgateway sidecar provider in the standalone chart.
     * Generic helper support for rendering an agtw config ConfigMap.
  5. Provide one inference routing configuration pattern in agtw, but validate it against two EPP modes GW API (default), and standalone.
  6. Add tests: 
     * Local-config parsing for InferenceRouting
     * An integration test with a mock ext-proc service returning `x-gateway-destination-endpoint`
     * A render/validate test for the example sidecar manifest/values.


## Request Flow
Here is an example request flow for standalone EPP+AGTW.
  1. The client sends an HTTP request to the proxy sidecar `<ip:port>`.
  2. Agtw matches a route and sees that inference routing is enabled.
  3. Before forwarding upstream, agtw calls EPP over ext-proc.
  4. EPP chooses the best destination pod from its current endpoint pool.
  5. EPP returns that choice to agtw as `x-gateway-destination-endpoint=<ip:port>`.
  6. Agtw forwards the request to that pod.
  7. The response comes back through agtw to the client.


## Example Config

```
binds:
- port: 8081
  listeners:
  - routes:
    - matches:
      - path:
          pathPrefix: /v1/chat/completions
      backends:
      - service:
          name: default/my-model
          port: 8000
        policies:
          inferenceRouting:
            endpointPicker:
              host: 127.0.0.1:9002
            failureMode: failOpen
...
```

### howardjohn commented on Mar 16, 2026 
Collaborator
More actions
[@danehans](https://github.com/danehans) just double checking, the only changes on this repo needed is to support inferenceRouting in the EPP right?
BTW if the EPP is bundled, should we expose failureMode at all? Seems you would not want failOpen
### danehans commented on Mar 16, 2026 
Last edited by danehans
CollaboratorAuthor
More actions
> [@danehans](https://github.com/danehans) just double checking, the only changes on this repo needed is to support inferenceRouting in the EPP right?
yes, the existing service-based discovery will be leveraged for routing to endpoints. This means ML owners will need to include a K8s service definition in their batch/RL manifests. If this becomes an issue, direct pod discovery based on a label selector would be needed.
> BTW if the EPP is bundled, should we expose failureMode at all? Seems you would not want failOpen
It's hard to tell without specific user requirements. I will remove it for the time being.
### danehans commented on Apr 14, 2026 
Last edited by danehans
CollaboratorAuthor
More actions
llm-d is now documenting standalone mode as a supported gateway deployment option ([xref](https://github.com/robertgshaw2-redhat/llm-d/blob/c12f86e488a34eb5cb3df74b9e05e4929729a718/docs/wip-docs-new/architecture/core/proxy.md#design)).
[npolshakova](https://github.com/npolshakova)
added this to the [v1.2.0](https://github.com/agentgateway/agentgateway/milestone/3) milestone [on Apr 16, 2026](https://github.com/agentgateway/agentgateway/issues/620#event-24581649073)
### danehans commented on Apr 21, 2026 
CollaboratorAuthor
More actions
Standalone is now doc'd upstream: <https://gateway-api-inference-extension.sigs.k8s.io/guides/standalone/>
[howardjohn](https://github.com/howardjohn)
added a commit that references this issue [on Apr 22, 2026](https://github.com/agentgateway/agentgateway/issues/620#event-24763061116)
[Add standalone inference routing local config ([#1429](https://github.com/agentgateway/agentgateway/pull/1429))](https://github.com/agentgateway/agentgateway/commit/a80b7e3527fa9fd33b99239d5f58f78deeaaa804)
[howardjohn](https://github.com/howardjohn)
closed this as [completed](https://github.com/agentgateway/agentgateway/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)in [#1658](https://github.com/agentgateway/agentgateway/pull/1658)[on Apr 24, 2026](https://github.com/agentgateway/agentgateway/issues/620#event-24851301841)
added a commit that references this issue [on Apr 24, 2026](https://github.com/agentgateway/agentgateway/issues/620#event-24857155125)
[Trust standalone EPP-selected destinations ([agentgateway#1658](https://github.com/agentgateway/agentgateway/pull/1658))](https://github.com/TheTechOddBug/agentgateway/commit/2cede80015e068617c014160d3e3273907bd5f99)
This issue will close once commit 2cede80 is merged into the 'main' branch.[2cede80](https://github.com/TheTechOddBug/agentgateway/commit/2cede80015e068617c014160d3e3273907bd5f99)
[alan-bentley-pgh](https://github.com/alan-bentley-pgh)
added 2 commits that reference this issue [on May 19, 2026](https://github.com/agentgateway/agentgateway/issues/620#event-25720050832)
[Add standalone inference routing local config ([agentgateway#1429](https://github.com/agentgateway/agentgateway/pull/1429))](https://github.com/solo-io/agentgateway-new-ui/commit/c2a074ed6fd36bb898aeaf5118111d032960c58a)
[Trust standalone EPP-selected destinations ([agentgateway#1658](https://github.com/agentgateway/agentgateway/pull/1658))](https://github.com/solo-io/agentgateway-new-ui/commit/f637cec71ff3712a00a0e5b99cac2234a8634677)
[Sign up for free](https://github.com/signup?return_to=https://github.com/agentgateway/agentgateway/issues/620)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/agentgateway/agentgateway/issues/620)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
[inference-extensionGateway API Inference Extension issue](https://github.com/agentgateway/agentgateway/issues?q=state%3Aopen%20label%3A%22inference-extension%22)Gateway API Inference Extension issue
No type
### Projects
No projects
### Milestone
  * [ v1.2.0Closed May 20, 2026, 100% complete ](https://github.com/agentgateway/agentgateway/milestone/3)


### Relationships
None yet
### Development
  * [Trust standalone EPP-selected destinationsagentgateway/agentgateway](https://github.com/agentgateway/agentgateway/pull/1658)[Releasev1.2.0-alpha.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.2.0-alpha.1)


### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
