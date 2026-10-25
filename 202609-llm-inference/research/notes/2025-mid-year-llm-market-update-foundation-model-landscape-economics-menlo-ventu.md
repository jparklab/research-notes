---
title: '2025 Mid-Year LLM Market Update: Foundation Model Landscape + Economics |
  Menlo Ventures'
id: 2025-mid-year-llm-market-update-foundation-model-landscape-economics-menlo-ventu
tags:
- llm-inference
- model-layer
- internal-llm-inference-stacks-7f0c90
- open-weight-share-decline
- enterprise-survey
- frontier-vs-open-weight
created: '2026-10-09T22:55:42.269922Z'
updated: '2026-10-10T14:52:17.089456Z'
source: https://menlovc.com/perspective/2025-mid-year-llm-market-update/
source_domain: menlovc.com
fetched_at: '2026-10-09T22:55:42.247554Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Menlo Ventures survey of 150 technical decision-makers (enterprises >=5,000
  employees and VC-funded startups), fielded June 30-July 10 2025, measuring self-reported
  SHARE OF PRODUCTION AI WORKLOADS (not spend) by model/provider, weighted by application
  scale. Finds Anthropic leads enterprise usage at 32% (overtaking OpenAI''s 25%,
  down from 50% in 2023), Google at 20%, Meta Llama at 9%, DeepSeek at 1%. Open-source/open-weight
  model usage FELL from 19% to 13% of workloads over six months, attributed to a persistent
  9-12 month performance gap versus closed frontier models, deployment complexity,
  and enterprise reluctance to use APIs from Chinese labs. Enterprise LLM API spend
  more than doubled from $3.5B (Nov 2024) to $8.4B (mid-2025). Only 11% of teams switched
  vendors in the past year; 66% upgraded within their existing provider. Inference
  now exceeds training spend: 74% of startups and 49% of enterprises report inference
  as majority of compute, up from 48%/29% a year prior.'
utility_score: 16.0
---

