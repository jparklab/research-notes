---
title: 'FineServe: A Fine-Grained Dataset and Characterization of'
id: fineserve-a-fine-grained-dataset-and-characterization-of
tags:
- internal-llm-inference-stacks-7f0c90
- workload-characterization
- multi-tenancy
- production-measurement
- moe
- capacity-planning
created: '2026-10-09T22:55:09.788461Z'
updated: '2026-10-10T14:52:17.066842Z'
source: https://arxiv.org/abs/2607.19349
source_domain: arxiv.org
fetched_at: '2026-10-09T22:55:09.788165Z'
fetch_provider: crawl4ai
status: review
type: note
tier: institutional
content_type: paper
deprecated: false
summary: 'Tianjin University researchers with PPIO Cloud (a Chinese multi-model GPU-serving
  marketplace) release FineServe, an in-the-wild, multi-model LLM serving workload
  dataset: 4 months, 55 models, 10 model series, 1.48 billion requests, covering both
  Dense Transformer and Mixture-of-Experts (MoE) architectures across 4 model-scale
  tiers and 10 distinct task intents (writing, coding, role-playing, etc.). This is
  a multi-tenant commercial GPU marketplace trace (not single-enterprise internal
  use) but directly informs capacity-planning/routing design that internal operators
  running heterogeneous multi-model fleets face. Comparison table against prior trace
  releases: ServeGen (4 months, 12 models, 3 series, 3.54B requests, no architecture/task-intent
  labeling) and BurstGPT (4 months, 2 models, 2 series, 5.29M requests, no architecture/scale/task
  labeling) -- FineServe is the only one of the three to label model architecture
  (dense vs MoE), scale tier, and task intent per request.


  KEY FINDINGS: workload arrival and token-length fluctuation regimes differ fundamentally
  by model architecture and scale -- dense vs MoE models exhibit distinct prefill/decode
  system behavior (MoE exploits expert parallelism for higher prefill utilization
  but suffers expert-imbalance/cross-device-communication contention during memory-bandwidth-bound
  decoding), and smaller models sustain higher concurrency with larger batches but
  show more demand-driven, volatile traffic with sharper and more frequent spikes
  than larger models. Task intent (chat/role-play vs coding vs others) materially
  changes prompt/response token geometry and latency sensitivity (e.g., multi-turn
  chat/role-play has stricter perceived TTFT requirements; coding requests show templated
  prompts and longer context carry-over), leading the authors to conclude that characterizing
  serving workloads by aggregate arrival rate alone is insufficient for capacity planning.
  The paper builds a workload generator from these fine-grained patterns for benchmarking
  multi-model routing/scheduling/capacity-planning strategies. Dataset and generator
  released at github.com/hihiztc1/FineServe.'
raw_file: raw/fineserve-a-fine-grained-dataset-and-characterization-of.pdf
doi: arXiv:2607.19349
utility_score: 16.0
citation_count: 0
venue: arXiv.org
is_retracted: false
---

FineServe: A Fine-Grained Dataset and Characterization of
Global LLM Serving Workloads
Tiancheng Zhang∗
Shaoyuan Huang∗
tianchengzhang@tju.edu.cn
hsy_23@tju.edu.cn
Tianjin University
Tianjin, China
Mingyuan Wang
Tianjin University
Tianjin, China
mingyuanwang@tju.edu.cn
Yunfeng Zhao†
Tianjin University
Tianjin, China
yfzhao97@tju.edu.cn
Xiaofei Wang
Tianjin University
Tianjin, China
xiaofeiwang@tju.edu.cn
Wenyu Wang
PPIO Cloud (Shanghai) Co., Ltd.
Shanghai, China
wayne@pplabs.org
Abstract
Large language models (LLMs) are increasingly deployed as always-
on online services, making efficient LLM serving a critical sys-
tems challenge. Achieving low latency and high throughput under
volatile demand requires deep understanding of real-world serv-
ing workloads, yet existing studies often rely on proxy traces or
coarse-grained characterizations that fail to capture the heterogene-
ity of modern multi-model LLM platforms. We present FineServe,
an in-the-wild, multi-model LLM serving workload dataset col-
lected from a global commercial marketplace, enabling fine-grained
characterization of real-world serving dynamics across heteroge-
neous models and tasks. Leveraging FineServe, we conduct a com-
prehensive analysis of arrival dynamics and token behavior, re-
vealing fundamentally different fluctuation regimes across model
architectures, scales and task intents. Building on these insights,
we develop the FineServe workload generator, which composes
fine-grained model-aware workloads into configurable mixtures
tailored for benchmarking multi-model serving platforms. By ex-
posing these fine-grained workload dynamics, FineServe provides a
realistic foundation for evaluating routing, scheduling, and capacity-
planning strategies in LLM serving systems. FineServe is available
at https://github.com/hihiztc1/FineServe.
Keywords
LLM Serving, Workload Trace, Workload Management
1
Introduction
Large language models (LLMs) have quickly become a central pillar
of modern AI services [8, 25], powering applications such as code
assistants, conversational robots, E-commerce [28], and emerging
agentic workflows [32]. With the growing deployment of LLMs for
always-on online services [20], LLM serving efficiency has turned
into a critical systems problem: providers must sustain low latency
and high throughput under volatile demand while keeping GPU
costs under control. This has sparked intense efforts to optimize
∗Both authors contributed equally to this research.
†Corresponding author.
LLM serving stacks [6, 21], including scheduling and batching poli-
cies [34], KV-cache management, and prefill-decode disaggrega-
tion [36], as well as elastic scaling in multi-model cloud deploy-
ments, where performance improvements translate directly into
substantial cost savings and better quality of service (QoS).
Realistic serving workload traces are central to this innovation
cycle: they not only drive system design by revealing the actual
operating regimes, but also provide the credible basis for evalu-
ating routing, scheduling, and capacity-planning strategies. Exist-
ing LLM serving research has relied on adapted or proxy work-
loads derived from general deep-learning and cloud-computing
traces [11, 13, 30], such as treating serverless function invocations
as inference requests, as well as more recent approaches combining
open-source LLM datasets (e.g., ShareGPT) with synthetic arrival
processes [24]. While useful, these proxies often impose simpli-
fied arrival assumptions (e.g., stationary Poisson-like processes),
which miss the non-stationary and bursty behaviors seen in pro-
duction and can distort system evaluation and derived insights.
Recent efforts have taken important steps toward grounding LLM
serving research in real-world traces. BurstGPT [29] releases a real-
world serving trace and highlights pronounced burstiness and non-
stationarity. ServeGen [33] provides a broader, production-scale
characterization across LLMs, emerging multimodal and reasoning
models, highlighting that both arrival dynamics and token-length
distributions can vary substantially across serving settings.
However, existing traces and characterizations still fall short for
today’s evolving LLM serving ecosystem in two key aspects:
(1) Insufficient workload granularity. With the rapid proliferation
of LLMs, especially high-quality open-source models [2, 14, 26]
and increasingly diverse user demands, LLM serving platforms are
shifting toward multi-model deployments [4, 19]. They host het-
erogeneous model portfolios that differ in architecture (e.g., Dense
vs. Mixture-of-Experts), scale, capability, and cost. Meanwhile, user
requests span diverse task intents (e.g., writing, coding, reasoning,
and role-playing), whose input/output token patterns and latency
sensitivities can vary significantly. Yet many existing traces and
analyses [12, 23, 29, 33] do not provide sufficient visibility into how
workload signatures differ across these fine-grained characteristics,
making it difficult to reason about multi-model system behaviors.
arXiv:2607.19349v1  [cs.AI]  17 Apr 2026


---

