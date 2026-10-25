---
title: 'LLM Inference: A Guide to Build vs. Buy'
id: llm-inference-a-guide-to-build-vs-buy
tags:
- internal-llm-inference-stacks-7f0c90
- build-vs-buy
- vendor-marketing
- gpu-utilization
- break-even-threshold
- on-call-burden
created: '2026-10-09T22:56:21.449087Z'
updated: '2026-10-10T14:52:17.110167Z'
source: https://www.fortegrp.com/insights/llm-inference-a-guide-to-build-vs-buy
source_domain: www.fortegrp.com
fetched_at: '2026-10-09T22:56:21.426667Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'VENDOR SOURCE (Forte Group, an AI/software consulting firm; authored by
  Forte''s CTO Lucas Hendrich, Aug 2026) but the most data-dense and best-sourced
  piece in this batch, with external citations rather than just vendor assertion.
  Central claim: the standard build-vs-buy volume-threshold model is ''arithmetically
  clean and operationally wrong'' because it assumes purchased hardware will be busy.
  Cites Cast AI''s 2026 State of Kubernetes Optimization Report, drawn from ~23,000
  production clusters across AWS/Azure/GCP, putting AVERAGE ENTERPRISE GPU UTILIZATION
  AT 5 PERCENT — called out explicitly as the number that ''is enough to break most
  build-versus-buy spreadsheets.'' Cites Spheron''s 2026 inference cost analysis:
  an 8-GPU H100 SXM5 pod (~$19.20/hr) serving Llama 3.1 70B FP16 via vLLM at 2,800
  tokens/sec costs ~$1.90/million tokens AT SATURATION (a ceiling, not an expectation);
  the same analysis models a realistic internal deployment (500 daily active users,
  9am-6pm weekday traffic, near-zero overnight/weekends) at 22% average GPU utilization
  — continuous batching lifted that to 68%, and FP8 quantization let the same 4-GPU
  footprint absorb 1.8x the traffic. Cites BenchLM''s Token Price Index (July 2026,
  130 models): median $1.00/M input, $4.00/M output tokens — a 22%-utilized self-hosted
  node does not undercut that; ''it is paying a premium and owning the outage as well.''
  Cites CloudZero''s FinOps in the AI Era 2026 report: 78% of organizations cannot
  distinguish AI costs from general cloud spend. On compliance: cites Gartner Predicts
  2026 (>75% of EU/Middle East enterprises will geopatriate workloads by 2030, vs
  <5% baseline today) and Deloitte''s Dec-2025 AI Infrastructure Survey (86% expect
  AI infra budgets to more than triple in 3 years); argues most compliance obligations
  are CONDITIONAL (satisfiable via BAA, ZDR commitment, regional tenancy) rather than
  absolute prohibitions, with a short enumerated list of genuine prohibitions (ITAR,
  classified/air-gapped, German BSI KRITIS, certain GDPR special-category data without
  an adequate transfer mechanism); and distinguishes data residency / processing locus
  / jurisdiction as three separately-satisfied obligations (citing the US CLOUD Act
  as why EU residency does not solve US jurisdictional exposure). On capability: cites
  Epoch AI (Jan-May 2026 window) finding the best open-weight models lag the closed
  frontier by an average of 4 months / ~8 points on Epoch''s Capability Index, and
  the Stanford AI Index 2026 (March 2026 snapshot) reporting 1,503 Arena points for
  the leading closed model vs 1,454 for the leading open-weight model — while flagging
  that Epoch itself believes this likely UNDERSTATES the gap. On hardware pricing
  volatility: H100 rental spanned $2.01-$12.29/GPU-hour as of July 2026 (CloudZero);
  SemiAnalysis''s one-year H100 rental index rose ~40% from $1.70/GPU-hour (Oct 2025)
  to $2.35 (March 2026) even as mid-tier API pricing fell ~80% between early 2025
  and early 2026 and frontier blended pricing roughly doubled after Jan 2026. Six
  concrete recommendations: instrument workloads for a full quarter before buying;
  classify compliance as absolute-prohibition vs conditional; sort workloads by sensitivity
  x capability and only self-host the sensitive/low-capability quadrant; fix batching/quantization
  before adding nodes; consolidate multiple applications onto one serving cluster
  to defend utilization; attribute cost per workload from week one. Explicitly states
  four unsolved limits: hybrid routers fail open on misclassification without a pre-agreed
  data classification policy; a shared vector index without per-requester permission
  filtering leaks regardless of self-hosting; public benchmark parity is not your
  parity; utilization forecasts are optimistic by default due to structural peak-to-average
  ratios. LONG SOURCE (2300 words), high citation density — strong candidate for primary-source
  chase of the Cast AI, Spheron, BenchLM, CloudZero, Epoch AI, Stanford AI Index,
  Gartner, and Deloitte reports it cites.'
