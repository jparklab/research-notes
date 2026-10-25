---
title: 'Fractional GPUs for AI Inference: vGPU, MPS, and Right-Sizing Your GPU Cloud
  Spend (2026 Guide) | Spheron'
id: fractional-gpus-for-ai-inference-vgpu-mps-and-right-sizing-your-gpu-cloud-spend
tags:
- onprem-llm-inference-stacks-859099
- gpu-sharing
- provenance-vendor-blog
- low-reliability
created: '2026-10-23T17:24:27.240276Z'
updated: '2026-10-23T17:25:07.028016Z'
source: https://www.spheron.network/blog/fractional-gpu-inference-vgpu-mps-right-sizing/
source_domain: www.spheron.network
fetched_at: '2026-10-23T17:24:27.216390Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Spheron (GPU cloud vendor marketing) post on fractional GPUs: claims most
  inference runs 20-40% SM utilization; MIG profile table for H100; MPS for L40S/RTX
  lacking MIG; vGPU requires hypervisor+licence; cloud-price cost math (7 MIG slices
  on one H100 at 2.01 USD/h vs 7x RTX 4090 3.57 USD/h). Cloud-rental economics, commercially
  motivated, not on-prem evidence.'
---

*Suggested by [[gpu-sharing-on-k8s-mig-vs-mps-vs-time-slicing-luca-berton]] — primary/vendor doc for GPU sharing claims*