Zhang and Huang, et al.
(2) Limited capture of market-driven dynamics. Most available
datasets are collected under provider-specific offerings, where the
served model portfolio and user behavior appear relatively stable
within curated production settings. In contrast, in market-driven
environments, user choice and cost-performance tradeoffs, together
with rapid model turnover, can trigger sharp, time-localized shifts in
demand. For example, the release of a new model or a sudden change
in availability may quickly re-route traffic, causing request surges,
shifting token-length profiles, and amplifying system overhead.
As a result, the community still lacks an in-the-wild dataset
that enables comprehensive, fine-grained characterization of multi-
model and multi-task serving workloads. We present the first in-the-
wild multi-model LLM serving workload dataset with fine-grained
system observability from a global commercial marketplace. Our
dataset records privacy-preserving service-level metadata, includ-
ing timestamps, model identifiers, and input/output token counts
enabling systematic characterization along multiple heterogene-
ity axes: model architecture, scale tier, and task intent. Using this
dataset, we conduct a comprehensive measurement study that or-
ganizes the analysis around system-critical dimensions (request ar-
rivals, token geometry, and latency) and distills seven key findings
that reveal distinct, axis-dependent workload signatures obscured
by aggregate views. Finally, we introduce FineServe, a lightweight
workload generation toolkit that parameterizes fine-grained work-
loads across models and tasks and composes them into configurable
mixtures, supporting both faithful trace replay and scalable synthe-
sis for benchmarking.
In summary, this paper makes the following contributions:
• We release an in-the-wild, privacy-preserving multi-model LLM
serving dataset with fine-grained service observability.
• We provide a fine-grained characterization across model architec-
tures, scale tiers, and task intents, and reveal seven key findings
that expose workload signatures in request arrivals and token
geometry, directly informing routing, scheduling, and capacity
planning for multi-model serving.
• We release FineServe, a practical workload generation frame-
work supporting both trace replay and parametric synthesis,
that operationalizes these signatures as configurable knobs for
realistic, reproducible multi-model benchmarking.
2
Preliminary and Motivation
2.1
LLM Characteristics in the Wild
Model architecture diversity. Modern LLM serving platforms in-
creasingly host models with heterogeneous architectures, primarily
Dense Transformers [3, 27] and Mixture-of-Experts (MoE) mod-
els [5, 15, 22]. Dense models activate all parameters for each token,
whereas MoE routes tokens to a subset of experts, leading to distinct
system behaviors across serving stages. In the compute-intensive
prefill stage, MoE can exploit expert parallelism to achieve higher
utilization, while in the memory-bandwidth-bound decoding stage,
expert imbalance and cross-device communication may introduce
additional contention and latency. Consequently, architectural dif-
ferences can materially affect throughput, latency and tail-latency
sensitivity under bursty, multi-tenant workloads.
Table 1: Comparison between FineServe and prior charac-
terizations of LLM serving workloads.
FineServe (Ours)
ServeGen [33]
BurstGPT [29]
Characterization > Scale
Duration
4 months
4 months
4 months
#Models
55
12
2
#Series
10
3
2
#Requests
1.48B
3.54B
5.29M
Characterization > Scope
Architecture
✓(Dense & MoE)
×
×
Scale tiers
✓(4 bins)
✓(3 bins)
×
Task intents
✓(10)
×
×
Workload Generation
Approach
Fine-grained
Parameterized
Parameterized
parameterized blending
clients
burstiness
Model scale diversity. Serving workloads also span a wide
range of model scales, commonly indexed by parameter count (e.g.,
7B/13B/70B/>100B) and, in practice, by the resulting hardware foot-
print (e.g., GPU memory demand, tensor/pipeline parallel degrees,
and achievable batch sizes). Larger models generally increase com-
pute per token and memory pressure, amplify KV-cache footprint
during decoding, and tighten batching and scheduling constraints.
In contrast, smaller models can sustain higher concurrency with
larger batches, yet their workloads tend to be more demand-driven
and volatile, exhibiting sharper and more frequent spikes.
Task-intent diversity. User requests span diverse task intents
such as writing, coding, and role-playing, each inducing distinct
prompt structures, input/output token geometry, and latency sensi-
tivity. For example, interactive chat and role-playing often involve
multi-turn exchanges and stricter perceived time-to-first-token re-
quirements, while coding requests may exhibit templated prompts,
structured outputs, and longer context carry-over. These intent-
driven differences imply that characterizing serving workloads
purely by aggregate arrival rates is insufficient.
2.2
Towards Fine-grained Serving Workload
Workload characterization and generation. Optimization of
LLM serving systems can yield substantial performance improve-
ments and cost reductions [17, 18, 35], yet doing so hinges on a deep
understanding of real-world serving workloads. Such understand-
ing is particularly critical for today’s multi-model platforms, which
simultaneously host heterogeneous model portfolios and diverse
task intents. Table 1 summarizes representative production-scale
characterizations. While recent datasets provide valuable evidence
on request burstiness and token-length behaviors [29, 33], they still
fall short of capturing the fine-grained heterogeneity inherent to
multi-model serving systems. As shown by the comparison, state-of-
the-art characterizations do not expose workload signatures jointly
along key axes such as architecture, scale tiers, and task intents,
and therefore leave many cross-cutting patterns uncharacterized.
Motivated by this, we perform a comprehensive production-scale
characterization of in-the-wild serving workloads on a multi-model
deployment platform. Further, we build a workload generation


---

FineServe: A Fine-Grained Dataset and Characterization of Global LLM Serving Workloads
module that parameterizes and fits the sub-workloads induced by
fine-grained axis including: architecture, scale, and task-specific
components and then blends them into configurable mixtures, en-
abling practitioners to instantiate realistic workloads that preserve
the fine-grained signatures observed in production.
Workload source. Our characterization is grounded in real in-
ference workloads collected from a large-scale, global multi-model
serving platform built on an open, distributed compute market-
place. The platform maintains a catalog of more than 60 foundation
models and continuously incorporates newly released models, lead-
ing to rapid demand shifts driven by model quality, pricing, and
emerging trends. It aggregates heterogeneous GPU resources across
over 4,000 distributed compute nodes worldwide, comprising on
the order of 104 GPUs, and exposes unified pay-as-you-go APIs and
on-demand instances, including containerized and serverless GPUs,
allowing users to invoke diverse models and capacities without
managing the underlying infrastructure.
As summarized in Table 1, we analyze four months of produc-
tion traces comprising 1.48B requests across 57 distinct models
from 10 model families (detailed in the Appendix D). The traces
are collected from the platform’s backend inference logging sys-
tem and record privacy-preserving service-level metadata for each
request, including arrival time, input and output token lengths,
and the associated model architecture and scale. We analyze Dense
models (<10B, 10–30B, 30–70B) and MoE models (>100B), exclud-
ing smaller MoE models due to limited samples. All data are fully
sanitized and contain no user-identifying content, retaining only
metadata-level signals required for workload characterization in
compliance with privacy requirements. As a preliminary overview,
Fig. 1 presents the weekly request distribution for a representa-
tive month and the aggregated daily distribution across all models.
Owing to the global coverage of the platform, we do not observe
pronounced diurnal or tidal patterns in request volume.
0
06
12
18
Hour of Day
0
1000
2000
Request Count
(b) Diurnal distribution of requests
Mon Tue Wed Thu Fri
Sat Sun
Day of Week
49
50
51
52
Week of  Year
(a) Weekly–daily distribution of requests
6
7
8
1e6
24
Figure 1: Temporal distribution of FineServe.
3
Characterizing Architectures and Scales
This section characterizes serving workloads across the model ar-
chitectures and scale tiers summarized in Table 1. We focus on two
critical dimensions: request arrival patterns (§3.1) and input–output
token geometry(§3.2), as they jointly determine queuing pressure,
batching opportunities, KV-cache footprint, and ultimately end-to-
end performance in LLM serving.
Architectures and scales shape these dimensions through both
market and system mechanisms. On the market side, architecture
and parameter size affect inference cost and achievable latency,
thereby influencing pricing and user model selection; consequently,
MoE >100B
0.0
0.2
0.4
0.6
0.8
1.0
KS statistic
0.00
0.05
0.10
0.15
0.20
0.25
0.30
Dense <10B
Dense 10–30B Dense 30–100B
Dense <10B
Dense 10–30B
Dense 30–100B
MoE >100B
(a) Distribution of two-sample KS statistics across architecture-scale groups.
(b) W1 distances between adjacent windows across architecture-scale groups.
0
10
20
30
W1 distance
Figure 2: Long-term request distribution shifts measured by
two-sample KS statistic and W1 distances.
different model families experience distinct request arrival patterns
rather than a single homogeneous arrival process. On the system
side, model architecture and scale also shape token geometry, as dif-
ferent model families and sizes exhibit distinct input-output length
profiles (in particular, output-length variability and tail behavior).
3.1
Request Arrival Pattern
We characterize request arrivals from two complementary perspec-
tives. 1) long-term distribution shifts capture how the arrival distri-
bution evolves over days to months, reflecting persistent changes
in demand composition that can invalidate stationary workload
assumptions and complicate capacity planning. 2) short-term bursti-
ness and fluctuation describes within-window and second-level vari-
ability, which directly drives real-time scheduling such as queue
buildup, batching dynamics, and tail-latency amplification.
3.1.1
Long-term Distribution Shifts. To quantify long-term distri-
bution shifts, we track distributional changes between consecutive
time windows over the full observation period.
Metrics and Measurement Methodology. Specifically, for
each architecture-scale group, we partition the per-second request-
count trace into fixed-length windows of 𝑇= 300 seconds (5 min-
utes). Let x𝑖∈R𝑇denote the request-count samples in 𝑖-th window:
x𝑖= {𝑥𝑖(𝑡)}𝑇
𝑡=1,
𝑖= 0, 1, . . . , 𝑁−1,
(1)
where 𝑁is the total number of windows across the entire measure-
ment period. We then compare consecutive windows (𝑖,𝑖+1) and
compute the following metrics:
(1) Two-sample Kolmogorov–Smirnov (KS) statistic. Let b𝐹𝑖(·) and
b𝐹𝑖+1(·) denote the empirical cumulative distribution functions (CDFs)
of samples in x𝑖and x𝑖+1, respectively. We compute the two-sample
Kolmogorov–Smirnov statistic as
KS𝑖≜sup
𝑥
b𝐹𝑖(𝑥) −b𝐹𝑖+1(𝑥)
 .
(2)
In practice, we use the standard two-sample KS test and record
its statistic value KS𝑖∈[0, 1]. A larger KS𝑖indicates a stronger
distributional discontinuity between two adjacent windows.
(2) Wasserstein-1 (W1) distance. To quantify the overall magnitude
of workload changes, we additionally compute the W1 distance


---

Zhang and Huang, et al.
between adjacent windows:
W1𝑖≜𝑊1

b𝐹𝑖, b𝐹𝑖+1

