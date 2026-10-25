---
title: 'llm-d on Kubernetes: Disaggregated LLM Inference Deployment Guide (2026) |
  Spheron Blog'
id: llm-d-on-kubernetes-disaggregated-llm-inference-deployment-guide-2026-spheron-bl
tags:
- llmd-router
- llm-d-architecture
- competing-approaches
- llmd-router-nonk8s-backends-54b774
- llmd-kv-cache-routing-c665d5
created: '2026-09-11T19:45:53.908718Z'
updated: '2026-09-15T04:55:23.775822Z'
source: https://www.spheron.network/blog/llm-d-kubernetes-disaggregated-inference-guide/
source_domain: www.spheron.network
fetched_at: '2026-09-11T19:45:53.889462Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Vendor blog (Spheron, low authority, but useful for architecture framing
  and a competitor contrast) covering llm-d''s March 24, 2026 acceptance into CNCF
  Sandbox (backed by IBM, Red Hat, Google, CoreWeave, NVIDIA). Names four core components:
  the llm-d scheduler (cache-aware routing process tracking per-decode-worker KV cache
  state), the InferencePool CRD (groups decode GPU pods into a named pool the scheduler
  targets), the InferenceObjective CRD (maps model/LoRA-adapter names to backend deployments
  with traffic weights and criticality labels), and Gateway API Inference Extension
  integration. Explicitly contrasts llm-d against NVIDIA Dynamo: Dynamo is an orchestration
  layer that runs ABOVE vLLM and OUTSIDE Kubernetes, using NIXL for KV transfer and
  coupled to NVIDIA DGX/HGX hardware, whereas llm-d uses standard Kubernetes networking,
  CRDs, and Gateway API -- a relevant data point for anyone considering an out-of-cluster
  router design, since Dynamo demonstrates that non-k8s-native orchestration for the
  same KV-aware routing problem is an established pattern outside llm-d itself. Notes
  LMCache as an option for sharing KV state across llm-d workers via a Redis-backed
  tier.'
utility_score: 8.0
---

