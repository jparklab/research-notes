---
title: 'AI Inference Cost Economics in 2026: GPU FinOps Playbook | Spheron Blog'
id: ai-inference-cost-economics-in-2026-gpu-finops-playbook-spheron-blog
tags:
- internal-llm-inference-stacks-7f0c90
- gpu-utilization
- primary-source
- vendor-marketing
created: '2026-10-09T23:05:41.462964Z'
updated: '2026-10-10T14:52:17.207984Z'
source: https://www.spheron.network/blog/ai-inference-cost-economics-2026/
source_domain: www.spheron.network
fetched_at: '2026-10-09T23:05:41.439897Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'PRIMARY SOURCE WITH A VENDOR INTEREST: Spheron is itself a GPU cloud rental
  platform, so this playbook has a direct commercial interest in presenting self-hosting-on-GPU-cloud
  (specifically Spheron) favorably against both managed inference APIs and against
  AWS -- a mirror-image vendor bias to BentoML/Azumo (those sell against self-hosting;
  Spheron sells self-hosting infrastructure). Confirms the core utilization claim
  Forte Group cites: a case study serving Llama 3.1 70B to an internal enterprise
  tool with 500 DAU, weekday 9am-6pm traffic, found average GPU utilization of 22%
  (''After'' state); applying FP8 quantization (1.8x traffic on the same footprint)
  and continuous batching (utilization 22%->68%) plus moving batch jobs to spot and
  switching cloud providers cut monthly spend from ~9,100 to ~6,151 (59% reduction).
  Gives exact cost-per-million-token (CPM) formulas and a GPU pricing/throughput table
  (04 Apr 2026 Spheron rates): A100 80G 8x ~.67/M tokens, H100 SXM5 8x ~.90/M tokens
  (~2,800 tok/s for Llama 3.1 70B FP16 via vLLM), H200 8x ~.80/M, B200 8x ~.18/M;
  FP8 on H100 cuts CPM to ~/bin/bash.95-1.10/M. States continuous batching (PagedAttention)
  raises GPU utilization from a baseline of 15-30% to 60-80% at typical traffic patterns
  -- a 3-4x effective throughput improvement at the same GPU cost, corroborating the
  utilization-reality gap theme from an inference-infrastructure-vendor''s perspective.
  Gives its own break-even framework: under 20M tokens/month managed APIs win on total
  cost including ops; above 100M tokens/month self-hosting almost always wins on unit
  economics; self-hosting breaks even at roughly 50-100M tokens/month for most 70B-class
  models once engineering overhead is included -- a THIRD distinct break-even band
  (50-100M tokens/month) that disagrees with tianpan.co''s 100M+ tokens/month ''scale
  stage,'' dev.to/rileykim''s internally-contradictory 50M vs 500M tokens/day claims,
  and promptcost.org''s 500K-2M tokens/day claims (which is a different unit: tokens/day
  vs tokens/month -- converting promptcost''s range to monthly terms, ~15M-60M tokens/month,
  actually lands closer to Spheron''s band than to the others, illustrating how unit
  confusion alone drives much of the apparent disagreement in this corpus). Compares
  Spheron (9.20/hr for an 8x H100 SXM5 pod) against AWS p5.48xlarge (~5.04/hr for
  the same GPU count) and Runpod (~1.52/hr), with AWS adding ~00/month egress fees
  per 10TB -- while flagging this is itself a vendor-favorable comparison.'
---

*Suggested by [[llm-inference-a-guide-to-build-vs-buy]] — cited for the 22% realistic internal-deployment GPU utilization scenario and continuous-batching/FP8 improvement figures*

