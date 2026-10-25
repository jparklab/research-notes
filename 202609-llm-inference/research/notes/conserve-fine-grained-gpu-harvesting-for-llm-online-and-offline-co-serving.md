---
title: 'ConServe: Fine-Grained GPU Harvesting for LLM Online and Offline Co-Serving'
id: conserve-fine-grained-gpu-harvesting-for-llm-online-and-offline-co-serving
tags:
- onprem-llm-inference-stacks-859099
- overprovisioning
- gpu-utilization
created: '2026-10-23T17:21:30.651973Z'
updated: '2026-10-23T17:22:55.088501Z'
source: https://arxiv.org/pdf/2410.01228
source_domain: arxiv.org
fetched_at: '2026-10-23T17:21:30.651680Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: institutional
content_type: paper
deprecated: false
summary: 'ConServe (Berkeley/UCLA, 2024): co-serves online LLM requests and offline
  batch via token-level latency-aware scheduler, layer-wise preemption, incremental
  KV cache. 2.2x higher throughput, 2.9x lower online tail latency vs SOTA. Motivating
  measurement: Llama-3.1 8B on one H100 peaks at ~5000 tok/s but at the average load
  (~2000 tok/s) over 70% of GPU resources are idle; peak load is multiple times average,
  so static reservation (including on-prem/reserved GPUs) forces overprovisioning;
  load can ramp 3x within minutes.'
raw_file: raw/conserve-fine-grained-gpu-harvesting-for-llm-online-and-offline-co-serving.pdf
doi: arXiv:2410.01228
utility_score: 16.0
citation_count: 12
is_retracted: false
---

ConServe: Fine-Grained GPU Harvesting for LLM Online and Offline Co-Serving
Yifan Qiao† Shu Anzai‡ Shan Yu‡ Haoran Ma‡ Shuo Yang† Yang Wang♯Miryung Kim‡
Yongji Wu† Yang Zhou†♣Jiarong Xing†⋄Joseph E. Gonzalez† Ion Stoica† Harry Xu‡
†UC Berkeley ‡UCLA ⋄Rice University ♣UC Davis ♯Intel
Abstract
Large language model (LLM) serving demands low latency
and high throughput, but high load variability makes it chal-
lenging to achieve high GPU utilization. In this paper, we
identify a synergetic but overlooked opportunity to co-serve
latency-critical online requests alongside latency-tolerant of-
fline tasks such as model benchmarking. While promising,
existing serving systems fail to co-serve them efficiently, as
their coarse-grained resource management at the request or it-
eration level cannot harvest millisecond-level GPU idle cycles
without introducing interference that violates online latency
objectives.
ConServe is a new LLM co-serving system that achieves
high throughput and strong online latency guarantees by man-
aging resources at finer granularities. ConServe introduces
three techniques: (1) a latency-aware token-level scheduler
that precisely sizes offline batches and tokens to fit within
online latency objectives; (2) sub-iteration, layer-wise pre-
emption that allows offline tasks to yield to online load spikes;
and (3) incremental KV cache management that enables pre-
empting and resuming offline requests at near-zero cost. Eval-
uations with Llama-3.1 and Qwen-2.5 models on real-world
workloads show that ConServe delivers an average of 2.2×
higher throughput and reduces online serving tail latency by
2.9× on average compared to state-of-the-art systems.
1
Introduction
Large language models (LLMs) are reshaping a broad range
of applications such as chatbots [7, 46, 59], coding assis-
tants [15,24,55], and document summarization [17,35]. To
meet rising demand, many providers [4,45] now offer LLM
online inference services, accessible through their APIs or
web UI. However, hosting LLMs entails substantial infrastruc-
ture costs. Serving millions of user requests [23,36] demands
running numerous model replicas across thousands or more
high-performance GPUs. For instance, DeepSeek deploys
~3000 H800 GPUs to serve its R1 model [10].
To make effective use of these costly infrastructures, LLM
service providers always strive to offer low-latency services
while maximizing their GPU utilization. However, in prac-
tice, the utilization of inference clusters is often unexpect-
edly low due to the bursty and unpredictable nature of online
LLM workloads. These workloads can vary sharply, not
just over hours but also within seconds. For example, a re-
cent study [62] found that load of LLM services can triple
within a few seconds. Such rapid fluctuations are difficult
for auto-scaling to handle, so today’s providers often reserve
long-term GPU instances or deploy on-premise clusters pro-
visioned for peak load [64]. This, in turn, leads to substantial
GPU underutilization during low-traffic periods.
A common practice in the CPU world to mitigate re-
source waste is to backfill leftover cycles with best-effort
jobs [26,49,52]. Similar ideas are being explored for GPUs,
such as harvesting leftover inference cycles for training or
fine-tuning jobs [31, 42, 65]. The central challenge in such
resource harvesting lies in ensuring that original workloads
remain unaffected. In GPU harvesting, this requires that
whenever online inference demands GPU resources, they can
be reclaimed and reallocated seamlessly. However, prior ap-
proaches have focused on colocated tasks such as training or
fine-tuning, which typically run as independent processes in
separate containers or virtual machines [14,68]. Repartition-
ing GPU resources between these jobs and inference tasks is
heavyweight, often incurring task-switching overheads that
take seconds to minutes to complete.
To make GPU harvesting practical, it is critical to find best-
effort jobs suitable for co-locating. In this work, we argue
that a more synergistic yet overlooked opportunity lies in co-
serving online, latency-sensitive requests with offline, latency-
tolerant batch inference. This option is increasingly viable as
providers like OpenAI [44] and Anthropic [3] now offer batch
APIs with a 24-hour turnaround at a lower price for tasks
such as data analytics [30] and model testing/evaluation [29].
Offline batch inference offers two key advantages for GPU
harvesting: (1) offline tasks share the same model and can be
co-scheduled with online ones in the same inference engine,
1
arXiv:2410.01228v2  [cs.DC]  3 Sep 2025


---

enabling seamless reuse of the loaded model without extra
provisioning and costly process switching; and (2) offline
requests can be freely preempted within the engine for fast
accommodation of sudden online traffic spikes.
However, simply colocating online and offline requests
is insufficient due to the well-known tension between low
latency and high efficiency [6,26,49]. Existing serving sys-
tems manage requests and their states at a coarse granularity
that fails to mitigate interference and meet online workload’s
tight SLOs. Specifically, our profiling reveals three critical
problems spanning the entire serving lifecycle: scheduling,
execution, and KV cache state management.
• Static, chunk-level scheduling creates unpredictable latency.
For online serving on overprovisioned clusters, schedulers
such as continuous batching [67] are designed to maxi-
mize GPU throughput. They greedily enlarge batches as
requests arrive, implicitly deprioritizing tail latency. Chun-
ked prefill [1] partially regulates latency by enforcing a
static compute budget (i.e., a fixed-size chunk), but still fills
that budget rather than actively controlling latency. Under
co-serving, where resources are intentionally saturated and
every batch can be filled, this static, chunk-level approach
becomes untenable. The scheduler must instead track the
performance impact of admitting each request and token
and dynamically adjust the token budget to guarantee online
SLOs.
• Iteration-level preemption causes slow responsiveness. To
handle a sudden spike in online traffic, offline work must
be preempted immediately. However, to maximize the
GPU efficiency, current serving engines decouple CPU
and GPU execution: the scheduler forwards a batch to a
dedicated GPU executor, which then runs the entire iteration
uninterruptibly as a single executable or a CUDA Graph. As
a result, the scheduler can only regain control and preempt
after the current iteration completes. For example, our
profiling shows that a large offline batch can introduce
970ms of delay for a Llama-3.1 8B model and stall online
serving response.
• Request-level state management incurs prohibitive over-
head. Since an LLM needs the entire past context to gen-
erate a new token, the request serves as the basic unit for
execution and state management. However, while this gran-
ularity simplifies scheduling, it is too coarse-grained for
KV cache management, which can grow to gigabytes of
state per request. As a result, preempting a request forces a
costly choice: either discard the entire KV cache and pay
for expensive recomputation later, or stall the GPU to swap
it back and forth to host memory, leading to a collapse in
throughput.
As a result, when co-serving LLM requests, existing sys-
tems suffer from a 3× online latency increase and 4× offline
throughput drop (§3).
Insight. This paper addresses a central question: Can idle
GPU cycles and memory be harvested during online LLM
serving while achieving both high throughput and strict SLO
guarantees? Our key insight is that resolving the tension
between low latency and high efficiency requires a co-serving
system to manage resources and orchestrate execution at a
granularity finer than requests or iterations. By operating
at this finer scale, the system can exploit scheduling and
preemption opportunities at the millisecond level, enabling
both efficient utilization and strong SLO adherence.
Our Approach. To realize this insight, we introduce Con-
Serve, the first LLM co-serving system that harvests idle
resources with the responsiveness to adhere to strong online
SLOs. ConServe orchestrates co-serving at the sub-request
and sub-iteration level through three core techniques.
• SLO-aware token-level scheduling (§4.2). To fix the is-
sue of unpredictable latency caused by static, chunk-level
scheduling, ConServe introduces a dynamic, fine-grained
scheduler. It uses an offline profiler to build a precise cost
model that captures how batch latency is affected by both
compute pressure (active tokens) and memory pressure (cu-
mulative KV cache). At runtime, the scheduler queries this
model to dynamically adjust the number of offline tokens it
adds to a batch, ensuring it precisely fits within the available
SLO slack while maximizing throughput.
• Layer-wise preemption (§4.3). To break the iteration-
level boundary that causes slow responsiveness, ConServe
enables preemption within a single forward pass. By in-
strumenting models with safe preemption points between
transformer layers, ConServe allows offline requests to
exit mid-iteration upon receiving a preemption signal from
the scheduler. This reduces preemption latency from the
hundreds of milliseconds of a full iteration to the single-
digit milliseconds of one or several layers, enabling near-
instantaneous reaction to traffic spikes.
• Incremental KV cache management (§4.4). To elimi-
nate the high cost of recomputing or swapping KV caches
upon preemption, ConServe leverages a key observation
that the KV cache is append-only and immutable. Specif-
ically, ConServe decouples a request from its full state
history by managing the state at the token level. Instead
of swapping the entire KV cache of a request, ConServe
asynchronously checkpoints only the state of the single,
newly-generated token after each iteration per request, elim-
inating overhead that would otherwise be proportional to
the sequence’s length. This incremental approach, com-
bined with background prefetching for restoration, makes
frequent, millisecond-scale preemption practical at near-
zero cost and allows GPU memory to be safely saturated
without risking performance.
Results. We evaluated ConServe with Llama-3.1 8B [18],
Qwen-2.5 14B [66], and Llama-3.1 70B [18] across four real-
2


