---
title: 'FIRST: Federated Inference Resource Scheduling Toolkit for'
id: first-federated-inference-resource-scheduling-toolkit-for-2
tags:
- onprem-llm-inference-stacks-859099
- slurm-hpc
- hpc-llm-serving
- derivative-of
created: '2026-10-23T17:22:46.997239Z'
updated: '2026-10-23T17:29:00.276733Z'
source: https://www.arxiv.org/pdf/2510.13724
source_domain: www.arxiv.org
fetched_at: '2026-10-23T17:22:46.996944Z'
fetch_provider: crawl4ai
status: deprecated
type: note
tier: institutional
content_type: paper
deprecated: false
summary: 'FIRST (Argonne): Globus Auth/Compute + OpenAI-compatible gateway federating
  vLLM across HPC clusters, hot nodes, batch+interactive. Sophia 24-node cluster:
  8.7M requests, 76 users, >10B tokens in 10 months.'
raw_file: raw/first-federated-inference-resource-scheduling-toolkit-for-2.pdf
doi: arXiv:2510.13724
citation_count: 10
venue: 'SC25-W: Workshops of the International Conference for High Performance Computing,
  Networking, Storage and Analysis'
is_retracted: false
---

*Suggested by [[experience-deploying-containerized-genai-services]] — HPC LLM serving lead*

FIRST: Federated Inference Resource Scheduling Toolkit for
Scientific AI Model Access
Aditya Tanikanti1, Benoit Côté1, Yanfei Guo1, Le Chen1, Nickolaus Saint2, Ryan Chard1, Ken
Raffenetti1, Rajeev Thakur1, Thomas Uram1, Ian Foster1,2, Michael E. Papka1,3, Venkatram
Vishwanath1
1Argonne National Laboratory, Lemont, IL, USA
2The University of Chicago, Chicago, IL, USA
3University of Illinois Chicago, Chicago, IL, USA
Abstract
We present the Federated Inference Resource Scheduling Toolkit
(FIRST), a framework enabling Inference-as-a-Service across dis-
tributed High-Performance Computing (HPC) clusters. FIRST pro-
vides cloud-like access to diverse AI models, like Large Language
Models (LLMs), on existing HPC infrastructure. Leveraging Globus
Auth and Globus Compute, the system allows researchers to run par-
allel inference workloads via an OpenAI-compliant API on private,
secure environments. This cluster-agnostic API allows requests to
be distributed across federated clusters, targeting numerous hosted
models. FIRST supports multiple inference backends (e.g., vLLM),
auto-scales resources, maintains "hot" nodes for low-latency ex-
ecution, and offers both high-throughput batch and interactive
modes. The framework addresses the growing demand for private,
secure, and scalable AI inference in scientific workflows, allowing
researchers to generate billions of tokens daily on-premises without
relying on commercial cloud infrastructure.
CCS Concepts
• Computer systems organization →Real-time system architec-
ture; Client-server architectures.
Keywords
Inference as a Service, High Performance Computing, Job Sched-
ulers, Large Language Models, Globus, Scientific Computing
ACM Reference Format:
Aditya Tanikanti1, Benoit Côté1, Yanfei Guo1, Le Chen1, Nickolaus Saint2,
Ryan Chard1, Ken Raffenetti1, Rajeev Thakur1, Thomas Uram1, Ian Foster1,2,
Michael E. Papka1,3, Venkatram Vishwanath1. 2025. FIRST: Federated Infer-
ence Resource Scheduling Toolkit for Scientific AI Model Access. In . ACM,
New York, NY, USA, 9 pages. https://doi.org/10.1145/nnnnnnn.nnnnnnn
1
Introduction
AI models are valuable tools in scientific computing, assisting with
tasks such as analyzing large datasets [32] (e.g., genomic sequences,
climate patterns), interpreting complex simulations (e.g., parti-
cle collisions), generating synthetic data, and optimizing research
methodologies. Many scientists at laboratories and universities
Conference’17, Washington, DC, USA
© 2025 Copyright held by the owner/author(s). Publication rights licensed to ACM.
This is the author’s version of the work. It is posted here for your personal use.
Not for redistribution. The definitive Version of Record was published in , https:
//doi.org/10.1145/nnnnnnn.nnnnnnn.
depend on High-Performance Computing (HPC) clusters, yet de-
ploying scalable, open-source AI inference services on such systems
without commercial cloud support is challenging due to complex
authentication, scheduling, and resource management. This paper
introduces the Federated Inference Resource Scheduling Toolkit
(FIRST), which enables Inference-as-a-Service across distributed
HPC clusters. It provides cloud-like access to powerful AI models
while leveraging existing infrastructure and maintaining the secu-
rity and scalability requirements of scientific applications. Our goal
is not to compete with cloud providers, but rather to provide an
on-premises option to support AI on traditional HPC workloads.
With FIRST, AI models are accessed through an OpenAI-compliant
API that is gated with Globus Auth [34] to accommodate flexible
authorization policies. We leverage Globus Compute [8] to make
our system agnostic to the underlying cluster and scheduler (e.g.,
Kubernetes, bare metal, PBS, Slurm) and to enable the dispatch of re-
quests to different federated clusters based on customizable criteria
such as resource availability. On HPC clusters, compute nodes are
acquired dynamically and initialized based on user requests. This
approach allows FIRST to easily support multiple state-of-the-art
inference frameworks (e.g., vLLM [26], Infinity [19], SGLang [39])
and host arbitrary numbers of AI models of any size.
Once compute nodes are acquired, they are kept “warm” to pro-
vide a low-latency experience for any user or tool who needs to
interact with the hosted models in real time. This is vital for scien-
tific research and it embeds AI models into existing workflows. For
instance, in exploratory data analysis, scientists can use tools with
APIs to query genomic datasets on the fly and thus to obtain instant
insights into unusual patterns that may guide further investiga-
tions. Similarly, during simulation optimization, researchers can
interactively adjust parameters, such as those in climate models,
and get immediate feedback via integrated platforms. The compute
resources can automatically scale up or down to adapt to bursty
demands. Our system also provides a batch mode, in which users
can submit a large number of inference tasks with a single request,
as well as a web interface for interactive chat sessions.
Our work addresses a critical gap in the scientific computing
ecosystem, providing an accessible and efficient bridge between
traditional HPC infrastructure and the growing demand for AI. This
approach is an alternative to cloud providers and offers several key
advantages. HPC data centers often contain sensitive data that can-
not be shared with external providers, and custom models trained
on such data can be securely deployed to trusted users. Scientific
datasets can be very large (petabytes), and on-premises inference
services can naturally interface with such dataset without making
arXiv:2510.13724v1  [cs.DC]  15 Oct 2025


