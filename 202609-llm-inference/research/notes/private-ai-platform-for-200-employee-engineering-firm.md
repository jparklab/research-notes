---
title: Private AI Platform for 200-Employee Engineering Firm
id: private-ai-platform-for-200-employee-engineering-firm
tags:
- onprem-llm-inference-stacks-859099
- hardware
- case-study
- small-fleet
created: '2026-10-23T17:23:40.528959Z'
updated: '2026-10-23T17:24:56.378423Z'
source: https://particula.tech/portfolio/local-ai-platform-engineering
source_domain: particula.tech
fetched_at: '2026-10-23T17:23:40.508965Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Consultancy case study (Particula Tech; anonymized German structural-engineering
  firm, NDA; vendor-authored, unverifiable). 200 employees, single Supermicro server
  4x NVIDIA L40S 48GB (192GB), dual Xeon, 256GB RAM, 4TB NVMe, EUR 48k total; L40S
  ~EUR7.5k/card vs H100 EUR28k+. GPUs 1-2 Qwen3-32B FP16 TP=2 via vLLM; GPU3 Qwen3-8B
  + Qwen2.5-Coder-32B INT8; GPU4 Whisper/embeddings/Qwen3-4B. Stack: vLLM, Ollama,
  Open WebUI, Qdrant, Keycloak/AD, Docker Compose. 30-50 concurrent peak, ~1.4kW GPU/~2kW
  server, ~EUR200/mo power, replaced EUR14k/mo cloud spend, payback <4 months.'
---

*Suggested by [[llm-inference-hardware-sizing-enterprise-gpu-guide-2026-gpusmith]] — cited as primary source for named deployment/spec*

