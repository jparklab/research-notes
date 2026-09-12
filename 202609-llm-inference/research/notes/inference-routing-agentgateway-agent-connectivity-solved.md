---
title: Inference routing – agentgateway | Agent Connectivity Solved
id: inference-routing-agentgateway-agent-connectivity-solved
tags:
- llmd-router-nonk8s-backends
- agentgateway
- standalone-mode
- epp
- llmd-router-nonk8s-backends-54b774
created: '2026-09-11T19:43:50.639822Z'
updated: '2026-09-12T03:37:37.544948Z'
source: https://agentgateway.dev/docs/standalone/latest/inference/
source_domain: agentgateway.dev
fetched_at: '2026-09-11T19:43:50.618148Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: 'Agentgateway docs describe two inference-routing deployment modes: Kubernetes
  Gateway API mode (InferencePool + HTTPRoute + EPP) and ''standalone request scheduler
  mode'', where agentgateway runs as a localhost sidecar to the EPP and does NOT support
  InferencePool. In standalone mode the config defines a synthetic top-level ''services''
  entry (e.g. ''default/my-model'') that the route backend references; when the EPP
  owns endpoint discovery, operators set ''destinationMode: passthrough'' under policies.inferenceRouting
  so EPP-selected ip:port destinations are forwarded to directly without matching
  them against locally known workload endpoints. This is the exact mechanism for routing
  to non-pod/non-k8s backends: the synthetic service is a placeholder, and passthrough
  mode tells agentgateway to trust the EPP''s returned destination rather than validating
  it against known endpoints.'
utility_score: 15.0
---

[Skip to content](https://agentgateway.dev/docs/standalone/latest/documentation/inference/#content)
`CTRL K`
Toggle theme [Docs](https://agentgateway.dev/docs/) [Standalone](https://agentgateway.dev/docs/standalone/latest/) [Kubernetes](https://agentgateway.dev/docs/kubernetes/latest/) [Models](https://agentgateway.dev/models) [Blog](https://agentgateway.dev/blog) [Enterprise](https://agentgateway.dev/enterprise) [Community](https://discord.gg/y9efgEmppm) [Get Started](https://agentgateway.dev/#getting-started) [GitHub](https://github.com/agentgateway/agentgateway)
agentgateway has joined the **Agentic AI Foundation** — [Learn more](https://aaif.io/blog/agentgateway-joins-aaif-as-an-open-gateway-for-agentic-ai-infrastructure/) ×
For the complete documentation index, see [llms.txt](https://agentgateway.dev/llms.txt). Markdown versions of all docs pages are available by appending .md to any docs URL.
# Inference routing
Copy as Markdown
  * Copy as Markdown
  * View as Markdown
  * Copy Codeblocks
  * Connect to Docs MCP
  * Open in Claude
  * Open in ChatGPT
  * Open in Perplexity
  * Print
  * Download all docs (PDF)


Page as Markdown
Copy Download ✕
Configure Kubernetes Gateway API or standalone inference routing for AI workloads.
Agentgateway supports the Kubernetes Gateway API Inference Extension in two deployment modes.
## Kubernetes Gateway API mode 
In Kubernetes Gateway API mode, agentgateway runs as the gateway data plane for Gateway API resources. You install the Inference Extension CRDs, create an `InferencePool`, and route to that pool from an `HTTPRoute`. The Endpoint Picker Extension (EPP) acts as an extension service that selects the best model server endpoint for each inference request.
Use this mode when you want Gateway API integration, `InferencePool` resources, traffic splitting, route matching, and other Kubernetes networking features.
[Set up Kubernetes inference routing](https://agentgateway.dev/docs/kubernetes/latest/documentation/inference/)
## Standalone request scheduler mode 
In standalone request scheduler mode, agentgateway runs as a sidecar proxy with the EPP. The proxy and EPP communicate over localhost, and agentgateway uses its standalone `inferenceRouting` local configuration to route requests to a synthetic service before consulting the EPP for endpoint selection.
Use this mode for single-tenant or job-scoped workloads where deploying a full Gateway API stack would add unnecessary operational overhead. In this mode, the [upstream standalone Helm chart](https://github.com/llm-d/llm-d-router/tree/main/config/charts) can deploy agentgateway as the sidecar proxy with `proxyType: agentgateway`.
Standalone request scheduler mode does not support `InferencePool`. The standalone configuration must define a top-level synthetic service, such as a `services` entry, and the route backend must reference that service. When EPP owns endpoint discovery, set `destinationMode: passthrough` so EPP-selected destinations can be forwarded to directly without matching local workload endpoint data.
For example, the standalone agentgateway configuration defines the synthetic service in `services`, and the route backend references it as `default/my-model`.

```
services:- name:my-modelnamespace:defaulthostname:my-modelvips:[]8000:8000- port:8081listeners:- name:defaultprotocol:HTTProutes:- name:standalone-eppmatches:pathPrefix:/v1/chat/completionsbackends:- service:name:default/my-modelport:8000policies:inferenceRouting:endpointPicker:host:127.0.0.1:9002destinationMode:passthrough
```

[Agentgateway llm-standalone-epp example](https://github.com/agentgateway/agentgateway/blob/main/examples/llm-standalone-epp/README.md)
Was this page helpful?
Ask AI
Agentgateway assistant
Ask me anything about agentgateway configuration, features, or usage.
Note: AI-generated content might contain errors; please verify and test all returned information.
Tip: one topic per conversation gives the best results. Use the **+** button in the chat header to start a new conversation.
Switching topics? Starting a new conversation improves accuracy. Start new conversation
↑↓ navigate ↵ select esc dismiss
Add this page Mention a page
Standalone
Standalone
Standalone deployment docs
Kubernetes
Kubernetes deployment docs
### What could be improved?
Your feedback helps us improve assistant answers and identify docs gaps we should fix.
Need more help? Join us on Discord: <https://discord.gg/y9efgEmppm>
Want to use your own agent? Add the Solo MCP server to query our docs directly. Get started here: <https://search.solo.io/>.
Skip Submit
