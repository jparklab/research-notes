---
title: 'Clear prefill target header if set in incoming request by elevran · Pull Request
  #244 · llm-d/llm-d-router · GitHub'
id: clear-prefill-target-header-if-set-in-incoming-request-by-elevran-pull-request-2
tags:
- llmd-router-nonk8s-backends-54b774
- ssrf-protection
- pd-sidecar
created: '2026-09-11T21:57:22.927575Z'
updated: '2026-09-15T04:55:23.997821Z'
source: https://github.com/llm-d/llm-d-router/pull/244
source_domain: github.com
fetched_at: '2026-09-11T21:57:22.902261Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'PR #244 (merged Jul 16 2025, elevran), the primary fix for issue #242: sets
  the prefill-target header to an empty string if present on an incoming request,
  before the scheduler/EPP sets its own value after routing decisions are made. Author''s
  rationale, quoted: ''A possibly better fix would have been to respond to the gateway
  (Envoy) with remove_headers mutation but GIE currently supports only set_headers
  mutations, so clearing the user provided values seems like the best option given
  the available functionality.'' This is EPP/scheduler-side logic (not the sidecar),
  meaning the SSRF defense-in-depth model is: (1) EPP/gateway strips attacker-supplied
  x-prefiller-host-port on ingress [this fix] and (2) sidecar-side InferencePool allowlist
  as a secondary check. If a non-Kubernetes deployment keeps the EPP/gateway path
  unchanged and only replaces backend discovery, mitigation (1) still applies as long
  as no untrusted client can reach the sidecar directly -- a network-topology requirement,
  not a Kubernetes-specific one.'
---

*Suggested by [[potential-ssrf-via-x-prefiller-url-or-x-prefiller-host-port-header-issue-242-llm]] — PR that closed issue 242 by clearing the prefill header on incoming requests -- the real primary mitigation, upstream of the sidecar allowlist*

[Skip to content](https://github.com/llm-d/llm-d-router/pull/244#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/244) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/244) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/244) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/244).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Conversation
### 
commented [Jul 16, 2025](https://github.com/llm-d/llm-d-router/pull/244#issue-3235765137)
Copy link  Copy Markdown 
Collaborator
Fixes [#242](https://github.com/llm-d/llm-d-router/issues/242)
This sets the prefill target header to an empty string value (an alternative would be to use a safe sentinel values such as `<stripped>`).
A possibly better fix would have been to respond to the gateway (Envoy) with `remove_headers` mutation but GIE currently supports only `set_headers` mutations, so clearing the user provided values seems like the best option given the available functionality.
The PR also includes an unrelated typo fix which I saw while reading related code.
` clear prefill target header if set in incoming request[](https://github.com/llm-d/llm-d-router/pull/244/commits/ec8a71540ec08d5a7f9fb41a27f184e1d5fdfe81 "clear prefill target header if set in incoming request

Signed-off-by: Etai Lev Ran <elevran@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[elevran](https://github.com/elevran) Etai Lev Ran 
SSH Key Fingerprint: MaRGtJ1PGrgungGrj+Ynt3KzOYKccSp7XkPaDLhNnvc
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Etai Lev Ran <elevran@gmail.com>
```

approved these changes  [ Jul 16, 2025 ](https://github.com/llm-d/llm-d-router/pull/244#pullrequestreview-3024753698)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/244/files/ec8a71540ec08d5a7f9fb41a27f184e1d5fdfe81)
### 
left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
/lgtm /approve
Thanks!
approved these changes  [ Jul 16, 2025 ](https://github.com/llm-d/llm-d-router/pull/244#pullrequestreview-3024881913)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/244/files/ec8a71540ec08d5a7f9fb41a27f184e1d5fdfe81)
[elevran](https://github.com/elevran) merged commit [`acddc0c`](https://github.com/llm-d/llm-d-router/commit/acddc0c5f012b918522efa29897eb3121e5ae9d2) into  llm-d:main [Jul 16, 2025](https://github.com/llm-d/llm-d-router/pull/244#event-18659209066)
[elevran](https://github.com/elevran) deleted the  issue_242  branch [July 16, 2025 13:11](https://github.com/llm-d/llm-d-router/pull/244#event-18659210183)
[elevran](https://github.com/elevran) mentioned this pull request [ Jul 16, 2025 ](https://github.com/llm-d/llm-d-router/pull/244#ref-issue-3235458086)
[ Potential SSRF via `x-prefiller-url` or `x-prefiller-host-port` header #242 ](https://github.com/llm-d/llm-d-router/issues/242)
Closed 
[dmitripikus](https://github.com/dmitripikus) pushed a commit to dmitripikus/llm-d-router that referenced this pull request [ Jul 16, 2025 ](https://github.com/llm-d/llm-d-router/pull/244#ref-commit-727b041)
` clear prefill target header if set in incoming request ([](https://github.com/dmitripikus/llm-d-router/commit/727b04105f7c4b088f928317f38977f5ccb2ee14 "clear prefill target header if set in incoming request \(#244\)

Signed-off-by: Etai Lev Ran <elevran@gmail.com>")llm-d#244[](https://github.com/llm-d/llm-d-router/pull/244))[](https://github.com/dmitripikus/llm-d-router/commit/727b04105f7c4b088f928317f38977f5ccb2ee14 "clear prefill target header if set in incoming request \(#244\)

Signed-off-by: Etai Lev Ran <elevran@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
SSH Key Fingerprint: rdGV/uZ9GbzXZd/y49/UPU2s3tgBiNMRTSiA7yRECPg
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Etai Lev Ran <elevran@gmail.com>
```

[elevran](https://github.com/elevran) mentioned this pull request [ Jul 16, 2025 ](https://github.com/llm-d/llm-d-router/pull/244#ref-pullrequest-3237055727)
[ sanity check target decode host llm-d/llm-d-routing-sidecar#48 ](https://github.com/llm-d/llm-d-routing-sidecar/pull/48)
Closed 
[pierDipi](https://github.com/pierDipi) pushed a commit to pierDipi/llm-d-inference-scheduler that referenced this pull request [ Nov 28, 2025 ](https://github.com/llm-d/llm-d-router/pull/244#ref-commit-f5463dd)
` Update registry.redhat.io/ubi9/go-toolset:1.24 Docker digest to 6f0e6…[](https://github.com/pierDipi/llm-d-inference-scheduler/commit/f5463dd3a77ef63582cc796b25cafc196a604587 "Update registry.redhat.io/ubi9/go-toolset:1.24 Docker digest to 6f0e6c5 \(#244\)

Signed-off-by: red-hat-konflux <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>
Co-authored-by: red-hat-konflux\[bot\] <126015336+red-hat-konflux\[bot\]@users.noreply.github.com>") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
…c5 (llm-d#244[](https://github.com/llm-d/llm-d-router/pull/244))

Signed-off-by: red-hat-konflux <126015336+red-hat-konflux[bot]@users.noreply.github.com>
Co-authored-by: red-hat-konflux[bot] <126015336+red-hat-konflux[bot]@users.noreply.github.com>
```

[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fllm-d%2Fllm-d-router%2Fpull%2F244)
###  Labels 
None yet 
###  3 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