[Predict creative performance before you spend.→](https://particula.tech/products/noesis)[NEWNoesis predicts how your creative performs before you spend on media.See Noesis→](https://particula.tech/products/noesis)
200
Employees Onboarded
€0
Per-Query Cost
94%
Monthly Active Adoption
<4mo
Hardware Payback Period Client
    German Engineering Consultancy Delivered
    2026 Read
    9 min read Reference
    On request, under NDA
Stack
  * Qwen3-32B
  * Qwen3-8B
  * Qwen2.5-Coder-32B
  * NVIDIA L40S
  * vLLM
  * Open WebUI
  * Whisper
  * Ollama
  * Qdrant
  * FastAPI
  * Docker
  * Keycloak


## Project Overview
A German structural engineering consultancy with 200 employees across two offices came to us with a problem that had nothing to do with engineering. Their people were already using AI, just not safely. About 80 employees had personal ChatGPT accounts they were pasting client data into. The IT department had approved ChatGPT Enterprise for some teams, GitHub Copilot for the software group, and various transcription services for project managers. Monthly cloud AI spending had reached EUR 14,000 across subscriptions and API costs, with no central oversight of what data was leaving the network.
Client NDAs on most of their infrastructure projects explicitly prohibited sending project data to third-party cloud services. German data protection regulations added another layer of constraint. They needed AI capabilities across the entire company, chat, document search, code assistance, meeting transcription, but everything had to run on their own hardware with zero data leaving their network. We built a self-hosted AI platform on a single server with four NVIDIA L40S GPUs running open-source Qwen3 models, deployed Open WebUI as the company-wide interface, and connected it to their engineering knowledge base through local RAG. Total hardware cost was EUR 48K. It paid for itself in under four months.
## Project Scope
The platform was delivered in four components over three months, starting with hardware and model infrastructure, then rolling out capabilities progressively to departments.  
| Component  | Focus  | Status  | Key Deliverables  |  
| --- | --- | --- | --- |  
| 1  | GPU Infrastructure & Model Serving  | Completed  | Supermicro 4x L40S server, vLLM inference engine, Ollama model management, Docker orchestration, Qwen3-32B and Qwen3-8B deployment  |  
| 2  | Company-Wide Chat Platform  | Completed  | Open WebUI deployment, Keycloak SSO with Active Directory, per-department prompt templates, model selection interface, conversation history  |  
| 3  | RAG Knowledge Base  | Completed  | Qdrant vector database, 23 years of project archives indexed, DIN/Eurocode standards, proposal templates, source-cited search interface  |  
| 4  | Productivity Tools  | Completed  | Local Whisper transcription (German + English), meeting summary generation, Qwen2.5-Coder-32B for Python/MATLAB code assistance, report draft automation  |  
## Infrastructure: 4x L40S Server and Model Serving
The entire platform runs on a single Supermicro server with four NVIDIA L40S GPUs, each with 48GB of GDDR6 memory, 192GB total GPU memory. We chose the L40S over the H100 for a specific reason: at roughly EUR 7,500 per card versus EUR 28,000+ for an H100, the L40S delivers the best cost-per-token for inference workloads. Since we're serving models, not training them, the L40S's inference-optimized Ada Lovelace architecture is the right tool. The PCIe form factor fits a standard rack server with no special NVLink interconnect required. Total hardware cost including the server chassis, dual Xeon CPUs, 256GB RAM, and 4TB NVMe storage came to EUR 48,000.
We allocated the four GPUs across workloads based on usage patterns. GPUs 1 and 2 run Qwen3-32B in FP16 precision with tensor parallelism via vLLM, splitting the model's 64GB memory footprint across two cards with room left for KV cache to handle concurrent requests. This is the primary model: it handles complex reasoning, document analysis, long-form report generation, and anything that needs GPT-4-class capability. GPU 3 serves Qwen3-8B for fast queries that don't need the full 32B model, plus Qwen2.5-Coder-32B in INT8 quantization for code assistance, swapped on demand. GPU 4 runs Whisper for speech-to-text, the embedding model for RAG indexing and retrieval, and Qwen3-4B for lightweight tasks like formatting and translation.
vLLM handles inference serving with continuous batching, which means multiple employees can query the same model simultaneously without waiting in a serial queue. At peak usage, around 30 to 50 concurrent requests from 200 employees, the system responds in under 2 seconds for Qwen3-8B queries and 5 to 15 seconds for complex Qwen3-32B tasks. Ollama manages model lifecycle: loading, unloading, and swapping models across GPUs based on demand patterns. The entire stack runs in Docker containers orchestrated with Docker Compose, making updates and rollbacks straightforward.
Power consumption sits at 1.4kW continuous under load from the GPUs alone, roughly 2kW for the full server. The firm's existing server room handled this without modifications, it's less than a standard office HVAC unit. Monthly electricity cost is approximately EUR 200, a rounding error compared to the EUR 14,000 in cloud costs it replaced.
## Company-Wide Chat Interface
We deployed Open WebUI as the frontend for every employee. It provides a ChatGPT-like experience, conversation threads, markdown rendering, file uploads, and the ability to switch between models, running entirely on internal infrastructure. Authentication goes through Keycloak, which federates with the firm's existing Active Directory. Employees log in with their regular company credentials, no separate accounts required.
The interface lets users select which model to use. Most employees default to Qwen3-8B for everyday questions: drafting emails, summarizing documents, explaining technical concepts. When they need deeper analysis, reviewing a structural calculation, generating a detailed project proposal, or analyzing a complex specification, they switch to Qwen3-32B. The experience mirrors how ChatGPT users choose between GPT-4o-mini for quick tasks and GPT-4o for heavy lifting, except nothing leaves the building.
We built per-department prompt templates that pre-load relevant context. The structural engineering team's default template includes German building code references and the firm's standard notation conventions. The project management team's template is tuned for timeline estimation and resource planning. The business development team's template includes proposal formatting standards and client communication guidelines. Employees can override these or create their own templates.
Conversation history persists in a local PostgreSQL database. Employees can search their past conversations, pick up where they left off, and share specific threads with colleagues, functionality that was fragmented across personal ChatGPT accounts before. IT administrators have visibility into usage patterns (not conversation contents) through a monitoring dashboard, letting them track adoption and identify departments that need additional training.
## RAG Over Engineering Knowledge Base
The firm had 23 years of project archives: structural calculations, soil reports, inspection records, project proposals, client correspondence, and internal technical memos. This institutional knowledge lived in a shared drive with no search capability beyond file names. Senior engineers carried critical project knowledge in their heads. When someone left, that knowledge went with them.
We deployed Qdrant as a local vector database and indexed the entire document archive, roughly 340,000 documents totaling 2.8 million pages. The embedding model runs on GPU 4, generating vector representations of document chunks that capture semantic meaning rather than just keywords. When an engineer asks a question, the system retrieves the most relevant document sections and passes them to the LLM along with the question.
The RAG system handles queries like 'What load-bearing assumptions did we use for the Mannheim bridge overpass in 2019?' by finding the specific calculation documents, extracting the relevant sections, and synthesizing an answer with citations pointing back to source files. Engineers can click through to the original documents to verify. We also indexed the full DIN and Eurocode standards libraries so engineers can query regulatory requirements in natural language instead of manually searching through thousands of pages of technical specifications.
Document indexing runs as a nightly batch job that picks up new and modified files from the shared drive. The system also watches specific directories for real-time indexing of high-priority documents. We implemented access controls that mirror the firm's existing folder permissions, an engineer without access to a confidential client folder can't retrieve documents from it through RAG, even indirectly through the LLM's response.
## Productivity Tools: Transcription, Documents, Code
Meeting transcription was one of the most requested features. Project managers were spending EUR 600/month on cloud transcription services and were uncomfortable sending client meeting recordings to external APIs. We deployed Whisper locally on GPU 4, supporting both German and English transcription. Project managers upload meeting recordings through the web interface and receive timestamped transcripts within minutes. A post-processing step using Qwen3-8B generates structured meeting summaries with action items, decisions made, and open questions, formatted in the firm's standard meeting notes template.
Document generation integrates directly with the chat interface. Engineers describe what they need, 'Draft a preliminary assessment report for the Düsseldorf parking structure project based on the site survey from last week', and the system pulls relevant data from the RAG knowledge base, applies the firm's report template, and generates a first draft. The output isn't final, engineers review and edit, but it eliminates the blank page problem and typically saves 2-3 hours per report.
The software engineering team (about 30 people) writes Python scripts for structural analysis, MATLAB code for simulations, and internal tools for project management. Qwen2.5-Coder-32B runs in INT8 quantization on GPU 3, providing code completion, debugging assistance, and documentation generation. It understands the firm's internal libraries and coding conventions after we included their code repository in the RAG index. Engineers interact with it through the same Open WebUI interface, selecting the Coder model when they need programming help.
## Adoption and Change Management
Technology deployments fail when people don't use them. We designed the rollout as a three-phase process over the first two months, with adoption tracking built in from day one.
Phase one targeted 30 'champions', two to three enthusiastic early adopters from each department. We ran hands-on workshops showing them specific use cases relevant to their daily work, not generic AI demos. A structural engineer learned to query past calculations. A project manager learned to generate meeting summaries. A business developer learned to draft proposals. These champions became the internal support network, answering colleagues' questions and sharing useful prompts.
Phase two opened access to all 200 employees with a self-service onboarding flow. Employees logged in with their existing credentials, completed a 10-minute interactive tutorial, and started using the platform immediately. We published a shared prompt library with department-specific examples that employees could copy and adapt. The champions in each department handled first-line support.
Phase three focused on integration into daily workflows. We added the platform to the firm's standard project kickoff checklist, embedded links in their project management tool, and created keyboard shortcuts for common actions. Usage data showed adoption climbing from 40% of employees in month one to 78% in month two to 94% by month three. The remaining 6% were primarily field engineers who rarely work at a desk.
The top use cases by query volume settled into a clear pattern: general Q&A and drafting (38%), engineering knowledge base queries (27%), document and report generation (18%), code assistance (11%), and meeting transcription (6%). The engineering knowledge base queries were the highest-value use case, they surfaced institutional knowledge that would otherwise require interrupting a senior engineer or digging through archives for hours.
## Results
The platform eliminated EUR 14,000 per month in cloud AI subscriptions and API costs. With EUR 48,000 in hardware and approximately EUR 400/month in electricity and maintenance, the investment paid for itself in 3.5 months. First-year net savings after hardware cost: EUR 115,000. Unlike cloud subscriptions that scale per-seat, the local platform serves all 200 employees at zero marginal cost per query, adding 50 more employees next year costs nothing additional.
The system processes over 12,000 queries per week across 200 active users. Peak concurrent usage reaches 40-50 simultaneous requests during morning hours without noticeable latency degradation. Qwen3-32B handles the complex analytical work at GPT-4-comparable quality, while Qwen3-8B serves the majority of everyday queries with sub-2-second response times.
Zero data exposure incidents since deployment. Every query, every document, every meeting recording stays on the firm's own hardware. The IT team confirmed compliance with all client NDAs and German data protection requirements during their quarterly audit. Several clients specifically noted the local AI deployment as a positive factor in contract renewals, they preferred working with a firm that takes data handling seriously enough to run their own infrastructure.
The engineering knowledge base became the most transformative feature. Senior engineers reported fewer interruptions from junior colleagues asking about past projects, the answers are now in the system. New hires reach productivity faster because they can query 23 years of institutional knowledge from day one. The firm estimates the knowledge base saves approximately 15 hours per engineer per month in research and information retrieval time, though this is self-reported and likely optimistic. Even at half that figure, the productivity gain across 120 engineers dwarfs the hardware cost.
Monthly operating cost for the entire platform, electricity, system administration time, model updates, runs approximately EUR 400. The system administrator spends about 4 hours per month on maintenance: updating models when new Qwen versions release, monitoring GPU utilization, and managing the document index. This is a fraction of the time previously spent managing 200 individual cloud AI subscriptions across multiple vendors.
Topics
  * Local AI
  * On-Premises
  * Qwen3
  * Open WebUI
  * NVIDIA L40S
  * vLLM
  * RAG
  * Enterprise AI
  * Data Privacy
  * GDPR
  * Whisper
  * Code Assistance


More work
### [Liquid-bulk forwarder, Latvia Two-week diagnostic across freight operations and back office, with an honest build-versus-skip verdict on each opportunity 7 opportunities ranked ](https://particula.tech/portfolio/latflex-ai-audit)### [Wholesale distribution group A Neo4j knowledge graph over the supply chain, answering multi-hop questions plain RAG cannot reach 12M-node graph ](https://particula.tech/portfolio/graphrag-knowledge-graph-platform)
Your environment
If your constraints look like these, the next step is a conversation about yours, not a walkthrough of ours.
Sebastian Mondragon, Founder of Particula Tech
[Discuss a similar project](https://particula.tech/book)
