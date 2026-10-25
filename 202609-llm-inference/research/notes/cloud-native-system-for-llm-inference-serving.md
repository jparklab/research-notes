---
title: Cloud Native System for LLM Inference Serving
id: cloud-native-system-for-llm-inference-serving
tags:
- internal-llm-inference-stacks-7f0c90
- cloud-native
- audited-weak-evidence
created: '2026-10-09T22:55:11.854783Z'
updated: '2026-10-10T14:52:17.069242Z'
source: https://arxiv.org/abs/2507.18007
source_domain: arxiv.org
fetched_at: '2026-10-09T22:55:11.854479Z'
fetch_provider: crawl4ai
status: review
type: note
tier: institutional
content_type: paper
deprecated: false
summary: 'Xu, Liao, Wu, He, Ye (Shenzhen Institute of Advanced Technology, CAS / Univ.
  of Macau), arXiv 2507.18007, J.ACM 2025 — a short (~4,700-word), position-paper-style
  article arguing that cloud-native primitives (containerization, microservices, Kubernetes-based
  dynamic scheduling) should underpin LLM inference serving, rather than a report
  of an operator''s actual production stack. RELIABILITY CAVEAT: this is weak evidence
  relative to the other papers in this batch — it is not first-person deployment evidence
  from an operator running LLM inference at scale, its empirical validation is a single
  small-scale lab experiment, and its central proposed technique (decomposing each
  individual Transformer layer of LLaMA-2-13B into its own Kubernetes microservice,
  connected via gRPC and Istio service mesh) is not how any disclosed production LLM
  serving stack in this research area actually operates — no major open-source engine
  (vLLM, SGLang, TGI) or named operator (Mooncake/Kimi, DistServe) partitions at per-layer
  granularity; this is a research proposal, not observed industry practice. Cites
  standard figures as motivation (inference is ''up to 90%'' of total ML cloud cost;
  training GPT-4-class models needs ''25,000 A100s for 90-100 days''; GPU clusters
  reportedly run at ''~30% utilization'' with ''>70% of allocated resources idle outside
  peak'') without primary sourcing for these numbers. Names PagedAttention, continuous
  batching, Llumnix, and DistServe as the state-of-the-art request-migration/latency-reduction
  techniques it positions its proposal against. Experimental setup: 3-node Kubernetes
  cluster, each node one NVIDIA A100-80GB (NVLink), LLaMA-2-13B with each of 40 Transformer
  layers run as a separate microservice, Prometheus/Grafana monitoring, Locust-generated
  load (50-2048 token inputs). Finds one bottleneck layer (Layer 27) with max latency
  over 230x that of another layer (Layer 30) under load, then shows Kubernetes HPA-based
  autoscaling of just that bottleneck layer''s microservice reduces its latency from
  15.23s to 12.28s and raises overall system throughput from 4.07 to 5.05 QPS at batch
  size 62 — a single-configuration, single-model result with no comparison to standard
  (non-decomposed) serving baselines like vLLM running the same model, and no discussion
  of the cross-microservice communication overhead that per-layer gRPC calls would
  add in a real deployment. Net assessment: useful only as an illustration of the
  general cloud-native-primitives framing (load balancing, autoscaling, migration,
  load prediction, profiling, fine-grained modularization) that NVIDIA Dynamo, llm-d,
  and AIBrix implement in production-proven form at the service-replica level (not
  per-layer); should not be cited for production-stack claims.'
raw_file: raw/cloud-native-system-for-llm-inference-serving.pdf
doi: arXiv:2507.18007
utility_score: 16.0
citation_count: 8
venue: arXiv.org
is_retracted: false
---

