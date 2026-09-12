---
title: Releases · llm-d/llm-d-router
id: releases-llm-dllm-d-router
tags:
- llmd-router-nonk8s-backends-54b774
- discovery-config
- llm-d-router-releases
- version-gap
created: '2026-09-11T21:45:53.725010Z'
updated: '2026-09-12T03:37:37.831557Z'
source: https://github.com/llm-d/llm-d-router/releases
source_domain: github.com
fetched_at: '2026-09-11T21:45:53.691209Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'llm-d-router GitHub Releases page, tags v0.7.0-rc.1 through v0.10.0. Newest
  release: v0.10.0, released 17 Aug 2026 21:00 UTC (tag signed by hexfusion); the
  prior v0.10.0-rc.1 shipped 11 Aug 2026, v0.9.0 shipped 23 Jun 2026. v0.10.0 highlights:
  RDMA-free offloading KV connector with P2P tier port, DisaggregatedSet revision
  routing, GPU-utilization-aware routing via DCGM extractor, header-profile-handler,
  utilization-filter (renamed from active-request-filter), session_id session-affinity,
  OpenAI /v1/images/generations routing for diffusion backends, OpenTelemetry spans
  over scoring + P/D trace propagation, mTLS for EPP metrics, Coordinator serving
  /inference/v1/generate, plugin stability levels gating alpha plugins behind --allow-experimental-plugins,
  bounded/resumable KV-event replay. v0.10.0 ''Upgrade Steps & Deprecations'' section
  makes NO mention of dataLayer.discovery, endpoints.pluginRef, or any discovery-plugin
  config reshaping -- the only discovery-adjacent notes are unrelated (utilization-filter
  rename, deprecated data-layer feature gate removed per PR #1728). v0.9.0 (23 Jun
  2026) introduced the FileDiscovery plugin (PR #1135, YAML/JSON file loading with
  fsnotify watching) as a Key Feature. No v0.10.1 or later patch tag exists on this
  page as of fetch (11 Sep 2026) -- v0.10.0 GA remains the newest tagged release.
  This directly establishes that PR #2434''s dataLayer.discovery.endpoints.pluginRef
  consolidation (merged 25 Aug 2026, eight days after the v0.10.0 tag) is NOT part
  of any released version; it postdates v0.10.0 and has not yet shipped in a tag.'
utility_score: 14.0
---

