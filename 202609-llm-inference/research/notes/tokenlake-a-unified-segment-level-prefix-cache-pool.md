---
title: 'TokenLake: A Unified Segment-level Prefix Cache Pool'
id: tokenlake-a-unified-segment-level-prefix-cache-pool
tags:
- llm-d
- kv-cache-routing
- load-balancing
- memory-fragmentation
- pd-disaggregation
- academic-comparison
- llmd-kv-cache-routing-c665d5
created: '2026-09-14T11:53:38.159962Z'
updated: '2026-09-15T04:55:24.043217Z'
source: https://arxiv.org/pdf/2508.17219
source_domain: arxiv.org
fetched_at: '2026-09-14T11:53:38.159680Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: institutional
content_type: paper
deprecated: false
summary: 'TokenLake (Peking University + StepFun) diagnoses three structural failure
  modes of instance-local cache-aware routing (the prefix-tree-per-instance model
  used by systems like llm-d''s KV-cache-aware router, SGLang, Mooncake): (1) load
  imbalance -- instances with high prefix hit rates become memory-bandwidth over-utilized
  while cold instances sit idle, because the router can only route whole requests
  to whichever instance already holds the matching prefix tree; (2) redundancy --
  to rebalance computation, hot shared prefixes (system prompts, shared documents)
  get replicated across multiple instances, wasting memory; (3) fragmentation -- because
  a full prefix must reside on one instance to form a contiguous prefix tree, memory
  slots scattered across instances cannot be pooled together. These problems compound
  under PD (prefill/decode) disaggregation, since prefill and decode instances have
  different memory-bandwidth profiles and must each separately hold the same prefix
  cache to avoid recomputation. TokenLake''s fix decouples cache management from scheduling
  via a declarative interface (exposing query tensors, prefix caches, and cache-aware
  ops) and manages cache at sub-request segment granularity with a heavy-hitter-aware
  load-balancing algorithm, letting the scheduler dispatch elastically without needing
  to track prefix locality. On real-world workloads (CharacterAI/Kimi report 75-95%
  production hit rates as baseline context), TokenLake improves throughput up to 2.6x
  and 2.0x and cache hit rate up to 2.0x and 2.1x versus state-of-the-art cache-aware
  routing and cache-centric PD-disaggregation baselines respectively.'
raw_file: raw/tokenlake-a-unified-segment-level-prefix-cache-pool.pdf
doi: arXiv:2508.17219
utility_score: 15.0
---

TokenLake: A Unified Segment-level Prefix Cache Pool
for Fine-grained Elastic Long-Context LLM Serving
Bingyang Wu1
Zili Zhang1
Yinmin Zhong1
Guanzhe Huang2
Yibo Zhu2
Xuanzhe Liu1
Xin Jin1
1Peking University
2StepFun
Abstract
Prefix caching is crucial to accelerate multi-turn interactions
and requests with shared prefixes. At the cluster level, exist-
ing prefix caching systems are tightly coupled with request
scheduling to optimize cache efficiency and computation
performance together, leading to load imbalance, data re-
dundancy, and memory fragmentation of caching systems
across instances. To address these issues, memory pooling
is promising to shield the scheduler from the underlying
cache management so that it can focus on the computation
optimization. However, because existing prefix caching sys-
tems only transfer increasingly longer prefix caches between
instances, they cannot achieve low-latency memory pooling.
To address these problems, we propose a unified segment-
level prefix cache pool, TokenLake. It uses a declarative cache
interface to expose requests’ query tensors, prefix caches,
and cache-aware operations to TokenLake for efficient pool-
ing. Powered by this abstraction, TokenLake can manage
prefix cache at the segment level with a heavy-hitter-aware
load balancing algorithm to achieve better cache load balance,
deduplication, and defragmentation. TokenLake also trans-
parently minimizes the communication volume of query ten-
sors and new caches. Based on TokenLake, the scheduler can
schedule requests elastically by using existing techniques
without considering prefix cache management. Evaluations
on real-world workloads show that TokenLake can improve
throughput by up to 2.6× and 2.0× and boost hit rate by 2.0×
and 2.1×, compared to state-of-the-art cache-aware routing
and cache-centric PD-disaggregation solutions, respectively.
1
Introduction
With the advancements of Large Language Models (LLMs),
the context lengths of LLMs are becoming increasingly longer
to support more sophisticated tasks [10, 20, 38]. This trend is
reflected in increasingly longer input prompts and a growing
number of multi-turn interactions with users and external
tools. For example, agent application Manus reports their
average input-to-output ratio is 100:1 [41], and the average
number of turns per session on Character.AI is 180 turns [13].
To reduce the computation introduced by long context, a
key strategy is prefix caching, specifically reusing previously
computed KV cache, for requests with the shared prefix. Ev-
idence from CharacterAI [13] and Kimi [50] reveals that the
hit rate can reach up to 75% to 95% in production. As the
KV cache size increases linearly with the sequence length,
efficiently managing such a large volume of KV cache to
achieve a high cache hit rate has become a critical problem.
A popular approach to maximize cache utilization is to
use cache-aware routing [31, 44, 56, 57, 73]. The prefix cache
on each instance is organized as a local prefix tree. For each
request, the router dispatches it to one instance based on the
per-instance hit rates and load-balancing strategy.
However, it leads to the following issues. (1) For instances
with high hit rates, the memory bandwidth is over-utilized,
whereas for instances with low hit rates, the memory band-
width is under-utilized, leading to the load imbalance. (2)
To balance the computation across instances, requests with
shared prefixes, such as system prompts and shared docu-
ments, are often replicated across instances, leading to severe
redundancy. (3) Because the entire prefix requires residing in
the same instance to form a prefix tree, available slots across
instances cannot be used together, leading to fragmentation.
Meanwhile, to avoid interference between the prefill and
decoding phase, PD disaggregation [27, 48, 74] has been
adopted to disaggregate these two phases on different in-
stances. This further exacerbates the above problems. Specif-
ically, instances in different phases have different demands
on memory bandwidth [67], leading to the load imbalance.
In multi-turn scenarios, both prefill and decode instances
must store the same prefix cache to avoid re-computation or
repeated transfer, causing redundancy. Similarly, the prefix
cache slots on prefill and decode instances are hard to share
by existing caching systems, leading to fragmentation.
To fundamentally address these issues, GPU memory pool-
ing is a promising solution. By aggregating all GPU memories
into a unified pool and decoupling memory access from com-
putation, it is possible to utilize all available GPU memory
bandwidth to avoid load imbalance and all GPU memory
as a single cache to eliminate redundancy and fragmenta-
tion, while supporting elastic scheduling to accommodate
dynamic workloads without considering data locality.
However, existing prefix caching systems [3, 26, 50, 56, 57]
are not efficient for GPU memory pooling. Their interface,
e.g., put, get, and transfer, requires the scheduler to con-
sider cache management and can only imperatively transfer
increasingly longer prefix cache across instances for compu-
tation. This architecture suffers from high communication
overhead and binds two different and sometimes conflicting
concerns together, i.e., cache management for high memory
1
arXiv:2508.17219v1  [cs.DC]  24 Aug 2025


---

efficiency and scheduling for high computation efficiency of
dynamic workloads, leading to suboptimal performance.
For the above problems, we propose a declarative prefix
cache interface and build a unified segment-level prefix cache
pool, TokenLake, to decouple prefix cache management and
scheduling by exposing query tensors, prefix caches, and
cache-aware operations to TokenLake. This design allows
TokenLake to achieve low-latency memory pooling by trans-
parently optimizing transmission of query tensors and prefix
cache together at finer granularity, while enabling the sched-
uler to elastically schedule requests’ cache-free operations,
such as self-attention and feed-forward network module, in
a stateless way for better performance.
However, efficiently utilizing all GPU resources is still
non-trivial. First, even if integrated query tensors and cache-
aware operations into TokenLake, a simple pooling strategy
does not guarantee efficient utilization of all memory band-
width. For example, if using a strict locality-based policy that
spills the prefix cache to other instances only when the local
memory is insufficient, it still leads to load imbalance and fre-
quent migration of prefix cache (§4). Second, finer-grained
management dramatically expands the decision space of
caching, making effective load balancing, duplication con-
trol, and communication optimization difficult.
To address these challenges, we introduce a heavy-hitter-
aware segment-level load balancing algorithm to effectively
utilize memory and bandwidth. We also propose a bipartite
matching-based dispatching algorithm to minimize the com-
munication volume of query tensors and newly generated
KV cache under the given scheduler’s decision. Finally, we
implement a goodput-optimized stateless elastic schedul-
ing algorithm by using PD disaggregation, chunked prefill,
and elastic sequence parallelism together to demonstrate its
expressiveness and its potential to enable fine-grained sched-
uling. The complexity of these algorithms is polynomial to
the number of instances, making them efficient and scalable.
Evaluations show that, compared to state-of-the-art cache-
aware routing, SGLang-Router [73] and cache-centric PD-
disaggregation solutions, MoonCake [50], TokenLake can
improve throughput under a given SLO by up to 2.6× and
2.0×, and boost hit rate by up to 2.0× and 2.1×, respectively.
In summary, our contributions are as follows:
• We propose a declarative prefix cache interface to decou-
ple prefix cache management and request scheduling to
achieve efficient memory pooling.
• We propose a heavy-hitter-aware segment-level load bal-
ancing algorithm and a bipartite matching-based dispatch-
ing algorithm to efficiently achieve load balance, lower
redundancy, and less fragmentation of the prefix cache.
• We implement a goodput-optimized stateless elastic sched-
uling algorithm to demonstrate the expressiveness of the
interface and conduct a comprehensive evaluation to show
the effectiveness of TokenLake.
Cache-Aware Request Router
Prefix
Statistics
Load
Statistics
t1
t2
t3
t4
t1
t2
t5
t6
Instance 2
Compute Load: 
33%
Actual Prefix Tree
Instance 3
Compute Load:
0%
Actual Prefix Tree
Instance 1
Compute Load: 
66%
Actual Prefix Tree
Overload!
Thrashing!
Re-computation!
Fragmentation!
Redundancy!
R1
R2
Figure 1. Limitations of cache-aware routing.
2
Background
2.1
LLM serving systems
The inference process of Large Language Models (LLMs) can
be divided into two distinct phases: the prefill phase and the
decoding phase. The prefill phase processes the entire input
tokens in one iteration to generate the initial Key-Value (KV)
cache, intermediate states for future iteration, while the de-
coding phase generates output tokens iteratively and stores
newly generated KV cache in the prefix cache. For multi-turn
sessions, they contain multiple turns of requests, each with
its respective prefill and decoding phase. Especially, each
turn also needs the KV cache of previous turns. In each iter-
ation of the inference process, the prefix cache only involves
the attention module. In each layer, query tensors generated
from the input tokens use prefix attention and self-attention
to interact with all preceding KV cache and KV cache of
themselves, respectively, and then reduce the partial output
tensors and normalizers to generate the final output tensors
of the attention module. Therefore, except for the prefix at-
tention, which is cache-aware, self-attention and all other
operations, such as feed-forward network (FFN) and projec-
tion layer, are cache-free, i.e., computation only depends on
the previous module’s output and model parameters without
directly using the prefix cache.
To support more applications, such as analyzing large
codebases and stateful multi-turn agent interactions, the
context length of LLMs has been significantly increased [10,
20, 38]. To accelerate the input processing speed of long-
context inference, prefix caching is proposed to further share
KV caches across requests with the same prefix or multiple
turns in a session [19, 28, 47, 64, 69, 73, 76]. To quickly re-
trieve the relevant prefix cache, prefix caches are typically
organized as a prefix tree in each LLM instance [73].
2


