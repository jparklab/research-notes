---
title: EVALUATING KUBERNETES PERFORMANCE FOR GENAI
id: evaluating-kubernetes-performance-for-genai
tags:
- onprem-llm-inference-stacks-859099
- gpu-sharing
- provenance-vendor-engineering-paper
- red-hat-openshift
created: '2026-10-23T17:21:36.285163Z'
updated: '2026-10-23T17:25:04.601337Z'
source: https://arxiv.org/pdf/2602.04900
source_domain: arxiv.org
fetched_at: '2026-10-23T17:21:36.284882Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: institutional
content_type: paper
deprecated: false
summary: 'Red Hat industry paper (Feb 2026, arXiv 2602.04900v3) measuring Kueue, Dynamic
  Accelerator Slicer (MIG-based, OpenShift 4.19.16, GPU Operator v25.3.4) and llm-d/GAIE
  on ONE AWS p4d.24xlarge (8x A100) ROSA node - cloud testbed, not on-prem, but stack
  is the on-prem OpenShift one. Whisper medium on 1g.5gb and large on 3g.20gb MIG
  slices: concurrency 8->25 jobs, mean JCT 28->18 min (-36%), P95 51->36, but per-job
  slowdown on a slice (mean JCT 14->17 min at equal concurrency) and mean GPU util
  44%->38% due to slice padding; CPU util 10->37%. Kueue preemption+priority cut makespan
  60.5->51.1 min (~15%); admission latency <25 ms P99. llm-d precise prefix-cache
  routing over 8 vLLM replicas (Qwen3-8B, A100 40GB, 143,360-token KV each): TTFT
  ~500 ms -> ~80 ms mean, P99 up to 20x better; InferencePool adds 3-11 ms overhead.
  DAS/Kueue integration still in progress at writing.'
raw_file: raw/evaluating-kubernetes-performance-for-genai.pdf
doi: arXiv:2602.04900
citation_count: 2
venue: International Conference on Performance Engineering
is_retracted: false
---

EVALUATING KUBERNETES PERFORMANCE FOR GENAI
INFERENCE: FROM AUTOMATIC SPEECH RECOGNITION TO LLM
SUMMARIZATION
Sai Sindhur Malleni∗
Red Hat
Boston, MA, US
smalleni@redhat.com
Raúl Sevilla
Red Hat
Madrid, Spain
rsevilla@redhat.com
Aleksei Vasilevskii
Red Hat
Munich, Germany
avasilev@redhat.com
José Castillo Lema
Red Hat
Madrid, Spain
jlema@redhat.com
André Bauer
Illinois Institute of Technology
Chicago, IL, US
andre.bauer@iit.edu
February 12, 2026
ABSTRACT
As Generative AI (GenAI), particularly inference, rapidly emerges as a dominant workload category,
the Kubernetes ecosystem is proactively evolving to natively support its unique demands. This
industry paper demonstrates how emerging Kubernetes-native projects can be combined to deliver
the benefits of container orchestration, such as scalability and resource efficiency, to complex AI
workflows. We implement and evaluate an illustrative, multi-stage use case consisting of automatic
speech recognition and summarization. First, we address batch inference by using Kueue to manage
jobs that transcribe audio files with Whisper models and Dynamic Accelerator Slicer (DAS) to
increase parallel job execution. Second, we address a discrete online inference scenario by feeding
the transcripts to a Large Language Model for summarization hosted using llm-d, a novel solution
utilizing the recent developments around the Kubernetes Gateway API Inference Extension (GAIE) for
optimized routing of inference requests. Our findings illustrate that these complementary components
(Kueue, DAS, and GAIE) form a cohesive, high-performance platform, proving Kubernetes’ capability
to serve as a unified foundation for demanding GenAI workloads: Kueue reduced total makespan by
up to 15%; DAS shortened mean job completion time by 36%; and GAIE working in conjunction
with llm-d improved tail Time to First Token latency by up to 90% even under high loads.
1
Introduction
The global shift toward Artificial Intelligence (AI) has fundamentally changed the landscape of cloud computing.
Modern AI workloads are intricate, spanning complex training pipelines that build sophisticated models and high-
throughput real-time inference serving that deploys them. While the training phase requires substantial, often bursty,
computational resources, the industry’s focus has increasingly shifted toward deployment. This is especially true for
Generative AI (GenAI), which was exhibiting an 18.7% increase in private investment from 2023 alone, according
to the Stanford HAI AI Index Report [1]. Critically, the primary infrastructure challenge today lies in serving this
massive demand; as noted by ACM Queue, “AI: It’s All About Inference Now” [2]. Inference workloads require
dynamic access to heterogeneous and often expensive resources, particularly GPUs and specialized accelerators [3].
To successfully manage the entire lifecycle of these multi-stage AI applications from provisioning data to model
∗This work was performed while the author was at Red Hat. The author is now with NVIDIA.
arXiv:2602.04900v3  [cs.ET]  11 Feb 2026


---

A PREPRINT - FEBRUARY 12, 2026
deployment, a sophisticated and extensible orchestration layer abstracting the underlying hardware and providing
declarative management is required.
This is where orchestration systems like Kubernetes become essential. Initially rising to prominence through the mi-
croservices revolution, Kubernetes has established itself as the de facto standard for container orchestration, automating
the deployment, scaling, and management of containerized applications [4]. According to the Cloud Native Computing
Foundation’s most recent annual survey, 91% of organizations use containers for production [5]. Given its ability to
efficiently scale to meet varying computational demands and its rich collection of AI frameworks and tools, Kubernetes
also has emerged as a key enabler for customers to run large scale AI. However, as AI models continue to evolve in size
and complexity, they require advanced capabilities beyond Kubernetes’ traditional boundaries. Traditional scheduling
and resource management APIs were not designed to handle fine-grained GPU resource slicing, complex scheduling
requirements for distributed training, or the specific traffic patterns of model serving endpoints.
This industry paper demonstrates how emerging Kubernetes-native projects can be combined to deliver the full benefits
of container orchestration, such as scalability and resource efficiency, to complex AI inference workflows. We argue that
three complementary projects explicitly address the distinct needs of such workloads by focusing on key stages of the
deployment and serving pipeline: Kueue [6] for efficient batch scheduling, Dynamic Accelerator Slicer [7] (DAS) for
resource optimization, and the newly emerged Gateway API Inference Extension [8] (GAIE) for distributed inference
performance on Kubernetes.
To showcase their synergy, we implement and evaluate an illustrative, multi-stage use case processing a dataset of
corporate earnings calls [9], a workflow that reflects common industry usage patterns. First, we address batch inference
by using Kueue to manage jobs that transcribe audio files with Whisper models, a core component of Automatic Speech
Recognition (ASR). ASR has emerged as a critical enabling technology, with the ASR software market valued at USD
5.49 billion in 2024 [10]. We demonstrate significant efficiency gains for these workloads by integrating DAS, which
leverages NVIDIA MIG (Multi-Instance GPU) slicing to dramatically increase parallel job execution on the same
hardware. Second, we address a discrete online inference scenario by feeding the generated transcripts to a Large
Language Model (LLM) for summarization. This service is hosted using llm-d [11], a novel solution utilizing the
recent developments around the GAIE for optimized routing of inference requests. We showcase responsiveness gains
by applying its vLLM-Optimized Inference Scheduler, which employs a “Precise Prefix-Cache Aware Scheduling”
strategy.
Our findings illustrate that these complementary components (Kueue, DAS, and GAIE) complement core Kubernetes
to form a cohesive, high-performance platform, proving Kubernetes’ capability to serve as a unified foundation for
demanding GenAI inference workloads. Specifically, Kueue reduced the total makespan by up to 15%, DAS decreased
the mean job completion time by 36%, and GAIE lowered inference latency by as much as 6 seconds while improving
the time to first token by more than sixfold. It is important to note that while model accuracy is a crucial metric from an
AI perspective, the scope of this industry paper is focused exclusively on the system-level performance, efficiency, and
operational overhead of the underlying orchestration infrastructure.
The remainder of this industry paper is structured as follows: Section 2 introduces Kubernetes primitives for AI
workloads and related work. Section 3 outlines the measurement setup, deployed inference AI solutions, and performed
test cases. Section 4 investigates the results of the analyzed solutions. Finally, Section 5 summarizes the paper and
Section 6 discusses future work directions.
2
Background
This chapter establishes the necessary background on AI workloads and Kubernetes and discusses related work.
2.1
Kubernetes Primitives for AI Workloads
Modern AI training and inference pipelines build on several Kubernetes primitives that expose accelerators, respect
hardware topology and NUMA affinities, and enforce resource limits. The device plugin framework allows vendors
to advertise GPUs, FPGAs, DPUs and other accelerators to the scheduler so that pods can request these resources
in a portable way [12]. The Topology Manager coordinates CPU and device placement and supports policies up to
single-NUMA-node to improve locality and latency for latency-sensitive or accelerator-heavy pods [13]. Dynamic
Resource Allocation (DRA) generalizes persistent-volume claims to generic devices (e.g., GPUs or DPUs), introducing
device classes and claims so operators can safely share accelerators; production support in cloud distributions is
emerging [14]. Finally, resource quotas, priority classes and related limit mechanisms remain essential for fair sharing
and multi-tenancy in AI clusters [15].
2


