---
title: 'POLCA: Power Oversubscription in LLM Cloud Providers'
id: polca-power-oversubscription-in-llm-cloud-providers
tags:
- onprem-llm-inference-stacks-859099
- locus-rack-power-density-ceiling-and-measured-anchors
- datacenter-facilities
- measured-power
- gpu-utilization
- primary-source
created: '2026-10-23T17:56:59.305668Z'
updated: '2026-10-23T17:59:44.342163Z'
source: https://arxiv.org/pdf/2308.12908
source_domain: arxiv.org
fetched_at: '2026-10-23T17:56:59.305402Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: institutional
content_type: paper
deprecated: false
summary: 'POLCA (Microsoft, arXiv 2308.12908) on power oversubscription for LLM serving:
  Azure inference clusters draw up to 80% of provisioned power against roughly 10%
  headroom for training. This is the probable uncited origin of the ''inference draws
  60-80% of maximum'' figure repeated across vendor sources, and its multiplexing
  premise does not transfer to small on-premise fleets.'
raw_file: raw/polca-power-oversubscription-in-llm-cloud-providers.pdf
doi: arXiv:2308.12908
---

POLCA: Power Oversubscription in LLM Cloud Providers
Pratyush Patel*, Esha Choukse,
Chaojie Zhang, Íñigo Goiri, Brijesh Warrier, Nithish Mahalingam, Ricardo Bianchini
Microsoft Azure
Abstract
Recent innovation in large language models (LLMs), and
their myriad use-cases have rapidly driven up the compute ca-
pacity demand for datacenter GPUs. Several cloud providers
and other enterprises have made substantial plans of growth
in their datacenters to support these new workloads. One of
the key bottleneck resources in datacenters is power, and given
the increasing model sizes of LLMs, they are becoming increas-
ingly power intensive. In this paper, we show that there is a
significant opportunity to oversubscribe power in LLM clus-
ters. Power oversubscription improves the power efficiency
of these datacenters, allowing more deployable servers per
datacenter, and reduces the deployment time, since building
new datacenters is slow.
We extensively characterize the power consumption patterns
of a variety of LLMs and their configurations. We identify the
differences between the inference and training power consump-
tion patterns. Based on our analysis of these LLMs, we claim
that the average and peak power utilization in LLM clusters
for inference should not be very high. Our deductions align
with the data from production LLM clusters, revealing that
inference workloads offer substantial headroom for power
oversubscription. However, the stringent set of telemetry and
controls that GPUs offer in a virtualized environment, makes
it challenging to have a reliable and robust power oversub-
scription mechanism.
We propose POLCA, our framework for power oversubscrip-
tion that is robust, reliable, and readily deployable for GPU
clusters. Using open-source models to replicate the power
patterns observed in production, we simulate POLCA and
demonstrate that we can deploy 30% more servers in the same
GPU cluster for inference, with minimal performance loss.
1. Introduction
Motivation. Datacenters and cloud providers today face a
massive GPU capacity crunch due to the explosion in demand
for large language models (LLMs) [8]. For example, Ope-
nAI scaled up their clusters to 7,500 GPU servers to train
LLMs like GPT-3 [37]; Meta deployed an AI training super-
cluster with over 6,000 A100 GPUs [29]. This demand is
only growing for training newer and larger models like Bard
and GPT-4 [13]. The demand for inference is even larger
*Pratyush Patel is affiliated with the University of Washington, but was
at Microsoft during this work.
than for training, and may constitute over 90% of the overall
LLM compute cycles [40, 46, 49]. To keep up, several enter-
prises are making large investments into building new GPU
clusters to run LLM workloads [29, 37]. However, building
new datacenters is expensive and carbon intensive [1, 28];
and crucially, doing so takes a long time which does not ad-
dress the immediate demand. Power, space, and cooling are
the major bottlenecks in making datacenters denser. Power-
dense deployments like DGX A100s though, have power as
their main bottleneck. Datacenters are deployed with a fixed
power budget, based on generators and contracts with the
utility companies [12, 14, 18, 59]. Therefore, despite lower
power utilization, without a proper power oversubscription
mechanism, adding more GPU servers to an existing data-
center would push beyond the available power budget, and is
therefore not an option.
Our work. We extensively analyze the power consumption
patterns and phases in LLMs. We investigate both, the training
and inference serving, with various configurations representa-
tive of different LLM use-cases. Based on this, it is clear that
there are specific properties in the workloads that would lead
to low power utilization at the inference cluster level, despite
high power utilization at the server-level. We then observe the
power utilization of LLM inference and training at the cluster
level in production, to verify our claims. We observe that the
power utilization does not peak to the level of the allocated
power, despite individual servers peaking to their allocated
power. Instead, LLM inference clusters utilize only up to 80%
of the provisioned power, making them excellent candidates
for power oversubscription. In contrast, LLM training clus-
ters offer a smaller headroom (about 10%) since they incur
in massive and coordinated power peaks due to large-scale
synchronous training jobs. In this work, our goal is to safely
oversubscribe the provisioned power in existing and upcom-
ing multi-tenant GPU clusters to reduce costs and address the
capacity crunch of running LLM workloads.
Power oversubscription could sometimes result in power
overload, and therefore cannot be deployed safely without
implementing mitigation mechanisms. This poses three main
challenges for power oversubscription in GPU clusters. First,
since LLM workloads are GPU intensive, existing CPU-based
power oversubscription techniques [22, 24, 26, 56] are ineffec-
tive in these clusters. To deal with this, we explore the power
distribution within the server to ascertain the impact the GPU
arXiv:2308.12908v1  [cs.DC]  24 Aug 2023


---

GPT forward
pass 1
GPT forward 
pass 2
GPT forward 
pass 3
GPT forward 
pass 4
KV 
cache
KV 
cache
KV 
cache
Is pizza better, 
or a burger?
Pizza
is
better
EOS
Prompt
processing
Token
sampling
Figure 1: An example of the prompt and token phases in a
GPT-style model.
Figure 2: Provisioned power
(8×A100-80GB server).
Category
Models
Encoder
RoBERTa
Decoder
GPT-NeoX-20B
OPT-30B*
BLOOM-176B*
Encoder-Decoder
Flan-T5 XXL
Figure 3: Our LLM workloads.
*inference only.
can have on the server-level power.
Second, since LLMs are new and rapidly evolving, the
efficacy of the throttling in reducing power usage, and the
impact on application performance is not well understood.
We base our design on our characterization of the efficacy
of GPU power capping and frequency scaling on modern
LLM inference workloads. We also target configurability and
robustness to support the changing LLMs over time.
And third, GPU power management poses its own set of
challenges [38]. GPUs do not expose the plethora of well
tailored power telemetry and control knobs that the CPU-
based datacenters use to make cluster-level power throttling
decisions. Datacenters need out-of-band mechanisms to com-
municate with the devices in a controlled and time-sensitive
way. Although out-of-band GPU power management inter-
faces do exist, they are slow and unreliable, which complicate
safe power throttling. We address this with a double thresh-
old solution, ensuring a safe time buffer before reaching peak
cluster power utilization. Our approach uses two cluster-level
power thresholds for frequency throttling based on the infer-
ence priority. If power usage remains insufficiently reduced,
and we reach maximum cluster power utilization, all GPUs
are rapidly throttled via the hardware powerbrake mechanism
to prevent power outages.
Based on these insights, we design POLCA, a robust, re-
liable power oversubscription framework for LLM inference
clusters, which integrates with existing cluster-level power
manager. Using open-source models, we replicate power pat-
terns from production LLM inference clusters for evaluation.
POLCA boosts allocated server capacity by 30% in existing
inference clusters, with minimal power throttling events. This
improves power efficiency, reduces costs through fewer data-
centers, and promptly meets the demand for running additional
LLM workloads.
Summary. We make the following contributions:
• An extensive characterization of the power patterns of mod-
ern LLMs in their training and inference phases, with a
deep dive into power consumption phases in inference with
different configurations.
• A characterization of the efficacy of existing GPU power
management knobs, namely frequency scaling and power
capping, at reclaiming power for LLM workloads.
• An overview of the power headroom available in production
LLM clusters, that is in line with our characterization and
understanding of these LLM workloads.
• A case for building separate inference-optimized clusters, to
increase the datacenter power utilization of LLM inference.
• POLCA, a robust and reliable approach for modern GPU
servers that enables safe and efficient power oversubscrip-
tion in LLM clusters today, while meeting performance
SLOs.
• An evaluation of POLCA on a replication of power patterns
in LLM inference production clusters.
2. LLM Characterization
In this section, we introduce modern LLMs and extensively
characterize their power usage patterns at a server level, focus-
ing on the intrinsic differences between training and inference
workloads, prompt and token phases within inferences, and
behaviors under GPU power management techniques.
2.1. LLMs in the Cloud
Transformer models. We focus this work on modern LLMs
that are transformer-based. Beyond the tokenizer and em-
bedding layers, transformer models generally consist of atten-
tion [53] and multi-layer-perceptron layers to contextualize the
inputs, and generate an output. Encoder-only transformer mod-
els like BERT [11] and RoBERTa [27] consist of bi-directional
self-attention, allowing them to contextualize the tokens al-
together for language understanding tasks like summarizing
and sentiment analysis. Decoder-only transformer models
like GPT [44] and BLOOM [47] consist of masked or uni-
directional self-attention for generating language sequences.
Encoder-decoder models like FLAN-T5 [10] use an encoder
for understanding the input and the decoder for generating
text. Transformer models can be for language, vision, or multi-
modal. Even though we focus on transformer-based LLMs, in
Section 7 we show that our work can be extended to vision
and multi-modal models.
Training vs. inference. LLMs are generally used in a train
once and use forever mode. Training is generally much more
resource intensive than inference, as the model is fed a lot
of data for many iterations in parallel. For example, Ope-
nAI scaled up their clusters to 7,500 GPU servers to train
LLMs like GPT-3 [37]. Since training for LLMs is generally
run across thousands of GPUs, and it must update weights
across GPUs, it involves both a computation-heavy and a
2


