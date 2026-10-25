---
title: Release v0.11.0 · llm-d/llm-d-router · GitHub
id: release-v0110-llm-dllm-d-router-github
tags:
- internal-llm-inference-stacks-7f0c90
- llm-d
- epp-architecture
- falsification
created: '2026-10-10T03:10:20.151281Z'
updated: '2026-10-10T14:52:17.271174Z'
source: https://github.com/llm-d/llm-d-router/releases/latest
source_domain: github.com
fetched_at: '2026-10-10T03:10:20.130434Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'llm-d-router v0.11.0 release notes (27 Sep 2026): ships NEW scoring and
  filtering plugins plus a structured Helm pluginsConfig surface — the primary evidence
  falsifying a maintainer''s earlier warning that llm-d might remove its EPP plugin
  extensibility model.'
---

[Skip to content](https://github.com/llm-d/llm-d-router/releases/tag/v0.11.0#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/releases/tag/v0.11.0) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/releases/tag/v0.11.0) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/releases/tag/v0.11.0) to refresh your session. Dismiss alert
# v0.11.0
Compare
#  Choose a tag to compare 
## Sorry, something went wrong.
Filter 
Loading
## Sorry, something went wrong.
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases/tag/v0.11.0).
## No results found
[github-actions](https://github.com/apps/github-actions) released this  27 Sep 05:28 
· [ 199 commits ](https://github.com/llm-d/llm-d-router/compare/v0.11.0...main) to main since this release 
[ v0.11.0  ](https://github.com/llm-d/llm-d-router/tree/v0.11.0)
This tag was signed with the committer’s **verified signature**. 
[vMaroon](https://github.com/vMaroon) Maroon Ayoub 
GPG key ID: C5EDD1D9C324538C
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
[ `a5cbe60`](https://github.com/llm-d/llm-d-router/commit/a5cbe600ebade00cf3e9885beaf2bfacddeabce1)
This commit was signed with the committer’s **verified signature**. 
[vMaroon](https://github.com/vMaroon) Maroon Ayoub 
GPG key ID: C5EDD1D9C324538C
Verified 
[Learn about vigilant mode](https://docs.github.com/github/authenticating-to-github/displaying-verification-statuses-for-all-of-your-commits). 
Release v0.11.0
## Highlights
  * **Flow control for P/D:** Saturation gating is stage-aware. Prefill and decode are measured separately and admission gates on the busier stage ([#2221](https://github.com/llm-d/llm-d-router/pull/2221)). The beta `max-saturation-detector` combines several detectors and gates on the highest one, and each child can be limited to prefill or decode ([#2827](https://github.com/llm-d/llm-d-router/pull/2827), [#2926](https://github.com/llm-d/llm-d-router/pull/2926)). Queue TTLs can be set per priority band, clients can ask for a shorter one with `x-llm-d-inference-ttl`, and `noEndpointRequestTTL` bounds the wait while the pool is empty ([#2649](https://github.com/llm-d/llm-d-router/pull/2649), [#2284](https://github.com/llm-d/llm-d-router/pull/2284)). New capacity utilization gauges and an `x-llm-d-flow-queue-duration-ms` response header ([#2184](https://github.com/llm-d/llm-d-router/pull/2184), [#2490](https://github.com/llm-d/llm-d-router/pull/2490)).
  * **Scheduling and routing:** The new `latency-observation-scorer` ranks endpoints by TTFT measured from real responses ([#2371](https://github.com/llm-d/llm-d-router/pull/2371)). In P/D, load is split by role: input tokens count against prefill and estimated output tokens against decode ([#2396](https://github.com/llm-d/llm-d-router/pull/2396)). The `context-length-aware` scorer can route cached follow-ups by the prefill work that remains ([#2559](https://github.com/llm-d/llm-d-router/pull/2559)). Endpoints tied for the top score take turns instead of one pod getting the whole burst ([#2515](https://github.com/llm-d/llm-d-router/pull/2515), [#2709](https://github.com/llm-d/llm-d-router/pull/2709)). New `turn-priority` fairness strategy and endpoint attribute weight scorer ([#2116](https://github.com/llm-d/llm-d-router/pull/2116), [#2204](https://github.com/llm-d/llm-d-router/pull/2204)).
  * **Tokenization and render:** `token-producer` renders Anthropic Messages natively through vLLM `/v1/messages/render`, and falls back to the old conversion when the endpoint is missing ([#2707](https://github.com/llm-d/llm-d-router/pull/2707)). It can balance render calls across discovered endpoints ([#2701](https://github.com/llm-d/llm-d-router/pull/2701)), and supports HTTPS, mTLS and render endpoints started with `--api-key` ([#2664](https://github.com/llm-d/llm-d-router/pull/2664), [#2678](https://github.com/llm-d/llm-d-router/pull/2678), [#2763](https://github.com/llm-d/llm-d-router/pull/2763)). Opt-in xxhash block hashing makes precise prefix hashing about 8x cheaper ([#2237](https://github.com/llm-d/llm-d-router/pull/2237)).
  * **API coverage:** SGLang `/generate` ([#2391](https://github.com/llm-d/llm-d-router/pull/2391)), OpenAI text-to-speech ([#2484](https://github.com/llm-d/llm-d-router/pull/2484)), `/v1/images/edits` ([#2562](https://github.com/llm-d/llm-d-router/pull/2562)), and pre-tokenized batch prompts on `/v1/completions` ([#2523](https://github.com/llm-d/llm-d-router/pull/2523)). Paths no parser claims are forwarded to the model server by default through `passthrough-parser` ([#2507](https://github.com/llm-d/llm-d-router/pull/2507)). ATOM is a built-in engine type ([#2476](https://github.com/llm-d/llm-d-router/pull/2476)).
  * **Coordinator and DisaggregatedSet:** The coordinator exposes Prometheus metrics, optionally over HTTPS, forwards unregistered paths such as `/v1/models` to a decode pod, and adds an optional async-broker step ([#2435](https://github.com/llm-d/llm-d-router/pull/2435), [#2752](https://github.com/llm-d/llm-d-router/pull/2752), [#2357](https://github.com/llm-d/llm-d-router/pull/2357), [#2325](https://github.com/llm-d/llm-d-router/pull/2325)). EPP replicas agree on DisaggregatedSet revision decisions, and the coordinator keeps the same decision across encode, prefill and decode ([#2570](https://github.com/llm-d/llm-d-router/pull/2570), [#2571](https://github.com/llm-d/llm-d-router/pull/2571), [#2572](https://github.com/llm-d/llm-d-router/pull/2572), [#2576](https://github.com/llm-d/llm-d-router/pull/2576)).
  * **Operations:** A `--feature-gates` flag enables gates without restating the whole config ([#2388](https://github.com/llm-d/llm-d-router/pull/2388)). Helm adds Envoy priority routing for EPP failover in standalone mode, `router.epp.deploymentStrategy`, structured `router.epp.pluginsConfig` and `router.imagePullSecrets`, and restarts the EPP pod when its ConfigMaps change ([#2228](https://github.com/llm-d/llm-d-router/pull/2228), [#2603](https://github.com/llm-d/llm-d-router/pull/2603), [#2620](https://github.com/llm-d/llm-d-router/pull/2620), [#2766](https://github.com/llm-d/llm-d-router/pull/2766), [#2460](https://github.com/llm-d/llm-d-router/pull/2460)).
  * **Observability:** Logs carry `trace_id` and `span_id`. New spans cover tokenization and the KV-event pipeline, and trace context is read from ext_proc gRPC metadata so traces connect end to end ([#2271](https://github.com/llm-d/llm-d-router/pull/2271), [#2272](https://github.com/llm-d/llm-d-router/pull/2272), [#2830](https://github.com/llm-d/llm-d-router/pull/2830)). Spans carry `service.name` and the pod resource attributes ([#2298](https://github.com/llm-d/llm-d-router/pull/2298)).
  * **Fixes:** `golang.org/x/crypto` v0.55.0 for [CVE-2026-56854](https://github.com/advisories/GHSA-gjhq-gjfw-99mq "CVE-2026-56854") ([#2739](https://github.com/llm-d/llm-d-router/pull/2739)). Stale precise prefix-cache state after endpoints are removed ([#2824](https://github.com/llm-d/llm-d-router/pull/2824)). The PD Multi Tier decode leg starting before prefill stored its KV ([#2451](https://github.com/llm-d/llm-d-router/pull/2451)). `min_tokens` breaking the prefill leg ([#2593](https://github.com/llm-d/llm-d-router/pull/2593)). MoRI-IO concurrent dispatch ([#2816](https://github.com/llm-d/llm-d-router/pull/2816)). Published standalone charts pointing agentgateway at a missing image tag ([#2732](https://github.com/llm-d/llm-d-router/pull/2732)).
  * **Precise prefix cache:** LoRA requests keep `BlockStored` extra keys aligned with request-side keys, restoring precise cache matches ([#2929](https://github.com/llm-d/llm-d-router/pull/2929)).


## Upgrade Steps & Deprecations
This release removes many items deprecated in v0.7.0 and v0.9.0. Configurations that still use them fail at startup.
  * **Removed plugins:** `pd-profile-handler`, use `disagg-profile-handler` with the nested `profiles` and `deciders` format ([#2509](https://github.com/llm-d/llm-d-router/pull/2509)). `precise-prefix-cache-scorer`, use `precise-prefix-cache-producer` plus `prefix-cache-scorer` with `prefixMatchInfoProducerName` ([#2844](https://github.com/llm-d/llm-d-router/pull/2844)). `by-label` and `by-label-selector`, use `label-selector-filter` or the role filters ([#2865](https://github.com/llm-d/llm-d-router/pull/2865)). `tokenizer`, use `token-producer` ([#2869](https://github.com/llm-d/llm-d-router/pull/2869)).
  * **Removed config:** The top-level `saturationDetector` and `parser` fields, use `flowControl.saturationDetector` and `requestHandler.parsers` ([#2838](https://github.com/llm-d/llm-d-router/pull/2838)). The UDS tokenizer: `udsTokenizerConfig` and `indexerConfig.tokenizersPoolConfig` are rejected, set `vllm.url` on `token-producer` instead ([#2512](https://github.com/llm-d/llm-d-router/pull/2512)). `lasDecayFactor` on `program-aware-fairness`, decay uses `lasHalfLifeSeconds` only ([#2120](https://github.com/llm-d/llm-d-router/pull/2120)).
  * **Pod label:** `llm-d.ai/role: both` no longer matches. Use `prefill-decode` ([#2509](https://github.com/llm-d/llm-d-router/pull/2509)).
  * **Removed metrics:** The legacy `inference_extension_*`, `llm_d_inference_scheduler_*`, core request and inference-pool, and `inference_objective` predicted-latency families. Use the `llm_d_epp_*` series. Three `inference_extension_*` series stay for the workload-variant autoscaler ([#2826](https://github.com/llm-d/llm-d-router/pull/2826), [#2675](https://github.com/llm-d/llm-d-router/pull/2675), [#2821](https://github.com/llm-d/llm-d-router/pull/2821), [#2509](https://github.com/llm-d/llm-d-router/pull/2509)).
  * **Changed metrics:** KV-event metrics use the `llm_d_epp` prefix instead of `llm_d_router_epp` ([#2385](https://github.com/llm-d/llm-d-router/pull/2385)). `kv_cache_index_lookup_hits_total` and `kv_cache_index_max_pod_hit_count_total` count only the longest contiguous prefix on one pod, so they read lower. Re-baseline dashboards and alerts ([#2400](https://github.com/llm-d/llm-d-router/pull/2400), [#2680](https://github.com/llm-d/llm-d-router/pull/2680)). An empty-pool expiry is labelled `EvictedNoEndpoints` instead of `EvictedTTL` ([#2284](https://github.com/llm-d/llm-d-router/pull/2284)).
  * **Coordinator TLS on by default:** The inference listener serves HTTPS. Use https, or set `--secure-coordinator=false` to keep plaintext ([#2805](https://github.com/llm-d/llm-d-router/pull/2805)).
  * **JSON logs:** Field names follow OpenTelemetry. Collectors that match on `msg` need to match `body` ([#2643](https://github.com/llm-d/llm-d-router/pull/2643)).
  * **Tracing:** With tracing on and `OTEL_TRACES_EXPORTER` unset, spans go to OTLP instead of stdout ([#2566](https://github.com/llm-d/llm-d-router/pull/2566)).
  * **Helm:** Charts default to Prometheus Operator monitoring for every gateway provider. GKE users who relied on implicit GMP monitoring must configure it explicitly ([#2374](https://github.com/llm-d/llm-d-router/pull/2374)). The Prometheus ClusterRole and binding are created only when metrics authentication is on ([#2798](https://github.com/llm-d/llm-d-router/pull/2798)).
  * **Endpoint attribute plugins:** `endpoint-attribute-filter` and `endpoint-attribute-scorer` take a separate `producer` parameter, and the combined `Name/Producer` form is rejected. Out-of-tree plugins that use `AttributeMap` need updating ([#2190](https://github.com/llm-d/llm-d-router/pull/2190)).
  * **Prefix scoring:** Device tiers without a configured weight score 0 instead of 1.0 ([#2809](https://github.com/llm-d/llm-d-router/pull/2809)).
  * **Stricter startup checks:** Optional dependency cycles and invalid plugin ordering fail startup ([#2721](https://github.com/llm-d/llm-d-router/pull/2721)). Non-positive `--metrics-staleness-threshold` and `--refresh-prometheus-metrics-interval` are rejected ([#2914](https://github.com/llm-d/llm-d-router/pull/2914)).
  * **Conditional decode:** The `Prefer: if-available` 412 gate moved into `prefix-based-pd-decider`. Without that plugin, such requests get 412. Set `nonCachedTokens: 1` to keep the old behavior ([#2335](https://github.com/llm-d/llm-d-router/pull/2335)).
  * **P/D failures:** When a required prefill stage finds no endpoint, the request fails instead of running prefill on the decode pod ([#2521](https://github.com/llm-d/llm-d-router/pull/2521)). No eligible decode endpoint returns 503 `rejected-no-endpoints` ([#2631](https://github.com/llm-d/llm-d-router/pull/2631)).
  * **Deprecated:** `dataLayer.discovery.pluginRef`, use `dataLayer.discovery.endpoints.pluginRef`; old configs are migrated automatically ([#2434](https://github.com/llm-d/llm-d-router/pull/2434)). `kvcache.KVBlockScorer` and `kvcache.NewTracedScorer` ([#2669](https://github.com/llm-d/llm-d-router/pull/2669)).
  * **Latency predictor:** Charts from this release use the `v0.8.0` latency predictor images, the latest published predictor release.


## Known Issues
  * Endpoint score emission (`--emit-endpoint-scores`) emits nothing under the deprecated `data-parallel-profile-handler`.
  * With KV-event replay disabled, an inference engine that restarts at the same endpoint can retain stale precise prefix-cache state until eviction. Follow [#2853](https://github.com/llm-d/llm-d-router/issues/2853) and [#2854](https://github.com/llm-d/llm-d-router/pull/2854).


## What's Changed
**Click to expand**
  * fix(kvevents): drop the LoRA name from BlockStored extra keys by [@wseaton](https://github.com/wseaton) in [#2929](https://github.com/llm-d/llm-d-router/pull/2929)
  * test(flowcontrol): eviction dynamics benchmark by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2107](https://github.com/llm-d/llm-d-router/pull/2107)
  * refactor(datalayer): key AttributeMap by DataKey instead of string by [@satyamg1620](https://github.com/satyamg1620) in [#2190](https://github.com/llm-d/llm-d-router/pull/2190)
  * Filter prefix cache events by cache group kind by [@vMaroon](https://github.com/vMaroon) in [#2363](https://github.com/llm-d/llm-d-router/pull/2363)
  * docs: add release-note fragment for PR [#2190](https://github.com/llm-d/llm-d-router/pull/2190) by @llm-d-router-release-notes[bot] in [#2367](https://github.com/llm-d/llm-d-router/pull/2367)
  * fix(charts): emit leading document separators in monitoring partials by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2368](https://github.com/llm-d/llm-d-router/pull/2368)
  * Bumped Go version directive from 1.26.5 to 1.26.6. by [@ahg-g](https://github.com/ahg-g) in [#2386](https://github.com/llm-d/llm-d-router/pull/2386)
  * Unify the metrics subsystem on llm_d_epp by [@ahg-g](https://github.com/ahg-g) in [#2385](https://github.com/llm-d/llm-d-router/pull/2385)
  * fix(flowcontrol): prevent aggregate stats underflow on flow GC by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2382](https://github.com/llm-d/llm-d-router/pull/2382)
  * docs: add release-note fragment for PR [#2385](https://github.com/llm-d/llm-d-router/pull/2385) by @llm-d-router-release-notes[bot] in [#2387](https://github.com/llm-d/llm-d-router/pull/2387)
  * feat: EPP peer discovery infrastructure by [@evacchi](https://github.com/evacchi) in [#2341](https://github.com/llm-d/llm-d-router/pull/2341)
  * make metricsLoggingInterval values match the comment by [@yamt](https://github.com/yamt) in [#2405](https://github.com/llm-d/llm-d-router/pull/2405)
  * fix(tokenizer): translate Anthropic tool calls and thinking for render by [@mschulist](https://github.com/mschulist) in [#2389](https://github.com/llm-d/llm-d-router/pull/2389)
  * Bound and resume KV-event replay by [@vMaroon](https://github.com/vMaroon) in [#2410](https://github.com/llm-d/llm-d-router/pull/2410)
  * fix(options): validate refresh-metrics-interval 50ms floor at startup by [@noalimoy](https://github.com/noalimoy) in [#2394](https://github.com/llm-d/llm-d-router/pull/2394)
  * deps(actions): bump github/codeql-action/upload-sarif from 4.37.6 to 4.37.7 in the github-actions group by [@dependabot](https://github.com/dependabot)[bot] in [#2418](https://github.com/llm-d/llm-d-router/pull/2418)
  * deps(go): bump google.golang.org/protobuf from 1.36.12-0.20260120151049-f2248ac996af to 1.36.12 in the go-dependencies group by [@dependabot](https://github.com/dependabot)[bot] in [#2417](https://github.com/llm-d/llm-d-router/pull/2417)
  * docs: add release-note fragment for PR [#2389](https://github.com/llm-d/llm-d-router/pull/2389) by @llm-d-router-release-notes[bot] in [#2413](https://github.com/llm-d/llm-d-router/pull/2413)
  * test(tokenizer): cover Anthropic tool render request by [@vMaroon](https://github.com/vMaroon) in [#2412](https://github.com/llm-d/llm-d-router/pull/2412)
  * perf(flowcontrol): index active queues so dispatch scans O(active) flows by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2120](https://github.com/llm-d/llm-d-router/pull/2120)
  * feat(flowcontrol): split queue-wait budget by unavailability regime by [@satyamg1620](https://github.com/satyamg1620) in [#2284](https://github.com/llm-d/llm-d-router/pull/2284)
  * docs: add release-note fragment for PR [#2120](https://github.com/llm-d/llm-d-router/pull/2120) by @llm-d-router-release-notes[bot] in [#2436](https://github.com/llm-d/llm-d-router/pull/2436)
  * perf(epp): reduce streaming parser allocations by [@muchengl](https://github.com/muchengl) in [#2425](https://github.com/llm-d/llm-d-router/pull/2425)
  * fix(epp): merge streaming usage per field instead of replacing it by [@himanshu1573](https://github.com/himanshu1573) in [#2437](https://github.com/llm-d/llm-d-router/pull/2437)
  * feat(epp): record a termination cause on end-of-stream response records by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2444](https://github.com/llm-d/llm-d-router/pull/2444)
  * docs: move Shmuel Kallner to emeritus, drop from CODEOWNERS by [@elevran](https://github.com/elevran) in [#2449](https://github.com/llm-d/llm-d-router/pull/2449)
  * [ci]: Add path based PR labeling by [@hickeyma](https://github.com/hickeyma) in [#2281](https://github.com/llm-d/llm-d-router/pull/2281)
  * feat(epp): expose a streamed-event count on the response record by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2446](https://github.com/llm-d/llm-d-router/pull/2446)
  * docs: assemble release notes for v0.10.0 by @llm-d-router-release-notes[bot] in [#2424](https://github.com/llm-d/llm-d-router/pull/2424)
  * docs: add release-note fragment for PR [#2437](https://github.com/llm-d/llm-d-router/pull/2437) by @llm-d-router-release-notes[bot] in [#2452](https://github.com/llm-d/llm-d-router/pull/2452)
  * docs: add release-note fragment for PR [#2444](https://github.com/llm-d/llm-d-router/pull/2444) by @llm-d-router-release-notes[bot] in [#2453](https://github.com/llm-d/llm-d-router/pull/2453)
  * docs: add release-note fragment for PR [#2446](https://github.com/llm-d/llm-d-router/pull/2446) by @llm-d-router-release-notes[bot] in [#2461](https://github.com/llm-d/llm-d-router/pull/2461)
  * feat(flowcontrol): add per-band and global capacity utilization gauges by [@sudoalok](https://github.com/sudoalok) in [#2184](https://github.com/llm-d/llm-d-router/pull/2184)
  * feat(telemetry): align kvcache OTel scope and span names with router standard by [@Alex-ai-future](https://github.com/Alex-ai-future) in [#2164](https://github.com/llm-d/llm-d-router/pull/2164)
  * feat(kvcache): add opt-in xxhash hashing to the KV-block token processor by [@vMaroon](https://github.com/vMaroon) in [#2237](https://github.com/llm-d/llm-d-router/pull/2237)
  * docs: add prose rules for model-drafted text to AGENTS.md by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2420](https://github.com/llm-d/llm-d-router/pull/2420)
  * Skip body re-marshal/replace when no model-name rewrite occurred by [@elevran](https://github.com/elevran) in [#2318](https://github.com/llm-d/llm-d-router/pull/2318)
  * feat(epp): add sglanghttp parser for /generate by [@NaomiEisen](https://github.com/NaomiEisen) in [#2391](https://github.com/llm-d/llm-d-router/pull/2391)
  * docs: add release-note fragment for PR [#2391](https://github.com/llm-d/llm-d-router/pull/2391) by @llm-d-router-release-notes[bot] in [#2467](https://github.com/llm-d/llm-d-router/pull/2467)
  * docs: add release-note fragment for PR [#2237](https://github.com/llm-d/llm-d-router/pull/2237) by @llm-d-router-release-notes[bot] in [#2466](https://github.com/llm-d/llm-d-router/pull/2466)
  * fix: Make TokenizedPrompt.MultiModalFeatures per-prompt by [@albertoperdomo2](https://github.com/albertoperdomo2) in [#1633](https://github.com/llm-d/llm-d-router/pull/1633)
  * refactor(epp): unexport RequestContext protocol and response state by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2457](https://github.com/llm-d/llm-d-router/pull/2457)
  * feat(datalayer): record more OpenAI fields on the models attribute by [@zdtsw](https://github.com/zdtsw) in [#2479](https://github.com/llm-d/llm-d-router/pull/2479)
  * fix(kvcache): report index hit metrics as contiguous prefix chains by [@nilig](https://github.com/nilig) in [#2400](https://github.com/llm-d/llm-d-router/pull/2400)
  * docs(epp): fix stale links in pkg/epp/README.md by [@varad-ahirwadkar](https://github.com/varad-ahirwadkar) in [#2454](https://github.com/llm-d/llm-d-router/pull/2454)
  * [Flow Control] Stage-Aware Saturation Gating for disaggregated P/D deployments by [@loicmarchal](https://github.com/loicmarchal) in [#2221](https://github.com/llm-d/llm-d-router/pull/2221)
  * perf(epp): remove per-scrape allocations in the metrics extractor by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2473](https://github.com/llm-d/llm-d-router/pull/2473)
  * cleanup(sidecar,logging): share the level encoder and use named verbosity constants by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2474](https://github.com/llm-d/llm-d-router/pull/2474)
  * docs: add release-note fragment for PR [#2221](https://github.com/llm-d/llm-d-router/pull/2221) by @llm-d-router-release-notes[bot] in [#2497](https://github.com/llm-d/llm-d-router/pull/2497)
  * docs: add release-note fragment for PR [#2400](https://github.com/llm-d/llm-d-router/pull/2400) by @llm-d-router-release-notes[bot] in [#2496](https://github.com/llm-d/llm-d-router/pull/2496)
  * fix(chart): avoid inferring GMP from GKE gateway provider by [@Alex-ai-future](https://github.com/Alex-ai-future) in [#2374](https://github.com/llm-d/llm-d-router/pull/2374)
  * tracing: configure SDK explicitly instead of mutating process env by [@satyamg1620](https://github.com/satyamg1620) in [#2298](https://github.com/llm-d/llm-d-router/pull/2298)
  * fix(prefix-scorer): aggregate missing match logs by [@nilig](https://github.com/nilig) in [#2404](https://github.com/llm-d/llm-d-router/pull/2404)
  * docs: add release-note fragment for PR [#2404](https://github.com/llm-d/llm-d-router/pull/2404) by @llm-d-router-release-notes[bot] in [#2502](https://github.com/llm-d/llm-d-router/pull/2502)
  * docs: add release-note fragment for PR [#2298](https://github.com/llm-d/llm-d-router/pull/2298) by @llm-d-router-release-notes[bot] in [#2499](https://github.com/llm-d/llm-d-router/pull/2499)
  * docs: add release-note fragment for PR [#2374](https://github.com/llm-d/llm-d-router/pull/2374) by @llm-d-router-release-notes[bot] in [#2498](https://github.com/llm-d/llm-d-router/pull/2498)
  * feat(scorer): migrate five legacy Consumes scorers to ConsumerPlugin by [@elevran](https://github.com/elevran) in [#2355](https://github.com/llm-d/llm-d-router/pull/2355)
  * fix(scheduling): return typed capacity rejection when filters drain candidates by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2462](https://github.com/llm-d/llm-d-router/pull/2462)
  * docs: add release-note fragment for PR [#2184](https://github.com/llm-d/llm-d-router/pull/2184) by @llm-d-router-release-notes[bot] in [#2464](https://github.com/llm-d/llm-d-router/pull/2464)
  * docs: add release-note fragment for PR [#2462](https://github.com/llm-d/llm-d-router/pull/2462) by @llm-d-router-release-notes[bot] in [#2504](https://github.com/llm-d/llm-d-router/pull/2504)
  * perf(epp): derive Scope allowed-key sets once at datalayer init by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2471](https://github.com/llm-d/llm-d-router/pull/2471)
  * perf(epp): guard hot-path log and span construction in scheduler, director, and handlers by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2472](https://github.com/llm-d/llm-d-router/pull/2472)
  * Add passthrough-parser to the default parser list by [@zetxqx](https://github.com/zetxqx) in [#2507](https://github.com/llm-d/llm-d-router/pull/2507)
  * docs: add release-note fragment for PR [#2507](https://github.com/llm-d/llm-d-router/pull/2507) by @llm-d-router-release-notes[bot] in [#2508](https://github.com/llm-d/llm-d-router/pull/2508)
  * Add an outcome metric for the affinity filter plugin by [@mathias-kende](https://github.com/mathias-kende) in [#2494](https://github.com/llm-d/llm-d-router/pull/2494)
  * feat(epp): move conditional-decode gate into prefix-based-pd-decider by [@dmitripikus](https://github.com/dmitripikus) in [#2335](https://github.com/llm-d/llm-d-router/pull/2335)
  * Output length aware TokenLoadScorer by [@DolevAdas](https://github.com/DolevAdas) in [#2309](https://github.com/llm-d/llm-d-router/pull/2309)
  * docs: add release-note fragment for PR [#2494](https://github.com/llm-d/llm-d-router/pull/2494) by @llm-d-router-release-notes[bot] in [#2526](https://github.com/llm-d/llm-d-router/pull/2526)
  * feat(epp): add x-llm-d-flow-queue-duration-ms response header by [@jtechapps](https://github.com/jtechapps) in [#2490](https://github.com/llm-d/llm-d-router/pull/2490)
  * deps(actions): bump github/codeql-action/upload-sarif from 4.37.7 to 4.37.8 in the github-actions group by [@dependabot](https://github.com/dependabot)[bot] in [#2535](https://github.com/llm-d/llm-d-router/pull/2535)
  * docs: add release-note fragment for PR [#2490](https://github.com/llm-d/llm-d-router/pull/2490) by @llm-d-router-release-notes[bot] in [#2538](https://github.com/llm-d/llm-d-router/pull/2538)
  * docs: add release-note fragment for PR [#2335](https://github.com/llm-d/llm-d-router/pull/2335) by @llm-d-router-release-notes[bot] in [#2527](https://github.com/llm-d/llm-d-router/pull/2527)
  * docs: add release-note fragment for PR [#2309](https://github.com/llm-d/llm-d-router/pull/2309) by @llm-d-router-release-notes[bot] in [#2528](https://github.com/llm-d/llm-d-router/pull/2528)
  * deps(go): bump the go-dependencies group with 2 updates by [@dependabot](https://github.com/dependabot)[bot] in [#2532](https://github.com/llm-d/llm-d-router/pull/2532)
  * deps(docker): bump golang from 1.26.6 to 1.26.7 by [@dependabot](https://github.com/dependabot)[bot] in [#2533](https://github.com/llm-d/llm-d-router/pull/2533)
  * deps(go): bump the kubernetes group with 6 updates by [@dependabot](https://github.com/dependabot)[bot] in [#2534](https://github.com/llm-d/llm-d-router/pull/2534)
  * epp: remove code deprecated since v0.7.0 by [@roytman](https://github.com/roytman) in [#2509](https://github.com/llm-d/llm-d-router/pull/2509)
  * docs: add release-note fragment for PR [#2509](https://github.com/llm-d/llm-d-router/pull/2509) by @llm-d-router-release-notes[bot] in [#2542](https://github.com/llm-d/llm-d-router/pull/2542)
  * fix(epp): fail P/D requests when a required prefill stage finds no endpoint by [@elinacse](https://github.com/elinacse) in [#2521](https://github.com/llm-d/llm-d-router/pull/2521)
  * fix(epp): classify non-OK gRPC trailers as errors by [@yuanhuizang-cloud](https://github.com/yuanhuizang-cloud) in [#2517](https://github.com/llm-d/llm-d-router/pull/2517)
  * Preserve JSON number precision during request repackaging by [@yankay](https://github.com/yankay) in [#2519](https://github.com/llm-d/llm-d-router/pull/2519)
  * feat(epp): add --feature-gates flag by [@abhishekkarki](https://github.com/abhishekkarki) in [#2388](https://github.com/llm-d/llm-d-router/pull/2388)
  * feat: Support array<array> prompt in /v1/completions by [@albertoperdomo2](https://github.com/albertoperdomo2) in [#2523](https://github.com/llm-d/llm-d-router/pull/2523)
  * docs: add release-note fragment for PR [#2388](https://github.com/llm-d/llm-d-router/pull/2388) by @llm-d-router-release-notes[bot] in [#2545](https://github.com/llm-d/llm-d-router/pull/2545)
  * docs: add release-note fragment for PR [#2164](https://github.com/llm-d/llm-d-router/pull/2164) by @llm-d-router-release-notes[bot] in [#2465](https://github.com/llm-d/llm-d-router/pull/2465)
  * docs: add release-note fragment for PR [#2523](https://github.com/llm-d/llm-d-router/pull/2523) by @llm-d-router-release-notes[bot] in [#2546](https://github.com/llm-d/llm-d-router/pull/2546)
  * config: consolidate discovery plugins under a single discovery section (follow up [#2341](https://github.com/llm-d/llm-d-router/pull/2341)) by [@pazqo](https://github.com/pazqo) in [#2434](https://github.com/llm-d/llm-d-router/pull/2434)
  * tracing: correlate logs with traces at request entry points by [@satyamg1620](https://github.com/satyamg1620) in [#2271](https://github.com/llm-d/llm-d-router/pull/2271)
  * feat: auto-tune approx prefix cache block size from prefix_match_unit by [@RishabhSaini](https://github.com/RishabhSaini) in [#2455](https://github.com/llm-d/llm-d-router/pull/2455)
  * refactor(flowcontrol): read capacity counters where they live by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2456](https://github.com/llm-d/llm-d-router/pull/2456)
  * docs: add release-note fragment for PR [#2434](https://github.com/llm-d/llm-d-router/pull/2434) by @llm-d-router-release-notes[bot] in [#2551](https://github.com/llm-d/llm-d-router/pull/2551)
  * docs: add release-note fragment for PR [#2271](https://github.com/llm-d/llm-d-router/pull/2271) by @llm-d-router-release-notes[bot] in [#2552](https://github.com/llm-d/llm-d-router/pull/2552)
  * feat(plugin-state): Allow customizing staleness threshold by [@asharkhan3101](https://github.com/asharkhan3101) in [#2138](https://github.com/llm-d/llm-d-router/pull/2138)
  * test(e2e): warm up before disruption in streaming-request spec by [@elevran](https://github.com/elevran) in [#2378](https://github.com/llm-d/llm-d-router/pull/2378)
  * docs: add release-note fragment for PR [#2284](https://github.com/llm-d/llm-d-router/pull/2284) by @llm-d-router-release-notes[bot] in [#2439](https://github.com/llm-d/llm-d-router/pull/2439)
  * tracing: support standard sampler values and validate the argument by [@satyamg1620](https://github.com/satyamg1620) in [#2550](https://github.com/llm-d/llm-d-router/pull/2550)
  * fix(dev): remove stale external tokenizer config by [@yuanhuizang-cloud](https://github.com/yuanhuizang-cloud) in [#2491](https://github.com/llm-d/llm-d-router/pull/2491)
  * Add endpoint-attribute parity coverage and a benchmark config ([#2059](https://github.com/llm-d/llm-d-router/issues/2059) item 1) by [@MicheleCampi](https://github.com/MicheleCampi) in [#2393](https://github.com/llm-d/llm-d-router/pull/2393)
  * tracing: default the trace exporter to otlp instead of console by [@satyamg1620](https://github.com/satyamg1620) in [#2566](https://github.com/llm-d/llm-d-router/pull/2566)
  * feat(telemetry): finish dropping component prefixes from EPP scoring and P/D proxy spans by [@gyliu513](https://github.com/gyliu513) in [#2558](https://github.com/llm-d/llm-d-router/pull/2558)
  * feat(telemetry): drop misleading gateway. prefix from EPP root spans by [@gyliu513](https://github.com/gyliu513) in [#2557](https://github.com/llm-d/llm-d-router/pull/2557)
  * docs: add release-note fragment for PR [#2455](https://github.com/llm-d/llm-d-router/pull/2455) by @llm-d-router-release-notes[bot] in [#2553](https://github.com/llm-d/llm-d-router/pull/2553)
  * feat: parse multipart /v1/images/edits requests in OpenAI parser by [@zetxqx](https://github.com/zetxqx) in [#2562](https://github.com/llm-d/llm-d-router/pull/2562)
  * docs: add release-note fragment for PR [#2562](https://github.com/llm-d/llm-d-router/pull/2562) by @llm-d-router-release-notes[bot] in [#2569](https://github.com/llm-d/llm-d-router/pull/2569)
  * [KVEvents] add dp rank to KV events by [@sagearc](https://github.com/sagearc) in [#2379](https://github.com/llm-d/llm-d-router/pull/2379)
  * docs: add release-note fragment for PR [#2138](https://github.com/llm-d/llm-d-router/pull/2138) by @llm-d-router-release-notes[bot] in [#2555](https://github.com/llm-d/llm-d-router/pull/2555)
  * Raise e2e EXISTS_TIMEOUT to 90s in CI to avoid namespace teardown flakes by [@zetxqx](https://github.com/zetxqx) in [#2582](https://github.com/llm-d/llm-d-router/pull/2582)
  * feat(coordinator): proxy unregistered paths to the decode profile by [@dmitripikus](https://github.com/dmitripikus) in [#2357](https://github.com/llm-d/llm-d-router/pull/2357)
  * feat(epp): make the fairness_id metric label cap configurable by [@mathias-kende](https://github.com/mathias-kende) in [#2495](https://github.com/llm-d/llm-d-router/pull/2495)
  * pdd: Allow PD decider to select cache producer by [@wez470](https://github.com/wez470) in [#2560](https://github.com/llm-d/llm-d-router/pull/2560)
  * docs: add release-note fragment for PR [#2560](https://github.com/llm-d/llm-d-router/pull/2560) by @llm-d-router-release-notes[bot] in [#2592](https://github.com/llm-d/llm-d-router/pull/2592)
  * docs: add release-note fragment for PR [#2357](https://github.com/llm-d/llm-d-router/pull/2357) by @llm-d-router-release-notes[bot] in [#2583](https://github.com/llm-d/llm-d-router/pull/2583)
  * docs: add release-note fragment for PR [#2495](https://github.com/llm-d/llm-d-router/pull/2495) by @llm-d-router-release-notes[bot] in [#2584](https://github.com/llm-d/llm-d-router/pull/2584)
  * Fix Data race in TestFlowControlAdmissionController_Admit by [@ahg-g](https://github.com/ahg-g) in [#2579](https://github.com/llm-d/llm-d-router/pull/2579)
  * fix: prune inflight metric series on endpoint deletion by [@MisterCool](https://github.com/MisterCool) in [#2577](https://github.com/llm-d/llm-d-router/pull/2577)
  * refactor(flowcontrol): derive QueueOutcome from the finalization error by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2458](https://github.com/llm-d/llm-d-router/pull/2458)
  * Add prefixMatchInfoProducerName to inflightload README.md by [@nt591](https://github.com/nt591) in [#2599](https://github.com/llm-d/llm-d-router/pull/2599)
  * feat(coordinator): expose Prometheus metrics by [@dmitripikus](https://github.com/dmitripikus) in [#2435](https://github.com/llm-d/llm-d-router/pull/2435)
  * docs: add release-note fragment for PR [#2435](https://github.com/llm-d/llm-d-router/pull/2435) by @llm-d-router-release-notes[bot] in [#2615](https://github.com/llm-d/llm-d-router/pull/2615)
  * [disagg-sidecar] Fix cached token usage for non-streaming P/D responses by [@learner0810](https://github.com/learner0810) in [#2372](https://github.com/llm-d/llm-d-router/pull/2372)
  * Add Bob as request lifecycle code owner by [@vMaroon](https://github.com/vMaroon) in [#2613](https://github.com/llm-d/llm-d-router/pull/2613)
  * Add Nili as P2P routing code owner by [@vMaroon](https://github.com/vMaroon) in [#2614](https://github.com/llm-d/llm-d-router/pull/2614)
  * check pluginType for defaults metrics source by [@zetxqx](https://github.com/zetxqx) in [#2618](https://github.com/llm-d/llm-d-router/pull/2618)
  * fix(epp): preserve endpoint attributes in data-parallel profile by [@yuanhuizang-cloud](https://github.com/yuanhuizang-cloud) in [#2563](https://github.com/llm-d/llm-d-router/pull/2563)
  * perf(epp): optimize HTTP token ID request parsing by [@yankay](https://github.com/yankay) in [#2522](https://github.com/llm-d/llm-d-router/pull/2522)
  * docs: add release-note fragment for PR [#2372](https://github.com/llm-d/llm-d-router/pull/2372) by @llm-d-router-release-notes[bot] in [#2624](https://github.com/llm-d/llm-d-router/pull/2624)
  * Fix: Scope Endpoint Metric Cleanup by Namespace and Producer by [@weizhoublue](https://github.com/weizhoublue) in [#2602](https://github.com/llm-d/llm-d-router/pull/2602)
  * fix(metrics): read SGLang cache capacity from dedicated gauges by [@satyamg1620](https://github.com/satyamg1620) in [#2619](https://github.com/llm-d/llm-d-router/pull/2619)
  * llm-d observability: align OpenTelemetry GenAI semconv and refactor telemetry usages by [@PlateauGao](https://github.com/PlateauGao) in [#2503](https://github.com/llm-d/llm-d-router/pull/2503)
  * docs: add release-note fragment for PR [#2458](https://github.com/llm-d/llm-d-router/pull/2458) by @llm-d-router-release-notes[bot] in [#2597](https://github.com/llm-d/llm-d-router/pull/2597)
  * deps(go): bump google.golang.org/grpc from 1.82.1 to 1.82.2 in the go-dependencies group by [@dependabot](https://github.com/dependabot)[bot] in [#2632](https://github.com/llm-d/llm-d-router/pull/2632)
  * docs: add release-note fragment for PR [#2619](https://github.com/llm-d/llm-d-router/pull/2619) by @llm-d-router-release-notes[bot] in [#2635](https://github.com/llm-d/llm-d-router/pull/2635)
  * feat(epp): add atomic cross-replica coordination API [Safe DisaggSet 1/4] by [@hasB4K](https://github.com/hasB4K) in [#2570](https://github.com/llm-d/llm-d-router/pull/2570)
  * fix(epp): use net.JoinHostPort for fallback random endpoints by [@liuyuan10](https://github.com/liuyuan10) in [#2607](https://github.com/llm-d/llm-d-router/pull/2607)
  * docs: add release-note fragment for PR [#2570](https://github.com/llm-d/llm-d-router/pull/2570) by @llm-d-router-release-notes[bot] in [#2646](https://github.com/llm-d/llm-d-router/pull/2646)
  * docs: add release-note fragment for PR [#2607](https://github.com/llm-d/llm-d-router/pull/2607) by @llm-d-router-release-notes[bot] in [#2647](https://github.com/llm-d/llm-d-router/pull/2647)
  * docs: document that endpoint discovery honours custom pod readinessGates by [@almogtavor](https://github.com/almogtavor) in [#2604](https://github.com/llm-d/llm-d-router/pull/2604)
  * deps(go): bump google.golang.org/grpc from 1.82.2 to 1.83.1 by [@dependabot](https://github.com/dependabot)[bot] in [#2660](https://github.com/llm-d/llm-d-router/pull/2660)
  * fix: IPv6 address in remaining endpoint by [@zdtsw](https://github.com/zdtsw) in [#2658](https://github.com/llm-d/llm-d-router/pull/2658)
  * fix(epp): reject negative maxPrefixBlocksToMatch config by [@Iceber](https://github.com/Iceber) in [#2656](https://github.com/llm-d/llm-d-router/pull/2656)
  * feat: add OpenTelemetry spans for tokenization and KV-cache event processing by [@satyamg1620](https://github.com/satyamg1620) in [#2272](https://github.com/llm-d/llm-d-router/pull/2272)
  * feat(epp): support OpenAI text-to-speech requests by [@xiaojun-zhang](https://github.com/xiaojun-zhang) in [#2484](https://github.com/llm-d/llm-d-router/pull/2484)
  * tracing: support OTLP/HTTP as an alternative to the gRPC exporter by [@satyamg1620](https://github.com/satyamg1620) in [#2573](https://github.com/llm-d/llm-d-router/pull/2573)
  * docs: add release-note fragment for PR [#2272](https://github.com/llm-d/llm-d-router/pull/2272) by @llm-d-router-release-notes[bot] in [#2662](https://github.com/llm-d/llm-d-router/pull/2662)
  * Fix routing imbalance: deterministic tie-breaking for equal-score endpoints by [@bbenshab](https://github.com/bbenshab) in [#2515](https://github.com/llm-d/llm-d-router/pull/2515)
  * chore(epp): remove dead nil check and fix comment typo in approx-prefix by [@Iceber](https://github.com/Iceber) in [#2655](https://github.com/llm-d/llm-d-router/pull/2655)
  * Enable Envoy priorities for high availability for standalone mode by [@rlakhtakia](https://github.com/rlakhtakia) in [#2228](https://github.com/llm-d/llm-d-router/pull/2228)
  * fix(chart): allow overriding the EPP Deployment strategy so multi-replica EPP can satisfy helm --wait by [@almogtavor](https://github.com/almogtavor) in [#2603](https://github.com/llm-d/llm-d-router/pull/2603)
  * docs: add release-note fragment for PR [#2228](https://github.com/llm-d/llm-d-router/pull/2228) by @llm-d-router-release-notes[bot] in [#2681](https://github.com/llm-d/llm-d-router/pull/2681)
  * approximateprefix: change block hash insertion order to tail-first by [@Iceber](https://github.com/Iceber) in [#2661](https://github.com/llm-d/llm-d-router/pull/2661)
  * docs: add release-note fragment for PR [#2603](https://github.com/llm-d/llm-d-router/pull/2603) by @llm-d-router-release-notes[bot] in [#2683](https://github.com/llm-d/llm-d-router/pull/2683)
  * fix(kind): forward Prometheus host port by [@Alex-ai-future](https://github.com/Alex-ai-future) in [#2654](https://github.com/llm-d/llm-d-router/pull/2654)
  * feat: add custom TLS support for vLLM render endpoint by [@zdtsw](https://github.com/zdtsw) in [#2664](https://github.com/llm-d/llm-d-router/pull/2664)
  * fix(epp): support vLLM render endpoints started with --api-key by [@Iceber](https://github.com/Iceber) in [#2678](https://github.com/llm-d/llm-d-router/pull/2678)
  * docs: add release-note fragment for PR [#2664](https://github.com/llm-d/llm-d-router/pull/2664) by @llm-d-router-release-notes[bot] in [#2688](https://github.com/llm-d/llm-d-router/pull/2688)
  * deps(actions): bump the github-actions group with 2 updates by [@dependabot](https://github.com/dependabot)[bot] in [#2634](https://github.com/llm-d/llm-d-router/pull/2634)
  * fix(CrossReplicaSyncer): disentangle CrossReplicaSyncing interval from main ticket in PoolingDispatcher by [@Lucas-Fernandes-Martins](https://github.com/Lucas-Fernandes-Martins) in [#2310](https://github.com/llm-d/llm-d-router/pull/2310)
  * docs(epp): add affinity filter composition requirements by [@LukeAVanDrie](https://github.com/LukeAVanDrie) in [#2651](https://github.com/llm-d/llm-d-router/pull/2651)
  * docs: add release-note fragment for PR [#2678](https://github.com/llm-d/llm-d-router/pull/2678) by @llm-d-router-release-notes[bot] in [#2689](https://github.com/llm-d/llm-d-router/pull/2689)
  * docs: add a repository contributing guide covering issue claiming by [@marzmesas](https://github.com/marzmesas) in [#2666](https://github.com/llm-d/llm-d-router/pull/2666)
  * Fix(helm): Enforce Boolean Types for Priority Routing and Health Checking by [@weizhoublue](https://github.com/weizhoublue) in [#2686](https://github.com/llm-d/llm-d-router/pull/2686)
  * feat(epp): order all requestcontrol hooks by data-dependency DAG by [@chethanuk](https://github.com/chethanuk) in [#2359](https://github.com/llm-d/llm-d-router/pull/2359)
  * refactor(kvcache): fold prefix matching into one accumulator by [@nilig](https://github.com/nilig) in [#2669](https://github.com/llm-d/llm-d-router/pull/2669)
  * Remove deprecated UDS/legacy tokenizer code and drop llm-d-kv-cache by [@satyamg1620](https://github.com/satyamg1620) in [#2512](https://github.com/llm-d/llm-d-router/pull/2512)
  * docs: use blockSizeTokens in tokenProcessorConfig samples ([#641](https://github.com/llm-d/llm-d-router/issues/641)) by [@chethanuk](https://github.com/chethanuk) in [#2705](https://github.com/llm-d/llm-d-router/pull/2705)
  * fix(sidecar): await prefill before dispatching PD Multi Tier decode leg by [@nilig](https://github.com/nilig) in [#2451](https://github.com/llm-d/llm-d-router/pull/2451)
  * docs: add release-note fragment for PR [#2451](https://github.com/llm-d/llm-d-router/pull/2451) by @llm-d-router-release-notes[bot] in [#2711](https://github.com/llm-d/llm-d-router/pull/2711)
  * docs: add release-note fragment for PR [#2512](https://github.com/llm-d/llm-d-router/pull/2512) by @llm-d-router-release-notes[bot] in [#2703](https://github.com/llm-d/llm-d-router/pull/2703)
  * docs: add release-note fragment for PR [#2669](https://github.com/llm-d/llm-d-router/pull/2669) by @llm-d-router-release-notes[bot] in [#2700](https://github.com/llm-d/llm-d-router/pull/2700)
  * docs: add release-note fragment for PR [#2359](https://github.com/llm-d/llm-d-router/pull/2359) by @llm-d-router-release-notes[bot] in [#2691](https://github.com/llm-d/llm-d-router/pull/2691)
  * [ci]: Run required checks on merge_group events by [@hickeyma](https://github.com/hickeyma) in [#2682](https://github.com/llm-d/llm-d-router/pull/2682)
  * [ci]: Sign published container images with actions/attest by [@hickeyma](https://github.com/hickeyma) in [#2285](https://github.com/llm-d/llm-d-router/pull/2285)
  * Validate prefix cache affinity threshold bounds by [@googs1025](https://github.com/googs1025) in [#2720](https://github.com/llm-d/llm-d-router/pull/2720)
  * fix(epp): register default producer for UncachedRequestTokens by [@ambermmxChen](https://github.com/ambermmxChen) in [#2407](https://github.com/llm-d/llm-d-router/pull/2407)
  * docs: drop links to deleted IGW guide pages ([#1863](https://github.com/llm-d/llm-d-router/issues/1863)) by [@chethanuk](https://github.com/chethanuk) in [#2704](https://github.com/llm-d/llm-d-router/pull/2704)
  * Point sim-epp token-producer at the render Service by [@tonibofarull](https://github.com/tonibofarull) in [#2426](https://github.com/llm-d/llm-d-router/pull/2426)
  * docs: add release-note fragment for PR [#2407](https://github.com/llm-d/llm-d-router/pull/2407) by @llm-d-router-release-notes[bot] in [#2728](https://github.com/llm-d/llm-d-router/pull/2728)
  * ci(trivy): bump scanner to v0.74.0 by [@elevran](https://github.com/elevran) in [#2718](https://github.com/llm-d/llm-d-router/pull/2718)
  * Enforce ordering for configured optional dependencies by [@vMaroon](https://github.com/vMaroon) in [#2721](https://github.com/llm-d/llm-d-router/pull/2721)
  * Update Kubernetes libraries to 1.36 by [@hasB4K](https://github.com/hasB4K) in [#2733](https://github.com/llm-d/llm-d-router/pull/2733)
  * fix: omit unexecuted pipeline step timings by [@learner0810](https://github.com/learner0810) in [#2726](https://github.com/llm-d/llm-d-router/pull/2726)
  * bump golang.org/x/crypto@v0.53.0 to 0.55.0 due to a critical vulnerability by [@roytman](https://github.com/roytman) in [#2739](https://github.com/llm-d/llm-d-router/pull/2739)
  * test(sidecar): add audio and video multimodal test coverage by [@dmitripikus](https://github.com/dmitripikus) in [#2639](https://github.com/llm-d/llm-d-router/pull/2639)
  * docs: add coordinator metrics specification by [@revit13](https://github.com/revit13) in [#2277](https://github.com/llm-d/llm-d-router/pull/2277)
  * fix(request): strip min_tokens in single-token request priming by [@roytman](https://github.com/roytman) in [#2593](https://github.com/llm-d/llm-d-router/pull/2593)
  * deps(go): bump sigs.k8s.io/structured-merge-diff/v6 from 6.4.0 to 6.4.2 in the kubernetes group by [@dependabot](https://github.com/dependabot)[bot] in [#2736](https://github.com/llm-d/llm-d-router/pull/2736)
  * deps(actions): bump crate-ci/typos from 1.50.0 to 1.50.1 in the github-actions group by [@dependabot](https://github.com/dependabot)[bot] in [#2737](https://github.com/llm-d/llm-d-router/pull/2737)
  * deps(docker): bump golang from 1.26.7 to 1.26.8 by [@dependabot](https://github.com/dependabot)[bot] in [#2734](https://github.com/llm-d/llm-d-router/pull/2734)
  * deps(go): bump the go-dependencies group across 1 directory with 3 updates by [@dependabot](https://github.com/dependabot)[bot] in [#2735](https://github.com/llm-d/llm-d-router/pull/2735)
  * docs: add release-note fragment for PR [#2593](https://github.com/llm-d/llm-d-router/pull/2593) by @llm-d-router-release-notes[bot] in [#2744](https://github.com/llm-d/llm-d-router/pull/2744)
  * docs: add release-note fragment for PR [#2739](https://github.com/llm-d/llm-d-router/pull/2739) by @llm-d-router-release-notes[bot] in [#2740](https://github.com/llm-d/llm-d-router/pull/2740)
  * feat(epp): gate DisaggregatedSet readiness on initial Pod snapshot by [@hasB4K](https://github.com/hasB4K) in [#2576](https://github.com/llm-d/llm-d-router/pull/2576)
  * docs: add release-note fragment for PR [#2576](https://github.com/llm-d/llm-d-router/pull/2576) by @llm-d-router-release-notes[bot] in [#2745](https://github.com/llm-d/llm-d-router/pull/2745)
  * ci: skip gosec SARIF upload on merge_group events by [@elevran](https://github.com/elevran) in [#2751](https://github.com/llm-d/llm-d-router/pull/2751)
  * feat(epp): coordinate DisaggregatedSet revision decisions [Safe DisaggSet 2/4] by [@hasB4K](https://github.com/hasB4K) in [#2571](https://github.com/llm-d/llm-d-router/pull/2571)
  * docs: add release-note fragment for PR [#2571](https://github.com/llm-d/llm-d-router/pull/2571) by @llm-d-router-release-notes[bot] in [#2756](https://github.com/llm-d/llm-d-router/pull/2756)
  * feat(epp): add latency-observation scorer and producer by [@Mohammad-nassar10](https://github.com/Mohammad-nassar10) in [#2371](https://github.com/llm-d/llm-d-router/pull/2371)
  * ci: add DCO check that runs on merge_group by [@elevran](https://github.com/elevran) in [#2750](https://github.com/llm-d/llm-d-router/pull/2750)
  * docs: add release-note fragment for PR [#2721](https://github.com/llm-d/llm-d-router/pull/2721) by @llm-d-router-release-notes[bot] in [#2730](https://github.com/llm-d/llm-d-router/pull/2730)
  * role-aware load split for P/D disaggregation by [@DolevAdas](https://github.com/DolevAdas) in [#2396](https://github.com/llm-d/llm-d-router/pull/2396)
  * fix(config): auto-inject saturation detector as scheduling filter by [@RishabhSaini](https://github.com/RishabhSaini) in [#1741](https://github.com/llm-d/llm-d-router/pull/1741)
  * docs: add release-note fragment for PR [#2396](https://github.com/llm-d/llm-d-router/pull/2396) by @llm-d-router-release-notes[bot] in [#2759](https://github.com/llm-d/llm-d-router/pull/2759)
  * docs: add release-note fragment for PR [#2371](https://github.com/llm-d/llm-d-router/pull/2371) by @llm-d-router-release-notes[bot] in [#2758](https://github.com/llm-d/llm-d-router/pull/2758)
  * fix: quote lychee glob and exclude auth-walled URLs by [@chethanuk](https://github.com/chethanuk) in [#2754](https://github.com/llm-d/llm-d-router/pull/2754)
  * Enable redundant-import-alias lint rule, remove existing hits by [@roytman](https://github.com/roytman) in [#2760](https://github.com/llm-d/llm-d-router/pull/2760)
  * [ci]: Attach SBOM to published container images by [@hickeyma](https://github.com/hickeyma) in [#2729](https://github.com/llm-d/llm-d-router/pull/2729)
  * docs: describe standalone service proxy mode by [@seanwestfall](https://github.com/seanwestfall) in [#2748](https://github.com/llm-d/llm-d-router/pull/2748)
  * fix(charts): roll the EPP pod when its ConfigMaps change by [@shimib](https://github.com/shimib) in [#2460](https://github.com/llm-d/llm-d-router/pull/2460)
  * feat(flowcontrol): support scoped request queue TTLs by [@nt591](https://github.com/nt591) in [#2649](https://github.com/llm-d/llm-d-router/pull/2649)
  * feat(flowcontrol): make stale-metrics scoring configurable by [@todayim](https://github.com/todayim) in [#2492](https://github.com/llm-d/llm-d-router/pull/2492)
  * perf(kvcache): match prefixes over an ordered index walk by [@nilig](https://github.com/nilig) in [#2680](https://github.com/llm-d/llm-d-router/pull/2680)
  * fix(docs): repair broken links surfaced by quoted lychee glob by [@Iceber](https://github.com/Iceber) in [#2764](https://github.com/llm-d/llm-d-router/pull/2764)
  * docs: add release-note fragment for PR [#2460](https://github.com/llm-d/llm-d-router/pull/2460) by @llm-d-router-release-notes[bot] in [#2768](https://github.com/llm-d/llm-d-router/pull/2768)
  * docs: add release-note fragment for PR [#2649](https://github.com/llm-d/llm-d-router/pull/2649) by @llm-d-router-release-notes[bot] in [#2769](https://github.com/llm-d/llm-d-router/pull/2769)
  * docs: add release-note fragment for PR [#2492](https://github.com/llm-d/llm-d-router/pull/2492) by @llm-d-router-release-notes[bot] in [#2770](https://github.com/llm-d/llm-d-router/pull/2770)
  * docs: add release-note fragment for PR [#2748](https://github.com/llm-d/llm-d-router/pull/2748) by @llm-d-router-release-notes[bot] in [#2767](https://github.com/llm-d/llm-d-router/pull/2767)
  * Add session affinity perf configurations and summary by [@jtechapps](https://github.com/jtechapps) in [#2217](https://github.com/llm-d/llm-d-router/pull/2217)
  * test(kvcache): release CostAware test resources by [@Alex-ai-future](https://github.com/Alex-ai-future) in [#2350](https://github.com/llm-d/llm-d-router/pull/2350)
  * feat(epp): add OTel span to prefix cache affinity filter decisions by [@sudoalok](https://github.com/sudoalok) in [#2594](https://github.com/llm-d/llm-d-router/pull/2594)
  * fix(sidecar): forward untouched request fields as raw JSON to keep key order by [@rebel-jinhwan](https://github.com/rebel-jinhwan) in [#2591](https://github.com/llm-d/llm-d-router/pull/2591)
  * fix(lora-affinity): correct ActiveModels docs and remove wrong capacity by [@zdtsw](https://github.com/zdtsw) in [#2625](https://github.com/llm-d/llm-d-router/pull/2625)
  * feat: emit OpenTelemetry structured JSON logs to stdout by [@den-rgb](https://github.com/den-rgb) in [#2643](https://github.com/llm-d/llm-d-router/pull/2643)
  * docs: add release-note fragment for PR [#2591](https://github.com/llm-d/llm-d-router/pull/2591) by @llm-d-router-release-notes[bot] in [#2772](https://github.com/llm-d/llm-d-router/pull/2772)
  * fix(cicd): move all lychee configuration to config file by [@elevran](https://github.com/elevran) in [#2776](https://github.com/llm-d/llm-d-router/pull/2776)
  * fix(epp): rotate only equal top-scoring endpoints in maxscore picker by [@quangh33](https://github.com/quangh33) in [#2709](https://github.com/llm-d/llm-d-router/pull/2709)
  * refactor(sidecar): extract shared concurrent prefill/decode dispatch helper by [@roytman](https://github.com/roytman) in [#2589](https://github.com/llm-d/llm-d-router/pull/2589)
  * docs: add release-note fragment for PR [#2643](https://github.com/llm-d/llm-d-router/pull/2643) by @llm-d-router-release-notes[bot] in [#2773](https://github.com/llm-d/llm-d-router/pull/2773)
  * Fix agentgateway image tag defaulting to the release tag by [@magic-peach](https://github.com/magic-peach) in [#2732](https://github.com/llm-d/llm-d-router/pull/2732)
  * docs(metrics): clarify metric sources and Prometheus series by [@zdtsw](https://github.com/zdtsw) in [#2747](https://github.com/llm-d/llm-d-router/pull/2747)
  * test(epp): add coverage for InferencePoolToEndpointPool by [@magic-peach](https://github.com/magic-peach) in [#2710](https://github.com/llm-d/llm-d-router/pull/2710)
  * fix(kvevents): wait for subscriber goroutines to exit on shutdown by [@quangh33](https://github.com/quangh33) in [#2717](https://github.com/llm-d/llm-d-router/pull/2717)
  * Validate KV cache utilization threshold bounds by [@googs1025](https://github.com/googs1025) in [#2753](https://github.com/llm-d/llm-d-router/pull/2753)
  * chore(chart): introduce imagePullSecrets to pull from private registries by [@PiePra](https://github.com/PiePra) in [#2766](https://github.com/llm-d/llm-d-router/pull/2766)
  * Rename completionRequest to body in sidecar proxy by [@cyclinder](https://github.com/cyclinder) in [#2775](https://github.com/llm-d/llm-d-router/pull/2775)
  * docs: add release-note fragment for PR [#2732](https://github.com/llm-d/llm-d-router/pull/2732) by @llm-d-router-release-notes[bot] in [#2781](https://github.com/llm-d/llm-d-router/pull/2781)
  * docs: add release-note fragment for PR [#2766](https://github.com/llm-d/llm-d-router/pull/2766) by @llm-d-router-release-notes[bot] in [#2785](https://github.com/llm-d/llm-d-router/pull/2785)
  * fix(docs): repair session affinity performance links by [@zdtsw](https://github.com/zdtsw) in [#2782](https://github.com/llm-d/llm-d-router/pull/2782)
  * test: validate CPU vLLM metrics for EPP by [@pazqo](https://github.com/pazqo) in [#2784](https://github.com/llm-d/llm-d-router/pull/2784)
  * metrics: remove core legacy metric families by [@JaredTan95](https://github.com/JaredTan95) in [#2675](https://github.com/llm-d/llm-d-router/pull/2675)
  * fix: preserve API type through encoder disaggregation by [@JaredTan95](https://github.com/JaredTan95) in [#2749](https://github.com/llm-d/llm-d-router/pull/2749)
  * docs: add release-note fragment for PR [#2675](https://github.com/llm-d/llm-d-router/pull/2675) by @llm-d-router-release-notes[bot] in [#2791](https://github.com/llm-d/llm-d-router/pull/2791)
  * feat(sidecar): add TLS security profile flags by [@andresllh](https://github.com/andresllh) in [#2676](https://github.com/llm-d/llm-d-router/pull/2676)
  * docs(epp): update maxscore picker doc for round-robin tie-breaking by [@quangh33](https://github.com/quangh33) in [#2789](https://github.com/llm-d/llm-d-router/pull/2789)
  * feat(chart): Gate Prometheus RBAC on authentication by [@learner0810](https://github.com/learner0810) in [#2798](https://github.com/llm-d/llm-d-router/pull/2798)
  * test(epp): wait for metrics delivery in endpoint scores test by [@Iceber](https://github.com/Iceber) in [#2797](https://github.com/llm-d/llm-d-router/pull/2797)
  * docs: add release-note fragment for PR [#2676](https://github.com/llm-d/llm-d-router/pull/2676) by @llm-d-router-release-notes[bot] in [#2795](https://github.com/llm-d/llm-d-router/pull/2795)
  * feat(coordinator): async-broker step serving OpenAI compatible enqueue, wait, and passthrough modes by [@BenjaminBraunDev](https://github.com/BenjaminBraunDev) in [#2325](https://github.com/llm-d/llm-d-router/pull/2325)
  * fix(scheduling): classify unavailable endpoints by [@nicole-lihui](https://github.com/nicole-lihui) in [#2631](https://github.com/llm-d/llm-d-router/pull/2631)
  * fix(kvcache): score unconfigured tiers zero and add storage tier weights by [@Iceber](https://github.com/Iceber) in [#2809](https://github.com/llm-d/llm-d-router/pull/2809)
  * llm-d observability: finish remaining OpenTelemetry GenAI semconv and telemetry refactoring by [@PlateauGao](https://github.com/PlateauGao) in [#2516](https://github.com/llm-d/llm-d-router/pull/2516)
  * docs: add release-note fragment for PR [#2325](https://github.com/llm-d/llm-d-router/pull/2325) by @llm-d-router-release-notes[bot] in [#2803](https://github.com/llm-d/llm-d-router/pull/2803)
  * docs: add release-note fragment for PR [#2631](https://github.com/llm-d/llm-d-router/pull/2631) by @llm-d-router-release-notes[bot] in [#2804](https://github.com/llm-d/llm-d-router/pull/2804)
  * docs: add release-note fragment for PR [#2809](https://github.com/llm-d/llm-d-router/pull/2809) by @llm-d-router-release-notes[bot] in [#2810](https://github.com/llm-d/llm-d-router/pull/2810)
  * fix(perf): honor tracing configuration and report scheduler p99 by [@0z5a](https://github.com/0z5a) in [#2725](https://github.com/llm-d/llm-d-router/pull/2725)
  * feat(datalayer): typed attribute slots and value-safety registry by [@elevran](https://github.com/elevran) in [#2352](https://github.com/llm-d/llm-d-router/pull/2352)
  * feat(chart): support vLLM Rust renderer flavor for tokenizer sidecar by [@Madhumasa84](https://github.com/Madhumasa84) in [#2657](https://github.com/llm-d/llm-d-router/pull/2657)
  * feat(metrics): add ATOM as a built-in engine by [@Jasen2201](https://github.com/Jasen2201) in [#2476](https://github.com/llm-d/llm-d-router/pull/2476)
  * perf(picker): guard debug logging to avoid hot-path reflection by [@mauriciopoppe](https://github.com/mauriciopoppe) in [#2806](https://github.com/llm-d/llm-d-router/pull/2806)
  * feature(epp): authenticate token-producer warmup probe via VLLM_API_KEY by [@Iceber](https://github.com/Iceber) in [#2763](https://github.com/llm-d/llm-d-router/pull/2763)
  * ci: assemble release notes once per tag instead of per PR merge by [@elevran](https://github.com/elevran) in [#2778](https://github.com/llm-d/llm-d-router/pull/2778)
  * feat(helm): support structured plugin config by [@hugosmoreira](https://github.com/hugosmoreira) in [#2620](https://github.com/llm-d/llm-d-router/pull/2620)
  * docs: add release-note fragment for PR [#2725](https://github.com/llm-d/llm-d-router/pull/2725) by @llm-d-router-release-notes[bot] in [#2811](https://github.com/llm-d/llm-d-router/pull/2811)
  * perf(tokenload): guard debug logging in TokenLoadScorer by [@ishwar170695](https://github.com/ishwar170695) in [#2819](https://github.com/llm-d/llm-d-router/pull/2819)
  * feat(sidecar+coordinator): add support for HTTPS /metrics by [@zdtsw](https://github.com/zdtsw) in [#2752](https://github.com/llm-d/llm-d-router/pull/2752)
  * docs: add release-note fragment for PR [#2763](https://github.com/llm-d/llm-d-router/pull/2763) by @llm-d-router-release-notes[bot] in [#2814](https://github.com/llm-d/llm-d-router/pull/2814)
  * chore: remove deprecated disagg header handlers by [@amirialy](https://github.com/amirialy) in [#2815](https://github.com/llm-d/llm-d-router/pull/2815)
  * metrics: remove predicted latency legacy metrics by [@JaredTan95](https://github.com/JaredTan95) in [#2821](https://github.com/llm-d/llm-d-router/pull/2821)
  * refactor(epp): add NewExtProcServerRunner and route every caller through it ([#1370](https://github.com/llm-d/llm-d-router/issues/1370)) by [@chethanuk](https://github.com/chethanuk) in [#2706](https://github.com/llm-d/llm-d-router/pull/2706)
  * fix(agentidentity): trim whitespace from extracted session headers by [@ishwar170695](https://github.com/ishwar170695) in [#2818](https://github.com/llm-d/llm-d-router/pull/2818)
  * epp: add turn-priority strategy to program-aware fairness by [@ruocco](https://github.com/ruocco) in [#2116](https://github.com/llm-d/llm-d-router/pull/2116)
  * refactor: unify API type handling across sidecar and coordinator by [@revit13](https://github.com/revit13) in [#2743](https://github.com/llm-d/llm-d-router/pull/2743)
  * Honor podLabelSelector for KV-event subscriptions by [@vMaroon](https://github.com/vMaroon) in [#2445](https://github.com/llm-d/llm-d-router/pull/2445)
  * apix: remove deprecated EPPConfig top-level saturationDetector and parser by [@zdtsw](https://github.com/zdtsw) in [#2838](https://github.com/llm-d/llm-d-router/pull/2838)
  * chore(epp): remove deprecated precise-prefix-cache-scorer plugin by [@zdtsw](https://github.com/zdtsw) in [#2844](https://github.com/llm-d/llm-d-router/pull/2844)
  * metrics: remove deprecated legacy metric series by [@JaredTan95](https://github.com/JaredTan95) in [#2826](https://github.com/llm-d/llm-d-router/pull/2826)
  * Broaden the link check exclusion pattern to cover the whole `make` documentation tree by [@roytman](https://github.com/roytman) in [#2863](https://github.com/llm-d/llm-d-router/pull/2863)
  * chore(epp): remove deprecated by-label filter plugins by [@zdtsw](https://github.com/zdtsw) in [#2865](https://github.com/llm-d/llm-d-router/pull/2865)
  * Pass native vLLM render requests through without content conversion by [@vMaroon](https://github.com/vMaroon) in [#2707](https://github.com/llm-d/llm-d-router/pull/2707)
  * test(flowcontrol): stop background GC racing hand-driven GC in band test by [@heliubj18](https://github.com/heliubj18) in [#2845](https://github.com/llm-d/llm-d-router/pull/2845)
  * chore(epp): remove deprecated tokenizer plugin by [@zdtsw](https://github.com/zdtsw) in [#2869](https://github.com/llm-d/llm-d-router/pull/2869)
  * fix(kvevents): wait for old subscriber to exit on endpoint change by [@quangh33](https://github.com/quangh33) in [#2719](https://github.com/llm-d/llm-d-router/pull/2719)
  * feat(flowcontrol): fail open in concurrency detector filter when all endpoints are filtered out by [@jtechapps](https://github.com/jtechapps) in [#2883](https://github.com/llm-d/llm-d-router/pull/2883)
  * Add discovery-backed vLLM render balancing by [@vMaroon](https://github.com/vMaroon) in [#2701](https://github.com/llm-d/llm-d-router/pull/2701)
  * coordinator: remove unused "tokens" field from chat-completions requests by [@roytman](https://github.com/roytman) in [#2842](https://github.com/llm-d/llm-d-router/pull/2842)
  * fix(coordinator): log pipeline step names with their durations by [@etai2404](https://github.com/etai2404) in [#2882](https://github.com/llm-d/llm-d-router/pull/2882)
  * fix(kvevents): stop the replay test buffer from dying on a client hangup by [@elevran](https://github.com/elevran) in [#2860](https://github.com/llm-d/llm-d-router/pull/2860)
  * ci: retire release-notes.d fragment pipeline remnants by [@elevran](https://github.com/elevran) in [#2868](https://github.com/llm-d/llm-d-router/pull/2868)
  * fix(kvevents): validate kvEventsConfig to prevent panics by [@Iceber](https://github.com/Iceber) in [#2885](https://github.com/llm-d/llm-d-router/pull/2885)
  * Document memory sizing for the in-memory KV index cap by [@magic-peach](https://github.com/magic-peach) in [#2880](https://github.com/llm-d/llm-d-router/pull/2880)
  * RHOAIENG-83718 | standardize llm-d OTel stdout logs by [@den-rgb](https://github.com/den-rgb) in [#2813](https://github.com/llm-d/llm-d-router/pull/2813)
  * chore: replace body-part terms in docs, comments, and code by [@zdtsw](https://github.com/zdtsw) in [#2712](https://github.com/llm-d/llm-d-router/pull/2712)
  * test(e2e): deploy router through the standalone Helm chart by [@delete-cloud](https://github.com/delete-cloud) in [#2832](https://github.com/llm-d/llm-d-router/pull/2832)
  * fix(epp): extract trace context from gRPC metadata by [@Phil-OSophy-42](https://github.com/Phil-OSophy-42) in [#2830](https://github.com/llm-d/llm-d-router/pull/2830)
  * fix(kvevents): Fix memory leak in dedup map for precise KV cache index by [@Lucas-Fernandes-Martins](https://github.com/Lucas-Fernandes-Martins) in [#2824](https://github.com/llm-d/llm-d-router/pull/2824)
  * feat(coordinator): share revision decisions across phases [Safe DisaggSet 4/4] by [@hasB4K](https://github.com/hasB4K) in [#2572](https://github.com/llm-d/llm-d-router/pull/2572)
  * Route cached follow-ups by remaining prefill work by [@nilig](https://github.com/nilig) in [#2559](https://github.com/llm-d/llm-d-router/pull/2559)
  * [doc] Add release verification guide for published images by [@hickeyma](https://github.com/hickeyma) in [#2787](https://github.com/llm-d/llm-d-router/pull/2787)
  * feat(coordinator): serve the inference listener over TLS by [@zdtsw](https://github.com/zdtsw) in [#2805](https://github.com/llm-d/llm-d-router/pull/2805)
  * chore: correct copyright attribution across migrated and llm-d-original files by [@elevran](https://github.com/elevran) in [#2450](https://github.com/llm-d/llm-d-router/pull/2450)
  * feat(scheduling): add endpoint attribute weight scorer by [@nicole-lihui](https://github.com/nicole-lihui) in [#2204](https://github.com/llm-d/llm-d-router/pull/2204)
  * ci: skip DCO and release-note checks for dependabot PRs by [@elevran](https://github.com/elevran) in [#2856](https://github.com/llm-d/llm-d-router/pull/2856)
  * Propagate EPP-resolved priority to native backend schedulers (SGLang/… by [@sywangyi](https://github.com/sywangyi) in [#2265](https://github.com/llm-d/llm-d-router/pull/2265)
  * fix(sidecar): send a pod-local DP rank on the MoRI-IO concurrent-dispatch path by [@shikamd123](https://github.com/shikamd123) in [#2816](https://github.com/llm-d/llm-d-router/pull/2816)
  * fix: run the release pipeline once per tag by [@apurv-1](https://github.com/apurv-1) in [#2847](https://github.com/llm-d/llm-d-router/pull/2847)
  * deps(actions): bump github/codeql-action/upload-sarif from 4.37.9 to 4.38.0 in the github-actions group across 1 directory by [@dependabot](https://github.com/dependabot)[bot] in [#2851](https://github.com/llm-d/llm-d-router/pull/2851)
  * docs(dataproducer): explain ProducerPlugin vs DataProducer by [@chethanuk](https://github.com/chethanuk) in [#2894](https://github.com/llm-d/llm-d-router/pull/2894)
  * deps(go): bump go.opentelemetry.io/otel/sdk from 1.44.0 to 1.45.0 by [@dependabot](https://github.com/dependabot)[bot] in [#2915](https://github.com/llm-d/llm-d-router/pull/2915)
  * fix(server): reject non-positive metrics timing flags by [@ptimizeroracle](https://github.com/ptimizeroracle) in [#2914](https://github.com/llm-d/llm-d-router/pull/2914)
  * deps(go): bump go.opentelemetry.io/otel/exporters/otlp/otlptrace from 1.44.0 to 1.45.0 by [@dependabot](https://github.com/dependabot)[bot] in [#2918](https://github.com/llm-d/llm-d-router/pull/2918)
  * [Flow Control] Support Multiple Detectors by [@robertgshaw2-redhat](https://github.com/robertgshaw2-redhat) in [#2827](https://github.com/llm-d/llm-d-router/pull/2827)
  * feat(flowcontrol): scope max-saturation-detector children to stages by [@vMaroon](https://github.com/vMaroon) in [#2926](https://github.com/llm-d/llm-d-router/pull/2926)


## New Contributors
  * [@yamt](https://github.com/yamt) made their first contribution in [#2405](https://github.com/llm-d/llm-d-router/pull/2405)
  * [@muchengl](https://github.com/muchengl) made their first contribution in [#2425](https://github.com/llm-d/llm-d-router/pull/2425)
  * [@himanshu1573](https://github.com/himanshu1573) made their first contribution in [#2437](https://github.com/llm-d/llm-d-router/pull/2437)
  * [@Alex-ai-future](https://github.com/Alex-ai-future) made their first contribution in [#2164](https://github.com/llm-d/llm-d-router/pull/2164)
  * [@NaomiEisen](https://github.com/NaomiEisen) made their first contribution in [#2391](https://github.com/llm-d/llm-d-router/pull/2391)
  * [@DolevAdas](https://github.com/DolevAdas) made their first contribution in [#2309](https://github.com/llm-d/llm-d-router/pull/2309)
  * [@yuanhuizang-cloud](https://github.com/yuanhuizang-cloud) made their first contribution in [#2517](https://github.com/llm-d/llm-d-router/pull/2517)
  * [@abhishekkarki](https://github.com/abhishekkarki) made their first contribution in [#2388](https://github.com/llm-d/llm-d-router/pull/2388)
  * [@pazqo](https://github.com/pazqo) made their first contribution in [#2434](https://github.com/llm-d/llm-d-router/pull/2434)
  * [@wez470](https://github.com/wez470) made their first contribution in [#2560](https://github.com/llm-d/llm-d-router/pull/2560)
  * [@MisterCool](https://github.com/MisterCool) made their first contribution in [#2577](https://github.com/llm-d/llm-d-router/pull/2577)
  * [@nt591](https://github.com/nt591) made their first contribution in [#2599](https://github.com/llm-d/llm-d-router/pull/2599)
  * [@PlateauGao](https://github.com/PlateauGao) made their first contribution in [#2503](https://github.com/llm-d/llm-d-router/pull/2503)
  * [@liuyuan10](https://github.com/liuyuan10) made their first contribution in [#2607](https://github.com/llm-d/llm-d-router/pull/2607)
  * [@almogtavor](https://github.com/almogtavor) made their first contribution in [#2604](https://github.com/llm-d/llm-d-router/pull/2604)
  * [@xiaojun-zhang](https://github.com/xiaojun-zhang) made their first contribution in [#2484](https://github.com/llm-d/llm-d-router/pull/2484)
  * [@bbenshab](https://github.com/bbenshab) made their first contribution in [#2515](https://github.com/llm-d/llm-d-router/pull/2515)
  * [@ambermmxChen](https://github.com/ambermmxChen) made their first contribution in [#2407](https://github.com/llm-d/llm-d-router/pull/2407)
  * [@tonibofarull](https://github.com/tonibofarull) made their first contribution in [#2426](https://github.com/llm-d/llm-d-router/pull/2426)
  * [@Mohammad-nassar10](https://github.com/Mohammad-nassar10) made their first contribution in [#2371](https://github.com/llm-d/llm-d-router/pull/2371)
  * [@seanwestfall](https://github.com/seanwestfall) made their first contribution in [#2748](https://github.com/llm-d/llm-d-router/pull/2748)
  * [@shimib](https://github.com/shimib) made their first contribution in [#2460](https://github.com/llm-d/llm-d-router/pull/2460)
  * [@todayim](https://github.com/todayim) made their first contribution in [#2492](https://github.com/llm-d/llm-d-router/pull/2492)
  * [@rebel-jinhwan](https://github.com/rebel-jinhwan) made their first contribution in [#2591](https://github.com/llm-d/llm-d-router/pull/2591)
  * [@den-rgb](https://github.com/den-rgb) made their first contribution in [#2643](https://github.com/llm-d/llm-d-router/pull/2643)
  * [@quangh33](https://github.com/quangh33) made their first contribution in [#2709](https://github.com/llm-d/llm-d-router/pull/2709)
  * [@magic-peach](https://github.com/magic-peach) made their first contribution in [#2732](https://github.com/llm-d/llm-d-router/pull/2732)
  * [@PiePra](https://github.com/PiePra) made their first contribution in [#2766](https://github.com/llm-d/llm-d-router/pull/2766)
  * [@cyclinder](https://github.com/cyclinder) made their first contribution in [#2775](https://github.com/llm-d/llm-d-router/pull/2775)
  * [@JaredTan95](https://github.com/JaredTan95) made their first contribution in [#2675](https://github.com/llm-d/llm-d-router/pull/2675)
  * [@0z5a](https://github.com/0z5a) made their first contribution in [#2725](https://github.com/llm-d/llm-d-router/pull/2725)
  * [@Jasen2201](https://github.com/Jasen2201) made their first contribution in [#2476](https://github.com/llm-d/llm-d-router/pull/2476)
  * [@mauriciopoppe](https://github.com/mauriciopoppe) made their first contribution in [#2806](https://github.com/llm-d/llm-d-router/pull/2806)
  * [@hugosmoreira](https://github.com/hugosmoreira) made their first contribution in [#2620](https://github.com/llm-d/llm-d-router/pull/2620)
  * [@amirialy](https://github.com/amirialy) made their first contribution in [#2815](https://github.com/llm-d/llm-d-router/pull/2815)
  * [@ruocco](https://github.com/ruocco) made their first contribution in [#2116](https://github.com/llm-d/llm-d-router/pull/2116)
  * [@heliubj18](https://github.com/heliubj18) made their first contribution in [#2845](https://github.com/llm-d/llm-d-router/pull/2845)
  * [@etai2404](https://github.com/etai2404) made their first contribution in [#2882](https://github.com/llm-d/llm-d-router/pull/2882)
  * [@delete-cloud](https://github.com/delete-cloud) made their first contribution in [#2832](https://github.com/llm-d/llm-d-router/pull/2832)
  * [@sywangyi](https://github.com/sywangyi) made their first contribution in [#2265](https://github.com/llm-d/llm-d-router/pull/2265)
  * [@apurv-1](https://github.com/apurv-1) made their first contribution in [#2847](https://github.com/llm-d/llm-d-router/pull/2847)
  * [@ptimizeroracle](https://github.com/ptimizeroracle) made their first contribution in [#2914](https://github.com/llm-d/llm-d-router/pull/2914)


**Full Changelog** : [`v0.10.0...v0.11.0`](https://github.com/llm-d/llm-d-router/compare/v0.10.0...v0.11.0)
### Contributors
evacchi, zdtsw, and 87 other contributors 
Assets 4
Loading
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/releases/tag/v0.11.0).
You can’t perform that action at this time. 
