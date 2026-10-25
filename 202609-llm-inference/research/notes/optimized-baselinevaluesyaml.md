---
title: optimized-baseline.values.yaml
id: optimized-baselinevaluesyaml
tags:
- llmd-kv-cache-routing-c665d5
- helm-values
- epp-config-reference
- ground-truth
created: '2026-09-14T13:02:46.092167Z'
updated: '2026-10-10T14:52:17.313710Z'
source: https://raw.githubusercontent.com/llm-d/llm-d/main/guides/optimized-baseline/router/optimized-baseline.values.yaml
source_domain: raw.githubusercontent.com
fetched_at: '2026-09-14T13:02:46.071238Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Actual shipped Helm values file (guides/optimized-baseline/router/optimized-baseline.values.yaml,
  llm-d/llm-d main) confirming the well-lit-path default scoring plugin set. EndpointPickerConfig
  plugins: approx-prefix-cache-producer, inflight-load-producer, prefix-cache-affinity-filter,
  token-load-scorer -- all run with built-in defaults (no per-plugin parameters block).
  Single schedulingProfile named ''default'' with exactly two pluginRefs: prefix-cache-affinity-filter
  and token-load-scorer (both filter+scorer roles in the single profile, no queue-scorer,
  no kv-cache-utilization-scorer, no prefix-cache-scorer). Comment confirms peakPrefillThroughput
  default of 15928 is calibrated for Qwen3-32B/H100 80GB TP=2 and must be re-measured
  for other hardware via guides/recipes/router/calibration.'
utility_score: 14.0
---

```
## optimized-baseline guide overrides for the llm-d router.
## Layer on top of: recipes/router/base.values.yaml

router:
  extraServicePorts:
    - name: http
      port: 80
      protocol: TCP
      targetPort: 8081

  epp:
    pluginsConfigFile: "optimized-baseline-plugins.yaml"
    pluginsCustomConfig:
      optimized-baseline-plugins.yaml: |
        apiVersion: llm-d.ai/v1alpha1
        kind: EndpointPickerConfig
        # Plugins run with their built-in defaults: the prefix-cache-affinity-filter's
        # peakPrefillThroughput default (15928) is calibrated for this guide's reference
        # setup (Qwen3-32B on H100 80 GB, TP=2). For other hardware/models, measure your
        # own value with guides/recipes/router/calibration and set it on the filter.
        plugins:
        - type: approx-prefix-cache-producer
        - type: inflight-load-producer
        - type: prefix-cache-affinity-filter
        - type: token-load-scorer
        schedulingProfiles:
        - name: default
          plugins:
          - pluginRef: prefix-cache-affinity-filter
          - pluginRef: token-load-scorer

  modelServers:
    matchLabels:
      llm-d.ai/guide: "optimized-baseline"

```

