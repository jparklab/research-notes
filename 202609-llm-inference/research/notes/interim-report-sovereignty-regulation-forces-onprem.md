---
title: Interim report — sovereignty-regulation-forces-onprem
id: interim-report-sovereignty-regulation-forces-onprem
tags:
- onprem-llm-inference-stacks-859099
- locus-sovereignty-regulation-forces-onprem
created: '2026-10-23T18:00:24.600938Z'
status: draft
type: interim
deprecated: false
summary: No regulator primary mandates on-prem; classification enclaves and bank-controlled-infrastructure
  directives are the residue; CamoGPT partly breaks classified-absence
---

# Interim report: sovereignty-regulation-forces-onprem

**Locus question:** Does regulation or data sovereignty actually mandate on-premise inference, or is it satisfiable by in-jurisdiction cloud plus contractual terms, and where is the genuine residue?
**Flavor:** dialectical

## What the corpus already said
HHS calls the Security Rule "flexible, scalable, and technology neutral" and says it "does not dictate the specific security measures that a regulated entity must use" [[summary-of-the-hipaa-security-rule-hhsgov]]. A vendor blog (Stribog, a self-hosting seller) concedes OpenAI and Azure OpenAI will sign BAAs and then argues the real drivers are BAA-chain length, CLOUD Act reach and OCR audit evidence [[hipaa-compliant-llm-on-prem-phi-inference-without-a-baa]]. Ask This Guy: "sovereignty does not require on-premise", but CLOUD Act reaches US-parent providers anywhere; on-prem stays justified for "air-gapped systems (defence, industrial secrets), a customer's contractual constraints, or a policy that forbids any subcontracting", and for round-the-clock batch volume [[on-premise-llm-and-rag-for-the-enterprise-what-60k-really-buys-you-ask-this-guy]]. DefenseScoop/DISA: GenAI.mil on commercial Gemini [[the-era-of-genaimil-is-here-users-have-mixed-reactions-and-many-questions-defens]]; INDOPACOM SIPRNet chatbot on a commercial-cloud JOE node [[disa-launching-experimental-cloud-based-chatbot-for-indo-pacific-command-defense]]. CNAS index [[cnas-insights-sovereign-ais-second-wave-is-coming-into-view-cnas]]; ACM buy-vs-build [[buy-versus-build-an-llm-a-decision-framework-for-governments]].