---

0
10
20
30
40
Time (s)
0.50
0.75
1.00
Normalized Power
TDP
(a) Flan-T5.
0
5
10
15
Time (s)
0.50
0.75
1.00
Normalized Power
TDP
(b) GPT-NeoX-20B.
0
5
10
15
20
Time (s)
0.50
0.75
1.00
Normalized Power
TDP
(c) OPT-30B.
0
10
20
30
Time (s)
0.50
0.75
1.00
Normalized Power
TDP
(d) BLOOM-176B.
Figure 4: GPU power usage timeseries for multiple inference models. It shows distinct power usage patterns in prompt (spiky) vs. token phase
(longer, more stable, and lower). The phases in each model take different amount of times.
communication-heavy phase per iteration. Especially between
the forward and backward passes in a training iteration, there
is a communication bubble. On the other hand, inference
only requires forward pass of the model, and requires fewer
resources. For instance, a BLOOM inference (similar in size
to GPT-3) can be served using 8 GPUs on a single server.
Prompt processing vs. token sampling. Figure 1 shows the
two main phases in an LLM inference: prompt processing, and
token sampling. Prompt sampling can be done in parallel on
all the input tokens, making it compute-intensive. On the other
hand, token sampling is sequential, and uses the cached data
from the tokens processed so far, making it a computationally
light, and memory bandwidth-intensive phase. Most of the
computation, such as KV-cache, during the prompt processing
phase is then cached to avoid recomputation [53].
Configuration knobs in training and inference.
• Batch size defines the number of sequences processed to-
gether. A higher batch size leads to better throughput. For
training, the norm is to maximize the batch size based on
the available memory and compute. For inference, batching
is best-effort, since waiting to fill the batch size can cause
delays in the results.
• Input size defines the length of the prompt sequence.
• Output size defines the maximum number of output tokens
to be generated per prompt. The model can generate end of
sequence (EOS) before the suggested reaching the output
size, thereby truncating the output.
Offering LLMs in the cloud. Cloud providers can host LLMs
in different way. First, the customers can bring their own
models and host them using the infrastructure-as-a-service
based virtual machine offerings, making the model opaque
to the cloud provider. Second, the cloud provider could use
platforms like Singularity [51], Azure OpenAI service [6],
Google BARD [7], or Azure ML [5] to help customers use
known models. This method provides the cloud provider
visibility into the model type.
2.2. Characterization Methodology
Hardware. We run workloads on two NVIDIA DGX A100
machines with 8×A100-40GB and 8×A100-80GB GPUs re-
spectively [34, 35]. Due to GPU availability crunch, we use
the former machine to run training workloads and the latter to
run inference workloads. GPUs communicate with the host
CPU using PCIe 4.0 and are interconnected via NVLink 3.0
for fast inter-GPU communication. The CPU is a dual-socket
AMD Rome. Figure 2 shows that GPUs make around 50% of
the server power.
Workloads and metrics. Figure 3 shows the LLMs we evalu-
ated which span domains and tasks. We consider popular open-
source LLM models varying structures and sizes: Encoder
(RoBERTa [27]), Decoder (GPT-NeoX, OPT, BLOOM [47]),
and Encoder+Decoder (Flan-T5 [10]) transformer models [53].
We use the models weights and training scripts from open-
source repositories (i.e., Huggingface Transformers [55], GPT-
NeoX library [4], and DeepSpeed [3, 30, 48]).
To accurately reflect inference efficiency, we use paralleliza-
tion strategies including tensor and data parallelism, which are
supported by popular open-source deep learning frameworks
today [16, 30, 43]. To emulate the worst-case scenario in terms
of power utilization, we saturate GPUs by running a constant
stream of inference requests with no idle time.
For training, we run each workload on dedicated server for
at least 5 minutes (100+ iterations) across all 8 GPUs. Our
training setup uses distributed data parallelism (DDP) and/or
tensor parallelism [25, 50]. We configure the training batch
sizes to use at least 85% of the GPU memory.
We run NVIDIA DCGM with a 100 ms interval to track uti-
lization, power consumption, temperature, hardware activity,
and other performance counters on each GPU [36].
GPU controls. For our characterization, we use nvidia-smi
based GPU controls on the A100 GPUs. The two main con-
trols we explore are power caps and frequency caps. Each
GPU supports power caps ranging from 100W–400W and SM
clock frequencies from 0.2GHz–1.4GHz. For power capping
and frequency scaling experiments, we evaluate a subset of
the supported GPU clock frequencies and power caps ranging
between 1.1–1.4GHz and 325–400W respectively to make the
parameter space tractable. We discuss a subset of results.
2.3. LLM Inference Power Characterization
Phases in power consumption. Figure 4 shows the time-
series power consumption for multiple inference models, each
with three inferences of the same prompt. We observe that
across all inference models, during every iteration, the power
3


---