---

any cloud transfer. Since our framework is designed to be deployed
on existing HPC clusters, workloads can scale significantly and
become cost-effective for many scientists. Our proof-of-concept de-
ployment on a shared 24-node HPC cluster Sophia [1] processed 8.7
million inference requests, accommodated 76 users, and generated
over 10 billion tokens in the past 10 months.
2
Background and Related Work
Here we review recent progress in hosting inference services, and
discuss the challenges of deploying such services at HPC facilities.
2.1
Commercial Cloud Solutions
Major cloud providers now ship inference services that resemble
HPC workloads. Azure HPC AI supplies InfiniBand-connected VMs
with AI-tuned stacks [28]; AWS Batch for ML couples SageMaker’s
managed serving with elastic batch processing [4]; Google’s TPU
Service exposes high-bandwidth interconnects and TPU-pods for
low-latency inference [22]. These offerings demonstrate that cloud
infrastructures can meet HPC-scale performance, but they remain
external to institutional data-governance policies.
2.2
Inference Optimization Frameworks
A rich ecosystem of libraries accelerates LLM inference on modern
accelerators. NVIDIA’s TensorRT-LLM [30] provides kernel-fusion,
quantisation and dynamic-batching optimisations that can yield
roughly 4× the throughput of a vanilla PyTorch implementation.
HuggingFace’s Text Generation Inference (TGI) [23] is an open-source
server that supports continuous batching and efficient attention
mechanisms. The lightweight C++ engine llama.cpp [15] enables
8-bit quantised LLaMA models to run on commodity hardware.
vLLM [26] introduces PagedAttention together with continuous
batching to maximise GPU memory utilisation. SGLang [39], a
domain-specific language for LLM serving, adds RadixAttention and
other specialised optimisations, achieving up to 3.1× the through-
put of vLLM on selected models. Finally, Infinity [19] builds on
FlashAttention-2 [38] and aggressive memory-management strate-
gies to deliver high-throughput, low-latency serving. Together
these libraries constitute the core building blocks for modern in-
ference services, although they generally assume a traditional,
always-on server deployment model.
2.3
Inference Services at HPC Facilities
Integrating the above kernels into HPC centres is non-trivial. Pro-
duction HPC systems rely on batch schedulers (Slurm [25], PBS
[3], LSF [24]) that optimise resource allocation for long-running
jobs, yet they are ill-suited for the low-latency, interactive patterns
typical of inference [37]. Consequently, most existing solutions
require users to manually provision and configure servers, creating
a steep barrier for domain scientists.
Project Garden [2] supplies code for cross-DOI AI access but
lacks OpenAI-compatible APIs and does not exploit HPC-scale
resource management. Commercial batch services (AWS, Azure)
[4, 28] offer elasticity but operate outside the secure on-premises
environment required by many scientific workflows. Open-source
“llm-serving” frameworks [20] expose APIs on dedicated nodes yet
provide no dynamic model registration, scheduler integration, nor
institutional authentication.
Our contribution fills this gap: a service that couples cloud-style
APIs with native HPC schedulers, offering both interactive and
batch inference while respecting the security and policy constraints
of scientific facilities.
2.4
Scientific Computing with Globus
Globus [21] has established itself as a critical platform for scientific
workflows, providing authentication, transfer, and sharing capabili-
ties across institutional boundaries. Globus Compute [8], in partic-
ular, enables Function-as-a-Service capabilities across distributed
computing resources and can seamlessly interface with HPC clus-
ters and job schedulers. These advances provide a foundation for
building federated services that leverage authentication tools such
as Globus Auth [34] to conform with institutional policies.
3
FIRST Architecture
FIRST consists of three main components: 1) an Inference Gateway
API to process user requests, 2) Globus Compute to execute tasks on
HPC resources, and 3) Model Serving Tools to efficiently perform
the LLM inference. This layered architecture separates concerns and
enables flexible deployment across different types of environments.
The following subsections describe the role of each component.
3.1
Inference Gateway API
The Inference Gateway consists of an asynchronous Django-Ninja
[14] API application that connects to a PostgreSQL database and
communicates with Globus. The Gateway is deployed in a virtual
machine environment, separate from the HPC clusters where the
LLMs are hosted. The API application is served by Gunicorn [9] via
Uvicorn [11] workers, and can be deployed with systemctl services
or with containers. An Nginx [29] web server sits between users
and the application, which encrypts traffic with SSL certificates.
3.1.1
Role and Responsibilities. The Gateway is the main entry
point for users to interact with the system. The API is OpenAI-
compatible and supports the chat completions, completions, embed-
dings endpoints. It is also responsible for validating user identities,
validating incoming data, protecting services by rate limiting and
caching, converting user requests into Globus Compute requests
(see Section 3.2), and logging all user activities in the PostgreSQL
database. The metrics layer provides real-time monitoring of the
compute resources and queue status. Performance and summary
metrics are also exposed through a web dashboard.
3.1.2
Authentication and Authorization. The Gateway API
authorization layer was developed as a resource server on Globus
Auth, an OAuth2 and OpenID compliant identity and access man-
agement service. Globus Auth allows users to login from different
institutions across the world with multi-factor authentication. The
API uses Globus policies to control access to the platform and se-
cure the HPC resources. It also uses Globus Groups to implement
role-based access control for maintaining security in multi-user
HPC environments that may have varying levels of access. For
example, researchers working on sensitive projects may be granted
special access to specific models or computational resources.
2


---