---

Prefill-Decode Disaggregation
Request Router
Prefix
Statistics
Load
Statistics
t1
t2
t3
t4
Input
Output Input
Prefill Instance
Compute Load: 
80%
Actual Prefix Tree
Decode Instance
Compute Load: 
15%
Actual Prefix Tree
Redundancy!
Fragmentation!
Busy
Interconnection
Idle
Interconnection
Idle
Interconnection
Figure 2. Limitations of cache-centric PD disaggregation.
At the same time, many scheduling algorithms have been
proposed to accommodate the distinct characteristics of the
prefill and decoding phases. On the instance level, chunked
prefill [8, 24, 30] is proposed to split the long context into
smaller chunks and batch requests in two phases together to
reduce the interference between them. On the cluster level,
PD disaggregation [16, 18, 29, 49, 51, 60, 72] is proposed to
disaggregate the prefill and decoding phases into different
instances to eliminate interference between the two phases.
Elastic sequence parallelism (ESP) [62] is further proposed to
handle variable context lengths and frequent ratio changes
between prefill and decoding phases by elastically setting
the degree of parallelism (DoP) for requests with various
context lengths in different phases.
2.2
LLM caching systems
To support prefix management on each instance and com-
plex scheduling algorithms on the cluster level, LLM serving
systems require efficient caching systems to manage the
prefix cache. Existing caching systems typically expose an
imperative interface, such as put, get, and transfer APIs,
for LLM serving systems to imperatively cache and retrieve a
prefix cache in the local prefix tree and transfer it to another
instance [3, 26, 50, 56, 57].
Because of the imperative nature, the caching system is
tightly coupled with the scheduling algorithm. The sched-
uler of the LLM serving system has to consider the state of
the prefix cache in each instance for maximizing cache effi-
ciency while improving requests’ computation performance,
such as throughput and latency. The tight coupling between
schedulers and their underlying imperative caching systems
has given rise to two popular policies.
The first is cache-aware request routing. With the statistics
collected from instances, the cache-aware router dispatches
each incoming request to an instance by trading off between
multiple potentially conflicting goals, such as maximizing
cache hit rate and balancing computation across instances.
The other is cache-centric PD disaggregation. In this pol-
icy, the user decides the number of prefill and decode in-
stances based on the computation and cache demands of the
predicted workload. A PD disaggregation router is used to
dispatch requests to prefill instances first, then imperatively
invokes the transfer API to transfer the generated prefix
cache to decode instances for subsequent decoding.
2.3
Motivations
Limitations of existing solutions. To motivate our design,
we first analyze the limitations of existing solutions.
A primary limitation is the severe load imbalance inherent
in the two mainstream scheduling policies. For cache-aware
routing, this tight coupling makes instances with high hit
rate attract more requests computation, more KV cache gen-
eration and storage on the same instance, which further in-
creases the hit rate of the instance, while other instances are
under-utilized, leading to load imbalance in terms of number
of accessed prefix cache, as shown in overloaded instance 1
of Figure 1. For PD disaggregation, load imbalance is even
more pronounced. As shown in Figure 2, in PD disaggrega-
tion, decode instances not only use prefix cache generated at
the decoding phase, but also use prefix cache sent from the
prefill phase, creating a fixed, asymmetric cache load that
cannot be balanced between the two groups.
The second limitation is the data redundancy, which leads
to inefficient cache utilization. For cache-aware routing, load
balancing policy forces the caching system to replicate shared
prefix caches across multiple instances (𝑘𝑣1, 𝑘𝑣2), and may
introduce re-computation overhead to generate replicated
prefix (e.g., dispatch 𝑅1 to instance 3 in Figure 1). Similarly,
PD disaggregation necessitates that the prefix cache gener-
ated by prefill instances is transferred to decode instances
for subsequent computation, while caching them in prefill
instances for incoming turns or requests, leading to redun-
dancy (𝑘𝑣1, 𝑘𝑣2). In both scenarios, the redundancy is in-
evitable for existing imperative caching systems, otherwise
it causes frequent transfer for increasingly longer prefix
cache as the number of turns in sessions increases.
The last limitation is memory fragmentation. In the sce-
nario depicted in Figure 1 and Figure 2, the system has four
free slots in aggregate, but a new request (e.g., R3) requiring
this amount cannot be placed because the cache capacity on
any instance is insufficient. It leads to thrashing, i.e., eviction
of prefix caches that may be required in the future (𝑘𝑣5 in
instance 2 of Figure 1), introducing substantial overhead.
3


---

2
4
6
8
Degree of Sequence Parallelism
(a) Prefill Phase
0.0
0.2
0.4
0.6
0.8
1.0
Normalized Iteration Time (s)
Len=5k
Len=10k
Len=20k
Len=50k
Len=100k
Linear Scaling
2
4
6
8
Degree of Sequence Parallelism
(b) Decoding Phase
0.0
0.2
0.4
0.6
0.8
1.0
Figure 3. Effectiveness of GPU pooling.
Opportunities. Recent advancements in communication
hardware and software aspects have enabled the develop-
ment of declarative cache designs.
• Hardware. A significant opportunity arises from the in-
creasingly powerful interconnections and their inefficient
usage. The scale-out links are narrowing the gap with
PCIe [7, 43], and the scale-up links are even surpassing
PCIe [46, 53], while GPUDirect RDMA is also maturing for
low-latency communication. However, the bandwidth is
often underutilized. For instance, in the attention module,
inference traffic between instances is typically unidirec-
tional, flowing from a prefill to a decode instance, while
much of the interconnection is idle. It leaves a large op-
timization space to improve performance by unleashing
the full potential of modern networking.
• Software. A further opportunity exists in optimizing the
communication patterns of prefix cache management. Fig-
ure 3 shows an example. Rather than adhering to transmit-
ting only prefix cache, by combining communication with
Sequence Parallelism (SP) in the prefill phase or transmit-
ting typically smaller query tensors in the decoding phase,
the performance gains from pooling GPU resources across
can often outweigh the communication overhead, because
they enable less communication volume and overlap com-
munication better with computation.
3
System Architecture
3.1
Overview
To address these problems, as shown in Figure 4, we design
and build a unified segment-level prefix cache pool, Token-
Lake, across instances. TokenLake first introduces a declara-
tive prefix cache interface that abstracts cache-aware opera-
tions from request scheduling and exposes it to TokenLake
for better cache management (§3.2). Under the hood, prefix
caches are split into segments to reduce fragmentation. To-
kenLake uses a fully peer-to-peer asynchronous architecture
(§3.3) to enable low-latency asynchronous communication
of cache segments and query tensors (§3.4). With the global
view, TokenLake’s cache manager uses a heavy-hitter-aware
Segment-level Cache Pool
Segment Info
Query
Engine
Transfer
Engine
Compute
Engine
KV
KV
Q/
O&L
Qprefix
Q/
O&L
TokenLake
Model
Parameters
Homogeneous Instance
Scheduler
Global
Prefix Tree
SIB
Cache 
Manager
Dispatch 
Optimizer
TokenLake
Req 
Queue
Runtime
Info
Instance 1
Instance 2
Instance 3
Instance 4
Compute
Cache
Compute
Cache
Compute
Cache
Compute
Cache
Parallel Group 1
Parallel Group 2
Parallel Group 3
Figure 4. System overview.
segment-level load balancing algorithm to achieve better load
balance and conduct aggressive deduplication (§4) of existing
prefix caches. For query tensors and newly generated pre-
fix caches, TokenLake’s dispatch optimizer minimizes their
communication volume with a bipartite matching-based dis-
patching algorithm (§5). Finally, stateless elastic scheduling
can be performed by the scheduler with minimal considera-
tion of the underlying prefix cache (§6).
3.2
Interfaces
Figure 5 shows the declarative interfaces of TokenLake. At
the control plane, get_prefix_tree provides the global pre-
fix tree to the scheduler. The Scheduler can leverage it to
get caching information, e.g., hit rate of each request, slots
utilization, distribution of cache segments, etc, similar to a
local prefix tree in prior works [73]. get_cache_load pro-
vides the GPU load of TokenLake given a set of requests reqs
with specified input lengths. The scheduler can use this in-
formation to avoid interference with TokenLake (§6). In each
iteration, after the scheduler decides batching batches and
their respective degree of parallelism DoPs, gen_plans gen-
erates control flow query_plans and transfer_plans for
the data plane and allocates DoP instances group to each
batch for elastic parallel computing.
At the data plane, the compute engine in each instance
can initialize the query engine and transfer engine with
init_query and init_transfer by respective control flows
to get the shared buffers q_buf and kv_buf in GPU memory.
During inference, the compute engine prepares the query ten-
sor in q_buf and invokes query to get the attention output
on the global prefix cache pool. Similarly, after the compute
engine prepares the layer-wise newly generated KV cache in
kv_buf, it can invoke put to store them in TokenLake asyn-
chronously. Due to the declarative nature of these interfaces,
the compute engine does not need to know how to store
and query the prefix cache. TokenLake automatically opti-
mizes the prefix cache pool and query plans to achieve high
performance. At the same time, the degree of parallelism,
batching of requests, and splitting input sequences are still
fully controlled by the scheduler, enabling elastic scheduling.
4


---

