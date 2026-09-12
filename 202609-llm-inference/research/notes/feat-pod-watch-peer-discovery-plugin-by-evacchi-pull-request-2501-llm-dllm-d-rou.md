---
title: 'feat: pod watch peer discovery plugin by evacchi · Pull Request #2501 · llm-d/llm-d-router
  · GitHub'
id: feat-pod-watch-peer-discovery-plugin-by-evacchi-pull-request-2501-llm-dllm-d-rou
tags:
- llmd-router-nonk8s-backends-54b774
- discovery-config
- peer-discovery
- k8s-native
- non-k8s-gap-evidence
created: '2026-09-11T21:49:56.737943Z'
updated: '2026-09-12T03:37:37.838032Z'
source: https://github.com/llm-d/llm-d-router/pull/2501
source_domain: github.com
fetched_at: '2026-09-11T21:49:56.704199Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub PR #2501, ''feat: pod watch peer discovery plugin'', author evacchi.
  Opened 21 Aug 2026, still OPEN/unmerged as of this fetch (11 Sep 2026) -- not in
  v0.10.0 and not yet on main. Follow-up to PR #2341''s peer-discovery infrastructure
  and to issue #1892 (cross-replica EPP state sync). Ships a k8s-peer-discovery plugin
  (pkg/epp/framework/plugins/datalayer/discovery/k8speer), Alpha stability, selected
  via dataLayer.peerDiscovery.pluginRef, with selector/port/namespace parameters.
  Mechanism: an EPPPeerReconciler built on controller-runtime watches the EPP deployment''s
  own Pods directly (not EndpointSlices -- that RBAC-heavier approach was explored
  and rejected during review of #2341; a DNS/headless-service alternative was also
  proposed and passed over), filters by readiness, excludes self via POD_IP, diffs
  against the previous peer set, and drives add/update/delete through PeerNotifier;
  runs on every EPP replica with leader election disabled. This is the newest discovery-plugin
  PR found across the repo''s PR history and it is explicitly Kubernetes-native (Pod
  watch via controller-runtime) -- direct evidence that, as of 11 Sep 2026, llm-d-router
  has shipped or proposed NO Consul/etcd/Slurm/Ray-style non-Kubernetes discovery
  plugin; every discovery-plugin PR found (FileDiscovery in v0.9.0, k8s-peer-discovery
  here) targets either flat config files or Kubernetes primitives.'
---

*Suggested by [[pull-requests-llm-dllm-d-router-github]] — newest discovery-plugin PR found in search, check whether it targets k8s or non-k8s backends*