---

world datasets and a range of synthetic workloads. Results
show that ConServe significantly improves GPU utilization
while preserving low online latency and high offline through-
put. Compared to state-of-the-art serving systems with pre-
emptive scheduling, ConServe provides strong performance
isolation for online inference and reduces P99 latency by an
average of 2.9×, while improving the offline throughput by
an average of 2.2×.
2
Background
2.1
Large Language Model Inference
Modern LLMs process a sequence of input tokens (i.e.,
prompt) and generate output tokens through two sequential
phases: prefill and decode. An LLM inference starts with
the prefill phase, where the model processes the entire in-
put sequence to generate the first output token. An LLM
can process all input tokens in parallel, making the prefill
phase compute-bound. Following that, the decode phase gen-
erates subsequent tokens in an auto-regressive manner. For
each output token, its generation requires the computation of
a series of key and value vectors, which remain unchanged
throughout the inference process. Therefore, the inference
engine [37, 39, 48] caches them in GPU memory (i.e., KV
cache) to avoid recomputation in each decode step. Due to
the auto-regressive generation process and the design of the
KV cache, the decode phase is less compute-intensive and
typically bounded by the GPU memory bandwidth due to the
need to load model weights.
To improve the compute intensity and hardware efficiency
during LLM inference, a common practice is to batch multiple
requests in a single inference run to reuse the model weights
for computation and memory load [27,67].
2.2
Online and Offline LLM Serving
Broadly, LLM serving can be categorized into two types:
online serving, which generates real-time responses to user
inputs, and offline serving, which processes inputs in batches
and responds asynchronously.
Online serving is primarily designed for latency-critical
requests and is the de facto option offered by existing API
providers [4,45]. Unlike traditional cloud services, where pro-
cessing times are often predictable, an LLM request generates
a sequence of tokens with variable lengths. As a result, LLM
serving latency is measured on a per-token basis. Specifically,
an LLM’s serving latency is defined by two key metrics: time
to first token (TTFT), which is the duration of the prefill
phase, and time between tokens (TBT), which is the execution
time of each step in the decode phase1. To provide a low
1Note that TBT is measured per token and is stricter than time per output
token (TPOT), which averages the TBT across all tokens per request.
0
3
6
9
12
15
18
21
24
Time (hours)
0
2000
4000
Request Rate
(tokens/second)
(a) Load variation over 24 hours.
0
5
10
15
Time (minutes)
0
2000
4000
6000
Request Rate
(tokens/second)
(b) Load variation over 15 minutes.
Figure 1: User traffic to ChatGPT within a campus exposes high
load variability at various time scales.
end-to-end response latency and smooth user experience, it is
critical for an online LLM serving system to optimize both
TTFT and TBT under stringent SLOs. For example, an online
chatbot may set a 99th-percentile (P99) TTFT SLO of 1.5
seconds and a P99 TBT SLO of 100 milliseconds to respond
faster than a typical human can read [33,53].
Offline serving offers a cost-efficient solution for latency-
tolerant, best-effort requests. Users typically submit offline
requests in batches, which are processed by the serving sys-
tem in a best-effort manner to maximize hardware efficiency.
Unlike online serving which emphasizes low latency, offline
serving often involves processing large corpora or datasets,
where the first-order performance metric is throughput that
measures how many tokens are generated per second. Typ-
ical scenarios for offline serving include document summa-
rization [25], data wrangling [34], data analytics [30], LLM
benchmarking and evaluation [29], among other emerging
applications.
3
Motivation
In this section, we first demonstrate how real-world LLM
load can vary over time and why existing serving systems
fall short of achieving high GPU utilization. We then use an
experiment to quantitatively demonstrate the limitations of
simply co-locating online and offline serving with preemptive
scheduling.
3.1
GPU Underutilization in LLM Serving
Online Load Burstiness. Real-world LLM workloads often
expose diurnal patterns and high load variability, as shown
by reports from LLM service providers [32,51] and a recent
study [62] which collects ChatGPT user traffic for two months.
Figure 1(a) presents the sampled request rate within a day.
3


---

0
1000
2000
3000
4000
5000
6000
Offered Load (tokens/sec)
0
25
50
75
100
GPU Utilization (%)
GPU Utilization
P99 TTFT
10−1
100
101
102
P99 TTFT (s)
Figure 2: GPU utilization and the P99 TTFT across request rates
measured with Llama-3.1 8B on an H100 GPU. Utilization is defined
as the offered load relative to the GPU’s maximum achievable load.
Despite the average load being moderate, there is a clear
contrast between peak hours in the afternoon and non-peak
hours in the morning. Beyond diurnal patterns, this load
variance also extends to shorter timescales, and the request
rate can ramp up multiple times within minutes. Figure 1(b)
zooms in on a 10-minute window to analyze one such burst.
As shown, the load still fluctuates drastically at the minute
timescale, and the request rate increases by 3× at time t =
10min. Traces from other LLM service providers, such as
Azure, also expose similar high load variability [51].
Static Reservation Underutilizes GPUs.
LLM service
providers often use long-term reserved or on-premise GPU
instances, which both mitigate GPU unavailability and offer
greater cost efficiency than on-demand instances [64]. Due
to such high load variability, online serving must reserve suf-
ficient resources for the peak demand to avoid violating its
stringent latency SLOs. However, since the peak inference
load can be multiple times higher than the average load, over-
provisioning can lead to significant resource waste. Figure 2
illustrates the P99 TTFT and GPU utilization under different
request rates for a Llama-3.1 8B model running on an H100
GPU. As shown, the model achieves a good balance between
high GPU utilization and acceptable tail TTFT at a load of
5000 tokens/s, which is roughly the peak load in Figure 1b.
However, under a moderate load of 2000 tokens/s (average
load in Figure 1b), GPU utilization drops sharply, and over
70% of resources are left idle.
3.2
Challenges in LLM Co-Serving
Co-serving online and offline workloads on the same LLM
serving instance is a promising way to improve GPU utiliza-
tion. However, co-serving faces a fundamental challenge in
balancing the low-latency requirements of online inference
with the high-throughput demands of offline inference.
To quantify this trade-off, we extended a state-of-the-art
serving engine vLLM [61] with a priority-based scheduler
and preemption support (details in §6.1). At a high level, the
scheduler builds on chunked-prefill scheduling [1] by adding
additional offline tokens when online requests are unable to
fill up the chunk. Upon receiving online requests, our preemp-
tive scheduler, denoted as Preemptive, pauses running offline
requests. It keeps their KV cache in GPU memory as long as
14000
15000
14916
P99 Online TTFT (ms)
Online-Only
Preemptive
Non-Preemptive
0
1000 375
1346
Online-Only
Preemptive
Non-Preemptive
100
200
300
400
500
129
388
408
P99 Online TBT (ms)
Preemptive
Non-Preemptive
0
1
2
3
4
Offline Tput (KTokens/s)
Figure 3: P99 online latencies and offline throughput when co-
serving online and offline requests with preemptive or non-premptive
scheduling with Llama-3.1 8B on one H100 GPU.
space permits, deferring eviction and avoiding recomputation
until memory pressure necessitates it. Meanwhile, to estimate
the upper bound of achievable offline throughput, we also
introduce a non-preemptive variant, Non-Preemptive, which
still schedules offline requests at a low priority but allows
scheduled requests to run to completion.
We compared these two policies against Online-Only,
which serves only online inference and represents optimal
online latency, on an H100 GPU serving Llama-3.1 8B. We
used a synthetic online workload (Gamma process, CV=0.5,
2 req/s) alongside sufficient offline requests. Each request has
4096 input tokens and 256 output tokens (see §6.4 for other
request lengths). Figure 3 reports the P99 online TTFT, TBT,
and offline throughput. As shown, simple preemptive schedul-
ing can achieve neither low online latency nor high offline
throughput. Using the preemptive scheduler increases P99
online TTFT by 3.59× and TBT by 3.01×. Meanwhile, the
preemptive scheduler reduces offline throughput by 4.27×
compared to non-preemptive scheduling. Our analysis re-
vealed three root causes:
• Coarse-grained scheduling leads to contention: With
chunked-prefill, the scheduler operates at the chunk granu-
larity. To maximize utilization, it greedily fills each chunk
with offline tokens but ignores the potential compute and
memory contention they introduce. This directly slows
down the decoding of concurrent online tokens and inflates
online TBT.
• Iteration-level preemption causes queueing delays: Pre-
emption occurs only after a full iteration finishes, meaning
incoming online requests must wait for the entire in-flight
chunk to complete. This coarse granularity creates head-
of-line blocking that can last hundreds of milliseconds,
severely degrading TTFT.
• Request-level KV cache management incurs high over-
head: When preemption occurs, state is managed at the
entire sequence’s KV cache granularity. This forces a
costly dilemma: either recompute the entire KV cache later
or stall the GPU to swap it to host memory. Our profiling
shows that under memory pressure, up to 69% of GPU time
is wasted on recomputation or swapping [27].
Takeaway. These challenges necessitate a system featuring
fine-grained resource management: (1) Adaptive scheduling
4