Cloud Native System for LLM Inference Serving
MINXIAN XU, JUNHAN LIAO, JINGFENG WU, YIYUAN HE, KEJIANG YE∗, Shenzhen Institute
of Advanced Technology, Chinese Academy of Sciences; University of Chinese Academy of Sciences, China
CHENGZHONG XU, State Key Lab of IOTSC, University of Macau, China
Large Language Models (LLMs) are revolutionizing numerous industries, but their substantial computational
demands create challenges for efficient deployment, particularly in cloud environments. Traditional approaches
to inference serving often struggle with resource inefficiencies, leading to high operational costs, latency
issues, and limited scalability. This article explores how Cloud Native technologies—such as containerization,
microservices, and dynamic scheduling—can fundamentally improve LLM inference serving. By leveraging
these technologies, we demonstrate how a Cloud Native system enables more efficient resource allocation,
reduces latency, and enhances throughput in high-demand scenarios. Through real-world evaluations using
Kubernetes-based autoscaling, we show that Cloud Native architectures can dynamically adapt to workload
fluctuations, mitigating performance bottlenecks while optimizing LLM inference serving performance. This
discussion provides a broader perspective on how Cloud Native frameworks could reshape the future of
scalable LLM inference serving, offering key insights for researchers, practitioners, and industry leaders in
cloud computing and artificial intelligence.
CCS Concepts: • Networks →Cloud computing; • Computing methodologies →Distributed algo-
rithms.
Additional Key Words and Phrases: Cloud Native, LLM, Inference Serving, Resource Management
ACM Reference Format:
Minxian Xu, Junhan Liao, Jingfeng Wu, Yiyuan He, Kejiang Ye and Chengzhong Xu. 2025. Cloud Native System
for LLM Inference Serving. J. ACM 0, 0, Article 0 ( 2025), 10 pages. https://doi.org/10.1145/nnnnnnn.nnnnnnn
1
Introduction
Since the release of GPT-3.5 [3] in 2022, LLMs have gained widespread attention and are now
applied in various domains, including healthcare, law, and education. Rapid advancement of LLMs
has led to the development of numerous new models, such as the GPT series (GPT-1, GPT-2,
GPT-3, etc.), the LLaMA family (LLaMA2 [12], LongChat [10]), BLOOM [13], FALCON [1], GLM
[5], Mistral [8], PaLM [2], and DeepSeek [6]. These models have attracted strong interest from
both academia and industry, fueling innovation in artificial intelligence and natural language
processing. However, as these models grow in scale and complexity, their computational demands
have increased exponentially. Figure 1 illustrates the evolution of LLM parameters, revealing a
trend to increase the size of the model over time. Training state-of-the-art models such as GPT-
4 requires at least 25,000 NVIDIA A100 GPUs running continuously for 90 to 100 days, while
deploying these models for real-time inference demands at least 128 A100 GPUs to maintain service
∗Corresponding author
Authors’ Contact Information: Minxian Xu, Junhan Liao, Jingfeng Wu, Yiyuan He, Kejiang Ye, {mx.xu,jh.liao2,jf.wu2,yy.
he2,kj.ye}@siat.ac.cn, Shenzhen Institute of Advanced Technology, Chinese Academy of Sciences; University of Chinese
Academy of Sciences, China; Chengzhong Xu, czxu@um.edu.mo, State Key Lab of IOTSC, University of Macau, China.
Permission to make digital or hard copies of all or part of this work for personal or classroom use is granted without fee
provided that copies are not made or distributed for profit or commercial advantage and that copies bear this notice and the
full citation on the first page. Copyrights for components of this work owned by others than the author(s) must be honored.
Abstracting with credit is permitted. To copy otherwise, or republish, to post on servers or to redistribute to lists, requires
prior specific permission and/or a fee. Request permissions from permissions@acm.org.
© 2025 Copyright held by the owner/author(s). Publication rights licensed to ACM.
ACM 1557-735X/2025/0-ART0
https://doi.org/10.1145/nnnnnnn.nnnnnnn
J. ACM, Vol. 0, No. 0, Article 0. Publication date: 2025.
arXiv:2507.18007v1  [cs.DC]  24 Jul 2025


---

