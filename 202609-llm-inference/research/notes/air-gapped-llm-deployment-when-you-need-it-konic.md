---
title: 'Air-Gapped LLM Deployment: When You Need It | Konic'
id: air-gapped-llm-deployment-when-you-need-it-konic
tags:
- onprem-llm-inference-stacks-859099
- air-gapped
- vendor-marketing
- reliability-low
created: '2026-10-23T17:22:20.600808Z'
updated: '2026-10-23T17:25:07.649844Z'
source: https://konic.io/blog/air-gapped-llm
source_domain: konic.io
fetched_at: '2026-10-23T17:22:20.576188Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Konic Labs (model vendor, reliability low) conceptual explainer: air-gapped
  = no route; weights as versioned checksummed artifact imports; example docker run
  --network none --offline. Pushes its own ''Tres-1'' artifact; no deployment evidence
  or numbers.'
---

TL;DR
An air-gapped LLM runs on infrastructure with no connection to the public internet — the model, the data, and the serving stack all live inside your boundary. This guide explains what air-gapped deployment actually requires, which industries need it, and the practical constraints of running a model with no external dependencies.


## What air-gapped actually means
**What is air-gapped LLM deployment?** It is a language model deployment that is physically or logically isolated with no inbound or outbound network route. The model, data, and serving runtime all live inside your boundary and cannot call out to the internet.
An air-gapped deployment runs a model on infrastructure that has no route to the public internet. The model weights, the data, the serving runtime, and the monitoring all live inside your network boundary. Nothing leaves, and nothing needs to come in.
This is the strictest form of data control. It is not the same as a private cloud VPC, which is isolated from other tenants but still connected to the internet. Air-gapped means physically or logically disconnected — the model must be fully self-contained.
In practice that means every dependency is brought in deliberately. Weights are delivered as a versioned artifact you load from disk, the runtime is installed from an internal registry, and even evaluation and model upgrades happen by importing a new artifact rather than pulling from the internet. If the system cannot run with the network cable unplugged, it is not air-gapped.
## Who needs it
Air-gapped deployment is driven by regulation, national-security requirements, and the most sensitive commercial data. The industries that most often require it:
  * **Defense and government** — where data classification and network isolation are non-negotiable.
  * **Banking and insurance** — where customer data and model behavior are subject to strict audit and jurisdiction rules.
  * **Healthcare** — where patient data carries legal obligations that a hosted API cannot satisfy.
  * **Manufacturing and energy** — where operational data and control systems must stay off the public network.


If your compliance posture requires data to stay in a jurisdiction or on a specific network, air-gapped is the architecture that makes it possible.
The common thread is that the cost of a boundary breach exceeds the cost of self-operation. When a single prompt may contain classified, patient-level, or customer-account context that cannot leave the facility, even an encrypted API call is a policy violation. Air-gap removes the network path entirely, so the policy is enforced by architecture rather than by audit trail alone.
For those teams, the air-gap is not a preference for privacy in the abstract — it is a concrete requirement that the data plane never leaves the building. The model must therefore be capable enough to complete the task without calling a larger hosted model for help, which is why model quality under isolation matters more, not less.
## What air-gapped deployment requires
Running a model with no external dependencies changes the requirements in three concrete ways:
  * **Self-contained weights.** The model must be delivered as an artifact you can load offline — no runtime downloads, no telemetry calls, no dependency on a vendor's infrastructure.
  * **On-prem serving.** The runtime (vLLM, MLX, or llama.cpp) must run entirely on your hardware, with your keys and your data plane.
  * **Offline operations.** Monitoring, evaluation, and upgrades must work without a connection. Versioned releases you can bring in deliberately are essential.


