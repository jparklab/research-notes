---
title: 'Envoy AI Gateway Hits 1.0: What an LLM-Aware Kubernetes Gateway Actually Load-Balances
  On | bex.co'
id: envoy-ai-gateway-hits-10-what-an-llm-aware-kubernetes-gateway-actually-load-bala
tags:
- gateway-routing
- production-adopter
- routing-signals
- kv-cache-routing
created: '2026-10-09T22:53:55.479233Z'
updated: '2026-10-10T14:52:17.007366Z'
source: https://bex.co/blog/2026/09/23/envoy-ai-gateway-1-0-inference-extension-llm-load-balancing
source_domain: bex.co
fetched_at: '2026-10-09T22:53:55.458549Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Practitioner deep-dive (bex.co, published ~Sept 2026) framing Envoy AI Gateway''s
  v1.0 GA (June 23, 2026) around the question of what an inference-aware gateway sees
  that a generic HTTP LB does not. Gives a named table of 6 routing signals with explicit
  contrast against generic L7: per-backend KV-cache utilization (generic LB sees ''nothing
  -- cache state is invisible outside the model server''), queue depth/pending tokens
  (generic LB sees only connection count), prompt-prefix affinity (''nothing -- request
  bodies are opaque to L7 routing''), loaded LoRA adapter (''nothing -- same endpoint,
  different weights''), prefill vs decode role in a disaggregated fleet (''nothing
  -- all pods look identical''), and predicted time-to-first-token (generic LB has
  only lagging past response times). Cites two quantified benchmarks: llm-d project
  reports up to 57x faster time-to-first-token and 2x throughput vs round-robin under
  high prefix reuse on 8 pods; a separate 16-GPU case study found inference-aware
  routing doubled served users on the same hardware with zero new hardware purchased.
  Describes the three-part GIE architecture (InferencePool + InferenceModel CRDs,
  an out-of-process Endpoint Picker/EPP scoring service, and Envoy ext-proc as the
  glue) and names conforming gateways: Envoy Gateway, kgateway, and GKE Gateway (Google''s
  managed offering routes on KV-cache usage across clusters). Names the single production
  adopter explicitly: LY Corporation runs Envoy AI Gateway in front of multi-tenant
  self-hosted LLM traffic, citing unified routing, token-based rate limiting, authentication,
  and InferenceExtension alignment. Documents ecosystem breadth of independent EPP
  implementations: llm-d''s inference scheduler (prefix-aware KV-cache routing, prefill/decode
  disaggregation, pluggable scorers), NVIDIA Dynamo''s own EPP (scores on KV-cache
  hit probability), and Microsoft KAITO which overrides the default EPP image with
  the llm-d scheduler for managed inference. Reports Istio shipped alpha GIE support
  in v1.27 and announced beta (with ambient multicluster beta) at KubeCon EU 2026
  (March 25), gated behind the ENABLE_GATEWAY_API_INFERENCE_EXTENSION pilot flag --
  meaning inference-aware routing is now a service-mesh feature, not gateway-only.
  Explicitly scopes what the gateway does NOT do: clarifies ''AI Gateway'' here is
  a self-hosted-model routing layer, not a hosted-API reseller/proxy -- it does not
  call third-party model APIs or mark up tokens; states inference-aware routing gains
  concentrate in three situations (shared prompt prefixes, mixed request lengths,
  GPU-constrained fleets) and is pointless for single-replica or pure-hosted-API traffic.'
utility_score: 13.0
---

