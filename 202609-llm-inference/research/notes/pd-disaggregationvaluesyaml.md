---
title: pd-disaggregation.values.yaml
id: pd-disaggregationvaluesyaml
tags:
- llmd-kv-cache-routing-c665d5
- helm-values
- epp-config-reference
- ground-truth
- pd-disaggregation
created: '2026-09-14T13:03:53.114468Z'
updated: '2026-10-10T14:52:17.322497Z'
source: https://raw.githubusercontent.com/llm-d/llm-d/main/guides/pd-disaggregation/router/pd-disaggregation.values.yaml
source_domain: raw.githubusercontent.com
fetched_at: '2026-09-14T13:03:53.093315Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Actual shipped Helm values file (guides/pd-disaggregation/router/pd-disaggregation.values.yaml,
  llm-d/llm-d main) for the P/D-disaggregated well-lit path -- the only guide with
  TWO schedulingProfiles instead of one. Plugins: always-disagg-pd-decider, disagg-profile-handler
  (parameters.deciders.prefill: always-disagg-pd-decider), prefill-filter, decode-filter,
  approx-prefix-cache-producer (maxPrefixTokensToMatch:131072), inflight-load-producer,
  prefix-cache-affinity-filter (peakPrefillThroughput:33821, measured through the
  full P/D + NIXL transfer path on 8x prefill TP=1 H200), token-load-scorer, active-request-scorer,
  max-score-picker. schedulingProfile ''prefill'': prefill-filter -> prefix-cache-affinity-filter
  -> token-load-scorer -> max-score-picker (cache-affinity-aware). schedulingProfile
  ''decode'': decode-filter -> active-request-scorer -> max-score-picker (pure load-based,
  no cache affinity/prefix producer involved in the decode leg). Forces replicas:1
  for the same in-flight-accounting-locality reason as precise-prefix-cache-routing.'
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
    # Single EPP replica: the inflight-load-producer's in-flight token accounting
    # (which the token-load-scorer and affinity filter consume) is local to each
    # EPP process, so multiple replicas would each see only a fraction of the
    # per-endpoint load and incorrectly gate the affinity filter.
    replicas: 1
    pluginsConfigFile: "pd-config.yaml"
    pluginsCustomConfig:
      pd-config.yaml: |
        apiVersion: llm-d.ai/v1alpha1
        kind: EndpointPickerConfig
        plugins:
        - type: always-disagg-pd-decider
        - type: disagg-profile-handler
          parameters:
            deciders:
              prefill: always-disagg-pd-decider
        - type: prefill-filter
        - type: decode-filter
        - type: approx-prefix-cache-producer
          parameters:
            # 5k-ISL gpt-oss-120b reference workload; a modest match window
            # keeps EPP prefix-matching cheap.
            maxPrefixTokensToMatch: 131072
        - type: inflight-load-producer
        - type: prefix-cache-affinity-filter
          parameters:
            # Measured with guides/recipes/router/calibration/calibrate.sh
            # through the full P/D path on the reference fleet (gpt-oss-120b,
            # 8x prefill TP=1 on H200, chunk 8192). Includes the NIXL KV
            # transfer + sidecar hop, so it is the operational ceiling the
            # gate acts on. Re-measure for other hardware/models.
            peakPrefillThroughput: 33821
        - type: token-load-scorer
        - type: active-request-scorer
        - type: max-score-picker
        schedulingProfiles:
        # Prefill does the prompt pass: keep prefix groups on cache-warm pods
        # (affinity filter) and pick by queued prefill token load.
        - name: prefill
          plugins:
          - pluginRef: prefill-filter
          - pluginRef: prefix-cache-affinity-filter
          - pluginRef: token-load-scorer
          - pluginRef: max-score-picker
        # Pure decode is bound by concurrent in-flight requests, not prefill
        # throughput: pick the least-busy endpoint.
        - name: decode
          plugins:
          - pluginRef: decode-filter
          - pluginRef: active-request-scorer
          - pluginRef: max-score-picker

  modelServers:
    matchLabels:
      llm-d.ai/guide: "pd-disaggregation"

```

