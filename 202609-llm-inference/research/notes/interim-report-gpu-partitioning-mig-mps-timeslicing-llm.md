---
title: Interim report — gpu-partitioning-mig-mps-timeslicing-llm
id: interim-report-gpu-partitioning-mig-mps-timeslicing-llm
tags:
- onprem-llm-inference-stacks-859099
- locus-gpu-partitioning-mig-mps-timeslicing-llm
created: '2026-10-23T17:56:45.461236Z'
status: draft
type: interim
deprecated: false
summary: 'MIG helps queue-bound small-model tenants (Red Hat: -36% mean JCT but 45->19%
  util at equal concurrency); no LLM noisy-neighbour benchmark exists; MPS unmeasured'
---

# Interim report: gpu-partitioning-mig-mps-timeslicing-llm

**Locus question:** Does MIG, MPS or time-slicing actually improve on-premise LLM-serving efficiency, given that only two measured sources exist, both non-LLM, and no LLM noisy-neighbour benchmark exists?
**Flavor:** dialectical

## What the corpus already said
Eleven corpus sources discuss GPU sharing; only two measured anything: the Red Hat arXiv paper [[evaluating-kubernetes-performance-for-genai]] and NVIDIA's NIM-Operator test [[maximize-ai-infrastructure-throughput-by-consolidating-underutilized-gpu-workloa]]. Mechanism facts live in primary docs [[time-slicing-gpus-in-kubernetes-nvidia-gpu-operator]] and [[github-nvidiak8s-device-plugin-nvidia-device-plugin-for-kubernetes-github]]. Proxy utilization evidence comes from [[muxflow-efficient-and-safe-gpu-sharing-in]] and [[deepshare-assurance-driven-deep-learning-job]]; host-CPU evidence from [[characterizing-cpu-induced-slowdowns-in]]. I re-read the bodies of the key notes and ran scholarly searches (below). I did not spend fetch budget: the verification was entirely vault-resident, and the searches turned up no LLM-specific candidate worth fetching.

## What the new sources say (verified against bodies)

**Red Hat, Feb 2026 (arXiv 2602.04900v3) [[evaluating-kubernetes-performance-for-genai]].** Cloud testbed (one AWS p4d.24xlarge, 8xA100), not on-prem, but the OpenShift/GPU-Operator stack is the on-prem one. Whisper medium and large jobs, kube-burner, 3 runs each. Two experiments, and the distinction matters:
- *Equal concurrency (Table 5; 8 jobs with and without MIG, 24 jobs over 3 runs):* mean JCT 14 -> 17 min, median 10 -> 13, P95 36 -> 40; mean GPU utilization **45% -> 19%**; worker CPU 9% -> 9%. Authors: "moderate but noticeable performance degradation due to resource constraints."
- *Full dataset (Table 6; 96 jobs over 3 runs):* concurrency 8 -> 25 (slice count); mean JCT 28 -> 18, median 28 -> 16, P95 51 -> 36, std 14 -> 9; mean GPU util 44 -> 38%; tensor-core 1 -> 3%; DRAM activity 13 -> 38%; worker CPU **10 -> 37%**. Authors: parallelism "essentially over-weighted the execution time degradation." They attribute the util dip to "slice padding" (e.g. two 3g.20gb profiles on one A100 leave stranded SMs).
- Two corrections to the brief's framing. (1) The 44->38% figure understates the waste; the controlled test shows 45->19%. (2) The CPU rise to 37% occurs only in the higher-concurrency run; at equal concurrency CPU was flat at 9%. So the CPU rise is a consequence of 3x more parallel jobs, not of slicing as such. It still shows slicing shifts the bottleneck to the host. It is corroborating, not independent, evidence for the CPU-starvation point.
- DAS/Kueue integration was unfinished at writing (per the note summary).

**NVIDIA blog [[maximize-ai-infrastructure-throughput-by-consolidating-underutilized-gpu-workloa]].** Voice pipeline (Parakeet 1.1B ASR, Magpie TTS, Llama-3.1-Nemotron-Nano-VL-8B as the LLM stage), A100s. MIG ~1.00 req/s per GPU vs time-slicing ~0.76; TTS mean latency 144.7 ms (time-sliced) vs 168.2 ms (MIG), which the authors call imperceptible beside a multi-second LLM response. Recommendation: "Default to MIG for production scale and stability"; time-slicing for dev/low-concurrency. Text says MPS "lacks hardware fault isolation." Caveats: vendor-authored; no MPS arm measured; the LLM was the pipeline bottleneck and (per the text) the reason req/s was capped, so the sharing comparison is mostly on the support models. This is the only source here touching an LLM stage, and only as a bottleneck, not as the test subject.

