# How Organizations Run LLM Inference Engines for Internal Use

Serving open-weight models to an organization's own services has converged on a recognizable default column of software. The engine is vLLM; the substrate is Kubernetes; above the engine sits a Kubernetes-native control plane (llm-d, KServe's `LLMInferenceService`, ByteDance's AIBrix, or NVIDIA Dynamo) that places requests by cache locality and scales replicas on engine-internal signals. In front sits a gateway hiding from internal callers whether a request went to a self-hosted model or a frontier vendor's API. Open source covers that entire column, and vLLM recurs as the engine across nine first-person accounts — Netflix, Atlassian, Intility, Snowflake, Hetzner, LinkedIn, T-Tech, Thomson Reuters and Tesla (Spotify a tenth, third-party-attested) [[llm-serving-on-kubernetes-llm-d-aibrix-dynamo-compared]] [[how-to-build-an-inference-platform]] [[linkedin-building-and-evolving-a-production-genai-application-stack-zenml-llmops]].

Custom code concentrates *above* the engine — gateway and policy plane, per-team quota enforcement, evaluation content as distinct from evaluation tooling, and glue to older infrastructure — drawn, across roughly a dozen named cases, less by gaps in open source than by what each organization already owns.

Two limits apply throughout: no reliable public measurement exists of how common internal self-hosted serving is, and the published record skews toward layers where disclosure flatters the publisher.

## 1. How Internal LLM Inference Is Actually Run: Deployment Patterns

Running inference internally means holding model weights in accelerator memory, batching requests against them and returning tokens, wrapped in enough infrastructure that other teams can call it. The first decision is whether the organization operates the stack at all: *alternatives to frontier models* resolves along two axes — self-host versus provider-operated, and customize the weights versus don't — with all four quadrants populated, and routing hybrid rather than exclusive [[interim-report-managed-middle-hosted-open-weight]] [[navigating-the-llm-landscape-ubers-innovation-with-genai-gateway]].

| Pattern | Condition that selects it | Named instances |
|---|---|---|
| Self-hosted on own Kubernetes | Data residency, volume, version pinning, an existing platform team | Thomson Reuters, T-Tech, Intility, Tesla, Hetzner [[what-we-learned-from-our-inference-experiment]] |
| Engine folded into a pre-existing ML platform | A multi-framework ML-serving platform predates the LLM workload | Netflix [[in-house-llm-serving-at-netflix-by-ai-platforms-model-runtime-team-and-by-netfli]], Atlassian [[atlassians-inference-engine-our-self-hosted-ai-inference-service-inside-atlassia]] |
| Managed endpoint, weights customized | Differentiation sits above the infrastructure line | Harvey [[harvey-tenet-research-preview]], Wispr Flow [[wispr-flow-creates-effortless-voice-dictation-with-llama-on-baseten]], EliseAI [[how-eliseai-trains-specialized-models-for-healthcare-and-housing-operations-with]] |
| Managed endpoint, weights unchanged | A cheaper tier suffices; system fixes close the gap | Amplitude, Hebbia [[how-hebbia-uses-baseten-to-power-ai-workflows-for-the-worlds-leading-financial-i]], Patreon [[patreon-saves-nearly-600kyear-in-ml-resources-with-baseten]] |
| On-premise or network-isolated | Classification level or statutory isolation | UK Dept for Business and Trade [[using-self-hosted-large-language-models-llms-securely-in-government-digital-trad]], GovTech Singapore, Snowflake [[llm-model-serving-faster-inference-on-snowflake]] |
| Bespoke frontier-scale serving | Extreme-scale serving is itself the product | DeepSeek [[open-infra-index202502opensourceweekday_6_one_more_thing_deepseekv3r1_inference_]], Moonshot AI [[source-analysis-mooncake-a-kvcache-centric-disaggregated-architecture-for-llm-se]] |
| Local / single-user runtimes | Development loops, not shared serving | Ollama, llama.cpp [[ollama-vs-vllm-a-deep-dive-into-performance-benchmarking-red-hat-developer]] |

Which component matters is set by workload geometry — prompt length, prefix sharing, latency tolerance — not by request rate [[a-year-in-llm-serving-workload-evolution-caching]]:

| Workload | Shape | What it selects |
|---|---|---|
| Internal chat, assistants | Latency-bound, multi-turn, high prefix reuse | Prefix-cache-aware routing, warm replicas (T-Tech) |
| Agentic, tool-calling | Long shared prefixes, sessions idle for minutes | SGLang or cache offload (§7.B) |
| RAG, retrieval | Short high-rate requests, embeddings outnumbering generations | A separate embedding and rerank tier (GovTech) |
| Classification, extraction | Throughput-bound, many small models | LoRA pooling on a shared base (UK DBT) |
| Batch enrichment | Throughput-only, no interactive deadline | Shared-and-busy capacity, where the cost case holds |

### A. What can and cannot be measured

Menlo Ventures, surveying 150-plus technical leaders, reports open-source share of enterprise production workloads *falling* from 19% to 13% [[2025-mid-year-llm-market-update-foundation-model-landscape-economics-menlo-ventu]]; Constellation Research, reporting one commercial gateway's telemetry, reports open-weight share of routed tokens *rising* to 62%, while conceding it cannot see self-hosted deployments [[open-weight-models-are-gaining-ground-in-enterprise-ai-constellation-research]]. The nearest defensible substrate statistic is CNCF's annual survey, reported in an Istio release: 66% of organizations run generative-AI workloads on Kubernetes, only 7% achieving daily deployments — Kubernetes GenAI workloads generally, not internal inference serving [[istio-brings-future-ready-service-mesh-to-the-ai-era-with-new-ambient-multiclust]].

### B. Stated motives, and where the cost argument runs out

Intility enumerates the motives most completely: data control, cost at volume, compliance, availability, and predictability against vendor pricing changes and model deprecations [[how-to-build-an-inference-platform]]. T-Tech says data residency forces self-hosting [[from-production-traffic-to-post-training-building-a-self-hosted-llm]]. Thomson Reuters cites lock-in risk "without a clear exit strategy" and a 19.7% saving on one self-hosted open-weight model [[thomson-continual-learning-of-frontier-models-for]].