---

A PREPRINT - FEBRUARY 12, 2026
On the device side, NVIDIA MIG partitions a single GPU into multiple isolated slices with dedicated memory
and compute engines, allowing heterogeneous and multi-tenant inference services to share an expensive accelerator
efficiently [16]. When strict isolation is unnecessary, GPU time-slicing multiplexes workloads over the entire device;
both modes are available through the NVIDIA GPU Operator and device-plugin stacks [17]. These primitives underpin
the singular inference solutions evaluated in this paper.
2.2
AI Workload Orchestration on Kubernetes
Integral to orchestrating AI workloads on Kubernetes are three capabilities: job queuing and scheduling, accelerator
slicing, and multi-node inference orchestration for distributed inference.
2.2.1
Scheduling (Queueing and Admission).
The default Kubernetes scheduler is insufficient for complex AI workloads, as it lacks job-level admission control
and gang scheduling. To address this, emerging systems extend the platform. These range from comprehensive
commercial platforms like NVIDIA Run.ai, which provides a full GPU orchestration layer with queueing, preemption,
and hierarchical quota management [18], to open-source, community-driven schedulers.
2.2.2
Accelerator Slicing.
Accelerator sharing requires both a low-level partitioning technology and a high-level orchestration API. The primary
partitioning methods are NVIDIA MIG, for hardware-isolated slices, and time-slicing, for software-based sharing [16].
The traditional Kubernetes device plugin model, however, only supports static, pre-configured slices. To address this,
operator-based solutions like the Dynamic Accelerator Slicer (DAS) were developed to automate “just-in-time” slicing.
DAS dynamically creates and destroys MIG partitions to match workload demands, improving utilization [7]. This
provides a bridge to the official, upstream API, DRA, which is maturing to provide a standardized, abstract framework
for requesting and sharing such slices natively in Kubernetes [14].
2.2.3
Distributed Inference.
Foundation models increasingly exceed a single node’s memory and accelerator budget, pushing inference toward
distributed execution. The Gateway API Inference Extension (GAIE) provides a new layer of intelligence to Kubernetes
networking, tailored for generative AI workloads [8]. Built on the standard Gateway API, it introduces domain-specific
constructs to dynamically route requests based on live model and accelerator metrics (e.g., queue length, prefix cache
hits, Low-Rank Adaptation (LoRA) [19] adapter availability). This model-aware routing enables higher GPU utilization
and lower tail latency.
This paper focuses on the optimizations and performance gains provided by this extension and its inference schedulers.
To conduct this analysis, we utilize vLLM [20] as the high-performance model server backend. While our distributed
inference solution is deployed using the llm-d framework [11], this paper’s evaluation is focused specifically on the
optimizations provided by the GAIE itself, which is detailed in Section 3.1.3.
2.3
Related Work
The rise of Kubernetes as the foundational platform for managing AI workloads has spurred extensive research focused
on enhancing its capabilities in scheduling, resource management, and model serving.
2.3.1
Scheduling and Orchestration
Research on scheduling in Kubernetes aims to optimize performance and resource utilization for AI/ML tasks. Surveys
by Rejiba et al. [21] and Ye et al. [22] detail common challenges and solutions, with the latter focusing specifically on
deep learning training and inference. To improve scheduling effectiveness, various learning-based frameworks have
been proposed: PAX [23] introduces a machine learning-driven scheduler for performance and energy efficiency, and
KaiS [24] applies a similar approach in edge-cloud environments to boost long-term performance. For distributed deep
learning, systems like ElasticDL [25] automate deployment and elastic scaling, while MuxFlow [26] refines allocation
via dynamic multiprocessor allocation and matching-based mechanisms.
2.3.2
Resource Slicing and Sharing
Another line of work addresses the efficient slicing and sharing of hardware accelerators, particularly GPUs. Tools
like nvshare [27] enable practical GPU sharing without memory-size constraints, and DISC [28] dynamically adjusts
3


---

A PREPRINT - FEBRUARY 12, 2026
GPU time slices for hyperparameter tuning jobs. For more advanced management, Jormungandr [29] optimizes the
allocation of NVIDIA’s MIG based on user-defined utility functions, and KIS-S [30] provides a GPU-aware autoscaler
tailored for inference workloads. Beyond GPUs, work also extends to container partitioning for distributed ML [31]
and integrating other accelerators, such as extending KServe for FPGA-based workloads [32].
2.3.3
Distributed Inference
Distributed inference has emerged as a pivotal approach for deploying large-scale models across multiple nodes.
Several systems address the challenges of partitioning and coordinating inference workloads. Alpa [33] introduces
compiler-based automated model parallelism for distributed serving. PetS [34] and DistServe [35] specifically target
disaggregated prefill and decode stages in LLM inference to enhance resource utilization and throughput. Xu et al. [36]
comprehensively investigate how Kubernetes can optimize LLM serving performance. ServerlessLLM [37] proposes
elastic scaling for distributed LLM workloads. Comparative evaluations [38] assess orchestration frameworks like
KServe for multi-node deployments.
2.3.4
Delimitation
Prior work has largely focused on novel scheduling algorithms or hardware specific resource sharing techniques. In
contrast, relatively few studies present an end to end analysis of the operational overheads, performance, and scaling
behavior of a full stack, state of the art AI workload running on Kubernetes. This work addresses that gap by evaluating
a complete inference pipeline rather than isolated system components or microbenchmarks.
We build and evaluate an end to end pipeline consisting of automatic speech recognition followed by text summarization.
For the ASR stage, we use OpenAI’s Whisper [39], which is available in multiple model sizes and serves as a
representative real world ASR workload. ASR systems are increasingly important in modern computing, supporting
applications such as voice assistants, accessibility tools, and real time transcription. These workloads are both
computationally intensive and latency sensitive, making them well suited for studying system level performance and
scalability [40, 41].
The pipeline is evaluated under two different inference regimes. First, we study batch inference, where Kueue is
used to manage large volumes of transcription jobs and Dynamic Accelerator Slicer (DAS) is used to improve GPU
utilization and increase parallel job execution. Second, we examine a real-time inference scenario in which the generated
transcripts are passed to a large language model for summarization. This stage is deployed using llm-d, a Kubernetes
native inference serving solution that builds on recent developments in the Kubernetes Gateway API Inference Extension
(GAIE) to enable efficient request routing and scalable online inference.
This work deliberately does not propose new scheduling algorithms or model architectures. Instead, it focuses on
characterizing the system level behavior of modern Kubernetes native components when running realistic AI workloads,
and on highlighting the performance, scalability, and operational trade offs that arise in production like environments.
3
Measurement Setup
The complete utilized technology stack is depicted in Figure 1 and described in the following sections.
3.1
Infrastructure Stack
To demonstrate how emerging Kubernetes-native projects can be combined to maximize the benefits of container
orchestration, we present an illustrative, multi-stage use case that processes a dataset of corporate earnings calls [9].
In this section, we elaborate on the solutions employed for scheduling, accelerator slicing, and multi-node inference.
Table 1 offers a concise overview of each solution.
3.1.1
Scheduling (Queueing & Admission)
Kueue [6] is an open-source project under the Kubernetes Scheduling Special Interest Group that extends native
Kubernetes scheduling to support advanced batch and job management. Unlike the standard Kubernetes Job controller,
which treats each job independently, Kueue introduces a hierarchical queuing model composed of LocalQueues and
ClusterQueues. This abstraction enables cluster operators to declaratively define resource boundaries, priority classes,
and admission controls for batch workloads, thereby improving fairness and utilization across shared clusters.
A LocalQueue is namespace-scoped and serves as the entry point for job submissions within that namespace. Each
LocalQueue is bound to one or more ClusterQueues, which operate at the cluster scope and represent the global
4