usage patterns exhibit two distinct phases: power spikes in
the beginning, and a stable, lower power consumption later.
Power spikes consistently occur at the start of every inference
request, often going beyond GPU’s TDP values. These spikes
correspond to the compute-intensive prompt phases of LLMs
as all the token in the prompt can be processed in parallel,
causing a large input matrix (i.e., the input query to the model).
Following the spike, the stable, lower power consumption
phase corresponds to sequential, autoregressive token sam-
pling. The token sampling phase sequentially generates new
tokens by re-using activations stored in the KV-cache and only
incurs light computation, so the power draw during token sam-
pling phase relatively stable and low. Since a large number of
output tokens may be sequentially generated in response to a
single request (an input prompt), the prompt-phase computa-
tion tends to last much shorter than the token phase and the
resulting power spike per request generally lasts < 1 second.
Although we see these power consumption spikes across the
GPUs serving the same inference, the spikes are not correlated
across different endpoints serving different inferences. This
is because of the arrival time variation in the prompts and
scheduling. Therefore, at a cluster level, we would not expect
to see these peaks align. Instead, a statistical multiplexing of
the prompt and token processing stages across various servers
should yield lower peak power consumption at the cluster
level, despite high peak power numbers per server.
Power patterns with different configurations. Inference
workloads can differ in their input and output parameters based
on the use cases, effectively changing the amount of computa-
tion during prompt and token phases. Therefore, we closely
examine the power consumption patterns and latency implica-
tions for a variety of LLM inference configurations in Figure 5.
To separately characterize the peak and mean power, we depict
GPU power normalized to TDP for each model and configu-
ration in stacked bars of two components: the lower (opaque
bars — mean power during iterations) and the higher (regular
bars — peak power). We first observe that with the same con-
figurations, larger models (e.g., BLOOM-176B) incur a much
higher amount of computation during both prompt and token
phases, and show significantly larger peak and mean power
consumption.
Input sizes. Figure 5a shows the mean and peak power to
TDP ratio varying input sizes from 256 to 8192 tokens. As
input size increases, peak power drastically goes up across
all models, reflecting the significant increase in prompt phase
computations. The mean power, dominated by lighter token
sampling computation, remains stable and low, further drawing
the distinction of power patterns between prompt and token
phases. Figure 5b shows the corresponding latency results. As
the sequential token sampling phase contributes to most of
the query latency, increasing input sizes shows little effect on
latency until >4k input tokens.
Batch Sizes. Figure 5c shows the power impact of varying
Flan-T5
GPT-NeoX
OPT
BLOOM
Model
0.00
0.25
0.50
0.75
1.00
GPU Power to TDP Ratio
TDP
256
512
1024
2048
4096
8192
(a) Power varying input sizes.
Flan-T5
GPT-NeoX
OPT
BLOOM
Model
0.0
2.5
5.0
7.5
10.0
Query Latency (s)
256
512
1024
2048
4096
8192
(b) Latency varying input sizes.
Flan-T5
GPT-NeoX
OPT
BLOOM
Model
0.00
0.25
0.50
0.75
1.00
GPU Power to TDP Ratio
TDP
1
2
4
8
16
(c) Power varying batch sizes.
Flan-T5
GPT-NeoX
OPT
BLOOM
Model
0.0
2.5
5.0
7.5
10.0
Query Latency (s)
1
2
4
8
16
(d) Latency varying batch sizes.
Flan-T5
GPT-NeoX
OPT
BLOOM
Model
0.00
0.25
0.50
0.75
1.00
GPU Power to TDP Ratio
TDP
128
256
512
1024
2048
4096
(e) Power varying output sizes.
Flan-T5
GPT-NeoX
OPT
BLOOM
Model
0
50
100
150
200
Query Latency (s)
128
256
512
1024
2048
4096
(f) Latency varying output sizes.
Figure 5: Power (mean, peak) and latency sensitivity to the input,
batch, and output sizes for multiple inference models running on
A100-80GB GPUs.
0
20
40
Time (s)
0.0
0.5
1.0
Normalized Power
TDP
(a) No cap.
0
20
40
Time (s)
0.0
0.5
1.0
Normalized Power
Cap
TDP
(b) 325W cap per GPU.
0
20
40
Time (s)
0.0
0.5
1.0
Normalized Power
Cap
TDP
(c) 1.1GHz SM clock.
Figure 6: GPU power capping and frequency scaling on BLOOM
inference (input=8192, output=128, and batch=1).
batch sizes from 1 to 16. Larger batch sizes effectively in-
crease the input sizes for prompt computation, resulting in
similar increase to peak power draws. Mean power also ex-
hibits a gradual increase, since the effective number of tokens
processed concurrently during token phase is higher. Figure 5d
shows a slight increase in latency due to computation increases
in both prompt and token phases from larger batch sizes.
Output Sizes. Because the sequential and autoregressive nature
of token sampling, similar computation and power consump-
tion patterns are expected to repeat for each generated token.
As a result, Figure 5e shows that increasing output size does
not affect the peak and mean power drawn but simply increases
the duration of request execution linearly (Figure 5f).
Impact of frequency and power capping. As discussed in
4


---

0
5
10
15
20
Peak Power Reduction (%)
0
5
10
15
Performance Reduction (%)
Flan-T5
GPT-NeoX
OPT
BLOOM
(a) All Models.
0
5
10
15
20
Peak Power Reduction (%)
0
5
10
15
Performance Reduction (%)
b=1 i=512
b=1 i=2048
b=1 i=8192
b=16 i=512
(b) BLOOM sensitivity.
Figure 7: Peak power reduction (based on TDP) vs. performance
reduction for multiple inference models at varying GPU SM frequen-
cies. The dashed black line shows a linear scaling of performance
with power drawn.
Section 2.2, the two main controls we can use in GPUs today
are frequency and power capping. Figure 6 shows the impact
of capping the power vs frequency on BLOOM inference.
Since power capping is reactive, it allows the initial peaks in
the prompt phase to go beyond the power cap. Frequency cap
being proactive is a better control, but leads to performance
impact throughout the execution, and not just when the power
utilization is high. Keeping this in mind, we should choose
frequency capping for a more reliable control. We discuss this
further in Section 5.
The next step is to quantify the performance and peak power
impact of frequency capping. Figure 7a shows the relative
peak power and performance reduction compared to no cap-
ping by varying GPU clock frequencies for all models. We
observe that the relationship between power reduction and
performance is superlinear—significant power (up to 20%)
can be reclaimed for minimal performance loss (maximum up
to 7%). Notably, the sensitivity to peak power reduction mech-
anisms varies across different models. In particular, larger
models tend to have more performance impact from frequency
capping. For example, GPT-NeoX incurs no performance
loss while BLOOM exhibits 5% at a similar peak power re-
duction level (13%). Figure 7b further shows the sensitivity
results of varying prompt computation (input and batch size)
for BLOOM. Smaller total input size shows less performance
loss with the same amount of peak power reduction, because
there is less prompt phase computation that gets impacted
by reduced frequencies. Overall, across models and config-
urations, the peak power reduction from locked frequency
execution is substantially higher than the relative performance
drop.
2.4. LLM Training Power Characterization
Peak power. Figure 8 (blue) shows the time series peak power
data updated every 100ms for 5 iterations of training per model.
The peak power during the training iterations goes up to the
TDP of the GPUs, and beyond for GPT-NeoX and Flan-T5.
On the other hand, RoBERTa, an encoder-only model does
not reach the TDP. Note that different types of data sharding,
0
5
10
Time (s)
0.25
0.50
0.75
1.00
Normalized Power
RoBERTa
0
5
10
Time (s)
GPT-NeoX
No cap
325W cap
1.1GHz
0
10
20
Time (s)
Flan-T5
Figure 8: Power usage timeseries for training workloads under no
cap, power cap, and frequency cap.
0
5
10
15
20
Peak Power Reduction (%)
0
5
10
15
Performance Reduction (%)
RoBERTa
GPT-NeoX
Flan-T5
(a) SM freqs.
0
5
10
15
20
Peak Power Reduction (%)
0
5
10
15
Performance Reduction (%)
RoBERTa
GPT-NeoX
Flan-T5
(b) Power caps.
Figure 9: Peak power vs. performance reduction for training.
batching, and parallelism techniques could slightly change
this behavior. Our takeaway here is that training can easily
reach the TDP of the system.
Power swings. Interestingly, Figure 8 (blue) also shows that
there are big swings in power draw across GPUs each iteration.
For example, in RoBERTa, an iteration lasts for 1 second.
Each iteration has a small dip in power around the 0.5 sec-
ond mark, and a big dip in power at the end. Based on the
model architecture, it is clear that the smaller dip is caused
between the forward and backward propagation phases, as
the threads working on the same data synchronize, and the
GPU utilization decreases. The larger dip on the other hand, is
caused at the end of the iteration as all the GPUs synchronize
before the next iteration starts. Therefore, the power swings
are caused by the inherent workload behavior of switching
between computation- and communication-intensive phases.
The power consumption in the communication-intensive
phase is different across the three models. While RoBERTa is
still at 75% of the TDP at the iteration boundary, GPT-NeoX
drops down to 50%, and Flan-T5 goes down all the way to
20%, which corresponds to the idle power of the GPUs.
In a larger scale training, these power swings will be corre-
lated across thousands of GPUs working on the same training
job, potentially causing challenges in the power delivery in-
frastructure. Note that the main challenge here is the power
swings, and not the peak power.
Impact of capping. We investigate the impact of frequency
capping on the training workloads and their power consump-
tion patterns. Figure 8 shows the power consumption under
power and frequency caps. The peak power is reduced by up
to 20% for these workloads. However, our main challenge is
5


---