.
(3)
The W1 distance measures the minimal transport cost to transform
the request-count distribution in window 𝑖into that in window 𝑖+1.
Analysis across Architectures and Scales. As illustrated in
Fig. 2, Dense models below 10B parameters exhibit the strongest
long-term distribution shifts across the two metrics. Their KS statis-
tics are large and heavy-tailed, indicating substantial deviations in
distributional shape. Besides, the W1 distances reveal that the drift
is often severe, with shift magnitudes spanning orders of magnitude.
This behavior aligns with the market positioning of small Dense
models. Their low deployment cost and short inference latency
make them attractive for a wide range of heterogeneous applica-
tions, resulting in broad and disruptive workload shifts. As model
scale increases, Dense models exhibit improved workload stabil-
ity. Both KS statistics and W1 distances decrease, indicating that
distributional changes become less extreme.
MoE models demonstrate a markedly different profile. Their KS
statistics and W1 distances remain consistently low, suggesting
limited long-term drift in both shape and magnitude. These mild
distributional adjustments are consistent with their more special-
ized and higher-cost application scenarios.
Finding 1: Although request arrivals are not directly deter-
mined by model architecture, architectural and scale choices
shape market adoption and usage patterns, leading to systematic
differences in long-term distribution shifts, with Dense models,
especially small ones, exhibit larger and more severe distribution
shifts, while large MoE models remain comparatively stable.
3.1.2
Short-term Burstiness and Fluctuation. Long-term drift de-
scribes how arrival distributions evolve across time, but serving
systems must also react to second-level burstiness and jitter that
directly drive queue buildup and tail latency. In practice, short-term
burstiness is multi-faceted: arrivals can be uneven within a window
(high peak-to-mean disparity) and highly oscillatory at fine tempo-
ral granularity. We therefore characterize short-term dynamics at
per-second resolution using complementary metrics.
Metrics and Measurement Methodology. We quantify short-
term burstiness over 5-minute windows (𝑇=300) using:
Coefficient of Variation (CV). For each window, we compute the
CV of per-second request counts, which measures relative disper-
sion and captures how uneven arrivals are within a window. No-
tably, CV is order-invariant and does not encode temporal ordering.
Mean Squared Successive Difference (MSSD). To characterize fine-
grained fluctuations, we compute
MSSD =
1
𝑇−1
𝑇−1
∑︁
𝑡=1
(𝑥𝑡+1 −𝑥𝑡)2,
(4)
where {𝑥𝑡}𝑇
𝑡=1 are per-second request counts in the window. MSSD
measures the step-to-step change in arrivals, i.e., how strongly the
request rate fluctuates between adjacent seconds. MSSD is order-
aware and sensitive to rapid fluctuations and short-lived spikes.
Analysis across Architectures and Scales. Fig. 3(a) presents
diurnal profiles of CV and MSSD for different architecture-scale
0.25
0.50
0.75
CV
1
2
CV
1.00
1.25
CV
Mon
Tue
Wed
Thu
Fri
Sat
Sun
Day of week
2
4
CV
0
6 12 18 24
0.2
0.4
0.6
CV
1.2
1.4
1.6
1.8
0.9
1.0
1.1
1.2
1.3
2.0
2.5
3.0
3.5
Hour of day
Dense <10B
Dense 10–30B
Dense 30–100B
MoE >100B
0
6 12 18 24
0
6 12 18 24
0
6 12 18 24
50
100
150
200
MSSD
0.6
0.8
1.0
1.2
1.4
1.5
2.0
0.2
0.3
0.4
(a) Daily evolution of CV and MSSD.
(b) Weekly evolution of CV.
Figure 3: Short-term workload burstiness and fluctuation.
groups. Across all groups, both metrics exhibit clear diurnal patterns
aligned with user activity cycles. However, the magnitude and
nature of burstiness differ substantially.
Dense models below 10B parameters consistently exhibit low
CV but high MSSD, indicating that their arrivals are relatively even
in magnitude within each 5-minute window, yet fluctuate sharply
on a finer timescale (second-level). In contrast, MoE models show
high CV but comparatively lower MSSD, reflecting highly uneven ar-
rivals dominated by large-amplitude bursts, while the second-level
fluctuations are smoother. Interestingly, MSSD reaches its mini-
mum around midday and peaks during late-night hours across all
groups. This indicates that high-load periods tend to be temporally
smoother, whereas low-load periods are dominated by sporadic and
irregular arrivals, which in turn amplify jitter.
Fig. 3(b) further shows that these short-term regimes persist
over the week. The ordering of CV across model groups remains
remarkably stable across the week: Dense <10B maintains the low-
est CV, whereas MoE remains the highest. This stability indicates
that the observed burstiness patterns are not driven by isolated
anomalous days, but represent inherent properties of how different
model classes are used in production.
Beyond characterizing local burstiness and fluctuation patterns,
we further quantify the system-level importance of bursty arrivals
by measuring how much traffic mass is concentrated in extreme
spikes. As shown in Fig. 4, across all four architecture-scale groups,
extreme seconds contribute a disproportionately large share of
hourly traffic: even for the least burst-dominant category, the top 5%
seconds account for nearly 9.5% of total requests, underscoring the
non-negligible role of short-lived bursts in shaping aggregate load.


---

FineServe: A Fine-Grained Dataset and Characterization of Global LLM Serving Workloads
0
2
4
6
8
10
12
14
16
18
20
22
Hour of day
Dense <10B
Dense 10–30B
Dense 30–100B
MoE >100B
0.05
0.10
0.15
Figure 4: Top 5% burst contribution by hour.
This effect varies across model groups. Dense models, especially
those under 10B parameters, exhibit the highest burst contribution,
where the top 5% seconds can account for up to 15–18% of hourly
requests during certain periods. This observation is consistent with
our earlier finding that Dense workloads are dominated by frequent
short-term fluctuations.
Finding 2: Short-term burstiness and fluctuations are multi-
dimensional and architecture-dependent, with Dense models
dominated by high-frequency fluctuations and low-amplitude
bursts, while MoE models are characterized by lower-frequency,
high-amplitude bursts.
3.1.3
Request Arrival Modeling. Our analysis shows that LLM re-
quest arrivals are both non-stationary and bursty, and these proper-
ties vary systematically across model architectures and scales. At
coarse time scales, arrival intensity and distribution shift over time;
at fine time scales, requests cluster within short intervals, leading to
over-dispersion and pronounced bursts. These observations moti-
vate an architecture- and scale-aware, multi-level arrival modeling
for credible benchmarking of multi-model serving systems.
Prior work such as BurstGPT models request arrivals by fit-
ting a single inter-arrival distribution within fixed-length windows
(e.g., 20 minutes), which captures coarse-grained arrival rates but
overlooks short-term burstiness at fine time scales. To address this
limitation, FineServe adopts a piecewise arrival modeling strat-
egy that combines Gamma-based modeling [16, 31] with Negative
Binomial (NB) modeling [9, 10] of fine-grained request counts.
We partition the request trace into fixed-length windows of
𝑇= 300 seconds, extract inter-arrival time (IAT) samples {𝑥(𝑠)
𝑖
}
within each window, and fit their distribution with a Gamma model:
𝑥(𝑠) ∼Gamma(𝛼𝑠, 𝛽𝑠),
(5)
where 𝛼𝑠and 𝛽𝑠denote shape and scale parameters for window 𝑠.
Fig. 5 reports the temporal evolution of the fitted Gamma param-
eters, which implies the key arrival behaviors: the shape parameter
𝛼𝑠captures arrival regularity, whereas the scale parameter 𝛽𝑠re-
flects request sparsity. Across architecture-scale groups, the fitted
parameters differ systematically, indicating distinct usage patterns.
Specifically, small Dense models exhibit relatively low 𝛼𝑠with small
𝛽𝑠, consistent with frequent and loosely structured arrivals; larger
Dense models show higher 𝛼𝑠and much larger 𝛽𝑠, implying sparser
yet more regular arrivals. MoE models fall in between, with moder-
ate sparsity and pronounced temporal non-stationarity, consistent
with more workload-driven invocation patterns in production.
Gamma-based inter-arrival modeling captures segment-level
request timing but fails to represent fine-grained clustering when
multiple requests arrive within the same instant. To check whether
16
0.28
0.30
Value of 𝛼!
Dense <10B
0.250
0.275
0.300
Value of 𝛼!
Dense 10–30B
Time
0.50
0.75
1.00
Value of 𝛼!
Dense 30–100B
Time
0.30
0.35
0.40
Value of 𝛼!
MoE >100B
200
400
Value of β!
2000
3000
4000
Value of β!
750
1000
1250
0
4
8
12
20
24
Value of β!
60
80
16
0
4
8
12
20
24
16
0
4
8
12
20
24
16
0
4
8
12
20
24
Value of β!
𝛼!
β!
Figure 5: Temporal evolution of Gamma arrival parameters.
such clustering matters, we analyze arrival counts at millisecond
granularity. As shown in Table 2 in the Appendix A, only the Dense
<10B group exhibits clear over-dispersion, with a variance-to-mean
ratio > 1, indicating short-term request aggregation, while other
model groups are too sparse at the millisecond scale, where fine-
grained burstiness is negligible.
Accordingly, FineServe adopts a conditional strategy. For architecture-
scale groups with over-dispersed millisecond-level counts, we dis-
cretize time into Δ=1 ms slots and model the per-slot arrival count
𝑛𝑠,𝑡within window 𝑠using an NB distribution:
𝑛𝑠,𝑡∼NB(𝑟𝑠, 𝑝𝑠),
(6)
where𝑟𝑠and 𝑝𝑠control arrival dispersion and intensity, respectively.
This formulation naturally allows multiple requests to arrive within
the same time slot and captures short-term bursty aggregation.
Finding 3: In heterogeneous multi-model serving, modeling re-
quest arrivals with Gamma-distributed IAT alone is insufficient,
accurately reproducing short-term concurrency requires addi-
tionally modeling millisecond-scale arrival counts (e.g., with an
NB model).
3.1.4
Implications for LLM Serving and Elastic Cloud Systems. Long-
term distribution shifts. Arrival distributions exhibit pronounced
shifts over days to months, invalidating stationary assumptions
and necessitating elastic policies that adapt provisioning and rout-
ing as demand evolves. On multi-model platforms, these shifts are
architecture- and scale-dependent. Treating all models as expe-
riencing identical drift can misallocate capacity, causing under-
provisioning of volatile small Dense groups with resulting overload
and tail-latency spikes, while over-provisioning comparatively sta-
ble large MoE groups and wasting capacity.
Short-term burstiness. Burstiness is inherently multi-dimensional.
Dense workloads are dominated by high-frequency second-level jit-
ter, whereas MoE workloads exhibit lower-frequency, high-amplitude
bursts that concentrate load within short intervals. A single vari-
ability metric is therefore insufficient: focusing only on burst mag-
nitude misses Dense jitter, while focusing only on jitter misses
MoE spikes. These differences suggest architecture-aware adapta-
tion: fast-reacting, jitter-aware scheduling for Dense models and
capacity-aware, burst-resilient provisioning for MoE models.


