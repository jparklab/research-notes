---
title: 'Add standalone agentgateway proxy support by danehans · Pull Request #2882
  · kubernetes-sigs/gateway-api-inference-extension · GitHub'
id: add-standalone-agentgateway-proxy-support-by-danehans-pull-request-2882-kubernet
tags:
- llmd-router-nonk8s-backends-54b774
- standalone-epp
- agentgateway
- helm
- gie-design
created: '2026-09-11T21:03:58.255333Z'
updated: '2026-09-15T04:55:23.911468Z'
source: https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882
source_domain: github.com
fetched_at: '2026-09-11T21:03:58.234131Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GIE PR #2882 (danehans, opened Apr 24 2026, fixes issue #2883, approved)
  adds standalone Helm chart support for running agentgateway as the EPP sidecar proxy:
  ''Adds standalone Helm chart support for using agentgateway as the EPP sidecar proxy.
  This includes proxy preset selection, agentgateway validation, generated service-backed
  agentgateway local config, a generated model Service, release-tooling tag replacement,
  and standalone guide updates.'' User-facing change: ''The standalone Helm chart
  can now run agentgateway as the sidecar proxy by setting inferenceExtension.sidecar.proxyType=agentgateway.''
  This is the concrete, mergeable GIE-side chart change that wires up agentgateway''s
  PR #1658/#1429 passthrough destinationMode work into the official standalone-EPP
  Helm chart -- confirming ''proxyType=agentgateway'' is now a first-class, officially
  supported sidecar option for standalone EPP deployments, alongside the earlier Envoy-based
  default. Testing was done live via a kind e2e cluster using the vLLM simulator and
  a patched agentgateway image built from PR #1658, directly cross-linking this PR
  to the destination-trust/passthrough work as its validation vehicle. Notably still
  generates ''a generated model Service'' -- i.e. even this agentgateway-based standalone
  chart path still creates a Kubernetes Service object for the model backend, reinforcing
  that full non-k8s backend support requires further work beyond this chart (a Service
  is still the addressing anchor).'
---

*Suggested by [[trust-standalone-epp-selected-destinations-by-danehans-pull-request-1658-agentga]] — e2e test companion PR for agentgateway passthrough destinationMode*

[Skip to content](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882).
/ **[gateway-api-inference-extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension) You must be signed in to change notification settings
  * [ Fork 313 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)
  * [ Star  766 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)


