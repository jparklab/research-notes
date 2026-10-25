---
title: 'docs: add design proposal for migrating EPP to llm-d inference scheduler by
  andyzhangx · Pull Request #1979 · kaito-project/kaito'
id: docs-add-design-proposal-for-migrating-epp-to-llm-d-inference-scheduler-by-andyz
tags:
- gateway-routing
- epp
- kaito
- llm-d
created: '2026-10-09T23:01:07.013710Z'
updated: '2026-10-10T14:52:17.163592Z'
source: https://github.com/kaito-project/kaito/pull/1979
source_domain: github.com
fetched_at: '2026-10-09T23:01:06.990485Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GitHub PR #1979 (kaito-project/kaito, opened April 21 2026 by Microsoft
  engineer andyzhangx) -- the actual primary-source design proposal and implementation
  for migrating Microsoft KAITO''s default Endpoint Picker (EPP) image from the upstream
  Gateway API Inference Extension (GWIE) reference EPP to the llm-d inference scheduler.
  Confirms the claim (repeated in secondary sources in this batch) with the exact
  artifact: new EPP image constants point to mcr.microsoft.com/oss/v2/llm-d/llm-d-inference-scheduler:v0.7.1,
  explicitly commented in the diff as ''Using llm-d inference scheduler which consolidates
  the GWIE EPP implementation with advanced scheduling plugins (KV cache-aware routing,
  P/D disaggregation, etc.)''. States llm-d is a ''drop-in replacement'' using the
  same EndpointPickerConfig API (no breaking changes) with default plugins (queue-scorer,
  kv-cache-utilization-scorer, prefix-cache-scorer) working out of the box against
  KAITO''s existing vLLM metrics; advanced/optional access is added to llm-d-specific
  plugins (precise prefix cache, P/D disaggregation, label-based filtering) via a
  new optional EPPPluginsConfig field. Explicitly frames this as following an upstream
  ''GIE to llm-d migration plan'' (linked GitHub issue kubernetes-sigs/gateway-api-inference-extension#2430),
  i.e., part of a broader project-level plan to consolidate the reference EPP implementation
  into llm-d rather than maintain it separately. A PR-Agent reviewer flags a direct
  discrepancy: the PR describes itself as ''design-only, no code changes'' but the
  diff includes real code changes to consts.go, manifests.go, and tests -- i.e., implementation
  shipped alongside the design doc, not after it. A llm-d maintainer comments mid-thread
  with a load-bearing caveat for anyone building a non-KAITO integration: the InferenceModelRewrite
  and InferenceObjective CRD API groups are expected to move to llm-d''s own API group
  (installed with llm-d CRDs rather than GAIE), and the merged llm-d EPP implementation
  ''may remove the plugin extensibility model in favor of just implementing all existing
  upstream and llm-d-specific plugins directly in-tree'' -- a potential loss of the
  pluggable-scorer extension point other vendors (NVIDIA Dynamo, etc.) currently rely
  on. Audited absence/status finding: despite an associated implementation PR (#1975)
  and MCR image build, this specific design-proposal PR went stale after 90 days of
  inactivity (flagged Aug 31 2026) and was auto-closed Sept 15 2026 without merging
  -- the documented migration did not land as of that date, evidence that the EPP
  consolidation effort stalled or was superseded.'
---

*Suggested by [[envoy-ai-gateway-hits-10-what-an-llm-aware-kubernetes-gateway-actually-load-bala]] — KAITO design proposal for migrating its EPP to the llm-d inference scheduler, named in the bex.co deep dive*

[Skip to content](https://github.com/kaito-project/kaito/pull/1979#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kaito-project/kaito/pull/1979) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kaito-project/kaito/pull/1979) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kaito-project/kaito/pull/1979) to refresh your session. Dismiss alert
## Conversation
### 
commented [Apr 21, 2026](https://github.com/kaito-project/kaito/pull/1979#issue-4301843803) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kaito-project/kaito/pull/1979).
Copy link  Copy Markdown 
Collaborator
## What type of PR is this?
/kind documentation
## What this PR does / why we need it:
Adds a design proposal for migrating KAITO's default Endpoint Picker (EPP) from the upstream Gateway API Inference Extension (GWIE) EPP to the [llm-d inference scheduler](https://github.com/llm-d/llm-d-inference-scheduler).
**I have added optional EPPPluginsConfig in InferenceSetSpec for "Advanced: Custom EPP Plugin Configuration", this is the only API change during migration. And by default, we don't even need to set this EPPPluginsConfig , this is for better scheduling with advanced features.**
### Key points:
  * **Drop-in replacement** : llm-d uses the same `EndpointPickerConfig` API — no breaking changes
  * **Zero config** : Default plugins (queue-scorer, kv-cache-utilization-scorer, prefix-cache-scorer) work out of the box with KAITO's vLLM metrics
  * **Advanced plugins** : Enables access to llm-d-specific plugins (precise prefix cache, P/D disaggregation, label-based filtering)
  * **Upstream alignment** : Follows the [GIE to llm-d migration plan](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/2430)


### Related PRs:
  * Implementation: [feat: Migrate default EPP from GWIE to llm-d inference scheduler #1975](https://github.com/kaito-project/kaito/pull/1975)
  * MCR image build: [Azure/dalec-build-defs#6817](https://github.com/Azure/dalec-build-defs/pull/6817)


## Which issue(s) this PR fixes:
N/A (design proposal)
## Special notes for your reviewer:
This is a design-only proposal with no code changes.
[andyzhangx](https://github.com/andyzhangx) requested review from [Fei-Guo](https://github.com/Fei-Guo), [chewong](https://github.com/chewong) and [zhuangqh](https://github.com/zhuangqh) as [code owners](https://github.com/kaito-project/kaito/blob/1c50dee47a342169a9b4272a26cc998b613727a4/CODEOWNERS#L3) [April 21, 2026 10:06](https://github.com/kaito-project/kaito/pull/1979#event-24713023777)
[github-project-automation](https://github.com/apps/github-project-automation) Bot added this to [KAITO Roadmap](https://github.com/orgs/kaito-project/projects/1) [Apr 21, 2026](https://github.com/kaito-project/kaito/pull/1979#event-24713024929)
[andyzhangx](https://github.com/andyzhangx) [had a problem deploying](https://github.com/kaito-project/kaito/actions/runs/24716521726/job/72294273685) to e2e-test [April 21, 2026 10:06](https://github.com/kaito-project/kaito/pull/1979#event-24713026031) — with [GitHub Actions](https://github.com/apps/github-actions) Failure 
[andyzhangx](https://github.com/andyzhangx) [had a problem deploying](https://github.com/kaito-project/kaito/actions/runs/24716521726/job/72294273694) to e2e-test [April 21, 2026 10:06](https://github.com/kaito-project/kaito/pull/1979#event-24713026044) — with [GitHub Actions](https://github.com/apps/github-actions) Failure 
[andyzhangx](https://github.com/andyzhangx) [temporarily deployed](https://github.com/kaito-project/kaito/actions/runs/24716521776/job/72294273847) to unit-tests [April 21, 2026 10:06](https://github.com/kaito-project/kaito/pull/1979#event-24713026054) — with [GitHub Actions](https://github.com/apps/github-actions) Inactive 
[andyzhangx](https://github.com/andyzhangx) [temporarily deployed](https://github.com/kaito-project/kaito/actions/runs/24716521770/job/72294273696) to unit-tests [April 21, 2026 10:06](https://github.com/kaito-project/kaito/pull/1979#event-24713026096) — with [GitHub Actions](https://github.com/apps/github-actions) Inactive 
[andyzhangx](https://github.com/andyzhangx) [had a problem deploying](https://github.com/kaito-project/kaito/actions/runs/24716521728/job/72294273845) to e2e-test [April 21, 2026 10:06](https://github.com/kaito-project/kaito/pull/1979#event-24713026098) — with [GitHub Actions](https://github.com/apps/github-actions) Error 
### 
commented [Apr 21, 2026](https://github.com/kaito-project/kaito/pull/1979#issuecomment-4287669790)
Copy link  Copy Markdown 
Contributor  
| 
## Title
Migrate default EPP to llm-d inference scheduler and update documentation
### **Description**
  * Migrate default Endpoint Picker (EPP) image to llm-d inference scheduler.
  * Update constants and Helm release generation for new image structure.
  * Add design proposal document detailing migration strategy and plugins.
  * Update user documentation to reflect llm-d EPP implementation details.


###  **Changes walkthrough** 📝  
 | Relevant files  |  
| --- |  
| **Enhancement**  |   
 |  **consts.go**     `Update EPP image constants for llm-d scheduler` pkg/utils/consts/consts.go
* Replaced single image repository constant with hub, name, and tag constants. 
* Updated constants to point to llm-d inference scheduler image. 
* Added comments explaining the migration to llm-d scheduler. 
 |  
| --- |  
|  **manifests.go**     `Update Helm release generation for new EPP image` pkg/workspace/manifests/manifests.go
* Updated GenerateInferencePoolHelmRelease to use new image constants. 
* Modified Helm values to include hub, name, and tag fields. 
* Ensures Helm chart receives correct llm-d scheduler image details. 
 |  
 |  
| **Tests**  |   
 |  **manifests_test.go**     `Update tests for new EPP image constants` pkg/workspace/manifests/manifests_test.go
* Updated test expectations to match new image constants. 
* Modified Helm values assertions in test cases. 
* Ensures tests validate the correct llm-d scheduler configuration. 
 |  
| --- |  
 |  
| **Documentation**  |   
 |  **20260421-migrate-epp-to-llm-d-inference-scheduler.md**     `Add design proposal for EPP migration to llm-d` docs/proposals/20260421-migrate-epp-to-llm-d-inference-scheduler.md
* Added design proposal for migrating EPP to llm-d scheduler. 
* Documented architecture, image changes, and plugin compatibility. 
* Explained advanced plugins like precise prefix cache and P/D disaggregation. 
 |  
| --- |  
|  **gateway-api-inference-extension.md**     `Update Gateway API Inference Extension documentation` website/docs/gateway-api-inference-extension.md
* Updated documentation to mention llm-d integration. 
* Clarified EPP image override in HelmRelease description. 
* Modified verification steps to reflect new EPP image. 
 |  
 |  
> Need help?
> * Type `/help how to ...` in the comments thread for any questions about PR-Agent usage.
> * Check out the [documentation](https://qodo-merge-docs.qodo.ai/usage-guide/) for more information.
 |  
[kaito-pr-agent](https://github.com/apps/kaito-pr-agent) Bot added the [ Review effort 3/5 ](https://github.com/kaito-project/kaito/issues?q=state%3Aopen%20label%3A%22Review%20effort%203%2F5%22) label [Apr 21, 2026](https://github.com/kaito-project/kaito/pull/1979#event-24713082856)
### 
commented [Apr 21, 2026](https://github.com/kaito-project/kaito/pull/1979#issuecomment-4287675105)
Copy link  Copy Markdown 
Contributor  
| 
## PR Reviewer Guide 🔍
Here are some key observations to aid the review process:  
 | ⏱️ **Estimated effort to review** : 3 🔵🔵🔵⚪⚪ |  
| --- |  
| 🧪 **PR contains tests**  |  
| 🔒 **No security concerns identified**  |  
| ⚡ **Recommended focus areas for review** [**PR Description Mismatch**](https://github.com/kaito-project/kaito/pull/1979/files#diff-b591626b039f6bb7fe8aebf16f6f940e240dbc580024668ca2b05732cbba5081R88-R95) The PR description explicitly states "This is a design-only proposal with no code changes." However, the diff includes significant code changes in `consts.go`, `manifests.go`, and `manifests_test.go`. This discrepancy should be clarified to ensure the PR is correctly categorized and reviewed as an implementation PR rather than just a documentation proposal. 
```
// EPP (Endpoint Picker) image configuration.
// The InferencePool chart composes the image as: {hub}/{name}:{tag}
// Using llm-d inference scheduler which consolidates the GWIE EPP implementation
// with advanced scheduling plugins (KV cache-aware routing, P/D disaggregation, etc.)
// See: https://github.com/llm-d/llm-d-inference-scheduler
EPPImageHub  = "mcr.microsoft.com/oss/v2/llm-d"
EPPImageName = "llm-d-inference-scheduler"
EPPImageTag  = "v0.7.1"
```
[**Image Version Tracking**](https://github.com/kaito-project/kaito/pull/1979/files#diff-b591626b039f6bb7fe8aebf16f6f940e240dbc580024668ca2b05732cbba5081R95-R95) The `EPPImageTag` constant is hardcoded to `v0.7.1`. Unlike `InferencePoolChartVersion`, there is no comment indicating it must be kept in sync with a specific dependency or go.mod version. Consider adding a similar comment or ensuring there is a clear process for updating this tag when the llm-d inference scheduler releases new versions to avoid drift. 
```
EPPImageTag  = "v0.7.1"
```
[**Helm Chart Compatibility**](https://github.com/kaito-project/kaito/pull/1979/files#diff-5857e0b2f1941c47dd88c752fab691c28690d8ca2c3a497dbc78a7ed4b23d4b4R383-R387) The code change updates the Helm values structure to use `hub`, `name`, and `tag` fields for the EPP image. Verify that the InferencePool Helm chart version `v1.3.1` (specified in `InferencePoolChartVersion`) fully supports this image composition structure to ensure the HelmRelease renders correctly without errors. 
```
	"hub":        consts.EPPImageHub,
	"name":       consts.EPPImageName,
	"tag":        consts.EPPImageTag,
	"pullPolicy": string(corev1.PullIfNotPresent),
},
```
 |  
 |  
[andyzhangx](https://github.com/andyzhangx) [had a problem deploying](https://github.com/kaito-project/kaito/actions/runs/24716917254/job/72295620225) to e2e-test [April 21, 2026 10:16](https://github.com/kaito-project/kaito/pull/1979#event-24713337034) — with [GitHub Actions](https://github.com/apps/github-actions) Failure 
[andyzhangx](https://github.com/andyzhangx) [had a problem deploying](https://github.com/kaito-project/kaito/actions/runs/24716917254/job/72295620221) to e2e-test [April 21, 2026 10:16](https://github.com/kaito-project/kaito/pull/1979#event-24713337037) — with [GitHub Actions](https://github.com/apps/github-actions) Failure 
[andyzhangx](https://github.com/andyzhangx) [temporarily deployed](https://github.com/kaito-project/kaito/actions/runs/24716917234/job/72295620134) to unit-tests [April 21, 2026 10:16](https://github.com/kaito-project/kaito/pull/1979#event-24713337044) — with [GitHub Actions](https://github.com/apps/github-actions) Inactive 
[andyzhangx](https://github.com/andyzhangx) [temporarily deployed](https://github.com/kaito-project/kaito/actions/runs/24716917182/job/72295620396) to unit-tests [April 21, 2026 10:16](https://github.com/kaito-project/kaito/pull/1979#event-24713337073) — with [GitHub Actions](https://github.com/apps/github-actions) Inactive 
[andyzhangx](https://github.com/andyzhangx) [had a problem deploying](https://github.com/kaito-project/kaito/actions/runs/24716917218/job/72295621983) to e2e-test [April 21, 2026 10:16](https://github.com/kaito-project/kaito/pull/1979#event-24713337425) — with [GitHub Actions](https://github.com/apps/github-actions) Failure 
[andyzhangx](https://github.com/andyzhangx) added 2 commits [April 21, 2026 13:44](https://github.com/kaito-project/kaito/pull/1979#commits-pushed-301391b)
` docs: add design proposal for migrating EPP to llm-d inference scheduler[](https://github.com/kaito-project/kaito/pull/1979/commits/301391bed854174a421d462ca455f6a7eb49afa9 "docs: add design proposal for migrating EPP to llm-d inference scheduler") `
` docs: remove code-level details from design proposal[](https://github.com/kaito-project/kaito/pull/1979/commits/847afe30dae6bf77cb5c6f48aef62192aa201207 "docs: remove code-level details from design proposal") `
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kaito-project/kaito/pull/1979).
[andyzhangx](https://github.com/andyzhangx) [force-pushed](https://github.com/kaito-project/kaito/compare/55a73f01b97577a8070a3e17a5f65186a7a2d7a7..847afe30dae6bf77cb5c6f48aef62192aa201207) the  migrate-epp-to-llm-d-design-doc branch from [`55a73f0`](https://github.com/kaito-project/kaito/commit/55a73f01b97577a8070a3e17a5f65186a7a2d7a7) to [`847afe3`](https://github.com/kaito-project/kaito/commit/847afe30dae6bf77cb5c6f48aef62192aa201207) [ Compare ](https://github.com/kaito-project/kaito/compare/55a73f01b97577a8070a3e17a5f65186a7a2d7a7..847afe30dae6bf77cb5c6f48aef62192aa201207) [April 21, 2026 13:44](https://github.com/kaito-project/kaito/pull/1979#event-24719895022)
[andyzhangx](https://github.com/andyzhangx) [had a problem deploying](https://github.com/kaito-project/kaito/actions/runs/24725867612/job/72326618868) to e2e-test [April 21, 2026 13:44](https://github.com/kaito-project/kaito/pull/1979#event-24719897840) — with [GitHub Actions](https://github.com/apps/github-actions) Failure 
[andyzhangx](https://github.com/andyzhangx) [had a problem deploying](https://github.com/kaito-project/kaito/actions/runs/24725867612/job/72326618874) to e2e-test [April 21, 2026 13:44](https://github.com/kaito-project/kaito/pull/1979#event-24719897882) — with [GitHub Actions](https://github.com/apps/github-actions) Failure 
[andyzhangx](https://github.com/andyzhangx) requested a review from [Copilot](https://github.com/apps/copilot-pull-request-reviewer) [April 21, 2026 13:57](https://github.com/kaito-project/kaito/pull/1979#event-24720392064)
**Copilot** [started reviewing](https://github.com/kaito-project/kaito/sessions/e7fc0874-9a09-46b8-b929-5e1f3155b4e1 "View session") on behalf of [andyzhangx](https://github.com/andyzhangx) [April 21, 2026 13:58](https://github.com/kaito-project/kaito/pull/1979#event-24720425367) [ View session ](https://github.com/kaito-project/kaito/sessions/e7fc0874-9a09-46b8-b929-5e1f3155b4e1)
reviewed  [ Apr 21, 2026 ](https://github.com/kaito-project/kaito/pull/1979#pullrequestreview-4148268767)
[ View reviewed changes ](https://github.com/kaito-project/kaito/pull/1979/files/847afe30dae6bf77cb5c6f48aef62192aa201207)
### 
left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
## Pull request overview
Adds a design proposal documenting the planned migration of KAITO’s default Endpoint Picker (EPP) implementation from GWIE’s EPP to the llm-d inference scheduler, focusing on compatibility, default behavior, and advanced plugin configuration paths.
**Changes:**
  * Introduces a new proposal document describing the migration rationale, architecture, and request flow.
  * Documents default plugin/metrics compatibility and examples for advanced llm-d plugin configurations.
  * Captures an implementation strategy and references relevant upstream and related PRs.


💡 [Add Copilot custom instructions](https://github.com/kaito-project/kaito/new/main?filename=.github/instructions/*.instructions.md) for smarter, more guided reviews. [Learn how to get started](https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot).
Positive feedbackNegative feedback
Comment thread [docs/proposals/20260421-migrate-epp-to-llm-d-inference-scheduler.md](https://github.com/kaito-project/kaito/pull/1979/files/847afe30dae6bf77cb5c6f48aef62192aa201207#diff-a9b31f94a23a5df8d350f164c10b03ca2d3c74be965c16d1196137cf1396a504) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kaito-project/kaito/pull/1979).
Comment thread [docs/proposals/20260421-migrate-epp-to-llm-d-inference-scheduler.md](https://github.com/kaito-project/kaito/pull/1979/files/847afe30dae6bf77cb5c6f48aef62192aa201207#diff-a9b31f94a23a5df8d350f164c10b03ca2d3c74be965c16d1196137cf1396a504) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kaito-project/kaito/pull/1979).
Comment thread [docs/proposals/20260421-migrate-epp-to-llm-d-inference-scheduler.md](https://github.com/kaito-project/kaito/pull/1979/files/847afe30dae6bf77cb5c6f48aef62192aa201207#diff-a9b31f94a23a5df8d350f164c10b03ca2d3c74be965c16d1196137cf1396a504) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kaito-project/kaito/pull/1979).
` Fix review: absolute path, pinned chart version, upstream link[](https://github.com/kaito-project/kaito/pull/1979/commits/8ae02881ed7d9084669660899750afa3d3e59bb9 "Fix review: absolute path, pinned chart version, upstream link") `
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kaito-project/kaito/pull/1979).
[andyzhangx](https://github.com/andyzhangx) [had a problem deploying](https://github.com/kaito-project/kaito/actions/runs/24726925627/job/72330376144) to e2e-test [April 21, 2026 14:05](https://github.com/kaito-project/kaito/pull/1979#event-24720693211) — with [GitHub Actions](https://github.com/apps/github-actions) Failure 
[andyzhangx](https://github.com/andyzhangx) [had a problem deploying](https://github.com/kaito-project/kaito/actions/runs/24726925627/job/72330376119) to e2e-test [April 21, 2026 14:05](https://github.com/kaito-project/kaito/pull/1979#event-24720693217) — with [GitHub Actions](https://github.com/apps/github-actions) Failure 
[andyzhangx](https://github.com/andyzhangx) requested a review from [Copilot](https://github.com/apps/copilot-pull-request-reviewer) [April 21, 2026 14:15](https://github.com/kaito-project/kaito/pull/1979#event-24721031078)
**Copilot** [started reviewing](https://github.com/kaito-project/kaito/sessions/3dcebb86-8226-4bd2-90a8-26cfa418b70c "View session") on behalf of [andyzhangx](https://github.com/andyzhangx) [April 21, 2026 14:15](https://github.com/kaito-project/kaito/pull/1979#event-24721048511) [ View session ](https://github.com/kaito-project/kaito/sessions/3dcebb86-8226-4bd2-90a8-26cfa418b70c)
` proposal: rename eppPluginsConfigRef to eppPluginsConfig (string type…[](https://github.com/kaito-project/kaito/pull/1979/commits/46cade4ee92150dd731dffa3ffde8ba597c27408 "proposal: rename eppPluginsConfigRef to eppPluginsConfig \(string type\) for consistency") `
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kaito-project/kaito/pull/1979).

```
…) for consistency
```

[andyzhangx](https://github.com/andyzhangx) [had a problem deploying](https://github.com/kaito-project/kaito/actions/runs/24983168060/job/73149940740) to e2e-test [April 27, 2026 07:54](https://github.com/kaito-project/kaito/pull/1979#event-24895695990) — with [GitHub Actions](https://github.com/apps/github-actions) Failure 
[andyzhangx](https://github.com/andyzhangx) [had a problem deploying](https://github.com/kaito-project/kaito/actions/runs/24983168060/job/73149940843) to e2e-test [April 27, 2026 07:54](https://github.com/kaito-project/kaito/pull/1979#event-24895695993) — with [GitHub Actions](https://github.com/apps/github-actions) Failure 
` proposal: simplify eppPluginsConfig update behavior - track name chan…[](https://github.com/kaito-project/kaito/pull/1979/commits/dfd8903065daac118cee371a78999870425bab29 "proposal: simplify eppPluginsConfig update behavior - track name change only") `
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kaito-project/kaito/pull/1979).

```
…ge only
```

[andyzhangx](https://github.com/andyzhangx) [had a problem deploying](https://github.com/kaito-project/kaito/actions/runs/24983297663/job/73150358965) to e2e-test [April 27, 2026 07:57](https://github.com/kaito-project/kaito/pull/1979#event-24895809306) — with [GitHub Actions](https://github.com/apps/github-actions) Failure 
[andyzhangx](https://github.com/andyzhangx) [had a problem deploying](https://github.com/kaito-project/kaito/actions/runs/24983297663/job/73150358974) to e2e-test [April 27, 2026 07:57](https://github.com/kaito-project/kaito/pull/1979#event-24895809309) — with [GitHub Actions](https://github.com/apps/github-actions) Failure 
[andyzhangx](https://github.com/andyzhangx) requested a review from [Copilot](https://github.com/apps/copilot-pull-request-reviewer) [April 27, 2026 07:58](https://github.com/kaito-project/kaito/pull/1979#event-24895845629)
**Copilot** [started reviewing](https://github.com/kaito-project/kaito/sessions/1cb2acde-17d6-450c-9330-81c125bf40c8 "View session") on behalf of [andyzhangx](https://github.com/andyzhangx) [April 27, 2026 07:58](https://github.com/kaito-project/kaito/pull/1979#event-24895860611) [ View session ](https://github.com/kaito-project/kaito/sessions/1cb2acde-17d6-450c-9330-81c125bf40c8)
reviewed  [ Apr 27, 2026 ](https://github.com/kaito-project/kaito/pull/1979#pullrequestreview-4179138805)
[ View reviewed changes ](https://github.com/kaito-project/kaito/pull/1979/files/dfd8903065daac118cee371a78999870425bab29)
### 
left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
## Pull request overview
Adds a design proposal document describing how KAITO would migrate its default Endpoint Picker (EPP) implementation from the upstream Gateway API Inference Extension (GWIE) EPP to the llm-d inference scheduler, including expected default behavior and an optional future InferenceSet API extension for advanced plugin configuration.
**Changes:**
  * Introduces a new design proposal covering architecture, migration strategy, and request flow.
  * Documents default plugin/metrics expectations and an “advanced custom plugin config” UX via a ConfigMap reference.
  * Outlines staged implementation steps and references related upstream work.


💡 [Add Copilot custom instructions](https://github.com/kaito-project/kaito/new/main?filename=.github/instructions/*.instructions.md) for smarter, more guided reviews. [Learn how to get started](https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot).
Positive feedbackNegative feedback
Comment thread [docs/proposals/20260421-migrate-epp-to-llm-d-inference-scheduler.md](https://github.com/kaito-project/kaito/pull/1979/files/dfd8903065daac118cee371a78999870425bab29#diff-a9b31f94a23a5df8d350f164c10b03ca2d3c74be965c16d1196137cf1396a504) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kaito-project/kaito/pull/1979).
Comment thread [docs/proposals/20260421-migrate-epp-to-llm-d-inference-scheduler.md](https://github.com/kaito-project/kaito/pull/1979/files/dfd8903065daac118cee371a78999870425bab29#diff-a9b31f94a23a5df8d350f164c10b03ca2d3c74be965c16d1196137cf1396a504) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kaito-project/kaito/pull/1979).
` proposal: remove stale ConfigMap watch reference in controller behavior[](https://github.com/kaito-project/kaito/pull/1979/commits/50ee07e765139a82ebe8cfc84b954c0237b977d8 "proposal: remove stale ConfigMap watch reference in controller behavior") `
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kaito-project/kaito/pull/1979).
[andyzhangx](https://github.com/andyzhangx) [had a problem deploying](https://github.com/kaito-project/kaito/actions/runs/24983715393/job/73151721788) to e2e-test [April 27, 2026 08:07](https://github.com/kaito-project/kaito/pull/1979#event-24896166957) — with [GitHub Actions](https://github.com/apps/github-actions) Failure 
[andyzhangx](https://github.com/andyzhangx) [had a problem deploying](https://github.com/kaito-project/kaito/actions/runs/24983715393/job/73151721814) to e2e-test [April 27, 2026 08:07](https://github.com/kaito-project/kaito/pull/1979#event-24896166977) — with [GitHub Actions](https://github.com/apps/github-actions) Failure 
[andyzhangx](https://github.com/andyzhangx) requested a review from [Copilot](https://github.com/apps/copilot-pull-request-reviewer) [April 27, 2026 08:11](https://github.com/kaito-project/kaito/pull/1979#event-24896319904)
**Copilot** [started reviewing](https://github.com/kaito-project/kaito/sessions/a5904e7d-18e9-401b-b135-133dde25a706 "View session") on behalf of [andyzhangx](https://github.com/andyzhangx) [April 27, 2026 08:11](https://github.com/kaito-project/kaito/pull/1979#event-24896337865) [ View session ](https://github.com/kaito-project/kaito/sessions/a5904e7d-18e9-401b-b135-133dde25a706)
reviewed  [ Apr 27, 2026 ](https://github.com/kaito-project/kaito/pull/1979#pullrequestreview-4179214664)
[ View reviewed changes ](https://github.com/kaito-project/kaito/pull/1979/files/50ee07e765139a82ebe8cfc84b954c0237b977d8)
### 
left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
## Pull request overview
Adds a design proposal describing how KAITO would migrate its default Endpoint Picker (EPP) from the upstream Gateway API Inference Extension (GWIE) EPP implementation to the llm-d inference scheduler, while keeping the InferencePool chart/CRDs unchanged and documenting an optional advanced plugin-config UX.
**Changes:**
  * Introduces a new proposal document outlining the llm-d EPP migration architecture and “zero-config” default behavior.
  * Documents an optional `eppPluginsConfig` InferenceSetSpec field concept for advanced/custom llm-d plugin configurations via ConfigMap reference.
  * Captures operational details (metrics expectations, config update behavior, Istio DestinationRule requirement) and a phased implementation plan.


💡 [Add Copilot custom instructions](https://github.com/kaito-project/kaito/new/main?filename=.github/instructions/*.instructions.md) for smarter, more guided reviews. [Learn how to get started](https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot).
Positive feedbackNegative feedback
[andyzhangx](https://github.com/andyzhangx) requested a review from [Copilot](https://github.com/apps/copilot-pull-request-reviewer) [April 27, 2026 08:25](https://github.com/kaito-project/kaito/pull/1979#event-24896806579)
**Copilot** [started reviewing](https://github.com/kaito-project/kaito/sessions/d8b94106-ab0c-4ab1-be91-45894fb42210 "View session") on behalf of [andyzhangx](https://github.com/andyzhangx) [April 27, 2026 08:26](https://github.com/kaito-project/kaito/pull/1979#event-24896826756) [ View session ](https://github.com/kaito-project/kaito/sessions/d8b94106-ab0c-4ab1-be91-45894fb42210)
reviewed  [ Apr 27, 2026 ](https://github.com/kaito-project/kaito/pull/1979#pullrequestreview-4179317854)
[ View reviewed changes ](https://github.com/kaito-project/kaito/pull/1979/files/50ee07e765139a82ebe8cfc84b954c0237b977d8)
### 
left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
## Pull request overview
Adds a design proposal describing how KAITO would migrate its default Endpoint Picker (EPP) implementation from the upstream Gateway API Inference Extension (GWIE) EPP to the llm-d inference scheduler, including default behavior and an “advanced” path for custom plugin configuration.
**Changes:**
  * Introduce a new design proposal document outlining the llm-d migration approach and architecture.
  * Document default (zero-config) behavior, metrics expectations, and request flow.
  * Propose an optional `eppPluginsConfig` InferenceSet API field for advanced/custom llm-d plugin configuration.


💡 [Add Copilot custom instructions](https://github.com/kaito-project/kaito/new/main?filename=.github/instructions/*.instructions.md) for smarter, more guided reviews. [Learn how to get started](https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot).
Positive feedbackNegative feedback
Comment thread [docs/proposals/20260421-migrate-epp-to-llm-d-inference-scheduler.md](https://github.com/kaito-project/kaito/pull/1979/files/50ee07e765139a82ebe8cfc84b954c0237b977d8#diff-a9b31f94a23a5df8d350f164c10b03ca2d3c74be965c16d1196137cf1396a504) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kaito-project/kaito/pull/1979).
Comment thread [docs/proposals/20260421-migrate-epp-to-llm-d-inference-scheduler.md](https://github.com/kaito-project/kaito/pull/1979/files/50ee07e765139a82ebe8cfc84b954c0237b977d8#diff-a9b31f94a23a5df8d350f164c10b03ca2d3c74be965c16d1196137cf1396a504) Outdated  Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kaito-project/kaito/pull/1979).
Comment thread [docs/proposals/20260421-migrate-epp-to-llm-d-inference-scheduler.md](https://github.com/kaito-project/kaito/pull/1979/files/50ee07e765139a82ebe8cfc84b954c0237b977d8#diff-a9b31f94a23a5df8d350f164c10b03ca2d3c74be965c16d1196137cf1396a504) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kaito-project/kaito/pull/1979).
` proposal: clarify API changes are proposed, fix Gateway deployment wo…[](https://github.com/kaito-project/kaito/pull/1979/commits/f6ac6fe3ce15eef9f18d8f4d79c03ff402547daa "proposal: clarify API changes are proposed, fix Gateway deployment wording") `
Loading
Loading status checks…
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kaito-project/kaito/pull/1979).

```
…rding
```

[andyzhangx](https://github.com/andyzhangx) [had a problem deploying](https://github.com/kaito-project/kaito/actions/runs/24984886346/job/73155592703) to e2e-test [April 27, 2026 08:35](https://github.com/kaito-project/kaito/pull/1979#event-24897170350) — with [GitHub Actions](https://github.com/apps/github-actions) Failure 
[andyzhangx](https://github.com/andyzhangx) [had a problem deploying](https://github.com/kaito-project/kaito/actions/runs/24984886346/job/73155592711) to e2e-test [April 27, 2026 08:35](https://github.com/kaito-project/kaito/pull/1979#event-24897170370) — with [GitHub Actions](https://github.com/apps/github-actions) Failure 
[andyzhangx](https://github.com/andyzhangx) requested a review from [Copilot](https://github.com/apps/copilot-pull-request-reviewer) [April 27, 2026 08:40](https://github.com/kaito-project/kaito/pull/1979#event-24897349515)
**Copilot** [started reviewing](https://github.com/kaito-project/kaito/sessions/8aae8efd-c53e-43c1-9024-8ac326330b5d "View session") on behalf of [andyzhangx](https://github.com/andyzhangx) [April 27, 2026 08:41](https://github.com/kaito-project/kaito/pull/1979#event-24897367091) [ View session ](https://github.com/kaito-project/kaito/sessions/8aae8efd-c53e-43c1-9024-8ac326330b5d)
reviewed  [ Apr 27, 2026 ](https://github.com/kaito-project/kaito/pull/1979#pullrequestreview-4179414175)
[ View reviewed changes ](https://github.com/kaito-project/kaito/pull/1979/files/f6ac6fe3ce15eef9f18d8f4d79c03ff402547daa)
### 
left a comment 
Copy link  Copy Markdown 
Contributor
There was a problem hiding this comment.
### Choose a reason for hiding this comment
The reason will be displayed to describe this comment to others. [Learn more](https://docs.github.com/articles/managing-disruptive-comments/#hiding-a-comment). 
## Pull request overview
Adds a design proposal describing how KAITO will migrate the default Endpoint Picker (EPP) implementation from the upstream Gateway API Inference Extension (GWIE) EPP image to the llm-d inference scheduler, while keeping the InferencePool chart/CRDs unchanged and outlining an optional future API extension for advanced plugin configuration.
**Changes:**
  * Introduces a new proposal document covering motivation, architecture, compatibility, and migration steps for adopting llm-d as the default EPP.
  * Documents default plugin behavior/required vLLM metrics and provides examples for advanced llm-d plugin configurations via a referenced ConfigMap.
  * Describes an (optional, follow-up) InferenceSetSpec field for custom EPP plugin configuration and expected controller behavior.


💡 [Add Copilot custom instructions](https://github.com/kaito-project/kaito/new/main?filename=.github/instructions/*.instructions.md) for smarter, more guided reviews. [Learn how to get started](https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot).
Positive feedbackNegative feedback
Comment thread [docs/proposals/20260421-migrate-epp-to-llm-d-inference-scheduler.md](https://github.com/kaito-project/kaito/pull/1979/files/f6ac6fe3ce15eef9f18d8f4d79c03ff402547daa#diff-a9b31f94a23a5df8d350f164c10b03ca2d3c74be965c16d1196137cf1396a504) Show resolved Hide resolved
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kaito-project/kaito/pull/1979).
### 
commented [Jun 1, 2026](https://github.com/kaito-project/kaito/pull/1979#issuecomment-4596618812) •
edited 
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kaito-project/kaito/pull/1979).
Copy link  Copy Markdown   
|  Let me know if you encounter any difficulties with this approach, especially if you intend to use llm-d's EPP implementation in a standalone manner with serving backends other than llm-d. A few impactful changes to note are that the API groups for the InferenceModelRewrite and InferenceObjective CRDs will move to llm-d's API group (and I expect would be installed with the llm-d CRDs rather than with GAIE) and that the merged EPP implementation in llm-d may _remove_ the plugin extensibility model in favor of just implementing all existing upstream and llm-d-specific plugins directly in-tree.  |  
| --- |  
### 
commented [Aug 31, 2026](https://github.com/kaito-project/kaito/pull/1979#issuecomment-5472132514)
Copy link  Copy Markdown 
Contributor  
|  This pull request has been automatically marked as stale because it has not had activity in the last 90 days. It will be closed in 14 days if no further activity occurs. Thank you for your contributions.  |  
| --- |  
[github-actions](https://github.com/apps/github-actions) Bot added the [ stale ](https://github.com/kaito-project/kaito/issues?q=state%3Aopen%20label%3Astale) label [Aug 31, 2026](https://github.com/kaito-project/kaito/pull/1979#event-30250406600)
### 
commented [Sep 15, 2026](https://github.com/kaito-project/kaito/pull/1979#issuecomment-5672630532)
Copy link  Copy Markdown 
Contributor  
|  This pull request has been automatically closed due to inactivity. Please reopen if this is still relevant.  |  
| --- |  
[github-actions](https://github.com/apps/github-actions) Bot closed this [Sep 15, 2026](https://github.com/kaito-project/kaito/pull/1979#event-31137968311)
[github-project-automation](https://github.com/apps/github-project-automation) Bot moved this to **Done** in [KAITO Roadmap](https://github.com/orgs/kaito-project/projects/1) [Sep 15, 2026](https://github.com/kaito-project/kaito/pull/1979#event-31138082603)
Hide environments Show environments
### This branch had an error being deployed
1 failed and 1 inactive (outdated) deployments 
e2e-test — `f6ac6fe3` [Deployed](https://github.com/kaito-project/kaito/actions/runs/24984886346/job/73155592711) Apr 27, 2026 by [andyzhangx](https://github.com/andyzhangx) via [chart-upgrade-test-byo #1150](https://github.com/kaito-project/kaito/actions/runs/24984886346)
unit-tests — `55a73f01` [Deployed](https://github.com/kaito-project/kaito/actions/runs/24716917182/job/72295620396) Apr 21, 2026 by [andyzhangx](https://github.com/andyzhangx) via [unit-tests #2491](https://github.com/kaito-project/kaito/actions/runs/24716917182)
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fkaito-project%2Fkaito%2Fpull%2F1979)
###  Labels 
[ Review effort 3/5 ](https://github.com/kaito-project/kaito/issues?q=state%3Aopen%20label%3A%22Review%20effort%203%2F5%22) [ stale ](https://github.com/kaito-project/kaito/issues?q=state%3Aopen%20label%3Astale)
###  4 participants 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