---

Zhang and Huang, et al.
Token Count
0
1000
2000
3000
4000
5000
6000
7000
10 −6
10 −5
10 −4
10 −3
10 −2
10 −1
10 −7
10 −5
10 −3
10 −1
Probability Mass per Bin
Dense <10B
Dense 30-100B
10 −6
10 −5
10 −4
10 −3
10 −2
10 −1
Probability Mass per Bin
0
1000
2000
2500 3000 3500
Token Count
0
1000
2000
4000
6000
10 −6
10 −5
10 −4
10 −3
10 −2
10 −1
Dense 10-30B
0
1000
2000
4000
6000
10 −14
10 −11
10 −8
10 −5
10 −2
MoE >100B
0
1000
2000
Token Count
5000
10000
Dense 30-100B
Input Tokens
Output Tokens
Log-normal fit (Input)
Dense <10B
Figure 6: Input and output length marginal distribution.
3.2
Input-output Token Geometry
Request arrivals determine when workload enters the system, but
token geometry determines how much computation each request
requires. We characterize input and output token lengths across
architecture-scale groups, examining both their marginal distribu-
tions and their conditional relationships.
3.2.1
Marginal Distributions. Fig. 6 presents token-length histograms
as probability mass per bin, well-suited to the heavy-tailed distri-
butions observed in real-world workloads. Across all architecture-
scale groups, input lengths exhibit substantially extended tails com-
pared to those reported in BurstGPT, indicating that long-context
prompts have become increasingly common. In contrast, output
lengths decay more rapidly in the tail, although long generations
remain non-negligible. Dense models tend to produce more con-
centrated outputs, whereas MoE models exhibit visibly heavier
output tails. To enable scalable workload synthesis, we explicitly
model input token length distributions. Unlike Zipf assumptions
used in BurstGPT, our empirical data exhibit stable unimodal struc-
tures with smoother tail decay on the logarithmic scale, motivating
log-normal input length modeling:
𝑃(Input) = Log-normal(𝜇, 𝜎2),
(7)
where 𝜇and 𝜎denote the mean and standard deviation of the
logarithm of input token lengths.
Finding 4: Token distributions reveal a clear transition toward
long-context regimes. Input tails extend substantially beyond
prior reports, while MoE models exhibit heavier output tails than
Dense models. Log-normal distributions capture both central
mass and tail behavior more faithfully than Zipf assumptions.
3.2.2
Input-Output Correlation. While marginal distributions char-
acterize overall statistics, they cannot capture dependencies be-
tween input and output lengths. Fig. 7 reveals architecture-specific
patterns through median output trajectories across input bins.
Contrary to simple monotonic assumptions, Dense models, par-
ticularly those below 10B parameters, exhibit a pronounced non-
monotonic, inverted-bowl relationship between input and output
lengths. As input length increases, the median output grows from
short contexts to a peak at moderate inputs around 1.5K–2K tokens,
and then declines substantially for longer contexts, stabilizing at
much shorter outputs for very long prompts. From the peak to
the long-input regime, the median output length drops by more
than 60%. Larger Dense models show similar but attenuated trends:
Dense 30–100B models exhibit a weaker inverted-bowl pattern with
lower peak outputs, while Dense 10–30B models largely approach
near-monotonic behavior.
This behavior may reflect a combination of workload seman-
tics and architectural effects. As input length increases, requests
increasingly correspond to summarization or condensation scenar-
ios that favor concise outputs, whereas moderate-length inputs
more often elicit generative tasks requiring expanded responses.
At the same time, Dense architectures appear to compress informa-
tion from long contexts through full-parameter attention, distilling
salient content rather than expanding outputs proportionally. In
contrast to the roughly linear input–output correlation reported in
BurstGPT, our observations suggest that input–output geometry is
strongly shaped by model architecture and scale.
By contrast, MoE models exhibit a monotonic growth–saturation
pattern. Median output length increases steadily with input length
and saturates at high-context regimes, reflecting MoE’s sparse ex-
pert routing that allows longer inputs to activate more diverse
expert combinations rather than being aggressively compressed.
This architectural contrast leads to a crossover effect: Dense mod-
els produce longer outputs at moderate input lengths, while MoE
models dominate in long-context regimes. Within Dense models,
parameter scale introduces secondary modulation.
Finding 5: Input-output relationships are fundamentally
architecture-dependent and non-monotonic. Dense models ex-
hibit inverted-bowl patterns (peak at moderate inputs, substan-
tial decline for long contexts), while MoE models show sustained
growth (3.5–4×) with saturation.
0
1000 2000 3000 4000 5000 6000 7000
0
200
400
600
800
1000
Output Length
Dense <10B
0
2000
4000
6000
8000
10000
Input Length
0
100
200
300
400
500
600
MoE >100B
Input Length
0
100
200
300
400
500
600
Output Length
Dense 30-100B
0
1000 2000 3000 4000 5000 6000 7000
0
100
200
300
400
500
600
Dense 10-30B
0
500 1000 1500 2000 2500 3000 3500 4000
25-75 Percentile
Median
Mean Output Length
Figure 7: Input-output correlation across model categories.


---

FineServe: A Fine-Grained Dataset and Characterization of Global LLM Serving Workloads
4
Characterizing Tasks
Different task intents pursue different end goals and require differ-
ent types and amounts of information, which can lead to systemat-
ically distinct input and output characteristics and, in turn, shape
system load and resource demand. Motivated by this observation,
we conduct a fine-grained analysis of task composition and task-
specific input and output token properties. Unlike prior studies [1]
that primarily report task proportions, we focus on how task in-
tent influences token geometry, providing insights that are directly
relevant to LLM system design and evaluation.
We perform task classification on a random sample of 100K re-
quests from our dataset. Requests are mapped into 10 representative
task intent categories (detailed in the Appendix D). To reflect multi-
intent prompts, we retain up to two high-confidence task labels per
request. All classification is executed offline on sanitized prompt
content in a privacy-preserving manner.
The classification pipeline proceeds in two stages. In the first
stage, following prior work [1], we apply Google Cloud Natural
Language’s classifyText API to a labeled subset of 20K requests
drawn from internal user interactions, which are pre-approved
for research use. The API maps textual input (up to the first 1,000
characters of each prompt) to a hierarchical, language-agnostic
taxonomy, returning category paths with associated confidence
scores in [0,1]; categories below the default confidence threshold
of 0.5 are discarded. We then consolidate the API’s fine-grained
taxonomy into our 10 study-defined task intent buckets, yielding
20K silver-labeled annotated requests as a training corpus.
In the second stage, we use this labeled corpus to fine-tune a
BERT-based classifier [7] for multi-label classification, retaining
up to two predicted labels per request. The model is trained and
served entirely within our internal server cluster, so that no prompt
content is transmitted to external services during inference. To
further protect user privacy, predictions are logged at the request
level without any linkage to individual users. The fine-tuned model
is then applied in batch to the full 100K-request sample in an auto-
mated offline pipeline, producing the task-intent labels used in all
subsequent analyses.
Science 
45.9%
Writing 
10.7%
Roleplaying
10.6%
Entertainment
10.3%
Social 
8.8%
Finance 
5.9%
Health
4.3%
Others 1.4%
Programme 0.8%
Law 0.7%
Commerce 0.6%
Figure 8: Distribution of task intents.
4.1
Task Composition
In practice, deployed LLM services rarely operate under single-task
settings, but instead handle a mixture of requests spanning multi-
ple application domains. Characterizing task composition therefore
provides essential context for understanding workload heterogene-
ity and its implications for system behavior.
Fig. 8 shows the distribution of task intents in our dataset. Sci-
entific requests account for the largest share, comprising nearly
half of all requests. Writing, role-playing, and entertainment tasks
also contribute substantial fractions, while categories such as law,
programming, and commerce appear less frequently and exhibit
long-tailed behavior. Workload displays a diverse and heteroge-
neous task composition rather than being dominated by an intent,
reflecting realistic usage patterns in production LLM services.
4.2
Task-specific Token Characteristics
Complementary to the architecture-scale analysis in §3.2, we char-
acterize task-driven heterogeneity in token-length patterns. We
analyze both marginal distributions of input and output lengths
and conditional relationships between them under different task
intents, providing quantitative foundations for task-aware work-
load generation and downstream system design, including buffer
allocation and prefill-decode resource planning.
4.2.1
Marginal Distributions. Fig. 9 shows input-token histograms
for major task categories. Unlike architecture-specific patterns
that differ primarily in scale and tail weight, task heterogeneity
manifests through distribution shapes. Some tasks concentrate
mass in continuous humps with smooth tails (Programme, Com-
merce), others exhibit needle-like spikes at stable locations plus low-
density backgrounds (Law, Science, Finance, Health, Social), and a
third group spreads mass across wide plateaus with no dominant
mode (Writing, Entertainment, Role-playing). This shape-driven
taxonomy remains stable across random subsampling and supports
0
500 1000 1500 2000
Length
Length
0
1000
2000
3000
4000
5000
Social
Length
0
1000
2000
3000
4000
Writing
Input Tokens
Output Tokens
0
500 1000 1500 2000
0
500 1000 1500 2000
0
1000
2000
3000
4000
Roleplaying
Frequency
0
25
50
75
100
125
150
175
Law
0
100
200
300
400
Frequency
Programme
0
20
40
60
80
100
Commerce
Figure 9: Task-specific marginal token distributions.
25-75 percentile
Median
Mean Output Length
0
200
400
600
800
1000
1200
Output Length
Programme
0
200
400
600
800
1000
1200 Science
0
200
400
600
800
1000
1200 Law
0
200
400
600
800
1000
1200
Output Length
Social
0
200
400
600
800
1000
1200 Writing
0
200
400
600
800
1000
1200 Roleplaying
0 250
500
750
1000
1250
1500
1750
2000
0 250
500
750
1000
1250
1500
1750
2000
0 250
500
750
1000
1250
1500
1750
2000
Input Length
Input Length
Input Length
Figure 10: Task-specific input-output correlation patterns.