# Control Plane Interfaces
def get_prefix_tree()
-> tree
def get_cache_load(reqs, tree)
-> load
def gen_plans(batches, DoPs, tree)
-> query_plans, transfer_plans, groups
# Data Plane Interfaces
def init_query(batch, group, query_plan)
-> q_buf
def query(q_buf)
-> out
def init_transfer(batch, transfer_plan)
-> kv_buf
def put(kv_buf)
-> None
Figure 5. Interfaces of TokenLake
3.3
Fully peer-to-peer asynchronous architecture
To support fine-grained co-optimization of query tensors,
prefix cache segments, and prefix attention, TokenLake em-
ploys a fully peer-to-peer (P2P) asynchronous architecture.
As shown in Figure 4, the two main components of Token-
Lake: the query engine and the transfer engine, run as inde-
pendent processes, colocated on the GPU with the compute
engine. Powered by NVIDIA Multi-Process Service (MPS) [4],
three engines can run asynchronously and concurrently.
They can even handle different tasks for different batches
concurrently for better performance.
Query and transfer engines themselves also support multi-
tasking. Query engines are responsible for generating partial
outputs and normalizers of the query on local prefix seg-
ments. They use multiple CUDA streams to concurrently
send query tensors to other query engines and handle query
tensors from other query engines concurrently. Similarly,
transfer engines can concurrently transfer KV cache to or
from different instances. This design maximizes the utiliza-
tion of interconnect bandwidth.
3.4
Low-latency asynchronous communication
Powered by the fully P2P asynchronous architecture, it is
possible to achieve low-latency asynchronous communica-
tion for both query tensors and prefix caches.
First, TokenLake enables zero-copy data transfer between
engines. q_buf and kv_buf are shared buffers allocated with
CUDA IPC [45]. As long as the compute engine generates
tensors directly in these buffers, no extra GPU memory copy
is needed between engines. Furthermore, these buffers are
also registered with ncclCommRegister as network buffers,
minimizing memory copies during network transfers.
Second, communication can be overlapped with computa-
tion. As shown in Figure 6, layer-wise KV cache transfer (𝑃𝑢𝑡)
can be overlapped with the subsequent computation of the
entire layer after generation (𝑃𝑘𝑣). Similarly, scattering query
tensors and gathering query results, whose communication
FFN
PKV
PQ
Self Attn & 
Prefix Attn
R PO
FFN
PKV
PQ
Self Attn &
Prefix Attn
R PO
Compute
Engine
Transfer
Engine
Query
Engine
Transfer/Store
Layer-wise KV
Scatter Q
Gather O&LSE
Layer i
Layer i+1
Query
Query
Put
Put
Instance n
FFN
PKV
PQ
Self Attn &
Prefix Attn
R PO
FFN
PKV
PQ
Self Attn &
Prefix Attn
R PO
Compute
Engine
Instance m
SP (DoP=2)
SP (DoP=2)
Transfer
Engine
Instance l
St
St
Query
Engine
Instance k
Prefix
Attn
Prefix
Attn
Figure 6. Interactions of instance 𝑛with remote engines.
volume is linear to the sequence length, can be overlapped
with the self-attention (Self Attn), which is quadratic to the
sequence length.
Third, the communication interference is minimized. Dif-
ferent engines are triggered at different times. The transfer
engine is triggered after 𝑃𝑘𝑣, while the query engine is trig-
gered after 𝑃𝑄. Furthermore, if the scheduler decides to use
sequence parallelism (SP) to perform self-attention across
instances, 𝑄𝑢𝑒𝑟𝑦will use the pass-Q variant of SP [65] that
swaps query tensors within the parallel group in the ring
style and removes redundant transmission of query tensors
for attending to the prefix cache in the SP group. Therefore,
the destined instances of the query engine are different from
the instances involving SP, reducing network contention.
Last but not least, the communication volume is small. For
example, for a Llama-7B model in its decoding phase, the KV
cache transfer at each layer is only 16 KB per request. The
query tensor communication volume is also small, which
is approximately equal to 2𝑑𝑙𝑁𝑝, where 𝑑is hidden size, 𝑙
is the sequence length, and 𝑁𝑝is the number of instances
hosting relevant segments. Compared to the communication
in large-scale Expert Parallelism (EP) [71] that possibly oc-
curs in 𝐹𝐹𝑁, whose volume is equal to 2𝑑𝑙𝑁𝑒, where 𝑁𝑒is
the number of experts, the query engine’s communication
volumes are often comparable (§4), representing a widely
accepted trade-off in practical deployments. The analysis
also aligns with the experimental results in Figure 3.
4
Segment-based Load Balancing
Because the declarative interface exposes query tensors and
prefix attention to TokenLake, prefix cache segments are
essentially independent of each other. Query tensors can at-
tend to them independently, generate partial output tensors,
and normalizers. The final output can be generated by com-
bining them together [67]. Therefore, prefix cache segments
can be split and distributed arbitrarily across instances in
TokenLake. However, even with this flexibility, it is still chal-
lenging to efficiently utilize all GPU memories and memory
bandwidth as a cache pool.
5


---

1
2
3
Prefill: R1
Prefill: R3
Decode: R3
Instance
Prefill: R2
Prefill: R3
Decode: R2
1
1
Prefill: R1
Decode: R1, R2
Decode: R1
Time
$
2
$
1
1
$ 3
$
3
2
$
2
1
1
1
1
1
$
3
3
3
$
2
2
2
$
1
1
1
1
1
1
$
R1, R2
R3
Prefill: R4
Decode: R2, R3
Prefill: R4
4
3
$
3
3
3
2
2
2
$
4
2
$
R4
R1
Figure 7. Limitations of strict locality-based policy.
4.1
Strawman solution: Strict locality-based policy
A straightforward solution is to maintain locality whenever
possible, which we term a strict locality-based policy. It uti-
lizes other instances only when the local cache capacity is
exhausted. However, this strict locality-based policy suffers
from two major drawbacks: severe load imbalance and fre-
quent data migration.
First, it ignores memory bandwidth utilization across in-
stances, leading to load imbalance. As shown in Figure 7, it
makes prefix cache segments concentrate on a few instances
(e.g., instance 3 in iterations 2 and 3, and instance 2 in itera-
tion 4) with high memory bandwidth pressure, while other
instances are under-utilized. This disparity becomes more
pronounced as the variation of sequence lengths grows and
shared prefix lengths become longer.
Second, the policy reacts poorly to the dynamic workload.
Because sequence lengths and phases of requests change over
time, the set of instances occupied by a request also needs to
change accordingly to maximize overall performance [48, 62].
To enforce strict locality, TokenLake triggers frequent unnec-
essary migration to maintain locality. As shown in Figure 7,
to leave computation resources for new requests (𝑅1 and 𝑅2
in iteration 1) or to balance the computation (𝑅1, 𝑅2, and 𝑅3
in iteration 2), segments need to be migrated frequently to
keep strict locality, leading to high communication overhead.
4.2
Segment size tradeoff: locality vs. load balance
In contrast, our key insight is that although locality is im-
portant to reduce communication volume, it is not strictly
necessary for achieving high performance. As long as the seg-
ment size is larger than a certain threshold, the distributed
query could achieve better performance than the local query.
This is because the memory bandwidth and compute of mul-
tiple GPUs can be aggregated to process the prefix attention,
and the communication overhead can be amortized over
the entire segment and overlapped by local self-attention
computation. Furthermore, it also avoids load imbalance and
frequent data migration caused by strict locality.
Consider a prefix cache of sequence length 𝑆partitioned
across 𝑁instances. In each decoding iteration, a single query
tensor 𝑞must attend to the entire prefix cache. The cache on
each instance represents a data segment of size𝐶, where 𝑁=
𝑆/𝐶. The goal is that when the segment resides on a remote
instance, the local computation latency saved is greater than
or equal to the communication overhead incurred.
The saved computation latency to process a segment of
size 𝐶is bound by either compute or memory bandwidth
according to the Roofline model. Let 𝐹be the floating-point
operations per second (FLOPS) of a single GPU, 𝐵𝑚𝑒𝑚be
the memory bandwidth, and 𝑑be the hidden dimension.
The time to process a segment of size 𝐶is: 𝑇𝑐𝑜𝑚𝑝(𝐶) =
max( 4𝐶𝑑
𝐹,
4𝐶𝑑
𝐵𝑚𝑒𝑚) = 𝑘𝑐𝑜𝑚𝑝𝐶, where 𝑘𝑐𝑜𝑚𝑝= max( 4𝑑
𝐹,
4𝑑
𝐵𝑚𝑒𝑚)
is a constant for a given model and hardware.
The communication time consists of two phases, including
sending query tensors and receiving partial output tensors
and normalizers. In the whole process, the total communi-
cation latency is: 𝑇𝑐𝑜𝑚𝑚(𝐶) = 2𝛼𝑛𝑒𝑡+
4𝑑
𝐵𝑛𝑒𝑡, where 𝛼𝑛𝑒𝑡is the
network latency, and 𝐵𝑛𝑒𝑡is the network bandwidth.
If a segment size can make computation latency saved
greater than or equal to the communication latency, then we
can derive: 𝐶≥2𝛼𝑛𝑒𝑡
𝑘𝑐𝑜𝑚𝑝+
4𝑑
𝑘𝑐𝑜𝑚𝑝𝐵𝑛𝑒𝑡. So if we set the segment
size 𝐶to be larger than this threshold, it can harvest mem-
ory bandwidth and compute on remote instances without
incurring significant communication overhead. When we
instantiate this threshold using parameters from our experi-
mental setup (§ 7), 𝐶is approximately equal to 568 tokens,
enabling fine-grained segment-level cache pooling possible.
Furthermore, even if the communication overhead can not
be completely hidden, it is still negligible compared to the
overall computation time. For example, based on the above
analysis, when 𝐶is 568 tokens, the communication overhead
of a short request in the decoding phase with 1000 prefix
tokens is only 4.66 us per layer, a negligible time in practice.
4.3
Heavy-hitter-aware segment-level load balancing
After choosing a segment size larger than the threshold,
the next challenge is how to distribute them to achieve a
balanced load across instances. Highly skewed and dynamic
access patterns make it challenging. Because a segment is
accessed only when the prefix of this segment is accessed,
the access pattern is inherently skewed, concentrating on
segments closer to the root of the prefix tree. Furthermore,
because the workload is dynamic, the access pattern changes
over time. The rapid processing speed exacerbates this issue.
To address this problem, we propose a heavy-hitter-aware
load-balancing algorithm that handles heavy hitters and
normal segments differently. Heavy hitter is defined as fre-
quently accessed segments. For normal segments, TokenLake
employs a hash-based distribution strategy to spread them
uniformly across all instances. A hash function is used to map
each segment to an instance. Given that normal segments
are numerous, this method typically achieves effective load
balancing without migrations and aggressive deduplication.
6


---

