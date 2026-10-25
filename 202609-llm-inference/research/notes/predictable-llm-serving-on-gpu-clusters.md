---
title: Predictable LLM Serving on GPU Clusters
id: predictable-llm-serving-on-gpu-clusters
tags:
- onprem-llm-inference-stacks-859099
- cc-gap-fill
created: '2026-10-23T18:13:28.020606Z'
updated: '2026-10-23T18:19:15.890114Z'
source: https://arxiv.org/abs/2508.20274
source_domain: arxiv.org
fetched_at: '2026-10-23T18:13:28.020326Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: institutional
content_type: paper
deprecated: false
summary: 'arXiv 2508.20274 (Darzi, Bharadwaj, Balija, Aug 2025; v1 preprint, not peer-reviewed):
  host-level controller combining dynamic MIG reconfiguration, PCIe-aware placement
  and MPS quotas/cgroup I/O throttles on A100s (2x AWS p4d.24xlarge, 16 A100-80GB,
  Slurm 23.02, 7 runs, 95% CI). Against static MIG + naive placement: SLO miss rate
  down ~32%, p99 down ~15%, <=5% throughput cost. LLM case (Table 2): vLLM on OLMo
  2 7B Instruct under synthetic PCIe-heavy ETL and compute-heavy training neighbours,
  TTFT p99 232 ms (static MIG, over the 200 ms SLO) -> 199 ms (full system), normalized
  throughput 0.96. This is the closest thing in the corpus to a measured vLLM noisy-neighbour
  result under MIG, but it is one 7B model, synthetic interference, cloud testbed,
  compares a controller to static MIG (no unpartitioned, MPS-only or time-slicing
  arm), and its interference model is PCIe-centric. Authors note MIG isolates compute/HBM
  but not the PCIe path.'
raw_file: raw/predictable-llm-serving-on-gpu-clusters.pdf
doi: arXiv:2508.20274
citation_count: 5
venue: arXiv.org
is_retracted: false
---

