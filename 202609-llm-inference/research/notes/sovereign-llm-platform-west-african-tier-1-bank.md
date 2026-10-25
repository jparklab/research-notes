---
title: Sovereign LLM Platform — West African Tier-1 Bank
id: sovereign-llm-platform-west-african-tier-1-bank
tags:
- onprem-llm-inference-stacks-859099
- hardware
- case-study
- air-gapped
created: '2026-10-23T17:23:48.949497Z'
updated: '2026-10-23T17:24:56.615218Z'
source: https://www.mindmapdigital.ai/stories/west-african-bank-sovereign-llm
source_domain: www.mindmapdigital.ai
fetched_at: '2026-10-23T17:23:48.927233Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Vendor customer story (MindMap Digital; anonymized West African Tier-1 bank,
  9 countries; vendor-authored, unverifiable). Air-gapped on-prem platform: Llama
  3.1 70B on 32 H100 in tensor-parallel groups of 4 via vLLM continuous batching;
  Llama 3.1 8B / Mistral 7B on separate L40S fleet (heterogeneous H100+L40S, routing
  by model size). LLM Gateway, guardrails, Qdrant RAG, BGE-M3. 40+ use cases, 36-week
  delivery, claimed 62% lower inference cost vs prior cloud, regulator-cited as reference.
  Note GPUSmith says ''five accelerators''; the story''s own text names H100 + L40S
  fleet.'
---

*Suggested by [[llm-inference-hardware-sizing-enterprise-gpu-guide-2026-gpusmith]] — cited as primary source for named deployment/spec*

