---
title: 'Dynamic forwarding load balancing policy by yanavlasov · Pull Request #38757
  · envoyproxy/envoy · GitHub'
id: dynamic-forwarding-load-balancing-policy-by-yanavlasov-pull-request-38757-envoyp
tags:
- llmd-router-nonk8s-backends-54b774
- override-host
- envoy-lb-policy
- epp-protocol
- standalone-epp
created: '2026-09-11T20:49:32.207056Z'
updated: '2026-09-12T03:37:37.697751Z'
source: https://github.com/envoyproxy/envoy/pull/38757
source_domain: github.com
fetched_at: '2026-09-11T20:49:32.183057Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Envoy PR #38757 (yanavlasov/Google, opened Mar 15 2025, merged) adds a new
  ''dynamic_forwarding'' load-balancing-policy extension that lets an external endpoint
  picker (ext_proc filter) set PRIMARY and FALLBACK/retry candidate endpoints via
  request header or dynamic metadata, consumed at LB-selection time via chooseHost(),
  with a configurable fallback LB policy (e.g. round robin) used only if the ext_proc-set
  header/metadata is absent or fails to parse. This directly targets the InferencePool/EPP
  endpoint-picker-protocol use case (author links docs/proposals/004-endpoint-picker-protocol).
  The PR body contains the exact architectural debate the router-customization question
  hinges on: reviewer wbpcode initially argued the existing setUpstreamOverrideHost()
  API (single-host override, honored by all LB policies unless the host fails health
  checks) already does what''s needed and a whole new LB policy was unnecessary complexity.
  Author yanavlasov countered that override-host + an HTTP filter cannot satisfy BOTH
  requirements simultaneously: (1) ext_proc-driven selection of primary AND retry/fallback
  hosts (avoiding ''stale retry endpoints'' when only one override host is set) and
  (2) the longer-term goal of a full ''ext_lb'' remote-callout protocol for asynchronous
  external load balancing. A second reviewer (posting independently, having built
  similar functionality) reiterated the override-host critique nearly verbatim: ''I
  didn''t find anything setUpstreamOverrideHost cannot do (except the retry)... the
  story could [be] much simpler, the ext proc filter could set these endpoint candidates
  by the setUpstreamOverrideHost. This only enhancement necessarily is making setUpstreamOverrideHost
  could accept multiple addresses to support retry.'' The fallback-endpoint header
  is later identified in the thread as x-gateway-destination-endpoint-fallback, distinct
  from the primary x-gateway-destination-endpoint header. CORRECTED FOLLOW-UP (per
  kgateway-dev/kgateway#11824, fetched separately): kgateway initially worked around
  the absence of override_host support with a ''static cluster + subset LB'' hack
  to pass the EppUnAvailableFailOpen conformance test, and explicitly planned to refactor
  to override_host LB once its Envoy pin reached v1.35 (which ships this PR''s feature)
  -- but that refactor issue was closed in Sep 2025 not because kgateway adopted override_host,
  but because ''the community decided to deprecate the Envoy-based data plane in favor
  of agentgateway for AI-related features.'' So the override_host-vs-dynamic_forwarding
  debate was ultimately superseded, in at least one major GIE-integrated gateway,
  by abandoning Envoy-as-data-plane for AI/inference routing altogether in favor of
  agentgateway. This is a load-bearing fact for router customization strategy: it
  suggests that for llm-d + GIE, the durable extension point for non-pod-destination
  routing may increasingly be agentgateway''s own destinationMode/passthrough config
  (PR #1658/#1429) rather than either of these two Envoy LB mechanisms.'
utility_score: 13.0
---

[Skip to content](https://github.com/envoyproxy/envoy/pull/38757#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/envoyproxy/envoy/pull/38757) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/envoyproxy/envoy/pull/38757) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/envoyproxy/envoy/pull/38757) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fenvoyproxy%2Fenvoy) You must be signed in to change notification settings
  * [ Fork 5.6k ](https://github.com/login?return_to=%2Fenvoyproxy%2Fenvoy)
  * [ Star  28.9k ](https://github.com/login?return_to=%2Fenvoyproxy%2Fenvoy)


## Conversation
### 
commented [Mar 15, 2025](https://github.com/envoyproxy/envoy/pull/38757#issue-2921576397)
Copy link  Copy Markdown 
Contributor
Commit Message: Additional Description:
Risk Level: Low, new extension Testing: Unit Tests Docs Changes: Yes Release Notes: Yes Platform Specific Features: N/A
` Dynamic forwarding loadbalancing policy[](https://github.com/envoyproxy/envoy/pull/38757/commits/5d2ac40e861e4220dfde0c8673f37f9ff6129bd0 "Dynamic forwarding loadbalancing policy

Signed-off-by: Yan Avlasov <yavlasov@google.com>") `

```
Signed-off-by: Yan Avlasov <yavlasov@google.com>
```

### 
**[repokitteh-read-only](https://github.com/apps/repokitteh-read-only) Bot ** commented [Mar 15, 2025](https://github.com/envoyproxy/envoy/pull/38757#issuecomment-2726090120)
Copy link  Copy Markdown   
|  As a reminder, PRs marked as draft will not be automatically assigned reviewers, or be handled by maintainer-oncall triage. Please mark your PR as ready when you want it to be reviewed! 🐱 Caused by: [#38757](https://github.com/envoyproxy/envoy/pull/38757) was opened by yanavlasov. see: [more](https://github.com/envoyproxy/envoy/pull/38757), [trace](https://prod.repokitteh.app/traces/ui/envoyproxy/envoy/56307580-013b-11f0-92cb-b2bae559ca1f).  |  
| --- |  
[repokitteh-read-only](https://github.com/apps/repokitteh-read-only) Bot added the [ api ](https://github.com/envoyproxy/envoy/issues?q=state%3Aopen%20label%3Aapi) label [Mar 15, 2025](https://github.com/envoyproxy/envoy/pull/38757#event-16782699163)
[repokitteh-read-only](https://github.com/apps/repokitteh-read-only) Bot assigned [markdroth](https://github.com/markdroth) [Mar 15, 2025](https://github.com/envoyproxy/envoy/pull/38757#event-16782699225)
### 
**[repokitteh-read-only](https://github.com/apps/repokitteh-read-only) Bot ** commented [Mar 15, 2025](https://github.com/envoyproxy/envoy/pull/38757#issuecomment-2726090167)
Copy link  Copy Markdown   
|  CC @envoyproxy/api-shepherds: Your approval is needed for changes made to `(api/envoy/|docs/root/api-docs/)`. envoyproxy/api-shepherds assignee is [@markdroth](https://github.com/markdroth) CC @envoyproxy/api-watchers: FYI only for changes made to `(api/envoy/|docs/root/api-docs/)`. 🐱 Caused by: [#38757](https://github.com/envoyproxy/envoy/pull/38757) was opened by yanavlasov. see: [more](https://github.com/envoyproxy/envoy/pull/38757), [trace](https://prod.repokitteh.app/traces/ui/envoyproxy/envoy/56307580-013b-11f0-92cb-b2bae559ca1f).  |  
| --- |  
### 
commented [Mar 15, 2025](https://github.com/envoyproxy/envoy/pull/38757#issuecomment-2726282980) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).
Copy link  Copy Markdown 
Member  
|  [@yanavlasov](https://github.com/yanavlasov) If I got this PR correctly, this PR provides a way to allow the downstream/filters to force the LB to select a specific host (by header or metadata). If if there is no target host, then the fallback lb will be used? If it's your goal, I think we have provided the `setUpstreamOverrideHost()` which could specific a host directly. If the override host is set, then all LB will select that host as priority except the health status of that host doesn't meet requirements. So, I think it would be better to use or enhance the `setUpstreamOverrideHost()` directly rather than to add a new complex LB. /wait-any  |  
| --- |  
[repokitteh-read-only](https://github.com/apps/repokitteh-read-only) Bot added the [ waiting:any ](https://github.com/envoyproxy/envoy/issues?q=state%3Aopen%20label%3Awaiting%3Aany) label [Mar 15, 2025](https://github.com/envoyproxy/envoy/pull/38757#event-16784295390)
[wbpcode](https://github.com/wbpcode) self-assigned this [Mar 15, 2025](https://github.com/envoyproxy/envoy/pull/38757#event-16784295461)

```
Signed-off-by: Yan Avlasov <yavlasov@google.com>
```

[repokitteh-read-only](https://github.com/apps/repokitteh-read-only) Bot removed the [ waiting:any ](https://github.com/envoyproxy/envoy/issues?q=state%3Aopen%20label%3Awaiting%3Aany) label [Mar 17, 2025](https://github.com/envoyproxy/envoy/pull/38757#event-16810670671)
### 
commented [Mar 17, 2025](https://github.com/envoyproxy/envoy/pull/38757#issuecomment-2730495099)
Copy link  Copy Markdown 
Contributor Author  
| 
> [@yanavlasov](https://github.com/yanavlasov) If I got this PR correctly, this PR provides a way to allow the downstream/filters to force the LB to select a specific host (by header or metadata). If if there is no target host, then the fallback lb will be used? If it's your goal, I think we have provided the `setUpstreamOverrideHost()` which could specific a host directly. If the override host is set, then all LB will select that host as priority except the health status of that host doesn't meet requirements. So, I think it would be better to use or enhance the `setUpstreamOverrideHost()` directly rather than to add a new complex LB.
The goal is broader than the downstream filter overriding the upstream endpoint. The first part is to allow selection of endpoints with ext_proc filter - the external endpoint picker. It needs to provide primary and retry hosts. The eventual goal is to make ext_lb - protocol for external picking of endpoints, as the initial approach is not very effective for retries - it can end up using stale retry endpoints. This is part of the ai-gateway work to support load balancing to inference workloads where load balancing policy can be more complicated than what is supported by Envoy today and where there often operator specific business logic for picking endpoints. Initially this LB support this proposal in k8s inference gateway exptensions: <https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/004-endpoint-picker-protocol> I did not think the `setUpstreamOverrideHost()` and an HTTP filter is going to meet **both** of these requirements and started adding LB policy, which will eventually support the remote callout protocol.  |  
| --- |  
[yanavlasov](https://github.com/yanavlasov) added 3 commits [March 17, 2025 19:44](https://github.com/envoyproxy/envoy/pull/38757#commits-pushed-c6be990)
` Add CODEOWNERS and fix proto doc[](https://github.com/envoyproxy/envoy/pull/38757/commits/c6be9907b47d3f0beb5994b556ec48fc88ca3576 "Add CODEOWNERS and fix proto doc

Signed-off-by: Yan Avlasov <yavlasov@google.com>") `

```
Signed-off-by: Yan Avlasov <yavlasov@google.com>
```


```
Signed-off-by: Yan Avlasov <yavlasov@google.com>
```


```
Signed-off-by: Yan Avlasov <yavlasov@google.com>
```

reviewed  [ Mar 17, 2025 ](https://github.com/envoyproxy/envoy/pull/38757#pullrequestreview-2691992935)
[ View reviewed changes ](https://github.com/envoyproxy/envoy/pull/38757/files/08abf293d3b09b0b63af87b4521019a93c41a64a)
Comment thread [api/envoy/extensions/load_balancing_policies/dynamic_forwarding/v3/dynamic_forwarding.proto](https://github.com/envoyproxy/envoy/pull/38757/files/08abf293d3b09b0b63af87b4521019a93c41a64a#diff-f79e54693352808fa55a8d549954976d71b61144b4168840dca9785d6340e17e) Outdated 
Comment on lines to   
|  // If neither header nor metadata is present or there were errors parsing header or metadata values the  |  
| --- |  
|  // specified fallback load balancing policy is used. This allows load balancing to degrade to a  |  
|  // a built in policy (i.e. Round Robin) in case external endpoint picker fails.  |  
### 
[Mar 17, 2025](https://github.com/envoyproxy/envoy/pull/38757#discussion_r1999617560)
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
sorry if i am missing something and i am no expert in the LB code, but does this mean that the fallback happens only when the header or metadata parsing failed, not when a HTTP request failed or received 5xx ? cc [@yuzisun](https://github.com/yuzisun) [@wengyao04](https://github.com/wengyao04)
### 
[Mar 17, 2025](https://github.com/envoyproxy/envoy/pull/38757#discussion_r1999667148)
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
ah I _guess_ the fallback here != the fallback/subsetting in the proposal ?
### 
[Mar 18, 2025](https://github.com/envoyproxy/envoy/pull/38757#discussion_r1999941292)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
What happens when request failed or received unsuccessful HTTP status is determined by the route retry policy. It is not up to LB policy. If request has to be retried per policy, the router filter will call `chooseHost` again to get endpoint for retry attempt.
The fallback LB may be called in this case, if there are no retry endpoints specified in the metadata or headers (note that this PR does not implement this functionality - it will be added in the next PR) or there are more retry attempts than endpoints in the metatata or headers.
### 
[Mar 18, 2025](https://github.com/envoyproxy/envoy/pull/38757#discussion_r2001629408)
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
thank you for clarifying, Yan... i am still trying to get my heads around exactly when this fallback via `x-gateway-destination-endpoint-fallback` happens. Since I read that both `x-gateway-destination-endpoint-fallback` and `x-gateway-destination-endpoint` are set by the extproc, what's the point of having two separate metadata? The proposal doc doesn't mention exactly when fallback happens (sorry if i miss anything). I think i should wait for the next PR as well as the reference impl...
### 
[Mar 19, 2025](https://github.com/envoyproxy/envoy/pull/38757#discussion_r2002203655)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
There are two fallback things: fallback endpoints and fallback load balancing policy. The fallback endpoints are set by the endpoint picking extension. For example it can select one primary endpoint and one fallback, based on their queue length. If the primary endpoint is not reachable or returned an error status the fallback endpoint will be used for retry.
The fallback load balancing policy is used when endpoint picker has failed altogether and set neither primary nor fallback endpoints. In this case Envoy will use potentially inefficient fallback LB policy, but would still be able to serve the request.
### 
[Mar 19, 2025](https://github.com/envoyproxy/envoy/pull/38757#discussion_r2003886107)
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
ok now it all has started making sense and see the clear distinction between fallback lb policy vs fallback endpoints in the metadata. I think I would love to see the actual implementation of the fallback endpoints logic then, which will clear all my remaining questions.
[mathetake](https://github.com/mathetake) mentioned this pull request [ Mar 17, 2025 ](https://github.com/envoyproxy/envoy/pull/38757#ref-pullrequest-2918168689)
[ docs: adds high level proposal on GAIE support theagentrouter/agent-router#492 ](https://github.com/theagentrouter/agent-router/pull/492)
Merged 

```
Signed-off-by: Yan Avlasov <yavlasov@google.com>
```

[yanavlasov](https://github.com/yanavlasov) marked this pull request as ready for review [March 18, 2025 00:57](https://github.com/envoyproxy/envoy/pull/38757#event-16816860436)
### 
commented [Mar 18, 2025](https://github.com/envoyproxy/envoy/pull/38757#issuecomment-2731307740)
Copy link  Copy Markdown 
Contributor Author  
|  [@LiorLieberman](https://github.com/LiorLieberman) PR with the LB policy  |  
| --- |  
[yanavlasov](https://github.com/yanavlasov) added 2 commits [March 18, 2025 01:11](https://github.com/envoyproxy/envoy/pull/38757#commits-pushed-7a62842)

```
Signed-off-by: Yan Avlasov <yavlasov@google.com>
```


```
Signed-off-by: Yan Avlasov <yavlasov@google.com>
```

### 
commented [Mar 19, 2025](https://github.com/envoyproxy/envoy/pull/38757#issuecomment-2737360555) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).
Copy link  Copy Markdown 
Member  
|  I also did some similar work. From the implementation of the code, I didn't found any thing is the setUpstreamOverrideHost cannot do (except the retry.). In the current implementation, all complex AI related selection will be handled by the ext_proc sever (like polling metrics and comparing the load), and finally the ext proc server will return some endpoint candidates and the ext proc filter will set these endpoint candidates to metadata and lets the LB to use the metadata to do the final decision. But the story could much simpler, the ext proc filter could set these endpoint candidates by the setUpstreamOverrideHost. This only enhancement necessarily is making setUpstreamOverrideHost could accept multiple addresses to support retry. The asynchronous load balancing and external scheduler is a great point (and is helpful to make accurate retry) and I also happy to see it, If this PR is concentrate on that, I am fine. But from the code and the API, seems it doesn't?  |  
| --- |  
### 
commented [Mar 20, 2025](https://github.com/envoyproxy/envoy/pull/38757#issuecomment-2738684929)
Copy link  Copy Markdown 
Contributor Author  
| 
> I also did some similar work. From the implementation of the code, I didn't found any thing is the setUpstreamOverrideHost cannot do (except the retry.). In the current implementation, all complex AI related selection will be handled by the ext_proc sever (like polling metrics and comparing the load), and finally the ext proc server will return some endpoint candidates and the ext proc filter will set these endpoint candidates to metadata and lets the LB to use the metadata to do the final decision. But the story could much simpler, the ext proc filter could set these endpoint candidates by the setUpstreamOverrideHost. This only enhancement necessarily is making setUpstreamOverrideHost could accept multiple addresses to support retry. The asynchronous load balancing and external scheduler is a great point (and is helpful to make accurate retry) and I also happy to see it, If this PR is concentrate on that, I am fine. But from the code and the API, seems it doesn't?
Adding functionality to set upstream hosts to ext_proc is a move in the wrong direction. It will not work for <https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/main/docs/proposals/004-endpoint-picker-protocol> but also conceptually I do not think it belongs there. The `setUpstreamOverrideHost()` API does not look like a good choice to me. It was added to support session affinity before we had load balancing policies, and I think it would have been better implemented as LB policy rather than this additional method on the decoder filter callbacks. Using LB policy is I think a better choice as it fits better with the abstraction model, where LB policy is responsible for selecting the endpoint with request attributes as one of the inputs. And while it does not right now have remote callout it would be a natural next step to implement it there.  |  
| --- |  
### 
commented [Mar 20, 2025](https://github.com/envoyproxy/envoy/pull/38757#issuecomment-2738900780) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).
Copy link  Copy Markdown 
Member  
| 
> The setUpstreamOverrideHost() API does not look like a good choice to me. It was added to support session affinity before we had load balancing policies
Long time ago, when [@htuch](https://github.com/htuch) and me designed this API, **we hope it's a common way to let the downstream to effect the LB selection and we make it's LB_POLICY-independent by design.** `session affinity` is only one of usage. **And we now have expose this API to lua. And this method may be exposed to wasm/dynamic module to let the dynamic extension could also effect the LB selection.** I personally think there should no difference between ext_proc and lua/wasm, there all are ways to extend envoy dynamically. The setUpstreamOverrideHost() API make the whole LB selection a 2-ply structure. The first layer will select the host based on the downstream's explicit input (see `ThreadLocalClusterManagerImpl::ClusterEntry::chooseHost`) . The second layer will select the host based on the LB policy if we cannot find one at the first layer (see different implementations of LB policy). **I think the`Dynamic forwarding load balancing policy` actually do completely same thing: it also try to make the LB selection a 2-ply structure. The first layer will select the host based on the downstream's explicit input (metadata/headers). The second layer fallback to a LB policy if we cannot find one at the first layer. See, it's completely same in high-level.** I removed some comments, because after a re-thinking, some comment may be one-sided. In conclusion, I think what actually is important is the 2-ply structure for host selection: **one for downstream selection/decision, one for upstream load balancing. These two layer should be orthogonal or independent.** That's say, we actually could add an extension point at the cluster (We have added a `override_host_status` in the cluster in the past, we can add a new field `override_host_policy` there) to support to get addresses from downstream's direct input (like your current metadata/headers/etc.). I also added a draft PR to show how it works: [#38814](https://github.com/envoyproxy/envoy/pull/38814) This extension will be evaluated first at the `ThreadLocalClusterManagerImpl::ClusterEntry::chooseHost`. If the extension return a valid host, then that host will be selected directly. Or we will call the LB policy to do the load balancing. This way could meet your current requirements of the AI gateway without adding a complete new LB policy. And it still fine if you want to support async load balancing in the future. This way have following advantages:
  1. The downstream selection (override_host_policy) is completely independent with the upstream load balancing. We can composite different different downstream selection policies (override_host_extension) and upstream load balanciing policies.
  2. It's unnecessary to add such a big new lb policy implementation (thousands lines of code). A simple/light extension could cover you current logic.
  3. No need to handle the fallback. That's supported for now.
  4. No need to handle the embeded load balancing policy. Load balancing policy is a pretty complex extension, thread aware load balancer, load balancer factory, worker local load balancer, all these make it's pretty hard to write or mainatain. If embeded sub local balancing policy is used, it brings more complexity.

 |  
| --- |  
### 
commented [Mar 20, 2025](https://github.com/envoyproxy/envoy/pull/38757#issuecomment-2739349490)
Copy link  Copy Markdown 
Member  
|  Sorry I started a long discussion about PR and even after you have done something. 😞 I am pretty happy to see more AI related thing in the Envoy (because I am also doing some similar thing). But if we add this new extension/API, then it basically is forever, so, I think it deserve more disscussion.  |  
| --- |  
### 
commented [Mar 21, 2025](https://github.com/envoyproxy/envoy/pull/38757#issuecomment-2743668059)
Copy link  Copy Markdown 
Contributor Author  
|  I do not see the benefit of adding an extension to an extension over just adding an extension. Why would the size of code be any different - it still needs to do the same thing? The LB extension itself is 500 lines of code it is not thousands. There are multiple ways of achieving the same goal, this is a very good way, which fits with the current abstraction model for LBs in Envoy, so there is no reason to be concerned about its longevity. The ways you are proposing do not seem to offer any particular benefits.  |  
| --- |  
### 
commented [Mar 21, 2025](https://github.com/envoyproxy/envoy/pull/38757#issuecomment-2743892293) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).
Copy link  Copy Markdown 
Member  
| 
> I do not see the benefit of adding an extension to an extension over just adding an extension. Why would the size of code be any different - it still needs to do the same thing? The LB extension itself is 500 lines of code it is not thousands. There are multiple ways of achieving the same goal, this is a very good way, which fits with the current abstraction model for LBs in Envoy, so there is no reason to be concerned about its longevity. The ways you are proposing do not seem to offer any particular benefits.
  1. The core logic of this PR is actually pretty simple (search a header or metadata and return the results), but we use lots code to handle the lb embedding. I think the complexity is be enlarged because the lb self's complexity. Ideally, you only need to implement a very simple extension (may be no more than 100 lines.)to get a string from headers or metadata. The test would also be very simple.
  2. The core logic of this PR actually is not part of the upstream load balancing. **It actually inject additional layer (direct selection from downstream) in the path. It would be better to split it into a another specific abstraction. That provides the possibility to extend without changing the exist code (like we can support to get addresses from filter state, by CEL, etc. by a new extension).**
  3. Make this logic could also be used by clusters which doesn't support custom lb policy. (The clusters who user cluster provided lb.)

 |  
| --- |  
[mathetake](https://github.com/mathetake) mentioned this pull request [ Mar 21, 2025 ](https://github.com/envoyproxy/envoy/pull/38757#ref-issue-2938251490)
[ Support fallback to different backend cluster based on response status code from the primary cluster #38841 ](https://github.com/envoyproxy/envoy/issues/38841)
[wbpcode](https://github.com/wbpcode) mentioned this pull request [ Mar 22, 2025 ](https://github.com/envoyproxy/envoy/pull/38757#ref-pullrequest-2935391195)
[ upstream: new override host policy support #38814 ](https://github.com/envoyproxy/envoy/pull/38814)
Closed 
###  This comment was marked as off-topic. 
### 
commented [Apr 23, 2025](https://github.com/envoyproxy/envoy/pull/38757#issuecomment-2825377442)
Copy link  Copy Markdown 
Contributor  
|  /lgtm api  |  
| --- |  
[repokitteh-read-only](https://github.com/apps/repokitteh-read-only) Bot removed the [ api ](https://github.com/envoyproxy/envoy/issues?q=state%3Aopen%20label%3Aapi) label [Apr 23, 2025](https://github.com/envoyproxy/envoy/pull/38757#event-17377577301)
### 
commented [Apr 24, 2025](https://github.com/envoyproxy/envoy/pull/38757#issuecomment-2827975132)
Copy link  Copy Markdown 
Contributor Author  
|  [@wbpcode](https://github.com/wbpcode) I have addressed all comments, please take a look.  |  
| --- |  
### 
commented [Apr 25, 2025](https://github.com/envoyproxy/envoy/pull/38757#issuecomment-2829329012)
Copy link  Copy Markdown 
Member  
|  got it, thanks, I will take a look today.  |  
| --- |  
reviewed  [ Apr 30, 2025 ](https://github.com/envoyproxy/envoy/pull/38757#pullrequestreview-2806458864)
[ View reviewed changes ](https://github.com/envoyproxy/envoy/pull/38757/files/dda32f83677030842d678c0e969a73668949e214)
### 
left a comment 
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Thanks for this great contribution. It's happy to see thing is landing gradually 🌹 ❤️ I added some comment to the API and the implementation.
/wait
Comment thread [source/extensions/load_balancing_policies/override_host/load_balancer.cc](https://github.com/envoyproxy/envoy/pull/38757/files/dda32f83677030842d678c0e969a73668949e214#diff-9713bf52504c2dd887b1b6bf6a47ad83692947700fa85706310a557d5611d11a) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).
Comment thread [source/extensions/load_balancing_policies/override_host/load_balancer.cc](https://github.com/envoyproxy/envoy/pull/38757/files/dda32f83677030842d678c0e969a73668949e214#diff-9713bf52504c2dd887b1b6bf6a47ad83692947700fa85706310a557d5611d11a) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).
Comment thread [source/extensions/load_balancing_policies/override_host/load_balancer.cc](https://github.com/envoyproxy/envoy/pull/38757/files/dda32f83677030842d678c0e969a73668949e214#diff-9713bf52504c2dd887b1b6bf6a47ad83692947700fa85706310a557d5611d11a) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).
Comment thread [source/extensions/load_balancing_policies/override_host/load_balancer.cc](https://github.com/envoyproxy/envoy/pull/38757/files/dda32f83677030842d678c0e969a73668949e214#diff-9713bf52504c2dd887b1b6bf6a47ad83692947700fa85706310a557d5611d11a) Outdated 
Comment on lines  +188  to  +231   
|  absl::StatusOr<std::unique_ptr<SelectedHosts>>  |  
| --- |  
|  OverrideHostLoadBalancer::LoadBalancerImpl::getSelectedHostsFromMetadata(  |  
|  const ::envoy::config::core::v3::Metadata& metadata, const Config::MetadataKey& metadata_key) {  |  
|  std::unique_ptr<SelectedHosts> selected_hosts;  |  
|  const ProtobufWkt::Value& metadata_value =  |  
|  Config::Metadata::metadataValue(&metadata, metadata_key);  |  
|  if (metadata_value.has_string_value() && !metadata_value.string_value().empty()) {  |  
|  auto selected_hosts_result = SelectedHosts::make(metadata_value.string_value());  |  
|  if (!selected_hosts_result.ok()) {  |  
|  ENVOY_LOG(trace, "Failed to parse SelectedEndpoints OSS {} with error {}",  |  
|  metadata_value.string_value(), selected_hosts_result.status().message());  |  
|  return selected_hosts_result.status();  |  
|  selected_hosts = std::move(selected_hosts_result.value());  |  
|  return selected_hosts;  |  
|  absl::StatusOr<std::unique_ptr<SelectedHosts>>  |  
|  OverrideHostLoadBalancer::LoadBalancerImpl::getSelectedHostsFromHeader(  |  
|  const Envoy::Http::RequestHeaderMap* header_map, const Http::LowerCaseString& header_name) {  |  
|  std::unique_ptr<SelectedHosts> selected_hosts;  |  
|  if (!header_map) {  |  
|  return selected_hosts;  |  
|  HeaderMap::GetResult result = header_map->get(header_name);  |  
|  if (result.empty()) {  |  
|  return selected_hosts;  |  
|  // Use only the first value of the header, if it happens to be have multiple.  |  
|  const std::string primary_host_address(result[0]->value().getStringView());  |  
|  Envoy::Network::Address::InstanceConstSharedPtr primary_host =  |  
|  Envoy::Network::Utility::parseInternetAddressAndPortNoThrow(primary_host_address, false);  |  
|  if (!primary_host || primary_host->type() != Envoy::Network::Address::Type::Ip) {  |  
|  ENVOY_LOG(debug, "Invalid primary host in header {}: {}", header_name, primary_host_address);  |  
|  return absl::InvalidArgumentError("Invalid primary host in header");  |  
|  // TODO(yanavlasov): implement parsing of fallback headers  |  
|  selected_hosts = std::make_unique<SelectedHosts>(  |  
|  SelectedHosts{{{primary_host->ip()->addressAsString(), primary_host->ip()->port()}}, {}});  |  
|  return selected_hosts;  |  
### 
[Apr 30, 2025](https://github.com/envoyproxy/envoy/pull/38757#discussion_r2068360513) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I do think what we need here is to get first valid `string` (or even `string_view` i think) from the header or metadata source. Then we use the string to find a host from the cross host map. If the string is unvalid address, then we get nothing from the map and we will fallback to the default policy.
So, IMO, the parsing here make the code more complex and bring some performance overhead, but only bring trival benefit.
And from the definition of the `SelectedHosts`, you may want to get primary host and fallback host at same time in the future. But you actually could do that after the first choice is failed. We can use a `absl::StatusOr<absl::string_view> getSelectedHosts(LoadBalancerContext* context, const std::vector<OverrideSource>& soruces)` to share the search logic.
By the way, I actually think we needn't the `fallback_host_sources`. The header/metadata value could be a list of addresses the separated by the `,` (for example, `1.2.3.4:90,1.2.3.5:90`), then we try first address at first attempt and try second address at second attempt. This will make thing much simpler.
### 
[May 6, 2025](https://github.com/envoyproxy/envoy/pull/38757#discussion_r2074599606)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I have reworked the code.
Comment thread [source/extensions/load_balancing_policies/override_host/load_balancer.cc](https://github.com/envoyproxy/envoy/pull/38757/files/dda32f83677030842d678c0e969a73668949e214#diff-9713bf52504c2dd887b1b6bf6a47ad83692947700fa85706310a557d5611d11a) Outdated 
Comment on lines  +309  to  +324   
|  if (!metadata.filter_metadata().contains(kEndpointsFallbackIndexKey)) {  |  
| --- |  
|  // Use the primary endpoint.  |  
|  ENVOY_LOG(trace, "Selecting primary endpoint {}", selected_hosts.primary.address.address);  |  
|  // Endpoint extracted from the header does not have locality.  |  
|  HostConstSharedPtr host = findHost(selected_hosts.primary);  |  
|  // If the primary endpoint was found in the current host set, use it.  |  
|  // Otherwise try to see if one of the failover endpoints is available. This  |  
|  // is possible when the cluster received EDS update while the request to the  |  
|  // endpoint picker was in flight.  |  
|  if (host) {  |  
|  // Save the first index into fallback hosts, so that subsequent calls to  |  
|  // chooseHost method will use the fallback hosts.  |  
|  updateFallbackIndexMetadata(metadata, 0);  |  
|  return host;  |  
### 
[Apr 30, 2025](https://github.com/envoyproxy/envoy/pull/38757#discussion_r2068364471) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Prefer filter state to store the index. Then in the future, we may could store the address list in that filter state in the future.
### 
[May 6, 2025](https://github.com/envoyproxy/envoy/pull/38757#discussion_r2074598866)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Comment thread [api/envoy/extensions/load_balancing_policies/override_host/v3/override_host.proto](https://github.com/envoyproxy/envoy/pull/38757/files/dda32f83677030842d678c0e969a73668949e214#diff-2b23e9a3cf884a3c2928acaa6512f355e6ea029598491db6df4e9122ec7c541b) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).
Comment thread [api/envoy/extensions/load_balancing_policies/override_host/v3/override_host.proto](https://github.com/envoyproxy/envoy/pull/38757/files/dda32f83677030842d678c0e969a73668949e214#diff-2b23e9a3cf884a3c2928acaa6512f355e6ea029598491db6df4e9122ec7c541b) Outdated 
Comment on lines to   
|  // A list of host sources to get the fallback host addresses from. The first one who provides  |  
| --- |  
|  // the addresses will be used, no matter if the addresses are valid or not.  |  
|  // [#not-implemented-hide:]  |  
|  repeated OverrideHostSource fallback_host_sources = 2;  |  
### 
[Apr 30, 2025](https://github.com/envoyproxy/envoy/pull/38757#discussion_r2068371759) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
I didn't see the implementation, but I guess the fallback host source will contains a list of addresses, like `1.2.3.4:90,1.2.3.5:90`. Then I think we can use single host source which contains multiple addresses for both primary and fallback? Then we needn't `primary_host_sources` and `fallback_host_sources`, we only need one `host_sources`.
I saw proposal in the <https://github.com/kubernetes-sigs/gateway-api-inference-extension/> ... only single fallback address could be used.....
Okay, I will try to create PR at there to see if that possible to use single source for multiple address. Before that, I inclined to use only one `host_sources` first in our API.
### 
[Apr 30, 2025](https://github.com/envoyproxy/envoy/pull/38757#discussion_r2068710115) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
See also [kubernetes-sigs/gateway-api-inference-extension#761](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761)
### 
[May 6, 2025](https://github.com/envoyproxy/envoy/pull/38757#discussion_r2074521222)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
You still need two configurations for cases where an implementation that uses original_dst is transitioning to the override_host policy.
### 
[May 6, 2025](https://github.com/envoyproxy/envoy/pull/38757#discussion_r2074645619) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
> You still need two configurations for cases where an implementation that uses original_dst is transitioning to the override_host policy.
Yeah, the migration is another problem. The EPP server may need to aware whether the proxy provide the fallback support and use different header/metadata value format.
**But although the EPP is one of our design target, I will still hope we can treat this new extension as general feature of Envoy and the EPP is only one usage of the new extension.** That's say, we may needn't to consider the backward compatiblity of EPP self here, we should let the EPP server to resolve the compatibility problem and the new extension of Envoy only need to support the required feature like fallback, retrying, etc?
cc [@yanavlasov](https://github.com/yanavlasov) WDYT?
### 
[May 6, 2025](https://github.com/envoyproxy/envoy/pull/38757#discussion_r2074662665) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
And no matter how the EPP implement the fallback, we can always treat the header value or metadata value (from the specified `host_sources`) as list of addresses and iterate them when finding a valid host. (The single address case could be treat as list which contains only one element.)
So, I still think we can keep only one `host_sources` field here to parse an address list （from the the value of specified `host_sources`）, and remove the `fallback_host_sources` in this initial version.
If finally we prove that the `fallback_host_sources` is the only choice, we can add it in the future.
### 
[May 8, 2025](https://github.com/envoyproxy/envoy/pull/38757#discussion_r2079818556)
Copy link  Copy Markdown 
Contributor Author
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Ok, sounds good
Comment thread [api/envoy/extensions/load_balancing_policies/override_host/v3/override_host.proto](https://github.com/envoyproxy/envoy/pull/38757/files/dda32f83677030842d678c0e969a73668949e214#diff-2b23e9a3cf884a3c2928acaa6512f355e6ea029598491db6df4e9122ec7c541b) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).
[repokitteh-read-only](https://github.com/apps/repokitteh-read-only) Bot added the [ waiting ](https://github.com/envoyproxy/envoy/issues?q=state%3Aopen%20label%3Awaiting) label [Apr 30, 2025](https://github.com/envoyproxy/envoy/pull/38757#event-17470377327)
[wbpcode](https://github.com/wbpcode) mentioned this pull request [ Apr 30, 2025 ](https://github.com/envoyproxy/envoy/pull/38757#ref-pullrequest-3031295825)
[ Amend the endpoint picker protocol to support multiple fallback endpoints kubernetes-sigs/gateway-api-inference-extension#761 ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/761)
Merged 
[danehans](https://github.com/danehans) mentioned this pull request [ May 1, 2025 ](https://github.com/envoyproxy/envoy/pull/38757#ref-issue-2751302693)
[ Add Initial Gateway API Inference Extension Support kgateway-dev/kgateway#10411 ](https://github.com/kgateway-dev/kgateway/issues/10411)
Closed 
25 tasks
### 
commented [May 1, 2025](https://github.com/envoyproxy/envoy/pull/38757#issuecomment-2845854295)
Copy link  Copy Markdown 
Contributor  
| 
> > Sorry, I misunderstood it. Yeah, these typically should be work of an external LB policy or HTTP filter. So, if i get you correctly, we need: 1. Select host from downstream input; 2. Call the external LB; 3. Fallback to local LB?
> The requirements that I have right now are the following:
>   1. Choose primary endpoint based on metadata or headers (for example set by ext_proc callout or WASM)
>   2. Choose from a list of retry endpoints when router selects hosts for retries.
>   3. Use fallback LB in case headers and metadata is not present.
>   4. Support fallback LBs with EDS provisioned endpoints and original_dst behavior, where endpoint populated in the cluster based on the header metadata values.
>   5. Support combined EDS and original_dst behavior. For example if an endpoint specified by external picker does not exist in the set of endpoints provided by EDS it is automatically added to the endpoint set. This supports various Cloud use cases depending on how the inference pool is configured or for customer provided control planes.
>   6. Provide external endpoint picker with the list of endpoints in the locality of the proxy (i.e.e in the request metadata). At this point my solution will require an additional HTTP filter to work with LB extension.
>   7. Eventually implement lb_ext protocol for calling out to external endpoint picker as part of endpoint selection.
> 

Are these other requirements captures in another issue or design doc somewhere?  |  
| --- |  
[yanavlasov](https://github.com/yanavlasov) added 2 commits [May 5, 2025 18:26](https://github.com/envoyproxy/envoy/pull/38757#commits-pushed-937c0d1)
` Merge branch 'main' into dynamic-forwarding-lb[](https://github.com/envoyproxy/envoy/pull/38757/commits/937c0d11b4eaf70bdfba81d8bd558264b868b4fd "Merge branch 'main' into dynamic-forwarding-lb") `

```
Signed-off-by: Yan Avlasov <yavlasov@google.com>
```

[repokitteh-read-only](https://github.com/apps/repokitteh-read-only) Bot added [ api ](https://github.com/envoyproxy/envoy/issues?q=state%3Aopen%20label%3Aapi) and removed [ waiting ](https://github.com/envoyproxy/envoy/issues?q=state%3Aopen%20label%3Awaiting) labels [May 6, 2025](https://github.com/envoyproxy/envoy/pull/38757#event-17529089411)
### 
commented [May 6, 2025](https://github.com/envoyproxy/envoy/pull/38757#issuecomment-2853043578)
Copy link  Copy Markdown 
Contributor Author  
| 
> > > Sorry, I misunderstood it. Yeah, these typically should be work of an external LB policy or HTTP filter. So, if i get you correctly, we need: 1. Select host from downstream input; 2. Call the external LB; 3. Fallback to local LB?
>> The requirements that I have right now are the following:
>>   1. Choose primary endpoint based on metadata or headers (for example set by ext_proc callout or WASM)
>>   2. Choose from a list of retry endpoints when router selects hosts for retries.
>>   3. Use fallback LB in case headers and metadata is not present.
>>   4. Support fallback LBs with EDS provisioned endpoints and original_dst behavior, where endpoint populated in the cluster based on the header metadata values.
>>   5. Support combined EDS and original_dst behavior. For example if an endpoint specified by external picker does not exist in the set of endpoints provided by EDS it is automatically added to the endpoint set. This supports various Cloud use cases depending on how the inference pool is configured or for customer provided control planes.
>>   6. Provide external endpoint picker with the list of endpoints in the locality of the proxy (i.e.e in the request metadata). At this point my solution will require an additional HTTP filter to work with LB extension.
>>   7. Eventually implement lb_ext protocol for calling out to external endpoint picker as part of endpoint selection.
>> 

> Are these other requirements captures in another issue or design doc somewhere?
No, I do not have them captured in other place.  |  
| --- |  
reviewed  [ May 6, 2025 ](https://github.com/envoyproxy/envoy/pull/38757#pullrequestreview-2816734692)
[ View reviewed changes ](https://github.com/envoyproxy/envoy/pull/38757/files/5e111a6130d2de9de0d44f87a0ef27869a173895)
### 
left a comment 
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Thanks for the update. Some more comments are added.
Comment thread [source/extensions/load_balancing_policies/override_host/selected_hosts.h](https://github.com/envoyproxy/envoy/pull/38757/files/5e111a6130d2de9de0d44f87a0ef27869a173895#diff-135ff279e14ab9c1aae0f1143c2cd3704e0d982bbbc6de90a215e562ce98f248) Outdated 
Comment on lines to   
|  struct SelectedHosts {  |  
| --- |  
|  struct Endpoint {  |  
|  public:  |  
|  struct Address {  |  
|  const std::string address;  |  
|  const uint32_t port;  |  
|  const Address address;  |  
|  const Endpoint primary;  |  
|  const std::vector<Endpoint> failover;  |  
### 
[May 6, 2025](https://github.com/envoyproxy/envoy/pull/38757#discussion_r2074657955)
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
It's unecessary to split the primary address and fallback addreses into two field. We will iterate then in order anyway. And it's also unnecessary to use the `Address` struct.
A simple `std::vector<std::string>` could be used as the `SelectedHosts`
Comment thread [source/extensions/load_balancing_policies/override_host/selected_hosts.cc](https://github.com/envoyproxy/envoy/pull/38757/files/5e111a6130d2de9de0d44f87a0ef27869a173895#diff-fdfbff74cae6e15c7f9493c43001eebcc06d8c33c44515df7b4406ef8b66110f) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).
reviewed  [ May 6, 2025 ](https://github.com/envoyproxy/envoy/pull/38757#pullrequestreview-2817014039)
[ View reviewed changes ](https://github.com/envoyproxy/envoy/pull/38757/files/5e111a6130d2de9de0d44f87a0ef27869a173895)
Comment thread [api/envoy/extensions/load_balancing_policies/override_host/v3/override_host.proto](https://github.com/envoyproxy/envoy/pull/38757/files/5e111a6130d2de9de0d44f87a0ef27869a173895#diff-2b23e9a3cf884a3c2928acaa6512f355e6ea029598491db6df4e9122ec7c541b) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).
[yanavlasov](https://github.com/yanavlasov) added 5 commits [May 7, 2025 00:57](https://github.com/envoyproxy/envoy/pull/38757#commits-pushed-ffcef01)

```
Signed-off-by: Yan Avlasov <yavlasov@google.com>
```


```
Signed-off-by: Yan Avlasov <yavlasov@google.com>
```

` Merge branch 'main' into dynamic-forwarding-lb[](https://github.com/envoyproxy/envoy/pull/38757/commits/7405ff384dc77a2189132e0fed0f635742b4c5eb "Merge branch 'main' into dynamic-forwarding-lb") `

```
Signed-off-by: Yan Avlasov <yavlasov@google.com>
```


```
Signed-off-by: Yan Avlasov <yavlasov@google.com>
```

reviewed  [ May 8, 2025 ](https://github.com/envoyproxy/envoy/pull/38757#pullrequestreview-2825554282)
[ View reviewed changes ](https://github.com/envoyproxy/envoy/pull/38757/files/47d2994d4ac15c9eebe4dfbb1c7733e898dfced7)
### 
left a comment 
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
LGTM overall with only one minor comment, thanks for the update.
Comment thread [api/envoy/extensions/load_balancing_policies/override_host/v3/override_host.proto](https://github.com/envoyproxy/envoy/pull/38757/files/7405ff384dc77a2189132e0fed0f635742b4c5eb..7a7531c749cf386aedb7e389c96a729cb26c56f9#diff-2b23e9a3cf884a3c2928acaa6512f355e6ea029598491db6df4e9122ec7c541b) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/envoyproxy/envoy/pull/38757).

```
Signed-off-by: Yan Avlasov <yavlasov@google.com>
```

approved these changes  [ May 9, 2025 ](https://github.com/envoyproxy/envoy/pull/38757#pullrequestreview-2826758278)
[ View reviewed changes ](https://github.com/envoyproxy/envoy/pull/38757/files/049adde409633fbdb0acf51a84165409fe29e479)
### 
left a comment 
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
LGTM. Thanks.
[repokitteh-read-only](https://github.com/apps/repokitteh-read-only) Bot removed the [ api ](https://github.com/envoyproxy/envoy/issues?q=state%3Aopen%20label%3Aapi) label [May 9, 2025](https://github.com/envoyproxy/envoy/pull/38757#event-17583131015)
[wbpcode](https://github.com/wbpcode) merged commit [`1e5bbb5`](https://github.com/envoyproxy/envoy/commit/1e5bbb5b53f00c99d7068bc59b4e7fcaeb8d03f1) into  envoyproxy:main [May 9, 2025](https://github.com/envoyproxy/envoy/pull/38757#event-17583521252)
[yanavlasov](https://github.com/yanavlasov) deleted the  dynamic-forwarding-lb  branch [May 10, 2025 00:26](https://github.com/envoyproxy/envoy/pull/38757#event-17598665132)
[fishpan1209](https://github.com/fishpan1209) pushed a commit to fishpan1209/envoy that referenced this pull request [ May 22, 2025 ](https://github.com/envoyproxy/envoy/pull/38757#ref-commit-ba0746f)
` Dynamic forwarding load balancing policy ([](https://github.com/fishpan1209/envoy/commit/ba0746f3c9377425c24e145510bc957255c19900 "Dynamic forwarding load balancing policy \(#38757\)

Commit Message:
Additional Description:

Risk Level: Low, new extension
Testing: Unit Tests
Docs Changes: Yes
Release Notes: Yes
Platform Specific Features: N/A

---------

Signed-off-by: Yan Avlasov <yavlasov@google.com>
Signed-off-by: Ting Pan <panting@google.com>")envoyproxy#38757[](https://github.com/envoyproxy/envoy/pull/38757))[](https://github.com/fishpan1209/envoy/commit/ba0746f3c9377425c24e145510bc957255c19900 "Dynamic forwarding load balancing policy \(#38757\)

Commit Message:
Additional Description:

Risk Level: Low, new extension
Testing: Unit Tests
Docs Changes: Yes
Release Notes: Yes
Platform Specific Features: N/A

---------

Signed-off-by: Yan Avlasov <yavlasov@google.com>
Signed-off-by: Ting Pan <panting@google.com>") `

```
Commit Message:
Additional Description:

Risk Level: Low, new extension
Testing: Unit Tests
Docs Changes: Yes
Release Notes: Yes
Platform Specific Features: N/A

---------

Signed-off-by: Yan Avlasov <yavlasov@google.com>
Signed-off-by: Ting Pan <panting@google.com>
```

[danehans](https://github.com/danehans) mentioned this pull request [ Jul 14, 2025 ](https://github.com/envoyproxy/envoy/pull/38757#ref-issue-3230284956)
[ Track Envoy Main Branch kgateway-dev/kgateway#11678 ](https://github.com/kgateway-dev/kgateway/issues/11678)
Closed 
[danehans](https://github.com/danehans) mentioned this pull request [ Jul 30, 2025 ](https://github.com/envoyproxy/envoy/pull/38757#ref-issue-3277619400)
[ Inference: Refactor Plugin to Use Override_Host LB kgateway-dev/kgateway#11824 ](https://github.com/kgateway-dev/kgateway/issues/11824)
Closed 
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fenvoyproxy%2Fenvoy%2Fpull%2F38757)
###  Labels 
None yet 
###  6 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