---

Worker 1
(§4.3)
Real-Time Streaming API
Batch Process API
Prompt
Online API Server
Offline Request Pool
SLO-Aware 
Token-Level 
Scheduling (§4.2)
Host Memory
Incremental KV Cache 
Management (§4.4)
Checkpointed 
KV Cache Blocks
Interconnection
(PCIe, NVLink-C2C, etc.)
Worker N
Layer-Wise 
Preemption (§4.3)
(upon online load bursts)
latency objective
Figure 4: Design overview of ConServe.
that bounds latency impact of co-served offline work within
online SLOs; (2) sub-iteration preemption to mitigate head-
of-line blocking; and (3) sub-request KV cache management
for efficient context switching.
4
Design
4.1
ConServe Overview
As shown in Figure 4, ConServe is an LLM serving system
that co-serves online and offline requests to maximize uti-
lization while preserving online SLOs and offline progress.
The key to its efficiency is breaking the coarse-grained limi-
tations of existing systems and utilizing resources at a much
finer granularity across three core components of the serv-
ing process: scheduling, model execution, and KV cache
management.
• SLO-aware token-level scheduling (§4.2). ConServe’s
scheduler admits requests at the token granularity. Instead
of using a static, fixed-size chunk, the scheduler employs
a context-aware performance model to calculate a precise
token budget for every single iteration. This allows it to
safely fill offline tokens for maximized utilization subject
to SLO and adapt in real-time to workload dynamics.
• Layer-wise preemption (§4.3). To provide immediate
responsiveness to online traffic, ConServe’s execution en-
gine supports sub-iteration preemption. By instrumenting
the model, it creates preemption points between model
layers, allowing the scheduler to interrupt offline work in
single-digit milliseconds. This breaks the atomicity of the
iteration, enabling the system to react to SLO threats at
the layer level rather than waiting for a full, potentially
long-running, forward pass to complete.
• Incremental, token-level state management (§4.4). To
make fine-grained preemption practical, ConServe man-
ages the KV cache with incremental, token-level check-
pointing. Leveraging the insight that the cache is append-
only, it asynchronously saves only the state for the single,
newly-generated token after each iteration. This decouples
the cost of preemption from a request’s history, making the
overhead constant and negligible, and resolving the costly
swap-vs-recompute dilemma of monolithic, sequence-level
state management.
4.2
SLO-Aware Token-Level Scheduling
In this section, we first study why existing schedulers with
static budgets are “unsafe” for co-serving, i.e., they can fre-
quently cause online SLO violations. We then introduce
our context-aware performance model and present a dynamic,
token-level scheduling algorithm that uses this model to safely
maximize GPU utilization while adhering to strict online
SLOs.
Limitations of static chunk-level scheduling. Chunked
prefill [1] stands for the state-of-the-art scheduler for online
serving and has been enabled by default in modern serving
engines [27, 71]. It splits a long prefill into smaller chunks
and schedules them alongside decode phases across iterations,
which reduces head-of-line blocking from long prefill and
stabilizes TBTs. The algorithm assumes that a chunk of k
tokens takes roughly constant time to execute and uses a fixed
chunk size as the budget for the number of tokens to compute.
While this assumption may suffice in online serving when
GPUs are rarely saturated, it is unsafe for co-serving when
GPUs run at a high utilization and are sensitive to latency.
This is because it ignores the performance impact of the con-
text and KV caches being loaded for all active requests and
can lead to inaccurate latency estimations. For instance, pre-
filling a 2048-token chunk for Llama-3.1 8B on an H100
takes 51ms for a new request but increases to 124ms when the
request already has a 40K token context. Given a 100ms TBT
SLO, a static chunking policy treats both scenarios identically
and makes the same scheduling decision, leading to an SLO
violation in the latter case. This demonstrates that no static
chunk size can simultaneously achieve both high efficiency
and strict latency guarantees across varying request contexts.
Context-aware performance model. To accurately predict
the latency of any potential batch, we first build a context-
aware performance model to analyze the performance of au-
toregressive transformer-based LLMs. We model the latency
as a function of two key variables: the number of compute
tokens (P) being actively processed, and the number of pre-
existing context tokens (C) held in the KV cache.
The latency for a given iteration is approximated by the
following polynomial:
5


---

Latency(P,C) =
k1P
|{z}
Linear Compute
+
k2P(P+C)
|
{z
}
Attention
+
k3P
|{z}
Communication
+ k4(P+C)
|
{z
}
Memory Access
+
k5
|{z}
Constant
Each term in the model captures a distinct aspect of the
transformer architecture’s performance:
1. Linear compute (k1P): This term covers per-token op-
erations like the QKV and MLP projections, whose cost
scales linearly with the number of compute tokens.
2. Attention (k2P(P+C)): This is the most critical term for
context-awareness. It models the quadratic complexity of
attention, which depends on both the new tokens being
processed (P) and their interaction with all existing context
tokens (C). This term often dominates latency in requests
with long contexts.
3. Communication (k3P): This captures the overhead of
collective communication operations (e.g., all reduce) in
tensor parallel configurations, which scales with P being
communicated.
4. Memory Access (k4(P +C)): This models the cost of
memory-bound operations, primarily loading the KV
cache from HBM for the attention computation.
5. Constant (k5): This accounts for fixed overheads in each
iteration, such as loading model weights and other small,
constant-time kernels.
ConServe learns the conefficients (k1 to k5) via a one-time,
offline profiler. For a given model and hardware setup, the
profiler empirically measures iteration latency across a grid
of P and C values and uses polynomial regression to fit the
model. In practice, the profiling process is fast and accurate.
It finishes in 20 minutes even for 70B models with a relative
prediction error below 4%.
The profiler offers a can_schedule(batch,req,TBT) inter-
face that informs the scheduler if a new request can be added
into the batch while adhering to TBT SLO. It returns the
number of compute tokens that can be scheduled from req
or 0, which means req cannot be put into the batch with the
current TBT objective.
Latency-aware token-level scheduling. Armed with the pre-
dictive model, the ConServe scheduler executes a dynamic,
token-level algorithm at every iteration to compose the opti-
mal batch2. At a high level, the scheduler prioritizes online
requests to avoid introducing any queuing delay. After ac-
commodating all pending online requests, it iterates through
offline requests and leverages the profiler to decide how many
offline requests and tokens can be added into the batch given
the online TBT SLO.
2This paper uses “batch” and “chunk” interchangeably; in chunked prefill,
a batch refers to a chunk of compute tokens from scheduled requests.
Algorithm 1: Preventing online TTFT violations.
1 Let Qon denote incoming online requests, and B denote the
current batch.
Input: (1) Online TTFT objective tTTFT .
▷Run in the monitor thread.
2 Function ONRECVONLINEREQUEST(tTTFT ):
3
if no offline requests in the current batch then
4
return
5
ton ←Profiler.estimate_exec_time(Qon)
6
tremaining ←Profiler.estimate_exec_time(B) - tpassed
7
if tremaining +ton > tTTFT then
8
Worker.signal_layerwise_preemption()
In addition to latency, the scheduler must also manage the
finite capacity of the GPU’s KV cache. A unique challenge
in co-serving is that idle offline requests, which are paused to
meet online SLOs, still consume memory and block incoming
online requests. To resolve this, ConServe releases their
KV caches on demand to incorporate online requests. We
will discuss how ConServe manages and restores KV caches
efficiently in §4.4.
Finally, during periods of no online traffic, the scheduler
enters an offline-optimized mode. It uses the performance
model to determine the optimal batch size that maximizes
throughput until the next online request arrives.
4.3
Layer-Wise Preemption
Real-world workloads often see sudden online load changes
(§3.1). However, a single iteration of a large offline batch can
occupy the GPU for up to seconds, blocking the execution
of incoming online requests and potentially causing online
TTFT violations. To address this, online requests must be
able to preempt offline requests at a sub-iteration level.
However, this is challenging because, for efficiency, infer-
ence engines dispatch a batch to dedicated GPU workers to
execute an iteration uninterruptibly as a single executable or
a CUDA Graph and return control only when the iteration
finishes. Intra-iteration interruption can leave inconsistent
states and potentially crash the engine.
ConServe breaks this iteration-level barrier by instrument-
ing the model at the Transformer layer granularity, which
brings two key advantages. First, modern LLMs typically
comprise dozens of layers where each layer runs for a few
milliseconds, ensuring high responsiveness. Second, layers
represent natural computation boundaries with clear input and
output dependencies, which allows offline requests to pause
and exit early with simple cleanup.
ConServe codesigns the scheduler and workers to realize
layer-wise preemption. The scheduler is responsible for de-
tecting potential latency violations. It spawns a dedicated
monitor thread, which is triggered upon receiving a new on-
line request. Algorithm 1 outlines its logic. It uses the per-
6


---

