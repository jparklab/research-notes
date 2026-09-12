---
title: 'file-discovery (3/4):  add FileDiscovery plugin with YAML/JSON file loading
  and fsnotify watch by ezrasilvera · Pull Request #1135 · llm-d/llm-d-router · GitHub'
id: file-discovery-34-add-filediscovery-plugin-with-yamljson-file-loading-and-fsnoti
tags:
- llm-d
- non-k8s
- file-discovery
- llmd-router-nonk8s-backends-54b774
- filediscovery
- configmap
created: '2026-09-11T20:52:10.934040Z'
updated: '2026-09-12T03:37:37.719091Z'
source: https://github.com/llm-d/llm-d-router/pull/1135
source_domain: github.com
fetched_at: '2026-09-11T20:52:10.910971Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'PRIMARY SOURCE. Merged PR #1135 (llm-d/llm-d-router), "file-discovery (3/4):
  add FileDiscovery plugin with YAML/JSON file loading and fsnotify watch", opened
  May 14 2026 by Ezra Silvera, merged May 19 2026 by ahg-g at commit 25b2584. Third
  of a four-part PR series adding file-based discovery "allowing the EPP to run without
  Kubernetes"; the author states file-based discovery is "the first phase in re-creating
  the functionality of #1007" (an earlier closed PR, "Discovery Plugins: K8s and File-based
  Endpoint Discovery"). This PR implements FileDiscovery, described in the PR body
  as "the first EndpointDiscovery backend for non-Kubernetes deployments."


  BEHAVIOR: FileDiscovery reads a YAML or JSON file listing inference endpoints and
  drives their lifecycle in the datastore via DiscoveryNotifier. On startup it loads
  endpoints from the file and emits one Upsert per entry so the datastore is populated
  before the EPP begins serving traffic. It can optionally watch the file with fsnotify
  and reconcile additions/removals on every write -- "allowing zero-downtime endpoint
  updates without restarting the EPP." On reload it diffs the previous snapshot against
  new file contents and removes stale endpoints. Configured via EndpointPickerConfig.dataLayer.discovery.pluginRef
  pointing to a plugin instance of type "file-discovery" with "path" and "watchFile"
  params. NOTE (important caveat): as of this PR the plugin is "fully implemented
  and tested in isolation but is not yet usable end-to-end" -- the runner wiring,
  config detection, and runWithFileDiscovery execution path shipped separately in
  the follow-on PR #1218 (4/4); until #1218 merged, setting dataLayer.discovery.pluginRef
  in the config had no effect.


  CONFIG SCHEMA (evolved during review): the per-endpoint YAML/JSON schema is EndpointEntry{
  Name string; Namespace string (optional); Address string; Port string }. A "MetricsHost"
  field was added, then briefly renamed to "MetricsPort" to let operators point metrics
  scraping at a different port than inference traffic (motivated by "vLLM running
  inference on :8000 but /metrics on :9090"), then a reviewer pointed out the EPP
  "can not scrape and serve on different IPs, only same IP and different port is feasible
  (and deprecated)" -- the author ultimately DROPPED the metrics-port override entirely,
  so the final shipped behavior always scrapes Prometheus metrics from the same address:port
  the endpoint serves inference on, with no way to configure a separate metrics port
  via the file-discovery plugin as merged.


  NOTABLE LIMITATION: a commit titled "file-discovery: validate IPv4 only and reject
  IPv6 addresses" shows the plugin explicitly rejects IPv6 endpoint addresses -- IPv6
  backends cannot be registered via file-discovery as of this PR.


  K8S RESIDUE: despite being the non-Kubernetes discovery backend, the internal endpoint-identity
  tracking still uses Kubernetes'' types.NamespacedName as the map key type (`endpoints
  map[types.NamespacedName]struct{}`, renamed from `current` during review) -- confirming
  the k8s-shaped identifier gap flagged as unresolved in PR #908 persisted into this
  implementation. The plugin also depends on "sigs.k8s.io/yaml" for YAML parsing (chosen
  because it "converts all to json" so json struct tags alone suffice) -- a Kubernetes-ecosystem
  library, though used here purely as a YAML-to-JSON codec with no live cluster dependency.


  RESILIENCE FIXES made during review (both by reviewer nilig/vMaroon, addressed by
  author): (1) a "partial-Upsert atomicity bug that can leave zombie endpoints in
  the datastore" during file load; (2) "missing watcher re-add that breaks watchFile
  mode after atomic renames" -- fixed via a commit "re-attach fsnotify watcher after
  atomic rename and ConfigMap rotation." That commit message explicitly names ConfigMap
  rotation as a scenario the plugin must tolerate, implying the file-discovery plugin
  is also designed to work when the backing file is mounted into a Kubernetes pod
  from a ConfigMap (which Kubernetes updates via atomic symlink swap) -- i.e. file-discovery
  is usable BOTH fully outside Kubernetes (e.g. a flat file on a Slurm node) AND inside
  a Kubernetes pod as an alternative to the native K8s/InferencePool discovery plugin.
  Additionally, the author changed reload behavior to "skip invalid endpoints on reload
  instead of aborting the entire load," and added a "README for file-discovery plugin"
  following the project''s plugin-documentation template (issue #917), plus a promised
  follow-up discovery.md describing the overall discovery-plugin framework.


  Package path: pkg/epp/framework/plugins/datalayer/discovery/file/plugin.go (module
  path is github.com/llm-d/llm-d-router, following the project''s earlier rename from
  llm-d-inference-scheduler). Immediate successor PR referenced in the thread: #1218
  "FileDiscovery plugin (4/4) - wiring into the runner" (merged).'
---

*Suggested by [[v08-release-goals-mid-june-issue-1520-llm-dllm-d]] — core FileDiscovery plugin implementation PR (3/4 of the file-discovery series)*

[Skip to content](https://github.com/llm-d/llm-d-router/pull/1135#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/1135) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/1135) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/1135) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Conversation
### 
commented [May 14, 2026](https://github.com/llm-d/llm-d-router/pull/1135#issue-4448056219) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).
Copy link  Copy Markdown 
Contributor
This PR is the third of 4 PRs adding file based discovery allowing the EPP to run without Kubernetes. The file based discovery is the first phase in re-creating the functionality of [#1007](https://github.com/llm-d/llm-d-router/pull/1007).
This PR implements the FileDiscovery plugin, the first EndpointDiscovery backend for non-Kubernetes deployments. It reads a YAML or JSON file that lists inference endpoints and drives their lifecycle in the datastore via DiscoveryNotifier.
**Note** : The FileDiscovery plugin is fully implemented and tested in isolation but is not yet usable end-to-end. The runner wiring plugin registration, config detection, and the runWithFileDiscovery execution path will land in the next (and final) PR. Until then setting dataLayer.discovery.pluginRef in the config will have no effect.
  * Loads endpoints from a YAML/JSON file at startup, emitting one Upsert per entry so the datastore is populated before the EPP begins serving traffic
  * Optionally watches the file with fsnotify and reconciles additions/removals on every write, allowing zero-downtime endpoint updates without restarting the EPP
  * Removes stale endpoints on reload by diffing the previous snapshot against the new file contents
  * Configured via EndpointPickerConfig.dataLayer.discovery.pluginRef pointing to a plugin instance of type file-discovery with path and watchFile params


[github-project-automation](https://github.com/apps/github-project-automation) Bot added this to [llm-d-router](https://github.com/orgs/llm-d/projects/15) [May 14, 2026](https://github.com/llm-d/llm-d-router/pull/1135#event-25541202705)
[github-actions](https://github.com/apps/github-actions) Bot added the [ size/L ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files. label [May 14, 2026](https://github.com/llm-d/llm-d-router/pull/1135#event-25541213978)
[github-actions](https://github.com/apps/github-actions) Bot requested review from [nilig](https://github.com/nilig) and [vMaroon](https://github.com/vMaroon) [May 14, 2026 17:47](https://github.com/llm-d/llm-d-router/pull/1135#event-25541215106)
reviewed  [ May 14, 2026 ](https://github.com/llm-d/llm-d-router/pull/1135#pullrequestreview-4292539907)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1135/files)
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/file/file_discovery.go](https://github.com/llm-d/llm-d-router/pull/1135/files#diff-927d0a721d28cbf8f89ce29acccbbb9e42e1552c74650ab3b5552c49b1fdb80d) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).
reviewed  [ May 14, 2026 ](https://github.com/llm-d/llm-d-router/pull/1135#pullrequestreview-4292406293)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1135/files)
### 
left a comment 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Thanks [@ezrasilvera](https://github.com/ezrasilvera) Two issues in `load()` worth fixing before merging:
  * a partial-Upsert atomicity bug that can leave zombie endpoints in the datastore; and
  * missing watcher re-add that breaks `watchFile` mode after atomic renames See inline.


Comment thread [pkg/epp/framework/plugins/datalayer/discovery/file/plugin.go](https://github.com/llm-d/llm-d-router/pull/1135/files#diff-01f54de51a40d0be227178ff8afda2e1852d013d7e1190201f49e98ba2a3fabf) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/file/plugin.go](https://github.com/llm-d/llm-d-router/pull/1135/files#diff-01f54de51a40d0be227178ff8afda2e1852d013d7e1190201f49e98ba2a3fabf) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).
[github-actions](https://github.com/apps/github-actions) Bot added [ size/XL ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FXL) Denotes a PR that changes 500-999 lines, ignoring generated files. and removed [ size/L ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files. labels [May 15, 2026](https://github.com/llm-d/llm-d-router/pull/1135#event-25563723343)
### 
commented [May 15, 2026](https://github.com/llm-d/llm-d-router/pull/1135#issuecomment-4461094778)
Copy link  Copy Markdown 
Contributor Author  
|  Once [#1150](https://github.com/llm-d/llm-d-router/pull/1150) is merged I'll remove my docs commit and rebase.  |  
| --- |  
### 
commented [May 15, 2026](https://github.com/llm-d/llm-d-router/pull/1135#issuecomment-4462425365)
Copy link  Copy Markdown 
Contributor Author  
|  /hold Need to rebase to [#1154](https://github.com/llm-d/llm-d-router/pull/1154) once it's merged  |  
| --- |  
[github-actions](https://github.com/apps/github-actions) Bot added the [ hold ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Ahold) PRs that are blocked on design, other features, release cycle, etc. label [May 15, 2026](https://github.com/llm-d/llm-d-router/pull/1135#event-25588826547)
[ezrasilvera](https://github.com/ezrasilvera) [force-pushed](https://github.com/llm-d/llm-d-router/compare/c3161e9269fdb5acd2f9c9ba316868b22e9216e8..c54763cbcb7e1403d2a2f0389b54e0a23b9625a3) the  filediscovery-3 branch from [`c3161e9`](https://github.com/llm-d/llm-d-router/commit/c3161e9269fdb5acd2f9c9ba316868b22e9216e8) to [`c54763c`](https://github.com/llm-d/llm-d-router/commit/c54763cbcb7e1403d2a2f0389b54e0a23b9625a3) [ Compare ](https://github.com/llm-d/llm-d-router/compare/c3161e9269fdb5acd2f9c9ba316868b22e9216e8..c54763cbcb7e1403d2a2f0389b54e0a23b9625a3) [May 16, 2026 12:33](https://github.com/llm-d/llm-d-router/pull/1135#event-25608175861)
### 
commented [May 16, 2026](https://github.com/llm-d/llm-d-router/pull/1135#issuecomment-4466867485)
Copy link  Copy Markdown 
Contributor Author  
|  /unhold  |  
| --- |  
### 
commented [May 16, 2026](https://github.com/llm-d/llm-d-router/pull/1135#issuecomment-4466869061)
Copy link  Copy Markdown 
Contributor Author  
|  [@elevran](https://github.com/elevran) I addressed all comments. Thanks  |  
| --- |  
[ezrasilvera](https://github.com/ezrasilvera) [force-pushed](https://github.com/llm-d/llm-d-router/compare/c54763cbcb7e1403d2a2f0389b54e0a23b9625a3..8b528a4b7eda4c28e15df5dbcdbd53c5e4966f57) the  filediscovery-3 branch from [`c54763c`](https://github.com/llm-d/llm-d-router/commit/c54763cbcb7e1403d2a2f0389b54e0a23b9625a3) to [`8b528a4`](https://github.com/llm-d/llm-d-router/commit/8b528a4b7eda4c28e15df5dbcdbd53c5e4966f57) [ Compare ](https://github.com/llm-d/llm-d-router/compare/c54763cbcb7e1403d2a2f0389b54e0a23b9625a3..8b528a4b7eda4c28e15df5dbcdbd53c5e4966f57) [May 16, 2026 12:42](https://github.com/llm-d/llm-d-router/pull/1135#event-25608278474)
reviewed  [ May 17, 2026 ](https://github.com/llm-d/llm-d-router/pull/1135#pullrequestreview-4306234299)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1135/files)
### 
left a comment 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
pls add a README.md file to document the plugin, see other plugins for examples or [#917](https://github.com/llm-d/llm-d-router/issues/917) for a suggested template..
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/file/plugin.go](https://github.com/llm-d/llm-d-router/pull/1135/files#diff-01f54de51a40d0be227178ff8afda2e1852d013d7e1190201f49e98ba2a3fabf) Outdated   
|  "sigs.k8s.io/yaml"  |  
| --- |  
|  fwkdl "github.com/llm-d/llm-d-inference-scheduler/pkg/epp/framework/interface/datalayer"  |  
|  fwkplugin "github.com/llm-d/llm-d-inference-scheduler/pkg/epp/framework/interface/plugin"  |  
### 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
pls update the path `llm-d-router`
### 
[May 18, 2026](https://github.com/llm-d/llm-d-router/pull/1135#discussion_r3258302313) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Oh that happened over the last couple of days. Rebased and fixed in [`37b5139`](https://github.com/llm-d/llm-d-router/commit/37b5139d30e16d4d61f02235c097f7d127137aa1)
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/file/plugin.go](https://github.com/llm-d/llm-d-router/pull/1135/files#diff-01f54de51a40d0be227178ff8afda2e1852d013d7e1190201f49e98ba2a3fabf) Outdated   
|  type params struct {  |  
| --- |  
|  Path string `json:"path"`  |  
|  WatchFile bool `json:"watchFile"`  |  
### 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
pls document the parameters, this is a user facing api
### 
[May 18, 2026](https://github.com/llm-d/llm-d-router/pull/1135#discussion_r3258019916)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Done. [`2295a19`](https://github.com/llm-d/llm-d-router/commit/2295a193439baaf1af85d89f91579b17b5f6bee1)
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/file/plugin.go](https://github.com/llm-d/llm-d-router/pull/1135/files#diff-01f54de51a40d0be227178ff8afda2e1852d013d7e1190201f49e98ba2a3fabf) Outdated   
|  typedName fwkplugin.TypedName  |  
| --- |  
|  path string  |  
|  watchFile bool  |  
|  current map[types.NamespacedName]struct{}  |  
### 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
pls add a comment documenting what this map is for and the value it tracks.
### 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
if this is tracking the endpoints, perhaps name it `endpoints`?
### 
[May 18, 2026](https://github.com/llm-d/llm-d-router/pull/1135#discussion_r3258352958)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Done both in [`06986a0`](https://github.com/llm-d/llm-d-router/commit/06986a05f837a9a2f45a61e51c37cf6aafcd8223)
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/file/plugin.go](https://github.com/llm-d/llm-d-router/pull/1135/files#diff-01f54de51a40d0be227178ff8afda2e1852d013d7e1190201f49e98ba2a3fabf)  
|  // Package file provides a file-based EndpointDiscovery implementation that reads  |  
| --- |  
|  // a YAML (or JSON) file listing inference endpoints.  |  
|  package file  |  
### 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
nit: we could drop the file_ prefix from file_discovery.go, the pkg is already named file; in fact we can just name it plugin.go
### 
[May 18, 2026](https://github.com/llm-d/llm-d-router/pull/1135#discussion_r3257895862)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Sounds reasonable. Fixed in [`624dc48`](https://github.com/llm-d/llm-d-router/commit/624dc48ae6711d24d3ce188b11a509340adf3685)
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/file/plugin.go](https://github.com/llm-d/llm-d-router/pull/1135/files#diff-01f54de51a40d0be227178ff8afda2e1852d013d7e1190201f49e98ba2a3fabf) Outdated   
|  Namespace string `json:"namespace,omitempty" yaml:"namespace,omitempty"`  |  
| --- |  
|  Address string `json:"address" yaml:"address"`  |  
|  Port string `json:"port" yaml:"port"`  |  
|  MetricsHost string `json:"metricsHost,omitempty" yaml:"metricsHost,omitempty"`  |  
### 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
What is a "Metrics Host"?
### 
[May 18, 2026](https://github.com/llm-d/llm-d-router/pull/1135#discussion_r3257802475) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Added a comment to explain [`c6901f6`](https://github.com/llm-d/llm-d-router/commit/c6901f695703f91cf19aad76f2495bac63aeb47a). This is documented in the discovery readme but I was planning on adding it only when I actually wire in the feature in the next PR. A host:port string that tells the EPP where to scrape Prometheus metrics from for this endpoint. Allow to indicate a different port than they accept inference requests on (e.g., vLLM running inference on :8000 but /metrics on :9090).
### 
[May 19, 2026](https://github.com/llm-d/llm-d-router/pull/1135#discussion_r3265693915)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
sounds like only a port change makes sense. The EPP can not scrape and serve on **different IPs** , only same IP and differenet port is feasible (and _deprecated_).
### 
commented [May 18, 2026](https://github.com/llm-d/llm-d-router/pull/1135#issuecomment-4476266510)
Copy link  Copy Markdown 
Contributor Author  
|  /hold cancel  |  
| --- |  
[github-actions](https://github.com/apps/github-actions) Bot removed the [ hold ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Ahold) PRs that are blocked on design, other features, release cycle, etc. label [May 18, 2026](https://github.com/llm-d/llm-d-router/pull/1135#event-25649914061)
[ezrasilvera](https://github.com/ezrasilvera) added 9 commits [May 18, 2026 13:47](https://github.com/llm-d/llm-d-router/pull/1135#commits-pushed-10ce817)
` file-discovery: add FileDiscovery plugin with YAML/JSON file loading …[](https://github.com/llm-d/llm-d-router/pull/1135/commits/10ce8175b49f26c96c2946dd4e5d767e1e03c992 "file-discovery: add FileDiscovery plugin with YAML/JSON file loading and fsnotify watch

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: hYuv32GGazlAD6dWQsWF6ak2JuZdqDrOnkR4AvZqw9Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
…and fsnotify watch

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` Add integration test: DiscoveryNotifier works alongside direct Backen…[](https://github.com/llm-d/llm-d-router/pull/1135/commits/8477fbf56828d75418321c0d43ce4fb94ef1dfb7 "Add integration test: DiscoveryNotifier works alongside direct BackendUpsert

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: hYuv32GGazlAD6dWQsWF6ak2JuZdqDrOnkR4AvZqw9Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
…dUpsert

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` re-attach fsnotify watcher after atomic rename and ConfigMap rotation[](https://github.com/llm-d/llm-d-router/pull/1135/commits/2c39db396044195a718c8afa897c5d48eba75fc2 "re-attach fsnotify watcher after atomic rename and ConfigMap rotation

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

` skip invalid endpoints on reload instead of aborting the entire load[](https://github.com/llm-d/llm-d-router/pull/1135/commits/8dbda93f5b45a5e99c2d31fa53e1570f4ccab4b8 "skip invalid endpoints on reload instead of aborting the entire load

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

` test: rename stale BackendUpsert call to EndpointUpsert[](https://github.com/llm-d/llm-d-router/pull/1135/commits/ed5bffe9e8dcef4f658a101c867fc587b98c1cf8 "test: rename stale BackendUpsert call to EndpointUpsert

Test was added before the BackendUpsert/Delete -> EndpointUpsert/Delete
rename landed on main; go vet caught the unresolved reference.

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[ezrasilvera](https://github.com/ezrasilvera) Ezra Silvera 
SSH Key Fingerprint: hYuv32GGazlAD6dWQsWF6ak2JuZdqDrOnkR4AvZqw9Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Test was added before the BackendUpsert/Delete -> EndpointUpsert/Delete
rename landed on main; go vet caught the unresolved reference.

Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` document EndpointEntry.MetricsHost field[](https://github.com/llm-d/llm-d-router/pull/1135/commits/a51d766d1565581ae02fe2248448f7fe1fe91f02 "document EndpointEntry.MetricsHost field

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

` rename file_discovery.go to plugin.go[](https://github.com/llm-d/llm-d-router/pull/1135/commits/dab8f4191eec93578ff2828736309a24c7fdbd7e "rename file_discovery.go to plugin.go

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

` comment file-discovery params for user-facing config[](https://github.com/llm-d/llm-d-router/pull/1135/commits/525823bec30e782adb00d3269fcc9e53b63d3e08 "comment file-discovery params for user-facing config

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

` update imports after main module rename to llm-d-router[](https://github.com/llm-d/llm-d-router/pull/1135/commits/37b5139d30e16d4d61f02235c097f7d127137aa1 "update imports after main module rename to llm-d-router

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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

[ezrasilvera](https://github.com/ezrasilvera) [force-pushed](https://github.com/llm-d/llm-d-router/compare/2295a193439baaf1af85d89f91579b17b5f6bee1..37b5139d30e16d4d61f02235c097f7d127137aa1) the  filediscovery-3 branch from [`2295a19`](https://github.com/llm-d/llm-d-router/commit/2295a193439baaf1af85d89f91579b17b5f6bee1) to [`37b5139`](https://github.com/llm-d/llm-d-router/commit/37b5139d30e16d4d61f02235c097f7d127137aa1) [ Compare ](https://github.com/llm-d/llm-d-router/compare/2295a193439baaf1af85d89f91579b17b5f6bee1..37b5139d30e16d4d61f02235c097f7d127137aa1) [May 18, 2026 10:50](https://github.com/llm-d/llm-d-router/pull/1135#event-25653189953)
[ezrasilvera](https://github.com/ezrasilvera) added 2 commits [May 18, 2026 13:59](https://github.com/llm-d/llm-d-router/pull/1135#commits-pushed-06986a0)
` rename FileDiscovery.current to endpoints and document it[](https://github.com/llm-d/llm-d-router/pull/1135/commits/06986a05f837a9a2f45a61e51c37cf6aafcd8223 "rename FileDiscovery.current to endpoints and document it

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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` add README for file-discovery plugin[](https://github.com/llm-d/llm-d-router/pull/1135/commits/93421cc8511bdb0fdf44ad73991e17f6fc87b2d3 "add README for file-discovery plugin

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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

### 
commented [May 18, 2026](https://github.com/llm-d/llm-d-router/pull/1135#issuecomment-4477048301)
Copy link  Copy Markdown 
Contributor Author  
| 
> pls add a README.md file to document the plugin, see other plugins for examples or [#917](https://github.com/llm-d/llm-d-router/issues/917) for a suggested template..
I'm planning to have a discovery.md (next PR) describing the overall discovery plugin framework. I also added a README to the filediscovery folder according to the template. [`93421cc`](https://github.com/llm-d/llm-d-router/commit/93421cc8511bdb0fdf44ad73991e17f6fc87b2d3)  |  
| --- |  
reviewed  [ May 19, 2026 ](https://github.com/llm-d/llm-d-router/pull/1135#pullrequestreview-4318164736)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1135/files/93421cc8511bdb0fdf44ad73991e17f6fc87b2d3)
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/file/README.md](https://github.com/llm-d/llm-d-router/pull/1135/files/93421cc8511bdb0fdf44ad73991e17f6fc87b2d3#diff-3390778fc2e9d2d14f4e27ca883fa69ff7b66f291b85cf8142b065febbb89aa2) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/file/README.md](https://github.com/llm-d/llm-d-router/pull/1135/files/93421cc8511bdb0fdf44ad73991e17f6fc87b2d3#diff-3390778fc2e9d2d14f4e27ca883fa69ff7b66f291b85cf8142b065febbb89aa2) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/file/README.md](https://github.com/llm-d/llm-d-router/pull/1135/files/93421cc8511bdb0fdf44ad73991e17f6fc87b2d3#diff-3390778fc2e9d2d14f4e27ca883fa69ff7b66f291b85cf8142b065febbb89aa2) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/file/README.md](https://github.com/llm-d/llm-d-router/pull/1135/files/93421cc8511bdb0fdf44ad73991e17f6fc87b2d3#diff-3390778fc2e9d2d14f4e27ca883fa69ff7b66f291b85cf8142b065febbb89aa2) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/file/plugin.go](https://github.com/llm-d/llm-d-router/pull/1135/files#diff-01f54de51a40d0be227178ff8afda2e1852d013d7e1190201f49e98ba2a3fabf) Outdated   
|  Namespace string `json:"namespace,omitempty" yaml:"namespace,omitempty"`  |  
| --- |  
|  Address string `json:"address" yaml:"address"`  |  
|  Port string `json:"port" yaml:"port"`  |  
|  MetricsHost string `json:"metricsHost,omitempty" yaml:"metricsHost,omitempty"`  |  
### 
[May 19, 2026](https://github.com/llm-d/llm-d-router/pull/1135#discussion_r3265693915)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
sounds like only a port change makes sense. The EPP can not scrape and serve on **different IPs** , only same IP and differenet port is feasible (and _deprecated_).
### 
commented [May 19, 2026](https://github.com/llm-d/llm-d-router/pull/1135#issuecomment-4487029786)
Copy link  Copy Markdown 
Collaborator  
|  please do not use Sleep in tests, use Eventually instead.  |  
| --- |  
[ezrasilvera](https://github.com/ezrasilvera) added 2 commits [May 19, 2026 14:47](https://github.com/llm-d/llm-d-router/pull/1135#commits-pushed-5ae49ca)
` file-discovery: replace metricsHost with metricsPort[](https://github.com/llm-d/llm-d-router/pull/1135/commits/5ae49ca9d354fc3a6230d044cdb7130e76685ec4 "file-discovery: replace metricsHost with metricsPort

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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

` file-discovery: replace test Sleep with Eventually for reload test[](https://github.com/llm-d/llm-d-router/pull/1135/commits/73eaa820b910443d9cd03795c303125499ec4e1b "file-discovery: replace test Sleep with Eventually for reload test

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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

### 
commented [May 19, 2026](https://github.com/llm-d/llm-d-router/pull/1135#issuecomment-4487539392)
Copy link  Copy Markdown 
Contributor Author  
| 
> please do not use Sleep in tests, use Eventually instead.
Done. [`73eaa82`](https://github.com/llm-d/llm-d-router/commit/73eaa820b910443d9cd03795c303125499ec4e1b)  |  
| --- |  
reviewed  [ May 19, 2026 ](https://github.com/llm-d/llm-d-router/pull/1135#pullrequestreview-4318801629)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1135/files/73eaa820b910443d9cd03795c303125499ec4e1b)
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/file/plugin.go](https://github.com/llm-d/llm-d-router/pull/1135/files/73eaa820b910443d9cd03795c303125499ec4e1b#diff-01f54de51a40d0be227178ff8afda2e1852d013d7e1190201f49e98ba2a3fabf) Outdated   
|  // EndpointEntry is the YAML/JSON representation of a single endpoint.  |  
| --- |  
|  type EndpointEntry struct {  |  
|  Name string `json:"name" yaml:"name"`  |  
### 
[May 19, 2026](https://github.com/llm-d/llm-d-router/pull/1135#discussion_r3266200159)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Q: IIRC, the `sigs.k8s.io/yaml` converts all to `json` so you might be able to get away with json tags alone
` file-discovery: validate IPv4 only and reject IPv6 addresses[](https://github.com/llm-d/llm-d-router/pull/1135/commits/bdf8ebd8df380d4290052f362fb1e0753188669a "file-discovery: validate IPv4 only and reject IPv6 addresses

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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

reviewed  [ May 19, 2026 ](https://github.com/llm-d/llm-d-router/pull/1135#pullrequestreview-4318818117)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1135/files/bdf8ebd8df380d4290052f362fb1e0753188669a)
### 
left a comment 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
/lgtm from my point of view (nits can be addressed or not at your discretion). Leaving final approval to [@ahg-g](https://github.com/ahg-g)
` file-discovery: drop metricsPort, always scrape from address:port[](https://github.com/llm-d/llm-d-router/pull/1135/commits/d11516033d57a70630e0144806d00006b3229c00 "file-discovery: drop metricsPort, always scrape from address:port

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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).

```
Signed-off-by: Ezra Silvera <ezra@il.ibm.com>
```

### 
commented [May 19, 2026](https://github.com/llm-d/llm-d-router/pull/1135#issuecomment-4487887435) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).
Copy link  Copy Markdown 
Contributor Author  
|  [@elevran](https://github.com/elevran) as you suggested I removed the MetricsPort and HOST completely so scrapping always from address:port fixed in [`d115160`](https://github.com/llm-d/llm-d-router/commit/d11516033d57a70630e0144806d00006b3229c00)  |  
| --- |  
reviewed  [ May 19, 2026 ](https://github.com/llm-d/llm-d-router/pull/1135#pullrequestreview-4320524606)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1135/files/d11516033d57a70630e0144806d00006b3229c00)
### 
left a comment 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
/approve
Comment thread [pkg/epp/framework/plugins/datalayer/discovery/file/plugin.go](https://github.com/llm-d/llm-d-router/pull/1135/files/d11516033d57a70630e0144806d00006b3229c00#diff-01f54de51a40d0be227178ff8afda2e1852d013d7e1190201f49e98ba2a3fabf)  
|  // from the last successful load. Used as a key set only -- values are  |  
| --- |  
|  // zero-byte structs. Compared against the entries parsed during a  |  
|  // reload to compute which endpoints to delete from the datastore.  |  
|  endpoints map[types.NamespacedName]struct{}  |  
### 
[May 19, 2026](https://github.com/llm-d/llm-d-router/pull/1135#discussion_r3267585626) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
nit, I think for a set `any` is typically being used as the value
[ahg-g](https://github.com/ahg-g) enabled auto-merge (squash) [May 19, 2026 15:40](https://github.com/llm-d/llm-d-router/pull/1135#event-25713826442)
### 
commented [May 19, 2026](https://github.com/llm-d/llm-d-router/pull/1135#issuecomment-4489781466)
Copy link  Copy Markdown 
Collaborator  
|  /approve  |  
| --- |  
approved these changes  [ May 19, 2026 ](https://github.com/llm-d/llm-d-router/pull/1135#pullrequestreview-4320883292)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1135/files/d11516033d57a70630e0144806d00006b3229c00)
Hide details View details
[ahg-g](https://github.com/ahg-g) merged commit [`25b2584`](https://github.com/llm-d/llm-d-router/commit/25b258433b2909070499976a209b9a99e08bdfba) into  llm-d:main [May 19, 2026](https://github.com/llm-d/llm-d-router/pull/1135#event-25715641014)
12 checks passed 
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1135).
[github-project-automation](https://github.com/apps/github-project-automation) Bot moved this to **In review** in [llm-d-router](https://github.com/orgs/llm-d/projects/15) [May 19, 2026](https://github.com/llm-d/llm-d-router/pull/1135#event-25715643570)
[github-project-automation](https://github.com/apps/github-project-automation) Bot moved this from **In review** to **Done** in [llm-d-router](https://github.com/orgs/llm-d/projects/15) [May 19, 2026](https://github.com/llm-d/llm-d-router/pull/1135#event-25715645389)
[ezrasilvera](https://github.com/ezrasilvera) mentioned this pull request [ May 19, 2026 ](https://github.com/llm-d/llm-d-router/pull/1135#ref-pullrequest-4480293218)
[ FileDiscovery plugin (4/4) - wiring into the runner #1218 ](https://github.com/llm-d/llm-d-router/pull/1218)
Merged 
[chcost](https://github.com/chcost) mentioned this pull request [ Jun 2, 2026 ](https://github.com/llm-d/llm-d-router/pull/1135#ref-issue-4460278591)
[ v0.8 Release Goals (Mid-June) llm-d/llm-d#1520 ](https://github.com/llm-d/llm-d/issues/1520)
Closed 
67 tasks
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fllm-d%2Fllm-d-router%2Fpull%2F1135)
###  Labels 
[ size/XL ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FXL) Denotes a PR that changes 500-999 lines, ignoring generated files.
###  5 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