R1In
R2In
R3In
R4In
R1 -> I1
R2 -> I2
New KV Caches’
Transfer Plans
Batches
Instances
R3 -> I3
R4 -> I3
3
4
5
3
2
1
2
1
1
Instance 1
Instance 2
Instance 3
Figure 8. Example of dispatching problem.
As for heavy hitters, TokenLake employs a selective repli-
cation strategy to maintain load balance. If an instance is
overloaded, it replicates heavy hitter segments on that in-
stance to the least-loaded instances without its copy. Because
heavy hitters reside closer to the prefix tree’s root, Token-
Lake can find them by using a breadth-first search (BFS) with-
out traversing the entire prefix tree. To make the replication
efficiently, our approach is informed by a well-established
result from early front-end caching systems [17]: if a sys-
tem can effectively absorb the traffic from the top 𝑂(𝑁log 𝑁)
heavy hitters, the load across 𝑁storage servers is guaranteed to
be balanced with high probability, regardless of the query dis-
tribution or the total number of objects. Therefore, the number
of heavy hitters is set to 𝑂(𝑁log 𝑁) for quick replication.
4.4
Replicated segment selection and eviction
For replicated segments, TokenLake uses the power-of-two-
choices (PoT) policy to select a replica for matched requests.
Specifically, it directs the query to the less loaded of the
two randomly sampled replicas. It is a variant of the balls-
into-bins problem and has been proven that it can achieve
near-perfect load balancing with high probability if traffic to a
segment is less than half of the total traffic [40], which always
holds in TokenLake. For eviction, the global LRU policy is
used by recording the last access time of replicas.
5
Dispatching Optimization
5.1
Problem formulation
Although the load balancing algorithm can effectively utilize
GPU memories and memory bandwidth, the communication
volume of query and put operations can still change accord-
ing to the dispatching of requests. Specifically, for a batch
of requests provided by the scheduler, if segments required
by the batch are predominantly located on an instance, or if
the newly generated KV caches are primarily intended to be
stored on an instance, TokenLake can dispatch the batch to
that instance to reduce communication volume.
Figure 8 illustrates an example where the scheduler has
grouped four requests into three batches. The communica-
tion volume generated by each batch depends on the instance
it is placed on. For example, consider the batch containing
request 𝑅3. If it is executed on Instance 3, it only needs to
send one query to Instance 2 for attending related segments.
Then the communication volume of this dispatching is 1×4𝑑.
If, however, it is executed on Instance 1, a query tensor and
the newly generated KV cache need to be sent to Instance 2
for query and Instance 3 for put, leading to 2 × 4𝑑communi-
cation volume. Therefore, for efficient memory pooling, the
dispatching of batches should be optimized to minimize the
overall communication volume.
5.2
Bipartite matching-based dispatching algorithm
Regarding this problem, our key insight is that the dispatch-
ing can be decoupled from the computation scheduling. Be-
cause load has been actively balanced across instances (§4),
where to place the computation has no significant impact on
the computation. Therefore, dispatching can be optimized as
an independent problem and be transparent to the scheduler.
The scheduler only needs to invoke gen_plans with batch-
ing specifications and their respective DoP. TokenLake can
optimize dispatching plans transparently.
For this problem, our key observation is that it can be
converted into a bipartite matching problem to efficiently
find the solution with the minimum communication volume.
Specifically, we construct a bipartite graph 𝐺= (𝑈,𝑉, 𝐸),
where 𝑢𝑖∈𝑈represents a batch with its transfer plan, and
𝑉is the set of instances. Let 𝑄(𝑢𝑖) be the set of instances
caching segments related to 𝑢𝑖, 𝑃(𝑢𝑖) be the set of instances
where new KV cache of𝑢𝑖will be cached and 𝑁𝑝(𝑢𝑖, 𝑣𝑘) is the
number of new KV cache of 𝑢𝑖sent to instance 𝑣𝑘, according
to the transfer plans. For a batch with DoP greater than one,
the corresponding node 𝑢𝑖needs to be decomposed into 𝐷𝑜𝑃
sub-batch nodes 𝑢′
𝑖1 · · ·𝑢′
𝑖𝑑, and the above functions are then
defined for each new sub-batch node according to the actual
communication of that sub-batch. Then the weight of an
edge from 𝑢𝑖∈𝑈to 𝑣𝑗∈𝑉is equal to:
𝑒(𝑢𝑖, 𝑣𝑗) = −©­
«
∑︁
𝑣𝑘≠𝑣𝑗,𝑣𝑘∈𝑄(𝑢𝑖)
4𝑑+
∑︁
𝑣𝑘≠𝑣𝑗,𝑣𝑘∈𝑃(𝑢𝑖)
4𝑑∗𝑁𝑝(𝑢𝑖, 𝑣𝑘)ª®
¬
With this construction, we convert the problem of mini-
mizing communication volume to finding a perfect matching
in 𝐺with minimal edge weights. It can be solved by us-
ing the Hungarian algorithm [5]. The time complexity of
this algorithm is 𝑂(𝑛3), where 𝑛is the number of instances.
Therefore, TokenLake can efficiently find the dispatching
plan with the minimal communication volume by invoking
this algorithm at the beginning of each iteration.
6
Stateless Elastic Scheduling
6.1
Cache load isolation
With TokenLake, the scheduler can still serve for different
objectives and combine various scheduling strategies, such
as PD disaggregation [48], chunked prefill [24], and elastic
7


---

sequence parallelism [62], as usual, and does not need to
manage prefix cache. The only difference is that TokenLake
inevitably occupies some GPU resources for prefix cache
management. But as long as the scheduler is aware of this
load, it can adjust its scheduling accordingly to avoid inter-
ference. For example, it can reduce the batch size, split long
sequences into chunks, or use sequence parallelism to reduce
the load on each instance. Because the scheduling becomes
stateless, these adjustments can be made efficiently.
In the end, TokenLake provides a get_cache_load API
to expose its own resource consumption. Specifically, Token-
Lake calculates the percentage of GPU resources consumed
by TokenLake for requests (reqs) in the worst case. First,
it estimates the minimum execution time of reqs, 𝑇𝑟, un-
der an ideal scenario where all GPUs are fully utilized. This
estimation is based on pre-profiled results and follows the
quadratic curve fitting method as prior work [62]. Next, the
memory bandwidth and computation consumption 𝑀𝑟and
𝐹𝑟of TokenLake are respectively calculated as follows:
𝑀𝑟=
∑︁
𝑟∈𝑟𝑒𝑞𝑠
4 × 𝑑× (𝑟.𝑝𝑟𝑒𝑓𝑖𝑥_𝑙𝑒𝑛+ 𝑟.𝑖𝑛𝑝𝑢𝑡_𝑙𝑒𝑛)
𝐹𝑟=
∑︁
𝑟∈𝑟𝑒𝑞𝑠
2 × 𝑑× 𝑟.𝑝𝑟𝑒𝑓𝑖𝑥_𝑙𝑒𝑛× 𝑟.𝑖𝑛𝑝𝑢𝑡_𝑙𝑒𝑛
Based on the roofline model, the percentage of GPU resources
consumed by TokenLake is then calculated as:
𝐿= max(
𝑀𝑟
𝑀𝑟+ 𝑁× 𝐵𝑚𝑒𝑚×𝑇𝑟
,
𝐹𝑟
𝐹𝑟+ 𝑁× 𝐹×𝑇𝑟
)
Because the load of TokenLake is balanced (§4), 𝐿is indepen-
dent of instances. With this information, the scheduler can
factor the load of TokenLake into its scheduling decisions.
6.2
Goodput-optimized stateless elastic scheduling
To showcase the flexibility and potential of elastic scheduling
on TokenLake, we designed a goodput-optimized stateless
elastic scheduling algorithm. To optimize the overall goodput
for requests with varying lengths and in different phases,
our optimization objective is to minimize the overall latency,
subject to the constraint that the output token latency, a.k.a
Time Between Tokens (TBT), meets a Service Level Objective
(SLO). This objective first minimizes the overall processing
time. Second, it allows more requests in the prefill phase to
enter the decoding phase quickly. This, in turn, increases
the batch size during the decoding phase, thereby improving
overall GPU efficiency.
To tackle this optimization problem, in each iteration, fol-
lowing existing systems [24, 31, 73], we first use chunked
prefill to split the long context to avoid overloading the whole
cluster. Then it can be formulated as a dynamic program-
ming (DP) problem to decide batching and DoP of each batch.
Since requests with similar context lengths exhibit similar
characteristics, we first sort all requests by their context
length. Let 𝑓(𝑖,𝑘) represent the minimum overall latency for
the first 𝑖requests given 𝑘instances. The DP equation can
be formulated as follows:
𝑓(𝑖,𝑘) =
min
0≤𝑗<𝑖,0≤𝑙<𝑘,
𝑆𝐿𝑂𝑗+1...𝑖≤𝑇(𝑗+1,𝑖,𝑘−𝑙,𝐿)
𝑓(𝑗,𝑙) + (𝑖−𝑗) ×𝑇(𝑗+1,𝑖,𝑘−𝑙, 𝐿)
where𝑇(𝑗+1,𝑖,𝑘−𝑙) is the latency to batch requests from 𝑗+1
to𝑖using𝑘−𝑙instances when cache load is 𝐿. The time is also
estimated by the quadratic curve fitting method [62]. Then
the scheduling plan is constructed by backtracking from
min𝑘≤𝑁𝑓(𝑀,𝑘), where 𝑀is the total number of requests.
To ensure PD disaggregation, we constrain requests in a
batch to be in the same phase. If the SLO actually cannot
be met, the algorithm will fall back to throughput-oriented
scheduling by ignoring the SLO constraint to mitigate head-
of-line blocking. Because DoP of request in the decoding
phase must be one, the algorithm complexity is 𝑂(𝑀2
𝑝× 𝑁2),
where 𝑀𝑝is the number of requests in the prefill phase.
7
Evaluation
We implement TokenLake as a prototype based on FlashIn-
fer [67], SGLang [73], ZMQ [6], Ray [42], and LoongServe [62]
in 16k LoCs. TokenLake uses CUDA IPC to share GPU mem-
ory among engines and uses inter-process CUDA events and
Python semaphores for interactions between the engines. In
this section, we evaluate it with state-of-the-art systems on
real-world workloads to show its effectiveness.
Experimental Setup. We conduct experiments on servers
with 8 NVIDIA A100 80 GB GPUs and 128 CPUs. Most of the
experiments are conducted on one server, and we evaluate
multi-node performance in Section 7.5. The NVLink band-
width is 400 GB/s. The servers are interconnected via four
200 Gbps InfiniBand NICs. We use CUDA 12.4 and NCCL
2.24.3. Similar to prior works [35, 62], we use the LWM-1M-
Text, a long-context Llama-2-7B model [58] for experiments.
Workloads As prior works [31, 62, 74], the requests are
sampled from the three real-world datasets with the arrival
pattern generated by a Poisson process. LooGLE [33] is a
popular dataset of requests with shared prefixes. The average
sequence length is 24K. SCBench [34] is a dataset of long-
context multi-turn sessions. The average sequence length is
227K, and the average number of turns is 5. ShareGPT [2]
is a conversation dataset with short requests. The sequence
length is less than 2.4K. To mimic the real-world scenario,
we add a system prompt of OpenAI o3 [1] to each request.
Baselines. vLLM [31] and SGLang [73] are two state-of-
the-art open-sourced LLM serving ecosystems without a
significant difference. Because we reuse some components
from SGLang, we choose solutions on top of it to ensure a
fair comparison. Specifically, SGLang-MoonCake-xPyD is a
cache-centric PD disaggregation solution based on Moon-
Cake [50] and SGLang. Because the P:D ratio can affect per-
formance, we evaluate two configurations, P:D = 1:3 and P:D
= 3:1. SGLang-Router is a cache-aware routing solution by
combining multiple SGLang instances and an SGL-router. It
8


---

