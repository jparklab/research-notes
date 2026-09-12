---
title: 'helm support for sidecar injection in EPP  by capri-xiyue · Pull Request #1821
  · kubernetes-sigs/gateway-api-inference-extension · GitHub'
id: helm-support-for-sidecar-injection-in-epp-by-capri-xiyue-pull-request-1821-kuber
tags:
- llmd-router-nonk8s-backends-54b774
- standalone-epp
- gie-design
- helm
- sidecar-injection
created: '2026-09-11T20:49:16.905236Z'
updated: '2026-09-12T03:37:37.690396Z'
source: https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821
source_domain: github.com
fetched_at: '2026-09-11T20:49:16.880755Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GIE PR #1821 (capri-xiyue, opened Nov 5 2025, fixes issue #1778) originally
  titled ''helm support for EPP standalone mode with Envoy Proxy'' but retitled ''helm
  support for sidecar injection in EPP'' after review -- the rename itself is evidence
  of the maintainers'' preferred framing: this is generic sidecar-proxy injection
  into the EPP Helm chart, not a standalone-EPP-specific chart. Author capri-xiyue
  clarifies the scope explicitly: ''For this stage, EPP standalone mode still uses
  InferencePool. That''s why I didn''t put it in a distinct chart. Later, if we decide
  to have a standalone EPP without any k8s crd, I will refactor it to a distinct helm
  chart. To clarify, this PR just removes gateway api dependency, inferencepool api
  is still there.'' This maps directly onto GIE issue #1838''s deployment-mode taxonomy:
  PR #1821 implements mode 2.2 (extension CRDs/InferencePool retained, proxy statically
  configured as a sidecar, no Gateway API dependency) as a stepping stone, explicitly
  deferring the harder mode-1/2.1 case (EPP with zero k8s CRD dependency) to future
  work. Reviewer kfswain pushed for making the chart''s sidecar-injection hooks generic/vendor-agnostic
  (''offer the necessary hooks to enable that... allow customizing it by an external
  one to allow those other deployment patterns'') rather than hardcoding a specific
  proxy; capri-xiyue''s response confirms the PR''s actual deliverable is generic
  ''additional sidecar proxy injection'' support in the Helm chart.'
utility_score: 14.0
---