Utility 
power grid
Rack n
Rack 1
Rack 1
Rack n
Rack n
Rack 1
PDU
ATS
Generator
...
...
...
PDU
PDU
UPS
UPS
UPS
UPS
Figure 10: Example of the power ar-
chitecture in a datacenter.
1.00
1.05
1.10
1.15
Peak GPU Power
0.80
0.85
0.90
0.95
Peak Server Power
Figure 11: Server and GPU peak power
consumption normalized to their TDPs.
Parameter
Value
Number of servers
40
Server type
DGX-A100
Power telemetry delay
2s
Power brake latency
5s
OOB commands latency
40s
Table 1: Default row-level parameters for
our study.
Training
Inference
Peak power utilization
97%
79%
Power usage pattern
Coordinated swings
every few seconds
Diurnal with
short-term variations
Max. power spike in 2s
37.5%
9%
Max. power spike in 5s
(power brake latency)
–
9.1%
Max. power spike in 40s
(OOB capping latency)
–
11.8%
Table 2: LLM cluster power usage in production.
the power swing, for which, we will need to bring down the
peak power, while maintaining the power troughs high. Note
that RoBERTa and GPT-NeoX having a considerable power
consumption at the iteration boundary means that they have
computations in the GPUs even in that phase. Therefore, for
these models, capping brings down the trough in the power
consumption too, not helping the power swing challenge. On
the other hand, Flan-T5 training iteration boundary brings the
GPUs down to idle, thereby reacting well to capping.
Figure 9 shows the impact of frequency and power cap-
ping on the throughput of the training. Frequency capping
is more effective in reclaiming larger amount of peak power,
and power capping introduces more performance variability
with less control over computation. For Flan-T5 and GPT-
NeoX, frequency capping reduces the peak server power by
22% while only impacting the performance by 10%. For sin-
gle large cluster-level training jobs, this can be used to deal
with the power swings, or, in case of multiple smaller con-
current jobs, can be used to oversubscribe power for more
deployments.
Since the usage of a training cluster over its lifetime can
vary in terms of the training job sizes, power oversubscription
could mean turning the cluster partially off to support larger
training jobs. Given the high cost and low availability of
GPUs, we think this is not a reasonable approach. Therefore,
we conclude that power oversubscription in training clusters
is tricky. Instead, we think that our capping results can mainly
be used to deal with the power swings effectively.
3. LLM Cluster Power Characteristics
3.1. Datacenter Power Management
Power provisioning. GPU servers by default are provisioned
for peak power draw because: (1) GPUs are typically designed
to maximize FLOPS, so hitting peak power draw is a likely sce-
nario, (2) cloud servers may run any workload, so provisioning
for the worst case ensures safety, and (3) out-of-band GPU
power monitoring is slow and capping not very reliable, which
makes power oversubscription difficult [38]. Consequently,
provisioning power for GPU servers is expensive.
Most large-scale CPU clusters deployed today use some
form of power oversubscription to reduce cost [14, 22]. For
example, they might uniformly de-rate servers, use workload-
aware power capping, or implement throttle-aware power man-
agement. In contrast, power oversubscription is challenging
in GPU clusters, as we elaborate upon in Section 4.
A datacenter floor plan is generally built around the power
hierarchy. A few servers are deployed within a rack, and
several racks make a row in the datacenter. Figure 10 shows
an example power hierarchy, where the PDUs would power
the row of racks [59].
3.2. GPU Power Usage Patterns at Scale
We only show subsets of the data from production clusters and
normalize the numbers for confidentiality.
Row-level Power. Table 2 shows the normalized aggregate
power consumption patterns of LLM training and inference
clusters at a large cloud provider. Note that we consider an in-
teractive inference cluster (i.e., where users make inference re-
quests and expect rapid responses). We observe that: (1) train-
ing has higher peak and average power draw compared to
inference, (2) training incurs large swings in power consump-
tion, up to 37.5% of the provisioned power capacity, whereas
inference only incurs a change of up to 9%, (3) inference
power consumption shows a diurnal pattern since it is an in-
teractive workload; yet, over the course of a few seconds, its
power usage remains relatively stable compared to training.
These differences imply that training tends to put much
higher strain on the cluster power delivery infrastructure com-
pared to inference. The power swings at scale are a chal-
lenge, as we also predict in Section 2. Inference workloads
6


---

are promising for power oversubscription, whereas training
workloads are not.
Server-level power. Next, since peak power drives power
provisioning decisions per server, we plot the peak server
and GPU power in a production cluster, relative to their TDP,
in Figure 11. We find that: (1) GPU power constitutes on
average 60% of server-level power consumption; hence we
focus on GPUs in the rest of this paper, (2) peak GPU power
far exceeds the overall server GPU TDP (by up to 500W), (3)
peak server power is highly correlated with peak GPU power,
(4) peak GPU power has a much smaller range than peak
server power, and (5) peak power remains largely unchanged
over time since servers are heavily utilized.
4. Challenges
Power oversubscription in LLM clusters today poses several
challenges.
A. Mixed inference and training. Current datacenters host
both inference and training in the same infrastructure. How-
ever, when managing power this is suboptimal due to the huge
disparity between their at-scale characteristics as discussed
in Section 2.4. Power swings in large training jobs preclude
power optimizations for inference.
B. Latency sensitive workloads. Servers in the LLM clus-
ters could be deployed for various use-cases. While use-
cases like summarization and understanding tasks may not be
latency-sensitive, a use-case like chat is very latency-sensitive.
Capping these all workloads equally is unreasonable and we
should prioritize latency-sensitive ones.
C. Distinct inference phases. Based on the power character-
ization in Section 2, LLM inference has two distinct phases
with very different profiles. While the prompt processing
takes up a lot of power, token sampling does not. This makes
it really difficult to manage power for these workloads.
D. Virtualized environment. Cloud providers can host LLMs
in Virtual Machines (VMs) or as services like Amazon Sage-
Maker [2], Azure ML [5], or Google Vertex AI [54]. In all of
these cases, GPUs are used in Direct Device Access (DDA)
mode, that precludes the cloud provider from accessing the
GPU drivers. Reliable and fast power or frequency capping
is necessary as a fallback for power oversubscription, in case
the overall power draw exceeds supported capacity. Although
GPUs have fast in-band controls (for instance, nvidia-smi for
A100 GPUs) that allow the use of drivers for frequency and
power capping, these are out of reach for the cloud provider.
E. Tight latency bounds with slow interfaces. At a worst-
case load of 133%, UPSes provide 10s for failure toler-
ance [59]. This places an upper bound on the latency of any
backstop capping, and makes it a requirement that the control
happens out-of-band, without the VM in the loop.
Lack of server-power controls. Features like Intel RAPL [42]
provide fast and reliable controls in the CPU servers to bring
down the power of the entire server down, by setting a single
cap on the CPU. However, for GPU servers, where about 60%
of the consumed power comes from GPUs, there are no fast
controls available to bring the entire server power down by
setting one cap.
Lack of fast out-of-band controls. Due to challenge D, the vir-
tualization and DDA, any power controls need to be accessible
out-of-band, through the rack manager and board management
controller (BMC) to be useful for cloud providers.
NVIDIA has a few controls through the SMBPBI [33] that
are helpful, but slow. The available controls are: (1) Frequency
caps for the GPU compute. Note that this does not allow us
to control the GPU memory clock, but just the GPU compute
clock. (2) Power caps at the GPU level, which allow us to
cap the power consumed by individual GPUs. As shown in
Section 2, the power caps do not guarantee the spikes from
breaching the desired level. (3) Power brake, which is a fast
lever (Table 1) to bring the GPU down to almost a halt, stop-
ping all progress.
F. Changes in the workload over time. The number of
additional racks added through power oversubscription is a
static decision, that stays for the lifetime of the servers (i.e.,
4-6 years). However, as the types of models and their use-
cases running in the cluster change with time, the policy for
power capping needs to be configurable enough to support
high performance for the workloads at most times, and avoid
frequent power capping.
G. Reliability. The power and frequency capping are back-
stops to ensure that the UPS does not trip. Therefore, the path
exercised to implement these need to be extremely reliable.
Therefore, we need to avoid depending on the user or any
unreliable code during the critical path for capping.
H. Ability to work with existing datacenters. Any changes
to the datacenter architecture would take many years to be
available. Such solutions are impractical to support the cur-
rent LLM demand. Our solution needs to work with existing
infrastructure available today. For this, we need to be able to
retrofit into the existing datacenters.
5. Designing POLCA
We start by discussing system-level design points and deci-
sions addressing aforementioned challenges, followed by the
detailed policy and flow.
A. Inference-optimized clusters. Challenge A makes it diffi-
cult to oversubscribe power in clusters today due to the mix
of inference and training. We argue that there is a need to
have separate clusters, optimized for inference, since the de-
mand for inference is much higher (more than 90%) than
large training jobs [40, 46, 49]. We build POLCA targeting
inference-optimized clusters, with room for further optimiza-
tions, like removing the back-end infiniband or other RDMA
network in the inference clusters.
7


---

