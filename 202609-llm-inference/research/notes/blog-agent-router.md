---
title: Blog | Agent Router
id: blog-agent-router
tags:
- gateway-routing
- ai-gateway
- agent-router-rebrand
created: '2026-10-09T22:53:39.230587Z'
updated: '2026-10-10T14:52:16.996439Z'
source: https://aigateway.envoyproxy.io/blog/
source_domain: aigateway.envoyproxy.io
fetched_at: '2026-10-09T22:53:39.205191Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Blog index for the Envoy AI Gateway project, which rebranded to ''Agent
  Router'' under the Agentic AI Foundation as of Sept 9 2026 (''same code, same maintainers,
  same APIs''). Chronicles the feature timeline relevant to inference-aware routing:
  Endpoint Picker support for intelligent inference routing based on KV-cache usage
  and queue depth was introduced in the v0.3 release (Jul 30 / Aug 22, 2025), moving
  the gateway ''from static to intelligent.'' v1.0 (Jun 23 2026) is described as the
  first stable, production-ready release with a committed-stable control-plane API,
  16 AI providers, an MCP gateway, multimodal support, and enterprise observability.
  Post-1.0 development pivots toward agentic/MCP concerns: per-user identity at the
  gateway layer via header forwarding for multi-tenant MCP (v1.2, Oct 9 2026), credential
  isolation between teams sharing one gateway, and a separate benchmarking post measuring
  control-plane scaling to 2,000 AIGatewayRoute resources (latency/CPU/memory). Confirms
  the project has multiple named individual maintainers/authors (Erica Hughberg, Xunzhuo
  ''Bit'' Liu, Takeshi Yoneda, Ignasi Barrera, Adrian Cole, Mohit Gurnani, Hrushikesh
  Patil) rather than a single corporate voice.'
utility_score: 16.0
---

[Skip to main content](https://theagentrouter.ai/blog/#__docusaurus_skipToContent_fallback)
**Formerly Envoy AI Gateway** — now an Agentic AI Foundation project. Same code, same maintainers. [Read the announcement →](https://theagentrouter.ai/blog/envoy-ai-gateway-is-now-agent-router)
Blog
## Announcements, releases, and deep dives
Stories from the Agent Router community — and the Envoy AI Gateway years before it.
[ RELEASES · NEWSAgent Router 1.2: safe to share across users and teams Agent Router 1.2 is the first release under the new name. It hardens the MCP gateway for real users, isolates credentials between teams that share a gateway, and fixes the bugs that bite in production. Here is what that means for you, what to check before you upgrade, and how to get involved. Agent Router MaintainersOct 9, 2026 · 5 min read ](https://theagentrouter.ai/blog/v1.2-release-announcement)[ ANNOUNCEMENTS · NEWSEnvoy AI Gateway is becoming Agent Router, an Agentic AI Foundation project On 10 September, Envoy AI Gateway officially joins the Agentic AI Foundation as Agent Router. Same code, same maintainers, same APIs. Here is what changes, what does not, and where to find us. Agent Router MaintainersSep 9, 2026 · 2 min read ](https://theagentrouter.ai/blog/envoy-ai-gateway-is-now-agent-router)[ FEATURESMulti-User MCP in Production: Per-User Identity at the Gateway Layer How Envoy AI Gateway's header forwarding gives MCP tool calls per-user identity without OAuth infrastructure — and when you need OAuth instead. Mohit GurnaniJul 5, 2026 · 10 min read ](https://theagentrouter.ai/blog/multi-user-mcp-header-forwarding)[ NEWS · RELEASESAnnouncing Envoy AI Gateway 1.0 — A Stable, Production-Ready AI Gateway Envoy AI Gateway 1.0 is here — the first stable release of the open source AI gateway built on CNCF Envoy Gateway. A committed-stable control-plane API, 16 AI providers, an MCP gateway, multimodal support, and enterprise observability, all backed by a growing community. Erica Hughberg +8Jun 23, 2026 · 6 min read ](https://theagentrouter.ai/blog/v1.0-release-announcement)[ FEATURESBenchmarking Envoy AI Gateway Control Plane Scaling How many AIGatewayRoute resources can Envoy AI Gateway handle? We benchmarked the control plane scaling to 2,000 routes and measured latency, CPU, and memory usage. Hrushikesh PatilFeb 23, 2026 · 8 min read ](https://theagentrouter.ai/blog/benchmarking-control-plane-scaling)[ FEATURESThe Reality and Performance of MCP Traffic Routing with Envoy AI Gateway In this article explore how Envoy AI Gateway handles stateful MCP sessions, keeps performance competitive, and stays aligned with the broader Envoy ecosystem. Ignasi Barrera & Erica HughbergDec 8, 2025 · 9 min read ](https://theagentrouter.ai/blog/mcp-in-envoy-ai-gateway)[ NEWS · FEATURESAnnouncing Model Context Protocol Support in Envoy AI Gateway Envoy AI Gateway now supports Model Context Protocol (MCP), bringing enterprise-grade security, routing, and observability to AI agent tool integrations with full spec compliance, OAuth authentication, and zero-friction deployment. Ignasi Barrera & Takeshi YonedaOct 2, 2025 · 8 min read ](https://theagentrouter.ai/blog/mcp-implementation)[ FEATURESEnhancing AI Gateway Observability - OpenTelemetry Tracing Arrives in Envoy AI Gateway Envoy AI Gateway v0.3 introduces OpenTelemetry tracing with OpenInference conventions, providing complete visibility into LLM application behavior beyond metrics.Understand not just what happened, but why. Erica Hughberg & Adrian ColeAug 25, 2025 · 6 min read ](https://theagentrouter.ai/blog/openinference-for-ai-observability)[ NEWS · RELEASESAnnouncing the Envoy AI Gateway v0.3 Release Envoy AI Gateway v0.3 brings intelligent inference routing with Endpoint Picker, production-ready Google Vertex AI support, and enterprise observability with OpenInference tracing—transforming AI infrastructure from static to intelligent. Erica Hughberg & Xunzhuo (Bit) LiuAug 22, 2025 · 8 min read ](https://theagentrouter.ai/blog/v0.3-release-announcement)[ NEWS · FEATURESEnvoy AI Gateway Introduces Endpoint Picker Support Envoy AI Gateway introduces Endpoint Picker Provider support for intelligent inference routing based on real-time AI metrics like KV-cache usage and queue depth—moving beyond traditional load balancing to optimize AI workload performance. Erica Hughberg & Xunzhuo (Bit) LiuJul 30, 2025 · 7 min read ](https://theagentrouter.ai/blog/endpoint-picker-for-inference-routing)