---

A PREPRINT - FEBRUARY 12, 2026
Whisper
Automatic Speech
Recognition/
Transcription
vLLM
LLM Inference/
Summarization
OpenShift
AWS EC2
GPU Drivers & Runtime + GPU Partitioning
llm-d
Distributed
Inference
of LLMs
GPU
Slicing
DAS
Kueue
Batch/Queue
Scheduling
Figure 1: Overview of the technology stack.
scheduling domains responsible for admitting jobs based on cluster-wide policies and resource availability. This
decoupling between job submission via LocalQueues and resource management via ClusterQueues allows individual
teams to operate independently while maintaining centralized control over compute resources.
Table 1: Compact comparison of Kueue, DAS, and llm-d for AI workloads
Aspect
Kueue
DAS
llm-d and GAIE
Purpose
Job queuing and batching
GPU/accelerator slicing
Distributed LLM inference
Resource Mgmt
Workload admission based on quotas
Dynamic GPU slices per pod
Pools of model servers
Scheduler Link
Uses admission hooks
Enforces slices via scheduler
Routes requests to optimal inference endpoints
Workloads
Batch/AI jobs needing fairness
GPU-as-a-Service + Data Science
LLM prefill/decode tasks
Isolation
Fair queuing, priorities
Strong GPU isolation
Through inference pools
Perf. Focus
Throughput + fairness
GPU utilization, low waste
Efficient parallel inference
Complexity
Light, native integration
Needs custom device plugins
Requires distributed inference layer
Extensibility
Generic; policy-extensible
AI-specific; NVIDIA MIG focus
Integrates with Kubernetes Gateway API
Within each ClusterQueue, Kueue introduces the concept of a ResourceFlavor to represent distinct types or configura-
tions of underlying resources. Each ResourceFlavor defines a specific capacity pool and ClusterQueues can advertise
multiple flavors simultaneously. When jobs are enqueued through a LocalQueue, Kueue maps them to the appropriate
LocalQueue and selects a ResourceFlavor that satisfies the job’s requirements, effectively bridging namespace-level
workloads with cluster-level resource scheduling.
This hierarchical mapping (LocalQueue →ClusterQueue →ResourceFlavor) forms the foundation of Kueue’s
scheduling architecture, enabling scalable, fair, and efficient sharing of heterogeneous compute resources. Figure 2
illustrates the key Kueue components and their interactions.
Kueue addresses three key challenges: job admission delay, resource fragmentation, and fairness in multi-tenant
scheduling. Its admission controller first evaluates whether a job can be admitted into its associated queue given current
resource usage and configured limits. Once admitted, the job waits until scheduling slots become available in the
ClusterQueue.
3.1.2
Accelerator slicing
In Kubernetes-based AI inference deployments, a central challenge lies in efficiently partitioning and sharing GPU
resources across workloads while maintaining performance isolation. To address this, OpenShift introduces the Dynamic
Accelerator Slicer (DAS) [7], which provides on-demand, just-in-time GPU slice allocation driven by actual workload
requirements.
Conventional GPU partitioning methods, such as static pre-slicing, often lead to resource fragmentation and suboptimal
utilization. Moreover, static allocation performed at node initialization can cause service disruptions when workloads
evolve and reconfiguration becomes necessary. In contrast, DAS dynamically provisions and releases GPU slices in
5


---

A PREPRINT - FEBRUARY 12, 2026
 Cohort
Cluster Queue 
Cluster Queue 
Local Queue 
Local Queue 
Local Queue 
Local Queue 
Cluster Queue 
Resource
Flavor
Resource
Flavor
Resource
Flavor
Kubernetes
Nodes
Instance-type: CPU
Kubernetes
Nodes
Instance-type: GPU
Kubernetes
Nodes
Instance-type: spot
Figure 2: Kueue working mechanism.
response to workload specifications, allocating resources only when a pod requests them and freeing them upon job
completion.
DAS addresses four key challenges in achieving efficient, multi-tenant GPU utilization: it provisions slices just-in-time
for new jobs, dynamically allocates and reclaims them as workloads change, coordinates with the NVIDIA GPU
Operator to expose precise device configurations, and binds scheduling with slice allocation to ensure pods receive
physical GPU slices only after successful provisioning. Together, these capabilities enable multiple inference workloads
to share a single GPU efficiently, increasing utilization, reducing idle costs, and enhancing operational flexibility in
shared AI-serving environments [42].
3.1.3
Distributed Inference: llm-d and Gateway API Inference Extension
llm-d [11] extends the Kubernetes ecosystem to efficiently orchestrate LLM inference workloads, addressing the
limitations of conventional stateless load balancing. In Kubernetes, services paired with deployments typically distribute
traffic evenly across identical replicas. This model is well-suited for microservices with short-lived, uniform requests,
but LLM inference requests differ fundamentally: they are stateful, vary widely in compute cost, and benefit greatly
from cache reuse and latency-aware routing [11].
To support these characteristics, the Gateway API Inference Extension (GAIE) [8] introduces first-class Kubernetes
abstractions for managing distributed inference. GAIE extends the standard Gateway API with specialized primitives
that allow operators to declaratively expose, route and monitor model-serving endpoints. Its design bridges service
networking and model serving, bringing inference-specific features such as model versioning, adaptive backend selection
and prefix-cache awareness into the Kubernetes control plane.
GAIE extends existing gateway concepts through two core components: (i) The InferenceGateway acts as the primary
entry point for inference traffic, augmenting a standard Gateway with capabilities for model registration, authentication,
versioning, and routing policies—enabling multiple model versions to be served under a unified logical endpoint.
(ii) The InferencePool manages groups of backend model-serving replicas (e.g., vLLM [20]) and integrates an infer-
ence scheduler, implemented in llm-d as the Endpoint Picker (EPP), which intelligently routes each request to the
most suitable backend, enabling cache-aware and latency-optimized inference scheduling within a Kubernetes-native
framework.
A key efficiency driver in this design is the Key-Value (KV) cache [43], an internal mechanism in transformer-based
LLMs that stores intermediate representations (keys and values) for previously processed tokens. The initial prefill
phase encodes the full prompt into this cache, which is a computationally intensive step. Subsequent decoding stages
reuse these cached representations, avoiding redundant computation and significantly reducing per-token latency.
In distributed environments, if similar prompts are routed to different replicas without cache awareness, each must
recompute the same prefix state, wasting GPU cycles and increasing latency.
6


---