---

Zhang and Huang, et al.
generator-friendly modeling. In contrast, output distributions re-
main largely consistent across tasks, with most mass concentrated
in 0–300 tokens and only minor tail variations. This consistency
implies that task-awareness is most critical for modeling prefill-side
input load, while decode-side output statistics admit a shared base
distribution with light task-specific corrections.
Finding 6: Task intents induce strong heterogeneity in input
distributions through shape-level differences (hump-tail, spike-
background, wide-plateau), while output distributions remain
largely consistent. Task-aware generation must explicitly model
prefill-side diversity, while decode-side can rely on shared pa-
rameterizations.
4.2.2
Input-Output Correlation. Fig. 10 shows median output tra-
jectories across input bins for representative tasks (the complete set
of ten tasks is provided in Appendix B). Technical and knowledge-
intensive tasks (Programme, Science, Finance, Health, Commerce,
Law) exhibit rise–peak–decline–stable patterns that closely resem-
ble the Dense inverted-bowl geometry observed in §3.2. Median
output increases with input length at moderate scales and then
declines toward the tail regime. This suggests a semantic shift:
moderate contexts trigger expanded explanations or step-by-step
reasoning, while very long contexts correspond to compression-
oriented tasks such as summarization.
In contrast, conversational and creative tasks (Social, Writing,
Entertainment, Role-playing) are largely input-insensitive, with
median output remaining approximately constant across wide input
ranges and tight percentile bands. This suggests that high-volume
interactive tasks can be served using conservative decode buffers.
Finding 7: Task patterns exhibit structural compatibility with ar-
chitecture patterns. Technical tasks show inverted-bowl profiles
(peak at moderate inputs, decline for long contexts), while con-
versational tasks remain flat. This cross-dimensional consistency
enables compositional generation combining architecture-aware
and task-aware modules.
5
Workload Generation
5.1
FineServe Framework
FineServe provides a lightweight and practical framework for gen-
erating realistic multi-model LLM serving workloads (Fig. 11). At a
high level, FineServe constructs per-model request streams and then
aggregates them into a unified mixed workload for evaluation in an
inference cluster. To use FineServe, a user first specifies the target
model set, the task composition (optional), and the desired work-
load scale through the Configurator. FineServe then invokes the
Fine-grained Workload Generator to produce each per-model stream
in one of two modes: (i) real-trace replay, which directly samples
requests from the trace repository while preserving their original
timestamps and payloads, and (ii) parametric synthesis, which sam-
ples timestamps and payloads from fitted statistical models to scale
workloads to arbitrary volumes.
In parametric synthesis, FineServe follows an arrival-then-payload
pipeline. It first samples request timestamps using model-aware
arrival models (Gamma inter-arrivals, augmented with NB count
Mixed Requests
Models
Tasks
𝛼𝛼, β
𝜇𝜇, 𝜎𝜎
Mode
…
Arrival time
Model_tag
Task_tag
Payload
Timestamp 𝑇𝑇𝑎𝑎
Input tokens 𝐿𝐿𝑖𝑖𝑖𝑖
Output tokens 𝐿𝐿𝑜𝑜𝑜𝑜𝑜𝑜
Task tag
…
Arrival time
Payload
Model tag
Task tag
Requests Sampler
𝑇𝑇𝑎𝑎, 𝐿𝐿𝑖𝑖𝑖𝑖, 𝐿𝐿𝑜𝑜𝑜𝑜𝑜𝑜, …
Arrival time
Generator 
Gemma
Negative Binomial
Configurator
LLM Inference Cluster
Inference 
Engine
HTTP Server
Task-Aware 
Payload Adapter
𝑇𝑇𝑎𝑎, 𝐿𝐿𝑖𝑖𝑖𝑖+, 𝐿𝐿𝑜𝑜𝑜𝑜𝑜𝑜+,…
Datasets
Parametric
Replay
Fine-grained Workload Generator
Workload 
Mixer
𝐿𝐿𝑖𝑖𝑖𝑖, 𝐿𝐿𝑜𝑜𝑜𝑜𝑜𝑜
𝑇𝑇𝑎𝑎
General 
Payload Generator
Log-normal
Correlation
Figure 11: Overview of the FineServe framework. Per-model
request streams are mixed to produce a unified multi-model
workload benchmark. The color gray indicates task-aware
payload adaptation is optional, allowing FineServe to natu-
rally degenerate to general task workload simulation.
0
10
20
30
40
10
20
30
40
50
Requests per Second
0
50
100
150
200
250
Time (seconds)
2.5
5.0
7.5
10.0
12.5
15.0
0
50
100
150
200
250
2
4
6
8
10 MoE >100B Replay Mode
MoE >100B Parametric Mode
Dense 10-30B Replay Mode
0
10
20
30
40
Time (seconds)
10
15
20
25
30
35
40
Requests per Second
Dense 10-30B Parametric Mode
Figure 12: Request rate over time generated by replay and
parametric modes.
modeling when millisecond-level concurrency is present), capturing
both long-term rate shifts and short-term burstiness. It then samples
correlated input/output token lengths using class-specific payload
models, optionally refining payloads via a task-aware adapter to
match the configured task composition. Finally, a request sampler
assembles (𝑇𝑎, 𝐿in, 𝐿out, model, task) tuples for each model, where
𝑇𝑎is the arrival time, (𝐿in, 𝐿out) denotes input/output token lengths
(and associated prompt metadata from ShareGPT or other public
datasets). Then, the Workload Mixer merges per-model streams into
a single multi-model workload. Overall, FineServe operationalizes
the workload signatures identified in earlier sections and exposes
them as configurable knobs, enabling scalable yet realistic workload
construction for benchmarking multi-model LLM serving systems.
5.2
Parametric Generation vs. Trace Replay
To validate the fidelity of our parametric workload generation, we
compare the arrival patterns produced by the parametric mode
against those obtained via trace replay. For each architecture group,
we select a parameter scale and generate 1,000 requests using the
default fitted parameters. We then plot the resulting per-second
request rates alongside their replayed counterparts.
As shown in Fig. 12, the parametric workloads closely resemble
the replayed traces in terms of arrival intensity, burst frequency,
and temporal fluctuation patterns, across both Dense and MoE
model families. We emphasize that our goal is not to reproduce


---

