---
title: precise-prefix-cache-routing.values.yaml
id: precise-prefix-cache-routingvaluesyaml
tags:
- llmd-kv-cache-routing-c665d5
- helm-values
- epp-config-reference
- ground-truth
created: '2026-09-14T13:03:21.998712Z'
updated: '2026-10-10T14:52:17.319078Z'
source: https://raw.githubusercontent.com/llm-d/llm-d/main/guides/precise-prefix-cache-routing/router/precise-prefix-cache-routing.values.yaml
source_domain: raw.githubusercontent.com
fetched_at: '2026-09-14T13:03:21.978149Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Actual shipped Helm values file (guides/precise-prefix-cache-routing/router/precise-prefix-cache-routing.values.yaml,
  llm-d/llm-d main) for the precise KV-cache-aware routing well-lit path. Uses precise-prefix-cache-producer
  (source comment: ''precise-prefix-cache-scorer is deprecated''), NOT approx-prefix-cache-producer,
  configured with blockSizeTokens:64, speculativeIndexing:true, kvEventsConfig{topicFilter:''kv@'',
  concurrency:8, discoverPods:true, podDiscoveryConfig{socketPort:5556, replaySocketPort:5559}}.
  Also wires token-producer (HTTP tokenization against a render Service, modelName
  Qwen/Qwen3-32B), endpoint-notification-source, inflight-load-producer, prefix-cache-affinity-filter
  (parameterized with prefixMatchInfoProducerName: precise-prefix-cache-producer and
  peakPrefillThroughput:15926), and token-load-scorer. schedulingProfiles default
  plugins: prefix-cache-affinity-filter + token-load-scorer -- same two-plugin scheduling
  profile shape as optimized-baseline, differing only in which prefix producer feeds
  the affinity filter. Forces router.epp.replicas:1 (in-flight load accounting is
  per-EPP-process, not shared across replicas) with a documented pflag workaround
  to keep ha-enable-leader-election off.'
utility_score: 14.0
---

```
## precise-prefix-cache-routing guide overrides for the router.
## Layer on top of: recipes/router/base.values.yaml
##
## Tokenization is served by the `../render/` Service, NOT the chart-injected
## EPP sidecar. The chart's `router.tokenizer` sidecar is off by default, so no
## override is needed here; the EPP `token-producer` plugin tokenizes prompts
## over HTTP against that Service (see its `vllm.url` below).
##
## That Service has no pods of its own — it fronts the vLLM model servers, which
## already expose `/v1/*/render`. Render capacity therefore scales with
## the model server fleet rather than with a separately-scheduled renderer pool,
## which keeps render latency more contained at higher QPS. Backends without
## vLLM's render endpoints (SGLang) apply `../render/standalone/` instead; it
## publishes the same Service name, so this `vllm.url` is unchanged either way.
##
## Single EPP replica: the token-load-scorer's in-flight token accounting is
## local to each EPP process, so active-active HA (replicas: 2) would halve the
## per-endpoint load each replica sees and mis-gate the affinity filter. Keep
## replicas: 1 while this routing config is in use. (The precise KV index itself
## is HA-safe — each replica converges via pod-discovery — so HA can return if
## in-flight state becomes shared across replicas.)

router:
  epp:
    replicas: 1
    flags:
      # The chart auto-adds `--ha-enable-leader-election` when replicas > 1,
      # which makes only the leader's readiness probe pass — collapsing to
      # active-passive. Embedding `=false` into the key forces the single-arg
      # form (pflag won't parse `--key value` as a bool); pflag honors
      # last-occurrence wins. No-op when replicas=1.
      ha-enable-leader-election: false
    env:
      - name: HF_TOKEN
        valueFrom:
          secretKeyRef:
            name: llm-d-hf-token
            key: HF_TOKEN

    pluginsConfigFile: "precise-prefix-cache-routing-plugins.yaml"
    pluginsCustomConfig:
      precise-prefix-cache-routing-plugins.yaml: |
        apiVersion: llm-d.ai/v1alpha1
        kind: EndpointPickerConfig
        plugins:
          - type: token-producer
            parameters:
              # Must match the model the overlay deploys: with the default
              # `../render/` overlay the render call lands on the model servers
              # themselves, so a mismatch is rejected rather than silently
              # producing token IDs from the wrong tokenizer.
              modelName: Qwen/Qwen3-32B
              vllm:
                # Guide render Service (../render/), not a loopback sidecar.
                # `precise-prefix-cache-routing-` namePrefix + Service name `render`.
                url: "http://precise-prefix-cache-routing-render:8000"
          - type: endpoint-notification-source
          - type: precise-prefix-cache-producer # `precise-prefix-cache-scorer` is deprecated
            parameters:
              tokenProcessorConfig:
                blockSizeTokens: 64
              speculativeIndexing: true
              indexerConfig:
                kvBlockIndexConfig:
                  enableMetrics: true
              kvEventsConfig:
                topicFilter: "kv@"
                concurrency: 8
                discoverPods: true
                podDiscoveryConfig:
                  socketPort: 5556
                  replaySocketPort: 5559
          - type: inflight-load-producer
            parameters:
              prefixMatchInfoProducerName: precise-prefix-cache-producer
          - type: prefix-cache-affinity-filter
            parameters:
              # Exact resident-block fractions from KV-cache events, not the
              # approximate producer.
              prefixMatchInfoProducerName: precise-prefix-cache-producer
              # Measured with guides/recipes/router/calibration/calibrate.sh on
              # this guide's reference setup (Qwen3-32B, H100 80 GB, TP=2,
              # chunk 8192) — SGLang measures the same value as vLLM. Re-measure
              # for other hardware/models.
              peakPrefillThroughput: 15926
          - type: token-load-scorer
        dataLayer: # metrics-data-source + core-metrics-extractor injected by default
          sources:
            - pluginRef: endpoint-notification-source
              extractors:
                - pluginRef: precise-prefix-cache-producer
        schedulingProfiles:
          - name: default
            plugins:
              - pluginRef: prefix-cache-affinity-filter
              - pluginRef: token-load-scorer

  extraServicePorts:
    - name: http
      port: 80
      protocol: TCP
      targetPort: 8081

  modelServers:
    type: vllm
    targetPorts:
      - number: 8000
    matchLabels:
      llm-d.ai/guide: "precise-prefix-cache-routing"

```

