---
title: 'MuxFlow: Efficient and Safe GPU Sharing in'
id: muxflow-efficient-and-safe-gpu-sharing-in
tags:
- onprem-llm-inference-stacks-859099
- quota-idling
- gpu-utilization
created: '2026-10-23T17:21:31.526766Z'
updated: '2026-10-23T17:22:55.325351Z'
source: https://arxiv.org/pdf/2303.13803
source_domain: arxiv.org
fetched_at: '2026-10-23T17:21:31.526469Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: institutional
content_type: paper
deprecated: false
summary: 'MuxFlow (ByteDance ''CompanyX'' + Peking U): production GPU space-sharing
  (MPS) for online+offline DL in a cluster of >20,000 GPUs serving tens of thousands
  of workloads. One-week inference-cluster stats: GPU util and SM activity <60% for
  >99% of GPUs; memory <60% for ~90% of GPUs (online workloads get dedicated GPUs).
  Deployment raised GPU util 26%->76%, SM activity 16%->33%, memory 42%->48%. Strong
  measured evidence of dedicated-GPU inference idling at hyperscale-owned fleet.'
raw_file: raw/muxflow-efficient-and-safe-gpu-sharing-in.pdf
doi: arXiv:2303.13803
utility_score: 15.0
citation_count: 22
venue: arXiv.org
is_retracted: false
---

MuxFlow: Efficient and Safe GPU Sharing in
Large-Scale Production Deep Learning Clusters
Yihao Zhao∗
Peking University
Xin Liu∗
ByteDance Inc.
Shufan Liu
ByteDance Inc.
Xiang Li
ByteDance Inc.
Yibo Zhu
ByteDance Inc.
Gang Huang
Peking University
Xuanzhe Liu
Peking University
Xin Jin
Peking University
Abstract
Large-scale GPU clusters are widely-used to speed up both
latency-critical (online) and best-effort (offline) deep learning
(DL) workloads. However, most DL clusters either dedicate
each GPU to one workload or share workloads in time, lead-
ing to very low GPU resource utilization.
We present MuxFlow, the first production cluster sys-
tem that supports efficient and safe space-sharing for DL
workloads. NVIDIA MPS provides an opportunity to share
multiple workloads in space on widely-deployed NVIDIA
GPUs, but it cannot guarantee the performance and safety of
online workloads. MuxFlow introduces a two-level protec-
tion mechanism for memory and computation to guarantee
the performance of online workloads. Based on our practical
error analysis, we design a mixed error-handling mecha-
nism to guarantee the safety of online workloads. MuxFlow
further proposes dynamic streaming multiprocessor (SM)
allocation and matching-based scheduling to improve the
efficiency of offline workloads. MuxFlow has been deployed
at CompanyX’s clusters with more than 20,000 GPUs. The
deployment results indicate that MuxFlow substantially im-
proves the GPU utilization from 26% to 76%, SM activity from
16% to 33%, and GPU memory from 42% to 48%.
1
Introduction
Deep learning (DL) has been widely integrated into intelli-
gent applications and services, such as intelligent recommen-
dation [14, 17], autonomous driving [9, 30], image recogni-
tion [27, 49], and machine translation [18, 53]. Some of them
provide real-time inference and have critical latency demand
(called online workloads). Meanwhile, other workloads do
not have hard latency demand (called offline workloads). En-
terprises usually build large-scale GPU clusters for DL work-
loads and reserve specific GPUs for online workloads.
Existing efforts in online workload management have sig-
nificantly improved the serving efficiency [15, 25, 48]. How-
ever, a major limitation is that most methods dedicate the
whole GPU to a single workload. It is reported that an on-
line workload usually cannot fully utilize the expensive GPU
∗Yihao Zhao and Xin Liu contributed equally.
resource [26, 36] mainly due to two reasons. First, the fre-
quency of online requests fluctuates from time to time. When
the request frequency is low, more GPU computing units are
idle, leading to a great waste of GPU. Second, even if the re-
quest frequency is high, the batch size of online workloads is
usually limited to a small value for latency demand and most
kernels need few computing resources. Thus, the computing
units in GPU are still underutilized.
A common idea is to share GPUs among multiple work-
loads with different latency demands [26, 57, 59], i.e., sharing
GPUs between online and offline workloads. Time-sharing
and space-sharing are two paradigms for GPU sharing. Time-
sharing [2, 59] assigns time slices to different workloads, but
it may degrade the performance of online workloads and
cannot improve GPU resource utilization in space. Space-
sharing [5, 26] is a better way to improve GPU resource uti-
lization. For widely-deployed NVIDIA GPUs, multi-process
service (MPS) [5] is a feasible choice due to its efficacy, flexi-
bility, and compatibility with NVIDIA GPUs.
However, MPS brings new challenges to production clus-
ters. First, the primary goal for production clusters is to
guarantee the performance of online workloads, such as
real-time recommendation and machine translation. These
workloads have hard latency demands because longer la-
tency may influence the user’s experience. However, MPS
cannot guarantee the performance of online workloads. Sec-
ond, MPS has a serious error propagation problem, i.e., when
one workload encounters an error, the shared workload may
also be influenced. It is critical to guarantee the safety of
shared workloads, especially the online workloads in pro-
duction clusters.
This paper presents MuxFlow, a system that supports effi-
cient and safe space-sharing of large-scale GPU clusters for
DL workloads in the production environment. MuxFlow ad-
dresses the above challenges to guarantee the performance
and safety of online workloads. MuxFlow exploits a two-level
protection mechanism to guarantee the performance of on-
line workloads from both the workload level and GPU level.
At the workload level, we propose xCUDA to constrain the
1
arXiv:2303.13803v1  [cs.DC]  24 Mar 2023


---

GPU memory and computing power used by offline work-
loads. xCUDA monitors the GPU memory allocation to limit
the memory usage of offline workloads, and controls kernel
launches to limit the computing power used by offline work-
loads. Besides, xCUDA provides adjustable parameters to
control how much the online workloads are influenced. At
the GPU level, MuxFlow employs the SysMonitor to monitor
the GPU device status. The SysMonitor maintains a state
machine according to multi-dimensional GPU metrics, and
will evict offline workloads if the GPU status can potentially
compromise the performance of online workloads.
To guarantee the safety of online workloads, we inves-
tigate all propagated errors in our production clusters and
propose a mixed error-handling mechanism. We find that
99% propagated errors are caused by SIGINT and SIGTERM
signals, which are usually used to stop containers in Ku-
bernetes [4]. MuxFlow employs a graceful exit mechanism
that intercepts related signals and releases CUDA context
actively to avoid the propagated error. For other corner cases,
MuxFlow resets the CUDA context and restarts the work-
loads.
Furthermore, MuxFlow improves the efficiency of offline
workloads. MuxFlow dynamically allocates the computing
unit of NVIDIA GPU, i.e., streaming multiprocessor (SM),
used by offline workloads. Our key intuition is that we can
set the SM percentage of offline workloads complementary
to the SM percentage used by online workloads, with an
acceptable slowdown of online workloads. Besides, we ob-
serve that different sharing pairs vary dramatically in the
efficiency of offline workloads. To maximize the efficiency,
we formulate the problem as a maximum weighted bipartite
matching problem. MuxFlow exploits a DL approach to build
the bipartite graph and the KM algorithm [33, 38] to solve
this problem.
In summary, we make the following contributions.
• We investigate the characteristics of production inference
clusters and identify the opportunity in space sharing to
better utilize GPU.
• We provide efficient and safe space-sharing with three
mechanisms: a two-level protection mechanism to guaran-
tee the performance of online workloads, a mixed error-
handling mechanism to ensure the safety of online work-
loads, and a dynamic SM percentage mechanism to im-
prove the efficiency of offline workloads.
• We design a matching-based scheduling algorithm to
improve the sharing efficiency at the cluster level. The
scheduling algorithm can improve the overall normalized
throughput of offline workloads while maintaining the
performance of online workloads.
• We introduce MuxFlow, the first production cluster system
that enables efficient and safe space sharing. We have
deployed MuxFlow in a production cluster with more than
20, 000 GPUs at CompanyX to serve tens of thousands of
0
20
40
60
80
100
GPU resource (%)
0
0.2
0.4
0.6
0.8
1
CDF
GPU util.
SM act.
GPU mem.
Figure 1. GPU resource statistic in a production cluster for
online workloads.
daily workloads. Deployment results show that MuxFlow
improves the GPU utilization from 26% to 76%, SM activity
from 16% to 33%, and GPU memory from 42% to 48%.
2
Motivation
In this section, we begin with introducing DL workloads and
critical terminologies. Then we describe the observations
from the production cluster for online workloads to motivate
the design of MuxFlow. We end by discussing opportunities
to share the GPUs between different DL workloads.
2.1
DL workloads
DL workloads use the deep neural network (DNN) to per-
form inference or training. DL workloads are usually clas-
sified into two categories, i.e., online workload and offline
workload, according to the latency demand. Online workload
refers to latency-critical inference, such as real-time recom-
mendation [14, 17] and machine translation [18, 53]. Online
workloads have strict latency demands because longer end-
to-end latency may hurt users’ experience. Additionally, the
requests for online workloads are usually submitted periodi-
cally at different frequencies. Offline workload does not have
strong latency demand, such as DL training, batch inference,
scientific computing [47], and automatic neural architecture
search [35, 51]. These workloads usually take hours or even
days to finish. The offline workloads do not have hard time
requirements and can usually highly utilize the computing
units of GPU, making them suitable to fill the idle GPU re-
source.
2.2
Production cluster for online workloads
Production clusters exploit GPUs to accelerate DL work-
loads [15, 25, 26]. GPUs are usually assigned to online work-
loads exclusively to guarantee the latency demand. We study
GPU resource utilization in production clusters from two
aspects: memory and computing power.
Low GPU resource utilization. We collect one week’s sta-
tistics of GPU computation utilization and memory usage
in the inference cluster of CompanyX, as shown in Figure 1.
The inference workloads include various popular DL mod-
els, such as CNN, GNN, LLM, and recommendation models.
As for the GPU computing utilization, we use two metrics:
GPU utilization and SM activity [3]. GPU utilization and SM
utilization represent how busy the GPU is in time and in
2