[Skip to content](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
/ **[gateway-api-inference-extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension) You must be signed in to change notification settings
  * [ Fork 313 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)
  * [ Star  766 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)


## Conversation
### 
commented [Nov 5, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#issue-3592841806)
Copy link  Copy Markdown 
Contributor
**What type of PR is this?**
**What this PR does / why we need it** : see [#1778](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1778)
**Which issue(s) this PR fixes** :
Fixes [#1778](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1778)
**Does this PR introduce a user-facing change?** :

```
NONE

```

[capri-xiyue](https://github.com/capri-xiyue) added 4 commits [November 3, 2025 16:35](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#commits-pushed-67d8bc9)
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
GPG key ID: B8E967D031AB7EAA
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
GPG key ID: B8E967D031AB7EAA
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
GPG key ID: B8E967D031AB7EAA
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
GPG key ID: B8E967D031AB7EAA
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
### 
commented [Nov 5, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#issuecomment-3493513395) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
Copy link  Copy Markdown   
| 
###  ✅ Deploy Preview for _gateway-api-inference-extension_ ready!  
 | Name  | Link  |  
| --- | --- |  
|  🔨 Latest commit  |  
|  🔍 Latest deploy log  | <https://app.netlify.com/projects/gateway-api-inference-extension/deploys/691781c63eca46000871b2e7>  |  
|  😎 Deploy Preview  | <https://deploy-preview-1821--gateway-api-inference-extension.netlify.app>  |  
|  📱 Preview on mobile  |  Toggle QR Code... _Use your smartphone camera to open QR code link._  |  
_To edit notification comments on pull requests, go to your[Netlify project configuration](https://app.netlify.com/projects/gateway-api-inference-extension/configuration/notifications#deploy-notifications)._
 |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) requested review from [liu-cong](https://github.com/liu-cong) and [nirrozenbaum](https://github.com/nirrozenbaum) [November 5, 2025 21:17](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#event-20741687486)
[k8s-ci-robot](https://github.com/k8s-ci-robot) added [ cncf-cla: yes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22cncf-cla%3A%20yes%22) Indicates the PR's author has signed the CNCF CLA. [ size/XL ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FXL) Denotes a PR that changes 500-999 lines, ignoring generated files. labels [Nov 5, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#event-20741687563)
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
GPG key ID: B8E967D031AB7EAA
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
### 
commented [Nov 5, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#issuecomment-3493624244)
Copy link  Copy Markdown 
Contributor  
|  Thanks [@capri-xiyue](https://github.com/capri-xiyue)! Can we make this a distinct helm chart? Since we call this current chart `inferencePool` it would be odd to have a mode in that chart that doesn't even use inferencePool.  |  
| --- |  
### 
commented [Nov 5, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#issuecomment-3493697710) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
Copy link  Copy Markdown 
Contributor Author  
|  [@kfswain](https://github.com/kfswain) For this stage, EPP standalone mode still uses InferencePool. That's why I didn't put it in a distinct chart. Later, if we decide to have a standalone EPP without any k8s crd, I will refactor it to a distinct helm chart. To clarify, this PR just removes gateway api dependency, inferencepool api is still used.
> Thanks [@capri-xiyue](https://github.com/capri-xiyue)! Can we make this a distinct helm chart? Since we call this current chart `inferencePool` it would be odd to have a mode in that chart that doesn't even use inferencePool.
 |  
| --- |  
### 
commented [Nov 5, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#issuecomment-3493699524)
Copy link  Copy Markdown 
Contributor Author  
|  /assign [@ahg-g](https://github.com/ahg-g)  |  
| --- |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) assigned [ahg-g](https://github.com/ahg-g) [Nov 5, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#event-20742514720)
### 
commented [Nov 5, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#issuecomment-3493727605)
Copy link  Copy Markdown 
Contributor  
| 
> [@kfswain](https://github.com/kfswain) For this stage, EPP standalone mode still uses InferencePool. That's why I didn't put it in a distinct chart. Later, if we decide to have a standalone EPP without any k8s crd, I will refactor it to a distinct helm chart.
>> Thanks [@capri-xiyue](https://github.com/capri-xiyue)! Can we make this a distinct helm chart? Since we call this current chart `inferencePool` it would be odd to have a mode in that chart that doesn't even use inferencePool.
That may lead to confusing UX in the case where this is deployed in a cluster that has a GW controller, as it will attempt to reconcile on the InferencePool and integrate it into the GW system, is modifying EPP to just accept a selector not a viable path forward?  |  
| --- |  
### 
commented [Nov 5, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#issuecomment-3493759528) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
Copy link  Copy Markdown 
Contributor Author  
| 
> > [@kfswain](https://github.com/kfswain) For this stage, EPP standalone mode still uses InferencePool. That's why I didn't put it in a distinct chart. Later, if we decide to have a standalone EPP without any k8s crd, I will refactor it to a distinct helm chart.
>>> Thanks [@capri-xiyue](https://github.com/capri-xiyue)! Can we make this a distinct helm chart? Since we call this current chart `inferencePool` it would be odd to have a mode in that chart that doesn't even use inferencePool.
> That may lead to confusing UX in the case where this is deployed in a cluster that has a GW controller, as it will attempt to reconcile on the InferencePool and integrate it into the GW system, is modifying EPP to just accept a selector not a viable path forward?
I talked with [@ahg-g](https://github.com/ahg-g) before, modifying EPP to just accept a selector needs further discussion. Therefore he suggested me finalizing EPP with envoy proxy first with helm chart. Curious now what will happen when a inference pool deployed in a cluster with two GW controller?(for example kgateway and istio), will it cause issues here? Initially I thought each GW controller is able to handle this case.  |  
| --- |  
### 
commented [Nov 5, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#issuecomment-3493841093)
Copy link  Copy Markdown 
Contributor  
|  Yeah, my suggestion is to take a gradual approach, a gateway controller should not care about an inferencePool that is not referenced by an httpRoute.  |  
| --- |  
reviewed  [ Nov 5, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#pullrequestreview-3424816410)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821/files/6d06c165269b793767f5e1c5282564c703454ea6)
Comment thread [config/charts/inferencepool/templates/epp-deployment.yaml](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821/files/6d06c165269b793767f5e1c5282564c703454ea6#diff-28a24ec70476bf560f8f9f29d7f86c06583c979edfbf4d2b7d8680b1c69051c9) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
` move epp-envoy to a standalone file[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821/commits/92d36bd2ee162d16e2f45d8905a5c9e0d8766505 "move epp-envoy to a standalone file") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
GPG key ID: B8E967D031AB7EAA
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
[capri-xiyue](https://github.com/capri-xiyue) requested a review from [ahg-g](https://github.com/ahg-g) [November 6, 2025 00:28](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#event-20744848364)
[k8s-ci-robot](https://github.com/k8s-ci-robot) added [ size/L ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files. and removed [ size/XL ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FXL) Denotes a PR that changes 500-999 lines, ignoring generated files. labels [Nov 6, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#event-20744848838)
### 
commented [Nov 6, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#issuecomment-3494729017) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
Copy link  Copy Markdown 
Contributor Author  
|  As an update, I'm now working on another PR to modify EPP to just accept a selector and will refactor the helm chart to have a distinct one as no inference pool is needed in that PR. EPP refactor probably takes a little while as fix bunch of ut takes time. Will let you know when I send a PR. [@kfswain](https://github.com/kfswain)  |  
| --- |  
reviewed  [ Nov 7, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#pullrequestreview-3435108181)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821/files/92d36bd2ee162d16e2f45d8905a5c9e0d8766505)
Comment thread [config/charts/inferencepool/values.yaml](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821/files/92d36bd2ee162d16e2f45d8905a5c9e0d8766505#diff-49f9c779298b67f3c89b7c668e7f026d302212b742349015dbd90f2c303d1076) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
reviewed  [ Nov 7, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#pullrequestreview-3435113117)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821/files/92d36bd2ee162d16e2f45d8905a5c9e0d8766505)
Comment thread [config/charts/inferencepool/values.yaml](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821/files/92d36bd2ee162d16e2f45d8905a5c9e0d8766505#diff-49f9c779298b67f3c89b7c668e7f026d302212b742349015dbd90f2c303d1076) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
GPG key ID: B8E967D031AB7EAA
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
[k8s-ci-robot](https://github.com/k8s-ci-robot) added [ size/XL ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FXL) Denotes a PR that changes 500-999 lines, ignoring generated files. and removed [ size/L ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files. labels [Nov 7, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#event-20794792730)
[capri-xiyue](https://github.com/capri-xiyue) requested a review from [ahg-g](https://github.com/ahg-g) [November 7, 2025 18:18](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#event-20794807678)
### 
commented [Nov 7, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#issuecomment-3504566063) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
Copy link  Copy Markdown 
Contributor  
|  IMHO we need to get the [#1778](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1778) design gdoc that [@ahg-g](https://github.com/ahg-g) created into an md and added to `docs/proposals`.  |  
| --- |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) added the [ size/L ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files. label [Nov 13, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#event-20924977090)
[capri-xiyue](https://github.com/capri-xiyue) added 3 commits [November 13, 2025 11:16](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#commits-pushed-b294e51)
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
GPG key ID: B8E967D031AB7EAA
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
GPG key ID: B8E967D031AB7EAA
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
GPG key ID: B8E967D031AB7EAA
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
[k8s-ci-robot](https://github.com/k8s-ci-robot) added [ size/XL ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FXL) Denotes a PR that changes 500-999 lines, ignoring generated files. and removed [ size/L ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files. labels [Nov 14, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#event-20930396238)
### 
commented [Nov 14, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#issuecomment-3530337089)
Copy link  Copy Markdown 
Contributor Author  
| 
> > With that being said, I agree with the point of splitting the vendor specific to a separate repo provided that we offer the necessary "hooks" to enable that. So my suggestion is to make the necessary changes in this chart to enable customizing it by an external one to allow those other deployment patterns.
> [@capri-xiyue](https://github.com/capri-xiyue) isn't all needed here to enable the above is to allow adding a sidecar proxy to the epp deployment?
Yes. I've updated it to enable additional sidecar proxy injection.  |  
| --- |  
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
GPG key ID: B8E967D031AB7EAA
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
[k8s-ci-robot](https://github.com/k8s-ci-robot) added [ size/L ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files. and removed [ size/XL ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FXL) Denotes a PR that changes 500-999 lines, ignoring generated files. labels [Nov 14, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#event-20930427340)
[capri-xiyue](https://github.com/capri-xiyue) changed the title ~~helm support for EPP standalone mode with Envoy Proxy~~ helm support for sidecar injection in EPP  [Nov 14, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#event-20930453220)
[capri-xiyue](https://github.com/capri-xiyue) requested review from [ahg-g](https://github.com/ahg-g) and [kfswain](https://github.com/kfswain) [November 14, 2025 17:58](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#event-20949502916)
reviewed  [ Nov 14, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#pullrequestreview-3466140334)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821/files/2b594750674cf45d9183b4cd4b4bab630777abf5)
### 
left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Great!
[@danehans](https://github.com/danehans) can you please make sure this works for agentgateway?
Comment thread [config/charts/inferencepool/templates/epp-deployment.yaml](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821/files/2b594750674cf45d9183b4cd4b4bab630777abf5#diff-28a24ec70476bf560f8f9f29d7f86c06583c979edfbf4d2b7d8680b1c69051c9) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
Comment thread [config/charts/inferencepool/templates/epp-deployment.yaml](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821/files/2b594750674cf45d9183b4cd4b4bab630777abf5#diff-28a24ec70476bf560f8f9f29d7f86c06583c979edfbf4d2b7d8680b1c69051c9) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
Comment thread [config/charts/inferencepool/values.yaml](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821/files/2b594750674cf45d9183b4cd4b4bab630777abf5#diff-49f9c779298b67f3c89b7c668e7f026d302212b742349015dbd90f2c303d1076) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
Comment thread [config/charts/inferencepool/values.yaml](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821/files/2b594750674cf45d9183b4cd4b4bab630777abf5#diff-49f9c779298b67f3c89b7c668e7f026d302212b742349015dbd90f2c303d1076) Outdated   
|  # # Because the template just dumps this section, the keys become filenames.  |  
| --- |  
|  # # The values MUST be strings (note the literal block scalar '|')  |  
|  # data:  |  
|  # envoy.yaml: |  |  
### 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
What assumptions are we making in this configuration with regards to the EPP configuration, the configuration of the model servers and inferencePool? Put another way, is this configuration assuming that the epp/servers/infoPool is configuration in a specific way?
### 
[Nov 14, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#discussion_r2528551613) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
The example envoy config only assumes the communication between envoy proxy and epp communication. It has nothing to do with model server and inference pool. By the way, such envoy config can be customized by users. Users can use what ever config they want to use.
### 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Ideally we want a config that just works, and the helm chart we offer should not even offer any customization for this config.
### 
[Nov 14, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#discussion_r2528580810) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Yep. The current default config works as I just disable side car injection. By the way, originally the example envoy config was commented already. Now I just removed it as it will live in llm-d
### 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
> The current default config works as I just disable side car injection
I was referring to the envoy config, that config in llm-d should just work and not be customizable.
### 
[Nov 14, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#discussion_r2528667832)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Yep. The envoy config in llm-d will just work and won't be customizable. I tested it with no inference pool dependency. Will ping you when I open a PR in llm-d
` added more customized info and removed example[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821/commits/f8400915705c90b52a661434c9fa129e02b63e81 "added more customized info and removed example") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
GPG key ID: B8E967D031AB7EAA
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
[k8s-ci-robot](https://github.com/k8s-ci-robot) added [ size/M ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FM) Denotes a PR that changes 30-99 lines, ignoring generated files. and removed [ size/L ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files. labels [Nov 14, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#event-20950382232)
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
GPG key ID: B8E967D031AB7EAA
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
[capri-xiyue](https://github.com/capri-xiyue) requested a review from [ahg-g](https://github.com/ahg-g) [November 14, 2025 18:51](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#event-20950403522)
reviewed  [ Nov 14, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#pullrequestreview-3466460579)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821/files/bd809e59e9210b947a57ff54e684ab213405817a)
Comment thread [config/charts/inferencepool/templates/epp-deployment.yaml](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821/files/bd809e59e9210b947a57ff54e684ab213405817a#diff-28a24ec70476bf560f8f9f29d7f86c06583c979edfbf4d2b7d8680b1c69051c9) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
reviewed  [ Nov 14, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#pullrequestreview-3466463839)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821/files/bd809e59e9210b947a57ff54e684ab213405817a)
Comment thread [config/charts/inferencepool/values.yaml](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821/files/bd809e59e9210b947a57ff54e684ab213405817a#diff-49f9c779298b67f3c89b7c668e7f026d302212b742349015dbd90f2c303d1076) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
` fixed default image pull policy[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821/commits/fd9cdea8261ceb4bfd17bebbbe418a4e162b4d5e "fixed default image pull policy") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
GPG key ID: B8E967D031AB7EAA
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
[capri-xiyue](https://github.com/capri-xiyue) requested a review from [ahg-g](https://github.com/ahg-g) [November 14, 2025 19:24](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#event-20951034324)
### 
commented [Nov 14, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#issuecomment-3534378268)
Copy link  Copy Markdown 
Contributor  
|  /lgtm /approve Great!  |  
| --- |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) added the [ lgtm ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Algtm) "Looks good to me", indicates that a PR is ready to be merged. label [Nov 14, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#event-20951893578)
### 
commented [Nov 14, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#issuecomment-3534379039)
Copy link  Copy Markdown 
Contributor  
|  [APPROVALNOTIFIER] This PR is **APPROVED** This pull-request has been approved by: ,  The full list of commands accepted by this bot can be found [here](https://go.k8s.io/bot-commands?repo=kubernetes-sigs%2Fgateway-api-inference-extension). The pull request process is described [here](https://git.k8s.io/community/contributors/guide/owners.md#the-code-review-process) Details Needs approval from an approver in each of these files: 
  * [ahg-g]

Approvers can indicate their approval by writing `/approve` in a comment Approvers can cancel approval by writing `/approve cancel` in a comment  |  
| --- |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) added the [ approved ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Aapproved) Indicates a PR has been approved by an approver from all required OWNERS files. label [Nov 14, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#event-20951894610)
Hide details View details
[k8s-ci-robot](https://github.com/k8s-ci-robot) merged commit [`81deb19`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/81deb1923da27095292a80103f39ef660d008c67) into  kubernetes-sigs:main [Nov 14, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#event-20951919527)
11 checks passed 
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821).
[rlakhtakia](https://github.com/rlakhtakia) pushed a commit to rlakhtakia/gateway-api-inference-extension that referenced this pull request [ Nov 17, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#ref-commit-02e667f)
` helm support for sidecar injection in EPP ([](https://github.com/rlakhtakia/gateway-api-inference-extension/commit/02e667f377c3e245669d5a33e13674be068f05a3 "helm support for sidecar injection in EPP  \(#1821\)

* draft

* draft

* workable version

* added docs

* fixed format

* move epp-envoy to a standalone file

* make standalone a provider

* removed unused change

* remove unused change

* syntax fix

* workable version

* remove unused yaml

* added more customized info and removed example

* fixed typo

* fixed default image pull policy")kubernetes-sigs#1821[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821))[](https://github.com/rlakhtakia/gateway-api-inference-extension/commit/02e667f377c3e245669d5a33e13674be068f05a3 "helm support for sidecar injection in EPP  \(#1821\)

* draft

* draft

* workable version

* added docs

* fixed format

* move epp-envoy to a standalone file

* make standalone a provider

* removed unused change

* remove unused change

* syntax fix

* workable version

* remove unused yaml

* added more customized info and removed example

* fixed typo

* fixed default image pull policy") `

```
* draft

* draft

* workable version

* added docs

* fixed format

* move epp-envoy to a standalone file

* make standalone a provider

* removed unused change

* remove unused change

* syntax fix

* workable version

* remove unused yaml

* added more customized info and removed example

* fixed typo

* fixed default image pull policy
```

[delavet](https://github.com/delavet) mentioned this pull request [ Dec 8, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1821#ref-pullrequest-3693659821)
[ feature: (helm) support custom volumes and volumeMounts for epp #1945 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1945)
Merged 
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fkubernetes-sigs%2Fgateway-api-inference-extension%2Fpull%2F1821)
###  Labels 
[ approved ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Aapproved) Indicates a PR has been approved by an approver from all required OWNERS files. [ cncf-cla: yes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22cncf-cla%3A%20yes%22) Indicates the PR's author has signed the CNCF CLA. [ lgtm ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Algtm) "Looks good to me", indicates that a PR is ready to be merged. [ size/M ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FM) Denotes a PR that changes 30-99 lines, ignoring generated files.
###  7 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