Predictable LLM Serving on GPU Clusters
Erfan Darzi
Harvard University, MIT
Cambridge, MA, USA
Shreeanant Bharadwaj
Northeastern University
Boston, MA, USA
Sree Bhargavi Balija
University of California, San Diego
La Jolla, CA, USA
ABSTRACT
Latency-sensitive inference on shared A100 clusters often suffers
noisy-neighbor interference on the PCIe fabric, inflating tail latency
and SLO violations. We present a fabric-agnostic, VM-deployable
host-level controller that combines dynamic Multi-Instance GPU
(MIG) reconfiguration, PCIe-aware placement, and lightweight
guardrails (MPS quotas, cgroup I/O). It samples per-tenant tails
and system signals, uses topology hints to avoid PCIe hot spots,
and gates actions with dwell/cool-down to avoid thrash. On a single
host and a 2-node (16-GPU) cluster, SLO miss-rate is reduced by
≈32% (≈1.5×) and p99 latency improves ≈15% with ≤5% through-
put cost versus static MIG and naive placement; ablations show
MIG and placement contribute comparably. We also evaluate LLM
serving with vLLM on OLMo 2 7B Instruct: TTFT p99 improves
≈10–15% at ≤5% cost without changing the controller.
KEYWORDS
GPU multi-tenancy, LLM serving, vLLM, TTFT, cluster scheduling,
A100, SLO compliance, QoS
1
INTRODUCTION
For latency-sensitive GPU inference services, controlling tail la-
tency is paramount. On shared hosts, the "noisy-neighbor" problem
is a primary cause of unpredictable performance, where co-located
tenants—such as batch training jobs, data loaders, or background
I/O-intensive tasks—create resource contention that leads to signif-
icant jitter in inference latency. This jitter frequently causes viola-
tions of Service Level Objectives (SLOs), exhausting error budgets
and degrading service quality. A key source of this interference is
contention for shared Peripheral Component Interconnect Express
(PCIe) bandwidth, which becomes a bottleneck during intensive
data transfers between the host and GPU [4, 7]. Our scope includes
LLM serving with vLLM; we additionally track time-to-first-token
(TTFT) tails. This challenge is particularly acute in multi-tenant
environments with limited visibility into the underlying hardware
topology, including PCIe lane allocation and Non-Uniform Mem-
ory Access (NUMA) domains [2]. In practice, MIG provides hard
isolation for compute and HBM but not for the shared PCIe path, so
co-running PCIe-bound tenants can still throttle one another across
MIG instances [7]. Meanwhile, pure MPS-based sharing improves
utilization but offers weaker isolation for memory and I/O [9].
Consequently, there is a strong need for host-only solutions that
mitigate interference and guarantee performance while operating
within the constraints of a renter’s VM.
We target this setting with a controller that integrates three
simple levers: (i) dynamic MIG to increase or relax isolation as
conditions change, (ii) PCIe-aware placement to avoid topologi-
cally hot paths, and (iii) lightweight guardrails (MPS quotas and
cgroup I/O throttles) to contain bursty neighbors. The controller is
deliberately conservative: it only escalates isolation when p99/p999
tails persist above threshold, enforces dwell/cool-down to avoid
thrashing, and returns resources when the system is stable. Our ap-
proach complements recent work showing the benefits of dynamic
MIG reconfiguration [3, 8] and PCIe-aware scheduling [7], but is
deployable by renters without fabric privileges.
Our contributions are:
(1) A multi-tenancy controller for NVIDIA A100 GPUs that
integrates dynamic MIG and PCIe-aware placement.
(2) First SLO-safe, multi-tenant control demo on a multi-node
(16-GPU) cloud cluster without fabric privileges.
2
METHOD
We implement a controller that minimizes tail latency for GPU
workloads. The controller operates in a continuous loop, sampling
per-tenant tail latency and system-level signals such as PCIe band-
width and I/O activity. If a tenant’s latency exceeds a predefined
SLO threshold, the controller acts from a three-tiered decision space
to mitigate interference. It can (1) dynamically reconfigure MIG
profiles to provide stronger hardware isolation, (2) perform PCIe-
aware placement to move tenants away from hardware hot spots, or
(3) apply lightweight guardrails such as MPS quotas and I/O throt-
tles. To prevent oscillation, the policy enforces dwell times and
cool-down periods between actions. This approach allows for adap-
tive, fine-grained resource management to ensure SLO compliance
under dynamic load.
Signals and sampling. Every Δ seconds (1–5s), the controller
collects: (i) per-tenant p95/p99/p999 latencies, including TTFT for
autoregressive serving, and SLO miss-rate; (ii) NVML metrics (SM
utilization, memory bandwidth); (iii) PCIe counters (bytes/s; retries
if available) and host block I/O; and optionally, (iv) NIC and IRQ
activity. Signals are smoothed with exponential moving averages
and hysteresis to reduce spurious triggers.
Objective and constraints. The primary objective is to minimize
SLO miss-rate and p99/p999 latency for the latency-sensitive tenant
subject to a throughput budget (≤5% degradation). Actions must be
infrequent enough to keep reconfiguration overhead small (≤30s
per change on A100), so we impose a minimum dwell time between
changes and a cool-down period after recovery.
2.1
Signals & Objective
Our controller’s decision-making process is driven by a continuous
stream of performance signals, polled at a configurable interval, Δ
(typically 1–5 seconds), using standard monitoring tools like the
NVIDIA Management Library (NVML). The primary signal is the
per-tenant 99th percentile (p99) latency, which directly measures
adherence to its SLO. To diagnose the root cause of SLO violations,
the controller also ingests a set of secondary signals. These in-
clude Peripheral Component Interconnect Express (PCIe) counters
(bytes/s, retries) to detect bandwidth contention, NVML metrics for
arXiv:2508.20274v1  [cs.DC]  27 Aug 2025


---

