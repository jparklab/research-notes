---
title: 'feat(helm): support structured plugin config by hugosmoreira · Pull Request
  #2620 · llm-d/llm-d-router · GitHub'
id: feathelm-support-structured-plugin-config-by-hugosmoreira-pull-request-2620-llm
tags:
- llmd-router-nonk8s-backends-54b774
- epp-plugin-registration
- helm-chart
created: '2026-09-11T20:58:24.413581Z'
updated: '2026-09-12T03:37:37.744936Z'
source: https://github.com/llm-d/llm-d-router/pull/2620
source_domain: github.com
fetched_at: '2026-09-11T20:58:24.390523Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'PR #2620 ''feat(helm): support structured plugin config'' (opened Aug 30
  2026, merged 2026-09-11T17:51:16Z -- HEAD as of this run). Implements option (b)
  of issue #2358: adds a Helm value router.epp.pluginsConfig that accepts a STRUCTURED
  (map/YAML) EndpointPickerConfig and templates it into pluginsConfigFile, so feature-gate
  overlay values files can share/override plugins and schedulingProfiles defined in
  a base values file (Helm list-replace semantics on merge). All 8 changed files are
  Helm chart artifacts (config/charts/routerlib/templates/_config.yaml, config/charts/routerlib/values.yaml,
  config/charts/README.md, hack/testdata/plugins-config-*.yaml, hack/verify-helm.sh,
  hack/verify-plugins-config.sh) -- confirmed via GitHub API (changed_files=8, +234/-4)
  -- with ZERO changes to any .go file. This PR does NOT touch pkg/epp/framework/interface/plugin/registry.go
  or cmd/epp/runner/runner.go: it does not change how plugin factories are registered,
  does not add an external/out-of-tree registration hook, and has no bearing on the
  discovery-plugin registration question. It only changes how the EndpointPickerConfig
  YAML (which references already-registered plugin types by name) is authored/composed
  via Helm values.'
utility_score: 15.0
---

[Skip to content](https://github.com/llm-d/llm-d-router/pull/2620#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/2620) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/2620) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/pull/2620) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2620).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Conversation
### 
commented [Aug 30, 2026](https://github.com/llm-d/llm-d-router/pull/2620#issue-5293801394)
Copy link  Copy Markdown 
Contributor
**What type of PR is this?**
/kind feature
**What this PR does / why we need it** :
Implements option (b) from [#2358](https://github.com/llm-d/llm-d-router/issues/2358). `router.epp.pluginsConfig` accepts a structured EPP configuration and renders it into `pluginsConfigFile`. A feature-gate overlay can share the plugins and scheduling profiles defined in a base values file.
An empty configuration preserves the built-in files. Existing `pluginsCustomConfig` entries remain supported; using both settings for the same filename produces an error. The documentation explains that structured configuration supplies a complete file and that Helm replaces lists when merging values files.
The chart regression checks run for both the gateway and standalone variants:
  * A feature-gate overlay preserves base plugins and scheduling profiles; list overrides replace the base list.
  * Custom and built-in filenames produce one configuration entry and match the EPP configuration-file argument.
  * Empty configuration and raw custom files preserve the built-in output.
  * Conflicting filenames and non-map structured configuration produce errors.


Validation: `make verify-helm-charts`, strict Helm lint with subcharts, `make presubmit`, and `go test -race -count=1 ./pkg/epp/config/loader` passed with Go 1.26.7. Presubmit ran inside the prepared builder container with `BUILDER_RUN="sh -c"` and `-o image-build-builder` to avoid nesting builder containers. Eight legacy rendering cases matched upstream byte-for-byte. The regression failed on unmodified upstream before passing with the feature.
**Which issue(s) this PR fixes** :
Fixes [#2358](https://github.com/llm-d/llm-d-router/issues/2358). Option (a) was merged in [#2388](https://github.com/llm-d/llm-d-router/pull/2388).
**Release note** _(write`NONE` if no user-facing change)_:

```
The Helm charts support router.epp.pluginsConfig for structured EPP configuration that can be layered across Helm values files.

```

` feat(helm): support structured plugin config[](https://github.com/llm-d/llm-d-router/pull/2620/commits/a79bd0313d2dc0b3a1e47d9636023aa2f4bf64aa "feat\(helm\): support structured plugin config

Signed-off-by: hugosmoreira <hugo.n4@gmail.com>") `
Verified
#  Verified 
This commit was signed with the committer’s **verified signature**. 
[hugosmoreira](https://github.com/hugosmoreira) Hugo Moreira 
SSH Key Fingerprint: m7jE6WCFlcvPX09dRHiS52c+ABbQlzuS6xsFtzoJy3I
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits)
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2620).

```
Signed-off-by: hugosmoreira <hugo.n4@gmail.com>
```

[hugosmoreira](https://github.com/hugosmoreira) requested a review from a team as a [code owner](https://github.com/llm-d/llm-d-router/blob/644a885639ac64ca09d6f35af3a67fe61bcc2e31/CODEOWNERS#L14) [August 30, 2026 18:54](https://github.com/llm-d/llm-d-router/pull/2620#event-30244006353)
[hugosmoreira](https://github.com/hugosmoreira) requested review from [ahg-g](https://github.com/ahg-g) and [vMaroon](https://github.com/vMaroon) [August 30, 2026 18:54](https://github.com/llm-d/llm-d-router/pull/2620#event-30244006477)
[github-actions](https://github.com/apps/github-actions) Bot added [ size/L ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files. [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. [ area/dev ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fdev) Related to dev tooling, experience, etc. and removed [ kind/feature ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Akind%2Ffeature) Categorizes issue or PR as related to a new feature. labels [Aug 30, 2026](https://github.com/llm-d/llm-d-router/pull/2620#event-30244008488)
### 
commented [Aug 31, 2026](https://github.com/llm-d/llm-d-router/pull/2620#issuecomment-5482205925)
Copy link  Copy Markdown 
Collaborator  
|  /assign [@LukeAVanDrie](https://github.com/LukeAVanDrie)  |  
| --- |  
approved these changes  [ Sep 11, 2026 ](https://github.com/llm-d/llm-d-router/pull/2620#pullrequestreview-5181882894)
[ View reviewed changes ](https://github.com/llm-d/llm-d-router/pull/2620/files/a79bd0313d2dc0b3a1e47d9636023aa2f4bf64aa)
Hide details View details
[ahg-g](https://github.com/ahg-g) merged commit [`6493998`](https://github.com/llm-d/llm-d-router/commit/6493998a3735ccf1b07450fe6308c9ec5dd929e1) into  llm-d:main [Sep 11, 2026](https://github.com/llm-d/llm-d-router/pull/2620#event-30990089042)
39 checks passed 
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/pull/2620).
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fllm-d%2Fllm-d-router%2Fpull%2F2620)
###  Labels 
[ area/dev ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Aarea%2Fdev) Related to dev tooling, experience, etc. [ size/L ](https://github.com/llm-d/llm-d-router/issues?q=state%3Aopen%20label%3Asize%2FL) Denotes a PR that changes 100-499 lines, ignoring generated files.
###  2 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
