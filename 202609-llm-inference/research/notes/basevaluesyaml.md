---
title: base.values.yaml
id: basevaluesyaml
tags:
- llmd-kv-cache-routing-c665d5
- helm-values
- epp-config-reference
created: '2026-09-14T13:02:15.001557Z'
updated: '2026-10-10T14:52:17.312481Z'
source: https://raw.githubusercontent.com/llm-d/llm-d/main/guides/recipes/router/base.values.yaml
source_domain: raw.githubusercontent.com
fetched_at: '2026-09-14T13:02:14.981912Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Shared Helm values base (guides/recipes/router/base.values.yaml, llm-d/llm-d
  main) layered under every well-lit-path guide''s own values file via ''-f base.values.yaml
  -f <guide>.values.yaml''. Sets router.epp.replicas=1, router.epp.pluginsConfigFile:
  default-plugins.yaml (the pre-override default), inferencePool.failureMode: FailOpen,
  envoy sidecar proxy defaults (--concurrency 8, --log-level warn, --drain-time-s
  60), and OTel tracing at 10% sample rate (parentbased_traceidratio). Contains no
  schedulingProfiles/plugins of its own -- those are guide-specific overrides on top
  of this file.'
utility_score: 14.0
---

```
## Base values shared by both standalone and llm-d Router charts.
## Layer chart-specific or guide-specific overrides on top with additional -f flags.

router:
  epp:
    replicas: 1
    flags:
      v: 2
    pluginsConfigFile: "default-plugins.yaml"
    resources:
      requests:
        cpu: "4"
        memory: 8Gi
      limits:
        memory: 16Gi

  inferencePool:
    failureMode: "FailOpen"

  modelServers:
    protocol: http # http, grpc

  proxy:
    # Sidecar proxy type. Options: envoy (default) or agentgateway.
    # To use agentgateway instead of Envoy, override in your guide-specific
    # values file or pass --set router.proxy.proxyType=agentgateway on the
    # helm install command. See guides/recipes/router/README.md for details.
    proxyType: envoy

    # CLI args for the Envoy sidecar. Override the chart's defaults of
    # `--log-level trace` (very expensive under load) and no `--concurrency`
    # (Envoy spawns ~hardware_concurrency() worker threads, oversubscribing
    # the cgroup's CPU slice). `--concurrency 8` matches the resources.limits
    # equivalent below; trace-level logging would burn a meaningful share of
    # the sidecar's CPU at high QPS.
    #
    # For proxyType=agentgateway, layer features/agentgateway-proxy.values.yaml
    # on top of this file. It swaps these Envoy flags for agentgateway's args.
    args:
      - "--service-node"
      - "envoy-sidecar"
      - "--log-level"
      - "warn"
      - "--concurrency"
      - "8"
      - "--drain-strategy"
      - "immediate"
      - "--drain-time-s"
      - "60"
      - "-c"
      - "/etc/envoy/envoy.yaml"
    # Resource requests and limits for the sidecar proxy.
    resources:
      requests:
        cpu: "4"
        memory: 8Gi
      limits:
        memory: 16Gi

  tracing:
    otelExporterEndpoint: "http://localhost:4317"
    sampling:
      sampler: "parentbased_traceidratio"
      samplerArg: "0.1"

```