Mode
Low Priority
High Priority
Uncapped
Uncapped
Uncapped
Threshold T1
Frequency capped
Uncapped
(1275 MHz)
Threshold T2
Frequency capped
Frequency capped
(1110 MHz)
(1305 MHz)
Powerbrake
Frequency capped
Frequency capped
(288MHz)
(288MHz)
Table 3: Power modes for low and high priority
workloads.
Row-level PDU
Power meter
Server
...
Telemetry
SMBPBI
GPU0
GPU7
BMC
Rack Manager
Power Manager
Figure 12: Power management
flow.
Workload
Prompt size
Output size
Ratio
Priority
Summarize
2048-8192
256–512
25%
Low
Search
512–2048
1024–2048
25%
High
Chat
2048–4096
128–2048
50%
50:50
Table 4: Workloads distribution, based on the
BLOOM-176B model.
Metric
High priority workload
Low priority workload
P50 latency impact
< 1%
< 5%
P99 latency impact
< 5%
< 50%
Number of powerbrakes
0
0
Table 5: Service level objectives (SLOs) for POLCA.
B. Per-priority power capping. To deal with challenge B, the
latency-sesitive workloads, we propose two service-level pri-
orities: high priority (HP) and low priority (LP), with the LP
workloads being more probable to be capped. Table 4 shows
our example distribution of the priorities between the types
of services. The allocator in the cloud is aware of these work-
load priorities, and can make power-oversubsciption aware
allocation to ensure a good mix of high and low-priority jobs
in every row.
C. Dealing with distinct inference phases. Challenge C
occurs due to the distinct power usage patterns between the
prompt and token phases in LLM inference. However, at a
cluster-level, the statistical multiplexing of these phases re-
duces the power utilization peaks lower, as seen in Section 2.4.
With this insight, we choose a higher power aggregation level,
the PDU breaker as our capping decision point. This corre-
sponds a row of racks as shown in Figure 10.
D. Handling virtualization. To provide guarantees against
power trips, POLCA uses only out-of-band interfaces that are
available to the cloud providers from outside the VM. We en-
sure that any of the settings we use can overwrite any settings
that the VM user asks for, thereby dealing with challenge D.
E. Designing within latency bounds. Table 1 quantifies the
latency of the slow out-of-band interfaces, representing chal-
lenge E. The main latency upper bound is imposed by the 10s
deadline from the UPSes [59]. The PDU telemetry-based de-
tection of a power threshold breach can be in the order of 3-5s.
Given that the A100 powerbrake takes 5s to implement, we
meet the 10s deadline from the UPSes [59]. However, power-
brake substantially throttles workload performance since it
brings down the frequency of all the GPUs to 288 MHz, and
should only be used in dire situations. On the other hand, the
less aggressive frequency and power caps take as long as 40s
to take effect. Our policy therefore uses multiple power thresh-
olds, while accounting for any power spikes that may happen
within these 40s. We track and use parameters like maximum,
P99, and P90 power spikes at 40s and 5s granularity in our
policy making. Table 2 shows a subset of these parameters.
5.1. Policy: Thresholds and Power Modes
The goal of POLCA is to maximize the additional servers
deployed using power oversubscription, while meeting the
Service Level Objectives (SLOs) in Table 5. To support per-
priority performance SLOs, in POLCA, we use two power
thresholds, as shown in Table 3.
Threshold T1. This is the lower power threshold, only appli-
cable to low-priority workloads. The two objectives here are:
(1) to sufficiently avoid capping HP workloads, and (2) to do
so while maintaining the SLOs for the low-priority workloads.
As we previously note in Section 2, prompt phase has high
power peaks, while token phase does not. A power cap only
impacts the prompt phase, whereas a frequency cap reduces
the power in both the phases. In order to maximize the power
savings from capping low-priority workloads, we choose fre-
quency capping for T1. Upon reaching T1, we set all the
low-priority workloads to the base frequency (the minimum
promised frequency) of A100 GPUs, 1275 MHz.
Threshold T2. This is the upper power threshold, which is
chosen to avoid powerbrakes completely. We therefore use
the observed value of maximum power spike in 40s to choose
this threshold. When T2 is breached, we start by frequency
capping all the low-priority workloads further down to 1110
MHz. If the power is still above the threshold, we cap the HP
workloads down to 1305MHz frequency, to incur negligible
performance impact while still reclaiming power.
Uncapping. The policy also needs to define when to uncap
the servers. It is important to build in a hysteresis, to avoid
constant capping, uncapping and overwhelm the power man-
agement system. Based on our parameter sweep, we choose
the uncap thresholds to be 5% below the corresponding cap-
ping threshold of T1 or T2.
Robustness and configurability. We divide the challenge
F (i.e., updates in LLMs over time) into two parts. First,
frequent efficiency and use-case updates to the models, that
can cause their peak power to increase, while keeping the
overall characteristics similar. Second, long term changes in
the model types. For the first challenge, we build in robustness
into POLCA as we discuss and evaluate further in Section 6.
For the second challenge, POLCA infrequently updates the
policy parameters (T1, T2, and the capping frequency) based
on the workload changes over time. For this, POLCA tunes
the variables using power traces combined with the history of
8


---

capping decisions. The steps followed for tuning are the same
as we show in Section 6.
5.2. Power Management Flow
We design a flow of control that is reliable and works in
existing datacenters, therefore resolving challenges G and
H. Figure 12 shows the hierarchy of telemetry and control
in the power management in POLCA. The power manager
running at rack-level receives frequent telemetry from the
PDU about row-level telemetry. We assume a homogeneous
distribution of power and caps for fast control. Based on its
knowledge of the high vs low priority per VM, the power
manager implements the threshold and caps as per the policy
we describe in Algorithm 1. Once the BMC at the server gets
the per-GPU caps from the power manager, it sets it across the
GPUs in the server, using an interface like the SMBPBI [33].
The POLCA design can be retrofitted into existing datacen-
ters, without new hardware, meters, or structures.
Algorithm 1 POLCA power management.
T1,T2 :Power thresholds
T1Buf fer,T2Buf fer :Thresholds to uncap based on estimated power saves and spikes
LP,HP :Current workload priority fraction
N : Number of servers in the row
T1cap ←false,T2cap ←false,Powerbrake ←false
loop
P ←NormalizedRowPowerReading
if P > 1.0 then
Powerbrake ←true
▷BMC sets powerbrake
T1cap ←true
T2cap ←true
else if P > T2 then
if T2cap == false then
T2cap ←true
LP GPUs to 1110 MHz
▷Start by capping only LP for T2
else
Cap HP GPUs to 1305 MHz
▷Cap HP subsequently if needed
else if P > T1 then
T1cap ←true
Cap LP to 1275 MHz
▷Cap LP to GPU base frequency for T1
if T2cap & P < T2−T2Buf fer then
Uncap HP servers
Change LP server caps to 1275 MHz
if T1cap & P < T1−T1Buf fer then
Remove frequency cap for all LP servers
6. Evaluation
We first show a sweep of various parameters in the POLCA
policy, and then demonstrate the efficacy of POLCA at over-
subscribing power to allow additional capacity for LLM infer-
ence, under the defined SLOs in Table 5.
6.1. Methodology
We implement a discrete event simulator to evaluate the degree
of oversubscription that we can support in a production LLM
inference cluster. Our simulator is optimized for a high-traffic
scenario, where it assumes that all the servers in the cluster
have the models loaded, and are serving inference.
Workloads. We evaluate the workloads as listed in Table 4.
We configure the BLOOM-176B model for different tasks
(summarization, search, and chat) based on their input/output
token sizes and priorities. Note that based on Section 2.3,
BLOOM-176B has the most performance impact from cap-
ping, making this our worst-case workload. Each workload
runs on a dedicated DGX-A100 server.
Replicating production traces. We use a six-week power
consumption trace between June 21st to August 2nd 2023
from the production inference cluster described in Table 2.
Based on this trace and the characteristics (i.e., power and
time per token) for the open-source models, we generate a
synthetic trace. This synthetic trace contains the arrivals for
each inference request, the number of tokens for the prompt
and the output. The Mean Absolute Percentage Error (MAPE)
between the synthetic and original power timeseries is within
3%. Figure 16 shows example traces. Note that we only show
subsets of the data and normalize the numbers in the figures
for confidentiality reasons. We use the first week to train the
parameters of our power capping policy and evaluate on the
subsequent five weeks.
6.2. Policy exploration on 1-week power trace
POLCA thresholds and additional servers. We search for
the power thresholds (T1 and T2) for POLCA that maximize
additional servers while meeting SLOs (refer to Table 5). We
incrementally add servers, monitoring low- and high-priority
latency and power brakes. We assume the workload distribu-
tion from Table 4 and present a subset of results in Figure 13.
T1 and T2 are set 10% apart to ensure that capping low-priority
workloads at T1 sufficiently avoids capping high-priority ones
at T2. As discussed in Section 5.1, based on the data in Table 2,
to allow for the maximum power spikes at 40s granularity to
be masked (11%), our T2 should be set at 89%. We therefore
add a T1-T2 combination of 80-89% to the mix.
The 75-85% and 80-89% T1-T2 combinations allow 35%
more servers without powerbrake (dashed gray line), while
85-95% permits only 32.5% more. 75-85% misses the SLOs
for low-poriority workloads by a huge margin, since it starts
capping them much earlier. On the other hand, 85-95% incurs
a lower performance impact on the LP and HP workloads,
but is in a much higher danger of leading to powerbrakes,
especially since the T2 is not far enough away from maxi-
mum power to avoid powerbrakes from the 40s spikes (11%).
To balance powerbrake avoidance and performance based on
SLOs, we select T1 = 80% and T2 = 89% for POLCA. With
these thresholds, we add 30% more servers (dashed red line
in Figure 13) to stay strictly within the performance SLOs for
LP and HP workloads.
Sweeping the capping frequency. In Figure 15a, we show
the performance impact of varying the capping frequency for
low-priority workloads at T1. Below 1275 MHz, we can no
longer meet the SLO for LP workloads. Therefore, we choose
1275 MHz, the base frequency of A100 as the low-priority
capping frequency at T1.
9