FineServe: A Fine-Grained Dataset and Characterization of Global LLM Serving Workloads
the original arrival sequence verbatim, but rather to approximate
its empirical arrival dynamics at a macroscopic timescale. This
parameterized formulation enables researchers to flexibly gener-
ate workloads with controllable scale, burstiness, and variability,
thereby facilitating reproducible evaluation and scheduling studies
under diverse operating regimes.
6
Conclusion
We present a comprehensive and fine-grained characterization of
real-world LLM serving workloads for today’s multi-model plat-
forms. We analyze workload heterogeneity along model architec-
ture/scale tiers and task intents, and unveil various meaningful
findings. Motivated by these findings, we introduce FineServe, a
model-aware workload generation framework that supports both
real-trace replay and parametric synthesis, enabling realistic multi-
model workload mixing and scalable evaluation. We believe that
the dataset, analyses, and generation framework can support more
faithful benchmarking and system studies on LLM serving systems.
References
[1] Malika Aubakirova, Alex Atallah, Chris Clark, Justin Summerville, and Anjney
Midha. 2026. State of AI: An Empirical 100 Trillion Token Study with OpenRouter.
arXiv:2601.10088 [cs.AI] https://arxiv.org/abs/2601.10088
[2] Jinze Bai, Shuai Bai, Yunfei Chu, Zhenyu Cui, Kai Dang, Xiaodong Deng, et al.
2023. Qwen Technical Report. arXiv preprint arXiv:2309.16609 (2023).
[3] Tom B. Brown, Benjamin Mann, Nick Ryder, Melanie Subbiah, Jared Kaplan,
Prafulla Dhariwal, Arvind Neelakantan, Pranav Shyam, Girish Sastry, Amanda
Askell, Sandhini Agarwal, Ariel Herbert-Voss, Gretchen Krueger, Tom Henighan,
Rewon Child, Aditya Ramesh, Daniel M. Ziegler, Jeffrey Wu, Clemens Winter,
Christopher Hesse, Mark Chen, Eric Sigler, Mateusz Litwin, Scott Gray, Benjamin
Chess, Jack Clark, Christopher Berner, Sam McCandlish, Alec Radford, Ilya
Sutskever, and Dario Amodei. 2020. Language Models are Few-Shot Learners.
In Advances in Neural Information Processing Systems 33: Annual Conference on
Neural Information Processing Systems 2020, NeurIPS 2020, December 6-12, 2020,
virtual.
[4] Lingjiao Chen, Matei Zaharia, and James Zou. 2023. FrugalGPT: How to Use
Large Language Models While Reducing Cost and Improving Performance. arXiv
preprint arXiv:2305.05176 (2023).
[5] Damai Dai, Chengqi Deng, Chenggang Zhao, R. X. Xu, Huazuo Gao, Deli Chen,
Jiashi Li, Wangding Zeng, Xingkai Yu, Y. Wu, Zhenda Xie, Y. K. Li, Panpan Huang,
Fuli Luo, Chong Ruan, Zhifang Sui, and Wenfeng Liang. 2024. DeepSeekMoE:
Towards Ultimate Expert Specialization in Mixture-of-Experts Language Models.
In Proceedings of the 62nd Annual Meeting of the Association for Computational
Linguistics (Volume 1: Long Papers), ACL 2024, Bangkok, Thailand, August 11-16,
2024. Association for Computational Linguistics, 1280–1297.
[6] Tri Dao. 2024. FlashAttention-2: Faster Attention with Better Parallelism and
Work Partitioning. In The Twelfth International Conference on Learning Represen-
tations, ICLR 2024, Vienna, Austria, May 7-11, 2024. OpenReview.net.
[7] Jacob Devlin, Ming-Wei Chang, Kenton Lee, and Kristina Toutanova. 2019. BERT:
Pre-training of Deep Bidirectional Transformers for Language Understanding.
In Proceedings of the 2019 Conference of the North American Chapter of the Associ-
ation for Computational Linguistics: Human Language Technologies, NAACL-HLT
Volume 1 (Long and Short Papers). 4171–4186.
[8] DeepSeek-AI et al. 2025. DeepSeek-V3 Technical Report. arXiv:2412.19437 [cs.CL]
https://arxiv.org/abs/2412.19437
[9] Glauber D. Gonçalves, Idilio Drago, Alex B. Vieira, Ana Paula Couto da Silva,
Jussara M. Almeida, and Marco Mellia. 2016. Workload models and performance
evaluation of cloud storage services. Computer Networks 109 (2016), 183–199.
doi:10.1016/j.comnet.2016.03.024 Traffic and Performance in the Big Data Era.
[10] Joseph M Hilbe. 2011. Negative binomial regression. Cambridge University Press.
[11] Qinghao Hu, Zhisheng Ye, Zerui Wang, Guoteng Wang, Meng Zhang, Qiaoling
Chen, Peng Sun, Dahua Lin, Xiaolin Wang, Yingwei Luo, Yonggang Wen, and
Tianwei Zhang. 2024. Characterization of Large Language Model Development
in the Datacenter. In 21st USENIX Symposium on Networked Systems Design
and Implementation, NSDI 2024, Santa Clara, CA, April 15-17, 2024. USENIX
Association, 709–729.
[12] Qinghao Hu, Zhisheng Ye, Zerui Wang, Guoteng Wang, Meng Zhang, Qiaoling
Chen, Peng Sun, Dahua Lin, Xiaolin Wang, Yingwei Luo, Yonggang Wen, and
Tianwei Zhang. 2024. Characterization of Large Language Model Development
in the Datacenter. In USENIX Symposium on Networked Systems Design and
Implementation (NSDI).
[13] Myeongjae Jeon, Shivaram Venkataraman, Amar Phanishayee, Junjie Qian,
Wencong Xiao, and Fan Yang. 2019.
Analysis of Large-Scale Multi-Tenant
GPU Clusters for DNN Training Workloads. In 2019 USENIX Annual Techni-
cal Conference (USENIX ATC 19). USENIX Association, Renton, WA, 947–960.
https://www.usenix.org/conference/atc19/presentation/jeon
[14] Kimi Team. 2025.
Kimi K2: Open Agentic Intelligence.
arXiv preprint
arXiv:2507.20534 (2025).
[15] Dmitry Lepikhin, HyoukJoong Lee, Yuanzhong Xu, Dehao Chen, Orhan Firat,
Yanping Huang, Maxim Krikun, Noam Shazeer, and Zhifeng Chen. 2021. GShard:
Scaling Giant Models with Conditional Computation and Automatic Sharding.
In 9th International Conference on Learning Representations, ICLR 2021, Virtual
Event, Austria, May 3-7, 2021. OpenReview.net. https://openreview.net/forum?
id=qrwe7XHTmYb
[16] Zhuohan Li, Lianmin Zheng, Yinmin Zhong, Vincent Liu, Ying Sheng, Xin Jin,
Yanping Huang, Zhifeng Chen, Hao Zhang, Joseph E. Gonzalez, and Ion Sto-
ica. 2023. AlpaServe: Statistical Multiplexing with Model Parallelism for Deep
Learning Serving. In 17th USENIX Symposium on Operating Systems Design and
Implementation, OSDI 2023, Boston, MA, USA, July 10-12, 2023, Roxana Geambasu
and Ed Nightingale (Eds.). USENIX Association, 663–679.
[17] Microsoft. 2023. DeepSpeed-MII. https://github.com/microsoft/DeepSpeed-MII.
Accessed: 2023.
[18] NVIDIA. 2023. TensorRT-LLM. https://github.com/NVIDIA/TensorRT-LLM.
Accessed: 2023.
[19] Isaac Ong, Amjad Almahairi, Vincent Wu, Wei-Lin Chiang, Tianhao Wu, Joseph E.
Gonzalez, M. Waleed Kadous, and Ion Stoica. 2025. RouteLLM: Learning to Route
LLMs from Preference Data. In International Conference on Learning Representa-
tions (ICLR). https://openreview.net/forum?id=8sSqNntaMr
[20] OpenAI. 2024. GPT-4o: Introducing a Multimodal Omni Model. https://openai.
com/index/hello-gpt-4o/
[21] Jay Shah, Ganesh Bikshandi, Ying Zhang, Vijay Thakkar, Pradeep Ramani, and
Tri Dao. 2024. FlashAttention-3: Fast and Accurate Attention with Asynchrony
and Low-precision. In Advances in Neural Information Processing Systems 38:
Annual Conference on Neural Information Processing Systems 2024, NeurIPS 2024,
Vancouver, BC, Canada, December 10 - 15, 2024, Amir Globersons, Lester Mackey,
Danielle Belgrave, Angela Fan, Ulrich Paquet, Jakub M. Tomczak, and Cheng
Zhang (Eds.).
[22] Noam Shazeer, Azalia Mirhoseini, Krzysztof Maziarz, Andy Davis, Quoc V. Le,
Geoffrey E. Hinton, and Jeff Dean. 2017. Outrageously Large Neural Networks:
The Sparsely-Gated Mixture-of-Experts Layer. In 5th International Conference on
Learning Representations, ICLR 2017, Toulon, France, April 24-26, 2017, Conference
Track Proceedings.
[23] Jovan Stojkovic, Chaojie Zhang, Íñigo Goiri, Josep Torrellas, and Esha Choukse.
2025. DynamoLLM: Designing LLM Inference Clusters for Performance and En-
ergy Efficiency. In IEEE International Symposium on High-Performance Computer
Architecture (HPCA). arXiv:2408.00741.
[24] Biao Sun, Ziming Huang, Hanyu Zhao, Wencong Xiao, Xinyi Zhang, Yong Li, and
Wei Lin. 2024. Llumnix: Dynamic Scheduling for Large Language Model Serving.
In 18th USENIX Symposium on Operating Systems Design and Implementation,
OSDI 2024, Santa Clara, CA, USA, July 10-12, 2024, Ada Gavrilovska and Douglas B.
Terry (Eds.). USENIX Association, 173–191.
[25] Qwen Team. 2025. Qwen2.5 Technical Report. arXiv:2412.15115 [cs.CL] https:
//arxiv.org/abs/2412.15115
[26] Hugo Touvron, Thibaut Lavril, Gautier Izacard, Xavier Martinet, Marie-Anne
Lachaux, Timothée Lacroix, Baptiste Rozière, Naman Goyal, Eric Hambro, Faisal
Azhar, Aurélien Rodriguez, Armand Joulin, Edouard Grave, and Guillaume Lam-
ple. 2023. LLaMA: Open and Efficient Foundation Language Models. arXiv
preprint arXiv:2302.13971 (2023).
[27] Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones,
Aidan N. Gomez, Lukasz Kaiser, and Illia Polosukhin. 2017. Attention is All
you Need. In Advances in Neural Information Processing Systems 30: Annual Con-
ference on Neural Information Processing Systems 2017, December 4-9, 2017, Long
Beach, CA, USA. 5998–6008.
[28] Haixun Wang. 2024. Generative AI in E-Commerce: What Can We Expect?. In
Proceedings of the 30th ACM SIGKDD Conference on Knowledge Discovery and
Data Mining (Barcelona, Spain) (KDD ’24). 4739–4740.
[29] Yuxin Wang, Yuhan Chen, Zeyu Li, Xueze Kang, Yuchu Fang, Yeju Zhou, Yang
Zheng, Zhenheng Tang, Xin He, Rui Guo, Xin Wang, Qiang Wang, Amelie Chi
Zhou, and Xiaowen Chu. 2025. BurstGPT: A Real-World Workload Dataset
to Optimize LLM Serving Systems. In Proceedings of the 31st ACM SIGKDD
Conference on Knowledge Discovery and Data Mining V.2 (KDD ’25). Association
for Computing Machinery, New York, NY, USA, 5831–5841. doi:10.1145/3711896.
3737413
[30] Qizhen Weng, Wencong Xiao, Yinghao Yu, Wei Wang, Cheng Wang, Jian He,
Yong Li, Liping Zhang, Wei Lin, and Yu Ding. 2022. MLaaS in the Wild: Workload
Analysis and Scheduling in Large-Scale Heterogeneous GPU Clusters. In 19th