A PREPRINT - FEBRUARY 12, 2026
The llm-d framework, through GAIE’s programmable scheduling layer, optimizes routing based on cache locality and
workload telemetry. By aligning inference request placement with active cache state, it reduces redundant computation,
improves GPU utilization, and enhances throughput predictability.
Table 2: Overview of the employed audio dataset, containing 16 files for medium model and 16 files for large model
jobs from the Earnings 22 dataset benchmark [9]. All values denote run time of the audio files to be transcribed.
Metric
Count
Mean [s]
SD [s]
Median [s]
Min [s]
Max [s]
All Jobs
32
3127.38
1217.90
3287.51
1258.34
7405.44
Large Model Jobs
16
3077.63
1584.01
3119.85
1258.34
7405.44
Medium Model Jobs
16
3177.13
742.37
3287.51
1682.72
4380.08
3.1.4
Inference Runtime
vLLM [20] is a high-performance and memory-efficient inference engine for LLMs. It is designed to address a key
bottleneck in LLM serving: the inefficient use of GPU memory due to static and coarse-grained allocation in existing
systems. vLLM introduces PagedAttention, a memory management mechanism inspired by virtual memory paging in
operating systems. PagedAttention enables fine-grained sharing of key-value (KV) caches across multiple inference
requests, allowing dynamic batching and efficient context reuse without redundant memory copies. This design
substantially increases GPU utilization and throughput while preserving low latency and model accuracy. Evaluations
showed that vLLM can achieve up to 3–5× higher throughput than baseline inference engines such as Hugging Face
Accelerate or DeepSpeed-Inference under comparable latency targets [20].
3.2
Testbed Description
All experiments were conducted on Red Hat OpenShift Service on AWS [44] (ROSA), a fully managed service that runs
Red Hat’s OpenShift enterprise Kubernetes platform with Platform-as-a-Service capabilities on Amazon Web Services.
The cluster configuration used the default node types and counts for the control plane and infrastructure: 3 control plane
nodes on m5.2xlarge instances and 3 infrastructure nodes on r5.xlarge instances. For the experiments, we added a
single GPU node utilizing a p4d.24xlarge instance deployed in the region us-west2, having 8 NVIDIA A100 GPUs.
The Whisper transcription jobs deployed on the GPU node are built on the Red Hat Universal Base Image 10 (UBI 10).
The cluster ran OpenShift version 4.19.16, which is based on Kubernetes 1.32. GPU management was handled by the
NVIDIA GPU Operator v25.3.4. The complete technology stack is described in Section 3.4 and depicted in Figure 1.
All the necessary files and configuration for replicating the test setup on any environment can be found on our GitHub
repo [45].
3.3
Test Cases & Workload
To showcase how the Kubernetes and its ecosystem of projects can be combined to improve performance, we designed
test cases for the multi-stage AI inference workflow. To run these test cases, we utilized the Earnings 22 dataset [9].
This dataset comprises a corpus of 125 financial earnings calls collected from various global companies. It serves as a
commonly used benchmark for automatic speech recognition (ASR) models in real-world scenarios. We selected a
subset of 32 files from this dataset: 16 files each to be transcribed by a job running the medium (769 M parameters)
Whisper model and 16 files each to be transcribed by a job running the large (1550 M parameters) Whisper model. We
selected two distinct models to mirror the industry trend of deploying models of varying sizes based on the use case,
and to demonstrate how different MIG slice configurations can be utilized effectively with DAS. For the same audio file,
the large Whisper model is expected to take longer to transcribe than the medium model, since inference time scales
approximately proportionally with model size and computational complexity. The dataset statistics are presented in
Table 2.
3.3.1
Scheduling
The first test case involves transcribing audio files from the dataset using Kueue for job queue management. To achieve
this, we had three different setups: (i) Not using Kueue (to baseline with default Kubernetes job scheduling), (ii) using
Kueue with BestEffortFIFO queueingStrategy and 1 ClusterQueue as outlined in Figure 3a, and (iii) using Kueue with
2 cluster queues and resource borrowing as outlined in Figure 3b. The last setup allows us to borrow unused quota
from the collective pool of the cohort to accommodate more jobs. From a technical standpoint, each file transcription is
deployed as a Kubernetes job that translates to a Kueue Workload running a Whisper model, utilizing the entire GPU.
7


---

A PREPRINT - FEBRUARY 12, 2026
Whisper-scale
Namespace
Whisper Jobs (32)
Kubernetes API Server
Namespace-scoped LocalQueue
LocalQueue-Large
LocalQueue-Medium
Job Controller
ClusterQueue
ResourceFlavor: GPU
Node (8 GPUs)
Kueue Admission
Control
admits within GPU limits
(a) Transcription with Whisper with two local queues and one
cluster queue.
Kueue
Admission
Control
LocalQueue-Large
Quota: 4 GPUs
LocalQueue-Medium
Quota: 4 GPUs
Whisper-scale
Namespace
Whisper Jobs (32)
Kubernetes API Server
Namespace-scoped LocalQueue
LocalQueue-Large
LocalQueue-Medium
Cohort:
Whisper-cohort
sharing
ResourceFlavor: GPU
Node (8 GPUs)
admits within
4-GPU quota
admits within 
4-GPU quota
Job Controller
(b) Transcription with Whisper with two local queues and two
cluster queues and GPU borrowing.
Figure 3: Kueue setup comparison for Whisper transcription experiments.
3.3.2
Accelerator Slicing
In the second test case, we employed DAS to dynamically allocate GPU resources and execute transcription tasks
using Kubernetes jobs that leverage Whisper models. Two configurations were evaluated: (i) without DAS, where
transcription jobs were distributed across eight full GPUs, and (ii) with DAS enabled, where GPUs were partitioned into
slices using the 1g.5gb instance profile for medium jobs and the 3g.20gb instance profile for large model jobs according
to the Whisper model resource requirements [46].
3.3.3
Distributed Inference
In the final stage of our workflow, we evaluate distributed inference performance for summarization tasks using the
open-weight and publicly released Qwen3-8B model [47], deployed on Kubernetes through the llm-d stack. Specifically,
we deploy eight replicas of vLLM, each serving an identical Qwen3-8B model instance. The Qwen LLM family is
developed by Alibaba, and we opted for this particular model due to its open-source nature and enterprise adoption. We
first compare three configurations to quantify the overhead of GAIE: (i) a standard Kubernetes ClusterIP service for
stateless load balancing, (ii) GAIE with only the InferenceGateway, and (iii) GAIE with both the InferenceGateway
and InferencePool enabled, including its inference-scheduler. Additionally, we assess routing strategies within GAIE,
contrasting a random scheduler with the precise-cache “well-lit path” [48] strategy that leverages KV-cache telemetry
to route requests toward replicas with matching prefix tokens. An overview of the setup is shown in Figure 4.
3.4
Tooling
For test orchestration, we selected kube-burner [49] as our primary workload orchestration and analysis framework.
Unlike generic load-testing utilities, kube-burner is purpose-built for Kubernetes and OpenShift environments, providing
a Kubernetes-native approach to performance and scalability evaluation. Its core strength lies in a declarative, YAML-
based configuration model that enables precise definition of complex object creation, modification, and deletion
scenarios. This declarative design is critical for experimental rigor, offering powerful and flexible control over workload.
It enables robust measurement capabilities, which elevate it from a simple stress tool to a comprehensive analysis
framework. Its core strength is the built-in instrumentation to scrape Prometheus endpoints on the cluster before
and after each test, capturing empirical data on system health and resource utilization. Beyond what is available in
Prometheus, it includes several custom measurements like jobLatency, which quantifies the total time from a job’s
creation to the completion of its pods, providing key performance indicators for our Kueue and DAS experiments.
8


---

A PREPRINT - FEBRUARY 12, 2026
For the final LLM powered transcript summarization test case, we used the open-source benchmarking framework
GuideLLM [50] to orchestrate the execution of the test against our OpenAI-compatible Kubernetes hosted inference
endpoint. By simulating real-world inference workloads, GuideLLM enables users to assess the performance, resource
requirements, and cost implications of deploying LLMs on various hardware configurations. We fed the transcript
data from the previous stage to GuideLLM, which used it as a custom dataset for generating prompts. The tool was
sending these prompts at various concurrency levels (i.e., number of synchronous streams in parallel), allowing us to
capture critical LLM inference performance metrics such as time-to-first-token (TTFT) and inter-token latency (ITL),
and ensuring benchmark reproducibility.
Kubernetes
Inference Gateway
(Envoy)
Shared Prefix Cache
vLLM engine 1
(QWEN-8B)
vLLM engine 8
(QWEN-8B)
...
Routes to selected Pods
Inference Scheduler
Selects optimal replica
based on state
Load, KV
Cache Report
Inference Pool
KV-Cache
Manager
Figure 4: Overview of the llm-d setup.
4
Evaluation
Table 3: Performance comparison of Kueue Configurations (96 jobs across 3 runs per configuration). All values are in
minutes. Makespan median computed from 3 run-level makespans comprising 32 jobs per run; other medians computed
from 96 jobs (32 jobs across 3 test runs) from each configuration.
Configuration
Makespan
Queue Time
Exec Time
Medium Jobs
Large Jobs
(Med, (Min–Max))
(Med / P95)
(Med)
Queue (Med)
Exec (Med)
Queue (Med)
Exec (Med)
Pure Jobs (Baseline)
60.5 (59.1–60.5)
0.0 / 0.0
28.4
0.0
23.6
0.0
35.3
Kueue BestEffortFIFO
62.5 (62.4–65.8)
16.7 / 39.5
9.4
15.9
6.5
17.3
17.8
+ Priority
54.7 (52.2–56.1)
19.5 / 44.5
9.5
39.1
6.7
10.2
17.1
+ Preemption
51.1 (49.2–52.7)
25.0 / 41.9
8.9
36.7
6.4
3.7
15.6
2 ClusterQueues + Borrowing
55.0 (54.2–60.0)
13.7 / 32.4
8.7
10.2
6.7
25.1
18.0
To validate the proposed multi-stage inference use case, we conducted a series of experiments assessing the contribution
of each component to the overall Generative AI (GenAI) workflow. The evaluation is structured to isolate and measure
the performance gains delivered by Kueue in batch scheduling, Dynamic Accelerator Slicer (DAS) in GPU resource
slicing and utilization, and the Gateway API Inference Extension (GAIE) in low-latency distributed inference serving.
Together, these results comprehensively illustrate how orchestrating Kubernetes-native primitives can efficiently support
both large-scale and latency-sensitive AI workloads.
4.1
Scheduling: Kueue
To evaluate the scheduling and queueing behavior of Whisper transcription workloads under Kueue, we conducted
experiments as described in Section 3.3.1 both with and without Kueue enabled. We further explored two distinct Kueue
configurations: (i) two LocalQueues feeding into a single ClusterQueue, and (ii) two LocalQueues each mapped to
separate ClusterQueues. In all configurations, the cluster contained a total of eight GPUs, all corresponding to the same
ResourceFlavor. The key distinction between the two setups lies in resource sharing: in the first configuration, all eight
9