0:2
Xu et al.
Fig. 1. The Development of Parameters in Current Mainstream Open-source LLMs.
performance. With the widespread adoption of LLMs, inference has become the dominant cost
factor in cloud-based machine learning, accounting for up to 90% of total expenditures.
LLM inference serving, the process of deploying trained models to generate real-time predictions,
is computationally intensive due to frequent auto-regressive computations, memory access, and
storage operations. As a result, inference performance is highly dependent on GPU resources,
directly affecting both service efficiency and user experience. Furthermore, increasing user ex-
pectations demand lower latency and greater stability in LLM responses. Two key performance
metrics define inference efficiency: latency and throughput. Latency is primarily determined by
Time to First Token (TTFT), Time Per Output Token (TPOT), and the total response token count,
while throughput reflects the number of requests processed per second. Traditional LLM inference
services often suffer from inefficiencies. To improve throughput, techniques such as PagedAtten-
tion [9], Continuous Batching [14] have been developed and integrated into inference systems.
Currently, methods such as Llumnix [11] and DistServe [15], which leverage request migration
strategies, have been proposed to reduce end-to-end latency, queueing delays, and TTFT. However,
these optimizations introduce trade-offs: reducing latency often comes at the cost of lower through-
put, while increasing throughput may lead to significantly higher per-request latency, making it
challenging to achieve an optimal balance between the two.
Despite advances in LLM technology, cloud-based inference deployment remains inefficient,
leading to considerable resource under-utilization and scalability challenges. Most LLMs rely on
Transformer architectures with self-attention mechanisms and are typically deployed as large,
monolithic models. Their sheer size (e.g., GPT-4 with 1.8 trillion parameters) results in high overhead
costs related to initialization and replication. In high-concurrency, low-latency environments, cloud
providers frequently over-provision model replicas to ensure service quality, but this approach
exacerbates resource inefficiencies. For example, GPU clusters in data centers often operate at a
low utilization 30%. Although multiple replicas of models are deployed to handle peak demand,
more than 70% of the allocated resources remain idle outside of peak usage periods. This imbalance
in resource allocation significantly limits the scalability and commercial viability of LLM services.
J. ACM, Vol. 0, No. 0, Article 0. Publication date: 2025.


---

Cloud Native System for LLM Inference Serving
0:3
Addressing these inefficiencies requires innovative approaches to resource scheduling and de-
ployment strategies. One promising direction is Cloud Native computing, an architectural paradigm
designed to maximize the scalability, flexibility, and efficiency of applications in cloud environ-
ments. Cloud Native systems leverage technologies such as containerization, microservices, and
dynamic orchestration to enable more efficient resource management. Unlike traditional monolithic
deployments, Cloud Native architectures break down applications into modular components that
can be independently managed, scaled, and optimized based on real-time demand. By applying
these principles to LLM inference serving, Cloud Native frameworks can significantly enhance
resource utilization, reduce latency, and improve throughput. This article explores how Cloud
Native technologies can transform LLM deployment, offering a scalable and cost-effective solution
for cloud-based AI services.
The key insights of this article are as follows:
• Large-scale LLM inference serving faces fundamental challenges in resource utilization, often
leading to inefficiencies in cloud environments.
• Achieving optimal inference performance is difficult due to workload variability, dynamic
resource demands, and imbalances in allocation strategies.
• Cloud Native technologies provide a structured approach to improving LLM inference serving
by enhancing scalability, efficiency, and adaptability.
2
Why Is LLM Inference Serving Hard to Optimize?
Can resource utilization efficiency be improved while maintaining the Service Level Objective (SLO)
of inference services? This question remains a critical challenge for LLM research teams worldwide.
Several fundamental factors contribute to the difficulty of optimizing inference performance:
Uncertainty in Input and Output Length: LLM inference services handle diverse inputs,
with highly variable and unpredictable output lengths. Static resource configurations struggle to
accommodate these variations, making it difficult to efficiently allocate computational resources
and predict system load.
Imbalance in Inference Stages: The inference process consists of multiple stages, such as prefill
and decode, each with different resource demands. Here, the prefill stage refers to the process of
generating the first token by LLMs, while the decode stage refers to the process of generating each
subsequent token after the first. The prefill stage may require intensive computation, while the
decode stage is more memory-bound. This imbalance can lead to inefficient resource usage, with
certain hardware components underutilized while others are overloaded.
Inefficiencies in Traditional Resource Allocation: Static resource allocation methods fail to
adapt to dynamically changing workloads and varying SLOs. Pre-reserving resources and using fixed
configurations often result in over-provisioning (leading to wasted resources) or under-provisioning
(causing performance degradation).
Lack of Fine-Grained System Adaptability: Many inference systems are designed with coarse-
grained modular architectures that lack the flexibility to adjust resource allocation dynamically.
This rigidity makes it difficult to respond efficiently to workload fluctuations and high-concurrency
scenarios.
Resource Competition and Scheduling Bottlenecks: In high-concurrency environments, multi-
ple requests compete for limited computing resources, increasing service latency. Static scheduling
strategies struggle to handle this contention, further exacerbating performance delays and incon-
sistencies.
Hardware and Resource Imbalances: Mismatches between hardware capabilities—such as
GPU compute power, memory bandwidth, and network throughput—often create bottlenecks in
J. ACM, Vol. 0, No. 0, Article 0. Publication date: 2025.


---

