---
title: 'GKE Inference Gateway: KV-Cache-Aware LLM Routing Explained | Spheron Blog'
id: gke-inference-gateway-kv-cache-aware-llm-routing-explained-spheron-blog
tags:
- llmd-router
- epp-architecture
- gke
- llmd-router-nonk8s-backends-54b774
- llmd-kv-cache-routing-c665d5
created: '2026-09-11T19:46:04.084203Z'
updated: '2026-09-15T04:55:23.779184Z'
source: https://www.spheron.network/blog/gke-inference-gateway-kv-cache-aware-llm-routing/
source_domain: www.spheron.network
fetched_at: '2026-09-11T19:46:04.063079Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Vendor blog (Spheron, low authority) on Google''s GKE Inference Gateway
  (announced Google Cloud Next 2025), explicitly stating it is ''powered by the llm-d
  Endpoint Picker (EPP)'' and that ''the EPP is not proprietary. It ships in the open-source
  gateway-api-inference-extension repository under kubernetes-sigs... The routing
  logic, the CRDs, and the vLLM integration are identical whether you run on GKE or
  on your own Kubernetes cluster'' -- direct evidence that GCP wraps/deploys the same
  upstream EPP binary rather than reimplementing it, with only the managed control
  plane, GCLB provisioning, and IAM integration being GCP-specific. Documents EPP''s
  fallback behavior: when no replica has a strong prefix match, it falls back to routing
  on vllm:kv_cache_usage_perc. Provides a worked cost example: round-robin across
  8 replicas yields ~12.5% per-replica cache hit rate for a shared system prompt;
  a 128K-token system prompt on a 70B FP8 model costs ~11s prefill, so 1,000 req/hr
  with round-robin wastes ~11,000 GPU-seconds/hr vs <2,000 GPU-seconds/hr with KV-aware
  routing. Notes cache-hit-rate advantage of KV-aware routing improves (not degrades)
  as replica count scales up, the opposite of round-robin.'
utility_score: 8.0
---

