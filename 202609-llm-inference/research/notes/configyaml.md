---
title: config.yaml
id: configyaml
tags:
- llmd-router-nonk8s-backends-54b774
- endpointpickerconfig
- file-discovery
created: '2026-09-11T20:52:28.086735Z'
updated: '2026-09-12T03:37:37.721611Z'
source: https://raw.githubusercontent.com/llm-d/llm-d/main/guides/no-kubernetes-deployment/router/epp/config.yaml
source_domain: raw.githubusercontent.com
fetched_at: '2026-09-11T20:52:28.064132Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'llm-d guides/no-kubernetes-deployment/router/epp/config.yaml at main (fetched
  2026-09-11): the shipped EndpointPickerConfig (apiVersion llm-d.ai/v1alpha1) for
  running the EPP without Kubernetes. Registers a file-discovery plugin (path /etc/epp/endpoints.yaml,
  watchFile: true; comment: EPP must have network access to all worker IPs for metrics
  scraping), the optimized-baseline scorers (prefix-cache-scorer weight 3; queue-scorer,
  kv-cache-utilization-scorer, no-hit-lru-scorer weight 2), metrics-data-source +
  core-metrics-extractor, and selects discovery with dataLayer.injectDefaults: false
  + dataLayer.discovery.pluginRef: file-discovery. Ground truth that the discovery
  key lives under dataLayer (resolves discrepancy with discovery.go comment).'
utility_score: 15.0
---

```
apiVersion: llm-d.ai/v1alpha1
kind: EndpointPickerConfig

# Plugin set mirrors the optimized-baseline guide
# (guides/optimized-baseline/router/optimized-baseline.values.yaml),
# with the file-discovery plugin added so the EPP gets its endpoint
# inventory from a YAML file on disk instead of a Kubernetes InferencePool.
plugins:
  - name: file-discovery
    type: file-discovery
    parameters:
      path: /etc/epp/endpoints.yaml  # EPP must have network access to all worker IPs for metrics scraping
      watchFile: true

  - type: queue-scorer
  - type: kv-cache-utilization-scorer
  - type: prefix-cache-scorer
  - type: no-hit-lru-scorer

  - name: metrics-source
    type: metrics-data-source
  - name: metrics-extractor
    type: core-metrics-extractor

schedulingProfiles:
  - name: default
    plugins:
      - pluginRef: queue-scorer
        weight: 2
      - pluginRef: kv-cache-utilization-scorer
        weight: 2
      - pluginRef: prefix-cache-scorer
        weight: 3
      - pluginRef: no-hit-lru-scorer
        weight: 2

dataLayer:
  injectDefaults: false
  discovery:
    pluginRef: file-discovery
  sources:
    - pluginRef: metrics-source
      extractors:
        - pluginRef: metrics-extractor

```