**Primary-doc mechanism facts.** GPU Operator docs: time-slicing has "no memory or fault-isolation between replicas"; requesting multiple replicas "does not guarantee that the pod receives access to a proportional amount of GPU compute power"; cluster default plus per-node label override is supported; MIG "provides memory and fault isolation at the hardware layer." Device-plugin README: "Time-slicing and MPS are mutually exclusive"; MPS "does space partitioning and allows memory and compute resources to be explicitly partitioned and enforces these limits per workload"; "the same sharing method is applied to all GPUs on a node. You cannot configure sharing on a per-GPU basis." (Time-slice-on-MIG 7x4=28 was not re-verified in the bodies I searched; cite to NVIDIA docs only if the drafter confirms.)

**Tooling churn.** [[github-openshiftinstaslice-operator-instaslice-operator-facilitates-slicing-of-a]] shows "This repository was archived by the owner on Aug 11, 2026. It is now read-only." Combined with the unfinished DAS-Kueue integration, dynamic MIG is unsettled.

**Host CPU.** [[characterizing-cpu-induced-slowdowns-in]]: "H100 nodes, accounting for 34.3k GPU hours out of 50.9k total, include cases in which users request as few as 1 CPU core across 4 or 8 GPUs"; "ratios below 4-8 cores per GPU risk measurable slowdown for LLM inference workloads"; the paper notes CPU scarcity is rare for whole-node jobs and common in shared-node settings, which is exactly what slicing creates.

**Licensing.** [[nvidia-ai-enterprise-licence-pricing-subscription-eurokommerz]] (derivative; reseller): licence is per physical GPU; "We found no mention of MIG slices in the licensing guide: partitioning a card into four instances does not change the count." This is an inference from silence by a reseller, so treat as unconfirmed pending NVIDIA's licensing guide.

**Search for the missing benchmark.** Scholar searches ("vLLM MIG MPS time-slicing interference benchmark"; "noisy neighbor vLLM MIG partition latency"; "MPS versus MIG LLM inference throughput A100 H100 colocated"; "Metric-Driven Kubernetes Operator Dynamic MPS") returned co-serving/multiplexing systems papers (FlexLLM, DuetServe, Prism, DeltaServe, ElastiCo, TokenFlow, Granularity- and Interference-Aware GPU Sharing with MPS 2024) and an MPS operator paper targeting consumer GPUs without MIG. None is a controlled MIG vs MPS vs time-slicing test of vLLM under noisy-neighbour load on-prem. Audited absence (d) stands. Caveat: I only read abstracts of these hits, so a measurement may sit inside one (the 2024 MPS interference paper and ElastiCo are the likeliest) and is worth a targeted follow-up.

## Evidence synthesis
Both measured sources agree on direction and disagree on nuance. Red Hat shows partitioning wins on queue-dominated batch work (-36% mean JCT, -29% P95) while each job runs slower (+21% mean at equal concurrency) and the GPU is measurably less busy per unit time (19% vs 45% in the controlled test). NVIDIA shows MIG ahead of time-slicing by ~32% on throughput and behind by ~16% on one stage's latency. The two reconcile: MIG buys isolation and predictable aggregate throughput; it does not buy per-request speed, and it strands capacity through profile geometry. Neither compares to simply running more dedicated GPUs, and neither tests MPS.

The case FOR partitioning at scale is proxy evidence only: utilization below 60% across a >20,000-GPU inference cluster, Alibaba PAI tasks mostly requesting <10% of a GPU, MuxFlow 26%->76%, DeepShare 70.58% with 46% less queueing. These are training/mixed or hyperscaler fleets, not on-prem LLM serving. Large LLMs, the actual on-prem target, usually need whole GPUs or tensor-parallel groups for weights + KV cache; MIG helps mainly for small models, embedders, rerankers, ASR/TTS, notebooks and dev. That is inference from memory arithmetic, not a measurement.