The Gateway API secures requests via Globus Auth access tokens
obtained through an authorized identity provider. These tokens
are passed in the request headers and cached for rapid repeated
requests. This architecture allows the Gateway API to efficiently
utilize powerful API models at high request cadence, while main-
taining a secure environment.
3.2
Globus Compute
Globus Compute [8] is a Function-as-a-Service framework that can
remotely trigger compute tasks on any system where an endpoint
is deployed. In FIRST, it acts as the communication layer between
the Gateway API and the HPC resources. This service is hosted at
AWS, is fully maintained and supported by the Globus team, and
naturally integrates with Globus Auth to enhance security.
3.2.1
Role and Responsibilities. At the Gateway API level, the
Globus Compute SDK forwards each inference request to the Globus
Compute service in the cloud. The request carries (i) the pre-registered
inference function to be invoked and (ii) the identifier of the Com-
pute endpoint on which the function should run. Upon receipt,
the Globus service validates the request against the access-policy
defined in Section 3.2.3, dispatches the function to the chosen end-
point, and finally relays the inference result back to the Gateway
host for delivery to the user.
Within each HPC facility, FIRST administrators deploy a Globus
Compute endpoint on the target cluster(s). Each endpoint is config-
ured independently (Section 3.2.2) to host one or more LLMs, with
the specific models selected according to their size and the available
compute nodes. When a request arrives, the endpoint automatically
acquires the necessary resources—either on local nodes, inside a
Kubernetes pod, or through a batch-scheduler submission (e.g., PBS
or Slurm)—executes the inference, and returns the output to the
Globus service without human intervention.
Because the Gateway API is agnostic to the underlying execu-
tion environment, administrators retain full control over where
and how inference is performed. This flexibility enables FIRST to
support an arbitrary number of LLMs, employ diverse inference
frameworks, and distribute load across multiple federated HPC
clusters, all without requiring any changes to the Gateway API
code.
3.2.2
Endpoint Configuration and Safety Features. The flexi-
ble Globus Compute endpoint configuration can adapt to different
hardware environments, and is designed to ensure efficient resource
utilization, fault tolerance, and optimal performance. Key features
of such a configuration include:
Auto-scaling: The system automatically provisions additional
compute resources when demand exceeds capacity. Administrators
can set the maximum number of parallel inference tasks within a
node, and the maximum number of nodes an LLM can scale up to.
Fault Tolerance: Process management scripts monitor inference
server health and automatically restart failed instances.
Resource Utilization: Multiple models can be co-located on a
single node to promote efficient use of GPU resources. For example,
in a compute node with 8 GPUs, a 70B parameter model might use
6 GPUs, while 8B and 7B models use the remaining 2 GPUs.
Figure 1: High-level overview of the FIRST framework inte-
gration with ALCF infrastructure and Globus services.
Hot Node Management: Nodes remain active after completing
requests, allowing subsequent requests to be processed without
model reloading overhead. This strategy significantly reduces la-
tency for interactive workloads while still allowing resources to be
released after an extended idle period (currently 2 hours).
Security: Only functions that are pre-registered by the admin-
istrators are permitted to be executed on an endpoint, preventing
execution of malicious code.
3.2.3
Access and Permissions. The Globus Compute SDK and
endpoints can be authenticated with a common Globus confidential
client, which is owned by the system administrators. With the
client identification and secrets securely stored and hidden, general
users cannot communicate with the Globus Compute endpoints
directly. Instead, they must go via the Inference Gateway API and
all validation steps in order to interface with the running LLMs.
3.3
Model Serving Tools
The Serving Tools represent the LLM serving frameworks that
Globus Compute deploys upon initialization to serve models (see
Figure 1). Our service is currently configured for serving open-
source LLMs, and currently supports vLLM and Infinity, which
are optimized for NVIDIA A100, H100, and AMD MI250 GPUs.
However, other optimization frameworks can be deployed and
configured according to the HPC cluster hardware.
4
Deployment of the Service
We describe how we deployed the inference service on an HPC
system. Figure 1 shows the overall architecture, with the Gateway
API hosted on a virtual machine, and the LLMs served through
Globus Compute endpoints deployed on the Sophia cluster [17] at
the Argonne Leadership Computing Facility (ALCF).
FIRST provides a comprehensive set of OpenAI-compliant API
endpoints. This compatibility ensures that researchers can leverage
3


---