## Conversation
### 
commented [Apr 24, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#issue-4324432745) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882).
Copy link  Copy Markdown 
Contributor
**What type of PR is this?** /kind feature /kind documentation
**What this PR does / why we need it** : Adds standalone Helm chart support for using agentgateway as the EPP sidecar proxy.
This includes proxy preset selection, agentgateway validation, generated service-backed agentgateway local config, a generated model Service, release-tooling tag replacement, and standalone guide updates.
**Which issue(s) this PR fixes** : Fixes: [#2883](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2883)
**Does this PR introduce a user-facing change?** :

```
The standalone Helm chart can now run agentgateway as the sidecar proxy by setting inferenceExtension.sidecar.proxyType=agentgateway.

```

**Testing** :
  * `./hack/verify-helm.sh local`
  * `python3.11 -m mkdocs build` with existing repository warnings
  * Live kind e2e with the vLLM simulator and a [patched agentgateway image](https://github.com/agentgateway/agentgateway/pull/1658)


[k8s-ci-robot](https://github.com/k8s-ci-robot) added [ kind/feature ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. [ kind/documentation ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Akind%2Fdocumentation) Categorizes issue or PR as related to documentation. labels [Apr 24, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#event-24845794345)
### 
commented [Apr 24, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#issuecomment-4314928376)
Copy link  Copy Markdown 
Contributor  
|  [APPROVALNOTIFIER] This PR is **APPROVED** This pull-request has been approved by:  The full list of commands accepted by this bot can be found [here](https://go.k8s.io/bot-commands?repo=kubernetes-sigs%2Fgateway-api-inference-extension). The pull request process is described [here](https://git.k8s.io/community/contributors/guide/owners.md#the-code-review-process) Details Needs approval from an approver in each of these files: 
  * [danehans]

Approvers can indicate their approval by writing `/approve` in a comment Approvers can cancel approval by writing `/approve cancel` in a comment  |  
| --- |  
### 
commented [Apr 24, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#issuecomment-4314928405) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882).
Copy link  Copy Markdown   
| 
###  ✅ Deploy Preview for _gateway-api-inference-extension_ ready!  
 | Name  | Link  |  
| --- | --- |  
|  🔨 Latest commit  |  
|  🔍 Latest deploy log  | <https://app.netlify.com/projects/gateway-api-inference-extension/deploys/69ebbc7f537e870008add797>  |  
|  😎 Deploy Preview  | <https://deploy-preview-2882--gateway-api-inference-extension.netlify.app>  |  
|  📱 Preview on mobile  |  Toggle QR Code... _Use your smartphone camera to open QR code link._  |  
_To edit notification comments on pull requests, go to your[Netlify project configuration](https://app.netlify.com/projects/gateway-api-inference-extension/configuration/notifications#deploy-notifications)._
 |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) requested review from [kfswain](https://github.com/kfswain) and [nirrozenbaum](https://github.com/nirrozenbaum) [April 24, 2026 17:03](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#event-24845796650)
[k8s-ci-robot](https://github.com/k8s-ci-robot) added [ approved ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Aapproved) Indicates a PR has been approved by an approver from all required OWNERS files. [ size/XL ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FXL) Denotes a PR that changes 500-999 lines, ignoring generated files. [ cncf-cla: yes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22cncf-cla%3A%20yes%22) Indicates the PR's author has signed the CNCF CLA. [ size/XXL ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FXXL) Denotes a PR that changes 1000+ lines, ignoring generated files. and removed [ size/XL ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FXL) Denotes a PR that changes 500-999 lines, ignoring generated files. labels [Apr 24, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#event-24845796919)
[danehans](https://github.com/danehans) added 3 commits [April 24, 2026 11:07](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#commits-pushed-142f657)
` Add standalone agentgateway proxy support[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882/commits/142f657cc3b85b51636d960c73be79882ec64c21 "Add standalone agentgateway proxy support

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

` Harden standalone agentgateway chart ports[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882/commits/6af99ed3a6fa689c31b3f246937c3fda8a73aabe "Harden standalone agentgateway chart ports

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

` Avoid standalone service label collision[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882/commits/d9661ed5ad9b515e107164a9b1de4f81bdd0e52e "Avoid standalone service label collision

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
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882).

```
Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

[danehans](https://github.com/danehans) [force-pushed](https://github.com/kubernetes-sigs/gateway-api-inference-extension/compare/023983f100354f06f32493baf373dbc4e9c7185e..d9661ed5ad9b515e107164a9b1de4f81bdd0e52e) the  standalone-agentgateway branch from [`023983f`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/023983f100354f06f32493baf373dbc4e9c7185e) to [`d9661ed`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/d9661ed5ad9b515e107164a9b1de4f81bdd0e52e) [ Compare ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/compare/023983f100354f06f32493baf373dbc4e9c7185e..d9661ed5ad9b515e107164a9b1de4f81bdd0e52e) [April 24, 2026 18:11](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#event-24847731988)
[danehans](https://github.com/danehans) added 2 commits [April 24, 2026 11:36](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#commits-pushed-1ddaf6e)
` Make agentgateway listener port deterministic[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882/commits/1ddaf6e05cbe7918b706e7cf55cfeed37e585729 "Make agentgateway listener port deterministic

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
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882).

```
Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

` Set agentgateway EPP destination passthrough[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882/commits/db61f4920ef44c10035e39a6522135ce78a213ff "Set agentgateway EPP destination passthrough

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
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882).

```
Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

[danehans](https://github.com/danehans) mentioned this pull request [ Apr 24, 2026 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#ref-pullrequest-4324430998)
[ Trust standalone EPP-selected destinations agentgateway/agentgateway#1658 ](https://github.com/agentgateway/agentgateway/pull/1658)
Merged 
### 
commented [Apr 24, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#issuecomment-4315913134)
Copy link  Copy Markdown 
Contributor  
|  Thanks [@danehans](https://github.com/danehans) this is great, did we also test and validated that there are no regressions to the original envoy-based approach as well?  |  
| --- |  
### 
commented [Apr 24, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#issuecomment-4317050408)
Copy link  Copy Markdown 
Contributor Author  
|  cc: [@ahg-g](https://github.com/ahg-g) [@zetxqx](https://github.com/zetxqx)  |  
| --- |  
### 
commented [Apr 27, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#issuecomment-4328267890)
Copy link  Copy Markdown 
Contributor Author  
| 
> did we also test and validated that there are no regressions to the original envoy-based approach as well?
Yes, I manually tested for backwards compatibility (both `proxyType` unspecified and `proxyType: envoy`). I was considering adding CI coverage similar to [#2864](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2864) but I would like to address CI coverage in broader terms and also consider the repo move (no need to add coverage here if llm-d is the future).  |  
| --- |  
### 
commented [Apr 27, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#issuecomment-4328560156)
Copy link  Copy Markdown 
Contributor Author  
|  [@ahg-g](https://github.com/ahg-g) now that [agentgateway/agentgateway#1429](https://github.com/agentgateway/agentgateway/pull/1429) merged, I retested using vllm-sim and the agentgateway `latest-dev` tag. I created 3 different deployments for each supported proxyType (unspecified=`envoy` for backwards compatability, `envoy`, and `agentgateway`) and each worked as expected: 
```
$ kubectl get svc -A
NAMESPACE                                  NAME                                                 TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                        AGE
...
standalone-ag-latest-20260427083311        vllm-qwen3-32b-standalone-epp                        ClusterIP      10.96.112.100   none        9002/TCP,9090/TCP,8081/TCP     30m
standalone-envoy-default-20260427083347    vllm-qwen3-32b-standalone-epp                        ClusterIP      10.96.140.143   none        9002/TCP,9090/TCP,8081/TCP     29m
standalone-envoy-explicit-20260427083359   vllm-qwen3-32b-standalone-epp                        ClusterIP      10.96.113.34    none        9002/TCP,9090/TCP,8081/TCP     29m

$ kubectl exec -n standalone-ag-latest-20260427083311 curl -- curl -i \
  http://vllm-qwen3-32b-standalone-epp:8081/v1/completions \
  -H 'Content-Type: application/json' \
  -d '{"model":"Qwen/Qwen3-32B","prompt":"Write as if you were a critic: San Francisco","max_tokens":100,"temperature":0}'

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0HTTP/1.1 200 OK
server: fasthttp
date: Mon, 27 Apr 2026 16:04:18 GMT
content-type: application/json
x-inference-port: 8000
x-inference-pod: vllm-qwen3-32b-5c8c976cdd-vmvvl
x-went-into-resp-headers: true
transfer-encoding: chunked

{:"cmpl-fe3190c3-36f4-5709-9f4f-fcbfc233517c","created":1777305859,"model":"Qwen/Qwen3-32B","usage":{"prompt_tokens":10,"completion_tokens":81,"total_tokens":91},"object":"text_completion","kv_transfer_params":null,"choices":[{"index":0,"finish_reason":"stop","text":"Today it is partially cloudy and raining. Today is a nice sunny day. I am your AI assistant, how can I help yo100   656    0   541  100   115  55561  11810 --:--:-- --:--:-- --:--:-- 72888ting@, #testing 1$ ,2%,3^, [4\u0026*5], 6~, 7-_ + (8 : 9) / \\ \u003c \u003e "}]}

$ kubectl exec -n standalone-envoy-default-20260427083347 curl -- curl -i \
  http://vllm-qwen3-32b-standalone-epp:8081/v1/completions \
  -H 'Content-Type: application/json' \
  -d '{"model":"Qwen/Qwen3-32B","prompt":"Write as if you were a critic: San Francisco","max_tokens":100,"temperature":0}'

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   447    0   332  100   115  37607  13026 --:--:-- --:--:-- --:--:-- 55875
HTTP/1.1 200 OK
x-went-into-resp-headers: true
content-type: application/json
x-inference-pod: vllm-qwen3-32b-5c8c976cdd-gwmlq
x-inference-port: 8000
server: fasthttp
date: Mon, 27 Apr 2026 16:04:52 GMT
transfer-encoding: chunked

{:"cmpl-f366ce46-d1da-5415-96ad-ba2372891ba5","created":1777305893,"model":"Qwen/Qwen3-32B","usage":{"prompt_tokens":10,"completion_tokens":10,"total_tokens":20},"object":"text_completion","kv_transfer_params":null,"choices":[{"index":0,"finish_reason":"stop","text":"The temperature here is twenty-five degrees centigrade. "}]}%

$ kubectl exec -n standalone-envoy-explicit-20260427083359 curl -- curl -i \
  http://vllm-qwen3-32b-standalone-epp:8081/v1/completions \
  -H 'Content-Type: application/json' \
  -d '{"model":"Qwen/Qwen3-32B","prompt":"Write as if you were a critic: San Francisco","max_tokens":100,"temperature":0}'

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   478    0   363  100   115  50627  16039 --:--:-- --:--:-- --:--:-- 68285
HTTP/1.1 200 OK
x-went-into-resp-headers: true
date: Mon, 27 Apr 2026 16:05:11 GMT
content-type: application/json
x-inference-pod: vllm-qwen3-32b-5c8c976cdd-l5ndd
x-inference-port: 8000
server: fasthttp
transfer-encoding: chunked

{:"cmpl-69a336ca-a617-5c73-875e-de2ff7d54a5a","created":1777305911,"model":"Qwen/Qwen3-32B","usage":{"prompt_tokens":10,"completion_tokens":23,"total_tokens":33},"object":"text_completion","kv_transfer_params":null,"choices":[{"index":0,"finish_reason":"stop","text":"Give a man a fish and you feed him for a day; teach a man to fish and you feed him for "}]}%
```
 |  
| --- |  
### 
commented [Apr 27, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#issuecomment-4328581972)
Copy link  Copy Markdown 
Contributor  
|  /lgtm  |  
| --- |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) assigned [ahg-g](https://github.com/ahg-g) [Apr 27, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#event-24913938253)
[k8s-ci-robot](https://github.com/k8s-ci-robot) added the [ lgtm ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Algtm) "Looks good to me", indicates that a PR is ready to be merged. label [Apr 27, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#event-24913939185)
Hide details View details
[k8s-ci-robot](https://github.com/k8s-ci-robot) merged commit [`dc90843`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/dc908434a7d5a4a151902130f00031580003b6f9) into  kubernetes-sigs:main [Apr 27, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#event-24914956958)
11 checks passed 
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882).
[danehans](https://github.com/danehans) mentioned this pull request [ Apr 27, 2026 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#ref-pullrequest-4338161864)
[ Tighten standalone guide wording #2889 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2889)
Closed 
### 
commented [Apr 28, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#issuecomment-4336278335) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882).
Copy link  Copy Markdown 
Contributor  
|  Can we revert this PR as it brought bugs, see `pull-gateway-api-inference-extension-verify-main` actually the yaml is not valid? cc [@danehans](https://github.com/danehans) [@ahg-g](https://github.com/ahg-g)  |  
| --- |  
[ahg-g](https://github.com/ahg-g) mentioned this pull request [ Apr 28, 2026 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#ref-pullrequest-4344084650)
[ Revert "Add standalone agentgateway proxy support" #2893 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2893)
Merged 
[k8s-ci-robot](https://github.com/k8s-ci-robot) pushed a commit that referenced this pull request [ Apr 28, 2026 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#ref-commit-a382da7)
` Revert "Add standalone agentgateway proxy support ([](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/a382da73911a06782fcafba992f368f977792eb6 "Revert "Add standalone agentgateway proxy support \(#2882\)" \(#2893\)

This reverts commit dc908434a7d5a4a151902130f00031580003b6f9.")#2882[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882))" ([](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/a382da73911a06782fcafba992f368f977792eb6 "Revert "Add standalone agentgateway proxy support \(#2882\)" \(#2893\)

This reverts commit dc908434a7d5a4a151902130f00031580003b6f9.")#2893[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2893))[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/a382da73911a06782fcafba992f368f977792eb6 "Revert "Add standalone agentgateway proxy support \(#2882\)" \(#2893\)

This reverts commit dc908434a7d5a4a151902130f00031580003b6f9.") `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882).

```
This reverts commit [`dc90843`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/dc908434a7d5a4a151902130f00031580003b6f9).
```

### 
commented [Apr 28, 2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#issuecomment-4337306035)
Copy link  Copy Markdown 
Contributor Author  
|  [@capri-xiyue](https://github.com/capri-xiyue) [@ahg-g](https://github.com/ahg-g) The Helm `Error:` output is expected due to [these](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882/changes#diff-2efabd233120e944241b5f94c5783bbcc5537bb73502d307a22c4c727aaa1bfd) negative test cases. The test fails only if Helm succeeds for an invalid config, e.g. `proxyType: bogus`. I also ran `hack/verify-helm.sh` locally and it reproduced the same validation `Error:` messages and exited successfully. This PR's [pull-gateway-api-inference-extension-verify-main](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882/changes#diff-2efabd233120e944241b5f94c5783bbcc5537bb73502d307a22c4c727aaa1bfd) check also shows SUCCESS with the same expected validation `Error:` messages. I have [agentgateway/website#434](https://github.com/agentgateway/website/pull/434) ready to merge based on this PR being merged. I will re-revert the PR ;-)  |  
| --- |  
This was referenced Apr 28, 2026
[ Harden InferencePool HTTPRoute gateway helper usage #2894 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2894)
[ Restore standalone Agentgateway proxy support #2895 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2895)
Merged 
[k8s-ci-robot](https://github.com/k8s-ci-robot) pushed a commit that referenced this pull request [ Apr 29, 2026 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882#ref-commit-a43641a)
` Revert "Revert "Add standalone agentgateway proxy support ([](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/a43641a27455a52971455261cff8ab864ad207a0 "Revert "Revert "Add standalone agentgateway proxy support \(#2882\)" \(#2893\)" \(#2895\)

This reverts commit a382da73911a06782fcafba992f368f977792eb6.

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>")#2882[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882))" ([](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/a43641a27455a52971455261cff8ab864ad207a0 "Revert "Revert "Add standalone agentgateway proxy support \(#2882\)" \(#2893\)" \(#2895\)

This reverts commit a382da73911a06782fcafba992f368f977792eb6.

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>")#…[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2893) `
Verified
#  Verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2882).

```
…2893[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2893))" (#2895[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2895))

This reverts commit [`a382da7`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/a382da73911a06782fcafba992f368f977792eb6).

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fkubernetes-sigs%2Fgateway-api-inference-extension%2Fpull%2F2882)
###  Labels 
[ approved ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Aapproved) Indicates a PR has been approved by an approver from all required OWNERS files. [ cncf-cla: yes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22cncf-cla%3A%20yes%22) Indicates the PR's author has signed the CNCF CLA. [ kind/documentation ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Akind%2Fdocumentation) Categorizes issue or PR as related to documentation. [ kind/feature ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. [ lgtm ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Algtm) "Looks good to me", indicates that a PR is ready to be merged. [ size/XXL ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FXXL) Denotes a PR that changes 1000+ lines, ignoring generated files.
###  4 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
