---
title: Open-Weight Models Are Gaining Ground in Enterprise AI | Constellation Research
id: open-weight-models-are-gaining-ground-in-enterprise-ai-constellation-research
tags:
- llm-inference
- model-layer
- internal-llm-inference-stacks-7f0c90
- open-weight-share-rise
- gateway-telemetry
- frontier-vs-open-weight
created: '2026-10-09T22:55:51.868091Z'
updated: '2026-10-10T14:52:17.093268Z'
source: https://www.constellationr.com/research/blog/open-weight-models-are-gaining-ground-enterprise-ai
source_domain: www.constellationr.com
fetched_at: '2026-10-09T22:55:51.844070Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Constellation Research analyst piece (Chirag Mehta, Aug 24 2026) built entirely
  on Vercel AI Gateway TELEMETRY (actual token-routing volume through one specific
  commercial LLM gateway, not a survey) — explicitly caveated as ''just one gateway''
  not overall market share. Open-weight SHARE OF TOKEN VOLUME rose from ~11% (April
  2026) to 29% (June 2026), then to 28.4% on June 24 and a ''record'' 62% by August
  22; roughly half of token volume over the latest two-month window. Critically, open-weight
  models captured 29% of token volume on Vercel in June while accounting for under
  4% of SPEND, versus Anthropic''s 32% of tokens but 61% of spend — i.e., open models
  win volume/cheap-task share long before revenue share. Notes heavy multi-model usage
  scales with request volume (teams at >10M requests/month use ~35 models on average
  per Vercel data). Documents that Chinese labs (DeepSeek, Qwen, GLM, Kimi) are setting
  the pace in open-weight releases and gaining real production traffic on gateways;
  Hugging Face counted >151,000 downstream Qwen derivative repos, with ~180-210 new
  ones/day in H1 2026. Argues enterprises adopt open-weight models via managed inference
  providers (Together AI: ~30B to >400 trillion tokens/month in 9 months; Fireworks:
  >40 trillion tokens/day, >95% of traffic from customer-specialized models) FASTER
  than they adopt self-hosted inference — i.e. gateway/provider telemetry captures
  API-mediated open-weight consumption, not self-hosted internal deployments, which
  this metric structurally cannot see.'
utility_score: 17.0
---