0.0
0.5
1.0
1.5
2.0
2.5
0
50
100
LooGLE
SLO Attainment (%)
0.0
0.5
1.0
1.5
2.0
2.5
0.0
0.5
1.0
Normalized Latency
(s/token)
0.0
0.5
1.0
1.5
2.0
2.5
0.0
0.5
1.0
Normalized Latency
(s/token)
0.0
0.1
0.2
0.3
0.4
0.5
0
50
100
SCBench
SLO Attainment (%)
0.0
0.1
0.2
0.3
0.4
0.5
0.0
0.5
1.0
Normalized Latency
(s/token)
0.0
0.1
0.2
0.3
0.4
0.5
0.0
0.5
1.0
Normalized Latency
(s/token)
0.0
2.5
5.0
7.5
10.0
12.5
15.0
Request Rate (req/s)
Goodput
0
50
100
ShareGPT
SLO Attainment (%)
0.0
2.5
5.0
7.5
10.0
12.5
15.0
Request Rate (req/s)
Input Token Latency
0.0
0.5
1.0
Normalized Latency
(s/token)
0.0
2.5
5.0
7.5
10.0
12.5
15.0
Request Rate (req/s)
Output Token Latency
0.0
0.5
1.0
Normalized Latency
(s/token)
TokenLake
SGLang-Router
SGLang-MoonCake-1P3D
SGLang-MoonCake-3P1D
SLO
Figure 9. End-to-end performance.
considers both cache hit rate and load balancing to route
requests. Similar to prior works [15, 62], tensor parallelism
(TP) is set to 2, and chunked prefill with the same chunked
prefill size is enabled.
Metrics. For prefill performance, we measure the input
token latency [37, 62], i.e., time to first token (TTFT) nor-
malized by the input length. For decoding performance, we
measure the output token latency, i.e. the time between to-
kens (TBT). For the SLO attainment, we set the SLO to be
that all input and output token latency is less than 10× the
processing time when the batch size is 1. The P90 goodput
is defined as the maximum throughput of the system when
90% of requests meet the SLO.
7.1
End-to-end performance
We compare TokenLake against three distinct baselines across
three different datasets on three key metrics. As shown in Fig-
ure 9, TokenLake outperforms the other baselines on nearly
all datasets, particularly in SLO attainment.
The first row shows the results on LooGLE. For SGLang-
Router, although it attempts to balance computation load,
it still tends to direct requests to instances with related pre-
fixes. Given the limited local cache size, this often results in
cache thrashing and costly recomputation. Consequently, at
the same input token latency, its throughput is 4.64× lower
than that of TokenLake. Its output token latency is also high
due to the interference from prefill requests. In contrast,
SGLang-MoonCake-1P-3D and SGLang-MoonCake-3P-1D,
although protecting the decoding phase, only 25% and 75%
of prefix cache slots can be used during the prefill phase,
respectively, leading to a lower cache hit rate. Furthermore,
because only 75% and 25% of prefix cache slots can be used
during the decoding phase, requests in the prefill phase have
to wait for cache slots in decode instances to be released,
which also inflates the input token latency. By efficiently
pooling GPU memories, TokenLake improves P90 goodput by
2.08× and 2.04× over SGLang-MoonCake-1P-3D and SGLang-
MoonCake-3P-1D, respectively. At equivalent input token
latencies, the throughput improvements are 4.46× and 3.32×.
On SCBench, because it contains multi-turn conversation,
which involves multiple cycles of prefill and decoding phases,
cache-centric PD disaggregation has to frequently transfer
prefix caches between prefill and decode instances, creat-
ing substantial transfer overhead. Fragmentation between
two phases also decreases the hit rate. Therefore, SGLang-
MoonCake-1P-3D and SGLang-MoonCake-3P-1D can only
serve about 20% of requests, essentially the first turn of a
conversation, under the SLO. Although SGLang-Router does
not suffer from this issue, it still causes a low hit rate due to
inherent fragmentation and load imbalance across instances.
Consequently, TokenLake achieves 2.55× higher P90 good-
put than SGLang-Router. Moreover, because TokenLake em-
ploys PD disaggregation, its decoding phase latency is also
significantly lower than SGLang-Router’s.
Finally, on the ShareGPT dataset, TokenLake demonstrates
the flexibility of its interfaces. Different from SGLang-Router,
which sacrifices the SLO attainment, it prevents interference
between the two phases as PD disaggregation does, while
empowering the dynamic adjustments of the resources allo-
cated to the two phases based on real-time workload. As a
result, compared to SGLang-MoonCake-1P-3D and SGLang-
MoonCake-3P-1D with the static partitioning, TokenLake
improves the P90 goodput by 3.71× and 1.56× respectively.
7.2
Load balancing comparison
To compare the load-balancing capability, we monitored the
prefix cache accesses on each instance at a request rate ap-
proaching TokenLake’s P90 SLO threshold. Figure 10 shows
9


---

0
20
40
60
80
100
Time (seconds)
0
20000
40000
60000
#Cache Access
(tokens/iteration)
0%
50%
100%
150%
200%
CV
Instance-0
Instance-1
Instance-2
Instance-3
CV
(a) TokenLake
0
20
40
60
80
100
120
Time (seconds)
0
100000
200000
300000
#Cache Access
(tokens/iteration)
0%
50%
100%
150%
200%
CV
Instance-0
Instance-1
Instance-2
Instance-3
CV
(b) SGLang-Router
0
50
100
150
200
Time (seconds)
0
20000
40000
60000
80000
100000
#Cache Access
(tokens/iteration)
0%
50%
100%
150%
200%
CV
Decode-0
Decode-1
Decode-2
Prefill-0
CV
(c) SGLang-MoonCake-1P3D
0
50
100
150
200
250
Time (seconds)
0
50000
100000
150000
200000
#Cache Access
(tokens/iteration)
0%
50%
100%
150%
200%
CV
Decode-0
Prefill-0
Prefill-1
Prefill-2
CV
(d) SGLang-MoonCake-3P1D
Figure 10. Load balancing of different systems.
200000
400000
800000
#Cache Slots
0%
20%
40%
60%
80%
100%
Hit Rate
TokenLake
SGLang-MoonCake-1P3D
SGLang-MoonCake-3P1D
SGLang-Router
Figure 11. Hit rate under different cache sizes.
both the per-instance cache access pattern over time and the
corresponding Coefficient of Variation (CV), which serves
to quantify the degree of load imbalance across instances.
As shown in Figure 10a, the CV of TokenLake remains
steadily below 15%, with an average value of just 11%. This
result demonstrates the effectiveness of TokenLake’s load-
balancing mechanism over dynamic workloads.
As shown in Figure 10b, SGLang-Router exhibits signif-
icant shortcomings in load balancing. Although SGLang
Router takes load conditions into account when routing re-
quests, without memory pooling, it has to trade load balance
for a higher hit rate. Therefore, its average CV reached 99%,
substantially 9× higher than that of TokenLake.
As shown in Figure 10c and Figure 10d, due to disaggrega-
tion, both SGLang-MoonCake-1P3D and SGLang-MoonCake-
3P1D exhibit significant load imbalance. Furthermore, the
load imbalance also occurs across decode instances of SGLang-
MoonCake-1P3D and across prefill instances of SGLang-
MoonCake-3P1D. The figure also reveals that without mem-
ory pooling, they serve initial requests at a low hit rate. Ulti-
mately, their average CV reaches 62% and 122%, respectively,
5.63× and 11.09× higher than that of TokenLake.
7.3
Hit rate analysis
We then analyzed the hit rate of each system under varying
cache sizes using the LooGLE dataset. As shown in Figure 11,
the hit rate of TokenLake is the highest across all cache sizes
and consistently improves as the capacity increases. In con-
trast, the SGLang-MoonCake-1P3D and SGLang-MoonCake-
3P1D suffer from their isolated prefix caches for the prefill
and decoding phases, so their hit rates are 1.67-2.14× and
25000
50000
75000
SGLang-MoonCake-1P3D
TokenLake-1P3D
SGLang-MoonCake-3P1D
TokenLake-3P1D
TokenLake-Elastic (Default)
200000
400000
800000
#Cache Slots
0
2000
4000
Request Latency (ms)
Figure 12. Effectiveness of pooling and elasticity.
1.78-2.70× lower than TokenLake’s, respectively. SGLang-
Router also exhibits a low hit rate due to the limitations
of its isolated local caches, which lead to load imbalance,
data redundancy, and memory fragmentation. Furthermore,
the interference from the requests in the prefill phase slows
down its decoding phase performance, which in turn dimin-
ishes cache utilization efficiency. Therefore, the hit rate of
TokenLake is 1.72-2.04× higher than SGLang-Router’s.
7.4
Effectiveness of pooling and elasticity
To evaluate the respective effectiveness of the TokenLake
interface to support elastic scheduling and prefix cache pool-
ing, we implemented PD disaggregation (TokenLake-1P3D
and TokenLake-3P1D) on top of TokenLake to isolate their
contributions. As shown in Figure 12, first, the latency of
TokenLake-1P3D and TokenLake-3P1D is 26.62-86.05× and
2.18-39.48× lower than that of SGLang-MoonCake-1P3D and
SGLang-MoonCake-3P1D, respectively, showing the effec-
tiveness of prefix cache pooling. Second, our fully-featured
system, TokenLake-Elastic, consistently outperforms other
TokenLake variants across all cache sizes. The performance
advantage becomes more pronounced as the cache size de-
creases, reducing latency by up to 5.22× and 2.82× compared
to TokenLake-1P3D and TokenLake-3P1D, respectively. This
result validates the effectiveness of the TokenLake interface
to enable stateless elastic scheduling.
7.5
Multi-node performance
Finally, we also tested the multi-node performance of Token-
Lake on 16 A800 80GB GPUs across two servers. We mixed
10


---

