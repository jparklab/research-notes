---
title: The LLM Is Extraordinary. The Bank Can't Use It.
id: the-llm-is-extraordinary-the-bank-cant-use-it
tags:
- llm-inference
- model-layer
- internal-llm-inference-stacks-7f0c90
- trust-boundary
- model-risk-management
- data-residency
- practitioner-essay
created: '2026-10-09T22:56:57.473938Z'
updated: '2026-10-10T14:52:17.127496Z'
source: https://mycroftproject.substack.com/p/llm-deployment-financial-institutions-trust-boundary
source_domain: mycroftproject.substack.com
fetched_at: '2026-10-09T22:56:57.447695Z'
fetch_provider: crawl4ai
status: review
type: note
tier: commentary
content_type: blog
deprecated: false
summary: 'Mycroft Project (Substack, May 5 2026), companion piece to the infrastructure-decisions
  post, again built on an EXPLICITLY FICTIONALIZED/COMPOSITE scenario (a tier-one
  bank tech executive''s vendor demo), not a verified named case. Defines the ''trust
  boundary'' as the compounding set of legal/regulatory/contractual obligations --
  MNPI information-barrier rules, GDPR/CCPA (legal basis + DPA + cross-border transfer
  mechanism), bank secrecy/confidentiality obligations, and SR 11-7 model risk management
  (argues SR 11-7 makes it ''practically impossible to validate and monitor a black-box
  external LLM used for credit decisioning support'') -- that makes routing sensitive
  data to an external LLM API a compliance event regardless of the vendor''s SOC 2/DPA
  paperwork. Names three architectural patterns financial institutions actually use,
  in order of operational complexity: (1) on-premise open-weight deployment (names
  Llama and Mistral explicitly as the open-weight families enabling this) -- no data
  leaves the building, full compliance control, but the institution owns all serving/versioning/monitoring;
  viable mainly for large institutions with existing GPU infra and MLOps maturity;
  (2) private-cloud dedicated tenancy -- physically/logically isolated hardware with
  contractually committed data residency, cloud-provider-operated but institution-governed,
  as a middle path; (3) federated inference with RAG -- a smaller on-prem model (e.g.,
  7B class, explicitly stated as not matching GPT-4 on complex reasoning) retrieves
  from internal documents/databases so sensitive data never leaves the boundary, trading
  capability for compliance posture. Argues the two axes vendors conflate are ''security''
  (SOC 2, DPA) versus ''sovereignty'' (legally defensible proof that specific data
  was processed under specific controls within the institution''s compliance perimeter),
  and that most vendor ''private deployment'' offers are a dedicated namespace in
  multi-tenant cloud rather than genuine isolation. Recommends a portfolio approach:
  public cloud LLMs for low-sensitivity workloads (internal knowledge management,
  code assistance) vs on-prem/private-cloud for MNPI/customer-data/regulated decisioning.'
utility_score: 16.0
---