[Skip to main content](https://bex.co/blog/2026/09/23/envoy-ai-gateway-1-0-inference-extension-llm-load-balancing#main)
Copy page
Share
On this page
  * [Why round-robin destroys inference throughput](https://bex.co/blog/2026/09/23/envoy-ai-gateway-1-0-inference-extension-llm-load-balancing#why-round-robin-destroys-inference-throughput)
  * [How any Gateway API proxy becomes an Inference Gateway](https://bex.co/blog/2026/09/23/envoy-ai-gateway-1-0-inference-extension-llm-load-balancing#how-any-gateway-api-proxy-becomes-an-inference-gateway)
  * [What Envoy AI Gateway 1.0 adds on top](https://bex.co/blog/2026/09/23/envoy-ai-gateway-1-0-inference-extension-llm-load-balancing#what-envoy-ai-gateway-10-adds-on-top)
  * [Cross-mesh now, not one vendor's bet](https://bex.co/blog/2026/09/23/envoy-ai-gateway-1-0-inference-extension-llm-load-balancing#cross-mesh-now-not-one-vendors-bet)
  * [What self-hosters actually gain — and when generic LB is fine](https://bex.co/blog/2026/09/23/envoy-ai-gateway-1-0-inference-extension-llm-load-balancing#what-self-hosters-actually-gain--and-when-generic-lb-is-fine)


The same 16 GPUs served twice the users, and nobody bought new hardware. The only change was the routing layer in front of the inference fleet: instead of spraying requests round-robin across vLLM replicas, the gateway started sending each prompt to the backend most likely to already hold its prefix in KV cache. That single decision — route on what the model servers know, not on connection counts — is the entire thesis of the Gateway API Inference Extension, and it just got its most production-ready expression yet: Envoy AI Gateway hit v1.0 GA on June 23, 2026.
If you run inference behind Kubernetes, here is the concrete payoff up front. A generic HTTP load balancer sees connections and response codes. An inference gateway sees this:
Scrollable table  
| Routing signal  | What it measures  | What a generic HTTP LB sees instead  |  
| --- | --- | --- |  
| Per-backend KV-cache utilization  | How full each replica's KV cache is, and whether your prompt's prefix is already cached there  | Nothing — cache state is invisible outside the model server  |  
| Queue depth / pending tokens  | Outstanding requests and estimated remaining tokens per backend  | Connection count, which says nothing about decode backlog  |  
| Prompt-prefix affinity  | Which replica already holds the shared system prompt or conversation prefix  | Nothing — request bodies are opaque to L7 routing  |  
| Loaded LoRA adapter  | Which fine-tuned adapter each backend currently has resident  | Nothing — same [endpoint](https://bex.co/glossary/endpoint), different weights  |  
| Prefill vs decode role  | Whether a pod is assigned prompt processing or token generation in a disaggregated fleet  | Nothing — all pods look identical  |  
| Predicted latency  | Expected time-to-first-token given the signals above  | Past response times, which lag behind queue buildup  |  
Everything below is the story of how that table became a cross-vendor standard — and what it means if you self-host models on a fleet you manage yourself.
## Why round-robin destroys inference throughput[​](https://bex.co/blog/2026/09/23/envoy-ai-gateway-1-0-inference-extension-llm-load-balancing#why-round-robin-destroys-inference-throughput "Direct link to Why round-robin destroys inference throughput")
Round-robin assumes every request costs roughly the same work, so spreading requests evenly spreads load evenly. LLM traffic violates that assumption in three specific ways, each of which a generic `Service` or ingress faithfully ignores.
First, each replica owns an independent KV cache. On a single vLLM instance, prefix caching is a massive speedup: a shared system prompt or agent tool schema is processed once and reused. Put round-robin in front of eight replicas and successive turns of the same conversation land on different backends every time, so every replica re-prefills the same prefix and most of the caching speedup evaporates. Cache-aware routing exists to fix exactly this: send the request to the worker with the warmest KV state for its prefix.
Second, request cost varies enormously. A 2,000-token generation and a 20-token classification look identical to a connection counter but differ by roughly two orders of magnitude in GPU time. Under continuous batching, the scheduler packs in-flight requests into each forward pass, so one replica holding several long decodes is far more loaded than its connection count suggests. Routing on pending tokens rather than connections is the difference between balancing work and balancing a proxy metric.
Third, a single long request can monopolize a pod while its siblings sit idle. Random routing has no notion of queue depth per backend, so latency becomes unpredictable: the same prompt is fast or slow depending on which replica's decode queue it happened to land in. Least-queue-depth routing alone is a large jump over round-robin; adding token awareness and prefix affinity compounds it.
The published numbers put scale on these mechanisms. The llm-d project's benchmarks report up to 57x faster time-to-first-token and 2x throughput against round-robin routing under high prefix reuse on 8 pods — the regime every agent workload with a shared system prompt lives in. A separate 16-GPU case study found inference-aware routing doubled the users the same hardware could serve. These are routing gains, not hardware gains: the GPUs were already paid for.
## How any Gateway API proxy becomes an Inference Gateway[​](https://bex.co/blog/2026/09/23/envoy-ai-gateway-1-0-inference-extension-llm-load-balancing#how-any-gateway-api-proxy-becomes-an-inference-gateway "Direct link to How any Gateway API proxy becomes an Inference Gateway")
The Gateway API Inference Extension, developed in `kubernetes-sigs/gateway-api-inference-extension` by SIG-Network together with WG-Serving, standardizes this routing intelligence so no single gateway vendor owns it. The design has three moving parts.
Two new CRDs describe intent. `InferencePool` selects the set of model-server pods behind one logical endpoint, and `InferenceModel` names the servable model (or LoRA adapter) and points at its pool. Operators declare "model X is served by these pods" in the same declarative style as `HTTPRoute`, instead of embedding backend topology in gateway-specific config.
An Endpoint Picker (EPP) makes the per-request decision. The EPP is a separate service that scores candidate endpoints on the inference signals from the table above — KV-cache utilization, queue depth, prefix affinity, adapter residency, predicted latency — and picks the best backend for each request. Because scoring lives outside the data plane, scheduling policy can evolve (new scorers, new plugins) without changing the proxy.
Envoy's External Processing protocol (ext-proc) glues them together. Any gateway that speaks both Gateway API and ext-proc can delegate endpoint selection to an EPP and become, in the project's terminology, an Inference Gateway. The documented conforming gateways are Envoy Gateway, kgateway, and GKE Gateway — Google's managed offering routes on KV-cache usage across clusters. The extension point is the product: routing policy is portable across proxies instead of locked to one.
Optional body-based routing handles the awkward fact that OpenAI-compatible requests carry the model name in the JSON body, where L7 matchers cannot see it. A filter extracts the `model` field and injects it as a header so ordinary Gateway API matching can route on it.
## What Envoy AI Gateway 1.0 adds on top[​](https://bex.co/blog/2026/09/23/envoy-ai-gateway-1-0-inference-extension-llm-load-balancing#what-envoy-ai-gateway-10-adds-on-top "Direct link to What Envoy AI Gateway 1.0 adds on top")
If the Inference Extension is the standard, Envoy AI Gateway is its most opinionated implementation — an additive layer on CNCF's Envoy Gateway purpose-built for GenAI traffic. The v1.0 announcement framed it as the open-source standard for enterprise AI traffic, and the named production user is telling: LY Corporation runs it in front of multi-tenant, self-hosted LLM traffic, citing unified routing, token-based [rate limiting](https://bex.co/glossary/rate-limiting), authentication, and alignment with the Inference Extension as the reasons.
The core abstraction is a three-link chain. An `AIGatewayRoute` attaches to a `Gateway` and matches on the model header the gateway derives from each request body's `model` field (`x-ai-eg-model`). It forwards to an `AIServiceBackend`, which resolves to per-model `Backend` entries — typically the FQDN of the vLLM `Service` for that model. A minimal sketch looks like this:
yaml
```
apiVersion: aigateway.envoyproxy.io/v1alpha1
kind: AIGatewayRoute
metadata:
  name: llm-inference
  namespace: llm
spec:
  parentRefs:
 name: inference-gateway
  rules:
 matches:
 headers:
 name: x-ai-eg-model
              value: llama-4-scout
      backendRefs:
 name: llama-backend
          kind: AIServiceBackend
```

Note what this buys over a hand-rolled ingress. Model routing is dynamic and content-derived: clients send ordinary OpenAI-compatible requests and the gateway sorts them by model without per-model hostnames. Rate limiting is token-based rather than request-based, which is the only unit that makes sense when one request can cost 100x another. Authentication and multi-tenant policy attach at the same layer, so per-tenant model access is gateway config, not application code. And because the backends are `InferencePool`s with an EPP behind them, every request still gets the inference-aware endpoint selection from the previous section.
One clarification worth making explicit, because the name invites confusion: "AI Gateway" here is a routing layer for self-hosted model traffic, not a hosted-API reseller product. Nothing in this stack calls out to a third-party model API or marks up tokens. It sits in your cluster, in front of your GPUs, and decides which of your replicas serves each prompt.
## Cross-mesh now, not one vendor's bet[​](https://bex.co/blog/2026/09/23/envoy-ai-gateway-1-0-inference-extension-llm-load-balancing#cross-mesh-now-not-one-vendors-bet "Direct link to Cross-mesh now, not one vendor's bet")
A routing standard is only as good as its second implementation, and the Inference Extension now has several. The headline one is Istio: at KubeCon + CloudNativeCon Europe 2026 on March 25, the project announced beta support for the Gateway API Inference Extension alongside ambient multicluster beta, promoting the alpha that had shipped in Istio v1.27 toward beta in v1.29. Operators enable it with the `ENABLE_GATEWAY_API_INFERENCE_EXTENSION` pilot flag, and the data plane gains the same model-aware endpoint selection — meaning inference-aware routing is now a service-mesh feature, not just a gateway feature.
The specialized Endpoint Pickers prove the extension point is real rather than ceremonial. The llm-d inference scheduler builds directly on the upstream EPP and adds production scheduling plugins: prefix-aware KV-cache routing, prefill/decode disaggregation that scales prompt processing and token generation independently, and pluggable scorers per deployment. NVIDIA's Dynamo ships its own EPP that scores endpoints on KV-cache hit probability. Microsoft's KAITO overrides the default EPP image with the llm-d scheduler for its managed inference. Each of these is a different scheduling brain behind the same `InferencePool` API — exactly the portability the extension was designed for.
The trajectory matters more than any single release. In roughly a year the pattern went from one project's experiment to a SIG-backed API with Envoy Gateway, kgateway, GKE Gateway, and Istio implementations plus vendor-specific schedulers competing on scoring quality. Betting your inference routing on this API is betting with the ecosystem's direction, not on a single vendor's roadmap.
## What self-hosters actually gain — and when generic LB is fine[​](https://bex.co/blog/2026/09/23/envoy-ai-gateway-1-0-inference-extension-llm-load-balancing#what-self-hosters-actually-gain--and-when-generic-lb-is-fine "Direct link to What self-hosters actually gain — and when generic LB is fine")
Now translate all of this to the operator's view: you run a Cluster-API-managed fleet and serve models on machines you own. The gains from inference-aware routing concentrate in three situations, and you almost certainly sit in at least one.
You gain when prompts share prefixes. Agent loops, RAG pipelines, and chat products all repeat large system prompts or tool schemas across requests. That repetition is free throughput under prefix-aware routing and pure waste under round-robin. If your p50 prompt shares its first thousand tokens with its neighbors, the gateway is the cheapest optimization in your stack.
You gain when request lengths mix. A fleet serving both quick classifications and long generations on the same replicas will see queue-depth blindness as tail-latency spikes. Token-aware routing smooths exactly this: it prices each backend by remaining work, not open connections.
You gain when GPUs are the constraint. Every percentage point of throughput recovered by better routing is capacity you do not have to buy. That 2x-throughput figure comes from the high-prefix-reuse case, but even modest reuse moves the needle when each GPU costs thousands per month to own or rent.
Conversely, stay with the generic HTTP load balancer your git-push [PaaS](https://bex.co/glossary/platform-as-a-service) already ships when none of this applies: a single replica has nothing to route between, uniformly short stateless prompts leave little prefix to exploit, and traffic you forward to hosted model APIs never touches your GPUs at all. Inference gateways optimize self-hosted serving; they do not make hosted APIs cheaper.
For the self-hosting team in the middle — a few GPU nodes behind a Gateway API proxy, agent traffic growing, tail latency creeping — the adoption path is incremental. Declare your model servers as an `InferencePool`, point an EPP at them, and keep your existing gateway. The 1.0s landing across Envoy AI Gateway, the extension itself, and mesh integrations mean the APIs you adopt today are the ones the ecosystem is standardizing on, not experiments you will rewrite next year.
_Bex.co is the open-source, AI-native Render alternative — push a git repo, get a running HTTPS service on machines you own.[Star the repo on GitHub](https://github.com/bex-co/bex) or deploy your first app today._
## Related articles
  * ### [OpenCost 1.121 Brings Inference Cost Tracking to Kubernetes: Metering GPU Token Workloads Like Any Other Tenant OpenCost 1.121 adds per-inference cost metering to Kubernetes, so self-hosters… Kubernetes cost-optimization 10 min ](https://bex.co/blog/2026/08/06/opencost-kubernetes-inference-cost-tracking)
  * ### [Hot-Pluggable GPUs Join the CNCF: What CoHDI's Sandbox Debut Means for Your Self-Hosted GPU Pool CoHDI entered the CNCF Sandbox promising hot-pluggable GPUs via Kubernetes DRA.… self-hosting Kubernetes 11 min ](https://bex.co/blog/2026/09/11/cohdi-cncf-sandbox-gpu-disaggregation)
  * ### [OpenCost 1.121 Finally Answers: What Does Each Token Cost on Your Own GPUs? OpenCost 1.121 adds Kubernetes-native per-token inference metering via llm-d —… AI LLM 13 min ](https://bex.co/blog/2026/08/21/opencost-per-token-inference-cost-tracking-self-hosted-gpus)
  * ### [Kubernetes 1.37 Locks In On-Demand PLEG Relist: What Cost-Sized Nodes Actually Save Kubernetes 1.37 graduates PLEG on-demand relist to GA, cutting pod-create… Kubernetes self-hosting 9 min ](https://bex.co/blog/2026/10/02/pleg-ondemand-relist-ga-kubernetes-137)


## Run this on infrastructure you own
bex is the open-source, AI-native Render alternative — push a git repo and get a running HTTPS service on your own machines.
[Get started with bex](https://bex.co/docs/platform)