0:4
Xu et al.
specific inference tasks. These imbalances lead to suboptimal system performance and reduced
overall efficiency.
Challenges in Managing Burst Workloads: While inference systems are designed with antici-
pated demand in mind, they often fail to handle unexpected traffic spikes effectively. This limitation
results in performance degradation and increased response time during peak loads.
Trade-Off Between Latency and Throughput: A persistent challenge in inference serving
is balancing latency and throughput. Reducing latency can decrease overall system throughput,
while optimizing for throughput can introduce excessive delays for individual requests. Finding an
optimal balance remains a complex problem in large-scale LLM deployment.
Given the challenges of unpredictable workloads in LLM inference and the coarse granularity of
existing system designs, three key issues commonly emerge: inaccurate resource profiling, rigid
scheduling and orchestration, and imbalanced model deployment.
2.1
Inaccurate Resource Profiling
LLM inference tasks exhibit complex and unpredictable load patterns, making it difficult to develop
precise resource profiles. These profiles are critical for optimizing operational efficiency and
resource utilization, especially in distributed and multi-node environments. However, several key
challenges hinder accurate profiling:
Limited Adaptability to Dynamic Hardware Conditions: LLMs run on diverse hardware
platforms (e.g., GPUs and TPUs), yet real-time monitoring of hardware variations remains inade-
quate. Factors such as GPU thermal throttling, scheduling conflicts, and background processes can
degrade performance. When resource profiles fail to capture these fluctuations, scheduling and
optimization strategies become less effective, leading to inefficiencies in model execution.
Inaccurate Task Load Prediction: The workload of LLMs depends on variables such as in-
put size and model complexity. Without precise load predictions, resource scheduling becomes
suboptimal, affecting computational efficiency and response time. In multi-GPU or multi-node envi-
ronments, uneven load distribution can increase latency and degrade overall system performance.
Variability Across Inference Stages: Inference consists of distinct stages, such as prefill
(model loading and input processing) and decode (token generation). These stages impose different
demands—prefill is computation-heavy, while decode relies more on memory and storage band-
width. Additional complexities, such as frequent storage updates during token generation and
shared context management in multi-task scenarios, make precise resource modeling even more
challenging.
These challenges contribute to bottlenecks during peak workloads, ultimately affecting inference
service quality, scalability, and responsiveness, which requires accurate resource profiling for LLM
inference task.
2.2
Rigid Scheduling and Orchestration
In large-scale distributed environments, orchestration and scheduling are critical for managing
resources efficiently. However, existing approaches often introduce excessive complexity and
operational burdens, leading to suboptimal performance. The key challenges include:
High Complexity of Scheduling Algorithms. Many scheduling algorithms focus on global
optimization to maximize resource utilization. However, in practice, these approaches introduce
significant computational and communication overhead. Factors such as inter-node communication
delays and data locality must be accounted for, increasing algorithmic complexity and slowing
down decision-making.
Limited Real-Time Adaptability. LLM inference requires low-latency, real-time scheduling,
but many traditional systems rely on periodic scheduling methods that struggle to adapt to sudden
J. ACM, Vol. 0, No. 0, Article 0. Publication date: 2025.


---

