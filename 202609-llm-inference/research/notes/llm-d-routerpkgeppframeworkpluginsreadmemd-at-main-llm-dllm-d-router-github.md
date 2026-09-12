---
title: llm-d-router/pkg/epp/framework/plugins/README.md at main · llm-d/llm-d-router
  · GitHub
id: llm-d-routerpkgeppframeworkpluginsreadmemd-at-main-llm-dllm-d-router-github
tags:
- llmd-router
- llmd-router-nonk8s-backends-54b774
- epp-architecture
- plugin-registration
- multicluster
- non-k8s-precedent
created: '2026-09-11T20:49:19.863424Z'
updated: '2026-09-12T03:37:37.693559Z'
source: https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/README.md
source_domain: github.com
fetched_at: '2026-09-11T20:49:19.841200Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'llm-d-router pkg/epp/framework/plugins/README.md (fetched Sep 2026, latest
  commit 950c9636, Aug 25 2026, ''consolidate discovery plugins under a single discovery
  section''). Two findings directly answer the router-customization question. (1)
  Plugin registration and stability: every plugin gets a Stability Level (Alpha/Beta/Stable)
  assigned at registration, and ''cmd/epp/runner/runner.go is the single source of
  truth for in-tree plugin stability'' -- confirming registration is compile-time
  Go code, gated by CLI flag for Alpha plugins (--allow-experimental-plugins; EPP
  fails initialization if an Alpha plugin is configured without the flag). All in-tree
  plugins are currently Alpha or Beta; none are Stable pre-1.0. (2) Multi-cluster
  variants are a SHIPPING, DOCUMENTED PRECEDENT for exactly the non-pod-backend use
  case: a cluster-scoped EPP whose ''candidate endpoints are clusters'' rather than
  pods, using multicluster-kv-cache-utilization-scorer, multicluster-queue-scorer,
  multicluster-session-affinity-filter, multicluster-prefix-cache-scorer (scoring/filtering
  variants), multicluster-approx-prefix-cache-producer (data producer), and three
  DataLayer variants: multicluster-file-discovery (discovers peer ''clusters'' as
  endpoints from a static file, e.g. /etc/epp/clusters.yaml), multicluster-metrics-data-source,
  and multicluster-metrics-extractor (scrape each cluster and write pool-aggregate
  metrics). Critically: ''clusters.yaml follows the same format as the pod file discovery,
  except an address may be a hostname'' -- i.e. the discovery plugin''s endpoint model
  already supports hostname/non-pod-IP addressing as a first-class alternative to
  Kubernetes pod discovery, driven entirely through the dataLayer.discovery.endpoints.pluginRef
  config field. Config example shows a complete non-pod-discovery EndpointPickerConfig
  using apiVersion llm-d.ai/v1alpha1.'
---

*Suggested by [[llm-d-routerdocsarchitecturemd-at-main-llm-dllm-d-router-github]] — architecture.md links here as the canonical plugin catalog*