---

A PREPRINT - FEBRUARY 12, 2026
GPUs are managed by a single ClusterQueue, whereas in the second, each ClusterQueue controls four GPUs, with one
allowed to borrow resources from the other when idle capacity is available. In this experimental setup, it is important to
clarify the usage of the terms medium and large. These terms refer exclusively to the Whisper model variants used for
transcription and do not denote the duration or size of the input audio files. Specifically, jobs labeled as large use the
Whisper large model, which offers higher transcription accuracy at the cost of increased computational complexity and
processing time. Consequently, job runtime is influenced by both the selected model variant and the input audio length;
for example, a transcription using a medium model on a longer audio file may take longer to complete than a large
model applied to a shorter audio clip.
For the first configuration, we evaluated three scheduling policies: (a) no priorities and no preemption, (b) higher
priority assigned to large jobs through a WorkloadPriorityClass but no preemption, and (c) higher priority assigned
to large jobs, with preemption enabled for higher-priority workloads. In total, five distinct experimental setups were
executed, each repeated three times. We used kube-burner to create one large Whisper job followed by one medium
Whisper job until 16 jobs of each kind were created for a total of 32 Kueue Workloads. A key performance metric in our
evaluation is the makespan, which represents the total elapsed time required for all 32 transcription jobs to complete.
The aggregated results across all runs are presented in Table 3.
Our experimental results reveal a nuanced trade-off between performance optimization and scheduling predictability
when comparing Kueue-managed workloads to native Kubernetes job scheduling. While the pure-jobs scenario achieved
a lower makespan (60.5 min) compared to Kueue’s BestEffortFIFO (62.5 min), this improvement came at the cost of
non-deterministic execution ordering. In both scenarios, the initial job execution was identical: the first eight jobs
(medium-0 through medium-3 and large-0 through large-3) initially consumed all available GPU resources. However,
the subsequent scheduling of the 24 pending pods from the 24 pending jobs diverged significantly between the two
approaches. This difference in execution order, rather than resource utilization efficiency, accounts for the observed
variation in makespan.
Kueue’s strict BestEffortFIFO ordering produced predictable and reproducible scheduling patterns. Once the initial
eight jobs occupied all GPU resources, Kueue consistently scheduled jobs in the sequence: medium-4, large-4, medium-
5, large-5, and so forth through medium-15 and large-15. This deterministic behavior stems from Kueue’s queue
management logic, which maintains a stable ordering independent of the lower-level Kubernetes scheduler dynamics. In
contrast, the pure-jobs scenario exhibited variable scheduling patterns influenced by the native Kubernetes scheduler’s
internal mechanisms, including its unschedulable and backoff queue policies. While the first eight jobs remained
consistent, subsequent job ordering varied across runs—sometimes following the medium-4, large-4 pattern, but other
times deviating based on transient cluster state and scheduler heuristics.
This comparison underscores a fundamental tension in batch workload management: the native Kubernetes scheduler’s
opportunistic approach may occasionally yield a shorter makespan through fortuitous ordering, but Kueue provides
guarantees of fairness, predictability, and reproducibility, which are qualities essential for multi-tenant and production
environments. The deterministic behavior offered by Kueue ensures that workload execution patterns remain consistent
across runs, enabling reliable capacity planning, accurate service level agreement commitments, and equitable resource
sharing among competing workloads.
While the baseline scenario achieved a lower makespan than Kueue’s BestEffortFIFO, there is a clear progression
in scheduling efficiency and workload balance across successive Kueue configurations. Adding higher workload
priority to the large Whisper jobs through WorkloadPriorityClass reduced the makespan to 54.7 min and improved
throughput for large jobs, reducing their queue times by nearly half, at the expense of longer delays for medium jobs.
Enabling preemption along with higher workload priority provided the best improvement in overall makespan among all
Kueue-based configurations (51.1 minutes; an improvement of about 15% compared to the baseline), as higher-priority
jobs could interrupt and reclaim GPU resources, achieving faster turnaround without compromising execution efficiency.
Interestingly, while large jobs gained most from this strategy, their longer execution durations still dominated the tail of
the makespan distribution. It is important to note that while queueing time is effectively 0, median job execution times
are higher (28.4 minutes) in the pure-jobs configuration because pods are created immediately after job submission,
allowing all workloads to begin execution concurrently even when several pods are unschedulable due to lack of
resources. In contrast, with Kueue-managed scheduling, a workload is only admitted when adequate resources become
available, after which the corresponding job and pod are created, leading to lower execution times (8.7–9.5 minutes).
The final configuration, employing two ClusterQueues with borrowing enabled, struck the best balance between fairness
and utilization. It allowed underutilized GPUs from the medium ClusterQueue queue to be dynamically reallocated
once all medium jobs completed execution, improving medium job latency while maintaining near-optimal makespan.
Together, these results demonstrate how combining queue management, priority, preemption, and resource sharing
enables Kueue to reduce total makespan while gracefully handling large, long-running jobs in heterogeneous AI
workloads.
10


---

A PREPRINT - FEBRUARY 12, 2026
Kueue introduces an admission control mechanism that determines when a workload can be accepted into execution based
on the availability of cluster resources and the queue policy. To this end, we investigate the admission attempt durations,
that is, the time between a workload’s admission attempt and Kueue’s final admission response. Table 4 summarizes the
median (P50), 95th, and 99th percentile admission attempt durations across multiple Kueue configurations. All values
remain below 25 ms even at the 99th percentile, demonstrating that the queueing logic imposes negligible overhead on
scheduling latency. Priority and preemption have little effect on admission latency, maintaining sub-10 ms decision
times across all percentiles. The configuration with two LocalQueues and two ClusterQueues exhibits slightly higher
tail latencies (P95 ~17 ms, P99 ~24 ms) due to cross-queue synchronization and resource borrowing coordination.
Nonetheless, the absolute values remain operationally negligible compared to job execution times, showcasing that
Kueue’s admission control scales efficiently and can enforce advanced policies such as priority and borrowing without
incurring measurable delay in workload admission.
Table 4: Mean admission attempt duration across Kueue configurations (averaged over 3 runs). All values in millisec-
onds.
Configuration
Admission Attempt Duration (ms)
P50
P95
P99
Kueue BestEffortFIFO
2.67
7.64
20.9
+ Priority
2.51
4.77
6.48
+ Priority + Preemption
2.57
5.48
12.8
2 LocalQueues + 2 ClusterQueues
2.82
17.0
23.7
4.2
Accelerator Slicing: DAS
To assess the impact of utilizing DAS, we executed the second test case outlined in Section 3.3.2 in two different setups,
one using DAS to partition GPUs and another without it, having each job consume an entire GPU. In short, this test case
aimed to quantify the efficiency gains obtained when using dynamic MIG-slicing to split the GPUs into smaller slices
in order to run more than one Whisper transcription job concurrently. Theoretically, this method offers efficiency gains
as each job no longer consumes an entire physical GPU. The GPU instance profiles have been selected depending on
the Whisper model resource requirements [46]: for the medium model jobs the slice size had been set to 1g.5gb and
for large model jobs to 3g.20gb. Similar to the Kueue experiments discussed in 4.1, the terms large and medium refer
exclusively to the Whisper model variants used for transcription and have no relation to the duration or size of the input
audio files.
At the time of this writing, the integration between the DAS operator and Kueue for GPU memory aware scheduling of
AI workloads was still in progress [51], so the following experiments were orchestrated directly through Kubernetes
native jobs utilizing DAS. By exposing GPU memory as an extended resource, Kueue will be able to enforce quotas and
admission control while DAS continues to manage MIG slice allocation. The integration will focus on long-running
workloads (Pods, Deployments, StatefulSets) commonly used for inference and serving applications.
Before investigating efficiency gains, we wanted to gain a thorough understanding of the impact of running a Whisper
transcription job on a MIG slice as opposed to a full GPU. To this end, we conducted an experiment with only eight
total jobs, half running the medium model and half running the large model. This setup ensured that waiting time was
excluded from the job completion time measurement as both variants provided enough concurrency primitives: eight
GPUs for no MIG setup and eight GPU slices for the MIG setup. Each configuration (no MIG vs. MIG) was run three
times, the aggregated results are presented in Table 5.
The mean job completion time increased from 14 minutes without slicing to 17 minutes with slicing, a similar increase
is evident in the median (from 10 to 13 minutes) and the 95th percentile (from 36 to 40 minutes). The average GPU
utilization dropped from 45% without slicing to 19% with slicing and all other relevant GPU metrics showed almost no
difference. Running Whisper transcription jobs on proposed slice sizes introduced moderate but noticeable performance
degradation due to resource constraints.
After understanding the performance characteristics of Whisper jobs on MIG slices, we conducted a second experiment
with the complete dataset of 32 jobs to assess the potential gains that can be achieved with parallel execution utilizing
DAS. Again, each configuration (no DAS vs. DAS) was run three times and kube-burner was used to create one medium
Whisper model job followed by a large Whisper model job until 16 jobs of each kind were created for each run. The
aggregated results are presented in Table 6.
11


