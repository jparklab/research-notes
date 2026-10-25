---
title: The Build-vs-Buy LLM Infrastructure Decision Most Teams Get Wrong
id: the-build-vs-buy-llm-infrastructure-decision-most-teams-get-wrong
tags:
- internal-llm-inference-stacks-7f0c90
- build-vs-buy
- independent-adversarial-evidence
- break-even-threshold
- gpu-utilization
- concurrency-collapse
- model-update-treadmill
created: '2026-10-09T22:55:20.994335Z'
updated: '2026-10-10T14:52:17.078266Z'
source: https://tianpan.co/blog/2026-04-15-build-vs-buy-llm-infrastructure
source_domain: tianpan.co
fetched_at: '2026-10-09T22:55:20.969655Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Independent engineering blog post (tianpan.co, April 2026) arguing both
  ''stay on APIs too long'' and ''self-host too early'' are common failure modes,
  with staged break-even thresholds. Startup stage (<$10K/mo API spend): self-hosting
  has 34-month payback for a 70B model, APIs win decisively. Growth stage ($10K-$50K/mo):
  hybrid routing — bulk/low-complexity traffic to self-hosted 7B-13B models, frontier
  tasks stay on API; a single H100 running a 13B model at 50% GPU utilization covers
  ~8,000 conversations/day at $1,500-$5,000/month including staff. Scale stage ($50K+/mo,
  100M+ tokens/month): self-hosting bulk traffic is ''almost mandatory for margin''
  against GPT-5-Mini/Haiku-tier pricing (3-5x cheaper) but self-hosting LOSES against
  DeepSeek V3-tier pricing ($0.28/M input) for commodity tasks — the piece calls this
  ''the uncomfortable 2026 reality'' that price-competitive providers have reset the
  math. Cites a (unsourced, peer-reviewed claim) finding that hardware+electricity
  are only 20-30% of total self-hosting cost across 54 deployment scenarios, with
  staff the dominant 70-80%. Details CUDA/driver maintenance tax (1-3 engineering
  days per model upgrade, 8-20 hrs/month steady-state), notes HF''s TGI entered maintenance
  mode Dec 2025 (HF recommends vLLM/SGLang), and ranks serving frameworks by concurrency
  collapse: Ollama throughput collapses ~20x below vLLM at 10+ concurrent users (head-of-line
  blocking), llama.cpp ~44x below vLLM at high concurrency. Lists 14 external references
  (GPU pricing trackers, red hat framework comparisons, TCO blogs) as citation leads.'
utility_score: 16.0
---

