---
title: 'fix: harden header sanitization and handling logic by LukeAVanDrie · Pull
  Request #2025 · kubernetes-sigs/gateway-api-inference-extension · GitHub'
id: fix-harden-header-sanitization-and-handling-logic-by-lukeavandrie-pull-request-2
tags:
- llmd-router-nonk8s-backends-54b774
- locus-availability-and-lifecycle-off-k8s
- epp-header-sanitization
- x-gateway-destination-endpoint
created: '2026-09-11T21:23:10.992070Z'
updated: '2026-09-12T03:37:37.756389Z'
source: https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025
source_domain: github.com
fetched_at: '2026-09-11T21:23:10.969239Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GIE PR #2025 (LukeAVanDrie, merged Dec 19 2025, later cherry-picked into
  llm-d/llm-d-router Apr 23 2026) hardens ext_proc header handling so a client-supplied
  x-gateway-destination-endpoint cannot override EPP''s own routing decision. Before
  the fix, generateHeaders()/generateResponseHeaders() did an ''include all headers''
  passthrough of the incoming request/response header map, so an attacker-set x-gateway-destination-endpoint
  (and other EPP-owned headers) would ride along untouched. The fix adds pkg/epp/util/request/headers.go
  IsSystemOwnedHeader(), which checks three blocklists -- InputControlHeaders (flow-fairness-id,
  objective, model-name-rewrite, subset-filter), OutputInjectionHeaders (destination-endpoint,
  destination-endpoint-served), and ProtocolHeaders (content-length) -- and silently
  drops any of these from the passthrough loop, letting only the value EPP itself
  sets (from reqCtx.TargetEndpoint) survive. PR description: ''Previously, the header
  generation logic in generateHeaders (Request) and generateResponseHeaders (Response)
  acted as a transparent passthrough, appending user-supplied headers to the outgoing
  message... These changes are necessary to prevent ambiguity in downstream proxies
  (Envoy) and backends.'' The added test TestGenerateHeaders_Sanitization directly
  proves the client-injected value is discarded: it sets metadata.DestinationEndpointKey
  to ''1.1.1.1:666'' on the inbound headers and asserts the generated header instead
  equals reqCtx.TargetEndpoint''s real value ''1.2.3.4:8080''.'
---

[Skip to content](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025).
/ **[gateway-api-inference-extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension) You must be signed in to change notification settings
  * [ Fork 313 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)
  * [ Star  766 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)


