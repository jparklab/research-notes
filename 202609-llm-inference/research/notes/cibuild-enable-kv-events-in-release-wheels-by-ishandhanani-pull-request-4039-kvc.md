---
title: '[CI/Build] Enable KV events in release wheels by ishandhanani · Pull Request
  #4039 · kvcache-ai/Mooncake · GitHub'
id: cibuild-enable-kv-events-in-release-wheels-by-ishandhanani-pull-request-4039-kvc
tags:
- llmd-kv-cache-routing-c665d5
- locus-offload-connector-routing-visibility
- mooncake
- kv-cache
- zmq-kv-events
created: '2026-09-14T12:44:17.812659Z'
updated: '2026-09-15T04:55:24.132792Z'
source: https://github.com/kvcache-ai/Mooncake/pull/4039
source_domain: github.com
fetched_at: '2026-09-14T12:44:17.791316Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'Mooncake PR #4039 (''[CI/Build] Enable KV events in release wheels,'' opened
  Sep 11 2026) treats ENABLE_KV_EVENTS as an already-working build flag and tries
  to wire it into official release-wheel CI; closed Sep 14 2026 in favor of direct
  successor PR #4060 (''Enable KV events in wheels with ZeroMQ dependencies''), which
  merged. Confirms Mooncake''s KV-event publishing capability (--kv_events_bind_endpoint,
  --kv_events_backend_id) shipped to main via a different PR chain than the dead-end
  #1251, and was being enabled in release wheels the same week as this research run.'
---