---

Zhang and Huang, et al.
USENIX Symposium on Networked Systems Design and Implementation (NSDI 22).
USENIX Association, Renton, WA, 945–960. https://www.usenix.org/conference/
nsdi22/presentation/weng
[31] Bingyang Wu, Yinmin Zhong, Zili Zhang, Shengyu Liu, Fangyue Liu, Yuanhang
Sun, Gang Huang, Xuanzhe Liu, and Xin Jin. 2024. Fast Distributed Inference
Serving for Large Language Models. arXiv:2305.05920 [cs.LG]
[32] Qingyun Wu, Gagan Bansal, Jieyu Zhang, Yiran Wu, Beibin Li, Erkang Zhu, Li
Jiang, Xiaoyun Zhang, Shaokun Zhang, Jiale Liu, Ahmed H. Awadallah, Ryen
White, Doug Burger, and Chi Wang. 2024. AutoGen: Enabling Next-Gen LLM
Applications via Multi-Agent Conversation. In Workshop on Large Language
Models for Agents. ICLR 2024 Workshop.
[33] Yuxing Xiang, Xue Li, Kun Qian, Wenyuan Yu, Ennan Zhai, and Xin Jin. 2025.
ServeGen: Workload Characterization and Generation of Large Language Model
Serving in Production. arXiv:2505.09999 [cs.DC] https://arxiv.org/abs/2505.09999
[34] Gyeong-In Yu, Joo Seong Jeong, Geon-Woo Kim, Soojeong Kim, and Byung-
Gon Chun. 2022. Orca: A Distributed Serving System for Transformer-Based
Generative Models. In 16th USENIX Symposium on Operating Systems Design
and Implementation, OSDI 2022, Carlsbad, CA, USA, July 11-13, 2022, Marcos K.
Aguilera and Hakim Weatherspoon (Eds.). USENIX Association, 521–538.
[35] Lianmin Zheng, Liangsheng Yin, Zhiqiang Xie, Chuyue Sun, Jeff Huang,
Cody Hao Yu, Shiyi Cao, Christos Kozyrakis, Ion Stoica, Joseph E. Gonzalez,
Clark W. Barrett, and Ying Sheng. 2024. SGLang: Efficient Execution of Struc-
tured Language Model Programs. In Advances in Neural Information Processing
Systems 38: Annual Conference on Neural Information Processing Systems 2024,
NeurIPS 2024, Vancouver, BC, Canada, December 10 - 15, 2024, Amir Globersons,
Lester Mackey, Danielle Belgrave, Angela Fan, Ulrich Paquet, Jakub M. Tomczak,
and Cheng Zhang (Eds.).
[36] Yinmin Zhong, Shengyu Liu, Junda Chen, Jianbo Hu, Yibo Zhu, Xuanzhe Liu,
Xin Jin, and Hao Zhang. 2024. DistServe: Disaggregating Prefill and Decoding for
Goodput-optimized Large Language Model Serving. In 18th USENIX Symposium
on Operating Systems Design and Implementation, OSDI 2024, Santa Clara, CA,
USA, July 10-12, 2024, Ada Gavrilovska and Douglas B. Terry (Eds.). USENIX
Association, 193–210.


---

FineServe: A Fine-Grained Dataset and Characterization of Global LLM Serving Workloads
A
Per-Millisecond Arrival Count Statistics
Table 2: Per-millisecond arrival count statistics and
architecture-aware modeling decisions.
Category
Mean
Var
Var/Mean
NB 𝑟𝑠
Function
Dense <10B
2.63
2.97
1.13
0.03696
Gamma+NB
Dense 10–30B
1.12
0.12
0.11
0.00002 Gamma-only
Dense 30–100B
1
0.003
0.003
0.00002 Gamma-only
MoE >100B
1.01
0.01
0.01
0.00002 Gamma-only
B
Full Task-Specific Token Distributions
Fig. 13 and Fig. 14 present the complete input and output token
length distributions for all ten task intent categories, complement-
ing the representative results discussed in the main text.
C
Parametric Modeling
C.1
Architecture-Specific Input–Output Token
Models
Let 𝑥and 𝑦denote the input and output token lengths of a request,
respectively. We characterize Dense outputs via non-monotonic
piecewise functions, where each segment corresponds to a distinct
operational regime:
E[𝑦|𝑥]Dense ≈


𝑂base + 𝛼Dense · 𝑥,
𝑥< 𝜏peak,
𝑂peak −𝛽Dense · (𝑥−𝜏peak),
𝜏peak ≤𝑥< 𝜏stable,
𝑂tail,
𝑥≥𝜏stable.
(8)
The rising phase is characterized by slope 𝛼Dense, reflecting context
accumulation that peaks at input length𝜏peak with output𝑂peak. The
declining phase with slope 𝛽Dense captures compression-task domi-
nance, stabilizing beyond 𝜏stable at output level 𝑂tail. We sample the
residual as 𝜀∼N (0, 𝜎Dense(𝑥)) to match empirical percentile bands.
This formulation enables input-aware buffer allocation strategies.
We model MoE outputs as:
E[𝑦|𝑥]MoE ≈


𝑂init + 𝑘1 · 𝑥,
𝑥< 𝜏growth,
𝑂mid + 𝑘2 · (𝑥−𝜏growth),
𝜏growth ≤𝑥< 𝜏sat,
min(𝑂sat + 𝑘3 · (𝑥−𝜏sat), 𝑀max),
𝑥≥𝜏sat,
(9)
where initial output 𝑂init grows with slope 𝑘1 until 𝜏growth, then
accelerates with slope 𝑘2 from 𝑂mid until saturation onset at 𝜏sat,
finally plateauing at ceiling 𝑀max with residual slope 𝑘3. 𝑀max is the
output-length ceiling (in tokens) in the saturated regime. Table 3
summarizes key parameters for both architectures.
Generator implementations sample input from Eq. (7), compute
median output via Eq. (8) or (9) using parameters from Table 3, and
overlay variance from empirical P25–P75 bands. For serving sys-
tems, Dense workloads require input-aware buffer allocation (large
buffers for moderate inputs, reduced allocation for long contexts),
while MoE workloads require dynamic expansion strategies.
0
500
1000
1500
2000
0
20
40
60
80
100
Frequency
Commerce
0
500
1000
1500
2000
2500
Frequency
Entertainment
0
100
200
300
400
Frequency
Finance
0
100
200
300
400
500
Frequency
Health
0
25
50
75
100
125
150
175
Frequency
Law
0
100
200
300
400
Frequency
Programme
0
1000
2000
3000
4000
Frequency
Roleplaying
0
1000
2000
3000
4000
5000
Frequency
Science
Length
0
1000
2000
3000
4000
5000
Frequency
Social
Length
0
1000
2000
3000
4000
Frequency
Writing
Input Tokens
Output Tokens
0
500
1000
1500
2000
Figure 13: Distributions of input and output token lengths
across tasks. For each task intent, the figure shows the em-
pirical distributions of input tokens and output tokens, trun-
cated at 2048 tokens for visualization.
Table 3: Fitted parameters for input-output correlation mod-
eling. Piecewise functions capture median trends across ar-
chitectures.
Arc.
Function Type
Key Stages
Characteristics
Dense
Non-monotonic
Rise
Peak: 620 @ 1500 input
Piecewise
Decline
Stable: 165 @ 5000+
Stable
Drop: 60%
MoE
Growth-
Slow Growth
Start: 100 @ 500
Saturation
Linear Growth
Mid: 290 @ 5000
Piecewise
Saturation
Max: 400 @ 8000+
C.2
Task-Specific Token Modeling
C.2.1
Task-Specific Marginal Distributions. We model task inputs
using three families corresponding to the observed shape patterns.


---

