---
title: 'LinkedIn: Building and Evolving a Production GenAI Application Stack - ZenML
  LLMOps Database'
id: linkedin-building-and-evolving-a-production-genai-application-stack-zenml-llmops
tags:
- llmops-case-study
- inference-stack
- vllm
- langchain
created: '2026-10-09T22:52:57.975148Z'
updated: '2026-10-10T14:52:16.978698Z'
source: https://www.zenml.io/llmops-database/building-and-evolving-a-production-genai-application-stack
source_domain: www.zenml.io
fetched_at: '2026-10-09T22:52:57.950250Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'ZenML summary of LinkedIn''s Nov 2024 engineering blog (primary at linkedin.com/blog/engineering/generative-ai/behind-the-platform-...).
  LinkedIn ran a fragmented stack for ~2 years: Java midtier for online serving vs.
  Python for offline LLM workflows/evals, because AI engineers needed Python''s OSS
  ecosystem but LinkedIn''s RPC/storage/tracing infra was Java-only. Concluded Java-for-serving
  was unsustainable and invested in first-class Python online serving (building Python
  clients only for *future* infra like gRPC, not legacy rest.li). Adopted LangChain
  as the base framework after evaluating it on functionality/operability/community/roadmap;
  LinkedIn''s internal GenAI framework is now a mandated thin wrapper over LangChain.
  Standardized prompts on Jinja templates plus a versioned ''Prompt Source of Truth''
  service, converging on the OpenAI Chat Completions API format. Built a ''skill registry''
  (Skill Inversion: downstream services define/expose skills rather than apps wrapping
  them) exposed to LangChain''s tool API. Conversational memory reuses LinkedIn''s
  existing production messaging stack (not a new vector DB) via the LangChain Conversational
  Memory abstraction. For model inference: initial LLMs came from Azure OpenAI via
  a centralized GenAI proxy (routing, streaming, quota, Trust/Responsible-AI checks);
  increasingly, in-house fine-tuning/serving runs on an internal AI platform built
  on open-source PyTorch + DeepSpeed + vLLM, with fine-tuned Llama reported to match
  or beat commercial frontier models at lower cost/latency for LinkedIn-specific tasks.
  All models (external and internal) are exposed behind a single OpenAI Chat Completions-compatible
  API for seamless on-prem/external switching.'
utility_score: 14.0
---

