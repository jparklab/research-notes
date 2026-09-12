---
title: 'Add standalone inference routing local config by danehans · Pull Request #1429
  · agentgateway/agentgateway · GitHub'
id: add-standalone-inference-routing-local-config-by-danehans-pull-request-1429-agen
tags:
- llmd-router-nonk8s-backends-54b774
- standalone-epp
- agentgateway
- fail-closed
- local-config
created: '2026-09-11T20:48:58.521482Z'
updated: '2026-09-12T03:37:37.684893Z'
source: https://github.com/agentgateway/agentgateway/pull/1429
source_domain: github.com
fetched_at: '2026-09-11T20:48:58.497315Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'agentgateway PR #1429 (merged, danehans/solo.io, part of umbrella issue
  #620) adds local static-config support for standalone inference routing on service
  backends, ships a user-facing examples/standalone-epp config example, and deliberately
  keeps standalone inference routing fail-closed ''for now'' (i.e., if the EPP call
  fails, the request is rejected rather than falling back to a default backend --
  the safer default prior to #1658''s later passthrough/trust work). Maintainer howardjohn
  pushed back on adding a separate LocalInferenceRouting-vs-InferenceRouting split
  meant to prevent inference routing on non-Service backend types, noting agentgateway
  already permits inference routing on non-Service types elsewhere (e.g. LocalRouteBackend)
  and that guarding it in only one place added ~190 LOC for ''0.01% of people'' --
  danehans agreed to keep the old (permissive) behavior instead of adding the guard.
  Author''s stated next steps included documenting ''how this fits into the upstream
  GIE EPP standalone chart'' and documenting required Kubernetes deps such as ''model
  workloads require a k8s Service'' -- direct evidence that even standalone/non-pod
  routing in this design still assumes a k8s Service object exists for the backend
  group, not necessarily individual pod endpoints.'
utility_score: 13.0
---

[Skip to content](https://github.com/agentgateway/agentgateway/pull/1429#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/agentgateway/agentgateway/pull/1429) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/agentgateway/agentgateway/pull/1429) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/agentgateway/agentgateway/pull/1429) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/agentgateway/agentgateway/pull/1429).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fagentgateway%2Fagentgateway) You must be signed in to change notification settings
  * [ Fork 829 ](https://github.com/login?return_to=%2Fagentgateway%2Fagentgateway)
  * [ Star  4.8k ](https://github.com/login?return_to=%2Fagentgateway%2Fagentgateway)


## Conversation
### 
commented [Apr 1, 2026](https://github.com/agentgateway/agentgateway/pull/1429#issue-4190503973) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/agentgateway/agentgateway/pull/1429).
Copy link  Copy Markdown 
Collaborator
## Summary
  * Add local static config support for standalone inference routing on service backends.
  * Keep standalone inference routing fail-closed (for now).
  * Reuse the existing ext-proc test helpers for standalone inference-routing coverage.
  * Add a user-facing standalone EPP example under `examples/standalone-epp`.
  * Simplify the local config implementation based on review feedback and inline inference-routing parsing.


Part of [#620](https://github.com/agentgateway/agentgateway/issues/620)
## Testing
`cargo test -p agentgateway inference_routing` `cargo test -p agentgateway --test validate_examples` `cargo run -p xtask -- schema`
[danehans](https://github.com/danehans) [force-pushed](https://github.com/agentgateway/agentgateway/compare/185b739ae07505d55a3546b5684ab1c4cd546c5e..9aeb33946990d798cdc12291ba321b8d70a033b4) the  inference-routing-static-config branch from [`185b739`](https://github.com/agentgateway/agentgateway/commit/185b739ae07505d55a3546b5684ab1c4cd546c5e) to [`9aeb339`](https://github.com/agentgateway/agentgateway/commit/9aeb33946990d798cdc12291ba321b8d70a033b4) [ Compare ](https://github.com/agentgateway/agentgateway/compare/185b739ae07505d55a3546b5684ab1c4cd546c5e..9aeb33946990d798cdc12291ba321b8d70a033b4) [April 1, 2026 23:36](https://github.com/agentgateway/agentgateway/pull/1429#event-24130291867)
### 
commented [Apr 1, 2026](https://github.com/agentgateway/agentgateway/pull/1429#issuecomment-4173639756) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/agentgateway/agentgateway/pull/1429).
Copy link  Copy Markdown 
Collaborator Author  
|  Next steps:
  * Add a standalone EPP example for agentgateway.
  * Provide the agentgateway local config that users should mount into the sidecar.
  * Document how this fits into the upstream GIE EPP standalone chart.
  * Document the required Kubernetes deps, e.g. model workloads require a k8s Service.
  * Others?

 |  
| --- |  
reviewed  [ Apr 7, 2026 ](https://github.com/agentgateway/agentgateway/pull/1429#pullrequestreview-4070686572)
[ View reviewed changes ](https://github.com/agentgateway/agentgateway/pull/1429/files)
Comment thread [crates/agentgateway/tests/tests/standalone.rs](https://github.com/agentgateway/agentgateway/pull/1429/files#diff-3a2df88f474329fa34b68d7a4769c23c83d2f2d9cc6f780a35edb90b7e42d24a) Outdated   
| @@ -0,0 +1,337 @@  |  
| --- |  
|  use std::net::SocketAddr;  |  
### 
[Apr 7, 2026](https://github.com/agentgateway/agentgateway/pull/1429#discussion_r3047339649)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I would prefer to not re-implement all of this. We already have existing ext_proc mocks (`ExtProcMock`) that could be used. We do not need to bind to real ports either and can use ephemeral ports to avoid issues around port conflicts and reuse.
I would take a look at `setup_ext_proc_mock`, we would just change the setup to attach an an `inferenceRouting` instead of an `extProc` really. This should drop down the net-new test code to ~50 LOC instead of 350
### 
[Apr 21, 2026](https://github.com/agentgateway/agentgateway/pull/1429#discussion_r3118895503)
Copy link  Copy Markdown 
Collaborator Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I'll reuse the existing ext-proc test helpers.
Comment thread [examples/standalone-epp/config.yaml](https://github.com/agentgateway/agentgateway/pull/1429/files#diff-97232951b725bec5442e1b0f81533224b341f845c4968841cf64c496e366b257)  
| @@ -0,0 +1,17 @@  |  
| --- |  
|  # yaml-language-server: $schema=../../schema/local.json  |  
### 
[Apr 7, 2026](https://github.com/agentgateway/agentgateway/pull/1429#discussion_r3047342701)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I don't think this is really needed, the point of the local-test is to test complex transformations. If we put a whole yaml for every single policy that has trivial translations we will end up with 10KLOC of not useful tests.
### 
[Apr 7, 2026](https://github.com/agentgateway/agentgateway/pull/1429#discussion_r3047344670)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
If we want an example under examples/ that could be useful (user facing). CI will validate all configs under examples/ too.
### 
[Apr 21, 2026](https://github.com/agentgateway/agentgateway/pull/1429#discussion_r3118886005)
Copy link  Copy Markdown 
Collaborator Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
> I don't think this is really needed, the point of the local-test is to test complex transformations.
Okay. I was unsure since `crates/agentgateway/src/types/local_tests` contains a few fairly simple tests.
### 
[Apr 21, 2026](https://github.com/agentgateway/agentgateway/pull/1429#discussion_r3118891489)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
that is because peoples AI bots add them faster than I can tell them not to 😆
Comment thread [crates/agentgateway/src/types/local.rs](https://github.com/agentgateway/agentgateway/pull/1429/files#diff-54db4875809642fdd31c503c56565ce7edb0e8ff7dc2b1a65a3c332d0e6294f3) Outdated   
|  #[apply(schema_de!)]  |  
| --- |  
|  pub struct LocalInferenceRouting {  |  
### 
[Apr 7, 2026](https://github.com/agentgateway/agentgateway/pull/1429#discussion_r3047350086)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
No need for this I think; `InferenceRouting` can just implement Deserialize directly. The `LocalXYX` is only needed for types that need to do complex things like fetching HTTP objects, etc
Comment thread [crates/agentgateway/src/types/local.rs](https://github.com/agentgateway/agentgateway/pull/1429/files#diff-54db4875809642fdd31c503c56565ce7edb0e8ff7dc2b1a65a3c332d0e6294f3) Outdated   
|  pub host: Target,  |  
| --- |  
|  #[serde(default, skip_serializing_if = "Option::is_none")]  |  
|  pub policies: Option<LocalBackendPolicies>,  |  
|  pub policies: Option<LocalNamedBackendPolicies>,  |  
### 
[Apr 7, 2026](https://github.com/agentgateway/agentgateway/pull/1429#discussion_r3047365507)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I don't know this split is worth doing. I assume the goal is to avoid Inference routing on non-Service types, but we already allow that in other places (like LocalRouteBackend) so adding ~190 LOC to avoid it in one place (that is used by 0.01% of people) doesn't seem great.
It may be cleaner to just keep the old behavior
### 
[Apr 21, 2026](https://github.com/agentgateway/agentgateway/pull/1429#discussion_r3118930457)
Copy link  Copy Markdown 
Collaborator Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
> I assume the goal is to avoid Inference routing on non-Service types
Correct.
> It may be cleaner to just keep the old behavior
+1
Comment thread [crates/agentgateway/src/types/local.rs](https://github.com/agentgateway/agentgateway/pull/1429/files#diff-54db4875809642fdd31c503c56565ce7edb0e8ff7dc2b1a65a3c332d0e6294f3) Outdated   
|  pub ai: Option<llm::Policy>,  |  
| --- |  
|  impl LocalAIProviderPolicies {  |  
### 
[Apr 7, 2026](https://github.com/agentgateway/agentgateway/pull/1429#discussion_r3047369193)
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I don't really like this duplication... it will make it easy to make mistakes. if we are going to do this it would be good to at least split out SimpleLocalBackendPolicies common processing?
### 
commented [Apr 21, 2026](https://github.com/agentgateway/agentgateway/pull/1429#issuecomment-4290113784)
Copy link  Copy Markdown 
Collaborator Author  
|  [@howardjohn](https://github.com/howardjohn) Sorry for the delay. I had to complete other work before I could come back to this. I'll work on updating the PR based on your review comments.  |  
| --- |  
[danehans](https://github.com/danehans) [force-pushed](https://github.com/agentgateway/agentgateway/compare/9aeb33946990d798cdc12291ba321b8d70a033b4..0180a16818256dc3614041427fe206dfe064cc48) the  inference-routing-static-config branch from [`9aeb339`](https://github.com/agentgateway/agentgateway/commit/9aeb33946990d798cdc12291ba321b8d70a033b4) to [`0180a16`](https://github.com/agentgateway/agentgateway/commit/0180a16818256dc3614041427fe206dfe064cc48) [ Compare ](https://github.com/agentgateway/agentgateway/compare/9aeb33946990d798cdc12291ba321b8d70a033b4..0180a16818256dc3614041427fe206dfe064cc48) [April 21, 2026 21:22](https://github.com/agentgateway/agentgateway/pull/1429#event-24733916261)
[danehans](https://github.com/danehans) added 6 commits [April 21, 2026 21:17](https://github.com/agentgateway/agentgateway/pull/1429#commits-pushed-381b62f)
` Add standalone inference routing local config[](https://github.com/agentgateway/agentgateway/pull/1429/commits/381b62f94f73ba653fec14ce8b664db2b4608687 "Add standalone inference routing local config

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[danehans](https://github.com/danehans) Daneyon Hansen 
GPG key ID: A9BCCF56D7755933
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

` test: add standalone inference routing coverage[](https://github.com/agentgateway/agentgateway/pull/1429/commits/1f8224bf11a8a87ea3b8d757981adf4ad6c7b1a7 "test: add standalone inference routing coverage

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[danehans](https://github.com/danehans) Daneyon Hansen 
GPG key ID: A9BCCF56D7755933
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

` test: reuse ext proc helpers for standalone routing[](https://github.com/agentgateway/agentgateway/pull/1429/commits/bc3248b28a25ec291b7a2972af9919935ab19707 "test: reuse ext proc helpers for standalone routing

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[danehans](https://github.com/danehans) Daneyon Hansen 
GPG key ID: A9BCCF56D7755933
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

` examples: add standalone EPP config example[](https://github.com/agentgateway/agentgateway/pull/1429/commits/92190c94b80a39c4f71da7cd64843cc8f1c22ad0 "examples: add standalone EPP config example

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[danehans](https://github.com/danehans) Daneyon Hansen 
GPG key ID: A9BCCF56D7755933
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

` config: simplify inference routing policies[](https://github.com/agentgateway/agentgateway/pull/1429/commits/c8336bae21e367eeef70f7dfa57ec3fe0ad2b8d0 "config: simplify inference routing policies

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[danehans](https://github.com/danehans) Daneyon Hansen 
GPG key ID: A9BCCF56D7755933
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

` config: inline inference routing parsing[](https://github.com/agentgateway/agentgateway/pull/1429/commits/c05ae1b5c7023e3c35c4796c52c7ef78e452b0c9 "config: inline inference routing parsing

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
There was an error while loading. [Please reload this page](https://github.com/agentgateway/agentgateway/pull/1429).

```
Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

[danehans](https://github.com/danehans) [force-pushed](https://github.com/agentgateway/agentgateway/compare/0180a16818256dc3614041427fe206dfe064cc48..c05ae1b5c7023e3c35c4796c52c7ef78e452b0c9) the  inference-routing-static-config branch from [`0180a16`](https://github.com/agentgateway/agentgateway/commit/0180a16818256dc3614041427fe206dfe064cc48) to [`c05ae1b`](https://github.com/agentgateway/agentgateway/commit/c05ae1b5c7023e3c35c4796c52c7ef78e452b0c9) [ Compare ](https://github.com/agentgateway/agentgateway/compare/0180a16818256dc3614041427fe206dfe064cc48..c05ae1b5c7023e3c35c4796c52c7ef78e452b0c9) [April 22, 2026 04:18](https://github.com/agentgateway/agentgateway/pull/1429#event-24741493082)
[danehans](https://github.com/danehans) added 2 commits [April 21, 2026 21:25](https://github.com/agentgateway/agentgateway/pull/1429#commits-pushed-c3caa07)
` config: simplify inference routing guard[](https://github.com/agentgateway/agentgateway/pull/1429/commits/c3caa076d4a93731c74111272ffdcc246ea45264 "config: simplify inference routing guard

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
There was an error while loading. [Please reload this page](https://github.com/agentgateway/agentgateway/pull/1429).

```
Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

` test: dedupe local config test client[](https://github.com/agentgateway/agentgateway/pull/1429/commits/7e317a3d58d472b53658a1db5f22e7473bac348f "test: dedupe local config test client

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
There was an error while loading. [Please reload this page](https://github.com/agentgateway/agentgateway/pull/1429).

```
Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

[danehans](https://github.com/danehans) marked this pull request as ready for review [April 22, 2026 06:27](https://github.com/agentgateway/agentgateway/pull/1429#event-24744133757)
[danehans](https://github.com/danehans) requested a review from a team as a [code owner](https://github.com/agentgateway/agentgateway/blob/7482fda8333c89e77c732536a13a1ec52edbcfbd/CODEOWNERS#L1) [April 22, 2026 06:27](https://github.com/agentgateway/agentgateway/pull/1429#event-24744133979)
approved these changes  [ Apr 22, 2026 ](https://github.com/agentgateway/agentgateway/pull/1429#pullrequestreview-4156155632)
[ View reviewed changes ](https://github.com/agentgateway/agentgateway/pull/1429/files/7e317a3d58d472b53658a1db5f22e7473bac348f)
### 
left a comment 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
very nice!
Hide details View details
[howardjohn](https://github.com/howardjohn) merged commit [`a80b7e3`](https://github.com/agentgateway/agentgateway/commit/a80b7e3527fa9fd33b99239d5f58f78deeaaa804) into  agentgateway:main [Apr 22, 2026](https://github.com/agentgateway/agentgateway/pull/1429#event-24763060351)
10 checks passed 
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/agentgateway/agentgateway/pull/1429).
[danehans](https://github.com/danehans) requested a review from [howardjohn](https://github.com/howardjohn) [April 22, 2026 16:06](https://github.com/agentgateway/agentgateway/pull/1429#event-24763078180)
[danehans](https://github.com/danehans) mentioned this pull request [ Apr 27, 2026 ](https://github.com/agentgateway/agentgateway/pull/1429#ref-pullrequest-4324432745)
[ Add standalone agentgateway proxy support kubernetes-sigs/gateway-api-inference-extension#2882 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882)
Merged 
[alan-bentley-pgh](https://github.com/alan-bentley-pgh) pushed a commit to solo-io/agentgateway-new-ui that referenced this pull request [ May 19, 2026 ](https://github.com/agentgateway/agentgateway/pull/1429#ref-commit-c2a074e)
` Add standalone inference routing local config ([](https://github.com/solo-io/agentgateway-new-ui/commit/c2a074ed6fd36bb898aeaf5118111d032960c58a "Add standalone inference routing local config \(#1429\)

## Summary
- add local static config support for standalone inference routing on
service route backends
- keep standalone inference routing fail-closed for now
- reuse the existing ext-proc test helpers for standalone
inference-routing coverage
- add a user-facing standalone EPP example under
`examples/standalone-epp`
- simplify the local config implementation based on review feedback and
inline inference-routing parsing

Part of #620

## Testing
`cargo test -p agentgateway inference_routing`
`cargo test -p agentgateway --test validate_examples`
`cargo run -p xtask -- schema`

---------

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>")agentgateway#1429[](https://github.com/agentgateway/agentgateway/pull/1429))[](https://github.com/solo-io/agentgateway-new-ui/commit/c2a074ed6fd36bb898aeaf5118111d032960c58a "Add standalone inference routing local config \(#1429\)

## Summary
- add local static config support for standalone inference routing on
service route backends
- keep standalone inference routing fail-closed for now
- reuse the existing ext-proc test helpers for standalone
inference-routing coverage
- add a user-facing standalone EPP example under
`examples/standalone-epp`
- simplify the local config implementation based on review feedback and
inline inference-routing parsing

Part of #620

## Testing
`cargo test -p agentgateway inference_routing`
`cargo test -p agentgateway --test validate_examples`
`cargo run -p xtask -- schema`

---------

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>") `

```
## Summary
- add local static config support for standalone inference routing on
service route backends
- keep standalone inference routing fail-closed for now
- reuse the existing ext-proc test helpers for standalone
inference-routing coverage
- add a user-facing standalone EPP example under
`examples/standalone-epp`
- simplify the local config implementation based on review feedback and
inline inference-routing parsing

Part of agentgateway#620[](https://github.com/agentgateway/agentgateway/issues/620)

## Testing
`cargo test -p agentgateway inference_routing`
`cargo test -p agentgateway --test validate_examples`
`cargo run -p xtask -- schema`

---------

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fagentgateway%2Fagentgateway%2Fpull%2F1429)
###  Labels 
None yet 
###  2 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
