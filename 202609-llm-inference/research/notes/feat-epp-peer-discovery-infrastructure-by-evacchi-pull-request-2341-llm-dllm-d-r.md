---
title: 'feat: EPP peer discovery infrastructure by evacchi · Pull Request #2341 ·
  llm-d/llm-d-router · GitHub'
id: feat-epp-peer-discovery-infrastructure-by-evacchi-pull-request-2341-llm-dllm-d-r
tags:
- llmd-router-nonk8s-backends-54b774
- discovery-config
- peer-discovery
- v0.10.0-included
created: '2026-09-11T21:47:49.501302Z'
updated: '2026-09-12T03:37:37.834869Z'
source: https://github.com/llm-d/llm-d-router/pull/2341
source_domain: github.com
fetched_at: '2026-09-11T21:47:49.475325Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub PR #2341, ''feat: EPP peer discovery infrastructure'', author evacchi.
  Opened 10 Aug 2026, MERGED into main 16 Aug 2026 (commit 620045b) -- one day BEFORE
  the v0.10.0 tag (17 Aug 2026), so v0.10.0 DOES include this PR''s separate top-level
  dataLayer.peerDiscovery.pluginRef field. This is the base infrastructure for cross-replica
  EPP peer discovery (distinct from endpoint/backend discovery): a PeerDiscovery interface
  mirroring EndpointDiscovery (Start, Ready), selected via dataLayer.peerDiscovery.pluginRef;
  PeerMetadata/PeerStore/PeerNotifier types; MemoryPeerStore (goroutine-safe in-memory
  peer set); PeerDiscoveryConfig added to DataLayerConfig with pluginRef; docs/peer-discovery.md.
  Explicitly no concrete discovery mechanism ships in this PR -- a Kubernetes-based
  implementation (EndpointSlice reconciler, RBAC, Helm/kustomize) is deferred to a
  separate follow-up PR pending design questions in issue #1892. This establishes
  the pre-consolidation state that PR #2434 (merged 25 Aug 2026, after v0.10.0) later
  folds into dataLayer.discovery.peers.'
---

*Suggested by [[config-consolidate-discovery-plugins-under-a-single-discovery-section-follow-up]] — prior PR that #2434 follows up on, part of discovery config reshaping history*

