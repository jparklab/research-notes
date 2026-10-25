---
title: 'Trust standalone EPP-selected destinations by danehans · Pull Request #1658
  · agentgateway/agentgateway · GitHub'
id: trust-standalone-epp-selected-destinations-by-danehans-pull-request-1658-agentga
tags:
- llmd-router-nonk8s-backends-54b774
- standalone-epp
- agentgateway
- destination-mode
- epp-passthrough
created: '2026-09-11T20:48:52.046344Z'
updated: '2026-09-15T04:55:23.839771Z'
source: https://github.com/agentgateway/agentgateway/pull/1658
source_domain: github.com
fetched_at: '2026-09-11T20:48:52.023705Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'agentgateway PR #1658 (merged Apr 24 2026, danehans/solo.io) adds an explicit
  inferenceRouting destinationMode config. Default mode stays ''endpoint-validated'':
  agentgateway cross-checks the EPP-selected destination against locally mirrored
  Kubernetes Endpoints/workload data before routing. The new ''passthrough'' mode
  makes agentgateway use the EPP-selected destination ip:port directly as the proxy
  backend without requiring any mirrored local endpoint data — i.e. agentgateway does
  not need to know about the backend via k8s Endpoints/EndpointSlice at all, only
  the EPP''s decision. This is the mechanism that lets a standalone (non-InferencePool)
  EPP own endpoint discovery for non-pod backends. Fixes issue #620; e2e-tested against
  kubernetes-sigs/gateway-api-inference-extension PR #2882 (''Add standalone agentgateway
  proxy support''). A companion issue #1661 ''Support fail-open fallback for standalone
  EPP inference routing'' was opened alongside this PR, indicating maintainers explicitly
  designed passthrough mode with a fail-open path in mind (what happens when EPP is
  unreachable) as a separate follow-on concern from trusting EPP''s picks.'
utility_score: 15.0
---