---

A PREPRINT - FEBRUARY 12, 2026
Table 5: Influence of MIG slicing on transcription job performance with limited concurrency (24 jobs across 3 runs,
each with 8 parallel executions).
Metric
No MIG
MIG
Concurrent Jobs
8
8
Mean Job Completion Time, min
14
17
Median Job Completion Time, min
10
13
P95 Job Completion Time, min
36
40
Std Dev, min
11
11
Mean GPU Utilization, %
45
19
Mean Tensor Core Utilization, %
1
1
Mean DRAM Activity, %
13
12
Mean Worker CPU Utilization, %
9
9
Table 6: Influence of DAS on transcription job performance on the full dataset (96 jobs across 3 runs).
Metric
No DAS
DAS
Concurrent Jobs
8
25
Mean Job Completion Time, min
28
18
Median Job Completion Time, min
28
16
P95 Job Completion Time, min
51
36
Std Dev, min
14
9
Mean GPU Utilization, %
44
38
Mean Tensor Core Utilization, %
1
3
Mean DRAM Activity, %
13
38
Mean Worker CPU Utilization, %
10
37
DAS dynamically increased the number of concurrently running transcription jobs from 8 (number of GPUs) to 25
(number of slices for the given specifications). This enhanced parallelism reduced the overall completion time of jobs,
which is here equivalent to the sum of waiting time and execution time. The mean job completion time decreased
from 28 minutes without DAS to 18 minutes with DAS. Similar improvements are evident in the median (from 28 to
16 minutes) and the 95th percentile (from 51 to 36 minutes). Comparing these results with the first experiment, the
execution parallelization achieved with DAS and the resulting waiting times reduction essentially over-weighted the
execution time degradation caused by resource constraints on MIG slices.
Increased execution parallelism also resulted in increased resource utilization. With DAS, both the average tensor core
utilization and the average DRAM activity were almost three times higher. Note that the moderate decrease of the
average GPU utilization is due to the nature of MIG-slicing: with certain slice allocation patterns (e.g., two 3g.20gb
GPU instance profiles scheduled on a single A100 GPU), the residual capacity may not be allocable, resulting in some
streaming multiprocessors remaining idle and consequently lowering the overall utilization average. Despite this “slice
padding”, the overall idle capacity had been significantly reduced. Also the average CPU utilization on the worker node
was nearly four times higher with DAS (37%) compared to the not using DAS case (10%).
4.3
Distributed Inference: llm-d and Gateway API Inference Extension
To evaluate distributed inference performance using llm-d and the InferenceGateway (via the Gateway API Inference
Extension, GAIE), we adopted intelligent inference scheduling through the Precise Prefix cache aware (also known as
Precise Scheduling) “well-lit path"" in the final stage of our workflow. The setup deployed eight replicas of vLLM, each
serving the Qwen/Qwen3-8B model, behind an OpenShift Service Mesh envoy proxy [52] acting as the GAIE gateway.
The first experiment investigates the latency overhead introduced by the GAIE components as explained in Section 3.3.3.
To avoid potential latency introduced by the LLM runtime and accurately measure overhead, a single replica of the
vLLM Simulator [53] pod was used instead of a real vLLM instance. Figure 5 depicts the overhead where the x-axis is
reflecting the concurrency level (the number of synchronous streams in parallel) and the y-axis the average Time to
First Token (TTFT) in ms. Throughout the experiment, the ClusterIP and Inference Pool-disabled scenarios exhibit
very similar TTFT. In contrast, in the Inference Pool-enabled scenario this latency is between 3 ms and 11 ms higher
compared to the other two setups. TTFT increases with concurrency and the Inference Pool-enabled scenario exhibits
the highest latency, especially at higher concurrency levels, introduced by additional routing overhead added by the
12


---

A PREPRINT - FEBRUARY 12, 2026
gateway and endpoint picker components. This overhead is clearly offset by the latency improvements observed in the
results that follow with real-world LLM use cases.
6
8
10
12
14
16
18
20
Concurrency
20
30
40
50
60
70
TTFT (ms)
ClusterIP
InferencePool: On
InferencePool: Off
Figure 5: Comparison of the overhead introduced by different scenarios. Error bars indicate 95% confidence interval.
To evaluate the routing strategies within GAIE, we compare two endpoint selection strategies: Random Scheduling
(Baseline) and Precise Scheduling (“well-lit path”). The workload comprised the 32 previously transcribed earnings call
audio clips, each averaging 8,500 tokens (totaling 272,000 tokens). Using GuideLLM, we generated 256 summarization
requests (each of the 32 prompts repeated eight times) sent through the inference gateway, with a maximum output of
512 tokens per request for the summary. Each vLLM replica ran on an NVIDIA A100 40GB GPU, with a per-instance
KV-cache capacity of 143,360 tokens, yielding an aggregate of 1,146,880 tokens across all GPUs. As the total prompt
volume exceeds the per-GPU cache capacity, random routing inevitably incurs cache misses and LLM prefilling
overhead. In contrast, Precise Scheduling maximizes the reuse of hot prefixes across replicas, demonstrating measurable
gains in cache hit rate, end-to-end latency, and overall throughput.
Figure 6 compares the TTFT between Random Scheduling and Precise Scheduling. The x-axis displays the synchronous
concurrent streams in parallel, while the y-axis, in logarithmic scale, represents the TTFT in milliseconds. On average,
Random Scheduling (∼500 ms) is approximately 6.25 times slower than Precise Scheduling (∼80 ms). Also, the
variability is significantly higher for Random Scheduling, with a 95% confidence spread of 100 ms compared to about 6
ms for Precise Scheduling. Additionally, the 99th percentile of TTFT for Random Scheduling is up to 20 times slower
than Precise Scheduling.
6
8
10
12
14
16
18
20
Concurrency
101
102
103
TTFT (ms)
Random Scheduling (mean)
Random Scheduling (P99)
Precise-Scheduling (mean)
Precise-Scheduling (P99)
Figure 6: Comparison of the Time to First Token (TTFT). Error bars indicate 95% confidence interval. Y-axis is
depicted in log-scale.
Figure 7 illustrates the per-request end-to-end request latency difference between Random Scheduling and Precise
Scheduling routing strategies. The x-axis depicts the concurrency and the y-axis the latency. Random scheduling, on
13


---