their existing codebase and tools without significant adaptation ef-
forts. The system also supports multiple inference frameworks (see
Section 4.1) and a wide range of models, from small language mod-
els to large vision-language models (see Section 4.2). We describe
the user interaction in Section 4.6, discuss wait times for interactive
requests in Section 4.3, and highlight the batch mode in Section 4.4.
Since its deployment on July 2024, this service has processed 8.7
million inference tasks, served 76 users in total, and generated over
10 billion tokens. About 4.1 million tasks were single interactive
requests. The other 4.6 million tasks were packaged into 49 batch
requests, which generated over 6.2 billion tokens.
4.1
Model Serving Framework Selection
Numerous benchmark studies, such as LLM-Inference-Bench[10],
compare the performance of various inference serving backends
across different hardware architectures. For the initial deployment
of FIRST, we selected vLLM [26] as the primary inference frame-
work. This decision was based on findings available at the time,
which indicated vLLM offered superior throughput and low latency,
largely attributed to its PagedAttention mechanism [36]. Further-
more, vLLM’s compatibility with a range of hardware, including
GPUs from NVIDIA, AMD, and potentially others, aligned with our
goal of a flexible HPC service.
While frameworks such as NVIDIA’s TensorRT-LLM [30] may of-
fer higher performance on specific NVIDIA hardware, a key design
principle of FIRST is its backend agnosticism. Our architecture can
readily integrate with any of the inference frameworks discussed in
Section 2.2 (e.g., TensorRT-LLM, TGI [23], SGLang [39]), provided
they expose an OpenAI-compatible API.
4.2
Available Models
FIRST exposes a curated set of models covering three functional
groups. For chat-language tasks we provide several families: Qwen2.5
(14B,7B,32B), Meta-Llama 3/3.1 (70B,405B), Mistral-7B-Instruct-v0.3
and Mixtral-8x22B-Instruct-v0.1, and the science-focused Auror-
aGPT suite [5] (7B,IT-v4-0125,Tulu3-SFT-0125).
Multimodal vision-language models are also available, namely
Qwen/Qwen2-VL-72B-Instruct and Meta-Llama-3.2-90B-Vision, en-
abling combined text-image interactions. For embedding generation
we ship NVIDIA’s NV-Embed-v2, which yields high-quality vec-
tor representations for downstream retrieval-augmented pipelines.
Adding a new model is straightforward: the model only needs to
be supported by one of the configured back-ends (e.g., vLLM, In-
finity), after which it can be registered via the service’s dashboard.
The current catalogue reflects a mix of user-driven demand and
usage-pattern insights gathered from our monitoring infrastruc-
ture.
4.3
Performance and Wait Times
FIRST balances responsiveness with HPC resource efficiency. When
a user requests a model that is already loaded onto GPUs (a “hot”
model), inference requests are processed with minimal latency, en-
abling real-time interaction. However, if the requested model is
not currently active (a “cold” start), FIRST initiates a process that
involves several steps typical of HPC environments. First, a job
is submitted to the cluster’s scheduler (e.g., PBS). This job may
spend time waiting in the queue until the necessary compute nodes
and GPUs are allocated. Once allocated, the primary task is load-
ing the model’s weights from storage into the GPU memory. The
time required for this loading process and the resulting memory
footprint are highly dependent on the model’s parameter count
as well as on parameters such as the chosen context size, applied
quantization techniques (if any), and expected sequence lengths.
For instance, an 8B parameter model (typically requiring around
16 GB of VRAM) generally has a smaller memory footprint and
loads relatively quickly onto fewer GPUs compared to a large 405B
parameter model (potentially needing 800 GB or more of VRAM),
which requires coordinating the loading process across multiple
nodes and GPUs, significantly increasing the cold start time.
To provide users transparency into model availability and poten-
tial wait times, FIRST offers a ‘/jobs’ API endpoint. This endpoint
queries the underlying scheduler and reports the status of models
managed by the service. Users can see which models are currently
“running” (hot and ready for immediate inference), “starting” (nodes
acquired, model loading in progress), or “queued” (waiting for re-
source allocation).
4.4
Batch Processing
For large-scale inference tasks, such as for generating compre-
hensive textual descriptions of genomic features , FIRST provides
a dedicated batch processing mode. This mode achieves higher
throughput compared to interactive requests by eliminating the
overhead associated with online model serving. Online serving
often involves requests passing through a shared server process,
which can face limitations (e.g., vLLM’s API server historically be-
ing single-threaded [7]) and resource contention with other users.
In contrast, FIRST’s batch mode executes each batch job as a
dedicated HPC job. This job loads the specified model solely for that
task, processing all requests from the user’s input file directly with-
out the mediation of a shared online server. Users submit batch jobs
via the ‘/v1/batches’ endpoint, providing an input file in JSON Lines
format where each line constitutes a complete inference request.
This approach allows for the efficient processing of thousands of
requests within a single, dedicated job allocation, maximizing re-
source utilization for the task. The batch system also provides status
updates for monitoring long-running jobs.
4.5
Federation Implementation
The federation layer is a key advantage of our architecture, enabling
seamless access to resources across distributed clusters. As a proof
of concept, we deployed additional endpoints on the ALCF Polaris
System [18] - another HPC system at ALCF. We then implemented
a development API URL that does not target any specific cluster and
is agnostic. Once a request is received, our API queries the database
to see which clusters can host the inference (Sophia or Polaris in our
case), queries the publicly available status of each cluster, decides
which cluster to use based on node availability, and submits the
request to the automatically selected cluster. The configuration of
this functionality needs to be adapted for deployments at other
facilities, since it relies on how the status of compute resources is
queried internally within each facility.
4


---

The core logic for selecting an endpoint in this federated proof-
of-concept follows a simplified priority-based algorithm. It first
attempts to find an endpoint where the requested model is already
running or queued. If none exists, it searches for an endpoint asso-
ciated with a cluster that has available nodes. If neither condition
is met, it defaults to the first endpoint configured for that model,
where priority is determined simply by the order in which end-
points are listed in the configuration registry. This ensures that
requests are preferentially sent to active instances for low latency.
4.6
User Interaction
Users interact with the system through a streamlined authentication
and API workflow designed to abstract complexity while maintain-
ing security (see Figure 1). The process begins with authentication
through Globus Auth, which integrates with institutional identity
providers, ensuring that only authorized users can access the sys-
tem. Access tokens are valid for 48 hours and can be automatically
refreshed to reduce the need for frequent re-authentications. Once
authenticated, users can make API requests using standard HTTP
clients or the OpenAI Python package. This allows researchers to
leverage familiar tools and libraries, reducing the learning curve
associated with adopting new systems. We provide an easy-to-use
Python script to guide users through the authentication, as well as
numerous examples on how to interact with our system.
4.7
Web Interface for Interactive Chat
We have developed a web interface that enables users to engage
in interactive chat sessions with models running on the backend
(see Figure 2). The interface is based on Open WebUI [31], cus-
tomized to integrate with Globus Auth for secure access, and is
supported by a backend stack consisting of FastAPI served by Uvi-
corn workers behind an Nginx reverse proxy, with a PostgreSQL
database used to persist user sessions and request metadata. All
user requests, along with the access tokens generated through the
authentication process, are forwarded to our Gateway API. Through
the interface, users can select and view which models are currently
running from a dropdown menu, maintain chat histories, compare
responses from different LLMs in a multi-column layout, and adjust
OpenAI-compliant parameters. The interface also supports stream-
ing responses and rich formatting for equations, code, and tables,
making the user experience both functional and visually engaging.
5
Performance Evaluation
We conducted benchmarks to highlight the concurrency levels
achievable with the FIRST framework. Our goal here is not to
compete with commercial cloud providers, but rather to measure
its performance when deployed on limited HPC resources.
5.1
Performance Metrics
We focus on the following key metrics [33] to evaluate performance:
• Request throughput (req/s): The number of successful infer-
ence requests completed per second. This metric measures the
overall request processing capacity of the system.
• Output Token Throughput (tok/s): The total number of output
tokens generated by the model per second across all successful
requests. This metric reflects the effective generation speed.
Figure 2: Inference service user chat interface.
• Median End-to-End Latency (s): The time elapsed from send-
ing a request to receiving the complete response. We primarily
report the median latency, as it is less sensitive to outliers than
the mean. This metric reflects the user-perceived responsiveness
for a single request.
• Benchmark Duration (s): The total time taken to complete
all requests in a given benchmark run. This provides an overall
measure of how quickly the entire workload was processed.
5.2
Experimental Setup
5.2.1
Hardware. All benchmarks were conducted on Sophia at
ALCF [17]. Sophia comprises 24 NVIDIA DGX A100 nodes, each
equipped with eight NVIDIA A100 Tensor Core GPUs (mostly 40 GB,
with two nodes offering 80 GB GPUs for a total of 8320 GB VRAM
across the system) and two AMD Rome CPUs. The nodes feature
fast local storage (15 TB SSD per node) and are interconnected with
a Mellanox HDR Infiniband fabric in a fat-tree topology. Specific
model configurations using Tensor Parallelism (TP) were 4 GPUs,
TP=4 for Llama 3.1 8B and 8 GPUs, TP=8 for Llama 3.3 70B.
5.2.2
Software. The following software setup was used.
• Inference Server: vLLM (version 0.8.2) was used for the infer-
ence engine for both Direct and FIRST benchmarks.
• FIRST Deployment: The FIRST API Gateway was deployed
using Gunicorn with Uvicorn workers, configured to handle
asynchronous tasks. The specific Gunicorn configuration utilized
multiprocessing.cpu_count() × 2 + 1 workers, 4 threads
per worker, and a backlog of 2048.
• Benchmarking Tool: We adapted the benchmark script pro-
vided by the vLLM project [35] to measure performance. This
script utilizes the ShareGPT dataset [13], which contains thou-
sands of real-world user-AI conversations across diverse topics.
Using ShareGPT ensures our benchmarks reflect realistic us-
age patterns and query types, providing a robust evaluation of
throughput and latency metrics.
• Workload: The benchmarks used the ShareGPT dataset, sending
1000 requests. For peak performance tests, an “infinite” request
rate was used, meaning all requests are sent as quickly as possible
at the beginning of the benchmark to fully saturate the server.
Using an infinite request rate effectively emulates the usage pat-
terns of popular open models where numerous users might send
requests concurrently. To ensure fair comparison between sce-
narios (FIRST vs. Direct, different request rates), the same set
of input prompts and corresponding target output lengths were
used for each model across all relevant tests.
5