Cloud Native System for LLM Inference Serving
0:5
workload fluctuations. This lag in response time can degrade service quality, particularly in high-
demand scenarios where immediate resource adjustments are necessary.
Coarse-Grained Model Management. Cloud-based LLM inference often suffers from coarse-
grained resource allocation, where entire models are treated as monolithic units. This approach
results in high computational and storage costs, making model loading, migration, and replication
inefficient. As a result, during peak periods, the system struggles to dynamically reallocate resources
in response to changing demand, leading to service delays and underutilized infrastructure.
These limitations highlight the need for more flexible, fine-grained scheduling and orchestration
strategies that can dynamically respond to workload changes while maintaining efficient resource
utilization.
2.3
Imbalanced model deployment
In large-scale deployments, efficient resource allocation is essential for ensuring optimal perfor-
mance and cost-effectiveness. However, uneven distribution of computational resources often
leads to inefficiencies, where some nodes experience excessive workloads while others remain
underutilized. Key challenges include:
Insufficient Data Parallelism. LLM inference often relies on data parallelism to distribute
workloads across multiple computing units. However, this distribution is frequently uneven due to
factors such as data bias and workload imbalance. Some nodes may process more data or perform
more complex computations than others, creating bottlenecks while leaving other resources idle.
Uneven Computational Demand Across Model Components. Different layers of LLM archi-
tectures—such as those in Transformer models—have varying computational requirements. Current
deployment strategies do not effectively account for these differences, leading to disproportionate
workloads on certain nodes, which limits overall system efficiency.
Suboptimal Hardware Resource Utilization. LLM inference typically runs on a mix of
hardware resources, including GPUs and TPUs, each with varying computational power, memory
bandwidth, and network capabilities. Existing deployment strategies often fail to consider these
hardware discrepancies, leading to overloaded nodes and underutilized resources. Additionally,
most deployments rely on static, pre-configured resource allocations, reserving excess capacity for
peak demand periods. This approach results in low resource utilization during off-peak times and
limits the system’s ability to adapt to traffic fluctuations.
Addressing these challenges requires adaptive, real-time resource allocation strategies that lever-
age dynamic monitoring and predictive scaling to balance workloads effectively. By enabling
more flexible and intelligent deployment mechanisms, cloud-based LLM inference services can
significantly improve efficiency, reduce costs, and enhance scalability.
3
Cloud Native For LLM
Based on the identified challenges, we propose a solution that leverages cloud native technology
for LLM applications. A cloud native system is an architectural paradigm designed to optimize
and enhance the deployment, management, and scalability of large-scale applications through
techniques such as containerization, microservices, and dynamic scheduling. In traditional cloud
computing architectures, applications tend to be monolithic with static resource allocation and
management. In contrast, cloud native approaches advocate decomposing applications into small,
independent modules (also called microservices) that can operate, be deployed, and scale indepen-
dently [7]. This approach offers flexible scalability, automated resource management, and efficient
deployment—attributes that are particularly advantageous for running LLMs efficiently.
A comprehensive review of both domestic and international literature reveals that existing LLM
inference services have yet to fully exploit the potential of cloud native systems, lacking a precise,
J. ACM, Vol. 0, No. 0, Article 0. Publication date: 2025.


---

0:6
Xu et al.
Fig. 2. Cloud Native System Support Framework for LLM Inference Serving.
fine-grained, and dynamic prototype framework. To overcome the performance optimization
challenges encountered in current LLM inference services, we suggest integrating the efficient
orchestration and resource management strategies [16] inherent in cloud native systems into
task profiling, module expansion, and model deployment, thereby constructing a cloud native
based inference service platform. Figure 2 illustrates the proposed cloud native LLM inference
service architecture, systematically organized as a multi-tiered framework to address the limitations
identified in existing approaches. The top tier consists of various LLM inference workloads that
will be transmitted to the cloud native platform from different LLMs. The third and fourth tiers
depict the AI infrastructure used to build the cloud native platform (e.g., public and private clouds)
and the types of hardware it employs (e.g., GPU, TPU and NPU). The second tier presents the key
components within the cloud native platform to support LLMs inference serving, which can be
broadly categorized into six modules as below:
Load balancing is one of the critical factors for enhancing the performance of LLM inference
services. In distributed systems, the load associated with inference tasks is inherently dynamic,
particularly under high request volumes or when input data varies significantly, leading to potential
load imbalances. Without an effective load balancing mechanism, some nodes may become overbur-
dened while others remain underutilized, resulting in resource wastage and increased response time.
Intelligent load balancing in cloud native environments dynamically adjusts task allocation based
on real-time resource usage metrics—such as computing power, memory, and bandwidth—across
nodes. For instance, Kubernetes-based [4] service meshes (e.g., Istio) can automatically redistribute
traffic among service instances in response to real-time demand, ensuring that each request is
directed to a node with lower load, thereby mitigating bottlenecks.
Autoscaling represents another key technology for optimizing LLM inference services. In cloud
native settings, it is crucial for the inference service to dynamically adjust computing resources in
J. ACM, Vol. 0, No. 0, Article 0. Publication date: 2025.


---

