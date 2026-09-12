---
title: Release v1.3.0 · kubernetes-sigs/gateway-api-inference-extension · GitHub
id: release-v130-kubernetes-sigsgateway-api-inference-extension-github
tags:
- llmd-router-nonk8s
- llmd-router-nonk8s-backends-54b774
- epp-releases
- standalone-mode
created: '2026-09-11T19:43:40.701540Z'
updated: '2026-09-12T03:37:37.542142Z'
source: https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v1.3.0
source_domain: github.com
fetched_at: '2026-09-11T19:43:40.681474Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'GIE v1.3.0 release notes, released by @kfswain on Jan 21 (2026), tag signed
  with verified SSH signature. Noteworthy section: (1) LoRA Syncer feature deprecated
  — the lora-syncer image will not ship with this or future releases (similar functionality
  moves to vLLM''s filesystem LoRA resolver plugin), old images kept indefinitely
  for model servers using the discrete LoRA management endpoints, syncer code slated
  for full removal in the next release; (2) Flow Control gained Scale-from/to-Zero
  support, allowing requests to reach an EPP with zero model-serving endpoints behind
  it while emitting metrics an autoscaler can use to scale the pool back up; (3) ''Standalone
  EPP: This functionality allows the EPP to be deployed as a proxy, all contained
  within a single pod. This is achieved by the Envoy proxy having EPP as a sidecar
  container. This feature was developed for batch inference scenarios, and is currently
  considered experimental'' — this is the first appearance of standalone mode, introduced
  as experimental at v1.3.0. Also fixed the approximate prefix-cache scorer''s interaction
  with the llm-d P/D (prefill/decode disaggregation) setup. Merged PR list includes
  ''Execute prepare data plugins in topological order of data dependencies'' (#1878,
  @rahulgurnani) and ''Implement EPP Plugins by datalayer objects'' (#1901, @elevran)
  — early datalayer-based plugin refactor work, and ''SLO Aware Routing Sidecar +
  Plugin EPP Integration and Helm Deployment'' (#1839, @BenjaminBraunDev).'
utility_score: 10.0
---

[Skip to content](https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v1.3.0#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v1.3.0) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v1.3.0) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v1.3.0) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v1.3.0).
/ **[gateway-api-inference-extension](https://github.com/kubernetes-sigs/gateway-api-inference-extension) ** Public
  * [ Notifications ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension) You must be signed in to change notification settings
  * [ Fork 313 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)
  * [ Star  766 ](https://github.com/login?return_to=%2Fkubernetes-sigs%2Fgateway-api-inference-extension)


# v1.3.0
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v1.3.0).
## No results found
[kfswain](https://github.com/kfswain) released this  21 Jan 14:17 
· [ 551 commits ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/compare/v1.3.0...main) to main since this release 
[ v1.3.0  ](https://github.com/kubernetes-sigs/gateway-api-inference-extension/tree/v1.3.0)
This tag was signed with the committer’s **verified signature**. 
[kfswain](https://github.com/kfswain) Kellen Swain 
SSH Key Fingerprint: qdX+2lJVLhNKKzz9yNa2Jy/jrE+nq3rro1wms4jcoXM
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
[ `616745e`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/commit/616745e99b44f7ef0650b40ed1475756282d7e1f)
This commit was signed with the committer’s **verified signature**. 
[kfswain](https://github.com/kfswain) Kellen Swain 
SSH Key Fingerprint: qdX+2lJVLhNKKzz9yNa2Jy/jrE+nq3rro1wms4jcoXM
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
## Noteworthy
### LoRA Syncer
This release, and future releases will _not_ have the lora syncer image associated with them, as we are deprecating that feature, a similar functionality will still exist in the form of the [file system resolver](https://docs.vllm.ai/en/latest/api/vllm/plugins/lora_resolvers/filesystem_resolver/). For model servers that do not yet support this form of LoRA management, but support the discrete LoRA management endpoints that the `lora-syncer` uses, the old images will be kept indefinitely, and can still be used.
In the next release, the lora syncer code will be removed from the codebase.
### Flow Control
Flow Control continues to evolve with the addition of _Scale from/to Zero_ support. Allowing requests to be sent to an EPP with no model serving endpoints behind it, and emitting metrics to be used by the autoscaler to then scale up the pool.
In following releases we will continue to develop towards this feature being default enabled.
### Standalone EPP
This functionality allows the EPP to be deployed as a proxy, all contained within a single pod. This is achieved by the Envoy proxy having EPP as a sidecar container. This feature was developed for batch inference scenarios, and is currently considered experimental.
### Fix(es)
  * We improved the functionality of the approximate prefix cache scorer when working with the llm-d P/D setup


## What's Changed
  * Added crd validation ci workflow. by [@bexxmodd](https://github.com/bexxmodd) in [#1879](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1879)
  * chore: bump sim version by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#1890](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1890)
  * feat(conformance): add conformance test for verifying `x-gateway-destination-endpoint-served` by [@zetxqx](https://github.com/zetxqx) in [#1862](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1862)
  * Add deprecation notice on metrics port in runner and datastore by [@elevran](https://github.com/elevran) in [#1886](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1886)
  * refactor: Flatten Flow Control inter-flow policy plugin directory structure by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#1841](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1841)
  * Execute prepare data plugins in topological order of data dependencies by [@rahulgurnani](https://github.com/rahulgurnani) in [#1878](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1878)
  * chore(deps): bump go.uber.org/zap from 1.27.0 to 1.27.1 by [@dependabot](https://github.com/dependabot)[bot] in [#1896](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1896)
  * chore(deps): bump google.golang.org/grpc from 1.76.0 to 1.77.0 by [@dependabot](https://github.com/dependabot)[bot] in [#1897](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1897)
  * chore(deps): bump github.com/prometheus/common from 0.67.2 to 0.67.4 by [@dependabot](https://github.com/dependabot)[bot] in [#1895](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1895)
  * enhance bbr helm chart to generalize cmd-line args by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#1900](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1900)
  * feat: Add totalRunningRequests metric for latency predictor by [@BenjaminBraunDev](https://github.com/BenjaminBraunDev) in [#1899](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1899)
  * chore(deps): bump sigs.k8s.io/structured-merge-diff/v6 from 6.3.0 to 6.3.1 by [@dependabot](https://github.com/dependabot)[bot] in [#1898](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1898)
  * SLO Aware Routing Sidecar + Plugin EPP Integration and Helm Deployment by [@BenjaminBraunDev](https://github.com/BenjaminBraunDev) in [#1839](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1839)
  * Use the correct vllm metric gpu_cache_usage_perc --> kv_cache_usage_perc by [@ezrasilvera](https://github.com/ezrasilvera) in [#1905](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1905)
  * fix: fixed helm chart by [@capri-xiyue](https://github.com/capri-xiyue) in [#1907](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1907)
  * docs: add Kgateway BBR documentation by [@howardjohn](https://github.com/howardjohn) in [#1908](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1908)
  * Implement EPP Plugins by datalayer objects by [@elevran](https://github.com/elevran) in [#1901](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1901)
  * feat: Implement Model Rewrite and Traffic Splitting Logic by [@zetxqx](https://github.com/zetxqx) in [#1820](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1820)
  * docs: Updated quickstart to use stable Istio release 1.28.0 by @atharva-310 in [#1902](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1902)
  * fix(release): correctly update lora-syncer and epp image tags across RC and final releases by [@googs1025](https://github.com/googs1025) in [#1916](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1916)
  * fix: sort InferenceModelRewrite lists by (Namespace, Name) in tests by [@googs1025](https://github.com/googs1025) in [#1917](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1917)
  * Define and register plugin factories for datalayer by [@elevran](https://github.com/elevran) in [#1911](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1911)
  * fix: Properly install the InferenceModelRewrite CRD using kustomize by [@shmuelk](https://github.com/shmuelk) in [#1934](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1934)
  * Move AllPodsPredicate to datastore package by [@elevran](https://github.com/elevran) in [#1939](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1939)
  * Add automatic TLS certificate reloading for EPP by [@pierDipi](https://github.com/pierDipi) in [#1765](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1765)
  * feat(modelRewrite): Add metrics for InferenceModelRewrite decisions by [@zetxqx](https://github.com/zetxqx) in [#1938](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1938)
  * fix: CI golangci-lint errors by [@shmuelk](https://github.com/shmuelk) in [#1948](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1948)
  * Update inference perf chart to match upstream chart + Add Prefix Cache Github Actions by [@rlakhtakia](https://github.com/rlakhtakia) in [#1949](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1949)
  * Standardize plugins.TypedName field name from 'tn' to 'typedName' by [@rohithnarasimha](https://github.com/rohithnarasimha) in [#1918](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1918)
  * Update inference perf chart to use new hf token structure. by [@rlakhtakia](https://github.com/rlakhtakia) in [#1955](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1955)
  * fix infinite loop in profile picker and switch predictor based routing to on by default with a header to disable by [@BenjaminBraunDev](https://github.com/BenjaminBraunDev) in [#1929](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1929)
  * fix config load error when picker is set before the scoerer w/o weight. by [@zetxqx](https://github.com/zetxqx) in [#1958](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1958)
  * add kaushikmitr as appoved of slo aware routing plugin by [@kaushikmitr](https://github.com/kaushikmitr) in [#1956](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1956)
  * refactor: [Scale from Zero] Introduce PodLocator by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#1950](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1950)
  * feat: add config validation in predicted-latency-scorer plugin by [@googs1025](https://github.com/googs1025) in [#1904](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1904)
  * Run tests with two data layer implementations by [@irar2](https://github.com/irar2) in [#1930](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1930)
  * Rename PodInfo struct to EndpointMetadata to better reflect its purpose by [@shmuelk](https://github.com/shmuelk) in [#1866](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1866)
  * feat(metrics): add scheduler attempt counter by [@googs1025](https://github.com/googs1025) in [#1931](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1931)
  * chore: update released quickstart to v1.2.1 by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#1941](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1941)
  * generalize latest release quickstart by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#1966](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1966)
  * chore(deps): bump github.com/onsi/ginkgo/v2 from 2.27.2 to 2.27.3 by [@dependabot](https://github.com/dependabot)[bot] in [#1971](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1971)
  * chore(deps): bump golang.org/x/sync from 0.18.0 to 0.19.0 by [@dependabot](https://github.com/dependabot)[bot] in [#1972](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1972)
  * chore(deps): bump go.opentelemetry.io/otel/sdk from 1.38.0 to 1.39.0 by [@dependabot](https://github.com/dependabot)[bot] in [#1975](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1975)
  * refactor: Standardize config loading and system default injection by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#1953](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1953)
  * chore(deps): bump github.com/onsi/gomega from 1.38.2 to 1.38.3 by [@dependabot](https://github.com/dependabot)[bot] in [#1974](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1974)
  * chore(deps): bump go.opentelemetry.io/otel/exporters/stdout/stdouttrace from 1.38.0 to 1.39.0 by [@dependabot](https://github.com/dependabot)[bot] in [#1973](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1973)
  * feat: Enable Scale-from-Zero with Flow Control enabled by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#1952](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1952)
  * feature: (helm) support custom volumes and volumeMounts for epp by [@delavet](https://github.com/delavet) in [#1945](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1945)
  * Use spf13/pflag instead of Go's standard flag package by [@elevran](https://github.com/elevran) in [#1979](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1979)
  * Extend textual configuration support with the Datalayer's configuration by [@shmuelk](https://github.com/shmuelk) in [#1914](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1914)
  * test/integration: introduce robust harness and migrate BBR suite by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#1959](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1959)
  * test/bbr: fix startup race condition and IPv6 address formatting by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#1987](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1987)
  * [chore]Bump vLLM Image Tags by [@Frapschen](https://github.com/Frapschen) in [#1733](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1733)
  * Add Prefill Heavy E2E Test to Github Actions by [@rlakhtakia](https://github.com/rlakhtakia) in [#1894](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1894)
  * Add decode heavy benchmark e2e test to github actions. by [@rlakhtakia](https://github.com/rlakhtakia) in [#1893](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1893)
  * BBR multi lora guide by [@davidbreitgand](https://github.com/davidbreitgand) in [#1940](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1940)
  * [feat] Add running requests scorer and tests by [@BenjaminBraunDev](https://github.com/BenjaminBraunDev) in [#1957](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1957)
  * Implement PrepareDataPlugin for prefix cache match plugin by [@rahulgurnani](https://github.com/rahulgurnani) in [#1942](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1942)
  * Define and implement command line parsing with Options struct by [@elevran](https://github.com/elevran) in [#1984](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1984)
  * fix(inferenceModelRewrites): conditionally skip watching InferenceModelRewrite and InferenceObjective by [@zetxqx](https://github.com/zetxqx) in [#1967](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1967)
  * Add e2e test for multiport InferencePool enhancement by [@RyanRosario](https://github.com/RyanRosario) in [#1885](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1885)
  * chore(deps): bump go.opentelemetry.io/otel/exporters/otlp/otlptrace/otlptracegrpc from 1.38.0 to 1.39.0 by [@dependabot](https://github.com/dependabot)[bot] in [#1997](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1997)
  * flowcontrol: refactor registry config to support dynamic priority provisioning by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2001](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2001)
  * test(e2e): use kustomize to install all the crds by [@zetxqx](https://github.com/zetxqx) in [#1990](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1990)
  * chore(deps): bump github.com/prometheus/prometheus from 0.307.3 to 0.308.0 by [@dependabot](https://github.com/dependabot)[bot] in [#1999](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1999)
  * chore(deps): bump the kubernetes group with 6 updates by [@dependabot](https://github.com/dependabot)[bot] in [#1996](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1996)
  * chore(deps): bump github.com/spf13/pflag from 1.0.7 to 1.0.10 by [@dependabot](https://github.com/dependabot)[bot] in [#2000](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2000)
  * remove duplicate lora adapter scorer entry in docs by [@strangiato](https://github.com/strangiato) in [#2009](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2009)
  * doc: add doc for infernecemodelrewrites. by [@zetxqx](https://github.com/zetxqx) in [#1978](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1978)
  * Update benchmarking to use correct secret by [@rlakhtakia](https://github.com/rlakhtakia) in [#2004](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2004)
  * flowcontrol: Support dynamic priority provisioning by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2006](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2006)
  * pkg/epp: use labels.Equals for label comparison by [@ErikJiang](https://github.com/ErikJiang) in [#2015](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2015)
  * fix: fix header parsing to prevent trace ID loss by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2024](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2024)
  * fix: decouple streaming usage parsing from [DONE] signal to handle network fragmentation by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2026](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2026)
  * refactor: Flatten Flow Control intra-flow policy plugin directory structure by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#1840](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1840)
  * fix: harden header sanitization and handling logic by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2025](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2025)
  * Refactor: Prepare EPP SaturationDetection as an Extension Point by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#1976](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1976)
  * test: fix flaky garbage collection test by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2014](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2014)
  * feat(flowcontrol): add pool and model labels to metrics by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2010](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2010)
  * add preparedata plugin to latency based scorer to consume prefix states by [@kaushikmitr](https://github.com/kaushikmitr) in [#2005](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2005)
  * chore(deps): bump github.com/prometheus/prometheus from 0.308.0 to 0.308.1 by [@dependabot](https://github.com/dependabot)[bot] in [#2036](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2036)
  * cleanup: Migrate raw map sets to k8s.io/apimachinery/pkg/util/sets by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2030](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2030)
  * test: expose fake data store for downstream tests by [@MregXN](https://github.com/MregXN) in [#2027](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2027)
  * chore(deps): bump google.golang.org/protobuf from 1.36.10 to 1.36.11 by [@dependabot](https://github.com/dependabot)[bot] in [#2037](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2037)
  * chore(deps): bump google.golang.org/grpc from 1.77.0 to 1.78.0 by [@dependabot](https://github.com/dependabot)[bot] in [#2043](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2043)
  * bbr configmap reconciler and bbr datastore by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2045](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2045)
  * refactor: refactor monitoring session by [@capri-xiyue](https://github.com/capri-xiyue) in [#1906](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1906)
  * fix: correctly handle zero fresh pods in pool metrics by [@googs1025](https://github.com/googs1025) in [#2049](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2049)
  * Set up data layer based on configuration by [@elevran](https://github.com/elevran) in [#2046](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2046)
  * track base models in bbr by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2050](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2050)
  * bbr helm chart rbac enhancements for multi pool management by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2047](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2047)
  * setup configmap reconciler with controller manager by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2051](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2051)
  * create httproute via helm chart by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2054](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2054)
  * Double check the flow before marking it idle by [@shmuelk](https://github.com/shmuelk) in [#2041](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2041)
  * cleanup: remove min helper function by [@ErikJiang](https://github.com/ErikJiang) in [#2052](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2052)
  * feat: add DeadlinePriority plugin in intra-flow dispatch policy by [@googs1025](https://github.com/googs1025) in [#1960](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1960)
  * test/integration: introduce robust harness and migrate EPP suite by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2022](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2022)
  * chore(deps): bump github.com/prometheus/common from 0.67.4 to 0.67.5 by [@dependabot](https://github.com/dependabot)[bot] in [#2059](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2059)
  * fix prometheus auth by [@sallyom](https://github.com/sallyom) in [#2061](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2061)
  * Limit response body size by [@adelsam](https://github.com/adelsam) in [#2058](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2058)
  * fix: ensure ResponseComplete hook always executes by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2064](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2064)
  * chore(comment): correct OpenAI chat completions endpoint path by [@googs1025](https://github.com/googs1025) in [#2065](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2065)
  * fix bbr image build. by [@zetxqx](https://github.com/zetxqx) in [#2066](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2066)
  * remove setup log when it is not needed to pass it as arg by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2069](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2069)
  * enable configmap controller in bbr by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2067](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2067)
  * enable bbr integration tests in makefile by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2071](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2071)
  * added optional base model flag to inferencepool helm chart by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2073](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2073)
  * typo in bbr helm chart rbac by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2074](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2074)
  * typo fix by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2075](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2075)
  * Conformance report for NGINX Gateway Fabric by [@sjberman](https://github.com/sjberman) in [#2023](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2023)
  * "non streaming mode" configuration to the SLO-aware router, by [@kaushikmitr](https://github.com/kaushikmitr) in [#2048](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2048)
  * add server side filtering based on namespace if the ns env var is set by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#2077](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2077)
  * [release-1.3] prefill aware prefix plugin by [@k8s-infra-cherrypick-robot](https://github.com/k8s-infra-cherrypick-robot) in [#2106](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2106)
  * [release-1.3] Fixed targetPorts copy error by [@k8s-infra-cherrypick-robot](https://github.com/k8s-infra-cherrypick-robot) in [#2107](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2107)
  * [release-1.3] changed httproute creation to be behind a flag. by [@k8s-infra-cherrypick-robot](https://github.com/k8s-infra-cherrypick-robot) in [#2129](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2129)
  * [release-1.3] rename of experimental http route creation section in helm by [@k8s-infra-cherrypick-robot](https://github.com/k8s-infra-cherrypick-robot) in [#2130](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2130)
  * [release-1.3] fix: [Flow Control]: Optionally disable endpoint subset filtering while dispatching requests by [@k8s-infra-cherrypick-robot](https://github.com/k8s-infra-cherrypick-robot) in [#2155](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2155)
  * [release-1.3] Increase default FlowGCTimeout to 1h to prevent premature GC by [@k8s-infra-cherrypick-robot](https://github.com/k8s-infra-cherrypick-robot) in [#2154](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2154)


## New Contributors
  * [@ezrasilvera](https://github.com/ezrasilvera) made their first contribution in [#1905](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1905)
  * @atharva-310 made their first contribution in [#1902](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1902)
  * [@rohithnarasimha](https://github.com/rohithnarasimha) made their first contribution in [#1918](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1918)
  * [@RyanRosario](https://github.com/RyanRosario) made their first contribution in [#1885](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/1885)
  * [@strangiato](https://github.com/strangiato) made their first contribution in [#2009](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2009)
  * [@MregXN](https://github.com/MregXN) made their first contribution in [#2027](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2027)
  * [@adelsam](https://github.com/adelsam) made their first contribution in [#2058](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2058)
  * [@sjberman](https://github.com/sjberman) made their first contribution in [#2023](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2023)
  * [@k8s-infra-cherrypick-robot](https://github.com/k8s-infra-cherrypick-robot) made their first contribution in [#2106](https://github.com/kubernetes-sigs/gateway-api-inference-extension/pull/2106)


**Full Changelog** : [`v1.2.1...v1.3.0`](https://github.com/kubernetes-sigs/gateway-api-inference-extension/compare/v1.2.1...v1.3.0)
### Contributors
howardjohn, strangiato, and 28 other contributors 
Assets 5
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/tag/v1.3.0).
You can’t perform that action at this time. 