---

0
12
24
36
48
60
72
Time (h)
0
20
40
60
80
100
GPU resource (%)
GPU util.
SM act.
GPU mem.
Figure 2. Resource usage of one typical online workload.
GPU util., SM act., and GPU mem. are short for GPU utiliza-
tion, SM activity, and GPU memory usage, respectively.
space, respectively. GPU memory usage is the ratio of used
memory to memory capacity. Figure 1 illustrates that both
GPU utilization and SM utilization are lower than 60% for
more than 99% GPUs. In addition, GPU memory usage is
less than 60% for about 90% GPUs. These numbers show
that GPUs are underutilized in both memory and computing
power, indicating a great waste of valuable GPUs.
Fluctuating and predictable GPU utilization. We take
one typical online workload in the production cluster of
CompanyX as an example and show its GPU computing
utilization and memory usage in Figure 2. Both the GPU
utilization and SM activity fluctuate greatly in one day, be-
cause the number of online requests varies from time to time.
For example, more users use entertainment applications in
the evening and send more online requests to related ser-
vices, while during the day, fewer requests are sent. The
GPU memory usage is stable because the DL framework, e.g.,
PyTorch [42], caches the intermediate GPU memory for effi-
ciency. Besides, we observe that the curves of the GPU usage
metrics are smooth in minutes and periodical in days. Thus,
we can predict the GPU usage metrics by the past values.
2.3
Opportunities in GPU sharing
Some recent work [1, 2, 5, 6] has exploited GPU sharing ap-
proaches to improve GPU resource utilization. There are two
aspects of GPU sharing, i.e., time-sharing and space-sharing.
We compare GPU sharing approaches for widely-deployed
NVIDIA GPUs with an example as shown in Figure 3.
Time-sharing is not efficient to improve GPU resource
utilization. In time-sharing, shared workloads use different
time slices. To protect the performance of online workloads,
priority-based time-sharing [2, 59] (Figure 3(a)) assigns more
time slices to high-priority workloads. However, a single
online workload usually cannot fill all SMs of one GPU com-
pletely [26, 36], leading to a waste of GPU computing power.
Opportunity: space-sharing of GPU. When a workload
cannot fully utilize the GPU computing units, i.e., SMs, it can
share the idle SMs with other workloads. The SMs of one
GPU can be divided into multiple parts, and used by different
workloads simultaneously, i.e., space-sharing. We summarize
three space-sharing approaches to share widely-deployed
NVIDIA GPUs in Figure 3. NVIDIA proposes multi-instance
Online workload A
Offline workload B
kernel
one batch
…
GPU
SM0
SM1
SM2
SM3
Priority-based
time-sharing
Space-sharing
GPU
time
time
NVIDIA
MIG
Multiple
streams
NVIDIA
MPS
(a)
(b)
(c)
(d)
Time-sharing
GPU
instance
GPU
GPU
Figure 3. An example of different GPU sharing approaches
for NVIDIA GPUs.
GPU (MIG) [6] which can partition one GPU into multiple
instances, as shown in Figure 3(b). However, the partition
cannot be dynamically adjusted during workload execution,
and thus, we have to allocate maximum resources for online
workloads which leads to a waste of GPU. Additionally, MIG
only works for specific new-generation GPU types, e.g., A100
and H100, which are not widely used in production clusters.
CUDA provides multiple streams [1] (Figure 3(c)) to execute
kernels from multiple workloads, whereas the concurrent
workloads can significantly degrade the performance of on-
line workloads. Besides, NVIDIA stream can only share with
other streams in one process, which needs to merge multiple
workloads and is hard to manage in production clusters. We
find that NVIDIA MPS [5] (Figure 3(d)) is the best trade-off
between GPU resource utilization and online performance.
MPS is supported by Kepler and newer NVIDIA GPUs which
are the majority of the GPUs used in production clusters.
MPS enables NVIDIA GPU to execute multiple workloads at
the same time by assigning different sets of SMs to the shared
workloads. Besides, MPS provides environment variables to
roughly control the SM percentage used by each workload,
which enables performance protection of online workloads.
To show the effect of MPS, we choose two DL models,
VGG16 [49] and DenseNet201 [28] as workloads. We use the
inference of these DL models as online workloads and the
training as offline workloads. These workloads are tested on
NVIDIA T4 GPU. Figure 4(a) reports the normalized perfor-
mance when we share one online workload with one offline
workload. The normalized performance is the average iter-
ation duration when running alone divided by the average
3


---

V-V
V-D
D-V
D-D
0.0
0.5
1.0
Normalized performance
Online
Offline
(a)
10
40
70
100
SM percentage (%)
for offline workloads
0.00
0.25
0.50
0.75
1.00
Normalized performance
Online
Offline
(b)
Figure 4. (a) GPU sharing with MPS and adjusted SM per-
centage (A-B represents sharing one online workload A
with one offline workload B. V, D are short for VGG16 and
DenseNet201, respectively), and (b) Impact of the SM per-
centage for offline workloads (DenseNet201 as the online
workload and VGG16 as the offline workload).
iteration duration when shared with other workloads. To
protect the performance of online workloads, we adjust the
SM percentage of offline workloads. Figure 4(a) demonstrates
that one GPU can provide up to 62% more computing power
while slowing online workloads less than 20%. The results
indicate the potential of sharing GPU in space with MPS.
Challenges for space-sharing. There are some technical
challenges to deploy space-sharing in large-scale DL clusters.
First, the primary goal of production clusters is to guarantee
the performance of online workloads. MPS enables us to
roughly change the SM percentage used by each workload.
However, it cannot guarantee the performance of online
workloads. For example, the online requests may suddenly
burst due to a special activity, but the SM percentage for
offline workloads cannot be reduced timely. Thus, we need
to control the execution process of the shared workloads
to protect online workloads. Second, MPS is notorious for
its serious error propagation problems. Specifically, when
one workload encounters an error, the shared workload will
also be influenced. These safety problems are intractable
and critical in production clusters. Third, different SM per-
centages assigned to offline workloads can greatly influence
the efficiency of both shared workloads. We change the SM
percentage assigned to offline workloads from 10% to 100%
as shown in Figure 4(b). The normalized performance of
both online workload and offline workload varies more than
5×. Thus, choosing a proper SM percentage is important to
provide efficient GPU sharing. Fourth, different shared pairs
of online and offline workloads show different impacts on
the shared workloads in Figure 4(a). The normalized perfor-
mance of offline workloads varies up to 50% in Figure 4(a).
Additionally, the number of possible sharing plans is factorial
to the number of workloads, which is enormous for a pro-
duction cluster. We need to efficiently decide how to share
workloads to maximize offline efficiency, while maintaining
the performance of online workloads.
MuxFlow
Global manager
Workload
profiler
Speed
predictor
Scheduler
Online workloads
Offline workloads
Service 
manager
Online
container
Offline
container
xCUDA
NVIDIA MPS
Local executor
GPU device
e.g., T4 and A10
GPU
monitor
SysMonitor
Figure 5. MuxFlow architecture.
3
MuxFlow architecture
MuxFlow is a cluster system that enables efficient and safe
space-sharing in production clusters with tens of thousands
of heterogeneous GPUs at CompanyX. The architecture of
MuxFlow is shown in Figure 5. MuxFlow consists of a service
manager for online workloads, a global manager for offline
workloads, and a set of local executors for each GPU.
Service manager. In this paper, we only describe the func-
tionality of the service manager briefly as the details of the
service manager are beyond the scope of this paper. The
service manager is responsible for online workloads deploy-
ment, online requests discovery, and horizontal pod autoscal-
ing.
Global manager. When an offline workload is submitted to
MuxFlow, the global manager buffers the offline workload in
a pending queue and makes scheduling decisions periodically.
The global manager includes three components: workload
profiler, speed predictor, and scheduler.
Workload profiler. The workload profiler gets the GPU
resource utilization and execution time for each offline work-
load. When an offline workload is first submitted, the work-
load profiler runs the job for a few iterations and measures
the execution information. The measured information is
stored in a database and can be used by the speed predictor
when making scheduling decisions.
Speed predictor. When the speed predictor gets an online
workload and an offline workload, it can predict the sharing
speed of the given workloads. The speed predictor employs
a DL model to perform prediction. The DL model leverages
the execution information when the workloads are executed
separately. The execution information is reported by the GPU
monitor for online workloads and is profiled by the workload
4


---