A PREPRINT - FEBRUARY 12, 2026
6
8
10
12
14
16
18
20
Concurrency
8
10
12
14
e2e-latency (seconds)
Random Scheduling (mean)
Random Scheduling (P99)
Precise-Scheduling (mean)
Precise-Scheduling (P99)
Figure 7: Comparison of the per request end-to-end request latency. Error bars indicate 95% confidence interval.
average, incurs a latency of approximately 1 second more compared to Precise Scheduling. Moreover, for the 99th
percentile of end-to-end latency, Random Scheduling can be up to 6 s slower. Additionally, it exhibits higher variability,
as evident from the error bars and the significantly higher 99th percentile, suggesting that Precise Scheduling provides
better predictability.
4.4
Takeaways
Across all experiments, the results collectively demonstrate the complementary strengths of Kueue, DAS, and distributed
inference scheduling (via GAIE) in improving the efficiency, predictability, and scalability of AI inference workloads
on Kubernetes. Kueue provides deterministic, fair, and reproducible scheduling with negligible admission latency
overhead, while priority and preemption mechanisms substantially reduce makespan, by up to 15% and improve
throughput for large jobs. DAS enhances parallelism by dynamically partitioning GPUs into smaller slices, achieving
up to a 36% reduction in mean job completion time despite modest per-job slowdowns from resource sharing. Finally,
GAIE’s intelligent inference routing with Precise Scheduling decreases latency, i.e., reducing 99th percentile TTFT
by up to 90% and end-to-end latency by up to 25% , while ensuring stable, predictable performance even at the
highest concurrency. Together, these findings highlight a clear design direction: combining queue-aware scheduling,
dynamic accelerator partitioning, and intelligent inference routing yields an adaptive and efficient orchestration stack
for large-scale, heterogeneous AI workloads, balancing throughput, fairness, and determinism across the stack.
5
Conclusion
This work demonstrates that Kubernetes, when extended with emerging AI-oriented projects, can effectively serve
as a unified substrate for both batch and online Generative AI (GenAI) workloads. By combining Kueue, Dynamic
Accelerator Slicer (DAS), and the Gateway API Inference Extension (GAIE), we have shown that the Kubernetes
ecosystem is capable of orchestrating complex, multi-stage inference pipelines with improved utilization, scalability, and
responsiveness. Our showcase, Whisper-based transcription and LLM-driven summarization, illustrates that container-
native design principles can be successfully applied to heterogeneous, GPU-accelerated environments traditionally
dominated by bespoke AI infrastructure.
Beyond technical integration, our evaluation reveals the operational synergies among these components: Kueue
optimizes batch job queueing (up to 15%), DAS maximizes accelerator efficiency through fine-grained GPU slicing (up
to 36%), and GAIE with the llm-d-inference-scheduler implementation delivers optimized Time to First Token (up to
82%). Together, these elements represent a cohesive and forward-looking framework for self-hosting GenAI inference
workloads in production-grade Kubernetes clusters. We conclude that the rapid evolution of Kubernetes-native AI
extensions signals a maturing ecosystem—one that is increasingly capable of addressing the stringent performance,
scalability, and manageability demands of modern AI systems.
14


---

A PREPRINT - FEBRUARY 12, 2026
6
Future Work
Future work will focus on extending the evaluation to encompass a broader set of AI workloads, including multimodal
and streaming inference scenarios, to further validate the flexibility and scalability of the proposed workflow. As
previously stated, native integration between Kueue and Dynamic Accelerator Slicing (DAS) is currently lacking;
therefore we plan to have future work focusing on characterizing an end-to-end batch inference workflow that combines
Kueue for workload queueing and DAS for accelerator slicing within Kubernetes. Additionally, we plan to integrate
and assess emerging Kubernetes-native components for AI inference, such as gang scheduling and Dynamic Resource
Allocation (DRA), to enhance the system’s efficiency and responsiveness under dynamic workloads. These efforts
aim to generalize the presented framework and position it as a foundational blueprint for end-to-end AI workload
orchestration on cloud-native infrastructure.
15


---

A PREPRINT - FEBRUARY 12, 2026
References
[1] Nestor Maslej, Loredana Fattorini, et al. The AI Index 2025 Annual Report, April 2025. Accessed: November 1,
2025.
[2] Michael Gschwind. AI: It’s All About Inference Now. ACM Queue, 23, May 2025.
[3] Grand View Research. Cloud ai market size, share & trends: Industry report, 2030. Technical Report GVR-4-
68040-055-7, Grand View Research, Inc., 2025. Historical range: 2018–2024, Forecast period: 2025–2030.
[4] Brendan Burns, Brian Grant, David Oppenheimer, Eric Brewer, and John Wilkes. Borg, omega, and kubernetes:
Lessons learned from three container-management systems over a decade. Queue, 14(1):70–93, 2016.
[5] Cloud Native Computing Foundation. Cncf 2023 annual survey. https://www.cncf.io/reports/cncf-annual-survey-
2024/, 2023. Accessed: 2025-10-13.
[6] Kueue Project Authors. Kueue: Kubernetes-native job queueing. https://kueue.sigs.k8s.io/, 2025.
Accessed: 2025-09-29.
[7] Red Hat Inc.
Dynamic Accelerator Slicer (DAS) Operator.
https://github.com/openshift/
instaslice-operator, 2025. Accessed: 2025-11-01.
[8] Kubernetes SIGs. Gateway api inference extension: Kubernetes-native routing & scheduling for genai inference
workloads, 2025. Accessed: 2025-10-27.
[9] Miguel Del Rio, Corey Miller, Jan Profant, Jennifer Drexler-Fox, Quinn McNamara, Nishchal Bhandari, Natalie
Delworth, Ilya Pirkin, Migüel Jetté, Shipra Chandra, Peter Ha, and Ryan Westerman. Accents in speech recognition
through the lens of a World Englishes evaluation set. Research in Language, 21(3):225–244, 2023.
[10] Market Research Future. Automatic speech recognition software market size | cagr of 24.16%, 2024. Accessed:
2025-10-04.
[11] llm-d Project Authors. llm-d: a kubernetes-native high-performance distributed llm inference framework. https:
//llm-d.ai/, 2025. Accessed: 2025-09-29.
[12] The Kubernetes Authors. Kubernetes device plugin framework. https://kubernetes.io/docs/concepts/
extend-kubernetes/compute-storage-net/device-plugins/, 2025. Accessed: 2025-09-29.
[13] The Kubernetes Authors.
Kubernetes topology manager.
https://kubernetes.io/docs/tasks/
administer-cluster/topology-manager/, 2025. Accessed: 2025-09-29.
[14] The Kubernetes Authors. Dynamic resource allocation (dra) in kubernetes. https://kubernetes.io/docs/
concepts/scheduling-eviction/dynamic-resource-allocation/, 2025. Accessed: 2025-09-29.
[15] The Kubernetes Authors. Resource quotas in kubernetes. https://kubernetes.io/docs/concepts/policy/
resource-quotas/, 2025. Accessed: 2025-09-29.
[16] NVIDIA Corporation. Nvidia multi-instance gpu (mig). https://docs.nvidia.com/datacenter/tesla/
mig-user-guide/, 2025. Accessed: 2025-09-29.
[17] NVIDIA Corporation.
Nvidia gpu operator for kubernetes.
https://docs.nvidia.com/datacenter/
cloud-native/gpu-operator/, 2025. Accessed: 2025-09-29.
[18] NVIDIA Corporation. Run:ai: Gpu orchestration. https://docs.run.ai/v2.20/home/overview/, 2025.
Accessed: 2025-01-01.
[19] Edward J Hu, Yelong Shen, Phillip Wallis, Zeyuan Allen-Zhu, Yuanzhi Li, Shean Wang, Lu Wang, Weizhu Chen,
et al. Lora: Low-rank adaptation of large language models. ICLR, 1(2):3, 2022.
[20] Woosuk Kwon, Sang-Woo Lee, Hyunji Lee, Beomsu Kim, and Hyeontaek Lim. vllm: Easy, fast, and cheap llm
serving with pagedattention, 2023.
[21] Zeineb Rejiba and Javad Chamanara. Custom scheduling in kubernetes: A survey on common problems and
solution approaches. ACM Comput. Surv., 55(7), December 2022.
[22] Zhisheng Ye, Wei Gao, Qinghao Hu, Peng Sun, Xiaolin Wang, Yingwei Luo, Tianwei Zhang, and Yonggang Wen.
Deep learning workload scheduling in gpu datacenters: A survey. ACM Comput. Surv., 56(6), January 2024.
[23] Han Dong, Parul Singh, Yara Awad, Felix George, Krishnasuri Narayanam, Sanjay Arora, and Jonathan Appavoo.
Towards performance and energy aware kubernetes scheduler. SIGENERGY Energy Inform. Rev., 5(2):69–75,
August 2025.
[24] Shihao Shen, Yiwen Han, Xiaofei Wang, Shiqiang Wang, and Victor C. M. Leung. Collaborative learning-
based scheduling for <italic>kubernetes</italic>-oriented edge-cloud network.
IEEE/ACM Trans. Netw.,
31(6):2950–2964, April 2023.
16


