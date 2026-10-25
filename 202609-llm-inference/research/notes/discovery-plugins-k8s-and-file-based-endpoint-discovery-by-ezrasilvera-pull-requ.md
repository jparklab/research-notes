---
title: 'Discovery Plugins: K8s and File-based Endpoint Discovery by ezrasilvera ·
  Pull Request #1007 · llm-d/llm-d-router · GitHub'
id: discovery-plugins-k8s-and-file-based-endpoint-discovery-by-ezrasilvera-pull-requ
tags:
- llmd-router-nonk8s
- llmd-router-nonk8s-backends-54b774
- endpoint-discovery
- pull-request
- k8s-discovery
- correction
- not-merged
created: '2026-09-11T20:54:22.946877Z'
updated: '2026-09-15T04:55:23.888235Z'
source: https://github.com/llm-d/llm-d-router/pull/1007
source_domain: github.com
fetched_at: '2026-09-11T20:54:22.922913Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'CORRECTION (verified 2026-09-11 via GitHub API): PR #1007 was NEVER merged.
  GitHub API confirms merged=false, merge_commit_sha=null, state=closed, closed_at=2026-05-25T10:35:40Z.
  The PR proposed two Kubernetes-based EndpointDiscovery plugins (inference-pool-discovery,
  static-selector-discovery) under pkg/epp/framework/plugins/datalayer/discovery/k8s/,
  but the author put a /hold on it: comment (2026-05-13) ''Don''t merge this PR for
  now. Instead we will introduce the change in stages starting from the file discovery...
  Replace with the file discovery PRs'', and closed it (2026-05-25) with ''Replace
  with teh file discovery PRs'' [sic]. Only the file-discovery plugin actually shipped,
  via PRs #1080 and #1081 (both merged 2026-05-14), landing at pkg/epp/framework/plugins/datalayer/discovery/file/.
  Verified via `git/trees/main?recursive=1`: NO pkg/epp/framework/plugins/datalayer/discovery/k8s/
  directory exists at HEAD -- only discovery/file/ (README.md, multicluster.go, multicluster_test.go,
  plugin.go, plugin_test.go) exists. IMPLICATION FOR THE LOCUS: Kubernetes pod discovery
  in llm-d-router today is NOT an EndpointDiscovery plugin at all -- it remains a
  separate, hardcoded controller-runtime path in cmd/epp/runner/runner.go (the ctrl.GetConfig()
  branch, distinct from the runWithFileDiscovery branch taken when dataLayer.discovery.endpoints
  is configured). The file-discovery plugin (#1080/#1081) is therefore the ONLY EndpointDiscovery
  plugin implementation that exists anywhere in the codebase as of HEAD -- there is
  no watch-based (as opposed to poll/fsnotify-based) reference implementation to copy
  for a custom (e.g. Consul- or Dynamo-backed) discovery plugin. Prior summary on
  this note incorrectly stated the PR was ''merged May 25 2026'' -- that claim is
  superseded by this correction.'
utility_score: 16.0
---

*Suggested by [[feat-add-pluggable-discoveryplugin-interface-by-ezrasilvera-pull-request-908-llm]] — linked follow-on PR implementing K8s and File-based Endpoint Discovery, referenced in PR #908 thread*

[Skip to content](https://github.com/llm-d/llm-d-router/pull/1007#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/1007) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/1007) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/1007) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Conversation
### 
commented [May 6, 2026](https://github.com/llm-d/llm-d-router/pull/1007#issue-4393608552) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).
Copy link  Copy Markdown 
Contributor
## Summary
This PR builds on the _EndpointDiscovery_ interface ([#908](https://github.com/llm-d/llm-d-router/pull/908)) by delivering the concrete plugin implementations:
  1. Two Kubernetes-based plugins that migrate the existing pod-watching logic into the new framework,
  2. A new file-based plugin that allows llm-d to run entirely without Kubernetes.


## Details
### K8s discovery plugins
Introduced two new K8s discovery plugins under (pkg/epp/framework/plugins/datalayer/discovery/k8s). The existing Kubernetes pod-watching logic has been refactored into two explicit plugins:
  * **inference-pool-discovery** : watches pods matching an InferencePool CRD, equivalent to the previous default behavior
  * **static-selector-discovery** : watches pods matching a fixed label selector, equivalent to the previous --endpoint-selector CLI path


Both plugins use the existing datalayer.BindNotificationSource with a shared new podDiscoveryExtractor to translate pod lifecycle events into DiscoveryNotifier calls, eliminating duplicated controller-runtime wiring.
### File based discovery
Introduced a new file-discovery plugin (pkg/epp/framework/plugins/datalayer/discovery/file). The plugin reads endpoints from a YAML file and optionally watches it for live changes via fsnotify.
When this plugin is used the EPP has no dependency on Kubernetes and can run on bare-metal, Slurm, Ray, or any other non-Kubernetes infrastructure -- enabling llm-d inference scheduling in environments such as RL training clusters where a Kubernetes cluster is unavailable or undesirable.
### Runner wiring (cmd/epp/runner/runner.go):
The runner now resolves the active discovery plugin at startup and drives it through NewDiscoveryNotifier. The discovery.pluginRef field in EndpointPickerConfig selects the plugin; when absent, the runner falls back to the legacy K8s behavior described below.
### Backward compatibility
No breaking changes. Existing deployments require no config changes:
The discovery section is only needed when explicitly opting into a non-default plugin such as file-discovery.  
| Config  | Behavior  |  
| --- | --- |  
| No `discovery` section, `--pool-name` set  | Uses `inference-pool-discovery` -- identical to the previous default  |  
| No `discovery` section, `--endpoint-selector` set  | Uses `static-selector-discovery` -- identical to the previous `--endpoint-selector` path  |  
| Neither flag set, no `discovery` section  | EPP fails to start with a clear error  |  
**Documentation** A detailed description can be found in: docs/discovery.md . It covers all three built-in plugins, the backward compatibility behavior, and runnable examples for both standard and P/D disaggregated setups using file-discovery.
[github-project-automation](https://github.com/apps/github-project-automation) Bot added this to [llm-d-router](https://github.com/orgs/llm-d/projects/15) [May 6, 2026](https://github.com/llm-d/llm-d-router/pull/1007#event-25231685507)
[github-actions](https://github.com/apps/github-actions) Bot requested review from [ahg-g](https://github.com/ahg-g) and [elevran](https://github.com/elevran) [May 6, 2026 18:43](https://github.com/llm-d/llm-d-router/pull/1007#event-25231690134)
[github-actions](https://github.com/apps/github-actions) Bot added the [ size/XXL ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FXXL) Denotes a PR that changes 1000+ lines, ignoring generated files. label [May 7, 2026](https://github.com/llm-d/llm-d-router/pull/1007#event-25259534751)
[ezrasilvera](https://github.com/ezrasilvera) [force-pushed](https://github.com/llm-d/llm-d-router/compare/50e715754e2534ad0bdcadbc9789f02f1729d02a..21be895f1656485b11aeef1155f00eb057f655bc) the  datalayer-discovery branch from [`50e7157`](https://github.com/llm-d/llm-d-router/commit/50e715754e2534ad0bdcadbc9789f02f1729d02a) to [`21be895`](https://github.com/llm-d/llm-d-router/commit/21be895f1656485b11aeef1155f00eb057f655bc) [ Compare ](https://github.com/llm-d/llm-d-router/compare/50e715754e2534ad0bdcadbc9789f02f1729d02a..21be895f1656485b11aeef1155f00eb057f655bc) [May 7, 2026 14:07](https://github.com/llm-d/llm-d-router/pull/1007#event-25264677685)
reviewed  [ May 7, 2026 ](https://github.com/llm-d/llm-d-router/pull/1007#pullrequestreview-4245778305)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1007/files)
Comment thread [apix/config/v1alpha1/endpointpickerconfig_types.go](https://github.com/llm-d/llm-d-router/pull/1007/files#diff-b0ac55cea5177bf6d95d92857360cd09545672ee55c54014376c3f72050f6257) Outdated   
|  // relies entirely on the referenced plugin to enumerate and track inference  |  
| --- |  
|  // endpoints. This enables running the EPP without a Kubernetes cluster.  |  
|  // If omitted, the EPP uses the default Kubernetes-based discovery.  |  
|  Discovery *DiscoveryConfig `json:"discovery,omitempty"`  |  
### 
[May 7, 2026](https://github.com/llm-d/llm-d-router/pull/1007#discussion_r3203021190)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I wonder if this does not belong as a section under `DataLayer`?
### 
[May 7, 2026](https://github.com/llm-d/llm-d-router/pull/1007#discussion_r3203707475)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
One issue with that is that DataLayer *DataLayerConfig is already an optional pointer. If discovery moves inside it a user who only wants a custom discovery plugin (no custom sources) would need to write:
dataLayer: discovery: pluginRef: my-file-discovery
instead of the current flat:
discovery: pluginRef: my-file-discovery
### 
[May 7, 2026](https://github.com/llm-d/llm-d-router/pull/1007#discussion_r3203809564)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Saves a single configuration line. If we implement discovery in the datalayer's framework hierarchy I think it makes sense to have it follow the same in config,
### 
[May 9, 2026](https://github.com/llm-d/llm-d-router/pull/1007#discussion_r3213056439)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
You're right that in the code it lives under pkg/epp/framework/plugins/datalayer/discovery, so conceptually it's a datalayer concern. But I think there are a few reasons to keep it top-level beyond just the one config line:
The bigger issue for me is as I pointed previously is that DataLayer is itself an optional pointer. If someone just wants a file-discovery and no custom metrics sources, they currently have no reason to write a dataLayer block at all. For most users dataLayer mainly means metrics collection It would look like discovery is a metrics concern, which it isn't. Also on the runner , we currently just need to check rawConfig.Discovery != nil. If discovery moves under DataLayer we need two checks rawConfig.DataLayer != nil && rawConfig.DataLayer.Discovery != nil . While this is a small change it adds a null-dereference risk if someone forgets it. Looking at the other top-level fields, Parser, FlowControl, SaturationDetector they are all subsystems that live in their own implementation packages too, but they are kept as top-level because they are primary EPP-level knobs. Discovery seems the same to me. I just wanted to make sure you are aware of the concerns, nevertheless if you still prefer to move it I have no problem doing it.
### 
[May 10, 2026](https://github.com/llm-d/llm-d-router/pull/1007#discussion_r3215371372)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Done. Discovery was moved to under Datalayer in the config and is no longer top level (cc: [@elevran](https://github.com/elevran) [@ahg-g](https://github.com/ahg-g) )
reviewed  [ May 7, 2026 ](https://github.com/llm-d/llm-d-router/pull/1007#pullrequestreview-4245789472)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1007/files)
Comment thread [cmd/epp/runner/runner.go](https://github.com/llm-d/llm-d-router/pull/1007/files#diff-64d12375c06d1950ba073a94fd54042d5374b840489908ccb297be4e3cc897e0) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).
reviewed  [ May 7, 2026 ](https://github.com/llm-d/llm-d-router/pull/1007#pullrequestreview-4245794094)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1007/files)
Comment thread [cmd/epp/runner/runner.go](https://github.com/llm-d/llm-d-router/pull/1007/files#diff-64d12375c06d1950ba073a94fd54042d5374b840489908ccb297be4e3cc897e0) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).
reviewed  [ May 7, 2026 ](https://github.com/llm-d/llm-d-router/pull/1007#pullrequestreview-4245810278)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1007/files)
Comment thread Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).
reviewed  [ May 7, 2026 ](https://github.com/llm-d/llm-d-router/pull/1007#pullrequestreview-4246475546)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1007/files)
### 
left a comment 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Thanks [@ezrasilvera](https://github.com/ezrasilvera)!
The overall architecture is sound: `baseK8sDiscovery.runManager` cleanly factors out shared pod-watching code, the file-based plugin is well-tested, and the `DiscoveryNotifier`/`BackendUpsert`/`BackendDelete` additions are clean. Several correctness issues need fixing before merging. See inline comments for four of these.
**pprof and`/metrics` are served unauthenticated. This is a regression from old path** Check `cmd/epp/runner/runner.go` new `serveMetrics` function. The old `setup()` path registered pprof via `profiling.SetupPprofHandlers(mgr)` on the controller-runtime manager, which consults `--secure-serving` and `MetricsEndpointAuth`. The new `serveMetrics` uses a plain `http.Server` with no auth regardless of `opts.MetricsEndpointAuth`. With `EnablePprof=true` heap/goroutine profiles are exposed unauthenticated. Also, importing `"net/http/pprof"` as `nhpprof` registers all standard pprof endpoints on `http.DefaultServeMux` via its `init()`. The new path needs to honor `opts.MetricsEndpointAuth` for `/metrics` and gate pprof behind the same auth filter as before.
**`ExtractNotification`which is the core event-handling logic, is entirely untested** `pkg/epp/framework/plugins/datalayer/discovery/k8s/pod_extractor_test.go`
All branches unexercised: `PoolHasSynced()==false` early return; `EventDelete`; pod not-ready/label-mismatch delete; happy-path upsert. A fake `PoolInfoProvider` is trivial; `NotificationEvent` wraps `*unstructured.Unstructured`.
**`BackendUpsert`and`BackendDelete` have no tests** `pkg/epp/datastore/datastore_test.go`
These are the new entry points for the entire discovery layer. No test covers: new endpoint creation, update-on-existing, `NewEndpoint` returning `nil`, delete-existing, delete-missing (no-op).
**CLI`--pool-name` silently ignored when `discovery.pluginRef` is set without `poolName` param** `cmd/epp/runner/runner.go` - `resolveDiscovery`
The docs table says `poolName` defaults to "from `--pool-name` flag" for `inference-pool-discovery`. The code doesn't implement this: when `discovery.pluginRef: inference-pool-discovery` is in config with no `poolName` param, `--pool-name` is silently ignored and `Start()` fails at runtime with "poolName is required". Either implement the fallback (`ip.PoolName = opts.PoolName` when factory left it empty) or correct the docs and emit a warning log when both are provided.
**`DatalayerBinder`/`DatastoreProvider` defined in the K8s plugin package, not the framework interface layer** `pkg/epp/framework/plugins/datalayer/discovery/k8s/k8s_discovery.go` - `DatastoreProvider`/`DatalayerBinder` interfaces
These are framework-level contracts but defined in a leaf plugin package. The runner imports `k8sdiscovery` solely for these type assertions, and any non-K8s plugin author who needs datalayer injection must import an internal K8s package. Move to `pkg/epp/framework/interface/datalayer/discovery.go`.
**`InferencePoolDiscoveryPlugin`exports struct fields; all other plugins in this codebase don't** In `k8s_discovery.go` the `PoolName`, `PoolNamespace`, `PoolGroup`, `LeaderElect` are exported only because tests access them via type cast. Every other plugin uses unexported fields.
**`rungroup`package has no tests** `internal/rungroup/rungroup.go` — new package with no `_test.go`. The error-propagation contract (one failure cancels context for all) is untested.
  * **File size limit** : `os.ReadFile` in `file_discovery.go:load()` has no upper bound; with `watchFile: true` a large file write can exhaust heap. Use `io.LimitReader` (e.g. 1 MiB cap).
  * **Validate selector at factory time** : `StaticSelectorFactory` defers `ConvertSelectorToLabelsMap` to `Start()`; a malformed selector passes config load but fails at runtime. Move to `StaticSelectorFactory`.
  * **`serveMetrics`shutdown timeout** : `srv.Shutdown(context.Background())` can block indefinitely; use a 5-second timeout context.


Comment thread [pkg/epp/framework/plugins/datalayer/discovery/k8s/k8s_discovery.go](https://github.com/llm-d/llm-d-router/pull/1007/files#diff-c2b1af31a3f28dd32a1a118c09fba3ce95e92053e6520b3e15f1ef79b2503e4f) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/k8s/pod_extractor.go](https://github.com/llm-d/llm-d-router/pull/1007/files#diff-0a1e09f3add5546c966df1c3734e4aa10717a10c04738de7553a69b9dd53c85a) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/file/file_discovery.go](https://github.com/llm-d/llm-d-router/pull/1007/files#diff-927d0a721d28cbf8f89ce29acccbbb9e42e1552c74650ab3b5552c49b1fdb80d) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/k8s/k8s_discovery.go](https://github.com/llm-d/llm-d-router/pull/1007/files#diff-c2b1af31a3f28dd32a1a118c09fba3ce95e92053e6520b3e15f1ef79b2503e4f)  
|  return fmt.Errorf("inference-pool-discovery: failed to create manager: %w", err)  |  
| --- |  
|  if err := (&controller.InferencePoolReconciler{  |  
### 
[May 7, 2026](https://github.com/llm-d/llm-d-router/pull/1007#discussion_r3203628892)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
`InferencePoolReconciler` is registered directly here while Pod events go through `BindNotificationSource` + `NotificationExtractor` in `runManager`. Both watch K8s events on the same manager; using two different patterns is inconsistent and bypasses the datalayer notification machinery.
The reconciler logic maps directly onto a `NotificationExtractor` - `EventDelete`/deletion-timestamp -> `ds.Clear()`, `EventAddOrUpdate` -> convert unstructured to `v1.InferencePool` -> `ds.PoolSet(ctx, reader, endpointPool)`. A new `inferencePoolSelectorExtractor` alongside `podDiscoveryExtractor` in `pod_extractor.go`, wired via:

```
poolSrc := dlnotifications.NewK8sNotificationSource("k8s-notification", "pool-selector-sync", inferencePoolGVK)
datalayer.BindNotificationSource(poolSrc,
    []fwkdl.NotificationExtractor{newInferencePoolSelectorExtractor(k.ds, mgr.GetClient())},
    mgr)
```

`InferenceObjectiveReconciler` and `InferenceModelRewriteReconciler` follow the same pattern. `mgr.GetClient()` is available before `mgr.Start()` so `client.Reader` injection is straightforward.
### 
[May 8, 2026](https://github.com/llm-d/llm-d-router/pull/1007#discussion_r3209349918)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I Agree, but prefer to defer to follow-up PR. Will open an issue to track it. The three reconcilers each have non-trivial logic. Converting them is a major refactor that would require separate review. Adding a TODO comment above each SetupWithManager call:
// TODO: convert to NotificationExtractor via BindNotificationSource for // consistency with pod-discovery.
### 
[May 8, 2026](https://github.com/llm-d/llm-d-router/pull/1007#discussion_r3209359672)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Added TODO comments on all three reconcilers in [`68bbdef`](https://github.com/llm-d/llm-d-router/commit/68bbdef31a5c371387f2f8f7b648d71c9d0c0716). Full conversion to NotificationExtractor will be done in a follow-up.
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/k8s/k8s_discovery.go](https://github.com/llm-d/llm-d-router/pull/1007/files#diff-c2b1af31a3f28dd32a1a118c09fba3ce95e92053e6520b3e15f1ef79b2503e4f) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/k8s/pod_extractor.go](https://github.com/llm-d/llm-d-router/pull/1007/files#diff-0a1e09f3add5546c966df1c3734e4aa10717a10c04738de7553a69b9dd53c85a) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).
Comment thread [pkg/epp/framework/interface/datalayer/discovery_test.go](https://github.com/llm-d/llm-d-router/pull/1007/files#diff-35134e51a2aadc3b757676a2c0661a9fab88ef0f9e9539c88bc115cfcaff0d38) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).
Comment thread Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).
### 
commented [May 8, 2026](https://github.com/llm-d/llm-d-router/pull/1007#issuecomment-4408601638)
Copy link  Copy Markdown 
Contributor Author  
|  [@elevran](https://github.com/elevran) Thanks for the deep review. I replied your inline comments, and this is for the additional list of issues.
> **pprof and`/metrics` are served unauthenticated. This is a regression from old path** Check `cmd/epp/runner/runner.go` new `serveMetrics` function. The old `setup()` path registered pprof via `profiling.SetupPprofHandlers(mgr)` on the controller-runtime manager, which consults `--secure-serving` and `MetricsEndpointAuth`. The new `serveMetrics` uses a plain `http.Server` with no auth regardless of `opts.MetricsEndpointAuth`. With `EnablePprof=true` heap/goroutine profiles are exposed unauthenticated. Also, importing `"net/http/pprof"` as `nhpprof` registers all standard pprof endpoints on `http.DefaultServeMux` via its `init()`. The new path needs to honor `opts.MetricsEndpointAuth` for `/metrics` and gate pprof behind the same auth filter as before.
runner: honor MetricsEndpointAuth in serveMetrics, fix shutdown timeout. Fixed in [`d40587a`](https://github.com/llm-d/llm-d-router/commit/d40587ac500e922feb06cdf459427ed40cb57e16)
> **`ExtractNotification`which is the core event-handling logic, is entirely untested** `pkg/epp/framework/plugins/datalayer/discovery/k8s/pod_extractor_test.go` All branches unexercised: `PoolHasSynced()==false` early return; `EventDelete`; pod not-ready/label-mismatch delete; happy-path upsert. A fake `PoolInfoProvider` is trivial; `NotificationEvent` wraps `*unstructured.Unstructured`.
k8s discovery: add ExtractNotification tests covering all branches. Fixed in [`0701484`](https://github.com/llm-d/llm-d-router/commit/0701484824fa2210f8d68d577237385a5a2bfe01)
> **`BackendUpsert`and`BackendDelete` have no tests** `pkg/epp/datastore/datastore_test.go` These are the new entry points for the entire discovery layer. No test covers: new endpoint creation, update-on-existing, `NewEndpoint` returning `nil`, delete-existing, delete-missing (no-op).
datastore: add tests for BackendUpsert and BackendDelete. Fixed in [`a6d120c`](https://github.com/llm-d/llm-d-router/commit/a6d120c082a296352e58917141522889cc6101aa)
> **CLI`--pool-name` silently ignored when `discovery.pluginRef` is set without `poolName` param** `cmd/epp/runner/runner.go` - `resolveDiscovery` The docs table says `poolName` defaults to "from `--pool-name` flag" for `inference-pool-discovery`. The code doesn't implement this: when `discovery.pluginRef: inference-pool-discovery` is in config with no `poolName` param, `--pool-name` is silently ignored and `Start()` fails at runtime with "poolName is required". Either implement the fallback (`ip.PoolName = opts.PoolName` when factory left it empty) or correct the docs and emit a warning log when both are provided.
runner: inject --pool-name into InferencePoolDiscoveryPlugin when poolName param is omitted. Fixed in [`0e3388e`](https://github.com/llm-d/llm-d-router/commit/0e3388ea3bfd556509626927a387ed6ddb0a4150)
> **`DatalayerBinder`/`DatastoreProvider` defined in the K8s plugin package, not the framework interface layer** `pkg/epp/framework/plugins/datalayer/discovery/k8s/k8s_discovery.go` - `DatastoreProvider`/`DatalayerBinder` interfaces These are framework-level contracts but defined in a leaf plugin package. The runner imports `k8sdiscovery` solely for these type assertions, and any non-K8s plugin author who needs datalayer injection must import an internal K8s package. Move to `pkg/epp/framework/interface/datalayer/discovery.go`.
discovery: move DatastoreProvider and DatalayerBinder to inject package. Fixed in [`3f94d11`](https://github.com/llm-d/llm-d-router/commit/3f94d112516d2a15b61195ef83712e9bc340f3f4)
> **`InferencePoolDiscoveryPlugin`exports struct fields; all other plugins in this codebase don't** In `k8s_discovery.go` the `PoolName`, `PoolNamespace`, `PoolGroup`, `LeaderElect` are exported only because tests access them via type cast. Every other plugin uses unexported fields.
k8s discovery: unexport InferencePoolDiscoveryPlugin fields, add GetPoolName/SetPoolName accessors. Fixed in [`a47abe0`](https://github.com/llm-d/llm-d-router/commit/a47abe0551a5f39878739b1330bd8e09d30430d8)
> **`rungroup`package has no tests** `internal/rungroup/rungroup.go` — new package with no `_test.go`. The error-propagation contract (one failure cancels context for all) is untested.
runner: add tests for RunnableGroup error propagation contract. Fixed in [`c37ba40`](https://github.com/llm-d/llm-d-router/commit/c37ba40978611f6646bdcdb1ac86432ee5c556d6)
>   * **File size limit** : `os.ReadFile` in `file_discovery.go:load()` has no upper bound; with `watchFile: true` a large file write can exhaust heap. Use `io.LimitReader` (e.g. 1 MiB cap).
> 

file-discovery: cap endpoints file reads at 1 MiB to prevent heap exhaustion. Fixed in [`0f21645`](https://github.com/llm-d/llm-d-router/commit/0f216451243e660739845d843952b276bf6cdf4d)
>   * **Validate selector at factory time** : `StaticSelectorFactory` defers `ConvertSelectorToLabelsMap` to `Start()`; a malformed selector passes config load but fails at runtime. Move to `StaticSelectorFactory`.
> 

k8s discovery: validate StaticSelector endpoint selector at construction time. Fixed in [`618e502`](https://github.com/llm-d/llm-d-router/commit/618e50263c205999ecf8f31e346ae2aa8a1e15f3)
>   * **`serveMetrics`shutdown timeout** : `srv.Shutdown(context.Background())` can block indefinitely; use a 5-second timeout context.
> 

runner: honor MetricsEndpointAuth in serveMetrics, fix shutdown timeout. Fixed in [`d40587a`](https://github.com/llm-d/llm-d-router/commit/d40587ac500e922feb06cdf459427ed40cb57e16)  |  
| --- |  
### 
commented [May 9, 2026](https://github.com/llm-d/llm-d-router/pull/1007#issuecomment-4412390570)
Copy link  Copy Markdown 
Contributor Author  
|  Hi [@elevran](https://github.com/elevran) I accepted all your comments and suggestions (except 2: one I suggested to defer and one is waiting for your response).  |  
| --- |  
[ezrasilvera](https://github.com/ezrasilvera) [force-pushed](https://github.com/llm-d/llm-d-router/compare/0701484824fa2210f8d68d577237385a5a2bfe01..d51dd48b969248c0d75c2f0ed21be9c463c58efd) the  datalayer-discovery branch from [`0701484`](https://github.com/llm-d/llm-d-router/commit/0701484824fa2210f8d68d577237385a5a2bfe01) to [`d51dd48`](https://github.com/llm-d/llm-d-router/commit/d51dd48b969248c0d75c2f0ed21be9c463c58efd) [ Compare ](https://github.com/llm-d/llm-d-router/compare/0701484824fa2210f8d68d577237385a5a2bfe01..d51dd48b969248c0d75c2f0ed21be9c463c58efd) [May 9, 2026 11:31](https://github.com/llm-d/llm-d-router/pull/1007#event-25337477377)
[ezrasilvera](https://github.com/ezrasilvera) added 17 commits [May 9, 2026 14:36](https://github.com/llm-d/llm-d-router/pull/1007#commits-pushed-420d549)
` Initial code for discovery plugins[](https://github.com/llm-d/llm-d-router/pull/1007/commits/420d549394d77c5147da52414946449a9c5d1961 "Initial code for discovery plugins

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` use the notificationReconciler for Pod based discovery[](https://github.com/llm-d/llm-d-router/pull/1007/commits/bdd502e5c193f54ed69432cd1f4ab367537100c6 "use the notificationReconciler for Pod based discovery

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` use StaticSelectorDiscoveryPlugin[](https://github.com/llm-d/llm-d-router/pull/1007/commits/58d9de68235847203933792f5a1599058cda13ba "use StaticSelectorDiscoveryPlugin

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` update doc and remove examples[](https://github.com/llm-d/llm-d-router/pull/1007/commits/ab929a8b922700a596d7ba49b885ebe3008a6f42 "update doc and remove examples

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` remove redundant discovery-architecture.png[](https://github.com/llm-d/llm-d-router/pull/1007/commits/ec0bd4e3a89b3ec1bb21cbfe274d25125f477afa "remove redundant discovery-architecture.png

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` go mod tidy: promote sigs.k8s.io/yaml to direct dependency[](https://github.com/llm-d/llm-d-router/pull/1007/commits/7acbb927995ee271bf5cff7ff5e3500452be8f40 "go mod tidy: promote sigs.k8s.io/yaml to direct dependency

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` runner: remove POD_NAME fallback from internal pool label assignment[](https://github.com/llm-d/llm-d-router/pull/1007/commits/a879605dde89e98db2d5cf8ba52c43f7c4f5cb44 "runner: remove POD_NAME fallback from internal pool label assignment

When no --pool-name is set, the pool label used for NewEndpointPool is now always "epp"

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
When no --pool-name is set, the pool label used for NewEndpointPool is now always "epp"

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

[elevran](https://github.com/elevran) removed the [ do-not-merge/hold ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Ado-not-merge%2Fhold) Indicates that a PR should not merge because someone has issued a /hold command. label [May 10, 2026](https://github.com/llm-d/llm-d-router/pull/1007#event-25353025334)
### 
commented [May 10, 2026](https://github.com/llm-d/llm-d-router/pull/1007#issuecomment-4415261270)
Copy link  Copy Markdown 
Collaborator  
|  [@ezrasilvera](https://github.com/ezrasilvera) note DCO status.  |  
| --- |  
` set namesapce for the default case[](https://github.com/llm-d/llm-d-router/pull/1007/commits/29cd38a59dd7d5ed5117952727dd840def1941c9 "set namesapce for the default case

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

[ezrasilvera](https://github.com/ezrasilvera) [force-pushed](https://github.com/llm-d/llm-d-router/compare/0a95b9dee58360154846c1f9fe2f14ed86360268..29cd38a59dd7d5ed5117952727dd840def1941c9) the  datalayer-discovery branch from [`0a95b9d`](https://github.com/llm-d/llm-d-router/commit/0a95b9dee58360154846c1f9fe2f14ed86360268) to [`29cd38a`](https://github.com/llm-d/llm-d-router/commit/29cd38a59dd7d5ed5117952727dd840def1941c9) [ Compare ](https://github.com/llm-d/llm-d-router/compare/0a95b9dee58360154846c1f9fe2f14ed86360268..29cd38a59dd7d5ed5117952727dd840def1941c9) [May 10, 2026 13:13](https://github.com/llm-d/llm-d-router/pull/1007#event-25353621421)
### 
commented [May 10, 2026](https://github.com/llm-d/llm-d-router/pull/1007#issuecomment-4415381728)
Copy link  Copy Markdown 
Contributor Author  
| 
> I was planning to send out a dev guide, but didn't get a chance because we are still working on the llm-d release. I wanted to suggest to have the configuration API aligned with the four core components of EPP: RequestHandler, FlowControl, Scheduler and Datalayer. We need to refactor the existing structure to align with this suggestion as well.
[@ahg-g](https://github.com/ahg-g) sounds reasonable. Do you think it's a blocking issue for this PR, or we can make any needed change in following PRs ? We have have been asked for this ability for couple of concrete use cases that are waiting for it.  |  
| --- |  
### 
commented [May 10, 2026](https://github.com/llm-d/llm-d-router/pull/1007#issuecomment-4415444281)
Copy link  Copy Markdown 
Collaborator  
| 
> [@ahg-g](https://github.com/ahg-g) sounds reasonable. Do you think it's a blocking issue for this PR, or we can make any needed change in following PRs ? We have have been asked for this ability for couple of concrete use cases that are waiting for it.
It has to wait for a release anyway for any one to use it. I am fine if you would like to do that in a follow up, but we should aim to do that before the next release.  |  
| --- |  
### 
commented [May 10, 2026](https://github.com/llm-d/llm-d-router/pull/1007#issuecomment-4415460357)
Copy link  Copy Markdown 
Collaborator  
|  Created [#1041](https://github.com/llm-d/llm-d-router/issues/1041)  |  
| --- |  
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

[ezrasilvera](https://github.com/ezrasilvera) [force-pushed](https://github.com/llm-d/llm-d-router/compare/51bacde3c4052761fa1e745e4cd2316c0b616e93..c14f54804bc3725bf610e36b5fd685aed10f47f2) the  datalayer-discovery branch from [`51bacde`](https://github.com/llm-d/llm-d-router/commit/51bacde3c4052761fa1e745e4cd2316c0b616e93) to [`c14f548`](https://github.com/llm-d/llm-d-router/commit/c14f54804bc3725bf610e36b5fd685aed10f47f2) [ Compare ](https://github.com/llm-d/llm-d-router/compare/51bacde3c4052761fa1e745e4cd2316c0b616e93..c14f54804bc3725bf610e36b5fd685aed10f47f2) [May 10, 2026 18:22](https://github.com/llm-d/llm-d-router/pull/1007#event-25357090045)
### 
commented [May 10, 2026](https://github.com/llm-d/llm-d-router/pull/1007#issuecomment-4416017859)
Copy link  Copy Markdown 
Contributor Author  
| 
> > [@ahg-g](https://github.com/ahg-g) sounds reasonable. Do you think it's a blocking issue for this PR, or we can make any needed change in following PRs ? We have have been asked for this ability for couple of concrete use cases that are waiting for it.
> It has to wait for a release anyway for any one to use it. I am fine if you would like to do that in a follow up, but we should aim to do that before the next release.
[@ahg-g](https://github.com/ahg-g) In your opinion the discovery should be under DataLayer or a top level ? See my comment to [@elevran](https://github.com/elevran) here [#1007 (comment)](https://github.com/llm-d/llm-d-router/pull/1007#discussion_r3213056439) If you strongly prefer to move the discovery to under datalayer I have no problem making that change as part of this PR.  |  
| --- |  
### 
commented [May 10, 2026](https://github.com/llm-d/llm-d-router/pull/1007#issuecomment-4416046769)
Copy link  Copy Markdown 
Contributor Author  
| 
> It has to wait for a release anyway for any one to use it.
[@ahg-g](https://github.com/ahg-g) Good that you mentioned the release, it reminded me that we might need to also add the appropriate guides and documentation. [@elevran](https://github.com/elevran) We probably need to define an issue covering everything you think is required for the release to expose this new capability so we can plan for it. Thanks.  |  
| --- |  
### 
commented [May 10, 2026](https://github.com/llm-d/llm-d-router/pull/1007#issuecomment-4416058226)
Copy link  Copy Markdown 
Collaborator  
|  Thanks [@ezrasilvera](https://github.com/ezrasilvera) , yes it would be good to do it in this PR.  |  
| --- |  
[ezrasilvera](https://github.com/ezrasilvera) added 3 commits [May 10, 2026 22:06](https://github.com/llm-d/llm-d-router/pull/1007#commits-pushed-698939e)
` Discovery removed from EndpointPickerConfig, added to DataLayerConfig[](https://github.com/llm-d/llm-d-router/pull/1007/commits/698939ea2fd54273923b7ac254792bddc1186827 "Discovery removed from EndpointPickerConfig, added to DataLayerConfig

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` update docs with latest changes[](https://github.com/llm-d/llm-d-router/pull/1007/commits/a0fe1ad4b4b3e9fcbd0fa6a464e2589c1f8d134d "update docs with latest changes

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: l8PVNEImzvdNfW0mfaGoNZi0BVVnIhJxrUuOduzYV1Q
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

reviewed  [ May 13, 2026 ](https://github.com/llm-d/llm-d-router/pull/1007#pullrequestreview-4272035047)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1007/files/a0fe1ad4b4b3e9fcbd0fa6a464e2589c1f8d134d)
### 
left a comment 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Thanks [@ezrasilvera](https://github.com/ezrasilvera) for working on this. This PR is very large and I advise against merging it as is and recommend to break it into smaller PRs. Before we add new discovery plugins, we better have a couple of PRs that refactor the code to allow adding them, for example:
  * A PR to refactor the startup code
  * A PR to refactor the existing discovery mechanism into the new plugin architecture
  * A PR to add the new discovery plugins.


Comment thread  
|  // The returned Datastore is **only** meant to be used in the integration test.  |  
| --- |  
|  // Optional managerOverrides are applied to the controller manager options before creation.  |  
|  // setup is kept for integration tests that need to run against a live K8s cluster.  |  
|  // Production code uses Run() instead.  |  
### 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
This divergence is dangerous, integration tests should as much as possible test the exact same path as the one executed in reality. We worked carefully to make that happen in the past after a number of regressions and we need to maintain this principle.
Comment thread
Comment on lines  +213  to  +215   
|  if poolName == "" {  |  
| --- |  
|  poolName = "epp"  |  
### 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I recommend against defaulting in random places, if this is read from a flag, then the flag itself should set the default
Comment thread  
|  LoRAInfoMetric: opts.LoRAInfoMetric,  |  
| --- |  
|  CacheInfoMetric: opts.CacheInfoMetric,  |  
|  useNewMetrics := !r.featureGates[datalayer.EnableLegacyMetricsFeatureGate]  |  
### 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I suggest we deprecate this path in a separate PR and not bother with it here. [@elevran](https://github.com/elevran) wdyt?
### 
[May 13, 2026](https://github.com/llm-d/llm-d-router/pull/1007#discussion_r3236362703)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
this is to be removed in v0.10, tracked separately. Part of removing the whole `backend.metrics` package.
### 
commented [May 13, 2026](https://github.com/llm-d/llm-d-router/pull/1007#issuecomment-4440207043) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).
Copy link  Copy Markdown 
Contributor Author  
| 
> Thanks [@ezrasilvera](https://github.com/ezrasilvera) for working on this. This PR is very large and I advise against merging it as is and recommend to break it into smaller PRs. Before we add new discovery plugins, we better have a couple of PRs that refactor the code to allow adding them, for example:
>   * A PR to refactor the startup code
>   * A PR to refactor the existing discovery mechanism into the new plugin architecture
>   * A PR to add the new discovery plugins.
> 

Hi [@ahg-g](https://github.com/ahg-g) , We do need the file discovery for the coming release, so my plan is to hold with this PR, and instead push several small PRs that will add a filediscovery plugin without changing the k8s behavior at all (not moving the k8s into plugins). The actuall wiring of the filediscovey plugin in the runner will be in the last PR to minimize the impact even further. We can then start slowly as you suggested refactoring the existing k8s code into the new discovery plugin framework through smaller PRs. Thanks for the input.  |  
| --- |  
### 
commented [May 13, 2026](https://github.com/llm-d/llm-d-router/pull/1007#issuecomment-4440979733)
Copy link  Copy Markdown 
Contributor Author  
|  /hold  |  
| --- |  
[github-actions](https://github.com/apps/github-actions) Bot added the [ hold ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Ahold) PRs that are blocked on design, other features, release cycle, etc. label [May 13, 2026](https://github.com/llm-d/llm-d-router/pull/1007#event-25481286800)
### 
commented [May 13, 2026](https://github.com/llm-d/llm-d-router/pull/1007#issuecomment-4440987516) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1007).
Copy link  Copy Markdown 
Contributor Author  
|  Don't merge this PR for now. Instead we will interduce the change in stages starting from the file discovery. The first two PRs in the path to support filediscovery can be found in [#1080](https://github.com/llm-d/llm-d-router/pull/1080) and [#1081](https://github.com/llm-d/llm-d-router/pull/1081)  |  
| --- |  
This was referenced May 13, 2026
[ file-discovery-1: add BackendUpsert/Delete interface methods and extract upertEndpoint helper #1080 ](https://github.com/llm-d/llm-d-router/pull/1080)
Merged 
[ filediscovery-2: add DiscoveryBackendStore interface, NewDiscoveryNotifier … #1081 ](https://github.com/llm-d/llm-d-router/pull/1081)
Merged 
[ filediscovery-3: add FileDiscovery plugin with YAML/JSON file loading and fsnotify watch #1082 ](https://github.com/llm-d/llm-d-router/pull/1082)
Closed 
[ Filediscovery 4: add file discovery path with RunnableGroup, resolveDiscovery, and serveMetrics #1083 ](https://github.com/llm-d/llm-d-router/pull/1083)
Closed 
[elevran](https://github.com/elevran) added the [ do-not-merge/hold ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Ado-not-merge%2Fhold) Indicates that a PR should not merge because someone has issued a /hold command. label [May 13, 2026](https://github.com/llm-d/llm-d-router/pull/1007#event-25494461901)
[ezrasilvera](https://github.com/ezrasilvera) mentioned this pull request [ May 14, 2026 ](https://github.com/llm-d/llm-d-router/pull/1007#ref-pullrequest-4448056219)
[ file-discovery (3/4): add FileDiscovery plugin with YAML/JSON file loading and fsnotify watch #1135 ](https://github.com/llm-d/llm-d-router/pull/1135)
Merged 
[ezrasilvera](https://github.com/ezrasilvera) closed this [May 25, 2026](https://github.com/llm-d/llm-d-router/pull/1007#event-25922525360)
[github-project-automation](https://github.com/apps/github-project-automation) Bot moved this to **Done** in [llm-d-router](https://github.com/orgs/llm-d/projects/15) [May 25, 2026](https://github.com/llm-d/llm-d-router/pull/1007#event-25922529308)
### 
commented [May 25, 2026](https://github.com/llm-d/llm-d-router/pull/1007#issuecomment-4533582557)
Copy link  Copy Markdown 
Contributor Author  
|  Replace with teh file discovery PRs  |  
| --- |  
[ezrasilvera](https://github.com/ezrasilvera) mentioned this pull request [ May 25, 2026 ](https://github.com/llm-d/llm-d-router/pull/1007#ref-issue-4343818096)
[ Refactor the Pod watching to fit the Discovery plugin #910 ](https://github.com/llm-d/llm-d-router/issues/910)
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fllm-d%2Fllm-d-router%2Fpull%2F1007)
###  Labels 
[ do-not-merge/hold ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Ado-not-merge%2Fhold) Indicates that a PR should not merge because someone has issued a /hold command. [ hold ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Ahold) PRs that are blocked on design, other features, release cycle, etc. [ size/XXL ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FXXL) Denotes a PR that changes 1000+ lines, ignoring generated files.
###  4 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