A 7B model in FP16 needs 14 GB VRAM. An H100 PCIe has 80 GB. That leaves 66 GB sitting idle if you run one model per GPU. For most inference workloads serving moderate traffic, GPU SM utilization stays below 40%. You are paying for 100% and using a fraction. Fractional GPU technologies fix this by splitting one physical GPU into isolated or shared slices, so multiple models or tenants can share the hardware without each paying for a full GPU. The [GPU cost optimization playbook](https://www.spheron.network/blog/gpu-cost-optimization-playbook/) identifies this idle capacity as one of the most common sources of wasted cloud spend. The [run multiple LLMs on one GPU guide](https://www.spheron.network/blog/run-multiple-llms-one-gpu-mig-time-slicing-guide/) covers MIG and time-slicing setup step by step. This post goes wider: all four fractional GPU methods, when each fits, cost math with live Spheron pricing, and the cases where sharing backfires.
## TL;DR: Fractional GPUs for AI Inference, vGPU, MPS and Right-Sizing
  * **Why:** a 7B FP16 model needs 14GB, leaving 66GB of an 80GB H100 idle, and most inference workloads keep SM utilization under 40% (Apr 2026).
  * **MIG:** on H100 and A100, MIG gives hardware-isolated slices; one H100 holds up to 3 MIG 2g.20gb instances for 7B models (Apr 2026).
  * **MPS:** on L40S and RTX GPUs, which lack MIG, MPS runs concurrent kernels with per-process thread limits (2026).
  * **Savings:** MIG or MPS cut per-model cost 40-70% for 7B-13B models at moderate traffic in Apr 2026 worked examples.
  * **Skip it** for 70B+ FP16 models that need the full VRAM. Spheron H100 runs $2.65/hr on-demand as of 23 Oct 2026. [Compare live GPU rates](https://www.spheron.network/pricing/).


## The GPU Utilization Problem
Training keeps GPU SMs busy above 90% because the forward pass, backward pass, and optimizer step fill every cycle. Inference is different. Requests arrive in bursts. Between bursts, the GPU waits. Even during a burst, the decode phase is memory-bandwidth bound, not compute bound, so SM occupancy is lower than the raw TFLOPS suggest.
The result: most inference deployments run at 20-40% average SM utilization. A 7B INT4 model on an H100 PCIe handles 2,000-4,000 tokens per second at peak, but at typical API traffic levels it spends most of its time waiting for the next request. For a full operator playbook connecting goodput metrics to packing strategies, see the [GPU goodput engineering guide](https://www.spheron.network/blog/gpu-goodput-engineering-enterprise-ai-cluster-utilization/).
The VRAM story is similar:  
| Model  | Precision  | VRAM needed  | H100 PCIe (80GB) headroom  | A100 80GB headroom  | L40S (48GB) headroom  |  
| --- | --- | --- | --- | --- | --- |  
| 7B  | FP16  | ~14 GB  | 66 GB  | 66 GB  | 34 GB  |  
| 7B  | INT4  | ~4 GB  | 76 GB  | 76 GB  | 44 GB  |  
| 13B  | FP16  | ~26 GB  | 54 GB  | 54 GB  | 22 GB  |  
| 13B  | INT4  | ~7 GB  | 73 GB  | 73 GB  | 41 GB  |  
| 70B  | FP16  | ~140 GB  | Does not fit  | Does not fit  | Does not fit  |  
| 70B  | INT4  | ~35 GB  | 45 GB  | 45 GB  | 13 GB  |  
Running a single 7B FP16 model on an H100 leaves 83% of VRAM unused. Fractional GPU methods let you fill that space with additional model instances. For guidance on which GPU fits which model size and workload, see [best GPU for AI inference 2026](https://www.spheron.network/blog/best-gpu-for-ai-inference-2026/).
## Fractional GPU Technologies: Four Options  
| Technology  | Isolation  | VRAM allocation  | GPU compatibility  | Use case  |  
| --- | --- | --- | --- | --- |  
| MIG  | Hardware  | Fixed slices (10/20/40 GB on H100)  | A100, A30, H100, H200, B200  | Multi-tenant, hard isolation  |  
| MPS  | Process-level  | Shared pool  | Volta+ (all modern data center GPUs)  | Concurrent inference, controlled env  |  
| NVIDIA vGPU  | Hypervisor  | Configurable profiles  | Requires vGPU-licensed GPU and hypervisor  | Enterprise VM deployments  |  
| Time-slicing  | None  | Shared pool  | All NVIDIA GPUs  | Dev/test, low-traffic endpoints  |  
### MIG (Multi-Instance GPU)
MIG partitions a GPU at the hardware level into up to 7 isolated instances, each with its own VRAM, cache, and compute engines. A crash or memory error in one instance does not affect the others. Each instance appears as a separate CUDA device.
**H100 80GB MIG profiles (run`nvidia-smi mig -lgip` to list valid combinations for your driver version):**  
| Profile  | VRAM per instance  | Max instances  | Best for  |  
| --- | --- | --- | --- |  
| 1g.10gb  | 10 GB  | 7  | 7B INT4, 3B FP16  |  
| 2g.20gb  | 20 GB  | 3  | 7B FP16, 13B INT4  |  
| 3g.40gb  | 40 GB  | 2  | 13B FP16, 30B INT4  |  
| 4g.40gb  | 40 GB  | 1  | 30B INT4 with more compute  |  
| 7g.80gb  | 80 GB  | 1  | Full GPU (no sharing)  |  
**A100 80GB** also supports up to 7 instances with the 1g.10gb profile, identical to the H100 80GB (7 × 10 GB = 70 GB, within the 80 GB physical capacity). The A100 40GB uses the smaller 1g.5gb profile to reach 7 instances (5 GB VRAM per slice).
The right-sizing angle: a 7B INT4 model fits in a 1g.10gb slice (needs 4 GB VRAM, well within 10 GB). That gives you 7 independent inference endpoints for the price of one H100. At the rates recorded on 05 Apr 2026, 7 separate RTX 4090 instances at $0.51/hr each came to $3.57/hr total, versus one H100 at $2.01/hr with 7 MIG slices (effective $0.29/hr per slice).
Important: the numeric profile IDs used in `nvidia-smi mig -cgi` (e.g., `14` for `2g.20gb` on H100 SXM5) differ between GPU models and driver versions. Always run `nvidia-smi mig -lgip` first and use the profile ID from your output rather than copying IDs from documentation.
### MPS (Multi-Process Service)
MPS is a CUDA daemon that replaces the default GPU time-multiplexing with a single shared GPU context. Multiple client processes submit work through the MPS server, which schedules their kernels to run concurrently. The result: lower launch overhead and actual parallel kernel execution across processes, unlike time-slicing which interleaves them sequentially.
**Starting the MPS daemon:**
bash

```
export CUDA_MPS_PIPE_DIRECTORY=/tmp/nvidia-mps
export CUDA_MPS_LOG_DIRECTORY=/tmp/nvidia-log
nvidia-cuda-mps-control -d
```

**Capping SM allocation per process with`CUDA_MPS_ACTIVE_THREAD_PERCENTAGE` :**
bash

```
# Process 1 gets 50% of SMs
CUDA_MPS_ACTIVE_THREAD_PERCENTAGE=50 CUDA_VISIBLE_DEVICES=0 \
  python -m vllm.entrypoints.openai.api_server \
  --model meta-llama/Llama-3.1-8B-Instruct \
  --port 8000 --gpu-memory-utilization 0.45 &

# Process 2 gets 50% of SMs
CUDA_MPS_ACTIVE_THREAD_PERCENTAGE=50 CUDA_VISIBLE_DEVICES=0 \
  python -m vllm.entrypoints.openai.api_server \
  --model Qwen/Qwen2.5-7B-Instruct \
  --port 8001 --gpu-memory-utilization 0.45 &
```

One important caveat: `CUDA_MPS_ACTIVE_THREAD_PERCENTAGE` sets a hard cap on the maximum SM percentage a client process can use. This is different from MIG, which provides hard VRAM and compute isolation. Do not use MPS as a substitute for MIG when strict tenant isolation is required.
**MPS versus MIG for L40S and RTX GPUs:** The L40S does not support MIG. Neither does the RTX 4090 or RTX 5090. MPS is the only hardware-concurrent option for these GPUs, making it the primary right-sizing tool for non-A100/H100 inference deployments.
On Volta+ GPUs, MPS also provides address space isolation between client processes, reducing the blast radius of errant memory accesses compared to pure time-slicing.
### NVIDIA vGPU
NVIDIA vGPU virtualizes the GPU at the hypervisor layer. A licensed NVIDIA vGPU host driver runs on VMware vSphere, Proxmox, or KVM, and presents virtual GPU devices to guest VMs. Each VM gets a fraction of the physical GPU's VRAM and compute.
**Key vGPU profile types:**  
| Profile series  | Purpose  |  
| --- | --- |  
| C-series (e.g., A100-4C, A100-40C)  | Compute and inference workloads  |  
| Q-series  | Workstation / professional visualization  |  
| A-series  | Virtual applications (vApps)  |  
| B-series  | Virtual desktops (VDI)  |  
On the A100 40GB, the A100-4C profile gives a VM 4 GB of VRAM. On the A100 80GB, the equivalent A100D-4C profile provides 4 GB per VM. The A100-40C profile gives 40 GB. Profile selection determines the fraction.
**What this means for cloud buyers:** Fractional GPU VMs on hyperscalers (e.g., Google Cloud's fractional G4 instances) use NVIDIA vGPU under the hood. The "fractional" price reflects the GPU slice plus the NVIDIA vGPU software license plus the hypervisor management overhead, all bundled into the VM rate.
**Bare-metal trade-off:** Running on Spheron bare-metal gives you full physical GPU access with no hypervisor overhead and no vGPU licensing cost. You use MIG or MPS to share the GPU instead. For the typical inference use case, bare metal with MIG offers equivalent isolation to vGPU without the licensing markup. The same per-GPU licensing question shows up again one layer higher in the stack, on the serving runtime rather than the hypervisor, and our [NVIDIA NIM pricing vs self-hosted vLLM](https://www.spheron.network/blog/nvidia-nim-pricing-vs-self-hosted-vllm-cost-2026/) breakdown runs the break-even on that one.
**If you want to use vGPU on Spheron bare-metal:** You would need to install a hypervisor (Proxmox or KVM) on the instance and obtain an NVIDIA vGPU software license. This is an enterprise workflow suited for teams with existing VMware or Proxmox infrastructure, not for typical inference deployments.
### Time-Slicing
Time-slicing uses the GPU's built-in hardware scheduling to rapidly switch between processes. No VRAM isolation, no compute isolation. If one process allocates more VRAM than expected, it can OOM-kill the others.
The compatibility advantage is the main reason to use it: time-slicing works on every NVIDIA GPU, including consumer cards, without any special mode or daemon. It is the simplest way to run multiple processes on a single GPU in a dev or testing environment.
When time-slicing fits:
  * Development notebooks and experimentation
  * Internal tools with low, sporadic traffic
  * Pre-Volta GPUs that lack MPS address space isolation
  * Situations where operational simplicity matters more than performance isolation


## Decision Matrix: Which Method for Your Workload  
| Scenario  | Model size  | GPU  | Recommended method  | Why  |  
| --- | --- | --- | --- | --- |  
| Single small model, low traffic  | 7B INT4  | H100/A100  | MIG (1g.10gb)  | Dedicated VRAM slice, up to 7x density  |  
| Multiple small models, isolated  | 7B-13B FP16  | H100/A100  | MIG (2g.20gb or 3g.40gb)  | Hardware isolation, no cross-process interference  |  
| Multiple models, L40S or RTX GPU  | 7B-13B INT4  | L40S, RTX 4090  | MPS with thread percentage limits  | Concurrent kernels, no MIG support on these GPUs  |  
| Multi-tenant inference as a service  | Any  | H100/A100  | MIG with per-tenant containers  | Strongest isolation guarantee  |  
| Development, testing, experiments  | Any  | Any  | Time-slicing  | Simple setup, acceptable for non-production  |  
| Enterprise VM deployment  | Any  | vGPU-licensed GPU  | NVIDIA vGPU  | Fits existing hypervisor workflow  |  
| Large model, full throughput needed  | 70B+ FP16  | H100, A100  | Full GPU (no fractioning)  | VRAM requirement leaves no room to share  |  
## Setting Up Fractional GPU Inference with vLLM
### MIG with vLLM
bash

```
# List available MIG profiles - use the profile ID from YOUR output, not hardcoded values
sudo nvidia-smi mig -lgip

# Create a 2g.20gb MIG instance on H100 (profile ID 14 on H100 SXM5 - verify with lgip)
sudo nvidia-smi mig -cgi 14 -C

# List MIG instances and their UUIDs
nvidia-smi -L | grep MIG

# Launch vLLM targeting the MIG device by UUID
docker run --gpus '"device=MIG-GPU-xxxx-yyy-zzz"' vllm/vllm-openai:latest \
  --model meta-llama/Llama-3.1-8B-Instruct \
  --gpu-memory-utilization 0.90
```

One MIG instance per container. Each container gets its own slice with dedicated VRAM and compute. For more detail on MIG configuration and multi-model deployment patterns, see the [run multiple LLMs on one GPU guide](https://www.spheron.network/blog/run-multiple-llms-one-gpu-mig-time-slicing-guide/).
### MPS with vLLM
bash

```
# Start MPS daemon
nvidia-cuda-mps-control -d

# Verify daemon is running
echo get_default_active_thread_percentage | nvidia-cuda-mps-control

# Process 1: first 7B model, capped at 50% SMs, 45% VRAM
CUDA_MPS_ACTIVE_THREAD_PERCENTAGE=50 CUDA_VISIBLE_DEVICES=0 \
  python -m vllm.entrypoints.openai.api_server \
  --model meta-llama/Llama-3.1-8B-Instruct \
  --port 8000 --gpu-memory-utilization 0.45 &

# Process 2: second 7B model, capped at 50% SMs, 45% VRAM
CUDA_MPS_ACTIVE_THREAD_PERCENTAGE=50 CUDA_VISIBLE_DEVICES=0 \
  python -m vllm.entrypoints.openai.api_server \
  --model Qwen/Qwen2.5-7B-Instruct \
  --port 8001 --gpu-memory-utilization 0.45 &
```

Set `--gpu-memory-utilization 0.45` per process (not 0.5) when running two instances. The flag applies to total physical VRAM, not a per-process quota. If both processes set 0.5, KV cache growth in either process can push total usage above 100% and cause an out-of-memory error. The 10% buffer (2 x 0.45 = 0.90) gives enough headroom for uneven KV cache allocation.
### Triton Inference Server with MPS
Triton can launch multiple model instances on the same GPU automatically. In your model config:
json

```
{
  "instance_group": [{ "kind": "KIND_GPU", "count": 2, "gpus": [0] }]
}
```

Two model instances on GPU 0. When MPS is running, Triton's two instances execute kernels concurrently via the MPS server rather than time-slicing.
## Throughput per Dollar: Fractional vs Full GPU
**Scenario: Serving a 7B model (Llama 3.1 8B, INT4 quantized, batch=1, 512 context).** A worked example at the Spheron rates recorded on 05 Apr 2026.  
| Configuration  | GPU cost (05 Apr 2026)  | Approx. throughput  | Effective cost per 1M tokens  |  
| --- | --- | --- | --- |  
| 1x H100 PCIe (full, 1 model)  | $2.01/hr  | ~4,000 tok/s  | ~$0.14  |  
| H100 PCIe, 3x MIG 2g.20gb slices  | $2.01/hr total / $0.67/hr per slice  | ~1,100 tok/s per slice  | ~$0.17 per slice  |  
| 1x L40S PCIe (full, 1 model)  | $2.06/hr  | ~2,200 tok/s  | ~$0.26  |  
| L40S PCIe, 2x processes via MPS  | $2.06/hr total / $1.03/hr per process  | ~1,400 tok/s per process  | ~$0.20 per process  |  
| 1x RTX 4090 (full, 1 model)  | $0.51/hr  | ~1,500 tok/s  | ~$0.09  |  
Throughput numbers above are representative estimates for INT4 quantized 7B models at batch=1 with vLLM 0.6.x. Actual numbers vary with context length, batch size, quantization implementation, and hardware memory bandwidth. Run your own benchmark using your production model and traffic pattern before making capacity decisions.
> The figures above are a worked example at Spheron rates recorded on **05 Apr 2026**. Spheron's live rates today: H100 $2.65/hr, L40S $0.96/hr, RTX 4090 $0.48/hr, as of **23 Oct 2026**. Check [current GPU pricing →](https://www.spheron.network/pricing/) for live rates.
The cost story at those rates: a 3-slice MIG configuration on a single H100 served 3 concurrent 7B models at $0.67/hr effective cost per model, versus renting 3 separate RTX 4090 instances at $0.51/hr each ($1.53/hr total). The 2g.20gb profile maxes out at 3 instances per H100 (3 × 2 GPC slices = 6 of 7, with 1 unused), so 3 is the ceiling for this profile. MIG wins on isolation and VRAM headroom per slice; RTX 4090 is cheaper if you only need the compute.
## Multi-Tenant Inference: Serving Multiple Models on Shared Infrastructure
For teams running inference as a service with per-tenant isolation, the pattern is:
  * One MIG instance per tenant (H100/A100) or one MPS process per tenant (L40S/RTX)
  * Each tenant gets their own vLLM or llama.cpp server on a distinct port
  * A routing layer (nginx upstream block or Traefik with path-based routing) directs requests to the correct backend


Health checks per instance:
bash

```
# Check each vLLM endpoint independently
curl -s http://localhost:8000/health
curl -s http://localhost:8001/health
```

For serving multiple tasks from a single model rather than running separate models per tenant, LoRA adapters are a complementary approach. See [LoRA multi-adapter serving on GPU cloud](https://www.spheron.network/blog/lora-multi-adapter-serving-gpu-cloud/) for that pattern. Fractional sharing and GPU mixing solve related but different problems: MIG packs more models onto one GPU, while [heterogeneous GPU inference](https://www.spheron.network/blog/heterogeneous-gpu-inference-cost-optimization/) routes prefill and decode to different GPU types entirely. Large deployments often combine both.
## Cost Analysis: Fractional vs Full GPU vs Spot  
| Strategy  | Best for  | Cost vs baseline  |  
| --- | --- | --- |  
| Full GPU on-demand  | Large models (70B+ FP16), high traffic, full VRAM needed  | Baseline  |  
| Fractional GPU (MIG/MPS)  | 7B-13B models, moderate traffic, multi-tenant  | 40-70% lower per-model  |  
| Spot GPU (A100 available on Spheron)  | Fault-tolerant batch inference, checkpoint-enabled  | Varies; compare live rates  |  
| Fractional + spot combined  | Batch inference with small models  | Largest savings  |  
A100 80GB instances run $1.48/hr on-demand and $1.19/hr on spot, as of 23 Oct 2026. Combining spot with MIG partitioning on an A100 gives you multiple model instances on an interruptible GPU, suitable for batch inference jobs that checkpoint their state. If cost-per-token is the metric your team reports on rather than cost-per-GPU-hour, our [AI inference cost economics guide](https://www.spheron.network/blog/ai-inference-cost-economics-2026/) shows how right-sizing fits into a broader per-token FinOps model.
See [serverless GPU vs on-demand vs reserved](https://www.spheron.network/blog/serverless-gpu-vs-on-demand-vs-reserved/) for a full breakdown of GPU billing models and when each reduces total cost. Once right-sizing gets your baseline load stable, moving that floor onto [Spheron's reserved commitments](https://www.spheron.network/features/reserved-commitments/) is usually the next lever, since a predictable MIG-partitioned baseline is exactly the pattern reserved pricing rewards.
## When NOT to Use Fractional GPUs
Fractional GPU sharing is the right call for a specific subset of workloads. Here are the cases where it backfires:
  1. **Model does not fit in the slice.** A 70B FP16 model needs 140 GB VRAM. No MIG profile on a single H100 80GB comes close. Multi-GPU tensor parallelism is the answer, not fractioning.


  1. **SM utilization is already above 80%.** If a single inference process is using 80%+ of GPU compute under normal load, sharing SMs with another process adds latency without reducing cost. Profile first with `nvidia-smi dmon`.


  1. **Speculative decoding pipelines.** Draft model and target model run in lockstep. Capping SM allocation with MPS slows the draft model and can erase the latency benefit of speculative decoding entirely.


  1. **Tensor parallelism across GPUs.** TP workloads use NCCL all-reduce across multiple physical GPUs. MIG/MPS on a single GPU does not help multi-GPU tensor parallel inference, and MPS with multiple GPUs does not improve inter-GPU bandwidth.


  1. **Long-context workloads with large batch sizes.** 128K+ context windows with large batches fill VRAM with KV cache. The memory overhead leaves no room to share the GPU with another process. See [NVMe KV cache offloading for LLM inference](https://www.spheron.network/blog/nvme-kv-cache-offloading-llm-inference/) for strategies to reduce KV cache memory pressure.


## Spheron for Fractional GPU Inference
Spheron provides bare-metal H100, A100, and L40S instances with full root access. This is required for MIG configuration (`nvidia-smi mig -mig 1` needs root) and MPS daemon setup. Managed cloud VMs from hyperscalers often restrict MIG mode at the hypervisor level, meaning you cannot enable or reconfigure MIG partitions on AWS, GCP, or Azure GPU instances without special arrangements.
Pricing is per GPU per hour with per-minute billing. On hyperscalers, fractional GPU VMs bundle vGPU licensing fees and hypervisor management overhead into an opaque per-vCPU or per-GB rate. On Spheron bare metal, you pay the full GPU rate and use MIG or MPS to extract multiple model slots from the hardware. The math typically favors bare metal plus fractioning over paying for pre-fractioned VMs once you account for licensing markup.
Right-sizing workflow: start with a full GPU benchmark on Spheron using per-minute billing (you pay only for what you run). Measure actual SM and VRAM utilization with `nvidia-smi dmon`. If SM utilization is below 50% and VRAM is below 50% at your target traffic, switch to a MIG or MPS configuration and re-benchmark. If latency SLAs still hold, your cost per model drops by the sharing factor.
H100, A100, and L40S are all available on Spheron. For spec comparisons and current pricing, see [H100 rental](https://www.spheron.network/gpu-rental/h100/), [A100 rental](https://www.spheron.network/gpu-rental/a100/), and [L40S rental](https://www.spheron.network/gpu-rental/l40s/).
> Most inference workloads need 20-40 GB of VRAM and 30-50% of a GPU's compute, not a full H100. Spheron bare-metal instances give you root access to configure MIG and MPS, per-minute billing so you only pay while running, and transparent pricing with no fractional GPU licensing markup.
> [Browse H100 instances →](https://www.spheron.network/gpu-rental/h100/) | [Rent an A100 →](https://www.spheron.network/gpu-rental/a100/) | [View all pricing →](https://www.spheron.network/pricing/)
> **[Get started on Spheron →](https://app.spheron.ai)**
STEPS / 05
## Quick Setup Guide
  1. 01
### Measure current GPU utilization
SSH into your instance and run nvidia-smi dmon -s u -d 5 to sample GPU SM utilization every 5 seconds during a representative traffic window. Also run nvidia-smi --query-gpu=memory.used,memory.total --format=csv,noheader,nounits to check VRAM usage. If SM utilization stays below 40% and VRAM usage is below 50% of total, the GPU is a strong candidate for fractional sharing.
  2. 02
### Choose a fractional GPU method based on GPU model and workload
If your GPU is an A100, H100, H200, or B200 and you need hard isolation between tenants or models, use MIG. Run nvidia-smi mig -lgip to list available profiles, then create instances with sudo nvidia-smi mig -cgi PROFILE_ID -C. If your GPU is an L40S, RTX 4090, or another non-MIG GPU and you need concurrent kernel execution, use MPS. For development or low-traffic endpoints on any GPU, time-slicing is the simplest option.
  3. 03
### Configure MPS for concurrent multi-process inference
Start the MPS daemon: export CUDA_MPS_PIPE_DIRECTORY=/tmp/nvidia-mps && export CUDA_MPS_LOG_DIRECTORY=/tmp/nvidia-log && nvidia-cuda-mps-control -d. Set CUDA_MPS_ACTIVE_THREAD_PERCENTAGE on each client process to cap SM allocation (e.g., 50 for two equal processes). This is a hard cap on the maximum SM percentage a client process can use.
  4. 04
### Set vLLM memory limits for fractional GPU allocation
When running two vLLM instances via MPS on the same GPU, set --gpu-memory-utilization 0.45 on each process (not 0.5). This leaves 10% VRAM headroom to prevent out-of-memory errors from KV cache growth. The utilization fraction applies to the full physical VRAM, not a per-process allocation, so both processes compete for the same pool.
  5. 05
### Validate throughput and cost per token on a fractional configuration
After deployment, run a load test with a tool like locust or a simple ab script against each inference endpoint. Record tokens per second and p99 latency. Divide the GPU hourly cost by the number of fractional instances to get effective cost per instance. Calculate cost per million tokens as (hourly cost / instances) / (throughput * 3600 / 1,000,000). Compare against your full-GPU baseline to confirm the fractional configuration meets your latency SLA at lower cost.


FAQ / 05
## Frequently Asked Questions
### 01What is the difference between vGPU, MIG, MPS, and time-slicing?
MIG (Multi-Instance GPU) uses hardware partitioning to create isolated slices with dedicated VRAM and compute engines. It requires an A100, H100, H200, or B200 GPU. MPS (Multi-Process Service) is a CUDA daemon that shares a single GPU context across multiple processes, enabling concurrent kernel execution on any Volta+ GPU. NVIDIA vGPU virtualizes the GPU at the hypervisor layer (VMware vSphere, KVM, Proxmox), requiring an NVIDIA vGPU software license on the host. Time-slicing is software-level scheduling where the GPU rapidly switches between processes with no memory isolation. MIG offers the strongest isolation; MPS offers hardware-concurrent sharing on a wider range of GPUs; vGPU fits enterprise VM workflows; time-slicing works on any GPU but provides no isolation.
### 02Can I use fractional GPUs with vLLM?
Yes. For MIG, launch vLLM with --gpus device=MIG-GPU-UUID pointing to a specific MIG instance. For MPS, start the MPS daemon first, then launch multiple vLLM processes on the same CUDA_VISIBLE_DEVICES index, setting --gpu-memory-utilization to cap each process (e.g., 0.45 per process for two concurrent instances). For time-slicing, vLLM works as-is, but multiple concurrent vLLM processes will context-switch rather than run in parallel.
### 03When should I NOT use fractional GPUs?
Avoid fractional GPU configurations when: (1) the model does not fit in the available fractional slice (e.g., a 70B FP16 model needs 140 GB and cannot be MIG-partitioned on a single H100 80GB); (2) SM utilization is already above 80% on a single process, meaning sharing adds latency without reducing cost; (3) you are running speculative decoding, where capping SM allocation slows the draft model; (4) you need tensor parallelism across GPUs, which requires dedicated NCCL bandwidth; or (5) you have 128K+ context lengths with large batch sizes that fill VRAM with KV cache, leaving no room to share.
### 04What GPU models support MIG partitioning?
MIG is supported on A100 (all variants), A30, H100 (all variants), H200, and B200 GPUs. The A100 80GB and H100 SXM5 80GB both support up to 7 MIG instances using the smallest 1g.10gb profile. Consumer GPUs (RTX 4090, RTX 5090), the L40S, and older data center GPUs like the A10 and V100 do not support MIG. On non-MIG GPUs, MPS is the next best option for concurrent inference.
### 05How much can fractional GPU inference save compared to a full GPU?
At the Spheron rates recorded on 05 Apr 2026, running 3 MIG 2g.20gb instances for 7B INT4 models on a single H100 PCIe at $2.01/hr gave a per-model effective cost of about $0.67/hr, compared to renting 3 separate RTX 4090 instances at $0.51/hr each ($1.53/hr total). With MPS on an L40S at $2.06/hr, running 2 concurrent 7B INT4 models split the cost to $1.03/hr per model, with hardware-concurrent execution and no vGPU licensing markup. Actual savings depend on your traffic pattern and throughput requirements.
## Related Reading
[More on Cost Optimization →](https://www.spheron.network/blog/topics/cost-optimization/)
[ Engineering Token Factory on GPU Cloud: Maximize Tokens per Watt for AI Inference Revenue (2026 Guide) Apr 13, 2026 ](https://www.spheron.network/blog/token-factory-gpu-cloud-tokens-per-watt-guide/)[ Research GPU Cluster Profit Margin Calculator by Utilization (2026) Oct 17, 2026 ](https://www.spheron.network/blog/gpu-cluster-profit-margin-calculator-2026/)[ Research Funding a GPU Buildout: Financing & Offtake Deals (2026) Oct 9, 2026 ](https://www.spheron.network/blog/gpu-financing-and-offtake-agreements-for-smaller-operators-2/)[ Engineering LLM API Cost Comparison: The Real Cost Per Token Formula Sep 17, 2026 ](https://www.spheron.network/blog/llm-api-cost-comparison-cost-per-token-formula/)[ Engineering LoRA Inference Cost Optimization: Shared vs Dedicated GPUs Sep 14, 2026 ](https://www.spheron.network/blog/multi-lora-serving-cost-teardown-when-shared-adapters-beat-d/)[ Tutorial Deploy Gemma 4 QAT on GPU Cloud: 31B Dense at ~66% Less VRAM Jun 15, 2026 ](https://www.spheron.network/blog/deploy-gemma-4-qat-gpu-cloud/)
// Try It Yourself
### Try It on Real GPUs
The GPUs behind these guides are the ones you can rent here: H100s, H200s, B200s, and more, billed per minute after a 20-minute minimum runtime, with no contracts. Pick one and you are live in under two minutes.
Rent a GPU
View Pricing
[Or talk to our team →](https://www.spheron.network/contact/)
We use cookies to measure traffic and improve Spheron. Accept all, reject non-essential, or choose what's on. See our [Privacy Policy](https://www.spheron.network/privacy/).
ManageRejectAccept
