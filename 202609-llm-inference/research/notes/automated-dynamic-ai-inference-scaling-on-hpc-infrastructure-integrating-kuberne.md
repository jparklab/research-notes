---
title: 'Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating
  Kubernetes, Slurm and vLLM'
id: automated-dynamic-ai-inference-scaling-on-hpc-infrastructure-integrating-kuberne
tags:
- onprem-llm-inference-stacks-859099
- hpc
created: '2026-10-23T18:14:04.047460Z'
updated: '2026-10-23T18:17:22.313185Z'
source: https://arxiv.org/html/2511.21413v1
source_domain: arxiv.org
fetched_at: '2026-10-23T18:14:04.027019Z'
fetch_provider: crawl4ai
status: draft
type: note
tier: institutional
content_type: paper
deprecated: false
summary: 'Bochum/Cologne (Open Source-KI.nrw): K8s web gateway + Slurm + Apptainer
  + vLLM autoscaling by GPU load; benchmarks on 2xL40S and 1xH100; no production utilization
  stats.'
---

arXiv is now an independent nonprofit! [Learn more](https://info.arxiv.org/about) ×
[ License: CC BY-SA 4.0 ](https://info.arxiv.org/help/license/index.html#licenses-available)
arXiv:2511.21413v1 [cs.DC] 26 Nov 2025 

AI
    Artificial Intelligence 

BT
    Block Tridiagonal 

BWA
    Burrows-Wheeler Aligner 

CCD
    Core Chiplet Die 

CCX
    Core Complex 

CG
    Conjugate Gradient 

CPU
    Central Processing Unit 

CVM
    Confidential Virtual Machines 

DBMS
    Database Management System 

DNA
    Deoxyribonucleic Acid 

DPU
    Data Processing Unit 

DRAM
    Dynamic Random-Access Memory 

EP
    Embarrassingly Parallel 

EPC
    Enclave Page Cache 

EU
    European Union 

FLOPS
    Floating Point Operations Per Second 

FT
    Fast Fourier Transform 

GPFS
    General Parallel File System 

GPU
    Graphics Processing Unit 

HPC
    High-Performance Computing 

IoT
    Internet of Things 

IS
    Integer Sort 

JVM
    Java Virtual Machine 

KVC
    Key-Value Cache 

KVM
    Kernel-based Virtual Machine 

LLM
    Large Language Model 

LU
    LU Decomposition 

ML
    Machine Learning 

MG
    MultiGrid 

NGS
    Next-Generation Sequencing 

NPB
    NAS Parallel Benchmarks 

NUMA
    Non-Uniform Memory Access 

OS
    Operating System 

RNA
    Ribonucleic Acid 

SEV
    Secure Encrypted Virtualization 

SEV-ES
    Secure Encrypted Virtualization–Encrypted State 

SEV-SNP
    Secure Encrypted Virtualization–Secure Nested Paging 

SGX
    Software Guard Extensions 

SME
    Secure Memory Encryption 

SMP
    Symmetric Multiprocessing 

SNP
    Secure Nested Paging 

SP
    Scalar Pentadiagonal 

TCB
    Trusted Computing Base 

TD
    Trust Domain 

TDX
    Trust Domain Extensions 

TEE
    Trusted Execution Environment 

TPU
    Tensor Processing Unit 

UA
    Unstructured Adaptive 

VM
    Virtual Machine 

VPN
    Virtual Private Network
# Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM
Conference: Next-Gen Middleware for MLOps in Distributed Systems ; December 15–19, 2025; Nashville, TN, USA
Tim Trappen  Affiliation: Ruhr University Bochum, Bochum, Germany email: tim.trappen@ruhr-uni-bochum.de ,  Robert Keßler  Affiliation: University of Cologne, Cologne, Germany email: kessler@uni-koeln.de ,  Roland Pabel  Affiliation: University of Cologne, Cologne, Germany email: pabel@uni-koeln.de ,  Viktor Achter  Affiliation: University of Cologne, Cologne, Germany email: achter@uni-koeln.de and  Stefan Wesner  Affiliation: University of Cologne, Cologne, Germany email: wesner@uni-koeln.de
2025
###### Abstract.
Due to rising demands for Artificial Inteligence (AI) inference, especially in higher education, novel solutions utilising existing infrastructure are emerging. The utilisation of High-Performance Computing (HPC) has become a prevalent approach for the implementation of such solutions. However, the classical operating model of HPC does not adapt well to the requirements of synchronous, user-facing dynamic AI application workloads. In this paper, we propose our solution that serves LLMs by integrating vLLM, Slurm and Kubernetes on the supercomputer RAMSES. The initial benchmark indicates that the proposed architecture scales efficiently for 100, 500 and 1000 concurrent requests, incurring only an overhead of approximately 500 ms in terms of end-to-end latency.
###### Keywords: 
Inference, High-Performance Computing, Large Language Model 
##  1. Introduction
(), and in particular , have become an integral factor in numerous areas of our lives. For instance, the use of in an university setting involves a wide range of user groups, including students, researchers, and even the administration. Students benefit from personalized education pathways, are provided with dynamic learning content and feedback, and thus can self-regulate their individual progress (bernalRevolutionizingELearningAssessments2024; liuOptimizingELearningEnvironments2024). The use of chatbots within e-learning platforms, such as MoodleBot, provides students with a personal tutor who is available 24/7, thereby reducing the administrative workload for teachers (neumannLLMDrivenChatbotHigher2025; liuTeachingCS50AI2024). Scientists are already making use of the increasingly powerful for their research work in various domains such as drug discovery or partial differential equations, where the requests range from simple literature search and summary to concept clarification, data analysis and methodology guidance (ai4scienceImpactLargeLanguage2023). However, the potential of goes way beyond this, meaning that in the future they could serve as a kind of research assistant, as envisaged by the AuroraGPT 11 1 <https://www.anl.gov/cels/auroragpt-foundation-models-for-science> project (cappelloAuroraGPTExploringAI2024). Even the administrative offices are increasingly using AI tools, for example to support the enrollment process or to provide better assistance with scholarship applications (khairullahImplementingArtificialIntelligence2025).
For higher education institutions, compliance and data governance pose the greatest challenges when adopting to . This creates the need for independent and sovereign infrastructure which provides easy to use and privacy preservering access to models. Universities that operate their own data centers are predestined to offer such services themselves, as they tend to possess the necessary computing power and can also guarantee the data privacy aspects. The computing systems operated by higher education institutions are often () systems, also known as supercomputers, which are used by scientists from a wide range of scientific disciplines to solve highly complex research problems and perform simulations, but tend to be underutilized (10740905). However, the operating model of systems is based on static allocation of computing resources, which is suitable for training but is not an inherent match for processing dynamic inference requests (lopezAIFactoriesIts2025). To overcome this discrepancy, one can either (i) implement dynamic resource allocation in systems from scratch, which requires at least adjustments at the level of the resource manager and programming models (tarrafMalleabilityModernHPC2024), or (ii) implement a scalable web API, that handles the dynamic management of HPC jobs and forwards the inference requests accordingly. Due to the manifold modifications required for approach (i), this comprehensive implementation is very sophisticated, whereas approach (ii) offers an implementation using micro-service architecture, which can be deployed in a scalable manner using e.g. Kubernetes.
The remainder of this paper is structured as follows, in [section 2](https://arxiv.org/html/2511.21413v1#S2 "2. Related Work ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM") we present and discuss challenges and other approaches to solve the problem of serving dynamic inference workloads on infrastructure. In [section 3](https://arxiv.org/html/2511.21413v1#S3 "3. Architecture ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM") we describe our own approach, evaluate its performance in [section 4](https://arxiv.org/html/2511.21413v1#S4 "4. Performance Evaluation ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM") and discuss eventual threats to validity in [section 5](https://arxiv.org/html/2511.21413v1#S5 "5. Discussion ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM"). We conclude our paper with an outlook on the future work in [section 6](https://arxiv.org/html/2511.21413v1#S6 "6. Conclusion & Outlook ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM").
##  2. Related Work
As and converge, this is reflected, for example, in the massive investments of the () in so-called AI Factories22 <https://digital-strategy.ec.europa.eu/en/policies/ai-factories>, which are intended to drive forward the development of trustworthy generative models in Europe. But also on a smaller scale, existing infrastructures at higher education institutions are being used to run projects such as Open Source-KI.nrw 3. This poses a certain contradiction, as environments are usually based on dynamic cloud-native solutions, whereas systems are operated using batch processing and static resource allocation. Specifically, systems are not inherently designed to support interactive applications, which is why lopezAIFactoriesIts2025 propose to rely on a sovereign dual-stack architecture for future systems, in order to combine the best of both worlds (lopezAIFactoriesIts2025). systems have always been performance-sensitive, yet with the advent of AI workloads, it is becoming critical for cloud computing as well. In order to ensure sustainable performance of systems, especially in the exascale era, accelerators such as are essential, which in turn are also required for . The approach proposed by hoeflerXaaSAccelerationService2024, aims on the advatages of the -Cloud convergence, by providing a simple and fast access to accelerators with their Acceleration as a Service (XaaS) model (hoeflerXaaSAccelerationService2024). In turn, AI itself can be used in the HPC domain to efficiently generate high-performant parallel code, which subsequently improves not only conventional HPC applications but also the models themselves (chenLandscapeChallengesHPC2024).
This kind of convergence does also pose certain challenges, including not only performance but also sustainability and scalability factors. reschFutureHPCAI2025 therefore suggests that, in addition to the traditional TOP500 list of supercomputers, future evaluations should also focus specifically on workloads, emphasizing the time to deliver a solution or the complexity of finding a solution rather than pure [FLOPS](https://arxiv.org/html/2511.21413v1#id16) ([FLOPS](https://arxiv.org/html/2511.21413v1#id16)) (reschFutureHPCAI2025). In order to overcome the challenges associated with realizing -coupled workflows, the development of new middleware solutions is required to accommodate various execution motifs such as dynamic orchestration, multistage pipeline, and adaptive training (brewerAIcoupledHPCWorkflow2025).
To provide inference on a system, a variety of services must be operated and linked with each other, e.g., a user registry, service registry, and service identifier components, but also the support of scheduling and scaling functions. guranMiddlewareLargeLanguage2024 present a middleware solution that considers this and serves as a gateway, but is lacking support for environments (guranMiddlewareLargeLanguage2024). The FLEXI project at FernUniversiät in Hagen implements on-premise hosting of open-source in an higher-education environment but also lacks support for systems (zeschFlexibleLLMExperimental2024). This shortcoming has been addressed by luizScalableEnginePerformance2025, who have developed a solution that implements a scalable microservice-based master-slave architecture that serves heterogeneous models on infrastructure leveraging the Slurm workload manager (luizScalableEnginePerformance2025). A similar implementation is offered by Chat AI, which operates its chat front-end as a web service in the Cloud , and then forwards the corresponding requests via an API gateway to the respective model on the infrastructure for being processed (doosthosseiniChatAISeamless2024a).
Despite all these efforts, resource and workflow managers still show a lack of support for the openness, transparency, and reusability of such workflows on HPC systems, meaning that the inherent dynamicity in such systems needs to be further fostered (ejarqueEnablingDynamicIntelligent2022b). Furthermore, although attempts have been made to implement AI inference on HPC systems, there is a lack of systematic performance studies (brewerInferenceBenchmarkingHPC2020).
##  3. Architecture
Our approach separates various microservices into two layers, as illustrated in [Figure 1](https://arxiv.org/html/2511.21413v1#S3.F1 "In 3. Architecture ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM"). The first layer consists of (i) a Kubernetes-based web API that forwards inference requests from users to the corresponding models in the backend and manages the lifecycle of resources / models. The second layer consists of (ii) the models themselves which are encapsulated into Slurm jobs.
Figure 1. Visualization of the flow between layers for an incoming inference request.Describe the image
To orchestrate the containers required for the services of our web API, we use Kubernetes4 to automatically schedule, manage and scale the deployments. For the underlying container runtime we use Apptainer5, since it is specifically designed for environments. As resource manager for the allocation and deployment of the nodes running the endpoints, we use Slurm6, as this is the de facto standard in this domain.
Central to almost all of the microservices is a single relational PostgreSQL7 database running in Kubernetes. It holds two domains (): (a) authentication and (b) Slurm job management, as visualized in [Figure 2](https://arxiv.org/html/2511.21413v1#S3.F2 "In 3. Architecture ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM"). Authentication consists of a simple 1:N relation of identity_tenants and their API keys identity_tenant_ authentications, which are stored in an encrypted format. Slurm job management consists of multiple 1:N relations, which have proven to provide consistency in an actual production scenario.
Figure 2. Visualization of the central database schema.Describe the image
###  3.1. Inference Components
####  3.1.1. vLLM
Offering heterogeneous serving and inference in a high-throughput scenario requires a fully scalable inference engine, such as the open-source software vLLM88 8 <https://github.com/vllm-project/vllm>. One of its major advantages is the introduction of _PagedAttention_ , which splits the () into blocks and assigns them to logical pages allowing for dynamic scaling of allocated GPU memory (kwon2023efficient). This mapping is handled via block tables, managed by a central KV cache manager. In doing so, blocks may be reused in single- and shared across multiple requests, leading to significant performance gains in scenarios where single prompts consist of long inputs, or a large amount of requests need to be served simultaneously. This also allows for models with weight sizes exceeding the memory capacity of a single to run efficiently across multiple . vLLM implements an OpenAI-standard compatible FastAPI frontend for serving requests. If the number of requests received exceeds the system’s concurrent throughput capabilities, a first-come, first-served scheduling policy is employed for all incoming requests.
####  3.1.2. Web Gateway
The flow of a single request from a user’s application through the Web Gateway to the desired vLLM endpoint running inside a Slurm job is visualized in [Figure 1](https://arxiv.org/html/2511.21413v1#S3.F1 "In 3. Architecture ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM"). The system’s primary entry point for client applications is the Web Gateway, which is responsible for authentication and the subsequent forwarding of incoming requests. Its API endpoints are OpenAI-standard compatible, making them agnostic to the consuming client application. Request properties are strongly typed and validated, adding an additional layer of robustness. Authentication is handled via long-lived bearer-tokens, stored in an encrypted format in the database and authenticated on each request. To minimise database load, a distributed memory cache stores recently authenticated API keys for a limited period. Subsequent to the authentication and validation process (1), the Web Gateway conducts a search for available endpoints corresponding to the requested LLM within the ai_model_endpoints table (2). It then forwards the request - along with all request parameters - to a matching node (3). The response flows from vLLM back to the Web Gateway (4) and then to the requesting application (5). If no matching vLLM endpoint ready for inference is found, custom HTTP status codes are returned.
###  3.2. Management Components
[Figure 3](https://arxiv.org/html/2511.21413v1#S3.F3 "In 3.2. Management Components ‣ 3. Architecture ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM") visualizes the flow between microservices and layers described in the following subsections, which are ordered in the way the components interact with each other, starting from the Job Worker.
Figure 3. Visualization of the flow between layer for Slurm and vLLM management.Describe the image
####  3.2.1. Job Worker
The Job Worker functions as an intermediary between our Kubernetes-based microservices and the Slurm-managed infrastructure. It is implemented as a long-running background process, responsible for scheduling new vLLM instances via Slurm and removing expired jobs. Each run, it compares the entries in the ai_model_endpoint_jobs table with the configurations specified in the ai_model_configurations table, for example number of requested model instances. If no matching endpoint job is found, configuration parameters are passed to the [3.2.2. Slurm Submit](https://arxiv.org/html/2511.21413v1#S3.SS2.SSS2 "In 3.2. Management Components ‣ 3. Architecture ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM"), which requests ressourcess for a job with the corresponding model. On success, an ai_model_endpoint_jobs entry is created, containing information such as the Slurm job id, timestamps for job submission, job registration and readiness of the contained vLLM server. To prevent simultaneous submission and subsequent startup of jobs, which could lead to inconsistent port mappings on compute-nodes, model configurations are iterated synchronously. The Job Worker waits for a specified timespan after a successful submit before proceeding to the next configuration.
####  3.2.2. Slurm Submit
Slurm Submit is the service handling communication between the [3.2.1. Job Worker](https://arxiv.org/html/2511.21413v1#S3.SS2.SSS1 "In 3.2. Management Components ‣ 3. Architecture ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM") and Slurm. It accepts a string from an SSH connection, which is forwarded to a bash script that actually deploys the job. A dedicated munged process inside the container provides authentication for the Slurm components. The comma-delimited parameters from the string are parsed and used to select a model-specific .slurm file from a folder mounted into the container, that in turn is used to run Slurm’s sbatch. The file holds `#SBATCH` directives specifying hardware requirements of the requested node(s), among others. In addition, it sets environment variables for the vLLM container and configures a curl request to the [3.2.3. Endpoint Gateway](https://arxiv.org/html/2511.21413v1#S3.SS2.SSS3 "In 3.2. Management Components ‣ 3. Architecture ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM"). For multi-node multi-GPU setups, required for very large models, it also contains logic for setting up head and worker nodes.
####  3.2.3. Endpoint Gateway
Upon successful submission of a Slurm job, a curl `POST` is initiated from the Slurm script to the API of the Endpoint Gateway. It contains the endpoint job id, Slurm job id, node id, model version, capabilities and bearer token for the vLLM server that is being spawned. Authentication with the Endpoint Gateway is facilitated by a bearer token that is passed to the script. The underlying business logic proceeds to check if the internal job id holds a corresponding job in ai_model_endpoint_jobs that has no endpoint attached. If that is the case, it compares the ports of all already existing endpoints on the supplied node and assigns a port p=a​r​g​m​a​x​(p​o​r​t)+1p=argmax(port)+1, which is passed back to the Slurm script in the curl response to be used by the vLLM server. Finally, a new entry is created in ai_model_endpoints, holding information such as node, port, model version, bearer token and a nullable datetime field indicating readiness of the endpoint, which at that time is null.
####  3.2.4. Endpoint Worker
The Endpoint Worker is another long-running background service, responsible for managing endpoint health status. Each run, it iterates over all entries in the ai_model _endpoint_jobs table and sends a `GET` request to the `/health` endpoint of each job, which is provided by the vLLM server. Endpoint jobs not yet marked as ready but receive a response status-code `200` are updated accordingly by setting their corresponding fields to the current datetime. Once marked ready, the endpoint will be considered for forwarding requests by the [3.1.2. Web Gateway](https://arxiv.org/html/2511.21413v1#S3.SS1.SSS2 "In 3.1. Inference Components ‣ 3. Architecture ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM"). In cases where no response is returned, the Endpoint Worker differentiates two cases: (1) jobs that have been canceled or expired and (2) jobs that are still starting up. Since loading model weights can require a substantial amount of time, a configurable 30-minute timeout is currently implemented before a job is deemed cancelled or expired. Moving forward, this could be dynamically configured with an additional estimated load time on a per-model basis via the ai_model_configurations table. For cancelled or expired jobs, the Endpoint Worker removes the corresponding entries in ai_model_endpoints and ai_model_endpoint_jobs.
####  3.2.5. Metrics Gateway
The Metrics Gateway serves API endpoints for Prometheus, a time-series database, and Grafana, an open-source observability platform. The Prometheus endpoint returns a response with the required format for Prometheus’ HTTP service discovery, which allows for dynamic (de-)registration of targets for metrics scraping. By accessing the ai_model_endpoints table, it converts necessary information like node id, port and bearer token of all running vLLM instances to the specified Prometheus template and allows for additional meta fields to be added, such as job id and Slurm job id. While metrics of our Kubernetes based microservices can be conveniently scraped with Prometheus Operator, obtaining metrics from the vLLM instances requires this workaround, as they are not part of the Kubernetes cluster and may change network adresses over time. The Grafana endpoints, in contrast, accept various `POST` request parameters, which are used to control the amount and type of running model instances.
###  3.3. Observability & Automated Dynamic Scaling
In addition to the fundamental inference and management components, an observability stack is utilized, comprising Prometheus, Grafana and Grafana Loki to facilitate visualisation, reporting and logging. This allows for real-time monitoring of critical metrics, such as the vLLM instance load or concurrent [3.1.2. Web Gateway](https://arxiv.org/html/2511.21413v1#S3.SS1.SSS2 "In 3.1. Inference Components ‣ 3. Architecture ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM") requests, hence providing insight into how frequently certain models are accessed. In combination with live logging, it provides the capability to efficiently identify and resolve issues almost instantaneously.
This stack also comprises the central part of the mechanism to dynamically scale up or down vLLM model instances inside Slurm jobs, based on GPU load. Utilizing Grafanas alert rules with its contact point webhook notifications, custom JSON payloads are automatically sent to the metrics gateway. Here, the business logic adjusts the number of model instances specified in the ai_model_job _configurations table. This triggers the [3.2.1. Job Worker](https://arxiv.org/html/2511.21413v1#S3.SS2.SSS1 "In 3.2. Management Components ‣ 3. Architecture ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM") to automatically start the newly requested number of instances on its next invocation, currently every 15 seconds. Hence, the system scales by actual hardware load, since vLLM reports metrics like KVC utilization, queue times and token throughput. In our first tests, we utilized the vLLM queue time metric, where a queue time above 5 seconds over 30 sustained seconds triggered instantiation of an additional model instance (see subsection 4.2 for further explanation). Compared to scaling by number of requests, which in the context of is an arbitrary metric on its own given the lack of uniformity of the requests (BurstGPT), for example input token count or hyperparameters like maximum generated tokens, this setup allows for maximizing GPU load and thus token throughput. In our first tests, we utilized the vLLM queue time metric, where a queue time above 5 seconds over 30 sustained seconds triggers instantiation of an additional model instance (c.f. subsection 4.2).
##  4. Performance Evaluation
###  4.1. Methodology
In this section, we provide preliminary performance results for our system via the serve-benchmark in vLLM using the _BurstGPT_without_fails_2_ dataset (BurstGPT).
We benchmark two system configurations: (1) _GPU-S_ , which utilizes two NVIDIA L40S GPUs with an AMD EPYC 9654 CPU, and (2) _GPU-L_ , which uses one NVIDIA H100 GPU and an AMD EPYC 9454 CPU. All configurations utilize 96GB RAM. Networking is based on Mellanox Infiniband HDR100 (100GBit/s), with two ports for redundancy on Kubernetes nodes and one port on compute nodes. The topology consists of 8 leaf-switches linked to 2 spine-switches via 5 HDR (200GBit/s) links each, resulting in a bisection bandwidth of 8000 Gb/s. The maximum blocking factor is 2.3:1.
For our baseline model, we use Mistral Small 3.2 24B Instruct 250699 9 [Mistral Small 3.2 24B Instruct 2506 on HuggingFace](https://huggingface.co/mistralai/Mistral-Small-3.2-24B-Instruct-2506) with default parameters. The load is simulated and averaged over 50 runs for three scenarios: 100, 500 and 1000 concurrent requests. All runs are performed from a dedicated interactive node inside the HPC cluster, using the same vLLM container image running inside the Slurm jobs used for inference serving (v0.10.2). The seed is set to 0, to ensure that every benchmark run uses the same samples from the dataset. The vLLM node is not restarted or modified between runs; however, between each run, the KVC is fully cleared. All requests are handled using streaming.
In addition to the baseline benchmark, the system is also benchmarked including the Web Gateway. Before starting a full run, the vLLM serve-benchmark sends one initial request to the specified target, which triggers the caching of the authentication in the Web Gateway, causing it to only perform one database trip for each incoming request to look up the vLLM node endpoint.
Table 1. GPU-S and GPU-L configuration performance benchmarks for concurrent vLLM and Web Gateway requests.  
| Configuration  | GPU-S  | GPU-L  |  
| --- | --- | --- |  
| Benchmark  | vLLM Node  | Web Gateway  | vLLM Node  | Web Gateway  |  
| Concurrent Requests  | 1000  | 1000  | 1000  | 1000  |  
| E2EL Median (ms)  | 2307.96  | 7381.58  | 22385.04  | 2186.39  | 6331.33  | 22963.34  | 1195.80  | 3029.86  | 6190.10  | 1149.93  | 3076.11  | 9419.23  |  
| E2EL Std (ms)  | 5215.32  | 9316.33  | 20958.25  | 5151.45  | 9148.52  | 19999.29  | 2940.16  | 4984.30  | 8206.37  | 2981.23  | 4994.50  | 8465.61  |  
| Requests Total Duration (s)  | 30.01  | 48.02  | 84.76  | 30.03  | 46.95  | 79.75  | 17.20  | 25.94  | 34.81  | 17.31  | 26.28  | 37.25  |  
| Requests Total Input Tokens  | 77561.00  | 381456.00  | 768960.00  | 77561.00  | 381456.00  | 768960.00  | 77561.00  | 381456.00  | 768960.00  | 77561.00  | 381456.00  | 768960.00  |  
| Requests Total Output Tokens  | 7048.98  | 49763.68  | 141407.84  | 6976.34  | 49567.64  | 141313.34  | 6978.78  | 50126.12  | 142778.08  | 7027.58  | 49955.58  | 142664.64  |  
| TPOT Median (ms)  | 65.52  | 90.61  | 102.13  | 63.33  | 79.20  | 94.97  | 33.41  | 67.21  | 86.74  | 32.49  | 42.20  | 49.05  |  
| TPOT Std (ms)  | 6.36  | 13.07  | 18.00  | 5.52  | 6.92  | 10.50  | 2.34  | 12.19  | 29.23  | 2.08  | 3.16  | 5.58  |  
| TTFT Median (ms)  | 404.38  | 1849.79  | 8328.36  | 334.79  | 2412.93  | 6825.14  | 225.58  | 993.91  | 2190.97  | 207.35  | 1133.02  | 3109.98  |  
| TTFT Std (ms)  | 37.59  | 1477.46  | 12460.81  | 146.29  | 1950.86  | 13191.81  | 14.34  | 87.98  | 189.22  | 62.97  | 841.35  | 3642.25  |  
| Throughput Requests (req/s)  | 3.33  | 10.54  | 11.83  | 3.33  | 10.77  | 12.56  | 5.82  | 19.52  | 28.87  | 5.79  | 19.31  | 26.95  |  
| Throughput Token Output (tok/s)  | 234.91  | 1048.13  | 1672.62  | 232.51  | 1066.95  | 1775.28  | 405.79  | 1956.22  | 4121.70  | 406.30  | 1928.61  | 3844.97  |  
| Throughput Token Total (tok/s)  | 2820.34  | 9088.70  | 10768.98  | 2818.61  | 9282.65  | 11435.67  | 4916.07  | 16849.58  | 26322.19  | 4893.63  | 16664.20  | 24569.65  |  
###  4.2. Results
Results for the GPU-S and GPU-L configurations are shown in [Table 1](https://arxiv.org/html/2511.21413v1#S4.T1 "Table 1 ‣ 4.1. Methodology ‣ 4. Performance Evaluation ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM"). The vLLM benchmark suite defines time to first token (TTFT) as a timespan between sending the request and receiving the corresponding first token. Similarly, end to end latency (E2EL) is a timespan between sending the request and receiving the corresponding last token. Therefore time per output token (TPOT) is defined as:  
| 𝚝𝚙𝚘𝚝=𝚎𝟸𝚎𝚕−𝚝𝚝𝚏𝚝𝚘𝚞𝚝𝚙𝚞𝚝​_​𝚕𝚎𝚗−1\mathtt{tpot}=\frac{\mathtt{e2el}-\mathtt{ttft}}{\mathtt{output\\_len}-1}  |  
| --- |  
Unintuitively, for GPU-S, total request duration and TPOT favors the Web Gateway setup, albeit with slightly increased E2EL in 100 and 1000 concurrent request scenarios. The 500 concurrent request scenario stands out particularly for TTFT, where sending requests to vLLM directly is noticeably faster by about 500ms (23.34%23.34\%). GPU-L, in contrast, behaves more as expected, displaying slightly higher total request durations across all Web Gateway scenarios, with a higher E2EL and TTFT from 500 concurrent requests upward. Curiously, TPOT is significantly lower in these cases (37.21%37.21\% / 43.45%43.45\%). The reasons for this will need to be investigated in a more detailed system analysis in the future. Inspecting the vLLM logs, the GPU-S configuration starts queuing requests from 255 concurrent requests onward, while the GPU-L configuration does not appear to queue requests in any of our scenarios. While this could be an indicator for queueing impacting general performance, especially when done across multiple GPUs, it also leads to the assumption that once queueing has started, the slight latency introduced by the Web Gateway could buffer the impact of load put on the vLLM api server, ‘masking’ the queuing process. When compared to GPU-L (where no queueing occurs), we observe higher E2EL and TTFT as expected. However, at 500 and 1000 concurrent requests, MD and Std for TTFT deviates massively for the Web Gateway when compared to direct vLLM node access, with a nearly 1 second (41.95%41.95\%) slower median TTFT.
This suggests that our current implementation of routing requests via the Web Gateway starts to bottleneck in scenarios where enough GPU compute is present to handle the incoming load.
##  5. Discussion
The presented architecture is still considered a work in progress, and this paper does not claim to represent best practices. Its purpose is to provide a starting point for other infrastructure projects, highlighting where the chosen implementation has succeeded, where it faced problems, and where it currently falls short.
From early friendly user tests with small groups that use various applications to access our [3.1.2. Web Gateway](https://arxiv.org/html/2511.21413v1#S3.SS1.SSS2 "In 3.1. Inference Components ‣ 3. Architecture ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM"), such as KI:Connect1010 10 <https://kiconnect.pages.rwth-aachen.de/pages/> or Open WebUI11, we are confident that the foundation of our architecture - scaling LLM serving for synchronous applications in an HPC environment - is sound. Management of vLLM instances in Slurm jobs has run successfully in this small-scale production setting for multiple months, across a variety of tested models, covering single-node single-GPU, single-node multi-GPU, and even multi-node multi-GPU configurations.
Our benchmarks show that when scaling beyond a 1000 concurrent request scenario, improvements must be made to our benchmark setup and/or the [3.1.2. Web Gateway](https://arxiv.org/html/2511.21413v1#S3.SS1.SSS2 "In 3.1. Inference Components ‣ 3. Architecture ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM"). The increase in TTFT degrades end-user experience, signaling a longer wait time between the user sending a prompt and the model starting to return a response. However, TPOT remains consistent, which translates to smooth streaming and therefore ‘typing’-animation for chatbot applications. We identify several key areas that require further investigation to solve the arising bottleneck:
Networking: since we run the vLLM benchmark suite from within a compute-node of the HPC system, high concurrency scenarios might be impacted by networking components, favoring a direct request from compute-node to compute-node. Alternatively, improper request handling in the [3.1.2. Web Gateway](https://arxiv.org/html/2511.21413v1#S3.SS1.SSS2 "In 3.1. Inference Components ‣ 3. Architecture ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM") may contribute.
Caching: as every request performs a database lookup for a matching model endpoint, implementing a caching mechanism could reduce database load and speed up request handling.
Scaling: horizontal or vertical scaling of either the [3.1.2. Web Gateway](https://arxiv.org/html/2511.21413v1#S3.SS1.SSS2 "In 3.1. Inference Components ‣ 3. Architecture ‣ Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM") or PostgreSQL via Kubernetes could improve performance.
Despite these necessary improvements, institutional provisioning of on sovereign infrastructure provides clear data privacy benefits, especially when handling sensitive data. However, preventing -based attack vectors like prompt injections requires trained experts and domain specific knowledge, which must be seen as additional cost for said privacy benefits.
##  6. Conclusion & Outlook
In this work, we presented our approach to building a production-ready, modular architecture for highly scalable serving of LLM inference in an HPC context. By leveraging HPC-native components like Slurm and Apptainer, managed by custom tailored microservices and integrating them with cloud-native solutions for orchestration and observability like Kubernetes, Prometheus and Grafana, we implemented a robust solution to deploy and scale LLMs using the open-source inference engine vLLM. In addition, we shared performance benchmarks for two distinct compute-node hardware configurations under different load scenarios, showcasing the viability of HPC infrastructure for synchronous workloads like web requests from AI chatbot applications. As our architecture represents a work in progress, our next steps will be directed toward a high-troughput production scenario in a higher education setting. Given the scarcity of usage data for this application, we hope to attain a better understanding of how far sovereign infrastructure for AI applications needs to scale. We will also explore the deployment of other generative ai modalities. With growing concerns about the sustainability of AI, ensuring efficient usage of compute resources will take high priority. Thus, further work on leveraging components like Slurm to balance compute during peak usage times of AI inference while allocating the unused ressources to research computations during off-hours should take priority. Furthermore, exploring hardware configurations from vendors other than NVIDIA seems advisable to prevent a vendor lock-in for datacenters.
###### Acknowledgements.
The work presented in this paper is the result of the joint project by Ruhr-University Bochum and University of Cologne: ‘Open Source-KI.nrw’. The project is fully funded by the Ministry of Culture and Science of the State of North Rhine-Westphalia. 
## References
  *[AI]: Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM
  *[LLM]: Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM
  *[HPC]: Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM
  *[EU]: Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM
  *[FLOPS]: Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM
  *[KVC]: Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM
  *[GPU]: Automated Dynamic AI Inference Scaling on HPC-Infrastructure: Integrating Kubernetes, Slurm and vLLM