---

Figure 3: Performance vs. request rate for FIRST vs. vLLM Direct for Llama 3.3 70B on a single Sophia node with 8 A100 GPUs,
showing request throughput, output token throughput, median end-to-end latency, and duration vs. offered request rate.
• OpenAI Models: We ran benchmarks against the OpenAI API
for GPT-4o-mini for comparison, using the same benchmarking
tool and workload where applicable (subject to API rate limits).
5.2.3
Benchmark Scenarios. For all scenarios involving vLLM
(Direct and FIRST), identical vLLM serving parameters were used
for a given model to ensure consistency.
• vLLM Direct: Requests were sent directly from the benchmark-
ing client to the vLLM OpenAI-compatible API endpoint running
on the designated Sophia nodes.
• FIRST: Requests were sent to the FIRST API Gateway, which then
authenticated the request, routed it to the appropriate Globus
Compute endpoint on Sophia, executed the inference function,
and returned the result.
• OpenAI API: Requests were sent to the official OpenAI API
endpoints for GPT-4o-mini.
5.3
Results and Discussion
5.3.1
Performance vs. Request Rate (Llama 3.3 70B Model).
To understand how FIRST behaves under varying load, we bench-
marked the Llama 3.3 70B model at different request rates (1, 5, 10,
20 req/s, and infinite/maximum rate) and this is depicted in Figure 3.
At lower request rates (1 and 5 req/s), the additional network
hop and processing within FIRST introduces observable latency
compared to direct vLLM access (e.g., median latency of 9.2s vs.
3.0s at 1 req/s). However, as the request rate increases (10 req/s and
beyond), FIRST demonstrates superior scalability. At 20 req/s and
the infinite rate, FIRST achieves significantly higher request and
output token throughput (e.g., 9.2 vs. 5.8 req/s and 1677 vs. 1054
tok/s). On a related note, FIRST’s median latency at these high rates
becomes lower than direct access (46.9s vs. 80.2s at infinite rate).
This scaling advantage stems from FIRST’s architecture. The
asynchronous API gateway (using Gunicorn/Uvicorn) effectively
buffers and manages high volumes of incoming user requests, pre-
venting the backend inference engine from being overwhelmed.
This contrasts with directly exposing the vLLM server, which his-
torically faced limitations in its own API handling capacity (e.g.,
single-threaded processing [7]). This demonstrates that our gate-
way architecture can provide superior performance under high load
conditions, validating our approach for production HPC environ-
ments. FIRST provides this scaling capability transparently to users,
without requiring changes to their client-side scripts. We imple-
mented three sets of improvements to reach this level of efficiency:
Optimization 1: We initially determined request status by polling
every 2 s, returning results to users once tasks were labeled as
completed. We optimized this by using concurrent Python future
objects to retrieve results as soon as possible, which eliminated the
polling overhead for each request.
Optimization 2: In our early design, each request queried the
Globus servers to introspect the user token and to create new con-
nections with the compute endpoints. To improve efficiency, these
repetitive steps are now cached for frequently incoming requests.
This eliminated 2 s from the latency of each request and prevented
our framework from being rate-limited by the Globus services.
Optimization 3: A major improvement was the migration from
synchronous Django REST [12] to asynchronous Django Ninja [14].
Originally, only nine requests could be processed at a time, and
the API’s compute resources were idle while waiting for results.
Now, the API can continuously offload requests to the HPC cluster.
Benchmarks with Artillery [6] (100 incoming requests per second
for 300 seconds) showed that over 8000 inference tasks could be
queued at Globus, successfully removing our API as a bottleneck.
The benchmark also showed that the response throughput rates
could be increased by a factor of 20 on a single compute node.
Figure 4: Auto-scaling performance: Single vs. two, three,
and four instances for Llama 3.3 70B on Sophia’s A100 GPUs
under maximum load.
6


---