Choosing an artifact that is already built for this posture saves months. For example, [Tres-1](https://konic.io/models/tres-1) is air-gap capable and ships as a self-contained artifact you can load offline with no runtime downloads. It is designed so the serving stack, tokenizer, and weights all live inside your boundary and can be imported as a versioned release without a network call.
The same offline requirement applies to the operating model. You still need logging, latency metrics, and an eval loop — but all of it must run locally. Plan for locally computed evaluation, locally stored traces, and upgrade by importing a new versioned artifact after you have validated it. If you are scoping cost and support for that cycle, see [pricing](https://konic.io/pricing) for how versioned delivery and offline support are structured.
Thinking in imports rather than connections also simplifies audit. Each model version is a discrete bundle you can checksum, stage, and promote through environments the same way you promote any other critical artifact — with approvals and reproducibility, not with a live pull during serving.
## The trade-off: capability vs. isolation
The honest trade-off is that air-gapped deployment rules out the convenience of a hosted frontier model. You cannot call a bigger model for the hard cases, and you cannot rely on the vendor to keep improving it for you. You are committing to a model that runs entirely inside your wall.
That is why the model choice matters so much. For regulated and isolated workloads, the right approach is a model that is high-capability enough to handle the task on its own, delivered as a versioned, self-contained artifact — not a raw checkpoint you have to operationalize yourself.
Isolation also changes how you think about compression. You cannot rely on a fallback API when the small model fails, so the compressed model must hold quality under the same isolation. Our REAP Apple Silicon study (2026-06-29, 96.8% at 25% compression) — [REAP expert pruning on Apple Silicon](https://konic.io/research/reap-expert-pruning-apple-silicon) — is instructive here: REAP retained 96.8% quality at 25% compression on MoE models for Apple Silicon, showing that a well-chosen pruning recipe can shrink the footprint for edge and on-device air-gap scenarios without giving up the capability you committed to keep inside the wall.
Measure isolation and capability together. An air-gapped model that is 30% smaller but drops below your accuracy bar is not cheaper — it is a failed deployment. The right metric is capability per isolated gigabyte: task success inside the boundary per unit of VRAM you provision for it.
## When air-gapped is the right call
Choose air-gapped deployment when the cost of a data or network boundary breach exceeds the cost of running your own model. For most enterprises that threshold is crossed by regulation or by the sensitivity of the data itself. If you are not sure whether you need it, you probably do not — but if your compliance team is asking, the answer is usually yes.
Use the requirements matrix in the next section to make the decision explicit. If your workload must show no inbound or outbound route, self-contained weights, and offline operations, you are in air-gap territory. If tenant isolation with internet routing satisfies your audit, a private cloud VPC may be sufficient. If data can leave the boundary under contract, a hosted API is the simplest path. Naming the requirement before choosing the architecture keeps you from over- or under-isolating.
When isolation is required, start with a model that is already packaged for it and validate on your data inside the boundary. A single offline eval on your real prompts tells you more than any external benchmark about whether the isolation trade is worth it.
### Isolation requirements at a glance
This matrix isolates what changes when you move from a hosted API to a private cloud and then to a true air-gap. Use it to align engineering and compliance on the same terms before you commit to hardware or a model artifact.  
| Requirement  | Air-gapped  | Private cloud  | API  |  
| --- | --- | --- | --- |  
| Network isolation  | Fully isolated — no inbound/outbound  | VPC isolated, internet-routed  | Provider-hosted, internet-called  |  
| Data plane  | Stays inside your boundary  | Tenant-isolated, egress possible  | Leaves boundary to vendor  |  
| Model weights  | Self-contained offline artifact  | Cloud-hosted, pullable  | Vendor-hosted  |  
| Operations & upgrades  | Offline, versioned imports  | Connected ops, rolling pulls  | Vendor-managed  |  
Air-gapped requires deliberate imports; private cloud isolates tenants but remains network-connected; API leaves the boundary by design.
The practical test is whether the system functions with external networking disabled. If monitoring, evaluation, and inference all continue with no outside route, you have met the air-gap bar. If any of them require a pull, callback, or telemetry endpoint, you are in private-cloud posture, not air-gap.
### Running inference with no network route
Air-gapped inference must run with networking disabled at the container boundary. The example below starts a self-contained Tres-1 artifact with the network namespace removed, mounts the model read-only from your internal store, and serves it on your data plane only:

```
docker run --network none --gpus all \
  -v /models/tres-1:/models/tres-1:ro \
  -v /data/prompts:/data:ro \
  konic/tres-1-serve:latest \
  --model /models/tres-1 \
  --offline \
  --port 8000
```

`--network none` is the enforcement; `--offline` tells the runtime to refuse any attempt to fetch weights, tokenizer extensions, or telemetry endpoints. Upgrade is a new image and a new `/models/tres-1` import — never a live pull inside the container.
## Frequently asked questions
**How is air-gapped different from a private cloud VPC?**
A private cloud VPC isolates your workload from other tenants but remains connected to the internet. You can pull images, call external APIs, and reach vendor endpoints over the network — traffic is gated, but the route exists. An air-gapped deployment has no inbound or outbound route at all. The model, runtime, data, and evaluation all live inside your network boundary and cannot call out. The matrix above makes the split explicit: VPC gives you tenancy isolation with internet routing; air-gap gives you physical or logical disconnection. If your requirement says no data or weight fetch may traverse the public network even in principle, you need the latter.
**How is air-gapped different from on-prem?**
On-prem means the model runs on infrastructure you own — your servers, your network, your keys. Air-gapped is the strictest form of on-prem: on-prem with the network cable conceptually unplugged. All on-prem deployments give you control of the data plane; only air-gapped deployments guarantee that the data plane has no route out. In practice that adds three constraints beyond standard on-prem: weights must be self-contained offline artifacts, the serving runtime must have no external dependencies, and every operational task — logging, metrics, evaluation, and upgrades — must work without a connection. If you can satisfy your requirements with on-prem that still pulls a container image over the internet, you do not need air-gap; if not, you do.
**Which industries need air-gapped LLMs?**
Defense and government lead the list, where classification and network isolation are non-negotiable, but the same requirement appears in banking and insurance where customer data and model behavior are under audit, in healthcare where patient data carries legal obligations that a hosted API cannot meet, and in manufacturing and energy where operational data and control systems must stay off the public network. The pattern is not the sector but the data: when a prompt, document, or trace cannot leave a jurisdiction or a facility under any contract, the only architecture that satisfies the policy by construction is one with no network path out.
Author
Written by — founders and research team at Konic Labs, Inc. Open, reproducible research for on-prem enterprise LLMs.
[GitHub](https://github.com/konic-labs)[Hugging Face](https://huggingface.co/konic-labs)[LinkedIn](https://www.linkedin.com/company/koniclabs)[X](https://x.com/koniclabs)·[Book a demo →](https://calendar.app.google/BA7ADCjsyesnHL1Z8)
## More guides
[Sep 8, 2026 LLM Data Sovereignty: Why Enterprises Keep Models In-House Data sovereignty is the reason most regulated enterprises cannot use hosted LLM APIs: the moment proprietary data crosses a network boundary, control over it is shared. This guide explains what data sovereignty means for AI, why it is driving on-prem adoption, and how to evaluate a deployment against your sovereignty requirements. Read guide](https://konic.io/blog/llm-data-sovereignty)[Sep 3, 2026 How to Deploy an LLM On-Premise: A Practical Guide Deploying an LLM on your own infrastructure is a sequence of concrete decisions: pick the model, size the hardware, choose a serving runtime, quantize for your GPU, and wire in monitoring. This guide walks each step with the trade-offs that actually matter for a production on-prem deployment. Read guide](https://konic.io/blog/deploy-llm-on-premise)[Sep 1, 2026 On-Prem LLM vs API: When to Self-Host Your Language Models API-based LLMs are fast to adopt but scale poorly on cost, latency, and data control. On-prem LLMs trade setup effort for predictable economics, lower per-token cost at volume, and data that never leaves your boundary. This guide breaks down the decision across cost, latency, privacy, compliance, and operational effort — and when each path is the right call. Read guide](https://konic.io/blog/on-prem-llm-vs-api)
## Related research
[Aug 13, 2026 Optimized LFM2.5-VL-3B: FFN Width Pruned, Distillation Aligned, INT4 Quantized We surgically compress Liquid AI’s LFM2.5-VL-3B — an architecture-searched hybrid conv+attention vision-language model — by cutting FFN width where the search never optimized it: joint-SwiGLU Wanda pruning (10752→8192 and →7168), distillation-to-baseline LoRA recovery, and hand-rolled W4A16 GPTQ + W8 mixed-precision quantization. All three tiers are near-lossless against the base on our paired eval shards — PPL ratios at or below baseline (0.79–0.85), top-5 token agreement ≥ 0.95, quantization adding only +0.02–0.06 nats — across 5.30 GB (−15%), 4.92 GB (−21%), and 1.93 GB packed / 2.22 GB (Konic Optimized) — native INT4/INT8 compressed-tensors in vLLM (−69%). Live comparison on the smallest tier preserves scenes, facts, and tool calling — 10/10 tool rounds, including correct abstention where the base over-triggered. Recorded runs, not general benchmarks. Read article](https://konic.io/research/lfm2.5-vl-3b-optimized)[Aug 12, 2026 LFM2.5 Encoder 230M + SigLIP2: A Compact Multimodal Encoder We augment Liquid AI’s LFM2.5 Encoder 230M — a 230M-parameter bidirectional masked-language encoder — with a SigLIP2 vision tower and 32 learned soft tokens to build an encoder-only multimodal model for retrieval and image-text matching. Clean image-only retrieval on 12,500 held-out MONET pairs reaches 0.1194 image→text R@1 with the BF16 reference; the GPTQ INT4 release retains 0.1091 while shrinking the package from 923.65 MB to 370.46 MB (−59.9%). Cyclic-negative matching AUROC is 0.9733; text-nearest hard negatives and image-conditioned masked-token prediction remain at chance, and the model does not generate. Read article](https://konic.io/research/lfm2.5-multimodal-encoder-230m)[Jul 20, 2026 Two Stages, Much Smaller MoE: REAP Expert Pruning Followed by AWQ INT4 Quantization We chain two complementary compression stages on Liquid LFM2.5-8B-A1B — REAP CUDA expert pruning (32→16 experts across 22 MoE layers) then external AWQ INT4 quantization (W4A16_ASYM, group 128). The published packed artifact is 1.15B packed-weight equivalent / 2.79 GB; recorded MATH500 and BFCLv3 results use a separate 9.18 GB AWQ-scaled BF16 derivative for vLLM evaluation, not direct packed-INT4 runtime. These are recorded run summaries, not general benchmarks. Read article](https://konic.io/research/reap-awq-moe-compression)