[Skip to main content](https://tianpan.co/blog/2026/04/15/build-vs-buy-llm-infrastructure#main)
Ask AI
A FinTech team built their AI chatbot on GPT-4o. Month one: $15K. Month two: $35K. Month three: $60K. Projecting $700K annually, they panicked and decided to self-host. Six months and one burned-out engineer later, they were spending $85K/month on infrastructure, a part-time DevOps engineer, and three CUDA incidents that took down production. They eventually landed at $8K/month — but not by self-hosting everything. By routing intelligently.
Both decisions were wrong. The real failure was that they never ran the actual math.
The build-vs-buy decision for LLM infrastructure is reliably the most expensive mistake teams make, and they make it in both directions. Teams stay on hosted APIs too long and get blindsided by cost. Teams self-host too early and discover that "free" open-weight models come with a $6K/month hidden invoice in engineering time. The problem isn't making the wrong choice — it's making the choice based on incomplete numbers.
This post gives you the actual numbers, the framework for running the math at your specific scale, and the break-even points where the answer genuinely changes.
## The Hidden Costs of Hosted APIs: What Your Invoice Doesn't Show[​](https://tianpan.co/blog/2026/04/15/build-vs-buy-llm-infrastructure#the-hidden-costs-of-hosted-apis-what-your-invoice-doesnt-show "Direct link to The Hidden Costs of Hosted APIs: What Your Invoice Doesn't Show")
The per-token rate on a pricing page is the minimum you will pay, not the actual cost.
**Rate limits force over-engineering.** Most hosted providers enforce hard limits: token-per-minute caps, requests-per-minute caps, and context-window restrictions that don't match what's advertised. When a new Claude Sonnet account is effectively limited to 50 RPM and 20K input TPM, you can't just point traffic at it — you need a queuing layer, retry logic, backpressure handling, and exponential backoff. That's 2–4 weeks of engineering. Budget $15K–$30K in developer time for a production-quality request wrapper, and then ongoing maintenance.
**Egress and overhead add 5–15%.** Cloud egress fees ($0.05–$0.12/GB depending on provider) compound on high-throughput applications. JSON envelope overhead, streaming metadata, and verbose response formats inflate effective token costs beyond what the pricing calculator estimates. For applications making millions of daily calls, this matters.
**Committed throughput creates financial lock-in.** AWS Bedrock's provisioned throughput model is a representative example: Llama 3 70B costs roughly $13/hr on a 6-month commitment vs. $24/hr on-demand. The discount is real, but you're committing to a volume 6 months in advance for a technology landscape that changes quarterly. When DeepSeek halved prices in late 2025, teams on committed contracts didn't automatically benefit.
**Fine-tuning access is expensive or unavailable.** If your use case requires domain-specific tuning — medical vocabulary, legal document structure, proprietary code conventions — most hosted providers either don't offer it, charge significant premium rates on top of already-high base costs, or limit what data you can submit. This gap forces teams either to pay frontier prices for tasks a fine-tuned 7B model could handle, or to self-host specifically for fine-tuning while keeping inference hosted. Neither is ideal.
**The 340% cost overrun statistic.** A 2025 analysis of multi-tenant SaaS products found that teams routinely exceeded their LLM cost budgets by 340%. The root cause was almost never the per-token rate — it was lack of per-tenant usage tracking and no query-level cost attribution. Your invoice tells you what you spent; it doesn't tell you which customer, which feature, or which prompt template is burning 80% of the budget. Without instrumentation, you can't optimize.
## The Hidden Costs of Self-Hosting: What the Hardware Quote Doesn't Show[​](https://tianpan.co/blog/2026/04/15/build-vs-buy-llm-infrastructure#the-hidden-costs-of-self-hosting-what-the-hardware-quote-doesnt-show "Direct link to The Hidden Costs of Self-Hosting: What the Hardware Quote Doesn't Show")
The GPU rental price — or the purchase quote for owned hardware — is also the minimum you will pay.
**Staff is the dominant cost, not hardware.** A peer-reviewed analysis of 54 LLM deployment scenarios found that hardware costs and electricity together account for 20–30% of total self-hosting cost for most organizations. The 70–80% majority is people. Allocating 20–30% of a senior engineer's time to infrastructure maintenance costs $3,000–$6,000/month at market rates — before incidents, before model upgrades, before compliance work.
**CUDA driver maintenance is a recurring tax.** Self-hosting an LLM means owning the entire software stack: NVIDIA drivers, CUDA toolkit, cuDNN, the inference framework (vLLM, SGLang, llama.cpp), and the model weights. These components have their own release cycles, interdependencies, and breaking changes. Upgrading vLLM to support a new model's architecture requires verifying the new version doesn't break your existing CUDA environment. FlashAttention and xformers must be compiled against your specific CUDA version or you'll get silent performance degradation instead of an error. The practical cost: 1–3 engineering days per significant model version upgrade, plus an average of 8–20 hours/month in steady-state maintenance.
**Security is your problem, not the framework's.** None of the major inference frameworks — vLLM, llama.cpp, Ollama, or the now-maintenance-mode TGI — ship with authentication or authorization by default. Every self-hosted deployment needs a reverse proxy, API gateway, or service mesh to implement even basic access control. Model weights serialized in PyTorch pickle format carry arbitrary code execution risk; migrating to safetensors format requires explicit effort. SOC 2 or HIPAA certification for an AI feature adds $15K–$25K in audit overhead annually.
**Framework churn is real.** Hugging Face's Text Generation Inference entered maintenance mode in December 2025, with Hugging Face explicitly recommending teams migrate to vLLM or SGLang. Teams that built on TGI didn't choose to build on a deprecated framework — they chose the most popular option at the time. Expect this pattern to repeat.
**Model storage is not free.** A 7B parameter model in 16-bit precision requires 14GB of storage. A 70B model: 140GB. If you're maintaining multiple model checkpoints, doing A/B testing between model versions, or storing fine-tuning datasets, storage costs at $0.08–$0.12/GB/month become a line item worth tracking.
## The Break-Even Math by Stage[​](https://tianpan.co/blog/2026/04/15/build-vs-buy-llm-infrastructure#the-break-even-math-by-stage "Direct link to The Break-Even Math by Stage")
The actual calculation requires three inputs: your monthly API spend, your token volume by model tier, and an honest estimate of your available MLOps capacity.
**Startup stage ( < $10K/month API spend)**
Self-hosting does not make economic sense here. The hardware minimum for production-grade inference of a 70B model — 4–8 H100s on cloud rental — runs $15K–$40K/month before staff. The break-even period for a 70B model deployment at this scale is 34 months under optimistic assumptions. APIs win decisively, and that's the correct answer even though it feels like paying rent rather than building equity.
**Growth stage ($10K–$50K/month API spend)**
This is where hybrid routing starts paying off. The strategy: keep frontier-model calls (complex reasoning, creative generation, low-volume high-value tasks) on hosted APIs, route bulk-volume tasks (classification, summarization, extraction at scale) to self-hosted 7B–13B models. A single H100 running a 13B model at 50% GPU utilization covers roughly 8,000 conversations/day and costs $1,500–$5,000/month including staff overhead. For teams in this range, selective self-hosting of bulk traffic typically reduces total AI spend by 40–70%.
The FinTech case from the introduction wasn't wrong to explore self-hosting — it was wrong to self-host everything. Their $47K/month dropped to $8K by routing 600K daily prompts (the bulk classification and extraction work) to self-hosted infrastructure while keeping the 5K daily complex queries on hosted APIs.
**Scale stage ($50K+/month API spend, 100M+ tokens/month)**
At this volume, self-hosting for bulk traffic is almost mandatory for margin — but the math is more nuanced than it looks, because the frontier model price war has reset the break-even point.
At 100M tokens/month, comparing self-hosted (cloud GPU) vs. hosted API:
  * Against GPT-5 Mini / Claude Haiku tier: self-hosted wins by 3–5x
  * Against DeepSeek V3 pricing ($0.28/M input): self-hosted _loses_ for commodity tasks — the model provider's scale economics now beat yours


This is the uncomfortable 2026 reality. DeepSeek and other price-competitive providers have made the economics of self-hosting commodity inference much harder to justify on pure cost grounds alone. What still forces self-hosting at scale: compliance requirements that prohibit data leaving your infrastructure, sub-100ms latency requirements that network RTT makes impossible via API, and proprietary fine-tuned models that can't run on third-party infrastructure.
## The Decision Framework[​](https://tianpan.co/blog/2026/04/15/build-vs-buy-llm-infrastructure#the-decision-framework "Direct link to The Decision Framework")
Before running the math, answer these questions:
**What's the compliance requirement?** If you're in healthcare, financial services, or any regulated vertical requiring data residency — the decision is not primarily about cost. You may be required to self-host specific data types regardless of the break-even calculation.
**What's your actual monthly token volume?** Not projected, not theoretical. Run a week of production logs. The number that surprises most teams is how concentrated token usage is: 80% of tokens come from 20% of query types. Those high-volume, low-complexity queries are the self-hosting candidates.
**What MLOps capacity do you have?** "We'll hire someone" is not capacity. Honest assessment: do you have a person currently on staff who has operated inference infrastructure in production? If not, factor in 3–6 months of ramp time and 30–40% productivity degradation during that period.
**What's the latency requirement?** API inference adds 30–100ms of network RTT plus queuing latency. For interactive features with p95 targets under 200ms, this matters. For async batch processing, it doesn't.
The framework simplifies to: APIs for everything under $10K/month and for frontier-model tasks at any volume; hybrid routing between $10K–$50K/month for bulk tasks; self-hosted bulk + API frontier-model above $50K/month, with compliance requirements overriding the cost calculus entirely.
## Choosing Your Self-Hosting Stack[​](https://tianpan.co/blog/2026/04/15/build-vs-buy-llm-infrastructure#choosing-your-self-hosting-stack "Direct link to Choosing Your Self-Hosting Stack")
If the math says self-hosting makes sense, framework selection matters more than teams realize. The inference framework determines your day-two operational overhead more than the raw model choice.
**vLLM** is the production default for multi-user serving. It delivers 35x+ the request throughput of llama.cpp at 10+ concurrent users, supports FP8 and FlashAttention, and has an OpenAI-compatible API that reduces migration effort. It requires a CUDA environment setup and container management — budget 1–2 weeks for initial production hardening.
**Ollama** is for development and single-user prototyping only. At 10+ concurrent users, it exhibits head-of-line blocking and throughput collapses to 20x below vLLM. Do not use it for production multi-user workloads.
**llama.cpp** handles edge deployment and environments where GPUs are unavailable. At high concurrency, throughput is 44x below vLLM. For CPU-only inference or single-user production deployments, it's valid. For multi-tenant serving, it's not.
**TGI (Text Generation Inference)** entered maintenance mode in December 2025. Don't start new projects on it.
## The Takeaway[​](https://tianpan.co/blog/2026/04/15/build-vs-buy-llm-infrastructure#the-takeaway "Direct link to The Takeaway")
The build-vs-buy decision isn't about which option is "better" — it's about matching your actual cost structure to your actual operational capacity at your actual scale. Both sides of the equation have hidden costs that standard analyses miss: hosted APIs undercharge on the invoice and overcharge in engineering time; self-hosting looks cheaper per-token until you add the engineer who's going to be woken up at 2am when the CUDA driver update breaks the production inference server.
The teams that get this right don't pick a side. They build a routing layer early, instrument every query with token counts and latency, and let the numbers tell them which traffic should move. The teams that get it wrong pick a side based on a pricing page and a GitHub star count.
Run the math. Then run it again six months later, because the market will have changed.
## References
  * <https://intuitionlabs.ai/articles/h100-rental-prices-cloud-comparison>
  * <https://arxiv.org/html/2509.18101v1>
  * <https://www.ptolemay.com/post/llm-total-cost-of-ownership>
  * <https://tokenmix.ai/blog/self-host-llm-vs-api>
  * <https://introl.com/blog/inference-unit-economics-true-cost-per-million-tokens-guide>
  * <https://www.sitepoint.com/self-hosted-llm-costs-2026/>
  * <https://www.sitepoint.com/local-llms-vs-cloud-api-cost-analysis-2026/>
  * <https://developers.redhat.com/articles/2025/08/08/ollama-vs-vllm-deep-dive-performance-benchmarking>
  * <https://developers.redhat.com/articles/2025/09/30/vllm-or-llamacpp-choosing-right-llm-inference-engine-your-use-case>
  * <https://www.freeportmetrics.com/blog/the-2025-self-hosting-field-guide-to-open-llms>
  * <https://deepsense.ai/blog/llm-inference-as-a-service-vs-self-hosted-which-is-right-for-your-business/>
  * <https://www.aimprosoft.com/blog/cost-to-host-private-llm-2025/>
  * <https://ashah007.medium.com/navigating-the-llm-cost-maze-a-q2-2025-pricing-and-limits-analysis-80e9c832ef39>
  * <https://www.gmicloud.ai/en/blog/2025-cost-of-renting-or-uying-nvidia-h100-gpus-for-data-centers>


Let's stay in touch and follow me for more
## Related articles
[ 9 min read Tokenizer Drift: Your Local Counter Lies, the Bill Tells the Truth Local tokenizers and provider billing counters disagree by 5–15% on the long-tail content your CI never tests. The gap eats your safety margin where your users… llm tokenization ](https://tianpan.co/blog/2026/04/28/tokenizer-drift-local-count-vs-billing)[ 8 min read Open-Weight Models in Production: When Self-Hosting Actually Beats the API A practical decision framework for self-hosting open-weight models like Llama, Mistral, and Qwen versus using frontier APIs — covering real cost breakdowns,… llm self-hosting ](https://tianpan.co/blog/2026/04/13/open-weight-models-production-when-llama-beats-api)[ 10 min read Scheduling Fairness in Multi-Tenant LLM Inference: Why FIFO Is the Wrong Default When multiple teams share LLM inference infrastructure, naive FIFO scheduling causes priority inversion and SLO violations. Here's what fair scheduling… insider llm ](https://tianpan.co/blog/2026/05/07/scheduling-fairness-multi-tenant-llm-workloads)[ 10 min read SSE vs WebSockets vs gRPC Streaming for LLM Apps: The Protocol Decision That Bites You Later How SSE, WebSockets, and gRPC streaming fail differently under backpressure, what browser constraints and edge proxies break in production, and the… llm streaming ](https://tianpan.co/blog/2026/04/19/sse-websockets-grpc-streaming-llm-applications)[ 11 min read The Noisy Neighbor Problem in Shared LLM Infrastructure: Tenancy Models for AI Features When one feature's batch job eats the shared API quota, paying users see 429s. Detection signals and isolation patterns for shared LLM infrastructure. insider llm ](https://tianpan.co/blog/2026/04/16/noisy-neighbor-llm-shared-infrastructure-tenancy)