0.0
0.2
0.4
0.6
0.8
1.0
1.2
1.4
Request Rate (req/s)
(a) Goodput
0
25
50
75
100
ShareGPT+LooGLE+SCBench
SLO Attainment (%)
0.0
0.2
0.4
0.6
0.8
1.0
1.2
1.4
Request Rate (req/s)
(b) Input Token Latency
0.00
0.25
0.50
0.75
1.00
Normalized Latency
(s/token)
0.0
0.2
0.4
0.6
0.8
1.0
1.2
1.4
Request Rate (req/s)
(c) Output Token Latency
0.00
0.25
0.50
0.75
1.00
Normalized Latency
(s/token)
TokenLake
SGLang-Router
SGLang-MoonCake-2P6D
SGLang-MoonCake-6P2D
SLO
Figure 13. Multi-node performance.
the three datasets in a 1:1:1 ratio as a more dynamic workload.
The PD disaggregation baselines are adjusted to SGLang-
MoonCake-6P2D and SGLang-MoonCake-2P6D to maintain
the original PD ratios. As shown in Figure 13, TokenLake still
outperforms the baselines in multi-node scenarios. Due to the
increased variability in requests from the mixed dataset, the
problem of load imbalance, redundancy, and fragmentation is
more severe. Similar to Section 7.1, SGLang-MoonCake-6P2D
and SGLang-MoonCake-2P6D struggle to handle multi-turn
interactions, while SGLang-Router’s attempt to improve hit
rates leads to load imbalance and interference between the
two phases. Ultimately, they fail to achieve 90% SLO attain-
ment. Additionally, under the same input token latency, To-
kenLake’s throughput is 5.47×, 28.49×, and 5.46× higher than
that of SGLang-MoonCake-6P2D, SGLang-MoonCake-2P6D,
and SGLang-Router, respectively. Furthermore, because only
25% of prefix cache slots in SGLang-MoonCake-6P2D is avail-
able to its decoding phase, it has to frequently trigger recom-
putation, leading to a substantial output token latency.
8
Related Work
LLM serving. Modern LLM serving systems combine many
techniques for better performance. Continuous batching [68]
is proposed to batch different requests. To reduce interfer-
ence between two phases, chunked prefill [8, 24, 30] is pro-
posed to split the long context into smaller chunks, while
PD disaggregation [27, 48, 74] is proposed to disaggregate
two phases into different instances. Elastic sequence paral-
lelism [62] is proposed to handle variable context lengths
and frequent ratio changes between two phases by setting
DoP elastically. Many recent works also explore dynamic PD
instance reconfiguration [16, 18, 29, 49, 51, 60, 72] and multi-
plexing partial computation between two phases [15, 25, 35,
37, 52] to accommodate dynamic workloads. These works fo-
cus on computation optimizations, while TokenLake focuses
on prefix cache pooling and provides declarative interfaces
to support them. AF disaggregation [14, 22, 59, 63, 75] is
proposed to disaggregate attention and FFN. Much research
focuses on expert parallelism [21, 23, 32, 70, 71] and sequence
parallelism [9, 11, 12, 39, 54, 65] to efficiently parallelize the
computation of FFN and attention modules across multiple
devices. They are orthogonal to TokenLake.
KV cache optimizations. Reusing KV caches is critical
to accelerate LLM serving. PagedAttention [31] manages
the KV cache as paged memory to reduce fragmentation.
SGLang [73] manages prefix cache as radix tree to enable
efficient prefix sharing in an instance. Many prefix caching
systems, such as Mooncake [50], LMCache [3], Aibrix [57],
Preble [56], and MemServe [26], explore cache reuse across
instances, while suffering from load imbalance, redundancy,
and fragmentation as discussed in Section 2. Infinite-LLM [36]
borrows the KV cache slots from other instances when local
memory is insufficient, but it does not support prefix shar-
ing between requests, leading to redundancy, uses the strict
locality-based policy, leading to load imbalance, tightly cou-
ples the cache-free computation scheduling, restricting the
elasticity, and cannot concurrently handle prefix attention
and cache-free computation in an instance. Many opera-
tors are developed to optimize prefix attention [55, 66, 67],
and the cache policy is explored to optimize hit rate [61].
They are orthogonal to TokenLake. Many works also explore
hierarchical KV cache management [19, 28, 47, 64, 69, 76].
Because GPU memory often has higher bandwidth than host
memory, and cache typically needs to be swapped back to
GPU memory when used, TokenLake can be used together
with these solutions to further improve the performance.
9
Conclusion
In this paper, we present TokenLake, a unified segment-level
prefix cache pool for elastic long-context LLM serving. By
designing a declarative interface to expose query tensors
and prefix attention, TokenLake can manages prefix cache
at the segment level to reduce fragmentation and enable
fine-grained cache management. Based on it, we propose a
heavy-hitter-aware segment-level load balancing algorithm
to achieve better load balance and deduplication, and pro-
pose the bipartite matching-based dispatching algorithm
to minimize the communication volume of query tensors
and new prefix caches. Finally, TokenLake enables stateless
elastic scheduling to accommodate dynamic workloads. The
evaluation results on real-world workloads demonstrate that
TokenLake significantly improves throughput and hit rate
compared to existing cache-aware routing and cache-centric
PD-disaggregation solutions.
11


---

References
[1] 2023. PromptCraft: The Ultimate GPT System Prompt Collection. https:
//github.com/LouisShark/chatgpt_system_prompt/tree/main. (2023).
[2] 2023. ShareGPT Teams. https://sharegpt.com/. (2023).
[3] 2024. LMCache. https://github.com/LMCache/LMCache. (2024).
[4] 2025. CUDA Multi-Process Service. (2025). https://docs.nvidia.com/
deploy/pdf/CUDA_Multi_Process_Service_Overview.pdf.
[5] 2025. Hungarian algorithm. https://en.wikipedia.org/wiki/Hungarian_
algorithm. (2025).
[6] 2025. The ZeroMQ project. https://github.com/zeromq. (2025).
[7] Saksham Agarwal, Arvind Krishnamurthy, and Rachit Agarwal. 2023.
Host Congestion Control. In ACM SIGCOMM.
[8] Amey Agrawal, Nitin Kedia, Ashish Panwar, Jayashree Mohan, Nipun
Kwatra, Bhargav Gulavani, Alexey Tumanov, and Ramachandran Ram-
jee. 2024. Taming Throughput-Latency tradeoff in LLM inference with
Sarathi-Serve. In USENIX OSDI.
[9] Amey Agrawal, Haoran Qiu, Junda Chen, Íñigo Goiri, Chaojie Zhang,
Rayyan Shahid, Ramachandran Ramjee, Alexey Tumanov, and Esha
Choukse. 2024. Medha: Efficiently Serving Multi-Million Context
Length LLM Inference Requests Without Approximations. arXiv
(2024).
[10] Anthropic. 2025. Claude Sonnet 4 now supports 1M tokens of context.
https://www.anthropic.com/news/1m-context. (2025).
[11] Nidhi Bhatia, Ankit More, Ritika Borkar, Tiyasa Mitra, Ramon Matas,
Ritchie Zhao, Maximilian Golub, Dheevatsa Mudigere, Brian Phar-
ris, and Bita Darvish Rouhani. 2025. Helix Parallelism: Rethinking
Sharding Strategies for Interactive Multi-Million-Token LLM Decod-
ing. arXiv (2025).
[12] William Brandon, Aniruddha Nrusimha, Kevin Qian, Zachary Ankner,
Tian Jin, Zhiye Song, and Jonathan Ragan-Kelley. 2023. Striped atten-
tion: Faster ring attention for causal transformers. arXiv (2023).
[13] Charactor.AI. 2024. Optimizing AI Inference at Character.AI. https:
//research.character.ai/optimizing-inference/. (2024).
[14] Shaoyuan Chen, Wencong Xiao, Yutong Lin, Mingxing Zhang, Yingdi
Shan, Jinlei Jiang, Kang Chen, and Yongwei Wu. 2024. Efficient Het-
erogeneous Large Language Model Decoding with Model-Attention
Disaggregation. arXiv (2024).
[15] Weihao Cui, Yukang Chen, Han Zhao, Ziyi Xu, Quan Chen, Xusheng
Chen, Yangjie Zhou, Shixuan Sun, and Minyi Guo. 2025. Optimizing
SLO-oriented LLM Serving with PD-Multiplexing. arXiv (2025).
[16] Jiangsu Du, Hongbin Zhang, Taosheng Wei, Zhenyi Zheng, Kaiyi
Wu, Zhiguang Chen, and Yutong Lu. 2025. EcoServe: Enabling Cost-
effective LLM Serving with Proactive Intra-and Inter-Instance Orches-
tration. arXiv (2025).
[17] Bin Fan, Hyeontaek Lim, David G Andersen, and Michael Kaminsky.
2011. Small cache, big effect: Provable load balancing for randomly
partitioned cluster services. In ACM Symposium on Cloud Computing.
[18] Jingqi Feng, Yukai Huang, Rui Zhang, Sicheng Liang, Ming Yan, and
Jie Wu. 2025. WindServe: Efficient Phase-Disaggregated LLM Serving
with Stream-based Dynamic Scheduling. In ACM/IEEE ISCA.
[19] Bin Gao, Zhuomin He, Puru Sharma, Qingxuan Kang, Djordje Jevdjic,
Junbo Deng, Xingkun Yang, Zhou Yu, and Pengfei Zuo. 2024. Cost-
Efficient large language model serving for multi-turn conversations
with CachedAttention. In USENIX ATC.
[20] Google.
2024.
Our
next-generation
model:
Gemini
1.5.
https://blog.google/technology/ai/google-gemini-next-generation-
model-february-2024/. (2024).
[21] Jiaao He, Jiezhong Qiu, Aohan Zeng, Zhilin Yang, Jidong Zhai, and Jie
Tang. 2021. Fastmoe: A fast mixture-of-expert training system. arXiv
preprint arXiv:2103.13262 (2021).
[22] Jiaao He and Jidong Zhai. 2024. Fastdecode: High-throughput gpu-
efficient llm serving using heterogeneous pipelines. arXiv (2024).
[23] Jiaao He, Jidong Zhai, Tiago Antunes, Haojie Wang, Fuwen Luo,
Shangfeng Shi, and Qin Li. 2022. Fastermoe: modeling and optimizing
training of large-scale dynamic pre-trained models. In ACM PPoPP.
[24] Connor Holmes, Masahiro Tanaka, Michael Wyatt, Ammar Ahmad
Awan, Jeff Rasley, Samyam Rajbhandari, Reza Yazdani Aminabadi,
Heyang Qin, Arash Bakhtiari, Lev Kurilenko, and Yuxiong He. 2024.
Deepspeed-fastgen: High-throughput text generation for llms via mii
and deepspeed-inference. arXiv (2024).
[25] Ke Hong, Lufang Chen, Zhong Wang, Xiuhong Li, Qiuli Mao, Jianping
Ma, Chao Xiong, Guanyu Wu, Buhe Han, Guohao Dai, Yun Liang,
and Yu Wang. 2025. semi-PD: Towards Efficient LLM Serving via
Phase-Wise Disaggregated Computation and Unified Storage. arXiv
(2025).
[26] Cunchen Hu, Heyang Huang, Junhao Hu, Jiang Xu, Xusheng Chen,
Tao Xie, Chenxi Wang, Sa Wang, Yungang Bao, Ninghui Sun, and
Yizhou Shan. 2024. Memserve: Context caching for disaggregated llm
serving with elastic memory pool. arXiv (2024).
[27] Cunchen Hu, Heyang Huang, Liangliang Xu, Xusheng Chen, Jiang
Xu, Shuang Chen, Hao Feng, Chenxi Wang, Sa Wang, Yungang Bao,
Ninghui Sun, and Yizhou Shan. 2024. Inference without interference:
Disaggregate llm inference for mixed downstream workloads. arXiv
(2024).
[28] Xuanlin Jiang, Yang Zhou, Shiyi Cao, Ion Stoica, and Minlan Yu. 2024.
Neo: Saving gpu memory crisis with cpu offloading for online llm
inference. arXiv (2024).
[29] Hongyi Jin, Ruihang Lai, Charlie F Ruan, Yingcheng Wang, Todd C
Mowry, Xupeng Miao, Zhihao Jia, and Tianqi Chen. 2024. A System
for Microserving of LLMs. arXiv (2024).
[30] Aditya K Kamath, Ramya Prabhu, Jayashree Mohan, Simon Peter,
Ramachandran Ramjee, and Ashish Panwar. 2025. Pod-attention:
Unlocking full prefill-decode overlap for faster llm inference. In ACM
ASPLOS.
[31] Woosuk Kwon, Zhuohan Li, Siyuan Zhuang, Ying Sheng, Lianmin
Zheng, Cody Hao Yu, Joseph Gonzalez, Hao Zhang, and Ion Stoica.
2023. Efficient memory management for large language model serving
with pagedattention. In ACM SOSP.
[32] Jiamin Li, Yimin Jiang, Yibo Zhu, Cong Wang, and Hong Xu. 2023.
Accelerating Distributed MoE Training and Inference with Lina. In
USENIX ATC.
[33] Jiaqi Li, Mengmeng Wang, Zilong Zheng, and Muhan Zhang. 2024.
LooGLE: Can Long-Context Language Models Understand Long Con-
texts?. In Association for Computational Linguistics (ACL).
[34] Yucheng Li, Huiqiang Jiang, Qianhui Wu, Xufang Luo, Surin Ahn,
Chengruidong Zhang, Amir H. Abdi, Dongsheng Li, Jianfeng Gao,
Yuqing Yang, and Lili Qiu. 2025. SCBench: A KV Cache-Centric Analy-
sis of Long-Context Methods. In International Conference on Learning
Representations (ICLR).
[35] Yunkai Liang, Zhangyu Chen, Pengfei Zuo, Zhi Zhou, Xu Chen, and
Zhou Yu. 2025. Injecting Adrenaline into LLM Serving: Boosting
Resource Utilization and Throughput via Attention Disaggregation.
arXiv (2025).
[36] Bin Lin, Chen Zhang, Tao Peng, Hanyu Zhao, Wencong Xiao, Minmin
Sun, Anmin Liu, Zhipeng Zhang, Lanbo Li, Xiafei Qiu, Shen Li, Zhi-
gang Ji, Tao Xie, Yong Li, and Wei Lin. 2024. Infinite-llm: Efficient llm
service for long context with distattention and distributed kvcache.
arXiv (2024).
[37] Zejia Lin, Hongxin Xu, Guanyi Chen, Xianwei Zhang, and Yutong Lu.
2025. Bullet: Boosting GPU Utilization for LLM Serving via Dynamic
Spatial-Temporal Orchestration. arXiv (2025).
[38] Hao Liu, Wilson Yan, Matei Zaharia, and Pieter Abbeel. 2025. World
Model on Million-Length Video And Language With Blockwise
RingAttention. In International Conference on Learning Representa-
tions (ICLR).
12


