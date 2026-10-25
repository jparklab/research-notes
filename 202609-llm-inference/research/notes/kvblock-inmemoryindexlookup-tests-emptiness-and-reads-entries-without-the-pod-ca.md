---
title: 'kvblock: InMemoryIndex.Lookup tests emptiness and reads entries without the
  pod-cache lock · Issue #2698 · llm-d/llm-d-router'
id: kvblock-inmemoryindexlookup-tests-emptiness-and-reads-entries-without-the-pod-ca
tags:
- llmd-kv-cache-routing-c665d5
- kv-block-index
- concurrency-bug
- prefix-chain
- github-issue
created: '2026-09-14T11:53:45.691657Z'
updated: '2026-09-15T04:55:24.050641Z'
source: https://github.com/llm-d/llm-d-router/issues/2698
source_domain: github.com
fetched_at: '2026-09-14T11:53:45.668512Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub issue #2698 (opened Sep 4, 2026, llm-d/llm-d-router, by collaborator
  nilig) is a precise concurrency-bug writeup in kvblock.InMemoryIndex.Lookup: the
  method checks whether a key''s pod-cache is empty and then reads its holder keys
  as two SEPARATELY locked calls (not atomic), even though PodCache.mu exists for
  exactly this purpose and writers (Add, Evict) correctly hold it across their check-then-act
  sequences. If a concurrent Evict or Clear removes the last entry for a key between
  Lookup''s two calls, Keys() returns empty, causing (1) the unfiltered branch to
  assign an empty slice into the result map -- producing a key that is ''present but
  held by nobody,'' contradicting the early-stop guard meant to prevent exactly this
  -- and (2) the prefix-chain-break check being skipped in both branches, letting
  the returned map carry keys from beyond where the chain should have stopped. States
  the bug does NOT corrupt routing today because the downstream matcher walks keys
  in order and stops at the first empty-entries key regardless, so the cost is only
  wasted lookups/materialization, not wrong routing decisions. The visible symptom
  was in telemetry: keysFound counted map entries rather than entries-with-holders,
  so an empty key was published as a false block-hit; PR #2669 already fixed the metric
  by counting only keys with >=1 entry. The full fix, PR #2680, rewrites Lookup to
  read entries and count in a single critical section under PodCache.mu via a new
  PodCache.filteredEntries method, storing a result only when the filtered slice is
  non-empty. Notes this is not a Go data race (go -race will never catch it) since
  each locked call is individually safe -- only the sequence across calls is wrong
  -- and the race window is only a few instructions wide, though evictions run continuously
  from BlockRemoved events, pod clears, and LRU capacity pressure.'
utility_score: 12.0
---

[Skip to content](https://github.com/llm-d/llm-d-router/issues/2698#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/2698) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/2698) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/issues/2698) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/issues/2698).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  336 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


#  kvblock: InMemoryIndex.Lookup tests emptiness and reads entries without the pod-cache lock
Copy link
Copy link
[kvblock: InMemoryIndex.Lookup tests emptiness and reads entries without the pod-cache lock](https://github.com/llm-d/llm-d-router/issues/2698#top)#2698
Copy link
Assignees
## Description
opened [on Sep 4, 2026](https://github.com/llm-d/llm-d-router/issues/2698#issue-5351242902)
Last edited by nilig
Collaborator
Issue body actions
`InMemoryIndex.Lookup` decides whether a key is held and then reads its holders as two separately locked calls, so the pair is not atomic:

```
if pods, found := m.data.Get(requestKey); found {
    if pods == nil || pods.cache.Len() == 0 {
        return podsPerKey, nil // early stop since prefix-chain breaks here
    }
    ...
    podsPerKey[requestKey] = pods.cache.Keys()   // unfiltered branch
```

`PodCache.mu` exists for exactly this and the writers use it: `Add` holds it around its inserts, and `Evict` holds it across `Remove` plus the `cache.Len() == 0` emptiness check, treating remove-then-test as one critical section. `Lookup` holds it for neither call. The underlying LRU is individually thread-safe, so each call is fine on its own and only the sequence is wrong.
If a concurrent `Evict` or `Clear` removes the last entry for that key between the two calls, `Keys()` returns empty and two things follow:
  1. The unfiltered branch (`podIdentifierSet` empty) assigns that empty slice into the result, producing a key that is present but held by nobody. That contradicts the early-stop the guard above it is there to perform.
  2. The prefix-chain cut is skipped in both branches, so the returned map can carry keys from beyond a break in the chain.


Neither corrupts routing today. The matcher walks keys in order and breaks at the first key with no entries, so a chain that should have ended still ends in the same place; the cost is keys looked up and materialized for nothing. The visible symptom was telemetry: `keysFound` counted map entries, so an empty entry was published as a block hit. [#2669](https://github.com/llm-d/llm-d-router/pull/2669) fixed that by counting only keys with at least one entry.
**[#2680](https://github.com/llm-d/llm-d-router/pull/2680) closes this.** Its rewrite of `Lookup` reads the entries and their count in a single critical section under `PodCache.mu` (`PodCache.filteredEntries`) and stores the result only when the filtered slice is non-empty, so neither the check-then-use window nor the empty entry can occur. Until that lands the window remains on the current head, where the consequence is wasted lookups rather than wrong routing, so this is tracked here rather than fixed separately: a standalone patch would land in the same function [#2680](https://github.com/llm-d/llm-d-router/pull/2680) rewrites.
Two reasons this is easy to miss. It is not a Go data race, since both calls are properly synchronized, so `-race` will never flag it. And the opportunity is constant while the window is a few instructions: evictions run continuously from `BlockRemoved` events, pod clears, and LRU capacity pressure, but the race needs the last entry for that exact key to disappear inside the gap.
Wonder if we could fix it by reading the holders once and letting that single result decide, rather than locking around the pair:

```
entries := pods.cache.Keys()
if len(entries) == 0 {
    return podsPerKey, nil // prefix chain breaks here
}
```

That is atomic by construction, drops a redundant `Len()` call, and removes the check-then-use instead of guarding it. Worth confirming the filtered branch reads the same snapshot rather than calling `Keys()` again.
cc [@vMaroon](https://github.com/vMaroon)
## Activity
[nilig](https://github.com/nilig)
assigned [on Sep 4, 2026](https://github.com/llm-d/llm-d-router/issues/2698#event-30571616038)
[Sign up for free](https://github.com/signup?return_to=https://github.com/llm-d/llm-d-router/issues/2698)**to join this conversation on GitHub.** Already have an account? [Sign in to comment](https://github.com/login?return_to=https://github.com/llm-d/llm-d-router/issues/2698)
## Metadata
## Metadata
### Assignees
  * [](https://github.com/nilig)
  * [vMaroon](https://github.com/vMaroon)


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