## Conversation
### 
commented [Dec 18, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#issue-3744857602)
Copy link  Copy Markdown 
Contributor
**What type of PR is this?** /kind bug
**What this PR does / why we need it** : This PR hardens the HTTP header handling logic within the extension to ensure strict ownership of protocol and system metadata headers.
Previously, the header generation logic in `generateHeaders` (Request) and `generateResponseHeaders` (Response) acted as a transparent passthrough, appending user-supplied headers to the outgoing message.
**Changes:**
  * Strict Blocklist: Introduces `requtil.IsSystemOwnedHeader` to define a specific set of headers (Protocol, Routing, and Control) that must be managed exclusively by the extension.
  * Sanitization: Updates the header generation loops to silently drop any external values for these keys.


**Reasoning:** These changes are necessary to prevent ambiguity in downstream proxies (Envoy) and backends. For example, internal routing headers should not be influenceable by external input.
**Which issue(s) this PR fixes** : Fixes [#1624](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1624)
**Does this PR introduce a user-facing change?** :

```
Harden header handling logic to enforce strict ownership of system and protocol headers, preventing potential ambiguity in backend routing and response processing.

```

[k8s-ci-robot](https://github.com/k8s-ci-robot) added the [ kind/bug ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Akind%2Fbug) Categorizes issue or PR as related to a bug. label [Dec 18, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#event-21654311088)
### 
commented [Dec 18, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#issuecomment-3672220404) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025).
Copy link  Copy Markdown   
| 
###  ✅ Deploy Preview for _gateway-api-inference-extension_ ready!  
 | Name  | Link  |  
| --- | --- |  
|  🔨 Latest commit  |  
|  🔍 Latest deploy log  | <https://app.netlify.com/projects/gateway-api-inference-extension/deploys/6944f9bee04d44000828ec56>  |  
|  😎 Deploy Preview  | <https://deploy-preview-2025--gateway-api-inference-extension.netlify.app>  |  
|  📱 Preview on mobile  |  Toggle QR Code... _Use your smartphone camera to open QR code link._  |  
_To edit notification comments on pull requests, go to your[Netlify project configuration](https://app.netlify.com/projects/gateway-api-inference-extension/configuration/notifications#deploy-notifications)._
 |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) requested a review from [danehans](https://github.com/danehans) [December 18, 2025 21:06](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#event-21654312097)
[k8s-ci-robot](https://github.com/k8s-ci-robot) added the [ cncf-cla: yes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22cncf-cla%3A%20yes%22) Indicates the PR's author has signed the CNCF CLA. label [Dec 18, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#event-21654312113)
[k8s-ci-robot](https://github.com/k8s-ci-robot) requested a review from [elevran](https://github.com/elevran) [December 18, 2025 21:06](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#event-21654312121)
### 
commented [Dec 18, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#issuecomment-3672220531)
Copy link  Copy Markdown 
Contributor  
|  Hi [@LukeAVanDrie](https://github.com/LukeAVanDrie). Thanks for your PR. I'm waiting for a [github.com](https://kubernetes-sigs/orgs/kubernetes-sigs/people) member to verify that this patch is reasonable to test. If it is, they should reply with `/ok-to-test` on its own line. Until that is done, I will not automatically test new commits in this PR, but the usual testing commands by org members will still work. Regular contributors should [join the org](https://git.k8s.io/community/community-membership.md#member) to skip this step. Once the patch is verified, the new status will be reflected by the `ok-to-test` label. I understand the commands that are listed [here](https://go.k8s.io/bot-commands?repo=kubernetes-sigs%2Fgateway-api-inference-extension). Details Instructions for interacting with me using PR comments are available [here](https://git.k8s.io/community/contributors/guide/pull-requests.md). If you have questions or suggestions related to my behavior, please file an issue against the [kubernetes-sigs/prow](https://github.com/kubernetes-sigs/prow/issues/new?title=Prow%20issue:) repository.  |  
| --- |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) added [ needs-ok-to-test ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Aneeds-ok-to-test) Indicates a PR that requires an org member to verify it is safe to test. [ size/L ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files. labels [Dec 18, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#event-21654312534)
[LukeAVanDrie](https://github.com/LukeAVanDrie) mentioned this pull request [ Dec 18, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#ref-pullrequest-3436494910)
[ fix: streaming metrics and header parsing bugs #1627 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1627)
Closed 
### 
commented [Dec 19, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#issuecomment-3673255181)
Copy link  Copy Markdown 
Contributor  
|  /ok-to-test  |  
| --- |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) added [ ok-to-test ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Aok-to-test) Indicates a non-member PR verified by an org member that is safe to test. and removed [ needs-ok-to-test ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Aneeds-ok-to-test) Indicates a PR that requires an org member to verify it is safe to test. labels [Dec 19, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#event-21660345427)
### 
commented [Dec 19, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#issuecomment-3673282781)
Copy link  Copy Markdown 
Contributor  
|  /lgtm /approve  |  
| --- |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) assigned [ahg-g](https://github.com/ahg-g) [Dec 19, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#event-21660530680)
[k8s-ci-robot](https://github.com/k8s-ci-robot) added the [ lgtm ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Algtm) "Looks good to me", indicates that a PR is ready to be merged. label [Dec 19, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#event-21660530821)
### 
commented [Dec 19, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#issuecomment-3673282983)
Copy link  Copy Markdown 
Contributor  
|  [APPROVALNOTIFIER] This PR is **APPROVED** This pull-request has been approved by: ,  The full list of commands accepted by this bot can be found [here](https://go.k8s.io/bot-commands?repo=kubernetes-sigs%2Fgateway-api-inference-extension). The pull request process is described [here](https://git.k8s.io/community/contributors/guide/owners.md#the-code-review-process) Details Needs approval from an approver in each of these files: 
  * [ahg-g]

Approvers can indicate their approval by writing `/approve` in a comment Approvers can cancel approval by writing `/approve cancel` in a comment  |  
| --- |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) added [ approved ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Aapproved) Indicates a PR has been approved by an approver from all required OWNERS files. [ needs-rebase ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Aneeds-rebase) Indicates a PR cannot be merged because it has merge conflicts with HEAD. labels [Dec 19, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#event-21660531261)
` fix: harden header sanitization and handling logic[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025/commits/95d9f1b0459763edbf2f478abee6a2cca1b906fb "fix: harden header sanitization and handling logic

This change improves the robustness of header manipulation in the
ext_proc server. It implements strict sanitization for "system-owned"
headers \(such as Content-Length and internal routing metadata\) in both
the request and response paths.

Previously, these headers were passed through transparently from the
input. This change ensures the extension maintains authoritative control
over protocol and routing headers, preventing potential ambiguity in
downstream processing.") `
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025).

```
This change improves the robustness of header manipulation in the
ext_proc server. It implements strict sanitization for "system-owned"
headers (such as Content-Length and internal routing metadata) in both
the request and response paths.

Previously, these headers were passed through transparently from the
input. This change ensures the extension maintains authoritative control
over protocol and routing headers, preventing potential ambiguity in
downstream processing.
```

[LukeAVanDrie](https://github.com/LukeAVanDrie) [force-pushed](https://github.com/kubernetes-sigs/gateway-api-inference-extension/compare/a20b48c8d843c1bd92a1d162c5ea89b5a115184d..95d9f1b0459763edbf2f478abee6a2cca1b906fb) the  fix/headers-1624 branch from [`a20b48c`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/a20b48c8d843c1bd92a1d162c5ea89b5a115184d) to [`95d9f1b`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/95d9f1b0459763edbf2f478abee6a2cca1b906fb) [ Compare ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/compare/a20b48c8d843c1bd92a1d162c5ea89b5a115184d..95d9f1b0459763edbf2f478abee6a2cca1b906fb) [December 19, 2025 07:07](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#event-21663840763)
[k8s-ci-robot](https://github.com/k8s-ci-robot) removed [ lgtm ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Algtm) "Looks good to me", indicates that a PR is ready to be merged. [ needs-rebase ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Aneeds-rebase) Indicates a PR cannot be merged because it has merge conflicts with HEAD. labels [Dec 19, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#event-21663841127)
### 
commented [Dec 19, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#issuecomment-3675993332)
Copy link  Copy Markdown 
Contributor  
|  /lgtm  |  
| --- |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) added the [ lgtm ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Algtm) "Looks good to me", indicates that a PR is ready to be merged. label [Dec 19, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#event-21675576137)
Hide details View details
[k8s-ci-robot](https://github.com/k8s-ci-robot) merged commit [`5ec1fa1`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/5ec1fa1d9a7ecc83e0ceee57c2d1abc656aa2716) into  kubernetes-sigs:main [Dec 19, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#event-21675602939)
12 checks passed 
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025).
reviewed  [ Dec 19, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#pullrequestreview-3599622299)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025/files/95d9f1b0459763edbf2f478abee6a2cca1b906fb)
Comment thread  
|  var (  |  
| --- |  
|  // InputControlHeaders are sent by the Gateway/User to control EPP behavior.  |  
|  // We must extract these, then strip them so they don't leak to the backend.  |  
|  InputControlHeaders = map[string]bool{  |  
### 
[Dec 19, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#discussion_r2635944538) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025).
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
nit: there’s no meaning for the “true” here, and this map acts as a hash set. could be nice to use “set” from k8s utils instead of this map, and use the set.Has function (it’s backed by a map to struct{}).
it reads clearer IMO. (if you want to push in a follow up)
### 
[Dec 19, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#discussion_r2635947002)
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
example here: 
[gateway-api-inference-extension/pkg/epp/config/loader/configloader.go](https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/5ec1fa1d9a7ecc83e0ceee57c2d1abc656aa2716/pkg/epp/config/loader/configloader.go#L122)
Line 122 in [5ec1fa1](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/5ec1fa1d9a7ecc83e0ceee57c2d1abc656aa2716)  
|  pluginNames := sets.New[string]()   |  
| --- |  
### 
[Dec 19, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#discussion_r2635970462)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I considered this but figured `map[string]bool` or `map[string]struct{}` was idiomatic enough. The set utility reads nicer though, especially now that we have generics. Will send out a small followup!
[LukeAVanDrie](https://github.com/LukeAVanDrie) mentioned this pull request [ Dec 21, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#ref-pullrequest-3751688751)
[ cleanup: Migrate raw map sets to k8s.io/apimachinery/pkg/util/sets #2030 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2030)
Merged 
[davidbreitgand](https://github.com/davidbreitgand) pushed a commit to davidbreitgand/gateway-api-inference-extension that referenced this pull request [ Jan 6, 2026 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#ref-commit-cb76f38)
` fix: harden header sanitization and handling logic ([](https://github.com/davidbreitgand/gateway-api-inference-extension/commit/cb76f38b4702c943afd8f22ee0b710f1fe7fec54 "fix: harden header sanitization and handling logic \(#2025\)

This change improves the robustness of header manipulation in the
ext_proc server. It implements strict sanitization for "system-owned"
headers \(such as Content-Length and internal routing metadata\) in both
the request and response paths.

Previously, these headers were passed through transparently from the
input. This change ensures the extension maintains authoritative control
over protocol and routing headers, preventing potential ambiguity in
downstream processing.")kubernetes-sigs#2025[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025) `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[davidbreitgand](https://github.com/davidbreitgand) David Breitgand 
SSH Key Fingerprint: a5xMeKXcdMrf0NEqIY7Aq8JsA3jhuSmaI2ZePmv2h8M
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
)

This change improves the robustness of header manipulation in the
ext_proc server. It implements strict sanitization for "system-owned"
headers (such as Content-Length and internal routing metadata) in both
the request and response paths.

Previously, these headers were passed through transparently from the
input. This change ensures the extension maintains authoritative control
over protocol and routing headers, preventing potential ambiguity in
downstream processing.
```

[elevran](https://github.com/elevran) pushed a commit to llm-d/llm-d-router that referenced this pull request [ Apr 23, 2026 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025#ref-commit-bf6e842)
` fix: harden header sanitization and handling logic ([](https://github.com/llm-d/llm-d-router/commit/bf6e842d8d875826f321ba75306f5baad24fb86a "fix: harden header sanitization and handling logic \(kubernetes-sigs/gateway-api-inference-extension#2025\)

This change improves the robustness of header manipulation in the
ext_proc server. It implements strict sanitization for "system-owned"
headers \(such as Content-Length and internal routing metadata\) in both
the request and response paths.

Previously, these headers were passed through transparently from the
input. This change ensures the extension maintains authoritative control
over protocol and routing headers, preventing potential ambiguity in
downstream processing.")kubernetes-sigs/g…[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025) `

```
…ateway-api-inference-extension#2025[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025))

This change improves the robustness of header manipulation in the
ext_proc server. It implements strict sanitization for "system-owned"
headers (such as Content-Length and internal routing metadata) in both
the request and response paths.

Previously, these headers were passed through transparently from the
input. This change ensures the extension maintains authoritative control
over protocol and routing headers, preventing potential ambiguity in
downstream processing.
```

[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fkubernetes-sigs%2Fgateway-api-inference-extension%2Fpull%2F2025)
###  Labels 
[ approved ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Aapproved) Indicates a PR has been approved by an approver from all required OWNERS files. [ cncf-cla: yes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22cncf-cla%3A%20yes%22) Indicates the PR's author has signed the CNCF CLA. [ kind/bug ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Akind%2Fbug) Categorizes issue or PR as related to a bug. [ lgtm ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Algtm) "Looks good to me", indicates that a PR is ready to be merged. [ ok-to-test ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Aok-to-test) Indicates a non-member PR verified by an org member that is safe to test. [ size/L ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files.
###  4 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
