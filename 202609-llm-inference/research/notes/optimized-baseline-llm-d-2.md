---
title: Optimized Baseline | llm-d
id: optimized-baseline-llm-d-2
tags:
- llmd-kv-cache-routing-c665d5
- locus-affinity-helps-or-hurts
- scheduler-defaults
- optimized-baseline
created: '2026-09-14T12:41:23.112096Z'
updated: '2026-09-15T04:55:24.124912Z'
source: https://llm-d.ai/docs/dev/well-lit-paths/foundations/optimized-baseline
source_domain: llm-d.ai
fetched_at: '2026-09-14T12:41:23.091696Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: llm-d.ai docs page (dev branch) for the 'Optimized Baseline' well-lit path.
  States the llm-d Router injects LLM-workload awareness into load balancing via prefix-cache
  affinity and server load metrics, and that EPP maintains an in-memory view of each
  endpoint's prefix-cache state, routing to the pod already holding the matching prefix.
  Explicitly caveats that this is 'one approach' — the router also supports session
  affinity and active-request-based routing as alternate scorers (see router configuration
  doc). Notes load-aware scoring probes /metrics every 50ms by default, scoring on
  queue depth, running requests, and KV-cache utilization. Flags the peakPrefillThroughput
  calibration trap on non-default hardware as a documented failure mode.
---

[Skip to main content](https://llm-d.ai/docs/dev/well-lit-paths/foundations/optimized-baseline#__docusaurus_skipToContent_fallback)
🎉 **llm-d 0.9 is here!** Hardened for scale with router HA, enhanced autoscaling, end-to-end observability and expanded hardware support. [**Read the release blog →**](https://llm-d.ai/blog/llm-d-v0.9-hardened-for-scale)
This is unreleased documentation for llm-d **dev** version.
For up-to-date documentation, see the (v0.9 (latest)).
Version: dev
On this page
Traditional HTTP requests are fast, uniform, and cheap. Standard round-robin request scheduling strategies balance this load well.
LLM requests break all three assumptions. They are:
  * **Multi-turn** - conversations and agentic tool loops send the same growing prefix repeatedly
  * **Slow** - a single request can take over a minute generating tokens
  * **Non-uniform** - range from 1000s of reasoning tokens to a 100k+ context tokens


The llm-d Router injects awareness of the LLM-workload into the load-balancing layer considering **prefix-cache affinity** and **server load metrics**.
note
This guide demonstrates one approach to prefix- and load-aware routing. The llm-d Router supports other options as well, including session affinity and active request based routing, which make no assumptions about the router's ability to parse the request or probe the servers. See [configuration](https://llm-d.ai/docs/dev/architecture/core/router/epp/configuration) for more details on the available scorers, or [precise prefix cache routing](https://llm-d.ai/docs/dev/well-lit-paths/foundations/precise-prefix-cache-routing) for KV-event-driven scoring.
## Deploy[​](https://llm-d.ai/docs/dev/well-lit-paths/foundations/optimized-baseline#deploy "Direct link to Deploy")
See the [optimized baseline guide](https://github.com/llm-d/llm-d/tree/main/guides/optimized-baseline) for manifests and step-by-step deployment.
## Architecture[​](https://llm-d.ai/docs/dev/well-lit-paths/foundations/optimized-baseline#architecture "Direct link to Architecture")
### Prefix-Aware Scheduling[​](https://llm-d.ai/docs/dev/well-lit-paths/foundations/optimized-baseline#prefix-aware-scheduling "Direct link to Prefix-Aware Scheduling")
EPP maintains a view of each endpoints' prefix-cache state in memory. When a request arrives, it identifies which pod already holds the matching prefix in KV-cache and routes the request there. For multi-turn workloads, this optimization is critical to avoid excessive recomputation in a scale-out setting.
### Load-Aware Scheduling[​](https://llm-d.ai/docs/dev/well-lit-paths/foundations/optimized-baseline#load-aware-scheduling "Direct link to Load-Aware Scheduling")
EPP continuously probes each endpoints' metrics by scraping `/metrics` at a regular interval (50ms default). It scores endpoints on queue depth, running requests, and KV-cache utilization to schedule requests to the endpoint with the lowest load, avoiding hotspots caused by heterogeneous request patterns.
## Observability[​](https://llm-d.ai/docs/dev/well-lit-paths/foundations/optimized-baseline#observability "Direct link to Observability")
The optimized baseline balances two routing objectives, prefix-cache affinity and load-aware spread, so the signals that matter are cache hit rate and per-pod load balance watched together. The [guide's Observability & Troubleshooting section](https://github.com/llm-d/llm-d/tree/main/guides/optimized-baseline#4-observability--troubleshooting) covers the key metrics for this path and the common failure modes (including the `peakPrefillThroughput` calibration trap on non-default hardware), backed by the shared [PromQL](https://llm-d.ai/docs/dev/operations/observability/promql) and [metric](https://llm-d.ai/docs/dev/operations/observability/metrics) references.
## Further Reading[​](https://llm-d.ai/docs/dev/well-lit-paths/foundations/optimized-baseline#further-reading "Direct link to Further Reading")
See [EPP Architecture](https://llm-d.ai/docs/dev/architecture/core/router/epp) for more details.
  * [Architecture](https://llm-d.ai/docs/dev/well-lit-paths/foundations/optimized-baseline#architecture)
    * [Prefix-Aware Scheduling](https://llm-d.ai/docs/dev/well-lit-paths/foundations/optimized-baseline#prefix-aware-scheduling)
    * [Load-Aware Scheduling](https://llm-d.ai/docs/dev/well-lit-paths/foundations/optimized-baseline#load-aware-scheduling)


