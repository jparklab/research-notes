---
title: 'Amend the endpoint picker protocol to support multiple fallback endpoints
  by wbpcode · Pull Request #761 · kubernetes-sigs/gateway-api-inference-extension
  · GitHub'
id: amend-the-endpoint-picker-protocol-to-support-multiple-fallback-endpoints-by-wbp
tags:
- llmd-router-nonk8s-backends-54b774
- epp-protocol
- destination-header
- fallback-endpoints
- standalone-epp
created: '2026-09-11T20:49:40.007623Z'
updated: '2026-09-12T03:37:37.701412Z'
source: https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761
source_domain: github.com
fetched_at: '2026-09-11T20:49:39.982412Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GIE PR #761 (wbpcode, opened Apr 30 2025, part of issue #414) amends the
  endpoint-picker-protocol to change the x-gateway-destination-endpoint header value
  format from a single ip:port to a LIST of endpoints, making the previous single-endpoint
  format a special case of the new list format. Author''s stated rationale, in direct
  response to reviewing the Envoy dynamic_forwarding LB policy work (envoyproxy/envoy#38757):
  the existing design only supported one fallback endpoint, and if EPP needs to communicate
  multiple fallback candidates, it is cleaner to send one ordered list in one header
  than to split primary/fallback across two headers or metadata entries -- ''from
  the proxy''s perspective, I will prefer only one source to get the external specified
  endpoints... the proxy could iterate the endpoints list in order and select the
  first valid one. If the retrying happens, the proxy could continue the iteration
  from the position where it stopped last time.'' This simplifies both EPP and proxy
  implementations by eliminating the need to handle multiple endpoint-source encodings.
  A key open design question raised by reviewer LiorLieberman: how does an ordered-endpoint-list
  header interact with proxy implementations that use Envoy subsets (cited: an internal
  Istio implementation doc of this same endpoint-picker API) -- i.e. subset-based
  routing and explicit destination-list routing are two different mechanisms for reaching
  a picked endpoint and may not compose cleanly. This PR is the header-protocol-level
  precursor to the Envoy dynamic_forwarding LB policy consuming that list.'
utility_score: 11.0
---

[Skip to content](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761).
/ **[gateway-api-inference-extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension) You must be signed in to change notification settings
  * [ Fork 313 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)
  * [ Star  766 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)


## Conversation
### 
commented [Apr 30, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issue-3031295825) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761).
Copy link  Copy Markdown 
Contributor
Part of [#414](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/414)
  1. This PR extend value format of `x-gateway-destination-endpoint` to support multiple endpoints.
  2. The new format could support mutliple fallback endpoints.
  3. **The new format is compatible with previous format becase the previous format could be treat a speical case of new format.**
  4. This will simplify the proxy implementation. The proxy needn't to handle different endpoint sources.
  5. This will simplify the EPP implementation because the EPP needn't to set different endpoint sources.
  6. It's possible to specify the fallback endpoints by HTTP header now.


### 
**[linux-foundation-easycla](https://github.com/apps/linux-foundation-easycla) Bot ** commented [Apr 30, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2841980791) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761).
Copy link  Copy Markdown   
|  The committers listed above are authorized under a signed CLA.
  * ✅ login: wbpcode / name: code ([`c62d3b3`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/c62d3b3026abef07cca4a91d38572020ffd17088), [`c204cf7`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/c204cf7ed3bd32760dc65dabdd3c14bca3784059), [`6a3c8f6`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/6a3c8f699c8d1633a2542a2cddbbb314744549f1), [`1047cb6`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/1047cb6c6f87e956c67ff3dd4cc1ae57ad3a97c5), [`f7d96a8`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/f7d96a858aab858f99bb76703fc6c795f7b092ec))

 |  
| --- |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) requested review from [ahg-g](https://github.com/ahg-g) and [danehans](https://github.com/danehans) [April 30, 2025 13:30](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#event-17473088912)
[k8s-ci-robot](https://github.com/k8s-ci-robot) added the [ cncf-cla: no ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22cncf-cla%3A%20no%22) Indicates the PR's author has not signed the CNCF CLA. label [Apr 30, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#event-17473089109)
### 
commented [Apr 30, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2841980995)
Copy link  Copy Markdown 
Contributor  
|  Welcome [@wbpcode](https://github.com/wbpcode)! It looks like this is your first PR to [kubernetes-sigs/gateway-api-inference-extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension) 🎉. Please refer to our [pull request process documentation](https://www.kubernetes.dev/docs/guide/pull-requests/) to help your PR have a smooth ride to approval. You will be prompted by a bot to use commands during the review process. Do not be afraid to follow the prompts! It is okay to experiment. [Here is the bot commands documentation](https://go.k8s.io/bot-commands). You can also check if kubernetes-sigs/gateway-api-inference-extension has [its own contribution guidelines](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/master/CONTRIBUTING.md). You may want to refer to our [testing guide](https://git.k8s.io/community/contributors/devel/sig-testing/testing.md) if you run into trouble with your tests not passing. If you are having difficulty getting your pull request seen, please follow the [recommended escalation practices](https://www.kubernetes.dev/docs/guide/pull-requests/#why-is-my-pull-request-not-getting-reviewed). Also, for tips and tricks in the contribution process you may want to read the [Kubernetes contributor cheat sheet](https://www.kubernetes.dev/docs/contributor-cheatsheet/). We want to make sure your contribution gets all the attention it needs! Thank you, and welcome to Kubernetes. 😃  |  
| --- |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) added the [ needs-ok-to-test ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Aneeds-ok-to-test) Indicates a PR that requires an org member to verify it is safe to test. label [Apr 30, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#event-17473089888)
### 
commented [Apr 30, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2841981051)
Copy link  Copy Markdown 
Contributor  
|  Hi [@wbpcode](https://github.com/wbpcode). Thanks for your PR. I'm waiting for a [kubernetes-sigs](https://github.com/orgs/kubernetes-sigs/people) member to verify that this patch is reasonable to test. If it is, they should reply with `/ok-to-test` on its own line. Until that is done, I will not automatically test new commits in this PR, but the usual testing commands by org members will still work. Regular contributors should [join the org](https://git.k8s.io/community/community-membership.md#member) to skip this step. Once the patch is verified, the new status will be reflected by the `ok-to-test` label. I understand the commands that are listed [here](https://go.k8s.io/bot-commands?repo=kubernetes-sigs%2Fgateway-api-inference-extension). Details Instructions for interacting with me using PR comments are available [here](https://git.k8s.io/community/contributors/guide/pull-requests.md). If you have questions or suggestions related to my behavior, please file an issue against the [kubernetes-sigs/prow](https://github.com/kubernetes-sigs/prow/issues/new?title=Prow%20issue:) repository.  |  
| --- |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) added the [ size/S ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FS) Denotes a PR that changes 10-29 lines, ignoring generated files. label [Apr 30, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#event-17473091839)
### 
commented [Apr 30, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2841986053) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761).
Copy link  Copy Markdown   
| 
###  ✅ Deploy Preview for _gateway-api-inference-extension_ ready!  
 | Name  | Link  |  
| --- | --- |  
|  🔨 Latest commit  |  
|  🔍 Latest deploy log  | <https://app.netlify.com/sites/gateway-api-inference-extension/deploys/6822c0369eba680009f39f78>  |  
|  😎 Deploy Preview  | <https://deploy-preview-761--gateway-api-inference-extension.netlify.app>  |  
|  📱 Preview on mobile  |  Toggle QR Code... _Use your smartphone camera to open QR code link._  |  
_To edit notification comments on pull requests, go to your[Netlify site configuration](https://app.netlify.com/sites/gateway-api-inference-extension/configuration/notifications#deploy-webhooks)._
 |  
` Amend the endpoint picker protocol to support multiple fallback endpo…[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/commits/c62d3b3026abef07cca4a91d38572020ffd17088 "Amend the endpoint picker protocol to support multiple fallback endpoints

Signed-off-by: wangbaiping\(wbpcode\) <wbphub@gmail.com>") `

```
…ints

Signed-off-by: wangbaiping(wbpcode) <wbphub@gmail.com>
```

[wbpcode](https://github.com/wbpcode) [force-pushed](https://github.com/kubernetes-sigs/gateway-api-inference-extension/compare/77fbe07bd035f402455b33fc76901c485d4f85fd..c62d3b3026abef07cca4a91d38572020ffd17088) the  dev-multiple-fallback-support branch from [`77fbe07`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/77fbe07bd035f402455b33fc76901c485d4f85fd) to [`c62d3b3`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/c62d3b3026abef07cca4a91d38572020ffd17088) [ Compare ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/compare/77fbe07bd035f402455b33fc76901c485d4f85fd..c62d3b3026abef07cca4a91d38572020ffd17088) [April 30, 2025 13:34](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#event-17473160081)
[k8s-ci-robot](https://github.com/k8s-ci-robot) added [ cncf-cla: yes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22cncf-cla%3A%20yes%22) Indicates the PR's author has signed the CNCF CLA. and removed [ cncf-cla: no ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22cncf-cla%3A%20no%22) Indicates the PR's author has not signed the CNCF CLA. labels [Apr 30, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#event-17473251712)
### 
commented [Apr 30, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2842047198) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761).
Copy link  Copy Markdown 
Contributor Author  
|  When I am reviewing the related Envoy implementation of this EPP ([envoyproxy/envoy#38757](https://github.com/envoyproxy/envoy/pull/38757)), I found the current design only support single fallback endpoint. That inspire me to create this PR. And if we want to support multiple fallback endpoints, the EPP **must** send a list of addresses to proxy. If there must be a list of addresses, i think it would be better to make the x-gateway-destination-endpoint a list directly. Then we needn't break the list into different two headers or metadata entries. And from the proxy's pespective, I will prefer only one source to get the external specfied endpoints. And the proxy could iterate the endpoints list in order and select the first valid one. If the retrying happens, the proxy could continue the iteration from the position where it stop last time. cc [@yanavlasov](https://github.com/yanavlasov)  |  
| --- |  
[wbpcode](https://github.com/wbpcode) mentioned this pull request [ Apr 30, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#ref-pullrequest-2921576397)
[ Dynamic forwarding load balancing policy envoyproxy/envoy#38757 ](https://github.com/envoyproxy/envoy/pull/38757)
Merged 
### 
commented [Apr 30, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2843224311)
Copy link  Copy Markdown 
Contributor  
|  /assign [@LiorLieberman](https://github.com/LiorLieberman) for review /hold  |  
| --- |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) added the [ do-not-merge/hold ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Ado-not-merge%2Fhold) Indicates that a PR should not merge because someone has issued a /hold command. label [Apr 30, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#event-17480002852)
### 
commented [Apr 30, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2843224757)
Copy link  Copy Markdown 
Contributor  
|  /ok-to-test  |  
| --- |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) added [ ok-to-test ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Aok-to-test) Indicates a non-member PR verified by an org member that is safe to test. and removed [ needs-ok-to-test ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Aneeds-ok-to-test) Indicates a PR that requires an org member to verify it is safe to test. labels [Apr 30, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#event-17480005130)
### 
commented [May 1, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2843810234)
Copy link  Copy Markdown 
Member
[k8s-ci-robot](https://github.com/k8s-ci-robot) requested a review from [LiorLieberman](https://github.com/LiorLieberman) [May 1, 2025 00:42](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#event-17482320967)
reviewed  [ May 1, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#pullrequestreview-2808899472)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/c62d3b3026abef07cca4a91d38572020ffd17088)
### 
left a comment 
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Thanks [@wbpcode](https://github.com/wbpcode)!
Question - how would it work with an implementation that leverage envoy subsets?
For ref - see the [istio implementation](https://docs.google.com/document/d/1-x7vv_SnN8XCziIBo33aiOzwvqWDeilMWLP-kxK25co/edit?userstoinvite=lin.sun%40solo.io&sharingaction=manageaccess&role=writer&resourcekey=0-7xtao0uAAABuYeyYi4L-Xg&tab=t.0) of this API
### 
commented [May 1, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2843877677) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761).
Copy link  Copy Markdown 
Contributor Author  
| 
> Thanks [@wbpcode](https://github.com/wbpcode)! Question - how would it work with an implementation that leverage envoy subsets? For ref - see the [istio implementation](https://docs.google.com/document/d/1-x7vv_SnN8XCziIBo33aiOzwvqWDeilMWLP-kxK25co/edit?userstoinvite=lin.sun%40solo.io&sharingaction=manageaccess&role=writer&resourcekey=0-7xtao0uAAABuYeyYi4L-Xg&tab=t.0) of this API
Sorry，I am not allowed to access the doc. But I think it is impossible for subset lb based solution to support multiple fallback endpoints. That's say, if the istio's implementation is used, then the EPP couldn't support multiple fallback endpoints anyway and the legacy format should be used. And I believe the subset lb based solution couldn't support retrying also. (The problem is similar with the original dst based solution.) I will also suggest the istio to update the implementation to use the new override host lb. From the reference document, seems it's an experimental implementation and hasn't implemented the fallback mechanism, so I think it's possible to take the retrying and multiple fallback endpoints into account at this timepoint.  |  
| --- |  
### 
commented [May 1, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2843890335) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761).
Copy link  Copy Markdown 
Contributor Author  
|  I think another alternative is to define a new key like `x-gateway-destination-endpoints` for both primary host and multiple fallback endpoints. But anyway, the EPP may need additional switch to enable fallback endpoints or not, it is possible to keep compatibility with the implementation detail of istio. The istio haven't release this feature and I think it's possible to switch subset lb to the override host lb in the future.  |  
| --- |  
reviewed  [ May 1, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#pullrequestreview-2810166328)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/c62d3b3026abef07cca4a91d38572020ffd17088)
Comment thread [docs/proposals/004-endpoint-picker-protocol/README.md](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/c62d3b3026abef07cca4a91d38572020ffd17088#diff-38b9b0ed5954971e020ad315834cbe40b209a6d255e4ec69aa077f0659030fb7) Outdated   
|  Single fallback endpoint also CAN be set using the key `x-gateway-destination-endpoint-fallback` in the same metadata namespace as one used for `x-gateway-destination-endpoint` as follows:  |  
| --- |  
### 
[May 1, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#discussion_r2070486322)
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Should the EPP still support `x-gateway-destination-endpoint-fallback` if `x-gateway-destination-endpoint` now supports endpoint fallback by listing multiple addresses? cc: [@ahg-g](https://github.com/ahg-g) [@kfswain](https://github.com/kfswain)
### 
[May 2, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#discussion_r2071208778) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761).
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Is the API break change acceptable or is there a way to deprecate the `x-gateway-destination-endpoint-fallback`? Ideally, the EPP should has a flag to enable the new format or use the legacy format. And then we can deprecate the legacy format gradually.
Comment thread [docs/proposals/004-endpoint-picker-protocol/README.md](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/c62d3b3026abef07cca4a91d38572020ffd17088#diff-38b9b0ed5954971e020ad315834cbe40b209a6d255e4ec69aa077f0659030fb7) Outdated   
|  ### Destination endpoint fallback  |  
| --- |  
|  A single fallback endpoint CAN be set using the key `x-gateway-destination-endpoint-fallback` in the same metadata namespace as one used for `x-gateway-destination-endpoint` as follows:  |  
|  For each HTTP request, if destination endpoint fallback is necessary or possible, the EPP CAN set the `x-gateway-destination-endpoint` HTTP header or metadata entry with multiple addresses in `<ip:port>,<ip:port>,...` format. Multiple addresses are separated by commas. The first valid endpoint in the addresses list will be used as the primary endpoint. And if retrying is happening, the proxy will try the endpoints after the selected endpoint in order.  |  
### 
[May 1, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#discussion_r2070497565)
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Should we continue to have this subsection or update the `## Destination Endpoint` section to state that one address MUST be set or multiple addresses CAN be set for fallback purposes? Thoughts [@ahg-g](https://github.com/ahg-g) [@kfswain](https://github.com/kfswain)?
### 
[May 2, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#discussion_r2071213718)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
SGTM.
### 
[May 9, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#discussion_r2080852095)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I have updated content of the `## Destination Endpoint` section.

```
The value of the header or metadata entry MUST contains at least one endpoint in `<ip:port>` format or multiple endpoints in `<ip:port>,<ip:port>,...` format. Multiple endpoints are separated by commas. The first valid endpoint in the value will be used. And if retrying is happening, the proxy will try the endpoints after the previously selected endpoint in order.

```

The `x-gateway-destination-endpoint-fallback` is still be kept for backward compatibility.
reviewed  [ May 1, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#pullrequestreview-2810385240)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/c62d3b3026abef07cca4a91d38572020ffd17088)
Comment thread [docs/proposals/004-endpoint-picker-protocol/README.md](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/c62d3b3026abef07cca4a91d38572020ffd17088#diff-38b9b0ed5954971e020ad315834cbe40b209a6d255e4ec69aa077f0659030fb7) Outdated   
|  ### Destination endpoint fallback  |  
| --- |  
|  A single fallback endpoint CAN be set using the key `x-gateway-destination-endpoint-fallback` in the same metadata namespace as one used for `x-gateway-destination-endpoint` as follows:  |  
|  For each HTTP request, if destination endpoint fallback is necessary or possible, the EPP CAN set the `x-gateway-destination-endpoint` HTTP header or metadata entry with multiple addresses in `<ip:port>,<ip:port>,...` format. Multiple addresses are separated by commas. The first valid endpoint in the addresses list will be used as the primary endpoint. And if retrying is happening, the proxy will try the endpoints after the selected endpoint in order.  |  
### 
[May 1, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#discussion_r2070611913)
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Note that this is a breaking change that will impact the Istio implementation at least, which is based on Envoy LB subsetting and requires a single value to be specified.
Adding [@LiorLieberman](https://github.com/LiorLieberman) for feedback.
### 
[May 2, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#discussion_r2071212154)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
From API's perspective, this is not a break change. But if take the implementation detail into account, this actually may break the istio implementation. But ideally, we should have a flag to enable the new format or use legacy format in the EPP.
### 
[May 3, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#discussion_r2072484082)
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
left a comment below
[danehans](https://github.com/danehans) mentioned this pull request [ May 1, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#ref-issue-2751302693)
[ Add Initial Gateway API Inference Extension Support kgateway-dev/kgateway#10411 ](https://github.com/kgateway-dev/kgateway/issues/10411)
Closed 
25 tasks
reviewed  [ May 3, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#pullrequestreview-2813320891)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/c62d3b3026abef07cca4a91d38572020ffd17088)
### 
left a comment 
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
> The istio haven't release this feature and I think it's possible to switch subset lb to the override host lb in the future.
Its definitely possible to switch. I assume you are talking about [envoyproxy/envoy#38757](https://github.com/envoyproxy/envoy/pull/38757) ?
Will need to to catch up with this PR its been a month or two since I last look at it.
This PR is not even merged yet though - whats the timeline to get it a non experimental API in envoy?
### 
commented [May 6, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2853077508)
Copy link  Copy Markdown 
Contributor Author  
| 
> > The istio haven't release this feature and I think it's possible to switch subset lb to the override host lb in the future.
> Its definitely possible to switch. I assume you are talking about [envoyproxy/envoy#38757](https://github.com/envoyproxy/envoy/pull/38757) ? Will need to to catch up with this PR its been a month or two since I last look at it. This PR is not even merged yet though - whats the timeline to get it a non experimental API in envoy?
We hope the [envoyproxy/envoy#38757](https://github.com/envoyproxy/envoy/pull/38757) could be used to support EPP with multiple fallback endpionts and retrying support. If we can get agreement with the multiple addresses header/metadata format (`<ip>:<port>,<ip>:<port>,...`, no matter which key is used), I think it will be pretty quick for Envoy to merge that (may be only one or two weeks, I will discuss that with [@yanavlasov](https://github.com/yanavlasov))  |  
| --- |  
reviewed  [ May 9, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#pullrequestreview-2829324548)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/f7d96a858aab858f99bb76703fc6c795f7b092ec)
### 
left a comment  •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761).
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I am going to take this proposal with other istio folks, hopefully we can converge on override_host_lb, but would like to get the feedback first.
[@wbpcode](https://github.com/wbpcode) To clarify - what is the benefit of supporting multiple keys in `x-gateway-destination-endpoint` as opposed to `x-gateway-destination-endpoint-fallback` or `-fallback*s*`?
>   1. This will simplify the proxy implementation. The proxy needn't to handle different endpoint sources.
>   2. This will simplify the EPP implementation because the EPP needn't to set different endpoint sources.
>   3. It's possible to specify the fallback endpoints by HTTP header now.
> 

I dont see any major simplification by moving the fallbacks to x-gateway-destination-endpoint. I would in fact prefer to leave `x-gateway-destination-endpoint` a single value now, and in the future maybe evolve it to support some key:value map or list formatted values. For example one random idea in mind, `x-gateway-destination-endpoint: zone-a:<ip:port>,zone-b:<ip-port>` to support some version of topology. Is this too crazy?
### 
commented [May 10, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2868636627)
Copy link  Copy Markdown 
Contributor Author  
| 
> To clarify - what is the benefit of supporting multiple keys in x-gateway-destination-endpoint as opposed to x-gateway-destination-endpoint-fallback or -fallback _s_
**The key benefit is the multiple fallback endpoints support. In the current proposal, only one primary endpoint and an optional fallback endpoint could be selected.** With the new format, the EPP server could select a list of endpoints and the proxy could try them one by one. And from the perspective of the API, this is a compatible change.
> I dont see any major simplification by moving the fallbacks to x-gateway-destination-endpoint.
At the initial design of the override host lb, we designed a little complex API to support fallback endpoints. This is why I think moving the fallbacks to x-gateway-destination-endpoint could simplify the proxy implementation. But finally we resolved it in the new design. But anyway, I still think single/clear/explicit endpoints source would be helpful for both proxy and EPP server.
> I would in fact prefer to leave x-gateway-destination-endpoint a single value now, and in the future maybe evolve it to support some key:value map or list formatted values. For example one random idea in mind, x-gateway-destination-endpoint: zone-a:ip:port,zone-b: to support some version of topology. Is this too crazy?
If there is reasonable requirement, I think it's fine to support more complex format in the future. But considering the fact the `<ip:port>` basically is the most fine-grained unit, I didn't get why the zone make sense here?  |  
| --- |  
reviewed  [ May 11, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#pullrequestreview-2831518690)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/f7d96a858aab858f99bb76703fc6c795f7b092ec)
### 
left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
why do we need 2 lists of endpoints? I would expect either a single endpoint + fallback OR a list of endpoints, but not 2 lists?
reviewed  [ May 12, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#pullrequestreview-2833669528)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/f7d96a858aab858f99bb76703fc6c795f7b092ec)
Comment thread [docs/proposals/004-endpoint-picker-protocol/README.md](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/f7d96a858aab858f99bb76703fc6c795f7b092ec#diff-38b9b0ed5954971e020ad315834cbe40b209a6d255e4ec69aa077f0659030fb7) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761).
reviewed  [ May 12, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#pullrequestreview-2833677304)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/f7d96a858aab858f99bb76703fc6c795f7b092ec)
Comment thread [docs/proposals/004-endpoint-picker-protocol/README.md](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/f7d96a858aab858f99bb76703fc6c795f7b092ec#diff-38b9b0ed5954971e020ad315834cbe40b209a6d255e4ec69aa077f0659030fb7) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761).
reviewed  [ May 12, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#pullrequestreview-2833681075)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/f7d96a858aab858f99bb76703fc6c795f7b092ec)
Comment thread [docs/proposals/004-endpoint-picker-protocol/README.md](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/f7d96a858aab858f99bb76703fc6c795f7b092ec#diff-38b9b0ed5954971e020ad315834cbe40b209a6d255e4ec69aa077f0659030fb7) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761).
reviewed  [ May 12, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#pullrequestreview-2833714039)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/f7d96a858aab858f99bb76703fc6c795f7b092ec)
Comment thread [docs/proposals/004-endpoint-picker-protocol/README.md](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/f7d96a858aab858f99bb76703fc6c795f7b092ec#diff-38b9b0ed5954971e020ad315834cbe40b209a6d255e4ec69aa077f0659030fb7) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761).
reviewed  [ May 12, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#pullrequestreview-2833719529)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/f7d96a858aab858f99bb76703fc6c795f7b092ec)
Comment thread [docs/proposals/004-endpoint-picker-protocol/README.md](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/f7d96a858aab858f99bb76703fc6c795f7b092ec#diff-38b9b0ed5954971e020ad315834cbe40b209a6d255e4ec69aa077f0659030fb7) Outdated   
|  ### Why envoy.lb namespace as a default?   |  
| --- |  
|  The endpoints specified in `x-gateway-destination-endpoint-fallback` MAY be tried after the endpoints specified in `x-gateway-destination-endpoint` if all the endpoints specified in `x-gateway-destination-endpoint` are unavailable.  |  
### 
[May 12, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#discussion_r2084977542)
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
When `x-gateway-destination-endpoint` defines more than one `IP:Port`, the additional entries act as a fallback, correct? If so, why is `x-gateway-destination-endpoint-fallback` still needed?
### 
[May 13, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#discussion_r2085868662)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
it's unnecessary, but not sure if it's ok or not to remove it directly from the proposal. I will removed it first.
### 
commented [May 13, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2874963443) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761).
Copy link  Copy Markdown 
Contributor Author  
| 
> why do we need 2 lists of endpoints? I would expect either a single endpoint + fallback OR a list of endpoints, but not 2 lists?
Yeah. I think we all prefer the only one list of endpoints. In the format, Only the `x-gateway-destination-endpoint` could be a list of endpoints and the `x-gateway-destination-endpoint-fallback` is only be kept for API backward compatibility. I have removed the `x-gateway-destination-endpoint-fallback` according to comments from [@danehans](https://github.com/danehans) [@howardjohn](https://github.com/howardjohn)  |  
| --- |  
[wbpcode](https://github.com/wbpcode) added 2 commits [May 13, 2025 11:42](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#commits-pushed-c204cf7)

```
Signed-off-by: wangbaiping(wbpcode) <wbphub@gmail.com>
```


```
Signed-off-by: wangbaiping(wbpcode) <wbphub@gmail.com>
```

### 
commented [May 13, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2877741257)
Copy link  Copy Markdown 
Contributor  
| 
> Only the x-gateway-destination-endpoint could be a list of endpoints and the x-gateway-destination-endpoint-fallback is only be kept for API backward compatibility.
[@LiorLieberman](https://github.com/LiorLieberman), can you confirm whether `x-gateway-destination-endpoint-fallback` is still required by the Istio implementation? If so, [@wbpcode](https://github.com/wbpcode), we can mark it as deprecated and remove it in a future release.  |  
| --- |  
approved these changes  [ May 13, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#pullrequestreview-2838292971)
[ View reviewed changes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761/files/6a3c8f699c8d1633a2542a2cddbbb314744549f1)
### 
left a comment 
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
> If there is reasonable requirement, I think it's fine to support more complex format in the future. But considering the fact the ip:port basically is the most fine-grained unit, I didn't get why the zone make sense here?
Theoretically the API can evolve to support something like "this is the best endpoint in this zone" (or any other topology key). Thats what I was referring to.
> [@LiorLieberman](https://github.com/LiorLieberman), can you confirm whether x-gateway-destination-endpoint-fallback is still required by the Istio implementation? If so, [@wbpcode](https://github.com/wbpcode), we can mark it as deprecated and remove it in a future release.
Istio does not support this key currently, so it is safe from this perspective. But would be good to check with all implementations of this API, last time we had a breaking change in the EPP istio release could not function.
If you all in agreement that one list is better than two, I am fine with that. I thought a different key is slightly cleaner and leaves more room for expansion but this opinion is not very strong.
### 
commented [May 14, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2880901332)
Copy link  Copy Markdown 
Contributor  
|  [@ahg-g](https://github.com/ahg-g), since you applied the hold, PTAL now that [@LiorLieberman](https://github.com/LiorLieberman) provided feedback. cc: [@arkodg](https://github.com/arkodg) and [@kfswain](https://github.com/kfswain) to confirm whether Envoy Gateway uses `x-gateway-destination-endpoint-fallback`.  |  
| --- |  
### 
commented [May 19, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2891148119)
Copy link  Copy Markdown 
Contributor Author  
|  friendly ping [@ahg-g](https://github.com/ahg-g)  |  
| --- |  
### 
commented [May 30, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2923387913)
Copy link  Copy Markdown 
Contributor  
|  [@AndresGuedez](https://github.com/AndresGuedez) the proposal is to completely remove the explict fallback list (x-gateway-destination-endpoint-fallback), is this a breaking change to GCP LB?  |  
| --- |  
### 
commented [May 31, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2925314763)
Copy link  Copy Markdown 
Contributor  
| 
> [@AndresGuedez](https://github.com/AndresGuedez) the proposal is to completely remove the explict fallback list (x-gateway-destination-endpoint-fallback), is this a breaking change to GCP LB?
GCP LBs already support the endpoint-fallback metadata, so while not necessarily breaking goodput behavior, the fallback path won't work as expected if the EPP extension was already providing the -fallback metadata. The bigger concern is breaking existing "x-gateway-destination-endpoint" implementations that validate for a single value. The GCP LB logic is defensive and won't break if the metadata has multiple values, but other implementations may have stricter validation.  |  
| --- |  
### 
commented [Jun 1, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2926052148) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761).
Copy link  Copy Markdown 
Contributor Author  
| 
> > [@AndresGuedez](https://github.com/AndresGuedez) the proposal is to completely remove the explict fallback list (x-gateway-destination-endpoint-fallback), is this a breaking change to GCP LB?
> GCP LBs already support the endpoint-fallback metadata, so while not necessarily breaking goodput behavior, the fallback path won't work as expected if the EPP extension was already providing the -fallback metadata. The bigger concern is breaking existing "x-gateway-destination-endpoint" implementations that validate for a single value. The GCP LB logic is defensive and won't break if the metadata has multiple values, but other implementations may have stricter validation.
Then I think we could keep the `x-gateway-destination-endpoint-fallback` and `x-gateway-destination-endpoint` for backward compatibility. And only add new `x-gateway-destination-endpoints` to support multiple addresses fallback if necessary like my suggesstion in the [#761 (comment)](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2843890335) WDYT? cc [@AndresGuedez](https://github.com/AndresGuedez) [@ahg-g](https://github.com/ahg-g)  |  
| --- |  
### 
commented [Jun 2, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2931291725)
Copy link  Copy Markdown 
Contributor  
|  I am fine with removing the old approach and stick with only one list. /approve /lgtm  |  
| --- |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) assigned [ahg-g](https://github.com/ahg-g) [Jun 2, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#event-17937236202)
[k8s-ci-robot](https://github.com/k8s-ci-robot) added the [ lgtm ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Algtm) "Looks good to me", indicates that a PR is ready to be merged. label [Jun 2, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#event-17937236546)
### 
commented [Jun 2, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2931292120)
Copy link  Copy Markdown 
Contributor  
|  [APPROVALNOTIFIER] This PR is **APPROVED** This pull-request has been approved by: , ,  The full list of commands accepted by this bot can be found [here](https://go.k8s.io/bot-commands?repo=kubernetes-sigs%2Fgateway-api-inference-extension). The pull request process is described [here](https://git.k8s.io/community/contributors/guide/owners.md#the-code-review-process) Details Needs approval from an approver in each of these files: 
  * [ahg-g]

Approvers can indicate their approval by writing `/approve` in a comment Approvers can cancel approval by writing `/approve cancel` in a comment  |  
| --- |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) added the [ approved ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Aapproved) Indicates a PR has been approved by an approver from all required OWNERS files. label [Jun 2, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#event-17937237308)
### 
commented [Jun 2, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#issuecomment-2931385126)
Copy link  Copy Markdown 
Contributor  
|  /hold cancel  |  
| --- |  
[k8s-ci-robot](https://github.com/k8s-ci-robot) removed the [ do-not-merge/hold ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Ado-not-merge%2Fhold) Indicates that a PR should not merge because someone has issued a /hold command. label [Jun 2, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#event-17937702590)
[k8s-ci-robot](https://github.com/k8s-ci-robot) merged commit [`fdea49d`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/fdea49d87eac966798c381aec0e5a8e665414891) into  kubernetes-sigs:main [Jun 2, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#event-17938036943)
[nirrozenbaum](https://github.com/nirrozenbaum) mentioned this pull request [ Jun 2, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#ref-pullrequest-3106032851)
[ scheduler proposal continuation #905 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/905)
Merged 
[shmuelk](https://github.com/shmuelk) pushed a commit to shmuelk/gateway-api-inference-extension that referenced this pull request [ Jun 3, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#ref-commit-ae6b21a)
` Amend the endpoint picker protocol to support multiple fallback endpo…[](https://github.com/shmuelk/gateway-api-inference-extension/commit/ae6b21a9e7b3c4d0ebb53c1ae87de8710c3326d2 "Amend the endpoint picker protocol to support multiple fallback endpoints \(#761\)

* Amend the endpoint picker protocol to support multiple fallback endpoints

Signed-off-by: wangbaiping\(wbpcode\) <wbphub@gmail.com>

* address comments

Signed-off-by: wangbaiping\(wbpcode\) <wbphub@gmail.com>

* add optionally

Signed-off-by: wangbaiping\(wbpcode\) <wbphub@gmail.com>

* address comments

Signed-off-by: wangbaiping\(wbpcode\) <wbphub@gmail.com>

* address comments

Signed-off-by: wangbaiping\(wbpcode\) <wbphub@gmail.com>

---------

Signed-off-by: wangbaiping\(wbpcode\) <wbphub@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[shmuelk](https://github.com/shmuelk) Shmuel Kallner 
SSH Key Fingerprint: voJRiE8uApGB5SzvZ/zVxeOxhV3io8tRdDbwZNQvH6Y
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
…ints (kubernetes-sigs#761[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761))

* Amend the endpoint picker protocol to support multiple fallback endpoints

Signed-off-by: wangbaiping(wbpcode) <wbphub@gmail.com>

* address comments

Signed-off-by: wangbaiping(wbpcode) <wbphub@gmail.com>

* add optionally

Signed-off-by: wangbaiping(wbpcode) <wbphub@gmail.com>

* address comments

Signed-off-by: wangbaiping(wbpcode) <wbphub@gmail.com>

* address comments

Signed-off-by: wangbaiping(wbpcode) <wbphub@gmail.com>

---------

Signed-off-by: wangbaiping(wbpcode) <wbphub@gmail.com>
```

[rlakhtakia](https://github.com/rlakhtakia) pushed a commit to rlakhtakia/gateway-api-inference-extension that referenced this pull request [ Jun 11, 2025 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#ref-commit-aa7fb91)
` Amend the endpoint picker protocol to support multiple fallback endpo…[](https://github.com/rlakhtakia/gateway-api-inference-extension/commit/aa7fb9175871ff0a58eec4068792c9e2630c8547 "Amend the endpoint picker protocol to support multiple fallback endpoints \(#761\)

* Amend the endpoint picker protocol to support multiple fallback endpoints

Signed-off-by: wangbaiping\(wbpcode\) <wbphub@gmail.com>

* address comments

Signed-off-by: wangbaiping\(wbpcode\) <wbphub@gmail.com>

* add optionally

Signed-off-by: wangbaiping\(wbpcode\) <wbphub@gmail.com>

* address comments

Signed-off-by: wangbaiping\(wbpcode\) <wbphub@gmail.com>

* address comments

Signed-off-by: wangbaiping\(wbpcode\) <wbphub@gmail.com>

---------

Signed-off-by: wangbaiping\(wbpcode\) <wbphub@gmail.com>") `

```
…ints (kubernetes-sigs#761[](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761))

* Amend the endpoint picker protocol to support multiple fallback endpoints

Signed-off-by: wangbaiping(wbpcode) <wbphub@gmail.com>

* address comments

Signed-off-by: wangbaiping(wbpcode) <wbphub@gmail.com>

* add optionally

Signed-off-by: wangbaiping(wbpcode) <wbphub@gmail.com>

* address comments

Signed-off-by: wangbaiping(wbpcode) <wbphub@gmail.com>

* address comments

Signed-off-by: wangbaiping(wbpcode) <wbphub@gmail.com>

---------

Signed-off-by: wangbaiping(wbpcode) <wbphub@gmail.com>
```

[danehans](https://github.com/danehans) mentioned this pull request [ Jul 22, 2026 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761#ref-issue-4456155105)
[ Clarify EPP endpoint fallback semantics with route-level retries #2936 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2936)
Closed 
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fkubernetes-sigs%2Fgateway-api-inference-extension%2Fpull%2F761)
###  Labels 
[ approved ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Aapproved) Indicates a PR has been approved by an approver from all required OWNERS files. [ cncf-cla: yes ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22cncf-cla%3A%20yes%22) Indicates the PR's author has signed the CNCF CLA. [ lgtm ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Algtm) "Looks good to me", indicates that a PR is ready to be merged. [ ok-to-test ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Aok-to-test) Indicates a non-member PR verified by an org member that is safe to test. [ size/M ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3Asize%2FM) Denotes a PR that changes 30-99 lines, ignoring generated files.
###  7 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
