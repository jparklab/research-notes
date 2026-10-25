---
title: Designing MCP Gateway Uber's MCP Management Platform
id: designing-mcp-gateway-ubers-mcp-management-platform
tags:
- internal-llm-inference-stacks
- uber
- mcp
- agent-tooling
- first-person-account
created: '2026-10-09T23:00:26.519969Z'
updated: '2026-10-10T14:52:17.151564Z'
source: https://www.uber.com/blog/designing-mcp-gateway/
source_domain: www.uber.com
fetched_at: '2026-10-09T23:00:26.498722Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Uber engineering blog (Oct 1 2026) on the MCP Gateway, a custom-built microservice
  orchestrating Model Context Protocol tool access for AI agents at Uber -- this is
  an agent-tool-orchestration layer, not an LLM inference-serving stack, and the post
  names no inference engine, model-serving framework, or GPU scheduling component
  (flagged: does not update the stale 2024 GenAI Gateway inference-stack description;
  it is a related but distinct system). Scale: hosts 800+ MCP servers and 5,000+ tools.
  Architecture: MCP Registry (control plane, catalog/governance) + Proxy Gateway (data
  plane, protocol translation). AutoCrawler, a Cadence-powered (OSS workflow engine,
  cadenceworkflow.io) distributed workflow, continuously scans Uber''s internal IDL
  (Protobuf/Thrift) registry to auto-generate MCP server/tool definitions, using an
  LLM to write agent-friendly tool descriptions from schemas/comments -- tools register
  disabled-by-default pending owning-team review. Native MCP servers are built via
  Uber''s in-house MCPFx framework and auto-discovered via heartbeat signals. Downstream
  protocol execution (HTTP/gRPC/TChannel) is delegated to Muttley, Uber''s existing
  service-mesh sidecar, reusing existing service-to-service routing rather than building
  new transport. At scale, Uber built three additional in-house mechanisms to solve
  context-window/cost blowup from exposing hundreds of servers to agents: Omni MCP
  (a single proxy enabling incremental/gradual tool discovery via discover_server/discover_tools/get_tool_schema/invoke_tool,
  to avoid loading all tool schemas into context at once), Response Projection (a
  GraphQL-like field-selection mechanism letting the LLM request only needed response
  fields, trimmed server-side), and Code Mode (routes MCP calls through Uber''s aifx
  CLI so coding agents write tool output to files instead of loading it into model
  context; stated to be ''now the company default for MCP tool use in coding agents'').
  Security: built-in per-tool authorization via Uber''s internal Attribute-Based Access
  Control system, plus out-of-the-box PII/sensitive-data redaction on tool responses.'
---

*Suggested by [[navigating-the-llm-landscape-ubers-innovation-with-genai-gateway]] — recent (Oct 2026) Uber engineering post on their MCP management platform, linked as related content, likely updates the stale 2024 GenAI Gateway stack description*