Cloud Native System for LLM Inference Serving
0:7
response to fluctuating loads, especially during peak periods. Autoscaling mechanisms automatically
increase or decrease resource capacity based on real-time load data, ensuring optimal resource
utilization and high system availability. For example, Kubernetes’ Horizontal Pod Autoscaler (HPA)
adjusts the number of Pods (independent microservices) based on CPU or memory usage; it scales
out the number of instances when faced with a surge in inference requests, and scales them back
when the load diminishes, thus balancing performance and cost-efficiency.
Migration technology also plays a vital role in optimizing the performance of LLM inference
services, particularly under conditions of resource demand imbalance. This technology facilitates
the transfer of tasks across different nodes, ensuring better resource utilization and preventing
overload on individual nodes. In high-performance GPU environments, for example, LLM inference
tasks may overload certain GPUs, thereby reducing execution efficiency. Through transparent
migration enabled by cloud native systems, tasks can be relocated to less burdened nodes or GPUs,
achieving more efficient resource use and lower latency without manual intervention.
Accurate load prediction is another essential strategy for optimizing LLM inference perfor-
mance. By forecasting future load conditions through the analysis of historical data with machine
learning algorithms, systems can preemptively allocate resources and schedule tasks. For instance,
integrating time series prediction models enables the anticipation of high-load periods, allowing
proactive resource scaling and adjustments. This approach not only prevents service interrup-
tions due to insufficient resources but also minimizes the waste associated with over-provisioning,
thereby optimizing resource allocation.
Application profiling is indispensable for performance optimization in LLM inference services.
In a cloud native framework, application profiling involves comprehensive monitoring and modeling
of an application’s performance, resource requirements, and behavioral patterns, which helps in
understanding the specific demands of each inference request. By collecting detailed operational
data—such as processing latency, resource usage, and failure rates—profiling provides a robust basis
for decisions regarding load balancing, autoscaling, and migration.
Fine-grained modularization. Furthermore, the modular design inherent in cloud native
systems facilitates more granular monitoring and control, making it feasible to optimize individual
components of the model. Unlike traditional monolithic deployments, where the entire model
inference pipeline is treated as a single unit, fine-grained modularization decomposes the inference
process into multiple independent functional components. This approach enables selective opti-
mization, independent scaling, and precise fault isolation, ultimately improving resource utilization
and system resilience.
Through these techniques, cloud native systems can significantly improve service efficiency by
enhancing resource utilization, reducing operating costs, and boosting both model performance
and stability. Ultimately, cloud native not only provides architectural flexibility and scalability but
also offers robust support for the efficient operation of LLM applications in large-scale computing
environments.
4
LLM Serving Performance Improvement with Cloud Native
In this section, we introduce our experimental framework and findings aimed at evaluating the
performance enhancements achieved by applying cloud native techniques to LLM serving. Our
objective is to demonstrate that decomposing LLMs into microservices and employing dynamic
autoscaling can effectively mitigate bottlenecks in Transformer layers, thereby reducing inference
latency and increasing overall throughput.
J. ACM, Vol. 0, No. 0, Article 0. Publication date: 2025.


---

0:8
Xu et al.
Fig. 3. Maximum Inference Latency at Different Transformer Layers.
4.1
Experimental Settings
Traditional deployments treat the entire model as a monolithic unit with uniform resource allocation.
In contrast, cloud native systems permit granular decomposition, such as isolating each Transformer
layer of an LLM as an independent microservice for targeted optimization. To validate this approach,
we conducted a series of experiments focusing on load bottlenecks and the impact of autoscaling
on a bottleneck Transformer layer.
Our experimental environment comprised a Kubernetes cluster with three nodes, each equipped
with an NVIDIA A100-80GB GPU interconnected via NVLink. We employed the LLaMA-2-13B
model, decomposing each of its Transformer layers into separate microservices. Inter-service
communication was managed via gRPC1 and orchestrated using the Istio2 service mesh. Monitoring
was performed with Prometheus3 and Grafana4, which collected data on GPU utilization, memory
bandwidth, and per-layer forward propagation times at a 100ms sampling interval. To simulate
diverse load conditions, Locust5 toolkit generated requests with input lengths varying from 50 to
2048 tokens.
Based on the latency distribution, we identified a specific Transformer layer with a pronounced
right-skewed latency profile as the primary bottleneck. To assess the effectiveness of cloud native
autoscaling (CN autoscaling), we first established a baseline without autoscaling (w/o autoscaling)
which disables the Kubernetes HPA, recording key performance metrics such as average latency,
peak latency, and latency jitter under varied workloads. We then deployed HPA for the microservice
hosting the bottleneck layer by setting target GPU utilization and custom latency thresholds to
trigger horizontal scaling.
4.2
Observations and Results
The experimental results yielded several notable observations:
Bottlenecks Identification: Figure 3 displays the maximum inference latency observed across 40
Transformer layers, highlighting distinct performance bottlenecks—particularly in layers exhibiting
1https://grpc.io/
2https://istio.io/
3https://prometheus.io/
4https://grafana.com/
5https://locust.io/
J. ACM, Vol. 0, No. 0, Article 0. Publication date: 2025.


