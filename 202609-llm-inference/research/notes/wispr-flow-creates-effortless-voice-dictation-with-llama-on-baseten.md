---
title: Wispr Flow creates effortless voice dictation with Llama on Baseten
id: wispr-flow-creates-effortless-voice-dictation-with-llama-on-baseten
tags:
- internal-llm-inference-stacks-7f0c90
- locus-managed-middle-hosted-open-weight
- baseten
- llama
- fine-tuning
created: '2026-10-10T00:03:12.274006Z'
updated: '2026-10-10T14:52:17.262536Z'
source: https://www.baseten.co/resources/customers/wispr-flow/
source_domain: www.baseten.co
fetched_at: '2026-10-10T00:03:12.254627Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Baseten''s own case study on Wispr Flow (voice dictation app ''Flow''),
  a NEW named organization fitting the managed-middle pattern: Wispr Flow fine-tunes
  Meta''s open-weight Llama models for real-time transcript cleanup/style-matching,
  then runs that fine-tuned model in PRODUCTION entirely on Baseten''s managed dedicated
  deployments (on AWS workload planes), not self-hosted. Baseten supplies the serving
  stack: TensorRT-LLM engine builder, the Chains multi-step inference framework, autoscaling
  GPUs, and SOC 2 Type II / HIPAA-compliant regional infrastructure. Quantified results:
  full pipeline (speech recognition + Llama-based enhancement) runs end-to-end in
  under 700ms at p99, with the Llama step generating 100+ tokens in under 250ms. CTO
  Sahaj Garg is quoted explicitly choosing fine-tuning + managed dedicated hosting
  over building their own serving infra (''we no longer have to reserve GPUs just
  in case'').'
---

Use your OpenAI commitments on Baseten open models. [Learn more](https://www.baseten.co/talk-to-us/openai/)
Case study
# Wispr Flow creates effortless voice dictation with Llama on Baseten
## <700
milliseconds end-to-end
## P99
latency measured
## 100+
tokens in <250 ms
## Company overview 
Based in San Francisco, [**Wispr Flow**](https://wisprflow.ai/) is a voice interface company rethinking how humans interact with technology. Their first product, Flow, provides effortless voice dictation within any application across Mac and Windows. Professionals at the world’s leading companies use Flow to streamline their writing, coding, and communication.
## Challenges 
### Building contextual speech-to-text that works everywhere 
Flow replaces typing with talking. For the output to be perfect, it can’t just capture what the user says – it has to capture what the user means.
**Sahaj Garg, Co-Founder and CTO** of Wispr Flow, and his team go beyond ordinary speech-to-text transcription to make a magical user experience. Flow structures, formats, and contextualizes dictation in real time to recreate exactly what the user would have typed. It’s a challenge to precisely use AI to subtly smooth speech while matching the user’s style and preferences.
> I write differently when texting my mom, my wife, and my colleagues. One early challenge was encapsulating this behavior and making it controllable, matching Flow’s output to the user’s context and preferences.
### Delivering a seamless user experience 
Two things matter for dictation: latency and reliability. Users expect a super-fast response every time, as occasional slowness can break their concentration.
One standard metric for LLM latency is p50 time-to-first-token, or the median time it takes to generate the first token from an inference request. Flow needed to hit a much more ambitious target: p99 end-to-end latency, or the time it takes to generate the complete output in at least 99 of 100 cases.
> We measure latency on a p90 or p99 basis for each user; we don’t care at all about p50. We’re optimizing the p99 experience for the p99 user.
### Building user trust 
Flow invites users to “break up with their keyboards” and go all-in on voice dictation for faster, more accurate writing. This requires building massive user trust. Running model inference on private, dedicated deployments is necessary for processing sensitive user data.
Recently, Flow launched new team and enterprise plans, which introduced additional requirements around security, compliance, and data residency. These requirements extend to model inference, which is at the core of the product.
## Solutions 
### Fine-tuning Llama models for transcript cleanup tasks 
Sahaj and his team chose Llama, a family of open-source LLMs by Meta, as the base for their real-time transcript cleanup step. They fine-tuned these LLMs to precisely solve user tasks based on the users’ context and preferences.
Building on open-source models like Llama gives Wispr Flow’s engineers more flexibility and control. They can completely customize the model to fit their needs, and they retain complete ownership over their AI systems. This customization extends beyond the fine-tuning process into inference, as they can run the model with their choice of serving framework, hardware, and cloud provider on private dedicated deployments.
> Llama is controllable and customizable, which lets us focus on the output.
### Running Llama inference with Baseten and AWS 
Baseten powers Flow with low-latency inference on dedicated deployments for these fine-tuned Llama models. With Baseten, Flow gets:
  1. Model performance optimizations to run fine-tuned Llama models with state-of-the-art latency.
  2. A suite of [model management ](https://www.baseten.co/platform/model-management/)tools for orchestrating inference across multiple custom models in a multi-step pipeline.
  3. Autoscaling infrastructure to handle traffic spikes and peak usage.
  4. Hands-on technical support from AI engineering experts to build frontier capabilities.


Baseten runs Flow’s Llama inference on AWS workload planes. Sahaj and his team decided to work with AWS as they’re a trusted infrastructure provider with a robust presence in multiple regions near Flow’s users.
> With Baseten and AWS, we have providers that we trust, and more importantly, our users can trust.
## Results 
### Clean transcripts in under 700 milliseconds, every time 
With Baseten, Flow’s entire pipeline, from speech recognition models to Llama-based transcript enhancement, runs end-to-end in under 700 milliseconds. This latency target isn’t for p50 or even p90 – this is the p99 latency to ensure a smooth user experience.
To hit this latency target, the Llama model must consistently process and generate 100+ tokens in under 250 milliseconds. The Wispr Flow team achieved this with Baseten’s [TensorRT-LLM engine builder for fine-tuned Llama models](https://www.baseten.co/blog/automatic-llm-optimization-with-tensorrt-llm-engine-builder/) and [Chains framework for multi-step inference](https://www.baseten.co/blog/baseten-chains-for-production-compound-ai-systems/).
> With Baseten, we gained a lot of control over our entire inference pipeline and worked with Baseten’s team to optimize each step.
### Secure, compliant, multi-region infrastructure 
Sahaj and his team have always taken security seriously, as Flow’s users trust their application to handle highly sensitive data. But to expand their product to support teams and enterprises, they knew they’d need additional security and compliance measures.
Baseten’s SOC 2 Type II certified and HIPAA-compliant inference platform, combined with the ability to deploy into specific regions within AWS, provided Flow a strong starting point for its certification processes ahead of launching its new team and enterprise plans.
### Seamless scale for viral moments 
The Wispr Flow team is no stranger to viral moments. From reaching #1 product of the week on Product Hunt to viral launch videos featuring everything from six-foot keyboards to crosscut saws, the team is an expert at getting massive user attention.
With Baseten, Sahaj and his team can access autoscaling GPUs to handle traffic spikes. This extra capacity is available when needed and scales to zero when not in use, saving money.
> We no longer have to reserve GPUs just in case we see usage spikes during viral moments.
This scalability is critical as Flow gains wider adoption with its new team and enterprise offerings.
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
