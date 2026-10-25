---
title: 'Air-Gapped LLM: Models, Hardware & Serving | VDF AI'
id: air-gapped-llm-models-hardware-serving-vdf-ai
tags:
- onprem-llm-inference-stacks-859099
- air-gapped
- vendor-marketing
- reliability-low-medium
created: '2026-10-23T17:22:27.248323Z'
updated: '2026-10-23T17:25:07.889100Z'
source: https://vdf.ai/air-gapped-llm/
source_domain: vdf.ai
fetched_at: '2026-10-23T17:22:27.225651Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'VDF AI (vendor, reliability low-medium) air-gapped reference stack: vLLM
  (CUDA stack pinned in bundle) or llama.cpp, checksummed offline registry, local
  router, in-enclave Prometheus/Grafana; sizing tiers (~50 users on one 48GB GPU;
  200-1000 users routed fleet; N+1 redundant enclave); key insight that model refresh
  (transfer window possibly 3 months) is the neglected cost and click-through licences
  cannot be accepted offline. Claims unsourced.'
---

[ New VDF AI Enterprise is now available on AWS Marketplace → ](https://aws.amazon.com/marketplace/pp/prodview-udrq52ay4omew)
Air-Gapped Deployment
# Air-Gapped LLM
An enterprise LLM deployment is the infrastructure for running large language models — open-weight models like Llama, Mistral, and Qwen served through engines like vLLM and Ollama — as a production service for your organization, operating on a network with no connection to the public internet — models, updates, and telemetry all move by controlled offline transfer, so the system functions fully inside a classified or isolated enclave.
[ Request a Demo ](https://vdf.ai/demo/) [See the reference architecture](https://vdf.ai/on-prem-ai-reference-architecture/)
Air-Gapped deployment No external calls
Your boundary
Model servingOpen-weight LLMs on your GPUs
Private retrievalRAG over your own corpus
Governance & auditApprovals, policy, immutable logs
Access & identitySSO, RBAC, per-user scoping
No outbound call of any kind; models and updates arrive as signed offline bundles. 
40–60%cost cut from model routing
10×cheaper small-model inference for routine tasks
0tokens leaving your perimeter
9–18 motypical hardware payback at volume
Built for regulated deployment Deploys on your infrastructureNo data leaves your boundaryFull audit trailSSO & role-based access
“ The air-gapped llm decision
Running LLMs air-gapped is now routine defense practice: weights arrive as signed bundles, inference runs on enclave GPUs, and nothing ever calls out. The overlooked cost is model refresh — without a controlled offline update pipeline, enclaves end up running year-old models. Treat model logistics as seriously as the initial deployment and capability stays current.
Serving a large language model with no internet access is, mechanically, the easiest air-gapped workload: weights are static files, and an inference engine is a process that reads them. Everything difficult about it is upstream — getting the weights in, proving what they are, and deciding what happens when a better model is released and your transfer window is three months away.
That last point reshapes model strategy. In a connected environment, you follow the frontier: swap the endpoint, keep the API. Inside the gap, each model generation is a procurement and accreditation event, so the question is not "which model is best today" but "which model family can we support for a year, at a size our enclave GPUs can actually hold". Teams consistently over-select on parameter count and under-select on quantization tooling and context-length headroom.
The other thing that changes is licensing hygiene. Open-weight does not mean unrestricted, and an offline enclave has no way to accept a click-through licence at runtime. Model licences, their acceptable-use terms, and any third-party dependencies have to be reviewed before the bundle is built, because there is no way to correct that decision afterwards without another transfer.
Air-Gapped by design
## Why teams run their LLM deployment air-gapped
Built for defense, intelligence, critical-infrastructure and classified-environment teams.
### Zero external connectivity, by design
An air-gapped LLM deployment makes no outbound calls — no license pings, no telemetry, no model API fallbacks. If a component phones home, it fails certification; the architecture must assume the internet does not exist.
### Built for classified and SCIF environments
Defense, intelligence, and critical-infrastructure operators need AI capability where cloud AI is categorically prohibited. The LLM deployment runs entirely on enclave hardware and clears accreditation reviews because there is nothing external to assess.
### Controlled update path
Models, embeddings, and software updates arrive as signed offline bundles through your cross-domain transfer process — the same discipline you already apply to any software entering the enclave.
What it does
## Core capabilities of an enterprise LLM deployment
### Open-weight model serving
Serve Llama, Mistral, Qwen, and domain models on your own GPUs with vLLM-class throughput — models you possess, not endpoints you rent.
### LLM routing
Route each request to the cheapest capable model instead of sending everything to the largest one — the single biggest lever on inference cost.
### Fine-tuning on your data
Adapt open-weight models to your terminology and tasks with data that never leaves your environment.
### Evaluation and benchmarking
Measure model quality on your actual workloads with audit-grade reports before and after every model change.
Architecture
## What an air-gapped deployment changes
Everything ships as a self-contained bundle: container images, model weights, embedding models, and documentation must install from local media with no registry or CDN access.
Local models only: the LLM deployment serves open-weight models on enclave GPUs; there is no cloud fallback tier, so model selection and routing happen entirely inside the gap.
Audit evidence must be exportable on your terms — logs stay in the enclave and leave only through your controlled review process.
Reference stack
## The air-gapped LLM deployment stack
Air-gapped LLM, layer by layer — with the reason each choice holds up under this deployment mode.  
| Layer  | Typical choice  | Why, here  |  
| --- | --- | --- |  
| Inference engine  | **vLLM for throughput; llama.cpp where GPU supply is constrained**  | Both run fully offline once images and weights are local. vLLM needs its CUDA stack pinned in the bundle — a version mismatch surfaces only inside the enclave, where you cannot pull a fix.  |  
| Model weights  | **Open-weight families in safetensors, plus a quantized fallback**  | Carry a smaller quantized variant in the same bundle. If the enclave gets fewer GPUs than planned, that is the difference between degraded service and no service.  |  
| Model registry  | **Offline object store or registry holding checksummed weights**  | Gives accreditors a verifiable answer to "which exact model is running", which is much harder to evidence when weights are copied around by hand.  |  
| Routing  | **A local router in front of the served models**  | With no cloud tier to escalate to, routing is how you protect scarce large-model capacity: cheap requests go to the small model or they consume the budget for the ones that matter.  |  
| Observability  | **In-enclave Prometheus and Grafana, no external exporters**  | Default telemetry in many serving stacks phones home. Disabling it explicitly, and proving it, is part of the accreditation package.  |  
Capacity planning
## Sizing an air-gapped LLM deployment  
| Profile  | Scale  | Hardware  | What actually binds  |  
| --- | --- | --- | --- |  
| Single-model enclave  | ~50 concurrent users, one mid-size model  | One 48 GB GPU, or two 24 GB cards with tensor parallelism  | KV cache, not weights, decides real concurrency. Long-context use collapses user count faster than model size does.  |  
| Routed fleet  | 200–1,000 users, mixed task difficulty  | 1 small model resident plus 1–2 large-model cards  | Keeping the small model always resident matters more than peak large-model capacity — it absorbs most traffic.  |  
| Redundant enclave  | Mission-critical, no tolerance for an outage  | N+1 GPU nodes with identical bundle versions  | You cannot fail over to a cloud region. Spare capacity has to physically exist inside the gap.  |  
Compliance drivers
## Regulations that point to air-gapped
### Classified handling
The LLM deployment operates inside SCIF and enclave boundaries; there is nothing to accredit outside them.
### ITAR / export control
Technical data entering the LLM deployment never transits foreign-controlled infrastructure.
### NIS2 / NERC CIP
Critical-infrastructure isolation is met structurally by the LLM deployment, not contractually.
### Zero-trust postures
No third-party endpoints to allow-list — the LLM deployment’s attack surface is your own network.
Honest fit check
## When air-gapped is the right call — and when it isn’t
### Choose air-gapped when
  * The network the LLM deployment must serve is already isolated — classified programs, OT networks, offline research enclaves.
  * Policy prohibits any external AI API, including via proxy or private link.
  * You need AI capability in disconnected field or vessel environments with intermittent or no connectivity.


### Consider another mode when
  * You can tolerate controlled outbound connectivity → an on-premises LLM deployment is simpler to operate and update.
  * Your requirement is legal jurisdiction rather than physical isolation → the sovereign variant fits; air-gapping an LLM deployment is stricter than most regulators ask.


Same capability, different deployment mode:
[On-Premises LLM](https://vdf.ai/on-premises-llm/ "On-Premises LLM — for data-center control and hardware economics")[Self-Hosted LLM](https://vdf.ai/self-hosted-llm/ "Self-Hosted LLM — for stack control without vendor lock-in")[Sovereign LLM](https://vdf.ai/sovereign-llm/ "Sovereign LLM — for jurisdictional and regulatory control")[Private LLM](https://vdf.ai/private-llm/ "Private LLM — for confidentiality without the infrastructure program")
Deployment modes compared
## LLM: Air-Gapped vs the alternatives  
| Deployment mode  | Typical owner  | What you gain — and give up  |  
| --- | --- | --- |  
|  Air-Gapped _(this page)_  | CISO / Classified Program Lead  | Structural security no contract can match — in exchange for moving every model, index, and software update through an offline bundle process.  |  
| CTO / Head of Infrastructure  | Maximum physical control and the strongest economics at steady volume — in exchange for owning the hardware, the capacity plan, and the upgrade cycle.  |  
| Platform Engineering Lead  | Complete stack and model freedom with no usage meter — in exchange for your team owning operations, CVE response, and the upgrade cadence.  |  
| CIO / Chief Data Officer (public sector & regulated EU)  | Legal control that survives foreign disclosure orders and sanctions — in exchange for in-country hosting constraints and heavier procurement diligence.  |  
| CISO / Data Protection Officer  | The fastest route to confidential AI — in exchange for a boundary defined by tenancy and contract rather than by a building you own.  |  
Rollout
## How to deploy an air-gapped LLM deployment
  1. 01
### Select the model family before the hardware
Pick the open-weight family and size you can support for at least a year, then size GPUs to hold it with context headroom. Doing this in the other order produces enclaves that cannot run the model the mission actually needs.
  2. 02
### Clear licences and provenance while still connected
Review model licences, acceptable-use terms, and dependency licences before the bundle is sealed. Record checksums for every weight file — inside the gap this is the only provenance evidence you will have.
  3. 03
### Build a self-contained serving bundle
Package the engine image, pinned CUDA or ROCm stack, weights, tokenizer, and a quantized fallback together. Validate on a disconnected network that startup makes no outbound request, including telemetry and licence checks.
  4. 04
### Establish an in-enclave benchmark
Measure tokens per second and time-to-first-token at realistic concurrency on the actual hardware. Vendor benchmarks assume configurations you will not have, and this is your only baseline for later comparisons.
  5. 05
### Define the model refresh cycle
Agree in advance how often a new model generation is evaluated, bundled, and transferred, and how the previous version is retained for rollback. Without a cycle, enclaves silently freeze on their first model for years.


Failure modes
## Where air-gapped LLM deployment projects fail
### A dependency that phones home on startup
A tokenizer download, a licence ping, or a telemetry exporter that works fine in staging blocks first boot inside the enclave. Test on a genuinely disconnected network, not one that merely has a firewall rule.
### Sizing for weights instead of KV cache
A model that fits in VRAM at rest can still fail at ten concurrent long-context users. Concurrency and context length, not parameter count, determine whether the deployment is usable.
### No rollback bundle
A model upgrade that degrades quality on enclave-specific tasks is unrecoverable if the prior weights were deleted to reclaim storage. Retain the previous version until the new one has been validated in production.
Buyer checklist
## How to evaluate an air-gapped LLM deployment
1
Which open-weight models does the stack serve today, and how fast can you adopt new ones?
2
Is there a routing layer, or does every request pay flagship-model prices?
3
What GPU footprint does your workload actually need once routing and quantization are applied?
4
How are model updates tested — is there an evaluation harness with your data?
5
Can inference logs feed your observability and audit stack?
Air-gapped deployments trade update convenience for structural security; budget for the offline bundle process, but the LLM deployment itself prices like any fixed in-enclave infrastructure — no per-token exposure and no consumption bill after the fact.
How VDF AI delivers it
## An air-gapped LLM deployment, on the VDF AI platform
VDF AI ships the serving, routing, fine-tuning, and evaluation layers as one platform — the Self-Evolving Model Router picks the cheapest capable model per request, on your hardware.
[VDF AI Router](https://vdf.ai/products/vdf-ai-router/)[Model Fine-Tuning](https://vdf.ai/products/model-fine-tuning/)[Model Evaluation Suite](https://vdf.ai/products/model-evaluation-suite/) [Trust Center](https://vdf.ai/trust-center/)
FAQ
## Air-Gapped LLM questions, answered
What is an air-gapped LLM deployment?
An enterprise LLM deployment is the infrastructure for running large language models — open-weight models like Llama, Mistral, and Qwen served through engines like vLLM and Ollama — as a production service for your organization, operating on a network with no connection to the public internet — models, updates, and telemetry all move by controlled offline transfer, so the system functions fully inside a classified or isolated enclave.
Why do enterprises choose an air-gapped LLM deployment over a cloud service?
An air-gapped LLM deployment makes no outbound calls — no license pings, no telemetry, no model API fallbacks. If a component phones home, it fails certification; the architecture must assume the internet does not exist. Air-gapped deployments trade update convenience for structural security; budget for the offline bundle process, but the LLM deployment itself prices like any fixed in-enclave infrastructure — no per-token exposure and no consumption bill after the fact.
Which regulations drive air-gapped LLM deployment adoption?
The most common drivers are Classified handling, ITAR / export control, NIS2 / NERC CIP, Zero-trust postures. Classified handling: The LLM deployment operates inside SCIF and enclave boundaries; there is nothing to accredit outside them.
Can VDF AI run as an air-gapped LLM deployment?
Yes. VDF AI ships the serving, routing, fine-tuning, and evaluation layers as one platform — the Self-Evolving Model Router picks the cheapest capable model per request, on your hardware. VDF AI ships the serving engine, routing layer, and model weights as one signed offline bundle, with a quantized fallback included so the deployment degrades gracefully rather than failing when enclave GPU supply is tighter than planned.
Can you run an LLM with no internet connection?
Yes. Once the weights and inference engine are on local disk, generation needs no network at all — the model is a file and the engine is a local process. The internet is only involved in acquiring models and dependencies, which in an air-gapped deployment becomes a signed offline bundle transferred through your existing cross-domain process.
Which open-weight models are suitable for an air-gapped deployment?
Any model distributed as downloadable weights under a licence your legal team accepts, and small enough for your enclave GPUs with context headroom. The selection criteria that matter offline differ from the usual ones: licence clarity, availability of quantized variants, and stability of the family across releases outweigh a few points on a public benchmark.
How do you update an LLM in an air-gapped environment?
Bundle the new weights, engine image, and pinned dependencies, verify checksums, transfer through your controlled process, then deploy alongside the existing model and cut over after benchmarking. Keep the previous version until the new one is proven — there is no fast path to re-acquire it.
How much GPU memory does an air-gapped LLM deployment need?
Enough for the weights plus the KV cache at your target concurrency, which is usually the larger of the two at scale. A quantized mid-size model can serve a small team on a single 48 GB card, while the same model at high concurrency with long documents can saturate several. Benchmark on your own hardware, since you cannot add capacity elastically later.
Keep exploring
## Related guides and resources
[LLM routing explained](https://vdf.ai/resources/llm-routing/)[On-premise LLM cost comparison 2026](https://vdf.ai/resources/on-premise-llm-cost-comparison-2026/)[Where vLLM fits enterprise LLM infrastructure](https://vdf.ai/resources/where-vllm-fits-enterprise-llm-infrastructure/)[Local AI with Ollama, vLLM, llama.cpp](https://vdf.ai/playbooks/local-ai-enterprise-ollama-vllm-llamacpp/) [All deployment guides](https://vdf.ai/private-ai/)
AI Cost & Energy
## Calculate your AI infrastructure savings
Model the cost and energy impact of running AI on-prem versus cloud-only — then see the benchmark data behind the numbers.
[ Calculate AI Infrastructure Savings ](https://vdf.ai/ai-savings-calculator/) [ Read the energy white paper ](https://vdf.ai/white-papers/how-we-reduce-energy-consumption/)
[ Or start free — no credit card → ](https://app.vdf.ai/)
### Request a Demo
×
#### Thank You!
Your demo request has been sent successfully. We'll get back to you soon to schedule your personalized demo!
Discover how VDF AI can transform your organization with our AI-driven solutions. Fill out the form below and we'll schedule a personalized demo for you. 