Erfan Darzi, Shreeanant Bharadwaj, and Sree Bhargavi Balija
Figure 1: System architecture of the multi-tenancy controller. The monitoring domain detects SLO violations and performs
root-cause analysis; the decision FSM and optimizer select among dynamic MIG reconfiguration, PCIe-aware placement, and
lightweight guardrails; the execution path applies changes via nvidia-smi and runtime controls (MIG UUID selection, process
pinning, cgroups), with a feedback loop to latency tracking.
Streaming Multiprocessor (SM) and memory bandwidth utilization
to identify compute or memory pressure, and host-level I/O statis-
tics to correlate with storage-intensive noisy neighbors. Optionally,
Network Interface Controller (NIC) and Interrupt Request (IRQ)
statistics can be used to identify network or host-level interference.
The controller’s objective is to minimize the SLO miss-rate and
p99 latency for the primary tenant, with a throughput degradation
constraint of 5% or less.
2.2
Decision Space
To counteract the sources of interference identified by its signals,
the controller operates within a well-defined decision space encom-
passing three categories of actions. First, it can perform Dynamic
MIG Reconfiguration [3, 8], changing the size of a tenant’s Multi-
Instance GPU (MIG) profile, such as ‘1g.10gb‘ to ‘2g.20gb‘ on an
A100. This provides stronger hardware isolation for compute and
memory resources. Second, it can perform PCIe-Aware Place-
ment, migrating a tenant between different MIG instances on the
same GPU. This action is guided by topology-aware heuristics that
seek to map tenants to MIG instances that do not share a busy
PCIe root switch or Non-Uniform Memory Access (NUMA) node
with a high-traffic neighbor [7]. Finally, the controller can apply
lightweight, host-level Guardrails. These include adjusting the
Multi-Process Service (MPS) active thread percentage to cap a ten-
ant’s concurrency and applying cgroup I/O throttles using ‘io.max‘
to limit the disk bandwidth of a noisy background job [9].
2.2.1
Topology-aware placement heuristic. We query GPU and PCIe
topology via DCGM/NVML and host tools including ‘lspci‘ and
NUMA maps to form a simple placement score for each candidate
MIG instance. The score penalizes (i) sharing a PCIe root com-
plex with a bandwidth-heavy tenant, (ii) colocating with a NUMA
domain exhibiting high block I/O, and (iii) recent IRQ bursts on
adjacent CPU cores. When upgrading isolation, we first attempt
an intra-GPU move to the least-penalized MIG instance; only if
insufficient do we enlarge the MIG slice. When relaxing isolation,
we select a smaller profile whose placement score remains below a
conservative threshold, preventing regressions.
2.3
Policy
The controller’s policy is a simple, robust state machine designed
to react to performance degradation while avoiding oscillation. The
policy is triggered when a tenant’s p99 latency exceeds a threshold,
𝜏, for 𝑌consecutive observation windows. Upon triggering, the
controller first attempts to diagnose the issue using its secondary
signals. If high PCIe or I/O pressure is detected, it applies a cgroup
I/O throttle for a fixed duration, 𝑍, on the offending background
tenant. If the primary cause appears to be compute or memory
contention, or if throttling does not resolve the issue, the controller
proceeds to upgrade the tenant’s isolation. This involves either
increasing its MIG share (if headroom is available on the GPU)
or migrating it to a less contended MIG instance. As part of this
upgrade, it also pins the tenant’s CPU affinity away from cores
handling high IRQ loads and enforces a stricter MPS quota. Con-
versely, when a tenant’s performance has been stable and well
within its SLO for a sustained period, the controller may attempt to
relax its isolation, moving it to a smaller MIG profile to free up
resources and improve overall GPU utilization. To prevent rapid,
counter-productive changes, the policy enforces a minimum dwell
time between actions and a cool-down period before re-evaluating
a tenant’s status after a move. In cases where no safe placement can
be found for a new tenant without violating the SLOs of existing
tenants, an admission control mechanism will queue or reject the
new workload.


---

Predictable LLM Serving on GPU Clusters
2.4
Implementation Notes
The controller is implemented in Python and deployed as a host-
level service. It uses nvidia-smi mig for reconfiguration and en-
forces placement via process pinning with CUDA_VISIBLE_DEVICES
and cgroups. Root access within the VM is sufficient. We gate
actions behind feature flags, log all decisions with signal snap-
shots for audit, and support rollback to the last-known-good con-
figuration if post-change p99 worsens within a short validation
window. CPU affinity is applied to keep the latency-sensitive ten-
ant away from IRQ-heavy cores; MPS quotas are adjusted via
CUDA_MPS_ACTIVE_THREAD_PERCENTAGE; and I/O throttles use cgroup
io.max with bounded windows (tens of seconds) to reduce collat-
eral damage.
Algorithm 1 Multi-Tenancy Controller
1: Inputs: Latency stream L, tail threshold 𝜏, persistence 𝑌
2: State: Window 𝑊, config 𝐶, cooldown timer 𝑇𝑐𝑑
3: procedure OnObservation(𝑙)
4:
𝑊←(𝑊∪{𝑙})
5:
𝑝99 ←quantile(𝑊, 0.99)
6:
if not at_reconfig_boundary() or is_cooling_down() then
return
7:
end if
8:
if 𝑝99 > 𝜏for 𝑌consecutive windows then
9:
𝐶←UpgradeIsolation(𝐶)
10:
Relaunch(C); 𝑇𝑐𝑑←DWELL_TIME
11:
else if tail_is_stable() and throughput_ok() then
12:
𝐶←RelaxIsolation(𝐶)
13:
Relaunch(C); 𝑇𝑐𝑑←DWELL_TIME
14:
end if
15: end procedure
2.5
Formal Modeling
2.5.1
Modeling PCIe Contention and Latency Interference. We model
the PCIe fabric as a single processor-sharing (PS) server of capacity
𝐵. When a set A(𝑡) of tenants is active, tenant 𝑖receives instanta-
neous bandwidth
𝑏𝑖(𝑡) = min

