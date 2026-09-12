---
title: Releases · llm-d/llm-d
id: releases-llm-dllm-d
tags:
- llmd-router-nonk8s-backends-54b774
- llm-d-releases
- standalone-epp
- component-rename
- file-discovery
- non-k8s-backend
created: '2026-09-11T20:45:34.739674Z'
updated: '2026-09-12T03:37:37.654193Z'
source: https://github.com/llm-d/llm-d/releases
source_domain: github.com
fetched_at: '2026-09-11T20:45:34.715838Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'llm-d/llm-d GitHub Releases page, fetched covering v0.3.0 (10 Oct 2025)
  through v0.9.0 (17 Aug 2026). Dated timeline: v0.3.0 (10 Oct 2025), v0.3.1 (6 Nov
  2025), v0.4.0 (26 Nov 2025), v0.5.0 (3 Feb 2026 -- adopted the GIE v1 InferencePool
  API, bumped Gateway API v1.3.0->v1.4.0, KGateway v2.0.3->v2.1.1), v0.5.1 (5 Mar
  2026), v0.6.0 (3 Apr 2026), v0.7.0 (12 May 2026), v0.8.0 (24 Jun 2026), v0.8.1 (26
  Jun 2026), v0.9.0 (17 Aug 2026, latest at fetch time -- release goal tracked in
  issue #1945). Two load-bearing, directly on-topic findings: (1) v0.7.0''s release
  notes announce a UX change verbatim: ''due to the difficulty configuring gateways
  for many adopters, we have made the default deployment of llm-d to use "standalone
  mode" where we use a generic proxy instead of the more feature full gateway. We
  still recommend a fully gateway for customers in production'' -- i.e. llm-d itself
  made standalone/non-full-Gateway-API EPP deployment its DEFAULT as of 12 May 2026,
  confirming standalone mode is not a niche/experimental path but llm-d''s mainline
  onboarding experience. (2) A ''New Capabilities'' table (appearing in the v0.8.0/v0.8.1/v0.9.0
  release window -- see caveat below) lists ''Non-Kubernetes mode (RL/Slurm) | llm-d-router
  (FileDiscovery plugin) | New'' as a shipped capability of llm-d-router -- direct,
  dated confirmation that llm-d''s own router ships a FileDiscovery backend-discovery
  plugin explicitly for non-Kubernetes (RL/Slurm) deployment, which is the single
  most directly relevant fact in this note to a non-k8s-backend router customization
  project; also listed in the same table: ''Agentic workload routing (New)'' and ''Predicted
  latency scheduling (New)'', both on llm-d-router. Component renames confirmed: llm-d-inference-scheduler
  renamed to llm-d-router-endpoint-picker (the core EPP image), and llm-d-routing-sidecar
  renamed to llm-d-router-disagg-sidecar (P/D disaggregation routing sidecar) -- GitHub
  ''Deprecated/Removed Components'' tables explicitly mark both as ''Renamed''. Gateway/proxy
  naming evolution: ''KGateway'' (through v0.5.1, v2.1.1) -> labeled ''agentgateway
  (old KGateway)'' starting at v0.7.0 (v2.2.1) -> by v0.9.0 tracked as an independently-versioned
  ''AgentGateway v1.1.0'' (previously v2.3.3 as kgateway) alongside newly-added infra
  options ''Envoy Gateway v1.8.1'' and ''Envoy AI Gateway v0.7.0''. CAVEAT: the scraped
  page shows apparent verbatim content duplication across the v0.8.0, v0.8.1, and
  v0.9.0 sections (identical Component Summary and New Capabilities tables recur across
  what should be distinct release bodies, interspersed with client-side ''there was
  an error while loading'' placeholders suggesting a rendering/scrape artifact rather
  than a real maintainer copy-paste). The FileDiscovery-plugin and router-rename facts
  themselves are corroborated by explicit ''Renamed''/''New'' status markers and are
  treated as reliable, but their precise attribution to v0.8.0 vs v0.9.0 specifically
  could not be disambiguated from this single fetch -- recommend fetching the individual
  v0.8.0 and v0.9.0 tag release pages directly (e.g. github.com/llm-d/llm-d/releases/tag/v0.8.0)
  to pin down the exact release.'
utility_score: 13.0
---

[Skip to content](https://github.com/llm-d/llm-d/releases#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d/releases) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d/releases) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d/releases) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d) You must be signed in to change notification settings
  * [ Fork 761 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)
  * [ Star  4.5k ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d)


# Releases: llm-d/llm-d
Releases · llm-d/llm-d
## Release list
Previous [Next](https://github.com/llm-d/llm-d/releases?page=2)
Jump to release
  * [ v0.9.0 Release  ](https://github.com/llm-d/llm-d/releases#release-v0.9.0)
  * [ Release v0.8.1  ](https://github.com/llm-d/llm-d/releases#release-v0.8.1)
  * [ Release v0.8.0  ](https://github.com/llm-d/llm-d/releases#release-v0.8.0)
  * [ Release v0.7.0  ](https://github.com/llm-d/llm-d/releases#release-v0.7.0)
  * [ Release v0.6.0  ](https://github.com/llm-d/llm-d/releases#release-v0.6.0)
  * [ Release v0.5.1  ](https://github.com/llm-d/llm-d/releases#release-v0.5.1)
  * [ Release v0.4.0  ](https://github.com/llm-d/llm-d/releases#release-v0.4.0)
  * [ Release v0.5.0  ](https://github.com/llm-d/llm-d/releases#release-v0.5.0)
  * [ v0.3.1 Release  ](https://github.com/llm-d/llm-d/releases#release-v0.3.1)
  * [ v0.3.0  ](https://github.com/llm-d/llm-d/releases#release-v0.3.0)


Previous [Next](https://github.com/llm-d/llm-d/releases?page=2)
## v0.9.0 Release
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
## No results found
[Gregory-Pereira](https://github.com/Gregory-Pereira) released this  17 Aug 23:38 
[ v0.9.0  ](https://github.com/llm-d/llm-d/tree/v0.9.0)
This tag was signed with the committer’s **verified signature**. 
[Gregory-Pereira](https://github.com/Gregory-Pereira) Greg Pereira 
GPG key ID: E457695267DD2C52
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
[ `3291bca`](https://github.com/llm-d/llm-d/commit/3291bca445be5bd309387fa78cc24f487f07003d)
This commit was signed with the committer’s **verified signature**. 
[Gregory-Pereira](https://github.com/Gregory-Pereira) Greg Pereira 
GPG key ID: E457695267DD2C52
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
# llm-d v0.9.0 Release
Release goal and issues tracked here: [#1945](https://github.com/llm-d/llm-d/issues/1945), although not all of that was accomplished. Thank you to all our new and old contributors.
## LLM-D v0.9.0 Component Summary  
| Component  | Version  | Previous Version  | Type  |  
| --- | --- | --- | --- |  
| llm-d/llm-d-router-endpoint-picker  | `v0.10.0`  | `v0.9.0`  | Image + Helm Chart  |  
| llm-d/llm-d-router-disagg-sidecar  | `v0.10.0`  | `v0.9.0`  | Image  |  
| llm-d/llm-d-uds-tokenizer  | —  | `v0.9.0`  | Image  |  
| llm-d/llm-d-kv-cache  | `v0.9.0`  | `v0.8.0`  | Library  |  
| llm-d/llm-d-inference-sim  | `v0.10.2`  | `v0.9.2`  | Image  |  
| llm-d/llm-d-cuda  | `v0.9.0`  | `v0.8.1`  | Image  |  
| llm-d/llm-d-aws (EFA)  | `v0.9.0`  | `v0.8.1`  | Image  |  
| llm-d/llm-d-rocm  | `v0.9.0`  | `v0.8.1`  | Image  |  
| llm-d/llm-d-xpu  | `v0.9.0`  | `v0.8.1`  | Image  |  
| llm-d/llm-d-xpu-sglang  | `v0.9.0`  | —  | Image  |  
| llm-d/llm-d-cpu  | `v0.9.0`  | `v0.8.1`  | Image  |  
| llm-d/llm-d-kv-cache/llmd-fs-connector  | `0.23`  | `0.23`  | Wheel installed in `llm-d`  |  
| llm-d/llm-d-benchmark  | `v0.8.0`  | `v0.6.8.1`  | Image  |  
| llm-d/llm-d-workload-variant-autoscaler  | `v0.9.0`  | `v0.8.0`  | Helm Chart + Image  |  
| llm-d/llm-d-async  | `v0.9.0`  | `v0.8.0`  | Helm Chart + Image  |  
| llm-d/llm-d-batch-gateway  | `v0.5.0`  | —  | Image + Helm Chart  |  
| llm-d/llm-d-latency-predictor  | `0.9.0`  | —  | Image  |  
| llm-d/mooncake-master-store  | `v0.8.0`  | `v0.8.0`  | Image  |  
| vllm-project/vllm  | `v0.26.0`  | `v0.23.0`  | Wheel installed in `llm-d`  |  
| kubernetes-sigs/gateway-api-inference-extension  | `v1.5.0`  | `v1.5.0`  | Helm Chart + CRDs  |  
| kubernetes-sigs/inference-perf  | `v0.6.1`  | `v0.6.0`  | Tool  |  
### Upstream Model Server Images  
| Engine  | Image  | Tag  | Previous Tag  |  
| --- | --- | --- | --- |  
| vLLM  | `docker.io/vllm/vllm-openai`  | `v0.26.0`  | `v0.23.0`  |  
| vLLM Omni  | `docker.io/vllm/vllm-omni`  | `v0.26.0`  | —  |  
| vLLM TPU  | `docker.io/vllm/vllm-tpu`  | `v0.26.0`  | `v0.25.0`  |  
| vLLM XPU  | `docker.io/vllm/vllm-openai-xpu`  | `v0.26.0`  | `v0.23.0`  |  
| vLLM ROCm  | `docker.io/vllm/vllm-openai-rocm`  | `v0.26.0`  | `v0.23.0`  |  
| vLLM ROCm Omni  | `docker.io/vllm/vllm-omni-rocm`  | `v0.24.1`  | —  |  
| vLLM CPU  | `docker.io/vllm/vllm-openai-cpu`  | `v0.26.0`  | `v0.23.0`  |  
| SGLang  | `docker.io/lmsysorg/sglang`  | `v0.5.16`  | `v0.5.13.post1`  |  
| TRT-LLM  | `nvcr.io/nvidia/tensorrt-llm/release`  | `1.3.0rc23`  | —  |  
## Infrastructure Changes  
| Component  | Version  | Previous Version  |  
| --- | --- | --- |  
| Gateway API CRDs  | `v1.5.1`  | `v1.5.1`  |  
| GAIE CRDs  | `v1.5.0`  | `v1.5.0`  |  
| Istio  | `1.29.4`  | `1.29.4`  |  
| AgentGateway  | `v1.1.0`  |  `v2.3.3` (as kgateway)  |  
| Envoy Gateway / Envoy AI Gateway  |  `v1.8.1` / `v0.7.0`  | —  |  
## What's Changed
  * bump wide-ep images by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#1937](https://github.com/llm-d/llm-d/pull/1937)
  * Add benchmark results for SGLang with tiered prefix cache by [@rahulgurnani](https://github.com/rahulgurnani) in [#1921](https://github.com/llm-d/llm-d/pull/1921)
  * pin inference-perf version by [@diegocastanibm](https://github.com/diegocastanibm) in [#1946](https://github.com/llm-d/llm-d/pull/1946)
  * Add Intel XPU vLLM support for multimodal optimized-baseline by [@joshuayao](https://github.com/joshuayao) in [#1772](https://github.com/llm-d/llm-d/pull/1772)
  * docs: list founders inline instead of logos on getting-started intro by [@Ibrahim2595](https://github.com/Ibrahim2595) in [#1960](https://github.com/llm-d/llm-d/pull/1960)
  * Create skeleton for sig-llm-d-inference-payload-processor by [@petecheslock](https://github.com/petecheslock) in [#1499](https://github.com/llm-d/llm-d/pull/1499)
  * Add cleanup parameter to slash command by [@diegocastanibm](https://github.com/diegocastanibm) in [#1961](https://github.com/llm-d/llm-d/pull/1961)
  * update-badge parameter by [@diegocastanibm](https://github.com/diegocastanibm) in [#1951](https://github.com/llm-d/llm-d/pull/1951)
  * docs: add Deploy section to batch-gateway well-lit path by [@lioraron](https://github.com/lioraron) in [#1965](https://github.com/llm-d/llm-d/pull/1965)
  * Update links in kv-offloader documentation to point to the GitHub repo for deployment manifests by [@petecheslock](https://github.com/petecheslock) in [#1962](https://github.com/llm-d/llm-d/pull/1962)
  * fix: Add missing `update_badge` parameters to ci workflows by [@amirfr3](https://github.com/amirfr3) in [#1969](https://github.com/llm-d/llm-d/pull/1969)
  * docs: fix broken well-lit-path links in README by [@Ibrahim2595](https://github.com/Ibrahim2595) in [#1975](https://github.com/llm-d/llm-d/pull/1975)
  * Fix metric names for SGLang in Grafana Dashboards by [@rahulgurnani](https://github.com/rahulgurnani) in [#1970](https://github.com/llm-d/llm-d/pull/1970)
  * docs(flowcontrol): map shutdown-drained requests to 503 by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#1801](https://github.com/llm-d/llm-d/pull/1801)
  * Add Snowflake as a user of llm-d by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1978](https://github.com/llm-d/llm-d/pull/1978)
  * [Docs] Update PD flow to include KV lease extension by [@NickLucche](https://github.com/NickLucche) in [#1543](https://github.com/llm-d/llm-d/pull/1543)
  * deps(actions): bump actions/checkout from 6 to 7 by [@dependabot](https://github.com/dependabot)[bot] in [#1976](https://github.com/llm-d/llm-d/pull/1976)
  * fix: correct the typos in the project by [@AayushSaini101](https://github.com/AayushSaini101) in [#1971](https://github.com/llm-d/llm-d/pull/1971)
  * Add sig-batch-inference to SIGS.md by [@jtechapps](https://github.com/jtechapps) in [#1922](https://github.com/llm-d/llm-d/pull/1922)
  * proposal: Observability integration across the llm-d stack by [@gyliu513](https://github.com/gyliu513) in [#1685](https://github.com/llm-d/llm-d/pull/1685)
  * docs: add XPU WideEP guide backend by [@yao531441](https://github.com/yao531441) in [#1925](https://github.com/llm-d/llm-d/pull/1925)
  * Add Novita as a user of llm-d by [@UranusSeven](https://github.com/UranusSeven) in [#1986](https://github.com/llm-d/llm-d/pull/1986)
  * [ROCm] Docker AMD ROCm image update by [@vcave](https://github.com/vcave) in [#1977](https://github.com/llm-d/llm-d/pull/1977)
  * fix(ci): fix dry_run default value for wide-ep-lws update-badge jobs by [@weizhoublue](https://github.com/weizhoublue) in [#1955](https://github.com/llm-d/llm-d/pull/1955)
  * fix: xpu rdma deviceclass name by [@poussa](https://github.com/poussa) in [#1811](https://github.com/llm-d/llm-d/pull/1811)
  * Support tiered prefix cache on Intel XPU with LMCache connector by [@XinyuYe-Intel](https://github.com/XinyuYe-Intel) in [#1731](https://github.com/llm-d/llm-d/pull/1731)
  * Update version badge to v0.8 by [@chcost](https://github.com/chcost) in [#1989](https://github.com/llm-d/llm-d/pull/1989)
  * [Autoscaling]: Update autoscaling guide OWNERS file by [@lionelvillard](https://github.com/lionelvillard) in [#1990](https://github.com/llm-d/llm-d/pull/1990)
  * [Autoscaling] Unpin WVA release 0.8 by [@lionelvillard](https://github.com/lionelvillard) in [#1979](https://github.com/llm-d/llm-d/pull/1979)
  * chore: Remove superfluous nightly prefix. by [@lionelvillard](https://github.com/lionelvillard) in [#1991](https://github.com/llm-d/llm-d/pull/1991)
  * [Fix] Add new config required by `actions/checkout@v7` by [@maugustosilva](https://github.com/maugustosilva) in [#1992](https://github.com/llm-d/llm-d/pull/1992)
  * [Fix] Add `allow-unsafe-pr-checkout: true` to all instances of `actions/checkout` by [@maugustosilva](https://github.com/maugustosilva) in [#1993](https://github.com/llm-d/llm-d/pull/1993)
  * docs(ipp): add Inference Payload Processor documentation by [@yehuditkerido](https://github.com/yehuditkerido) in [#1884](https://github.com/llm-d/llm-d/pull/1884)
  * docs: fix broken links and script paths in flow control guide by [@ishwar170695](https://github.com/ishwar170695) in [#1997](https://github.com/llm-d/llm-d/pull/1997)
  * Add DigitalOcean as an adopter in ADOPTERS.md by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#2004](https://github.com/llm-d/llm-d/pull/2004)
  * docs(wva): add KEDA support and shared-cluster setup by [@omerap12](https://github.com/omerap12) in [#1792](https://github.com/llm-d/llm-d/pull/1792)
  * Fix return type annotation by [@diegocastanibm](https://github.com/diegocastanibm) in [#2005](https://github.com/llm-d/llm-d/pull/2005)
  * docs: fix typo for vllm attribute by [@zdtsw](https://github.com/zdtsw) in [#2000](https://github.com/llm-d/llm-d/pull/2000)
  * update: add GH nightly for optimized baseline on SGLang by [@zdtsw](https://github.com/zdtsw) in [#1999](https://github.com/llm-d/llm-d/pull/1999)
  * feat(monitoring): ship default EPP Prometheus alerting rules by [@sudoalok](https://github.com/sudoalok) in [#1972](https://github.com/llm-d/llm-d/pull/1972)
  * Add license scan report and status by [@fossabot](https://github.com/fossabot) in [#2013](https://github.com/llm-d/llm-d/pull/2013)
  * Resync nightly matrix by [@diegocastanibm](https://github.com/diegocastanibm) in [#2018](https://github.com/llm-d/llm-d/pull/2018)
  * Add engine-type: sglang label to optimized-baseline kustomization by [@Amit-Berman](https://github.com/Amit-Berman) in [#2012](https://github.com/llm-d/llm-d/pull/2012)
  * docs(agentic-serving): add TPU v7 disaggregated P/D benchmark results and manifests by [@yangspirit](https://github.com/yangspirit) in [#1903](https://github.com/llm-d/llm-d/pull/1903)
  * Add new adopter of llm-d by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#2028](https://github.com/llm-d/llm-d/pull/2028)
  * Docs: add menu-config.json to control the documentation sidebar by [@Ibrahim2595](https://github.com/Ibrahim2595) in [#2021](https://github.com/llm-d/llm-d/pull/2021)
  * Docs: fix broken multimodal guide link in EPP HTTP APIs reference by [@Ibrahim2595](https://github.com/Ibrahim2595) in [#2031](https://github.com/llm-d/llm-d/pull/2031)
  * Prevent Incorrect Load-Aware Routing for AMD SGLang Backends by [@weizhoublue](https://github.com/weizhoublue) in [#2025](https://github.com/llm-d/llm-d/pull/2025)
  * Update README.md with two Docs links by [@Ibrahim2595](https://github.com/Ibrahim2595) in [#2007](https://github.com/llm-d/llm-d/pull/2007)
  * fix(wva): migrate nightly + guide from Prometheus Adapter to KEDA by [@mamy-CS](https://github.com/mamy-CS) in [#2038](https://github.com/llm-d/llm-d/pull/2038)
  * docs(autoscaling): convert EPP autoscaling guide to KEDA by [@nourey](https://github.com/nourey) in [#1981](https://github.com/llm-d/llm-d/pull/1981)
  * fix(wva): authenticate KEDA to Thanos and assert the metric path by [@mamy-CS](https://github.com/mamy-CS) in [#2039](https://github.com/llm-d/llm-d/pull/2039)
  * Initial Details for sig-agentic-inference by [@petecheslock](https://github.com/petecheslock) in [#1868](https://github.com/llm-d/llm-d/pull/1868)
  * feat(guides): add GKE A4X (GB200) P/D disaggregation overlay by [@huaxig](https://github.com/huaxig) in [#2043](https://github.com/llm-d/llm-d/pull/2043)
  * refactor(a4x): update A4X kustomization without gib and patching by [@huaxig](https://github.com/huaxig) in [#2058](https://github.com/llm-d/llm-d/pull/2058)
  * support ipv6 setting by [@eating-chen](https://github.com/eating-chen) in [#1974](https://github.com/llm-d/llm-d/pull/1974)
  * fix: Move the router's grafana dashboard to llm-d/llm-d by [@gyliu513](https://github.com/gyliu513) in [#1984](https://github.com/llm-d/llm-d/pull/1984)
  * fix: CI dry-run and optimized-baseline guide by [@zdtsw](https://github.com/zdtsw) in [#2035](https://github.com/llm-d/llm-d/pull/2035)
  * update(CI): convert wva dry-run job to do recursive find for kustomization by [@zdtsw](https://github.com/zdtsw) in [#2063](https://github.com/llm-d/llm-d/pull/2063)
  * predicted-latency-routing: add the P/D disaggregated deployment and its benchmark by [@kaushikmitr](https://github.com/kaushikmitr) in [#2048](https://github.com/llm-d/llm-d/pull/2048)
  * refactor(gke): restructure GKE overlays to gke/base and gke/a4x by [@liu-cong](https://github.com/liu-cong) in [#2062](https://github.com/llm-d/llm-d/pull/2062)
  * Optimized Baseline Architectural Shift & Updated Benchmarks by [@kaushikmitr](https://github.com/kaushikmitr) in [#1651](https://github.com/llm-d/llm-d/pull/1651)
  * [Minor] Update machine types in GKE README by [@seanhorgan](https://github.com/seanhorgan) in [#2056](https://github.com/llm-d/llm-d/pull/2056)
  * feat(a4xmax): ...


[Read more](https://github.com/llm-d/llm-d/releases/tag/v0.9.0)
### Contributors
seanhorgan, petecheslock, and 76 other contributors 
Assets 2
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
2 people reacted
## Release v0.8.1
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
## No results found
[diegocastanibm](https://github.com/diegocastanibm) released this  26 Jun 19:13 
[ v0.8.1  ](https://github.com/llm-d/llm-d/tree/v0.8.1)
This tag was signed with the committer’s **verified signature**. 
SSH Key Fingerprint: K12D6+rmQtyjMYD0hAthTjBNOzV9updx4VMtU/vjMCk
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
[ `d6ffb5c`](https://github.com/llm-d/llm-d/commit/d6ffb5c7cff80e3953c62f464c08984ccd6a6096)
This commit was signed with the committer’s **verified signature**. 
SSH Key Fingerprint: K12D6+rmQtyjMYD0hAthTjBNOzV9updx4VMtU/vjMCk
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
## LLM-D v0.8.0 Component Summary
> **Themes:** solidify CI coverage & project operations, expand accelerator coverage, graduate multimodal/batch/flow-control to production, introduce initial RL support.  
| Component  | Version  | Previous Version  | Type  | Notes  |  
| --- | --- | --- | --- | --- |  
| llm-d/llm-d-router-endpoint-picker  | `v0.9.0`  | `v0.8.0`  | Image + Helm Chart  | Core EPP image (renamed from llm-d-inference-scheduler)  |  
| llm-d/llm-d-router-disagg-sidecar  | `v0.9.0`  | `v0.8.0`  | Image  | P/D routing sidecar (renamed from llm-d-routing-sidecar)  |  
| llm-d/llm-d-uds-tokenizer  | `vllm-v0.23.0`  | `vllm-v0.19.1`  | Image  | Tokenizer sidecar aligned with vLLM version  |  
| llm-d/llm-d-kv-cache  | `v0.9.0`  | `v0.8.0`  | Library  | HMA support, storage events, multi-tier offloading  |  
| llm-d/llm-d-inference-sim  | `v0.9.2`  | `v0.8.2`  | Image  | Multimodal support, Mooncake bootstrap, configurable latency  |  
| llm-d/llm-d-cuda  | `v0.8.0`  | `v0.7.0`  | Image  | vLLM v0.23.0, CUDA 13.0.2  |  
| llm-d/llm-d-aws (EFA)  | `v0.8.0`  | `v0.7.0`  | Image  |  
| llm-d/llm-d-hpu  | `v0.8.0`  | `v0.7.0`  | Image  |  
| llm-d/llm-d-kv-cache/llmd_fs_backend_connector  | `v0.23`  | `v0.19.1`  | Wheel installed in `llm-d`  | Migrated to vLLM 0.23.0 offload API  |  
| llm-d/llm-d-benchmark  | `v0.7.0`  | `v0.6.8.1`  | Image  | Benchmark workload launcher  |  
| llm-d/llm-d-workload-variant-autoscaler  | `v0.8.0`  | `v0.7.0`  | Helm Chart + Image  | CRD migration to llm-d.ai API group, improved observability  |  
| vllm-project/vllm  | `v0.23.0`  | `v0.19.1`  | Wheel installed in `llm-d`  | Confirmed by [@Gregory-Pereira](https://github.com/Gregory-Pereira) and [@tessapham](https://github.com/tessapham)  |  
| kubernetes-sigs/gateway-api-inference-extension  | `v1.5.0`  | `v1.5.0`  | Helm Chart  | Charts now published from llm-d-router OCI registry  |  
### Upstream vLLM Images (replacing llm-d-built images)
> Per PR [#1791](https://github.com/llm-d/llm-d/pull/1791), the following platforms now use upstream vLLM images directly instead of llm-d-built custom images:  
| Platform  | New Image  | Tag  | Previous llm-d Image  |  
| --- | --- | --- | --- |  
| GPU (CUDA)  | `vllm/vllm-openai`  | `v0.23.0`  |  `ghcr.io/llm-d/llm-d-cuda` (still available for advanced builds)  |  
| ROCm (AMD)  | `vllm/vllm-openai-rocm`  | `ghcr.io/llm-d/llm-d-rocm`  |  
| CPU  | `ghcr.io/llm-d/llm-d-cpu`  | `v0.7.0`  | Same image, version bump  |  
| XPU (Intel)  | `vllm/vllm-openai`  | `v0.23.0`  | `ghcr.io/llm-d/llm-d-xpu`  |  
## Infrastructure Changes  
| Component  | Version  | Previous Version  | Notes  |  
| --- | --- | --- | --- |  
| Gateway API  | `v1.5.1`  | `v1.5.1`  | No change  |  
| Istio  | `1.29.4`  | `1.29.1`  | Patch update  |  
| kgateway (agentgateway)  | `v2.3.3`  | `v2.2.1`  |  
## Deprecated / Removed Components  
| Component  | Status  | Replaced By  |  
| --- | --- | --- |  
| llm-d/llm-d-inference-scheduler  | **Renamed**  | `ghcr.io/llm-d/llm-d-router-endpoint-picker`  |  
| llm-d/llm-d-routing-sidecar  | **Renamed**  | `ghcr.io/llm-d/llm-d-router-disagg-sidecar`  |  
| llm-d/llm-d-cuda (debug)  | **Removed**  | N/A  |  
| llm-d/llm-d-cuda-gb200  | **Removed**  | N/A  |  
| llm-d/llm-d-xpu  | **Removed**  | `vllm/vllm-openai`  |  
| llm-d/llm-d-rocm  | **Removed**  | `vllm/vllm-openai-rocm`  |  
## New Capabilities in This Release  
| Capability  | Component(s)  | Status  |  
| --- | --- | --- |  
| Multi-modal serving (production)  | llm-d-router, llm-d-inference-sim  | Graduated  |  
| Batch gateway (production)  | llm-d-router  | Graduated  |  
| Flow control (production)  | llm-d-router  | Graduated  |  
| Non-Kubernetes mode (RL/Slurm)  | llm-d-router (FileDiscovery plugin)  | New  |  
| Responses API support  | llm-d-router  | New  |  
| Multi-tier KV offloading (CPU → storage)  | llm-d-kv-cache  | New  |  
| HMA (Heterogeneous Memory Allocation) support  | llm-d-kv-cache, fs-connector  | New  |  
| DP-Aware scheduling  | llm-d-router  | Graduating  |  
| Mooncake connector  | llm-d-kv-cache  | New  |  
| Predicted latency scheduling  | llm-d-router  | New  |  
| Agentic workload routing  | llm-d-router  | New  |  
| TPU nightly tests  | CI/CD  | New  |  
## What's Changed
  * Point to patch v0.8.1 by [@maugustosilva](https://github.com/maugustosilva) in commit [`d6ffb5c`](https://github.com/llm-d/llm-d/commit/d6ffb5c7cff80e3953c62f464c08984ccd6a6096)
  * Updated the branch to clone in the guides to release-0.8 branch by [@ahg-g](https://github.com/ahg-g) in [#1958](https://github.com/llm-d/llm-d/pull/1958)
  * Pin inference-perf version by [@diegocastanibm](https://github.com/diegocastanibm) in [#1946](https://github.com/llm-d/llm-d/pull/1946)


**Full Changelog** : [`v0.8.0...v0.8.1`](https://github.com/llm-d/llm-d/compare/v0.8.0...v0.8.1)
### Contributors
maugustosilva, Gregory-Pereira, and 3 other contributors 
Assets 2
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
## Release v0.8.0
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
## No results found
[diegocastanibm](https://github.com/diegocastanibm) released this  24 Jun 21:20 
[ v0.8.0  ](https://github.com/llm-d/llm-d/tree/v0.8.0)
[ `6afd09c`](https://github.com/llm-d/llm-d/commit/6afd09cc686309e3fd721cd9d88db8654c2c079b)
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
## LLM-D v0.8.0 Component Summary
> **Themes:** solidify CI coverage & project operations, expand accelerator coverage, graduate multimodal/batch/flow-control to production, introduce initial RL support.  
| Component  | Version  | Previous Version  | Type  | Notes  |  
| --- | --- | --- | --- | --- |  
| llm-d/llm-d-router-endpoint-picker  | `v0.9.0`  | `v0.8.0`  | Image + Helm Chart  | Core EPP image (renamed from llm-d-inference-scheduler)  |  
| llm-d/llm-d-router-disagg-sidecar  | `v0.9.0`  | `v0.8.0`  | Image  | P/D routing sidecar (renamed from llm-d-routing-sidecar)  |  
| llm-d/llm-d-uds-tokenizer  | `vllm-v0.23.0`  | `vllm-v0.19.1`  | Image  | Tokenizer sidecar aligned with vLLM version  |  
| llm-d/llm-d-kv-cache  | `v0.9.0`  | `v0.8.0`  | Library  | HMA support, storage events, multi-tier offloading  |  
| llm-d/llm-d-inference-sim  | `v0.9.2`  | `v0.8.2`  | Image  | Multimodal support, Mooncake bootstrap, configurable latency  |  
| llm-d/llm-d-cuda  | `v0.8.0`  | `v0.7.0`  | Image  | vLLM v0.23.0, CUDA 13.0.2  |  
| llm-d/llm-d-aws (EFA)  | `v0.8.0`  | `v0.7.0`  | Image  |  
| llm-d/llm-d-hpu  | `v0.8.0`  | `v0.7.0`  | Image  |  
| llm-d/llm-d-kv-cache/llmd_fs_backend_connector  | `v0.23`  | `v0.19.1`  | Wheel installed in `llm-d`  | Migrated to vLLM 0.23.0 offload API  |  
| llm-d/llm-d-benchmark  | `v0.7.0`  | `v0.6.8.1`  | Image  | Benchmark workload launcher  |  
| llm-d/llm-d-workload-variant-autoscaler  | `v0.8.0`  | `v0.7.0`  | Helm Chart + Image  | CRD migration to llm-d.ai API group, improved observability  |  
| vllm-project/vllm  | `v0.23.0`  | `v0.19.1`  | Wheel installed in `llm-d`  | Confirmed by [@Gregory-Pereira](https://github.com/Gregory-Pereira) and [@tessapham](https://github.com/tessapham)  |  
| kubernetes-sigs/gateway-api-inference-extension  | `v1.5.0`  | `v1.5.0`  | Helm Chart  | Charts now published from llm-d-router OCI registry  |  
### Upstream vLLM Images (replacing llm-d-built images)
> Per PR [#1791](https://github.com/llm-d/llm-d/pull/1791), the following platforms now use upstream vLLM images directly instead of llm-d-built custom images:  
| Platform  | New Image  | Tag  | Previous llm-d Image  |  
| --- | --- | --- | --- |  
| GPU (CUDA)  | `vllm/vllm-openai`  | `v0.23.0`  |  `ghcr.io/llm-d/llm-d-cuda` (still available for advanced builds)  |  
| ROCm (AMD)  | `vllm/vllm-openai-rocm`  | `ghcr.io/llm-d/llm-d-rocm`  |  
| CPU  | `ghcr.io/llm-d/llm-d-cpu`  | `v0.7.0`  | Same image, version bump  |  
| XPU (Intel)  | `vllm/vllm-openai`  | `v0.23.0`  | `ghcr.io/llm-d/llm-d-xpu`  |  
## Infrastructure Changes  
| Component  | Version  | Previous Version  | Notes  |  
| --- | --- | --- | --- |  
| Gateway API  | `v1.5.1`  | `v1.5.1`  | No change  |  
| Istio  | `1.29.4`  | `1.29.1`  | Patch update  |  
| kgateway (agentgateway)  | `v2.3.3`  | `v2.2.1`  |  
## Deprecated / Removed Components  
| Component  | Status  | Replaced By  |  
| --- | --- | --- |  
| llm-d/llm-d-inference-scheduler  | **Renamed**  | `ghcr.io/llm-d/llm-d-router-endpoint-picker`  |  
| llm-d/llm-d-routing-sidecar  | **Renamed**  | `ghcr.io/llm-d/llm-d-router-disagg-sidecar`  |  
| llm-d/llm-d-cuda (debug)  | **Removed**  | N/A  |  
| llm-d/llm-d-cuda-gb200  | **Removed**  | N/A  |  
| llm-d/llm-d-xpu  | **Removed**  | `vllm/vllm-openai`  |  
| llm-d/llm-d-rocm  | **Removed**  | `vllm/vllm-openai-rocm`  |  
## New Capabilities in This Release  
| Capability  | Component(s)  | Status  |  
| --- | --- | --- |  
| Multi-modal serving (production)  | llm-d-router, llm-d-inference-sim  | Graduated  |  
| Batch gateway (production)  | llm-d-router  | Graduated  |  
| Flow control (production)  | llm-d-router  | Graduated  |  
| Non-Kubernetes mode (RL/Slurm)  | llm-d-router (FileDiscovery plugin)  | New  |  
| Responses API support  | llm-d-router  | New  |  
| Multi-tier KV offloading (CPU → storage)  | llm-d-kv-cache  | New  |  
| HMA (Heterogeneous Memory Allocation) support  | llm-d-kv-cache, fs-connector  | New  |  
| DP-Aware scheduling  | llm-d-router  | Graduating  |  
| Mooncake connector  | llm-d-kv-cache  | New  |  
| Predicted latency scheduling  | llm-d-router  | New  |  
| Agentic workload routing  | llm-d-router  | New  |  
| TPU nightly tests  | CI/CD  | New  |  
## What's Changed
  * Simplify WVA guide test by [@lionelvillard](https://github.com/lionelvillard) in [#1072](https://github.com/llm-d/llm-d/pull/1072)
  * fix concurrency group to sha not PR by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#1073](https://github.com/llm-d/llm-d/pull/1073)
  * fix block-size alignment by [@vMaroon](https://github.com/vMaroon) in [#1084](https://github.com/llm-d/llm-d/pull/1084)
  * Revise maturity status and TPU VM type details by [@seanhorgan](https://github.com/seanhorgan) in [#1085](https://github.com/llm-d/llm-d/pull/1085)
  * Fix formatting of automated test status in README by [@seanhorgan](https://github.com/seanhorgan) in [#1087](https://github.com/llm-d/llm-d/pull/1087)
  * Fix image on pd user guide by [@Edwinhr716](https://github.com/Edwinhr716) in [#1086](https://github.com/llm-d/llm-d/pull/1086)
  * Updated maturity testing level on all guides by [@maugustosilva](https://github.com/maugustosilva) in [#1094](https://github.com/llm-d/llm-d/pull/1094)
  * Skip latest tag for release candidates by [@diegocastanibm](https://github.com/diegocastanibm) in [#1034](https://github.com/llm-d/llm-d/pull/1034)
  * fix(xpu): enable TP=2 for Qwen3-32B for fixing XPU prefix-cache test failed by [@yuanwu2017](https://github.com/yuanwu2017) in [#1081](https://github.com/llm-d/llm-d/pull/1081)
  * [guides] Add a commented `priorityClassName` for use in nightly CI/CD by [@maugustosilva](https://github.com/maugustosilva) in [#1062](https://github.com/llm-d/llm-d/pull/1062)
  * deps(actions): bump docker/build-push-action from 6 to 7 by [@dependabot](https://github.com/dependabot)[bot] in [#1089](https://github.com/llm-d/llm-d/pull/1089)
  * deps(actions): bump actions/github-script from 7 to 8 by [@dependabot](https://github.com/dependabot)[bot] in [#1091](https://github.com/llm-d/llm-d/pull/1091)
  * deps(actions): bump google-github-actions/auth from 2.1.12 to 3.0.0 by [@dependabot](https://github.com/dependabot)[bot] in [#1090](https://github.com/llm-d/llm-d/pull/1090)
  * deps(actions): bump dorny/paths-filter from 3 to 4 by [@dependabot](https://github.com/dependabot)[bot] in [#1092](https://github.com/llm-d/llm-d/pull/1092)
  * deps(actions): bump docker/login-action from 3 to 4 by [@dependabot](https://github.com/dependabot)[bot] in [#1093](https://github.com/llm-d/llm-d/pull/1093)
  * Add shared drive for sig-rl by [@petecheslock](https://github.com/petecheslock) in [#1109](https://github.com/llm-d/llm-d/pull/1109)
  * Fix llm-d performance dashboard queries by [@danehans](https://github.com/danehans) in [#1098](https://github.com/llm-d/llm-d/pull/1098)
  * Fix/e2e validate single curl pod by [@yuanwu2017](https://github.com/yuanwu2017) in [#1078](https://github.com/llm-d/llm-d/pull/1078)
  * Add Moreh as a contributor to the adopters list by [@hhk7734](https://github.com/hhk7734) in [#1111](https://github.com/llm-d/llm-d/pull/1111)
  * Update guides with status badges. by [@maugustosilva](https://github.com/maugustosilva) in [#1110](https://github.com/llm-d/llm-d/pull/1110)
  * fix: use vllmServe modelCommand in precise-prefix-cache-aware XPU values by [@sharvil10](https://github.com/sharvil10) in [#1101](https://github.com/llm-d/llm-d/pull/1101)
  * Add deployment health-check smoke test for llm-d clusters by [@lisperz](https://github.com/lisperz) in [#767](https://github.com/llm-d/llm-d/pull/767)
  * [1/N] Documentation Revamp by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1100](https://github.com/llm-d/llm-d/pull/1100)
  * fix: split vLLM command array and fill in quickstart TODOs by [@madhugoutham](https://github.com/madhugoutham) in [#1126](https://github.com/llm-d/llm-d/pull/1126)
  * add OCI well-lit-path for P/D disaggregation by [@hexfusion](https://github.com/hexfusion) in [#1123](https://github.com/llm-d/llm-d/pull/1123)
  * fix: use the proper git ref for tag extraction by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#1133](https://github.com/llm-d/llm-d/pull/1133)
  * Update latency-predictor.md diagram by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#1127](https://github.com/llm-d/llm-d/pull/1127)
  * docs: fix typos detected by nightly scan (issue [#1056](https://github.com/llm-d/llm-d/issues/1056)) by [@ianliuy](https://github.com/ianliuy) in [#1135](https://github.com/llm-d/llm-d/pull/1135)
  * [Docs][4/N] Update architecture/core/epp/scheduling.md docs by [@ahg-g](https://github.com/ahg-g) in [#1122](https://github.com/llm-d/llm-d/pull/1122)
  * [2/N] Proxy Doc by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1121](https://github.com/llm-d/llm-d/pull/1121)
  * [Docs][6/N]: add Istio gateway setup guide by [@madhugoutham](https://github.com/madhugoutham) in [#1146](https://github.com/llm-d/llm-d/pull/1146)
  * docs: add EPP flow control reference by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#1130](https://github.com/llm-d/llm-d/pull/1130)
  * [Docs] [3/N] Add Disaggregation Architecture Docs by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1138](https://github.com/llm-d/llm-d/pull/1138)
  * [Docs] Add glossary page by [@ianliuy](https://github.com/ianliuy) in [#1148](https://github.com/llm-d/llm-d/pull/1148)
  * Add agentgateway guide by [@danehans](https://github.com/danehans) in [#1159](https://github.com/llm-d/llm-d/pull/1159)
  * fix(docs): correct typos from nightly scan, add false-positive config by [@ianliuy](https://github.com/ianliuy) in [#1160](https://github.com/llm-d/llm-d/pull/1160)
  * [Docs] Guides Directory Cleanup by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1163](https://github.com/llm-d/llm-d/pull/1163)
  * [Docs][EPP] Doc for request handling and control by [@zetxqx](https://github.com/zetxqx) in [#1128](https://github.com/llm-d/llm-d/pull/1128)
  * fix(docker): add LIBRARY_PATH and ldconfig to CUDA runtime stage by [@ianliuy](https://github.com/ianliuy) in [#1147](https://github.com/llm-d/llm-d/pull/1147)
  * Update ms-inference-scheduling/values_tpu_v7.yaml to use RunAI model streamer by [@amacaskill](https://github.com/amacaskill) in [#1102](https://github.com/llm-d/llm-d/pull/1102)
  * deps(actions): bump hashicorp/setup-terraform from 3.1.2 to 4.0.0 by [@dependabot](https://github.com/dependabot)[bot] in [#1153](https://github.com/llm-d/llm-d/pull/1153)
  * deps(actions): bump aws-actions/configure-aws-credentials from 4.3.1 to 6.1.0 by [@dependabot](https://github.com/dependabot)[bot] in [#1152](https://github.com/llm-d/llm-d/pull/1152)
  * deps(actions): bump actions/github-script from 8 to 9 by [@dependabot](https://github.com/dependabot)[bot] in [#1150](https://github.com/llm-d/llm-d/pull/1150)
  * deps(actions): bump j178/prek-action from 1 to 2 by [@dependabot](https://github.com/dependabot)[bot] in [#1151](https://github.com/llm-d/llm-d/pull/1151)
  * deps(actions): bump actions/upload-artifact from 6 to 7 by [@dependabot](https://github.com/dependabot)[bot] in [#1149](https://github.com/llm-d/llm-d/pull/1149)
  * [Docs] Remove `customizing-a-guide.md` by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1165](https://github.com/llm-d/llm-d/pull/1165)
  * [Docs] Move `guides/benchmarks` to `helpers/benchmark.md` by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1164](https://github.com/llm-d/llm-d/pull/1164)
  * [Docs][Istio] Align to AgentGateway Doc by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1161](https://github.com/llm-d/llm-d/pull/1161)
  * deps(docker): bump gdrcopy from v2.5.1 to v2.5.2 by [@ianliuy](https://github.com/ianliuy) in [#1171](https://github.com/llm-d/llm-d/pull/1171)
  * [Docs] Envoy Proxy -> GAIE-Conformant Proxy by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1162](https://github.com/llm-d/llm-d/pull/1162)
  * Updated the basic architecture diagram by [@ahg-g](https://github.com/ahg-g) in [#1173](https://github.com/llm-d/llm-d/pull/1173)
  * [Docs] Fix Broken Quickstart Links by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1175](https://github.com/llm-d/llm-d/pull/1175)
  * [Docs] Remove guide/pre...


[Read more](https://github.com/llm-d/llm-d/releases/tag/v0.8.0)
### Contributors
clubanderson, seanhorgan, and 90 other contributors 
Assets 2
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
1 person reacted
## Release v0.7.0
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
## No results found
[diegocastanibm](https://github.com/diegocastanibm) released this  12 May 19:15 
[ v0.7.0  ](https://github.com/llm-d/llm-d/tree/v0.7.0)
This tag was signed with the committer’s **verified signature**. 
[Gregory-Pereira](https://github.com/Gregory-Pereira) Greg Pereira 
GPG key ID: 2249E56187C0D043
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
[ `a47c3bd`](https://github.com/llm-d/llm-d/commit/a47c3bd4e296c54b25f957f314317c974ad4ebbb)
This commit was signed with the committer’s **verified signature**. 
SSH Key Fingerprint: K12D6+rmQtyjMYD0hAthTjBNOzV9updx4VMtU/vjMCk
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
## LLM-D Component Summary
  * **⚠️ BREAKING CHANGE — CUDA 13.0.2 runtime:** All llm-d CUDA images now ship with CUDA 13.0.2 (upgraded from 12.x). This requires **NVIDIA driver 580 or later** on the host. Nodes running older drivers must be upgraded before deploying v0.7.0 images.
  * UX Change - due to the difficulty configuring gateways for many adopters, we have made the default deployment of llm-d to use "standalone mode" where we use a generic proxy instead of the more feature full gateway. We still recomend a fully gateway for customers in production.

  
| Component  | Version  | Previous Version  | Type  |  
| --- | --- | --- | --- |  
| llm-d/llm-d-inference-scheduler  | `v0.8.0`  | `v0.7.1`  | Image  |  
| llm-d/llm-d-uds-tokenizer  | `vllm-v0.19.1`  | `v0.7.1`  | Image  |  
| llm-d/llm-d-kv-cache  | `v0.8.0`  | `v0.7.1`  | Library  |  
| llm-d/llm-d-routing-sidecar  | `v0.8.0`  | `v0.7.1`  | Image  |  
| llm-d/llm-d-inference-sim  | `v0.8.2`  | `v0.7.1`  | Image  |  
| llm-d/llm-d-cuda  | `v0.7.0`  | `v0.6.0`  | Image  |  
| llm-d/llm-d-cuda (debug)  | `v0.7.0`  | `v0.6.0`  | Image  |  
| llm-d/llm-d-cuda-gb200  | `v0.7.0`  | N/A  | Image (New)  |  
| llm-d/llm-d-aws (EFA)  | `v0.7.0`  | `v0.6.0`  | Image  |  
| llm-d/llm-d-xpu  | `v0.7.0`  | `v0.6.0`  | Image  |  
| llm-d/llm-d-hpu  | `v0.7.0`  | `v0.6.0`  | Image  |  
| llm-d/llm-d-cpu  | `v0.7.0`  | `v0.6.0`  | Image  |  
| llm-d/llm-d-rocm  | `v0.7.0`  | `v0.6.0`  | Image  |  
| llm-d/llm-d-kv-cache/llmd_fs_backend_connector  | `v0.19.1`  | `v0.17.1`  | Wheel installed in `llm-d`  |  
| llm-d/llm-d-workload-variant-autoscaler  | `v0.7.0`  | `v0.6.0`  | Helm Chart + Image  |  
| llm-d-incubation/llm-d-infra (Deprecated)  | N/A  | `v1.4.0`  | Helm Chart  |  
| llm-d-incubation/llm-d-modelservice (Deprecated)  | N/A  | `v0.4.9`  | Helm Chart  |  
| vllm-project/vllm  | `v0.19.1`  | `v0.17.1`  | Wheel installed in `llm-d`  |  
| kubernetes-sigs/gateway-api-inference-extension  | `v1.5.0`  | `v1.4.0`  | Helm Chart  |  
## Infrastructure Changes  
| Component  | Version  | Previous Version  |  
| --- | --- | --- |  
| Gateway API  | `v1.5.1`  | `v1.4.0`  |  
| Istio  | `1.29.1`  | `1.28.1`  |  
| agentgateway (old KGateway)  | `v2.2.1`  | `v2.1.1`  |  
## What's Changed
  * Simplify WVA guide test by [@lionelvillard](https://github.com/lionelvillard) in [#1072](https://github.com/llm-d/llm-d/pull/1072)
  * fix concurrency group to sha not PR by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#1073](https://github.com/llm-d/llm-d/pull/1073)
  * fix block-size alignment by [@vMaroon](https://github.com/vMaroon) in [#1084](https://github.com/llm-d/llm-d/pull/1084)
  * Revise maturity status and TPU VM type details by [@seanhorgan](https://github.com/seanhorgan) in [#1085](https://github.com/llm-d/llm-d/pull/1085)
  * Fix formatting of automated test status in README by [@seanhorgan](https://github.com/seanhorgan) in [#1087](https://github.com/llm-d/llm-d/pull/1087)
  * Fix image on pd user guide by [@Edwinhr716](https://github.com/Edwinhr716) in [#1086](https://github.com/llm-d/llm-d/pull/1086)
  * Updated maturity testing level on all guides by [@maugustosilva](https://github.com/maugustosilva) in [#1094](https://github.com/llm-d/llm-d/pull/1094)
  * Skip latest tag for release candidates by [@diegocastanibm](https://github.com/diegocastanibm) in [#1034](https://github.com/llm-d/llm-d/pull/1034)
  * fix(xpu): enable TP=2 for Qwen3-32B for fixing XPU prefix-cache test failed by [@yuanwu2017](https://github.com/yuanwu2017) in [#1081](https://github.com/llm-d/llm-d/pull/1081)
  * [guides] Add a commented `priorityClassName` for use in nightly CI/CD by [@maugustosilva](https://github.com/maugustosilva) in [#1062](https://github.com/llm-d/llm-d/pull/1062)
  * deps(actions): bump docker/build-push-action from 6 to 7 by [@dependabot](https://github.com/dependabot)[bot] in [#1089](https://github.com/llm-d/llm-d/pull/1089)
  * deps(actions): bump actions/github-script from 7 to 8 by [@dependabot](https://github.com/dependabot)[bot] in [#1091](https://github.com/llm-d/llm-d/pull/1091)
  * deps(actions): bump google-github-actions/auth from 2.1.12 to 3.0.0 by [@dependabot](https://github.com/dependabot)[bot] in [#1090](https://github.com/llm-d/llm-d/pull/1090)
  * deps(actions): bump dorny/paths-filter from 3 to 4 by [@dependabot](https://github.com/dependabot)[bot] in [#1092](https://github.com/llm-d/llm-d/pull/1092)
  * deps(actions): bump docker/login-action from 3 to 4 by [@dependabot](https://github.com/dependabot)[bot] in [#1093](https://github.com/llm-d/llm-d/pull/1093)
  * Add shared drive for sig-rl by [@petecheslock](https://github.com/petecheslock) in [#1109](https://github.com/llm-d/llm-d/pull/1109)
  * Fix llm-d performance dashboard queries by [@danehans](https://github.com/danehans) in [#1098](https://github.com/llm-d/llm-d/pull/1098)
  * Fix/e2e validate single curl pod by [@yuanwu2017](https://github.com/yuanwu2017) in [#1078](https://github.com/llm-d/llm-d/pull/1078)
  * Add Moreh as a contributor to the adopters list by [@hhk7734](https://github.com/hhk7734) in [#1111](https://github.com/llm-d/llm-d/pull/1111)
  * Update guides with status badges. by [@maugustosilva](https://github.com/maugustosilva) in [#1110](https://github.com/llm-d/llm-d/pull/1110)
  * fix: use vllmServe modelCommand in precise-prefix-cache-aware XPU values by [@sharvil10](https://github.com/sharvil10) in [#1101](https://github.com/llm-d/llm-d/pull/1101)
  * Add deployment health-check smoke test for llm-d clusters by [@lisperz](https://github.com/lisperz) in [#767](https://github.com/llm-d/llm-d/pull/767)
  * [1/N] Documentation Revamp by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1100](https://github.com/llm-d/llm-d/pull/1100)
  * fix: split vLLM command array and fill in quickstart TODOs by [@madhugoutham](https://github.com/madhugoutham) in [#1126](https://github.com/llm-d/llm-d/pull/1126)
  * add OCI well-lit-path for P/D disaggregation by [@hexfusion](https://github.com/hexfusion) in [#1123](https://github.com/llm-d/llm-d/pull/1123)
  * fix: use the proper git ref for tag extraction by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#1133](https://github.com/llm-d/llm-d/pull/1133)
  * Update latency-predictor.md diagram by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#1127](https://github.com/llm-d/llm-d/pull/1127)
  * docs: fix typos detected by nightly scan (issue [#1056](https://github.com/llm-d/llm-d/issues/1056)) by [@ianliuy](https://github.com/ianliuy) in [#1135](https://github.com/llm-d/llm-d/pull/1135)
  * [Docs][4/N] Update architecture/core/epp/scheduling.md docs by [@ahg-g](https://github.com/ahg-g) in [#1122](https://github.com/llm-d/llm-d/pull/1122)
  * [2/N] Proxy Doc by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1121](https://github.com/llm-d/llm-d/pull/1121)
  * [Docs][6/N]: add Istio gateway setup guide by [@madhugoutham](https://github.com/madhugoutham) in [#1146](https://github.com/llm-d/llm-d/pull/1146)
  * docs: add EPP flow control reference by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#1130](https://github.com/llm-d/llm-d/pull/1130)
  * [Docs] [3/N] Add Disaggregation Architecture Docs by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1138](https://github.com/llm-d/llm-d/pull/1138)
  * [Docs] Add glossary page by [@ianliuy](https://github.com/ianliuy) in [#1148](https://github.com/llm-d/llm-d/pull/1148)
  * Add agentgateway guide by [@danehans](https://github.com/danehans) in [#1159](https://github.com/llm-d/llm-d/pull/1159)
  * fix(docs): correct typos from nightly scan, add false-positive config by [@ianliuy](https://github.com/ianliuy) in [#1160](https://github.com/llm-d/llm-d/pull/1160)
  * [Docs] Guides Directory Cleanup by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1163](https://github.com/llm-d/llm-d/pull/1163)
  * [Docs][EPP] Doc for request handling and control by [@zetxqx](https://github.com/zetxqx) in [#1128](https://github.com/llm-d/llm-d/pull/1128)
  * fix(docker): add LIBRARY_PATH and ldconfig to CUDA runtime stage by [@ianliuy](https://github.com/ianliuy) in [#1147](https://github.com/llm-d/llm-d/pull/1147)
  * Update ms-inference-scheduling/values_tpu_v7.yaml to use RunAI model streamer by [@amacaskill](https://github.com/amacaskill) in [#1102](https://github.com/llm-d/llm-d/pull/1102)
  * deps(actions): bump hashicorp/setup-terraform from 3.1.2 to 4.0.0 by [@dependabot](https://github.com/dependabot)[bot] in [#1153](https://github.com/llm-d/llm-d/pull/1153)
  * deps(actions): bump aws-actions/configure-aws-credentials from 4.3.1 to 6.1.0 by [@dependabot](https://github.com/dependabot)[bot] in [#1152](https://github.com/llm-d/llm-d/pull/1152)
  * deps(actions): bump actions/github-script from 8 to 9 by [@dependabot](https://github.com/dependabot)[bot] in [#1150](https://github.com/llm-d/llm-d/pull/1150)
  * deps(actions): bump j178/prek-action from 1 to 2 by [@dependabot](https://github.com/dependabot)[bot] in [#1151](https://github.com/llm-d/llm-d/pull/1151)
  * deps(actions): bump actions/upload-artifact from 6 to 7 by [@dependabot](https://github.com/dependabot)[bot] in [#1149](https://github.com/llm-d/llm-d/pull/1149)
  * [Docs] Remove `customizing-a-guide.md` by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1165](https://github.com/llm-d/llm-d/pull/1165)
  * [Docs] Move `guides/benchmarks` to `helpers/benchmark.md` by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1164](https://github.com/llm-d/llm-d/pull/1164)
  * [Docs][Istio] Align to AgentGateway Doc by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1161](https://github.com/llm-d/llm-d/pull/1161)
  * deps(docker): bump gdrcopy from v2.5.1 to v2.5.2 by [@ianliuy](https://github.com/ianliuy) in [#1171](https://github.com/llm-d/llm-d/pull/1171)
  * [Docs] Envoy Proxy -> GAIE-Conformant Proxy by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1162](https://github.com/llm-d/llm-d/pull/1162)
  * Updated the basic architecture diagram by [@ahg-g](https://github.com/ahg-g) in [#1173](https://github.com/llm-d/llm-d/pull/1173)
  * [Docs] Fix Broken Quickstart Links by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1175](https://github.com/llm-d/llm-d/pull/1175)
  * [Docs] Remove guide/prereq/infrastructure by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1168](https://github.com/llm-d/llm-d/pull/1168)
  * Added GKE gateway guide by [@ahg-g](https://github.com/ahg-g) in [#1174](https://github.com/llm-d/llm-d/pull/1174)
  * [Docs] Move Client Tools from Guides --> Helpers by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1167](https://github.com/llm-d/llm-d/pull/1167)
  * feat: add Rebellions as a supported accelerator vendor by [@rebel-jinmoo](https://github.com/rebel-jinmoo) in [#1115](https://github.com/llm-d/llm-d/pull/1115)
  * docs: rewrite predicted latency architecture and add well-lit path by [@kaushikmitr](https://github.com/kaushikmitr) in [#1166](https://github.com/llm-d/llm-d/pull/1166)
  * [Docs][Autoscaling][2/N] hpa/keda vs hpa design choices/features by [@lionelvillard](https://github.com/lionelvillard) in [#1157](https://github.com/llm-d/llm-d/pull/1157)
  * [Docs][7/N] KV Indexer by [@vMaroon](https://github.com/vMaroon) in [#1143](https://github.com/llm-d/llm-d/pull/1143)
  * [Docs][autoscaling][1/N] Autoscaling intro by [@lionelvillard](https://github.com/lionelvillard) in [#1145](https://github.com/llm-d/llm-d/pull/1145)
  * [Docs] Refine RDMA docs by [@praveingk](https://github.com/praveingk) in [#1181](https://github.com/llm-d/llm-d/pull/1181)
  * Fixup merge conflict markers in `tiered-prefix-cache/storage/README.md` by [@tlrmchlsmth](https://github.com/tlrmchlsmth) in [#1190](https://github.com/llm-d/llm-d/pull/1190)
  * [Docs] fix broken link by [@lionelvillard](https://github.com/lionelvillard) in [#1191](https://github.com/llm-d/llm-d/pull/1191)
  * docs: llm-d-inference-payload-processor proposal by [@nilig](https://github.com/nilig) in [#1184](https://github.com/llm-d/llm-d/pull/1184)
  * [Docs] Well Lit Paths Doc by [@chcost](https://github.com/chcost) in [#1156](https://github.com/llm-d/llm-d/pull/1156)
  * Tweaks to the latency predictor docs by [@ahg-g](https://github.com/ahg-g) in [#1201](https://github.com/llm-d/llm-d/pull/1201)
  * [Docs] Restructure Predicted Latency Well Lit Path Doc by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#1200](https://github.com/llm-d/llm-d/pull/1200)
  * [Docs][Autoscaling][4/N] first pass at the HPA+IGW doc by [@lionelvillard](https://github.com/lionelvillard) in [#1192](https://github.com/llm-d/llm-d/pull/1192)
  * Updating native HPA-based autoscaling guide to reference EPP instead of IGW by [@ahg-g](https://github.com/ahg-g) in [#1212](https://github.com/llm-d/llm-d/pull/1212)
  * [Docs] Rename Well-Lit-Paths -> Guides, Guides -> Resources by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in <https://github.com/llm-d/llm-d/pul>...


[Read more](https://github.com/llm-d/llm-d/releases/tag/v0.7.0)
### Contributors
clubanderson, seanhorgan, and 53 other contributors 
Assets 2
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
5 people reacted
## Release v0.6.0
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
## No results found
[maugustosilva](https://github.com/maugustosilva) released this  03 Apr 16:48 
[ v0.6.0  ](https://github.com/llm-d/llm-d/tree/v0.6.0)
[ `899f851`](https://github.com/llm-d/llm-d/commit/899f8517faad3b9255e1dca7430cb864a3cd30da)
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
## LLM-D Component Summary  
| Component  | Version  | Previous Version  | Type  |  
| --- | --- | --- | --- |  
| llm-d/llm-d-inference-scheduler  | `v0.7.1`  | `v0.6.0`  | Image  |  
| llm-d/llm-d-uds-tokenizer  | `v0.7.1`  | `v0.6.0`  | Image  |  
| llm-d/llm-d-kv-cache  | `v0.7.1`  | `v0.6.0`  | Library  |  
| llm-d/llm-d-routing-sidecar  | `v0.7.1`  | `v0.6.0`  | Image  |  
| llm-d/llm-d-inference-sim  | `v0.8.2`  | `v0.7.1`  | Image  |  
| llm-d/llm-d-cuda  | `v0.6.0`  | `v0.5.1`  | Image  |  
| llm-d/llm-d-cuda (debug)  | `v0.6.0`  | `v0.5.1`  | Image  |  
| llm-d/llm-d-aws (EFA)  | `v0.6.0`  | `v0.5.1`  | Image  |  
| llm-d/llm-d-xpu  | `v0.6.0`  | `v0.5.1`  | Image (Temporarily Unavailable)  |  
| llm-d/llm-d-hpu  | `v0.6.0`  | `v0.5.1`  | Image (Temporarily Unavailable)  |  
| llm-d/llm-d-cpu  | `v0.6.0`  | `v0.5.1`  | Image  |  
| llm-d/llm-d-rocm  | `v0.6.0`  | `v0.5.1`  | Image  |  
| llm-d/llm-d-kv-cache/llmd_fs_backend_connector  | `v0.17.1`  | `v0.15.1`  | Wheel installed in `llm-d`  |  
| llm-d/llm-d-workload-variant-autoscaler  | `v0.6.0`  | `v0.5.1`  | Helm Chart + Image  |  
| llm-d-incubation/llm-d-infra  | `v1.4.0`  | `v1.3.6`  | Helm Chart  |  
| llm-d-incubation/llm-d-modelservice  | `v0.4.9`  | `v0.4.7`  | Helm Chart  |  
| vllm-project/vllm  | `v0.17.1`  | `v0.15.1`  | Wheel installed in `llm-d`  |  
| kubernetes-sigs/gateway-api-inference-extension  | `v1.4.0`  | `v1.3.1`  | Helm Chart  |  
## Infrastructure Changes  
| Component  | Version  | Previous Version  |  
| --- | --- | --- |  
| Gateway API  | `v1.5.1`  | `v1.4.0`  |  
| Istio  | `1.29.1`  | `1.28.1`  |  
| agentgateway (old KGateway)  | `v2.2.1`  | `v2.1.1`  |  
## What's Changed
  * Add SGLang option for inference-scheduling well-lit path by [@andreyod](https://github.com/andreyod) in [#527](https://github.com/llm-d/llm-d/pull/527)
  * add step to build hpu by [@diegocastanibm](https://github.com/diegocastanibm) in [#917](https://github.com/llm-d/llm-d/pull/917)
  * 🌱 Remove per-repo gh-aw typo/link/upstream workflows by [@clubanderson](https://github.com/clubanderson) in [#920](https://github.com/llm-d/llm-d/pull/920)
  * docs: Small fixes in the inference-scheduling installation guide by [@roytman](https://github.com/roytman) in [#924](https://github.com/llm-d/llm-d/pull/924)
  * Fix link to vLLM Native CPU Offloading documentation by [@petecheslock](https://github.com/petecheslock) in [#928](https://github.com/llm-d/llm-d/pull/928)
  * docs: Remove unnecessary backticks around llm-d by [@terrytangyuan](https://github.com/terrytangyuan) in [#933](https://github.com/llm-d/llm-d/pull/933)
  * Partial enablement of CICD for GKE by [@maugustosilva](https://github.com/maugustosilva) in [#934](https://github.com/llm-d/llm-d/pull/934)
  * Add hpu to ci-release by [@diegocastanibm](https://github.com/diegocastanibm) in [#918](https://github.com/llm-d/llm-d/pull/918)
  * Only report on 404 broken links and not temp issues or scraper blocks by [@petecheslock](https://github.com/petecheslock) in [#846](https://github.com/llm-d/llm-d/pull/846)
  * hpu: update images by [@poussa](https://github.com/poussa) in [#936](https://github.com/llm-d/llm-d/pull/936)
  * fix: correct typos in documentation and code comments by [@BaskDuan](https://github.com/BaskDuan) in [#973](https://github.com/llm-d/llm-d/pull/973)
  * Fix the HPU failed issue and enable HPU PR checks in CI by [@yuanwu2017](https://github.com/yuanwu2017) in [#965](https://github.com/llm-d/llm-d/pull/965)
  * chore: update xpu image build inputs by [@VincyZhang](https://github.com/VincyZhang) in [#911](https://github.com/llm-d/llm-d/pull/911)
  * Add HPA + IGW metrics autoscaling guide to the autoscaling well-lit path by [@aishukamal](https://github.com/aishukamal) in [#972](https://github.com/llm-d/llm-d/pull/972)
  * deps(actions): bump actions/checkout from 4 to 6 by [@dependabot](https://github.com/dependabot)[bot] in [#836](https://github.com/llm-d/llm-d/pull/836)
  * Build llm-d-cuda locally by [@diegocastanibm](https://github.com/diegocastanibm) in [#865](https://github.com/llm-d/llm-d/pull/865)
  * build: add CUDA versions into central build file by [@zdtsw](https://github.com/zdtsw) in [#982](https://github.com/llm-d/llm-d/pull/982)
  * Mark myself as inactive project maintainer while on leave by [@smarterclayton](https://github.com/smarterclayton) in [#985](https://github.com/llm-d/llm-d/pull/985)
  * update the result for inference-scheduling and precise-prefix-cache-aware by [@zetxqx](https://github.com/zetxqx) in [#722](https://github.com/llm-d/llm-d/pull/722)
  * install storage offloading connector by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#640](https://github.com/llm-d/llm-d/pull/640)
  * try set and unset cuda stubs in LIBRARY_PATH by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#987](https://github.com/llm-d/llm-d/pull/987)
  * upgrading to vLLM v0.17.1 by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#977](https://github.com/llm-d/llm-d/pull/977)
  * Refresh support for kgateway agentgateway GIE and GW API by [@danehans](https://github.com/danehans) in [#421](https://github.com/llm-d/llm-d/pull/421)
  * Update gke inference scheduling test to address failures by [@rlakhtakia](https://github.com/rlakhtakia) in [#649](https://github.com/llm-d/llm-d/pull/649)
  * deps(actions): bump google-github-actions/setup-gcloud from 2.2.0 to 3.0.1 by [@dependabot](https://github.com/dependabot)[bot] in [#835](https://github.com/llm-d/llm-d/pull/835)
  * updating common configurations with infra refactor 272 by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#997](https://github.com/llm-d/llm-d/pull/997)
  * ci test helm chart schemas + helmfile examples + dry run k8s apply by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#998](https://github.com/llm-d/llm-d/pull/998)
  * any build triggered from push to main should be latest by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#996](https://github.com/llm-d/llm-d/pull/996)
  * fix: use VLLM_PRECOMPILED_WHEEL_COMMIT from vllm 0.17.1 by [@zdtsw](https://github.com/zdtsw) in [#1007](https://github.com/llm-d/llm-d/pull/1007)
  * enable building from open PRs by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#1011](https://github.com/llm-d/llm-d/pull/1011)
  * build: reduce cuda runtime image size by switch base image by [@zdtsw](https://github.com/zdtsw) in [#606](https://github.com/llm-d/llm-d/pull/606)
  * github: pin to a version for trivy than mastser by [@zdtsw](https://github.com/zdtsw) in [#1018](https://github.com/llm-d/llm-d/pull/1018)
  * ci: use pull_request for upstream PRs, pull_request_target for forks by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#1023](https://github.com/llm-d/llm-d/pull/1023)
  * removing sm70 and 75 because lack of cuda 12.9.X support rdma-tools by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#1024](https://github.com/llm-d/llm-d/pull/1024)
  * docs: llm-d planner proposal by [@anfredette](https://github.com/anfredette) in [#963](https://github.com/llm-d/llm-d/pull/963)
  * Image verification workflow by [@diegocastanibm](https://github.com/diegocastanibm) in [#1010](https://github.com/llm-d/llm-d/pull/1010)
  * deps(actions): bump actions/setup-go from 5 to 6 by [@dependabot](https://github.com/dependabot)[bot] in [#1016](https://github.com/llm-d/llm-d/pull/1016)
  * deps(actions): bump github/codeql-action from 3 to 4 by [@dependabot](https://github.com/dependabot)[bot] in [#1015](https://github.com/llm-d/llm-d/pull/1015)
  * deps(actions): bump lycheeverse/lychee-action from 2.7.0 to 2.8.0 by [@dependabot](https://github.com/dependabot)[bot] in [#930](https://github.com/llm-d/llm-d/pull/930)
  * Add benchmarking numbers for Lustre with LMcache connector by [@Sneha-at](https://github.com/Sneha-at) in [#1020](https://github.com/llm-d/llm-d/pull/1020)
  * Add P/D Disaggregation with RoCE/GDR for OpenShift by [@maugustosilva](https://github.com/maugustosilva) in [#1025](https://github.com/llm-d/llm-d/pull/1025)
  * Move benchmark templates into the specific guide directories by [@deanlorenz](https://github.com/deanlorenz) in [#784](https://github.com/llm-d/llm-d/pull/784)
  * build: add infiniband-diags to rdma-tools runtime image by [@rajkiranjoshi](https://github.com/rajkiranjoshi) in [#1040](https://github.com/llm-d/llm-d/pull/1040)
  * fix: typos from nightly scan by [@tessapham](https://github.com/tessapham) in [#1037](https://github.com/llm-d/llm-d/pull/1037)
  * Attempted fix for CI/CD GKE by explicitly exporting LD_LIBRARY_PATH by [@maugustosilva](https://github.com/maugustosilva) in [#1032](https://github.com/llm-d/llm-d/pull/1032)
  * Change `HELMFILE_ENV` on P/D Disaggregation (OpenShift) to `ocp` by [@maugustosilva](https://github.com/maugustosilva) in [#1041](https://github.com/llm-d/llm-d/pull/1041)
  * Adding Topology Aware Scheduling labels on Wide-EP GKE example by [@Edwinhr716](https://github.com/Edwinhr716) in [#531](https://github.com/llm-d/llm-d/pull/531)
  * Fix `llm-d.ai/guide` for pods on PD guide by [@maugustosilva](https://github.com/maugustosilva) in [#1047](https://github.com/llm-d/llm-d/pull/1047)
  * Async Processor well-lit path guide by [@shimib](https://github.com/shimib) in [#1031](https://github.com/llm-d/llm-d/pull/1031)
  * And llm_d_ref defaults to main by [@diegocastanibm](https://github.com/diegocastanibm) in [#1049](https://github.com/llm-d/llm-d/pull/1049)
  * docs: expand suspended hyphens in Inference Scheduler description by [@skmahe1077](https://github.com/skmahe1077) in [#1050](https://github.com/llm-d/llm-d/pull/1050)
  * deps(actions): bump docker/setup-buildx-action from 3 to 4 by [@dependabot](https://github.com/dependabot)[bot] in [#1044](https://github.com/llm-d/llm-d/pull/1044)
  * deps(actions): bump actions/github-script from 7 to 8 by [@dependabot](https://github.com/dependabot)[bot] in [#1043](https://github.com/llm-d/llm-d/pull/1043)
  * deps(actions): bump docker/metadata-action from 5 to 6 by [@dependabot](https://github.com/dependabot)[bot] in [#1042](https://github.com/llm-d/llm-d/pull/1042)
  * llm-d skills proposal by [@rachelt44](https://github.com/rachelt44) in [#1039](https://github.com/llm-d/llm-d/pull/1039)
  * Additional fix for gke ld library path by [@maugustosilva](https://github.com/maugustosilva) in [#1052](https://github.com/llm-d/llm-d/pull/1052)
  * 🐛 Fix kubectl attach race condition in e2e-validate.sh by [@clubanderson](https://github.com/clubanderson) in [#1051](https://github.com/llm-d/llm-d/pull/1051)
  * Fix pd on gke by [@maugustosilva](https://github.com/maugustosilva) in [#1053](https://github.com/llm-d/llm-d/pull/1053)
  * ✨ Add /test-nightly slash command for fork PRs by [@clubanderson](https://github.com/clubanderson) in [#1048](https://github.com/llm-d/llm-d/pull/1048)
  * Bump simulator version to v0.8.2 by [@mayabar](https://github.com/mayabar) in [#1045](https://github.com/llm-d/llm-d/pull/1045)
  * build: add libibverbs-utils to rdma-tools runtime image by [@rajkiranjoshi](https://github.com/rajkiranjoshi) in [#1055](https://github.com/llm-d/llm-d/pull/1055)
  * fix: add missing sizeLimit and fix label typo in inference-scheduling configs by [@wenhug](https://github.com/wenhug) in [#1038](https://github.com/llm-d/llm-d/pull/1038)
  * docs: replace Envoy proxy references with Gateway API by [@tessapham](https://github.com/tessapham) in [#1036](https://github.com/llm-d/llm-d/pull/1036)
  * docs: update KV events topic to ip:port format for scheduler v0.7.0 by [@bongwoobak](https://github.com/bongwoobak) in [#1008](https://github.com/llm-d/llm-d/pull/1008)
  * deps(actions): bump haya14busa/action-update-semver from 1.3.0 to 1.5.1 by [@dependabot](https://github.com/dependabot)[bot] in [#754](https://github.com/llm-d/llm-d/pull/754)
  * deps(actions): bump actions/setup-python from 5 to 6 by [@dependabot](https://github.com/dependabot)[bot] in [#753](https://github.com/llm-d/llm-d/pull/753)
  * docs: assign a level of maturity for each guide by [@maugustosilva](https://github.com/maugustosilva) in [#1058](https://github.com/llm-d/llm-d/pull/1058)
  * update versions for v0.6.0 release by [@diegocastanibm](https://github.com/diegocastanibm) in [#1064](https://github.com/llm-d/llm-d/pull/1064)
  * Add SGLang option to PD disaggregation well-lit path by [@andreyod](https://github.com/andreyod) in [#580](https://github.com/llm-d/llm-d/pull/580)
  * docs: bump inference-scheduler image to v0.7.1 by [@bongwoobak](https://github.com/bongwoobak) in [#1069](https://github.com/llm-d/llm-d/pull/1069)
  * ✨ Add glob patterns and run URL feedback to /test-nightly by [@clubanderson](https://github.com/clubanderson) in [#1065](https://github.com/llm-d/llm-d/pull/1065)
  * ✨ Replace blocking lychee CI with gh-aw nightly checkers by [@clubanderson](https://github.com/clubanderson) in [#1066](https://github.com/llm-d/llm-d/pull/1066)
  * Images Preparation for v0.6.0 by [@diegocastanibm](https://github.com/diegocastanibm) in [#1067](https://github.com/llm-d/llm-d/pull/1067)
  * Last version bump before cutting `v0.6` release by [@maugustosilva](https://github.com/maugustosilva) in htt...


[Read more](https://github.com/llm-d/llm-d/releases/tag/v0.6.0)
### Contributors
clubanderson, petecheslock, and 29 other contributors 
Assets 2
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
4 people reacted
## Release v0.5.1
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
## No results found
[maugustosilva](https://github.com/maugustosilva) released this  05 Mar 17:20 
[ v0.5.1  ](https://github.com/llm-d/llm-d/tree/v0.5.1)
[ `393c5c5`](https://github.com/llm-d/llm-d/commit/393c5c5e6a74cf3be2e11e2aa7263c050a94f10a)
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
## LLM-D Component Summary  
| Component  | Version  | Previous Version  | Type  |  
| --- | --- | --- | --- |  
| llm-d/llm-d-inference-scheduler  | `v0.6.0`  | `v0.5.0`  | Image  |  
| llm-d/llm-d-kv-cache  | `v0.6.0`  | `v0.5.0`  | Library  |  
| llm-d-incubation/llm-d-modelservice  | `v0.4.7`  | `v0.4.5`  | Helm Chart  |  
| llm-d/llm-d-routing-sidecar  | `v0.6.0`  | `v0.5.0`  | Image  |  
| llm-d/llm-d-inference-sim  | `v0.7.1`  | `v0.7.1`  | Image  |  
| llm-d/llm-d-cuda  | `v0.5.1`  | `v0.5.0`  | Image  |  
| llm-d/llm-d-cuda (debug)  | `v0.5.1`  | NA  | Image (New)  |  
| llm-d/llm-d-aws (EFA)  | `v0.5.1`  | Deprecated in v0.5.0  | Image (Re-enabled)  |  
| llm-d/llm-d-xpu  | `v0.5.1`  | `v0.5.0`  | Image  |  
| llm-d/llm-d-cpu  | `v0.5.1`  | `v0.5.0`  | Image  |  
| llm-d/llm-d-rocm  | `v0.5.1`  | NA  | Image (New)  |  
| llm-d/llm-d-hpu  | `v0.5.1`  | NA  | Image (New)  |  
| vllm-project/vllm  | `v0.15.1`  | `v0.14.1`  | Wheel installed in `llm-d`  |  
| llm-d-incubation/llm-d-infra  | `v1.3.6`  | `v1.3.6`  | Helm Chart  |  
| kubernetes-sigs/gateway-api-inference-extension  | `v1.3.1`  | `v1.3.0`  | Helm Chart (Pending upstream release)  |  
| llm-d/llm-d-workload-variant-autoscaler  | `v0.5.1`  | `v0.5.0`  | Helm Chart + Image  |  
## Infrastructure Changes  
| Component  | Version  | Previous Version  |  
| --- | --- | --- |  
| Gateway API  | `v1.3.1`  | `v1.3.0`  |  
| Istio  | `1.28.1`  | `1.28.1`  |  
| KGateway  | `v2.1.1`  | `v2.1.1`  |  
## What's Changed
  * Fix XPU example errors by [@yuanwu2017](https://github.com/yuanwu2017) in [#696](https://github.com/llm-d/llm-d/pull/696)
  * Simplify storage offloading guides into a single one by [@liu-cong](https://github.com/liu-cong) in [#683](https://github.com/llm-d/llm-d/pull/683)
  * feat: remove standalone by [@capri-xiyue](https://github.com/capri-xiyue) in [#688](https://github.com/llm-d/llm-d/pull/688)
  * update permissions for lustre guide setup by [@Sneha-at](https://github.com/Sneha-at) in [#710](https://github.com/llm-d/llm-d/pull/710)
  * Fix: add retries for model discovery by [@yuanwu2017](https://github.com/yuanwu2017) in [#704](https://github.com/llm-d/llm-d/pull/704)
  * [BUGFIX] Comment caching to avoid error by [@diegocastanibm](https://github.com/diegocastanibm) in [#719](https://github.com/llm-d/llm-d/pull/719)
  * Fix lint issues by [@diegocastanibm](https://github.com/diegocastanibm) in [#721](https://github.com/llm-d/llm-d/pull/721)
  * llm-d install doc on openshift 4.20 by [@fgharo](https://github.com/fgharo) in [#538](https://github.com/llm-d/llm-d/pull/538)
  * Enables AMD inference scheduling well-lit path by [@vcave](https://github.com/vcave) in [#642](https://github.com/llm-d/llm-d/pull/642)
  * [build fix] Remove linking compat dir by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#727](https://github.com/llm-d/llm-d/pull/727)
  * just deprecate queue scorrer params by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#730](https://github.com/llm-d/llm-d/pull/730)
  * Add Gaudi inference scheduling CI workflow test by [@yuanwu2017](https://github.com/yuanwu2017) in [#486](https://github.com/llm-d/llm-d/pull/486)
  * fix gke monitoring in helmfile by [@zetxqx](https://github.com/zetxqx) in [#697](https://github.com/llm-d/llm-d/pull/697)
  * add intel HPU for llm-d P/D disaggregation by [@ZhengHongming888](https://github.com/ZhengHongming888) in [#372](https://github.com/llm-d/llm-d/pull/372)
  * Add the XPU CI test by [@yuanwu2017](https://github.com/yuanwu2017) in [#380](https://github.com/llm-d/llm-d/pull/380)
  * feat: add feature request issue template by [@thillai-c](https://github.com/thillai-c) in [#733](https://github.com/llm-d/llm-d/pull/733)
  * Add the format hint for CI by [@yuanwu2017](https://github.com/yuanwu2017) in [#717](https://github.com/llm-d/llm-d/pull/717)
  * feat: add support to build for CPU on AMX by [@zdtsw](https://github.com/zdtsw) in [#725](https://github.com/llm-d/llm-d/pull/725)
  * fix: add missing check-buildah target in Makefile by [@GuilinDev](https://github.com/GuilinDev) in [#735](https://github.com/llm-d/llm-d/pull/735)
  * docs: fix typos and spelling errors across docs, guides, and Dockerfile by [@thillai-c](https://github.com/thillai-c) in [#731](https://github.com/llm-d/llm-d/pull/731)
  * docs: update container image link in PROJECT.md by [@petecheslock](https://github.com/petecheslock) in [#738](https://github.com/llm-d/llm-d/pull/738)
  * Add debug variant support for llm-d images across CUDA by [@kapiljain1989](https://github.com/kapiljain1989) in [#734](https://github.com/llm-d/llm-d/pull/734)
  * 🌱 Add typos config and Dependabot by [@clubanderson](https://github.com/clubanderson) in [#746](https://github.com/llm-d/llm-d/pull/746)
  * chore: update bug report issue template with current versions by [@thillai-c](https://github.com/thillai-c) in [#732](https://github.com/llm-d/llm-d/pull/732)
  * ✨ Add nightly E2E OpenShift workflows for 5 guides by [@clubanderson](https://github.com/clubanderson) in [#747](https://github.com/llm-d/llm-d/pull/747)
  * 🌱 Standardize governance workflows via llm-d-infra by [@clubanderson](https://github.com/clubanderson) in [#744](https://github.com/llm-d/llm-d/pull/744)
  * 🌱 Remove dependabot configuration by [@clubanderson](https://github.com/clubanderson) in [#756](https://github.com/llm-d/llm-d/pull/756)
  * linting links by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#757](https://github.com/llm-d/llm-d/pull/757)
  * 🐛 Fix broken reusable workflow references by [@clubanderson](https://github.com/clubanderson) in [#762](https://github.com/llm-d/llm-d/pull/762)
  * 🐛 Bump nightly pod_wait_timeout from 15m to 30m by [@clubanderson](https://github.com/clubanderson) in [#760](https://github.com/llm-d/llm-d/pull/760)
  * 🐛 Fix PD disaggregation nightly decode tensor parallelism by [@clubanderson](https://github.com/clubanderson) in [#764](https://github.com/llm-d/llm-d/pull/764)
  * 🐛 Fix tiered-prefix-cache CrashLoopBackOff: num_cpu_blocks → cpu_bytes_to_use by [@clubanderson](https://github.com/clubanderson) in [#768](https://github.com/llm-d/llm-d/pull/768)
  * ⚠️ Revert: Fix tiered-prefix-cache CrashLoopBackOff ([#768](https://github.com/llm-d/llm-d/pull/768)) by [@clubanderson](https://github.com/clubanderson) in [#769](https://github.com/llm-d/llm-d/pull/769)
  * 🐛 Fix tiered-prefix-cache CrashLoopBackOff: num_cpu_blocks → cpu_bytes_to_use by [@clubanderson](https://github.com/clubanderson) in [#770](https://github.com/llm-d/llm-d/pull/770)
  * adding required packages for LMCache runtime by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#701](https://github.com/llm-d/llm-d/pull/701)
  * ✨ Add upstream dependency monitor, dependabot, and remove legacy CI callers by [@clubanderson](https://github.com/clubanderson) in [#771](https://github.com/llm-d/llm-d/pull/771)
  * Set accelerator_type to H100 and rename OCP nightly workflows by [@diegocastanibm](https://github.com/diegocastanibm) in [#777](https://github.com/llm-d/llm-d/pull/777)
  * ✨ Add CKS nightly E2E workflows for IS and PD by [@clubanderson](https://github.com/clubanderson) in [#780](https://github.com/llm-d/llm-d/pull/780)
  * ressurect efa by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#741](https://github.com/llm-d/llm-d/pull/741)
  * ✨ Add wide-ep and benchmark CKS nightly callers by [@clubanderson](https://github.com/clubanderson) in [#782](https://github.com/llm-d/llm-d/pull/782)
  * deps(actions): bump docker/build-push-action from 5 to 6 by [@dependabot](https://github.com/dependabot)[bot] in [#755](https://github.com/llm-d/llm-d/pull/755)
  * deps(actions): bump actions/upload-artifact from 4 to 6 by [@dependabot](https://github.com/dependabot)[bot] in [#752](https://github.com/llm-d/llm-d/pull/752)
  * ✨ Fix dev image latest tag + nightly build + image_override for all E2E nightlies by [@clubanderson](https://github.com/clubanderson) in [#783](https://github.com/llm-d/llm-d/pull/783)
  * deps(actions): bump actions/checkout from 4 to 6 by [@dependabot](https://github.com/dependabot)[bot] in [#751](https://github.com/llm-d/llm-d/pull/751)
  * Update interactive-pod to use latest guidellm by [@natoscott](https://github.com/natoscott) in [#716](https://github.com/llm-d/llm-d/pull/716)
  * 🌱 OCP H100 nodeSelector + CKS GPU preemption for nightly E2E by [@clubanderson](https://github.com/clubanderson) in [#788](https://github.com/llm-d/llm-d/pull/788)
  * 🐛 Fix typo in CKS benchmark caller workflow dispatch by [@clubanderson](https://github.com/clubanderson) in [#790](https://github.com/llm-d/llm-d/pull/790)
  * 🐛 Build WVA image from main + prevent fork schedule runs by [@clubanderson](https://github.com/clubanderson) in [#794](https://github.com/llm-d/llm-d/pull/794)
  * 🐛 Fix CKS WVA test target: use test-e2e-openshift (no kind dependency) by [@clubanderson](https://github.com/clubanderson) in [#797](https://github.com/llm-d/llm-d/pull/797)
  * 🐛 Build WVA controller image from main for CKS nightly by [@clubanderson](https://github.com/clubanderson) in [#798](https://github.com/llm-d/llm-d/pull/798)
  * 🐛 Add docker build retry for WVA nightly E2E workflows by [@clubanderson](https://github.com/clubanderson) in [#801](https://github.com/llm-d/llm-d/pull/801)
  * ✨ Enable GPU preemption on all nightly E2E workflows by [@clubanderson](https://github.com/clubanderson) in [#810](https://github.com/llm-d/llm-d/pull/810)
  * bump to nixl 0.10.0 and vllm v0.15.1 by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#713](https://github.com/llm-d/llm-d/pull/713)
  * Replace RHEL packages with static CentOS Stream 9 RPMs by [@dagrayvid](https://github.com/dagrayvid) in [#669](https://github.com/llm-d/llm-d/pull/669)
  * sa was missing, copy it in by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#813](https://github.com/llm-d/llm-d/pull/813)
  * ocp nightly wide-ep sa was missing, copy it in by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#814](https://github.com/llm-d/llm-d/pull/814)
  * Fix typos tracked in _typos.toml by [@lisperz](https://github.com/lisperz) in [#786](https://github.com/llm-d/llm-d/pull/786)
  * feat: add support to use kv-cache UDS tokenizer sidecar in guide by [@zdtsw](https://github.com/zdtsw) in [#740](https://github.com/llm-d/llm-d/pull/740)
  * Add RDMA tools container image for RoCE/InfiniBand validation by [@dagrayvid](https://github.com/dagrayvid) in [#626](https://github.com/llm-d/llm-d/pull/626)
  * docs: update README for v0.5 release by [@chcost](https://github.com/chcost) in [#822](https://github.com/llm-d/llm-d/pull/822)
  * Proposal: Prism - Performance analysis for distributed inference systems by [@seanhorgan](https://github.com/seanhorgan) in [#796](https://github.com/llm-d/llm-d/pull/796)
  * 🐛 Fix uds-tokenizer image tag in PPC guide (v0.5.0-rc1 → v0.5.1-rc1) by [@clubanderson](https://github.com/clubanderson) in [#829](https://github.com/llm-d/llm-d/pull/829)
  * Add gke rilb gateway class by [@liu-cong](https://github.com/liu-cong) in [#815](https://github.com/llm-d/llm-d/pull/815)
  * Release doc by [@diegocastanibm](https://github.com/diegocastanibm) in [#819](https://github.com/llm-d/llm-d/pull/819)
  * Add gateway provider prereq to the tiered prefix cache guides by [@liu-cong](https://github.com/liu-cong) in [#761](https://github.com/llm-d/llm-d/pull/761)
  * [BUG] sourcing docker/vllm-version in ci-release by [@diegocastanibm](https://github.com/diegocastanibm) in [#823](https://github.com/llm-d/llm-d/pull/823)
  * ⚠️ Skip gateway provider install on OCP nightly E2Es by [@clubanderson](https://github.com/clubanderson) in [#833](https://github.com/llm-d/llm-d/pull/833)
  * Add a MAINTAINERS file for current and future maintainers list by [@petecheslock](https://github.com/petecheslock) in [#817](https://github.com/llm-d/llm-d/pull/817)
  * update: bump llm-d-modelservice chart to 0.4.6 by [@zdtsw](https://github.com/zdtsw) in [#644](https://github.com/llm-d/llm-d/pull/644)
  * Add sig-rl structure proposal by [@smarterclayton](https://github.com/smarterclayton) in [#840](https://github.com/llm-d/llm-d/pull/840)
  * Update benchmark templates to 0.5 by [@dmitripikus](https://github.com/dmitripikus) in [#816](https://github.com/llm-d/llm-d/pull/816)
  * Revert "Update benchmark templates to 0.5" by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#841](https://github.com/llm-d/llm-d/pull/841)
  * Add ADOPTERS.md to document project contributors and supporters by [@petecheslock](https://github.com/petecheslock) in [#830](https://github.com/llm-d/llm-d/pull/830)
  * [ADOPTERS.md] Add Tesla as a user of llm-d by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#845](https://github.com/llm-d/llm-d/pull/845)
  * Change the model name and workflow name by [@yuanwu2017](https://github.com/yuanwu2017) in [#742](https://github.com/llm-d/llm-d/pull/742)
  * 🐛 fix: increase startup probe and pod wait timeouts for IS nightly E2E by [@clubanderson](https://github.com/clubanderson) in [#852](https://github.com/llm-d/llm-d/pull/852)
  * [Benchmark Guide] [WVA + IS] Document and Provide a WVA + IS Benchmark Guide by [@Vezio](https://github.com/Vezio) in [#820](https://github.com/llm-d/llm-d/pull/820)
  * Update benchmark templates to 0.5 ([#816](https://github.com/llm-d/llm-d/pull/816)) by ...


[Read more](https://github.com/llm-d/llm-d/releases/tag/v0.5.1)
### Contributors
clubanderson, seanhorgan, and 30 other contributors 
Assets 2
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
2 people reacted
## Release v0.4.0
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
## No results found
[Gregory-Pereira](https://github.com/Gregory-Pereira) released this  26 Nov 20:19 
[ v0.4.0  ](https://github.com/llm-d/llm-d/tree/v0.4.0)
[ `9247440`](https://github.com/llm-d/llm-d/commit/9247440471cbcd94020ed800b3075eddb03786cf)
This commit was signed with the committer’s **verified signature**. 
[Gregory-Pereira](https://github.com/Gregory-Pereira) Greg Pereira 
GPG key ID: CCBE624D8FF6D016
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
# 📦 llm-d v0.4.0 Release Notes
This release of the `llm-d` repo will capture the release for the entirety of the project, guides, components, and all.
**Release Date:** 2025-11-26
## 🧩 Component Summary  
| Component  | Version  | Previous Version  | Type  |  
| --- | --- | --- | --- |  
| llmd/llm-d-inference-scheduler  | `v0.4.0-rc.1`  | `v0.3.1`  | Image  |  
| llm-d-incubation/llm-d-modelservice  | `v0.3.8`  | `v0.2.10`  | Helm Chart  |  
| llm-d/llm-d-routing-sidecar  | `v0.4.0-rc.1`  | `v0.3.1`  | Image  |  
| llm-d/llm-d-cuda  | `v0.4.0`  | `v0.3.1`  | Image  |  
| llm-d/llm-d-aws  | `v0.4.0`  | `v0.3.1`  | Image  |  
| llm-d/llm-d-xpu  | `v0.4.0`  | `v0.3.1`  | Image  |  
| llm-d/llm-d-cpu  | `v0.4.0`  | `v0.3.1`  | Image (New)  |  
| llm-d-incubation/llm-d-infra  | `v1.3.4`  | `v1.3.3`  | Helm Chart  |  
| kubernetes-sig/gateway-api-inference-extension  | `v1.2.0-rc.1`  | `v1.0.1`  | Helm Chart  |  
| llm-d/llm-d-workload-variant-autoscaler  | `v0.0.8`  | NA (new)  | Helm Chart + Image  |  
## 🔹 lmd/llm-d-inference-scheduler
  * **Description** : This scheduler that makes optimized routing decisions for inference requests to the llm-d inference framework.
  * **Diff** : [v0.3.1 → v0.4.0-rc.1](https://github.com/llm-d/llm-d-inference-scheduler/compare/v0.3.1...v0.4.0-rc.1)


## 🔹 llm-d-incubation/llm-d-modelservice
  * **Description** : `modelservice` is a Helm chart that simplifies LLM deployment on llm-d by declaratively managing Kubernetes resources for serving base models. It enables reproducible, scalable, and tunable model deployments through modular presets, and clean integration with llm-d ecosystem components (including vLLM, Gateway API Inference Extension, LeaderWorkerSet).
  * **Diff** : [v0.2.10 → v0.3.8](https://github.com/llm-d-incubation/llm-d-modelservice/compare/llm-d-modelservice-v0.2.10...llm-d-modelservice-v0.3.8)


## 🔹 llm-d/llm-d-routing-sidecar
  * **Description** : A reverse proxy redirecting incoming requests to the prefill worker specified in the x-prefiller-host-port HTTP request header.
  * **Diff** : [v0.3.1 → v0.4.0-rc.1](https://github.com/llm-d/llm-d-routing-sidecar/compare/v0.3.1...v0.4.0-rc.1)


## 🔹 llm-d/llm-d
  * **Description** : A midstreamed image of `vllm-project/vllm` for inferencing, supporting features such as PD disaggregation, KV cache awareness and more.
  * **Diff** : [v0.3.1 → v0.4.0](https://github.com/llm-d/llm-d/compare/v0.3.1...v0.4.0)
  * **Image Variants** : Different image variants of this component: 
    * XPU: `ghcr.io/llm-d/llm-d-xpu:v0.4.0`
    * AWS: `ghcr.io/llm-d/llm-d-aws:v0.4.0`
    * CUDA: `ghcr.io/llm-d/llm-d-cuda:v0.4.0`
    * CPU: `ghcr.io/llm-d/llm-d-cpu:v0.4.0`


## 🔹 llm-d-incubation/llm-d-infra
  * **Description** : A helm chart for deploying gateway and gateway related infrastructure assets for llm-d.
  * **Diff** : [v1.3.3 → v1.3.4](https://github.com/llm-d-incubation/llm-d-infra/compare/v1.3.3...v1.3.4)


## 🔹 kubernetes-sig/gateway-api-inference-extension
  * **Description** : A Helm chart to deploy an InferencePool, a corresponding EndpointPicker (epp) deployment, and any other related assets.
  * **Diff** : [v1.0.1 → v1.2.0-rc.1](https://github.com/kubernetes-sigs/gateway-api-inference-extension/compare/v1.0.1...v1.2.0-rc.1)


## 🔹 llm-d/llm-d-workload-variant-autoscaler (New - Experimental)
  * **Description** : Variant optimization autoscaler for distributed inference workloads
  * **History** (new): [v0.0.5](https://github.com/llm-d-incubation/workload-variant-autoscaler/tree/v0.0.5)
  * **Note** : This is an experimental component being included in this release for early testing and feedback.


For more information on any of the component project or versions, please checkout their repos directly. For information on installing and using the new release refer to our [guides](https://github.com/llm-d/llm-d/blob/v0.4.0/guides). Thank you to all contributors who helped make this happen. Automated release notes will be included below, but it should be noted this only tracks work in the main repo, and does not fully reflect a changelog across the project
## What's Changed
  * Add umbrella kv cache offloading well-lit path folder structure by [@liu-cong](https://github.com/liu-cong) in [#401](https://github.com/llm-d/llm-d/pull/401)
  * Correct wide-ep resource requirements. by [@liu-cong](https://github.com/liu-cong) in [#373](https://github.com/llm-d/llm-d/pull/373)
  * add information about component testing by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#361](https://github.com/llm-d/llm-d/pull/361)
  * doc(guides): Introduce standardized recipes for Gateway, InferencePool, and vLLM by [@zetxqx](https://github.com/zetxqx) in [#444](https://github.com/llm-d/llm-d/pull/444)
  * Fix a broken link in the cpu prefix cache readme by [@smarterclayton](https://github.com/smarterclayton) in [#451](https://github.com/llm-d/llm-d/pull/451)
  * Add more GKE specific workarounds and known issues by [@smarterclayton](https://github.com/smarterclayton) in [#419](https://github.com/llm-d/llm-d/pull/419)
  * Update SIGs documentation to remove outdated schedule details. by [@petecheslock](https://github.com/petecheslock) in [#431](https://github.com/llm-d/llm-d/pull/431)
  * Update links to deploying vLLM multi-host in stable docs by [@smarterclayton](https://github.com/smarterclayton) in [#436](https://github.com/llm-d/llm-d/pull/436)
  * fix kutomization error and model flag error in cpu offloading. by [@zetxqx](https://github.com/zetxqx) in [#453](https://github.com/llm-d/llm-d/pull/453)
  * Add GKE B200 readme notes by [@smarterclayton](https://github.com/smarterclayton) in [#454](https://github.com/llm-d/llm-d/pull/454)
  * doc: enrich the prefix-cache-storage vllm cpu native offloading with benchmark results by [@zetxqx](https://github.com/zetxqx) in [#438](https://github.com/llm-d/llm-d/pull/438)
  * Add CPU for llm-d Inference Scheduling by [@ZhengHongming888](https://github.com/ZhengHongming888) in [#428](https://github.com/llm-d/llm-d/pull/428)
  * Add cpu offloading example for GKE + LMCache by [@dannawang0221](https://github.com/dannawang0221) in [#318](https://github.com/llm-d/llm-d/pull/318)
  * Add tab format for better UX on the website by [@liu-cong](https://github.com/liu-cong) in [#452](https://github.com/llm-d/llm-d/pull/452)
  * Rename `prefix-cache-storage` to `tiered-prefix-cache` by [@vMaroon](https://github.com/vMaroon) in [#468](https://github.com/llm-d/llm-d/pull/468)
  * Remove the dockerfile.gke as it is no longer used by [@smarterclayton](https://github.com/smarterclayton) in [#462](https://github.com/llm-d/llm-d/pull/462)
  * Token credentials fix + vLLM v0.11.1 by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#456](https://github.com/llm-d/llm-d/pull/456)
  * guides: Make vLLM log more useful in inference-scheduling by [@russellb](https://github.com/russellb) in [#439](https://github.com/llm-d/llm-d/pull/439)
  * Inference scheduling support for Intel Gaudi accelerator by [@poussa](https://github.com/poussa) in [#374](https://github.com/llm-d/llm-d/pull/374)
  * Add JIT directories and model directories by [@smarterclayton](https://github.com/smarterclayton) in [#418](https://github.com/llm-d/llm-d/pull/418)
  * Use markdown comments for Tabs support on docusaurus by [@petecheslock](https://github.com/petecheslock) in [#474](https://github.com/llm-d/llm-d/pull/474)
  * Highlight P/D benefits with throughput-interactivity tradeoff by [@liu-cong](https://github.com/liu-cong) in [#472](https://github.com/llm-d/llm-d/pull/472)
  * add benchmark results lmcache results and tuned epp scorers by [@zetxqx](https://github.com/zetxqx) in [#457](https://github.com/llm-d/llm-d/pull/457)
  * Add step by step guide for setting up p/d with TPU on GKE by [@yangligt2](https://github.com/yangligt2) in [#443](https://github.com/llm-d/llm-d/pull/443)
  * refactor: restructure vllm recipe with base and overlay pattern by [@diego-torres](https://github.com/diego-torres) in [#475](https://github.com/llm-d/llm-d/pull/475)
  * [Build] Add FI JIT Cache to Image by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#482](https://github.com/llm-d/llm-d/pull/482)
  * Add instructions to clone git repo and checkout the release by [@liu-cong](https://github.com/liu-cong) in [#477](https://github.com/llm-d/llm-d/pull/477)
  * Create CPU dockefile for PD and Inference Scheduling by [@ZhengHongming888](https://github.com/ZhengHongming888) in [#465](https://github.com/llm-d/llm-d/pull/465)
  * guides/prereq/client-setup/install-deps.sh - increment HELMFILE_VERSION to 1.2.1 by [@herbertkb](https://github.com/herbertkb) in [#492](https://github.com/llm-d/llm-d/pull/492)
  * docs: Addresses CPU support added in PR [#428](https://github.com/llm-d/llm-d/pull/428) by [@aneeshkp](https://github.com/aneeshkp) in [#466](https://github.com/llm-d/llm-d/pull/466)
  * Infra, MS and GAIE bumps + istio change compat by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#459](https://github.com/llm-d/llm-d/pull/459)
  * Update release version for cpu offloading guide by [@liu-cong](https://github.com/liu-cong) in [#495](https://github.com/llm-d/llm-d/pull/495)
  * enable TLS in monitoring for prom by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#496](https://github.com/llm-d/llm-d/pull/496)
  * helmfile and supporting artifacts for wva by [@clubanderson](https://github.com/clubanderson) in [#464](https://github.com/llm-d/llm-d/pull/464)
  * updating LMCACHe to be non fork by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#501](https://github.com/llm-d/llm-d/pull/501)
  * component bumps for WVA guide by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#502](https://github.com/llm-d/llm-d/pull/502)
  * Build vLLM 0.11.2 + patches for 0.4 by [@smarterclayton](https://github.com/smarterclayton) in [#461](https://github.com/llm-d/llm-d/pull/461)
  * Avoid defining LMCACHE_COMMIT_SHA in multiple places by [@terrytangyuan](https://github.com/terrytangyuan) in [#503](https://github.com/llm-d/llm-d/pull/503)
  * WVA guide integration targeting v0.4 by [@mamy-CS](https://github.com/mamy-CS) in [#470](https://github.com/llm-d/llm-d/pull/470)
  * fixing AWS image by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#506](https://github.com/llm-d/llm-d/pull/506)
  * remove pre-passing values for VLLM by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#507](https://github.com/llm-d/llm-d/pull/507)


## New Contributors
  * [@zetxqx](https://github.com/zetxqx) made their first contribution in [#444](https://github.com/llm-d/llm-d/pull/444)
  * [@ZhengHongming888](https://github.com/ZhengHongming888) made their first contribution in [#428](https://github.com/llm-d/llm-d/pull/428)
  * [@dannawang0221](https://github.com/dannawang0221) made their first contribution in [#318](https://github.com/llm-d/llm-d/pull/318)
  * [@russellb](https://github.com/russellb) made their first contribution in [#439](https://github.com/llm-d/llm-d/pull/439)
  * [@poussa](https://github.com/poussa) made their first contribution in [#374](https://github.com/llm-d/llm-d/pull/374)
  * [@yangligt2](https://github.com/yangligt2) made their first contribution in [#443](https://github.com/llm-d/llm-d/pull/443)
  * [@diego-torres](https://github.com/diego-torres) made their first contribution in [#475](https://github.com/llm-d/llm-d/pull/475)
  * [@herbertkb](https://github.com/herbertkb) made their first contribution in [#492](https://github.com/llm-d/llm-d/pull/492)
  * [@aneeshkp](https://github.com/aneeshkp) made their first contribution in [#466](https://github.com/llm-d/llm-d/pull/466)
  * [@mamy-CS](https://github.com/mamy-CS) made their first contribution in [#470](https://github.com/llm-d/llm-d/pull/470)


**Full Changelog** : [`v0.3.1...v0.4.0`](https://github.com/llm-d/llm-d/compare/v0.3.1...v0.4.0)
### Contributors
russellb, clubanderson, and 16 other contributors 
Assets 2
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
1 person reacted
## Release v0.5.0
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
## No results found
[Gregory-Pereira](https://github.com/Gregory-Pereira) released this  04 Feb 06:09 
[ v0.5.0  ](https://github.com/llm-d/llm-d/tree/v0.5.0)
[ `4ec87d8`](https://github.com/llm-d/llm-d/commit/4ec87d8e005c36f088fb4b726cb6f882536fccce)
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
# 📦 llm-d v0.5.0 Release Notes
This release of the `llm-d` repo will capture the release for the entirety of the project, guides, and components.
**Release Date:** 2026-02-03
## Core Objectives
  * Reproducible benchmarking
  * Scaling KV-cache
  * Autoscaling improvements
  * Greater Metrics + tracing story


## 🏗️ Infrastructure Changes - BREAKING CHANGES  
| Component  | Version  | Previous Version  |  
| --- | --- | --- |  
| Gateway API  | `v1.4.0`  | `v1.3.0`  |  
| Istio  | `1.28.1`  | `1.28-alpha.89f30b26ba71bf5e538083a4720d0bc2d8c06401`  |  
| KGateway  | `v2.1.1`  | `v2.0.3`  |  
| GKE Gateways  | NA - tied to GKE  | NA - tied to GKE  |  
**NOTE:** We upgraded to the following versions to consume the new v1 Inference Pool API. This means cluster admins should descale their workloads and upgrade these Infrastructure level components before proceeding. You should be able to use the old `inference.networking.x-k8s.io/v1alpha2` API, however the guides will require changes to make them work - use this at your own risk.
## 🧩 LLM-D Component Summary  
| Component  | Version  | Previous Version  | Type  |  
| --- | --- | --- | --- |  
| llm-d/llm-d-inference-scheduler  | `v0.5.0`  | `v0.4.0-rc1`  | Image  |  
| llm-d/llm-d-kv-cache  | `v0.5.0`  | `v0.4.0`  | Library  |  
| llm-d-incubation/llm-d-modelservice  | `v0.4.5`  | `v0.3.8`  | Helm Chart  |  
| llm-d/llm-d-routing-sidecar  | `v0.5.0`  | `v0.4.0-rc1`  | Image  |  
| llm-d/llm-d-inference-sim  | `v0.7.1`  | `v0.6.1`  | Image  |  
| llm-d/llm-d-cuda  | `v0.5.0`  | `v0.4.0`  | Image  |  
| llm-d/llm-d-aws  | Deprecated  | `v0.4.0`  | Image  |  
| llm-d/llm-d-xpu  | `v0.5.0`  | `v0.4.0`  | Image  |  
| llm-d/llm-d-cpu  | `v0.5.0`  | `v0.4.0`  | Image (New)  |  
| vllm-project/vllm  | `v0.14.1`  |  `v0.11.2` + additional cherry-picks (built from fork)  | Wheel installed in `llm-d`  |  
| llm-d-incubation/llm-d-infra  | `v1.3.6`  | `v1.3.4`  | Helm Chart  |  
| kubernetes-sig/gateway-api-inference-extension  | `v1.3.0`  | `v1.2.0-rc1`  | Helm Chart  |  
| llm-d/llm-d-workload-variant-autoscaler  | `v0.5.0`  | `v0.0.8`  | Helm Chart + Image  |  
### Deprecations and changes
**Temporary Deprecation** : EFA
We ran into a bug 2 days before release, between EFA and and our regular path. This was that the base image install version of libiverbs / rdma core is different than the version being packaged by EFA. Having both these versions causes nvshmem initialization errors for anything over RDMA. As a temporary measure we have dissabled EFA while we figure out a proper way to deine who owns the core RDMA core user space packages.
**WVA promotion** :
Our [Workload-Variant-Autoscaler](https://github.com/llm-d-incubation/workload-variant-autoscaler) has graduated from a experimental to core component of llm-d! Congratulations to the SIG team!
### Migrations
The `llm-d/llm-d-routing-sidecar` image has been moved under the `llm-d/llm-d-inference-scheduler` repo, and its previous one **archieved**.
### 🧩 CUDA Image Specific Component Summary  
| Component  | Version  | Previous Version  |  
| --- | --- | --- |  
| LMCache  | `v0.3.13`  | `v0.3.8`  |  
| UCX  | `v1.20.0`  | `v1.19.0`  |  
| NVSHMEM  |  `v3.4.5-0` (git)  |  `v3.3.20` (developer source distribution)  |  
| NIXL  | `v0.9.0`  | `v0.6.0`  |  
| GDR Copy  | `v2.5.1`  | Commit `0f7366e` (maps to v2.5.1, no change)  |  
| Infinistore  | `v0.2.33`  | NA (New)  |  
| EFA installer (Temporary deprecation)  | `v1.46.0`  | `v1.43.3`  |  
| Flashinfer  | `v0.5.3`  | `v0.5.2`  |  
| DEEPGEMM  | `v0.5.3`  | `v0.5.2`  |  
| DEEPEP  | `v0.5.3`  | `v0.5.2`  |  
| PPLX  | `v0.5.3`  | `v0.5.2`  |  
### RC Images in v0.4.0 release 🤦🏼
Due to some unfortunate circumstances last release, we shipped the official release with many code compelete RC images. We plan to have better release hygeine going forward.
## Meta changes
  * Change the default NIXL port to 5600 to be consistent with vLLM v0.11.1 +
  * Adopt the new v1 inferencepool API
  * Remove routing sidecar from non-pd deployments as its not necesary 
    * in those cases adjust decode port 8200 back to default 8000 port as it no longer needs to be proxied
  * More accurate labels for model and accelerator type on inference servers
  * And many more! Please see each component's diff / changelog for a more complete understanding of the changes across the project.


## 🔹 llm-d/llm-d-inference-scheduler
  * **Description** : The scheduler that makes optimized routing decisions for inference requests to the llm-d inference framework.
  * **Diff** : [v0.4.0-rc1 → v0.5.0](https://github.com/llm-d/llm-d-inference-scheduler/compare/v0.4.0-rc1...v0.5.0)


## 🔹 llm-d/llm-d-kv-cache
  * **Description** : The libraries for tokenization, KV-events processing, and KV-cache indexing and offloading.
  * **Diff** : [v0.4.0 → v0.5.0](https://github.com/llm-d/llm-d-kv-cache/compare/v0.4.0...v0.5.0)


## 🔹 llm-d-incubation/llm-d-modelservice
  * **Description** : `modelservice` is a Helm chart that simplifies LLM deployment on llm-d by declaratively managing Kubernetes resources for serving base models. It enables reproducible, scalable, and tunable model deployments through modular presets, and clean integration with llm-d ecosystem components (including vLLM, Gateway API Inference Extension, LeaderWorkerSet).
  * **Diff** : [v0.3.8 → v0.4.5](https://github.com/llm-d-incubation/llm-d-modelservice/compare/llm-d-modelservice-v0.3.8...llm-d-modelservice-v0.4.5)


## 🔹 llm-d/llm-d-routing-sidecar
  * **Description** : A reverse proxy redirecting incoming requests to the prefill worker specified in the x-prefiller-host-port HTTP request header.
  * **Diff** : [v0.4.0-rc.1 → v0.5.0] 
    * Note, no compare link can be provided because **this component was moved under the inference-scheduler repo**.


## 🔹 llm-d/llm-d-inference-sim
  * **Description** : A light-weight inference simulator.
  * **Diff** : [v0.6.1 → v0.7.1](https://github.com/llm-d/llm-d-inference-sim/compare/v0.6.0...v0.7.1)


## 🔹 llm-d/llm-d
Note: in the `v0.4.0` release the guides were not as updated as we would have liked. Because of this we built a v0.4.0 image but we used the v0.3.1 image in the guides.
  * **Description** : A midstreamed image of `vllm-project/vllm` for inferencing, supporting features such as PD disaggregation, KV cache awareness and more.
  * **Diff** : [v0.4.0 → v0.5.0](https://github.com/llm-d/llm-d/compare/v0.4.0...v0.5.0)
  * **Image Variants** : Different image variants of this component: 
    * XPU: `ghcr.io/llm-d/llm-d-xpu:v0.5.0`
    * AWS: Deprecated
    * CUDA: `ghcr.io/llm-d/llm-d-cuda:v0.5.0`
    * CPU: `ghcr.io/llm-d/llm-d-cpu:v0.5.0`


## 🔹 vllm-project/vllm
Note: last release we built our inferencing images off of the [nerual magic fork of vLLM](https://github.com/neuralmagic/vllm/tree/llm-d-release-0.4). This was built off of vLLM `v0.11.2` with additional cherry-picks for feature work that did not make it in on time.
As vLLM has an extreemly high contribution velocity, rather than listing a Diff it makes more sense to refer to the release notes for the releases that happened in between directly:


## 🔹 llm-d-incubation/llm-d-infra
  * **Description** : A helm chart for deploying gateway and gateway related infrastructure assets for llm-d.
  * **Diff** : [v1.3.4 → v1.3.6](https://github.com/llm-d-incubation/llm-d-infra/compare/v1.3.4...v1.3.6)


## 🔹 kubernetes-sig/gateway-api-inference-extension
  * **Description** : A Helm chart to deploy an InferencePool, a corresponding EndpointPicker (epp) deployment, and any other related assets.
  * **Diff** : [v1.2.0-rc1 → v1.3.0](https://github.com/kubernetes-sigs/gateway-api-inference-extension/compare/v1.2.0-rc1...v1.3.0)


## 🔹 llm-d/llm-d-workload-variant-autoscaler (New - Experimental)
  * **Description** : [TODO: Add description of the workload variant autoscaler]
  * **Diff** : [v0.0.8 → v0.5.0](https://github.com/llm-d-incubation/workload-variant-autoscaler/compare/v0.0.8...v0.5.0)
  * **NOTE** : This component was previously experimental, but it has graduated to a core component of LLM-D. We hoped to move it from the `llm-d-incubation` org to the `llm-d` main org for this release but did not have time - stay tuned for this in the next release.


For more information on any of the component project or versions, please checkout their repos directly. For information on installing and using the new release refer to our [guides](https://github.com/llm-d/llm-d/blob/v0.5.0/guides). Thank you to all contributors who helped make this happen.
## In repo change-log - inference image and guides only
  * docs: added standalone EPP without gateway api guide by [@capri-xiyue](https://github.com/capri-xiyue) in [#508](https://github.com/llm-d/llm-d/pull/508)
  * refactor wideep to use recipes folder. by [@zetxqx](https://github.com/zetxqx) in [#455](https://github.com/llm-d/llm-d/pull/455)
  * Fix dead link by [@yuanwu2017](https://github.com/yuanwu2017) in [#510](https://github.com/llm-d/llm-d/pull/510)
  * Add tiered prefix cache to the top level guide by [@liu-cong](https://github.com/liu-cong) in [#516](https://github.com/llm-d/llm-d/pull/516)
  * Update the correct vllm kv cache utilization metric name in inference scheduler by [@liu-cong](https://github.com/liu-cong) in [#509](https://github.com/llm-d/llm-d/pull/509)
  * fix: EPP configuration issues in precise-prefix-cache-aware guide by [@yuanwu2017](https://github.com/yuanwu2017) in [#512](https://github.com/llm-d/llm-d/pull/512)
  * Add tiered prefix cache to GKE doc by [@liu-cong](https://github.com/liu-cong) in [#523](https://github.com/llm-d/llm-d/pull/523)
  * Update TPU and XPU supported well-lit paths by [@liu-cong](https://github.com/liu-cong) in [#524](https://github.com/llm-d/llm-d/pull/524)
  * Link to the 0.4 release blog by [@smarterclayton](https://github.com/smarterclayton) in [#525](https://github.com/llm-d/llm-d/pull/525)
  * Update wide-ep example to 0.4.0 image ...


[Read more](https://github.com/llm-d/llm-d/releases/tag/v0.5.0)
### Contributors
russellb, clubanderson, and 36 other contributors 
Assets 2
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
2 people reacted
## v0.3.1 Release
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
## No results found
[Gregory-Pereira](https://github.com/Gregory-Pereira) released this  06 Nov 15:28 
[ v0.3.1  ](https://github.com/llm-d/llm-d/tree/v0.3.1)
[ `267293b`](https://github.com/llm-d/llm-d/commit/267293be07080273b2444e08c7a96ea34aa5bfa9)
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
# Release overview
This release was focused on following up on our objectives from the v0.3.0 that could not make it into that release. A few key stories to highlight:
  * ARM support
  * Refactor image build process to scripts
  * Unifying the GKE image into our core Cuda image
  * Adding AKS cloud provider support


Welcome to all our new contributors, and thanks to the team for their hard work.
## Component version bumps:
  * Inference SIM ([`v0.5.1` --> `v0.6.1`](https://github.com/llm-d/llm-d-inference-sim/compare/v0.5.1...v0.6.1))
  * llm-d image (`v0.3.0` --> `v0.3.1`, diff encapsulated in change-log below)


## What's Changed
  * [bugfix] changing filename to not reference old plugin by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#353](https://github.com/llm-d/llm-d/pull/353)
  * Deprecate all InferenceModel in XPU guides by [@yankay](https://github.com/yankay) in [#358](https://github.com/llm-d/llm-d/pull/358)
  * version bump on vllm release v0.11.0 tag move by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#365](https://github.com/llm-d/llm-d/pull/365)
  * Update SIGS.md owners by [@petecheslock](https://github.com/petecheslock) in [#363](https://github.com/llm-d/llm-d/pull/363)
  * Clear /dev/shm before process startup to prevent crashloops by [@smarterclayton](https://github.com/smarterclayton) in [#364](https://github.com/llm-d/llm-d/pull/364)
  * Add hardware and platform support issue template by [@Ayobami-00](https://github.com/Ayobami-00) in [#359](https://github.com/llm-d/llm-d/pull/359)
  * minor typo in the inference guide by [@effi-ofer](https://github.com/effi-ofer) in [#377](https://github.com/llm-d/llm-d/pull/377)
  * docs: introduce AKS as a well-lit infra provider by [@chewong](https://github.com/chewong) in [#335](https://github.com/llm-d/llm-d/pull/335)
  * Correct # of measured output tokens / s by [@smarterclayton](https://github.com/smarterclayton) in [#350](https://github.com/llm-d/llm-d/pull/350)
  * refactor dockerfiles to a set bash scripts by [@wseaton](https://github.com/wseaton) in [#324](https://github.com/llm-d/llm-d/pull/324)
  * Add wide ep gke test by [@rlakhtakia](https://github.com/rlakhtakia) in [#367](https://github.com/llm-d/llm-d/pull/367)
  * Intel pd workflow + v0.3 lagging updates by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#310](https://github.com/llm-d/llm-d/pull/310)
  * Use the vLLM image for xpu by [@yuanwu2017](https://github.com/yuanwu2017) in [#357](https://github.com/llm-d/llm-d/pull/357)
  * Fix markdown-link-checker failed issue by [@yuanwu2017](https://github.com/yuanwu2017) in [#385](https://github.com/llm-d/llm-d/pull/385)
  * feat: Add updated readiness probe for vLLM containers by [@rajinator](https://github.com/rajinator) in [#330](https://github.com/llm-d/llm-d/pull/330)
  * Fix queries and load script in monitoring by [@Hritik003](https://github.com/Hritik003) in [#383](https://github.com/llm-d/llm-d/pull/383)
  * Arm cuda support (from clean branch) by [@wseaton](https://github.com/wseaton) in [#382](https://github.com/llm-d/llm-d/pull/382)
  * Fix deadlink error in markdown-link-check by [@yuanwu2017](https://github.com/yuanwu2017) in [#404](https://github.com/llm-d/llm-d/pull/404)
  * Update bug report template by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#413](https://github.com/llm-d/llm-d/pull/413)
  * Fix release image tagging by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#379](https://github.com/llm-d/llm-d/pull/379)
  * Update monitoring install for CKS by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#375](https://github.com/llm-d/llm-d/pull/375)
  * Patch nvshmem to avoid an uninitialized value passed to RoCE by [@smarterclayton](https://github.com/smarterclayton) in [#407](https://github.com/llm-d/llm-d/pull/407)
  * [Docs] Fix InferencePool version number getting cut off in WideEP guide by [@tlrmchlsmth](https://github.com/tlrmchlsmth) in [#414](https://github.com/llm-d/llm-d/pull/414)
  * Fix broken monitoring dashboard link by [@smarterclayton](https://github.com/smarterclayton) in [#422](https://github.com/llm-d/llm-d/pull/422)
  * Set correct variables for built NVSHMEM by [@smarterclayton](https://github.com/smarterclayton) in [#417](https://github.com/llm-d/llm-d/pull/417)
  * Update DeepEP to a version with a patch for setting NVSHMEM HCA mappings to CUDA device by [@smarterclayton](https://github.com/smarterclayton) in [#397](https://github.com/llm-d/llm-d/pull/397)
  * swap release to tag creation image tagging by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#423](https://github.com/llm-d/llm-d/pull/423)
  * pr vs release tag by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#424](https://github.com/llm-d/llm-d/pull/424)
  * enable cache busting by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#425](https://github.com/llm-d/llm-d/pull/425)
  * release.tag_name does not exist for a tag not part of a release by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#427](https://github.com/llm-d/llm-d/pull/427)
  * unify darwin/arm64 with other platforms when install helmfile in install-deps.sh by [@yitingdc](https://github.com/yitingdc) in [#267](https://github.com/llm-d/llm-d/pull/267)
  * Set LD_LIBRARY_PATH for nvshmem appropriately by [@smarterclayton](https://github.com/smarterclayton) in [#429](https://github.com/llm-d/llm-d/pull/429)
  * Update GKE to align to UBI images by [@smarterclayton](https://github.com/smarterclayton) in [#415](https://github.com/llm-d/llm-d/pull/415)
  * updating to tags for v0.3.1 release by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#432](https://github.com/llm-d/llm-d/pull/432)
  * bugfixing by [@Gregory-Pereira](https://github.com/Gregory-Pereira) in [#433](https://github.com/llm-d/llm-d/pull/433)


## New Contributors
  * [@yankay](https://github.com/yankay) made their first contribution in [#358](https://github.com/llm-d/llm-d/pull/358)
  * [@Ayobami-00](https://github.com/Ayobami-00) made their first contribution in [#359](https://github.com/llm-d/llm-d/pull/359)
  * [@effi-ofer](https://github.com/effi-ofer) made their first contribution in [#377](https://github.com/llm-d/llm-d/pull/377)
  * [@chewong](https://github.com/chewong) made their first contribution in [#335](https://github.com/llm-d/llm-d/pull/335)
  * [@rlakhtakia](https://github.com/rlakhtakia) made their first contribution in [#367](https://github.com/llm-d/llm-d/pull/367)
  * [@rajinator](https://github.com/rajinator) made their first contribution in [#330](https://github.com/llm-d/llm-d/pull/330)
  * [@Hritik003](https://github.com/Hritik003) made their first contribution in [#383](https://github.com/llm-d/llm-d/pull/383)
  * [@yitingdc](https://github.com/yitingdc) made their first contribution in [#267](https://github.com/llm-d/llm-d/pull/267)


**Full Changelog** : [`v0.3.0...v0.3.1`](https://github.com/llm-d/llm-d/compare/v0.3.0...v0.3.1)
### Contributors
petecheslock, smarterclayton, and 12 other contributors 
Assets 2
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
3 people reacted
## v0.3.0
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
## No results found
[Gregory-Pereira](https://github.com/Gregory-Pereira) released this  10 Oct 21:50 
[ v0.3.0  ](https://github.com/llm-d/llm-d/tree/v0.3.0)
[ `116093b`](https://github.com/llm-d/llm-d/commit/116093bba2fa0f82cf959f3543c8facca82fa2a6)
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
# 📦 llm-d v0.3.0 Release Notes
This release of the `llm-d` repo will capture the release for the entirety of the project, guides, components, and all.
**Release Date:** 2025-10-10
## Core Objectives
This release had a few key objectives:
  * Increase support for specialized hardware backends (TPU, XPU)
  * Increase cloud provider support (DOKS)
  * Establish a metrics and observability story
  * Wide-ep optimizations (EPLB, DBO, Async Scheduling, etc.)


## 🧩 Component Summary  
| Component  | Version  | Previous Version  | Type  |  
| --- | --- | --- | --- |  
| llm-d/llm-d-inference-scheduler  | `v0.3.2`  | `v0.2.1`  | Image  |  
| llm-d-incubation/llm-d-modelservice  | `v0.2.10`  | `v0.2.0`  | Helm Chart  |  
| llm-d/llm-d-routing-sidecar  | `v0.3.0`  | `v0.2.0`  | Image  |  
| vllm-project/vllm  | `v0.11.0`  | `v0.10.0`  | Editable install based on precompiled wheel  |  
| llm-d/llm-d-cuda  | `v0.3.0`  | `v0.2.0`  | Image  |  
| llm-d/llm-d-gke  | `v0.3.0`  | NA (new)  | Image  |  
| llm-d/llm-d-aws  | `v0.3.0`  | NA (new)  | Image  |  
| llm-d/llm-d-xpu  | `v0.3.0`  | NA (new)  | Image  |  
| llm-d/llm-d-inference-sim  | `v0.5.1`  | `v0.3.0`  | Image  |  
| llm-d-incubation/llm-d-infra  | `v1.3.3`  | `v1.1.1`  | Helm Chart  |  
| kubernetes-sig/gateway-api-inference-extension  | `v1.0.1`  | `v0.5.1`  | Helm Chart  |  
| llm-d/llm-d-kv-cache-manager  | `v0.3.2`  | `v0.2.0`  | Go Package (consumed in `inference-scheduler`)  |  
| llm-d/llm-d-benchmark  | `v0.3.0`  | `v0.2.0`  | Tooling and Image  |  
NOTE: In future we want to support compatibility matrixes. However as we are still getting off the ground, we cannot ensure that these components work with legacy versions.
## 🔹 llm-d/llm-d-inference-scheduler
  * **Description** : This scheduler that makes optimized routing decisions for inference requests to the llm-d inference framework.
  * **Diff** : [v0.2.1 → v0.3.2 ](https://github.com/llm-d/llm-d-inference-scheduler/compare/v0.2.1...v0.3.2)


## 🔹 llm-d-incubation/llm-d-modelservice
  * **Description** : `modelservice` is a Helm chart that simplifies LLM deployment on llm-d by declaratively managing Kubernetes resources for serving base models. It enables reproducible, scalable, and tunable model deployments through modular presets, and clean integration with llm-d ecosystem components (including vLLM, Gateway API Inference Extension, LeaderWorkerSet).
  * **Diff** : [v0.2.0 → v0.2.10 ](https://github.com/llm-d-incubation/llm-d-modelservice/compare/llm-d-modelservice-v0.2.0...llm-d-modelservice-v0.2.10)


## 🔹 llm-d/llm-d-routing-sidecar
  * **Description** : A reverse proxy redirecting incoming requests to the prefill worker specified in the x-prefiller-host-port HTTP request header.
  * **Diff** : [v0.2.0 → v0.3.0](https://github.com/llm-d/llm-d-routing-sidecar/compare/v0.2.0...v0.3.0)


## 🔹 vllm-project/vllm (upstream)
  * **Description** : `vLLM` is a fast and easy-to-use library for LLM inference and serving. This project is the inferencing engine that forms the upstream of our `llm-d/llm-d` image.
  * **Diff** : [v0.10.0 → v0.11.0](https://github.com/vllm-project/vllm/compare/v0.10.0...v0.11.0)


## 🔹 llm-d/llm-d
  * **Description** : A midstreamed image of `vllm-project/vllm` for inferencing, supporting features such as PD disaggregation, KV cache awareness and more.
  * **Diff** : [v0.2.0 → v0.3.0](https://github.com/llm-d/llm-d/compare/v0.2.0...v0.3.0)
  * **Image Variants** : Different image variants of this component: 
    * XPU: `ghcr.io/llm-d/llm-d-xpu:v0.3.0`
    * AWS: `ghcr.io/llm-d/llm-d-aws:v0.3.0`
      * Release `v0.3.0` workaround for getting EFA to work
    * CUDA: `ghcr.io/llm-d/llm-d-cuda:v0.3.0`
    * GKE: `ghcr.io/llm-d/llm-d-gke:v0.3.0`
      * Release `v0.3.0` workaround for running wide-ep on GKE with H200s


## 🔹 llm-d/llm-d-inference-sim
  * **Description** : A light weight vLLM simulator emulates responses to the HTTP REST endpoints of vLLM.
  * **Diff** : [v0.3.0 → v0.5.1](https://github.com/llm-d/llm-d-inference-sim/compare/v0.3.0...v0.5.1)


## 🔹 llm-d-incubation/llm-d-infra
  * **Description** : A helm chart for deploying gateway and gateway related infrastructure assets for llm-d.
  * **Diff** : [v1.1.1 → v1.3.3](https://github.com/llm-d-incubation/llm-d-infra/compare/v1.1.1...v1.3.3)


## 🔹 kubernetes-sig/gateway-api-inference-extension
  * **Description** : A Helm chart to deploy an InferencePool, a corresponding EndpointPicker (epp) deployment, and any other related assets.
  * **Diff** : [v0.5.1 → v1.0.1](https://github.com/kubernetes-sigs/gateway-api-inference-extension/compare/v0.5.1...v1.0.1)


## 🔹 llm-d/llm-d-kv-cache-manager
  * **Description** : This repository contains the llm-d-kv-cache-manager, a pluggable service designed to enable KV-Cache Aware Routing and lay the foundation for advanced, cross-node cache coordination in vLLM-based serving platforms.
  * **Diff** : [v0.2.0 → v0.3.0](https://github.com/llm-d/llm-d-kv-cache-manager/compare/v0.2.0...v0.3.0)


## 🔹 llm-d/llm-d-benchmark
  * **Description** : This repository provides an automated workflow for benchmarking LLM inference using the llm-d stack. It includes tools for deployment, experiment execution, data collection, and teardown across multiple environments and deployment styles.
  * **Diff** : [v0.2.0 → v0.3.0](https://github.com/llm-d/llm-d-benchmark/compare/v0.2.0...v0.3.0)


For more information on any of the component project or versions, please checkout their repos directly. For information on installing and using the new release refer to our [guides](https://github.com/llm-d/llm-d/blob/v0.3.0/guides). Thank you to all contributors who helped make this happen.
Assets 2
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d/releases).
3 people reacted
Previous [1](https://github.com/llm-d/llm-d/releases?page=1) [2](https://github.com/llm-d/llm-d/releases?page=2) [Next](https://github.com/llm-d/llm-d/releases?page=2)
Previous [Next](https://github.com/llm-d/llm-d/releases?page=2)
You can’t perform that action at this time. 