---

Cloud Native System for LLM Inference Serving
0:9
(a) Inference latency comparison.
(b) Throughput comparison.
Fig. 4. Performance Improvement with Autoscaling in Cloud Native.
greater computational or memory overhead. Our analysis of 40 Transformer layers revealed that
several layers, especially Layer 27, whose maximum latency was more than 230 times that of
Layer 30, exhibited significant latency jitter and extreme peak latencies under high load. While
low-load conditions produced relatively uniform latencies across layers, increased concurrency led
to dramatic latency surges in bottleneck layers, indicating higher susceptibility to saturation from
computational, memory, or scheduling constraints.
Autoscaling Effects: Figure 4 presents a comparison of inference throughput and latency under
different batch size settings between the default w/o autoscaling and CN autoscaling. Comparative
tests before and after enabling CN autoscaling demonstrated that CN autoscaling substantially
improved performance. As shown in Fig. 4(a), with a batch size of 62, the inference latency of the
targeted bottleneck layer decreased from 15.23 seconds to 12.28 seconds, with a notable reduction in
peak latency and mitigation of the long-tail effect as evidenced by a leftward shift in terms of latency.
As shown in Fig. 4(b), when the batch size was 62, under the same request load, the overall system
throughput increased from 4.07 queries per second (QPS) to 5.05 QPS. These improvements are
attributed to the dynamic load balancing achieved through horizontal scaling, which redistributed
the computational load across additional pods.
To summarize, our experiments confirm that decomposing LLMs into fine-grained microservices
and employing cloud native autoscaling via Kubernetes HPA effectively alleviates performance
bottlenecks in Transformer layers. This strategy not only reduces the inference latency of critical
layers but also enhances the overall throughput and stability of the LLM serving system. The
results underscore the potential of cloud native architectures for optimizing large language model
inference in dynamic and high-load environments.
5
Conclusions
In this paper, we demonstrated that a cloud native platform is highly effective in optimizing LLM
inference. By decomposing monolithic LLM architectures into fine-grained microservices, our ap-
proach enables targeted performance improvements through dynamic load balancing and resource
management. In particular, the application of cloud native autoscaling to identified bottleneck
modules significantly reduced inference latency and enhanced throughput. The experimental results
validate that cloud native techniques not only mitigate layer-specific performance bottlenecks
J. ACM, Vol. 0, No. 0, Article 0. Publication date: 2025.


---

