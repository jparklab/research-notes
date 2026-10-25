---
title: You Decided to Run the LLM Inside. Now What?
id: you-decided-to-run-the-llm-inside-now-what
tags:
- llm-inference
- model-layer
- internal-llm-inference-stacks-7f0c90
- serving-framework-choice
- model-risk-management
- quantization
- practitioner-essay
- onprem-llm-inference-stacks-859099
created: '2026-10-09T22:56:49.609905Z'
updated: '2026-10-23T17:25:28.370239Z'
source: https://mycroftproject.substack.com/p/on-premise-llm-deployment-financial-institutions
source_domain: mycroftproject.substack.com
fetched_at: '2026-10-09T22:56:49.586400Z'
fetch_provider: crawl4ai
status: review
type: note
tier: commentary
content_type: blog
deprecated: false
summary: 'Mycroft Project (Substack newsletter/practitioner essay, May 5 2026) framed
  around an EXPLICITLY FICTIONALIZED/COMPOSITE scenario (''a composite of deployment
  experiences observed across financial institutions,'' not a named, verified case)
  of a regional-bank ML engineer deploying an on-prem LLM for credit analysis. Not
  empirical data -- an opinionated practitioner framework on five infrastructure decisions:
  (1) GPU sizing as capacity planning (provision for quantized-precision footprint
  + >=20% headroom + 2-3x peak concurrent volume + a second deployment unit for zero-downtime
  updates from day one -- illustrative figures given: 7B at INT8 ~7GB fits an A100
  40GB; 70B at FP16 ~140GB needs 4x A100-80GB vs ~35GB at INT4 fitting one A100-80GB);
  (2) quantization as the lever determining on-prem economic viability, chosen per-workload
  (e.g., INT4 for summarization vs INT8 for credit-decisioning where regulatory scrutiny
  is higher); (3) serving framework choice as what separates a model file from a production
  system -- names vLLM (PagedAttention, default for high-concurrency), TGI (model-sharding
  across GPUs), and Triton Inference Server (NVIDIA-stack integration) as the three
  leading options; (4) monitoring architecture must separate infrastructure monitoring
  (Prometheus/Grafana/NVIDIA DCGM -- GPU utilization, latency percentiles, queue depth)
  from model behavior monitoring (hallucination rate, refusal rate, output-format
  violations, task-specific accuracy) -- explicitly ties ongoing model behavior monitoring
  to SR 11-7 model risk management obligations, arguing a deployment without it is
  ''an unmonitored model that happens to be running on internal infrastructure'';
  (5) model currency/update pipeline as a recurring responsibility (versioned, staged,
  blue-green/canary rollback) because open-weight models improve fast enough that
  defensibility under MRM scrutiny decays between releases.'
utility_score: 16.0
---

