---
title: 'feat(epp): simplified thunder-agent per-pod admission gate in fairnessPolicy
  by zetxqx · Pull Request #3076 · llm-d/llm-d-router'
id: featepp-simplified-thunder-agent-per-pod-admission-gate-in-fairnesspolicy-by-zet
tags:
- internal-llm-inference-stacks-7f0c90
- epp-plugin-framework
- llm-d-router
- gap-check-cc1
created: '2026-10-10T10:59:54.620404Z'
updated: '2026-10-10T11:02:15.265105Z'
source: https://github.com/llm-d/llm-d-router/pull/3076
source_domain: github.com
fetched_at: '2026-10-10T10:59:54.596469Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Open llm-d-router PR #3076 (opened 2026-09-29 by zetxqx, labeled area/epp,
  kind/feature, size/XXL), still active as of 2026-10-09: adds a ''simplified thunder-agent''
  per-pod admission gate inside the fairnessPolicy extension point, part of tracking
  issue #2964. Primary evidence that llm-d-router''s EPP plugin/extension-point architecture
  is under active, non-trivial feature development in the weeks immediately preceding
  this gap-check (late Sept/early Oct 2026) -- the opposite direction from removing
  plugin extensibility.'
---

[Skip to content](https://github.com/llm-d/llm-d-router/pull/3076#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/3076) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/3076) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/3076) to refresh your session. Dismiss alert
## Conversation
### 
commented [Sep 29, 2026](https://github.com/llm-d/llm-d-router/pull/3076#issue-5628198758) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/3076).
Copy link  Copy Markdown 
Contributor
**What type of PR is this?**
/kind feature
**What this PR does / why we need it** :
This is a much simplified version of thunder agent: some explanation can be found [here](https://docs.google.com/document/d/1c7hztONMXoQEkG6xzG998g6HWEsFWd5E4iuAdvRNFpY/edit?tab=t.4lb0lkg7ce9g)
This is in parallel with [#3053](https://github.com/llm-d/llm-d-router/pull/3053). Recommend review this first as it's simple
Stacked on [#2968](https://github.com/llm-d/llm-d-router/pull/2968) and [#3052](https://github.com/llm-d/llm-d-router/pull/3052), review the last two commit only.
**Which issue(s) this PR fixes** :
Part of [#2964](https://github.com/llm-d/llm-d-router/issues/2964)
**Release note** :

```
thunder-agent: per-endpoint admission gate with a simplified idle-lease model.

```

[github-actions](https://github.com/apps/github-actions) Bot added [ size/XXL ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FXXL) Denotes a PR that changes 1000+ lines, ignoring generated files. [ area/epp ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fepp) [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. and removed [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. labels [Sep 29, 2026](https://github.com/llm-d/llm-d-router/pull/3076#event-32066075369)
[zetxqx](https://github.com/zetxqx) changed the title ~~feat(epp): thunder-agent per-pod admission gate, simplified lease model~~ feat(epp): simplified thunder-agent per-pod admission gate in fairnessPolicy [Sep 29, 2026](https://github.com/llm-d/llm-d-router/pull/3076#event-32111433807)
[github-actions](https://github.com/apps/github-actions) Bot added [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. and removed [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. labels [Sep 29, 2026](https://github.com/llm-d/llm-d-router/pull/3076#event-32111447826)
[zetxqx](https://github.com/zetxqx) marked this pull request as ready for review [September 29, 2026 20:19](https://github.com/llm-d/llm-d-router/pull/3076#event-32111504853)
[zetxqx](https://github.com/zetxqx) requested a review from a team as a [code owner](https://github.com/llm-d/llm-d-router/blob/aded2549079a252f5eb9aaf910b75763b0a9b365/CODEOWNERS#L14) [September 29, 2026 20:19](https://github.com/llm-d/llm-d-router/pull/3076#event-32111505622)
[zetxqx](https://github.com/zetxqx) requested review from [ahg-g](https://github.com/ahg-g), [elevran](https://github.com/elevran), [hexfusion](https://github.com/hexfusion), [liu-cong](https://github.com/liu-cong) and [vMaroon](https://github.com/vMaroon) [September 29, 2026 20:19](https://github.com/llm-d/llm-d-router/pull/3076#event-32111506088)
[github-actions](https://github.com/apps/github-actions) Bot added [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. and removed [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. labels [Sep 29, 2026](https://github.com/llm-d/llm-d-router/pull/3076#event-32111511706)
[zetxqx](https://github.com/zetxqx) [force-pushed](https://github.com/llm-d/llm-d-router/compare/44544c04aa6febd453d1009f9ed34c8cc6ea1ca4..7d548355ad8153646d6733860947bed9580d1e34) the  thunder-agent-lease-main branch from [`44544c0`](https://github.com/llm-d/llm-d-router/commit/44544c04aa6febd453d1009f9ed34c8cc6ea1ca4) to [`7d54835`](https://github.com/llm-d/llm-d-router/commit/7d548355ad8153646d6733860947bed9580d1e34) [ Compare ](https://github.com/llm-d/llm-d-router/compare/44544c04aa6febd453d1009f9ed34c8cc6ea1ca4..7d548355ad8153646d6733860947bed9580d1e34) [September 30, 2026 04:37](https://github.com/llm-d/llm-d-router/pull/3076#event-32135159005)
This was referenced Oct 6, 2026
[ feat(epp): add a session manager used by thunder agent #2968 ](https://github.com/llm-d/llm-d-router/pull/2968)
Merged 
[ feat(epp): thunder-agent per-pod admission gate #3053 ](https://github.com/llm-d/llm-d-router/pull/3053)
Closed 
requested changes  [ Oct 6, 2026 ](https://github.com/llm-d/llm-d-router/pull/3076#pullrequestreview-5429333885)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/3076/files)
### 
left a comment 
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Thanks, this is easier to follow than [#3053](https://github.com/llm-d/llm-d-router/pull/3053). Shared with [#3053](https://github.com/llm-d/llm-d-router/pull/3053): the SaturationDetector is used only for side effects and returns 0, which disables saturation HoL blocking and UsageLimitPolicy ceilings; when the FairnessPolicy returns nil, lower bands still dispatch and bypass the gate; and the pod the gate admits against is not enforced, since the scorer is 1.0/0.0 and PreRequest rebinds the session to the picked pod. I think these need new flow control extension points (a per-cycle endpoint view, a per-flow hold that does not fall through, scheduling constrained to the admitted pod), which are worth settling on [#2964](https://github.com/llm-d/llm-d-router/issues/2964). Specific to this PR:
**Must**
  * A paused session can only restore onto its own pod and may wait until the 30 min backstop. The paper restores onto any pod with room; please do that, or document the limit and lower the backstop.
  * Admitted turns reclaim with `minIdle=0`, so a turn can reclaim sessions that just finished a turn.
  * Every 1 ms dispatch cycle does O(pods x sessions) work under the ledger lock and emits a DEBUG log.


**Should**
  * The 30 min starvation default never fires under the default 60 s request TTL; the request expires first. Make the defaults consistent.
  * Log a warning when flow control is disabled, when P/D is configured (prefill pods make every new session appear to fit), and when other scorers can outweigh the session scorer.
  * Log when the 4M-token fallback capacity is used.


**Nits**
  * `holds_total` should name what is counted.
  * The README footprint text is stale.
  * Log keys should follow the in-tree camelCase style.


[ahg-g](https://github.com/ahg-g) mentioned this pull request [ Oct 6, 2026 ](https://github.com/llm-d/llm-d-router/pull/3076#ref-issue-5537496390)
[ [Agentic] Session level admission control: add a ThunderAgent plugin #2964 ](https://github.com/llm-d/llm-d-router/issues/2964)
[zetxqx](https://github.com/zetxqx) [force-pushed](https://github.com/llm-d/llm-d-router/compare/cdf0ab9e4d18f3842a4fab54a11bc1fca9fa134c..5a31644d7d45057861c4b1028daa0c8126cf1086) the  thunder-agent-lease-main branch 2 times, most recently from [`cdf0ab9`](https://github.com/llm-d/llm-d-router/commit/cdf0ab9e4d18f3842a4fab54a11bc1fca9fa134c) to [`5a31644`](https://github.com/llm-d/llm-d-router/commit/5a31644d7d45057861c4b1028daa0c8126cf1086) [ Compare ](https://github.com/llm-d/llm-d-router/compare/cdf0ab9e4d18f3842a4fab54a11bc1fca9fa134c..5a31644d7d45057861c4b1028daa0c8126cf1086) [October 7, 2026 20:01](https://github.com/llm-d/llm-d-router/pull/3076#event-32722199040)
[zetxqx](https://github.com/zetxqx) mentioned this pull request [ Oct 7, 2026 ](https://github.com/llm-d/llm-d-router/pull/3076#ref-issue-5751314306)
[ thunder-agent: refactoring to fit the current plugin and flow control framework #3221 ](https://github.com/llm-d/llm-d-router/issues/3221)
[zetxqx](https://github.com/zetxqx) [force-pushed](https://github.com/llm-d/llm-d-router/compare/917f6f3d82899b9de8bd7420768781eb9781e5c9..86105bca61709db68e68d183a5c16e7a1c2ba7c4) the  thunder-agent-lease-main branch 2 times, most recently from [`917f6f3`](https://github.com/llm-d/llm-d-router/commit/917f6f3d82899b9de8bd7420768781eb9781e5c9) to [`86105bc`](https://github.com/llm-d/llm-d-router/commit/86105bca61709db68e68d183a5c16e7a1c2ba7c4) [ Compare ](https://github.com/llm-d/llm-d-router/compare/917f6f3d82899b9de8bd7420768781eb9781e5c9..86105bca61709db68e68d183a5c16e7a1c2ba7c4) [October 8, 2026 04:47](https://github.com/llm-d/llm-d-router/pull/3076#event-32748599036)
[zetxqx](https://github.com/zetxqx) requested a review from [sagearc](https://github.com/sagearc) as a [code owner](https://github.com/llm-d/llm-d-router/blob/be206f181430494c1c53c54aee477cc30ec1df95/CODEOWNERS#L58) [October 8, 2026 04:47](https://github.com/llm-d/llm-d-router/pull/3076#event-32748599145)
[github-actions](https://github.com/apps/github-actions) Bot added the [ area/scheduling ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fscheduling) label [Oct 8, 2026](https://github.com/llm-d/llm-d-router/pull/3076#event-32748609985)
[zetxqx](https://github.com/zetxqx) [force-pushed](https://github.com/llm-d/llm-d-router/compare/86105bca61709db68e68d183a5c16e7a1c2ba7c4..a025437bdc44b911231825595c3eebd2977d3ff8) the  thunder-agent-lease-main branch from [`86105bc`](https://github.com/llm-d/llm-d-router/commit/86105bca61709db68e68d183a5c16e7a1c2ba7c4) to [`a025437`](https://github.com/llm-d/llm-d-router/commit/a025437bdc44b911231825595c3eebd2977d3ff8) [ Compare ](https://github.com/llm-d/llm-d-router/compare/86105bca61709db68e68d183a5c16e7a1c2ba7c4..a025437bdc44b911231825595c3eebd2977d3ff8) [October 8, 2026 05:03](https://github.com/llm-d/llm-d-router/pull/3076#event-32749704681)
### 
commented [Oct 8, 2026](https://github.com/llm-d/llm-d-router/pull/3076#issuecomment-6052845328) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/3076).
Copy link  Copy Markdown 
Contributor Author  
|  Thanks for the review! I rebased this PR onto the updated [#2968](https://github.com/llm-d/llm-d-router/pull/2968) and [#3052](https://github.com/llm-d/llm-d-router/pull/3052): the gate now reads each queue head's session from the agent-identity attribute, the same key as the ledger. Each fix is its own commit; replies below. On the three shared points: the first two (the SaturationDetector used only for its side effects, and `Pick` returning nil letting lower bands bypass the gate) need flow control to pick first and then ask an admission check for that request. This is tracked in [#3221](https://github.com/llm-d/llm-d-router/issues/3221) (item 2). The third, the admitted pod not being enforced, is deliberate. The scorer is a soft pin, like `session-affinity-scorer`: it is meant to be combined with a load scorer such as `active-request-scorer`, with the thunder-agent scorer given a higher weight than the others. **Must**
> A paused session can only restore onto its own pod and may wait until the 30 min backstop. The paper restores onto any pod with room; please do that, or document the limit and lower the backstop.
This is intended: waiting for the session's own pod keeps its warm prefix. I documented it in the README and in `Pick`, including that it differs from the original ThunderAgent, in [`4864adb`](https://github.com/llm-d/llm-d-router/commit/4864adb018619f3db9f2ba3fda5cf3eddb30c68e). An option to restore onto another pod is tracked in [#3221](https://github.com/llm-d/llm-d-router/issues/3221) (TODO in [`dd32a67`](https://github.com/llm-d/llm-d-router/commit/dd32a67798a88fbafd4dcad9c2b108388bf4022e)). The backstop default goes down in the defaults item below.
> Admitted turns reclaim with minIdle=0, so a turn can reclaim sessions that just finished a turn.
Fixed in [`78caca1`](https://github.com/llm-d/llm-d-router/commit/78caca145d37ddcbd29febc98b0c9a9098a6ee3d): admitted turns use the idle lease too, so a session that just finished a turn keeps its room. The lease now lives in the ledger, so every reclaim uses the same value.
> Every 1 ms dispatch cycle does O(pods x sessions) work under the ledger lock and emits a DEBUG log.
Fixed in [`9613245`](https://github.com/llm-d/llm-d-router/commit/9613245f135e1738378279357e3243bb11623ee1):
  1. Removed the per-cycle hold log.
  2. Pod room is computed only when a paused or new session is waiting. Admitted and anonymous heads always dispatch, so the common case skips it. With 2000 sessions on 32 pods, a cycle went from about 55us to 9us (`BenchmarkPickAdmittedHeads`).

**Should**
> The 30 min starvation default never fires under the default 60 s request TTL; the request expires first. Make the defaults consistent.
Fixed in [`4ac4c75`](https://github.com/llm-d/llm-d-router/commit/4ac4c751b82bb267944547d80191a30e4fba90b5): the `headWaitStarvationMs` default is now 30 s, below the 60 s request TTL, and the README says to keep it below the band's TTL.
> Log a warning when flow control is disabled, when P/D is configured (prefill pods make every new session appear to fit), and when other scorers can outweigh the session scorer.
Added two logs in [`6926dfc`](https://github.com/llm-d/llm-d-router/commit/6926dfcb490ece99508b773585b824556e86c368): a prefill pod is logged when it first enters the ledger, and an admitted session moving off its pod is logged at DEBUG. I skipped the flow control one: the plugin cannot see whether flow control is on, and the README and sample config show the required setup.
> Log when the 4M-token fallback capacity is used.
I didn't add a log: the `endpoint_capacity_tokens` gauge already shows each pod's capacity, so a pod on the fallback shows 4194304. A log when a pod first enters the ledger would fire for every new pod, because the ledger sees it before its first metrics scrape. **Nits**
> holds_total should name what is counted.
Renamed to `thunder_agent_delayed_dispatches_total` in [`bddb05a`](https://github.com/llm-d/llm-d-router/commit/bddb05acde7d21568544e5fac41b68bb18cdec96): dispatches whose request waited at least 1 s in the queue, by session class.
> The README footprint text is stale.
Fixed in the rebase: the footprint is the larger of the last `usage.total_tokens` and the sum of the estimates of the turns in flight.
> Log keys should follow the in-tree camelCase style.
Fixed in [`a025437`](https://github.com/llm-d/llm-d-router/commit/a025437bdc44b911231825595c3eebd2977d3ff8): the reclaim log now uses a sentence and camelCase keys (`sessionClass`, `pausedSessions`).  |  
| --- |  
[zetxqx](https://github.com/zetxqx) requested a review from [ahg-g](https://github.com/ahg-g) [October 8, 2026 05:17](https://github.com/llm-d/llm-d-router/pull/3076#event-32750668494)
[ahg-g](https://github.com/ahg-g) added this to the [v1.0.0](https://github.com/llm-d/llm-d-router/milestone/10) milestone [Oct 8, 2026](https://github.com/llm-d/llm-d-router/pull/3076#event-32766499101)
[ptimizeroracle](https://github.com/ptimizeroracle) mentioned this pull request [ Oct 8, 2026 ](https://github.com/llm-d/llm-d-router/pull/3076#ref-pullrequest-5760304040)
[ fix(tokenizer): count chat-completions tool_calls in the estimate backend #3257 ](https://github.com/llm-d/llm-d-router/pull/3257)
[ahg-g](https://github.com/ahg-g) mentioned this pull request [ Oct 8, 2026 ](https://github.com/llm-d/llm-d-router/pull/3076#ref-issue-5404632444)
[ [Flow Control] Make fairness and ordering agentic-aware #2794 ](https://github.com/llm-d/llm-d-router/issues/2794)
[github-actions](https://github.com/apps/github-actions) Bot added the [ needs-rebase ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aneeds-rebase) PR has merge conflicts and needs a rebase label [Oct 9, 2026](https://github.com/llm-d/llm-d-router/pull/3076#event-32891408882)
### 
commented [Oct 9, 2026](https://github.com/llm-d/llm-d-router/pull/3076#issuecomment-6084825734)
Copy link  Copy Markdown 
Contributor  
|  [@zetxqx](https://github.com/zetxqx) This PR has merge conflicts with `main`. Please rebase on `main` and push to resolve them: 
```
gh pr checkout 3076
git fetch origin main
git rebase origin/main
# resolve conflicts
git push --force-with-lease
```
 |  
| --- |  
[zetxqx](https://github.com/zetxqx) added 12 commits [October 9, 2026 10:02](https://github.com/llm-d/llm-d-router/pull/3076#commits-pushed-c045738)
` feat(epp): thunder-agent scorer, keep a session on the pod that holds…[](https://github.com/llm-d/llm-d-router/pull/3076/commits/c04573833737d90f7a01a47b1729c03b218beddd "feat\(epp\): thunder-agent scorer, keep a session on the pod that holds its KV

Pin a session's turns to the pod recorded in the ledger: 1.0 for that pod,
0.0 for every other candidate. For sessions the ledger does not know, and
when the bound pod is not a candidate, the scorer abstains and PreRequest
binds the session to whatever pod was picked.

Signed-off-by: bob tian <zetxqx@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[zetxqx](https://github.com/zetxqx) Bob Tian 
SSH Key Fingerprint: JgaqgM7rX7a/sjn2dVSmSiKU1Xua2qGQFRC4STTlUBw
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
… its KV

Pin a session's turns to the pod recorded in the ledger: 1.0 for that pod,
0.0 for every other candidate. For sessions the ledger does not know, and
when the bound pod is not a candidate, the scorer abstains and PreRequest
binds the session to whatever pod was picked.

Signed-off-by: bob tian <zetxqx@gmail.com>
```

` fix(epp): thunder-agent scorer is an Affinity scorer, document it[](https://github.com/llm-d/llm-d-router/pull/3076/commits/478d2f87fa2bd5fb89376e542ac8e23dddbfdd39 "fix\(epp\): thunder-agent scorer is an Affinity scorer, document it

The scorer pins a session to one pod, so its category is Affinity. The
README describes the scorer and its overlap with session-affinity-scorer:
the binding comes from the ledger, so the two should not be configured in
one profile.

Signed-off-by: bob tian <zetxqx@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[zetxqx](https://github.com/zetxqx) Bob Tian 
SSH Key Fingerprint: JgaqgM7rX7a/sjn2dVSmSiKU1Xua2qGQFRC4STTlUBw
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/3076).

```
The scorer pins a session to one pod, so its category is Affinity. The
README describes the scorer and its overlap with session-affinity-scorer:
the binding comes from the ledger, so the two should not be configured in
one profile.

Signed-off-by: bob tian <zetxqx@gmail.com>
```

` feat(epp): thunder-agent per-pod admission gate[](https://github.com/llm-d/llm-d-router/pull/3076/commits/6835f3f6744fc4ce6b0eecade0ba683b757c7e02 "feat\(epp\): thunder-agent per-pod admission gate

The gate: turns of admitted sessions always dispatch; a paused session's
next turn waits for its own pod \(strict origin affinity, the session never
moves\); a new session is admitted onto a pod with room and holds until one
fits it; any head waiting past headWaitStarvationMs is force-admitted.

Idle sessions give up their room on demand, in Pick, longest idle first:
when the picked paused or new session does not fit, only sessions idle past
idleLeaseSeconds; when an admitted session's turn pushes its pod over the
ceiling, any idle session. A session that gives up its room is paused and
its next turn must fit again. Sessions with a turn queued are never paused.
There is one working-set view and no pause sweep: pausing only matters to
the next admission, so it happens there. The background sweep keeps live
reservations.

Pick reads the session of each queue head from the agent-identity
attribute, the same key as the ledger; the queue itself is named by the
fairness ID.

Wiring: the plugin fills the flow-control saturationDetector slot to receive
the endpoint list each dispatch cycle \(it refreshes the pod ledger there\)
but always reports unsaturated, because the controller's own gate is a
band-level head-of-line block that would also stall the turns which free
capacity. Admission happens per session in the fairness policy's Pick. A
picked session's room is reserved until PreRequest charges it, so
back-to-back dispatch cycles cannot admit into the same room twice and a
dispatched turn's session cannot be reclaimed as idle; unconfirmed
reservations expire, so a cancelled request cannot become phantom occupancy.

Signed-off-by: bob tian <zetxqx@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[zetxqx](https://github.com/zetxqx) Bob Tian 
SSH Key Fingerprint: JgaqgM7rX7a/sjn2dVSmSiKU1Xua2qGQFRC4STTlUBw
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
The gate: turns of admitted sessions always dispatch; a paused session's
next turn waits for its own pod (strict origin affinity, the session never
moves); a new session is admitted onto a pod with room and holds until one
fits it; any head waiting past headWaitStarvationMs is force-admitted.

Idle sessions give up their room on demand, in Pick, longest idle first:
when the picked paused or new session does not fit, only sessions idle past
idleLeaseSeconds; when an admitted session's turn pushes its pod over the
ceiling, any idle session. A session that gives up its room is paused and
its next turn must fit again. Sessions with a turn queued are never paused.
There is one working-set view and no pause sweep: pausing only matters to
the next admission, so it happens there. The background sweep keeps live
reservations.

Pick reads the session of each queue head from the agent-identity
attribute, the same key as the ledger; the queue itself is named by the
fairness ID.

Wiring: the plugin fills the flow-control saturationDetector slot to receive
the endpoint list each dispatch cycle (it refreshes the pod ledger there)
but always reports unsaturated, because the controller's own gate is a
band-level head-of-line block that would also stall the turns which free
capacity. Admission happens per session in the fairness policy's Pick. A
picked session's room is reserved until PreRequest charges it, so
back-to-back dispatch cycles cannot admit into the same room twice and a
dispatched turn's session cannot be reclaimed as idle; unconfirmed
reservations expire, so a cancelled request cannot become phantom occupancy.

Signed-off-by: bob tian <zetxqx@gmail.com>
```

` fix(epp): thunder-agent gate sizes a session with its turns in flight[](https://github.com/llm-d/llm-d-router/pull/3076/commits/d86190a3199d3b213d2bacb35592d5893dd2af6d "fix\(epp\): thunder-agent gate sizes a session with its turns in flight

PreRequest adds each turn's estimate to the turns already in flight, so the
gate sizes a waiting turn as the session's turns in flight plus the new one,
with committed tokens as the floor. Sizing it as the new turn alone
undercounted a session with parallel turns, and the gate then reserved too
little room for it.

Signed-off-by: bob tian <zetxqx@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[zetxqx](https://github.com/zetxqx) Bob Tian 
SSH Key Fingerprint: JgaqgM7rX7a/sjn2dVSmSiKU1Xua2qGQFRC4STTlUBw
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
PreRequest adds each turn's estimate to the turns already in flight, so the
gate sizes a waiting turn as the session's turns in flight plus the new one,
with committed tokens as the floor. Sizing it as the new turn alone
undercounted a session with parallel turns, and the gate then reserved too
little room for it.

Signed-off-by: bob tian <zetxqx@gmail.com>
```

` docs(epp): thunder-agent gate restores a paused session only on its pod[](https://github.com/llm-d/llm-d-router/pull/3076/commits/ada0db3ef4047ca19bbcec8c78c2442b0053794a "docs\(epp\): thunder-agent gate restores a paused session only on its pod

State that this is intended and differs from the original ThunderAgent,
which restores a paused session onto any pod with room. Waiting for the
session's own pod keeps its warm prefix; the wait is bounded by
headWaitStarvationMs.

Signed-off-by: bob tian <zetxqx@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[zetxqx](https://github.com/zetxqx) Bob Tian 
SSH Key Fingerprint: JgaqgM7rX7a/sjn2dVSmSiKU1Xua2qGQFRC4STTlUBw
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
State that this is intended and differs from the original ThunderAgent,
which restores a paused session onto any pod with room. Waiting for the
session's own pod keeps its warm prefix; the wait is bounded by
headWaitStarvationMs.

Signed-off-by: bob tian <zetxqx@gmail.com>
```

` docs(epp): link the thunder-agent paused-session restore limit to[](https://github.com/llm-d/llm-d-router/pull/3076/commits/869bfdb15f3c2930b6a5ce0aee3031be9e6737e3 "docs\(epp\): link the thunder-agent paused-session restore limit to #3221

Signed-off-by: bob tian <zetxqx@gmail.com>") llm…[](https://github.com/llm-d/llm-d-router/issues/3221) `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[zetxqx](https://github.com/zetxqx) Bob Tian 
SSH Key Fingerprint: JgaqgM7rX7a/sjn2dVSmSiKU1Xua2qGQFRC4STTlUBw
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
…-d#3221[](https://github.com/llm-d/llm-d-router/issues/3221)

Signed-off-by: bob tian <zetxqx@gmail.com>
```

` fix(epp): thunder-agent admitted turns reclaim only past the idle lease[](https://github.com/llm-d/llm-d-router/pull/3076/commits/1ff5bc37cff7c1eddb874122116ec5b1873aa798 "fix\(epp\): thunder-agent admitted turns reclaim only past the idle lease

An admitted session's turn that pushed its pod over the ceiling reclaimed
with no minimum idle time, so it could pause a session that had just
finished a turn and was about to send the next one. Use the idle lease for
every reclaim: such a session keeps its room, and the pod can stay over the
ceiling until a session passes the lease. The lease now lives in the ledger,
so callers cannot pass a different value.

Signed-off-by: bob tian <zetxqx@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[zetxqx](https://github.com/zetxqx) Bob Tian 
SSH Key Fingerprint: JgaqgM7rX7a/sjn2dVSmSiKU1Xua2qGQFRC4STTlUBw
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
An admitted session's turn that pushed its pod over the ceiling reclaimed
with no minimum idle time, so it could pause a session that had just
finished a turn and was about to send the next one. Use the idle lease for
every reclaim: such a session keeps its room, and the pod can stay over the
ceiling until a session passes the lease. The lease now lives in the ledger,
so callers cannot pass a different value.

Signed-off-by: bob tian <zetxqx@gmail.com>
```

` perf(epp): thunder-agent gate skips pod room when only admitted heads…[](https://github.com/llm-d/llm-d-router/pull/3076/commits/8f53dcc082655617f5c4d610ea3d5c62c3336136 "perf\(epp\): thunder-agent gate skips pod room when only admitted heads wait

Pick ran on every dispatch cycle and computed every pod's room and
reclaimable tokens under the ledger lock, scanning all sessions, and logged
at DEBUG whenever a head was held. Admitted and anonymous heads always
dispatch, so compute pod room only when a paused or new session waits, and
compute the picked session's own pod room in admitLocked. Drop the
per-cycle hold log. With 2000 sessions on 32 pods and 64 admitted heads,
BenchmarkPickAdmittedHeads goes from about 55us to 9us per cycle.

Signed-off-by: bob tian <zetxqx@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[zetxqx](https://github.com/zetxqx) Bob Tian 
SSH Key Fingerprint: JgaqgM7rX7a/sjn2dVSmSiKU1Xua2qGQFRC4STTlUBw
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
… wait

Pick ran on every dispatch cycle and computed every pod's room and
reclaimable tokens under the ledger lock, scanning all sessions, and logged
at DEBUG whenever a head was held. Admitted and anonymous heads always
dispatch, so compute pod room only when a paused or new session waits, and
compute the picked session's own pod room in admitLocked. Drop the
per-cycle hold log. With 2000 sessions on 32 pods and 64 admitted heads,
BenchmarkPickAdmittedHeads goes from about 55us to 9us per cycle.

Signed-off-by: bob tian <zetxqx@gmail.com>
```

` fix(epp): thunder-agent starvation backstop defaults below the reques…[](https://github.com/llm-d/llm-d-router/pull/3076/commits/593f933e9a55a83804927e5689979bd93b35c16b "fix\(epp\): thunder-agent starvation backstop defaults below the request TTL

The headWaitStarvationMs default was 30 minutes, but flow control sheds a
queued request after its default 60s request TTL, so the backstop never
fired. Default it to 30s and document that it must stay below the band's
request TTL. The sample config keeps its explicit 30 minutes, since it also
disables queue eviction.

Signed-off-by: bob tian <zetxqx@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[zetxqx](https://github.com/zetxqx) Bob Tian 
SSH Key Fingerprint: JgaqgM7rX7a/sjn2dVSmSiKU1Xua2qGQFRC4STTlUBw
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
…t TTL

The headWaitStarvationMs default was 30 minutes, but flow control sheds a
queued request after its default 60s request TTL, so the backstop never
fired. Default it to 30s and document that it must stay below the band's
request TTL. The sample config keeps its explicit 30 minutes, since it also
disables queue eviction.

Signed-off-by: bob tian <zetxqx@gmail.com>
```

` feat(epp): thunder-agent logs setups that defeat the gate[](https://github.com/llm-d/llm-d-router/pull/3076/commits/051e521441eb52198c41b8184f2b56352f53371f "feat\(epp\): thunder-agent logs setups that defeat the gate

Log when a prefill pod first enters the ledger, since prefill pods make
new sessions appear to fit, and log at DEBUG when an admitted session
moves off its pod \(the pod was filtered out, or other scorers outweigh the
session scorer\).

Signed-off-by: bob tian <zetxqx@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[zetxqx](https://github.com/zetxqx) Bob Tian 
SSH Key Fingerprint: JgaqgM7rX7a/sjn2dVSmSiKU1Xua2qGQFRC4STTlUBw
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
Log when a prefill pod first enters the ledger, since prefill pods make
new sessions appear to fit, and log at DEBUG when an admitted session
moves off its pod (the pod was filtered out, or other scorers outweigh the
session scorer).

Signed-off-by: bob tian <zetxqx@gmail.com>
```

` refactor(epp): rename thunder-agent holds_total to delayed_dispatches…[](https://github.com/llm-d/llm-d-router/pull/3076/commits/1fa2b369c64f719a9757f99e2ac9d2c9c599d3a8 "refactor\(epp\): rename thunder-agent holds_total to delayed_dispatches_total

The counter counts dispatches whose request waited at least 1s in the
flow-control queue, so name it for that.

Signed-off-by: bob tian <zetxqx@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[zetxqx](https://github.com/zetxqx) Bob Tian 
SSH Key Fingerprint: JgaqgM7rX7a/sjn2dVSmSiKU1Xua2qGQFRC4STTlUBw
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)

```
…_total

The counter counts dispatches whose request waited at least 1s in the
flow-control queue, so name it for that.

Signed-off-by: bob tian <zetxqx@gmail.com>
```

` fix(epp): thunder-agent reclaim log uses a sentence and camelCase keys[](https://github.com/llm-d/llm-d-router/pull/3076/commits/b46d7f2a3f5c5cd6ae6f106838c512e59399fccb "fix\(epp\): thunder-agent reclaim log uses a sentence and camelCase keys

Signed-off-by: bob tian <zetxqx@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[zetxqx](https://github.com/zetxqx) Bob Tian 
SSH Key Fingerprint: JgaqgM7rX7a/sjn2dVSmSiKU1Xua2qGQFRC4STTlUBw
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/3076).

```
Signed-off-by: bob tian <zetxqx@gmail.com>
```

[zetxqx](https://github.com/zetxqx) [force-pushed](https://github.com/llm-d/llm-d-router/compare/a025437bdc44b911231825595c3eebd2977d3ff8..b46d7f2a3f5c5cd6ae6f106838c512e59399fccb) the  thunder-agent-lease-main branch from [`a025437`](https://github.com/llm-d/llm-d-router/commit/a025437bdc44b911231825595c3eebd2977d3ff8) to [`b46d7f2`](https://github.com/llm-d/llm-d-router/commit/b46d7f2a3f5c5cd6ae6f106838c512e59399fccb) [ Compare ](https://github.com/llm-d/llm-d-router/compare/a025437bdc44b911231825595c3eebd2977d3ff8..b46d7f2a3f5c5cd6ae6f106838c512e59399fccb) [October 9, 2026 17:07](https://github.com/llm-d/llm-d-router/pull/3076#event-32894509720)
[github-actions](https://github.com/apps/github-actions) Bot removed the [ needs-rebase ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aneeds-rebase) PR has merge conflicts and needs a rebase label [Oct 9, 2026](https://github.com/llm-d/llm-d-router/pull/3076#event-32894520845)
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fllm-d%2Fllm-d-router%2Fpull%2F3076)
###  Labels 
[ area/epp ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fepp) [ area/scheduling ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fscheduling) [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. [ size/XXL ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FXXL) Denotes a PR that changes 1000+ lines, ignoring generated files.
###  2 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