[ Skip to Main Content ](https://menlovc.com/perspective/2025-mid-year-llm-market-update/#main-content "Skip to Main Content")
MENLO
Established 1976 50 years of going **ALL IN**
[ All Perspectives ](https://menlovc.com/perspective/ "All Perspectives")
PerspectivePortfolioReport 
# 2025 Mid-Year LLM Market Update: Foundation Model Landscape + Economics
A new enterprise LLM leader has emerged as usage and spend surge.
July 31, 2025
[Tim Tully](https://menlovc.com/team/tim-tully/), [Joff Redfern](https://menlovc.com/team/joff-redfern/), [Deedy Das](https://menlovc.com/team/deedy-das/), [Derek Xiao](https://menlovc.com/team/derek-xiao/)
[ Facebook ](https://www.facebook.com/sharer/sharer.php?u=http://https%3A%2F%2Fmenlovc.com%2Fperspective%2F2025-mid-year-llm-market-update%2F&quote=2025%20Mid-Year%20LLM%20Market%20Update:%20Foundation%20Model%20Landscape%20+%20Economics "Facebook") [ Linkedin ](http://www.linkedin.com/shareArticle?mini=true&url=http://https%3A%2F%2Fmenlovc.com%2Fperspective%2F2025-mid-year-llm-market-update%2F&title=2025%20Mid-Year%20LLM%20Market%20Update:%20Foundation%20Model%20Landscape%20+%20Economics "Linkedin") [ Twitter ](https://twitter.com/intent/tweet?text=2025%20Mid-Year%20LLM%20Market%20Update:%20Foundation%20Model%20Landscape%20+%20Economics&url=http://https%3A%2F%2Fmenlovc.com%2Fperspective%2F2025-mid-year-llm-market-update%2F "Twitter") Envelope
Copy link
Copied to clipboard!
Table of Contents 
[Anthropic Surpasses OpenAI in Enterprise Usage](https://menlovc.com/perspective/2025-mid-year-llm-market-update/#h-anthropic-surpasses-openai-in-enterprise-usage)
[Open-Source Adoption in the Enterprise Flattens](https://menlovc.com/perspective/2025-mid-year-llm-market-update/#h-open-source-adoption-in-the-enterprise-flattens)
[Enterprises Switch Models for Performance, Not Price](https://menlovc.com/perspective/2025-mid-year-llm-market-update/#h-enterprises-switch-models-for-performance-not-price)
[AI Spend Is Moving from Training to Inference](https://menlovc.com/perspective/2025-mid-year-llm-market-update/#h-ai-spend-is-moving-from-training-to-inference)
[Where We Go from Here](https://menlovc.com/perspective/2025-mid-year-llm-market-update/#h-where-we-go-from-here)
## Key Findings
  * Enterprise LLM spend more than doubled in just six months, rising from **$3.5B** in November 2024 to**$8.4B** by mid-2025 as workloads moved into full production.
  * Anthropic is now the leading enterprise LLM provider with **32%** market share. OpenAI holds**25%** , down from **50%** in 2023. Google has taken third place, claiming **20%** of enterprise usage with strong adoption of its Gemini models.
  * Only **11%** of teams report changing model providers in the past year. But**66%** upgraded to newer models from their existing vendor, while**23%** made no changes or upgrades at all.
  * Closed-source models now dominate, powering **87%** of enterprise workloads. Open-source usage fell from**19%** to **13%** over the past six months as performance gaps widened.
  * Inference has overtaken training as the primary workload, with**74%** of startups and **49%** of enterprises reporting that inference accounts for most of their compute usage. Both figures are up significantly from November of last year.


**Foundation models are not just powering generative AI. They are shaping the future of computing.** As their capabilities and economics evolve, so will the systems, applications, and industries built on top of them.
When we released Menlo Ventures’ [2024: The State of Generative AI in the Enterprise](https://menlovc.com/2024-the-state-of-generative-ai-in-the-enterprise/) report last November, several critical questions about this foundational layer remained unanswered:
  * Would demand for LLM APIs keep pace with the growth of consumer applications?
  * How smart will these models get, and how fast will they get there? 
  * Would open-source models catch up to closed-source frontier models in performance, and if so, how would that impact enterprise adoption?
  * And most importantly, where might long-term value accrue?


Six months later, the data tells a clearer story:
Model API spending has more than doubled in this brief period—jumping from **$3.5 billion**(of a total **$13.8 billion** generative AI spend we estimated last year) to **$8.4 billion**. Enterprises are increasing production inference rather than just model development, marking a shift from previous years.
Code generation has become AI’s first breakout use case. Beyond pre-training, foundation models are now scaling along a second axis: reinforcement learning with verifiers. And while open-source continues to advance, the slowdown in frontier breakthroughs from Western labs has tempered what had previously been a rise in enterprise adoption. As a result, enterprise dollars are now consolidating around a few high-performing, closed-source models, giving us a new market leader in [Anthropic](https://www.anthropic.com/)*.
To capture the state of the current LLM market, we surveyed over 150 technical leaders across startups and enterprises on the modern AI stack’s foundation layer: who’s gaining share, what’s running in production, and the selection criteria shaping the entire stack.
Here is what we learned:
### **Anthropic Surpasses OpenAI in Enterprise Usage**
By the end of 2023, [OpenAI](https://openai.com/)commanded **50%** of the enterprise LLM market, but its early lead has eroded. Today, it captures just **25%** of enterprise usage—half of what it held two years ago.
Anthropic is the new top player in enterprise AI markets with **32%** , ahead of OpenAI and [Google](https://gemini.google.com/) (**20%**), which has shown strong growth in recent months. [Meta](https://www.meta.com/)’s [Llama](https://www.llama.com/) holds **9** %, while [DeepSeek](https://www.deepseek.com/),despite its high-profile launchat the beginning of the year, accounts for just **1%**.
The momentum that carried Anthropic to the top of the LLM leaderboard began in earnest with the release of Claude Sonnet 3.5 in June 2024. Momentum accelerated with Claude Sonnet 3.7 in February 2025, which introduced the first real glimpse of an agent-first LLM. By May 2025, [Claude Sonnet 4](https://www.anthropic.com/claude/sonnet), [Opus 4](https://www.anthropic.com/claude/opus), and [Claude Code](https://www.anthropic.com/claude-code) cemented Anthropic’s lead.
Three industry-defining trends fueled Anthropic’s momentum:
  1. **Code generation became AI’s first killer app**.Claude quickly became the developer’s top choice for code generation, capturing**42%** market share, more than double OpenAI’s (**21%**). In just one year, Claude helped transform a single-product space ([GitHub Copilot](https://github.com/copilot)) into a**$1.9 billion** ecosystem. The release of Claude Sonnet 3.5 in June 2024 demonstrated how breakthroughs at the model layer can move application markets, making possible entirely new categories like AI IDEs ([Cursor](https://cursor.com/)*, [Windsurf](https://windsurf.com/)), app builders ([Lovable](https://lovable.dev/)*, [Bolt](https://bolt.new/), [Replit](https://replit.com/)), and enterprise coding agents (Claude Code, [OpenHands](https://www.openhands.dev/)*).
  2. **Reinforcement learning with verifiers is the new path to scaling intelligence**.In 2024, the primary way to scale intelligence was by pre-training larger and larger models with more and more data. The scale of internet data is now becoming a rate limiter. Post-training using reinforcement learning with verifiable rewards (RLVR) was the next unlock to push the envelope. This strategy works particularly well in fields like coding, which is easier to deterministically verify.
  3. **Training models as “agents” to use tools makes them far more useful**.LLMs were initially designed to provide complete answers in a single response. However, enabling them to think step-by-step, reason through problems, and use external tools across multiple interactions—creating what’s known as an agent—makes them dramatically more effective for real-world applications. 2025 has become known as the “year of agents.” Anthropic led the way in training models to iteratively improve their responses and integrate tools like search, calculators, coding environments, and other resources through MCP ([model context protocol](https://www.anthropic.com/news/model-context-protocol)), significantly boosting both their capabilities and user adoption.


### **Open-Source Adoption in the Enterprise Flattens**
**Thirteen percent** of AI workloads today use open-source models, down slightly from**19%** six months ago. The market leader remains the popular Llama model by Meta, though the Llama 4 launch in April underwhelmed in real-world settings.
The landscape has stayed active, with notable launches from DeepSeek (V3, R1), [Bytedance Seed](https://seed.bytedance.com/en/)(Doubao), [Minimax](https://www.minimaxi.com/)(Text 1), [Alibaba](https://www.alibabacloud.com/en/solutions/generative-ai/qwen?_p_lc=1)(Qwen 3), [Moonshot AI](https://www.moonshot-ai.com/)(Kimi K2), and [Z AI](https://z.ai/) (GLM 4.5) in the last six months. You can try all of these in one API on [OpenRouter](https://openrouter.ai/)*.
Open-source models offer clear enterprise advantages: greater customization, potential cost savings, and the ability to deploy within private cloud or on-premises environments. But despite these benefits and recent improvements, open-source has continued to trail frontier, closed-source models in performance by nine to 12 months.
This performance gap, along with the technical complexity of deploying open-source models and enterprise reluctance to use APIs from Chinese companies—which have produced many of the more recent top-performing open-source models—has led to a stagnating market share.
It’s not just enterprises. Fewer startups adopt open-source models for these reasons, too. As one respondent put it:
> Currently, 100% of our production workloads are running on closed-source models. We initially started with Llama and DeepSeek for POCs, but they couldn’t keep up with the performance of closed-source over time.”
### **Enterprises Switch Models for Performance, Not Price**
**Switching between vendors is relatively easy, but increasingly rare.** Most teams remain with their provider and simply upgrade to the newest model as it becomes available. Once builders commit to a platform, they tend to stay, but move quickly to upgrade to newer, higher-performing models when they’re released.
According to our survey: **66%** of builders upgraded models within their existing provider, while **23%** did not switch models at all this past year. Only **11%** switched vendors.
**Performance drives decisions.** Builders consistently choose frontier models over cheaper, faster alternatives. They prioritize and pay for performance. When new models come out, switching happens in weeks. Within one month of Claude 4’s release, for instance, Claude 4 Sonnet captured **45%** of Anthropic users while Sonnet 3.5 share decreased from **83%** to **16%**.
This creates an unexpected market dynamic: Even as individual models drop **10x** in price, builders don’t capture savings by using older models; they just move en masse to the best performing one.
### **AI Spend Is Moving from Training to Inference**
Spending on compute is steadily shifting from building and training models to inference, with models actually running in production. This shift is most pronounced among startups: **74%** of builders now say the majority of their workloads are inference, up from **48%** a year ago. Large enterprises are not far behind. Nearly half (**49%**) report that most or nearly all of their compute is inference-driven—up from **29%** last year.
### **Where We Go from Here**
Predicting the future of AI can be a fool’s errand. The market changes by the week, with exciting new model launches, advancements in foundation model capabilities, and plunging costs. What has become clear, though, is that conditions are ripe for a new generation of enduring AI businesses to be built on top of today’s foundational building blocks. 
At Menlo Ventures, we’ve been partnering with founders building at the AI infrastructure layer for years, including [Anthropic](https://menlovc.com/portfolio/anthropic/),[ Cleanlab](https://menlovc.com/portfolio/cleanlab/), [Goodfire](https://www.goodfire.ai), [Mercor](https://menlovc.com/portfolio/mercor/), [OpenRouter](https://menlovc.com/portfolio/openrouter/), [Pinecone](https://menlovc.com/portfolio/pinecone/), and [Unstructured](https://menlovc.com/portfolio/unstructured/). If you’re creating infrastructure, tooling, and applications for the AI era, we’d love to hear from you.
[**Download a PDF of the report**](https://menlovc.com/wp-content/uploads/2025/11/menlo_ventures_mid-year_llm_report-2025.pdf)
  1. Our LLM market sizing excludes frontier AI lab revenue from consumer-facing products such as ChatGPT, or enterprise applications like Claude for Work and Claude Code. In our November 2024 report, we estimated the size of this market to be $3.5 billion of a total $13.8 billion spent on generative AI across foundation models, model training, AI infrastructure, and applications. [↩︎](https://menlovc.com/perspective/2025-mid-year-llm-market-update/#231d94dd-4d90-47a1-a125-c62daefe5dc1-link)
  2. This report summarizes data from a survey of 150 technical decision-makers at enterprises and startups building AI applications, conducted from June 30 to July 10, 2025. Enterprises are defined as organizations with 5,000 or more employees. Startups included in the sample have raised at least $5 million in venture funding. Across this foundational data, we overlaid our perspective and insights as active investors in the space. [↩︎](https://menlovc.com/perspective/2025-mid-year-llm-market-update/#864269b4-ecde-4afe-8163-9e314ea670fe-link)
  3. LLM market share reflects the proportion of production AI usage, not spend. Survey respondents reported the share of their AI workloads using each model. Responses were weighted based on each enterprise and startup application’s scale. [↩︎](https://menlovc.com/perspective/2025-mid-year-llm-market-update/#3e74707d-51f4-4d3f-bd3d-3047775b5784-link)
  4. Source: Menlo Ventures’ [2024: The State of Generative AI in the Enterprise](https://menlovc.com/2024-the-state-of-generative-ai-in-the-enterprise/), November 2024 [↩︎](https://menlovc.com/perspective/2025-mid-year-llm-market-update/#18aaeef7-0c05-404c-b36f-01edbc154d0f-link)


*Menlo Ventures portfolio company
Tim is a partner at Menlo Ventures focused on early-stage investments that speak to his passion for AI/ML, the new data stack, and the next-generation cloud. His background as a technology builder, buyer, and seller informs his investments in companies like Neon (acquired by Databricks), Eve, Fleet, and Gimlet, among…
####  [ Joff Redfern ](https://menlovc.com/team/joff-redfern/)
A self described “tall, slightly nerdy product guy,” Joff was previously the Chief Product Officer of Atlassian, responsible for leading its acclaimed portfolio of products, including Jira, Confluence, and Trello. During that time, Joff was named on the “Global CPO 20” list by Products That Count and as a “Top…
Deedy is a partner at Menlo Ventures focused on early-stage investments in AI/ML, next-generation infrastructure, and enterprise software. Having been an engineer and product leader at both a successful startup and large public companies, Deedy is well-equipped to help technical founders navigate how to build and scale enduring tech companies.…
As a principal at Menlo Ventures, Derek focuses on early-stage investments across AI, cloud infrastructure, and digital health. He partners with companies from seed through inflection, including Anthropic, Eve, Neon, and Unstructured. Derek joined Menlo from Bain & Company, where he advised technology investors on opportunities ranging from machine learning…
## Related Perspectives
PortfolioPerspectiveReport 
#### [2026: The State of Consumer AI](https://menlovc.com/perspective/2026-the-state-of-consumer-ai/)
Portfolio 
#### [Stripe to Acquire OpenRouter: Why Everyone Is Obsessed With Model Routing](https://menlovc.com/perspective/stripe-to-acquire-openrouter-why-everyone-is-obsessed-with-model-routing/)
FirmPortfolio 
#### [Menlo’s 2026 AI Cyber Summit: The Window for Defense Is Closing Fast](https://menlovc.com/perspective/menlos-2026-ai-cyber-summit-the-window-for-defense-is-closing-fast/)
PerspectivePortfolioReport 
#### [2025: The State of Generative AI in the Enterprise](https://menlovc.com/perspective/2025-the-state-of-generative-ai-in-the-enterprise/)
PerspectiveReportPortfolio 
#### [2025: The State of AI in Healthcare](https://menlovc.com/perspective/2025-the-state-of-ai-in-healthcare/)
PortfolioReportPerspective 
#### [2025: The State of Consumer AI](https://menlovc.com/perspective/2025-the-state-of-consumer-ai/)
[ View all ](https://menlovc.com/perspective/ "View all")
[ View all ](https://menlovc.com/perspective/ "View all")
Move to previous carousel slide Move to next carousel slide
By using this website, you agree to our use of cookies. We use cookies to provide you with a great experience and to help our website run effectively.AcceptDecline
