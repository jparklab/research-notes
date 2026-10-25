---
title: 'Support fail-open fallback for standalone EPP inference routing · Issue #1661
  · agentgateway/agentgateway'
id: support-fail-open-fallback-for-standalone-epp-inference-routing-issue-1661-agent
tags:
- llmd-router-nonk8s-backends
- agentgateway
- fail-open
- standalone-mode
- epp
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:43:55.676800Z'
updated: '2026-09-15T04:55:23.699065Z'
source: https://github.com/agentgateway/agentgateway/issues/1661
source_domain: github.com
fetched_at: '2026-09-11T19:43:55.655948Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Open agentgateway feature issue (#1661, opened Apr 24 2026) tracking fail-open
  fallback for standalone-EPP inference routing. Confirms PR #1658 made agentgateway
  trust the EPP-selected destination and forward directly to the returned ip:port,
  but deliberately kept standalone routing FAIL-CLOSED: if the local EPP is unavailable
  or omits the ''x-gateway-destination-endpoint'' header, agentgateway errors instead
  of falling back to normal service load-balancing. Maintainer howardjohn objects
  that a fail-open path has no endpoints to route to since users don''t configure
  them in passthrough mode. Contributor steverogersX proposes fail-open only activates
  if the user explicitly declares a fallback pool (EPP down + no fallback pool = same
  error as fail-closed; EPP down + fallback pool = route through fallback). As of
  the last comment (Jun 22 2026) danehans says a real FailOpen design needs either
  configured/tracked fallback endpoints, a non-passthrough mode with service endpoint
  discovery, or separate virtual-model failover — and is soliciting standalone-scheduler
  user feedback before proceeding. Unresolved / unmerged as of fetch.'
utility_score: 13.0
---

[Skip to content](https://github.com/agentgateway/agentgateway/issues/1661#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/agentgateway/agentgateway/issues/1661) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/agentgateway/agentgateway/issues/1661) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/agentgateway/agentgateway/issues/1661) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/agentgateway/agentgateway/issues/1661).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fagentgateway%2Fagentgateway) You must be signed in to change notification settings
  * [ Fork 829 ](https://github.com/login?return_to=%2Fagentgateway%2Fagentgateway)
  * [ Star  4.8k ](https://github.com/login?return_to=%2Fagentgateway%2Fagentgateway)


#  Support fail-open fallback for standalone EPP inference routing
Copy link
Copy link
[Support fail-open fallback for standalone EPP inference routing](https://github.com/agentgateway/agentgateway/issues/1661#top)#1661
Feature
Copy link
## Description
opened [on Apr 24, 2026](https://github.com/agentgateway/agentgateway/issues/1661#issue-4324534392)
Last edited by danehans
Collaborator
Issue body actions
## Background
PR [#1658](https://github.com/agentgateway/agentgateway/pull/1658) updates standalone EPP inference routing so agentgateway trusts the destination selected by EPP and forwards directly to the returned `ip:port`. That PR intentionally keeps standalone inference routing fail-closed. If EPP fails or does not return `x-gateway-destination-endpoint`, agentgateway returns an error instead of routing directly to service endpoints.
## Requirement
Track a future fail-open/fallback mode for standalone EPP + agentgateway deployments.
When enabled, if the local EPP is unavailable or does not return a destination, agentgateway should be able to fall-back to routing through the configured service backend endpoints using the normal service load-balancing path.
## Activity
### danehans commented on Apr 24, 2026 
CollaboratorAuthor
More actions
xref [#1658 (comment)](https://github.com/agentgateway/agentgateway/pull/1658#discussion_r3139331124) to adding `handle` to track endpoint health for failOpen.
### steverogersX commented on Apr 29, 2026 
More actions
hi, [@danehans](https://github.com/danehans) i'd love to work on this. can i pick this up?
[howardjohn](https://github.com/howardjohn)
added the
issue type [on May 8, 2026](https://github.com/agentgateway/agentgateway/issues/1661#event-25319971484)
### howardjohn commented on May 8, 2026 
Collaborator
More actions
How will this work since we don't have any endpoints to route to? the user will not put them in their cfg
### steverogersX commented on Jun 9, 2026 
More actions
for passthrough mode, failOpen only does something useful if the user explicitly declares a fallback pool. if no fallback pool is configured, it behaves the same as failClosed just errors. that way users who want resilience can opt in by declaring a fallback pool, and everyone else gets the same simple behavior as today.
EPP down + no fallback pool → error (same as failClosed) EPP down + fallback pool → route through fallback pool this also directly addresses the "user won't put endpoints in their cfg" concern they only need to if they want resilience.
### danehans commented on Jun 22, 2026 
CollaboratorAuthor
More actions
> How will this work since we don't have any endpoints to route to? the user will not put them in their cfg
[@howardjohn](https://github.com/howardjohn) makes a good point here. I think this issue needs to be reframed. A real `FailOpen` design would need one of these:
  * Fallback service endpoints configured and tracked.
  * A non-passthrough mode that includes service endpoint discovery.
  * Separate backend failover, like [virtual model failover](https://agentgateway.dev/docs/standalone/main/llm/virtual-models/#failover-routing) (not the same as EPP fail-open).


I would like to get feedback from a [standalone request scheduler](https://agentgateway.dev/docs/standalone/main/inference/#standalone-request-scheduler-mode) user before procedding with this isue.
[Sign up for free](https://github.com/signup?return_to=https://github.com/agentgateway/agentgateway/issues/1661)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/agentgateway/agentgateway/issues/1661)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
No labels
No labels
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