[Home](https://www.mindmapdigital.ai/) · [Customer Stories](https://www.mindmapdigital.ai/stories) · West African Tier-1 Bank
BFSI · Africa
# Sovereign LLM Platform at a West African Tier-1 Bank — A Single On-Prem Foundation for 40+ AI Use Cases
An air-gapped LLM serving platform inside the bank's own data centre, shared across forty business-line use cases — built on Sovereign LLM Platform + LLM Gateway.
[Talk to the team that built this →](https://www.mindmapdigital.ai/contact)[All customer stories →](https://www.mindmapdigital.ai/stories)
40+
Production AI use cases on one platform
36w
Delivery duration
On-Premises
Deployment
5
Accelerators used
On-Premises
40+
Production use cases live
0
External LLM calls (air-gapped)
62%
Inference cost reduction vs cloud
180+
Business users with model access
MindMap OS · installed inside the customer's perimeter by forward deployed engineers · On-Premises · billed on the result
In this storySovereign AIBFSIOn-PremisesLLM PlatformAir-Gapped
01
The challenge
## The challenge
The bank — a Tier-1 universal bank headquartered in West Africa with operations across nine countries — had a proliferation problem. Over the previous two years, individual business lines had each commissioned their own LLM-enabled pilots: a contact-centre chatbot, a credit-memo drafting assistant for SME lending, a SWIFT MT message parser for trade finance, a marketing-content drafting tool, a customer-correspondence summariser. Each pilot had been built by a different vendor, on a different cloud, with different security postures, different data flows and different cost models. The bank's CIO calculated that at least six of these pilots were sending bank customer data to public cloud LLM endpoints in violation of the bank's data-residency policy.
The bank's regulator — the central bank in the home market — had also issued a draft directive requiring that any AI system processing customer data must do so within the country's borders, on infrastructure under the bank's direct control. The directive was scheduled to come into force within twelve months, and would force a wholesale review of every existing AI pilot.
The CIO and the Chief Data Officer concluded that the proliferation pattern was unsustainable. Each use case re-solving the same plumbing — model serving, prompt management, guardrails, audit logging, integration with the bank's SSO, cost tracking — was both expensive and a regulatory liability. The brief to MindMap was clear: build a single shared sovereign LLM platform that all current and future use cases would run on, with the existing pilots migrated onto it within nine months.
02
The approach
## The approach
We led with Sovereign LLM Platform (Sl) as the model-serving foundation, LLM Gateway (Lg) as the access layer (model routing, cost controls, rate limiting), Guardrail System (Gs) for safety, RAG Builder (Rg) for the use cases that needed retrieval, and Model Benchmarker (Mb) for ongoing model evaluation. Together these formed a platform that the bank's data-science teams, the business-line product teams and the third-party vendors building bank-specific solutions could all build against, with a uniform API and a uniform governance model.
Phase one was the platform build — twelve weeks to a production-ready cluster with three foundation models live (Llama 3.1 70B and 8B, plus Mistral 7B), the gateway and guardrails operational, SSO integration with the bank's existing identity platform, and the audit-and-cost-tracking dashboards live for the CDO's team.
Phase two — running in parallel from week eight — was the migration of the existing pilots. Each pilot's integration team was given a four-week window to refactor onto the new platform, with MindMap engineers embedded in each migration to handle the integration mechanics. By week twenty-eight, twelve of the legacy pilots had been migrated and the remaining four had been retired (in favour of a use-case rebuild on the new platform).
Phase three was the new-build acceleration. With the platform live, new use cases could be stood up in days rather than months — the plumbing was already there, the security review was a delta-only review rather than a full one, and the cost-tracking governance meant the CFO knew what each use case actually cost to run. Within the first year post-platform-go-live, twenty-eight new use cases were added beyond the migrated twelve.
Accelerators in this engagement
## The pre-built building blocks
Rather than commission a ground-up build, the engagement leaned on MindMap's pre-built accelerator library — production-tested components that compress what would otherwise be a six-to-nine-month build into weeks.
[Sl Sovereign LLM Platform → Foundation model serving — Llama 3.1, Mistral, on-prem H100 + L40S fleet](https://www.mindmapdigital.ai/library/sovereign-llm-platform)[Lg LLM Gateway → Model routing, rate limiting, per-use-case cost tracking](https://www.mindmapdigital.ai/library/llm-gateway)[Gs Guardrail System → PII, prompt-injection, jailbreak, content moderation](https://www.mindmapdigital.ai/library/guardrail-system)[Rg RAG Builder → Per-use-case retrieval infrastructure on Qdrant](https://www.mindmapdigital.ai/library/rag-builder)[Mb Model Benchmarker → Continuous model evaluation across open-source candidates](https://www.mindmapdigital.ai/library/model-benchmarker)
03
The architecture
## The architecture
The platform runs entirely inside the bank's primary data centre in the home market, on a dedicated GPU cluster. There are no outbound calls to any external LLM provider — the platform is fully air-gapped from the public internet. The bank's regulator audited the deployment and certified it as compliant with the draft data-residency directive in advance of the directive coming into force.
The GPU fleet is sized to handle peak concurrent load across all use cases. The largest models — Llama 3.1 70B for the heavier reasoning use cases (credit-memo drafting, complex case narrative generation) — run on a cluster of thirty-two H100 GPUs in tensor-parallel groups of four, served via vLLM with continuous batching. The smaller models — Llama 3.1 8B and Mistral 7B for the high-volume, lower-complexity use cases (intent classification, simple summarisation, document classification) — run on a separate fleet of L40S GPUs.
The LLM Gateway is the access layer every use case interacts with. It handles model routing (the same logical request can be routed to different physical models based on cost and capability), rate limiting per use case and per user, cost tracking with per-use-case showback to the business line, request and response logging with the bank's required retention, and the integration with the bank's SSO for user-level authentication and authorisation.
The Guardrail System sits in line on every model request, applying PII detection (with the bank's own configurable redaction rules), prompt-injection defence, jailbreak detection, and output content moderation. Different use cases can configure different guardrail profiles — the contact-centre chatbot has a stricter profile than the internal credit-memo drafting tool — but no use case can bypass the guardrails entirely.
The RAG infrastructure uses Qdrant as the vector database, with separate logical indexes per use case to enforce data-access boundaries. Embedding generation uses BGE-M3 (multilingual) and a fine-tuned domain-specific embedding model for the more technical use cases. The platform team operates the RAG infrastructure; individual use case teams own their data ingestion and index management.
The outcomes
## The numbers behind the story
40+
Production use cases live
0
External LLM calls (air-gapped)
62%
Inference cost reduction vs cloud
180+
Business users with model access
Forty-plus production AI use cases are now live on the platform, ranging from contact-centre conversation assistance to credit underwriting drafting to SWIFT message parsing to internal policy Q&A. New use cases routinely go from concept to production in 4 to 8 weeks versus the 6 to 9 months that comparable pilots had previously taken.
Inference cost per use case is approximately 62% lower than the equivalent cost on the bank's previous mix of public cloud LLM endpoints — driven by shared infrastructure, model right-sizing (the LLM Gateway routes simple requests to smaller models), and the elimination of per-token markups. The CFO has visibility into per-use-case cost for the first time.
Zero data-residency incidents. The bank's regulator's compliance audit, conducted six months after the platform went live, explicitly cited the deployment as a reference architecture and referenced it in the final published version of the data-residency directive.
An unexpected outcome: the platform has become a magnet for the bank's data-science talent. The bank's recruiter reports that the platform's existence — and the fact that the bank's data scientists are working on production AI systems rather than on slide-ware — has materially improved the bank's ability to hire senior ML engineers in a competitive market.
> “We had forty different AI initiatives running on four different clouds with five different security postures, and the regulator was about to make most of them illegal. MindMap built us a single sovereign platform in nine months. Every use case now runs on it. New use cases ship in weeks. The regulator audited it and held it up as the reference architecture for the country.”
**— Chief Information Officer** · West African Tier-1 Bank
04
Why MindMap was chosen
## Why MindMap was chosen
The bank had been advised by two consulting firms that a sovereign LLM platform of this scope would be a 24-month build. Both proposed approaches that involved heavy custom engineering on open-source primitives — a path the bank's CTO considered too risky given the team's lack of prior LLM platform experience.
MindMap's pre-built accelerator stack — Sovereign LLM Platform, LLM Gateway, Guardrail System, RAG Builder — let the bank skip the platform-engineering phase entirely. We could demonstrate the platform running in production at another African bank, with the same architectural pattern, the same model serving stack and the same governance posture. The bank's CTO described this as 'buying outcomes, not buying components'.
Our willingness to operate the platform in the first year as a managed service while the bank's internal team built operational capability was the third factor. By the end of year one, the platform had been fully transitioned to the bank's own infrastructure team, with MindMap remaining as an L3 escalation contract — exactly the curve the CTO had asked for.
Not ready for a call?
## Get the MindMap capability deck
The platform, the accelerator library and how an engagement runs, in one deck. Our team emails it within one business day.
Request the capability deck
More customer stories
## Related deployments
[67% BFSI · AfricaSovereign WhatsApp Banking ChatNext-powered WhatsApp bot deployed inside the bank's air-gapped data centre, handling balance, transfers, statements and loan applications in English and Swahili. 67% ](https://www.mindmapdigital.ai/stories/pan-african-bank-whatsapp)[99.1% BFSI · Middle EastCheque OCR at 99.1% Accuracy DocuMage replaced a legacy template OCR for cheque clearing, processing 10,000 cheques per day at 99.1% field accuracy with 94% straight-through processing. 99.1% ](https://www.mindmapdigital.ai/stories/gulf-bank-cheque-ocr)[4 hrs BFSI · United KingdomUK Challenger Bank KYC OnboardX rebuilt the KYC pipeline with liveness, sanctions and PEP enrichment in a single STP flow, collapsing onboarding from 5 days to 4 hours. 4 hrs ](https://www.mindmapdigital.ai/stories/uk-challenger-bank-kyc)
## Want an outcome like this?
Start with a 2-week AI Readiness Sprint. We deliver a prioritised use-case backlog and business case grounded in what's actually buildable with our accelerator library.
[Book a walkthrough →](https://www.mindmapdigital.ai/contact)[Explore BFSI →](https://www.mindmapdigital.ai/industries/bfsi)
**We use cookies to understand how the site is used** Anonymised analytics via Google Analytics. No advertising or third-party tracking. [See the privacy policy](https://www.mindmapdigital.ai/privacy).
RejectAccept
