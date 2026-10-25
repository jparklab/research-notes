---
title: 'feat(sidecar): implement SSRF protection with InferencePool host-port  allowlist
  by weizhoublue · Pull Request #1647 · llm-d/llm-d-router · GitHub'
id: featsidecar-implement-ssrf-protection-with-inferencepool-host-port-allowlist-by
tags:
- llmd-router-nonk8s-backends-54b774
- ssrf-protection
- pd-sidecar
created: '2026-09-11T21:56:52.801688Z'
updated: '2026-09-15T04:55:23.994367Z'
source: https://github.com/llm-d/llm-d-router/pull/1647
source_domain: github.com
fetched_at: '2026-09-11T21:56:52.775514Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'PR #1647 (merged ~Jun 2026 by weizhoublue), fixing issue #979, is the CURRENT
  SSRF-protection implementation at HEAD (the allowlist.go/chat_completions.go code
  fetched separately). PR description states the exact mechanism: sidecar ''monitors
  InferencePool spec.selector.matchLabels / spec.targetPorts to construct allowed
  IP and port allowlist'' for inference.networking.k8s.io/v1, then ''inspects incoming
  HTTP requests, comparing X-Prefiller-Host-Port header against constructed allowlist''
  and returns 403 on mismatch. Review discussion on the e2e test (test/e2e/ssrf_test.go)
  references ''Apply SSRF RBAC (ClusterRole + ClusterRoleBinding for InferencePool/Pod
  watch)'' -- confirming that enabling SSRF protection requires cluster-level RBAC
  grants in addition to the InferencePool CRD and pod watch, all Kubernetes-native
  primitives with no non-Kubernetes equivalent offered.'
---

*Suggested by [[chat_completionsgo]] — merged PR that implemented SSRF protection allowlist, likely documents design rationale*