[Skip to content](https://github.com/agentgateway/agentgateway/pull/1658#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/agentgateway/agentgateway/pull/1658) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/agentgateway/agentgateway/pull/1658) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/agentgateway/agentgateway/pull/1658) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/agentgateway/agentgateway/pull/1658).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fagentgateway%2Fagentgateway) You must be signed in to change notification settings
  * [ Fork 829 ](https://github.com/login?return_to=%2Fagentgateway%2Fagentgateway)
  * [ Star  4.8k ](https://github.com/login?return_to=%2Fagentgateway%2Fagentgateway)


## Conversation
### 
commented [Apr 24, 2026](https://github.com/agentgateway/agentgateway/pull/1658#issue-4324430998) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/agentgateway/agentgateway/pull/1658).
Copy link  Copy Markdown 
Collaborator
## Summary
  * Use EPP-selected destinations directly for inferenceRouting service backends when `destinationMode: passthrough` is configured.
  * Keep the default destination mode endpoint-validated so existing configs retain local endpoint-derived behavior.
  * Keep the logical service metadata requirement without requiring mirrored local workload endpoints in passthrough mode.
  * Update the standalone EPP example to opt into the passthrough flow.


## Testing
  * cargo fmt --check
  * cargo test -p agentgateway inference_routing
  * cargo test -p agentgateway --test validate_examples
  * cargo clippy -p agentgateway --all-targets -- -D warnings
  * cargo run -p xtask -- schema


Fixes: [#620](https://github.com/agentgateway/agentgateway/issues/620)
[danehans](https://github.com/danehans) requested a review from a team as a [code owner](https://github.com/agentgateway/agentgateway/blob/736bd050a7382af7348d78430c471b330be1d18f/CODEOWNERS#L1) [April 24, 2026 17:03](https://github.com/agentgateway/agentgateway/pull/1658#event-24845782575)
[danehans](https://github.com/danehans) marked this pull request as draft [April 24, 2026 17:08](https://github.com/agentgateway/agentgateway/pull/1658#event-24845945797)
This was referenced Apr 24, 2026
[ Add standalone agentgateway proxy support kubernetes-sigs/gateway-api-inference-extension#2882 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882)
Merged 
[ Support fail-open fallback for standalone EPP inference routing #1661 ](https://github.com/agentgateway/agentgateway/issues/1661)
reviewed  [ Apr 24, 2026 ](https://github.com/agentgateway/agentgateway/pull/1658#pullrequestreview-4172323376)
[ View reviewed changes ](https://github.com/agentgateway/agentgateway/pull/1658/files)
Comment thread [crates/agentgateway/src/proxy/httpproxy.rs](https://github.com/agentgateway/agentgateway/pull/1658/files#diff-a5673f03b2fc2fc9786eeba0f0df6cbeff9cb7b96726e72fce021d6366972069) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/agentgateway/agentgateway/pull/1658).
Comment thread [crates/agentgateway/src/proxy/httpproxy.rs](https://github.com/agentgateway/agentgateway/pull/1658/files#diff-a5673f03b2fc2fc9786eeba0f0df6cbeff9cb7b96726e72fce021d6366972069) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/agentgateway/agentgateway/pull/1658).
[danehans](https://github.com/danehans) added 2 commits [April 24, 2026 10:57](https://github.com/agentgateway/agentgateway/pull/1658#commits-pushed-d4d4e50)
` proxy: trust standalone EPP destinations[](https://github.com/agentgateway/agentgateway/pull/1658/commits/d4d4e5031b41ccd8d3955b38006769b43f1eb740 "proxy: trust standalone EPP destinations

Use the destination returned by inference routing directly for service
backends so standalone EPP can own endpoint discovery without mirrored
local workload data.

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[danehans](https://github.com/danehans) Daneyon Hansen 
GPG key ID: A9BCCF56D7755933
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Use the destination returned by inference routing directly for service
backends so standalone EPP can own endpoint discovery without mirrored
local workload data.

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

` test: reuse standalone inference router[](https://github.com/agentgateway/agentgateway/pull/1658/commits/2ca0914c6eef5804e2b60d3f1e09532b9eee594b "test: reuse standalone inference router

Let the existing standalone inference test router return either a selected
destination or no destination so the fail-closed case uses the same
fixture.

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[danehans](https://github.com/danehans) Daneyon Hansen 
GPG key ID: A9BCCF56D7755933
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/agentgateway/agentgateway/pull/1658).

```
Let the existing standalone inference test router return either a selected
destination or no destination so the fail-closed case uses the same
fixture.

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

[danehans](https://github.com/danehans) [force-pushed](https://github.com/agentgateway/agentgateway/compare/d84f6e90bfdd388079e6ac300c270061eabd7ef8..2ca0914c6eef5804e2b60d3f1e09532b9eee594b) the  standalone-inference-passthrough branch from [`d84f6e9`](https://github.com/agentgateway/agentgateway/commit/d84f6e90bfdd388079e6ac300c270061eabd7ef8) to [`2ca0914`](https://github.com/agentgateway/agentgateway/commit/2ca0914c6eef5804e2b60d3f1e09532b9eee594b) [ Compare ](https://github.com/agentgateway/agentgateway/compare/d84f6e90bfdd388079e6ac300c270061eabd7ef8..2ca0914c6eef5804e2b60d3f1e09532b9eee594b) [April 24, 2026 17:57](https://github.com/agentgateway/agentgateway/pull/1658#event-24847343429)
` proxy: gate EPP destination passthrough[](https://github.com/agentgateway/agentgateway/pull/1658/commits/ef0ff5d56561193d71247779c8401f5f0ae75466 "proxy: gate EPP destination passthrough

Add an explicit inferenceRouting destination mode so standalone EPP can
opt into trusting selected destinations while default configs keep
endpoint validation.

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[danehans](https://github.com/danehans) Daneyon Hansen 
GPG key ID: A9BCCF56D7755933
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/agentgateway/agentgateway/pull/1658).

```
Add an explicit inferenceRouting destination mode so standalone EPP can
opt into trusting selected destinations while default configs keep
endpoint validation.

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

### 
commented [Apr 24, 2026](https://github.com/agentgateway/agentgateway/pull/1658#issuecomment-4315454605) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/agentgateway/agentgateway/pull/1658).
Copy link  Copy Markdown 
Collaborator Author  
|  [@howardjohn](https://github.com/howardjohn) the [Branch / controller-e2e (pull_request)](https://github.com/agentgateway/agentgateway/actions/runs/24904642112/job/72930769531?pr=1658) is failing due to `TestAgentgatewayIntegration/MCP/TestDynamicMCPAdminVsUserTools` failing with `context canceled`. This does not appear to be related to this PR. I found the same MCP context canceled failure on recent main runs today.  |  
| --- |  
[danehans](https://github.com/danehans) marked this pull request as ready for review [April 24, 2026 18:56](https://github.com/agentgateway/agentgateway/pull/1658#event-24848967850)
### 
commented [Apr 24, 2026](https://github.com/agentgateway/agentgateway/pull/1658#issuecomment-4315576417)
Copy link  Copy Markdown 
Collaborator Author  
|  I e2e tested `passthrough` mode using [kubernetes-sigs/gateway-api-inference-extension#2882](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882).  |  
| --- |  
[danehans](https://github.com/danehans) requested a review from [howardjohn](https://github.com/howardjohn) [April 24, 2026 18:59](https://github.com/agentgateway/agentgateway/pull/1658#event-24849036999)
approved these changes  [ Apr 24, 2026 ](https://github.com/agentgateway/agentgateway/pull/1658#pullrequestreview-4172975219)
[ View reviewed changes ](https://github.com/agentgateway/agentgateway/pull/1658/files/ef0ff5d56561193d71247779c8401f5f0ae75466)
### 
left a comment 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
nice!
Hide details View details
[howardjohn](https://github.com/howardjohn) merged commit [`2cede80`](https://github.com/agentgateway/agentgateway/commit/2cede80015e068617c014160d3e3273907bd5f99) into  agentgateway:main [Apr 24, 2026](https://github.com/agentgateway/agentgateway/pull/1658#event-24851300856)
18 of 19 checks passed 
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/agentgateway/agentgateway/pull/1658).
[danehans](https://github.com/danehans) deleted the  standalone-inference-passthrough  branch [April 24, 2026 20:24](https://github.com/agentgateway/agentgateway/pull/1658#event-24851402742)
[alan-bentley-pgh](https://github.com/alan-bentley-pgh) pushed a commit to solo-io/agentgateway-new-ui that referenced this pull request [ May 19, 2026 ](https://github.com/agentgateway/agentgateway/pull/1658#ref-commit-f637cec)
` Trust standalone EPP-selected destinations ([](https://github.com/solo-io/agentgateway-new-ui/commit/f637cec71ff3712a00a0e5b99cac2234a8634677 "Trust standalone EPP-selected destinations \(#1658\)

## Summary
- Use EPP-selected destinations directly for inferenceRouting service
backends when `destinationMode: passthrough` is configured.
- Keep the default destination mode endpoint-validated so existing
configs retain local endpoint-derived behavior.
- Keep the logical service metadata requirement without requiring
mirrored local workload endpoints in passthrough mode.
- Update the standalone EPP example to opt into the passthrough flow.

## Testing
- cargo fmt --check
- cargo test -p agentgateway inference_routing
- cargo test -p agentgateway --test validate_examples
- cargo clippy -p agentgateway --all-targets -- -D warnings
- cargo run -p xtask -- schema

Fixes: #620

---------

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>")agentgateway#1658[](https://github.com/agentgateway/agentgateway/pull/1658))[](https://github.com/solo-io/agentgateway-new-ui/commit/f637cec71ff3712a00a0e5b99cac2234a8634677 "Trust standalone EPP-selected destinations \(#1658\)

## Summary
- Use EPP-selected destinations directly for inferenceRouting service
backends when `destinationMode: passthrough` is configured.
- Keep the default destination mode endpoint-validated so existing
configs retain local endpoint-derived behavior.
- Keep the logical service metadata requirement without requiring
mirrored local workload endpoints in passthrough mode.
- Update the standalone EPP example to opt into the passthrough flow.

## Testing
- cargo fmt --check
- cargo test -p agentgateway inference_routing
- cargo test -p agentgateway --test validate_examples
- cargo clippy -p agentgateway --all-targets -- -D warnings
- cargo run -p xtask -- schema

Fixes: #620

---------

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>") `

```
## Summary
- Use EPP-selected destinations directly for inferenceRouting service
backends when `destinationMode: passthrough` is configured.
- Keep the default destination mode endpoint-validated so existing
configs retain local endpoint-derived behavior.
- Keep the logical service metadata requirement without requiring
mirrored local workload endpoints in passthrough mode.
- Update the standalone EPP example to opt into the passthrough flow.

## Testing
- cargo fmt --check
- cargo test -p agentgateway inference_routing
- cargo test -p agentgateway --test validate_examples
- cargo clippy -p agentgateway --all-targets -- -D warnings
- cargo run -p xtask -- schema

Fixes: agentgateway#620[](https://github.com/agentgateway/agentgateway/issues/620)

---------

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fagentgateway%2Fagentgateway%2Fpull%2F1658)
###  Labels 
None yet 
###  2 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