---

[39] Hao Liu, Matei Zaharia, and Pieter Abbeel. 2023. Ring attention with
blockwise transformers for near-infinite context. arXiv (2023).
[40] Zaoxing Liu, Zhihao Bai, Zhenming Liu, Xiaozhou Li, Changhoon Kim,
Vladimir Braverman, Xin Jin, and Ion Stoica. 2019. DistCache: Provable
Load Balancing for Large-Scale Storage Systems with Distributed
Caching. In USENIX FAST.
[41] Manus. 2025.
Context Engineering for AI Agents: Lessons from
Building Manus. https://manus.im/blog/Context-Engineering-for-
AI-Agents-Lessons-from-Building-Manus. (2025).
[42] Philipp Moritz, Robert Nishihara, Stephanie Wang, Alexey Tumanov,
Richard Liaw, Eric Liang, Melih Elibol, Zongheng Yang, William Paul,
Michael I. Jordan, and Ion Stoica. 2018. Ray: A Distributed Framework
for Emerging AI Applications. In USENIX OSDI.
[43] NVIDIA. 2020. NVIDIA A100 Tensor Core GPU. https://www.nvidia.
com/en-us/data-center/a100/. (2020).
[44] NVIDIA. 2024. NVIDIA Dynamo. https://github.com/ai-dynamo/
dynamo. (2024).
[45] NVIDIA. 2025. CUDA Programming Guide. https://docs.nvidia.com/
cuda/cuda-c-programming-guide. (2025).
[46] NVIDIA. 2025. NVIDIA GB200 NVL72. https://www.nvidia.com/en-
us/data-center/gb200-nvl72/. (2025).
[47] Zaifeng Pan, Ajjkumar Patel, Zhengding Hu, Yipeng Shen, Yue Guan,
Wan-Lu Li, Lianhui Qin, Yida Wang, and Yufei Ding. 2025. KVFlow:
Efficient Prefix Caching for Accelerating LLM-Based Multi-Agent
Workflows. arXiv (2025).
[48] Pratyush Patel, Esha Choukse, Chaojie Zhang, Aashaka Shah, Íñigo
Goiri, Saeed Maleki, and Ricardo Bianchini. 2024. Splitwise: Efficient
generative llm inference using phase splitting. In ACM/IEEE ISCA.
[49] Yifan Qiao, Shu Anzai, Shan Yu, Haoran Ma, Yang Wang, Miryung
Kim, and Harry Xu. 2024. Conserve: Harvesting gpus for low-latency
and high-throughput large language model serving. arXiv (2024).
[50] Ruoyu Qin, Zheming Li, Weiran He, Jialei Cui, Feng Ren, Mingxing
Zhang, Yongwei Wu, Weimin Zheng, and Xinran Xu. 2025. Moon-
cake: Trading more storage for less computation—a KVCache-centric
architecture for serving LLM chatbot. In USENIX FAST.
[51] Chaoyi Ruan, Yinhe Chen, Dongqi Tian, Yandong Shi, Yongji Wu, Jialin
Li, and Cheng Li. 2025. DynaServe: Unified and Elastic Execution for
Dynamic Disaggregated LLM Serving. arXiv (2025).
[52] Xiaoxiang Shi, Colin Cai, Junjia Du, Zhanda Zhu, Xingda Wei, and
Zhihao Jia. 2025. Nexus: Taming Throughput-Latency Tradeoff in
LLM Serving via Efficient GPU Sharing. arXiv (2025).
[53] Chenchen Shou, Guyue Liu, Hao Nie, Huaiyu Meng, Yu Zhou, Yimin
Jiang, Wenqing Lv, Yelong Xu, Yuanwei Lu, Zhang Chen, Yanbo Yu,
Yichen Shen, Yibo Zhu, and Daxin Jiang. 2025. InfiniteHBD: Build-
ing Datacenter-Scale High-Bandwidth Domain for LLM with Optical
Circuit Switching Transceivers. In ACM SIGCOMM.
[54] Vasudev Shyam, Jonathan Pilault, Emily Shepperd, Quentin Anthony,
and Beren Millidge. 2024. Tree attention: Topology-aware decoding
for long-context attention on gpu clusters. arXiv (2024).
[55] Mingcong Song, Xinru Tang, Fengfan Hou, Jing Li, Wei Wei, Yipeng
Ma, Runqiu Xiao, Hongjie Si, Dingcheng Jiang, Shouyi Yin, Yang Hu,
and Guoping Long. 2024. Tackling the dynamicity in a production llm
serving system with sota optimizations via hybrid prefill/decode/verify
scheduling on efficient meta-kernels. arXiv (2024).
[56] Vikranth Srivatsa, Zijian He, Reyna Abhyankar, Dongming Li, and
Yiying Zhang. 2025. Preble: Efficient distributed prompt scheduling
for llm serving. International Conference on Learning Representations
(ICLR) (2025).
[57] The AIBrix Team, Jiaxin Shan, Varun Gupta, Le Xu, Haiyang Shi,
Jingyuan Zhang, Ning Wang, Linhui Xu, Rong Kang, Tongping Liu,
Yifei Zhang, Yiqing Zhu, Shuowei Jin, Gangmuk Lim, Binbin Chen,
Zuzhi Chen, Xiao Liu, Xin Chen, Kante Yin, Chak-Pong Chung,
Chenyu Jiang, Yicheng Lu, Jianjun Chen, Caixue Lin, Wu Xiang, Rui
Shi, and Liguang Xie. 2025. AIBrix: Towards Scalable, Cost-Effective
Large Language Model Inference Infrastructure. arXiv (2025).
[58] Hugo Touvron, Louis Martin, Kevin Stone, Peter Albert, Amjad Alma-
hairi, Yasmine Babaei, Nikolay Bashlykov, Soumya Batra, Prajjwal
Bhargava, Shruti Bhosale, Dan Bikel, Lukas Blecher, Cristian Canton
Ferrer, Moya Chen, Guillem Cucurull, David Esiobu, Jude Fernandes,
Jeremy Fu, Wenyin Fu, Brian Fuller, Cynthia Gao, Vedanuj Goswami,
Naman Goyal, Anthony Hartshorn, Saghar Hosseini, Rui Hou, Hakan
Inan, Marcin Kardas, Viktor Kerkez, Madian Khabsa, Isabel Kloumann,
Artem Korenev, Punit Singh Koura, Marie-Anne Lachaux, Thibaut
Lavril, Jenya Lee, Diana Liskovich, Yinghai Lu, Yuning Mao, Xavier
Martinet, Todor Mihaylov, Pushkar Mishra, Igor Molybog, Yixin Nie,
Andrew Poulton, Jeremy Reizenstein, Rashi Rungta, Kalyan Saladi,
Alan Schelten, Ruan Silva, Eric Michael Smith, Ranjan Subramanian,
Xiaoqing Ellen Tan, Binh Tang, Ross Taylor, Adina Williams, Jian Xi-
ang Kuan, Puxin Xu, Zheng Yan, Iliyan Zarov, Yuchen Zhang, Angela
Fan, Melanie Kambadur, Sharan Narang, Aurelien Rodriguez, Robert
Stojnic, Sergey Edunov, and Thomas Scialom. 2023. Llama 2: Open
Foundation and Fine-Tuned Chat Models. (2023).
[59] Bin Wang, Bojun Wang, Changyi Wan, Guanzhe Huang, Hanpeng Hu,
Haonan Jia, Hao Nie, Mingliang Li, Nuo Chen, Siyu Chen, Song Yuan,
Wuxun Xie, Xiaoniu Song, Xing Chen, Xingping Yang, Xuelin Zhang,
Yanbo Yu, Yaoyu Wang, Yibo Zhu, Yimin Jiang, Yu Zhou, Yuanwei
Lu, Houyi Li, Jingcheng Hu, Ka Man Lo, Ailin Huang, Binxing Jiao,
Bo Li, Boyu Chen, Changxin Miao, Chang Lou, Chen Hu, Chen Xu,
Chenfeng Yu, Chengyuan Yao, Daokuan Lv, Dapeng Shi, Deshan Sun,
Ding Huang, Dingyuan Hu, Dongqing Pang, Enle Liu, Fajie Zhang,
Fanqi Wan, Gulin Yan, Han Zhang, Han Zhou, Hanghao Wu, Hangyu
Guo, Hanqi Chen, Hanshan Zhang, Hao Wu, Haocheng Zhang, Hao-
long Yan, Haoran Lv, Haoran Wei, Hebin Zhou, Heng Wang, Heng
Wang, Hongxin Li, Hongyu Zhou, Hongyuan Wang, Huiyong Guo, Jia
Wang, Jiahao Gong, Jialing Xie, Jian Zhou, Jianjian Sun, Jiaoren Wu,
Jiaran Zhang, Jiayu Liu, Jie Cheng, Jie Luo, Jie Yan, Jie Yang, Jieyi Hou,
Jinguang Zhang, Jinlan Cao, Jisheng Yin, Junfeng Liu, Junhao Huang,
Junzhe Lin, Kaijun Tan, Kaixiang Li, Kang An, Kangheng Lin, Kenkun
Liu, Lei Yang, Liang Zhao, Liangyu Chen, Lieyu Shi, Liguo Tan, Lin
Lin, Lin Zhang, Lina Chen, Liwen Huang, Liying Shi, Longlong Gu,
Mei Chen, Mengqiang Ren, Ming Li, Mingzhe Chen, Na Wang, Nan
Wu, Qi Han, Qian Zhao, Qiang Zhang, Qianni Liu, Qiaohui Chen,
Qiling Wu, Qinglin He, Qinyuan Tan, Qiufeng Wang, Qiuping Wu,
Qiuyan Liang, Quan Sun, Rui Li, Ruihang Miao, Ruosi Wan, Ruyan
Guo, Shangwu Zhong, Shaoliang Pang, Shengjie Fan, Shijie Shang,
Shilei Jiang, Shiliang Yang, Shiming Hao, Shuli Gao, Siming Huang,
Siqi Liu, Tiancheng Cao, Tianhao Cheng, Tianhao Peng, Wang You,
Wei Ji, Wen Sun, Wenjin Deng, Wenqing He, Wenzhen Zheng, Xi
Chen, Xiangwen Kong, Xianzhen Luo, Xiaobo Yang, Xiaojia Liu, Xi-
aoxiao Ren, Xin Han, Xin Li, Xin Wu, Xu Zhao, Yanan Wei, Yang Li,
Yangguang Li, Yangshijie Xu, Yanming Xu, Yaqiang Shi, Yeqing Shen,
Yi Yang, Yifei Yang, Yifeng Gong, Yihan Chen, Yijing Yang, Yinmin
Zhang, Yizhuang Zhou, Yuanhao Ding, Yuantao Fan, Yuanzhen Yang,
Yuchu Luo, Yue Peng, Yufan Lu, Yuhang Deng, Yuhe Yin, Yujie Liu,
Yukun Chen, Yuling Zhao, Yun Mou, Yunlong Li, Yunzhou Ju, Yusheng
Li, Yuxiang Yang, Yuxiang Zhang, Yuyang Chen, Zejia Weng, Zhe Xie,
Zheng Ge, Zheng Gong, Zhenyi Lu, Zhewei Huang, Zhichao Chang,
Zhiguo Huang, Zhirui Wang, Zidong Yang, Zili Wang, Ziqi Wang,
Zixin Zhang, Binxing Jiao, Daxin Jiang, Heung-Yeung Shum, and Xi-
angyu Zhang. 2025. Step-3 is Large yet Affordable: Model-system
Co-design for Cost-effective Decoding. (2025).
[60] Chao Wang, Pengfei Zuo, Zhangyu Chen, Yunkai Liang, Zhou Yu,
and Ming-Chang Yang. 2025. Prefill-Decode Aggregation or Disaggre-
gation? Unifying Both for Goodput-Optimized LLM Serving. arXiv
(2025).
[61] Jiahao Wang, Jinbo Han, Xingda Wei, Sijie Shen, Dingyan Zhang,
Chenguang Fang, Rong Chen, Wenyuan Yu, and Haibo Chen. 2025.
13


