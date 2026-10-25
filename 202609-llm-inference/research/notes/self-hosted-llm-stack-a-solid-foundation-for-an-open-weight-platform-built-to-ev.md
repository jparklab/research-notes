---
title: 'Self-hosted LLM stack: a solid foundation for an open-weight platform built
  to evolve | Ogenki'
id: self-hosted-llm-stack-a-solid-foundation-for-an-open-weight-platform-built-to-ev
tags:
- inference-stack
- vllm
- envoy-ai-gateway
- semantic-router
created: '2026-10-09T22:53:42.587379Z'
updated: '2026-10-10T14:52:17.000206Z'
source: https://blog.ogenki.io/post/series/agentic_ai/llm-self-hosted-stack/
source_domain: blog.ogenki.io
fetched_at: '2026-10-09T22:53:42.565250Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Ogenki blog (personal/demo project, May 2026, 4739 words, primary first-person
  account, not a named company''s internal production service but an unusually detailed
  reference architecture) documenting a full self-hosted open-weight LLM stack on
  Kubernetes (AWS EKS / GCP GKE). Three layers: (1) Storage -- model weights (HuggingFace)
  cached on a shared Amazon S3 Files (POSIX NFS, GA April 2026) volume so only the
  first pod cold-starts a download; (2) Inference -- vLLM (open-source, ''de-facto
  standard'') deployed via the vLLM Production Stack Helm chart, with fp8 quantization,
  prefix caching, chunked prefill, and Hermes tool-call parsing enabled per-model;
  GPU autoscaling via KEDA on vLLM''s own Prometheus metrics (vllm:num_requests_running
  / configured batch size at 70% threshold, vllm:gpu_cache_usage_perc at 60%) rather
  than CPU/memory HPA, anticipating saturation before a queue forms; node provisioning
  via Karpenter; (3) Access -- Envoy AI Gateway (open-source, built on Envoy Gateway)
  as the single OpenAI-compatible front door, doing per-model routing, bearer-token
  auth, and per-token cost tracking via OpenTelemetry Gen AI semantic conventions
  (gen_ai.* metrics), plus vLLM Semantic Router (''Iris'', open-source, a ~100M-param
  mmBERT-derived CPU classifier) for intent/jailbreak/PII-based dynamic model selection
  when a client doesn''t pick a model explicitly (adds ~250-300ms, acceptable for
  chat, not for <200ms p95 IDE autocomplete, which uses explicit routing instead).
  Kubernetes resource generation is abstracted via a custom Crossplane ''InferenceService''
  composition (written in KCL, unit-tested, OCI-packaged) so swapping models is a
  2-line PR. Clients are open-source: OpenWebUI (chat), Continue (VSCode FIM autocomplete),
  OpenCode (CLI coding agent, an open alternative to Claude Code). Continuous quality
  evaluation via Promptfoo (deterministic + LLM-as-judge assertions) run as a nightly
  CronJob, metrics pushed to the same Prometheus/Grafana stack. States vLLM delivers
  3-10x throughput over naive serving (HuggingFace Transformers/Ollama defaults) via
  continuous batching + paged attention, citing Kwon et al. SOSP 2023 (arXiv:2309.06180)
  for the original 2-4x vs FasterTransformer / 14-24x vs HF Transformers figures.
  Explicitly states open-weight models only become compelling for orgs with real data-sovereignty
  requirements (healthcare, defense, finance) or once hardware/financial accessibility
  improves; for the author''s own usage the proprietary Claude ecosystem remains cheaper
  than replicating equivalent quality self-hosted.'
utility_score: 16.0
---