Summary (FIRST vs. vLLM Direct): FIRST introduces min-
imal overhead, outperforming direct vLLM access in peak tests.
Interpreting this alongside the rate comparison (Figure 3), FIRST’s
gateway architecture is better suited for handling sustained high
request volumes typical of scientific workloads, offering superior
scalability and robustness compared to direct backend access.
FIRST also offers a batch mode optimized for high-throughput,
non-interactive tasks. In this mode, requests are processed using
vLLM’s offline batch mode without serving within a dedicated job
allocation. For instance, a batch job processing 1000 requests using
the Llama 3.3 70B model achieved an overall output throughput
of 2117 tokens/second, completing in 409 seconds. Because batch
jobs typically involve a cold start (loading the targeted model), the
initial model loading time can dominate the total execution time for
smaller batches. However, for larger workloads (>10,000 requests),
the amortization of the loading cost across many requests makes
batch mode highly efficient for large-scale data generation.
5.3.2
Auto-Scaling Performance (Llama 3.3 70B Model). Auto-
scaling in FIRST refers to the capability, configured via Globus Com-
pute endpoint settings (Section 3.2.2), to automatically launch addi-
tional instances (jobs) of a model on separate compute resources
when the existing instance(s) become saturated. This capability
allows the system to adapt dynamically to increased request loads.
To evaluate FIRST’s ability to scale out, we benchmarked the Llama
3.3 70B model using FIRST first on a single instance and then on
configurations that automatically launched additional instances (up
to four total) when demand saturated existing ones. All scenarios
used an infinite request rate with the ShareGPT dataset.
The results, presented in Figure 4, show that scaling from one
to multiple instances significantly increased the overall processing
capacity. Request throughput scaled from 8.3 req/s (1 instance) to
14.6 req/s (2 instances), 20.9 req/s (3 instances), and 23.9 req/s (4
instances). Similarly, output token throughput increased substan-
tially from 1432 (1 instance) to 4131 tok/s (4 instances). In terms
of the output token throughput, we observe an increase to 1.75×
as we scale to two instance, to 2.52× at three instances, and 2.88×
at four instances. Our overall scaling is currently limited by the
ability of Globus Compute to scale and route requests to the multi-
ple instances. Overall, the scaling demonstrates FIRST’s capability
to effectively leverage additional resources provisioned via Globus
Compute’s auto-scaling features by distributing the incoming re-
quest load across simultaneously running model instances.
Scaling also substantially decreased the median end-to-end la-
tency from 54.5s (1 instance) down to 30.1s (2 instances), 18.8s (3
instances), and finally 16.0s (4 instances) by distributing the load
and reducing queueing within each instance. These results validate
that the FIRST architecture, combined with Globus Compute’s re-
source management, can effectively auto-scale to handle increased
inference demand, improving both throughput and responsiveness.
5.3.3
FIRST vs. OpenAI API. To provide context against industry-
standard cloud offerings, we compared FIRST (running the open-
source Llama 3.1 8B model) with the OpenAI API serving GPT-4o-
mini (estimated to be in a similar parameter class). This comparison
serves to contextualize FIRST’s performance within the broader
inference landscape, not to position it as a direct competitor to
Figure 5: Benchmark comparison of FIRST (Llama 3.1 8B) vs.
OpenAI API (the ∼8B-parameter GPT-4o-mini). Note: OpenAI
API results may be influenced by service-side rate limiting.
commercial services. The results are shown in Figure 5. In this com-
parison, FIRST significantly outperforms the OpenAI API in request
throughput (25.1 vs. 6.7 req/s) and output token throughput (3283
vs. 1199 tok/s). Conversely, the OpenAI API offers lower median
latency (2.0s vs. 16.3s for FIRST). This highlights a common trade-
off: while commercial cloud APIs such as OpenAI may provide low
latency, likely due to significant scale and optimization beyond
our visibility, FIRST deployed on dedicated HPC resources demon-
strates the capacity to handle a much larger volume of concurrent
requests, achieving substantially higher overall throughput. This
makes FIRST particularly suitable for scientific workloads involving
large numbers of concurrent users or high-volume batch process-
ing tasks, where maximizing throughput on available, secure HPC
resources is paramount. The results underscore the value of special-
ized, self-hosted inference solutions within the HPC ecosystem. It
is important to note that benchmarks against the OpenAI API are
subject to service-side rate limits and potentially variable resource
allocation, which may influence the observed performance.
5.3.4
Concurrency and Throughput Analysis for WebUI. We
measured the performance of our WebUI deployment under in-
creasing concurrency, defined as the number of simultaneous in-
ference requests being processed. Measurements include request
throughput (the number of successful requests completed per sec-
ond) and output token throughput (the total number of output
tokens generated per second across all successful requests). Bench-
marks were performed using simulated concurrent WebUI sessions
targeting three models: Llama-3.1-8B, Gemma-27B, and Llama-3.3-
70B. As shown in Table 1, both token and request throughput scale
nearly linearly from 50 to 500 concurrent sessions, with diminish-
ing returns beyond this point as the backend approaches saturation.
Shorter runs (60 sec) consistently yielded higher throughput than
longer runs (120 sec), likely due to reduced resource contention
and the avoidance of long-tail latency effects. Overall, the results
demonstrate that our web interface can sustain high concurrency
without becoming a performance bottleneck.
7


---

