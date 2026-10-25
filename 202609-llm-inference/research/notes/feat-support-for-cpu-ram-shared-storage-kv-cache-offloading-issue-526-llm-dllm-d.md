---
title: 'feat: support for CPU RAM -> shared storage KV Cache offloading · Issue #526
  · llm-d/llm-d-kv-cache'
id: feat-support-for-cpu-ram-shared-storage-kv-cache-offloading-issue-526-llm-dllm-d
tags:
- llmd-kv-cache-routing-c665d5
- kv-cache-offloading
- cpu-only
- github-issue
created: '2026-09-14T11:53:39.553428Z'
updated: '2026-10-10T14:52:17.295042Z'
source: https://github.com/llm-d/llm-d-kv-cache/issues/526
source_domain: github.com
fetched_at: '2026-09-14T11:53:39.533255Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub issue #526 (opened Apr 16, 2026, llm-d/llm-d-kv-cache) is a feature
  request noting existing offloading-connector backends only support GPU->CPU RAM
  (via Offloading Connector or LMCache) and GPU->Storage (via llm-d FS Connector or
  LMCache) block transfer paths; nodes running vLLM on CPU-only cores (no GPU/CUDA)
  cannot use KV-cache offloading at all. Proposed extending support to a CPU-only
  deployment path: CPU memory -> shared storage offloading with no CUDA dependency,
  generalizing from the existing GPU->Primary(CPU)->Secondary tiers RFC pattern to
  CPU->Primary->Secondary. By Jul 23, 2026 a maintainer (dannyharnik) noted this appears
  now covered by vLLM''s native multi-tier offloading feature (docs.vllm.ai/en/latest/features/kv_offloading_usage/),
  consistent with the llm-d-kv-cache repo''s own migration note that llmd-fs-backend
  was upstreamed into vLLM''s TieringOffloadingSpec.'
utility_score: 12.0
---

[Skip to content](https://github.com/llm-d/llm-d-kv-cache/issues/526#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/issues/526) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/issues/526) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/issues/526) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-kv-cache/issues/526).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache) You must be signed in to change notification settings
  * [ Fork 156 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache)
  * [ Star  177 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache)


#  feat: support for CPU RAM -> shared storage KV Cache offloading
Copy link
Copy link
[feat: support for CPU RAM -> shared storage KV Cache offloading](https://github.com/llm-d/llm-d-kv-cache/issues/526#top)#526
Copy link
Assignees
## Description
opened [on Apr 16, 2026](https://github.com/llm-d/llm-d-kv-cache/issues/526#issue-4275976016)
Issue body actions
## Summary
Current backend implementations of the offloading connector support memory blocks transfer for the following paths:
  * `GPU->CPU RAM` (via Offloading Connector or LMCache)
  * `GPU->Storage` (via llm-d FS Connector or LMCache)


For use-cases where vLLM runs on CPU cores only, especially nodes without GPU, it is impossible to use the KV Cache Offloading functionality.
## Proposal
  * Support CPU-only deployment path.
  * Enable KV Cache Offloading for `CPU memory->Storage` with no GPU/CUDA dependency.


## Activity
### satyamg1620 commented on Apr 18, 2026 
More actions
/assign
[github-actions](https://github.com/apps/github-actions)
assigned [on Apr 18, 2026](https://github.com/llm-d/llm-d-kv-cache/issues/526#event-24641589725)
### effi-ofer commented on Apr 20, 2026 
Contributor
More actions
Won't this be resolved by [vllm-project/vllm#38260](https://github.com/vllm-project/vllm/issues/38260)? [@dannyharnik](https://github.com/dannyharnik)
### LedWork commented on Apr 20, 2026 
Author
More actions
Thanks [@effi-ofer](https://github.com/effi-ofer) for linking this here!
To my understanding the RFC proposes the following paths:

```
GPU → Primary Tier (CPU) → [all] Secondary Tiers
GPU ← Primary Tier (CPU) ← Secondary Tier

```

Referring to above RFC, maybe it would be possible to support something like:

```
CPU → Primary Tier → Secondary Tiers
CPU ← Primary Tier ← Secondary Tier

```

[guygir](https://github.com/guygir)
added a commit that references this issue [on Apr 20, 2026](https://github.com/llm-d/llm-d-kv-cache/issues/526#event-24675720293)
[deps(actions): bump crate-ci/typos from 1.40.0 to 1.40.1 ([llm-d#526](https://github.com/llm-d/llm-d-kv-cache/issues/526))](https://github.com/guygir/llm-d-kv-cache-manager/commit/c22eb3cdd3f72ed18ed2f2817b192e518962c430)
### github-actions commented on Jul 22, 2026 
[on Jul 22, 2026](https://github.com/llm-d/llm-d-kv-cache/issues/526#issuecomment-5053380432) – with [GitHub Actions](https://help.github.com/en/actions)
More actions
This issue is marked as stale after 90d of inactivity. After an additional 30d of inactivity (15d to become rotten, then 15d more), it will be closed. To prevent this issue from being closed, add a comment or remove the `lifecycle/stale` label.
[github-actions](https://github.com/apps/github-actions)
added [on Jul 22, 2026](https://github.com/llm-d/llm-d-kv-cache/issues/526#event-28357982954)
### LedWork commented on Jul 23, 2026 
Author
More actions
Hello, bumping up to avoid retention - do you by chance have any update on this? ;) Thanks!
[github-actions](https://github.com/apps/github-actions)
removed [on Jul 23, 2026](https://github.com/llm-d/llm-d-kv-cache/issues/526#event-28373662153)
### satyamg1620 commented on Jul 23, 2026 
More actions
[@LedWork](https://github.com/LedWork) I missed it. I will work on it and soon raise PR
### dannyharnik commented on Jul 23, 2026 
Collaborator
More actions
This seems to be covered by vLLM multi-tier offloading. See here: <https://docs.vllm.ai/en/latest/features/kv_offloading_usage/>
👍React with 👍1Reacted by Satyam Gupta
[Sign up for free](https://github.com/signup?return_to=https://github.com/llm-d/llm-d-kv-cache/issues/526)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/llm-d/llm-d-kv-cache/issues/526)
## Metadata
## Metadata
### Assignees
  * [satyamg1620](https://github.com/satyamg1620)


### Labels
No labels
No labels
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