profiler in advance for offline workloads. The predicted speed
is passed to the scheduler to make scheduling decisions.
Scheduler. The scheduler schedules the offline workloads
from the pending queue. By utilizing the predicted speed
from the speed predictor, the scheduler exploits a matching-
based scheduling algorithm to decide which offline work-
load and online workload should share the same GPU. The
scheduling algorithm can find the optimal sharing strategy.
The scheduler performs global rescheduling periodically at
a fixed interval.
Local executor. Each local executor manages the workloads
on one GPU. The local executor executes workloads accord-
ing to the scheduling decision of the scheduler and moni-
tors the running workloads. Besides, it can evict the offline
workload if the online workload is under threat. The work-
loads share the same GPU in space with MPS. There are four
components in the local executor: online container, offline
container with xCUDA, GPU monitor, and SysMonitor.
Online container and offline container. The online container
runs the server for online workload and serves the online
requests from upstream callers registered in the server man-
ager. The offline container runs the offline workload. With
xCUDA built in the offline container, the execution of the
offline workload is controlled to guarantee the performance
of the online workload. To do so, xCUDA limits the GPU
memory and computing power used by the offline workloads.
GPU monitor. The GPU monitor periodically collects GPU
metrics, such as GPU utilization, memory usage, and SM
clock. These data reflect the workload pressure of each GPU
and they are leveraged by the SysMonitor and xCUDA to
manage offline workloads.
SysMonitor. The SysMonitor maintains a state machine re-
flecting the device status and ensures that the device is not in
unhealthy status. The state machine transits according to the
GPU metrics collected by the GPU monitor. When the state
machine indicates one online workload is highly influenced,
the SysMonitor will evict the shared offline workload.
4
Efficient and safe space-sharing
The goal of MuxFlow is to guarantee the performance and
safety of online workloads, and improve the efficiency of
offline workloads. In this section, we introduce how to pro-
vide efficient and safe space-sharing in each local executor.
We first describe how we protect the performance of online
workloads with a two-level protection mechanism. Then we
introduce how we protect the safety of online workloads
with a mixed error-handling mechanism. We end with the
dynamic SM allocation mechanism for offline efficiency im-
provement.
4.1
Two-level performance protection
MPS provides an environment variable to control the SM
percentage used by a workload.∗We can roughly limit the of-
fline workload with this environment variable and reduce the
slowdown of online workloads indirectly. However, in pro-
duction clusters, we need rigorous protection mechanisms
for online workloads. MuxFlow employs a two-level protec-
tion mechanism as shown in Figure 6. Specifically, MuxFlow
controls offline workloads to protect online workloads at the
workload level by xCUDA and the GPU level by SysMonitor.
First of all, we need GPU metrics to make decisions on
how to control the offline workloads. In the local executor,
the GPU monitor collects real-time GPU metrics periodically.
The collection interval is in the millisecond level for timely
control. The metrics include GPU resource utilization (e.g.,
GPU utilization, SM activity, and GPU memory usage), and
GPU device status (e.g., SM clocks, power, and temperature).
The GPU monitor stores the metrics for only several min-
utes because old data not only consume storage but also are
useless for timely workload management.
Workload level. xCUDA is built in the offline container to
control the GPU memory and computing power used by
offline workloads, as shown in Figure 6(a). In the aspect of
memory, xCUDA can keep track of the GPU memory usage
and make sure that the memory used by the offline work-
load does not exceed the GPU memory quota. Specifically,
whenever the DL framework, e.g., TensorFlow [8] and Py-
Torch [42], applies for GPU memory by calling the related
CUDA API, the call will be checked by xCUDA first.
In terms of computing power, we aim to guarantee the per-
formance of online workloads and improve GPU computing
utilization. For NVIDIA GPUs, the SM clock represents how
fast the SMs execute instructions. The performance of online
workloads is greatly affected by the SM clock, and the SM
clock will decrease when the GPU load is high. The decrease
in SM clock is especially noteworthy for NVIDIA GPU for
inference, e.g., T4. Thus, our goal is equivalent to attaining
both high SM clock and high GPU utilization. When the SM
clock is low, we can delay the kernel launches of the offline
workloads to reduce the GPU load and improve the SM clock.
When the GPU utilization is low, we can launch more kernels
to improve it. Formally, we define 𝑈𝑆𝑀as the SM activity
and 𝑎𝐶as a clock factor that is negatively correlated with
the SM clock. We use the GPU load 𝑈𝐺𝑃𝑈to quantify our
goal, which can be calculated by,
𝑈𝐺𝑃𝑈= 𝑈𝑆𝑀× 𝑎𝐶.
(1)
However, the SM clock and GPU utilization are conflicting
in practice because the SM clock is negatively correlated to
∗𝐶𝑈𝐷𝐴_𝑀𝑃𝑆_𝐴𝐶𝑇𝐼𝑉𝐸_𝑇𝐻𝑅𝐸𝐴𝐷_𝑃𝐸𝑅𝐶𝐸𝑁𝑇𝐴𝐺𝐸configures the ac-
tive thread percentage at the client process level and limits the SM percent-
age used by the client process.
5


---