Round-robin load balancing works fine for stateless services. For LLM inference, it's actively harmful. Each vLLM replica holds a different KV cache state, and routing to the wrong replica means throwing away computed work and re-running prefill from scratch. Google's GKE Inference Gateway, announced at Google Cloud Next 2025, addresses this directly with cache-aware routing powered by the llm-d Endpoint Picker (EPP). The interesting part: the routing logic underneath is open source, built on the Kubernetes Gateway API Inference Extension, and runs on any cluster. For teams already running [llm-d for disaggregated inference on Kubernetes](https://www.spheron.network/blog/llm-d-kubernetes-disaggregated-inference-guide/), the EPP is the same component that drives cache-aware request dispatch in that stack too.
## What the GKE Inference Gateway Actually Does
GKE Inference Gateway is a managed Kubernetes gateway with inference-aware routing built in. You point it at a pool of vLLM replicas, and it takes over request dispatch from a standard load balancer.
Three things make it different from a dumb L7 proxy:
**Model-aware routing:** The gateway reads the `model` field from every incoming `/v1/chat/completions` request and routes it to the correct backend pool. Multiple models behind a single endpoint, no separate Services per model.
**Prefix cache routing:** Before forwarding a request, the Endpoint Picker hashes the incoming token prefix and checks which replica most likely holds that prefix in its KV cache. Requests with matching prefixes go to the replica that already computed them.
**KV cache utilization balancing:** When no replica has a strong prefix match, the EPP falls back to routing based on `vllm:kv_cache_usage_perc`, sending new requests to the replica with the most available cache capacity.
Here is what the architecture looks like:

```
Client
  |
  v
GKE Inference Gateway (Envoy + EPP plugin)
  |
  v
Endpoint Picker (EPP) -- scrapes /metrics from each replica
  |         |          |
  v         v          v
vLLM-0   vLLM-1   vLLM-2
(KV cache A) (KV cache B) (KV cache C)
```

The GKE layer manages the Envoy proxy, the load balancer provisioning, and the Kubernetes control plane. The EPP is where the routing intelligence lives. It is a Go binary that plugs into the gateway as an extension filter.
Crucially, the EPP is not proprietary. It ships in the open-source `gateway-api-inference-extension` repository under `kubernetes-sigs`. The GCP-specific parts are the managed control plane, the Google Cloud Load Balancer provisioning, and the IAM integration. The routing logic, the CRDs, and the vLLM integration are identical whether you run on GKE or on your own Kubernetes cluster.
## Why Round-Robin Fails for LLM Serving
A stateless web server is a good fit for round-robin because each replica has identical capacity and no local state that makes one replica better than another for a specific request. LLM serving breaks both assumptions.
Each vLLM replica maintains its own KV cache. When a request comes in with a long system prompt, the first replica to process it computes the full KV cache for that prompt and stores it. The next identical request, if routed to the same replica, skips that computation entirely via prefix caching. But with round-robin across 8 replicas, the same system prompt gets distributed across all 8 replicas, and each replica sees the prefix only about 12.5% of the time. Cache hit rate per replica is near zero.
The math on wasted compute is ugly. A 128K-token system prompt on a 70B FP8 model takes roughly 11 seconds of prefill on a multi-GPU setup capable of holding the full context (the model weights alone are ~70GB, so running 128K-token contexts requires tensor parallelism across multiple H100s). With round-robin across 8 replicas, almost every request hits that full prefill. With KV-aware routing, the first request per replica pays the full cost, and every subsequent request with the same prefix hits cache in under 2 seconds. On a chatbot handling 1,000 requests per hour with a shared system prompt, that's the difference between 11,000 GPU-seconds wasted on redundant prefill versus under 2,000.
This scales worse as you add replicas. More replicas means lower per-replica cache hit rate with round-robin. KV-aware routing gets more efficient as you scale because it has more cache capacity to route into, not less.
Coding assistants generate exactly this kind of repeated-prefix load. Tools like Cursor and GitHub Copilot resend a large chunk of repository context on every completion request, and the [GPU infrastructure guide for AI coding tools](https://www.spheron.network/blog/gpu-infrastructure-ai-coding-tools-2026/) breaks down how that traffic pattern shapes GPU provisioning for teams serving those assistants at scale.
Here is a concrete before/after comparison on a chatbot workload with a 4K-token system prompt, 8 vLLM replicas (illustrative estimates based on modeled workload behavior):  
| Metric  | Round-Robin  | KV-Aware Routing  |  
| --- | --- | --- |  
| Avg TTFT (first request per prefix)  | 3.2s  | 3.2s  |  
| Avg TTFT (repeated prefix, p50)  | 3.1s  | 0.4s  |  
| Avg TTFT (repeated prefix, p95)  | 4.8s  | 0.9s  |  
| GPU prefill compute wasted  | ~88%  | ~12%  |  
| Effective throughput (tokens/s)  | 1.0x  | ~3.1x  |  
The TTFT improvement compounds at longer contexts and higher concurrency. For RAG pipelines with document-length system prompts, the difference is even larger.
For a deeper explanation of why KV cache state matters and how PagedAttention allocates cache blocks, see the [KV cache optimization guide](https://www.spheron.network/blog/kv-cache-optimization-guide/).
## Under the Hood: the llm-d Endpoint Picker and Gateway API Inference Extension
The routing logic lives in two components: the Gateway API Inference Extension CRDs and the Endpoint Picker binary.
### The Endpoint Picker
The EPP runs as a Kubernetes Deployment alongside the gateway. It maintains a background scrape loop that polls each registered vLLM replica's Prometheus `/metrics` endpoint every 100ms. From these metrics it reads:
  * `vllm:kv_cache_usage_perc`: fraction of KV cache blocks currently occupied
  * Queue depth and running request counts for load pressure signals


Prefix-cache locality is tracked by the EPP itself, not reported by replicas. The EPP maintains an in-memory prefix map that hashes each request's token prefix and records which replica last computed it.
On each incoming request, the EPP:
  1. Hashes the token prefix of the request (system prompt + conversation history up to this turn)
  2. Looks up which replicas have seen this prefix hash before
  3. Among those replicas, selects the one with available KV cache capacity (low `vllm:kv_cache_usage_perc`)
  4. If no replica has a prefix match, falls back to the replica with the lowest `vllm:kv_cache_usage_perc`
  5. Forwards the request to the selected replica and returns the response


The whole decision takes under 2ms of added latency. The EPP is stateful: it keeps an in-memory prefix map that tracks which replicas have computed which prefixes. When a replica restarts and clears its KV cache, the EPP's prefix map ages out the stale prefix entries for that replica and stops routing those prefixes there until the cache warms up again.
### The Gateway API Inference Extension CRDs
The Inference Extension adds a custom resource type to standard Kubernetes Gateway API:
**InferencePool:** Groups a set of backend pods into a named pool. Points to a pod selector and port. The EPP uses this to know which pods to poll and route to.
yaml

```
apiVersion: inference.networking.k8s.io/v1
kind: InferencePool
metadata:
  name: llama-70b-pool
spec:
  selector:
    matchLabels:
      app: llama-70b-worker
  targetPorts:
    - number: 8000
  endpointPickerRef:
    name: llama-70b-epp
```

The `InferenceModel` CRD was replaced by `InferenceObjective` when the project graduated to `inference.networking.k8s.io/v1`. `InferenceObjective` carries request priority and criticality settings that the EPP uses during scheduling. Model-name routing in the current API is handled through HTTPRoute rules that reference the InferencePool directly by name. For multi-model clusters, create one InferencePool per model and use separate HTTPRoutes (or header-based routing rules) to direct traffic to the correct pool.
For broader context on model routing architectures and how this fits into a tiered serving strategy, see the [LLM inference router guide](https://www.spheron.network/blog/llm-inference-router-gpu-cloud/).
## The Lock-In Question: What Is GKE-Specific vs Portable
Before committing to GKE Inference Gateway, it is worth being precise about which parts are Google-specific and which are not.  
| Component  | GKE-Specific?  | Open Source Alternative  |  
| --- | --- | --- |  
| Gateway API Inference Extension CRDs  | No  | Same CRDs on any cluster  |  
| llm-d Endpoint Picker (EPP)  | No  | Deploy from gateway-api-inference-extension repo  |  
| vLLM with `--enable-prefix-caching`  | No  | Same vLLM version on any GPU  |  
| GKE managed control plane  | Yes  | Self-managed Kubernetes  |  
| Google Cloud Load Balancer  | Yes  | Envoy Gateway, Istio, NGINX Gateway Fabric  |  
| GCP IAM and Workload Identity  | Yes  | RBAC on self-hosted cluster  |  
| GCP egress billing ($0.08-$0.12/GB)  | Yes  | No egress markup on bare-metal  |  
The routing intelligence, the CRDs, and the vLLM integration are fully portable. You pay GCP for the managed control plane and the load balancer provisioning. If you already operate Kubernetes clusters and want cache-aware inference routing without a managed service, you can deploy the exact same EPP binary and the same CRDs from the `kubernetes-sigs/gateway-api-inference-extension` repository.
The egress cost is worth calling out. At 1 TB/month of outbound traffic, GCP charges $80-120 in egress fees. Bare-metal GPU clusters do not carry this per-GB markup. For high-throughput inference serving, egress costs compound quickly.
## Replicating the Stack on Any GPU Cloud
Running the same open-source stack outside GCP takes about 45 minutes end to end. Here is the deployment path on Spheron bare-metal GPUs.
### Step 1: Provision GPU Nodes
For a 70B model at FP8, you need at least two H100 SXM5 nodes for the vLLM decode pool plus one smaller node for the Gateway and EPP process. The EPP is a lightweight Go binary and runs fine on a CPU node or a smaller GPU.
Deploy your H100 nodes from Spheron with per-minute billing. For 13B-40B models, [A100 80GB instances](https://www.spheron.network/gpu-rental/a100/) work well and cost less. SSH into each node and verify GPU access:
bash

```
nvidia-smi
# Should show your H100 SXM5 or A100 devices
```

For the vLLM baseline setup, see the [Spheron vLLM quick guide](https://docs.spheron.ai/quick-guides/llms/frameworks/vllm-server).
Provisioning fast isn't the same as serving fast on a cold start. Loading a 70B model's weights onto a freshly scaled replica still means moving 140GB from disk into GPU memory, and [GPU Direct Storage](https://www.spheron.network/blog/gpu-direct-storage-nvme-ai-training-inference-guide/) cuts that load time from minutes to under a minute by DMA'ing straight from NVMe instead of routing through a CPU staging buffer.
### Step 2: Install the Gateway API CRDs
On your Kubernetes cluster (you can use k3s or kubeadm on the provisioned nodes):
bash

```
# Gateway API CRDs (v1.3.0+ required)
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.3.0/standard-install.yaml

# Gateway API Inference Extension CRDs
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/latest/download/manifests.yaml
```

This installs the `InferencePool` CRD (`inference.networking.k8s.io/v1`) into your cluster.
### Step 3: Deploy vLLM with Prefix Caching
On each GPU node, start vLLM with the options the EPP needs:
bash

```
vllm serve meta-llama/Llama-3.1-70B-Instruct \
  --dtype fp8 \
  --enable-prefix-caching \
  --enable-chunked-prefill \
  --port 8000
```

The `--enable-prefix-caching` flag is what makes the EPP routing effective. Without it, the KV cache does not persist prefix blocks between requests, so routing to a specific replica gives no benefit. The EPP scrapes each replica's standard Prometheus `/metrics` endpoint (exposed on the same port as the vLLM API server) to read `vllm:kv_cache_usage_perc` and queue-depth counters.
### Step 4: Deploy the EPP
Clone the Inference Extension repo and apply the EPP deployment:
bash

```
git clone https://github.com/kubernetes-sigs/gateway-api-inference-extension.git
cd gateway-api-inference-extension

# Deploy the EPP
kubectl apply -f config/default/
```

### Step 5: Apply the InferencePool
yaml

```
# inferencepool.yaml
apiVersion: inference.networking.k8s.io/v1
kind: InferencePool
metadata:
  name: llama-70b-pool
  namespace: default
spec:
  selector:
    matchLabels:
      app: llama-70b-worker
  targetPorts:
    - number: 8000
  endpointPickerRef:
    name: endpoint-picker
```

bash

```
kubectl apply -f inferencepool.yaml
```

### Step 6: Wire Up Envoy Gateway
Install Envoy Gateway (simpler than Istio for this use case):
bash

```
helm install eg oci://docker.io/envoyproxy/gateway-helm \
  --version v1.3.0 \
  -n envoy-gateway-system \
  --create-namespace
```

Create the Gateway and HTTPRoute:
yaml

```
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: inference-gateway
  namespace: default
spec:
  gatewayClassName: envoy
  listeners:
    - name: http
      protocol: HTTP
      port: 80
---
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: llm-route
  namespace: default
spec:
  parentRefs:
    - name: inference-gateway
  rules:
    - backendRefs:
        - group: inference.networking.k8s.io
          kind: InferencePool
          name: llama-70b-pool
          port: 8000
```

Test it:
bash

```
curl http://<GATEWAY_IP>/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model": "meta-llama/Llama-3.1-70B-Instruct", "messages": [{"role": "user", "content": "Hello"}]}'
```

## Cost Comparison: Managed GKE Gateway vs Self-Hosted on a GPU Marketplace
Here is a concrete side-by-side for a two-replica 70B serving stack.
**Spheron bare-metal H100 SXM5 configuration:**
  * 2x H100 SXM5 nodes for vLLM: 2 x $3.17/hr = $6.34/hr
  * 1 CPU node for EPP + Envoy Gateway: ~$0.10/hr
  * Total: ~$6.44/hr
  * Egress cost: $0 markup on bare-metal


**Equivalent GKE configuration:**
  * 2x a3-highgpu-8g nodes (H100 equivalent): ~2 x $3.20/hr = $6.40/hr (approximate)
  * GKE managed control plane: $0.10/hr per cluster
  * Google Cloud Load Balancer: $0.025/hr + per-rule charges
  * Egress at 1 TB/mo: ~$80-120 one-time
  * Total: ~$6.50/hr plus egress


The compute gap closes if you factor in GKE's operational overhead savings (managed node upgrades, built-in monitoring). The egress gap is harder to close. At 1 TB/month outbound, GCP charges roughly $100 that you do not pay on bare-metal.
The EPP is a lightweight process. It does not justify a managed service tier on its own. The value of GKE Inference Gateway is the managed control plane, not the EPP binary.  
| Item  | Spheron Bare-Metal  | GKE  |  
| --- | --- | --- |  
| H100 SXM5 (per hr, per GPU)  | $3.17  | ~$3.20  |  
| A100 80G PCIe (per hr, per GPU)  | $1.48  | ~$1.90  |  
| Load balancer  | None  | $0.025/hr + rules  |  
| Egress (1 TB/mo)  | $0  | ~$100  |  
| EPP routing  | Open source  | Included  |  
| Managed control plane  | Self-managed  | Included  |  
> Pricing fluctuates based on GPU availability. The prices above are based on **22 Jun 2026** and may have changed. Check [current GPU pricing](https://www.spheron.network/pricing/) for live rates.
The two-replica configuration above assumes a fixed pool that stays up around the clock. If your traffic has real peaks and troughs, running replicas 24/7 wastes the exact GPU-hours cache-aware routing just saved you. [GPU inference autoscaling with KEDA and Knative](https://www.spheron.network/blog/keda-knative-gpu-autoscaling-kubernetes-llm-cold-start/) covers scaling the vLLM replica pool behind the gateway up and down with demand, including the cold-start tradeoffs that come with scale-to-zero.
Cache-aware routing also assumes every replica behind the gateway is serving the same model. If you're routing across many different models rather than replicas of one, prefix-cache locality alone doesn't solve the capacity problem; [token-level GPU pooling for multi-LLM marketplace inference](https://www.spheron.network/blog/token-level-gpu-pooling-multi-llm-marketplace-inference/) covers scheduling at the token-batch level across a shared pool of models instead of a fixed replica set per model.
## Benchmarking Cache-Aware vs Round-Robin
The performance gap between round-robin and KV-aware routing depends heavily on workload shape. Here is a concrete scenario: 1,000 requests on an 8-replica vLLM cluster, 4K-token system prompt shared across all requests (a typical chatbot or RAG pattern).
_Note: The numbers below are illustrative estimates based on modeled workload behavior, not production-measured benchmarks. Actual results vary by model size, context length, hardware configuration, and traffic distribution._
**Setup:**
  * 8x vLLM replicas, each on an H100 SXM5
  * Model: Llama 3.1 70B, FP8
  * System prompt: 4,096 tokens (about 3,000 words)
  * User messages: random, 50-200 tokens each


**Results:**  
| Metric  | Round-Robin  | KV-Aware (EPP)  | Improvement  |  
| --- | --- | --- | --- |  
| TTFT, p50  | 3.1s  | 0.38s  | 8.2x  |  
| TTFT, p95  | 4.9s  | 0.71s  | 6.9x  |  
| TTFT, p99  | 6.2s  | 1.2s  | 5.2x  |  
| Throughput (tokens/s)  | 840  | 2,620  | 3.1x  |  
| GPU prefill utilization  | 71%  | 9%  | -62pp  |  
| EPP routing overhead  | n/a  | 1.4ms  |  
The throughput gain comes from freeing prefill capacity. With round-robin, most of the GPU time on each replica goes toward recomputing the same 4K-token system prompt. With KV-aware routing, each replica computes the system prompt once and cache hits make up the rest. That freed GPU capacity goes toward decode, which is what the user waits for.
The EPP adds 1.4ms per request on average, which is negligible against the 11s prefill baseline.
For teams wanting to extend further than single-node prefix caching, [LMCache for distributed KV cache sharing](https://www.spheron.network/blog/deploy-lmcache-vllm-kv-cache-sharing-gpu-cloud/) adds a Redis-backed shared cache tier so any replica can serve prefixes computed on any other replica, removing the constraint that the routing must always hit the same node.
> If you want prefix caching and KV-aware routing without GCP lock-in or egress bills, the same open-source EPP stack runs on Spheron bare-metal GPUs at a fraction of the managed cost. Deploy on H100, A100, or L40S with per-minute billing and no contracts.
> [H100 on Spheron →](https://www.spheron.network/gpu-rental/h100/) | [View all GPU pricing →](https://www.spheron.network/pricing/) | [Get started →](https://app.spheron.ai)
STEPS / 07
## Quick Setup Guide
  1. 01
### Provision GPU instances on Spheron for the routing stack
Deploy at least two vLLM-capable GPU instances on Spheron for the decode pool, plus one lighter CPU or GPU instance for the Gateway and EPP control plane. For a 70B model at FP8, use H100 SXM5 instances. For 13B-40B models, A100 80GB or L40S are cost-efficient. SSH into each node and verify GPU access with nvidia-smi.
  2. 02
### Install the Gateway API CRDs and Inference Extension
Apply the Kubernetes Gateway API CRDs (v1.3.0+): kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.3.0/standard-install.yaml. Then apply the Gateway API Inference Extension CRDs: kubectl apply -f https://github.com/kubernetes-sigs/gateway-api-inference-extension/releases/latest/download/manifests.yaml. These CRDs add the InferencePool custom resource (inference.networking.k8s.io/v1).
  3. 03
### Deploy vLLM replicas with metrics endpoints enabled
On each GPU node, start vLLM with --enable-prefix-caching and --enable-chunked-prefill. The EPP scrapes each replica's Prometheus /metrics endpoint to read KV cache utilization (vllm:kv_cache_usage_perc). Example: vllm serve meta-llama/Llama-3.1-70B-Instruct --dtype fp8 --enable-prefix-caching --port 8000.
  4. 04
### Deploy the llm-d Endpoint Picker
Clone the Gateway API Inference Extension repo and deploy the EPP Deployment manifest. Configure the InferencePool CRD to list your vLLM service endpoints and set the routing policy to kv-cache-aware. The EPP reads KV cache utilization from each replica and routes incoming requests to the replica with the highest prefix cache hit probability for that request's token prefix.
  5. 05
### Create InferencePool resource
Define an InferencePool custom resource (apiVersion: inference.networking.k8s.io/v1) that groups your vLLM Deployment pods and points the endpointPickerRef at your EPP Service. The InferenceModel CRD was replaced by InferenceObjective in the v1 API (inference.networking.k8s.io/v1). InferenceObjective carries request priority and criticality settings used by the EPP. Multi-model routing is now handled through HTTPRoute rules pointing directly to the appropriate InferencePool per model.
  6. 06
### Configure the HTTPRoute to point to the InferencePool
Create a Kubernetes Gateway resource (use Envoy Gateway, Istio, or any Gateway API-compatible implementation) and an HTTPRoute that forwards /v1/* traffic to the InferencePool. The EPP intercepts requests at the gateway filter layer, picks the best backend, and forwards the request. Test with: curl http://<GATEWAY_IP>/v1/chat/completions -d '{"model": "meta-llama/Llama-3.1-70B-Instruct", "messages": [{"role": "user", "content": "Hello"}]}'.
  7. 07
### Benchmark cache-aware vs round-robin routing
Run a workload with repeated system prompts (simulating a chatbot or RAG system). Compare TTFT on the first request vs the 10th request with the same prefix. With round-robin, TTFT stays flat at ~11s per request because each request lands on a different replica. With KV-aware routing, TTFT for requests sharing a prefix drops to ~1.5s after the first cache population, matching the behavior you would see on GKE Inference Gateway.


FAQ / 05
## Frequently Asked Questions
### 01What is the GKE Inference Gateway?
GKE Inference Gateway is Google's managed inference routing layer built on top of the Kubernetes Gateway API Inference Extension. It uses the llm-d Endpoint Picker (EPP) to route LLM requests based on KV cache state and active LoRA adapters rather than round-robin, reducing TTFT and improving GPU utilization in multi-replica vLLM deployments.
### 02What is the llm-d Endpoint Picker?
The llm-d Endpoint Picker (EPP) is an open-source plugin for the Kubernetes Gateway API Inference Extension. It scrapes each vLLM replica's Prometheus /metrics endpoint to read KV cache utilization (vllm:kv_cache_usage_perc) and queue depth. It also maintains an internal prefix map that hashes token prefixes and tracks which replicas have cached which prompts, then routes each incoming request to the replica most likely to serve it from existing cache. It is the routing brain behind both the self-hosted llm-d stack and GKE Inference Gateway.
### 03Is the GKE Inference Gateway stack portable outside GCP?
Yes. The Gateway API Inference Extension and the llm-d Endpoint Picker are open-source Kubernetes components. You can deploy them on any Kubernetes cluster, including self-hosted clusters on bare-metal GPUs. Only the GKE-managed control plane, the Google-provisioned load balancer, and the GCP egress costs are GCP-specific. The routing logic, CRDs, and vLLM integration are identical.
### 04Why does round-robin load balancing hurt LLM inference?
Round-robin ignores KV cache state on each replica. A request whose system prompt was already computed and cached on replica A gets routed to replica B, forcing a full re-prefill from scratch. On a long system prompt at 70B parameter scale, this can waste 10+ seconds of GPU time per request compared to routing it to the replica holding the prefix cache. KV-aware routing eliminates this waste.
### 05How much does GKE Inference Gateway cost vs self-hosting the EPP on a GPU marketplace?
GKE Inference Gateway costs include the GKE cluster node fee, the managed load balancer hourly charge, and GCP egress fees ($0.08-$0.12/GB for inter-region, $0.04-$0.08/GB for egress to internet). Self-hosting the open-source EPP and Gateway API Inference Extension on Spheron bare-metal GPUs incurs no load balancer surcharge and no egress markup. The EPP itself is a lightweight Go process running on the same node as the gateway.
## Related Reading
[More on Infrastructure →](https://www.spheron.network/blog/topics/infrastructure/)
[ Pillar Guide GPU Monitoring for ML: nvidia-smi, DCGM, and Production Observability Guide Jan 15, 2026 ](https://www.spheron.network/blog/gpu-monitoring-for-ml/)[ Tutorial NVIDIA Grove on GPU Cloud: Kubernetes-Native Orchestration for Disaggregated LLM Inference (2026) Jun 22, 2026 ](https://www.spheron.network/blog/nvidia-grove-kubernetes-disaggregated-inference-guide/)[ Engineering Multi-Tenant LLM Serving on GPU Cloud: Per-Customer Isolation, Token Quotas, and Production SaaS Architecture Guide (2026) Jun 7, 2026 ](https://www.spheron.network/blog/multi-tenant-llm-serving-gpu-cloud/)[ Tutorial LLM-as-Judge Evaluation Pipelines on GPU Cloud: Build Production Model Evaluation Infrastructure (2026 Guide) Apr 22, 2026 ](https://www.spheron.network/blog/llm-as-judge-evaluation-pipeline-gpu-cloud/)[ Engineering GPU Count vs Training Speedup: Debunking the Myth Sep 10, 2026 ](https://www.spheron.network/blog/myth-doubling-your-gpu-count-halves-training-time/)[ Engineering KV Cache Quantization vLLM Setup: KIVI's INT2 Method (2026) Sep 5, 2026 ](https://www.spheron.network/blog/kv-cache-quantization-with-kivi-int2-setup-guide-2026/)
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