utility_score: 13.0
---

# LLM Inference: A Guide to Build vs. Buy
Author
Lucas Hendrich
Date
August 7, 2026
Time to read
7 minutes
[Home](https://www.fortegrp.com/)
LLM Inference: A Guide to Build vs. Buy
### The recommendations, in summary:
  1. **Instrument your workloads for a full quarter before buying anything.**
  2. **Establish whether your compliance obligation is a genuine prohibition or a requirement you can satisfy with controls and contracts. Most are the second kind.**
  3. **Sort every workload by data sensitivity and required capability, and treat only the sensitive, low-capability quadrant as a candidate for local inference.**
  4. **Fix batching and quantization before adding nodes.**
  5. **Put several applications on one serving cluster rather than standing up a GPU per team.**
  6. **Attribute cost per workload from the first week.** ‍


Item one is where most evaluations stall. Few organizations can state token consumption by team or by workload, and that figure is the input every other step depends on. Forte Group built this measurement into its [AI PDLC framework](https://www.fortegrp.com/services/ai-solutions). Every agent session writes a structured record capturing the model invoked, the capability tier it routed to, input and output tokens, calculated cost, wall-clock duration, and outcome status. Those records support the views this decision actually requires: spend by project and by skill, average tokens per session for each workload type, and the modeled gap between actual blended cost and what the same work would have cost had every session run at premium tier. Running that measurement before the procurement conversation opens is what turns this decision from an argument into an arithmetic problem.Now let's break down this decision-making process.The standard build-versus-buy analysis for inference treats the decision as a volume threshold. Find the crossover point in tokens per day, compare it against the current API invoice, and buy hardware once you clear the line. The model is arithmetically clean and operationally wrong. It assumes the hardware you buy will be busy. Measured production telemetry says it will not be. Cast AI's 2026 State of Kubernetes Optimization Report, drawn from roughly 23,000 production clusters across AWS, Azure, and GCP, put average enterprise GPU utilization at 5 percent. That one number is enough to break most build-versus-buy spreadsheets.
‍
### The Utilization Math
Cost per million tokens on owned or reserved hardware is a division problem: cluster cost per hour divided by realized throughput. Spheron's 2026 [inference cost analysis](https://www.spheron.network/blog/ai-inference-cost-economics-2026/) works the arithmetic on a common configuration. An eight-GPU H100 SXM5 pod at roughly 19.20 dollars per hour, serving Llama 3.1 70B in FP16 through vLLM at 2,800 tokens per second, produces a cost of approximately 1.90 dollars [per million tokens](https://packet.ai/blog/llm-inference-cost). That is a competitive number. It is also a ceiling, not an expectation, because it assumes the pod is saturated.The same analysis models a realistic internal deployment: 500 daily active users, traffic concentrated between nine and six on weekdays, near zero overnight and on weekends. Average GPU utilization in that scenario was 22 percent. The cluster is billed for 168 hours a week and does useful work for a fraction of them. Introducing continuous batching lifted utilization from 22 percent to 68 percent, and FP8 quantization let the same four-GPU footprint absorb 1.8 times the traffic.Compare that against the rented alternative. As of late July 2026, the median published price across the 130 models tracked by BenchLM's [Token Price Index](https://axis-intelligence.com/ai-inference-cost-statistics/) was 1.00 dollar per million input tokens and 4.00 dollars per million output tokens. A self-hosted node running at 22 percent utilization is not undercutting that. It is paying a premium and owning the outage as well.The volume threshold is a proxy. Utilization is the actual variable. Volume only matters because sustained volume is one way to produce utilization, and it is not the only way. Three internal applications at moderate throughput, consolidated onto one serving cluster, reach defensible utilization that no single application would justify alone.
###  Where Compliance Actually Binds
Compliance runs ahead of cost in the sequence, and the reason is narrower than it sounds. A genuine legal prohibition removes an option from the menu, and you do not price an option you cannot buy. Modeling token economics for a hosted API that your regulator has already ruled out is wasted analysis, and it is a common way for an evaluation to consume a quarter and produce nothing.The qualifier matters more than the rule. Very few obligations are actual prohibitions. Most are requirements that can be satisfied with controls, contracts, and documentation: a business associate agreement, a zero data retention commitment, a processing addendum, a tenancy in the right region, an audit log the regulator will accept. Those convert an apparent veto into a priced mitigation, at which point compliance stops being a gate and becomes another line inside the same cost comparison.The claim is not that compliance outranks cost. It is that you have to establish which category you are in before modeling anything, because the two categories are analyzed differently. The set of obligations that genuinely forecloses hosted inference is short. ITAR-controlled technical data. Classified and air-gapped environments. Specific critical infrastructure mandates, including the German BSI guidance for KRITIS operators. Certain special-category personal data under GDPR where no adequate transfer mechanism exists. Outside that set, you are negotiating terms rather than obeying a prohibition, and the negotiation has a price you can compare against a GPU.This is also where one word hides three obligations. Data residency is a question of where bytes sit at rest. Processing locus is a question of where inference actually executes. Jurisdiction is a question of which government can compel disclosure. They are satisfied by different architectures at very different price points. Data held in an EU data center by a United States provider still sits inside a US corporate entity subject to the [CLOUD Act](https://en.wikipedia.org/wiki/CLOUD_Act), which means residency was solved and jurisdiction was not.The pressure is real, and companies are already putting budget behind it. Gartner's Predicts 2026: AI Sovereignty report projects that more than 75 percent of European and Middle Eastern enterprises will geopatriate virtual workloads by 2030, against a baseline below 5 percent today. Deloitte's AI Infrastructure Survey from December 2025 found that 86 percent of enterprise respondents expect AI infrastructure budgets to more than triple over three years.The practical instruction is narrow. Require legal and compliance to state in writing which of the three obligations binds, and whether it binds absolutely or conditionally, before anyone prices a GPU. A meaningful share of organizations buy jurisdictional control when the regulation only required processing locus, and a managed deployment inside their own cloud tenancy would have satisfied it at a fraction of the capital cost and none of the operating cost.
‍
Build an Enterprise AI Platform That Scales
[Talk to an Expert](https://www.fortegrp.com/contact-us)
‍
### The Capability Gap
The best model you can own is the best open-weight model. Frontier laboratories sell tokens and retain weights, so any workload that requires frontier reasoning leaves your network regardless of what the cost model concludes. That constraint caps the entire exercise.The size of the discount is smaller than most executives assume. [Epoch AI](https://epoch.ai/data-insights/open-closed-eci-gap), analyzing the window from January through late May 2026, found that the most capable open-weight models lagged the closed frontier by an average of four months, or roughly 8 points on its composite Epoch Capability Index. The Stanford AI Index 2026 technical performance chapter reported 1,503 Arena points for the leading closed model against 1,454 for the leading open-weight model in its March 2026 snapshot.Two qualifications matter more than the headline. First, Epoch states plainly that its own estimate may understate the gap, because open-weight models tend to perform worse on private benchmarks than public ones, and leading laboratories do not always release their strongest systems. Second, a composite score erases the shape of the capability. A model can sit at parity on extraction, fall behind on multi-step agentic work, and recover on a narrow classification task.That shape is what should drive placement. Consider a claims pipeline in a regulated payer environment. Document extraction and coding are high volume, protected-health-information heavy, and narrow. Four months of frontier lag is noise there, and the sensitivity argues for local execution. Adjudication rationale is low volume, high consequence, and reviewed downstream by a human who may not catch a plausible-sounding error. Four months of lag is not noise there. Two workloads inside one business process, two different answers.In practice, that judgment becomes a tier map. In the Forte Group AI PDLC framework, each unit of work declares the capability tier it requires, and a routing layer resolves that declaration to a model endpoint at configuration time rather than at runtime. Premium tier covers architecture, complex reasoning, and code generation. A standard tier absorbs structured, well-specified work such as implementation planning and test case generation, where open-weight models are competitive. A lightweight tier takes summarization and documentation updates. The work definitions themselves do not change when the routing changes, and the routing layer is the only component holding endpoint configuration.That last part matters more than where the work lands on day one. A placement decision becomes reversible without touching the applications sitting on top of it, which is the only defensible posture when the capability gap and the price of both options move every quarter.‍
### Hardware Is the Cheap Part
Treating hardware as the decisive cost is the second common error. [H100 rental](https://www.cloudzero.com/blog/h100-gpu-cost/) spanned roughly 2.01 to 12.29 dollars per GPU-hour across providers as of July 2026, and a single card purchases in the 25,000 to 40,000 dollar range depending on form factor. Those numbers are falling on the spot market.Committed capacity is moving the other way. SemiAnalysis's [one-year H100 rental index](https://www.spheron.network/blog/nvidia-h100-price-2026/) rose approximately 40 percent, from 1.70 dollars per GPU-hour in October 2025 to 2.35 dollars by March 2026, as on-demand supply sold out and holders of reserved compute declined to release it. On the other side of the ledger, mid-tier API pricing fell roughly 80 percent between [early 2025 and early 2026](https://gpusmith.com/articles/en/h100-rental-price-history-trends), while frontier blended pricing roughly doubled after January 2026 as newer generations displaced older ones at higher list prices.A three-year hardware amortization is a three-year bet against a market that repriced in both directions inside eighteen months. That does not make the bet wrong. It makes it a bet, and it should be underwritten as one.The genuinely non-amortizing line is the operating function. Someone patches drivers, watches tail latency, manages the model upgrade cadence, and answers the page. That is a standing headcount cost that persists at 5 percent utilization exactly as it does at 70 percent. Most organizations cannot see it clearly. [CloudZero](https://www.cloudzero.com/blog/h100-gpu-cost/)'s FinOps in the AI Era 2026 report found that 78 percent of organizations cannot distinguish AI costs from general cloud spend. A decision you cannot attribute is a decision you cannot evaluate.‍
### Limits
Four things this framework does not solve.
  * **Hybrid routing is a system, and it is the component most likely to fail open.** A single misclassified request sends the exact record the local lane was built to contain. The discipline that prevents it is a written data classification policy establishing which categories may reach which endpoint, agreed before the second endpoint is enabled rather than after an incident. The router also needs its own tests, its own monitoring, and its own failure mode analysis.
  * **The retrieval layer leaks before the model does.** A shared vector index without per-requester permission filtering will surface one user's confidential documents inside another user's answer. Self-hosting does not fix that, and it obscures the problem by making the perimeter feel solved.
  * **Public benchmark parity is not your parity.** Run an evaluation against your own data and your own failure definitions before accepting a capability claim from any leaderboard.
  * **Utilization forecasts are optimistic by default.** Peak-to-average ratios for internal enterprise tools are severe, and the overnight and weekend gap is structural rather than a tuning problem.‍


### What To Do
  1. **Instrument before you procure.** Measure tokens per day by workload, peak-to-average ratio, and sustained concurrency across a full quarter. Ninety days of telemetry costs nothing and settles most of the argument.
  2. **Classify every workload on two axes.** Data sensitivity and required capability. Only the high-sensitivity, low-capability quadrant is a serious candidate for local inference.
  3. **Establish whether the obligation is absolute or conditional.** Residency, processing locus, and jurisdiction are three obligations with three architectures and three price points, and most of them can be satisfied contractually rather than physically.
  4. **Solve utilization before you solve capacity.** Continuous batching and quantization routinely deliver more effective throughput than adding nodes, and they cost engineering time rather than capital.
  5. **Consolidate workloads onto shared serving infrastructure.** Spreading fixed cost across several applications is the most reliable path to utilization that defends the investment.
  6. **Attribute cost per workload from the first day of the deployment.** Without attribution, the review at renewal is an opinion.‍


Owning inference is a control decision that has to survive a cost test. Buy control when the regulation requires it. Buy hardware when the utilization justifies it. Those are two separate decisions, and most organizations are making them as one.
‍
#### About the author
[Lucas Hendrich](https://www.fortegrp.com/authors/lucas-hendrich)
CTO at Forte Group
With 20+ years' technical experience spanning hands-on to leadership roles, Lucas is passionate about driving value, innovation, growth, and sustainability. Prior to Forte Group, he served as CTO for BairesDev and held senior leadership roles at Kestra Financial, Globant and Blackbaud.
[See all articles](https://www.fortegrp.com/authors/lucas-hendrich)
## You may also like
#### [Context Is Everything (and everything is code) Learn more ](https://www.fortegrp.com/insights/context-is-everything-and-everything-is-code)
#### [Forte Group Achieves Select tier in Claude Partner Network, Backed by Governed AI Practice Already Proven in Production Learn more ](https://www.fortegrp.com/insights/forte-group-achieved-claude-partner-network-as-a-select-partner-backed-by-a-governed-ai-practice-already-proven-in-production)
## Transform AI into a Scalable Delivery Capability
83% faster delivery. Under 10% rework. See exactly how Xceptor got there.
