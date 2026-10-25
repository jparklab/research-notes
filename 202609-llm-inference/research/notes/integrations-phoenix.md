---
title: Integrations - Phoenix
id: integrations-phoenix
tags:
- internal-llm-inference-stacks-7f0c90
- locus-observability-layer-oss-maturity
created: '2026-10-10T00:00:13.921360Z'
updated: '2026-10-10T14:52:17.258872Z'
source: https://arize.com/docs/phoenix/integrations
source_domain: arize.com
fetched_at: '2026-10-10T00:00:13.882395Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: docs
deprecated: false
summary: '> Fetch the complete documentation index at: /llms.txt'
---

> ## Documentation Index
> Fetch the complete documentation index at: [/llms.txt](https://arize.com/llms.txt)
> Use this file to discover all available pages before exploring further.
[Skip to main content](https://arize.com/docs/phoenix/integrations#content-area)
Phoenix integrates with the leading AI frameworks, LLM providers, and tools to provide seamless observability, evaluation, and debugging for your AI applications. Whether you’re building with Python, TypeScript, or Java, Phoenix has you covered.
Don’t see an integration you need? We’d love to [hear from you!](https://github.com/Arize-ai/openinference/issues/new/choose)
##  Integration Types
Phoenix offers several types of integrations to support your AI development workflow:
## [Developer Tools Integrate Phoenix with AI coding assistants like Claude Code and Cursor for debugging and analysis. ](https://arize.com/docs/phoenix/integrations#developer-tools)
## [Tracing Integrations Automatically capture traces from your AI applications built with popular frameworks and LLM providers. ](https://arize.com/docs/phoenix/integrations#tracing-integrations)
## [Eval Model Integrations Use any LLM provider to power Phoenix’s evaluation capabilities for scoring and classifying your traces. ](https://arize.com/docs/phoenix/integrations#eval-model-integrations)
## [Evaluation Integrations Record results from evaluation frameworks and libraries such as Harbor, Ragas, Cleanlab, and MLflow. ](https://arize.com/docs/phoenix/integrations#evaluation-integrations)
## [Span Processors Convert traces from other instrumentation libraries to the OpenInference format. ](https://arize.com/docs/phoenix/integrations#span-processors)
## [Sandboxes Run code evaluators in hosted sandbox providers with kernel-level isolation. ](https://arize.com/docs/phoenix/integrations#sandboxes)
##  Developer Tools
Integrate Phoenix with AI coding assistants to debug and analyze your LLM applications directly from your development environment.
## [Coding Agents Install Phoenix debugging skills and CLI for Claude Code, Cursor, and other AI coding assistants. ](https://arize.com/docs/phoenix/integrations/developer-tools/coding-agents)
## [MCP Server Connect AI assistants to your Phoenix data and docs via the Model Context Protocol. ](https://arize.com/docs/phoenix/integrations/mcp)
##  Coding Agents
Trace your sessions with a coding agent — turns, tool calls, and token costs — in Phoenix, no application code changes required. Install the [coding-harness-tracing](https://github.com/Arize-ai/coding-harness-tracing) toolkit and pick your agent:
##  Tracing Integrations
Phoenix captures detailed traces from your AI applications, giving you visibility into every step of your LLM pipeline.
###  By Language
  * Python
  * TypeScript


###  LLM Providers
Phoenix provides native tracing support for all major LLM providers:
###  Platforms
Integrate Phoenix with AI development platforms and infrastructure:
###  Vector Databases
Trace retrieval from vector databases to see which stage of a search to fix:
##  Eval Model Integrations
Phoenix’s evaluation library (`phoenix-evals`) can use any LLM provider to power evaluations. These models score, classify, and analyze your traces.
##  Evaluation Integrations
Connect external evaluation frameworks and libraries to Phoenix:
##  Span Processors
Normalize and convert data from other instrumentation libraries by adding span processors that unify traces to the OpenInference format:
##  Sandboxes
Run Phoenix [code evaluators](https://arize.com/docs/phoenix/evaluation/server-evals/code-evaluators) in hosted sandbox providers for kernel-level isolation, runtime dependency installation, and opt-in outbound network access. Configure providers from [Settings → Sandboxes](https://arize.com/docs/phoenix/settings/sandboxes).
Was this page helpful?
Assistant
Responses are generated using AI and may contain mistakes.
This website stores cookies on your computer. These cookies are used to collect information about how you interact with our website and allow us to remember you. We use this information in order to improve and customize your browsing experience and for analytics and metrics about our visitors both on this website and other media. To find out more about the cookies we use, see our Privacy Policy
If you decline, your information won’t be tracked when you visit this website. A single cookie will be used in your browser to remember your preference not to be tracked.
Cookies settings
AcceptDecline