formance model and profiler (§4.2) to estimate if waiting for
the current batch to finish would cause the new request to
miss its TTFT SLO. If a violation is predicted, the scheduler
immediately signals for preemption by writing to a designated
flag in shared host memory.
ConServe workers instrument the model with lightweight
synchronization points (safepoints) between layers, inspired
by language runtime designs [47]. At each safepoint, the
worker checks the preemption flag with a single device-to-
host memory load. If the flag is set, the worker will terminate
computation for all offline requests in the batch, clean up
their hidden states and attention metadata, and then resume
execution for the remainder of the iteration with only the
online requests.
Tensor parallelism. One additional challenge in implement-
ing the safepoint with tensor parallelism (TP) is that collec-
tive communication (e.g., all-reduce) in TP requires strict
synchronization across workers. Arbitrary preemption of a
subset of workers can cause communication deadlocks [50].
To address this, ConServe will synchronize all workers with
ncclBroadcast at the beginning of the safepoint before check-
ing for preemption signals.
CUDA graph compatibility. Our safepoint mechanism is
fully compatible with CUDA Graphs. First, modern engines
such as vLLM capture each model layer as a separate graph,
where we can insert the safepoint between graph launches.
For systems that capture the model as a single graph, safepoint
can be implemented as a conditional CUDA graph node [41]
within the graph.
Overhead analysis. We have carefully implemented safe-
point to minimize its cost. Each safepoint check consists of
a single read from pinned host memory (7µs) and a single
broadcast/barrier operation across GPUs (13µs on NVLink).
This total cost of 21µs is negligible compared to the several
milliseconds required to execute a transformer layer. More
details are presented in §6.5.
4.4
Incremental KV Cache Management
Effective preemption requires not only interrupting computa-
tion but also rapidly reclaiming GPU memory. While existing
systems, including SGLang [71] and vLLM [27], allocate
KV cache memory in fine-grained chunks (tokens [71] or
pages [27]), they evict them at request granularity during
preemption: the KV history of a preempted request is treated
as a single, indivisible unit. Consequently, the entire KV of
that request is evicted at once.
This coarse-grained “fate-sharing” forces a costly tradeoff
between reclamation latency and restoration cost. Figure 5
demonstrates this with two strawman eviction approaches:
(a) recomputation and (b) swap. Recomputation immediately
discards the KV cache but wastes GPU cycles to recompute
the offline job from scratch later. Swapping preserves KV
IC
Time
Offline Prefill
D1
D2
D3
Swap out
Swap in
Online
Recompute
Time
Offline Prefill
D1
D2
D3
Online
IC
Time
Offline Prefill
D1
D2
D3
Online
IC
Preemption
Swap in
Blocking
(b) Resume by Swapping
Wasted compute
Resume
Online serving (prefill/decode)
Offline prefill phase
Di Offline decode phase (i-th token)
IC Incremental checkpointing/prefetching
IC
IC
Time
Offline Prefill
D1
D2
D1’&3
Online
IC
Prefetch
Offline Prefill’
Idle
Idle
(c) Incremental Checkpointing  (IC)
(d) Incremental Checkpointing + Prefetching
(a) Resume by Recomputation
D1’&4
Offline Prefill’
D2’&4
IC
IC
IC
IC
Figure 5: Different preemption and resumption strategies. (a) Re-
suming by recomputation achieves low preemption delay at the cost
of additional computation. (b) Resuming by swapping avoids recom-
putation but blocks the schedule of incoming online requests. (c)
Incremental checkpointing (IC) minimizes both preemption delay
and resumption cost. (d) Incremental checkpointing + prefetching
overlaps swap-in with computation of the next batch.
cache states by writing them to host memory, but the blocking
I/O delays the scheduling of incoming online requests propor-
tionally to the number of tokens of the request, causing SLO
violations.
The fine-grained solution: decoupling lifecycles. Such inef-
ficiency stems from the tight coupling of a request’s lifecycle
and its KV caches. ConServe tackles this by managing the
KV cache state of each token individually. This fine-grained
design allows tokens and their KV caches of a preempted
request to be flexibly distributed across GPU, host memory,
or marked for recomputation.
Incremental checkpointing and prefetching. Enabled by
per-token state management, ConServe can incrementally
checkpoint and load KV cache of each token independently
with their parent request. Leveraging the immutability of KV
cache (i.e., append-only), ConServe proactively checkpoints
KV caches of only the newly generated tokens to host memory
after each generation step (Figure 5(c)). This amortizes the
I/O traffic across multiple iterations and avoids sudden bursts
of device-to-host data transfers, allowing I/O to fully overlap
with computation.
Unlike traditional swapping, incremental checkpointing
keeps KV caches in GPU memory rather than discarding them
directly, so ongoing offline requests can continue inference
until preemption occurs, at which point their KV caches will
be discarded on demand at the token level.
7


---

Symmetrically, when GPU memory is available for resum-
ing a preempted request, ConServe prefetches KV caches for
evicted tokens incrementally in the background. As shown in
Figure 5(d), prefetching I/O is overlapped with the computa-
tion and completely off the critical path.
Cost and feasibility analysis. The time overhead of incre-
mental KV checkpointing is negligible. On an H100 GPU
with 64GB/s device-to-host PCIe interconnect, data transfer
throughput for Qwen-2.5 14B (192KB KV cache per token)
achieves 64 × 10242/192 = 349Ktokens/s, which is much
higher than LLM generation throughput. Our measurement
shows that even transferring an entire chunk of 2048 tokens
takes only 10ms and can be effectively hidden by computation
overlapping (detailed evaluation in §6.5).
The host memory footprint is also strictly bounded by the
total physical GPU memory. In a typical setup where host
memory is overprovisioned (e.g., DGX server with 8 H100s,
640 GB of GPU memory, and 2 TB of host memory), this
is not a practical constraint. Even under extreme cases with
limited host memory, ConServe can gracefully degrade and
adapt to the available host memory size. When host memory
is exhausted, ConServe can evict individual tokens and fall
back to recomputation for only those specific tokens, which
is still more efficient than a coarse-grained, all-or-nothing
eviction policy.
5
Implementation
We have implemented ConServe atop vLLM 0.8.4 [27]. by
adding or modifying 9169 lines of Python and C++ code.
To support incremental KV cache checkpointing, ConServe
manages KV cache at a granularity of 16 tokens to align with
vLLM page size, and enhances vLLM’s KV cache manager
to track the mapping between each GPU KV page and its cor-
responding CPU KV page for checkpointing. This mapping
is recorded in an extended field of the virtual page table. To
support efficient KV cache transfer for tokens scattered in
GPU memory, we implement an efficient kernel that gathers
their KV cache into a contiguous buffer and transfers it once.
To ensure overlap between computation and asynchronous
swap I/O, ConServe creates a separate CUDA stream for KV
cache checkpointing operations. Original model executable
or CUDA Graphs are still launched in the default stream as
is. Since the checkpointed or prefetched KV cache blocks
have no data dependencies with the ongoing model computa-
tion, no synchronization is required between the two CUDA
streams.
ConServe includes a standalone offline profiler that can
be run independently with model configuration to generate a
performance profile, which is then loaded by the scheduler
at initialization. ConServe also comes with a built-in load
generator that can generate precisely timed requests following
the gamma distribution. The load generator can be configured
with various parameters including the request rate, bursti-
ness (i.e., skewness of the gamma distribution), and request
lengths to cover the characteristics of real-world workloads,
enabling users to better understand and optimize ConServe’s
performance.
6
Evaluation
Our evaluation aims to answer the following questions:
1. Can ConServe efficiently harvest GPU resources while
maintaining low online latency? (§6.2)
2. Can ConServe achieve both low online latency and high
offline throughput under real-world workloads? (§6.3)
3. Can ConServe maintain efficiency for workloads with dif-
ferent degrees of burstiness and request lengths? (§6.4)
4. How does each component contribute to ConServe’s effi-
ciency? (§6.5)
6.1
Setup
Environment and models. We conducted experiments on
a DGX server with two 48-core CPUs, 2TB of memory, and
eight NVIDIA H100 GPUs connected with 900GB/s fully-
meshed NVLink. The server runs Ubuntu 22.04 and CUDA
12.8. To reduce latency jitter, we disabled dynamic voltage
and frequency scaling (DVFS) of GPUs [38,58] and Python
garbage collector [54]. We have verified that these settings
do not negatively impact LLM inference performance.
We evaluated ConServe on three models of varying sizes,
including Llama-3.1 8B [18], Qwen-2.5 14B [66], and Llama-
3.1 70B [18]. We served each model with FP16 precision and
a minimum number of GPUs, which is 1 H100 for Llama-
3.1 8B and Qwen-2.5 14B, and 4 H100s for Llama-3.1 70B
with tensor parallelism. Among them, Llama-3.1 8B repre-
sents a resource-rich setup with enough GPU memory for KV
caches (16GB model weights, 60GB KV cache), Qwen-2.5
14B reflects a more constrained setup with 28GB of model
weights and 48GB allocated for KV cache, and Llama-3.1
70B demonstrates tensor parallelism across multiple GPUs.
Baselines. We compared ConServe with four baselines:
(1) Online-Only (Latency Optimal) refers to the original
vLLM configuration, which serves only online inference.
It provides optimal online serving latency but zero offline
throughput.
(2) Non-Preemptive (Throughput Optimal) is a variant policy
designed to maximize offline throughput. It prioritizes
online requests over offline ones, but schedules offline re-
quests at a lower priority when online inference does not
fully utilize the GPU, but does not preempt running of-
fline requests when new online requests arrive. While this
approach degrades online serving latency, it provides an
upper-bound estimate of offline throughput by allowing
offline requests to complete uninterrupted.
8


---

