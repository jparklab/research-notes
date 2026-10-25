---
title: 'The Experimentation Phase Is Over: Key Findings from 1,200 Production Deployments
  - ZenML Blog'
id: the-experimentation-phase-is-over-key-findings-from-1200-production-deployments
tags:
- llmops-aggregate
- audited-absence
created: '2026-10-09T23:00:11.361904Z'
updated: '2026-10-10T14:52:17.145180Z'
source: https://www.zenml.io/blog/the-experimentation-phase-is-over-key-findings-from-1-200-production-deployments
source_domain: www.zenml.io
fetched_at: '2026-10-09T23:00:11.340199Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'ZenML''s condensed executive-summary companion (Dec 19, 2025, ~1,545 words)
  to the longer ''What 1,200 Production Deployments Reveal'' post -- same six themes
  (engineering fundamentals over AI skill, context engineering, infrastructure guardrails,
  evaluation maturity, MCP stabilisation, not waiting for frontier models) in abbreviated
  form, same named case studies (Stripe, nib, Ramp, Loblaws, Sentry, Digits, Cox Automotive,
  Robinhood, Care Access). AUDITED ABSENCE confirmed: like its longer companion, this
  piece contains no aggregate percentage-of-deployments statistic with stated denominator/methodology
  for which inference engines (vLLM/SGLang/TensorRT-LLM/etc.), API gateways, or orchestration
  frameworks organizations actually use -- despite synthesizing patterns across 1,200
  catalogued case studies. ZenML''s LLMOps-database aggregate content, across both
  posts fetched in this batch, never quantifies inference-stack-component prevalence;
  any ''X% of deployments use Y'' figure for this research topic will have to come
  from a different corpus (e.g. a16z/vendor survey, Stack Overflow/JetBrains developer
  surveys, or direct tally of the database''s own tag taxonomy) rather than from ZenML''s
  own narrative posts.'
---

*Suggested by [[what-1200-production-deployments-reveal-about-llmops-in-2025-zenml-blog]] — linked companion executive-summary post that may carry aggregate prevalence statistics*

