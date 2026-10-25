---
title: agentic-serving.values.yaml
id: agentic-servingvaluesyaml
tags:
- llmd-kv-cache-routing-c665d5
- helm-values
- epp-config-reference
- ground-truth
created: '2026-09-14T13:04:28.341480Z'
updated: '2026-10-10T14:52:17.324069Z'
source: https://raw.githubusercontent.com/llm-d/llm-d/main/guides/agentic-serving/router/agentic-serving.values.yaml
source_domain: raw.githubusercontent.com
fetched_at: '2026-09-14T13:04:28.317351Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Actual shipped Helm values file (guides/agentic-serving/router/agentic-serving.values.yaml,
  llm-d/llm-d main) for the agentic-serving well-lit path. Uses approx-prefix-cache-producer
  (not precise) but with autoTune explicitly disabled and lruCapacityPerServer manually
  set to 1,570,000 (~1000x num_gpu_blocks) and maxPrefixTokensToMatch:262144 (2x the
  pd-disaggregation guide''s window) -- comment explains autoTune derives capacity
  from on-device HBM blocks only and under-counts cache when KV-offloading is enabled,
  workaround tracked at llm-d-router issue #1650 (offload-aware LRU), to be removed
  once fixed upstream. prefix-cache-affinity-filter parameterized with peakPrefillThroughput:16444
  (Qwen3-Coder-480B-FP8 on TPU v7x, distinct from the 15926-15928 H100/Qwen3-32B value
  other guides use). schedulingProfiles ''default'': prefix-cache-affinity-filter
  + token-load-scorer -- same two-plugin shape as optimized-baseline and precise-prefix-cache-routing.'
utility_score: 14.0
---

```
router:
  extraServicePorts:
    - name: http
      port: 80
      protocol: TCP
      targetPort: 8081

  epp:
    pluginsConfigFile: agentic-serving-plugins.yaml
    pluginsCustomConfig:
      agentic-serving-plugins.yaml: |
        apiVersion: llm-d.ai/v1alpha1
        kind: EndpointPickerConfig
        plugins:
        - type: token-load-scorer
        - type: inflight-load-producer
        - type: approx-prefix-cache-producer
          parameters:
            maxPrefixTokensToMatch: 262144
            # WORKAROUND: autoTune is disabled and the LRU is sized manually.
            # autoTune derives lruCapacityPerServer from on-device (HBM) blocks
            # only (num_gpu_blocks) and ignores the KV-cache offload tier. With
            # offloading enabled this under-counts cached prefixes.
            # We size the LRU for the offload-extended cache
            # (~num_gpu_blocks x 1000). Remove once autoTune is made offload-aware
            # upstream — tracked by the llm-d-router offload-aware-LRU issue.
            # https://github.com/llm-d/llm-d-router/issues/1650
            autoTune: false
            lruCapacityPerServer: 1570000
            blockSizeTokens: 256
        - type: prefix-cache-affinity-filter
          parameters:
            peakPrefillThroughput: 16444
        schedulingProfiles:
        - name: default
          plugins:
          - pluginRef: prefix-cache-affinity-filter
          - pluginRef: token-load-scorer

  modelServers:
    matchLabels:
      llm-d.ai/guide: "agentic-serving"

```

