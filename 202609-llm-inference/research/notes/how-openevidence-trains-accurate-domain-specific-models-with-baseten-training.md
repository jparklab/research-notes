---
title: How OpenEvidence trains accurate, domain-specific models with Baseten Training
id: how-openevidence-trains-accurate-domain-specific-models-with-baseten-training
tags:
- internal-llm-inference-stacks-7f0c90
- baseten-managed-middle
- baseten-case-study
- managed-middle-confirmed
created: '2026-10-10T10:58:22.072566Z'
updated: '2026-10-10T14:52:17.273509Z'
source: https://www.baseten.co/resources/customers/openevidence-baseten-training/
source_domain: www.baseten.co
fetched_at: '2026-10-10T10:58:22.049792Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Baseten vendor case study (training side): OpenEvidence (medical search
  engine used by 40%+ of US physicians) trains custom, domain-specific LLMs on Baseten
  Training rather than using general-purpose closed models, training exclusively on
  peer-reviewed literature (NEJM/JAMA/NCCN licensed content). Baseten Training gives
  parallel hyperparameter-sweep job execution and ''one-click deploy to production
  inference''; OpenEvidence reports training 23x faster and projects $1.9M saved,
  with non-ML engineers able to train in under 30 minutes. Confirms part 1 of the
  managed-middle test (genuine custom/fine-tuned weights) explicitly; this page documents
  training, not serving -- the companion inference case study (not fetched, titled
  ''OpenEvidence delivers instant medical information with Baseten'') is the one that
  documents Baseten operating the serving side.'
---

Use your OpenAI commitments on Baseten open models. [Learn more](https://www.baseten.co/talk-to-us/openai/)
Case study
# How OpenEvidence trains accurate, domain-specific models with Baseten Training
## 30
minutes to get started
## $1.9M
saved via model training
## 23x
latency improvement
## Background: Domain-specialized LLMs 
OpenEvidence is the most widely used medical search engine in the U.S., trusted by over 40% of physicians across 10,000+ hospitals and medical centers. OpenEvidence's approach centers on training domain-specialized Large Language Models (LLMs) rather than relying on general-purpose AI.
Key differentiators in their model strategy include:
  * **Trusted data sources only:** Models are trained exclusively on peer-reviewed medical literature rather than the open internet or social media to ensure accuracy and avoid low-quality medical information.
  * **Research journal partnerships:** OpenEvidence has signed exclusive content agreements with the New England Journal of Medicine (NEJM), the JAMA Network specialty journals, and the National Comprehensive Cancer Network (NCCN) Guidelines.
  * **Medical benchmark leadership:** OpenEvidence created the first AI to score above 90% on the United States Medical Licensing Examination (USMLE), which later became [the first AI](https://www.openevidence.com/announcements/openevidence-creates-the-first-ai-in-history-to-score-a-perfect-100percent-on-the-united-states-medical-licensing-examination-usmle) in history to achieve a perfect 100% score.


OpenEvidence owns their AI stack through custom, domain-specific models that present the right information at the right time to doctors. While closed-source models offer strong general capabilities, they impose constraints like non-secure data, high cost per token, reliability and latency issues, and the inability to optimize for domain-specific performance. For a medical AI company embedding intelligence throughout their product, these limitations compounded quickly.
For this effort, OpenEvidence's machine learning team needed infrastructure that could support aggressive training experimentation, especially for highly specialized medical use cases where new information is published daily and rapid iteration is critical.
## Training challenges 
When OpenEvidence began their transition to domain-specific models, they faced the fundamental challenge confronting every team pursuing open-source alternatives: closing the quality gap with closed-source models while keeping up with the pace of new model releases and medical information.
The margin between a viable and non-viable open-source strategy often comes down to how many hyperparameter combinations a team can test. More experiments mean better configurations, which translate directly to model quality. [Baseten Training's](https://www.baseten.co/products/training/) parallel execution capabilities enabled the OpenEvidence team to bridge this gap.
### The infrastructure bottleneck 
Rapid new model releases led to the OpenEvidence team needing to train and optimize multiple custom models concurrently. Previously, the team ran into problems around sequential job execution and waiting for compute. If each training run took 2 hours and they needed to test 10 different configurations, they waited 20 hours for results. This iteration speed made it difficult for OpenEvidence researchers to match closed-source quality with custom open-source models.
> "Our engineers are great at training models. What they’re really bad at is fishing for GPUs. I've received so many texts asking for GPUs and I have to let them know there's none available in the queue."
## Solutions: Owning their intelligence 
To address their velocity needs, OpenEvidence trained their custom models on [Baseten Training](https://www.baseten.co/products/training/), utilizing key features such as code customizability, on-demand compute, and one-click deploys to production inference.
Baseten Training enabled parallel job execution, where researchers could launch dozens of training jobs simultaneously. Each job tested different hyperparameter settings (learning rates, batch sizes, training schedules, datasets, etc.) without waiting for others to complete.
> “No other product lets you launch ten different training jobs on four different datasets. You can sweep anything that you care about to get signal on what’s important. And you only have to pay for compute when you use it, which makes this a scalable process for us. It’s made training so much easier. It would have basically been impossible to do before.”
### Flexible code-first product 
OpenEvidence machine learning engineers (MLEs) were able to apply the newest training techniques to the most powerful open-source models. Since MLEs were able to customize their training code, they were able to maintain flexibility and control without being constrained by point-and-click interfaces.
> "The PhDs on my team are really good at writing training scripts, and in fact, they're very picky about writing their training scripts. They only want to use the code that they wrote, and they're the best in the world at doing it."
### On-demand compute 
Baseten Training handles complex orchestration of GPU resources across multiple concurrent jobs. ML engineers only need to focus on training the model rather than managing infrastructure and acquiring compute.
> "The problem was not 'Can we write this fine-tuning script?' It was 'Where is the hardware?' There's literally no GPU to run this on."
## Results 
Baseten Training enabled engineers on the team with no prior ML background to train powerful models in under 30 minutes using Baseten’s sample [scripts](https://github.com/basetenlabs/ml-cookbook/tree/main), achieving performance nearly identical to expert-built versions that would have taken weeks otherwise. The team found the product “so easy” to use, eliminating the need for manual VM setup or complex GPU management.
> “Baseten helped us train models to be 23x faster and is projected to save us $1.9M—while making the process so easy that even non-ML engineers could get results in under 30 minutes.”
Powered by custom, domain-specific models and [Baseten's Training product](https://www.baseten.co/products/training/), OpenEvidence continues to push the boundaries of what's possible in medical AI, with infrastructure that enables them to experiment rapidly and bring the most accurate information to physicians. Learn more about OpenEvidence's inference story [here](https://www.baseten.co/resources/customers/openevidence-delivers-instant-medical-information-with-baseten/).
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