---

0
20
40
Added Servers (%)
1.00
1.02
1.04
1.06
1.08
Normalized
p50 Latency
Low Pri. p50
High Pri. p50
1.0
1.5
2.0
Normalized
p99 Latency
Low Pri. p99
High Pri. p99
(a) T1=75%, T2=85%.
0
20
40
Added Servers (%)
1.00
1.02
1.04
1.06
1.08
Normalized
p50 Latency
Low Pri. p50
High Pri. p50
1.0
1.5
2.0
Normalized
p99 Latency
Low Pri. p99
High Pri. p99
(b) T1=80%, T2=89%.
0
20
40
Added Servers (%)
1.000
1.025
1.050
1.075
1.100
Normalized
p50 Latency
Low Pri. p50
High Pri. p50
1
2
3
4
Normalized
p99 Latency
Low Pri. p99
High Pri. p99
(c) T1=85%, T2=95%.
Figure 13: Threshold space search. The dashed gray line shows the max servers without powerbrake
event. The dashed red line indicates adding 30% more servers.
0
20
40
Added Servers (%)
0.96
0.97
0.98
0.99
1.00
Normalized Throughput
Low Pri. Throughput
High Pri. Throughput
Figure 14: Server throughput for
POLCA.
1150
1200
1250
1300
SM frequency
1.00
1.02
1.04
1.06
1.08
Normalized
p50 Latency
Low Pri. p50
High Pri. p50
1.0
1.5
2.0
Normalized
p99 Latency
Low Pri. p99
High Pri. p99
(a) Impact of capping frequency for
low-priority workloads at T1.
0
25
50
75
100
Low Priority Servers (%)
1.02
1.04
1.06
Normalized
p50 Latency
Low Pri. p50
High Pri. p50
1.0
1.2
1.4
1.6
1.8
Normalized
p99 Latency
Low Pri. p99
High Pri. p99
(b) Impact of the fraction of low-
priority workloads.
Figure 15: Parameters sweeps for POLCA.
6.3. Evaluation on 6-week power traces
Power impact. Figure 16 shows the impact on daily power
utilization as we add the 30% more servers using POLCA.
The main insights are: (1) the power utilization average over 5
minutes follows the same pattern with a higher power offset,
and (2) the power spikes increase, since the absolute number
of workloads that can be triggered together increases.
Throughput impact. Our simulator assumes a one-request
buffer per server to simulate queueing delays. This is based on
the typical load balanced setup, reducing the chance of simul-
taneous capping. For the chosen thresholds, we present the
impact on throughput per service for the low- and high-priority
workloads. Figure 14 shows the high-priority workload re-
mains unaffected, while the low-priority throughput sees a
minor < 2% decline.
Impact of low-priority workloads. Since we prioritize cap-
ping low-priority workloads to avoid capping high-priority
workloads, the low-to-high priority distribution impacts work-
load performance. Figure 15 shows the impact on workload
performance as the low- to high-priority ratio in the cluster
changes. A decrease in low-priority workloads can lead to P99
latency of high-priority workloads exceeding SLO limits.
Comparison with other techniques.
We compare our
dual-threshold power capping policy against three base-
lines: (1) single threshold at 89% for low priority workloads
(1-Thresh-Low-Pri), (2) single threshold at 89% for all
workloads (1-Thresh-All), and (3) no capping (No-cap) All
baselines include a powerbrake as fallback for power failure
safety. The first four bars in Figure 17 show the performance
1
2
3
4
5
6
Time (days)
Power Utilization
Default servers (2s avg)
Default servers (5min avg)
30% more servers (2s avg)
30% more servers (5min avg)
Figure 16: Row-level power utilization timeseries using BLOOM,
generated based on production data. Y-axis hidden for confidentiality.
of various baselines normalized against POLCA.
1-Thresh-Low-Pri does not meet the low-priority SLOs,
since it directly reduces their frequency without gradual power
reduction. 1-Thresh-All breaches the P99 SLOs for both
low- and high-priority workloads, since it caps them aggres-
sively at the 89% threshold. The POLCA dual-threshold policy
prioritizes the performance of high-priority workloads at the
expense of the low priority workloads, still meeting SLOs for
both. No-cap lacks powerbrake protection, impacting P99
and P100 latency. This policy is comparable to POLCA under
expected conditions, but vulnerable to model power changes.
Impact of short-term changes in workloads. We simulate
the impact of workloads becoming more power-intensive than
profiled. This can happen as the same models are updated to be
more efficient. We uniformly increase the power per workload
by 5% and measure the robustness of each technique. The last
four bars in Figure 17 show this performance impact. POLCA
is the most robust maintaining SLOs despite the fast, inevitable
workload changes. As the changes become more prominent,
and newer models take over, we reconfigure POLCA.
Number of powerbrake events. Although Figure 17 shows
the performance impact of various policies, it is also important
to track the number of powerbrake events. The reason POLCA
targets and SLO of zero powerbrake events is to avoid the
alarms this can cause at the cluster level for the cloud provider.
Figure 18 shows the number of powerbrake events per policy,
for the regular, and scaled-up power usage workloads.
10


---

Low priority
p50 latency
High priority
p50 latency
Low priority
p99 latency
High priority
p99 latency
Low priority
Max latency
High priority
Max latency
0
1
2
3
4
Normalized Value
(Lower is better)
POLCA
1-Thresh-Low-Pri
1-Thresh-All
No-cap
POLCA+5%
1-Thresh-Low-Pri+5%
1-Thresh-All+5%
No-cap+5%
Figure 17: Performance impact of the dual-threshold POLCA with
other thresholding policies at 30% oversubscription.
Configuration
101
102
103
104
Number of power brakes
POLCA
1-Thresh-Low-Pri
1-Thresh-All
No-cap
POLCA+5%
1-Thresh-Low-Pri+5%
1-Thresh-All+5%
No-cap+5%
Figure 18: Number of power brakes triggered under each configura-
tion when running default and power-intensive workloads.
7. Discussion
We summarize a few additional opportunities to improve and
extend POLCA.
Extending power oversubscription insights beyond LLMs.
Unlike the prompt and token phase power patterns in genera-
tive LLMs, the vision and multi-modal deep learning inference
workloads exhibit relatively stable power consumption pat-
terns. However, they still display susceptibility to frequency
scaling interventions. Figure 19 unveils similar response pat-
terns in vision and multi-modal transformer models, indicating
the broader applicability of power oversubscription principles.
Workload-aware POLCA policy. Given the rise of inference-
as-a-service platforms, POLCA could be extended to use LLM
and use-case specific power profiles to reduce the impact on
performance, while getting the most power savings.
Using the POLCA stack to mitigate training power swings.
With Figure 9, we show that the power swings during large
training can be reduced at minimal performance loss using cap-
ping. The software stack that POLCA provides can be tuned
to handle this as per the training job size and performance
profiles.
Asynchronous training. LLM training incurs large power
swings due to the synchronous iterations required across many
GPUs. Lazy weight updates and asynchronous training could
help alleviate this challenge.
Phase-aware power management for generative LLMs.
Prompt phases are compute and power intensive, while to-
ken phases are not as we show in Section 2. Customizing
power-performance knobs on the GPU based on the inference
phase can yield substantial savings; for example, using lower
frequencies during the (longer) token phase can help reduce
the average power consumption, which in turn helps free up
more power to be oversubscribed.
0
5
10
15
20
Server Peak Power Reduction (%)
0
5
10
Perf. Reduction (%)
ConvNextV2
SwinV2
BLIP2
(a) Training.
0
5
10
15
20
Server Peak Power Reduction (%)
0
5
10
15
Perf. Reduction (%)
ConvNextV2
SwinV2
BLIP2
(b) Inference.
Figure 19: Peak power reduction (relative to server TDP) vs. perfor-
mance reduction for DL training and inference workloads at varying
GPU SM frequencies. The dashed black line shows a linear scaling
of performance with power drawn.
Better and standardized hardware support. The OOB
power management interfaces in GPUs today, are not only
slow, but completely non-standardized. This makes building
a stack that works across vendors almost impossible. With
faster, standardized out-of-band management interfaces, a lot
more power and performance optimizations at scale can be
achieved. In the absence of these though, there is still scope
for large application owners to use in-band interfaces for better
power and energy operating points.
8. Related Work
Our paper is the first to extensively characterize the power
patterns and power management knobs of modern LLMs in
GPU clusters and propose to enable safe and efficient power
oversubscription. We will discuss some of the related works.
Cluster power management. Many efforts seek to deploy the
maximum number of servers possible within power capacity
through power provisioning and management techniques, such
as power capping and frequency scaling, focusing solely on
CPUs [12, 26, 42, 45], power oversubscription[14, 15, 22],
and workload-aware placement[18, 59].
DL energy efficiency. Some works have looked at improving
energy efficiency for both training and inference workloads
through customized frameworks[9, 31, 32, 57] and system pa-
rameters[17]. Reducing average power or energy consumption
is different from reducing peak power, which is essential to
server provisioning decisions, that POLCA targets.
GPU and DL workload characterization. Many works have
characterized and analyzed DL workloads in GPU clusters [19,
21, 23] to understand utilization and performance. Others have
studied power behaviors[20, 58] and the implications of per-
formance under power management techniques of GPUs[39,
41, 52]. We are the first to extensively study the power charac-
teristics of generative LLMs, and peak power in general.
9. Conclusion
We introduce POLCA, a power oversubscription framework
for LLM inference clusters. With substantial characteriza-
tion, we showed that generative LLM inference workloads
11