Task
Dataset
Input Tokens Output Tokens
Mean
P99 Mean
P99
Online chatting
BurstGPT [62]
2747
3446
267
481
Benchmarking
DuReader [21]
21776 29306
166
754
Question answer MultiNews [11]
6916 12084
394
788
Summarization
VCSUM [63]
18803 32768
337
510
Table 1: Real-world online and offline workloads used for evaluation.
(3) Sarathi-Preemptive (Sarathi-P) incorporates Sarathi’s [1]
chunked-prefill scheduling. To create a strong co-serving
baseline, we extend its scheduler to prioritize online re-
quests and then opportunistically fill offline tokens with
the remaining chunk space. The scheduler pauses offline
requests when the token budget is met, and evicts their
KV caches only under memory pressure.
(4) DistServe-Preemptive (DistServe-P) represents the disag-
gregated architecture [72], where prefill and decode oper-
ations are handled by separate sets of GPUs. We extend
its scheduler similar to (3) on both the prefill engine and
decode engine to prioritize online requests and preempt
offline requests under memory pressure. We carefully
tuned the ratio of prefill-to-decode GPUs (e.g., 2:1, 1:1,
1:2) and report the best-performing configuration.
We implemented or ported all baselines to vLLM 0.8.4 [27]
with chunked-prefill enabled by default for fair comparisons.
For all systems, configurations were carefully tuned for their
optimal performance.
Real-world workloads. For the online workload, we evalu-
ated BurstGPT [62], a representative trace of user requests to
GPT-4 [43] collected from a university campus.
For offline workloads, we evaluated three tasks: DuReader
[21] for model benchmarking, MultiNews [11] for multi-
document summarization, and VCSUM [63] for meeting sum-
marization. Table 1 summarizes all four evaluated workloads,
including their input and output length distributions.
Synthetic workloads. To evaluate ConServe’s ability to
balance efficiency with strict SLOs under diverse conditions,
we generate synthetic request arrivals using a Gamma process,
following methodologies established in prior trace studies [28,
56,62]. We vary key parameters, including request rate, SLO
tightness, request lengths, and traffic burstiness for overall
performance testing (§6.2) and sensitivity analysis (§6.4).
Metrics. Since online and offline serving have different per-
formance objectives, we evaluate them with distinct metrics.
For online serving, following prior work [1], we measure each
request’s P99 TTFT and TBT. For offline serving, we evaluate
throughput by measuring the number of tokens processed per
second, including both prefill and decode tokens.
6.2
GPU Efficiency and Latency
This subsection investigates whether ConServe can efficiently
harvest idle GPU resources—whenever the online request rate
is low or the latency SLO is sufficiently relaxed—for offline
throughput while still preserving online SLOs.
Setup. To enable a comprehensive evaluation, we gener-
ate synthetic online loads with varying request rates and la-
tency SLO scale. Following workload parameters from Burst-
GPT [62], we set the burstiness (measured as the coefficient
of variation, CV) to 0.5, with each request containing 4096
input tokens and 256 output tokens. A sensitivity analysis on
request lengths and burstiness is provided in §6.4.
We configured ConServe’s target SLOs based on the P99
latencies achieved by Online-Only.
Online load vs. performance trade-offs. Figure 6(a) shows
the P99 online TTFT, TBT, and offline throughput for all
systems as the online request rate increases across all models.
The results demonstrate that ConServe is the only system that
achieves both low online latency and high offline throughput.
Regarding online latency, ConServe maintains P99 TTFT
and TBT close to the optimal online latency baseline
(Online-Only) across all tested request rates, staying within
25% and 19% on average, respectively. In contrast, the other
baselines suffer from significant latency degradation due to
their coarse-grained designs. Non-Preemptive suffers from
high TTFT and TBT due to head-of-line blocking caused
by scheduled offline requests. Two preemptive baselines im-
prove TTFT but remain slow in TBT because they cannot
bound the compute and memory interference introduced by
offline tokens. Notably, Sarathi-P can sometimes yield worse
TBT than Non-Preemptive because preemption forces costly
recomputation of the entire prefill phase.
DistServe-P shows poor latencies because its disaggre-
gated architecture creates load imbalance between prefill and
decode instances which amplifies the contention from of-
fline requests. Decode instances quickly exhaust their GPU
memory and become the bottleneck due to excessive memory-
heavy decoding requests. This triggers frequent swapping
that not only stalls decoding but also applies back-pressure
to the prefill instances, ultimately degrading both TBT and
TTFT.
In terms of offline throughput, ConServe consistently out-
performs all other preemptive baselines and achieves an aver-
age of 82.3% of the optimal throughput. At low request rates,
where GPU memory is underutilized, both Sarathi-P and
ConServe maintain over 70% of optimal offline throughput,
as GPU memory is sufficient to keep offline requests’ KV
caches. However, as load increases, ConServe ’s advantage
becomes more pronounced. By eliminating the recomputa-
tion overhead, ConServe achieves an average of 3.0× (up
to 7.48×) higher offline throughput compared to Sarathi-P.
DistServe-P yields the lowest throughput in most cases due to
9


---

102
103
104
105
P99 Online
TTFT (ms)
Llama-3.1-8B
on 1 H100
Qwen-2.5-14B
on 1 H100
Llama-3.1-70B
 TP on 4 H100s
0
50
100
Online TTFT
Attainment (%)
Llama-3.1-8B
on 1 H100
Qwen-2.5-14B
on 1 H100
Llama-3.1-70B
 TP on 4 H100s
0
200
400
600
P99 Online
TBT (ms)
0
50
100
Online TBT
Attainment (%)
1
2
3
Req Rate (req/s)
0
50
100
Normalized
Ofline Tput (%)
0.5
1.0
1.5
Req Rate (req/s)
(a)
0.5
1.0
1.5
Req Rate (req/s)
1.0
1.5
2.0
SLO Scale (2 req/s)
0
50
100
Normalized
Ofline Tput (%)
1.0
1.5
2.0
SLO Scale (1 req/s)
(b)
1.0
1.5
2.0
SLO Scale (1 req/s)
Online-Only (Latency Optimal)
Non-Preemptive (Tput Optimal)
Sarathi-Preemptive
DistServe-Preemptive
ConServe
Figure 6: Overall serving performance for all three models on synthetic workloads with varying (a) request rates and (b) SLO scales. ConServe
achieves consistently low TTFT and TBT that are comparable with optimal online latency (measured with Online-Only) and high overall
serving throughput that are close to optimal offline throughput (measured with Non-Preemptive).
frequent preemptions on decode instances and back-pressure
on prefill instances.
Online SLO scale vs. performance trade-offs. Figure 6(b)
illustrates how system performance changes as we relax the
online SLO scale s, where SLOTTFT/TBT(s) = s·P99TTFT/TBT,
dictating the allowed latency increase over Online-Only.
Uniquely, ConServe can trade-off offline throughput for
stricter online SLO adherence, enabling >99% TTFT and
TBT attainment in all settings when s>1. Even in the most
challenging setting with no slack (s = 1.0), where minor sys-
tem jitter can cause violations, ConServe still maintains at
least 86% attainment, significantly exceeding all baselines.
This SLO awareness also enables ConServe to harness la-
tency slack for higher offline throughput until the s = 1.5,
where ConServe achieves an average 93% optimal through-
put. Further increasing s brings a diminishing 2% throughput
improvement.
In contrast, all other baselines are oblivious to the config-
ured SLO scale, and their offline throughput remains constant.
DistServe-P presents an interesting case. It can achieve high
TBT attainment thanks to the disaggregation. But the attain-
ment is still lower than ConServe in most cases because pre-
emptions still cause TBT jitter. Its TTFT attainment remains
poor due to back-pressure.
In summary, the results demonstrate that ConServe can
effectively co-serve online and offline inference and adapt to
different SLOs. It maintains near-optimal P99 online latency
while achieving high GPU utilization for offline throughput,
closely matching the optimal baseline. Compared to other
baselines, on average, ConServe reduces P99 online TTFT by
2.37× and TBT by 2.72×, respectively, while simultaneously
increasing offline throughput by 3.00×.
102
103
104
P99 Online
TTFT (ms)
0
200
400
600
P99 Online
TBT (ms)
0
200 400 600
Time (s)
(a) Online-Only
(Lat. Optimal).
0
2
4
6
Offline Tput
(KTokens/s)
0
200 400 600
Time (s)
(b) Non-Preempt.
(Tput. Optimal).
0
200 400 600
Time (s)
(c) Sarathi-
Preemptive.
0
200 400 600
Time (s)
(d) ConServe.
0
200 400 600
Time (s)
(e) DistServe-
Preemptive.
Figure 7: Serving performance for Llama-3.1 70B on real workloads.
ConServe is the online system that consistently meets online SLOs
(red lines) and achieves 78% of the ideal offline serving throughput
(measured with Non-Preemptive).
6.3
End-to-End Serving
with Real-World
Workloads
This subsection evaluates ConServe with real-world work-
loads and compares its end-to-end performance against the
baselines. We sampled a 10-minute trace from BurstGPT [62]
as the online workload. For the offline workload, we use
both a random mixture of three distinct offline datasets and
evaluate co-serving with each dataset individually.
Figure 7 presents the time-series results for Llama-3.1 70B.
The top two rows present P99 online latencies per 5 sec-
onds, with red lines indicating the respective TTFT and TBT
SLOs. Both Online-Only and ConServe are able to achieve
constantly low P99 TTFT and TBT and meet SLOs. In con-
trast, all three baselines exhibit highly unstable latency and
frequently violate TTFT and TBT SLOs.
10


---

