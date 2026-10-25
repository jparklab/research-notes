---
title: '[Framework] Define a panic-isolation posture for plugin extension points ·
  Issue #2105 · llm-d/llm-d-router'
id: framework-define-a-panic-isolation-posture-for-plugin-extension-points-issue-210
tags:
- llmd-router-nonk8s-backends-54b774
- locus-extension-mechanism
- maintainer-statement
- out-of-tree-plugin-support
created: '2026-09-11T21:25:16.208754Z'
updated: '2026-09-15T04:55:23.938012Z'
source: https://github.com/llm-d/llm-d-router/issues/2105
source_domain: github.com
fetched_at: '2026-09-11T21:25:16.184072Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'llm-d-router issue #2105 ''[Framework] Define a panic-isolation posture
  for plugin extension points'' (opened Jul 21 2026 by contributor LukeAVanDrie; label
  needs-triage; still open/unassigned/undecided as of fetch). Asks the maintainers
  to record a decision on how the EPP plugin framework should handle a panicking plugin,
  since ''a panicking plugin kills the EPP process from any extension point; nothing
  recovers plugin invocations anywhere in the framework'' (flow control''s dispatch
  goroutine holding queue mutexes is cited as the sharpest case, e.g. priorityqueue.go:76-78,
  eviction/queue.go:147, processor.go:371-383). THE LOAD-BEARING SENTENCE for router-customization/out-of-tree-plugin
  research is option 2 of the three proposed: ''Documented at-your-own-risk: in-tree
  plugins are the supported surface; out-of-tree plugins that panic take the process
  down, stated loudly in the plugin developer docs.'' This is the single clearest
  maintainer/contributor articulation found of llm-d-router''s plugin support boundary:
  it explicitly names ''out-of-tree plugins'' as a real, contemplated category distinct
  from in-tree ones, and frames in-tree as ''the supported surface'' while out-of-tree
  would be tolerated only on an undocumented-safety, at-your-own-risk basis if this
  option were adopted -- as of fetch time the issue is unresolved (needs-triage, no
  comments, no linked PR), so this is a PROPOSED framing under discussion, not a ratified
  project policy. Explicitly split out of issue #1187 (flow control''s own goroutine
  recover, which the author notes ''incidentally backstops plugin panics on those
  goroutines regardless of this decision'').'
---

*Suggested by [[issues-llm-dllm-d-router-github]] — search hit for out-of-tree plugin extension point terminology*

[Skip to content](https://github.com/llm-d/llm-d-router/issues/2105#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/2105) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/2105) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/2105) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/issues/2105).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


#  [Framework] Define a panic-isolation posture for plugin extension points
Copy link
Copy link
[[Framework] Define a panic-isolation posture for plugin extension points](https://github.com/llm-d/llm-d-router/issues/2105#top)#2105
Copy link
Labels
[needs-triageIndicates an issue or PR lacks a triage label and requires one.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a triage label and requires one.
## Description
opened [on Jul 21, 2026](https://github.com/llm-d/llm-d-router/issues/2105#issue-4934162295)
Contributor
Issue body actions
**What would you like to be added** :
A recorded decision (and, depending on the outcome, an implementation issue) for how the EPP plugin framework handles panicking plugins. Options:
  1. Guard-at-invocation: the framework wraps every plugin call in recover + metric (`plugin_panics_total{extension_point, plugin}`), failing the affected request/cycle. Cost: recovering with locks held risks continuing on corrupted state; per-call overhead.
  2. Documented at-your-own-risk: in-tree plugins are the supported surface; out-of-tree plugins that panic take the process down, stated loudly in the plugin developer docs.
  3. Hybrid: guard only extension points that execute on shared long-lived goroutines (flow control policies, producers), document the rest.


**Why is this needed** :
A panicking plugin kills the EPP process from any extension point; nothing recovers plugin invocations anywhere in the framework. Flow control makes the sharpest case -- policy plugins execute inside the single dispatch goroutine and inside heap operations while holding queue mutexes (`priorityqueue.go:76-78`, `eviction/queue.go:147`, `processor.go:371-383`) -- but the exposure is identical for scorers, filters, producers, and profile handlers. Deliberately framework scope, split out from [#1187](https://github.com/llm-d/llm-d-router/issues/1187) (flow control's own goroutine recover lands separately and incidentally backstops plugin panics on those goroutines regardless of this decision).
## Activity
[LukeAVanDrie](https://github.com/LukeAVanDrie)
added 
[needs-triageIndicates an issue or PR lacks a triage label and requires one.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a triage label and requires one.
[on Jul 20, 2026](https://github.com/llm-d/llm-d-router/issues/2105#event-28241565757)
[Sign up for free](https://github.com/signup?return_to=https://github.com/llm-d/llm-d-router/issues/2105)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/llm-d/llm-d-router/issues/2105)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
[needs-triageIndicates an issue or PR lacks a triage label and requires one.](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3A%22needs-triage%22)Indicates an issue or PR lacks a triage label and requires one.
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