---

have distinct power consumption patterns that allow for power
oversubscription. We characterized the effectiveness and limi-
tations of existing GPU frequency scaling and power capping
for LLM inference and training. Based on our insights, we
designed POLCA for safe and efficient power oversubscrip-
tion in LLM clusters despite unreliable and slow out-of-band
GPU power management interfaces, and ever-changing mod-
els. With production-based power simulations, we demon-
strated that it can increase the allocated server capacity by 30%
in existing inference clusters, while maintaining the SLOs.
This translates to an equivalent cost and carbon reduction due
to building fewer datacenters, while promptly providing much
needed cluster capacity to run additional LLM workloads.
References
[1]
International Energy Agency. 2022. Data Centres and Data Transmis-
sions Networks. Retrieved May 20, 2023 from https://www.iea.o
rg/reports/data-centres-and-data-transmission-netwo
rks.
[2]
2023. Amazon SageMaker. Amazon Web Services, (2023).
[3]
Reza Yazdani Aminabadi, Samyam Rajbhandari, Ammar Ahmad
Awan, Cheng Li, Du Li, Elton Zheng, Olatunji Ruwase, Shaden
Smith, Minjia Zhang, Jeff Rasley, and Yuxiong He. 2022. DeepSpeed-
Inference: Enabling Efficient Inference of Transformer Models at
Unprecedented Scale. In SC.
[4]
Alex Andonian, Quentin Anthony, Stella Biderman, Sid Black,
Preetham Gali, Leo Gao, Eric Hallahan, Josh Levy-Kramer, Connor
Leahy, Lucas Nestler, Kip Parker, Michael Pieler, Shivanshu Purohit,
Tri Songz, Wang Phil, and Samuel Weinbach. 2021. GPT-NeoX: Large
Scale Autoregressive Language Modeling in PyTorch. https://www
.github.com/eleutherai/gpt-neox.
[5]
2023. Azure Machine Learning - ML as a Service. Microsoft Azure,
(2023).
[6]
2022. Azure OpenAI Service. Microsoft Azure, (2022).
[7]
2023. Bard: An important next step on our AI journey. Google, (2023).
[8]
Emily M Bender, Timnit Gebru, Angelina McMillan-Major, and
Shmargaret Shmitchell. 2021. On the Dangers of Stochastic Parrots:
Can Language Models Be Too Big? In ACM Conference on Fairness,
Accountability, and Transparency (FAccT).
[9]
Sangjin Choi, Inhoe Koo, Jeongseob Ahn, Myeongjae Jeon, and
Youngjin Kwon. 2023. EnvPipe: Performance-preserving DNN Train-
ing Framework for Saving Energy. In USENIX ATC.
[10]
Hyung Won Chung, Le Hou, Shayne Longpre, Barret Zoph, Yi Tay,
William Fedus, Eric Li, Xuezhi Wang, Mostafa Dehghani, Siddhartha
Brahma, et al. 2022. Scaling instruction-finetuned language models.
arXiv preprint arXiv:2210.11416.
[11]
Jacob Devlin, Ming-Wei Chang, Kenton Lee, and Kristina Toutanova.
2019. Bert: Pre-training of deep bidirectional transformers for lan-
guage understanding. In NAACL.
[12]
Xiaobo Fan, Wolf-Dietrich Weber, and Luiz Andre Barroso. 2007.
Power Provisioning for a Warehouse-sized Computer. In ISCA.
[13]
International Institute for Strategic Studies. 2023. Large language mod-
els: fast proliferation and budding international competition. Strategic
Comments, 29. Paul Fraioli, (Ed.)
[14]
Xing Fu, Xiaorui Wang, and Charles Lefurgy. 2011. How much power
oversubscription is safe and allowed in data centers. In ICAC, 21–30.
[15]
Sriram Govindan, Jeonghwan Choi, Bhuvan Urgaonkar, Anand Siva-
subramaniam, and Andrea Baldini. 2009. Statistical profiling-based
techniques for effective power provisioning in data centers. In EuroSys.
[16]
Sylvain Gugger, Lysandre Debut, Thomas Wolf, Philipp Schmid,
Zachary Mueller, and Sourab Mangrulkar. 2022. Accelerate: Train-
ing and Inference at Scale Made Simple, Efficient and Adaptable.
https://github.com/huggingface/accelerate.
[17]
Miro Hodak, Masha Gorkovenko, and Ajay Dholakia. 2019. Towards
power efficiency in deep learning on data center hardware. In BigData.
[18]
Chang-Hong Hsu, Qingyuan Deng, Jason Mars, and Lingjia Tang.
2018. SmoothOperator: Reducing Power Fragmentation and Improv-
ing Power Utilization in Large-Scale Datacenters. In ASPLOS.
[19]
Qinghao Hu, Peng Sun, Shengen Yan, Yonggang Wen, and Tianwei
Zhang. 2021. Characterization and prediction of deep learning work-
loads in large-scale gpu datacenters. In SC.
[20]
Ali Jahanshahi, Hadi Zamani Sabzi, Chester Lau, and Daniel Wong.
2020. GPU-NEST: Characterizing energy efficiency of multi-GPU
inference servers. In CAL.
[21]
Myeongjae Jeon, Shivaram Venkataraman, Amar Phanishayee, Junjie
Qian, Wencong Xiao, and Fan Yang. 2019. Analysis of Large-Scale
Multi-Tenant GPU clusters for DNN training workloads. In USENIX
ATC.
[22]
Alok Gautam Kumbhare, Reza Azimi, Ioannis Manousakis, Anand
Bonde, Felipe Frujeri, Nithish Mahalingam, Pulkit A Misra, Seyyed
Ahmad Javadi, Bianca Schroeder, Marcus Fontoura, et al. 2021.
Prediction-Based Power Oversubscription in Cloud Platforms. In
USENIX ATC, 473–487.
[23]
Baolin Li, Rohin Arora, Siddharth Samsi, Tirthak Patel, William Ar-
cand, David Bestor, Chansup Byun, Rohan Basu Roy, Bill Bergeron,
John Holodnak, Michael Houle, Matthew Hubbell, Michael Jones,
Jeremy Kepner, Anna Klein, Peter Michaleas, Joseph McDonald, Lau-
ren Milechin, Julie Mullen, Andrew Prout, Benjamin Price, Albert
Reuther, Antonio Rosa, Matthew Weiss, Charles Yee, Daniel Edelman,
Allan Vanterpool, Anson Cheng, Vijay Gadepally, and Devesh Ti-
wari. 2022. AI-Enabling Workloads on Large-Scale GPU-Accelerated
System: Characterization, Opportunities, and Implications. In HPCA.
[24]
Shaohong Li et al. 2020. Thunderbolt: throughput-optimized, quality-
of-service-aware power capping at scale. In OSDI.
[25]
Shen Li, Yanli Zhao, Rohan Varma, Omkar Salpekar, Pieter Noordhuis,
Teng Li, Adam Paszke, Jeff Smith, Brian Vaughan, Pritam Damania, et
al. 2020. Pytorch distributed: experiences on accelerating data parallel
training. arXiv preprint arXiv:2006.15704.
[26]
Yang Li, Charles R Lefurgy, Karthick Rajamani, Malcolm S Allen-
Ware, Guillermo J Silva, Daniel D Heimsoth, Saugata Ghose, and
Onur Mutlu. 2019. A Scalable Priority-aware Approach to Managing
Data Center Server Power. In IEEE International Symposium on High-
Performance Computer Architecture (HPCA).
[27]
Yinhan Liu, Myle Ott, Naman Goyal, Jingfei Du, Mandar Joshi, Danqi
Chen, Omer Levy, Mike Lewis, Luke Zettlemoyer, and Veselin Stoy-
anov. 2019. RoBERTa: A Robustly Optimized BERT Pretraining Ap-
proach. arXiv preprint arXiv:1907.11692.
[28]
Alexandra Sasha Luccioni, Sylvain Viguier, and Anne-Laure Ligozat.
2022. Estimating the Carbon Footprint of BLOOM, a 176B Parameter
Language Model. arXiv preprint arXiv:2211.02001.
[29]
Meta. [n. d.] Introducing the AI Research SuperCluster — Meta’s
cutting-edge AI supercomputer for AI research. Retrieved Oct. 4, 2022
from https://ai.facebook.com/blog/ai-rsc/.
[30]
Microsoft. [n. d.] DeepSpeed: Model Implementations for Inference
(MII). Retrieved June 5, 2023 from https://github.com/micros
oft/DeepSpeed-MII.
[31]
Seyed Morteza Nabavinejad, Sherief Reda, and Masoumeh Ebrahimi.
2022. Coordinated batching and DVFS for DNN inference on GPU
accelerators. TPDS.
12