0:10
Xu et al.
but also facilitate efficient resource utilization under varying load conditions, underscoring their
promise for LLM inference serving.
Acknowledgments
This work is supported by Guangdong Basic and Applied Basic Research Foundation (No. 2024A1515010251,
2023B1515130002), Guangdong Special Support Plan (No. 2021TQ06X990), and Shenzhen Basic
Research Program under grants JCYJ20220818101610023 and JCYJ20240809180935001.
References
[1] E. Almazrouei, H. Alobeidli, A. Alshamsi, A. Cappelli, R. Cojocaru, M. Debbah, E. Goffinet, D. Hesslow, J. Launay, Q.
Malartic, et al. 2023. The Falcon series of open language models. arXiv preprint arXiv:2311.16867 (2023).
[2] R. Anil, A. M. Dai, O. Firat, M. Johnson, D. Lepikhin, A. Passos, S. Shakeri, E. Taropa, P. Bailey, Z. Chen, and et al. 2023.
Palm 2 technical report. arXiv preprint arXiv:2305.10403 (2023).
[3] T. Brown, B. Mann, N. Ryder, M. Subbiah, J. D. Kaplan, P. Dhariwal, A. Neelakantan, P. Shyam, G. Sastry, A. Askell, et al.
2020. Language models are few-shot learners. Advances in Neural Information Processing Systems 33 (2020), 1877–1901.
[4] Brendan Burns, Brian Grant, David Oppenheimer, Eric Brewer, and John Wilkes. 2016. Borg, Omega, and Kubernetes.
Commun. ACM 59, 5 (April 2016), 50–57. doi:10.1145/2890784
[5] Z. Du, Y. Qian, X. Liu, M. Ding, J. Qiu, Z. Yang, and J. Tang. 2022. GLM: General language model pretraining with
autoregressive blank infilling. In Proceedings of the 60th Annual Meeting of the Association for Computational Linguistics
(ACL). 320–335.
[6] D. Guo, D. Yang, H. Zhang, J. Song, R. Zhang, R. Xu, Q. Zhu, S. Ma, P. Wang, X. Bi, and et al. 2025. Deepseek-r1:
Incentivizing reasoning capability in LLMs via reinforcement learning. arXiv preprint arXiv:2501.12948 (2025).
[7] Xiang He, Zhiying Tu, Markus Wagner, Xiaofei Xu, and Zhongjie Wang. 2023. Online Deployment Algorithms for
Microservice Systems With Complex Dependencies. IEEE Transactions on Cloud Computing 11, 2 (2023), 1746–1763.
doi:10.1109/TCC.2022.3161684
[8] A. Q. Jiang, A. Sablayrolles, A. Roux, A. Mensch, B. Savary, C. Bamford, D. S. Chaplot, D. d. l. Casas, E. B. Hanna, F.
Bressand, et al. 2024. Mixtral of experts. arXiv preprint arXiv:2401.04088 (2024).
[9] Woosuk Kwon, Zhuohan Li, Siyuan Zhuang, Ying Sheng, Lianmin Zheng, Cody Hao Yu, Joseph Gonzalez, Hao Zhang,
and Ion Stoica. 2023. Efficient Memory Management for Large Language Model Serving with PagedAttention. In
Proceedings of the 29th Symposium on Operating Systems Principles (Koblenz, Germany) (SOSP ’23). Association for
Computing Machinery, New York, NY, USA, 611–626. doi:10.1145/3600006.3613165
[10] D. Li, R. Shao, A. Xie, Y. Sheng, L. Zheng, J. Gonzalez, I. Stoica, X. Ma, and H. Zhang. 2023. How long can context
length of open-source LLMs truly promise?. In Proceedings of the NeurIPS 2023 Workshop on Instruction Tuning and
Instruction Following.
[11] Biao Sun, Ziming Huang, Hanyu Zhao, Wencong Xiao, Xinyi Zhang, Yong Li, and Wei Lin. 2024. Llumnix: dynamic
scheduling for large language model serving. In Proceedings of the 18th USENIX Conference on Operating Systems Design
and Implementation (Santa Clara, CA, USA) (OSDI’24). USENIX Association, USA, Article 10, 19 pages.
[12] H. Touvron, T. Lavril, G. Izacard, X. Martinet, M.-A. Lachaux, T. Lacroix, B. Rozière, N. Goyal, E. Hambro, F. Azhar,
et al. 2023. Llama: Open and efficient foundation language models. arXiv preprint arXiv:2302.13971 (2023).
[13] B. Workshop, T. L. Scao, A. Fan, C. Akiki, E. Pavlick, S. Ilić, D. Hesslow, R. Castagné, A. S. Luccioni, F. Yvon, et al. 2022.
Bloom: A 176B-parameter open-access multilingual language model. arXiv preprint arXiv:2211.05100 (2022).
[14] Gyeong-In Yu, Joo Seong Jeong, Geon-Woo Kim, Soojeong Kim, and Byung-Gon Chun. 2022. Orca: A Distributed
Serving System for Transformer-Based Generative Models. In 16th USENIX Symposium on Operating Systems Design
and Implementation (OSDI 22). USENIX Association, Carlsbad, CA, 521–538. https://www.usenix.org/conference/
osdi22/presentation/yu
[15] Yinmin Zhong, Shengyu Liu, Junda Chen, Jianbo Hu, Yibo Zhu, Xuanzhe Liu, Xin Jin, and Hao Zhang. 2024. DistServe:
disaggregating prefill and decoding for goodput-optimized large language model serving. In Proceedings of the 18th
USENIX Conference on Operating Systems Design and Implementation (Santa Clara, CA, USA) (OSDI’24). USENIX
Association, USA, Article 11, 18 pages.
[16] Zhiheng Zhong, Minxian Xu, Maria Alejandra Rodriguez, Chengzhong Xu, and Rajkumar Buyya. 2022. Machine
Learning-based Orchestration of Containers: A Taxonomy and Future Directions. ACM Comput. Surv. 54, 10s, Article
217 (Sept. 2022), 35 pages. doi:10.1145/3510415
J. ACM, Vol. 0, No. 0, Article 0. Publication date: 2025.