The cost leg is weakest. Published break-even thresholds are incommensurable [[ai-inference-cost-economics-in-2026-gpu-finops-playbook-spheron-blog]], and one circulated preprint restricts operating expenditure to electricity while computing its API comparator from full monthly token *capacity*. Those choices jointly imply, though the paper does not say so, a deployment saturated every operating hour and staffed by nobody [[a-cost-benefit-analysis-of-on-premise-large-language-model-deployment-breaking-e]]. The one treatment that controls allocation finds the same workload flipping from a **40.1% total-cost saving under shared capacity** to a **43.8% penalty under dedicated always-on reservation** [[inference-economics-of-enterprise-coding-agents]].

Dropbox, having moved storage off public cloud, declines to own dedicated AI compute because cost and capability curves are not yet predictable enough [[building-the-silicon-brain-with-drew-houston-of-dropbox]] [[dropbox-building-a-silicon-brain-for-universal-enterprise-search-zenml-llmops-da]]. Repatriation logic holds where the workload is stable and the constraint structural, not where the organization is still discovering which models it needs.

## 2. The Model Layer: Alternatives to Frontier Models

The open-weight families served internally usually work as a cheaper tier inside a routed portfolio rather than as drop-in replacements. AT&T runs roughly 45 billion AI tokens a day across more than 1,000 internal use cases, with open-weight models carrying about a quarter [[what-early-open-weight-model-deployments-reveal-about-production-ai]]. Pinterest is the counter-case: at some 620 million monthly users it made an open model the primary path, replacing Qwen3-VL's vision layer with proprietary embeddings for a reported **90% cost cut** — customization "foundationally in-house," in its CTO's words [[pinterest-cut-ai-costs-90-by-gutting-a-frontier-models-vision-layer-venturebeat]].

| Family | Licence class | Sizes served | Internal role, and who serves it |
|---|---|---|---|
| Qwen 3.x–3.6 | Permissive | 4B – 397B mixture-of-experts | Continual-learning base (Thomson Reuters [[thomson-continual-learning-of-frontier-models-for]]); assistant across 200+ services (T-Tech); extraction (EliseAI) |
| Llama 3.x / 4 | Community licence | 8B – 70B+ | General serving (Tesla [[production-grade-llm-inference-at-scale-with-kserve-llm-d-and-vllm-llm-d]]); document extraction (UK DBT); voice cleanup (Wispr Flow) |
| Kimi K2.5 – K3 | Open weights | Trillion-parameter class | Analytics agent (Amplitude); legal agent base (Harvey); edge serving (Cloudflare [[building-the-foundation-for-running-extra-large-language-models]]) |
| GLM 5.1 / 5.2 | Open weights | Up to 744B at FP8 | Self-hosted near-frontier tier (Intility, Thomson Reuters); post-trained for structured extraction (Harvey [[harvey-tenet-research-preview]]) |
| Task models | Apache-2.0 / MIT | 100M – 1B | Embeddings (GovTech Singapore); a ~200M injection classifier at ~90ms/query on CPU [[developing-llm-guardrail-configs-locally-with-nemo-guardrails-red-hat-developer]]; transcription (Patreon) |

The capability gap is narrow, and the one measurement from a party with no inference-platform interest puts a number on it: Epoch AI finds the most capable open-weight models lagging frontier closed models by an average of **four months** since January 2026 and by **8 index points**, and flags that the gap "may be understated" because open-weight models perform worse on private benchmarks [[open-models-lag-state-of-the-art-closed-models-by-4-months-epoch-ai]].

Three customization tiers recur. **Serving stock weights:** Amplitude ran Kimi K2.7 unmodified on provider instances, scored 64 against Sonnet 4.6 on its own 186-case suite, self-published and scored with its own product, then reached 73.7 — past Sonnet's 72.7, at roughly one third the cost — purely through system-level fixes to the surrounding application [[practical-guide-to-evaluating-open-models-achieving-sonnet-level-performance-wit]].

**Post-training:** Harvey post-trained a Kimi K3 base into a legal agent over roughly 1,750 agentic legal environments, using no customer data, reporting nearly twice the base checkpoint's held-out task completion [[harvey-tenet-research-preview]]. T-Tech trains one reinforcement-learning expert per weak axis and merges them in weight space, having found instruction-following and formatting errors were 37.9% of production failures [[from-production-traffic-to-post-training-building-a-self-hosted-llm]]. Dynamic loading of LoRA adapters (small low-rank weight deltas over a shared base model) makes long-tail task models affordable, at a reported 4.7x cost reduction versus static deployment [[introducing-aibrix-cost-effective-and-scalable-control-plane-for-vllm-aibrix-blo]].

**Continual learning:** Thomson Reuters applied continued pre-training, value realignment and post-training to Qwen checkpoints, curating 200 billion tokens from a pool exceeding 19 trillion, its final three-week run estimated under $450,000 against roughly $40 million total development cost [[thomson-continual-learning-of-frontier-models-for]].

Most activity sits in the middle tier, which does not imply self-hosting — Harvey and EliseAI customize weights while a provider operates serving. One study priced the quality side, the only one in the record: Pegatron's single-developer comparison of a frontier-API coding agent against an on-premise quantized open-weight configuration reports a fix-commit ratio of 74.9% versus 45.9%, with the odds of a commit being a repair 2.6 to 4.9 times higher in every difficulty tier [[inference-economics-of-enterprise-coding-agents]]. It is N=1 and hypothesis-aware, so it does not generalize. FP8 (8-bit floating point) is the routine quantization choice, and `llm-compressor` became the maintained toolchain after AutoAWQ was archived in May 2025 [[awq-quantization-guide-deploy-llms-at-half-the-gpu-cost-2026-spheron-blog]].

## 3. The Internal Inference Stack, Layer by Layer

An internal inference platform is a stack of about ten layers, each with a job something has to do: weights must be versioned and moved into accelerator memory; an engine must batch and generate; pods must be placed and scaled; requests must be authenticated, metered and routed; signals must be collected; versions must roll out without dropping traffic. Few of those layers are genuinely contested.

| Layer | Near-universal component | What varies, and when |
|---|---|---|
| Weight registry | Safetensors in object storage behind an indirection layer | DataHub and SeaweedFS (Thomson Reuters); a shared read-many volume (Intility). Never the public hub at request time |
| Weight loading | Safetensors everywhere; mechanism varies | Three mechanisms, selected by which problem you have (§3.A) |
| Inference engine | **vLLM** (PagedAttention blocks plus continuous batching), overwhelmingly | SGLang, whose RadixAttention prefix tree suits long shared prefixes and agentic traffic; TensorRT-LLM where compiled kernels squeeze latency hardest [[sglang-joins-pytorch-ecosystem-efficient-llm-serving-engine-pytorch]] |
| Orchestration | Kubernetes plus a purpose-built operator | Karpenter for nodes; GitOps via ArgoCD or Flux [[atlassians-inference-engine-our-self-hosted-ai-inference-service-inside-atlassia]] |
| Pod autoscaling | KEDA (a Kubernetes event-driven autoscaler) on vLLM's metrics | Scale-to-zero needs a keepalive trigger; steady-traffic shops run warm replicas [[autoscaling-with-keda-production-stack]] |
| Gateway | Always present; built, or adopted and operated seriously | Rust plane (Thomson Reuters), Java plane (Netflix), gateway plus SDK (Atlassian, Uber), LiteLLM (Pfizer), Envoy AI Gateway (Intility, Tesla) |
| Routing signal | Prefix-cache affinity, queue depth, KV-cache occupancy | Semantic routing optional above it, at ~250–300ms classification cost [[self-hosted-llm-stack-a-solid-foundation-for-an-open-weight-platform-built-to-ev]] |
| Cache tier | LMCache, Dynamo's KV Block Manager, or AIBrix's own pool | Bandwidth and context length decide whether offloading pays (§3.B) |
| Client API | OpenAI-compatible HTTP | gRPC-first where an ML-serving convention predated LLMs (Netflix); Snowflake exposes Python, SQL and REST [[llm-model-serving-faster-inference-on-snowflake]] |
| Observability | Prometheus for engine telemetry; OpenTelemetry for app traces | Two unrelated layers (§3.D) |
| Rollout | GitOps with Helm, Kustomize or Crossplane | Shadow mode, then a canary ramp with session-sticky cohorts (§3.C); red-black shift for compatible changes, versioned deployments per model-and-version pair for breaking ones [[in-house-llm-serving-at-netflix-by-ai-platforms-model-runtime-team-and-by-netfli]] |

### A. The request path, and the two documented forks

An internal caller hits one OpenAI-compatible endpoint, and an endpoint-selection layer picks a pod on live telemetry. The Gateway API Inference Extension formalizes this: an `InferencePool` groups pods sharing a base model and server configuration. The Endpoint Picker tracks KV-cache utilization, pending-queue length and active LoRA adapters, fails open if unresponsive, and load-sheds non-critical requests when no endpoint has both KV-cache utilization under 80% and fewer than five queued requests [[inferencepool-kubernetes-gateway-api-inference-extension]] [[deep-dive-into-the-gateway-api-inference-extension-cncf]]. Intility notes what breaks classic gateway assumptions: responses are probabilistic, requests long-lived, usage must be measured rather than counted, and rate limiting must follow authentication [[how-to-build-an-inference-platform]].

The first fork is whether the engine is fronted, and it tracks whether a non-LLM serving platform already existed. Netflix routes LLM traffic through a shared Model Scoring Service with Triton underneath, because that path already served XGBoost, TensorFlow and PyTorch models and LLM models "should NOT be special snowflakes" [[in-house-llm-serving-at-netflix-by-ai-platforms-model-runtime-team-and-by-netfli]], while Thomson Reuters and T-Tech run vLLM directly on Kubernetes. Fronting has a priceable cost: Triton's vLLM backend compiles against a particular vLLM API surface, and Netflix had to pin versions when Triton 25.09 and vLLM 0.11.2 drifted apart.

The second fork is how weights reach memory. Streaming loaders attack load time: NVIDIA's benchmark of Run:ai Model Streamer loads a 15 GB model in 4.88 to 14.34 seconds across three storage tiers against 47–48 seconds for the default safetensors loader, which has no object-store path at all [[reducing-cold-start-latency-for-llm-inference-with-nvidia-runai-model-streamer-n]].

Pinned host memory suits restart-cascade risk: Thomson Reuters moved weights to an in-memory volume and cut cold start from "often 20 minutes or longer" to roughly five minutes, to stop slow replica starts cascading into failures [[thomson-continual-learning-of-frontier-models-for]]. Snowflake is the perimeter exception, co-engineering internal routing to replace egress proxies, with a stress test downloading a 700 GB model in 83 seconds over it [[llm-model-serving-faster-inference-on-snowflake]].

### B. KV-cache reuse, the dominant lever

Language models are stateless, so every turn resends the whole history and the engine recomputes the attention key-value cache for text it has already seen. Roughly 73% of conversations in a widely used public trace are multi-turn, and because engines discard idle sessions' caches, up to **99%** of multi-turn prefill cost is repeated computation [[cost-efficient-large-language-model-serving-for-multi-turn-conversations-with]].

The cheapest large win is routing to where the cache already is: Tesla measured a 3x improvement in output tokens per second and a 2x reduction in time-to-first-token after enabling prefix-cache-aware routing on a previously round-robin deployment [[production-grade-llm-inference-at-scale-with-kserve-llm-d-and-vllm-llm-d]], and Cloudflare raised its peak input-token cache hit ratio from 60% to 80% by persuading heavy internal users to send a session-affinity header [[building-the-foundation-for-running-extra-large-language-models]].

Offloading beyond one node comes next: LMCache manages the cache across accelerator memory, host memory, disk, object storage and network, reporting 3.7–6.8x lower time-to-first-token and 19–58% lower inter-token latency on two anonymized enterprise traces. Those gains hold only under specific bandwidth and context-length conditions, and context truncation halved one customer's prefix hit ratio from 85% to 45% [[lmcache-an-efficient-kv-cache-layer-for-enterprise-scale-llm-inference-2]]. Moonshot AI's account is the corrective: measured reuse on its own production traces is lower than open benchmarks imply, and its four-policy replay of one trace finds roughly a fifteenfold spread in tail time-to-first-token, citable as a relative comparison only, since its absolute figures use a stand-in model [[source-analysis-mooncake-a-kvcache-centric-disaggregated-architecture-for-llm-se]].

### C. Locality, fairness, and what utilization does not measure

Cache locality and load balance pull against each other: concentrating traffic raises hit rate and unbalances load, spreading it balances load and fragments the cache [[a-year-in-llm-serving-workload-evolution-caching]]. Locality-aware schedulers without fairness guarantees can impose up to 7.18x higher latency on well-behaved clients when a dominant client holds cache-resident state, which on a multi-tenant internal platform is a fairness problem presenting as a performance optimization [[locality-aware-fair-scheduling-in-llm-serving]]. Chunked prefill is the cheap answer to phase interference and disaggregating prefill from decode the expensive one, paying only under long prompts and high concurrency; the practitioner guidance is to fix routing before splitting pools [[distserve-disaggregating-prefill-and-decoding-for-goodput-optimized]] [[llm-serving-on-kubernetes-llm-d-aibrix-dynamo-compared]].

**Raw accelerator core-utilization metrics are near-useless for scaling**, because continuous batching keeps the device busy whenever any request is resident, so a pod with fifty queued requests reports roughly the same utilization as one with two [[llm-autoscaling-and-cold-starts-in-kubernetes-architecture-keda-metrics-model-we]]. Every account that describes its autoscaler uses engine-native metrics instead: the vLLM production stack scales on `vllm:num_requests_waiting` at five pending requests [[autoscaling-with-keda-production-stack]], and AIBrix replaces polling with sliding-window aggregation, reporting 11.5% lower latency and 33% fewer scaling oscillations than native horizontal pod autoscaling [[introducing-aibrix-cost-effective-and-scalable-control-plane-for-vllm-aibrix-blo]].

Whether reserved capacity sits idle is a separate question: a vendor survey of ~23,000 non-optimized clusters averages 5% accelerator utilization against 49% in one well-run cluster, a spread supporting the finding that reserved-and-idle is dearer [[cast-ais-2026-state-of-kubernetes-optimization-report-reveals-gpu-utilization-at]].

Rollout runs on the same signals: shadow mode scores a silent candidate against the incumbent on mirrored traffic, then a 1%→5%→20%→50% canary with session-level cohort stickiness and automatic rollback on p99 latency +40% or refusal rate +5 points. Pin an exact model digest — a `latest`-style alias is an implicit rollout that bypasses the canary gate [[releasing-ai-features-without-breaking-production-shadow-mode-canary-deployments]].

### D. *Observability* names two unrelated layers

Application tracing and evaluation — Langfuse (MIT except its enterprise directories), Arize Phoenix (Elastic Licence 2.0, not OSI-approved) and the Apache-2.0 libraries OpenLLMetry and OpenInference — is mature and adoptable as-is [[github-langfuselangfuse-open-source-agent-evals-observability-trace-evaluate-and]] [[github-arize-aiphoenix-ai-observability-evaluation-github]] [[github-traceloopopenllmetry-open-source-observability-for-your-genai-or-llm-appl]]. Engine telemetry is a different stack: vLLM's `/metrics` endpoint scraped by Prometheus, with llm-d shipping pod monitors and a PromQL library against those names, and none of the tracing products ingests it [[production-metrics-vllm]] [[interim-report-observability-layer-oss-maturity]].

The durable gap is cross-engine naming: OpenTelemetry's generative-AI metric conventions remain at development status with no KV-cache, queue-depth or batch-size metric, and vLLM's maintainers declined that path on label-cardinality grounds, the dual-emit implementation closed by its own author in September 2026 citing a lack of appetite "regardless of implementation approach" [[rfc-adopt-otel-genai-semantic-conventions-for-metrics-issue-51186-vllm-projectvl]] [[core-add-otel-genai-semantic-convention-metrics-dual-emit-by-shaurya856-pull-req]].

## 4. The Open-Source Landscape: Which Projects Fill Which Layer

Nearly every layer named above has at least one actively maintained open-source project behind it, and most have several. What distinguishes them is less feature coverage than governance — who maintains the project, what its roadmap commits to, and which capabilities sit behind a commercial licence.

| Project | Layer | Governance | Adoption evidence and known limitation |
|---|---|---|---|
| vLLM | Engine | Apache-2.0 | Default across nine first-person accounts; Python footprint limits untrusted multi-tenant use [[llm-serving-on-kubernetes-llm-d-aibrix-dynamo-compared]] |
| SGLang | Engine | PyTorch ecosystem | xAI, LinkedIn, Cursor; thin disclosure, divergent metric names [[sglang-joins-pytorch-ecosystem-efficient-llm-serving-engine-pytorch]] |
| TensorRT-LLM | Engine | NVIDIA open core | Atlassian; first-class in AIBrix v0.7.0; compilation slows iteration — Netflix left in 2025 |
| Text Generation Inference | Engine (historical) | Hugging Face | **Stale.** Maintenance mode 11 Dec 2025, archived 21 Mar 2026 [[github-huggingfacetext-generation-inference-large-language-model-text-generation]] [[10-best-vllm-alternatives-for-llm-inference-in-production-2026]] |
| Ollama / llama.cpp | Local and dev | Permissive | Tracks upstream `llama-server`; four parallel requests by default, erratic latency [[github-ollamaollama-get-up-and-running-with-kimi-glm-minimax-deepseek-gpt-oss-qw]] |
| llm-d | Control plane | Apache-2.0; CNCF Sandbox 12 Mar 2026; Red Hat, Google, IBM | Tesla via KServe, Hetzner, KAITO; its reference picker left neutral governance [[llm-serving-on-kubernetes-llm-d-aibrix-dynamo-compared]] |
| KServe `LLMInferenceService` | Control plane | CNCF Incubating | Tesla's hardening produced upstream fixes; built on llm-d, so adopts its scheduler [[understanding-llminferenceservice-kserve]] [[production-grade-llm-inference-at-scale-with-kserve-llm-d-and-vllm-llm-d]] |
| AIBrix | Control plane | Apache-2.0; 24 of 25 authors from one company | ByteDance production from early 2024, released Feb 2025; KV cache outside the shared ecosystem [[llm-serving-on-kubernetes-llm-d-aibrix-dynamo-compared]] |
| NVIDIA Dynamo | Control plane | Open core beside paid tiers | Rust core, cost-function KV router, SLO Planner; forked picker, vendor-blog figures [[router-design-nvidia-dynamo-documentation]] |
| vLLM production stack | Reference deployment | Apache-2.0, LMCache team | The documented starting point below control-plane scale [[github-vllm-projectproduction-stack-vllms-reference-system-for-k8s-native-cluste]] |
| Gateway API Inference Extension | Routing contract | `kubernetes-sigs`; `InferencePool` GA at v1 | Presented as the converging upward contract, though one practitioner account calls the reference implementation Alpha and not production-recommended [[beyond-gateway-inference-traffic-control-practice-with]] |
| Envoy AI Gateway | Gateway | Multi-vendor foundation; Bloomberg, Google | v1.0 GA 23 June 2026; spend governance is roadmap [[envoy-ai-gateway-reaches-v10-establishing-the-open-source-standard-for-enterpris]] |
| LiteLLM | Gateway, metering | Open core | Pfizer, in a joint vendor-and-customer debugging account; ~8ms p95 at 1,170 requests/second; team administration gated [[benchmarks-litellm]] [[role-based-access-controls-rbac-litellm]] |
| LMCache, Mooncake Store, Dynamo KVBM | KV-cache tier | Permissive | LMCache is the implementation inside the production stack, Dynamo, llm-d and KServe [[lmcache-an-efficient-kv-cache-layer-for-enterprise-scale-llm-inference-2]] |
| NeMo Guardrails, Llama Guard, Presidio | Guardrails | NVIDIA, Meta, Microsoft | 7.3k stars, 450+ dependents, in OpenShift AI 3.5 [[github-nvidia-nemoguardrails-nemo-guardrails-is-an-open-source-toolkit-for-easil]] [[developing-llm-guardrail-configs-locally-with-nemo-guardrails-red-hat-developer]] |
| garak, PyRIT | Red teaming | NVIDIA, Microsoft | 9.5k and 4.6k stars; scanners, not deployment gates [[github-nvidiagarak-the-llm-vulnerability-scanner-github]] [[github-microsoftpyrit-the-python-risk-identification-tool-for-generative-ai-pyri]] |
| promptfoo, lm-evaluation-harness | Eval tooling | MIT, permissive | 60+ benchmarks, built for CI gating; no organization documents either as a rollout gate [[github-eleutherailm-evaluation-harness-a-framework-for-few-shot-evaluation-of-la]] |
| Text Embeddings Inference, Infinity | Embedding, rerank serving | Apache-2.0, MIT | **Audited absence:** no named production adopter found; Infinity's last release August 2025 [[github-michaelfeilinfinity-infinity-is-a-high-throughput-low-latency-serving-eng]] |

Guardrails are used by composition rather than bespoke engineering: Presidio stripping personally identifiable information at ingress, a small injection classifier inline, NeMo Guardrails orchestrating dialog policy, a schema validator gating structured output, within a stated budget of 100–300 milliseconds [[ai-guardrails-implementing-safety-for-production-llm-apps]]. A Red Hat walkthrough gives the escalation honestly: a regular expression is defeated by a homoglyph substitution, and a 200M-parameter classifier scored 12 of 13 and still missed a subtle attack [[developing-llm-guardrail-configs-locally-with-nemo-guardrails-red-hat-developer]].

### A. What choosing a control plane commits you to

All four control planes launch vLLM or SGLang processes in pods, converge at the substrate and the upward API contract, and diverge at the layer most expensive to migrate off later: the routing decision and who governs it [[interim-report-control-plane-convergence-or-proliferation]]. The practitioner heuristic is to adopt a heavy control plane when at least two hold: multiple models sharing a pool, multiple teams needing isolation, a model too large for one node, a hard latency objective, or an idle-capacity bill someone has questioned [[llm-serving-on-kubernetes-llm-d-aibrix-dynamo-compared]]. Intility stayed below that line, choosing the Inference Extension's reference picker plus Envoy AI Gateway and writing no custom control-plane code [[how-to-build-an-inference-platform]].

The governance record is dated, specific, and the clearest live disagreement in the field. Google proposed moving the reference Endpoint Picker and body-based router out of the vendor-neutral `kubernetes-sigs` organization into llm-d's own repository. An Istio maintainer objected on the record that what llm-d does not provide is neutral governance, naming the risk that competing frameworks would face "starting from scratch or importing a competitor's EPP". The move completed in May 2026; NVIDIA responded by vendoring its own fork; Microsoft's attempt to migrate KAITO's default picker was auto-closed unmerged on 15 September 2026 [[proposal-to-move-epp-and-bbr-to-the-llm-d-repo-issue-2430-kubernetes-sigsgateway]] [[docs-add-design-proposal-for-migrating-epp-to-llm-d-inference-scheduler-by-andyz]].

One prediction from that thread has since been falsified: llm-d has not closed its extension points, and llm-d-router v0.11.0 (27 September 2026) shipped *new* scoring and filtering plugins [[release-v0110-llm-dllm-d-router-github-2]]. The surviving claim concerns authority rather than capability, and the trajectory is not monotonic — in the same window the gateway layer above moved into a neutral multi-vendor foundation [[istio-brings-future-ready-service-mesh-to-the-ai-era-with-new-ambient-multiclust]].

## 5. Where Teams Build Custom — and Why

What organizations write themselves is narrower than the build-versus-buy framing suggests, clustering in seams their own prior architecture created. Nobody writes an engine unless their operating environment is itself unusual, and the exceptions name their constraint.

| Organization | What was built in-house | Stated reason |
|---|---|---|
| Thomson Reuters | Rust routing, authorization and rate-limiting plane across 11 clusters; custom vLLM logits processors and reasoning parsers; a capability-tracking eval framework | Data-residency law, blast-radius containment, and no benchmark giving "a multifaceted view of model quality beyond aggregate accuracy" [[source-analysis-thomson-continual-learning-of-frontier-models-for-sovereignai]] |
| Uber | Go gateway mirroring the OpenAI API shape, PII redactor, audit logging; later an MCP Gateway | Team-by-team integration produced redundancy across 60+ use cases [[navigating-the-llm-landscape-ubers-innovation-with-genai-gateway]]; "MCP has no native concept of cross-server search" across 800+ servers [[designing-mcp-gateway-ubers-mcp-management-platform]] |
| Atlassian | AI Gateway plus SDK and CLI; model-status tracking; cross-vendor fallback; capacity pooling and quota | One API contract across vendors, centralized compliance, avoiding the noisy-neighbour effect [[atlassians-ai-gateway-best-in-class-model-garden-inside-atlassian]] |
| Netflix | Java control plane over Triton; a proxy merging two Prometheus sources; a patch for Triton's frontend silently dropping `response_format` | Consistency with a pre-existing company-wide scoring service [[in-house-llm-serving-at-netflix-by-ai-platforms-model-runtime-team-and-by-netfli]] |
| Cloudflare | Infire, a Rust engine | A Rust-everywhere standard, features "difficult if not impossible to upstream," and unwillingness to run untrusted Python on edge nodes [[how-we-built-the-most-efficient-inference-engine-for-cloudflares-network]] |
| Moonshot AI | Distributed KV-cache pool, global scheduler, RDMA transfer, chunked pipeline parallelism | "The open-source version of vLLM only supports local KVCache caching" — accurate in July 2024 [[source-analysis-mooncake-a-kvcache-centric-disaggregated-architecture-for-llm-se]] |
| ByteDance | AIBrix: LoRA-aware scheduling, three autoscaler strategies, own KV cache, internal metric store | Microservice frameworks "not designed for GPU-based inference workloads"; prior ML frameworks "lack deep integration with LLM inference engines" [[aibrix-towards-scalable-cost-effective-large-2]] |
| Snowflake | Egress proxy and internal network routing inside a container-services perimeter | Its own network perimeter, not a generic loading gap [[llm-model-serving-faster-inference-on-snowflake]] |
| T-Tech | Production error taxonomy; traffic-stratified benchmarks with deterministic verifiers and calibrated judges | The rubric is the organization's own traffic; the engine is unmodified vLLM [[from-production-traffic-to-post-training-building-a-self-hosted-llm]] |
| GovTech Singapore | Embedding server on Gunicorn, FastAPI and sentence-transformers | Predates dedicated embedding servers' maturity — weak evidence of a considered rejection [[productionising-large-language-models-in-government-by-ng-shangru-ai-practice-an]] |

One row is routinely misread. Netflix's metrics proxy is cited as evidence that open-source observability is immature, and does not support that: Triton's bridge surfaces only 9 of 40-plus vLLM metrics, an artifact of Netflix's own choice to front vLLM through Triton. vLLM's native endpoint exposes KV-cache utilization, prefix-cache hit rate, queue depth and token throughput in full, and llm-d's shipped Prometheus pipeline consumes all of it with no custom code [[metrics-llm-d]] [[interim-report-observability-layer-oss-maturity]].

### A. The counter-direction is equally well evidenced

Migrations *away* from custom code are costly, deliberate acts, which makes them unusually strong evidence about where the boundary sits. Uber deprecated its own Neuropod serving engine for Triton because Neuropod "lacks continuous community support," and replaced in-house hyperparameter tuning with RayTune [[from-predictive-to-generative-how-michelangelo-accelerates-ubers-ai-journey]]. Ollama removed its vendored GGML backend and custom Go model implementations, making upstream `llama-server` the sole engine for GGUF models [[github-ollamaollama-get-up-and-running-with-kimi-glm-minimax-deepseek-gpt-oss-qw]]. Tesla moved off a self-managed vLLM-in-StatefulSet deployment — network-storage drag, rigid node affinity, cache-blind round-robin — onto KServe with llm-d and Envoy AI Gateway, upstreaming its fixes [[production-grade-llm-inference-at-scale-with-kserve-llm-d-and-vllm-llm-d]]. Hetzner adopted llm-d alongside, not instead of, routing it had built itself [[what-we-learned-from-our-inference-experiment]].

The gateway build pattern is also not universal. Pfizer runs LiteLLM rather than building one, evidenced by a joint vendor-and-customer account of a Redis misconfiguration that cut its gateway throughput roughly in half with zero HTTP errors in application logs [[29-posts-tagged-with-engineering-litellm]]. That account is vendor-published and weighted accordingly, but it fixes the pattern's shape rather than damaging it: an organization without inherited gateway-shaped infrastructure debt adopting a stock gateway is what the pattern predicts.

### B. The quota layer, where the line is commercial

