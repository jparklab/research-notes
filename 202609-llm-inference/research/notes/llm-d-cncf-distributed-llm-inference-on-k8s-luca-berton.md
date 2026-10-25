---
title: 'llm-d: CNCF Distributed LLM Inference on K8s | Luca Berton'
id: llm-d-cncf-distributed-llm-inference-on-k8s-luca-berton
tags:
- internal-llm-inference-stacks-7f0c90
- llm-d
- cncf
- practitioner-opinion
created: '2026-10-09T22:53:49.903758Z'
updated: '2026-10-09T22:56:23.096668Z'
source: https://lucaberton.com/blog/llm-d-cncf-kubernetes-distributed-inference-2026/
source_domain: lucaberton.com
fetched_at: '2026-10-09T22:53:49.879098Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Practitioner/consultant blog (Luca Berton, AI/Cloud advisor selling ''Production
  AI Readiness Assessment'' consultations -- promotional framing, lower authority,
  treat predictions as opinion not fact) covering llm-d''s CNCF Sandbox acceptance,
  announced at KubeCon Europe 2026. States founding consortium as ''IBM Research,
  Google, Red Hat, CoreWeave, and NVIDIA'' with later joiners ''AMD, Cisco, Intel,
  Lambda,'' plus Hugging Face (model ecosystem) and Mistral AI (named as actively
  contributing a ''DisaggregatedSet operator for LeaderWorkerSet (LWS) back to the
  project'' -- this is a specific, checkable upstream-contribution claim, not just
  a logo-wall mention) and research involvement from UC Berkeley and University of
  Chicago. Frames llm-d''s position relative to other CNCF-adjacent projects: ''llm-d
  bridges the gap between KServe (high-level model serving control plane) and vLLM
  (low-level inference engine).'' Also claims integration with the ''Kubernetes AI
  Conformance Program'' for disaggregated-serving interoperability, though this is
  asserted without supporting detail. The author''s own prediction (''llm-d will become
  the standard way to serve LLMs on Kubernetes within 18 months'') and performance
  claims (''I have seen 2-3x improvement in tokens per second per GPU in similar architectures'')
  are first-person opinion/anecdote from the consultant, not sourced to llm-d or a
  named production deployer -- flag as unverified anecdotal evidence, not a confirmed
  production claim.'
utility_score: 11.0
---

