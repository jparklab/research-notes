---
title: Optimized Baseline | llm-d
id: optimized-baseline-llm-d
tags:
- llmd-kv-cache-routing-c665d5
- llm-d
- routing
- epp
- load-balancing
created: '2026-09-14T11:56:21.275422Z'
updated: '2026-09-15T04:55:24.071554Z'
source: https://llm-d.ai/docs/well-lit-paths/foundations/optimized-baseline
source_domain: llm-d.ai
fetched_at: '2026-09-14T11:56:21.254777Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'v0.9 llm-d well-lit-path doc explaining why LLM request scheduling needs
  to diverge from standard round-robin HTTP load balancing: LLM requests are multi-turn
  (repeatedly resending a growing prefix), slow (single requests can exceed a minute),
  and non-uniform (1000s of reasoning tokens to 100k+ context). The llm-d Router''s
  Optimized Baseline injects two signals into load balancing: Prefix-Aware Scheduling
  (EPP maintains an in-memory view of each endpoint''s prefix-cache state and routes
  to the pod already holding the matching prefix — critical for multi-turn workloads
  to avoid recomputation at scale) and Load-Aware Scheduling (EPP scrapes each endpoint''s
  /metrics at a default 50ms interval, scoring queue depth, running requests, and
  KV-cache utilization to avoid hotspots from heterogeneous request patterns). Notes
  the Router also supports session-affinity and active-request-based routing that
  make no assumptions about parsing requests or probing servers, and positions this
  baseline as distinct from the event-driven precise-prefix-cache-routing path.'
---

*Suggested by [[precise-prefix-cache-routing-llm-d]] — referenced as the comparison baseline path that combines with prefix-affinity scoring*

[Skip to main content](https://llm-d.ai/docs/well-lit-paths/foundations/optimized-baseline#__docusaurus_skipToContent_fallback)
🎉 **llm-d 0.9 is here!** Hardened for scale with router HA, enhanced autoscaling, end-to-end observability and expanded hardware support. [**Read the release blog →**](https://llm-d.ai/blog/llm-d-v0.9-hardened-for-scale)
Version: v0.9 (latest)
On this page
Traditional HTTP requests are fast, uniform, and cheap. Standard round-robin request scheduling strategies balance this load well.
LLM requests break all three assumptions. They are:
  * **Multi-turn** - conversations and agentic tool loops send the same growing prefix repeatedly
  * **Slow** - a single request can take over a minute generating tokens
  * **Non-uniform** - range from 1000s of reasoning tokens to a 100k+ context tokens


The llm-d Router injects awareness of the LLM-workload into the load-balancing layer considering **prefix-cache affinity** and **server load metrics**.
note
This guide demonstrates one approach to prefix- and load-aware routing. The llm-d Router supports other options as well, including session affinity and active request based routing, which make no assumptions about the router's ability to parse the request or probe the servers. See [configuration](https://llm-d.ai/docs/architecture/core/router/epp/configuration) for more details on the available scorers, or [precise prefix cache routing](https://llm-d.ai/docs/well-lit-paths/foundations/precise-prefix-cache-routing) for KV-event-driven scoring.
## Deploy[​](https://llm-d.ai/docs/well-lit-paths/foundations/optimized-baseline#deploy "Direct link to Deploy")
See the [optimized baseline guide](https://github.com/llm-d/llm-d/tree/main/guides/optimized-baseline) for manifests and step-by-step deployment.
## Architecture[​](https://llm-d.ai/docs/well-lit-paths/foundations/optimized-baseline#architecture "Direct link to Architecture")
### Prefix-Aware Scheduling[​](https://llm-d.ai/docs/well-lit-paths/foundations/optimized-baseline#prefix-aware-scheduling "Direct link to Prefix-Aware Scheduling")
EPP maintains a view of each endpoints' prefix-cache state in memory. When a request arrives, it identifies which pod already holds the matching prefix in KV-cache and routes the request there. For multi-turn workloads, this optimization is critical to avoid excessive recomputation in a scale-out setting.
### Load-Aware Scheduling[​](https://llm-d.ai/docs/well-lit-paths/foundations/optimized-baseline#load-aware-scheduling "Direct link to Load-Aware Scheduling")
EPP continuously probes each endpoints' metrics by scraping `/metrics` at a regular interval (50ms default). It scores endpoints on queue depth, running requests, and KV-cache utilization to schedule requests to the endpoint with the lowest load, avoiding hotspots caused by heterogeneous request patterns.
## Further Reading[​](https://llm-d.ai/docs/well-lit-paths/foundations/optimized-baseline#further-reading "Direct link to Further Reading")
See [EPP Architecture](https://llm-d.ai/docs/architecture/core/router/epp) for more details.
  * [Architecture](https://llm-d.ai/docs/well-lit-paths/foundations/optimized-baseline#architecture)
    * [Prefix-Aware Scheduling](https://llm-d.ai/docs/well-lit-paths/foundations/optimized-baseline#prefix-aware-scheduling)
    * [Load-Aware Scheduling](https://llm-d.ai/docs/well-lit-paths/foundations/optimized-baseline#load-aware-scheduling)