𝐵𝑤𝑖
Í
𝑗∈A(𝑡) 𝑤𝑗
, 𝑔𝑖

,
where 𝑤𝑖> 0 are optional weights (equal weights recover equal
sharing) and 𝑔𝑖is an optional host-level throttle. This captures both
equal/weighted sharing and explicit rate limits.
The latency 𝐿𝑖(𝑡) for tenant 𝑖’s inference requests includes a
transfer component proportional to the data size 𝑠𝑖divided by
𝑏𝑖(𝑡), plus compute time 𝑐𝑖: 𝐿𝑖(𝑡) = 𝑐𝑖+
𝑠𝑖
𝑏𝑖(𝑡) + 𝜖(𝑡), where 𝜖(𝑡)
captures stochastic noise from queuing and scheduling. For tails,
we avoid positing a parametric form. Instead, for guidance we use
the classical Kingman approximation for the mean queueing delay
in a G/G/1 stage upstream of compute:
E[𝑊𝑞] ≈
𝜌
1 −𝜌
𝑐2𝑎+ 𝑐2𝑠
2
E[𝑆],
𝜌= 𝜆E[𝑆],
where 𝑐𝑎and 𝑐𝑠are the coefficients of variation of inter-arrival and
service times, respectively. We report empirical 𝑝99/𝑝999 in eval-
uation and use the bound qualitatively to explain how saturation
(𝜌→1) inflates tails.
2.5.2
Optimization Objective for SLO Compliance. The controller
optimizes SLO compliance by minimizing the expected SLO miss-
rate 𝑀= Pr(𝐿𝑖> 𝜏) for the primary tenant, subject to throughput
constraints. Formally, define the objective as min E[𝑀] over MIG
configurations 𝑚𝑖∈M and placements 𝑝𝑖∈P (PCIe topology-
aware slots), with guardrail parameters 𝑔𝑖(MPS quota, I/O throttle).
The constraint ensures throughput 𝑇𝑖≥0.95𝑇base, where 𝑇𝑖=
𝜆𝑖(1 −𝑀).
To solve this, we use a greedy heuristic approximating the NP-
hard allocation problem. Consider the isolation upgrade: if 𝑝99,𝑖> 𝜏
persists, upgrade 𝑚𝑖to maximize Δ𝜇𝑖= 𝜇(𝑚′
𝑖) −𝜇(𝑚𝑖), where
𝜇(𝑚) ∝SM cores and memory in profile 𝑚. Finite termination
of isolation upgrades: the action/state space is finite, and each
upgrade strictly increases isolation (more SMs and/or memory),
so the policy performs at most |M| −1 upgrades before reaching
maximal isolation. We do not claim that this guarantees 𝑝99,𝑖≤𝜏;
it only ensures that upgrade sequences terminate in finitely many
steps.
2.5.3
Theorem on Guardrail Stability.
Claim 1 (Stability under PS and bounded throttles). As-
sume (i) the PCIe fabric behaves as a single PS server of capacity 𝐵;
(ii) each tenant 𝑗’s effective PCIe demand is bounded by a throttle
𝑔𝑗when active; (iii) the aggregate offered load satisfies Í
𝑗𝑔𝑗< 𝐵;
and (iv) arrivals to the latency-sensitive tenant 𝑖are stationary with
𝜆𝑖< 𝜇𝑖(𝐵) under these throttles. Then the queue for tenant 𝑖is stable
and its latency distribution has finite moments; in particular, 𝑝99,𝑖
exists and is finite.
Sketch. Under (i)–(iii) the PCIe stage is a PS queue with utiliza-
tion 𝜌= Í
𝑗𝑔𝑗/𝐵< 1. Standard results imply positive recurrence
and finite mean waiting time for each flow. With (iv), the end-to-end
service rate for tenant 𝑖exceeds its arrival rate. Our controller only
increases isolation or tightens throttles when observed tails exceed
𝜏and never violates (iii), so it reaches a stable regime in finitely
many steps. We report 𝑝99/𝑝999 empirically; the model serves as
qualitative guidance rather than an exact tail predictor.
□
3
SINGLE- AND MULTI-NODE EVALUATION
3.1
Setup
Our multi-node evaluation is conducted on a 2-node cluster consist-
ing of two AWS ‘p4d.24xlarge‘ instances, each with eight NVIDIA
A100-80GB GPUs, linked by a 200 Gbps InfiniBand/EFA intercon-
nect. The 16-GPU pool is orchestrated by Slurm 23.02, with MOFED
5.9 and DCGM 3.2 installed on both nodes. All tenants run inside
Docker containers. Experiments were repeated 7 times with fixed
seeds; we report means with 95% confidence intervals. We pin CPU
cores for the SLO-sensitive tenant and isolate background dataload-
ers to separate NUMA domains when possible. Reconfiguration
dwell/cool-down are configured per Table 1.
Workloads: We design a suite of three co-located tenants to simu-
late a realistic multi-tenancy scenario.