---

A PREPRINT - FEBRUARY 12, 2026
[25] Jun Zhou, Ke Zhang, and et.al. Elasticdl: A kubernetes-native deep learning framework with fault-tolerance
and elastic scheduling. In Proceedings of the Sixteenth ACM International Conference on Web Search and Data
Mining, WSDM ’23, page 1148–1151, New York, NY, USA, 2023. Association for Computing Machinery.
[26] Yihao Zhao, Xin Liu, Shufan Liu, Xiang Li, Yibo Zhu, Gang Huang, Xuanzhe Liu, and Xin Jin. Muxflow:
Efficient and safe gpu sharing in large-scale production deep learning clusters. arXiv preprint arXiv:2303.13803,
2023.
[27] Georgios Alexopoulos and Dimitris Mitropoulos. nvshare: Practical gpu sharing without memory size constraints.
In Proceedings of the 2024 IEEE/ACM 46th International Conference on Software Engineering: Companion
Proceedings, ICSE-Companion ’24, page 16–20, New York, NY, USA, 2024. Association for Computing
Machinery.
[28] Liu Liu, Jian Yu, and Zhijun Ding. Adaptive and efficient gpu time sharing for hyperparameter tuning in cloud. In
Proceedings of the 51st International Conference on Parallel Processing, ICPP ’22, New York, NY, USA, 2023.
Association for Computing Machinery.
[29] Xinpeng Wei, Zhichao Li, and Cheng Tan. Optimizing gpu sharing for container-based dnn serving with multi-
instance gpus. In Proceedings of the 17th ACM International Systems and Storage Conference, SYSTOR ’24,
page 68–82, New York, NY, USA, 2024. Association for Computing Machinery.
[30] Guilin Zhang, Wulan Guo, Ziqi Tan, Qiang Guan, and Hailong Jiang. Kis-s: A gpu-aware kubernetes inference
simulator with rl-based auto-scaling. arXiv preprint arXiv:2507.07932, 2025.
[31] Giovanni Bartolomeo, Navidreza Asadi, Wolfgang Kellerer, Jorg Ott, and Nitinder Mohan. {On-Demand}
container partitioning for distributed {ML}. In 2025 USENIX Annual Technical Conference (USENIX ATC 25),
pages 1481–1500, 2025.
[32] Diego Ciangottini, Giulio Bianchini, Mirko Mariotti, Daniele Spiga, Loriano Storchi, and Giacomo Surace. Kserve
inference extension for an fpga vendor-free ecosystem. In EPJ Web of Conferences, volume 295, page 11012.
EDP Sciences, 2024.
[33] Lianmin Zheng, Zhuohan Li, Hao Zhang, Yonghao Zhuang, Zhifeng Chen, Yanping Huang, Yida Wang,
Yuanzhong Xu, Danyang Zhuo, Eric P Xing, et al. Alpa: Automating inter-and {Intra-Operator} parallelism for
distributed deep learning. In 16th USENIX Symposium on Operating Systems Design and Implementation (OSDI
22), pages 559–578, 2022.
[34] Zhe Zhou, Xuechao Wei, Jiejing Zhang, and Guangyu Sun. {PetS}: A unified framework for {Parameter-Efficient}
transformers serving. In 2022 USENIX Annual Technical Conference (USENIX ATC 22), pages 489–504, 2022.
[35] Yinmin Zhong, Shengyu Liu, Junda Chen, Jianbo Hu, Yibo Zhu, Xuanzhe Liu, Xin Jin, and Hao Zhang.
{DistServe}: Disaggregating prefill and decoding for goodput-optimized large language model serving. In 18th
USENIX Symposium on Operating Systems Design and Implementation (OSDI 24), pages 193–210, 2024.
[36] Minxian Xu, Junhan Liao, Jingfeng Wu, Yiyuan He, Kejiang Ye, and Chengzhong Xu. Cloud native system for
llm inference serving. arXiv preprint arXiv:2507.18007, 2025.
[37] Yao Fu, Leyang Xue, Yeqi Huang, Andrei-Octavian Brabete, Dmitrii Ustiugov, Yuvraj Patel, and Luo Mai.
{ServerlessLLM}:{Low-Latency} serverless inference for large language models. In 18th USENIX Symposium
on Operating Systems Design and Implementation (OSDI 24), pages 135–153, 2024.
[38] Pasquale De Rosa, Yérom-David Bromberg, Pascal Felber, Djob Mvondo, and Valerio Schiavoni. On the cost
of model-serving frameworks: An experimental evaluation. In 2024 IEEE International Conference on Cloud
Engineering (IC2E), pages 221–232. IEEE, 2024.
[39] Alec Radford, Jong Wook Kim, Tao Xu, Greg Brockman, Christine McLeavey, and Ilya Sutskever. Robust
speech recognition via large-scale weak supervision. In International conference on machine learning, pages
28492–28518. PMLR, 2023.
[40] Rohit Prabhavalkar, Takaaki Hori, Tara N. Sainath, Ralf Schlüter, and Shinji Watanabe. End-to-end speech
recognition: A survey. IEEE/ACM Transactions on Audio, Speech, and Language Processing, 32:325–351, 2024.
[41] Jaehyun Park, Hyeonkyu Noh, Hyunjoon Nam, Won-Cheol Lee, and Hong-June Park. A low-latency streaming
on-device automatic speech recognition system using a cnn acoustic model on fpga and a language model on
smartphone. Electronics, 11(12):1831, 2022.
[42] Gaurav Singh, Harshal Patil, and Abhishek Malvankar. The benefits of dynamic gpu slicing in openshift. https://
developers.redhat.com/articles/2025/05/06/benefits-dynamic-gpu-slicing-openshift, 2025.
Accessed: 2025-10-29.
17


---

A PREPRINT - FEBRUARY 12, 2026
[43] llm-d.ai. Kv-cache wins you can see. https://llm-d.ai/blog/kvcache-wins-you-can-see, 2024. Ac-
cessed: 2025-10-27.
[44] Amazon Web Services, Inc. . Red hat openshift service on aws. https://aws.amazon.com/rosa/, 2024.
Accessed: 2025-10-27.
[45] Sai Sindhur Malleni, José Castillo Lema, Raúl Sevilla, and Aleksei Vasilevskii.
Manifests for icpe’26.
https://github.com/RedHatResearch/icpe26-k8s-ai-apis, 2025. Accessed: 2025-11-01.
[46] OpenAI. Whisper github page. https://github.com/openai/whisper, 2022. GitHub repository. Accessed:
2025-11-07.
[47] Hugging Face. Qwen3-8B. https://huggingface.co/Qwen/Qwen3-8B, 2024. Accessed: 2025-10-27.
[48] llm-d.ai.
Intelligent
inference
scheduling
with
llm-d.
https://llm-d.ai/blog/
intelligent-inference-scheduling-with-llm-d, 2024. Accessed: 2025-10-27.
[49] Sai Sindhur Malleni, Raúl Sevilla Canavate, and Vishnu Challa. Into the fire: Delving into kubernetes performance
and scale with kube-burner. In Companion of the 15th ACM/SPEC International Conference on Performance
Engineering, ICPE ’24 Companion, page 89–90, New York, NY, USA, 2024. Association for Computing
Machinery.
[50] Neural Magic, Inc. Guidellm: Scalable inference and optimization for large language models. https://github.
com/vllm-project/guidellm, 2024.
[51] OpenShift Team.
Integration with kueue in instaslice operator.
https://github.com/openshift/
instaslice-operator/pull/910, 2025. Accessed: 2025-11-07.
[52] Envoy Project. Envoy proxy. https://www.envoyproxy.io/, 2025. Accessed: 2025-10-27.
[53] llm-d Developers. llm-d-inference-sim: A light weight vllm simulator, for mocking out replicas. https:
//github.com/llm-d/llm-d-inference-sim, 2025. GitHub repository. Published: May 2025 (Project
Announcement), Last Updated: November 6, 2025.
18