December 19, 2025·3 mins
# The Experimentation Phase Is Over: Key Findings from 1,200 Production Deployments
Analysis of 1,200 production LLM deployments reveals six key patterns separating successful teams from those stuck in demo mode: context engineering over prompt engineering, infrastructure-based guardrails, rigorous evaluation practices, and the recognition that software engineering fundamentals—not frontier models—remain the primary predictor of success.
On this page
The LLMOps Database crossed 1,200 case studies this month. Since I last published a summary, I’ve catalogued another 400 production deployments: real systems handling real traffic, built by teams who’ve learned the hard way what actually works when the on-call engineer is asleep.
This post distils the key findings. I begin by examining why general engineering maturity remains the primary predictor of success. From there, I explore six specific trends defining the current landscape: the shift to context engineering, the migration of guardrails into infrastructure, the rise of “shadow testing,” and the stabilizing role of the Model Context Protocol (MCP).
For the full analysis with detailed case studies across all themes, [[read the complete report →]](https://www.zenml.io/blog/what-1200-production-deployments-reveal-about-llmops-in-2025)
## The Core Finding: Engineering Remains Central
A consistent pattern emerges across 1,200 case studies. Teams shipping reliable production LLM systems rely heavily on robust software engineering fundamentals. While AI-specific skills like context engineering are vital, they operate best when supported by solid infrastructure.
Generative AI does not bypass traditional engineering disciplines. Indeed, the challenge often lies less in model capability and more in the infrastructure required to deploy that capability reliably. Distributed systems, networking, durable execution, and state management provide the necessary foundation. These core skills allow specialized AI techniques to function at enterprise scale.
[LinkedIn’s AI team](https://www.zenml.io/llmops-database/building-and-scaling-a-production-generative-ai-assistant-for-professional-networking) noted that while prompt engineering involves significant nuance, the surrounding system demands rigorous engineering. Building reliable products requires async pipelines, streaming response handling, and capacity management alongside model optimization.
[GetOnStack’s experience](https://www.zenml.io/llmops-database/production-deployment-challenges-and-infrastructure-gaps-for-multi-agent-ai-systems) illustrates this balance. Their multi-agent system costs escalated from $127 to $47,000 in weekly costs over four weeks due to an undetected infinite conversation loop. They subsequently invested six weeks in building message queues, circuit breakers, and monitoring. These standard distributed systems components proved essential for controlling their AI agents.
Once this engineering foundation is established, however, distinct patterns emerge in how teams handle the unique challenges of LLMs. Beyond the basics of reliability and uptime, specific architectural choices set successful deployments apart.
## Six Themes Emerging from Production
### 1. From Demos to Real Business Outcomes
The proof-of-concept era is visibly ending. The database now regularly tracks systems handling genuinely critical business functions with quantified impact.
[Stripe’s fraud detection](https://www.zenml.io/llmops-database/building-economic-infrastructure-for-ai-with-foundation-models-and-agentic-commerce) improved card-testing accuracy from 59% to 97% for their largest merchants, on infrastructure processing 1.3% of global GDP. [nib’s chatbot ](https://www.zenml.io/llmops-database/transforming-agent-and-customer-experience-with-generative-ai-in-health-insurance)generates $22 million in documented savings across 4 million interactions. [Ramp’s policy agent](https://www.zenml.io/llmops-database/building-trustworthy-llm-agents-for-automated-expense-management) handles 65% of expense approvals autonomously.
The organisations extracting real value aren’t the ones with the most innovative demos. They’re the ones doing the unglamorous engineering work: evaluation pipelines, guardrails, designing for uncertainty.
### 2. Context Engineering > Prompt Engineering
If 2023 was the year of prompt engineering (learning how to talk to models), 2024-2025 have marked the rise of context engineering: learning how to architect the information models consume.
The core insight: just because you _can_ fit everything into a million-token context window doesn’t mean you _should_. [Manus found](https://www.zenml.io/llmops-database/context-engineering-for-production-ai-agents-at-scale) that “context rot” begins between 50k–150k tokens regardless of theoretical limits. [Dropbox](https://www.zenml.io/llmops-database/context-engineering-for-agentic-ai-systems) encountered “analysis paralysis” when exposing too many tools. The model spent more time deciding which tool to use than actually acting.
The winning pattern is “just-in-time” context injection: dynamically assembling only what’s needed based on the user’s immediate state. [Shopify](https://www.zenml.io/llmops-database/building-production-ready-ai-assistant-with-agentic-architecture) collocates instructions with tool outputs rather than loading everything upfront. [Elyos AI](https://www.zenml.io/llmops-database/building-low-latency-voice-ai-agents-for-home-services) actively removes context that’s served its purpose, calling it “just-in-time in, just-in-time out.”
Leaner contexts make models smarter, along with being faster and cheaper. The million-token window is a ceiling to stay under, rather than a feature to exploit.
### 3. Guardrails Move from Prompts to Infrastructure
The limitations of prompt-based guardrails are now well understood. As [Oso’s framework](https://www.zenml.io/llmops-database/production-ready-agent-behavior-identity-intent-and-governance) puts it: “what 1997 was for SQL injection, 2025 is for prompt injection.”
The most reliable guardrails are implemented in code, not prompts. Oso’s “session tainting” automatically blocks agents from using communication tools after they’ve touched both untrusted content and sensitive data, regardless of what the model tries to do. [Wakam](https://www.zenml.io/llmops-database/enterprise-scale-ai-agent-deployment-in-insurance) implements dual-layer permissions: one controlling what humans see, another controlling what agents access. [Komodo Health](https://www.zenml.io/llmops-database/building-a-multi-agent-healthcare-analytics-assistant-with-llm-powered-natural-language-queries) stripped their LLM of all authorisation logic entirely. The APIs the LLM calls handle security, and if they return 403 Forbidden, the LLM simply says “I can’t access that.”
The pattern is clear: architectural constraints provide guarantees that prompt engineering cannot.
### 4. Evaluation Grows Up
The phrase “evals are the new unit tests” has become a mantra for good reason. [Ramp’s approach](https://www.zenml.io/llmops-database/building-trustworthy-llm-agents-for-automated-expense-management) is instructive: they discovered that user feedback requires careful interpretation. Finance teams might approve expenses that technically violate policy out of convenience. Treating user actions as ground truth would bias the system toward excessive leniency. Their solution: “golden datasets” independently labelled to establish correct decisions based solely on available information.
Shadow testing before live deployment has become standard for high-stakes applications. Ramp runs agents in “shadow mode” on real transactions, comparing predictions to human decisions. They only enable live actions once shadow accuracy hits specific thresholds, testing guardrails on real financial data without risking a dollar.
Circuit breakers acknowledge that perfect reliability is impossible. [Cox Automotive ](https://www.zenml.io/llmops-database/scaling-ai-agents-to-production-a-blueprint-for-autonomous-customer-service)implements hard limits on cost and conversation turns; when thresholds are exceeded, the system gracefully hands off to humans. The goal isn’t preventing all failures but ensuring failures are detected quickly and handled gracefully.
### 5. MCP Quietly Stabilises
While agent harnesses and memory systems remain in flux, the Model Context Protocol has achieved something unusual: relative stability. Enterprises are building servers, SaaS companies are exposing APIs, and a practical knowledge base is accumulating.
[Loblaws wrapped 50+ internal APIs](https://www.zenml.io/llmops-database/building-alfred-production-ready-agentic-orchestration-layer-for-e-commerce) so their orchestration agent could handle complex workflows like shopping for recipe ingredients. [Sentry’s MCP server ](https://www.zenml.io/llmops-database/scaling-an-mcp-server-for-error-monitoring-to-60-million-monthly-requests)handles 60 million requests per month with a three-person team.
But the database also documents MCP’s real challenges. [CloudQuery discovered ](https://www.zenml.io/llmops-database/building-and-operating-an-mcp-server-for-llm-powered-cloud-infrastructure-queries)that tool naming matters more than expected: a tool named `example_queries` sat unused for two weeks; renaming it to `known_good_queries` with a verbose description made it frequently used. Tools are prompts. [Digits explicitly rejected MCP](https://www.zenml.io/llmops-database/running-llm-agents-in-production-for-accounting-automation) for production: “all our data is internal and major security questions remain unresolved.”
MCP is settling into its appropriate role as infrastructure for connecting agents to existing systems, rather than a solution in itself.
### 6. Stop Waiting for the Frontier
There’s a seductive narrative that the next model release will solve your production problems. The database tells a different story.
[Cubic’s AI code review agent](https://www.zenml.io/llmops-database/reducing-false-positives-in-ai-code-review-agents-through-architecture-refinement) got _worse_ when they gave it more tools. The agent became confused, generating false positives until developers stopped trusting it. Instead of a smarter model, they needed streamlining: fewer capabilities, better results. [Alexa’s team](https://www.zenml.io/llmops-database/transforming-a-voice-assistant-from-scripted-commands-to-generative-ai-conversation-at-scale) found that adding more few-shot examples *decreased *accuracy because the model overfitted to examples instead of reasoning.
[Stripe’s compliance lead](https://www.zenml.io/llmops-database/ai-powered-compliance-investigation-agents-for-enhanced-due-diligence) describes end-to-end automation as a “fairytale.” Instead, they decomposed complex reviews into bite-sized tasks on strict “rails,” ensuring that even unexpected model behaviour is contained to a single task.
The teams shipping systems that work aren’t waiting for GPT-6. [Robinhood](https://www.zenml.io/llmops-database/fine-tuning-and-multi-stage-model-optimization-for-financial-ai-agents) reduced P90 latencies from 55 seconds to under one second through hierarchical tuning (prompt optimisation, trajectory tuning, LoRA fine-tuning) rather than waiting for faster models. [Care Access](https://www.zenml.io/llmops-database/optimizing-medical-record-processing-with-prompt-caching-at-scale) cut costs 86% by separating static medical records (cached) from dynamic questions: pure infrastructure engineering.
The allure of the frontier is that it promises to make engineering unnecessary. It doesn’t. Better models shift where challenges lie; they don’t eliminate them.
## What This Means for Practitioners
The implication is straightforward: invest in engineering.
The organisations winning with LLMs are distinguished by their infrastructure, their evaluation practices, their operational discipline, and their willingness to treat AI systems with the same rigour they’d apply to any mission-critical technology. Model access and prompt libraries matter far less than these fundamentals.
The experimentation phase has ended. The engineering phase has begun.
[[Read the full report →]](https://www.zenml.io/blog/what-1200-production-deployments-reveal-about-llmops-in-2025) for detailed case studies on each theme, including specific architectural patterns, failure modes, and implementation details from companies including Stripe, Amazon, DoorDash, Shopify, LinkedIn, Manus, Ramp, Cox Automotive, and dozens more.
*The LLMOps Database is maintained by ZenML. We catalogue production LLM deployments to help practitioners learn from what’s actually working in the field. *[Explore the database →](https://www.zenml.io/llmops-database)
Tags
[llmops-database](https://www.zenml.io/tags/llmops-database)[LLMOps](https://www.zenml.io/tags/llmops)[agents](https://www.zenml.io/tags/agents)[best-practices](https://www.zenml.io/tags/best-practices)[genai](https://www.zenml.io/tags/genai)
[ Alex Strick van Linschoten View all posts → ](https://www.zenml.io/author/alex-strick-van-linschoten)
[← PreviousThe Hidden Complexity of ML Pipeline Schedules](https://www.zenml.io/blog/the-hidden-complexity-of-ml-pipeline-schedules)[Next →What 1,200 Production Deployments Reveal About LLMOps in 2025](https://www.zenml.io/blog/what-1200-production-deployments-reveal-about-llmops-in-2025)
## Continue reading
### [What 1,200 Production Deployments Reveal About LLMOps in 2025 December 19, 2025 LLMOps ](https://www.zenml.io/blog/what-1200-production-deployments-reveal-about-llmops-in-2025)### [LLMOps in Production: Another 419 Case Studies of What Actually Works December 15, 2025 LLMOps ](https://www.zenml.io/blog/llmops-in-production-another-419-case-studies-of-what-actually-works)### [LLMOps in Production: 287 More Case Studies of What Actually Works July 17, 2025 LLMOps ](https://www.zenml.io/blog/llmops-in-production-287-more-case-studies-of-what-actually-works)
## AI orchestration,on the infra you choose
### Get new posts by email
New posts and product updates, sent when we publish. No spam.
By subscribing, you agree to our [privacy policy](https://www.zenml.io/privacy-policy).
We use cookies to improve your experience and analyze site traffic. See our [privacy policy](https://www.zenml.io/privacy-policy).
ManageReject allAccept all