---

Erfan Darzi, Shreeanant Bharadwaj, and Sree Bhargavi Balija
Figure 2: PCIe contention model and topology. Under a PS model with capacity 𝐵, co-active tenants share bandwidth; caps 𝑔𝑖
(when applied) prevent pathological oversubscription. Saturation inflates the transfer component of latency and contributes to
heavy tails.
• T1 (Latency-Sensitive Tenant): Our primary tenant is
a latency-sensitive inference workload with a p99 latency
SLO of 15 ms in the non-LLM experiments.
• T2 (Bandwidth-Heavy Tenant): To create sustained PCIe
and memory bandwidth pressure, we use a background
tenant that simulates a data-intensive Extract, Transform,
Load (ETL) process. This tenant continuously reads large
files from NVMe storage into host memory, performs a
data transformation kernel on the GPU, and writes the
results back. This workload is designed to contend for PCIe
bandwidth.
• T3 (Compute-Heavy Tenant): To create SM contention,
we run a background training job using a synthetic model.
This tenant is designed to be compute-bound, maximiz-
ing its SM occupancy and creating a "noisy neighbor" that
interferes with T1’s access to compute resources.
An interference script toggles the activity of T2 and T3 to create dy-
namic periods of contention, allowing us to evaluate the controller’s
responsiveness. The key parameters for our controller are detailed
in Table 1. Unless otherwise noted, the SLO for T1 is 15 ms (p99).
For the LLM case study (Section "LLM Serving Case Study"), we use
TTFT p99 ≤200 ms. Batch size is 1, and input sizes are drawn from
a realistic mixture to induce time-varying PCIe pressure. We collect
NVML/DCGM, host I/O, and controller logs to correlate actions
with outcomes.
3.2
Metrics
Our primary metric is SLO miss-rate; secondary metrics include
p99/p999 latency and throughput. We measure the SLO miss-rate,
defined as the percentage of inference requests that exceed the 15ms
latency target. We also record the full latency distribution, report-
ing the 95th, 99th, and 99.9th percentiles (p95, p99, p999) to provide
a detailed view of tail behavior. For LLM serving, we additionally
report time-to-first-token (TTFT) at p50/p95/p99 when relevant.
Our secondary metrics evaluate the overhead and system impact
of our controller. We measure the throughput of T1 in Requests Per
Second (RPS) to quantify any performance degradation. We also
monitor GPU SM utilization and total PCIe bandwidth to under-
stand resource usage patterns. Finally, we measure the controller’s
own overhead, including the time required for a MIG reconfig-
uration or tenant move, the frequency of these events, and the
controller’s CPU utilization. All reported comparisons use identical
interference schedules across configurations; confidence intervals
are computed over the 7 repeated runs.
3.3
Experiments
Our experimental evaluation is designed to validate the claims with
controlled setups.
3.3.1
Main Experiment (E1). We compare our full controller against
a baseline using static MIG partitions and naive placement. We sub-
ject the system to dynamic interference by toggling the bandwidth-
heavy (T2) and compute-heavy (T3) tenants on and off, measuring
the impact on the SLO-sensitive tenant (T1).
3.3.2
Ablation Study (E2). To understand the contribution of each
component of our controller, we perform an ablation study. We
evaluate four configurations: our full controller, and then with
each of the three main components disabled in turn: dynamic MIG
reconfiguration only (placement and guardrails disabled), PCIe-
aware placement only (dynamic MIG and guardrails disabled), and
guardrails only (dynamic MIG and placement disabled).