[Skip to main content](https://lucaberton.com/blog/llm-d-cncf-kubernetes-distributed-inference-2026/#main-content)
[🚀 Taking AI from prototype to production? | Find the architecture, GPU, security and governance gaps before they become incidents. Get a Production AI Readiness Assessment ](https://lucaberton.com/production-ai-assessment/)
The CNCF just accepted **llm-d** as a sandbox project, and it might be the most important AI infrastructure project of 2026. Built by IBM Research, Google, Red Hat, CoreWeave, and NVIDIA, llm-d turns Kubernetes into a state-of-the-art LLM inference platform.
I saw this announced at [KubeCon Europe 2026](https://lucaberton.com/kubecon/) and immediately understood why it matters.
## What Problem Does llm-d Solve?
LLM inference is not like serving a REST API. It is:
  * **Highly stateful** — KV caches must be preserved across requests for efficiency
  * **Latency-sensitive** — users wait in real time for token generation
  * **Asymmetric** — prompt processing (prefill) is compute-heavy; token generation (decode) is memory-bound
  * **Expensive** — GPU utilization directly translates to cost per token


Traditional Kubernetes load balancing (round-robin, least-connections) ignores all of this. It sends requests to random pods, fragmenting KV caches, misaligning workloads with hardware, and causing unpredictable latency.
llm-d fixes this by making Kubernetes inference-aware.
## Key Capabilities
### 1. Prefill/Decode Disaggregation
The breakthrough architecture: separate prompt processing from token generation into independently scalable pods.
**Why this matters:**
  * Prefill is compute-bound (needs GPU FLOPS)
  * Decode is memory-bound (needs GPU bandwidth)
  * Same hardware cannot optimally serve both
  * Disaggregation lets you scale each phase independently


```
Request → [Prefill Pods] → KV Cache Transfer → [Decode Pods] → Response
           (GPU compute)                        (GPU memory)
```

This is the same architecture that Mistral AI uses internally. They are contributing a `DisaggregatedSet` operator for LeaderWorkerSet (LWS) back to the project.
### 2. Inference-Aware Traffic Routing
llm-d implements the Kubernetes Gateway API Inference Extension (GAIE) with an Endpoint Picker (EPP) that understands:
  * **Prefix cache locality** — route requests to pods that already have relevant KV cache entries
  * **Model readiness** — not just container health, but model loading state
  * **Queue depth** — actual inference queue, not just TCP connections
  * **Hardware topology** — route to pods on matching accelerator types


### 3. Hierarchical KV Cache Management
KV cache offloading across multiple tiers:

```
GPU HBM (fastest) → CPU RAM → NVMe SSD → Network Storage (cheapest)
```

Hot caches stay on GPU. Warm caches spill to CPU. Cold caches go to disk. This maximizes GPU utilization while keeping frequently used contexts accessible.
### 4. Any Model, Any Accelerator, Any Cloud
llm-d is hardware-agnostic by design:
  * NVIDIA GPUs (A100, H100, B200)
  * AMD GPUs (MI300X)
  * Intel GPUs (Gaudi)
  * Google TPUs
  * Habana accelerators


The routing layer adapts request placement based on hardware characteristics — a request that benefits from high memory bandwidth goes to different hardware than one that needs compute throughput.
## Architecture on Kubernetes

```
# llm-d uses LeaderWorkerSet for multi-node inference
apiVersion: leaderworkerset.x-k8s.io/v1
kind: LeaderWorkerSet
metadata:
  name: llm-inference
spec:
  replicas: 3
  leaderWorkerTemplate:
    leaderTemplate:
      spec:
        containers:
name: prefill-engine
            image: ghcr.io/llm-d/prefill:latest
            resources:
              limits:
                nvidia.com/gpu: "4"
    workerTemplate:
      spec:
        containers:
name: decode-engine
            image: ghcr.io/llm-d/decode:latest
            resources:
              limits:
                nvidia.com/gpu: "2"
```

## Who Is Behind It
The founding consortium is impressive:
  * **IBM Research** — core architecture and KV cache management
  * **Google Cloud** — Gateway API integration and GKE optimization
  * **Red Hat** — OpenShift integration and operator development
  * **CoreWeave** — bare-metal GPU infrastructure expertise
  * **NVIDIA** — hardware optimization and accelerator support
  * **AMD, Cisco, Intel, Lambda** — multi-accelerator support
  * **Hugging Face** — model ecosystem integration
  * **Mistral AI** — disaggregated serving operator contributions
  * **UC Berkeley and University of Chicago** — research


## How It Relates to Existing CNCF Projects
llm-d bridges the gap between:
  * **KServe** (high-level model serving control plane) and
  * **vLLM** (low-level inference engine)


It also integrates with the [Kubernetes AI Conformance Program](https://lucaberton.com/blog/kubernetes-ai-conformance-program-cncf-kubecon-2026/) to ensure disaggregated serving is interoperable across platforms.
## My Prediction
llm-d will become the standard way to serve LLMs on Kubernetes within 18 months. The combination of CNCF governance, multi-vendor backing, and solving a real technical problem (inference-aware scheduling) makes it inevitable.
For platform teams currently running vLLM behind basic Kubernetes services: watch this project closely. The efficiency gains from prefill/decode disaggregation and cache-aware routing are substantial — I have seen 2-3x improvement in tokens per second per GPU in similar architectures.
## Related Resources
  * [AI on Kubernetes in Production](https://lucaberton.com/blog/ai-kubernetes-poc-production-gap/)
  * [Kubernetes AI Conformance Program](https://lucaberton.com/blog/kubernetes-ai-conformance-program-cncf-kubecon-2026/)
  * [GPU Kubernetes Guide](https://lucaberton.com/blog/gpu-kubernetes-getting-started-nvidia-2026/)
  * [Multi-Tenant GPUs on Bare Metal](https://lucaberton.com/blog/kubecon-2026-talk-multi-tenant-gpus-recap/)
  * [NVIDIA GPU Operator](https://lucaberton.com/blog/nvidia-gpu-operator-kubernetes-setup/)
  * [KEDA vs HPA Autoscaling](https://lucaberton.com/blog/keda-vs-hpa-2026/)


## About the Author
I am Luca Berton, AI and Cloud Advisor. I [presented on GPU scheduling at KubeCon EU 2026](https://lucaberton.com/kubecon/) and help enterprises deploy inference infrastructure. [Book a consultation](https://calendly.com/lucaberton/) to design your LLM serving architecture.
## Related Articles
[ AIllm-d: Kubernetes-Native Distributed Inference at Scale Red Hat's llm-d project enters CNCF Sandbox, bringing disaggregated xPyD serving, tiered prefix caching, and multi-accelerator support to Kubernetes. ](https://lucaberton.com/blog/llm-d-kubernetes-native-distributed-inference/)[ AIInference Gold Rush: AI Startup Cost Economics 2026 AI inference is a $255B market by 2030. Baseten, Fireworks, Modal Labs lead a funding frenzy. Here is why Kubernetes-native inference with disaggregated. ](https://lucaberton.com/blog/inference-gold-rush-ai-startups-kubernetes-2026/)[ AIRed Hat AI Model-as-a-Service with llm-d How Red Hat's llm-d transforms LLM inference into a composable Kubernetes-native architecture: disaggregated serving, smart autoscaling, and MaaS. ](https://lucaberton.com/blog/red-hat-ai-model-as-a-service-llm-d-maas-platform-2026/)[ AINVIDIA Dynamo: Why It Replaces Triton for LLM Serving NVIDIA Dynamo is the open-source successor to Triton. Disaggregated prefill/decode, KV-cache-aware routing, and 2-3x throughput gains for vLLM deployments. ](https://lucaberton.com/blog/nvidia-dynamo-inference-framework-distributed-serving-2026/)
Need expert guidance?
Free 30-min consultation with Luca Berton
Free 30-min Production AI consultation
[Book Now](https://calendly.com/lucaberton/)
## We value your privacy
This website uses cookies to ensure the best experience. Essential cookies are required for the site to function correctly, while analytics cookies help us improve our content and services. You can manage your preferences below.
Accept allReject all
Manage preferences
[Privacy Policy](https://lucaberton.com/privacy-policy/) [Terms and Conditions](https://lucaberton.com/terms/)
