---
title: How Hebbia uses Baseten to power AI workflows for the world's leading financial
  institutions
id: how-hebbia-uses-baseten-to-power-ai-workflows-for-the-worlds-leading-financial-i
tags:
- internal-llm-inference-stacks-7f0c90
- baseten-managed-middle
- baseten-case-study
- managed-middle-unconfirmed
created: '2026-10-10T10:58:14.921357Z'
updated: '2026-10-10T14:52:17.272238Z'
source: https://www.baseten.co/resources/customers/hebbia/
source_domain: www.baseten.co
fetched_at: '2026-10-10T10:58:14.899891Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Baseten vendor case study: Hebbia (financial-intelligence search for asset
  managers/investment banks) runs a ''dedicated deployment of an open-source LLM''
  on Baseten, using Baseten''s KV-aware cache routing and speculative decoding. Reports
  2.5x TPS, 4x TTFT improvement, >10x cost reduction vs its prior closed-source provider.
  Does NOT confirm Hebbia itself fine-tuned/customized the weights -- the only fine-tuning
  language is a forward-looking customer quote that Baseten gives them ''the ability
  to deploy custom fine-tuned models'' and that they ''can also shift to new open-source
  models (or custom LLMs)''. Fails part 1 of the managed-middle two-part test (customized
  weights) on the evidence given; confirms part 2 (provider operates serving).'
---

Use your OpenAI commitments on Baseten open models. [Learn more](https://www.baseten.co/talk-to-us/openai/)
Case study
# How Hebbia uses Baseten to power AI workflows for the world's leading financial institutions
## 2.5x
improvement in TPS
## 4x
improvement in TTFT
## 10x
lower cost by shifting to Baseten 
## Company overview 
[Hebbia](https://www.hebbia.com/) is the leader in institutional intelligence. The world's most sophisticated asset managers, investment banks, professional services firms, and Fortune 500 companies rely on Hebbia to turn massive amounts of data into fast, reliable insights. In finance, speed is a competitive advantage. Professionals driving deals and investment decisions need answers they can trust, consistently, in a secure and collaborative environment built for how they actually work.
### Impact highlights 
  * 2.5x improvement in TPS and 4x improvement in TTFT using the Baseten Inference Stack
  * Reduced cost by over 10x by shifting to Baseten


##  **Real time data insights through chat**
###  _Challenge_
Finance professionals expect fast and accurate responses that they can rely on as they navigate competitive and tight deal processes. Hebbia needed an inference provider that could serve latency-sensitive chat traffic quickly, with high uptime, to ensure they could create a user experience befitting finance professionals working in large institutions.
As Hebbia’s user base scaled they faced limited flexibility to optimize for the latency and reliability their customers demanded.
> "The reason why we came to Baseten in the first place was the latency requirements. With our bursty workloads, we got queued for our requests similar to any other user of AI. And our customers don't care about who's queuing you."
###  _Solution_
Hebbia partnered with Baseten for a dedicated deployment of an open-source LLM. To maximize the efficiency of Hebbia’s workload, Baseten utilizes KV-aware cache routing which ensures requests are routed to each instance that hold relevant cached context. This greatly reduces latency on repeat requests where users feel delays the most. Hebbia’s deployment also utilizes speculative decoding which improves token generation speed by drafting multiple tokens in parallel rather than sequentially. 
With Baseten’s elastic infrastructure, Hebbia only pays for the GPUs they use rather than having to reserve fixed-capacity which greatly reduces cost. Hebbia can also shift to new open-source models (or custom LLMs) all with the same infrastructure which gives their team the flexibility to use the latest and greatest models whenever they become available. This ensures their customers always have access to the best quality outputs and can stay on the bleeding edge.
> "Our customers really care about speed, reliability, and the ability to deploy custom fine-tuned models for financial services. Baseten met all three criteria so far and it’s been a boon to our business."
###  _Result_
Baseten delivered consistent, low-latency inference for Hebbia's high priority chat traffic, meeting the real-time experience their users expect in production. With Baseten, Hebbia saw a 2.5x increase in tokens per second and a 4x improvement in time to first token compared to their previous deployment. By shifting from a closed-source model provider to Baseten, Hebbia reduced inference costs by over 10x without compromising on the reliability or latency their enterprise customers depend on.
## Chosen by the world's most **ambitious builders**
Case study
[EliseAI case study](https://www.baseten.co/resources/customers/how-eliseai-trains-specialized-models/)
Case study
[Gamma case study](https://www.baseten.co/resources/customers/how-gamma-makes-building-presentations-criminally-fun/)
Case study
[Hebbia case study](https://www.baseten.co/resources/customers/hebbia/)
Case study
[Latent case study](https://www.baseten.co/resources/customers/latent-delivers-pharmaceutical-search-with-baseten/)
Case study
[Notion case study](https://www.baseten.co/resources/customers/how-notion-acheived-2-3x-lower-latency-with-ai-meeting-notes/)
Case study
[OpenCode case study](https://www.baseten.co/resources/customers/opencode/)
Case study
[OpenEvidence case study](https://www.baseten.co/resources/customers/openevidence-delivers-instant-medical-information-with-baseten/)
Case study
[Oxen AI case study](https://www.baseten.co/resources/customers/from-datasets-to-deployed-models-how-oxen-ai-builds-on-baseten/)
Case study
[Parallel case study](https://www.baseten.co/resources/customers/how-parallel-web-systems-pioneers-programmatic-web-with-high-throughput-agentic-i/)
Case study
[Patreon case study](https://www.baseten.co/resources/customers/patreon/)
Case study
[Speechify case study](https://www.baseten.co/resources/customers/speechify-real-time-text-to-speech/)
## Explore Baseten today