102
103
104
P99 Online
TTFT (ms)
0
250
500
P99 Online
TBT (ms)
0
200 400 600
Time (s)
(a) Online-Only
(Lat. Optimal).
0
2
4
6
Offline Tput
(KTokens/s)
0
200 400 600
Time (s)
(b) Non-Preempt.
(Tput. Optimal).
0
200 400 600
Time (s)
(c) Sarathi-
Preemptive.
0
200 400 600
Time (s)
(d) ConServe.
0
200 400 600
Time (s)
(e) DistServe-
Preemptive.
Figure 8: Serving performance for Llama-3.1 8B on real workloads.
ConServe maintains its significant lead, achieving 88% of optimal
offline throughput while keeping online latencies low. Although
the less-saturated 8B setup is more favorable to DistServe-P’s
architecture, ConServe still outperforms it by 2.5× in throughput
and over 3× in latency.
Offline
P99 Online TTFT (s)
P99 Online TBT (ms)
Offline Tput (tok/s)
Workload
OO
NP
SP
DP Ours
OO
NP
SP
DP Ours OO
NP
SP DP Ours
Mixed
1.08
15.2 2.91 12.4 1.14
184
450 441 1398
201
0
1995
895 26 1553
DuReader
38.7 2.98 14.9 1.67
659 400 1607
204
1276
715 15 1187
MultiNews
11.0 2.11 11.3 1.04
305 212 1384
194
2765 1698 39 2281
VCSUM
46.2 3.01 15.1 1.68
566 453 1774
208
1218
255 15 1064
Table 2: Overall latency and throughput of Llama-3.1 70B when co-
serving real-world online and different offline workloads achieved
by Online-Only / Non-Preemptive / Sarathi-Preemptive /
DistServe-Preemptive / ConServe.
Regarding offline throughput, ConServe is able to achieve
an average of 1553 tokens/s, which is 78% of optimal through-
put measured with Non-Preemptive. Because real-world of-
fline datasets typically feature long sequences, Sarathi-P
triggers frequent, costly recomputations, resulting in a low
throughput of only 895 tokens/s. DistServe-P’s performance
is more nuanced. Under the heavy load of this workload, it
performs the worst, as the long sequences exacerbate the back-
pressure and preemption issues detailed in §6.2. However,
in less saturated setups, such as with the smaller Llama-3.1
8B model (Figure 8), its architectural imbalances are less
pronounced, allowing it to slightly outperform Sarathi-P.
Even in this more favorable scenario, DistServe-P remains
significantly worse than ConServe, suffering from 2.5× lower
throughput and over 3× higher online latency.
Finally, Table 2 summarizes results across all offline
datasets. Compared to the best-performing baseline, Con-
Serve reduces P99 online TTFT and TBT by an average of
2.04× and 2.86×, respectively, while improving the offline
throughput by 2.23× on average.
In summary, under realistic and dynamic workload condi-
tions, ConServe can maintain strict online SLOs while deliv-
ering close to optimal offline throughput.
6.4
Performance Sensitivity Analysis
In this section, we evaluate ConServe’s robustness to vary-
ing workload characteristics, specifically load burstiness and
request lengths. We conduct these experiments based on the
setup used for Figure 6(a), varying one parameter at a time
(burstiness, input length, or output length) while keeping other
conditions constant.
Load burstiness vs. performance. We use CV to represent
load burstiness, where higher CV indicates greater load vari-
ability. As Figure 9(a) shows, ConServe demonstrates strong
resilience to bursty loads with low online latency and high
offline throughput that are close to ideal values. Compared to
the best-performing baseline, ConServe reduces P99 TTFT
and TBT by an average of 1.68× and 2.38×, respectively,
and improves offline throughput by 3.62×.
Input/output lengths vs. performance. To maintain com-
parable load when varying sequence lengths, we adjust the
request rate inversely proportional to length relative to the
§6.2 setup (4096 input tokens, 256 output tokens, at 2 req/s).
Figure 9(b) and (c) show results for varying input and out-
put lengths. ConServe delivers good online latency that is
close to Online-Only across most settings. Regarding offline
throughput, ConServe consistently outperforms two preemp-
tive baselines. This advantage is most pronounced for typical
requests with input lengths between 2K and 16K tokens, and
output lengths longer than 128 tokens. We analyze the per-
formance at the extremes. (1) Requests with short inputs
(<2K tokens) cannot fully utilize KV cache capacity. This
allows preemptive baselines to keep paused offline requests
in GPU memory and avoid recomputation; (2) Requests with
long inputs (>16K tokens) or short outputs (<128 tokens) are
compute-intensive, leaving little idle capacity for offline serv-
ing. (3) Requests with long outputs (>512 tokens) are less
compute-intensive but consume more KV cache because they
stay on GPUs longer. This memory pressure limits the num-
ber of offline requests that can be served and slightly hinders
offline throughput.
In summary, ConServe can achieve robust performance
across varying degrees of burstiness and request lengths, and it
delivers consistently higher offline throughput than baselines,
especially for typical-length requests that closely align with
real-world workloads.
6.5
Design Drill-Down
Ablation study. We quantified the individual contribution
of each of our three core optimizations by incrementally en-
abling them atop the Sarathi-P baseline, using the Llama-3.1
8B setup from Figure 6(a). Compared to Sarathi-P, our
SLO-aware scheduler reduces P99 online TBT from 375ms
to 73ms. This shorter iteration time also makes the sched-
uler more responsive to new requests and reduces P99 online
TTFT from 1399ms to 686ms. Enabling layer-wise preemp-
11


---

103
105
P99 Online
TTFT (ms)
0
200
P99 Online
TBT (ms)
0.5
1
1.5
2
(a) CV
(2 req/s)
0
50
100
Normalized
Offline Tput (%)
1k
2k
4k
8k
16k
(b) Input Length
(output_len=256)
64 256
512
768
1k
(c) Output Length
(input_len=4096)
Online-Only (Latency Optimal)
Non-Preemptive (Tput Optimal)
Sarathi-Preemptive
DistServe-Preemptive
ConServe
Figure 9: Llama-3.1 8B serving performance under workloads with
varying CVs and input and output lengths.
tion eliminates TTFT jitter and further reduces P99 TTFT
from 686ms to 523ms and worst-case TTFT by 34% from
866ms to 573ms. Finally, adding incremental KV cache man-
agement further improves offline throughput by 1.29× from
8810 tokens/s to 11366 tokens/s.
Cost of incremental KV cache management. We measured
the overhead and PCIe utilization of our incremental KV
cache mechanism. For Llama-3.1 8B on a single H100 GPU,
our kernel can transfer KV caches at 37GB/s, and the largest
transfer finishes within 7ms and can be completed overlapped.
The kernel introduces a <500µs cost to gather KV caches
of scattered tokens, which is negligible compared to model
execution. It also scales well in a distributed setting. For
Llama-3.1 70B on 4 H100s, each TP worker checkpoints its
local KV cache shard concurrently, achieving an aggregated
bandwidth of 132GB/s.
Efficiency of layer-wise preemption. We measured the run-
time cost of the preemption safepoint and compared it to the
model execution time. On a single GPU, the instrumented
safepoint incurs negligible cost (<10µs). Even on four GPUs
with TP, each safepoint takes only 167.2µs on average. This is
slightly higher than raw instruction cost due to the minor lag
among TP workers, but still three orders of magnitude faster
than model execution, which takes 262ms on average. Con-
Serve instruments the model every 4 layers and introduces a
total latency increase of 3.0ms when no preemption occurs,
accounting for a 1.1% overhead compared to the model exe-
cution time. Meanwhile, ConServe maintains responsiveness
by detecting new requests and preempting the running batch
within 13ms, ensuring timely responses to online load bursts.
7
Related Work
LLM Serving Systems. Prior LLM serving systems mainly
focus on optimizing throughput and latency for a single type
of workload. To improve throughput, Orca [67] proposed
continuous batching, and vLLM [27] proposed PagedAtten-
tion. They addressed inefficiencies in scheduling and KV
cache management. To reduce latency, Sarathi [1] and split-
fuse [22] proposed chunked-prefill to split long prefills into
small chunks for better TBT. ConServe is built upon these
techniques and uniquely co-serves online and offline tasks
for maximized GPU utilization. DistServe [72] and Split-
wise [51] disaggregate prefill and decode computation to
model replicas for best TTFT and TBT, but they suffer from
low GPU utilization because of imbalanced computation load
between prefill and decode instances.
Generic Model Serving Systems. Before the rise of LLMs,
many systems target serving general ML models [5,8,9,16,19,
20,28,40,60]. Among them, Clipper [9] and Clockwork [19]
serve general neural networks by batching and scheduling
requests. REEF [20], SHEPHERD [69], and PipeSwitch [5]
multiplex a GPU for serving multiple small DNN models
using preemptive scheduling. AlpaServe [28], on the other
hand, leverages model parallelism for statistical multiplexing.
However, these systems overlook the huge model size and the
autoregressive nature of LLM inference, hence do not support
or only achieve suboptimal performance for LLM serving.
Offline LLM Serving. As offline inference has gained in-
creasing traction, many systems are specifically optimized
for offline LLM serving. DeepSpeed ZeRO-Inference [2] and
FlexGen [57] offload model weights and KV caches to host
memory to serve LLMs on small commodity GPUs. However,
due to the high latency of model swapping, these systems are
ill-suited for online serving. BlendServe [70] and PSM [30]
improve KV cache reuse and compute efficiency by reorder-
ing offline requests within a batch. ConServe is compatible
with them for further performance improvements.
Workload Colocation. Colocating latency-critical applica-
tions with batch applications is a widely adopted approach
to improve resource utilization in datacenters. Various CPU
schedulers [6, 13, 49] and operating systems [12, 52] have
been proposed to partition or reassign CPU cores between
latency-critical jobs and batch jobs for higher CPU utilization.
Compared to them, ConServe is the first to apply the principle
of workload co-location specifically for LLM serving.
8
Conclusion
We present ConServe, a new LLM co-serving system that
harvests idle GPU resources in online inference for offline
serving. It maintains low online serving latency with an SLO-
aware scheduler and a near real-time, layer-wise preemption
mechanism. It also achieves high offline serving through-
put by incremental KV cache management to avoid recom-
putation. Our evaluation shows that ConServe significantly
outperforms existing state-of-the-art systems.
12


