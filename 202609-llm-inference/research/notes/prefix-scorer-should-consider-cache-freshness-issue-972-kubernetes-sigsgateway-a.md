---
title: 'Prefix Scorer should consider cache freshness · Issue #972 · kubernetes-sigs/gateway-api-inference-extension'
id: prefix-scorer-should-consider-cache-freshness-issue-972-kubernetes-sigsgateway-a
tags:
- llmd-kv-cache-routing-c665d5
- prefix-cache-scorer
- maintainer-issue
created: '2026-09-14T13:03:01.912384Z'
updated: '2026-10-10T14:52:17.315080Z'
source: https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/972
source_domain: github.com
fetched_at: '2026-09-14T13:03:01.890974Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GIE GitHub issue #972 (opened Jun 2025 by a maintainer, closed Sep 2025
  as superseded by #1497): proposes that the prefix scorer weight cache hits by recency
  (freshest 80% of LRU insertion order counted as ''fresh'', older 20% discounted),
  since an old 101-block hit is less valuable than a recent 100-block hit. Thin, maintainer-internal
  feature request, not an independent evaluation and not evidence of hotspotting or
  underperformance vs. plain load balancing — closed without implementation in favor
  of a follow-on issue.'
---

[Skip to content](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/972#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/972) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/972) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/972) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/972).
/ **[gateway-api-inference-extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension) You must be signed in to change notification settings
  * [ Fork 313 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)
  * [ Star  767 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)


#  Prefix Scorer should consider cache freshness
Copy link
Copy link
Closed
Closed
[Prefix Scorer should consider cache freshness](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/972#top)#972
Copy link
Labels
[lifecycle/staleDenotes an issue or PR has remained open with no activity and has become stale.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22lifecycle%2Fstale%22)Denotes an issue or PR has remained open with no activity and has become stale.[needs-triageIndicates an issue or PR lacks a `triage/foo` label and requires one.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a `triage/foo` label and requires one.
## Description
opened [on Jun 12, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/972#issue-3140895491)
Contributor
Issue body actions
**What would you like to be added** :
The implementation can simply divide the cache freshness into 2 categories: cache entries in the most recent 80% is considered “fresh”, and the older 20% is considered “not fresh enough”.
More advanced strategies can be explored as well.
**Why is this needed** :
A cache hit on older entries will likely be less accurate than recent entries. The score can take into account the freshness of the cache. The freshness is determined by the order of insertion to the LRU cache, not by timestamp. For example, we will prefer a recent prefix cache hit of 100 blocks on pod1 over an old prefix cache hit of 101 blocks on pod2.
## Activity
[liu-cong](https://github.com/liu-cong)
added 
[needs-triageIndicates an issue or PR lacks a `triage/foo` label and requires one.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a `triage/foo` label and requires one.
[on Jun 12, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/972#event-18119100097)
### k8s-triage-robot commented on Sep 10, 2025 
More actions
The Kubernetes project currently lacks enough contributors to adequately respond to all issues.
This bot triages un-triaged issues according to the following rules:
  * After 90d of inactivity, `lifecycle/stale` is applied
  * After 30d of inactivity since `lifecycle/stale` was applied, `lifecycle/rotten` is applied
  * After 30d of inactivity since `lifecycle/rotten` was applied, the issue is closed


You can:
  * Mark this issue as fresh with `/remove-lifecycle stale`
  * Close this issue with `/close`
  * Offer to help out with [Issue Triage](https://www.kubernetes.dev/docs/guide/issue-triage/)


Please send feedback to sig-contributor-experience at [kubernetes/community](https://github.com/kubernetes/community).
/lifecycle stale
[k8s-ci-robot](https://github.com/k8s-ci-robot)
added 
[lifecycle/staleDenotes an issue or PR has remained open with no activity and has become stale.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22lifecycle%2Fstale%22)Denotes an issue or PR has remained open with no activity and has become stale.
[on Sep 10, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/972#event-19617333230)
### ahg-g commented on Sep 10, 2025 
Contributor
More actions
[@liu-cong](https://github.com/liu-cong) is this still relevant?
### nirrozenbaum commented on Sep 10, 2025 
Contributor
More actions
this will be covered in one of the recent issues that was opened [#1497](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1497), for which I'm planning to push a fix soon. I think we can close this one.
[ahg-g](https://github.com/ahg-g)
closed this as [completed](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)[on Sep 10, 2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/972#event-19617426066)
[Sign up for free](https://github.com/signup?return_to=https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/972)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/972)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
[lifecycle/staleDenotes an issue or PR has remained open with no activity and has become stale.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22lifecycle%2Fstale%22)Denotes an issue or PR has remained open with no activity and has become stale.[needs-triageIndicates an issue or PR lacks a `triage/foo` label and requires one.](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a `triage/foo` label and requires one.
No type
### Projects
No projects
### Milestone
No milestone
### Relationships
None yet
### Development
No branches or pull requests
### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
