---
title: Envoy AI Gateway Reaches v1.0, Establishing the Open Source Standard for Enterprise
  AI Traffic
id: envoy-ai-gateway-reaches-v10-establishing-the-open-source-standard-for-enterpris
tags:
- gateway-routing
- production-adopter
- corporate-maintainer
- mcp-gateway
created: '2026-10-09T22:54:08.695260Z'
updated: '2026-10-10T14:52:17.015589Z'
source: https://www.prnewswire.com/news-releases/envoy-ai-gateway-reaches-v1-0--establishing-the-open-source-standard-for-enterprise-ai-traffic-302808088.html
source_domain: www.prnewswire.com
fetched_at: '2026-10-09T22:54:08.672604Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Official press release (PRNewswire, June 23 2026, issued by Tetrate) announcing
  Envoy AI Gateway v1.0 GA -- the primary corporate-voice source for who built and
  runs this project. Names the exact corporate maintainer set explicitly: Tetrate
  (primary upstream Envoy contributor), Bloomberg (proposed the effort to the Envoy
  Gateway community ''to address its own challenge: routing generative AI workloads''),
  and Nutanix; project first released February 2025, reaching v1.0 after ''16 months''
  of collaboration. Two named production adopters with direct quotes: Bloomberg runs
  it in production (''demonstrating the same open source code available to the community
  is stable and scalable enough to power demanding enterprise workloads'' -- quote
  from Dan Sun, Envoy AI Gateway/KServe co-founder) and LY Corporation, quoted via
  principal AI-infrastructure architect Shingo Omura, uses it to manage ''multi-tenant,
  self-hosted LLM traffic'' citing ''a unified API for flexible routing, token-based
  rate limiting, authentication and authorization, monitoring and extensibility, aligned
  with open standards like the Kubernetes Gateway API Inference Extension.'' Nutanix
  is incorporating it into its own Nutanix Agent Gateway and Nutanix Enterprise AI
  products. Lists the v1.0 feature set verbatim by category: unified OpenAI-compatible
  API across OpenAI/Anthropic/Bedrock/Vertex/Gemini/Azure OpenAI/Groq/Together/Mistral/Cohere/DeepSeek/SambaNova;
  a native MCP Gateway (server multiplexing, tool include/exclude filtering, OAuth2
  + JWT claim forwarding, CEL-based fine-grained authz, per-tool observability); token-aware
  traffic management (separate cost attribution for input/output/cached/reasoning
  tokens, route-scoped costs, a QuotaPolicy API for quota-aware routing around rate-limited
  upstreams); centralized upstream credential management (API keys plus AWS SDK default
  credential chain -- IRSA, EKS Pod Identity, instance profiles); AI-native observability
  (OpenInference tracing + OpenTelemetry GenAI semantic conventions); and a stable
  v1 CRD surface: AIGatewayRoute, AIServiceBackend, BackendSecurityPolicy, GatewayConfig,
  MCPRoute, MCPRouteSecurityPolicy. Explicitly states what''s still roadmap, not shipped
  at v1.0: dollar-based (not just token-based) spend-limit cost governance, and deeper
  MCP authorization/identity (OIDC token exchange for MCP backends, finer-grained
  policy across tools/resources/prompts) -- confirming per-team budget caps and granular
  MCP identity were gaps as of the v1.0 GA date.'
utility_score: 13.0
---

