---
title: 'Refresh support for kgateway agentgateway GIE and GW API by danehans · Pull
  Request #421 · llm-d/llm-d · GitHub'
id: refresh-support-for-kgateway-agentgateway-gie-and-gw-api-by-danehans-pull-reques
tags:
- llmd-router-nonk8s-backends-54b774
- agentgateway
- llm-d-releases
- gateway-provider
- gie
created: '2026-09-11T20:49:53.845925Z'
updated: '2026-09-12T03:37:37.704999Z'
source: https://github.com/llm-d/llm-d/pull/421
source_domain: github.com
fetched_at: '2026-09-11T20:49:53.818296Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'llm-d PR #421 (danehans/solo.io, opened Oct 31 2025, targeted at llm-d v0.4/v0.5
  release tracker, fixes issue #399) is llm-d''s official gateway-provider refresh:
  it deprecates the legacy kgateway integration path (marking it for future removal,
  migrating remaining kgateway users to kgateway-dev''s agentgateway charts v2.2.1)
  and adds first-class agentgateway v1.0.0 support across llm-d''s provider assets,
  guides, recipes, and CI, while bumping GAIE (gateway-api-inference-extension) to
  v1.4.0 and Gateway API to v1.5.1. This is direct, dated evidence of llm-d''s own
  strategic direction: agentgateway (not kgateway or generic Envoy Gateway) is becoming
  the reference/first-class proxy for llm-d''s GIE-based routing, which matters for
  anyone building a router customization on top of llm-d components since agentgateway
  is where the standalone-EPP passthrough destinationMode work (PR #1658, #1429) landed.
  The PR stalled across multiple release cycles (v0.4 -> missed -> targeted for v0.5)
  due to reviewer bandwidth and needing a rebase; a related llm-d-infra issue ''Migrate
  kgateway Support to agentgateway'' (llm-d-incubation/llm-d-infra#269) and a kgateway-side
  test issue ''agentgateway: test llm-d support'' (kgateway-dev/kgateway#12451) show
  the migration was tracked in parallel across three repos.'
utility_score: 10.0
---

[Skip to content](https://github.com/llm-d/llm-d/pull/421#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d/pull/421) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d/pull/421) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d/pull/421) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d) You must be signed in to change notification settings
  * [ Fork 761 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)
  * [ Star  4.5k ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)


## Conversation
### 
commented [Oct 31, 2025](https://github.com/llm-d/llm-d/pull/421#issue-3572896117) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).
Copy link  Copy Markdown 
Collaborator
  * update gateway-provider assets for the deprecated `kgateway` path to use the `kgateway-dev` `agentgateway` charts at `v2.2.1`
  * add first-class [agentgateway](https://agentgateway.dev/) v1.0.0 support across provider assets, guides, recipes, and CI inputs
  * mark `kgateway` paths deprecated for future removal
  * add OpenShift-specific `AgentgatewayParameters` recipes for `agentgateway` and the legacy `kgateway` migration path
  * bump GAIE to v1.4.0 and GW API to v1.5.1


Fixes: [#399](https://github.com/llm-d/llm-d/issues/399)
[Gregory-Pereira](https://github.com/Gregory-Pereira) added this to the [v0.4 Release Tracker](https://github.com/llm-d/llm-d/milestone/3) milestone [Oct 31, 2025](https://github.com/llm-d/llm-d/pull/421#event-20635695762)
[Gregory-Pereira](https://github.com/Gregory-Pereira) self-assigned this [Oct 31, 2025](https://github.com/llm-d/llm-d/pull/421#event-20635698404)
[Gregory-Pereira](https://github.com/Gregory-Pereira) added [ gateway-provider ](https://github.com/llm-d/llm-d/issues?q=state%3Aopen%20label%3Agateway-provider) Features or issues related to gateway provider changes [ needs-maintainer-triage ](https://github.com/llm-d/llm-d/issues?q=state%3Aopen%20label%3Aneeds-maintainer-triage) Waiting on consensus from maintainers to pickup and accept this workstream or contribution labels [Oct 31, 2025](https://github.com/llm-d/llm-d/pull/421#event-20635718745)
[danehans](https://github.com/danehans) mentioned this pull request [ Nov 4, 2025 ](https://github.com/llm-d/llm-d/pull/421#ref-issue-3466173483)
[ agentgateway: test llm-d support kgateway-dev/kgateway#12451 ](https://github.com/kgateway-dev/kgateway/issues/12451)
Closed 
[danehans](https://github.com/danehans) changed the title ~~Updates kgateway for v2.1.1 Release~~ Updates kgateway for v2.1.x Release [Nov 4, 2025](https://github.com/llm-d/llm-d/pull/421#event-20714195069)
### 
commented [Nov 19, 2025](https://github.com/llm-d/llm-d/pull/421#issuecomment-3553900411)
Copy link  Copy Markdown 
Collaborator Author  
|  [@Gregory-Pereira](https://github.com/Gregory-Pereira) friendly ping. If you are waiting on an llm-d release before reviewing, is 12/4 still the ETA for the release?  |  
| --- |  
### 
commented [Jan 27, 2026](https://github.com/llm-d/llm-d/pull/421#issuecomment-3807426715)
Copy link  Copy Markdown 
Member  
|  [@Gregory-Pereira](https://github.com/Gregory-Pereira) we might have missed this in the 0.4 release - too late to get included in 0.5? [@danehans](https://github.com/danehans) can you give this a rebase, sorry for the delays.  |  
| --- |  
[petecheslock](https://github.com/petecheslock) added the [ needs-rebase ](https://github.com/llm-d/llm-d/issues?q=state%3Aopen%20label%3Aneeds-rebase) label [Jan 27, 2026](https://github.com/llm-d/llm-d/pull/421#event-22328022141)
### 
commented [Jan 28, 2026](https://github.com/llm-d/llm-d/pull/421#issuecomment-3808242863) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).
Copy link  Copy Markdown 
Member  
|  Most of the support is there - just not taking the gateway API 1.4 and a few more things. I started testing last night and hit some bugs. We need to add `OmitDefaultSecurityContext` as spec level entry to replace floatingUserId (opt in for OCP). we also need to change the default listener I think, to 8080 instead of 80 as a protected port, but not sure if this is OCP specific or kgateway specific. I dont have cycles to fix this now  |  
| --- |  
### 
commented [Jan 28, 2026](https://github.com/llm-d/llm-d/pull/421#issuecomment-3812706696)
Copy link  Copy Markdown 
Collaborator Author  
|  [@petecheslock](https://github.com/petecheslock) thanks for the ping. Before rebasing, is it possible to get [#399](https://github.com/llm-d/llm-d/issues/399) included in an upcoming release?  |  
| --- |  
### 
commented [Feb 26, 2026](https://github.com/llm-d/llm-d/pull/421#issuecomment-3969659101)
Copy link  Copy Markdown 
Collaborator  
|  Hey all, I did a quick look through here. All this seems reasonable to include for next release imo.  |  
| --- |  
This was referenced Mar 10, 2026
[ Migrate kgateway Support to agentgateway llm-d-incubation/llm-d-infra#269 ](https://github.com/llm-d-incubation/llm-d-infra/issues/269)
Closed 
[ [CI] Refresh stale EC2 Wide EP workflow to match current wide-ep-lws guide #964 ](https://github.com/llm-d/llm-d/issues/964)
Closed 
[danehans](https://github.com/danehans) [force-pushed](https://github.com/llm-d/llm-d/compare/5eb2b388404ca8f2384f785060ebcd4f7d8de634..f07a5f594de6011b4067a2dacd7af1c940a5f7d1) the  issue_399 branch from [`5eb2b38`](https://github.com/llm-d/llm-d/commit/5eb2b388404ca8f2384f785060ebcd4f7d8de634) to [`f07a5f5`](https://github.com/llm-d/llm-d/commit/f07a5f594de6011b4067a2dacd7af1c940a5f7d1) [ Compare ](https://github.com/llm-d/llm-d/compare/5eb2b388404ca8f2384f785060ebcd4f7d8de634..f07a5f594de6011b4067a2dacd7af1c940a5f7d1) [March 13, 2026 00:24](https://github.com/llm-d/llm-d/pull/421#event-23528173417)
[danehans](https://github.com/danehans) changed the title ~~Updates kgateway for v2.1.x Release~~ Refresh gateway provider support for kgateway v2.2.1 and agentgateway v1.0.0-alpha.4 [Mar 13, 2026](https://github.com/llm-d/llm-d/pull/421#event-23530971274)
[danehans](https://github.com/danehans) changed the title ~~Refresh gateway provider support for kgateway v2.2.1 and agentgateway v1.0.0-alpha.4~~ Refresh support for kgateway and agentgateway [Mar 13, 2026](https://github.com/llm-d/llm-d/pull/421#event-23530994086)
[danehans](https://github.com/danehans) [force-pushed](https://github.com/llm-d/llm-d/compare/38cd9093ccdb963b1e67b5e4b9ed8cc645d3201f..89f6e468e029acf39ddcbd49df6a6664666b33e4) the  issue_399 branch 2 times, most recently from [`38cd909`](https://github.com/llm-d/llm-d/commit/38cd9093ccdb963b1e67b5e4b9ed8cc645d3201f) to [`89f6e46`](https://github.com/llm-d/llm-d/commit/89f6e468e029acf39ddcbd49df6a6664666b33e4) [ Compare ](https://github.com/llm-d/llm-d/compare/38cd9093ccdb963b1e67b5e4b9ed8cc645d3201f..89f6e468e029acf39ddcbd49df6a6664666b33e4) [March 13, 2026 03:29](https://github.com/llm-d/llm-d/pull/421#event-23531600540)
### 
commented [Mar 13, 2026](https://github.com/llm-d/llm-d/pull/421#issuecomment-4052328439)
Copy link  Copy Markdown 
Collaborator Author  
|  Should kgateway be replaced with agentgateway instead?  |  
| --- |  
[danehans](https://github.com/danehans) [force-pushed](https://github.com/llm-d/llm-d/compare/9e26be34cd607041bf6ba0afe79a08426a3967ad..609297dcf85c15ac344b5ce664cc71ce18a836ce) the  issue_399 branch 7 times, most recently from [`9e26be3`](https://github.com/llm-d/llm-d/commit/9e26be34cd607041bf6ba0afe79a08426a3967ad) to [`609297d`](https://github.com/llm-d/llm-d/commit/609297dcf85c15ac344b5ce664cc71ce18a836ce) [ Compare ](https://github.com/llm-d/llm-d/compare/9e26be34cd607041bf6ba0afe79a08426a3967ad..609297dcf85c15ac344b5ce664cc71ce18a836ce) [March 13, 2026 17:16](https://github.com/llm-d/llm-d/pull/421#event-23554122740)
[danehans](https://github.com/danehans) mentioned this pull request [ Mar 13, 2026 ](https://github.com/llm-d/llm-d/pull/421#ref-pullrequest-4062188663)
[ Add agentgateway provider support to the chart llm-d-incubation/llm-d-infra#272 ](https://github.com/llm-d-incubation/llm-d-infra/pull/272)
Merged 
reviewed  [ Mar 13, 2026 ](https://github.com/llm-d/llm-d/pull/421#pullrequestreview-3947192210)
[ View reviewed changes ](https://github.com/llm-d/llm-d/pull/421/files)
Comment thread [.github/workflows/e2e-simulated-accelerators-test.yaml](https://github.com/llm-d/llm-d/pull/421/files#diff-84f077a2a676949481ad143bafd9e6992d642bf832dbf91a9c3b63b0cf05bc01) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).
reviewed  [ Mar 13, 2026 ](https://github.com/llm-d/llm-d/pull/421#pullrequestreview-3947201812)
[ View reviewed changes ](https://github.com/llm-d/llm-d/pull/421/files)
Comment thread [.github/workflows/e2e-simulated-accelerators-test.yaml](https://github.com/llm-d/llm-d/pull/421/files#diff-84f077a2a676949481ad143bafd9e6992d642bf832dbf91a9c3b63b0cf05bc01) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).
### 
commented [Mar 20, 2026](https://github.com/llm-d/llm-d/pull/421#issuecomment-4099881226)
Copy link  Copy Markdown 
Collaborator  
|  GIE 1.4.0 is released 🎉  |  
| --- |  
### 
commented [Mar 20, 2026](https://github.com/llm-d/llm-d/pull/421#issuecomment-4101049595)
Copy link  Copy Markdown 
Collaborator Author  
|  GAIE v1.4.0 release ref: <https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v1.4.0>  |  
| --- |  
requested changes  [ Mar 21, 2026 ](https://github.com/llm-d/llm-d/pull/421#pullrequestreview-3985271497)
[ View reviewed changes ](https://github.com/llm-d/llm-d/pull/421/files)
### 
left a comment 
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Looks good. My only real comment is around the overrides for `infra` chart and the overrides for `infernece-scheduler` images. I am willing to hear arguments against this but my idea is that we want to keep the `helmfile`s as simple as possible - and so adding support for overrides via ENV var is not the solution here. `helmfile` supports most of the helm ecosystem so you can still do a `helmfile apply --set  <whatever you want>` to control overriding the images. As our complexity grows our helmfiles will to I just want to delay hitting that issue as long as possible.
Conceptually though this is ready to go with a few minor changes + waiting on 272
Comment thread [guides/inference-scheduling/helmfile.yaml.gotmpl](https://github.com/llm-d/llm-d/pull/421/files#diff-cb196b471133ad0c820b8e3790123ceefb5726318798fea87707c171b5af0965) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).
Comment thread [guides/inference-scheduling/helmfile.yaml.gotmpl](https://github.com/llm-d/llm-d/pull/421/files#diff-cb196b471133ad0c820b8e3790123ceefb5726318798fea87707c171b5af0965) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).
Comment thread [guides/inference-scheduling/helmfile.yaml.gotmpl](https://github.com/llm-d/llm-d/pull/421/files#diff-cb196b471133ad0c820b8e3790123ceefb5726318798fea87707c171b5af0965) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).
Comment thread [guides/pd-disaggregation/helmfile.yaml.gotmpl](https://github.com/llm-d/llm-d/pull/421/files#diff-43894c3f03262a0a0561775a1bafd17d4f83332d7c6b86cad1b207fc794ba5dc) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).
Comment thread [guides/pd-disaggregation/helmfile.yaml.gotmpl](https://github.com/llm-d/llm-d/pull/421/files#diff-43894c3f03262a0a0561775a1bafd17d4f83332d7c6b86cad1b207fc794ba5dc) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).
Comment thread [guides/simulated-accelerators/README.md](https://github.com/llm-d/llm-d/pull/421/files#diff-bd4f3972a4e4ffb129042375767a069cef768c0c966fda99566774a29e74be43) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).
Comment thread [guides/workload-autoscaling/helmfile.yaml.gotmpl](https://github.com/llm-d/llm-d/pull/421/files#diff-d8199b72d2ab6fc1d5663cd9a1ab97c5d4793a7f145843076c8309c3fecd06db) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).
Comment thread [guides/workload-autoscaling/helmfile.yaml.gotmpl](https://github.com/llm-d/llm-d/pull/421/files#diff-d8199b72d2ab6fc1d5663cd9a1ab97c5d4793a7f145843076c8309c3fecd06db) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).
Comment thread [guides/workload-autoscaling/helmfile.yaml.gotmpl](https://github.com/llm-d/llm-d/pull/421/files#diff-d8199b72d2ab6fc1d5663cd9a1ab97c5d4793a7f145843076c8309c3fecd06db) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).
Comment thread [guides/workload-autoscaling/helmfile.yaml.gotmpl](https://github.com/llm-d/llm-d/pull/421/files#diff-d8199b72d2ab6fc1d5663cd9a1ab97c5d4793a7f145843076c8309c3fecd06db) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).
reviewed  [ Mar 21, 2026 ](https://github.com/llm-d/llm-d/pull/421#pullrequestreview-3986221562)
[ View reviewed changes ](https://github.com/llm-d/llm-d/pull/421/files)
Comment thread [guides/prereq/gateway-provider/README.md](https://github.com/llm-d/llm-d/pull/421/files#diff-24d738ef4f984f98ba50451f4c911b9fd2ff0994fb61ed173d2d1740745d7d06)
### 
[Mar 21, 2026](https://github.com/llm-d/llm-d/pull/421#discussion_r2969789996)
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Lets also bump istio to `1.29.1` (cc [@diegocastanibm](https://github.com/diegocastanibm) and [@maugustosilva](https://github.com/maugustosilva) )
[danehans](https://github.com/danehans) added 4 commits [March 21, 2026 15:40](https://github.com/llm-d/llm-d/pull/421#commits-pushed-a7d2e46)
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[danehans](https://github.com/danehans) Daneyon Hansen 
GPG key ID: A9BCCF56D7755933
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Update gateway-provider assets and guides for the current\nkgateway v2.2.1 compatibility path and agentgateway\nv1.0.0-alpha.4.\n\nPrefer agentgateway, keep kgateway deprecated, add\nOpenShift-specific gateway recipes, and trim workflow\nchanges to the required support updates.\n\nFixes: llm-d#399[](https://github.com/llm-d/llm-d/issues/399)

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

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

` Use GAIE v1.4.0 and fix CI regressions[](https://github.com/llm-d/llm-d/pull/421/commits/7702e637a181cbac66e35e8290797d39671c5799 "Use GAIE v1.4.0 and fix CI regressions

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

[danehans](https://github.com/danehans) mentioned this pull request [ Mar 21, 2026 ](https://github.com/llm-d/llm-d/pull/421#ref-issue-4114104309)
[ Local override workflow for guide-based gateway validation #992 ](https://github.com/llm-d/llm-d/issues/992)
Closed 
` Simplify guide helmfiles and bump Istio ([](https://github.com/llm-d/llm-d/pull/421/commits/38daa58d5ac77ebf9e58a0f14a11c4f50f9f7ef6 "Simplify guide helmfiles and bump Istio \(#992\)

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>")llm-d#992[](https://github.com/llm-d/llm-d/issues/992))[](https://github.com/llm-d/llm-d/pull/421/commits/38daa58d5ac77ebf9e58a0f14a11c4f50f9f7ef6 "Simplify guide helmfiles and bump Istio \(#992\)

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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).

```
Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

[danehans](https://github.com/danehans) [force-pushed](https://github.com/llm-d/llm-d/compare/7714c4560a5155f6896e89d0d17cb0b019ef32b5..38daa58d5ac77ebf9e58a0f14a11c4f50f9f7ef6) the  issue_399 branch from [`7714c45`](https://github.com/llm-d/llm-d/commit/7714c4560a5155f6896e89d0d17cb0b019ef32b5) to [`38daa58`](https://github.com/llm-d/llm-d/commit/38daa58d5ac77ebf9e58a0f14a11c4f50f9f7ef6) [ Compare ](https://github.com/llm-d/llm-d/compare/7714c4560a5155f6896e89d0d17cb0b019ef32b5..38daa58d5ac77ebf9e58a0f14a11c4f50f9f7ef6) [March 21, 2026 23:00](https://github.com/llm-d/llm-d/pull/421#event-23783932319)
### 
commented [Mar 21, 2026](https://github.com/llm-d/llm-d/pull/421#issuecomment-4104741200)
Copy link  Copy Markdown 
Collaborator Author  
|  Updated this branch to address Gregory's latest review comments:
  * removed the guide helmfile env-var override hooks for local chart/image swapping
  * removed the matching simulated-accelerators README override instructions
  * bumped Istio from `1.28.1` to `1.29.1`
  * opened [Local override workflow for guide-based gateway validation #992](https://github.com/llm-d/llm-d/issues/992) to track a supported local-development workflow for unreleased chart/image validation without adding override hooks to the published guides

One remaining dependency note: the associated `llm-d-infra` chart changes from [llm-d-incubation/llm-d-infra#272](https://github.com/llm-d-incubation/llm-d-infra/pull/272) are still not published as `llm-d-infra v1.4.0` in the chart repo yet, so the guide chart pins in this PR are still on the currently published release for now. I'll keep driving CI on this branch and will continue updating it based on failures.  |  
| --- |  
[Gregory-Pereira](https://github.com/Gregory-Pereira) mentioned this pull request [ Mar 21, 2026 ](https://github.com/llm-d/llm-d/pull/421#ref-pullrequest-4003978836)
[ lora-serving #868 ](https://github.com/llm-d/llm-d/pull/868)
Closed 
### 
commented [Mar 21, 2026](https://github.com/llm-d/llm-d/pull/421#issuecomment-4104852736)
Copy link  Copy Markdown 
Collaborator Author  
|  Follow-up status:
  * all CI checks are now green on `38daa58`
  * reran local kind simulator validation against this branch plus the current `llm-d-infra#272` chart state (`3259ffb`)
  * validation passed with `agentgateway`, GAIE `v1.4.0`, Gateway API `v1.5.1`, and the simulator backend
  * smoke tests passed for `/v1/models`, `/v1/completions`, and `/v1/chat/completions`

I also removed the temporary local override hooks from the published guide helmfiles and tracked that local-dev use case separately in [#992](https://github.com/llm-d/llm-d/issues/992).  |  
| --- |  
[danehans](https://github.com/danehans) requested review from [Gregory-Pereira](https://github.com/Gregory-Pereira) and [nirrozenbaum](https://github.com/nirrozenbaum) [March 22, 2026 02:21](https://github.com/llm-d/llm-d/pull/421#event-23785747045)
### 
commented [Mar 22, 2026](https://github.com/llm-d/llm-d/pull/421#issuecomment-4105149480)
Copy link  Copy Markdown 
Member  
|  I merged [llm-d-incubation/llm-d-infra#272](https://github.com/llm-d-incubation/llm-d-infra/pull/272) so we can go ahead and pickup the v1.4.0 chart bump and this looks good to merge from my perspective  |  
| --- |  
### 
commented [Mar 22, 2026](https://github.com/llm-d/llm-d/pull/421#issuecomment-4105152281)
Copy link  Copy Markdown 
Collaborator Author  
|  [@Gregory-Pereira](https://github.com/Gregory-Pereira) great to hear and thanks again for the reviews!  |  
| --- |  
[danehans](https://github.com/danehans) added 2 commits [March 22, 2026 00:21](https://github.com/llm-d/llm-d/pull/421#commits-pushed-9a3c404)
` Bump llm-d-infra chart pins to v1.4.0[](https://github.com/llm-d/llm-d/pull/421/commits/9a3c4044d9493c9f17f2a0044cf6537627176c19 "Bump llm-d-infra chart pins to v1.4.0

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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).

```
Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

` Fix stale gateway docs links[](https://github.com/llm-d/llm-d/pull/421/commits/a4f1187b6c39a61e0be112638d290d80784e3bdc "Fix stale gateway docs links

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
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).

```
Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

approved these changes  [ Mar 22, 2026 ](https://github.com/llm-d/llm-d/pull/421#pullrequestreview-3987875099)
[ View reviewed changes ](https://github.com/llm-d/llm-d/pull/421/files/a4f1187b6c39a61e0be112638d290d80784e3bdc)
### 
left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
/lgtm
approved these changes  [ Mar 22, 2026 ](https://github.com/llm-d/llm-d/pull/421#pullrequestreview-3988167220)
[ View reviewed changes ](https://github.com/llm-d/llm-d/pull/421/files/a4f1187b6c39a61e0be112638d290d80784e3bdc)
### 
left a comment 
Copy link  Copy Markdown 
Member
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
/lgtm
Hide details View details
[Gregory-Pereira](https://github.com/Gregory-Pereira) merged commit [`a576104`](https://github.com/llm-d/llm-d/commit/a57610457d9f5ae193b9683e7a4ead03724cc076) into  llm-d:main [Mar 22, 2026](https://github.com/llm-d/llm-d/pull/421#event-23791344987)
27 checks passed 
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/pull/421).
### 
commented [Mar 22, 2026](https://github.com/llm-d/llm-d/pull/421#issuecomment-4106307094)
Copy link  Copy Markdown 
Member  
|  Thanks for continuing to drive this [@danehans](https://github.com/danehans), awesome work here  |  
| --- |  
This was referenced Mar 22, 2026
[ fix(gateway): Add tmp volume to Envoy gateway for readOnlyRootFilesystem #869 ](https://github.com/llm-d/llm-d/pull/869)
Closed 
[ updating common configurations with infra refactor 272 #997 ](https://github.com/llm-d/llm-d/pull/997)
Merged 
[danehans](https://github.com/danehans) mentioned this pull request [ Apr 14, 2026 ](https://github.com/llm-d/llm-d/pull/421#ref-issue-4002586151)
[ [Upstream Breaking Change] Gateway API v1.4.0 → v1.5.0 #862 ](https://github.com/llm-d/llm-d/issues/862)
Closed 
[danehans](https://github.com/danehans) mentioned this pull request [ Apr 22, 2026 ](https://github.com/llm-d/llm-d/pull/421#ref-issue-4300403654)
[ [Upstream Update] kgateway (deprecated llm-d install path) v2.2.1 -> v2.2.3 #1218 ](https://github.com/llm-d/llm-d/issues/1218)
Closed 
[diegocastanibm](https://github.com/diegocastanibm) mentioned this pull request [ Apr 24, 2026 ](https://github.com/llm-d/llm-d/pull/421#ref-issue-4298065909)
[ [Release-prep]: v0.7.0 release prep #1209 ](https://github.com/llm-d/llm-d/issues/1209)
Closed 
[oshritf](https://github.com/oshritf) pushed a commit to oshritf/llm-d that referenced this pull request [ Jun 1, 2026 ](https://github.com/llm-d/llm-d/pull/421#ref-commit-7362c68)
` Refresh support for kgateway agentgateway GIE and GW API ([](https://github.com/oshritf/llm-d/commit/7362c68ec95f48b46429c715b66b3a665089a4f4 "Refresh support for kgateway agentgateway GIE and GW API \(#421\)

* Refresh gateway provider support

Update gateway-provider assets and guides for the current\\nkgateway v2.2.1 compatibility path and agentgateway\\nv1.0.0-alpha.4.\\n\\nPrefer agentgateway, keep kgateway deprecated, add\\nOpenShift-specific gateway recipes, and trim workflow\\nchanges to the required support updates.\\n\\nFixes: #399

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

* Bump GAIE to v1.4.0-rc.3

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

* Bump agentgateway to v1.0.0

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

* Use GAIE v1.4.0 and fix CI regressions

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

* Simplify guide helmfiles and bump Istio \(#992\)

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

* Bump llm-d-infra chart pins to v1.4.0

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

* Fix stale gateway docs links

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

---------

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>")llm-d#421[](https://github.com/llm-d/llm-d/pull/421))[](https://github.com/oshritf/llm-d/commit/7362c68ec95f48b46429c715b66b3a665089a4f4 "Refresh support for kgateway agentgateway GIE and GW API \(#421\)

* Refresh gateway provider support

Update gateway-provider assets and guides for the current\\nkgateway v2.2.1 compatibility path and agentgateway\\nv1.0.0-alpha.4.\\n\\nPrefer agentgateway, keep kgateway deprecated, add\\nOpenShift-specific gateway recipes, and trim workflow\\nchanges to the required support updates.\\n\\nFixes: #399

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

* Bump GAIE to v1.4.0-rc.3

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

* Bump agentgateway to v1.0.0

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

* Use GAIE v1.4.0 and fix CI regressions

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

* Simplify guide helmfiles and bump Istio \(#992\)

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

* Bump llm-d-infra chart pins to v1.4.0

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

* Fix stale gateway docs links

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

---------

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[oshfeder](https://github.com/oshfeder) Oshrit Feder 
GPG key ID: BFAC03724B68C8B5
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
* Refresh gateway provider support

Update gateway-provider assets and guides for the current\nkgateway v2.2.1 compatibility path and agentgateway\nv1.0.0-alpha.4.\n\nPrefer agentgateway, keep kgateway deprecated, add\nOpenShift-specific gateway recipes, and trim workflow\nchanges to the required support updates.\n\nFixes: llm-d#399[](https://github.com/llm-d/llm-d/issues/399)

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

* Bump GAIE to v1.4.0-rc.3

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

* Bump agentgateway to v1.0.0

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

* Use GAIE v1.4.0 and fix CI regressions

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

* Simplify guide helmfiles and bump Istio (llm-d#992[](https://github.com/llm-d/llm-d/issues/992))

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

* Bump llm-d-infra chart pins to v1.4.0

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

* Fix stale gateway docs links

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>

---------

Signed-off-by: Daneyon Hansen <daneyon.hansen@solo.io>
```

[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fllm-d%2Fllm-d%2Fpull%2F421)
###  Labels 
[ gateway-provider ](https://github.com/llm-d/llm-d/issues?q=state%3Aopen%20label%3Agateway-provider) Features or issues related to gateway provider changes [ needs-maintainer-triage ](https://github.com/llm-d/llm-d/issues?q=state%3Aopen%20label%3Aneeds-maintainer-triage) Waiting on consensus from maintainers to pickup and accept this workstream or contribution [ needs-rebase ](https://github.com/llm-d/llm-d/issues?q=state%3Aopen%20label%3Aneeds-rebase)
###  6 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