[Skip to content](https://github.com/llm-d/llm-d-router/pull/1647#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/1647) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/1647) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/1647) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1647).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Conversation
### 
commented [Jun 14, 2026](https://github.com/llm-d/llm-d-router/pull/1647#issue-4659044304)
Copy link  Copy Markdown 
Contributor
**What type of PR is this?**
/kind feature
**What this PR does / why we need it** :
SSRF Working Principle：
In P/D disaggregated mode, when request enters decode side, sidecar forwards prefill request to specified prefiller based on header:

```
   X-Prefiller-Host-Port: <prefiller-host>:<port>

```

Validation of `<prefiller-host>:<port>` is required to prevent attacker from directly accessing sidecar and forcing it to forward arbitrary requests to arbitrary ports of prefill worker.
Therefore: • Sidecar monitors InferencePool spec.selector.matchLabels / spec.targetPorts to construct allowed IP and port allowlist. This PR supports inference.networking.k8s.io/v1.
• Sidecar inspects incoming HTTP requests, comparing X-Prefiller-Host-Port header against constructed allowlist to intercept unauthorized requests in real time. It returns 403 for Invalid host/port blocked
**Which issue(s) this PR fixes** :
Fixes [#979](https://github.com/llm-d/llm-d-router/issues/979)
**Release note** _(write`NONE` if no user-facing change)_:

```
sidecar implements SSRF protection with InferencePool host-port  allowlist

```

[github-actions](https://github.com/apps/github-actions) Bot added [ size/L ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files. [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. labels [Jun 14, 2026](https://github.com/llm-d/llm-d-router/pull/1647#event-26725082182)
[weizhoublue](https://github.com/weizhoublue) marked this pull request as ready for review [June 14, 2026 12:29](https://github.com/llm-d/llm-d-router/pull/1647#event-26725297982)
[weizhoublue](https://github.com/weizhoublue) requested review from a team and [shmuelk](https://github.com/shmuelk) as [code owners](https://github.com/llm-d/llm-d-router/blob/96317d5b0eb875364cfc146b5463c049df2b03f4/CODEOWNERS#L14) [June 14, 2026 12:29](https://github.com/llm-d/llm-d-router/pull/1647#event-26725298101)
[weizhoublue](https://github.com/weizhoublue) requested review from [liu-cong](https://github.com/liu-cong) and [vMaroon](https://github.com/vMaroon) [June 14, 2026 12:29](https://github.com/llm-d/llm-d-router/pull/1647#event-26725298205)
[weizhoublue](https://github.com/weizhoublue) mentioned this pull request [ Jun 14, 2026 ](https://github.com/llm-d/llm-d-router/pull/1647#ref-pullrequest-4645828783)
[ fix(sidecar): add startup validation for CLI port and replica options #1619 ](https://github.com/llm-d/llm-d-router/pull/1619)
Merged 
### 
commented [Jun 14, 2026](https://github.com/llm-d/llm-d-router/pull/1647#issuecomment-4701752207)
Copy link  Copy Markdown 
Contributor Author  
|  [@hexfusion](https://github.com/hexfusion) I submitted this PR for issue [#979](https://github.com/llm-d/llm-d-router/issues/979)  |  
| --- |  
reviewed  [ Jun 16, 2026 ](https://github.com/llm-d/llm-d-router/pull/1647#pullrequestreview-4510847277)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/1647/files)
### 
left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
nice improvement thank you [@weizhoublue](https://github.com/weizhoublue)
Comment thread [test/e2e/ssrf_test.go](https://github.com/llm-d/llm-d-router/pull/1647/files#diff-1f17544ad8d7a210d5e7062a41e550d6add61c129f9dd263d5a724c6e7d672ce) Outdated   
|  // createModelServersPDSSRF creates model servers with SSRF protection enabled on the sidecar.  |  
| --- |  
|  func createModelServersPDSSRF(prefillReplicas, decodeReplicas int) []string {  |  
|  // Apply SSRF RBAC (ClusterRole + ClusterRoleBinding for InferencePool/Pod watch).  |  
### 
[Jun 16, 2026](https://github.com/llm-d/llm-d-router/pull/1647#discussion_r3424458173)
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
minor: above we are using Role RoleBinding can we align comment
### 
[Jun 17, 2026](https://github.com/llm-d/llm-d-router/pull/1647#discussion_r3425112034)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
thanks, updated
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[weizhoublue](https://github.com/weizhoublue) weizhoublue 
SSH Key Fingerprint: Dd0J4Lb+tRzF5zZ6isqdQxjVnIn2NsqWuQ9YVQ9iJSo
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/1647).

```
Signed-off-by: weizhou.lan@daocloud.io <weizhou.lan@daocloud.io>
```

[weizhoublue](https://github.com/weizhoublue) [force-pushed](https://github.com/llm-d/llm-d-router/compare/5a24fbf92c1f4b7a11af1edb0b58fd27b639b428..5ce40c2dd8032ea02f5ded818cbe1b3c48438b0d) the  fea/ssrf branch from [`5a24fbf`](https://github.com/llm-d/llm-d-router/commit/5a24fbf92c1f4b7a11af1edb0b58fd27b639b428) to [`5ce40c2`](https://github.com/llm-d/llm-d-router/commit/5ce40c2dd8032ea02f5ded818cbe1b3c48438b0d) [ Compare ](https://github.com/llm-d/llm-d-router/compare/5a24fbf92c1f4b7a11af1edb0b58fd27b639b428..5ce40c2dd8032ea02f5ded818cbe1b3c48438b0d) [June 17, 2026 01:59](https://github.com/llm-d/llm-d-router/pull/1647#event-26837583213)
### 
commented [Jul 9, 2026](https://github.com/llm-d/llm-d-router/pull/1647#issuecomment-4920746535)
Copy link  Copy Markdown 
Contributor  
|  This PR is marked as stale after 21d of inactivity. After an additional 14d of inactivity (7d to become rotten, then 7d more), it will be closed. To prevent this PR from being closed, add a comment or remove the `lifecycle/stale` label.  |  
| --- |  
[github-actions](https://github.com/apps/github-actions) Bot added the [ lifecycle/stale ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Alifecycle%2Fstale) label [Jul 9, 2026](https://github.com/llm-d/llm-d-router/pull/1647#event-27743376445)
[weizhoublue](https://github.com/weizhoublue) closed this [Jul 19, 2026](https://github.com/llm-d/llm-d-router/pull/1647#event-28177300278)
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fllm-d%2Fllm-d-router%2Fpull%2F1647)
###  Labels 
[ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. [ lifecycle/stale ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Alifecycle%2Fstale) [ size/L ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files.
###  2 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
