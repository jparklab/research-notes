---
title: 'Dynamic Speculative TTL for Unified Prefix-Cache Scoring · Issue #544 · llm-d/llm-d-kv-cache'
id: dynamic-speculative-ttl-for-unified-prefix-cache-scoring-issue-544-llm-dllm-d-kv
tags:
- llmd-kv-cache-routing-c665d5
- speculative-indexing
- ttl
- prefix-cache-scoring
- github-issue
- proposal
- locus-index-freshness-and-recovery
created: '2026-09-14T11:53:33.015961Z'
updated: '2026-09-15T04:55:24.036878Z'
source: https://github.com/llm-d/llm-d-kv-cache/issues/544
source_domain: github.com
fetched_at: '2026-09-14T11:53:32.995004Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub issue #544 (opened Apr 27, 2026, llm-d/llm-d-kv-cache, part of #353)
  is a design proposal for ''speculative'' KV-cache indexing with dynamic TTL, motivated
  by the unreliability of modeling engine-internal LRU cache state for agentic/multi-turn/tool-calling/branching
  inference workloads. Proposed mechanism: a per-endpoint ''KV Events Health Monitor''
  (PR 1, in llm-d-inference-scheduler) tracks last-event-received time, last-routed
  time, event rate, and speculative-to-confirmed confirmation rate to distinguish
  ''pipeline broken'' (routing without events) from ''idle'' (no routing). A dynamic
  TTL scorer (PR 3) then adapts automatically: no TTL config means permanent entries
  relying on index LRU capacity; when KV events are configured and flowing, short/precise
  TTL; when routing continues but events are delayed, TTL gradually increases; new
  config fields maxSpeculativeTTL and adaptiveTTL. PR 4 extends PodEntry with a Kind
  field distinguishing entry provenance and scoring weight: kv-event (confirmed from
  engine, weight 1.0), speculative (predicted from routing, weight 0.95), approximate
  (self-reported from response capture, weight 0.7) -- entries get promoted to kv-event
  once a real event confirms them. PR 2 (depends on #437''s gen-counter) calls Index.Clear()
  when the health monitor detects a pod has disappeared, covering crash/scale-down
  cases where no AllBlocksCleared event arrives. PR 5 hashes response output tokens
  into approximate index entries on completion to capture multi-turn context. As of
  Sep 2026 the issue is stale/unimplemented (no linked merged PRs), flagged with an
  open question on whether it''s a ''must have'' for the next release.'
utility_score: 13.0
---

[Skip to content](https://github.com/llm-d/llm-d-kv-cache/issues/544#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/issues/544) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/issues/544) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-kv-cache/issues/544) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-kv-cache/issues/544).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache) You must be signed in to change notification settings
  * [ Fork 156 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache)
  * [ Star  177 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-kv-cache)


#  Dynamic Speculative TTL for Unified Prefix-Cache Scoring
Copy link
Copy link
[Dynamic Speculative TTL for Unified Prefix-Cache Scoring](https://github.com/llm-d/llm-d-kv-cache/issues/544#top)#544
Copy link
Labels
## Description
opened [on Apr 27, 2026](https://github.com/llm-d/llm-d-kv-cache/issues/544#issue-4333747559)
Last edited by bongwoobak
Member
Issue body actions
Part of [#353](https://github.com/llm-d/llm-d-kv-cache/issues/353)
## Summary
Extend speculative indexing with dynamic TTL adjustment based on KV events health, enabling automatic adaptation without modeling engine-internal cache state. The scorer automatically adapts: short TTL when KV events are flowing (precise behavior), longer TTL when events are delayed, and permanent entries when KV events are unavailable.
## Motivation
As KV-cache management grows more complex, especially for agentic inference with multi-turn, tool-calling, and branching patterns - modeling engine internals via LRU becomes increasingly unreliable. The TTL-based approach is more robust against this growing complexity, adapting automatically based on KV events availability rather than approximating engine-internal state.
## Plan
### PR 1: KV Events Health Monitor
**Repo** : llm-d-inference-scheduler
Per-endpoint tracking of:
  * Last KV event received time
  * Last routed time (PreRequest)
  * Event rate
  * Confirmation rate (speculative → confirmed)


Data collection only — no TTL changes yet. Distinguishes "pipeline broken" (routing but no events) from "idle" (no routing, no events).
### PR 2: Pod disappearance cleanup
**Repo** : llm-d-inference-scheduler **Depends on** : llm-d-kv-cache#437 (gen-counter)
When health monitor detects a pod has disappeared from endpoint list, call `Index.Clear()` to invalidate all entries for that pod (including speculative). Covers pod crash, scale-down, node failure — cases where AllBlocksCleared event won't come from a dead pod.
### PR 3: Dynamic TTL
**Repo** : llm-d-inference-scheduler
Use health monitor data to adjust speculative entry TTL in PreRequest:
  * No KV events config → no TTL (permanent), index LRU capacity handles memory
  * KV events configured + recent routing + events delayed → gradually increase TTL
  * KV events configured + no recent routing → keep base TTL (normal idle)


New config fields: `maxSpeculativeTTL`, `adaptiveTTL`.
### PR 4: Annotation kind + scoring weights
**Repo** : llm-d-kv-cache + llm-d-inference-scheduler
Extend PodEntry with `Kind` field to distinguish entry source:
  * `kv-event` (confirmed from engine) — weight 1.0
  * `speculative` (predicted from routing) — weight 0.95
  * `approximate` (self-reported from response capture) — weight 0.7


When a KV event arrives for a block with speculative/approximate entry, promote to kv-event. Scorer applies kind-based weights.
### PR 5: ResponseComplete hook (optional)
**Repo** : llm-d-inference-scheduler
On response completion, hash output tokens and insert into index as approximate entries. Captures full conversation context for subsequent requests sharing the same prefix.
### PR 6: Dummy tokenizer (later)
**Repo** : llm-d-kv-cache
For tokenizer-free environments (Scenario D), generate pseudo-tokens from hash-based character chunking compatible with kvblock.Index.
## Dependencies
  * PR 2 depends on [feat:add support to invalidate KV cache via AllBlocksCleared event #437](https://github.com/llm-d/llm-d-kv-cache/pull/437) (gen-counter for AllBlocksCleared)
  * PR 4 requires changes in both llm-d-kv-cache (PodEntry) and llm-d-inference-scheduler (scorer)
  * PRs 1, 3, 5, 6 are independent of each other


🚀React with 🚀1Reacted by Maroon Ayoub
## Activity
### ahg-g commented on Jun 6, 2026 
More actions
[@bongwoobak](https://github.com/bongwoobak) [@vMaroon](https://github.com/vMaroon) is this a must have for the next release?
### github-actions commented on Sep 4, 2026 
[on Sep 4, 2026](https://github.com/llm-d/llm-d-kv-cache/issues/544#issuecomment-5548370247) – with [GitHub Actions](https://help.github.com/en/actions)
More actions
This issue is marked as stale after 90d of inactivity. After an additional 30d of inactivity (15d to become rotten, then 15d more), it will be closed. To prevent this issue from being closed, add a comment or remove the `lifecycle/stale` label.
[github-actions](https://github.com/apps/github-actions)
added [on Sep 4, 2026](https://github.com/llm-d/llm-d-kv-cache/issues/544#event-30591044608)
[Sign up for free](https://github.com/signup?return_to=https://github.com/llm-d/llm-d-kv-cache/issues/544)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/llm-d/llm-d-kv-cache/issues/544)
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