# Self-hosted LLM stack: a solid foundation for an open-weight platform built to evolve
calendar 13-05-2026 · 22 min read ·
Share on: [bluesky ](https://bsky.app/intent/compose?text=Self-hosted%20LLM%20stack%3a%20a%20solid%20foundation%20for%20an%20open-weight%20platform%20built%20to%20evolve%20https%3a%2f%2fblog.ogenki.io%2fpost%2fseries%2fagentic_ai%2fllm-self-hosted-stack%2f "Share on Bluesky")[facebook ](https://www.facebook.com/sharer.php?u=https%3a%2f%2fblog.ogenki.io%2fpost%2fseries%2fagentic_ai%2fllm-self-hosted-stack%2f&t=Self-hosted%20LLM%20stack%3a%20a%20solid%20foundation%20for%20an%20open-weight%20platform%20built%20to%20evolve "Share on Facebook")[linkedin ](https://blog.ogenki.io/post/series/agentic_ai/llm-self-hosted-stack/#linkedinshare "Share on LinkedIn")[copy](https://blog.ogenki.io/post/series/agentic_ai/llm-self-hosted-stack/ "Copy Link")
## Overview
Agentic AI Series — Part 3
This article follows [Agentic Coding: concepts and hands-on use cases](https://blog.ogenki.io/post/series/agentic_ai/ai-coding-agent/) (the fundamentals of coding agents) and [A few months with Claude Code](https://blog.ogenki.io/post/series/agentic_ai/ai-coding-tips/) (daily tips and lessons learned). **Here, we take a step back** : what can we reasonably do self-hosted today?
I now use agentic coding on a daily basis, like many of us. I'm overall happy with my Claude Code experience, but I keep a close eye on the **open-weight ecosystem** , which is also rapidly evolving: Qwen, Llama, Mistral, GLM, DeepSeek — and on the fierce competition between the various players in the space.
So I set myself a challenge: **how could we host our own LLMs in our own infrastructure?** Data sovereignty, model choice, vendor independence, and above all — the technical curiosity to see what's _really_ feasible today with the modern tools at our disposal.
This article is a demonstration
The models deployed here are **7-8B parameters** , served on a **L4 24GB GPU**. On complex agentic tasks, they fall **well short of** a Sonnet 4.6 or an Opus 4.7 — they hallucinate more, loop on errors where a _frontier_ model breezes through.
The open-weight models that could _truly_ compete (DeepSeek V4 Pro, Kimi K2.6) require hardware that's simply out of reach for personal use.
This article aims to **lay the foundations** of an alternative meant to evolve as the ecosystem catches up, and demonstrates what's _already_ possible today — even with modest models.
## 🎯 Goals 
  * Understand the **building blocks of a modern self-hosted LLM stack** on Kubernetes
  * Discover the technical choices made: open-weight models, **vLLM Production Stack** , smart routing, autoscaling on GPU signals
  * See how this platform is consumed from three clients: **OpenWebUI** , **Continue VSCode** , **OpenCode CLI**
  * Honestly evaluate what works, what doesn't, and what it really costs


The reference repo
The whole stack is deployed via GitOps from [**cloud-native-ref**](https://cnref.ogenki.io) ([source on GitHub](https://github.com/Smana/cloud-native-ref)) — beyond the LLM layer, the project illustrates **a complete cloud-native ecosystem** , running on both AWS EKS and GCP GKE:
  * **GitOps** — Flux, continuous reconciliation
  * **Platform engineering** — Crossplane (one YAML claim generates all the Kubernetes plumbing)
  * **Observability** — VictoriaMetrics, VictoriaLogs, Grafana
  * **Security & access** — secret management + private exposure via Tailscale


## 🗺️ Stack overview 
Before diving into the heart of the matter, here's a bird's-eye view:
The stack is organized into **three layers** , which we'll walk through from the foundation up to the front door:
  1. **Storage** — model weights pooled on a shared volume.
  2. **Inference** — a _fleet_ of **vLLM** pods serving models on GPU.
  3. **Access** — the **Envoy AI Gateway** at the front, and the **vLLM Semantic Router** that dynamically picks the model when needed.


The whole thing is driven by **GitOps** (Flux reconciles everything from [`cloud-native-ref`](https://github.com/Smana/cloud-native-ref)) and exposed privately via **Tailscale** — no data leaves the infrastructure. Each layer is detailed in the sections that follow.
## 🔌 The centerpiece: the `InferenceService` abstraction 
If you've read some of [my previous articles](https://blog.ogenki.io/post/crossplane_composition_functions/), you know I particularly like `Crossplane` for providing the right abstraction to end users. It's one of my essential components and lets me expose a **simple, fit-for-purpose interface**. I already have a few: `App`, `SQLInstance`, `EPI` — and now `InferenceService`. All of them are packaged and versioned in a dedicated repository: [crossplane-configuration](https://github.com/Smana/crossplane-configuration).
### Declaring a new model 
Concretely, exposing a new LLM on the platform boils down to adding a YAML file like this:

```
 1apiVersion:cloud.ogenki.io/v1alpha1 2kind:InferenceService 3metadata: 4name:xplane-my-new-model 5namespace:llm 7 8repository:Qwen/Qwen3-8B 9revision:<hf-commit-sha>10quantization:fp811contextWindow:3276812toolCallParser:hermes13preload:14enabled:true16count:117routing:18tier:medium19specialty:general20scaling:21minReplicas:122maxReplicas:2
```

yaml
A few seconds after the push, Flux reconciles, the Crossplane composition fires, and **all required Kubernetes resources are applied automatically**.
### What the composition generates for us 
From this _Claim_ , about a dozen Kubernetes resources are created. Beyond the usual suspects (`Service`, `ServiceAccount`, `ExternalSecret`), six deserve a closer look:
  * 🧠 **[vLLM](https://github.com/vllm-project/vllm) (`Deployment`)** — the inference engine: _continuous batching_ + _paged attention_ deliver **3-10× higher** throughput than naive serving on the same GPU. Image `vllm/vllm-openai`, `nodeSelector: gpu-l4`. _([more on vLLM in Layer 2](https://blog.ogenki.io/post/series/agentic_ai/llm-self-hosted-stack/#vllm-production-stack))_
  * 📦 **Preloading`Job`** — downloads the ~15 GB of HuggingFace weights to the S3 Files PVC. **Idempotent** (1× per `repository@revision` pair): vLLM pods (replicas, redeploys) then mount the volume in seconds.
  * 🛡️ **Two`CiliumNetworkPolicy`** (zero-trust): a restrictive policy for the long-lived vLLM pod (outbound DNS + ingress AI Gateway/Iris/vmagent), a more permissive one scoped to the preloading Job (HF, AWS API, EKS Pod Identity Agent). Avoids granting the serving pod the broader permissions only needed during _bootstrap_.
  * ⚡ **KEDA`ScaledObject`** — autoscaling triggered **before** load saturates a pod, rather than reacting to a queue forming. _([the math is detailed in Layer 2](https://blog.ogenki.io/post/series/agentic_ai/llm-self-hosted-stack/#autoscaling-with-keda))_
  * 📊 **`VMServiceScrape`+`VMRule`** — vLLM metrics scraped by **VictoriaMetrics** on `/metrics`, SLOs and alerts (cold-start budget, error rate, latency) _shipped_ alongside the model. _([detailed approach in the Observability series](https://blog.ogenki.io/post/series/observability/metrics/))_
  * 🚪 **`AIGatewayRoute`**— declares how the**Envoy AI Gateway** dispatches traffic to this model: `model: xplane-qwen-coder` in an OpenAI request lands on the right pod, with no application logic.


**To switch from one model to another** , you only change two fields (`model.repository` and `model.revision`). Here's the typical PR to move from the current Qwen2.5-Coder-7B to a Qwen3-Coder-30B the day it fits on the hardware:

```
1# apps/base/ai/llm/qwen-coder.yaml
2spec:
3  model:
4-    repository: Qwen/Qwen2.5-Coder-7B-Instruct
5-    revision: c03e6d358207e414f1eca0bb1891e29f1db0e242
6+    repository: Qwen/Qwen3-Coder-30B-A3B-Instruct-FP8
7+    revision: <matching-hash>
8     quantization: fp8
9     contextWindow: 32768

```

diff
Four lines change. Flux reconciles, KEDA readjusts triggers, Karpenter provisions the right GPU — the rest of the wiring follows.
KCL: version, test, validate a composition
The composition isn't written as YAML patches (unreadable, untestable) but in [**KCL**](https://kcl-lang.io/) via the [function-kcl](https://github.com/crossplane-contrib/function-kcl) — a **typed** configuration language with **native assertions**. Three direct consequences:
  * **Unit tests** — a [`main_test.k`](https://github.com/Smana/crossplane-configuration/blob/main/apis/inferenceservice/kcl/main_test.k) file validates each behavior (`kcl test` runs in CI on every PR).
  * **Versioned OCI packaging** — the composition is published as a Crossplane Configuration package (`ghcr.io/smana/crossplane-configuration-aws:v0.4.1`) from the dedicated [crossplane-configuration](https://github.com/Smana/crossplane-configuration) repository, referenced by immutable tag.
  * **Claim schema validated at the API server** — `kubectl apply` rejects inconsistent claims (e.g. `minReplicas > maxReplicas`) **before** the composition is even triggered.


Versions, tests, schema — not just "YAML we copy-paste".
## 🧠 Layer 1 — Models and their storage 
Now let's walk the three layers of the diagram, starting at the base. Two structural decisions here: **what hardware is financially accessible** (and therefore which models can fit), and **how their weights are stored and loaded on demand**.
### The ecosystem in May 2026 
Over the last few months, open-weight model quality has progressed **dramatically**. On **SWE-bench Verified** , the top two open-weight models are now **DeepSeek V4 Pro (Max)** (80.6%) and **Kimi K2.6** (80.2%) — they hold their own against the proprietary leaders (Claude Opus 4.7 at 87.6%, GPT-5.5 at 88.7%). On the lighter side, **Qwen2.5-Coder** and **Qwen3-Coder** (Alibaba) remain references for code generation.
But between "being able to download the model from HuggingFace" and "running it on **financially accessible** GPUs", there's a world of difference. The best open-weight coding models demand **multi-H100** or **multi-H200** configurations, and even mid-tier models like **Qwen3-Coder-30B-A3B-FP8** require at minimum an **L40S 48GB**.
The limiting factor here isn't the technical solution but **cost**. For this demo, I capped myself at modest models.
What would a DeepSeek V4 Pro or Kimi K2.6 cost?
For an order of magnitude: running a **DeepSeek V4 Pro** (1.6T params, ~49B active in MoE) or a **Kimi K2.6** (~1T total, ~32B active) in FP8 typically demands **8 to 16× H100 80GB**. Running 24/7 on AWS, the monthly bill lands somewhere between **$40,000 and $80,000**. Even on spot or _reserved_ for a year, you stay comfortably above **$15,000/month**.
Personally, that's obviously out of reach. The demo stays on L4 24GB and picks models that fit.
### Weight storage: S3 Files 
Loading ~15GB of weights from HuggingFace at every pod cold-start is slow and expensive (egress bandwidth). The chosen solution: [**Amazon S3 Files**](https://aws.amazon.com/blogs/aws/launching-s3-files-making-s3-buckets-accessible-as-file-systems/), a **brand-new** AWS feature (GA in **April 2026**) exposing an S3 bucket as a **POSIX-compliant NFS filesystem** , mountable as a shared Kubernetes volume across pods.
The benefits in our case:
  * **Shorter cold-start** — a preloading Job downloads the weights from HuggingFace only once, on first deployment; each vLLM pod then mounts the volume directly, starting up in seconds
  * **Native multi-attach** — multiple pods (replicas, distinct models) share the same bucket without special configuration, unlike an EBS PVC
  * **Smart caching** — automatic prefetching, ~1ms latency on the _active set_ (frequently read weights stay on EFS under the hood)
  * **EFS pricing** on active data only, and **S3 standard pricing** (~$0.023/GB/month) on long-term storage — way cheaper than a fixed-size provisioned EBS PVC
  * **Full POSIX** — vLLM reads weights as if from a local disk, no adaptation


The initial fill is still slow
S3 Files isn't magic on the initial _bootstrap_ : the preloading Job has to download the ~15GB of weights from HuggingFace **a first time** into S3. This step is bound by the instance's network bandwidth (~10 Gbps on `g6.xlarge`) and typically takes several tens of seconds. The upfront cost amortizes from there: every **subsequent** start (replicas, redeploys, other models sharing the bucket) mounts the volume in seconds.
## ⚙️ Layer 2 — The inference layer: vLLM on GPU 
Once the model landscape is laid out and the weights are accessible via S3 Files, we still need to **serve them efficiently** — a choice that directly affects latency, throughput, and cost.
### vLLM Production Stack 
[**vLLM**](https://github.com/vllm-project/vllm) is an **open-source inference engine** specialized in serving LLMs on GPU, and has become a de-facto standard. It's the component that actually runs the models in this stack, deployed via its [Production Stack Helm chart](https://github.com/vllm-project/production-stack). The features that matter here:
  * Native **OpenAI-compatible API** — all clients (OpenWebUI, Continue, OpenCode…) talk to vLLM without adaptation.
  * **Mature fp8 support** at the hardware level on L4 / L40S / H100 / H200 — halves VRAM usage.
  * **Hermes parser for function-calling** — required to make the `tools[]` of agentic clients work.
  * **Continuous batching** + **paged attention** — the duo that supercharges GPU throughput: typically **3-10× more** than naive serving like HuggingFace Transformers or Ollama default.


_Continuous batching_ and _paged attention_, in two sentences
**Continuous batching** : a naive serving stack waits for a complete batch of requests before processing it (static batching). vLLM instead **inserts each new request into the in-flight GPU batch** — the GPU never sleeps between requests.
**Paged attention** : a naive serving stack reserves the _KV cache_ (the memory accumulated per generated token, which dominates VRAM) as one contiguous block sized for the max length — a worst-case `malloc`, wasted on short requests. vLLM **pages it just like an OS pages its virtual memory** : fixed-size pages allocated on demand, with sequences of very different lengths cohabiting on the same GPU without fragmentation.
Concretely, each model is served by a vLLM Pod with this kind of config (generated by the Crossplane composition):

```
 1servingEngineSpec: 2modelSpec: 3- name:"xplane-qwen-coder" 4repository:"vllm/vllm-openai" 5modelURL:"Qwen/Qwen2.5-Coder-7B-Instruct" 6requestGPU:1 7vllmConfig: 8enablePrefixCaching:true 9enableChunkedPrefill:true10maxModelLen:3276811dtype:"fp8"12maxNumSeqs:3213extraArgs:["--tool-call-parser","hermes"]
```

yaml
What is quantization (fp8)?
A language model stores its **weights** as decimal numbers. In native precision (**FP16** / **BF16**), each weight takes **16 bits** (2 bytes) — precise, but VRAM-hungry.
**Quantization** consists in representing those same weights on **fewer bits**. We round the values with reduced numerical precision, which **divides memory** usage and **speeds up computation**. The trade-off: the model's outputs (probabilities over tokens) **drift slightly** from those of the unquantized model.
**FP8** halves VRAM and is supported natively (at the hardware level) by recent GPUs: **L4, L40S, H100, H200** — hence its widespread adoption for inference.
One flag in the YAML above deserves an extra word: **`enablePrefixCaching: true`**, crucial for**FIM** (_Fill-In-the-Middle_ , the autocomplete in the IDE). During an autocomplete session, every request the IDE sends contains the same **prefix** (the code around the cursor); only the last few characters change as the developer types. With _prefix caching_ , vLLM **reuses** the KV cache already computed for that shared prefix from one request to the next, instead of recomputing everything — that's what keeps p95 latency under 200 ms during intensive tab-completion.
### Autoscaling with KEDA 
The standard Kubernetes HPA is limited to CPU/memory — signals that don't reflect the **actual** load of a vLLM pod (the limiting factor is VRAM and the engine's internal _batch_ , not host CPU). [**KEDA**](https://keda.sh) solves this by allowing scaling on **any external signal** : Prometheus, queue, business event, etc.
#### Scaling on meaningful indicators 
For each model, the KEDA `ScaledObject` relies on two Prometheus metrics exposed **by vLLM** :
  * `vllm:num_requests_running` — how many requests vLLM is processing in parallel, divided by the configured batch size (`maxNumSeqs`) to measure internal batch saturation.
  * `vllm:gpu_cache_usage_perc` — pressure on the GPU KV cache, which climbs fast with long contexts.



```
 1# Snippet from the Crossplane composition 2triggers: 3- type:prometheus 4metadata: 5query:max(vllm:num_requests_running{model_name="xplane-qwen-coder"}) / scalar(vector(32)) 6threshold:"0.7"# 70% of the batch (maxNumSeqs) occupied 7- type:prometheus 8metadata: 9query:max(vllm:gpu_cache_usage_perc{model_name="xplane-qwen-coder"})10threshold:"0.6"# 60% of the GPU KV cache
```

yaml
The goal is to **anticipate** : these indicators rise _before_ a queue forms on the user side, which lets KEDA add a replica early enough to absorb the surge **without degrading the latency of in-flight requests**. That's the key difference between an autoscaler that _reacts_ (queue grows, latency explodes) and one that _anticipates_.
#### Karpenter takes over on the node side 
KEDA scales **vLLM replicas** , but those replicas need an available GPU to be scheduled. [**Karpenter**](https://karpenter.sh/) handles the layer below: when a pod can't find a free GPU node, Karpenter automatically provisions a GPU-capable instance (~60s cycle); and as soon as a node no longer hosts any GPU pod, it gets decommissioned.
## 🔀 Layer 3 — The access layer: Gateway + routing 
What's left is the **front door** : how a request makes its way to the right vLLM pod, and who's allowed to send one.
### Envoy AI Gateway: the front door 
[**Envoy AI Gateway**](https://github.com/envoyproxy/ai-gateway) is an open-source project built on top of Envoy Gateway, dedicated to managing traffic toward Generative AI services. It acts here as the **single entry point** to the platform. Its main features:
  * **OpenAI API compatibility** — clients hit a single endpoint (`https://llm.priv.cloud.ogenki.io/v1/...`) without knowing about individual vLLM pods.
  * **Per-model routing** — the Gateway extracts the `model` field from the OpenAI request body (or from an `x-ai-eg-model` header) and dispatches to the right Kubernetes `AIServiceBackend` (details in the next sub-section).
  * **Bearer-token authentication** — an Envoy Gateway `SecurityPolicy` checks each request against a list of known tokens.
  * **Per-token cost tracking** — `llmRequestCosts` extracts input/output/total tokens from responses, a useful basis for _rate limiting_ per tenant or per model (wired up but not enabled here).
  * **Multi-provider** — although the stack is 100% self-hosted today, we could tomorrow point a client at OpenAI / Anthropic / Bedrock through the same Gateway, with no client-side code change.


### Two complementary routing modes 
A single model doesn't do everything: **coder for code** , **reasoner for math** , **guard for safety**. The front door combines **two complementary mechanisms** , each used as needed:
  * **Explicit routing** — the client targets a model directly (`model: xplane-qwen-coder`) via the `x-ai-eg-model` header or the request body. dispatches natively, with negligible latency. That's what Continue (autocomplete) and OpenCode do when they know which model to use.
  * **Semantic routing** — the client asks for the virtual model (_Mixture of Models_) and **[vLLM Semantic Router (Iris)](https://github.com/vllm-project/semantic-router)** analyzes the prompt to pick the right actual model (coder, reasoner, guard). Detailed in the next sub-section.


With this setup, a client that knows what it wants incurs no extra latency, and a generic client (OpenWebUI) benefits from automatic smart routing.
### Iris and semantic routing 
[**Iris (vLLM Semantic Router)**](https://github.com/vllm-project/semantic-router) is the open-source project that implements the **_Mixture of Models_** (`MoM`) logic. Deployed as a **sidecar** next to the AI Gateway, it intercepts requests addressed to the virtual `MoM` model and dynamically picks the real model that will answer.
Under the hood, a compact classifier (~100M parameters, derived from **mmBERT** , served on **CPU** — so no pressure on GPU-pod VRAM) evaluates the prompt against **several criteria** : intent (code, reasoning, multilingual…), the presence of a possible _jailbreak_ attempt, or **personally identifiable information (PII)** detection. Based on the verdict, Iris routes to the appropriate model — or applies a dedicated safety _guardrail_.
**The benefit** : clients hit a single endpoint (`MoM`) and get a per-prompt routing without coding their own selection logic. The _trade-off_ : ~250-300 ms of classification, incurred **only** on requests routed to `MoM`. That's acceptable for chat — the overall TTFT stays imperceptible to a human — but a deal-breaker for IDE autocomplete, which has to stay under 200 ms p95: that's precisely why **Continue** hits the coder pod directly via explicit routing.
## 💻 The clients 
A platform without clients is just plumbing. No need to walk through each config in detail here — the point is to show that we have credible **open-source alternatives to proprietary tools** : web chat, IDE autocomplete, CLI agent.
Your browser does not support video playback._OpenWebUI — web chat_ Your browser does not support video playback._OpenCode — CLI agent_
### OpenWebUI — the open-source alternative to ChatGPT / Gemini 
[**OpenWebUI**](https://openwebui.com/) exposes a standard web chat UI on top of our OpenAI-compatible API. In the models dropdown, you find the platform's pods plus the virtual `MoM` model (Iris then picks the routing by reading the prompt). **Use case** : exploratory chat, quick tests, non-developer access — exactly what you'd do on chat.openai.com or gemini.google.com.
### Continue VSCode — FIM autocomplete in the IDE 
[**Continue**](https://continue.dev/) plugs the API into VSCode (or JetBrains). The _killer feature_ here is **FIM** (_Fill-In-the-Middle_): autocomplete under **200ms p95** thanks to the dedicated always-warm coder pod and vLLM's prefix cache. That's the difference between a snappy autocomplete and a frustrating one — the equivalent of a Cursor Tab or a Copilot, but on our own infra.
### OpenCode — the open-source alternative to Claude Code 
I just discovered [**OpenCode**](https://opencode.ai/) — the CLI agent that comes closest to the **Claude Code** experience, and therefore the only serious candidate to consider for a migration the day it becomes necessary. It ships an explicit _compatibility shim_ — `AGENTS.md` ↔ `CLAUDE.md`, **skills** , **MCPs** , sub-agents, slash commands — so that **the entire workflow built around Claude Code carries over directly**. That's what sets it apart from Aider, Crush, or Continue's agent mode.
## 📊 Monitoring and continuous evaluation 
An LLM platform produces signals **on several axes at once** — serving health, per-tenant token consumption, response quality — and each has its own indicators (TTFT, inter-token latency, prefix cache hit, token usage per operation…) that classic web monitoring doesn't capture. Good news: the **existing observability stack** (VictoriaMetrics, VictoriaLogs, Grafana) absorbs all of this with no new tooling — it was just a matter of wiring up the right sources. And the stake goes **beyond anomaly detection** : understanding _how_ the platform is used — who consumes what, on which models, at what cost — is just as important.
### Platform health — `vLLM` metrics 
[`vLLM` natively exposes](https://docs.vllm.ai/en/stable/usage/metrics/) a full Prometheus endpoint, with each metric carrying a `model_name` label:
  * **Load** : `vllm:num_requests_running`, `vllm:num_requests_waiting`, `vllm:gpu_cache_usage_perc` (KEDA triggers come from here)
  * **Performance** : `vllm:time_to_first_token_seconds`, `vllm:inter_token_latency_seconds`, `vllm:e2e_request_latency_seconds`
  * **Throughput** : `vllm:prompt_tokens`, `vllm:generation_tokens`
  * **Optimizations** : `vllm:prefix_cache_hits` (essential to measure FIM efficiency)


The `LLM Platform` Grafana dashboard (deployed via `Grafana Operator` from `apps/base/ai/llm/grafana-dashboard.yaml`) aggregates all of this per model:
Grafana dashboard — LLM fleet overview
In the screenshot, the stack is idle: 4 active models (one replica each), 0 in-flight requests, KV cache at 0.01% — the "always warm" state with no load.
### Usage and FinOps — AI Gateway metrics 
The **`Envoy AI Gateway`**observes traffic**one level up** from vLLM — at the business level. Its metrics follow the [OpenTelemetry Gen AI Semantic Conventions](https://aigateway.envoyproxy.io/docs/capabilities/observability/metrics/) standard: it doesn't count requests, it instruments the **actual business vocabulary of LLMs** :  
| Metric  | Measures  |  
| --- | --- |  
| `gen_ai.client.token.usage`  | Tokens consumed (input / output / total)  |  
| `gen_ai.server.request.duration`  | End-to-end per-request latency  |  
| `gen_ai.server.time_to_first_token`  | TTFT at the gateway level  |  
| `gen_ai.server.time_per_output_token`  | Inter-token latency  |  
Each metric is automatically enriched with `gen_ai.*` labels (model, operation, provider) — you already know _who consumes what_ in a single PromQL query. And you can **inject arbitrary HTTP headers as labels** (`x-tenant-id`, `x-team`…): from there, you can answer:
  * **Which tenant burns the most tokens** over the last 30 days?
  * **Which team has the highest p95 latency** on the coder?
  * **What's the prompt/generation ratio** per user? (useful for context sizing)
  * **How many chat vs embedding calls** per client?


> The usual SLOs / alerts (p95 latency, error rate, GPU saturation) stay defined as `VMRule` next to that — nothing LLM-specific, I cover it in the [observability/alerting article](https://blog.ogenki.io/post/series/observability/alerts/).
### Qualitative evaluation with `Promptfoo`
Infrastructure metrics tell you whether the platform **works**. They don't tell you whether it **produces good answers** — that's a completely orthogonal dimension, and exactly what [**Promptfoo**](https://www.promptfoo.dev/) brings to the table.
The idea is simple: you declare test cases that look like unit tests — an input prompt + assertions on the expected output. The assertion types cover the whole spectrum:
  * **Deterministic** : `equals`, `contains`, `regex`, `is-json` (with schema), `javascript` / `python` — perfect for validating structured output (tool-calling, formatted JSON)
  * **Model-assisted** : `llm-rubric` (an LLM grades the output against a qualitative rubric), `factuality` (adherence to provided facts), `similar` (embeddings + cosine), `g-eval` (chain-of-thought with custom criteria) — the same approach as proprietary evals (HELM, MT-Bench), but declarable in YAML in-house


In the stack, this is packaged as a **nightly CronJob** (`tooling/base/promptfoo/`): the suite lives in a `ConfigMap`, the job runs against the models + the `MoM` routing, and results are **pushed as Prometheus metrics** — so they show up in the same Grafana as the technical metrics.
The concrete benefits:
  * **Regression detection** on a model bump or a `vLLM` version bump — a quality score that drops from 80 → 65% overnight is far more telling than a latency graph
  * **Cascade vs direct model comparison** : does the `MoM` routing degrade quality compared to a direct call to the right model?
  * **Stress-testing sequential tool-calling** : how many consecutive tool calls can `Qwen2.5-Coder` chain before crashing?


Continuous eval isn't about reassuring yourself in absolute terms, but about **measuring evolution over time** and avoiding silent regressions — that's what lets me quantify the claims I make in the conclusion.
## 💭 Closing thoughts 
### What did we manage to achieve? 
We managed to build a **simple way to serve open-weight models** on Kubernetes: a YAML claim triggers all the necessary plumbing, swapping a model is a few-line change, and observability is in place end-to-end.
But let's be honest about user experience: without access to "serious" models (DeepSeek V4 Pro, Kimi K2.6, Qwen3-Coder-30B…), our tests mostly came down to **checking that we got _any_ response** — not to measuring real production-grade quality. With the 7-8B models that fit on an L4, you sometimes feel like you're back in the stone age 😅.
The bright side is what's under the hood: **the stack itself is solid and production-ready**.
### Who this resonates with today 
The experience is immediately relevant for **organizations with a real data-sovereignty concern** — healthcare, defense, finance, regulated industries. And with the most advanced open-weight models (DeepSeek V4 Pro, Kimi K2.6, within ten points of Opus on SWE-bench), the quality gap with proprietary models has become marginal. For data that can't leave the premises anyway, the question barely even comes up anymore.
For everyone else — myself first — **the point is to lay the foundations** for the moment the calculus really shifts. That moment depends on two fast-moving factors:
  * **Hardware diversification** : Nvidia's hegemony is starting to crack. DeepSeek V4 already runs on Huawei Ascend 950 (with _Day 0 adaptation_ confirmed by Huawei, Cambricon, and Hygon). More vendors → more availability → downward pressure on prices.
  * **Financial accessibility** : open-weight models closing in on the frontier (1T+ params) still demand 8 to 16× H100 — out of reach personally, still steep for many organizations.


### A (deliberately short) geopolitical note 
I deliberately kept these considerations out of the main article — and I take the benchmarks at face value, assuming they're unbiased. That said, two observations are worth putting down:
  * **Chinese players are betting on open-weight** (DeepSeek, Kimi/Moonshot, Qwen/Alibaba). It's an industrial-strategy bet that may pay off long-term — the more the open-weight ecosystem matures, the more competitive it becomes against closed models.
  * **And their results on SWE-bench** — the coding reference — are **really good** : on SWE-bench Pro, Kimi K2.6 leads the open-weight pack at 58.6%, ~6 points behind Claude Opus 4.7. On Verified, DeepSeek V4 Pro brushes 80%, ~7 points behind Opus. The gap is closing, benchmark after benchmark.


### What's next? 
Let's be clear: today I wouldn't trade my Claude ecosystem. Mainly for **financial** reasons — not out of any particular attachment. At my usage scale, Sonnet and Opus cost me less than replicating equivalent quality through self-hosting.
That said, I would have liked to push my use of **OpenCode** further and migrate my Claude setup (skills, MCPs, sub-agents) onto that backend for good — that may be the topic of a future article dedicated to this open-source coding agent.
But I'm keeping the stack alive. The day a Qwen3-Coder-30B-A3B runs cleanly on a quantized L4 — a path documented in the [AI platform roadmap](https://cnref.ogenki.io/docs/platform/ai-platform/roadmap/) — the swap will be a few-line PR. That's the **main point** of this demo: positioning yourself to **move fast when the time comes** , rather than scrambling to (re)build everything the day open-weight catches up to the frontier.
And this catch-up isn't only about models: the open-source serving layer evolves just as fast and regularly brings in capabilities previously reserved for proprietary solutions. For instance, [**vLLM-Omni**](https://github.com/vllm-project/vllm-omni) (first stable late 2025) extends `vLLM` to **omni-modality** (text, image, audio, video, as **inputs and outputs**) with the same OpenAI-compatible API, so it plugs directly into the platform described here.
**The kicker** : this entire stack was designed and built with the help of **Claude Code** 🙃.
## 🔖 References 
### Repos 
  * [`cloud-native-ref`](https://cnref.ogenki.io) — The complete platform documentation · [source on GitHub](https://github.com/Smana/cloud-native-ref)
  * [`crossplane-configuration`](https://github.com/Smana/crossplane-configuration) — The Crossplane compositions (`App`, `SQLInstance`, `InferenceService`…)
  * [Architecture decisions](https://cnref.ogenki.io/docs/decisions/) — ADRs (vLLM Production Stack, S3 Files…)
  * [AI platform roadmap](https://cnref.ogenki.io/docs/platform/ai-platform/roadmap/) — Evolution paths


### Technical components 
  * [vLLM Production Stack](https://github.com/vllm-project/production-stack) — Production-grade LLM inference
  * [vLLM-Omni](https://github.com/vllm-project/vllm-omni) — Omni-modality serving (text/image/audio/video, in & out)
  * [vLLM Semantic Router (Iris)](https://github.com/vllm-project/semantic-router) — Smart multi-model routing
  * [Envoy AI Gateway](https://github.com/envoyproxy/ai-gateway) — Gateway API for LLMs
  * [Promptfoo](https://www.promptfoo.dev/) — Continuous LLM evaluation
  * [Continue](https://continue.dev/) — VSCode IDE assist extension
  * [OpenCode](https://opencode.ai/) — OSS CLI agent loop
  * [OpenWebUI](https://openwebui.com/) — Web chat for LLM APIs


### Open-weight models referenced 
  * [Qwen2.5-Coder-7B-Instruct](https://huggingface.co/Qwen/Qwen2.5-Coder-7B-Instruct) (Apache 2.0)
  * [Qwen3-8B](https://huggingface.co/Qwen/Qwen3-8B) (Apache 2.0)
  * [Llama-Guard-3-1B](https://huggingface.co/meta-llama/Llama-Guard-3-1B) (Llama 3 license)
  * [DeepSeek V4 / V4-Pro](https://api-docs.deepseek.com/) (official model card, May 2026)


### DeepSeek V4 sources 
  * [DeepSeek launches 1.6 trillion parameter V4 on Huawei chips](https://www.tomshardware.com/tech-industry/artificial-intelligence/deepseek-launches-1-6-trillion-parameter-v4-on-huawei-chips-as-us-escalates-ai-theft-accusations) — Tom's Hardware
  * [Three reasons why DeepSeek's new model matters](https://www.technologyreview.com/2026/04/24/1136422/why-deepseeks-v4-matters/) — MIT Technology Review
  * [DeepSeek V4 arrives with near state-of-the-art intelligence at 1/6th the cost of Opus 4.7, GPT-5.5](https://venturebeat.com/technology/deepseek-v4-arrives-with-near-state-of-the-art-intelligence-at-1-6th-the-cost-of-opus-4-7-gpt-5-5) — VentureBeat
  * [Huawei Ascend, Cambricon and Hygon Completed Day 0 Adaptation to DeepSeek-V4](https://www.trendforce.com/news/2026/04/29/news-huawei-ascend-cambricon-and-hygon-completed-day-0-adaptation-to-deepseek-v4/) — TrendForce


### Previous articles in the series 
  * [Agentic Coding: concepts and hands-on use cases](https://blog.ogenki.io/post/series/agentic_ai/ai-coding-agent/) — Part 1
  * [A few months with Claude Code: tips and workflows](https://blog.ogenki.io/post/series/agentic_ai/ai-coding-tips/) — Part 2


  1. For the precise definition — and the distinction from _open-source_ — see [Open Weights, by the Open Source Initiative](https://opensource.org/ai/open-weights). [↩︎](https://blog.ogenki.io/post/series/agentic_ai/llm-self-hosted-stack/#fnref:1)
  2. Verified is now considered partially contaminated: OpenAI has [stopped reporting on it](https://openai.com/index/why-we-no-longer-evaluate-swe-bench-verified/) in favor of [SWE-bench Pro](https://labs.scale.com/leaderboard/swe_bench_pro_public), where Kimi K2.6 leads the open-weight pack at 58.6%. [↩︎](https://blog.ogenki.io/post/series/agentic_ai/llm-self-hosted-stack/#fnref:2)
  3. Multipliers documented in the original paper [Efficient Memory Management for Large Language Model Serving with PagedAttention](https://arxiv.org/abs/2309.06180) (Kwon et al., SOSP 2023): vLLM reaches **2-4×** the throughput of FasterTransformer and **14-24×** that of a HuggingFace Transformers baseline. The actual range depends on the workload and chosen baseline. [↩︎](https://blog.ogenki.io/post/series/agentic_ai/llm-self-hosted-stack/#fnref:3)


### Translations:
