---
title: 'LLM Serving on Kubernetes: llm-d, AIBrix, Dynamo Compared'
id: llm-serving-on-kubernetes-llm-d-aibrix-dynamo-compared
tags:
- internal-llm-inference-stacks-7f0c90
- llm-d
- aibrix
- nvidia-dynamo
- layer-boundaries
- gateway-api-inference-extension
created: '2026-10-09T22:53:45.371408Z'
updated: '2026-10-10T14:52:17.333921Z'
source: https://builderai.tools/blog/serving-llms-on-kubernetes-llm-d-aibrix-dynamo
source_domain: builderai.tools
fetched_at: '2026-10-09T22:53:45.344016Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Dense practitioner comparison (mid-2026, builderai.tools) of the Kubernetes
  control-plane layer, explicit about layer boundaries: ''Kubernetes LLM serving in
  mid 2026 splits into three layers: an engine that runs the model, a control plane
  that decides how many replicas exist and where each request lands, and a gateway
  that understands what a KV cache is.'' States flatly: ''none of the control planes
  discussed below replace the engine. llm-d, AIBrix and Dynamo all launch vLLM or
  SGLang processes in pods and orchestrate around them.'' Documents the Gateway API
  Inference Extension as the converging standard layer ABOVE all three control planes:
  GA, v1 InferencePool API, Endpoint Picker (EPP) extension reading model-server metrics
  for KV-cache-locality/load/priority placement; InferenceObjective config object
  ''has moved into the llm-d Router repository'' -- ''the routing contract is becoming
  portable even though the control planes are not.'' Per-project governance/backing
  with specific dates: llm-d is Apache 2.0, ''launched in May 2025 by Red Hat, Google
  Cloud, IBM Research, CoreWeave and NVIDIA, later joined by AMD, Cisco, Hugging Face,
  Intel, Lambda and Mistral AI,'' accepted into CNCF Sandbox 12 March 2026, announced
  at KubeCon Europe 24 March. AIBrix is Apache 2.0, lives inside vllm-project GitHub
  org, ''built by a team led by ByteDance engineers,'' first public release Nov 2024,
  white paper Feb 2025, v0.7.0 (18 June 2026) had 242 merged PRs. NVIDIA Dynamo is
  Apache 2.0, Rust core + Python extensibility, v1.3.0 as of 22 July 2026, ''the most
  explicitly datacenter-scale of the three,'' built for NVL72-class hardware with
  contractual latency SLAs via Grove (alpha, Volcano scheduler). Names llm-d''s production-adjacent
  integration point: ''If you already run KServe... KServe''s LLMInferenceService
  CRD is described in its own documentation as built on the foundation of llm-d.''
  Documents the pre-control-plane capability gap directly: cache-aware routing fixes
  what ''round robin across replicas is actively hostile to'' (prefix cache fragmentation);
  disaggregation addresses prefill(compute-bound)/decode(memory-bound) interference
  that ''no amount of extra GPUs fixes'' when colocated. Gives a concrete adoption
  heuristic: ''adopt a control plane when at least two of the following are true,
  not one: multiple models sharing a GPU pool, multiple teams needing isolation/fairness,
  a model too large for one node, a hard TTFT/ITL SLO, an idle-GPU bill large enough
  that someone has asked about it'' -- implying a single vLLM box behind a plain Kubernetes
  Service is the default and these three control planes are deliberate escalations,
  not defaults. Comparison table covers 6 stacks (llm-d, AIBrix, Dynamo, GPUStack,
  Xinference, OpenLLM) across license/engines-driven/PD-disaggregation/cache-routing/K8s-requirement/operational-weight.'
utility_score: 16.0
---

Kubernetes LLM serving in mid 2026 splits into three layers: an engine that runs the model, a control plane that decides how many replicas exist and where each request lands, and a gateway that understands what a KV cache is. This article covers the control plane layer, [llm-d](https://builderai.tools/tool/llm-d), [AIBrix](https://builderai.tools/tool/aibrix), [NVIDIA Dynamo](https://builderai.tools/tool/nvidia-dynamo), [GPUStack](https://builderai.tools/tool/gpustack), [OpenLLM](https://builderai.tools/tool/openllm), and [Xinference](https://builderai.tools/tool/xinference), how each handles prefill and decode disaggregation and cache aware routing, and the cases where one box running [vLLM](https://builderai.tools/tool/vllm) still wins on every axis that matters.
## The layer cake most diagrams get wrong
At the bottom are the engines. [vLLM](https://builderai.tools/tool/vllm) and [SGLang](https://builderai.tools/tool/sglang) are the two that essentially everything else in this article builds on. vLLM shipped v0.26.0 on 27 July 2026; the v0.25.0 release a couple of weeks earlier made Model Runner V2 the default for all dense models and deleted PagedAttention, closing out the legacy attention path. SGLang shipped v0.5.16 two days before that vLLM release, making UnifiedRadixTree the default for SWA, Mamba and DSA models and adding DSpark, a confidence-driven speculative decoding algorithm you opt into with `--speculative-algorithm DSPARK`. [TensorRT-LLM](https://builderai.tools/tool/tensorrt-llm) and [LMDeploy](https://builderai.tools/tool/lmdeploy) round out the engine tier for NVIDIA-heavy shops, and [Triton Inference Server](https://builderai.tools/tool/triton-inference) is still the general purpose model server underneath a lot of older production graphs.
The important structural fact: none of the control planes discussed below replace the engine. llm-d, AIBrix and Dynamo all launch vLLM or SGLang processes in pods and orchestrate around them. If your engine configuration is wrong, no control plane saves you.
At the top is the gateway, and this is where Kubernetes has actually standardized something. The Gateway API Inference Extension defines an `InferencePool`, a set of pods sharing the same accelerator type, base model and server config, paired with an Endpoint Picker extension (EPP) that reads metrics emitted by the model servers and places requests by KV cache locality, current load and priority. The project has declared itself GA and ships a v1 API. Its companion `InferenceObjective`, which configures scheduling goals such as priority level and performance targets for a class of requests, has moved into the llm-d Router repository along with the core EPP code. Gateway API has more than 25 implementations and inference gateway support is spreading across them: llm-d ships with Istio, Dynamo has moved to agentgateway. The routing contract is becoming portable even though the control planes are not.
## Disaggregation and cache aware routing, in that order of hype
### Prefill and decode want different machines
Prefill processes the whole prompt in one pass and is compute bound. Decode emits one token at a time per sequence and is memory bandwidth bound. Run them in the same engine process and they interfere: a long prompt arriving mid-stream stalls every sequence currently decoding, which shows up as inter-token latency spikes that no amount of extra GPUs fixes.
Chunked prefill was the first mitigation and it remains the correct answer on a single node. Instead of one blocking pass over a 30k token prompt, the engine breaks it into fixed size chunks and interleaves them with ongoing decode steps. It costs you nothing operationally.
Disaggregation goes further: separate pools of prefill workers and decode workers, with KV blocks shipped between them. vLLM V1 exposes this through a connector interface, `KVConnectorBase_V1`, with a `NixlConnector` that moves blocks over NVIDIA's NIXL transfer library and a `MultiConnector` for composing more than one. The same tree carries LMCache, Mooncake, hf3fs and CPU offloading connectors. That design is why third party stacks can add disaggregation without forking vLLM.
Disaggregation pays when prompts are long, concurrency is high, and you care more about throughput and stable inter-token latency than about the last few tens of milliseconds of time to first token. Agentic and RAG traffic with reused long system prompts is the archetype. It does not pay for short prompts, where prefill is cheap and the KV hop is pure overhead; for concurrency in the single digits, where batching gains never materialize; or when TTFT is your hard SLO and the network transfer adds real latency before decode can start. The llm-d project frames P/D disaggregation as a long context and GPU utilization play, not a universal default.
### Cache aware routing is the cheaper win
Before you split pools, fix routing. Round robin across replicas is actively hostile to prefix caching: an identical system prompt gets scattered across every pod, so each one recomputes it. Prefix cache aware routing sends a request to the replica that already holds the matching blocks. It requires no new pools, no KV transport, and no topology change.
The three serious control planes each do this differently.
  * The llm-d Router scores endpoints on prefix cache locality and load through a plugin chain of filters and scorers. Two of its well-lit paths matter here: precise prefix cache routing, which uses a real-time index of vLLM KV state rather than a hash-based approximation, and predicted latency based routing, which scores endpoints against a live-trained XGBoost model of request latency instead of static heuristics.
  * AIBrix ships routing as selectable gateway strategies: `prefix-cache`, `prefix-cache-preble` (based on the Preble scheduling paper, scoring both cache hit and pod load), `least-kv-cache`, `least-request`, `throughput`, `vtc-basic` (a simplified Virtual Token Counter variant that balances per-user token fairness against pod utilization), plus a family of `slo-*` strategies and `session-affinity`.
  * Dynamo's KV-aware Router scores worker load and KV cache overlap together to skip redundant prefill. NVIDIA's own materials claim roughly 2x faster time to first token from routing alone, measured on Qwen3-Coder 480B.


If multi-tenant LoRA is the actual problem, AIBrix's high-density adapter management (extended with SGLang lifecycle support in v0.6.0) is the closest fit among the control planes, and [LoRAX](https://builderai.tools/tool/lorax) is worth a look if adapters are the whole workload rather than a feature of it.
## llm-d: the CNCF reference stack
llm-d is Apache 2.0, launched in May 2025 by Red Hat, Google Cloud, IBM Research, CoreWeave and NVIDIA, later joined by AMD, Cisco, Hugging Face, Intel, Lambda and Mistral AI. It was accepted into the CNCF at Sandbox level on 12 March 2026, announced publicly at KubeCon Europe on 24 March.
The project's organizing idea is the "well-lit path": a documented and tested recipe rather than a pile of knobs. The current set covers an optimized baseline, predicted latency based routing, precise prefix cache routing, tiered prefix cache, prefill/decode disaggregation, wide expert parallelism for MoE models like DeepSeek-R1, flow control, workload autoscaling, rollouts, fast model actuation, agentic serving and multimodal serving. Asynchronous processing, the batch gateway and encode disaggregation sit in an explicitly experimental tier.
v0.7.0, released 12 May 2026, was a hardening release: standalone mode became the default so you can deploy behind a generic proxy without the full gateway configuration, installation moved from Helm to a kustomize-first layout, and the CUDA base moved to 13.0.2, which imposes a hard floor of NVIDIA driver 580 or later. It pinned vLLM 0.19.1 and LMCache 0.4.4-cu13, shipped a dedicated GB200 image, and added AWS EFS as a tiered prefix cache target.
v0.8.0 landed 24 June 2026, moving to vLLM 0.23.0 and graduating multimodal serving and flow control to production. It added multi-tier KV offloading with heterogeneous memory allocation, DP-aware scheduling, a Mooncake connector, and, notably, a non-Kubernetes mode aimed at reinforcement learning and Slurm environments. Names changed with it: the Inference Scheduler is now the llm-d Router, its component image is `endpoint-picker`, and the routing sidecar became `disagg-sidecar`. v0.8.1 followed two days later as a patch.
Accelerator coverage is the widest of any stack here: NVIDIA including a dedicated GB200 build, AMD ROCm, Intel XPU and Gaudi, Google TPU, and CPU, with AWS EFA support in the AWS image.
If you already run [KServe](https://builderai.tools/tool/kserve), you may get llm-d without adopting it directly. KServe's `LLMInferenceService` CRD is described in its own documentation as built on the foundation of llm-d, and exposes KV cache aware scheduling and disaggregated prefill/decode serving through a resource your platform team already understands.
The honest caveat is surface area. v0.7.0 came with a wholesale documentation restructure around those well-lit paths. That is a signal of care, and also a signal of how much there is to learn.
## AIBrix: batteries included, from ByteDance
AIBrix is Apache 2.0 and lives inside the `vllm-project` GitHub organization, built by a team led by ByteDance engineers. Its first public release shipped in November 2024 and its design was written up as a white paper in February 2025. v0.7.0 shipped on 18 June 2026 with 242 merged PRs behind it. Installation is three manifests against an existing cluster:

```
kubectl apply -f "https://github.com/vllm-project/aibrix/releases/download/v0.7.0/aibrix-dependency-v0.7.0.yaml" --server-side
kubectl apply -f "https://github.com/vllm-project/aibrix/releases/download/v0.7.0/aibrix-core-crds-v0.7.0.yaml" --server-side
kubectl apply -f "https://github.com/vllm-project/aibrix/releases/download/v0.7.0/aibrix-core-v0.7.0.yaml"

```

Autoscaling is where AIBrix is most opinionated. Its `PodAutoscaler` CRD supports three strategies: standard Kubernetes HPA, a Knative-derived KPA, and APA, its own variant. KPA maintains a long stable window and a short panic window, with a configurable `panic-threshold`, so a traffic spike triggers fast scale-out without the stable window damping it. Critically, AIBrix fetches and maintains engine metrics internally rather than round-tripping through Prometheus, which its documentation credits directly for faster reaction times. APA is the LLM-specific variant, aimed at stopping latency-sensitive services from oscillating, and KV cache utilization is a first class scaling target: `gpu_cache_usage_perc` and `gpu_kv_cache_utilization` both work.
For topology, AIBrix exposes StormService, which orchestrates prefill/decode disaggregated deployments through RoleSets, where each role is prefill or decode and owns its own pods. Replica mode treats a whole RoleSet as the unit; pooled mode treats each role as an independently scalable pool. v0.5.0 added a `subTargetSelector` field to the PodAutoscaler API so a single role inside a StormService can be scaled on its own. RayClusterFleet handles multi-node inference where the model does not fit on one host, and KubeRay is optional if you do not use it.
v0.6.0 (3 March 2026) broadened the API surface well past chat completions: audio transcription and translation, image and video generation, and rerank endpoints, plus session affinity and external header filters for routing. v0.7.0 (18 June 2026) made TensorRT-LLM a first class engine alongside vLLM and SGLang, unified the KV data plane with zero-copy APIs and pluggable cross-engine routing, added a gateway that syncs routing state across replicas through Redis with per-model rate limiting and graceful shutdown, shipped a production OpenAI-compatible Batch API, and added a web management console plus a resource manager for cloud GPU providers such as Lambda Cloud and RunPod. The console and resource manager are brand new in that release; treat them accordingly.
The other two features worth naming: GPU hardware failure detection, and cost-efficient heterogeneous serving, which the project describes as mixed GPU inference with SLO guarantees.
## NVIDIA Dynamo: datacenter scale, Rust core
Dynamo is Apache 2.0, at v1.3.0 as of 22 July 2026, and built in Rust for performance with Python for extensibility. It is the most explicitly datacenter-scale of the three.
The architecture is a handful of named pieces. Frontend serves an OpenAI-compatible HTTP API, now with configurable tool-calling and reasoning parsers aligned to vLLM and SGLang. The KV-aware Router places requests by worker load and KV cache overlap; v1.3.0 gave it a standalone selection service, a branch-sharded KV indexer and topology-aware routing. Workers run the engine. Planner is an SLA-driven autoscaler that right-sizes prefill and decode pools against latency targets rather than utilization, and v1.3.0 swapped its hand-tuned models for AIConfigurator latency prediction. KV Block Manager (KVBM) offloads cache across GPU, CPU, SSD and remote storage, including S3 and Azure blob, which is how you serve context longer than GPU memory allows. NIXL handles transfer between GPUs and storage tiers, and ModelExpress streams model weights between GPUs; NVIDIA cites roughly 7x faster model startup from that path, measured on DeepSeek-V3 on H200.
Backends are vLLM, SGLang and TensorRT-LLM, all three with disaggregation, KV-aware routing and multimodal support. v1.3.0 is CUDA 13 only: the CUDA 12 container images are gone. Getting started is a pip install:

```
uv pip install --prerelease=allow "ai-dynamo[sglang]"

```

On Kubernetes, Dynamo ships an operator with `DynamoGraphDeployment` (now at `v1beta1`, with admission webhooks), `DynamoComponentDeployment` and `DynamoGraphDeploymentRequest` CRDs, the last of which lets you declare a model, a backend and SLA targets and have the stack size itself. Grove is the companion operator for topology-aware scheduling, built for NVLink-connected racks like GB200 NVL72; it is still early, integrated at v0.1.0-alpha with Volcano scheduler support. Dynamo has also replaced its standalone inference-gateway Helm chart with agentgateway and the Gateway API Inference Extension, so it can slot under the same gateway contract as llm-d.
Dynamo is the heaviest option here and it is aimed squarely at people running MoE models across NVL72-class hardware with contractual latency targets. If that is not you, the operational cost is hard to justify.
## How the six compare  
| Stack  | License  | Engines it drives  | P/D disaggregation  | Cache aware routing  | Kubernetes  | Operational weight  |  
| --- | --- | --- | --- | --- | --- | --- |  
| llm-d  | Apache 2.0  | vLLM  | First class, dedicated guide  | Router scoring plus precise prefix index  | Required  | High  |  
| AIBrix  | Apache 2.0  | vLLM, SGLang, TensorRT-LLM  | First class, unified KV data plane  | Pluggable strategies, prefix cache and fairness  | Required  | Medium to high  |  
| NVIDIA Dynamo  | Apache 2.0  | vLLM, SGLang, TensorRT-LLM  | Core design point  | KV-aware Router on load plus cache overlap  | Operator and Grove, also runs off cluster  | High  |  
| GPUStack  | Apache 2.0  | vLLM, SGLang, TensorRT-LLM, custom  | Delegated to the engine  | Engine level only  | Optional, K8s is one deployment target  | Low  |  
| Xinference  | Apache 2.0  | vLLM, llama.cpp, transformers  | Not offered  | Engine level only  | Optional, Helm charts exist  | Low  |  
| OpenLLM  | Apache 2.0  | vLLM  | Not offered  | Engine level only  | Via BentoML deploys  | Low  |  
## Autoscale on queue depth, not GPU utilization
GPU utilization as reported by the usual exporters is close to useless for inference: it reads near 100 percent whenever a kernel is resident, regardless of whether the batch is full. Scale on engine metrics instead.
The primary trigger is `vllm:num_requests_waiting`, the count of requests waiting to be processed. It rises before latency degrades, which is exactly what you want from a scale-out signal. `vllm:num_requests_running` gives you in-flight concurrency, and `vllm:kv_cache_usage_perc` gives you a saturation ceiling. If your platform can act on percentiles, TTFT and inter-token latency are better still, which is the argument behind Dynamo's Planner.
The standard wiring on vanilla Kubernetes is KEDA with a Prometheus scaler driving replica count, and Cluster Autoscaler or Karpenter driving GPU node count underneath it. KEDA also gives you scale to zero, which matters more than it sounds for dev and staging pools. AIBrix skips Prometheus entirely for its own internal metric store, and llm-d has a workload autoscaling well-lit path keyed on queue depth and KV cache pressure.
Cold start is the constraint that ruins naive autoscaling. Pulling a 70B checkpoint onto a fresh node takes minutes, so by the time the replica is ready the spike is over. The mitigations, in rough order of effort: node-local model caches and pre-pulled images, weight streaming (Dynamo's ModelExpress, llm-d's fast model actuation), and shrinking the checkpoint with [llm-compressor](https://builderai.tools/tool/llm-compressor) so there is less to move. If you burst across clouds or chase spot capacity, [SkyPilot](https://builderai.tools/tool/skypilot) handles the provisioning side without your control plane needing to know.
## Multi node without a Kubernetes budget
Not every team that needs multiple GPU hosts needs an operator and a gateway.
[GPUStack](https://builderai.tools/tool/gpustack) is Apache 2.0, at v2.2.2 as of 24 July 2026, and starts with one container:

```
sudo docker run -d --name gpustack \
    --restart unless-stopped \
    -p 80:80 \
    --volume gpustack-data:/var/lib/gpustack \
    gpustack/gpustack

```

It pools GPUs across hosts, automatically configures vLLM, SGLang, TensorRT-LLM or a custom engine, and can also hand out SSH-accessible GPU instances on demand for development and fine-tuning. Its differentiator is vendor spread: NVIDIA GPU, AMD GPU, Ascend NPU, Hygon DCU, MThreads GPU, Iluvatar GPU, MetaX GPU, Cambricon MLU and T-Head PPU, with engine builds pinned per accelerator toolkit. v2.2.0 shipped CUDA 13.0 and 12.9, ROCm 7.2 and 7.0, and CANN 9.0 and 8.5 variants, deprecated CUDA 12.6 and ROCm 6.4, added Helm chart support for Kubernetes, and added vLLM's MP distributed mode, so multi-node serving no longer forces a Ray cluster on you. v2.2.2 itself is a security release, so do not sit on an older 2.2.x.
[Xinference](https://builderai.tools/tool/xinference) is Apache 2.0 and reached 3.0.0 on 19 July 2026. That release switches the default frontend to a Next.js app and turns on database-backed authentication by default, and it ships a documented migration and breaking changes guide, so read the notes before upgrading.

```
pip install "xinference[all]"
xinference-local --host 0.0.0.0 --port 9997

```

For a cluster it runs a supervisor plus workers, and there is a separate Helm charts repository at `xorbitsai.github.io/xinference-helm-charts` if you do want it on Kubernetes. The reason to choose it is breadth rather than scale: language models, embeddings, rerankers, speech, image and multimodal models all behind one OpenAI-compatible API. If your product needs a chat model, an embedder and a speech endpoint and you would rather run one service than four, this is the shortest path.
[OpenLLM](https://builderai.tools/tool/openllm) is Apache 2.0 and takes the narrowest scope: `pip install openllm`, then `openllm serve llama3.2:1b` to run a supported open model as an OpenAI-compatible endpoint with a chat UI at `/chat`. vLLM is the backend, [BentoML](https://builderai.tools/tool/bentoml) is the packaging and serving layer, and a model repository at `bentoml/openllm-models` covers the common builds. Because the output is a Bento, deployment targets are Docker, Kubernetes or BentoCloud. It is the right answer when the goal is an endpoint this afternoon rather than a platform this quarter.
[Ray Serve](https://builderai.tools/tool/ray-serve) with KubeRay is the fourth path and deserves a mention: Python-first composition, an engine-agnostic architecture covering vLLM and SGLang, unified multi-node multi-model deployment, multi-LoRA on shared base models, and prefill-decode disaggregation. Recent work on direct streaming and a v2 Ray executor in vLLM closed most of the orchestration overhead gap it used to carry.
Whatever sits underneath, [LiteLLM](https://builderai.tools/tool/litellm) in front is a cheap way to get per-team keys, budgets, and fallback to hosted models without touching the serving layer.
## When one vLLM box is still the right answer
A single well-configured node beats a cluster whenever the cluster's advantages do not apply. The checklist:
  * The model fits on one node with tensor parallelism you can afford.
  * Peak concurrency is in the tens, not the hundreds.
  * Prompts are short enough that prefill is not dominating.
  * One model, one team, one SLO.


```
vllm serve Qwen/Qwen3-32B \
  --tensor-parallel-size 2 \
  --max-model-len 32768 \
  --port 8000

```

Prefix caching and chunked prefill are handled inside the engine, and with one replica the entire routing problem disappears, because there is nowhere else to send the request. Two or three replicas behind an ordinary Kubernetes Service, with KEDA scaling on `vllm:num_requests_waiting`, covers far more production traffic than the current discourse suggests. For laptops and dev loops, [Ollama](https://builderai.tools/tool/ollama) or [llama.cpp](https://builderai.tools/tool/llama-cpp) skip even that.
A usable rule: adopt a control plane when at least two of the following are true, not one. Multiple models sharing a GPU pool. Multiple teams needing isolation and fairness. A model too large for one node. A hard TTFT or ITL SLO written down somewhere. An idle-GPU bill large enough that someone has asked about it.
## Choosing, and what to watch next
A short decision path. If you already run KServe or OpenShift, take llm-d through `LLMInferenceService` and inherit the CRD your platform team knows. If you are on stock Kubernetes and want the most capability per unit of assembly, AIBrix is the densest package, with the caveat that its newest surfaces are one release old. If you operate NVL72-class hardware and serve MoE models against contractual latency targets, Dynamo with Grove is built for exactly that and nothing smaller, though Grove itself is still alpha. If your fleet is heterogeneous and you have no Kubernetes team, GPUStack. If you need LLM plus embedding plus audio behind one API, Xinference. If you are already a BentoML shop, OpenLLM.
Three things worth tracking through the rest of 2026. First, the Gateway API Inference Extension is GA with a v1 `InferencePool`, so the routing layer is becoming a portable decision even while the control planes stay incompatible; watch whether `InferenceObjective` stays coherent now that it lives in the llm-d Router repository. Second, KV cache is turning into a storage tiering problem: llm-d offloads to EFS and arbitrary filesystems, Dynamo's KVBM reaches S3 and Azure blob, AIBrix unified its KV data plane in v0.7.0. Expect cache placement to become an infrastructure line item rather than an engine flag. Third, disaggregation keeps getting cheaper to run, and for MoE serving it is already closer to the default than the exception. For dense models under 100B on short prompts, it still is not.
## Related Tools
### [AIBrix Kubernetes control plane that scales vLLM serving with LLM-aware routing, autoscaling, and KV cache reuse.](https://builderai.tools/tool/aibrix)### [GPUStack Cluster manager that pools heterogeneous GPUs across machines to serve models through vLLM and SGLang.](https://builderai.tools/tool/gpustack)### [KServe Kubernetes platform for serving predictive models and LLMs with autoscaling, canaries and an OpenAI-style API.](https://builderai.tools/tool/kserve)### [llm-d Kubernetes-native distributed inference stack combining vLLM with KV-cache-aware routing at scale.](https://builderai.tools/tool/llm-d)### [NVIDIA Dynamo Distributed inference framework with disaggregated prefill and decode for datacenter LLM serving.](https://builderai.tools/tool/nvidia-dynamo)### [OpenLLM Runs open-source LLMs as OpenAI-compatible API endpoints with a single command and built-in chat UI.](https://builderai.tools/tool/openllm)### [Ray Serve Scalable model serving library built on Ray for ML applications.](https://builderai.tools/tool/ray-serve)### [SGLang Fast serving framework for LLMs with structured generation and RadixAttention.](https://builderai.tools/tool/sglang)### [vLLM Open source engine for serving LLMs at high throughput behind an OpenAI compatible API.](https://builderai.tools/tool/vllm)### [Xinference Platform for serving LLM, embedding, speech, and image models behind one OpenAI-compatible API.](https://builderai.tools/tool/xinference)
## More Articles
### [Running LLMs Without Nvidia in 2026: ROCm and Apple Silicon The 2026 state of local LLMs off Nvidia: which runtimes actually work on AMD ROCm and Apple Silicon, real throughput numbers, and the porting traps that remain. August 20, 2026](https://builderai.tools/blog/running-llms-on-amd-and-apple-silicon-2026)### [From ComfyUI to Production: Serving Diffusion Models in 2026 A working guide to moving diffusion pipelines from ComfyUI prototype to production: environment locking, queue-backed serving, quantization, caching, and per-image cost math. August 18, 2026](https://builderai.tools/blog/serving-diffusion-models-in-production-2026)### [Securing Self-Hosted AI Stacks in 2026: A Defensive Playbook 175,000 exposed Ollama hosts, an RCE chain in Triton, memory corruption in vLLM, and prompt injection everywhere: a practical guide to hardening your self-hosted AI stack. August 16, 2026](https://builderai.tools/blog/securing-self-hosted-ai-stacks-2026)
### We value your privacy
We use strictly necessary cookies to keep you signed in and to remember your cookie preferences. With your consent, we may also use analytics cookies to understand how the site is used. Your dark mode preference is saved in your browser's local storage and does not require consent. See our [Cookie Policy](https://builderai.tools/cookies) and [Privacy Policy](https://builderai.tools/privacy) for details.
Accept AllReject Non-EssentialManage
