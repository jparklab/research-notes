---
title: 'Endpoint metadata updates do not trigger endpoint notification events · Issue
  #1403 · llm-d/llm-d-router'
id: endpoint-metadata-updates-do-not-trigger-endpoint-notification-events-issue-1403
tags:
- llmd-router-nonk8s-backends-54b774
- endpoint-discovery
- datastore
- bug-fix
created: '2026-09-11T20:48:23.833563Z'
updated: '2026-09-12T03:37:37.672215Z'
source: https://github.com/llm-d/llm-d-router/issues/1403
source_domain: github.com
fetched_at: '2026-09-11T20:48:23.809595Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub issue #1403 (llm-d-router, opened May 30 2026, closed Jun 1 2026
  via PR #1404, shipped in v0.9.0): the datastore only dispatches the EventAddOrUpdate
  notification through endpoint-notification-source when an endpoint is FIRST added
  (via Runtime.NewEndpoint()); when an existing endpoint''s metadata changes (address,
  labels, rank, ports), the datastore calls Endpoint.UpdateMetadata(meta) but returns
  without dispatching any event. Plugins that subscribe to endpoint-notification-source
  — explicitly including precise-prefix-cache components that install/refresh per-endpoint
  KV-events (ZMQ) subscribers keyed on endpoint address/port — can silently hold stale
  state after such changes. Fix (merged): dispatch EventAddOrUpdate on metadata change
  too, skipping dispatch only when new metadata is identical. Directly relevant to
  a custom (non-k8s) EndpointDiscovery/datastore implementation: it must fire update
  notifications on metadata mutation, not just on add, or downstream plugins (esp.
  KV-cache-event subscribers) will not know an endpoint''s address/port changed.'
utility_score: 16.0
---

[Skip to content](https://github.com/llm-d/llm-d-router/issues/1403#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/1403) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/1403) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/1403) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/issues/1403).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


#  Endpoint metadata updates do not trigger endpoint notification events
Copy link
Copy link
Closed
Closed
[Endpoint metadata updates do not trigger endpoint notification events](https://github.com/llm-d/llm-d-router/issues/1403#top)#1403
Copy link
## Description
opened [on May 30, 2026](https://github.com/llm-d/llm-d-router/issues/1403#issue-4553565478)
Last edited by zhouyou9505
Member
Issue body actions
## Problem
Endpoint metadata updates are applied in the datastore, but existing endpoints do not emit an endpoint lifecycle notification.
Today, when an endpoint is first added, the datastore calls `Runtime.NewEndpoint()`, and the runtime dispatches `EventAddOrUpdate` through `endpoint-notification-source`. However, when the same endpoint already exists and only its metadata changes, the datastore only calls `Endpoint.UpdateMetadata(meta)` and returns without dispatching an event.
## Impact
Plugins that rely on `endpoint-notification-source` can miss endpoint updates. For example, precise prefix cache components use endpoint add/update events to install or refresh per-endpoint KV-events subscribers. If endpoint metadata such as address, labels, rank, or ports changes without a notification, plugin state can remain stale.
## Expected behavior
Existing endpoint metadata changes should dispatch `EventAddOrUpdate`, just like endpoint additions, so endpoint-aware plugins can refresh their per-endpoint state.
## Suggested fix
When the datastore sees an existing endpoint with changed metadata:
  * update the endpoint metadata
  * dispatch an endpoint update event through the runtime
  * avoid dispatching when the new metadata is identical to the current metadata


## Activity
[github-project-automation](https://github.com/apps/github-project-automation)
added this to [on May 30, 2026](https://github.com/llm-d/llm-d-router/issues/1403#event-26137936377)
[github-project-automation](https://github.com/apps/github-project-automation)
moved this to In review in [on May 30, 2026](https://github.com/llm-d/llm-d-router/issues/1403)
[github-actions](https://github.com/apps/github-actions)
closed this as [completed](https://github.com/llm-d/llm-d-router/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Acompleted)in [#1404](https://github.com/llm-d/llm-d-router/pull/1404)[on Jun 1, 2026](https://github.com/llm-d/llm-d-router/issues/1403#event-26186324757)
[github-project-automation](https://github.com/apps/github-project-automation)
moved this from In review to Done in [on Jun 1, 2026](https://github.com/llm-d/llm-d-router/issues/1403)
[Sign up for free](https://github.com/signup?return_to=https://github.com/llm-d/llm-d-router/issues/1403)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/llm-d/llm-d-router/issues/1403)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
No labels
No labels
No type
### Projects
  * [llm-d-router Closed](https://github.com/orgs/llm-d/projects/15)
StatusShow more project fields


Show 1 closed project
### Milestone
No milestone
### Relationships
None yet
### Development
  * [fix(datalayer): dispatch endpoint update notificationsllm-d/llm-d-router](https://github.com/llm-d/llm-d-router/pull/1404)[Releasev0.9.0](https://github.com/llm-d/llm-d-router/releases/tag/v0.9.0)


### Participants
## Issue actions
  * Open in GitHub Copilot app


You can’t perform that action at this time. 