---

[32]
Bin Nie, Ji Xue, Saurabh Gupta, Christian Engelmann, Evgenia Smirni,
and Devesh Tiwari. 2017. Characterizing temperature, power, and
soft-error behaviors in data center systems: Insights, challenges, and
opportunities. In MASCOTS.
[33]
NVIDIA. [n. d.] Data Center GPU Driver. Retrieved Aug. 9, 2023
from https : / / docs . nvidia . com / datacenter / tesla / pdf
/NVIDIA_Data_Center_GPU_Driver_Release_Notes_450_v1
.pdf.
[34]
NVIDIA. [n. d.] DGX A100: The Universal System for AI Infrastruc-
ture. Retrieved June 5, 2023 from https://resources.nvidia.co
m/en-us-dgx-systems/dgx-ai.
[35]
[n. d.] NVIDIA A100 80GB PCIe GPU Product Brief. Retrieved Oct. 4,
2022 from https://www.nvidia.com/content/dam/en-zz/Sol
utions/Data-Center/a100/pdf/PB-10577-001_v02.pdf.
[36]
[n. d.] NVIDIA Data Center GPU Manager. Retrieved Oct. 4, 2022
from https://developer.nvidia.com/dcgm.
[37]
OpenAI. [n. d.] Scaling Kubernetes to 7,500 nodes. Retrieved Oct. 4,
2022 from https://openai.com/research/scaling-kubernet
es-to-7500-nodes.
[38]
Pratyush Patel, Zibo Gong, Syeda Rizvi, Esha Choukse, Pulkit Misra,
Thomas Anderson, and Akshitha Sriraman. 2023. Towards Improved
Power Management in Cloud GPUs. In CAL.
[39]
Tapasya Patki, Zachary Frye, Harsh Bhatia, Francesco Di Natale,
James Glosli, Helgi Ingolfsson, and Barry Rountree. 2019. Comparing
GPU Power and Frequency Capping: A Case Study with the MuMMI
Workflow. In WORK).
[40]
David Patterson, Joseph Gonzalez, Urs Hölzle, Quoc Le, Chen Liang,
Lluis-Miquel Munguia, Daniel Rothchild, David R So, Maud Texier,
and Jeff Dean. 2022. The Carbon Footprint of Machine Learning
Training Will Plateau, Then Shrink. Computer.
[41]
Martin Peres. 2013. Reverse engineering power management on
NVIDIA GPUs - A detailed overview. In XDC.
[42]
Pavlos Petoumenos, Lev Mukhanov, Zheng Wang, Hugh Leather, and
Dimitrios S. Nikolopoulos. 2015. Power capping: what works, what
does not. In 2015 IEEE 21st International Conference on Parallel and
Distributed Systems (ICPADS), 525–534. DOI: 10.1109/ICPADS.20
15.72.
[43]
[n. d.] PyTorch. Retrieved May 15, 2021 from https://www.pytor
ch.org.
[44]
Alec Radford, Jeffrey Wu, Rewon Child, David Luan, Dario Amodei,
Ilya Sutskever, et al. 2019. Language models are unsupervised multi-
task learners. OpenAI blog, 1, 8, 9.
[45]
Parthasarathy Ranganathan, Phil Leech, David Irwin, and Jeffrey
Chase. 2006. Ensemble-level power management for dense blade
servers. In ISCA.
[46]
Tirias Research. 2019. Why Your AI infrastructure Needs Both Train-
ing and Inference.
[47]
Teven Le Scao, Angela Fan, Christopher Akiki, Ellie Pavlick, Suzana
Ili´c, Daniel Hesslow, Roman Castagné, Alexandra Sasha Luccioni,
François Yvon, Matthias Gallé, et al. 2022. BLOOM: A 176b-
parameter open-access multilingual language model. arXiv preprint
arXiv:2211.05100.
[48]
Philipp Schmid. [n. d.] Fine-tune FLAN-T5 XL/XXL using Deep-
Speed & Hugging Face Transformers. Retrieved June 5, 2023 from ht
tps://www.philschmid.de/fine-tune-flan-t5-deepspeed.
[49]
Amazon Web Services. [n. d.] Amazon EC2 Update – Inf1 Instances
with AWS Inferentia Chips for High Performance Cost-Effective In-
ferencing. Retrieved June 5, 2023 from https://aws.amazon.com
/blogs/aws/amazon-ec2-update-inf1-instances-with-aw
s-inferentia-chips-for-high-performance-cost-effect
ive-inferencing/.
[50]
Mohammad Shoeybi, Mostofa Patwary, Raul Puri, Patrick LeGresley,
Jared Casper, and Bryan Catanzaro. 2019. Megatron-lm: training multi-
billion parameter language models using model parallelism. arXiv
preprint arXiv:1909.08053.
[51]
Dharma Shukla, Muthian Sivathanu, Srinidhi Viswanatha, Bhargav
Gulavani, Rimma Nehme, Amey Agrawal, Chen Chen, Nipun Kwa-
tra, Ramachandran Ramjee, Pankaj Sharma, et al. 2022. Singularity:
Planet-scale, preemptive and elastic scheduling of AI workloads. arXiv
preprint arXiv:2202.07848.
[52]
Prasoon Sinha, Akhil Guliani, Rutwik Jain, Brandon Tran, Matthew
D Sinclair, and Shivaram Venkataraman. 2022. Not all GPUs are
created equal: characterizing variability in large-scale, accelerator-rich
systems. In SC.
[53]
Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion
Jones, Aidan N Gomez, Łukasz Kaiser, and Illia Polosukhin. 2017.
Attention is all you need. Advances in neural information processing
systems, 30.
[54]
2023. Vertex AI. Google Cloud, (2023).
[55]
Thomas Wolf, Lysandre Debut, Victor Sanh, Julien Chaumond,
Clement Delangue, Anthony Moi, Pierric Cistac, Tim Rault, Rémi
Louf, Morgan Funtowicz, Joe Davison, Sam Shleifer, Patrick von
Platen, Clara Ma, Yacine Jernite, Julien Plu, Canwen Xu, Teven Le
Scao, Sylvain Gugger, Mariama Drame, Quentin Lhoest, and Alexan-
der M. Rush. 2020. Transformers: State-of-the-art Natural Language
Processing. In EMNLP.
[56]
Qiang Wu, Qingyuan Deng, Lakshmi Ganesh, Chang-Hong Hsu, Yun
Jin, Sanjeev Kumar, Bin Li, Justin Meza, and Yee Jiun Song. 2016.
Dynamo: Facebook’s Data Center-Wide Power Management System.
ACM SIGARCH Computer Architecture News.
[57]
Jie You, Jae-Won Chung, and Mosharaf Chowdhury. 2023. Zeus:
Understanding and Optimizing GPU Energy Consumption of DNN
Training. In NSDI.
[58]
Junyeol Yu, Jongseok Kim, and Euiseong Seo. 2023. Know Your
Enemy To Save Cloud Energy: Energy-Performance Characterization
of Machine Learning Serving. In HPCA.
[59]
Chaojie Zhang, Alok Gautam Kumbhare, Ioannis Manousakis, Deli
Zhang, Pulkit A Misra, Rod Assis, Kyle Woolcock, Nithish Ma-
halingam, Brijesh Warrier, David Gauthier, Lalu Kunnath, Steve
Solomon, Osvaldo Morales, Marcus Fontoura, and Ricardo Bianchini.
2021. Flex: High-Availability Datacenters With Zero Reserved Power.
In ISCA.
13