---

KVCache Cache in the Wild: Characterizing and Optimizing KVCache
Cache at a Large Cloud Provider. USENIX ATC (2025).
[62] Bingyang Wu, Shengyu Liu, Yinmin Zhong, Peng Sun, Xuanzhe Liu,
and Xin Jin. 2024. Loongserve: Efficiently serving long-context large
language models with elastic sequence parallelism. In ACM SOSP.
[63] Ao Xiao, Bangzheng He, Baoquan Zhang, Baoxing Huai, Bingji Wang,
Bo Wang, Bo Xu, Boyi Hou, Chan Yang, Changhong Liu, Cheng Cui,
Chenyu Zhu, Cong Feng, Daohui Wang, Dayun Lin, Duo Zhao, Feng-
shao Zou, Fu Wang, Gangqiang Zhang, Gengyuan Dan, Guanjie Chen,
Guodong Guan, Guodong Yang, Haifeng Li, Haipei Zhu, Haley Li, Hao
Feng, Hao Huang, Hao Xu, Hengrui Ma, Hengtao Fan, Hui Liu, Jia Li,
Jiang Liu, Jiang Xu, Jie Meng, Jinhan Xin, Junhao Hu, Juwei Chen, Lan
Yu, Lanxin Miao, Liang Liu, Linan Jing, Lu Zhou, Meina Han, Mingkun
Deng, Mingyu Deng, Naitian Deng, Nizhong Lin, Peihan Zhao, Peng
Pan, Pengfei Shen, Ping Li, Qi Zhang, Qian Wang, Qin ZhC Qingrong
Xia, Qingyi Zhang, Qunchao Fu, Ren Guo, Ruimin Gao, Shaochun Li,
Sheng Long, Shentian Li, Shining Wan, Shuai Shen, Shuangfu Zeng,
Shuming Jing, Siqi Yang, Song Zhang, Tao Xu, Tianlin Du, Ting Chen,
Wanxu Wu, Wei Jiang, Weinan Tong, Weiwei Chen, Wen Peng, Wenli
Zhou, Wenquan Yang, Wenxin Liang, Xiang Liu, Xiaoli Zhou, Xin Jin,
Xinyu Duan, Xu Li, Xu Zhang, Xusheng Chen, Yalong Shan, Yang
Gan, Yao Lu, Yi Deng, Yi Zheng, Ying Xiong, Yingfei Zheng, Yiyun
Zheng, Yizhou Shan, Yong Gao, Yong Zhang, Yongqiang Yang, Yuanjin
Gong, Yue Yu, Yuetao Chen, Yukun Zhu, Yulong He, Yusu Zhao, Yuyan
Wu, Zenan Zhang, Zhaojin Zhuo, Zhaoyang Ji, Zhefeng Wang, Zheng
Wang, Zhenan Fan, Zhenhua Yang, Zhenli Sheng, Zhibin Yu, Zhigang
Ji, Zhihao Ren, Zhipeng Bian, Zhixia Liu, Zhiyu Dong, Zhonghua Li,
Zhou Yu, Zhuoming Shen, Zhuwei Peng, Zi Ye, Zihao Xiang, Zimin Fu,
and Zixuan Zhang. 2025. xDeepServe: Model-as-a-Service on Huawei
CloudMatrix384. arXiv (2025).
[64] Yi Xu, Ziming Mao, Xiangxi Mo, Shu Liu, and Ion Stoica. 2024. Pie:
Pooling cpu memory for llm inference. arXiv (2024).
[65] Amy Yang, Jingyi Yang, Aya Ibrahim, Xinfeng Xie, Bangsheng Tang,
Grigory Sizov, Jeremy Reizenstein, Jongsoo Park, and Jianyu Huang.
2025. Context parallelism for scalable million-token inference. Con-
ference on Machine Learning and Systems (2025).
[66] Lu Ye, Ze Tao, Yong Huang, and Yang Li. 2024. ChunkAttention:
Efficient Self-Attention with Prefix-Aware KV Cache and Two-Phase
Partition. In Association for Computational Linguistics (ACL).
[67] Zihao Ye, Lequn Chen, Ruihang Lai, Wuwei Lin, Yineng Zhang,
Stephanie Wang, Tianqi Chen, Baris Kasikci, Vinod Grover, Arvind
Krishnamurthy, and Luis Ceze. 2025. Flashinfer: Efficient and cus-
tomizable attention engine for llm inference serving. Conference on
Machine Learning and Systems (2025).
[68] Gyeong-In Yu, Joo Seong Jeong, Geon-Woo Kim, Soojeong Kim, and
Byung-Gon Chun. 2022. Orca: A Distributed Serving System for
Transformer-Based Generative Models. In USENIX OSDI.
[69] Lingfan Yu, Jinkun Lin, and Jinyang Li. 2025. Stateful large language
model serving with pensieve. In EuroSys.
[70] Mingshu Zhai, Jiaao He, Zixuan Ma, Zan Zong, Runqing Zhang, and
Jidong Zhai. 2023. SmartMoE: Efficiently Training Sparsely-Activated
Models through Combining Offline and Online Parallelization. In
USENIX ATC.
[71] Chenggang Zhao, Chengqi Deng, Chong Ruan, Damai Dai, Huazuo
Gao, Jiashi Li, Liyue Zhang, Panpan Huang, Shangyan Zhou, Shirong
Ma, Wenfeng Liang, Ying He, Yuqing Wang, Yuxuan Liu, and Y.X. Wei.
2025. Insights into deepseek-v3: Scaling challenges and reflections on
hardware for ai architectures. In ACM/IEEE ISCA.
[72] Yilong Zhao, Shuo Yang, Kan Zhu, Lianmin Zheng, Baris Kasikci, Yang
Zhou, Jiarong Xing, and Ion Stoica. 2024. Blendserve: Optimizing
offline inference for auto-regressive large models with resource-aware
batching. arXiv (2024).
[73] Lianmin Zheng, Liangsheng Yin, Zhiqiang Xie, Chuyue Sun, Jeff
Huang, Cody Hao Yu, Shiyi Cao, Christos Kozyrakis, Ion Stoica,
Joseph E. Gonzalez, Clark Barrett, and Ying Sheng. 2024. SGLang:
Efficient Execution of Structured Language Model Programs. In Neural
Information Processing Systems.
[74] Yinmin Zhong, Shengyu Liu, Junda Chen, Jianbo Hu, Yibo Zhu, Xu-
anzhe Liu, Xin Jin, and Hao Zhang. 2024. DistServe: Disaggregating
prefill and decoding for goodput-optimized large language model
serving. In USENIX OSDI.
[75] Ruidong Zhu, Ziheng Jiang, Chao Jin, Peng Wu, Cesar A. Stuardo,
Dongyang Wang, Xinlei Zhang, Huaping Zhou, Haoran Wei, Yang
Cheng, Jianzhe Xiao, Xinyi Zhang, Lingjun Liu, Haibin Lin, Li-Wen
Chang, Jianxi Ye, Xiao Yu, Xuanzhe Liu, Xin Jin, and Xin Liu. 2025.
MegaScale-Infer: Serving Mixture-of-Experts at Scale with Disaggre-
gated Expert Parallelism. ACM SIGCOMM (2025).
[76] Pengfei Zuo, Huimin Lin, Junbo Deng, Nan Zou, Xingkun Yang, Yingyu
Diao, Weifeng Gao, Ke Xu, Zhangyu Chen, Shirui Lu, Zhao Qiu,
Peiyang Li, Xianyu Chang, Zhengzhong Yu, Fangzheng Miao, Jia
Zheng, Ying Li, Yuan Feng, Bei Wang, Zaijian Zong, Mosong Zhou,
Wenli Zhou, Houjiang Chen, Xingyu Liao, Yipeng Li, Wenxiao Zhang,
Ping Zhu, Yinggang Wang, Chuanjie Xiao, Depeng Liang, Dong Cao,
Juncheng Liu, Yongqiang Yang, Xiaolong Bai, Yi Li, Huaguo Xie, Hu-
atao Wu, Zhibin Yu, Lv Chen, Hu Liu, Yujun Ding, Haipei Zhu, Jing
Xia, Yi Xiong, Zhou Yu, and Heng Liao. 2025. Serving Large Language
Models on Huawei CloudMatrix384. arXiv (2025).
14
