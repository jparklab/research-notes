---
title: 'FileDiscovery plugin (4/4) - wiring into the runner by ezrasilvera · Pull
  Request #1218 · llm-d/llm-d-router · GitHub'
id: filediscovery-plugin-44-wiring-into-the-runner-by-ezrasilvera-pull-request-1218
tags:
- llmd-router-nonk8s-backends-54b774
- epp-plugin-registration
- router-extensibility
- no-k8s-mode
created: '2026-09-11T20:58:30.703154Z'
updated: '2026-09-12T03:37:37.746523Z'
source: https://github.com/llm-d/llm-d-router/pull/1218
source_domain: github.com
fetched_at: '2026-09-11T20:58:30.680322Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'PR #1218 ''FileDiscovery plugin (4/4) - wiring into the runner'' (opened
  May 19 2026 by ezrasilvera), the PR that first wired an EndpointDiscovery plugin
  into cmd/epp/runner.go and made ''run EPP/llm-d without Kubernetes'' real. Shows
  exactly what any custom discovery plugin (K8s or non-K8s) must satisfy: (1) implement
  the fwkdl.EndpointDiscovery interface (pkg/epp/framework/interface/datalayer/discovery.go),
  which as of this PR requires both Start(ctx, notifier DiscoveryNotifier) error and
  a newly-added Ready() <-chan struct{} method -- Ready''s channel must be closed
  exactly once, only after a successful initial sync (e.g. first file load or first
  K8s list+watch reconcile), and must stay open if Start errors before that point;
  (2) plug into TypedName()/plugin.Plugin via a Factory(name, parameters, handle)
  function registered with fwkplugin.Register. When dataLayer.discovery(.endpoints).pluginRef
  is set, runner.go''s Run() skips controller-manager setup entirely, runs discovery
  + ext_proc/health servers together under a RunnableGroup, and gates request-serving
  (epp-server, health) -- but not /metrics -- on Ready() closing; on initial-load
  failure the runnable group cancels via ctx.Done() instead. The FileDiscovery reference
  implementation (pkg/epp/framework/plugins/datalayer/discovery/file/plugin.go) satisfies
  Ready() with a sync.Once-guarded channel closed right after its first f.load(notifier)
  call succeeds. A reviewer flagged (and the PR discussion addresses) a double-instantiation
  risk: the discovery plugin is already created via loader.InstantiateAndConfigure
  and registered in r.PluginHandle during parseConfigurationPhaseTwo, so resolveDiscovery()
  must look it up rather than re-invoke its factory.'
utility_score: 15.0
---

