---
title: '[Feature]: Support persistent tags on User entity with automatic propagation
  to SpendLogs · Issue #25069 · BerriAI/litellm'
id: feature-support-persistent-tags-on-user-entity-with-automatic-propagation-to-spe
tags:
- internal-llm-inference-stacks-7f0c90
- locus-oss-boundary-sharpening
- litellm
- open-core
- monetization-boundary
created: '2026-10-10T00:08:05.078954Z'
updated: '2026-10-10T14:52:17.269289Z'
source: https://github.com/BerriAI/litellm/issues/25069
source_domain: github.com
fetched_at: '2026-10-10T00:08:05.056965Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'LiteLLM issue #25069: a feature request that accepted the Enterprise gate
  on tag-based budgets and asked only for an adjacent open-source field, closed ''not
  planned'' with zero maintainer engagement — the load-bearing evidence that this
  boundary is a deliberate monetization choice rather than a capability gap.'
---

[Skip to content](https://github.com/BerriAI/litellm/issues/25069#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/BerriAI/litellm/issues/25069) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/BerriAI/litellm/issues/25069) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/BerriAI/litellm/issues/25069) to refresh your session. Dismiss alert
#  [Feature]: Support persistent tags on User entity with automatic propagation to SpendLogs #25069
Copy link
Copy link
Closed as not planned
Closed as not planned
[[Feature]: Support persistent tags on User entity with automatic propagation to SpendLogs](https://github.com/BerriAI/litellm/issues/25069#top)#25069
Copy link
Labels
## Description
opened [on Apr 3, 2026](https://github.com/BerriAI/litellm/issues/25069#issue-4199785725)
Issue body actions
## Feature Description
Currently, LiteLLM supports tags at the **request level** (`request_tags`), **virtual key level** (`metadata.tags`), and **HTTP header level** (`extra_spend_tag_headers`). However, there is no way to assign **persistent tags directly to a User entity** that automatically propagate to SpendLogs on every request made by that user.
## Use Case
In enterprise environments, a single Team can be composed of users belonging to **different consulting companies or vendors**. Today there is no native way to track spending per supplier within the same team.
Adding a `tags` field on the User entity would allow cost attribution at the user level, enabling organizations to split costs by vendor/supplier without restructuring their team hierarchy.
## Proposed Solution
Add a `tags` field to the **User entity** (`LiteLLM_UserTable`) that:
  1. Can be set via `/user/new` and `/user/update` endpoints
  2. Is automatically merged into `request_tags` on every request made by that user
  3. Appears in `SpendLogs.request_tags` alongside any request-level or key-level tags
  4. Propagates to OTEL span attributes (like existing `request_tags` do)


## Why not use existing workarounds?  
| Approach  | Limitation  |  
| --- | --- |  
|  `request_tags` in body  | Requires client-side changes on every request  |  
|  `metadata.tags` on key  | Couples cost attribution to key management; one user may have multiple keys  |  
| `extra_spend_tag_headers`  | Requires infrastructure/gateway changes to inject headers  |  
| **User-level`tags` (proposed)**  | **Centralized, persistent, no client/infra changes needed**  |  
## Additional Context
  * This would complement the existing tag-based budget enforcement (`tag_budget_config`) by enabling automatic tag application without client cooperation
  * Enterprise feature is acceptable


## Activity
  1. ### github-actions commented on Jul 3, 2026 
[on Jul 3, 2026](https://github.com/BerriAI/litellm/issues/25069#issuecomment-4871511045) – with [GitHub Actions](https://help.github.com/en/actions)
Contributor
More actions
This issue has been automatically marked as stale because it has not had recent activity. It will be closed if no further activity occurs.
  2. [github-actions](https://github.com/apps/github-actions)
added [on Jul 3, 2026](https://github.com/BerriAI/litellm/issues/25069#event-27504905899)
  3. [github-actions](https://github.com/apps/github-actions)
closed this as [not planned](https://github.com/BerriAI/litellm/issues?q=is%3Aissue%20state%3Aclosed%20archived%3Afalse%20reason%3Anot-planned)[on Jul 10, 2026](https://github.com/BerriAI/litellm/issues/25069#event-27791016381)
  4. ### aqeeb-warraich commented on Sep 23, 2026 
More actions
Tagging at the user level to split cost by vendor without restructuring the team hierarchy is a good instinct team trees almost never match how finance actually wants to slice it. We built attribution to be dimension-agnostic for that reason; worked example: <https://unitsense.ai/blog/smart-routing-pilot/>


[Sign up for free](https://github.com/signup?return_to=https://github.com/BerriAI/litellm/issues/25069)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/BerriAI/litellm/issues/25069)
## Metadata
## Metadata
### Assignees
No one assigned
### Labels
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
