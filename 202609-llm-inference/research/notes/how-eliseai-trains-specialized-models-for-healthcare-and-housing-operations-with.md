---
title: How EliseAI trains specialized models for healthcare and housing operations
  with Baseten Research
id: how-eliseai-trains-specialized-models-for-healthcare-and-housing-operations-with
tags:
- internal-llm-inference-stacks-7f0c90
- baseten-managed-middle
- baseten-case-study
- managed-middle-confirmed
created: '2026-10-10T10:58:28.618122Z'
updated: '2026-10-10T14:52:17.274811Z'
source: https://www.baseten.co/resources/customers/how-eliseai-trains-specialized-models/
source_domain: www.baseten.co
fetched_at: '2026-10-10T10:58:28.597831Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Baseten vendor case study: EliseAI (AI for housing/healthcare ops -- leasing,
  patient intake) fine-tunes a 4B-parameter Qwen model via supervised fine-tuning
  plus Baseten Research post-training techniques (On-Policy Self-Distillation, Iterative
  SFT) to replace closed-source APIs on its latency-critical voice agent. Reports
  p90 latency dropped from 2.2s (closed-source) to 250ms, ~60% inference cost reduction,
  99% accuracy on a key leasing feature, and ''frontier-matching accuracy'' at 1/25th
  the parameter count. Baseten also operates the serving side: TensorRT-LLM engines
  with tensor parallelism, chunked prefill, prefix caching, speculative decoding,
  multi-cloud/region autoscaling. This is the strongest fully-confirmed managed-middle
  case in the batch: BOTH parts of the test hold -- EliseAI customized the weights
  (SFT + OPSD/Iterative SFT on Qwen-4B) AND Baseten operates the production serving
  stack.'
---