[Skip to content](https://github.com/llm-d/llm-d-router/pull/1218#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/1218) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/1218) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/1218) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1218).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Conversation
### 
commented [May 19, 2026](https://github.com/llm-d/llm-d-router/pull/1218#issue-4480293218) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1218).
Copy link  Copy Markdown 
Contributor
## Summary
This is the last PR to enable FileDIscovery plugin in EPP, once merged this will **allow users to run EPP (and llm-d) without Kubernetes.**
Wire the file-discovery plugin ([#1135](https://github.com/llm-d/llm-d-router/pull/1135)) into the runner so the EPP can run without a Kubernetes cluster. When dataLayer.discovery.pluginRef`is set, the runner skips controller-manager setup, runs discovery + servers under a`RunnableGroup`, gates request-serving on `EndpointDiscovery.Ready()`, and warns about K8s-only features that are inactive in this mode.
The K8s path is unchanged behavior-wise (one cleanup commit avoids a double-parse of the EPP config).
### Changes to the discoveryEndpoint interface
One function `EndpointDiscovery.Ready()` was added to the interface.
In K8s mode the controller-runtime manager waits for cache sync before the first reconcile; in file-discovery mode there is no manager. Without a synchronization point the runner would start the ext_proc server while discovery is still inside the initial file load, and early requests would land on an empty datastore (503 / "no candidates").
`Ready()` returns a channel closed once after the plugin's first successful sync. The runner gates `epp-server` and `health` on it (metrics is not gated, so `/metrics` is observable during a slow startup). On initial-load failure the channel stays open and the runnable group cancels via `ctx.Done()`.
[github-project-automation](https://github.com/apps/github-project-automation) Bot added this to [llm-d-router](https://github.com/orgs/llm-d/projects/15) [May 19, 2026](https://github.com/llm-d/llm-d-router/pull/1218#event-25722617094)
[github-actions](https://github.com/apps/github-actions) Bot added the [ size/XXL ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FXXL) Denotes a PR that changes 1000+ lines, ignoring generated files. label [May 19, 2026](https://github.com/llm-d/llm-d-router/pull/1218#event-25722622669)
[github-actions](https://github.com/apps/github-actions) Bot requested review from [ahg-g](https://github.com/ahg-g) and [shmuelk](https://github.com/shmuelk) [May 19, 2026 19:15](https://github.com/llm-d/llm-d-router/pull/1218#event-25722623184)
[ezrasilvera](https://github.com/ezrasilvera) [force-pushed](https://github.com/llm-d/llm-d-router/compare/b610afd59f989186fa660f4a2ca3b19791649ab7..1428b67c9626234ccc28b1af1b206385e4e8ae6d) the  filediscovery-4 branch 2 times, most recently from [`b610afd`](https://github.com/llm-d/llm-d-router/commit/b610afd59f989186fa660f4a2ca3b19791649ab7) to [`1428b67`](https://github.com/llm-d/llm-d-router/commit/1428b67c9626234ccc28b1af1b206385e4e8ae6d) [ Compare ](https://github.com/llm-d/llm-d-router/compare/b610afd59f989186fa660f4a2ca3b19791649ab7..1428b67c9626234ccc28b1af1b206385e4e8ae6d) [May 24, 2026 13:40](https://github.com/llm-d/llm-d-router/pull/1218#event-25899545840)
reviewed  [ May 24, 2026 ](https://github.com/llm-d/llm-d-router/pull/1218#pullrequestreview-4353147599)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1218/files)
### 
left a comment 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Wiring is sound and tests pass. One design concern worth addressing before merge, see inline.
/lgtm overall. Leaving approval for later.
Comment thread [cmd/epp/runner/runner.go](https://github.com/llm-d/llm-d-router/pull/1218/files#diff-64d12375c06d1950ba073a94fd54042d5374b840489908ccb297be4e3cc897e0) Outdated   
|  if !ok {  |  
| --- |  
|  return nil, fmt.Errorf("discovery: unknown plugin type %q for plugin %q", spec.Type, ref)  |  
|  p, err := factory(name, spec.Parameters, nil)  |  
### 
[May 24, 2026](https://github.com/llm-d/llm-d-router/pull/1218#discussion_r3295006409)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
This re-instantiates the discovery plugin even though `parseConfigurationPhaseTwo` (called a few lines above) already created it via `loader.InstantiateAndConfigure` and registered it in `r.PluginHandle`. Two instances coexist, **only this second one** has `Start()` called. While no harm today (FileDiscovery's factory has no side effects), but it's confusing and breaks the moment a discovery plugin's factory does anything stateful (open a connection, claim a port, etc.). It also passes a `nil` for `Handle` here while the previous instance got the real one handle.
it would be better to look up the existing instance from `r.PluginHandle` and type-asserting to `fwkdl.EndpointDiscovery` to find the one you need.
### 
[May 24, 2026](https://github.com/llm-d/llm-d-router/pull/1218#discussion_r3295046549)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
[@elevran](https://github.com/elevran) Good catch. I fixed in [`fe57457`](https://github.com/llm-d/llm-d-router/commit/fe57457104e0b9c762c21bc0866d410dbfe4dc2e) I also run local e2e test that verified with the new resolveDiscovery:
  1. Plugins instantiated once by the loader (Instantiated all plugins... log includes file-discovery).
  2. EPP starting (file discovery mode) logs discoveryPlugin: file-discovery/file-discovery the loader-created instance, looked up via r.PluginHandle.
  3. watching endpoints file for changes: proves Start() fired on that same instance (the only one now).
  4. Verified my test got "The capital of France is Paris." back through Envoy -> EPP -> vLLM.


### 
commented [May 24, 2026](https://github.com/llm-d/llm-d-router/pull/1218#issuecomment-4529559094)
Copy link  Copy Markdown 
Collaborator  
|  /lgtm  |  
| --- |  
[github-actions](https://github.com/apps/github-actions) Bot added the [ lgtm ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Algtm) "Looks good to me", indicates that a PR is ready to be merged. label [May 24, 2026](https://github.com/llm-d/llm-d-router/pull/1218#event-25903014330)
reviewed  [ May 24, 2026 ](https://github.com/llm-d/llm-d-router/pull/1218#pullrequestreview-4353395271)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1218/files)
Comment thread  
|  func (r *Runner) parseConfigurationPhaseOne(ctx context.Context, opts *runserver.Options) (*configapi.EndpointPickerConfig, error) {  |  
| --- |  
|  if r.rawConfig != nil {  |  
|  return r.rawConfig, nil  |  
### 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
pls add a comment that this can happen in the case the function is invoked multiple times.
### 
[May 25, 2026](https://github.com/llm-d/llm-d-router/pull/1218#discussion_r3297706800)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
fixed in [`ca3910f`](https://github.com/llm-d/llm-d-router/commit/ca3910f42eb958c04bab8ff1bae7f28e50e88845)
Comment thread [cmd/epp/runner/runner.go](https://github.com/llm-d/llm-d-router/pull/1218/files#diff-64d12375c06d1950ba073a94fd54042d5374b840489908ccb297be4e3cc897e0) Outdated   
|  mux := http.NewServeMux()  |  
| --- |  
|  mux.Handle("/metrics", promhttp.HandlerFor(ctrlmetrics.Registry, promhttp.HandlerOpts{EnableOpenMetrics: true}))  |  
|  if enablePprof {  |  
|  mux.HandleFunc("/debug/pprof/", nhpprof.Index)  |  
### 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
lets define a function in <https://github.com/llm-d/llm-d-router/blob/main/pkg/common/observability/profiling/pprof.go> that returns a `map[string]http.HandlerFunc` of handlers to register, and have this function called here and in the other place pls.
### 
[May 25, 2026](https://github.com/llm-d/llm-d-router/pull/1218#discussion_r3297905695)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Good idea. Done in [`eb06021`](https://github.com/llm-d/llm-d-router/commit/eb06021359bbd9df92528bac35d206485adc4572)
Comment thread  
|  // runWithFileDiscovery handles the execution path when a discovery plugin is configured.  |  
| --- |  
|  // It builds the EPP server stack without a Kubernetes cluster or controller manager.  |  
|  func (r *Runner) runWithFileDiscovery(ctx context.Context, opts *runserver.Options, rawConfig *configapi.EndpointPickerConfig) error {  |  
### 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Is this path covered in any integration tests?
### 
[May 25, 2026](https://github.com/llm-d/llm-d-router/pull/1218#discussion_r3298004739)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Not really. I was testing it manually on my local system, but you are correct that integration test is needed. I put something which I believe is good enough for the current state (at [`8fa1baf`](https://github.com/llm-d/llm-d-router/commit/8fa1baf7f91ad8724dbc7af287da81ca5ddf96b1)) . In the future we would probably want to add a more extensive test or even reuse test/integration/epp envtest harness. Fixed in [`8fa1baf`](https://github.com/llm-d/llm-d-router/commit/8fa1baf7f91ad8724dbc7af287da81ca5ddf96b1)
Comment thread  
|  // runWithFileDiscovery handles the execution path when a discovery plugin is configured.  |  
| --- |  
|  // It builds the EPP server stack without a Kubernetes cluster or controller manager.  |  
### 
[May 24, 2026](https://github.com/llm-d/llm-d-router/pull/1218#discussion_r3295332248) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1218).
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Are you sure we can't use controller manager to manage go routines (which seems to be the reason for this special path)? I think we can if we pass an empty `rest.Config`, and that will help us reduce all this complexity.
Note that in <https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/server/controller_manager.go#L47> we already have a flag to not watch on infpool/infobj/infModelRewrite resources in standalone mode, and we can have another flag to disable that for pods as well by passing another flag, call it `nonk8s`.
### 
[May 25, 2026](https://github.com/llm-d/llm-d-router/pull/1218#discussion_r3298181983) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1218).
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
> Are you sure we can't use controller manager to manage go routines (which seems to be the reason for this special path)? I think we can if we pass an empty `rest.Config`, and that will help us reduce all this complexity.
> Note that in <https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/server/controller_manager.go#L47> we already have a flag to not watch on infpool/infobj/infModelRewrite resources in standalone mode, and we can have another flag to disable that for pods as well by passing another flag, call it `nonk8s`.
[@ahg-g](https://github.com/ahg-g) Thanks for the feedback. I think that for now we should stay with the current design. I looked into this and technically speaking your suggestion does work: the cache isn't doing anything without informers, leader election early-returns when disabled, and the lazy recorder/REST mapper only fire on first use.
However, the savings are very small (62 lines of rungroup.go, an errgroup.Group wrapper) and I'm not sure the costs worth it.
  * First, from a design perspective - we plan eventually to move K8s itself behind a discovery plugin, the runner becomes plugin-agnostic and the K8s plugin owns its own Manager internally. The current RunnableGroup design is the right preparation; the Manager-unification proposal actually goes in the opposite direction towards k8s.
  * Basically the the statement of "This path doesn't touch K8s" moves from compile-time absence (no mgr variable) to a runtime negative-test. Future code that calls mgr.GetClient() or fires an event would fail at first request, not at build.
  * We couples non-K8s mode to controller-runtime's manager+cluster+cache+client+recorder, when today it only depends on the Runnable interface.
  * I think it will mess up code organization. Today we have two clean paths: 
    * setup() -- K8s mode,
    * runWithFileDiscovery() -- file mode For a specific mode you reads one function, top to bottom. The proposal merges them into one setup() that decides at every step which flavor to do. To express both modes in one function you need three flags simultaneously


So currently I prefer we keep the split, but happy to revisit if a third deployment mode lands or once K8s-as-plugin happens
### 
commented [May 25, 2026](https://github.com/llm-d/llm-d-router/pull/1218#issuecomment-4534822863)
Copy link  Copy Markdown 
Collaborator  
|  Sounds good, do we have an issue to track moving k8s discovery to a plugin?  |  
| --- |  
### 
commented [May 25, 2026](https://github.com/llm-d/llm-d-router/pull/1218#issuecomment-4535517249)
Copy link  Copy Markdown 
Contributor Author  
| 
> Sounds good, do we have an issue to track moving k8s discovery to a plugin?
Yes. [#910](https://github.com/llm-d/llm-d-router/issues/910) I added more info into it.  |  
| --- |  
[ezrasilvera](https://github.com/ezrasilvera) added 15 commits [May 25, 2026 20:59](https://github.com/llm-d/llm-d-router/pull/1218#commits-pushed-d8e42d0)
` runner: add file discovery path with RunnableGroup, resolveDiscovery,…[](https://github.com/llm-d/llm-d-router/pull/1218/commits/d8e42d0528fb24b53b71c6db0ffb03ca5b49673b "runner: add file discovery path with RunnableGroup, resolveDiscovery, and serveMetrics

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: hYuv32GGazlAD6dWQsWF6ak2JuZdqDrOnkR4AvZqw9Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
… and serveMetrics

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` gate request serving on EndpointDiscovery.Ready() to fix sync issues[](https://github.com/llm-d/llm-d-router/pull/1218/commits/600f2259b6af163fa5885a78cb1f83fd940fa010 "gate request serving on EndpointDiscovery.Ready\(\) to fix sync issues

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: hYuv32GGazlAD6dWQsWF6ak2JuZdqDrOnkR4AvZqw9Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` serve metrics from controller-runtime registry with bounded shutdown[](https://github.com/llm-d/llm-d-router/pull/1218/commits/dd954c4e1ce01c79fc1c48a41999dd6ab09786e2 "serve metrics from controller-runtime registry with bounded shutdown

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: hYuv32GGazlAD6dWQsWF6ak2JuZdqDrOnkR4AvZqw9Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` treat unset pool AppProtocol as no constraint in health check[](https://github.com/llm-d/llm-d-router/pull/1218/commits/13e0dbcbe13562081453bf7c0011398c230fe917 "treat unset pool AppProtocol as no constraint in health check

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: hYuv32GGazlAD6dWQsWF6ak2JuZdqDrOnkR4AvZqw9Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` log inactive Kubernetes-only features at file-discovery startup[](https://github.com/llm-d/llm-d-router/pull/1218/commits/78a3c2c989cfb8b75bc3961c826904f016e320c7 "log inactive Kubernetes-only features at file-discovery startup

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: hYuv32GGazlAD6dWQsWF6ak2JuZdqDrOnkR4AvZqw9Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` document pool namespace handling and warn on default fallback in file…[](https://github.com/llm-d/llm-d-router/pull/1218/commits/e8a20e0aabd20fcfd5d1490eaa09c302613d94b8 "document pool namespace handling and warn on default fallback in file mode

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: hYuv32GGazlAD6dWQsWF6ak2JuZdqDrOnkR4AvZqw9Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
… mode

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` avoid double parse and duplicate startup logs in K8s mode[](https://github.com/llm-d/llm-d-router/pull/1218/commits/1e49a8519cff48ac63c3c8aef47cc61df06772ad "avoid double parse and duplicate startup logs in K8s mode

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: hYuv32GGazlAD6dWQsWF6ak2JuZdqDrOnkR4AvZqw9Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` fix resolve_discovery_test.go imports for renamed module[](https://github.com/llm-d/llm-d-router/pull/1218/commits/db539b20d69aa0f590d956d4a61cdda449c9fb9c "fix resolve_discovery_test.go imports for renamed module

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: hYuv32GGazlAD6dWQsWF6ak2JuZdqDrOnkR4AvZqw9Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` docs(discovery): drop metricsHost references and tighten IPv4 constraint[](https://github.com/llm-d/llm-d-router/pull/1218/commits/bfc89bd447ec97ec33f2f15c67ed2b3ff009839b "docs\(discovery\): drop metricsHost references and tighten IPv4 constraint

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: hYuv32GGazlAD6dWQsWF6ak2JuZdqDrOnkR4AvZqw9Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` fix(runner): drop stray blank line in runWithFileDiscovery[](https://github.com/llm-d/llm-d-router/pull/1218/commits/88693be35b72ff328e6cb522992a21234db65aa6 "fix\(runner\): drop stray blank line in runWithFileDiscovery

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: hYuv32GGazlAD6dWQsWF6ak2JuZdqDrOnkR4AvZqw9Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` fix(runner): look up discovery plugin from handle instead of re-insta…[](https://github.com/llm-d/llm-d-router/pull/1218/commits/48b6e48fb652aba63e5b14e1eddc7b302f00458c "fix\(runner\): look up discovery plugin from handle instead of re-instantiating

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: hYuv32GGazlAD6dWQsWF6ak2JuZdqDrOnkR4AvZqw9Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
…ntiating

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` docs(runner): document phaseOne idempotency at the early-return guard[](https://github.com/llm-d/llm-d-router/pull/1218/commits/19d942558c21392cd574d65c5e1ee6afafb1635d "docs\(runner\): document phaseOne idempotency at the early-return guard

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: hYuv32GGazlAD6dWQsWF6ak2JuZdqDrOnkR4AvZqw9Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` refactor(profiling): share pprof handler set between K8s and file-dis…[](https://github.com/llm-d/llm-d-router/pull/1218/commits/221db791d6c07667559b0a0d1cb4974702752915 "refactor\(profiling\): share pprof handler set between K8s and file-discovery paths

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: hYuv32GGazlAD6dWQsWF6ak2JuZdqDrOnkR4AvZqw9Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
…covery paths

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` test(runner): add smoke test for runWithFileDiscovery wiring[](https://github.com/llm-d/llm-d-router/pull/1218/commits/44be34ed2099cb046513a1b912624be363c5b85d "test\(runner\): add smoke test for runWithFileDiscovery wiring

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: hYuv32GGazlAD6dWQsWF6ak2JuZdqDrOnkR4AvZqw9Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` fix(runner): adapt resolve_discovery_test to upstream *json.Decoder F…[](https://github.com/llm-d/llm-d-router/pull/1218/commits/bedffceff31818d47da47f7a655a15abc95cd70d "fix\(runner\): adapt resolve_discovery_test to upstream *json.Decoder Factory signature

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: hYuv32GGazlAD6dWQsWF6ak2JuZdqDrOnkR4AvZqw9Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1218).

```
…actory signature

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

[ezrasilvera](https://github.com/ezrasilvera) [force-pushed](https://github.com/llm-d/llm-d-router/compare/8fa1baf7f91ad8724dbc7af287da81ca5ddf96b1..bedffceff31818d47da47f7a655a15abc95cd70d) the  filediscovery-4 branch from [`8fa1baf`](https://github.com/llm-d/llm-d-router/commit/8fa1baf7f91ad8724dbc7af287da81ca5ddf96b1) to [`bedffce`](https://github.com/llm-d/llm-d-router/commit/bedffceff31818d47da47f7a655a15abc95cd70d) [ Compare ](https://github.com/llm-d/llm-d-router/compare/8fa1baf7f91ad8724dbc7af287da81ca5ddf96b1..bedffceff31818d47da47f7a655a15abc95cd70d) [May 25, 2026 18:06](https://github.com/llm-d/llm-d-router/pull/1218#event-25935810501)
### 
commented [May 25, 2026](https://github.com/llm-d/llm-d-router/pull/1218#issuecomment-4536648116)
Copy link  Copy Markdown 
Collaborator  
|  /approve  |  
| --- |  
[ahg-g](https://github.com/ahg-g) enabled auto-merge (squash) [May 25, 2026 19:20](https://github.com/llm-d/llm-d-router/pull/1218#event-25937581625)
approved these changes  [ May 25, 2026 ](https://github.com/llm-d/llm-d-router/pull/1218#pullrequestreview-4358431982)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1218/files/bedffceff31818d47da47f7a655a15abc95cd70d)
[github-project-automation](https://github.com/apps/github-project-automation) Bot moved this to **In review** in [llm-d-router](https://github.com/orgs/llm-d/projects/15) [May 25, 2026](https://github.com/llm-d/llm-d-router/pull/1218#event-25937584724)
Hide details View details
[ahg-g](https://github.com/ahg-g) merged commit [`bda7c04`](https://github.com/llm-d/llm-d-router/commit/bda7c04a9f8b4de93c71ed01a0990f0f7b5a3695) into  llm-d:main [May 25, 2026](https://github.com/llm-d/llm-d-router/pull/1218#event-25938744872)
14 checks passed 
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1218).
[github-project-automation](https://github.com/apps/github-project-automation) Bot moved this from **In review** to **Done** in [llm-d-router](https://github.com/orgs/llm-d/projects/15) [May 25, 2026](https://github.com/llm-d/llm-d-router/pull/1218#event-25938747504)
[elevran](https://github.com/elevran) mentioned this pull request [ May 26, 2026 ](https://github.com/llm-d/llm-d-router/pull/1218#ref-pullrequest-4521685324)
[ docs: backfill release-note fragments for PRs #1030, #1218, #1276 #1327 ](https://github.com/llm-d/llm-d-router/pull/1327)
Merged 
[ahg-g](https://github.com/ahg-g) mentioned this pull request [ May 26, 2026 ](https://github.com/llm-d/llm-d-router/pull/1218#ref-issue-4343818096)
[ Refactor the Pod watching to fit the Discovery plugin #910 ](https://github.com/llm-d/llm-d-router/issues/910)
[elevran](https://github.com/elevran) added a commit that referenced this pull request [ May 26, 2026 ](https://github.com/llm-d/llm-d-router/pull/1218#ref-commit-d7f1edc)
` docs: backfill release-note fragments for PRs[](https://github.com/llm-d/llm-d-router/commit/d7f1edc71cba7b6583e409588bea321140b3bcf9 "docs: backfill release-note fragments for PRs #1030, #1218, #1276 \(#1327\)

* docs: backfill release-note fragments for PRs #1030, #1218, #1276

These PRs merged after the fragment workflow went live but did not
include a release-note block in their description, so no fragment
was staged automatically. Add best-guess fragments derived from the
PR descriptions; authors can amend if the wording is off.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* docs: also reference docs/discovery.md in 1218 release note

Address review nit on PR #1327.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

---------

Signed-off-by: Etai Lev Ran <elevran@gmail.com>") #1030[](https://github.com/llm-d/llm-d-router/pull/1030),[](https://github.com/llm-d/llm-d-router/commit/d7f1edc71cba7b6583e409588bea321140b3bcf9 "docs: backfill release-note fragments for PRs #1030, #1218, #1276 \(#1327\)

* docs: backfill release-note fragments for PRs #1030, #1218, #1276

These PRs merged after the fragment workflow went live but did not
include a release-note block in their description, so no fragment
was staged automatically. Add best-guess fragments derived from the
PR descriptions; authors can amend if the wording is off.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* docs: also reference docs/discovery.md in 1218 release note

Address review nit on PR #1327.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

---------

Signed-off-by: Etai Lev Ran <elevran@gmail.com>") #1218[](https://github.com/llm-d/llm-d-router/pull/1218),[](https://github.com/llm-d/llm-d-router/commit/d7f1edc71cba7b6583e409588bea321140b3bcf9 "docs: backfill release-note fragments for PRs #1030, #1218, #1276 \(#1327\)

* docs: backfill release-note fragments for PRs #1030, #1218, #1276

These PRs merged after the fragment workflow went live but did not
include a release-note block in their description, so no fragment
was staged automatically. Add best-guess fragments derived from the
PR descriptions; authors can amend if the wording is off.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* docs: also reference docs/discovery.md in 1218 release note

Address review nit on PR #1327.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

---------

Signed-off-by: Etai Lev Ran <elevran@gmail.com>") #1276[](https://github.com/llm-d/llm-d-router/pull/1276) ([](https://github.com/llm-d/llm-d-router/commit/d7f1edc71cba7b6583e409588bea321140b3bcf9 "docs: backfill release-note fragments for PRs #1030, #1218, #1276 \(#1327\)

* docs: backfill release-note fragments for PRs #1030, #1218, #1276

These PRs merged after the fragment workflow went live but did not
include a release-note block in their description, so no fragment
was staged automatically. Add best-guess fragments derived from the
PR descriptions; authors can amend if the wording is off.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* docs: also reference docs/discovery.md in 1218 release note

Address review nit on PR #1327.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

---------

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")#1327[](https://github.com/llm-d/llm-d-router/pull/1327) `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1218).

```
)

* docs: backfill release-note fragments for PRs #1030[](https://github.com/llm-d/llm-d-router/pull/1030), #1218[](https://github.com/llm-d/llm-d-router/pull/1218), #1276[](https://github.com/llm-d/llm-d-router/pull/1276)

These PRs merged after the fragment workflow went live but did not
include a release-note block in their description, so no fragment
was staged automatically. Add best-guess fragments derived from the
PR descriptions; authors can amend if the wording is off.

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

* docs: also reference docs/discovery.md in 1218 release note

Address review nit on PR #1327[](https://github.com/llm-d/llm-d-router/pull/1327).

Signed-off-by: Etai Lev Ran <elevran@gmail.com>

---------

Signed-off-by: Etai Lev Ran <elevran@gmail.com>
```

[chcost](https://github.com/chcost) mentioned this pull request [ Jun 2, 2026 ](https://github.com/llm-d/llm-d-router/pull/1218#ref-issue-4460278591)
[ v0.8 Release Goals (Mid-June) llm-d/llm-d#1520 ](https://github.com/llm-d/llm-d/issues/1520)
Closed 
67 tasks
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fllm-d%2Fllm-d-router%2Fpull%2F1218)
###  Labels 
[ lgtm ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Algtm) "Looks good to me", indicates that a PR is ready to be merged. [ size/XXL ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FXXL) Denotes a PR that changes 1000+ lines, ignoring generated files.
###  4 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