Zhang and Huang, et al.
0
250 500 750 10001250150017502000
0
250 500 750 10001250150017502000
25-75 percentile
Median
Mean Output Length
0
200
400
600
800
1000
1200
Output Length
Health
0
200
400
600
800
1000
1200
Law
0
200
400
600
800
1000
1200
Output Length
Programme
0
200
400
600
800
1000
1200
Output Length
Roleplaying
Input Length
0
200
400
600
800
1000
1200
Output Length
Social
Input Length
0
200
400
600
800
1000
1200
Writing
0
200
400
600
800
1000
1200
Output Length
Commerce
0
200
400
600
800
1000
1200
Entertainment
0
200
400
600
800
1000
1200
Finance
0
200
400
600
800
1000
1200 Science
Figure 14: Input–output token length relationships across
tasks. For each task intent, the figure reports conditional sta-
tistics of output token length as a function of input (prompt)
length, including the mean (bars), median (dashed line), and
interquartile range (25–75 percentile, shaded).
For Hump-Tail tasks (Programme, Commerce):
𝑃𝑡(𝑋) = 𝜋𝑡· LogNormal(𝜇𝑡, 𝜎2
𝑡) + (1 −𝜋𝑡) · Exp(𝜆𝑡),
(10)
where 𝜋𝑡controls the weight of the core distribution, (𝜇𝑡, 𝜎𝑡) pa-
rameterize the log-normal component, and 𝜆𝑡controls exponential
tail decay.
For Spike-Background tasks (Law, Science, Finance, Health, So-
cial):
𝑃𝑡(𝑋) =
𝐾𝑡
∑︁
𝑘=1
𝜌𝑡,𝑘· 𝛿(𝑋−𝜏𝑡,𝑘) +
 
1 −
∑︁
𝑘
𝜌𝑡,𝑘
!
· 𝑞𝑡(𝑋),
(11)
where 𝛿(·) denotes point masses at spike locations {𝜏𝑡,𝑘}, 𝜌𝑡,𝑘are
spike weights, and 𝑞𝑡(𝑋) is a continuous background distribution.
For Wide-Plateau tasks (Writing, Entertainment, Role-playing):
𝑃𝑡(𝑋) ∝1[𝑋∈(𝑎𝑡,𝑏𝑡)],
(12)
a truncated uniform over range (𝑎𝑡,𝑏𝑡).
All tasks share a common output base:
𝑃(𝑌) = LogNormal(𝜇out, 𝜎2
out).
(13)
C.2.2
Task-Conditional Input–Output Modeling. We model task-
conditional outputs via a two-component framework. For task 𝑡:
𝑦= 𝑔ℎ(𝑡) (𝑥;𝜃𝑡) + 𝜀,
𝜀∼N (0, 𝜎2
𝑡(𝑥)),
(14)
where ℎ(𝑡) maps task 𝑡to its pattern class (inverted-bowl or flat),
𝑔ℎ(𝑡) (·) is the class-specific median function, 𝜀captures residual
variability and 𝜎2
𝑡(𝑥) is the variance.
For inverted-bowl tasks (technical and knowledge-intensive):
𝑔inv(𝑥;𝜃𝑡) =


𝛼𝑡𝑥,
𝑥< 𝜏(𝑡)
peak,
𝑂(𝑡)
peak −𝛽𝑡(𝑥−𝜏(𝑡)
peak),
𝜏(𝑡)
peak ≤𝑥< 𝜏(𝑡)
stable,
𝑂(𝑡)
tail,
𝑥≥𝜏(𝑡)
stable.
(15)
For flat tasks (conversational and creative):
𝑔flat(𝑥;𝜃𝑡) = 𝑐𝑡,
(16)
where 𝑐𝑡is a task-specific constant.
These findings inform both workload generation and serving sys-
tem design. Generator implementations classify tasks into pattern
classes, apply corresponding median functions (Eq. (15) or (16)) as
shown in Table 4, and overlay variance sampled from task-specific
percentile bands. For serving systems, task-aware routing enables
technical tasks to benefit from input-aware buffer allocation (large
buffers for moderate inputs, reduced allocation for long contexts),
while conversational tasks use fixed conservative buffers that sim-
plify memory management.
Table 4: Task-specific input-output correlation parameters.
Two pattern classes capture median trends across task in-
tents.
Pattern
Example Tasks
𝜏peak
𝑂peak
𝑂tail
Inverted-bowl
Programme
1100
950
520
Science
1200
860
540
Law
750
620
280
Flat
Social
–
–
140 (const)
Writing
–
–
155 (const)
D
List of Models and Tasks
In Table 5 and Table 6, we provide a detailed overview of all models
and task intents included in FineServe.


---

FineServe: A Fine-Grained Dataset and Characterization of Global LLM Serving Workloads
Table 5: Model suite used in our analysis. Models are grouped by architecture (Dense vs. MoE) and parameter scale (<10B,
10–30B, 30–100B, >100B), and annotated with their public release month to contextualize model recency.
Architecture
Scale
Model
Model Series
Release
Dense
<10B
Qwen3-4B-FP8
Qwen
Jan-25
Qwen3-8B-FP8
Qwen
Jan-25
Qwen2.5-7B-Instruct
Qwen
Oct-24
DeepSeek-R1-0528-Qwen3-8B
DeepSeek
Jun-24
Mistral-7B-Instruct
Mistral
Oct-23
LLaMA-3.2-1B-Instruct
LLaMA
Sep-24
LLaMA-3.2-3B-Instruct
LLaMA
Sep-24
LLaMA-3.1-8B-Instruct-BF16
LLaMA
Jul-24
LLaMA-3.1-8B-Instruct-FP8
LLaMA
Jul-24
LLaMA-3-8B-Instruct
LLaMA
Apr-24
Hermes-2-Pro-LLaMA-3-8B
LLaMA
Jun-24
DeepSeek-R1-Distill-LLaMA-8B
DeepSeek
Jun-24
GLM-Z1-9B-0414
GLM
Apr-24
GLM-4-9B-0414
GLM
Apr-24
L3-8B-Stheno-v3.2
Sao10K
Nov-24
L3-8B-Lunaris
Sao10K
Nov-24
L3-8B-Stheno-v3.2-SpicyChat
Sao10K
Nov-24
WizardLM-2-7B
WizardLM
Jun-24
10–30B
Gemma-3-27B-IT
Gemma
Dec-24
Mistral-Nemo
Mistral
Jul-24
DeepSeek-R1-Distill-Qwen-14B
DeepSeek
Jun-24
MythoMax-L2-13B
LLaMA
Dec-23
Captain-Eris_Violet-v0.420-12B
LLaMA
Dec-24
MN-12B-Lyra-Spiced-v1-FP8
LLaMA
Dec-24
MN-12B-Mag-Mell-R1-Yodayo
LLaMA
Dec-24
30–100B
Qwen3-32B-FP8
Qwen
Jan-25
Qwen2.5-72B-Instruct
Qwen
Oct-24
Qwen2.5-VL-72B-Instruct
Qwen
Oct-24
Dolphin-2.9.2-Qwen2-72B
Dolphin
Nov-24
DeepSeek-R1-Distill-Qwen-32B
DeepSeek
Jun-24
LLaMA-3.3-70B-Instruct
LLaMA
Dec-24
LLaMA-3-70B-Instruct
LLaMA
Apr-24
DeepSeek-R1-Distill-LLaMA-70B
DeepSeek
Jun-24
LLaMA-3.1-70B-Euryale-v2.2
Sao10K
Dec-24
LLaMA-3-70B-Euryale-v2.1
Sao10K
Nov-24
Midnight-Rose-70B
Sao10K
Dec-24
Midnight-Rose-70B-SpicyChat
Sao10K
Dec-24
GLM-Z1-32B-0414
GLM
Apr-24
GLM-4-32B-0414
GLM
Apr-24
GLM-Z1-Rumination-32B-0414
GLM
Apr-24
MoE
10–30B
Qwen3-30B-A3B-FP8
Qwen
Jan-25
LLaMA-4-Scout-17B-16E-Instruct
LLaMA
Jan-25
LLaMA-4-Maverick-17B-128E-Instruct-FP8
LLaMA
Jan-25
>100B
Qwen3-235B-A22B-FP8
Qwen
Jan-25
DeepSeek-Prover-V2-671B
DeepSeek
Aug-24
DeepSeek-R1-Turbo
DeepSeek
Jul-24
DeepSeek-V3-Turbo
DeepSeek
Jun-24
DeepSeek-R1-0528
DeepSeek
May-24
DeepSeek-R1
DeepSeek
May-24
DeepSeek-R1-Community
DeepSeek
May-24
DeepSeek-V3-Community
DeepSeek
Apr-24
DeepSeek-V3-0324
DeepSeek
Mar-24
DeepSeek-V3
DeepSeek
Mar-24
WizardLM-2-8×22B
WizardLM
Jun-24
Dolphin-Mixtral-8×22B
Dolphin
Jan-24


---

Zhang and Huang, et al.
Table 6: Task intent definitions used in our analysis.
Intent
Description
Science
Scientific reasoning, mathematics, engineering
Writing
Essay writing, summarization, rewriting
Roleplaying
Roleplay, fictional personas, interactive storytelling and adult
Entertainment
Games, jokes, creative content, and casual entertainment
Social
Conversation, personal interaction, and social queries
Finance
Financial analysis, investment, accounting, and business planning
Health
Medical, wellness, and health-related queries
Programme
Code generation, debugging, and software-related tasks
Law
Legal interpretation, compliance, and regulation-related queries
Commerce
E-commerce, product comparison, marketing, and sales-related queries