On March 24, 2026, llm-d was accepted into the CNCF Sandbox. The project solves a specific problem: monolithic vLLM saturates prefill GPUs during high-concurrency decode, and disaggregating those two phases onto separate GPU pools fixes it. Unlike [NVIDIA Dynamo's disaggregated inference approach](https://www.spheron.network/blog/nvidia-dynamo-disaggregated-inference-guide/), which is an orchestration layer running above vLLM outside Kubernetes, llm-d is a first-class Kubernetes citizen using standard CRDs and the Gateway API Inference Extension. If you haven't set up vLLM on bare metal yet, the [vLLM production deployment guide](https://www.spheron.network/blog/vllm-production-deployment-2026/) covers that baseline first.
## What Is llm-d
llm-d is a Kubernetes-native framework for disaggregated LLM inference. Backed by IBM, Red Hat, Google, CoreWeave, and NVIDIA, among others, the project was donated to CNCF on March 24, 2026 and immediately accepted into the Sandbox program.
Four core components:
  * **llm-d scheduler** : the cache-aware routing process that tracks KV cache state per decode worker and routes requests to maximize prefix cache hits
  * **InferencePool CRD** : a custom resource that groups decode GPU pods into a named pool the scheduler can target
  * **InferenceObjective CRD** : maps model names (and LoRA adapter names) to backend deployments with traffic weights and request criticality labels
  * **Gateway API Inference Extension integration** : plugs into the standard Kubernetes Gateway API to replace round-robin with inference-aware routing


The key difference from NVIDIA Dynamo: Dynamo is an orchestration layer that sits above vLLM and runs outside Kubernetes. It uses NIXL for KV cache transfer and is tightly coupled to NVIDIA's DGX/HGX hardware stack. llm-d uses standard Kubernetes networking, CRDs, and the Gateway API. If your team already manages Kubernetes clusters and wants disaggregation without adopting a separate proprietary control plane, llm-d is the natural path. For a deeper look at the new DRA driver and KAI Scheduler that underpin Kubernetes GPU scheduling in 2026, see our [Kubernetes GPU Orchestration guide](https://www.spheron.network/blog/kubernetes-gpu-orchestration-2026/). For NVIDIA's own Kubernetes-native approach using PodCliqueSets and DRA-backed GPU selection, see the [NVIDIA Grove guide](https://www.spheron.network/blog/nvidia-grove-kubernetes-disaggregated-inference-guide/).
## Prefill/Decode Disaggregation on Kubernetes
### The Two Phases and Why They Need Different Hardware
Every LLM generation request has two phases. **Prefill** processes all input tokens in a single forward pass. Attention is computed over the full prompt length, making prefill compute-bound: FLOPs scale with prompt token count.
**Decode** generates one output token per step, reading the full KV cache at every step. With a 4K-token context, decode makes 256 separate KV cache reads across 4K+ tokens each. It is dominated by memory bandwidth, not compute.
In monolithic vLLM, both phases share the same GPU. A long prefill request blocks decode batches from continuing. This head-of-line blocking is what disaggregation solves: send prefill work to compute-optimized nodes, send decode work to memory-bandwidth-optimized nodes.
### How llm-d Routes Requests

```
Client
  |
  v
Kubernetes Gateway (Gateway API)
  |
  v
llm-d Scheduler (cache-aware routing)
  |         \
  v           v
Prefill Pool     KV transfer
(H100 SXM/B200)     |
                     v
               Decode Pool
               (H100 PCIe / A100 80GB)
                     |
                     v
               Response to Client
```

The scheduler has a key advantage over naive load balancing: it knows which decode workers hold which KV cache blocks. When an incoming request shares a prefix with a recently cached request, the scheduler routes it to the decode worker that already has that prefix cached. This reduces redundant prefill work and cuts time-to-first-token for repeated-prefix workloads. For teams that want to extend this further by sharing KV state across all llm-d workers rather than routing to the one that already cached it, [LMCache for KV cache sharing across llm-d workers](https://www.spheron.network/blog/deploy-lmcache-vllm-kv-cache-sharing-gpu-cloud/) adds a Redis-backed shared cache tier that any worker can read from.
For deeper background on how KV cache memory works, how PagedAttention allocates blocks, and what prefix caching looks like at the memory level, see the [KV Cache Optimization Guide](https://www.spheron.network/blog/kv-cache-optimization-guide/).
## Hardware Requirements and GPU Selection
The right GPU depends on which pool it's in.  
| Role  | Recommended GPU  | Why  | Price  |  
| --- | --- | --- | --- |  
| Prefill  | H100 SXM5  | Compute-bound, 3,958 TFLOPS FP8 (with sparsity)  | $2.40/hr on-demand  |  
| Prefill (high-end)  | B200 SXM6  | ~2.3x dense FP8 FLOP increase over H100  | $1.67/hr (spot)  |  
| Decode  | H100 PCIe  | Memory bandwidth, lower cost  | $2.01/hr on-demand  |  
| Decode  | A100 SXM4 80GB  | Cost-efficient memory bandwidth  | $1.08/hr on-demand  |  
| Mixed (dev/test)  | H100 SXM5  | Single pool, no disaggregation  | $2.40/hr on-demand  |  
The v0.5 benchmark validates meaningful throughput at scale: 3,100 tokens/second per B200 decode GPU, and 50,000 output tokens/second on a 16x16 B200 topology (16 prefill nodes, 16 decode nodes). At single-node or small-cluster scale, results will differ. For per-GPU benchmark data including memory bandwidth and inference throughput comparisons across H100, A100, and B200, see [Best GPU for AI Inference 2026](https://www.spheron.network/blog/best-gpu-for-ai-inference-2026/).
Getting close to these numbers on your own cluster means confirming the GPUs in each pool are actually running at the utilization the benchmark assumes, not just that the pods are healthy. The [nvidia-smi and DCGM monitoring guide](https://www.spheron.network/blog/gpu-monitoring-for-ml/) covers tracking per-GPU utilization, memory bandwidth saturation, and ECC errors across a prefill/decode split like this one.
> Pricing fluctuates based on GPU availability. The prices above are based on **01 Apr 2026** and may have changed. Check [current GPU pricing](https://www.spheron.network/pricing/) for live rates.
## Step-by-Step Deployment on Spheron with Kubernetes
### Provision GPU Instances on Spheron
Rent at least two GPU instances: one for prefill, one or more for decode. For prefill, use [H100 SXM5](https://www.spheron.network/gpu-rental/h100/) or [B200](https://www.spheron.network/gpu-rental/b200/) for compute throughput. For decode, H100 PCIe or A100 80GB at lower hourly rates are a good fit.
After SSH access:
bash

```
nvidia-smi
# Verify: GPU model, VRAM, driver version (575.xx+ for H100/B200)
```

Both nodes need Kubernetes 1.29+ running. If you're starting from scratch, `kubeadm init` on the control plane node and `kubeadm join` on workers is the standard path. Multi-node clusters need a CNI (Flannel, Calico, Cilium all work).
### Install Kubernetes Prerequisites
Install the NVIDIA GPU Operator first. This handles the NVIDIA device plugin, container toolkit, and driver management in one Helm chart:
bash

```
helm repo add nvidia https://helm.ngc.nvidia.com/nvidia
helm repo update
helm install gpu-operator nvidia/gpu-operator \
  --namespace gpu-operator \
  --create-namespace
```

Install Gateway API CRDs (v1.3.0+ required for llm-d):
bash

```
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.3.0/standard-install.yaml
```

Install cert-manager (required by llm-d's webhook):
bash

```
helm repo add jetstack https://charts.jetstack.io
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --set crds.enabled=true
```

### Install llm-d via Helm
bash

```
helm repo add llm-d-infra https://llm-d-incubation.github.io/llm-d-infra/
helm repo update
helm install llm-d llm-d-infra/llm-d-infra \
  --namespace llm-d \
  --create-namespace \
  -f values.yaml
```

A minimal `values.yaml` for a 1-prefill, 1-decode deployment (verify field names against the [actual chart values](https://github.com/llm-d-incubation/llm-d-infra) before applying; consult llm-d scaling docs before increasing replicas beyond 1+1):
yaml

```
prefillReplicas: 1
decodeReplicas: 1

model:
  name: "meta-llama/Llama-3.1-70B-Instruct"
  source: "huggingface"
  dtype: "fp8"

scheduler:
  cacheAwareRouting: true

gateway:
  enabled: true
  className: "llm-d-gateway"
```

### Define InferencePool and InferenceObjective CRDs
The `InferencePool` groups decode pods and is the target for the scheduler. The examples below use `inference.networking.x-k8s.io/v1alpha2`, the experimental API group. A stable `inference.networking.k8s.io/v1` API is also available; check the [Gateway API Inference Extension docs](https://gateway-api-inference-extension.sigs.k8s.io/) for the version shipped with your llm-d release.
yaml

```
apiVersion: inference.networking.x-k8s.io/v1alpha2
kind: InferencePool
metadata:
  name: llama-70b-decode-pool
  namespace: llm-d
spec:
  targetPortNumber: 8000
  selector:
    matchLabels:
      llm-d.ai/role: decode
      app: llama-70b-worker
```

The `InferenceObjective` (renamed from `InferenceModel` in the v1 GA release; v1alpha2 still uses `InferenceModel`) maps a model name to the pool and sets request criticality:
yaml

```
apiVersion: inference.networking.x-k8s.io/v1alpha2
kind: InferenceModel
metadata:
  name: llama-70b
  namespace: llm-d
spec:
  modelName: "meta-llama/Llama-3.1-70B-Instruct"
  criticality: Standard
  poolRef:
    name: llama-70b-decode-pool
  targetModels:
    - name: "meta-llama/Llama-3.1-70B-Instruct"
      weight: 100
```

### Deploy Prefill and Decode Workers
Prefill worker Deployment:
> **Important:** llm-d uses NIXL (NVIDIA Inference Xfer Library) for KV cache transfer between prefill and decode nodes. NIXL handles peer discovery at the transport layer, so you do not need to specify fixed NCCL ranks or `kv_parallel_size` in the connector config. The manifests below use `replicas: 1` for both deployments as a starting point. Scaling to multiple replicas with NIXL still requires coordination so each prefill pod connects to the correct decode peer. Consult the [llm-d scaling documentation](https://llm-d.ai/docs/well-lit-paths/foundations/pd-disaggregation) before increasing replicas beyond 1.
yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: llama-70b-prefill
  namespace: llm-d
spec:
  replicas: 1
  selector:
    matchLabels:
      app: llama-70b-worker
      llm-d.ai/role: prefill
  template:
    metadata:
      labels:
        app: llama-70b-worker
        llm-d.ai/role: prefill
    spec:
      containers:
        - name: vllm-prefill
          image: vllm/vllm-openai:v0.18.0
          env:
            - name: VLLM_MODEL
              value: "meta-llama/Llama-3.1-70B-Instruct"
          args:
            - '--kv-transfer-config={"kv_connector":"NixlConnector","kv_role":"kv_producer"}'
            - "--dtype=fp8"
            - "--gpu-memory-utilization=0.90"
            - "--max-model-len=32768"
          resources:
            requests:
              nvidia.com/gpu: "1"
            limits:
              nvidia.com/gpu: "1"
```

Decode worker Deployment:
yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: llama-70b-decode
  namespace: llm-d
spec:
  replicas: 1
  selector:
    matchLabels:
      app: llama-70b-worker
      llm-d.ai/role: decode
  template:
    metadata:
      labels:
        app: llama-70b-worker
        llm-d.ai/role: decode
    spec:
      containers:
        - name: vllm-decode
          image: vllm/vllm-openai:v0.18.0
          env:
            - name: VLLM_MODEL
              value: "meta-llama/Llama-3.1-70B-Instruct"
          args:
            - '--kv-transfer-config={"kv_connector":"NixlConnector","kv_role":"kv_consumer"}'
            - "--dtype=fp8"
            - "--gpu-memory-utilization=0.92"
            - "--max-model-len=32768"
          resources:
            requests:
              nvidia.com/gpu: "1"
            limits:
              nvidia.com/gpu: "1"
```

> **vLLM version and KV connector:** Both manifests use `v0.18.0` (current as of March 2026). llm-d's minimum requirement is v0.10.0, so any version at or above that works. Check [Docker Hub for the latest vllm-openai tag](https://hub.docker.com/r/vllm/vllm-openai/tags) before deploying. The `kv_connector` field is pluggable. `NixlConnector` is the connector llm-d uses for KV cache transfer via NIXL (NVIDIA Inference Xfer Library). Verify the connector name against your vLLM version's [disaggregated prefill documentation](https://docs.vllm.ai/en/latest/features/disagg_prefill/) if you change versions.
### Configure the Gateway HTTPRoute
yaml

```
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: llama-70b-route
  namespace: llm-d
spec:
  parentRefs:
    - name: llm-d-gateway
      namespace: llm-d
  rules:
    - backendRefs:
        - group: inference.networking.x-k8s.io
          kind: InferencePool
          name: llama-70b-decode-pool
          port: 8000
```

### Send a Test Request
Get the Gateway's external IP:
bash

```
kubectl get gateway llm-d-gateway -n llm-d -o jsonpath='{.status.addresses[0].value}'
```

Send a completion request:
bash

```
curl http://<GATEWAY_IP>/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "meta-llama/Llama-3.1-70B-Instruct",
    "messages": [{"role": "user", "content": "Explain KV cache disaggregation in two sentences."}],
    "max_tokens": 100
  }'
```

A successful response means prefill ran on the prefill pool, KV tensors transferred to a decode worker, and decode completed the generation.
A successful test request tells you the pipeline works, but it doesn't tell you where time is actually going once real traffic hits both pools. The [LLM observability guide for Langfuse, Arize Phoenix, and Helicone](https://www.spheron.network/blog/llm-observability-gpu-cloud-langfuse-arize-phoenix-helicone/) covers tracing individual requests across a prefill/decode split so a slow response can be attributed to the right node.
## Configuring the Gateway API Inference Extension
The Gateway API Inference Extension adds three capabilities that standard round-robin load balancing doesn't have:
  1. **Cache-aware load balancing** : routes requests to decode workers that already hold matching KV prefix blocks, reducing redundant prefill work
  2. **Model-aware routing** : dispatches based on the `model` field in the request body, supporting multiple models behind one Gateway endpoint
  3. **Request prioritization** : uses the `criticality` field on `InferenceObjective` to shed low-priority requests under load before dropping high-priority ones


For teams evaluating GKE's managed layer on top of this stack, [GKE Inference Gateway uses the same EPP for KV-cache-aware routing](https://www.spheron.network/blog/gke-inference-gateway-kv-cache-aware-llm-routing/) and can be self-hosted with identical behavior outside GCP.
### InferencePool in Detail
The `targetPortNumber` must match the port your vLLM decode pods expose. The `selector` uses standard Kubernetes label selectors, so you can target any pod label combination:
yaml

```
spec:
  targetPortNumber: 8000
  selector:
    matchLabels:
      llm-d.ai/role: decode
      app: llama-70b-worker
```

### InferenceObjective Criticality
The `criticality` field on `InferenceObjective` has three values:
  * **Critical** : always served; shed last under load
  * **Standard** : normal priority; shed before Critical requests
  * **Sheddable** : background workloads; shed first when capacity is tight


This lets you run batch inference jobs (Sheddable) alongside interactive API traffic (Critical) on the same cluster without batch jobs crowding out user-facing requests.
### Multi-Version Traffic Splitting
The `targetModels` list supports weighted routing across model versions, useful for A/B testing a fine-tuned checkpoint against the base model:
yaml

```
targetModels:
  - name: "meta-llama/Llama-3.1-70B-Instruct"
    weight: 90
  - name: "llama-70b-finetuned-v2"
    weight: 10
```

For more on Spheron bare metal instances, see the [Spheron documentation](https://docs.spheron.ai).
## Benchmarks: llm-d vs Monolithic vLLM
The v0.5 published numbers use a 16x16 B200 topology (16 prefill nodes, 16 decode nodes) running Llama 3.1 70B FP8. That is a large cluster; single-node and small-cluster comparisons will show different magnitudes.
The pattern holds across cluster sizes: disaggregation gains are most pronounced at high concurrency. At low concurrency (under 20 simultaneous requests), disaggregation adds KV transfer latency overhead that may outweigh the throughput benefit.  
| Configuration  | Throughput  | TTFT (p50)  | Notes  |  
| --- | --- | --- | --- |  
| vLLM monolithic (2x H100 SXM5)  | baseline  | baseline  | No KV transfer overhead  |  
| llm-d disaggregated (2P + 4D H100)  | ~2-3x higher at >50 concurrent  | lower (dedicated prefill)  | KV transfer adds ~5-15ms  |  
| llm-d (16x16 B200, v0.5)  | 50,000 tok/s total  | not published  | Published benchmark  |  
The 2-3x throughput figure at high concurrency comes from prefill and decode no longer competing for the same GPU cycles. When a batch of 100 requests arrives simultaneously, monolithic vLLM serializes prefill for each request before decode can continue; disaggregated llm-d runs prefill in parallel across the prefill pool while the decode pool stays active.
For broader inference framework comparisons including TensorRT-LLM and SGLang, see the [vLLM vs TensorRT-LLM vs SGLang benchmarks](https://www.spheron.network/blog/vllm-vs-tensorrt-llm-vs-sglang-benchmarks/). For GPU-level throughput and cost-per-token data on H100 and B200, see [GPU cloud benchmarks 2026](https://www.spheron.network/blog/gpu-cloud-benchmarks/).
Disaggregation cuts the cost of serving a single large model, but if your traffic mixes simple and complex queries, the bigger lever might be sending simpler requests to a smaller model entirely. The [LLM inference router guide](https://www.spheron.network/blog/llm-inference-router-gpu-cloud/) covers building that tiered routing layer on top of a disaggregated deployment like this one.
## Scaling: Autoscaling, LoRA Routing, and Multi-Model Serving
### Autoscaling with KEDA or HPA
Prefill and decode pools scale independently because their bottlenecks differ. KEDA with a custom metric works well here: scale decode workers on queue depth, scale prefill workers on compute utilization.
**Note on scaling with NixlConnector:** The deployments in this guide use `NixlConnector`, which handles peer discovery at the transport layer and does not require fixed NCCL ranks. Scaling `NixlConnector`-based deployments requires that each prefill pod can discover and connect to the correct decode peer. Consult the [llm-d scaling documentation](https://llm-d.ai/docs/well-lit-paths/foundations/pd-disaggregation) before increasing replicas beyond 1+1. If you switch to `P2pNcclConnector` (a generic vLLM connector), note that it uses a fixed 2-member NCCL communicator with static ranks, and you would need dynamic rank assignment before autoscaling. The KEDA example below shows the pattern for NixlConnector-based deployments.
A KEDA ScaledObject for the decode Deployment:
yaml

```
apiVersion: keda.sh/v1alpha1
kind: ScaledObject
metadata:
  name: llama-decode-scaler
  namespace: llm-d
spec:
  scaleTargetRef:
    name: llama-70b-decode
  minReplicaCount: 1
  maxReplicaCount: 16
  triggers:
    - type: prometheus
      metadata:
        serverAddress: http://prometheus.monitoring:9090
        metricName: vllm_num_requests_waiting
        threshold: "10"
        query: sum(vllm:num_requests_waiting{model_name="meta-llama/Llama-3.1-70B-Instruct"})
```

When `vllm:num_requests_waiting` exceeds 10 pending requests per decode pod, KEDA adds replicas. When it drops below, it scales down. The decode pool responds to load without touching prefill capacity.
The example above glosses over what happens when KEDA scales from zero: a fresh decode pod has to load model weights before it can serve, and that cold-start window is where a lot of scale-to-zero setups fall over in practice. [GPU inference autoscaling with KEDA and Knative](https://www.spheron.network/blog/keda-knative-gpu-autoscaling-kubernetes-llm-cold-start/) covers cold-start mitigation strategies specifically for this scenario, including pre-warming and Knative's request-based scaling as an alternative to a pure Prometheus trigger.
### LoRA Adapter Routing
The `InferenceModel` CRD (called `InferenceObjective` in the v1 GA API) natively supports LoRA adapter routing. Multiple adapters can share a decode pool, with traffic distributed by weight:
yaml

```
apiVersion: inference.networking.x-k8s.io/v1alpha2
kind: InferenceModel
metadata:
  name: llama-70b-with-adapters
  namespace: llm-d
spec:
  modelName: "meta-llama/Llama-3.1-70B-Instruct"
  criticality: Standard
  poolRef:
    name: llama-70b-decode-pool
  targetModels:
    - name: "meta-llama/Llama-3.1-70B-Instruct"
      weight: 70
    - name: "llama-70b-customer-support-lora"
      weight: 20
    - name: "llama-70b-code-lora"
      weight: 10
```

The scheduler routes requests to decode workers that have the requested adapter loaded, minimizing adapter swap overhead. For the full setup including adapter loading, hot-swap, and memory management, see the [LoRA multi-adapter serving guide](https://www.spheron.network/blog/lora-multi-adapter-serving-gpu-cloud/).
### Multi-Model Serving
Multiple `InferenceObjective` CRDs can share a single `InferencePool`. The scheduler routes by model name from the request body. The constraint is GPU memory: each model you load adds to the memory footprint of each decode worker. On an 80 GB H100, you might load a 7B and a 13B simultaneously (roughly 14 GB + 26 GB in FP8), but a 70B alone nearly fills the card.
## Cost Analysis: llm-d on Spheron vs Managed Inference APIs
Self-hosted cost depends on actual GPU utilization. The figures below assume steady-state utilization at roughly 60-70% for a production workload generating 1M tokens per day.  
| Option  | Setup  | $/1M tokens  | Control  |  
| --- | --- | --- | --- |  
| OpenAI GPT-4o  | None  | ~$5 (input) + $15 (output)  | None  |  
| [Fireworks / Together AI](https://www.spheron.network/blog/fireworks-ai-alternatives/)  | None  | ~$0.90-$3 (model-dependent)  | Limited  |  
| llm-d on Spheron H100 SXM5  | ~1hr  | ~$0.15-$0.40 (at utilization)  | Full  |  
| llm-d on Spheron B200 SXM6 (spot)  | ~1hr  | ~$0.02-$0.05 (at utilization)  | Full  |  
The self-hosted estimates derive from current Spheron prices ($2.40/hr on-demand for H100 SXM5; $1.67/hr spot for B200 SXM6; on-demand B200 pricing is not currently available) divided by estimated throughput from the v0.5 benchmark data at realistic utilization. At higher utilization, cost per token drops further.
Bare metal matters here. KVM hypervisors add 5-15% CUDA overhead; shared cloud VMs add more. Spheron's bare metal instances give you direct PCIe and NVLink access to the GPU. For disaggregated inference specifically, KV cache transfer latency over the network fabric is a critical variable, and hypervisor-induced jitter in network I/O degrades it.
For a full comparison across cloud providers including per-GPU pricing history and spot availability, see [GPU cloud pricing comparison 2026](https://www.spheron.network/blog/gpu-cloud-pricing-comparison-2026/).
A note on CNCF Sandbox status: accepted does not mean production-ready. Sandbox signals that the CNCF Technical Oversight Committee found the project technically sound and the governance model appropriate for community development. The v0.5 API surface will change before a stable release. Run llm-d in staging, validate your specific workload, and pin to a specific Helm chart version before deploying to production traffic.
> llm-d's disaggregated inference delivers its best results on bare metal where you control the full CUDA stack and network. Spheron's H100 and B200 bare metal instances give you Kubernetes-ready GPU nodes without virtualization overhead.
> [On-demand H100 →](https://www.spheron.network/gpu-rental/h100/) | [View all GPU pricing →](https://www.spheron.network/pricing/)
> **[Get started on Spheron →](https://app.spheron.ai)**
STEPS / 06
## Quick Setup Guide
  1. 01
### Provision GPU instances on Spheron
Rent two or more GPU instances on Spheron for separate prefill and decode worker pools. For prefill nodes, use H100 SXM5 or B200 for compute throughput. For decode nodes, H100 PCIe or A100 80GB provide cost-efficient memory bandwidth. Both nodes must be Kubernetes-ready with NVIDIA Container Toolkit and kubeadm or an existing cluster.
  2. 02
### Install llm-d and its Kubernetes prerequisites
Install the llm-d Helm chart from the official registry. Prerequisites: Kubernetes 1.29+, cert-manager, the Gateway API CRDs (v1.3.0+), and the NVIDIA GPU Operator. Run `helm repo add llm-d-infra https://llm-d-incubation.github.io/llm-d-infra/ && helm install llm-d llm-d-infra/llm-d-infra -f values.yaml` with your prefill/decode topology defined in values.yaml.
  3. 03
### Configure the InferencePool and InferenceObjective CRDs
Define an InferencePool custom resource that groups your decode GPU pods, and an InferenceObjective resource that maps model names to backend deployments. These CRDs (from the Gateway API Inference Extension) are what enable cache-aware routing and LoRA adapter dispatch.
  4. 04
### Deploy prefill and decode worker Deployments
Create two Kubernetes Deployments: one for prefill workers and one for decode workers. Annotate pods with `llm-d.ai/role: prefill` or `llm-d.ai/role: decode`. Each pod runs vLLM with the `--kv-transfer-config` CLI argument specifying the connector (NixlConnector) and role (kv_producer or kv_consumer). NIXL handles peer discovery at the transport layer, so no fixed NCCL ranks are needed. Start with replicas: 1 for each; consult llm-d scaling docs before increasing replicas. Set resource requests/limits with `nvidia.com/gpu: 1` per pod.
  5. 05
### Start the llm-d scheduler and Gateway
Deploy the llm-d scheduler (cache-aware routing logic) and configure the HTTPRoute resource to point the Gateway to the InferencePool. The scheduler tracks KV cache state per decode worker and routes incoming requests to workers with the best prefix cache hit rate.
  6. 06
### Test and benchmark the deployment
Send a test request via the Gateway endpoint using the OpenAI-compatible API. Run throughput benchmarks using the llm-d benchmarking tool or vLLM's benchmark_serving.py. Compare tokens/second on the disaggregated deployment against a baseline monolithic vLLM deployment to validate the disaggregation gain.


FAQ / 05
## Frequently Asked Questions
### 01What is llm-d?
llm-d is a Kubernetes-native framework for disaggregated LLM inference, donated to CNCF on March 24, 2026. It separates prefill and decode phases onto independent GPU pools and uses the Kubernetes Gateway API Inference Extension for cache-aware request routing.
### 02How does llm-d differ from NVIDIA Dynamo?
Both do prefill/decode disaggregation, but llm-d is Kubernetes-native and CNCF-governed, while Dynamo is an NVIDIA-proprietary orchestration layer. llm-d uses standard Kubernetes CRDs and the Gateway API Inference Extension, making it a better fit for teams already running Kubernetes. Dynamo targets bare-metal or NVIDIA DGX clusters without a Kubernetes dependency.
### 03What GPUs work best with llm-d?
For prefill nodes, use compute-heavy GPUs like H100 SXM5 or B200. For decode nodes, memory-bandwidth-heavy GPUs like H100 PCIe or A100 80GB work well and cost less. The v0.5 benchmark validated 3,100 tokens/second per B200 decode GPU and 50,000 output tokens/second on a 16x16 B200 topology.
### 04What is the Gateway API Inference Extension used in llm-d?
The Gateway API Inference Extension (part of the Kubernetes SIG Network effort) adds LLM-aware routing to the standard Kubernetes Gateway API. It enables cache-aware load balancing based on KV cache hits and model-aware routing for multi-model and LoRA-adapter serving, replacing static round-robin with inference-aware request dispatch.
### 05Is llm-d production ready?
As of v0.5 (March 2026), llm-d is in active development under CNCF Sandbox status. Sandbox does not imply production-ready; it signals community governance and trajectory. For production workloads, treat v0.5 as early-adopter/staging. The benchmark numbers from v0.5 on B200 topologies are genuine, but expect API changes before a stable release.
## Related Reading
[More on Infrastructure →](https://www.spheron.network/blog/topics/infrastructure/)
[ Engineering GPU Infrastructure Behind World Models: What Powers Genie 3, Marble, and Real-Time Interactive AI (2026 Guide) Jun 17, 2026 ](https://www.spheron.network/blog/gpu-infrastructure-world-models-2026/)[ Engineering Multi-Tenant LLM Serving on GPU Cloud: Per-Customer Isolation, Token Quotas, and Production SaaS Architecture Guide (2026) Jun 7, 2026 ](https://www.spheron.network/blog/multi-tenant-llm-serving-gpu-cloud/)[ Engineering Token-Level GPU Pooling for Multi-LLM Marketplace Inference (2026 Guide) May 19, 2026 ](https://www.spheron.network/blog/token-level-gpu-pooling-multi-llm-marketplace-inference/)[ Tutorial LLM-as-Judge Evaluation Pipelines on GPU Cloud: Build Production Model Evaluation Infrastructure (2026 Guide) Apr 22, 2026 ](https://www.spheron.network/blog/llm-as-judge-evaluation-pipeline-gpu-cloud/)[ Tutorial GPU Infrastructure Behind AI Coding Tools: Cursor, Claude Code, and GitHub Copilot in 2026 Mar 30, 2026 ](https://www.spheron.network/blog/gpu-infrastructure-ai-coding-tools-2026/)[ Tutorial Deploy vLLM-Omni on GPU Cloud: Fully Disaggregated Serving for Any-to-Any Multimodal Models (2026 Guide) Jun 16, 2026 ](https://www.spheron.network/blog/deploy-vllm-omni-disaggregated-multimodal-serving-gpu-cloud/)
Try It Yourself
### Try It on Real GPUs
The GPUs behind these guides are the ones you can rent here: H100s, H200s, B200s, and more, billed per minute with no contracts and no minimum. Pick one and you are live in under two minutes.
Rent a GPU
Deploy Time
< 2 min
Uptime SLA
99.9%
GPU Models
10+
Starting At
$0.72/hr
We use cookies to measure traffic and improve Spheron. Accept all, reject non-essential, or choose what's on. See our [Privacy Policy](https://www.spheron.network/privacy/).
ManageRejectAccept