[Skip to content](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/README.md#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/README.md) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/README.md) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/README.md) to refresh your session. Dismiss alert
###  Uh oh! 
There was an error while loading. [Please reload this page](https://github.com/llm-d/llm-d-router/blob/main/pkg/epp/framework/plugins/README.md).
/ Public
  * [ Notifications ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router) You must be signed in to change notification settings
  * [ Fork 366 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)
  * [ Star  333 ](https://github.com/login?return_to=%2Fllm-d%2Fllm-d-router)


## Collapse file tree
## Files
Search this repository(forward slash)` forward slash/`
/
# README.md
Copy path
More file actions
More file actions
## Latest commit
[config: consolidate discovery plugins under a single discovery sectio…](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19)
Open commit detailsfailure
Aug 25, 2026
[950c963](https://github.com/llm-d/llm-d-router/commit/950c963644b88d697fafda6f9e79390855996e19) · Aug 25, 2026
## History
[History](https://github.com/llm-d/llm-d-router/commits/main/pkg/epp/framework/plugins/README.md)
Open commit details
History
79 lines (59 loc) · 4.33 KB
Code owner: @llm-d/router-maintainers
/
# README.md
Copy path
## File metadata and controls
  * Preview
  * 

79 lines (59 loc) · 4.33 KB
Code owner: @llm-d/router-maintainers
Copy raw file
Download raw file
You must be signed in to make or propose changes
More edit options
Outline
Edit and raw actions
# Plugins
This directory contains the available plugins. For detailed information on individual plugins, please refer to the README.md located within each plugin's respective directory. To understand how they integrate into the incoming request processing lifecycle, see the [Endpoint Picker (EPP) design](https://github.com/llm-d/llm-d/tree/main/docs/architecture/core/router/epp) document.
## Plugin Stability Levels
Every plugin in `llm-d-router` is assigned a **Stability Level** upon registration (`cmd/epp/runner/runner.go` is the single source of truth for in-tree plugin stability):  
| Stability Level  | Lifecycle & Backwards Compatibility Guarantees  | Command Line Flag Requirement  |  
| --- | --- | --- |  
| **Alpha**  | Experimental features under active development. No backwards-compatibility guarantees (parameters or behavior may change anytime).  | Requires `--allow-experimental-plugins` CLI flag.  |  
| **Beta**  | Feature-complete and enabled by default. Backwards-compatible within current version; subject to a +2 minor version deprecation policy before removal.  | Allowed by default (no CLI flag required).  |  
| **Stable**  | Production-grade and fully backwards-compatible across minor releases. Breaking changes only on major version bumps.  | Allowed by default (no CLI flag required).  |  
Note
Currently, all in-tree plugins are classified as **Alpha** or **Beta**. Plugins will be promoted to **Stable** as the project approaches its 1.0 release.
### Alpha Plugin CLI Flag (`--allow-experimental-plugins`)
To ensure experimental plugins are only enabled intentionally, Alpha plugins require passing the `--allow-experimental-plugins` command-line flag to the EPP runner:

```
epp --allow-experimental-plugins
```

If an Alpha plugin is configured while `--allow-experimental-plugins` is not set (the default), the EPP runner will fail initialization with an explicit error.
## Multi-cluster variants
A few plugins ship a multi-cluster variant beside the stock plugin, for a cluster-scoped EPP whose candidate endpoints are clusters. The scheduling variants are `multicluster-kv-cache-utilization-scorer`, `multicluster-queue-scorer`, `multicluster-session-affinity-filter`, and `multicluster-prefix-cache-scorer`. The `multicluster-approx-prefix-cache-producer` is the matching request-control data producer. They rely on three datalayer variants: `multicluster-file-discovery` discovers peer clusters as endpoints, and `multicluster-metrics-data-source` with `multicluster-metrics-extractor` scrape each cluster and write its pool-aggregate metrics into the attributes the scorers read.
The pure-delegation variants (`multicluster-session-affinity-filter`, `multicluster-prefix-cache-scorer`, `multicluster-approx-prefix-cache-producer`) reuse the stock plugin's behavior and only rename the type, so their per-plugin Prometheus metrics report under the stock type label rather than the multicluster one.
### Dependencies
The metric scorers are not standalone. `multicluster-kv-cache-utilization-scorer` and `multicluster-queue-scorer` score a cluster from the pool-aggregate metrics that `multicluster-metrics-extractor` writes from what `multicluster-metrics-data-source` scrapes. Configure all three together in `dataLayer`. A config with the scorers but without the source and extractor fails to load.
### Example

```
apiVersion: llm-d.ai/v1alpha1
kind: EndpointPickerConfig
plugins:
  - type: multicluster-file-discovery
    name: discovery
    parameters:
      path: /etc/epp/clusters.yaml
  - type: multicluster-metrics-data-source
    name: metrics-source
    parameters:
      scheme: https
      caCertPath: /etc/epp/tls/ca.crt
  - type: multicluster-metrics-extractor
    name: metrics-extractor
  - type: multicluster-session-affinity-filter
    name: session-affinity
  - type: multicluster-kv-cache-utilization-scorer
    name: kv-cache
  - type: multicluster-queue-scorer
    name: queue
  - type: max-score-picker
  - type: single-profile-handler
dataLayer:
  discovery:
    endpoints:
      pluginRef: discovery
  sources:
    - pluginRef: metrics-source
      extractors:
        - pluginRef: metrics-extractor
```

`clusters.yaml` follows the same format as the pod file discovery, except an `address` may be a hostname.
## Related Documentation
  * [Architecture Overview](https://github.com/llm-d/llm-d-router/blob/main/docs/architecture.md)
  * [Creating a new Filter guide](https://github.com/llm-d/llm-d-router/blob/main/docs/create_new_filter.md)


You can’t perform that action at this time. 