# [The Mycroft Project](https://mycroftproject.substack.com/)
SubscribeSign in
Discover more from The Mycroft Project
The Mycroft Project
Already have an account? Sign in
# You Decided to Run the LLM Inside. Now What?
### Getting an LLM inside a financial institution's trust boundary is the easy decision. Building the infrastructure that keeps it production-grade is where most deployments stall.
May 05, 2026
Share
A senior ML engineer at a regional bank is handed a mandate in Q1: deploy a large language model for internal credit analysis, entirely on-premise, no data leaving the building. The compliance team has signed off. The use case is clear. The business case is approved.
Thanks for reading The Mycroft Project! Subscribe for free to receive new posts and support my work.
Three months later, she has a model running. It is slow — P99 latency sits at twelve seconds for a credit memo summary, which is four times longer than the workflow it is supposed to accelerate. It is expensive to run — the GPU cluster provisioned for the pilot is consuming infrastructure budget at a rate the finance team is beginning to question. And it is fragile — two model updates have required manual re-deployment that took the system offline for hours each time.
The compliance team approved the deployment. Nobody scoped the infrastructure.
This scene — a composite of deployment experiences observed across financial institutions — plays out more often than the post-deployment retrospectives acknowledge. The infrastructure decisions made in those first ninety days — on GPU sizing, model quantization, serving frameworks, monitoring architecture, and model currency — are the difference between a deployment that scales and one that becomes a cautionary tale. This piece covers those decisions specifically.
## The Five Infrastructure Decisions That Determine Everything
### GPU sizing is a capacity planning problem, not a hardware procurement problem
The instinct when provisioning GPU infrastructure for an on-premise LLM is to ask: how much GPU memory does the model require? This is the wrong question. The right question is: what inference throughput does the workflow require, at what latency, under what peak load conditions, and what headroom is needed for model updates, concurrent requests, and the batch workloads that will inevitably be scheduled on the same infrastructure?
A 7B parameter model at INT8 quantized precision requires approximately 7GB of GPU memory — fitting comfortably on a single A100 40GB card with meaningful headroom. But fitting the model in GPU memory is the minimum requirement, not the production requirement. Production inference requires memory headroom for the KV cache that grows with context length, for concurrent request batching, and for the serving framework overhead that sits above the model. A deployment that loads a model into GPU memory with no headroom will serve requests slowly, queue under load, and fail unpredictably when context lengths exceed what the available memory can accommodate.
The practical sizing rule I’ve seen work consistently: provision for the model at quantized precision, allocate at least 20% of total GPU memory as headroom, size for 2-3x your expected peak concurrent request volume, and plan for a second deployment unit from day one — not as redundancy, but as the target for model updates while the primary unit continues serving traffic. Single-GPU deployments that require taking the system offline for model updates are not production deployments. They are expensive development environments.
### Model quantization is the decision that determines whether on-premise LLM is economically viable
A model at full FP32 precision is accurate but expensive. A model at FP16 half precision is nearly as accurate, runs on half the GPU memory, and is faster at inference — with minimal quality degradation for most financial NLP tasks. A model at INT8 or INT4 precision is smaller still, faster still, and requires significantly less GPU memory — with quality degradation that is workload-dependent and must be evaluated specifically for the use case.
For financial institutions deploying on-premise LLMs, quantization is not an optimization. It is the mechanism that determines whether the deployment is economically viable. A 70B parameter model at FP16 requires approximately 140GB of GPU memory — four A100 80GB cards running at near-capacity. The same model at INT4 requires approximately 35GB — fitting on a single A100 80GB card with meaningful headroom. The hardware cost difference between those two configurations is significant. The quality difference for credit memo summarization, covenant extraction, and regulatory document analysis is usually small enough to be acceptable.
The quantization decision is not made once. It is made per workload, per model, per acceptable quality threshold. A model quantized to INT4 for internal document summarization may need to run at INT8 for credit decisioning support, where the quality bar is higher and the regulatory scrutiny of model outputs is more intense. Building a serving infrastructure that can host the same model at different quantization levels for different workloads — rather than deploying a single quantized version for all use cases — is the architectural pattern that gives financial institutions flexibility as their on-premise LLM usage matures.
### Serving frameworks are the difference between a model and a production system
A model file is not a production system. A production system is a model plus the serving framework that handles request routing, batching, queuing, load balancing, version management, and the API layer that downstream applications use to interact with the model.
The leading serving frameworks for on-premise LLM deployment — vLLM, TGI (Text Generation Inference), and Triton Inference Server — differ in ways that matter for financial deployments. vLLM’s PagedAttention memory management produces significantly higher throughput than naive serving approaches, making it the default choice for high-concurrency deployments. TGI’s model sharding support makes it more practical for deployments that need to split large models across multiple GPUs. Triton’s integration with NVIDIA’s broader inference stack makes it the natural choice for institutions already running NVIDIA-based ML infrastructure and wanting a unified serving layer across model types.
The serving framework decision also determines the operational model for the deployment. The fragile manual re-deployment in the opening scenario was a symptom of a serving framework chosen for ease of initial setup rather than production operability. Every model update that requires taking the serving endpoint offline is a risk event in a financial production environment. The serving framework should make that risk unnecessary.
### Monitoring architecture for on-premise LLMs has two layers that must not be conflated
For financial institutions subject to SR 11-7, model behavior monitoring is not optional. It is the ongoing monitoring component of the model risk management framework applied to the LLM. An on-premise LLM without model behavior monitoring is not a monitored deployment under SR 11-7. It is an unmonitored model that happens to be running on internal infrastructure.
Infrastructure monitoring and model behavior monitoring are distinct concerns that require distinct instrumentation. Infrastructure monitoring tracks what every GPU workload needs tracked: GPU memory utilization, compute utilization, inference latency by percentile, request queue depth, error rates, and hardware health metrics. This layer is handled adequately by standard infrastructure monitoring tooling — Prometheus, Grafana, NVIDIA’s DCGM exporter.
Model behavior monitoring tracks whether the model is producing outputs that are appropriate, accurate, and consistent with the use case it was deployed for. In a financial context, this means monitoring output quality metrics specific to the workflow — factual accuracy in document summarization, extraction completeness in covenant analysis, consistency of classification in regulatory document categorization — and alerting when those metrics degrade. It also means monitoring for the failure modes specific to LLMs: hallucination rates, refusal rates, output format violations, and prompt injection patterns in adversarial inputs. Conflating the two layers — building infrastructure monitoring and calling it model observability — is the most common failure mode in on-premise LLM deployments at financial institutions, and it is the hardest to detect because the infrastructure monitoring looks green while the model behavior is degrading.
### Model currency is a recurring infrastructure responsibility, not a one-time deployment decision
A model that was state-of-the-art at deployment and has since been superseded is not just slower than alternatives. Its outputs are less defensible under model risk management scrutiny — and the institution may not know it until a model governance review surfaces the version history.
Open-weight models improve rapidly. A model deployed in Q1 may be meaningfully outperformed by a new version released in Q2 — not on general benchmarks, but on the specific financial NLP tasks the deployment was built for. Updating the model requires re-evaluating quantization parameters, re-validating output quality on the institution’s specific workloads, re-testing the serving framework configuration, and deploying the update without downtime. This is not a one-time exercise. It is a recurring operational responsibility that scales with the pace of open-weight model improvement.
The institutions that have solved this have built a model update pipeline that treats LLM updates the same way they treat production software deployments: versioned, tested, staged, deployed via a blue-green or canary mechanism that allows rollback if the new model underperforms on production traffic. The update pipeline is infrastructure. It needs to be built before the first model update is needed, not in response to it.
## The Decision Was the Easy Part
Deciding to run the LLM inside the trust boundary is a governance decision. It requires compliance sign-off, legal review, and executive approval. It is difficult to get but straightforward to scope.
Building the infrastructure that makes the deployment production-grade is an engineering decision. It requires GPU sizing discipline, quantization strategy, serving framework selection, a monitoring architecture that distinguishes infrastructure health from model behavior, and an update pipeline that keeps the model current without operational disruption. It is less visible than the governance decision, less celebrated when done well, and far more consequential when done poorly.
The senior ML engineer in the opening scenario had the governance decision. What she needed — and what most on-premise LLM deployments at financial institutions lack from day one — was the same rigor applied to GPU sizing, quantization, serving framework, and monitoring that was applied to the compliance sign-off that made the deployment possible.
> **An on-premise LLM deployment that runs slowly, degrades silently, and requires manual intervention for every model update is not a production deployment. It is a pilot that survived its own demo. The infrastructure decisions made in the first ninety days determine which one you end up with.**
_The next piece covers LLM evaluation frameworks for regulated financial use cases — how to benchmark models for the performance and fairness requirements that financial AI demands, and why the benchmarks that come with the model card are not adequate for production financial systems. Subscribe and reply with the infrastructure decision that has most constrained your on-premise AI deployment. I read every response._
Thanks for reading The Mycroft Project! Subscribe for free to receive new posts and support my work.
Share
#### Discussion about this post
CommentsRestacks
TopLatestDiscussions
[The Invisible Infrastructure: What 435,000 Stars Tell You About the AI Hierarchy](https://mycroftproject.substack.com/p/the-invisible-infrastructure-what)
[Five companies. One question. No server required.](https://mycroftproject.substack.com/p/the-invisible-infrastructure-what)
Feb 17 •
[Can the audit layer audit itself?](https://mycroftproject.substack.com/p/the-audit-trail-cannot-prove-the)
[It can enforce the record. It cannot verify the truth of it.](https://mycroftproject.substack.com/p/the-audit-trail-cannot-prove-the)
May 26 •
[Eleven Decisions. $38,400 Gone. Here’s the Agent That Would Have Stopped It. ](https://mycroftproject.substack.com/p/eleven-decisions-38400-gone-heres)
[PM Summary. The Mycroft Project · Sprint Report](https://mycroftproject.substack.com/p/eleven-decisions-38400-gone-heres)
Mar 11 •
See all
### Ready for more?
© 2026 LiamBearBrown[Privacy](https://substack.com/privacy)[Terms](https://substack.com/tos)[Collection notice](https://substack.com/ccpa#personal-data-collected)
[Substack](https://substack.com) is the home for great culture
