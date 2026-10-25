---
title: 'GPU Sharing on Kubernetes: Time-Slicing, MIG, and MPS Compared for LLM Workloads
  - Pongsak Khamdee'
id: gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads-po
tags:
- onprem-llm-inference-stacks-859099
- gpu-sharing
- provenance-practitioner-blog
- dra-kubernetes
created: '2026-10-23T17:23:08.056663Z'
updated: '2026-10-23T17:25:05.327731Z'
source: https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/
source_domain: pkhamdee.blog
fetched_at: '2026-10-23T17:23:08.029318Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: Practitioner blog (Pongsak Khamdee, 17 Jun 2026) on time-slicing/MIG/MPS/DRA
  for LLM inference. Recommends 4 time-sliced replicas per H100 for same-model vLLM
  fleets (GPU Operator v26.3.2), MIG 1g.20gb for cross-team isolation, MPS only for
  training experiments, notes DRA is beta (K8s 1.34-1.36) and NVIDIA DRA driver v0.4.0
  (2026-05-15) does not officially support general GPU allocation (only ComputeDomain/MNNVL).
  Honest 'what I could not verify' section; no measured benchmarks (says 'measure
  for your case'); MPS config snippet is garbled (missing mps key) and MPS/time-slicing
  mutual exclusivity not stated.
---

# GPU Sharing on Kubernetes: Time-Slicing, MIG, and MPS Compared for LLM Workloads[¶](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads "Permanent link")
You have 8 H100s. You have 30 inference pods that need one GPU each. The naive answer — buy 22 more H100s — is a quarter-million-dollar mistake. The right answer is GPU sharing, and there are four of them now: **time-slicing, MIG, MPS, and DRA**. Pick wrong and you get OOM kills, throughput collapse, or training jobs that silently corrupt each other. Pick right and you run 30 pods on 8 GPUs with measurable isolation guarantees.
This post walks each option with the actual config, the actual tradeoffs, and the decision tree that tells you which one to use for which LLM workload.
GPU sharing isn't free. Every sharing mode trades something — memory isolation, fault isolation, scheduling precision, or operational complexity. The four modes also don't compose freely. A pod scheduled via DRA on a MIG slice behaves differently than one on a time-sliced replica of the same GPU. Get the layering wrong and you'll debug for days.
## The four sharing modes, one line each[¶](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#the-four-sharing-modes-one-line-each "Permanent link")  
| Mode  | What it does  | Isolation  | Best for  |  
| --- | --- | --- | --- |  
| **Time-slicing**  | Context-switches GPU access across replicas  | None (shared memory, shared fault domain)  | Bursty inference with small models, low cost  |  
| **MIG**  | Hardware partition into isolated GPU instances  | Hardware-level (memory + fault)  | Steady inference at 7B-13B scale, multi-tenant  |  
| **MPS**  | Space-partition GPU compute + memory, software-enforced limits  | Memory only (no fault isolation)  | Many small CUDA contexts, training experiments  |  
| **DRA**  | Scheduling primitive (works with any of the above)  | Depends on backing resource  | Mixed-tenant clusters, gang scheduling, topology  |  
The first three are **physical sharing mechanisms**. DRA is a **scheduling framework** that can request any of the above. You don't pick DRA _or_ MIG; you pick DRA _over_ MIG.
## Mode 1: time-slicing (the cheap default)[¶](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#mode-1-time-slicing-the-cheap-default "Permanent link")
Time-slicing is the simplest and the most common. The device plugin advertises N replicas of each physical GPU. A pod requesting `nvidia.com/gpu: 1` gets scheduled onto a replica; the plugin context-switches between replicas at the CUDA layer.
**Config** (from the k8s-device-plugin README):

```
apiVersion:v1
kind:ConfigMap
metadata:
name:nvidia-device-plugin
config.yaml:|
version: v1
sharing:
timeSlicing:
resources:
- name: nvidia.com/gpu
replicas: 4

```

On a node with 8 H100s, this makes Kubernetes see `nvidia.com/gpu: 32`. A typical ceiling for inference replicas is 4-8 per GPU; going much higher and context-switch overhead starts to dominate (workload-dependent, measure for your case).
**What time-slicing does not give you:**
  * **No memory isolation.** All replicas share the GPU's HBM. If two pods each try to allocate 70 GB on a 80 GB card, both OOM.
  * **No fault isolation.** One pod crashing the GPU kills all replicas on it.
  * **No QoS.** A pod that issues 100 kernels per second gets the same scheduling weight as one that issues 1.


**The right use case:** vLLM serving multiple replicas of a 7B-13B model where each replica uses ~10-15 GB of VRAM. With 4 replicas per H100, you have 80 GB of usable VRAM and 4 pods sharing the SMs. The model load is amortized across replicas if you share the model cache (separate vLLM `--served-model-name` + shared `--model` path). Reported context-switch overhead is low for batched inference but varies by workload — measure for your specific model and batch size rather than trusting a single number.
**The wrong use case:** running a 70B model that needs 140 GB on time-sliced GPUs. You have one card; you can't time-slice a model that's bigger than the card. MIG (or two cards with tensor parallelism) is the answer.
## Mode 2: MIG (hardware isolation, the safest bet)[¶](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#mode-2-mig-hardware-isolation-the-safest-bet "Permanent link")
MIG (Multi-Instance GPU) partitions a physical GPU at the hardware level into isolated instances. Each instance has dedicated SMs, memory controllers, and L2 cache partitions. An OOM in one MIG slice does not affect the others. A kernel fault in one slice does not crash the rest of the card.
**Supported on:** A100 40GB/80GB, A30, H100, H200, B200, GB200, L4, L40S, and RTX PRO 6000 Blackwell Server Edition. Not on consumer cards (RTX 4090, RTX 5090). Note that MIG instance counts vary: H100/H200/GB200 support up to 7 instances per GPU; the RTX PRO 6000 BSE supports up to 4.
**H100/H200 profile sizes** (the common 1-3-7 layout):  
| Profile  | SMs  | Memory  | Memory bandwidth  | Use case  |  
| --- | --- | --- | --- | --- |  
| `1g.10gb`  | 1/7  | ~10 GB  | 1/7  | Tiny models, embeddings, classifier heads  |  
| `1g.20gb`  | 1/7  | ~20 GB  | 1/7  | 7B FP16 inference  |  
| `2g.20gb`  | 2/7  | ~20 GB  | 2/7  | 7B FP16 with headroom for KV cache  |  
| `3g.40gb`  | 3/7  | ~40 GB  | 3/7  | 13B FP16, 7B FP8  |  
| `4g.40gb`  | 4/7  | ~40 GB  | 4/7  | 13B with batch headroom  |  
| `7g.80gb`  | 7/7  | ~80 GB  | full  | Full card, no sharing  |  
Note: these are the **H100/A100** profile names. H200 uses the same 1g.10gb / 1g.20gb / 2g.20gb / 3g.40gb / 4g.40gb / 7g.80gb layout (H200 has 141 GB, so the largest MIG profile gets proportionally more). GB200 has its own scale: a 186 GB card can be sliced into 2×93 GB, 4×46 GB, or 7×23 GB instances. The key constraint: once you slice, **the slice is the slice** — you cannot dynamically resize without a node reboot.
**Config** (via the GPU Operator's MIG manager):

```
apiVersion:v1
kind:ConfigMap
metadata:
name:nvidia-mig-manager
namespace:gpu-operator
config.yaml:|
version: v1
mig-config:
- devices: all
mig-enabled: true
attributes:
- "1g.20gb"
- "1g.20gb"
- "1g.20gb"

```

This configures every H100 on the node as three `1g.20gb` slices. The k8s-device-plugin (deployed by the GPU Operator) then advertises each slice as a separate `nvidia.com/gpu` resource, labeled `nvidia.com/mig.config.state: "success"` and `nvidia.com/mig.profile: "1g.20gb"`.
**The right use case:** a 30-pod deployment of a 7B model where each pod needs ~15 GB. With `1g.20gb` MIG slices, you get 3 slices per H100, hardware-isolated. Each pod has 20 GB guaranteed. No OOM bleed. No fault bleed. Slightly lower per-pod throughput than full-GPU (you gave up 5/7 of the SMs), but predictable.
**The wrong use case:** workloads that need to share model weights across pods. MIG slices are isolated at the HBM level — there's no way for two slices to share a model cache. If your serving pattern is "10 replicas of the same 7B model," you pay the model load cost 10 times, once per slice. Time-slicing is cheaper.
## Mode 3: MPS (compute sharing, the underused option)[¶](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#mode-3-mps-compute-sharing-the-underused-option "Permanent link")
CUDA MPS (Multi-Process Service) is the oldest of the four. It runs a control daemon that partitions SM time across multiple CUDA contexts and enforces memory caps per client. Unlike time-slicing, MPS gives you **explicit memory limits** per process. Unlike MIG, MPS is **software-enforced** — a buggy MPS client can exceed its limit under load.
**Config** (from the k8s-device-plugin README):

```
apiVersion:v1
kind:ConfigMap
metadata:
name:nvidia-device-plugin
config.yaml:|
version: v1
sharing:
resources:
- name: nvidia.com/gpu
replicas: 8

```

**What MPS gives you:**
  * Memory limits per pod (enforced at the CUDA driver level, not the K8s scheduler)
  * SM sharing with lower overhead than time-slicing (MPS schedules at kernel-launch granularity, not full context-switch)
  * No model-load duplication if the processes share model files at the OS level


**What MPS does not give you:**
  * Fault isolation (one process crashing still brings down the MPS daemon, often taking the others with it)
  * GPU memory bandwidth isolation (two bandwidth-bound processes can still starve each other)


**The right use case:** training experiments where you want to run 4-8 small fine-tuning jobs on one H100, each capped at 20 GB. MPS lets you oversubscribe compute without oversubscribing memory, which is the actual binding constraint for LoRA/QLoRA fine-tuning.
**The wrong use case:** inference serving. The fault-isolation gap is too dangerous for production inference pods. A 4% improvement in throughput over time-slicing isn't worth the risk of a memory leak in one pod killing all the others.
## Mode 4: DRA (the scheduling layer, the 2026 story)[¶](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#mode-4-dra-the-scheduling-layer-the-2026-story "Permanent link")
Dynamic Resource Allocation (DRA) is the Kubernetes-native answer to "the device plugin model is too rigid." DRA introduces `ResourceClaim` and `ResourceClaimTemplate` as first-class API objects, lets drivers expose custom selection logic, and supports structured sharing parameters per resource request.
DRA went **beta in Kubernetes 1.34** and stayed beta through 1.35 and 1.36. The current API versions in 1.36 are `resource.k8s.io/v1beta1` for ResourceClaim/ResourceClaimTemplate/DeviceClass and `resource.k8s.io/v1alpha3` for DeviceTaintRule.
**DRA isn't a sharing mechanism on its own.** It can request MIG slices, time-sliced replicas, plain GPUs, or anything a DRA driver exposes. What DRA adds is:
  * **Structured parameters per claim** (e.g., "request 30 GB of GPU memory and 5 GB/s of bandwidth" rather than "request 1 nvidia.com/gpu")
  * **Gang scheduling** (claim a set of GPUs that all land on the same node)
  * **Topology awareness** (claim a specific NVLink island)
  * **Per-driver selection logic** (the driver decides which physical resource best fits the request)


**Config** (DRA claim referencing a DeviceClass for NVIDIA GPUs):

```
apiVersion:resource.k8s.io/v1beta1
kind:ResourceClaimTemplate
metadata:
name:llm-gpu-claim
devices:
requests:
-name:gpu
deviceClassName:gpu.nvidia.com
selectors:
expression:device.attributes["nvidia.com"].memory >= "30Gi"

```

This claims any NVIDIA GPU with at least 30 GB of memory. The DRA driver for NVIDIA GPUs (`kubernetes-sigs/dra-driver-nvidia-gpu` v0.4.0, 2026-05-15) handles the actual allocation. **Caveat:** as of v0.4.0 the GPU allocation side is _not yet officially supported_ and is disabled by default in the Helm chart — only the ComputeDomain side (for Multi-Node NVLink) is officially supported today. Treating DRA as production-ready for general GPU allocation in mid-2026 is premature; verify the project's release notes before betting on it.
**The right use case:** multi-tenant clusters where pods have different GPU requirements (some need 80 GB, some need 20 GB, some need specific NVLink topology). DRA's structured selection eliminates the manual "label-mash" you do with the device plugin.
**The wrong use case:** small single-tenant clusters. The setup cost (DRA driver, DeviceClass definitions, claim templates per workload) only pays off when you have heterogeneous requirements and don't want to write a custom scheduler.
## The decision tree[¶](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#the-decision-tree "Permanent link")
Start here, follow the arrows, land on one mode:

```
How many pods share a GPU?
├─ 1 (dedicated) → no sharing mode needed, request nvidia.com/gpu: N
├─ 2-4, similar workload
│   ├─ Need model cache shared across pods? → time-slicing
│   ├─ Need hard memory limits per pod? → MIG (H100/A100/H200/B200/L40S/RTX PRO 6000 BSE)
│   └─ Need fine-grained compute limits, fault-isolation not critical? → MPS
├─ 5-8, mixed workload sizes
│   ├─ All pods need same hardware slice? → MIG with uniform profile
│   └─ Pods need different slice sizes? → DRA over MIG
└─ 8+, mostly inference with same model
    └─ time-slicing (the only mode that scales to 8+ without MIG-capable hardware)

```

The second branch is where 80% of LLM deployments live. For a 30-pod fleet of 7B vLLM replicas on H100, the answer is **time-slicing with 4 replicas per H100** , **unless** the pods come from different teams and you need memory-isolation guarantees, in which case **MIG with`1g.20gb` slices**.
## The config that wins in 2026[¶](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#the-config-that-wins-in-2026 "Permanent link")
For a typical LLM inference fleet on H100 80GB:

```
# gpu-operator-values.yaml
enabled:true
devicePlugin:
name:time-slicing-config
default:false
configs:
-name:time-slicing-config
sharing:
timeSlicing:
resources:
-name:nvidia.com/gpu
replicas:4
migManager:
enabled:false
driver:
enabled:true

```

This gives you 4 time-sliced replicas per H100. A pod requests `nvidia.com/gpu: 1` and gets a slice. vLLM serves the model from a shared model cache. The scheduler sees 4× more GPUs than you physically have, so the bin-packing is efficient.
If you need to mix time-slicing with MIG (some pods in slices, some in replicas), you label nodes with their config and the device plugin respects per-node configuration via node labels.
## What I couldn't fully verify[¶](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#what-i-couldnt-fully-verify "Permanent link")
  * **H200 MIG profile sizes** — the 1g/2g/3g/4g/7g layout is the same as H100, but exact memory per slice for H200 (141 GB total) is not explicitly documented in the k8s-device-plugin README; the GPU Operator docs reference H100 only.
  * **MPS memory limits under load** — the software-enforced limit can be exceeded by a misbehaving client; the failure mode (process OOM-killed vs container OOM-killed) is implementation-specific.
  * **DRA + MIG composition** — the GPU allocation side of the DRA driver is not yet officially supported as of v0.4.0, so MIG slices via DRA is currently aspirational. Stick with the legacy device-plugin path for MIG.


## Summary[¶](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#summary "Permanent link")
  * **Time-slicing** is the cheap default for many-pod inference of the same model. No isolation, no model-cache sharing benefit, but no setup cost.
  * **MIG** (H100/A100/H200/B200/GB200/L4/L40S/RTX PRO 6000 BSE; not consumer RTX cards) is the only mode with hardware-level memory + fault isolation. Best for multi-tenant or mixed-workload deployments where OOM bleed is unacceptable.
  * **MPS** is the underused option for training experiments where you want explicit memory caps. Avoid for production inference.
  * **DRA** (DRA API in beta in K8s 1.36; DRA driver for NVIDIA GPUs at v0.4.0 is **not yet officially supported** for general GPU allocation — ComputeDomain/NVLink is the only production-ready path) is the scheduling layer, not a sharing mechanism on its own. Worth it for multi-tenant clusters with heterogeneous requirements, but verify release status before betting on it.
  * **Decision tree:** same model + many pods = time-slicing; mixed workload + isolation = MIG; training experiments = MPS; heterogeneous + complex = DRA over the above.
  * **Config that wins in 2026 for H100 inference fleets:** GPU Operator v26.3.2 with time-slicing at 4 replicas per H100, MIG manager off.


_Questions or discussion? Connect on[LinkedIn](https://www.linkedin.com/in/pongsak-khamdee-737483a0/), [X](https://x.com/pongsak_khamdee) or reach out via email._
  1. NVIDIA k8s-device-plugin v0.19.2 README — sections "With CUDA Time-Slicing" and "With CUDA MPS" define the sharing config schema. License: Apache 2.0. https://github.com/NVIDIA/k8s-device-plugin [↩](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#fnref:k8s-device-plugin-readme "Jump back to footnote 1 in the text")[↩](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#fnref2:k8s-device-plugin-readme "Jump back to footnote 1 in the text")
  2. Kubernetes docs, "Dynamic Resource Allocation" — DRA APIs at `resource.k8s.io/v1beta1` (ResourceClaim, ResourceClaimTemplate, DeviceClass) and `v1alpha3` (DeviceTaintRule) as of K8s 1.36. https://kubernetes.io/docs/concepts/scheduling-eviction/dynamic-resource-allocation/ [↩](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#fnref:dra-doc "Jump back to footnote 2 in the text")
  3. Kubernetes Enhancement Proposal KEP-4815 — the DRA track in sig-scheduling. Beta in 1.34, still beta in 1.36. https://github.com/kubernetes/enhancements/tree/master/keps/sig-scheduling [↩](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#fnref:kep-4815 "Jump back to footnote 3 in the text")
  4. NVIDIA GPU Operator v26.3.2 (2026-05-29) — manages k8s-device-plugin, MIG manager, driver container, and DCGM via the operator framework. License: Apache 2.0. https://github.com/NVIDIA/gpu-operator [↩](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#fnref:gpu-operator-readme "Jump back to footnote 4 in the text")
  5. NVIDIA GPU Operator release notes — added support for new MIG profiles with HGX B200 and HGX GB200. https://github.com/NVIDIA/gpu-operator/releases [↩](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#fnref:gpu-operator-rn "Jump back to footnote 5 in the text")
  6. DRA Driver for NVIDIA GPUs (kubernetes-sigs/dra-driver-nvidia-gpu) v0.4.0 (2026-05-15) — README explicitly states: "While some GPU allocation features can be tried out, they are not yet officially supported. Hence, the GPU kubelet plugin is currently disabled by default in the Helm chart installation." ComputeDomain (Multi-Node NVLink) is the only officially supported feature in v0.4.0. https://github.com/kubernetes-sigs/dra-driver-nvidia-gpu [↩](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#fnref:dra-driver-readme "Jump back to footnote 6 in the text")
  7. NVIDIA Multi-Instance GPU (MIG) user guide — H100 and A100 MIG profile names: 1g.10gb, 1g.20gb, 2g.20gb, 3g.40gb, 4g.40gb, 7g.80gb. https://docs.nvidia.com/datacenter/tesla/mig-user-guide/ [↩](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#fnref:mig-profiles "Jump back to footnote 7 in the text")
  8. NVIDIA MIG documentation — GB200 (186 GB) can be partitioned into 2×93 GB, 4×46 GB, or 7×23 GB MIG instances. https://docs.nvidia.com/datacenter/tesla/mig-user-guide/ [↩](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#fnref:gb200-mig "Jump back to footnote 8 in the text")
  9. NVIDIA RTX PRO 6000 Blackwell Server Edition product page — 96 GB GDDR7 with ECC, 1,597 GB/s memory bandwidth, 24,064 CUDA cores, GB202 GPU, 600 W. https://www.nvidia.com/en-us/products/workstations/professional-desktop-gpus/rtx-pro-6000/ [↩](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#fnref:rtxpro6000-spec "Jump back to footnote 9 in the text")
  10. NVIDIA RTX PRO 6000 BSE MIG support — "Multi-Instance GPU (MIG) expands the performance and value of RTX PRO 6000, enabling the creation of up to four fully isolated instances." (Up to 4 instances per GPU, vs. 7 for H100/H200/GB200.) https://www.nvidia.com/en-us/products/workstations/professional-desktop-gpus/rtx-pro-6000/ [↩](https://pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/#fnref:rtxpro6000-mig "Jump back to footnote 10 in the text")


Share this post [ Share on X ](https://x.com/intent/tweet?url=https%3A//pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/&text=GPU%20Sharing%20on%20Kubernetes%3A%20Time-Slicing%2C%20MIG%2C%20and%20MPS%20Compared%20for%20LLM%20Workloads) [ Share on Facebook ](https://www.facebook.com/sharer/sharer.php?u=https%3A//pkhamdee.blog/2026/06/17/gpu-sharing-on-kubernetes-time-slicing-mig-and-mps-compared-for-llm-workloads/)
## Discussion
Have thoughts on this post? Share them below — questions, corrections, or your own experience are all welcome.
Back to top 