---

References
[1] A. Agrawal, N. Kedia, A. Panwar, J. Mohan, N. Kwatra,
B. Gulavani, A. Tumanov, and R. Ramjee.
Taming
Throughput-Latency tradeoff in LLM inference with
Sarathi-Serve, July 2024.
[2] R. Y. Aminabadi, S. Rajbhandari, M. Zhang, A. A.
Awan, C. Li, D. Li, E. Zheng, J. Rasley, S. Smith,
O. Ruwase, and Y. He. Deepspeed inference: Enabling
efficient inference of transformer models at unprece-
dented scale, 2022.
[3] Anthropic.
Message
batches.
https :
/ / docs.anthropic.com / en / docs / build - with -
claude/message-batches, 2024.
[4] Anthropic.
Streaming messages.
https :
/ / docs.anthropic.com / en / api / messages -
streaming, 2024.
[5] Z. Bai, Z. Zhang, Y. Zhu, and X. Jin.
PipeSwitch:
Fast pipelined context switching for deep learning ap-
plications. In 14th USENIX Symposium on Operating
Systems Design and Implementation (OSDI 20), pages
499–514. USENIX Association, Nov. 2020.
[6] S. Chen, C. Delimitrou, and J. F. Martínez.
Parties:
Qos-aware resource partitioning for multiple interac-
tive services.
In Proceedings of the Twenty-Fourth
International Conference on Architectural Support for
Programming Languages and Operating Systems, AS-
PLOS ’19, page 107–120, New York, NY, USA, 2019.
Association for Computing Machinery.
[7] W.-L. Chiang, L. Zheng, Y. Sheng, A. N. Angelopoulos,
T. Li, D. Li, H. Zhang, B. Zhu, M. Jordan, J. E. Gonza-
lez, and I. Stoica. Chatbot arena: An open platform for
evaluating llms by human preference, 2024.
[8] D. Crankshaw, G.-E. Sela, X. Mo, C. Zumar, I. Stoica,
J. Gonzalez, and A. Tumanov. Inferline: latency-aware
provisioning and scaling for prediction serving pipelines.
In Proceedings of the 11th ACM Symposium on Cloud
Computing, SoCC ’20, page 477–491, New York, NY,
USA, 2020. Association for Computing Machinery.
[9] D. Crankshaw, X. Wang, G. Zhou, M. J. Franklin, J. E.
Gonzalez, and I. Stoica. Clipper: A Low-Latency on-
line prediction serving system. In 14th USENIX Sympo-
sium on Networked Systems Design and Implementation
(NSDI 17), pages 613–627, Boston, MA, Mar. 2017.
USENIX Association.
[10] DeepSeek AI.
DeepSeek-V3/R1 Inference System
Overview. https://shorturl.at/UzPvq, 2024.
[11] A. Fabbri, I. Li, T. She, S. Li, and D. Radev. Multi-news:
A large-scale multi-document summarization dataset
and abstractive hierarchical model.
In A. Korhonen,
D. Traum, and L. Màrquez, editors, Proceedings of the
57th Annual Meeting of the Association for Computa-
tional Linguistics, pages 1074–1084, Florence, Italy,
July 2019. Association for Computational Linguistics.
[12] J. Fried, G. I. Chaudhry, E. Saurez, E. Choukse, I. Goiri,
S. Elnikety, R. Fonseca, and A. Belay. Making kernel
bypass practical for the cloud with junction. In 21st
USENIX Symposium on Networked Systems Design and
Implementation (NSDI 24), pages 55–73, Santa Clara,
CA, Apr. 2024. USENIX Association.
[13] J. Fried, Z. Ruan, A. Ousterhout, and A. Belay. Caladan:
Mitigating interference at microsecond timescales. In
OSDI. USENIX Association, 2020.
[14] Y. Fu, L. Xue, Y. Huang, A.-O. Brabete, D. Ustiugov,
Y. Patel, and L. Mai. ServerlessLLM: Low-Latency
serverless inference for large language models. In 18th
USENIX Symposium on Operating Systems Design and
Implementation (OSDI 24), pages 135–153, Santa Clara,
CA, July 2024. USENIX Association.
[15] GitHub.
Github copilot - write code faster.
https:
//copilot.github.com/, 2025.
[16] Google.
Tensorflow serving is a flexible, high-
performance serving system for machine learning mod-
els.
https://www.tensorflow.org/tfx/guide/
serving, 2024.
[17] Google Labs.
NotebookLM | Note Taking and
Research Assistant Powered by AI.
https://
notebooklm.google.com/, 2024.
[18] A. Grattafiori, A. Dubey, A. Jauhri, and E. al. The llama
3 herd of models, 2024.
[19] A. Gujarati, R. Karimi, S. Alzayat, W. Hao, A. Kauf-
mann, Y. Vigfusson, and J. Mace. Serving DNNs like
clockwork: Performance predictability from the bottom
up. In 14th USENIX Symposium on Operating Systems
Design and Implementation (OSDI 20), pages 443–462.
USENIX Association, Nov. 2020.
[20] M. Han, H. Zhang, R. Chen, and H. Chen. Microsecond-
scale preemption for concurrent GPU-accelerated DNN
inferences. In 16th USENIX Symposium on Operating
Systems Design and Implementation (OSDI 22), pages
539–558, Carlsbad, CA, July 2022. USENIX Associa-
tion.
[21] W. He, K. Liu, J. Liu, Y. Lyu, S. Zhao, X. Xiao, Y. Liu,
Y. Wang, H. Wu, Q. She, X. Liu, T. Wu, and H. Wang.
Dureader: a chinese machine reading comprehension
dataset from real-world applications, 2018.
13


---

[22] C. Holmes, M. Tanaka, M. Wyatt, A. A.
Awan,
J. Rasley, S. Rajbhandari, R. Y. Aminabadi, H. Qin,
A. Bakhtiari, L. Kurilenko, and Y. He.
Deepspeed-
fastgen: High-throughput text generation for llms via
mii and deepspeed-inference, 2024.
[23] K. Hu.
Chatgpt sets record for fastest-growing user
base.
https://www.reuters.com/technology/
chatgpt-sets-record-fastest-growing-user-
base-analyst-note-2023-02-01/, 2023.
[24] N. Jain, T. Zhang, W.-L. Chiang, J. E. Gonzalez, K. Sen,
and I. Stoica. Llm-assisted code cleaning for training
accurate code generators, 2023.
[25] H. Jin, Y. Zhang, D. Meng, J. Wang, and J. Tan. A com-
prehensive survey on process-oriented automatic text
summarization with exploration of llm-based methods,
2024.
[26] K. Kaffes, T. Chong, J. T. Humphries, A. Belay, D. Maz-
ières, and C. Kozyrakis. Shinjuku: Preemptive schedul-
ing for usecond-scale tail latency.
In 16th USENIX
Symposium on Networked Systems Design and Imple-
mentation (NSDI 19), pages 345–360, Boston, MA, Feb.
2019. USENIX Association.
[27] W. Kwon, Z. Li, S. Zhuang, Y. Sheng, L. Zheng, C. H.
Yu, J. Gonzalez, H. Zhang, and I. Stoica.
Efficient
memory management for large language model serving
with pagedattention. In Proceedings of the 29th Sympo-
sium on Operating Systems Principles, SOSP ’23, page
611–626, New York, NY, USA, 2023. Association for
Computing Machinery.
[28] Z. Li, L. Zheng, Y. Zhong, V. Liu, Y. Sheng, X. Jin,
Y. Huang, Z. Chen, H. Zhang, J. E. Gonzalez, and I. Sto-
ica.
AlpaServe: Statistical multiplexing with model
parallelism for deep learning serving. In 17th USENIX
Symposium on Operating Systems Design and Imple-
mentation (OSDI 23), pages 663–679, Boston, MA, July
2023. USENIX Association.
[29] P. Liang, R. Bommasani, T. Lee, D. Tsipras, D. Soylu,
M. Yasunaga, Y. Zhang, D. Narayanan, Y. Wu, A. Ku-
mar, B. Newman, B. Yuan, B. Yan, C. Zhang, C. Cos-
grove, C. D.
Manning, C. Ré, D. Acosta-Navas,
D. A.
Hudson, E. Zelikman, E. Durmus, F. Lad-
hak, F. Rong, H. Ren, H. Yao, J. Wang, K. San-
thanam, L. Orr, L. Zheng, M. Yuksekgonul, M. Suzgun,
N. Kim, N. Guha, N. Chatterji, O. Khattab, P. Hen-
derson, Q. Huang, R. Chi, S. M. Xie, S. Santurkar,
S. Ganguli, T. Hashimoto, T. Icard, T. Zhang, V. Chaud-
hary, W. Wang, X. Li, Y. Mai, Y. Zhang, and Y. Koreeda.
Holistic evaluation of language models, 2023.
[30] S. Liu, A. Biswal, A. Cheng, X. Mo, S. Cao, J. E. Gon-
zalez, I. Stoica, and M. Zaharia. Optimizing llm queries
in relational workloads, 2024.
[31] X. Miao, C. Shi, J. Duan, X. Xi, D. Lin, B. Cui, and
Z. Jia. Spotserve: Serving generative large language
models on preemptible instances. In Proceedings of
the 29th ACM International Conference on Architec-
tural Support for Programming Languages and Operat-
ing Systems, Volume 2, ASPLOS ’24, page 1112–1127,
New York, NY, USA, 2024. Association for Computing
Machinery.
[32] Moonshot AI.
Mooncake KV Cache Transfer
Engine.
https://github.com/kvcache- ai/
Mooncake/, 2024.
[33] Mosaic AI Research.
Llm inference perfor-
mance engineering:
Best practices.
https :
/ / www.databricks.com / blog / llm - inference -
performance - engineering - best - practices,
2024.
[34] A. Narayan, I. Chami, L. Orr, S. Arora, and C. Ré. Can
foundation models wrangle your data?, 2022.
[35] S. Narayan, S. B. Cohen, and M. Lapata. Don’t give me
the details, just the summary! topic-aware convolutional
neural networks for extreme summarization.
ArXiv,
abs/1808.08745, 2018.
[36] NerdyNav.
107 Up-to-Date ChatGPT Statistics and
User Numbers.
https://nerdynav.com/chatgpt-
statistics/, 2024.
[37] NVIDIA. Fastertransformer: Transformer related opti-
mization, including bert, gpt. https://github.com/
NVIDIA/FasterTransformer, 2024.
[38] NVIDIA.
System Management Interface SMI.
https : / / developer.download.nvidia.com /
compute / DCGM / docs / nvidia - smi - 367.38.pdf,
2024.
[39] NVIDIA.
Tensorrt-llm: A tensorrt toolbox for op-
timized large language model inference.
https://
github.com/NVIDIA/TensorRT-LLM, 2024.
[40] NVIDIA.
Triton Inference Server.
https:
/ / developer.nvidia.com / triton - inference -
server, 2024.
[41] NVIDIA. Conditional CUDA Graph Nodes. https:
//docs.nvidia.com/cuda/cuda- c- programming-
guide / index.html#conditional - graph - nodes,
2025.
14


