---
title: What Early Open Weight Model Deployments Reveal About Production AI
id: what-early-open-weight-model-deployments-reveal-about-production-ai
tags:
- llm-inference
- model-layer
- internal-llm-inference-stacks-7f0c90
- named-org-deployment
- post-training
- continual-learning
created: '2026-10-09T22:56:27.800852Z'
updated: '2026-10-10T14:52:17.114919Z'
source: https://fastino.ai/blog/open-weight-model-deployment-observations
source_domain: fastino.ai
fetched_at: '2026-10-09T22:56:27.761987Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Fastino (vendor) blog, Sept 9 2026, surveying NAMED companies'' open-weight
  deployments: Uber, AT&T, Amplitude, Shopify, Pinterest, Harvey, Thomson Reuters.
  Cites McKinsey May 2026 Enterprise AI FinOps survey: 93% of qualified respondents
  exceeded AI budgets; spend rises ~4x moving from isolated use cases to enterprise-wide
  rollout. Uber burned its annual AI budget in Q1 2026 from coding-agent adoption
  (Axios). AT&T runs ~45 billion tokens/day across >1,000 internal use cases (WSJ),
  with open models now powering roughly a quarter of that usage and growing. Describes
  three deployment pathways with specific evidence: (1) hosted-base-model + system
  redesign -- Amplitude ran Kimi K2.7 unmodified on Fireworks dedicated GPU instances,
  scoring 64/100 on an internal 186-case eval (vs Sonnet 4.6''s baseline), then raised
  it to 73.7 (beating Sonnet''s 72.7) purely via system fixes (caching/parallelizing
  permission checks, single-agent compiler routing, typed chart schemas, remediations
  for tool loops/arithmetic/date handling) at ~1/3 the cost; Shopify self-hosted Qwen
  3 with a redesigned tool-scoped extraction system cutting inference cost 75x; Pinterest
  replaced part of Qwen3-VL''s vision stack with its own precomputed embeddings, cutting
  AI costs 90%. (2) post-training on proprietary data -- Harvey post-trained Kimi
  K3 via RL on ~1,750 synthetic/public/expert legal-task environments to build ''Tenet,''
  reporting it solves ~2x as many held-out legal-agent tasks as the base model; separately
  post-trained Qwen3.8-27B for firm-knowledge retrieval, reporting 58% fewer tokens/trajectory
  and 90% lower cost/query. (3) continual learning -- Thomson Reuters'' technical
  report (arXiv:2608.27147) describes building its ''Thomson'' model from Qwen checkpoints
  via value realignment, mid-training and post-training, using a ''CapTrack'' framework
  and curating 200B tokens from a 19T-token pool; final 3-week GPU training run cost
  under $450K but total development (staff, compute, domain experts, infra) cost ~$40M.
  UK Business Data Survey 2026: 73% of businesses uncomfortable with data being used
  to train external AI models.'
utility_score: 15.0
---