[Skip to content](https://github.com/llm-d/llm-d-router/releases#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/releases) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/releases) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/releases) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


# Releases: llm-d/llm-d-router
Releases · llm-d/llm-d-router
## Release list
Previous [Next](https://github.com/llm-d/llm-d-router/releases?page=2)
Jump to release
  * [ v0.10.0  ](https://github.com/llm-d/llm-d-router/releases#release-v0.10.0)
  * [ v0.10.0-rc.1  ](https://github.com/llm-d/llm-d-router/releases#release-v0.10.0-rc.1)
  * [ v0.9.0  ](https://github.com/llm-d/llm-d-router/releases#release-v0.9.0)
  * [ v0.9.0-rc.2  ](https://github.com/llm-d/llm-d-router/releases#release-v0.9.0-rc.2)
  * [ v0.8.0  ](https://github.com/llm-d/llm-d-router/releases#release-v0.8.0)
  * [ v0.8.0-rc.2  ](https://github.com/llm-d/llm-d-router/releases#release-v0.8.0-rc.2)
  * [ v0.8.0-rc.1  ](https://github.com/llm-d/llm-d-router/releases#release-v0.8.0-rc.1)
  * [ v0.7.1  ](https://github.com/llm-d/llm-d-router/releases#release-v0.7.1)
  * [ v0.7.0  ](https://github.com/llm-d/llm-d-router/releases#release-v0.7.0)
  * [ v0.7.0-rc.1  ](https://github.com/llm-d/llm-d-router/releases#release-v0.7.0-rc.1)


Previous [Next](https://github.com/llm-d/llm-d-router/releases?page=2)
## v0.10.0
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
## No results found
[github-actions](https://github.com/apps/github-actions) released this  17 Aug 21:00 
[ v0.10.0  ](https://github.com/llm-d/llm-d-router/tree/v0.10.0)
This tag was signed with the committer’s **verified signature**. 
[hexfusion](https://github.com/hexfusion) Sam Batschelet 
GPG key ID: 85E2EAD14C6212C9
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
[ `71f4f09`](https://github.com/llm-d/llm-d-router/commit/71f4f0999f95b96c49a9d0c4afbd18dfdb943c26)
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
Release v0.10.0
## Highlights
  * **Disaggregated prefill and decode:** RDMA-free KV transfer through the new `offloading` KV connector on the sidecar, now with data-parallel support and a configurable P2P tier port ([#1888](https://github.com/llm-d/llm-d-router/pull/1888), [#2075](https://github.com/llm-d/llm-d-router/pull/2075), [#1937](https://github.com/llm-d/llm-d-router/pull/1937)). Native DisaggregatedSet revision routing for consistent rolling P/D updates ([#2141](https://github.com/llm-d/llm-d-router/pull/2141)). Load-proportional and CPU-tier-aware pull-source selection to avoid transfer hot spots ([#2032](https://github.com/llm-d/llm-d-router/pull/2032), [#2321](https://github.com/llm-d/llm-d-router/pull/2321)).
  * **Scheduling and routing:** GPU utilization-aware routing via the DCGM extractor and the generic endpoint-attribute filter and scorer ([#1904](https://github.com/llm-d/llm-d-router/pull/1904)). The `header-profile-handler` selects one scheduling profile per request from a header ([#2176](https://github.com/llm-d/llm-d-router/pull/2176)). The `utilization-filter` (formerly active-request-filter) caps endpoints on any combination of active, running, waiting, and KV-cache utilization ([#2218](https://github.com/llm-d/llm-d-router/pull/2218)). New priority-aware stepped gating and soft-reflective-ceiling policies ([#1592](https://github.com/llm-d/llm-d-router/pull/1592), [#1956](https://github.com/llm-d/llm-d-router/pull/1956)), and a new `session_id` session-affinity strategy ([#2238](https://github.com/llm-d/llm-d-router/pull/2238)).
  * **Multimodal:** OpenAI `/v1/images/generations` routing for diffusion backends such as vLLM-Omni ([#1936](https://github.com/llm-d/llm-d-router/pull/1936)), and video input in approximate prefix match ([#1911](https://github.com/llm-d/llm-d-router/pull/1911)).
  * **Observability and security:** OpenTelemetry spans over the scoring path and end-to-end P/D trace propagation ([#1834](https://github.com/llm-d/llm-d-router/pull/1834), [#1667](https://github.com/llm-d/llm-d-router/pull/1667)). mTLS for the EPP metrics endpoint and scrape sources, plus configurable ext_proc TLS minimum version and cipher suites ([#1919](https://github.com/llm-d/llm-d-router/pull/1919), [#1858](https://github.com/llm-d/llm-d-router/pull/1858), [#2230](https://github.com/llm-d/llm-d-router/pull/2230)). Bounded metric cardinality for model-name and fairness-id labels ([#1909](https://github.com/llm-d/llm-d-router/pull/1909), [#2119](https://github.com/llm-d/llm-d-router/pull/2119)).
  * **Coordinator and availability:** the Coordinator serves `/inference/v1/generate` ([#2077](https://github.com/llm-d/llm-d-router/pull/2077)). Non-leader EPP replicas keep their datastore populated so a request reaching a standby routes instead of returning 503, and `--drain-timeout` adds graceful shutdown ([#1884](https://github.com/llm-d/llm-d-router/pull/1884), [#1885](https://github.com/llm-d/llm-d-router/pull/1885)). Plugin stability levels gate alpha plugins behind `--allow-experimental-plugins` ([#2073](https://github.com/llm-d/llm-d-router/pull/2073)). KV-event replay is bounded and resumable across a dropped response ([#2410](https://github.com/llm-d/llm-d-router/pull/2410)).


## Upgrade Steps & Deprecations
  * **Action required, out-of-tree UsageLimitPolicy plugins:** `ComputeLimit` now fills a framework-provided ceilings buffer instead of returning a slice ([#2171](https://github.com/llm-d/llm-d-router/pull/2171)).
  * **Action required, P2P key rename:** the sidecar `kv_transfer_params` keys are renamed to match the vLLM OffloadingConnector branch. Upgrade the sidecar and the engine together, since a mixed pair silently disables cached-prefix pulls ([#2140](https://github.com/llm-d/llm-d-router/pull/2140)).
  * **Flow control bounded by default:** `defaultRequestTTL` defaults to 60s and per-band `maxRequests` to 5000. Scale-to-zero deployments should raise `defaultRequestTTL` above their cold-start time ([#2123](https://github.com/llm-d/llm-d-router/pull/2123)). Rejection codes now distinguish an empty pool (503) from retryable backpressure (429) ([#1751](https://github.com/llm-d/llm-d-router/pull/1751), [#2047](https://github.com/llm-d/llm-d-router/pull/2047)).
  * **Config renames:** the flow-control queue `MaxMinHeap` is now `PriorityQueue`, and the QueueCapability system and FCFS ListQueue are removed ([#1690](https://github.com/llm-d/llm-d-router/pull/1690), [#1831](https://github.com/llm-d/llm-d-router/pull/1831)). The active-request-filter plugin is now `utilization-filter` ([#2218](https://github.com/llm-d/llm-d-router/pull/2218)).
  * **Removed flags:** pd-sidecar flags deprecated since v0.7.0 are removed; use `--kv-connector`, `--enable-tls`, and `--tls-insecure-skip-verify` ([#2054](https://github.com/llm-d/llm-d-router/pull/2054)). `--vllm-port` is deprecated in favor of `--model-server-port` ([#2142](https://github.com/llm-d/llm-d-router/pull/2142)). The deprecated data-layer feature gate is removed ([#1728](https://github.com/llm-d/llm-d-router/pull/1728)).
  * **Metric renames:** EPP KV metrics move to the `llm_d_router_epp` subsystem, with the former `kvcache_*` names kept as deprecated aliases. Update dashboards and alerts ([#1886](https://github.com/llm-d/llm-d-router/pull/1886)).
  * **Publishing change:** the `-dev` charts and images are no longer published. Use the base charts with `--version v0` and the `:main` image tags ([#2048](https://github.com/llm-d/llm-d-router/pull/2048)).
  * `samplingMean` and `maxDecodeTokenSamplesForPrediction` are deprecated and ignored ([#2037](https://github.com/llm-d/llm-d-router/pull/2037)). On prefill endpoints, `InFlightLoad.Requests` now drops at first token, which changes any custom config that scores or filters on it ([#2243](https://github.com/llm-d/llm-d-router/pull/2243)).


## Known Issues
  * Endpoint score emission (`--emit-endpoint-scores`) emits nothing under the deprecated data-parallel-profile-handler, or under pd-profile-handler when `primaryPort` is set ([#2004](https://github.com/llm-d/llm-d-router/pull/2004)).


## What's Changed
**Click to expand**
  * docs: add release-note fragment for PR [#1657](https://github.com/llm-d/llm-d-router/pull/1657) by @llm-d-router-release-notes[bot] in [#1660](https://github.com/llm-d/llm-d-router/pull/1660)
  * release: configure and pass LATENCY_PREDICTOR_TAG via Makefile by [@liu-cong](https://github.com/liu-cong) in [#1675](https://github.com/llm-d/llm-d-router/pull/1675)
  * fix(sidecar): inject W3C trace context on outbound proxy requests by [@gyliu513](https://github.com/gyliu513) in [#1667](https://github.com/llm-d/llm-d-router/pull/1667)
  * Add operations.md guide for right sizing EPP container resources by [@jtechapps](https://github.com/jtechapps) in [#1616](https://github.com/llm-d/llm-d-router/pull/1616)
  * deps(go): bump llm-d-kv-cache to v0.9.0 by [@vMaroon](https://github.com/vMaroon) in [#1691](https://github.com/llm-d/llm-d-router/pull/1691)
  * fix(sidecar): allowlist selector parsing for GA InferencePool API by [@pierDipi](https://github.com/pierDipi) in [#1696](https://github.com/llm-d/llm-d-router/pull/1696)
  * fix(standalone): require modelServers.matchLabels when InferencePool is disabled by [@Jwrede](https://github.com/Jwrede) in [#1695](https://github.com/llm-d/llm-d-router/pull/1695)
  * test: consolidate and rename e2e runner scripts by [@roytman](https://github.com/roytman) in [#1698](https://github.com/llm-d/llm-d-router/pull/1698)
  * feat(standalone): align agentgateway configuration with upstream GAIE PR 2905 by [@abhay1999](https://github.com/abhay1999) in [#1600](https://github.com/llm-d/llm-d-router/pull/1600)
  * fix(scripts): support single quotes in image check by [@weizhoublue](https://github.com/weizhoublue) in [#1676](https://github.com/llm-d/llm-d-router/pull/1676)
  * fix: resolve mismatched yq path for nested router subchart values by [@weizhoublue](https://github.com/weizhoublue) in [#1683](https://github.com/llm-d/llm-d-router/pull/1683)
  * docs: add release-note fragment for PR [#1616](https://github.com/llm-d/llm-d-router/pull/1616) by @llm-d-router-release-notes[bot] in [#1687](https://github.com/llm-d/llm-d-router/pull/1687)
  * docs: add release-note fragment for PR [#1667](https://github.com/llm-d/llm-d-router/pull/1667) by @llm-d-router-release-notes[bot] in [#1685](https://github.com/llm-d/llm-d-router/pull/1685)
  * ci: propagate HF_TOKEN to e2e tests to avoid HF Hub rate limits by [@Jwrede](https://github.com/Jwrede) in [#1572](https://github.com/llm-d/llm-d-router/pull/1572)
  * ci: publish CRD manifests to release assets by [@liu-cong](https://github.com/liu-cong) in [#1713](https://github.com/llm-d/llm-d-router/pull/1713)
  * deps(actions): bump the github-actions group across 1 directory with 3 updates by [@dependabot](https://github.com/dependabot)[bot] in [#1715](https://github.com/llm-d/llm-d-router/pull/1715)
  * deps(go): bump github.com/jellydator/ttlcache/v3 from 3.4.0 to 3.4.1 in the go-dependencies group by [@dependabot](https://github.com/dependabot)[bot] in [#1709](https://github.com/llm-d/llm-d-router/pull/1709)
  * fix: bump golang.org/x/crypto to v0.52.0 to resolve Trivy CVEs by [@liu-cong](https://github.com/liu-cong) in [#1720](https://github.com/llm-d/llm-d-router/pull/1720)
  * refactor(flowcontrol): single-ended SafeQueue backed by container/heap by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#1690](https://github.com/llm-d/llm-d-router/pull/1690)
  * docs: add release-note fragment for PR [#1690](https://github.com/llm-d/llm-d-router/pull/1690) by @llm-d-router-release-notes[bot] in [#1725](https://github.com/llm-d/llm-d-router/pull/1725)
  * chore: switch default vllm port from 8001 to 8200 for sidecar by [@zdtsw](https://github.com/zdtsw) in [#1723](https://github.com/llm-d/llm-d-router/pull/1723)
  * Fix inotify pre-flight check for PROM_ENABLED on macOS by [@roytman](https://github.com/roytman) in [#1654](https://github.com/llm-d/llm-d-router/pull/1654)
  * feat(inflightloadproducer): allow configuring output ratio by [@asharkhan3101](https://github.com/asharkhan3101) in [#1731](https://github.com/llm-d/llm-d-router/pull/1731)
  * docs: add release-note fragment for PR [#1731](https://github.com/llm-d/llm-d-router/pull/1731) by @llm-d-router-release-notes[bot] in [#1743](https://github.com/llm-d/llm-d-router/pull/1743)
  * feat: Enable the disabling of feature gates that are by default on/active by [@shmuelk](https://github.com/shmuelk) in [#1728](https://github.com/llm-d/llm-d-router/pull/1728)
  * fix(metrics): reset flow control metrics and correct deprecated references by [@ishwar170695](https://github.com/ishwar170695) in [#1733](https://github.com/llm-d/llm-d-router/pull/1733)
  * fix(flowcontrol): resolve ManagedQueue eagerly and fix FlowKey.String by [@RishabhSaini](https://github.com/RishabhSaini) in [#1747](https://github.com/llm-d/llm-d-router/pull/1747)
  * Update release instruction for fetching the existing release branch by [@liu-cong](https://github.com/liu-cong) in [#1716](https://github.com/llm-d/llm-d-router/pull/1716)
  * docs: assemble re...


[Read more](https://github.com/llm-d/llm-d-router/releases/tag/v0.10.0)
### Contributors
gkneighb, mvanhorn, and 70 other contributors 
Assets 4
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
3 people reacted
## v0.10.0-rc.1
Pre-release
Pre-release
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
## No results found
[github-actions](https://github.com/apps/github-actions) released this  11 Aug 16:29 
[ v0.10.0-rc.1  ](https://github.com/llm-d/llm-d-router/tree/v0.10.0-rc.1)
This tag was signed with the committer’s **verified signature**. 
[hexfusion](https://github.com/hexfusion) Sam Batschelet 
GPG key ID: 85E2EAD14C6212C9
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
[ `4a0edbb`](https://github.com/llm-d/llm-d-router/commit/4a0edbb00d799135ea819688e80b887becdb6757)
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
GPG key ID: B5690EEEBB952194
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
Release v0.10.0-rc.1
## Highlights
  * **Disaggregated prefill and decode:** RDMA-free KV transfer through the new `offloading` KV connector on the sidecar, now with data-parallel support and a configurable P2P tier port ([#1888](https://github.com/llm-d/llm-d-router/pull/1888), [#2075](https://github.com/llm-d/llm-d-router/pull/2075), [#1937](https://github.com/llm-d/llm-d-router/pull/1937)). Native DisaggregatedSet revision routing for consistent rolling P/D updates ([#2141](https://github.com/llm-d/llm-d-router/pull/2141)). Load-proportional and CPU-tier-aware pull-source selection to avoid transfer hot spots ([#2032](https://github.com/llm-d/llm-d-router/pull/2032), [#2321](https://github.com/llm-d/llm-d-router/pull/2321)).
  * **Scheduling and routing:** GPU utilization-aware routing via the DCGM extractor and the generic endpoint-attribute filter and scorer ([#1904](https://github.com/llm-d/llm-d-router/pull/1904)). The `header-profile-handler` selects one scheduling profile per request from a header ([#2176](https://github.com/llm-d/llm-d-router/pull/2176)). The `utilization-filter` (formerly active-request-filter) caps endpoints on any combination of active, running, waiting, and KV-cache utilization ([#2218](https://github.com/llm-d/llm-d-router/pull/2218)). New priority-aware stepped gating and soft-reflective-ceiling policies ([#1592](https://github.com/llm-d/llm-d-router/pull/1592), [#1956](https://github.com/llm-d/llm-d-router/pull/1956)), and a new `session_id` session-affinity strategy ([#2238](https://github.com/llm-d/llm-d-router/pull/2238)).
  * **Multimodal:** OpenAI `/v1/images/generations` routing for diffusion backends such as vLLM-Omni ([#1936](https://github.com/llm-d/llm-d-router/pull/1936)), and video input in approximate prefix match ([#1911](https://github.com/llm-d/llm-d-router/pull/1911)).
  * **Observability and security:** OpenTelemetry spans over the scoring path and end-to-end P/D trace propagation ([#1834](https://github.com/llm-d/llm-d-router/pull/1834), [#1667](https://github.com/llm-d/llm-d-router/pull/1667)). mTLS for the EPP metrics endpoint and scrape sources, plus configurable ext_proc TLS minimum version and cipher suites ([#1919](https://github.com/llm-d/llm-d-router/pull/1919), [#1858](https://github.com/llm-d/llm-d-router/pull/1858), [#2230](https://github.com/llm-d/llm-d-router/pull/2230)). Bounded metric cardinality for model-name and fairness-id labels ([#1909](https://github.com/llm-d/llm-d-router/pull/1909), [#2119](https://github.com/llm-d/llm-d-router/pull/2119)).
  * **Coordinator and availability:** the Coordinator serves `/inference/v1/generate` ([#2077](https://github.com/llm-d/llm-d-router/pull/2077)). Non-leader EPP replicas keep their datastore populated so a request reaching a standby routes instead of returning 503, and `--drain-timeout` adds graceful shutdown ([#1884](https://github.com/llm-d/llm-d-router/pull/1884), [#1885](https://github.com/llm-d/llm-d-router/pull/1885)). Plugin stability levels gate alpha plugins behind `--allow-experimental-plugins` ([#2073](https://github.com/llm-d/llm-d-router/pull/2073)).


## Upgrade Steps & Deprecations
  * **Action required, out-of-tree UsageLimitPolicy plugins:** `ComputeLimit` now fills a framework-provided ceilings buffer instead of returning a slice ([#2171](https://github.com/llm-d/llm-d-router/pull/2171)).
  * **Action required, P2P key rename:** the sidecar `kv_transfer_params` keys are renamed to match the vLLM OffloadingConnector branch. Upgrade the sidecar and the engine together, since a mixed pair silently disables cached-prefix pulls ([#2140](https://github.com/llm-d/llm-d-router/pull/2140)).
  * **Flow control bounded by default:** `defaultRequestTTL` defaults to 60s and per-band `maxRequests` to 5000. Scale-to-zero deployments should raise `defaultRequestTTL` above their cold-start time ([#2123](https://github.com/llm-d/llm-d-router/pull/2123)). Rejection codes now distinguish an empty pool (503) from retryable backpressure (429) ([#1751](https://github.com/llm-d/llm-d-router/pull/1751), [#2047](https://github.com/llm-d/llm-d-router/pull/2047)).
  * **Config renames:** the flow-control queue `MaxMinHeap` is now `PriorityQueue`, and the QueueCapability system and FCFS ListQueue are removed ([#1690](https://github.com/llm-d/llm-d-router/pull/1690), [#1831](https://github.com/llm-d/llm-d-router/pull/1831)). The active-request-filter plugin is now `utilization-filter` ([#2218](https://github.com/llm-d/llm-d-router/pull/2218)).
  * **Removed flags:** pd-sidecar flags deprecated since v0.7.0 are removed; use `--kv-connector`, `--enable-tls`, and `--tls-insecure-skip-verify` ([#2054](https://github.com/llm-d/llm-d-router/pull/2054)). `--vllm-port` is deprecated in favor of `--model-server-port` ([#2142](https://github.com/llm-d/llm-d-router/pull/2142)). The deprecated data-layer feature gate is removed ([#1728](https://github.com/llm-d/llm-d-router/pull/1728)).
  * **Metric renames:** EPP KV metrics move to the `llm_d_router_epp` subsystem, with the former `kvcache_*` names kept as deprecated aliases. Update dashboards and alerts ([#1886](https://github.com/llm-d/llm-d-router/pull/1886)).
  * **Publishing change:** the `-dev` charts and images are no longer published. Use the base charts with `--version v0` and the `:main` image tags ([#2048](https://github.com/llm-d/llm-d-router/pull/2048)).
  * `samplingMean` and `maxDecodeTokenSamplesForPrediction` are deprecated and ignored ([#2037](https://github.com/llm-d/llm-d-router/pull/2037)). On prefill endpoints, `InFlightLoad.Requests` now drops at first token, which changes any custom config that scores or filters on it ([#2243](https://github.com/llm-d/llm-d-router/pull/2243)).


## Known Issues
  * Endpoint score emission (`--emit-endpoint-scores`) emits nothing under the deprecated data-parallel-profile-handler, or under pd-profile-handler when `primaryPort` is set ([#2004](https://github.com/llm-d/llm-d-router/pull/2004)).


## What's Changed
**Click to expand**
  * docs: add release-note fragment for PR [#1657](https://github.com/llm-d/llm-d-router/pull/1657) by @llm-d-router-release-notes[bot] in [#1660](https://github.com/llm-d/llm-d-router/pull/1660)
  * release: configure and pass LATENCY_PREDICTOR_TAG via Makefile by [@liu-cong](https://github.com/liu-cong) in [#1675](https://github.com/llm-d/llm-d-router/pull/1675)
  * fix(sidecar): inject W3C trace context on outbound proxy requests by [@gyliu513](https://github.com/gyliu513) in [#1667](https://github.com/llm-d/llm-d-router/pull/1667)
  * Add operations.md guide for right sizing EPP container resources by [@jtechapps](https://github.com/jtechapps) in [#1616](https://github.com/llm-d/llm-d-router/pull/1616)
  * deps(go): bump llm-d-kv-cache to v0.9.0 by [@vMaroon](https://github.com/vMaroon) in [#1691](https://github.com/llm-d/llm-d-router/pull/1691)
  * fix(sidecar): allowlist selector parsing for GA InferencePool API by [@pierDipi](https://github.com/pierDipi) in [#1696](https://github.com/llm-d/llm-d-router/pull/1696)
  * fix(standalone): require modelServers.matchLabels when InferencePool is disabled by [@Jwrede](https://github.com/Jwrede) in [#1695](https://github.com/llm-d/llm-d-router/pull/1695)
  * test: consolidate and rename e2e runner scripts by [@roytman](https://github.com/roytman) in [#1698](https://github.com/llm-d/llm-d-router/pull/1698)
  * feat(standalone): align agentgateway configuration with upstream GAIE PR 2905 by [@abhay1999](https://github.com/abhay1999) in [#1600](https://github.com/llm-d/llm-d-router/pull/1600)
  * fix(scripts): support single quotes in image check by [@weizhoublue](https://github.com/weizhoublue) in [#1676](https://github.com/llm-d/llm-d-router/pull/1676)
  * fix: resolve mismatched yq path for nested router subchart values by [@weizhoublue](https://github.com/weizhoublue) in [#1683](https://github.com/llm-d/llm-d-router/pull/1683)
  * docs: add release-note fragment for PR [#1616](https://github.com/llm-d/llm-d-router/pull/1616) by @llm-d-router-release-notes[bot] in [#1687](https://github.com/llm-d/llm-d-router/pull/1687)
  * docs: add release-note fragment for PR [#1667](https://github.com/llm-d/llm-d-router/pull/1667) by @llm-d-router-release-notes[bot] in [#1685](https://github.com/llm-d/llm-d-router/pull/1685)
  * ci: propagate HF_TOKEN to e2e tests to avoid HF Hub rate limits by [@Jwrede](https://github.com/Jwrede) in [#1572](https://github.com/llm-d/llm-d-router/pull/1572)
  * ci: publish CRD manifests to release assets by [@liu-cong](https://github.com/liu-cong) in [#1713](https://github.com/llm-d/llm-d-router/pull/1713)
  * deps(actions): bump the github-actions group across 1 directory with 3 updates by [@dependabot](https://github.com/dependabot)[bot] in [#1715](https://github.com/llm-d/llm-d-router/pull/1715)
  * deps(go): bump github.com/jellydator/ttlcache/v3 from 3.4.0 to 3.4.1 in the go-dependencies group by [@dependabot](https://github.com/dependabot)[bot] in [#1709](https://github.com/llm-d/llm-d-router/pull/1709)
  * fix: bump golang.org/x/crypto to v0.52.0 to resolve Trivy CVEs by [@liu-cong](https://github.com/liu-cong) in [#1720](https://github.com/llm-d/llm-d-router/pull/1720)
  * refactor(flowcontrol): single-ended SafeQueue backed by container/heap by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#1690](https://github.com/llm-d/llm-d-router/pull/1690)
  * docs: add release-note fragment for PR [#1690](https://github.com/llm-d/llm-d-router/pull/1690) by @llm-d-router-release-notes[bot] in [#1725](https://github.com/llm-d/llm-d-router/pull/1725)
  * chore: switch default vllm port from 8001 to 8200 for sidecar by [@zdtsw](https://github.com/zdtsw) in [#1723](https://github.com/llm-d/llm-d-router/pull/1723)
  * Fix inotify pre-flight check for PROM_ENABLED on macOS by [@roytman](https://github.com/roytman) in [#1654](https://github.com/llm-d/llm-d-router/pull/1654)
  * feat(inflightloadproducer): allow configuring output ratio by [@asharkhan3101](https://github.com/asharkhan3101) in [#1731](https://github.com/llm-d/llm-d-router/pull/1731)
  * docs: add release-note fragment for PR [#1731](https://github.com/llm-d/llm-d-router/pull/1731) by @llm-d-router-release-notes[bot] in [#1743](https://github.com/llm-d/llm-d-router/pull/1743)
  * feat: Enable the disabling of feature gates that are by default on/active by [@shmuelk](https://github.com/shmuelk) in [#1728](https://github.com/llm-d/llm-d-router/pull/1728)
  * fix(metrics): reset flow control metrics and correct deprecated references by [@ishwar170695](https://github.com/ishwar170695) in [#1733](https://github.com/llm-d/llm-d-router/pull/1733)
  * fix(flowcontrol): resolve ManagedQueue eagerly and fix FlowKey.String by [@RishabhSaini](https://github.com/RishabhSaini) in [#1747](https://github.com/llm-d/llm-d-router/pull/1747)
  * Update release instruction for fetching the existing release branch by [@liu-cong](https://github.com/liu-cong) in [#1716](https://github.com/llm-d/llm-d-router/pull/1716)
  * docs: assemble release notes for v0.9.0 by @llm-d-router-release-notes[bot] in [#1688](https://github.com/llm-d/llm-d-router/pull/1688)
  * docs: add...


[Read more](https://github.com/llm-d/llm-d-router/releases/tag/v0.10.0-rc.1)
### Contributors
gkneighb, mvanhorn, and 70 other contributors 
Assets 4
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
## v0.9.0
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
## No results found
[github-actions](https://github.com/apps/github-actions) released this  23 Jun 20:56 
[ v0.9.0  ](https://github.com/llm-d/llm-d-router/tree/v0.9.0)
This tag was signed with the committer’s **verified signature**. 
[liu-cong](https://github.com/liu-cong) Cong Liu 
SSH Key Fingerprint: hVOj2gqDMNSM1u+KlxU2Gjo2P323i+/ga19nOnil7d8
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
[ `5f4e762`](https://github.com/llm-d/llm-d-router/commit/5f4e762f341a5196393ce79f8a57c3e1900c4a6b)
This commit was signed with the committer’s **verified signature**. 
[liu-cong](https://github.com/liu-cong) Cong Liu 
SSH Key Fingerprint: hVOj2gqDMNSM1u+KlxU2Gjo2P323i+/ga19nOnil7d8
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
Release v0.9.0
## 1. Highlights
  * **Performance Wins:** Drastically reduced memory allocations in `Scheduler.Schedule` by ~90% on large fleets ([PR #1171](https://github.com/llm-d/llm-d-router/pull/1171)) and optimized response usage parsing in the OpenAI parser ([PR #1583](https://github.com/llm-d/llm-d-router/pull/1583)).
  * **Key Features & Additions:**
    * Added an approximate prefix-cache estimation feature for the Anthropic `v1/messages` API, including tools support ([PR #1508](https://github.com/llm-d/llm-d-router/pull/1508), [PR #1554](https://github.com/llm-d/llm-d-router/pull/1554)).
    * Introduced a new `FileDiscovery` plugin to handle YAML/JSON file loading and `fsnotify` file watching ([PR #1135](https://github.com/llm-d/llm-d-router/pull/1135)).
    * Enabled all HTTP parsers by default in the EPP ([PR #1488](https://github.com/llm-d/llm-d-router/pull/1488)) and added native metrics mapping for standard NVIDIA Triton Inference Server workloads ([PR #1207](https://github.com/llm-d/llm-d-router/pull/1207)).
    * Added stream-aware Time-to-First-Token (TTFT) and Inter-Token Latency (ITL) metrics ([PR #1499](https://github.com/llm-d/llm-d-router/pull/1499), [PR #1591](https://github.com/llm-d/llm-d-router/pull/1591)).
  * **Key Bug Fixes:** Resolved a critical memory bound issue by clamping autotuned `blockSizeTokens` in the EPP indexer ([PR #1160](https://github.com/llm-d/llm-d-router/pull/1160)), fixed a ZMQ port range validation issue to prevent subscriber startup failures on multi-rank pods ([PR #1242](https://github.com/llm-d/llm-d-router/pull/1242)), and secured TLS defaults by isolating HTTP clients ([PR #1395](https://github.com/llm-d/llm-d-router/pull/1395)).


## 2. Upgrade Steps & Deprecations
  * **Breaking Flag & CRD Deletions:**
    * Removed the deprecated `--inference-pool-name` and `--inference-pool-namespace` flags from the sidecar ([PR #1416](https://github.com/llm-d/llm-d-router/pull/1416)).
  * **Configuration Changes:**
    * EPP-managed headers have been renamed to follow the `x-llm-d` convention ([PR #1161](https://github.com/llm-d/llm-d-router/pull/1161)).
    * Flow control and request handler configuration parameters have been consolidated inside the config API ([PR #1247](https://github.com/llm-d/llm-d-router/pull/1247)).
  * **Metric System Changes:** The legacy metrics feature gate and `pkg/epp/backend/metrics` have been entirely removed ([PR #1418](https://github.com/llm-d/llm-d-router/pull/1418)). Subsystem metrics names have been unified and renamed to `llm_d_epp` ([PR #1661](https://github.com/llm-d/llm-d-router/pull/1661)), and all EPP span attributes are now normalized under the `llm_d.epp.*` namespace ([PR #1670](https://github.com/llm-d/llm-d-router/pull/1670)).
  * **Token Producer Backend Deprecation:** Deprecated the UDS-backend in `token-producer` in favor of the newer vLLM render endpoint ([PR #1079](https://github.com/llm-d/llm-d-router/pull/1079)).


## What's Changed
**Click to expand**
  * deps(go): bump the kubernetes group with 5 updates by [@dependabot](https://github.com/dependabot)[bot] in [#853](https://github.com/llm-d/llm-d-router/pull/853)
  * [migration] Clear GAIE destination paths of conflicting Go files by [@elevran](https://github.com/elevran) in [#855](https://github.com/llm-d/llm-d-router/pull/855)
  * lint: simplify golangci config for GAIE migration by [@elevran](https://github.com/elevran) in [#859](https://github.com/llm-d/llm-d-router/pull/859)
  * move scheduler pd unittests to the new locations by [@roytman](https://github.com/roytman) in [#849](https://github.com/llm-d/llm-d-router/pull/849)
  * migrate: sigs.k8s.io/gateway-api-inference-extension @ a048a796 by [@elevran](https://github.com/elevran) in [#870](https://github.com/llm-d/llm-d-router/pull/870)
  * Added datalayer plugins docs by [@ahg-g](https://github.com/ahg-g) in [#875](https://github.com/llm-d/llm-d-router/pull/875)
  * fix image build with Podman by [@roytman](https://github.com/roytman) in [#869](https://github.com/llm-d/llm-d-router/pull/869)
  * use endpoints extractor for kvevents subscription mgmt in `precise-prefix-cache-scorer` by [@vMaroon](https://github.com/vMaroon) in [#862](https://github.com/llm-d/llm-d-router/pull/862)
  * fix: [CVE-2025-61729](https://github.com/advisories/GHSA-7c64-f9jr-v9h2 "CVE-2025-61729") - pin toolchain to go1.25.8 by [@andresllh](https://github.com/andresllh) in [#861](https://github.com/llm-d/llm-d-router/pull/861)
  * fix: align pod label selector to kebab-case inference-serving by [@yankay](https://github.com/yankay) in [#854](https://github.com/llm-d/llm-d-router/pull/854)
  * move sidecar/latencypredictorasync into predictedlatency by [@kaushikmitr](https://github.com/kaushikmitr) in [#881](https://github.com/llm-d/llm-d-router/pull/881)
  * feat(epp): allow Parser.ParseRequest to signal skip and fallback to random endpoint by [@zetxqx](https://github.com/zetxqx) in [#882](https://github.com/llm-d/llm-d-router/pull/882)
  * fix(test): eliminate data race in SGLang connector test by [@elevran](https://github.com/elevran) in [#886](https://github.com/llm-d/llm-d-router/pull/886)
  * Fix: dispatch endpoint events when no PollingDataSource is configured by [@elevran](https://github.com/elevran) in [#879](https://github.com/llm-d/llm-d-router/pull/879)
  * Exclude envoyproxy.io from markdown link checker by [@roytman](https://github.com/roytman) in [#865](https://github.com/llm-d/llm-d-router/pull/865)
  * Add FULL_DUPLEX_STREAMED requirement by [@roytman](https://github.com/roytman) in [#892](https://github.com/llm-d/llm-d-router/pull/892)
  * updates to go.mod changes not included by latencypredictor client by [@elevran](https://github.com/elevran) in [#897](https://github.com/llm-d/llm-d-router/pull/897)
  * deps(actions): bump the github-actions group with 2 updates by [@dependabot](https://github.com/dependabot)[bot] in [#903](https://github.com/llm-d/llm-d-router/pull/903)
  * migrate: Correct some of the lint issues with the migrated IGW code by [@shmuelk](https://github.com/shmuelk) in [#909](https://github.com/llm-d/llm-d-router/pull/909)
  * add disk cleanup before e2e tests by [@roytman](https://github.com/roytman) in [#915](https://github.com/llm-d/llm-d-router/pull/915)
  * Refactoring and unification of e2e tests and deployment yaml files by [@roytman](https://github.com/roytman) in [#811](https://github.com/llm-d/llm-d-router/pull/811)
  * refactor: introduce ExtractorBase to remove forced no-op Extract from event-driven extractors by [@elevran](https://github.com/elevran) in [#880](https://github.com/llm-d/llm-d-router/pull/880)
  * refactor: migrate GAIE runtime strings to llm-d.ai by [@evacchi](https://github.com/evacchi) in [#922](https://github.com/llm-d/llm-d-router/pull/922)
  * refactor: align eviction plugin Type names with naming conventions by [@jia-gao](https://github.com/jia-gao) in [#894](https://github.com/llm-d/llm-d-router/pull/894)
  * fix make test-unit on arm64 by [@roytman](https://github.com/roytman) in [#940](https://github.com/llm-d/llm-d-router/pull/940)
  * Enabled more lint rules and corrected related code issues by [@shmuelk](https://github.com/shmuelk) in [#938](https://github.com/llm-d/llm-d-router/pull/938)
  * feat: Add Vertex AI parser support by [@zetxqx](https://github.com/zetxqx) in [#884](https://github.com/llm-d/llm-d-router/pull/884)
  * Add documentation to the plugins. by [@revit13](https://github.com/revit13) in [#888](https://github.com/llm-d/llm-d-router/pull/888)
  * feat(datalayer): additive ensureDataLayer with InjectDefaults opt-out by [@elevran](https://github.com/elevran) in [#941](https://github.com/llm-d/llm-d-router/pull/941)
  * migration: Migrate portions of the IGW Makefile by [@shmuelk](https://github.com/shmuelk) in [#887](https://github.com/llm-d/llm-d-router/pull/887)
  * [Migration]: Move GAIE deployment assets by [@danehans](https://github.com/danehans) in [#950](https://github.com/llm-d/llm-d-router/pull/950)
  * [Migration]: sigs.k8s.io/gateway-api-inference-extension @ 8ed5a0cd by [@hexfusion](https://github.com/hexfusion) in [#927](https://github.com/llm-d/llm-d-router/pull/927)
  * improve metrics logging by [@andreyod](https://github.com/andreyod) in [#966](https://github.com/llm-d/llm-d-router/pull/966)
  * fix: make eviction runtime a builtin, not a plugin by [@abdallahsamabd](https://github.com/abdallahsamabd) in [#898](https://github.com/llm-d/llm-d-router/pull/898)
  * feat(datalayer): plugin-driven source/extractor registration via Registrant interface by [@elevran](https://github.com/elevran) in [#942](https://github.com/llm-d/llm-d-router/pull/942)
  * feat: Add pluggable DiscoveryPlugin interface by [@ezrasilvera](https://github.com/ezrasilvera) in [#908](https://github.com/llm-d/llm-d-router/pull/908)
  * refactor(disagg): add PreRequest to disagg-profile-handler, deprecate disagg-headers-handler by [@noalimoy](https://github.com/noalimoy) in [#905](https://github.com/llm-d/llm-d-router/pull/905)
  * deps(actions): bump crate-ci/typos from 1.45.2 to 1.46.0 in the github-actions group by [@dependabot](https://github.com/dependabot)[bot] in [#985](https://github.com/llm-d/llm-d-router/pull/985)
  * refactor(test): drop igw/ prefix on integration test paths by [@hexfusion](https://github.com/hexfusion) in [#984](https://github.com/llm-d/llm-d-router/pull/984)
  * tokenizer: consolidate to a single PrepareData plugin by [@vMaroon](https://github.com/vMaroon) in [#863](https://github.com/llm-d/llm-d-router/pull/863)
  * ci: implement Phase 1 security and reliability hardening by [@Jwrede](https://github.com/Jwrede) in [#960](https://github.com/llm-d/llm-d-router/pull/960)
  * [migrate:] phase 1 of e2e tests migration from sigs.k8s.io/gateway-api-inference-extension @ ed1af5d8 by [@roytman](https://github.com/roytman) in [#987](https://github.com/llm-d/llm-d-router/pull/987)
  * Enabled more lint rules and corrected related code issues - part three by [@shmuelk](https://github.com/shmuelk) in [#990](https://github.com/llm-d/llm-d-router/pull/990)
  * Refactor label-based filters: rename, deprecate, and add runtime warn… by [@szedan-rh](https://github.com/szedan-rh) in [#842](https://github.com/llm-d/llm-d-router/pull/842)
  * [flake] Testing: wait for manager shutdown before releasing test cleanup by [@elevran](https://github.com/elevran) in [#992](https://github.com/llm-d/llm-d-router/pull/992)
  * migration: Change the Group Name in the configuration pseudo CRD to llm-d.ai by [@shmuelk](https://github.com/shmuelk) in [#972](https://github.com/llm-d/llm-d-router/pull/972)
  * Updated the main readme file by [@ahg-g](https://github.com/ahg-g) in [#978](https://github.com/llm-d/llm-d-router/pull/978)
  * ci(deps): allow only patch bumps by dependabot by [@elevran](https://github.com/elevran) in [#991](https://github.com/llm-d/llm-d-router/pull/991)
  * refactor(datalayer): simplify Collector lifecycle, add error metrics by [@hexfusion](https://github.com/hexfusion) in [#899](https://github.com/llm-d/llm-d-router/pull/899)
  * deps(go): bump the go-dependencies group across 1 directory with 3 updates by [@dependabot](https://github.com/dependabot)[bot] in [#993](https://github.com/llm-d/llm-d-router/pull/993)
  * switch to `on: pull_request_target` as GH unconditionally restricts GITHUB_TOKEN to read-only for pull_request by [@elevran](https://github.com/elevran) in [#994](https://github.com/llm-d/llm-d-router/pull/994)
  * refactor(tokenizer): return errors from PrepareRequestData instead of swallowing them by [@acardace](https://github.com/acardace) in [#998](https://github.com/llm-d/llm-d-router/pull/998)
  * Add PR size labeler workflow by [@yehuditkerido](https://github.com/yehuditkerido) in [#997](https://github.com/llm-d/llm-d-router/pull/997)
  * [sidecar] strip disaggregation headers before forwarding to model server by [@andreyod](https://github.com/andreyod) in [#999](https://github.com/llm-d/llm-d-router/pull/999)
  * fix(e2e/epp): treat non-HTTP-200 as failure in generateTraffic by [@revit13](https://github.com/revit13) in <https://github.com/llm>...


[Read more](https://github.com/llm-d/llm-d-router/releases/tag/v0.9.0)
### Contributors
gkneighb, evacchi, and 74 other contributors 
Assets 4
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
## v0.9.0-rc.2
Pre-release
Pre-release
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
## No results found
[liu-cong](https://github.com/liu-cong) released this  17 Jun 17:10 
[ v0.9.0-rc.2  ](https://github.com/llm-d/llm-d-router/tree/v0.9.0-rc.2)
This tag was signed with the committer’s **verified signature**. 
[liu-cong](https://github.com/liu-cong) Cong Liu 
SSH Key Fingerprint: hVOj2gqDMNSM1u+KlxU2Gjo2P323i+/ga19nOnil7d8
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
[ `181aa83`](https://github.com/llm-d/llm-d-router/commit/181aa8358916e19b8844ccc752b2d6153d4b2ad6)
This commit was signed with the committer’s **verified signature**. 
[liu-cong](https://github.com/liu-cong) Cong Liu 
SSH Key Fingerprint: hVOj2gqDMNSM1u+KlxU2Gjo2P323i+/ga19nOnil7d8
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
Here is a summary of the release notes for **v0.9.0-rc.2** :
## Highlights
  * **Performance Wins:** Optimizations were made to the sidecar proxy hot path for high-concurrency P/D routing ([#746](https://github.com/llm-d/llm-d-router/pull/746)) and allocations in `Scheduler.Schedule` were reduced by ~90% on large fleets ([#1171](https://github.com/llm-d/llm-d-router/pull/1171)). OpenAI parser response usage parsing allocations were also minimized ([#1583](https://github.com/llm-d/llm-d-router/pull/1583)).
  * **Key Features:** Added support for multi-modal image content in approximate prefix cache match, and encoder cache affinity scorer, Added support for the Anthropic `v1/messages` API ([#1088](https://github.com/llm-d/llm-d-router/pull/1088)), Mooncake as a new KV-connector in the routing sidecar ([#1193](https://github.com/llm-d/llm-d-router/pull/1193)), and a chunked decode feature to the sidecar ([#822](https://github.com/llm-d/llm-d-router/pull/822)).
  * **New Metrics:** Introduced inter-token latency (ITL) ([#1591](https://github.com/llm-d/llm-d-router/pull/1591)), stream-aware TPOT, and core TTFT metrics ([#1499](https://github.com/llm-d/llm-d-router/pull/1499)).
  * **Major Bug Fixes:** Resolved a panic in the SGLang proxy handling concurrent requests ([#632](https://github.com/llm-d/llm-d-router/pull/632)), fixed dropped multimodal content in prefix cache scoring/tokenizer rendering ([#781](https://github.com/llm-d/llm-d-router/pull/781)), and patched a ZMQ port range validation issue to prevent multi-rank pod failures ([#1242](https://github.com/llm-d/llm-d-router/pull/1242)).


## Upgrade Steps & Deprecations
  * **Repository & Binary Renaming:** The project has officially transitioned from `llm-d-inference-scheduler` to **`llm-d-router`**. Reflecting this change, Docker images have been renamed to`router-endpoint-picker` and `router-disagg-sidecar` ([#1098](https://github.com/llm-d/llm-d-router/pull/1098)), and configuration pseudo CRD groups are migrating to `llm-d.ai` ([#972](https://github.com/llm-d/llm-d-router/pull/972)).
  * **Removed Sidecar Flags:** The deprecated `--inference-pool-name` and `--inference-pool-namespace` flags have been permanently removed from the sidecar ([#1416](https://github.com/llm-d/llm-d-router/pull/1416)).
  * **Metric Changes:** Legacy metrics feature gates and their associated CLI flags have been removed from the EPP template ([#1418](https://github.com/llm-d/llm-d-router/pull/1418), [#1466](https://github.com/llm-d/llm-d-router/pull/1466)). The metrics subsystem has also been unified and renamed to `llm_d_epp` ([#1661](https://github.com/llm-d/llm-d-router/pull/1661)).
  * **Component Deprecations:**
    * The `disagg-headers-handler` has been deprecated in favor of `PreRequest` inside the `disagg-profile-handler` ([#905](https://github.com/llm-d/llm-d-router/pull/905)).
    * The UDS-backend in `token-producer` is deprecated; configuring the vLLM render endpoint instead is highly suggested ([#1079](https://github.com/llm-d/llm-d-router/pull/1079)).
  * **Istio & Gateway API:** Istio has been upgraded to **1.29.2** ([#1052](https://github.com/llm-d/llm-d-router/pull/1052)) and the Gateway API has been updated to **v1.5.1** ([#780](https://github.com/llm-d/llm-d-router/pull/780)). The previous workaround for vLLM Data Parallel on Istio 1.28 is now deprecated ([#727](https://github.com/llm-d/llm-d-router/pull/727)).


## What's Changed
  * Fix for flaky sites during lychee md link checker by [@pierDipi](https://github.com/pierDipi) in [#485](https://github.com/llm-d/llm-d-router/pull/485)
  * deps(actions): bump actions/checkout from 5 to 6 by [@dependabot](https://github.com/dependabot)[bot] in [#487](https://github.com/llm-d/llm-d-router/pull/487)
  * deps(go): bump google.golang.org/grpc from 1.76.0 to 1.77.0 in the go-dependencies group by [@dependabot](https://github.com/dependabot)[bot] in [#486](https://github.com/llm-d/llm-d-router/pull/486)
  * Use kv-cache-manager based on Go mod version instead of hardcoded by [@pierDipi](https://github.com/pierDipi) in [#484](https://github.com/llm-d/llm-d-router/pull/484)
  * update llm-d-kv-cache version to v0.4.0 by [@vMaroon](https://github.com/vMaroon) in [#492](https://github.com/llm-d/llm-d-router/pull/492)
  * fix: github action missing Trivy scan on sidecar image by [@zdtsw](https://github.com/zdtsw) in [#481](https://github.com/llm-d/llm-d-router/pull/481)
  * [Fix] Enhance macOS Makefile to Support Non-Homebrew Python Installations by [@hyeongyun0916](https://github.com/hyeongyun0916) in [#489](https://github.com/llm-d/llm-d-router/pull/489)
  * feat(allowlist): support both v1 and v1alpha2 InferencePool APIs with flag by [@googs1025](https://github.com/googs1025) in [#474](https://github.com/llm-d/llm-d-router/pull/474)
  * fix: make 'install-dependencies' and 'build' target by [@zdtsw](https://github.com/zdtsw) in [#493](https://github.com/llm-d/llm-d-router/pull/493)
  * skip lint and test when only docs change by [@setsunakute](https://github.com/setsunakute) in [#494](https://github.com/llm-d/llm-d-router/pull/494)
  * fix: Fixes for Data Parallel support when also running with Prefix Disaggregation by [@shmuelk](https://github.com/shmuelk) in [#498](https://github.com/llm-d/llm-d-router/pull/498)
  * sync gie to v1.2.0 by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#499](https://github.com/llm-d/llm-d-router/pull/499)
  * Error if PYTHON_CONFIG is empty by [@elevran](https://github.com/elevran) in [#497](https://github.com/llm-d/llm-d-router/pull/497)
  * Add GH action to check for signed and verified commits in PR by [@elevran](https://github.com/elevran) in [#500](https://github.com/llm-d/llm-d-router/pull/500)
  * deps(actions): bump crate-ci/typos from 1.39.2 to 1.40.0 by [@dependabot](https://github.com/dependabot)[bot] in [#501](https://github.com/llm-d/llm-d-router/pull/501)
  * build: make build should use CGO_CFLAGS, CGO_LDFLAGS by [@evacchi](https://github.com/evacchi) in [#503](https://github.com/llm-d/llm-d-router/pull/503)
  * chore: bump gie to v1.2.1 by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#504](https://github.com/llm-d/llm-d-router/pull/504)
  * deps(go): bump sigs.k8s.io/gateway-api from 1.4.0 to 1.4.1 in the kubernetes group by [@dependabot](https://github.com/dependabot)[bot] in [#508](https://github.com/llm-d/llm-d-router/pull/508)
  * deps(go): bump the go-dependencies group with 3 updates by [@dependabot](https://github.com/dependabot)[bot] in [#507](https://github.com/llm-d/llm-d-router/pull/507)
  * Miscellaneous dependency updates by [@shmuelk](https://github.com/shmuelk) in [#510](https://github.com/llm-d/llm-d-router/pull/510)
  * deps(go): bump the kubernetes group with 5 updates by [@dependabot](https://github.com/dependabot)[bot] in [#513](https://github.com/llm-d/llm-d-router/pull/513)
  * Fix running `make env-dev-kind` by [@acardace](https://github.com/acardace) in [#512](https://github.com/llm-d/llm-d-router/pull/512)
  * test: add precise_prefix_cache_test by [@evacchi](https://github.com/evacchi) in [#505](https://github.com/llm-d/llm-d-router/pull/505)
  * test: reuse upstream data store and enable logr in unit tests by [@MregXN](https://github.com/MregXN) in [#518](https://github.com/llm-d/llm-d-router/pull/518)
  * feat: allow pd_profile_handler to handle diverse plugin types by [@hyeongyun0916](https://github.com/hyeongyun0916) in [#516](https://github.com/llm-d/llm-d-router/pull/516)
  * deps(actions): bump crate-ci/typos from 1.40.0 to 1.40.1 by [@dependabot](https://github.com/dependabot)[bot] in [#526](https://github.com/llm-d/llm-d-router/pull/526)
  * deps(go): bump google.golang.org/grpc from 1.77.0 to 1.78.0 in the go-dependencies group by [@dependabot](https://github.com/dependabot)[bot] in [#527](https://github.com/llm-d/llm-d-router/pull/527)
  * feat(metrics): add model_name label to PD decision metric by [@googs1025](https://github.com/googs1025) in [#528](https://github.com/llm-d/llm-d-router/pull/528)
  * deps(actions): bump crate-ci/typos from 1.40.1 to 1.41.0 by [@dependabot](https://github.com/dependabot)[bot] in [#532](https://github.com/llm-d/llm-d-router/pull/532)
  * Configure dependabot ignores Go version updates by [@elevran](https://github.com/elevran) in [#533](https://github.com/llm-d/llm-d-router/pull/533)
  * Updates the architecture description by [@davidbreitgand](https://github.com/davidbreitgand) in [#525](https://github.com/llm-d/llm-d-router/pull/525)
  * Dependabot: exert finer control over package updates by [@elevran](https://github.com/elevran) in [#542](https://github.com/llm-d/llm-d-router/pull/542)
  * port auto-assign action from llm-d-kv-cache by [@vMaroon](https://github.com/vMaroon) in [#551](https://github.com/llm-d/llm-d-router/pull/551)
  * refactor: set python version and pin docker image with tag by [@zdtsw](https://github.com/zdtsw) in [#543](https://github.com/llm-d/llm-d-router/pull/543)
  * chore(test): update API version for nixl test by [@zdtsw](https://github.com/zdtsw) in [#555](https://github.com/llm-d/llm-d-router/pull/555)
  * deps(go): bump the go-dependencies group with 2 updates by [@dependabot](https://github.com/dependabot)[bot] in [#558](https://github.com/llm-d/llm-d-router/pull/558)
  * deps(actions): bump crate-ci/typos from 1.41.0 to 1.42.0 by [@dependabot](https://github.com/dependabot)[bot] in [#557](https://github.com/llm-d/llm-d-router/pull/557)
  * deps(actions): bump actions/checkout from 4 to 6 by [@dependabot](https://github.com/dependabot)[bot] in [#556](https://github.com/llm-d/llm-d-router/pull/556)
  * Update auto-assign logic by [@elevran](https://github.com/elevran) in [#560](https://github.com/llm-d/llm-d-router/pull/560)
  * Remove newline in unsigned commit message by [@elevran](https://github.com/elevran) in [#561](https://github.com/llm-d/llm-d-router/pull/561)
  * bump gie to v1.3.0 rc2 by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#562](https://github.com/llm-d/llm-d-router/pull/562)
  * Update OWNERS by [@elevran](https://github.com/elevran) in [#559](https://github.com/llm-d/llm-d-router/pull/559)
  * refactor: Makefile, update docs by [@zdtsw](https://github.com/zdtsw) in [#463](https://github.com/llm-d/llm-d-router/pull/463)
  * feat: add metrics validation in e2e test by [@googs1025](https://github.com/googs1025) in [#529](https://github.com/llm-d/llm-d-router/pull/529)
  * feat: make no-hit-lru P/D-aware by [@evacchi](https://github.com/evacchi) in [#522](https://github.com/llm-d/llm-d-router/pull/522)
  * Update disaggregated Prefill/Decode inference serving documentation by [@mayabar](https://github.com/mayabar) in [#571](https://github.com/llm-d/llm-d-router/pull/571)
  * deps(actions): bump crate-ci/typos from 1.42.0 to 1.42.1 by [@dependabot](https://github.com/dependabot)[bot] in [#572](https://github.com/llm-d/llm-d-router/pull/572)
  * deps(go): bump github.com/onsi/ginkgo/v2 from 2.27.4 to 2.27.5 in the go-dependencies group by [@dependabot](https://github.com/dependabot)[bot] in [#573](https://github.com/llm-d/llm-d-router/pull/573)
  * fix reviewers auto assign minor bug by [@nirrozenbaum](https://github.com/nirrozenbaum) in [#575](https://github.com/llm-d/llm-d-router/pull/575)
  * fix(scorer): make active request pd aware by [@kyanokashi](https://github.com/kyanokashi) in [#569](https://github.com/llm-d/llm-d-router/pull/569)
  * test(e2e): cleanup kind cluster by [@zdtsw](https://github.com/zdtsw) in [#563](https://github.com/llm-d/llm-d-router/pull/563)
  * refactor: add early validation in DP profile handler by [@zdtsw](https://github.com/zdtsw) in [#554](https://github.com/llm-d/llm-d-router/pull/554)
  * deps(go): bump the kubernetes group with 2 updates by [@dependabot](https://github.com/dependabot)[bot] in [#574](https://github.com/llm-d/llm-d-router/pull/574)
  * refactor: kv cache manager repo by [@sagearc](https://github.com/sagearc) in [#570](https://github.com/llm-d/llm-d-router/pull/570)
  * bumping IGW version to the full released version by [@kfswain](https://github.com/kfswain) in [#583](https://github.com/llm-d/llm-d-router/pull/583)
  * Enable prefix-...


[Read more](https://github.com/llm-d/llm-d-router/releases/tag/v0.9.0-rc.2)
### Contributors
gkneighb, evacchi, and 88 other contributors 
Assets 2
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
## v0.8.0
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
## No results found
[vMaroon](https://github.com/vMaroon) released this  28 Apr 19:38 
[ v0.8.0  ](https://github.com/llm-d/llm-d-router/tree/v0.8.0)
[ `05c91f0`](https://github.com/llm-d/llm-d-router/commit/05c91f0d655fbd7ea71fc50949c899c360d06f07)
This commit was signed with the committer’s **verified signature**. 
[vMaroon](https://github.com/vMaroon) Maroon Ayoub 
GPG key ID: 8AE3727338CA27C3
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
## What's Changed
  * [bug] fix multimodal content dropped in prefix cache scoring and tokenizer chat rendering by [@vMaroon](https://github.com/vMaroon) in [#781](https://github.com/llm-d/llm-d-router/pull/781)
  * [Docs] Restructure development.md by [@elevran](https://github.com/elevran) in [#769](https://github.com/llm-d/llm-d-router/pull/769)
  * deps(actions): bump actions/cache from 4 to 5 in the github-actions group by [@dependabot](https://github.com/dependabot)[bot] in [#788](https://github.com/llm-d/llm-d-router/pull/788)
  * Print Pod statuses and logs if e2e tests fail by [@roytman](https://github.com/roytman) in [#783](https://github.com/llm-d/llm-d-router/pull/783)
  * Propagates relevant environment variables into test container by [@roytman](https://github.com/roytman) in [#785](https://github.com/llm-d/llm-d-router/pull/785)
  * bump kvc import to v0.7.0 by [@vMaroon](https://github.com/vMaroon) in [#789](https://github.com/llm-d/llm-d-router/pull/789)
  * import kvc-v0.7.1 by [@vMaroon](https://github.com/vMaroon) in [#796](https://github.com/llm-d/llm-d-router/pull/796)
  * Add tests for disaggregated Encoder. by [@revit13](https://github.com/revit13) in [#777](https://github.com/llm-d/llm-d-router/pull/777)
  * (1/2) import igw v1.5.0-rc1 by [@zetxqx](https://github.com/zetxqx) in [#806](https://github.com/llm-d/llm-d-router/pull/806)
  * deps(go): bump go.opentelemetry.io/contrib/instrumentation/net/http/otelhttp from 0.67.0 to 0.68.0 in the go-dependencies group across 1 directory by [@dependabot](https://github.com/dependabot)[bot] in [#809](https://github.com/llm-d/llm-d-router/pull/809)
  * deps(actions): bump crate-ci/typos from 1.44.0 to 1.45.0 in the github-actions group across 1 directory by [@dependabot](https://github.com/dependabot)[bot] in [#801](https://github.com/llm-d/llm-d-router/pull/801)
  * perf: optimize sidecar proxy hot path for high-concurrency P/D routing by [@tlrmchlsmth](https://github.com/tlrmchlsmth) in [#746](https://github.com/llm-d/llm-d-router/pull/746)
  * [feat] [cicd] Use distroless/static as default runtime base image by [@elevran](https://github.com/elevran) in [#795](https://github.com/llm-d/llm-d-router/pull/795)
  * Support image build with different base via Maefile and document runtime debugging by [@elevran](https://github.com/elevran) in [#821](https://github.com/llm-d/llm-d-router/pull/821)
  * [cicd] Compare coverage against `main` and latest `release-*` by [@elevran](https://github.com/elevran) in [#757](https://github.com/llm-d/llm-d-router/pull/757)
  * deps(actions): bump the github-actions group across 1 directory with 2 updates by [@dependabot](https://github.com/dependabot)[bot] in [#824](https://github.com/llm-d/llm-d-router/pull/824)
  * Script to automate migration for GAIE code into llm-d by [@elevran](https://github.com/elevran) in [#804](https://github.com/llm-d/llm-d-router/pull/804)
  * feat: Add support for Responses API by [@asharkhan3101](https://github.com/asharkhan3101) in [#599](https://github.com/llm-d/llm-d-router/pull/599)
  * update image-build-builder rule by [@roytman](https://github.com/roytman) in [#830](https://github.com/llm-d/llm-d-router/pull/830)
  * [cicd] Add release notes action by [@elevran](https://github.com/elevran) in [#791](https://github.com/llm-d/llm-d-router/pull/791)
  * switch active-request scorer's logging from debug to trace by [@vMaroon](https://github.com/vMaroon) in [#825](https://github.com/llm-d/llm-d-router/pull/825)
  * Add ahg-g as an owner by [@ahg-g](https://github.com/ahg-g) in [#835](https://github.com/llm-d/llm-d-router/pull/835)
  * Refactoring plugins according to GAIE structure by [@roytman](https://github.com/roytman) in [#827](https://github.com/llm-d/llm-d-router/pull/827)
  * Add lint exclusions from GAIE by [@elevran](https://github.com/elevran) in [#834](https://github.com/llm-d/llm-d-router/pull/834)
  * fix: broken link after refactoring by [@zdtsw](https://github.com/zdtsw) in [#839](https://github.com/llm-d/llm-d-router/pull/839)
  * deps(go): bump github.com/moby/spdystream from 0.5.0 to 0.5.1 by [@dependabot](https://github.com/dependabot)[bot] in [#838](https://github.com/llm-d/llm-d-router/pull/838)
  * migration scripts: improve tooling robustness by [@elevran](https://github.com/elevran) in [#843](https://github.com/llm-d/llm-d-router/pull/843)
  * Update to GAIE v1.5 by [@elevran](https://github.com/elevran) in [#844](https://github.com/llm-d/llm-d-router/pull/844)
  * fix: [CVE-2025-61729](https://github.com/advisories/GHSA-7c64-f9jr-v9h2 "CVE-2025-61729") - pin toolchain to go1.25.8 by [@andresllh](https://github.com/andresllh) [#861](https://github.com/llm-d/llm-d-router/pull/861)
  * use endpoints extractor for kvevents subscription mgmt in precise-prefix-cache-scorer by [@vMaroon](https://github.com/vMaroon) in [#862](https://github.com/llm-d/llm-d-router/pull/862)
  * fix image build with Podman by [@roytman](https://github.com/roytman) in [#869](https://github.com/llm-d/llm-d-router/pull/869)


## New Contributors
  * [@revit13](https://github.com/revit13) made their first contribution in [#777](https://github.com/llm-d/llm-d-router/pull/777)
  * [@zetxqx](https://github.com/zetxqx) made their first contribution in [#806](https://github.com/llm-d/llm-d-router/pull/806)
  * [@tlrmchlsmth](https://github.com/tlrmchlsmth) made their first contribution in [#746](https://github.com/llm-d/llm-d-router/pull/746)
  * [@asharkhan3101](https://github.com/asharkhan3101) made their first contribution in [#599](https://github.com/llm-d/llm-d-router/pull/599)


**Full Changelog** : [`v0.7.1...v0.8.0`](https://github.com/llm-d/llm-d-router/compare/v0.7.1...v0.8.0)
### Contributors
zdtsw, tlrmchlsmth, and 9 other contributors 
Assets 2
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
## v0.8.0-rc.2
Pre-release
Pre-release
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
## No results found
[vMaroon](https://github.com/vMaroon) released this  25 Apr 20:15 
[ v0.8.0-rc.2  ](https://github.com/llm-d/llm-d-router/tree/v0.8.0-rc.2)
[ `05c91f0`](https://github.com/llm-d/llm-d-router/commit/05c91f0d655fbd7ea71fc50949c899c360d06f07)
This commit was signed with the committer’s **verified signature**. 
[vMaroon](https://github.com/vMaroon) Maroon Ayoub 
GPG key ID: 8AE3727338CA27C3
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
This release candidate picks up a CVE fix:


**Full Changelog** : [`v0.8.0-rc.1...v0.8.0-rc.2`](https://github.com/llm-d/llm-d-router/compare/v0.8.0-rc.1...v0.8.0-rc.2)
### Contributors
andresllh 
Assets 2
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
## v0.8.0-rc.1
Pre-release
Pre-release
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
## No results found
[vMaroon](https://github.com/vMaroon) released this  24 Apr 10:30 
[ v0.8.0-rc.1  ](https://github.com/llm-d/llm-d-router/tree/v0.8.0-rc.1)
[ `72db945`](https://github.com/llm-d/llm-d-router/commit/72db94559d46b20f53cecd955223d04c90712303)
This commit was signed with the committer’s **verified signature**. 
[vMaroon](https://github.com/vMaroon) Maroon Ayoub 
GPG key ID: 8AE3727338CA27C3
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
## What's Changed
  * [bug] fix multimodal content dropped in prefix cache scoring and tokenizer chat rendering by [@vMaroon](https://github.com/vMaroon) in [#781](https://github.com/llm-d/llm-d-router/pull/781)
  * [Docs] Restructure development.md by [@elevran](https://github.com/elevran) in [#769](https://github.com/llm-d/llm-d-router/pull/769)
  * deps(actions): bump actions/cache from 4 to 5 in the github-actions group by [@dependabot](https://github.com/dependabot)[bot] in [#788](https://github.com/llm-d/llm-d-router/pull/788)
  * Print Pod statuses and logs if e2e tests fail by [@roytman](https://github.com/roytman) in [#783](https://github.com/llm-d/llm-d-router/pull/783)
  * Propagates relevant environment variables into test container by [@roytman](https://github.com/roytman) in [#785](https://github.com/llm-d/llm-d-router/pull/785)
  * bump kvc import to v0.7.0 by [@vMaroon](https://github.com/vMaroon) in [#789](https://github.com/llm-d/llm-d-router/pull/789)
  * import kvc-v0.7.1 by [@vMaroon](https://github.com/vMaroon) in [#796](https://github.com/llm-d/llm-d-router/pull/796)
  * Add tests for disaggregated Encoder. by [@revit13](https://github.com/revit13) in [#777](https://github.com/llm-d/llm-d-router/pull/777)
  * (1/2) import igw v1.5.0-rc1 by [@zetxqx](https://github.com/zetxqx) in [#806](https://github.com/llm-d/llm-d-router/pull/806)
  * deps(go): bump go.opentelemetry.io/contrib/instrumentation/net/http/otelhttp from 0.67.0 to 0.68.0 in the go-dependencies group across 1 directory by [@dependabot](https://github.com/dependabot)[bot] in [#809](https://github.com/llm-d/llm-d-router/pull/809)
  * deps(actions): bump crate-ci/typos from 1.44.0 to 1.45.0 in the github-actions group across 1 directory by [@dependabot](https://github.com/dependabot)[bot] in [#801](https://github.com/llm-d/llm-d-router/pull/801)
  * perf: optimize sidecar proxy hot path for high-concurrency P/D routing by [@tlrmchlsmth](https://github.com/tlrmchlsmth) in [#746](https://github.com/llm-d/llm-d-router/pull/746)
  * [feat] [cicd] Use distroless/static as default runtime base image by [@elevran](https://github.com/elevran) in [#795](https://github.com/llm-d/llm-d-router/pull/795)
  * Support image build with different base via Maefile and document runtime debugging by [@elevran](https://github.com/elevran) in [#821](https://github.com/llm-d/llm-d-router/pull/821)
  * [cicd] Compare coverage against `main` and latest `release-*` by [@elevran](https://github.com/elevran) in [#757](https://github.com/llm-d/llm-d-router/pull/757)
  * deps(actions): bump the github-actions group across 1 directory with 2 updates by [@dependabot](https://github.com/dependabot)[bot] in [#824](https://github.com/llm-d/llm-d-router/pull/824)
  * Script to automate migration for GAIE code into llm-d by [@elevran](https://github.com/elevran) in [#804](https://github.com/llm-d/llm-d-router/pull/804)
  * feat: Add support for Responses API by [@asharkhan3101](https://github.com/asharkhan3101) in [#599](https://github.com/llm-d/llm-d-router/pull/599)
  * update image-build-builder rule by [@roytman](https://github.com/roytman) in [#830](https://github.com/llm-d/llm-d-router/pull/830)
  * [cicd] Add release notes action by [@elevran](https://github.com/elevran) in [#791](https://github.com/llm-d/llm-d-router/pull/791)
  * switch active-request scorer's logging from debug to trace by [@vMaroon](https://github.com/vMaroon) in [#825](https://github.com/llm-d/llm-d-router/pull/825)
  * Add ahg-g as an owner by [@ahg-g](https://github.com/ahg-g) in [#835](https://github.com/llm-d/llm-d-router/pull/835)
  * Refactoring plugins according to GAIE structure by [@roytman](https://github.com/roytman) in [#827](https://github.com/llm-d/llm-d-router/pull/827)
  * Add lint exclusions from GAIE by [@elevran](https://github.com/elevran) in [#834](https://github.com/llm-d/llm-d-router/pull/834)
  * fix: broken link after refactoring by [@zdtsw](https://github.com/zdtsw) in [#839](https://github.com/llm-d/llm-d-router/pull/839)
  * deps(go): bump github.com/moby/spdystream from 0.5.0 to 0.5.1 by [@dependabot](https://github.com/dependabot)[bot] in [#838](https://github.com/llm-d/llm-d-router/pull/838)
  * migration scripts: improve tooling robustness by [@elevran](https://github.com/elevran) in [#843](https://github.com/llm-d/llm-d-router/pull/843)
  * Update to GAIE v1.5 by [@elevran](https://github.com/elevran) in [#844](https://github.com/llm-d/llm-d-router/pull/844)
  * use endpoints extractor for kvevents subscription mgmt in precise-prefix-cache-scorer by [@vMaroon](https://github.com/vMaroon) in [#862](https://github.com/llm-d/llm-d-router/pull/862)
  * fix image build with Podman by [@roytman](https://github.com/roytman) in [#869](https://github.com/llm-d/llm-d-router/pull/869)


## New Contributors
  * [@revit13](https://github.com/revit13) made their first contribution in [#777](https://github.com/llm-d/llm-d-router/pull/777)
  * [@zetxqx](https://github.com/zetxqx) made their first contribution in [#806](https://github.com/llm-d/llm-d-router/pull/806)
  * [@tlrmchlsmth](https://github.com/tlrmchlsmth) made their first contribution in [#746](https://github.com/llm-d/llm-d-router/pull/746)
  * [@asharkhan3101](https://github.com/asharkhan3101) made their first contribution in [#599](https://github.com/llm-d/llm-d-router/pull/599)


**Full Changelog** : [`v0.7.1...v0.8.0-rc.1`](https://github.com/llm-d/llm-d-router/compare/v0.7.1...v0.8.0-rc.1)
### Contributors
zdtsw, tlrmchlsmth, and 8 other contributors 
Assets 2
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
## v0.7.1
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
## No results found
[elevran](https://github.com/elevran) released this  03 Apr 10:25 
[ v0.7.1  ](https://github.com/llm-d/llm-d-router/tree/v0.7.1)
This tag was signed with the committer’s **verified signature**. 
[elevran](https://github.com/elevran) Etai Lev Ran 
SSH Key Fingerprint: MaRGtJ1PGrgungGrj+Ynt3KzOYKccSp7XkPaDLhNnvc
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
[ `2d2799f`](https://github.com/llm-d/llm-d-router/commit/2d2799ff0d719a409870bfb2164018556243f74c)
This commit was signed with the committer’s **verified signature**. 
[elevran](https://github.com/elevran) Etai Lev Ran 
SSH Key Fingerprint: MaRGtJ1PGrgungGrj+Ynt3KzOYKccSp7XkPaDLhNnvc
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
Inference Scheduler release v0.7.1
This patch release updates KV-Cache to v0.7.1. See [v0.7.0 release notes](https://github.com/llm-d/llm-d-inference-scheduler/releases/tag/v0.7.0).
Docker images are available:

```
# EPP
docker pull ghcr.io/llm-d/llm-d-inference-scheduler:v0.7.1
# sidecar
docker pull ghcr.io/llm-d/llm-d-routing-sidecar:v0.7.1
```

Assets 2
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
## v0.7.0
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
## No results found
[elevran](https://github.com/elevran) released this  02 Apr 16:27 
[ v0.7.0  ](https://github.com/llm-d/llm-d-router/tree/v0.7.0)
This tag was signed with the committer’s **verified signature**. 
[elevran](https://github.com/elevran) Etai Lev Ran 
SSH Key Fingerprint: MaRGtJ1PGrgungGrj+Ynt3KzOYKccSp7XkPaDLhNnvc
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
[ `6f5cb93`](https://github.com/llm-d/llm-d-router/commit/6f5cb9333d1648d6e9328991b8fb027f3bc51114)
This commit was signed with the committer’s **verified signature**. 
[elevran](https://github.com/elevran) Etai Lev Ran 
SSH Key Fingerprint: MaRGtJ1PGrgungGrj+Ynt3KzOYKccSp7XkPaDLhNnvc
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
llm-d-inference-scheduler v0.7.0 Release
Docker images are available:

```
# EPP
docker pull ghcr.io/llm-d/llm-d-inference-scheduler:v0.7.0
# sidecar
docker pull ghcr.io/llm-d/llm-d-routing-sidecar:v0.7.0
```

## Highlights
  * Gateway API Inference Extension v1.4 (and Gateway API v1.5.1): standalone EPP, InferencePool / Helm / gRPC-related improvements, significant work in flow control and datalayer internals.
  * Initial EPD support (disaggregation profile, KV cache enablement).
  * Sidecar command line option handling has been cleaned up and simplified.
  * Numerous testing and developer experience improvements (e.g., new Makefile targets. elimination of CGO requirements, containerized builds, improved Podman support, automatic dev image built on every PR to main, etc.)


Note
Trivy reports a [CVE-2026-4046 on ubi9-micro](https://access.redhat.com/security/cve/cve-2026-4046). As both inference-scheduler and sidecar executables are built with `CGO_ENABLED=0`, the binaries are fully statically linked and do not link against glibc at all. This is listed for awareness.
## What's Changed
  * fix(ci): update Trivy to v0.69.2 by [@pierDipi](https://github.com/pierDipi) in [#675](https://github.com/llm-d/llm-d-router/pull/675)
  * Allow sidecar server to reload TLS certificates by [@pierDipi](https://github.com/pierDipi) in [#607](https://github.com/llm-d/llm-d-router/pull/607)
  * Use trivy action for image scanning by [@elevran](https://github.com/elevran) in [#688](https://github.com/llm-d/llm-d-router/pull/688)
  * deps(go): bump the go-dependencies group with 7 updates by [@dependabot](https://github.com/dependabot)[bot] in [#692](https://github.com/llm-d/llm-d-router/pull/692)
  * feat(sidecar): simplify TLS command line options with StringSlice flags by [@gyliu513](https://github.com/gyliu513) in [#682](https://github.com/llm-d/llm-d-router/pull/682)
  * Fix terminolgy and add links to docs/archiecture.md by [@elevran](https://github.com/elevran) in [#695](https://github.com/llm-d/llm-d-router/pull/695)
  * replace map[string]bool with map[string]struct{} by [@roytman](https://github.com/roytman) in [#696](https://github.com/llm-d/llm-d-router/pull/696)
  * Add presubmit makefile target for local verification before pushing a PR by [@elevran](https://github.com/elevran) in [#687](https://github.com/llm-d/llm-d-router/pull/687)
  * Update Trivy scan to run newer version with explicit auth tokens by [@elevran](https://github.com/elevran) in [#698](https://github.com/llm-d/llm-d-router/pull/698)
  * Remove extra trivy params by [@elevran](https://github.com/elevran) in [#702](https://github.com/llm-d/llm-d-router/pull/702)
  * fix: simplify InferencePool flag to namespace/name format by [@gyliu513](https://github.com/gyliu513) in [#685](https://github.com/llm-d/llm-d-router/pull/685)
  * Trivy complains of user-password mismatch by [@elevran](https://github.com/elevran) in [#704](https://github.com/llm-d/llm-d-router/pull/704)
  * fix(test): Add unit test for pd_prerequest.go by [@gyliu513](https://github.com/gyliu513) in [#706](https://github.com/llm-d/llm-d-router/pull/706)
  * Remove trivy cache and enable manual workflow dispatch by [@elevran](https://github.com/elevran) in [#713](https://github.com/llm-d/llm-d-router/pull/713)
  * initial E/PD extension of the sidecar by [@roytman](https://github.com/roytman) in [#643](https://github.com/llm-d/llm-d-router/pull/643)
  * Check for uniqueness of media URLs by [@roytman](https://github.com/roytman) in [#717](https://github.com/llm-d/llm-d-router/pull/717)
  * Move typo checking from tools makefile to main, under lint by [@elevran](https://github.com/elevran) in [#719](https://github.com/llm-d/llm-d-router/pull/719)
  * Rename EncoderPodsHeader according to other constants by [@roytman](https://github.com/roytman) in [#721](https://github.com/llm-d/llm-d-router/pull/721)
  * Implement Options pattern for sidecar proxy by [@Mohamedma96](https://github.com/Mohamedma96) in [#697](https://github.com/llm-d/llm-d-router/pull/697)
  * Rename common constants by [@roytman](https://github.com/roytman) in [#722](https://github.com/llm-d/llm-d-router/pull/722)
  * deps(actions): bump dorny/paths-filter from 3 to 4 by [@dependabot](https://github.com/dependabot)[bot] in [#723](https://github.com/llm-d/llm-d-router/pull/723)
  * Enable major version updates to gh actions by [@elevran](https://github.com/elevran) in [#714](https://github.com/llm-d/llm-d-router/pull/714)
  * NonCachedTokens defines the minimum number of non-cached tokens required by [@modassarrana89-new](https://github.com/modassarrana89-new) in [#691](https://github.com/llm-d/llm-d-router/pull/691)
  * Add external tokenizer PrepareData plugin and TokenizedPrompt scorer support by [@acardace](https://github.com/acardace) in [#694](https://github.com/llm-d/llm-d-router/pull/694)
  * Deprecate the workaround used to support vLLM Data Parallel on Istio 1.28 by [@shmuelk](https://github.com/shmuelk) in [#727](https://github.com/llm-d/llm-d-router/pull/727)
  * build: remove CGO dependency by migrating to pure-Go ZMQ by [@elevran](https://github.com/elevran) in [#728](https://github.com/llm-d/llm-d-router/pull/728)
  * deps(go): bump google.golang.org/grpc from 1.79.2 to 1.79.3 by [@dependabot](https://github.com/dependabot)[bot] in [#737](https://github.com/llm-d/llm-d-router/pull/737)
  * Use kubectl kustomize instead of standalone kustomize by [@elevran](https://github.com/elevran) in [#741](https://github.com/llm-d/llm-d-router/pull/741)
  * Add idle pod config to active-request-scorer by [@dagrayvid](https://github.com/dagrayvid) in [#669](https://github.com/llm-d/llm-d-router/pull/669)
  * [build] Optimize docker build (local and CICD) by [@elevran](https://github.com/elevran) in [#740](https://github.com/llm-d/llm-d-router/pull/740)
  * feat: speculative indexing for PrecisePrefixCacheScorer by [@bongwoobak](https://github.com/bongwoobak) in [#659](https://github.com/llm-d/llm-d-router/pull/659)
  * Remove obsolete hashBlockSize by [@roytman](https://github.com/roytman) in [#748](https://github.com/llm-d/llm-d-router/pull/748)
  * feat: add optional Prometheus monitoring to Kind dev environment by [@hexfusion](https://github.com/hexfusion) in [#742](https://github.com/llm-d/llm-d-router/pull/742)
  * fix: support podman-docker in e2e tests and Makefile by [@hexfusion](https://github.com/hexfusion) in [#730](https://github.com/llm-d/llm-d-router/pull/730)
  * Remove UDS tokenizer image build from inference scheduler repo by [@elevran](https://github.com/elevran) in [#739](https://github.com/llm-d/llm-d-router/pull/739)
  * deps(go): bump the kubernetes group with 5 updates by [@dependabot](https://github.com/dependabot)[bot] in [#754](https://github.com/llm-d/llm-d-router/pull/754)
  * test: add disruption e2e tests for scheduler failure scenarios by [@hexfusion](https://github.com/hexfusion) in [#735](https://github.com/llm-d/llm-d-router/pull/735)
  * Unified Disaggregate Handler by [@roytman](https://github.com/roytman) in [#732](https://github.com/llm-d/llm-d-router/pull/732)
  * [build] Add test coverage reporting by [@elevran](https://github.com/elevran) in [#749](https://github.com/llm-d/llm-d-router/pull/749)
  * [refactor] Unify sidecar `Config` and `Options` by [@elevran](https://github.com/elevran) in [#751](https://github.com/llm-d/llm-d-router/pull/751)
  * [cicd[ Make coverage comparison optional by [@elevran](https://github.com/elevran) in https://github.com/[/pull/762](https://github.com/llm-d/llm-d-router/pull/762)
  * A temporary fix to use the previous simulator image by [@roytman](https://github.com/roytman) in [#766](https://github.com/llm-d/llm-d-router/pull/766)
  * Combine EncodeHeaderHandler and PrefillHeaderHandler into a single DisaggHeadersHandler by [@roytman](https://github.com/roytman) in [#758](https://github.com/llm-d/llm-d-router/pull/758)
  * Prevent mismatch between new and deprecated APIs by [@roytman](https://github.com/roytman) in [#756](https://github.com/llm-d/llm-d-router/pull/756)
  * Revert IGW import to 1.4.0 and update tokenizer plugin accordingly by [@vMaroon](https://github.com/vMaroon) in [#770](https://github.com/llm-d/llm-d-router/pull/770)
  * fix(test): Increase test coverage for prefix_based_pd_decider.go by [@gyliu513](https://github.com/gyliu513) in [#715](https://github.com/llm-d/llm-d-router/pull/715)
  * Run unit, integration tests and builds in a container by [@acardace](https://github.com/acardace) in [#521](https://github.com/llm-d/llm-d-router/pull/521)
  * update Makefile to run e2e tests locally with Podman by [@roytman](https://github.com/roytman) in [#775](https://github.com/llm-d/llm-d-router/pull/775)
  * implement context-length-aware plugin (scorer/filter) by [@vMaroon](https://github.com/vMaroon) in [#550](https://github.com/llm-d/llm-d-router/pull/550)
  * Build images use Go builtin cross compilation by [@shmuelk](https://github.com/shmuelk) in [#776](https://github.com/llm-d/llm-d-router/pull/776)
  * feat: bump llm-d-kv-cache for MM-aware prefix-cache routing by [@vMaroon](https://github.com/vMaroon) in [#772](https://github.com/llm-d/llm-d-router/pull/772)
  * change simulator version to v0.8.1 by [@mayabar](https://github.com/mayabar) in [#773](https://github.com/llm-d/llm-d-router/pull/773)
  * [fix] [cicd] Add missing newline at end of OWNERS, make script more robust for missing newlines by [@elevran](https://github.com/elevran) in [#779](https://github.com/llm-d/llm-d-router/pull/779)
  * [fix] Added custom LevelEncoder for sidecar by [@elevran](https://github.com/elevran) in [#778](https://github.com/llm-d/llm-d-router/pull/778)
  * Update GW API to v1.5.1 by [@elevran](https://github.com/elevran) in [#780](https://github.com/llm-d/llm-d-router/pull/780)
  * Add E2E_KEEP_CLUSTER_ON_FAILURE env variable by [@roytman](https://github.com/roytman) in [#782](https://github.com/llm-d/llm-d-router/pull/782)
  * bump kvc import to v0.7.0 by [@vMaroon](https://github.com/vMaroon) [#789](https://github.com/llm-d/llm-d-router/pull/789)


## New Contributors
  * [@Mohamedma96](https://github.com/Mohamedma96) made their first contribution in [#697](https://github.com/llm-d/llm-d-router/pull/697)
  * [@modassarrana89-new](https://github.com/modassarrana89-new) made their first contribution in [#691](https://github.com/llm-d/llm-d-router/pull/691)
  * [@dagrayvid](https://github.com/dagrayvid) made their first contribution in [#669](https://github.com/llm-d/llm-d-router/pull/669)
  * [@hexfusion](https://github.com/hexfusion) made their first contribution in [#742](https://github.com/llm-d/llm-d-router/pull/742)


**Full Changelog** : [`v0.6.0...v0.7.0`](https://github.com/llm-d/llm-d-router/compare/v0.6.0...v0.7.0)
### Contributors
hexfusion, roytman, and 12 other contributors 
Assets 2
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
## v0.7.0-rc.1
Pre-release
Pre-release
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
## No results found
[elevran](https://github.com/elevran) released this  30 Mar 12:49 
[ v0.7.0-rc.1  ](https://github.com/llm-d/llm-d-router/tree/v0.7.0-rc.1)
This tag was signed with the committer’s **verified signature**. 
[elevran](https://github.com/elevran) Etai Lev Ran 
SSH Key Fingerprint: MaRGtJ1PGrgungGrj+Ynt3KzOYKccSp7XkPaDLhNnvc
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
[ `6ca6f0b`](https://github.com/llm-d/llm-d-router/commit/6ca6f0bfe8d45b07e535d14b4f8afff0616157b4)
This commit was signed with the committer’s **verified signature**. 
[elevran](https://github.com/elevran) Etai Lev Ran 
SSH Key Fingerprint: MaRGtJ1PGrgungGrj+Ynt3KzOYKccSp7XkPaDLhNnvc
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
llm-d-inference-scheduler v0.7.0-rc.1 Release Candidate
Docker images are available:

```
# EPP
docker pull ghcr.io/llm-d/llm-d-inference-scheduler:v0.7.0-rc.1
# sidecar
docker pull ghcr.io/llm-d/llm-d-routing-sidecar:v0.7.0-rc.1
```

## Highlights
  * Gateway API Inference Extension v1.4 (and Gateway API v1.5.1): standalone EPP, InferencePool / Helm / gRPC-related improvements, significant work in flow control and datalayer internals.
  * Initial EPD support (disaggregation profile, KV cache enablement).
  * Sidecar command line option handling has been cleaned up and simplified.
  * Numerous testing and developer experience improvements (e.g., new Makefile targets. elimination of CGO requirements, containerized builds, improved Podman support, automatic dev image built on every PR to main, etc.)


## What's Changed
  * fix(ci): update Trivy to v0.69.2 by [@pierDipi](https://github.com/pierDipi) in [#675](https://github.com/llm-d/llm-d-router/pull/675)
  * Allow sidecar server to reload TLS certificates by [@pierDipi](https://github.com/pierDipi) in [#607](https://github.com/llm-d/llm-d-router/pull/607)
  * Use trivy action for image scanning by [@elevran](https://github.com/elevran) in [#688](https://github.com/llm-d/llm-d-router/pull/688)
  * deps(go): bump the go-dependencies group with 7 updates by [@dependabot](https://github.com/dependabot)[bot] in [#692](https://github.com/llm-d/llm-d-router/pull/692)
  * feat(sidecar): simplify TLS command line options with StringSlice flags by [@gyliu513](https://github.com/gyliu513) in [#682](https://github.com/llm-d/llm-d-router/pull/682)
  * Fix terminolgy and add links to docs/archiecture.md by [@elevran](https://github.com/elevran) in [#695](https://github.com/llm-d/llm-d-router/pull/695)
  * replace map[string]bool with map[string]struct{} by [@roytman](https://github.com/roytman) in [#696](https://github.com/llm-d/llm-d-router/pull/696)
  * Add presubmit makefile target for local verification before pushing a PR by [@elevran](https://github.com/elevran) in [#687](https://github.com/llm-d/llm-d-router/pull/687)
  * Update Trivy scan to run newer version with explicit auth tokens by [@elevran](https://github.com/elevran) in [#698](https://github.com/llm-d/llm-d-router/pull/698)
  * Remove extra trivy params by [@elevran](https://github.com/elevran) in [#702](https://github.com/llm-d/llm-d-router/pull/702)
  * fix: simplify InferencePool flag to namespace/name format by [@gyliu513](https://github.com/gyliu513) in [#685](https://github.com/llm-d/llm-d-router/pull/685)
  * Trivy complains of user-password mismatch by [@elevran](https://github.com/elevran) in [#704](https://github.com/llm-d/llm-d-router/pull/704)
  * fix(test): Add unit test for pd_prerequest.go by [@gyliu513](https://github.com/gyliu513) in [#706](https://github.com/llm-d/llm-d-router/pull/706)
  * Remove trivy cache and enable manual workflow dispatch by [@elevran](https://github.com/elevran) in [#713](https://github.com/llm-d/llm-d-router/pull/713)
  * initial E/PD extension of the sidecar by [@roytman](https://github.com/roytman) in [#643](https://github.com/llm-d/llm-d-router/pull/643)
  * Check for uniqueness of media URLs by [@roytman](https://github.com/roytman) in [#717](https://github.com/llm-d/llm-d-router/pull/717)
  * Move typo checking from tools makefile to main, under lint by [@elevran](https://github.com/elevran) in [#719](https://github.com/llm-d/llm-d-router/pull/719)
  * Rename EncoderPodsHeader according to other constants by [@roytman](https://github.com/roytman) in [#721](https://github.com/llm-d/llm-d-router/pull/721)
  * Implement Options pattern for sidecar proxy by [@Mohamedma96](https://github.com/Mohamedma96) in [#697](https://github.com/llm-d/llm-d-router/pull/697)
  * Rename common constants by [@roytman](https://github.com/roytman) in [#722](https://github.com/llm-d/llm-d-router/pull/722)
  * deps(actions): bump dorny/paths-filter from 3 to 4 by [@dependabot](https://github.com/dependabot)[bot] in [#723](https://github.com/llm-d/llm-d-router/pull/723)
  * Enable major version updates to gh actions by [@elevran](https://github.com/elevran) in [#714](https://github.com/llm-d/llm-d-router/pull/714)
  * NonCachedTokens defines the minimum number of non-cached tokens required by [@modassarrana89-new](https://github.com/modassarrana89-new) in [#691](https://github.com/llm-d/llm-d-router/pull/691)
  * Add external tokenizer PrepareData plugin and TokenizedPrompt scorer support by [@acardace](https://github.com/acardace) in [#694](https://github.com/llm-d/llm-d-router/pull/694)
  * Deprecate the workaround used to support vLLM Data Parallel on Istio 1.28 by [@shmuelk](https://github.com/shmuelk) in [#727](https://github.com/llm-d/llm-d-router/pull/727)
  * build: remove CGO dependency by migrating to pure-Go ZMQ by [@elevran](https://github.com/elevran) in [#728](https://github.com/llm-d/llm-d-router/pull/728)
  * deps(go): bump google.golang.org/grpc from 1.79.2 to 1.79.3 by [@dependabot](https://github.com/dependabot)[bot] in [#737](https://github.com/llm-d/llm-d-router/pull/737)
  * Use kubectl kustomize instead of standalone kustomize by [@elevran](https://github.com/elevran) in [#741](https://github.com/llm-d/llm-d-router/pull/741)
  * Add idle pod config to active-request-scorer by [@dagrayvid](https://github.com/dagrayvid) in [#669](https://github.com/llm-d/llm-d-router/pull/669)
  * [build] Optimize docker build (local and CICD) by [@elevran](https://github.com/elevran) in [#740](https://github.com/llm-d/llm-d-router/pull/740)
  * feat: speculative indexing for PrecisePrefixCacheScorer by [@bongwoobak](https://github.com/bongwoobak) in [#659](https://github.com/llm-d/llm-d-router/pull/659)
  * Remove obsolete hashBlockSize by [@roytman](https://github.com/roytman) in [#748](https://github.com/llm-d/llm-d-router/pull/748)
  * feat: add optional Prometheus monitoring to Kind dev environment by [@hexfusion](https://github.com/hexfusion) in [#742](https://github.com/llm-d/llm-d-router/pull/742)
  * fix: support podman-docker in e2e tests and Makefile by [@hexfusion](https://github.com/hexfusion) in [#730](https://github.com/llm-d/llm-d-router/pull/730)
  * Remove UDS tokenizer image build from inference scheduler repo by [@elevran](https://github.com/elevran) in [#739](https://github.com/llm-d/llm-d-router/pull/739)
  * deps(go): bump the kubernetes group with 5 updates by [@dependabot](https://github.com/dependabot)[bot] in [#754](https://github.com/llm-d/llm-d-router/pull/754)
  * test: add disruption e2e tests for scheduler failure scenarios by [@hexfusion](https://github.com/hexfusion) in [#735](https://github.com/llm-d/llm-d-router/pull/735)
  * Unified Disaggregate Handler by [@roytman](https://github.com/roytman) in [#732](https://github.com/llm-d/llm-d-router/pull/732)
  * [build] Add test coverage reporting by [@elevran](https://github.com/elevran) in [#749](https://github.com/llm-d/llm-d-router/pull/749)
  * [refactor] Unify sidecar `Config` and `Options` by [@elevran](https://github.com/elevran) in [#751](https://github.com/llm-d/llm-d-router/pull/751)
  * [cicd[ Make coverage comparison optional by [@elevran](https://github.com/elevran) in https://github.com/[/pull/762](https://github.com/llm-d/llm-d-router/pull/762)
  * A temporary fix to use the previous simulator image by [@roytman](https://github.com/roytman) in [#766](https://github.com/llm-d/llm-d-router/pull/766)
  * Combine EncodeHeaderHandler and PrefillHeaderHandler into a single DisaggHeadersHandler by [@roytman](https://github.com/roytman) in [#758](https://github.com/llm-d/llm-d-router/pull/758)
  * Prevent mismatch between new and deprecated APIs by [@roytman](https://github.com/roytman) in [#756](https://github.com/llm-d/llm-d-router/pull/756)
  * Revert IGW import to 1.4.0 and update tokenizer plugin accordingly by [@vMaroon](https://github.com/vMaroon) in [#770](https://github.com/llm-d/llm-d-router/pull/770)
  * fix(test): Increase test coverage for prefix_based_pd_decider.go by [@gyliu513](https://github.com/gyliu513) in [#715](https://github.com/llm-d/llm-d-router/pull/715)
  * Run unit, integration tests and builds in a container by [@acardace](https://github.com/acardace) in [#521](https://github.com/llm-d/llm-d-router/pull/521)
  * update Makefile to run e2e tests locally with Podman by [@roytman](https://github.com/roytman) in [#775](https://github.com/llm-d/llm-d-router/pull/775)
  * implement context-length-aware plugin (scorer/filter) by [@vMaroon](https://github.com/vMaroon) in [#550](https://github.com/llm-d/llm-d-router/pull/550)
  * Build images use Go builtin cross compilation by [@shmuelk](https://github.com/shmuelk) in [#776](https://github.com/llm-d/llm-d-router/pull/776)
  * feat: bump llm-d-kv-cache for MM-aware prefix-cache routing by [@vMaroon](https://github.com/vMaroon) in [#772](https://github.com/llm-d/llm-d-router/pull/772)
  * change simulator version to v0.8.1 by [@mayabar](https://github.com/mayabar) in [#773](https://github.com/llm-d/llm-d-router/pull/773)
  * [fix] [cicd] Add missing newline at end of OWNERS, make script more robust for missing newlines by [@elevran](https://github.com/elevran) in [#779](https://github.com/llm-d/llm-d-router/pull/779)
  * [fix] Added custom LevelEncoder for sidecar by [@elevran](https://github.com/elevran) in [#778](https://github.com/llm-d/llm-d-router/pull/778)
  * Update GW API to v1.5.1 by [@elevran](https://github.com/elevran) in [#780](https://github.com/llm-d/llm-d-router/pull/780)
  * [bug] fix multimodal content dropped in prefix cache scoring and tokenizer chat rendering by [@vMaroon](https://github.com/vMaroon) [#781](https://github.com/llm-d/llm-d-router/pull/781)
  * Add E2E_KEEP_CLUSTER_ON_FAILURE env variable by [@roytman](https://github.com/roytman) in [#782](https://github.com/llm-d/llm-d-router/pull/782)


## New Contributors
  * [@roytman](https://github.com/roytman) made their first contribution in [#696](https://github.com/llm-d/llm-d-router/pull/696)
  * [@Mohamedma96](https://github.com/Mohamedma96) made their first contribution in [#697](https://github.com/llm-d/llm-d-router/pull/697)
  * [@modassarrana89-new](https://github.com/modassarrana89-new) made their first contribution in [#691](https://github.com/llm-d/llm-d-router/pull/691)
  * [@dagrayvid](https://github.com/dagrayvid) made their first contribution in [#669](https://github.com/llm-d/llm-d-router/pull/669)
  * [@hexfusion](https://github.com/hexfusion) made their first contribution in [#742](https://github.com/llm-d/llm-d-router/pull/742)


**Full Changelog** : [`v0.6.1...v0.7.0-rc.1`](https://github.com/llm-d/llm-d-router/compare/v0.6.1...v0.7.0-rc.1)
### Contributors
hexfusion, roytman, and 12 other contributors 
Assets 2
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases).
Previous [1](https://github.com/llm-d/llm-d-router/releases?page=1) [2](https://github.com/llm-d/llm-d-router/releases?page=2) [3](https://github.com/llm-d/llm-d-router/releases?page=3) [4](https://github.com/llm-d/llm-d-router/releases?page=4) [Next](https://github.com/llm-d/llm-d-router/releases?page=2)
Previous [Next](https://github.com/llm-d/llm-d-router/releases?page=2)
You can’t perform that action at this time. 
