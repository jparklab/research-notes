---
title: Release v1.4.0 · kubernetes-sigs/gateway-api-inference-extension · GitHub
id: release-v140-kubernetes-sigsgateway-api-inference-extension-github
tags:
- llmd-router-nonk8s
- llmd-router-nonk8s-backends-54b774
- epp-releases
- fail-open
created: '2026-09-11T19:43:35.505461Z'
updated: '2026-09-15T04:55:23.690997Z'
source: https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v1.4.0
source_domain: github.com
fetched_at: '2026-09-11T19:43:35.485389Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GIE v1.4.0 release notes, released by @danehans on Mar 20 (2026). Release
  Highlights: standalone chart work landed and is included in release artifacts; conformance
  split into its own Go module; InferencePool/Helm/gRPC improvements including appProtocol,
  FailOpen, and ALPN h2; ongoing work in flow control, BBR, predicted latency, and
  datalayer internals. Merged PRs of note: ''Update InferencePool helm chart to use
  FailOpen as default'' (#2365, @RyanRosario) — confirms FailOpen became the Helm
  chart''s default failure-mode setting at this release; ''Refactor datalayer interfaces,
  separating polling and notification based types'' (#2407, @elevran) — the split
  that produced the current PollingDataSource vs NotificationSource interfaces documented
  in pkg/epp/framework/interface/datalayer. Per pkg.go.dev, BindNotificationSource
  and ValidateAndOrderDataDependencies in pkg/epp/datalayer were both ''added in v1.4.0.'''
utility_score: 12.0
---

[Skip to content](https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v1.4.0#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v1.4.0) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v1.4.0) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v1.4.0) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v1.4.0).
/ **[gateway-api-inference-extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension) You must be signed in to change notification settings
  * [ Fork 313 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)
  * [ Star  766 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)


# v1.4.0
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v1.4.0).
## No results found
[danehans](https://github.com/danehans) released this  20 Mar 04:48 
· [ 347 commits ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/compare/v1.4.0...main) to main since this release 
[ v1.4.0  ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/v1.4.0)
This tag was signed with the committer’s **verified signature**. 
[danehans](https://github.com/danehans) Daneyon Hansen 
GPG key ID: A9BCCF56D7755933
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
[ `6e787dd`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/6e787dd396f0228c31cf6fc5187a13c6b4991a0b)
This commit was signed with the committer’s **verified signature**. 
[danehans](https://github.com/danehans) Daneyon Hansen 
GPG key ID: A9BCCF56D7755933
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
## Release Highlights
  * Standalone chart work landed and is included in release artifacts
  * Conformance was split into its own Go module
  * InferencePool / Helm / gRPC-related improvements landed, including `appProtocol`, `FailOpen`, and ALPN `h2`
  * Significant ongoing work landed in flow control, BBR, predicted latency, and datalayer internals


## What's Changed
  * cleanup: resolve technical debt and link tracking issues by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2083](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2083)
  * Removing dead code that throws an err when no match is found by [@kfswain](https://github.com/kfswain) in [#2088](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2088)
  * cleanup: rename integration test utilities to remove _test suffix by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2084](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2084)
  * Fixed targetPorts copy error by [@capri-xiyue](https://github.com/capri-xiyue) in [#2092](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2092)
  * Add PR write permissions to label checker GHA, as it cannot add label… by [@kfswain](https://github.com/kfswain) in [#2094](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2094)
  * clean unused interface by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2098](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2098)
  * prefill aware prefix plugin by [@ahg-g](https://github.com/ahg-g) in [#2104](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2104)
  * Removing perm-restricted GHA by [@kfswain](https://github.com/kfswain) in [#2105](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2105)
  * Updating vllm versions and fixing git commit sign by [@kfswain](https://github.com/kfswain) in [#2108](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2108)
  * Standardize inferencepool Helm templates and drop unnecessary tpl by [@tsj-30](https://github.com/tsj-30) in [#1989](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1989)
  * feat(bbr): add configuration flags for metrics auth and secure serving by [@jpekmez](https://github.com/jpekmez) in [#2112](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2112)
  * chore(deps): bump github.com/prometheus/prometheus from 0.308.1 to 0.309.0 by [@dependabot](https://github.com/dependabot)[bot] in [#2090](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2090)
  * fix both error propogation and priority band fullness by [@wseaton](https://github.com/wseaton) in [#2103](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2103)
  * Datalayer refactoring: HTTP datasource and client by [@irar2](https://github.com/irar2) in [#2120](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2120)
  * Add v1 conformance report for alibabacloud ack gateway by [@delavet](https://github.com/delavet) in [#2007](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2007)
  * changed httproute creation to be behind a flag. by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2118](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2118)
  * Rename part two by [@shmuelk](https://github.com/shmuelk) in [#1968](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1968)
  * rename of experimental http route creation section in helm by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2123](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2123)
  * add scoring preference to scorer interface. by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2119](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2119)
  * feat: make epp-standalone be its own chart by [@capri-xiyue](https://github.com/capri-xiyue) in [#2122](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2122)
  * fix: [Flow Control]: Optionally disable endpoint subset filtering while dispatching requests by [@aishukamal](https://github.com/aishukamal) in [#2126](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2126)
  * fix: add update helm dependency by [@zetxqx](https://github.com/zetxqx) in [#2135](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2135)
  * chore(deps): bump github.com/onsi/ginkgo/v2 from 2.27.3 to 2.27.5 by [@dependabot](https://github.com/dependabot)[bot] in [#2138](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2138)
  * chore(deps): bump github.com/prometheus/prometheus from 0.309.0 to 0.309.1 by [@dependabot](https://github.com/dependabot)[bot] in [#2136](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2136)
  * chore(deps): bump github.com/onsi/gomega from 1.38.3 to 1.39.0 by [@dependabot](https://github.com/dependabot)[bot] in [#2137](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2137)
  * Rename part three by [@shmuelk](https://github.com/shmuelk) in [#2124](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2124)
  * fixed latest guide to use httproute creation in via the helm chart by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2141](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2141)
  * Removed duplicated field in log message by [@shmuelk](https://github.com/shmuelk) in [#2142](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2142)
  * Update the metrics used by the dashboard by [@learner0810](https://github.com/learner0810) in [#2139](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2139)
  * registry: switch to fine-grained leasing for flow lifecycle by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2127](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2127)
  * Increase default FlowGCTimeout to 1h to prevent premature GC by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2143](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2143)
  * update bbr quickstart guide with latest functionality by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2150](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2150)
  * Separate conformance tests modules from main tests by [@rikatz](https://github.com/rikatz) in [#1994](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1994)
  * feat: Add concurrency saturation detector by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2062](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2062)
  * feat: epp standalone helm chart included in release to docker by [@capri-xiyue](https://github.com/capri-xiyue) in [#2148](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2148)
  * Fix indention error for latency predictor by [@liu-cong](https://github.com/liu-cong) in [#2158](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2158)
  * Removing alpha status in GH landing page by [@kfswain](https://github.com/kfswain) in [#2132](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2132)
  * docs: added epp standalone user guide by [@capri-xiyue](https://github.com/capri-xiyue) in [#2147](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2147)
  * Add tracing entry span with W3C propagation to EPP handler by [@sallyom](https://github.com/sallyom) in [#2057](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2057)
  * feat(docs): enable content tab linking in mkdocs by [@AvineshTripathi](https://github.com/AvineshTripathi) in [#2176](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2176)
  * update bbr label filtering to align with best practices by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2178](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2178)
  * updated kgateway section in bbr quickstart guide by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2179](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2179)
  * move logging util to common pkg by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2180](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2180)
  * Interfaces towards pluggable BBR framework (initial PR) by [@davidbreitgand](https://github.com/davidbreitgand) in [#2121](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2121)
  * feat(api): Add appProtocol to InferencePool API for gRPC support by [@zetxqx](https://github.com/zetxqx) in [#2162](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2162)
  * docs: reference right manifest file by [@sats-23](https://github.com/sats-23) in [#2186](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2186)
  * test: add hermetic coverage for standalone mode by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2175](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2175)
  * Add support for video/audio formats for multimodal inputs by [@rahulgurnani](https://github.com/rahulgurnani) in [#2181](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2181)
  * fix identation bug in quickstart by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2182](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2182)
  * refactor(flowcontrol): Migrate Fairness Policies to EPP Plugin System by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2031](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2031)
  * [Conformance] copy pkgs from gateway-api to enable upgrade to gateway-api v1.4.0 by [@zetxqx](https://github.com/zetxqx) in [#2159](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2159)
  * controller: extend flow lease scope to fix orphaned queues [#1982](https://github.com/kubernetes-sigs/gateway-api-inference-extension/issues/1982) by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2131](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2131)
  * rename slo-aware-router to predicted-latency by [@kaushikmitr](https://github.com/kaushikmitr) in [#2183](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2183)
  * Better encapsulate data layer set up and validation. by [@elevran](https://github.com/elevran) in [#2185](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2185)
  * test: added latency predictor converage for inferencepool and added convera… by [@capri-xiyue](https://github.com/capri-xiyue) in [#2187](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2187)
  * cleanup: refactor multiple include into one file by [@capri-xiyue](https://github.com/capri-xiyue) in [#2191](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2191)
  * feat: Allow request control plugins to return ext_proc dynamic metadata by [@fcfort](https://github.com/fcfort) in [#2156](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2156)
  * Moving the scheduling component pluggable interface and types to the common framework pkg by [@ahg-g](https://github.com/ahg-g) in [#2192](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2192)
  * Update troubleshooting guide to include remediation for incorrect pre… by [@BenjaminBraunDev](https://github.com/BenjaminBraunDev) in [#2040](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2040)
  * Add flowcontrol queue length in bytes metric by [@RyanRosario](https://github.com/RyanRosario) in [#2044](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2044)
  * Moved the epp/plugins pkg to be under the new framework pkg by [@ahg-g](https://github.com/ahg-g) in [#2194](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2194)
  * Move framework interfaces under epp/framework/interface by [@ahg-g](https://github.com/ahg-g) in [#2195](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2195)
  * feat: added a local mode in verify helm script by [@capri-xiyue](https://github.com/capri-xiyue) in [#2196](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2196)
  * [Flow Control] Garbage Collection for Priority Bands by [@evacchi](https://github.com/evacchi) in [#2097](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2097)
  * Moving requestcontrol pluggable interfaces/types to epp/framework/interface by [@ahg-g](https://github.com/ahg-g) in [#2197](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2197)
  * Flow Control: Ordering Policy Migration (Phase 1) by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2188](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2188)
  * docs: reorganize EPP configuration guide structure by [@cr7258](https://github.com/cr7258) in [#1962](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1962)
  * fix: fixed epp standalone helm and updated verify helm script by [@capri-xiyue](https://github.com/capri-xiyue) in [#2204](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2204)
  * Fix kgateway document link by [@learner0810](https://github.com/learner0810) in [#2140](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2140)
  * change scorer weight type from int to float by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2207](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2207)
  * Flow Control: Ordering Policy Migration (Phase 2) by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2193](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2193)
  * cleanup of setupLog from function calls in runner by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2206](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2206)
  * fix(registry): fix JIT error scoping race by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2199](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2199)
  * refactor(flowcontrol): migrate interfaces to framework/interface/flowcontrol by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2208](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2208)
  * Move requestcontrol interface dependencies to epp/framework/interface by [@ahg-g](https://github.com/ahg-g) in [#2213](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2213)
  * Update prefix scorer to report cached prefix length in tokens by [@mayabar](https://github.com/mayabar) in [#2053](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2053)
  * Add SGlang example/docs by [@rahulgurnani](https://github.com/rahulgurnani) in [#2002](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2002)
  * refactor: rename interflow package to fairness by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2214](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2214)
  * Extracted all scheduling execution logic out of framework/interface/scheduling pkg by [@ahg-g](https://github.com/ahg-g) in [#2223](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2223)
  * refactor: rename intraflow package to ordering by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2215](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2215)
  * feat: switch saturation detection to gradient signal by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2224](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2224)
  * fix: removed duplicate include by [@capri-xiyue](https://github.com/capri-xiyue) in [#2225](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2225)
  * test file for datastore.go by [@tsj-30](https://github.com/tsj-30) in [#2200](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2200)
  * cleanup(flowcontrol): Refactor registry with generic leasing and atomic GC by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2198](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2198)
  * chore: bump sim version by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2227](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2227)
  * Move datalayer plugin intefaces and associated types to framework/interface/datalayer by [@ahg-g](https://github.com/ahg-g) in [#2228](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2228)
  * feat: add FC configuration to EndpointPickerConfig by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2217](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2217)
  * refactor: move flow control types to framework pkg by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2221](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2221)
  * Moving the scheduling plugins under epp/framework/plugins by [@ahg-g](https://github.com/ahg-g) in [#2230](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2230)
  * Fix/ feature for issue 2028. Add support for responses api and conversations api by [@srampal](https://github.com/srampal) in [#2133](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2133)
  * Moved request control plugins to epp/framework/plugins by [@ahg-g](https://github.com/ahg-g) in [#2231](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2231)
  * feat: wire flow control policies and smart queue defaults by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2232](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2232)
  * Add NGF 2.4 conformance report by [@bjee19](https://github.com/bjee19) in [#2236](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2236)
  * documentation: add guide for trace support by [@JeffLuoo](https://github.com/JeffLuoo) in [#2212](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2212)
  * feat: New cost reporting plugin for returning cost in ext_proc dynamic metadata response to proxy by [@fcfort](https://github.com/fcfort) in [#2114](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2114)
  * feat(metrics): support engine-aware metric collection by [@bongwoobak](https://github.com/bongwoobak) in [#2161](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2161)
  * Add `pod_type` categorical feature to latency prediction models by [@RishabhSaini](https://github.com/RishabhSaini) in [#1993](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1993)
  * test: fix flakes in controller and registry tests by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2250](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2250)
  * chore(deps): bump go.opentelemetry.io/otel/trace from 1.39.0 to 1.40.0 by [@dependabot](https://github.com/dependabot)[bot] in [#2254](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2254)
  * Add v1.0.1 conformance report for Istio by [@ericdbishop](https://github.com/ericdbishop) in [#2235](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2235)
  * feat: added epp standalone label to track usage by [@capri-xiyue](https://github.com/capri-xiyue) in [#2252](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2252)
  * docs: fix epp README.md link for metrics by [@rumstead](https://github.com/rumstead) in [#2259](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2259)
  * docs: Removed speculative statement about creating a new Slack channel by [@terrytangyuan](https://github.com/terrytangyuan) in [#2272](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2272)
  * Implement garbage collection for SLO context store by [@aniketmohanty82](https://github.com/aniketmohanty82) in [#2245](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2245)
  * Change compilation-config flag to valid default parameter by [@BenjaminBraunDev](https://github.com/BenjaminBraunDev) in [#2244](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2244)
  * feat: added guide for epp standalone with inferencepool dependency by [@capri-xiyue](https://github.com/capri-xiyue) in [#2260](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2260)
  * support for setting up epp deployment resources by [@learner0810](https://github.com/learner0810) in [#2273](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2273)
  * SGlang: Fixes to the getting started guide by [@rahulgurnani](https://github.com/rahulgurnani) in [#2279](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2279)
  * add priority to admit request by [@kaushikmitr](https://github.com/kaushikmitr) in [#2251](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2251)
  * Initial move of datalayer plugins to epp/framework/plugins by [@ahg-g](https://github.com/ahg-g) in [#2286](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2286)
  * Moved metrics keys to the datalayer framework pkg by [@ahg-g](https://github.com/ahg-g) in [#2288](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2288)
  * align attritbute reporter with plugins structure by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2290](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2290)
  * move logging under observability dir by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2292](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2292)
  * extracted InitLogging to common observability logging pkg by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2296](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2296)
  * refactor: rename epp standalone to standalone by [@capri-xiyue](https://github.com/capri-xiyue) in [#2298](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2298)
  * Validate dependencies for all producer/consumer plugins in datalayer by [@rahulgurnani](https://github.com/rahulgurnani) in [#2246](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2246)
  * refactor: rename to epplib by [@capri-xiyue](https://github.com/capri-xiyue) in [#2299](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2299)
  * chore(deps): bump sigs.k8s.io/kustomize/kyaml from 0.21.0 to 0.21.1 by [@dependabot](https://github.com/dependabot)[bot] in [#2303](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2303)
  * Check scheme in the HTTP datasource creation by [@irar2](https://github.com/irar2) in [#2305](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2305)
  * chore(deps): bump go.opentelemetry.io/otel/exporters/otlp/otlptrace/otlptracegrpc from 1.39.0 to 1.40.0 by [@dependabot](https://github.com/dependabot)[bot] in [#2304](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2304)
  * Move the metrics extractor to its own pkg under plugins/datalayer/extractor by [@ahg-g](https://github.com/ahg-g) in [#2291](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2291)
  * extracted tracing/profiling/metrics common to observability by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2307](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2307)
  * SGLang docs: Revert changes to the released guide and apply them to the latest guide by [@rahulgurnani](https://github.com/rahulgurnani) in [#2302](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2302)
  * fix(datastore): remove orphaned rank endpoints when targetPorts changes by [@bongwoobak](https://github.com/bongwoobak) in [#2308](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2308)
  * chore: remove deprecated top-level istio values by [@varad-ahirwadkar](https://github.com/varad-ahirwadkar) in [#2280](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2280)
  * add PopulateControllerConfig in runner.go by [@kaushikmitr](https://github.com/kaushikmitr) in [#2300](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2300)
  * clean up: remove model server type by [@capri-xiyue](https://github.com/capri-xiyue) in [#2311](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2311)
  * Revert "clean up: remove model server type" by [@capri-xiyue](https://github.com/capri-xiyue) in [#2312](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2312)
  * conformance: better polling times by [@howardjohn](https://github.com/howardjohn) in [#2313](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2313)
  * chore: fix typo by [@hhk7734](https://github.com/hhk7734) in [#2322](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2322)
  * docs: fixed standalone docs by [@capri-xiyue](https://github.com/capri-xiyue) in [#2314](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2314)
  * Config options fix by [@shmuelk](https://github.com/shmuelk) in [#2321](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2321)
  * chore: Separate Gateway API main module and conformance module dependencies by [@ericdbishop](https://github.com/ericdbishop) in [#2285](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2285)
  * conformance: modify warmup from weighted two-pools test by [@danehans](https://github.com/danehans) in [#2316](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2316)
  * Add dispatch cycle duration metric for flow control by [@RyanRosario](https://github.com/RyanRosario) in [#2110](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2110)
  * Add unit tests to validate Datalayer metrics source and extractor produce the same metrics as backend.PodMetrics by [@elevran](https://github.com/elevran) in [#2323](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2323)
  * Refactor: Centralize EndpointPool initialization for Standalone mode by [@varad-ahirwadkar](https://github.com/varad-ahirwadkar) in [#2229](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2229)
  * Add flow control request enqueue metric by [@RyanRosario](https://github.com/RyanRosario) in [#2153](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2153)
  * move latency prediction to prepare data step and add admit request plugin hook by [@kaushikmitr](https://github.com/kaushikmitr) in [#2319](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2319)
  * [Feature] Add k8s notification data sources by [@elevran](https://github.com/elevran) in [#2320](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2320)
  * Script to validate framework imports are confined by [@elevran](https://github.com/elevran) in [#2337](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2337)
  * Have a default configuration for the EndpointPicker by [@shmuelk](https://github.com/shmuelk) in [#2341](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2341)
  * A transitional PR towards pluggable BBR Framework by [@davidbreitgand](https://github.com/davidbreitgand) in [#2209](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2209)
  * chore(deps): bump github.com/onsi/gomega from 1.39.0 to 1.39.1 by [@dependabot](https://github.com/dependabot)[bot] in [#2346](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2346)
  * chore(deps): bump the kubernetes group with 6 updates by [@dependabot](https://github.com/dependabot)[bot] in [#2344](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2344)
  * chore(deps): bump sigs.k8s.io/structured-merge-diff/v6 from 6.3.1 to 6.3.2 by [@dependabot](https://github.com/dependabot)[bot] in [#2345](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2345)
  * chore(deps): bump sigs.k8s.io/controller-runtime from 0.22.4 to 0.23.1 by [@dependabot](https://github.com/dependabot)[bot] in [#2216](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2216)
  * Enable make `verify-fw-imports` by [@elevran](https://github.com/elevran) in [#2358](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2358)
  * more work on pluggable bbr and alignment with epp structure and naming by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2348](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2348)
  * Move datasource/extractor validations to datalayer runtime. by [@elevran](https://github.com/elevran) in [#2357](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2357)
  * Make PredictedLatencyScorer PD Aware by [@RishabhSaini](https://github.com/RishabhSaini) in [#2361](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2361)
  * fix: InitLogging verbosity ignored due to ctrl.SetLogger single-fulfillment by [@kaushikmitr](https://github.com/kaushikmitr) in [#2363](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2363)
  * chore(deps): bump sigs.k8s.io/controller-tools from 0.19.0 to 0.20.1 by [@dependabot](https://github.com/dependabot)[bot] in [#2347](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2347)
  * refactor(bbr): extract cmd-line args to Options struct by [@yehuditkerido](https://github.com/yehuditkerido) in [#2353](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2353)
  * make kaushikmitr latencypredictor owner by [@kaushikmitr](https://github.com/kaushikmitr) in [#2373](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2373)
  * Fix typo in the letter l should be in capital case by [@szedan-rh](https://github.com/szedan-rh) in [#2380](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2380)
  * Moved data layer metrics compatibility test to keep framework imports self contained by [@elevran](https://github.com/elevran) in [#2381](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2381)
  * Build and distribute latencyprediction images by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#2287](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2287)
  * Make latency prediction server multi threaded compatable by [@kaushikmitr](https://github.com/kaushikmitr) in [#2349](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2349)
  * docs: Fix typos in config-text.md by [@terrytangyuan](https://github.com/terrytangyuan) in [#2387](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2387)
  * fix: stable epp version for conformance test in main by [@zetxqx](https://github.com/zetxqx) in [#2386](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2386)
  * fix: Fix toggle for cpu/gpu/sim model server deployment by [@terrytangyuan](https://github.com/terrytangyuan) in [#2388](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2388)
  * feat: improve filter plugin logging by [@terrytangyuan](https://github.com/terrytangyuan) in [#2389](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2389)
  * Model replacement to Qwen3-32B by [@sats-23](https://github.com/sats-23) in [#2189](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2189)
  * Refactor EPP initialization for unified integration testing by [@zetxqx](https://github.com/zetxqx) in [#2351](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2351)
  * fix: reuse scheduling predictions for TTFT and first TPOT reporting by [@kaushikmitr](https://github.com/kaushikmitr) in [#2372](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2372)
  * Update Grafana setup directions by [@RyanRosario](https://github.com/RyanRosario) in [#2335](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2335)
  * Emit metrics on execution time of BBR plugins by [@asaadbalum](https://github.com/asaadbalum) in [#2379](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2379)
  * Update vllm version in gpu deployment and minor doc fixes by [@rahulgurnani](https://github.com/rahulgurnani) in [#2375](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2375)
  * LatencyPredicton in Disagg Mode should handle filtered predictions by [@RishabhSaini](https://github.com/RishabhSaini) in [#2390](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2390)
  * chore(deps): bump google.golang.org/grpc from 1.78.0 to 1.79.1 by [@dependabot](https://github.com/dependabot)[bot] in [#2403](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2403)
  * chore(deps): bump sigs.k8s.io/kustomize/api from 0.21.0 to 0.21.1 by [@dependabot](https://github.com/dependabot)[bot] in [#2399](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2399)
  * chore(deps): bump github.com/elastic/crd-ref-docs from 0.2.0 to 0.3.0 by [@dependabot](https://github.com/dependabot)[bot] in [#2402](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2402)
  * fix: Add ALPN h2 support to gRPC TLS configuration by [@johnahull](https://github.com/johnahull) in [#2385](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2385)
  * chore: metrics vars private instead of public by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2396](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2396)
  * Datalayer: Validate data plugins execution order across different layers by [@rahulgurnani](https://github.com/rahulgurnani) in [#2333](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2333)
  * Add logs (at log level 1) for better observability by [@rahulgurnani](https://github.com/rahulgurnani) in [#2384](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2384)
  * Enable golangci-lint v2.9.0 and fix lint errors by [@elevran](https://github.com/elevran) in [#2416](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2416)
  * Update InferencePool helm chart to use FailOpen as default by [@RyanRosario](https://github.com/RyanRosario) in [#2365](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2365)
  * Refactor datalayer interfaces, separating polling and notification based types by [@elevran](https://github.com/elevran) in [#2407](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2407)
  * Refactor response path: accept raw bytes and consolidate ResponseComplete logic by [@zetxqx](https://github.com/zetxqx) in [#2410](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2410)
  * add objective type to latency prediction go client by [@kaushikmitr](https://github.com/kaushikmitr) in [#2423](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2423)
  * create predictedLatencyCtx during scoring if not created in prepareddata by [@kaushikmitr](https://github.com/kaushikmitr) in [#2408](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2408)
  * Refactor request path for pluggable parser by [@zetxqx](https://github.com/zetxqx) in [#2409](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2409)
  * chore(deps): bump github.com/onsi/ginkgo/v2 from 2.28.0 to 2.28.1 by [@dependabot](https://github.com/dependabot)[bot] in [#2400](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2400)
  * Guide fixes as a followup to the qwen move by [@ahg-g](https://github.com/ahg-g) in [#2431](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2431)
  * Revert "[Conformance] copy pkgs from gateway-api to enable upgrade to gateway-api v1.4.0 ([#2159](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2159))" by [@ericdbishop](https://github.com/ericdbishop) in [#2331](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2331)
  * fix(predicted-latency-scorer): prevent nil pointer panic for non-completions API types by [@noalimoy](https://github.com/noalimoy) in [#2415](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2415)
  * feat(bbr): build RequestContext to store headers for plugins by [@noalimoy](https://github.com/noalimoy) in [#2368](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2368)
  * [pluggable bbr] Configurable body fields to headers BBR plugin by [@davidbreitgand](https://github.com/davidbreitgand) in [#2417](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2417)
  * Makes codebase pflag consistent by [@carmal891](https://github.com/carmal891) in [#2443](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2443)
  * Add BBR metrics to Inference Gateway Grafana dashboard by [@asaadbalum](https://github.com/asaadbalum) in [#2397](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2397)
  * First part of refactoring the ExtProc used by both the EPP and the BBR by [@shmuelk](https://github.com/shmuelk) in [#2428](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2428)
  * support active port declaration on pod annotation by [@delavet](https://github.com/delavet) in [#2256](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2256)
  * Fix: Downgrade NTPOT metrics error to verbose info to reduce noise by [@ycjiang50](https://github.com/ycjiang50) in [#2435](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2435)
  * add the option to register custom metrics by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2445](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2445)
  * sidecar: add request coalescing for bulk predictions by [@kaushikmitr](https://github.com/kaushikmitr) in [#2425](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2425)
  * chore(deps): bump go.opentelemetry.io/otel/exporters/stdout/stdouttrace from 1.39.0 to 1.41.0 by [@dependabot](https://github.com/dependabot)[bot] in [#2460](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2460)
  * chore(deps): bump the kubernetes group with 6 updates by [@dependabot](https://github.com/dependabot)[bot] in [#2456](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2456)
  * chore(deps): bump github.com/envoyproxy/go-control-plane/envoy from 1.36.0 to 1.37.0 by [@dependabot](https://github.com/dependabot)[bot] in [#2457](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2457)
  * chore(deps): bump go.opentelemetry.io/otel/exporters/otlp/otlptrace/otlptracegrpc from 1.40.0 to 1.41.0 by [@dependabot](https://github.com/dependabot)[bot] in [#2459](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2459)
  * Epp resource defaults by [@kaushikmitr](https://github.com/kaushikmitr) in [#2455](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2455)
  * Add hermetic test for epp with inferencepool by [@capri-xiyue](https://github.com/capri-xiyue) in [#2277](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2277)
  * chore(Conformance): update dependabot.yml for conformance sub module by [@zetxqx](https://github.com/zetxqx) in [#2462](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2462)
  * [release-1.4] fix(release): sync quickstart vllm images by [@danehans](https://github.com/danehans) in [#2522](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2522)
  * [release-1.4] chore(conformance): bump gateway-api to v1.5.0 by [@danehans](https://github.com/danehans) in [#2520](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2520)
  * [release-1.4] fix: avoid configError when logging EndpointPickerConfig by [@danehans](https://github.com/danehans) in [#2626](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2626)
  * [release-1.4] [fix] properly template extraContainerPorts by [@danehans](https://github.com/danehans) in [#2624](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2624)
  * fix(release): backport latency predictor release updates by [@danehans](https://github.com/danehans) in [#2633](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2633)


## New Contributors
  * [@tsj-30](https://github.com/tsj-30) made their first contribution in [#1989](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1989)
  * [@jpekmez](https://github.com/jpekmez) made their first contribution in [#2112](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2112)
  * [@wseaton](https://github.com/wseaton) made their first contribution in [#2103](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2103)
  * [@aishukamal](https://github.com/aishukamal) made their first contribution in [#2126](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2126)
  * [@fcfort](https://github.com/fcfort) made their first contribution in [#2156](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2156)
  * [@bjee19](https://github.com/bjee19) made their first contribution in [#2236](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2236)
  * [@bongwoobak](https://github.com/bongwoobak) made their first contribution in [#2161](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2161)
  * [@ericdbishop](https://github.com/ericdbishop) made their first contribution in [#2235](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2235)
  * [@rumstead](https://github.com/rumstead) made their first contribution in [#2259](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2259)
  * [@aniketmohanty82](https://github.com/aniketmohanty82) made their first contribution in [#2245](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2245)
  * [@johnahull](https://github.com/johnahull) made their first contribution in [#2385](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2385)
  * [@carmal891](https://github.com/carmal891) made their first contribution in [#2443](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2443)
  * [@ycjiang50](https://github.com/ycjiang50) made their first contribution in [#2435](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2435)


**Full Changelog** : [`v1.3.1...v1.4.0`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/compare/v1.3.1...v1.4.0)
### Contributors
evacchi, fcfort, and 49 other contributors 
Assets 5
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v1.4.0).
You can’t perform that action at this time. 