Table 1: WebUI benchmark results per model. Conc = concur-
rency; TP = throughput tokens; Req = requests.
Model
Conc.
60 s
120 s
TP/s
Req./s
TP/s
Req./s
Llama-3.1-8B
50
690.68
4.97
441.17
3.12
100
738.33
5.25
563.18
4.01
300
1103.70
7.90
981.45
6.81
500
1672.15
12.08
1271.04
8.94
700
2119.50
14.68
1385.93
9.74
Gemma-27B
50
297.97
2.70
864.83
5.13
100
906.62
5.42
865.05
5.10
300
1469.53
8.67
1211.75
7.25
500
1849.67
10.95
1144.79
6.83
700
2651.40
15.57
1353.15
8.17
Llama-3.3-70B
50
217.38
1.63
472.05
3.57
100
785.83
5.88
503.52
3.86
300
1061.93
7.92
948.13
7.13
500
1646.53
12.30
1176.39
8.75
700
2134.10
15.67
1372.27
10.35
6
Case Studies
We present case studies in which researchers leverage FIRST capa-
bilities to support diverse scientific applications.
6.1
Model Evaluation and Comparison
Using the Inference Gateway, researchers benchmarked fifteen
GPT-style models—including AuroraGPT [5] fine-tuned on HPC
documentation and a suite of open-source LLMs—against custom
models trained on proprietary scientific corpora. Over 50,000 infer-
ence requests generated more than one million tokens, allowing the
team to assess accuracy on domain-specific terminology, compare
architectures of varying size, and quantify the trade-offs between
computational cost and performance. The gateway’s ability to swap
models instantly eliminated manual deployment steps, yielding
a 40 percent reduction in total evaluation time while preserving
consistent throughput across all model variants.
6.2
HPC Assistant with RAG
To provide on-demand support for a high-performance computing
environment, a chatbot was constructed that combines FIRST’s
embedding and inference services. NVIDIA’s NV-Embed-v2 pro-
duced dense vector representations of HPC manuals, guides, and
troubleshooting documents, which were stored in a FAISS index
[16] for rapid similarity search. When a user poses a question, a
Retrieval-Augmented Generation (RAG) pipeline [27] retrieves the
most relevant passages and incorporates them into the prompt sent
to the LLM. This architecture enables the system to disambiguate
user intent, surface highly relevant guidance, and dramatically im-
prove the quality of HPC-specific assistance.
6.3
Synthetic Data Generation for Training
Researchers leveraged the Inference Gateway to generate synthetic
data for training machine learning models, showcasing the system’s
capabilities in supporting large-scale data generation workflows.
The batch mode was particularly valuable for this use case, allowing
researchers to generate billions of tokens of synthetic data with
minimal overhead. The system processed a batch job of 10,000 re-
quests, generating over 1 million tokens of synthetic data. The batch
mode eliminated the per-request overhead, achieving a throughput
of 25,000 tokens per second per model. The total job completed in
under 24 hours, compared to an estimated 2 days using a manual
deployment approach. This significant time savings enabled the re-
searchers to iterate more quickly on their data generation strategies
and produce larger volumes of training data.
7
Conclusions and Future Work
We introduced FIRST, an Inference-as-a-Service framework that
securely hosts and scales AI inference workloads on HPC systems
using Globus for authentication, communication, and scheduler
integration. FIRST is flexible, provides both API and web-chat ac-
cess to AI models, including LLMs and domain specific models,
supports multiple inference frameworks, optimizes performance
and latency, and offers greater data control compared to commer-
cial cloud solutions. Our work represents a significant step toward
democratizing AI access, which enables researchers without deep
HPC expertise to leverage advanced inference capabilities while
preserving institutional security and resource policies.
In future work we aim to expand FIRST’s capabilities to enable
direct job submission for users, allowing AI Models to execute
custom codes as tool calls and run traditional HPC simulations
through the same API interface. We will reduce barriers for new
users by providing a unified interface for both AI inference and
traditional HPC workloads, further demonstrating how FIRST can
coexist with and enhance, rather than replace, existing HPC infras-
tructure. We plan to expand our backend API to support scientific
foundation models beyond LLMs, including vision models, protein
folding models, and other domain-specific AI models commonly
used in scientific research. We also plan to improve scheduling
for resource optimization, enhance monitoring for deeper insights,
and strengthen security to meet evolving threats and compliance
requirements.
Acknowledgments
This research used resources of the Argonne Leadership Comput-
ing Facility, a U.S. Department of Energy (DOE) Office of Science
user facility at Argonne National Laboratory and is based on re-
search supported by the U.S. DOE Office of Science-Advanced Sci-
entific Computing Research Program, under Contract No. DE-AC02-
06CH11357.
References
[1] [n. d.]. ALCF Sophia. https://www.alcf.anl.gov/sophia
[2] 2025. The Gardens.ai Platform. https://thegardens.ai/. Accessed: 2025-01-10.
[3] Altair. [n. d.]. PBS Professional Scheduler. https://altair.com/pbs-professional/
[4] Amazon Web Services. 2024. AWS Batch for Machine Learning. https://aws.
amazon.com/batch/.
[5] Argonne National Laboratory. [n. d.]. AuroraGPT. https://auroragpt.anl.gov/.
Accessed: April 10, 2025.
[6] Artillery Project. 2025. Artillery - Modern load testing and smoke testing for
SRE and DevOps. https://www.artillery.io. Accessed: 2025-04-14.
8


---

