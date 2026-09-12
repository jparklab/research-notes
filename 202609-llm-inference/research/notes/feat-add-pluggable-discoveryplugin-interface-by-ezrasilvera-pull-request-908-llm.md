---
title: 'feat: Add pluggable DiscoveryPlugin interface by ezrasilvera · Pull Request
  #908 · llm-d/llm-d-router · GitHub'
id: feat-add-pluggable-discoveryplugin-interface-by-ezrasilvera-pull-request-908-llm
tags:
- llm-d
- non-k8s
- discovery-plugin
- endpoint-discovery
- epp-config
- ray
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T20:49:54.891369Z'
updated: '2026-09-12T03:37:37.707330Z'
source: https://github.com/llm-d/llm-d-router/pull/908
source_domain: github.com
fetched_at: '2026-09-11T20:49:54.863032Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'PRIMARY SOURCE, highest relevance to the research query. Merged PR #908
  (llm-d/llm-d-router), opened Apr 28 2026 by Ezra Silvera (IBM), merged May 3 2026
  by ahg-g at commit 0463fa5. This PR defines the exact extension point for adding
  non-Kubernetes backends to the llm-d router. Full code-review thread captured verbatim.


  THE INTERFACE (final form, after review): originally named DiscoveryPlugin at pkg/epp/framework/interface/discovery/discovery.go,
  renamed to EndpointDiscovery and physically MOVED to under the datalayer package
  (pkg/epp/framework/interface/datalayer/discovery.go) per the author''s own design
  rationale. Implementations must also satisfy fwkplugin.Plugin so they can register
  in the plugin registry and be selected via EndpointPickerConfig.discovery.pluginRef
  -- i.e. a custom non-k8s discovery backend is enabled purely by config, no fork
  of the router needed once the plugin is registered. Core method: Start(ctx context.Context,
  notifier Notifier) error -- expected to enumerate all known endpoints via notifier.Upsert
  on startup (loosened during review from a hard requirement to a guideline) and then
  block, continuing to watch for changes until ctx is cancelled or a fatal error occurs;
  the PR explicitly leaves concurrency management (whether Start spawns its own goroutine)
  to the plugin''s discretion, documented via a code comment added during review.
  The Notifier interface exposes Upsert(endpoint *fwkdl.EndpointMetadata) and Delete(id
  types.NamespacedName); review comments established the notifier is NOT goroutine-safe
  and plugin implementations MUST preserve event ordering (e.g. an Upsert followed
  by a Delete for the same endpoint must arrive in that order or the endpoint is incorrectly
  left in the datastore).


  *** THE SINGLE MOST DIRECTLY RESPONSIVE QUOTE TO THE RESEARCH QUERY ***, from the
  PR author (Ezra Silvera) explaining why Discovery must be a separate, pluggable
  component from the datalayer: "We aim to also support non k8s in the future so we
  need to keep the datalayer clean: we want K8s, file, Slurm, and Ray discovery all
  as plugins. If discovery lives inside the datalayer, the datalayer gets contaminated
  with infrastructure-specific code (ctrl.Manager, fsnotify, etc.), which defeats
  the whole point of the abstraction." This CONFIRMS that (a) the llm-d-router maintainers
  designed the discovery abstraction specifically to be backend-agnostic across Kubernetes,
  flat files, Slurm, AND Ray, and (b) Kubernetes-specific machinery (ctrl.Manager,
  i.e. controller-runtime) lives ONLY inside the k8s discovery plugin implementation,
  not in the shared datalayer/notifier code path -- meaning a from-scratch non-k8s
  DiscoveryPlugin implementation does not need to touch or depend on any Kubernetes
  client libraries.


  The author also articulated the architectural boundary precisely: "Discovery is
  upstream of the datalayer, not a peer, the datalayer only activates per endpoint
  AFTER discovery creates it... Discovery answers ''which endpoints exist?'' [while]
  the datalayer answers ''what is the state of known endpoints?'' (metrics, KV cache,
  queue depth)" -- i.e. a custom backend integration needs a Discovery plugin (topology/existence)
  and can reuse the existing datalayer machinery (health/metrics polling) unmodified,
  PROVIDED the backend exposes compatible metrics endpoints.


  One open design question flagged but explicitly NOT resolved in this PR: a reviewer
  asked "I wonder if this is not too k8s centric and we should include an explicit
  UniqueID in the endpoint metadata?" referring to the use of Kubernetes'' types.NamespacedName
  as the endpoint identifier type in Delete(id types.NamespacedName) -- the author
  agreed this is a real gap ("several other places using NamespacedName and we should
  switch to ID") but deferred it as out of scope, meaning as of this PR the endpoint
  identifier type itself still carries a k8s-shaped type even though the interface
  is otherwise backend-agnostic; downstream PRs (e.g. the file-discovery series #1080/#1081)
  may or may not have resolved this -- worth checking.


  Linked follow-on/related work visible in the PR thread: issue #910 "Refactor the
  Pod watching to fit the Discovery plugin" (making the existing Kubernetes pod-watch
  conform to the new interface), issue #911 "Implement a File discovery plugin", and
  a related closed PR #1007 "Discovery Plugins: K8s and File-based Endpoint Discovery".'
---

*Suggested by [[v08-release-goals-mid-june-issue-1520-llm-dllm-d]] — first PR in the file-discovery / non-k8s implementation chain: pluggable DiscoveryPlugin interface*

[Skip to content](https://github.com/llm-d/llm-d-router/pull/908#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/908) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/908) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/908) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/908).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Conversation
### 
commented [Apr 28, 2026](https://github.com/llm-d/llm-d-router/pull/908#issue-4342902246)
Copy link  Copy Markdown 
Contributor
**What type of PR is this?**
**What this PR does / why we need it** : This PR addresses [#907](https://github.com/llm-d/llm-d-router/issues/907)
**Which issue(s) this PR fixes** :
Fixes #
**Release note** _(write`NONE` if no user-facing change)_:

```
NONE

```

[ezrasilvera](https://github.com/ezrasilvera) added 2 commits [April 28, 2026 14:28](https://github.com/llm-d/llm-d-router/pull/908#commits-pushed-f7f074e)
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
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/908).

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

[github-project-automation](https://github.com/apps/github-project-automation) Bot added this to [llm-d-router](https://github.com/orgs/llm-d/projects/15) [Apr 28, 2026](https://github.com/llm-d/llm-d-router/pull/908#event-24945992539)
[github-actions](https://github.com/apps/github-actions) Bot requested review from [ahg-g](https://github.com/ahg-g) and [shmuelk](https://github.com/shmuelk) [April 28, 2026 11:52](https://github.com/llm-d/llm-d-router/pull/908#event-24945994419)
### 
commented [Apr 28, 2026](https://github.com/llm-d/llm-d-router/pull/908#issuecomment-4335709970)
Copy link  Copy Markdown 
Contributor  
|  /lgtm  |  
| --- |  
[github-actions](https://github.com/apps/github-actions) Bot added the [ lgtm ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Algtm) "Looks good to me", indicates that a PR is ready to be merged. label [Apr 28, 2026](https://github.com/llm-d/llm-d-router/pull/908#event-24949783864)
This was referenced Apr 28, 2026
[ Refactor the Pod watching to fit the Discovery plugin #910 ](https://github.com/llm-d/llm-d-router/issues/910)
[ Implement a File discovery plugin #911 ](https://github.com/llm-d/llm-d-router/issues/911)
Closed 
` change Upsert to add single endpoint and not slice[](https://github.com/llm-d/llm-d-router/pull/908/commits/5150a99ec3261d85c3806c9c5bd9179c60cd9c0b "change Upsert to add single endpoint and not slice") `
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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/908).
reviewed  [ Apr 30, 2026 ](https://github.com/llm-d/llm-d-router/pull/908#pullrequestreview-4204439481)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/908/files/5150a99ec3261d85c3806c9c5bd9179c60cd9c0b)
Comment thread [pkg/epp/framework/interface/discovery/discovery.go](https://github.com/llm-d/llm-d-router/pull/908/files/5150a99ec3261d85c3806c9c5bd9179c60cd9c0b#diff-ae2530730624617581b19f5b90945fbe249081764d7ea5983d8779706fac8007) Outdated   
|  // DiscoveryPlugin discovers inference endpoints and drives their lifecycle in the datastore.  |  
| --- |  
|  // Implementations must also implement fwkplugin.Plugin so they can be registered in the  |  
|  // plugin registry and selected via EndpointPickerConfig.discovery.pluginRef.  |  
|  type DiscoveryPlugin interface {  |  
### 
[Apr 30, 2026](https://github.com/llm-d/llm-d-router/pull/908#discussion_r3167328954)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Instead of noting this in the comment, it would be better to embed the `Plugin` interface here so the requirement is enforced by the compiler.
### 
[Apr 30, 2026](https://github.com/llm-d/llm-d-router/pull/908#discussion_r3168320510)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Agree. Done.
Comment thread [pkg/epp/framework/interface/discovery/discovery.go](https://github.com/llm-d/llm-d-router/pull/908/files/5150a99ec3261d85c3806c9c5bd9179c60cd9c0b#diff-ae2530730624617581b19f5b90945fbe249081764d7ea5983d8779706fac8007) Outdated 
Comment on lines to   
|  // 1. Enumerate all known endpoints, calling notifier.Upsert for each.  |  
| --- |  
|  // 2. Continue watching for changes until ctx is cancelled.  |  
### 
[Apr 30, 2026](https://github.com/llm-d/llm-d-router/pull/908#discussion_r3167340835)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I think these are implementation details and specific plugin implementations shuold be allowed to deviate. For example, is (1) really required or can it be handled as part of normal life cycle updates as in (2)?
### 
[Apr 30, 2026](https://github.com/llm-d/llm-d-router/pull/908#discussion_r3168308151)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
The requirement is here because without initial enumeration, the datastore starts empty and requests fail until the first event arrives. I will loosened from a hard requirement to a guideline
Comment thread [pkg/epp/framework/interface/discovery/discovery.go](https://github.com/llm-d/llm-d-router/pull/908/files/5150a99ec3261d85c3806c9c5bd9179c60cd9c0b#diff-ae2530730624617581b19f5b90945fbe249081764d7ea5983d8779706fac8007) Outdated   
|  // 1. Enumerate all known endpoints, calling notifier.Upsert for each.  |  
| --- |  
|  // 2. Continue watching for changes until ctx is cancelled.  |  
|  // Blocks until ctx is cancelled or a fatal error occurs.  |  
|  Start(ctx context.Context, notifier Notifier) error  |  
### 
[Apr 30, 2026](https://github.com/llm-d/llm-d-router/pull/908#discussion_r3167348303)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
who manages the concurrency - is `Start()` expected to be called in a new goroutine or should it create one internally?
### 
[Apr 30, 2026](https://github.com/llm-d/llm-d-router/pull/908#discussion_r3168322938)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Agree. Added comment
Comment thread [pkg/epp/framework/interface/datalayer/discovery.go](https://github.com/llm-d/llm-d-router/pull/908/files/5150a99ec3261d85c3806c9c5bd9179c60cd9c0b#diff-1bdd08ca7bc51cbf0396e9fac08dab387f58e52d20231e7d59502a8c5b67784b)  
|  // they are received. Plugin implementations MUST preserve event order -- do not  |  
| --- |  
|  // buffer or dispatch calls concurrently in a way that could reorder them. For  |  
|  // example, an Upsert followed by a Delete for the same endpoint must arrive in  |  
|  // that order, or the endpoint will be incorrectly left in the datastore.  |  
### 
[Apr 30, 2026](https://github.com/llm-d/llm-d-router/pull/908#discussion_r3167354165)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Add a note that notifier is not expected to be go routine safe? Hence the ordering requirement.
### 
[Apr 30, 2026](https://github.com/llm-d/llm-d-router/pull/908#discussion_r3168324546)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Added
Comment thread [pkg/epp/framework/interface/datalayer/discovery.go](https://github.com/llm-d/llm-d-router/pull/908/files/5150a99ec3261d85c3806c9c5bd9179c60cd9c0b#diff-1bdd08ca7bc51cbf0396e9fac08dab387f58e52d20231e7d59502a8c5b67784b)  
|  // Upsert adds or updates an endpoint in the datastore.  |  
| --- |  
|  Upsert(endpoint *fwkdl.EndpointMetadata)  |  
|  // Delete removes an endpoint from the datastore by its namespaced name.  |  
|  Delete(id types.NamespacedName)  |  
### 
[Apr 30, 2026](https://github.com/llm-d/llm-d-router/pull/908#discussion_r3167360580)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I wonder if this is not too k8s centric and we should include an explicit `UniqueID` in the endpoint metadata? Not part of this PR.
### 
[Apr 30, 2026](https://github.com/llm-d/llm-d-router/pull/908#discussion_r3168330315)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I agree. But it's wider than this instance. several other places using NamespacedName and we should switch to ID
[elevran](https://github.com/elevran) removed the [ lgtm ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Algtm) "Looks good to me", indicates that a PR is ready to be merged. label [Apr 30, 2026](https://github.com/llm-d/llm-d-router/pull/908#event-25030148202)
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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/908).
### 
commented [Apr 30, 2026](https://github.com/llm-d/llm-d-router/pull/908#issuecomment-4352945255)
Copy link  Copy Markdown 
Contributor Author  
|  [@elevran](https://github.com/elevran) Thanks for the review. I addressed all comments.  |  
| --- |  
reviewed  [ Apr 30, 2026 ](https://github.com/llm-d/llm-d-router/pull/908#pullrequestreview-4205759367)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/908/files/723e87d818bbe83caa8dc664425b35566b8e2fb1)
Comment thread [pkg/epp/framework/interface/discovery/discovery.go](https://github.com/llm-d/llm-d-router/pull/908/files/723e87d818bbe83caa8dc664425b35566b8e2fb1#diff-ae2530730624617581b19f5b90945fbe249081764d7ea5983d8779706fac8007) Outdated   
|  // DiscoveryPlugin discovers inference endpoints and drives their lifecycle in the datastore.  |  
| --- |  
|  // Implementations are registered in the plugin registry and selected via  |  
|  // EndpointPickerConfig.discovery.pluginRef.  |  
|  type DiscoveryPlugin interface {  |  
### 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
We don't need to suffix it with a "Plugin"
reviewed  [ Apr 30, 2026 ](https://github.com/llm-d/llm-d-router/pull/908#pullrequestreview-4205839112)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/908/files/723e87d818bbe83caa8dc664425b35566b8e2fb1)
### 
left a comment 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Suggestion:
  1. we name it EndpointManager (in the spirit of naming plugins using agent nouns).
  2. place it under the datalayer acomponent. Which we defined as the layer that performs async data syncing.


### 
commented [Apr 30, 2026](https://github.com/llm-d/llm-d-router/pull/908#issuecomment-4354556698) •
edited by elevran 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/908).
Copy link  Copy Markdown 
Contributor Author  
| 
>   1. we name it EndpointManager (in the spirit of naming plugins using agent nouns).
> 

EndpointManager implies it directly manages endpoints (deploy/remove them etc...). How about EndpointDiscovery ?  |  
| --- |  
### 
commented [Apr 30, 2026](https://github.com/llm-d/llm-d-router/pull/908#issuecomment-4355935042)
Copy link  Copy Markdown 
Collaborator  
| 
> >   1. we name it EndpointManager (in the spirit of naming plugins using agent nouns). EndpointManager implies it directly manages endpoints (deploy/remove them etc...). How about EndpointDiscovery ?
>> 

Sounds good  |  
| --- |  
` Rename interface to EndpointDiscovery[](https://github.com/llm-d/llm-d-router/pull/908/commits/bfb2807e362cb45809b6a13ac4523647de7eabfa "Rename interface to EndpointDiscovery") `
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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/908).
### 
commented [May 1, 2026](https://github.com/llm-d/llm-d-router/pull/908#issuecomment-4359713163) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/908).
Copy link  Copy Markdown 
Contributor Author  
|  [@ahg-g](https://github.com/ahg-g) renamed to EndpointDiscovery.
> Regarding the suggestion to place it under the datalayer component, I have some concerns:
>   1. The datalayer and discovery have different responsibilities: the datalayer answers "what is the state of **known** endpoints?" (metrics, KV cache, queue depth). Discovery answers "which endpoints exist?". These are fundamentally different concerns -- topology management vs gather endpoint data.
>   2. We aim to also support non k8s in the future so we need to keep the datalayer clean: we want K8s, file, Slurm, and Ray discovery all as plugins. If discovery lives inside the datalayer, the datalayer gets contaminated with infrastructure-specific code (ctrl.Manager, fsnotify, etc.), which defeats the whole point of the abstraction.
>   3. Discovery is upstream of the datalayer, not a peer, the datalayer only activates per endpoint AFTER discovery creates it. Putting the thing that creates endpoints inside the thing that depends on those endpoints existing is an inversion. the fact that discovery triggers datalayer activity is expected and correct: any data pipeline has a source that feeds into a processor. That does not mean the source and the processor should live in the same component. Keeping them separate is what allows each to evolve independently
>   4. EndpointSource vs EndpointDiscovery are opposite in direction Same word "endpoint events", opposite flow. EndpointSource: datastore event --> outward --> consumers EndpointDiscovery: outside world --> inward --> datastore
> 

[@ahg-g](https://github.com/ahg-g) [@elevran](https://github.com/elevran) After some more thinking, this will help in the plugin implementation for k8s. I moved the interface to under the datalayer. With this I addressed all review comments  |  
| --- |  
` move the discovery interface to the datalayer[](https://github.com/llm-d/llm-d-router/pull/908/commits/e0b63563b28068ab3344a244f3d94b184eea8f22 "move the discovery interface to the datalayer") `
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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/908).
### 
commented [May 3, 2026](https://github.com/llm-d/llm-d-router/pull/908#issuecomment-4366439743)
Copy link  Copy Markdown 
Contributor Author  
|  [@ahg-g](https://github.com/ahg-g) [@elevran](https://github.com/elevran) After some more thinking, I moved the interface to under the datalayer, this will also help in the plugin implementation for k8s. With this I addressed all review comments. Thanks for your feedback  |  
| --- |  
### 
commented [May 3, 2026](https://github.com/llm-d/llm-d-router/pull/908#issuecomment-4366551079)
Copy link  Copy Markdown 
Collaborator  
|  /lgtm will leave approval to [@ahg-g](https://github.com/ahg-g)  |  
| --- |  
[github-actions](https://github.com/apps/github-actions) Bot added the [ lgtm ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Algtm) "Looks good to me", indicates that a PR is ready to be merged. label [May 3, 2026](https://github.com/llm-d/llm-d-router/pull/908#event-25109159975)
### 
commented [May 3, 2026](https://github.com/llm-d/llm-d-router/pull/908#issuecomment-4366926920)
Copy link  Copy Markdown 
Collaborator  
|  /approve  |  
| --- |  
approved these changes  [ May 3, 2026 ](https://github.com/llm-d/llm-d-router/pull/908#pullrequestreview-4216706824)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/908/files/e0b63563b28068ab3344a244f3d94b184eea8f22)
[github-project-automation](https://github.com/apps/github-project-automation) Bot moved this to **In review** in [llm-d-router](https://github.com/orgs/llm-d/projects/15) [May 3, 2026](https://github.com/llm-d/llm-d-router/pull/908#event-25111261718)
Hide details View details
[ahg-g](https://github.com/ahg-g) merged commit [`0463fa5`](https://github.com/llm-d/llm-d-router/commit/0463fa5c38f37c9a6b47f774982f7318d1355f04) into  llm-d:main [May 3, 2026](https://github.com/llm-d/llm-d-router/pull/908#event-25111260684)
7 checks passed 
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/908).
[github-project-automation](https://github.com/apps/github-project-automation) Bot moved this from **In review** to **Done** in [llm-d-router](https://github.com/orgs/llm-d/projects/15) [May 3, 2026](https://github.com/llm-d/llm-d-router/pull/908#event-25111261965)
[ezrasilvera](https://github.com/ezrasilvera) mentioned this pull request [ May 6, 2026 ](https://github.com/llm-d/llm-d-router/pull/908#ref-pullrequest-4393608552)
[ Discovery Plugins: K8s and File-based Endpoint Discovery #1007 ](https://github.com/llm-d/llm-d-router/pull/1007)
Closed 
[chcost](https://github.com/chcost) mentioned this pull request [ Jun 2, 2026 ](https://github.com/llm-d/llm-d-router/pull/908#ref-issue-4460278591)
[ v0.8 Release Goals (Mid-June) llm-d/llm-d#1520 ](https://github.com/llm-d/llm-d/issues/1520)
Closed 
67 tasks
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fllm-d%2Fllm-d-router%2Fpull%2F908)
###  Labels 
[ lgtm ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Algtm) "Looks good to me", indicates that a PR is ready to be merged.
###  5 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