---

[42] G. Oliaro, X. Miao, X. Cheng, V. Kada, R. Gao,
Y. Huang, R. Delacourt, A. Yang, Y. Wang, M. Wu,
C. Unger, and Z. Jia. Flexllm: A system for co-serving
large language model inference and parameter-efficient
finetuning, 2025.
[43] OpenAI. Gpt-4 technical report, 2023.
[44] OpenAI. Batch api. https://platform.openai.com/
docs/guides/batch/batch-api, 2024.
[45] OpenAI.
Streaming
api.
https : / /
platform.openai.com / docs / api - reference /
streaming, 2024.
[46] OpenAI.
Chatgpt: Conversational language model.
https://chat.openai.com, 2025.
[47] OpenJDK.
HotSpot Glossary of Terms:
Safe-
point.
https://openjdk.org/groups/hotspot/
docs/HotSpotGlossary.html, 2024.
[48] M. Ott, S. Edunov, A. Baevski, A. Fan, S. Gross, N. Ng,
D. Grangier, and M. Auli. fairseq: A fast, extensible
toolkit for sequence modeling, 2019.
[49] A. Ousterhout, J. Fried, J. Behrens, A. Belay, and H. Bal-
akrishnan. Shenango: Achieving high CPU efficiency
for latency-sensitive datacenter workloads. In NSDI,
pages 361–378, 2019.
[50] L. Pan, J. Liu, J. Yuan, R. Zhang, P. Li, and Z. Xiao.
Occl: a deadlock-free library for gpu collective commu-
nication, 2023.
[51] P. Patel, E. Choukse, C. Zhang, A. Shah, Íñigo Goiri,
S. Maleki, and R. Bianchini. Splitwise: Efficient gener-
ative llm inference using phase splitting, 2024.
[52] G. Prekas, M. Kogias, and E. Bugnion. Zygos: Achiev-
ing low tail latency for microsecond-scale networked
tasks. In Proceedings of the 26th Symposium on Op-
erating Systems Principles, SOSP ’17, page 325–341,
New York, NY, USA, 2017. Association for Computing
Machinery.
[53] Proxet.
Llm has a performance problem inherent to
its architecture: Latency. https://www.proxet.com/
blog / llm - has - a - performance - problem -
inherent - to - its - architecture - latency,
2023.
[54] Python Software Foundation. gc—Garbage Collector
interface.
https://docs.python.org/3/library/
gc.html, 2024.
[55] B. Rozière, J. Gehring, F. Gloeckle, S. Sootla, I. Gat,
X. E.
Tan, Y. Adi, J. Liu, R. Sauvestre, T. Re-
mez, J. Rapin, A. Kozhevnikov, I. Evtimov, J. Bit-
ton, M. Bhatt, C. C. Ferrer, A. Grattafiori, W. Xiong,
A. Défossez, J. Copet, F. Azhar, H. Touvron, L. Martin,
N. Usunier, T. Scialom, and G. Synnaeve. Code llama:
Open foundation models for code, 2024.
[56] Y. Sheng, S. Cao, D. Li, B. Zhu, Z. Li, D. Zhuo, J. E.
Gonzalez, and I. Stoica. Fairness in serving large lan-
guage models. In 18th USENIX Symposium on Oper-
ating Systems Design and Implementation (OSDI 24),
pages 965–988, Santa Clara, CA, July 2024. USENIX
Association.
[57] Y. Sheng, L. Zheng, B. Yuan, Z. Li, M. Ryabinin,
B. Chen, P. Liang, C. Ré, I. Stoica, and C. Zhang. Flex-
gen: high-throughput generative inference of large lan-
guage models with a single gpu. In Proceedings of the
40th International Conference on Machine Learning,
ICML’23. JMLR.org, 2023.
[58] Z. Tang, Y. Wang, Q. Wang, and X. Chu. The impact
of gpu dvfs on the energy and performance of deep
learning: an empirical study.
In Proceedings of the
Tenth ACM International Conference on Future Energy
Systems, e-Energy ’19, page 315–325, New York, NY,
USA, 2019. Association for Computing Machinery.
[59] The Claude Team. Introducing the next generation of
claude. https://www.anthropic.com/news/claude-
3-family, 2024.
[60] The PyTorch Foundation. Torchserve is a performant,
flexible, and easy to use tool for serving pytorch models
in production. https://pytorch.org/serve/, 2024.
[61] vLLM
team.
vLLM
Piecewise
CUDA
Graph.
https :/ /docs.vllm.ai /en /stable /
design / torch_compile.html?h = piecewise +
cuda#cudagraph-capture, 2025.
[62] Y. Wang, Y. Chen, Z. Li, Z. Tang, R. Guo, X. Wang,
Q. Wang, A. C. Zhou, and X. Chu. Towards efficient
and reliable llm serving: A real-world workload study,
2024.
[63] H. Wu, M. Zhan, H. Tan, Z. Hou, D. Liang, and L. Song.
Vcsum: A versatile chinese meeting summarization
dataset, 2023.
[64] T. Xia, Z. Mao, J. Kerney, E. J. Jackson, Z. Li, J. Xing,
S. Shenker, and I. Stoica. Skylb: A locality-aware cross-
region load balancer for llm inference. arXiv preprint
arXiv:2505.24095, 2025.
15


---

[65] W. Xiao, S. Ren, Y. Li, Y. Zhang, P. Hou, Z. Li, Y. Feng,
W. Lin, and Y. Jia. AntMan: Dynamic scaling on GPU
clusters for deep learning.
In 14th USENIX Sympo-
sium on Operating Systems Design and Implementation
(OSDI 20), pages 533–548. USENIX Association, Nov.
2020.
[66] A. Yang, B. Yang, B. Zhang, B. Hui, B. Zheng, B. Yu,
C. Li, D. Liu, F. Huang, H. Wei, H. Lin, J. Yang,
J. Tu, J. Zhang, J. Yang, J. Yang, J. Zhou, J. Lin,
K. Dang, K. Lu, K. Bao, K. Yang, L. Yu, M. Li, M. Xue,
P. Zhang, Q. Zhu, R. Men, R. Lin, T. Li, T. Xia, X. Ren,
X. Ren, Y. Fan, Y. Su, Y. Zhang, Y. Wan, Y. Liu, Z. Cui,
Z. Zhang, and Z. Qiu. Qwen2.5 technical report. arXiv
preprint arXiv:2412.15115, 2024.
[67] G.-I. Yu, J. S. Jeong, G.-W. Kim, S. Kim, and B.-
G. Chun.
Orca: A distributed serving system for
Transformer-Based generative models. In 16th USENIX
Symposium on Operating Systems Design and Imple-
mentation (OSDI 22), pages 521–538, Carlsbad, CA,
July 2022. USENIX Association.
[68] D. Zhang, H. Wang, Y. Liu, X. Wei, Y. Shan, R. Chen,
and H. Chen. Fast and live model auto scaling with o(1)
host caching, 2024.
[69] H. Zhang, Y. Tang, A. Khandelwal, and I. Stoica. SHEP-
HERD: Serving DNNs in the wild. In 20th USENIX
Symposium on Networked Systems Design and Imple-
mentation (NSDI 23), pages 787–808, Boston, MA, Apr.
2023. USENIX Association.
[70] Y. Zhao, S. Yang, K. Zhu, L. Zheng, B. Kasikci, Y. Zhou,
J. Xing, and I. Stoica. Blendserve: Optimizing offline
inference for auto-regressive large models with resource-
aware batching, 2024.
[71] L. Zheng, L. Yin, Z. Xie, J. Huang, C. Sun, C. H. Yu,
S. Cao, C. Kozyrakis, I. Stoica, J. E. Gonzalez, C. Bar-
rett, and Y. Sheng. Efficiently programming large lan-
guage models using sglang, 2023.
[72] Y. Zhong, S. Liu, J. Chen, J. Hu, Y. Zhu, X. Liu, X. Jin,
and H. Zhang. DistServe: Disaggregating prefill and
decoding for goodput-optimized large language model
serving, July 2024.
16