[Skip to content](https://github.com/kvcache-ai/Mooncake/pull/4039#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kvcache-ai/Mooncake/pull/4039) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kvcache-ai/Mooncake/pull/4039) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kvcache-ai/Mooncake/pull/4039) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/4039).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkvcache-ai%2FMooncake) You must be signed in to change notification settings
  * [ Fork 1.2k ](https://github.com/login?return_to=%2Fkvcache-ai%2FMooncake)
  * [ Star  6.6k ](https://github.com/login?return_to=%2Fkvcache-ai%2FMooncake)


## Conversation
### 
commented [Sep 11, 2026](https://github.com/kvcache-ai/Mooncake/pull/4039#issue-5421674232) •
edited by github-actions Bot
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/4039).
Copy link  Copy Markdown 
Collaborator
## Description
## Module
  * Transfer Engine (`mooncake-transfer-engine`)
  * Mooncake Store (`mooncake-store`)
  * Reshard (`mooncake-reshard`)
  * Mooncake EP (`mooncake-ep`)
  * Mooncake PG (`mooncake-pg`)
  * Integration (`mooncake-integration`)
  * P2P Store (`mooncake-p2p-store`)
  * Python Wheel (`mooncake-wheel`)
  * Common (`mooncake-common`)
  * Mooncake RL (`mooncake-rl`)
  * CI/CD
  * Docs
  * Other


## Type of Change
  * Bug fix
  * New feature
  * Refactor
  * Breaking change
  * Documentation update
  * Performance improvement
  * Other


## How Has This Been Tested?
**Test commands:**

```
# Example: bash scripts/run_ci_test.sh
```

**Test results:**
  * Unit tests pass
  * Integration tests pass (if applicable)
  * Manual testing done (describe below)


## Checklist
  * I have performed a self-review of my own code
  * I have formatted my code using `./scripts/code_format.sh`
  * I have run pre-commit on the files changed in this PR and all hooks pass
  * I have updated the documentation (if applicable)
  * I have added tests to prove my changes are effective
  * For changes >500 LOC: I have filed an RFC issue


## AI Assistance Disclosure
  * No AI tools were used
  * AI tools were used (specify below)


Opus
` [CI/Build] Enable KV events in release wheels[](https://github.com/kvcache-ai/Mooncake/pull/4039/commits/28020985ff84d0afbcfea0e66d88710fa2484e9f "\[CI/Build\] Enable KV events in release wheels

Signed-off-by: Ishan Dhanani <ishandhanani@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
SSH Key Fingerprint: 52BsfLUviIkvxEXvwmxwnrEsl6vyfT1AgHzf1exrvvY
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/4039).

```
Signed-off-by: Ishan Dhanani <ishandhanani@gmail.com>
```

[ishandhanani](https://github.com/ishandhanani) requested review from [Aionw](https://github.com/Aionw), [Ann-1024](https://github.com/Ann-1024), [luketong777](https://github.com/luketong777), [stmatengss](https://github.com/stmatengss) and [ykwd](https://github.com/ykwd) as [code owners](https://github.com/kvcache-ai/Mooncake/blob/738ab1b7b8b82c2753f7739837a3c3a788ba46de/.github/CODEOWNERS#L9) [September 11, 2026 07:35](https://github.com/kvcache-ai/Mooncake/pull/4039#event-30956325361)
approved these changes  [ Sep 11, 2026 ](https://github.com/kvcache-ai/Mooncake/pull/4039#pullrequestreview-5176072151)
[ View reviewed changes ](https://github.com/kvcache-ai/Mooncake/pull/4039/files/28020985ff84d0afbcfea0e66d88710fa2484e9f)
### 
commented [Sep 11, 2026](https://github.com/kvcache-ai/Mooncake/pull/4039#issuecomment-5631092715)
Copy link  Copy Markdown 
Collaborator  
|  Once CI passed, this PR can be merged.  |  
| --- |  
[github-actions](https://github.com/apps/github-actions) Bot added [ run-ci ](https://github.com/kvcache-ai/Mooncake/issues?q=state%3Aopen%20label%3Arun-ci) [ CI/Build ](https://github.com/kvcache-ai/Mooncake/issues?q=state%3Aopen%20label%3ACI%2FBuild) labels [Sep 11, 2026](https://github.com/kvcache-ai/Mooncake/pull/4039#event-30957022376)
### 
commented [Sep 12, 2026](https://github.com/kvcache-ai/Mooncake/pull/4039#issuecomment-5643146177) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/4039).
Copy link  Copy Markdown 
Collaborator  
|  The minimal fix belongs in `dependencies.sh`: add `zeromq-devel` to the shared RPM `SYSTEM_PACKAGES` list, immediately after `msgpack-devel`. 
```
                      msgpack-devel \
+                     zeromq-devel \
                      libzstd-devel \
```
Keep `ENABLE_KV_EVENTS=ON`; no extra workflow package-install command or smoke-test change is needed. This one-line change replaces all earlier patches and suggestions in this review. `dependencies.sh` is outside the current PR diff, so GitHub cannot attach an applyable inline suggestion to that file yet. The [updated review comment](https://github.com/kvcache-ai/Mooncake/pull/4039#discussion_r3994964466) records the exact edit. Validation: one file, one insertion against PR head `28020985`; Bash syntax and scoped pre-commit passed locally. Linux wheel build/runtime and hosted CI remain unverified. AI assistance: Codex.  |  
| --- |  
reviewed  [ Sep 12, 2026 ](https://github.com/kvcache-ai/Mooncake/pull/4039#pullrequestreview-5185057613)
[ View reviewed changes ](https://github.com/kvcache-ai/Mooncake/pull/4039/files/28020985ff84d0afbcfea0e66d88710fa2484e9f)
### 
left a comment  •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/4039).
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Minimal fix: add exactly one zeromq-devel line to the shared RPM SYSTEM_PACKAGES list in dependencies.sh, keeping ENABLE_KV_EVENTS=ON. The earlier workflow-install and extra smoke-test suggestions have been withdrawn. The local diff is one file and one insertion; Bash syntax and scoped pre-commit passed. dependencies.sh is not in the current PR diff, so the updated review comment supplies the exact patch rather than an Apply suggestion for another file. Linux wheel build/runtime and hosted CI remain unverified. AI assistance: Codex.
Comment thread  
|  if [ -n "$SUDO" ] && command -v apt-get >/dev/null 2>&1; then  |  
| --- |  
|  $SUDO apt-get update -y || true  |  
|  $SUDO bash -x dependencies.sh -y  |  
### 
[Sep 12, 2026](https://github.com/kvcache-ai/Mooncake/pull/4039#discussion_r3994964466) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/4039).
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Minimal fix: add exactly one line, `zeromq-devel`, to the shared RPM `SYSTEM_PACKAGES` list in `dependencies.sh`, immediately after `msgpack-devel` (current head: line 210). Keep `ENABLE_KV_EVENTS=ON`.

```
                      msgpack-devel \
+                     zeromq-devel \
                      libzstd-devel \
```

This replaces the earlier workflow installation suggestion. No additional workflow or wheel-packaging changes are needed for this dependency fix. `dependencies.sh` is not in the current PR diff, so this is a patch for that file, not an Apply suggestion targeting the workflow line above.
Validation: the local diff against PR head `28020985` is exactly one insertion in `dependencies.sh`; Bash syntax and scoped pre-commit passed. Linux wheel build/runtime and hosted CI still need validation after the change.
AI assistance: Codex.
Comment thread  
|  "$smoke_venv/bin/mooncake_master" --version  |  
| --- |  
### 
[Sep 12, 2026](https://github.com/kvcache-ai/Mooncake/pull/4039#discussion_r3994968400) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kvcache-ai/Mooncake/pull/4039).
Copy link  Copy Markdown 
Collaborator
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
Withdrawn: the minimal fix only adds `zeromq-devel` to the shared RPM package list in `dependencies.sh`. No additional smoke-test change is requested. See the [updated one-line patch](https://github.com/kvcache-ai/Mooncake/pull/4039#discussion_r3994964466).
[stmatengss](https://github.com/stmatengss) mentioned this pull request [ Sep 12, 2026 ](https://github.com/kvcache-ai/Mooncake/pull/4039#ref-pullrequest-5432436116)
[ [CI/Build] Enable KV events in wheels with ZeroMQ dependencies #4060 ](https://github.com/kvcache-ai/Mooncake/pull/4060)
Merged 
31 tasks
[ishandhanani](https://github.com/ishandhanani) closed this [Sep 14, 2026](https://github.com/kvcache-ai/Mooncake/pull/4039#event-31081060231)
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fkvcache-ai%2FMooncake%2Fpull%2F4039)
###  Labels 
[ CI/Build ](https://github.com/kvcache-ai/Mooncake/issues?q=state%3Aopen%20label%3ACI%2FBuild) [ run-ci ](https://github.com/kvcache-ai/Mooncake/issues?q=state%3Aopen%20label%3Arun-ci)
###  3 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
