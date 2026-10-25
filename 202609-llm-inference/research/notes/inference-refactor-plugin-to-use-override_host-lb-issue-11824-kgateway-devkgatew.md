---
title: 'Inference: Refactor Plugin to Use Override_Host LB · Issue #11824 · kgateway-dev/kgateway'
id: inference-refactor-plugin-to-use-override_host-lb-issue-11824-kgateway-devkgatew
tags:
- llmd-router-nonk8s-backends-54b774
- override-host
- agentgateway
- kgateway
- fail-open
created: '2026-09-11T21:04:03.372666Z'
updated: '2026-09-15T04:55:23.913260Z'
source: https://github.com/kgateway-dev/kgateway/issues/11824
source_domain: github.com
fetched_at: '2026-09-11T21:04:03.350094Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'kgateway issue #11824 (opened Jul 30 2025 by danehans, closed Sep 30 2025)
  documents kgateway''s Inference (GIE) plugin evolution: PR #11810 shipped EPP-fail-open
  conformance (''EppUnAvailableFailOpen'') using ''a static cluster + subset LB''
  as a workaround; once kgateway''s Envoy pin advanced to v1.35 (via PR #11784), which
  ships Envoy''s new override_host LB feature (linking directly to envoyproxy/envoy#38757),
  the plan was to refactor the plugin away from the subset-LB hack to use override_host
  LB. The issue was closed WITHOUT that refactor happening, with the closing comment
  stating plainly: ''Closing since the community decided to deprecate the Envoy-based
  data plane in favor of agentgateway for AI-related features.'' This is a highly
  load-bearing, dated (Sep 30 2025) data point: kgateway -- a production, GIE-integrated
  gateway -- abandoned Envoy as its AI/inference data plane entirely in favor of agentgateway,
  rather than adopting either dynamic_forwarding or override_host LB as its long-term
  mechanism for honoring EPP-picked destinations. Referenced cross-project context:
  Istio tracked a related override_host need at istio/istio#56230, and Envoy Gateway
  discussed it at envoyproxy/gateway#6458. Net effect for router-customization strategy:
  agentgateway''s own destinationMode/passthrough mechanism (not either Envoy LB approach)
  is the mechanism kgateway''s maintainers bet on going forward for GIE-integrated
  non-pod destination routing.'
---

*Suggested by [[dynamic-forwarding-load-balancing-policy-by-yanavlasov-pull-request-38757-envoyp]] — shows production gateway chose Override_Host LB over dynamic_forwarding policy*

[Skip to content](https://github.com/kgateway-dev/kgateway/issues/11824#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kgateway-dev/kgateway/issues/11824) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kgateway-dev/kgateway/issues/11824) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kgateway-dev/kgateway/issues/11824) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kgateway-dev/kgateway/issues/11824).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkgateway-dev%2Fkgateway) You must be signed in to change notification settings
  * [ Fork 807 ](https://github.com/login?return_to=%2Fkgateway-dev%2Fkgateway)
  * [ Star  5.7k ](https://github.com/login?return_to=%2Fkgateway-dev%2Fkgateway)


#  Inference: Refactor Plugin to Use Override_Host LB #11824
Copy link
Copy link
Closed
Closed
[Inference: Refactor Plugin to Use Override_Host LB](https://github.com/kgateway-dev/kgateway/issues/11824#top)#11824
Copy link
Labels
[Area: InferenceActivities related to Gateway API Inference Extension support.](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Area%3A%20Inference%22)Activities related to Gateway API Inference Extension support.
## Description
opened [on Jul 30, 2025](https://github.com/kgateway-dev/kgateway/issues/11824#issue-3277619400)
Contributor
Issue body actions
When [#11810](https://github.com/kgateway-dev/kgateway/pull/11810) merges, the Inference plugin will support the `EppUnAvailableFailOpen` conformance test. This PR uses a static cluster + subset LB to support EPP fail-open. Now that [#11784](https://github.com/kgateway-dev/kgateway/pull/11784) has been merged, Kgateway supports Envoy v1.35, which includes the [override_host LB](https://github.com/envoyproxy/envoy/pull/38757) feature. Refactor the plugin to use override_host LB.
**Refs** :
Istio: [istio/istio#56230](https://github.com/istio/istio/issues/56230) EG: [envoyproxy/gateway#6458 (comment)](https://github.com/envoyproxy/gateway/pull/6458#issuecomment-3060746376)
## Activity
[danehans](https://github.com/danehans)
added 
[Area: InferenceActivities related to Gateway API Inference Extension support.](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Area%3A%20Inference%22)Activities related to Gateway API Inference Extension support.
[on Jul 30, 2025](https://github.com/kgateway-dev/kgateway/issues/11824#event-18902554775)
### danehans commented on Sep 30, 2025 
ContributorAuthor
More actions
Closing since the community decided to deprecate the Envoy-based data plane in favor of agentgateway for AI-related features.
[danehans](https://github.com/danehans)
closed this as [completed](https://github.com/kgateway-dev/kgateway/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)[on Sep 30, 2025](https://github.com/kgateway-dev/kgateway/issues/11824#event-20016169604)
[Sign up for free](https://github.com/signup?return_to=https://github.com/kgateway-dev/kgateway/issues/11824)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/kgateway-dev/kgateway/issues/11824)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
[Area: InferenceActivities related to Gateway API Inference Extension support.](https://github.com/kgateway-dev/kgateway/issues?q=state%3Aopen%20label%3A%22Area%3A%20Inference%22)Activities related to Gateway API Inference Extension support.
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