LiteLLM models tenancy as organization, team, user and key, attributing every request's spend to all four levels at once and blocking when any level on the path is over budget [[multi-tenant-architecture-with-litellm-litellm]]. Its role matrix places organization and team administrator roles behind the premium tier, and tag-based budgets are enterprise too; a request that accepted the enterprise gate and asked only for an adjacent open-source field was auto-closed "not planned," while the same project moved other enterprise features into open source, including MCP gateway cost tracking [[role-based-access-controls-rbac-litellm]] [[enterprise-litellm]] [[internal-user-self-serve-litellm]] [[mcp-gateway-move-cost-tracking-and-permission-management-to-oss-by-jugaldb-pull]]. The boundary here is a series of revisable commercial choices rather than a fixed architectural line.

Where enforcement is documented at all, it comes from vendors rather than operators — soft alerts at 80%, hard limits returning HTTP 429 [[llm-cost-attribution-and-team-budget-tracking]] [[gpu-cloud-finops-for-ai-teams-cost-allocation-per-project-chargeback-and-tag-bas]]. Absent is any named organization describing an actual currency transfer between internal teams — Uber generates audit logs for attribution, Atlassian tracks cost alongside rate limiting, a government department describes sharing infrastructure to optimize cost [[using-self-hosted-large-language-models-llms-securely-in-government-digital-trad]].

## 6. Reference Stacks: Named Organizations and Their Disclosed Architectures

The cases below described their own stacks in their own words. Each row states only what that organization confirmed; blanks are non-disclosure rather than inference.

| Organization | Engine | Orchestration | Gateway or router | Disclosed scale or outcome |
|---|---|---|---|---|
| Thomson Reuters | vLLM, customized | Kubernetes; 11 clusters, 4 geographies, 2 clouds | Rust routing, authorization, rate limiting | 40M+ requests and 370B tokens since Apr 2026, upwards of 99.9% success [[thomson-continual-learning-of-frontier-models-for]] |
| T-Tech | vLLM, unmodified, FP8 | Kubernetes; one replica per pod | Internal platform fronting 200+ services | 116M requests/month, 110 peak requests/second, p95 3.2s [[from-production-traffic-to-post-training-building-a-self-hosted-llm]] |
| Netflix | vLLM, from TensorRT-LLM in 2025, fronted by Triton | Java control plane in a shared Model Scoring Service | gRPC plus OpenAI-compatible HTTP | Workload breadth drove engine choice [[in-house-llm-serving-at-netflix-by-ai-platforms-model-runtime-team-and-by-netfli]] |
| Atlassian | Triton with TensorRT-LLM and vLLM backends | Helm, ArgoCD, Kustomize; Karpenter; Istio; KEDA | In-house AI Gateway over a 20+ model garden | Up to 40% P-90 latency and >60% cost reduction vs third-party hosting; 72% latency reduction on one query-rewriting workload specifically [[atlassians-inference-engine-our-self-hosted-ai-inference-service-inside-atlassia]] [[atlassians-ai-gateway-best-in-class-model-garden-inside-atlassian]] |
| Tesla | vLLM | KServe plus llm-d, after a raw StatefulSet | Envoy AI Gateway, Inference Extension | 3x output tokens/second, 2x lower time-to-first-token [[production-grade-llm-inference-at-scale-with-kserve-llm-d-and-vllm-llm-d]] |
| Uber | Not named for in-house LLM serving | Michelangelo; Triton; Ray trainers | Go GenAI Gateway; separate MCP Gateway | ~30 customer teams, 16M queries/month, 60+ identified use cases [[navigating-the-llm-landscape-ubers-innovation-with-genai-gateway]] |
| Intility | vLLM | Talos Linux, Kubernetes, GitOps, shared volume | Inference Extension picker plus Envoy AI Gateway | No custom control-plane code — the cleanest open-source case [[how-to-build-an-inference-platform]] |
| Cloudflare | Infire (own Rust engine) plus vLLM elsewhere | Edge nodes; prefill and decode disaggregated | Internal KV-aware routing plus a session header | p90 time-per-token from ~100ms to 20–30ms [[building-the-foundation-for-running-extra-large-language-models]] |
| Snowflake | vLLM | Container services, network-isolated | OpenAI protocol via Python, SQL and REST | Stress test: 700 GB model downloaded in 83 seconds [[llm-model-serving-faster-inference-on-snowflake]] |
| GovTech Singapore | TGI (2023, now stale) | On-premise for classified workloads | Not disclosed | Built its own embedding server [[productionising-large-language-models-in-government-by-ng-shangru-ai-practice-an]] |
| Harvey, EliseAI, Hebbia, Amplitude | Provider-operated | Provider-operated | Application-level routing | Harvey ~2x base-model task completion [[harvey-tenet-research-preview]]; Amplitude 73.7 vs 72.7 at ~one-third cost [[practical-guide-to-evaluating-open-models-achieving-sonnet-level-performance-wit]] |
| Pfizer | Not disclosed | Not disclosed | LiteLLM, adopted | Gateway throughput incident resolved by bisection [[29-posts-tagged-with-engineering-litellm]] |

Disclosure is unevenly distributed, and the unevenness is informative. No commercial bank, insurer or healthcare payer in this record names itself or describes a serving stack — not in engineering blogs across eight named banks and fintechs, not in the supervisory surveys searched, and not in the sample of filings checked, which answers the objection that the search looked in the wrong place.

The Bank of England and FCA joint survey establishes that 75% of UK financial-services firms use AI, up from 58% in 2022, and mentions inference, self-hosting, open source or vLLM zero times in 4,499 words [[artificial-intelligence-in-uk-financial-services-2024-bank-of-england-the-uks-ce]]. The ECB's anonymized 2025 annex records "self-hosted (LLM) models on private cloud or with distant back-up locations" across its 13-bank sample with no technical detail [[wwwecbeuropaeu]] [[ais-impact-on-banking-use-cases-for-credit-scoring-and-fraud-detection]], while two government bodies under constraints at least as strict published detailed first-person architecture [[using-self-hosted-large-language-models-llms-securely-in-government-digital-trad]] [[productionising-large-language-models-in-government-by-ng-shangru-ai-practice-an]]. That asymmetry points to disclosure incentive rather than confidentiality, and narrows the finding to regulated *commercial* industry.