[ Introducing GLiDE, the first thinking decision model.](https://fastino.ai/models/demo-library)
[Back](https://fastino.ai/blog)
# What Early Open Weight Model Deployments Reveal About Production AI
Guide
[Thao Nguyen](https://www.linkedin.com/in/nguyenpthao/)
September 9, 2026
Open weight models are moving from developer experiments into production AI systems. To understand what that shift looks like in practice, we looked at recent deployments and experiments at companies including Uber, AT&T, Amplitude, Shopify, Pinterest, Harvey, and Thomson Reuters. We wanted to see why these companies are exploring open weight models, which strategies they are using, and what they are learning along the way.
The motivations are familiar: companies are responding to rising costs, data and governance requirements, and the possibility of building more capable systems for specialized workloads. The approaches vary more widely. Some are testing hosted open weight models, some are redesigning the system around them, and others are post-training models on proprietary data. Taken together, these early deployments suggest companies are still deciding, gradually, how much of their AI stack they should rent and how much they should own.
## Why companies are looking beyond frontier models
### Cost overruns are becoming a production problem
Reports of AI cost overruns are no longer rare. In [McKinsey's May 2026 Enterprise AI FinOps survey](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-cost-of-intelligence-how-cios-can-manage-ai-demand-at-scale), 93% of qualified respondents said their organizations had exceeded AI budgets. The survey also found that spend rises nearly fourfold as companies move from isolated use cases to enterprise-wide deployment. McKinsey's analysis points to why: an agent can make many model calls, retry, and accumulate context within one user interaction. Those costs rarely resemble traditional software expenses.
Recent company examples make this concrete. [Uber burned through its annual AI budget in the first quarter](https://www.axios.com/2026/08/27/ai-uber-spending) of 2026, as adoption of coding agents accelerated. Microsoft moved Copilot Cowork to usage-based pricing after testing showed that heavy users could make unlimited use uneconomic.
At AT&T's scale, the problem is especially visible. The company uses roughly 45 billion AI tokens a day across more than 1,000 internal use cases, according to the [Wall Street Journal](https://www.wsj.com/cio-journal/why-at-t-is-betting-big-on-open-weight-ai-a0ea03b1). At that volume, a small mismatch between a task and the model chosen for it becomes a material operating expense. Gartner estimates an agentic reasoning workflow can cost providers at least five times as much to run as a basic chatbot interaction. [Its broader forecast](https://www.gartner.com/en/newsroom/press-releases/2026-08-17-gartner-predicts-ai-inference-costs-per-agentic-workflow-will-increase-more-than-fivefold-through-2028) is that inference cost per agentic workflow will rise more than fivefold through 2028.
That's where open weight models come in. They are appealing because they create another economic option with a lower token price, but that alone does not solve an uncontrolled system. The companies reducing spend most effectively are also measuring usage, constraining waste, and changing how work is divided among models.
### Proprietary data is both an asset and a governance concern
Companies also worry about what happens when their most valuable context leaves their environment. An enterprise agreement may limit training on customer data, but prompts, retrieved documents, tool outputs, and user interactions can still pass through a vendor-operated service. That raises questions about data residency, access control, auditability, and how much of a product's hard-won knowledge remains within the company.
The concern is broad. In the [UK Business Data Survey 2026](https://www.gov.uk/government/statistics/uk-business-data-survey-2026/uk-business-data-survey-2026), 73% of businesses said they would be uncomfortable with their data being used to train external AI models. That question is about training, not ordinary API inference, but it makes the underlying caution clear. AT&T's chief data and AI officer called the company's data its "gold mine." [Sequoia makes a similar case](https://sequoiacap.com/article/own-your-intelligence-a-how-to-guide/): when feedback, evaluations, customer interactions, and domain data make a product better, a company may want that data to stay within its walls.
Yet, open weight models do not automatically guarantee data control. A managed endpoint still means using another company's infrastructure. The distinction is that a company can choose where the model runs and how its data is handled: on its own infrastructure, in a private cloud, or through a provider with the right residency and governance controls. That flexibility matters most in regulated or data-intensive work, but it is increasingly relevant to any product whose advantage comes from proprietary context.
## Open weight model deployment pathways
There is no single open weight deployment pattern.
The companies that we’ve looked at for this post are making choices across two dimensions: how much infrastructure they want to own, and how deeply they want to change the model.
That produces a progression from consuming a hosted open-weight model, to adapting the surrounding system, to post-training a specialized model, and eventually to running an ongoing learning process.
### 1. Deploy an open weight base model and improve the system around it
The lowest-commitment path is to use a hosted open weight model and keep the weights unchanged. This lets a team test a model without managing serving infrastructure.
[Amplitude’s Kimi deployment](https://amplitude.com/blog/open-weights-experimentation) shows this approach in practice. The team ran Kimi K2.7 on Fireworks’ dedicated GPU instances without post-training the model. On Amplitude’s 186-case internal evaluation suite, Kimi initially scored 64, behind Sonnet 4.6. Most of the engineering work focused on identifying recurring agent failures and improving the surrounding system. The team cached and parallelized permission checks, routed chart work through a single-agent compiler flow, and introduced typed, validated chart definitions. It also added targeted remediations for tool loops, arithmetic and date handling, capability grounding, reasoning leakage, empty final responses, and context-length limits. With these changes, Kimi scored 73.7 on the same test cases, edging past Sonnet’s 72.7, at roughly one-third the cost.
Uber is also experimenting with open weight models, but the companies experience shows that model choice is only one cost lever. Routing, caching, token limits, and observability can materially change the economics of the same workload.
[Shopify](https://aidatainsider.com/news/shopify-uses-qwen-3-to-cut-inference-cost-by-75x/) paired a self-hosted Qwen 3 model with a redesigned, tool-scoped extraction system. [Pinterest](https://venturebeat.com/orchestration/pinterest-cut-ai-costs-90-by-gutting-a-frontier-models-vision-layer) replaced part of Qwen3-VL's vision stack with its own embeddings. In both cases, the model swap mattered, but the performance gains and cost savings also came from changing the system that supplied context and performed the work.
### 2. Post-train open weight models with proprietary data
The next path is post-training the model itself. Post-training is an umbrella term that can include supervised fine-tuning, preference optimization, reinforcement learning, and related techniques. The right method depends on the problem.
[Harvey's Tenet development](https://www.harvey.ai/blog/post-training-update-harvey-tenet) illustrates the higher-investment version. Harvey started with Kimi K3 and post-trained it through reinforcement learning on roughly 1,750 legal-task environments built from synthetic, public, and expert-created data. The company reports that Tenet solved nearly twice as many held-out legal-agent tasks as the base model. In a separate [Firm Knowledge project](https://www.harvey.ai/blog/legal-agent-bench-law-firm-knowledge), Harvey post-trained Qwen3.8-27B to work more efficiently with a firm's internal knowledge, reporting 58% fewer tokens per completed trajectory and a 90% lower cost per query.
There is no clean one-technique-to-one-problem map. Retrieval may be the right fix when a model lacks current information. Supervised fine-tuning can improve specialized tasks when a team has clear examples of the desired behavior or output. Preference optimization is useful when feedback is comparative, while reinforcement learning is better suited to multistep tasks that can be evaluated in an environment or against a rubric. Distillation can make a capable system smaller and faster to run.
Choosing the right post-training technique is just the beginning, the next challenge is measuring meaningful improvement in the work, whether that is quality, latency, cost, or all three. [Hims & Hers' CEO](https://www.businessinsider.com/hims-hers-ceo-andrew-dudum-datasets-ai-costs-open-weight-2026-8) argues that a model trained on the company's closed-loop patient data outperformed available alternatives and could reduce costs by 70% to 80%. That is an ambitious company claim, but it captures the logic of post-training: the relevant advantage does not come from solely swapping base models. It is the combination of a capable base, proprietary data, domain feedback, and a way to measure whether the model is becoming more useful.
### 3. Build a continual learning discipline
Continual learning is more specific than simply fine-tuning a model again. It is repeated training designed to add new knowledge or skills without degrading existing capabilities. Its two competing goals are **plasticity** , the ability to learn something new, and **stability** , the ability to retain prior capabilities.
[Thomson Reuters' technical report](https://arxiv.org/abs/2608.27147) highlights the importance of continual learning in the development of its Thomson model. The team began with Qwen checkpoints, then applied value realignment, mid-training, and post-training. It used CapTrack to monitor target performance and general capability retention, and curated 200 billion tokens from a pool of more than 19 trillion tokens, including proprietary content and replay data.
The costs to build Thomson model show why continual learning is more than a compute decision. The company estimates that its final three-week GPU run cost under $450,000, while the full development effort cost about $40 million. That broader figure includes staff, compute, domain experts, vendor partnerships, reusable research, infrastructure engineering, and experimentation.
The GPU bill is only one part of the investment. Building a system that can improve models over time also requires data pipelines, feedback mechanisms, evaluations, reward design, and production infrastructure. Thomson Reuters has built a training and evaluation discipline for continual learning; whether it becomes a fully automated production-feedback loop remains an open question.
## Four early lessons from these deployments
### 1. Adoption is incremental
The early pattern is not a binary switch from closed to open models. Companies start with workloads where costs are material, the task is reasonably bounded, and they can measure success. They compare an open weight option against the existing system, route a limited share of traffic, and expand only when quality, latency, and cost hold up. This limits the risk of a model change while revealing the production behavior that offline testing cannot capture.
Amplitude tested Kimi against its own eval suite before moving to a segmented rollout to determine whether some customer groups should remain on Sonnet. AT&T has open models powering roughly a quarter of its AI usage across more than 1,000 use cases, with plans to increase that share. Uber is experimenting with open weight options while using routing, caching, session caps, and cost visibility to control spend across its broader AI stack.
Over time, these incremental decisions create a model portfolio. Companies are not declaring one provider or model the winner. They are assigning different models to different workloads, segments, and cost profiles, then adjusting those allocations as new evidence arrives. The portfolio is the result of a careful rollout process, not a single platform decision.
### 2. Rigorous evals are a prerequisite
A good eval suite does more than compare models. It tells a team what to fix, what not to break, and whether a change is ready to reach more users.
Amplitude’s 186-case eval suite was designed around the shape of its production traffic. It showed that roughly 80% of Kimi’s failures came from the head agent and resolved those failures into repeatable patterns, including tool loops, arithmetic errors, capability hallucinations, invalid outputs, and context overflows. That gave the team a concrete remediation roadmap. Kimi’s score rose from 64 to 73.7 on the same suite, slightly above Sonnet’s 72.7. Amplitude is now using a staged rollout to test customer-segment effects that its offline evaluations cannot capture.
Thomson Reuters made rigorous evaluation central to its training process. It used CapTrack, a framework for tracking capability change against the starting model, alongside target-domain benchmarks and expert-authored task rubrics. The team used smaller benchmark subsets for frequent iteration and reserved separate test sets for final evaluation. This kind of discipline lets a company detect regressions, direct model and system improvements, and decide when a model is ready for broader production use.
### 3. Cost savings come from both model swap and system design
A cheaper model is only one part of the equation. Serving is a separate decision: at sustained volume, dedicated managed capacity can trade variable per-token pricing for a fixed hourly cost and more consistent latency. That was part of Amplitude’s reasoning for running Kimi on dedicated Fireworks GPUs.
The larger savings can potentially come from redesigning how the workload runs. Uber cut waste through routing, caching, session caps, and cost visibility. Shopify decomposed a monolithic workflow. Pinterest moved vision work out of the request path by replacing a live vision encoder with its own precomputed embeddings. Amplitude fixed recurring failures in its agent system. The recurring unit of improvement is not the model alone, but the model, serving setup, harness, tools, context, and operating controls.
### 4. Owning more of the stack shifts the investment, but AI spending is likely here to stay
Moving beyond a frontier API can reduce variable inference costs and create a model that fits a specific business better. It also creates new responsibilities: serving, data curation, security, evaluations, training, and monitoring. The right comparison is the total cost and strategic value of a system the company can improve over time.
NEXT
[American Open Weight Models: The Opportunity Ahead](https://fastino.ai/blog/american-open-weight-models-the-opportunity-ahead)
PREVIOUS
[The Best Open Weight Models of Summer 2026](https://fastino.ai/blog/best-open-weight-models-2026)
Products
[Fastino API](https://agent.fastino.ai)
[NVIDIA Collaboration](https://fastino.ai/nvidia-collaboration)
[Demo Library](https://fastino.ai/models/demo-library)
Fastino Labs
[Privacy Policy](https://agent.pioneer.ai/privacy)
Connect
### Subscribe to our newsletter
Get the latest updates on model releases, product news, and research.
Fastino Inc. (“Fastino”) develops specialized AI models and provides APIs designed to support structured data extraction, classification, reasoning, and production AI workflows. Fastino is a technology company and does not provide legal, financial, compliance, or advisory services.
Any outputs, predictions, classifications, or decisions generated through Fastino models are based on the configuration, data, and implementation provided by the customer. Fastino does not control, verify, or guarantee the accuracy, completeness, or suitability of model outputs for any specific purpose. By using this website or Fastino’s models and services, you acknowledge that all content and outputs are provided for informational and operational purposes only and agree to our Terms of Use and Privacy Policy.
2026 Fastino Inc.
All rights reserved