Use your OpenAI commitments on Baseten open models. [Learn more](https://www.baseten.co/talk-to-us/openai/)
Case study
# How EliseAI trains specialized models for healthcare and housing operations with Baseten Research
## 60%
reduction in inference cost
## 80%
reduction in latency
## 250ms
p90 latency
##  **Domain experts tackling AI's hardest problems in housing and healthcare**
EliseAI is the leading AI startup automating complex housing and healthcare systems, which are two of the most operationally demanding domains in the economy. Its platform handles patient scheduling, intake, and front-desk operations for medical practices, and leasing, maintenance, and resident engagement for property managers. These use cases require deep domain expertise in regulated industries where accuracy, latency, and reliability directly impact patient care and resident experience.
**EliseAI processes millions of conversations every month across housing and healthcare.** Over 80 percent of the country’s largest property management firms use EliseAI’s agents to speed up repetitive admin work like tracking maintenance requests, scheduling apartment tours and renewing leases. These requests need to be handled in real time. This includes sub-second latency, high extraction accuracy, and the flexibility to adapt as the product changes weekly.
When their closed-source APIs hit a ceiling on cost, speed, and controllability, EliseAI set out to build their own custom models. They partnered with the Baseten research team to get there. The Baseten research team, with backgrounds from Oxford, Cambridge, and MATS, brought hands-on experience with advanced post-training techniques like [On-Policy Self-Distillation (OPSD) ](https://arxiv.org/abs/2601.18734)and [Iterative SFT](https://www.baseten.co/research/iterative-sft/) to fix specific failure modes. 
##  **The cost of renting intelligence**
As EliseAI has grown rapidly, the need to own their models and SLAs became clear due to latency and cost issues. Specific task latency sat at 2.2 seconds p90 with closed-source providers. These models sit on the critical path of real-time conversations between residents and property managers. For EliseAI users, 2.2 seconds versus 250 milliseconds is the difference between an AI that feels responsive and one that feels broken.
##  **What extraction actually looks like**
Here's what EliseAI's models do thousands of times per hour. A resident sends a series of messages:
**Resident** : Hi, I'm looking for a 2-bed near downtown. My move-in is flexible, sometime in March works.
**Agent** : We have a few options! Any preference on layout?
**Resident** : Something with a balcony would be great. Oh and my budget is around $2,200.
The model reads this conversation and outputs structured JSON:
`{`
`  "layout_preferences": "2-bedroom with balcony",`
`  "budget_max": 2200,`
`  "move_in_date": "2026-03",`
`  "unit_numbers": null,`
`  "floorplan_names": null`
`}`
On the surface, the task looks simple, but different fields start to interact with each other: does mentioning a unit number override previously extracted layout preferences? Temporal references are ambiguous: is "ASAP" a date or null? And the line between an expressed preference and a casual question is often unclear.
EliseAI runs dozens of these extraction tasks across housing and healthcare. They range from straightforward (parsing a tour time from a confirmation message) to complex and challenging (inferring a full preference snapshot from a multi-turn conversation with implicit signals).
General-purpose models required increasingly elaborate prompting to handle these cases. Even then, accuracy plateaued on the most complex conversations.
EliseAI needed a critical voice agent feature to respond quickly, as it was one of the most important capabilities in their leasing platform. **Closed-source models took on average around 2 seconds, but the voice pipeline had a hard latency ceiling of 1.3 seconds.** The team couldn't ship the feature without a much faster model. Fine-tuning a smaller open-source model to match and surpass accuracy under that latency ceiling had an immediate product impact.
##  **Training on Baseten**
EliseAI runs all fine-tuning work through [Baseten Training](https://docs.baseten.co/training/overview). The team kicks off jobs across tasks and configurations without managing GPU infrastructure and deploys the resulting models directly to Baseten's inference platform.
The default recipe is supervised fine-tuning on Qwen-4B. The 4B parameter size is a hard constraint from EliseAI's latency budget: models need to produce structured JSON fast enough to sit on the critical path of a real-time conversation. On their earliest production tasks, SFT on clean data matched the teacher model's accuracy at a fraction of the latency.
After onboarding, EliseAI's team was launching training jobs independently. The tight coupling between training and serving meant they could go from "training complete" to "deployed in production inference" without additional integration work.
> Nearly every time we found the fine-tuned model made a mistake in production, the root cause was the same: the ground truth data contained the error. Not the model, not the fine-tuning. The data.
##  **With Baseten, EliseAI achieved**
  * **99% accuracy** on one of the most important features in the leasing platform
  * **250ms p90 latency** , down from 2.2 seconds on closed-source APIs
  * **~60% reduction** in inference costs
  * **Frontier-matching accuracy** on a 4B parameter model, matching or exceeding models 25x its size on production benchmarks
  * **Independent training operations** with EliseAI's team launching jobs on their own after initial onboarding


##  **Research support**
For straightforward extraction tasks, SFT on clean data was enough. The fine-tuned model matched the teacher on weighted field accuracy while running at a fraction of the latency. But some tasks proved harder due to ambiguity and the degree of "correctness" required.
The Baseten post-training research team, with backgrounds from Oxford, Cambridge, and MATS, brought hands-on experience with advanced post-training techniques to push past these plateaus.
When standard SFT hit its ceiling, the team layered in [On-Policy Self-Distillation (OPSD)](https://arxiv.org/abs/2601.18734)—a technique that generates training signal from the student model's own outputs, correcting errors in the distribution it will actually encounter at serving time. OPSD outperformed pure SFT across every field EliseAI tested, with the largest gains on fields requiring implicit reasoning or multi-step extraction logic.
For targeted improvements, they employed [Iterative SFT](https://www.baseten.co/research/iterative-sft/) to fix specific failure modes. Because the student model's errors tend to be independent and decomposable (unlike the teacher's correlated failures on hard examples), this approach proved especially effective—each correction addressed a distinct, fixable pattern.
Our team recommended which techniques to try fist, helped diagnose where SFT was plateauing, and structured the training pipeline for EliseAI's extraction tasks specifically.
> The researchers we worked with at Baseten are among the smartest people you can find anywhere.
##  **Continuous improvement**
Fine-tuning isn't a one-time project. EliseAI's product requirements change weekly, and the AI space moves fast. Models need to be maintained and improved continuously to stay ahead of what general-purpose APIs can do out of the box.
Baseten Training supports this by making iteration cheap. EliseAI can test new training configurations, evaluate results against a stable test set, and deploy updates to production, all within the same platform. For a team shipping model updates weekly, that speed is what makes continuous improvement practical rather than aspirational.
> You can't treat fine-tuning as a one-and-done check item. This space moves so fast that you have to continuously keep up.
##  **Results**  
| Metric  | Before  | After (Fine-tuned Qwen 4B)  |  
| --- | --- | --- |  
| p90 latency  | 2.2 seconds  | 250 milliseconds  |  
| Inference cost  | Baseline  | ~60% reduction  |  
| Accuracy (weighted field)  | Closed-source ceiling  | Matches or exceeds frontier  |  
| Model size  | 100B+ parameter APIs  | 4B parameters  |  
> “Once you know your task deeply and have the expertise to design the right system around it, fine-tuned open-source models can offer major advantages. They often provide stronger control, lower latency, and better efficiency for specialized, latency-sensitive workflows. In many cases, a model like Qwen-4B can be a particularly compelling option.”
## Serving the voice agent in production 
The voice agent has to answer inside a live phone call, so each request has to clear a sub-second end-to-end budget, and a slow or dropped response is something the caller hears. The workload also has an awkward shape for an LLM: the prompts are long, because every turn carries the property's full context (unit data, availability, media links), while the spoken replies are short. Baseten tuned the inference stack around that shape. The model is served with TensorRT-LLM engines and tensor parallelism across GPUs; chunked prefill keeps those long prompts from stalling the queue; prefix caching reuses the repeated context across turns instead of recomputing it every time; and speculative decoding shortens the generation step. Baseten also co-locates the speech and language models in the same region so the audio pipeline isn't paying network round-trips between stages.
In production, the model handles the voice agent's live call volume and clears EliseAI's full agentic test suite, so it holds the conversation as well as the closed model it replaced. The other half of the story is uptime, which matters more for a phone product than a raw benchmark: third-party API outages and latency spikes had been dropping live calls, so Baseten runs the model across multiple clouds and regions with autoscaling tuned to EliseAI's call patterns, keeping enough replica headroom that a surge in traffic doesn't turn into dropped calls or p99 blowups. The upshot is that EliseAI's largest and most latency-sensitive workload now runs on an open-source model they own at the reliability a live voice product needs.
Powered by custom, fine-tuned models and[ Baseten's Training Product and research support](https://www.baseten.co/products/training/), EliseAI owns the intelligence behind its platform, and continues to push the boundaries of what AI automation can deliver for the hardest problems in housing and healthcare.
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