---

Predictable LLM Serving on GPU Clusters
3.3.3
Sensitivity Analysis (E3). We analyze the controller’s sensi-
tivity to its key parameters. We vary the SLO miss-rate threshold
(𝜏) and the observation window (𝑌) to understand their impact on
responsiveness and stability. We also test the bounds of the MPS
quota and I/O throttles.
Table 1: Key Controller Parameters.
Parameter
Value
Description
Tail Threshold (𝜏)
15 ms
p99 latency threshold to
trigger policy change.
Persistence (𝑌)
3 windows
Consecutive windows tail
must exceed 𝜏.
Dwell Time
256 obs.
Minimum
observations
before
another
policy
change.
Cool-down
128 obs.
Grace period after return-
ing to performance mode.
MPS Quota
50-100%
Bounds on MPS active
thread percentage.
IO Throttle
100-500 MB/s
Bounds on cgroup IO throt-
tles.
4
RESULTS
Our results demonstrate a layered approach to mitigating tail la-
tency, where each component of our controller contributes to the
overall improvement. As shown in the ablation study in Table 3,
each component provides a consistent improvement over the base-
line. In all experiments we verified that controller actions did not
violate the dwell/cool-down policy, and that post-change validation
windows confirmed improvement before persisting a new configu-
ration.
MIG and placement contribute comparably. Dynamic MIG re-
configuration yields the largest single-component p99 reduction
(20.0 →17.2 ms), while PCIe-aware placement is close (20.0 →17.8
ms). Combined, they are additive, with the full system reaching 16.5
ms. Placement monitors PCIe traffic and NUMA topology to avoid
"hot" hardware paths shared with I/O-heavy background jobs.
Finally, lightweight guardrails in the form of MPS quotas and
cgroup I/O throttling offer a smaller improvement, reducing the
p99 latency to 19.0 ms.
As shown in Figure 3, our controller adaptively responds to
interference, improving SLO compliance and resource efficiency.
The impact on tail latency is quantified in Figure 4, which shows
a 15% reduction in p99 latency under PCIe contention. On the
2-node cluster, the policy shows similar improvements. For LLM
serving with vLLM (OLMo 2 7B Instruct, paged KV cache, default
configuration), TTFT p99 drops by ≈12–15% with a ≤5% throughput
cost, mirroring the non-LLM results. The results in Table 3 and 4
confirm these improvements with a minimal throughput cost.
4.0.1
LLM Serving Case Study (TTFT). We evaluate LLM serving
with vLLM on OLMo 2 7B Instruct with streaming output. The
SLO is TTFT p99 ≤200 ms at fixed QPS. Under the same T2/T3
interference used elsewhere, our controller reduces TTFT p99 by
≈13% at a ≤4% throughput cost, without any controller changes.
Results are summarized in Table 2.
Table 2: LLM serving (vLLM, OLMo 2 7B Instruct) under in-
terference: TTFT p99 and normalized throughput.
Configuration
TTFT p99 (ms)
Norm. Throughput
Static MIG
232
1.00
Full System
199
0.96
Table 3: Ablation study results (mean ± 95% CI). Throughput
is normalized to the static MIG configuration.
Configuration
SLO miss-rate
p99 (ms)
Norm. Throughput
Static MIG
16.4% ± 1.5
20.0 ± 1.2
1.00
Guards-only
14.5% ± 1.4
19.0 ± 1.0
0.99 ± 0.02
Placement-only
13.0% ± 1.2
17.8 ± 0.9
0.98 ± 0.02
MIG-only
12.2% ± 1.1
17.2 ± 0.8
0.98 ± 0.02
Full System
11.1% ± 1.0
16.5 ± 0.7
0.97 ± 0.02
Table 4: Controller overheads (mean ± 95% CI).
Metric
Value
MIG reconfig time (s)
18 ± 6
Move frequency (/hr)
< 5
Controller CPU (%)
< 2%
5
DISCUSSION & LIMITATIONS
Our method has several limitations. In some cloud environments,
the PCIe topology is partially opaque, so we must infer contention
from counters. MIG profile changes require a brief pause of the
tenant and may reload model state; we limit frequency and bound
pauses, but bursty workloads might still perceive short disruptions.
For very heavy training tenants, our approach is clearly insufficient
and should be paired with stricter admission control or higher
baseline isolation. Finally, our placement heuristic is intentionally
simple; richer learning-based predictors could improve stability at
the cost of complexity.
Our approach is complementary to other isolation techniques.
For instance, it could operate on top of a kernel-space interception
system like KRYPTON [11], managing the virtualized resources
KRYPTON exposes. Cluster schedulers like Themis and Gandiva [5,
10] could supply better hints or pre-placement guarantees, while
our controller refines mapping at the host level to enforce SLOs.
This highlights our modularity: a controller can provide SLO-aware,
dynamic management within the resource boundaries of a VM,
regardless of how the underlying platform enforces them.