## What the new sources say
- DoD Cloud Computing SRG v1r2 (18 Mar 2016; old version, current SRG not reached) [[unclassified]]. §5.2.2.4: "Impact Level 6 information up to the SECRET level must be stored and processed in a dedicated cloud infrastructure located in facilities approved for the processing of classified information"; the IL6 offering "will be a closed self-contained environment ... only connected to SIPRNet"; "Physical separation from non-DoD/non-Federal Government tenants ... is required"; and it may be "on-premises or off-premises". So classification compels a dedicated cleared enclave, not DoD-owned GPUs. (The fetcher also reports IL5 permits a government-community cloud on- or off-premises; I verified IL6 text myself, IL5 from the fetcher's report.)
- Army AL&T, Aug 2025 [[army-intelligence-article-the-united-states-army]]: CamoGPT, ~75,000 users, first deployed spring 2024, "can process classified data on SIPRNet"; developer: "Our team does not train LLMs; we simply host published, open-source models that have been trained by companies like Meta, Google and Mistral." This partially breaks audited absence (b): self-hosted open weights on classified networks exist, model families named; engine, GPUs, scale still absent.
- TechTarget Oct 2024 [[government-use-of-ai-by-key-us-military-branches-techtarget]]: CENTCOM got an LLM onto SIPRNet in Jan-Feb 2024; CamoGPT ~10,000 users then. No hardware or engine named.
- EBA Outsourcing Guidelines EBA/GL/2019/02 [[final-report-on]] (per fetcher): para 30 allows critical outsourcing to third-country providers "subject to additional safeguards"; no localisation or on-prem rule. EU banking regulation is thus anti-mandate. (DORA text itself not read.)
- RBI payment-data directive, 2018 [[data-localisation-requirement-in-reserve-bank-of-india-rbi-storage-of-payment-sy]] (thin secondary summary): payment data "only in India". This is in-jurisdiction, not on-prem; India-hosted cloud complies.
- Univ. Hospital Halle, GMDS 2025 abstract [[implementing-local-open-source-llms-for-chatbot-based-document-retrieval-in-a-un]]: "a local Linux server equipped with an Nvidia H100 GPU and utilizes the Falcon-7B-Instruct model"; 287 queries in a one-month test; 75% of responses under 5 s. A named hospital with disclosed GPU count, but a one-GPU prototype. It breaks absence (f) only at toy scale; the stated motive was that "powerful proprietary models such as ChatGPT are not suitable" given strict data protection, which is a preference, not a cited mandate.

## Evidence synthesis
### Position A: regulation compels on-premise (strongest case)
The West African bank story [[sovereign-llm-platform-west-african-tier-1-bank]]: a draft central-bank directive required customer-data AI processing "within the country's borders, on infrastructure under the bank's direct control", leading to an air-gapped 32x H100 Llama 3.1 70B vLLM build. "Under the bank's direct control" is the on-prem-forcing phrase. Plus CLOUD Act: US-parent clouds fail strict sovereignty tests wherever hardware sits. Plus IL6/SIPRNet: classified processing needs closed enclaves; Army CamoGPT and CENTCOM do run self-hosted open weights there. ibl.ai claims agencies "should not buy a commercial AI subscription" [[ai-for-federal-agencies-fedramp-ato-and-the-sovereign]] but is a vendor selling the alternative. The bank story is a vendor customer story, unverifiable, and the directive was a draft.

### Position B: regulation compels jurisdiction and controls, not hardware (stronger evidence)
Every regulator primary read says so: HHS technology-neutral; EBA permits third-country outsourcing with safeguards; RBI says "in India"; SRG IL5/IL6 allow off-premises dedicated clouds. The vendors who want on-prem sales concede BAAs are available. DoD's visible direction is purchase: GenAI.mil (9 Dec 2025, Gemini for Government), where an official called it "a collection of stovepiped commercial products that you lo[ck yourself into]"; DISA said of SIPRNet chatbot "We're not trying to deploy this on our own. We're not trying to make it a production system", hosted on commercial JOE cloud. Ask This Guy's own table gives a European non-US host the same legal protection. CNAS: 184 projects/67 countries, 41 new in H1 2026, infrastructure 80% of H1-2026 projects; most model projects fine-tune foreign open-weight bases (Llama most used, Mistral and Gemma tied second, Qwen in five countries). Sovereign projects are national compute/model programmes, not enterprise on-prem inference mandates. SEA-LION v4 shrank from 70B to 27B because of inference cost.

### Evidence-base bias (quote for the report)
ACM paper: "examples of governments purchasing LLM services tend to be less frequently disclosed... Details of such contracts are often undisclosed, given confidentiality provisions... It is therefore reasonable to assume that more procurement arrangements exist than are publicly documented." Also: "high volume government use can make a subscription more expensive than the maintenance of an internal system." Public evidence therefore skews toward "build"; this report's own evidence base shares that bias, and the paper's cost remark is the economic, not legal, route to on-prem.

### Audited absences
(b) Partly broken: CamoGPT discloses open-weight hosting (Meta, Google, Mistral) on SIPRNet. Still absent: engine, GPUs, scale, specific models, any ATO/RMF artifact for a self-hosted LLM. Searched: NIPRGPT/GenAI.mil/SIPRNet/CamoGPT/Ask Sage IL5/IL6 queries, DefenseScoop, army.mil, TechTarget, SRG. Not reached: Ask Sage press, govconic awards, MARADMIN (login wall, escalated), Coalfire IL6 (bot-blocked), ACM TechBrief. (f) Largely holds: no finance, pharma or health organisation discloses an architecture at scale; Halle is one H100 prototype; the bank is an anonymous vendor story; the Magdeburg JMIR AI and ZenML health-insurance leads are unread. The Mayo and TrueFoundry claims stay falsified; not used.

### Burn-After-Use and radiology
BAU [[burn-after-use-for-preventing-data-leakage-through-a]]: per-department isolated instances, 92% of 55 infrastructure attacks defended, residual risk in credential misconfiguration and shared logging pipelines. On-prem does not remove the compliance burden; it relocates it to the operator's logging and credentials. Radiology GPU-barrier detail is from [[the-role-of-high-performance-gpurole-of-high-performance-gpu-resources-in-large]] (not re-read).

## Committed position
No regulation I could read mandates on-premise inference as such. Regulators mandate jurisdiction (RBI "in India"), controls and evidence (HIPAA, EBA safeguards), or an accredited enclave by classification (IL6/SIPRNet: dedicated, cleared, physically separate from public tenants, but explicitly allowed off-premises). On-premise is the cheapest way to satisfy the strictest of these, so it is chosen where three things coincide: (1) a classification or control-of-infrastructure requirement ("under the bank's direct control") or air-gap; (2) a contractual or policy ban on subprocessors; (3) round-the-clock batch volume. The genuine residue is classified networks and the control-of-infrastructure directive, and even there the buyer often is not self-building (INDOPACOM's JOE cloud; GenAI.mil). CamoGPT shows the residue does get self-hosted open weights. The sovereignty narrative is an industry framing; CLOUD Act exposure is a real legal argument but a risk-appetite choice, not a rule. Load-bearing reason: every regulator primary text read (HHS, EBA, RBI, DoD SRG) permits a non-self-owned compliant environment.

- **Position:** Regulation compels in-jurisdiction processing and controlled enclaves; only classification-level enclaves, "bank-controlled infrastructure" directives and contractual/air-gap constraints genuinely force self-operated hardware.
- **Confidence:** medium-high (about 75%) for the anti-mandate claim; low-medium (about 40%) on the size of the residue, because disclosure bias hides it.
- **Boundary conditions:** US federal, EU banking, HIPAA, India payments; a regime such as France SecNumCloud or Russian/Chinese localisation was not verified, and SecNumCloud v3.2's extraterritoriality angle (search snippets only) could push toward non-US hosts rather than own hardware. SRG read is the 2016 version.
- **What would change this position:** a primary instrument (a final central-bank rule, a current DoD SRG/Component directive, a national classification policy) that requires institution-owned infrastructure for LLM processing; or a named bank/pharma/health system with disclosed GPU counts citing a regulator as the reason.
- **Evidence weight:** 4 regulator primaries (HHS, EBA, DoD SRG, RBI via secondary) support anti-mandate; 1 unverifiable vendor customer story supports mandate; 3 trade/official press items (CamoGPT, CENTCOM, GenAI.mil/DISA) show both self-hosting on classified and buy-first elsewhere; vendor sources lean pro-on-prem and are used only as concessions.

## Open questions
- Current SRG (v1r3+) IL5 text; DORA Arts 28-30 primary; SecNumCloud v3.2 PDF.
- Engine/GPU/scale of CamoGPT (Army AI2C talks, GitHub).
- Ask Sage IL5/IL6 press, govconic awards, MARADMIN IL5/IL6 statement, ACM TechBrief.
- Magdeburg JMIR AI, ZenML health-insurance appeals entry.

## Sources
1. [[summary-of-the-hipaa-security-rule-hhsgov]]
2. [[hipaa-compliant-llm-on-prem-phi-inference-without-a-baa]]
3. [[on-premise-llm-and-rag-for-the-enterprise-what-60k-really-buys-you-ask-this-guy]]
4. [[the-era-of-genaimil-is-here-users-have-mixed-reactions-and-many-questions-defens]]
5. [[disa-launching-experimental-cloud-based-chatbot-for-indo-pacific-command-defense]]
6. [[cnas-insights-sovereign-ais-second-wave-is-coming-into-view-cnas]]
7. [[buy-versus-build-an-llm-a-decision-framework-for-governments]]
8. [[ai-for-federal-agencies-fedramp-ato-and-the-sovereign]]
9. [[sovereign-llm-platform-west-african-tier-1-bank]]
10. [[audited-absence-classified-network-llm-serving-stack-disclosures]]
11. [[burn-after-use-for-preventing-data-leakage-through-a]]
12. [[unclassified]] (DoD SRG v1r2)
13. [[army-intelligence-article-the-united-states-army]]
14. [[government-use-of-ai-by-key-us-military-branches-techtarget]]
15. [[final-report-on]] (EBA GL)
16. [[data-localisation-requirement-in-reserve-bank-of-india-rbi-storage-of-payment-sy]]
17. [[implementing-local-open-source-llms-for-chatbot-based-document-retrieval-in-a-un]]