Sizing guides in the corpus omit any host CPU term, while the HPC logs show chronic CPU under-allocation and Red Hat shows CPU rising fourfold with density. Slicing multiplies tenants per node and therefore sharpens the omission.

## Committed position
Partitioning is worth it on-premise for small-model, bursty, or queue-bound tenants (embedding, rerank, ASR/TTS, small-LLM dev/test, batch jobs) where the dominant cost is waiting for a whole GPU; it is not demonstrated, and probably not worth the operational cost, for production LLM serving of 7B+ models that fill a GPU with batching. Where you do partition, choose MIG for anything with an SLA (hardware memory and fault isolation; ~32% higher throughput than time-slicing in NVIDIA's own test), reserve time-slicing for dev/CI because it has no memory or fault isolation and gives no proportional share, and treat MPS as unproven (no measured data; no hardware fault isolation per NVIDIA). The load-bearing reason: the only controlled evidence shows partitioning trades per-job speed and GPU efficiency (45%->19% busy at equal concurrency) for queue relief, so it pays only when queueing dominates execution time, and the Red Hat result is an ASR batch result that cannot be assumed to transfer to LLM decode, where no one has published a noisy-neighbour measurement. Operationally, plan host CPU (at least the 4-8 cores per GPU floor the HPC paper identifies, scaled to slice count) as a first-class resource, and avoid betting on InstaSlice/DAS or DRA-based MIG until they stabilise.

- **Position:** Partition for small/bursty/queue-bound tenants using MIG for SLA work; do not partition GPU-filling production LLM serving absent a measurement of your own.
- **Confidence:** medium (about 65%) for the conditional rule; low for any claim about MPS and about LLM decode-phase interference, which is unmeasured.
- **Boundary conditions:** Holds for A100/H100-class GPUs on Kubernetes with the NVIDIA GPU Operator, queue-bound workloads and models fitting a 1g-3g profile. Outside that (large TP models, Blackwell MIG behaviour, non-NVIDIA hardware, Slurm sites) evidence is absent. Rule of thumb from the data: re-profile if utilization stays under ~40% (a consultancy heuristic, not measured).
- **What would change this position:** A controlled vLLM benchmark (same model, MIG vs MPS vs time-slice vs dedicated, with an adversarial neighbour) showing MIG or MPS retaining >=90% of dedicated tail latency would widen the case to LLM serving; one showing MIG decode-latency inflation >25% would narrow it to non-LLM tenants. A Red Hat/vendor rerun on on-prem hardware with CPU held constant would test whether the CPU rise is intrinsic.
- **Evidence weight:** 2 measured sources (both non-LLM, one cloud testbed, one vendor-authored), 2 primary-doc mechanism sources (strong), 4 proxy utilization studies (training/hyperscaler), 1 derivative licensing source (weak), 2 blogs with internal errors (excluded from claims). Tooling-churn claim rests on one repo-archive banner plus the paper's self-reported unfinished integration.

## Open questions
- Does any of the 2024-2026 MPS/multiplexing papers (e.g. Granularity- and Interference-Aware GPU Sharing with MPS; ElastiCo) contain a vLLM interference measurement? Only abstracts were read.
- NVIDIA's own licensing guide on MIG and per-physical-GPU counts; time-slice-on-MIG 28-slot figure from primary docs.
- Whether DRA-based MIG reached production readiness after v0.4.0; DRA claim was not re-verified here.
- No named on-prem deployment publishing a sharing-mode choice for LLM serving.

## Sources
1. [[evaluating-kubernetes-performance-for-genai]] Red Hat arXiv 2602.04900
2. [[maximize-ai-infrastructure-throughput-by-consolidating-underutilized-gpu-workloa]] NVIDIA technical blog
3. [[time-slicing-gpus-in-kubernetes-nvidia-gpu-operator]] NVIDIA docs
4. [[github-nvidiak8s-device-plugin-nvidia-device-plugin-for-kubernetes-github]] device plugin README
5. [[github-openshiftinstaslice-operator-instaslice-operator-facilitates-slicing-of-a]] archived repo
6. [[characterizing-cpu-induced-slowdowns-in]] CPU-induced slowdowns
7. [[muxflow-efficient-and-safe-gpu-sharing-in]], [[deepshare-assurance-driven-deep-learning-job]] proxy evidence
8. [[nvidia-ai-enterprise-licence-pricing-subscription-eurokommerz]] derivative