LLMOps database
# Building and Evolving a Production GenAI Application Stack
LinkedIn·2023
[View source](https://www.linkedin.com/blog/engineering/generative-ai/behind-the-platform-the-journey-to-create-the-linkedin-genai-application-tech-stack)
Summary
LinkedIn's journey in developing their GenAI application tech stack, transitioning from simple prompt-based solutions to complex conversational agents. The company evolved from Java-based services to a Python-first approach using LangChain, implemented comprehensive prompt management, developed a skill-based task automation framework, and built robust conversational memory infrastructure. This transformation included migrating existing applications while maintaining production stability and enabling both commercial and fine-tuned open-source LLM deployments.
In Production
Four LLMOps case studies like this one, in your inbox every Tuesday and Thursday. Industry
Technologies · 27
    
[chatbot](https://www.zenml.io/llmops-tags/chatbot)[structured_output](https://www.zenml.io/llmops-tags/structured-output)[high_stakes_application](https://www.zenml.io/llmops-tags/high-stakes-application)[regulatory_compliance](https://www.zenml.io/llmops-tags/regulatory-compliance)[rag](https://www.zenml.io/llmops-tags/rag)[prompt_engineering](https://www.zenml.io/llmops-tags/prompt-engineering)[fine_tuning](https://www.zenml.io/llmops-tags/fine-tuning)[embeddings](https://www.zenml.io/llmops-tags/embeddings)[semantic_search](https://www.zenml.io/llmops-tags/semantic-search)+18 moreShow fewer[system_prompts](https://www.zenml.io/llmops-tags/system-prompts)[error_handling](https://www.zenml.io/llmops-tags/error-handling)[latency_optimization](https://www.zenml.io/llmops-tags/latency-optimization)[cost_optimization](https://www.zenml.io/llmops-tags/cost-optimization)[langchain](https://www.zenml.io/llmops-tags/langchain)[pytorch](https://www.zenml.io/llmops-tags/pytorch)[vllm](https://www.zenml.io/llmops-tags/vllm)[fastapi](https://www.zenml.io/llmops-tags/fastapi)[redis](https://www.zenml.io/llmops-tags/redis)[cache](https://www.zenml.io/llmops-tags/cache)[elasticsearch](https://www.zenml.io/llmops-tags/elasticsearch)[monitoring](https://www.zenml.io/llmops-tags/monitoring)[cicd](https://www.zenml.io/llmops-tags/cicd)[microservices](https://www.zenml.io/llmops-tags/microservices)[kubernetes](https://www.zenml.io/llmops-tags/kubernetes)[docker](https://www.zenml.io/llmops-tags/docker)[microsoft_azure](https://www.zenml.io/llmops-tags/microsoft-azure)[openai](https://www.zenml.io/llmops-tags/openai)
On this page
## Overview
LinkedIn’s engineering blog post from November 2024 provides a detailed look into how the company built its GenAI application technology stack to support AI-powered products at enterprise scale. This case study is particularly valuable because it documents the evolution of their approach over approximately two years, from initial experimentation in early 2023 to a mature, standardized platform. The journey encompasses critical LLMOps concerns including framework selection, language choices for production serving, prompt management, task automation through skills, memory infrastructure, model inference abstraction, and migration strategies.
The products enabled by this stack include collaborative articles, AI-assisted Recruiter, AI-powered insights, and most recently, their first AI agent called Hiring Assistant. The evolution moved from simple prompt-based solutions to sophisticated assistive agent experiences with multi-turn conversation capabilities supported by advanced contextual memory.
## Framework Evolution and Language Selection
One of the most significant decisions LinkedIn faced was the choice of programming language and framework for their GenAI applications. Their initial pragmatic approach leveraged their existing Java-based online serving infrastructure, building a shared Java midtier that encapsulated common GenAI functionality as reusable code. However, this approach quickly encountered scaling challenges as the number of use cases grew, causing the midtier to become both a development and operational bottleneck.
A key tension emerged between their online Java infrastructure and the preferences of AI engineers who worked on offline LLM workflows, prompt engineering, and evaluations. These practitioners preferred Python due to the extensive ecosystem of open-source Python solutions in the GenAI domain. Rather than being blocked by the substantial effort required to rebuild this tooling in Java or immediately adding Python support to their online serving stack, LinkedIn made a conscious decision to proceed with fragmented online (Java) and offline (Python) stacks with basic bridging tooling.
This fragmented approach maintained short-term momentum but proved unsustainable at scale. Preventing divergence across various Java midtier services and keeping logic synchronized between offline Python and online Java stacks required substantial effort. As the GenAI landscape and associated open-source libraries continued to evolve primarily in Python, LinkedIn concluded that staying on Java for serving was suboptimal for the long term.
The decision to invest in Python as a first-class language for both offline iteration and online serving represented a significant infrastructure investment. Since LinkedIn had historically used Java almost exclusively for online serving, much of their online infrastructure (RPC, storage access, request context passing, distributed tracing) only had Java client implementations. They launched an initiative to enable Python support for critical infrastructure dependencies, guided by several principles:
  * **Pragmatic prioritization** : Rather than building Python equivalents of everything available in Java, they stack-ranked requirements and found creative solutions. For example, they implemented only part of the request context specification in Python, omitting functionality like bidirectional context passing that wasn’t needed for GenAI applications. For their Espresso distributed document store, they used an existing REST proxy rather than building a native Python client.
  * **Opportunistic alignment with future technology** : Instead of building Python support for existing infrastructure, they evaluated upcoming technology transitions and built support only for future tech. Since LinkedIn was transitioning RPCs from rest.li to gRPC, they built Python support only for gRPC.
  * **First-class developer experience** : They invested in Python native builds, re-engineered solutions involving Python calling C/C++ code to be fully Python native for debugging purposes, built tooling automation for importing open-source Python libraries, and established processes to maintain reasonably current Python versions.


LinkedIn’s analysis of the LangChain open source project, including functionality, operability, community involvement, evolution track record, and future extensibility, convinced them to adopt it for online serving. Their GenAI application framework is now a thin wrapper atop LangChain, bridging it with LinkedIn infrastructure for logging, instrumentation, and storage access. This framework is vended as a versioned internal library mandated for all new GenAI applications.
## Prompt Management
LinkedIn’s approach to prompt management evolved significantly from initial manual string interpolation in code. They recognized that complex prompt engineering required more structure around modularization and versioning, leading to the creation of a Prompt Source of Truth component.
Key observations that shaped their approach included the recognition that many use cases benefited from partial or full prompts for shared functionality, particularly around Trust and Responsible AI requirements that needed to be universally injected as guardrails. Additionally, they identified the essential need to gradually ramp new prompt versions to ensure they didn’t break or worsen existing product experiences.
They standardized on the Jinja template language for authoring prompts and built a prompt resolution library (initially in Java, later rewritten in Python) to avoid common string interpolation bugs. As conversational assistants with multi-turn UIs emerged, they enhanced the component to provide more structure around human and AI roles in conversations, eventually converging on the OpenAI Chat Completions API format once it was released and widely adopted.
All prompt engineers at LinkedIn now author prompts using these guidelines and must adhere to modularization and versioning requirements for storing prompts. In exchange, they receive fluent sharing across prompts and seamless integration with the application framework.
## Task Automation via Skills
LinkedIn extended their existing skills-based approach to work into their GenAI applications, using skills as a mechanism to enable task automation. The skill abstraction enables LLMs to move beyond vanilla text generation and use function calling for Retrieval Augmented Generation (RAG) or task automation by converting natural language instructions into API calls. In their products, this manifests as skills for viewing profiles, searching for posts, querying internal analytics systems, and accessing external tools like Bing for search and news.
Initially, they built skills as custom code within each GenAI product, wrapping existing LinkedIn internal and external APIs using LLM-friendly JSON schemas compatible with the LangChain tool API. However, this approach encountered scaling bottlenecks:
  * Teams frequently re-implemented the same skills across different products
  * Skills needed updates whenever downstream services evolved
  * Application developers had to manually specify skills in prompts


To address these issues, they developed “Skill Inversion,” where instead of calling applications defining skills over implementing downstreams, the downstreams define and expose skills to calling applications. This organically eliminated duplication and evolution problems.
Their skill infrastructure now includes a centralized skill registry service for adding and retrieving definitions (via skill ID or semantic search) at runtime, build plugins that enable downstream applications to annotate endpoint implementations and automatically register them in the skill registry with proper validations, and a dynamic LangChain tool that retrieves skill definitions from the registry and invokes actual skills with supplied arguments. This approach eliminates developer-specified skills in prompts and gives significantly larger agency to LLMs.
## Conversational Memory Infrastructure
Recognizing that LLMs are stateless by default and that contextualization and personalization are essential for great GenAI product experiences, LinkedIn built Conversational Memory Infrastructure to store LLM interactions, retrieve past context, and inject it into future prompts.
Their initial solution used Couchbase or Espresso databases for storage, with application teams responsible for repetitive tasks like database setup, writing requests/responses, and reading from memory before inference. However, they soon needed more than raw conversation storage and retrieval. Since LLM context windows are limited and increasing input tokens has cost and latency implications, they needed to retrieve only relevant parts of conversations through semantic search using embeddings and summarization capabilities.
Rather than building a new system from scratch, they decided to leverage LinkedIn’s existing messaging stack for several reasons: conversations between humans and GenAI applications were similar to human-to-human conversations, the stack was proven for high availability and reliability in production, enhancements like semantic search and summarization would benefit non-GenAI use cases, and they could leverage low-latency reliable message delivery to mobile/web clients with state synchronization across devices.
The LinkedIn messaging-based Conversational Memory infrastructure is now integrated into their GenAI application framework using the LangChain Conversational Memory abstraction. They also developed “Experiential Memory” for deriving signals based on user-application interaction experiences, such as voice and tone preferences for text authoring, preferred notification channels, and UI template choices for visualizing AI-generated content.
## Model Inference and Fine-Tuning
LinkedIn’s initial GenAI applications used LLMs provided by Azure OpenAI service, with all requests routed through a centralized GenAI proxy that offered Trust and Responsible AI checks, seamless support for new models and versions, incremental response streaming to reduce user-perceived latency, and quota management for fair resource usage across products.
Their GenAI applications have increasingly started depending on their internal AI platform built atop open-source frameworks like PyTorch, DeepSpeed, and vLLM, providing robust and highly scalable fine-tuning and serving infrastructure. They found that LLMs like Llama, when fine-tuned for LinkedIn-specific tasks, often achieve comparable or better quality than state-of-the-art commercial foundational models while offering much lower costs and latencies. They also built member-facing settings to control whether data is used for training or fine-tuning models.
To provide a transparent experience across external and internal models, they invested in exposing an OpenAI Chat Completions API for all LLMs in use, allowing application developers to program to this single API regardless of underlying model. Configuration hooks in the application framework enable easy switching between on-premises and external models without routing details, facilitating experimentation with different models for debugging and production A/B tests.
## Migration Strategy
As their GenAI application stack evolved, rapid migration from legacy bespoke solutions to standardized ones was essential to minimize technical debt and increase leverage. They handled migrations using a lean team combining engineers with deep knowledge of the historical Java stack and engineers working on the new stack.
Their migration followed two key principles. First, incrementality: rather than big-bang migrations, they migrated individual components sequentially. For example, as soon as the messaging-based conversational memory infrastructure was ready, they migrated Java-based GenAI applications to it without waiting for the Python LangChain framework migration. They started with simpler, smaller apps before handling more complex ones, using a depth-first approach for prototyping and identifying gaps, followed by breadth-first migration with A/B tests for gradual ramping.
Second, upskilling talent: many senior Java-proficient engineers needed Python experience, so they paired them with earlier-career but more experienced Python developers for on-the-job learning through an “accelerated Python class.”
## Architectural Considerations and Lessons Learned
The case study emphasizes several key takeaways for organizations building GenAI applications at scale:
  * There is no one-size-fits-all formula for scalable GenAI product development given the technology’s relative recency and rapid evolution. Engineering organizations should make calculated framework investments balancing pragmatism and time-to-market with long-term leverage.
  * Prompt management, while deceptively simple initially, involves significant nuance including templating, versioning, and prompt structure management to work at scale for complex GenAI applications.
  * Significant product value is unlocked by using GenAI for task automation versus content generation alone, but intentional full-stack tooling is necessary to enable scalable task automation.
  * Memory is becoming a critical capability requiring thoughtful tech stack integration, enabling learning from activities, incorporating feedback, and building personalized experiences.
  * Strategic abstraction layers that enable the same core infrastructure across different models provide long-term benefits in efficiency and product capabilities.


The post acknowledges that building production-grade GenAI applications also depends on several other critical areas including AI platform infrastructure, in-house modeling, observability and monitoring stacks, Responsible AI/Trust services, and evaluation processes and frameworks, which they may cover in future posts.
## More like this
### [Agentic AI Copilot for Insurance Underwriting with Multi-Tool Integration Snorkel · 2025 ](https://www.zenml.io/llmops-database/agentic-ai-copilot-for-insurance-underwriting-with-multi-tool-integration)### [Reinforcement Learning for Code Generation and Agent-Based Development Tools Cursor · 2025 ](https://www.zenml.io/llmops-database/reinforcement-learning-for-code-generation-and-agent-based-development-tools)### [Building Production-Grade Generative AI Applications with Comprehensive LLMOps Block (Square) · 2023 ](https://www.zenml.io/llmops-database/building-production-grade-generative-ai-applications-with-comprehensive-llmops)
## AI orchestration,on the infra you choose
### Get In Production
Four LLMOps case studies in your inbox, every Tuesday and Thursday. No spam.
By subscribing, you agree to our [privacy policy](https://www.zenml.io/privacy-policy).
We use cookies to improve your experience and analyze site traffic. See our [privacy policy](https://www.zenml.io/privacy-policy).
ManageReject allAccept all