[Skip to content](https://github.com/llm-d/llm-d-router/pull/2341#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/2341) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/2341) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/2341) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2341).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Conversation
### 
commented [Aug 10, 2026](https://github.com/llm-d/llm-d-router/pull/2341#issue-5108864326) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2341).
Copy link  Copy Markdown 
Contributor
**What type of PR is this?**
/kind feature
**What this PR does / why we need it** :
Introduces the base infrastructure for peer discovery: the plugin interface, data types, in-memory store, config wiring, and documentation. No concrete discovery mechanism is included; a Kubernetes-based implementation (EndpointSlice reconciler, RBAC, Helm/kustomize) will follow in a separate PR once the enablement and transport design questions from [#1892](https://github.com/llm-d/llm-d-router/issues/1892) are settled.
  * `PeerDiscovery` interface mirroring `EndpointDiscovery`: `Start`, `Ready`, selected via `dataLayer.peerDiscovery.pluginRef` in the EPP config.
  * `PeerMetadata`, `PeerStore`, `PeerNotifier` types and the `NewPeerNotifier` adapter, following the same pattern as endpoint discovery.
  * `MemoryPeerStore`: goroutine-safe, in-memory peer set with deterministic ordering.
  * `PeerDiscoveryConfig` added to `DataLayerConfig` with `pluginRef`.
  * `docs/peer-discovery.md` covering the interface, config, store, and how to write a plugin.


`PeerStore` is intentionally separate from `Datastore`: they share the Upsert/Delete/notifier shape but serve different consumers (scheduler vs. cross-replica syncer) and have different lifecycles (InferencePool vs. EPP Deployment).
**Test plan** :
  * Interface contract: fake plugin drives PeerNotifier into a recording store
  * Full stack: fake plugin -> PeerNotifier -> MemoryPeerStore upsert and delete
  * MemoryPeerStore unit: upsert, replace, delete, no-op delete, deterministic ordering


**Which issue(s) this PR fixes** :
Part of [#1892](https://github.com/llm-d/llm-d-router/issues/1892).
**Release note** _(write`NONE` if no user-facing change)_:

```
NONE

```

[evacchi](https://github.com/evacchi) requested a review from a team as a [code owner](https://github.com/llm-d/llm-d-router/blob/95625abeed826accc4dfa7903ef34a4d369830d8/CODEOWNERS#L14) [August 10, 2026 09:55](https://github.com/llm-d/llm-d-router/pull/2341#event-29214888641)
[evacchi](https://github.com/evacchi) requested review from [ahg-g](https://github.com/ahg-g) and [elevran](https://github.com/elevran) and a lite review from [Copilot](https://github.com/apps/copilot-pull-request-reviewer) [August 10, 2026 09:55](https://github.com/llm-d/llm-d-router/pull/2341#event-29214888685)
[github-actions](https://github.com/apps/github-actions) Bot added [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. [ size/XL ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FXL) Denotes a PR that changes 500-999 lines, ignoring generated files. labels [Aug 10, 2026](https://github.com/llm-d/llm-d-router/pull/2341#event-29214894001)
**Copilot** [started reviewing](https://github.com/llm-d/llm-d-router/sessions/2916c0f9-e3af-4a34-9828-742c72c9f70f "View session") on behalf of [evacchi](https://github.com/evacchi) [August 10, 2026 09:55](https://github.com/llm-d/llm-d-router/pull/2341#event-29214912942) [ View session ](https://github.com/llm-d/llm-d-router/sessions/2916c0f9-e3af-4a34-9828-742c72c9f70f)
[evacchi](https://github.com/evacchi) changed the title ~~refactor: EPP peer discovery via EndpointSlices~~ feat: EPP peer discovery via EndpointSlices [Aug 10, 2026](https://github.com/llm-d/llm-d-router/pull/2341#event-29215064340)
[github-actions](https://github.com/apps/github-actions) Bot added [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. and removed [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. labels [Aug 10, 2026](https://github.com/llm-d/llm-d-router/pull/2341#event-29215071554)
reviewed  [ Aug 10, 2026 ](https://github.com/llm-d/llm-d-router/pull/2341#pullrequestreview-4895600406)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/2341/files)
### 
left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
## Pull request overview
Adds an opt-in, Kubernetes-native mechanism for discovering peer EPP replicas by watching the EPP Service’s `EndpointSlice` objects, storing the live peer set for future cross-replica state synchronization work.
**Changes:**
  * Introduce `EPPPeerReconciler` (controller-runtime) to aggregate ready endpoints across EndpointSlices, exclude self, and emit add/update/delete via a `PeerNotifier`.
  * Add `PeerMetadata`/`PeerStore` abstractions and an in-memory `MemoryPeerStore`, plus unit + envtest-style integration coverage.
  * Wire feature gating via `--enable-peer-discovery` + `--epp-peer-service`, and extend RBAC to allow `get/list/watch` on `discovery.k8s.io/endpointslices`.


### Reviewed changes
Copilot reviewed 15 out of 15 changed files in this pull request and generated 3 comments.
Show a summary per file  
| File  | Description  |  
| --- | --- |  
| test/integration/epp/epp_peer_discovery_integration_test.go  | Integration test exercising the reconciler against envtest by manually creating/updating/deleting EndpointSlices.  |  
| pkg/epp/statesync/peerstore.go  | Adds `MemoryPeerStore` implementation for storing live peer membership.  |  
| pkg/epp/statesync/peerstore_test.go  | Unit tests for `MemoryPeerStore` upsert/delete semantics and deterministic ordering.  |  
| pkg/epp/server/runserver.go  | Wires peer discovery reconciler into the server runner when enabled; instantiates the in-memory peer store.  |  
| pkg/epp/server/options.go  | Adds CLI flags and validation for opt-in peer discovery configuration.  |  
| pkg/epp/server/options_test.go  | Adds validation test ensuring peer discovery requires specifying the peer Service.  |  
| pkg/epp/server/controller_manager.go  | Scopes the controller-runtime cache for EndpointSlices to the pool namespace + Service label when enabled.  |  
| pkg/epp/server/controller_config.go  | Extends controller config to carry the peer Service name for cache scoping.  |  
| pkg/epp/framework/interface/datalayer/peer.go  | Introduces peer discovery interfaces/types (`PeerMetadata`, `PeerStore`, `PeerNotifier`).  |  
| pkg/epp/controller/setup_test.go  | Adds a test asserting peer discovery is never leader-election gated.  |  
| pkg/epp/controller/epp_peer_reconciler.go  | Implements the EndpointSlice-based peer discovery reconciler and diffing against prior peer state.  |  
| pkg/epp/controller/epp_peer_reconciler_test.go  | Unit tests for peer aggregation, self-exclusion, diffing behavior, and ID fallback logic.  |  
| deploy/components/inference-gateway/rbac.yaml  | Adds EndpointSlice read permissions to the kustomize component Role.  |  
| config/charts/routerlib/templates/_rbac.yaml  | Adds EndpointSlice read permissions to the Helm Role template.  |  
| cmd/epp/runner/runner.go  | Plumbs enablement/config into controller config + runner, and captures `POD_IP` for self-exclusion.  |  
💡 [Add a `code-review` agent skill](https://github.com/llm-d/llm-d-router/new/main?filename=.github/skills/code-review/SKILL.md) or configure MCP servers for context-aware, tailored reviews. [Learn more in the docs.](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review#mcp-servers-and-agent-skills)
Positive feedbackNegative feedback
Comment thread [config/charts/routerlib/templates/_rbac.yaml](https://github.com/llm-d/llm-d-router/pull/2341/files#diff-3597df43c529a40286c3bbc32698a6310ab30177f3679ad77c15c32411da4e7d) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2341).
Comment thread [deploy/components/inference-gateway/rbac.yaml](https://github.com/llm-d/llm-d-router/pull/2341/files#diff-7eea356646785ce525aa51b7f17330a8eef53205490d5aa5321da86edfce457a) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2341).
Comment thread [test/integration/epp/epp_peer_discovery_integration_test.go](https://github.com/llm-d/llm-d-router/pull/2341/files#diff-e1d0b07078a5111895ed266fb0e8481e85569a5151a85a0d4bff895d47ecab28) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2341).
reviewed  [ Aug 10, 2026 ](https://github.com/llm-d/llm-d-router/pull/2341#pullrequestreview-4897848167)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/2341/files)
Comment thread [pkg/epp/server/options.go](https://github.com/llm-d/llm-d-router/pull/2341/files#diff-b1ab15894417f37f6606e84b1558a51747421cf24a8eddbd86519c0d6c678d07) Outdated   
|  // Peer discovery (active-active state synchronization).  |  
| --- |  
|  EnablePeerDiscovery bool // Enables discovery of peer EPP replicas. Requires PeerServiceName.  |  
### 
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
do we need this flag, i mean isnt `PeerServiceName` enough to enable the feature
### 
[Aug 10, 2026](https://github.com/llm-d/llm-d-router/pull/2341#discussion_r3750868803)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
yeah debatable, I added that because implicitly enabling when PeerServiceName is present seemed brittle, but I am not strongly opinionated
### 
[Aug 13, 2026](https://github.com/llm-d/llm-d-router/pull/2341#discussion_r3774483554)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Prefer seeing this a config file option, not CLI flags. See feedback on L183 (`fs.BoolVar(&opts.EnablePeerDiscovery, "enable-peer-discovery", ...)`)
Comment thread [pkg/epp/controller/epp_peer_reconciler.go](https://github.com/llm-d/llm-d-router/pull/2341/files#diff-c3afe3844fca64ba4151ef642e4b95f0e9dc1cb112d9b4ab9e228882f6e51ec5) Outdated   
|  // endpointReady reports whether an endpoint is serving. A nil Ready condition is  |  
| --- |  
|  // treated as not ready.  |  
|  func endpointReady(ep *discoveryv1.Endpoint) bool {  |  
|  return ep.Conditions.Ready != nil && *ep.Conditions.Ready  |  
### 
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
should not check `serving` instead?
### 
[Aug 10, 2026](https://github.com/llm-d/llm-d-router/pull/2341#discussion_r3750970189)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
incorrect doc string, `Ready` is when it is serving _and not_ terminating, so I think `Ready` should be the appropriate one (you don't use a peer that's serving but about to terminate) -- updated
### 
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
right, the one in termination should be skipped
[evacchi](https://github.com/evacchi) [force-pushed](https://github.com/llm-d/llm-d-router/compare/5a1512510bdf5fc248dfe6acc82bf62dfd7fe395..b79374960ac9f0bd759544eae2144e1dd32c1feb) the  epp-peer-discovery branch from [`5a15125`](https://github.com/llm-d/llm-d-router/commit/5a1512510bdf5fc248dfe6acc82bf62dfd7fe395) to [`b793749`](https://github.com/llm-d/llm-d-router/commit/b79374960ac9f0bd759544eae2144e1dd32c1feb) [ Compare ](https://github.com/llm-d/llm-d-router/compare/5a1512510bdf5fc248dfe6acc82bf62dfd7fe395..b79374960ac9f0bd759544eae2144e1dd32c1feb) [August 10, 2026 15:35](https://github.com/llm-d/llm-d-router/pull/2341#event-29231801374)
[github-actions](https://github.com/apps/github-actions) Bot added [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. and removed [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. labels [Aug 10, 2026](https://github.com/llm-d/llm-d-router/pull/2341#event-29233042409)
[evacchi](https://github.com/evacchi) [force-pushed](https://github.com/llm-d/llm-d-router/compare/0edd771ad45ce4a101a87c7ded5c1decb3874c30..0ac46cbc4216b6535404012c8c556ffbbe7384fa) the  epp-peer-discovery branch from [`0edd771`](https://github.com/llm-d/llm-d-router/commit/0edd771ad45ce4a101a87c7ded5c1decb3874c30) to [`0ac46cb`](https://github.com/llm-d/llm-d-router/commit/0ac46cbc4216b6535404012c8c556ffbbe7384fa) [ Compare ](https://github.com/llm-d/llm-d-router/compare/0edd771ad45ce4a101a87c7ded5c1decb3874c30..0ac46cbc4216b6535404012c8c556ffbbe7384fa) [August 13, 2026 08:24](https://github.com/llm-d/llm-d-router/pull/2341#event-29388219602)
[elevran](https://github.com/elevran) self-assigned this [Aug 13, 2026](https://github.com/llm-d/llm-d-router/pull/2341#event-29392722644)
reviewed  [ Aug 13, 2026 ](https://github.com/llm-d/llm-d-router/pull/2341#pullrequestreview-4925921260)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/2341/files)
### 
left a comment  •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2341).
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Solid implementation of the mechanics (reconciler diffing, self-exclusion, RBAC gating, tests, envtest coverage) but I have design questions to discuss and resolve, plus one concrete bug.
  1. **Enablement mechanism** : this mirrors `EndpointDiscovery`/`DiscoveryNotifier` in shape, but `docs/discovery.md` establishes that this class of feature (a discovery source feeding a datastore) is selected via a plugin entry in the EPP config (`dataLayer.discovery.pluginRef`), not a CLI flag. Presence of the plugin ref is what enables it - no separate `--enable-*` flag exists for endpoint discovery. This PR introduces `--enable-peer-discovery`/`--epp-peer-service` instead, a second, competing enablement idiom for the same shape of problem. Recommend a `PeerDiscovery` plugin interface (mirroring `EndpointDiscovery`) selected via config, both for consistency and because it would let peer discovery work under file-discovery mode too (today it's structurally unavailable there since it's wired directly into the controller-runtime manager setup).
  2. **RBAC widening** : the EndpointSlice watch requires a new `discovery.k8s.io/endpointslices` get/list/watch grant plus an informer cache. Is there a lighter alternative? The EPP Service is currently a plain `ClusterIP` Service (`_service.yaml`), so a DNS SRV query against it today would only resolve to the single VIP, not individual replica addresses - getting per-pod records via DNS would require making the Service **headless**. That's a real option, but it changes how the same Service is resolved on the ext_proc traffic path too (`_helpers.tpl` shows Envoy uses `STRICT_DNS` against this Service in service mode; headless would shift load-balancing from kube-proxy to Envoy directly) - a second behavior change riding along with peer discovery, not a like-for-like swap. Worth deciding deliberately rather than defaulting to the EndpointSlice watch by inertia, and if EndpointSlice is kept, worth stating why explicitly (e.g., per-endpoint `Ready`/`Terminating` conditions and push vs. poll latency are real advantages this PR could call out).
  3. **Peer storage** : `MemoryPeerStore` is structurally identical to the endpoint tracking the existing `Datastore` already does (same key type, same Upsert/Delete/notifier shape). What's the reasoning for a standalone store rather than extending `Datastore` with a second collection?
  4. **Scope** : nothing in this PR consumes `PeerStore`/`PeerNotifier` - the `PeerDiscovery.Store` field comment itself calls it "the handoff point for a cross-replica syncer" that doesn't exist yet. Given (1) and (2) are still open design questions, splitting this into (a) the base interfaces/store with no watcher, and (b) the first concrete discovery mechanism once the enablement/transport questions are settled, would let each land on its own merits. It would also naturally produce a smaller diff (no RBAC/Helm/kustomize changes in the base-infra PR).
  5. **Docs** : the analogous endpoint-discovery feature has a full doc (`docs/discovery.md`) covering the interface, config wiring, and a runnable example. This PR adds two CLI flags, an RBAC permission, a Helm value, and a Kustomize component with no equivalent documentation - worth adding before merge, once the enablement mechanism from point 1 is settled.


One concrete bug alongside the design questions - see inline comment on runner.go.
Comment thread [cmd/epp/runner/runner.go](https://github.com/llm-d/llm-d-router/pull/2341/files#diff-64d12375c06d1950ba073a94fd54042d5374b840489908ccb297be4e3cc897e0) Outdated   
|  EmitEndpointScores: opts.EmitEndpointScores,  |  
| --- |  
|  PeerDiscovery: runserver.PeerDiscovery{  |  
|  ServiceName: peerServiceName,  |  
|  SelfAddress: os.Getenv("POD_IP"),  |  
### 
[Aug 13, 2026](https://github.com/llm-d/llm-d-router/pull/2341#discussion_r3774456846)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
`SelfAddress` comes from `POD_IP`, but nothing in this PR's Helm chart or kustomize overlay sets a `POD_IP` env var on the EPP container. With it unset, `SelfAddress` is `""`, which never matches a real endpoint address, so a replica never excludes itself from its own peer set - silently, since nothing consumes the store yet to surface the bug. Needs a downward-API `POD_IP` env var added to the EPP pod spec (or a `Validate()` check that fails fast when peer discovery is enabled but `POD_IP` is empty).
### 
[Aug 14, 2026](https://github.com/llm-d/llm-d-router/pull/2341#discussion_r3783476561)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
dropping this, rescoping to interface-only
Comment thread [pkg/epp/server/options.go](https://github.com/llm-d/llm-d-router/pull/2341/files#diff-b1ab15894417f37f6606e84b1558a51747421cf24a8eddbd86519c0d6c678d07) Outdated   
|  fs.StringVar(&opts.PoolNamespace, "pool-namespace", opts.PoolNamespace,  |  
| --- |  
|  "Namespace of the InferencePool this Endpoint Picker is associated with.")  |  
|  fs.StringVar(&opts.PoolName, "pool-name", opts.PoolName, "Name of the InferencePool this Endpoint Picker is associated with.")  |  
|  fs.BoolVar(&opts.EnablePeerDiscovery, "enable-peer-discovery", opts.EnablePeerDiscovery,  |  
### 
[Aug 13, 2026](https://github.com/llm-d/llm-d-router/pull/2341#discussion_r3774456850)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
See review body point 1 - this flag pair introduces a second enablement idiom alongside the existing plugin-config pattern for the same kind of feature.
### 
[Aug 14, 2026](https://github.com/llm-d/llm-d-router/pull/2341#discussion_r3783451285)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
good point thanks
Comment thread [pkg/epp/framework/interface/datalayer/peer.go](https://github.com/llm-d/llm-d-router/pull/2341/files#diff-cf0e6bc1a9f576b4201d4d12be6136207246c68921627fa3f25881515ee19c93)  
|  // PeerStore is the narrow interface required by NewPeerNotifier. Any store that  |  
| --- |  
|  // implements PeerUpsert and PeerDelete satisfies it.  |  
|  type PeerStore interface {  |  
### 
[Aug 13, 2026](https://github.com/llm-d/llm-d-router/pull/2341#discussion_r3774456853)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
See review body point 3 - `PeerStore` duplicates `Datastore`'s existing Upsert/Delete/notifier shape for a different kind of object. Worth stating why this is separate rather than folding into `Datastore`.
### 
[Aug 14, 2026](https://github.com/llm-d/llm-d-router/pull/2341#discussion_r3783471299)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I'll extend the doc comment to explain it; essentially the lifecycle of `Datastore` vs `PeerStore` should be different. The `Datastore` is bound to the InferencePool while the `PeerStore` is used by the EPP deployment, adding Peer data in the Datastore felt out of place. The pattern was kept the same for design consistency.
Comment thread [pkg/epp/controller/epp_peer_reconciler_test.go](https://github.com/llm-d/llm-d-router/pull/2341/files#diff-cce4aab0832715d848637eada3675dac37d4ae37df76f7fffd071baa8702b0ae) Outdated   
|  func TestEPPPeerReconciler(t *testing.T) {  |  
| --- |  
|  scheme := runtime.NewScheme()  |  
### 
[Aug 13, 2026](https://github.com/llm-d/llm-d-router/pull/2341#discussion_r3774456860)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
nit: `TestEPPPeerReconciler` covers add, no-op, and delete, but never exercises an in-place metadata change for an existing peer (the `existing != peer` branch that re-emits an `Upsert`) or a `nil` `Ready` condition (vs. explicit `false`). Worth adding both cases.
### 
[Aug 14, 2026](https://github.com/llm-d/llm-d-router/pull/2341#discussion_r3783474289)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
dropping, PR will be rescoped to interface-only as suggested above
Comment thread [pkg/epp/server/options.go](https://github.com/llm-d/llm-d-router/pull/2341/files#diff-b1ab15894417f37f6606e84b1558a51747421cf24a8eddbd86519c0d6c678d07) Outdated   
|  // Peer discovery (active-active state synchronization).  |  
| --- |  
|  EnablePeerDiscovery bool // Enables discovery of peer EPP replicas. Requires PeerServiceName.  |  
### 
[Aug 13, 2026](https://github.com/llm-d/llm-d-router/pull/2341#discussion_r3774483554)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Prefer seeing this a config file option, not CLI flags. See feedback on L183 (`fs.BoolVar(&opts.EnablePeerDiscovery, "enable-peer-discovery", ...)`)
### 
commented [Aug 14, 2026](https://github.com/llm-d/llm-d-router/pull/2341#issuecomment-5293103224)
Copy link  Copy Markdown 
Contributor Author  
| 
>   1. **Enablement mechanism** : this mirrors `EndpointDiscovery`/`DiscoveryNotifier` in shape, but `docs/discovery.md` establishes that this class of feature (a discovery source feeding a datastore) is selected via a plugin entry in the EPP config (`dataLayer.discovery.pluginRef`), not a CLI flag.
> 

totally fair, apologies for the oversight -- I think I actually did this in the past already :D
>   1. **Peer storage** : `MemoryPeerStore` is structurally identical to the endpoint tracking the existing `Datastore` already does (same key type, same Upsert/Delete/notifier shape). What's the reasoning for a standalone store rather than extending `Datastore` with a second collection?
> 

answered inline, essentially although they are similar in shape, the data store has a different lifecycle than the peerstore and it felt a bit out of place -- besides datastore contains a few fields already. Not particularly attached to this design decision and I'm happy to collapse them if that's better.
>   1. **RBAC widening** and **Scope**
> 

As suggested, I'll reduce scope here to mostly interfaces.
>   1. **Docs** : the analogous endpoint-discovery feature has a full doc (`docs/discovery.md`)
> 

adding docs  |  
| --- |  
[evacchi](https://github.com/evacchi) changed the title ~~feat: EPP peer discovery via EndpointSlices~~ feat: EPP peer discovery infrastructure [Aug 14, 2026](https://github.com/llm-d/llm-d-router/pull/2341#event-29456962513)
[github-actions](https://github.com/apps/github-actions) Bot added [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. and removed [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. labels [Aug 14, 2026](https://github.com/llm-d/llm-d-router/pull/2341#event-29456962920)
[evacchi](https://github.com/evacchi) [force-pushed](https://github.com/llm-d/llm-d-router/compare/0ac46cbc4216b6535404012c8c556ffbbe7384fa..8b530b16db99893a2499436f1dcd271275dcf936) the  epp-peer-discovery branch from [`0ac46cb`](https://github.com/llm-d/llm-d-router/commit/0ac46cbc4216b6535404012c8c556ffbbe7384fa) to [`8b530b1`](https://github.com/llm-d/llm-d-router/commit/8b530b16db99893a2499436f1dcd271275dcf936) [ Compare ](https://github.com/llm-d/llm-d-router/compare/0ac46cbc4216b6535404012c8c556ffbbe7384fa..8b530b16db99893a2499436f1dcd271275dcf936) [August 14, 2026 12:17](https://github.com/llm-d/llm-d-router/pull/2341#event-29456973468)
[evacchi](https://github.com/evacchi) requested a review from [shmuelk](https://github.com/shmuelk) as a [code owner](https://github.com/llm-d/llm-d-router/blob/b3af8cc8d159c2a19eb7d329ec0417a527a22dd4/CODEOWNERS#L26) [August 14, 2026 12:17](https://github.com/llm-d/llm-d-router/pull/2341#event-29456973516)
[evacchi](https://github.com/evacchi) added 2 commits [August 14, 2026 14:17](https://github.com/llm-d/llm-d-router/pull/2341#commits-pushed-b3d19fd)
` refactor: EPP peer discovery via EndpointSlices[](https://github.com/llm-d/llm-d-router/pull/2341/commits/b3d19fd8ab75f8b81f53fa8210bd5e13e0e71024 "refactor: EPP peer discovery via EndpointSlices

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[evacchi](https://github.com/evacchi) Edoardo Vacchi 
SSH Key Fingerprint: O38RkrbpO1QdWl6+wTKHJZwus8KVSErGqwx4r64bu00
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>
```

Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[evacchi](https://github.com/evacchi) Edoardo Vacchi 
SSH Key Fingerprint: O38RkrbpO1QdWl6+wTKHJZwus8KVSErGqwx4r64bu00
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>
```

[evacchi](https://github.com/evacchi) added 5 commits [August 14, 2026 14:17](https://github.com/llm-d/llm-d-router/pull/2341#commits-pushed-1cf21fa)
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[evacchi](https://github.com/evacchi) Edoardo Vacchi 
SSH Key Fingerprint: O38RkrbpO1QdWl6+wTKHJZwus8KVSErGqwx4r64bu00
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>
```

` ensure rbac is gated only when peer discovery is enabled[](https://github.com/llm-d/llm-d-router/pull/2341/commits/ec674369b699715938da24ecbcf545307c4e23cd "ensure rbac is gated only when peer discovery is enabled

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[evacchi](https://github.com/evacchi) Edoardo Vacchi 
SSH Key Fingerprint: O38RkrbpO1QdWl6+wTKHJZwus8KVSErGqwx4r64bu00
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>
```

` ensure rbac is gated only when peer discovery is enabled (kustomize o…[](https://github.com/llm-d/llm-d-router/pull/2341/commits/7fc02d38e80788ab253e8ebdfd7b0a6999cec35c "ensure rbac is gated only when peer discovery is enabled \(kustomize overlay\)

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[evacchi](https://github.com/evacchi) Edoardo Vacchi 
SSH Key Fingerprint: O38RkrbpO1QdWl6+wTKHJZwus8KVSErGqwx4r64bu00
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
…verlay)

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>
```

` refactor: use config instead of flags[](https://github.com/llm-d/llm-d-router/pull/2341/commits/654d37676f2aa72ce376c8f7665c21fe1691e97d "refactor: use config instead of flags

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[evacchi](https://github.com/evacchi) Edoardo Vacchi 
SSH Key Fingerprint: O38RkrbpO1QdWl6+wTKHJZwus8KVSErGqwx4r64bu00
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>
```

` restore to main to rescope + address comments[](https://github.com/llm-d/llm-d-router/pull/2341/commits/d5dc032b135ce88a2ef402db846e9d13cefbd905 "restore to main to rescope + address comments

Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[evacchi](https://github.com/evacchi) Edoardo Vacchi 
SSH Key Fingerprint: O38RkrbpO1QdWl6+wTKHJZwus8KVSErGqwx4r64bu00
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2341).

```
Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>
```

[evacchi](https://github.com/evacchi) [force-pushed](https://github.com/llm-d/llm-d-router/compare/8b530b16db99893a2499436f1dcd271275dcf936..d5dc032b135ce88a2ef402db846e9d13cefbd905) the  epp-peer-discovery branch from [`8b530b1`](https://github.com/llm-d/llm-d-router/commit/8b530b16db99893a2499436f1dcd271275dcf936) to [`d5dc032`](https://github.com/llm-d/llm-d-router/commit/d5dc032b135ce88a2ef402db846e9d13cefbd905) [ Compare ](https://github.com/llm-d/llm-d-router/compare/8b530b16db99893a2499436f1dcd271275dcf936..d5dc032b135ce88a2ef402db846e9d13cefbd905) [August 14, 2026 12:17](https://github.com/llm-d/llm-d-router/pull/2341#event-29456987023)
### 
commented [Aug 14, 2026](https://github.com/llm-d/llm-d-router/pull/2341#issuecomment-5293193121)
Copy link  Copy Markdown 
Contributor Author  
|  rescoped: only interfaces. Comments addressed. Rebased.  |  
| --- |  
approved these changes  [ Aug 16, 2026 ](https://github.com/llm-d/llm-d-router/pull/2341#pullrequestreview-4946257596)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/2341/files/d5dc032b135ce88a2ef402db846e9d13cefbd905)
### 
left a comment 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
rescoped diff directly and completely addresses all prior review feedback; small, clean, matches existing discovery.go patterns closely. Thanks!
### 
commented [Aug 16, 2026](https://github.com/llm-d/llm-d-router/pull/2341#issuecomment-5307614268)
Copy link  Copy Markdown 
Collaborator  
|  one thing worth discussing: does it make sense to group the two discovery plugins in the config togther? 
```
discovery:
    - endoints:
       pluginRef: ....
    - peers:
       pluginRef: ....
```
Would require a backward compatibility shim for two releases but after would be consolidated. Thoughts? Can be a separate PR as there are no users for peers yet.  |  
| --- |  
### 
commented [Aug 16, 2026](https://github.com/llm-d/llm-d-router/pull/2341#issuecomment-5307617176) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2341).
Copy link  Copy Markdown 
Collaborator  
|  [@evacchi](https://github.com/evacchi) - merging, but would appreciate your thoughts on the config schema change.  |  
| --- |  
Hide details View details
[elevran](https://github.com/elevran) merged commit [`620045b`](https://github.com/llm-d/llm-d-router/commit/620045bb19a95b088dc53f046c78a2d712b90a76) into  llm-d:main [Aug 16, 2026](https://github.com/llm-d/llm-d-router/pull/2341#event-29522922194)
39 checks passed 
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2341).
[evacchi](https://github.com/evacchi) deleted the  epp-peer-discovery  branch [August 17, 2026 09:22](https://github.com/llm-d/llm-d-router/pull/2341#event-29553840708)
### 
commented [Aug 17, 2026](https://github.com/llm-d/llm-d-router/pull/2341#issuecomment-5314266828)
Copy link  Copy Markdown 
Contributor Author  
|  oh yes that sounds good!  |  
| --- |  
[pazqo](https://github.com/pazqo) mentioned this pull request [ Aug 18, 2026 ](https://github.com/llm-d/llm-d-router/pull/2341#ref-pullrequest-5180116866)
[ config: consolidate discovery plugins under a single discovery section (follow up #2341) #2434 ](https://github.com/llm-d/llm-d-router/pull/2434)
Merged 
[pazqo](https://github.com/pazqo) added a commit to pazqo/llm-d-router that referenced this pull request [ Aug 20, 2026 ](https://github.com/llm-d/llm-d-router/pull/2341#ref-commit-6f11b89)
` config: remove pre-release peerDiscovery deprecation shim[](https://github.com/pazqo/llm-d-router/commit/6f11b89ba56520dc4bb5a34f0806c8154be10580 "config: remove pre-release peerDiscovery deprecation shim

The dataLayer.peerDiscovery field was introduced in #2341 but never
shipped in a release, so no users depend on it. Remove the field, its
migration shim, and its test instead of carrying unnecessary deprecation
machinery. The endpoint discovery pluginRef migration \(which IS released\)
is preserved.

Signed-off-by: Stefano Pascolutti <stefano.pascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[pazqo](https://github.com/pazqo) Stefano Pascolutti 
SSH Key Fingerprint: mwgPqeAFzir5KT87TfNQTpPsVMyY6u9Iqx1JmMeYtWg
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2341).

```
The dataLayer.peerDiscovery field was introduced in llm-d#2341[](https://github.com/llm-d/llm-d-router/pull/2341) but never
shipped in a release, so no users depend on it. Remove the field, its
migration shim, and its test instead of carrying unnecessary deprecation
machinery. The endpoint discovery pluginRef migration (which IS released)
is preserved.

Signed-off-by: Stefano Pascolutti <stefano.pascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>
```

This was referenced Aug 21, 2026
[ Cross-replica state synchronization for active-active EPP #1892 ](https://github.com/llm-d/llm-d-router/issues/1892)
[ feat: pod watch peer discovery plugin #2501 ](https://github.com/llm-d/llm-d-router/pull/2501)
[elevran](https://github.com/elevran) pushed a commit that referenced this pull request [ Aug 25, 2026 ](https://github.com/llm-d/llm-d-router/pull/2341#ref-commit-950c963)
` config: consolidate discovery plugins under a single discovery sectio…[](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19 "config: consolidate discovery plugins under a single discovery section \(follow up #2341\) \(#2434\)

* config: consolidate discovery plugins under a single discovery section

Group endpoint discovery and peer discovery under dataLayer.discovery
with endpoints and peers sub-fields instead of two separate top-level
fields \(dataLayer.discovery and dataLayer.peerDiscovery\).

A migration shim in the config loader preserves backward compatibility:
- dataLayer.discovery.pluginRef migrates to discovery.endpoints.pluginRef
- dataLayer.peerDiscovery migrates to discovery.peers

Follows the existing deprecation pattern used for parser and
saturationDetector fields.

Signed-off-by: Stefano Pascolutti <spascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>

* config: remove pre-release peerDiscovery deprecation shim

The dataLayer.peerDiscovery field was introduced in #2341 but never
shipped in a release, so no users depend on it. Remove the field, its
migration shim, and its test instead of carrying unnecessary deprecation
machinery. The endpoint discovery pluginRef migration \(which IS released\)
is preserved.

Signed-off-by: Stefano Pascolutti <stefano.pascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>

* address review feedback on discovery config migration

Add "If both are set, the new field is used." to the discovery
pluginRef deprecation message, matching the saturationDetector and
parser migration patterns. Extract a local variable in runner.go
to reduce repetition of the DataLayer field path.

Signed-off-by: Stefano Pascolutti <spascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>

---------

Signed-off-by: Stefano Pascolutti <spascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>
Signed-off-by: Stefano Pascolutti <stefano.pascolutti@gmail.com>") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2341).

```
…n (follow up #2341[](https://github.com/llm-d/llm-d-router/pull/2341)) (#2434[](https://github.com/llm-d/llm-d-router/pull/2434))

* config: consolidate discovery plugins under a single discovery section

Group endpoint discovery and peer discovery under dataLayer.discovery
with endpoints and peers sub-fields instead of two separate top-level
fields (dataLayer.discovery and dataLayer.peerDiscovery).

A migration shim in the config loader preserves backward compatibility:
- dataLayer.discovery.pluginRef migrates to discovery.endpoints.pluginRef
- dataLayer.peerDiscovery migrates to discovery.peers

Follows the existing deprecation pattern used for parser and
saturationDetector fields.

Signed-off-by: Stefano Pascolutti <spascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>

* config: remove pre-release peerDiscovery deprecation shim

The dataLayer.peerDiscovery field was introduced in #2341[](https://github.com/llm-d/llm-d-router/pull/2341) but never
shipped in a release, so no users depend on it. Remove the field, its
migration shim, and its test instead of carrying unnecessary deprecation
machinery. The endpoint discovery pluginRef migration (which IS released)
is preserved.

Signed-off-by: Stefano Pascolutti <stefano.pascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>

* address review feedback on discovery config migration

Add "If both are set, the new field is used." to the discovery
pluginRef deprecation message, matching the saturationDetector and
parser migration patterns. Extract a local variable in runner.go
to reduce repetition of the DataLayer field path.

Signed-off-by: Stefano Pascolutti <spascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>

---------

Signed-off-by: Stefano Pascolutti <spascolutti@gmail.com>
Signed-off-by: Stefano Pascolutti <pascoluttistefano@gmail.com>
Signed-off-by: Stefano Pascolutti <stefano.pascolutti@gmail.com>
```

[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fllm-d%2Fllm-d-router%2Fpull%2F2341)
###  Labels 
[ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. [ size/XL ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FXL) Denotes a PR that changes 500-999 lines, ignoring generated files.
###  4 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