---

Erfan Darzi, Shreeanant Bharadwaj, and Sree Bhargavi Balija
Figure 3: Adaptive controller behavior and efficiency. (a) The controller responds to dynamic interference bursts with progres-
sively stronger actions (Guardrails, Placement, MIG). (b) The efficiency-compliance trade-off, showing the full system (purple
star) outperforming the static MIG and partial configurations by achieving high SLO compliance and resource utilization.
Figure 4: The impact of PCIe contention on the tail of the
latency distribution. High contention (red) creates a signif-
icant heavy tail, causing SLO violations. The full system’s
PCIe-aware placement (blue) mitigates this, reducing p99
latency and moving it closer to the SLO threshold (dashed
line).
Threats to validity. Our small-scale cluster and synthetic inter-
ference patterns may not capture all production behaviors. We
mitigate this by repeating runs, reporting confidence intervals, and
validating that the qualitative ordering of configurations is con-
sistent across runs and workloads. Broader validation on larger
clusters and real traces is future work.
6
RELATED WORK
Our work is situated within a rich body of research on multi-tenant
GPU systems. The foundational paradigms of GPU sharing establish
a trade-off between the hard, predictable isolation of spatial parti-
tioning, exemplified by NVIDIA’s Multi-Instance GPU (MIG), and
the flexibility of temporal multiplexing, facilitated by technologies
like the Multi-Process Service (MPS) [2]. Static partitions reduce
interference but risk fragmentation; temporal sharing improves
utilization but exposes tenants to contention.
Dynamic spatial management has gained traction. MIGRator [8]
formulates dynamic MIG reconfiguration for continuous learning
workloads and shows notable gains; MISO [3] explores exploiting
MIG at cluster scale. Our controller adopts the dynamic reconfigu-
ration principle but couples it with topology-aware placement and
guardrails for SLOs in renter-constrained settings.
System-level interference on shared I/O paths has been stud-
ied from PCIe arbitration [4] to recent demonstrations that MIG
instances still share PCIe bandwidth [7]. We incorporate these
observations directly by avoiding PCIe hot spots and throttling
background I/O when necessary.
For SLO-aware serving, Clipper [1] and subsequent systems im-
proved batching and predictability at the service layer. Transparent
GPU sharing in container clouds [9] and Sponge [6] offer host-level
temporal controls; our approach complements them with spatial
reconfiguration. Finally, cluster schedulers like Themis and Gan-
diva [5, 10] address fairness and efficiency at scale; our focus is
the single-node control loop that refines placement and isolation
within a VM.
REFERENCES
[1] Daniel Crankshaw, Xin Wang, Giulio Zhou, Michael J. Franklin, Joseph E. Gon-
zalez, and Ion Stoica. 2017. Clipper: A Low-Latency Online Prediction Serving
System. In 14th USENIX Symposium on Networked Systems Design and Implemen-
tation (NSDI 17). USENIX Association, Boston, MA, 613–627.
[2] Myeongjae Jeon, Shivaram Venkataraman, Amar Phanishayee, Junjie Qian, Wen-
cong Xiao, and Fan Yang. 2019. Analysis of Large-Scale Multi-Tenant GPU