# [The Mycroft Project](https://mycroftproject.substack.com/)
SubscribeSign in
Discover more from The Mycroft Project
The Mycroft Project
Already have an account? Sign in
# The LLM Is Extraordinary. The Bank Can't Use It.
### The trust boundary between what banks can do with AI and what they're allowed to do is not a temporary obstacle. It's the design constraint — and most vendors still don't understand it.
May 05, 2026
Share
A technology executive at a tier-one bank watches a demo of a large language model summarizing credit memos, extracting covenant terms from loan agreements, and drafting preliminary credit assessments from raw financials. The model is genuinely impressive. The outputs are accurate, structured, and would save her team hours per deal.
Thanks for reading The Mycroft Project! Subscribe for free to receive new posts and support my work.
After the demo, she asks the vendor one question: where does the data go when it’s processed?
The answer — to the vendor’s inference infrastructure, hosted on public cloud, with standard enterprise data processing agreements — ends the conversation. The loan documents contain material non-public information. The credit memos reference transactions under active regulatory scrutiny. The financials belong to borrowers who have not consented to their data being processed by a third-party AI system. Every one of those data categories is subject to obligations that make sending them outside the bank’s controlled infrastructure a compliance event, not a procurement decision.
The model stays outside. The problem stays inside. The team goes back to reading the documents manually.
This scene plays out dozens of times a week across financial institutions of every size. The capability exists. The trust boundary prevents its use. The institutions building inside that constraint are not waiting for it to change. The ones still waiting are still reading the documents manually.
## What the Trust Boundary Actually Is
The trust boundary is permanent. It was not written with LLMs in mind, but it applies to them — and it is not moving.
It is not a firewall rule. It is the legal, regulatory, and contractual perimeter within which a financial institution can process sensitive data — and beyond which it cannot, regardless of what the vendor’s data handling agreement says.
For most financial institutions, the boundary is defined by overlapping obligations that compound rather than substitute for each other. MNPI controls are the most immediately recognizable to the investment banking audience: institutions with access to material non-public information operate under information barrier requirements that restrict how MNPI is handled and who it can be shared with. Sending MNPI to an external LLM — even one with strong data handling commitments — is a potential information barrier violation that compliance teams at most institutions will not approve.
The same logic applies across data protection law — GDPR, CCPA, and their equivalents require a legal basis, a data processing agreement, and in cross-border scenarios a transfer mechanism before customer data can be processed by a third party. Bank secrecy and confidentiality obligations exist independently of data protection law and are not satisfied by a DPA. And SR 11-7’s model risk management requirements make it practically impossible to validate and monitor a black-box external LLM used for credit decisioning support.
The trust boundary is not one rule. It is what you get when every one of those obligations applies at once — and none of them yields to the others.
## What “Inside the Boundary” Actually Requires
Running a large language model inside a financial institution’s trust boundary means running it on infrastructure the institution controls — on-premise data centers, private cloud environments, or dedicated cloud tenancies that are architecturally isolated from public cloud infrastructure and governed by the institution’s own security and compliance controls.
The LLMs that produced the outputs that impressed the technology executive in the opening scenario are large in a literal sense: a model with 70 billion parameters requires multiple high-memory GPUs to serve at inference latency useful for production workflows. The hardware cost is significant. The operational complexity is substantial. The expertise required to deploy, maintain, and optimize these models in a production environment is scarce and expensive.
Most financial institutions are not GPU infrastructure operators. Their data centers were built for transactional workloads. Running a production LLM inside the trust boundary requires either significant hardware investment, significant re-architecture of existing infrastructure, or a new category of deployment model — private cloud dedicated tenancies provisioned specifically for the institution, physically and logically isolated from multi-tenant public cloud, operated by the cloud provider but governed by the institution’s security controls. That category is now available. The trust boundary analysis for it is more complex than for on-premise deployment, but it is tractable for institutions with mature cloud governance programs.
## The Three Architectural Patterns Financial Institutions Are Actually Using
**On-premise open-weight model deployment.** Open-weight models — Llama, Mistral, and their derivatives — have made it possible for financial institutions to deploy capable LLMs on infrastructure they own and operate, without a dependency on any external vendor for the model itself. The institution downloads the model weights, deploys them on its own GPU infrastructure, and serves inference requests entirely within its own environment. No data leaves the building. The compliance analysis is straightforward. The operational complexity is significant — the institution is now responsible for model serving infrastructure, version management, and the full monitoring and governance requirements that apply to any production AI system. This pattern is most viable for large institutions with existing GPU infrastructure and strong MLOps capabilities.
**Private cloud dedicated tenancy.** The institution provisions a dedicated cloud environment — physically isolated hardware, logically isolated network, contractually committed data residency — and deploys an LLM within that environment. The cloud provider operates the infrastructure. The institution owns the deployment. Data processed by the LLM stays within the dedicated tenancy and is subject to the institution’s data governance controls. This pattern offers a middle path between the full operational complexity of on-premise deployment and the compliance exposure of standard cloud API usage.
**Federated inference with retrieval-augmented generation.** Rather than running a large model on sensitive data directly, the institution runs a smaller model inside the trust boundary and uses retrieval-augmented generation to provide it with relevant context from internal documents and databases. The sensitive data never leaves the boundary. This pattern sacrifices raw capability — a 7B parameter model on-premise will not match GPT-4 on complex reasoning tasks. What it preserves is compliance posture. For most financial institutions, that is not a tradeoff. It is the only viable path to deploying sensitive-data inference at all.
## What Most Vendors Are Still Getting Wrong
Enterprise cloud customers care about data security. Financial institutions care about data sovereignty — the ability to make a legally defensible claim that specific data was processed under specific controls by infrastructure within their compliance perimeter.
Security and sovereignty are related. They are not the same thing. The gap between them is the trust boundary problem.
This is not bad faith. It is a mismatch between what vendors have built for enterprise cloud customers and what financial institutions actually need. The conversation follows a predictable pattern: the vendor demonstrates a capable model, the compliance team asks where the data goes, the vendor explains their SOC 2 certification and data processing agreements, the compliance team explains that SOC 2 and a DPA are not sufficient to address the specific obligations the institution is subject to. The vendor offers a private deployment option. The institution asks what that looks like. The vendor describes a dedicated namespace in their multi-tenant cloud infrastructure rather than a genuinely isolated private deployment.
The vendors winning in financial services are the ones that understand the security/sovereignty distinction and have built deployment architectures that address sovereignty specifically — not just security. The ones that are losing are the ones still explaining to compliance teams why their enterprise data handling commitments should be sufficient.
## The Honest Assessment
Here is what I keep seeing in conversations with financial institutions trying to solve this problem: the trust boundary is not going away, the frontier models are not coming inside without significant investment, and the practical path forward for most institutions is not one architecture but a portfolio matched to data sensitivity.
Public cloud LLMs for low-sensitivity workloads — internal knowledge management, non-customer-facing document summarization, code assistance for engineering teams — where the data does not trigger the compliance obligations that define the trust boundary. Private cloud or on-premise deployment for workloads involving customer data, MNPI, or regulated decision support. RAG-based architectures that let the institution use smaller on-premise models for sensitive inference while drawing on the retrieval layer to compensate for capability gaps.
No single architecture solves everything. The institutions making progress are the ones that have mapped their workloads against their data sensitivity classifications, identified which use cases fall inside the trust boundary and which fall outside, and built the infrastructure for each category rather than searching for a single deployment model that satisfies all of them simultaneously.
The LLM is extraordinary. The bank can use it.
> **The trust boundary is not an obstacle to financial AI. It is the constraint within which financial AI must be built. The institutions that treat it as an obstacle are the ones still waiting for permission. The ones that treat it as a design requirement are the ones building.**
_The next piece covers what LLM evaluation frameworks for regulated environments actually look like — not the benchmarks that come with the model card, but the ones that hold up under model risk management scrutiny. Subscribe and reply with which of the three architectures above your institution is actually running — or trying to run. I read every response._
Thanks for reading The Mycroft Project! Subscribe for free to receive new posts and support my work.
Share
#### Discussion about this post
CommentsRestacks
TopLatestDiscussions
[The Invisible Infrastructure: What 435,000 Stars Tell You About the AI Hierarchy](https://mycroftproject.substack.com/p/the-invisible-infrastructure-what)
[Five companies. One question. No server required.](https://mycroftproject.substack.com/p/the-invisible-infrastructure-what)
Feb 17 •
[Can the audit layer audit itself?](https://mycroftproject.substack.com/p/the-audit-trail-cannot-prove-the)
[It can enforce the record. It cannot verify the truth of it.](https://mycroftproject.substack.com/p/the-audit-trail-cannot-prove-the)
May 26 •
[Eleven Decisions. $38,400 Gone. Here’s the Agent That Would Have Stopped It. ](https://mycroftproject.substack.com/p/eleven-decisions-38400-gone-heres)
[PM Summary. The Mycroft Project · Sprint Report](https://mycroftproject.substack.com/p/eleven-decisions-38400-gone-heres)
Mar 11 •
See all
### Ready for more?
© 2026 LiamBearBrown[Privacy](https://substack.com/privacy)[Terms](https://substack.com/tos)[Collection notice](https://substack.com/ccpa#personal-data-collected)
[Substack](https://substack.com) is the home for great culture