[Skip to main content](https://www.uber.com/us/en/blog/designing-mcp-gateway/#main)
October 1, 2026
# Designing MCP Gateway Uber's MCP Management Platform
Alok Srivastava
Principal Engineer
Deepanshu Mehndiratta
Senior Staff Engineer
Gaurav Gill
Sr. Software Engineer
5+
Share this article
FacebookLinkedinX socialLink
## Introduction
Uber’s rapid adoption of AI agents has fundamentally changed how teams interact with code, data, and operational systems. Early ad-hoc integrations with the MCP (Model Context Protocol) showed clear value: agents became dramatically more capable when they could access live business context, query internal services, and take meaningful actions on behalf of users. These early wins validated MCP as a powerful abstraction for building agentic systems inside Uber.
However, as adoption accelerated, significant challenges emerged. Individual teams independently built integrations, leading to fragmentation in tooling and duplicated infrastructure. MCP tools were hard to discover, difficult to operate reliably, and tightly coupled to specific services or agent implementations. While these approaches worked at a small scale, they didn’t meet Uber’s needs as hundreds of teams began exploring agentic workflows. Without a unified architecture, scaling MCP would increase operational complexity, security risk, and developer friction, ultimately limiting its impact. 
To unlock MCP’s full potential at Uber’s scale, we needed a centralized, scalable solution to standardize how AI agents interact with existing back-end systems while preserving flexibility for teams. This solution needed to abstract protocol differences (HTTP, gRPC _™_ , TChannel), enforce consistent security and observability guarantees, and make MCP tools easy to create, discover, and reuse across the company.
We built the MCP Gateway to address this need. It’s a foundational microservice that powers all MCP interactions at Uber, serving as an orchestration and routing layer between AI agents and existing back-end services and native MCP servers. By centralizing MCP logic into a single gateway, we provide a consistent execution model for agent-to-service interactions while eliminating the need for teams to reinvent core infrastructure. Existing APIs can be seamlessly exposed as MCP tools, governed and operated in one place, and consumed by multiple agents in a uniform way. MCP Gateway unlocked a scalable, fast, and consistent path to building AI agents within Uber and is currently hosting over 800 MCP servers and over 5000 tools.
Figure 1: MCP Gateway (APIs as tools). 
In this blog, we review the design of the MCP Gateway, covering the Proxy Layer (translation of MCP to existing protocols and back), Discovery Layer (MCP Registry and API crawling) and its control plane (authoring).
### The Gateway
MCP Gateway follows a microservice-based architecture, with the gateway acting as the central integration point between AI-enabled systems and Uber’s back-end services. The platform comprises two primary components: the MCP Registry, which serves as the control plane, and the Proxy Gateway, which forms the data plane.
The MCP Registry maintains a catalog of hundreds of MCP servers backed by internal services, along with thousands of MCP tools. These tools range from no-code definitions that expose existing APIs as MCP tools to fully native implementations built explicitly against the MCP specification. The registry provides a single source of truth for discovery, ownership, and enablement across the ecosystem.
The Proxy Gateway is responsible for executing MCP requests at runtime. It translates MCP protocol calls into HTTP, gRPC, or TChannel requests, forwards them to the appropriate back-end service, and converts the responses back into MCP-compatible results. This translation layer allows AI agents to interact with existing systems through a consistent MCP interface, without requiring changes to the underlying services.
### Control Plane
Uber has a microservice architecture and operates with thousands of internal services that expose APIs over HTTP, gRPC, and TChannel. These APIs provide valuable context to an AI system, but asking teams to manually author an MCP server would be slow and painful. To solve this problem, we built AutoCrawler, which continuously scans Uber’s IDL registry for APIs, translates them, and updates them in the registry. It also queries native MCP servers and adds them to the registry.
#### AutoCrawler: Discovery Engine
Autocrawler is a [Cadence](https://cadenceworkflow.io/)-powered distributed workflow system subscribed to Uber’s IDL registry and internal service signals. On a fixed schedule, a cron job triggers a Cadence workflow that scans for newly added services, APIs, and schema changes.
For every discovered entity, AutoCrawler is responsible for:
  * Creating or updating MCP server representations
  * Generating or fetching tool definitions and schemas
  * Registering tools in the MCP Registry in a disabled-by-default state


This shared foundation enables MCP discovery to scale across thousands of services while keeping service teams off the critical path.
#### 
Figure 2: Auto Crawler. 
Discovery for IDL-Backed Services
For traditional back-end services defined through Protobuf or Thrift IDLs, AutoCrawler derives MCP servers and tools directly from the IDL Registry. For each service:API group, AutoCrawler performs the following steps:
  * **Upsert MCP server:** Creates or updates a virtual MCP server corresponding to the discovered service.
  * **Parse IDL definitions:** Parses the associated protobuf or Thrift files to extract method names, request and response schemas, and documentation comments.
  * **Generate tool descriptions:** Uses an LLM to generate enriched, agent-friendly MCP tool descriptions based on the extracted schemas and comments.
  * **Schema translation:** Translates protobuf or Thrift schemas into MCP-compatible JSON-RPC 2.0 schemas.
  * **Upsert MCP tools:** Registers or updates the generated MCP tools in the MCP Registry in a disabled-by-default state.


#### Discovery for Native Servers
In addition to IDL-backed services, the MCP Gateway also supports native MCP servers—services that implement the MCP protocol directly and expose agent-optimized tools.
MCPFx is the framework Uber uses to build native MCP servers. Each native MCP server emits a heartbeat metric that signals its presence and readiness. AutoCrawler continuously monitors these heartbeat signals to automatically discover new native MCP servers. When a native MCP server is discovered, AutoCrawler follows a different discovery path:
  1. It makes a _listTools_ call to the native MCP server to retrieve the tools it explicitly exposes, along with their schemas.
  2. It creates a virtual proxy MCP server in the MCP Registry that contains all discovered tools and their schemas, in a disabled-by-default state.


### 3P MCP Servers
The MCP Gateway serves as the centralized orchestration layer for all MCP interactions across Uber, extending seamless support to third-party integrations such as Jira and Google. 
Provisioning third-party MCP servers relies on the co-operation of two key components:
  1. **MCP Gateway:** Relays the caller's user token downstream while enforcing essential gateway capabilities, including authorization, rate limiting, and sensitive data redaction.
  2. **Third-Party MCP Service:** Exchanges the internal user token for a corresponding third-party authentication token before dispatching the request to the external MCP server.


#### Authoring and Enablement
While we may create MCP servers without involving the service team, ownership and control of the MCP server must reside with the service team. A core design principle of the MCP Gateway is that discovery doesn’t imply exposure. Every MCP server and tool starts in a disabled state and must be explicitly reviewed and enabled by the owning team. Service owners can review and refine the generated tool definitions before enabling them. 
Every change to the tool description triggers a config change diff, which must be approved by server owners. Owners can approve and deploy the config change, and, if needed, roll back to a previous known version.
Figure 3: MCP Registry UI.
#### 
Figure 4: MCP tool UI.
Data Plane
The MCP Gateway data plane is the core runtime service responsible for executing MCP requests. It continuously consumes server and tool configurations from the control plane and refreshes its in-memory state at a fixed cadence, allowing configuration changes, such as tool updates or enablement changes, to take effect in real time without service restarts or redeployments. 
Based on this configuration, the data plane dynamically materializes virtual MCP servers. For each virtual server, the Gateway exposes a single _/ <service-name>/mcp_ endpoint that serves as the entry point for AI agent execution. Incoming requests are resolved to their corresponding server handlers through a built-in proxy server.
#### 
Figure 5: MCP Gateway Data Plane. 
Protocol Translation and Execution
Protocol translation in the MCP Gateway is handled by server handlers within the Proxy Gateway. Each server handler is tool-aware and downstream-aware, allowing it to correctly route and execute MCP requests at runtime.
#### Security
MCP Gateway provides built-in authorization and redaction for all servers at tool-level granularity. MCP gateway uses Uber’s internal [Access Control System](https://www.uber.com/us/en/blog/attribute-based-access-control-at-uber/?_csid=B1oPUnHmEXf8Dhixs6S_nw&state=-fWBWtMXKTwophlLntpUZxHQvUzvjpG_7-var0iwiYg%3D&effect=&sm_flow_id=TXl1jZZg) to apply different charter policies configured on detected caller actors (humans, services, and agents). Charter policies are created at the server level with optional tool-level overrides if required.
MCP Gateway also does out-of-the-box redaction for any PII or sensitive data from the tool responses. 
#### IDL-Backed Downstream Services
For tools backed by existing back-end services, the server handler maintains an in-memory mapping that describes the downstream destination, such as HTTP endpoint config or gRPC/TChannel procedures. 
When an MCP request arrives, the handler:
  1. Translates the incoming JSON payload into the appropriate wire format.
  2. Serializes the request into Protobuf or Thrift bytes.
  3. Forwards the request to the downstream service.
  4. Translates the Protobuf or Thrift byte responses back to MCP-compatible JSON, and returns to the calling agent.


The actual downstream request is executed via Muttley, Uber’s service mesh sidecar that runs alongside all back-end services. By delegating request execution to Muttley, the MCP Gateway automatically benefits from existing service-to-service routing capabilities. 
#### Native MCP Servers
Native MCP servers are also registered as virtual servers in the MCP registry, which acts as a proxy to the original server. At runtime, ‌native MCP requests are transparently proxied to the downstream server and responses are proxied back to the caller. 
### Benefits of the Gateway
By building the MCP-Gateway, Uber has achieved a scalable and unified approach towards building agentic systems, with most impactful benefits coming from:
  * Easy discovery and installation
  * No-code approach towards existing APIs
  * Built-in observability and security
  * Centralized ownership and governance 


## Extending the Gateway
Scaling MCP Gateway to hundreds of servers and thousands of tools revealed problems that don’t exist at small scale. Context Bloat and Excessive Cost
### Runtime Discovery
MCP has no native concept of cross-server search. An agent has to already know which server to talk to before it can ask what tools are available. Configuring an agent to use an MCP server requires explicitly wiring up the server URL, credentials, and tool list. Doing this for hundreds of servers doesn’t scale, as all this context would eat up the model context limit. We solved this problem with the following 
  * **Omni MCP** - A single proxy server that allows MCP clients to access any of the MCP Gateway’s servers with a gradual discovery pattern, which also unlocks context/token optimization via incremental discovery. Omni MCP exposes these tools.
  *     * **discover_server** - discover MCP server based on the query intent
    * **discover_tools** - lookup tools for a server
    * **get_tool_schema** - get the json schema for a tool
    * **invoke_tool** - invoke a tool


Together, these tools enable incremental discovery and access to all MCP servers, with built-in access control and the rest of the gateway features.
  * **Response Projection -** MCP Gateway also offers Response Projection, a GraphQL-like calling pattern for MCP tools. It works by injecting a new field in the tool request schema, which instructs the gateway to request only the needed fields, not all. The LLM reads and injects the fields with an array of nested paths of only required fields. The Gateway then trims the response in runtime by only keeping the projected fields. This has allowed us to scale the API schema compatibility for MCP at the enterprise level.
  * **Code Mode** - Coding agents often operate in shell environments where writing tool output directly to files is more efficient than loading full responses into model context. Code Mode serves this pattern through aifx, Uber's CLI for agentic operations, routing MCP calls through the gateway without requiring any MCP server to be installed. It helps agents discover correct MCP tools for the job without the MCP definition being present in context. aifx exposes three commands:
  *     * aifx mcp list - list available MCP servers
    * aifx mcp search - search for tools across all MCP servers
    * aifx mcp call - invoke an MCP tool through MCP Gateway


Agents can chain these in a single command and write output to files, which filesystem agents grep selectively, loading only what they need into context. Code Mode is now the company default for MCP tool use in coding agents.
###  Conclusion
Building the MCP Gateway has fundamentally changed how AI agents operate at Uber. What began as a fragmentation problem, with dozens of teams independently wiring up MCP integrations with inconsistent tooling, no shared security guarantees, and duplicated infrastructure, is now a unified, scalable platform that any team can plug into in minutes. 
The core insight driving our design was simple: existing APIs are the fastest way to provide tools to an agent. Rather than asking teams to rewrite their services for an agentic world, MCP Gateway meets them where they are—translating HTTP, gRPC, and TChannel calls into MCP-compatible interactions transparently, through Muttley, with zero changes to downstream services.
If you’re building agentic systems at scale, the hardest part isn’t the AI. It’s building the connective tissue—the discovery, the security, the reliability that makes agents trustworthy enough to act on behalf of real users in a production environment. MCP Gateway is our answer to that challenge, and we hope the design decisions documented here are useful to others facing the same problem.
## Acknowledgments
Cover Photo Attribution: Generated with ChatGPT by OpenAI; no external images, logos, or third-party assets used.
_gRPC is a trademark of The Linux Foundation._
Stay up to date with the latest from Uber Engineering—follow us on [LinkedIn](https://p.uber.com/eng-linkedin) for our newest blog posts and insights.
Written by
Alok Srivastava
Principal Engineer
Alok Srivastava is a Principal Engineer on Uber's Business Platform team. He leads Uber's Edge Platform, the ingress and egress tier for Uber's business traffic, spanning APIs, content, and push messaging across all mobile and web surfaces.
Deepanshu Mehndiratta
Senior Staff Engineer
Deepanshu Mehndiratta is a Senior Staff Engineer in Uber's Business Platform org, where he leads Reliability and AI Engineering. His AI work spans the MCP Gateway and Uber's frontier deep-agent ecosystem, connecting all Uber services to AI agents and leveraged by tens of thousands of employees.
Gaurav Gill
Sr. Software Engineer
Gaurav Gill is a Sr. Software Engineer on the Edge Platform Team within Business Platform at Uber building scalable Agentic AI Infrastructure at Uber. His work includes the design and development for MCP Gateway and Deep Agent Ecosystem.
Prashik Sahare
Senior Software Engineer
Prashik Sahare is a Senior Software Engineer on Uber’s Business Platform team, working on AI infrastructure and Edge Streaming. Alongside the MCP Gateway, he’s built the Skills Marketplace that lets teams author, share, and reuse agentic skills across the organization.
Vaibhav Tayal
Software Engineer II
Vaibhav Tayal is an Software Engineer II at Uber on the Edge Gateway team, where he leads frontend development across multiple projects. He designed and launched the MCP Gateway Dashboard and developed the GraphQL Persisted Operations management system.
Shiven Tripathi
Software Engineer II
Shiven Tripathi is a Software Engineer II on Uber's Business Platform team, focused on AI infrastructure and reliability engineering. He helped build and scale MCP Gateway and works on Uber's frontier deep-agent ecosystem.
Uday Kiran Medisetty
Distinguished Engineer
Uday Kiran Medisetty is a Distinguished Engineer at Uber, where he leads initiatives for engineering productivity (agentic coding, AI debugging, code reviews, and large-scale refactoring). He also co-leads the company-wide engineering community shaping Uber's architecture, culture, and standards.
Abhishek Bhatia
Staff Software Engineer
Abhishek Bhatia is a Staff Software Engineer on Uber's Ads team, focused on recommendation systems for Uber Eats merchants. Beyond his product work, he contributes to Uber's AI developer tooling, including code-mode, Uber's default MCP discovery and execution layer in aifx.
Category
Related Articles
6 articles
AI / ML
Engineering
[Scaling AI in Legal: Building Uber’s Redlining Agent](https://www.uber.com/us/en/blog/building-ubers-redlining-agent/)
October 8, 2026
AI / ML
Engineering
[Taming the ML Firehose: Scaling Feature Consistency](https://www.uber.com/us/en/blog/taming-ml-firehose/)
September 22, 2026
Backend
Engineering
[How Uber Protects Against Retry Storms](https://www.uber.com/us/en/blog/protecting-against-retry-storms/)
September 17, 2026
Backend
Engineering
[Large-Scale Automated Dependency Analysis Across Uber's Service Mesh](https://www.uber.com/us/en/blog/automated-dependency-analysis/)
September 15, 2026
Backend
Engineering
[Halving the Time: How Uber Eats Rebuilt Its Search Pipeline](https://www.uber.com/us/en/blog/uber-eats-search-pipeline/)
September 10, 2026
Backend
Engineering
[From One Horizontal Scaling Controller to Many: Evolving Uber's Compute Platform](https://www.uber.com/us/en/blog/evolving-ubers-compute-platform/)
September 9, 2026
## Select your preferred language
[English, English](https://www.uber.com/us/en/blog/designing-mcp-gateway/)
  * ### Products
    * [Advertising Learn more about advertising on Uber. Reach consumers as they go anywhere and get anything.](https://www.uber.com/us/en/blog/advertising/)
    * [Earn Resources for driving and delivering with Uber](https://www.uber.com/us/en/blog/earn/)
    * [Ride Experiences and information for people on the move](https://www.uber.com/us/en/blog/ride/)
    * [Eat Ordering meals for delivery is just the beginning with Uber Eats](https://www.uber.com/us/en/blog/eat/)
    * [Merchants Putting stores within reach of a world of customers](https://www.uber.com/us/en/blog/merchants/)
    * [Business Transforming the way companies move and feed their people](https://www.uber.com/us/en/blog/business/)
    * [Freight Taking shipping logistics in a new direction](https://www.uber.com/us/en/blog/freight/)
    * [Health Moving care forward together with medical providers](https://www.uber.com/us/en/blog/health/)
    * [Higher Education Enhancing campus transportation](https://www.uber.com/us/en/blog/higher-education/)
    * [Transit Expanding the reach of public transportation](https://www.uber.com/us/en/blog/transit/)
  * ### Company
    * [Engineering The technology behind Uber Engineering](https://www.uber.com/us/en/blog/engineering/)
    * [Community support Doing the right thing for cities and communities globally](https://www.uber.com/us/en/blog/community-support/)
    * [Newsroom Uber news and updates in your country](https://www.uber.com/us/en/newsroom/)
    * [Uber.com Product, how-to, and policy content—and more](https://www.uber.com/)


## Select your preferred language
[English, English](https://www.uber.com/us/en/blog/designing-mcp-gateway/)
## [Drive & deliver](https://drivers.uber.com/)
## [Uber Eats](https://ubereats.com/login-redirect/)
## [Business](https://business.uber.com/)
## [Drive & deliver](https://drivers.uber.com/)
## [Uber Eats](https://ubereats.com/login-redirect/)
## [Uber for Business](https://business.uber.com/)
## [Manage account](https://account.uber.com/)
This website uses third party cookies to serve you relevant ads and provide personalization by remembering your location. You may opt out from these cookies by selecting the "Opt out" button below. If you have an Uber account, you may opt out of the "sale" or "sharing" of your data [here](https://privacy.uber.com/us).
Opt outGot it