Training was the cost center in 2021-2023. Inference is the cost center now. Industry analysts estimate 55-80% of enterprise AI GPU spend goes to inference. Either way, the trend is clear: once a model ships to production, serving costs accumulate every hour, every day, indefinitely. This playbook covers the four layers of optimization (model, runtime, infrastructure, FinOps), the math behind cost-per-token, and a real-world case study that took a 70B model deployment from $39K to $16K per month.
## The Great Inversion: How Inference Ate the Training Budget
Training is a fixed compute job. You run it for days or weeks, it finishes, and the cost stops. Inference is the opposite: it starts when you ship and never stops as long as users are hitting your API.
The math compounds fast. Take a 70B model serving 1,000 daily active users averaging 1,000 requests per user per day at 500 tokens per request:
  * Daily token volume: 500M tokens/day (1,000 users × 1,000 requests × 500 tokens)
  * At $1.90/million tokens (on-demand H100 SXM5, 8x node, at the Spheron rate recorded on 04 Apr 2026): $950/day
  * Annualized: ~$347,000/year in compute alone, before egress and infrastructure overhead


That number is not unusual. For any team with real production traffic, inference costs will exceed training costs within weeks of launch. Most teams underestimate this because they plan around training budgets and treat inference as an afterthought until the bill arrives.
For foundational GPU cost strategies that apply to both training and inference, see the [GPU Cost Optimization Playbook](https://www.spheron.network/blog/gpu-cost-optimization-playbook/). For a complete breakdown of the inference engineering role and the skills involved, see [What Is Inference Engineering](https://www.spheron.network/blog/inference-engineering-guide-2026/). For a cross-model, cross-GPU benchmark showing CPM for Llama 4, Qwen 3, Gemma 4, and DeepSeek V3, see the [GPU Cost Per Token Benchmark 2026](https://www.spheron.network/blog/gpu-cost-per-token-benchmark-llm-inference-2026/).
## Anatomy of an Inference Bill: Cost Per Token Across GPU Types
The most useful metric for inference economics is cost per million tokens (CPM). It normalizes GPU price and throughput into a single comparable figure. For the underlying spec and pricing data across every GPU tier we reference below, see our [GPU cloud benchmarks guide](https://www.spheron.network/blog/gpu-cloud-benchmarks/) with April 2026 live rates.
The formula:

```
CPM = (GPU $/hr) / (tokens_per_sec × 3600 / 1,000,000)
```

Using the Spheron on-demand rates recorded on 04 Apr 2026 (a frozen worked example) and vLLM throughput benchmarks for Llama 3.1 70B at batch size 256 (512 input / 512 output tokens):  
| GPU  | Config  | $/hr (on-demand, 04 Apr 2026)  | vLLM Throughput (Llama 3.1 70B)  | CPM ($)  |  
| --- | --- | --- | --- | --- |  
| A100 80G SXM4  | 8x  | $1.05/GPU = $8.40  | ~1,400 tok/s  | ~$1.67  |  
| H100 SXM5  | 8x  | $2.40/GPU = $19.20  | ~2,800 tok/s  | ~$1.90  |  
| H200 SXM5  | 8x  | $4.54/GPU = $36.32  | ~3,600 tok/s  | ~$2.80  |  
| B200 SXM6  | 8x  | $7.43/GPU = $59.44  | ~5,200 tok/s  | ~$3.18  |  
Throughput figures are for 70B FP16 at batch 256. Actual numbers vary with sequence length and concurrency profile. For exact measured values across engines, see [vLLM vs TensorRT-LLM vs SGLang Benchmarks](https://www.spheron.network/blog/vllm-vs-tensorrt-llm-vs-sglang-benchmarks/).
Live 8-GPU on-demand node rates today: A100 $11.84/hr, H100 $21.20/hr, H200 $44.08/hr, B200 $72.48/hr. Rerun the CPM formula with them.
> Pricing fluctuates based on GPU availability. Spheron rates above are live as of **09 Oct 2026** ; other providers reflect their most recent published rates and may have changed. Check [current GPU pricing →](https://www.spheron.network/pricing/) for live rates.
The A100 has the lowest CPM for 70B FP16 at this batch size because its lower per-GPU price offsets its lower throughput. The H100 and newer GPUs become more cost-efficient when you factor in FP8 quantization, which roughly doubles throughput without changing the $/hr. FP8 on H100 brings CPM for the 8x node from ~$1.90 to approximately $0.95-1.10, making it the better choice for most production deployments.
These figures assume GPU cloud pricing, where power and cooling are bundled into the hourly rate. Teams evaluating on-premise deployment should read our [AI inference power and electricity cost breakdown](https://www.spheron.network/blog/ai-inference-power-electricity-cost-2026/) for the separate electricity TCO model.
For per-GPU benchmark detail and workload-specific guidance, see [Best GPU for AI Inference in 2026](https://www.spheron.network/blog/best-gpu-for-ai-inference-2026/). Looking further ahead, NVIDIA's Groq 3 LPU (announced at GTC 2026) promises 35x more inference throughput per megawatt than HBM-based GPUs by replacing off-chip memory with a massive on-chip SRAM pool. It targets the decode phase specifically, not training or prefill. See the [NVIDIA Groq 3 LPU explained](https://www.spheron.network/blog/nvidia-groq-3-lpu-explained/) for architecture details and how it fits into the cost-per-token picture.
## The Four Optimization Layers
Inference cost reduction is not one technique. It is four layers applied in sequence, each with independent savings potential:  
| Layer  | What It Covers  | Typical Savings  |  
| --- | --- | --- |  
| Model  | Quantization, distillation, right-sizing  | 30-75% cost reduction  |  
| Runtime  | Batching, speculative decoding, KV cache  | 40-80% throughput gain  |  
| Infrastructure  | Spot vs on-demand, auto-scaling, GPU type  | 40-65% unit cost reduction  |  
| FinOps  | Attribution, metering, budgets  | Prevents waste accumulation  |  
The order matters. Start with the model layer: model changes affect every subsequent dollar spent. Then optimize the runtime. Then choose infrastructure to match the workload pattern. FinOps runs continuously throughout.
### Model Layer: Quantization, Distillation, and Right-Sizing
**FP8 quantization** is the first thing to apply on H100 (vLLM supports it natively). It gives 1.3-2x throughput gain over FP16 at under 2% quality loss on instruction-tuned models. For standard conversational AI, summarization, and code generation tasks, the quality difference is not perceptible in production.
**FP4 on B200** via TensorRT-LLM adds a further 1.5-2x gain over FP8. More quantization error than FP8, so test against your eval suite before moving to production. For the full B200 quantization economics, see [FP4 Quantization on Blackwell GPUs](https://www.spheron.network/blog/fp4-quantization-blackwell-gpu-cost/).
**INT4 (GPTQ/AWQ):** Suitable for less quality-sensitive workloads like classification and summarization. Not recommended for complex reasoning tasks. At INT4, a 70B model fits on a single H100 with room for KV cache, versus two GPUs minimum at FP16. For high-batch serving, INT4 lets you cut the pod from eight GPUs to two or three while maintaining throughput.
**Distillation** is the highest-impact model change, but also the most work. If your 70B model handles tasks that a 14B or 8B model manages adequately, distillation cuts GPU requirements by 4-8x. The cost math: 70B on 8x H100 at $21.20/hr versus 14B on 2x H100 at $5.30/hr for comparable throughput on the target task. For setup details, see [Model Distillation on GPU Cloud](https://www.spheron.network/blog/model-distillation-gpu-cloud-7b-student-70b-teacher/).
**Right-sizing** is often skipped. A 14B model at FP8 frequently matches a 70B INT4 on standard NLP tasks. Run your eval before defaulting to the largest available model.
### Runtime Layer: Batching, Speculative Decoding, and KV Cache
**Continuous batching** is the single most impactful runtime change for most teams. Static batching leaves the GPU idle between requests. vLLM's continuous batching (PagedAttention) processes new tokens as slots free up, raising GPU utilization from 15-30% to 60-80% at typical traffic patterns. That is a 3-4x improvement in effective throughput at the same GPU cost.
**Speculative decoding** uses a small draft model to generate candidate tokens and a larger target model to verify them in parallel. For output-heavy workloads (code generation, long-form text), this gives 2-4x throughput improvement. It works best when the draft and target models are from the same family. For production setup details, see [Speculative Decoding Production Guide](https://www.spheron.network/blog/speculative-decoding-production-guide/).
**KV cache optimization** matters most for long-context workloads. PagedAttention eliminates internal VRAM fragmentation. For 32K+ token contexts, KV cache quantization to INT8 or FP8 cuts VRAM usage by 30-50%, freeing capacity for more concurrent requests. See the [KV Cache Optimization Guide](https://www.spheron.network/blog/kv-cache-optimization-guide/) for memory calculations.
For a head-to-head comparison of how vLLM, TensorRT-LLM, and SGLang implement these techniques and how they affect throughput and latency numbers, see [vLLM vs TensorRT-LLM vs SGLang Benchmarks](https://www.spheron.network/blog/vllm-vs-tensorrt-llm-vs-sglang-benchmarks/).
### Infrastructure Layer: Spot vs On-Demand, Auto-Scaling, and GPU Right-Sizing
The infrastructure decision changes based on workload type. Using live Spheron pricing:  
| GPU  | On-Demand  | Spot  |  
| --- | --- | --- |  
| H100 SXM5  | $2.65/hr  | $2.20/hr  |  
| H200 SXM5  | $5.51/hr  | $3.32/hr  |  
| B200 SXM6  | $9.06/hr  | $5.22/hr  |  
| A100 80G SXM4  | $1.48/hr  | $1.22/hr  |  
| L40S  | $0.96/hr  | $1.02/hr  |  
> Pricing fluctuates based on GPU availability. Spheron rates above are live as of **09 Oct 2026** ; other providers reflect their most recent published rates and may have changed. Check [current GPU pricing →](https://www.spheron.network/pricing/) for live rates.
**Synchronous API with latency SLAs:** Use on-demand H100 or H200. Spot interruptions are incompatible with sub-2-second P99 requirements.
**Batch inference pipelines:** Embeddings, async summarization, nightly report generation, evaluation runs. These are the right workloads for spot. Compare each GPU's spot rate against its on-demand rate in the table above before committing (L40S spot is $1.02/hr), and remember spot capacity can be reclaimed without notice. Build a job queue with retry logic and exponential backoff. A spot interruption causes a retry, not a user-visible error. See [instance types on Spheron](https://docs.spheron.ai/concepts/instance-types) for current spot availability by GPU model. If you're choosing between L40 and L40S for inference cost, our [L40 vs L40S inference comparison](https://www.spheron.network/blog/nvidia-l40-vs-l40s-inference-comparison/) covers the tokens-per-second-per-watt math. Teams running batch jobs across more than one provider can squeeze further savings out of this layer with [cross-cloud spot arbitrage](https://www.spheron.network/blog/gpu-spot-instance-arbitrage-2026/), routing each job to whichever provider's spot pool is cheapest that hour.
**Auto-scaling trigger:** Scale GPU count based on pending request queue depth, not CPU or memory. A queue of more than 50 pending requests per GPU is typically the signal to scale out. CPU and memory give lagging signals for inference workloads; queue depth is real-time.
**GPU right-sizing:** For models under 14B at FP8, a single H100 PCIe ($2.65/hr) may match four A100 SXM4 GPUs ($5.92/hr combined) with simpler networking. Run benchmarks before committing. For 70B-class serving where prefill and decode have very different hardware needs, [heterogeneous GPU inference](https://www.spheron.network/blog/heterogeneous-gpu-inference-cost-optimization/) goes further than right-sizing a single GPU type, routing each phase to the hardware it actually needs.
At scale, GPU count directly maps to continuous data center power draw. When power availability caps expansion before budget does, [AI data center power constraints 2026](https://www.spheron.network/blog/ai-data-center-power-constraints-2026/) covers distributed capacity and grid bypass strategies.
For the billing model decision framework (serverless vs on-demand vs reserved), see [Serverless vs On-Demand vs Reserved GPU](https://www.spheron.network/blog/serverless-gpu-vs-on-demand-vs-reserved/). For GPU sharing strategies using MIG and time-slicing, see [Run Multiple LLMs on One GPU](https://www.spheron.network/blog/run-multiple-llms-one-gpu-mig-time-slicing-guide/). If you are evaluating whether Google TPU is a cheaper alternative to GPU for inference, see our [TPU Trillium v6 vs B200 cost-per-token comparison](https://www.spheron.network/blog/google-tpu-trillium-v6-vs-nvidia-b200-llm-inference/). Analysts project hyperscalers will shift 60-70% of their internal inference to custom ASICs by 2028. That projection is about Trainium, TPU, Maia, and MTIA - captive chips unavailable on the open market. See [Hyperscaler Custom AI Chips in 2026](https://www.spheron.network/blog/hyperscaler-custom-ai-chips-2026-trainium-tpu-maia-mtia-vs-nvidia-gpu/) for the full breakdown. For a detailed case on captive ASIC vs GPU cloud cost tradeoffs specifically around Trainium 3, see [Trainium 3 vs H200 and B200: real cost per token](https://www.spheron.network/blog/aws-trainium-3-vs-nvidia-h200-b200-llm-training-inference-2026/) which includes the full Neuron SDK migration cost in the break-even analysis.
### FinOps Layer: Cost Attribution, Token Metering, and Budget Alerts
Without attribution, GPU spend is a black box. With it, you can see exactly which model, use-case, and team is driving cost.
**Tag at submission.** Every inference job should carry three tags: model name, use-case ID, and team or product. These flow through to your cost reporting without any additional work.
**Emit token counts as metrics.** GPU hours alone do not tell you if your cost is rising because traffic increased or because efficiency dropped. Token counts give you the denominator. Store both in your data warehouse and compute CPM as a weekly KPI.
**Budget alerts at 80%, not 100%.** By the time you hit 100% of budget, there is nothing left to do. An 80% alert gives you a week or two to investigate and course-correct before the next billing cycle.
**Review cadence.** Inference costs move fast. Weekly reviews catch regressions before they compound. Monthly reviews are appropriate for training, where costs are more predictable.
The mechanics of splitting that cost across teams - and actually billing them for it - are covered in the [GPU FinOps chargeback guide](https://www.spheron.network/blog/gpu-cloud-finops-ai-teams-cost-allocation-chargeback-budgeting/).
## Spheron vs AWS vs Runpod: Inference Cost Per Million Tokens
Provider pricing affects the baseline before any optimization. The table below compares Spheron's on-demand H100 SXM5 pricing against representative estimates for AWS and Runpod. AWS and Runpod prices are estimates based on public pricing pages as of March 2026 and may not reflect reserved or negotiated rates.  
| Provider  | GPU  | 8-GPU Pod $/hr  | vLLM 70B FP16 (tok/s)  | CPM ($)  | Egress/Networking  |  
| --- | --- | --- | --- | --- | --- |  
| Spheron  | H100 SXM5  | $19.20 (8 × $2.40, 04 Apr 2026)  | ~2,800  | ~$1.90  | None  |  
| Runpod  | H100 SXM5  | ~$21.52 (8 × $2.69)  | ~2,800  | ~$2.13  | Included  |  
| AWS (p5.48xlarge)  | H100 SXM5  | ~$55.04  | ~2,800  | ~$5.46  | $0.09/GB outbound  |  
Spheron's row is frozen at the rate recorded on 04 Apr 2026; its live 8x H100 on-demand node rate is $21.20/hr.
> Pricing fluctuates based on GPU availability. Spheron rates above are live as of **09 Oct 2026** ; other providers reflect their most recent published rates and may have changed. Check [current GPU pricing →](https://www.spheron.network/pricing/) for live rates.
Llama 3.1 70B in FP16 requires approximately 140 GB of VRAM, so it cannot run on a single 80 GB H100. An 8-GPU pod is the minimum configuration for this workload, which is why the table uses 8-GPU pod pricing. The AWS figure reflects the p5.48xlarge on-demand rate ($55.04/hr in us-east-1), which provides 8x H100 SXM5 GPUs. AWS inference workloads also accumulate ancillary costs: VPC NAT gateway, ALB for load balancing, and egress. A team pushing 10TB/month in API response data pays roughly $900/month in egress fees alone on AWS. Spheron charges a flat per-GPU-hour rate with no egress or networking overhead.
For teams running $20K/month in raw compute on AWS, migrating to Spheron typically reduces total spend by 20-35% after accounting for the rate difference and eliminated ancillary fees.
## Real-World Savings: From ~$39K/month to $16K/month
This scenario is based on a team serving Llama 3.1 70B to an internal enterprise tool with 500 DAU. Traffic peaks 9am-6pm on weekdays, drops to near-zero overnight and on weekends.
**Before:**  
| Item  | Details  | Monthly Cost  |  
| --- | --- | --- |  
| 4x H100 SXM5 nodes (4 GPUs each) on prior cloud provider  | On-demand, always-on, FP16, static batching at $2.50/GPU/hr (16 GPUs × $2.50 × 730 hrs)  | $29,200  |  
| AWS egress and networking  | ALB, NAT gateway, 8TB outbound  | $7,500  |  
| Storage  | Training artifacts, logs, checkpoints  | $2,400  |  
| **Total**  | -  | **~$39,100**  |  
Average GPU utilization: 22%. The cluster sits mostly idle from 7pm to 8am and all weekend.
**After (all four layers applied):**  
| Item  | Details  | Monthly Cost  |  
| --- | --- | --- |  
| 2x H100 SXM5 nodes on-demand (Spheron)  | FP8, continuous batching, always-on, 8 GPUs at the $2.40/GPU/hr rate recorded on 04 Apr 2026  | $14,016  |  
| 4x L40S spot for batch jobs (Spheron)  | Embeddings, async summarization, at the $0.32/hr spot rate recorded on 04 Apr 2026  | $935  |  
| Storage  | Reduced footprint after archive cleanup  | $1,200  |  
| **Total**  | -  | **~$16,151**  |  
Net savings after fully eliminating AWS ancillary fees: the effective monthly bill came to approximately $16K, a 59% reduction from the prior $39,100/month.
The changes that drove it:
  1. **FP8 quantization:** Same 4-GPU footprint served 1.8x the traffic. Reduced the number of nodes needed at peak from four to two.
  2. **Continuous batching:** GPU utilization rose from 22% to 68%. Far fewer idle GPU-hours.
  3. **Spot for batch:** Moved all embedding and summarization jobs to L40S spot, then $0.32/hr, a fraction of the H100 on-demand inference rate.
  4. **Provider switch:** Eliminated AWS egress, NAT gateway, and ALB costs entirely.


For a comparable cost reduction story on the training side, see [Spot GPU Training Case Study](https://www.spheron.network/blog/spot-gpu-training-case-study/).  
| Metric  | Before  | After  |  
| --- | --- | --- |  
| Monthly GPU cost  | ~$29,200  | ~$14,951  |  
| Infrastructure overhead  | ~$9,900  | ~$1,200  |  
| GPU utilization  | 22%  | 68%  |  
| Total monthly spend  | ~$39,100  | ~$16,151  |  
## Decision Framework: When to Self-Host vs Use Inference APIs
Managed inference APIs (OpenAI, Anthropic, Google) trade cost for simplicity. Self-hosting on GPU cloud trades simplicity for cost. For a worked example of where that crossover sits, see the [Claude Opus 4.8 API vs self-hosted cost breakdown](https://www.spheron.network/blog/claude-opus-4-8-api-vs-self-hosted-llms-cost-privacy-2026/). Here is how to pick:  
| Condition  | Self-Host on GPU Cloud  | Use Inference API  |  
| --- | --- | --- |  
| Token volume  | Greater than 100M tokens/month  | Less than 50M tokens/month  |  
| Latency SLA  | Strict P99 requirements  | Flexible  |  
| Model customization  | Fine-tuned or private model  | Foundation model  |  
| Ops capability  | Dedicated infra team  | Small team, no infra resources  |  
| Data privacy  | Private or regulated data  | Public or low-sensitivity data  |  
| Cost at scale  | $0.10-0.50/M tokens self-hosted  | $0.60-15/M tokens via API  |  
The break-even math: at 500M tokens/month with a 70B FP16 model on an 8x H100 pod via Spheron at roughly $1.90/M tokens (04 Apr 2026 rates), compute costs are ~$950/month. The same volume via OpenAI GPT-4o at $10.00/M output tokens is $5,000/month. With FP8 quantization cutting CPM to roughly $0.95-1.10/M, the gap widens further. Self-hosting breaks even at roughly 50-100M tokens/month for most 70B-class models when you factor in engineering overhead for running the infrastructure.
Below 20M tokens/month, managed APIs win on total cost including ops. Above 100M tokens/month, self-hosting almost always wins on unit economics. If your workload runs on 7B-13B models specifically, the math is even more favorable: see the [SLM deployment guide](https://www.spheron.network/blog/deploy-small-language-models-gpu-cloud/) for a worked cost example showing how a 7B model on an A100 ($1.48/hr) handles the same workload as a $10,000/month API bill. For teams evaluating serverless LLM inference platforms specifically, the [Fireworks AI alternatives guide](https://www.spheron.network/blog/fireworks-ai-alternatives/) covers the break-even math and migration options in detail. Teams paying Baseten's replica-hour markup should also read our [Baseten alternatives breakdown](https://www.spheron.network/blog/baseten-alternatives/) for direct H100-on-Spheron cost math, and our [Baseten pricing breakdown](https://www.spheron.network/blog/baseten-pricing-2026-model-serving-cost/) works the same utilization math per GPU tier, from T4 through B200. If your fine-tuned model case is actually dozens of per-tenant LoRA adapters on a shared base model rather than one custom checkpoint, the "fine-tuned model" row above needs its own math: see the [LoRA inference cost teardown](https://www.spheron.network/blog/multi-lora-serving-cost-teardown-when-shared-adapters-beat-d/) for where shared multi-adapter serving beats dedicated per-tenant GPUs.
For a specific breakdown comparing Gemini 3 Flash-Lite API spend against self-hosting efficient open SLMs at the same token volume, see the [Gemini Flash-Lite vs self-hosted inference cost guide](https://www.spheron.network/blog/gemini-3-flash-lite-vs-self-hosted-open-models-gpu-cloud/).
> Inference costs compound fast. If you are serving more than 50M tokens a month, running on-demand cloud instances with no cost optimization strategy is leaving real money behind. Spheron gives you transparent per-hour GPU pricing, spot instances on select GPU models, and no hidden egress fees.
> [Spheron H100 →](https://www.spheron.network/gpu-rental/h100/) | [View all GPU pricing →](https://www.spheron.network/pricing/) | [Compare all GPUs →](https://www.spheron.network/gpu-rental/)
> **[Start saving on inference today →](https://app.spheron.ai)**
STEPS / 06
## Quick Setup Guide
  1. 01
### Audit your current inference bill
Pull the last 30 days of GPU spend. Separate inference from training by tagging jobs or filtering by runtime duration. Calculate your average GPU utilization during inference windows using DCGM metrics or provider dashboards. Anything under 50% utilization is recoverable cost.
  2. 02
### Benchmark your inference throughput baseline
Run your current serving stack against a realistic request distribution. Measure tokens/sec at your P50 and P95 concurrency. This gives you the denominator for cost-per-token math and sets the baseline before any optimization.
  3. 03
### Apply the model layer optimizations
Start with FP8 quantization if you are on H100 or newer hardware. On B200, test FP4 via TensorRT-LLM. Measure output quality with your eval suite. For workloads where quality loss is acceptable at INT4, distillation or a smaller base model may reduce GPU requirements by 50-75%.
  4. 04
### Enable continuous batching and KV cache optimization
Switch to vLLM or SGLang if you are not already using them. Enable PagedAttention and set max_num_batched_tokens appropriate for your GPU VRAM. For long-context workloads, configure KV cache quantization (INT8 or INT4) to reclaim 30-50% of VRAM for more concurrent requests.
  5. 05
### Move batch workloads to spot instances
Identify all async inference jobs (embeddings, batch summarization, scheduled enrichments). Migrate these to spot GPU instances on Spheron where available. Configure retry logic with exponential backoff. Spot instances run significantly cheaper than on-demand for the same GPU type, making them viable for any workload that queues requests and can tolerate interruption.
  6. 06
### Instrument cost attribution and set budget alerts
Tag every inference job with model name, use-case, and team. Export token counts alongside GPU costs to your data warehouse. Set budget alerts at 80% of monthly target. Review cost-per-million-tokens weekly, not monthly, to catch regressions before they compound.


FAQ / 05
## Frequently Asked Questions
### 01Why does inference cost more than training for most AI teams in 2026?
Training runs once or a handful of times. Inference runs continuously as long as users hit the API. At production scale, a 70B model serving 1,000 daily active users generates enough token volume to dwarf the original training bill within weeks. Analysts estimate inference now accounts for 70-80% of total GPU cloud spend for teams that have shipped to production.
### 02What is cost per million tokens and how do I calculate it?
Cost per million tokens (CPM) is cluster cost divided by throughput. Formula: CPM = (cluster $/hr) / (tokens/sec × 3600 / 1,000,000). For example, at the $19.20/hr total recorded on 04 Apr 2026 for an 8x H100 SXM5 on-demand pod on Spheron, serving Llama 3.1 70B FP16 via vLLM at 2,800 tok/sec yields roughly $1.90 per million tokens. A 70B FP16 model requires 8 H100s to fit in VRAM, so the full pod cost is the right denominator. Throughput varies by model size, batch size, and quantization.
### 03What is the single biggest lever to reduce LLM inference costs?
Batching. Most teams run inference at low batch utilization, paying for GPU time that is mostly idle. Continuous batching (as implemented in vLLM and SGLang) can raise GPU utilization from under 20% to over 70%, cutting effective cost per token by 3-4x without changing hardware or model.
### 04When does spot GPU inference make sense?
Spot works for batch inference workloads (embeddings, async summarization, nightly report generation) where you can tolerate interruption. For synchronous APIs with latency SLAs, use on-demand. Spheron offers spot instances on select GPU models at significant discounts versus on-demand rates, making them viable for any pipeline that queues requests and can retry. Check the Spheron pricing page for current spot availability.
### 05How does Spheron compare to AWS for inference costs?
Spheron charges a flat per-hour GPU rate with no separate egress, networking, or storage fees. AWS inference workloads typically add 15-30% in ancillary charges on top of the instance cost. For a team running $20K/month in raw compute, Spheron's pricing model typically results in a 20-35% lower total bill for equivalent GPU hours.
## Related Reading
[More on Cost Optimization →](https://www.spheron.network/blog/topics/cost-optimization/)
[ Engineering RAG Inference Cost Calculator: Cost Per Token, Broken Down (2026) Sep 29, 2026 ](https://www.spheron.network/blog/rag-cost-per-token-embedding-retrieval-and-generation-broken/)[ Engineering Cost Per Token Vision Model: Image Tokens Cost More Sep 23, 2026 ](https://www.spheron.network/blog/cost-per-token-for-vision-language-models-image-tokens-cost-/)[ Engineering Agentic AI Inference Cost: Why Agents Burn 5-30x Tokens Jul 16, 2026 ](https://www.spheron.network/blog/agentic-ai-inference-cost-2026/)[ Engineering LLM API Cost Comparison: The Real Cost Per Token Formula Sep 17, 2026 ](https://www.spheron.network/blog/llm-api-cost-comparison-cost-per-token-formula/)[ Engineering Cost Per Million Tokens: How to Calculate Your Inference Bill Sep 3, 2026 ](https://www.spheron.network/blog/cost-per-million-tokens/)[ Tutorial NCCL Tests: Benchmark Your GPU Cluster Interconnect (2026) Sep 24, 2026 ](https://www.spheron.network/blog/benchmark-your-gpu-cluster-interconnect-with-nccl-tests-2026/)
Try It Yourself
### Try It on Real GPUs
The GPUs behind these guides are the ones you can rent here: H100s, H200s, B200s, and more, billed per minute after a 20-minute minimum runtime, with no contracts. Pick one and you are live in under two minutes.
Rent a GPU
Deploy Time
< 2 min
Uptime SLA
99.9%
GPU Models
10+
Starting At
$0.78/hr
We use cookies to measure traffic and improve Spheron. Accept all, reject non-essential, or choose what's on. See our [Privacy Policy](https://www.spheron.network/privacy/).
ManageRejectAccept