## 7. Where the Open-Source / Custom Boundary Falls

Open source now covers most of these layers; the pattern is less about which project is good enough than about four forces, only one of which closes with software maturity.

| Layer | Where it stands | Basis |
|---|---|---|
| Inference engine | **Covered**, settled | Nine first-person adopter accounts; exceptions name unusual operating constraints |
| Control plane | **Covered**, as of 2026 | The capability list AIBrix called missing in Feb 2025 ships inside CNCF-governed KServe by Mar 2026 [[announcing-kserve-v017-production-ready-llm-serving-with-llminferenceservice-kse]] |
| Distributed KV cache | **Storage covered**; cache-aware routing re-implemented per stack | Three independently funded implementations within ~12 months; the cross-stack block-attribution seam is an open RFC |
| Guardrails | **Covered by composition** | See §4 |
| Eval tooling / eval content | **Covered** / **in-house, permanently** | Harnesses are open; the rubric is the organization's own definition of shippable |
| Gateway mechanics | **Covered without integration debt** (Intility, Pfizer); **in-house with it** (Uber, Atlassian, Thomson Reuters, Netflix) | The debt is non-transferable |
| Observability — app tracing | **Covered**, adopt as-is | Langfuse, Phoenix, OpenLLMetry |
| Observability — cross-engine metrics | **Open deliberately**, not immaturely | Conventions at development status; vLLM's request open on a cardinality objection [[core-add-otel-genai-semantic-convention-metrics-dual-emit-by-shaurya856-pull-req]] [[interim-report-observability-layer-oss-maturity]] |
| Chargeback data / enforcement | **Covered** / **gated, no roadmap pressure** | Virtual keys joined to token counters; the tag-budget request auto-closed "not planned" |
| Gateway governance | **Unsettled** | See §4.A |

### A. Four factors, and why two of them do not close

**Capability-gap recency** predicts movement rather than position, and movement is fast: every cleanly dated gap here closed within **4 to 18 months** — Moonshot's 2024 cache-pool complaint absorbed by LMCache, AIBrix's early-2025 integration complaint absorbed by KServe thirteen months later, and llm-d's Kubernetes-only limitation filed 16 May 2026 and shipped in v0.8.0 on 23 June 2026, though no operator account of running it off Kubernetes exists yet, so shipping and reliance stay separate claims [[interim-report-oss-boundary-sharpening]].

**Inherited infrastructure** predicts where residual custom code sits, and beats the pure capability story on every surviving example in §5's table: each trades a small technical win for integration with something older than the LLM workload [[interim-report-oss-boundary-sharpening]].

**Policy ownership** is non-transferable by construction: *where a given request may legally be served*, *who may spend what* and *what counts as shippable* are not capabilities any project can supply, which is why Thomson Reuters' routing plane is bespoke by construction rather than by capability shortfall [[thomson-continual-learning-of-frontier-models-for]].

**Supply-side incentives** form a separate axis — a component can be fully open-source-capable and still force a build-or-pay-or-depend-on-a-competitor decision, the quota layer being the monetization case and the endpoint picker the governance case. This is the thinnest finding here, resting on recent issue threads rather than a trend, with one attached prediction already falsified (§4.A).

Two independent analyses in this work reached the inherited-infrastructure explanation by different routes — one by dating capability closures and finding the residuals all organization-specific, the other by assembling six first-person deployments and noticing the forks sorted by company history rather than by which option benchmarked better [[interim-report-oss-boundary-sharpening]]. A deliberate search for the decisive counter-example, an organization retiring a bespoke *policy* layer because open source absorbed its rules, returned nothing. Both are syntheses with two to three data points per fork.

### B. The time dimension

The boundary reads better as a moving front than a fixed line, because the open-source serving layer here is substantially internal systems released outward rather than community projects adopted inward. AIBrix ran inside ByteDance from early 2024 before its February 2025 release [[introducing-aibrix-a-scalable-cost-effective-control-plane-for-vllm-vllm-blog]]; Mooncake was Moonshot's production system, built around a gap the ecosystem then closed; Envoy AI Gateway began as Bloomberg's routing problem and reached a neutral foundation in 2026 [[envoy-ai-gateway-reaches-v10-establishing-the-open-source-standard-for-enterpris]]; llm-d was launched by Red Hat, Google and IBM Research [[llm-serving-on-kubernetes-llm-d-aibrix-dynamo-compared]]. The question about a layer is therefore not whether it is covered today but how long a gap of that shape has historically stayed open.

Three forces will move the line over the next eighteen months in different directions:

- **Commoditization** keeps running where several funded efforts chase one gap.
- **Governance and monetization boundaries** are hardening rather than resolving, being supply-side decisions rather than gaps adopters wait on.
- **The workload is shifting toward agentic traffic**, where instrumented measurement of ten applications found non-LLM components dominating end-to-end latency in 5 of 10 and sessions holding state idle for minutes to hours, naming a lifecycle state the serving layer lacks [[from-llm-inference-to-agentic-workloads]].

Uber's MCP Gateway is that pattern appearing at a new layer [[designing-mcp-gateway-ubers-mcp-management-platform]].

### C. Four tests, and a limitation

For a reader deciding whether to run inference internally, four tests serve better than a threshold.

- Is the workload stable enough that a commitment amortizes against a fixed target?
- Will capacity be shared across consumers or reserved for one, the variable that flipped the cost case (§1.B)?
- Is the quality delta measured and priced, or only the token cost (§2)?
- Is the organization carrying infrastructure or policy debt an off-the-shelf gateway cannot absorb — because if not, the cleanest cases in the published record adopted rather than built.

Confidence is higher for engines and control planes, where evidence is first-person and abundant, than for chargeback enforcement, release gating, application tracing and embedding serving — the layers to expect to build today because disclosure, not capability, is what is missing; a commercial gateway maintaining a paid cross-team cost-attribution tier implies someone is buying it [[interim-report-under-documented-layers-audited-absence]].