---

Predictable LLM Serving on GPU Clusters
Clusters for DNN Training Workloads. In Proceedings of the 2019 USENIX An-
nual Technical Conference (USENIX ATC ’19). USENIX Association, 947–960.
https://www.usenix.org/system/files/atc19-jeon.pdf
[3] Baolin Li, Tirthak Patel, Siddharth Samsi, Vijay Gadepally, and Devesh Tiwari.
2022. MISO: Exploiting Multi-Instance GPU Capability on Multi-Tenant GPU
Clusters. In Proceedings of the 13th ACM Symposium on Cloud Computing (SoCC
’22). Association for Computing Machinery, 173–189. https://doi.org/10.1145/
3542929.3563510
[4] Chen Li, Yifan Sun, Lingling Jin, Lingjie Xu, Zheng Cao, Pengfei Fan, David
Kaeli, Sheng Ma, Yang Guo, and Jun Yang. 2019. Priority-Based PCIe Scheduling
for Multi-Tenant Multi-GPU Systems. IEEE Computer Architecture Letters 18, 2
(2019), 157–160. https://doi.org/10.1109/LCA.2019.2955119
[5] Kshiteej Mahajan, Arjun Balasubramanian, Arjun Singhvi, Shivaram Venkatara-
man, Aditya Akella, Amar Phanishayee, and Shuchi Chawla. 2020. Themis:
Fair and Efficient GPU Cluster Scheduling. In Proceedings of the 17th USENIX
Symposium on Networked Systems Design and Implementation (NSDI ’20). USENIX
Association, 289–304.
[6] Kamran Razavi, Saeid Ghafouri, Max M"uhlh"auser, Pooyan Jamshidi, and Lin
Wang. 2024. Sponge: Inference Serving with Dynamic SLOs Using In-Place
Vertical Scaling. In Proceedings of the 4th Workshop on Machine Learning and Sys-
tems (EuroMLSys 2024), co-located with EuroSys 2024. Association for Computing
Machinery, 184–191. https://doi.org/10.1145/3642970.3655833
[7] Yan-Mei Tang, Wei-Fang Sun, Hsu-Tzu Ting, Ming-Hung Chen, I-Hsin Chung,
and Jerry Chou. 2025. PCIe Bandwidth-Aware Scheduling for Multi-Instance
GPUs. In Proceedings of the International Conference on High Performance Comput-
ing in Asia-Pacific Region (HPC Asia ’25). Association for Computing Machinery,
Hsinchu, Taiwan. https://doi.org/10.1145/3712031.3712324
[8] Tianyu Wang, Sheng Li, Bingyao Li, Yue Dai, Ao Li, Geng Yuan, Yufei Ding,
Youtao Zhang, and Xulong Tang. 2024. Improving GPU Multi-Tenancy Through
Dynamic Multi-Instance GPU Reconfiguration. arXiv preprint arXiv:2407.13126
(2024).
[9] Bingyang Wu, Zili Zhang, Zhihao Bai, Xuanzhe Liu, and Xin Jin. 2023. Trans-
parent GPU Sharing in Container Clouds for Deep Learning Workloads. In 20th
USENIX Symposium on Networked Systems Design and Implementation (NSDI 23).
USENIX Association, 69–85.
[10] Wencong Xiao, Romil Bhardwaj, Ramachandran Ramjee, Muthian Sivathanu,
Nipun Kwatra, Zhenhua Han, Pratyush Patel, Xuan Peng, Hanyu Zhao, Quanlu
Zhang, Fan Yang, and Lidong Zhou. 2018. Gandiva: Introspective Cluster Schedul-
ing for Deep Learning. In Proceedings of the 13th USENIX Symposium on Operating
Systems Design and Implementation (OSDI ’18). USENIX Association, 595–610.
[11] Shulai Zhang, Ao Xu, Quan Chen, Han Zhao, Weihao Cui, Zhen Wang, Yan Li,
Limin Xiao, and Minyi Guo. 2025. Efficient Performance-Aware GPU Sharing with
Compatibility and Isolation through Kernel Space Interception. In Proceedings of
the 2025 USENIX Annual Technical Conference (USENIX ATC ’25). USENIX Associ-
ation, 1003–1019. https://www.usenix.org/system/files/atc25-zhang-shulai.pdf