memory control
computing power control
xCUDA
CUDA driver (libcuda)
CUDA runtime (cudart)
NVML
satisfy memory 
quota ?
yes
invoke CUDA API
allocated
memory
required memory
allocated
memory
SM 
activity
SM 
clock
GPU utility
satisfy?
yes
PID algorithm
no
kernel
GPU
launch kernel
(a) xCUDA
Init
Healthy
Unhealthy
Overlimit
Disabled
initialized
disabled
unhealthy
threshold
unhealthy
threshold
>
<
overlimit
threshold
>
overlimit
threshold
<
enabled
available
offline workloads
unavailable
evict
(b) The state machine of SysMonitor
Figure 6. Two-level performance protection for online workloads. xCUDA and SysMonitor protect the performance of online
workloads from workload level and GPU level, respectively.
the GPU load, while the GPU utilization is positively corre-
lated to the GPU load. Note that when sharing online and
offline workloads, it is enough to get an SM clock which
is similar to the SM clock when the online workload runs
separately. Consequently, xCUDA sets an SM clock thresh-
old for these two goals. When the SM clock is below the
threshold, xCUDA tends to improve the SM clock. When the
SM clock is over the threshold, xCUDA tends to improve the
GPU utilization. The factor 𝑎𝐶can be calculated by,
𝑎𝐶=
(
1 + 𝑎𝐿∗𝑇𝑆𝑀−𝐶𝑆𝑀
𝑇𝑆𝑀
𝐶𝑆𝑀< 𝑇𝑆𝑀
1 −𝑎𝐻∗𝐶𝑆𝑀−𝑇𝑆𝑀
𝐶𝐻−𝑇𝑆𝑀
𝐶𝑆𝑀≥𝑇𝑆𝑀,
(2)
where 𝑎𝐿is a parameter for low SM clock, 𝑎𝐻is a parameter
for high SM clock, 𝐶𝑆𝑀is the SM clock, 𝑇𝑆𝑀is a SM clock
threshold, and 𝐶𝐻is the highest SM clock. 𝑎𝐿is much larger
than 𝑎𝐻to show the preference of increasing the SM clock
when it is below the threshold. With the GPU load 𝑈𝐺𝑃𝑈,
xCUDA will delay the kernel when 𝑈𝐺𝑃𝑈is high and launch
the kernel when 𝑈𝐺𝑃𝑈is low. Additionally, as the GPU load
𝑈𝐺𝑃𝑈may change rapidly, xCUDA leverages the PID algo-
rithm [32] to provide more stable and robust controlling.
GPU level. xCUDA constrains offline workloads and pro-
vides indirect performance protection for online workloads.
However, it cannot reply to changes caused by online work-
loads in time. For example, when the GPU memory usage
of the online workload bursts, xCUDA cannot dynamically
adjust the GPU memory quota for offline workloads. Thus,
at the GPU level, MuxFlow uses SysMonitor to monitor the
GPU device status with a state machine. Figure 6(b) shows
the state machine of SysMonitor. The state machine has five
states and each state has a set of metric thresholds for GPU
utilization, SM activity, SM clock, and GPU memory usage.
The threshold values are empirically selected. Note that
offline workloads can only be scheduled to Healthy GPUs.
The five states of SysMonitor are as follows: (1) Init state
represents that the GPU is being initialized. When the ini-
tialization finishes, the Init state will transit to the Healthy
Propagated errors
MPS hangs caused 
by SIGINT/SIGTERM
99%
MPS server 
crashes
0.28%
XID31 
0.15%
Other MPS 
hangs 
0.08%
Other
0.49%
Figure 7. Propagated errors in a production cluster.
state. (2)Healthy state represents that the GPU is healthy and
is able to execute offline workloads. The metric thresholds
of this state guarantee that the online workload is not influ-
enced by the offline workloads. Once one metric reaches the
Unhealthy threshold, the state will transit to the Unhealthy
state. Furthermore, once one metric exceeds the Overlimit
threshold, the state will directly transit to the Overlimit state.
(3)Unhealthy state represents that one GPU metric is in the
Unhealthy state and none is in the Overlimit state. Intuitively,
this state means that the online workloads may be influenced.
The offline workloads are forbidden to be scheduled to the
GPU in this state. Once one metric exceeds the Overlimit
threshold, the state will transit to the Overlimit state. Op-
positely, if all metrics are below the Healthy threshold, the
state will transit to the Healthy state. (4) Overlimit state rep-
resents that the GPU device is overloaded. In this state, the
offline workloads are evicted. When all metrics are below the
Overlimit threshold after a period, the state will transit to
the Unhealthy state. To avoid frequent eviction, the period is
set to be the exponent of the times going into the Overlimit
state during the last two hours. (5)Disabled state represents
that the GPU device is unavailable and no workload runs on
it.
4.2
Safety protection
MPS has a serious error propagation problem, i.e., one work-
load’s error may impact other workloads sharing the same
GPU. The error propagation problem is dangerous in large-
scale clusters, especially for online workloads. For example,
if one offline workload is canceled by SIGINT signal, the
6


---

MPS context may hang and the shared online workload can-
not serve requests. We summarize propagated errors in one
production cluster with MPS enabled as shown in Figure 7,
and propose a mixed error-handling mechanism.
We find that 99% of such propagated errors are caused by
SIGINT/SIGTERM. To handle the dominant error type, we
use xCUDA to intercept SIGINT and SIGTERM signals and
exit gracefully. Specifically, when xCUDA gets these signals,
it will freeze all kernel launches and release CUDA context
actively. Other errors only count to 1%, e.g., MPS server crash
(caused by program bugs), XID31 event (GPU memory page
fault), and MPS hangs caused by other reasons. For these
errors, we summarize their error patterns. An automated
detector monitors GPUs and alerts when the error patterns
are satisfied. Once xCUDA gets the alert, it will reset the
CUDA context and MPS server.
4.3
Dynamic SM allocation
In Figure 4(b), we illustrate that the SM percentage assigned
to offline workloads can influence the speed of shared work-
loads dramatically. In other words, we can balance the speed
of the online workload and that of the offline workload by
changing the SM allocation. Our goal is to maximize the effi-
ciency of offline workloads with an acceptable slowdown of
the online workloads. As SM is the computing unit of GPU,
maximizing the efficiency of offline workloads is approxi-
mately equal to maximizing the percentage of SMs assigned
to offline workloads. Apparently, fixed SM allocation is not
a panacea for all sharing cases. We use the example in Fig-
ure 8 to illustrate the drawbacks of fixed SM allocation. A
and B are online workloads, and C and D are offline work-
loads. Assume that the SM percentage is set to 40% for offline
workloads. The online workload A only uses 20% SMs and
leaves more than 40% SMs. If we fix the SM percentage for
offline workloads to 40%, there will be 40% idle SM and the
computing power is wasted. The online workload B uses 80%
SMs when running alone and the left SMs are less 40%. With
the fixed SM allocation, the offline workload D will occupy
B’s SM and slow the online workload B down.
To provide efficient space-sharing, we propose the dy-
namic SM allocation mechanism by selecting the proper SM
percentage for offline workloads. A natural idea is to assign
the SM percentage according to the SM activity of online
workloads, as shown in Figure 8(c). For example, we can set
the SM percentage for the offline workload C to 80% and D
to 20%. In this way, the computing units, i.e., SMs, are used
up and the shared workloads do not contend for SMs.
5
Matching-based scheduling
Note that one offline workload has diverse throughput when
sharing with different online workloads. We next consider
scheduling submitted workloads to achieve high overall
throughput for offline workloads. The method of schedul-
ing and deploying online workloads is orthogonal to the
scheduling algorithm of offline workloads. For online work-
loads, we reuse the scheduling and deployment strategy of
the exclusive inference cluster at CompanyX. The details
of the strategy are beyond the scope of this paper. For of-
fline workloads, the scheduling algorithm needs to solve two
problems.
The first problem is to capture the overall throughput for
all offline workloads. It is unfair and meaningless to simply
sum up the throughput of every offline workload because
different kinds of workloads vary in throughput when run-
ning separately. Thus, we use the normalized throughput,
which is defined as the throughput when sharing divided
by the throughput when running separately. We can get
the throughput of separate execution by profiling the of-
fline workloads when it is submitted. However, the shared
throughput is difficult to get because a production cluster
usually has thousands of online workloads and it is impossi-
ble to profile all sharing pairs. Fortunately, getting the shared
throughput can be seen as a regression problem, which can
be solved by DL. We can use the profiled separate execution
features of online and offline workloads as input, and em-
ploy a DL model to get shared throughput. Specifically, we
choose highly related execution features, e.g., GPU utiliza-
tion, SM activity, SM occupancy, separate execution time,
and assigned SM percentage, as input. We employ the multi-
layer perceptron (MLP) as the speed predictor to get the
shared throughput. Because different GPU types perform
diversely, we train multiple MLPs for each GPU type.
The second problem is to select the best sharing plan from
all potential sharing plans. For a production cluster with
thousands of online and offline workloads, there is an enor-
mous number of combinations to share workloads. Formally,
given 𝑛online workloads and 𝑚offline workloads, we pair
the workloads to maximize the normalized throughput of
the entire sharing plan. This problem can be transformed
to a maximum weighted bipartite matching problem. Fig-
ure 9 shows how we model this problem. We build a bipartite
graph 𝐺(𝑉, 𝐸), where each node 𝑣∈𝑉represents a work-
load. Every node belongs to one of the two node sets, i.e.,
the online workload set and the offline workload set. The
edge (𝑢, 𝑣) ∈𝐸represents sharing online workload 𝑢and of-
fline workload 𝑣with the normalized throughput of 𝑣as the
edge weight. A matching of 𝐺is a set of disjoint edges and
it corresponds to a feasible sharing plan. Finding the shar-
ing plan with maximum overall throughput is converted to
finding the maximum weighted matching of the correspond-
ing bipartite graph. We leverage the Kuhn-Munkres (KM)
algorithm [33, 38] for this well-studied problem. The KM
algorithm can find the optimal maximum weighted bipartite
matching in 𝑂(|𝑉|3).
We use an example in Figure 9 to illustrate the matching
problem. There are two online workloads (A and B) and
7


---

A
SM
B
(a) Online workloads only
C
A
SM
D
B
(b) Sharing with fixed SM percentage
C
A
SM
D
B
(c) Sharing with dynamic SM percentage
Figure 8. Dynamic SM allocation. A and B are two online workloads. C and D are two offline workloads. (b) The SM percentages
for offline workloads are fixed at 40%. (c) The SM percentages for offline workloads are dynamically adjusted.
online 
workload
(GPU)
offline 
workload
edge
weight
MLP
online
workload
feature
offline
workload
feature
SM
percentage
normalized throughput
A
B
C
D
E
0.3
0.8
0.8
0.4
0.4
0.3
Figure 9. Scheduling plans are computed with maximum
weighted bipartite matching. The edge weights represent the
normalized throughput of offline workloads.
three offline workloads (C, D, and E). The number on the edge
represents the normalized throughput of the offline workload
when sharing with the online workload. For example, when
sharing C with A, the normalized throughput of C is 0.3.
We compare two matching plans. Plan 1 shares A with D
and B with C. The overall throughput of offline workloads
is 0.8 + 0.8 = 1.6. Plan 2 shares A with C and B with E. The
overall throughput of offline workloads is 0.3 + 0.4 = 0.7.
Apparently, plan 1 has higher overall throughput and is more
efficient in the efficiency of offline workloads.
Algorithm 1 shows the pseudocode of the matching-based
scheduling. Given online workloads 𝑊𝑜𝑛and offline work-
loads 𝑊𝑜𝑓𝑓, we initialize the bipartite graph 𝐺where online
workloads 𝑊𝑜𝑛and offline workloads 𝑊𝑜𝑓𝑓are two disjoint
sets of𝐺(Line 1-4). For each pair of nodes, we get the SM per-
centage for the offline workload by dynamic SM allocation
mechanism (Line 5-6). The edge weights are calculated by
the speed predictor 𝑃(Line 7-8). Then we get the maximum
weighted bipartite matching 𝑀by the KM algorithm (Line
9-10). Edge (𝑢, 𝑣) in matching 𝑀represents that the offline
workload 𝑣should be shared with the online workload 𝑢.
6
Implementation
At CompanyX, we have deployed MuxFlow in our inter-
nal clusters to serve daily DL workloads. The internal clus-
ters consist of heterogeneous GPUs, including NVIDIA T4
GPU and NVIDIA A10 GPU. Integrated with Kubernetes [4],
MuxFlow manages thousands of GPUs in each cluster and
more than 20,000 GPUs in all.
Algorithm 1 Scheduling algorithm of MuxFlow
Input: Online workloads 𝑊𝑜𝑛; offline workloads 𝑊𝑜𝑓𝑓; speed
predictor 𝑃.
Main routine:
1: // Initialize
2: 𝐺.𝐼𝑛𝑖𝑡()
3: 𝐺.𝐴𝑑𝑑𝑁𝑜𝑑𝑒𝑠(𝑊𝑜𝑛)
4: 𝐺.𝐴𝑑𝑑𝑁𝑜𝑑𝑒𝑠(𝑊𝑜𝑓𝑓)
5: for each pair (𝑢, 𝑣), where 𝑢∈𝑊𝑜𝑛, 𝑣∈𝑊𝑜𝑓𝑓do
6:
𝑠𝑚←𝐷𝑦𝑛𝑎𝑚𝑖𝑐𝑆𝑀(𝑢, 𝑣)
7:
𝑤𝑒𝑖𝑔ℎ𝑡←𝑃.𝐶𝑎𝑙𝑐𝑁𝑜𝑟𝑚𝑇𝑝𝑢𝑡(𝑢, 𝑣,𝑠𝑚)
8:
𝐺.𝐴𝑑𝑑𝐸𝑑𝑔𝑒(𝑢, 𝑣,𝑤𝑒𝑖𝑔ℎ𝑡)
9: // Find optimal matching with the KM algorithm
10: 𝑀←𝐺.𝐺𝑒𝑡𝑀𝑎𝑡𝑐ℎ𝑖𝑛𝑔()
Subroutines:
• 𝐺.𝐼𝑛𝑖𝑡(): Initialize an empty bipartite graph.
• 𝐺.𝐴𝑑𝑑𝑁𝑜𝑑𝑒𝑠(𝑊): Add every workload 𝑤∈𝑊as a node to
graph 𝐺.
• 𝐺.𝐴𝑑𝑑𝐸𝑑𝑔𝑒(𝑢, 𝑣,𝑐): Add an edge (𝑢, 𝑣) with edge weight 𝑐to
graph 𝐺.
• 𝐺.𝐺𝑒𝑡𝑀𝑎𝑡𝑐ℎ𝑖𝑛𝑔(): Calculate the maximum weighted bipar-
tite matching of graph 𝐺by the KM algorithm.
• 𝐷𝑦𝑛𝑎𝑚𝑖𝑐𝑆𝑀(𝑢, 𝑣): Use dynamic SM allocation mechanism to
get the proper SM percentage for offline workload 𝑣.
•
𝑃.𝐶𝑎𝑙𝑐𝑁𝑜𝑟𝑚𝑇𝑝𝑢𝑡(𝑢, 𝑣,𝑠𝑚):
Calculate
the
normalized
throughput of 𝑣by the speed predictor 𝑃.
Service manager. For online workloads, we use the existing
service manager at CompanyX which deploys containers,
discovers service, and autoscales horizontal pods.
Global manager. We modify the Kubernetes scheduler to
schedule offline workloads. The workload profiler takes sev-
eral dedicated GPUs, whose number is negligible to the total
number of GPUs. When a new offline workload comes, the
workload profiler performs a few dry runs of the workload
and utilizes the NVIDIA Data Center GPU Manager (DCGM)
tools [3] and NVIDIA Management Library (NVML) [7] li-
braries to collect GPU metrics. We collect about 2,000 data
for each GPU type to train the speed predictor. The MLPs of
the speed predictor have four layers with hidden size 64× 64.
The MLPs are trained with momentum SGD optimizer [45]
in PyTorch v1.8.0 [42] until they converge. MuxFlow invokes
the scheduler periodically to schedule all offline workloads.
When moving workloads, we record checkpoints of offline
8


---

workloads and restart the workloads after transmitting the
models and checkpoints. As the datasets are usually colos-
sal, we store the datasets in a remote file system and fetch
data during the execution. We implement the scheduler as a
third-party plugin to the Kubernetes scheduler.
Local executor. Each local executor executes online
workloads according to the service manager and offline
workloads according to the global manager. DL work-
loads are executed in Docker containers with our cus-
tomized components. We add Best-Effort GPU Device-
Plugin in Kubernetes and relevant control paths with
Kubelet and SysMonitor for offline workloads. To con-
trol SM percentage, we leverage the environment variable
𝐶𝑈𝐷𝐴_𝑀𝑃𝑆_𝐴𝐶𝑇𝐼𝑉𝐸_𝑇𝐻𝑅𝐸𝐴𝐷_𝑃𝐸𝑅𝐶𝐸𝑁𝑇𝐴𝐺𝐸provided
by MPS. The GPU monitor collects resource metrics through
DCGM [3] and NVML [7] for NVIDIA GPU. The SysMonitor
updates the state machine with the collected resource metrics
and empirically-set thresholds. When the state is unhealthy,
the SysMonitor will ask the NodeManager in Kubernetes to
evict offline workloads. xCUDA intercepts nearly 800 CUDA
driver APIs for GPU memory allocation and kernel launch.
The GPU memory quota of offline workloads is fixed to 40%
as Figure 1 reports that most online workloads use less than
60% GPU memory. We adopt the cpuset of Cgroup for CPU
isolation. For memory, MuxFlow will evict offline workloads
if memory usage is higher than a threshold or the kernel
swap daemon is busy for a long time. The parameters to
calculate GPU load in Equation 1&2 are empirically selected
through trial-and-error.
7
Evaluation
Our evaluation consists of testbed experiments, trace-driven
simulations, and results from the production deployment.
We mainly focus on the efficiency and safety of MuxFlow.
The results show that MuxFlow oversells up to 90.0% GPU
resources to offline workloads, and improves the GPU utiliza-
tion by 4.0×, SM activity by 4.7×, and GPU memory usage by
1.5×. The error rate of MuxFlow is similar to the dedicated
inference clusters in production deployment at CompanyX.
7.1
Experimental setup
Testbed. We conduct the testbed experiments with 125 ma-
chines and 1, 000 GPUs. Each machine is equipped with 8
NVIDIA Tesla T4 GPUs, 2 Intel Xeon Platinum CPUs, 128G
memory, and 100+25G NIC. We PyTorch v1.8.0 with CUDA
11.1 for offline workloads. We use real online workloads in
our production clusters and these workloads use different
DL frameworks and CUDA drivers.
Simulator. Inspired by [23, 63], we build a simulator to
evaluate a broader set of configurations, traces, and baselines.
We profile the iteration duration, GPU resource utilization,
and device metrics of the selected DL workloads when they
are executed separately and shared with others. The profiling
results include more than 200 executions. The difference
between the simulation and testbed experiment is under 5%,
showing the high fidelity of the simulator.
Workloads. We use the actual online workloads and real-
time requests at CompanyX for the testbed experiment. The
online workloads use a wide spectrum of DL models, in-
cluding CNN, GNN, LLM, and recommendation models. For
trace-driven simulation, we select three online workloads
deployed over hundreds of GPUs at CompanyX, and gener-
ate requests according to their actual query per second (QPS)
varying from 20 to 190. For offline workloads, we leverage
the public trace from Microsoft [31] and split the trace ac-
cording to the virtual cluster ID. We use submission time
and duration from the traces and randomly choose DL mod-
els from four popular DL models including ResNet50 [27],
VGG16 [49], DensNet201 [28], and Inception-V3 [50], in ac-
cordance with the common practice [23, 26, 63]. We repeat
the workloads to fit in 1,000 GPUs and guarantee that the
generated traces can be finished in 12 hours for the testbed
experiment and 24 hours for simulations. The numbers of
offline workloads in these traces vary from 1,410 to 7,287.
We set the batch size according to the memory quota limited
by xCUDA.
Baselines. We compare MuxFlow with three related sys-
tems, Online-only, Time-sharing, and Priority-based time-
sharing (PB-time-sharing). Online-only executes only the
online workloads and shows the optimal latency for online
workloads. Time-sharing shares workloads in time and as-
signs the time slices of GPUs to the shared workloads by the
GPU driver strategy, which is adopted by Gandiva [58]. PB-
time-sharing sets online workloads with high priority and
assigns more time slices of GPUs to high-priority workloads
to protect the high-priority workloads’ performance, which
is adopted by AntMan [59] and PAI [56].
Metrics. Average latency and 99% −𝑡ℎlatency are two
common metrics to evaluate the performance of online
workloads [25, 26]. Average job completion time (JCT) and
makespan are used to reflect the workload efficiency of sched-
ulers [59, 63]. Offline normalized throughput shows the shar-
ing efficiency. We define how much GPU the offline work-
loads get in the aspect of computation speed as the oversold
GPU. This metric is in the range of [0, 1], where 0 represents
that the offline workloads get no GPU computation resource,
and 1 represents that the offline workloads get equivalent
GPU computation resources as they are executed exclusively.
This metric can be calculated by Equation 3.
𝑂𝑣𝑒𝑟𝑠𝑜𝑙𝑑𝐺𝑃𝑈=
Í
𝑤∈𝑊𝑜𝑓𝑓𝑇𝑟𝑒𝑎𝑙
𝑤
Í
𝑤∈𝑊𝑜𝑓𝑓𝑇𝑠𝑒𝑝
𝑤
,
(3)
where 𝑊𝑜𝑓𝑓represents offline workloads, 𝑇𝑟𝑒𝑎𝑙
𝑤
represents
the real execution time of 𝑤, and 𝑇𝑠𝑒𝑝
𝑤
represents the execu-
tion time of 𝑤when running exclusively. We report GPU
9


---

0
10
20
30
Average
latency
(ms)
Online-only
MuxFlow
0
25
50
75
99%-th
latency
(ms)
Online-only
MuxFlow
0
500
1000
Offline
normalized
throughput
MuxFlow
0
50
100
GPU util.
(%)
Online-only
MuxFlow
0
50
100
SM act.
(%)
Online-only
MuxFlow
0
3
6
9
12
Time (h)
0
50
100
GPU mem.
(%)
Online-only
MuxFlow
Figure 10. Detailed metrics in the testbed experiment.
resource utilization with three metrics: GPU utilization, SM
activity, and GPU memory utilization.
7.2
Testbed experiments
We first evaluate MuxFlow on a testbed with 1,000 GPUs.
Figure 10 shows the detailed metrics for online workloads,
offline workloads, and GPU resource utilization. To get the
metrics of Online-only, we stop the offline workloads for one
minute in every scheduling interval and collect the metrics.
The scheduling interval is set to 15 minutes considering the
overhead of pulling images and initialization. Though the
trace of offline workloads lasts for 12 hours, most workloads
finish before 8 hours. Thus, there is an obvious shift for
MuxFlow’s curves of offline normalized throughput and GPU
resource utilizations between 7 to 8 hours.
Performance of online workloads. Figure 10 shows that
MuxFlow increases the average latency by 16.0%, the 99%−𝑡ℎ
latency by 15.3%. These results indicate that MuxFlow slows
down online workloads less than 20%, i.e., 10ms. It is worth
mentioning that such a slowdown is almost imperceptible for
most online workloads, e.g. recommendation services and
machine translation. In the wide spectrum of industrial on-
line workloads deployed in CompanyX, the latency demand
of most online workloads is more than 100ms, hence the
10ms slowdown of online workloads is acceptable in prac-
tice. Additionally, we can adjust xCUDA and the dynamic
SM allocation mechanism to reduce the slowdown threshold
or improve the oversold resource for offline workloads. We
observe that 1.5% executions of offline workloads are evicted,
indicating the functionality of performance protection mech-
anisms.
Efficiency of offline workloads. We find that MuxFlow
provides up to 86.42% GPU resource to offline workloads,
which is a substantial number considering the large number
of GPUs in production.
GPU resource utilization. Figure 10 compares the GPU
computing utilization and memory usage between Online-
only and MuxFlow. The utilization numbers are the average
of all GPUs. MuxFlow improves the GPU utilization by 4.0×,
SM activity by 4.7×, and GPU memory usage by 1.5×.
Safety. We observe that during the 12-hour testbed exper-
iments, no error propagation happens. That is, no online
workload is influenced by offline workload errors, verifying
the safety of MuxFlow.
7.3
Comparison with related work
We compare MuxFlow with three related systems, Online-
only, Time-sharing, and PB-time-sharing. The related sys-
tems are implemented in our simulator, and evaluated with
production online workloads and popular offline workloads.
Figure 11 demonstrates the average latency of online work-
loads, average JCT of offline workloads, and oversold GPU
to offline workloads. We normalize the latency by that of
Online-only and other metrics by that of MuxFlow. MuxFlow
improves the average JCT by 1.10 −2.24×, and the oversold
GPU by 1.08 −1.97×, while slowing down the online work-
loads by less than 20%. Time-sharing slows down online
workloads by up to 50%, indicating a great impact on on-
line workloads. PB-time-sharing utilizes priority to protect
the performance of online workloads, but its metrics for
offline workloads are worse than MuxFlow due to two rea-
sons. First, MuxFlow can utilize the GPU resource wasted
by online workloads in space. Second, MuxFlow employs
the scheduling algorithm to improve the efficiency of offline
workloads.
7.4
Analysis of MuxFlow
Accuracy of the speed predictor. To better understand
the impact of MLP architecture on prediction accuracy, we
evaluate the speed predictor with various hidden sizes and
numbers of the network layers in MLP. We vary the hidden
size from 64 × 64 to 1024 × 1024 and fix the number of layers
to 4. The test error curves in Figure 12(a) show that the
MLPs with different hidden sizes have similar accuracy and
convergence speed. For the number of layers, we evaluate
the MLP with 2 to 8 layers as shown in Figure 12(b), with the
hidden size fixed to 64 × 64. We find that the error is lowest
for 4 layers due to its proper relationship between dataset
size and parameters. Thus, we select 64 × 64 as the hidden
size and 4 as the layer number for better accuracy and faster
inference time.
10


---

A
B
C
D
0
0.5
1.0
1.5
2.0
Normalized latency
Online-only
Time-sharing
PB-time-sharing
MuxFlow
(a) Latency of online workloads.
A
B
C
D
0
1
2
3
Normalized
Average JCT
Online-only
Time-sharing
PB-time-sharing
MuxFlow
(b) JCT of offline workloads.
A
B
C
D
0.0
0.5
1.0
1.5
Oversold GPU (%)
Online-only
Time-sharing
PB-time-sharing
MuxFlow
(c) Oversold GPU.
Figure 11. Compare MuxFlow with related work.
0
2000
4000
6000
Epoch
0.0
0.1
0.2
0.3
Test error (%)
64
256
512
1024
(a) Impact of the hidden size.
0
2000
4000
6000
Epoch
0
0.1
0.2
0.3
0.4
Test error (%)
2
4
8
(b) Impact of the number of layers.
Figure 12. Impact of the MLP architecture on the prediction
accuracy.
Impact of the mechanisms for offline efficiency. We
also study the impact of the dynamic SM allocation
mechanism and the matching-based scheduling. We lever-
age the simulator to compare MuxFlow with three vari-
ants, MuxFlow without dynamic SM allocation mechanism
(MuxFlow-S), MuxFlow without matching-based scheduling
(MuxFlow-M), and MuxFlow without both dynamic SM allo-
cation mechanism and matching-based scheduling but only
the protection mechanisms for online workloads (MuxFlow-
S-M). Figure 13 reports the metrics for online and offline
workloads over traces A to D. We normalize the latency by
that of Online-only and average JCT by that of MuxFlow.
Note that compared with MuxFlow-S-M, both MuxFlow-S
and MuxFlow-M improve the average JCT and oversold GPU.
These improvements confirm that only the online protection
mechanisms are not efficient, and dynamic SM allocation
mechanism and matching-based scheduling are important
for offline efficiency. Additionally, combining the two mech-
anisms, i.e., MuxFlow, brings more benefits.
System overhead. MuxFlow mainly introduces two kinds
of overhead, i.e., the profiling overhead and the scheduling
overhead. The profiling takes less than 10 minutes for each
offline workload. The profiling overhead is marginal, as the
offline workloads usually take hours or even days, The over-
head of the scheduling algorithm consists of two periods. The
first period is to predict the sharing performance and build
the bipartite graph. Each prediction only takes less than one
millisecond, and each internal cluster at CompanyX consists
of thousands of GPUs and thousands of workloads. Thus, the
first period only takes several seconds for each cluster using
the batched prediction. The second period is to execute the
KM algorithm, which takes several minutes for thousands
of workloads. Note that the scheduling algorithm can be
executed in parallel with the workload execution. Thus, we
can hide the scheduling overhead within each scheduling
interval.
7.5
Production deployment
We have deployed MuxFlow on the production clusters with
more than 20,000 GPUs at CompanyX. As the results of
the whole system are not ready when the paper is written,
we concentrate on the results of MuxFlow without the dy-
namic SM allocation mechanism and the matching-based
scheduling. To verify the performance protection for online
workloads provided by MuxFlow, we collect the latency and
throughput of online workloads that are deployed with both
MuxFlow and dedicated inference clusters (Online-only), as
shown in Figure 14. The average latency and 99% −𝑡ℎla-
tency of online workloads increase by less than 10𝑚𝑠. The
slowdown of online workloads is acceptable compared with
the latency demand of our online workloads. Besides, we
collect the average resource utilization of all GPUs used by
MuxFlow and Online-only for four weeks in Figure 15. We
observe that MuxFlow improves the GPU utilization from
26% to 76%, SM activity from 16% to 33%, and GPU mem-
ory from 42% to 48%, indicating the efficiency of MuxFlow.
The GPU memory utilization increases less than other uti-
lizations because of our conservative memory limitation for
offline workloads.
The percentage of daily error devices of MuxFlow is below
0.9%, which is slightly higher than the error rate of Online-
only at CompanyX, 0.7%. However, the increase of the error
rate is much less than the increase of the executed containers,
i.e., 2×. Compared with Online-only, the extra device errors
of MuxFlow come from MPS server crashes and other MPS
hangs, which cannot be handled with existing mechanisms.
8
Lessons and future direction
Safety protection. How to guarantee the safety of shared
workloads is one of the most important problems of deploy-
ing GPU sharing. Thus, most GPU sharing solutions [2, 24]
in production do not consider MPS due to its poor isolation
ability. In contrast, to our best knowledge, we are the first
to thoroughly analyze all unsafe cases encountered in our
production cluster and propose corresponding solutions for
11


---

A
B
C
D
0
0.5
1.0
1.5
2.0
Normalized latency
Online-only
MuxFlow-S-M
MuxFlow-S
MuxFlow-M
MuxFlow
(a) Latency of online workloads.
A
B
C
D
0
1
2
3
Normalized
Average JCT
Online-only
MuxFlow-S-M
MuxFlow-S
MuxFlow-M
MuxFlow
(b) JCT of offline workloads.
A
B
C
D
0.0
0.5
1.0
1.5
Oversold GPU (%)
Online-only
MuxFlow-S-M
MuxFlow-S
MuxFlow-M
MuxFlow
(c) Oversold GPU.
Figure 13. Impact of the mechanisms for offline efficiency.
0
100
200
Latency (ms)
0
20
40
60
80
100
CDF (%)
Online-only
MuxFlow
(a) Latency.
0
200
400
Throughput (reqs/s)
0
20
40
60
80
100
CDF (%)
Online-only
MuxFlow
(b) Throughput.
Figure 14. Performance of online workloads in deployment.
0
50
100
GPU util.
(%)
Online-only
MuxFlow
0
50
100
SM act.
(%)
Online-only
MuxFlow
0
1
2
3
4
Time (weeks)
0
50
100
GPU mem.
(%)
Online-only
MuxFlow
Figure 15. GPU resource utilization in deployment.
these cases. Almost all unsafe cases in our deployment can be
handled by the graceful exit mechanism of MuxFlow. Besides,
we have worked with NVIDIA to improve MPS. Some bugs
and features we reported have been verified and fixed by
NVIDIA. For example, we observed that sharing workloads
compiled by different GCC versions with MPS can cause the
MPS server hangs, and this problem has been verified by
NVIDIA and fixed in NVIDIA GPU Driver 470.
However, things become complicated when considering
malicious behaviors, e.g., intriguing sticky CUDA error by
dividing zero to influence the shared workload. To avoid ma-
licious behaviors, MuxFlow only accepts trustworthy offline
workloads and we employ a fault detector with manually-
summarized rules to monitor collected device metrics and
alert when abnormal situations are found. For now, MuxFlow
is only used in our internal clusters and for internal users.
These protection approaches seem safe enough according to
our deployment experience. Yet if considering external users
in a cloud setting, we need more general and automated
approaches for safety protection and malicious behaviour
detection. One opportunity is to leverage DL to discover ma-
licious behaviours automatically [46]. Besides, enabling the
scheduler to identify fail-prone workloads and avoid sharing
them with other workloads is another possible approach.
Slowdown of online workloads. In this paper, we get the
latency of online workloads increases less than 20%, i.e., 10𝑚𝑠.
The degradation is affordable and acceptable in our internal
cluster, because most latency demands are more than 100ms
for production online workloads. Note that the degradation
threshold is a tradeoff between the online service quality
and resource utilization, and it can be changed in MuxFlow
by two mechanisms. First, the parameters of GPU load 1&2
in xCUDA affect how the offline workload is executed and
then how the online workload is influenced. Second, we can
adjust the SM percentage assigned to offline workloads to
change the slowdown of online workloads. How to select a
proper degradation threshold for each cluster or even each
online workload is left as an open problem.
The number of shared workloads. In MuxFlow, we share
at most one offline workload with each online workload
because one offline workload is usually enough to fill SMs
up. Sharing multiple offline workloads with multiple on-
line workloads may bring more benefits, especially for light-
weighted offline workloads. However, there are four chal-
lenges to sharing multiple workloads. First, we need to guar-
antee the performance of all online workloads. Second, we
need to limit the total SM percentage used by multiple of-
fline workloads which cannot be simply limited by MPS
parameters. Third, xCUDA needs to monitor kernel launches
of all offline workloads and decide which kernel to delay
or launch according to their priority. Fourth, the schedul-
ing algorithm to choose sharing pairs with more than three
workloads becomes an NP-hard problem [63]. How to solve
these challenges to utilize GPU better is an interesting future
direction.
9
Related work
DL workload scheduling. Existing DL schedulers are
mainly designed for online or offline workloads, but not
both, to ensure the performance of online workloads and
avoid interference. The primary goals of online workload
schedulers [15, 25, 44, 48] are meeting the latency demand
12


---

and improving overall throughput. However, these sched-
ulers let one workload monopolize GPUs and thus, cannot
fully exploit the GPU resource. Most prior offline workload
schedulers [23, 29, 37, 43] also allocate GPUs exclusively.
However, existing offline workload schedulers cannot be di-
rectly applied to GPU-sharing clusters because they cannot
ensure the performance of high-priority online workloads.
Differently, MuxFlow leverages space-sharing to improve
GPU resource utilization and applies a two-level protection
mechanism to guarantee the performance of online work-
loads.
Resource sharing for big data workloads. Prior work
has studied resource sharing for big data workloads and
CPU clusters. DRF [19] extends max-min fairness to achieve
resource sharing fairness. Tetris [20], Graphene [22], and
Carbyne [21] propose heuristic algorithms to solve multi-
resource scheduling problems and improve resource utiliza-
tion. MonoSpark [40] improves performance clarity by split-
ting data analytics tasks into monotasks. Many large enter-
prises deploy resource-sharing clusters. Apollo system [11]
improves resource utilization by opportunistic tasks in Mi-
crosoft. Google’s Borg [52, 54] adopts machine sharing with
performance isolation to achieve high utilization. In com-
parison, DL workloads use GPUs to speed up, and GPUs
lack efficient and safe sharing mechanisms. Thus, it is more
challenging to deploy GPU sharing in production clusters.
GPU sharing for DL workloads. Recently, GPU sharing
has been studied for DL workloads. The techniques to share
GPUs mainly fall into two categories. Prior time-sharing ap-
proaches [34, 55, 58, 63] may impact the efficiency of shared
workloads and cannot fully utilize GPU computing power in
every time slice. Salus [60] and PipeSwitch [10] propose fast
job switching and memory management techniques to speed
up time-sharing. But they cannot avoid the intrinsic draw-
backs of time-sharing. Some work [2, 24, 56, 59] assigns time
slices according to priority to guarantee the performance of
online workloads. However, these approaches still cannot
improve resource utilization for each time slice. MuxFlow
employs space-sharing and performance protection mecha-
nisms for efficient and safe GPU sharing.
Space-sharing is the other direction to share GPU.
NVIDIA’s MPS [5] is a general method to multiplex jobs
on NVIDIA GPUs. Gavel [39] leverages MPS directly but
it cannot guarantee the performance of online workloads.
GSLICE [16] advances MPS to support dynamic and fair
resource allocation but it does not consider cluster-level
scheduling. Retiarii [61] merges multiple similar models
to improve GPU utilization which is infeasible for produc-
tion clusters running diverse workloads. DeepPool [41] and
Reef [26] leverage priority-based multi-stream approaches.
However, multi-stream approaches are unfit for production
deployment mainly due to two reasons. First, it needs to man-
ually merge different workloads into one process to leverage
NVIDIA GPU streams, which is not friendly to existing infras-
tructure and users. Second, multiple streams may introduce
the overhead of locks and CPU kernel launches. In contrast,
MuxFlow is a practical space-sharing cluster system that has
been deployed at CompanyX.
Some work predicts performance interference among
shared workloads for time-sharing [12, 13] and space-
sharing [62]. These approaches play the similar role as the
DL-based speed predictor in MuxFlow, and are orthogonal
to other system designs.
10
Conclusion
In this paper, we have presented MuxFlow, the first produc-
tion DL cluster system for efficient and safe space-sharing.
MuxFlow ensures the performance of online workloads from
both workload level and GPU level. To guarantee the safety
of online workloads, MuxFlow leverages a mixed error-
handling mechanism based on the analysis of production
errors. Furthermore, MuxFlow exploits dynamical SM allo-
cation and matching-based scheduling to improve the effi-
ciency of offline workloads. The evaluation results demon-
strate the efficiency and efficacy of MuxFlow. Particularly,
MuxFlow has been already deployed in the production DL
clusters at CompanyX with more than 20,000 GPUs.
13


---

References
[1] [n. d.]. NVIDIA Multiple Streams. https://developer.download.nvidia.
cn/CUDA/training/StreamsAndConcurrencyWebinar.pdf.
[2] 2022.
cGPU.
https://www.alibabacloud.com/help/en/elastic-gpu-
service/latest/cgpu.
[3] 2022. DCGM. https://developer.nvidia.com/dcgm.
[4] 2022. Kubernetes. http://kubernetes.io.
[5] 2022. NVDIA MPS. https://docs.nvidia.com/deploy/mps/index.html.
[6] 2022. NVIDIA MIG. https://www.nvidia.com/en-us/technologies/
multi-instance-gpu/.
[7] 2022.
NVML.
https://developer.nvidia.com/nvidia-management-
library-nvml.
[8] Martín Abadi, Paul Barham, Jianmin Chen, Zhifeng Chen, Andy Davis,
Jeffrey Dean, Matthieu Devin, Sanjay Ghemawat, Geoffrey Irving,
Michael Isard, et al. 2016. TensorFlow: a system for Large-Scale ma-
chine learning. In USENIX OSDI.
[9] Miguel Alcon, Hamid Tabani, Leonidas Kosmidis, Enrico Mezzetti,
Jaume Abella, and Francisco J Cazorla. 2020. Timing of autonomous
driving software: Problem analysis and prospects for future solutions.
In IEEE RTAS.
[10] Zhihao Bai, Zhen Zhang, Yibo Zhu, and Xin Jin. 2020. PipeSwitch:
Fast Pipelined Context Switching for Deep Learning Applications. In
USENIX OSDI.
[11] Eric Boutin, Jaliya Ekanayake, Wei Lin, Bing Shi, Jingren Zhou, Zheng-
ping Qian, Ming Wu, and Lidong Zhou. 2014. Apollo: Scalable and
Coordinated Scheduling for {Cloud-Scale} Computing. In USENIX
OSDI.
[12] Quan Chen, Hailong Yang, Minyi Guo, Ram Srivatsa Kannan, Jason
Mars, and Lingjia Tang. 2017. Prophet: Precise qos prediction on non-
preemptive accelerators to improve utilization in warehouse-scale
computers. In ACM ASPLOS.
[13] Quan Chen, Hailong Yang, Jason Mars, and Lingjia Tang. 2016. Bay-
max: Qos Awareness and Increased Utilization for Non-preemptive
Accelerators in Warehouse Scale Computers. ACM ASPLOS (2016).
[14] Paul Covington, Jay Adams, and Emre Sargin. 2016. Deep neural
networks for youtube recommendations. In ACM RecSys.
[15] Daniel Crankshaw, Xin Wang, Guilio Zhou, Michael J Franklin,
Joseph E Gonzalez, and Ion Stoica. 2017. Clipper: A Low-Latency
Online Prediction Serving System. In USENIX NSDI.
[16] Aditya Dhakal, Sameer G Kulkarni, and KK Ramakrishnan. 2020.
Gslice: Controlled Spatial Sharing of GPUs for a Scalable Inference
Platform. In ACM SoCC.
[17] Weihao Gao, Xiangjun Fan, Chong Wang, Jiankai Sun, Kai Jia, Wenzi
Xiao, Ruofan Ding, Xingyan Bin, Hui Yang, and Xiaobing Liu. 2021.
Learning An End-to-End Structure for Retrieval in Large-Scale Rec-
ommendations. In ACM CIKM.
[18] Jonas Gehring, Michael Auli, David Grangier, Denis Yarats, and Yann N
Dauphin. 2017. Convolutional sequence to sequence learning. In
PMLR.
[19] Ali Ghodsi, Matei Zaharia, Benjamin Hindman, Andy Konwinski, Scott
Shenker, and Ion Stoica. 2011. Dominant resource fairness: Fair alloca-
tion of multiple resource types. In USENIX NSDI.
[20] Robert Grandl, Ganesh Ananthanarayanan, Srikanth Kandula, Sriram
Rao, and Aditya Akella. 2014. Multi-resource packing for cluster
schedulers. In ACM SIGCOMM.
[21] Robert Grandl, Mosharaf Chowdhury, Aditya Akella, and Ganesh
Ananthanarayanan. 2016. Altruistic scheduling in multi-resource
clusters. In USENIX OSDI.
[22] Robert Grandl, Srikanth Kandula, Sriram Rao, Aditya Akella, and
Janardhan Kulkarni. 2016. Graphene: Packing and dependency-aware
scheduling for data-parallel clusters. In USENIX OSDI.
[23] Juncheng Gu, Mosharaf Chowdhury, Kang G Shin, Yibo Zhu, Myeong-
jae Jeon, Junjie Qian, Hongqiang Liu, and Chuanxiong Guo. 2019.
Tiresias: A GPU cluster manager for distributed deep learning. In
USENIX NSDI.
[24] Jing
Gu,
Shengbo
Song,
Ying
Li,
and
Hanmei
Luo.
2018.
GaiaGPU:
sharing
GPUs
in
container
clouds.
In
IEEE
ISPA/IUCC/BDCloud/SocialCom/SustainCom.
[25] Arpan Gujarati, Reza Karimi, Safya Alzayat, Wei Hao, Antoine Kauf-
mann, Ymir Vigfusson, and Jonathan Mace. 2020. Serving DNNs like
Clockwork: Performance Predictability from the Bottom Up. In USENIX
OSDI.
[26] Mingcong Han, Hanze Zhang, Rong Chen, and Haibo Chen. 2022.
Microsecond-scale Preemption for Concurrent GPU-accelerated DNN
Inferences. In USENIX OSDI.
[27] Kaiming He, Xiangyu Zhang, Shaoqing Ren, and Jian Sun. 2016. Deep
residual learning for image recognition. In IEEE CVPR.
[28] Gao Huang, Zhuang Liu, Laurens Van Der Maaten, and Kilian Q Wein-
berger. 2017. Densely connected convolutional networks. In IEEE
CVPR.
[29] Changho Hwang, Taehyun Kim, Sunghyun Kim, Jinwoo Shin, and
KyoungSoo Park. 2021. Elastic resource sharing for distributed deep
learning. In USENIX NSDI.
[30] Wonseok Jang, Hansaem Jeong, Kyungtae Kang, Nikil Dutt, and Jong-
Chan Kim. 2020. R-TOD: Real-time object detector with minimized
end-to-end delay for autonomous driving. In IEEE RTSS.
[31] Myeongjae Jeon, Shivaram Venkataraman, Amar Phanishayee, Junjie
Qian, Wencong Xiao, and Fan Yang. 2019. Analysis of large-scale
multi-tenant GPU clusters for DNN training workloads. In USENIX
ATC.
[32] Michael A Johnson and Mohammad H Moradi. 2005. PID control.
Springer.
[33] Harold W Kuhn. 1955. The Hungarian method for the assignment
problem. Naval research logistics quarterly (1955).
[34] Gangmuk Lim, Jeongseob Ahn, Wencong Xiao, Youngjin Kwon, and
Myeongjae Jeon. 2021. Zico: Efficient GPU memory sharing for con-
current DNN training. In USENIX ATC.
[35] Chenxi Liu, Barret Zoph, Maxim Neumann, Jonathon Shlens, Wei Hua,
Li-Jia Li, Li Fei-Fei, Alan Yuille, Jonathan Huang, and Kevin Murphy.
2018. Progressive neural architecture search. In ECCV.
[36] Lingxiao Ma, Zhiqiang Xie, Zhi Yang, Jilong Xue, Youshan Miao, Wei
Cui, Wenxiang Hu, Fan Yang, Lintao Zhang, and Lidong Zhou. 2020.
Rammer: Enabling Holistic Deep Learning Compiler Optimizations
with rTasks. In USENIX OSDI.
[37] Jayashree Mohan, Amar Phanishayee, Janardhan Kulkarni, and Vijay
Chidambaram. 2022. Looking beyond GPUs for DNN scheduling on
multi-tenant clusters. In USENIX OSDI.
[38] James Munkres. 1957. Algorithms for the assignment and transporta-
tion problems. Journal of the society for industrial and applied mathe-
matics (1957).
[39] Deepak Narayanan, Keshav Santhanam, Fiodar Kazhamiaka, Amar
Phanishayee, and Matei Zaharia. 2020. Heterogeneity-aware cluster
scheduling policies for deep learning workloads. In USENIX OSDI.
[40] Kay Ousterhout, Christopher Canel, Sylvia Ratnasamy, and Scott
Shenker. 2017. Monotasks: Architecting for performance clarity in
data analytics frameworks. In ACM SOSP.
[41] Seo Jin Park, Joshua Fried, Sunghyun Kim, Mohammad Alizadeh, and
Adam Belay. 2022. Efficient Strong Scaling Through Burst Parallel
Training.
[42] Adam Paszke, Sam Gross, Francisco Massa, Adam Lerer, James
Bradbury, Gregory Chanan, Trevor Killeen, Zeming Lin, Natalia
Gimelshein, Luca Antiga, et al. 2019. Pytorch: An imperative style,
high-performance deep learning library. In NIPS.
[43] Aurick Qiao, Willie Neiswanger, Qirong Ho, Hao Zhang, Gregory R
Ganger, and Eric P Xing. 2021. Pollux: Co-adaptive cluster scheduling
for goodput-optimized deep learning. In USENIX OSDI.
[44] Francisco Romero, Qian Li, Neeraja J Yadwadkar, and Christos
Kozyrakis. 2021. INFaaS: Automated Model-less Inference Serving. In
14


---

USENIX ATC.
[45] Sebastian Ruder. 2016. An overview of gradient descent optimization
algorithms. arXiv preprint arXiv:1609.04747 (2016).
[46] Syahril Ramadhan Saufi, Zair Asrar Bin Ahmad, Mohd Salman Leong,
and Meng Hee Lim. 2019. Challenges and opportunities of deep learn-
ing models for machinery fault detection and diagnosis: A review. IEEE
Access (2019).
[47] Andrew W Senior, Richard Evans, John Jumper, James Kirkpatrick,
Laurent Sifre, Tim Green, Chongli Qin, Augustin Žídek, Alexander WR
Nelson, Alex Bridgland, et al. 2020. Improved protein structure predic-
tion using potentials from deep learning. Nature (2020).
[48] Haichen Shen, Lequn Chen, Yuchen Jin, Liangyu Zhao, Bingyu Kong,
Matthai Philipose, Arvind Krishnamurthy, and Ravi Sundaram. 2019.
Nexus: A GPU cluster engine for accelerating DNN-based video anal-
ysis. In ACM SOSP.
[49] Karen Simonyan and Andrew Zisserman. 2015. Very deep convolu-
tional networks for large-scale image recognition. In ICLR.
[50] Christian Szegedy, Vincent Vanhoucke, Sergey Ioffe, Jon Shlens, and
Zbigniew Wojna. 2016. Rethinking the inception architecture for
computer vision. In IEEE CVPR.
[51] Mingxing Tan, Bo Chen, Ruoming Pang, Vijay Vasudevan, Mark San-
dler, Andrew Howard, and Quoc V Le. 2019. Mnasnet: Platform-aware
neural architecture search for mobile. In CVPR.
[52] Muhammad Tirmazi, Adam Barker, Nan Deng, Md E Haque, Zhi-
jing Gene Qin, Steven Hand, Mor Harchol-Balter, and John Wilkes.
2020. Borg: the next generation. In EuroSys.
[53] Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion
Jones, Aidan N Gomez, Łukasz Kaiser, and Illia Polosukhin. 2017. At-
tention is all you need. In NIPS.
[54] Abhishek Verma, Luis Pedrosa, Madhukar Korupolu, David Oppen-
heimer, Eric Tune, and John Wilkes. 2015. Large-scale cluster manage-
ment at Google with Borg. In EuroSys.
[55] Guanhua Wang, Kehan Wang, Kenan Jiang, Xiangjun Li, and Ion Stoica.
2021. Wavelet: Efficient DNN training with tick-tock scheduling. In
MLSys.
[56] Qizhen Weng, Wencong Xiao, Yinghao Yu, Wei Wang, Cheng Wang,
Jian He, Yong Li, Liping Zhang, Wei Lin, and Yu Ding. 2022. MLaaS in
the wild: Workload analysis and scheduling in large-scale heteroge-
neous GPU clusters. In USENIX NSDI.
[57] Yecheng Xiang and Hyoseung Kim. 2019.
Pipelined data-parallel
CPU/GPU scheduling for multi-DNN real-time inference. In IEEE RTSS.
[58] Wencong Xiao, Romil Bhardwaj, Ramachandran Ramjee, Muthian Si-
vathanu, Nipun Kwatra, Zhenhua Han, Pratyush Patel, Xuan Peng,
Hanyu Zhao, Quanlu Zhang, Fan Yang, and Lidong Zhou. 2018. Gan-
diva: Introspective cluster scheduling for deep learning. In USENIX
OSDI.
[59] Wencong Xiao, Shiru Ren, Yong Li, Yang Zhang, Pengyang Hou, Zhi
Li, Yihui Feng, Wei Lin, and Yangqing Jia. 2020. AntMan: Dynamic
scaling on GPU clusters for deep learning. In USENIX OSDI.
[60] Peifeng Yu and Mosharaf Chowdhury. 2019.
Salus: Fine-
grained GPU sharing primitives for deep learning applications.
arXiv:arXiv:1902.04610.
[61] Quanlu Zhang, Zhenhua Han, Fan Yang, Yuge Zhang, Zhe Liu, Mao
Yang, and Lidong Zhou. 2020. Retiarii: A deep learning exploratory-
training framework. In USENIX OSDI.
[62] Wenyi Zhao, Quan Chen, Hao Lin, Jianfeng Zhang, Jingwen Leng,
Chao Li, Wenli Zheng, Li Li, and Minyi Guo. 2019. Themis: Predicting
and Reining in Application-level Slowdown on Spatial Multitasking
GPUs. In IEEE IPDPS.
[63] Yihao Zhao, Yuanqiang Liu, Yanghua Peng, Yibo Zhu, Xuanzhe Liu, and
Xin Jin. 2022. Multi-resource interleaving for deep learning training.
In ACM SIGCOMM.
15