[7] Russell Bryant et al. 2025. [RFC]: Scale the API server across multiple CPUs
#12705. https://github.com/vllm-project/vllm/issues/12705. GitHub Issue.
[8] Ryan Chard, Yadu Babuji, Zhuozhao Li, Tyler Skluzacek, Anna Woodard, Ben
Blaiszik, Ian Foster, and Kyle Chard. 2020. funcX: A Federated Function Serving
Fabric for Science. In Proceedings of the 29th International Symposium on High-
Performance Parallel and Distributed Computing (Stockholm, Sweden) (HPDC ’20).
Association for Computing Machinery, New York, NY, USA, 65–76. doi:10.1145/
3369583.3392683
[9] Benoît Chesneau, Paul J. Davis, et al. [n. d.]. Gunicorn: Python WSGI HTTP
Server for UNIX. https://gunicorn.org.
[10] Krishna Teja Chitty-Venkata, Siddhisanket Raskar, Bharat Kale, Farah Ferdaus,
Aditya Tanikanti, Ken Raffenetti, Valerie Taylor, Murali Emani, and Venkatram
Vishwanath. 2024. LLM-Inference-Bench: Inference Benchmarking of Large
Language Models on AI Accelerators. In Proceedings of the SC ’24 Workshops of
the International Conference on High Performance Computing, Network, Storage,
and Analysis (Atlanta, GA, USA) (SC-W ’24). IEEE Press, 1362–1379. doi:10.1109/
SCW63240.2024.00178
[11] Tom Christie et al. [n. d.]. Uvicorn: ASGI web server implementation for Python.
https://www.uvicorn.org/.
[12] Tom Christie and contributors. 2025. Django REST framework. https://www.
django-rest-framework.org/. Version 3.14.0.
[13] ShareGPT Contributors. 2023. ShareGPT: Crowdsourced Conversations for
Instruction Tuning. https://huggingface.co/datasets/anon8231489123/ShareGPT_
Vicuna_unfiltered. Accessed: 2025-04-11.
[14] Joshua M. L. Davis. 2021. Django Ninja. https://django-ninja.dev/
[15] Tim Dettmers et al. 2022. LLM.int8(): 8-bit Matrix Multiplication for Transformers
at Scale. arXiv preprint arXiv:2208.07339 (2022).
[16] Matthijs Douze, Alexandr Guzhva, Chengqi Deng, Jeff Johnson, Gergely Szilvasy,
Pierre-Emmanuel Mazaré, Maria Lomeli, Lucas Hosseini, and Hervé Jégou. 2025.
The Faiss library. arXiv:2401.08281 [cs.LG] https://arxiv.org/abs/2401.08281
[17] Argonne Leadership Computing Facility. [n. d.]. Sophia Documentation. https:
//www.alcf.anl.gov/sophia
[18] Argonne Leadership Computing Facility. 2024. Polaris System Documentation.
https://www.alcf.anl.gov/polaris
[19] Michael Feil. 2023. Infinity - To Embeddings and Beyond. doi:10.5281/zenodo.
11630143
[20] Center for AI-Innovation. [n. d.]. llm-serving. https://github.com/Center-for-
AI-Innovation/llm-serving
[21] Ian Foster. 2011. Globus Online: Accelerating and Democratizing Science through
Cloud-Based Services. IEEE Internet Computing 15, 3 (2011), 70–73. doi:10.1109/
MIC.2011.64
[22] Google Cloud. 2024. Cloud TPU. https://cloud.google.com/tpu.
[23] Hugging Face. 2024. Text Generation Inference. https://github.com/huggingface/
text-generation-inference.
[24] IBM. [n. d.]. LSF Scheduler. https://www.ibm.com/docs/en/spectrum-lsf/10.1.0/
[25] Morris A. Jette and Tim Wickberg. 2023. Architecture of the Slurm Workload
Manager. In Job Scheduling Strategies for Parallel Processing, Dalibor Klusáček,
Julita Corbalán, and Gonzalo P. Rodrigo (Eds.). Springer Nature Switzerland,
Cham, 3–23.
[26] Woosuk Kwon, Zhuohan Li, Siyuan Zhuang, Ying Sheng, Lianmin Zheng,
Cody Hao Yu, Joseph E. Gonzalez, Hao Zhang, and Ion Stoica. 2023. Efficient Mem-
ory Management for Large Language Model Serving with PagedAttention. In
Proceedings of the ACM SIGOPS 29th Symposium on Operating Systems Principles.
[27] Patrick Lewis, Ethan Perez, Aleksandra Piktus, Fabio Petroni, Vladimir Karpukhin,
Naman Goyal, Heinrich Küttler, Mike Lewis, Wen-tau Yih, Tim Rocktäschel,
Sebastian Riedel, and Douwe Kiela. 2020. Retrieval-augmented generation for
knowledge-intensive NLP tasks. In Proceedings of the 34th International Conference
on Neural Information Processing Systems (Vancouver, BC, Canada) (NIPS ’20).
Curran Associates Inc., Red Hook, NY, USA, Article 793, 16 pages.
[28] Microsoft. 2024. Azure HPC + AI. https://azure.microsoft.com/en-us/solutions/
high-performance-computing/.
[29] Inc. NGINX. 2004. NGINX: High Performance Web Server and Reverse Proxy.
https://nginx.org. Accessed April 10, 2025.
[30] NVIDIA. 2024. TensorRT-LLM. https://github.com/NVIDIA/TensorRT-LLM.
[31] Open WebUI. 2025. Open WebUI. https://openwebui.com/. Accessed: 2025-08-11.
[32] Michael H. Prince, Henry Chan, Aikaterini Vriza, Tao Zhou, Varuni K. Sastry,
Yanqi Luo, Matthew T. Dearing, Ross J. Harder, Rama K. Vasudevan, and Mathew J.
Cherukara. 2024. Opportunities for retrieval and tool augmented large language
models in scientific facilities. npj Computational Materials 10, 1 (2024), 251.
doi:10.1038/s41524-024-01423-2
[33] Sriram Sagi. 2025. Optimizing LLM Inference: Metrics that Matter for Real Time
Applications. Journal of Artificial Intelligence & Cloud Computing 4, 1 (2025), 1–4.
doi:10.47363/JAICC/2025(4)446
[34] Steven Tuecke, Rachana Ananthakrishnan, Kyle Chard, Mattias Lidman, Brendan
McCollam, Stephen Rosen, and Ian Foster. 2016. Globus Auth: A research identity
and access management platform. In 2016 IEEE 12th International Conference on
e-Science (e-Science). 203–212. doi:10.1109/eScience.2016.7870901
[35] vLLM Project Contributors. 2025. vLLM Benchmark Serving Script. https://github.
com/vllm-project/vllm/blob/main/benchmarks/benchmark_serving.py.
Ac-
cessed: April 10, 2025.
[36] Moritz Wallawitsch. 2024.
Introduction to vLLM and PagedAtten-
tion.
https://blog.runpod.io/introduction-to-vllm-and-how-to-run-vllm-on-
runpod-serverless/. RunPod Blog.
[37] Cheng Wu, Vinamra Jindal, Rohan Dathathri, Gregory Diamos, Shengen Xie,
Jeffrey Dean, Yacine Jernite, Julian Togelius, Haidar Patel, Gabriel Ilharco, Ken
Lebowitz, and Alice Wang. 2023. LLM in HPC: Efficient Large Language Model
Training and Serving in HPC Systems. In 2023 IEEE/ACM Workshop on Machine
Learning in High Performance Computing Environments (MLHPC). 46–56. doi:10.
1145/3581784.3607067
[38] Tri Dao Zhang et al. 2023. FlashAttention-2: Faster Attention with Better Par-
allelism and Work Partitioning. In Advances in Neural Information Processing
Systems.
[39] Lianmin Zheng, Liangsheng Yin, Zhiqiang Xie, Chuyue Sun, Jeff Huang,
Cody Hao Yu, Shiyi Cao, Christos Kozyrakis, Ion Stoica, Joseph E. Gonzalez, Clark
Barrett, and Ying Sheng. 2024. SGLang: Efficient Execution of Structured Lan-
guage Model Programs. arXiv:2312.07104 [cs.AI] https://arxiv.org/abs/2312.07104
9