[ Skip to main content ](https://www.constellationr.com/research/blog/open-weight-models-are-gaining-ground-enterprise-ai#main-content)
# Open-Weight Models Are Gaining Ground in Enterprise AI
August 24, 2026
[ Copy link Link Copied ](https://www.constellationr.com/research/blog/open-weight-models-are-gaining-ground-enterprise-ai "Copy link")
For a long time, the open-weight model discussion centered mostly on developers. Downloads, experiments, local inference, fine-tunes and benchmarks gave us plenty of evidence that developers were interested, but it was harder to assess whether that interest would turn into meaningful production use. We have now begun to get some strong evidence.
According to Vercel’s AI Gateway, open-weight models accounted for about 11% of token volume in April 2026. By June, that had risen to 29%. In June, roughly one in eight enterprise AI Gateway customers was already running at least one open-weight model in production.
More recent data suggests that growth has continued. Open weights accounted for 28.4% of Vercel AI Gateway token volume on June 24. By August 22, their share had reached a record 62%. Over the latest two-month period, they accounted for about half of all token volume.
_Source: Vercel_
This is just one gateway and should not be treated as overall market share. But the speed of the increase is worth paying attention to, especially as the same models are also seeing adoption across other inference providers and developer platforms.
That leads to my first hypothesis:
### **Open-weight models are moving beyond developer experimentation and becoming a meaningful part of production AI workloads.**
The Vercel data is only one view of the market, but the combination of rising token share and early enterprise usage makes the production signal difficult to ignore.
The economics behind that adoption are also worth looking at. In June, open-weight models generated 29% of Vercel’s token volume while accounting for less than 4% of spend. Anthropic, by comparison, accounted for 32% of token volume but 61% of spend.
_Source: Vercel_
That leads to my second hypothesis:
### **Open-weight models may win significant volume well before they win significant revenue.**
Closed frontier models still capture a disproportionate amount of spending. Enterprise customers appear willing to pay for stronger models when the quality of the answer or the cost of failure justifies it. But they do not need the most expensive model for every task.
As enterprise customers deploy more AI, they will have an incentive to match models to workloads. The hardest reasoning task might go to a frontier model, but routine classification, extraction, repetitive agent steps, coding tasks or highly specialized workloads might go somewhere else. This also changes how I think about enterprise adoption.
There is already evidence that larger AI users are becoming heavily multi-model. Vercel found that teams processing 100 to 1,000 requests per month regularly used an average of one model. At 1,000 to 10,000 requests, that increased to three. At 100,000 to 1 million requests, it was eight. Teams processing more than 10 million requests per month regularly used an average of 35 models.
_Source: Vercel_
My third hypothesis:
### **Enterprise AI will become a multi-model market, and open weights will enter through that door.**
A customer does not need to decide between Claude and Qwen, or between GPT and DeepSeek. It can use different models for different tasks. Claude or another frontier model might handle complex reasoning, whereas an open model might execute repetitive agent steps. A smaller specialized model could classify documents while another model could be tuned for a specific domain or company data. That also means enterprise adoption of open weights does not necessarily mean customers will download a model and start operating GPU clusters themselves.
Together AI says its API traffic grew from roughly 30 billion to more than 400 trillion tokens per month in nine months. Fireworks says it serves more than 40 trillion tokens per day. Fireworks also says more than 95% of its traffic comes from models specialized on customers’ proprietary data. These companies are making it possible for customers to choose and customize models without taking on all of the infrastructure work themselves.
My fourth hypothesis:
### **Enterprise customers will adopt open-weight models faster than they adopt self-hosted inference.**
An enterprise customer can choose Qwen, DeepSeek or another open-weight model while letting an inference provider handle GPUs, scaling, reliability and much of the operational complexity. That makes open weights easier to adopt without requiring companies to build and operate their own inference infrastructure.
The distinction between “closed model through an API” and “open model running in my data center” is becoming less useful. Open-weight models can increasingly be consumed as managed services while still giving customers more choice over the model itself.
There is also a supply-side signal worth watching: what developers choose to build on. Qwen is a good example. Hugging Face recently counted more than 151,000 downstream Qwen derivatives, with developers creating roughly 180 to 210 new Qwen-derived repositories per day during the first seven months of 2026. These include fine-tunes, adapters, quantizations and other specialized versions. Downloading a model can indicate experimentation, but building on top of it represents a deeper commitment.
My fifth hypothesis:
### **The most important open model may not be the one that tops the benchmarks, but the one developers build on the most.**
Benchmark performance still matters and is worth paying attention to. But a model family becomes more useful as developers create specialized versions, tooling, deployment options and expertise around it. Over time, that surrounding activity can become an important indicator of adoption alongside benchmark scores. Qwen also points to a broader development in the open-weight market.
#### **Chinese labs are setting much of the pace in open-weight models.**
Qwen’s developer adoption is one indication, but the trend is broader. DeepSeek, GLM, Kimi and other Chinese model families are competing near the frontier while continuing to make weights available. Chinese labs have also been aggressive about releasing large models under relatively permissive licenses.
The usage data is beginning to reflect this as well. Models from DeepSeek, Qwen and GLM are attracting meaningful production traffic across gateways such as Vercel and OpenRouter. This makes China an important part of the open-weight discussion, not only as a source of new models but also as a source of models that developers and customers are actually choosing for production.
That does not mean Chinese model companies will capture all of the economic value. A model developed in China may be served by a US hyperscaler or inference provider, run on NVIDIA or AMD hardware, routed through a gateway, and customized by another company for an enterprise workload. Open weights can distribute value across several layers: the model developer, inference provider, cloud provider, hardware vendor, gateway, and the company specializing the model for a particular workload.
There is also a demand-side factor that could push adoption further: agents.
#### **Agentic AI could become one of the strongest economic drivers for open-weight adoption.**
Agents can make many model calls to complete a single task, and that changes the economics quickly. While a chatbot making one expensive model call may be reasonable, an agent making 20 or 50 calls to complete a business process creates a much stronger incentive to manage inference costs. This could favor open-weight models in workloads where they are capable enough and considerably less expensive. As agentic workloads grow, the cost difference between models is multiplied across many calls rather than showing up once.
That does not mean frontier models lose their place. Customers will continue to pay for them when stronger reasoning, reliability or accuracy justifies the cost. But agents make the economics of model selection much harder to ignore. This is why I am less interested in whether open models will “beat” closed models and more interested in whether open weights are becoming a durable part of enterprise AI.
There are several numbers worth watching over the next year: the share of production tokens going to open weights, the percentage of enterprise customers using at least one open-weight model, the rate at which developers build on major model families, the number of inference providers supporting them, and the types of workloads customers are willing to move to open models.
Developers adopted open weights first. Then more developers started building on them. Inference providers made them easier to consume without operating the underlying infrastructure. Now we are seeing production token volume rise, providing an early signal of enterprise adoption.
The next question is how broadly enterprise customers will use open-weight models, and where the economic value will accrue as they do.
[ Copy link Link Copied ](https://www.constellationr.com/research/blog/open-weight-models-are-gaining-ground-enterprise-ai "Copy link")
Vice President and Principal Analyst Constellation Research 
Chirag Mehta is Vice President and Principal Analyst focusing on cybersecurity, next-gen application development, and product-led growth. With over 25 years of experience, he has built, shipped, marketed, and sold successful enterprise SaaS products and solutions across startups, mid-size, and large companies. As a product leader overseeing engineering, product management, and design, he has consistently driven revenue growth and product innovation. He also held key leadership roles in product marketing, corporate strategy, ecosystem partnerships, and business development, leveraging his expertise to make a significant impact on various aspects of product success. His holistic research approach on cybersecurity is grounded in the reality that as sophisticated AI-led attacks become…...
[Read more](https://www.constellationr.com/user/chirag-mehta)
##### Published
August 24, 2026
##### Author
Categories
##### Business Themes
[Innovation & Product-led Growth](https://www.constellationr.com/business-themes/innovation-product-led-growth)
##### Related Blog Posts
## Results
Analyst Blog
September 14, 2026
### [Experience Data Is Getting a New Job: From Measurement to Decision Context](https://www.constellationr.com/research/blog/experience-data-getting-new-job-measurement-decision-context)
Data to Decisions
As a serial CMO/CPO, like many CX executives, I used experience management so...
Michael Ni 
Orbits Blog
September 12, 2026
### [Pragmatic, Practical Actions (Not Fear): We Can and Should Take Actions to Reduce Harms, Risks, and Concerns in the Present](https://www.constellationr.com/research/blog/pragmatic-practical-actions-not-fear-we-can-and-should-take-actions-reduce-harms)
Board Strategy
Unlike the actual tragedy of 9/11, conjured images of mass human extinction due to what AI might become have not actually happened. These visions may not ever happen. We can and sh...
Dr. David Bray 
Analyst Blog
August 12, 2026
### [New Release: 39 Constellation ShortList™ Updates for Q3 2026](https://www.constellationr.com/research/blog/new-release-39-constellation-shortlisttm-updates-q3-2026)
Data to Decisions
Constellation Research is pleased to announce the first wave of Q3 2026 Constellation ShortList™ updates, highlighting leading vendors across some of today's most important enterpr...