[Accessibility Statement](https://www.cision.com/about/accessibility/)
[Skip Navigation](https://www.prnewswire.com/news-releases/envoy-ai-gateway-reaches-v1-0--establishing-the-open-source-standard-for-enterprise-ai-traffic-302808088.html#main)
**_Backed by contributions from Bloomberg, Nutanix, and Tetrate, v1.0 delivers production-grade routing, governance and extensibility for AI workloads built on the same proxy trusted by Databricks, Google, Lyft, Netflix, Spotify, and many others._**
SAN FRANCISCO, June 23, 2026 /PRNewswire/ -- [Tetrate](https://edge.prnewswire.com/c/link/?t=0&l=en&o=4717212-1&h=1952511111&u=https%3A%2F%2Ftetrate.io%2F&a=Tetrate), a primary upstream contributor to the Envoy open source project and a leader in enterprise-ready AI Gateways, today announced the v1.0 release of [Envoy AI Gateway](https://edge.prnewswire.com/c/link/?t=0&l=en&o=4717212-1&h=1794716512&u=https%3A%2F%2Faigateway.envoyproxy.io%2F&a=Envoy+AI+Gateway), the first open source AI gateway built on the Cloud Native Computing Foundation's Envoy Gateway project. The v1.0 milestone signals production maturity for the project and reflects contributions from maintainers at Bloomberg, Nutanix, Tetrate, and the broader Envoy community.
[Continue Reading ](https://www.prnewswire.com/news-releases/envoy-ai-gateway-reaches-v1-0--establishing-the-open-source-standard-for-enterprise-ai-traffic-302808088.html#continue-jump)
The World's Leading Application Networking and Security Platform  Envoy AI Gateway 
Production-stable Envoy AI Gateway has been hardened for production use by Tetrate, Bloomberg, and Nutanix across real workloads, providing an open source solution ready for the needs of enterprise workloads.
**Envoy: The Standard for Internet-Scale Traffic**[Created at Lyft in 2015](https://edge.prnewswire.com/c/link/?t=0&l=en&o=4717212-1&h=3472134203&u=https%3A%2F%2Feng.lyft.com%2Fannouncing-envoy-c-l7-proxy-and-communication-bus-92520b6c8191&a=Created+at+Lyft+in+2015), Envoy has been used in production for nearly a decade. By September 2016, Envoy was processing over 2 million requests per second across Lyft's infrastructure, and today handles millions of machine learning predictions daily. Netflix processes billions of API requests daily, while Airbnb handles over 1 million user events per second, with both organizations relying on Envoy for mission-critical traffic management. At least 44% of enterprises [now use Envoy in production environments](https://edge.prnewswire.com/c/link/?t=0&l=en&o=4717212-1&h=3104881172&u=https%3A%2F%2Fwww.cncf.io%2Freports%2Fcncf-annual-survey-2023%2F&a=now+use+Envoy+in+production+environments) or are evaluating Envoy for production, including AWS, Docker, SAP, Atlassian, and LY Corporation. Leading AI providers including Databricks, Google, Lyft, Netflix, and Spotify have documented their reliance on Envoy to manage AI inference traffic at scale.
Envoy AI Gateway extends this production-proven foundation with native support for agentic traffic. Where Envoy became the standard for internet traffic, Envoy AI Gateway is its modern adaptation for AI workloads, bringing the same reliability, extensibility and governance to a new class of infrastructure demands.
**v1.0: A Major Milestone Backed by Industry Partners** The path to v1.0 began when Bloomberg [proposed the effort](https://edge.prnewswire.com/c/link/?t=0&l=en&o=4717212-1&h=93705708&u=https%3A%2F%2Ftetrate.io%2Fpress%2Ftetrate-and-bloomberg-collaborate-on-community-led-open-standard-for-ai-gateways-built-on-cncfs-envoy-gateway-project&a=proposed+the+effort) through the Envoy Gateway community to address its own challenge: routing generative AI workloads with the same reliability and governance expected of Envoy. The project was [first released](https://edge.prnewswire.com/c/link/?t=0&l=en&o=4717212-1&h=2925904817&u=https%3A%2F%2Fwww.prnewswire.com%2Fnews-releases%2Ftetrate-and-bloomberg-release-open-source-envoy-ai-gateway-built-on-cncfs-envoy-gateway-project-302384023.html&a=first+released) in February 2025. Over the past 16 months, sustained collaboration among Tetrate, Bloomberg, Nutanix, and the open source community has produced a mature, stable codebase ready for broad adoption.
At Bloomberg, Envoy AI Gateway is in production, demonstrating that the same open source code available to the community is stable and scalable enough to power demanding enterprise workloads.
"We see the Envoy AI Gateway as a key element toward standardizing how enterprises securely and reliably serve AI workloads," said Dan Sun, Envoy AI Gateway and KServe co-founder/maintainer. "Bloomberg engineers have made hundreds of contributions to this project — in the spirit of our firm's commitment to scalable, open source AI infrastructure that brings vendor neutrality, consistency, observability and control to AI inference at scale."
"Envoy has become the foundational layer for internet traffic at the world's most demanding organizations. With v1.0, Envoy AI Gateway brings that same trust to AI workloads," said Varun Talwar, co-founder and CTO at Tetrate. "The code in the public repo is the same code running in production at Bloomberg and Tetrate. That level of transparency is rare in open source, and it's what enterprises need as they scale AI."
Nutanix is taking Envoy AI Gateway into production, including bug fixing and more. The company is incorporating the project into its Nutanix Agent Gateway and Nutanix Enterprise AI solutions.
"Nutanix is proud to be a maintainer and an active contributor in the Envoy AI Gateway community, helping bring the same transparency and production-grade reliability that powers enterprise Internet traffic to the next wave of AI workloads," said Debo Dutta, chief AI officer at Nutanix. "We are using the project's capabilities to bring transparent, multiprovider flexibility and production-ready AI infrastructure to our customers."
"At LY Corporation, we utilize the Envoy AI Gateway to manage our multi-tenant, self-hosted LLM traffic," said Shingo Omura, principal architect of AI infrastructure at LY Corporation.
"It provides a unified API for flexible routing, token-based rate limiting, authentication and authorization, monitoring and extensibility, aligned with open standards like the Kubernetes Gateway API Inference Extension. This allows us to maximize operational efficiency of our LLM platform. We believe version 1.0 is a major milestone for production-grade AI infrastructure engineering."
New capabilities in v1.0 are made possible by open source community collaboration among engineers working across a diversity of organizations and use cases:
**Unified API across every major AI provider —** A single OpenAI-compatible interface in front of OpenAI, Anthropic (direct, AWS Bedrock, and GCP Vertex), Google Gemini, Azure OpenAI, AWS Bedrock, and the long tail of OpenAI-compatible providers (Groq, Together, Mistral, Cohere, DeepSeek, SambaNova).
**Native MCP Gateway for the agentic era —** Production-grade Model Context Protocol routing — server multiplexing behind a single endpoint, tool filtering with include/exclude rules, OAuth 2.0 with JWT claim forwarding to backends, CEL-based fine-grained authorization, and per-tool observability.
**Token -aware traffic management — **Rate limiting, budgets, and quotas that understand AI workloads — separate cost attribution for input, output, cached, and reasoning tokens; route-scoped costs with fleet-wide defaults; quota-aware routing primitives (QuotaPolicy API) to route around rate-limited upstreams.
**Centralized upstream credential management —** One control plane for every provider's auth story — API keys for OpenAI, Anthropic, Azure; AWS SDK default credential chain (IRSA, EKS Pod Identity, instance profiles).
**AI-native observability —** OpenInference distributed tracing and OpenTelemetry GenAI semantic conventions across chat, embeddings, image generation, audio, MCP, and reasoning endpoints.
**Stable v1 API surface —** AIGatewayRoute, AIServiceBackend, BackendSecurityPolicy, GatewayConfig, MCPRoute, and MCPRouteSecurityPolicy graduate to v1 — production-stable CRDs hardened by Tetrate, Bloomberg, and Nutanix across real workloads.
The Envoy AI Gateway roadmap includes features that community members participating in the project are working on for the next version:
**Dollar-based control, not just tokens —** Cost governance that moves beyond token counts to actual spend limits.
**Deeper MCP authorization and identity —** Continued investment in agentic infrastructure, including backend security policy for MCP, support for OIDC token exchange when connecting to MCP backends and finer-grained policy across tools, resources and prompts.
**Closer alignment with the agentic AI ecosystem —** Strengthening collaboration with frontier model providers and the broader agentic AI community to ensure Envoy AI Gateway remains the open standard for AI traffic.
**Get Started, Get Involved** The Envoy AI Gateway community invites broader participation as the project enters its next phase of growth. Organizations are encouraged to contribute extensions, governance policies and new integrations.
  * Visit the project [website](https://edge.prnewswire.com/c/link/?t=0&l=en&o=4717212-1&h=3364371256&u=https%3A%2F%2Faigateway.envoyproxy.io%2F&a=website) and [download Envoy AI Gateway](https://edge.prnewswire.com/c/link/?t=0&l=en&o=4717212-1&h=1411732616&u=https%3A%2F%2Fgithub.com%2Fenvoyproxy%2Fai-gateway&a=download+Envoy+AI+Gateway).
  * Get started with the [Envoy AI Gateway installation guide](https://edge.prnewswire.com/c/link/?t=0&l=en&o=4717212-1&h=3251225281&u=https%3A%2F%2Faigateway.envoyproxy.io%2Fdocs%2Fgetting-started%2F&a=Envoy+AI+Gateway+installation+guide).
  * Visit Bloomberg's [open source](https://edge.prnewswire.com/c/link/?t=0&l=en&o=4717212-1&h=3849262476&u=https%3A%2F%2Fwww.bloomberg.com%2Fcompany%2Fvalues%2Ftech-at-bloomberg%2Fopen-source%2F&a=open+source) and [open source projects](https://edge.prnewswire.com/c/link/?t=0&l=en&o=4717212-1&h=3658002987&u=https%3A%2F%2Fwww.bloomberg.com%2Fcompany%2Fvalues%2Ftech-at-bloomberg%2Fopen-source%2Fprojects%2F&a=open+source+projects) pages.
  * Visit Nutanix's gateway blog [here](https://edge.prnewswire.com/c/link/?t=0&l=en&o=4717212-1&h=3364578649&u=https%3A%2F%2Fwww.nutanix.com%2Fblog%2Fintroducing-nutanix-agent-gateway&a=here).


**About Envoy AI Gateway** Envoy AI Gateway extends CNCF's Envoy Gateway with native support for generative AI traffic, offering routing, failover, observability and compliance features. The project is community-driven and open source under the CNCF umbrella. Learn more at [aigateway.envoyproxy.io](https://edge.prnewswire.com/c/link/?t=0&l=en&o=4717212-1&h=3568642709&u=https%3A%2F%2Faigateway.envoyproxy.io%2F&a=aigateway.envoyproxy.io).
**About Tetrate** Tetrate is a primary upstream contributor to the Envoy project and a leader in its technical development. Tetrate enables safe, fast and profitable AI transformation by providing trusted connectivity in AI operations. As the driving force behind Envoy and Envoy AI Gateway, Tetrate builds on proven open source foundations to deliver solutions fit for regulated industries and government organizations. Tetrate's Agent Router Enterprise, built on Envoy AI Gateway, gives enterprises the standards, guardrails and evaluation metrics needed to run Envoy AI Gateway with full governance, failover and optimization capabilities. This is why some of the largest financial institutions, governments and other enterprises rely on Tetrate to deliver modern application networking and security. Learn more at [tetrate.io](https://edge.prnewswire.com/c/link/?t=0&l=en&o=4717212-1&h=309686069&u=https%3A%2F%2Ftetrate.io%2F&a=tetrate.io).
**MEDIA CONTACT:** Cristin ConnellyCathey.co for Tetratecristin@cathey.co
SOURCE Tetrate
## [21% more press release views with  Request a Demo  ](https://www.prnewswire.com/amplify-platform/?site_refer=press-release-widget)
### Modal title
## Also from this source
### [Tetrate Adds Token Brokering Capability for AI Code Gen Cost Management Through Agent Router Enterprise  Soaring token costs are pushing engineering teams to manage spend, and Tetrate is rolling out a new capability to solve the problem as part of its... ](https://www.prnewswire.com/news-releases/tetrate-adds-token-brokering-capability-for-ai-code-gen-cost-management-through-agent-router-enterprise-302826151.html)
### [Tetrate and Ory Bring Dynamic, Granular Control to Enterprise AI Agents  Tetrate and Ory today announced a strategic partnership to help enterprises secure AI agents in production. The joint solution combines Ory's... ](https://www.prnewswire.com/news-releases/tetrate-and-ory-bring-dynamic-granular-control-to-enterprise-ai-agents-302790416.html)
[ More Releases From This Source ](https://www.prnewswire.com/news/tetrate/)
## Explore
[Artificial Intelligence](https://www.prnewswire.com/news-releases/consumer-technology-latest-news/artificial-intelligence-list/)
[The Latest Artificial Intelligence News](https://www.prnewswire.com/news-releases/latest-news-topics/artificial-intelligence/)
[Computer & Electronics](https://www.prnewswire.com/news-releases/business-technology-latest-news/computer-electronics-list/)
[Computer Software](https://www.prnewswire.com/news-releases/business-technology-latest-news/computer-software-list/)
[ News Releases in Similar Topics ](https://www.prnewswire.com/news-releases/)
## We value your privacy
We use cookies to personalize content and ads, to provide social media features and to analyse our traffic. We also share information about your use of our site with our social media, advertising and analytics partners.[Cookie Policy](https://gdpr.cision.com/).
Accept All Manage Preferences