[Skip to content](https://github.com/llm-d/llm-d-router/pull/2501#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/2501) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/2501) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/2501) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2501).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Conversation
### 
commented [Aug 21, 2026](https://github.com/llm-d/llm-d-router/pull/2501#issue-5216539302) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2501).
Copy link  Copy Markdown 
Contributor
**What type of PR is this?**
/kind feature
**What this PR does / why we need it** : [#2341](https://github.com/llm-d/llm-d-router/pull/2341) introduced interfaces to implement Peer discovery; we also explored watching `EndpointSlices` to populate the peer list (requiring RBAC changes), but it was suggested to reconsider during review.
Alternative approaches were proposed, including DNS queries (requiring a headless service); we propose an alternative that does not require changes in RBAC or Service config.
We add a controller-runtime reconciler that watches this EPP deployment's own Pods directly.
  * `k8s-peer-discovery` plugin (`pkg/epp/framework/plugins/datalayer/discovery/k8speer`), Alpha stability, with `selector`, `port`, and `namespace` parameters, selected via `dataLayer.peerDiscovery.pluginRef`.
  * `EPPPeerReconciler`: watches Pods matching `Selector`/`Namespace`, filters by readiness, excludes self via `POD_IP`, diffs against the previous peer set to drive add/update/delete through `PeerNotifier`. Runs on every replica (leader election disabled for this controller).
  * Runner wiring resolves the configured `PeerDiscovery` plugin and registers the reconciler with the manager via the concrete plugin type.
  * `docs/peer-discovery.md` documents the plugin, its parameters, and an example config.


**Which issue(s) this PR fixes** :
  * Part of [Cross-replica state synchronization for active-active EPP #1892](https://github.com/llm-d/llm-d-router/issues/1892)
  * Follow up to [feat: EPP peer discovery infrastructure #2341](https://github.com/llm-d/llm-d-router/pull/2341)


**Release note** _(write`NONE` if no user-facing change)_:

```
NONE

```

[github-actions](https://github.com/apps/github-actions) Bot added [ size/XL ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FXL) Denotes a PR that changes 500-999 lines, ignoring generated files. [ area/epp ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fepp) [ area/datalayer ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fdatalayer) [ area/docs ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fdocs) labels [Aug 21, 2026](https://github.com/llm-d/llm-d-router/pull/2501#event-29818663919)
[evacchi](https://github.com/evacchi) mentioned this pull request [ Aug 21, 2026 ](https://github.com/llm-d/llm-d-router/pull/2501#ref-issue-4810549098)
[ Cross-replica state synchronization for active-active EPP #1892 ](https://github.com/llm-d/llm-d-router/issues/1892)
4 tasks
[evacchi](https://github.com/evacchi) [force-pushed](https://github.com/llm-d/llm-d-router/compare/db739ed4e5077ecb4fe5f93ee231a2b1b30fca1b..a8577bfee3307869542eb7dc0ed040464360efa5) the  epp-peer-pod branch 4 times, most recently from [`db739ed`](https://github.com/llm-d/llm-d-router/commit/db739ed4e5077ecb4fe5f93ee231a2b1b30fca1b) to [`a8577bf`](https://github.com/llm-d/llm-d-router/commit/a8577bfee3307869542eb7dc0ed040464360efa5) [ Compare ](https://github.com/llm-d/llm-d-router/compare/db739ed4e5077ecb4fe5f93ee231a2b1b30fca1b..a8577bfee3307869542eb7dc0ed040464360efa5) [August 26, 2026 09:28](https://github.com/llm-d/llm-d-router/pull/2501#event-30033369159)
[evacchi](https://github.com/evacchi) marked this pull request as ready for review [August 26, 2026 12:23](https://github.com/llm-d/llm-d-router/pull/2501#event-30041820461)
[evacchi](https://github.com/evacchi) requested a review from a team as a [code owner](https://github.com/llm-d/llm-d-router/blob/531f889ff651f7b40480007d226e9f1a7f6b6e81/CODEOWNERS#L14) [August 26, 2026 12:23](https://github.com/llm-d/llm-d-router/pull/2501#event-30041821309)
[evacchi](https://github.com/evacchi) requested review from [ahg-g](https://github.com/ahg-g) and [elevran](https://github.com/elevran) and a lite review from [Copilot](https://github.com/apps/copilot-pull-request-reviewer) [August 26, 2026 12:23](https://github.com/llm-d/llm-d-router/pull/2501#event-30041821414)
[evacchi](https://github.com/evacchi) [force-pushed](https://github.com/llm-d/llm-d-router/compare/a8577bfee3307869542eb7dc0ed040464360efa5..74536f6b7c6915b81dd34f5df3b3babdef658264) the  epp-peer-pod branch from [`a8577bf`](https://github.com/llm-d/llm-d-router/commit/a8577bfee3307869542eb7dc0ed040464360efa5) to [`74536f6`](https://github.com/llm-d/llm-d-router/commit/74536f6b7c6915b81dd34f5df3b3babdef658264) [ Compare ](https://github.com/llm-d/llm-d-router/compare/a8577bfee3307869542eb7dc0ed040464360efa5..74536f6b7c6915b81dd34f5df3b3babdef658264) [August 26, 2026 12:24](https://github.com/llm-d/llm-d-router/pull/2501#event-30041842975)
**Copilot** [started reviewing](https://github.com/llm-d/llm-d-router/sessions/90ee139a-3e8c-4fae-87b7-3e441e4b9b76 "View session") on behalf of [evacchi](https://github.com/evacchi) [August 26, 2026 12:24](https://github.com/llm-d/llm-d-router/pull/2501#event-30041847679) [ View session ](https://github.com/llm-d/llm-d-router/sessions/90ee139a-3e8c-4fae-87b7-3e441e4b9b76)
reviewed  [ Aug 26, 2026 ](https://github.com/llm-d/llm-d-router/pull/2501#pullrequestreview-5030372030)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/2501/files)
### 
left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
## Pull request overview
Adds a Kubernetes-based peer discovery mechanism for EPP replicas by introducing a controller-runtime reconciler that watches the EPP deployment’s own Pods and a new in-tree `k8s-peer-discovery` plugin to configure and wire that reconciler.
**Changes:**
  * Introduces `EPPPeerReconciler` to watch Pods (selector/namespace), filter by readiness, exclude self, and emit peer upsert/delete events.
  * Adds `k8s-peer-discovery` plugin (plus runner registration and optional `ManagerSetup` hook) to register the reconciler with the manager.
  * Adds unit + integration tests and updates `docs/peer-discovery.md` with plugin documentation and example configuration.


### Reviewed changes
Copilot reviewed 8 out of 8 changed files in this pull request and generated 5 comments.
Show a summary per file  
| File  | Description  |  
| --- | --- |  
| test/integration/epp/epp_peer_discovery_integration_test.go  | Envtest-style integration coverage for peer reconciliation behavior (ready/unready/delete).  |  
| pkg/epp/framework/plugins/datalayer/discovery/k8speer/plugin.go  | New peer discovery plugin that configures and registers the Pod-watching reconciler.  |  
| pkg/epp/framework/interface/plugin/plugins.go  | Adds optional `ManagerSetup` interface for plugins that need manager-time registration.  |  
| pkg/epp/controller/setup_test.go  | Extends leader-election behavior tests to cover the new peer reconciler.  |  
| pkg/epp/controller/epp_peer_reconciler.go  | New reconciler implementation for Pod-based peer discovery.  |  
| pkg/epp/controller/epp_peer_reconciler_test.go  | Unit tests for reconcile diffing and late notifier binding replay.  |  
| docs/peer-discovery.md  | Documents the new in-tree plugin and configuration.  |  
| cmd/epp/runner/runner.go  | Registers the new plugin and conditionally calls `SetupWithManager` when peer discovery is configured.  |  
Suppressed comments (5)
**pkg/epp/controller/epp_peer_reconciler.go:49**
  * BindNotifier is intended to be callable after the controller has already reconciled, but the reconciler struct currently has no synchronization protecting notifier/prev across BindNotifier and Reconcile. Introduce a mutex on the reconciler to serialize access when late-binding the notifier.



```
type EPPPeerReconciler struct {
	client.Reader
	// notifier receives peer add/update/remove events.
	notifier fwkdl.PeerNotifier
	// Selector matches this EPP deployment's own pods (its peers).
	Selector labels.Selector
	// Namespace is the namespace of this EPP deployment's pods.

```

**pkg/epp/controller/epp_peer_reconciler.go:65**
  * The comment on prev says access is serialized by the controller, but BindNotifier can be invoked from another goroutine (late binding). Once a mutex is added, update the comment to reflect the actual synchronization/ownership.



```
	// prev is the last reported peer set, used to compute deletes. Access is
	// serialized by the controller (single concurrent reconcile).
	prev           map[types.NamespacedName]fwkdl.PeerMetadata
	firstReconcile bool

```

**pkg/epp/controller/epp_peer_reconciler.go:95**
  * Reconcile reads/writes r.prev and calls into r.notifier without any synchronization. With late notifier binding (BindNotifier) and potential controller concurrency, this can race and/or panic. Guard the notifier/prev critical section with the reconciler mutex.



```
	if r.notifier != nil {
		for id, peer := range desired {
			if existing, ok := r.prev[id]; !ok || existing != peer {
				p := peer
				r.notifier.Upsert(&p)
			}
		}
		for id := range r.prev {
			if _, ok := desired[id]; !ok {
				r.notifier.Delete(id)
			}
		}
	}
	r.prev = desired


```

**pkg/epp/controller/epp_peer_reconciler.go:119**
  * BindNotifier iterates r.prev while Reconcile may concurrently replace it, which can cause a concurrent map iteration/write panic. Lock around checking/setting notifier and replaying prev.



```
// BindNotifier sets the notifier and replays any previously discovered peers.
// It may only be called once; subsequent calls return an error.
func (r *EPPPeerReconciler) BindNotifier(n fwkdl.PeerNotifier) error {
	if r.notifier != nil {
		return errors.New("notifier already bound")
	}
	r.notifier = n
	for _, peer := range r.prev {
		p := peer
		n.Upsert(&p)
	}
	return nil
}

```

**docs/peer-discovery.md:167**
  * The generic peer discovery config snippet also omits the required `namespace` parameter for k8s-peer-discovery, so copying it will fail plugin parameter validation.



```
```yaml
plugins:
  - name: my-peer-disc
    type: k8s-peer-discovery
    parameters:
      selector: "app=my-epp"
      port: "9002"

dataLayer:
  discovery:
    peers:
      pluginRef: my-peer-disc  # must match a name in plugins above

```


```
</details>



---

💡 <a href="/llm-d/llm-d-router/new/main?filename=.github/skills/code-review/SKILL.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add a `code-review` agent skill</a> or configure MCP servers for context-aware, tailored reviews. <a href="https://docs.github.com/en/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review#mcp-servers-and-agent-skills" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn more in the docs.</a>

```

Positive feedbackNegative feedback
Comment thread [docs/peer-discovery.md](https://github.com/llm-d/llm-d-router/pull/2501/files#diff-7add8ee48389e52ad4cf3aee6a9912b0390742c427e50067c985cd7198a1eba1) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2501).
Comment thread Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2501).
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/k8speer/plugin.go](https://github.com/llm-d/llm-d-router/pull/2501/files#diff-6341516834a34ba9b33708a3cf8bcf21320c50c73aa994de004aafc9a9e77c0a) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2501).
Comment thread [pkg/epp/controller/epp_peer_reconciler.go](https://github.com/llm-d/llm-d-router/pull/2501/files#diff-c3afe3844fca64ba4151ef642e4b95f0e9dc1cb112d9b4ab9e228882f6e51ec5) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2501).
Comment thread Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2501).
[evacchi](https://github.com/evacchi) requested a lite review from [Copilot](https://github.com/apps/copilot-pull-request-reviewer) [August 26, 2026 13:43](https://github.com/llm-d/llm-d-router/pull/2501#event-30046481149)
**Copilot** [started reviewing](https://github.com/llm-d/llm-d-router/sessions/03beeb51-ad13-4cca-afd4-e17295e4bc17 "View session") on behalf of [evacchi](https://github.com/evacchi) [August 26, 2026 13:45](https://github.com/llm-d/llm-d-router/pull/2501#event-30046558471) [ View session ](https://github.com/llm-d/llm-d-router/sessions/03beeb51-ad13-4cca-afd4-e17295e4bc17)
reviewed  [ Aug 26, 2026 ](https://github.com/llm-d/llm-d-router/pull/2501#pullrequestreview-5031205111)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/2501/files)
### 
left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
## Pull request overview
Copilot reviewed 8 out of 8 changed files in this pull request and generated 2 comments.
Positive feedbackNegative feedback
Comment thread Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2501).
Comment thread [pkg/epp/controller/epp_peer_reconciler.go](https://github.com/llm-d/llm-d-router/pull/2501/files#diff-c3afe3844fca64ba4151ef642e4b95f0e9dc1cb112d9b4ab9e228882f6e51ec5) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2501).
[evacchi](https://github.com/evacchi) marked this pull request as draft [August 26, 2026 14:11](https://github.com/llm-d/llm-d-router/pull/2501#event-30048220893)
[evacchi](https://github.com/evacchi) [force-pushed](https://github.com/llm-d/llm-d-router/compare/f97f2486e298256baab1e115d03eb4bd5e5fdad6..4b6091fd66faa1979d45886711a7199d79b65f70) the  epp-peer-pod branch 2 times, most recently from [`f97f248`](https://github.com/llm-d/llm-d-router/commit/f97f2486e298256baab1e115d03eb4bd5e5fdad6) to [`4b6091f`](https://github.com/llm-d/llm-d-router/commit/4b6091fd66faa1979d45886711a7199d79b65f70) [ Compare ](https://github.com/llm-d/llm-d-router/compare/f97f2486e298256baab1e115d03eb4bd5e5fdad6..4b6091fd66faa1979d45886711a7199d79b65f70) [August 26, 2026 14:55](https://github.com/llm-d/llm-d-router/pull/2501#event-30050950608)
[evacchi](https://github.com/evacchi) marked this pull request as ready for review [August 26, 2026 14:55](https://github.com/llm-d/llm-d-router/pull/2501#event-30050954740)
[evacchi](https://github.com/evacchi) requested a lite review from [Copilot](https://github.com/apps/copilot-pull-request-reviewer) [August 26, 2026 15:16](https://github.com/llm-d/llm-d-router/pull/2501#event-30052026581)
reviewed  [ Aug 26, 2026 ](https://github.com/llm-d/llm-d-router/pull/2501#pullrequestreview-5032382640)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/2501/files)
### 
left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
## Pull request overview
Copilot reviewed 8 out of 8 changed files in this pull request and generated 6 comments.
Positive feedbackNegative feedback
Comment thread Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2501).
Comment thread Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2501).
Comment thread [pkg/epp/framework/interface/plugin/plugins.go](https://github.com/llm-d/llm-d-router/pull/2501/files#diff-8386a1b94a2cd37a898fb10f346f39dd947cdda9d7a85e8fc99ab86eda864a7e) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2501).
Comment thread [pkg/epp/framework/interface/plugin/plugins.go](https://github.com/llm-d/llm-d-router/pull/2501/files#diff-8386a1b94a2cd37a898fb10f346f39dd947cdda9d7a85e8fc99ab86eda864a7e) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2501).
Comment thread [cmd/epp/runner/runner.go](https://github.com/llm-d/llm-d-router/pull/2501/files#diff-64d12375c06d1950ba073a94fd54042d5374b840489908ccb297be4e3cc897e0) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2501).
Comment thread [pkg/epp/controller/epp_peer_reconciler.go](https://github.com/llm-d/llm-d-router/pull/2501/files#diff-c3afe3844fca64ba4151ef642e4b95f0e9dc1cb112d9b4ab9e228882f6e51ec5) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2501).
[evacchi](https://github.com/evacchi) added 4 commits [September 1, 2026 10:51](https://github.com/llm-d/llm-d-router/pull/2501#commits-pushed-5cfe86f)
` feat: pod watch peer discovery plugin[](https://github.com/llm-d/llm-d-router/pull/2501/commits/5cfe86fb0c5554223cf874c4a94be064332f9e83 "feat: pod watch peer discovery plugin

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

` refactor: simpler design, no mutexes or buffers[](https://github.com/llm-d/llm-d-router/pull/2501/commits/989e9de145ade530749c8775bc037f8c4e19c646 "refactor: simpler design, no mutexes or buffers

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
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2501).

```
Signed-off-by: Edoardo Vacchi <evacchi@users.noreply.github.com>
```

[evacchi](https://github.com/evacchi) [force-pushed](https://github.com/llm-d/llm-d-router/compare/379e194b7970a5c559f6bedb0f1754adc3b5a806..e46287935a677d3431a1717078c6fe1ee22dfaa0) the  epp-peer-pod branch from [`379e194`](https://github.com/llm-d/llm-d-router/commit/379e194b7970a5c559f6bedb0f1754adc3b5a806) to [`e462879`](https://github.com/llm-d/llm-d-router/commit/e46287935a677d3431a1717078c6fe1ee22dfaa0) [ Compare ](https://github.com/llm-d/llm-d-router/compare/379e194b7970a5c559f6bedb0f1754adc3b5a806..e46287935a677d3431a1717078c6fe1ee22dfaa0) [September 1, 2026 08:51](https://github.com/llm-d/llm-d-router/pull/2501#event-30333778090)
[elevran](https://github.com/elevran) self-assigned this [Sep 8, 2026](https://github.com/llm-d/llm-d-router/pull/2501#event-30756216790)
reviewed  [ Sep 8, 2026 ](https://github.com/llm-d/llm-d-router/pull/2501#pullrequestreview-5142204098)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/2501/files/e46287935a677d3431a1717078c6fe1ee22dfaa0)
### 
left a comment 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
This implementation follows the existing controller-runtime reconciler pattern closely, but I'd like to raise an architectural question: this adds a bespoke controller-runtime reconciler (EPPPeerReconciler) wired directly against the manager, plus concrete-type special-casing in runner.go. The code in pkg/epp/datalayer already generalizes exactly this need ("watch a k8s GVK, react to events") via NotificationSource/NotificationExtractor/Registrant - see how TopologyExtractor's podNotificationHandler watches Pods and filters by readiness inside Extract(), auto-creating its own Pod notification source. Runtime.Start(ctx, mgr) then binds every registered NotificationSource to the manager generically, with no per-plugin-type wiring needed in runner.go.
Implementing k8s-peer-discovery as a NotificationExtractor for the Pod GVK (plus filtering by namespace/selector/readiness/self-IP, etc) would drop epp_peer_reconciler.go and its test entirely, drop the concrete-type assertion in runner.go, and also means any future non-k8speer PeerDiscovery implementation gets started for free instead of silently never running (see inline comment).
One detail to work out under that design: Ready() currently closes after the reconciler's first List() completes; the notification-extractor path has no equivalent single "initial sync done" callback, so Ready() would need a different signal (e.g. waiting on the manager cache sync for the Pod GVK directly).
Also confirming: Plugin.Store() isn't consumed anywhere yet (no CrossReplicaSyncer wiring) - assuming that's expected as staged follow-up work toward [#1892](https://github.com/llm-d/llm-d-router/issues/1892).
Comment thread [pkg/epp/controller/epp_peer_reconciler.go](https://github.com/llm-d/llm-d-router/pull/2501/files/e46287935a677d3431a1717078c6fe1ee22dfaa0#diff-c3afe3844fca64ba4151ef642e4b95f0e9dc1cb112d9b4ab9e228882f6e51ec5)  
|  // PeerNotifier. Peer discovery must run on every replica (not just the  |  
| --- |  
|  // leader), so leader election is disabled for its controller.  |  
|  type EPPPeerReconciler struct {  |  
|  client.Reader  |  
### 
[Sep 8, 2026](https://github.com/llm-d/llm-d-router/pull/2501#discussion_r3958290702)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
This reconciler duplicates machinery the datalayer framework already provides generically. See `pkg/epp/framework/plugins/datalayer/extractor/topology/extractor.go`'s `podNotificationHandler` for the existing pattern: a plugin implementing Registrant registers a NotificationExtractor for Pod GVK, and Runtime.Start binds it to the manager without any per-plugin wiring in runner.go. Filtering (readiness there, readiness+namespace+selector+self-IP here) happens inside `Extract()` against the deep-copied unstructured object.
Worth reconsidering this whole file in favor of that path - see review body for the full rationale.
Comment thread  
|  return nil, nil, err  |  
| --- |  
|  if kp, ok := peerDiscPlugin.(*k8speer.Plugin); ok {  |  
### 
[Sep 8, 2026](https://github.com/llm-d/llm-d-router/pull/2501#discussion_r3958290712)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
resolvePeerDiscovery resolves any fwkdl.PeerDiscovery generically, but only *k8speer.Plugin is ever actually started here. A future PeerDiscovery implementation that isn't *k8speer.Plugin would resolve fine and pass config validation, then silently never run - no error, no log. Compare to EndpointDiscovery, which is started generically (disc.Start(ctx, fwkdl.NewDiscoveryNotifier(ds))) regardless of concrete type.
This goes away if the plugin is reworked to use Registrant/NotificationExtractor instead (see review body), since Runtime.Start binds every registered NotificationSource generically.
Comment thread [test/integration/epp/epp_peer_discovery_integration_test.go](https://github.com/llm-d/llm-d-router/pull/2501/files/e46287935a677d3431a1717078c6fe1ee22dfaa0#diff-e1d0b07078a5111895ed266fb0e8481e85569a5151a85a0d4bff895d47ecab28)  
|  startManagerAndWaitForSync(ctx, t, mgr)  |  
| --- |  
|  // Create pods. The manager-registered Start runnable binds the buffer to  |  
### 
[Sep 8, 2026](https://github.com/llm-d/llm-d-router/pull/2501#discussion_r3958290717)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Stale comment: "binds the buffer to the store" refers to a buffering mechanism that was removed (see the "refactor: simpler design, no mutexes or buffers" commit). The reconciler now writes directly into the store via a notifier bound at construction, there's no buffer to bind.
Comment thread [pkg/epp/framework/interface/plugin/plugins.go](https://github.com/llm-d/llm-d-router/pull/2501/files/e46287935a677d3431a1717078c6fe1ee22dfaa0#diff-8386a1b94a2cd37a898fb10f346f39dd947cdda9d7a85e8fc99ab86eda864a7e)  
|  package plugin  |  
| --- |  
|  import "encoding/json"  |  
|  import (  |  
### 
[Sep 8, 2026](https://github.com/llm-d/llm-d-router/pull/2501#discussion_r3958290725)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
nit: unrelated import-style reformat (single import to grouped form), nothing else in this file is touched by this PR. Suggest reverting to keep the diff scoped to peer discovery.
Comment thread  
|  ## In-tree plugin: k8s-peer-discovery  |  
| --- |  
|  The `k8s-peer-discovery` plugin  |  
|  (`pkg/epp/framework/plugins/datalayer/discovery/k8speer`) watches Pods  |  
### 
[Sep 8, 2026](https://github.com/llm-d/llm-d-router/pull/2501#discussion_r3958434919)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
This section should be modified if the built-in k8s notification extractor mechanism is used.
### 
commented [Sep 8, 2026](https://github.com/llm-d/llm-d-router/pull/2501#issuecomment-5586165773)
Copy link  Copy Markdown 
Contributor Author  
|  thanks for the review, will follow-up soon!  |  
| --- |  
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fllm-d%2Fllm-d-router%2Fpull%2F2501)
###  Labels 
[ area/datalayer ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fdatalayer) [ area/docs ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fdocs) [ area/epp ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fepp) [ size/XL ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FXL) Denotes a PR that changes 500-999 lines, ignoring generated files.
###  3 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
